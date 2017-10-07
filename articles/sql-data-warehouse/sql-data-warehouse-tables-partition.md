---
title: aaaPartitioning tabulek v SQL Data Warehouse | Microsoft Docs
description: "Začínáme s vytváření oddílů tabulky v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 6cef870c-114f-470c-af10-02300c58885d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: aa63c51562f3e6f83063320860b195e135a721e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a>Vytváření oddílů tabulky v SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Přehled][Overview]
> * [Datové typy][Data Types]
> * [Distribuce][Distribute]
> * [Index][Index]
> * [Oddíl][Partition]
> * [Statistiky][Statistics]
> * [Dočasné][Temporary]
> 
> 

Vytváření oddílů je podporován u všech typů tabulek SQL Data Warehouse; včetně clusterových columnstore, clusterovaný index a haldy.  Vytváření oddílů je podporováno také na všechny typy distribučních, včetně hodnoty hash nebo kruhové dotazování distribuován.  Vytváření oddílů umožňuje toodivide, které vaše data do menší skupiny dat a ve většině případů dělení se provádí na sloupci, datum.

## <a name="benefits-of-partitioning"></a>Výhody dělení
Vytváření oddílů využívat data výkonu údržby a dotazů.  Jestli výhody oba, nebo pouze jeden, je závislá na tom, jak načíst data a jestli hello stejný sloupec lze použít pro obě účely, protože vytváření oddílů je možné provést pouze na jeden sloupec.

### <a name="benefits-tooloads"></a>Výhody tooloads
Hello Hlavní výhoda vytváření oddílů v SQL Data Warehouse je zvýšit efektivitu hello a výkon načítání dat pomocí odstranění oddílu, přepínání a slučování.  Ve většině případů, které jsou data rozdělena na datum vázaný sloupec, který je úzce toohello pořadí datových hello je načíst toohello databáze.  Jeden z hello největší výhody použití dat toomaintain oddíly ho hello předcházení protokolování transakcí.  Když jednoduše vkládání, aktualizaci nebo odstranění dat může být hello nejjednodušší způsob, s malým množstvím myšlenku a úsilí, pomocí dělení během procesu vaše zatížení může podstatně zlepšit výkon.

Přepnutí oddílu můžete být použité tooquickly odeberte nebo nahraďte oddíl tabulky.  Tabulka faktů prodeje například může obsahovat pouze data pro hello posledních 36 měsíců.  Na konci hello v každém měsíci hello nejstarší měsíc prodejních dat je odstraněn z tabulky hello.  Tato data může odstranit pomocí odstranit příkaz toodelete hello data pro hello nejstarší měsíc.  Ale odstraňování velké množství dat řádek po řádku příkazem delete může trvat velmi dlouho, stejně jako hello riziko velké transakcí, které může trvat dlouhou dobu toorollback, pokud dojde k chybě.  Více optimální přístup je toosimply rozevírací hello nejstarší oddílu data.  Kde odstranění jednotlivých řádků hello může trvat hodiny, odstraňování celý oddíl může trvat sekund.

### <a name="benefits-tooqueries"></a>Výhody tooqueries
Dělení může být také použít tooimprove výkon dotazů.  Pokud dotaz použije filtr na sloupec rozdělení, to můžete omezit hello kontroly tooonly hello kvalifikující oddíly, které můžou být mnohem menší podmnožinu dat hello, zabraňující prohledání úplnou tabulky.  Při zavedení hello Clusterované indexy columnstore jsou méně výhodné hello predikátem odstranění výkonnostních výhod, ale v některých případech může být tooqueries výhody.  Například pokud hello prodeje fakt tabulka je rozdělena na oddíly do 36 měsíců pomocí hello datum prodeje pole, pak dotazy, které filtrovat hello prodej datum můžete přeskočit hledání v oddíly, které neodpovídají hello filtru.

## <a name="partition-sizing-guidance"></a>Pokyny k dimenzování oddílu
Při vytváření oddílů lze použít tooimprove výkonu některých scénářích, vytvoření tabulky s **příliš mnoho** oddíly může narušit výkonnost za určitých okolností.  Tyto problémy jsou především pro Clusterované tabulky columnstore.  U oddílů toobe užitečné, je důležité toounderstand při toouse vytváření oddílů a hello počet toocreate oddíly.  Existuje žádné pevné pravidlo rychlé jako toohow mnoha oddílů jsou příliš mnoho, závisí na vaše data a kolik oddíly jsou načítání toosimultaneously.  Ale jako obecné pravidlo, vezměte v úvahu přidávání 10s too100s oddílů není 1000s.

