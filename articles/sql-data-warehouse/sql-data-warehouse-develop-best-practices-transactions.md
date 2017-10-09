---
title: transakce aaaOptimizing pro SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 1a821161711db9460b7e10d3cf7ba498d711448b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimizing-transactions-for-sql-data-warehouse"></a><span data-ttu-id="78eb4-103">Optimalizace transakcí pro SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="78eb4-103">Optimizing transactions for SQL Data Warehouse</span></span>
<span data-ttu-id="78eb4-104">Tento článek vysvětluje, jak toooptimize hello výkonu transakcí kódu a současně minimalizujete její rizik dlouho odvolání.</span><span class="sxs-lookup"><span data-stu-id="78eb4-104">This article explains how toooptimize hello performance of your transactional code while minimizing risk for long rollbacks.</span></span>

## <a name="transactions-and-logging"></a><span data-ttu-id="78eb4-105">Transakce a protokolování</span><span class="sxs-lookup"><span data-stu-id="78eb4-105">Transactions and logging</span></span>
<span data-ttu-id="78eb4-106">Transakce jsou důležitou součástí relační databázový stroj.</span><span class="sxs-lookup"><span data-stu-id="78eb4-106">Transactions are an important component of a relational database engine.</span></span> <span data-ttu-id="78eb4-107">SQL Data Warehouse používá při úpravě dat transakce.</span><span class="sxs-lookup"><span data-stu-id="78eb4-107">SQL Data Warehouse uses transactions during data modification.</span></span> <span data-ttu-id="78eb4-108">Tyto transakce může být explicitní nebo implicitní.</span><span class="sxs-lookup"><span data-stu-id="78eb4-108">These transactions can be explicit or implicit.</span></span> <span data-ttu-id="78eb4-109">Jeden `INSERT`, `UPDATE` a `DELETE` příkazy jsou všechny příklady implicitní transakce.</span><span class="sxs-lookup"><span data-stu-id="78eb4-109">Single `INSERT`, `UPDATE` and `DELETE` statements are all examples of implicit transactions.</span></span> <span data-ttu-id="78eb4-110">Explicitní transakce jsou zapsány explicitně vývojáři pomocí `BEGIN TRAN`, `COMMIT TRAN` nebo `ROLLBACK TRAN` a jsou obvykle používány při více příkazů úpravy potřebovat toobe svázané společně v jedné jednotky atomic.</span><span class="sxs-lookup"><span data-stu-id="78eb4-110">Explicit transactions are written explicitly by a developer using `BEGIN TRAN`, `COMMIT TRAN` or `ROLLBACK TRAN` and are typically used when multiple modification statements need toobe tied together in a single atomic unit.</span></span> 

<span data-ttu-id="78eb4-111">Azure SQL Data Warehouse potvrdí změny toohello databázi pomocí protokolů transakcí.</span><span class="sxs-lookup"><span data-stu-id="78eb4-111">Azure SQL Data Warehouse commits changes toohello database using transaction logs.</span></span> <span data-ttu-id="78eb4-112">Každý distribuční má svou vlastní transakčního protokolu.</span><span class="sxs-lookup"><span data-stu-id="78eb4-112">Each distribution has its own transaction log.</span></span> <span data-ttu-id="78eb4-113">Zápisy protokolu transakce jsou automatické.</span><span class="sxs-lookup"><span data-stu-id="78eb4-113">Transaction log writes are automatic.</span></span> <span data-ttu-id="78eb4-114">Není nutná žádná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="78eb4-114">There is no configuration required.</span></span> <span data-ttu-id="78eb4-115">A zároveň zaručí se tento proces zápisu hello ji v systému hello zavést režijní náklady.</span><span class="sxs-lookup"><span data-stu-id="78eb4-115">However, whilst this process guarantees hello write it does introduce an overhead in hello system.</span></span> <span data-ttu-id="78eb4-116">Zápisem transakčně efektivní kódu můžete minimalizovat tomuto vlivu.</span><span class="sxs-lookup"><span data-stu-id="78eb4-116">You can minimize this impact by writing transactionally efficient code.</span></span> <span data-ttu-id="78eb4-117">Transakčně efektivní kód široce spadá do dvou kategorií.</span><span class="sxs-lookup"><span data-stu-id="78eb4-117">Transactionally efficient code broadly falls into two categories.</span></span>

