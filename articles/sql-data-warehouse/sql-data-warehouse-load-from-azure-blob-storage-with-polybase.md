---
title: "aaaLoad z Azure blob tooAzure datového skladu | Microsoft Docs"
description: "Zjistěte, jak toouse PolyBase tooload dat z Azure blob storage do SQL Data Warehouse. Načtení několik tabulek z veřejná data do schématu hello Contoso prodejní datového skladu."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: faca0fe7-62e7-4e1f-a86f-032b4ffcb06e
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 4b4978ccefa4d55ff5c89fba84c5e705422ddbb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a><span data-ttu-id="521dd-104">Načtení dat z Azure blob storage do SQL Data Warehouse (PolyBase)</span><span class="sxs-lookup"><span data-stu-id="521dd-104">Load data from Azure blob storage into SQL Data Warehouse (PolyBase)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="521dd-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="521dd-105">Data Factory</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [<span data-ttu-id="521dd-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="521dd-106">PolyBase</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

<span data-ttu-id="521dd-107">Použijte PolyBase a T-SQL příkazy tooload dat z Azure blob storage do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="521dd-107">Use PolyBase and T-SQL commands tooload data from Azure blob storage into Azure SQL Data Warehouse.</span></span> 

<span data-ttu-id="521dd-108">tookeep jednoduchý, v tomto kurzu načte dvou tabulek z veřejné Azure Blob Storage do schématu hello Contoso prodejní datového skladu.</span><span class="sxs-lookup"><span data-stu-id="521dd-108">tookeep it simple, this tutorial loads two tables from a public Azure Storage Blob into hello Contoso Retail Data Warehouse schema.</span></span> <span data-ttu-id="521dd-109">tooload hello úplné datové sady, spusťte hello příklad [zatížení hello úplné Contoso prodejní datového skladu] [ Load hello full Contoso Retail Data Warehouse] z úložiště hello ukázky Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="521dd-109">tooload hello full data set, run hello example [Load hello full Contoso Retail Data Warehouse][Load hello full Contoso Retail Data Warehouse] from hello Microsoft SQL Server Samples repository.</span></span>

<span data-ttu-id="521dd-110">V tomto kurzu provedete následující:</span><span class="sxs-lookup"><span data-stu-id="521dd-110">In this tutorial you will:</span></span>