Při vytváření dělení na **Clusterové columnstore** tabulky, je důležité tooconsider, kolik řádků se nebude zobrazovat v každém oddílu.  Pro optimální komprese a výkon Clusterované tabulky columnstore je potřeba minimálně 1 milionu řádků na distribuce a oddíl.  Před vytvořením oddíly, SQL Data Warehouse každá tabulka již rozdělí na 60 distribuované databáze.  Žádné rozdělení přidané tooa tabulce je navíc toohello distribuce vytvořit pozadí hello.  V tomto příkladu, pokud tabulka faktů prodeje hello obsažené 36 měsíční oddíly a vzhledem k tomu, že SQL Data Warehouse je 60 distribuce hello prodeje fakt, že tabulky by měl obsahovat 60 milionu řádků měsíčně nebo 2.1 miliardy řádků, když jsou naplněny všechny měsíce.  Pokud tabulka obsahuje výrazně méně řádků než hello Doporučená minimální počet řádků na jeden oddíl, zvažte použití méně oddíly v pořadí toomake zvýšení hello počet řádků na jeden oddíl.  Viz také hello [indexování] [ Index] článek, který obsahuje dotazy, které lze spustit v SQL Data Warehouse tooassess hello kvalitu indexy columnstore clusteru.

## <a name="syntax-difference-from-sql-server"></a>Syntaxe rozdíl oproti systému SQL Server
SQL Data Warehouse zavádí zjednodušenou definice oddíly, který se mírně liší od systému SQL Server.  Vytváření oddílů funkce a schémata nejsou použity v SQL Data Warehouse, jako jsou v systému SQL Server.  Místo toho stačí toodo je identifikovat oddílů sloupce a hello hranic body.  Hello syntaxe vytváření oddílů se mírně liší v systému SQL Server, jsou hello stejné základní koncepty hello.  SQL Server a SQL Data Warehouse podporují jeden sloupec oddílu na jednu tabulku, která může být pohyboval oddílu.  toolearn Další informace o vytváření oddílů, najdete v části [rozdělena na oddíly tabulky a indexy][Partitioned Tables and Indexes].

Hello následujícím příkladu SQL Data Warehouse rozdělena na oddíly [CREATE TABLE] [ CREATE TABLE] příkaz oddíly tabulka FactInternetSales hello hello OrderDateKey sloupec:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Migrace, vytváření oddílů v systému SQL Server
toomigrate systému SQL Server jednoduše oddílu tooSQL definice datového skladu:

* Odstranění hello systému SQL Server [schéma oddílu][partition scheme].
* Přidat hello [oddílu funkce] [ partition function] definice tooyour vytvořit tabulku.

Pokud migrujete dělenou tabulku z hello instance serveru SQL pod SQL vám může pomoct toointerrogate hello počet řádků, které jsou v každém oddílu.  Uvědomte si, že pokud hello stejnou členitost rozdělení se používá v SQL Data Warehouse, hello počet řádků na jeden oddíl se sníží o faktor 60.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Správa zatížení
Jeden aspekt toofactor poslední díl v toohello tabulce oddílu rozhodnutí je [úlohy správy][workload management].  Úlohy správy v SQL Data Warehouse je primárně hello Správa paměti a souběžnosti.  V SQL Data Warehouse hello je maximální velikost paměti přidělené tooeach distribuční během provádění dotazu třídy upraveny prostředků.  V ideálním případě budou vaše oddíly velké s ohledem na dalších faktorech, jako je vytváření Clusterované indexy columnstore hello paměti potřebám.  Clusterovaný benefit indexy columnstore výrazně při jejich přidělení více paměti.  Proto je vhodné tooensure, který znovu sestavit index oddílu není nedostatek paměti. Zvýšení hello množství paměti k dispozici tooyour dotazu lze dosáhnout přepínání z hello výchozí role, smallrc, tooone z jiných rolí, například largerc hello.

Informace o hello přidělení paměti na jeden distribuční je k dispozici pomocí dotazu na zobrazení dynamické správy Správce prostředků hello. Ve skutečnosti vaší přidělení paměti bude menší než následující hello obrázky. To však poskytuje úroveň pokyny, které můžete použít, když vaše oddíly pro operace správy dat pro definování velikosti.  Zkuste tooavoid Změna velikosti vašeho oddíly nad rámec poskytovaný Třída prostředků se velmi velké hello přidělení paměti hello. Pokud vaše oddíly růst nad rámec tohoto obrázku spuštěním hello riziko přetížení paměti, což pak vede tooless optimální komprese.

