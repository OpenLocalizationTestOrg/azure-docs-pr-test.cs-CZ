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
# <a name="partitioning-tables-in-sql-data-warehouse"></a><span data-ttu-id="4b319-103">Vytváření oddílů tabulky v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4b319-103">Partitioning tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="4b319-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="4b319-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="4b319-105">[Datové typy][Data Types]</span><span class="sxs-lookup"><span data-stu-id="4b319-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="4b319-106">[Distribuce][Distribute]</span><span class="sxs-lookup"><span data-stu-id="4b319-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="4b319-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="4b319-107">[Index][Index]</span></span>
> * <span data-ttu-id="4b319-108">[Oddíl][Partition]</span><span class="sxs-lookup"><span data-stu-id="4b319-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="4b319-109">[Statistiky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="4b319-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="4b319-110">[Dočasné][Temporary]</span><span class="sxs-lookup"><span data-stu-id="4b319-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="4b319-111">Vytváření oddílů je podporován u všech typů tabulek SQL Data Warehouse; včetně clusterových columnstore, clusterovaný index a haldy.</span><span class="sxs-lookup"><span data-stu-id="4b319-111">Partitioning is supported on all SQL Data Warehouse table types; including clustered columnstore, clustered index, and heap.</span></span>  <span data-ttu-id="4b319-112">Vytváření oddílů je podporováno také na všechny typy distribučních, včetně hodnoty hash nebo kruhové dotazování distribuován.</span><span class="sxs-lookup"><span data-stu-id="4b319-112">Partitioning is also supported on all distribution types, including both hash or round robin distributed.</span></span>  <span data-ttu-id="4b319-113">Vytváření oddílů umožňuje toodivide, které vaše data do menší skupiny dat a ve většině případů dělení se provádí na sloupci, datum.</span><span class="sxs-lookup"><span data-stu-id="4b319-113">Partitioning enables you toodivide your data into smaller groups of data and in most cases, partitioning is done on a date column.</span></span>

## <a name="benefits-of-partitioning"></a><span data-ttu-id="4b319-114">Výhody dělení</span><span class="sxs-lookup"><span data-stu-id="4b319-114">Benefits of partitioning</span></span>
<span data-ttu-id="4b319-115">Vytváření oddílů využívat data výkonu údržby a dotazů.</span><span class="sxs-lookup"><span data-stu-id="4b319-115">Partitioning can benefit data maintenance and query performance.</span></span>  <span data-ttu-id="4b319-116">Jestli výhody oba, nebo pouze jeden, je závislá na tom, jak načíst data a jestli hello stejný sloupec lze použít pro obě účely, protože vytváření oddílů je možné provést pouze na jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="4b319-116">Whether it benefits both or just one is dependent on how data is loaded and whether hello same column can be used for both purposes, since partitioning can only be done on one column.</span></span>

### <a name="benefits-tooloads"></a><span data-ttu-id="4b319-117">Výhody tooloads</span><span class="sxs-lookup"><span data-stu-id="4b319-117">Benefits tooloads</span></span>
<span data-ttu-id="4b319-118">Hello Hlavní výhoda vytváření oddílů v SQL Data Warehouse je zvýšit efektivitu hello a výkon načítání dat pomocí odstranění oddílu, přepínání a slučování.</span><span class="sxs-lookup"><span data-stu-id="4b319-118">hello primary benefit of partitioning in SQL Data Warehouse is improve hello efficiency and performance of loading data by use of partition deletion, switching and merging.</span></span>  <span data-ttu-id="4b319-119">Ve většině případů, které jsou data rozdělena na datum vázaný sloupec, který je úzce toohello pořadí datových hello je načíst toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="4b319-119">In most cases data is partitioned on a date column that is closely tied toohello sequence which hello data is loaded toohello database.</span></span>  <span data-ttu-id="4b319-120">Jeden z hello největší výhody použití dat toomaintain oddíly ho hello předcházení protokolování transakcí.</span><span class="sxs-lookup"><span data-stu-id="4b319-120">One of hello greatest benefits of using partitions toomaintain data it hello avoidance of transaction logging.</span></span>  <span data-ttu-id="4b319-121">Když jednoduše vkládání, aktualizaci nebo odstranění dat může být hello nejjednodušší způsob, s malým množstvím myšlenku a úsilí, pomocí dělení během procesu vaše zatížení může podstatně zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="4b319-121">While simply inserting, updating or deleting data can be hello most straightforward approach, with a little thought and effort, using partitioning during your load process can substantially improve performance.</span></span>

