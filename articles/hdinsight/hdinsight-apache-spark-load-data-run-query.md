---
title: "Spusťte interaktivní dotazy v clusteru Azure HDInsight Spark | Microsoft Docs"
description: "Rychlý start k HDInsight Spark týkající se vytvoření clusteru Apache Spark ve službě HDInsight."
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
ms.openlocfilehash: ada1c3d1482c68834dbbf5eabbd045a7e0c01f9f
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="49903-104">Spusťte interaktivní dotazy na clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="49903-104">Run interactive queries on an HDInsight Spark cluster</span></span>

<span data-ttu-id="49903-105">V tomto článku pomocí poznámkového bloku Jupyter ke spouštění interaktivních dotazů Spark SQL na clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="49903-105">In this article, you use a Jupyter notebook to run interactive Spark SQL queries against a Spark cluster.</span></span> <span data-ttu-id="49903-106">Poznámkový blok Jupyter je aplikace založené na prohlížeči, která rozšiřuje možnosti interaktivního pomocí konzoly na webu.</span><span class="sxs-lookup"><span data-stu-id="49903-106">Jupyter notebook is a browser-based application that extends the console-based interactive experience to the Web.</span></span> <span data-ttu-id="49903-107">Další informace najdete v tématu [Poznámkový blok Jupyter](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).</span><span class="sxs-lookup"><span data-stu-id="49903-107">For more information, see [The Jupyter notebook](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).</span></span>

