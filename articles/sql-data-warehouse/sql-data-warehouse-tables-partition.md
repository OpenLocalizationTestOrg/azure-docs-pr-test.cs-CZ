---
title: "Vytváření oddílů tabulky v SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 3edfd34d368228be32afef48688739639a3b03ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a><span data-ttu-id="dd2c5-103">Vytváření oddílů tabulky v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="dd2c5-103">Partitioning tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="dd2c5-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="dd2c5-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="dd2c5-105">[Datové typy][Data Types]</span><span class="sxs-lookup"><span data-stu-id="dd2c5-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="dd2c5-106">[Distribuce][Distribute]</span><span class="sxs-lookup"><span data-stu-id="dd2c5-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="dd2c5-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="dd2c5-107">[Index][Index]</span></span>
> * <span data-ttu-id="dd2c5-108">[Oddíl][Partition]</span><span class="sxs-lookup"><span data-stu-id="dd2c5-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="dd2c5-109">[Statistiky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="dd2c5-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="dd2c5-110">[Dočasné][Temporary]</span><span class="sxs-lookup"><span data-stu-id="dd2c5-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="dd2c5-111">Vytváření oddílů je podporován u všech typů tabulek SQL Data Warehouse; včetně clusterových columnstore, clusterovaný index a haldy.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-111">Partitioning is supported on all SQL Data Warehouse table types; including clustered columnstore, clustered index, and heap.</span></span>  <span data-ttu-id="dd2c5-112">Vytváření oddílů je podporováno také na všechny typy distribučních, včetně hodnoty hash nebo kruhové dotazování distribuován.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-112">Partitioning is also supported on all distribution types, including both hash or round robin distributed.</span></span>  <span data-ttu-id="dd2c5-113">Vytváření oddílů umožňuje vám data rozdělíte do menší skupiny dat a ve většině případů dělení se provádí na sloupci, datum.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-113">Partitioning enables you to divide your data into smaller groups of data and in most cases, partitioning is done on a date column.</span></span>

## <a name="benefits-of-partitioning"></a><span data-ttu-id="dd2c5-114">Výhody dělení</span><span class="sxs-lookup"><span data-stu-id="dd2c5-114">Benefits of partitioning</span></span>
<span data-ttu-id="dd2c5-115">Vytváření oddílů využívat data výkonu údržby a dotazů.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-115">Partitioning can benefit data maintenance and query performance.</span></span>  <span data-ttu-id="dd2c5-116">Jestli výhody oba, nebo pouze jeden, je závislá na tom, jak načíst data a zda na stejný sloupec lze použít pro obě účely, protože vytváření oddílů je možné provést pouze na jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-116">Whether it benefits both or just one is dependent on how data is loaded and whether the same column can be used for both purposes, since partitioning can only be done on one column.</span></span>

### <a name="benefits-to-loads"></a><span data-ttu-id="dd2c5-117">Výhody zatížení</span><span class="sxs-lookup"><span data-stu-id="dd2c5-117">Benefits to loads</span></span>
<span data-ttu-id="dd2c5-118">Hlavní výhoda vytváření oddílů v SQL Data Warehouse je zvýšit efektivitu a výkon načtení dat pomocí použití odstranění oddílu, přepínání a slučování.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-118">The primary benefit of partitioning in SQL Data Warehouse is improve the efficiency and performance of loading data by use of partition deletion, switching and merging.</span></span>  <span data-ttu-id="dd2c5-119">Ve většině případů jsou data rozdělena na sloupec data, která úzce souvisí pořadí, ve kterém je načíst data do databáze.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-119">In most cases data is partitioned on a date column that is closely tied to the sequence which the data is loaded to the database.</span></span>  <span data-ttu-id="dd2c5-120">Jednou z výhod použití oddíly pro zachování dat největší ho předcházení protokolování transakcí.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-120">One of the greatest benefits of using partitions to maintain data it the avoidance of transaction logging.</span></span>  <span data-ttu-id="dd2c5-121">Když jednoduše vkládání, aktualizaci nebo odstranění dat může být nejjednodušší způsob, s malým množstvím myšlenku a úsilí, pomocí dělení během procesu vaše zatížení může podstatně zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-121">While simply inserting, updating or deleting data can be the most straightforward approach, with a little thought and effort, using partitioning during your load process can substantially improve performance.</span></span>

