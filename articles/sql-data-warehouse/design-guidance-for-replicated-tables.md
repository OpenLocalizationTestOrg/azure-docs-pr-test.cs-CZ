---
title: "Návrh pokyny pro replikované tabulky – Azure SQL Data Warehouse | Microsoft Docs"
description: "Doporučení k návrhu replikovaných tabulek v Azure SQL Data Warehouse schéma."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/14/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 437a4f628a343312984d1fa2981df7fa01459e26
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a><span data-ttu-id="71359-103">Pokyny k návrhu pro používání replikovaných tabulek v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="71359-103">Design guidance for using replicated tables in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="71359-104">Tento článek obsahuje doporučení pro návrh replikovaných tabulek v SQL Data Warehouse schéma.</span><span class="sxs-lookup"><span data-stu-id="71359-104">This article gives recommendations for designing replicated tables in your SQL Data Warehouse schema.</span></span> <span data-ttu-id="71359-105">Použijte tato doporučení pro zlepšení výkonu dotazů, protože se sníží složitost dat přesouvání a dotazu.</span><span class="sxs-lookup"><span data-stu-id="71359-105">Use these recommendations to improve query performance by reducing data movement and query complexity.</span></span>

> [!NOTE]
> <span data-ttu-id="71359-106">Funkce replikované tabulky je aktuálně ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="71359-106">The replicated table feature is currently in public preview.</span></span> <span data-ttu-id="71359-107">Některé chování se může měnit.</span><span class="sxs-lookup"><span data-stu-id="71359-107">Some behaviors are subject to change.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="71359-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="71359-108">Prerequisites</span></span>
<span data-ttu-id="71359-109">Tento článek předpokládá, že jste obeznámeni s distribuci dat a koncepty přesun dat v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="71359-109">This article assumes you are familiar with data distribution and data movement concepts in SQL Data Warehouse.</span></span>  <span data-ttu-id="71359-110">Další informace najdete v tématu [Distributed data](sql-data-warehouse-distributed-data.md).</span><span class="sxs-lookup"><span data-stu-id="71359-110">For more information, see [Distributed data](sql-data-warehouse-distributed-data.md).</span></span> 

<span data-ttu-id="71359-111">Jako součást návrh tabulky Pochopte, co nejvíce o vašich dat a jak je dotazován data.</span><span class="sxs-lookup"><span data-stu-id="71359-111">As part of table design, understand as much as possible about your data and how the data is queried.</span></span>  <span data-ttu-id="71359-112">Například zvažte tyto otázky:</span><span class="sxs-lookup"><span data-stu-id="71359-112">For example, consider these questions:</span></span>

- <span data-ttu-id="71359-113">Jak velká je tabulka?</span><span class="sxs-lookup"><span data-stu-id="71359-113">How large is the table?</span></span>   
- <span data-ttu-id="71359-114">Jak často se aktualizují v tabulce?</span><span class="sxs-lookup"><span data-stu-id="71359-114">How often is the table refreshed?</span></span>   
- <span data-ttu-id="71359-115">Je nutné provést tabulkami faktů a dimenzí v datovém skladu?</span><span class="sxs-lookup"><span data-stu-id="71359-115">Do I have fact and dimension tables in a data warehouse?</span></span>   

## <a name="what-is-a-replicated-table"></a><span data-ttu-id="71359-116">Co je replikované tabulky?</span><span class="sxs-lookup"><span data-stu-id="71359-116">What is a replicated table?</span></span>
<span data-ttu-id="71359-117">Replikované tabulky obsahuje úplnou kopii v tabulce, která je přístupná na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="71359-117">A replicated table has a full copy of the table accessible on each Compute node.</span></span> <span data-ttu-id="71359-118">Replikace tabulku eliminuje nutnost k přenosu dat mezi výpočetní uzly před spojení nebo agregace.</span><span class="sxs-lookup"><span data-stu-id="71359-118">Replicating a table removes the need to transfer data among Compute nodes before a join or aggregation.</span></span> <span data-ttu-id="71359-119">Vzhledem k tomu, že tabulka obsahuje více kopií, replikované tabulky fungují lépe, když velikost tabulky je menší než 2 GB komprimované.</span><span class="sxs-lookup"><span data-stu-id="71359-119">Since the table has multiple copies, replicated tables work best when the table size is less than 2 GB compressed.</span></span>