<span data-ttu-id="49903-108">V tomto kurzu použijete **PySpark** jádra v poznámkového bloku Jupyter ke spuštění interaktivních dotazů Spark SQL.</span><span class="sxs-lookup"><span data-stu-id="49903-108">For this tutorial, you use the **PySpark** kernel in the Jupyter notebook to run an interactive Spark SQL query.</span></span> <span data-ttu-id="49903-109">Poznámkové bloky Jupyter v clusterech HDInsight také podporují dva další jádra - **PySpark3** a **Spark**.</span><span class="sxs-lookup"><span data-stu-id="49903-109">Jupyter notebooks on HDInsight clusters also support two other kernels - **PySpark3** and **Spark**.</span></span> <span data-ttu-id="49903-110">Další informace o jádrech a výhody použití **PySpark**, najdete v části [clusterů jádra poznámkového bloku Jupyter použít s Apache Spark v HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="49903-110">For more information about the kernels, and the benefits of using **PySpark**, see [Use Jupyter notebook kernels with Apache Spark clusters in HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49903-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="49903-111">Prerequisites</span></span>

* <span data-ttu-id="49903-112">**Clusteru Azure HDInsight Spark**.</span><span class="sxs-lookup"><span data-stu-id="49903-112">**An Azure HDInsight Spark cluster**.</span></span> <span data-ttu-id="49903-113">Pokyny najdete v tématu [vytvářet cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="49903-113">For instructions, see [Create an Apache Spark cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="create-a-jupyter-notebook-to-run-interactive-queries"></a><span data-ttu-id="49903-114">Vytvoření poznámkového bloku Jupyter ke spuštění interaktivních dotazů</span><span class="sxs-lookup"><span data-stu-id="49903-114">Create a Jupyter notebook to run interactive queries</span></span>

<span data-ttu-id="49903-115">Spuštění dotazů, použijeme ukázková data, která je ve výchozím nastavení k dispozici v úložišti, které jsou přidruženy ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="49903-115">To run queries, we use sample data that is by default available in the storage associated with the cluster.</span></span> <span data-ttu-id="49903-116">Ale je nutné nejdřív načíst data do Spark jako dataframe.</span><span class="sxs-lookup"><span data-stu-id="49903-116">However, you must first load that data into Spark as a dataframe.</span></span> <span data-ttu-id="49903-117">Jakmile máte dataframe, můžete spouštět dotazy na pomocí poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="49903-117">Once you have the dataframe, you can run queries on it using the Jupyter notebook.</span></span> <span data-ttu-id="49903-118">V této části vám tak informace o tom, jak:</span><span class="sxs-lookup"><span data-stu-id="49903-118">In this section, you look at how to:</span></span>

* <span data-ttu-id="49903-119">Zaregistrujte se jako Spark dataframe Ukázka datové sady.</span><span class="sxs-lookup"><span data-stu-id="49903-119">Register a sample data set as a Spark dataframe.</span></span>
* <span data-ttu-id="49903-120">Na dataframe spouštět dotazy.</span><span class="sxs-lookup"><span data-stu-id="49903-120">Run queries on the dataframe.</span></span>

1. <span data-ttu-id="49903-121">Otevřete web [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="49903-121">Open the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="49903-122">Pokud jste se rozhodli připnout cluster na řídicí panel, kliknutím na dlaždici clusteru na řídicím panelu spusťte okno clusteru.</span><span class="sxs-lookup"><span data-stu-id="49903-122">If you opted to pin the cluster to the dashboard, click the cluster tile from the dashboard to launch the cluster blade.</span></span>

    <span data-ttu-id="49903-123">Pokud jste cluster na řídicí panel nepřipnuli, v levém podokně klikněte na **Clustery HDInsight** a pak klikněte na cluster, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="49903-123">If you did not pin the cluster to the dashboard, from the left pane, click **HDInsight clusters**, and then click the cluster you created.</span></span>

3. <span data-ttu-id="49903-124">V části **Rychlé odkazy** klikněte na **Řídicí panely clusteru** a potom klikněte na **Poznámkový blok Jupyter**.</span><span class="sxs-lookup"><span data-stu-id="49903-124">From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="49903-125">Po vyzvání zadejte přihlašovací údaje správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="49903-125">If prompted, enter the admin credentials for the cluster.</span></span>

   <span data-ttu-id="49903-126">![Otevření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "Otevření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL")</span><span class="sxs-lookup"><span data-stu-id="49903-126">![Open Jupyter notebook to run interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook to run interactive Spark SQL query")</span></span>

   > [!NOTE]
   > <span data-ttu-id="49903-127">K poznámkovému bloku Jupyter pro váš cluster se dostanete také otevřením následující adresy URL v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="49903-127">You may also access the Jupyter notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="49903-128">Nahraďte **CLUSTERNAME** názvem clusteru:</span><span class="sxs-lookup"><span data-stu-id="49903-128">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="49903-129">Vytvořte poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="49903-129">Create a notebook.</span></span> <span data-ttu-id="49903-130">Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="49903-130">Click **New**, and then click **PySpark**.</span></span>

   <span data-ttu-id="49903-131">![Vytvoření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Vytvoření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL")</span><span class="sxs-lookup"><span data-stu-id="49903-131">![Create a Jupyter notebook to run interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook to run interactive Spark SQL query")</span></span>

   <span data-ttu-id="49903-132">Nový poznámkový blok se vytvoří a otevře s názvem Bez názvu (Bez názvu.pynb).</span><span class="sxs-lookup"><span data-stu-id="49903-132">A new notebook is created and opened with the name Untitled(Untitled.pynb).</span></span>

4. <span data-ttu-id="49903-133">Pokud chcete, klikněte na název poznámkového bloku v horní části a zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="49903-133">Click the notebook name at the top, and enter a friendly name if you want.</span></span>

    <span data-ttu-id="49903-134">![Zadání názvu, ze kterého má poznámkový blok Jupyter spustit interaktivní dotaz Spark](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "Zadání názvu, ze kterého má poznámkový blok Jupyter spustit interaktivní dotaz Spark")</span><span class="sxs-lookup"><span data-stu-id="49903-134">![Provide a name for the Jupter notebook to run interactive Spark query from](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "Provide a name for the Jupter notebook to run interactive Spark query from")</span></span>

5. <span data-ttu-id="49903-135">Do prázdné buňky vložte následující kód a stisknutím **SHIFT + ENTER** kód spusťte.</span><span class="sxs-lookup"><span data-stu-id="49903-135">Paste the following code in an empty cell, and then press **SHIFT + ENTER** to run the code.</span></span> <span data-ttu-id="49903-136">Kód naimportuje typy potřebné pro tento scénář:</span><span class="sxs-lookup"><span data-stu-id="49903-136">The code imports the types required for this scenario:</span></span>

        from pyspark.sql.types import *

    <span data-ttu-id="49903-137">Vzhledem k tomu, že jste poznámkový blok vytvořili pomocí jádra PySpark, není nutné explicitně tvořit kontexty.</span><span class="sxs-lookup"><span data-stu-id="49903-137">Because you created a notebook using the PySpark kernel, you do not need to create any contexts explicitly.</span></span> <span data-ttu-id="49903-138">Kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu.</span><span class="sxs-lookup"><span data-stu-id="49903-138">The Spark and Hive contexts are automatically created for you when you run the first code cell.</span></span>

    <span data-ttu-id="49903-139">![Stav interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Stav interaktivního dotazu Spark SQL")</span><span class="sxs-lookup"><span data-stu-id="49903-139">![Status of interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status of interactive Spark SQL query")</span></span>

    <span data-ttu-id="49903-140">Při každém spuštění interaktivního dotazu v Jupyter se název okna webového prohlížeče zobrazí jako **(Zaneprázdněn)** společně s názvem poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="49903-140">Every time you run an interactive query in Jupyter, your web browser window title shows a **(Busy)** status along with the notebook title.</span></span> <span data-ttu-id="49903-141">Zobrazí se také plný kroužek vedle textu **PySpark** v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="49903-141">You also see a solid circle next to the **PySpark** text in the top-right corner.</span></span> <span data-ttu-id="49903-142">Po dokončení úlohy se změní na prázdný kruh.</span><span class="sxs-lookup"><span data-stu-id="49903-142">After the job is completed, it changes to a hollow circle.</span></span>

6. <span data-ttu-id="49903-143">Před načtením dat do clusteru Spark, dejte nám vypadat snímek ho.</span><span class="sxs-lookup"><span data-stu-id="49903-143">Before you load the data into a Spark cluster, let us look a snapshot of it.</span></span> <span data-ttu-id="49903-144">Ukázková data použili v tomto kurzu jsou k dispozici jako soubor CSV na všechny clustery HDInsight Spark v **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**.</span><span class="sxs-lookup"><span data-stu-id="49903-144">The sample data used in this tutorial is available as a CSV file on all HDInsight Spark clusters at **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**.</span></span> <span data-ttu-id="49903-145">Data zaznamená změny teploty budovy.</span><span class="sxs-lookup"><span data-stu-id="49903-145">The data captures the temperature variations of a building.</span></span> <span data-ttu-id="49903-146">Zde naleznete několik prvních řádků data.</span><span class="sxs-lookup"><span data-stu-id="49903-146">Here are the first few rows of the data.</span></span>

    <span data-ttu-id="49903-147">![Snímek dat pro interaktivních dotazů Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "snímek dat pro interaktivních dotazů Spark SQL")</span><span class="sxs-lookup"><span data-stu-id="49903-147">![Snapshot of data for interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Snapshot of data for interactive Spark SQL query")</span></span>

6. <span data-ttu-id="49903-148">Vytvoření dataframe a do dočasné tabulky (**TVK**) tak, že spustíte následující kód.</span><span class="sxs-lookup"><span data-stu-id="49903-148">Create a dataframe and a temporary table (**hvac**) by running the following code.</span></span> <span data-ttu-id="49903-149">V tomto kurzu jsme nevytvářejte všechny sloupce v dočasné tabulce porovnání sloupce v nezpracovaných dat sdíleného svazku clusteru.</span><span class="sxs-lookup"><span data-stu-id="49903-149">For this tutorial, we do not create all the columns in the temporary table as compared to the columns in the raw CSV data.</span></span> 

        # Create an RDD from sample data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

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

7. <span data-ttu-id="49903-150">Po vytvoření tabulky, spusťte interaktivní dotaz na data, použijte následující kód.</span><span class="sxs-lookup"><span data-stu-id="49903-150">Once the table is created, run interactive query on the data, use the following code.</span></span>

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   <span data-ttu-id="49903-151">Vzhledem k tomu, že používáte jádro PySpark, můžete nyní přímo spustit interaktivní dotaz SQL nad dočasnou tabulkou **hvac**, kterou jste vytvořili pomocí magických příkazů `%%sql`.</span><span class="sxs-lookup"><span data-stu-id="49903-151">Because you are using a PySpark kernel, you can now directly run an interactive SQL query on the temporary table **hvac** that you created by using the `%%sql` magic.</span></span> <span data-ttu-id="49903-152">Další informace o magických příkazech `%%sql` a dalších magických příkazech, které jsou k dispozici s jádrem PySpark, najdete v části [Jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="49903-152">For more information about the `%%sql` magic, and other magics available with the PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

   <span data-ttu-id="49903-153">Ve výchozím nastavení se zobrazí následující tabulkový výstup.</span><span class="sxs-lookup"><span data-stu-id="49903-153">The following tabular output is displayed by default.</span></span>

     <span data-ttu-id="49903-154">![Tabulkový výstup výsledku interaktivního dotazu Spark](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabulkový výstup výsledku interaktivního dotazu Spark")</span><span class="sxs-lookup"><span data-stu-id="49903-154">![Table output of interactive Spark query result](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Table output of interactive Spark query result")</span></span>

    <span data-ttu-id="49903-155">Výsledky můžete také zobrazit v dalších vizualizacích.</span><span class="sxs-lookup"><span data-stu-id="49903-155">You can also see the results in other visualizations as well.</span></span> <span data-ttu-id="49903-156">Například plošný graf pro stejný výstup bude vypadat následovně.</span><span class="sxs-lookup"><span data-stu-id="49903-156">For example, an area graph for the same output would look like the following.</span></span>

    <span data-ttu-id="49903-157">![Plošný graf výsledku interaktivního dotazu Spark](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Plošný graf výsledku interaktivního dotazu Spark")</span><span class="sxs-lookup"><span data-stu-id="49903-157">![Area graph of interactive Spark query result](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Area graph of interactive Spark query result")</span></span>

9. <span data-ttu-id="49903-158">Po dokončení spuštění aplikace můžete poznámkový blok vypnout a uvolnit tak prostředky clusteru.</span><span class="sxs-lookup"><span data-stu-id="49903-158">Shut down the notebook to release the cluster resources after you have finished running the application.</span></span> <span data-ttu-id="49903-159">To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku.</span><span class="sxs-lookup"><span data-stu-id="49903-159">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span>

## <a name="next-step"></a><span data-ttu-id="49903-160">Další krok</span><span class="sxs-lookup"><span data-stu-id="49903-160">Next step</span></span>

<span data-ttu-id="49903-161">V tomto článku jste se naučili ke spuštění interaktivních dotazů v Spark pomocí poznámkového bloku Jupyter.</span><span class="sxs-lookup"><span data-stu-id="49903-161">In this article you learned how to run interactive queries in Spark using Jupyter notebook.</span></span> <span data-ttu-id="49903-162">Přechodu na další článek a zobrazit, jak mohou být vyžádány data, která jste zaregistrovali v Spark do nástroj pro analýzu BI, například Power BI a Tableau.</span><span class="sxs-lookup"><span data-stu-id="49903-162">Advance to the next article to see how the data you registered in Spark can be pulled into a BI analytics tool such as Power BI and Tableau.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="49903-163">Spark BI nástroje vizualizaci dat pomocí Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="49903-163">Spark BI using data visualization tools with Azure HDInsight</span></span>](hdinsight-apache-spark-use-bi-tools.md)




