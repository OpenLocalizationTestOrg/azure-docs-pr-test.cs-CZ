---
title: "aaaUse Caffe v Azure HDInsight Spark pro distribuované hloubkové learning | Microsoft Docs"
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
ms.openlocfilehash: d6476a7ed3a0df38538e845d7d5404067b01113c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a>Použití Caffe v Azure HDInsight Spark pro distribuované hloubkové learning


## <a name="introduction"></a>Úvod

Hloubkové learning je ovlivňující všechno zdravotní péče tootransportation toomanufacturing a další. Společnosti jsou vypnutí toodeep učení toosolve pevný problémy, jako je [bitové kopie klasifikace](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [rozpoznávání řeči](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html)objektu rozpoznávání a počítač překlad. 

Existují [mnoha oblíbených rozhraní](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), včetně [kognitivní nástrojů Microsoft](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, atd. Caffe je jedním z hello nejznámější architektury-symbolický (imperativní) neuronové sítě a široce používaných v mnoha oblastech, včetně vize počítače. Kromě toho [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinuje Caffe s Apache Spark, v takovém případě hloubky učení lze snadno použít na existující Hadoop clusteru spolu s Spark ETL kanály, snižuje složitost systému a latence pro učení začátku do konce.

[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) je hello jen nabídka plně spravované cloudové Hadoop, která poskytuje optimalizované s otevřeným zdrojem analytické clustery Spark, Hive, MapReduce, HBase, Storm, Kafka a zajišťoval SLA 99,9 % R Server. Každou z těchto technologií pro velké objemy dat, stejně jako aplikace nezávislých výrobců softwaru, je možné jednoduše nasadit jako spravovaný cluster se zabezpečením a monitorováním na podnikové úrovni.

Někteří uživatelé jsou zprávu s požadavkem o toouse hloubkové učení v HDInsight, což je PaaS Hadoop produktu společnosti Microsoft. Další tooshare jsme bude mít v hello budoucí, ale ještě dnes chceme toosummarize technické blog o toouse Caffe na HDInsight Spark.

Pokud jste nainstalovali Caffe před, si všimnete, že instalace toto rozhraní je chvíli náročné. V tomto blogu nám nejdřív ilustruje způsob tooinstall [Caffe na Spark](https://github.com/yahoo/CaffeOnSpark) pro cluster služby HDInsight, pak použít hello předdefinované MNIST ukázku toodemostrate jak toouse distribuované hloubkové Learning pomocí HDInsight Spark v procesorech.

Existují čtyři hlavní kroky tooget ho fungovat v HDInsight.

1. Nainstalujte hello požadované závislosti na všechny uzly hello
2. Sestavení Caffe na Spark pro HDInsight hello hlavního uzlu
3. Distribuovat hello požadované knihovny tooall hello pracovní uzly
4. Napište Caffe modelu a potom ho spusťte distributely

Vzhledem k tomu, že HDInsight je řešení PaaS, nabízí skvělý funkce - proto je poměrně snadné tooperform některé úlohy. Jedna z funkcí hello, které výraznou používáme v tomto příspěvku na blogu nazývá [akce skriptu](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), pomocí kterého můžete spustit příkazy prostředí toocustomize uzly clusteru (hlavního uzlu, pracovního uzlu nebo hraniční uzel).

## <a name="step-1--install-hello-required-dependencies-on-all-hello-nodes"></a>Krok 1: Instalace hello požadované závislosti na všech uzlech hello

tooget spuštěna, potřebujeme tooinstall hello závislosti, které potřebujeme. Hello Caffe lokality a [CaffeOnSpark lokality](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) nabízí některé wiki velmi užitečná pro instalování hello závislostí pro Spark na YARN režimu (což je hello režim pro HDInsight Spark), ale potřebujeme pár Další závislosti pro tooadd pro HDInsight platformu. Jsme se použití akce skriptu hello jak je uvedeno níže a spusťte ho na všechny hello head uzlů a uzlů pracovního procesu. Tato akce skriptu bude trvat asi 20 minut, jak tyto závislosti také závisí na jiné balíčky. By ji umístit do některé umístění, které je přístupné tooyour clusteru HDInsight, jako je například umístění GitHub nebo hello výchozí účet úložiště objektů BLOB.

    #!/bin/bash
    #Please be aware that installing hello below will add additional 20 mins toocluster creation because of hello dependencies
    #installing all dependencies, including hello ones mentioned in http://caffe.berkeleyvision.org/install_apt.html, as well a few packages that are not included in HDInsight, such as gflags, glog, lmdb, numpy
    #It seems numpy will only needed during compilation time, but for safety purpose we install them on all hello nodes

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


Existují dva kroky v akce skriptu hello výše. prvním krokem Hello je tooinstall všechny hello požadované knihovny. Tyto knihovny zahrnují hello potřebné knihovny pro kompilaci Caffe (například gflags, glog) i s Caffe (například numpy). Libatlas se používá pro optimalizaci procesoru, ale můžete vždy následují hello CaffeOnSpark wiki k instalaci další optimalizace knihovny, například MKL nebo CUDA (pro GPU).

Hello druhým krokem je toodownload, kompilace a nainstalovat protobuf 2.5.0 pro Caffe za běhu. Protobuf 2.5.0 [je vyžadován](https://github.com/yahoo/CaffeOnSpark/issues/87), ale tato verze není k dispozici jako balíček na Ubuntu 16, takže potřebujeme toocompile z hello zdrojového kódu. Na hello Internet návod, jak jsou také několik prostředků toocompile, jako například [to](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)

Začněte toosimply, jenom pro můžete spouštět tuto akci skriptu vašeho clusteru tooall hello pracovní uzly uzly a head (pro HDInsight 3.5). Můžete buď spustit hello akcí skriptů pro cluster s podporou spuštěné, nebo hello skriptových akcí můžete taky spustit během doby zřízení clusteru hello. Další informace o hello akcí skriptů naleznete v dokumentaci hello [sem](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)

![Skript akce tooInstall závislosti](./media/hdinsight-deep-learning-caffe-spark/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-hello-head-node"></a>Krok 2: Vytvoření Caffe na Spark pro HDInsight hello hlavního uzlu

druhý krok Hello je toobuild Caffe na hello headnode a pak distribuovat hello zkompilovat knihovny tooall hello pracovním uzlům. V tomto kroku je nutné příliš[ssh do vaší headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), jednoduše podle hello [proces sestavení CaffeOnSpark](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), a dále je skript hello toobuild CaffeOnSpark můžete použít s několika dalších krocích. 

    #!/bin/bash
    git clone https://github.com/yahoo/CaffeOnSpark.git --recursive
    export CAFFE_ON_SPARK=$(pwd)/CaffeOnSpark

    pushd ${CAFFE_ON_SPARK}/caffe-public/
    cp Makefile.config.example Makefile.config
    echo "INCLUDE_DIRS += ${JAVA_HOME}/include" >> Makefile.config
    #Below configurations might need toobe updated based on actual cases. For example, if you are using GPU, or using a different BLAS library, you may want tooupdate those settings accordingly.
    echo "CPU_ONLY := 1" >> Makefile.config
    echo "BLAS := atlas" >> Makefile.config
    echo "INCLUDE_DIRS += /usr/include/hdf5/serial/" >> Makefile.config
    echo "LIBRARY_DIRS += /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config
    popd

    #compile CaffeOnSpark
    pushd ${CAFFE_ON_SPARK}
    #always clean up hello environment before building (especially when rebuiding), or there will be errors such as "failed tooexecute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2"
    make clean 
    #hello build step usually takes 20~30 mins, since it has a lot maven dependencies
    make build 
    popd
    export LD_LIBRARY_PATH=${CAFFE_ON_SPARK}/caffe-public/distribute/lib:${CAFFE_ON_SPARK}/caffe-distri/distribute/lib

    hadoop fs -mkdir -p wasb:///projects/machine_learning/image_dataset

    ${CAFFE_ON_SPARK}/scripts/setup-mnist.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/mnist_*_lmdb wasb:///projects/machine_learning/image_dataset/

    ${CAFFE_ON_SPARK}/scripts/setup-cifar10.sh
    hadoop fs -put -f ${CAFFE_ON_SPARK}/data/cifar10_*_lmdb wasb:///projects/machine_learning/image_dataset/

    #put hello already compiled CaffeOnSpark libraries toowasb storage, then read back tooeach node using script actions. This is because CaffeOnSpark requires all hello nodes have hello libarries
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-public/distribute/lib/
    hadoop fs -mkdir -p /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-distri/distribute/lib/* /CaffeOnSpark/caffe-distri/distribute/lib/
    hadoop fs -put CaffeOnSpark/caffe-public/distribute/lib/* /CaffeOnSpark/caffe-public/distribute/lib/

Může být nutné toodo víc než jaké hello dokumentaci CaffeOnSpark uvádí. Hello změny jsou:
- Změňte pouze tooCPU a použít libatlas pro tento konkrétní účel.
- Vložení hello datové sady toohello úložiště objektů BLOB, což je sdílené umístění, které je přístupné tooall uzlů pracovního procesu pro pozdější použití.
- Vložení hello zkompilovat Caffe knihovny tooBLOB úložiště a později se zkopírujte tyto knihovny tooall hello uzlů pomocí doby další kompilace tooavoid akce skriptu.


### <a name="troubleshooting-an-ant-buildexception-has-occured-exec-returned-2"></a>Řešení potíží: Ant BuildException došlo k chybě: exec vrátí: 2

Při prvním pokusu o toobuild CaffeOnSpark, někdy je se dozvíte

    failed tooexecute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

Jednoduše čistou hello úložiště kódu pomocí "Zkontrolujte čisté" a pak spusťte "Zkontrolujte sestavení" se tento problém vyřešit, tak dlouho, dokud máte hello správnými závislostmi.

### <a name="troubleshooting-maven-repository-connection-time-out"></a>Řešení potíží: Časový limit připojení Maven úložiště

Někdy maven mi poskytuje chyba vypršení časového limitu hello připojení, podobně jako toobelow:

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request too{s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

Bude OK po čekání pár minut a opakujte právě toorebuild hello kódu, takže může být Maven nějakým způsobem omezení hello provoz z dané IP adresy.


### <a name="troubleshooting-test-failure-for-caffe"></a>Řešení potíží: Testujte selhání Caffe

Pravděpodobně se zobrazí selhání testu při provádění hello poslední kontrola CaffeOnSpark, podobně jako s níže. To je prabably související s kódováním UTF-8, ale by neměla mít vliv na použití hello Caffe

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-hello-required-libraries-tooall-hello-worker-nodes"></a>Krok 3: Distribuujte hello požadované knihovny tooall hello pracovní uzly

dalším krokem Hello je toodistribute hello knihovny (v podstatě hello knihovny v CaffeOnSpark/caffe veřejný nebo distribuovat/lib/a CaffeOnSpark/caffe distribuční nebo distribuovat/lib /) tooall hello uzly. V kroku 2, můžeme tyto knihovny na umístit úložiště objektů BLOB a v tomto kroku použijeme skript akce toocopy ho tooall hello head uzlů a uzlů pracovního procesu.

toodo se jednoduché spustit akci skriptu jako níže (je třeba toopoint toohello správného umístění konkrétní tooyour clusteru):

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

Vzhledem k tomu, že v kroku 2, jsme umístí jej hello úložiště objektů BLOB, který je přístupný tooall hello uzly, v tomto kroku jsme právě jednoduše zkopírujte jej tooall hello uzly.

## <a name="step-4-compose-a-caffe-model-and-run-it-distributely"></a>Krok 4: Tvoří Caffe modelu a potom ho spusťte distributely

Po spuštění hello výše uvedené kroky, Caffe je již nainstalován na hello headnode a snažíme se dobrý toogo. dalším krokem Hello je toowrite Caffe modelu. 

Caffe používá "výrazovou architekturu", kde skládání modelu, můžete jednoduše nutné toodefine konfigurační soubor a bez kódování vůbec (ve většině případů). Proto se podíváme existuje. 

Hello model, který jsme bude dnes cvičení je ukázkový model MNIST školení. databáze MNIST Hello psané číslic má sadu 60 000 příklady školení a testovací sadu 10 000 příklady. Je podmnožinou většímu z NIST k dispozici. Hello číslic byly normalized velikost a zarovnaný na střed v bitové kopii pevné velikosti. CaffeOnSpark má datovou sadu některé skripty toodownload hello a převádět je do hello správném formátu.

CaffeOnSpark uvádí některé ukázkové topologie sítě pro MNIST školení. Má dobrý návrh rozdělení hello síťovou architekturu (hello topologie sítě hello) a optimalizace. V takovém případě existují dva soubory potřebné: 

soubor "Solver" Hello (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) se používá pro dohled nad hello optimalizace a generování parametr aktualizace. Například definuje, zda procesoru nebo GPU se použije, co je hello výkonnosti, kolik opakování bude, atd. Také definuje, které neuron topologie sítě by měl hello program použití (což je hello druhý soubor, potřebujeme). Další informace o Solver naleznete příliš[Caffe dokumentaci](http://caffe.berkeleyvision.org/tutorial/solver.html).

V tomto příkladu jsme měli vzhledem k tomu, že používáme procesoru než GPU, změnit hello poslední řádek:

    # solver mode: CPU or GPU
    solver_mode: CPU

![Konfigurace Caffe](./media/hdinsight-deep-learning-caffe-spark/Caffe-1.png)

Podle potřeby můžete změnit další řádky.

druhý soubor Hello (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) definuje, jak vypadá hello neuron sítě jako a hello relevantní vstupní a výstupní soubor. Potřebujeme také umístění dat školení tooreflect tooupdate hello souboru hello. Změna hello následující součástí lenet_memory_train_test.prototxt (je třeba toopoint toohello správného umístění konkrétní tooyour clusteru):

- změnit hello "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" příliš "wasb: / / / projekty/machine_learning/image_dataset/mnist_train_lmdb"
- Změňte "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" příliš "wasb: / / / projekty/machine_learning/image_dataset/mnist_test_lmdb"

![Konfigurace Caffe](./media/hdinsight-deep-learning-caffe-spark/Caffe-2.png)

Další informace o tom, jak toodefine hello sítě, zkontrolujte, zda text hello [Caffe dokumentace pro datovou sadu MNIST](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)

Za účelem hello tomto blogu používáme pouze tomto jednoduchém příkladu MNIST. Spusťte příkaz hello níže z hlavního uzlu hello:

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

V podstatě distribuuje hello požadované soubory (lenet_memory_solver.prototxt a lenet_memory_train_test.prototxt) tooeach PŘÍZOVÝ kontejneru a také nastavit hello relevantní CESTU tooLD_LIBRARY_PATH ovladače nebo vykonavatele každý Spark, která je definována v hello Předchozí kód fragment kódu a body toohello umístění, které má CaffeOnSpark knihovny. 

## <a name="monitoring-and-troubleshooting"></a>Monitorování a řešení potíží

Vzhledem k tomu, že používáme YARN clusteru režim, v takovém případě bude hello Spark ovladač naplánované tooan libovolný kontejneru (a libovolné pracovního uzlu) můžete byste měli vidět jenom v výstup hello konzoly vytvořeného něco jako:

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

Pokud chcete tooknow, co se stalo, je obvykle třeba tooget hello Spark ovladače protokol, který obsahuje další informace. V takovém případě musíte toogo toohello uživatelském rozhraní YARN toofind hello relevantní protokolů YARN. Můžete získat hello uživatelském rozhraní YARN tuto adresu URL: 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN UŽIVATELSKÉHO ROZHRANÍ](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-1.png)

Podívejte se na tom, kolik prostředky se přidělují pro tuto konkrétní aplikaci může trvat. Můžete kliknout na odkaz "Scheduler" hello, a pak se zobrazí, že pro tuto aplikaci, nejsou 9 kontejnery systémem. Můžeme požádat YARN tooprovide 8 vykonavatelů a jiný kontejner je pro proces ovladačů. 

![YARN plánovače](./media/hdinsight-deep-learning-caffe-spark/YARN-Scheduler.png)

Pokud jsou selhání může být vhodné toocheck hello ovladač protokolu nebo kontejner protokoly. Pro ovladač protokoly můžete kliknutím na ID aplikace hello v uživatelském rozhraní YARN a pak klikněte na tlačítko "Protokoly" hello. Hello ovladač protokolu se zapisují do stderr.

![UŽIVATELSKÉ ROZHRANÍ YARN 2](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-2.png)

Například může zobrazit některé chyby hello níže z protokolů ovladač hello, což značí, že přidělíte příliš mnoho vykonavatelů.

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

V některých případech hello problému může dojít v vykonavatelů spíše než ovladače. V takovém případě musíte toocheck hello kontejneru protokoly. Můžete vždy získat hello kontejneru protokoly a potom získat hello kontejneru se nezdařilo. Tato chyba může odpovídat například při spuštění Caffe.

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

V takovém případě je třeba ID kontejneru tooget hello se nezdařilo (v hello výše případ, je container_1485916338528_0008_05_000005). Budete potřebovat toorun 

    yarn logs -containerId container_1485916338528_0008_03_000005

z hello headnode. Po zkontrolování chyby kontejneru, je příčinou pomocí režimu grafický procesor (kde měli použít procesoru režim místo) v lenet_memory_solver.prototxt.

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written tooSTDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a>Získávání výsledků

Vzhledem k tomu, že jsme se přidělování 8 vykonavatelů a topologie sítě hello je jednoduché, má pouze trvat přibližně 30 minut toorun hello výsledek. Z příkazového řádku hello, uvidíte jsme put hello modelu toowasb:///mnist.model a put hello výsledky tooa složku s názvem wasb: / / / mnist_features_result.

Hello výsledky můžete získat spuštěním

    hadoop fs -cat hdfs:///mnist_features_result/*

a hello výsledek bude vypadat takto:

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

Hello SampleID reprezentuje ID hello v datové sadě MNIST hello a popisek hello je, že identifikuje hello číslo, které hello modelu.


## <a name="conclusion"></a>Závěr

V této dokumentaci pokoušíte tooinstall CaffeOnSpark se spouštěním jednoduchý příklad. HDInsight je úplná spravované cloudové platformy distribuovaná výpočetní a hello nejlepší místo pro spouštění strojového učení a úlohy pokročilou analýzu na velké sady dat a pro distribuované hloubkové learning, můžete použít Caffe na HDInsight Spark tooperform hloubkové učení úlohy.


## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

