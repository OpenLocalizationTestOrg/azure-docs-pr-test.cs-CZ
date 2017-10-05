---
title: "Vlastní balíčky Maven pomocí Jupyter ve Sparku v Azure HDInsight | Microsoft Docs"
description: "Podrobné pokyny o tom, jak nakonfigurovat k dispozici poznámkové bloky Jupyter s clustery HDInsight Spark použití vlastní Maven balíčků."
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
ms.openlocfilehash: 0bcfe220e60e34937c667c7b416065d5f3dc8d63
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="49f0a-103">Použijte externí balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="49f0a-103">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="49f0a-104">Pomocí buňky magic</span><span class="sxs-lookup"><span data-stu-id="49f0a-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="49f0a-105">Pomocí akce skriptu</span><span class="sxs-lookup"><span data-stu-id="49f0a-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="49f0a-106">Naučte se konfigurovat poznámkového bloku Jupyter v clusteru Apache Spark v HDInsight používat externí, komunity podílí **maven** balíčky, které nejsou zahrnuty v clusteru se na pole.</span><span class="sxs-lookup"><span data-stu-id="49f0a-106">Learn how to configure a Jupyter notebook in Apache Spark cluster on HDInsight to use external, community-contributed **maven** packages that are not included out-of-the-box in the cluster.</span></span> 

