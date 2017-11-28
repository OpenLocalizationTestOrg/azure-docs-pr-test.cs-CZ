---
title: aaaIndexing tabulek v SQL Data Warehouse | Microsoft Azure
description: "Začínáme s tabulkou indexování v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 3e617674-7b62-43ab-9ca2-3f40c41d5a88
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/12/2016
ms.author: shigu;barbkess
ms.openlocfilehash: e614d63c8fb871f2ba388f14576cf9f282d4b818
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-tables-in-sql-data-warehouse"></a>Indexování tabulek v SQL Data Warehouse
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

SQL Data Warehouse nabízí několik možností indexování včetně [Clusterované indexy columnstore][clustered columnstore indexes], [Clusterované indexy a neclusterované indexy] [ clustered indexes and nonclustered indexes].  Kromě toho nabízí také žádná možnost indexu také označované jako [haldy][heap].  Tento článek popisuje výhody hello každého typu index a také tipy toogetting hello většina výkonu mimo vaší indexy. V tématu [vytvoření tabulky syntax] [ create table syntax] další podrobnosti o tom, toocreate tabulky v SQL Data Warehouse.

## <a name="clustered-columnstore-indexes"></a>Clusterované indexy columnstore
Ve výchozím nastavení vytvoří SQL Data Warehouse clusterovaný index columnstore při nejsou zadány žádné možnosti indexu v tabulce. Clusterované tabulky columnstore nabízí i hello nejvyšší úroveň komprese dat a také hello nejlepší celkový výkon dotazů.  Clusterované tabulky columnstore se obecně překonat clusterovaný index nebo haldy tabulek a jsou obvykle hello nejlepší volbou pro rozsáhlé tabulky.  Z těchto důvodů Clusterové columnstore je nejlepší místo toostart hello, pokud si nejste jistí, jak tooindex tabulku.  

toocreate Clusterové columnstore tabulku, jednoduše zadejte CLUSTEROVANÝ INDEX COLUMNSTORE v klauzuli WITH hello, nebo ponechte v klauzuli WITH hello:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Existuje několik situací, kde Clusterové columnstore nemusí být vhodný:

* Tabulky Columnstore nepodporují, varchar(max), nvarchar(max) a varbinary(max).  Místo toho zvažte haldy nebo clusterovaný index.
* Tabulky Columnstore může být méně efektivní pro přechodný data.  Zvažte haldy a možná i dočasných tabulek.
* Malé tabulky s méně než 100 milionu řádků.  Vezměte v úvahu haldy tabulky.

## <a name="heap-tables"></a>Tabulky haldy
Pokud data jsou dočasně cílová v SQL Data Warehouse, můžete zjistit, které pomocí tabulky haldy provede hello celý proces rychlejší.  Je to proto tooheaps zatížení je rychlejší než tooindex tabulky a v některých případech hello lze provést následné číst z mezipaměti.  Při zavádění toostage pouze data před spuštěním další transformace, načítání tooheap tabulku hello je mnohem rychlejší než načítání dat tooa hello Clusterované tabulky columnstore. Kromě toho načítání dat tooa [dočasné tabulky] [ Temporary] také načte mnohem rychleji než načítání úložiště toopermanent tabulky.  

Pro malé vyhledávací tabulky, méně než 100 miliónů řádky, často haldy tabulky smysl.  Tabulky columnstore clusteru začít tooachieve optimální komprese, jakmile bude existovat více než 100 miliónů řádků.

toocreate haldy tabulku, jednoduše zadejte HALDY v klauzuli WITH hello:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Clusterovaných a neclusterovaných indexů
Clusterované indexy může překonat Clusterované tabulky columnstore, když jeden řádek potřebuje toobe rychle načíst.  Pro dotazy, které je jeden nebo jen několik řádek vyhledávání požadované tooperformance s extrémně rychlost zvažte index clusteru nebo sekundární neclusterovaný index.  Hello nevýhodou toousing clusterovaný index je, že bude mít prospěch pouze na dotazy, které používají vysoce selektivní filtr pro sloupec hello clusterovaný index.  na další sloupce, které může být neclusterovaný index filtr tooimprove přidat tooother sloupce.  Každý index, který se přidal tooa tabulky však přidá místa a zpracování tooloads čas.

