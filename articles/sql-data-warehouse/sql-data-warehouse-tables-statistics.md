---
title: statistiky aaaManaging u tabulek v SQL Data Warehouse | Microsoft Docs
description: "Začínáme s statistiky pro tabulky v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: c9521dc47891f68d124e77a53e2e15d03275caaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Správa statistiky u tabulek v SQL Data Warehouse
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

Hello další SQL Data Warehouse ví o vašich dat hello rychlejší ho můžete spouštět dotazy na data.  Hello způsobem, který říct SQL Data Warehouse o vašich dat je shromažďování statistických údajů o vaše data.  S statistické údaje o vašich dat je jedním z nejdůležitějších věcí hello můžete provést toooptimize své dotazy.  Statistiky pomoci vytvořit hello optimální plán pro své dotazy SQL Data Warehouse.  To je proto hello SQL Data Warehouse dotazu pro optimalizaci je s náklady na základě pro optimalizaci.  To znamená porovná hello náklady na různé plány dotazů a potom vybere hello plán s nejnižší náklady hello, které by se měly také hello plán, který provede nejrychlejší hello.

Statistiku lze vytvořit na jeden sloupec, více sloupců nebo index tabulky.  Statistiky jsou uloženy v histogram, který zachycuje hello rozsah a selektivitu hodnot.  Toto je zajímají hlavně o při hello Optimalizátor musí tooevaluate spojení, GROUP BY, HAVING a klauzule WHERE v dotazu.  Například pokud hello Optimalizátor odhadne, datum hello filtrujete v dotazu vrátí 1 řádek, může vybrat, velmi jiné plánování než pokud ho odhady jejich datum, na které jste vybrali bude vracet 1 milionu řádků.  Při vytváření statistik je velmi důležité, je stejně důležité této statistiky *přesně* odráží aktuální stav hello hello tabulky.  S aktuální statistiky zajistí, že je kvalitní plán vybrány ve Optimalizátor hello.  Hello plány vytvořené hello Optimalizátor jsou pouze jako vhodné jako hello statistiky na vaše data.

Hello proces vytváření a aktualizaci statistiky je aktuálně ruční proces, ale je velmi jednoduchý toodo.  To je rozdíl oproti systému SQL Server, který automaticky vytvoří a aktualizuje statistické údaje o jednoho sloupce a indexy.  Pomocí níže uvedené informace hello můžete výrazně automatizovat správu hello hello statistik na vaše data. 

## <a name="getting-started-with-statistics"></a>Začínáme s statistiky
 Vytváření jen Vzorkovaná statistiku pro každý sloupec je tooget snadno pracovat s statistiky.  Vzhledem k tomu, že je stejně důležité tookeep statistiky aktuální, konzervativní přístup může být tooupdate statistice denně nebo po každé zatížení. Jsou vždy kompromis mezi výkonem a hello náklady toocreate a aktualizaci statistik.  Pokud zjistíte, že trvá příliš dlouho toomaintain všechny statistické údaje, můžete, třeba tootry toobe užší sloupce, které mají statistiky nebo sloupce, které často aktualizovat.  Můžete například sloupců s kalendářními daty tooupdate denně, jak lze přidat nové hodnoty spíše než po každé zatížení. Znovu, bude získáte hello využívat výhod tak, že statistiky na sloupce použité ve spojení, GROUP BY, HAVING a klauzule WHERE.  Pokud máte tabulku s velkým množstvím sloupce, které se používají jenom v hello SELECT – klauzule, nemusí být úspěšná statistiky pro tyto sloupce a výdaje trochu další úsilí tooidentify jenom hello sloupce, které vám umožní statistiky, můžete snížit čas toomaintain hello statistice .

## <a name="multi-column-statistics"></a>Statistiky více sloupce
Kromě toho toocreating statistik na jednoho sloupce, můžete zjistit, vaše dotazy bude využívat více sloupci statistiky.  Statistiky více sloupce jsou statistiky vytvořena na seznam sloupců.  Obsahují jeden sloupec statistiku hello první sloupec v seznamu hello plus densities – názvem některé informace korelace mezi sloupci.  Například pokud máte tabulku, která připojí tooanother na dva sloupce, můžete zjistit, že SQL Data Warehouse můžete lépe optimalizovat hello plán v případě, že rozumí hello relaci mezi dvěma sloupci.   Statistiky více sloupce můžete zlepšit výkon dotazu pro některé operace, jako je například složené spojení a seskupit podle.

