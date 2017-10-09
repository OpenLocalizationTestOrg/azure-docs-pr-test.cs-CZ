---
title: "pokyny, aaaDesign replikovaných tabulek – Azure SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 5d405b8c404c65177b387ba959126839c1cf8799
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a><span data-ttu-id="926e0-103">Pokyny k návrhu pro používání replikovaných tabulek v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="926e0-103">Design guidance for using replicated tables in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="926e0-104">Tento článek obsahuje doporučení pro návrh replikovaných tabulek v SQL Data Warehouse schéma.</span><span class="sxs-lookup"><span data-stu-id="926e0-104">This article gives recommendations for designing replicated tables in your SQL Data Warehouse schema.</span></span> <span data-ttu-id="926e0-105">Použijte tyto výkon dotazů tooimprove doporučení pomocí snižuje složitost dat přesouvání a dotazu.</span><span class="sxs-lookup"><span data-stu-id="926e0-105">Use these recommendations tooimprove query performance by reducing data movement and query complexity.</span></span>

> [!NOTE]
> <span data-ttu-id="926e0-106">Funkce replikované tabulky Hello je aktuálně ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="926e0-106">hello replicated table feature is currently in public preview.</span></span> <span data-ttu-id="926e0-107">Některé chování jsou toochange subjektu.</span><span class="sxs-lookup"><span data-stu-id="926e0-107">Some behaviors are subject toochange.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="926e0-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="926e0-108">Prerequisites</span></span>
<span data-ttu-id="926e0-109">Tento článek předpokládá, že jste obeznámeni s distribuci dat a koncepty přesun dat v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="926e0-109">This article assumes you are familiar with data distribution and data movement concepts in SQL Data Warehouse.</span></span>  <span data-ttu-id="926e0-110">Další informace najdete v tématu [Distributed data](sql-data-warehouse-distributed-data.md).</span><span class="sxs-lookup"><span data-stu-id="926e0-110">For more information, see [Distributed data](sql-data-warehouse-distributed-data.md).</span></span> 

<span data-ttu-id="926e0-111">Jako součást návrh tabulky Pochopte, co nejvíce o vašich dat a jak je dotazován hello data.</span><span class="sxs-lookup"><span data-stu-id="926e0-111">As part of table design, understand as much as possible about your data and how hello data is queried.</span></span>  <span data-ttu-id="926e0-112">Například zvažte tyto otázky:</span><span class="sxs-lookup"><span data-stu-id="926e0-112">For example, consider these questions:</span></span>

- <span data-ttu-id="926e0-113">Jak velká je tabulka hello?</span><span class="sxs-lookup"><span data-stu-id="926e0-113">How large is hello table?</span></span>   
- <span data-ttu-id="926e0-114">Jak často se aktualizují hello tabulky?</span><span class="sxs-lookup"><span data-stu-id="926e0-114">How often is hello table refreshed?</span></span>   
- <span data-ttu-id="926e0-115">Je nutné provést tabulkami faktů a dimenzí v datovém skladu?</span><span class="sxs-lookup"><span data-stu-id="926e0-115">Do I have fact and dimension tables in a data warehouse?</span></span>   

## <a name="what-is-a-replicated-table"></a><span data-ttu-id="926e0-116">Co je replikované tabulky?</span><span class="sxs-lookup"><span data-stu-id="926e0-116">What is a replicated table?</span></span>
<span data-ttu-id="926e0-117">Replikované tabulky obsahuje úplnou kopii hello tabulky, které jsou přístupné na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="926e0-117">A replicated table has a full copy of hello table accessible on each Compute node.</span></span> <span data-ttu-id="926e0-118">Replikace tabulku odebere hello nutné tootransfer data mezi výpočetní uzly před spojení nebo agregace.</span><span class="sxs-lookup"><span data-stu-id="926e0-118">Replicating a table removes hello need tootransfer data among Compute nodes before a join or aggregation.</span></span> <span data-ttu-id="926e0-119">Protože hello tabulka obsahuje více kopií, replikované tabulky fungují lépe, když velikost tabulky hello je menší než 2 GB komprimované.</span><span class="sxs-lookup"><span data-stu-id="926e0-119">Since hello table has multiple copies, replicated tables work best when hello table size is less than 2 GB compressed.</span></span>