<span data-ttu-id="71359-120">Následující diagram znázorňuje replikované tabulky, která je přístupná na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="71359-120">The following diagram shows a replicated table that is accessible on each Compute node.</span></span> <span data-ttu-id="71359-121">V SQL Data Warehouse replikované tabulce zkopírován plně distribuční databázi na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="71359-121">In SQL Data Warehouse, the replicated table is fully copied to a distribution database on each Compute node.</span></span> 

<span data-ttu-id="71359-122">![Replikované tabulky](media/guidance-for-using-replicated-tables/replicated-table.png "replikované tabulky")</span><span class="sxs-lookup"><span data-stu-id="71359-122">![Replicated table](media/guidance-for-using-replicated-tables/replicated-table.png "Replicated table")</span></span>  

<span data-ttu-id="71359-123">Replikované tabulky pracovní i pro malý dimenze tabulky v hvězdicové schéma.</span><span class="sxs-lookup"><span data-stu-id="71359-123">Replicated tables work well for small dimension tables in a star schema.</span></span> <span data-ttu-id="71359-124">Tabulky dimenzí jsou obvykle velikosti, která je vhodná k uložení a správě více kopií.</span><span class="sxs-lookup"><span data-stu-id="71359-124">Dimension tables are usually of a size that makes it feasible to store and maintain multiple copies.</span></span> <span data-ttu-id="71359-125">Dimenze ukládat popisný data, která změní pomalu, jako je například jméno zákazníka a adresu a podrobnosti o produktu.</span><span class="sxs-lookup"><span data-stu-id="71359-125">Dimensions store descriptive data that changes slowly, such as customer name and address, and product details.</span></span> <span data-ttu-id="71359-126">Pomalu se měnící se povaze dat vede k méně znovu sestaví replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-126">The slowly changing nature of the data leads to fewer rebuilds of the replicated table.</span></span> 

<span data-ttu-id="71359-127">Zvažte použití replikované tabulky, když:</span><span class="sxs-lookup"><span data-stu-id="71359-127">Consider using a replicated table when:</span></span>

- <span data-ttu-id="71359-128">Velikost tabulky na disku je menší než 2 GB, bez ohledu na počet řádků.</span><span class="sxs-lookup"><span data-stu-id="71359-128">The table size on disk is less than 2 GB, regardless of the number of rows.</span></span> <span data-ttu-id="71359-129">Chcete-li zjistit velikost tabulky, můžete použít [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) příkaz: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`.</span><span class="sxs-lookup"><span data-stu-id="71359-129">To find the size of a table, you can use the [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) command: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`.</span></span> 
- <span data-ttu-id="71359-130">Tabulka se používá ve spojení, které by jinak vyžadovaly přesun dat.</span><span class="sxs-lookup"><span data-stu-id="71359-130">The table is used in joins that would otherwise require data movement.</span></span> <span data-ttu-id="71359-131">Například připojení k síti na distribuovat algoritmu hash tabulky vyžaduje přesun dat, pokud spojující sloupce nejsou na stejný sloupec distribuční.</span><span class="sxs-lookup"><span data-stu-id="71359-131">For example, a join on hash-distributed tables requires data movement when the joining columns are not the same distribution column.</span></span> <span data-ttu-id="71359-132">Pokud jeden z tabulky distribuovat algoritmu hash je malý, vezměte v úvahu replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-132">If one of the hash-distributed tables is small, consider a replicated table.</span></span> <span data-ttu-id="71359-133">Spojení v tabulce kruhového dotazování vyžaduje přesun dat.</span><span class="sxs-lookup"><span data-stu-id="71359-133">A join on a round-robin table requires data movement.</span></span> <span data-ttu-id="71359-134">Doporučujeme používat replikované tabulky místo kruhového dotazování tabulky ve většině případů.</span><span class="sxs-lookup"><span data-stu-id="71359-134">We recommend using replicated tables instead of round-robin tables in most cases.</span></span> 


