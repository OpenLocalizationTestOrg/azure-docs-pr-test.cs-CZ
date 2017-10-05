---
title: "Načtení dat z SQL Serveru do Azure SQL Data Warehouse (PolyBase) | Dokumentace Microsoftu"
description: "Využívá bcp k exportu dat z SQL Serveru do plochých souborů, AZCopy k importu dat do Azure Blob Storage a PolyBase ke zpracování příjmu dat do Azure SQL Data Warehouse."
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
ms.openlocfilehash: 966100094f98bae41bf90df500d005fa78b31ec3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="load-data-with-polybase-in-sql-data-warehouse"></a><span data-ttu-id="0c5db-103">Načtení dat pomocí funkce PolyBase v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0c5db-103">Load data with PolyBase in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0c5db-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="0c5db-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="0c5db-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="0c5db-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="0c5db-106">bcp</span><span class="sxs-lookup"><span data-stu-id="0c5db-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="0c5db-107">Tento kurz ukazuje, jak načíst data do SQL Data Warehouse s použitím AzCopy a PolyBase.</span><span class="sxs-lookup"><span data-stu-id="0c5db-107">This tutorial shows how to load data into SQL Data Warehouse by using AzCopy and PolyBase.</span></span> <span data-ttu-id="0c5db-108">Po dokončení tohoto kurzu budete vědět, jak:</span><span class="sxs-lookup"><span data-stu-id="0c5db-108">When finished, you will know how to:</span></span>

* <span data-ttu-id="0c5db-109">Pomocí AzCopy kopírovat data do Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="0c5db-109">Use AzCopy to copy data to Azure blob storage</span></span>
* <span data-ttu-id="0c5db-110">Vytvářením databázových objektů definovat data</span><span class="sxs-lookup"><span data-stu-id="0c5db-110">Create database objects to define the data</span></span>
* <span data-ttu-id="0c5db-111">Spuštěním dotazu T-SQL načítat data</span><span class="sxs-lookup"><span data-stu-id="0c5db-111">Run a T-SQL query to load the data</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-with-PolyBase-in-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="0c5db-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0c5db-112">Prerequisites</span></span>
<span data-ttu-id="0c5db-113">Pro jednotlivé kroky v tomto kurzu budete potřebovat</span><span class="sxs-lookup"><span data-stu-id="0c5db-113">To step through this tutorial, you need</span></span>

* <span data-ttu-id="0c5db-114">Databázi SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0c5db-114">A SQL Data Warehouse database.</span></span>
* <span data-ttu-id="0c5db-115">Účet úložiště Azure typu Standard Locally Redundant Storage (Standard-LRS), Standard Geo-Redundant Storage (Standard-GRS) nebo Standard Read-Access Geo-Redundant Storage (Standard-RAGRS)</span><span class="sxs-lookup"><span data-stu-id="0c5db-115">An Azure storage account of type Standard Locally Redundant Storage (Standard-LRS), Standard Geo-Redundant Storage (Standard-GRS), or Standard Read-Access Geo-Redundant Storage (Standard-RAGRS).</span></span>
* <span data-ttu-id="0c5db-116">Nástroj příkazového řádku AzCopy.</span><span class="sxs-lookup"><span data-stu-id="0c5db-116">AzCopy Command-Line Utility.</span></span> <span data-ttu-id="0c5db-117">Stáhněte a nainstalujte si [nejnovější verzi AzCopy][latest version of AzCopy], která se instaluje s nástroji Microsoft Azure Storage Tools.</span><span class="sxs-lookup"><span data-stu-id="0c5db-117">Download and install the [latest version of AzCopy][latest version of AzCopy] which is installed with the Microsoft Azure Storage Tools.</span></span>
  
    ![Nástroje Azure Storage Tools](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)

