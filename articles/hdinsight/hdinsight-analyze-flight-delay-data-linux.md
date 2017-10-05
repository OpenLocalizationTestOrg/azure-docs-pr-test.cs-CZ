---
title: "Analýza dat zpoždění letu pomocí Hive v HDInsight - Azure | Microsoft Docs"
description: "Další informace o použití Hive k analýze dat letu na HDInsight se systémem Linux a potom exportovat data do databáze SQL pomocí Sqoop."
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
ms.openlocfilehash: 8cdc19ac8a517b6d8eefabb5476a686aa252a332
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a><span data-ttu-id="4edd4-103">Analýza dat zpoždění letu pomocí Hive v HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="4edd4-103">Analyze flight delay data by using Hive on Linux-based HDInsight</span></span>

<span data-ttu-id="4edd4-104">Zjistěte, jak analyzovat data zpoždění letu používání Hive v HDInsight se systémem Linux a export dat do Azure SQL Database pomocí Sqoop.</span><span class="sxs-lookup"><span data-stu-id="4edd4-104">Learn how to analyze flight delay data using Hive on Linux-based HDInsight then export the data to Azure SQL Database using Sqoop.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4edd4-105">Kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="4edd4-105">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="4edd4-106">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="4edd4-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="4edd4-107">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="4edd4-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="4edd4-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4edd4-108">Prerequisites</span></span>

* <span data-ttu-id="4edd4-109">**Cluster služby HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="4edd4-109">**An HDInsight cluster**.</span></span> <span data-ttu-id="4edd4-110">V tématu [Začínáme pomocí Hadoop Hive v HDInsight v Linuxu](hdinsight-hadoop-linux-tutorial-get-started.md) kroky k vytvoření nového clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="4edd4-110">See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md) for steps on creating a new Linux-based HDInsight cluster.</span></span>

* <span data-ttu-id="4edd4-111">**Databáze Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="4edd4-111">**Azure SQL Database**.</span></span> <span data-ttu-id="4edd4-112">Jako cílového úložiště dat používáte Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="4edd4-112">You use an Azure SQL database as a destination data store.</span></span> <span data-ttu-id="4edd4-113">Pokud již nemáte databázi SQL, najdete v části [kurz k SQL Database: vytvoření databáze SQL v minutách](../sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4edd4-113">If you do not have a SQL Database already, see [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md).</span></span>

* <span data-ttu-id="4edd4-114">**Rozhraní příkazového řádku Azure**.</span><span class="sxs-lookup"><span data-stu-id="4edd4-114">**Azure CLI**.</span></span> <span data-ttu-id="4edd4-115">Pokud jste nenainstalovali Azure CLI, najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) pro další kroky.</span><span class="sxs-lookup"><span data-stu-id="4edd4-115">If you have not installed the Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) for more steps.</span></span>

## <a name="download-the-flight-data"></a><span data-ttu-id="4edd4-116">Stáhněte si data pohybující se</span><span class="sxs-lookup"><span data-stu-id="4edd4-116">Download the flight data</span></span>

1. <span data-ttu-id="4edd4-117">Přejděte do [výzkum a inovativní technologie správy, statistický úřad Transport][rita-website].</span><span class="sxs-lookup"><span data-stu-id="4edd4-117">Browse to [Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website].</span></span>