<span data-ttu-id="4b319-122">Přepnutí oddílu můžete být použité tooquickly odeberte nebo nahraďte oddíl tabulky.</span><span class="sxs-lookup"><span data-stu-id="4b319-122">Partition switching can be used tooquickly remove or replace a section of a table.</span></span>  <span data-ttu-id="4b319-123">Tabulka faktů prodeje například může obsahovat pouze data pro hello posledních 36 měsíců.</span><span class="sxs-lookup"><span data-stu-id="4b319-123">For example, a sales fact table might contain just data for hello past 36 months.</span></span>  <span data-ttu-id="4b319-124">Na konci hello v každém měsíci hello nejstarší měsíc prodejních dat je odstraněn z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="4b319-124">At hello end of every month, hello oldest month of sales data is deleted from hello table.</span></span>  <span data-ttu-id="4b319-125">Tato data může odstranit pomocí odstranit příkaz toodelete hello data pro hello nejstarší měsíc.</span><span class="sxs-lookup"><span data-stu-id="4b319-125">This data could be deleted by using a delete statement toodelete hello data for hello oldest month.</span></span>  <span data-ttu-id="4b319-126">Ale odstraňování velké množství dat řádek po řádku příkazem delete může trvat velmi dlouho, stejně jako hello riziko velké transakcí, které může trvat dlouhou dobu toorollback, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="4b319-126">However, deleting a large amount of data row-by-row with a delete statement can take a very long time, as well as create hello risk of large transactions which could take a long time toorollback if something goes wrong.</span></span>  <span data-ttu-id="4b319-127">Více optimální přístup je toosimply rozevírací hello nejstarší oddílu data.</span><span class="sxs-lookup"><span data-stu-id="4b319-127">A more optimal approach is toosimply drop hello oldest partition of data.</span></span>  <span data-ttu-id="4b319-128">Kde odstranění jednotlivých řádků hello může trvat hodiny, odstraňování celý oddíl může trvat sekund.</span><span class="sxs-lookup"><span data-stu-id="4b319-128">Where deleting hello individual rows could take hours, deleting an entire partition could take seconds.</span></span>

### <a name="benefits-tooqueries"></a><span data-ttu-id="4b319-129">Výhody tooqueries</span><span class="sxs-lookup"><span data-stu-id="4b319-129">Benefits tooqueries</span></span>
<span data-ttu-id="4b319-130">Dělení může být také použít tooimprove výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="4b319-130">Partitioning can also be used tooimprove query performance.</span></span>  <span data-ttu-id="4b319-131">Pokud dotaz použije filtr na sloupec rozdělení, to můžete omezit hello kontroly tooonly hello kvalifikující oddíly, které můžou být mnohem menší podmnožinu dat hello, zabraňující prohledání úplnou tabulky.</span><span class="sxs-lookup"><span data-stu-id="4b319-131">If a query applies a filter on a partitioned column, this can limit hello scan tooonly hello qualifying partitions which may be a much smaller subset of hello data, avoiding a full table scan.</span></span>  <span data-ttu-id="4b319-132">Při zavedení hello Clusterované indexy columnstore jsou méně výhodné hello predikátem odstranění výkonnostních výhod, ale v některých případech může být tooqueries výhody.</span><span class="sxs-lookup"><span data-stu-id="4b319-132">With hello introduction of clustered columnstore indexes, hello predicate elimination performance benefits are less beneficial, but in some cases there can be a benefit tooqueries.</span></span>  <span data-ttu-id="4b319-133">Například pokud hello prodeje fakt tabulka je rozdělena na oddíly do 36 měsíců pomocí hello datum prodeje pole, pak dotazy, které filtrovat hello prodej datum můžete přeskočit hledání v oddíly, které neodpovídají hello filtru.</span><span class="sxs-lookup"><span data-stu-id="4b319-133">For example, if hello sales fact table is partitioned into 36 months using hello sales date field, then queries that filter on hello sale date can skip searching in partitions that don’t match hello filter.</span></span>