1. <span data-ttu-id="521dd-111">Konfigurace PolyBase tooload z Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="521dd-111">Configure PolyBase tooload from Azure blob storage</span></span>
2. <span data-ttu-id="521dd-112">Načíst veřejná data do databáze</span><span class="sxs-lookup"><span data-stu-id="521dd-112">Load public data into your database</span></span>
3. <span data-ttu-id="521dd-113">Po dokončení hello zatížení, provedení optimalizace.</span><span class="sxs-lookup"><span data-stu-id="521dd-113">Perform optimizations after hello load is finished.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="521dd-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="521dd-114">Before you begin</span></span>
<span data-ttu-id="521dd-115">toorun tohoto kurzu potřebujete účet Azure, která již obsahuje databázi SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="521dd-115">toorun this tutorial, you need an Azure account that already has a SQL Data Warehouse database.</span></span> <span data-ttu-id="521dd-116">Pokud to nemáte, přečtěte si téma [vytvořit SQL Data Warehouse][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="521dd-116">If you don't already have this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>

## <a name="1-configure-hello-data-source"></a><span data-ttu-id="521dd-117">1. Konfigurace zdroje dat hello</span><span class="sxs-lookup"><span data-stu-id="521dd-117">1. Configure hello data source</span></span>
<span data-ttu-id="521dd-118">PolyBase používá T-SQL externí objekty toodefine hello umístění a atributy externích dat hello.</span><span class="sxs-lookup"><span data-stu-id="521dd-118">PolyBase uses T-SQL external objects toodefine hello location and attributes of hello external data.</span></span> <span data-ttu-id="521dd-119">definice externího objektu Hello jsou uloženy v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="521dd-119">hello external object definitions are stored in SQL Data Warehouse.</span></span> <span data-ttu-id="521dd-120">samotná data Hello je uložených externě.</span><span class="sxs-lookup"><span data-stu-id="521dd-120">hello data itself is stored externally.</span></span>

### <a name="11-create-a-credential"></a><span data-ttu-id="521dd-121">1.1.</span><span class="sxs-lookup"><span data-stu-id="521dd-121">1.1.</span></span> <span data-ttu-id="521dd-122">Vytvoření přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="521dd-122">Create a credential</span></span>
<span data-ttu-id="521dd-123">**Tento krok přeskočit** při zavádění hello Contoso veřejná data.</span><span class="sxs-lookup"><span data-stu-id="521dd-123">**Skip this step** if you are loading hello Contoso public data.</span></span> <span data-ttu-id="521dd-124">Protože je již dostupný tooanyone nepotřebujete veřejná data toohello zabezpečený přístup.</span><span class="sxs-lookup"><span data-stu-id="521dd-124">You don't need secure access toohello public data since it is already accessible tooanyone.</span></span>

<span data-ttu-id="521dd-125">**Není tento krok přeskočit** Pokud používáte v tomto kurzu jako šablona pro načítání svoje vlastní data.</span><span class="sxs-lookup"><span data-stu-id="521dd-125">**Don't skip this step** if you are using this tutorial as a template for loading your own data.</span></span> <span data-ttu-id="521dd-126">tooaccess data pomocí přihlašovacích údajů, použijte hello následující skript toocreate databáze obor pověření a pak jeho pomocí při definování hello umístění zdroje dat hello.</span><span class="sxs-lookup"><span data-stu-id="521dd-126">tooaccess data through a credential, use hello following script toocreate a database-scoped credential, and then use it when defining hello location of hello data source.</span></span>

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
```

<span data-ttu-id="521dd-127">Přeskočte toostep 2.</span><span class="sxs-lookup"><span data-stu-id="521dd-127">Skip toostep 2.</span></span>

### <a name="12-create-hello-external-data-source"></a><span data-ttu-id="521dd-128">1.2.</span><span class="sxs-lookup"><span data-stu-id="521dd-128">1.2.</span></span> <span data-ttu-id="521dd-129">Vytvoření hello externí zdroj dat</span><span class="sxs-lookup"><span data-stu-id="521dd-129">Create hello external data source</span></span>
<span data-ttu-id="521dd-130">Použít [vytvořit externí zdroj dat] [ CREATE EXTERNAL DATA SOURCE] příkaz toostore hello umístění hello dat a datový typ hello.</span><span class="sxs-lookup"><span data-stu-id="521dd-130">Use this [CREATE EXTERNAL DATA SOURCE][CREATE EXTERNAL DATA SOURCE] command toostore hello location of hello data, and hello type of data.</span></span> 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [!IMPORTANT]
> <span data-ttu-id="521dd-131">Pokud si zvolíte toomake vaše veřejné kontejnery úložiště objektů blob v azure, mějte na paměti, že jako vlastník dat hello vám bude účtována pro data s nimi spojeným nákladům při data ponechá hello datového centra.</span><span class="sxs-lookup"><span data-stu-id="521dd-131">If you choose toomake your azure blob storage containers public, remember that as hello data owner you will be charged for data egress charges when data leaves hello data center.</span></span> 
> 
> 

## <a name="2-configure-data-format"></a><span data-ttu-id="521dd-132">2. Konfigurovat formát dat</span><span class="sxs-lookup"><span data-stu-id="521dd-132">2. Configure data format</span></span>
<span data-ttu-id="521dd-133">Hello data jsou uložena v textových souborů v Azure blob storage a každé pole je oddělená s oddělovačem.</span><span class="sxs-lookup"><span data-stu-id="521dd-133">hello data is stored in text files in Azure blob storage, and each field is separated with a delimiter.</span></span> <span data-ttu-id="521dd-134">To [vytvořit EXTERNAL FILE FORMAT] [ CREATE EXTERNAL FILE FORMAT] příkaz toospecify hello formát data hello hello textových souborů.</span><span class="sxs-lookup"><span data-stu-id="521dd-134">Run this [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT] command toospecify hello format of hello data in hello text files.</span></span> <span data-ttu-id="521dd-135">nekomprimované Hello data společnosti Contoso a oddělený kanálu.</span><span class="sxs-lookup"><span data-stu-id="521dd-135">hello Contoso data is uncompressed and pipe delimited.</span></span>

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-hello-external-tables"></a><span data-ttu-id="521dd-136">3. Vytvoření hello externí tabulky</span><span class="sxs-lookup"><span data-stu-id="521dd-136">3. Create hello external tables</span></span>
<span data-ttu-id="521dd-137">Teď, když jste zadali hello datového zdroje a formát souboru, jste připravené toocreate hello externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="521dd-137">Now that you have specified hello data source and file format, you are ready toocreate hello external tables.</span></span> 

### <a name="31-create-a-schema-for-hello-data"></a><span data-ttu-id="521dd-138">3.1.</span><span class="sxs-lookup"><span data-stu-id="521dd-138">3.1.</span></span> <span data-ttu-id="521dd-139">Vytvořte schéma pro hello data.</span><span class="sxs-lookup"><span data-stu-id="521dd-139">Create a schema for hello data.</span></span>
<span data-ttu-id="521dd-140">toocreate místě toostore hello Contoso data z databáze, vytvořte schéma.</span><span class="sxs-lookup"><span data-stu-id="521dd-140">toocreate a place toostore hello Contoso data in your database, create a schema.</span></span>

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-hello-external-tables"></a><span data-ttu-id="521dd-141">3.2.</span><span class="sxs-lookup"><span data-stu-id="521dd-141">3.2.</span></span> <span data-ttu-id="521dd-142">Vytvořte hello externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="521dd-142">Create hello external tables.</span></span>
<span data-ttu-id="521dd-143">Spusťte tento skript toocreate hello DimProduct a FactOnlineSales externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="521dd-143">Run this script toocreate hello DimProduct and FactOnlineSales external tables.</span></span> <span data-ttu-id="521dd-144">Všechny, které jsme to děláte, zde je definování názvy sloupců a datové typy a vazba je toohello umístění a formát souborů úložiště objektů blob v Azure hello.</span><span class="sxs-lookup"><span data-stu-id="521dd-144">All we are doing here is defining column names and data types, and binding them toohello location and format of hello Azure blob storage files.</span></span> <span data-ttu-id="521dd-145">definice Hello je uložená v SQL Data Warehouse a hello dat je stále v hello Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="521dd-145">hello definition is stored in SQL Data Warehouse and hello data is still in hello Azure Storage Blob.</span></span>

<span data-ttu-id="521dd-146">Hello **umístění** parametr je hello složky v kořenové složce hello v hello Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="521dd-146">hello  **LOCATION** parameter is hello folder under hello root folder in hello Azure Storage Blob.</span></span> <span data-ttu-id="521dd-147">Každá tabulka je v jiné složce.</span><span class="sxs-lookup"><span data-stu-id="521dd-147">Each table is in a different folder.</span></span>

```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-hello-data"></a><span data-ttu-id="521dd-148">4. Načtení dat hello</span><span class="sxs-lookup"><span data-stu-id="521dd-148">4. Load hello data</span></span>
<span data-ttu-id="521dd-149">Není k dispozici různé způsoby tooaccess externí data.</span><span class="sxs-lookup"><span data-stu-id="521dd-149">There's different ways tooaccess external data.</span></span>  <span data-ttu-id="521dd-150">Můžete zadávat dotazy na data přímo z externí tabulky hello, načtení dat hello do nové tabulky databáze nebo přidat externí data tooexisting databázových tabulek.</span><span class="sxs-lookup"><span data-stu-id="521dd-150">You can query data directly from hello external table, load hello data into new database tables, or add external data tooexisting database tables.</span></span>  

### <a name="41-create-a-new-schema"></a><span data-ttu-id="521dd-151">4.1.</span><span class="sxs-lookup"><span data-stu-id="521dd-151">4.1.</span></span> <span data-ttu-id="521dd-152">Vytvořit nové schéma</span><span class="sxs-lookup"><span data-stu-id="521dd-152">Create a new schema</span></span>
<span data-ttu-id="521dd-153">Funkce CTAS vytvoří novou tabulku, která obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="521dd-153">CTAS creates a new table that contains data.</span></span>  <span data-ttu-id="521dd-154">Nejprve vytvořte schéma pro data společnosti contoso hello.</span><span class="sxs-lookup"><span data-stu-id="521dd-154">First, create a schema for hello contoso data.</span></span>

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-hello-data-into-new-tables"></a><span data-ttu-id="521dd-155">4.2.</span><span class="sxs-lookup"><span data-stu-id="521dd-155">4.2.</span></span> <span data-ttu-id="521dd-156">Načtení dat hello do nové tabulky</span><span class="sxs-lookup"><span data-stu-id="521dd-156">Load hello data into new tables</span></span>
<span data-ttu-id="521dd-157">tooload dat z Azure úložiště objektů blob a uložte ho v tabulce uvnitř vaší databáze, použijte hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] příkaz.</span><span class="sxs-lookup"><span data-stu-id="521dd-157">tooload data from Azure blob storage and save it in a table inside of your database, use hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="521dd-158">Načtení se funkce CTAS využívá hello silného typu externí tabulky se právě created.tooload hello data do nové tabulky, použijte jednu [funkce CTAS] [ CTAS] příkaz na jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="521dd-158">Loading with CTAS leverages hello strongly typed external tables you have just created.tooload hello data into new tables, use one [CTAS][CTAS] statement per table.</span></span> 
 
