---
title: "Vytvoření tabulky jako vyberte (funkce CTAS) v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro kódování s tabulkou vytvořit jako SELECT (funkce CTAS) v Azure SQL Data Warehouse na vývoj řešení."
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
ms.openlocfilehash: cb08313726e8135feaa9b413937c2197ea397f4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a><span data-ttu-id="070b7-103">Vytvoření tabulky jako vyberte (funkce CTAS) v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="070b7-103">Create Table As Select (CTAS) in SQL Data Warehouse</span></span>
<span data-ttu-id="070b7-104">Vytvoření tabulky jako vyberte nebo `CTAS` je jedním z nejdůležitějších funkcích T-SQL k dispozici.</span><span class="sxs-lookup"><span data-stu-id="070b7-104">Create table as select or `CTAS` is one of the most important T-SQL features available.</span></span> <span data-ttu-id="070b7-105">Je plně parallelized operaci, která vytvoří novou tabulku ve výstupu příkazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="070b7-105">It is a fully parallelized operation that creates a new table based on the output of a SELECT statement.</span></span> <span data-ttu-id="070b7-106">`CTAS`je nejjednodušší a nejrychlejší způsob, jak vytvořit kopii tabulky.</span><span class="sxs-lookup"><span data-stu-id="070b7-106">`CTAS` is the simplest and fastest way to create a copy of a table.</span></span> <span data-ttu-id="070b7-107">Tento dokument obsahuje příklady a osvědčené postupy pro `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="070b7-107">This document provides both examples and best practices for `CTAS`.</span></span>

## <a name="selectinto-vs-ctas"></a><span data-ttu-id="070b7-108">VYBERTE... DO vs. CTAS</span><span class="sxs-lookup"><span data-stu-id="070b7-108">SELECT..INTO vs. CTAS</span></span>
<span data-ttu-id="070b7-109">Můžete zvážit `CTAS` jako extrémně účtovat verze `SELECT..INTO`.</span><span class="sxs-lookup"><span data-stu-id="070b7-109">You can consider `CTAS` as a super-charged version of `SELECT..INTO`.</span></span>

<span data-ttu-id="070b7-110">Dole je příklad jednoduchou `SELECT..INTO` příkaz:</span><span class="sxs-lookup"><span data-stu-id="070b7-110">Below is an example of a simple `SELECT..INTO` statement:</span></span>

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

<span data-ttu-id="070b7-111">V předchozím příkladu `[dbo].[FactInternetSales_new]` by se vytvořily jako ROUND_ROBIN distribuované tabulku s INDEXEM COLUMNSTORE v clusteru na něm jsou tyto výchozí hodnoty tabulky v Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="070b7-111">In the example above `[dbo].[FactInternetSales_new]` would be created as ROUND_ROBIN distributed table with a CLUSTERED COLUMNSTORE INDEX on it as these are the table defaults in Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="070b7-112">`SELECT..INTO`ale neumožňuje změnit metodu distribuce nebo typ indexu v rámci operace.</span><span class="sxs-lookup"><span data-stu-id="070b7-112">`SELECT..INTO` however does not allow you to change either the distribution method or the index type as part of the operation.</span></span> <span data-ttu-id="070b7-113">To je, kdy `CTAS` odeslán.</span><span class="sxs-lookup"><span data-stu-id="070b7-113">This is where `CTAS` comes in.</span></span>

<span data-ttu-id="070b7-114">Chcete-li převést výše a `CTAS` je poměrně jednoduché:</span><span class="sxs-lookup"><span data-stu-id="070b7-114">To convert the above to `CTAS` is quite straight-forward:</span></span>

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

<span data-ttu-id="070b7-115">S `CTAS` budete moci změnit rozdělení dat v tabulce, jakož i typu tabulky.</span><span class="sxs-lookup"><span data-stu-id="070b7-115">With `CTAS` you are able to change both the distribution of the table data as well as the table type.</span></span> 

> [!NOTE]
> <span data-ttu-id="070b7-116">Pokud se pouze pokoušíte změnit index v vaše `CTAS` operace a zdrojová tabulka je distribuovat algoritmu hash pak vaší `CTAS` operace provede nejlépe, pokud chcete zachovat stejné distribuční typ sloupce a data.</span><span class="sxs-lookup"><span data-stu-id="070b7-116">If you are only trying to change the index in your `CTAS` operation and the source table is hash distributed then your `CTAS` operation will perform best if you maintain the same distribution column and data type.</span></span> <span data-ttu-id="070b7-117">Tím se vyhnete křížové distribuční přesun dat během operace, které je efektivnější.</span><span class="sxs-lookup"><span data-stu-id="070b7-117">This will avoid cross distribution data movement during the operation which is more efficient.</span></span>
> 
> 

## <a name="using-ctas-to-copy-a-table"></a><span data-ttu-id="070b7-118">Pomocí funkce CTAS zkopírujte tabulku</span><span class="sxs-lookup"><span data-stu-id="070b7-118">Using CTAS to copy a table</span></span>
<span data-ttu-id="070b7-119">Možná jedním z většiny běžných používá `CTAS` vytváří kopie tabulku, ve kterém můžete změnit DDL.</span><span class="sxs-lookup"><span data-stu-id="070b7-119">Perhaps one of the most common uses of `CTAS` is creating a copy of a table so that you can change the DDL.</span></span> <span data-ttu-id="070b7-120">Pokud třeba jste původně vytvořili tabulku jako `ROUND_ROBIN` a teď chcete ho změnit na tabulku rozdělit na sloupci, `CTAS` je, jak by sloupec distribuční změnit.</span><span class="sxs-lookup"><span data-stu-id="070b7-120">If for example you originally created your table as `ROUND_ROBIN` and now want change it to a table distributed on a column, `CTAS` is how you would change the distribution column.</span></span> <span data-ttu-id="070b7-121">`CTAS`lze také změnit typy rozdělení do oddílů, indexování nebo sloupec.</span><span class="sxs-lookup"><span data-stu-id="070b7-121">`CTAS` can also be used to change partitioning, indexing, or column types.</span></span>

<span data-ttu-id="070b7-122">Řekněme, že jste vytvořili tuto tabulku pomocí výchozí typ distribuce tohoto `ROUND_ROBIN` distribuované vzhledem k tomu, že žádný distribuční sloupec byl zadán v `CREATE TABLE`.</span><span class="sxs-lookup"><span data-stu-id="070b7-122">Let's say you created this table using the default distribution type of `ROUND_ROBIN` distributed since no distribution column was specified in the `CREATE TABLE`.</span></span>

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

<span data-ttu-id="070b7-123">Teď chcete vytvořit novou kopii této tabulky s clusterovaný Columnstore Index, takže můžete využít výhod výkonu tabulky Columnstore v clusteru.</span><span class="sxs-lookup"><span data-stu-id="070b7-123">Now you want to create a new copy of this table with a Clustered Columnstore Index so that you can take advantage of the performance of Clustered Columnstore tables.</span></span> <span data-ttu-id="070b7-124">Můžete také chcete distribuovat tuto tabulku na ProductKey vzhledem k tomu, že se očekává spojení na tomto sloupci a chcete zabránit přesunu dat během spojení na ProductKey.</span><span class="sxs-lookup"><span data-stu-id="070b7-124">You also want to distribute this table on ProductKey since you are anticipating joins on this column and want to avoid data movement during joins on ProductKey.</span></span> <span data-ttu-id="070b7-125">Nakonec chcete také přidat dělení na OrderDateKey, takže můžete rychle odstranit stará data odstranit staré oddíly.</span><span class="sxs-lookup"><span data-stu-id="070b7-125">Lastly you also want to add partitioning on OrderDateKey so that you can quickly delete old data by dropping old partitions.</span></span> <span data-ttu-id="070b7-126">Zde je funkce CTAS příkaz, který by vaše staré tabulka zkopírujte do nové tabulky.</span><span class="sxs-lookup"><span data-stu-id="070b7-126">Here is the CTAS statement which would copy your old table into a new table.</span></span>

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

<span data-ttu-id="070b7-127">Nakonec můžete přejmenovat vaše tabulky odkládacího souboru v nové tabulce a pak vyřadit staré tabulky.</span><span class="sxs-lookup"><span data-stu-id="070b7-127">Finally you can rename your tables to swap in your new table and then drop your old table.</span></span>

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> <span data-ttu-id="070b7-128">Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.</span><span class="sxs-lookup"><span data-stu-id="070b7-128">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="070b7-129">Aby vám dotazy vracely co nejlepší výsledky, je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením nebo kdykoli, kdy v datech dojde k podstatným změnám.</span><span class="sxs-lookup"><span data-stu-id="070b7-129">In order to get the best performance from your queries, it's important that statistics be created on all columns of all tables after the first load or any substantial changes occur in the data.</span></span>  <span data-ttu-id="070b7-130">Podrobné vysvětlení statistiky najdete v tématu [Statistika][Statistics] ve skupině témat věnovaných vývoji.</span><span class="sxs-lookup"><span data-stu-id="070b7-130">For a detailed explanation of statistics, see the [Statistics][Statistics] topic in the Develop group of topics.</span></span>
> 
> 

## <a name="using-ctas-to-work-around-unsupported-features"></a><span data-ttu-id="070b7-131">Pomocí funkce CTAS obejít nepodporované funkce</span><span class="sxs-lookup"><span data-stu-id="070b7-131">Using CTAS to work around unsupported features</span></span>
<span data-ttu-id="070b7-132">`CTAS`Můžete také použít obejít počet níže uvedené nepodporované funkce.</span><span class="sxs-lookup"><span data-stu-id="070b7-132">`CTAS` can also be used to work around a number of the unsupported features listed below.</span></span> <span data-ttu-id="070b7-133">To může být často na win/win situaci, protože pouze váš kód bude kompatibilní, ale je často spustí rychleji SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="070b7-133">This can often prove to be a win/win situation as not only will your code be compliant but it will often execute faster on SQL Data Warehouse.</span></span> <span data-ttu-id="070b7-134">Toto je v důsledku plně parallelized návrh.</span><span class="sxs-lookup"><span data-stu-id="070b7-134">This is as a result of its fully parallelized design.</span></span> <span data-ttu-id="070b7-135">Mezi scénáře, které může být kolem pracovali funkce CTAS patří:</span><span class="sxs-lookup"><span data-stu-id="070b7-135">Scenarios that can be worked around with CTAS include:</span></span>

* <span data-ttu-id="070b7-136">ANSI spojení na aktualizace</span><span class="sxs-lookup"><span data-stu-id="070b7-136">ANSI JOINS on UPDATEs</span></span>
* <span data-ttu-id="070b7-137">ANSI spojení na odstranění</span><span class="sxs-lookup"><span data-stu-id="070b7-137">ANSI JOINs on DELETEs</span></span>
* <span data-ttu-id="070b7-138">MERGE – příkaz</span><span class="sxs-lookup"><span data-stu-id="070b7-138">MERGE statement</span></span>

> [!NOTE]
> <span data-ttu-id="070b7-139">Zamyslete se nad "funkce CTAS první".</span><span class="sxs-lookup"><span data-stu-id="070b7-139">Try to think "CTAS first".</span></span> <span data-ttu-id="070b7-140">Pokud se domníváte, že vám může pomoct vyřešit problém pomocí `CTAS` , je obecně nejlepší způsob, jak postupovat, je - i v případě, že v důsledku píšete další data.</span><span class="sxs-lookup"><span data-stu-id="070b7-140">If you think you can solve a problem using `CTAS` then that is generally the best way to approach it - even if you are writing more data as a result.</span></span>
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a><span data-ttu-id="070b7-141">Připojení k nahrazení ANSI pro příkazy aktualizace</span><span class="sxs-lookup"><span data-stu-id="070b7-141">ANSI join replacement for update statements</span></span>
<span data-ttu-id="070b7-142">Můžete zjistit, že máte komplexní aktualizace, které spojí dohromady pomocí ANSI připojení syntaxe k provedení aktualizace nebo odstranění více než dvě tabulky.</span><span class="sxs-lookup"><span data-stu-id="070b7-142">You may find you have a complex update that joins more than two tables together using ANSI joining syntax to perform the UPDATE or DELETE.</span></span>

<span data-ttu-id="070b7-143">Představte si, že jste museli aktualizovat v této tabulce:</span><span class="sxs-lookup"><span data-stu-id="070b7-143">Imagine you had to update this table:</span></span>

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

<span data-ttu-id="070b7-144">Původní dotaz může mít hledá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="070b7-144">The original query might have looked something like this:</span></span>

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

<span data-ttu-id="070b7-145">Vzhledem k tomu, že SQL Data Warehouse nepodporuje ANSI Příkazy JOIN v `FROM` klauzuli `UPDATE` prohlášení, nelze zkopírovat tento kód přes bez provedení změn mírně.</span><span class="sxs-lookup"><span data-stu-id="070b7-145">Since SQL Data Warehouse does not support ANSI joins in the `FROM` clause of an `UPDATE` statement, you cannot copy this code over without changing it slightly.</span></span>

<span data-ttu-id="070b7-146">Můžete použít kombinaci `CTAS` a implicitní spojení nahraďte tento kód:</span><span class="sxs-lookup"><span data-stu-id="070b7-146">You can use a combination of a `CTAS` and an implicit join to replace this code:</span></span>

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

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a><span data-ttu-id="070b7-147">Odstranit připojení k nahrazení ANSI pro příkazy</span><span class="sxs-lookup"><span data-stu-id="070b7-147">ANSI join replacement for delete statements</span></span>
<span data-ttu-id="070b7-148">Někdy je nejlepší metodou pro odstranění dat použít `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="070b7-148">Sometimes the best approach for deleting data is to use `CTAS`.</span></span> <span data-ttu-id="070b7-149">Místo odstraněním dat jednoduše vyberte data, která chcete zachovat.</span><span class="sxs-lookup"><span data-stu-id="070b7-149">Rather than deleting the data simply select the data you want to keep.</span></span> <span data-ttu-id="070b7-150">Tato především pro `DELETE` příkazy, které používají ansi spojující syntaxe, protože SQL Data Warehouse nepodporuje ANSI Příkazy JOIN v `FROM` klauzuli `DELETE` příkaz.</span><span class="sxs-lookup"><span data-stu-id="070b7-150">This especially true for `DELETE` statements that use ansi joining syntax since SQL Data Warehouse does not support ANSI joins in the `FROM` clause of a `DELETE` statement.</span></span>

