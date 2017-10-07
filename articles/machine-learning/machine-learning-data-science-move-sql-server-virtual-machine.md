---
title: "aaaMove data tooSQL Server na virtuální počítač Azure | Microsoft Docs"
description: "Přesun dat z plochých souborů nebo z tooSQL systému SQL Server místní Server na virtuálním počítači Azure."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 2c9ef1d3-4f5c-4b1f-bf06-223646c8af06
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 63e02158f9c99f4478c4eb1cde62c877983dcf27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-toosql-server-on-an-azure-virtual-machine"></a>Přesunutí dat tooSQL Server na virtuální počítač Azure
Toto téma popisuje možnosti hello pro přesun dat z plochých souborů (formáty CSV nebo TSV) nebo z tooSQL systému SQL Server místní Server na virtuální počítač Azure. Tyto úlohy pro přesunutí dat v cloudu toohello jsou součástí hello proces vědecké účely dat týmu.

Téma, které popisuje možnosti hello přesunutí dat tooan Azure SQL Database pro Machine Learning, najdete v části [přesunout data tooan Azure SQL Database pro Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).

Hello **nabídky** pod tootopics odkazy, které popisují, jak tooingest data do jiných cíl prostředích, kde můžete ukládat a zpracovávat během hello data hello tým datové vědy procesu (TDSP).

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Hello následující tabulka shrnuje hello možnosti pro přesunutí dat tooSQL Server na virtuální počítač Azure.

