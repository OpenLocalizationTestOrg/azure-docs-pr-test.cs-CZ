---
title: "Použití Beeline s Apache Hive - Azure HDInsight | Microsoft Docs"
description: "Další informace o použití Beeline klienta ke spouštění dotazů Hive se systémem Hadoop v HDInsight. Beeline je nástroj pro práci s HiveServer2 prostřednictvím JDBC."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: beeline hive, hive beeline
ms.assetid: 3adfb1ba-8924-4a13-98db-10a67ab24fca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 153044aafa3a67ee85bb1997beb821777c938563
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-beeline-client-with-apache-hive"></a><span data-ttu-id="91be2-105">Pomocí klienta Beeline s Apache Hive</span><span class="sxs-lookup"><span data-stu-id="91be2-105">Use the Beeline client with Apache Hive</span></span>

<span data-ttu-id="91be2-106">Další informace o použití [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) ke spouštění dotazů Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="91be2-106">Learn how to use [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) to run Hive queries on HDInsight.</span></span>

<span data-ttu-id="91be2-107">Beeline je klient Hive, který je zahrnutý o hlavních uzlech clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="91be2-107">Beeline is a Hive client that is included on the head nodes of your HDInsight cluster.</span></span> <span data-ttu-id="91be2-108">Beeline JDBC používá pro připojení k HiveServer2, službě hostované na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="91be2-108">Beeline uses JDBC to connect to HiveServer2, a service hosted on your HDInsight cluster.</span></span> <span data-ttu-id="91be2-109">Můžete taky Beeline pro přístup k Hive v HDInsight vzdáleně přes internet.</span><span class="sxs-lookup"><span data-stu-id="91be2-109">You can also use Beeline to access Hive on HDInsight remotely over the internet.</span></span> <span data-ttu-id="91be2-110">Následující tabulka obsahuje připojovací řetězce pro použití s Beeline:</span><span class="sxs-lookup"><span data-stu-id="91be2-110">The following table provides connection strings for use with Beeline:</span></span>

| <span data-ttu-id="91be2-111">Kde spouštíte Beeline z</span><span class="sxs-lookup"><span data-stu-id="91be2-111">Where you run Beeline from</span></span> | <span data-ttu-id="91be2-112">Parametry</span><span class="sxs-lookup"><span data-stu-id="91be2-112">Parameters</span></span> |
| --- | --- | --- |
| <span data-ttu-id="91be2-113">Připojení SSH do uzlu headnode nebo Microsoft edge</span><span class="sxs-lookup"><span data-stu-id="91be2-113">An SSH connection to a headnode or edge node</span></span> | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
| <span data-ttu-id="91be2-114">Mimo cluster</span><span class="sxs-lookup"><span data-stu-id="91be2-114">Outside the cluster</span></span> | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

> [!NOTE]
> <span data-ttu-id="91be2-115">Nahraďte `admin` s účtem přihlášení clusteru pro cluster.</span><span class="sxs-lookup"><span data-stu-id="91be2-115">Replace `admin` with the cluster login account for your cluster.</span></span>
>
> <span data-ttu-id="91be2-116">Nahraďte `password` pomocí hesla pro účet přihlášení clusteru.</span><span class="sxs-lookup"><span data-stu-id="91be2-116">Replace `password` with the password for the cluster login account.</span></span>
>
> <span data-ttu-id="91be2-117">Parametr `clustername` nahraďte názvem vašeho clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="91be2-117">Replace `clustername` with the name of your HDInsight cluster.</span></span>