<span data-ttu-id="dd2c5-122">Přepnutí oddílu umožňuje rychle odeberte nebo nahraďte oddíl tabulky.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-122">Partition switching can be used to quickly remove or replace a section of a table.</span></span>  <span data-ttu-id="dd2c5-123">Tabulka faktů prodeje například může obsahovat jen data po dobu posledních 36 měsíců.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-123">For example, a sales fact table might contain just data for the past 36 months.</span></span>  <span data-ttu-id="dd2c5-124">Na konci každého měsíce se odstraní nejstarší měsíc prodejních dat z tabulky.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-124">At the end of every month, the oldest month of sales data is deleted from the table.</span></span>  <span data-ttu-id="dd2c5-125">Tato data může odstranit pomocí příkazu delete k odstranění dat nejstarší měsíc.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-125">This data could be deleted by using a delete statement to delete the data for the oldest month.</span></span>  <span data-ttu-id="dd2c5-126">Ale odstraňování velké množství dat řádek po řádku příkazem delete může trvat velmi dlouho, stejně jako vytvořit riziko velké transakcí, které může trvat dlouhou dobu vrátit zpět, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-126">However, deleting a large amount of data row-by-row with a delete statement can take a very long time, as well as create the risk of large transactions which could take a long time to rollback if something goes wrong.</span></span>  <span data-ttu-id="dd2c5-127">Více optimální metodu je jednoduše vyřadit nejstarší oddílu data.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-127">A more optimal approach is to simply drop the oldest partition of data.</span></span>  <span data-ttu-id="dd2c5-128">Kde odstranění jednotlivých řádků může trvat hodiny, odstraňování celý oddíl může trvat sekund.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-128">Where deleting the individual rows could take hours, deleting an entire partition could take seconds.</span></span>

### <a name="benefits-to-queries"></a><span data-ttu-id="dd2c5-129">Výhody pro dotazy</span><span class="sxs-lookup"><span data-stu-id="dd2c5-129">Benefits to queries</span></span>
<span data-ttu-id="dd2c5-130">Vytváření oddílů můžete použít také pro zlepšení výkonu dotazů.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-130">Partitioning can also be used to improve query performance.</span></span>  <span data-ttu-id="dd2c5-131">Pokud dotaz použije filtr na sloupec rozdělení, to můžete omezit prohledávání pouze opravňující oddíly, které můžou být mnohem menší podmnožinu dat, zabraňující prohledání úplnou tabulky.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-131">If a query applies a filter on a partitioned column, this can limit the scan to only the qualifying partitions which may be a much smaller subset of the data, avoiding a full table scan.</span></span>  <span data-ttu-id="dd2c5-132">Se zavedením Clusterované indexy columnstore jsou méně výhodné predikátem odstranění výkonnostních výhod, ale v některých případech může být výhoda dotazy.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-132">With the introduction of clustered columnstore indexes, the predicate elimination performance benefits are less beneficial, but in some cases there can be a benefit to queries.</span></span>  <span data-ttu-id="dd2c5-133">Například pokud tabulka faktů prodeje jsou rozděleny do 36 měsíců pomocí pole Datum prodeje a potom se dotazuje tento filtr na datum prodej můžete přeskočit hledání v oddíly, které neodpovídají filtru.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-133">For example, if the sales fact table is partitioned into 36 months using the sales date field, then queries that filter on the sale date can skip searching in partitions that don’t match the filter.</span></span>

