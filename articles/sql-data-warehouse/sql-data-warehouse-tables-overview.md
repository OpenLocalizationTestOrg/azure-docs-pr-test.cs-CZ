---
title: aaaOverview tabulek v SQL Data Warehouse | Microsoft Docs
description: "Začínáme s Azure SQL Data Warehouse tabulky."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 2114d9ad-c113-43da-859f-419d72604bdf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 06/29/2016
ms.author: shigu;jrj
ms.openlocfilehash: 4edabcb4b0754bf6c99c2b6b3f0c077749051d9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-tables-in-sql-data-warehouse"></a><span data-ttu-id="04026-103">Přehled tabulek v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="04026-103">Overview of tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="04026-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="04026-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="04026-105">[Datové typy][Data Types]</span><span class="sxs-lookup"><span data-stu-id="04026-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="04026-106">[Distribuce][Distribute]</span><span class="sxs-lookup"><span data-stu-id="04026-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="04026-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="04026-107">[Index][Index]</span></span>
> * <span data-ttu-id="04026-108">[Oddíl][Partition]</span><span class="sxs-lookup"><span data-stu-id="04026-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="04026-109">[Statistiky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="04026-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="04026-110">[Dočasné][Temporary]</span><span class="sxs-lookup"><span data-stu-id="04026-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="04026-111">Začínáme s vytváření tabulek v SQL Data Warehouse je jednoduché.</span><span class="sxs-lookup"><span data-stu-id="04026-111">Getting started with creating tables in SQL Data Warehouse is simple.</span></span>  <span data-ttu-id="04026-112">Hello základní [CREATE TABLE] [ CREATE TABLE] syntaxe následuje hello běžné syntaxe se pravděpodobně už znáte z práce jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="04026-112">hello basic [CREATE TABLE][CREATE TABLE] syntax follows hello common syntax you are most likely already familiar with from working with other databases.</span></span>  <span data-ttu-id="04026-113">toocreate tabulku, potřebujete tooname tabulka, název sloupce a definování typů dat pro každý sloupec.</span><span class="sxs-lookup"><span data-stu-id="04026-113">toocreate a table, you simply need tooname your table, name your columns and define data types for each column.</span></span>  <span data-ttu-id="04026-114">Pokud jste vytváření tabulek v jiných databázích, to by měla vypadat dobře obeznámeni tooyou.</span><span class="sxs-lookup"><span data-stu-id="04026-114">If you've create tables in other databases, this should look very familiar tooyou.</span></span>

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

<span data-ttu-id="04026-115">Hello výše příklad vytvoří tabulku s názvem zákazníků se dva sloupce, jméno a příjmení.</span><span class="sxs-lookup"><span data-stu-id="04026-115">hello above example creates a table named Customers with two columns, FirstName and LastName.</span></span>  <span data-ttu-id="04026-116">Každý sloupec je definován s datovým typem VARCHAR(25), což omezí hello data too25 znaků.</span><span class="sxs-lookup"><span data-stu-id="04026-116">Each column is defined with a data type of VARCHAR(25), which limits hello data too25 characters.</span></span>  <span data-ttu-id="04026-117">Tyto atributy základní tabulku, jakož i ostatní uživatelé jsou většinou hello stejné jako jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="04026-117">These fundamental attributes of a table, as well as others, are mostly hello same as other databases.</span></span>  <span data-ttu-id="04026-118">Datové typy jsou definovány pro každý sloupec a zajišťují integritu hello vaše data.</span><span class="sxs-lookup"><span data-stu-id="04026-118">Data types are defined for each column and ensure hello integrity of your data.</span></span>  <span data-ttu-id="04026-119">Indexy lze přidat tooimprove výkon snížením vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="04026-119">Indexes can be added tooimprove performance by reducing I/O.</span></span>  <span data-ttu-id="04026-120">Vytváření oddílů lze přidat tooimprove výkon při potřebujete toomodify data.</span><span class="sxs-lookup"><span data-stu-id="04026-120">Partitioning can be added tooimprove performance when you need toomodify data.</span></span>

<span data-ttu-id="04026-121">[Přejmenování] [ RENAME] SQL Data Warehouse tabulku vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="04026-121">[Renaming][RENAME] a SQL Data Warehouse table looks like this:</span></span>

```sql  
RENAME OBJECT Customer tooCustomerOrig; 
 ```

