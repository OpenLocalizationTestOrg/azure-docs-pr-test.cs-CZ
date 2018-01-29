---
title: "Optimalizace transakcí pro SQL Data Warehouse | Microsoft Docs"
description: "Příručky nejlepších praktik o zápisu efektivní transakce aktualizace v Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 6f326f26-8a54-49df-a482-9c96a58db371
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: f9f19d75a37351b3562ce8c2f3629df14c5437c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Optimalizace transakcí pro SQL Data Warehouse
Tento článek vysvětluje, jak optimalizovat výkon transakcí kódu a současně minimalizujete její rizik dlouho odvolání.

## <a name="transactions-and-logging"></a>Transakce a protokolování
Transakce jsou důležitou součástí relační databázový stroj. SQL Data Warehouse používá při úpravě dat transakce. Tyto transakce může být explicitní nebo implicitní. Jeden `INSERT`, `UPDATE` a `DELETE` příkazy jsou všechny příklady implicitní transakce. Explicitní transakce jsou zapsány explicitně vývojáři pomocí `BEGIN TRAN`, `COMMIT TRAN` nebo `ROLLBACK TRAN` a jsou obvykle používány, pokud se více příkazů úpravy musí být vázáno společně v jedné jednotky atomic. 

Azure SQL Data Warehouse potvrdí změny do databáze pomocí protokolů transakcí. Každý distribuční má svou vlastní transakčního protokolu. Zápisy protokolu transakce jsou automatické. Není nutná žádná konfigurace. Ale a zároveň zaručí se tento proces zápisu zavést režijní náklady v systému. Zápisem transakčně efektivní kódu můžete minimalizovat tomuto vlivu. Transakčně efektivní kód široce spadá do dvou kategorií.

* Pokud je to možné konstruktů minimální úroveň protokolování
* Zpracování dat pomocí obor dávky, aby se zabránilo singulární dlouhotrvající transakce
* Použít oddíl přepínání vzor pro velké změny v daném oddílu

## <a name="minimal-vs-full-logging"></a>Minimální oproti úplné protokolování
Na rozdíl od plně protokolovaných operací, které používají ke sledování každé změně řádek transakčního protokolu, minimálně protokolovaných operací uchovávání informací o přidělení rozsahu a pouze metadata změny. Proto minimální protokolování zahrnuje protokolování pouze informace, které je potřeba vrácení transakce v případě selhání nebo explicitní žádost (`ROLLBACK TRAN`). Jak informace je mnohem méně sledovat v protokolu transakcí, provede se lépe než podobné velikosti plně protokolovaných operací minimálně protokolovaných operací. Kromě toho protože méně zápisy přejděte transakčního protokolu, je generována mnohem menší množství dat protokolu a stejně tak více vstupně-výstupních operací efektivní.

Omezení zabezpečení transakce se vztahují pouze na plně protokolovaných operací.

> [!NOTE]
> Minimálně protokolovaných operací se účastnit explicitních transakcí. Jak jsou sledovat všechny změny v přidělení struktury, je možné vrátit minimálně protokolovaných operací. Je důležité si uvědomit, že změna "minimálně" protokolu není zrušení protokolu.
> 
> 

## <a name="minimally-logged-operations"></a>Minimálně protokolovaných operací
Následující operace podporují minimálně protokolována:

* VYTVOŘENÍ TABLE AS SELECT ([FUNKCE CTAS][CTAS])
* PŘÍKAZ INSERT... VYBERTE
* VYTVOŘENÍ INDEXU
* PŘÍKAZ ALTER INDEX OPĚTOVNÉ SESTAVENÍ
* DROP INDEX
* ZKRÁCENÍ TABULKY
* ODPOJIT TABULKU
* PŘÍKAZ ALTER TABLE SWITCH PARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> Operace přesunu dat interní (například `BROADCAST` a `SHUFFLE`) nemá vliv omezení zabezpečení transakce.
> 
> 

## <a name="minimal-logging-with-bulk-load"></a>Minimální protokolování s hromadné načtení
`CTAS`a `INSERT...SELECT` jsou obě hromadné operace zatížení. Ale obě jsou ovlivněné definici cílové tabulky a závisí na scénáři zatížení. Dole je tabulku, která vysvětluje, pokud hromadné operace plně nebo minimálně zaznamenán:  

