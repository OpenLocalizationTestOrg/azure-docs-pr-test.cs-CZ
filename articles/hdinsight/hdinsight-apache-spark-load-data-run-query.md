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
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a>Spusťte interaktivní dotazy na clusteru HDInsight Spark

V tomto článku použijte Jupyter poznámkového bloku toorun interaktivních dotazů Spark SQL na clusteru Spark. Poznámkový blok Jupyter je aplikace založené na prohlížeči, která rozšiřuje hello pomocí konzoly interaktivní možnosti toohello Web. Další informace najdete v tématu [hello Poznámkový blok Jupyter](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).

V tomto kurzu použijete hello **PySpark** jádra v toorun poznámkového bloku Jupyter hello interaktivních dotazů Spark SQL. Poznámkové bloky Jupyter v clusterech HDInsight také podporují dva další jádra - **PySpark3** a **Spark**. Další informace o hello jádra a hello výhody použití **PySpark**, najdete v části [clusterů jádra poznámkového bloku Jupyter použít s Apache Spark v HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Požadavky

* **Clusteru Azure HDInsight Spark**. Pokyny najdete v tématu [vytvářet cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-toorun-interactive-queries"></a>Vytvoření poznámkového bloku Jupyter toorun interaktivních dotazů

dotazy toorun používáme ukázková data, která je ve výchozím nastavení k dispozici v hello úložiště přidruženého k hello clusteru. Ale je nutné nejdřív načíst data do Spark jako dataframe. Až budete mít hello dataframe, můžete spouštět dotazy na pomocí poznámkového bloku Jupyter hello. V této části vám tak informace o tom, jak:

* Zaregistrujte se jako Spark dataframe Ukázka datové sady.
* Spuštění dotazů na hello dataframe.

1. Otevřete hello [portál Azure](https://portal.azure.com/). Když jste se rozhodli řídicí panel toohello toopin hello clusteru, klikněte na dlaždici hello clusteru z hello řídicí panel toolaunch hello clusteru okno.

    Pokud připnete není hello toohello řídicí panel clusteru, v levém podokně hello, klikněte na tlačítko **clustery HDInsight**a potom klikněte na cluster hello jste vytvořili.

3. V části **Rychlé odkazy** klikněte na **Řídicí panely clusteru** a potom klikněte na **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.

   ![Otevřete Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "poznámkového bloku Jupyter otevřete toorun interaktivní Spark SQL dotazu")

   > [!NOTE]
   > Může také přístup k hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** s hello název clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Vytvořte poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

   ![Vytvořit Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "vytvořit Jupyter poznámkového bloku toorun interaktivní Spark SQL dotaz")

   Nový poznámkový blok se vytvoří a otevřít s názvem hello Untitled(Untitled.pynb).

4. Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název, pokud chcete.

    ![Zadejte název pro hello Jupter poznámkového bloku toorun interaktivní Spark dotaz z](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "zadejte název pro hello Jupter poznámkového bloku toorun interaktivní Spark dotaz z")

5. Následující hello vložení kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER** toorun hello kódu. Hello kód importuje hello typů nezbytných pro tento scénář:

        from pyspark.sql.types import *

    Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně. kontexty Spark a Hive Hello jsou pro vás automaticky vytvoří při spuštění první buňky kódu hello.

    ![Stav interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Stav interaktivního dotazu Spark SQL")

    Při každém spuštění interaktivních dotazů v Jupyter se název okna webového prohlížeče ukazuje **(zaneprázdněn)** společně s názvem poznámkového bloku hello. Zobrazí také další toohello plný kroužek **PySpark** text v pravém horním rohu hello. Po dokončení úlohy hello změní tooa prázdný kruh.

6. Před načtením dat hello do clusteru Spark, dejte nám vypadat snímek ho. Hello ukázková data použili v tomto kurzu jsou k dispozici jako soubor CSV na všechny clustery HDInsight Spark v **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. Hello data zaznamená změny teploty hello budovy. Tady jsou hello několik prvních řádků dat hello.

    ![Snímek dat pro interaktivních dotazů Spark SQL](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "snímek dat pro interaktivních dotazů Spark SQL")

6. Vytvoření dataframe a do dočasné tabulky (**TVK**) tak, že spustíte hello následující kód. V tomto kurzu jsme nevytvářejte všechny sloupce hello v dočasné tabulce hello jako porovnání toohello sloupce hello nezpracovaná data ve formátu CSV. 

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

7. Po vytvoření tabulky hello Spusťte interaktivní dotaz na hello data, použijte následující kód hello.

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Vzhledem k tomu, že používáte jádro PySpark, můžete nyní přímo spustit interaktivní dotazu SQL na dočasnou tabulku hello **TVK** jste vytvořili pomocí hello `%%sql` magic. Další informace o hello `%%sql` magic a dalších Magic, které jsou k dispozici s jádrem pyspark hello, najdete v části [jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   ve výchozím nastavení se zobrazí následující tabulkový výstup Hello.

     ![Tabulkový výstup výsledku interaktivního dotazu Spark](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabulkový výstup výsledku interaktivního dotazu Spark")

    Můžete také zjistit hello výsledky v dalších vizualizacích. Například plošný graf pro stejný výstup bude vypadat hello následující hello.

    ![Plošný graf výsledku interaktivního dotazu Spark](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Plošný graf výsledku interaktivního dotazu Spark")

9. Po dokončení spuštění aplikace hello vypněte prostředky clusteru hello poznámkového bloku toorelease hello. toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**.

## <a name="next-step"></a>Další krok

V tomto článku jste se naučili jak toorun interaktivních dotazů v Spark pomocí poznámkového bloku Jupyter. Posunutí toohello další článek toosee jak mohou být vyžádány hello data, která jste zaregistrovali v Spark do nástroj pro analýzu BI, například Power BI a Tableau. 

> [!div class="nextstepaction"]
>[Spark BI nástroje vizualizaci dat pomocí Azure HDInsight](hdinsight-apache-spark-use-bi-tools.md)