## <a name="updating-statistics"></a>Aktualizuje statistické údaje
Aktualizuje statistické údaje, je důležitou součástí rutiny správy vaší databáze.  Když se změní hello distribuci dat v databázi hello, třeba statistiky toobe aktualizovat.  Zastaralé statistiky povede dotazu toosub optimální výkon.

Jeden osvědčeným postupem je tooupdate statistiku sloupců s kalendářními daty jako nová data jsou přidány každý den.  Každé nové řádky času jsou načtená do datového skladu hello, nové zatížení kalendářní data nebo data transakcí se přidají. Tyto změnit hello distribuci dat a proveďte hello statistiky zastaralé. Naopak statistiky země sloupec v tabulce zákazníka může být nikdy nepotřebují toobe aktualizován, jako distribuční hello hodnot nemění obecně. Za předpokladu, že distribuce hello je konstantní mezi odběrateli, přidání nové řádky toohello tabulky variace není přejdete distribuci dat toochange hello. Ale pokud váš datový sklad obsahuje pouze jeden země a připojte ve data z nové země, výsledkem data z několika zemích, které ukládají, výborný musíte tooupdate statistiky ve sloupci země hello.

Jeden z hello první otázky tooask při řešení potíží s dotazem, "jsou hello statistiky aktuální?"

Tento dotaz není ten, který může odpovídat hello stáří dat hello. Objekt si statistiky toodate může být velmi staré, pokud byl žádné závažné změny toohello základní data. Když hello počet řádků, došlo ke změně podstatně nebo dojde ke změně podstatným hello rozdělení hodnot pro daný sloupec *pak* je čas tooupdate statistiky.  

Pro referenci **systému SQL Server** (ne SQL Data Warehouse) automaticky aktualizuje statistické údaje o těchto situacích:

* Pokud máte nulový počet řádků v tabulce hello při přidání řádků, získáte automatickou aktualizaci statistik
* Když přidáte více než 500 tabulky tooa řádky začínající menší než 500 řádků (například při spuštění 499 a pak přidejte 500 celkový počet řádků tooa 999 řádků), získáte automatických aktualizací 
* Jakmile jste více než 500 řádků budete mít tooadd 500 dodatečné řádky + 20 % velikosti hello hello tabulky předtím, než se zobrazí automatických aktualizací na hello statistiky

Vzhledem k tomu, že neexistuje žádná DMV toodetermine Pokud došlo ke změně dat v rámci hello tabulky byly aktualizace hello poslední čas statistiky, znalost hello stáří statistice můžete nabízejí části hello obrázku.  Můžete použít následující dotaz toodetermine hello čas poslední statistice hello tam, kde na každou tabulku aktualizovat.  

> [!NOTE]
> Mějte na paměti, že pokud dojde ke změně podstatným hello rozdělení hodnot pro daný sloupec, je třeba aktualizovat statistiku bez ohledu na to hello čas posledního jejich aktualizací.  
> 
> 

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Sloupců s kalendářními daty v datovém skladu, například třeba často aktualizace statistiky. Každé nové řádky času jsou načtená do datového skladu hello, nové zatížení kalendářní data nebo data transakcí se přidají. Tyto změnit hello distribuci dat a proveďte hello statistiky zastaralé.  Statistiky o pohlaví sloupce pro tabulku zákazníků a naopak, může být nutné nikdy toobe aktualizovat. Za předpokladu, že distribuce hello je konstantní mezi odběrateli, přidání nové řádky toohello tabulky variace není přejdete distribuci dat toochange hello. Ale pokud váš datový sklad obsahuje pouze jeden pohlaví a nový požadavek výsledkem více pohlaví výborný musíte tooupdate statistiku hello pohlaví sloupce.

Další informace naleznete v části [statistiky] [ Statistics] na webu MSDN.