## <a name="distributed-tables"></a><span data-ttu-id="04026-122">Distribuované tabulky</span><span class="sxs-lookup"><span data-stu-id="04026-122">Distributed tables</span></span>
<span data-ttu-id="04026-123">Nový základní atribut zaváděné distribuovaných systémů, jako je datový sklad SQL je hello **distribuční sloupce**.</span><span class="sxs-lookup"><span data-stu-id="04026-123">A new fundamental attribute introduced by distributed systems like SQL Data Warehouse is hello **distribution column**.</span></span>  <span data-ttu-id="04026-124">Hello distribuční sloupce je velmi mnohem co vypadá jako.</span><span class="sxs-lookup"><span data-stu-id="04026-124">hello distribution column is very much what it sounds like.</span></span>  <span data-ttu-id="04026-125">Je hello sloupec, který určuje, jak toodistribute, nebo dělení, vaše data pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="04026-125">It is hello column that determines how toodistribute, or divide, your data behind hello scenes.</span></span>  <span data-ttu-id="04026-126">Při vytváření tabulky bez zadání hello distribuční sloupce je tabulka hello automaticky distribuovaných přes **kruhové dotazování**.</span><span class="sxs-lookup"><span data-stu-id="04026-126">When you create a table without specifying hello distribution column, hello table is automatically distributed using **round robin**.</span></span>  <span data-ttu-id="04026-127">Kruhové dotazování tabulky mohou být dostatečná v některých scénářích, definování distribuční sloupce může výrazně omezit přesun dat během dotazy, proto optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="04026-127">While round robin tables can be sufficient in some scenarios, defining distribution columns can greatly reduce data movement during queries, thus optimizing performance.</span></span>  <span data-ttu-id="04026-128">V situacích, kde existuje malé množství dat v tabulce, výběr toocreate hello tabulku s hello **replikovat** distribuční typ zkopíruje data tooeach výpočetním uzlu a uloží přesun dat v době provedení dotazu.</span><span class="sxs-lookup"><span data-stu-id="04026-128">In situations where there is a small amount of data in a table, choosing toocreate hello table with hello **replicate** distribution type copies data tooeach compute node and saves data movement at query execution time.</span></span> <span data-ttu-id="04026-129">V tématu [distribuci tabulku] [ Distribute] toolearn Další informace o tom tooselect distribuční sloupce.</span><span class="sxs-lookup"><span data-stu-id="04026-129">See [Distributing a Table][Distribute] toolearn more about how tooselect a distribution column.</span></span>

## <a name="indexing-and-partitioning-tables"></a><span data-ttu-id="04026-130">Indexování a rozdělení do oddílů tabulky</span><span class="sxs-lookup"><span data-stu-id="04026-130">Indexing and partitioning tables</span></span>
<span data-ttu-id="04026-131">Jako stát pokročilejší pomocí SQL Data Warehouse a chcete toooptimize výkonu, budete muset toolearn více informací o návrh tabulky.</span><span class="sxs-lookup"><span data-stu-id="04026-131">As you become more advanced in using SQL Data Warehouse and want toooptimize performance, you'll want toolearn more about Table Design.</span></span>  <span data-ttu-id="04026-132">články hello toolearn více, najdete na [tabulky datové typy][Data Types], [distribuci tabulku][Distribute], [indexování tabulku] [ Index] a [vytváření oddílů tabulky][Partition].</span><span class="sxs-lookup"><span data-stu-id="04026-132">toolearn more, see hello articles on [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index] and  [Partitioning a Table][Partition].</span></span>

## <a name="table-statistics"></a><span data-ttu-id="04026-133">Statistiky tabulky</span><span class="sxs-lookup"><span data-stu-id="04026-133">Table statistics</span></span>
<span data-ttu-id="04026-134">Statistiky jsou velmi důležité toogetting hello nejlepší výkon z SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="04026-134">Statistics are an extremely important toogetting hello best performance out of your SQL Data Warehouse.</span></span>  <span data-ttu-id="04026-135">Vzhledem k tomu, že SQL Data Warehouse automaticky ještě není vytvářet a aktualizovat statistiku pro vás, jako je pocházet tooexpect ve službě Azure SQL Database, čtení na náš článek [statistiky] [ Statistics] může být jedním z hello Nejdůležitější články číst tooensure získáte nejlepší výkon hello od své dotazy.</span><span class="sxs-lookup"><span data-stu-id="04026-135">Since SQL Data Warehouse does not yet automatically create and update statistics for you, like you may have come tooexpect in Azure SQL Database, reading our article on [Statistics][Statistics] might be one of hello most important articles you read tooensure that you get hello best performance from your queries.</span></span>

