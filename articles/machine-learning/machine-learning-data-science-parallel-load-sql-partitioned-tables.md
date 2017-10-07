---
title: "aaaBuild a optimalizovat tabulky pro rychlé paralelní import dat do systému SQL Server na virtuálním počítači Azure | Microsoft Docs"
description: "Paralelní hromadný import dat pomocí tabulek oddílů SQL"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ff90fdb0-5bc7-49e8-aee7-678b54f901c8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: ab748c47348ec6ca3b98ba39e27181bba5d36fc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a><span data-ttu-id="c042b-103">Paralelní hromadný import dat pomocí tabulek oddílů SQL</span><span class="sxs-lookup"><span data-stu-id="c042b-103">Parallel Bulk Data Import Using SQL Partition Tables</span></span>
<span data-ttu-id="c042b-104">Tento dokument popisuje, jak toobuild oddíly tabulky pro rychlé paralelní hromadný import databáze systému SQL Server tooa data.</span><span class="sxs-lookup"><span data-stu-id="c042b-104">This document describes how toobuild partitioned tables for fast parallel bulk importing of data tooa SQL Server database.</span></span> <span data-ttu-id="c042b-105">Pro databázi SQL tooa načítání nebo přenos velkých objemů dat, import dat toohello databázi SQL a následné dotazy lze zvýšit pomocí *rozdělena na oddíly tabulky a zobrazení*.</span><span class="sxs-lookup"><span data-stu-id="c042b-105">For big data loading/transfer tooa SQL database, importing data toohello SQL DB and subsequent queries can be improved by using *Partitioned Tables and Views*.</span></span> 

