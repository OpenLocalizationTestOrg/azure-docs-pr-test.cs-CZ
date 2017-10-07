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
# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Vytvoření tabulky jako vyberte (funkce CTAS) v SQL Data Warehouse
Vytvoření tabulky jako vyberte nebo `CTAS` je jedním z hello nejdůležitější funkce T-SQL dostupné. Je plně parallelized operaci, která vytvoří novou tabulku podle hello výstup příkazu SELECT. `CTAS`je snadno a rychle toocreate hello kopii tabulky. Tento dokument obsahuje příklady a osvědčené postupy pro `CTAS`.

## <a name="selectinto-vs-ctas"></a>VYBERTE... DO vs. CTAS
Můžete zvážit `CTAS` jako extrémně účtovat verze `SELECT..INTO`.

Dole je příklad jednoduchou `SELECT..INTO` příkaz:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

V předchozím příkladu hello `[dbo].[FactInternetSales_new]` by se vytvořily jako ROUND_ROBIN distribuované tabulku s INDEXEM COLUMNSTORE v clusteru na něm jsou tyto výchozí hodnoty tabulky hello v Azure SQL Data Warehouse.

`SELECT..INTO`však neumožňuje toochange index buď hello distribuční metody nebo hello zadejte v rámci operace hello. To je, kdy `CTAS` odeslán.

tooconvert hello výše příliš`CTAS` je poměrně jednoduché:

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

S `CTAS` jsou možné toochange obě hello distribuci dat v tabulce hello, jakož i hello typ tabulky. 

> [!NOTE]
> Pokud zkoušíte pouze toochange hello index v vaší `CTAS` operace a hello zdrojová tabulka je distribuovat algoritmu hash a vaše `CTAS` provede operaci nejvhodnější, pokud chcete zachovat hello stejného typu distribuční sloupce a data. Tím se vyhnete křížové distribuční přesun dat během operace hello, což je efektivnější.
> 
> 

## <a name="using-ctas-toocopy-a-table"></a>Pomocí funkce CTAS toocopy tabulku
Možná jedním z nejčastějších hello používá `CTAS` vytváří kopie tabulku, ve kterém můžete změnit hello DDL. Pokud třeba jste původně vytvořili tabulku jako `ROUND_ROBIN` a teď ho chcete změnit distribuovaných tooa tabulky na sloupci, `CTAS` je, jak by změna hello distribuční sloupce. `CTAS`může být také použít toochange typy rozdělení do oddílů, indexování nebo sloupec.

Řekněme, že jste vytvořili tuto tabulku pomocí hello výchozí typ distribuce `ROUND_ROBIN` distribuované vzhledem k tomu, že žádný distribuční sloupec byl zadán v hello `CREATE TABLE`.

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

Nyní chcete toocreate novou kopii této tabulce s indexem Columnstore v clusteru tak, aby můžete využít výhod hello výkon tabulky Columnstore v clusteru. Chcete taky toodistribute této tabulky na ProductKey vzhledem k tomu, že se očekává spojení na tomto sloupci a chcete tooavoid přesun dat během spojení na ProductKey. Nakonec chcete taky tooadd vytváření oddílů na OrderDateKey, takže můžete rychle odstranit stará data odstranit staré oddíly. Zde je hello funkce CTAS příkaz, který by vaše staré tabulka zkopírujte do nové tabulky.

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

Nakonec můžete přejmenovat vaší tooswap tabulky v nové tabulce a pak vyřadit staré tabulky.

