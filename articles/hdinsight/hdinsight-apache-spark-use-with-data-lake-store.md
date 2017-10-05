---
title: "Použití Apache Spark k analýze dat v Azure Data Lake Store | Microsoft Docs"
description: "Spuštění úloh Spark k analýze dat uložených v Azure Data Lake Store"
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
ms.openlocfilehash: 66f115c4f348ccaeb8855568ba1ad50faa442173
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-hdinsight-spark-cluster-to-analyze-data-in-data-lake-store"></a><span data-ttu-id="8ed31-103">Použití clusteru HDInsight Spark k analýze dat v Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="8ed31-103">Use HDInsight Spark cluster to analyze data in Data Lake Store</span></span>

<span data-ttu-id="8ed31-104">V tomto kurzu použijete k dispozici poznámkového bloku Jupyter s clustery HDInsight Spark k spustit úlohu, která čte data z účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="8ed31-104">In this tutorial, you use Jupyter notebook available with HDInsight Spark clusters to run a job that reads data from a Data Lake Store account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ed31-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8ed31-105">Prerequisites</span></span>

* <span data-ttu-id="8ed31-106">Účet Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="8ed31-106">Azure Data Lake Store account.</span></span> <span data-ttu-id="8ed31-107">Postupujte podle pokynů v tématu [Začínáme s Azure Data Lake Store s použitím webu Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8ed31-107">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

* <span data-ttu-id="8ed31-108">Cluster Azure HDInsight Spark s Data Lake Store jako úložiště.</span><span class="sxs-lookup"><span data-stu-id="8ed31-108">Azure HDInsight Spark cluster with Data Lake Store as storage.</span></span> <span data-ttu-id="8ed31-109">Postupujte podle pokynů v [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8ed31-109">Follow the instructions at [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    
## <a name="prepare-the-data"></a><span data-ttu-id="8ed31-110">Příprava dat</span><span class="sxs-lookup"><span data-stu-id="8ed31-110">Prepare the data</span></span>

> [!NOTE]
> <span data-ttu-id="8ed31-111">Není nutné k provedení tohoto kroku, pokud jste vytvořili clusteru HDInsight s Data Lake Store jako výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="8ed31-111">You do not need to perform this step if you have created the HDInsight cluster with Data Lake Store as default storage.</span></span> <span data-ttu-id="8ed31-112">Procesy vytváření clusteru přidá ukázková data v účtu Data Lake Store, který zadáte při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="8ed31-112">The cluster creation processes adds some sample data in the Data Lake Store account that you specify while creating the cluster.</span></span> <span data-ttu-id="8ed31-113">Přeskočit k části [clusteru používejte HDInsight Spark s Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="8ed31-113">Skip to the section [Use HDInsight Spark cluster with Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span></span>
>
>

<span data-ttu-id="8ed31-114">Pokud jste vytvořili clusteru HDInsight s Data Lake Store jako další úložiště a Azure Blob Storage jako výchozí úložiště, měli byste nejprve zkopírovat přes ukázková data do účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="8ed31-114">If you created an HDInsight cluster with Data Lake Store as additional storage and Azure Storage Blob as default storage, you should first copy over some sample data to the Data Lake Store account.</span></span> <span data-ttu-id="8ed31-115">Můžete použít ukázkových dat z Azure Storage Blob přidruženého ke clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8ed31-115">You can use the sample data from the Azure Storage Blob associated with the HDInsight cluster.</span></span> <span data-ttu-id="8ed31-116">Můžete použít [ADLCopy nástroj](http://aka.ms/downloadadlcopy) Uděláte to tak.</span><span class="sxs-lookup"><span data-stu-id="8ed31-116">You can use the [ADLCopy tool](http://aka.ms/downloadadlcopy) to do so.</span></span> <span data-ttu-id="8ed31-117">Stáhněte a nainstalujte nástroj z odkazu.</span><span class="sxs-lookup"><span data-stu-id="8ed31-117">Download and install the tool from the link.</span></span>

1. <span data-ttu-id="8ed31-118">Otevřete příkazový řádek a přejděte do adresáře, kde AdlCopy je nainstalován, obvykle `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="8ed31-118">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>

2. <span data-ttu-id="8ed31-119">Spusťte následující příkaz pro kopírování konkrétní objekt blob z kontejneru zdroje do Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="8ed31-119">Run the following command to copy a specific blob from the source container to a Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    <span data-ttu-id="8ed31-120">Kopírování **HVAC.csv** ukázková data souborů **/HdiSamples/HdiSamples/SensorSampleData/TVK/** k účtu Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="8ed31-120">Copy the **HVAC.csv** sample data file at **/HdiSamples/HdiSamples/SensorSampleData/hvac/** to the Azure Data Lake Store account.</span></span> <span data-ttu-id="8ed31-121">Fragment kódu by měla vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="8ed31-121">The code snippet should look like:</span></span>

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > <span data-ttu-id="8ed31-122">Nezapomeňte, které jsou názvy souborů a cestu v případě, že správné.</span><span class="sxs-lookup"><span data-stu-id="8ed31-122">Make sure you the file and path names are in the proper case.</span></span>
   >
   >
3. <span data-ttu-id="8ed31-123">Zobrazí se výzva k zadání přihlašovacích údajů pro předplatné Azure, ve kterém máte účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="8ed31-123">You will be prompted to enter the credentials for the Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="8ed31-124">Zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="8ed31-124">You will see an output similar to the following:</span></span>

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    <span data-ttu-id="8ed31-125">Datový soubor (**HVAC.csv**) se zkopírují složce **/hvac** v účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="8ed31-125">The data file (**HVAC.csv**) will be copied under a folder **/hvac** in the Data Lake Store account.</span></span>

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a><span data-ttu-id="8ed31-126">Používání clusteru HDInsight Spark s Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="8ed31-126">Use an HDInsight Spark cluster with Data Lake Store</span></span>

1. <span data-ttu-id="8ed31-127">Z [Portálu Azure](https://portal.azure.com/) z úvodního panelu klikněte na dlaždici pro váš cluster Spark (pokud je připnutý na úvodní panel).</span><span class="sxs-lookup"><span data-stu-id="8ed31-127">From the [Azure Portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="8ed31-128">Můžete také přejít na cluster pod položkou **Procházet vše** > **Clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="8ed31-128">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>

2. <span data-ttu-id="8ed31-129">Z okna clusteru Spark klikněte na tlačítko **Rychlé odkazy** a pak z okna **Řídicí panel clusteru** klikněte na tlačítko **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="8ed31-129">From the Spark cluster blade, click **Quick Links**, and then from the **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="8ed31-130">Po vyzvání zadejte přihlašovací údaje správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="8ed31-130">If prompted, enter the admin credentials for the cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8ed31-131">Může také otevřít poznámkový blok Jupyter pro váš cluster tak, že otevřete následující adresu URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="8ed31-131">You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="8ed31-132">Nahraďte **CLUSTERNAME** názvem clusteru:</span><span class="sxs-lookup"><span data-stu-id="8ed31-132">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. <span data-ttu-id="8ed31-133">Vytvořte nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="8ed31-133">Create a new notebook.</span></span> <span data-ttu-id="8ed31-134">Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="8ed31-134">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="8ed31-135">![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Vytvoření nového poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="8ed31-135">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="8ed31-136">Vzhledem k tomu, že jste poznámkový blok vytvořili pomocí jádra PySpark, není nutné explicitně tvořit kontexty.</span><span class="sxs-lookup"><span data-stu-id="8ed31-136">Because you created a notebook using the PySpark kernel, you do not need to create any contexts explicitly.</span></span> <span data-ttu-id="8ed31-137">Kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu.</span><span class="sxs-lookup"><span data-stu-id="8ed31-137">The Spark and Hive contexts will be automatically created for you when you run the first code cell.</span></span> <span data-ttu-id="8ed31-138">Můžete začít importem typů nezbytných pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="8ed31-138">You can start by importing the types required for this scenario.</span></span> <span data-ttu-id="8ed31-139">Chcete-li tak učinit, vložte následující fragment kódu do buňky a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="8ed31-139">To do so, paste the following code snippet in a cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.sql.types import *

    <span data-ttu-id="8ed31-140">Při každém spuštění úlohy v Jupyter se název okna webového prohlížeče zobrazí jako **(Zaneprázdněn)** společně s názvem poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="8ed31-140">Every time you run a job in Jupyter, your web browser window title will show a **(Busy)** status along with the notebook title.</span></span> <span data-ttu-id="8ed31-141">Zobrazí se také plný kroužek vedle textu **PySpark** v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="8ed31-141">You will also see a solid circle next to the **PySpark** text in the top-right corner.</span></span> <span data-ttu-id="8ed31-142">Po dokončení úlohy se změní na prázdný kruh.</span><span class="sxs-lookup"><span data-stu-id="8ed31-142">After the job is completed, this will change to a hollow circle.</span></span>

     <span data-ttu-id="8ed31-143">![Stav úlohy poznámkového bloku Jupyter](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Stav úlohy poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="8ed31-143">![Status of a Jupyter notebook job](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Status of a Jupyter notebook job")</span></span>

5. <span data-ttu-id="8ed31-144">Načíst ukázková data do dočasné tabulky pomocí **HVAC.csv** soubor zkopírován do účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="8ed31-144">Load sample data into a temporary table using the **HVAC.csv** file you copied to the Data Lake Store account.</span></span> <span data-ttu-id="8ed31-145">Měli přístup k datům v účtu Data Lake Store pomocí následující vzor adresy URL.</span><span class="sxs-lookup"><span data-stu-id="8ed31-145">You can access the data in the Data Lake Store account using the following URL pattern.</span></span>

    * <span data-ttu-id="8ed31-146">Pokud máte Data Lake Store jako výchozí úložiště, HVAC.csv bude v cestě podobná následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="8ed31-146">If you have Data Lake Store as default storage, HVAC.csv will be at the path similar to the following URL:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        <span data-ttu-id="8ed31-147">Nebo můžete použít taky zkrácení formátu například následující:</span><span class="sxs-lookup"><span data-stu-id="8ed31-147">Or, you could also use a shortened format such as the following:</span></span>

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * <span data-ttu-id="8ed31-148">Pokud máte Data Lake Store jako další úložiště, HVAC.csv bude v umístění, kam jste zkopírovali, jako například:</span><span class="sxs-lookup"><span data-stu-id="8ed31-148">If you have Data Lake Store as additional storage, HVAC.csv will be at the location where you copied it, such as:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     <span data-ttu-id="8ed31-149">Do prázdné buňky vložte následující příklad kódu, nahraďte **MYDATALAKESTORE** s názvem účtu Data Lake Store a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="8ed31-149">In an empty cell, paste the following code example, replace **MYDATALAKESTORE** with your Data Lake Store account name, and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="8ed31-150">Tento ukázkový kód registruje data do dočasné tabulky nazývané **TVK**.</span><span class="sxs-lookup"><span data-stu-id="8ed31-150">This code example registers the data into a temporary table called **hvac**.</span></span>

            # Load the data. The path below assumes Data Lake Store is default storage for the Spark cluster
            hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            # Create the schema
            hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

            # Parse the data in hvacText
            hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

            # Create a data frame
            hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

            # Register the data fram as a table to run queries against
            hvacdf.registerTempTable("hvac")

6. <span data-ttu-id="8ed31-151">Vzhledem k tomu, že používáte jádro PySpark, můžete nyní přímo spustit dotaz SQL na dočasnou tabulku **TVK**, kterou jste právě vytvořili pomocí `%%sql` magic.</span><span class="sxs-lookup"><span data-stu-id="8ed31-151">Because you are using a PySpark kernel, you can now directly run a SQL query on the temporary table **hvac** that you just created by using the `%%sql` magic.</span></span> <span data-ttu-id="8ed31-152">Další informace o `%%sql` magic a také dalších magic, které jsou k dispozici s jádrem PySpark, naleznete v části [Jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="8ed31-152">For more information about the `%%sql` magic, as well as other magics available with the PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. <span data-ttu-id="8ed31-153">Po úspěšném dokončení úlohy se ve výchozím nastavení zobrazí následující tabulkový výstup.</span><span class="sxs-lookup"><span data-stu-id="8ed31-153">Once the job is completed successfully, the following tabular output is displayed by default.</span></span>

      <span data-ttu-id="8ed31-154">![Tabulkový výstup výsledků dotazu](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Tabulkový výstup výsledků dotazu")</span><span class="sxs-lookup"><span data-stu-id="8ed31-154">![Table output of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Table output of query result")</span></span>

     <span data-ttu-id="8ed31-155">Výsledky můžete také zobrazit v dalších vizualizacích.</span><span class="sxs-lookup"><span data-stu-id="8ed31-155">You can also see the results in other visualizations as well.</span></span> <span data-ttu-id="8ed31-156">Například plošný graf pro stejný výstup bude vypadat následovně.</span><span class="sxs-lookup"><span data-stu-id="8ed31-156">For example, an area graph for the same output would look like the following.</span></span>

     <span data-ttu-id="8ed31-157">![Plošný graf výsledku dotazu](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Plošný graf výsledku dotazu")</span><span class="sxs-lookup"><span data-stu-id="8ed31-157">![Area graph of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Area graph of query result")</span></span>

8. <span data-ttu-id="8ed31-158">Po dokončení spuštění aplikace byste měli poznámkový blok vypnout a uvolnit tak prostředky.</span><span class="sxs-lookup"><span data-stu-id="8ed31-158">After you have finished running the application, you should shutdown the notebook to release the resources.</span></span> <span data-ttu-id="8ed31-159">To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku.</span><span class="sxs-lookup"><span data-stu-id="8ed31-159">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span> <span data-ttu-id="8ed31-160">Dojde k vypnutí a zavření poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="8ed31-160">This will shutdown and close the notebook.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8ed31-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8ed31-161">Next steps</span></span>

* [<span data-ttu-id="8ed31-162">Vytvořit samostatný spuštění v clusteru Apache Spark Scala aplikace</span><span class="sxs-lookup"><span data-stu-id="8ed31-162">Create a standalone Scala application to run on Apache Spark cluster</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="8ed31-163">Použití nástrojů HDInsight v Azure nástrojů pro IntelliJ k vytvoření aplikací Spark pro cluster HDInsight Spark Linux</span><span class="sxs-lookup"><span data-stu-id="8ed31-163">Use HDInsight Tools in Azure Toolkit for IntelliJ to create Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="8ed31-164">Použití nástrojů HDInsight v Azure nástrojů pro Eclipse k vytvoření aplikací Spark pro cluster HDInsight Spark Linux</span><span class="sxs-lookup"><span data-stu-id="8ed31-164">Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
