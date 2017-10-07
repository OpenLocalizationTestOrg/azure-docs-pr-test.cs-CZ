---
title: "aaaAnalyze letu zpoždění dat pomocí Hive v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Hive tooanalyze letu dat na HDInsight se systémem Linux a pak exportovat hello data tooSQL databáze pomocí Sqoop."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0c23a079-981a-4079-b3f7-ad147b4609e5
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 7830457a7100880dff1c647dde1b4d203bfea3c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a>Analýza dat zpoždění letu pomocí Hive v HDInsight se systémem Linux

Zjistěte, jak exportovat tooanalyze letu zpoždění data pak používání Hive v HDInsight se systémem Linux hello data tooAzure SQL Database pomocí Sqoop.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

### <a name="prerequisites"></a>Požadavky

* **Cluster služby HDInsight**. V tématu [Začínáme pomocí Hadoop Hive v HDInsight v Linuxu](hdinsight-hadoop-linux-tutorial-get-started.md) kroky k vytvoření nového clusteru HDInsight se systémem Linux.

* **Azure SQL Database**. Jako cílového úložiště dat používáte Azure SQL database. Pokud již nemáte databázi SQL, najdete v části [kurz k SQL Database: vytvoření databáze SQL v minutách](../sql-database/sql-database-get-started.md).

* **Azure CLI**. Pokud jste nenainstalovali hello příkazového řádku Azure CLI, najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) pro další kroky.

## <a name="download-hello-flight-data"></a>Stáhněte si data pohybující se hello

1. Procházet příliš[výzkum a inovativní technologie správy, úřad Transport statistických][rita-website].

2. Na stránce hello vyberte hello následující hodnoty:

   | Name (Název) | Hodnota |
   | --- | --- |
   | Filtr roku |2013 |
   | Filtrovat období |Leden |
   | Pole |Rok, FlightDate, UniqueCarrier, operátora, FlightNum, OriginAirportID, původu, OriginCityName, OriginState, DestAirportID, cíle, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay NASDelay, SecurityDelay, LateAircraftDelay. Zrušte zaškrtnutí všech ostatních polí |

3. Klikněte na **Stáhnout**.

## <a name="upload-hello-data"></a>Nahrání dat hello

1. Použijte následující příkaz tooupload hello zip souboru toohello hlavního uzlu clusteru HDInsight hello:

    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

    Nahraďte **FILENAME** hello název souboru zip hello. Nahraďte **uživatelské jméno** s hello přihlašování přes SSH pro hello HDInsight cluster. Nahraďte název clusteru s názvem hello hello clusteru HDInsight.

   > [!NOTE]
   > Pokud použijete heslo tooauthenticate vaše přihlášení SSH, budete vyzváni k hello heslo. Pokud jste použili veřejný klíč, může být nutné toouse hello `-i` parametr a zadejte hello cesta toohello odpovídající privátní klíč. Například, `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Po dokončení nahrávání hello připojte toohello clusteru pomocí protokolu SSH:

    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```

    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Po připojení, použijte následující soubor .zip hello toounzip hello:

    ```
    unzip FILENAME.zip
    ```

    Tento příkaz extrahuje soubor CSV, který je přibližně 60 MB.

4. Použijte následující příkaz toocreate adresář na úložiště HDInsight hello a zkopírujte adresář toohello souboru hello:

    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-hello-hiveql"></a>Vytvoření a spuštění hello HiveQL

Použití hello následující kroky tooimport data ze souboru CSV hello do tabulky Hive s názvem **zpoždění**.

1. Použití hello následující příkaz toocreate a upravit nový soubor s názvem **flightdelays.hql**:

    ```
    nano flightdelays.hql
    ```

    Použijte hello následující text jako hello obsah tohoto souboru:

    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over hello csv file
    CREATE EXTERNAL TABLE delays_raw (
        YEAR string,
        FL_DATE string,
        UNIQUE_CARRIER string,
        CARRIER string,
        FL_NUM string,
        ORIGIN_AIRPORT_ID string,
        ORIGIN string,
        ORIGIN_CITY_NAME string,
        ORIGIN_CITY_NAME_TEMP string,
        ORIGIN_STATE_ABR string,
        DEST_AIRPORT_ID string,
        DEST string,
        DEST_CITY_NAME string,
        DEST_CITY_NAME_TEMP string,
        DEST_STATE_ABR string,
        DEP_DELAY_NEW float,
        ARR_DELAY_NEW float,
        CARRIER_DELAY float,
        WEATHER_DELAY float,
        NAS_DELAY float,
        SECURITY_DELAY float,
        LATE_AIRCRAFT_DELAY float)
    -- hello following lines describe hello format and location of hello file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION '/tutorials/flightdelays/data';

    -- Drop hello delays table if it exists
    DROP TABLE delays;
    -- Create hello delays table and populate it with data
    -- pulled in from hello CSV file (via hello external table defined previously)
    CREATE TABLE delays AS
    SELECT YEAR AS year,
        FL_DATE AS flight_date,
        substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
        substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
        substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
        ORIGIN_AIRPORT_ID AS origin_airport_id,
        substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
        substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
        substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
        DEST_AIRPORT_ID AS dest_airport_id,
        substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
        substring(DEST_CITY_NAME,2) AS dest_city_name,
        substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
        DEP_DELAY_NEW AS dep_delay_new,
        ARR_DELAY_NEW AS arr_delay_new,
        CARRIER_DELAY AS carrier_delay,
        WEATHER_DELAY AS weather_delay,
        NAS_DELAY AS nas_delay,
        SECURITY_DELAY AS security_delay,
        LATE_AIRCRAFT_DELAY AS late_aircraft_delay
    FROM delays_raw;
    ```

2. toosave hello soubor, použijte **kombinaci kláves Ctrl + X**, pak **Y** .

3. toostart Hive a spuštění hello **flightdelays.hql** soubor, použijte následující příkaz hello:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

   > [!NOTE]
   > V tomto příkladu `localhost` je použít, protože jsou připojené toohello hlavního uzlu v clusteru HDInsight hello, což je, kde je spuštěna HiveServer2.

4. Jednou hello __flightdelays.hql__ dokončení systémem použijte hello následující příkaz tooopen na interaktivní relace Beeline skriptu:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. Po přijetí hello `jdbc:hive2://localhost:10001/>` řádku, použijte následující tooretrieve dotaz na data z hello importovat letu zpoždění dat hello.

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    Tento dotaz načte seznam města, zpoždění Doba zpoždění zkušeného počasí, společně s hello průměr a uložte ho příliš`/tutorials/flightdelays/output`. Později Sqoop čte hello data z tohoto umístění a exportovat je tooAzure databáze SQL.