<span data-ttu-id="926e0-120">Hello následující diagram znázorňuje replikované tabulky, která je přístupná na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="926e0-120">hello following diagram shows a replicated table that is accessible on each Compute node.</span></span> <span data-ttu-id="926e0-121">Hello replikované tabulky v SQL Data Warehouse je plně zkopírovaný tooa distribuční databázi na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="926e0-121">In SQL Data Warehouse, hello replicated table is fully copied tooa distribution database on each Compute node.</span></span> 

<span data-ttu-id="926e0-122">![Replikované tabulky](media/guidance-for-using-replicated-tables/replicated-table.png "replikované tabulky")</span><span class="sxs-lookup"><span data-stu-id="926e0-122">![Replicated table](media/guidance-for-using-replicated-tables/replicated-table.png "Replicated table")</span></span>  

<span data-ttu-id="926e0-123">Replikované tabulky pracovní i pro malý dimenze tabulky v hvězdicové schéma.</span><span class="sxs-lookup"><span data-stu-id="926e0-123">Replicated tables work well for small dimension tables in a star schema.</span></span> <span data-ttu-id="926e0-124">Tabulky dimenzí obvykle jsou velikosti, která je vhodná toostore a udržovat více kopií.</span><span class="sxs-lookup"><span data-stu-id="926e0-124">Dimension tables are usually of a size that makes it feasible toostore and maintain multiple copies.</span></span> <span data-ttu-id="926e0-125">Dimenze ukládat popisný data, která změní pomalu, jako je například jméno zákazníka a adresu a podrobnosti o produktu.</span><span class="sxs-lookup"><span data-stu-id="926e0-125">Dimensions store descriptive data that changes slowly, such as customer name and address, and product details.</span></span> <span data-ttu-id="926e0-126">Hello pomalu změna povaha hello dat vede znovu sestaví toofewer hello replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="926e0-126">hello slowly changing nature of hello data leads toofewer rebuilds of hello replicated table.</span></span> 

<span data-ttu-id="926e0-127">Zvažte použití replikované tabulky, když:</span><span class="sxs-lookup"><span data-stu-id="926e0-127">Consider using a replicated table when:</span></span>

- <span data-ttu-id="926e0-128">velikost Hello tabulky na disku je menší než 2 GB, bez ohledu na to hello počet řádků.</span><span class="sxs-lookup"><span data-stu-id="926e0-128">hello table size on disk is less than 2 GB, regardless of hello number of rows.</span></span> <span data-ttu-id="926e0-129">velikost hello toofind tabulky, můžete použít hello [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) příkaz: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`.</span><span class="sxs-lookup"><span data-stu-id="926e0-129">toofind hello size of a table, you can use hello [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) command: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`.</span></span> 
- <span data-ttu-id="926e0-130">Tabulka Hello se používá ve spojení, které by jinak vyžadovaly přesun dat.</span><span class="sxs-lookup"><span data-stu-id="926e0-130">hello table is used in joins that would otherwise require data movement.</span></span> <span data-ttu-id="926e0-131">Například připojení k síti na distribuovat algoritmu hash tabulky při hello spojující sloupce nejsou hello stejný sloupec distribuční vyžaduje přesun dat.</span><span class="sxs-lookup"><span data-stu-id="926e0-131">For example, a join on hash-distributed tables requires data movement when hello joining columns are not hello same distribution column.</span></span> <span data-ttu-id="926e0-132">Pokud jeden z tabulky distribuovat algoritmu hash hello je malý, vezměte v úvahu replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="926e0-132">If one of hello hash-distributed tables is small, consider a replicated table.</span></span> <span data-ttu-id="926e0-133">Spojení v tabulce kruhového dotazování vyžaduje přesun dat.</span><span class="sxs-lookup"><span data-stu-id="926e0-133">A join on a round-robin table requires data movement.</span></span> <span data-ttu-id="926e0-134">Doporučujeme používat replikované tabulky místo kruhového dotazování tabulky ve většině případů.</span><span class="sxs-lookup"><span data-stu-id="926e0-134">We recommend using replicated tables instead of round-robin tables in most cases.</span></span> 