* <span data-ttu-id="78eb4-118">Pokud je to možné konstruktů minimální úroveň protokolování</span><span class="sxs-lookup"><span data-stu-id="78eb4-118">Leverage minimal logging constructs where possible</span></span>
* <span data-ttu-id="78eb4-119">Zpracování dat pomocí obor jednotném čísle tooavoid dávek dlouho běžící transakce</span><span class="sxs-lookup"><span data-stu-id="78eb4-119">Process data using scoped batches tooavoid singular long running transactions</span></span>
* <span data-ttu-id="78eb4-120">Použít oddíl přepínání vzor pro zadaný oddíl tooa velké změny</span><span class="sxs-lookup"><span data-stu-id="78eb4-120">Adopt a partition switching pattern for large modifications tooa given partition</span></span>

## <a name="minimal-vs-full-logging"></a><span data-ttu-id="78eb4-121">Minimální oproti úplné protokolování</span><span class="sxs-lookup"><span data-stu-id="78eb4-121">Minimal vs. full logging</span></span>
<span data-ttu-id="78eb4-122">Na rozdíl od plně protokolovaných operací, které používají hello transakce protokolu tookeep sledovat všechny změny řádek, minimálně protokolovaných operací uchovávání informací o přidělení rozsahu a pouze metadata změny.</span><span class="sxs-lookup"><span data-stu-id="78eb4-122">Unlike fully logged operations, which use hello transaction log tookeep track of every row change, minimally logged operations keep track of extent allocations and meta-data changes only.</span></span> <span data-ttu-id="78eb4-123">Proto minimální protokolování zahrnuje protokolování pouze hello informace, které jsou požadované toorollback hello transakce v události hello selhání nebo explicitní požadavku (`ROLLBACK TRAN`).</span><span class="sxs-lookup"><span data-stu-id="78eb4-123">Therefore, minimal logging involves logging only hello information that is required toorollback hello transaction in hello event of a failure or an explicit request (`ROLLBACK TRAN`).</span></span> <span data-ttu-id="78eb4-124">Jak informace je mnohem méně sledovat v protokolu transakcí hello, provede se lépe než podobné velikosti plně protokolovaných operací minimálně protokolovaných operací.</span><span class="sxs-lookup"><span data-stu-id="78eb4-124">As much less information is tracked in hello transaction log, a minimally logged operation performs better than a similarly sized fully logged operation.</span></span> <span data-ttu-id="78eb4-125">Kromě toho protože méně zápisy přejděte hello transakčního protokolu, je generována mnohem menší množství dat protokolu a stejně tak více vstupně-výstupních operací efektivní.</span><span class="sxs-lookup"><span data-stu-id="78eb4-125">Furthermore, because fewer writes go hello transaction log, a much smaller amount of log data is generated and so is more I/O efficient.</span></span>

<span data-ttu-id="78eb4-126">omezení zabezpečení transakce Hello platit pouze operace toofully přihlášení.</span><span class="sxs-lookup"><span data-stu-id="78eb4-126">hello transaction safety limits only apply toofully logged operations.</span></span>

> [!NOTE]
> <span data-ttu-id="78eb4-127">Minimálně protokolovaných operací se účastnit explicitních transakcí.</span><span class="sxs-lookup"><span data-stu-id="78eb4-127">Minimally logged operations can participate in explicit transactions.</span></span> <span data-ttu-id="78eb4-128">Sledování všech změn v přidělení struktury, je možné tooroll zpět minimálně protokolována operace.</span><span class="sxs-lookup"><span data-stu-id="78eb4-128">As all changes in allocation structures are tracked, it is possible tooroll back minimally logged operations.</span></span> <span data-ttu-id="78eb4-129">Je důležité k jeho nastavení toounderstand, který změnu hello je "minimálně" není zrušení protokolu.</span><span class="sxs-lookup"><span data-stu-id="78eb4-129">It is important toounderstand that hello change is "minimally" logged it is not un-logged.</span></span>
> 
> 

