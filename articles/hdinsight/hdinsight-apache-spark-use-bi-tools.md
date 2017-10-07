---
title: "aaaSpark BI pomocí nástrojů vizualizace data v Azure HDInsight | Microsoft Docs"
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
ms.openlocfilehash: ba4bfff737ce80ffca5c24f1c2c82a1447f467fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a><span data-ttu-id="ef3b5-104">Apache Spark BI nástroje vizualizaci dat pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ef3b5-104">Apache Spark BI using data visualization tools with Azure HDInsight</span></span>

<span data-ttu-id="ef3b5-105">Zjistěte, jak vizualizace dat toouse nástroje například Power BI a Tableau tooanalyze nezpracovaná ukázkové sady dat pomocí Apache Spark BI v clusterech prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-105">Learn how toouse data visualization tools such as Power BI and Tableau tooanalyze a raw sample data set using Apache Spark BI on HDInsight clusters.</span></span>

> [!NOTE]
> <span data-ttu-id="ef3b5-106">2.1 Spark v Azure HDInsight 3.6 Preview nepodporuje připojení pomocí nástrojů BI, které jsou popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-106">Connectivity with BI tools described in this article is not supported on Spark 2.1 on Azure HDInsight 3.6 Preview.</span></span> <span data-ttu-id="ef3b5-107">Pouze Spark verze 1.6 a 2.0 (HDInsight 3,4, 3.5 v uvedeném pořadí) jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-107">Only Spark versions 1.6 and 2.0 (HDInsight 3.4, 3.5 respectively) are supported.</span></span>
>

<span data-ttu-id="ef3b5-108">V tomto kurzu je také k dispozici jako poznámkový blok Jupyter v clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-108">This tutorial is also available as a Jupyter notebook on an HDInsight Spark cluster.</span></span> <span data-ttu-id="ef3b5-109">Hello poznámkového bloku prostředí vám umožní spustit hello Python fragmenty z Poznámkový blok hello sám sebe.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-109">hello notebook experience lets you run hello Python snippets from hello notebook itself.</span></span> <span data-ttu-id="ef3b5-110">kurz hello tooperform z v rámci Poznámkový blok, vytvořte Spark cluster, spusťte poznámkového bloku Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a poté spusťte Poznámkový blok hello **nástrojů BI použití s Apache Spark v HDInsight.ipynb** pod hello **Python**  složky.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-110">tooperform hello tutorial from within a notebook, create a Spark cluster, launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), and then run hello notebook **Use BI tools with Apache Spark on HDInsight.ipynb** under hello **Python** folder.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef3b5-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ef3b5-111">Prerequisites</span></span>

* <span data-ttu-id="ef3b5-112">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-112">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="ef3b5-113">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="ef3b5-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>


## <span data-ttu-id="ef3b5-114"><a name="hivetable"></a>Příprava dat pro vizualizaci dat Spark</span><span class="sxs-lookup"><span data-stu-id="ef3b5-114"><a name="hivetable"></a>Prepare data for Spark data visualization</span></span>

