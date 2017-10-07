---
title: "vlastní balíčky Maven aaaUse s Jupyter ve Sparku v Azure HDInsight | Microsoft Docs"
description: "Podrobný návod, jak tooconfigure poznámkové bloky Jupyter k dispozici s HDInsight Spark clusterů toouse vlastní Maven balíčky."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2a8bc545-064e-436f-8b5f-e67c26cfbf98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ba8ac13716bc94ab082a18fe02d4a40b2f1e09e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="6f2eb-103">Použijte externí balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="6f2eb-103">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6f2eb-104">Pomocí buňky magic</span><span class="sxs-lookup"><span data-stu-id="6f2eb-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="6f2eb-105">Pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="6f2eb-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="6f2eb-106">Zjistěte, jak tooconfigure poznámkového bloku Jupyter v clusteru Apache Spark v HDInsight toouse externí, komunity podílí **maven** balíčky, které nejsou součástí clusteru hello se na pole.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-106">Learn how tooconfigure a Jupyter notebook in Apache Spark cluster on HDInsight toouse external, community-contributed **maven** packages that are not included out-of-the-box in hello cluster.</span></span> 

<span data-ttu-id="6f2eb-107">Můžete hledat hello [Maven úložiště](http://search.maven.org/) hello úplný seznam balíčků, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-107">You can search hello [Maven repository](http://search.maven.org/) for hello complete list of packages that are available.</span></span> <span data-ttu-id="6f2eb-108">Seznam dostupných balíčků můžete také získat z jiných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-108">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="6f2eb-109">Například je k dispozici úplný seznam balíčků podílí komunity [Spark balíčky](http://spark-packages.org/).</span><span class="sxs-lookup"><span data-stu-id="6f2eb-109">For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).</span></span>

<span data-ttu-id="6f2eb-110">V tomto článku se dozvíte, jak toouse hello [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) balíček s hello Poznámkový blok Jupyter.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-110">In this article, you will learn how toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with hello Jupyter notebook.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="6f2eb-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6f2eb-111">Prerequisites</span></span>
<span data-ttu-id="6f2eb-112">Musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="6f2eb-112">You must have hello following:</span></span>

* <span data-ttu-id="6f2eb-113">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-113">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="6f2eb-114">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="6f2eb-114">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="6f2eb-115">Použijte externí balíčky s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="6f2eb-115">Use external packages with Jupyter notebooks</span></span>
1. <span data-ttu-id="6f2eb-116">Z hello [portálu Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel).</span><span class="sxs-lookup"><span data-stu-id="6f2eb-116">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="6f2eb-117">Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-117">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="6f2eb-118">Z okna clusteru Spark hello, klikněte na tlačítko **rychlé odkazy**a potom z hello **řídicí panel clusteru** okně klikněte na tlačítko **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-118">From hello Spark cluster blade, click **Quick Links**, and then from hello **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="6f2eb-119">Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-119">If prompted, enter hello admin credentials for hello cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6f2eb-120">Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-120">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="6f2eb-121">Nahraďte **CLUSTERNAME** s hello název clusteru:</span><span class="sxs-lookup"><span data-stu-id="6f2eb-121">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

   

3. <span data-ttu-id="6f2eb-122">Vytvořte nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-122">Create a new notebook.</span></span> <span data-ttu-id="6f2eb-123">Klikněte na tlačítko **nový**a potom klikněte na **Spark**.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-123">Click **New**, and then click **Spark**.</span></span>
   
    <span data-ttu-id="6f2eb-124">![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Vytvoření nového poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="6f2eb-124">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="6f2eb-125">Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-125">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="6f2eb-126">Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-126">Click hello notebook name at hello top, and enter a friendly name.</span></span>
   
    <span data-ttu-id="6f2eb-127">![Zadejte název pro hello notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "zadejte název pro hello Poznámkový blok")</span><span class="sxs-lookup"><span data-stu-id="6f2eb-127">![Provide a name for hello notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Provide a name for hello notebook")</span></span>

5. <span data-ttu-id="6f2eb-128">Budete používat hello `%%configure` magic tooconfigure hello poznámkového bloku toouse externí balíčku.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-128">You will use hello `%%configure` magic tooconfigure hello notebook toouse an external package.</span></span> <span data-ttu-id="6f2eb-129">Ujistěte se, volání hello v poznámkových bloků, které používají externí balíčky, `%%configure` magic v hello první buňky kódu.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-129">In notebooks that use external packages, make sure you call hello `%%configure` magic in hello first code cell.</span></span> <span data-ttu-id="6f2eb-130">Tím se zajistí, že tento jádra hello je nakonfigurované toouse hello balíčku před zahájením relace hello.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-130">This ensures that hello kernel is configured toouse hello package before hello session starts.</span></span>

    >[!IMPORTANT] 
    ><span data-ttu-id="6f2eb-131">Pokud zapomenete tooconfigure hello jádra hello první buňky, můžete použít hello `%%configure` s hello `-f` parametr, ale který restartuje hello relace a všechny průběh budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-131">If you forget tooconfigure hello kernel in hello first cell, you can use hello `%%configure` with hello `-f` parameter, but that will restart hello session and all progress will be lost.</span></span>

    | <span data-ttu-id="6f2eb-132">HDInsight verze</span><span class="sxs-lookup"><span data-stu-id="6f2eb-132">HDInsight version</span></span> | <span data-ttu-id="6f2eb-133">Příkaz</span><span class="sxs-lookup"><span data-stu-id="6f2eb-133">Command</span></span> |
    |-------------------|---------|
    |<span data-ttu-id="6f2eb-134">Pro HDInsight 3.3 a HDInsight 3.4</span><span class="sxs-lookup"><span data-stu-id="6f2eb-134">For HDInsight 3.3 and HDInsight 3.4</span></span> | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | <span data-ttu-id="6f2eb-135">Pro HDInsight 3.5</span><span class="sxs-lookup"><span data-stu-id="6f2eb-135">For HDInsight 3.5</span></span> | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

6. <span data-ttu-id="6f2eb-136">výše Hello fragmentu očekává hello maven souřadnice hello externí balíček v centrálním úložišti Maven.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-136">hello snippet above expects hello maven coordinates for hello external package in Maven Central Repository.</span></span> <span data-ttu-id="6f2eb-137">V této fragmentu kódu `com.databricks:spark-csv_2.10:1.4.0` je souřadnice maven hello **spark csv** balíčku.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-137">In this snippet, `com.databricks:spark-csv_2.10:1.4.0` is hello maven coordinate for **spark-csv** package.</span></span> <span data-ttu-id="6f2eb-138">Zde je, jak vytvořit hello souřadnice pro balíček.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-138">Here's how you construct hello coordinates for a package.</span></span>
   
    <span data-ttu-id="6f2eb-139">a.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-139">a.</span></span> <span data-ttu-id="6f2eb-140">Najděte balíček hello v hello Maven úložiště.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-140">Locate hello package in hello Maven Repository.</span></span> <span data-ttu-id="6f2eb-141">V tomto kurzu používáme [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span><span class="sxs-lookup"><span data-stu-id="6f2eb-141">For this tutorial, we use [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span></span>
   
    <span data-ttu-id="6f2eb-142">b.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-142">b.</span></span> <span data-ttu-id="6f2eb-143">Z úložiště hello shromážděte hello hodnoty pro **GroupId**, **ArtifactId**, a **verze**.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-143">From hello repository, gather hello values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="6f2eb-144">Ujistěte se, aby odpovídaly hello hodnoty, které shromáždíte vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-144">Make sure that hello values you gather match your cluster.</span></span> <span data-ttu-id="6f2eb-145">V tomto případě používáme Scala 2.10 a Spark 1.4.0 balíček, ale musíte tooselect různé verze pro příslušnou verzi Scala nebo Spark na hello v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-145">In this case, we are using a Scala 2.10 and Spark 1.4.0 package, but you may need tooselect different versions for hello appropriate Scala or Spark version in your cluster.</span></span> <span data-ttu-id="6f2eb-146">Můžete zjistit verzi Scala hello v clusteru tak, že spustíte `scala.util.Properties.versionString` na hello Spark Jupyter jádra nebo odeslání Spark.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-146">You can find out hello Scala version on your cluster by running `scala.util.Properties.versionString` on hello Spark Jupyter kernel or on Spark submit.</span></span> <span data-ttu-id="6f2eb-147">Můžete zjistit hello Spark verze v clusteru tak, že spustíte `sc.version` v poznámkových blocích Jupyter.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-147">You can find out hello Spark version on your cluster by running `sc.version` on Jupyter notebooks.</span></span>
   
    <span data-ttu-id="6f2eb-148">![Použijte externí balíčky s Poznámkový blok Jupyter](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "použijte externí balíčky s poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="6f2eb-148">![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")</span></span>
   
    <span data-ttu-id="6f2eb-149">c.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-149">c.</span></span> <span data-ttu-id="6f2eb-150">Řetězení hello tři hodnoty, oddělené dvojtečkou (**:**).</span><span class="sxs-lookup"><span data-stu-id="6f2eb-150">Concatenate hello three values, separated by a colon (**:**).</span></span>
   
        com.databricks:spark-csv_2.10:1.4.0

7. <span data-ttu-id="6f2eb-151">Spusťte buňky kódu hello s hello `%%configure` magic.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-151">Run hello code cell with hello `%%configure` magic.</span></span> <span data-ttu-id="6f2eb-152">Tím nakonfigurujete hello základní Livy relace toouse hello balíčku, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-152">This will configure hello underlying Livy session toouse hello package you provided.</span></span> <span data-ttu-id="6f2eb-153">V hello následné buněk v hello Poznámkový blok můžete nyní používat hello balíček, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-153">In hello subsequent cells in hello notebook, you can now use hello package, as shown below.</span></span>
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

8. <span data-ttu-id="6f2eb-154">Potom můžete spustit hello fragmenty, jako vidíte níže, tooview hello data z hello dataframe jste vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="6f2eb-154">You can then run hello snippets, like shown below, tooview hello data from hello dataframe you created in hello previous step.</span></span>
   
        df.show()
   
        df.select("Time").count()

## <span data-ttu-id="6f2eb-155"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="6f2eb-155"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="6f2eb-156">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="6f2eb-156">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="6f2eb-157">Scénáře</span><span class="sxs-lookup"><span data-stu-id="6f2eb-157">Scenarios</span></span>
* [<span data-ttu-id="6f2eb-158">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="6f2eb-158">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="6f2eb-159">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="6f2eb-159">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="6f2eb-160">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="6f2eb-160">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="6f2eb-161">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="6f2eb-161">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="6f2eb-162">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="6f2eb-162">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="6f2eb-163">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="6f2eb-163">Create and run applications</span></span>
* [<span data-ttu-id="6f2eb-164">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="6f2eb-164">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="6f2eb-165">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="6f2eb-165">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="6f2eb-166">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="6f2eb-166">Tools and extensions</span></span>

* [<span data-ttu-id="6f2eb-167">Používat externí python balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark na HDInsight Linux</span><span class="sxs-lookup"><span data-stu-id="6f2eb-167">Use external python packages with Jupyter notebooks in Apache Spark clusters on HDInsight Linux</span></span>](hdinsight-apache-spark-python-package-installation.md)
* [<span data-ttu-id="6f2eb-168">Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="6f2eb-168">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="6f2eb-169">Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace</span><span class="sxs-lookup"><span data-stu-id="6f2eb-169">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="6f2eb-170">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="6f2eb-170">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="6f2eb-171">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="6f2eb-171">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="6f2eb-172">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="6f2eb-172">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="6f2eb-173">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="6f2eb-173">Manage resources</span></span>
* [<span data-ttu-id="6f2eb-174">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="6f2eb-174">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="6f2eb-175">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="6f2eb-175">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