## <span data-ttu-id="91be2-118"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="91be2-118"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="91be2-119">Systémem Linux Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="91be2-119">A Linux-based Hadoop on HDInsight cluster.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="91be2-120">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="91be2-120">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="91be2-121">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="91be2-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="91be2-122">Klientem SSH nebo místní Beeline klienta.</span><span class="sxs-lookup"><span data-stu-id="91be2-122">An SSH client or a local Beeline client.</span></span> <span data-ttu-id="91be2-123">Většina kroky v tomto dokumentu předpokládají, že používáte Beeline z relace SSH do clusteru.</span><span class="sxs-lookup"><span data-stu-id="91be2-123">Most of the steps in this document assume that you are using Beeline from an SSH session to the cluster.</span></span> <span data-ttu-id="91be2-124">Informace o spouštění Beeline z mimo cluster najdete v tématu [vzdáleně pomocí Beeline](#remote) části.</span><span class="sxs-lookup"><span data-stu-id="91be2-124">For information on running Beeline from outside the cluster, see the [use Beeline remotely](#remote) section.</span></span>

    <span data-ttu-id="91be2-125">Další informace o používání SSH najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="91be2-125">For more information on using SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="91be2-126"><a id="beeline"></a>Použití Beeline</span><span class="sxs-lookup"><span data-stu-id="91be2-126"><a id="beeline"></a>Use Beeline</span></span>

1. <span data-ttu-id="91be2-127">Při spouštění Beeline, musí zadat připojovací řetězec pro HiveServer2 v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="91be2-127">When starting Beeline, you must provide a connection string for HiveServer2 on your HDInsight cluster.</span></span> <span data-ttu-id="91be2-128">Ke spuštění příkazu z mimo cluster, je nutné také zadat název clusteru přihlašovací účet (výchozí `admin`) a heslo.</span><span class="sxs-lookup"><span data-stu-id="91be2-128">To run the command from outside the cluster, you must also provide the cluster login account name (default `admin`) and password.</span></span> <span data-ttu-id="91be2-129">Použijte formátu řetězce připojení a parametry, které použijí najdete v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="91be2-129">Use the following table to find the connection string format and parameters to use:</span></span>

    | <span data-ttu-id="91be2-130">Kde spouštíte Beeline z</span><span class="sxs-lookup"><span data-stu-id="91be2-130">Where you run Beeline from</span></span> | <span data-ttu-id="91be2-131">Parametry</span><span class="sxs-lookup"><span data-stu-id="91be2-131">Parameters</span></span> |
    | --- | --- | --- |
    | <span data-ttu-id="91be2-132">Připojení SSH do uzlu headnode nebo Microsoft edge</span><span class="sxs-lookup"><span data-stu-id="91be2-132">An SSH connection to a headnode or edge node</span></span> | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
    | <span data-ttu-id="91be2-133">Mimo cluster</span><span class="sxs-lookup"><span data-stu-id="91be2-133">Outside the cluster</span></span> | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

    <span data-ttu-id="91be2-134">Například následující příkaz lze použít ke spuštění Beeline z relace SSH do clusteru:</span><span class="sxs-lookup"><span data-stu-id="91be2-134">For example, the following command can be used to start Beeline from an SSH session to the cluster:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    <span data-ttu-id="91be2-135">Tento příkaz spustí Beeline klienta a připojí k systému HiveServer2 v hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="91be2-135">This command starts the Beeline client, and connects to HiveServer2 on the cluster head node.</span></span> <span data-ttu-id="91be2-136">Po dokončení příkazu přijedete do `jdbc:hive2://headnodehost:10001/>` řádku.</span><span class="sxs-lookup"><span data-stu-id="91be2-136">Once the command completes, you arrive at a `jdbc:hive2://headnodehost:10001/>` prompt.</span></span>

2. <span data-ttu-id="91be2-137">Příkazy beeline začínat `!` znak, třeba `!help` zobrazí nápovědu.</span><span class="sxs-lookup"><span data-stu-id="91be2-137">Beeline commands begin with a `!` character, for example `!help` displays help.</span></span> <span data-ttu-id="91be2-138">Ale `!` lze vynechat pro některé příkazy.</span><span class="sxs-lookup"><span data-stu-id="91be2-138">However the `!` can be omitted for some commands.</span></span> <span data-ttu-id="91be2-139">Například `help` funguje taky.</span><span class="sxs-lookup"><span data-stu-id="91be2-139">For example, `help` also works.</span></span>

    <span data-ttu-id="91be2-140">Došlo `!sql`, který se používá k provedení příkazy HiveQL.</span><span class="sxs-lookup"><span data-stu-id="91be2-140">There is a `!sql`, which is used to execute HiveQL statements.</span></span> <span data-ttu-id="91be2-141">HiveQL se ale tak běžně používá, můžete vynechat předchozím `!sql`.</span><span class="sxs-lookup"><span data-stu-id="91be2-141">However, HiveQL is so commonly used that you can omit the preceding `!sql`.</span></span> <span data-ttu-id="91be2-142">Odpovídají následující dva příkazy:</span><span class="sxs-lookup"><span data-stu-id="91be2-142">The following two statements are equivalent:</span></span>

    ```hiveql
    !sql show tables;
    show tables;
    ```

    <span data-ttu-id="91be2-143">Na novém clusteru, je uvedena pouze jednu tabulku: **hivesampletable**.</span><span class="sxs-lookup"><span data-stu-id="91be2-143">On a new cluster, only one table is listed: **hivesampletable**.</span></span>