## <a name="step-1-add-sample-data-to-azure-blob-storage"></a><span data-ttu-id="0c5db-119">Krok 1: Přidání ukázkových dat do Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="0c5db-119">Step 1: Add sample data to Azure blob storage</span></span>
<span data-ttu-id="0c5db-120">Aby bylo možné data načíst, musíme vložit nějaká ukázková data do Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="0c5db-120">In order to load data, we need to put some sample data into an Azure blob storage.</span></span> <span data-ttu-id="0c5db-121">V tomto kroku naplníme objekt blob úložiště Azure ukázkovými daty.</span><span class="sxs-lookup"><span data-stu-id="0c5db-121">In this step we populate an Azure Storage blob with sample data.</span></span> <span data-ttu-id="0c5db-122">Později pomocí funkce PolyBase načteme tato ukázková data do vaší databáze SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0c5db-122">Later, we will use PolyBase to load this sample data into your SQL Data Warehouse database.</span></span>

### <a name="a-prepare-a-sample-text-file"></a><span data-ttu-id="0c5db-123">A.</span><span class="sxs-lookup"><span data-stu-id="0c5db-123">A.</span></span> <span data-ttu-id="0c5db-124">Příprava ukázkového textového souboru</span><span class="sxs-lookup"><span data-stu-id="0c5db-124">Prepare a sample text file</span></span>
<span data-ttu-id="0c5db-125">Ukázkový textový soubor připravíte takto:</span><span class="sxs-lookup"><span data-stu-id="0c5db-125">To prepare a sample text file:</span></span>

1. <span data-ttu-id="0c5db-126">Otevřete Poznámkový blok a zkopírujte následující řádky dat do nového souboru.</span><span class="sxs-lookup"><span data-stu-id="0c5db-126">Open Notepad and copy the following lines of data into a new file.</span></span> <span data-ttu-id="0c5db-127">Uložte zkopírované řádky do místního dočasného (temp) adresáře do souboru % temp%\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="0c5db-127">Save this to your local temp directory as %temp%\DimDate2.txt.</span></span>

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

### <a name="b-find-your-blob-service-endpoint"></a><span data-ttu-id="0c5db-128">B.</span><span class="sxs-lookup"><span data-stu-id="0c5db-128">B.</span></span> <span data-ttu-id="0c5db-129">Vyhledání vašeho koncového bodu Služby objektů blob</span><span class="sxs-lookup"><span data-stu-id="0c5db-129">Find your blob service endpoint</span></span>
<span data-ttu-id="0c5db-130">Vyhledání vašeho koncového bodu Služby objektů blob:</span><span class="sxs-lookup"><span data-stu-id="0c5db-130">To find your blob service endpoint:</span></span>

1. <span data-ttu-id="0c5db-131">Na portálu Azure vyberte **Procházet** > **Účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="0c5db-131">From the Azure Portal select **Browse** > **Storage Accounts**.</span></span>
2. <span data-ttu-id="0c5db-132">Klikněte na účet úložiště, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="0c5db-132">Click the storage account you want to use.</span></span>
3. <span data-ttu-id="0c5db-133">V okně Účet úložiště klikněte na Objekty blob.</span><span class="sxs-lookup"><span data-stu-id="0c5db-133">In the Storage account blade, click Blobs</span></span>
   
    ![Klikněte na Objekty blob.](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)