## <a name="minimally-logged-operations"></a><span data-ttu-id="78eb4-130">Minimálně protokolovaných operací</span><span class="sxs-lookup"><span data-stu-id="78eb4-130">Minimally logged operations</span></span>
<span data-ttu-id="78eb4-131">Hello následující operace jsou schopny minimálně protokolována:</span><span class="sxs-lookup"><span data-stu-id="78eb4-131">hello following operations are capable of being minimally logged:</span></span>

* <span data-ttu-id="78eb4-132">VYTVOŘENÍ TABLE AS SELECT ([FUNKCE CTAS][CTAS])</span><span class="sxs-lookup"><span data-stu-id="78eb4-132">CREATE TABLE AS SELECT ([CTAS][CTAS])</span></span>
* <span data-ttu-id="78eb4-133">PŘÍKAZ INSERT... VYBERTE</span><span class="sxs-lookup"><span data-stu-id="78eb4-133">INSERT..SELECT</span></span>
* <span data-ttu-id="78eb4-134">VYTVOŘENÍ INDEXU</span><span class="sxs-lookup"><span data-stu-id="78eb4-134">CREATE INDEX</span></span>
* <span data-ttu-id="78eb4-135">PŘÍKAZ ALTER INDEX OPĚTOVNÉ SESTAVENÍ</span><span class="sxs-lookup"><span data-stu-id="78eb4-135">ALTER INDEX REBUILD</span></span>
* <span data-ttu-id="78eb4-136">DROP INDEX</span><span class="sxs-lookup"><span data-stu-id="78eb4-136">DROP INDEX</span></span>
* <span data-ttu-id="78eb4-137">ZKRÁCENÍ TABULKY</span><span class="sxs-lookup"><span data-stu-id="78eb4-137">TRUNCATE TABLE</span></span>
* <span data-ttu-id="78eb4-138">ODPOJIT TABULKU</span><span class="sxs-lookup"><span data-stu-id="78eb4-138">DROP TABLE</span></span>
* <span data-ttu-id="78eb4-139">PŘÍKAZ ALTER TABLE SWITCH PARTITION</span><span class="sxs-lookup"><span data-stu-id="78eb4-139">ALTER TABLE SWITCH PARTITION</span></span>

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> <span data-ttu-id="78eb4-140">Operace přesunu dat interní (například `BROADCAST` a `SHUFFLE`) nemá vliv hello transakce bezpečnostní omezení.</span><span class="sxs-lookup"><span data-stu-id="78eb4-140">Internal data movement operations (such as `BROADCAST` and `SHUFFLE`) are not affected by hello transaction safety limit.</span></span>
> 
> 

## <a name="minimal-logging-with-bulk-load"></a><span data-ttu-id="78eb4-141">Minimální protokolování s hromadné načtení</span><span class="sxs-lookup"><span data-stu-id="78eb4-141">Minimal logging with bulk load</span></span>
<span data-ttu-id="78eb4-142">`CTAS`a `INSERT...SELECT` jsou obě hromadné operace zatížení.</span><span class="sxs-lookup"><span data-stu-id="78eb4-142">`CTAS` and `INSERT...SELECT` are both bulk load operations.</span></span> <span data-ttu-id="78eb4-143">Však obě jsou ovlivněné definice hello cílové tabulky a závisí na scénáři zátěžového hello.</span><span class="sxs-lookup"><span data-stu-id="78eb4-143">However, both are influenced by hello target table definition and depend on hello load scenario.</span></span> <span data-ttu-id="78eb4-144">Dole je tabulku, která vysvětluje, pokud hromadné operace plně nebo minimálně zaznamenán:</span><span class="sxs-lookup"><span data-stu-id="78eb4-144">Below is a table that explains if your bulk operation will be fully or minimally logged:</span></span>  