<span data-ttu-id="71359-135">Vezměte v úvahu převod existující distribuované tabulka, která se replikované tabulky, když:</span><span class="sxs-lookup"><span data-stu-id="71359-135">Consider converting an existing distributed table to a replicated table when:</span></span>

- <span data-ttu-id="71359-136">Dotaz plány pomocí operace přesunu dat, které vysílají data na výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="71359-136">Query plans use data movement operations that broadcast the data to all the Compute nodes.</span></span> <span data-ttu-id="71359-137">BroadcastMoveOperation je nákladné a zpomalí výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="71359-137">The BroadcastMoveOperation is expensive and slows query performance.</span></span> <span data-ttu-id="71359-138">Chcete-li zobrazit operace přesunu dat v plány dotazů, použijte [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="71359-138">To view data movement operations in query plans, use [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).</span></span>
 
<span data-ttu-id="71359-139">Replikované tabulky nemusí poskytne nejlepší výkon dotazů při:</span><span class="sxs-lookup"><span data-stu-id="71359-139">Replicated tables may not yield the best query performance when:</span></span>

- <span data-ttu-id="71359-140">Tabulka obsahuje časté vložit, aktualizovat a odstraňovat operace.</span><span class="sxs-lookup"><span data-stu-id="71359-140">The table has frequent insert, update, and delete operations.</span></span> <span data-ttu-id="71359-141">Tyto operace jazyk (DML) manipulaci dat vyžadují opětovné sestavení replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-141">These data manipulation language (DML) operations require a rebuild of the replicated table.</span></span> <span data-ttu-id="71359-142">Opětovné sestavení často může způsobit snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="71359-142">Rebuilding frequently can cause slower performance.</span></span>
- <span data-ttu-id="71359-143">Datový sklad je často škálovat.</span><span class="sxs-lookup"><span data-stu-id="71359-143">The data warehouse is scaled frequently.</span></span> <span data-ttu-id="71359-144">Změna měřítka datového skladu změní počet výpočetních uzlů, který způsobuje opětovném sestavení.</span><span class="sxs-lookup"><span data-stu-id="71359-144">Scaling a data warehouse changes the number of Compute nodes, which incurs a rebuild.</span></span>
- <span data-ttu-id="71359-145">Tabulka obsahuje velký počet sloupců, ale operace dat obvykle přístup pouze malý počet sloupců.</span><span class="sxs-lookup"><span data-stu-id="71359-145">The table has a large number of columns, but data operations typically access only a small number of columns.</span></span> <span data-ttu-id="71359-146">V tomto scénáři, namísto replikace celou tabulku může být více platné pro hodnoty hash distribuovat v tabulce a pak vytvořit index pro často používaná sloupce.</span><span class="sxs-lookup"><span data-stu-id="71359-146">In this scenario, instead of replicating the entire table, it might be more effective to hash distribute the table, and then create an index on the frequently accessed columns.</span></span> <span data-ttu-id="71359-147">Pokud dotaz vyžaduje přesun dat, SQL Data Warehouse pouze přesouvá data v požadované sloupce.</span><span class="sxs-lookup"><span data-stu-id="71359-147">When a query requires data movement, SQL Data Warehouse only moves data in the requested columns.</span></span> 



## <a name="use-replicated-tables-with-simple-query-predicates"></a><span data-ttu-id="71359-148">Použití replikované tabulky s predikáty jednoduchý dotaz</span><span class="sxs-lookup"><span data-stu-id="71359-148">Use replicated tables with simple query predicates</span></span>
<span data-ttu-id="71359-149">Než rozhodnete distribuovat nebo replikovat tabulku, vezměte v úvahu typy dotazů, které máte v úmyslu provést na tabulce.</span><span class="sxs-lookup"><span data-stu-id="71359-149">Before you choose to distribute or replicate a table, think about the types of queries you plan to run against the table.</span></span> <span data-ttu-id="71359-150">Pokud je to možné,</span><span class="sxs-lookup"><span data-stu-id="71359-150">Whenever possible,</span></span>

- <span data-ttu-id="71359-151">Pro dotazy s predikáty jednoduchý dotaz, jako je například rovnosti nebo nerovnosti použijte replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-151">Use replicated tables for queries with simple query predicates, such as equality or inequality.</span></span>
- <span data-ttu-id="71359-152">Pro dotazy s predikáty složitý dotaz, jako je například jako použijte distribuované tabulky nebo není jako.</span><span class="sxs-lookup"><span data-stu-id="71359-152">Use distributed tables for queries with complex query predicates, such as LIKE or NOT LIKE.</span></span>

<span data-ttu-id="71359-153">Náročná na prostředky procesoru dotazů provést nejlépe při práci se distribuuje do všech výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="71359-153">CPU-intensive queries perform best when the work is distributed across all of the Compute nodes.</span></span> <span data-ttu-id="71359-154">Například dotazy, které běží výpočtů na každý řádek tabulky poskytují lepší výkon v distribuované tabulek než replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-154">For example, queries that run computations on each row of a table perform better on distributed tables than replicated tables.</span></span> <span data-ttu-id="71359-155">Vzhledem k tomu, že replikované tabulky je uložený ve plně na každém výpočetním uzlu, náročná na prostředky procesoru dotazy na replikované tabulky spouští celou tabulku na každý uzel výpočty.</span><span class="sxs-lookup"><span data-stu-id="71359-155">Since a replicated table is stored in full on each Compute node, a CPU-intensive query against a replicated table runs against the entire table on every Compute node.</span></span> <span data-ttu-id="71359-156">Navíc výpočet můžou způsobit snížení výkonnosti dotazu.</span><span class="sxs-lookup"><span data-stu-id="71359-156">The extra computation can slow query performance.</span></span>

<span data-ttu-id="71359-157">Tento dotaz má například komplexní predikátu.</span><span class="sxs-lookup"><span data-stu-id="71359-157">For example, this query has a complex predicate.</span></span>  <span data-ttu-id="71359-158">Rychleji spustí po dodavatele distribuované tabulku místo replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-158">It runs faster when supplier is a distributed table instead of a replicated table.</span></span> <span data-ttu-id="71359-159">V tomto příkladu dodavatele lze distribuovat algoritmu hash nebo distribuované kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="71359-159">In this example, supplier can be hash-distributed or round-robin distributed.</span></span>

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-to-replicated-tables"></a><span data-ttu-id="71359-160">Převést stávající tabulky pomocí kruhového dotazování na replikované tabulky</span><span class="sxs-lookup"><span data-stu-id="71359-160">Convert existing round-robin tables to replicated tables</span></span>
<span data-ttu-id="71359-161">Pokud již máte kruhového dotazování tabulky, doporučujeme převodu na replikované tabulky, pokud splňují s kritérii uvedených v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="71359-161">If you already have round-robin tables, we recommend converting them to replicated tables if they meet with criteria outlined in this article.</span></span> <span data-ttu-id="71359-162">Replikované tabulky zlepšit výkon na kruhového dotazování tabulky, protože se vyhnete nutnosti pro přesun dat.</span><span class="sxs-lookup"><span data-stu-id="71359-162">Replicated tables improve performance over round-robin tables because they eliminate the need for data movement.</span></span>  <span data-ttu-id="71359-163">Přesun dat pomocí kruhového dotazování tabulky vždy vyžaduje pro spojení.</span><span class="sxs-lookup"><span data-stu-id="71359-163">A round-robin table always requires data movement for joins.</span></span> 

<span data-ttu-id="71359-164">Tento příklad používá [funkce CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) ke změně v tabulce DimSalesTerritory replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-164">This example uses [CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) to change the DimSalesTerritory table to a replicated table.</span></span> <span data-ttu-id="71359-165">Tento příklad funguje bez ohledu na to, jestli je DimSalesTerritory distribuovat algoritmu hash nebo kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="71359-165">This example works regardless of whether DimSalesTerritory is hash-distributed or round-robin.</span></span>

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

--Create statistics on new table
CREATE STATISTICS [SalesTerritoryKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryKey]);
CREATE STATISTICS [SalesTerritoryAlternateKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryAlternateKey]);
CREATE STATISTICS [SalesTerritoryRegion] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryRegion]);
CREATE STATISTICS [SalesTerritoryCountry] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryCountry]);
CREATE STATISTICS [SalesTerritoryGroup] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryGroup]);

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a><span data-ttu-id="71359-166">Příklad výkonu dotazu, kruhové dotazování versus replikovat</span><span class="sxs-lookup"><span data-stu-id="71359-166">Query performance example for round-robin versus replicated</span></span> 

