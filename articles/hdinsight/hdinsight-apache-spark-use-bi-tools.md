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
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI nástroje vizualizaci dat pomocí Azure HDInsight

Zjistěte, jak vizualizace dat toouse nástroje například Power BI a Tableau tooanalyze nezpracovaná ukázkové sady dat pomocí Apache Spark BI v clusterech prostředí HDInsight.

> [!NOTE]
> 2.1 Spark v Azure HDInsight 3.6 Preview nepodporuje připojení pomocí nástrojů BI, které jsou popsané v tomto článku. Pouze Spark verze 1.6 a 2.0 (HDInsight 3,4, 3.5 v uvedeném pořadí) jsou podporovány.
>

V tomto kurzu je také k dispozici jako poznámkový blok Jupyter v clusteru HDInsight Spark. Hello poznámkového bloku prostředí vám umožní spustit hello Python fragmenty z Poznámkový blok hello sám sebe. kurz hello tooperform z v rámci Poznámkový blok, vytvořte Spark cluster, spusťte poznámkového bloku Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a poté spusťte Poznámkový blok hello **nástrojů BI použití s Apache Spark v HDInsight.ipynb** pod hello **Python**  složky.

## <a name="prerequisites"></a>Požadavky

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="hivetable"></a>Příprava dat pro vizualizaci dat Spark