## <a name="implementing-statistics-management"></a>Implementace správy statistiky
Často je vhodné tooextend hello data načítání tooensure proces, který se statistika aktualizuje na konci hello zatížení. načtení dat Hello je při tabulky nejčastěji změnit jejich velikost a jejich distribuci hodnoty. To je proto logické místní tooimplement některé procesy správy.

Některé zásady jsou uvedené pro aktualizaci statistice během procesu načítání hello:

* Zajistěte, aby každá tabulka načíst minimálně jeden objekt statistiky aktualizovat. Tato aktualizace hello tabulky informace o velikosti (počet řádků a počet stránek) jako součást aktualizace statistiky hello.
* Zaměřit se na sloupců podílejících se na připojení, GROUP BY, ORDER BY a DISTINCT – klauzule
* Zvažte aktualizaci data častěji, jak tyto hodnoty nebudou zahrnuty do histogram statistiky hello "vzestupné klíč" sloupců, jako je například transakce.
* Zvažte aktualizaci statické distribuční sloupce méně často.
* Mějte na paměti, že každý objekt statistiky je aktualizovat v řadě. Implementací `UPDATE STATISTICS <TABLE_NAME>` nemusí být právě ideální - hlavně pro široké tabulky s mnoha objekty statistiky.

> [!NOTE]
> Další podrobnosti na [vzestupné klíč] naleznete v dokumentu White Paper toohello SQL Server 2014 mohutnost odhad modelu.
> 
> 

Další informace naleznete v části [odhadu kardinality] [ Cardinality Estimation] na webu MSDN.

## <a name="examples-create-statistics"></a>Příklady: Vytvoření statistiky
Tyto příklady ukazují, jak toouse různé možnosti pro vytvoření statistiky. Hello možnosti, které používáte pro každý sloupec závisí na vlastnosti hello vašich dat a použití hello sloupec v dotazech.

### <a name="a-create-single-column-statistics-with-default-options"></a>A. Vytvoření statistiky jednoho sloupce s výchozími možnostmi
statistiky toocreate na sloupci, jednoduše zadejte název pro objekt hello statistiky a hello název sloupce hello.

Všechny výchozí možnosti hello využívá tuto syntaxi. Ve výchozím nastavení ukázky SQL Data Warehouse 20 procent hello tabulky při vytváření statistik.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Například:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a>B. Vytvořit jednosloupcovou statistiku tak, že prověří každý řádek
pro většinu situacích stačí Hello výchozí vzorkovací frekvenci 20 procent. Můžete však upravit vzorkovací frekvenci hello.

toosample hello úplné tabulky, použijte tuto syntaxi:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Například:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-hello-sample-size"></a>C. Vytvořte jednosloupcovou statistiku zadáním velikost vzorku hello
Alternativně můžete určit velikost vzorku hello v procentech:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-hello-rows"></a>D. Vytvořit jednosloupcovou statistiku pro jenom některé řádky hello
Další možností statistiky můžete vytvořit na část hello řádky v tabulce. Tomu se říká filtrované statistiky.

Můžete například použít filtrovanou statistiku při plánování tooquery na konkrétní oddíl velkých oddílů tabulky. Vytvořením statistiky na pouze hello oddílu hodnoty, bude hello přesnost hello statistiky zlepšit a proto zlepšit výkon dotazu.

Tento příklad vytvoří statistiky na rozsah hodnot. hodnoty Hello by snadno mohlo být definovaný v oddílu toomatch hello rozsah hodnot.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Pro hello dotazu pro optimalizaci tooconsider pomocí filtrovanou statistiku, když se vybere hello distribuovaného dotazu plán musí dotaz hello vejít do hello definici objektu statistiky hello. Použijeme předchozí příklad hello hello dotazu kde klauzule musí toospecify Sloupec1 hodnoty mezi 2000101 a 20001231.
> 
> 

### <a name="e-create-single-column-statistics-with-all-hello-options"></a>E. Vytvořit jednosloupcovou statistiku se všemi možnostmi hello
Samozřejmě můžete, možnosti hello kombinovat společně. Následující příklad Hello vytvoří objekt filtrovanou statistiku s velikost vlastní vzorku:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Hello úplný přehled najdete v tématu [CREATE STATISTICS] [ CREATE STATISTICS] na webu MSDN.