<span data-ttu-id="070b7-151">Příklad převedený příkazu DELETE je k dispozici následující:</span><span class="sxs-lookup"><span data-stu-id="070b7-151">An example of a converted DELETE statement is available below:</span></span>

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a><span data-ttu-id="070b7-152">Nahraďte příkazy merge</span><span class="sxs-lookup"><span data-stu-id="070b7-152">Replace merge statements</span></span>
<span data-ttu-id="070b7-153">Příkazy Merge lze nahradit, alespoň v rámci, pomocí `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="070b7-153">Merge statements can be replaced, at least in part, by using `CTAS`.</span></span> <span data-ttu-id="070b7-154">Může konsolidovat `INSERT` a `UPDATE` do jednoho příkazu.</span><span class="sxs-lookup"><span data-stu-id="070b7-154">You can consolidate the `INSERT` and the `UPDATE` into a single statement.</span></span> <span data-ttu-id="070b7-155">Odstraněné záznamy by třeba ukončit vypnout v druhém příkazu.</span><span class="sxs-lookup"><span data-stu-id="070b7-155">Any deleted records would need to be closed off in a second statement.</span></span>

<span data-ttu-id="070b7-156">Příklad `UPSERT` je k dispozici následující:</span><span class="sxs-lookup"><span data-stu-id="070b7-156">An example of an `UPSERT` is available below:</span></span>

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

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a><span data-ttu-id="070b7-157">Funkce CTAS doporučení: explicitně stavu datový typ a možnost použití hodnoty Null výstupu</span><span class="sxs-lookup"><span data-stu-id="070b7-157">CTAS recommendation: Explicitly state data type and nullability of output</span></span>
<span data-ttu-id="070b7-158">Při migraci kódu můžete zjistit, že spustíte přes tento typ kódování vzoru:</span><span class="sxs-lookup"><span data-stu-id="070b7-158">When migrating code you might find you run across this type of coding pattern:</span></span>

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

<span data-ttu-id="070b7-159">Instinktivně si myslíte měli byste migrovat tento kód na funkce CTAS a by byly správné.</span><span class="sxs-lookup"><span data-stu-id="070b7-159">Instinctively you might think you should migrate this code to a CTAS and you would be correct.</span></span> <span data-ttu-id="070b7-160">Je však skrytá problém tady.</span><span class="sxs-lookup"><span data-stu-id="070b7-160">However, there is a hidden issue here.</span></span>

<span data-ttu-id="070b7-161">Následující kód nepřinese stejný výsledek:</span><span class="sxs-lookup"><span data-stu-id="070b7-161">The following code does NOT yield the same result:</span></span>

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

<span data-ttu-id="070b7-162">Všimněte si sloupec "výsledek" představuje předat hodnoty datového typu a možnost použití hodnoty Null výrazu.</span><span class="sxs-lookup"><span data-stu-id="070b7-162">Notice that the column "result" carries forward the data type and nullability values of the expression.</span></span> <span data-ttu-id="070b7-163">To může vést k jemně odchylky v hodnoty, pokud nejste opatrní.</span><span class="sxs-lookup"><span data-stu-id="070b7-163">This can lead to subtle variances in values if you aren't careful.</span></span>

<span data-ttu-id="070b7-164">Zkuste následující kroky jako příklad:</span><span class="sxs-lookup"><span data-stu-id="070b7-164">Try the following as an example:</span></span>

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

<span data-ttu-id="070b7-165">Hodnota uložená pro výsledek se liší.</span><span class="sxs-lookup"><span data-stu-id="070b7-165">The value stored for result is different.</span></span> <span data-ttu-id="070b7-166">Jako trvalé hodnoty ve sloupci Výsledek se používá v jiné výrazy se změní i větších chyba.</span><span class="sxs-lookup"><span data-stu-id="070b7-166">As the persisted value in the result column is used in other expressions the error becomes even more significant.</span></span>

![][1]

<span data-ttu-id="070b7-167">To je zvlášť důležité pro data migrace.</span><span class="sxs-lookup"><span data-stu-id="070b7-167">This is particularly important for data migrations.</span></span> <span data-ttu-id="070b7-168">I když je pravděpodobně přesnější druhého dotazu došlo k potížím.</span><span class="sxs-lookup"><span data-stu-id="070b7-168">Even though the second query is arguably more accurate there is a problem.</span></span> <span data-ttu-id="070b7-169">Data byla odlišná ve srovnání s zdrojovém systému a který vede k otázky integrity migrace.</span><span class="sxs-lookup"><span data-stu-id="070b7-169">The data would be different compared to the source system and that leads to questions of integrity in the migration.</span></span> <span data-ttu-id="070b7-170">Toto je jedna z těchto výjimečných případech, kdy "nesprávný" odpověď je ve skutečnosti ten správný!</span><span class="sxs-lookup"><span data-stu-id="070b7-170">This is one of those rare cases where the "wrong" answer is actually the right one!</span></span>

<span data-ttu-id="070b7-171">Z důvodu, že vidíte tento rozdíl mezi dvěma výsledky je dolů implicitní typ přetypování.</span><span class="sxs-lookup"><span data-stu-id="070b7-171">The reason we see this disparity between the two results is down to implicit type casting.</span></span> <span data-ttu-id="070b7-172">V prvním příkladu definuje tabulky definice sloupce.</span><span class="sxs-lookup"><span data-stu-id="070b7-172">In the first example the table defines the column definition.</span></span> <span data-ttu-id="070b7-173">Když je vložit řádek dojde k konverzi implicitní typu.</span><span class="sxs-lookup"><span data-stu-id="070b7-173">When the row is inserted an implicit type conversion occurs.</span></span> <span data-ttu-id="070b7-174">V druhém příkladu není žádný typ implicitní převod jako výraz definuje datový typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="070b7-174">In the second example there is no implicit type conversion as the expression defines data type of the column.</span></span> <span data-ttu-id="070b7-175">Všimněte si také, že sloupec v druhém příkladu jako sloupec s možnou hodnotou Null byla definována zatímco v prvním příkladu má není.</span><span class="sxs-lookup"><span data-stu-id="070b7-175">Notice also that the column in the second example has been defined as a NULLable column whereas in the first example it has not.</span></span> <span data-ttu-id="070b7-176">Při vytváření tabulky v první Nullable sloupce příklad byl explicitně definován.</span><span class="sxs-lookup"><span data-stu-id="070b7-176">When the table was created in the first example column nullability was explicitly defined.</span></span> <span data-ttu-id="070b7-177">Ve druhém příkladu, který je právě bylo výrazu a ve výchozím nastavení to by způsobilo definici hodnotu NULL.</span><span class="sxs-lookup"><span data-stu-id="070b7-177">In the second example it was just left to the expression and by default this would result in a NULL definition.</span></span>  

<span data-ttu-id="070b7-178">Chcete-li vyřešit tyto problémy musíte explicitně nastavit převodu typu a možnost použití hodnoty Null v `SELECT` část `CTAS` příkaz.</span><span class="sxs-lookup"><span data-stu-id="070b7-178">To resolve these issues you must explicitly set the type conversion and nullability in the `SELECT` portion of the `CTAS` statement.</span></span> <span data-ttu-id="070b7-179">V části Vytvoření tabulky nelze nastavit tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="070b7-179">You cannot set these properties in the create table part.</span></span>

<span data-ttu-id="070b7-180">Následující příklad ukazuje, jak opravit kód:</span><span class="sxs-lookup"><span data-stu-id="070b7-180">The example below demonstrates how to fix the code:</span></span>

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

<span data-ttu-id="070b7-181">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="070b7-181">Note the following:</span></span>

* <span data-ttu-id="070b7-182">CAST nebo CONVERT byl použit</span><span class="sxs-lookup"><span data-stu-id="070b7-182">CAST or CONVERT could have been used</span></span>
* <span data-ttu-id="070b7-183">IsNull – slouží k vynucení možnost použití hodnoty Null nejsou COALESCE</span><span class="sxs-lookup"><span data-stu-id="070b7-183">ISNULL is used to force NULLability not COALESCE</span></span>
* <span data-ttu-id="070b7-184">ISNULL je nejzevnější funkce</span><span class="sxs-lookup"><span data-stu-id="070b7-184">ISNULL is the outermost function</span></span>
* <span data-ttu-id="070b7-185">Druhá část ISNULL je konstanta, tzn. 0</span><span class="sxs-lookup"><span data-stu-id="070b7-185">The second part of the ISNULL is a constant i.e. 0</span></span>

> [!NOTE]
> <span data-ttu-id="070b7-186">Pro možnost použití hodnoty Null, být správně nastavena je potřeba použít `ISNULL` a není `COALESCE`.</span><span class="sxs-lookup"><span data-stu-id="070b7-186">For the nullability to be correctly set it is vital to use `ISNULL` and not `COALESCE`.</span></span> <span data-ttu-id="070b7-187">`COALESCE`není deterministický funkce a tak výsledkem výrazu bude vždy s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="070b7-187">`COALESCE` is not a deterministic function and so the result of the expression will always be NULLable.</span></span> <span data-ttu-id="070b7-188">`ISNULL`se liší.</span><span class="sxs-lookup"><span data-stu-id="070b7-188">`ISNULL` is different.</span></span> <span data-ttu-id="070b7-189">Je deterministický.</span><span class="sxs-lookup"><span data-stu-id="070b7-189">It is deterministic.</span></span> <span data-ttu-id="070b7-190">Proto když druhou částí `ISNULL` funkce je konstanta, nebo literál pak bude výsledná hodnota není NULL.</span><span class="sxs-lookup"><span data-stu-id="070b7-190">Therefore when the second part of the `ISNULL` function is a constant or a literal then the resulting value will be NOT NULL.</span></span>
> 
> 

<span data-ttu-id="070b7-191">Tento tip není právě užitečné k zajištění integrity výpočtů.</span><span class="sxs-lookup"><span data-stu-id="070b7-191">This tip is not just useful for ensuring the integrity of your calculations.</span></span> <span data-ttu-id="070b7-192">Je také důležité k přepnutí oddílu tabulky.</span><span class="sxs-lookup"><span data-stu-id="070b7-192">It is also important for table partition switching.</span></span> <span data-ttu-id="070b7-193">Představte si, že máte tato tabulka definován jako vaše fakt:</span><span class="sxs-lookup"><span data-stu-id="070b7-193">Imagine you have this table defined as your fact:</span></span>

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

<span data-ttu-id="070b7-194">Hodnota pole je však počítané výraz, který není součástí zdrojová data.</span><span class="sxs-lookup"><span data-stu-id="070b7-194">However, the value field is a calculated expression it is not part of the source data.</span></span>

<span data-ttu-id="070b7-195">Vytvoření oddílů datovou sadu můžete chtít provést:</span><span class="sxs-lookup"><span data-stu-id="070b7-195">To create your partitioned dataset you might want to do this:</span></span>

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

<span data-ttu-id="070b7-196">Dotaz by byl spuštěn perfektně dobře.</span><span class="sxs-lookup"><span data-stu-id="070b7-196">The query would run perfectly fine.</span></span> <span data-ttu-id="070b7-197">Problém je při pokusu o provedení přepínač oddílu.</span><span class="sxs-lookup"><span data-stu-id="070b7-197">The problem comes when you try to perform the partition switch.</span></span> <span data-ttu-id="070b7-198">Definice tabulek se neshodují.</span><span class="sxs-lookup"><span data-stu-id="070b7-198">The table definitions do not match.</span></span> <span data-ttu-id="070b7-199">Chcete-li definice tabulky odpovídat funkce CTAS má být změněn.</span><span class="sxs-lookup"><span data-stu-id="070b7-199">To make the table definitions match the CTAS needs to be modified.</span></span>

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

<span data-ttu-id="070b7-200">Můžete zobrazit proto konzistenci typu a udržování možnost použití hodnoty NULL vlastnosti funkce CTAS je vhodné engineering nejlepší.</span><span class="sxs-lookup"><span data-stu-id="070b7-200">You can see therefore that type consistency and maintaining nullability properties on a CTAS is a good engineering best practice.</span></span> <span data-ttu-id="070b7-201">Pomáhá zachovat integritu do výpočtů a také zajistí, že přepnutí oddílu je možné.</span><span class="sxs-lookup"><span data-stu-id="070b7-201">It helps to maintain integrity in your calculations and also ensures that partition switching is possible.</span></span>

<span data-ttu-id="070b7-202">Naleznete na webu MSDN pro další informace o použití [funkce CTAS][CTAS].</span><span class="sxs-lookup"><span data-stu-id="070b7-202">Please refer to MSDN for more information on using [CTAS][CTAS].</span></span> <span data-ttu-id="070b7-203">Je jedním z nejdůležitějších příkazy v Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="070b7-203">It is one of the most important statements in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="070b7-204">Ujistěte se, že rozumíte důkladně.</span><span class="sxs-lookup"><span data-stu-id="070b7-204">Make sure you thoroughly understand it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="070b7-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="070b7-205">Next steps</span></span>
<span data-ttu-id="070b7-206">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="070b7-206">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
