---
title: aaaUse Beeline s Apache Hive - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toouse hello Beeline klienta toorun Hive dotazy s Hadoop v HDInsight. Beeline je nástroj pro práci s HiveServer2 prostřednictvím JDBC."
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
ms.openlocfilehash: e788ff39f33d928808cfcb83a92f62ac9ae8ca09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-beeline-client-with-apache-hive"></a><span data-ttu-id="1e2b0-105">Použijte hello Beeline klienta s Apache Hive</span><span class="sxs-lookup"><span data-stu-id="1e2b0-105">Use hello Beeline client with Apache Hive</span></span>

<span data-ttu-id="1e2b0-106">Zjistěte, jak toouse [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) dotazuje toorun Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-106">Learn how toouse [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) toorun Hive queries on HDInsight.</span></span>

<span data-ttu-id="1e2b0-107">Beeline je klient Hive, který je zahrnutý v hello hlavních uzlech clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-107">Beeline is a Hive client that is included on hello head nodes of your HDInsight cluster.</span></span> <span data-ttu-id="1e2b0-108">Beeline používá JDBC tooconnect tooHiveServer2, službě hostované na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-108">Beeline uses JDBC tooconnect tooHiveServer2, a service hosted on your HDInsight cluster.</span></span> <span data-ttu-id="1e2b0-109">Můžete také použít Beeline tooaccess Hive v HDInsight vzdáleně přes hello internet.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-109">You can also use Beeline tooaccess Hive on HDInsight remotely over hello internet.</span></span> <span data-ttu-id="1e2b0-110">Hello následující tabulka obsahuje připojovací řetězce pro použití s Beeline:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-110">hello following table provides connection strings for use with Beeline:</span></span>

| <span data-ttu-id="1e2b0-111">Kde spouštíte Beeline z</span><span class="sxs-lookup"><span data-stu-id="1e2b0-111">Where you run Beeline from</span></span> | <span data-ttu-id="1e2b0-112">Parametry</span><span class="sxs-lookup"><span data-stu-id="1e2b0-112">Parameters</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e2b0-113">Do SSH připojení tooa headnode nebo Microsoft edge uzlu</span><span class="sxs-lookup"><span data-stu-id="1e2b0-113">An SSH connection tooa headnode or edge node</span></span> | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
| <span data-ttu-id="1e2b0-114">Mimo cluster hello</span><span class="sxs-lookup"><span data-stu-id="1e2b0-114">Outside hello cluster</span></span> | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

> [!NOTE]
> <span data-ttu-id="1e2b0-115">Nahraďte `admin` s hello clusteru přihlašovací účet pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-115">Replace `admin` with hello cluster login account for your cluster.</span></span>
>
> <span data-ttu-id="1e2b0-116">Nahraďte `password` s hello heslo pro účet přihlášení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-116">Replace `password` with hello password for hello cluster login account.</span></span>
>
> <span data-ttu-id="1e2b0-117">Nahraďte `clustername` s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-117">Replace `clustername` with hello name of your HDInsight cluster.</span></span>

## <span data-ttu-id="1e2b0-118"><a id="prereq"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="1e2b0-118"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="1e2b0-119">Systémem Linux Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-119">A Linux-based Hadoop on HDInsight cluster.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="1e2b0-120">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-120">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="1e2b0-121">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="1e2b0-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="1e2b0-122">Klientem SSH nebo místní Beeline klienta.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-122">An SSH client or a local Beeline client.</span></span> <span data-ttu-id="1e2b0-123">Většina hello kroky v tomto dokumentu předpokládají, že používáte Beeline z clusteru toohello relace SSH.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-123">Most of hello steps in this document assume that you are using Beeline from an SSH session toohello cluster.</span></span> <span data-ttu-id="1e2b0-124">Informace o spouštění Beeline z mimo hello clusteru najdete v tématu hello [vzdáleně pomocí Beeline](#remote) části.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-124">For information on running Beeline from outside hello cluster, see hello [use Beeline remotely](#remote) section.</span></span>

    <span data-ttu-id="1e2b0-125">Další informace o používání SSH najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="1e2b0-125">For more information on using SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="1e2b0-126"><a id="beeline"></a>Použití Beeline</span><span class="sxs-lookup"><span data-stu-id="1e2b0-126"><a id="beeline"></a>Use Beeline</span></span>

