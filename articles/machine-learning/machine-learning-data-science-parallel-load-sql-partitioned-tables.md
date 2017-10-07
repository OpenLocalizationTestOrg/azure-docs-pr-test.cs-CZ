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
# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Paralelní hromadný import dat pomocí tabulek oddílů SQL
Tento dokument popisuje, jak toobuild oddíly tabulky pro rychlé paralelní hromadný import databáze systému SQL Server tooa data. Pro databázi SQL tooa načítání nebo přenos velkých objemů dat, import dat toohello databázi SQL a následné dotazy lze zvýšit pomocí *rozdělena na oddíly tabulky a zobrazení*. 

## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Vytvořit novou databázi a sadu skupin souborů
* [Vytvořit novou databázi](https://technet.microsoft.com/library/ms176061.aspx), pokud již neexistuje.
* Přidejte databázi toohello skupiny souborů databáze, která bude obsahovat fyzické soubory hello rozdělena na oddíly. To lze provést pomocí [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) Pokud nové nebo [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) Pokud hello databáze již existuje.
* Přidejte jeden nebo více souborů databáze tooeach soubory (podle potřeby).
  
  > [!NOTE]
  > Zadejte cílové skupině souborů hello, která obsahuje data pro tento oddíl a hello názvy souborů fyzická databáze, kde bude uložena data hello skupiny souborů.
  > 
  > 

Hello následující příklad vytvoří novou databázi s tři skupiny souborů než hello primární a skupiny protokolu obsahující jeden fyzický soubor v každé. soubory databáze Hello vytvářejí hello výchozí složky dat SQL serveru, jako nakonfigurovaný v instanci systému SQL Server hello. Další informace o hello výchozí umístění souborů najdete v tématu [umístění souborů pro výchozí a s názvem instance systému SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).

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

## <a name="create-a-partitioned-table"></a>Vytvoření oddílů tabulky
Vytvoření oddílů tabulky či tabulek podle schématu toohello data, skupiny souborů databáze namapované toohello vytvořili v předchozím kroku hello. Když je hromadného importu dat toohello oddíly tabulek, záznamy rozdělena mezi hello skupiny souborů podle tooa schéma oddílu, jak je popsáno níže.

**toocreate tabulka oddílů, budete muset:**

* [Vytvořit funkci oddílu](https://msdn.microsoft.com/library/ms187802.aspx) určující hello rozsah hodnot nebo hranice toobe součástí každé jednotlivé oddílu tabulky, například toolimit oddíly tím, že měsíc (některé\_data a času\_pole) hello roku 2013:
  
        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )
* [Vytvořit schéma oddílu](https://msdn.microsoft.com/library/ms179854.aspx) které mapuje každý oddíl rozsah hello oddílu funkce tooa fyzické skupiny souborů, například:
  
        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> too(
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )
  
  rozsahy hello tooverify platí v každém oddílu podle funkce nebo schéma toohello, spusťte následující dotaz hello:
  
        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>
* [Vytvoření oddílů tabulky](https://msdn.microsoft.com/library/ms174979.aspx)(s) podle tooyour schéma dat a zadejte hello oddílu schématu a omezení pole používá toopartition hello tabulky, například:
  
        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Další informace najdete v tématu [vytvoření tabulky rozdělena na oddíly a indexy](https://msdn.microsoft.com/library/ms188730.aspx).

## <a name="bulk-import-hello-data-for-each-individual-partition-table"></a>Hromadný import hello dat pro každou tabulku jednotlivých oddílu
* Můžete použít BCP, BULK INSERT nebo jiné metody, jako [Průvodce migrací serveru SQL](http://sqlazuremw.codeplex.com/). Ukázkový Hello používá metodu BCP hello.
* [Příkaz ALTER hello database](https://msdn.microsoft.com/library/bb522682.aspx) toochange transakce protokolování režijní náklady na schéma tooBULK_LOGGED toominimize protokolování, například:
  
        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED
* data tooexpedite načítání, spusťte hello hromadné operace importu paralelně. Tipy k urychlení hromadný import velkých objemů dat do databáze systému SQL Server, najdete v tématu [načíst 1TB za méně než 1 hodinu](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

Hello následující skript prostředí PowerShell je příklad paralelní dat pomocí BCP načítání.

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


## <a name="create-indexes-toooptimize-joins-and-query-performance"></a>Vytvořit spojení toooptimize indexy a výkon dotazů
* Pokud bude extrahovat data pro modelování z více tabulek, vytvořte indexy na hello spojení klíče tooimprove hello spojení výkonu.
* [Vytvářejte indexy](https://technet.microsoft.com/library/ms188783.aspx) (Clusterované či neclusterované) cílení hello stejné skupiny souborů pro každý oddíl pro např:
  
        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  Nebo,
  
        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  
  > [!NOTE]
  > Můžete se rozhodnout toocreate hello indexy před hromadného importu dat hello. Vytvoření indexu před hromadný import bude zpomalit načítání dat hello.
  > 
  > 

## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Proces pokročilou analýzu a technologie v příkladu akce
Příklad začátku do konce návod hello procesu analýzy Cortana pomocí veřejné datové sady, naleznete v části [Cortana Analytics procesu v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md).

