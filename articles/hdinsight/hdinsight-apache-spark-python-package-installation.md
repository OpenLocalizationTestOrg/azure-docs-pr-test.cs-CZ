---
title: "Skript akce – instalace Python balíčky s Jupyter v Azure HDInsight | Microsoft Docs"
description: "Podrobné pokyny o tom, jak nakonfigurovat k dispozici poznámkové bloky Jupyter s clustery HDInsight Spark pomocí skriptu akce použití externí python balíčků."
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
ms.openlocfilehash: 20cf384c96d4ff4eaf064c8880ad128d521fb9bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="8f18a-103">Použití akce skriptu k instalaci externích balíčků Python pro poznámkové bloky Jupyter v clusterech Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f18a-103">Use Script Action to install external Python packages for Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8f18a-104">Pomocí buňky magic</span><span class="sxs-lookup"><span data-stu-id="8f18a-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="8f18a-105">Pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="8f18a-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="8f18a-106">Zjistěte, jak pomocí skriptových akcí můžete nakonfigurovat cluster Apache Spark v HDInsight (Linux) používat externí, komunity podílí **python** balíčky, které nejsou zahrnuty v clusteru se na pole.</span><span class="sxs-lookup"><span data-stu-id="8f18a-106">Learn how to use Script Actions to configure an Apache Spark cluster on HDInsight (Linux) to use external, community-contributed **python** packages that are not included out-of-the-box in the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="8f18a-107">Můžete také nakonfigurovat poznámkového bloku Jupyter pomocí `%%configure` magic používat externí balíčky.</span><span class="sxs-lookup"><span data-stu-id="8f18a-107">You can also configure a Jupyter notebook by using `%%configure` magic to use external packages.</span></span> <span data-ttu-id="8f18a-108">Pokyny najdete v tématu [použijte externí balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark v HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span><span class="sxs-lookup"><span data-stu-id="8f18a-108">For instructions, see [Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span></span>
> 
> 

<span data-ttu-id="8f18a-109">Můžete hledat [indexu balíčků](https://pypi.python.org/pypi) úplný seznam balíčků, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8f18a-109">You can search the [package index](https://pypi.python.org/pypi) for the complete list of packages that are available.</span></span> <span data-ttu-id="8f18a-110">Seznam dostupných balíčků můžete také získat z jiných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="8f18a-110">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="8f18a-111">Například můžete nainstalovat balíčky, které jsou k dispozici prostřednictvím [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) nebo [conda vytvoření](https://conda-forge.github.io/feedstocks.html).</span><span class="sxs-lookup"><span data-stu-id="8f18a-111">For example, you can install packages made available through [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) or [conda-forge](https://conda-forge.github.io/feedstocks.html).</span></span>

<span data-ttu-id="8f18a-112">V tomto článku se dozvíte, jak nainstalovat [TensorFlow](https://www.tensorflow.org/) balíček pomocí skriptu Actoin v clusteru a použít ho pomocí poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="8f18a-112">In this article, you will learn how to install the [TensorFlow](https://www.tensorflow.org/) package using Script Actoin on your cluster and use it via the Jupyter notebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f18a-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8f18a-113">Prerequisites</span></span>
<span data-ttu-id="8f18a-114">Musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="8f18a-114">You must have the following:</span></span>

* <span data-ttu-id="8f18a-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="8f18a-115">An Azure subscription.</span></span> <span data-ttu-id="8f18a-116">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="8f18a-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="8f18a-117">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8f18a-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="8f18a-118">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="8f18a-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

   > [!NOTE]
   > <span data-ttu-id="8f18a-119">Pokud už cluster Spark na HDInsight Linux nemáte, můžete spustit skript akce při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="8f18a-119">If you do not already have a Spark cluster on HDInsight Linux, you can run script actions during cluster creation.</span></span> <span data-ttu-id="8f18a-120">Najdete v dokumentaci na [postup použít vlastní skript akce](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="8f18a-120">Visit the documentation on [how to use custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="8f18a-121">Použijte externí balíčky s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="8f18a-121">Use external packages with Jupyter notebooks</span></span>

1. <span data-ttu-id="8f18a-122">Z [Portálu Azure](https://portal.azure.com/) z úvodního panelu klikněte na dlaždici pro váš cluster Spark (pokud je připnutý na úvodní panel).</span><span class="sxs-lookup"><span data-stu-id="8f18a-122">From the [Azure Portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="8f18a-123">Můžete také přejít na cluster pod položkou **Procházet vše** > **Clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="8f18a-123">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>   

2. <span data-ttu-id="8f18a-124">Z okna clusteru Spark klikněte na tlačítko **akcí skriptů** pod **využití**.</span><span class="sxs-lookup"><span data-stu-id="8f18a-124">From the Spark cluster blade, click **Script Actions** under **Usage**.</span></span> <span data-ttu-id="8f18a-125">Spuštění vlastní akce, který se instaluje TensorFlow head uzlů a uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="8f18a-125">Run the custom action that installs TensorFlow in the head nodes and the worker nodes.</span></span> <span data-ttu-id="8f18a-126">Skript bash lze odkazovat z: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh najdete v dokumentaci na [postup použít vlastní skript akce](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="8f18a-126">The bash script can be referenced from: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh Visit the documentation on [how to use custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>

   > [!NOTE]
   > <span data-ttu-id="8f18a-127">Existují dvě python instalace v clusteru.</span><span class="sxs-lookup"><span data-stu-id="8f18a-127">There are two python installations in the cluster.</span></span> <span data-ttu-id="8f18a-128">Spark použije instalace python Anaconda nacházející se v `/usr/bin/anaconda/bin`.</span><span class="sxs-lookup"><span data-stu-id="8f18a-128">Spark will use the Anaconda python installation located at `/usr/bin/anaconda/bin`.</span></span> <span data-ttu-id="8f18a-129">Referenční instalaci ve vaší vlastní akce prostřednictvím `/usr/bin/anaconda/bin/pip` a `/usr/bin/anaconda/bin/conda`.</span><span class="sxs-lookup"><span data-stu-id="8f18a-129">Reference that installation in your custom actions via `/usr/bin/anaconda/bin/pip` and `/usr/bin/anaconda/bin/conda`.</span></span>
   > 
   > 

3. <span data-ttu-id="8f18a-130">Otevřete Poznámkový blok PySpark Jupyter</span><span class="sxs-lookup"><span data-stu-id="8f18a-130">Open a PySpark Jupyter notebook</span></span>

    <span data-ttu-id="8f18a-131">![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Vytvoření nového poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="8f18a-131">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="8f18a-132">Nový poznámkový blok se vytvoří a otevře s názvem Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="8f18a-132">A new notebook is created and opened with the name Untitled.pynb.</span></span> <span data-ttu-id="8f18a-133">Klikněte na název poznámkového bloku v horní části a zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="8f18a-133">Click the notebook name at the top, and enter a friendly name.</span></span>

    <span data-ttu-id="8f18a-134">![Zadání názvu poznámkového bloku](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Zadání názvu poznámkového bloku")</span><span class="sxs-lookup"><span data-stu-id="8f18a-134">![Provide a name for the notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Provide a name for the notebook")</span></span>

5. <span data-ttu-id="8f18a-135">Nyní budete `import tensorflow` a spusťte hello world – ukázka.</span><span class="sxs-lookup"><span data-stu-id="8f18a-135">You will now `import tensorflow` and run a hello world example.</span></span> 

    <span data-ttu-id="8f18a-136">Kód zkopírovat:</span><span class="sxs-lookup"><span data-stu-id="8f18a-136">Code to copy:</span></span>

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    <span data-ttu-id="8f18a-137">Výsledek bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="8f18a-137">The result will look like this:</span></span>
    
    <span data-ttu-id="8f18a-138">![Provádění kódu TensorFlow](./media/hdinsight-apache-spark-python-package-installation/execution.png "TensorFlow spuštění kódu")</span><span class="sxs-lookup"><span data-stu-id="8f18a-138">![TensorFlow code execution](./media/hdinsight-apache-spark-python-package-installation/execution.png "Execute TensorFlow code")</span></span>



## <span data-ttu-id="8f18a-139"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="8f18a-139"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="8f18a-140">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f18a-140">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="8f18a-141">Scénáře</span><span class="sxs-lookup"><span data-stu-id="8f18a-141">Scenarios</span></span>
* [<span data-ttu-id="8f18a-142">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="8f18a-142">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="8f18a-143">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="8f18a-143">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="8f18a-144">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="8f18a-144">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="8f18a-145">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="8f18a-145">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="8f18a-146">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f18a-146">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="8f18a-147">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="8f18a-147">Create and run applications</span></span>
* [<span data-ttu-id="8f18a-148">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="8f18a-148">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="8f18a-149">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="8f18a-149">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="8f18a-150">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="8f18a-150">Tools and extensions</span></span>
* [<span data-ttu-id="8f18a-151">Použijte externí balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f18a-151">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="8f18a-152">Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="8f18a-152">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="8f18a-153">Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark</span><span class="sxs-lookup"><span data-stu-id="8f18a-153">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="8f18a-154">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f18a-154">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="8f18a-155">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f18a-155">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="8f18a-156">Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="8f18a-156">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="8f18a-157">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="8f18a-157">Manage resources</span></span>
* [<span data-ttu-id="8f18a-158">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f18a-158">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="8f18a-159">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8f18a-159">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