| Primární Index | Scénář zatížení | Režim protokolování |
| --- | --- | --- |
| Haldy |Všechny |**Minimální** |
| Clusterovaný Index |Prázdná cílová tabulka |**Minimální** |
| Clusterovaný Index |Načíst řádky se nepřekrývají s existující stránky v cíli |**Minimální** |
| Clusterovaný Index |Načíst řádky překrývá s existující stránky v cíli |Úplná |
| Clusterovaný Index Columnstore |Velikost dávky > = 102,400 za oddílu zarovnán distribuce |**Minimální** |
| Clusterovaný Index Columnstore |Velikost < 102,400 za oddílu zarovnán distribuční služby batch |Úplná |

Je vhodné poznamenat, že všechny zápisy k aktualizaci sekundární nebo neclusterované indexy bude vždy plně protokolovaných operací.

> [!IMPORTANT]
> SQL Data Warehouse je 60 distribuce. Proto za předpokladu, že všechny řádky jsou rozloženy rovnoměrně a cílová v jeden oddíl, dávku muset obsahovat 6,144,000 řádky nebo větší minimálně protokolovaných při zápisu do clusterovaný Columnstore Index. Pokud tabulka je rozdělena na oddíly a řádky vkládání span hranice oddílů, budete potřebovat 6,144,000 řádky za hranice oddílu za předpokladu, že i distribuci dat. Každý oddíl v každém distribučním musí nezávisle překročit prahovou hodnotu činí 102 400 řádek pro vložení do distribuce minimálně protokolovaných.
> 
> 

Načítání dat do tabulky neprázdný clusterovaný index často může obsahovat kombinaci plně protokolu a minimálně zaznamenané řádků. Clusterovaný index je vyrovnáváním stromu (b stromu) stránek. Pokud stránky zapisuje do již obsahuje sloupce z jiné transakci, pak tyto zápisy plně zaznamenán. Ale pokud stránka je prázdný pak zápis na této stránce minimálně zaznamenán.

## <a name="optimizing-deletes"></a>Optimalizace odstranění
`DELETE`je plně protokolovaných operací.  Pokud potřebujete odstranit velké množství dat v tabulce nebo oddíl, často má smysl Další `SELECT` data chcete zachovat, která může běžet jako minimálně protokolovaných operací.  K tomu, vytvořit novou tabulku s [funkce CTAS][CTAS].  Po vytvoření použít [přejmenovat] [ RENAME] vyměnit vaše staré tabulky s nově vytvořené tabulky.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT     *
FROM     [dbo].[FactInternetSales]
WHERE    [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Optimalizace aktualizace
`UPDATE`je plně protokolovaných operací.  Pokud je potřeba aktualizovat velký počet řádků v tabulce nebo oddíl může být často mnohem efektivnější pomocí minimálně protokolovaných operací, jako například [funkce CTAS] [ CTAS] Uděláte to tak.

V příkladu níže úplné tabulku aktualizace byl převeden na `CTAS` tak, aby minimální protokolování je možné.

V takovém případě zpětně přidáváme částku slevy na prodeje v tabulce:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(    CLUSTERED INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [!NOTE]
> Znovu vytvořit velké tabulky můžete využívat výhod pomocí funkce SQL Data Warehouse úlohy správy. Pro další podrobnosti naleznete v části úlohy správy v [souběžnosti] [ concurrency] článku.
> 
> 

## <a name="optimizing-with-partition-switching"></a>Optimalizace s přepnutí oddílu
Při velkém měřítku úpravy uvnitř [tabulky oddílu][table partition], pak oddíl přepínání vzor díky spoustu smysl. Pokud úprava dat je důležité a zahrnuje více oddílů, pak jednoduše iterování přes oddíly dosáhne stejného výsledku.

Postup provedení oddílu přepínače jsou následující:

1. Vytvořte prázdnou na oddíl
2. Tato aktualizace, proveďte funkce CTAS
3. Přepínač existujících dat do výstupní tabulky
4. Přepínač ve nová data
5. Vyčistit data

Však k identifikaci oddílů přepnout jsme bude nejprve nutné sestavení pomocné rutiny postup je dole. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,    @table_name               NVARCHAR(128)
,    @boundary_value           INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
WITH CTE
AS
(
SELECT     s.name                            AS [schema_name]
,        t.name                            AS [table_name]
,         p.partition_number                AS [ptn_nmbr]
,        p.[rows]                        AS [ptn_rows]
,        CAST(r.[value] AS INT)            AS [boundary_value]
FROM        sys.schemas                    AS s
JOIN        sys.tables                    AS t    ON  s.[schema_id]        = t.[schema_id]
JOIN        sys.indexes                    AS i    ON     t.[object_id]        = i.[object_id]
JOIN        sys.partitions                AS p    ON     i.[object_id]        = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes        AS h    ON     i.[data_space_id]    = h.[data_space_id]
JOIN        sys.partition_functions        AS f    ON     h.[function_id]        = f.[function_id]
LEFT JOIN    sys.partition_range_values    AS r     ON     f.[function_id]        = r.[function_id] 
                                                AND r.[boundary_id]        = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT    *
FROM    CTE
WHERE    [schema_name]        = @schema_name
AND        [table_name]        = @table_name
AND        [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Tento postup maximalizuje opětovné použití kódu a udržuje přepnutí příklad kompaktnější oddílu.

Následující kód ukazuje pět kroků uvedených výše k dosažení úplné oddíl přepínání rutiny.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE    OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]    SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))    +' TO [dbo].[FactInternetSales_out] PARTITION '    +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))    +' TO [dbo].[FactInternetSales] PARTITION '        +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Minimalizovat protokolování s malé balíků
Pro operace úpravy velkého množství dat má smysl rozdělit operaci do bloků dat nebo dávky k určení rozsahu pracovní jednotce.