3. <span data-ttu-id="91be2-144">Pomocí následujícího příkazu můžete zobrazit schéma pro hivesampletable:</span><span class="sxs-lookup"><span data-stu-id="91be2-144">Use the following command to display the schema for the hivesampletable:</span></span>

    ```hiveql
    describe hivesampletable;
    ```

    <span data-ttu-id="91be2-145">Tento příkaz vrátí následující informace:</span><span class="sxs-lookup"><span data-stu-id="91be2-145">This command returns the following information:</span></span>

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    <span data-ttu-id="91be2-146">Tyto informace popisují sloupců v tabulce.</span><span class="sxs-lookup"><span data-stu-id="91be2-146">This information describes the columns in the table.</span></span> <span data-ttu-id="91be2-147">Když jsme může provádět některé dotazy pro tato data, místo toho vytvoříme zbrusu novou tabulku pro ukazují, jak načíst data do Hive a použijte schéma.</span><span class="sxs-lookup"><span data-stu-id="91be2-147">While we could perform some queries against this data, let's instead create a brand new table to demonstrate how to load data into Hive and apply a schema.</span></span>

4. <span data-ttu-id="91be2-148">Zadejte následující příkazy a vytvořte tabulku s názvem **log4jLogs** pomocí ukázkových dat, které jsou součástí clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="91be2-148">Enter the following statements to create a table named **log4jLogs** by using sample data provided with the HDInsight cluster:</span></span>

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    <span data-ttu-id="91be2-149">Tyto příkazy provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="91be2-149">These statements perform the following actions:</span></span>

    * <span data-ttu-id="91be2-150">`DROP TABLE`– Pokud je v tabulce existuje, je odstraněn.</span><span class="sxs-lookup"><span data-stu-id="91be2-150">`DROP TABLE` - If the table exists, it is deleted.</span></span>

    * <span data-ttu-id="91be2-151">`CREATE EXTERNAL TABLE`-Vytvoří **externí** tabulky v Hive.</span><span class="sxs-lookup"><span data-stu-id="91be2-151">`CREATE EXTERNAL TABLE` - Creates an **external** table in Hive.</span></span> <span data-ttu-id="91be2-152">Externí tabulky pouze uložit definici tabulky Hive.</span><span class="sxs-lookup"><span data-stu-id="91be2-152">External tables only store the table definition in Hive.</span></span> <span data-ttu-id="91be2-153">Data je ponechán v původním umístění.</span><span class="sxs-lookup"><span data-stu-id="91be2-153">The data is left in the original location.</span></span>

    * <span data-ttu-id="91be2-154">`ROW FORMAT`-Způsob formátování data.</span><span class="sxs-lookup"><span data-stu-id="91be2-154">`ROW FORMAT` - How the data is formatted.</span></span> <span data-ttu-id="91be2-155">V takovém případě polí v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="91be2-155">In this case, the fields in each log are separated by a space.</span></span>

    * <span data-ttu-id="91be2-156">`STORED AS TEXTFILE LOCATION`-Tam, kde jsou data uložena a jaký formát souboru.</span><span class="sxs-lookup"><span data-stu-id="91be2-156">`STORED AS TEXTFILE LOCATION` - Where the data is stored and in what file format.</span></span>

    * <span data-ttu-id="91be2-157">`SELECT`– Počet všech řádků vybere kde sloupec **t4** obsahuje hodnotu **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="91be2-157">`SELECT` - Selects a count of all rows where column **t4** contains the value **[ERROR]**.</span></span> <span data-ttu-id="91be2-158">Tento dotaz vrací hodnotu **3** jsou tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="91be2-158">This query returns a value of **3** as there are three rows that contain this value.</span></span>

    * <span data-ttu-id="91be2-159">`INPUT__FILE__NAME LIKE '%.log'`-Hive se pokusí použít schéma pro všechny soubory v adresáři.</span><span class="sxs-lookup"><span data-stu-id="91be2-159">`INPUT__FILE__NAME LIKE '%.log'` - Hive attempts to apply the schema to all files in the directory.</span></span> <span data-ttu-id="91be2-160">V takovém případě adresáře obsahuje soubory, které neodpovídají žádné schéma.</span><span class="sxs-lookup"><span data-stu-id="91be2-160">In this case, the directory contains files that do not match the schema.</span></span> <span data-ttu-id="91be2-161">Aby paměti data ve výsledcích tento příkaz informuje Hive, jsme by měl vrátit pouze data ze souborů končící na. log.</span><span class="sxs-lookup"><span data-stu-id="91be2-161">To prevent garbage data in the results, this statement tells Hive that we should only return data from files ending in .log.</span></span>

  > [!NOTE]
  > <span data-ttu-id="91be2-162">Externí tabulky by měl být použit při očekáváte, že v základních datech aktualizovat externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="91be2-162">External tables should be used when you expect the underlying data to be updated by an external source.</span></span> <span data-ttu-id="91be2-163">Například procesu nahrávání automatizované dat nebo operace MapReduce.</span><span class="sxs-lookup"><span data-stu-id="91be2-163">For example, an automated data upload process or a MapReduce operation.</span></span>
  >
  > <span data-ttu-id="91be2-164">Vyřazení externí tabulku nemá **není** odstranit data, jenom definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="91be2-164">Dropping an external table does **not** delete the data, only the table definition.</span></span>

    <span data-ttu-id="91be2-165">Výstup tohoto příkazu je podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="91be2-165">The output of this command is similar to the following text:</span></span>

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