## <a name="partition-sizing-guidance"></a><span data-ttu-id="4b319-134">Pokyny k dimenzování oddílu</span><span class="sxs-lookup"><span data-stu-id="4b319-134">Partition sizing guidance</span></span>
<span data-ttu-id="4b319-135">Při vytváření oddílů lze použít tooimprove výkonu některých scénářích, vytvoření tabulky s **příliš mnoho** oddíly může narušit výkonnost za určitých okolností.</span><span class="sxs-lookup"><span data-stu-id="4b319-135">While partitioning can be used tooimprove performance some scenarios, creating a table with **too many** partitions can hurt performance under some circumstances.</span></span>  <span data-ttu-id="4b319-136">Tyto problémy jsou především pro Clusterované tabulky columnstore.</span><span class="sxs-lookup"><span data-stu-id="4b319-136">These concerns are especially true for clustered columnstore tables.</span></span>  <span data-ttu-id="4b319-137">U oddílů toobe užitečné, je důležité toounderstand při toouse vytváření oddílů a hello počet toocreate oddíly.</span><span class="sxs-lookup"><span data-stu-id="4b319-137">For partitioning toobe helpful, it is important toounderstand when toouse partitioning and hello number of partitions toocreate.</span></span>  <span data-ttu-id="4b319-138">Existuje žádné pevné pravidlo rychlé jako toohow mnoha oddílů jsou příliš mnoho, závisí na vaše data a kolik oddíly jsou načítání toosimultaneously.</span><span class="sxs-lookup"><span data-stu-id="4b319-138">There is no hard fast rule as toohow many partitions are too many, it depends on your data and how many partitions you are loading toosimultaneously.</span></span>  <span data-ttu-id="4b319-139">Ale jako obecné pravidlo, vezměte v úvahu přidávání 10s too100s oddílů není 1000s.</span><span class="sxs-lookup"><span data-stu-id="4b319-139">But as a general rule of thumb, think of adding 10s too100s of partitions, not 1000s.</span></span>

