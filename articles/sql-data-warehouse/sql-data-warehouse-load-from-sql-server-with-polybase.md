---
title: aaaLoad dat z SQL serveru do Azure SQL Data Warehouse (PolyBase) | Microsoft Docs
description: "Využívá bcp tooexport data z tooflat soubory systému SQL Server, úložiště objektů blob tooAzure data tooimport AZCopy a PolyBase tooingest hello data do Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 860c86e0-90f7-492c-9a84-1bdd3d1735cd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1346fb016e0538a44426671bf4e29358cb24f7ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-polybase-in-sql-data-warehouse"></a><span data-ttu-id="48cc6-103">Načtení dat pomocí funkce PolyBase v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="48cc6-103">Load data with PolyBase in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="48cc6-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="48cc6-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="48cc6-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="48cc6-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="48cc6-106">bcp</span><span class="sxs-lookup"><span data-stu-id="48cc6-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="48cc6-107">Tento kurz ukazuje, jak tooload dat do SQL Data Warehouse s použitím AzCopy a PolyBase.</span><span class="sxs-lookup"><span data-stu-id="48cc6-107">This tutorial shows how tooload data into SQL Data Warehouse by using AzCopy and PolyBase.</span></span> <span data-ttu-id="48cc6-108">Po dokončení tohoto kurzu budete vědět, jak:</span><span class="sxs-lookup"><span data-stu-id="48cc6-108">When finished, you will know how to:</span></span>

* <span data-ttu-id="48cc6-109">Používání úložiště blob tooAzure data toocopy AzCopy</span><span class="sxs-lookup"><span data-stu-id="48cc6-109">Use AzCopy toocopy data tooAzure blob storage</span></span>
* <span data-ttu-id="48cc6-110">Vytvoření databázových objektů toodefine hello dat</span><span class="sxs-lookup"><span data-stu-id="48cc6-110">Create database objects toodefine hello data</span></span>
* <span data-ttu-id="48cc6-111">Hello dat tooload dotazu T-SQL</span><span class="sxs-lookup"><span data-stu-id="48cc6-111">Run a T-SQL query tooload hello data</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-with-PolyBase-in-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="48cc6-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="48cc6-112">Prerequisites</span></span>
<span data-ttu-id="48cc6-113">toostep prostřednictvím tohoto kurzu budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="48cc6-113">toostep through this tutorial, you need</span></span>

* <span data-ttu-id="48cc6-114">Databázi SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="48cc6-114">A SQL Data Warehouse database.</span></span>
* <span data-ttu-id="48cc6-115">Účet úložiště Azure typu Standard Locally Redundant Storage (Standard-LRS), Standard Geo-Redundant Storage (Standard-GRS) nebo Standard Read-Access Geo-Redundant Storage (Standard-RAGRS)</span><span class="sxs-lookup"><span data-stu-id="48cc6-115">An Azure storage account of type Standard Locally Redundant Storage (Standard-LRS), Standard Geo-Redundant Storage (Standard-GRS), or Standard Read-Access Geo-Redundant Storage (Standard-RAGRS).</span></span>
* <span data-ttu-id="48cc6-116">Nástroj příkazového řádku AzCopy.</span><span class="sxs-lookup"><span data-stu-id="48cc6-116">AzCopy Command-Line Utility.</span></span> <span data-ttu-id="48cc6-117">Stáhněte a nainstalujte hello [nejnovější verzi AzCopy] [ latest version of AzCopy] nainstalované s hello Microsoft Azure Storage Tools.</span><span class="sxs-lookup"><span data-stu-id="48cc6-117">Download and install hello [latest version of AzCopy][latest version of AzCopy] which is installed with hello Microsoft Azure Storage Tools.</span></span>
  
    ![Nástroje Azure Storage Tools](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)

## <a name="step-1-add-sample-data-tooazure-blob-storage"></a><span data-ttu-id="48cc6-119">Krok 1: Přidání ukázkových dat tooAzure objektu blob úložiště</span><span class="sxs-lookup"><span data-stu-id="48cc6-119">Step 1: Add sample data tooAzure blob storage</span></span>
<span data-ttu-id="48cc6-120">V datech tooload pořadí potřebujeme tooput ukázková data do Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="48cc6-120">In order tooload data, we need tooput some sample data into an Azure blob storage.</span></span> <span data-ttu-id="48cc6-121">V tomto kroku naplníme objekt blob úložiště Azure ukázkovými daty.</span><span class="sxs-lookup"><span data-stu-id="48cc6-121">In this step we populate an Azure Storage blob with sample data.</span></span> <span data-ttu-id="48cc6-122">Později použijeme PolyBase tooload Tato ukázková data do databáze SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="48cc6-122">Later, we will use PolyBase tooload this sample data into your SQL Data Warehouse database.</span></span>