toocreate tabulku clusterovaný index, jednoduše zadejte CLUSTEROVANÝ INDEX v klauzuli WITH hello:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

tooadd neclusterovaný index pro tabulku, jednoduše hello použijte následující syntaxi:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

## <a name="optimizing-clustered-columnstore-indexes"></a>Optimalizace Clusterované indexy columnstore
Clusterované tabulky columnstore jsou uspořádány v datech do segmentů.  Segment vysoké kvality je důležité tooachieving výkonu optimální dotazu na tabulky columnstore.  Segment kvality lze měřit hello počet řádků ve skupině komprimované řádek.  Segment quality je optimální, kde je nejméně 100K řádků na jeden řádek komprimované skupiny a získání výkonu jako hello počet řádků na jeden řádek skupiny přístup 1 048 576 řádků, který je hello většina řádky, které může obsahovat skupiny řádků.

Hello pod zobrazení lze vytvořit a použít na váš systém toocompute hello průměrný počet řádků na jeden řádek, skupiny a identifikovat žádné indexy columnstore neoptimálním průběhem clusteru.  Hello posledního sloupce v tomto zobrazení se vygeneruje jako příkaz jazyka SQL, který lze použít toorebuild vaše indexy.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,    COUNT(DISTINCT rg.[partition_number])                    AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,    CEILING    ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Teď, když jste vytvořili hello zobrazení, spusťte tento dotaz tooidentify tabulky s skupin řádků s méně než 100 tisíc řádků.  Samozřejmě můžete chtít tooincrease hello prahovou hodnotu 100 tisíc Pokud hledáte další kvality optimální segmentu. 