1. <span data-ttu-id="1e2b0-127">Při spouštění Beeline, musí zadat připojovací řetězec pro HiveServer2 v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-127">When starting Beeline, you must provide a connection string for HiveServer2 on your HDInsight cluster.</span></span> <span data-ttu-id="1e2b0-128">příkaz hello toorun z mimo hello clusteru, musíte taky zadat název účtu přihlášení clusteru hello (výchozí `admin`) a heslo.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-128">toorun hello command from outside hello cluster, you must also provide hello cluster login account name (default `admin`) and password.</span></span> <span data-ttu-id="1e2b0-129">Použijte následující tabulku toofind hello připojovací řetězec formátu a parametry toouse hello:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-129">Use hello following table toofind hello connection string format and parameters toouse:</span></span>

    | <span data-ttu-id="1e2b0-130">Kde spouštíte Beeline z</span><span class="sxs-lookup"><span data-stu-id="1e2b0-130">Where you run Beeline from</span></span> | <span data-ttu-id="1e2b0-131">Parametry</span><span class="sxs-lookup"><span data-stu-id="1e2b0-131">Parameters</span></span> |
    | --- | --- | --- |
    | <span data-ttu-id="1e2b0-132">Do SSH připojení tooa headnode nebo Microsoft edge uzlu</span><span class="sxs-lookup"><span data-stu-id="1e2b0-132">An SSH connection tooa headnode or edge node</span></span> | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
    | <span data-ttu-id="1e2b0-133">Mimo cluster hello</span><span class="sxs-lookup"><span data-stu-id="1e2b0-133">Outside hello cluster</span></span> | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

    <span data-ttu-id="1e2b0-134">Například hello následující příkaz lze použít toostart Beeline z clusteru toohello relace SSH:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-134">For example, hello following command can be used toostart Beeline from an SSH session toohello cluster:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    <span data-ttu-id="1e2b0-135">Tento příkaz spustí hello Beeline klienta a připojí tooHiveServer2 hello hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-135">This command starts hello Beeline client, and connects tooHiveServer2 on hello cluster head node.</span></span> <span data-ttu-id="1e2b0-136">Po dokončení příkazu hello přijedete do `jdbc:hive2://headnodehost:10001/>` řádku.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-136">Once hello command completes, you arrive at a `jdbc:hive2://headnodehost:10001/>` prompt.</span></span>

2. <span data-ttu-id="1e2b0-137">Příkazy beeline začínat `!` znak, třeba `!help` zobrazí nápovědu.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-137">Beeline commands begin with a `!` character, for example `!help` displays help.</span></span> <span data-ttu-id="1e2b0-138">Ale hello `!` lze vynechat pro některé příkazy.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-138">However hello `!` can be omitted for some commands.</span></span> <span data-ttu-id="1e2b0-139">Například `help` funguje taky.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-139">For example, `help` also works.</span></span>

    <span data-ttu-id="1e2b0-140">Je `!sql`, což je použité tooexecute příkazy HiveQL.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-140">There is a `!sql`, which is used tooexecute HiveQL statements.</span></span> <span data-ttu-id="1e2b0-141">HiveQL se ale tak běžně používá, můžete vynechat hello předchozí `!sql`.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-141">However, HiveQL is so commonly used that you can omit hello preceding `!sql`.</span></span> <span data-ttu-id="1e2b0-142">odpovídají Hello následující dva příkazy:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-142">hello following two statements are equivalent:</span></span>

    ```hiveql
    !sql show tables;
    show tables;
    ```

    <span data-ttu-id="1e2b0-143">Na novém clusteru, je uvedena pouze jednu tabulku: **hivesampletable**.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-143">On a new cluster, only one table is listed: **hivesampletable**.</span></span>

3. <span data-ttu-id="1e2b0-144">Použijte následující příkaz toodisplay hello schéma pro hello hivesampletable hello:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-144">Use hello following command toodisplay hello schema for hello hivesampletable:</span></span>

    ```hiveql
    describe hivesampletable;
    ```

    <span data-ttu-id="1e2b0-145">Tento příkaz vrátí hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-145">This command returns hello following information:</span></span>

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

    <span data-ttu-id="1e2b0-146">Tyto informace popisují hello sloupců v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-146">This information describes hello columns in hello table.</span></span> <span data-ttu-id="1e2b0-147">Když jsme může provádět některé dotazy pro tato data, místo toho vytvoříme nové tabulky toodemonstrate jak tooload data do Hive a použijte schéma.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-147">While we could perform some queries against this data, let's instead create a brand new table toodemonstrate how tooload data into Hive and apply a schema.</span></span>