2. <span data-ttu-id="4edd4-118">Na stránce vyberte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="4edd4-118">On the page, select the following values:</span></span>

   | <span data-ttu-id="4edd4-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="4edd4-119">Name</span></span> | <span data-ttu-id="4edd4-120">Hodnota</span><span class="sxs-lookup"><span data-stu-id="4edd4-120">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="4edd4-121">Filtr roku</span><span class="sxs-lookup"><span data-stu-id="4edd4-121">Filter Year</span></span> |<span data-ttu-id="4edd4-122">2013</span><span class="sxs-lookup"><span data-stu-id="4edd4-122">2013</span></span> |
   | <span data-ttu-id="4edd4-123">Filtrovat období</span><span class="sxs-lookup"><span data-stu-id="4edd4-123">Filter Period</span></span> |<span data-ttu-id="4edd4-124">Leden</span><span class="sxs-lookup"><span data-stu-id="4edd4-124">January</span></span> |
   | <span data-ttu-id="4edd4-125">Pole</span><span class="sxs-lookup"><span data-stu-id="4edd4-125">Fields</span></span> |<span data-ttu-id="4edd4-126">Rok, FlightDate, UniqueCarrier, operátora, FlightNum, OriginAirportID, původu, OriginCityName, OriginState, DestAirportID, cíle, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay NASDelay, SecurityDelay, LateAircraftDelay.</span><span class="sxs-lookup"><span data-stu-id="4edd4-126">Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay.</span></span> <span data-ttu-id="4edd4-127">Zrušte zaškrtnutí všech ostatních polí</span><span class="sxs-lookup"><span data-stu-id="4edd4-127">Clear all other fields</span></span> |

3. <span data-ttu-id="4edd4-128">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="4edd4-128">Click **Download**.</span></span>

## <a name="upload-the-data"></a><span data-ttu-id="4edd4-129">Odeslání dat</span><span class="sxs-lookup"><span data-stu-id="4edd4-129">Upload the data</span></span>

