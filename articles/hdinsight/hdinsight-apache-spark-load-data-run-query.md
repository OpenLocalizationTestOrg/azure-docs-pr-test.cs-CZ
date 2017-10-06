---
title: "aaaRun interaktivních dotazů v clusteru Azure HDInsight Spark | Microsoft Docs"
description: "Rychlý start HDInsight Spark na tom, jak clusteru toocreate Apache Spark v HDInsight."
keywords: spark quickstart, interactive spark, interactive query, hdinsight spark, azure spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 3864eba50eb3828a9ecb657ded88080e1974585f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="4f0f3-104">Spusťte interaktivní dotazy na clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="4f0f3-104">Run interactive queries on an HDInsight Spark cluster</span></span>

<span data-ttu-id="4f0f3-105">V tomto článku použijte Jupyter poznámkového bloku toorun interaktivních dotazů Spark SQL na clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-105">In this article, you use a Jupyter notebook toorun interactive Spark SQL queries against a Spark cluster.</span></span> <span data-ttu-id="4f0f3-106">Poznámkový blok Jupyter je aplikace založené na prohlížeči, která rozšiřuje hello pomocí konzoly interaktivní možnosti toohello Web.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-106">Jupyter notebook is a browser-based application that extends hello console-based interactive experience toohello Web.</span></span> <span data-ttu-id="4f0f3-107">Další informace najdete v tématu [hello Poznámkový blok Jupyter](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).</span><span class="sxs-lookup"><span data-stu-id="4f0f3-107">For more information, see [hello Jupyter notebook](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).</span></span>