## <a name="partition-sizing-guidance"></a><span data-ttu-id="dd2c5-134">Pokyny k dimenzování oddílu</span><span class="sxs-lookup"><span data-stu-id="dd2c5-134">Partition sizing guidance</span></span>
<span data-ttu-id="dd2c5-135">Při vytváření oddílů lze použít ke zlepšení výkonu některých scénářích, vytváření tabulku s **příliš mnoho** oddíly může narušit výkonnost za určitých okolností.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-135">While partitioning can be used to improve performance some scenarios, creating a table with **too many** partitions can hurt performance under some circumstances.</span></span>  <span data-ttu-id="dd2c5-136">Tyto problémy jsou především pro Clusterované tabulky columnstore.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-136">These concerns are especially true for clustered columnstore tables.</span></span>  <span data-ttu-id="dd2c5-137">Pro dělení být užitečné, je důležité pochopit, kdy použít vytváření oddílů a počet oddílů pro vytvoření.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-137">For partitioning to be helpful, it is important to understand when to use partitioning and the number of partitions to create.</span></span>  <span data-ttu-id="dd2c5-138">Není pevný rychlé pravidlo, kolik oddíly jsou příliš mnoho, závisí na vaše data a kolik oddíly jsou načítání současně.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-138">There is no hard fast rule as to how many partitions are too many, it depends on your data and how many partitions you are loading to simultaneously.</span></span>  <span data-ttu-id="dd2c5-139">Ale jako obecné pravidlo, vezměte v úvahu přidávání 10s k 100s oddílů není 1000s.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-139">But as a general rule of thumb, think of adding 10s to 100s of partitions, not 1000s.</span></span>

<span data-ttu-id="dd2c5-140">Při vytváření dělení na **Clusterové columnstore** tabulky, je důležité zvážit, kolik řádků se nebude zobrazovat v každém oddílu.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-140">When creating partitioning on **clustered columnstore** tables, it is important to consider how many rows will land in each partition.</span></span>  <span data-ttu-id="dd2c5-141">Pro optimální komprese a výkon Clusterované tabulky columnstore je potřeba minimálně 1 milionu řádků na distribuce a oddíl.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-141">For optimal compression and performance of clustered columnstore tables, a minimum of 1 million rows per distribution and partition is needed.</span></span>  <span data-ttu-id="dd2c5-142">Před vytvořením oddíly, SQL Data Warehouse každá tabulka již rozdělí na 60 distribuované databáze.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-142">Before partitions are created, SQL Data Warehouse already divides each table into 60 distributed databases.</span></span>  <span data-ttu-id="dd2c5-143">Všechny oddíly přidat do tabulky je kromě distribuce vytvořen na pozadí.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-143">Any partitioning added to a table is in addition to the distributions created behind the scenes.</span></span>  <span data-ttu-id="dd2c5-144">V tomto příkladu, pokud tabulka faktů prodeje obsažené 36 měsíční oddíly a vzhledem k tomu, že SQL Data Warehouse je 60 distribuce, pak tabulky faktů prodeje by měl obsahovat 60 milionu řádků měsíčně nebo 2.1 miliardy řádků při zaplnění všechny měsíce.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-144">Using this example, if the sales fact table contained 36 monthly partitions, and given that SQL Data Warehouse has 60 distributions, then the sales fact table should contain 60 million rows per month, or 2.1 billion rows when all months are populated.</span></span>  <span data-ttu-id="dd2c5-145">Pokud tabulka obsahuje výrazně méně řádků, než je minimální doporučený počet řádků na jeden oddíl, zvažte použití méně oddíly aby bylo možné zvýšit počet řádků na jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-145">If a table contains significantly less rows than the recommended minimum number of rows per partition, consider using fewer partitions in order to make increase the number of rows per partition.</span></span>  <span data-ttu-id="dd2c5-146">Viz také [indexování] [ Index] článek, který obsahuje dotazy, které lze spustit v SQL Data Warehouse k vyhodnocení kvality indexy columnstore clusteru.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-146">Also see the [Indexing][Index] article which includes queries that can be run on SQL Data Warehouse to assess the quality of cluster columnstore indexes.</span></span>