### <a name="f-create-multi-column-statistics"></a>F. Vytvoření statistiky více sloupci
toocreate vícesloupcového statistik, jednoduše použijte hello předchozích příkladech, ale zadat více sloupců.

> [!NOTE]
> Hello histogram, který se používá tooestimate počet řádků ve výsledku dotazu hello, je dostupná jenom pro první sloupec hello uvedené v definici objektu statistiky hello.
> 
> 

V tomto příkladu je hello histogram na *produktu\_kategorie*. Statistiky mezi sloupce jsou vypočítány na *produktu\_kategorie* a *produktu\_sub_c\ategory*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Vzhledem k tomu, že existuje korelace mezi *produktu\_kategorie* a *produktu\_sub\_kategorie*, může být užitečné, pokud tyto sloupce, ke kterým se přistupuje vícesloupcového stat v hello stejnou dobu.

### <a name="g-create-statistics-on-all-hello-columns-in-a-table"></a>G. Vytvoření statistiky pro všechny hello sloupců v tabulce.
Jedním ze způsobů toocreate statistiky je po vytvoření tabulky hello tooissues příkazy CREATE STATISTICS.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-toocreate-statistics-on-all-columns-in-a-database"></a>H. Uložené procedury toocreate statistik použití na všechny sloupce v databázi
SQL Data Warehouse nemá ekvivalentu uložené procedury systém příliš [] [sp_create_stats] v systému SQL Server. Tato uložená procedura vytvoří objekt statistiky jeden sloupec pro každý sloupec hello databáze, který ještě nemá statistiky.

To vám pomůže začít pracovat s návrhu databáze. Myslíte, že volné tooadapt ho tooyour potřebuje.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

toocreate statistiky pro všechny sloupce v tabulce hello s Tento postup jednoduše volání procedury hello.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Příklady: aktualizovat statistiky
tooupdate statistiky, můžete postupovat následovně:

1. Aktualizujte jeden objekt statistiky. Zadejte název hello hello statistiku objektu, že chcete tooupdate.
2. Aktualizujte všechny statistiky objekty v tabulce. Zadejte název hello hello tabulky místo jeden objekt konkrétní statistiku.

### <a name="a-update-one-specific-statistics-object"></a>A. Aktualizovat jeden objekt konkrétní Statistika
Použijte následující syntaxi tooupdate objekt konkrétní statistiku hello:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Například:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Při aktualizaci statistiky konkrétní objekty, můžete minimalizovat hello čas a prostředky požadované toomanage statistiky. To vyžaduje, že některé chápat, ale toochoose hello nejlepší statistiky objekty tooupdate.

### <a name="b-update-all-statistics-on-a-table"></a>B. Aktualizovat všechny statistiky v tabulce
Ukazuje to jednoduše aktualizuje všechny objekty statistiky hello v tabulce.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Například:

```sql
UPDATE STATISTICS dbo.table1;
```

Tento příkaz je snadno toouse. Jenom nezapomeňte to aktualizuje všechny statistické údaje o hello tabulky a proto může provést další práci, než je nezbytné. Pokud výkon hello není problém, je to výborný hello nejúplnější a nejjednodušší způsob, jakým tooguarantee statistiky jsou aktuální.

> [!NOTE]
> Při aktualizaci všechny statistiky v tabulce se SQL Data Warehouse nepodporuje kontrolu toosample hello tabulku pro každou statistiku. Pokud je tabulka hello velký, má mnoho sloupců a mnoho statistiky, může to být efektivnější jednotlivých statistiky tooupdate podle potřeb.
> 
> 

Implementace `UPDATE STATISTICS` postupu najdete v tématu hello [dočasných tabulek] [ Temporary] článku. Metoda implementace Hello je mírně odlišný toohello `CREATE STATISTICS` výše uvedeného postupu ale hello konečný výsledek je hello stejné.

Úplná syntaxe hello, najdete v části [Update Statistics] [ Update Statistics] na webu MSDN.