<span data-ttu-id="71359-167">Replikované tabulky nevyžaduje žádné přesun dat pro spojení, protože celá tabulka je již na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="71359-167">A replicated table does not require any data movement for joins because the entire table is already present on each Compute node.</span></span> <span data-ttu-id="71359-168">Pokud tabulky dimenzí distribuované kruhového dotazování, a připojte zkopíruje tabulce dimenze v plném rozsahu na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="71359-168">If the dimension tables are round-robin distributed, a join copies the dimension table in full to each Compute node.</span></span> <span data-ttu-id="71359-169">Plán dotazu pro přesun dat, obsahuje operace názvem BroadcastMoveOperation.</span><span class="sxs-lookup"><span data-stu-id="71359-169">To move the data, the query plan contains an operation called BroadcastMoveOperation.</span></span> <span data-ttu-id="71359-170">Tento typ operace přesunu dat zpomalí výkon dotazů a je eliminována použití replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-170">This type of data movement operation slows query performance and is eliminated by using replicated tables.</span></span> <span data-ttu-id="71359-171">Chcete-li zobrazit kroky plán dotazu, použijte [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) zobrazení katalogu systému.</span><span class="sxs-lookup"><span data-stu-id="71359-171">To view query plan steps, use the [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) system catalog view.</span></span> 

