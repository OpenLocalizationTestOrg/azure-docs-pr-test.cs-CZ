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
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a>Spusťte interaktivní dotazy na clusteru HDInsight Spark

V tomto článku pomocí poznámkového bloku Jupyter ke spouštění interaktivních dotazů Spark SQL na clusteru Spark. Poznámkový blok Jupyter je aplikace založené na prohlížeči, která rozšiřuje možnosti interaktivního pomocí konzoly na webu. Další informace najdete v tématu [Poznámkový blok Jupyter](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).

V tomto kurzu použijete **PySpark** jádra v poznámkového bloku Jupyter ke spuštění interaktivních dotazů Spark SQL. Poznámkové bloky Jupyter v clusterech HDInsight také podporují dva další jádra - **PySpark3** a **Spark**. Další informace o jádrech a výhody použití **PySpark**, najdete v části [clusterů jádra poznámkového bloku Jupyter použít s Apache Spark v HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Požadavky

* **Clusteru Azure HDInsight Spark**. Pokyny najdete v tématu [vytvářet cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-to-run-interactive-queries"></a>Vytvoření poznámkového bloku Jupyter ke spuštění interaktivních dotazů

Spuštění dotazů, použijeme ukázková data, která je ve výchozím nastavení k dispozici v úložišti, které jsou přidruženy ke clusteru. Ale je nutné nejdřív načíst data do Spark jako dataframe. Jakmile máte dataframe, můžete spouštět dotazy na pomocí poznámkového bloku Jupyter. V této části vám tak informace o tom, jak:

* Zaregistrujte se jako Spark dataframe Ukázka datové sady.
* Na dataframe spouštět dotazy.

1. Otevřete web [Azure Portal](https://portal.azure.com/). Pokud jste se rozhodli připnout cluster na řídicí panel, kliknutím na dlaždici clusteru na řídicím panelu spusťte okno clusteru.

    Pokud jste cluster na řídicí panel nepřipnuli, v levém podokně klikněte na **Clustery HDInsight** a pak klikněte na cluster, který jste vytvořili.

3. V části **Rychlé odkazy** klikněte na **Řídicí panely clusteru** a potom klikněte na **Poznámkový blok Jupyter**. Po vyzvání zadejte přihlašovací údaje správce clusteru.

   ![Otevření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "Otevření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL")

   > [!NOTE]
   > K poznámkovému bloku Jupyter pro váš cluster se dostanete také otevřením následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** názvem clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Vytvořte poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

   ![Vytvoření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Vytvoření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL")

   Nový poznámkový blok se vytvoří a otevře s názvem Bez názvu (Bez názvu.pynb).

4. Pokud chcete, klikněte na název poznámkového bloku v horní části a zadejte popisný název.

    ![Zadání názvu, ze kterého má poznámkový blok Jupyter spustit interaktivní dotaz Spark](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "Zadání názvu, ze kterého má poznámkový blok Jupyter spustit interaktivní dotaz Spark")

5. Do prázdné buňky vložte následující kód a stisknutím **SHIFT + ENTER** kód spusťte. Kód naimportuje typy potřebné pro tento scénář:

        from pyspark.sql.types import *

    Vzhledem k tomu, že jste poznámkový blok vytvořili pomocí jádra PySpark, není nutné explicitně tvořit kontexty. Kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu.

    ![Stav interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Stav interaktivního dotazu Spark SQL")

    Při každém spuštění interaktivního dotazu v Jupyter se název okna webového prohlížeče zobrazí jako **(Zaneprázdněn)** společně s názvem poznámkového bloku. Zobrazí se také plný kroužek vedle textu **PySpark** v pravém horním rohu. Po dokončení úlohy se změní na prázdný kruh.

6. Před načtením dat do clusteru Spark, dejte nám vypadat snímek ho. Ukázková data použili v tomto kurzu jsou k dispozici jako soubor CSV na všechny clustery HDInsight Spark v **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. Data zaznamená změny teploty budovy. Zde naleznete několik prvních řádků data.

    ![Snímek dat pro interaktivních dotazů Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "snímek dat pro interaktivních dotazů Spark SQL")

6. Vytvoření dataframe a do dočasné tabulky (**TVK**) tak, že spustíte následující kód. V tomto kurzu jsme nevytvářejte všechny sloupce v dočasné tabulce porovnání sloupce v nezpracovaných dat sdíleného svazku clusteru. 

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

7. Po vytvoření tabulky, spusťte interaktivní dotaz na data, použijte následující kód.

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Vzhledem k tomu, že používáte jádro PySpark, můžete nyní přímo spustit interaktivní dotaz SQL nad dočasnou tabulkou **hvac**, kterou jste vytvořili pomocí magických příkazů `%%sql`. Další informace o magických příkazech `%%sql` a dalších magických příkazech, které jsou k dispozici s jádrem PySpark, najdete v části [Jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   Ve výchozím nastavení se zobrazí následující tabulkový výstup.

     ![Tabulkový výstup výsledku interaktivního dotazu Spark](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabulkový výstup výsledku interaktivního dotazu Spark")

    Výsledky můžete také zobrazit v dalších vizualizacích. Například plošný graf pro stejný výstup bude vypadat následovně.

    ![Plošný graf výsledku interaktivního dotazu Spark](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Plošný graf výsledku interaktivního dotazu Spark")

9. Po dokončení spuštění aplikace můžete poznámkový blok vypnout a uvolnit tak prostředky clusteru. To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku.

## <a name="next-step"></a>Další krok

V tomto článku jste se naučili ke spuštění interaktivních dotazů v Spark pomocí poznámkového bloku Jupyter. Přechodu na další článek a zobrazit, jak mohou být vyžádány data, která jste zaregistrovali v Spark do nástroj pro analýzu BI, například Power BI a Tableau. 

> [!div class="nextstepaction"]
>[Spark BI nástroje vizualizaci dat pomocí Azure HDInsight](hdinsight-apache-spark-use-bi-tools.md)