## <a name="statistics-metadata"></a>Statistiky metadat
Existuje několik systémové zobrazení a funkce, které můžete použít toofind informace o statistiky. Například se zobrazí, pokud objekt statistiky může být zastaralé pomocí hello statistiky datum funkce toosee při statistiky byly naposledy vytvoření nebo aktualizaci.

### <a name="catalog-views-for-statistics"></a>Zobrazení katalogu pro statistiky
Tato systémová zobrazení obsahují informace o statistiky:

| Zobrazení katalogu | Popis |
|:--- |:--- |
| [Sys.Columns][sys.columns] |Jeden řádek pro každý sloupec. |
| [Sys.Objects][sys.objects] |Jeden řádek pro každý objekt v databázi hello. |
| [Sys.Schemas][sys.schemas] |Jeden řádek pro každý schématu v databázi hello. |
| [Sys.stats][sys.stats] |Jeden řádek pro každý objekt statistiky. |
| [Sys.stats_columns][sys.stats_columns] |Jeden řádek pro každý sloupec v objektu statistiky hello. Odkazy Zpět toosys.columns. |
| [zobrazení Sys.Tables][sys.tables] |Jeden řádek pro každou tabulku (zahrnuje externí tabulky). |
| [Sys.table_types][sys.table_types] |Jeden řádek pro každý typ dat. |

### <a name="system-functions-for-statistics"></a>Funkce systému pro statistiky
Tyto funkce systému jsou užitečné pro práci s statistiky:

| System – funkce | Popis |
|:--- |:--- |
| [STATS_DATE][STATS_DATE] |Objekt statistiky hello datum poslední aktualizace. |
| [PŘÍKAZ DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] |Poskytuje souhrn úrovně a podrobné informace o distribuci hello hodnot rozumí hello statistiku objektu. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Zkombinovat do jednoho zobrazení statistiky sloupce a funkce
Toto zobrazení zobrazí sloupce, které se týkají toostatistics a výsledky z hello [STATS_DATE()] [] funkce společně.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>Příkaz DBCC SHOW_STATISTICS() příklady
Příkaz DBCC SHOW_STATISTICS() ukazuje hello data ukládaná v rámci objektu statistiky. Tato data je rozdělena na tři části.

1. Záhlaví
2. Hustotu vektoru
3. Histogram

Hello záhlaví metadata o statistikách hello. Hello histogram zobrazí hello distribuci hodnot v první klíčový sloupec hello hello statistiku objektu. vektor hustotu Hello měří korelace mezi sloupci. SQLDW vypočítá mohutnost odhady s žádným z hello data hello statistiku objektu.

### <a name="show-header-density-and-histogram"></a>Zobrazit záhlaví, hustotu a histogram
Tento jednoduchý příklad ukazuje všechny tři částí objektu statistiky.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Například:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Zobrazit jednu nebo více částí DBCC SHOW_STATISTICS();
Pokud vás zajímá pouze v zobrazení konkrétní části, použijte hello `WITH` klauzule a určete, které jste části má toosee:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Například:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>Příkaz DBCC SHOW_STATISTICS() rozdíly
Příkaz DBCC SHOW_STATISTICS() je implementováno více výhradně v SQL Data Warehouse porovnání tooSQL serveru.

1. Nezdokumentovaný funkce nejsou podporovány.
2. Nelze použít Stats_stream
3. Se nemůže připojit k výsledky pro konkrétní podmnožiny dat statistiky např (STAT_HEADER spojení DENSITY_VECTOR)
4. NO_INFOMSGS nelze nastavit pro potlačení zprávy
5. Hranaté závorky kolem názvů statistiky nelze použít.
6. Nelze použít sloupec názvy tooidentify statistiky objekty
7. Vlastní chyba 2767 není podporovaná.

## <a name="next-steps"></a>Další kroky
Další podrobnosti najdete v tématu [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] na webu MSDN.  články hello toolearn více, najdete na [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuci tabulku] [ Distribute], [Indexování tabulku][Index], [vytváření oddílů tabulky] [ Partition] a [ Dočasné tabulky][Temporary].  Další informace o osvědčených postupech najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->  
[Cardinality Estimation]: https://msdn.microsoft.com/library/dn600374.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistics]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
