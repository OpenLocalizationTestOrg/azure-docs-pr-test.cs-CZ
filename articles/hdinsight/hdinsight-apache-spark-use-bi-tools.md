---
title: "Spark BI pomocí nástrojů vizualizace data v Azure HDInsight | Microsoft Docs"
description: "Pomocí nástrojů vizualizace dat pro analýzu pomocí Apache Spark BI v clusterech prostředí HDInsight"
keywords: Apache spark bi, spark bi vizualizace dat spark, spark business intelligence
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 49dd161049ac442081fe6d26cf8bd3a56a2e0687
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a><span data-ttu-id="93ed3-104">Apache Spark BI nástroje vizualizaci dat pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="93ed3-104">Apache Spark BI using data visualization tools with Azure HDInsight</span></span>

<span data-ttu-id="93ed3-105">Další informace o použití nástroje vizualizaci dat, jako je Power BI a Tableau k analýze nezpracovaná ukázkové sady dat pomocí Apache Spark BI v clusterech prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="93ed3-105">Learn how to use data visualization tools such as Power BI and Tableau to analyze a raw sample data set using Apache Spark BI on HDInsight clusters.</span></span>

> [!NOTE]
> <span data-ttu-id="93ed3-106">2.1 Spark v Azure HDInsight 3.6 Preview nepodporuje připojení pomocí nástrojů BI, které jsou popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="93ed3-106">Connectivity with BI tools described in this article is not supported on Spark 2.1 on Azure HDInsight 3.6 Preview.</span></span> <span data-ttu-id="93ed3-107">Pouze Spark verze 1.6 a 2.0 (HDInsight 3,4, 3.5 v uvedeném pořadí) jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="93ed3-107">Only Spark versions 1.6 and 2.0 (HDInsight 3.4, 3.5 respectively) are supported.</span></span>
>

<span data-ttu-id="93ed3-108">V tomto kurzu je také k dispozici jako poznámkový blok Jupyter v clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="93ed3-108">This tutorial is also available as a Jupyter notebook on an HDInsight Spark cluster.</span></span> <span data-ttu-id="93ed3-109">Poznámkový blok prostředí vám umožní spustit fragmenty Python ze samotného poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="93ed3-109">The notebook experience lets you run the Python snippets from the notebook itself.</span></span> <span data-ttu-id="93ed3-110">Chcete-li provést kurzem z v rámci Poznámkový blok, vytvořte Spark cluster, spusťte poznámkového bloku Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a poté spusťte poznámkového bloku **nástrojů BI použití s Apache Spark v HDInsight.ipynb** v části **Python** složky.</span><span class="sxs-lookup"><span data-stu-id="93ed3-110">To perform the tutorial from within a notebook, create a Spark cluster, launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), and then run the notebook **Use BI tools with Apache Spark on HDInsight.ipynb** under the **Python** folder.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93ed3-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="93ed3-111">Prerequisites</span></span>

* <span data-ttu-id="93ed3-112">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="93ed3-112">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="93ed3-113">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="93ed3-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>


## <span data-ttu-id="93ed3-114"><a name="hivetable"></a>Příprava dat pro vizualizaci dat Spark</span><span class="sxs-lookup"><span data-stu-id="93ed3-114"><a name="hivetable"></a>Prepare data for Spark data visualization</span></span>