<span data-ttu-id="4f0f3-108">V tomto kurzu použijete hello **PySpark** jádra v toorun poznámkového bloku Jupyter hello interaktivních dotazů Spark SQL.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-108">For this tutorial, you use hello **PySpark** kernel in hello Jupyter notebook toorun an interactive Spark SQL query.</span></span> <span data-ttu-id="4f0f3-109">Poznámkové bloky Jupyter v clusterech HDInsight také podporují dva další jádra - **PySpark3** a **Spark**.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-109">Jupyter notebooks on HDInsight clusters also support two other kernels - **PySpark3** and **Spark**.</span></span> <span data-ttu-id="4f0f3-110">Další informace o hello jádra a hello výhody použití **PySpark**, najdete v části [clusterů jádra poznámkového bloku Jupyter použít s Apache Spark v HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="4f0f3-110">For more information about hello kernels, and hello benefits of using **PySpark**, see [Use Jupyter notebook kernels with Apache Spark clusters in HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f0f3-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4f0f3-111">Prerequisites</span></span>

* <span data-ttu-id="4f0f3-112">**Clusteru Azure HDInsight Spark**.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-112">**An Azure HDInsight Spark cluster**.</span></span> <span data-ttu-id="4f0f3-113">Pokyny najdete v tématu [vytvářet cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="4f0f3-113">For instructions, see [Create an Apache Spark cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="create-a-jupyter-notebook-toorun-interactive-queries"></a><span data-ttu-id="4f0f3-114">Vytvoření poznámkového bloku Jupyter toorun interaktivních dotazů</span><span class="sxs-lookup"><span data-stu-id="4f0f3-114">Create a Jupyter notebook toorun interactive queries</span></span>

<span data-ttu-id="4f0f3-115">dotazy toorun používáme ukázková data, která je ve výchozím nastavení k dispozici v hello úložiště přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-115">toorun queries, we use sample data that is by default available in hello storage associated with hello cluster.</span></span> <span data-ttu-id="4f0f3-116">Ale je nutné nejdřív načíst data do Spark jako dataframe.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-116">However, you must first load that data into Spark as a dataframe.</span></span> <span data-ttu-id="4f0f3-117">Až budete mít hello dataframe, můžete spouštět dotazy na pomocí poznámkového bloku Jupyter hello.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-117">Once you have hello dataframe, you can run queries on it using hello Jupyter notebook.</span></span> <span data-ttu-id="4f0f3-118">V této části vám tak informace o tom, jak:</span><span class="sxs-lookup"><span data-stu-id="4f0f3-118">In this section, you look at how to:</span></span>

* <span data-ttu-id="4f0f3-119">Zaregistrujte se jako Spark dataframe Ukázka datové sady.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-119">Register a sample data set as a Spark dataframe.</span></span>
* <span data-ttu-id="4f0f3-120">Spuštění dotazů na hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-120">Run queries on hello dataframe.</span></span>

1. <span data-ttu-id="4f0f3-121">Otevřete hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4f0f3-121">Open hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="4f0f3-122">Když jste se rozhodli řídicí panel toohello toopin hello clusteru, klikněte na dlaždici hello clusteru z hello řídicí panel toolaunch hello clusteru okno.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-122">If you opted toopin hello cluster toohello dashboard, click hello cluster tile from hello dashboard toolaunch hello cluster blade.</span></span>

    <span data-ttu-id="4f0f3-123">Pokud připnete není hello toohello řídicí panel clusteru, v levém podokně hello, klikněte na tlačítko **clustery HDInsight**a potom klikněte na cluster hello jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-123">If you did not pin hello cluster toohello dashboard, from hello left pane, click **HDInsight clusters**, and then click hello cluster you created.</span></span>

3. <span data-ttu-id="4f0f3-124">V části **Rychlé odkazy** klikněte na **Řídicí panely clusteru** a potom klikněte na **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-124">From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="4f0f3-125">Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-125">If prompted, enter hello admin credentials for hello cluster.</span></span>

   <span data-ttu-id="4f0f3-126">![Otevřete Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "poznámkového bloku Jupyter otevřete toorun interaktivní Spark SQL dotazu")</span><span class="sxs-lookup"><span data-stu-id="4f0f3-126">![Open Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook toorun interactive Spark SQL query")</span></span>

   > [!NOTE]
   > <span data-ttu-id="4f0f3-127">Může také přístup k hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-127">You may also access hello Jupyter notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="4f0f3-128">Nahraďte **CLUSTERNAME** s hello název clusteru:</span><span class="sxs-lookup"><span data-stu-id="4f0f3-128">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="4f0f3-129">Vytvořte poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-129">Create a notebook.</span></span> <span data-ttu-id="4f0f3-130">Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-130">Click **New**, and then click **PySpark**.</span></span>

   <span data-ttu-id="4f0f3-131">![Vytvořit Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "vytvořit Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz")</span><span class="sxs-lookup"><span data-stu-id="4f0f3-131">![Create a Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook toorun interactive Spark SQL query")</span></span>

   <span data-ttu-id="4f0f3-132">Nový poznámkový blok se vytvoří a otevřít s názvem hello Untitled(Untitled.pynb).</span><span class="sxs-lookup"><span data-stu-id="4f0f3-132">A new notebook is created and opened with hello name Untitled(Untitled.pynb).</span></span>

4. <span data-ttu-id="4f0f3-133">Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název, pokud chcete.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-133">Click hello notebook name at hello top, and enter a friendly name if you want.</span></span>

    <span data-ttu-id="4f0f3-134">![Zadejte název pro hello Jupter poznámkového bloku toorun interaktivní Spark dotaz z](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "zadejte název pro hello Jupter poznámkového bloku toorun interaktivní Spark dotaz z")</span><span class="sxs-lookup"><span data-stu-id="4f0f3-134">![Provide a name for hello Jupter notebook toorun interactive Spark query from](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "Provide a name for hello Jupter notebook toorun interactive Spark query from")</span></span>

5. <span data-ttu-id="4f0f3-135">Následující hello vložení kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER** toorun hello kódu.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-135">Paste hello following code in an empty cell, and then press **SHIFT + ENTER** toorun hello code.</span></span> <span data-ttu-id="4f0f3-136">Hello kód importuje hello typů nezbytných pro tento scénář:</span><span class="sxs-lookup"><span data-stu-id="4f0f3-136">hello code imports hello types required for this scenario:</span></span>

        from pyspark.sql.types import *

    <span data-ttu-id="4f0f3-137">Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-137">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="4f0f3-138">kontexty Spark a Hive Hello jsou pro vás automaticky vytvoří při spuštění první buňky kódu hello.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-138">hello Spark and Hive contexts are automatically created for you when you run hello first code cell.</span></span>

    <span data-ttu-id="4f0f3-139">![Stav interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Stav interaktivního dotazu Spark SQL")</span><span class="sxs-lookup"><span data-stu-id="4f0f3-139">![Status of interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status of interactive Spark SQL query")</span></span>

    <span data-ttu-id="4f0f3-140">Při každém spuštění interaktivních dotazů v Jupyter se název okna webového prohlížeče ukazuje **(zaneprázdněn)** společně s názvem poznámkového bloku hello.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-140">Every time you run an interactive query in Jupyter, your web browser window title shows a **(Busy)** status along with hello notebook title.</span></span> <span data-ttu-id="4f0f3-141">Zobrazí také další toohello plný kroužek **PySpark** text v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-141">You also see a solid circle next toohello **PySpark** text in hello top-right corner.</span></span> <span data-ttu-id="4f0f3-142">Po dokončení úlohy hello změní tooa prázdný kruh.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-142">After hello job is completed, it changes tooa hollow circle.</span></span>

6. <span data-ttu-id="4f0f3-143">Před načtením dat hello do clusteru Spark, dejte nám vypadat snímek ho.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-143">Before you load hello data into a Spark cluster, let us look a snapshot of it.</span></span> <span data-ttu-id="4f0f3-144">Hello ukázková data použili v tomto kurzu jsou k dispozici jako soubor CSV na všechny clustery HDInsight Spark v **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-144">hello sample data used in this tutorial is available as a CSV file on all HDInsight Spark clusters at **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**.</span></span> <span data-ttu-id="4f0f3-145">Hello data zaznamená změny teploty hello budovy.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-145">hello data captures hello temperature variations of a building.</span></span> <span data-ttu-id="4f0f3-146">Tady jsou hello několik prvních řádků dat hello.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-146">Here are hello first few rows of hello data.</span></span>

    <span data-ttu-id="4f0f3-147">![Snímek dat pro interaktivních dotazů Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "snímek dat pro interaktivních dotazů Spark SQL")</span><span class="sxs-lookup"><span data-stu-id="4f0f3-147">![Snapshot of data for interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Snapshot of data for interactive Spark SQL query")</span></span>

6. <span data-ttu-id="4f0f3-148">Vytvoření dataframe a do dočasné tabulky (**TVK**) tak, že spustíte hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-148">Create a dataframe and a temporary table (**hvac**) by running hello following code.</span></span> <span data-ttu-id="4f0f3-149">V tomto kurzu jsme nevytvářejte všechny sloupce hello v dočasné tabulce hello jako porovnání toohello sloupce hello nezpracovaná data ve formátu CSV.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-149">For this tutorial, we do not create all hello columns in hello temporary table as compared toohello columns in hello raw CSV data.</span></span> 

        # Create an RDD from sample data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

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

7. <span data-ttu-id="4f0f3-150">Po vytvoření tabulky hello Spusťte interaktivní dotaz na hello data, použijte následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-150">Once hello table is created, run interactive query on hello data, use hello following code.</span></span>

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   <span data-ttu-id="4f0f3-151">Vzhledem k tomu, že používáte jádro PySpark, můžete nyní přímo spustit interaktivní dotazu SQL na dočasnou tabulku hello **TVK** jste vytvořili pomocí hello `%%sql` magic.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-151">Because you are using a PySpark kernel, you can now directly run an interactive SQL query on hello temporary table **hvac** that you created by using hello `%%sql` magic.</span></span> <span data-ttu-id="4f0f3-152">Další informace o hello `%%sql` magic a dalších Magic, které jsou k dispozici s jádrem pyspark hello, najdete v části [jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="4f0f3-152">For more information about hello `%%sql` magic, and other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

   <span data-ttu-id="4f0f3-153">ve výchozím nastavení se zobrazí následující tabulkový výstup Hello.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-153">hello following tabular output is displayed by default.</span></span>

     <span data-ttu-id="4f0f3-154">![Tabulkový výstup výsledku interaktivního dotazu Spark](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabulkový výstup výsledku interaktivního dotazu Spark")</span><span class="sxs-lookup"><span data-stu-id="4f0f3-154">![Table output of interactive Spark query result](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Table output of interactive Spark query result")</span></span>

    <span data-ttu-id="4f0f3-155">Můžete také zjistit hello výsledky v dalších vizualizacích.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-155">You can also see hello results in other visualizations as well.</span></span> <span data-ttu-id="4f0f3-156">Například plošný graf pro stejný výstup bude vypadat hello následující hello.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-156">For example, an area graph for hello same output would look like hello following.</span></span>

    <span data-ttu-id="4f0f3-157">![Plošný graf výsledku interaktivního dotazu Spark](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Plošný graf výsledku interaktivního dotazu Spark")</span><span class="sxs-lookup"><span data-stu-id="4f0f3-157">![Area graph of interactive Spark query result](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Area graph of interactive Spark query result")</span></span>

9. <span data-ttu-id="4f0f3-158">Po dokončení spuštění aplikace hello vypněte prostředky clusteru hello poznámkového bloku toorelease hello.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-158">Shut down hello notebook toorelease hello cluster resources after you have finished running hello application.</span></span> <span data-ttu-id="4f0f3-159">toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-159">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span>

## <a name="next-step"></a><span data-ttu-id="4f0f3-160">Další krok</span><span class="sxs-lookup"><span data-stu-id="4f0f3-160">Next step</span></span>

<span data-ttu-id="4f0f3-161">V tomto článku jste se naučili jak toorun interaktivních dotazů v Spark pomocí poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-161">In this article you learned how toorun interactive queries in Spark using Jupyter notebook.</span></span> <span data-ttu-id="4f0f3-162">Posunutí toohello další článek toosee jak mohou být vyžádány hello data, která jste zaregistrovali v Spark do nástroj pro analýzu BI, například Power BI a Tableau.</span><span class="sxs-lookup"><span data-stu-id="4f0f3-162">Advance toohello next article toosee how hello data you registered in Spark can be pulled into a BI analytics tool such as Power BI and Tableau.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="4f0f3-163">Spark BI nástroje vizualizaci dat pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="4f0f3-163">Spark BI using data visualization tools with Azure HDInsight</span></span>](hdinsight-apache-spark-use-bi-tools.md)