| <span data-ttu-id="78eb4-145">Primární Index</span><span class="sxs-lookup"><span data-stu-id="78eb4-145">Primary Index</span></span> | <span data-ttu-id="78eb4-146">Scénář zatížení</span><span class="sxs-lookup"><span data-stu-id="78eb4-146">Load Scenario</span></span> | <span data-ttu-id="78eb4-147">Režim protokolování</span><span class="sxs-lookup"><span data-stu-id="78eb4-147">Logging Mode</span></span> |
| --- | --- | --- |
| <span data-ttu-id="78eb4-148">Haldy</span><span class="sxs-lookup"><span data-stu-id="78eb4-148">Heap</span></span> |<span data-ttu-id="78eb4-149">Všechny</span><span class="sxs-lookup"><span data-stu-id="78eb4-149">Any</span></span> |<span data-ttu-id="78eb4-150">**Minimální**</span><span class="sxs-lookup"><span data-stu-id="78eb4-150">**Minimal**</span></span> |
| <span data-ttu-id="78eb4-151">Clusterovaný Index</span><span class="sxs-lookup"><span data-stu-id="78eb4-151">Clustered Index</span></span> |<span data-ttu-id="78eb4-152">Prázdná cílová tabulka</span><span class="sxs-lookup"><span data-stu-id="78eb4-152">Empty target table</span></span> |<span data-ttu-id="78eb4-153">**Minimální**</span><span class="sxs-lookup"><span data-stu-id="78eb4-153">**Minimal**</span></span> |
| <span data-ttu-id="78eb4-154">Clusterovaný Index</span><span class="sxs-lookup"><span data-stu-id="78eb4-154">Clustered Index</span></span> |<span data-ttu-id="78eb4-155">Načíst řádky se nepřekrývají s existující stránky v cíli</span><span class="sxs-lookup"><span data-stu-id="78eb4-155">Loaded rows do not overlap with existing pages in target</span></span> |<span data-ttu-id="78eb4-156">**Minimální**</span><span class="sxs-lookup"><span data-stu-id="78eb4-156">**Minimal**</span></span> |
| <span data-ttu-id="78eb4-157">Clusterovaný Index</span><span class="sxs-lookup"><span data-stu-id="78eb4-157">Clustered Index</span></span> |<span data-ttu-id="78eb4-158">Načíst řádky překrývá s existující stránky v cíli</span><span class="sxs-lookup"><span data-stu-id="78eb4-158">Loaded rows overlap with existing pages in target</span></span> |<span data-ttu-id="78eb4-159">Úplná</span><span class="sxs-lookup"><span data-stu-id="78eb4-159">Full</span></span> |
| <span data-ttu-id="78eb4-160">Clusterovaný Index Columnstore</span><span class="sxs-lookup"><span data-stu-id="78eb4-160">Clustered Columnstore Index</span></span> |<span data-ttu-id="78eb4-161">Velikost dávky > = 102,400 za oddílu zarovnán distribuce</span><span class="sxs-lookup"><span data-stu-id="78eb4-161">Batch size >= 102,400 per partition aligned distribution</span></span> |<span data-ttu-id="78eb4-162">**Minimální**</span><span class="sxs-lookup"><span data-stu-id="78eb4-162">**Minimal**</span></span> |
| <span data-ttu-id="78eb4-163">Clusterovaný Index Columnstore</span><span class="sxs-lookup"><span data-stu-id="78eb4-163">Clustered Columnstore Index</span></span> |<span data-ttu-id="78eb4-164">Velikost < 102,400 za oddílu zarovnán distribuční služby batch</span><span class="sxs-lookup"><span data-stu-id="78eb4-164">Batch size < 102,400 per partition aligned distribution</span></span> |<span data-ttu-id="78eb4-165">Úplná</span><span class="sxs-lookup"><span data-stu-id="78eb4-165">Full</span></span> |