<span data-ttu-id="4b319-140">Při vytváření dělení na **Clusterové columnstore** tabulky, je důležité tooconsider, kolik řádků se nebude zobrazovat v každém oddílu.</span><span class="sxs-lookup"><span data-stu-id="4b319-140">When creating partitioning on **clustered columnstore** tables, it is important tooconsider how many rows will land in each partition.</span></span>  <span data-ttu-id="4b319-141">Pro optimální komprese a výkon Clusterované tabulky columnstore je potřeba minimálně 1 milionu řádků na distribuce a oddíl.</span><span class="sxs-lookup"><span data-stu-id="4b319-141">For optimal compression and performance of clustered columnstore tables, a minimum of 1 million rows per distribution and partition is needed.</span></span>  <span data-ttu-id="4b319-142">Před vytvořením oddíly, SQL Data Warehouse každá tabulka již rozdělí na 60 distribuované databáze.</span><span class="sxs-lookup"><span data-stu-id="4b319-142">Before partitions are created, SQL Data Warehouse already divides each table into 60 distributed databases.</span></span>  <span data-ttu-id="4b319-143">Žádné rozdělení přidané tooa tabulce je navíc toohello distribuce vytvořit pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="4b319-143">Any partitioning added tooa table is in addition toohello distributions created behind hello scenes.</span></span>  <span data-ttu-id="4b319-144">V tomto příkladu, pokud tabulka faktů prodeje hello obsažené 36 měsíční oddíly a vzhledem k tomu, že SQL Data Warehouse je 60 distribuce hello prodeje fakt, že tabulky by měl obsahovat 60 milionu řádků měsíčně nebo 2.1 miliardy řádků, když jsou naplněny všechny měsíce.</span><span class="sxs-lookup"><span data-stu-id="4b319-144">Using this example, if hello sales fact table contained 36 monthly partitions, and given that SQL Data Warehouse has 60 distributions, then hello sales fact table should contain 60 million rows per month, or 2.1 billion rows when all months are populated.</span></span>  <span data-ttu-id="4b319-145">Pokud tabulka obsahuje výrazně méně řádků než hello Doporučená minimální počet řádků na jeden oddíl, zvažte použití méně oddíly v pořadí toomake zvýšení hello počet řádků na jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="4b319-145">If a table contains significantly less rows than hello recommended minimum number of rows per partition, consider using fewer partitions in order toomake increase hello number of rows per partition.</span></span>  <span data-ttu-id="4b319-146">Viz také hello [indexování] [ Index] článek, který obsahuje dotazy, které lze spustit v SQL Data Warehouse tooassess hello kvalitu indexy columnstore clusteru.</span><span class="sxs-lookup"><span data-stu-id="4b319-146">Also see hello [Indexing][Index] article which includes queries that can be run on SQL Data Warehouse tooassess hello quality of cluster columnstore indexes.</span></span>

## <a name="syntax-difference-from-sql-server"></a><span data-ttu-id="4b319-147">Syntaxe rozdíl oproti systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="4b319-147">Syntax difference from SQL Server</span></span>
<span data-ttu-id="4b319-148">SQL Data Warehouse zavádí zjednodušenou definice oddíly, který se mírně liší od systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4b319-148">SQL Data Warehouse introduces a simplified definition of partitions which is slightly different from SQL Server.</span></span>  <span data-ttu-id="4b319-149">Vytváření oddílů funkce a schémata nejsou použity v SQL Data Warehouse, jako jsou v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4b319-149">Partitioning functions and schemes are not used in SQL Data Warehouse as they are in SQL Server.</span></span>  <span data-ttu-id="4b319-150">Místo toho stačí toodo je identifikovat oddílů sloupce a hello hranic body.</span><span class="sxs-lookup"><span data-stu-id="4b319-150">Instead, all you need toodo is identify partitioned column and hello boundary points.</span></span>  <span data-ttu-id="4b319-151">Hello syntaxe vytváření oddílů se mírně liší v systému SQL Server, jsou hello stejné základní koncepty hello.</span><span class="sxs-lookup"><span data-stu-id="4b319-151">While hello syntax of partitioning may be slightly different from SQL Server, hello basic concepts are hello same.</span></span>  <span data-ttu-id="4b319-152">SQL Server a SQL Data Warehouse podporují jeden sloupec oddílu na jednu tabulku, která může být pohyboval oddílu.</span><span class="sxs-lookup"><span data-stu-id="4b319-152">SQL Server and SQL Data Warehouse support one partition column per table, which can be ranged partition.</span></span>  <span data-ttu-id="4b319-153">toolearn Další informace o vytváření oddílů, najdete v části [rozdělena na oddíly tabulky a indexy][Partitioned Tables and Indexes].</span><span class="sxs-lookup"><span data-stu-id="4b319-153">toolearn more about partitioning, see [Partitioned Tables and Indexes][Partitioned Tables and Indexes].</span></span>

