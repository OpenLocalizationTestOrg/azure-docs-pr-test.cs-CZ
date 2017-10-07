---
title: "aaaMicrosoft kognitivní Toolkit s Azure HDInsight Spark pro přímý learning | Microsoft Docs"
description: "Zjistěte, jak sada vyškolení Microsoft kognitivní nástrojů hloubkové learning modelu může být datové sady použité tooa pomocí hello Spark Python API v clusteru Azure HDInsight Spark."
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
ms.openlocfilehash: c296d4697f16d4ef6a958fdb55289807d745ea40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a><span data-ttu-id="07ec6-103">Použít Microsoft kognitivní Toolkit hloubkové učení modelu s clusteru Azure HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="07ec6-103">Use Microsoft Cognitive Toolkit deep learning model with Azure HDInsight Spark cluster</span></span>

<span data-ttu-id="07ec6-104">V tomto článku hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="07ec6-104">In this article, you do hello following steps.</span></span>

1. <span data-ttu-id="07ec6-105">Spusťte vlastní skript tooinstall Microsoft kognitivní Toolkit v clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="07ec6-105">Run a custom script tooinstall Microsoft Cognitive Toolkit on an Azure HDInsight Spark cluster.</span></span>

2. <span data-ttu-id="07ec6-106">Jak nahrát toosee clusteru Spark Jupyter poznámkového bloku toohello tooapply vyškolení Microsoft kognitivní Toolkit hloubkové učení modelu toofiles v účtu úložiště objektů Blob Azure pomocí hello [API Python Spark (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)</span><span class="sxs-lookup"><span data-stu-id="07ec6-106">Upload a Jupyter notebook toohello Spark cluster toosee how tooapply a trained Microsoft Cognitive Toolkit deep learning model toofiles in an Azure Blob Storage Account using hello [Spark Python API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07ec6-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="07ec6-107">Prerequisites</span></span>

* <span data-ttu-id="07ec6-108">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="07ec6-108">**An Azure subscription**.</span></span> <span data-ttu-id="07ec6-109">Než začnete tento kurz, musíte mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="07ec6-109">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="07ec6-110">Přečtěte si téma [Bezplatné vytvoření účtu Microsoft Azure ještě dnes](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="07ec6-110">See [Create your free Azure account today](https://azure.microsoft.com/free).</span></span>

* <span data-ttu-id="07ec6-111">**Cluster Azure HDInsight Spark**.</span><span class="sxs-lookup"><span data-stu-id="07ec6-111">**Azure HDInsight Spark cluster**.</span></span> <span data-ttu-id="07ec6-112">V tomto článku vytvořte cluster Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="07ec6-112">For this article, create a Spark 2.0 cluster.</span></span> <span data-ttu-id="07ec6-113">Pokyny najdete v tématu [cluster vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="07ec6-113">For instructions, see [Create Apache Spark cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="how-does-this-solution-flow"></a><span data-ttu-id="07ec6-114">Jak toto řešení tok?</span><span class="sxs-lookup"><span data-stu-id="07ec6-114">How does this solution flow?</span></span>

<span data-ttu-id="07ec6-115">Toto řešení je rozděleno mezi v tomto článku a který v rámci tohoto kurzu nahrajete poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="07ec6-115">This solution is divided between this article and a Jupyter notebook that you upload as part of this tutorial.</span></span> <span data-ttu-id="07ec6-116">V tomto článku proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="07ec6-116">In this article, you complete hello following steps:</span></span>

* <span data-ttu-id="07ec6-117">Spusťte akci skriptu na clusteru HDInsight Spark balíčků tooinstall Microsoft kognitivní Toolkit a Python.</span><span class="sxs-lookup"><span data-stu-id="07ec6-117">Run a script action on an HDInsight Spark cluster tooinstall Microsoft Cognitive Toolkit and Python packages.</span></span>
* <span data-ttu-id="07ec6-118">Nahrajte poznámkového bloku Jupyter hello, který spouští hello řešení toohello clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="07ec6-118">Upload hello Jupyter notebook that runs hello solution toohello HDInsight Spark cluster.</span></span>

<span data-ttu-id="07ec6-119">Hello následující zbývající kroky jsou popsané v hello Poznámkový blok Jupyter.</span><span class="sxs-lookup"><span data-stu-id="07ec6-119">hello following remaining steps are covered in hello Jupyter notebook.</span></span>

- <span data-ttu-id="07ec6-120">Načíst ukázková bitové kopie do Spark Resiliant distribuované datové sady nebo RDD</span><span class="sxs-lookup"><span data-stu-id="07ec6-120">Load sample images into a Spark Resiliant Distributed Dataset or RDD</span></span>
   - <span data-ttu-id="07ec6-121">Načtení moduly a definování přednastavení</span><span class="sxs-lookup"><span data-stu-id="07ec6-121">Load modules and define presets</span></span>
   - <span data-ttu-id="07ec6-122">Stáhnout datovou sadu hello místně na clusteru Spark hello</span><span class="sxs-lookup"><span data-stu-id="07ec6-122">Download hello dataset locally on hello Spark cluster</span></span>
   - <span data-ttu-id="07ec6-123">Převést RDD hello datové sady</span><span class="sxs-lookup"><span data-stu-id="07ec6-123">Convert hello dataset into an RDD</span></span>
- <span data-ttu-id="07ec6-124">Skóre hello obrázky pomocí modulu trained model kognitivní Toolkit</span><span class="sxs-lookup"><span data-stu-id="07ec6-124">Score hello images using a trained Cognitive Toolkit model</span></span>
   - <span data-ttu-id="07ec6-125">Stáhnout clusteru Spark toohello modelu kognitivní Toolkit hello cvičení</span><span class="sxs-lookup"><span data-stu-id="07ec6-125">Download hello trained Cognitive Toolkit model toohello Spark cluster</span></span>
   - <span data-ttu-id="07ec6-126">Definování funkcí toobe používané uzlů pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="07ec6-126">Define functions toobe used by worker nodes</span></span>
   - <span data-ttu-id="07ec6-127">Skóre hello obrázků uzlů pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="07ec6-127">Score hello images on worker nodes</span></span>
   - <span data-ttu-id="07ec6-128">Vyhodnocení přesnosti modelu</span><span class="sxs-lookup"><span data-stu-id="07ec6-128">Evaluate model accuracy</span></span>


## <a name="install-microsoft-cognitive-toolkit"></a><span data-ttu-id="07ec6-129">Nainstalovat sadu Microsoft kognitivní Toolkit</span><span class="sxs-lookup"><span data-stu-id="07ec6-129">Install Microsoft Cognitive Toolkit</span></span>

<span data-ttu-id="07ec6-130">Sada nástrojů pro kognitivní můžete nainstalovat na clusteru Spark pomocí akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="07ec6-130">You can install Microsoft Cognitive Toolkit on a Spark cluster using script action.</span></span> <span data-ttu-id="07ec6-131">Akce skriptu používá vlastní skripty tooinstall součásti v clusteru hello, které nejsou k dispozici ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="07ec6-131">Script action uses custom scripts tooinstall components on hello cluster that are not available by default.</span></span> <span data-ttu-id="07ec6-132">Pomocí HDInsight .NET SDK, nebo pomocí prostředí Azure PowerShell, můžete použít vlastní skript hello z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="07ec6-132">You can use hello custom script from hello Azure Portal, by using HDInsight .NET SDK, or by using Azure PowerShell.</span></span> <span data-ttu-id="07ec6-133">Můžete taky hello skriptu tooinstall hello toolkit buď jako součást vytváření clusteru, nebo po hello clusteru je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="07ec6-133">You can also use hello script tooinstall hello toolkit either as part of cluster creation, or after hello cluster is up and running.</span></span> 

<span data-ttu-id="07ec6-134">V tomto článku používáme hello portálu tooinstall hello toolkit, po vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="07ec6-134">In this article, we use hello portal tooinstall hello toolkit, after hello cluster has been created.</span></span> <span data-ttu-id="07ec6-135">Jiné způsoby toorun hello vlastních skriptů, najdete v části [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="07ec6-135">For other ways toorun hello custom script, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

### <a name="using-hello-azure-portal"></a><span data-ttu-id="07ec6-136">Pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="07ec6-136">Using hello Azure Portal</span></span>

<span data-ttu-id="07ec6-137">Pokyny jak toouse hello portálu Azure toorun skript akce, najdete v části [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation).</span><span class="sxs-lookup"><span data-stu-id="07ec6-137">For instructions on how toouse hello Azure Portal toorun script action, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation).</span></span> <span data-ttu-id="07ec6-138">Ujistěte se, že zadáte následující vstupy tooinstall kognitivní nástrojů Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="07ec6-138">Make sure you provide hello following inputs tooinstall Microsoft Cognitive Toolkit.</span></span>

* <span data-ttu-id="07ec6-139">Zadejte hodnotu pro název akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="07ec6-139">Provide a value for hello script action name.</span></span>

* <span data-ttu-id="07ec6-140">Pro **Bash skriptu URI**, zadejte `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.</span><span class="sxs-lookup"><span data-stu-id="07ec6-140">For **Bash script URI**, enter `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.</span></span>

* <span data-ttu-id="07ec6-141">Zajistěte, aby spuštění skriptu hello pouze na hello head a pracovní uzly a zrušte všechny ostatní zaškrtávací políčka hello.</span><span class="sxs-lookup"><span data-stu-id="07ec6-141">Make sure you run hello script only on hello head and worker nodes and clear all hello other checkboxes.</span></span>

* <span data-ttu-id="07ec6-142">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="07ec6-142">Click **Create**.</span></span>

## <a name="upload-hello-jupyter-notebook-tooazure-hdinsight-spark-cluster"></a><span data-ttu-id="07ec6-143">Nahrát tooAzure poznámkového bloku Jupyter hello clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="07ec6-143">Upload hello Jupyter notebook tooAzure HDInsight Spark cluster</span></span>

<span data-ttu-id="07ec6-144">hello toouse kognitivní nástrojů Microsoft s hello clusteru Azure HDInsight Spark, je nutné načíst hello Poznámkový blok Jupyter **CNTK_model_scoring_on_Spark_walkthrough.ipynb** toohello clusteru Azure HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="07ec6-144">toouse hello Microsoft Cognitive Toolkit with hello Azure HDInsight Spark cluster, you must load hello Jupyter notebook **CNTK_model_scoring_on_Spark_walkthrough.ipynb** toohello Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="07ec6-145">Tento poznámkový blok je k dispozici na webu GitHub na [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span><span class="sxs-lookup"><span data-stu-id="07ec6-145">This notebook is available on GitHub at [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span></span>

1. <span data-ttu-id="07ec6-146">Klonování úložiště GitHub hello [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span><span class="sxs-lookup"><span data-stu-id="07ec6-146">Clone hello GitHub repository [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span></span> <span data-ttu-id="07ec6-147">Tooclone pokyny najdete v části [klonování úložiště](https://help.github.com/articles/cloning-a-repository/).</span><span class="sxs-lookup"><span data-stu-id="07ec6-147">For instructions tooclone, see [Cloning a repository](https://help.github.com/articles/cloning-a-repository/).</span></span>

2. <span data-ttu-id="07ec6-148">Hello portálu Azure, otevřete okno clusteru Spark hello, které jste už zřízené, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="07ec6-148">From hello Azure Portal, open hello Spark cluster blade that you already provisioned, click **Cluster Dashboard**, and then click **Jupyter notebook**.</span></span>

    <span data-ttu-id="07ec6-149">Poznámkový blok Jupyter hello můžete spustit také tak, že přejdete na adresu URL toohello `https://<clustername>.azurehdinsight.net/jupyter/`.</span><span class="sxs-lookup"><span data-stu-id="07ec6-149">You can also launch hello Jupyter notebook by going toohello URL `https://<clustername>.azurehdinsight.net/jupyter/`.</span></span> <span data-ttu-id="07ec6-150">Nahraďte \<clustername > s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="07ec6-150">Replace \<clustername> with hello name of your HDInsight cluster.</span></span>

3. <span data-ttu-id="07ec6-151">Z hello Poznámkový blok Jupyter, klikněte na tlačítko **nahrát** v hello pravém horním rohu a pak přejděte toohello umístění, které jste naklonovali úložiště GitHub hello.</span><span class="sxs-lookup"><span data-stu-id="07ec6-151">From hello Jupyter notebook, click **Upload** in hello top-right corner and then navigate toohello location where you cloned hello GitHub repository.</span></span>

    <span data-ttu-id="07ec6-152">![Nahrát tooAzure poznámkového bloku Jupyter clusteru HDInsight Spark](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "tooAzure poznámkového bloku Jupyter nahrát clusteru HDInsight Spark")</span><span class="sxs-lookup"><span data-stu-id="07ec6-152">![Upload Jupyter notebook tooAzure HDInsight Spark cluster](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Upload Jupyter notebook tooAzure HDInsight Spark cluster")</span></span>

4. <span data-ttu-id="07ec6-153">Klikněte na tlačítko **nahrát** znovu.</span><span class="sxs-lookup"><span data-stu-id="07ec6-153">Click **Upload** again.</span></span>

5. <span data-ttu-id="07ec6-154">Po odeslání hello Poznámkový blok klikněte na název hello hello Poznámkový blok a pak postupujte podle pokynů hello v poznámkovém bloku hello sám sebe na tom, jak tooload hello datové sady a provádět hello kurzu.</span><span class="sxs-lookup"><span data-stu-id="07ec6-154">After hello notebook is uploaded, click hello name of hello notebook and then follow hello instructions in hello notebook itself on how tooload hello data set and perform hello tutorial.</span></span>

## <a name="see-also"></a><span data-ttu-id="07ec6-155">Viz také</span><span class="sxs-lookup"><span data-stu-id="07ec6-155">See also</span></span>
* [<span data-ttu-id="07ec6-156">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="07ec6-156">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="07ec6-157">Scénáře</span><span class="sxs-lookup"><span data-stu-id="07ec6-157">Scenarios</span></span>
* [<span data-ttu-id="07ec6-158">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="07ec6-158">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="07ec6-159">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="07ec6-159">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="07ec6-160">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="07ec6-160">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="07ec6-161">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="07ec6-161">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="07ec6-162">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="07ec6-162">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [<span data-ttu-id="07ec6-163">Analýza dat telemetrie Application Insights pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="07ec6-163">Application Insight telemetry data analysis using Spark in HDInsight</span></span>](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="07ec6-164">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="07ec6-164">Create and run applications</span></span>
* [<span data-ttu-id="07ec6-165">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="07ec6-165">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="07ec6-166">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="07ec6-166">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="07ec6-167">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="07ec6-167">Tools and extensions</span></span>
* [<span data-ttu-id="07ec6-168">Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="07ec6-168">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="07ec6-169">Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace</span><span class="sxs-lookup"><span data-stu-id="07ec6-169">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="07ec6-170">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="07ec6-170">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="07ec6-171">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="07ec6-171">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="07ec6-172">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="07ec6-172">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="07ec6-173">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="07ec6-173">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="07ec6-174">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="07ec6-174">Manage resources</span></span>
* [<span data-ttu-id="07ec6-175">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="07ec6-175">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="07ec6-176">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="07ec6-176">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
