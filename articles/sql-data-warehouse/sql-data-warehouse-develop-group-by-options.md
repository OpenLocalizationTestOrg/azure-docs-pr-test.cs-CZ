---
title: "aaaGroup pomocí možností v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro implementaci skupiny pomocí možnosti v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f95a1e43-768f-4b7b-8a10-8a0509d0c871
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: cc443c2af4e3ef2babd74d78aa6fb57bb3c1c7ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="group-by-options-in-sql-data-warehouse"></a><span data-ttu-id="42932-103">Seskupit podle možností v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="42932-103">Group by options in SQL Data Warehouse</span></span>
<span data-ttu-id="42932-104">Hello [GROUP BY] [ GROUP BY] klauzule tooaggregate data tooa souhrnné sadu řádků.</span><span class="sxs-lookup"><span data-stu-id="42932-104">hello [GROUP BY][GROUP BY] clause is used tooaggregate data tooa summary set of rows.</span></span> <span data-ttu-id="42932-105">Je také několik možností, které rozšiřují jeho funkce této nutné toobe fungovala kolem jako nejsou podporovány přímo pomocí Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="42932-105">It also has a few options that extend it's functionality that need toobe worked around as they are not directly supported by Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="42932-106">Tyto možnosti jsou</span><span class="sxs-lookup"><span data-stu-id="42932-106">These options are</span></span>

* <span data-ttu-id="42932-107">GROUP BY se ZAHRNUTÍM</span><span class="sxs-lookup"><span data-stu-id="42932-107">GROUP BY with ROLLUP</span></span>
* <span data-ttu-id="42932-108">GROUPING SETS</span><span class="sxs-lookup"><span data-stu-id="42932-108">GROUPING SETS</span></span>
* <span data-ttu-id="42932-109">Klauzule GROUP BY datové KRYCHLE</span><span class="sxs-lookup"><span data-stu-id="42932-109">GROUP BY with CUBE</span></span>

## <a name="rollup-and-grouping-sets-options"></a><span data-ttu-id="42932-110">Souhrn a seskupení nastaví možnosti</span><span class="sxs-lookup"><span data-stu-id="42932-110">Rollup and grouping sets options</span></span>
<span data-ttu-id="42932-111">Hello nejjednodušší možnost tady je toouse `UNION ALL` místo tooperform hello kumulativní namísto spoléhání na hello explicitní syntaxe.</span><span class="sxs-lookup"><span data-stu-id="42932-111">hello simplest option here is toouse `UNION ALL` instead tooperform hello rollup rather than relying on hello explicit syntax.</span></span> <span data-ttu-id="42932-112">výsledek Hello je hello přesně stejnou</span><span class="sxs-lookup"><span data-stu-id="42932-112">hello result is exactly hello same</span></span>

<span data-ttu-id="42932-113">Dole je příklad skupiny příkazem pomocí hello `ROLLUP` možnost:</span><span class="sxs-lookup"><span data-stu-id="42932-113">Below is an example of a group by statement using hello `ROLLUP` option:</span></span>

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

<span data-ttu-id="42932-114">Pomocí KUMULATIVNÍ jsme odeslali žádost o hello agregace následující:</span><span class="sxs-lookup"><span data-stu-id="42932-114">By using ROLLUP we have requested hello following aggregations:</span></span>

* <span data-ttu-id="42932-115">Zemí a oblastí</span><span class="sxs-lookup"><span data-stu-id="42932-115">Country and Region</span></span>
* <span data-ttu-id="42932-116">Země</span><span class="sxs-lookup"><span data-stu-id="42932-116">Country</span></span>
* <span data-ttu-id="42932-117">Celkový součet</span><span class="sxs-lookup"><span data-stu-id="42932-117">Grand Total</span></span>

<span data-ttu-id="42932-118">tooreplace to budete potřebovat toouse `UNION ALL`; určení agregace hello potřeba explicitně tooreturn hello stejné výsledky:</span><span class="sxs-lookup"><span data-stu-id="42932-118">tooreplace this you will need toouse `UNION ALL`; specifying hello aggregations required explicitly tooreturn hello same results:</span></span>

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

