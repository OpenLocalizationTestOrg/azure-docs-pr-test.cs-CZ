---
title: "Přesun dat do systému SQL Server na virtuální počítač Azure | Microsoft Docs"
description: "Přesun dat z plochých souborů nebo z místního serveru SQL do systému SQL Server na virtuálním počítači Azure."
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
ms.openlocfilehash: aa0cbba6bdb4cfdfe6ceee50c94f706aa0974924
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a><span data-ttu-id="34a2b-103">Přesun dat do SQL Serveru na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="34a2b-103">Move data to SQL Server on an Azure virtual machine</span></span>
<span data-ttu-id="34a2b-104">Toto téma popisuje možnosti pro přesun dat z plochých souborů (formáty CSV nebo TSV) nebo z místního serveru SQL do systému SQL Server na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="34a2b-104">This topic outlines the options for moving data either from flat files (CSV or TSV formats) or from an on-premises SQL Server to SQL Server on an Azure virtual machine.</span></span> <span data-ttu-id="34a2b-105">Tyto úlohy pro přesun dat do cloudu jsou součástí procesu Team dat vědecké účely.</span><span class="sxs-lookup"><span data-stu-id="34a2b-105">These tasks for moving data to the cloud are part of the Team Data Science Process.</span></span>

<span data-ttu-id="34a2b-106">Téma, které popisuje možnosti pro přesun dat do Azure SQL Database pro Machine Learning, najdete v části [přesun dat do Azure SQL Database pro Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span><span class="sxs-lookup"><span data-stu-id="34a2b-106">For a topic that outlines the options for moving data to an Azure SQL Database for Machine Learning, see [Move data to an Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span></span>

<span data-ttu-id="34a2b-107">**Nabídky** pod odkazy na témata, které popisují, jak ingestovat data do jiných cíl prostředích, kde můžete data ukládat a zpracovávat během tým datové vědy procesu (TDSP).</span><span class="sxs-lookup"><span data-stu-id="34a2b-107">The **menu** below links to topics that describe how to ingest data into other target environments where the data can be stored and processed during the Team Data Science Process (TDSP).</span></span>

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<span data-ttu-id="34a2b-108">Následující tabulka shrnuje možnosti pro přesun dat do systému SQL Server na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="34a2b-108">The following table summarizes the options for moving data to SQL Server on an Azure virtual machine.</span></span>

| <span data-ttu-id="34a2b-109"><b>ZDROJ</b></span><span class="sxs-lookup"><span data-stu-id="34a2b-109"><b>SOURCE</b></span></span> | <span data-ttu-id="34a2b-110"><b>Cíl: SQL Server na virtuální počítač Azure</b></span><span class="sxs-lookup"><span data-stu-id="34a2b-110"><b>DESTINATION: SQL Server on Azure VM</b></span></span> |
| --- | --- |
| <span data-ttu-id="34a2b-111"><b>Plochý soubor</b></span><span class="sxs-lookup"><span data-stu-id="34a2b-111"><b>Flat File</b></span></span> |<span data-ttu-id="34a2b-112">1. <a href="#insert-tables-bcp">Nástroj příkazového řádku pro hromadné kopírování (BCP)</a></span><span class="sxs-lookup"><span data-stu-id="34a2b-112">1. <a href="#insert-tables-bcp">Command-line bulk copy utility (BCP) </a></span></span><br> <span data-ttu-id="34a2b-113">2. <a href="#insert-tables-bulkquery">Hromadné vložení SQL dotazu</a></span><span class="sxs-lookup"><span data-stu-id="34a2b-113">2. <a href="#insert-tables-bulkquery">Bulk Insert SQL Query </a></span></span><br> <span data-ttu-id="34a2b-114">3. <a href="#sql-builtin-utilities">Grafické nástroje integrovanou v systému SQL Server</a></span><span class="sxs-lookup"><span data-stu-id="34a2b-114">3. <a href="#sql-builtin-utilities">Graphical Built-in Utilities in SQL Server</a></span></span> |
| <span data-ttu-id="34a2b-115"><b>Místní SQL Server</b></span><span class="sxs-lookup"><span data-stu-id="34a2b-115"><b>On-Premises SQL Server</b></span></span> |<span data-ttu-id="34a2b-116">1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Nasazení databáze systému SQL Server do Průvodce virtuálních počítačů služby Microsoft Azure</a></span><span class="sxs-lookup"><span data-stu-id="34a2b-116">1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Deploy a SQL Server Database to a Microsoft Azure VM wizard</a></span></span><br> <span data-ttu-id="34a2b-117">2. <a href="#export-flat-file">Exportovat do plochý soubor</a></span><span class="sxs-lookup"><span data-stu-id="34a2b-117">2. <a href="#export-flat-file">Export to a flat File </a></span></span><br> <span data-ttu-id="34a2b-118">3. <a href="#sql-migration">Průvodce migrací databáze SQL</a></span><span class="sxs-lookup"><span data-stu-id="34a2b-118">3. <a href="#sql-migration">SQL Database Migration Wizard </a></span></span> <br> <span data-ttu-id="34a2b-119">4. <a href="#sql-backup">Databáze back up a obnovení</a></span><span class="sxs-lookup"><span data-stu-id="34a2b-119">4. <a href="#sql-backup">Database back up and restore </a></span></span><br> |

<span data-ttu-id="34a2b-120">Všimněte si, že tento dokument předpokládá, že příkazy SQL budou provedeny z SQL Server Management Studio nebo Visual Studio Průzkumník databáze.</span><span class="sxs-lookup"><span data-stu-id="34a2b-120">Note that this document assumes that SQL commands are executed from SQL Server Management Studio or Visual Studio Database Explorer.</span></span>

> [!TIP]
> <span data-ttu-id="34a2b-121">Jako alternativu, můžete použít [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) k vytváření a plánování kanál, který bude přesouvat data virtuálního počítače s SQL Server na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="34a2b-121">As an alternative, you can use [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) to create and schedule a pipeline that will move data to a SQL Server VM on Azure.</span></span> <span data-ttu-id="34a2b-122">Další informace najdete v tématu [kopírování dat pomocí Azure Data Factory (aktivita kopírování)](../data-factory/data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="34a2b-122">For more information, see [Copy data with Azure Data Factory (Copy Activity)](../data-factory/data-factory-data-movement-activities.md).</span></span>
>
>

## <span data-ttu-id="34a2b-123"><a name="prereqs"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="34a2b-123"><a name="prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="34a2b-124">Tento kurz předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="34a2b-124">This tutorial assumes you have:</span></span>

* <span data-ttu-id="34a2b-125">**Předplatné**.</span><span class="sxs-lookup"><span data-stu-id="34a2b-125">An **Azure subscription**.</span></span> <span data-ttu-id="34a2b-126">Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="34a2b-126">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="34a2b-127">**Účtu úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="34a2b-127">An **Azure storage account**.</span></span> <span data-ttu-id="34a2b-128">Pro ukládání dat v tomto kurzu budete používat účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="34a2b-128">You will use an Azure storage account for storing the data in this tutorial.</span></span> <span data-ttu-id="34a2b-129">Pokud nemáte účet úložiště Azure, přečtěte si článek [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="34a2b-129">If you don't have an Azure storage account, see the [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article.</span></span> <span data-ttu-id="34a2b-130">Po vytvoření účtu úložiště, musíte získat klíč účtu, který používá pro přístup k úložišti.</span><span class="sxs-lookup"><span data-stu-id="34a2b-130">After you have created the storage account, you will need to obtain the account key used to access the storage.</span></span> <span data-ttu-id="34a2b-131">V tématu [Správa přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="34a2b-131">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>
* <span data-ttu-id="34a2b-132">Zřízení **systému SQL Server ve virtuálním počítači Azure**.</span><span class="sxs-lookup"><span data-stu-id="34a2b-132">Provisioned **SQL Server on an Azure VM**.</span></span> <span data-ttu-id="34a2b-133">Pokyny najdete v tématu [nastavení se virtuální počítač Azure SQL Server jako server IPython Poznámkový blok pro pokročilou analýzu](machine-learning-data-science-setup-sql-server-virtual-machine.md).</span><span class="sxs-lookup"><span data-stu-id="34a2b-133">For instructions, see [Set up an Azure SQL Server virtual machine as an IPython Notebook server for advanced analytics](machine-learning-data-science-setup-sql-server-virtual-machine.md).</span></span>
* <span data-ttu-id="34a2b-134">Nainstalovaný a nakonfigurovaný **prostředí Azure PowerShell** místně.</span><span class="sxs-lookup"><span data-stu-id="34a2b-134">Installed and configured **Azure PowerShell** locally.</span></span> <span data-ttu-id="34a2b-135">Pokyny najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="34a2b-135">For instructions, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <span data-ttu-id="34a2b-136"><a name="filesource_to_sqlonazurevm"></a>Přesun dat z plochých souborů zdroje k systému SQL Server na virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="34a2b-136"><a name="filesource_to_sqlonazurevm"></a> Moving data from a flat file source to SQL Server on an Azure VM</span></span>
<span data-ttu-id="34a2b-137">Pokud vaše data v plochý soubor (seřazeny ve formátu řádku nebo sloupce), se můžete přesunout na virtuální počítač SQL Server na platformě Azure pomocí následujících metod:</span><span class="sxs-lookup"><span data-stu-id="34a2b-137">If your data is in a flat file (arranged in a row/column format), it can be moved to SQL Server VM on Azure via the following methods:</span></span>

1. [<span data-ttu-id="34a2b-138">Nástroj příkazového řádku pro hromadné kopírování (BCP)</span><span class="sxs-lookup"><span data-stu-id="34a2b-138">Command-line bulk copy utility (BCP)</span></span>](#insert-tables-bcp)
2. [<span data-ttu-id="34a2b-139">Hromadné vložení SQL dotazu</span><span class="sxs-lookup"><span data-stu-id="34a2b-139">Bulk Insert SQL Query </span></span>](#insert-tables-bulkquery)
3. [<span data-ttu-id="34a2b-140">Grafické nástroje integrovanou v systému SQL Server (importu a exportu, SSIS)</span><span class="sxs-lookup"><span data-stu-id="34a2b-140">Graphical Built-in Utilities in SQL Server (Import/Export, SSIS)</span></span>](#sql-builtin-utilities)

### <span data-ttu-id="34a2b-141"><a name="insert-tables-bcp"></a>Nástroj příkazového řádku pro hromadné kopírování (BCP)</span><span class="sxs-lookup"><span data-stu-id="34a2b-141"><a name="insert-tables-bcp"></a>Command-line bulk copy utility (BCP)</span></span>
<span data-ttu-id="34a2b-142">Je nainstalovaný se systémem SQL Server nástroj příkazového řádku BCP a je nejrychlejší způsob pro přesun dat.</span><span class="sxs-lookup"><span data-stu-id="34a2b-142">BCP is a command-line utility installed with SQL Server and is one of the quickest ways to move data.</span></span> <span data-ttu-id="34a2b-143">Funguje v různých všechny tři varianty systému SQL Server (místní SQL Server, SQL Azure a virtuální počítač s SQL serverem v Azure).</span><span class="sxs-lookup"><span data-stu-id="34a2b-143">It works across all three SQL Server variants (On-premises SQL Server, SQL Azure and SQL Server VM on Azure).</span></span>

> [!NOTE]
> <span data-ttu-id="34a2b-144">**Kam mají být data pro BCP?**</span><span class="sxs-lookup"><span data-stu-id="34a2b-144">**Where should my data be for BCP?**</span></span>  
> <span data-ttu-id="34a2b-145">Když to není nutné, s soubory obsahující zdroje dat umístěných na stejném počítači jako cíl systému SQL Server umožňuje rychlejší přenosů (sítě rychlost vs místní disk vstupně-výstupní operace rychlost).</span><span class="sxs-lookup"><span data-stu-id="34a2b-145">While it is not required, having files containing source data located on the same machine as the target SQL Server allows for faster transfers (network speed vs local disk IO speed).</span></span> <span data-ttu-id="34a2b-146">Ploché soubory obsahující data do počítače můžete přesunout tam, kde je nainstalován systém SQL Server pomocí různých kopírování souborů nástroje jako [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Storage Explorer](http://storageexplorer.com/) nebo windows, kopírování a vkládání přes vzdálenou plochu Protokol (RDP).</span><span class="sxs-lookup"><span data-stu-id="34a2b-146">You can move the flat files containing data to the machine where SQL Server is installed using various file copying tools such as [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Storage Explorer](http://storageexplorer.com/) or windows copy/paste via Remote Desktop Protocol (RDP).</span></span>
>
>

1. <span data-ttu-id="34a2b-147">Ujistěte se, že databáze a tabulky jsou vytvořeny v cílové databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="34a2b-147">Ensure that the database and the tables are created on the target SQL Server database.</span></span> <span data-ttu-id="34a2b-148">Tady je příklad, jak to provést, že pomocí `Create Database` a `Create Table` příkazy:</span><span class="sxs-lookup"><span data-stu-id="34a2b-148">Here is an example of how to do that using the `Create Database` and `Create Table` commands:</span></span>

        CREATE DATABASE <database_name>

        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        )
2. <span data-ttu-id="34a2b-149">Generujte soubor formátu, který popisuje schéma pro tabulku po vydání následujícího příkazu z příkazového řádku na počítači nainstalovanou bcp.</span><span class="sxs-lookup"><span data-stu-id="34a2b-149">Generate the format file that describes the schema for the table by issuing the following command from the command-line of the machine where bcp is installed.</span></span>

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
3. <span data-ttu-id="34a2b-150">Vložení dat do databáze pomocí příkazu bcp následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="34a2b-150">Insert the data into the database using the bcp command as follows.</span></span> <span data-ttu-id="34a2b-151">Tento postup měl fungovat z příkazového řádku za předpokladu, že SQL Server je nainstalován ve stejném počítači:</span><span class="sxs-lookup"><span data-stu-id="34a2b-151">This should work from the command-line assuming that the SQL Server is installed on same machine:</span></span>

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> <span data-ttu-id="34a2b-152">**Optimalizace BCP vloží** naleznete v následujícím článku ["Pokyny pro optimalizaci hromadným importem"](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) za účelem optimalizace takové vložení.</span><span class="sxs-lookup"><span data-stu-id="34a2b-152">**Optimizing BCP Inserts** Please refer the following article ['Guidelines for Optimizing Bulk Import'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) to optimize such inserts.</span></span>
>
>

### <span data-ttu-id="34a2b-153"><a name="insert-tables-bulkquery-parallel"></a>Paralelním prováděním vložení pro rychlejší přesun dat</span><span class="sxs-lookup"><span data-stu-id="34a2b-153"><a name="insert-tables-bulkquery-parallel"></a>Parallelizing Inserts for Faster Data Movement</span></span>
<span data-ttu-id="34a2b-154">Pokud přesouváte data velká, můžete urychlit věcí současně spuštěním více BCP příkazů paralelně ve skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="34a2b-154">If the data you are moving is large, you can speed things up by simultaneously executing multiple BCP commands in parallel in a PowerShell Script.</span></span>

> [!NOTE]
> <span data-ttu-id="34a2b-155">**Velké objemy dat přijímání** za účelem optimalizace načítání pro velké a velké datové sady dat, oddílu logické a fyzické databázových tabulek pomocí více skupin souborů a oddíl tabulky.</span><span class="sxs-lookup"><span data-stu-id="34a2b-155">**Big data Ingestion** To optimize data loading for large and very large datasets, partition your logical and physical database tables using multiple filegroups and partition tables.</span></span> <span data-ttu-id="34a2b-156">Další informace o vytvoření a načtení dat do oddílu tabulky najdete v tématu [paralelní tabulky oddílů SQL zatížení](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="34a2b-156">For more information about creating and loading data to partition tables, see [Parallel Load SQL Partition Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
>
>

<span data-ttu-id="34a2b-157">Ukázkový skript prostředí PowerShell níže ukazují paralelní vloží při použití nástroje bcp:</span><span class="sxs-lookup"><span data-stu-id="34a2b-157">The sample PowerShell script below demonstrate parallel inserts using bcp:</span></span>

    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
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


    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }

    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <span data-ttu-id="34a2b-158"><a name="insert-tables-bulkquery"></a>Hromadné vložení SQL dotazu</span><span class="sxs-lookup"><span data-stu-id="34a2b-158"><a name="insert-tables-bulkquery"></a>Bulk Insert SQL Query</span></span>
<span data-ttu-id="34a2b-159">[Hromadné vložení dotazu SQL](https://msdn.microsoft.com/library/ms188365) lze použít k importu dat do databáze z řádku nebo sloupce na základě souborů (podporované typy jsou popsané v[Příprava Data pro hromadné exportu nebo importu (SQL Server)](https://msdn.microsoft.com/library/ms188609)) tématu.</span><span class="sxs-lookup"><span data-stu-id="34a2b-159">[Bulk Insert SQL Query](https://msdn.microsoft.com/library/ms188365) can be used to import data into the database from row/column based files (the supported types are covered in the[Prepare Data for Bulk Export or Import (SQL Server)](https://msdn.microsoft.com/library/ms188609)) topic.</span></span>

<span data-ttu-id="34a2b-160">Tady jsou některé ukázkové příkazy pro hromadné vložení se, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="34a2b-160">Here are some sample commands for Bulk Insert are as below:</span></span>  

1. <span data-ttu-id="34a2b-161">Analyzovat data a nastavte všechny vlastní možnosti před importem a ujistěte se, že databázi systému SQL Server předpokládá stejný formát pro všechny speciální pole, jako je například kalendářní data.</span><span class="sxs-lookup"><span data-stu-id="34a2b-161">Analyze your data and set any custom options before importing to make sure that the SQL Server database assumes the same format for any special fields such as dates.</span></span> <span data-ttu-id="34a2b-162">Tady je příklad toho, jak nastavit formát data jako rok měsíc den (Pokud data obsahují datum ve formátu rok měsíc den):</span><span class="sxs-lookup"><span data-stu-id="34a2b-162">Here is an example of how to set the date format as year-month-day (if your data contains the date in year-month-day format):</span></span>

        SET DATEFORMAT ymd;    
2. <span data-ttu-id="34a2b-163">Importujte dat pomocí hromadného importu příkazy:</span><span class="sxs-lookup"><span data-stu-id="34a2b-163">Import data using bulk import statements:</span></span>

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )

### <span data-ttu-id="34a2b-164"><a name="sql-builtin-utilities"></a>Integrované nástroje SQL serveru</span><span class="sxs-lookup"><span data-stu-id="34a2b-164"><a name="sql-builtin-utilities"></a>Built-in Utilities in SQL Server</span></span>
<span data-ttu-id="34a2b-165">Integrace služby SSIS (SQL Server) můžete použít k importu dat do virtuální počítač s SQL serverem na Azure z plochého souboru.</span><span class="sxs-lookup"><span data-stu-id="34a2b-165">You can use SQL Server Integrations Services (SSIS) to import data into SQL Server VM on Azure from a flat file.</span></span>
<span data-ttu-id="34a2b-166">Služby SSIS je k dispozici ve dvou studio prostředí.</span><span class="sxs-lookup"><span data-stu-id="34a2b-166">SSIS is available in two studio environments.</span></span> <span data-ttu-id="34a2b-167">Podrobnosti najdete v tématu [Integration Services (SSIS) a prostředí Studio](https://technet.microsoft.com/library/ms140028.aspx):</span><span class="sxs-lookup"><span data-stu-id="34a2b-167">For details, see [Integration Services (SSIS) and Studio Environments](https://technet.microsoft.com/library/ms140028.aspx):</span></span>

* <span data-ttu-id="34a2b-168">Podrobnosti na SQL Server Data Tools najdete v tématu [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)</span><span class="sxs-lookup"><span data-stu-id="34a2b-168">For details on SQL Server Data Tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)</span></span>  
* <span data-ttu-id="34a2b-169">Informace o Průvodci importem a exportem v [Microsoft SQL Server importovat a exportovat Průvodce](https://msdn.microsoft.com/library/ms141209.aspx)</span><span class="sxs-lookup"><span data-stu-id="34a2b-169">For details on the Import/Export Wizard, see [SQL Server Import and Export Wizard](https://msdn.microsoft.com/library/ms141209.aspx)</span></span>

## <span data-ttu-id="34a2b-170"><a name="sqlonprem_to_sqlonazurevm"></a>Přesun dat z místního SQL serveru do systému SQL Server ve virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="34a2b-170"><a name="sqlonprem_to_sqlonazurevm"></a>Moving Data from on-premises SQL Server to SQL Server on an Azure VM</span></span>
<span data-ttu-id="34a2b-171">Můžete také použít následující strategie migrace:</span><span class="sxs-lookup"><span data-stu-id="34a2b-171">You can also use the following migration strategies:</span></span>

1. [<span data-ttu-id="34a2b-172">Nasazení databáze systému SQL Server do Průvodce virtuálních počítačů služby Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="34a2b-172">Deploy a SQL Server Database to a Microsoft Azure VM wizard</span></span>](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [<span data-ttu-id="34a2b-173">Exportovat do plochý soubor</span><span class="sxs-lookup"><span data-stu-id="34a2b-173">Export to Flat File</span></span>](#export-flat-file)
3. [<span data-ttu-id="34a2b-174">Průvodce migrací databáze SQL</span><span class="sxs-lookup"><span data-stu-id="34a2b-174">SQL Database Migration Wizard</span></span>](#sql-migration)
4. [<span data-ttu-id="34a2b-175">Databáze back up a obnovení</span><span class="sxs-lookup"><span data-stu-id="34a2b-175">Database back up and restore</span></span>](#sql-backup)

<span data-ttu-id="34a2b-176">Jsme popisují každou z těchto níže:</span><span class="sxs-lookup"><span data-stu-id="34a2b-176">We describe each of these below:</span></span>

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a><span data-ttu-id="34a2b-177">Nasazení databáze systému SQL Server do Průvodce virtuálních počítačů služby Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="34a2b-177">Deploy a SQL Server Database to a Microsoft Azure VM wizard</span></span>
<span data-ttu-id="34a2b-178">**Nasazení databáze systému SQL Server do virtuálního počítače Microsoft Azure průvodce** je jednoduchý a doporučený způsob pro přesun dat z místní instance systému SQL Server do systému SQL Server na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="34a2b-178">The **Deploy a SQL Server Database to a Microsoft Azure VM wizard** is a simple and recommended way to move data from an on-premises SQL Server instance to SQL Server on an Azure VM.</span></span> <span data-ttu-id="34a2b-179">Podrobné kroky a také se zabývat náhradních řešení, najdete v části [migrace databáze do systému SQL Server na virtuálním počítači Azure](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).</span><span class="sxs-lookup"><span data-stu-id="34a2b-179">For detailed steps as well as a discussion of other alternatives, see [Migrate a database to SQL Server on an Azure VM](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).</span></span>

### <span data-ttu-id="34a2b-180"><a name="export-flat-file"></a>Exportovat do plochý soubor</span><span class="sxs-lookup"><span data-stu-id="34a2b-180"><a name="export-flat-file"></a>Export to Flat File</span></span>
<span data-ttu-id="34a2b-181">Chcete-li hromadně exportu dat z SQL serveru místní, jak je uvedeno v lze použít různé metody [hromadný Import a Export dat (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="34a2b-181">Various methods can be used to bulk export data from an On-Premises SQL Server as documented in the [Bulk Import and Export of Data (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) topic.</span></span> <span data-ttu-id="34a2b-182">Tento dokument se týká Program hromadného kopírování (BCP) jako příklad.</span><span class="sxs-lookup"><span data-stu-id="34a2b-182">This document will cover the Bulk Copy Program (BCP) as an example.</span></span> <span data-ttu-id="34a2b-183">Jakmile data se exportují do plochý soubor, lze importovat na jiný server SQL pomocí hromadného importu.</span><span class="sxs-lookup"><span data-stu-id="34a2b-183">Once data is exported into a flat file, it can be imported to another SQL server using bulk import.</span></span>

1. <span data-ttu-id="34a2b-184">Exportovat data z místního SQL serveru do souboru pomocí nástroje bcp takto</span><span class="sxs-lookup"><span data-stu-id="34a2b-184">Export the data from on-premises SQL Server to a File using the bcp utility as follows</span></span>

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. <span data-ttu-id="34a2b-185">Vytvořit databázi a tabulku na virtuální počítač s SQL serverem na Azure pomocí `create database` a `create table` pro schéma tabulky exportovali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="34a2b-185">Create the database and the table on SQL Server VM on Azure using the `create database` and `create table` for the table schema exported in step 1.</span></span>
3. <span data-ttu-id="34a2b-186">Vytvořte soubor ve formátu pro popisující schématu tabulky se exportovat nebo importovat data.</span><span class="sxs-lookup"><span data-stu-id="34a2b-186">Create a format file for describing the table schema of the data being exported/imported.</span></span> <span data-ttu-id="34a2b-187">Podrobnosti o formátu souboru jsou popsané v [vytvořit soubor ve formátu (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).</span><span class="sxs-lookup"><span data-stu-id="34a2b-187">Details of the format file are described in [Create a Format File (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).</span></span>

    <span data-ttu-id="34a2b-188">Generování souboru formátu při spuštění BCP z počítače systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="34a2b-188">Format file generation when running BCP from the SQL Server machine</span></span>

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    <span data-ttu-id="34a2b-189">Generování souboru formátu při spuštění BCP vzdáleně s SQL serverem</span><span class="sxs-lookup"><span data-stu-id="34a2b-189">Format file generation when running BCP remotely against a SQL Server</span></span>

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. <span data-ttu-id="34a2b-190">Použít některou z metod popsaných v části [přesun dat ze souboru zdroje](#filesource_to_sqlonazurevm) pro přesun dat v ploché soubory do systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="34a2b-190">Use any of the methods described in section [Moving Data from File Source](#filesource_to_sqlonazurevm) to move the data in flat files to a SQL Server.</span></span>

### <span data-ttu-id="34a2b-191"><a name="sql-migration"></a>Průvodce migrací databáze SQL</span><span class="sxs-lookup"><span data-stu-id="34a2b-191"><a name="sql-migration"></a>SQL Database Migration Wizard</span></span>
<span data-ttu-id="34a2b-192">[Průvodce migrací databáze SQL serveru](http://sqlazuremw.codeplex.com/) obsahuje uživatelsky přívětivé pro přesun dat mezi dvě instance serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="34a2b-192">[SQL Server Database Migration Wizard](http://sqlazuremw.codeplex.com/) provides a user-friendly way to move data between two SQL server instances.</span></span> <span data-ttu-id="34a2b-193">Umožňuje uživateli mapování schéma dat mezi zdroji a cílovou tabulku, zvolte typy sloupců a různé další funkce.</span><span class="sxs-lookup"><span data-stu-id="34a2b-193">It allows the user to map the data schema between sources and destination tables, choose column types and various other functionalities.</span></span> <span data-ttu-id="34a2b-194">Hromadné kopírování (BCP) skrytě používá.</span><span class="sxs-lookup"><span data-stu-id="34a2b-194">It uses bulk copy (BCP) under the covers.</span></span> <span data-ttu-id="34a2b-195">Snímek obrazovky na úvodní obrazovce Průvodce migrací databáze SQL jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="34a2b-195">A screenshot of the welcome screen for the SQL Database Migration wizard is shown below.</span></span>  

![Průvodce migrací serveru SQL][2]

### <span data-ttu-id="34a2b-197"><a name="sql-backup"></a>Databáze back up a obnovení</span><span class="sxs-lookup"><span data-stu-id="34a2b-197"><a name="sql-backup"></a>Database back up and restore</span></span>
<span data-ttu-id="34a2b-198">Systém SQL Server podporuje:</span><span class="sxs-lookup"><span data-stu-id="34a2b-198">SQL Server supports:</span></span>

1. <span data-ttu-id="34a2b-199">[Zálohu databáze zpět a obnovit funkčnost](https://msdn.microsoft.com/library/ms187048.aspx) (i do místního souboru nebo souboru bacpac exportovat do objektu blob) a [aplikace datové vrstvy](https://msdn.microsoft.com/library/ee210546.aspx) (pomocí souboru bacpac).</span><span class="sxs-lookup"><span data-stu-id="34a2b-199">[Database back up and restore functionality](https://msdn.microsoft.com/library/ms187048.aspx) (both to a local file or bacpac export to blob) and [Data Tier Applications](https://msdn.microsoft.com/library/ee210546.aspx) (using bacpac).</span></span>
2. <span data-ttu-id="34a2b-200">Schopnost přímo v Azure vytvořit virtuální počítače serveru SQL s zkopírovaný databáze nebo kopírovat do existující databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="34a2b-200">Ability to directly create SQL Server VMs on Azure with a copied database or copy to an existing SQL Azure database.</span></span> <span data-ttu-id="34a2b-201">Další podrobnosti najdete v tématu [pomocí Průvodce kopírováním databáze](https://msdn.microsoft.com/library/ms188664.aspx).</span><span class="sxs-lookup"><span data-stu-id="34a2b-201">For more details, see [Use the Copy Database Wizard](https://msdn.microsoft.com/library/ms188664.aspx).</span></span>

<span data-ttu-id="34a2b-202">Snímek obrazovky zálohování databáze nebo obnovit možnosti z SQL Server Management Studio jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="34a2b-202">A screenshot of the Database back up/restore options from SQL Server Management Studio is shown below.</span></span>

![Nástroj pro Import serveru SQL][1]

## <a name="resources"></a><span data-ttu-id="34a2b-204">Zdroje</span><span class="sxs-lookup"><span data-stu-id="34a2b-204">Resources</span></span>
[<span data-ttu-id="34a2b-205">Migrace databáze do systému SQL Server ve virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="34a2b-205">Migrate a Database to SQL Server on an Azure VM</span></span>](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[<span data-ttu-id="34a2b-206">SQL Server na Azure Virtual Machines – přehled</span><span class="sxs-lookup"><span data-stu-id="34a2b-206">SQL Server on Azure Virtual Machines overview</span></span>](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