4. <span data-ttu-id="1e2b0-148">Zadejte následující příkazy toocreate tabulku s názvem hello **log4jLogs** pomocí ukázkových dat, které jsou součástí clusteru HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-148">Enter hello following statements toocreate a table named **log4jLogs** by using sample data provided with hello HDInsight cluster:</span></span>

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    <span data-ttu-id="1e2b0-149">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-149">These statements perform hello following actions:</span></span>

    * <span data-ttu-id="1e2b0-150">`DROP TABLE`– Pokud hello tabulka existuje, je odstraněn.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-150">`DROP TABLE` - If hello table exists, it is deleted.</span></span>

    * <span data-ttu-id="1e2b0-151">`CREATE EXTERNAL TABLE`-Vytvoří **externí** tabulky v Hive.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-151">`CREATE EXTERNAL TABLE` - Creates an **external** table in Hive.</span></span> <span data-ttu-id="1e2b0-152">Externí tabulky pouze uložit definici tabulky hello Hive.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-152">External tables only store hello table definition in Hive.</span></span> <span data-ttu-id="1e2b0-153">Hello data je ponechán v hello původního umístění.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-153">hello data is left in hello original location.</span></span>

    * <span data-ttu-id="1e2b0-154">`ROW FORMAT`-Hello data formátování.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-154">`ROW FORMAT` - How hello data is formatted.</span></span> <span data-ttu-id="1e2b0-155">V takovém případě hello pole v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-155">In this case, hello fields in each log are separated by a space.</span></span>

    * <span data-ttu-id="1e2b0-156">`STORED AS TEXTFILE LOCATION`-Tam, kde jsou uložena hello data a jaký formát souboru.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-156">`STORED AS TEXTFILE LOCATION` - Where hello data is stored and in what file format.</span></span>

    * <span data-ttu-id="1e2b0-157">`SELECT`– Počet všech řádků vybere kde sloupec **t4** obsahuje hodnotu hello **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-157">`SELECT` - Selects a count of all rows where column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="1e2b0-158">Tento dotaz vrací hodnotu **3** jsou tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-158">This query returns a value of **3** as there are three rows that contain this value.</span></span>

    * <span data-ttu-id="1e2b0-159">`INPUT__FILE__NAME LIKE '%.log'`-Hive pokusí tooapply hello schématu tooall soubory v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-159">`INPUT__FILE__NAME LIKE '%.log'` - Hive attempts tooapply hello schema tooall files in hello directory.</span></span> <span data-ttu-id="1e2b0-160">V takovém případě hello adresář obsahuje soubory, které neodpovídají schématu hello.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-160">In this case, hello directory contains files that do not match hello schema.</span></span> <span data-ttu-id="1e2b0-161">tooprevent paměti data ve výsledcích hello, tento příkaz informuje Hive, jsme by měl vrátit pouze data ze souborů končící na. log.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-161">tooprevent garbage data in hello results, this statement tells Hive that we should only return data from files ending in .log.</span></span>

  > [!NOTE]
  > <span data-ttu-id="1e2b0-162">Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-162">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="1e2b0-163">Například procesu nahrávání automatizované dat nebo operace MapReduce.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-163">For example, an automated data upload process or a MapReduce operation.</span></span>
  >
  > <span data-ttu-id="1e2b0-164">Vyřazení externí tabulku nemá **není** odstranit hello data, jenom hello definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-164">Dropping an external table does **not** delete hello data, only hello table definition.</span></span>

    <span data-ttu-id="1e2b0-165">Hello výstup tohoto příkazu je podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-165">hello output of this command is similar toohello following text:</span></span>

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

5. <span data-ttu-id="1e2b0-166">použít tooexit Beeline, `!exit`.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-166">tooexit Beeline, use `!exit`.</span></span>

## <span data-ttu-id="1e2b0-167"><a id="file"></a>Použít Beeline toorun soubor HiveQL</span><span class="sxs-lookup"><span data-stu-id="1e2b0-167"><a id="file"></a>Use Beeline toorun a HiveQL file</span></span>

<span data-ttu-id="1e2b0-168">Pomocí následujících kroků toocreate soubor, spusťte ji pomocí Beeline hello.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-168">Use hello following steps toocreate a file, then run it using Beeline.</span></span>

