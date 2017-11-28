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
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a><span data-ttu-id="76116-103">Analýza dat zpoždění letu pomocí Hive v HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="76116-103">Analyze flight delay data by using Hive on Linux-based HDInsight</span></span>

<span data-ttu-id="76116-104">Zjistěte, jak exportovat tooanalyze letu zpoždění data pak používání Hive v HDInsight se systémem Linux hello data tooAzure SQL Database pomocí Sqoop.</span><span class="sxs-lookup"><span data-stu-id="76116-104">Learn how tooanalyze flight delay data using Hive on Linux-based HDInsight then export hello data tooAzure SQL Database using Sqoop.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="76116-105">Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="76116-105">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="76116-106">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="76116-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="76116-107">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="76116-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="76116-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="76116-108">Prerequisites</span></span>

* <span data-ttu-id="76116-109">**Cluster služby HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="76116-109">**An HDInsight cluster**.</span></span> <span data-ttu-id="76116-110">V tématu [Začínáme pomocí Hadoop Hive v HDInsight v Linuxu](hdinsight-hadoop-linux-tutorial-get-started.md) kroky k vytvoření nového clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="76116-110">See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md) for steps on creating a new Linux-based HDInsight cluster.</span></span>

* <span data-ttu-id="76116-111">**Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="76116-111">**Azure SQL Database**.</span></span> <span data-ttu-id="76116-112">Jako cílového úložiště dat používáte Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="76116-112">You use an Azure SQL database as a destination data store.</span></span> <span data-ttu-id="76116-113">Pokud již nemáte databázi SQL, najdete v části [kurz k SQL Database: vytvoření databáze SQL v minutách](../sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="76116-113">If you do not have a SQL Database already, see [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md).</span></span>

* <span data-ttu-id="76116-114">**Azure CLI**.</span><span class="sxs-lookup"><span data-stu-id="76116-114">**Azure CLI**.</span></span> <span data-ttu-id="76116-115">Pokud jste nenainstalovali hello příkazového řádku Azure CLI, najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) pro další kroky.</span><span class="sxs-lookup"><span data-stu-id="76116-115">If you have not installed hello Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) for more steps.</span></span>

## <a name="download-hello-flight-data"></a><span data-ttu-id="76116-116">Stáhněte si data pohybující se hello</span><span class="sxs-lookup"><span data-stu-id="76116-116">Download hello flight data</span></span>

1. <span data-ttu-id="76116-117">Procházet příliš[výzkum a inovativní technologie správy, úřad Transport statistických][rita-website].</span><span class="sxs-lookup"><span data-stu-id="76116-117">Browse too[Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website].</span></span>

2. <span data-ttu-id="76116-118">Na stránce hello vyberte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="76116-118">On hello page, select hello following values:</span></span>

   | <span data-ttu-id="76116-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="76116-119">Name</span></span> | <span data-ttu-id="76116-120">Hodnota</span><span class="sxs-lookup"><span data-stu-id="76116-120">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="76116-121">Filtr roku</span><span class="sxs-lookup"><span data-stu-id="76116-121">Filter Year</span></span> |<span data-ttu-id="76116-122">2013</span><span class="sxs-lookup"><span data-stu-id="76116-122">2013</span></span> |
   | <span data-ttu-id="76116-123">Filtrovat období</span><span class="sxs-lookup"><span data-stu-id="76116-123">Filter Period</span></span> |<span data-ttu-id="76116-124">Leden</span><span class="sxs-lookup"><span data-stu-id="76116-124">January</span></span> |
   | <span data-ttu-id="76116-125">Pole</span><span class="sxs-lookup"><span data-stu-id="76116-125">Fields</span></span> |<span data-ttu-id="76116-126">Rok, FlightDate, UniqueCarrier, operátora, FlightNum, OriginAirportID, původu, OriginCityName, OriginState, DestAirportID, cíle, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay NASDelay, SecurityDelay, LateAircraftDelay.</span><span class="sxs-lookup"><span data-stu-id="76116-126">Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay.</span></span> <span data-ttu-id="76116-127">Zrušte zaškrtnutí všech ostatních polí</span><span class="sxs-lookup"><span data-stu-id="76116-127">Clear all other fields</span></span> |

