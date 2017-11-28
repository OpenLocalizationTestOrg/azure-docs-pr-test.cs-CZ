---
title: Tabulka aaaCreate jako vyberte (funkce CTAS) v SQL Data Warehouse | Microsoft Docs
description: "Tipy pro psaní kódu s použitím hello vytvořit tabulku jako vyberte příkaz (funkce CTAS) v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 68ac9a94-09f9-424b-b536-06a125a653bd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 01/30/2017
ms.author: shigu;barbkess
ms.openlocfilehash: e381601a0a4d94e189d8f9115bf2e7593025410b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a><span data-ttu-id="8788f-103">Vytvoření tabulky jako vyberte (funkce CTAS) v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="8788f-103">Create Table As Select (CTAS) in SQL Data Warehouse</span></span>
<span data-ttu-id="8788f-104">Vytvoření tabulky jako vyberte nebo `CTAS` je jedním z hello nejdůležitější funkce T-SQL dostupné.</span><span class="sxs-lookup"><span data-stu-id="8788f-104">Create table as select or `CTAS` is one of hello most important T-SQL features available.</span></span> <span data-ttu-id="8788f-105">Je plně parallelized operaci, která vytvoří novou tabulku podle hello výstup příkazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="8788f-105">It is a fully parallelized operation that creates a new table based on hello output of a SELECT statement.</span></span> <span data-ttu-id="8788f-106">`CTAS`je snadno a rychle toocreate hello kopii tabulky.</span><span class="sxs-lookup"><span data-stu-id="8788f-106">`CTAS` is hello simplest and fastest way toocreate a copy of a table.</span></span> <span data-ttu-id="8788f-107">Tento dokument obsahuje příklady a osvědčené postupy pro `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="8788f-107">This document provides both examples and best practices for `CTAS`.</span></span>

## <a name="selectinto-vs-ctas"></a><span data-ttu-id="8788f-108">VYBERTE... DO vs. CTAS</span><span class="sxs-lookup"><span data-stu-id="8788f-108">SELECT..INTO vs. CTAS</span></span>
<span data-ttu-id="8788f-109">Můžete zvážit `CTAS` jako extrémně účtovat verze `SELECT..INTO`.</span><span class="sxs-lookup"><span data-stu-id="8788f-109">You can consider `CTAS` as a super-charged version of `SELECT..INTO`.</span></span>

<span data-ttu-id="8788f-110">Dole je příklad jednoduchou `SELECT..INTO` příkaz:</span><span class="sxs-lookup"><span data-stu-id="8788f-110">Below is an example of a simple `SELECT..INTO` statement:</span></span>

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

<span data-ttu-id="8788f-111">V předchozím příkladu hello `[dbo].[FactInternetSales_new]` by se vytvořily jako ROUND_ROBIN distribuované tabulku s INDEXEM COLUMNSTORE v clusteru na něm jsou tyto výchozí hodnoty tabulky hello v Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8788f-111">In hello example above `[dbo].[FactInternetSales_new]` would be created as ROUND_ROBIN distributed table with a CLUSTERED COLUMNSTORE INDEX on it as these are hello table defaults in Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="8788f-112">`SELECT..INTO`však neumožňuje toochange index buď hello distribuční metody nebo hello zadejte v rámci operace hello.</span><span class="sxs-lookup"><span data-stu-id="8788f-112">`SELECT..INTO` however does not allow you toochange either hello distribution method or hello index type as part of hello operation.</span></span> <span data-ttu-id="8788f-113">To je, kdy `CTAS` odeslán.</span><span class="sxs-lookup"><span data-stu-id="8788f-113">This is where `CTAS` comes in.</span></span>

<span data-ttu-id="8788f-114">tooconvert hello výše příliš`CTAS` je poměrně jednoduché:</span><span class="sxs-lookup"><span data-stu-id="8788f-114">tooconvert hello above too`CTAS` is quite straight-forward:</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
    DISTRIBUTION = ROUND_ROBIN
,   CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

<span data-ttu-id="8788f-115">S `CTAS` jsou možné toochange obě hello distribuci dat v tabulce hello, jakož i hello typ tabulky.</span><span class="sxs-lookup"><span data-stu-id="8788f-115">With `CTAS` you are able toochange both hello distribution of hello table data as well as hello table type.</span></span> 

> [!NOTE]
> <span data-ttu-id="8788f-116">Pokud zkoušíte pouze toochange hello index v vaší `CTAS` operace a hello zdrojová tabulka je distribuovat algoritmu hash a vaše `CTAS` provede operaci nejvhodnější, pokud chcete zachovat hello stejného typu distribuční sloupce a data.</span><span class="sxs-lookup"><span data-stu-id="8788f-116">If you are only trying toochange hello index in your `CTAS` operation and hello source table is hash distributed then your `CTAS` operation will perform best if you maintain hello same distribution column and data type.</span></span> <span data-ttu-id="8788f-117">Tím se vyhnete křížové distribuční přesun dat během operace hello, což je efektivnější.</span><span class="sxs-lookup"><span data-stu-id="8788f-117">This will avoid cross distribution data movement during hello operation which is more efficient.</span></span>
> 
> 

## <a name="using-ctas-toocopy-a-table"></a><span data-ttu-id="8788f-118">Pomocí funkce CTAS toocopy tabulku</span><span class="sxs-lookup"><span data-stu-id="8788f-118">Using CTAS toocopy a table</span></span>
<span data-ttu-id="8788f-119">Možná jedním z nejčastějších hello používá `CTAS` vytváří kopie tabulku, ve kterém můžete změnit hello DDL.</span><span class="sxs-lookup"><span data-stu-id="8788f-119">Perhaps one of hello most common uses of `CTAS` is creating a copy of a table so that you can change hello DDL.</span></span> <span data-ttu-id="8788f-120">Pokud třeba jste původně vytvořili tabulku jako `ROUND_ROBIN` a teď ho chcete změnit distribuovaných tooa tabulky na sloupci, `CTAS` je, jak by změna hello distribuční sloupce.</span><span class="sxs-lookup"><span data-stu-id="8788f-120">If for example you originally created your table as `ROUND_ROBIN` and now want change it tooa table distributed on a column, `CTAS` is how you would change hello distribution column.</span></span> <span data-ttu-id="8788f-121">`CTAS`může být také použít toochange typy rozdělení do oddílů, indexování nebo sloupec.</span><span class="sxs-lookup"><span data-stu-id="8788f-121">`CTAS` can also be used toochange partitioning, indexing, or column types.</span></span>

<span data-ttu-id="8788f-122">Řekněme, že jste vytvořili tuto tabulku pomocí hello výchozí typ distribuce `ROUND_ROBIN` distribuované vzhledem k tomu, že žádný distribuční sloupec byl zadán v hello `CREATE TABLE`.</span><span class="sxs-lookup"><span data-stu-id="8788f-122">Let's say you created this table using hello default distribution type of `ROUND_ROBIN` distributed since no distribution column was specified in hello `CREATE TABLE`.</span></span>

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

<span data-ttu-id="8788f-123">Nyní chcete toocreate novou kopii této tabulce s indexem Columnstore v clusteru tak, aby můžete využít výhod hello výkon tabulky Columnstore v clusteru.</span><span class="sxs-lookup"><span data-stu-id="8788f-123">Now you want toocreate a new copy of this table with a Clustered Columnstore Index so that you can take advantage of hello performance of Clustered Columnstore tables.</span></span> <span data-ttu-id="8788f-124">Chcete taky toodistribute této tabulky na ProductKey vzhledem k tomu, že se očekává spojení na tomto sloupci a chcete tooavoid přesun dat během spojení na ProductKey.</span><span class="sxs-lookup"><span data-stu-id="8788f-124">You also want toodistribute this table on ProductKey since you are anticipating joins on this column and want tooavoid data movement during joins on ProductKey.</span></span> <span data-ttu-id="8788f-125">Nakonec chcete taky tooadd vytváření oddílů na OrderDateKey, takže můžete rychle odstranit stará data odstranit staré oddíly.</span><span class="sxs-lookup"><span data-stu-id="8788f-125">Lastly you also want tooadd partitioning on OrderDateKey so that you can quickly delete old data by dropping old partitions.</span></span> <span data-ttu-id="8788f-126">Zde je hello funkce CTAS příkaz, který by vaše staré tabulka zkopírujte do nové tabulky.</span><span class="sxs-lookup"><span data-stu-id="8788f-126">Here is hello CTAS statement which would copy your old table into a new table.</span></span>

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

<span data-ttu-id="8788f-127">Nakonec můžete přejmenovat vaší tooswap tabulky v nové tabulce a pak vyřadit staré tabulky.</span><span class="sxs-lookup"><span data-stu-id="8788f-127">Finally you can rename your tables tooswap in your new table and then drop your old table.</span></span>

```sql
RENAME OBJECT FactInternetSales tooFactInternetSales_old;
RENAME OBJECT FactInternetSales_new tooFactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> <span data-ttu-id="8788f-128">Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="8788f-128">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="8788f-129">V pořadí tooget hello nejlepší výkon ze své dotazy je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením hello nebo dojít k významné změny v datech hello.</span><span class="sxs-lookup"><span data-stu-id="8788f-129">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span>  <span data-ttu-id="8788f-130">Podrobné vysvětlení statistiky najdete v tématu hello [statistiky] [ Statistics] tématu ve skupině témat věnovaných vývoji hello.</span><span class="sxs-lookup"><span data-stu-id="8788f-130">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span>
> 
> 