## <a name="create-a-new-database-and-a-set-of-filegroups"></a><span data-ttu-id="c042b-106">Vytvořit novou databázi a sadu skupin souborů</span><span class="sxs-lookup"><span data-stu-id="c042b-106">Create a new database and a set of filegroups</span></span>
* <span data-ttu-id="c042b-107">[Vytvořit novou databázi](https://technet.microsoft.com/library/ms176061.aspx), pokud již neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c042b-107">[Create a new database](https://technet.microsoft.com/library/ms176061.aspx), if it doesn't exist already.</span></span>
* <span data-ttu-id="c042b-108">Přidejte databázi toohello skupiny souborů databáze, která bude obsahovat fyzické soubory hello rozdělena na oddíly. To lze provést pomocí [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) Pokud nové nebo [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) Pokud hello databáze již existuje.</span><span class="sxs-lookup"><span data-stu-id="c042b-108">Add database filegroups toohello database which will hold hello partitioned physical files.This can be done with [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) if new or [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) if hello database exists already.</span></span>
* <span data-ttu-id="c042b-109">Přidejte jeden nebo více souborů databáze tooeach soubory (podle potřeby).</span><span class="sxs-lookup"><span data-stu-id="c042b-109">Add one or more files (as needed) tooeach database filegroup.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="c042b-110">Zadejte cílové skupině souborů hello, která obsahuje data pro tento oddíl a hello názvy souborů fyzická databáze, kde bude uložena data hello skupiny souborů.</span><span class="sxs-lookup"><span data-stu-id="c042b-110">Specify hello target filegroup which holds data for this partition and hello physical database file name(s) where hello filegroup data will be stored.</span></span>
  > 
  > 

<span data-ttu-id="c042b-111">Hello následující příklad vytvoří novou databázi s tři skupiny souborů než hello primární a skupiny protokolu obsahující jeden fyzický soubor v každé.</span><span class="sxs-lookup"><span data-stu-id="c042b-111">hello following example creates a new database with three filegroups other than hello primary and log groups, containing one physical file in each.</span></span> <span data-ttu-id="c042b-112">soubory databáze Hello vytvářejí hello výchozí složky dat SQL serveru, jako nakonfigurovaný v instanci systému SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="c042b-112">hello database files are created in hello default SQL Server Data folder, as configured in hello SQL Server instance.</span></span> <span data-ttu-id="c042b-113">Další informace o hello výchozí umístění souborů najdete v tématu [umístění souborů pro výchozí a s názvem instance systému SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).</span><span class="sxs-lookup"><span data-stu-id="c042b-113">For more information about hello default file locations, see [File Locations for Default and Named Instances of SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).</span></span>

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

    EXECUTE ('
        CREATE DATABASE <database_name>
         ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
         LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')

## <a name="create-a-partitioned-table"></a><span data-ttu-id="c042b-114">Vytvoření oddílů tabulky</span><span class="sxs-lookup"><span data-stu-id="c042b-114">Create a partitioned table</span></span>
<span data-ttu-id="c042b-115">Vytvoření oddílů tabulky či tabulek podle schématu toohello data, skupiny souborů databáze namapované toohello vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="c042b-115">Create partitioned table(s) according toohello data schema, mapped toohello database filegroups created in hello previous step.</span></span> <span data-ttu-id="c042b-116">Když je hromadného importu dat toohello oddíly tabulek, záznamy rozdělena mezi hello skupiny souborů podle tooa schéma oddílu, jak je popsáno níže.</span><span class="sxs-lookup"><span data-stu-id="c042b-116">When data is bulk imported toohello partitioned table(s), records will be distributed among hello filegroups according tooa partition scheme, as described below.</span></span>

<span data-ttu-id="c042b-117">**toocreate tabulka oddílů, budete muset:**</span><span class="sxs-lookup"><span data-stu-id="c042b-117">**toocreate a partition table, you need to:**</span></span>

* <span data-ttu-id="c042b-118">[Vytvořit funkci oddílu](https://msdn.microsoft.com/library/ms187802.aspx) určující hello rozsah hodnot nebo hranice toobe součástí každé jednotlivé oddílu tabulky, například toolimit oddíly tím, že měsíc (některé\_data a času\_pole) hello roku 2013:</span><span class="sxs-lookup"><span data-stu-id="c042b-118">[Create a partition function](https://msdn.microsoft.com/library/ms187802.aspx) which defines hello range of values/boundaries toobe included in each individual partition table, e.g., toolimit partitions by month(some\_datetime\_field) in hello year 2013:</span></span>
  
        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )
* <span data-ttu-id="c042b-119">[Vytvořit schéma oddílu](https://msdn.microsoft.com/library/ms179854.aspx) které mapuje každý oddíl rozsah hello oddílu funkce tooa fyzické skupiny souborů, například:</span><span class="sxs-lookup"><span data-stu-id="c042b-119">[Create a partition scheme](https://msdn.microsoft.com/library/ms179854.aspx) which maps each partition range in hello partition function tooa physical filegroup, e.g.:</span></span>
  
        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> too(
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )
  
  <span data-ttu-id="c042b-120">rozsahy hello tooverify platí v každém oddílu podle funkce nebo schéma toohello, spusťte následující dotaz hello:</span><span class="sxs-lookup"><span data-stu-id="c042b-120">tooverify hello ranges in effect in each partition according toohello function/scheme, run hello following query:</span></span>
  
        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>
* <span data-ttu-id="c042b-121">[Vytvoření oddílů tabulky](https://msdn.microsoft.com/library/ms174979.aspx)(s) podle tooyour schéma dat a zadejte hello oddílu schématu a omezení pole používá toopartition hello tabulky, například:</span><span class="sxs-lookup"><span data-stu-id="c042b-121">[Create partitioned table](https://msdn.microsoft.com/library/ms174979.aspx)(s) according tooyour data schema, and specify hello partition scheme and constraint field used toopartition hello table, e.g.:</span></span>
  
        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

<span data-ttu-id="c042b-122">Další informace najdete v tématu [vytvoření tabulky rozdělena na oddíly a indexy](https://msdn.microsoft.com/library/ms188730.aspx).</span><span class="sxs-lookup"><span data-stu-id="c042b-122">For more information, see [Create Partitioned Tables and Indexes](https://msdn.microsoft.com/library/ms188730.aspx).</span></span>

## <a name="bulk-import-hello-data-for-each-individual-partition-table"></a><span data-ttu-id="c042b-123">Hromadný import hello dat pro každou tabulku jednotlivých oddílu</span><span class="sxs-lookup"><span data-stu-id="c042b-123">Bulk import hello data for each individual partition table</span></span>
* <span data-ttu-id="c042b-124">Můžete použít BCP, BULK INSERT nebo jiné metody, jako [Průvodce migrací serveru SQL](http://sqlazuremw.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="c042b-124">You may use BCP, BULK INSERT, or other methods such as [SQL Server Migration Wizard](http://sqlazuremw.codeplex.com/).</span></span> <span data-ttu-id="c042b-125">Ukázkový Hello používá metodu BCP hello.</span><span class="sxs-lookup"><span data-stu-id="c042b-125">hello example provided uses hello BCP method.</span></span>
* <span data-ttu-id="c042b-126">[Příkaz ALTER hello database](https://msdn.microsoft.com/library/bb522682.aspx) toochange transakce protokolování režijní náklady na schéma tooBULK_LOGGED toominimize protokolování, například:</span><span class="sxs-lookup"><span data-stu-id="c042b-126">[Alter hello database](https://msdn.microsoft.com/library/bb522682.aspx) toochange transaction logging scheme tooBULK_LOGGED toominimize overhead of logging, e.g.:</span></span>
  
        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED
* <span data-ttu-id="c042b-127">data tooexpedite načítání, spusťte hello hromadné operace importu paralelně.</span><span class="sxs-lookup"><span data-stu-id="c042b-127">tooexpedite data loading, launch hello bulk import operations in parallel.</span></span> <span data-ttu-id="c042b-128">Tipy k urychlení hromadný import velkých objemů dat do databáze systému SQL Server, najdete v tématu [načíst 1TB za méně než 1 hodinu](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).</span><span class="sxs-lookup"><span data-stu-id="c042b-128">For tips on expediting bulk importing of big data into SQL Server databases, see [Load 1TB in less than 1 hour](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).</span></span>

<span data-ttu-id="c042b-129">Hello následující skript prostředí PowerShell je příklad paralelní dat pomocí BCP načítání.</span><span class="sxs-lookup"><span data-stu-id="c042b-129">hello following PowerShell script is an example of parallel data loading using BCP.</span></span>

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # hello example assumes hello partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes hello input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0

    # For SQL authentication, set hello server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match hello number of input data files per table
    $numofparts = <number_of_partitions>

    # Set table name toobe loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"

    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir

    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }

    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }

    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }

    Get-Job

    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-toooptimize-joins-and-query-performance"></a><span data-ttu-id="c042b-130">Vytvořit spojení toooptimize indexy a výkon dotazů</span><span class="sxs-lookup"><span data-stu-id="c042b-130">Create indexes toooptimize joins and query performance</span></span>
* <span data-ttu-id="c042b-131">Pokud bude extrahovat data pro modelování z více tabulek, vytvořte indexy na hello spojení klíče tooimprove hello spojení výkonu.</span><span class="sxs-lookup"><span data-stu-id="c042b-131">If you will extract data for modeling from multiple tables, create indexes on hello join keys tooimprove hello join performance.</span></span>
* <span data-ttu-id="c042b-132">[Vytvářejte indexy](https://technet.microsoft.com/library/ms188783.aspx) (Clusterované či neclusterované) cílení hello stejné skupiny souborů pro každý oddíl pro např:</span><span class="sxs-lookup"><span data-stu-id="c042b-132">[Create indexes](https://technet.microsoft.com/library/ms188783.aspx) (clustered or non-clustered) targeting hello same filegroup for each partition, for e.g.:</span></span>
  
        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  <span data-ttu-id="c042b-133">Nebo,</span><span class="sxs-lookup"><span data-stu-id="c042b-133">or,</span></span>
  
        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  
  > [!NOTE]
  > <span data-ttu-id="c042b-134">Můžete se rozhodnout toocreate hello indexy před hromadného importu dat hello.</span><span class="sxs-lookup"><span data-stu-id="c042b-134">You may choose toocreate hello indexes before bulk importing hello data.</span></span> <span data-ttu-id="c042b-135">Vytvoření indexu před hromadný import bude zpomalit načítání dat hello.</span><span class="sxs-lookup"><span data-stu-id="c042b-135">Index creation before bulk importing will slow down hello data loading.</span></span>
  > 
  > 

## <a name="advanced-analytics-process-and-technology-in-action-example"></a><span data-ttu-id="c042b-136">Proces pokročilou analýzu a technologie v příkladu akce</span><span class="sxs-lookup"><span data-stu-id="c042b-136">Advanced Analytics Process and Technology in Action Example</span></span>
<span data-ttu-id="c042b-137">Příklad začátku do konce návod hello procesu analýzy Cortana pomocí veřejné datové sady, naleznete v části [Cortana Analytics procesu v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="c042b-137">For an end-to-end walkthrough example using hello Cortana Analytics Process with a public dataset, see [Cortana Analytics Process in Action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