<span data-ttu-id="4b319-154">Hello následujícím příkladu SQL Data Warehouse rozdělena na oddíly [CREATE TABLE] [ CREATE TABLE] příkaz oddíly tabulka FactInternetSales hello hello OrderDateKey sloupec:</span><span class="sxs-lookup"><span data-stu-id="4b319-154">hello below example of a SQL Data Warehouse partitioned [CREATE TABLE][CREATE TABLE] statement, partitions hello FactInternetSales table on hello OrderDateKey column:</span></span>

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

## <a name="migrating-partitioning-from-sql-server"></a><span data-ttu-id="4b319-155">Migrace, vytváření oddílů v systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="4b319-155">Migrating partitioning from SQL Server</span></span>
<span data-ttu-id="4b319-156">toomigrate systému SQL Server jednoduše oddílu tooSQL definice datového skladu:</span><span class="sxs-lookup"><span data-stu-id="4b319-156">toomigrate SQL Server partition definitions tooSQL Data Warehouse simply:</span></span>

* <span data-ttu-id="4b319-157">Odstranění hello systému SQL Server [schéma oddílu][partition scheme].</span><span class="sxs-lookup"><span data-stu-id="4b319-157">Eliminate hello SQL Server [partition scheme][partition scheme].</span></span>
* <span data-ttu-id="4b319-158">Přidat hello [oddílu funkce] [ partition function] definice tooyour vytvořit tabulku.</span><span class="sxs-lookup"><span data-stu-id="4b319-158">Add hello [partition function][partition function] definition tooyour CREATE TABLE.</span></span>

<span data-ttu-id="4b319-159">Pokud migrujete dělenou tabulku z hello instance serveru SQL pod SQL vám může pomoct toointerrogate hello počet řádků, které jsou v každém oddílu.</span><span class="sxs-lookup"><span data-stu-id="4b319-159">If you are migrating a partitioned table from a SQL Server instance hello below SQL can help you toointerrogate hello number of rows that are in each partition.</span></span>  <span data-ttu-id="4b319-160">Uvědomte si, že pokud hello stejnou členitost rozdělení se používá v SQL Data Warehouse, hello počet řádků na jeden oddíl se sníží o faktor 60.</span><span class="sxs-lookup"><span data-stu-id="4b319-160">Keep in mind that if hello same partitioning granularity is used on SQL Data Warehouse, hello number of rows per partition will decrease by a factor of 60.</span></span>  

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

## <a name="workload-management"></a><span data-ttu-id="4b319-161">Správa zatížení</span><span class="sxs-lookup"><span data-stu-id="4b319-161">Workload management</span></span>
<span data-ttu-id="4b319-162">Jeden aspekt toofactor poslední díl v toohello tabulce oddílu rozhodnutí je [úlohy správy][workload management].</span><span class="sxs-lookup"><span data-stu-id="4b319-162">One final piece consideration toofactor in toohello table partition decision is [workload management][workload management].</span></span>  <span data-ttu-id="4b319-163">Úlohy správy v SQL Data Warehouse je primárně hello Správa paměti a souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="4b319-163">Workload management in SQL Data Warehouse is primarily hello management of memory and concurrency.</span></span>  <span data-ttu-id="4b319-164">V SQL Data Warehouse hello je maximální velikost paměti přidělené tooeach distribuční během provádění dotazu třídy upraveny prostředků.</span><span class="sxs-lookup"><span data-stu-id="4b319-164">In SQL Data Warehouse hello maximum memory allocated tooeach distribution during query execution is governed resource classes.</span></span>  <span data-ttu-id="4b319-165">V ideálním případě budou vaše oddíly velké s ohledem na dalších faktorech, jako je vytváření Clusterované indexy columnstore hello paměti potřebám.</span><span class="sxs-lookup"><span data-stu-id="4b319-165">Ideally your partitions will be sized in consideration of other factors like hello memory needs of building clustered columnstore indexes.</span></span>  <span data-ttu-id="4b319-166">Clusterovaný benefit indexy columnstore výrazně při jejich přidělení více paměti.</span><span class="sxs-lookup"><span data-stu-id="4b319-166">Clustered columnstore indexes benefit greatly when they are allocated more memory.</span></span>  <span data-ttu-id="4b319-167">Proto je vhodné tooensure, který znovu sestavit index oddílu není nedostatek paměti.</span><span class="sxs-lookup"><span data-stu-id="4b319-167">Therefore, you will want tooensure that a partition index rebuild is not starved of memory.</span></span> <span data-ttu-id="4b319-168">Zvýšení hello množství paměti k dispozici tooyour dotazu lze dosáhnout přepínání z hello výchozí role, smallrc, tooone z jiných rolí, například largerc hello.</span><span class="sxs-lookup"><span data-stu-id="4b319-168">Increasing hello amount of memory available tooyour query can be achieved by switching from hello default role, smallrc, tooone of hello other roles such as largerc.</span></span>