```sql
SELECT  rp.[name]                                AS [pool_name]
,       rp.[max_memory_kb]                        AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                    AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576                AS [mex_memory_gb]
,       rp.[max_memory_percent]                    AS [max_memory_percent]
,       wg.[name]                                AS [group_name]
,       wg.[importance]                            AS [group_importance]
,       wg.[request_max_memory_grant_percent]    AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups    wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools    rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Přepnutí oddílu
SQL Data Warehouse podporuje oddílu rozdělení, sloučení a přepínání. Každá z těchto funkcí je excuted pomocí hello [příkaz ALTER TABLE] [ ALTER TABLE] příkaz.

oddíly tooswitch mezi dvěma tabulkami musíte zajistit, že oddíly hello zarovnat na jejich odpovídající hranice a jestli se shodují hello Definice tabulky. Jako omezení check nejsou k dispozici tooenforce hello rozsahu hodnot v tabulce hello zdrojová tabulka musí obsahovat hello stejná jako cílová tabulka hello oddílu hranice. Pokud to není hello případ, pak hello oddílu přepínače selžou, protože metadata oddílu hello se nebudou synchronizovat.

### <a name="how-toosplit-a-partition-that-contains-data"></a>Jak toosplit oddílu, který obsahuje data
Hello nejúčinnější metoda toosplit oddíl, který už obsahuje data je toouse `CTAS` příkaz. Pokud je tabulka oddílů hello Clusterové columnstore pak hello tabulky oddíl musí být prázdný předtím, než je možné rozdělit.

Níže je ukázka tabulku oddílů columnstore obsahující jeden řádek v každém oddílu:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [!NOTE]
> Objekt pro vytváření hello statistiky jsme Ujistěte se, že tento metadat tabulky je přesnější. Pokud jsme vynechat, vytváření statistik, SQL Data Warehouse použije výchozí hodnoty. Pro Zkontrolujte prosím podrobnosti o statistiky [statistiky][statistics].
> 
> 

Jsme dotazu pro počet řádků hello pomocí hello `sys.partitions` katalogu zobrazení:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Pokud tato tabulka pokusíme toosplit jsme dojde k chybě:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Msg 35346, úroveň 15, State 1, řádek 44 ROZDĚLIT klauzule příkazu ALTER partition se nezdařila, protože hello oddíl není prázdný.  V lze rozdělit jen prázdné oddíly, když existuje columnstore index v tabulce hello. Zvažte zakázání hello index columnstore před spuštěním příkazu ALTER PARTITION hello a pak znovu sestavit hello columnstore index po dokončení příkazu ALTER PARTITION.

Ale můžeme použít `CTAS` toocreate nové tabulky toohold naše data.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Jako hranice oddílů hello je zarovnán přepínač je povolená. Zdrojová tabulka hello to bude nechte prázdné oddílu, který může následně rozdělit.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 too FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Všechno, co je ponechán toodo je tooalign naše data toohello oddílu nové hranice pomocí `CTAS` a přepněte zpět v hlavní tabulka toohello našich dat

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 toodbo.FactInternetSales PARTITION 2;
```

Po dokončení hello pohybů hello dat je statistické údaje vhodné toorefresh hello na hello cílové tabulky tooensure hello nový distribuční hello dat v jejich příslušné oddíly přesně odrážel:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabulka dělení zdrojového kódu
tooavoid vaše definice tabulky z **koroze** v systému správy zdrojů může být vhodné tooconsider hello následující postup:

1. Vytvoření tabulky hello jako dělenou tabulku, ale žádné hodnoty pro oddíl

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

1. `SPLIT`Hello tabulky jako součást procesu nasazení hello:

```sql
-- Create a table containing hello partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over hello partition boundaries and split hello table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

S Tento přístup hello zůstane statické kódu ve správě zdrojového kódu a dělení hodnoty hranice hello jsou povoleny toobe dynamické; vyvíjejí hello skladu v čase.

## <a name="next-steps"></a>Další kroky
články hello toolearn více, najdete na [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuci tabulku] [ Distribute], [Indexování tabulku][Index], [zachování statistiky tabulky] [ Statistics] a [ Dočasné tabulky][Temporary].  Další informace o osvědčených postupech najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[workload management]: ./sql-data-warehouse-develop-concurrency.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Partitioned Tables and Indexes]: https://msdn.microsoft.com/library/ms190787.aspx
[ALTER TABLE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[partition function]: https://msdn.microsoft.com/library/ms187802.aspx
[partition scheme]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