6. Zadejte tooexit Beeline, `!quit` řádku hello.

## <a name="create-a-sql-database"></a>Vytvoření databáze SQL

Pokud už máte databázi SQL, musíte získat název serveru hello. Název serveru hello můžete najít v hello [portál Azure](https://portal.azure.com) výběrem **databází SQL**, a potom na název hello hello filtrování databáze chcete toouse. název serveru Hello je uveden v hello **SERVER** sloupce.

Pokud již nemáte databázi SQL, použijte informace hello v [kurz k SQL Database: vytvoření databáze SQL v minutách](../sql-database/sql-database-get-started.md) toocreate jeden. Uložte hello název serveru použít pro databázi hello.

## <a name="create-a-sql-database-table"></a>Vytvořit tabulku databáze SQL

> [!NOTE]
> Existuje mnoho způsobů tooconnect tooSQL databázi a vytvořte tabulku. Následující postup použijte Hello [FreeTDS](http://www.freetds.org/) z clusteru HDInsight hello.


1. Použití clusteru HDInsight se systémem Linux toohello tooconnect SSH a spuštění hello postupem z relace SSH hello.

2. Použijte následující příkaz tooinstall FreeTDS hello:

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. Po dokončení instalace hello použijte hello následující příkaz tooconnect toohello databáze SQL serveru. Nahraďte **serverName** s názvem serveru SQL Database hello. Nahraďte **adminLogin** a **adminPassword** s hello přihlášení pro databázi SQL. Nahraďte **databaseName** s názvem databáze hello.

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    Zobrazí se výstup podobný toohello následující text:

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set toosqooptest
    1>
    ```

4. V hello `1>` výzva, zadejte hello následující řádky:

    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    Když hello `GO` příkaz je zadán, jsou vyhodnocovány hello předchozí příkazy. Tento dotaz vytvoří tabulku s názvem **zpoždění**, clusterovaný index.

    Použití hello následující tooverify dotazu, který hello tabulka byla vytvořena:

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    Hello výstup je podobné toohello následující text:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. Zadejte `exit` v hello `1>` výzvu tooexit hello tsql nástroj.

## <a name="export-data-with-sqoop"></a>Export dat s Sqoop

1. Použijte následující příkaz tooverify Sqoop můžete najdete v části SQL Database hello:

    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    Tento příkaz vrátí seznam databází, včetně hello databáze, kterou jste vytvořili dříve hello zpoždění tabulky v.

2. Použijte následující příkaz tooexport dat z tabulky mobiledata toohello hivesampletable hello:

    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    Sqoop připojí toohello databáze obsahující hello zpoždění tabulky a exportuje data z hello `/tutorials/flightdelays/output` tabulce zpoždění toohello adresáře.

3. Po dokončení příkazu hello použijte následující tooconnect toohello databázi pomocí TSQL hello:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    Po připojení, použijte následující příkazy tooverify že hello data byla exportovaný toohello mobiledata tabulky hello:

    ```
    SELECT * FROM delays
    GO
    ```

    Měli byste vidět seznam data v tabulce hello. Typ `exit` tooexit hello tsql nástroj.

## <a id="nextsteps"></a> Další kroky

najdete další způsoby toowork s daty v HDInsight, toolearn hello následující dokumenty:

* [Použití Hivu se službou HDInsight][hdinsight-use-hive]
* [Použijte Oozie s HDInsight][hdinsight-use-oozie]
* [Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]
* [Použití Pigu se službou HDInsight][hdinsight-use-pig]
* [Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]
* [Vývoj streamování programy pro HDInsight Hadoop Python][hdinsight-develop-streaming]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