1. <span data-ttu-id="1e2b0-169">Použití hello následující příkaz toocreate soubor s názvem **query.hql**:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-169">Use hello following command toocreate a file named **query.hql**:</span></span>

    ```bash
    nano query.hql
    ```

2. <span data-ttu-id="1e2b0-170">Použijte následující text jako hello obsah souboru hello hello.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-170">Use hello following text as hello contents of hello file.</span></span> <span data-ttu-id="1e2b0-171">Tento dotaz vytvoří novou tabulku "interní" s názvem **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-171">This query creates a new 'internal' table named **errorLogs**:</span></span>

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    <span data-ttu-id="1e2b0-172">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-172">These statements perform hello following actions:</span></span>

    * <span data-ttu-id="1e2b0-173">**Vytvoření tabulka není v případě existuje** – Pokud hello tabulky již neexistuje, vytvoří se.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-173">**CREATE TABLE IF NOT EXISTS** - If hello table does not already exist, it is created.</span></span> <span data-ttu-id="1e2b0-174">Od hello **externí** – klíčové slovo není, tento příkaz vytvoří interní tabulku.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-174">Since hello **EXTERNAL** keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="1e2b0-175">Interní tabulky jsou uložené v hello Hive datového skladu a jsou zcela spravovány Hive.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-175">Internal tables are stored in hello Hive data warehouse and are managed completely by Hive.</span></span>
    * <span data-ttu-id="1e2b0-176">**ULOŽENÉ ORC AS** – ukládá hello data ve formátu optimalizované řádek sloupcovém (ORC).</span><span class="sxs-lookup"><span data-stu-id="1e2b0-176">**STORED AS ORC** - Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="1e2b0-177">Formát ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-177">ORC format is a highly optimized and efficient format for storing Hive data.</span></span>
    * <span data-ttu-id="1e2b0-178">**PŘEPSAT INSERT... Vyberte** -vybere řádky z hello **log4jLogs** tabulku, která obsahují **[Chyba]**, pak vloží hello data do hello **errorLogs** tabulky.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-178">**INSERT OVERWRITE ... SELECT** - Selects rows from hello **log4jLogs** table that contain **[ERROR]**, then inserts hello data into hello **errorLogs** table.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1e2b0-179">Na rozdíl od externích tabulek se odstranit interní tabulku odstraní hello základní data.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-179">Unlike external tables, dropping an internal table deletes hello underlying data as well.</span></span>

3. <span data-ttu-id="1e2b0-180">toosave hello soubor, použijte **Ctrl**+**_X**, pak zadejte **Y**a v neposlední řadě **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-180">toosave hello file, use **Ctrl**+**_X**, then enter **Y**, and finally **Enter**.</span></span>

4. <span data-ttu-id="1e2b0-181">Použijte následující soubor hello toorun pomocí Beeline hello:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-181">Use hello following toorun hello file using Beeline:</span></span>

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]
    > <span data-ttu-id="1e2b0-182">Hello `-i` parametr spustí Beeline, spustí hello příkazy v souboru query.hql hello.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-182">hello `-i` parameter starts Beeline, runs hello statements in hello query.hql file.</span></span> <span data-ttu-id="1e2b0-183">Po dokončení dotazu hello přijedete do hello `jdbc:hive2://headnodehost:10001/>` řádku.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-183">Once hello query completes, you arrive at hello `jdbc:hive2://headnodehost:10001/>` prompt.</span></span> <span data-ttu-id="1e2b0-184">Můžete taky spustit soubor pomocí hello `-f` parametr, který ukončí Beeline po dokončení dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-184">You can also run a file using hello `-f` parameter, which exits Beeline after hello query completes.</span></span>

5. <span data-ttu-id="1e2b0-185">tooverify, který hello **errorLogs** tabulka byla vytvořena, použijte následující příkaz tooreturn všechny řádky z hello hello **errorLogs**:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-185">tooverify that hello **errorLogs** table was created, use hello following statement tooreturn all hello rows from **errorLogs**:</span></span>

    ```hiveql
    SELECT * from errorLogs;
    ```

    <span data-ttu-id="1e2b0-186">Tří řádků dat má být vrácen, všechny obsahující **[Chyba]** v t4 sloupce:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-186">Three rows of data should be returned, all containing **[ERROR]** in column t4:</span></span>

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <span data-ttu-id="1e2b0-187"><a id="remote"></a>Použít Beeline vzdáleně</span><span class="sxs-lookup"><span data-stu-id="1e2b0-187"><a id="remote"></a>Use Beeline remotely</span></span>

