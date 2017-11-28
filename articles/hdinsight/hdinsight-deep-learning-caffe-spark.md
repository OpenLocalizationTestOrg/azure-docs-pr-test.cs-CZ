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
# <a name="use-caffe-on-azure-hdinsight-spark-for-distributed-deep-learning"></a><span data-ttu-id="c7b7e-103">Použití Caffe v Azure HDInsight Spark pro distribuované hloubkové learning</span><span class="sxs-lookup"><span data-stu-id="c7b7e-103">Use Caffe on Azure HDInsight Spark for distributed deep learning</span></span>


## <a name="introduction"></a><span data-ttu-id="c7b7e-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="c7b7e-104">Introduction</span></span>

<span data-ttu-id="c7b7e-105">Hloubkové learning je ovlivňující všechno zdravotní péče tootransportation toomanufacturing a další.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-105">Deep learning is impacting everything from healthcare tootransportation toomanufacturing, and more.</span></span> <span data-ttu-id="c7b7e-106">Společnosti jsou vypnutí toodeep učení toosolve pevný problémy, jako je [bitové kopie klasifikace](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [rozpoznávání řeči](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html)objektu rozpoznávání a počítač překlad.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-106">Companies are turning toodeep learning toosolve hard problems, like [image classification](http://blogs.microsoft.com/next/2015/12/10/microsoft-researchers-win-imagenet-computer-vision-challenge/), [speech recognition](http://googleresearch.blogspot.jp/2015/08/the-neural-networks-behind-google-voice.html), object recognition, and machine translation.</span></span> 

<span data-ttu-id="c7b7e-107">Existují [mnoha oblíbených rozhraní](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), včetně [kognitivní nástrojů Microsoft](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, atd. Caffe je jedním z hello nejznámější architektury-symbolický (imperativní) neuronové sítě a široce používaných v mnoha oblastech, včetně vize počítače.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-107">There are [many popular frameworks](https://en.wikipedia.org/wiki/Comparison_of_deep_learning_software), including [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/research/product/cognitive-toolkit/), [Tensorflow](https://www.tensorflow.org/), MXNet, Theano, etc. Caffe is one of hello most famous non-symbolic (imperative) neural network frameworks, and widely used in many areas including computer vision.</span></span> <span data-ttu-id="c7b7e-108">Kromě toho [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) kombinuje Caffe s Apache Spark, v takovém případě hloubky učení lze snadno použít na existující Hadoop clusteru spolu s Spark ETL kanály, snižuje složitost systému a latence pro učení začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-108">Furthermore, [CaffeOnSpark](http://yahoohadoop.tumblr.com/post/139916563586/caffeonspark-open-sourced-for-distributed-deep) combines Caffe with Apache Spark, in which case deep learning can be easily used on an existing Hadoop cluster together with Spark ETL pipelines, reducing system complexity and latency for end-to-end learning.</span></span>

<span data-ttu-id="c7b7e-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) je hello jen nabídka plně spravované cloudové Hadoop, která poskytuje optimalizované s otevřeným zdrojem analytické clustery Spark, Hive, MapReduce, HBase, Storm, Kafka a zajišťoval SLA 99,9 % R Server.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-109">[HDInsight](https://azure.microsoft.com/en-us/services/hdinsight/) is hello only fully-managed cloud Hadoop offering that provides optimized open source analytic clusters for Spark, Hive, MapReduce, HBase, Storm, Kafka, and R Server backed by a 99.9% SLA.</span></span> <span data-ttu-id="c7b7e-110">Každou z těchto technologií pro velké objemy dat, stejně jako aplikace nezávislých výrobců softwaru, je možné jednoduše nasadit jako spravovaný cluster se zabezpečením a monitorováním na podnikové úrovni.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-110">Each of these big data technologies and ISV applications are easily deployable as managed clusters with enterprise-level security and monitoring.</span></span>

<span data-ttu-id="c7b7e-111">Někteří uživatelé jsou zprávu s požadavkem o toouse hloubkové učení v HDInsight, což je PaaS Hadoop produktu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-111">Some users are asking us about how toouse deep learning on HDInsight, which is Microsoft's PaaS Hadoop product.</span></span> <span data-ttu-id="c7b7e-112">Další tooshare jsme bude mít v hello budoucí, ale ještě dnes chceme toosummarize technické blog o toouse Caffe na HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-112">We will have more tooshare in hello future, but today we want toosummarize a technical blog on how toouse Caffe on HDInsight Spark.</span></span>

<span data-ttu-id="c7b7e-113">Pokud jste nainstalovali Caffe před, si všimnete, že instalace toto rozhraní je chvíli náročné.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-113">If you have installed Caffe before, you will notice that installing this framework is a little bit challenging.</span></span> <span data-ttu-id="c7b7e-114">V tomto blogu nám nejdřív ilustruje způsob tooinstall [Caffe na Spark](https://github.com/yahoo/CaffeOnSpark) pro cluster služby HDInsight, pak použít hello předdefinované MNIST ukázku toodemostrate jak toouse distribuované hloubkové Learning pomocí HDInsight Spark v procesorech.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-114">In this blog, we will first illustrate how tooinstall [Caffe on Spark](https://github.com/yahoo/CaffeOnSpark) for an HDInsight cluster, then use hello built-in MNIST demo toodemostrate how toouse Distributed Deep Learning using HDInsight Spark on CPUs.</span></span>

<span data-ttu-id="c7b7e-115">Existují čtyři hlavní kroky tooget ho fungovat v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-115">There are four major steps tooget it work on HDInsight.</span></span>

1. <span data-ttu-id="c7b7e-116">Nainstalujte hello požadované závislosti na všechny uzly hello</span><span class="sxs-lookup"><span data-stu-id="c7b7e-116">Install hello required dependencies on all hello nodes</span></span>
2. <span data-ttu-id="c7b7e-117">Sestavení Caffe na Spark pro HDInsight hello hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="c7b7e-117">Build Caffe on Spark for HDInsight on hello head node</span></span>
3. <span data-ttu-id="c7b7e-118">Distribuovat hello požadované knihovny tooall hello pracovní uzly</span><span class="sxs-lookup"><span data-stu-id="c7b7e-118">Distribute hello required libraries tooall hello worker nodes</span></span>
4. <span data-ttu-id="c7b7e-119">Napište Caffe modelu a potom ho spusťte distributely</span><span class="sxs-lookup"><span data-stu-id="c7b7e-119">Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="c7b7e-120">Vzhledem k tomu, že HDInsight je řešení PaaS, nabízí skvělý funkce - proto je poměrně snadné tooperform některé úlohy.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-120">Since HDInsight is a PaaS solution, it offers great platform features - so it is quite easy tooperform some tasks.</span></span> <span data-ttu-id="c7b7e-121">Jedna z funkcí hello, které výraznou používáme v tomto příspěvku na blogu nazývá [akce skriptu](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), pomocí kterého můžete spustit příkazy prostředí toocustomize uzly clusteru (hlavního uzlu, pracovního uzlu nebo hraniční uzel).</span><span class="sxs-lookup"><span data-stu-id="c7b7e-121">One of hello features that we heavily use in this blog post is called [Script Action](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), with which you can execute shell commands toocustomize cluster nodes (head node, worker node, or edge node).</span></span>

## <a name="step-1--install-hello-required-dependencies-on-all-hello-nodes"></a><span data-ttu-id="c7b7e-122">Krok 1: Instalace hello požadované závislosti na všech uzlech hello</span><span class="sxs-lookup"><span data-stu-id="c7b7e-122">Step 1:  Install hello required dependencies on all hello nodes</span></span>

<span data-ttu-id="c7b7e-123">tooget spuštěna, potřebujeme tooinstall hello závislosti, které potřebujeme.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-123">tooget started, we need tooinstall hello dependencies we need.</span></span> <span data-ttu-id="c7b7e-124">Hello Caffe lokality a [CaffeOnSpark lokality](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) nabízí některé wiki velmi užitečná pro instalování hello závislostí pro Spark na YARN režimu (což je hello režim pro HDInsight Spark), ale potřebujeme pár Další závislosti pro tooadd pro HDInsight platformu.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-124">hello Caffe site and [CaffeOnSpark site](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn) offers some very useful wiki for installing hello dependencies for Spark on YARN mode (which is hello mode for HDInsight Spark), but we need tooadd a few more dependencies for HDInsight platform.</span></span> <span data-ttu-id="c7b7e-125">Jsme se použití akce skriptu hello jak je uvedeno níže a spusťte ho na všechny hello head uzlů a uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-125">We will use hello script action as below and run it on all hello head nodes and worker nodes.</span></span> <span data-ttu-id="c7b7e-126">Tato akce skriptu bude trvat asi 20 minut, jak tyto závislosti také závisí na jiné balíčky.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-126">This script action will take about 20 minutes, as those dependencies also depend on other packages.</span></span> <span data-ttu-id="c7b7e-127">By ji umístit do některé umístění, které je přístupné tooyour clusteru HDInsight, jako je například umístění GitHub nebo hello výchozí účet úložiště objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-127">You should put it in some location that is accessible tooyour HDInsight cluster, such as a GitHub location or hello default BLOB storage account.</span></span>

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


<span data-ttu-id="c7b7e-128">Existují dva kroky v akce skriptu hello výše.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-128">There are two steps in hello script action above.</span></span> <span data-ttu-id="c7b7e-129">prvním krokem Hello je tooinstall všechny hello požadované knihovny.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-129">hello first step is tooinstall all hello required libraries.</span></span> <span data-ttu-id="c7b7e-130">Tyto knihovny zahrnují hello potřebné knihovny pro kompilaci Caffe (například gflags, glog) i s Caffe (například numpy).</span><span class="sxs-lookup"><span data-stu-id="c7b7e-130">Those libraries include hello necessary libraries for both compiling Caffe(such as gflags, glog) and running Caffe (such as numpy).</span></span> <span data-ttu-id="c7b7e-131">Libatlas se používá pro optimalizaci procesoru, ale můžete vždy následují hello CaffeOnSpark wiki k instalaci další optimalizace knihovny, například MKL nebo CUDA (pro GPU).</span><span class="sxs-lookup"><span data-stu-id="c7b7e-131">We are using libatlas for CPU optimization, but you can always follow hello CaffeOnSpark wiki on installing other optimization libraries, such as MKL or CUDA (for GPU).</span></span>

<span data-ttu-id="c7b7e-132">Hello druhým krokem je toodownload, kompilace a nainstalovat protobuf 2.5.0 pro Caffe za běhu.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-132">hello second step is toodownload, compile, and install protobuf 2.5.0 for Caffe during runtime.</span></span> <span data-ttu-id="c7b7e-133">Protobuf 2.5.0 [je vyžadován](https://github.com/yahoo/CaffeOnSpark/issues/87), ale tato verze není k dispozici jako balíček na Ubuntu 16, takže potřebujeme toocompile z hello zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-133">Protobuf 2.5.0 [is required](https://github.com/yahoo/CaffeOnSpark/issues/87), however this version is not available as a package on Ubuntu 16, so we need toocompile it from hello source code.</span></span> <span data-ttu-id="c7b7e-134">Na hello Internet návod, jak jsou také několik prostředků toocompile, jako například [to](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span><span class="sxs-lookup"><span data-stu-id="c7b7e-134">There are also a few resources on hello Internet on how toocompile it, such as [this](http://jugnu-life.blogspot.com/2013/09/install-protobuf-25-on-ubuntu.html)</span></span>

<span data-ttu-id="c7b7e-135">Začněte toosimply, jenom pro můžete spouštět tuto akci skriptu vašeho clusteru tooall hello pracovní uzly uzly a head (pro HDInsight 3.5).</span><span class="sxs-lookup"><span data-stu-id="c7b7e-135">toosimply get started, you can just run this script action against your cluster tooall hello worker nodes and head nodes (for HDInsight 3.5).</span></span> <span data-ttu-id="c7b7e-136">Můžete buď spustit hello akcí skriptů pro cluster s podporou spuštěné, nebo hello skriptových akcí můžete taky spustit během doby zřízení clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-136">You can either run hello script actions for a running cluster, or you can also run hello script actions during hello cluster provision time.</span></span> <span data-ttu-id="c7b7e-137">Další informace o hello akcí skriptů naleznete v dokumentaci hello [sem](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span><span class="sxs-lookup"><span data-stu-id="c7b7e-137">For more details on hello script actions, please see hello documentation [here](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#view-history-promote-and-demote-script-actions)</span></span>

![Skript akce tooInstall závislosti](./media/hdinsight-deep-learning-caffe-spark/Script-Action-1.png)


## <a name="step-2-build-caffe-on-spark-for-hdinsight-on-hello-head-node"></a><span data-ttu-id="c7b7e-139">Krok 2: Vytvoření Caffe na Spark pro HDInsight hello hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="c7b7e-139">Step 2: Build Caffe on Spark for HDInsight on hello head node</span></span>

<span data-ttu-id="c7b7e-140">druhý krok Hello je toobuild Caffe na hello headnode a pak distribuovat hello zkompilovat knihovny tooall hello pracovním uzlům.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-140">hello second step is toobuild Caffe on hello headnode, and then distribute hello compiled libraries tooall hello worker nodes.</span></span> <span data-ttu-id="c7b7e-141">V tomto kroku je nutné příliš[ssh do vaší headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), jednoduše podle hello [proces sestavení CaffeOnSpark](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), a dále je skript hello toobuild CaffeOnSpark můžete použít s několika dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-141">In this step, you will need too[ssh into your headnode](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix), then simply follow hello [CaffeOnSpark build process](https://github.com/yahoo/CaffeOnSpark/wiki/GetStarted_yarn), and below is hello script you can use toobuild CaffeOnSpark with a few additional steps.</span></span> 

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

<span data-ttu-id="c7b7e-142">Může být nutné toodo víc než jaké hello dokumentaci CaffeOnSpark uvádí.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-142">You may need toodo more than what hello documentation of CaffeOnSpark says.</span></span> <span data-ttu-id="c7b7e-143">Hello změny jsou:</span><span class="sxs-lookup"><span data-stu-id="c7b7e-143">hello changes are:</span></span>
- <span data-ttu-id="c7b7e-144">Změňte pouze tooCPU a použít libatlas pro tento konkrétní účel.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-144">Change tooCPU only and use libatlas for this particular purpose.</span></span>
- <span data-ttu-id="c7b7e-145">Vložení hello datové sady toohello úložiště objektů BLOB, což je sdílené umístění, které je přístupné tooall uzlů pracovního procesu pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-145">Put hello datasets toohello BLOB storage, which is a shared location that is accessible tooall worker nodes for later use.</span></span>
- <span data-ttu-id="c7b7e-146">Vložení hello zkompilovat Caffe knihovny tooBLOB úložiště a později se zkopírujte tyto knihovny tooall hello uzlů pomocí doby další kompilace tooavoid akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-146">Put hello compiled Caffe libraries tooBLOB storage, and later you will copy those libraries tooall hello nodes using script actions tooavoid additional compilation time.</span></span>


### <a name="troubleshooting-an-ant-buildexception-has-occured-exec-returned-2"></a><span data-ttu-id="c7b7e-147">Řešení potíží: Ant BuildException došlo k chybě: exec vrátí: 2</span><span class="sxs-lookup"><span data-stu-id="c7b7e-147">Troubleshooting: An Ant BuildException has occured: exec returned: 2</span></span>

<span data-ttu-id="c7b7e-148">Při prvním pokusu o toobuild CaffeOnSpark, někdy je se dozvíte</span><span class="sxs-lookup"><span data-stu-id="c7b7e-148">When first trying toobuild CaffeOnSpark, sometimes it will say</span></span>

    failed tooexecute goal org.apache.maven.plugins:maven-antrun-plugin:1.7:run (proto) on project caffe-distri: An Ant BuildException has occured: exec returned: 2

<span data-ttu-id="c7b7e-149">Jednoduše čistou hello úložiště kódu pomocí "Zkontrolujte čisté" a pak spusťte "Zkontrolujte sestavení" se tento problém vyřešit, tak dlouho, dokud máte hello správnými závislostmi.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-149">Simply clean hello code repository by "make clean" and then run "make build" will solve this issue, as long as you have hello correct dependencies.</span></span>

### <a name="troubleshooting-maven-repository-connection-time-out"></a><span data-ttu-id="c7b7e-150">Řešení potíží: Časový limit připojení Maven úložiště</span><span class="sxs-lookup"><span data-stu-id="c7b7e-150">Troubleshooting: Maven repository connection time out</span></span>

<span data-ttu-id="c7b7e-151">Někdy maven mi poskytuje chyba vypršení časového limitu hello připojení, podobně jako toobelow:</span><span class="sxs-lookup"><span data-stu-id="c7b7e-151">Sometimes maven gives me hello connection time out error, similar toobelow:</span></span>

    Retry:
    [INFO] Downloading: https://repo.maven.apache.org/maven2/com/twitter/chill_2.11/0.8.0/chill_2.11-0.8.0.jar
    Feb 01, 2017 5:14:49 AM org.apache.maven.wagon.providers.http.httpclient.impl.execchain.RetryExec execute
    INFO: I/O exception (java.net.SocketException) caught when processing request too{s}->https://repo.maven.apache.org:443: Connection timed out (Read failed)

<span data-ttu-id="c7b7e-152">Bude OK po čekání pár minut a opakujte právě toorebuild hello kódu, takže může být Maven nějakým způsobem omezení hello provoz z dané IP adresy.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-152">It will be OK after waiting for a few minutes and then just try toorebuild hello code, so it might be Maven somehow limits hello traffic from a given IP address.</span></span>


### <a name="troubleshooting-test-failure-for-caffe"></a><span data-ttu-id="c7b7e-153">Řešení potíží: Testujte selhání Caffe</span><span class="sxs-lookup"><span data-stu-id="c7b7e-153">Troubleshooting: Test failure for Caffe</span></span>

<span data-ttu-id="c7b7e-154">Pravděpodobně se zobrazí selhání testu při provádění hello poslední kontrola CaffeOnSpark, podobně jako s níže.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-154">You probably will see a test failure when doing hello final check for CaffeOnSpark, similar with below.</span></span> <span data-ttu-id="c7b7e-155">To je prabably související s kódováním UTF-8, ale by neměla mít vliv na použití hello Caffe</span><span class="sxs-lookup"><span data-stu-id="c7b7e-155">This is prabably related with UTF-8 encoding, but should not impact hello usage of Caffe</span></span>

    Run completed in 32 seconds, 78 milliseconds.
    Total number of tests run: 7
    Suites: completed 5, aborted 0
    Tests: succeeded 6, failed 1, canceled 0, ignored 0, pending 0
    *** 1 TEST FAILED ***

## <a name="step-3-distribute-hello-required-libraries-tooall-hello-worker-nodes"></a><span data-ttu-id="c7b7e-156">Krok 3: Distribuujte hello požadované knihovny tooall hello pracovní uzly</span><span class="sxs-lookup"><span data-stu-id="c7b7e-156">Step 3: Distribute hello required libraries tooall hello worker nodes</span></span>

<span data-ttu-id="c7b7e-157">dalším krokem Hello je toodistribute hello knihovny (v podstatě hello knihovny v CaffeOnSpark/caffe veřejný nebo distribuovat/lib/a CaffeOnSpark/caffe distribuční nebo distribuovat/lib /) tooall hello uzly.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-157">hello next step is toodistribute hello libraries (basically hello libraries in CaffeOnSpark/caffe-public/distribute/lib/ and CaffeOnSpark/caffe-distri/distribute/lib/) tooall hello nodes.</span></span> <span data-ttu-id="c7b7e-158">V kroku 2, můžeme tyto knihovny na umístit úložiště objektů BLOB a v tomto kroku použijeme skript akce toocopy ho tooall hello head uzlů a uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-158">In Step 2, we put those libraries on BLOB storage, and in this step, we will use script actions toocopy it tooall hello head nodes and worker nodes.</span></span>

<span data-ttu-id="c7b7e-159">toodo se jednoduché spustit akci skriptu jako níže (je třeba toopoint toohello správného umístění konkrétní tooyour clusteru):</span><span class="sxs-lookup"><span data-stu-id="c7b7e-159">toodo this, simple run a script action as below (you need toopoint toohello right location specific tooyour cluster):</span></span>

    #!/bin/bash
    hadoop fs -get wasb:///CaffeOnSpark /home/changetoyourusername/

<span data-ttu-id="c7b7e-160">Vzhledem k tomu, že v kroku 2, jsme umístí jej hello úložiště objektů BLOB, který je přístupný tooall hello uzly, v tomto kroku jsme právě jednoduše zkopírujte jej tooall hello uzly.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-160">Because in step 2, we put it on hello BLOB storage which is accessible tooall hello nodes, in this step we just simply copy it tooall hello nodes.</span></span>

## <a name="step-4-compose-a-caffe-model-and-run-it-distributely"></a><span data-ttu-id="c7b7e-161">Krok 4: Tvoří Caffe modelu a potom ho spusťte distributely</span><span class="sxs-lookup"><span data-stu-id="c7b7e-161">Step 4: Compose a Caffe model and run it distributely</span></span>

<span data-ttu-id="c7b7e-162">Po spuštění hello výše uvedené kroky, Caffe je již nainstalován na hello headnode a snažíme se dobrý toogo.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-162">After running hello above steps, Caffe is alreay installed on hello headnode and we are good toogo.</span></span> <span data-ttu-id="c7b7e-163">dalším krokem Hello je toowrite Caffe modelu.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-163">hello next step is toowrite a Caffe model.</span></span> 

<span data-ttu-id="c7b7e-164">Caffe používá "výrazovou architekturu", kde skládání modelu, můžete jednoduše nutné toodefine konfigurační soubor a bez kódování vůbec (ve většině případů).</span><span class="sxs-lookup"><span data-stu-id="c7b7e-164">Caffe is using an "expressive architecture", where for composing a model, you just need toodefine a configuration file, and without coding at all (in most cases).</span></span> <span data-ttu-id="c7b7e-165">Proto se podíváme existuje.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-165">So let's take a look there.</span></span> 

<span data-ttu-id="c7b7e-166">Hello model, který jsme bude dnes cvičení je ukázkový model MNIST školení.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-166">hello model we will train today is a sample model for MNIST training.</span></span> <span data-ttu-id="c7b7e-167">databáze MNIST Hello psané číslic má sadu 60 000 příklady školení a testovací sadu 10 000 příklady.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-167">hello MNIST database of handwritten digits has a training set of 60,000 examples, and a test set of 10,000 examples.</span></span> <span data-ttu-id="c7b7e-168">Je podmnožinou většímu z NIST k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-168">It is a subset of a larger set available from NIST.</span></span> <span data-ttu-id="c7b7e-169">Hello číslic byly normalized velikost a zarovnaný na střed v bitové kopii pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-169">hello digits have been size-normalized and centered in a fixed-size image.</span></span> <span data-ttu-id="c7b7e-170">CaffeOnSpark má datovou sadu některé skripty toodownload hello a převádět je do hello správném formátu.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-170">CaffeOnSpark has some scripts toodownload hello dataset and convert it into hello right format.</span></span>

<span data-ttu-id="c7b7e-171">CaffeOnSpark uvádí některé ukázkové topologie sítě pro MNIST školení.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-171">CaffeOnSpark provides some network topologies example for MNIST training.</span></span> <span data-ttu-id="c7b7e-172">Má dobrý návrh rozdělení hello síťovou architekturu (hello topologie sítě hello) a optimalizace.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-172">It has a nice design of splitting hello network architecture (hello topology of hello network) and optimization.</span></span> <span data-ttu-id="c7b7e-173">V takovém případě existují dva soubory potřebné:</span><span class="sxs-lookup"><span data-stu-id="c7b7e-173">In this case, There are two files required:</span></span> 

<span data-ttu-id="c7b7e-174">soubor "Solver" Hello (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) se používá pro dohled nad hello optimalizace a generování parametr aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-174">hello "Solver" file (${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt) is used for overseeing hello optimization and generating parameter updates.</span></span> <span data-ttu-id="c7b7e-175">Například definuje, zda procesoru nebo GPU se použije, co je hello výkonnosti, kolik opakování bude, atd. Také definuje, které neuron topologie sítě by měl hello program použití (což je hello druhý soubor, potřebujeme).</span><span class="sxs-lookup"><span data-stu-id="c7b7e-175">For example, it defines whether CPU or GPU will be used, what's hello momentum, how many iterations will be, etc. It also defines which neuron network topology should hello program use (which is hello second file we need).</span></span> <span data-ttu-id="c7b7e-176">Další informace o Solver naleznete příliš[Caffe dokumentaci](http://caffe.berkeleyvision.org/tutorial/solver.html).</span><span class="sxs-lookup"><span data-stu-id="c7b7e-176">For more information about Solver, please refer too[Caffe documentation](http://caffe.berkeleyvision.org/tutorial/solver.html).</span></span>

<span data-ttu-id="c7b7e-177">V tomto příkladu jsme měli vzhledem k tomu, že používáme procesoru než GPU, změnit hello poslední řádek:</span><span class="sxs-lookup"><span data-stu-id="c7b7e-177">For this example, since we are using CPU rather than GPU, we should change hello last line to:</span></span>

    # solver mode: CPU or GPU
    solver_mode: CPU

![Konfigurace Caffe](./media/hdinsight-deep-learning-caffe-spark/Caffe-1.png)

<span data-ttu-id="c7b7e-179">Podle potřeby můžete změnit další řádky.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-179">You can change other lines as needed.</span></span>

<span data-ttu-id="c7b7e-180">druhý soubor Hello (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) definuje, jak vypadá hello neuron sítě jako a hello relevantní vstupní a výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-180">hello second file (${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt) defines how hello neuron network looks like, and hello relevant input and output file.</span></span> <span data-ttu-id="c7b7e-181">Potřebujeme také umístění dat školení tooreflect tooupdate hello souboru hello.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-181">We also need tooupdate hello file tooreflect hello training data location.</span></span> <span data-ttu-id="c7b7e-182">Změna hello následující součástí lenet_memory_train_test.prototxt (je třeba toopoint toohello správného umístění konkrétní tooyour clusteru):</span><span class="sxs-lookup"><span data-stu-id="c7b7e-182">Change hello following part in lenet_memory_train_test.prototxt (you need toopoint toohello right location specific tooyour cluster):</span></span>

- <span data-ttu-id="c7b7e-183">změnit hello "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" příliš "wasb: / / / projekty/machine_learning/image_dataset/mnist_train_lmdb"</span><span class="sxs-lookup"><span data-stu-id="c7b7e-183">change hello "file:/Users/mridul/bigml/demodl/mnist_train_lmdb" too"wasb:///projects/machine_learning/image_dataset/mnist_train_lmdb"</span></span>
- <span data-ttu-id="c7b7e-184">Změňte "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" příliš "wasb: / / / projekty/machine_learning/image_dataset/mnist_test_lmdb"</span><span class="sxs-lookup"><span data-stu-id="c7b7e-184">change "file:/Users/mridul/bigml/demodl/mnist_test_lmdb/" too"wasb:///projects/machine_learning/image_dataset/mnist_test_lmdb"</span></span>

![Konfigurace Caffe](./media/hdinsight-deep-learning-caffe-spark/Caffe-2.png)

<span data-ttu-id="c7b7e-186">Další informace o tom, jak toodefine hello sítě, zkontrolujte, zda text hello [Caffe dokumentace pro datovou sadu MNIST](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span><span class="sxs-lookup"><span data-stu-id="c7b7e-186">For more information on how toodefine hello network, please check hello [Caffe documentation on MNIST dataset](http://caffe.berkeleyvision.org/gathered/examples/mnist.html)</span></span>

<span data-ttu-id="c7b7e-187">Za účelem hello tomto blogu používáme pouze tomto jednoduchém příkladu MNIST.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-187">For hello purpose of this blog, we just use this simple MNIST example.</span></span> <span data-ttu-id="c7b7e-188">Spusťte příkaz hello níže z hlavního uzlu hello:</span><span class="sxs-lookup"><span data-stu-id="c7b7e-188">You should run hello command below from hello head node:</span></span>

    spark-submit --master yarn --deploy-mode cluster --num-executors 8 --files ${CAFFE_ON_SPARK}/data/lenet_memory_solver.prototxt,${CAFFE_ON_SPARK}/data/lenet_memory_train_test.prototxt --conf spark.driver.extraLibraryPath="${LD_LIBRARY_PATH}" --conf spark.executorEnv.LD_LIBRARY_PATH="${LD_LIBRARY_PATH}" --class com.yahoo.ml.caffe.CaffeOnSpark ${CAFFE_ON_SPARK}/caffe-grid/target/caffe-grid-0.1-SNAPSHOT-jar-with-dependencies.jar -train -features accuracy,loss -label label -conf lenet_memory_solver.prototxt -devices 1 -connection ethernet -model wasb:///mnist.model -output wasb:///mnist_features_result

<span data-ttu-id="c7b7e-189">V podstatě distribuuje hello požadované soubory (lenet_memory_solver.prototxt a lenet_memory_train_test.prototxt) tooeach PŘÍZOVÝ kontejneru a také nastavit hello relevantní CESTU tooLD_LIBRARY_PATH ovladače nebo vykonavatele každý Spark, která je definována v hello Předchozí kód fragment kódu a body toohello umístění, které má CaffeOnSpark knihovny.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-189">Basically it distributes hello required files (lenet_memory_solver.prototxt and lenet_memory_train_test.prototxt) tooeach YARN container, and also set hello relevant PATH of each Spark driver/executor tooLD_LIBRARY_PATH, which is defined in hello previous code snippet and points toohello location that has CaffeOnSpark libraries.</span></span> 

## <a name="monitoring-and-troubleshooting"></a><span data-ttu-id="c7b7e-190">Monitorování a řešení potíží</span><span class="sxs-lookup"><span data-stu-id="c7b7e-190">Monitoring and troubleshooting</span></span>

<span data-ttu-id="c7b7e-191">Vzhledem k tomu, že používáme YARN clusteru režim, v takovém případě bude hello Spark ovladač naplánované tooan libovolný kontejneru (a libovolné pracovního uzlu) můžete byste měli vidět jenom v výstup hello konzoly vytvořeného něco jako:</span><span class="sxs-lookup"><span data-stu-id="c7b7e-191">Since we are using YARN cluster mode, in which case hello Spark driver will be scheduled tooan arbitrary container (and an arbitrary worker node) you should only see in hello console outputting something like:</span></span>

    17/02/01 23:22:16 INFO Client: Application report for application_1485916338528_0015 (state: RUNNING)

<span data-ttu-id="c7b7e-192">Pokud chcete tooknow, co se stalo, je obvykle třeba tooget hello Spark ovladače protokol, který obsahuje další informace.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-192">If you want tooknow what happened, you usually need tooget hello Spark driver's log, which has more information.</span></span> <span data-ttu-id="c7b7e-193">V takovém případě musíte toogo toohello uživatelském rozhraní YARN toofind hello relevantní protokolů YARN.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-193">In this case, you need toogo toohello YARN UI toofind hello relevant YARN logs.</span></span> <span data-ttu-id="c7b7e-194">Můžete získat hello uživatelském rozhraní YARN tuto adresu URL:</span><span class="sxs-lookup"><span data-stu-id="c7b7e-194">You can get hello YARN UI by this URL:</span></span> 

    https://yourclustername.azurehdinsight.net/yarnui
   
![YARN UŽIVATELSKÉHO ROZHRANÍ](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-1.png)

<span data-ttu-id="c7b7e-196">Podívejte se na tom, kolik prostředky se přidělují pro tuto konkrétní aplikaci může trvat.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-196">You can take a look at how many resources are allocated for this particular application.</span></span> <span data-ttu-id="c7b7e-197">Můžete kliknout na odkaz "Scheduler" hello, a pak se zobrazí, že pro tuto aplikaci, nejsou 9 kontejnery systémem.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-197">You can click hello "Scheduler" link, and then you will see that for this application, there are 9 containers running.</span></span> <span data-ttu-id="c7b7e-198">Můžeme požádat YARN tooprovide 8 vykonavatelů a jiný kontejner je pro proces ovladačů.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-198">We ask YARN tooprovide 8 executors, and another container is for driver process.</span></span> 

![YARN plánovače](./media/hdinsight-deep-learning-caffe-spark/YARN-Scheduler.png)

<span data-ttu-id="c7b7e-200">Pokud jsou selhání může být vhodné toocheck hello ovladač protokolu nebo kontejner protokoly.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-200">You may want toocheck hello  driver logs or container logs if there are failures.</span></span> <span data-ttu-id="c7b7e-201">Pro ovladač protokoly můžete kliknutím na ID aplikace hello v uživatelském rozhraní YARN a pak klikněte na tlačítko "Protokoly" hello.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-201">For driver logs, you can click hello application ID in YARN UI, then click hello "Logs" button.</span></span> <span data-ttu-id="c7b7e-202">Hello ovladač protokolu se zapisují do stderr.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-202">hello driver logs are written into stderr.</span></span>

![UŽIVATELSKÉ ROZHRANÍ YARN 2](./media/hdinsight-deep-learning-caffe-spark/YARN-UI-2.png)

<span data-ttu-id="c7b7e-204">Například může zobrazit některé chyby hello níže z protokolů ovladač hello, což značí, že přidělíte příliš mnoho vykonavatelů.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-204">For example, you might see some of hello error below from hello driver logs, indicating you allocate too many executors.</span></span>

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

<span data-ttu-id="c7b7e-205">V některých případech hello problému může dojít v vykonavatelů spíše než ovladače.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-205">Sometimes, hello issue can happen in executors rather than drivers.</span></span> <span data-ttu-id="c7b7e-206">V takovém případě musíte toocheck hello kontejneru protokoly.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-206">In this case, you need toocheck hello container logs.</span></span> <span data-ttu-id="c7b7e-207">Můžete vždy získat hello kontejneru protokoly a potom získat hello kontejneru se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-207">You can always get hello container logs, and then get hello failed container.</span></span> <span data-ttu-id="c7b7e-208">Tato chyba může odpovídat například při spuštění Caffe.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-208">For example, you might meet this failure when running Caffe.</span></span>

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

<span data-ttu-id="c7b7e-209">V takovém případě je třeba ID kontejneru tooget hello se nezdařilo (v hello výše případ, je container_1485916338528_0008_05_000005).</span><span class="sxs-lookup"><span data-stu-id="c7b7e-209">In this case, you need tooget hello failed container ID (in hello above case, it is container_1485916338528_0008_05_000005).</span></span> <span data-ttu-id="c7b7e-210">Budete potřebovat toorun</span><span class="sxs-lookup"><span data-stu-id="c7b7e-210">Then you need toorun</span></span> 

    yarn logs -containerId container_1485916338528_0008_03_000005

<span data-ttu-id="c7b7e-211">z hello headnode.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-211">from hello headnode.</span></span> <span data-ttu-id="c7b7e-212">Po zkontrolování chyby kontejneru, je příčinou pomocí režimu grafický procesor (kde měli použít procesoru režim místo) v lenet_memory_solver.prototxt.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-212">After checking container failure, it is caused by using GPU mode (where you should use CPU mode instead) in lenet_memory_solver.prototxt.</span></span>

    17/02/01 07:10:48 INFO LMDB: Batch size:100
    WARNING: Logging before InitGoogleLogging() is written tooSTDERR
    F0201 07:10:48.309725 11624 common.cpp:79] Cannot use GPU in CPU-only Caffe: check mode.


## <a name="getting-results"></a><span data-ttu-id="c7b7e-213">Získávání výsledků</span><span class="sxs-lookup"><span data-stu-id="c7b7e-213">Getting results</span></span>

<span data-ttu-id="c7b7e-214">Vzhledem k tomu, že jsme se přidělování 8 vykonavatelů a topologie sítě hello je jednoduché, má pouze trvat přibližně 30 minut toorun hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-214">Since we are allocating 8 executors, and hello network topology is simple, it should only take around 30 minutes toorun hello result.</span></span> <span data-ttu-id="c7b7e-215">Z příkazového řádku hello, uvidíte jsme put hello modelu toowasb:///mnist.model a put hello výsledky tooa složku s názvem wasb: / / / mnist_features_result.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-215">From hello command line, you can see that we put hello model toowasb:///mnist.model, and put hello results tooa folder named wasb:///mnist_features_result.</span></span>

<span data-ttu-id="c7b7e-216">Hello výsledky můžete získat spuštěním</span><span class="sxs-lookup"><span data-stu-id="c7b7e-216">You can get hello results by running</span></span>

    hadoop fs -cat hdfs:///mnist_features_result/*

<span data-ttu-id="c7b7e-217">a hello výsledek bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="c7b7e-217">and hello result looks like:</span></span>

    {"SampleID":"00009597","accuracy":[1.0],"loss":[0.028171852],"label":[2.0]}
    {"SampleID":"00009598","accuracy":[1.0],"loss":[0.028171852],"label":[6.0]}
    {"SampleID":"00009599","accuracy":[1.0],"loss":[0.028171852],"label":[1.0]}
    {"SampleID":"00009600","accuracy":[0.97],"loss":[0.0677709],"label":[5.0]}
    {"SampleID":"00009601","accuracy":[0.97],"loss":[0.0677709],"label":[0.0]}
    {"SampleID":"00009602","accuracy":[0.97],"loss":[0.0677709],"label":[1.0]}
    {"SampleID":"00009603","accuracy":[0.97],"loss":[0.0677709],"label":[2.0]}
    {"SampleID":"00009604","accuracy":[0.97],"loss":[0.0677709],"label":[3.0]}
    {"SampleID":"00009605","accuracy":[0.97],"loss":[0.0677709],"label":[4.0]}

<span data-ttu-id="c7b7e-218">Hello SampleID reprezentuje ID hello v datové sadě MNIST hello a popisek hello je, že identifikuje hello číslo, které hello modelu.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-218">hello SampleID represents hello ID in hello MNIST dataset, and hello label is hello number that hello model identifies.</span></span>


## <a name="conclusion"></a><span data-ttu-id="c7b7e-219">Závěr</span><span class="sxs-lookup"><span data-stu-id="c7b7e-219">Conclusion</span></span>

<span data-ttu-id="c7b7e-220">V této dokumentaci pokoušíte tooinstall CaffeOnSpark se spouštěním jednoduchý příklad.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-220">In this documentation, you have tried tooinstall CaffeOnSpark with running a simple example.</span></span> <span data-ttu-id="c7b7e-221">HDInsight je úplná spravované cloudové platformy distribuovaná výpočetní a hello nejlepší místo pro spouštění strojového učení a úlohy pokročilou analýzu na velké sady dat a pro distribuované hloubkové learning, můžete použít Caffe na HDInsight Spark tooperform hloubkové učení úlohy.</span><span class="sxs-lookup"><span data-stu-id="c7b7e-221">HDInsight is a full managed cloud distributed compute platform, and is hello best place for running machine learning and advanced analytics workloads on large data set, and for distributed deep learning, you can use Caffe on HDInsight Spark tooperform deep learning tasks.</span></span>


## <span data-ttu-id="c7b7e-222"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="c7b7e-222"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="c7b7e-223">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="c7b7e-223">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="c7b7e-224">Scénáře</span><span class="sxs-lookup"><span data-stu-id="c7b7e-224">Scenarios</span></span>
* [<span data-ttu-id="c7b7e-225">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="c7b7e-225">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="c7b7e-226">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="c7b7e-226">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

### <a name="manage-resources"></a><span data-ttu-id="c7b7e-227">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="c7b7e-227">Manage resources</span></span>
* [<span data-ttu-id="c7b7e-228">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="c7b7e-228">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)

