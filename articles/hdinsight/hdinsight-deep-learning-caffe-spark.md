---
title: "Použít Caffe v Azure HDInsight Spark pro distribuované hloubkové learning | Microsoft Docs"
description: "Použití Caffe v Azure HDInsight Spark pro distribuované hloubkové learning"
services: hdinsight
documentationcenter: 
author: xiaoyongzhu
manager: asadk
editor: cgronlun
tags: azure-portal
ms.assetid: 71dcd1ad-4cad-47ad-8a9d-dcb7fa3c2ff9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: xiaoyzhu
ms.openlocfilehash: 14b7808c9534bce3049422d6bce1e8914b2c2fbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a>Použití Caffe v Azure HDInsight Spark pro distribuované hloubkové learning


## <a name="introduction"></a>Úvod

Hloubkové learning je ovlivňující všechno zdravotní péče k Transport do výroby a další. Společnosti jsou vypnutí a učení pevný problémy, jako je třeba vyřešit [bitové kopie klasifikace](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [rozpoznávání řeči](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html)objektu rozpoznávání a počítač překlad. 

Existují [mnoha oblíbených rozhraní](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), včetně [kognitivní nástrojů Microsoft](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, atd. Caffe je nejslavnější rozhraní-symbolický (imperativní) neuronové sítě a široce používaných v mnoha oblastech, včetně vize počítače. Kromě toho [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinuje Caffe s Apache Spark, v takovém případě hloubky učení lze snadno použít na existující Hadoop clusteru spolu s Spark ETL kanály, snižuje složitost systému a latence pro učení začátku do konce.

[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) je zajištěna nabídku pouze plně spravované cloudové Hadoop, které poskytuje optimalizovanou s otevřeným zdrojem analytické clustery Spark, Hive, MapReduce, HBase, Storm, Kafka a R Server SLA 99,9 %. Každou z těchto technologií pro velké objemy dat, stejně jako aplikace nezávislých výrobců softwaru, je možné jednoduše nasadit jako spravovaný cluster se zabezpečením a monitorováním na podnikové úrovni.

Někteří uživatelé žádáme, nám o tom, jak používat hloubkové učení v HDInsight, což je PaaS Hadoop produktu společnosti Microsoft. Jsme bude mít více sdílení v budoucnu, ale dnes chceme shrnout technické blog o tom, jak používat Caffe na HDInsight Spark.

Pokud jste nainstalovali Caffe před, si všimnete, že instalace toto rozhraní je chvíli náročné. V tomto blogu jsme se nejprve ukazují, jak nainstalovat [Caffe na Spark](https://github.com/yahoo/CaffeOnSpark) pro cluster služby HDInsight, pak použít předdefinované MNIST ukázku k demostrate použití distribuovaných hloubkové Learning pomocí HDInsight Spark v procesorech.

Existují čtyři hlavní kroky se dá stáhnout fungovat v HDInsight.

1. Nainstalujte požadované závislosti pro všechny uzly v
2. Sestavení Caffe na Spark pro HDInsight z hlavního uzlu
3. Distribuovat požadované knihovny pro všechny uzly pracovního procesu
4. Napište Caffe modelu a potom ho spusťte distributely

Vzhledem k tomu, že HDInsight je řešení PaaS, proto je poměrně snadné k provedení některých úkolů nabízí skvělé funkce. Jedna z funkcí, které výraznou používáme v tomto příspěvku na blogu nazývá [akce skriptu](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), pomocí kterého můžete spustit příkazy prostředí pro přizpůsobení uzly clusteru (hlavního uzlu, pracovního uzlu nebo hraniční uzel).

## <a name="step-1--install-the-required-dependencies-on-all-the-nodes"></a>Krok 1: Instalace požadované závislosti pro všechny uzly v

Abyste mohli začít, potřebujeme závislosti, které je třeba nainstalovat. Webu Caffe a [CaffeOnSpark lokality](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) nabízí některé velmi užitečné wiki pro instalaci závislosti pro Spark na YARN režimu (což je režim pro HDInsight Spark), ale je potřeba přidat pár Další závislosti pro platformu HDInsight. Jsme se použití akce skriptu jak je uvedeno níže a spusťte ho na všechny head uzlů a uzlů pracovního procesu. Tato akce skriptu bude trvat asi 20 minut, jak tyto závislosti také závisí na jiné balíčky. Byste měli umístit v některém umístění, ke kterému mají přístup ke svému clusteru HDInsight, jako je například umístění GitHub nebo výchozí účet úložiště objektů BLOB.

    #!/bin/bash
    #Please be aware that installing the below will add additional 20 mins to cluster creation because of the dependencies
    #installing all dependencies, including the ones mentioned in http://caffe.berkeleyvision.org/install_apt.html, as well a few packages that are not included in HDInsight, such as gflags, glog, lmdb, numpy
    #It seems numpy will only needed during compilation time, but for safety purpose we install them on all the nodes

    sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler maven libatlas-base-dev libgflags-dev libgoogle-glog-dev liblmdb-dev build-essential  libboost-all-dev python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose

    #install protobuf
    wget https://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz
    sudo tar xzvf protobuf-2.5.0.tar.gz -C /tmp/
    cd /tmp/protobuf-2.5.0/
    sudo ./configure
    sudo make
    sudo make check
    sudo make install
    sudo ldconfig
    echo "protobuf installation done"


Existují dva kroky ve výše uvedené akce skriptu. Prvním krokem je instalace potřebných knihoven. Tyto knihovny obsahovat potřebné knihovny pro kompilaci Caffe (například gflags, glog) i s Caffe (například numpy). Libatlas se používá pro optimalizaci procesoru, ale je můžete provést na wikiwebu CaffeOnSpark o instalaci další optimalizace knihovny, například MKL nebo CUDA (pro GPU).

Druhým krokem je chcete stáhnout, kompilace a nainstalovat protobuf 2.5.0 pro Caffe za běhu. Protobuf 2.5.0 [je vyžadován](https://github.com/yahoo/CaffeOnSpark/issues/87), ale tato verze není k dispozici jako balíček na Ubuntu 16, takže potřebujeme zkompilovat ze zdrojového kódu. Existují také několik prostředkům na Internetu o tom, jak kompilovat, jako například [to](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)

Jednoduše začít můžete jenom spustit tuto akci skriptu pro váš cluster pro všechny uzly pracovního procesu a hlavních uzlech (pro HDInsight 3.5). Můžete buď spustit akcí skriptů pro cluster s podporou spuštěné, nebo akce skriptu můžete také spustit během doby zřízení clusteru. Další informace o akcí skriptů naleznete v dokumentaci [sem](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)

![Skript akce pro instalaci závislosti](./media/hdinsight-deep-learning-caffe-spark/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-the-head-node"></a>Krok 2: Vytvoření Caffe na Spark pro HDInsight z hlavního uzlu

Druhým krokem je vytvoření Caffe ve headnode, a pak distribuovat zkompilované knihovny pro všechny uzly pracovního procesu. V tomto kroku budete muset [ssh do vaší headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), jednoduše postupujte [proces sestavení CaffeOnSpark](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), a dále je skript, můžete použít k vytvoření CaffeOnSpark s pár dalších kroků. 

    #!/bin/bash
    git clone https://github.com/yahoo/CaffeOnSpark.git --recursive
    export CAFFE_ON_SPARK=$(pwd)/CaffeOnSpark

    pushd ${CAFFE_ON_SPARK}/caffe-public/
    cp Makefile.config.example Makefile.config
    echo "INCLUDE_DIRS += ${JAVA_HOME}/include" >> Makefile.config
    #Below configurations might need to be updated based on actual cases. For example, if you are using GPU, or using a different BLAS library, you may want to update those settings accordingly.
    echo "CPU_ONLY := 1" >> Makefile.config
    echo "BLAS := atlas" >> Makefile.config
    echo "INCLUDE_DIRS += /usr/include/hdf5/serial/" >> Makefile.config
    echo "LIBRARY_DIRS += /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config
    popd

    #compile CaffeOnSpark
    pushd ${CAFFE_ON_SPARK}
    #always clean up the environment before building (especially when rebuiding), or there will be errors such as "failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2"
    make clean 
    #the build step usually takes 20~30 mins, since it has a lot maven dependencies
    make build 
    popd
    export LD_LIBRARY_PATH=${CAFFE_ON_SPARK}/caffe-public/distribute/lib:${CAFFE_ON_SPARK}/caffe-distri/distribute/lib

    hadoop fs -mkdir -p wasb:///projects/machine_learning/image_dataset

    ${CAFFE_ON_SPARK}/scripts/setup-mnist.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/mnist_*_lmdb wasb:///projects/machine_learning/image_dataset/

    ${CAFFE_ON_SPARK}/scripts/setup-cifar10.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/cifar10_*_lmdb wasb:///projects/machine_learning/image_dataset/

    #put the already compiled CaffeOnSpark libraries to wasb storage, then read back to each node using script actions. This is because CaffeOnSpark requires all the nodes have the libarries
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-public/distribute/lib/
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-distri/distribute/lib/* /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-public/distribute/lib/* /CaffeOnSpark/caffe-public/distribute/lib/

Musíte udělat víc než co je uveden v dokumentaci CaffeOnSpark. Změny jsou tyto:
- Změnit pouze na procesor a používat libatlas pro tento konkrétní účel.
- Uveďte datové sady do úložiště objektů BLOB, který je sdílené umístění, které je přístupné pro všechny uzly pracovního procesu pro pozdější použití.
- Vložit zkompilované knihovny Caffe do úložiště objektů BLOB a později zkopíruje tyto knihovny pro všechny uzly pomocí akcí skriptů, aby se zabránilo další kompilace čas.


### <a name="troubleshooting-an-ant-buildexception-has-occured-exec-returned-2"></a>Řešení potíží: Ant BuildException došlo k chybě: exec vrátí: 2

Při prvním pokusu o sestavení CaffeOnSpark, někdy je se dozvíte

    failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

Jednoduše vyčistit úložiště kódu pomocí "make čisté" a pak spusťte "Zkontrolujte sestavení" se tento problém vyřešit, tak dlouho, dokud máte správnými závislostmi.

### <a name="troubleshooting-maven-repository-connection-time-out"></a>Řešení potíží: Časový limit připojení Maven úložiště

Někdy maven mi poskytuje vypršení časového limitu Chyba připojení, podobně jako následující:

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request to {s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

OK bude po čekání pár minut a pak zkuste použít k opětovnému sestavení kódu, proto může být, že Maven nějakým způsobem omezuje provoz z dané IP adresy.


### <a name="troubleshooting-test-failure-for-caffe"></a>Řešení potíží: Testujte selhání Caffe

Pravděpodobně se zobrazí selhání testu při provádění konečné kontrolu CaffeOnSpark, podobně jako s níže. To je prabably související s kódováním UTF-8, ale by neměla mít vliv na použití Caffe

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-the-required-libraries-to-all-the-worker-nodes"></a>Krok 3: Distribuujte požadované knihovny pro všechny uzly pracovního procesu

Dalším krokem je distribuovat do knihoven (v podstatě knihovny v CaffeOnSpark/caffe veřejný nebo distribuovat/lib/a CaffeOnSpark/caffe distribuční nebo distribuovat/lib /) na všech uzlech. V kroku 2 umístí jsme tyto knihovny úložiště objektů BLOB a v tomto kroku použijeme akcí skriptů a zkopírujte ho do hlavního uzlů a uzlů pracovního procesu.

K tomu jednoduchý spustit akci skriptu jako níže (je třeba přejděte do správného umístění, které jsou specifické pro váš cluster):

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

Vzhledem k tomu, že v kroku 2, jsme umístí jej úložiště objektů BLOB, který je dostupný pro všechny uzly, v tomto kroku jsme právě jednoduše zkopírujte jej do všech uzlů.

## <a name="step-4-compose-a-caffe-model-and-run-it-distributely"></a>Krok 4: Tvoří Caffe modelu a potom ho spusťte distributely

Po spuštění výše uvedené kroky, Caffe je již nainstalován na headnode a snažíme se pustit do práce. Dalším krokem je zápis Caffe modelu. 

Caffe je pomocí "výrazovou architektury", kde pro skládání modelu, stačí zadat konfigurační soubor, a bez kódování vůbec (ve většině případů). Proto se podíváme existuje. 

Model, který jsme bude dnes cvičení je ukázkový model MNIST školení. Databázi MNIST psané číslic má sadu 60 000 příklady školení a testovací sadu 10 000 příklady. Je podmnožinou většímu z NIST k dispozici. Číslice byly normalized velikost a zarovnaný na střed v bitové kopii pevné velikosti. CaffeOnSpark má některé skripty ke stažení datovou sadu a převádět je do správném formátu.

CaffeOnSpark uvádí některé ukázkové topologie sítě pro MNIST školení. Má dobrý návrh rozdělení síťovou architekturu (topologie sítě) a optimalizace. V takovém případě existují dva soubory potřebné: 

soubor "Solver" (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) se používá pro dohled nad optimalizace a generování parametr aktualizace. Například definuje, zda procesoru nebo GPU se použije, co je výkonnosti, kolik opakování bude, atd. Také definuje, které neuron síťové topologie program využít (což je druhý soubor, který potřebujeme). Další informace o Solver naleznete [Caffe dokumentaci](http://caffe.berkeleyvision.org/tutorial/solver.html).

Vzhledem k tomu, že používáme procesoru než GPU, budeme v tomto příkladu měli změnit poslední řádek pro:

    # solver mode: CPU or GPU
    solver_mode: CPU

![Konfigurace Caffe](./media/hdinsight-deep-learning-caffe-spark/Caffe-1.png)

Podle potřeby můžete změnit další řádky.

Druhý soubor (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) definuje, jak vypadá neuron sítě jako a relevantní vstupní a výstupní soubor. Také je potřeba aktualizovat soubor tak, aby odrážela umístění dat školení. Změňte následující součástí lenet_memory_train_test.prototxt (je třeba přejděte do správného umístění, které jsou specifické pro váš cluster):

- Změňte "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" na "wasb: / / / projekty/machine_learning/image_dataset/mnist_train_lmdb"
- Změňte "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" na "wasb: / / / projekty/machine_learning/image_dataset/mnist_test_lmdb"

![Konfigurace Caffe](./media/hdinsight-deep-learning-caffe-spark/Caffe-2.png)

Další informace o tom, jak definovat sítě, zkontrolujte, zda [Caffe dokumentace pro datovou sadu MNIST](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)

Pro účely tomto blogu používáme pouze tomto jednoduchém příkladu MNIST. Z hlavního uzlu by měl spustit příkaz níže:

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

V podstatě se distribuuje požadované soubory (lenet_memory_solver.prototxt a lenet_memory_train_test.prototxt) pro každý kontejner YARN a také nastavit relevantní CESTU jednotlivých ovladačů nebo vykonavatele Spark na LD_LIBRARY_PATH, která je definována v předchozím fragmentu kódu a odkazuje na umístění, které má CaffeOnSpark knihovny. 

## <a name="monitoring-and-troubleshooting"></a>Monitorování a řešení potíží

Vzhledem k tomu, že používáme YARN clusteru režim, v takovém případě Spark ovladače budou naplánovány na libovolný kontejneru (a libovolné pracovního uzlu) jenom se zobrazí v konzole něco podobného jako výstup:

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

Pokud chcete vědět, co se stalo, obvykle potřebujete získat Spark ovladače protokol, který obsahuje další informace. V takovém případě budete muset přejít do rozhraní YARN najít relevantní protokoly YARN. Můžete získat rozhraní YARN tuto adresu URL: 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN UŽIVATELSKÉHO ROZHRANÍ](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-1.png)

Podívejte se na tom, kolik prostředky se přidělují pro tuto konkrétní aplikaci může trvat. Můžete kliknout na odkaz "Scheduler" a pak se zobrazí, že pro tuto aplikaci, nejsou 9 kontejnery systémem. Můžeme požádat YARN zajistit 8 vykonavatelů a jiný kontejner je pro proces ovladačů. 

![YARN plánovače](./media/hdinsight-deep-learning-caffe-spark/YARN-Scheduler.png)

Můžete chtít Zkontrolujte protokoly ovladače nebo kontejneru protokoly, pokud tam jsou chyby. Pro protokoly ovladačů můžete kliknutím na ID aplikace v uživatelském rozhraní YARN a pak klikněte na tlačítko "Protokoly". Ovladač protokolu se zapisují do stderr.

![UŽIVATELSKÉ ROZHRANÍ YARN 2](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-2.png)

Například může se zobrazit některé chyby níže z protokolů ovladačů, což značí, že přidělíte příliš mnoho vykonavatelů.

    17/02/01 07:26:06 ERROR ApplicationMaster: User class threw exception: java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
    java.lang.IllegalStateException: Insufficient training data. Please adjust hyperparameters or increase dataset.
        at com.yahoo.ml.caffe.CaffeOnSpark.trainWithValidation(CaffeOnSpark.scala:261)
        at com.yahoo.ml.caffe.CaffeOnSpark$.main(CaffeOnSpark.scala:42)
        at com.yahoo.ml.caffe.CaffeOnSpark.main(CaffeOnSpark.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.spark.deploy.yarn.ApplicationMaster$$anon$2.run(ApplicationMaster.scala:627)

V některých případech problému může dojít v vykonavatelů spíše než ovladače. V takovém případě budete muset v protokolech kontejneru. Můžete vždy získat protokoly kontejner a potom získat kontejneru se nezdařilo. Tato chyba může odpovídat například při spuštění Caffe.

    17/02/01 07:12:05 WARN YarnAllocator: Container marked as failed: container_1485916338528_0008_05_000005 on host: 10.0.0.14. Exit status: 134. Diagnostics: Exception from container-launch.
    Container id: container_1485916338528_0008_05_000005
    Exit code: 134
    Exception message: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

    Stack trace: ExitCodeException exitCode=134: /bin/bash: line 1: 12230 Aborted                 (core dumped) LD_LIBRARY_PATH=/usr/hdp/current/hadoop-client/lib/native:/usr/hdp/current/hadoop-client/lib/native/Linux-amd64-64:/home/xiaoyzhu/CaffeOnSpark/caffe-public/distribute/lib:/home/xiaoyzhu/CaffeOnSpark/caffe-distri/distribute/lib /usr/lib/jvm/java-8-openjdk-amd64/bin/java -server -Xmx4608m '-Dhdp.version=' '-Detwlogger.component=sparkexecutor' '-DlogFilter.filename=SparkLogFilters.xml' '-DpatternGroup.filename=SparkPatternGroups.xml' '-Dlog4jspark.root.logger=INFO,console,RFA,ETW,Anonymizer' '-Dlog4jspark.log.dir=/var/log/sparkapp/${user.name}' '-Dlog4jspark.log.file=sparkexecutor.log' '-Dlog4j.configuration=file:/usr/hdp/current/spark2-client/conf/log4j.properties' '-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl' -Djava.io.tmpdir=/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/tmp '-Dspark.driver.port=43942' '-Dspark.history.ui.port=18080' '-Dspark.ui.port=0' -Dspark.yarn.app.container.log.dir=/mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005 -XX:OnOutOfMemoryError='kill %p' org.apache.spark.executor.CoarseGrainedExecutorBackend --driver-url spark://CoarseGrainedScheduler@10.0.0.13:43942 --executor-id 4 --hostname 10.0.0.14 --cores 3 --app-id application_1485916338528_0008 --user-class-path file:/mnt/resource/hadoop/yarn/local/usercache/xiaoyzhu/appcache/application_1485916338528_0008/container_1485916338528_0008_05_000005/__app__.jar > /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stdout 2> /mnt/resource/hadoop/yarn/log/application_1485916338528_0008/container_1485916338528_0008_05_000005/stderr

        at org.apache.hadoop.util.Shell.runCommand(Shell.java:933)
        at org.apache.hadoop.util.Shell.run(Shell.java:844)
        at org.apache.hadoop.util.Shell$ShellCommandExecutor.execute(Shell.java:1123)
        at org.apache.hadoop.yarn.server.nodemanager.DefaultContainerExecutor.launchContainer(DefaultContainerExecutor.java:225)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:317)
        at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:83)
        at java.util.concurrent.FutureTask.run(FutureTask.java:266)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
        at java.lang.Thread.run(Thread.java:745)


    Container exited with a non-zero exit code 134

V takovém případě budete muset získat ID se nezdařilo kontejneru (v případě výše je container_1485916338528_0008_05_000005). Pak budete muset spustit 

    yarn logs -containerId container_1485916338528_0008_03_000005

z headnode. Po zkontrolování chyby kontejneru, je příčinou pomocí režimu grafický procesor (kde měli použít procesoru režim místo) v lenet_memory_solver.prototxt.

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written to STDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a>Získávání výsledků

Vzhledem k tomu, že jsme se přidělování 8 vykonavatelů a síťové topologie je jednoduché, má pouze trvat zhruba 30 minut spustit výsledek. Z příkazového řádku, uvidíte jsme put modelu wasb:///mnist.model a put výsledky do složky s názvem wasb: / / / mnist_features_result.

Výsledky můžete získat spuštěním

    hadoop fs -cat hdfs:///mnist_features_result/*

a výsledek bude vypadat takto:

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

SampleID reprezentuje ID v datové sadě MNIST a popisek je číslo, které identifikuje modelu.


## <a name="conclusion"></a>Závěr

V této dokumentaci jste se pokusili nainstalovat CaffeOnSpark se spouštěním jednoduchý příklad. HDInsight je úplná spravované cloudové platformy distribuovaná výpočetní a je nejlepší místo pro spouštění strojového učení a úlohy pokročilou analýzu na velké datové sady, a pro distribuované hloubkové learning, můžete použít Caffe na HDInsight Spark k provádění hloubkové learning úloh.


## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

