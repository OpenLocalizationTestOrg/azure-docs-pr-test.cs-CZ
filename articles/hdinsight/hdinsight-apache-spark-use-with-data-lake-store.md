---
title: aaaUse data tooanalyze Apache Spark v Azure Data Lake Store | Microsoft Docs
description: "Spuštění úloh Spark tooanalyze data uložená v Azure Data Lake Store"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f174323-c17b-428c-903d-04f0e272784c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 3b7f628f7a8114d2ca6f3f9219ce107905f1c818
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hdinsight-spark-cluster-tooanalyze-data-in-data-lake-store"></a><span data-ttu-id="9d3e5-103">Použít data tooanalyze clusteru HDInsight Spark v Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="9d3e5-103">Use HDInsight Spark cluster tooanalyze data in Data Lake Store</span></span>

<span data-ttu-id="9d3e5-104">V tomto kurzu použijete k dispozici poznámkového bloku Jupyter s toorun clustery HDInsight Spark úlohu, která čte data z účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-104">In this tutorial, you use Jupyter notebook available with HDInsight Spark clusters toorun a job that reads data from a Data Lake Store account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d3e5-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9d3e5-105">Prerequisites</span></span>

* <span data-ttu-id="9d3e5-106">Účet Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-106">Azure Data Lake Store account.</span></span> <span data-ttu-id="9d3e5-107">Postupujte podle pokynů hello [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9d3e5-107">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