<span data-ttu-id="521dd-159">Funkce CTAS vytvoří novou tabulku a naplní s hello výsledky příkazu select.</span><span class="sxs-lookup"><span data-stu-id="521dd-159">CTAS creates a new table and populates it with hello results of a select statement.</span></span> <span data-ttu-id="521dd-160">Funkce CTAS definuje hello nové tabulky toohave hello stejnou sloupce a datové typy jako hello výsledky hello vyberte příkaz.</span><span class="sxs-lookup"><span data-stu-id="521dd-160">CTAS defines hello new table toohave hello same columns and data types as hello results of hello select statement.</span></span> <span data-ttu-id="521dd-161">Pokud vyberete všechny sloupce hello z externí tabulky, bude nová tabulka hello repliku hello sloupce a typy dat v hello externí tabulky.</span><span class="sxs-lookup"><span data-stu-id="521dd-161">If you select all hello columns from an external table, hello new table will be a replica of hello columns and data types in hello external table.</span></span>

<span data-ttu-id="521dd-162">V tomto příkladu vytvoříme hello dimenze a tabulky faktů hello jako hash distribuované tabulky.</span><span class="sxs-lookup"><span data-stu-id="521dd-162">In this example, we create both hello dimension and hello fact table as hash distributed tables.</span></span> 

