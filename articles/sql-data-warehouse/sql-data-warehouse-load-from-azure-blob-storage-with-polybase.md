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
# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Načtení dat z Azure blob storage do SQL Data Warehouse (PolyBase)
> [!div class="op_single_selector"]
> * [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

Použijte PolyBase a T-SQL příkazy tooload dat z Azure blob storage do Azure SQL Data Warehouse. 

tookeep jednoduchý, v tomto kurzu načte dvou tabulek z veřejné Azure Blob Storage do schématu hello Contoso prodejní datového skladu. tooload hello úplné datové sady, spusťte hello příklad [zatížení hello úplné Contoso prodejní datového skladu] [ Load hello full Contoso Retail Data Warehouse] z úložiště hello ukázky Microsoft SQL Server.

V tomto kurzu provedete následující:

1. Konfigurace PolyBase tooload z Azure blob storage
2. Načíst veřejná data do databáze
3. Po dokončení hello zatížení, provedení optimalizace.

## <a name="before-you-begin"></a>Než začnete
toorun tohoto kurzu potřebujete účet Azure, která již obsahuje databázi SQL Data Warehouse. Pokud to nemáte, přečtěte si téma [vytvořit SQL Data Warehouse][Create a SQL Data Warehouse].

## <a name="1-configure-hello-data-source"></a>1. Konfigurace zdroje dat hello
PolyBase používá T-SQL externí objekty toodefine hello umístění a atributy externích dat hello. definice externího objektu Hello jsou uloženy v SQL Data Warehouse. samotná data Hello je uložených externě.

### <a name="11-create-a-credential"></a>1.1. Vytvoření přihlašovacích údajů
**Tento krok přeskočit** při zavádění hello Contoso veřejná data. Protože je již dostupný tooanyone nepotřebujete veřejná data toohello zabezpečený přístup.

**Není tento krok přeskočit** Pokud používáte v tomto kurzu jako šablona pro načítání svoje vlastní data. tooaccess data pomocí přihlašovacích údajů, použijte hello následující skript toocreate databáze obor pověření a pak jeho pomocí při definování hello umístění zdroje dat hello.

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

Přeskočte toostep 2.

### <a name="12-create-hello-external-data-source"></a>1.2. Vytvoření hello externí zdroj dat
Použít [vytvořit externí zdroj dat] [ CREATE EXTERNAL DATA SOURCE] příkaz toostore hello umístění hello dat a datový typ hello. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [!IMPORTANT]
> Pokud si zvolíte toomake vaše veřejné kontejnery úložiště objektů blob v azure, mějte na paměti, že jako vlastník dat hello vám bude účtována pro data s nimi spojeným nákladům při data ponechá hello datového centra. 
> 
> 

## <a name="2-configure-data-format"></a>2. Konfigurovat formát dat
Hello data jsou uložena v textových souborů v Azure blob storage a každé pole je oddělená s oddělovačem. To [vytvořit EXTERNAL FILE FORMAT] [ CREATE EXTERNAL FILE FORMAT] příkaz toospecify hello formát data hello hello textových souborů. nekomprimované Hello data společnosti Contoso a oddělený kanálu.

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

## <a name="3-create-hello-external-tables"></a>3. Vytvoření hello externí tabulky
Teď, když jste zadali hello datového zdroje a formát souboru, jste připravené toocreate hello externí tabulky. 

### <a name="31-create-a-schema-for-hello-data"></a>3.1. Vytvořte schéma pro hello data.
toocreate místě toostore hello Contoso data z databáze, vytvořte schéma.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-hello-external-tables"></a>3.2. Vytvořte hello externí tabulky.
Spusťte tento skript toocreate hello DimProduct a FactOnlineSales externí tabulky. Všechny, které jsme to děláte, zde je definování názvy sloupců a datové typy a vazba je toohello umístění a formát souborů úložiště objektů blob v Azure hello. definice Hello je uložená v SQL Data Warehouse a hello dat je stále v hello Azure Storage Blob.

Hello **umístění** parametr je hello složky v kořenové složce hello v hello Azure Storage Blob. Každá tabulka je v jiné složce.

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

## <a name="4-load-hello-data"></a>4. Načtení dat hello
Není k dispozici různé způsoby tooaccess externí data.  Můžete zadávat dotazy na data přímo z externí tabulky hello, načtení dat hello do nové tabulky databáze nebo přidat externí data tooexisting databázových tabulek.  

### <a name="41-create-a-new-schema"></a>4.1. Vytvořit nové schéma
Funkce CTAS vytvoří novou tabulku, která obsahuje data.  Nejprve vytvořte schéma pro data společnosti contoso hello.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-hello-data-into-new-tables"></a>4.2. Načtení dat hello do nové tabulky
tooload dat z Azure úložiště objektů blob a uložte ho v tabulce uvnitř vaší databáze, použijte hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] příkaz. Načtení se funkce CTAS využívá hello silného typu externí tabulky se právě created.tooload hello data do nové tabulky, použijte jednu [funkce CTAS] [ CTAS] příkaz na jednu tabulku. 
 
