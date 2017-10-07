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
# <a name="use-hdinsight-spark-cluster-tooanalyze-data-in-data-lake-store"></a>Použít data tooanalyze clusteru HDInsight Spark v Data Lake Store

V tomto kurzu použijete k dispozici poznámkového bloku Jupyter s toorun clustery HDInsight Spark úlohu, která čte data z účtu Data Lake Store.

## <a name="prerequisites"></a>Požadavky

* Účet Azure Data Lake Store. Postupujte podle pokynů hello [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](../data-lake-store/data-lake-store-get-started-portal.md).

* Cluster Azure HDInsight Spark s Data Lake Store jako úložiště. Postupujte podle pokynů hello [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    
## <a name="prepare-hello-data"></a>Příprava dat hello

> [!NOTE]
> Není nutné tooperform tento krok Pokud jste vytvořili hello clusteru HDInsight s Data Lake Store jako výchozí úložiště. procesy vytváření clusteru Hello přidá ukázková data v účtu Data Lake Store hello, který zadáte při vytváření clusteru hello. Přeskočte část toohello [clusteru používejte HDInsight Spark s Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).
>
>

Pokud jste vytvořili clusteru HDInsight s Data Lake Store jako další úložiště a Azure Blob Storage jako výchozí úložiště, měli byste se nejprve zkopírovat přes toohello některých ukázkových dat účtu Data Lake Store. Můžete použít hello ukázkových dat z Azure Storage Blob přidruženého k clusteru HDInsight hello hello. Můžete použít hello [ADLCopy nástroj](http://aka.ms/downloadadlcopy) toodo tak. Stáhněte a nainstalujte nástroj hello hello odkaz.

1. Otevřete příkazový řádek a přejděte toohello directory AdlCopy nainstalovanou, obvykle `%HOMEPATH%\Documents\adlcopy`.

2. Spusťte následující příkaz toocopy hello konkrétní objekt blob z hello zdrojový kontejner tooa Data Lake Store:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Kopírování hello **HVAC.csv** ukázková data souborů **/HdiSamples/HdiSamples/SensorSampleData/TVK/** toohello účtu Azure Data Lake Store. fragment kódu Hello by měl vypadat podobně jako:

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > Zajistěte, aby hello souboru a v případě správné hello jsou názvy cest.
   >
   >
3. Bude výzvami tooenter hello přihlašovací údaje pro hello předplatné Azure, ve kterém máte účtu Data Lake Store. Zobrazí se výstup podobný toohello následující:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    Hello datový soubor (**HVAC.csv**) se zkopírují složce **/hvac** v hello účtu Data Lake Store.

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a>Používání clusteru HDInsight Spark s Data Lake Store

1. Z hello [portálu Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel). Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.

2. Z okna clusteru Spark hello, klikněte na tlačítko **rychlé odkazy**a potom z hello **řídicí panel clusteru** okně klikněte na tlačítko **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.

   > [!NOTE]
   > Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** s hello název clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Vytvořte nový poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

    ![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Vytvoření nového poznámkového bloku Jupyter")

4. Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně. Hello kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu hello. Můžete začít importem typů hello nezbytných pro tento scénář. toodo Ano, vložte následující fragment kódu do buňky a stiskněte klávesu hello **SHIFT + ENTER**.

        from pyspark.sql.types import *

    Při každém spuštění úlohy v Jupyter se název okna webového prohlížeče zobrazí **(zaneprázdněn)** společně s názvem poznámkového bloku hello. Zobrazí se také další toohello plný kroužek **PySpark** text v pravém horním rohu hello. Po dokončení úlohy hello to změní tooa prázdný kruh.

     ![Stav úlohy poznámkového bloku Jupyter](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Stav úlohy poznámkového bloku Jupyter")

5. Načíst ukázková data do dočasné tabulky pomocí hello **HVAC.csv** souborů, které jste zkopírovali toohello účtu Data Lake Store. Můžete přistupovat hello dat v účtu Data Lake Store hello pomocí hello následující vzor adresy URL.

    * Pokud máte Data Lake Store jako výchozí úložiště, HVAC.csv budou v hello cesta podobné toohello následující adresu URL:

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        Nebo můžete použít taky zkrácení formátu například hello následující:

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * Pokud máte Data Lake Store jako další úložiště, bude mít HVAC.csv hello umístění, kam jste zkopírovali, jako například:

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     V prázdné buňky vložte hello následující ukázka kódu, nahraďte **MYDATALAKESTORE** s názvem účtu Data Lake Store a stiskněte klávesu **SHIFT + ENTER**. Tento ukázkový kód registruje hello data do dočasné tabulky nazývané **TVK**.

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

6. Vzhledem k tomu, že používáte jádro PySpark, můžete nyní přímo spustit dotaz SQL na dočasnou tabulku hello **TVK** , že jste právě vytvořili pomocí hello `%%sql` magic. Další informace o hello `%%sql` magic a také dalších Magic, které jsou k dispozici s jádrem pyspark hello, najdete v části [jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. Po úspěšném dokončení úlohy hello je ve výchozím nastavení zobrazí následující tabulkový výstup hello.

      ![Tabulkový výstup výsledků dotazu](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Tabulkový výstup výsledků dotazu")

     Můžete také zjistit hello výsledky v dalších vizualizacích. Například plošný graf pro stejný výstup bude vypadat hello následující hello.

     ![Plošný graf výsledku dotazu](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Plošný graf výsledku dotazu")

8. Po dokončení spuštění aplikace hello, měli byste vypnout hello poznámkového bloku toorelease hello prostředky. toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**. Dojde k vypnutí a zavřít hello Poznámkový blok.


## <a name="next-steps"></a>Další kroky

* [Vytvoření samostatné Scala aplikace toorun na cluster Apache Spark](hdinsight-apache-spark-create-standalone-application.md)
* [Používat nástroje HDInsight pro IntelliJ toocreate aplikací Spark pro cluster HDInsight Spark Linux v Azure nástrojů](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Použití nástrojů HDInsight v Azure nástrojů Eclipse toocreate Spark aplikací pro cluster HDInsight Spark Linux](hdinsight-apache-spark-eclipse-tool-plugin.md)