3. <span data-ttu-id="76116-128">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="76116-128">Click **Download**.</span></span>

## <a name="upload-hello-data"></a><span data-ttu-id="76116-129">Nahrání dat hello</span><span class="sxs-lookup"><span data-stu-id="76116-129">Upload hello data</span></span>

1. <span data-ttu-id="76116-130">Použijte následující příkaz tooupload hello zip souboru toohello hlavního uzlu clusteru HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="76116-130">Use hello following command tooupload hello zip file toohello HDInsight cluster head node:</span></span>

    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="76116-131">Nahraďte **FILENAME** hello název souboru zip hello.</span><span class="sxs-lookup"><span data-stu-id="76116-131">Replace **FILENAME** with hello name of hello zip file.</span></span> <span data-ttu-id="76116-132">Nahraďte **uživatelské jméno** s hello přihlašování přes SSH pro hello HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="76116-132">Replace **USERNAME** with hello SSH login for hello HDInsight cluster.</span></span> <span data-ttu-id="76116-133">Nahraďte název clusteru s názvem hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="76116-133">Replace CLUSTERNAME with hello name of hello HDInsight cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="76116-134">Pokud použijete heslo tooauthenticate vaše přihlášení SSH, budete vyzváni k hello heslo.</span><span class="sxs-lookup"><span data-stu-id="76116-134">If you use a password tooauthenticate your SSH login, you are prompted for hello password.</span></span> <span data-ttu-id="76116-135">Pokud jste použili veřejný klíč, může být nutné toouse hello `-i` parametr a zadejte hello cesta toohello odpovídající privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="76116-135">If you used a public key, you may need toouse hello `-i` parameter and specify hello path toohello matching private key.</span></span> <span data-ttu-id="76116-136">Například, `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="76116-136">For example, `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="76116-137">Po dokončení nahrávání hello připojte toohello clusteru pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="76116-137">Once hello upload has completed, connect toohello cluster using SSH:</span></span>

    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```

    <span data-ttu-id="76116-138">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="76116-138">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="76116-139">Po připojení, použijte následující soubor .zip hello toounzip hello:</span><span class="sxs-lookup"><span data-stu-id="76116-139">Once connected, use hello following toounzip hello .zip file:</span></span>

    ```
    unzip FILENAME.zip
    ```

    <span data-ttu-id="76116-140">Tento příkaz extrahuje soubor CSV, který je přibližně 60 MB.</span><span class="sxs-lookup"><span data-stu-id="76116-140">This command extracts a .csv file that is roughly 60 MB.</span></span>

4. <span data-ttu-id="76116-141">Použijte následující příkaz toocreate adresář na úložiště HDInsight hello a zkopírujte adresář toohello souboru hello:</span><span class="sxs-lookup"><span data-stu-id="76116-141">Use hello following command toocreate a directory on HDInsight storage, and then copy hello file toohello directory:</span></span>

    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-hello-hiveql"></a><span data-ttu-id="76116-142">Vytvoření a spuštění hello HiveQL</span><span class="sxs-lookup"><span data-stu-id="76116-142">Create and run hello HiveQL</span></span>

<span data-ttu-id="76116-143">Použití hello následující kroky tooimport data ze souboru CSV hello do tabulky Hive s názvem **zpoždění**.</span><span class="sxs-lookup"><span data-stu-id="76116-143">Use hello following steps tooimport data from hello CSV file into a Hive table named **Delays**.</span></span>

1. <span data-ttu-id="76116-144">Použití hello následující příkaz toocreate a upravit nový soubor s názvem **flightdelays.hql**:</span><span class="sxs-lookup"><span data-stu-id="76116-144">Use hello following command toocreate and edit a new file named **flightdelays.hql**:</span></span>

    ```
    nano flightdelays.hql
    ```

    <span data-ttu-id="76116-145">Použijte hello následující text jako hello obsah tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="76116-145">Use hello following text as hello contents of this file:</span></span>

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

2. <span data-ttu-id="76116-146">toosave hello soubor, použijte **kombinaci kláves Ctrl + X**, pak **Y** .</span><span class="sxs-lookup"><span data-stu-id="76116-146">toosave hello file, use **Ctrl + X**, then **Y** .</span></span>

3. <span data-ttu-id="76116-147">toostart Hive a spuštění hello **flightdelays.hql** soubor, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="76116-147">toostart Hive and run hello **flightdelays.hql** file, use hello following command:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

   > [!NOTE]
   > <span data-ttu-id="76116-148">V tomto příkladu `localhost` je použít, protože jsou připojené toohello hlavního uzlu v clusteru HDInsight hello, což je, kde je spuštěna HiveServer2.</span><span class="sxs-lookup"><span data-stu-id="76116-148">In this example, `localhost` is used since you are connected toohello head node of hello HDInsight cluster, which is where HiveServer2 is running.</span></span>

4. <span data-ttu-id="76116-149">Jednou hello __flightdelays.hql__ dokončení systémem použijte hello následující příkaz tooopen na interaktivní relace Beeline skriptu:</span><span class="sxs-lookup"><span data-stu-id="76116-149">Once hello __flightdelays.hql__ script finishes running, use hello following command tooopen an interactive Beeline session:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. <span data-ttu-id="76116-150">Po přijetí hello `jdbc:hive2://localhost:10001/>` řádku, použijte následující tooretrieve dotaz na data z hello importovat letu zpoždění dat hello.</span><span class="sxs-lookup"><span data-stu-id="76116-150">When you receive hello `jdbc:hive2://localhost:10001/>` prompt, use hello following query tooretrieve data from hello imported flight delay data.</span></span>

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    <span data-ttu-id="76116-151">Tento dotaz načte seznam města, zpoždění Doba zpoždění zkušeného počasí, společně s hello průměr a uložte ho příliš`/tutorials/flightdelays/output`.</span><span class="sxs-lookup"><span data-stu-id="76116-151">This query retrieves a list of cities that experienced weather delays, along with hello average delay time, and save it too`/tutorials/flightdelays/output`.</span></span> <span data-ttu-id="76116-152">Později Sqoop čte hello data z tohoto umístění a exportovat je tooAzure databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="76116-152">Later, Sqoop reads hello data from this location and export it tooAzure SQL Database.</span></span>