Příklad pracovní najdete níže. Velikost dávky byla nastavena na trivial číslo chcete zvýraznit techniku. Ve skutečnosti bude podstatně větší velikost dávky. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
SELECT    ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,        SalesOrderNumber
,        SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE    [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE    @seq_start        INT = 1
,        @batch_iterator    INT = 1
,        @batch_size        INT = 50
,        @max_seq_nmbr    INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE    @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,        @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE    @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT    1
            FROM    #t t
            WHERE    seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND        FactInternetSales.SalesOrderNumber        = t.SalesOrderNumber
            AND        FactInternetSales.SalesOrderLineNumber    = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Pozastavení a škálování pokyny
Azure SQL Data Warehouse umožňuje pozastavit, obnovit a škálovat datový sklad na vyžádání. Při pozastavení nebo škálovat datový sklad SQL je důležité si uvědomit, že jakékoli během letu transakce budou ukončeny okamžitě; způsobuje všechny otevřené transakce zpět. Pokud vaše úlohy vystavila dlouho běžící a nedokončené úprava dat před operace pozastavení nebo určený počet číslic, tento pracovní bude muset být vrátit zpět. To může mít vliv na čas potřebný k pozastavení nebo škálování databáze Azure SQL Data Warehouse. 

> [!IMPORTANT]
> Obě `UPDATE` a `DELETE` jsou plně protokolovaných operací a tak tyto zpět/opakování operace může trvat výrazně déle, než ekvivalentní minimálně protokolována operace. 
> 
> 

Scénář Nejlepší je umožnit v cestě data změny transakce dokončení před pozastavení nebo škálování SQL Data Warehouse. Ale to nemusí být vždy praktické. Pro zmírnění rizik dlouho vrácení zpět, zvažte jednu z následujících možností:

* Znovu zapsat dlouhotrvající operace pomocí [funkce CTAS][CTAS]
* Operaci rozdělení do bloků; pracující na podmnožinu řádků

## <a name="next-steps"></a>Další kroky
V tématu [transakcí v SQL Data Warehouse] [ Transactions in SQL Data Warehouse] Další informace o úrovních izolace a omezení transakcí.  Přehled ostatní osvědčené postupy najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Transactions in SQL Data Warehouse]: ./sql-data-warehouse-develop-transactions.md
[table partition]: ./sql-data-warehouse-tables-partition.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

