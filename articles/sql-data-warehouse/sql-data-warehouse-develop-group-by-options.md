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
# <a name="group-by-options-in-sql-data-warehouse"></a>Seskupit podle možností v SQL Data Warehouse
Hello [GROUP BY] [ GROUP BY] klauzule tooaggregate data tooa souhrnné sadu řádků. Je také několik možností, které rozšiřují jeho funkce této nutné toobe fungovala kolem jako nejsou podporovány přímo pomocí Azure SQL Data Warehouse.

Tyto možnosti jsou

* GROUP BY se ZAHRNUTÍM
* GROUPING SETS
* Klauzule GROUP BY datové KRYCHLE

## <a name="rollup-and-grouping-sets-options"></a>Souhrn a seskupení nastaví možnosti
Hello nejjednodušší možnost tady je toouse `UNION ALL` místo tooperform hello kumulativní namísto spoléhání na hello explicitní syntaxe. výsledek Hello je hello přesně stejnou

Dole je příklad skupiny příkazem pomocí hello `ROLLUP` možnost:

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

Pomocí KUMULATIVNÍ jsme odeslali žádost o hello agregace následující:

* Zemí a oblastí
* Země
* Celkový součet

tooreplace to budete potřebovat toouse `UNION ALL`; určení agregace hello potřeba explicitně tooreturn hello stejné výsledky:

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

Pro GROUPING SETS jsme toodo stačí použít stejný hlavní hello ale vytvořit pouze UNION ALL oddíly pro hello úrovně agregace chceme toosee

## <a name="cube-options"></a>Možnosti datové krychle
Je možné toocreate skupiny podle s datové KRYCHLE hello UNION ALL přístup. Hello problém je, že hello kód může být pracné a nepraktické. toomitigate to tímto způsobem můžete pokročilejší přístup.

Použijeme hello příkladu výše.

prvním krokem Hello je toodefine hello 'datové krychle, který definuje všechny úrovně hello agregace, že má být toocreate. Je důležité tootake poznamenejte hello CROSS JOIN hello dvě odvozené tabulky. Abychom to generuje všechny úrovně hello. Hello zbytek hello kód skutečně existuje pro formátování.

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

výsledky Hello hello funkce CTAS si můžete prohlédnout níže:

![][1]

druhým krokem Hello je toospecify, které cílové tabulky toostore provizorní výsledků:

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

třetí krok Hello je tooloop nad naší datové krychle sloupců provádění hello agregace. Hello dotazu budou spuštěny jednou pro každý řádek v dočasné tabulce hello #Cube a uložte výsledky hello v dočasné tabulce hello #Results

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

Nakonec hello výsledky můžete vrátit načtením jednoduše z dočasné tabulky hello #Results

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Kód hello rozdělení do oddílů a generování opakování hello konstrukce kódu stane lepší spravovat a údržba.

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROUP BY]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