* <span data-ttu-id="9d3e5-108">Cluster Azure HDInsight Spark s Data Lake Store jako úložiště.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-108">Azure HDInsight Spark cluster with Data Lake Store as storage.</span></span> <span data-ttu-id="9d3e5-109">Postupujte podle pokynů hello [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9d3e5-109">Follow hello instructions at [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    
## <a name="prepare-hello-data"></a><span data-ttu-id="9d3e5-110">Příprava dat hello</span><span class="sxs-lookup"><span data-stu-id="9d3e5-110">Prepare hello data</span></span>

> [!NOTE]
> <span data-ttu-id="9d3e5-111">Není nutné tooperform tento krok Pokud jste vytvořili hello clusteru HDInsight s Data Lake Store jako výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-111">You do not need tooperform this step if you have created hello HDInsight cluster with Data Lake Store as default storage.</span></span> <span data-ttu-id="9d3e5-112">procesy vytváření clusteru Hello přidá ukázková data v účtu Data Lake Store hello, který zadáte při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-112">hello cluster creation processes adds some sample data in hello Data Lake Store account that you specify while creating hello cluster.</span></span> <span data-ttu-id="9d3e5-113">Přeskočte část toohello [clusteru používejte HDInsight Spark s Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="9d3e5-113">Skip toohello section [Use HDInsight Spark cluster with Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span></span>
>
>

<span data-ttu-id="9d3e5-114">Pokud jste vytvořili clusteru HDInsight s Data Lake Store jako další úložiště a Azure Blob Storage jako výchozí úložiště, měli byste se nejprve zkopírovat přes toohello některých ukázkových dat účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-114">If you created an HDInsight cluster with Data Lake Store as additional storage and Azure Storage Blob as default storage, you should first copy over some sample data toohello Data Lake Store account.</span></span> <span data-ttu-id="9d3e5-115">Můžete použít hello ukázkových dat z Azure Storage Blob přidruženého k clusteru HDInsight hello hello.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-115">You can use hello sample data from hello Azure Storage Blob associated with hello HDInsight cluster.</span></span> <span data-ttu-id="9d3e5-116">Můžete použít hello [ADLCopy nástroj](http://aka.ms/downloadadlcopy) toodo tak.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-116">You can use hello [ADLCopy tool](http://aka.ms/downloadadlcopy) toodo so.</span></span> <span data-ttu-id="9d3e5-117">Stáhněte a nainstalujte nástroj hello hello odkaz.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-117">Download and install hello tool from hello link.</span></span>

1. <span data-ttu-id="9d3e5-118">Otevřete příkazový řádek a přejděte toohello directory AdlCopy nainstalovanou, obvykle `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-118">Open a command prompt and navigate toohello directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>

2. <span data-ttu-id="9d3e5-119">Spusťte následující příkaz toocopy hello konkrétní objekt blob z hello zdrojový kontejner tooa Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="9d3e5-119">Run hello following command toocopy a specific blob from hello source container tooa Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    <span data-ttu-id="9d3e5-120">Kopírování hello **HVAC.csv** ukázková data souborů **/HdiSamples/HdiSamples/SensorSampleData/TVK/** toohello účtu Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-120">Copy hello **HVAC.csv** sample data file at **/HdiSamples/HdiSamples/SensorSampleData/hvac/** toohello Azure Data Lake Store account.</span></span> <span data-ttu-id="9d3e5-121">fragment kódu Hello by měl vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="9d3e5-121">hello code snippet should look like:</span></span>

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > <span data-ttu-id="9d3e5-122">Zajistěte, aby hello souboru a v případě správné hello jsou názvy cest.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-122">Make sure you hello file and path names are in hello proper case.</span></span>
   >
   >
3. <span data-ttu-id="9d3e5-123">Bude výzvami tooenter hello přihlašovací údaje pro hello předplatné Azure, ve kterém máte účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-123">You will be prompted tooenter hello credentials for hello Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="9d3e5-124">Zobrazí se výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="9d3e5-124">You will see an output similar toohello following:</span></span>

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    <span data-ttu-id="9d3e5-125">Hello datový soubor (**HVAC.csv**) se zkopírují složce **/hvac** v hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-125">hello data file (**HVAC.csv**) will be copied under a folder **/hvac** in hello Data Lake Store account.</span></span>

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a><span data-ttu-id="9d3e5-126">Používání clusteru HDInsight Spark s Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="9d3e5-126">Use an HDInsight Spark cluster with Data Lake Store</span></span>

1. <span data-ttu-id="9d3e5-127">Z hello [portálu Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel).</span><span class="sxs-lookup"><span data-stu-id="9d3e5-127">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="9d3e5-128">Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-128">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>

2. <span data-ttu-id="9d3e5-129">Z okna clusteru Spark hello, klikněte na tlačítko **rychlé odkazy**a potom z hello **řídicí panel clusteru** okně klikněte na tlačítko **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-129">From hello Spark cluster blade, click **Quick Links**, and then from hello **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="9d3e5-130">Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-130">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="9d3e5-131">Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-131">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="9d3e5-132">Nahraďte **CLUSTERNAME** s hello název clusteru:</span><span class="sxs-lookup"><span data-stu-id="9d3e5-132">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. <span data-ttu-id="9d3e5-133">Vytvořte nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-133">Create a new notebook.</span></span> <span data-ttu-id="9d3e5-134">Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-134">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="9d3e5-135">![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Vytvoření nového poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="9d3e5-135">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="9d3e5-136">Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-136">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="9d3e5-137">Hello kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu hello.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-137">hello Spark and Hive contexts will be automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="9d3e5-138">Můžete začít importem typů hello nezbytných pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-138">You can start by importing hello types required for this scenario.</span></span> <span data-ttu-id="9d3e5-139">toodo Ano, vložte následující fragment kódu do buňky a stiskněte klávesu hello **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-139">toodo so, paste hello following code snippet in a cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.sql.types import *

    <span data-ttu-id="9d3e5-140">Při každém spuštění úlohy v Jupyter se název okna webového prohlížeče zobrazí **(zaneprázdněn)** společně s názvem poznámkového bloku hello.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-140">Every time you run a job in Jupyter, your web browser window title will show a **(Busy)** status along with hello notebook title.</span></span> <span data-ttu-id="9d3e5-141">Zobrazí se také další toohello plný kroužek **PySpark** text v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-141">You will also see a solid circle next toohello **PySpark** text in hello top-right corner.</span></span> <span data-ttu-id="9d3e5-142">Po dokončení úlohy hello to změní tooa prázdný kruh.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-142">After hello job is completed, this will change tooa hollow circle.</span></span>

     <span data-ttu-id="9d3e5-143">![Stav úlohy poznámkového bloku Jupyter](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Stav úlohy poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="9d3e5-143">![Status of a Jupyter notebook job](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Status of a Jupyter notebook job")</span></span>

5. <span data-ttu-id="9d3e5-144">Načíst ukázková data do dočasné tabulky pomocí hello **HVAC.csv** souborů, které jste zkopírovali toohello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-144">Load sample data into a temporary table using hello **HVAC.csv** file you copied toohello Data Lake Store account.</span></span> <span data-ttu-id="9d3e5-145">Můžete přistupovat hello dat v účtu Data Lake Store hello pomocí hello následující vzor adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-145">You can access hello data in hello Data Lake Store account using hello following URL pattern.</span></span>

    * <span data-ttu-id="9d3e5-146">Pokud máte Data Lake Store jako výchozí úložiště, HVAC.csv budou v hello cesta podobné toohello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="9d3e5-146">If you have Data Lake Store as default storage, HVAC.csv will be at hello path similar toohello following URL:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        <span data-ttu-id="9d3e5-147">Nebo můžete použít taky zkrácení formátu například hello následující:</span><span class="sxs-lookup"><span data-stu-id="9d3e5-147">Or, you could also use a shortened format such as hello following:</span></span>

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * <span data-ttu-id="9d3e5-148">Pokud máte Data Lake Store jako další úložiště, bude mít HVAC.csv hello umístění, kam jste zkopírovali, jako například:</span><span class="sxs-lookup"><span data-stu-id="9d3e5-148">If you have Data Lake Store as additional storage, HVAC.csv will be at hello location where you copied it, such as:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     <span data-ttu-id="9d3e5-149">V prázdné buňky vložte hello následující ukázka kódu, nahraďte **MYDATALAKESTORE** s názvem účtu Data Lake Store a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-149">In an empty cell, paste hello following code example, replace **MYDATALAKESTORE** with your Data Lake Store account name, and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="9d3e5-150">Tento ukázkový kód registruje hello data do dočasné tabulky nazývané **TVK**.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-150">This code example registers hello data into a temporary table called **hvac**.</span></span>

            # Load hello data. hello path below assumes Data Lake Store is default storage for hello Spark cluster
            hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            # Create hello schema
            hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

            # Parse hello data in hvacText
            hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

            # Create a data frame
            hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

            # Register hello data fram as a table toorun queries against
            hvacdf.registerTempTable("hvac")

6. <span data-ttu-id="9d3e5-151">Vzhledem k tomu, že používáte jádro PySpark, můžete nyní přímo spustit dotaz SQL na dočasnou tabulku hello **TVK** , že jste právě vytvořili pomocí hello `%%sql` magic.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-151">Because you are using a PySpark kernel, you can now directly run a SQL query on hello temporary table **hvac** that you just created by using hello `%%sql` magic.</span></span> <span data-ttu-id="9d3e5-152">Další informace o hello `%%sql` magic a také dalších Magic, které jsou k dispozici s jádrem pyspark hello, najdete v části [jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="9d3e5-152">For more information about hello `%%sql` magic, as well as other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. <span data-ttu-id="9d3e5-153">Po úspěšném dokončení úlohy hello je ve výchozím nastavení zobrazí následující tabulkový výstup hello.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-153">Once hello job is completed successfully, hello following tabular output is displayed by default.</span></span>

      <span data-ttu-id="9d3e5-154">![Tabulkový výstup výsledků dotazu](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Tabulkový výstup výsledků dotazu")</span><span class="sxs-lookup"><span data-stu-id="9d3e5-154">![Table output of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Table output of query result")</span></span>

     <span data-ttu-id="9d3e5-155">Můžete také zjistit hello výsledky v dalších vizualizacích.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-155">You can also see hello results in other visualizations as well.</span></span> <span data-ttu-id="9d3e5-156">Například plošný graf pro stejný výstup bude vypadat hello následující hello.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-156">For example, an area graph for hello same output would look like hello following.</span></span>

     <span data-ttu-id="9d3e5-157">![Plošný graf výsledku dotazu](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Plošný graf výsledku dotazu")</span><span class="sxs-lookup"><span data-stu-id="9d3e5-157">![Area graph of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Area graph of query result")</span></span>

8. <span data-ttu-id="9d3e5-158">Po dokončení spuštění aplikace hello, měli byste vypnout hello poznámkového bloku toorelease hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-158">After you have finished running hello application, you should shutdown hello notebook toorelease hello resources.</span></span> <span data-ttu-id="9d3e5-159">toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-159">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="9d3e5-160">Dojde k vypnutí a zavřít hello Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="9d3e5-160">This will shutdown and close hello notebook.</span></span>


## <a name="next-steps"></a><span data-ttu-id="9d3e5-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d3e5-161">Next steps</span></span>

* [<span data-ttu-id="9d3e5-162">Vytvoření samostatné Scala aplikace toorun na cluster Apache Spark</span><span class="sxs-lookup"><span data-stu-id="9d3e5-162">Create a standalone Scala application toorun on Apache Spark cluster</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="9d3e5-163">Používat nástroje HDInsight pro IntelliJ toocreate aplikací Spark pro cluster HDInsight Spark Linux v Azure nástrojů</span><span class="sxs-lookup"><span data-stu-id="9d3e5-163">Use HDInsight Tools in Azure Toolkit for IntelliJ toocreate Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="9d3e5-164">Použití nástrojů HDInsight v Azure nástrojů Eclipse toocreate Spark aplikací pro cluster HDInsight Spark Linux</span><span class="sxs-lookup"><span data-stu-id="9d3e5-164">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