<span data-ttu-id="78eb4-166">Je vhodné poznamenat, že všech zápisu, sekundární nebo neclusterované indexy tooupdate bude vždy plně zaznamenány operace.</span><span class="sxs-lookup"><span data-stu-id="78eb4-166">It is worth noting that any writes tooupdate secondary or non-clustered indexes will always be fully logged operations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="78eb4-167">SQL Data Warehouse je 60 distribuce.</span><span class="sxs-lookup"><span data-stu-id="78eb4-167">SQL Data Warehouse has 60 distributions.</span></span> <span data-ttu-id="78eb4-168">Proto za předpokladu, že všechny řádky jsou rozloženy rovnoměrně a cílová v jeden oddíl, vaše batch bude nutné toocontain 6,144,000 řádků nebo větší toobe minimálně zaznamená při zápisu tooa Index Columnstore clusteru.</span><span class="sxs-lookup"><span data-stu-id="78eb4-168">Therefore, assuming all rows are evenly distributed and landing in a single partition, your batch will need toocontain 6,144,000 rows or larger toobe minimally logged when writing tooa Clustered Columnstore Index.</span></span> <span data-ttu-id="78eb4-169">Pokud hello tabulka je rozdělena na oddíly a hello řádky vkládání span hranice oddílů, budete potřebovat 6,144,000 řádky za hranice oddílu za předpokladu, že i distribuci dat.</span><span class="sxs-lookup"><span data-stu-id="78eb4-169">If hello table is partitioned and hello rows being inserted span partition boundaries, then you will need 6,144,000 rows per partition boundary assuming even data distribution.</span></span> <span data-ttu-id="78eb4-170">Každý oddíl v každém distribučním musí přesahovat nezávisle hello činí 102 400 řádek pro prahovou hodnotu toobe vložení hello minimálně přihlášen distribuční hello.</span><span class="sxs-lookup"><span data-stu-id="78eb4-170">Each partition in each distribution must independently exceed hello 102,400 row threshold for hello insert toobe minimally logged into hello distribution.</span></span>
> 
> 

<span data-ttu-id="78eb4-171">Načítání dat do tabulky neprázdný clusterovaný index často může obsahovat kombinaci plně protokolu a minimálně zaznamenané řádků.</span><span class="sxs-lookup"><span data-stu-id="78eb4-171">Loading data into a non-empty table with a clustered index can often contain a mixture of fully logged and minimally logged rows.</span></span> <span data-ttu-id="78eb4-172">Clusterovaný index je vyrovnáváním stromu (b stromu) stránek.</span><span class="sxs-lookup"><span data-stu-id="78eb4-172">A clustered index is a balanced tree (b-tree) of pages.</span></span> <span data-ttu-id="78eb4-173">Pokud stránku hello zapisovaný tooalready obsahuje řádky z jiné transakci, pak tyto zápisy plně zaznamenán.</span><span class="sxs-lookup"><span data-stu-id="78eb4-173">If hello page being written tooalready contains rows from another transaction, then these writes will be fully logged.</span></span> <span data-ttu-id="78eb4-174">Ale pokud stránku hello je prázdný pak hello zápisu toothat stránka bude minimálně protokolována.</span><span class="sxs-lookup"><span data-stu-id="78eb4-174">However, if hello page is empty then hello write toothat page will be minimally logged.</span></span>

## <a name="optimizing-deletes"></a><span data-ttu-id="78eb4-175">Optimalizace odstranění</span><span class="sxs-lookup"><span data-stu-id="78eb4-175">Optimizing deletes</span></span>
<span data-ttu-id="78eb4-176">`DELETE`je plně protokolovaných operací.</span><span class="sxs-lookup"><span data-stu-id="78eb4-176">`DELETE` is a fully logged operation.</span></span>  <span data-ttu-id="78eb4-177">Pokud potřebujete toodelete velké množství dat v tabulce nebo oddílu, je často vhodnější příliš`SELECT` hello data chcete tookeep, který může běžet jako minimálně protokolovaných operací.</span><span class="sxs-lookup"><span data-stu-id="78eb4-177">If you need toodelete a large amount of data in a table or a partition, it often makes more sense too`SELECT` hello data you wish tookeep, which can be run as a minimally logged operation.</span></span>  <span data-ttu-id="78eb4-178">tooaccomplish, vytvořit novou tabulku s [funkce CTAS][CTAS].</span><span class="sxs-lookup"><span data-stu-id="78eb4-178">tooaccomplish this, create a new table with [CTAS][CTAS].</span></span>  <span data-ttu-id="78eb4-179">Po vytvoření použít [přejmenovat] [ RENAME] tooswap se vaše staré tabulku s tabulkou hello nově vytvořený.</span><span class="sxs-lookup"><span data-stu-id="78eb4-179">Once created, use [RENAME][RENAME] tooswap out your old table with hello newly created table.</span></span>

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only hello records we want tookep (PromotionKey 2)
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