<span data-ttu-id="1e2b0-188">Pokud máte Beeline nainstalovány místně, nebo ji používají prostřednictvím Docker image, jako [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), je nutné použít hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-188">If you have Beeline installed locally, or are using it through a Docker image such as [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), you must use hello following parameters:</span></span>

* <span data-ttu-id="1e2b0-189">__Připojovací řetězec__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`</span><span class="sxs-lookup"><span data-stu-id="1e2b0-189">__Connection string__: `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`</span></span>

* <span data-ttu-id="1e2b0-190">__Přihlašovací jméno clusteru__:`-n admin`</span><span class="sxs-lookup"><span data-stu-id="1e2b0-190">__Cluster login name__: `-n admin`</span></span>

* <span data-ttu-id="1e2b0-191">__Heslo pro přihlášení clusteru__`-p 'password'`</span><span class="sxs-lookup"><span data-stu-id="1e2b0-191">__Cluster login password__ `-p 'password'`</span></span>

<span data-ttu-id="1e2b0-192">Nahraďte hello `clustername` v hello připojovací řetězec s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-192">Replace hello `clustername` in hello connection string with hello name of your HDInsight cluster.</span></span>

<span data-ttu-id="1e2b0-193">Nahraďte `admin` s názvem hello přihlášení clusteru a nahraďte `password` hello heslem pro váš cluster přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-193">Replace `admin` with hello name of your cluster login, and replace `password` with hello password for your cluster login.</span></span>

## <span data-ttu-id="1e2b0-194"><a id="sparksql"></a>Beeline pomocí Spark</span><span class="sxs-lookup"><span data-stu-id="1e2b0-194"><a id="sparksql"></a>Use Beeline with Spark</span></span>

<span data-ttu-id="1e2b0-195">Spark poskytuje svou vlastní implementaci systému HiveServer2, což se často stává serveru Spark Thrift hello tooas zmíněných.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-195">Spark provides its own implementation of HiveServer2, which is often refered tooas hello Spark Thrift server.</span></span> <span data-ttu-id="1e2b0-196">Tato služba používá dotazů Spark SQL tooresolve místo Hive a může poskytovat lepší výkon v závislosti na svůj dotaz.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-196">This service uses Spark SQL tooresolve queries instead of Hive, and may provide better performance depending on your query.</span></span>

<span data-ttu-id="1e2b0-197">tooconnect toohello serveru Spark Thrift Spark na clusteru HDInsight, použití portu `10002` místo `10001`.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-197">tooconnect toohello Spark Thrift server of a Spark on HDInsight cluster, use port `10002` instead of `10001`.</span></span> <span data-ttu-id="1e2b0-198">Například, `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-198">For example, `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e2b0-199">serveru Spark Thrift Hello nejsou přímo přístupné přes hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-199">hello Spark Thrift server is not directly accessible over hello internet.</span></span> <span data-ttu-id="1e2b0-200">Můžete připojit z relace SSH tooit nebo pouze uvnitř hello stejné virtuální síti Azure jako hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1e2b0-200">You can only connect tooit from an SSH session or inside hello same Azure Virtual Network as hello HDInsight cluster.</span></span>

## <span data-ttu-id="1e2b0-201"><a id="summary"></a><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e2b0-201"><a id="summary"></a><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="1e2b0-202">Další obecné informace o Hive v HDInsight najdete v části hello následujícím dokumentu:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-202">For more general information on Hive in HDInsight, see hello following document:</span></span>

* [<span data-ttu-id="1e2b0-203">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1e2b0-203">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="1e2b0-204">Další informace o jiných způsobech můžete pracovat s Hadoop v HDInsight naleznete v tématu hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-204">For more information on other ways you can work with Hadoop on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="1e2b0-205">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1e2b0-205">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="1e2b0-206">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1e2b0-206">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="1e2b0-207">Pokud používáte Tez s Hive, zobrazit hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="1e2b0-207">If you are using Tez with Hive, see hello following documents:</span></span>

* [<span data-ttu-id="1e2b0-208">Použití hello Tez uživatelského rozhraní na HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="1e2b0-208">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)
* [<span data-ttu-id="1e2b0-209">Použití hello zobrazení Ambari Tez na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="1e2b0-209">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

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