<span data-ttu-id="42932-119">Pro GROUPING SETS jsme toodo stačí použít stejný hlavní hello ale vytvořit pouze UNION ALL oddíly pro hello úrovně agregace chceme toosee</span><span class="sxs-lookup"><span data-stu-id="42932-119">For GROUPING SETS all we need toodo is adopt hello same principal but only create UNION ALL sections for hello aggregation levels we want toosee</span></span>

## <a name="cube-options"></a><span data-ttu-id="42932-120">Možnosti datové krychle</span><span class="sxs-lookup"><span data-stu-id="42932-120">Cube options</span></span>
<span data-ttu-id="42932-121">Je možné toocreate skupiny podle s datové KRYCHLE hello UNION ALL přístup.</span><span class="sxs-lookup"><span data-stu-id="42932-121">It is possible toocreate a GROUP BY WITH CUBE using hello UNION ALL approach.</span></span> <span data-ttu-id="42932-122">Hello problém je, že hello kód může být pracné a nepraktické.</span><span class="sxs-lookup"><span data-stu-id="42932-122">hello problem is that hello code can quickly become cumbersome and unwieldy.</span></span> <span data-ttu-id="42932-123">toomitigate to tímto způsobem můžete pokročilejší přístup.</span><span class="sxs-lookup"><span data-stu-id="42932-123">toomitigate this you can use this more advanced approach.</span></span>

<span data-ttu-id="42932-124">Použijeme hello příkladu výše.</span><span class="sxs-lookup"><span data-stu-id="42932-124">Let's use hello example above.</span></span>

<span data-ttu-id="42932-125">prvním krokem Hello je toodefine hello 'datové krychle, který definuje všechny úrovně hello agregace, že má být toocreate.</span><span class="sxs-lookup"><span data-stu-id="42932-125">hello first step is toodefine hello 'cube' that defines all hello levels of aggregation that we want toocreate.</span></span> <span data-ttu-id="42932-126">Je důležité tootake poznamenejte hello CROSS JOIN hello dvě odvozené tabulky.</span><span class="sxs-lookup"><span data-stu-id="42932-126">It is important tootake note of hello CROSS JOIN of hello two derived tables.</span></span> <span data-ttu-id="42932-127">Abychom to generuje všechny úrovně hello.</span><span class="sxs-lookup"><span data-stu-id="42932-127">This generates all hello levels for us.</span></span> <span data-ttu-id="42932-128">Hello zbytek hello kód skutečně existuje pro formátování.</span><span class="sxs-lookup"><span data-stu-id="42932-128">hello rest of hello code is really there for formatting.</span></span>

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

<span data-ttu-id="42932-129">výsledky Hello hello funkce CTAS si můžete prohlédnout níže:</span><span class="sxs-lookup"><span data-stu-id="42932-129">hello results of hello CTAS can be seen below:</span></span>

![][1]

<span data-ttu-id="42932-130">druhým krokem Hello je toospecify, které cílové tabulky toostore provizorní výsledků:</span><span class="sxs-lookup"><span data-stu-id="42932-130">hello second step is toospecify a target table toostore interim results:</span></span>

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

<span data-ttu-id="42932-131">třetí krok Hello je tooloop nad naší datové krychle sloupců provádění hello agregace.</span><span class="sxs-lookup"><span data-stu-id="42932-131">hello third step is tooloop over our cube of columns performing hello aggregation.</span></span> <span data-ttu-id="42932-132">Hello dotazu budou spuštěny jednou pro každý řádek v dočasné tabulce hello #Cube a uložte výsledky hello v dočasné tabulce hello #Results</span><span class="sxs-lookup"><span data-stu-id="42932-132">hello query will run once for every row in hello #Cube temporary table and store hello results in hello #Results temp table</span></span>

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

<span data-ttu-id="42932-133">Nakonec hello výsledky můžete vrátit načtením jednoduše z dočasné tabulky hello #Results</span><span class="sxs-lookup"><span data-stu-id="42932-133">Lastly we can return hello results by simply reading from hello #Results temporary table</span></span>

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

<span data-ttu-id="42932-134">Kód hello rozdělení do oddílů a generování opakování hello konstrukce kódu stane lepší spravovat a údržba.</span><span class="sxs-lookup"><span data-stu-id="42932-134">By breaking hello code up into sections and generating a looping construct hello code becomes more manageable and maintainable.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42932-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42932-135">Next steps</span></span>
<span data-ttu-id="42932-136">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="42932-136">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROUP BY]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
