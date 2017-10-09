---
title: "aaaAnalyze přehled aplikace protokoly s Spark - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooexport přehled aplikace protokoly tooblob úložiště a pak Analýza protokolů hello pomocí Spark v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 883beae6-9839-45b5-94f7-7eb0f4534ad5
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 11ed8cf68dba8d5f9d6e4a65eba0d2b5a950cd00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-application-insights-telemetry-logs-with-spark-on-hdinsight"></a>Analýza protokolů telemetrie Application Insights pomocí Spark v HDInsight

Zjistěte, jak toouse Spark na HDInsight tooanalyze přehled aplikace telemetrická data.

[Visual Studio Application Insights](../application-insights/app-insights-overview.md) je služba analýzy, která monitoruje webové aplikace. Telemetrická data generované Application Insights může být exportovaný tooAzure úložiště. Jakmile hello dat ve službě Azure Storage, HDInsight lze použít tooanalyze ho.

## <a name="prerequisites"></a>Požadavky

* Aplikace, která je nakonfigurovaná toouse Application Insights.

* Znalost vytvoření clusteru HDInsight se systémem Linux. Další informace najdete v tématu [vytvořit Spark v HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

  > [!IMPORTANT]
  > Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Webový prohlížeč.

Hello následující prostředky byly používány v vývoj a testování tohoto dokumentu:

* Application Insights telemetrická data byla generována pomocí [webové aplikace Node.js nakonfiguroval službu toouse Application Insights](../application-insights/app-insights-nodejs.md).

* Spark založené na verzi clusteru HDInsight 3.5 se použité tooanalyze hello data.

## <a name="architecture-and-planning"></a>Plánování a architektura

Hello následující diagram znázorňuje architektura služby hello tohoto příkladu:

![Diagram zobrazující dat odesílaných ze služby Application Insights tooblob úložiště a pak zpracovávaných Spark v HDInsight](./media/hdinsight-spark-analyze-application-insight-logs/appinsightshdinsight.png)

### <a name="azure-storage"></a>Úložiště Azure

Application Insights může být nakonfigurované toocontinuously export telemetrie informace tooblobs. HDInsight můžete pak přečte data uložená v hello objekty BLOB. Existuje však několik požadavků, které je třeba postupovat podle:

* **Umístění**: Pokud hello účtu úložiště a HDInsight jsou v různých umístěních, se může prodloužit latence. Také zvyšuje náklady, jsou s nimi spojeným nákladům použité toodata přesun mezi oblastmi.

    > [!WARNING]
    > Použití účtu úložiště v jiném umístění než HDInsight není podporováno.

* **Typ objektu BLOB**: HDInsight podporuje pouze objekty BLOB bloku. Application Insights výchozí toousing objekty BLOB bloku, takže by měla fungovat ve výchozím nastavení s HDInsight.

Informace o přidání dalšího úložiště tooan stávající HDInsight cluster, naleznete v části hello [přidat další účty úložiště](hdinsight-hadoop-add-storage.md) dokumentu.

### <a name="data-schema"></a>Schéma dat

Poskytuje služby Application Insights [Exportovat datový model](../application-insights/app-insights-export-data-model.md) tooblobs exportovat informace o formátu data telemetrie hello. Hello kroky v tomto dokumentu používají toowork Spark SQL s daty hello. Spark SQL můžete automaticky generovat schéma pro datovou strukturu JSON hello zaznamenaných Application Insights.

## <a name="export-telemetry-data"></a>Exportovat data telemetrie

Postupujte podle kroků hello v [nakonfigurovat nepřetržité exportovat](../application-insights/app-insights-export-telemetry.md) tooconfigure vaše Application Insights tooexport telemetrie informace tooan úložiště Azure blob.

## <a name="configure-hdinsight-tooaccess-hello-data"></a>Konfigurace HDInsight tooaccess hello dat

Při vytváření clusteru služby HDInsight, přidejte účet úložiště hello při vytváření clusteru.

tooadd hello účet úložiště Azure tooan existující cluster, použijte informace hello v hello [přidat další účty úložiště](hdinsight-hadoop-add-storage.md) dokumentu.

## <a name="analyze-hello-data-pyspark"></a>Analýza dat hello: PySpark

1. Z hello [portál Azure](https://portal.azure.com), vyberte vaše Spark v clusteru HDInsight. Z hello **rychlé odkazy** vyberte **řídicí panely clusteru**a potom vyberte **Poznámkový blok Jupyter** v okně clusteru Dashboard__ hello.

    ![řídicí panely Hello clusteru](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)

2. V hello pravém horním rohu stránky Jupyter hello, vyberte **nový**a potom **PySpark**. Otevře novou kartu prohlížeče obsahující poznámkového bloku Jupyter na základě Python.

3. V prvním poli hello (nazývá **buňky**) na stránce hello, zadejte následující text hello:

   ```python
   sc._jsc.hadoopConfiguration().set('mapreduce.input.fileinputformat.input.dir.recursive', 'true')
   ```

    Tento kód konfiguruje Spark toorecursively přístup hello adresářovou strukturu pro vstupní data hello. Telemetrii Application Insights je zaznamenané tooa directory struktura podobné toohello `/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Použití **SHIFT + ENTER** toorun hello kódu. Na levé straně hello buňky hello '\*, zobrazí se mezi tooindicate závorky hello je spouštěna hello kódu v této buňce. Po jeho dokončení hello '\*se změní tooa číslo a výstup podobný toohello následující text se zobrazí pod buňky hello:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    pyspark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Bude vytvořen nový buňku hello první z nich. Zadejte následující text v buňce nové hello hello. Nahraďte `CONTAINER` a `STORAGEACCOUNT` s názvem účtu úložiště Azure hello a název kontejneru objektu blob, který obsahuje data Application Insights.

   ```python
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Použití **SHIFT + ENTER** tooexecute tuto buňku. Zobrazí výsledek podobné toohello následující text:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Cesta wasb Hello vrátil je hello umístění hello Application Insights telemetrická data. Změna hello `hdfs dfs -ls` řádek v hello buňky toouse hello wasb cestu vrátila a pak použijte **SHIFT + ENTER** toorun hello znovu buňky. Tentokrát hello výsledky by měl zobrazit hello adresáře, které obsahují telemetrická data.

   > [!NOTE]
   > Pro zbytek hello hello kroky v této části, hello `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` adresáře byl použit. Struktury adresářů se může lišit.

6. V následující buňky hello, zadejte následující kód hello: Nahraďte `WASB_PATH` s cestou hello hello v předchozím kroku.

   ```python
   jsonFiles = sc.textFile('WASB_PATH')
   jsonData = sqlContext.read.json(jsonFiles)
   ```

    Tento kód vytvoří dataframe z soubory JSON hello exportované procesem průběžné export hello. Použití **SHIFT + ENTER** toorun tuto buňku.
7. V následující buňky hello zadejte a spusťte následující schéma hello tooview, Spark vytvořené pro soubory JSON hello hello:

   ```python
   jsonData.printSchema()
   ```

    Hello schématu pro každý typ telemetrie se liší. Hello následující příklad je hello schématu, který se vygeneruje pro webové žádosti (data uložená v hello `Requests` podadresáři):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)
8. Použijte následující tooregister hello dataframe jako dočasná tabulka hello a spuštění dotazu pro hello data:

   ```python
   jsonData.registerTempTable("requests")
   df = sqlContext.sql("select context.location.city from requests where context.location.city is not null")
   df.show()
   ```

    Tento dotaz vrací informace o městě hello hello nejvyšší 20 záznamů, kde context.location.city není null.

   > [!NOTE]
   > Struktura Hello kontextu není k dispozici ve všech telemetrických dat zaznamenaných funkcí Application Insights. element města Hello nemusí vložené do protokolů. Použijte hello schématu tooidentify další prvky s možností dotazu, které mohou obsahovat data pro svoje protokoly.

    Tento dotaz vrací informace o podobné toohello následující text:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="analyze-hello-data-scala"></a>Analýza dat hello: Scala

1. Z hello [portál Azure](https://portal.azure.com), vyberte vaše Spark v clusteru HDInsight. Z hello **rychlé odkazy** vyberte **řídicí panely clusteru**a potom vyberte **Poznámkový blok Jupyter** v okně clusteru Dashboard__ hello.

    ![řídicí panely Hello clusteru](./media/hdinsight-spark-analyze-application-insight-logs/clusterdashboards.png)
2. V hello pravém horním rohu stránky Jupyter hello, vyberte **nový**a potom **Scala**. Zobrazí se na nové záložce prohlížeče obsahující na základě Scala poznámkového bloku Jupyter.
3. V prvním poli hello (nazývá **buňky**) na stránce hello, zadejte následující text hello:

   ```scala
   sc.hadoopConfiguration.set("mapreduce.input.fileinputformat.input.dir.recursive", "true")
   ```

    Tento kód konfiguruje Spark toorecursively přístup hello adresářovou strukturu pro vstupní data hello. Application Insights telemetrie je zaznamenána tooa adresářovou strukturu podobné příliš`/{telemetry type}/YYYY-MM-DD/{##}/`.

4. Použití **SHIFT + ENTER** toorun hello kódu. Na levé straně hello buňky hello '\*, zobrazí se mezi tooindicate závorky hello je spouštěna hello kódu v této buňce. Po jeho dokončení hello '\*se změní tooa číslo a výstup podobný toohello následující text se zobrazí pod buňky hello:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    spark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...
5. Bude vytvořen nový buňku hello první z nich. Zadejte následující text v buňce nové hello hello. Nahraďte `CONTAINER` a `STORAGEACCOUNT` s názvem účtu úložiště Azure hello a název kontejneru objektu blob, který obsahuje Application Insights protokoly.

   ```scala
   %%bash
   hdfs dfs -ls wasb://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Použití **SHIFT + ENTER** tooexecute tuto buňku. Zobrazí výsledek podobné toohello následující text:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Cesta wasb Hello vrátil je hello umístění hello Application Insights telemetrická data. Změna hello `hdfs dfs -ls` řádek v hello buňky toouse hello wasb cestu vrátila a pak použijte **SHIFT + ENTER** toorun hello znovu buňky. Tentokrát hello výsledky by měl zobrazit hello adresáře, které obsahují telemetrická data.

   > [!NOTE]
   > Pro zbytek hello hello kroky v této části, hello `wasb://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` adresáře byl použit. Tento adresář neexistuje, není-li telemetrická data pro webovou aplikaci.

6. V následující buňky hello, zadejte následující kód hello: Nahraďte `WASB\_PATH` s cestou hello hello v předchozím kroku.

   ```scala
   var jsonFiles = sc.textFile('WASB_PATH')
   val sqlContext = new org.apache.spark.sql.SQLContext(sc)
   var jsonData = sqlContext.read.json(jsonFiles)
   ```

    Tento kód vytvoří dataframe z soubory JSON hello exportované procesem průběžné export hello. Použití **SHIFT + ENTER** toorun tuto buňku.

7. V následující buňky hello zadejte a spusťte následující schéma hello tooview, Spark vytvořené pro soubory JSON hello hello:

   ```scala
   jsonData.printSchema
   ```

    Hello schématu pro každý typ telemetrie se liší. Hello následující příklad je hello schématu, který se vygeneruje pro webové žádosti (data uložená v hello `Requests` podadresáři):

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)

8. Použijte následující tooregister hello dataframe jako dočasná tabulka hello a spuštění dotazu pro hello data:

   ```scala
   jsonData.registerTempTable("requests")
   var city = sqlContext.sql("select context.location.city from requests where context.location.city is not null limit 10").show()
   ```

    Tento dotaz vrací informace o městě hello hello nejvyšší 20 záznamů, kde context.location.city není null.

   > [!NOTE]
   > Struktura Hello kontextu není k dispozici ve všech telemetrických dat zaznamenaných funkcí Application Insights. element města Hello nemusí vložené do protokolů. Použijte hello schématu tooidentify další prvky s možností dotazu, které mohou obsahovat data pro svoje protokoly.
   >
   >

    Tento dotaz vrací informace o podobné toohello následující text:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="next-steps"></a>Další kroky

Další příklady použití Spark toowork s daty a služby v Azure najdete v tématu hello následující dokumenty:

* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro vytváření aplikací pro streamování](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

Informace o vytváření a spouštění aplikací Spark najdete v tématu hello následující dokumenty:

* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)