5. <span data-ttu-id="91be2-166">Chcete-li ukončit Beeline, použijte `!exit`.</span><span class="sxs-lookup"><span data-stu-id="91be2-166">To exit Beeline, use `!exit`.</span></span>

## <span data-ttu-id="91be2-167"><a id="file"></a>Spusťte soubor HiveQL pomocí Beeline</span><span class="sxs-lookup"><span data-stu-id="91be2-167"><a id="file"></a>Use Beeline to run a HiveQL file</span></span>

<span data-ttu-id="91be2-168">Použijte následující postup k vytvoření souboru a potom spustit pomocí Beeline.</span><span class="sxs-lookup"><span data-stu-id="91be2-168">Use the following steps to create a file, then run it using Beeline.</span></span>

1. <span data-ttu-id="91be2-169">Pomocí následujícího příkazu vytvořte soubor s názvem **query.hql**:</span><span class="sxs-lookup"><span data-stu-id="91be2-169">Use the following command to create a file named **query.hql**:</span></span>

    ```bash
    nano query.hql
    ```

2. <span data-ttu-id="91be2-170">Použijte následující text jako obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="91be2-170">Use the following text as the contents of the file.</span></span> <span data-ttu-id="91be2-171">Tento dotaz vytvoří novou tabulku "interní" s názvem **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="91be2-171">This query creates a new 'internal' table named **errorLogs**:</span></span>

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    <span data-ttu-id="91be2-172">Tyto příkazy provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="91be2-172">These statements perform the following actions:</span></span>

    * <span data-ttu-id="91be2-173">**Vytvoření tabulka není v případě existuje** – Pokud tabulky již neexistuje, vytvoří se.</span><span class="sxs-lookup"><span data-stu-id="91be2-173">**CREATE TABLE IF NOT EXISTS** - If the table does not already exist, it is created.</span></span> <span data-ttu-id="91be2-174">Vzhledem k tomu **externí** – klíčové slovo není, tento příkaz vytvoří interní tabulku.</span><span class="sxs-lookup"><span data-stu-id="91be2-174">Since the **EXTERNAL** keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="91be2-175">Interní tabulky jsou uložena v datovém skladu Hive a jsou zcela spravovány Hive.</span><span class="sxs-lookup"><span data-stu-id="91be2-175">Internal tables are stored in the Hive data warehouse and are managed completely by Hive.</span></span>
    * <span data-ttu-id="91be2-176">**ULOŽENÉ ORC AS** – ukládá data ve formátu optimalizované řádek sloupcovém (ORC).</span><span class="sxs-lookup"><span data-stu-id="91be2-176">**STORED AS ORC** - Stores the data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="91be2-177">Formát ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.</span><span class="sxs-lookup"><span data-stu-id="91be2-177">ORC format is a highly optimized and efficient format for storing Hive data.</span></span>
    * <span data-ttu-id="91be2-178">**PŘEPSAT INSERT... Vyberte** -vybere řádky z **log4jLogs** tabulku, která obsahují **[Chyba]**, pak vkládá data do **errorLogs** tabulky.</span><span class="sxs-lookup"><span data-stu-id="91be2-178">**INSERT OVERWRITE ... SELECT** - Selects rows from the **log4jLogs** table that contain **[ERROR]**, then inserts the data into the **errorLogs** table.</span></span>

    > [!NOTE]
    > <span data-ttu-id="91be2-179">Na rozdíl od externích tabulek se odstraní vyřazení interní tabulku v základních datech.</span><span class="sxs-lookup"><span data-stu-id="91be2-179">Unlike external tables, dropping an internal table deletes the underlying data as well.</span></span>