<span data-ttu-id="4b319-169">Informace o hello přidělení paměti na jeden distribuční je k dispozici pomocí dotazu na zobrazení dynamické správy Správce prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="4b319-169">Information on hello allocation of memory per distribution is available by querying hello resource governor dynamic management views.</span></span> <span data-ttu-id="4b319-170">Ve skutečnosti vaší přidělení paměti bude menší než následující hello obrázky.</span><span class="sxs-lookup"><span data-stu-id="4b319-170">In reality your memory grant will be less than hello figures below.</span></span> <span data-ttu-id="4b319-171">To však poskytuje úroveň pokyny, které můžete použít, když vaše oddíly pro operace správy dat pro definování velikosti.</span><span class="sxs-lookup"><span data-stu-id="4b319-171">However, this provides a level of guidance that you can use when sizing your partitions for data management operations.</span></span>  <span data-ttu-id="4b319-172">Zkuste tooavoid Změna velikosti vašeho oddíly nad rámec poskytovaný Třída prostředků se velmi velké hello přidělení paměti hello.</span><span class="sxs-lookup"><span data-stu-id="4b319-172">Try tooavoid sizing your partitions beyond hello memory grant provided by hello extra large resource class.</span></span> <span data-ttu-id="4b319-173">Pokud vaše oddíly růst nad rámec tohoto obrázku spuštěním hello riziko přetížení paměti, což pak vede tooless optimální komprese.</span><span class="sxs-lookup"><span data-stu-id="4b319-173">If your partitions grow beyond this figure you run hello risk of memory pressure which in turn leads tooless optimal compression.</span></span>

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

## <a name="partition-switching"></a><span data-ttu-id="4b319-174">Přepnutí oddílu</span><span class="sxs-lookup"><span data-stu-id="4b319-174">Partition switching</span></span>
<span data-ttu-id="4b319-175">SQL Data Warehouse podporuje oddílu rozdělení, sloučení a přepínání.</span><span class="sxs-lookup"><span data-stu-id="4b319-175">SQL Data Warehouse supports partition splitting, merging, and switching.</span></span> <span data-ttu-id="4b319-176">Každá z těchto funkcí je excuted pomocí hello [příkaz ALTER TABLE] [ ALTER TABLE] příkaz.</span><span class="sxs-lookup"><span data-stu-id="4b319-176">Each of these functions is excuted using hello [ALTER TABLE][ALTER TABLE] statement.</span></span>