6. <span data-ttu-id="76116-153">Zadejte tooexit Beeline, `!quit` řádku hello.</span><span class="sxs-lookup"><span data-stu-id="76116-153">tooexit Beeline, enter `!quit` at hello prompt.</span></span>

## <a name="create-a-sql-database"></a><span data-ttu-id="76116-154">Vytvoření databáze SQL</span><span class="sxs-lookup"><span data-stu-id="76116-154">Create a SQL Database</span></span>

<span data-ttu-id="76116-155">Pokud už máte databázi SQL, musíte získat název serveru hello.</span><span class="sxs-lookup"><span data-stu-id="76116-155">If you already have a SQL Database, you must get hello server name.</span></span> <span data-ttu-id="76116-156">Název serveru hello můžete najít v hello [portál Azure](https://portal.azure.com) výběrem **databází SQL**, a potom na název hello hello filtrování databáze chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="76116-156">You can find hello server name in hello [Azure portal](https://portal.azure.com) by selecting **SQL Databases**, and then filtering on hello name of hello database you wish toouse.</span></span> <span data-ttu-id="76116-157">název serveru Hello je uveden v hello **SERVER** sloupce.</span><span class="sxs-lookup"><span data-stu-id="76116-157">hello server name is listed in hello **SERVER** column.</span></span>

<span data-ttu-id="76116-158">Pokud již nemáte databázi SQL, použijte informace hello v [kurz k SQL Database: vytvoření databáze SQL v minutách](../sql-database/sql-database-get-started.md) toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="76116-158">If you do not already have a SQL Database, use hello information in [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md) toocreate one.</span></span> <span data-ttu-id="76116-159">Uložte hello název serveru použít pro databázi hello.</span><span class="sxs-lookup"><span data-stu-id="76116-159">Save hello server name used for hello database.</span></span>

## <a name="create-a-sql-database-table"></a><span data-ttu-id="76116-160">Vytvořit tabulku databáze SQL</span><span class="sxs-lookup"><span data-stu-id="76116-160">Create a SQL Database table</span></span>

> [!NOTE]
> <span data-ttu-id="76116-161">Existuje mnoho způsobů tooconnect tooSQL databázi a vytvořte tabulku.</span><span class="sxs-lookup"><span data-stu-id="76116-161">There are many ways tooconnect tooSQL Database and create a table.</span></span> <span data-ttu-id="76116-162">Následující postup použijte Hello [FreeTDS](http://www.freetds.org/) z clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="76116-162">hello following steps use [FreeTDS](http://www.freetds.org/) from hello HDInsight cluster.</span></span>


1. <span data-ttu-id="76116-163">Použití clusteru HDInsight se systémem Linux toohello tooconnect SSH a spuštění hello postupem z relace SSH hello.</span><span class="sxs-lookup"><span data-stu-id="76116-163">Use SSH tooconnect toohello Linux-based HDInsight cluster, and run hello following steps from hello SSH session.</span></span>

2. <span data-ttu-id="76116-164">Použijte následující příkaz tooinstall FreeTDS hello:</span><span class="sxs-lookup"><span data-stu-id="76116-164">Use hello following command tooinstall FreeTDS:</span></span>

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. <span data-ttu-id="76116-165">Po dokončení instalace hello použijte hello následující příkaz tooconnect toohello databáze SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="76116-165">Once hello install completes, use hello following command tooconnect toohello SQL Database server.</span></span> <span data-ttu-id="76116-166">Nahraďte **serverName** s názvem serveru SQL Database hello.</span><span class="sxs-lookup"><span data-stu-id="76116-166">Replace **serverName** with hello SQL Database server name.</span></span> <span data-ttu-id="76116-167">Nahraďte **adminLogin** a **adminPassword** s hello přihlášení pro databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="76116-167">Replace **adminLogin** and **adminPassword** with hello login for SQL Database.</span></span> <span data-ttu-id="76116-168">Nahraďte **databaseName** s názvem databáze hello.</span><span class="sxs-lookup"><span data-stu-id="76116-168">Replace **databaseName** with hello database name.</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="76116-169">Zobrazí se výstup podobný toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="76116-169">You receive output similar toohello following text:</span></span>

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set toosqooptest
    1>
    ```

4. <span data-ttu-id="76116-170">V hello `1>` výzva, zadejte hello následující řádky:</span><span class="sxs-lookup"><span data-stu-id="76116-170">At hello `1>` prompt, enter hello following lines:</span></span>

    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    <span data-ttu-id="76116-171">Když hello `GO` příkaz je zadán, jsou vyhodnocovány hello předchozí příkazy.</span><span class="sxs-lookup"><span data-stu-id="76116-171">When hello `GO` statement is entered, hello previous statements are evaluated.</span></span> <span data-ttu-id="76116-172">Tento dotaz vytvoří tabulku s názvem **zpoždění**, clusterovaný index.</span><span class="sxs-lookup"><span data-stu-id="76116-172">This query creates a table named **delays**, with a clustered index.</span></span>

    <span data-ttu-id="76116-173">Použití hello následující tooverify dotazu, který hello tabulka byla vytvořena:</span><span class="sxs-lookup"><span data-stu-id="76116-173">Use hello following query tooverify that hello table has been created:</span></span>

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="76116-174">Hello výstup je podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="76116-174">hello output is similar toohello following text:</span></span>

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. <span data-ttu-id="76116-175">Zadejte `exit` v hello `1>` výzvu tooexit hello tsql nástroj.</span><span class="sxs-lookup"><span data-stu-id="76116-175">Enter `exit` at hello `1>` prompt tooexit hello tsql utility.</span></span>

## <a name="export-data-with-sqoop"></a><span data-ttu-id="76116-176">Export dat s Sqoop</span><span class="sxs-lookup"><span data-stu-id="76116-176">Export data with Sqoop</span></span>

1. <span data-ttu-id="76116-177">Použijte následující příkaz tooverify Sqoop můžete najdete v části SQL Database hello:</span><span class="sxs-lookup"><span data-stu-id="76116-177">Use hello following command tooverify that Sqoop can see your SQL Database:</span></span>

    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    <span data-ttu-id="76116-178">Tento příkaz vrátí seznam databází, včetně hello databáze, kterou jste vytvořili dříve hello zpoždění tabulky v.</span><span class="sxs-lookup"><span data-stu-id="76116-178">This command returns a list of databases, including hello database that you created hello delays table in earlier.</span></span>

2. <span data-ttu-id="76116-179">Použijte následující příkaz tooexport dat z tabulky mobiledata toohello hivesampletable hello:</span><span class="sxs-lookup"><span data-stu-id="76116-179">Use hello following command tooexport data from hivesampletable toohello mobiledata table:</span></span>

    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    <span data-ttu-id="76116-180">Sqoop připojí toohello databáze obsahující hello zpoždění tabulky a exportuje data z hello `/tutorials/flightdelays/output` tabulce zpoždění toohello adresáře.</span><span class="sxs-lookup"><span data-stu-id="76116-180">Sqoop connects toohello database containing hello delays table, and exports data from hello `/tutorials/flightdelays/output` directory toohello delays table.</span></span>

3. <span data-ttu-id="76116-181">Po dokončení příkazu hello použijte následující tooconnect toohello databázi pomocí TSQL hello:</span><span class="sxs-lookup"><span data-stu-id="76116-181">After hello command completes, use hello following tooconnect toohello database using TSQL:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="76116-182">Po připojení, použijte následující příkazy tooverify že hello data byla exportovaný toohello mobiledata tabulky hello:</span><span class="sxs-lookup"><span data-stu-id="76116-182">Once connected, use hello following statements tooverify that hello data was exported toohello mobiledata table:</span></span>

    ```
    SELECT * FROM delays
    GO
    ```

    <span data-ttu-id="76116-183">Měli byste vidět seznam data v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="76116-183">You should see a listing of data in hello table.</span></span> <span data-ttu-id="76116-184">Typ `exit` tooexit hello tsql nástroj.</span><span class="sxs-lookup"><span data-stu-id="76116-184">Type `exit` tooexit hello tsql utility.</span></span>

## <span data-ttu-id="76116-185"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="76116-185"><a id="nextsteps"></a> Next steps</span></span>

<span data-ttu-id="76116-186">najdete další způsoby toowork s daty v HDInsight, toolearn hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="76116-186">toolearn more ways toowork with data in HDInsight, see hello following documents:</span></span>

* <span data-ttu-id="76116-187">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="76116-187">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="76116-188">[Použijte Oozie s HDInsight][hdinsight-use-oozie]</span><span class="sxs-lookup"><span data-stu-id="76116-188">[Use Oozie with HDInsight][hdinsight-use-oozie]</span></span>
* <span data-ttu-id="76116-189">[Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="76116-189">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="76116-190">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="76116-190">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="76116-191">[Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="76116-191">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>
* <span data-ttu-id="76116-192">[Vývoj streamování programy pro HDInsight Hadoop Python][hdinsight-develop-streaming]</span><span class="sxs-lookup"><span data-stu-id="76116-192">[Develop Python Hadoop streaming programs for HDInsight][hdinsight-develop-streaming]</span></span>

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
