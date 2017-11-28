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
# <a name="move-data-toosql-server-on-an-azure-virtual-machine"></a><span data-ttu-id="3751e-103">Přesunutí dat tooSQL Server na virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="3751e-103">Move data tooSQL Server on an Azure virtual machine</span></span>
<span data-ttu-id="3751e-104">Toto téma popisuje možnosti hello pro přesun dat z plochých souborů (formáty CSV nebo TSV) nebo z tooSQL systému SQL Server místní Server na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="3751e-104">This topic outlines hello options for moving data either from flat files (CSV or TSV formats) or from an on-premises SQL Server tooSQL Server on an Azure virtual machine.</span></span> <span data-ttu-id="3751e-105">Tyto úlohy pro přesunutí dat v cloudu toohello jsou součástí hello proces vědecké účely dat týmu.</span><span class="sxs-lookup"><span data-stu-id="3751e-105">These tasks for moving data toohello cloud are part of hello Team Data Science Process.</span></span>

<span data-ttu-id="3751e-106">Téma, které popisuje možnosti hello přesunutí dat tooan Azure SQL Database pro Machine Learning, najdete v části [přesunout data tooan Azure SQL Database pro Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span><span class="sxs-lookup"><span data-stu-id="3751e-106">For a topic that outlines hello options for moving data tooan Azure SQL Database for Machine Learning, see [Move data tooan Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span></span>

<span data-ttu-id="3751e-107">Hello **nabídky** pod tootopics odkazy, které popisují, jak tooingest data do jiných cíl prostředích, kde můžete ukládat a zpracovávat během hello data hello tým datové vědy procesu (TDSP).</span><span class="sxs-lookup"><span data-stu-id="3751e-107">hello **menu** below links tootopics that describe how tooingest data into other target environments where hello data can be stored and processed during hello Team Data Science Process (TDSP).</span></span>

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<span data-ttu-id="3751e-108">Hello následující tabulka shrnuje hello možnosti pro přesunutí dat tooSQL Server na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="3751e-108">hello following table summarizes hello options for moving data tooSQL Server on an Azure virtual machine.</span></span>

| <span data-ttu-id="3751e-109"><b>ZDROJ</b></span><span class="sxs-lookup"><span data-stu-id="3751e-109"><b>SOURCE</b></span></span> | <span data-ttu-id="3751e-110"><b>Cíl: SQL Server na virtuální počítač Azure</b></span><span class="sxs-lookup"><span data-stu-id="3751e-110"><b>DESTINATION: SQL Server on Azure VM</b></span></span> |
| --- | --- |
| <span data-ttu-id="3751e-111"><b>Plochý soubor</b></span><span class="sxs-lookup"><span data-stu-id="3751e-111"><b>Flat File</b></span></span> |<span data-ttu-id="3751e-112">1. <a href="#insert-tables-bcp">Nástroj příkazového řádku pro hromadné kopírování (BCP)</a></span><span class="sxs-lookup"><span data-stu-id="3751e-112">1. <a href="#insert-tables-bcp">Command-line bulk copy utility (BCP) </a></span></span><br> <span data-ttu-id="3751e-113">2. <a href="#insert-tables-bulkquery">Hromadné vložení SQL dotazu</a></span><span class="sxs-lookup"><span data-stu-id="3751e-113">2. <a href="#insert-tables-bulkquery">Bulk Insert SQL Query </a></span></span><br> <span data-ttu-id="3751e-114">3. <a href="#sql-builtin-utilities">Grafické nástroje integrovanou v systému SQL Server</a></span><span class="sxs-lookup"><span data-stu-id="3751e-114">3. <a href="#sql-builtin-utilities">Graphical Built-in Utilities in SQL Server</a></span></span> |
| <span data-ttu-id="3751e-115"><b>Místní SQL Server</b></span><span class="sxs-lookup"><span data-stu-id="3751e-115"><b>On-Premises SQL Server</b></span></span> |<span data-ttu-id="3751e-116">1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Nasazení virtuálních počítačů služby Microsoft Azure Průvodce tooa databáze serveru SQL</a></span><span class="sxs-lookup"><span data-stu-id="3751e-116">1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Deploy a SQL Server Database tooa Microsoft Azure VM wizard</a></span></span><br> <span data-ttu-id="3751e-117">2. <a href="#export-flat-file">Export tooa plochý soubor</a></span><span class="sxs-lookup"><span data-stu-id="3751e-117">2. <a href="#export-flat-file">Export tooa flat File </a></span></span><br> <span data-ttu-id="3751e-118">3. <a href="#sql-migration">Průvodce migrací databáze SQL</a></span><span class="sxs-lookup"><span data-stu-id="3751e-118">3. <a href="#sql-migration">SQL Database Migration Wizard </a></span></span> <br> <span data-ttu-id="3751e-119">4. <a href="#sql-backup">Databáze back up a obnovení</a></span><span class="sxs-lookup"><span data-stu-id="3751e-119">4. <a href="#sql-backup">Database back up and restore </a></span></span><br> |

<span data-ttu-id="3751e-120">Všimněte si, že tento dokument předpokládá, že příkazy SQL budou provedeny z SQL Server Management Studio nebo Visual Studio Průzkumník databáze.</span><span class="sxs-lookup"><span data-stu-id="3751e-120">Note that this document assumes that SQL commands are executed from SQL Server Management Studio or Visual Studio Database Explorer.</span></span>

> [!TIP]
> <span data-ttu-id="3751e-121">Jako alternativu, můžete použít [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) toocreate a naplánovat kanál, který se přesune data tooa virtuální počítač s SQL serverem v Azure.</span><span class="sxs-lookup"><span data-stu-id="3751e-121">As an alternative, you can use [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) toocreate and schedule a pipeline that will move data tooa SQL Server VM on Azure.</span></span> <span data-ttu-id="3751e-122">Další informace najdete v tématu [kopírování dat pomocí Azure Data Factory (aktivita kopírování)](../data-factory/data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="3751e-122">For more information, see [Copy data with Azure Data Factory (Copy Activity)](../data-factory/data-factory-data-movement-activities.md).</span></span>
>
>

## <span data-ttu-id="3751e-123"><a name="prereqs"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="3751e-123"><a name="prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="3751e-124">Tento kurz předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="3751e-124">This tutorial assumes you have:</span></span>

* <span data-ttu-id="3751e-125">**Předplatné**.</span><span class="sxs-lookup"><span data-stu-id="3751e-125">An **Azure subscription**.</span></span> <span data-ttu-id="3751e-126">Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3751e-126">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="3751e-127">**Účtu úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="3751e-127">An **Azure storage account**.</span></span> <span data-ttu-id="3751e-128">Pro ukládání dat hello v tomto kurzu budete používat účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3751e-128">You will use an Azure storage account for storing hello data in this tutorial.</span></span> <span data-ttu-id="3751e-129">Pokud nemáte účet úložiště Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) článku.</span><span class="sxs-lookup"><span data-stu-id="3751e-129">If you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article.</span></span> <span data-ttu-id="3751e-130">Po vytvoření účtu úložiště hello, budete potřebovat účet hello tooobtain, klíč používá tooaccess hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="3751e-130">After you have created hello storage account, you will need tooobtain hello account key used tooaccess hello storage.</span></span> <span data-ttu-id="3751e-131">V tématu [Správa přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="3751e-131">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>
* <span data-ttu-id="3751e-132">Zřízení **systému SQL Server ve virtuálním počítači Azure**.</span><span class="sxs-lookup"><span data-stu-id="3751e-132">Provisioned **SQL Server on an Azure VM**.</span></span> <span data-ttu-id="3751e-133">Pokyny najdete v tématu [nastavení se virtuální počítač Azure SQL Server jako server IPython Poznámkový blok pro pokročilou analýzu](machine-learning-data-science-setup-sql-server-virtual-machine.md).</span><span class="sxs-lookup"><span data-stu-id="3751e-133">For instructions, see [Set up an Azure SQL Server virtual machine as an IPython Notebook server for advanced analytics](machine-learning-data-science-setup-sql-server-virtual-machine.md).</span></span>
* <span data-ttu-id="3751e-134">Nainstalovaný a nakonfigurovaný **prostředí Azure PowerShell** místně.</span><span class="sxs-lookup"><span data-stu-id="3751e-134">Installed and configured **Azure PowerShell** locally.</span></span> <span data-ttu-id="3751e-135">Pokyny najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3751e-135">For instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <span data-ttu-id="3751e-136"><a name="filesource_to_sqlonazurevm"></a>Přesun dat z tooSQL plochý soubor zdrojového serveru na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="3751e-136"><a name="filesource_to_sqlonazurevm"></a> Moving data from a flat file source tooSQL Server on an Azure VM</span></span>
<span data-ttu-id="3751e-137">Pokud vaše data v plochý soubor (seřazeny ve formátu řádku nebo sloupce), může být přesunutý tooSQL virtuální počítač Server na platformě Azure prostřednictvím hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="3751e-137">If your data is in a flat file (arranged in a row/column format), it can be moved tooSQL Server VM on Azure via hello following methods:</span></span>

1. [<span data-ttu-id="3751e-138">Nástroj příkazového řádku pro hromadné kopírování (BCP)</span><span class="sxs-lookup"><span data-stu-id="3751e-138">Command-line bulk copy utility (BCP)</span></span>](#insert-tables-bcp)
2. [<span data-ttu-id="3751e-139">Hromadné vložení SQL dotazu</span><span class="sxs-lookup"><span data-stu-id="3751e-139">Bulk Insert SQL Query </span></span>](#insert-tables-bulkquery)
3. [<span data-ttu-id="3751e-140">Grafické nástroje integrovanou v systému SQL Server (importu a exportu, SSIS)</span><span class="sxs-lookup"><span data-stu-id="3751e-140">Graphical Built-in Utilities in SQL Server (Import/Export, SSIS)</span></span>](#sql-builtin-utilities)

### <span data-ttu-id="3751e-141"><a name="insert-tables-bcp"></a>Nástroj příkazového řádku pro hromadné kopírování (BCP)</span><span class="sxs-lookup"><span data-stu-id="3751e-141"><a name="insert-tables-bcp"></a>Command-line bulk copy utility (BCP)</span></span>
<span data-ttu-id="3751e-142">Je nainstalovaný se systémem SQL Server nástroj příkazového řádku BCP a je jedním z hello nejrychlejší způsoby toomove data.</span><span class="sxs-lookup"><span data-stu-id="3751e-142">BCP is a command-line utility installed with SQL Server and is one of hello quickest ways toomove data.</span></span> <span data-ttu-id="3751e-143">Funguje v různých všechny tři varianty systému SQL Server (místní SQL Server, SQL Azure a virtuální počítač s SQL serverem v Azure).</span><span class="sxs-lookup"><span data-stu-id="3751e-143">It works across all three SQL Server variants (On-premises SQL Server, SQL Azure and SQL Server VM on Azure).</span></span>

> [!NOTE]
> <span data-ttu-id="3751e-144">**Kam mají být data pro BCP?**</span><span class="sxs-lookup"><span data-stu-id="3751e-144">**Where should my data be for BCP?**</span></span>  
> <span data-ttu-id="3751e-145">I když není povinné, že soubory obsahující zdroje dat umístěných na stejném počítači jako hello cílového systému SQL Server umožňuje rychlejší přenosů (sítě rychlost vs místní disk vstupně-výstupní operace rychlost) hello.</span><span class="sxs-lookup"><span data-stu-id="3751e-145">While it is not required, having files containing source data located on hello same machine as hello target SQL Server allows for faster transfers (network speed vs local disk IO speed).</span></span> <span data-ttu-id="3751e-146">Můžete přesunout hello ploché soubory obsahující počítače toohello data tam, kde je nainstalován systém SQL Server pomocí různých kopírování souborů nástroje jako [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Storage Explorer](http://storageexplorer.com/) nebo windows, kopírování a vkládání přes vzdálené Protokol plochy (RDP).</span><span class="sxs-lookup"><span data-stu-id="3751e-146">You can move hello flat files containing data toohello machine where SQL Server is installed using various file copying tools such as [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Storage Explorer](http://storageexplorer.com/) or windows copy/paste via Remote Desktop Protocol (RDP).</span></span>
>
>

1. <span data-ttu-id="3751e-147">Zajistěte, aby hello databáze a tabulky hello jsou vytvořené v databázi systému SQL Server cílového hello.</span><span class="sxs-lookup"><span data-stu-id="3751e-147">Ensure that hello database and hello tables are created on hello target SQL Server database.</span></span> <span data-ttu-id="3751e-148">Tady je příklad toho, jak toodo této pomocí hello `Create Database` a `Create Table` příkazy:</span><span class="sxs-lookup"><span data-stu-id="3751e-148">Here is an example of how toodo that using hello `Create Database` and `Create Table` commands:</span></span>

        CREATE DATABASE <database_name>

        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        )
2. <span data-ttu-id="3751e-149">Generovat hello formát souboru, který popisuje hello schématu pro tabulku hello vydáním hello následující příkaz z příkazového řádku hello počítače hello nainstalovanou bcp.</span><span class="sxs-lookup"><span data-stu-id="3751e-149">Generate hello format file that describes hello schema for hello table by issuing hello following command from hello command-line of hello machine where bcp is installed.</span></span>

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
3. <span data-ttu-id="3751e-150">Vkládání dat hello do hello databáze pomocí příkazu bcp hello následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="3751e-150">Insert hello data into hello database using hello bcp command as follows.</span></span> <span data-ttu-id="3751e-151">Tento postup měl fungovat z příkazového řádku hello za předpokladu, že hello SQL Server je nainstalován ve stejném počítači:</span><span class="sxs-lookup"><span data-stu-id="3751e-151">This should work from hello command-line assuming that hello SQL Server is installed on same machine:</span></span>

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> <span data-ttu-id="3751e-152">**Optimalizace BCP vloží** naleznete hello následujícího článku ["Pokyny pro optimalizaci hromadným importem"](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) toooptimize takové vloží.</span><span class="sxs-lookup"><span data-stu-id="3751e-152">**Optimizing BCP Inserts** Please refer hello following article ['Guidelines for Optimizing Bulk Import'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) toooptimize such inserts.</span></span>
>
>

### <span data-ttu-id="3751e-153"><a name="insert-tables-bulkquery-parallel"></a>Paralelním prováděním vložení pro rychlejší přesun dat</span><span class="sxs-lookup"><span data-stu-id="3751e-153"><a name="insert-tables-bulkquery-parallel"></a>Parallelizing Inserts for Faster Data Movement</span></span>
<span data-ttu-id="3751e-154">Pokud přesouváte data hello velká, můžete urychlit věcí současně spuštěním více BCP příkazů paralelně ve skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3751e-154">If hello data you are moving is large, you can speed things up by simultaneously executing multiple BCP commands in parallel in a PowerShell Script.</span></span>

> [!NOTE]
> <span data-ttu-id="3751e-155">**Velké objemy dat přijímání** načítání pro velké a velké datové sady dat toooptimize oddílu logické a fyzické databázových tabulek pomocí více skupin souborů a oddíl tabulky.</span><span class="sxs-lookup"><span data-stu-id="3751e-155">**Big data Ingestion** toooptimize data loading for large and very large datasets, partition your logical and physical database tables using multiple filegroups and partition tables.</span></span> <span data-ttu-id="3751e-156">Další informace o vytváření a načítání dat tabulky toopartition najdete v tématu [paralelní tabulky oddílů SQL zatížení](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="3751e-156">For more information about creating and loading data toopartition tables, see [Parallel Load SQL Partition Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
>
>

<span data-ttu-id="3751e-157">Hello ukázkový skript prostředí PowerShell, které jsou níže ukazují paralelní vloží při použití nástroje bcp:</span><span class="sxs-lookup"><span data-stu-id="3751e-157">hello sample PowerShell script below demonstrate parallel inserts using bcp:</span></span>

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


### <span data-ttu-id="3751e-158"><a name="insert-tables-bulkquery"></a>Hromadné vložení SQL dotazu</span><span class="sxs-lookup"><span data-stu-id="3751e-158"><a name="insert-tables-bulkquery"></a>Bulk Insert SQL Query</span></span>
<span data-ttu-id="3751e-159">[Hromadné vložení dotazu SQL](https://msdn.microsoft.com/library/ms188365) lze použít tooimport data do databáze hello z řádku nebo sloupce na základě souborů (hello podporované typy jsou zahrnuté v[Příprava Data pro hromadné exportu nebo importu (SQL Server)](https://msdn.microsoft.com/library/ms188609)) tématu.</span><span class="sxs-lookup"><span data-stu-id="3751e-159">[Bulk Insert SQL Query](https://msdn.microsoft.com/library/ms188365) can be used tooimport data into hello database from row/column based files (hello supported types are covered in the[Prepare Data for Bulk Export or Import (SQL Server)](https://msdn.microsoft.com/library/ms188609)) topic.</span></span>

<span data-ttu-id="3751e-160">Tady jsou některé ukázkové příkazy pro hromadné vložení se, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="3751e-160">Here are some sample commands for Bulk Insert are as below:</span></span>  

1. <span data-ttu-id="3751e-161">Analyzovat data a nastavte všechny vlastní možnosti před importem toomake se, že se že předpokládá, že databáze systému SQL Server hello hello stejný formát pro všechny speciální pole, jako je například kalendářní data.</span><span class="sxs-lookup"><span data-stu-id="3751e-161">Analyze your data and set any custom options before importing toomake sure that hello SQL Server database assumes hello same format for any special fields such as dates.</span></span> <span data-ttu-id="3751e-162">Tady je příklad, jak tooset hello datum formátovat jako rok měsíc den (Pokud data obsahují hello datum ve formátu rok měsíc den):</span><span class="sxs-lookup"><span data-stu-id="3751e-162">Here is an example of how tooset hello date format as year-month-day (if your data contains hello date in year-month-day format):</span></span>

        SET DATEFORMAT ymd;    
2. <span data-ttu-id="3751e-163">Importujte dat pomocí hromadného importu příkazy:</span><span class="sxs-lookup"><span data-stu-id="3751e-163">Import data using bulk import statements:</span></span>

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be hello row separator in your data
        )

### <span data-ttu-id="3751e-164"><a name="sql-builtin-utilities"></a>Integrované nástroje SQL serveru</span><span class="sxs-lookup"><span data-stu-id="3751e-164"><a name="sql-builtin-utilities"></a>Built-in Utilities in SQL Server</span></span>
<span data-ttu-id="3751e-165">Integrace služby SSIS (SQL Server) tooimport data můžete použít na virtuální počítač s SQL serverem na Azure z plochého souboru.</span><span class="sxs-lookup"><span data-stu-id="3751e-165">You can use SQL Server Integrations Services (SSIS) tooimport data into SQL Server VM on Azure from a flat file.</span></span>
<span data-ttu-id="3751e-166">Služby SSIS je k dispozici ve dvou studio prostředí.</span><span class="sxs-lookup"><span data-stu-id="3751e-166">SSIS is available in two studio environments.</span></span> <span data-ttu-id="3751e-167">Podrobnosti najdete v tématu [Integration Services (SSIS) a prostředí Studio](https://technet.microsoft.com/library/ms140028.aspx):</span><span class="sxs-lookup"><span data-stu-id="3751e-167">For details, see [Integration Services (SSIS) and Studio Environments](https://technet.microsoft.com/library/ms140028.aspx):</span></span>

* <span data-ttu-id="3751e-168">Podrobnosti na SQL Server Data Tools najdete v tématu [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)</span><span class="sxs-lookup"><span data-stu-id="3751e-168">For details on SQL Server Data Tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)</span></span>  
* <span data-ttu-id="3751e-169">Podrobnosti na hello Průvodce importem a exportem najdete v tématu [Microsoft SQL Server importovat a exportovat Průvodce](https://msdn.microsoft.com/library/ms141209.aspx)</span><span class="sxs-lookup"><span data-stu-id="3751e-169">For details on hello Import/Export Wizard, see [SQL Server Import and Export Wizard](https://msdn.microsoft.com/library/ms141209.aspx)</span></span>

## <span data-ttu-id="3751e-170"><a name="sqlonprem_to_sqlonazurevm"></a>Přesun dat z tooSQL místní SQL Server Server na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="3751e-170"><a name="sqlonprem_to_sqlonazurevm"></a>Moving Data from on-premises SQL Server tooSQL Server on an Azure VM</span></span>
<span data-ttu-id="3751e-171">Můžete také použít hello následující strategie migrace:</span><span class="sxs-lookup"><span data-stu-id="3751e-171">You can also use hello following migration strategies:</span></span>

1. [<span data-ttu-id="3751e-172">Nasazení virtuálních počítačů služby Microsoft Azure Průvodce tooa databáze serveru SQL</span><span class="sxs-lookup"><span data-stu-id="3751e-172">Deploy a SQL Server Database tooa Microsoft Azure VM wizard</span></span>](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [<span data-ttu-id="3751e-173">Export tooFlat souboru</span><span class="sxs-lookup"><span data-stu-id="3751e-173">Export tooFlat File</span></span>](#export-flat-file)
3. [<span data-ttu-id="3751e-174">Průvodce migrací databáze SQL</span><span class="sxs-lookup"><span data-stu-id="3751e-174">SQL Database Migration Wizard</span></span>](#sql-migration)
4. [<span data-ttu-id="3751e-175">Databáze back up a obnovení</span><span class="sxs-lookup"><span data-stu-id="3751e-175">Database back up and restore</span></span>](#sql-backup)

<span data-ttu-id="3751e-176">Jsme popisují každou z těchto níže:</span><span class="sxs-lookup"><span data-stu-id="3751e-176">We describe each of these below:</span></span>

### <a name="deploy-a-sql-server-database-tooa-microsoft-azure-vm-wizard"></a><span data-ttu-id="3751e-177">Nasazení virtuálních počítačů služby Microsoft Azure Průvodce tooa databáze serveru SQL</span><span class="sxs-lookup"><span data-stu-id="3751e-177">Deploy a SQL Server Database tooa Microsoft Azure VM wizard</span></span>
<span data-ttu-id="3751e-178">Hello **nasazení virtuálních počítačů služby Microsoft Azure Průvodce tooa databáze systému SQL Server** je jednoduchý a doporučený způsob toomove dat z tooSQL instance systému SQL Server na místní Server na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="3751e-178">hello **Deploy a SQL Server Database tooa Microsoft Azure VM wizard** is a simple and recommended way toomove data from an on-premises SQL Server instance tooSQL Server on an Azure VM.</span></span> <span data-ttu-id="3751e-179">Podrobné kroky a také se zabývat náhradních řešení, najdete v části [migraci databáze tooSQL Server na virtuálním počítači Azure](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).</span><span class="sxs-lookup"><span data-stu-id="3751e-179">For detailed steps as well as a discussion of other alternatives, see [Migrate a database tooSQL Server on an Azure VM](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).</span></span>

### <span data-ttu-id="3751e-180"><a name="export-flat-file"></a>Export tooFlat souboru</span><span class="sxs-lookup"><span data-stu-id="3751e-180"><a name="export-flat-file"></a>Export tooFlat File</span></span>
<span data-ttu-id="3751e-181">Různé metody lze použít toobulk exportu dat z místního serveru SQL, jak je uvedeno v hello [hromadný Import a Export dat (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="3751e-181">Various methods can be used toobulk export data from an On-Premises SQL Server as documented in hello [Bulk Import and Export of Data (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) topic.</span></span> <span data-ttu-id="3751e-182">Tento dokument se týká hello hromadné kopírování programu (BCP) jako příklad.</span><span class="sxs-lookup"><span data-stu-id="3751e-182">This document will cover hello Bulk Copy Program (BCP) as an example.</span></span> <span data-ttu-id="3751e-183">Jakmile data se exportují do plochého souboru, může být importované tooanother systému SQL server pomocí hromadného importu.</span><span class="sxs-lookup"><span data-stu-id="3751e-183">Once data is exported into a flat file, it can be imported tooanother SQL server using bulk import.</span></span>

1. <span data-ttu-id="3751e-184">Exportovat hello data z místního serveru SQL Server tooa souboru pomocí nástroj bcp hello takto</span><span class="sxs-lookup"><span data-stu-id="3751e-184">Export hello data from on-premises SQL Server tooa File using hello bcp utility as follows</span></span>

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. <span data-ttu-id="3751e-185">Vytvoření hello databáze a tabulky hello na virtuální počítač s SQL serverem v Azure pomocí hello `create database` a `create table` pro schéma tabulky hello exportovali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="3751e-185">Create hello database and hello table on SQL Server VM on Azure using hello `create database` and `create table` for hello table schema exported in step 1.</span></span>
3. <span data-ttu-id="3751e-186">Vytvořte soubor ve formátu pro popisující hello schématu tabulky dat hello se exportovat nebo importovat.</span><span class="sxs-lookup"><span data-stu-id="3751e-186">Create a format file for describing hello table schema of hello data being exported/imported.</span></span> <span data-ttu-id="3751e-187">Podrobnosti o hello formátového souboru jsou popsané v [vytvořit soubor ve formátu (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).</span><span class="sxs-lookup"><span data-stu-id="3751e-187">Details of hello format file are described in [Create a Format File (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).</span></span>

    <span data-ttu-id="3751e-188">Generování souboru formátu při spuštění BCP z počítače systému SQL Server hello</span><span class="sxs-lookup"><span data-stu-id="3751e-188">Format file generation when running BCP from hello SQL Server machine</span></span>

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    <span data-ttu-id="3751e-189">Generování souboru formátu při spuštění BCP vzdáleně s SQL serverem</span><span class="sxs-lookup"><span data-stu-id="3751e-189">Format file generation when running BCP remotely against a SQL Server</span></span>

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. <span data-ttu-id="3751e-190">Použít některou z metod hello popsané v části [přesun dat ze souboru zdroje](#filesource_to_sqlonazurevm) toomove hello data v tooa plochých souborů systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3751e-190">Use any of hello methods described in section [Moving Data from File Source](#filesource_to_sqlonazurevm) toomove hello data in flat files tooa SQL Server.</span></span>

### <span data-ttu-id="3751e-191"><a name="sql-migration"></a>Průvodce migrací databáze SQL</span><span class="sxs-lookup"><span data-stu-id="3751e-191"><a name="sql-migration"></a>SQL Database Migration Wizard</span></span>
<span data-ttu-id="3751e-192">[Průvodce migrací databáze SQL serveru](http://sqlazuremw.codeplex.com/) obsahuje uživatelsky přívětivé toomove dat mezi dvě instance serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="3751e-192">[SQL Server Database Migration Wizard](http://sqlazuremw.codeplex.com/) provides a user-friendly way toomove data between two SQL server instances.</span></span> <span data-ttu-id="3751e-193">Umožňuje schéma dat hello uživatele toomap hello mezi zdroji a cílové tabulky, vyberte typy sloupců a různé další funkce.</span><span class="sxs-lookup"><span data-stu-id="3751e-193">It allows hello user toomap hello data schema between sources and destination tables, choose column types and various other functionalities.</span></span> <span data-ttu-id="3751e-194">V části zahrnuje hello používá hromadné kopírování (BCP).</span><span class="sxs-lookup"><span data-stu-id="3751e-194">It uses bulk copy (BCP) under hello covers.</span></span> <span data-ttu-id="3751e-195">Snímek obrazovky hello úvodní obrazovka pro hello migraci databáze SQL průvodce jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="3751e-195">A screenshot of hello welcome screen for hello SQL Database Migration wizard is shown below.</span></span>  

![Průvodce migrací serveru SQL][2]

### <span data-ttu-id="3751e-197"><a name="sql-backup"></a>Databáze back up a obnovení</span><span class="sxs-lookup"><span data-stu-id="3751e-197"><a name="sql-backup"></a>Database back up and restore</span></span>
<span data-ttu-id="3751e-198">Systém SQL Server podporuje:</span><span class="sxs-lookup"><span data-stu-id="3751e-198">SQL Server supports:</span></span>

1. <span data-ttu-id="3751e-199">[Zálohu databáze zpět a obnovit funkčnost](https://msdn.microsoft.com/library/ms187048.aspx) (obě tooa místního souboru nebo souboru bacpac export tooblob) a [aplikace datové vrstvy](https://msdn.microsoft.com/library/ee210546.aspx) (pomocí souboru bacpac).</span><span class="sxs-lookup"><span data-stu-id="3751e-199">[Database back up and restore functionality](https://msdn.microsoft.com/library/ms187048.aspx) (both tooa local file or bacpac export tooblob) and [Data Tier Applications](https://msdn.microsoft.com/library/ee210546.aspx) (using bacpac).</span></span>
2. <span data-ttu-id="3751e-200">Možnost toodirectly vytvořit virtuálním počítačům systému SQL Server na platformě Azure zkopírovaný databáze nebo kopírování tooan existující databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="3751e-200">Ability toodirectly create SQL Server VMs on Azure with a copied database or copy tooan existing SQL Azure database.</span></span> <span data-ttu-id="3751e-201">Další podrobnosti najdete v tématu [hello pomocí Průvodce kopírováním databáze](https://msdn.microsoft.com/library/ms188664.aspx).</span><span class="sxs-lookup"><span data-stu-id="3751e-201">For more details, see [Use hello Copy Database Wizard](https://msdn.microsoft.com/library/ms188664.aspx).</span></span>

<span data-ttu-id="3751e-202">Snímek obrazovky hello databáze zpět nebo obnovit možnosti z SQL Server Management Studio jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="3751e-202">A screenshot of hello Database back up/restore options from SQL Server Management Studio is shown below.</span></span>

![Nástroj pro Import serveru SQL][1]

## <a name="resources"></a><span data-ttu-id="3751e-204">Zdroje</span><span class="sxs-lookup"><span data-stu-id="3751e-204">Resources</span></span>
[<span data-ttu-id="3751e-205">Migrace databáze tooSQL Server na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="3751e-205">Migrate a Database tooSQL Server on an Azure VM</span></span>](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[<span data-ttu-id="3751e-206">SQL Server na Azure Virtual Machines – přehled</span><span class="sxs-lookup"><span data-stu-id="3751e-206">SQL Server on Azure Virtual Machines overview</span></span>](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