<span data-ttu-id="4b319-177">oddíly tooswitch mezi dvěma tabulkami musíte zajistit, že oddíly hello zarovnat na jejich odpovídající hranice a jestli se shodují hello Definice tabulky.</span><span class="sxs-lookup"><span data-stu-id="4b319-177">tooswitch partitions between two tables you must ensure that hello partitions align on their respective boundaries and that hello table definitions match.</span></span> <span data-ttu-id="4b319-178">Jako omezení check nejsou k dispozici tooenforce hello rozsahu hodnot v tabulce hello zdrojová tabulka musí obsahovat hello stejná jako cílová tabulka hello oddílu hranice.</span><span class="sxs-lookup"><span data-stu-id="4b319-178">As check constraints are not available tooenforce hello range of values in a table hello source table must contain hello same partition boundaries as hello target table.</span></span> <span data-ttu-id="4b319-179">Pokud to není hello případ, pak hello oddílu přepínače selžou, protože metadata oddílu hello se nebudou synchronizovat.</span><span class="sxs-lookup"><span data-stu-id="4b319-179">If this is not hello case, then hello partition switch will fail as hello partition metadata will not be synchronized.</span></span>

### <a name="how-toosplit-a-partition-that-contains-data"></a><span data-ttu-id="4b319-180">Jak toosplit oddílu, který obsahuje data</span><span class="sxs-lookup"><span data-stu-id="4b319-180">How toosplit a partition that contains data</span></span>
<span data-ttu-id="4b319-181">Hello nejúčinnější metoda toosplit oddíl, který už obsahuje data je toouse `CTAS` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4b319-181">hello most efficient method toosplit a partition that already contains data is toouse a `CTAS` statement.</span></span> <span data-ttu-id="4b319-182">Pokud je tabulka oddílů hello Clusterové columnstore pak hello tabulky oddíl musí být prázdný předtím, než je možné rozdělit.</span><span class="sxs-lookup"><span data-stu-id="4b319-182">If hello partitioned table is a clustered columnstore then hello table partition must be empty before it can be split.</span></span>

<span data-ttu-id="4b319-183">Níže je ukázka tabulku oddílů columnstore obsahující jeden řádek v každém oddílu:</span><span class="sxs-lookup"><span data-stu-id="4b319-183">Below is a sample partitioned columnstore table containing one row in each partition:</span></span>

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
> <span data-ttu-id="4b319-184">Objekt pro vytváření hello statistiky jsme Ujistěte se, že tento metadat tabulky je přesnější.</span><span class="sxs-lookup"><span data-stu-id="4b319-184">By Creating hello statistic object, we ensure that table metadata is more accurate.</span></span> <span data-ttu-id="4b319-185">Pokud jsme vynechat, vytváření statistik, SQL Data Warehouse použije výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4b319-185">If we omit creating statistics, then SQL Data Warehouse will use default values.</span></span> <span data-ttu-id="4b319-186">Pro Zkontrolujte prosím podrobnosti o statistiky [statistiky][statistics].</span><span class="sxs-lookup"><span data-stu-id="4b319-186">For details on statistics please review [statistics][statistics].</span></span>
> 
> 

<span data-ttu-id="4b319-187">Jsme dotazu pro počet řádků hello pomocí hello `sys.partitions` katalogu zobrazení:</span><span class="sxs-lookup"><span data-stu-id="4b319-187">We can then query for hello row count using hello `sys.partitions` catalog view:</span></span>

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

<span data-ttu-id="4b319-188">Pokud tato tabulka pokusíme toosplit jsme dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="4b319-188">If we try toosplit this table, we will get an error:</span></span>

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

<span data-ttu-id="4b319-189">Msg 35346, úroveň 15, State 1, řádek 44 ROZDĚLIT klauzule příkazu ALTER partition se nezdařila, protože hello oddíl není prázdný.</span><span class="sxs-lookup"><span data-stu-id="4b319-189">Msg 35346, Level 15, State 1, Line 44 SPLIT clause of ALTER PARTITION statement failed because hello partition is not empty.</span></span>  <span data-ttu-id="4b319-190">V lze rozdělit jen prázdné oddíly, když existuje columnstore index v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="4b319-190">Only empty partitions can be split in when a columnstore index exists on hello table.</span></span> <span data-ttu-id="4b319-191">Zvažte zakázání hello index columnstore před spuštěním příkazu ALTER PARTITION hello a pak znovu sestavit hello columnstore index po dokončení příkazu ALTER PARTITION.</span><span class="sxs-lookup"><span data-stu-id="4b319-191">Consider disabling hello columnstore index before issuing hello ALTER PARTITION statement, then rebuilding hello columnstore index after ALTER PARTITION is complete.</span></span>