## <a name="syntax-difference-from-sql-server"></a><span data-ttu-id="dd2c5-147">Syntaxe rozdíl oproti systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd2c5-147">Syntax difference from SQL Server</span></span>
<span data-ttu-id="dd2c5-148">SQL Data Warehouse zavádí zjednodušenou definice oddíly, který se mírně liší od systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-148">SQL Data Warehouse introduces a simplified definition of partitions which is slightly different from SQL Server.</span></span>  <span data-ttu-id="dd2c5-149">Vytváření oddílů funkce a schémata nejsou použity v SQL Data Warehouse, jako jsou v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-149">Partitioning functions and schemes are not used in SQL Data Warehouse as they are in SQL Server.</span></span>  <span data-ttu-id="dd2c5-150">Místo toho, které musíte udělat je identifikaci oddílů sloupce a body hranic.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-150">Instead, all you need to do is identify partitioned column and the boundary points.</span></span>  <span data-ttu-id="dd2c5-151">Syntaxe vytváření oddílů se mírně liší v systému SQL Server, se základními koncepty jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-151">While the syntax of partitioning may be slightly different from SQL Server, the basic concepts are the same.</span></span>  <span data-ttu-id="dd2c5-152">SQL Server a SQL Data Warehouse podporují jeden sloupec oddílu na jednu tabulku, která může být pohyboval oddílu.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-152">SQL Server and SQL Data Warehouse support one partition column per table, which can be ranged partition.</span></span>  <span data-ttu-id="dd2c5-153">Další informace o oddílech najdete v tématu [rozdělena na oddíly tabulky a indexy][Partitioned Tables and Indexes].</span><span class="sxs-lookup"><span data-stu-id="dd2c5-153">To learn more about partitioning, see [Partitioned Tables and Indexes][Partitioned Tables and Indexes].</span></span>

<span data-ttu-id="dd2c5-154">Následujícím příkladu SQL Data Warehouse rozdělena na oddíly [CREATE TABLE] [ CREATE TABLE] příkaz oddíly tabulka FactInternetSales na sloupci OrderDateKey:</span><span class="sxs-lookup"><span data-stu-id="dd2c5-154">The below example of a SQL Data Warehouse partitioned [CREATE TABLE][CREATE TABLE] statement, partitions the FactInternetSales table on the OrderDateKey column:</span></span>

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

## <a name="migrating-partitioning-from-sql-server"></a><span data-ttu-id="dd2c5-155">Migrace, vytváření oddílů v systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd2c5-155">Migrating partitioning from SQL Server</span></span>
<span data-ttu-id="dd2c5-156">Pokud chcete migrovat definice oddíl systému SQL Server do SQL Data Warehouse jednoduše:</span><span class="sxs-lookup"><span data-stu-id="dd2c5-156">To migrate SQL Server partition definitions to SQL Data Warehouse simply:</span></span>

* <span data-ttu-id="dd2c5-157">Odstranění serveru SQL Server [schéma oddílu][partition scheme].</span><span class="sxs-lookup"><span data-stu-id="dd2c5-157">Eliminate the SQL Server [partition scheme][partition scheme].</span></span>
* <span data-ttu-id="dd2c5-158">Přidat [oddílu funkce] [ partition function] definici tak, aby vaše CREATE TABLE.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-158">Add the [partition function][partition function] definition to your CREATE TABLE.</span></span>

<span data-ttu-id="dd2c5-159">Pokud provádíte migraci dělenou tabulku z instance systému SQL Server nižší než SQL vám může pomoci zjistěte počet řádků, které jsou v každém oddílu.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-159">If you are migrating a partitioned table from a SQL Server instance the below SQL can help you to interrogate the number of rows that are in each partition.</span></span>  <span data-ttu-id="dd2c5-160">Uvědomte si, že pokud se v SQL Data Warehouse používá stejnou členitost rozdělení, počet řádků na jeden oddíl se sníží o faktor 60.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-160">Keep in mind that if the same partitioning granularity is used on SQL Data Warehouse, the number of rows per partition will decrease by a factor of 60.</span></span>  

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