<span data-ttu-id="926e0-135">Vezměte v úvahu převod existující distribuované tabulky tooa replikované tabulky, když:</span><span class="sxs-lookup"><span data-stu-id="926e0-135">Consider converting an existing distributed table tooa replicated table when:</span></span>

- <span data-ttu-id="926e0-136">Dotaz plány pomocí operace přesunu dat, které vysílají hello data tooall hello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="926e0-136">Query plans use data movement operations that broadcast hello data tooall hello Compute nodes.</span></span> <span data-ttu-id="926e0-137">Hello BroadcastMoveOperation je nákladné a zpomalí výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="926e0-137">hello BroadcastMoveOperation is expensive and slows query performance.</span></span> <span data-ttu-id="926e0-138">operace přesunu dat tooview v plánech dotaz, použít [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="926e0-138">tooview data movement operations in query plans, use [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).</span></span>
 
<span data-ttu-id="926e0-139">Replikované tabulky nemusí poskytne nejlepší výkon dotazů hello při:</span><span class="sxs-lookup"><span data-stu-id="926e0-139">Replicated tables may not yield hello best query performance when:</span></span>

- <span data-ttu-id="926e0-140">Tabulka Hello má časté vložit, aktualizovat a odstraňovat operace.</span><span class="sxs-lookup"><span data-stu-id="926e0-140">hello table has frequent insert, update, and delete operations.</span></span> <span data-ttu-id="926e0-141">Tyto operace jazyk (DML) manipulaci dat vyžadují opětovné sestavení hello replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="926e0-141">These data manipulation language (DML) operations require a rebuild of hello replicated table.</span></span> <span data-ttu-id="926e0-142">Opětovné sestavení často může způsobit snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="926e0-142">Rebuilding frequently can cause slower performance.</span></span>
- <span data-ttu-id="926e0-143">často je škálovat Hello datového skladu.</span><span class="sxs-lookup"><span data-stu-id="926e0-143">hello data warehouse is scaled frequently.</span></span> <span data-ttu-id="926e0-144">Změna měřítka datového skladu změní hello počet výpočetních uzlů, které způsobuje opětovném sestavení.</span><span class="sxs-lookup"><span data-stu-id="926e0-144">Scaling a data warehouse changes hello number of Compute nodes, which incurs a rebuild.</span></span>
- <span data-ttu-id="926e0-145">Tabulka Hello má velký počet sloupců, ale operace dat obvykle přístup pouze malý počet sloupců.</span><span class="sxs-lookup"><span data-stu-id="926e0-145">hello table has a large number of columns, but data operations typically access only a small number of columns.</span></span> <span data-ttu-id="926e0-146">V tomto scénáři, namísto replikace hello celou tabulku, může být efektivnější toohash distribuovat hello tabulky a pak vytvořit index sloupce hello často používají.</span><span class="sxs-lookup"><span data-stu-id="926e0-146">In this scenario, instead of replicating hello entire table, it might be more effective toohash distribute hello table, and then create an index on hello frequently accessed columns.</span></span> <span data-ttu-id="926e0-147">Pokud je dotaz vyžaduje přesunu dat SQL Data Warehouse jen přesun dat v hello požadované sloupce.</span><span class="sxs-lookup"><span data-stu-id="926e0-147">When a query requires data movement, SQL Data Warehouse only moves data in hello requested columns.</span></span> 



## <a name="use-replicated-tables-with-simple-query-predicates"></a><span data-ttu-id="926e0-148">Použití replikované tabulky s predikáty jednoduchý dotaz</span><span class="sxs-lookup"><span data-stu-id="926e0-148">Use replicated tables with simple query predicates</span></span>
<span data-ttu-id="926e0-149">Než zvolte toodistribute nebo replikovat tabulku, vezměte v úvahu hello typy dotazů, že máte v plánu toorun s tabulkou hello.</span><span class="sxs-lookup"><span data-stu-id="926e0-149">Before you choose toodistribute or replicate a table, think about hello types of queries you plan toorun against hello table.</span></span> <span data-ttu-id="926e0-150">Pokud je to možné,</span><span class="sxs-lookup"><span data-stu-id="926e0-150">Whenever possible,</span></span>

- <span data-ttu-id="926e0-151">Pro dotazy s predikáty jednoduchý dotaz, jako je například rovnosti nebo nerovnosti použijte replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="926e0-151">Use replicated tables for queries with simple query predicates, such as equality or inequality.</span></span>
- <span data-ttu-id="926e0-152">Pro dotazy s predikáty složitý dotaz, jako je například jako použijte distribuované tabulky nebo není jako.</span><span class="sxs-lookup"><span data-stu-id="926e0-152">Use distributed tables for queries with complex query predicates, such as LIKE or NOT LIKE.</span></span>

<span data-ttu-id="926e0-153">Náročná na prostředky procesoru dotazů se nejlépe provést, když pracovní hello je distribuován do všech hello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="926e0-153">CPU-intensive queries perform best when hello work is distributed across all of hello Compute nodes.</span></span> <span data-ttu-id="926e0-154">Například dotazy, které běží výpočtů na každý řádek tabulky poskytují lepší výkon v distribuované tabulek než replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="926e0-154">For example, queries that run computations on each row of a table perform better on distributed tables than replicated tables.</span></span> <span data-ttu-id="926e0-155">Vzhledem k tomu, že replikované tabulky je uložený ve plně na každém výpočetním uzlu, náročná na prostředky procesoru dotazy na replikované tabulky spouští celou tabulku hello na každý uzel výpočty.</span><span class="sxs-lookup"><span data-stu-id="926e0-155">Since a replicated table is stored in full on each Compute node, a CPU-intensive query against a replicated table runs against hello entire table on every Compute node.</span></span> <span data-ttu-id="926e0-156">Hello navíc výpočetní můžou způsobit snížení výkonnosti dotazu.</span><span class="sxs-lookup"><span data-stu-id="926e0-156">hello extra computation can slow query performance.</span></span>

<span data-ttu-id="926e0-157">Tento dotaz má například komplexní predikátu.</span><span class="sxs-lookup"><span data-stu-id="926e0-157">For example, this query has a complex predicate.</span></span>  <span data-ttu-id="926e0-158">Rychleji spustí po dodavatele distribuované tabulku místo replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="926e0-158">It runs faster when supplier is a distributed table instead of a replicated table.</span></span> <span data-ttu-id="926e0-159">V tomto příkladu dodavatele lze distribuovat algoritmu hash nebo distribuované kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="926e0-159">In this example, supplier can be hash-distributed or round-robin distributed.</span></span>

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-tooreplicated-tables"></a><span data-ttu-id="926e0-160">Převést stávající tabulky tooreplicated kruhového dotazování tabulky</span><span class="sxs-lookup"><span data-stu-id="926e0-160">Convert existing round-robin tables tooreplicated tables</span></span>
<span data-ttu-id="926e0-161">Pokud již máte kruhového dotazování tabulky, doporučujeme, abyste je převod tooreplicated tabulky, pokud splňují s kritérii uvedených v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="926e0-161">If you already have round-robin tables, we recommend converting them tooreplicated tables if they meet with criteria outlined in this article.</span></span> <span data-ttu-id="926e0-162">Replikované tabulky zlepšit výkon na kruhového dotazování tabulky, protože se vyhnete hello potřebu přesun dat.</span><span class="sxs-lookup"><span data-stu-id="926e0-162">Replicated tables improve performance over round-robin tables because they eliminate hello need for data movement.</span></span>  <span data-ttu-id="926e0-163">Přesun dat pomocí kruhového dotazování tabulky vždy vyžaduje pro spojení.</span><span class="sxs-lookup"><span data-stu-id="926e0-163">A round-robin table always requires data movement for joins.</span></span> 

<span data-ttu-id="926e0-164">Tento příklad používá [funkce CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) toochange hello DimSalesTerritory tabulky tooa replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="926e0-164">This example uses [CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) toochange hello DimSalesTerritory table tooa replicated table.</span></span> <span data-ttu-id="926e0-165">Tento příklad funguje bez ohledu na to, jestli je DimSalesTerritory distribuovat algoritmu hash nebo kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="926e0-165">This example works regardless of whether DimSalesTerritory is hash-distributed or round-robin.</span></span>

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
RENAME OBJECT [dbo].[DimSalesTerritory] too[DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] too[DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a><span data-ttu-id="926e0-166">Příklad výkonu dotazu, kruhové dotazování versus replikovat</span><span class="sxs-lookup"><span data-stu-id="926e0-166">Query performance example for round-robin versus replicated</span></span> 

<span data-ttu-id="926e0-167">Replikované tabulky nevyžaduje žádné přesun dat pro spojení, protože hello celá tabulka je již na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="926e0-167">A replicated table does not require any data movement for joins because hello entire table is already present on each Compute node.</span></span> <span data-ttu-id="926e0-168">Pokud tabulky dimenzí hello distribuované kruhového dotazování, a připojte zkopíruje hello tabulce dimenze v úplné tooeach výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="926e0-168">If hello dimension tables are round-robin distributed, a join copies hello dimension table in full tooeach Compute node.</span></span> <span data-ttu-id="926e0-169">toomove hello data, plán dotazu hello obsahuje operace názvem BroadcastMoveOperation.</span><span class="sxs-lookup"><span data-stu-id="926e0-169">toomove hello data, hello query plan contains an operation called BroadcastMoveOperation.</span></span> <span data-ttu-id="926e0-170">Tento typ operace přesunu dat zpomalí výkon dotazů a je eliminována použití replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="926e0-170">This type of data movement operation slows query performance and is eliminated by using replicated tables.</span></span> <span data-ttu-id="926e0-171">kroky plánování tooview dotaz, použít hello [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) zobrazení katalogu systému.</span><span class="sxs-lookup"><span data-stu-id="926e0-171">tooview query plan steps, use hello [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) system catalog view.</span></span> 

<span data-ttu-id="926e0-172">Například v následujícím dotazu pro schéma AdventureWorks hello hello ` FactInternetSales` tabulka je distribuovat algoritmu hash.</span><span class="sxs-lookup"><span data-stu-id="926e0-172">For example, in following query against hello AdventureWorks schema, hello ` FactInternetSales` table is hash-distributed.</span></span> <span data-ttu-id="926e0-173">Hello `DimDate` a `DimSalesTerritory` tabulky jsou menší tabulky dimenzí.</span><span class="sxs-lookup"><span data-stu-id="926e0-173">hello `DimDate` and `DimSalesTerritory` tables are smaller dimension tables.</span></span> <span data-ttu-id="926e0-174">Tento dotaz vrátí celkový prodej hello v Severní Americe pro fiskálního roku 2004:</span><span class="sxs-lookup"><span data-stu-id="926e0-174">This query returns hello total sales in North America for fiscal year 2004:</span></span>
 
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
<span data-ttu-id="926e0-175">Znovu vytvořit `DimDate` a `DimSalesTerritory` jako kruhového dotazování tabulky.</span><span class="sxs-lookup"><span data-stu-id="926e0-175">We re-created `DimDate` and `DimSalesTerritory` as round-robin tables.</span></span> <span data-ttu-id="926e0-176">V důsledku toho hello dotazu vám ukázal hello následující plán dotazu, který má více vysílání přesunout operace:</span><span class="sxs-lookup"><span data-stu-id="926e0-176">As a result, hello query showed hello following query plan, which has multiple broadcast move operations:</span></span> 
 
![Plán dotazu pomocí kruhového dotazování](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

<span data-ttu-id="926e0-178">Znovu vytvořit `DimDate` a `DimSalesTerritory` jako replikovaných tabulek a znovu se spustil hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="926e0-178">We re-created `DimDate` and `DimSalesTerritory` as replicated tables, and ran hello query again.</span></span> <span data-ttu-id="926e0-179">Plán dotazu výsledné Hello je mnohem kratší a mít žádné nevysílá přesune.</span><span class="sxs-lookup"><span data-stu-id="926e0-179">hello resulting query plan is much shorter and does not have any broadcast moves.</span></span>

![Replikovat plán dotazu](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a><span data-ttu-id="926e0-181">Důležité informace o výkonu pro úpravy replikované tabulky</span><span class="sxs-lookup"><span data-stu-id="926e0-181">Performance considerations for modifying replicated tables</span></span>
<span data-ttu-id="926e0-182">SQL Data Warehouse implementuje replikované tabulky udržováním hlavní verze tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="926e0-182">SQL Data Warehouse implements a replicated table by maintaining a master version of hello table.</span></span> <span data-ttu-id="926e0-183">Kopíruje hello hlavní verze tooone distribuční databázi na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="926e0-183">It copies hello master version tooone distribution database on each Compute node.</span></span> <span data-ttu-id="926e0-184">Když dojde ke změně, aktualizuje SQL Data Warehouse nejprve hello hlavní tabulka.</span><span class="sxs-lookup"><span data-stu-id="926e0-184">When there is a change, SQL Data Warehouse first updates hello master table.</span></span> <span data-ttu-id="926e0-185">Potom vyžaduje opětovné sestavení hello tabulek na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="926e0-185">Then it requires a rebuild of hello tables on each Compute node.</span></span> <span data-ttu-id="926e0-186">Opětovné sestavení replikované tabulky zahrnuje kopírování hello tabulky tooeach výpočetní uzel a pak znovu sestavit indexy hello.</span><span class="sxs-lookup"><span data-stu-id="926e0-186">A rebuild of a replicated table includes copying hello table tooeach Compute node and then rebuilding hello indexes.</span></span>

<span data-ttu-id="926e0-187">Znovu sestaví je potřeba po:</span><span class="sxs-lookup"><span data-stu-id="926e0-187">Rebuilds are required after:</span></span>
- <span data-ttu-id="926e0-188">Data jsou načíst nebo upravit</span><span class="sxs-lookup"><span data-stu-id="926e0-188">Data is loaded or modified</span></span>
- <span data-ttu-id="926e0-189">datový sklad Hello je jiné nastavení DWU tooa škálovat.</span><span class="sxs-lookup"><span data-stu-id="926e0-189">hello data warehouse is scaled tooa different DWU setting</span></span>
- <span data-ttu-id="926e0-190">Aktualizace definice tabulky</span><span class="sxs-lookup"><span data-stu-id="926e0-190">Table definition is updated</span></span>

<span data-ttu-id="926e0-191">Znovu sestaví nejsou nutné po:</span><span class="sxs-lookup"><span data-stu-id="926e0-191">Rebuilds are not required after:</span></span>
- <span data-ttu-id="926e0-192">Operace pozastavení</span><span class="sxs-lookup"><span data-stu-id="926e0-192">Pause operation</span></span>
- <span data-ttu-id="926e0-193">Operace obnovení</span><span class="sxs-lookup"><span data-stu-id="926e0-193">Resume operation</span></span>

<span data-ttu-id="926e0-194">opětovné sestavení Hello neodehrává ihned po data je upravit.</span><span class="sxs-lookup"><span data-stu-id="926e0-194">hello rebuild does not happen immediately after data is modified.</span></span> <span data-ttu-id="926e0-195">Místo toho se aktivuje opětovné sestavení hello hello prvním dotazu vybere z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="926e0-195">Instead, hello rebuild is triggered hello first time a query selects from hello table.</span></span>  <span data-ttu-id="926e0-196">V rámci hello počáteční příkazu select z tabulky hello jsou kroky toorebuild hello replikované tabulky.</span><span class="sxs-lookup"><span data-stu-id="926e0-196">Within hello initial select statement from hello table are steps toorebuild hello replicated table.</span></span>  <span data-ttu-id="926e0-197">Protože opětovné sestavení hello se provádí v rámci dotazu hello, může být důležité v závislosti na velikosti hello hello tabulky hello dopad toohello počáteční příkazu select.</span><span class="sxs-lookup"><span data-stu-id="926e0-197">Because hello rebuild is done within hello query, hello impact toohello initial select statement could be significant depending on hello size of hello table.</span></span>  <span data-ttu-id="926e0-198">Pokud více replikované tabulky se podílejí vyžadující opětovném sestavení, každá kopie je znovu sestavit sériově jako kroky v rámci příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="926e0-198">If multiple replicated tables are involved that need a rebuild, each copy is rebuilt serially as steps within hello statement.</span></span>  <span data-ttu-id="926e0-199">toomaintain konzistenci dat během hello opakované hello replikované tabulky pořízení v tabulce hello výhradní zámek.</span><span class="sxs-lookup"><span data-stu-id="926e0-199">toomaintain data consistency during hello rebuild of hello replicated table an exclusive lock is taken on hello table.</span></span>  <span data-ttu-id="926e0-200">Hello uzamčení brání všechny tabulky toohello přístup hello dobu hello přestavení.</span><span class="sxs-lookup"><span data-stu-id="926e0-200">hello lock prevents all access toohello table for hello duration of hello rebuild.</span></span> 

### <a name="use-indexes-conservatively"></a><span data-ttu-id="926e0-201">Můžete použít indexy</span><span class="sxs-lookup"><span data-stu-id="926e0-201">Use indexes conservatively</span></span>
<span data-ttu-id="926e0-202">Standardní postupy indexování použít tooreplicated tabulky.</span><span class="sxs-lookup"><span data-stu-id="926e0-202">Standard indexing practices apply tooreplicated tables.</span></span> <span data-ttu-id="926e0-203">SQL Data Warehouse znovu sestaví každý index replikované tabulky v rámci opětovné sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="926e0-203">SQL Data Warehouse rebuilds each replicated table index as part of hello rebuild.</span></span> <span data-ttu-id="926e0-204">Indexy používejte jenom hello výkonnější převáží hello náklady na nové sestavení indexů hello.</span><span class="sxs-lookup"><span data-stu-id="926e0-204">Only use indexes when hello performance gain outweighs hello cost of rebuilding hello indexes.</span></span>  
 
### <a name="batch-data-loads"></a><span data-ttu-id="926e0-205">Načítání dat dávky</span><span class="sxs-lookup"><span data-stu-id="926e0-205">Batch data loads</span></span>
<span data-ttu-id="926e0-206">Při načítání dat do replikované tabulky, zkuste znovu sestaví toominimize dávkování zatížení dohromady.</span><span class="sxs-lookup"><span data-stu-id="926e0-206">When loading data into replicated tables, try toominimize rebuilds by batching loads together.</span></span> <span data-ttu-id="926e0-207">Proveďte všechny zatížení hello zpracovat v dávce před spuštěním příkazů select.</span><span class="sxs-lookup"><span data-stu-id="926e0-207">Perform all hello batched loads before running select statements.</span></span>

<span data-ttu-id="926e0-208">Tento vzor zatížení například načte data ze čtyř zdrojů a vyvolá čtyři znovu sestaví.</span><span class="sxs-lookup"><span data-stu-id="926e0-208">For example, this load pattern loads data from four sources and invokes four rebuilds.</span></span> 

- <span data-ttu-id="926e0-209">Načíst ze zdroje 1.</span><span class="sxs-lookup"><span data-stu-id="926e0-209">Load from source 1.</span></span>
- <span data-ttu-id="926e0-210">Příkaz SELECT aktivační události znovu sestavit 1.</span><span class="sxs-lookup"><span data-stu-id="926e0-210">Select statement triggers rebuild 1.</span></span>
- <span data-ttu-id="926e0-211">Načíst ze zdroje. 2.</span><span class="sxs-lookup"><span data-stu-id="926e0-211">Load from source 2.</span></span>
- <span data-ttu-id="926e0-212">Příkaz SELECT aktivační události znovu sestavit 2.</span><span class="sxs-lookup"><span data-stu-id="926e0-212">Select statement triggers rebuild 2.</span></span>
- <span data-ttu-id="926e0-213">Načíst ze zdroje 3.</span><span class="sxs-lookup"><span data-stu-id="926e0-213">Load from source 3.</span></span>
- <span data-ttu-id="926e0-214">Příkaz SELECT aktivační události znovu sestavit 3.</span><span class="sxs-lookup"><span data-stu-id="926e0-214">Select statement triggers rebuild 3.</span></span>
- <span data-ttu-id="926e0-215">Načíst ze zdroje 4.</span><span class="sxs-lookup"><span data-stu-id="926e0-215">Load from source 4.</span></span>
- <span data-ttu-id="926e0-216">Příkaz SELECT aktivační události znovu sestavit 4.</span><span class="sxs-lookup"><span data-stu-id="926e0-216">Select statement triggers rebuild 4.</span></span>

<span data-ttu-id="926e0-217">Tento vzor zatížení například načte data ze čtyř zdrojů, ale pouze vyvolá jeden opětovné sestavení.</span><span class="sxs-lookup"><span data-stu-id="926e0-217">For example, this load pattern loads data from four sources, but only invokes one rebuild.</span></span>

- <span data-ttu-id="926e0-218">Načíst ze zdroje 1.</span><span class="sxs-lookup"><span data-stu-id="926e0-218">Load from source 1.</span></span>
- <span data-ttu-id="926e0-219">Načíst ze zdroje. 2.</span><span class="sxs-lookup"><span data-stu-id="926e0-219">Load from source 2.</span></span>
- <span data-ttu-id="926e0-220">Načíst ze zdroje 3.</span><span class="sxs-lookup"><span data-stu-id="926e0-220">Load from source 3.</span></span>
- <span data-ttu-id="926e0-221">Načíst ze zdroje 4.</span><span class="sxs-lookup"><span data-stu-id="926e0-221">Load from source 4.</span></span>
- <span data-ttu-id="926e0-222">Příkaz SELECT aktivační události znovu sestavit.</span><span class="sxs-lookup"><span data-stu-id="926e0-222">Select statement triggers rebuild.</span></span>


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a><span data-ttu-id="926e0-223">Znovu sestavte replikované tabulky po zatížení batch</span><span class="sxs-lookup"><span data-stu-id="926e0-223">Rebuild a replicated table after a batch load</span></span>
<span data-ttu-id="926e0-224">dobu provádění konzistentní dotazu tooensure, doporučujeme vynutit aktualizaci hello replikované tabulky po batch zatížení.</span><span class="sxs-lookup"><span data-stu-id="926e0-224">tooensure consistent query execution times, we recommend forcing a refresh of hello replicated tables after a batch load.</span></span> <span data-ttu-id="926e0-225">První dotaz hello musí počkat, jinak hodnota toorefresh hello tabulky, která zahrnuje nové sestavení indexů hello.</span><span class="sxs-lookup"><span data-stu-id="926e0-225">Otherwise, hello first query must wait for hello tables toorefresh, which includes rebuilding hello indexes.</span></span> <span data-ttu-id="926e0-226">V závislosti na velikosti hello a počet replikované tabulky vliv může být důležité hello vlivu na výkon.</span><span class="sxs-lookup"><span data-stu-id="926e0-226">Depending on hello size and number of replicated tables affected, hello performance impact can be significant.</span></span>  

<span data-ttu-id="926e0-227">Tento dotaz používá hello [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV toolist hello replikovaných tabulek, které byla upravena, ale není znovu sestavit.</span><span class="sxs-lookup"><span data-stu-id="926e0-227">This query uses hello [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV toolist hello replicated tables that have been modified, but not rebuilt.</span></span>

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
 
<span data-ttu-id="926e0-228">tooforce opětovném sestavení, spusťte následující příkaz pro každou tabulku v předcházející výstup hello hello.</span><span class="sxs-lookup"><span data-stu-id="926e0-228">tooforce a rebuild, run hello following statement on each table in hello preceding output.</span></span> 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a><span data-ttu-id="926e0-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="926e0-229">Next steps</span></span> 
<span data-ttu-id="926e0-230">toocreate replikované tabulky, použijte jednu z těchto příkazů:</span><span class="sxs-lookup"><span data-stu-id="926e0-230">toocreate a replicated table, use one of these statements:</span></span>

- [<span data-ttu-id="926e0-231">Vytvoření tabulky (Azure SQL Data Warehouse)</span><span class="sxs-lookup"><span data-stu-id="926e0-231">CREATE TABLE (Azure SQL Data Warehouse)</span></span>](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [<span data-ttu-id="926e0-232">Vytvoření TABLE AS SELECT (Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="926e0-232">CREATE TABLE AS SELECT (Azure SQL Data Warehouse</span></span>](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

<span data-ttu-id="926e0-233">Přehled distribuované tabulek, najdete v tématu [distribuované tabulky](sql-data-warehouse-tables-distribute.md).</span><span class="sxs-lookup"><span data-stu-id="926e0-233">For an overview of distributed tables, see [distributed tables](sql-data-warehouse-tables-distribute.md).</span></span>