<span data-ttu-id="71359-172">Například v následujícím dotazu pro schéma AdventureWorks ` FactInternetSales` tabulka je distribuovat algoritmu hash.</span><span class="sxs-lookup"><span data-stu-id="71359-172">For example, in following query against the AdventureWorks schema, the ` FactInternetSales` table is hash-distributed.</span></span> <span data-ttu-id="71359-173">`DimDate` a `DimSalesTerritory` tabulky jsou menší tabulky dimenzí.</span><span class="sxs-lookup"><span data-stu-id="71359-173">The `DimDate` and `DimSalesTerritory` tables are smaller dimension tables.</span></span> <span data-ttu-id="71359-174">Tento dotaz vrátí celkový prodej v Severní Americe pro fiskálního roku 2004:</span><span class="sxs-lookup"><span data-stu-id="71359-174">This query returns the total sales in North America for fiscal year 2004:</span></span>
 
```sql
SELECT [TotalSalesAmount] = SUM(SalesAmount)
FROM dbo.FactInternetSales s
INNER JOIN dbo.DimDate d
  ON d.DateKey = s.OrderDateKey
INNER JOIN dbo.DimSalesTerritory t
  ON t.SalesTerritoryKey = s.SalesTerritoryKey
WHERE d.FiscalYear = 2004
  AND t.SalesTerritoryGroup = 'North America'
```
<span data-ttu-id="71359-175">Znovu vytvořit `DimDate` a `DimSalesTerritory` jako kruhového dotazování tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-175">We re-created `DimDate` and `DimSalesTerritory` as round-robin tables.</span></span> <span data-ttu-id="71359-176">V důsledku toho dotaz vám ukázal následující plán dotazu, který má více vysílání přesunout operace:</span><span class="sxs-lookup"><span data-stu-id="71359-176">As a result, the query showed the following query plan, which has multiple broadcast move operations:</span></span> 
 