### <a name="a-prepare-a-sample-text-file"></a><span data-ttu-id="48cc6-123">A.</span><span class="sxs-lookup"><span data-stu-id="48cc6-123">A.</span></span> <span data-ttu-id="48cc6-124">Příprava ukázkového textového souboru</span><span class="sxs-lookup"><span data-stu-id="48cc6-124">Prepare a sample text file</span></span>
<span data-ttu-id="48cc6-125">tooprepare ukázkový textový soubor:</span><span class="sxs-lookup"><span data-stu-id="48cc6-125">tooprepare a sample text file:</span></span>

1. <span data-ttu-id="48cc6-126">Otevřete Poznámkový blok a zkopírujte hello následující řádky dat do nového souboru.</span><span class="sxs-lookup"><span data-stu-id="48cc6-126">Open Notepad and copy hello following lines of data into a new file.</span></span> <span data-ttu-id="48cc6-127">Uložte tento tooyour místního dočasného adresáře jako % temp%\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="48cc6-127">Save this tooyour local temp directory as %temp%\DimDate2.txt.</span></span>

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a><span data-ttu-id="48cc6-128">B.</span><span class="sxs-lookup"><span data-stu-id="48cc6-128">B.</span></span> <span data-ttu-id="48cc6-129">Vyhledání vašeho koncového bodu Služby objektů blob</span><span class="sxs-lookup"><span data-stu-id="48cc6-129">Find your blob service endpoint</span></span>
<span data-ttu-id="48cc6-130">toofind vašeho koncového bodu služby objektů blob:</span><span class="sxs-lookup"><span data-stu-id="48cc6-130">toofind your blob service endpoint:</span></span>

1. <span data-ttu-id="48cc6-131">Hello portálu Azure vyberte **Procházet** > **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="48cc6-131">From hello Azure Portal select **Browse** > **Storage Accounts**.</span></span>
2. <span data-ttu-id="48cc6-132">Klikněte na tlačítko hello úložiště účet, že který má toouse.</span><span class="sxs-lookup"><span data-stu-id="48cc6-132">Click hello storage account you want toouse.</span></span>
3. <span data-ttu-id="48cc6-133">V okně účtu úložiště hello klikněte na objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="48cc6-133">In hello Storage account blade, click Blobs</span></span>
   
    ![Klikněte na Objekty blob.](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)