## <a name="using-ctas-toowork-around-unsupported-features"></a><span data-ttu-id="8788f-131">Pomocí funkce CTAS toowork kolem nepodporované funkce</span><span class="sxs-lookup"><span data-stu-id="8788f-131">Using CTAS toowork around unsupported features</span></span>
<span data-ttu-id="8788f-132">`CTAS`může být také použít toowork kolem počet níže uvedené hello nepodporované funkce.</span><span class="sxs-lookup"><span data-stu-id="8788f-132">`CTAS` can also be used toowork around a number of hello unsupported features listed below.</span></span> <span data-ttu-id="8788f-133">Často to může být toobe win/win situaci jako pouze váš kód bude kompatibilní, ale je často spustí rychleji SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8788f-133">This can often prove toobe a win/win situation as not only will your code be compliant but it will often execute faster on SQL Data Warehouse.</span></span> <span data-ttu-id="8788f-134">Toto je v důsledku plně parallelized návrh.</span><span class="sxs-lookup"><span data-stu-id="8788f-134">This is as a result of its fully parallelized design.</span></span> <span data-ttu-id="8788f-135">Mezi scénáře, které může být kolem pracovali funkce CTAS patří:</span><span class="sxs-lookup"><span data-stu-id="8788f-135">Scenarios that can be worked around with CTAS include:</span></span>