## <a name="workload-management"></a><span data-ttu-id="dd2c5-161">Správa zatížení</span><span class="sxs-lookup"><span data-stu-id="dd2c5-161">Workload management</span></span>
<span data-ttu-id="dd2c5-162">Jeden aspekt poslední díl okolnosti rozhodnutí oddílu tabulky je [úlohy správy][workload management].</span><span class="sxs-lookup"><span data-stu-id="dd2c5-162">One final piece consideration to factor in to the table partition decision is [workload management][workload management].</span></span>  <span data-ttu-id="dd2c5-163">Úlohy správy v SQL Data Warehouse je primárně správy paměti a souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-163">Workload management in SQL Data Warehouse is primarily the management of memory and concurrency.</span></span>  <span data-ttu-id="dd2c5-164">Maximální paměť přidělená pro každý distribuční během provádění dotazů v SQL Data Warehouse je třídy upraveny prostředků.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-164">In SQL Data Warehouse the maximum memory allocated to each distribution during query execution is governed resource classes.</span></span>  <span data-ttu-id="dd2c5-165">V ideálním případě bude mít velikost oddílů s ohledem na dalších faktorech, jako je potřebnou velikost paměti pro vytváření Clusterované indexy columnstore.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-165">Ideally your partitions will be sized in consideration of other factors like the memory needs of building clustered columnstore indexes.</span></span>  <span data-ttu-id="dd2c5-166">Clusterovaný benefit indexy columnstore výrazně při jejich přidělení více paměti.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-166">Clustered columnstore indexes benefit greatly when they are allocated more memory.</span></span>  <span data-ttu-id="dd2c5-167">Proto můžete zajistit, že nové vytvoření oddílu indexu není nedostatek paměti.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-167">Therefore, you will want to ensure that a partition index rebuild is not starved of memory.</span></span> <span data-ttu-id="dd2c5-168">Při přechodu z výchozí role, smallrc, jeden z jiných rolí, například largerc lze dosáhnout zvýšení množství paměti k dispozici pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-168">Increasing the amount of memory available to your query can be achieved by switching from the default role, smallrc, to one of the other roles such as largerc.</span></span>

<span data-ttu-id="dd2c5-169">Informace o přidělení paměti na jeden distribuční je k dispozici pomocí dotazu na zobrazení dynamické správy Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-169">Information on the allocation of memory per distribution is available by querying the resource governor dynamic management views.</span></span> <span data-ttu-id="dd2c5-170">Ve skutečnosti vaší přidělení paměti bude menší než údaje níže.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-170">In reality your memory grant will be less than the figures below.</span></span> <span data-ttu-id="dd2c5-171">To však poskytuje úroveň pokyny, které můžete použít, když vaše oddíly pro operace správy dat pro definování velikosti.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-171">However, this provides a level of guidance that you can use when sizing your partitions for data management operations.</span></span>  <span data-ttu-id="dd2c5-172">Pokuste se vyhnout, změna velikosti vašeho oddíly nad rámec poskytovaný třída velmi velké prostředků přidělení paměti.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-172">Try to avoid sizing your partitions beyond the memory grant provided by the extra large resource class.</span></span> <span data-ttu-id="dd2c5-173">Pokud vaše oddíly růst nad rámec tohoto obrázku spuštěním riziko přetížení paměti, což pak vede k menší optimální komprese.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-173">If your partitions grow beyond this figure you run the risk of memory pressure which in turn leads to less optimal compression.</span></span>

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

## <a name="partition-switching"></a><span data-ttu-id="dd2c5-174">Přepnutí oddílu</span><span class="sxs-lookup"><span data-stu-id="dd2c5-174">Partition switching</span></span>
<span data-ttu-id="dd2c5-175">SQL Data Warehouse podporuje oddílu rozdělení, sloučení a přepínání.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-175">SQL Data Warehouse supports partition splitting, merging, and switching.</span></span> <span data-ttu-id="dd2c5-176">Každá z těchto funkcí je excuted pomocí [příkaz ALTER TABLE] [ ALTER TABLE] příkaz.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-176">Each of these functions is excuted using the [ALTER TABLE][ALTER TABLE] statement.</span></span>