4. <span data-ttu-id="48cc6-135">Uložte si adresu URL koncového bodu Služby objektů blob pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="48cc6-135">Save your blob service endpoint URL for later.</span></span>
   
    ![Koncový bod Služby objektů blob](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a><span data-ttu-id="48cc6-137">C.</span><span class="sxs-lookup"><span data-stu-id="48cc6-137">C.</span></span> <span data-ttu-id="48cc6-138">Vyhledání klíče účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="48cc6-138">Find your Azure storage key</span></span>
<span data-ttu-id="48cc6-139">toofind klíče účtu úložiště Azure:</span><span class="sxs-lookup"><span data-stu-id="48cc6-139">toofind your Azure storage key:</span></span>

1. <span data-ttu-id="48cc6-140">Hello portálu Azure, vyberte **Procházet** > **účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="48cc6-140">From hello Azure Portal, select **Browse** > **Storage Accounts**.</span></span>
2. <span data-ttu-id="48cc6-141">Klikněte na účet úložiště hello, že chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="48cc6-141">Click on hello storage account you want toouse.</span></span>
3. <span data-ttu-id="48cc6-142">Vyberte **Všechna nastavení** > **Přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="48cc6-142">Select **All settings** > **Access keys**.</span></span>
4. <span data-ttu-id="48cc6-143">Klikněte na tlačítko hello kopírování pole toocopy mezi schránky toohello klíče přístup.</span><span class="sxs-lookup"><span data-stu-id="48cc6-143">Click hello copy box toocopy one of your access keys toohello clipboard.</span></span>
   
    ![Zkopírování klíče úložiště Azure](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-hello-sample-file-tooazure-blob-storage"></a><span data-ttu-id="48cc6-145">D.</span><span class="sxs-lookup"><span data-stu-id="48cc6-145">D.</span></span> <span data-ttu-id="48cc6-146">Zkopírujte hello ukázkový soubor tooAzure objektu blob úložiště</span><span class="sxs-lookup"><span data-stu-id="48cc6-146">Copy hello sample file tooAzure blob storage</span></span>
<span data-ttu-id="48cc6-147">toocopy úložiště objektů blob tooAzure dat:</span><span class="sxs-lookup"><span data-stu-id="48cc6-147">toocopy your data tooAzure blob storage:</span></span>

1. <span data-ttu-id="48cc6-148">Otevřete příkazový řádek a změnit instalační adresář AzCopy toohello adresáře.</span><span class="sxs-lookup"><span data-stu-id="48cc6-148">Open a command prompt, and change directories toohello AzCopy installation directory.</span></span> <span data-ttu-id="48cc6-149">Tento příkaz změní toohello výchozí instalační adresář v klientovi Windows 64-bit.</span><span class="sxs-lookup"><span data-stu-id="48cc6-149">This command changes toohello default installation directory on a 64-bit Windows client.</span></span>
   
    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```
2. <span data-ttu-id="48cc6-150">Spusťte následující příkaz tooupload hello soubor hello.</span><span class="sxs-lookup"><span data-stu-id="48cc6-150">Run hello following command tooupload hello file.</span></span> <span data-ttu-id="48cc6-151">Zadejte svoji adresu URL koncového bodu Služby objektů blob pro <blob service endpoint URL> a klíč účtu úložiště Azure pro <klíč_účtu_úložiště_azure>.</span><span class="sxs-lookup"><span data-stu-id="48cc6-151">Specify your blob service endpoint URL for <blob service endpoint URL> and your Azure storage account key for <azure_storage_account_key>.</span></span>
   
    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

<span data-ttu-id="48cc6-152">Viz také [Začínáme s hello příkazového řádku Azcopy][latest version of AzCopy].</span><span class="sxs-lookup"><span data-stu-id="48cc6-152">See also [Getting Started with hello AzCopy Command-Line Utility][latest version of AzCopy].</span></span>

### <a name="e-explore-your-blob-storage-container"></a><span data-ttu-id="48cc6-153">E.</span><span class="sxs-lookup"><span data-stu-id="48cc6-153">E.</span></span> <span data-ttu-id="48cc6-154">Prozkoumání vašeho kontejneru úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="48cc6-154">Explore your blob storage container</span></span>
<span data-ttu-id="48cc6-155">toosee hello soubor, který jste nahráli tooblob úložiště:</span><span class="sxs-lookup"><span data-stu-id="48cc6-155">toosee hello file you uploaded tooblob storage:</span></span>

1. <span data-ttu-id="48cc6-156">Vraťte se zpátky tooyour objektu Blob služby okno.</span><span class="sxs-lookup"><span data-stu-id="48cc6-156">Go back tooyour Blob service blade.</span></span>
2. <span data-ttu-id="48cc6-157">V části Kontejnery poklikejte na **datacontainer**.</span><span class="sxs-lookup"><span data-stu-id="48cc6-157">Under Containers, double-click **datacontainer**.</span></span>
3. <span data-ttu-id="48cc6-158">tooexplore hello cesta tooyour data, klikněte na složku hello **datedimension** a zobrazí se nahraný soubor **DimDate2.txt**.</span><span class="sxs-lookup"><span data-stu-id="48cc6-158">tooexplore hello path tooyour data, click hello folder **datedimension** and you will see your uploaded file **DimDate2.txt**.</span></span>
4. <span data-ttu-id="48cc6-159">Klikněte na tlačítko Vlastnosti tooview **DimDate2.txt**.</span><span class="sxs-lookup"><span data-stu-id="48cc6-159">tooview properties, click **DimDate2.txt**.</span></span>
5. <span data-ttu-id="48cc6-160">Všimněte si, že v okně Vlastnosti hello objektů Blob můžete stáhnout nebo odstranit soubor hello.</span><span class="sxs-lookup"><span data-stu-id="48cc6-160">Note that in hello Blob properties blade, you can download or delete hello file.</span></span>
   
    ![Zobrazení objektu blob úložiště Azure](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)

## <a name="step-2-create-an-external-table-for-hello-sample-data"></a><span data-ttu-id="48cc6-162">Krok 2: Vytvoření externí tabulky pro hello ukázková data</span><span class="sxs-lookup"><span data-stu-id="48cc6-162">Step 2: Create an external table for hello sample data</span></span>
<span data-ttu-id="48cc6-163">V této části vytvoříme externí tabulku, která definuje ukázková data hello.</span><span class="sxs-lookup"><span data-stu-id="48cc6-163">In this section we create an external table that defines hello sample data.</span></span>

<span data-ttu-id="48cc6-164">PolyBase používá externí tabulky tooaccess data v úložišti objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="48cc6-164">PolyBase uses external tables tooaccess data in Azure blob storage.</span></span> <span data-ttu-id="48cc6-165">Vzhledem k tomu, že hello data nejsou uložená v SQL Data Warehouse, zpracovává PolyBase ověřování toohello externí data pomocí přihlašovacích údajů platných pro databázi.</span><span class="sxs-lookup"><span data-stu-id="48cc6-165">Since hello data is not stored within SQL Data Warehouse, PolyBase handles authentication toohello external data by using a database-scoped credential.</span></span>

<span data-ttu-id="48cc6-166">Příklad Hello v tomto kroku používá tyto toocreate příkazy jazyka Transact-SQL externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="48cc6-166">hello example in this step uses these Transact-SQL statements toocreate an external table.</span></span>

* <span data-ttu-id="48cc6-167">[Vytvoření hlavního klíče (Transact-SQL)] [ Create Master Key (Transact-SQL)] tooencrypt hello tajný klíč vaší databáze vašich přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="48cc6-167">[Create Master Key (Transact-SQL)][Create Master Key (Transact-SQL)] tooencrypt hello secret of your database scoped credential.</span></span>
* <span data-ttu-id="48cc6-168">[Create Database Scoped Credential (Transact-SQL)] [ Create Database Scoped Credential (Transact-SQL)] toospecify ověřovacích informací pro váš účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="48cc6-168">[Create Database Scoped Credential (Transact-SQL)][Create Database Scoped Credential (Transact-SQL)] toospecify authentication information for your Azure storage account.</span></span>
* <span data-ttu-id="48cc6-169">[Create External Data Source (Transact-SQL)] [ Create External Data Source (Transact-SQL)] toospecify hello umístění úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="48cc6-169">[Create External Data Source (Transact-SQL)][Create External Data Source (Transact-SQL)] toospecify hello location of your Azure blob storage.</span></span>
* <span data-ttu-id="48cc6-170">[Create External File Format (Transact-SQL)] [ Create External File Format (Transact-SQL)] toospecify hello formát data.</span><span class="sxs-lookup"><span data-stu-id="48cc6-170">[Create External File Format (Transact-SQL)][Create External File Format (Transact-SQL)] toospecify hello format of your data.</span></span>
* <span data-ttu-id="48cc6-171">[Create External Table (Transact-SQL)] [ Create External Table (Transact-SQL)] definice tabulky hello toospecify a umístění hello data.</span><span class="sxs-lookup"><span data-stu-id="48cc6-171">[Create External Table (Transact-SQL)][Create External Table (Transact-SQL)] toospecify hello table definition and location of hello data.</span></span>

<span data-ttu-id="48cc6-172">Spusťte tento dotaz na databázi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="48cc6-172">Run this query against your SQL Data Warehouse database.</span></span> <span data-ttu-id="48cc6-173">Vytvoří externí tabulku s názvem DimDate2External ve schématu dbo hello, který odkazuje ukázková data DimDate2.txt toohello v hello Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="48cc6-173">It will create an external table named DimDate2External in hello dbo schema that points toohello DimDate2.txt sample data in hello Azure blob storage.</span></span>

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required tooencrypt hello credential secret in hello next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication tooAzure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs tooaccess data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide hello credential created in hello previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create hello external table
-- Specify column names and data types. This needs toomatch hello data in hello sample file.
-- LOCATION: Specify path toofile or directory that contains hello data (relative toohello blob container).
-- toopoint tooall files under hello blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on hello external table

SELECT count(*) FROM dbo.DimDate2External;

```


<span data-ttu-id="48cc6-174">V Průzkumníku objektů SQL Server v sadě Visual Studio uvidíte formát externích souborů hello, externí zdroj dat a tabulku DimDate2External hello.</span><span class="sxs-lookup"><span data-stu-id="48cc6-174">In SQL Server Object Explorer in Visual Studio, you can see hello external file format, external data source, and hello DimDate2External table.</span></span>

![Zobrazení externí tabulky](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a><span data-ttu-id="48cc6-176">Krok 3: Načtení dat do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="48cc6-176">Step 3: Load data into SQL Data Warehouse</span></span>
<span data-ttu-id="48cc6-177">Po vytvoření externí tabulky hello můžete načíst hello data do nové tabulky nebo vložit do existující tabulky.</span><span class="sxs-lookup"><span data-stu-id="48cc6-177">Once hello external table is created, you can either load hello data into a new table or insert it into an existing table.</span></span>

* <span data-ttu-id="48cc6-178">tooload hello data do nové tabulky, spusťte hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] příkaz.</span><span class="sxs-lookup"><span data-stu-id="48cc6-178">tooload hello data into a new table, run hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="48cc6-179">Hello nová tabulka bude mít hello sloupce pojmenované v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="48cc6-179">hello new table will have hello columns named in hello query.</span></span> <span data-ttu-id="48cc6-180">Hello datové typy sloupců hello bude odpovídat hello datových typů v definici externí tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="48cc6-180">hello data types of hello columns will match hello data types in hello external table definition.</span></span>
* <span data-ttu-id="48cc6-181">tooload hello data do existující tabulky, použijte hello [INSERT... SELECT (Transact-SQL)] [ INSERT...SELECT (Transact-SQL)] příkaz.</span><span class="sxs-lookup"><span data-stu-id="48cc6-181">tooload hello data into an existing table, use hello [INSERT...SELECT (Transact-SQL)][INSERT...SELECT (Transact-SQL)] statement.</span></span>

```sql
-- Load hello data from Azure blob storage tooSQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="48cc6-182">Krok 4: Vytvoření statistiky pro nově načtená data</span><span class="sxs-lookup"><span data-stu-id="48cc6-182">Step 4: Create statistics on your newly loaded data</span></span>
<span data-ttu-id="48cc6-183">SQL Data Warehouse nevytváří ani neaktualizuje statistiku automaticky.</span><span class="sxs-lookup"><span data-stu-id="48cc6-183">SQL Data Warehouse does not auto-create or auto-update statistics.</span></span> <span data-ttu-id="48cc6-184">Proto tooachieve vysokého výkonu dotazu, je důležité nejdřív načíst toocreate statistiku pro každý sloupec každé tabulky po hello.</span><span class="sxs-lookup"><span data-stu-id="48cc6-184">Therefore, tooachieve high query performance, it's important toocreate statistics on each column of each table after hello first load.</span></span> <span data-ttu-id="48cc6-185">Je také důležité tooupdate statistiku po důležitých změnách v datech hello.</span><span class="sxs-lookup"><span data-stu-id="48cc6-185">It's also important tooupdate statistics after substantial changes in hello data.</span></span>

<span data-ttu-id="48cc6-186">Tento příklad vytvoří jednosloupcovou statistiku na novou tabulku DimDate2 hello.</span><span class="sxs-lookup"><span data-stu-id="48cc6-186">This example creates single-column statistics on hello new DimDate2 table.</span></span>

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

<span data-ttu-id="48cc6-187">Další, najdete v části toolearn [statistiky][Statistics].</span><span class="sxs-lookup"><span data-stu-id="48cc6-187">toolearn more, see [Statistics][Statistics].</span></span>  

## <a name="next-steps"></a><span data-ttu-id="48cc6-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48cc6-188">Next steps</span></span>
<span data-ttu-id="48cc6-189">V tématu hello [Průvodce funkcí PolyBase] [ PolyBase guide] Další informace, které byste měli vědět, když budete vyvíjet řešení využívající funkci PolyBase.</span><span class="sxs-lookup"><span data-stu-id="48cc6-189">See hello [PolyBase guide][PolyBase guide] for further information you should know as you develop a solution that uses PolyBase.</span></span>

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[PolyBase guide]: ./sql-data-warehouse-load-polybase-guide.md
[latest version of AzCopy]:../storage/common/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