```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-hello-load-progress"></a><span data-ttu-id="521dd-163">4.3 sledovat průběh zatížení hello</span><span class="sxs-lookup"><span data-stu-id="521dd-163">4.3 Track hello load progress</span></span>
<span data-ttu-id="521dd-164">Můžete sledovat průběh hello zatížení pomocí zobrazení dynamické správy (zobrazení dynamické správy).</span><span class="sxs-lookup"><span data-stu-id="521dd-164">You can track hello progress of your load using dynamic management views (DMVs).</span></span> 

```sql
-- toosee all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- toosee a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- tootrack bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a><span data-ttu-id="521dd-165">5. Optimalizace columnstore komprese</span><span class="sxs-lookup"><span data-stu-id="521dd-165">5. Optimize columnstore compression</span></span>
<span data-ttu-id="521dd-166">Ve výchozím nastavení ukládá SQL Data Warehouse tabulku hello jako clusterovaný index columnstore.</span><span class="sxs-lookup"><span data-stu-id="521dd-166">By default, SQL Data Warehouse stores hello table as a clustered columnstore index.</span></span> <span data-ttu-id="521dd-167">Po dokončení zatížení, nemusí některé řádky dat hello komprimované do hello columnstore.</span><span class="sxs-lookup"><span data-stu-id="521dd-167">After a load completes, some of hello data rows might not be compressed into hello columnstore.</span></span>  <span data-ttu-id="521dd-168">Je z různých důvodů, proč k tomu může dojít.</span><span class="sxs-lookup"><span data-stu-id="521dd-168">There's a variety of reasons why this can happen.</span></span> <span data-ttu-id="521dd-169">Další, najdete v části toolearn [spravovat indexy columnstore][manage columnstore indexes].</span><span class="sxs-lookup"><span data-stu-id="521dd-169">toolearn more, see [manage columnstore indexes][manage columnstore indexes].</span></span>

<span data-ttu-id="521dd-170">toooptimize výkon dotazů a komprese columnstore po zatížení, znovu sestavit index columnstore toocompress pro hello tabulky tooforce hello všechny řádky hello.</span><span class="sxs-lookup"><span data-stu-id="521dd-170">toooptimize query performance and columnstore compression after a load, rebuild hello table tooforce hello columnstore index toocompress all hello rows.</span></span> 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