<span data-ttu-id="dd2c5-177">Přepnout oddíly mezi dvěma tabulkami musíte zajistit, že oddíly zarovnat na jejich odpovídající hranice a jestli se shodují definice tabulky.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-177">To switch partitions between two tables you must ensure that the partitions align on their respective boundaries and that the table definitions match.</span></span> <span data-ttu-id="dd2c5-178">Protože nejsou k dispozici pro vynucení rozsahu hodnot v tabulce omezení check zdrojové tabulky musí obsahovat stejné hranice oddílů jako cílová tabulka.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-178">As check constraints are not available to enforce the range of values in a table the source table must contain the same partition boundaries as the target table.</span></span> <span data-ttu-id="dd2c5-179">Pokud tomu tak není, pak přepínače oddílu selžou, protože metadata oddílu nebudou synchronizovány.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-179">If this is not the case, then the partition switch will fail as the partition metadata will not be synchronized.</span></span>

### <a name="how-to-split-a-partition-that-contains-data"></a><span data-ttu-id="dd2c5-180">Jak rozdělit oddíl, který obsahuje data</span><span class="sxs-lookup"><span data-stu-id="dd2c5-180">How to split a partition that contains data</span></span>
<span data-ttu-id="dd2c5-181">Nejúčinnější metodou rozdělit oddíl, který už obsahuje data je použití `CTAS` příkaz.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-181">The most efficient method to split a partition that already contains data is to use a `CTAS` statement.</span></span> <span data-ttu-id="dd2c5-182">Pokud je tabulka oddílů Clusterové columnstore pak oddíl tabulky musí být prázdná předtím, než je možné rozdělit.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-182">If the partitioned table is a clustered columnstore then the table partition must be empty before it can be split.</span></span>

<span data-ttu-id="dd2c5-183">Níže je ukázka tabulku oddílů columnstore obsahující jeden řádek v každém oddílu:</span><span class="sxs-lookup"><span data-stu-id="dd2c5-183">Below is a sample partitioned columnstore table containing one row in each partition:</span></span>

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
> <span data-ttu-id="dd2c5-184">Vytvořením objektu statistiky jsme Ujistěte se, že tento metadat tabulky je přesnější.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-184">By Creating the statistic object, we ensure that table metadata is more accurate.</span></span> <span data-ttu-id="dd2c5-185">Pokud jsme vynechat, vytváření statistik, SQL Data Warehouse použije výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-185">If we omit creating statistics, then SQL Data Warehouse will use default values.</span></span> <span data-ttu-id="dd2c5-186">Pro Zkontrolujte prosím podrobnosti o statistiky [statistiky][statistics].</span><span class="sxs-lookup"><span data-stu-id="dd2c5-186">For details on statistics please review [statistics][statistics].</span></span>
> 
> 

<span data-ttu-id="dd2c5-187">Jsme dotaz s využitím počet řádků `sys.partitions` katalogu zobrazení:</span><span class="sxs-lookup"><span data-stu-id="dd2c5-187">We can then query for the row count using the `sys.partitions` catalog view:</span></span>

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

<span data-ttu-id="dd2c5-188">Pokud jsme zkuste rozdělit tuto tabulku, jsme dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="dd2c5-188">If we try to split this table, we will get an error:</span></span>

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

<span data-ttu-id="dd2c5-189">Msg 35346, úroveň 15, State 1, řádek 44 ROZDĚLIT klauzule příkazu ALTER partition se nezdařila, protože oddíl není prázdný.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-189">Msg 35346, Level 15, State 1, Line 44 SPLIT clause of ALTER PARTITION statement failed because the partition is not empty.</span></span>  <span data-ttu-id="dd2c5-190">V lze rozdělit jen prázdné oddíly, když existuje columnstore index v tabulce.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-190">Only empty partitions can be split in when a columnstore index exists on the table.</span></span> <span data-ttu-id="dd2c5-191">Zvažte zakázání index columnstore před spuštěním příkazu ALTER PARTITION příkazu a pak znovu sestavit columnstore index po dokončení příkazu ALTER PARTITION.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-191">Consider disabling the columnstore index before issuing the ALTER PARTITION statement, then rebuilding the columnstore index after ALTER PARTITION is complete.</span></span>