<span data-ttu-id="49f0a-107">Můžete hledat [Maven úložiště](http://search.maven.org/) úplný seznam balíčků, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="49f0a-107">You can search the [Maven repository](http://search.maven.org/) for the complete list of packages that are available.</span></span> <span data-ttu-id="49f0a-108">Seznam dostupných balíčků můžete také získat z jiných zdrojů.</span><span class="sxs-lookup"><span data-stu-id="49f0a-108">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="49f0a-109">Například je k dispozici úplný seznam balíčků podílí komunity [Spark balíčky](http://spark-packages.org/).</span><span class="sxs-lookup"><span data-stu-id="49f0a-109">For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).</span></span>

<span data-ttu-id="49f0a-110">V tomto článku se dozvíte, jak používat [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) balíček s poznámkovým blokem Jupyter.</span><span class="sxs-lookup"><span data-stu-id="49f0a-110">In this article, you will learn how to use the [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with the Jupyter notebook.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="49f0a-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="49f0a-111">Prerequisites</span></span>
<span data-ttu-id="49f0a-112">Musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="49f0a-112">You must have the following:</span></span>

* <span data-ttu-id="49f0a-113">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="49f0a-113">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="49f0a-114">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="49f0a-114">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="49f0a-115">Použijte externí balíčky s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="49f0a-115">Use external packages with Jupyter notebooks</span></span>
1. <span data-ttu-id="49f0a-116">Z [Portálu Azure](https://portal.azure.com/) z úvodního panelu klikněte na dlaždici pro váš cluster Spark (pokud je připnutý na úvodní panel).</span><span class="sxs-lookup"><span data-stu-id="49f0a-116">From the [Azure Portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="49f0a-117">Můžete také přejít na cluster pod položkou **Procházet vše** > **Clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="49f0a-117">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="49f0a-118">Z okna clusteru Spark klikněte na tlačítko **Rychlé odkazy** a pak z okna **Řídicí panel clusteru** klikněte na tlačítko **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="49f0a-118">From the Spark cluster blade, click **Quick Links**, and then from the **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="49f0a-119">Po vyzvání zadejte přihlašovací údaje správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="49f0a-119">If prompted, enter the admin credentials for the cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="49f0a-120">Může také otevřít poznámkový blok Jupyter pro váš cluster tak, že otevřete následující adresu URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="49f0a-120">You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="49f0a-121">Nahraďte **CLUSTERNAME** názvem clusteru:</span><span class="sxs-lookup"><span data-stu-id="49f0a-121">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

   

3. <span data-ttu-id="49f0a-122">Vytvořte nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="49f0a-122">Create a new notebook.</span></span> <span data-ttu-id="49f0a-123">Klikněte na tlačítko **nový**a potom klikněte na **Spark**.</span><span class="sxs-lookup"><span data-stu-id="49f0a-123">Click **New**, and then click **Spark**.</span></span>
   
    <span data-ttu-id="49f0a-124">![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Vytvoření nového poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="49f0a-124">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="49f0a-125">Nový poznámkový blok se vytvoří a otevře s názvem Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="49f0a-125">A new notebook is created and opened with the name Untitled.pynb.</span></span> <span data-ttu-id="49f0a-126">Klikněte na název poznámkového bloku v horní části a zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="49f0a-126">Click the notebook name at the top, and enter a friendly name.</span></span>
   
    <span data-ttu-id="49f0a-127">![Zadání názvu poznámkového bloku](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Zadání názvu poznámkového bloku")</span><span class="sxs-lookup"><span data-stu-id="49f0a-127">![Provide a name for the notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Provide a name for the notebook")</span></span>

5. <span data-ttu-id="49f0a-128">Budete používat `%%configure` magic konfigurace poznámkového bloku na externí balíček použít.</span><span class="sxs-lookup"><span data-stu-id="49f0a-128">You will use the `%%configure` magic to configure the notebook to use an external package.</span></span> <span data-ttu-id="49f0a-129">Ujistěte se, zavoláte v poznámkových bloků, které používají externí balíčky, `%%configure` magic v první buňky kódu.</span><span class="sxs-lookup"><span data-stu-id="49f0a-129">In notebooks that use external packages, make sure you call the `%%configure` magic in the first code cell.</span></span> <span data-ttu-id="49f0a-130">Tím se zajistí, že jádra je nakonfigurované na použití balíčku před zahájením relace.</span><span class="sxs-lookup"><span data-stu-id="49f0a-130">This ensures that the kernel is configured to use the package before the session starts.</span></span>

    >[!IMPORTANT] 
    ><span data-ttu-id="49f0a-131">Pokud zapomenete nakonfigurovat jádra v první buňky, můžete použít `%%configure` s `-f` parametr, ale který restartuje relace a všechny průběh budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="49f0a-131">If you forget to configure the kernel in the first cell, you can use the `%%configure` with the `-f` parameter, but that will restart the session and all progress will be lost.</span></span>

    | <span data-ttu-id="49f0a-132">HDInsight verze</span><span class="sxs-lookup"><span data-stu-id="49f0a-132">HDInsight version</span></span> | <span data-ttu-id="49f0a-133">Příkaz</span><span class="sxs-lookup"><span data-stu-id="49f0a-133">Command</span></span> |
    |-------------------|---------|
    |<span data-ttu-id="49f0a-134">Pro HDInsight 3.3 a HDInsight 3.4</span><span class="sxs-lookup"><span data-stu-id="49f0a-134">For HDInsight 3.3 and HDInsight 3.4</span></span> | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | <span data-ttu-id="49f0a-135">Pro HDInsight 3.5</span><span class="sxs-lookup"><span data-stu-id="49f0a-135">For HDInsight 3.5</span></span> | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

6. <span data-ttu-id="49f0a-136">Výše uvedeném fragmentu očekává souřadnice maven pro externí balíček v centrálním úložišti Maven.</span><span class="sxs-lookup"><span data-stu-id="49f0a-136">The snippet above expects the maven coordinates for the external package in Maven Central Repository.</span></span> <span data-ttu-id="49f0a-137">V této fragmentu kódu `com.databricks:spark-csv_2.10:1.4.0` je souřadnice maven **spark csv** balíčku.</span><span class="sxs-lookup"><span data-stu-id="49f0a-137">In this snippet, `com.databricks:spark-csv_2.10:1.4.0` is the maven coordinate for **spark-csv** package.</span></span> <span data-ttu-id="49f0a-138">Zde je, jak vytvořit souřadnice pro balíček.</span><span class="sxs-lookup"><span data-stu-id="49f0a-138">Here's how you construct the coordinates for a package.</span></span>
   
    <span data-ttu-id="49f0a-139">a.</span><span class="sxs-lookup"><span data-stu-id="49f0a-139">a.</span></span> <span data-ttu-id="49f0a-140">Najděte balíček v úložišti Maven.</span><span class="sxs-lookup"><span data-stu-id="49f0a-140">Locate the package in the Maven Repository.</span></span> <span data-ttu-id="49f0a-141">V tomto kurzu používáme [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span><span class="sxs-lookup"><span data-stu-id="49f0a-141">For this tutorial, we use [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span></span>
   
    <span data-ttu-id="49f0a-142">b.</span><span class="sxs-lookup"><span data-stu-id="49f0a-142">b.</span></span> <span data-ttu-id="49f0a-143">Z úložiště, shromážděte hodnoty **GroupId**, **ArtifactId**, a **verze**.</span><span class="sxs-lookup"><span data-stu-id="49f0a-143">From the repository, gather the values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="49f0a-144">Ujistěte se, že hodnoty, které shromáždíte odpovídat vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="49f0a-144">Make sure that the values you gather match your cluster.</span></span> <span data-ttu-id="49f0a-145">V tomto případě používáme Scala 2.10 a Spark 1.4.0 balíček, ale budete muset vybrat různé verze pro příslušné Scala nebo Spark verze v clusteru.</span><span class="sxs-lookup"><span data-stu-id="49f0a-145">In this case, we are using a Scala 2.10 and Spark 1.4.0 package, but you may need to select different versions for the appropriate Scala or Spark version in your cluster.</span></span> <span data-ttu-id="49f0a-146">Můžete zjistit verzi Scala v clusteru tak, že spustíte `scala.util.Properties.versionString` na Spark Jupyter jádra nebo odeslání Spark.</span><span class="sxs-lookup"><span data-stu-id="49f0a-146">You can find out the Scala version on your cluster by running `scala.util.Properties.versionString` on the Spark Jupyter kernel or on Spark submit.</span></span> <span data-ttu-id="49f0a-147">Můžete zjistit verzi Spark v clusteru tak, že spustíte `sc.version` v poznámkových blocích Jupyter.</span><span class="sxs-lookup"><span data-stu-id="49f0a-147">You can find out the Spark version on your cluster by running `sc.version` on Jupyter notebooks.</span></span>
   
    <span data-ttu-id="49f0a-148">![Použijte externí balíčky s Poznámkový blok Jupyter](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "použijte externí balíčky s poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="49f0a-148">![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")</span></span>
   
    <span data-ttu-id="49f0a-149">c.</span><span class="sxs-lookup"><span data-stu-id="49f0a-149">c.</span></span> <span data-ttu-id="49f0a-150">Řetězení tři hodnoty oddělené dvojtečkou (**:**).</span><span class="sxs-lookup"><span data-stu-id="49f0a-150">Concatenate the three values, separated by a colon (**:**).</span></span>
   
        com.databricks:spark-csv_2.10:1.4.0

7. <span data-ttu-id="49f0a-151">Spustit buňky kódu pomocí `%%configure` magic.</span><span class="sxs-lookup"><span data-stu-id="49f0a-151">Run the code cell with the `%%configure` magic.</span></span> <span data-ttu-id="49f0a-152">Tím nakonfigurujete základní relace Livy k použití balíčku, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="49f0a-152">This will configure the underlying Livy session to use the package you provided.</span></span> <span data-ttu-id="49f0a-153">V následujících buněk v poznámkovém bloku můžete nyní používat balíčku, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="49f0a-153">In the subsequent cells in the notebook, you can now use the package, as shown below.</span></span>
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

8. <span data-ttu-id="49f0a-154">Potom můžete spustit fragmenty kódu, jako vidíte níže, chcete-li zobrazit data z dataframe jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="49f0a-154">You can then run the snippets, like shown below, to view the data from the dataframe you created in the previous step.</span></span>
   
        df.show()
   
        df.select("Time").count()

## <span data-ttu-id="49f0a-155"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="49f0a-155"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="49f0a-156">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="49f0a-156">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="49f0a-157">Scénáře</span><span class="sxs-lookup"><span data-stu-id="49f0a-157">Scenarios</span></span>
* [<span data-ttu-id="49f0a-158">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="49f0a-158">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="49f0a-159">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="49f0a-159">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="49f0a-160">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="49f0a-160">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="49f0a-161">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="49f0a-161">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="49f0a-162">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="49f0a-162">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="49f0a-163">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="49f0a-163">Create and run applications</span></span>
* [<span data-ttu-id="49f0a-164">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="49f0a-164">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="49f0a-165">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="49f0a-165">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="49f0a-166">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="49f0a-166">Tools and extensions</span></span>

* [<span data-ttu-id="49f0a-167">Používat externí python balíčky s poznámkovými bloky Jupyter v clusterech Apache Spark na HDInsight Linux</span><span class="sxs-lookup"><span data-stu-id="49f0a-167">Use external python packages with Jupyter notebooks in Apache Spark clusters on HDInsight Linux</span></span>](hdinsight-apache-spark-python-package-installation.md)
* [<span data-ttu-id="49f0a-168">Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="49f0a-168">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="49f0a-169">Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark</span><span class="sxs-lookup"><span data-stu-id="49f0a-169">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="49f0a-170">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="49f0a-170">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="49f0a-171">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="49f0a-171">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="49f0a-172">Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="49f0a-172">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="49f0a-173">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="49f0a-173">Manage resources</span></span>
* [<span data-ttu-id="49f0a-174">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="49f0a-174">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="49f0a-175">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="49f0a-175">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