V této části používáme hello [Jupyter](https://jupyter.org) poznámkového bloku z HDInsight Spark clusteru toorun úloh, které zpracování nezpracovaných ukázkových dat a uložte ho jako tabulku. Hello ukázkových dat je soubor .csv (hvac.csv) k dispozici na všech clusterech ve výchozím nastavení. Vaše data uložena jako tabulku, v další části hello jsme BI nástroje tooconnect toohello tabulka a provádět vizualizaci dat.

> [!NOTE]
> Pokud provádíte hello kroky v tomto článku po dokončení hello pokyny v [Spusťte interaktivní dotazy na clusteru HDInsight Spark](hdinsight-apache-spark-load-data-run-query.md), můžete přeskočit tooStep 8 níže.
>

1. Z hello [portál Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel). Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.   

2. Z okna clusteru Spark hello, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.

   > [!NOTE]
   > Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** s hello název clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Vytvořte poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

    ![Vytvoření poznámkového bloku Jupyter pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "vytvoření poznámkového bloku Jupyter pro Apache Spark BI")

4. Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb. Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.

    ![Zadejte název poznámkového bloku hello Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "zadejte název poznámkového bloku hello Apache Spark BI")

5. Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně. kontexty Spark a Hive Hello jsou pro vás automaticky vytvoří při spuštění první buňky kódu hello. Můžete začít importem typů hello nezbytných pro tento scénář. toodo Ano, umístěte kurzor hello hello buňky a stiskněte klávesu **SHIFT + ENTER**.

        from pyspark.sql import *

6. Načtěte vzorová data do dočasné tabulky. Když vytvoříte Spark cluster v HDInsight, ukázkový datový soubor hello, **hvac.csv**, je zkopírovaný toohello přidruženého účtu úložiště **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    Do prázdné buňky vložte hello následující fragment kódu a stiskněte klávesu **SHIFT + ENTER**. Tento fragment kódu zaregistruje hello dat do tabulky názvem **TVK**.

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

7. Ověřte, že tato tabulka hello byl úspěšně vytvořen. Můžete použít hello `%%sql` kouzelná dotazů Hive toorun přímo. Další informace o hello `%%sql` magic a dalších Magic, které jsou k dispozici s jádrem pyspark hello, najdete v části [jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SHOW TABLES

    Zobrazí výstup jako vidíte níže:

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    Pouze hello tabulky, které mají hodnotu false v rámci hello **isTemporary** sloupce jsou tabulek hive, které jsou uložené v hello metaúložiště a je přístupný z nástrojů BI hello. V tomto kurzu se nám připojit toohello **TVK** jsme vytvořili tabulku.

8. Ověřte, že tato tabulka hello obsahuje hello určená data. V prázdné buňky v poznámkovém bloku hello zkopírovat hello následující fragment kódu a stiskněte klávesu **SHIFT + ENTER**.

        %%sql
        SELECT * FROM hvac LIMIT 10

9. Vypněte hello poznámkového bloku toorelease hello prostředky. toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**.

## <a name="powerbi"></a>Pomocí Power BI pro vizualizaci dat Spark

> [!NOTE]
> Tato část je určena pouze pro 1.6 Spark na HDInsight 3.4 a 2.0 Spark na HDInsight 3.5.
>
>

Po uložení dat hello jako tabulku, můžete pomocí Power BI tooconnect toohello dat a vizualizovat ho toocreate sestavy, řídicí panely, atd.

1. Ujistěte se, že máte přístup tooPower BI. Můžete získat bezplatnou verzi preview odběr služby Power BI z [http://www.powerbi.com/](http://www.powerbi.com/).

2. Přihlaste se příliš[Power BI](http://www.powerbi.com/).

3. Hello dolní části levého podokna hello, klikněte na tlačítko **načíst Data**.

4. Na hello **načíst Data** v části **Import nebo připojení tooData**, pro **databáze**, klikněte na tlačítko **získat**.

    ![Získání dat do Power BI pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "načíst data do Power BI pro Apache Spark BI")

5. Na další obrazovce hello, klikněte na tlačítko **Spark v Azure HDInsight** a pak klikněte na **Connect**. Po zobrazení výzvy zadejte adresu URL clusteru hello (`mysparkcluster.azurehdinsight.net`) a hello pověření tooconnect toohello clusteru.

    ![Připojit tooApache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "připojit tooApache Spark BI")

    Po navázání připojení hello Power BI spustí import dat z hello clusteru Spark v HDInsight.

6. Importuje hello data Power BI a přidá **Spark** datovou sadu v části hello **datové sady** záhlaví. Klikněte na tlačítko tooopen hello sady dat nová data hello toovisualize listu. Můžete také uložit hello listu jako sestavu. toosave listu, z hello **soubor** nabídky, klikněte na tlačítko **Uložit**.

    ![Apache Spark BI dlaždice na řídicím panelu Power BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI dlaždice na řídicím panelu Power BI")
7. Všimněte si, že hello **pole** seznamu na pravé hello uvádí hello **TVK** tabulky, které jste vytvořili dříve. Rozbalte hello tabulky toosee hello pole v tabulce hello dříve definovaný v poznámkovém bloku.

      ![Seznam tabulek na řídicí panel Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "seznam tabulek na řídicí panel Apache Spark BI")

8. Sestavení vizualizace tooshow hello odchylka mezi teploty cíl a skutečný teploty pro každé sestavení. toovisualize chtěli dat, vyberte **plošný graf** (zobrazené v červeným rámečkem). toodefine hello osy, přetažení myší hello **BuildingID** pole v části **osy**, a **ActualTemp**/**TargetTemp** pole v části **hodnotu**.

    ![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")

9. Ve výchozím nastavení zobrazuje hello vizualizace hello součet **ActualTemp** a **TargetTemp**. Obě hello pole z hello rozevíracího seznamu vyberte **průměrná** tooget v průměru skutečné a teploty cíl pro obě budovy.

    ![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")

10. Vaše vizualizace dat by měla být podobné toohello jeden hello snímku obrazovky. Přesuňte kurzor hello vizualizace tooget popisy s příslušnými daty.

    ![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")

11. Klikněte na tlačítko **Uložit** z hello top nabídky a zadejte název sestavy. Můžete taky připnout hello visual. Pokud připnete vizualizace, je uložený na řídicím panelu, můžete sledovat hello nejnovější hodnotu na první pohled.

   Můžete přidat libovolný počet vizualizace tak, jak chcete pro hello stejné datové sady a připnete ji toohello řídicí panel pro snímek vaše data. Clustery Spark v HDInsight jsou také připojené tooPower BI s protokolem direct connect. Tím se zajistí, že má Power BI vždy hello většina aktuální data z clusteru, není nutné tooschedule aktualizace pro datovou sadu hello.

## <a name="tableau"></a>Použijte plochu Tableau pro vizualizaci dat Spark

> [!NOTE]
> Tato část je určena pouze pro clustery Spark 1.5.2 vytvořené v Azure HDInsight.
>
>

1. Nainstalujte [Tableau plochy](http://www.tableau.com/products/desktop) na kterém je spuštěn v tomto kurzu Apache Spark BI počítači hello.

2. Ujistěte se, že tento počítač má také nainstalovaný ovladač Microsoft Spark ODBC. Můžete nainstalovat ovladač hello z [zde](http://go.microsoft.com/fwlink/?LinkId=616229).

1. Spuštění Tableau plochy. V levém podokně hello hello seznamu tooconnect serveru, klikněte na **Spark SQL**. Pokud není ve výchozím nastavení v levém podokně hello zobrazí Spark SQL, můžete nějakého najít kliknutím **více serverů**.
2. V hello Spark SQL připojení dialogové okno, zadejte hodnoty hello, jak je znázorněno v hello snímek a potom klikněte na **OK**.

    ![Připojit tooa cluster Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect tooa cluster Apache Spark BI")

    Hello ověřování rozevírací seznamy **služby Microsoft Azure HDInsight** jako možnost, pouze v případě, že jste nainstalovali hello [ovladač ODBC Microsoft Sparku](http://go.microsoft.com/fwlink/?LinkId=616229) v počítači hello.
3. Na další obrazovce hello z hello **schématu** rozevíracího seznamu, klikněte na tlačítko hello **najít** ikonu a pak klikněte na tlačítko **výchozí**.

    ![Nalezeno schéma pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "najít schéma pro Apache Spark BI")
4. Pro hello **tabulky** pole, klikněte na tlačítko hello **najít** ikonu znovu toolist všechny hello Hive v clusteru hello dostupných tabulek. Měli byste vidět hello **TVK** tabulky, které jste vytvořili dříve pomocí poznámkového bloku hello.

    ![Najít tabulku pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "najít tabulku pro Apache Spark BI")
5. Přetáhnout myší nejvyšší poli toohello hello tabulky na pravé hello. Tableau importuje hello data a zobrazí hello schématu jako zvýrazněná podle hello red pole.

    ![Přidání tabulky tooTableau pro Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "přidat tooTableau tabulky pro Apache Spark BI")
6. Klikněte na tlačítko hello **Sheet1** karta v dolní části hello vlevo. Ujistěte se, vizualizace, který ukazuje hello průměrná cíl a skutečný teploty pro všechny budovy pro jednotlivá data. Přetáhněte **datum** a **ID budovy** příliš**sloupce** a **skutečné Temp**/**cíl Temp**příliš**řádky**. V části **značky**, vyberte **oblasti** toouse oblasti mapy pro vizualizaci dat Spark.

     ![Přidat pole pro vizualizaci dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "přidat pole pro vizualizaci dat Spark")
7. Ve výchozím nastavení, jsou uvedeny hello teploty pole jako agregace. Pokud chcete místo toho tooshow hello průměrné teploty, můžete provést tak z hello rozevíracího seznamu, jak je uvedeno níže.

    ![Trvat průměrná z teploty pro vizualizaci dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "trvat průměrná z teploty pro vizualizaci dat Spark")

8. Můžete také super použít jedna mapa teploty přes hello jiných tooget lepší chování rozdíl mezi cíl a skutečný teploty. Přesuňte hello myši toohello rohu hello nižší oblasti mapy, dokud najdete v části obrazce popisovač hello zvýrazněných v červeném kroužku. Přetáhněte hello mapy toohello další mapy na hello top a verze hello myši až uvidíte hello tvar zvýrazněných v červeným rámečkem.

    ![Sloučení mapy pro vizualizaci dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "sloučení mapy pro vizualizaci dat Spark")

     Vaše vizualizace dat měli změnit, jak je znázorněno v hello – snímek obrazovky:

    ![Výstup tableau vizualizace dat Spark](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau výstup vizualizace dat Spark")
9. Klikněte na tlačítko **Uložit** toosave hello listu. Můžete vytvořit řídicí panely a přidejte jeden nebo více stylů tooit.

## <a name="next-steps"></a>Další kroky

Pokud jste zjistili, jak vytvořit dat Spark datového rámce tooquery toocreate cluster s podporou a poté přístup k datům z nástrojů BI. Teď můžete prohlížet pokyny, jak toomanage hello prostředky clusteru a ladění úloh, které jsou spuštěné v clusteru služby HDInsight Spark.

* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)