4. <span data-ttu-id="0c5db-135">Uložte si adresu URL koncového bodu Služby objektů blob pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="0c5db-135">Save your blob service endpoint URL for later.</span></span>
   
    ![Koncový bod Služby objektů blob](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a><span data-ttu-id="0c5db-137">C.</span><span class="sxs-lookup"><span data-stu-id="0c5db-137">C.</span></span> <span data-ttu-id="0c5db-138">Vyhledání klíče účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="0c5db-138">Find your Azure storage key</span></span>
<span data-ttu-id="0c5db-139">Vyhledání klíče účtu úložiště Azure:</span><span class="sxs-lookup"><span data-stu-id="0c5db-139">To find your Azure storage key:</span></span>

1. <span data-ttu-id="0c5db-140">Na portálu Azure vyberte **Procházet** > **Účty úložiště**.</span><span class="sxs-lookup"><span data-stu-id="0c5db-140">From the Azure Portal, select **Browse** > **Storage Accounts**.</span></span>
2. <span data-ttu-id="0c5db-141">Klikněte na účet úložiště, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="0c5db-141">Click on the storage account you want to use.</span></span>
3. <span data-ttu-id="0c5db-142">Vyberte **Všechna nastavení** > **Přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="0c5db-142">Select **All settings** > **Access keys**.</span></span>
4. <span data-ttu-id="0c5db-143">Kliknutím na políčko pro kopírování si zkopírujte jeden ze svých přístupových klíčů do schránky.</span><span class="sxs-lookup"><span data-stu-id="0c5db-143">Click the copy box to copy one of your access keys to the clipboard.</span></span>
   
    ![Zkopírování klíče úložiště Azure](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a><span data-ttu-id="0c5db-145">D.</span><span class="sxs-lookup"><span data-stu-id="0c5db-145">D.</span></span> <span data-ttu-id="0c5db-146">Zkopírování ukázkového souboru do Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="0c5db-146">Copy the sample file to Azure blob storage</span></span>
<span data-ttu-id="0c5db-147">Zkopírování vašich dat do Azure Blob Storage:</span><span class="sxs-lookup"><span data-stu-id="0c5db-147">To copy your data to Azure blob storage:</span></span>

1. <span data-ttu-id="0c5db-148">Otevřete příkazový řádek a změňte adresář na instalační adresář AzCopy.</span><span class="sxs-lookup"><span data-stu-id="0c5db-148">Open a command prompt, and change directories to the AzCopy installation directory.</span></span> <span data-ttu-id="0c5db-149">Tento příkaz změní adresář na výchozí instalační adresář na 64bitovém klientovi Windows.</span><span class="sxs-lookup"><span data-stu-id="0c5db-149">This command changes to the default installation directory on a 64-bit Windows client.</span></span>
   
    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```
2. <span data-ttu-id="0c5db-150">Spuštěním následujícího příkazu nahrajte soubor:</span><span class="sxs-lookup"><span data-stu-id="0c5db-150">Run the following command to upload the file.</span></span> <span data-ttu-id="0c5db-151">Zadejte svoji adresu URL koncového bodu Služby objektů blob pro <blob service endpoint URL> a klíč účtu úložiště Azure pro <klíč_účtu_úložiště_azure>.</span><span class="sxs-lookup"><span data-stu-id="0c5db-151">Specify your blob service endpoint URL for <blob service endpoint URL> and your Azure storage account key for <azure_storage_account_key>.</span></span>
   
    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

<span data-ttu-id="0c5db-152">Viz také [Začínáme s nástrojem příkazového řádku AzCopy][latest version of AzCopy].</span><span class="sxs-lookup"><span data-stu-id="0c5db-152">See also [Getting Started with the AzCopy Command-Line Utility][latest version of AzCopy].</span></span>

### <a name="e-explore-your-blob-storage-container"></a><span data-ttu-id="0c5db-153">E.</span><span class="sxs-lookup"><span data-stu-id="0c5db-153">E.</span></span> <span data-ttu-id="0c5db-154">Prozkoumání vašeho kontejneru úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="0c5db-154">Explore your blob storage container</span></span>
<span data-ttu-id="0c5db-155">Zobrazení souboru, který jste nahráli do úložiště objektů blob:</span><span class="sxs-lookup"><span data-stu-id="0c5db-155">To see the file you uploaded to blob storage:</span></span>

1. <span data-ttu-id="0c5db-156">Přejděte zpět do okna vaší Služby objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0c5db-156">Go back to your Blob service blade.</span></span>
2. <span data-ttu-id="0c5db-157">V části Kontejnery poklikejte na **datacontainer**.</span><span class="sxs-lookup"><span data-stu-id="0c5db-157">Under Containers, double-click **datacontainer**.</span></span>
3. <span data-ttu-id="0c5db-158">Pokud chcete prozkoumat cestu ke svým datům, klikněte na složku **datedimension**. Zobrazí se nahraný soubor **DimDate2.txt**.</span><span class="sxs-lookup"><span data-stu-id="0c5db-158">To explore the path to your data, click the folder **datedimension** and you will see your uploaded file **DimDate2.txt**.</span></span>
4. <span data-ttu-id="0c5db-159">Pokud chcete zobrazit vlastnosti, klikněte na **DimDate2.txt**.</span><span class="sxs-lookup"><span data-stu-id="0c5db-159">To view properties, click **DimDate2.txt**.</span></span>
5. <span data-ttu-id="0c5db-160">Poznámka: V okně vlastností objektu blob můžete stáhnout nebo odstranit soubor.</span><span class="sxs-lookup"><span data-stu-id="0c5db-160">Note that in the Blob properties blade, you can download or delete the file.</span></span>
   
    ![Zobrazení objektu blob úložiště Azure](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)

## <a name="step-2-create-an-external-table-for-the-sample-data"></a><span data-ttu-id="0c5db-162">Krok 2: Vytvoření externí tabulky pro ukázková data</span><span class="sxs-lookup"><span data-stu-id="0c5db-162">Step 2: Create an external table for the sample data</span></span>
<span data-ttu-id="0c5db-163">V této části vytvoříme externí tabulku, která definuje ukázková data.</span><span class="sxs-lookup"><span data-stu-id="0c5db-163">In this section we create an external table that defines the sample data.</span></span>

<span data-ttu-id="0c5db-164">Funkce PolyBase používá pro přístup k datům v Azure Blob Storage externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="0c5db-164">PolyBase uses external tables to access data in Azure blob storage.</span></span> <span data-ttu-id="0c5db-165">Vzhledem k tomu, že data nejsou uložená v SQL Data Warehouse, zpracovává PolyBase ověřování pro externí data pomocí přihlašovacích údajů platných pro databázi.</span><span class="sxs-lookup"><span data-stu-id="0c5db-165">Since the data is not stored within SQL Data Warehouse, PolyBase handles authentication to the external data by using a database-scoped credential.</span></span>

<span data-ttu-id="0c5db-166">V příkladě v tomto kroku se k vytvoření externí tabulky používají následující příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="0c5db-166">The example in this step uses these Transact-SQL statements to create an external table.</span></span>

* <span data-ttu-id="0c5db-167">[Create Master Key (Transact-SQL)][Create Master Key (Transact-SQL)] k šifrování tajného klíče vašich přihlašovacích údajů pro vaši databázi</span><span class="sxs-lookup"><span data-stu-id="0c5db-167">[Create Master Key (Transact-SQL)][Create Master Key (Transact-SQL)] to encrypt the secret of your database scoped credential.</span></span>
* <span data-ttu-id="0c5db-168">[Create Database Scoped Credential (Transact-SQL)][Create Database Scoped Credential (Transact-SQL)] k zadání ověřovacích informací pro váš účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="0c5db-168">[Create Database Scoped Credential (Transact-SQL)][Create Database Scoped Credential (Transact-SQL)] to specify authentication information for your Azure storage account.</span></span>
* <span data-ttu-id="0c5db-169">[Create External Data Source (Transact-SQL)][Create External Data Source (Transact-SQL)] k určení umístění vaší služby Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="0c5db-169">[Create External Data Source (Transact-SQL)][Create External Data Source (Transact-SQL)] to specify the location of your Azure blob storage.</span></span>
* <span data-ttu-id="0c5db-170">[Create External File Format (Transact-SQL)][Create External File Format (Transact-SQL)] k určení formátu vašich dat</span><span class="sxs-lookup"><span data-stu-id="0c5db-170">[Create External File Format (Transact-SQL)][Create External File Format (Transact-SQL)] to specify the format of your data.</span></span>
* <span data-ttu-id="0c5db-171">[Create External Table (Transact-SQL)][Create External Table (Transact-SQL)] k určení definice tabulky a umístění dat</span><span class="sxs-lookup"><span data-stu-id="0c5db-171">[Create External Table (Transact-SQL)][Create External Table (Transact-SQL)] to specify the table definition and location of the data.</span></span>

<span data-ttu-id="0c5db-172">Spusťte tento dotaz na databázi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0c5db-172">Run this query against your SQL Data Warehouse database.</span></span> <span data-ttu-id="0c5db-173">Ten ve schématu dbo vytvoří externí tabulku s názvem DimDate2External, která odkazuje na ukázková data DimDate2.txt v Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="0c5db-173">It will create an external table named DimDate2External in the dbo schema that points to the DimDate2.txt sample data in the Azure blob storage.</span></span>

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

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


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

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


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


<span data-ttu-id="0c5db-174">V Průzkumníku objektů systému SQL Server v sadě Visual Studio uvidíte formát externích souborů, externí zdroj dat a tabulku DimDate2External.</span><span class="sxs-lookup"><span data-stu-id="0c5db-174">In SQL Server Object Explorer in Visual Studio, you can see the external file format, external data source, and the DimDate2External table.</span></span>

![Zobrazení externí tabulky](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a><span data-ttu-id="0c5db-176">Krok 3: Načtení dat do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0c5db-176">Step 3: Load data into SQL Data Warehouse</span></span>
<span data-ttu-id="0c5db-177">Po vytvoření externí tabulky můžete buď načíst data do nové tabulky, nebo je vložit do existující tabulky.</span><span class="sxs-lookup"><span data-stu-id="0c5db-177">Once the external table is created, you can either load the data into a new table or insert it into an existing table.</span></span>

* <span data-ttu-id="0c5db-178">Pokud chcete načíst data do nové tabulky, spusťte příkaz [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span><span class="sxs-lookup"><span data-stu-id="0c5db-178">To load the data into a new table, run the [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="0c5db-179">Nová tabulka bude mít sloupce pojmenované v dotazu.</span><span class="sxs-lookup"><span data-stu-id="0c5db-179">The new table will have the columns named in the query.</span></span> <span data-ttu-id="0c5db-180">Datové typy sloupců budou odpovídat datovým typům v definici externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="0c5db-180">The data types of the columns will match the data types in the external table definition.</span></span>
* <span data-ttu-id="0c5db-181">Pokud chcete načíst data do existující tabulky, použijte příkaz [INSERT...SELECT (Transact-SQL)][INSERT...SELECT (Transact-SQL)].</span><span class="sxs-lookup"><span data-stu-id="0c5db-181">To load the data into an existing table, use the [INSERT...SELECT (Transact-SQL)][INSERT...SELECT (Transact-SQL)] statement.</span></span>

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="0c5db-182">Krok 4: Vytvoření statistiky pro nově načtená data</span><span class="sxs-lookup"><span data-stu-id="0c5db-182">Step 4: Create statistics on your newly loaded data</span></span>
<span data-ttu-id="0c5db-183">SQL Data Warehouse nevytváří ani neaktualizuje statistiku automaticky.</span><span class="sxs-lookup"><span data-stu-id="0c5db-183">SQL Data Warehouse does not auto-create or auto-update statistics.</span></span> <span data-ttu-id="0c5db-184">Pro dosažení vysokého výkonu dotazu je proto důležité vytvořit statistiku pro každý sloupec každé tabulky po prvním načtení.</span><span class="sxs-lookup"><span data-stu-id="0c5db-184">Therefore, to achieve high query performance, it's important to create statistics on each column of each table after the first load.</span></span> <span data-ttu-id="0c5db-185">Důležité je také aktualizovat statistiku po důležitých změnách v datech.</span><span class="sxs-lookup"><span data-stu-id="0c5db-185">It's also important to update statistics after substantial changes in the data.</span></span>

<span data-ttu-id="0c5db-186">Tento příklad vytvoří jednosloupcovou statistiku pro novou tabulku DimDate2.</span><span class="sxs-lookup"><span data-stu-id="0c5db-186">This example creates single-column statistics on the new DimDate2 table.</span></span>

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

<span data-ttu-id="0c5db-187">Další informace viz [Statistika][Statistics].</span><span class="sxs-lookup"><span data-stu-id="0c5db-187">To learn more, see [Statistics][Statistics].</span></span>  

## <a name="next-steps"></a><span data-ttu-id="0c5db-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0c5db-188">Next steps</span></span>
<span data-ttu-id="0c5db-189">Projděte si [průvodce funkcí PolyBase][PolyBase guide], kde najdete další informace, které byste měli mít, když budete vyvíjet řešení využívající funkci PolyBase.</span><span class="sxs-lookup"><span data-stu-id="0c5db-189">See the [PolyBase guide][PolyBase guide] for further information you should know as you develop a solution that uses PolyBase.</span></span>

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