3. <span data-ttu-id="91be2-180">Chcete-li uložit soubor, použijte **Ctrl**+**_X**, pak zadejte **Y**a v neposlední řadě **Enter**.</span><span class="sxs-lookup"><span data-stu-id="91be2-180">To save the file, use **Ctrl**+**_X**, then enter **Y**, and finally **Enter**.</span></span>

4. <span data-ttu-id="91be2-181">Ke spuštění souboru pomocí Beeline použijte následující:</span><span class="sxs-lookup"><span data-stu-id="91be2-181">Use the following to run the file using Beeline:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]
    > <span data-ttu-id="91be2-182">`-i` Parametr spustí Beeline, spouští příkazy v souboru query.hql.</span><span class="sxs-lookup"><span data-stu-id="91be2-182">The `-i` parameter starts Beeline, runs the statements in the query.hql file.</span></span> <span data-ttu-id="91be2-183">Po dokončení dotazu, které přicházejí na `jdbc:hive2://headnodehost:10001/>` řádku.</span><span class="sxs-lookup"><span data-stu-id="91be2-183">Once the query completes, you arrive at the `jdbc:hive2://headnodehost:10001/>` prompt.</span></span> <span data-ttu-id="91be2-184">Můžete taky spustit soubor pomocí `-f` parametr, který ukončí Beeline po dokončení dotazu.</span><span class="sxs-lookup"><span data-stu-id="91be2-184">You can also run a file using the `-f` parameter, which exits Beeline after the query completes.</span></span>

5. <span data-ttu-id="91be2-185">Ověřit, jestli **errorLogs** tabulka byla vytvořena, použijte následující příkaz a vrátí všechny řádky z **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="91be2-185">To verify that the **errorLogs** table was created, use the following statement to return all the rows from **errorLogs**:</span></span>

    ```hiveql
    SELECT * from errorLogs;
    ```

    <span data-ttu-id="91be2-186">Tří řádků dat má být vrácen, všechny obsahující **[Chyba]** v t4 sloupce:</span><span class="sxs-lookup"><span data-stu-id="91be2-186">Three rows of data should be returned, all containing **[ERROR]** in column t4:</span></span>

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <span data-ttu-id="91be2-187"><a id="remote"></a>Použít Beeline vzdáleně</span><span class="sxs-lookup"><span data-stu-id="91be2-187"><a id="remote"></a>Use Beeline remotely</span></span>

<span data-ttu-id="91be2-188">Pokud máte Beeline nainstalovány místně, nebo ji používají prostřednictvím Docker image, jako [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), je nutné použít následující parametry:</span><span class="sxs-lookup"><span data-stu-id="91be2-188">If you have Beeline installed locally, or are using it through a Docker image such as [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), you must use the following parameters:</span></span>