<span data-ttu-id="93ed3-115">V této části používáme [Jupyter](https://jupyter.org) poznámkového bloku z clusteru služby HDInsight Spark ke spuštění úloh, které zpracování nezpracovaných ukázkových dat a uložte ho jako tabulku.</span><span class="sxs-lookup"><span data-stu-id="93ed3-115">In this section, we use the [Jupyter](https://jupyter.org) notebook from an HDInsight Spark cluster to run jobs that process your raw sample data and save it as a table.</span></span> <span data-ttu-id="93ed3-116">Ukázková data je soubor .csv (hvac.csv) k dispozici na všech clusterech ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="93ed3-116">The sample data is a .csv file (hvac.csv) available on all clusters by default.</span></span> <span data-ttu-id="93ed3-117">Vaše data uložena jako tabulku, v další části používáme nástrojů BI k připojení do tabulky a vizualizaci dat.</span><span class="sxs-lookup"><span data-stu-id="93ed3-117">Once your data is saved as a table, in the next section we use BI tools to connect to the table and perform data visualizations.</span></span>

> [!NOTE]
> <span data-ttu-id="93ed3-118">Pokud po dokončení podle pokynů v provádíte kroky v tomto článku [Spusťte interaktivní dotazy na clusteru HDInsight Spark](hdinsight-apache-spark-load-data-run-query.md), můžete přeskočit na krok 8 níže.</span><span class="sxs-lookup"><span data-stu-id="93ed3-118">If you are performing the steps in this article after completing the instructions in [Run interactive queries on an HDInsight Spark cluster](hdinsight-apache-spark-load-data-run-query.md), you can skip to Step 8 below.</span></span>
>

1. <span data-ttu-id="93ed3-119">Z portálu [Azure Portal](https://portal.azure.com/) z úvodního panelu klikněte na dlaždici pro váš cluster Spark (pokud je připnutý na úvodní panel).</span><span class="sxs-lookup"><span data-stu-id="93ed3-119">From the [Azure portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="93ed3-120">Můžete také přejít na cluster pod položkou **Procházet vše** > **Clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-120">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>   

2. <span data-ttu-id="93ed3-121">Z okna clusteru Spark klikněte na **Řídicí panel clusteru** a poté na **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-121">From the Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="93ed3-122">Po vyzvání zadejte přihlašovací údaje správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="93ed3-122">If prompted, enter the admin credentials for the cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="93ed3-123">Může také otevřít poznámkový blok Jupyter pro váš cluster tak, že otevřete následující adresu URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="93ed3-123">You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="93ed3-124">Nahraďte **CLUSTERNAME** názvem clusteru:</span><span class="sxs-lookup"><span data-stu-id="93ed3-124">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. <span data-ttu-id="93ed3-125">Vytvořte poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="93ed3-125">Create a notebook.</span></span> <span data-ttu-id="93ed3-126">Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-126">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="93ed3-127">![Vytvoření poznámkového bloku Jupyter pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "vytvoření poznámkového bloku Jupyter pro Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-127">![Create a Jupyter notebook for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "Create a Jupyter notebook for Apache Spark BI")</span></span>

4. <span data-ttu-id="93ed3-128">Nový poznámkový blok se vytvoří a otevře s názvem Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="93ed3-128">A new notebook is created and opened with the name Untitled.pynb.</span></span> <span data-ttu-id="93ed3-129">Klikněte na název poznámkového bloku v horní části a zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="93ed3-129">Click the notebook name at the top, and enter a friendly name.</span></span>

    <span data-ttu-id="93ed3-130">![Zadejte název poznámkového bloku pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "zadejte název poznámkového bloku pro Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-130">![Provide a name for the notebook for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "Provide a name for the notebook for Apache Spark BI")</span></span>

5. <span data-ttu-id="93ed3-131">Vzhledem k tomu, že jste poznámkový blok vytvořili pomocí jádra PySpark, není nutné explicitně tvořit kontexty.</span><span class="sxs-lookup"><span data-stu-id="93ed3-131">Because you created a notebook using the PySpark kernel, you do not need to create any contexts explicitly.</span></span> <span data-ttu-id="93ed3-132">Kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu.</span><span class="sxs-lookup"><span data-stu-id="93ed3-132">The Spark and Hive contexts are automatically created for you when you run the first code cell.</span></span> <span data-ttu-id="93ed3-133">Můžete začít importem typů nezbytných pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="93ed3-133">You can start by importing the types required for this scenario.</span></span> <span data-ttu-id="93ed3-134">Uděláte to tak, umístěte kurzor do buňky a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-134">To do so, place the cursor in the cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.sql import *

6. <span data-ttu-id="93ed3-135">Načtěte vzorová data do dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="93ed3-135">Load sample data into a temporary table.</span></span> <span data-ttu-id="93ed3-136">Když vytvoříte cluster Spark v HDInsight, ukázkový datový soubor **hvac.csv** se zkopíruje do přidruženého účtu úložiště na adrese **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-136">When you create a Spark cluster in HDInsight, the sample data file, **hvac.csv**, is copied to the associated storage account under **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span></span>

    <span data-ttu-id="93ed3-137">Do prázdné buňky vložte následující fragment kódu a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-137">In an empty cell, paste the following snippet and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="93ed3-138">Tento fragment kódu registruje data do tabulce s názvem **TVK**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-138">This snippet registers the data into a table called **hvac**.</span></span>

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse the data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))

        # Infer the schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. <span data-ttu-id="93ed3-139">Ověřte, že tabulka byla úspěšně vytvořena.</span><span class="sxs-lookup"><span data-stu-id="93ed3-139">Verify that the table was successfully created.</span></span> <span data-ttu-id="93ed3-140">Můžete použít `%%sql` magic ke spuštění Hive dotazuje přímo.</span><span class="sxs-lookup"><span data-stu-id="93ed3-140">You can use the `%%sql` magic to run Hive queries directly.</span></span> <span data-ttu-id="93ed3-141">Další informace o magických příkazech `%%sql` a dalších magických příkazech, které jsou k dispozici s jádrem PySpark, najdete v části [Jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="93ed3-141">For more information about the `%%sql` magic, and other magics available with the PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

        %%sql
        SHOW TABLES

    <span data-ttu-id="93ed3-142">Zobrazí výstup jako vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="93ed3-142">You see an output like shown below:</span></span>

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    <span data-ttu-id="93ed3-143">Pouze tabulky, které mají hodnotu false v rámci **isTemporary** sloupce jsou tabulek hive, které jsou uložené v metaúložiště a je přístupný z nástrojů BI.</span><span class="sxs-lookup"><span data-stu-id="93ed3-143">Only the tables that have false under the **isTemporary** column are hive tables that are stored in the metastore and can be accessed from the BI tools.</span></span> <span data-ttu-id="93ed3-144">V tomto kurzu jsme se připojit ke **TVK** jsme vytvořili tabulku.</span><span class="sxs-lookup"><span data-stu-id="93ed3-144">In this tutorial, we connect to the **hvac** table we created.</span></span>

8. <span data-ttu-id="93ed3-145">Ověřte, zda tabulka obsahuje určený data.</span><span class="sxs-lookup"><span data-stu-id="93ed3-145">Verify that the table contains the intended data.</span></span> <span data-ttu-id="93ed3-146">Do prázdné buňky v poznámkovém bloku, zkopírujte následující fragment kódu a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-146">In an empty cell in the notebook, copy the following snippet and press **SHIFT + ENTER**.</span></span>

        %%sql
        SELECT * FROM hvac LIMIT 10

9. <span data-ttu-id="93ed3-147">Vypněte poznámkového bloku k uvolnění prostředků.</span><span class="sxs-lookup"><span data-stu-id="93ed3-147">Shut down the notebook to release the resources.</span></span> <span data-ttu-id="93ed3-148">To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku.</span><span class="sxs-lookup"><span data-stu-id="93ed3-148">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span>

## <span data-ttu-id="93ed3-149"><a name="powerbi"></a>Pomocí Power BI pro vizualizaci dat Spark</span><span class="sxs-lookup"><span data-stu-id="93ed3-149"><a name="powerbi"></a>Use Power BI for Spark data visualization</span></span>

> [!NOTE]
> <span data-ttu-id="93ed3-150">Tato část je určena pouze pro 1.6 Spark na HDInsight 3.4 a 2.0 Spark na HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="93ed3-150">This section is applicable only for Spark 1.6 on HDInsight 3.4 and Spark 2.0 on HDInsight 3.5.</span></span>
>
>

<span data-ttu-id="93ed3-151">Po uložení dat jako tabulku, můžete Power BI a připojte se k datům vizualizovat má vytvořit sestavy, řídicí panely, atd.</span><span class="sxs-lookup"><span data-stu-id="93ed3-151">Once you have saved the data as a table, you can use Power BI to connect to the data and visualize it to create reports, dashboards, etc.</span></span>

1. <span data-ttu-id="93ed3-152">Zkontrolujte, zda že máte přístup k Power BI.</span><span class="sxs-lookup"><span data-stu-id="93ed3-152">Make sure you have access to Power BI.</span></span> <span data-ttu-id="93ed3-153">Můžete získat bezplatnou verzi preview odběr služby Power BI z [http://www.powerbi.com/](http://www.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="93ed3-153">You can get a free preview subscription of Power BI from [http://www.powerbi.com/](http://www.powerbi.com/).</span></span>

2. <span data-ttu-id="93ed3-154">Přihlaste se k [Power BI](http://www.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="93ed3-154">Sign in to [Power BI](http://www.powerbi.com/).</span></span>

3. <span data-ttu-id="93ed3-155">V dolní části v levém podokně klikněte na tlačítko **načíst Data**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-155">From the bottom of the left pane, click **Get Data**.</span></span>

4. <span data-ttu-id="93ed3-156">Na **načíst Data** v části **Import nebo připojení k datům**, pro **databáze**, klikněte na tlačítko **získat**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-156">On the **Get Data** page, under **Import or Connect to Data**, for **Databases**, click **Get**.</span></span>

    <span data-ttu-id="93ed3-157">![Získání dat do Power BI pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "načíst data do Power BI pro Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-157">![Get data into Power BI for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Get data into Power BI for Apache Spark BI")</span></span>

5. <span data-ttu-id="93ed3-158">Na další obrazovce klikněte na tlačítko **Spark v Azure HDInsight** a pak klikněte na **Connect**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-158">On the next screen, click **Spark on Azure HDInsight** and then click **Connect**.</span></span> <span data-ttu-id="93ed3-159">Po zobrazení výzvy zadejte adresu URL clusteru (`mysparkcluster.azurehdinsight.net`) a pověření pro připojení ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="93ed3-159">When prompted, enter the cluster URL (`mysparkcluster.azurehdinsight.net`) and the credentials to connect to the cluster.</span></span>

    <span data-ttu-id="93ed3-160">![Připojení k serveru Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "připojit k serveru Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-160">![Connect to Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "Connect to Apache Spark BI")</span></span>

    <span data-ttu-id="93ed3-161">Po navázání připojení Power BI spustí import dat z clusteru Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="93ed3-161">After the connection is established, Power BI starts importing data from the Spark cluster on HDInsight.</span></span>

6. <span data-ttu-id="93ed3-162">Power BI naimportuje data a přidá **Spark** datovou sadu v části **datové sady** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="93ed3-162">Power BI imports the data and adds a **Spark** dataset under the **Datasets** heading.</span></span> <span data-ttu-id="93ed3-163">Klikněte na datovou sadu otevřete do nového listu, která bude vizualizovat data.</span><span class="sxs-lookup"><span data-stu-id="93ed3-163">Click the data set to open a new worksheet to visualize the data.</span></span> <span data-ttu-id="93ed3-164">Můžete také list uložíte jako sestavu.</span><span class="sxs-lookup"><span data-stu-id="93ed3-164">You can also save the worksheet as a report.</span></span> <span data-ttu-id="93ed3-165">Uložení na listu z **soubor** nabídky, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-165">To save a worksheet, from the **File** menu, click **Save**.</span></span>

    <span data-ttu-id="93ed3-166">![Apache Spark BI dlaždice na řídicím panelu Power BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI dlaždice na řídicím panelu Power BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-166">![Apache Spark BI tile on Power BI dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI tile on Power BI dashboard")</span></span>
7. <span data-ttu-id="93ed3-167">Všimněte si, že **pole** seznamu na pravé seznamy **TVK** tabulky, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="93ed3-167">Notice that the **Fields** list on the right lists the **hvac** table you created earlier.</span></span> <span data-ttu-id="93ed3-168">Rozbalte položku v tabulce najdete pole v tabulce, jak jsou definovány v poznámkovém bloku dříve.</span><span class="sxs-lookup"><span data-stu-id="93ed3-168">Expand the table to see the fields in the table, as you defined in notebook earlier.</span></span>

      <span data-ttu-id="93ed3-169">![Seznam tabulek na řídicí panel Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "seznam tabulek na řídicí panel Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-169">![List tables on Apache Spark BI dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "List tables on Apache Spark BI dashboard")</span></span>

8. <span data-ttu-id="93ed3-170">Vytvořte vizualizaci zobrazit odchylka mezi teploty cíl a skutečný teploty pro každé sestavení.</span><span class="sxs-lookup"><span data-stu-id="93ed3-170">Build a visualization to show the variance between target temperature and actual temperature for each building.</span></span> <span data-ttu-id="93ed3-171">K vizualizaci dat chtěli, vyberte **plošný graf** (zobrazené v červeným rámečkem).</span><span class="sxs-lookup"><span data-stu-id="93ed3-171">To visualize yoru data, select **Area Chart** (shown in red box).</span></span> <span data-ttu-id="93ed3-172">K definování ose přetažení myší **BuildingID** pole v části **osy**, a **ActualTemp**/**TargetTemp** pole v části **hodnotu**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-172">To define the axis, drag-and-drop the **BuildingID** field under **Axis**, and **ActualTemp**/**TargetTemp** fields under **Value**.</span></span>

    <span data-ttu-id="93ed3-173">![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-173">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Create Spark data visualizations using Apache Spark BI")</span></span>

9. <span data-ttu-id="93ed3-174">Ve výchozím nastavení zobrazuje vizualizaci součet **ActualTemp** a **TargetTemp**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-174">By default the visualization shows the sum for **ActualTemp** and **TargetTemp**.</span></span> <span data-ttu-id="93ed3-175">Pro obě pole, z rozevíracího seznamu vyberte **průměrná** získat v průměru skutečné a teploty cíl pro obě budovy.</span><span class="sxs-lookup"><span data-stu-id="93ed3-175">For both the fields, from the drop-down, select **Average** to get an average of actual and target temperatures for both buildings.</span></span>

    <span data-ttu-id="93ed3-176">![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-176">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Create Spark data visualizations using Apache Spark BI")</span></span>

10. <span data-ttu-id="93ed3-177">Vaše vizualizace dat by měla být podobné na snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="93ed3-177">Your data visualization should be similar to the one in the screenshot.</span></span> <span data-ttu-id="93ed3-178">Přesuňte kurzor vizualizaci zobrazíte popisy s příslušnými daty.</span><span class="sxs-lookup"><span data-stu-id="93ed3-178">Move your cursor over the visualization to get tool tips with relevant data.</span></span>

    <span data-ttu-id="93ed3-179">![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-179">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Create Spark data visualizations using Apache Spark BI")</span></span>

11. <span data-ttu-id="93ed3-180">Klikněte na tlačítko **Uložit** v hlavní nabídce a zadejte název sestavy.</span><span class="sxs-lookup"><span data-stu-id="93ed3-180">Click **Save** from the top menu and provide a report name.</span></span> <span data-ttu-id="93ed3-181">Můžete taky připnout vizuál.</span><span class="sxs-lookup"><span data-stu-id="93ed3-181">You can also pin the visual.</span></span> <span data-ttu-id="93ed3-182">Pokud připnete vizualizace, je uložený na řídicím panelu, můžete sledovat nejpozdější hodnotu na první pohled.</span><span class="sxs-lookup"><span data-stu-id="93ed3-182">When you pin a visualization, it is stored on your dashboard so you can track the latest value at a glance.</span></span>

   <span data-ttu-id="93ed3-183">Můžete přidat tolik vizualizace, jako má stejné datové sady a jejich připnout na řídicí panel pro snímek vaše data.</span><span class="sxs-lookup"><span data-stu-id="93ed3-183">You can add as many visualizations as you want for the same dataset and pin them to the dashboard for a snapshot of your data.</span></span> <span data-ttu-id="93ed3-184">Navíc clustery Spark v HDInsight jsou připojená k Power BI s protokolem direct connect.</span><span class="sxs-lookup"><span data-stu-id="93ed3-184">Also, Spark clusters on HDInsight are connected to Power BI with direct connect.</span></span> <span data-ttu-id="93ed3-185">Tím se zajistí, že Power BI vždy obsahuje nejnovější data z clusteru, takže není potřeba naplánovat aktualizace pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="93ed3-185">This ensures that Power BI always has the most up-to-date data from your cluster so you do not need to schedule refreshes for the dataset.</span></span>

## <span data-ttu-id="93ed3-186"><a name="tableau"></a>Použijte plochu Tableau pro vizualizaci dat Spark</span><span class="sxs-lookup"><span data-stu-id="93ed3-186"><a name="tableau"></a>Use Tableau Desktop for Spark data visualization</span></span>

> [!NOTE]
> <span data-ttu-id="93ed3-187">Tato část je určena pouze pro clustery Spark 1.5.2 vytvořené v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="93ed3-187">This section is applicable only for Spark 1.5.2 clusters created in Azure HDInsight.</span></span>
>
>

1. <span data-ttu-id="93ed3-188">Nainstalujte [Tableau plochy](http://www.tableau.com/products/desktop) na počítači, na kterém je spuštěn v tomto kurzu Apache Spark BI.</span><span class="sxs-lookup"><span data-stu-id="93ed3-188">Install [Tableau Desktop](http://www.tableau.com/products/desktop) on the computer where you are running this Apache Spark BI tutorial.</span></span>

2. <span data-ttu-id="93ed3-189">Ujistěte se, že tento počítač má také nainstalovaný ovladač Microsoft Spark ODBC.</span><span class="sxs-lookup"><span data-stu-id="93ed3-189">Make sure that computer also has Microsoft Spark ODBC driver installed.</span></span> <span data-ttu-id="93ed3-190">Můžete nainstalovat ovladač z [zde](http://go.microsoft.com/fwlink/?LinkId=616229).</span><span class="sxs-lookup"><span data-stu-id="93ed3-190">You can install the driver from [here](http://go.microsoft.com/fwlink/?LinkId=616229).</span></span>

1. <span data-ttu-id="93ed3-191">Spuštění Tableau plochy.</span><span class="sxs-lookup"><span data-stu-id="93ed3-191">Launch Tableau Desktop.</span></span> <span data-ttu-id="93ed3-192">V levém podokně, ze seznamu serveru pro připojení, klikněte na tlačítko **Spark SQL**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-192">In the left pane, from the list of server to connect to, click **Spark SQL**.</span></span> <span data-ttu-id="93ed3-193">Pokud není ve výchozím nastavení v levém podokně zobrazí Spark SQL, můžete nějakého najít kliknutím **více serverů**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-193">If Spark SQL is not listed by default in the left pane, you can find it by click **More Servers**.</span></span>
2. <span data-ttu-id="93ed3-194">V dialogovém okně připojení Spark SQL zadejte hodnoty, jak je znázorněno na snímku obrazovky a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-194">In the Spark SQL connection dialog box, provide the values as shown in the screenshot, and then click **OK**.</span></span>

    <span data-ttu-id="93ed3-195">![Připojení ke clusteru Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "připojení ke clusteru Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-195">![Connect to a cluster for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect to a cluster for Apache Spark BI")</span></span>

    <span data-ttu-id="93ed3-196">Rozevírací seznamy ověřování **služby Microsoft Azure HDInsight** jako možnost, pouze pokud jste nainstalovali [ovladač ODBC Microsoft Sparku](http://go.microsoft.com/fwlink/?LinkId=616229) v počítači.</span><span class="sxs-lookup"><span data-stu-id="93ed3-196">The authentication drop-down lists **Microsoft Azure HDInsight Service** as an option, only if you installed the [Microsoft Spark ODBC Driver](http://go.microsoft.com/fwlink/?LinkId=616229) on the computer.</span></span>
3. <span data-ttu-id="93ed3-197">Na další obrazovce z **schématu** rozevírací seznam, klikněte **najít** ikonu a pak klikněte na tlačítko **výchozí**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-197">On the next screen, from the **Schema** drop-down, click the **Find** icon, and then click **default**.</span></span>

    <span data-ttu-id="93ed3-198">![Nalezeno schéma pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "najít schéma pro Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-198">![Find schema for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Find schema for Apache Spark BI")</span></span>
4. <span data-ttu-id="93ed3-199">Pro **tabulky** pole, klikněte na tlačítko **najít** ikonu seznam všech tabulek Hive v clusteru k dispozici.</span><span class="sxs-lookup"><span data-stu-id="93ed3-199">For the **Table** field, click the **Find** icon again to list all the Hive tables available in the cluster.</span></span> <span data-ttu-id="93ed3-200">Měli byste vidět **TVK** tabulky, které jste vytvořili dříve pomocí poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="93ed3-200">You should see the **hvac** table you created earlier using the notebook.</span></span>

    <span data-ttu-id="93ed3-201">![Najít tabulku pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "najít tabulku pro Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-201">![Find table for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Find table for Apache Spark BI")</span></span>
5. <span data-ttu-id="93ed3-202">Přetáhnout myší v tabulce nejvyšší pole na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="93ed3-202">Drag and drop the table to the top box on the right.</span></span> <span data-ttu-id="93ed3-203">Tableau naimportuje data a schéma se zobrazuje jako zvýrazněná podle červeným rámečkem.</span><span class="sxs-lookup"><span data-stu-id="93ed3-203">Tableau imports the data and displays the schema as highlighted by the red box.</span></span>

    <span data-ttu-id="93ed3-204">![Přidání tabulky do Tableau pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "přidání tabulky do Tableau pro Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="93ed3-204">![Add tables to Tableau for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Add tables to Tableau for Apache Spark BI")</span></span>
6. <span data-ttu-id="93ed3-205">Klikněte **Sheet1** kartě dole vlevo.</span><span class="sxs-lookup"><span data-stu-id="93ed3-205">Click the **Sheet1** tab at the bottom left.</span></span> <span data-ttu-id="93ed3-206">Ujistěte se, vizualizace, který ukazuje průměrná cíl a skutečný teploty pro všechny budovy pro jednotlivá data.</span><span class="sxs-lookup"><span data-stu-id="93ed3-206">Make a visualization that shows the average target and actual temperatures for all buildings for each date.</span></span> <span data-ttu-id="93ed3-207">Přetáhněte **datum** a **vytváření ID** k **sloupce** a **skutečné Temp**/**cíle Temp** k **řádky**.</span><span class="sxs-lookup"><span data-stu-id="93ed3-207">Drag **Date** and **Building ID** to **Columns** and **Actual Temp**/**Target Temp** to **Rows**.</span></span> <span data-ttu-id="93ed3-208">V části **značky**, vyberte **oblasti** sloužící oblasti mapy pro vizualizaci dat Spark.</span><span class="sxs-lookup"><span data-stu-id="93ed3-208">Under **Marks**, select **Area** to use an area map for Spark data visualization.</span></span>

     <span data-ttu-id="93ed3-209">![Přidat pole pro vizualizaci dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "přidat pole pro vizualizaci dat Spark")</span><span class="sxs-lookup"><span data-stu-id="93ed3-209">![Add fields for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Add fields for Spark data visualization")</span></span>
7. <span data-ttu-id="93ed3-210">Standardně se zobrazují pole teploty jako agregace.</span><span class="sxs-lookup"><span data-stu-id="93ed3-210">By default, the temperature fields are shown as aggregate.</span></span> <span data-ttu-id="93ed3-211">Pokud chcete zobrazit průměrné teploty místo, můžete provést tak z rozevíracího seznamu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="93ed3-211">If you want to show the average temperatures instead, you can do so from the drop-down, as shown below.</span></span>

    <span data-ttu-id="93ed3-212">![Trvat průměrná z teploty pro vizualizaci dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "trvat průměrná z teploty pro vizualizaci dat Spark")</span><span class="sxs-lookup"><span data-stu-id="93ed3-212">![Take average of temperature for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "Take average of temperature for Spark data visualization")</span></span>

8. <span data-ttu-id="93ed3-213">Můžete také super použít jedna mapa teploty z nich získat lepší chování rozdíl mezi cíl a skutečný teploty.</span><span class="sxs-lookup"><span data-stu-id="93ed3-213">You can also super-impose one temperature map over the other to get a better feel of difference between target and actual temperatures.</span></span> <span data-ttu-id="93ed3-214">Přesuňte myš do rohu nižší oblasti mapy, dokud najdete v části obrazce popisovač zvýrazněných v červeném kroužku.</span><span class="sxs-lookup"><span data-stu-id="93ed3-214">Move the mouse to the corner of the lower area map till you see the handle shape highlighted in a red circle.</span></span> <span data-ttu-id="93ed3-215">Přetáhněte mapy do jiných mapy nahoře a uvolnění tlačítka myši, když se tvar zvýrazněných v red obdélníku.</span><span class="sxs-lookup"><span data-stu-id="93ed3-215">Drag the map to the other map on the top and release the mouse when you see the shape highlighted in red rectangle.</span></span>

    <span data-ttu-id="93ed3-216">![Sloučení mapy pro vizualizaci dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "sloučení mapy pro vizualizaci dat Spark")</span><span class="sxs-lookup"><span data-stu-id="93ed3-216">![Merge maps for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Merge maps for Spark data visualization")</span></span>

     <span data-ttu-id="93ed3-217">Vaše vizualizace dat měli změnit, jak je znázorněno na snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="93ed3-217">Your data visualization should change as shown in the screenshot:</span></span>

    <span data-ttu-id="93ed3-218">![Výstup tableau vizualizace dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau výstup vizualizace dat Spark")</span><span class="sxs-lookup"><span data-stu-id="93ed3-218">![Tableau output for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau output for Spark data visualization")</span></span>
9. <span data-ttu-id="93ed3-219">Klikněte na tlačítko **Uložit** uložit listu.</span><span class="sxs-lookup"><span data-stu-id="93ed3-219">Click **Save** to save the worksheet.</span></span> <span data-ttu-id="93ed3-220">Můžete vytvořit řídicí panely a do ní přidejte jeden nebo více stylů.</span><span class="sxs-lookup"><span data-stu-id="93ed3-220">You can create dashboards and add one or more sheets to it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93ed3-221">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93ed3-221">Next steps</span></span>

<span data-ttu-id="93ed3-222">Pokud jste zjistili, jak vytvořit cluster, vytvořit Spark datové rámce k dotazování na data a pak z nástrojů BI přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="93ed3-222">So far you learned how to create a cluster, create Spark data frames to query data, and then access that data from BI tools.</span></span> <span data-ttu-id="93ed3-223">Teď můžete prohlížet pokyny, jak spravovat prostředky clusteru a ladění úloh, které jsou spuštěné v clusteru služby HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="93ed3-223">You can now look at instructions on how to manage the cluster resources and debug jobs that are running in an HDInsight Spark cluster.</span></span>

* [<span data-ttu-id="93ed3-224">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="93ed3-224">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="93ed3-225">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="93ed3-225">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