--Step 02. Rename hello Tables tooreplace hello 
RENAME OBJECT [dbo].[FactInternetSales]   too[FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] too[FactInternetSales];
```

## <a name="optimizing-updates"></a><span data-ttu-id="78eb4-180">Optimalizace aktualizace</span><span class="sxs-lookup"><span data-stu-id="78eb4-180">Optimizing updates</span></span>
<span data-ttu-id="78eb4-181">`UPDATE`je plně protokolovaných operací.</span><span class="sxs-lookup"><span data-stu-id="78eb4-181">`UPDATE` is a fully logged operation.</span></span>  <span data-ttu-id="78eb4-182">Pokud potřebujete tooupdate velký počet řádků v tabulce nebo oddíl může být často mnohem efektivnější toouse minimálně protokolovaných operací, jako [funkce CTAS] [ CTAS] toodo tak.</span><span class="sxs-lookup"><span data-stu-id="78eb4-182">If you need tooupdate a large number of rows in a table or a partition it can often be far more efficient toouse a minimally logged operation such as [CTAS][CTAS] toodo so.</span></span>

<span data-ttu-id="78eb4-183">Příklad dole plném tabulka aktualizace byl v hello převedený tooa `CTAS` tak, aby minimální protokolování je možné.</span><span class="sxs-lookup"><span data-stu-id="78eb4-183">In hello example below a full table update has been converted tooa `CTAS` so that minimal logging is possible.</span></span>

<span data-ttu-id="78eb4-184">V takovém případě zpětně přidáváme toohello prodejní částku slevu v tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="78eb4-184">In this case we are retrospectively adding a discount amount toohello sales in hello table:</span></span>

```sql
--Step 01. Create a new table containing hello "Update". 
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

--Step 02. Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales]   too[FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] too[FactInternetSales];

--Step 03. Drop hello old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [!NOTE]
> <span data-ttu-id="78eb4-185">Znovu vytvořit velké tabulky můžete využívat výhod pomocí funkce SQL Data Warehouse úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="78eb4-185">Re-creating large tables can benefit from using SQL Data Warehouse workload management features.</span></span> <span data-ttu-id="78eb4-186">Pro další podrobnosti naleznete v části Správa toohello zatížení v hello [souběžnosti] [ concurrency] článku.</span><span class="sxs-lookup"><span data-stu-id="78eb4-186">For more details please refer toohello workload management section in hello [concurrency][concurrency] article.</span></span>
> 
> 

## <a name="optimizing-with-partition-switching"></a><span data-ttu-id="78eb4-187">Optimalizace s přepnutí oddílu</span><span class="sxs-lookup"><span data-stu-id="78eb4-187">Optimizing with partition switching</span></span>
<span data-ttu-id="78eb4-188">Při velkém měřítku úpravy uvnitř [tabulky oddílu][table partition], pak oddíl přepínání vzor díky spoustu smysl.</span><span class="sxs-lookup"><span data-stu-id="78eb4-188">When faced with large scale modifications inside a [table partition][table partition], then a partition switching pattern makes a lot of sense.</span></span> <span data-ttu-id="78eb4-189">Pokud hello úprava dat je důležité a rozsahy, lze dosáhnout více oddílů, pak se jednoduše iterování přes hello oddíly hello stejného výsledku.</span><span class="sxs-lookup"><span data-stu-id="78eb4-189">If hello data modification is significant and spans multiple partitions, then simply iterating over hello partitions achieves hello same result.</span></span>