Funkce CTAS vytvoří novou tabulku a naplní s hello výsledky příkazu select. Funkce CTAS definuje hello nové tabulky toohave hello stejnou sloupce a datové typy jako hello výsledky hello vyberte příkaz. Pokud vyberete všechny sloupce hello z externí tabulky, bude nová tabulka hello repliku hello sloupce a typy dat v hello externí tabulky.

V tomto příkladu vytvoříme hello dimenze a tabulky faktů hello jako hash distribuované tabulky. 

```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-hello-load-progress"></a>4.3 sledovat průběh zatížení hello
Můžete sledovat průběh hello zatížení pomocí zobrazení dynamické správy (zobrazení dynamické správy). 

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

## <a name="5-optimize-columnstore-compression"></a>5. Optimalizace columnstore komprese
Ve výchozím nastavení ukládá SQL Data Warehouse tabulku hello jako clusterovaný index columnstore. Po dokončení zatížení, nemusí některé řádky dat hello komprimované do hello columnstore.  Je z různých důvodů, proč k tomu může dojít. Další, najdete v části toolearn [spravovat indexy columnstore][manage columnstore indexes].

toooptimize výkon dotazů a komprese columnstore po zatížení, znovu sestavit index columnstore toocompress pro hello tabulky tooforce hello všechny řádky hello. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Další informace o údržbě indexů columnstore najdete v tématu hello [spravovat indexy columnstore] [ manage columnstore indexes] článku.

## <a name="6-optimize-statistics"></a>6. Optimalizace statistiky
Je nejlepší toocreate jednosloupcovou statistiku ihned po zatížení. Existuje několik možností pro statistiky. Například pokud vytvoříte jednosloupcovou statistiku pro každý sloupec může trvat dlouhou dobu toorebuild všechny statistické údaje o hello. Pokud víte, že některé sloupce nebudete toobe v predikátech dotazu, můžete přeskočit vytvoření statistiky pro tyto sloupce.

Pokud se rozhodnete toocreate jednosloupcovou statistiku pro každý sloupec každé tabulky, můžete použít ukázka kódu uložené procedury hello `prc_sqldw_create_stats` v hello [statistiky] [ statistics] článku.

Následující ukázka Hello je to dobrý výchozí bod pro vytvoření statistiky. Vytvoří jednosloupcovou statistiku pro každý sloupec v tabulce dimenze hello a na každém spojující sloupci tabulky faktů hello. Sloupců tabulky faktů tooother statistiky jeden nebo více sloupci můžete vždy přidat později.

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

## <a name="achievement-unlocked"></a>Dosažení odemčený!
Veřejná data mají úspěšně načíst do Azure SQL Data Warehouse. Skvělá práce!

Nyní můžete spustit dotazování tabulky hello pomocí dotazů jako hello následující:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Další kroky
tooload hello úplné Contoso prodejní dat datového skladu, použijte skript hello v další tipy pro vývoj, najdete v části [přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].

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
