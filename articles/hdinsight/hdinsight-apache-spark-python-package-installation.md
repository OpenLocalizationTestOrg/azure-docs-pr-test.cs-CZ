---
title: "Akce aaaScript – balíčky nainstalovat Python s Jupyter v Azure HDInsight | Microsoft Docs"
description: "Podrobný návod, jak toouse skript akce tooconfigure poznámkové bloky Jupyter k dispozici s HDInsight Spark clusterů toouse python externí balíčky."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21978b71-eb53-480b-a3d1-c5d428a7eb5b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 54db79e67995dee7ca00abff979f7d74ae5ab9cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-script-action-tooinstall-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="431ff-103">Použijte externí balíčky Python akce skriptu tooinstall pro poznámkové bloky Jupyter v clusterech Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="431ff-103">Use Script Action tooinstall external Python packages for Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="431ff-104">Pomocí buňky magic</span><span class="sxs-lookup"><span data-stu-id="431ff-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="431ff-105">Pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="431ff-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="431ff-106">Zjistěte, jak toouse akcí skriptů tooconfigure cluster Apache Spark v HDInsight (Linux) toouse externí, komunity podílí **python** balíčky, které nejsou součástí clusteru hello se na pole.</span><span class="sxs-lookup"><span data-stu-id="431ff-106">Learn how toouse Script Actions tooconfigure an Apache Spark cluster on HDInsight (Linux) toouse external, community-contributed **python** packages that are not included out-of-the-box in hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="431ff-107">Můžete také nakonfigurovat poznámkového bloku Jupyter pomocí `%%configure` kouzelná toouse externí balíčky.</span><span class="sxs-lookup"><span data-stu-id="431ff-107">You can also configure a Jupyter notebook by using `%%configure` magic toouse external packages.</span></span> <span data-ttu-id="431ff-108">Pokyny najdete v tématu [použijte externí balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark v HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span><span class="sxs-lookup"><span data-stu-id="431ff-108">For instructions, see [Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span></span>
> 
> 

<span data-ttu-id="431ff-109">Můžete hledat hello [indexu balíčků](https://pypi.python.org/pypi) hello úplný seznam balíčků, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="431ff-109">You can search hello [package index](https://pypi.python.org/pypi) for hello complete list of packages that are available.</span></span> <span data-ttu-id="431ff-110">Seznam dostupných balíčků můžete také získat z jiných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="431ff-110">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="431ff-111">Například můžete nainstalovat balíčky, které jsou k dispozici prostřednictvím [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) nebo [conda vytvoření](https://conda-forge.github.io/feedstocks.html).</span><span class="sxs-lookup"><span data-stu-id="431ff-111">For example, you can install packages made available through [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) or [conda-forge](https://conda-forge.github.io/feedstocks.html).</span></span>

<span data-ttu-id="431ff-112">V tomto článku se dozvíte, jak tooinstall hello [TensorFlow](https://www.tensorflow.org/) balíček pomocí skriptu Actoin v clusteru a použít ho pomocí poznámkového bloku Jupyter hello.</span><span class="sxs-lookup"><span data-stu-id="431ff-112">In this article, you will learn how tooinstall hello [TensorFlow](https://www.tensorflow.org/) package using Script Actoin on your cluster and use it via hello Jupyter notebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="431ff-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="431ff-113">Prerequisites</span></span>
<span data-ttu-id="431ff-114">Musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="431ff-114">You must have hello following:</span></span>

* <span data-ttu-id="431ff-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="431ff-115">An Azure subscription.</span></span> <span data-ttu-id="431ff-116">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="431ff-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="431ff-117">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="431ff-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="431ff-118">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="431ff-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

   > [!NOTE]
   > <span data-ttu-id="431ff-119">Pokud už cluster Spark na HDInsight Linux nemáte, můžete spustit skript akce při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="431ff-119">If you do not already have a Spark cluster on HDInsight Linux, you can run script actions during cluster creation.</span></span> <span data-ttu-id="431ff-120">Navštivte hello dokumentace na [jak toouse vlastní skript akce](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="431ff-120">Visit hello documentation on [how toouse custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="431ff-121">Použijte externí balíčky s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="431ff-121">Use external packages with Jupyter notebooks</span></span>

1. <span data-ttu-id="431ff-122">Z hello [portálu Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel).</span><span class="sxs-lookup"><span data-stu-id="431ff-122">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="431ff-123">Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="431ff-123">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   

2. <span data-ttu-id="431ff-124">Z okna clusteru Spark hello, klikněte na tlačítko **akcí skriptů** pod **využití**.</span><span class="sxs-lookup"><span data-stu-id="431ff-124">From hello Spark cluster blade, click **Script Actions** under **Usage**.</span></span> <span data-ttu-id="431ff-125">Spuštěním hello vlastní akce, který se instaluje TensorFlow hello head uzlů a uzlů pracovního procesu hello.</span><span class="sxs-lookup"><span data-stu-id="431ff-125">Run hello custom action that installs TensorFlow in hello head nodes and hello worker nodes.</span></span> <span data-ttu-id="431ff-126">skript bash Hello lze odkazovat z: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh návštěvu hello dokumentaci na [jak toouse vlastní skript akce](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="431ff-126">hello bash script can be referenced from: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh Visit hello documentation on [how toouse custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>

   > [!NOTE]
   > <span data-ttu-id="431ff-127">Existují dvě python instalace v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="431ff-127">There are two python installations in hello cluster.</span></span> <span data-ttu-id="431ff-128">Spark použije hello Anaconda python instalace nacházející se v `/usr/bin/anaconda/bin`.</span><span class="sxs-lookup"><span data-stu-id="431ff-128">Spark will use hello Anaconda python installation located at `/usr/bin/anaconda/bin`.</span></span> <span data-ttu-id="431ff-129">Referenční instalaci ve vaší vlastní akce prostřednictvím `/usr/bin/anaconda/bin/pip` a `/usr/bin/anaconda/bin/conda`.</span><span class="sxs-lookup"><span data-stu-id="431ff-129">Reference that installation in your custom actions via `/usr/bin/anaconda/bin/pip` and `/usr/bin/anaconda/bin/conda`.</span></span>
   > 
   > 

3. <span data-ttu-id="431ff-130">Otevřete Poznámkový blok PySpark Jupyter</span><span class="sxs-lookup"><span data-stu-id="431ff-130">Open a PySpark Jupyter notebook</span></span>

    <span data-ttu-id="431ff-131">![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Vytvoření nového poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="431ff-131">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="431ff-132">Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="431ff-132">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="431ff-133">Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="431ff-133">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="431ff-134">![Zadejte název pro hello notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "zadejte název pro hello Poznámkový blok")</span><span class="sxs-lookup"><span data-stu-id="431ff-134">![Provide a name for hello notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Provide a name for hello notebook")</span></span>

5. <span data-ttu-id="431ff-135">Nyní budete `import tensorflow` a spusťte hello world – ukázka.</span><span class="sxs-lookup"><span data-stu-id="431ff-135">You will now `import tensorflow` and run a hello world example.</span></span> 

    <span data-ttu-id="431ff-136">Toocopy kódu:</span><span class="sxs-lookup"><span data-stu-id="431ff-136">Code toocopy:</span></span>

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    <span data-ttu-id="431ff-137">Hello výsledek bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="431ff-137">hello result will look like this:</span></span>
    
    <span data-ttu-id="431ff-138">![Provádění kódu TensorFlow](./media/hdinsight-apache-spark-python-package-installation/execution.png "TensorFlow spuštění kódu")</span><span class="sxs-lookup"><span data-stu-id="431ff-138">![TensorFlow code execution](./media/hdinsight-apache-spark-python-package-installation/execution.png "Execute TensorFlow code")</span></span>



## <span data-ttu-id="431ff-139"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="431ff-139"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="431ff-140">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="431ff-140">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="431ff-141">Scénáře</span><span class="sxs-lookup"><span data-stu-id="431ff-141">Scenarios</span></span>
* [<span data-ttu-id="431ff-142">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="431ff-142">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="431ff-143">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="431ff-143">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="431ff-144">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="431ff-144">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="431ff-145">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="431ff-145">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="431ff-146">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="431ff-146">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="431ff-147">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="431ff-147">Create and run applications</span></span>
* [<span data-ttu-id="431ff-148">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="431ff-148">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="431ff-149">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="431ff-149">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="431ff-150">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="431ff-150">Tools and extensions</span></span>
* [<span data-ttu-id="431ff-151">Použijte externí balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="431ff-151">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="431ff-152">Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="431ff-152">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="431ff-153">Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace</span><span class="sxs-lookup"><span data-stu-id="431ff-153">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="431ff-154">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="431ff-154">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="431ff-155">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="431ff-155">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="431ff-156">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="431ff-156">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="431ff-157">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="431ff-157">Manage resources</span></span>
* [<span data-ttu-id="431ff-158">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="431ff-158">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="431ff-159">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="431ff-159">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