1. <span data-ttu-id="4edd4-130">Použijte následující příkaz k nahrání souboru zip do hlavního uzlu clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="4edd4-130">Use the following command to upload the zip file to the HDInsight cluster head node:</span></span>

    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="4edd4-131">Nahraďte **FILENAME** s názvem souboru zip.</span><span class="sxs-lookup"><span data-stu-id="4edd4-131">Replace **FILENAME** with the name of the zip file.</span></span> <span data-ttu-id="4edd4-132">Nahraďte **uživatelské jméno** s přihlašování přes SSH pro HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="4edd4-132">Replace **USERNAME** with the SSH login for the HDInsight cluster.</span></span> <span data-ttu-id="4edd4-133">Nahraďte název clusteru s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4edd4-133">Replace CLUSTERNAME with the name of the HDInsight cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4edd4-134">Pokud použijete heslo ověření přihlášení SSH, zobrazí se výzva k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="4edd4-134">If you use a password to authenticate your SSH login, you are prompted for the password.</span></span> <span data-ttu-id="4edd4-135">Pokud jste použili veřejný klíč, budete možná muset použít `-i` parametr a zadejte cestu k odpovídající soukromý klíč.</span><span class="sxs-lookup"><span data-stu-id="4edd4-135">If you used a public key, you may need to use the `-i` parameter and specify the path to the matching private key.</span></span> <span data-ttu-id="4edd4-136">Například, `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="4edd4-136">For example, `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="4edd4-137">Po dokončení nahrávání se připojte ke clusteru pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="4edd4-137">Once the upload has completed, connect to the cluster using SSH:</span></span>

    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```

    <span data-ttu-id="4edd4-138">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="4edd4-138">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="4edd4-139">Po připojení k rozbalte soubor .zip použijte následující:</span><span class="sxs-lookup"><span data-stu-id="4edd4-139">Once connected, use the following to unzip the .zip file:</span></span>

    ```
    unzip FILENAME.zip
    ```

    <span data-ttu-id="4edd4-140">Tento příkaz extrahuje soubor CSV, který je přibližně 60 MB.</span><span class="sxs-lookup"><span data-stu-id="4edd4-140">This command extracts a .csv file that is roughly 60 MB.</span></span>

4. <span data-ttu-id="4edd4-141">Použijte následující příkaz k vytvoření adresáře v úložišti HDInsight a pak zkopírujte soubor do adresáře:</span><span class="sxs-lookup"><span data-stu-id="4edd4-141">Use the following command to create a directory on HDInsight storage, and then copy the file to the directory:</span></span>

    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-the-hiveql"></a><span data-ttu-id="4edd4-142">Vytvoření a spuštění HiveQL</span><span class="sxs-lookup"><span data-stu-id="4edd4-142">Create and run the HiveQL</span></span>

<span data-ttu-id="4edd4-143">Použijte následující postup k importu dat ze souboru CSV do Hive tabulku s názvem **zpoždění**.</span><span class="sxs-lookup"><span data-stu-id="4edd4-143">Use the following steps to import data from the CSV file into a Hive table named **Delays**.</span></span>

1. <span data-ttu-id="4edd4-144">Pomocí následujícího příkazu můžete vytvářet a upravovat nový soubor s názvem **flightdelays.hql**:</span><span class="sxs-lookup"><span data-stu-id="4edd4-144">Use the following command to create and edit a new file named **flightdelays.hql**:</span></span>

    ```
    nano flightdelays.hql
    ```

    <span data-ttu-id="4edd4-145">Jako obsah tohoto souboru použijte následující text:</span><span class="sxs-lookup"><span data-stu-id="4edd4-145">Use the following text as the contents of this file:</span></span>

    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over the csv file
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
    -- The following lines describe the format and location of the file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION '/tutorials/flightdelays/data';

    -- Drop the delays table if it exists
    DROP TABLE delays;
    -- Create the delays table and populate it with data
    -- pulled in from the CSV file (via the external table defined previously)
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

2. <span data-ttu-id="4edd4-146">Chcete-li uložit soubor, použijte **kombinaci kláves Ctrl + X**, pak **Y** .</span><span class="sxs-lookup"><span data-stu-id="4edd4-146">To save the file, use **Ctrl + X**, then **Y** .</span></span>

3. <span data-ttu-id="4edd4-147">Spuštění Hive a spuštění **flightdelays.hql** souboru, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4edd4-147">To start Hive and run the **flightdelays.hql** file, use the following command:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

   > [!NOTE]
   > <span data-ttu-id="4edd4-148">V tomto příkladu `localhost` je použít, protože jsou připojené k hlavnímu uzlu clusteru HDInsight, což je, kde je spuštěna HiveServer2.</span><span class="sxs-lookup"><span data-stu-id="4edd4-148">In this example, `localhost` is used since you are connected to the head node of the HDInsight cluster, which is where HiveServer2 is running.</span></span>

4. <span data-ttu-id="4edd4-149">Jednou __flightdelays.hql__ dokončení spuštění skriptu, otevřete relaci interaktivní Beeline použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4edd4-149">Once the __flightdelays.hql__ script finishes running, use the following command to open an interactive Beeline session:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. <span data-ttu-id="4edd4-150">Když se zobrazí `jdbc:hive2://localhost:10001/>` řádku, použijte následující dotaz pro načtení dat z data pohybující se importované zpoždění.</span><span class="sxs-lookup"><span data-stu-id="4edd4-150">When you receive the `jdbc:hive2://localhost:10001/>` prompt, use the following query to retrieve data from the imported flight delay data.</span></span>

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    <span data-ttu-id="4edd4-151">Tento dotaz načte seznam města, ve kterých došlo počasí zpoždění, společně s zpoždění průměrný čas a uložte ho do `/tutorials/flightdelays/output`.</span><span class="sxs-lookup"><span data-stu-id="4edd4-151">This query retrieves a list of cities that experienced weather delays, along with the average delay time, and save it to `/tutorials/flightdelays/output`.</span></span> <span data-ttu-id="4edd4-152">Později Sqoop čte data z tohoto umístění a exportovat je do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="4edd4-152">Later, Sqoop reads the data from this location and export it to Azure SQL Database.</span></span>

6. <span data-ttu-id="4edd4-153">Chcete-li ukončit Beeline, zadejte `!quit` příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4edd4-153">To exit Beeline, enter `!quit` at the prompt.</span></span>

## <a name="create-a-sql-database"></a><span data-ttu-id="4edd4-154">Vytvoření databáze SQL</span><span class="sxs-lookup"><span data-stu-id="4edd4-154">Create a SQL Database</span></span>