* <span data-ttu-id="8788f-136">ANSI spojení na aktualizace</span><span class="sxs-lookup"><span data-stu-id="8788f-136">ANSI JOINS on UPDATEs</span></span>
* <span data-ttu-id="8788f-137">ANSI spojení na odstranění</span><span class="sxs-lookup"><span data-stu-id="8788f-137">ANSI JOINs on DELETEs</span></span>
* <span data-ttu-id="8788f-138">MERGE – příkaz</span><span class="sxs-lookup"><span data-stu-id="8788f-138">MERGE statement</span></span>

> [!NOTE]
> <span data-ttu-id="8788f-139">Zkuste toothink "funkce CTAS první".</span><span class="sxs-lookup"><span data-stu-id="8788f-139">Try toothink "CTAS first".</span></span> <span data-ttu-id="8788f-140">Pokud se domníváte, že vám může pomoct vyřešit problém pomocí `CTAS` a který je obvykle hello nejlepší způsob, jak tooapproach ho - i v případě, že v důsledku píšete další data.</span><span class="sxs-lookup"><span data-stu-id="8788f-140">If you think you can solve a problem using `CTAS` then that is generally hello best way tooapproach it - even if you are writing more data as a result.</span></span>
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a><span data-ttu-id="8788f-141">Připojení k nahrazení ANSI pro příkazy aktualizace</span><span class="sxs-lookup"><span data-stu-id="8788f-141">ANSI join replacement for update statements</span></span>
<span data-ttu-id="8788f-142">Můžete zjistit, že máte komplexní aktualizace, která spojuje více než dvě tabulky pomocí ANSI připojení syntaxe tooperform hello aktualizovat nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="8788f-142">You may find you have a complex update that joins more than two tables together using ANSI joining syntax tooperform hello UPDATE or DELETE.</span></span>