<span data-ttu-id="ef3b5-115">V této části používáme hello [Jupyter](https://jupyter.org) poznámkového bloku z HDInsight Spark clusteru toorun úloh, které zpracování nezpracovaných ukázkových dat a uložte ho jako tabulku.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-115">In this section, we use hello [Jupyter](https://jupyter.org) notebook from an HDInsight Spark cluster toorun jobs that process your raw sample data and save it as a table.</span></span> <span data-ttu-id="ef3b5-116">Hello ukázkových dat je soubor .csv (hvac.csv) k dispozici na všech clusterech ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-116">hello sample data is a .csv file (hvac.csv) available on all clusters by default.</span></span> <span data-ttu-id="ef3b5-117">Vaše data uložena jako tabulku, v další části hello jsme BI nástroje tooconnect toohello tabulka a provádět vizualizaci dat.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-117">Once your data is saved as a table, in hello next section we use BI tools tooconnect toohello table and perform data visualizations.</span></span>

> [!NOTE]
> <span data-ttu-id="ef3b5-118">Pokud provádíte hello kroky v tomto článku po dokončení hello pokyny v [Spusťte interaktivní dotazy na clusteru HDInsight Spark](hdinsight-apache-spark-load-data-run-query.md), můžete přeskočit tooStep 8 níže.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-118">If you are performing hello steps in this article after completing hello instructions in [Run interactive queries on an HDInsight Spark cluster](hdinsight-apache-spark-load-data-run-query.md), you can skip tooStep 8 below.</span></span>
>

1. <span data-ttu-id="ef3b5-119">Z hello [portál Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel).</span><span class="sxs-lookup"><span data-stu-id="ef3b5-119">From hello [Azure portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="ef3b5-120">Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-120">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   

2. <span data-ttu-id="ef3b5-121">Z okna clusteru Spark hello, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-121">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="ef3b5-122">Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-122">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ef3b5-123">Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-123">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="ef3b5-124">Nahraďte **CLUSTERNAME** s hello název clusteru:</span><span class="sxs-lookup"><span data-stu-id="ef3b5-124">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. <span data-ttu-id="ef3b5-125">Vytvořte poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-125">Create a notebook.</span></span> <span data-ttu-id="ef3b5-126">Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-126">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="ef3b5-127">![Vytvoření poznámkového bloku Jupyter pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "vytvoření poznámkového bloku Jupyter pro Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-127">![Create a Jupyter notebook for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "Create a Jupyter notebook for Apache Spark BI")</span></span>

4. <span data-ttu-id="ef3b5-128">Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-128">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="ef3b5-129">Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-129">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="ef3b5-130">![Zadejte název poznámkového bloku hello Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "zadejte název poznámkového bloku hello Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-130">![Provide a name for hello notebook for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "Provide a name for hello notebook for Apache Spark BI")</span></span>

5. <span data-ttu-id="ef3b5-131">Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-131">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="ef3b5-132">kontexty Spark a Hive Hello jsou pro vás automaticky vytvoří při spuštění první buňky kódu hello.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-132">hello Spark and Hive contexts are automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="ef3b5-133">Můžete začít importem typů hello nezbytných pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-133">You can start by importing hello types required for this scenario.</span></span> <span data-ttu-id="ef3b5-134">toodo Ano, umístěte kurzor hello hello buňky a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-134">toodo so, place hello cursor in hello cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.sql import *

6. <span data-ttu-id="ef3b5-135">Načtěte vzorová data do dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-135">Load sample data into a temporary table.</span></span> <span data-ttu-id="ef3b5-136">Když vytvoříte Spark cluster v HDInsight, ukázkový datový soubor hello, **hvac.csv**, je zkopírovaný toohello přidruženého účtu úložiště **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-136">When you create a Spark cluster in HDInsight, hello sample data file, **hvac.csv**, is copied toohello associated storage account under **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span></span>

    <span data-ttu-id="ef3b5-137">Do prázdné buňky vložte hello následující fragment kódu a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-137">In an empty cell, paste hello following snippet and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="ef3b5-138">Tento fragment kódu zaregistruje hello dat do tabulky názvem **TVK**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-138">This snippet registers hello data into a table called **hvac**.</span></span>

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse hello data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))

        # Infer hello schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. <span data-ttu-id="ef3b5-139">Ověřte, že tato tabulka hello byl úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-139">Verify that hello table was successfully created.</span></span> <span data-ttu-id="ef3b5-140">Můžete použít hello `%%sql` kouzelná dotazů Hive toorun přímo.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-140">You can use hello `%%sql` magic toorun Hive queries directly.</span></span> <span data-ttu-id="ef3b5-141">Další informace o hello `%%sql` magic a dalších Magic, které jsou k dispozici s jádrem pyspark hello, najdete v části [jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="ef3b5-141">For more information about hello `%%sql` magic, and other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

        %%sql
        SHOW TABLES

    <span data-ttu-id="ef3b5-142">Zobrazí výstup jako vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="ef3b5-142">You see an output like shown below:</span></span>

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    <span data-ttu-id="ef3b5-143">Pouze hello tabulky, které mají hodnotu false v rámci hello **isTemporary** sloupce jsou tabulek hive, které jsou uložené v hello metaúložiště a je přístupný z nástrojů BI hello.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-143">Only hello tables that have false under hello **isTemporary** column are hive tables that are stored in hello metastore and can be accessed from hello BI tools.</span></span> <span data-ttu-id="ef3b5-144">V tomto kurzu se nám připojit toohello **TVK** jsme vytvořili tabulku.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-144">In this tutorial, we connect toohello **hvac** table we created.</span></span>

8. <span data-ttu-id="ef3b5-145">Ověřte, že tato tabulka hello obsahuje hello určená data.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-145">Verify that hello table contains hello intended data.</span></span> <span data-ttu-id="ef3b5-146">V prázdné buňky v poznámkovém bloku hello zkopírovat hello následující fragment kódu a stiskněte klávesu **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-146">In an empty cell in hello notebook, copy hello following snippet and press **SHIFT + ENTER**.</span></span>

        %%sql
        SELECT * FROM hvac LIMIT 10

9. <span data-ttu-id="ef3b5-147">Vypněte hello poznámkového bloku toorelease hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-147">Shut down hello notebook toorelease hello resources.</span></span> <span data-ttu-id="ef3b5-148">toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-148">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span>

## <span data-ttu-id="ef3b5-149"><a name="powerbi"></a>Pomocí Power BI pro vizualizaci dat Spark</span><span class="sxs-lookup"><span data-stu-id="ef3b5-149"><a name="powerbi"></a>Use Power BI for Spark data visualization</span></span>

> [!NOTE]
> <span data-ttu-id="ef3b5-150">Tato část je určena pouze pro 1.6 Spark na HDInsight 3.4 a 2.0 Spark na HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-150">This section is applicable only for Spark 1.6 on HDInsight 3.4 and Spark 2.0 on HDInsight 3.5.</span></span>
>
>

<span data-ttu-id="ef3b5-151">Po uložení dat hello jako tabulku, můžete pomocí Power BI tooconnect toohello dat a vizualizovat ho toocreate sestavy, řídicí panely, atd.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-151">Once you have saved hello data as a table, you can use Power BI tooconnect toohello data and visualize it toocreate reports, dashboards, etc.</span></span>

1. <span data-ttu-id="ef3b5-152">Ujistěte se, že máte přístup tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-152">Make sure you have access tooPower BI.</span></span> <span data-ttu-id="ef3b5-153">Můžete získat bezplatnou verzi preview odběr služby Power BI z [http://www.powerbi.com/](http://www.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="ef3b5-153">You can get a free preview subscription of Power BI from [http://www.powerbi.com/](http://www.powerbi.com/).</span></span>

2. <span data-ttu-id="ef3b5-154">Přihlaste se příliš[Power BI](http://www.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="ef3b5-154">Sign in too[Power BI](http://www.powerbi.com/).</span></span>

3. <span data-ttu-id="ef3b5-155">Hello dolní části levého podokna hello, klikněte na tlačítko **načíst Data**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-155">From hello bottom of hello left pane, click **Get Data**.</span></span>

4. <span data-ttu-id="ef3b5-156">Na hello **načíst Data** v části **Import nebo připojení tooData**, pro **databáze**, klikněte na tlačítko **získat**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-156">On hello **Get Data** page, under **Import or Connect tooData**, for **Databases**, click **Get**.</span></span>

    <span data-ttu-id="ef3b5-157">![Získání dat do Power BI pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "načíst data do Power BI pro Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-157">![Get data into Power BI for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Get data into Power BI for Apache Spark BI")</span></span>

5. <span data-ttu-id="ef3b5-158">Na další obrazovce hello, klikněte na tlačítko **Spark v Azure HDInsight** a pak klikněte na **Connect**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-158">On hello next screen, click **Spark on Azure HDInsight** and then click **Connect**.</span></span> <span data-ttu-id="ef3b5-159">Po zobrazení výzvy zadejte adresu URL clusteru hello (`mysparkcluster.azurehdinsight.net`) a hello pověření tooconnect toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-159">When prompted, enter hello cluster URL (`mysparkcluster.azurehdinsight.net`) and hello credentials tooconnect toohello cluster.</span></span>

    <span data-ttu-id="ef3b5-160">![Připojit tooApache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "připojit tooApache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-160">![Connect tooApache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "Connect tooApache Spark BI")</span></span>

    <span data-ttu-id="ef3b5-161">Po navázání připojení hello Power BI spustí import dat z hello clusteru Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-161">After hello connection is established, Power BI starts importing data from hello Spark cluster on HDInsight.</span></span>

6. <span data-ttu-id="ef3b5-162">Importuje hello data Power BI a přidá **Spark** datovou sadu v části hello **datové sady** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-162">Power BI imports hello data and adds a **Spark** dataset under hello **Datasets** heading.</span></span> <span data-ttu-id="ef3b5-163">Klikněte na tlačítko tooopen hello sady dat nová data hello toovisualize listu.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-163">Click hello data set tooopen a new worksheet toovisualize hello data.</span></span> <span data-ttu-id="ef3b5-164">Můžete také uložit hello listu jako sestavu.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-164">You can also save hello worksheet as a report.</span></span> <span data-ttu-id="ef3b5-165">toosave listu, z hello **soubor** nabídky, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-165">toosave a worksheet, from hello **File** menu, click **Save**.</span></span>

    <span data-ttu-id="ef3b5-166">![Apache Spark BI dlaždice na řídicím panelu Power BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI dlaždice na řídicím panelu Power BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-166">![Apache Spark BI tile on Power BI dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI tile on Power BI dashboard")</span></span>
7. <span data-ttu-id="ef3b5-167">Všimněte si, že hello **pole** seznamu na pravé hello uvádí hello **TVK** tabulky, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-167">Notice that hello **Fields** list on hello right lists hello **hvac** table you created earlier.</span></span> <span data-ttu-id="ef3b5-168">Rozbalte hello tabulky toosee hello pole v tabulce hello dříve definovaný v poznámkovém bloku.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-168">Expand hello table toosee hello fields in hello table, as you defined in notebook earlier.</span></span>

      <span data-ttu-id="ef3b5-169">![Seznam tabulek na řídicí panel Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "seznam tabulek na řídicí panel Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-169">![List tables on Apache Spark BI dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "List tables on Apache Spark BI dashboard")</span></span>

8. <span data-ttu-id="ef3b5-170">Sestavení vizualizace tooshow hello odchylka mezi teploty cíl a skutečný teploty pro každé sestavení.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-170">Build a visualization tooshow hello variance between target temperature and actual temperature for each building.</span></span> <span data-ttu-id="ef3b5-171">toovisualize chtěli dat, vyberte **plošný graf** (zobrazené v červeným rámečkem).</span><span class="sxs-lookup"><span data-stu-id="ef3b5-171">toovisualize yoru data, select **Area Chart** (shown in red box).</span></span> <span data-ttu-id="ef3b5-172">toodefine hello osy, přetažení myší hello **BuildingID** pole v části **osy**, a **ActualTemp**/**TargetTemp** pole v části **hodnotu**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-172">toodefine hello axis, drag-and-drop hello **BuildingID** field under **Axis**, and **ActualTemp**/**TargetTemp** fields under **Value**.</span></span>

    <span data-ttu-id="ef3b5-173">![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-173">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Create Spark data visualizations using Apache Spark BI")</span></span>

9. <span data-ttu-id="ef3b5-174">Ve výchozím nastavení zobrazuje hello vizualizace hello součet **ActualTemp** a **TargetTemp**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-174">By default hello visualization shows hello sum for **ActualTemp** and **TargetTemp**.</span></span> <span data-ttu-id="ef3b5-175">Obě hello pole z hello rozevíracího seznamu vyberte **průměrná** tooget v průměru skutečné a teploty cíl pro obě budovy.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-175">For both hello fields, from hello drop-down, select **Average** tooget an average of actual and target temperatures for both buildings.</span></span>

    <span data-ttu-id="ef3b5-176">![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-176">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Create Spark data visualizations using Apache Spark BI")</span></span>

10. <span data-ttu-id="ef3b5-177">Vaše vizualizace dat by měla být podobné toohello jeden hello snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-177">Your data visualization should be similar toohello one in hello screenshot.</span></span> <span data-ttu-id="ef3b5-178">Přesuňte kurzor hello vizualizace tooget popisy s příslušnými daty.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-178">Move your cursor over hello visualization tooget tool tips with relevant data.</span></span>

    <span data-ttu-id="ef3b5-179">![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-179">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Create Spark data visualizations using Apache Spark BI")</span></span>

11. <span data-ttu-id="ef3b5-180">Klikněte na tlačítko **Uložit** z hello top nabídky a zadejte název sestavy.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-180">Click **Save** from hello top menu and provide a report name.</span></span> <span data-ttu-id="ef3b5-181">Můžete taky připnout hello visual.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-181">You can also pin hello visual.</span></span> <span data-ttu-id="ef3b5-182">Pokud připnete vizualizace, je uložený na řídicím panelu, můžete sledovat hello nejnovější hodnotu na první pohled.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-182">When you pin a visualization, it is stored on your dashboard so you can track hello latest value at a glance.</span></span>

   <span data-ttu-id="ef3b5-183">Můžete přidat libovolný počet vizualizace tak, jak chcete pro hello stejné datové sady a připnete ji toohello řídicí panel pro snímek vaše data.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-183">You can add as many visualizations as you want for hello same dataset and pin them toohello dashboard for a snapshot of your data.</span></span> <span data-ttu-id="ef3b5-184">Clustery Spark v HDInsight jsou také připojené tooPower BI s protokolem direct connect.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-184">Also, Spark clusters on HDInsight are connected tooPower BI with direct connect.</span></span> <span data-ttu-id="ef3b5-185">Tím se zajistí, že má Power BI vždy hello většina aktuální data z clusteru, není nutné tooschedule aktualizace pro datovou sadu hello.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-185">This ensures that Power BI always has hello most up-to-date data from your cluster so you do not need tooschedule refreshes for hello dataset.</span></span>

## <span data-ttu-id="ef3b5-186"><a name="tableau"></a>Použijte plochu Tableau pro vizualizaci dat Spark</span><span class="sxs-lookup"><span data-stu-id="ef3b5-186"><a name="tableau"></a>Use Tableau Desktop for Spark data visualization</span></span>

> [!NOTE]
> <span data-ttu-id="ef3b5-187">Tato část je určena pouze pro clustery Spark 1.5.2 vytvořené v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-187">This section is applicable only for Spark 1.5.2 clusters created in Azure HDInsight.</span></span>
>
>

1. <span data-ttu-id="ef3b5-188">Nainstalujte [Tableau plochy](http://www.tableau.com/products/desktop) na kterém je spuštěn v tomto kurzu Apache Spark BI počítači hello.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-188">Install [Tableau Desktop](http://www.tableau.com/products/desktop) on hello computer where you are running this Apache Spark BI tutorial.</span></span>

2. <span data-ttu-id="ef3b5-189">Ujistěte se, že tento počítač má také nainstalovaný ovladač Microsoft Spark ODBC.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-189">Make sure that computer also has Microsoft Spark ODBC driver installed.</span></span> <span data-ttu-id="ef3b5-190">Můžete nainstalovat ovladač hello z [zde](http://go.microsoft.com/fwlink/?LinkId=616229).</span><span class="sxs-lookup"><span data-stu-id="ef3b5-190">You can install hello driver from [here](http://go.microsoft.com/fwlink/?LinkId=616229).</span></span>

1. <span data-ttu-id="ef3b5-191">Spuštění Tableau plochy.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-191">Launch Tableau Desktop.</span></span> <span data-ttu-id="ef3b5-192">V levém podokně hello hello seznamu tooconnect serveru, klikněte na **Spark SQL**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-192">In hello left pane, from hello list of server tooconnect to, click **Spark SQL**.</span></span> <span data-ttu-id="ef3b5-193">Pokud není ve výchozím nastavení v levém podokně hello zobrazí Spark SQL, můžete nějakého najít kliknutím **více serverů**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-193">If Spark SQL is not listed by default in hello left pane, you can find it by click **More Servers**.</span></span>
2. <span data-ttu-id="ef3b5-194">V hello Spark SQL připojení dialogové okno, zadejte hodnoty hello, jak je znázorněno v hello snímek a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-194">In hello Spark SQL connection dialog box, provide hello values as shown in hello screenshot, and then click **OK**.</span></span>

    <span data-ttu-id="ef3b5-195">![Připojit tooa cluster Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect tooa cluster Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-195">![Connect tooa cluster for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect tooa cluster for Apache Spark BI")</span></span>

    <span data-ttu-id="ef3b5-196">Hello ověřování rozevírací seznamy **služby Microsoft Azure HDInsight** jako možnost, pouze v případě, že jste nainstalovali hello [ovladač ODBC Microsoft Sparku](http://go.microsoft.com/fwlink/?LinkId=616229) v počítači hello.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-196">hello authentication drop-down lists **Microsoft Azure HDInsight Service** as an option, only if you installed hello [Microsoft Spark ODBC Driver](http://go.microsoft.com/fwlink/?LinkId=616229) on hello computer.</span></span>
3. <span data-ttu-id="ef3b5-197">Na další obrazovce hello z hello **schématu** rozevíracího seznamu, klikněte na tlačítko hello **najít** ikonu a pak klikněte na tlačítko **výchozí**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-197">On hello next screen, from hello **Schema** drop-down, click hello **Find** icon, and then click **default**.</span></span>

    <span data-ttu-id="ef3b5-198">![Nalezeno schéma pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "najít schéma pro Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-198">![Find schema for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Find schema for Apache Spark BI")</span></span>
4. <span data-ttu-id="ef3b5-199">Pro hello **tabulky** pole, klikněte na tlačítko hello **najít** ikonu znovu toolist všechny hello Hive v clusteru hello dostupných tabulek.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-199">For hello **Table** field, click hello **Find** icon again toolist all hello Hive tables available in hello cluster.</span></span> <span data-ttu-id="ef3b5-200">Měli byste vidět hello **TVK** tabulky, které jste vytvořili dříve pomocí poznámkového bloku hello.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-200">You should see hello **hvac** table you created earlier using hello notebook.</span></span>

    <span data-ttu-id="ef3b5-201">![Najít tabulku pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "najít tabulku pro Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-201">![Find table for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Find table for Apache Spark BI")</span></span>
5. <span data-ttu-id="ef3b5-202">Přetáhnout myší nejvyšší poli toohello hello tabulky na pravé hello.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-202">Drag and drop hello table toohello top box on hello right.</span></span> <span data-ttu-id="ef3b5-203">Tableau importuje hello data a zobrazí hello schématu jako zvýrazněná podle hello red pole.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-203">Tableau imports hello data and displays hello schema as highlighted by hello red box.</span></span>

    <span data-ttu-id="ef3b5-204">![Přidání tabulky tooTableau pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "přidat tooTableau tabulky pro Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-204">![Add tables tooTableau for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Add tables tooTableau for Apache Spark BI")</span></span>
6. <span data-ttu-id="ef3b5-205">Klikněte na tlačítko hello **Sheet1** karta v dolní části hello vlevo.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-205">Click hello **Sheet1** tab at hello bottom left.</span></span> <span data-ttu-id="ef3b5-206">Ujistěte se, vizualizace, který ukazuje hello průměrná cíl a skutečný teploty pro všechny budovy pro jednotlivá data.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-206">Make a visualization that shows hello average target and actual temperatures for all buildings for each date.</span></span> <span data-ttu-id="ef3b5-207">Přetáhněte **datum** a **ID budovy** příliš**sloupce** a **skutečné Temp**/**cíl Temp**příliš**řádky**.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-207">Drag **Date** and **Building ID** too**Columns** and **Actual Temp**/**Target Temp** too**Rows**.</span></span> <span data-ttu-id="ef3b5-208">V části **značky**, vyberte **oblasti** toouse oblasti mapy pro vizualizaci dat Spark.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-208">Under **Marks**, select **Area** toouse an area map for Spark data visualization.</span></span>

     <span data-ttu-id="ef3b5-209">![Přidat pole pro vizualizaci dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "přidat pole pro vizualizaci dat Spark")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-209">![Add fields for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Add fields for Spark data visualization")</span></span>
7. <span data-ttu-id="ef3b5-210">Ve výchozím nastavení, jsou uvedeny hello teploty pole jako agregace.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-210">By default, hello temperature fields are shown as aggregate.</span></span> <span data-ttu-id="ef3b5-211">Pokud chcete místo toho tooshow hello průměrné teploty, můžete provést tak z hello rozevíracího seznamu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-211">If you want tooshow hello average temperatures instead, you can do so from hello drop-down, as shown below.</span></span>

    <span data-ttu-id="ef3b5-212">![Trvat průměrná z teploty pro vizualizaci dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "trvat průměrná z teploty pro vizualizaci dat Spark")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-212">![Take average of temperature for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "Take average of temperature for Spark data visualization")</span></span>

8. <span data-ttu-id="ef3b5-213">Můžete také super použít jedna mapa teploty přes hello jiných tooget lepší chování rozdíl mezi cíl a skutečný teploty.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-213">You can also super-impose one temperature map over hello other tooget a better feel of difference between target and actual temperatures.</span></span> <span data-ttu-id="ef3b5-214">Přesuňte hello myši toohello rohu hello nižší oblasti mapy, dokud najdete v části obrazce popisovač hello zvýrazněných v červeném kroužku.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-214">Move hello mouse toohello corner of hello lower area map till you see hello handle shape highlighted in a red circle.</span></span> <span data-ttu-id="ef3b5-215">Přetáhněte hello mapy toohello další mapy na hello top a verze hello myši až uvidíte hello tvar zvýrazněných v červeným rámečkem.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-215">Drag hello map toohello other map on hello top and release hello mouse when you see hello shape highlighted in red rectangle.</span></span>

    <span data-ttu-id="ef3b5-216">![Sloučení mapy pro vizualizaci dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "sloučení mapy pro vizualizaci dat Spark")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-216">![Merge maps for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Merge maps for Spark data visualization")</span></span>

     <span data-ttu-id="ef3b5-217">Vaše vizualizace dat měli změnit, jak je znázorněno v hello – snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="ef3b5-217">Your data visualization should change as shown in hello screenshot:</span></span>

    <span data-ttu-id="ef3b5-218">![Výstup tableau vizualizace dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau výstup vizualizace dat Spark")</span><span class="sxs-lookup"><span data-stu-id="ef3b5-218">![Tableau output for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau output for Spark data visualization")</span></span>
9. <span data-ttu-id="ef3b5-219">Klikněte na tlačítko **Uložit** toosave hello listu.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-219">Click **Save** toosave hello worksheet.</span></span> <span data-ttu-id="ef3b5-220">Můžete vytvořit řídicí panely a přidejte jeden nebo více stylů tooit.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-220">You can create dashboards and add one or more sheets tooit.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef3b5-221">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ef3b5-221">Next steps</span></span>

<span data-ttu-id="ef3b5-222">Pokud jste zjistili, jak vytvořit dat Spark datového rámce tooquery toocreate cluster s podporou a poté přístup k datům z nástrojů BI.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-222">So far you learned how toocreate a cluster, create Spark data frames tooquery data, and then access that data from BI tools.</span></span> <span data-ttu-id="ef3b5-223">Teď můžete prohlížet pokyny, jak toomanage hello prostředky clusteru a ladění úloh, které jsou spuštěné v clusteru služby HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="ef3b5-223">You can now look at instructions on how toomanage hello cluster resources and debug jobs that are running in an HDInsight Spark cluster.</span></span>

* [<span data-ttu-id="ef3b5-224">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ef3b5-224">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="ef3b5-225">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ef3b5-225">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