* <span data-ttu-id="91be2-189">__Připojovací řetězec__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`</span><span class="sxs-lookup"><span data-stu-id="91be2-189">__Connection string__: `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`</span></span>

* <span data-ttu-id="91be2-190">__Přihlašovací jméno clusteru__:`-n admin`</span><span class="sxs-lookup"><span data-stu-id="91be2-190">__Cluster login name__: `-n admin`</span></span>

* <span data-ttu-id="91be2-191">__Heslo pro přihlášení clusteru__`-p 'password'`</span><span class="sxs-lookup"><span data-stu-id="91be2-191">__Cluster login password__ `-p 'password'`</span></span>

<span data-ttu-id="91be2-192">Nahraďte `clustername` v připojovacím řetězci s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="91be2-192">Replace the `clustername` in the connection string with the name of your HDInsight cluster.</span></span>

<span data-ttu-id="91be2-193">Nahraďte `admin` s názvem vašeho clusteru přihlášení a nahradit `password` heslem pro váš cluster přihlášení.</span><span class="sxs-lookup"><span data-stu-id="91be2-193">Replace `admin` with the name of your cluster login, and replace `password` with the password for your cluster login.</span></span>

## <span data-ttu-id="91be2-194"><a id="sparksql"></a>Beeline pomocí Spark</span><span class="sxs-lookup"><span data-stu-id="91be2-194"><a id="sparksql"></a>Use Beeline with Spark</span></span>

<span data-ttu-id="91be2-195">Spark poskytuje svou vlastní implementaci systému HiveServer2, což je často jen serveru Spark Thrift.</span><span class="sxs-lookup"><span data-stu-id="91be2-195">Spark provides its own implementation of HiveServer2, which is often refered to as the Spark Thrift server.</span></span> <span data-ttu-id="91be2-196">Tato služba používá Spark SQL při překladu místo Hive a může poskytovat lepší výkon v závislosti na svůj dotaz.</span><span class="sxs-lookup"><span data-stu-id="91be2-196">This service uses Spark SQL to resolve queries instead of Hive, and may provide better performance depending on your query.</span></span>

<span data-ttu-id="91be2-197">Pro připojení k serveru Spark Thrift Spark na clusteru HDInsight, použijte port `10002` místo `10001`.</span><span class="sxs-lookup"><span data-stu-id="91be2-197">To connect to the Spark Thrift server of a Spark on HDInsight cluster, use port `10002` instead of `10001`.</span></span> <span data-ttu-id="91be2-198">Například, `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.</span><span class="sxs-lookup"><span data-stu-id="91be2-198">For example, `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="91be2-199">Serveru Spark Thrift není přímo přístupné přes internet.</span><span class="sxs-lookup"><span data-stu-id="91be2-199">The Spark Thrift server is not directly accessible over the internet.</span></span> <span data-ttu-id="91be2-200">Můžete pouze připojit k němu z relace SSH nebo ve stejné virtuální síti Azure jako HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="91be2-200">You can only connect to it from an SSH session or inside the same Azure Virtual Network as the HDInsight cluster.</span></span>

## <span data-ttu-id="91be2-201"><a id="summary"></a><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="91be2-201"><a id="summary"></a><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="91be2-202">Další obecné informace o Hive v HDInsight najdete v následujícím dokumentu:</span><span class="sxs-lookup"><span data-stu-id="91be2-202">For more general information on Hive in HDInsight, see the following document:</span></span>

* [<span data-ttu-id="91be2-203">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="91be2-203">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="91be2-204">Další informace o jiných způsobech, že pracujete s Hadoop v HDInsight najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="91be2-204">For more information on other ways you can work with Hadoop on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="91be2-205">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="91be2-205">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="91be2-206">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="91be2-206">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="91be2-207">Pokud používáte s Hive Tez, najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="91be2-207">If you are using Tez with Hive, see the following documents:</span></span>

* [<span data-ttu-id="91be2-208">Pomocí uživatelského rozhraní Tez na HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="91be2-208">Use the Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="91be2-209">Použití zobrazení Ambari Tez na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="91be2-209">Use the Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