<span data-ttu-id="78eb4-190">Hello kroky tooperform přepínač oddílu jsou následující:</span><span class="sxs-lookup"><span data-stu-id="78eb4-190">hello steps tooperform a partition switch are as follows:</span></span>

1. <span data-ttu-id="78eb4-191">Vytvořte prázdnou na oddíl</span><span class="sxs-lookup"><span data-stu-id="78eb4-191">Create an empty out partition</span></span>
2. <span data-ttu-id="78eb4-192">Proveďte hello aktualizace funkce CTAS</span><span class="sxs-lookup"><span data-stu-id="78eb4-192">Perform hello 'update' as a CTAS</span></span>
3. <span data-ttu-id="78eb4-193">Přepnout na hello existující data toohello se tabulka</span><span class="sxs-lookup"><span data-stu-id="78eb4-193">Switch out hello existing data toohello out table</span></span>
4. <span data-ttu-id="78eb4-194">Přepínač ve hello nová data</span><span class="sxs-lookup"><span data-stu-id="78eb4-194">Switch in hello new data</span></span>
5. <span data-ttu-id="78eb4-195">Vyčištění dat hello</span><span class="sxs-lookup"><span data-stu-id="78eb4-195">Clean up hello data</span></span>

<span data-ttu-id="78eb4-196">Ale toohelp identifikovat tooswitch oddíly hello se nejdřív potřebujeme toobuild postup pomocníka například hello jeden níže.</span><span class="sxs-lookup"><span data-stu-id="78eb4-196">However, toohelp identify hello partitions tooswitch we will first need toobuild a helper procedure such as hello one below.</span></span> 

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

<span data-ttu-id="78eb4-197">Tento postup maximalizuje opětovné použití kódu a udržuje přepnutí příklad kompaktnější hello oddílu.</span><span class="sxs-lookup"><span data-stu-id="78eb4-197">This procedure maximizes code re-use and keeps hello partition switching example more compact.</span></span>

<span data-ttu-id="78eb4-198">Hello kód níže ukazuje, že v pěti krocích hello zmíněné tooachieve úplné oddíl přepínání rutiny.</span><span class="sxs-lookup"><span data-stu-id="78eb4-198">hello code below demonstrates hello five steps mentioned above tooachieve a full partition switching routine.</span></span>

