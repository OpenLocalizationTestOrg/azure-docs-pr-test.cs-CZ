---
title: "Sada nástrojů pro kognitivní s Azure HDInsight Spark pro přímý learning | Microsoft Docs"
description: "Zjistěte, jak lze použít model natrénujete hloubkové learning kognitivní nástrojů Microsoft pro datovou sadu pomocí rozhraní API Python Spark v clusteru Azure HDInsight Spark."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: fafd738f782660b824732bab8cc3bec8405947e7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a><span data-ttu-id="d2e84-103">Použít Microsoft kognitivní Toolkit hloubkové učení modelu s clusteru Azure HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="d2e84-103">Use Microsoft Cognitive Toolkit deep learning model with Azure HDInsight Spark cluster</span></span>

<span data-ttu-id="d2e84-104">V tomto článku proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="d2e84-104">In this article, you do the following steps.</span></span>

1. <span data-ttu-id="d2e84-105">Spuštění vlastního skriptu k instalaci Microsoft kognitivní Toolkit v clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="d2e84-105">Run a custom script to install Microsoft Cognitive Toolkit on an Azure HDInsight Spark cluster.</span></span>

2. <span data-ttu-id="d2e84-106">Nahrajte do clusteru Spark chcete zjistit, jak použít modulu trained model Microsoft kognitivní Toolkit hloubkové learning do souborů k účtu úložiště Azure Blob pomocí poznámkového bloku Jupyter [API Python Spark (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)</span><span class="sxs-lookup"><span data-stu-id="d2e84-106">Upload a Jupyter notebook to the Spark cluster to see how to apply a trained Microsoft Cognitive Toolkit deep learning model to files in an Azure Blob Storage Account using the [Spark Python API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2e84-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d2e84-107">Prerequisites</span></span>

* <span data-ttu-id="d2e84-108">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="d2e84-108">**An Azure subscription**.</span></span> <span data-ttu-id="d2e84-109">Než začnete tento kurz, musíte mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="d2e84-109">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="d2e84-110">Přečtěte si téma [Bezplatné vytvoření účtu Microsoft Azure ještě dnes](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="d2e84-110">See [Create your free Azure account today](https://azure.microsoft.com/free).</span></span>

* <span data-ttu-id="d2e84-111">**Cluster Azure HDInsight Spark**.</span><span class="sxs-lookup"><span data-stu-id="d2e84-111">**Azure HDInsight Spark cluster**.</span></span> <span data-ttu-id="d2e84-112">V tomto článku vytvořte cluster Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="d2e84-112">For this article, create a Spark 2.0 cluster.</span></span> <span data-ttu-id="d2e84-113">Pokyny najdete v tématu [cluster vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="d2e84-113">For instructions, see [Create Apache Spark cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="how-does-this-solution-flow"></a><span data-ttu-id="d2e84-114">Jak toto řešení tok?</span><span class="sxs-lookup"><span data-stu-id="d2e84-114">How does this solution flow?</span></span>

<span data-ttu-id="d2e84-115">Toto řešení je rozděleno mezi v tomto článku a který v rámci tohoto kurzu nahrajete poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="d2e84-115">This solution is divided between this article and a Jupyter notebook that you upload as part of this tutorial.</span></span> <span data-ttu-id="d2e84-116">V tomto článku je provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d2e84-116">In this article, you complete the following steps:</span></span>

* <span data-ttu-id="d2e84-117">Spuštěním skriptu akce na clusteru HDInsight Spark instalace Microsoft kognitivní Toolkit a balíčků Python.</span><span class="sxs-lookup"><span data-stu-id="d2e84-117">Run a script action on an HDInsight Spark cluster to install Microsoft Cognitive Toolkit and Python packages.</span></span>
* <span data-ttu-id="d2e84-118">Nahrajte poznámkového bloku Jupyter, který spouští řešení do clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="d2e84-118">Upload the Jupyter notebook that runs the solution to the HDInsight Spark cluster.</span></span>

<span data-ttu-id="d2e84-119">Následující zbývající kroky jsou popsané v poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="d2e84-119">The following remaining steps are covered in the Jupyter notebook.</span></span>

- <span data-ttu-id="d2e84-120">Načíst ukázková bitové kopie do Spark Resiliant distribuované datové sady nebo RDD</span><span class="sxs-lookup"><span data-stu-id="d2e84-120">Load sample images into a Spark Resiliant Distributed Dataset or RDD</span></span>
   - <span data-ttu-id="d2e84-121">Načtení moduly a definování přednastavení</span><span class="sxs-lookup"><span data-stu-id="d2e84-121">Load modules and define presets</span></span>
   - <span data-ttu-id="d2e84-122">Stáhnout datovou sadu místně na clusteru Spark</span><span class="sxs-lookup"><span data-stu-id="d2e84-122">Download the dataset locally on the Spark cluster</span></span>
   - <span data-ttu-id="d2e84-123">Převést datová sada RDD</span><span class="sxs-lookup"><span data-stu-id="d2e84-123">Convert the dataset into an RDD</span></span>
- <span data-ttu-id="d2e84-124">Stanovení skóre obrázky pomocí modulu trained model kognitivní Toolkit</span><span class="sxs-lookup"><span data-stu-id="d2e84-124">Score the images using a trained Cognitive Toolkit model</span></span>
   - <span data-ttu-id="d2e84-125">Stahovat do clusteru Spark pro cvičný model kognitivní Toolkit</span><span class="sxs-lookup"><span data-stu-id="d2e84-125">Download the trained Cognitive Toolkit model to the Spark cluster</span></span>
   - <span data-ttu-id="d2e84-126">Definování funkcí, které má být používána uzlů pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="d2e84-126">Define functions to be used by worker nodes</span></span>
   - <span data-ttu-id="d2e84-127">Stanovení skóre bitové kopie na uzly pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="d2e84-127">Score the images on worker nodes</span></span>
   - <span data-ttu-id="d2e84-128">Vyhodnocení přesnosti modelu</span><span class="sxs-lookup"><span data-stu-id="d2e84-128">Evaluate model accuracy</span></span>


## <a name="install-microsoft-cognitive-toolkit"></a><span data-ttu-id="d2e84-129">Nainstalovat sadu Microsoft kognitivní Toolkit</span><span class="sxs-lookup"><span data-stu-id="d2e84-129">Install Microsoft Cognitive Toolkit</span></span>

<span data-ttu-id="d2e84-130">Sada nástrojů pro kognitivní můžete nainstalovat na clusteru Spark pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="d2e84-130">You can install Microsoft Cognitive Toolkit on a Spark cluster using script action.</span></span> <span data-ttu-id="d2e84-131">Akce skriptu používá vlastní skripty k instalaci součásti v clusteru, které nejsou k dispozici ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d2e84-131">Script action uses custom scripts to install components on the cluster that are not available by default.</span></span> <span data-ttu-id="d2e84-132">Pomocí HDInsight .NET SDK, nebo pomocí prostředí Azure PowerShell, můžete použít vlastní skript z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d2e84-132">You can use the custom script from the Azure Portal, by using HDInsight .NET SDK, or by using Azure PowerShell.</span></span> <span data-ttu-id="d2e84-133">Tento skript můžete použít také k instalaci sady nástrojů buď jako součást vytváření clusteru, nebo po nastavení a spuštění clusteru.</span><span class="sxs-lookup"><span data-stu-id="d2e84-133">You can also use the script to install the toolkit either as part of cluster creation, or after the cluster is up and running.</span></span> 

<span data-ttu-id="d2e84-134">V tomto článku používáme portálu k instalaci sady nástrojů, po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="d2e84-134">In this article, we use the portal to install the toolkit, after the cluster has been created.</span></span> <span data-ttu-id="d2e84-135">Další způsoby vlastního skriptu, najdete v části [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="d2e84-135">For other ways to run the custom script, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

### <a name="using-the-azure-portal"></a><span data-ttu-id="d2e84-136">Použití Portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d2e84-136">Using the Azure Portal</span></span>

<span data-ttu-id="d2e84-137">Pokyny o tom, jak pomocí portálu Azure ke spuštění akce skriptu najdete v tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation).</span><span class="sxs-lookup"><span data-stu-id="d2e84-137">For instructions on how to use the Azure Portal to run script action, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation).</span></span> <span data-ttu-id="d2e84-138">Ujistěte se, že zadáte následující vstupy pro instalaci Microsoft kognitivní Toolkit.</span><span class="sxs-lookup"><span data-stu-id="d2e84-138">Make sure you provide the following inputs to install Microsoft Cognitive Toolkit.</span></span>

* <span data-ttu-id="d2e84-139">Zadejte hodnotu pro název akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="d2e84-139">Provide a value for the script action name.</span></span>

* <span data-ttu-id="d2e84-140">Pro **Bash skriptu URI**, zadejte `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.</span><span class="sxs-lookup"><span data-stu-id="d2e84-140">For **Bash script URI**, enter `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.</span></span>

* <span data-ttu-id="d2e84-141">Zajistěte, aby spuštění skriptu pouze na head a pracovní uzly a zrušte zaškrtnutí všech políček.</span><span class="sxs-lookup"><span data-stu-id="d2e84-141">Make sure you run the script only on the head and worker nodes and clear all the other checkboxes.</span></span>

* <span data-ttu-id="d2e84-142">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d2e84-142">Click **Create**.</span></span>

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a><span data-ttu-id="d2e84-143">Nahrajte do clusteru Azure HDInsight Spark poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="d2e84-143">Upload the Jupyter notebook to Azure HDInsight Spark cluster</span></span>

<span data-ttu-id="d2e84-144">Pokud chcete používat Microsoft kognitivní Toolkit s clusteru Azure HDInsight Spark, je nutné načíst poznámkového bloku Jupyter **CNTK_model_scoring_on_Spark_walkthrough.ipynb** do clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="d2e84-144">To use the Microsoft Cognitive Toolkit with the Azure HDInsight Spark cluster, you must load the Jupyter notebook **CNTK_model_scoring_on_Spark_walkthrough.ipynb** to the Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="d2e84-145">Tento poznámkový blok je k dispozici na webu GitHub na [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span><span class="sxs-lookup"><span data-stu-id="d2e84-145">This notebook is available on GitHub at [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span></span>

1. <span data-ttu-id="d2e84-146">Naklonujte úložiště GitHub [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span><span class="sxs-lookup"><span data-stu-id="d2e84-146">Clone the GitHub repository [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span></span> <span data-ttu-id="d2e84-147">Pokyny ke klonování najdete v tématu [klonování úložiště](https://help.github.com/articles/cloning-a-repository/).</span><span class="sxs-lookup"><span data-stu-id="d2e84-147">For instructions to clone, see [Cloning a repository](https://help.github.com/articles/cloning-a-repository/).</span></span>

2. <span data-ttu-id="d2e84-148">Z portálu Azure, otevřete okno clusteru Spark, které jste už zřízené, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="d2e84-148">From the Azure Portal, open the Spark cluster blade that you already provisioned, click **Cluster Dashboard**, and then click **Jupyter notebook**.</span></span>

    <span data-ttu-id="d2e84-149">Můžete také spustit poznámkového bloku Jupyter tak, že přejdete na adresu URL `https://<clustername>.azurehdinsight.net/jupyter/`.</span><span class="sxs-lookup"><span data-stu-id="d2e84-149">You can also launch the Jupyter notebook by going to the URL `https://<clustername>.azurehdinsight.net/jupyter/`.</span></span> <span data-ttu-id="d2e84-150">Nahraďte \<clustername > s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d2e84-150">Replace \<clustername> with the name of your HDInsight cluster.</span></span>

3. <span data-ttu-id="d2e84-151">Z poznámkového bloku Jupyter, klikněte na tlačítko **nahrát** v pravém horním rohu a pak přejděte do umístění, které jste naklonovali úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="d2e84-151">From the Jupyter notebook, click **Upload** in the top-right corner and then navigate to the location where you cloned the GitHub repository.</span></span>

    <span data-ttu-id="d2e84-152">![Nahrát Poznámkový blok Jupyter ke clusteru Azure HDInsight Spark](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Poznámkový blok Jupyter nahrát do clusteru Azure HDInsight Spark")</span><span class="sxs-lookup"><span data-stu-id="d2e84-152">![Upload Jupyter notebook to Azure HDInsight Spark cluster](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Upload Jupyter notebook to Azure HDInsight Spark cluster")</span></span>

4. <span data-ttu-id="d2e84-153">Klikněte na tlačítko **nahrát** znovu.</span><span class="sxs-lookup"><span data-stu-id="d2e84-153">Click **Upload** again.</span></span>

5. <span data-ttu-id="d2e84-154">Po odeslání poznámkového bloku, klikněte na název poznámkového bloku a pak postupujte podle pokynů v samotné poznámkového bloku o tom, jak načíst sadu dat a proveďte tento kurz.</span><span class="sxs-lookup"><span data-stu-id="d2e84-154">After the notebook is uploaded, click the name of the notebook and then follow the instructions in the notebook itself on how to load the data set and perform the tutorial.</span></span>

## <a name="see-also"></a><span data-ttu-id="d2e84-155">Viz také</span><span class="sxs-lookup"><span data-stu-id="d2e84-155">See also</span></span>
* [<span data-ttu-id="d2e84-156">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="d2e84-156">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="d2e84-157">Scénáře</span><span class="sxs-lookup"><span data-stu-id="d2e84-157">Scenarios</span></span>
* [<span data-ttu-id="d2e84-158">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="d2e84-158">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="d2e84-159">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="d2e84-159">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="d2e84-160">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="d2e84-160">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="d2e84-161">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="d2e84-161">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="d2e84-162">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d2e84-162">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [<span data-ttu-id="d2e84-163">Analýza dat telemetrie Application Insights pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d2e84-163">Application Insight telemetry data analysis using Spark in HDInsight</span></span>](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="d2e84-164">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="d2e84-164">Create and run applications</span></span>
* [<span data-ttu-id="d2e84-165">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="d2e84-165">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="d2e84-166">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="d2e84-166">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="d2e84-167">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="d2e84-167">Tools and extensions</span></span>
* [<span data-ttu-id="d2e84-168">Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="d2e84-168">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="d2e84-169">Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark</span><span class="sxs-lookup"><span data-stu-id="d2e84-169">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="d2e84-170">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d2e84-170">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="d2e84-171">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="d2e84-171">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="d2e84-172">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="d2e84-172">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="d2e84-173">Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="d2e84-173">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="d2e84-174">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="d2e84-174">Manage resources</span></span>
* [<span data-ttu-id="d2e84-175">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="d2e84-175">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="d2e84-176">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d2e84-176">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