<span data-ttu-id="521dd-171">Další informace o údržbě indexů columnstore najdete v tématu hello [spravovat indexy columnstore] [ manage columnstore indexes] článku.</span><span class="sxs-lookup"><span data-stu-id="521dd-171">For more information on maintaining columnstore indexes, see hello [manage columnstore indexes][manage columnstore indexes] article.</span></span>

## <a name="6-optimize-statistics"></a><span data-ttu-id="521dd-172">6. Optimalizace statistiky</span><span class="sxs-lookup"><span data-stu-id="521dd-172">6. Optimize statistics</span></span>
<span data-ttu-id="521dd-173">Je nejlepší toocreate jednosloupcovou statistiku ihned po zatížení.</span><span class="sxs-lookup"><span data-stu-id="521dd-173">It is best toocreate single-column statistics immediately after a load.</span></span> <span data-ttu-id="521dd-174">Existuje několik možností pro statistiky.</span><span class="sxs-lookup"><span data-stu-id="521dd-174">There are some choices for statistics.</span></span> <span data-ttu-id="521dd-175">Například pokud vytvoříte jednosloupcovou statistiku pro každý sloupec může trvat dlouhou dobu toorebuild všechny statistické údaje o hello.</span><span class="sxs-lookup"><span data-stu-id="521dd-175">For example, if you create single-column statistics on every column it might take a long time toorebuild all hello statistics.</span></span> <span data-ttu-id="521dd-176">Pokud víte, že některé sloupce nebudete toobe v predikátech dotazu, můžete přeskočit vytvoření statistiky pro tyto sloupce.</span><span class="sxs-lookup"><span data-stu-id="521dd-176">If you know certain columns are not going toobe in query predicates, you can skip creating statistics on those columns.</span></span>

<span data-ttu-id="521dd-177">Pokud se rozhodnete toocreate jednosloupcovou statistiku pro každý sloupec každé tabulky, můžete použít ukázka kódu uložené procedury hello `prc_sqldw_create_stats` v hello [statistiky] [ statistics] článku.</span><span class="sxs-lookup"><span data-stu-id="521dd-177">If you decide toocreate single-column statistics on every column of every table, you can use hello stored procedure code sample `prc_sqldw_create_stats` in hello [statistics][statistics] article.</span></span>

<span data-ttu-id="521dd-178">Následující ukázka Hello je to dobrý výchozí bod pro vytvoření statistiky.</span><span class="sxs-lookup"><span data-stu-id="521dd-178">hello following example is a good starting point for creating statistics.</span></span> <span data-ttu-id="521dd-179">Vytvoří jednosloupcovou statistiku pro každý sloupec v tabulce dimenze hello a na každém spojující sloupci tabulky faktů hello.</span><span class="sxs-lookup"><span data-stu-id="521dd-179">It creates single-column statistics on each column in hello dimension table, and on each joining column in hello fact tables.</span></span> <span data-ttu-id="521dd-180">Sloupců tabulky faktů tooother statistiky jeden nebo více sloupci můžete vždy přidat později.</span><span class="sxs-lookup"><span data-stu-id="521dd-180">You can always add single or multi-column statistics tooother fact table columns later on.</span></span>

```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a><span data-ttu-id="521dd-181">Dosažení odemčený!</span><span class="sxs-lookup"><span data-stu-id="521dd-181">Achievement unlocked!</span></span>
<span data-ttu-id="521dd-182">Veřejná data mají úspěšně načíst do Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="521dd-182">You have successfully loaded public data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="521dd-183">Skvělá práce!</span><span class="sxs-lookup"><span data-stu-id="521dd-183">Great job!</span></span>

<span data-ttu-id="521dd-184">Nyní můžete spustit dotazování tabulky hello pomocí dotazů jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="521dd-184">You can now start querying hello tables using queries like hello following:</span></span>

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a><span data-ttu-id="521dd-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="521dd-185">Next steps</span></span>
<span data-ttu-id="521dd-186">tooload hello úplné Contoso prodejní dat datového skladu, použijte skript hello v další tipy pro vývoj, najdete v části [přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="521dd-186">tooload hello full Contoso Retail Data Warehouse data, use hello script in For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[manage columnstore indexes]: sql-data-warehouse-tables-index.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Load hello full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