<span data-ttu-id="dd2c5-192">Ale můžeme použít `CTAS` vytvořit novou tabulku pro naše data.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-192">However, we can use `CTAS` to create a new table to hold our data.</span></span>

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

<span data-ttu-id="dd2c5-193">Jako hranice oddílů jsou v souladu přepínači je povolená.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-193">As the partition boundaries are aligned a switch is permitted.</span></span> <span data-ttu-id="dd2c5-194">Zdrojová tabulka to bude nechte prázdné oddílu, který může následně rozdělit.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-194">This will leave the source table with an empty partition that we can subsequently split.</span></span>

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

<span data-ttu-id="dd2c5-195">Již zbývá udělat, je zarovnat naše data do nové hranice oddílů pomocí `CTAS` a přepínače naše data zpět do hlavní tabulky</span><span class="sxs-lookup"><span data-stu-id="dd2c5-195">All that is left to do is to align our data to the new partition boundaries using `CTAS` and switch our data back in to the main table</span></span>

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

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

<span data-ttu-id="dd2c5-196">Po dokončení přesunu dat je vhodné aktualizovat statistiku v cílové tabulce zajistit, že se přesně odrážel novou rozdělení dat v jejich příslušné oddíly:</span><span class="sxs-lookup"><span data-stu-id="dd2c5-196">Once you have completed the movement of the data it is a good idea to refresh the statistics on the target table to ensure they accurately reflect the new distribution of the data in their respective partitions:</span></span>

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a><span data-ttu-id="dd2c5-197">Tabulka dělení zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="dd2c5-197">Table partitioning source control</span></span>
<span data-ttu-id="dd2c5-198">Aby se zabránilo vaše definice tabulky z **koroze** v systému správy zdrojů je vhodné vzít v úvahu následující postup:</span><span class="sxs-lookup"><span data-stu-id="dd2c5-198">To avoid your table definition from **rusting** in your source control system you may want to consider the following approach:</span></span>

1. <span data-ttu-id="dd2c5-199">Vytvořit tabulku jako tabulku oddílů, ale žádné hodnoty pro oddíl</span><span class="sxs-lookup"><span data-stu-id="dd2c5-199">Create the table as a partitioned table but with no partition values</span></span>

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

1. <span data-ttu-id="dd2c5-200">`SPLIT`v tabulce v rámci procesu nasazení:</span><span class="sxs-lookup"><span data-stu-id="dd2c5-200">`SPLIT` the table as part of the deployment process:</span></span>

```sql
-- Create a table containing the partition boundaries

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

-- Iterate over the partition boundaries and split the table

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

<span data-ttu-id="dd2c5-201">S tímto přístupem zůstane statické kód ve správě zdrojového kódu a dělení hodnoty hranic mohou být dynamické; vyvíjející se k skladu v čase.</span><span class="sxs-lookup"><span data-stu-id="dd2c5-201">With this approach the code in source control remains static and the partitioning boundary values are allowed to be dynamic; evolving with the warehouse over time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd2c5-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd2c5-202">Next steps</span></span>
<span data-ttu-id="dd2c5-203">Další informace najdete v článcích na [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuci tabulku] [ Distribute], [Indexování tabulku][Index], [zachování statistiky tabulky] [ Statistics] a [Dočasných tabulek][Temporary].</span><span class="sxs-lookup"><span data-stu-id="dd2c5-203">To learn more, see the articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index], [Maintaining Table Statistics][Statistics] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="dd2c5-204">Další informace o osvědčených postupech najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="dd2c5-204">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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