```sql
--Create a partitioned aligned empty table tooswitch out hello data 
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

--Create a partitioned aligned table and update hello data in hello select portion of hello CTAS
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

--Use hello helper procedure tooidentify hello partitions
--hello source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--hello "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--hello "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch hello partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]    SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))    +' too[dbo].[FactInternetSales_out] PARTITION '    +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))    +' too[dbo].[FactInternetSales] PARTITION '        +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform hello clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a><span data-ttu-id="78eb4-199">Minimalizovat protokolování s malé balíků</span><span class="sxs-lookup"><span data-stu-id="78eb4-199">Minimize logging with small batches</span></span>
<span data-ttu-id="78eb4-200">Pro operace úpravy velkého množství dat může být smysl operaci toodivide hello do bloků dat nebo dávek tooscope hello jednotky práce.</span><span class="sxs-lookup"><span data-stu-id="78eb4-200">For large data modification operations, it may make sense toodivide hello operation into chunks or batches tooscope hello unit of work.</span></span>

<span data-ttu-id="78eb4-201">Příklad pracovní najdete níže.</span><span class="sxs-lookup"><span data-stu-id="78eb4-201">A working example is provided below.</span></span> <span data-ttu-id="78eb4-202">velikost dávky Hello nastavený tooa trivial číslo toohighlight hello techniku.</span><span class="sxs-lookup"><span data-stu-id="78eb4-202">hello batch size has been set tooa trivial number toohighlight hello technique.</span></span> <span data-ttu-id="78eb4-203">Ve skutečnosti bude podstatně větší velikost dávky hello.</span><span class="sxs-lookup"><span data-stu-id="78eb4-203">In reality hello batch size would be significantly larger.</span></span> 

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

## <a name="pause-and-scaling-guidance"></a><span data-ttu-id="78eb4-204">Pozastavení a škálování pokyny</span><span class="sxs-lookup"><span data-stu-id="78eb4-204">Pause and scaling guidance</span></span>
<span data-ttu-id="78eb4-205">Azure SQL Data Warehouse umožňuje pozastavit, obnovit a škálovat datový sklad na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="78eb4-205">Azure SQL Data Warehouse lets you pause, resume and scale your data warehouse on demand.</span></span> <span data-ttu-id="78eb4-206">Při pozastavení nebo škálovat datový sklad SQL je důležité toounderstand, že jakékoli během letu transakce budou ukončeny okamžitě; způsobuje toobe všechny otevřené transakce vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="78eb4-206">When you pause or scale your SQL Data Warehouse it is important toounderstand that any in-flight transactions are terminated immediately; causing any open transactions toobe rolled back.</span></span> <span data-ttu-id="78eb4-207">Pokud vaše úlohy vystavila dlouho běžící a úpravy neúplná data předchozí toohello pozastavení nebo operaci škálování a potom tento pracovní potřebovat toobe odvolat.</span><span class="sxs-lookup"><span data-stu-id="78eb4-207">If your workload had issued a long running and incomplete data modification prior toohello pause or scale operation, then this work will need toobe undone.</span></span> <span data-ttu-id="78eb4-208">To může mít vliv na hello doba trvání toopause nebo škálování databáze Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="78eb4-208">This may impact hello time it takes toopause or scale your Azure SQL Data Warehouse database.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="78eb4-209">Obě `UPDATE` a `DELETE` jsou plně protokolovaných operací a tak tyto zpět/opakování operace může trvat výrazně déle, než ekvivalentní minimálně protokolována operace.</span><span class="sxs-lookup"><span data-stu-id="78eb4-209">Both `UPDATE` and `DELETE` are fully logged operations and so these undo/redo operations can take significantly longer than equivalent minimally logged operations.</span></span> 
> 
> 

<span data-ttu-id="78eb4-210">scénář Nejlepší Hello je toolet v cestě data změny transakce v dokončení předchozí toopausing nebo škálování SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="78eb4-210">hello best scenario is toolet in flight data modification transactions complete prior toopausing or scaling SQL Data Warehouse.</span></span> <span data-ttu-id="78eb4-211">Ale to nemusí být vždy praktické.</span><span class="sxs-lookup"><span data-stu-id="78eb4-211">However, this may not always be practical.</span></span> <span data-ttu-id="78eb4-212">riziko hello toomitigate dlouho vrácení zpět, vezměte v úvahu mezi hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="78eb4-212">toomitigate hello risk of a long rollback, consider one of hello following options:</span></span>

* <span data-ttu-id="78eb4-213">Znovu zapsat dlouhotrvající operace pomocí [funkce CTAS][CTAS]</span><span class="sxs-lookup"><span data-stu-id="78eb4-213">Re-write long running operations using [CTAS][CTAS]</span></span>
* <span data-ttu-id="78eb4-214">Operace hello rozdělení do bloků; pracující na podmnožinu řádků hello</span><span class="sxs-lookup"><span data-stu-id="78eb4-214">Break hello operation down into chunks; operating on a subset of hello rows</span></span>

## <a name="next-steps"></a><span data-ttu-id="78eb4-215">Další kroky</span><span class="sxs-lookup"><span data-stu-id="78eb4-215">Next steps</span></span>
<span data-ttu-id="78eb4-216">V tématu [transakcí v SQL Data Warehouse] [ Transactions in SQL Data Warehouse] toolearn Další informace o úrovních izolace a omezení transakcí.</span><span class="sxs-lookup"><span data-stu-id="78eb4-216">See [Transactions in SQL Data Warehouse][Transactions in SQL Data Warehouse] toolearn more about isolation levels and transactional limits.</span></span>  <span data-ttu-id="78eb4-217">Přehled ostatní osvědčené postupy najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="78eb4-217">For an overview of other Best Practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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