```sql
SELECT    *
FROM    [dbo].[vColumnstoreDensity]
WHERE    COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Po spuštění dotazu hello můžete začít toolook v hello dat a analyzovat vaše výsledky. Tato tabulka vysvětluje, co toolook pro analýzou skupiny řádků.

| Sloupec | Jak toouse tato data |
| --- | --- |
| [table_partition_count] |Pokud hello tabulka je rozdělena na oddíly, můžou očekávat toosee vyšší otevřete řádek skupiny počty. Každý oddíl v hello distribuční může mít teoreticky skupinu otevřete řádek s ním spojená. To dvoufaktorového do analýzy. Odebráním hello dělení úplně to by zlepšilo komprese může optimalizovat malé tabulku, která má byla rozdělena na oddíly. |
| [row_count_total] |Celkový počet řádků pro tabulku hello. Například můžete tuto hodnotu toocalculate podíl řádků v hello komprimované stavu. |
| [row_count_per_distribution_MAX] |Pokud všechny řádky jsou rovnoměrně rozmístěny. Tato hodnota by být hello cílový počet řádků na jeden distribuční. Porovnejte tuto hodnotu s hello compressed_rowgroup_count. |
| [COMPRESSED_rowgroup_rows] |Celkový počet řádků ve formátu columnstore pro tabulku hello. |
| [COMPRESSED_rowgroup_rows_AVG] |Pokud hello průměrný počet řádků je výrazně menší než hello maximální počet řádků pro skupinu řádek, pak zvažte použití funkce CTAS nebo ALTER INDEX REBUILD toorecompress hello dat |
| [COMPRESSED_rowgroup_count] |Počet skupin řádků ve formátu columnstore. Pokud toto číslo je velmi vysoká v tabulce toohello vztahů je indikátor nedostatku hustotu columnstore hello. |
| [COMPRESSED_rowgroup_rows_DELETED] |Ve formátu columnstore jsou logicky odstraněných řádků. Pokud je číslo hello vysoké relativní velikost tootable, zvažte opětovného vytvoření oddílu hello nebo znovu sestavit hello index, protože se odebere je fyzicky. |
| [COMPRESSED_rowgroup_rows_MIN] |Používejte ve spojení s hello Průměrný a maximální počet sloupců toounderstand hello rozsahu hodnot pro skupiny řádků hello ve vaší columnstore. Nízké číslo nadměrného zatížení hello (102,400 za oddílu zarovnán distribuční) naznačuje, že nejsou k dispozici v načtení dat hello optimalizace |
| [COMPRESSED_rowgroup_rows_MAX] |Stejně jako výše |
| [OPEN_rowgroup_count] |Skupiny otevřete řádků je normální. To bude přiměřeně jeden by uživatel očekával jednu skupinu OTEVŘETE řádek na distribuce tabulky (60). Nadměrné čísla navrhnout data načítání napříč oddíly. Překontrolujte hello dělení strategie toomake se, že je zvuku |
| [OPEN_rowgroup_rows] |Každý řádek skupiny může mít 1 048 576 řádků v něm jako maximální. Použijte tuto hodnotu toosee jak úplné hello otevřete řádek skupiny jsou aktuálně |
| [OPEN_rowgroup_rows_MIN] |Otevření skupin znamenat, že data skapat načítá do tabulky hello nebo který hello předchozí zatížení uniknout přes zbývající řádky do této skupiny řádků. Použití hello MIN, MAX průměr sloupce toosee, kolik dat je byla ve skupiny OTEVŘETE řádků. Pro malé tabulky může být 100 % všechna data hello! V takovém případě ALTER INDEX REBUILD tooforce hello data toocolumnstore. |
| [OPEN_rowgroup_rows_MAX] |Stejně jako výše |
| [OPEN_rowgroup_rows_AVG] |Stejně jako výše |
| [CLOSED_rowgroup_rows] |Podívejte se na seskupení řádků hello uzavřený řádek jako kontrolu správností. |
| [CLOSED_rowgroup_count] |Hello počet skupin uzavřené řádků by měl být nízká, pokud žádné vidět vůbec. Uzavřené řádek skupiny může být převedená toocompressed rowg roups pomocí hello ALTER INDEX... Příkaz REORGANIZOVAT. Není to však obvykle vyžaduje. Uzavřené skupiny jsou skupiny řádek automaticky převedený toocolumnstore procesem "přesunu řazené kolekce členů" hello pozadí. |
| [CLOSED_rowgroup_rows_MIN] |Uzavřené řádek skupiny musí mít na velmi vysokou výplně míru. Pokud je rychlost hello výplně pro skupinu uzavřené řádek nízkou, další analýza hello columnstore je požadovaná. |
| [CLOSED_rowgroup_rows_MAX] |Stejně jako výše |
| [CLOSED_rowgroup_rows_AVG] |Stejně jako výše |
| [Rebuild_Index_SQL] |Index columnstore toorebuild SQL pro tabulku |

## <a name="causes-of-poor-columnstore-index-quality"></a>Příčiny nízký columnstore index kvality
Pokud jste našli tabulky s kvality nízký segmentu, budete chtít tooidentify hello hlavní příčinu.  Tady jsou některé další běžné příčiny nízký segment quaility:

1. Pokud byl vytvořený index nárokům na paměť
2. Velkému počtu operace DML
3. Malá nebo skapat zatížení operací
4. Příliš mnoho oddíly

Tyto faktory mohou způsobit toohave index columnstore výrazně menší než hello optimální 1 milionu řádků na skupinu řádků.  Také způsobit skupiny řádků řádky toogo toohello rozdílů místo skupiny komprimované řádek. 

### <a name="memory-pressure-when-index-was-built"></a>Pokud byl vytvořený index nárokům na paměť
Hello počet řádků na skupinu komprimované řádek jsou přímo související toohello šířka řádku hello a hello množství paměti, že k dispozici tooprocess hello skupiny řádků.  Když řádky jsou zapsány toocolumnstore tabulky přetížena paměť, může dojít ke snížení kvality columnstore segmentu.  Proto hello osvědčeným postupem je toogive hello relace, který je zápis tabulky indexu columnstore tooyour přístup tooas mnoho paměti míře.  Vzhledem k tomu, že je kompromis mezi paměti a souběžnost, hello pokyny k hello správné jste paměti, že přidělení závisí na hello dat v každém řádku tabulky, hello množství DWU přidělené tooyour systému a hello množství souběžnosti přihrádek vám může poskytnout toohello relace, který zapisuje data tooyour tabulky.  Jako osvědčený postup doporučujeme začít s xlargerc, pokud používáte DW300 nebo méně largerc, pokud používáte DW400 tooDW600 a mediumrc Pokud používáte DW1000 a vyšší.

### <a name="high-volume-of-dml-operations"></a>Velkému počtu operace DML
K velkému počtu operace DML, které aktualizace a odstraňování řádků můžou představovat neefektivnost na hello columnstore. To platí hlavně při změně hello většina hello řádků v skupiny řádků.

* Odstranění řádku ze skupiny komprimované řádek pouze logicky označí hello řádku, jako je odstranit. řádek Hello zůstane v hello komprimované řádek skupiny, dokud znovu sestavit hello oddílu nebo tabulky.
* Vložíte řádek se přidá hello řádek tootooan interní rowstore tabulka s názvem skupiny řádků rozdílů. Hello vložit řádek není převedený toocolumnstore, dokud se skupiny řádků rozdílů hello je plný a je označené jako zavřené. Skupiny řádků jsou uzavřeny po uplynutí hello maximální kapacita 1 048 576 řádků. 
* Aktualizace řádků ve formátu columnstore zpracovávány jako logické odstranit a pak vložení. Hello vložit řádek můžou být uložené v úložišti rozdílů hello.

Zpracovat v dávce aktualizace a vkládání operace, které překročí prahovou hodnotu hromadné hello činí 102 400 řádků na jeden oddíl zarovnaný distribuční budou zapisovat přímo toohello columnstore formátu. Za předpokladu, že i distribuce, by však toobe úprava více než 6.144 milionu řádků v rámci jedné operace pro tento toooccur. Pokud hello počet řádků v daném oddílu zarovnán distribuce je menší než 102,400 hello řádky přejde toohello rozdílů úložiště a zůstanou existuje, dokud byly vloženy dostatečná řádků nebo byla znovu sestavena index upravené tooclose hello řádku skupiny nebo hello.

### <a name="small-or-trickle-load-operations"></a>Malá nebo skapat zatížení operací
Malá načte, že tok do SQL Data Warehouse se také někdy označuje jako skapat zatížení. Obvykle představují near nepřetržitý datový proud dat probíhá požita hello systému. Však jako tento datový proud je téměř průběžné hello svazku řádků není zvlášť velký. Častěji hello data je výrazně pod prahovou hodnotou hello požadované pro přímé zatížení toocolumnstore formátu.

V těchto situacích je často lepší tooland hello dat nejprve v Azure blob storage a nechat ji hromadit předchozí tooloading. Tento postup se často označuje jako *dávkování micro*.

### <a name="too-many-partitions"></a>Příliš mnoho oddíly
Jiné tooconsider věc je hello dopad oddíly na vaše Clusterované tabulky columnstore.  Před oddílů, SQL Data Warehouse již rozděluje data do 60 databáze.  Vytváření oddílů vydělí další data.  Pokud oddílu dat, pak budete chtít tooconsider, **každý** oddíl bude potřebovat toobenefit toohave alespoň 1 milionu řádků z clusterovaný index columnstore.  Pokud oddílu tabulku do 100 oddílů, pak tabulka bude nutné toobenefit toohave alespoň 6 miliardy řádků z s clusterovaným indexem columnstore (60 distribuce * 100 oddíly * 1 milionu řádků). Pokud 100 oddílu tabulky nemá 6 miliardy řádků, snižte počet hello oddílů nebo zvažte použití haldy tabulku místo.

Jakmile vaše tabulky byly načteny určitými daty, postupujte podle hello níže tooidentify kroků a znovu vytvořit tabulky s indexy columnstore neoptimálním průběhem clusteru.

## <a name="rebuilding-indexes-tooimprove-segment-quality"></a>Opětovné sestavení indexů tooimprove segment kvality
### <a name="step-1-identify-or-create-user-which-uses-hello-right-resource-class"></a>Krok 1: Určení nebo vytvoření uživatele, který používá třída správné prostředků hello
Jeden rychlý způsob tooimmediately zlepšení kvality segmentu je toorebuild hello index.  Hello SQL vrácený hello výše zobrazení vrátí příkaz ALTER INDEX REBUILD, který lze použít toorebuild vaše indexy.  Při opětovné sestavení indexů, se ujistěte, že přidělíte dostatek paměti toohello relace, který bude opětovné sestavení indexu.  toodo se třída zvýšení hello prostředků uživatele, který má oprávnění toorebuild hello indexu v této tabulce toohello doporučené minimální.  Třída prostředků Hello hello uživatele vlastníka databáze nelze změnit, pokud jste dosud nevytvořili uživatele systému hello, budete potřebovat toodo proto nejprve.  minimální Hello, doporučujeme je xlargerc Pokud používáte DW300 nebo méně largerc, pokud používáte DW400 tooDW600 a mediumrc Pokud používáte DW1000 a vyšší.

Dole je příklad toho, jak tooallocate další uživatele tooa paměti zvýšením jejich třída prostředků.  Další informace o prostředku třídy a jak toocreate nového uživatele naleznete v hello [souběžnosti a úlohy správy] [ Concurrency] článku.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Krok 2: Znovu vytvořit Clusterované indexy columnstore s vyšší uživatelem prostředků – třída
Přihlášení se jako hello uživatele z kroku 1 (např. LoadUser), který je teď pomocí vyšší Třída prostředků a spusťte příkazy ALTER INDEX hello.  Ujistěte se, že tento uživatel má ALTER oprávnění toohello tabulky, kde se hello index znovu sestaven.  Tyto příklady ukazují, jak toorebuild hello index columnstore celý nebo jak toorebuild jeden oddíl. V rozsáhlé tabulky je praktičtější toorebuild indexy jednoho oddílu současně.

Alternativně místo znovu sestavit hello index, můžete zkopírovat hello tabulky tooa novou tabulku pomocí [funkce CTAS][CTAS].  Jakým způsobem je nejlepší? Pro velké objemy dat [funkce CTAS] [ CTAS] je obvykle rychlejší než [ALTER INDEX][ALTER INDEX]. Pro menší objem dat [ALTER INDEX] [ ALTER INDEX] je snazší toouse a nebude vyžadovat tooswap out hello tabulky.  V tématu **nové sestavení indexů se funkce CTAS a přepnutí oddílu** níže další podrobnosti o tom, jak toorebuild indexovat pomocí funkce CTAS.

```sql
-- Rebuild hello entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Nové sestavení indexu v SQL Data Warehouse je offline operace.  Další informace o nové sestavení indexů, najdete v části hello příkaz ALTER INDEX opětovné sestavení kapitoly [defragmentace indexy Columnstore] [ Columnstore Indexes Defragmentation] a téma syntaxe hello [ALTER INDEX] [ALTER INDEX].

### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Krok 3: Ověření, že je vylepšený Clusterové columnstore segment kvality
Spusťte znovu hello dotaz, který identifikuje tabulky s nízká segmentovat kvality a ověřte, zda je vylepšený kvality segmentu.  Pokud ke zlepšení kvality segmentu, je možné, že hello řádky v tabulce jsou velmi široký.  Zvažte použití vyšší Třída prostředků nebo DWU při opětovném sestavování vaší indexy.

## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Nové sestavení indexů se funkce CTAS a přepnutí oddílu
Tento příklad používá [funkce CTAS] [ CTAS] a přepnutí toorebuild oddíl tabulky oddílu. 

```sql
-- Step 1: Select hello partition of data and write it out tooa new table using CTAS
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
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
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
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT hello data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 too [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN hello rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 too [dbo].[FactInternetSales] PARTITION 2;
```

Další podrobnosti o opětovné vytvoření oddíly používající `CTAS`, najdete v části hello [oddílu] [ Partition] článku.

## <a name="next-steps"></a>Další kroky
články hello toolearn více, najdete na [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuci tabulku] [ Distribute], [Vytváření oddílů tabulky][Partition], [zachování statistiky tabulky] [ Statistics] a [ Dočasné tabulky][Temporary].  toolearn Další informace o osvědčených postupů, najdete v části [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx
[heap]: https://msdn.microsoft.com/library/hh213609.aspx
[clustered indexes and nonclustered indexes]: https://msdn.microsoft.com/library/ms190457.aspx
[create table syntax]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore Indexes Defragmentation]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[clustered columnstore indexes]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
