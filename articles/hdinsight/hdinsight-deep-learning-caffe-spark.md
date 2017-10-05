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
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a><span data-ttu-id="c1ac6-103">Použití Caffe v Azure HDInsight Spark pro distribuované hloubkové learning</span><span class="sxs-lookup"><span data-stu-id="c1ac6-103">Use Caffe on Azure HDInsight Spark for distributed deep learning</span></span>


## <a name="introduction"></a><span data-ttu-id="c1ac6-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="c1ac6-104">Introduction</span></span>

<span data-ttu-id="c1ac6-105">Hloubkové learning je ovlivňující všechno zdravotní péče k Transport do výroby a další.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-105">Deep learning is impacting everything from healthcare to transportation to manufacturing, and more.</span></span> <span data-ttu-id="c1ac6-106">Společnosti jsou vypnutí a učení pevný problémy, jako je třeba vyřešit [bitové kopie klasifikace](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [rozpoznávání řeči](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html)objektu rozpoznávání a počítač překlad.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-106">Companies are turning to deep learning to solve hard problems, like [image classification](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [speech recognition](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html), object recognition, and machine translation.</span></span> 

<span data-ttu-id="c1ac6-107">Existují [mnoha oblíbených rozhraní](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), včetně [kognitivní nástrojů Microsoft](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, atd. Caffe je nejslavnější rozhraní-symbolický (imperativní) neuronové sítě a široce používaných v mnoha oblastech, včetně vize počítače.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-107">There are [many popular frameworks](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), including [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, etc. Caffe is one of the most famous non-symbolic (imperative) neural network frameworks, and widely used in many areas including computer vision.</span></span> <span data-ttu-id="c1ac6-108">Kromě toho [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinuje Caffe s Apache Spark, v takovém případě hloubky učení lze snadno použít na existující Hadoop clusteru spolu s Spark ETL kanály, snižuje složitost systému a latence pro učení začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-108">Furthermore, [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) combines Caffe with Apache Spark, in which case deep learning can be easily used on an existing Hadoop cluster together with Spark ETL pipelines, reducing system complexity and latency for end-to-end learning.</span></span>

<span data-ttu-id="c1ac6-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) je zajištěna nabídku pouze plně spravované cloudové Hadoop, které poskytuje optimalizovanou s otevřeným zdrojem analytické clustery Spark, Hive, MapReduce, HBase, Storm, Kafka a R Server SLA 99,9 %.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) is the only fully-managed cloud Hadoop offering that provides optimized open source analytic clusters for Spark, Hive, MapReduce, HBase, Storm, Kafka, and R Server backed by a 99.9% SLA.</span></span> <span data-ttu-id="c1ac6-110">Každou z těchto technologií pro velké objemy dat, stejně jako aplikace nezávislých výrobců softwaru, je možné jednoduše nasadit jako spravovaný cluster se zabezpečením a monitorováním na podnikové úrovni.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-110">Each of these big data technologies and ISV applications are easily deployable as managed clusters with enterprise-level security and monitoring.</span></span>

<span data-ttu-id="c1ac6-111">Někteří uživatelé žádáme, nám o tom, jak používat hloubkové učení v HDInsight, což je PaaS Hadoop produktu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-111">Some users are asking us about how to use deep learning on HDInsight, which is Microsoft's PaaS Hadoop product.</span></span> <span data-ttu-id="c1ac6-112">Jsme bude mít více sdílení v budoucnu, ale dnes chceme shrnout technické blog o tom, jak používat Caffe na HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-112">We will have more to share in the future, but today we want to summarize a technical blog on how to use Caffe on HDInsight Spark.</span></span>

<span data-ttu-id="c1ac6-113">Pokud jste nainstalovali Caffe před, si všimnete, že instalace toto rozhraní je chvíli náročné.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-113">If you have installed Caffe before, you will notice that installing this framework is a little bit challenging.</span></span> <span data-ttu-id="c1ac6-114">V tomto blogu jsme se nejprve ukazují, jak nainstalovat [Caffe na Spark](https://github.com/yahoo/CaffeOnSpark) pro cluster služby HDInsight, pak použít předdefinované MNIST ukázku k demostrate použití distribuovaných hloubkové Learning pomocí HDInsight Spark v procesorech.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-114">In this blog, we will first illustrate how to install [Caffe on Spark](https://github.com/yahoo/CaffeOnSpark) for an HDInsight cluster, then use the built-in MNIST demo to demostrate how to use Distributed Deep Learning using HDInsight Spark on CPUs.</span></span>

<span data-ttu-id="c1ac6-115">Existují čtyři hlavní kroky se dá stáhnout fungovat v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-115">There are four major steps to get it work on HDInsight.</span></span>

1. <span data-ttu-id="c1ac6-116">Nainstalujte požadované závislosti pro všechny uzly v</span><span class="sxs-lookup"><span data-stu-id="c1ac6-116">Install the required dependencies on all the nodes</span></span>
2. <span data-ttu-id="c1ac6-117">Sestavení Caffe na Spark pro HDInsight z hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="c1ac6-117">Build Caffe on Spark for HDInsight on the head node</span></span>
3. <span data-ttu-id="c1ac6-118">Distribuovat požadované knihovny pro všechny uzly pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="c1ac6-118">Distribute the required libraries to all the worker nodes</span></span>
4. <span data-ttu-id="c1ac6-119">Napište Caffe modelu a potom ho spusťte distributely</span><span class="sxs-lookup"><span data-stu-id="c1ac6-119">Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="c1ac6-120">Vzhledem k tomu, že HDInsight je řešení PaaS, proto je poměrně snadné k provedení některých úkolů nabízí skvělé funkce.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-120">Since HDInsight is a PaaS solution, it offers great platform features - so it is quite easy to perform some tasks.</span></span> <span data-ttu-id="c1ac6-121">Jedna z funkcí, které výraznou používáme v tomto příspěvku na blogu nazývá [akce skriptu](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), pomocí kterého můžete spustit příkazy prostředí pro přizpůsobení uzly clusteru (hlavního uzlu, pracovního uzlu nebo hraniční uzel).</span><span class="sxs-lookup"><span data-stu-id="c1ac6-121">One of the features that we heavily use in this blog post is called [Script Action](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), with which you can execute shell commands to customize cluster nodes (head node, worker node, or edge node).</span></span>

## <a name="step-1--install-the-required-dependencies-on-all-the-nodes"></a><span data-ttu-id="c1ac6-122">Krok 1: Instalace požadované závislosti pro všechny uzly v</span><span class="sxs-lookup"><span data-stu-id="c1ac6-122">Step 1:  Install the required dependencies on all the nodes</span></span>

<span data-ttu-id="c1ac6-123">Abyste mohli začít, potřebujeme závislosti, které je třeba nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-123">To get started, we need to install the dependencies we need.</span></span> <span data-ttu-id="c1ac6-124">Webu Caffe a [CaffeOnSpark lokality](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) nabízí některé velmi užitečné wiki pro instalaci závislosti pro Spark na YARN režimu (což je režim pro HDInsight Spark), ale je potřeba přidat pár Další závislosti pro platformu HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-124">The Caffe site and [CaffeOnSpark site](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) offers some very useful wiki for installing the dependencies for Spark on YARN mode (which is the mode for HDInsight Spark), but we need to add a few more dependencies for HDInsight platform.</span></span> <span data-ttu-id="c1ac6-125">Jsme se použití akce skriptu jak je uvedeno níže a spusťte ho na všechny head uzlů a uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-125">We will use the script action as below and run it on all the head nodes and worker nodes.</span></span> <span data-ttu-id="c1ac6-126">Tato akce skriptu bude trvat asi 20 minut, jak tyto závislosti také závisí na jiné balíčky.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-126">This script action will take about 20 minutes, as those dependencies also depend on other packages.</span></span> <span data-ttu-id="c1ac6-127">Byste měli umístit v některém umístění, ke kterému mají přístup ke svému clusteru HDInsight, jako je například umístění GitHub nebo výchozí účet úložiště objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-127">You should put it in some location that is accessible to your HDInsight cluster, such as a GitHub location or the default BLOB storage account.</span></span>

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


<span data-ttu-id="c1ac6-128">Existují dva kroky ve výše uvedené akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-128">There are two steps in the script action above.</span></span> <span data-ttu-id="c1ac6-129">Prvním krokem je instalace potřebných knihoven.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-129">The first step is to install all the required libraries.</span></span> <span data-ttu-id="c1ac6-130">Tyto knihovny obsahovat potřebné knihovny pro kompilaci Caffe (například gflags, glog) i s Caffe (například numpy).</span><span class="sxs-lookup"><span data-stu-id="c1ac6-130">Those libraries include the necessary libraries for both compiling Caffe(such as gflags, glog) and running Caffe (such as numpy).</span></span> <span data-ttu-id="c1ac6-131">Libatlas se používá pro optimalizaci procesoru, ale je můžete provést na wikiwebu CaffeOnSpark o instalaci další optimalizace knihovny, například MKL nebo CUDA (pro GPU).</span><span class="sxs-lookup"><span data-stu-id="c1ac6-131">We are using libatlas for CPU optimization, but you can always follow the CaffeOnSpark wiki on installing other optimization libraries, such as MKL or CUDA (for GPU).</span></span>

<span data-ttu-id="c1ac6-132">Druhým krokem je chcete stáhnout, kompilace a nainstalovat protobuf 2.5.0 pro Caffe za běhu.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-132">The second step is to download, compile, and install protobuf 2.5.0 for Caffe during runtime.</span></span> <span data-ttu-id="c1ac6-133">Protobuf 2.5.0 [je vyžadován](https://github.com/yahoo/CaffeOnSpark/issues/87), ale tato verze není k dispozici jako balíček na Ubuntu 16, takže potřebujeme zkompilovat ze zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-133">Protobuf 2.5.0 [is required](https://github.com/yahoo/CaffeOnSpark/issues/87), however this version is not available as a package on Ubuntu 16, so we need to compile it from the source code.</span></span> <span data-ttu-id="c1ac6-134">Existují také několik prostředkům na Internetu o tom, jak kompilovat, jako například [to](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span><span class="sxs-lookup"><span data-stu-id="c1ac6-134">There are also a few resources on the Internet on how to compile it, such as [this](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span></span>

<span data-ttu-id="c1ac6-135">Jednoduše začít můžete jenom spustit tuto akci skriptu pro váš cluster pro všechny uzly pracovního procesu a hlavních uzlech (pro HDInsight 3.5).</span><span class="sxs-lookup"><span data-stu-id="c1ac6-135">To simply get started, you can just run this script action against your cluster to all the worker nodes and head nodes (for HDInsight 3.5).</span></span> <span data-ttu-id="c1ac6-136">Můžete buď spustit akcí skriptů pro cluster s podporou spuštěné, nebo akce skriptu můžete také spustit během doby zřízení clusteru.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-136">You can either run the script actions for a running cluster, or you can also run the script actions during the cluster provision time.</span></span> <span data-ttu-id="c1ac6-137">Další informace o akcí skriptů naleznete v dokumentaci [sem](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span><span class="sxs-lookup"><span data-stu-id="c1ac6-137">For more details on the script actions, please see the documentation [here](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span></span>

![Skript akce pro instalaci závislosti](./media/hdinsight-deep-learning-caffe-spark/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-the-head-node"></a><span data-ttu-id="c1ac6-139">Krok 2: Vytvoření Caffe na Spark pro HDInsight z hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="c1ac6-139">Step 2: Build Caffe on Spark for HDInsight on the head node</span></span>

<span data-ttu-id="c1ac6-140">Druhým krokem je vytvoření Caffe ve headnode, a pak distribuovat zkompilované knihovny pro všechny uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-140">The second step is to build Caffe on the headnode, and then distribute the compiled libraries to all the worker nodes.</span></span> <span data-ttu-id="c1ac6-141">V tomto kroku budete muset [ssh do vaší headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), jednoduše postupujte [proces sestavení CaffeOnSpark](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), a dále je skript, můžete použít k vytvoření CaffeOnSpark s pár dalších kroků.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-141">In this step, you will need to [ssh into your headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), then simply follow the [CaffeOnSpark build process](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), and below is the script you can use to build CaffeOnSpark with a few additional steps.</span></span> 

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

<span data-ttu-id="c1ac6-142">Musíte udělat víc než co je uveden v dokumentaci CaffeOnSpark.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-142">You may need to do more than what the documentation of CaffeOnSpark says.</span></span> <span data-ttu-id="c1ac6-143">Změny jsou tyto:</span><span class="sxs-lookup"><span data-stu-id="c1ac6-143">The changes are:</span></span>
- <span data-ttu-id="c1ac6-144">Změnit pouze na procesor a používat libatlas pro tento konkrétní účel.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-144">Change to CPU only and use libatlas for this particular purpose.</span></span>
- <span data-ttu-id="c1ac6-145">Uveďte datové sady do úložiště objektů BLOB, který je sdílené umístění, které je přístupné pro všechny uzly pracovního procesu pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-145">Put the datasets to the BLOB storage, which is a shared location that is accessible to all worker nodes for later use.</span></span>
- <span data-ttu-id="c1ac6-146">Vložit zkompilované knihovny Caffe do úložiště objektů BLOB a později zkopíruje tyto knihovny pro všechny uzly pomocí akcí skriptů, aby se zabránilo další kompilace čas.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-146">Put the compiled Caffe libraries to BLOB storage, and later you will copy those libraries to all the nodes using script actions to avoid additional compilation time.</span></span>


### <a name="troubleshooting-an-ant-buildexception-has-occured-exec-returned-2"></a><span data-ttu-id="c1ac6-147">Řešení potíží: Ant BuildException došlo k chybě: exec vrátí: 2</span><span class="sxs-lookup"><span data-stu-id="c1ac6-147">Troubleshooting: An Ant BuildException has occured: exec returned: 2</span></span>

<span data-ttu-id="c1ac6-148">Při prvním pokusu o sestavení CaffeOnSpark, někdy je se dozvíte</span><span class="sxs-lookup"><span data-stu-id="c1ac6-148">When first trying to build CaffeOnSpark, sometimes it will say</span></span>

    failed to execute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

<span data-ttu-id="c1ac6-149">Jednoduše vyčistit úložiště kódu pomocí "make čisté" a pak spusťte "Zkontrolujte sestavení" se tento problém vyřešit, tak dlouho, dokud máte správnými závislostmi.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-149">Simply clean the code repository by "make clean" and then run "make build" will solve this issue, as long as you have the correct dependencies.</span></span>

### <a name="troubleshooting-maven-repository-connection-time-out"></a><span data-ttu-id="c1ac6-150">Řešení potíží: Časový limit připojení Maven úložiště</span><span class="sxs-lookup"><span data-stu-id="c1ac6-150">Troubleshooting: Maven repository connection time out</span></span>

<span data-ttu-id="c1ac6-151">Někdy maven mi poskytuje vypršení časového limitu Chyba připojení, podobně jako následující:</span><span class="sxs-lookup"><span data-stu-id="c1ac6-151">Sometimes maven gives me the connection time out error, similar to below:</span></span>

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request to {s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

<span data-ttu-id="c1ac6-152">OK bude po čekání pár minut a pak zkuste použít k opětovnému sestavení kódu, proto může být, že Maven nějakým způsobem omezuje provoz z dané IP adresy.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-152">It will be OK after waiting for a few minutes and then just try to rebuild the code, so it might be Maven somehow limits the traffic from a given IP address.</span></span>


### <a name="troubleshooting-test-failure-for-caffe"></a><span data-ttu-id="c1ac6-153">Řešení potíží: Testujte selhání Caffe</span><span class="sxs-lookup"><span data-stu-id="c1ac6-153">Troubleshooting: Test failure for Caffe</span></span>

<span data-ttu-id="c1ac6-154">Pravděpodobně se zobrazí selhání testu při provádění konečné kontrolu CaffeOnSpark, podobně jako s níže.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-154">You probably will see a test failure when doing the final check for CaffeOnSpark, similar with below.</span></span> <span data-ttu-id="c1ac6-155">To je prabably související s kódováním UTF-8, ale by neměla mít vliv na použití Caffe</span><span class="sxs-lookup"><span data-stu-id="c1ac6-155">This is prabably related with UTF-8 encoding, but should not impact the usage of Caffe</span></span>

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-the-required-libraries-to-all-the-worker-nodes"></a><span data-ttu-id="c1ac6-156">Krok 3: Distribuujte požadované knihovny pro všechny uzly pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="c1ac6-156">Step 3: Distribute the required libraries to all the worker nodes</span></span>

<span data-ttu-id="c1ac6-157">Dalším krokem je distribuovat do knihoven (v podstatě knihovny v CaffeOnSpark/caffe veřejný nebo distribuovat/lib/a CaffeOnSpark/caffe distribuční nebo distribuovat/lib /) na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-157">The next step is to distribute the libraries (basically the libraries in CaffeOnSpark/caffe-public/distribute/lib/ and CaffeOnSpark/caffe-distri/distribute/lib/) to all the nodes.</span></span> <span data-ttu-id="c1ac6-158">V kroku 2 umístí jsme tyto knihovny úložiště objektů BLOB a v tomto kroku použijeme akcí skriptů a zkopírujte ho do hlavního uzlů a uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-158">In Step 2, we put those libraries on BLOB storage, and in this step, we will use script actions to copy it to all the head nodes and worker nodes.</span></span>

<span data-ttu-id="c1ac6-159">K tomu jednoduchý spustit akci skriptu jako níže (je třeba přejděte do správného umístění, které jsou specifické pro váš cluster):</span><span class="sxs-lookup"><span data-stu-id="c1ac6-159">To do this, simple run a script action as below (you need to point to the right location specific to your cluster):</span></span>

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

<span data-ttu-id="c1ac6-160">Vzhledem k tomu, že v kroku 2, jsme umístí jej úložiště objektů BLOB, který je dostupný pro všechny uzly, v tomto kroku jsme právě jednoduše zkopírujte jej do všech uzlů.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-160">Because in step 2, we put it on the BLOB storage which is accessible to all the nodes, in this step we just simply copy it to all the nodes.</span></span>

## <a name="step-4-compose-a-caffe-model-and-run-it-distributely"></a><span data-ttu-id="c1ac6-161">Krok 4: Tvoří Caffe modelu a potom ho spusťte distributely</span><span class="sxs-lookup"><span data-stu-id="c1ac6-161">Step 4: Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="c1ac6-162">Po spuštění výše uvedené kroky, Caffe je již nainstalován na headnode a snažíme se pustit do práce.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-162">After running the above steps, Caffe is alreay installed on the headnode and we are good to go.</span></span> <span data-ttu-id="c1ac6-163">Dalším krokem je zápis Caffe modelu.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-163">The next step is to write a Caffe model.</span></span> 

<span data-ttu-id="c1ac6-164">Caffe je pomocí "výrazovou architektury", kde pro skládání modelu, stačí zadat konfigurační soubor, a bez kódování vůbec (ve většině případů).</span><span class="sxs-lookup"><span data-stu-id="c1ac6-164">Caffe is using an "expressive architecture", where for composing a model, you just need to define a configuration file, and without coding at all (in most cases).</span></span> <span data-ttu-id="c1ac6-165">Proto se podíváme existuje.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-165">So let's take a look there.</span></span> 

<span data-ttu-id="c1ac6-166">Model, který jsme bude dnes cvičení je ukázkový model MNIST školení.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-166">The model we will train today is a sample model for MNIST training.</span></span> <span data-ttu-id="c1ac6-167">Databázi MNIST psané číslic má sadu 60 000 příklady školení a testovací sadu 10 000 příklady.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-167">The MNIST database of handwritten digits has a training set of 60,000 examples, and a test set of 10,000 examples.</span></span> <span data-ttu-id="c1ac6-168">Je podmnožinou většímu z NIST k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-168">It is a subset of a larger set available from NIST.</span></span> <span data-ttu-id="c1ac6-169">Číslice byly normalized velikost a zarovnaný na střed v bitové kopii pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-169">The digits have been size-normalized and centered in a fixed-size image.</span></span> <span data-ttu-id="c1ac6-170">CaffeOnSpark má některé skripty ke stažení datovou sadu a převádět je do správném formátu.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-170">CaffeOnSpark has some scripts to download the dataset and convert it into the right format.</span></span>

<span data-ttu-id="c1ac6-171">CaffeOnSpark uvádí některé ukázkové topologie sítě pro MNIST školení.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-171">CaffeOnSpark provides some network topologies example for MNIST training.</span></span> <span data-ttu-id="c1ac6-172">Má dobrý návrh rozdělení síťovou architekturu (topologie sítě) a optimalizace.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-172">It has a nice design of splitting the network architecture (the topology of the network) and optimization.</span></span> <span data-ttu-id="c1ac6-173">V takovém případě existují dva soubory potřebné:</span><span class="sxs-lookup"><span data-stu-id="c1ac6-173">In this case, There are two files required:</span></span> 

<span data-ttu-id="c1ac6-174">soubor "Solver" (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) se používá pro dohled nad optimalizace a generování parametr aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-174">the "Solver" file (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) is used for overseeing the optimization and generating parameter updates.</span></span> <span data-ttu-id="c1ac6-175">Například definuje, zda procesoru nebo GPU se použije, co je výkonnosti, kolik opakování bude, atd. Také definuje, které neuron síťové topologie program využít (což je druhý soubor, který potřebujeme).</span><span class="sxs-lookup"><span data-stu-id="c1ac6-175">For example, it defines whether CPU or GPU will be used, what's the momentum, how many iterations will be, etc. It also defines which neuron network topology should the program use (which is the second file we need).</span></span> <span data-ttu-id="c1ac6-176">Další informace o Solver naleznete [Caffe dokumentaci](http://caffe.berkeleyvision.org/tutorial/solver.html).</span><span class="sxs-lookup"><span data-stu-id="c1ac6-176">For more information about Solver, please refer to [Caffe documentation](http://caffe.berkeleyvision.org/tutorial/solver.html).</span></span>

<span data-ttu-id="c1ac6-177">Vzhledem k tomu, že používáme procesoru než GPU, budeme v tomto příkladu měli změnit poslední řádek pro:</span><span class="sxs-lookup"><span data-stu-id="c1ac6-177">For this example, since we are using CPU rather than GPU, we should change the last line to:</span></span>

    # solver mode: CPU or GPU
    solver_mode: CPU

![Konfigurace Caffe](./media/hdinsight-deep-learning-caffe-spark/Caffe-1.png)

<span data-ttu-id="c1ac6-179">Podle potřeby můžete změnit další řádky.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-179">You can change other lines as needed.</span></span>

<span data-ttu-id="c1ac6-180">Druhý soubor (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) definuje, jak vypadá neuron sítě jako a relevantní vstupní a výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-180">The second file (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) defines how the neuron network looks like, and the relevant input and output file.</span></span> <span data-ttu-id="c1ac6-181">Také je potřeba aktualizovat soubor tak, aby odrážela umístění dat školení.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-181">We also need to update the file to reflect the training data location.</span></span> <span data-ttu-id="c1ac6-182">Změňte následující součástí lenet_memory_train_test.prototxt (je třeba přejděte do správného umístění, které jsou specifické pro váš cluster):</span><span class="sxs-lookup"><span data-stu-id="c1ac6-182">Change the following part in lenet_memory_train_test.prototxt (you need to point to the right location specific to your cluster):</span></span>

- <span data-ttu-id="c1ac6-183">Změňte "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" na "wasb: / / / projekty/machine_learning/image_dataset/mnist_train_lmdb"</span><span class="sxs-lookup"><span data-stu-id="c1ac6-183">change the "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" to "wasb:///projects/machine_learning/image_dataset/mnist_train_lmdb"</span></span>
- <span data-ttu-id="c1ac6-184">Změňte "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" na "wasb: / / / projekty/machine_learning/image_dataset/mnist_test_lmdb"</span><span class="sxs-lookup"><span data-stu-id="c1ac6-184">change "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" to "wasb:///projects/machine_learning/image_dataset/mnist_test_lmdb"</span></span>

![Konfigurace Caffe](./media/hdinsight-deep-learning-caffe-spark/Caffe-2.png)

<span data-ttu-id="c1ac6-186">Další informace o tom, jak definovat sítě, zkontrolujte, zda [Caffe dokumentace pro datovou sadu MNIST](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span><span class="sxs-lookup"><span data-stu-id="c1ac6-186">For more information on how to define the network, please check the [Caffe documentation on MNIST dataset](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span></span>

<span data-ttu-id="c1ac6-187">Pro účely tomto blogu používáme pouze tomto jednoduchém příkladu MNIST.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-187">For the purpose of this blog, we just use this simple MNIST example.</span></span> <span data-ttu-id="c1ac6-188">Z hlavního uzlu by měl spustit příkaz níže:</span><span class="sxs-lookup"><span data-stu-id="c1ac6-188">You should run the command below from the head node:</span></span>

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

<span data-ttu-id="c1ac6-189">V podstatě se distribuuje požadované soubory (lenet_memory_solver.prototxt a lenet_memory_train_test.prototxt) pro každý kontejner YARN a také nastavit relevantní CESTU jednotlivých ovladačů nebo vykonavatele Spark na LD_LIBRARY_PATH, která je definována v předchozím fragmentu kódu a odkazuje na umístění, které má CaffeOnSpark knihovny.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-189">Basically it distributes the required files (lenet_memory_solver.prototxt and lenet_memory_train_test.prototxt) to each YARN container, and also set the relevant PATH of each Spark driver/executor to LD_LIBRARY_PATH, which is defined in the previous code snippet and points to the location that has CaffeOnSpark libraries.</span></span> 

## <a name="monitoring-and-troubleshooting"></a><span data-ttu-id="c1ac6-190">Monitorování a řešení potíží</span><span class="sxs-lookup"><span data-stu-id="c1ac6-190">Monitoring and troubleshooting</span></span>

<span data-ttu-id="c1ac6-191">Vzhledem k tomu, že používáme YARN clusteru režim, v takovém případě Spark ovladače budou naplánovány na libovolný kontejneru (a libovolné pracovního uzlu) jenom se zobrazí v konzole něco podobného jako výstup:</span><span class="sxs-lookup"><span data-stu-id="c1ac6-191">Since we are using YARN cluster mode, in which case the Spark driver will be scheduled to an arbitrary container (and an arbitrary worker node) you should only see in the console outputting something like:</span></span>

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

<span data-ttu-id="c1ac6-192">Pokud chcete vědět, co se stalo, obvykle potřebujete získat Spark ovladače protokol, který obsahuje další informace.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-192">If you want to know what happened, you usually need to get the Spark driver's log, which has more information.</span></span> <span data-ttu-id="c1ac6-193">V takovém případě budete muset přejít do rozhraní YARN najít relevantní protokoly YARN.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-193">In this case, you need to go to the YARN UI to find the relevant YARN logs.</span></span> <span data-ttu-id="c1ac6-194">Můžete získat rozhraní YARN tuto adresu URL:</span><span class="sxs-lookup"><span data-stu-id="c1ac6-194">You can get the YARN UI by this URL:</span></span> 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN UŽIVATELSKÉHO ROZHRANÍ](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-1.png)

<span data-ttu-id="c1ac6-196">Podívejte se na tom, kolik prostředky se přidělují pro tuto konkrétní aplikaci může trvat.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-196">You can take a look at how many resources are allocated for this particular application.</span></span> <span data-ttu-id="c1ac6-197">Můžete kliknout na odkaz "Scheduler" a pak se zobrazí, že pro tuto aplikaci, nejsou 9 kontejnery systémem.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-197">You can click the "Scheduler" link, and then you will see that for this application, there are 9 containers running.</span></span> <span data-ttu-id="c1ac6-198">Můžeme požádat YARN zajistit 8 vykonavatelů a jiný kontejner je pro proces ovladačů.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-198">We ask YARN to provide 8 executors, and another container is for driver process.</span></span> 

![YARN plánovače](./media/hdinsight-deep-learning-caffe-spark/YARN-Scheduler.png)

<span data-ttu-id="c1ac6-200">Můžete chtít Zkontrolujte protokoly ovladače nebo kontejneru protokoly, pokud tam jsou chyby.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-200">You may want to check the  driver logs or container logs if there are failures.</span></span> <span data-ttu-id="c1ac6-201">Pro protokoly ovladačů můžete kliknutím na ID aplikace v uživatelském rozhraní YARN a pak klikněte na tlačítko "Protokoly".</span><span class="sxs-lookup"><span data-stu-id="c1ac6-201">For driver logs, you can click the application ID in YARN UI, then click the "Logs" button.</span></span> <span data-ttu-id="c1ac6-202">Ovladač protokolu se zapisují do stderr.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-202">The driver logs are written into stderr.</span></span>

![UŽIVATELSKÉ ROZHRANÍ YARN 2](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-2.png)

<span data-ttu-id="c1ac6-204">Například může se zobrazit některé chyby níže z protokolů ovladačů, což značí, že přidělíte příliš mnoho vykonavatelů.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-204">For example, you might see some of the error below from the driver logs, indicating you allocate too many executors.</span></span>

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

<span data-ttu-id="c1ac6-205">V některých případech problému může dojít v vykonavatelů spíše než ovladače.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-205">Sometimes, the issue can happen in executors rather than drivers.</span></span> <span data-ttu-id="c1ac6-206">V takovém případě budete muset v protokolech kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-206">In this case, you need to check the container logs.</span></span> <span data-ttu-id="c1ac6-207">Můžete vždy získat protokoly kontejner a potom získat kontejneru se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-207">You can always get the container logs, and then get the failed container.</span></span> <span data-ttu-id="c1ac6-208">Tato chyba může odpovídat například při spuštění Caffe.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-208">For example, you might meet this failure when running Caffe.</span></span>

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

<span data-ttu-id="c1ac6-209">V takovém případě budete muset získat ID se nezdařilo kontejneru (v případě výše je container_1485916338528_0008_05_000005).</span><span class="sxs-lookup"><span data-stu-id="c1ac6-209">In this case, you need to get the failed container ID (in the above case, it is container_1485916338528_0008_05_000005).</span></span> <span data-ttu-id="c1ac6-210">Pak budete muset spustit</span><span class="sxs-lookup"><span data-stu-id="c1ac6-210">Then you need to run</span></span> 

    yarn logs -containerId container_1485916338528_0008_03_000005

<span data-ttu-id="c1ac6-211">z headnode.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-211">from the headnode.</span></span> <span data-ttu-id="c1ac6-212">Po zkontrolování chyby kontejneru, je příčinou pomocí režimu grafický procesor (kde měli použít procesoru režim místo) v lenet_memory_solver.prototxt.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-212">After checking container failure, it is caused by using GPU mode (where you should use CPU mode instead) in lenet_memory_solver.prototxt.</span></span>

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written to STDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a><span data-ttu-id="c1ac6-213">Získávání výsledků</span><span class="sxs-lookup"><span data-stu-id="c1ac6-213">Getting results</span></span>

<span data-ttu-id="c1ac6-214">Vzhledem k tomu, že jsme se přidělování 8 vykonavatelů a síťové topologie je jednoduché, má pouze trvat zhruba 30 minut spustit výsledek.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-214">Since we are allocating 8 executors, and the network topology is simple, it should only take around 30 minutes to run the result.</span></span> <span data-ttu-id="c1ac6-215">Z příkazového řádku, uvidíte jsme put modelu wasb:///mnist.model a put výsledky do složky s názvem wasb: / / / mnist_features_result.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-215">From the command line, you can see that we put the model to wasb:///mnist.model, and put the results to a folder named wasb:///mnist_features_result.</span></span>

<span data-ttu-id="c1ac6-216">Výsledky můžete získat spuštěním</span><span class="sxs-lookup"><span data-stu-id="c1ac6-216">You can get the results by running</span></span>

    hadoop fs -cat hdfs:///mnist_features_result/*

<span data-ttu-id="c1ac6-217">a výsledek bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="c1ac6-217">and the result looks like:</span></span>

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

<span data-ttu-id="c1ac6-218">SampleID reprezentuje ID v datové sadě MNIST a popisek je číslo, které identifikuje modelu.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-218">The SampleID represents the ID in the MNIST dataset, and the label is the number that the model identifies.</span></span>


## <a name="conclusion"></a><span data-ttu-id="c1ac6-219">Závěr</span><span class="sxs-lookup"><span data-stu-id="c1ac6-219">Conclusion</span></span>

<span data-ttu-id="c1ac6-220">V této dokumentaci jste se pokusili nainstalovat CaffeOnSpark se spouštěním jednoduchý příklad.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-220">In this documentation, you have tried to install CaffeOnSpark with running a simple example.</span></span> <span data-ttu-id="c1ac6-221">HDInsight je úplná spravované cloudové platformy distribuovaná výpočetní a je nejlepší místo pro spouštění strojového učení a úlohy pokročilou analýzu na velké datové sady, a pro distribuované hloubkové learning, můžete použít Caffe na HDInsight Spark k provádění hloubkové learning úloh.</span><span class="sxs-lookup"><span data-stu-id="c1ac6-221">HDInsight is a full managed cloud distributed compute platform, and is the best place for running machine learning and advanced analytics workloads on large data set, and for distributed deep learning, you can use Caffe on HDInsight Spark to perform deep learning tasks.</span></span>


## <span data-ttu-id="c1ac6-222"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="c1ac6-222"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="c1ac6-223">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="c1ac6-223">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="c1ac6-224">Scénáře</span><span class="sxs-lookup"><span data-stu-id="c1ac6-224">Scenarios</span></span>
* [<span data-ttu-id="c1ac6-225">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="c1ac6-225">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="c1ac6-226">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="c1ac6-226">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a><span data-ttu-id="c1ac6-227">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="c1ac6-227">Manage resources</span></span>
* [<span data-ttu-id="c1ac6-228">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="c1ac6-228">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)