```sql
RENAME OBJECT FactInternetSales tooFactInternetSales_old;
RENAME OBJECT FactInternetSales_new tooFactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.  V pořadí tooget hello nejlepší výkon ze své dotazy je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením hello nebo dojít k významné změny v datech hello.  Podrobné vysvětlení statistiky najdete v tématu hello [statistiky] [ Statistics] tématu ve skupině témat věnovaných vývoji hello.
> 
> 

## <a name="using-ctas-toowork-around-unsupported-features"></a>Pomocí funkce CTAS toowork kolem nepodporované funkce
`CTAS`může být také použít toowork kolem počet níže uvedené hello nepodporované funkce. Často to může být toobe win/win situaci jako pouze váš kód bude kompatibilní, ale je často spustí rychleji SQL Data Warehouse. Toto je v důsledku plně parallelized návrh. Mezi scénáře, které může být kolem pracovali funkce CTAS patří:

* ANSI spojení na aktualizace
* ANSI spojení na odstranění
* MERGE – příkaz

> [!NOTE]
> Zkuste toothink "funkce CTAS první". Pokud se domníváte, že vám může pomoct vyřešit problém pomocí `CTAS` a který je obvykle hello nejlepší způsob, jak tooapproach ho - i v případě, že v důsledku píšete další data.
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a>Připojení k nahrazení ANSI pro příkazy aktualizace
Můžete zjistit, že máte komplexní aktualizace, která spojuje více než dvě tabulky pomocí ANSI připojení syntaxe tooperform hello aktualizovat nebo odstranit.

Představte si, že jste měli tooupdate v této tabulce:

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

původní dotaz Hello může mít hledá přibližně takto:

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

Vzhledem k tomu, že SQL Data Warehouse nepodporuje ANSI Příkazy JOIN v hello `FROM` klauzuli `UPDATE` prohlášení, nelze zkopírovat tento kód přes bez provedení změn mírně.

Můžete použít kombinaci `CTAS` a implicitní připojení tooreplace tento kód:

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

## <a name="ansi-join-replacement-for-delete-statements"></a>Odstranit připojení k nahrazení ANSI pro příkazy
Někdy hello nejlepší přístup pro odstranění dat je toouse `CTAS`. Místo odstranění dat hello jednoduše vyberte hello data, která chcete tookeep. Tato především pro `DELETE` příkazy, které používají připojení syntaxe, protože SQL Data Warehouse nepodporuje spojení ANSI v hello ansi `FROM` klauzuli `DELETE` příkaz.

Příklad převedený příkazu DELETE je k dispozici následující:

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

## <a name="replace-merge-statements"></a>Nahraďte příkazy merge
Příkazy Merge lze nahradit, alespoň v rámci, pomocí `CTAS`. Může konsolidovat hello `INSERT` a hello `UPDATE` do jednoho příkazu. Odstraněné záznamy potřebovat toobe vypnout byl uzavřen za druhý příkaz.

Příklad `UPSERT` je k dispozici následující:

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

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>Funkce CTAS doporučení: explicitně stavu datový typ a možnost použití hodnoty Null výstupu
Při migraci kódu můžete zjistit, že spustíte přes tento typ kódování vzoru:

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

Instinktivně si myslíte by bylo nutné migrovat tento kód tooa funkce CTAS a by byly správné. Je však skrytá problém tady.

Hello následující kód nepřinese hello stejný výsledek:

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

Všimněte si, hello sloupec "výsledek" představuje předat hodnoty typu a možnost použití hodnoty NULL dat hello hello výrazu. To může způsobit toosubtle odchylky hodnoty, pokud nejste opatrní.

Zkuste následující hello jako příklad:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Hodnota Hello uložené pro výsledek se liší. Jako hello trvalou hodnotu ve sloupci výsledek hello se používá v jiné se změní i větších výrazy hello chyby.

![][1]

To je zvlášť důležité pro data migrace. I když je pravděpodobně přesnější hello druhého dotazu došlo k potížím. Hello dat by být různých porovnání toohello zdrojovém systému a který vede tooquestions integrita hello migraci. Toto je jedna z výjimečných případech, kdy hello "nesprávný" odpovědí ve skutečnosti hello vpravo jeden!

Hello důvod vidíme tento rozdíl mezi dvěma hello výsledky je mimo provoz tooimplicit typ přetypování. V hello první tabulky hello příklad definuje hello definici sloupce. Pokud je vložit řádek hello dochází k konverzi implicitní typu. V druhém příkladu hello neexistuje žádný typ implicitní převod jako hello výraz definuje datový typ sloupce hello. Všimněte si že také hello sloupce v druhém příkladu hello byla definována jako sloupec s možnou hodnotou Null zatímco v prvním příkladu hello má není. Když hello tabulka byla vytvořena v byl explicitně definován hello první příklad sloupec možnost použití hodnoty Null. V druhém příkladu hello byl právě zůstane toohello výraz a ve výchozím nastavení to by způsobilo definici hodnotu NULL.  

tooresolve těchto problémů musíte explicitně nastavit převod typů hello a možnost použití hodnoty Null v hello `SELECT` část hello `CTAS` příkaz. Nelze nastavit tyto vlastnosti v hello vytvořit část tabulky.

Hello Příklad dole ukazuje, jak toofix hello kódu:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Vezměte na vědomí následující hello:

* CAST nebo CONVERT byl použit
* ISNULL je použité tooforce možnost použití hodnoty Null nejsou COALESCE
* ISNULL je nejzevnější funkce hello
* Druhá část hello ISNULL Hello je konstanta, tzn. 0

> [!NOTE]
> Pro možnost použití hodnoty Null toobe hello správně nastavené je životně důležité toouse `ISNULL` a není `COALESCE`. `COALESCE`není deterministický funkce a tak hello výsledek výrazu hello bude vždy s možnou hodnotou Null. `ISNULL`se liší. Je deterministický. Proto když hello druhou částí hello `ISNULL` funkce je konstanta, nebo literál pak bude hello výsledná hodnota není NULL.
> 
> 

Tento tip není právě užitečné pro zajištění integrity hello vaší výpočtů. Je také důležité k přepnutí oddílu tabulky. Představte si, že máte tato tabulka definován jako vaše fakt:

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

Hello hodnota pole je však počítané výraz, který není součástí hello zdrojová data.

toocreate oddílů datovou sadu můžete chtít toodo toto:

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

dotaz Hello by spustit perfektně dobře. Hello problém se dodává se při tooperform hello oddílu přepínače. definice tabulek Hello se neshodují. definice tabulek hello toomake odpovídat hello funkce CTAS musí toobe upravit.

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

Můžete zobrazit proto konzistenci typu a udržování možnost použití hodnoty NULL vlastnosti funkce CTAS je vhodné engineering nejlepší. Pomáhá toomaintain integrity do výpočtů a také zajistí, že přepnutí oddílu je možné.

Další informace o používání naleznete tooMSDN [funkce CTAS][CTAS]. Je jedním z nejdůležitějších příkazy hello v Azure SQL Data Warehouse. Ujistěte se, že rozumíte důkladně.

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