## <a name="temporary-tables"></a><span data-ttu-id="04026-136">Dočasné tabulky</span><span class="sxs-lookup"><span data-stu-id="04026-136">Temporary tables</span></span>
<span data-ttu-id="04026-137">Dočasné tabulky jsou tabulky, které pouze existovat hello dobu vaše přihlašovací jméno a ostatní uživatelé je nemohou vidět.</span><span class="sxs-lookup"><span data-stu-id="04026-137">Temporary tables are tables which only exist for hello duration of your logon and cannot be seen by other users.</span></span>  <span data-ttu-id="04026-138">Dočasné tabulky můžete být vhodné tooprevent ostatní vidět dočasné výsledky z a také snížit hello potřebu čištění.</span><span class="sxs-lookup"><span data-stu-id="04026-138">Temporary tables can be a good way tooprevent others from seeing temporary results and also reduce hello need for cleanup.</span></span>  <span data-ttu-id="04026-139">Vzhledem k tomu, že dočasných tabulek také používat místní úložiště, můžete nabízí vyšší výkon pro některé operace.</span><span class="sxs-lookup"><span data-stu-id="04026-139">Since temporary tables also utilize local storage, they can offer faster performance for some operations.</span></span>  <span data-ttu-id="04026-140">V tématu hello [dočasné tabulky] [ Temporary] články pro další podrobnosti o dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="04026-140">See hello [Temporary Table][Temporary] articles for more details about temporary tables.</span></span>

## <a name="external-tables"></a><span data-ttu-id="04026-141">Externí tabulky</span><span class="sxs-lookup"><span data-stu-id="04026-141">External tables</span></span>
<span data-ttu-id="04026-142">Externí tabulky, také známé jako Polybase tabulky, jsou tabulky, které může být dotazován z SQL Data Warehouse, ale toodata bodu externí z SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="04026-142">External tables, also known as Polybase tables, are tables which can be queried from SQL Data Warehouse, but point toodata external from SQL Data Warehouse.</span></span>  <span data-ttu-id="04026-143">Například můžete vytvoříte externí tabulku které toofiles body na Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="04026-143">For example, you can create an external table which points toofiles on Azure Blob Storage.</span></span>  <span data-ttu-id="04026-144">Další informace o tom, jak toocreate a dotaz externí tabulku, najdete v části [načtení dat pomocí funkce Polybase][Load data with Polybase].</span><span class="sxs-lookup"><span data-stu-id="04026-144">For more details on how toocreate and query an external table, see [Load data with Polybase][Load data with Polybase].</span></span>  

## <a name="unsupported-table-features"></a><span data-ttu-id="04026-145">Funkce nepodporované tabulky</span><span class="sxs-lookup"><span data-stu-id="04026-145">Unsupported table features</span></span>
<span data-ttu-id="04026-146">I když SQL Data Warehouse obsahuje řadu hello nabízí stejné funkce tabulky jiných databází, nejsou některé funkce, které ještě nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="04026-146">While SQL Data Warehouse contains many of hello same table features offered by other databases, there are some features which are not yet supported.</span></span>  <span data-ttu-id="04026-147">Níže je seznam některých hello tabulka funkce, které ještě nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="04026-147">Below is a list of some of hello table features which are not yet supported.</span></span>

| <span data-ttu-id="04026-148">Nepodporované funkce</span><span class="sxs-lookup"><span data-stu-id="04026-148">Unsupported features</span></span> |
| --- |
| <span data-ttu-id="04026-149">Primární klíč, cizí klíče, jedinečný a zkontrolujte [omezení tabulky][Table Constraints]</span><span class="sxs-lookup"><span data-stu-id="04026-149">Primary key, Foreign keys, Unique and Check [Table Constraints][Table Constraints]</span></span> |
| <span data-ttu-id="04026-150">[Jedinečné indexy][Unique Indexes]</span><span class="sxs-lookup"><span data-stu-id="04026-150">[Unique Indexes][Unique Indexes]</span></span> |
| <span data-ttu-id="04026-151">[Počítané sloupce][Computed Columns]</span><span class="sxs-lookup"><span data-stu-id="04026-151">[Computed Columns][Computed Columns]</span></span> |
| <span data-ttu-id="04026-152">[Zhuštěné sloupce][Sparse Columns]</span><span class="sxs-lookup"><span data-stu-id="04026-152">[Sparse Columns][Sparse Columns]</span></span> |
| <span data-ttu-id="04026-153">[Uživatelem definované typy][User-Defined Types]</span><span class="sxs-lookup"><span data-stu-id="04026-153">[User-Defined Types][User-Defined Types]</span></span> |
| <span data-ttu-id="04026-154">[Pořadí][Sequence]</span><span class="sxs-lookup"><span data-stu-id="04026-154">[Sequence][Sequence]</span></span> |
| <span data-ttu-id="04026-155">[Aktivační události][Triggers]</span><span class="sxs-lookup"><span data-stu-id="04026-155">[Triggers][Triggers]</span></span> |
| <span data-ttu-id="04026-156">[Indexovaná zobrazení][Indexed Views]</span><span class="sxs-lookup"><span data-stu-id="04026-156">[Indexed Views][Indexed Views]</span></span> |
| <span data-ttu-id="04026-157">[Synonyma.][Synonyms]</span><span class="sxs-lookup"><span data-stu-id="04026-157">[Synonyms][Synonyms]</span></span> |