<span data-ttu-id="4edd4-155">Pokud už máte databázi SQL, musíte získat název serveru.</span><span class="sxs-lookup"><span data-stu-id="4edd4-155">If you already have a SQL Database, you must get the server name.</span></span> <span data-ttu-id="4edd4-156">Můžete najít název serveru ve [portál Azure](https://portal.azure.com) výběrem **databází SQL**a pak filtrování na název databáze, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="4edd4-156">You can find the server name in the [Azure portal](https://portal.azure.com) by selecting **SQL Databases**, and then filtering on the name of the database you wish to use.</span></span> <span data-ttu-id="4edd4-157">Název serveru, je uvedena ve **SERVER** sloupce.</span><span class="sxs-lookup"><span data-stu-id="4edd4-157">The server name is listed in the **SERVER** column.</span></span>

<span data-ttu-id="4edd4-158">Pokud již nemáte databázi SQL, použijte informace v [kurz k SQL Database: vytvoření databáze SQL v minutách](../sql-database/sql-database-get-started.md) k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="4edd4-158">If you do not already have a SQL Database, use the information in [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md) to create one.</span></span> <span data-ttu-id="4edd4-159">Uložte název serveru, která je použita pro databázi.</span><span class="sxs-lookup"><span data-stu-id="4edd4-159">Save the server name used for the database.</span></span>

## <a name="create-a-sql-database-table"></a><span data-ttu-id="4edd4-160">Vytvořit tabulku databáze SQL</span><span class="sxs-lookup"><span data-stu-id="4edd4-160">Create a SQL Database table</span></span>

> [!NOTE]
> <span data-ttu-id="4edd4-161">Pro připojení k databázi SQL a vytvořte tabulku mnoha způsoby.</span><span class="sxs-lookup"><span data-stu-id="4edd4-161">There are many ways to connect to SQL Database and create a table.</span></span> <span data-ttu-id="4edd4-162">Následující postup použijte [FreeTDS](http://www.freetds.org/) z clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4edd4-162">The following steps use [FreeTDS](http://www.freetds.org/) from the HDInsight cluster.</span></span>


1. <span data-ttu-id="4edd4-163">Použití SSH se připojit ke clusteru HDInsight se systémem Linux a spustit následovně z relace SSH.</span><span class="sxs-lookup"><span data-stu-id="4edd4-163">Use SSH to connect to the Linux-based HDInsight cluster, and run the following steps from the SSH session.</span></span>

2. <span data-ttu-id="4edd4-164">Použijte následující příkaz k instalaci FreeTDS:</span><span class="sxs-lookup"><span data-stu-id="4edd4-164">Use the following command to install FreeTDS:</span></span>

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. <span data-ttu-id="4edd4-165">Po dokončení instalace použijte následující příkaz pro připojení k databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="4edd4-165">Once the install completes, use the following command to connect to the SQL Database server.</span></span> <span data-ttu-id="4edd4-166">Nahraďte **serverName** s názvem serveru SQL Database.</span><span class="sxs-lookup"><span data-stu-id="4edd4-166">Replace **serverName** with the SQL Database server name.</span></span> <span data-ttu-id="4edd4-167">Nahraďte **adminLogin** a **adminPassword** s přihlášení pro databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="4edd4-167">Replace **adminLogin** and **adminPassword** with the login for SQL Database.</span></span> <span data-ttu-id="4edd4-168">Nahraďte **databaseName** s názvem databáze.</span><span class="sxs-lookup"><span data-stu-id="4edd4-168">Replace **databaseName** with the database name.</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="4edd4-169">Zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="4edd4-169">You receive output similar to the following text:</span></span>

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to sqooptest
    1>
    ```

4. <span data-ttu-id="4edd4-170">Na `1>` výzva, zadejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="4edd4-170">At the `1>` prompt, enter the following lines:</span></span>

    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    <span data-ttu-id="4edd4-171">Když `GO` příkaz je zadán, jsou vyhodnocovány předchozí příkazy.</span><span class="sxs-lookup"><span data-stu-id="4edd4-171">When the `GO` statement is entered, the previous statements are evaluated.</span></span> <span data-ttu-id="4edd4-172">Tento dotaz vytvoří tabulku s názvem **zpoždění**, clusterovaný index.</span><span class="sxs-lookup"><span data-stu-id="4edd4-172">This query creates a table named **delays**, with a clustered index.</span></span>

    <span data-ttu-id="4edd4-173">Chcete-li ověřit, zda byl vytvořen v tabulce použijte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="4edd4-173">Use the following query to verify that the table has been created:</span></span>

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="4edd4-174">Výstup se bude podobat následujícímu:</span><span class="sxs-lookup"><span data-stu-id="4edd4-174">The output is similar to the following text:</span></span>

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. <span data-ttu-id="4edd4-175">Zadejte `exit` na `1>` výzvy ukončete nástroj tsql.</span><span class="sxs-lookup"><span data-stu-id="4edd4-175">Enter `exit` at the `1>` prompt to exit the tsql utility.</span></span>

## <a name="export-data-with-sqoop"></a><span data-ttu-id="4edd4-176">Export dat s Sqoop</span><span class="sxs-lookup"><span data-stu-id="4edd4-176">Export data with Sqoop</span></span>

1. <span data-ttu-id="4edd4-177">Pomocí následujícího příkazu ověřte, že Sqoop vidí vaší databázi SQL:</span><span class="sxs-lookup"><span data-stu-id="4edd4-177">Use the following command to verify that Sqoop can see your SQL Database:</span></span>

    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    <span data-ttu-id="4edd4-178">Tento příkaz vrátí seznam databází, včetně databáze, kterou jste vytvořili v tabulce zpoždění v dříve.</span><span class="sxs-lookup"><span data-stu-id="4edd4-178">This command returns a list of databases, including the database that you created the delays table in earlier.</span></span>

2. <span data-ttu-id="4edd4-179">Exportovat data z hivesampletable do tabulky mobiledata použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4edd4-179">Use the following command to export data from hivesampletable to the mobiledata table:</span></span>

    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    <span data-ttu-id="4edd4-180">Sqoop připojuje k databázi obsahující tabulce zpoždění a exportuje data z `/tutorials/flightdelays/output` adresář do tabulky zpoždění.</span><span class="sxs-lookup"><span data-stu-id="4edd4-180">Sqoop connects to the database containing the delays table, and exports data from the `/tutorials/flightdelays/output` directory to the delays table.</span></span>

3. <span data-ttu-id="4edd4-181">Po dokončení příkazu, použijte následující postupy pro připojení k databázi pomocí TSQL:</span><span class="sxs-lookup"><span data-stu-id="4edd4-181">After the command completes, use the following to connect to the database using TSQL:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="4edd4-182">Po připojení, ověřte, že se data vyexportovala do tabulky mobiledata pomocí následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="4edd4-182">Once connected, use the following statements to verify that the data was exported to the mobiledata table:</span></span>

    ```
    SELECT * FROM delays
    GO
    ```

    <span data-ttu-id="4edd4-183">Měli byste vidět seznam dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4edd4-183">You should see a listing of data in the table.</span></span> <span data-ttu-id="4edd4-184">Typ `exit` ukončete nástroj tsql.</span><span class="sxs-lookup"><span data-stu-id="4edd4-184">Type `exit` to exit the tsql utility.</span></span>

## <span data-ttu-id="4edd4-185"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="4edd4-185"><a id="nextsteps"></a> Next steps</span></span>

<span data-ttu-id="4edd4-186">Informace o další způsoby, jak pracovat s daty v HDInsight, najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="4edd4-186">To learn more ways to work with data in HDInsight, see the following documents:</span></span>

* <span data-ttu-id="4edd4-187">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="4edd4-187">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="4edd4-188">[Použijte Oozie s HDInsight][hdinsight-use-oozie]</span><span class="sxs-lookup"><span data-stu-id="4edd4-188">[Use Oozie with HDInsight][hdinsight-use-oozie]</span></span>
* <span data-ttu-id="4edd4-189">[Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="4edd4-189">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="4edd4-190">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="4edd4-190">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="4edd4-191">[Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="4edd4-191">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>
* <span data-ttu-id="4edd4-192">[Vývoj streamování programy pro HDInsight Hadoop Python][hdinsight-develop-streaming]</span><span class="sxs-lookup"><span data-stu-id="4edd4-192">[Develop Python Hadoop streaming programs for HDInsight][hdinsight-develop-streaming]</span></span>

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