| <b>ZDROJ</b> | <b>Cíl: SQL Server na virtuální počítač Azure</b> |
| --- | --- |
| <b>Plochý soubor</b> |1. <a href="#insert-tables-bcp">Nástroj příkazového řádku pro hromadné kopírování (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">Hromadné vložení SQL dotazu</a><br> 3. <a href="#sql-builtin-utilities">Grafické nástroje integrovanou v systému SQL Server</a> |
| <b>Místní SQL Server</b> |1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Nasazení virtuálních počítačů služby Microsoft Azure Průvodce tooa databáze serveru SQL</a><br> 2. <a href="#export-flat-file">Export tooa plochý soubor</a><br> 3. <a href="#sql-migration">Průvodce migrací databáze SQL</a> <br> 4. <a href="#sql-backup">Databáze back up a obnovení</a><br> |

Všimněte si, že tento dokument předpokládá, že příkazy SQL budou provedeny z SQL Server Management Studio nebo Visual Studio Průzkumník databáze.

> [!TIP]
> Jako alternativu, můžete použít [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) toocreate a naplánovat kanál, který se přesune data tooa virtuální počítač s SQL serverem v Azure. Další informace najdete v tématu [kopírování dat pomocí Azure Data Factory (aktivita kopírování)](../data-factory/data-factory-data-movement-activities.md).
>
>

## <a name="prereqs"></a>Požadavky
Tento kurz předpokládá, že máte:

* **Předplatné**. Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* **Účtu úložiště Azure**. Pro ukládání dat hello v tomto kurzu budete používat účet úložiště Azure. Pokud nemáte účet úložiště Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) článku. Po vytvoření účtu úložiště hello, budete potřebovat účet hello tooobtain, klíč používá tooaccess hello úložiště. V tématu [Správa přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Zřízení **systému SQL Server ve virtuálním počítači Azure**. Pokyny najdete v tématu [nastavení se virtuální počítač Azure SQL Server jako server IPython Poznámkový blok pro pokročilou analýzu](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Nainstalovaný a nakonfigurovaný **prostředí Azure PowerShell** místně. Pokyny najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a name="filesource_to_sqlonazurevm"></a>Přesun dat z tooSQL plochý soubor zdrojového serveru na virtuálním počítači Azure
Pokud vaše data v plochý soubor (seřazeny ve formátu řádku nebo sloupce), může být přesunutý tooSQL virtuální počítač Server na platformě Azure prostřednictvím hello následující metody:

1. [Nástroj příkazového řádku pro hromadné kopírování (BCP)](#insert-tables-bcp)
2. [Hromadné vložení SQL dotazu](#insert-tables-bulkquery)
3. [Grafické nástroje integrovanou v systému SQL Server (importu a exportu, SSIS)](#sql-builtin-utilities)

### <a name="insert-tables-bcp"></a>Nástroj příkazového řádku pro hromadné kopírování (BCP)
Je nainstalovaný se systémem SQL Server nástroj příkazového řádku BCP a je jedním z hello nejrychlejší způsoby toomove data. Funguje v různých všechny tři varianty systému SQL Server (místní SQL Server, SQL Azure a virtuální počítač s SQL serverem v Azure).

> [!NOTE]
> **Kam mají být data pro BCP?**  
> I když není povinné, že soubory obsahující zdroje dat umístěných na stejném počítači jako hello cílového systému SQL Server umožňuje rychlejší přenosů (sítě rychlost vs místní disk vstupně-výstupní operace rychlost) hello. Můžete přesunout hello ploché soubory obsahující počítače toohello data tam, kde je nainstalován systém SQL Server pomocí různých kopírování souborů nástroje jako [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Storage Explorer](http://storageexplorer.com/) nebo windows, kopírování a vkládání přes vzdálené Protokol plochy (RDP).
>
>

1. Zajistěte, aby hello databáze a tabulky hello jsou vytvořené v databázi systému SQL Server cílového hello. Tady je příklad toho, jak toodo této pomocí hello `Create Database` a `Create Table` příkazy:

        CREATE DATABASE <database_name>

        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        )
2. Generovat hello formát souboru, který popisuje hello schématu pro tabulku hello vydáním hello následující příkaz z příkazového řádku hello počítače hello nainstalovanou bcp.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
3. Vkládání dat hello do hello databáze pomocí příkazu bcp hello následujícím způsobem. Tento postup měl fungovat z příkazového řádku hello za předpokladu, že hello SQL Server je nainstalován ve stejném počítači:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Optimalizace BCP vloží** naleznete hello následujícího článku ["Pokyny pro optimalizaci hromadným importem"](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) toooptimize takové vloží.
>
>

### <a name="insert-tables-bulkquery-parallel"></a>Paralelním prováděním vložení pro rychlejší přesun dat
Pokud přesouváte data hello velká, můžete urychlit věcí současně spuštěním více BCP příkazů paralelně ve skriptu prostředí PowerShell.

> [!NOTE]
> **Velké objemy dat přijímání** načítání pro velké a velké datové sady dat toooptimize oddílu logické a fyzické databázových tabulek pomocí více skupin souborů a oddíl tabulky. Další informace o vytváření a načítání dat tabulky toopartition najdete v tématu [paralelní tabulky oddílů SQL zatížení](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).
>
>

Hello ukázkový skript prostředí PowerShell, které jsou níže ukazují paralelní vloží při použití nástroje bcp:

    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for hello script tooexecute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n
      }


    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }


    # Wait for it all toocomplete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }

    # Getting hello information back from hello jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset hello execution policy


### <a name="insert-tables-bulkquery"></a>Hromadné vložení SQL dotazu
[Hromadné vložení dotazu SQL](https://msdn.microsoft.com/library/ms188365) lze použít tooimport data do databáze hello z řádku nebo sloupce na základě souborů (hello podporované typy jsou zahrnuté v[Příprava Data pro hromadné exportu nebo importu (SQL Server)](https://msdn.microsoft.com/library/ms188609)) tématu.

Tady jsou některé ukázkové příkazy pro hromadné vložení se, jak je uvedeno níže:  

1. Analyzovat data a nastavte všechny vlastní možnosti před importem toomake se, že se že předpokládá, že databáze systému SQL Server hello hello stejný formát pro všechny speciální pole, jako je například kalendářní data. Tady je příklad, jak tooset hello datum formátovat jako rok měsíc den (Pokud data obsahují hello datum ve formátu rok měsíc den):

        SET DATEFORMAT ymd;    
2. Importujte dat pomocí hromadného importu příkazy:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be hello row separator in your data
        )

### <a name="sql-builtin-utilities"></a>Integrované nástroje SQL serveru
Integrace služby SSIS (SQL Server) tooimport data můžete použít na virtuální počítač s SQL serverem na Azure z plochého souboru.
Služby SSIS je k dispozici ve dvou studio prostředí. Podrobnosti najdete v tématu [Integration Services (SSIS) a prostředí Studio](https://technet.microsoft.com/library/ms140028.aspx):

* Podrobnosti na SQL Server Data Tools najdete v tématu [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
* Podrobnosti na hello Průvodce importem a exportem najdete v tématu [Microsoft SQL Server importovat a exportovat Průvodce](https://msdn.microsoft.com/library/ms141209.aspx)

## <a name="sqlonprem_to_sqlonazurevm"></a>Přesun dat z tooSQL místní SQL Server Server na virtuálním počítači Azure
Můžete také použít hello následující strategie migrace:

1. [Nasazení virtuálních počítačů služby Microsoft Azure Průvodce tooa databáze serveru SQL](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Export tooFlat souboru](#export-flat-file)
3. [Průvodce migrací databáze SQL](#sql-migration)
4. [Databáze back up a obnovení](#sql-backup)

Jsme popisují každou z těchto níže:

### <a name="deploy-a-sql-server-database-tooa-microsoft-azure-vm-wizard"></a>Nasazení virtuálních počítačů služby Microsoft Azure Průvodce tooa databáze serveru SQL
Hello **nasazení virtuálních počítačů služby Microsoft Azure Průvodce tooa databáze systému SQL Server** je jednoduchý a doporučený způsob toomove dat z tooSQL instance systému SQL Server na místní Server na virtuálním počítači Azure. Podrobné kroky a také se zabývat náhradních řešení, najdete v části [migraci databáze tooSQL Server na virtuálním počítači Azure](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Export tooFlat souboru
Různé metody lze použít toobulk exportu dat z místního serveru SQL, jak je uvedeno v hello [hromadný Import a Export dat (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) tématu. Tento dokument se týká hello hromadné kopírování programu (BCP) jako příklad. Jakmile data se exportují do plochého souboru, může být importované tooanother systému SQL server pomocí hromadného importu.

1. Exportovat hello data z místního serveru SQL Server tooa souboru pomocí nástroj bcp hello takto

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. Vytvoření hello databáze a tabulky hello na virtuální počítač s SQL serverem v Azure pomocí hello `create database` a `create table` pro schéma tabulky hello exportovali v kroku 1.
3. Vytvořte soubor ve formátu pro popisující hello schématu tabulky dat hello se exportovat nebo importovat. Podrobnosti o hello formátového souboru jsou popsané v [vytvořit soubor ve formátu (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).

    Generování souboru formátu při spuštění BCP z počítače systému SQL Server hello

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    Generování souboru formátu při spuštění BCP vzdáleně s SQL serverem

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. Použít některou z metod hello popsané v části [přesun dat ze souboru zdroje](#filesource_to_sqlonazurevm) toomove hello data v tooa plochých souborů systému SQL Server.

### <a name="sql-migration"></a>Průvodce migrací databáze SQL
[Průvodce migrací databáze SQL serveru](http://sqlazuremw.codeplex.com/) obsahuje uživatelsky přívětivé toomove dat mezi dvě instance serveru SQL. Umožňuje schéma dat hello uživatele toomap hello mezi zdroji a cílové tabulky, vyberte typy sloupců a různé další funkce. V části zahrnuje hello používá hromadné kopírování (BCP). Snímek obrazovky hello úvodní obrazovka pro hello migraci databáze SQL průvodce jsou uvedeny níže.  

![Průvodce migrací serveru SQL][2]

### <a name="sql-backup"></a>Databáze back up a obnovení
Systém SQL Server podporuje:

1. [Zálohu databáze zpět a obnovit funkčnost](https://msdn.microsoft.com/library/ms187048.aspx) (obě tooa místního souboru nebo souboru bacpac export tooblob) a [aplikace datové vrstvy](https://msdn.microsoft.com/library/ee210546.aspx) (pomocí souboru bacpac).
2. Možnost toodirectly vytvořit virtuálním počítačům systému SQL Server na platformě Azure zkopírovaný databáze nebo kopírování tooan existující databázi SQL Azure. Další podrobnosti najdete v tématu [hello pomocí Průvodce kopírováním databáze](https://msdn.microsoft.com/library/ms188664.aspx).

Snímek obrazovky hello databáze zpět nebo obnovit možnosti z SQL Server Management Studio jsou uvedeny níže.

![Nástroj pro Import serveru SQL][1]

## <a name="resources"></a>Zdroje
[Migrace databáze tooSQL Server na virtuálním počítači Azure](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[SQL Server na Azure Virtual Machines – přehled](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