<span data-ttu-id="8788f-143">Představte si, že jste měli tooupdate v této tabulce:</span><span class="sxs-lookup"><span data-stu-id="8788f-143">Imagine you had tooupdate this table:</span></span>

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(    [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,    [CalendarYear]                    SMALLINT        NOT NULL
,    [TotalSalesAmount]                MONEY            NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

<span data-ttu-id="8788f-144">původní dotaz Hello může mít hledá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="8788f-144">hello original query might have looked something like this:</span></span>

```sql
UPDATE    acs
SET        [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT    [EnglishProductCategoryName]
        ,        [CalendarYear]
        ,        SUM([SalesAmount])                AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]        AS s
        JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
        JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
        WHERE     [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,        [CalendarYear]
        ) AS fis
ON    [acs].[EnglishProductCategoryName]    = [fis].[EnglishProductCategoryName]
AND    [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

<span data-ttu-id="8788f-145">Vzhledem k tomu, že SQL Data Warehouse nepodporuje ANSI Příkazy JOIN v hello `FROM` klauzuli `UPDATE` prohlášení, nelze zkopírovat tento kód přes bez provedení změn mírně.</span><span class="sxs-lookup"><span data-stu-id="8788f-145">Since SQL Data Warehouse does not support ANSI joins in hello `FROM` clause of an `UPDATE` statement, you cannot copy this code over without changing it slightly.</span></span>

<span data-ttu-id="8788f-146">Můžete použít kombinaci `CTAS` a implicitní připojení tooreplace tento kód:</span><span class="sxs-lookup"><span data-stu-id="8788f-146">You can use a combination of a `CTAS` and an implicit join tooreplace this code:</span></span>

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT    ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,        ISNULL(CAST([CalendarYear] AS SMALLINT),0)                         AS [CalendarYear]
,        ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                        AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]        AS s
JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
WHERE     [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,        [CalendarYear]
;

-- Use an implicit join tooperform hello update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop hello interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a><span data-ttu-id="8788f-147">Odstranit připojení k nahrazení ANSI pro příkazy</span><span class="sxs-lookup"><span data-stu-id="8788f-147">ANSI join replacement for delete statements</span></span>
<span data-ttu-id="8788f-148">Někdy hello nejlepší přístup pro odstranění dat je toouse `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="8788f-148">Sometimes hello best approach for deleting data is toouse `CTAS`.</span></span> <span data-ttu-id="8788f-149">Místo odstranění dat hello jednoduše vyberte hello data, která chcete tookeep.</span><span class="sxs-lookup"><span data-stu-id="8788f-149">Rather than deleting hello data simply select hello data you want tookeep.</span></span> <span data-ttu-id="8788f-150">Tato především pro `DELETE` příkazy, které používají připojení syntaxe, protože SQL Data Warehouse nepodporuje spojení ANSI v hello ansi `FROM` klauzuli `DELETE` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8788f-150">This especially true for `DELETE` statements that use ansi joining syntax since SQL Data Warehouse does not support ANSI joins in hello `FROM` clause of a `DELETE` statement.</span></span>

<span data-ttu-id="8788f-151">Příklad převedený příkazu DELETE je k dispozici následující:</span><span class="sxs-lookup"><span data-stu-id="8788f-151">An example of a converted DELETE statement is available below:</span></span>

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish tookeep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        tooDimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert tooDimProduct;
```

## <a name="replace-merge-statements"></a><span data-ttu-id="8788f-152">Nahraďte příkazy merge</span><span class="sxs-lookup"><span data-stu-id="8788f-152">Replace merge statements</span></span>
<span data-ttu-id="8788f-153">Příkazy Merge lze nahradit, alespoň v rámci, pomocí `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="8788f-153">Merge statements can be replaced, at least in part, by using `CTAS`.</span></span> <span data-ttu-id="8788f-154">Může konsolidovat hello `INSERT` a hello `UPDATE` do jednoho příkazu.</span><span class="sxs-lookup"><span data-stu-id="8788f-154">You can consolidate hello `INSERT` and hello `UPDATE` into a single statement.</span></span> <span data-ttu-id="8788f-155">Odstraněné záznamy potřebovat toobe vypnout byl uzavřen za druhý příkaz.</span><span class="sxs-lookup"><span data-stu-id="8788f-155">Any deleted records would need toobe closed off in a second statement.</span></span>

<span data-ttu-id="8788f-156">Příklad `UPSERT` je k dispozici následující:</span><span class="sxs-lookup"><span data-stu-id="8788f-156">An example of an `UPSERT` is available below:</span></span>

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          too[DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  too[DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a><span data-ttu-id="8788f-157">Funkce CTAS doporučení: explicitně stavu datový typ a možnost použití hodnoty Null výstupu</span><span class="sxs-lookup"><span data-stu-id="8788f-157">CTAS recommendation: Explicitly state data type and nullability of output</span></span>
<span data-ttu-id="8788f-158">Při migraci kódu můžete zjistit, že spustíte přes tento typ kódování vzoru:</span><span class="sxs-lookup"><span data-stu-id="8788f-158">When migrating code you might find you run across this type of coding pattern:</span></span>

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

<span data-ttu-id="8788f-159">Instinktivně si myslíte by bylo nutné migrovat tento kód tooa funkce CTAS a by byly správné.</span><span class="sxs-lookup"><span data-stu-id="8788f-159">Instinctively you might think you should migrate this code tooa CTAS and you would be correct.</span></span> <span data-ttu-id="8788f-160">Je však skrytá problém tady.</span><span class="sxs-lookup"><span data-stu-id="8788f-160">However, there is a hidden issue here.</span></span>

<span data-ttu-id="8788f-161">Hello následující kód nepřinese hello stejný výsledek:</span><span class="sxs-lookup"><span data-stu-id="8788f-161">hello following code does NOT yield hello same result:</span></span>

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

<span data-ttu-id="8788f-162">Všimněte si, hello sloupec "výsledek" představuje předat hodnoty typu a možnost použití hodnoty NULL dat hello hello výrazu.</span><span class="sxs-lookup"><span data-stu-id="8788f-162">Notice that hello column "result" carries forward hello data type and nullability values of hello expression.</span></span> <span data-ttu-id="8788f-163">To může způsobit toosubtle odchylky hodnoty, pokud nejste opatrní.</span><span class="sxs-lookup"><span data-stu-id="8788f-163">This can lead toosubtle variances in values if you aren't careful.</span></span>

<span data-ttu-id="8788f-164">Zkuste následující hello jako příklad:</span><span class="sxs-lookup"><span data-stu-id="8788f-164">Try hello following as an example:</span></span>

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

<span data-ttu-id="8788f-165">Hodnota Hello uložené pro výsledek se liší.</span><span class="sxs-lookup"><span data-stu-id="8788f-165">hello value stored for result is different.</span></span> <span data-ttu-id="8788f-166">Jako hello trvalou hodnotu ve sloupci výsledek hello se používá v jiné se změní i větších výrazy hello chyby.</span><span class="sxs-lookup"><span data-stu-id="8788f-166">As hello persisted value in hello result column is used in other expressions hello error becomes even more significant.</span></span>

![][1]

<span data-ttu-id="8788f-167">To je zvlášť důležité pro data migrace.</span><span class="sxs-lookup"><span data-stu-id="8788f-167">This is particularly important for data migrations.</span></span> <span data-ttu-id="8788f-168">I když je pravděpodobně přesnější hello druhého dotazu došlo k potížím.</span><span class="sxs-lookup"><span data-stu-id="8788f-168">Even though hello second query is arguably more accurate there is a problem.</span></span> <span data-ttu-id="8788f-169">Hello dat by být různých porovnání toohello zdrojovém systému a který vede tooquestions integrita hello migraci.</span><span class="sxs-lookup"><span data-stu-id="8788f-169">hello data would be different compared toohello source system and that leads tooquestions of integrity in hello migration.</span></span> <span data-ttu-id="8788f-170">Toto je jedna z výjimečných případech, kdy hello "nesprávný" odpovědí ve skutečnosti hello vpravo jeden!</span><span class="sxs-lookup"><span data-stu-id="8788f-170">This is one of those rare cases where hello "wrong" answer is actually hello right one!</span></span>

<span data-ttu-id="8788f-171">Hello důvod vidíme tento rozdíl mezi dvěma hello výsledky je mimo provoz tooimplicit typ přetypování.</span><span class="sxs-lookup"><span data-stu-id="8788f-171">hello reason we see this disparity between hello two results is down tooimplicit type casting.</span></span> <span data-ttu-id="8788f-172">V hello první tabulky hello příklad definuje hello definici sloupce.</span><span class="sxs-lookup"><span data-stu-id="8788f-172">In hello first example hello table defines hello column definition.</span></span> <span data-ttu-id="8788f-173">Pokud je vložit řádek hello dochází k konverzi implicitní typu.</span><span class="sxs-lookup"><span data-stu-id="8788f-173">When hello row is inserted an implicit type conversion occurs.</span></span> <span data-ttu-id="8788f-174">V druhém příkladu hello neexistuje žádný typ implicitní převod jako hello výraz definuje datový typ sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="8788f-174">In hello second example there is no implicit type conversion as hello expression defines data type of hello column.</span></span> <span data-ttu-id="8788f-175">Všimněte si že také hello sloupce v druhém příkladu hello byla definována jako sloupec s možnou hodnotou Null zatímco v prvním příkladu hello má není.</span><span class="sxs-lookup"><span data-stu-id="8788f-175">Notice also that hello column in hello second example has been defined as a NULLable column whereas in hello first example it has not.</span></span> <span data-ttu-id="8788f-176">Když hello tabulka byla vytvořena v byl explicitně definován hello první příklad sloupec možnost použití hodnoty Null.</span><span class="sxs-lookup"><span data-stu-id="8788f-176">When hello table was created in hello first example column nullability was explicitly defined.</span></span> <span data-ttu-id="8788f-177">V druhém příkladu hello byl právě zůstane toohello výraz a ve výchozím nastavení to by způsobilo definici hodnotu NULL.</span><span class="sxs-lookup"><span data-stu-id="8788f-177">In hello second example it was just left toohello expression and by default this would result in a NULL definition.</span></span>  

<span data-ttu-id="8788f-178">tooresolve těchto problémů musíte explicitně nastavit převod typů hello a možnost použití hodnoty Null v hello `SELECT` část hello `CTAS` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8788f-178">tooresolve these issues you must explicitly set hello type conversion and nullability in hello `SELECT` portion of hello `CTAS` statement.</span></span> <span data-ttu-id="8788f-179">Nelze nastavit tyto vlastnosti v hello vytvořit část tabulky.</span><span class="sxs-lookup"><span data-stu-id="8788f-179">You cannot set these properties in hello create table part.</span></span>

<span data-ttu-id="8788f-180">Hello Příklad dole ukazuje, jak toofix hello kódu:</span><span class="sxs-lookup"><span data-stu-id="8788f-180">hello example below demonstrates how toofix hello code:</span></span>

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

<span data-ttu-id="8788f-181">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="8788f-181">Note hello following:</span></span>

* <span data-ttu-id="8788f-182">CAST nebo CONVERT byl použit</span><span class="sxs-lookup"><span data-stu-id="8788f-182">CAST or CONVERT could have been used</span></span>
* <span data-ttu-id="8788f-183">ISNULL je použité tooforce možnost použití hodnoty Null nejsou COALESCE</span><span class="sxs-lookup"><span data-stu-id="8788f-183">ISNULL is used tooforce NULLability not COALESCE</span></span>
* <span data-ttu-id="8788f-184">ISNULL je nejzevnější funkce hello</span><span class="sxs-lookup"><span data-stu-id="8788f-184">ISNULL is hello outermost function</span></span>
* <span data-ttu-id="8788f-185">Druhá část hello ISNULL Hello je konstanta, tzn. 0</span><span class="sxs-lookup"><span data-stu-id="8788f-185">hello second part of hello ISNULL is a constant i.e. 0</span></span>

> [!NOTE]
> <span data-ttu-id="8788f-186">Pro možnost použití hodnoty Null toobe hello správně nastavené je životně důležité toouse `ISNULL` a není `COALESCE`.</span><span class="sxs-lookup"><span data-stu-id="8788f-186">For hello nullability toobe correctly set it is vital toouse `ISNULL` and not `COALESCE`.</span></span> <span data-ttu-id="8788f-187">`COALESCE`není deterministický funkce a tak hello výsledek výrazu hello bude vždy s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="8788f-187">`COALESCE` is not a deterministic function and so hello result of hello expression will always be NULLable.</span></span> <span data-ttu-id="8788f-188">`ISNULL`se liší.</span><span class="sxs-lookup"><span data-stu-id="8788f-188">`ISNULL` is different.</span></span> <span data-ttu-id="8788f-189">Je deterministický.</span><span class="sxs-lookup"><span data-stu-id="8788f-189">It is deterministic.</span></span> <span data-ttu-id="8788f-190">Proto když hello druhou částí hello `ISNULL` funkce je konstanta, nebo literál pak bude hello výsledná hodnota není NULL.</span><span class="sxs-lookup"><span data-stu-id="8788f-190">Therefore when hello second part of hello `ISNULL` function is a constant or a literal then hello resulting value will be NOT NULL.</span></span>
> 
> 

<span data-ttu-id="8788f-191">Tento tip není právě užitečné pro zajištění integrity hello vaší výpočtů.</span><span class="sxs-lookup"><span data-stu-id="8788f-191">This tip is not just useful for ensuring hello integrity of your calculations.</span></span> <span data-ttu-id="8788f-192">Je také důležité k přepnutí oddílu tabulky.</span><span class="sxs-lookup"><span data-stu-id="8788f-192">It is also important for table partition switching.</span></span> <span data-ttu-id="8788f-193">Představte si, že máte tato tabulka definován jako vaše fakt:</span><span class="sxs-lookup"><span data-stu-id="8788f-193">Imagine you have this table defined as your fact:</span></span>

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

<span data-ttu-id="8788f-194">Hello hodnota pole je však počítané výraz, který není součástí hello zdrojová data.</span><span class="sxs-lookup"><span data-stu-id="8788f-194">However, hello value field is a calculated expression it is not part of hello source data.</span></span>

<span data-ttu-id="8788f-195">toocreate oddílů datovou sadu můžete chtít toodo toto:</span><span class="sxs-lookup"><span data-stu-id="8788f-195">toocreate your partitioned dataset you might want toodo this:</span></span>

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

<span data-ttu-id="8788f-196">dotaz Hello by spustit perfektně dobře.</span><span class="sxs-lookup"><span data-stu-id="8788f-196">hello query would run perfectly fine.</span></span> <span data-ttu-id="8788f-197">Hello problém se dodává se při tooperform hello oddílu přepínače.</span><span class="sxs-lookup"><span data-stu-id="8788f-197">hello problem comes when you try tooperform hello partition switch.</span></span> <span data-ttu-id="8788f-198">definice tabulek Hello se neshodují.</span><span class="sxs-lookup"><span data-stu-id="8788f-198">hello table definitions do not match.</span></span> <span data-ttu-id="8788f-199">definice tabulek hello toomake odpovídat hello funkce CTAS musí toobe upravit.</span><span class="sxs-lookup"><span data-stu-id="8788f-199">toomake hello table definitions match hello CTAS needs toobe modified.</span></span>

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

<span data-ttu-id="8788f-200">Můžete zobrazit proto konzistenci typu a udržování možnost použití hodnoty NULL vlastnosti funkce CTAS je vhodné engineering nejlepší.</span><span class="sxs-lookup"><span data-stu-id="8788f-200">You can see therefore that type consistency and maintaining nullability properties on a CTAS is a good engineering best practice.</span></span> <span data-ttu-id="8788f-201">Pomáhá toomaintain integrity do výpočtů a také zajistí, že přepnutí oddílu je možné.</span><span class="sxs-lookup"><span data-stu-id="8788f-201">It helps toomaintain integrity in your calculations and also ensures that partition switching is possible.</span></span>

<span data-ttu-id="8788f-202">Další informace o používání naleznete tooMSDN [funkce CTAS][CTAS].</span><span class="sxs-lookup"><span data-stu-id="8788f-202">Please refer tooMSDN for more information on using [CTAS][CTAS].</span></span> <span data-ttu-id="8788f-203">Je jedním z nejdůležitějších příkazy hello v Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8788f-203">It is one of hello most important statements in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="8788f-204">Ujistěte se, že rozumíte důkladně.</span><span class="sxs-lookup"><span data-stu-id="8788f-204">Make sure you thoroughly understand it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8788f-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8788f-205">Next steps</span></span>
<span data-ttu-id="8788f-206">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="8788f-206">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