## <a name="table-size-queries"></a><span data-ttu-id="04026-158">Dotazy na velikost tabulky</span><span class="sxs-lookup"><span data-stu-id="04026-158">Table size queries</span></span>
<span data-ttu-id="04026-159">Jedna jednoduchý způsob tooidentify místa a řádky, které spotřebovávají tabulku v každé z hello 60 distribuce je toouse [DBCC PDW_SHOWSPACEUSED][DBCC PDW_SHOWSPACEUSED].</span><span class="sxs-lookup"><span data-stu-id="04026-159">One simple way tooidentify space and rows consumed by a table in each of hello 60 distributions, is toouse [DBCC PDW_SHOWSPACEUSED][DBCC PDW_SHOWSPACEUSED].</span></span>

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

<span data-ttu-id="04026-160">Však příkazy DBCC může být poměrně omezení.</span><span class="sxs-lookup"><span data-stu-id="04026-160">However, using DBCC commands can be quite limiting.</span></span>  <span data-ttu-id="04026-161">Zobrazení dynamické správy (zobrazení dynamické správy) vám umožní toosee mnohem víc podrobností a také získáte mnohem větší kontrolu nad hello výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="04026-161">Dynamic management views (DMVs) will allow you toosee much more detail as well as give you much greater control over hello query results.</span></span>  <span data-ttu-id="04026-162">Začněte vytvořením toto zobrazení, která bude odkazované tooby mnoha příkladech v tomto a dalších článků.</span><span class="sxs-lookup"><span data-stu-id="04026-162">Start by creating this view, which will be referred tooby many of our examples in this and other articles.</span></span>

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a><span data-ttu-id="04026-163">Souhrnné tabulky místa</span><span class="sxs-lookup"><span data-stu-id="04026-163">Table space summary</span></span>
<span data-ttu-id="04026-164">Tento dotaz vrací řádky hello a místa v tabulce.</span><span class="sxs-lookup"><span data-stu-id="04026-164">This query returns hello rows and space by table.</span></span>  <span data-ttu-id="04026-165">Je skvělým dotazu toosee, které tabulky jsou vaše největší tabulky a zda jsou kruhové dotazování, replikovat nebo distribuovat algoritmu hash.</span><span class="sxs-lookup"><span data-stu-id="04026-165">It is a great query toosee which tables are your largest tables and whether they are round robin, replicated or hash distributed.</span></span>  <span data-ttu-id="04026-166">Pro distribuované zatřiďovacích tabulkách také ukazuje hello distribuční sloupce.</span><span class="sxs-lookup"><span data-stu-id="04026-166">For hash distributed tables it also shows hello distribution column.</span></span>  <span data-ttu-id="04026-167">Ve většině případů musí být vaše největší tabulky hash distribuované s clusterovaný index columnstore.</span><span class="sxs-lookup"><span data-stu-id="04026-167">In most cases your largest tables should be hash distributed with a clustered columnstore index.</span></span>

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a><span data-ttu-id="04026-168">Tabulka místo podle typu distribuce</span><span class="sxs-lookup"><span data-stu-id="04026-168">Table space by distribution type</span></span>
```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a><span data-ttu-id="04026-169">Tabulka místo podle indexu typu</span><span class="sxs-lookup"><span data-stu-id="04026-169">Table space by index type</span></span>
```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a><span data-ttu-id="04026-170">Souhrn distribuční prostoru</span><span class="sxs-lookup"><span data-stu-id="04026-170">Distribution space summary</span></span>
```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a><span data-ttu-id="04026-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04026-171">Next steps</span></span>
<span data-ttu-id="04026-172">články hello toolearn více, najdete na [tabulky datové typy][Data Types], [distribuci tabulku][Distribute], [indexování tabulku] [ Index], [Vytváření oddílů tabulky][Partition], [zachování statistiky tabulky] [ Statistics] a [ Dočasné tabulky][Temporary].</span><span class="sxs-lookup"><span data-stu-id="04026-172">toolearn more, see hello articles on [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition], [Maintaining Table Statistics][Statistics] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="04026-173">Další informace o osvědčených postupech najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="04026-173">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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
[Load data with Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Table Constraints]: https://msdn.microsoft.com/library/ms188066.aspx
[Computed Columns]: https://msdn.microsoft.com/library/ms186241.aspx
[Sparse Columns]: https://msdn.microsoft.com/library/cc280604.aspx
[User-Defined Types]: https://msdn.microsoft.com/library/ms131694.aspx
[Sequence]: https://msdn.microsoft.com/library/ff878091.aspx
[Triggers]: https://msdn.microsoft.com/library/ms189799.aspx
[Indexed Views]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonyms]: https://msdn.microsoft.com/library/ms177544.aspx
[Unique Indexes]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