<span data-ttu-id="4b319-192">Ale můžeme použít `CTAS` toocreate nové tabulky toohold naše data.</span><span class="sxs-lookup"><span data-stu-id="4b319-192">However, we can use `CTAS` toocreate a new table toohold our data.</span></span>

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

<span data-ttu-id="4b319-193">Jako hranice oddílů hello je zarovnán přepínač je povolená.</span><span class="sxs-lookup"><span data-stu-id="4b319-193">As hello partition boundaries are aligned a switch is permitted.</span></span> <span data-ttu-id="4b319-194">Zdrojová tabulka hello to bude nechte prázdné oddílu, který může následně rozdělit.</span><span class="sxs-lookup"><span data-stu-id="4b319-194">This will leave hello source table with an empty partition that we can subsequently split.</span></span>

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 too FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

<span data-ttu-id="4b319-195">Všechno, co je ponechán toodo je tooalign naše data toohello oddílu nové hranice pomocí `CTAS` a přepněte zpět v hlavní tabulka toohello našich dat</span><span class="sxs-lookup"><span data-stu-id="4b319-195">All that is left toodo is tooalign our data toohello new partition boundaries using `CTAS` and switch our data back in toohello main table</span></span>

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

<span data-ttu-id="4b319-196">Po dokončení hello pohybů hello dat je statistické údaje vhodné toorefresh hello na hello cílové tabulky tooensure hello nový distribuční hello dat v jejich příslušné oddíly přesně odrážel:</span><span class="sxs-lookup"><span data-stu-id="4b319-196">Once you have completed hello movement of hello data it is a good idea toorefresh hello statistics on hello target table tooensure they accurately reflect hello new distribution of hello data in their respective partitions:</span></span>

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a><span data-ttu-id="4b319-197">Tabulka dělení zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="4b319-197">Table partitioning source control</span></span>
<span data-ttu-id="4b319-198">tooavoid vaše definice tabulky z **koroze** v systému správy zdrojů může být vhodné tooconsider hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="4b319-198">tooavoid your table definition from **rusting** in your source control system you may want tooconsider hello following approach:</span></span>

1. <span data-ttu-id="4b319-199">Vytvoření tabulky hello jako dělenou tabulku, ale žádné hodnoty pro oddíl</span><span class="sxs-lookup"><span data-stu-id="4b319-199">Create hello table as a partitioned table but with no partition values</span></span>

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

1. <span data-ttu-id="4b319-200">`SPLIT`Hello tabulky jako součást procesu nasazení hello:</span><span class="sxs-lookup"><span data-stu-id="4b319-200">`SPLIT` hello table as part of hello deployment process:</span></span>

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

<span data-ttu-id="4b319-201">S Tento přístup hello zůstane statické kódu ve správě zdrojového kódu a dělení hodnoty hranice hello jsou povoleny toobe dynamické; vyvíjejí hello skladu v čase.</span><span class="sxs-lookup"><span data-stu-id="4b319-201">With this approach hello code in source control remains static and hello partitioning boundary values are allowed toobe dynamic; evolving with hello warehouse over time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b319-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b319-202">Next steps</span></span>
<span data-ttu-id="4b319-203">články hello toolearn více, najdete na [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuci tabulku] [ Distribute], [Indexování tabulku][Index], [zachování statistiky tabulky] [ Statistics] a [ Dočasné tabulky][Temporary].</span><span class="sxs-lookup"><span data-stu-id="4b319-203">toolearn more, see hello articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index], [Maintaining Table Statistics][Statistics] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="4b319-204">Další informace o osvědčených postupech najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="4b319-204">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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