![Plán dotazu pomocí kruhového dotazování](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

<span data-ttu-id="71359-178">Znovu vytvořit `DimDate` a `DimSalesTerritory` jako replikovaných tabulek a znovu se spustil dotaz.</span><span class="sxs-lookup"><span data-stu-id="71359-178">We re-created `DimDate` and `DimSalesTerritory` as replicated tables, and ran the query again.</span></span> <span data-ttu-id="71359-179">Výsledný plán dotazu je mnohem kratší a mají některé nevysílá přesune.</span><span class="sxs-lookup"><span data-stu-id="71359-179">The resulting query plan is much shorter and does not have any broadcast moves.</span></span>

![Replikovat plán dotazu](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a><span data-ttu-id="71359-181">Důležité informace o výkonu pro úpravy replikované tabulky</span><span class="sxs-lookup"><span data-stu-id="71359-181">Performance considerations for modifying replicated tables</span></span>
<span data-ttu-id="71359-182">SQL Data Warehouse implementuje replikované tabulky udržováním hlavní verze tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-182">SQL Data Warehouse implements a replicated table by maintaining a master version of the table.</span></span> <span data-ttu-id="71359-183">Hlavní verze se zkopíruje na jeden distribuční databázi na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="71359-183">It copies the master version to one distribution database on each Compute node.</span></span> <span data-ttu-id="71359-184">Když dojde ke změně, aktualizuje SQL Data Warehouse nejprve hlavní tabulka.</span><span class="sxs-lookup"><span data-stu-id="71359-184">When there is a change, SQL Data Warehouse first updates the master table.</span></span> <span data-ttu-id="71359-185">Potom vyžaduje nové vytvoření tabulky na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="71359-185">Then it requires a rebuild of the tables on each Compute node.</span></span> <span data-ttu-id="71359-186">Opětovné sestavení replikované tabulky zahrnuje kopírování v tabulce na každém výpočetním uzlu a pak znovu sestavit indexy.</span><span class="sxs-lookup"><span data-stu-id="71359-186">A rebuild of a replicated table includes copying the table to each Compute node and then rebuilding the indexes.</span></span>

<span data-ttu-id="71359-187">Znovu sestaví je potřeba po:</span><span class="sxs-lookup"><span data-stu-id="71359-187">Rebuilds are required after:</span></span>
- <span data-ttu-id="71359-188">Data jsou načíst nebo upravit</span><span class="sxs-lookup"><span data-stu-id="71359-188">Data is loaded or modified</span></span>
- <span data-ttu-id="71359-189">Datový sklad je škálovat na jiné nastavení DWU</span><span class="sxs-lookup"><span data-stu-id="71359-189">The data warehouse is scaled to a different DWU setting</span></span>
- <span data-ttu-id="71359-190">Aktualizace definice tabulky</span><span class="sxs-lookup"><span data-stu-id="71359-190">Table definition is updated</span></span>

<span data-ttu-id="71359-191">Znovu sestaví nejsou nutné po:</span><span class="sxs-lookup"><span data-stu-id="71359-191">Rebuilds are not required after:</span></span>
- <span data-ttu-id="71359-192">Operace pozastavení</span><span class="sxs-lookup"><span data-stu-id="71359-192">Pause operation</span></span>
- <span data-ttu-id="71359-193">Operace obnovení</span><span class="sxs-lookup"><span data-stu-id="71359-193">Resume operation</span></span>

<span data-ttu-id="71359-194">Sestavení se neodehrává ihned po data je upravit.</span><span class="sxs-lookup"><span data-stu-id="71359-194">The rebuild does not happen immediately after data is modified.</span></span> <span data-ttu-id="71359-195">Místo toho sestavení se aktivuje při prvním dotazu vybere z tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-195">Instead, the rebuild is triggered the first time a query selects from the table.</span></span>  <span data-ttu-id="71359-196">V rámci počáteční příkaz select z tabulky jsou kroky pro opětovné sestavení replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-196">Within the initial select statement from the table are steps to rebuild the replicated table.</span></span>  <span data-ttu-id="71359-197">Protože sestavení se provádí v dotazu, může být důležité v závislosti na velikosti tabulky dopad na počáteční příkaz select.</span><span class="sxs-lookup"><span data-stu-id="71359-197">Because the rebuild is done within the query, the impact to the initial select statement could be significant depending on the size of the table.</span></span>  <span data-ttu-id="71359-198">Pokud více replikované tabulky se podílejí vyžadující opětovném sestavení, každá kopie je znovu sestavit sériově jako kroky v rámci příkazu.</span><span class="sxs-lookup"><span data-stu-id="71359-198">If multiple replicated tables are involved that need a rebuild, each copy is rebuilt serially as steps within the statement.</span></span>  <span data-ttu-id="71359-199">Chcete-li zachovat data konzistence během sestavení replikované tabulky výhradní zámek pořízené v tabulce.</span><span class="sxs-lookup"><span data-stu-id="71359-199">To maintain data consistency during the rebuild of the replicated table an exclusive lock is taken on the table.</span></span>  <span data-ttu-id="71359-200">Zámek zabraňuje veškerý přístup k tabulce po dobu trvání sestavení.</span><span class="sxs-lookup"><span data-stu-id="71359-200">The lock prevents all access to the table for the duration of the rebuild.</span></span> 

### <a name="use-indexes-conservatively"></a><span data-ttu-id="71359-201">Můžete použít indexy</span><span class="sxs-lookup"><span data-stu-id="71359-201">Use indexes conservatively</span></span>
<span data-ttu-id="71359-202">Standardní indexování postupy platí pro replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="71359-202">Standard indexing practices apply to replicated tables.</span></span> <span data-ttu-id="71359-203">SQL Data Warehouse znovu sestaví každý index replikované tabulky v rámci sestavení.</span><span class="sxs-lookup"><span data-stu-id="71359-203">SQL Data Warehouse rebuilds each replicated table index as part of the rebuild.</span></span> <span data-ttu-id="71359-204">Indexy používejte pouze výkonnější převáží náklady znovu sestavit indexy.</span><span class="sxs-lookup"><span data-stu-id="71359-204">Only use indexes when the performance gain outweighs the cost of rebuilding the indexes.</span></span>  
 
### <a name="batch-data-loads"></a><span data-ttu-id="71359-205">Načítání dat dávky</span><span class="sxs-lookup"><span data-stu-id="71359-205">Batch data loads</span></span>
<span data-ttu-id="71359-206">Při načítání dat do replikované tabulky, měli snažte minimalizovat znovu sestaví podle dávkování zatížení dohromady.</span><span class="sxs-lookup"><span data-stu-id="71359-206">When loading data into replicated tables, try to minimize rebuilds by batching loads together.</span></span> <span data-ttu-id="71359-207">Proveďte všechny dávkové zatížení před spuštěním příkazů select.</span><span class="sxs-lookup"><span data-stu-id="71359-207">Perform all the batched loads before running select statements.</span></span>

<span data-ttu-id="71359-208">Tento vzor zatížení například načte data ze čtyř zdrojů a vyvolá čtyři znovu sestaví.</span><span class="sxs-lookup"><span data-stu-id="71359-208">For example, this load pattern loads data from four sources and invokes four rebuilds.</span></span> 

- <span data-ttu-id="71359-209">Načíst ze zdroje 1.</span><span class="sxs-lookup"><span data-stu-id="71359-209">Load from source 1.</span></span>
- <span data-ttu-id="71359-210">Příkaz SELECT aktivační události znovu sestavit 1.</span><span class="sxs-lookup"><span data-stu-id="71359-210">Select statement triggers rebuild 1.</span></span>
- <span data-ttu-id="71359-211">Načíst ze zdroje. 2.</span><span class="sxs-lookup"><span data-stu-id="71359-211">Load from source 2.</span></span>
- <span data-ttu-id="71359-212">Příkaz SELECT aktivační události znovu sestavit 2.</span><span class="sxs-lookup"><span data-stu-id="71359-212">Select statement triggers rebuild 2.</span></span>
- <span data-ttu-id="71359-213">Načíst ze zdroje 3.</span><span class="sxs-lookup"><span data-stu-id="71359-213">Load from source 3.</span></span>
- <span data-ttu-id="71359-214">Příkaz SELECT aktivační události znovu sestavit 3.</span><span class="sxs-lookup"><span data-stu-id="71359-214">Select statement triggers rebuild 3.</span></span>
- <span data-ttu-id="71359-215">Načíst ze zdroje 4.</span><span class="sxs-lookup"><span data-stu-id="71359-215">Load from source 4.</span></span>
- <span data-ttu-id="71359-216">Příkaz SELECT aktivační události znovu sestavit 4.</span><span class="sxs-lookup"><span data-stu-id="71359-216">Select statement triggers rebuild 4.</span></span>

<span data-ttu-id="71359-217">Tento vzor zatížení například načte data ze čtyř zdrojů, ale pouze vyvolá jeden opětovné sestavení.</span><span class="sxs-lookup"><span data-stu-id="71359-217">For example, this load pattern loads data from four sources, but only invokes one rebuild.</span></span>

- <span data-ttu-id="71359-218">Načíst ze zdroje 1.</span><span class="sxs-lookup"><span data-stu-id="71359-218">Load from source 1.</span></span>
- <span data-ttu-id="71359-219">Načíst ze zdroje. 2.</span><span class="sxs-lookup"><span data-stu-id="71359-219">Load from source 2.</span></span>
- <span data-ttu-id="71359-220">Načíst ze zdroje 3.</span><span class="sxs-lookup"><span data-stu-id="71359-220">Load from source 3.</span></span>
- <span data-ttu-id="71359-221">Načíst ze zdroje 4.</span><span class="sxs-lookup"><span data-stu-id="71359-221">Load from source 4.</span></span>
- <span data-ttu-id="71359-222">Příkaz SELECT aktivační události znovu sestavit.</span><span class="sxs-lookup"><span data-stu-id="71359-222">Select statement triggers rebuild.</span></span>


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a><span data-ttu-id="71359-223">Znovu sestavte replikované tabulky po zatížení batch</span><span class="sxs-lookup"><span data-stu-id="71359-223">Rebuild a replicated table after a batch load</span></span>
<span data-ttu-id="71359-224">Aby dobu provádění konzistentní dotazy, doporučujeme vynutit aktualizaci replikované tabulky po batch zatížení.</span><span class="sxs-lookup"><span data-stu-id="71359-224">To ensure consistent query execution times, we recommend forcing a refresh of the replicated tables after a batch load.</span></span> <span data-ttu-id="71359-225">Jinak hodnota první dotaz musí počkat tabulky, které chcete aktualizovat, která zahrnuje nové sestavení indexů.</span><span class="sxs-lookup"><span data-stu-id="71359-225">Otherwise, the first query must wait for the tables to refresh, which includes rebuilding the indexes.</span></span> <span data-ttu-id="71359-226">V závislosti na velikosti a počtu replikované tabulky vliv může být významný dopad na výkon.</span><span class="sxs-lookup"><span data-stu-id="71359-226">Depending on the size and number of replicated tables affected, the performance impact can be significant.</span></span>  

<span data-ttu-id="71359-227">Tento dotaz používá [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV seznam replikované tabulky, která byla upravena, ale není znovu sestavit.</span><span class="sxs-lookup"><span data-stu-id="71359-227">This query uses the [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV to list the replicated tables that have been modified, but not rebuilt.</span></span>

```sql 
SELECT [ReplicatedTable] = t.[name]
  FROM sys.tables t  
  JOIN sys.pdw_replicated_table_cache_state c  
    ON c.object_id = t.object_id 
  JOIN sys.pdw_table_distribution_properties p 
    ON p.object_id = t.object_id 
  WHERE c.[state] = 'NotReady'
    AND p.[distribution_policy_desc] = 'REPLICATE'
```
 
<span data-ttu-id="71359-228">Chcete-li vynutit opětovném sestavení, spustíte následující příkaz pro každou tabulku v předchozím výstup.</span><span class="sxs-lookup"><span data-stu-id="71359-228">To force a rebuild, run the following statement on each table in the preceding output.</span></span> 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a><span data-ttu-id="71359-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71359-229">Next steps</span></span> 
<span data-ttu-id="71359-230">Pokud chcete vytvořit replikované tabulky, použijte jednu z těchto příkazů:</span><span class="sxs-lookup"><span data-stu-id="71359-230">To create a replicated table, use one of these statements:</span></span>

- [<span data-ttu-id="71359-231">Vytvoření tabulky (Azure SQL Data Warehouse)</span><span class="sxs-lookup"><span data-stu-id="71359-231">CREATE TABLE (Azure SQL Data Warehouse)</span></span>](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [<span data-ttu-id="71359-232">Vytvoření TABLE AS SELECT (Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="71359-232">CREATE TABLE AS SELECT (Azure SQL Data Warehouse</span></span>](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

<span data-ttu-id="71359-233">Přehled distribuované tabulek, najdete v tématu [distribuované tabulky](sql-data-warehouse-tables-distribute.md).</span><span class="sxs-lookup"><span data-stu-id="71359-233">For an overview of distributed tables, see [distributed tables](sql-data-warehouse-tables-distribute.md).</span></span>



