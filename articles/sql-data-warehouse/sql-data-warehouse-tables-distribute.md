---
title: Distribuce tabulek v SQL Data Warehouse | Microsoft Docs
description: "Začínáme s distribuci tabulek v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 5ed4337f-7262-4ef6-8fd6-1809ce9634fc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: d0e12bf821a81826a20b8db84e76c48fa60ad9b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a><span data-ttu-id="87fab-103">Distribuce tabulek v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="87fab-103">Distributing tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="87fab-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="87fab-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="87fab-105">[Datové typy][Data Types]</span><span class="sxs-lookup"><span data-stu-id="87fab-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="87fab-106">[Distribuce][Distribute]</span><span class="sxs-lookup"><span data-stu-id="87fab-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="87fab-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="87fab-107">[Index][Index]</span></span>
> * <span data-ttu-id="87fab-108">[Oddíl][Partition]</span><span class="sxs-lookup"><span data-stu-id="87fab-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="87fab-109">[Statistiky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="87fab-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="87fab-110">[Dočasné][Temporary]</span><span class="sxs-lookup"><span data-stu-id="87fab-110">[Temporary][Temporary]</span></span>
>
>

<span data-ttu-id="87fab-111">Řešení SQL Data Warehouse je distribuovaný databázový systém, postavený na architektuře MPP (Massively Parallel Processing).</span><span class="sxs-lookup"><span data-stu-id="87fab-111">SQL Data Warehouse is a massively parallel processing (MPP) distributed database system.</span></span>  <span data-ttu-id="87fab-112">SQL Data Warehouse nabízí jedinečnou škálovatelnost, daleko za možnostmi jediného systému, a to díky dělení dat a schopnosti zpracování napříč více uzly.</span><span class="sxs-lookup"><span data-stu-id="87fab-112">By dividing data and processing capability across multiple nodes, SQL Data Warehouse can offer huge scalability - far beyond any single system.</span></span>  <span data-ttu-id="87fab-113">Rozhodování, jak se bude distribuovat svá data do SQL Data Warehouse je jedním z nejdůležitějších faktorů k dosažení optimálního výkonu.</span><span class="sxs-lookup"><span data-stu-id="87fab-113">Deciding how to distribute your data within your SQL Data Warehouse is one of the most important factors to achieving optimal performance.</span></span>   <span data-ttu-id="87fab-114">Klíč pro optimální výkon je minimalizaci přesun dat a pak klíč k minimalizaci přesun dat je výběr správné distribuční strategie.</span><span class="sxs-lookup"><span data-stu-id="87fab-114">The key to optimal performance is minimizing data movement and in turn the key to minimizing data movement is selecting the right distribution strategy.</span></span>

## <a name="understanding-data-movement"></a><span data-ttu-id="87fab-115">Princip přesunu dat</span><span class="sxs-lookup"><span data-stu-id="87fab-115">Understanding data movement</span></span>
<span data-ttu-id="87fab-116">V systému MPP data z tabulek rozděluje do několika základní databáze.</span><span class="sxs-lookup"><span data-stu-id="87fab-116">In an MPP system, the data from each table is divided across several underlying databases.</span></span>  <span data-ttu-id="87fab-117">Většina optimalizované dotazy v systému MPP můžete jednoduše předána ke spouštění na jednotlivé distribuované databáze bez jakéhokoli zásahu mezi ostatní databáze.</span><span class="sxs-lookup"><span data-stu-id="87fab-117">The most optimized queries on an MPP system can simply be passed through to execute on the individual distributed databases with no interaction between the other databases.</span></span>  <span data-ttu-id="87fab-118">Řekněme například, že máte databázi s prodejní data, která obsahuje dvě tabulky, prodej a zákazníků.</span><span class="sxs-lookup"><span data-stu-id="87fab-118">For example, let's say you have a database with sales data which contains two tables, sales and customers.</span></span>  <span data-ttu-id="87fab-119">Pokud máte dotaz, který potřebuje ke spojení vaší prodeje tabulky pro tabulku zákazníků a budete provádět dělení prodeje a tabulky zákazníka až po zákaznické číslo uvedení každého zákazníka a to v samostatné databáze, bude možné v rámci každou databázi bez znalosti ostatní databáze vyřešit všechny dotazy, které připojení prodeje a zákazníka.</span><span class="sxs-lookup"><span data-stu-id="87fab-119">If you have a query that needs to join your sales table to your customer table and you divide both your sales and customer tables up by customer number, putting each customer in a separate database, any queries which join sales and customer can be solved within each database with no knowledge of the other databases.</span></span>  <span data-ttu-id="87fab-120">Naproti tomu Pokud vaše data prodeje dělený pořadové číslo a zákaznických údajů číslem zákazníka, pak všechny danou databázi nebude mít odpovídající data pro každého zákazníka a proto pokud jste chtěli připojit vaše data prodeje k zákaznická data, potřebujete získat data pro každého zákazníka a to z jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="87fab-120">In contrast, if you divided your sales data by order number and your customer data by customer number, then any given database will not have the corresponding data for each customer and thus if you wanted to join your sales data to your customer data, you would need to get the data for each customer from the other databases.</span></span>  <span data-ttu-id="87fab-121">V tomto příkladu druhý přesun dat potřebovat proběhnout pro přesun dat zákazníka do data z prodeje, takže lze propojit dvě tabulky.</span><span class="sxs-lookup"><span data-stu-id="87fab-121">In this second example, data movement would need to occur to move the customer data to the sales data, so that the two tables can be joined.</span></span>  

<span data-ttu-id="87fab-122">Přesun dat není vždy chybný věcí, někdy je nezbytné k vyřešení dotazu.</span><span class="sxs-lookup"><span data-stu-id="87fab-122">Data movement isn't always a bad thing, sometimes it's necessary to solve a query.</span></span>  <span data-ttu-id="87fab-123">Ale když tento další krok se vyhnout, přirozeně dotazu bude pracovat rychleji.</span><span class="sxs-lookup"><span data-stu-id="87fab-123">But when this extra step can be avoided, naturally your query will run faster.</span></span>  <span data-ttu-id="87fab-124">Přesun dat nejčastěji mohou nastat, pokud jsou připojené k tabulky nebo agregace se provádí.</span><span class="sxs-lookup"><span data-stu-id="87fab-124">Data Movement most commonly arises when tables are joined or aggregations are performed.</span></span>  <span data-ttu-id="87fab-125">Často je potřeba udělat i, takže když bude pravděpodobně možné optimalizovat pro jeden scénář, jako jsou spojení, budete potřebovat přesun dat, které vám pomůžou vyřešit v situaci, jako je agregace.</span><span class="sxs-lookup"><span data-stu-id="87fab-125">Often you need to do both, so while you may be able to optimize for one scenario, like a join, you still need data movement to help you solve for the other scenario, like an aggregation.</span></span>  <span data-ttu-id="87fab-126">Efektu je přijít na to, což je méně práce.</span><span class="sxs-lookup"><span data-stu-id="87fab-126">The trick is figuring out which is less work.</span></span>  <span data-ttu-id="87fab-127">Ve většině případů je distribuce tabulky faktů velký na běžně připojené k sloupci co nejúčinnější metodu pro snížení většina přesun dat.</span><span class="sxs-lookup"><span data-stu-id="87fab-127">In most cases, distributing large fact tables on a commonly joined column is the most effective method for reducing the most data movement.</span></span>  <span data-ttu-id="87fab-128">Distribuci dat na sloupce spojení je mnohem víc běžnou metodu omezit přesun dat než distribuci dat na sloupce, které se účastní agregace.</span><span class="sxs-lookup"><span data-stu-id="87fab-128">Distributing data on join columns is a much more common method to reduce data movement than distributing data on columns involved in an aggregation.</span></span>

## <a name="select-distribution-method"></a><span data-ttu-id="87fab-129">Vyberte možnost distribution – metoda</span><span class="sxs-lookup"><span data-stu-id="87fab-129">Select distribution method</span></span>
<span data-ttu-id="87fab-130">SQL Data Warehouse na pozadí rozděluje data do 60 databáze.</span><span class="sxs-lookup"><span data-stu-id="87fab-130">Behind the scenes, SQL Data Warehouse divides your data into 60 databases.</span></span>  <span data-ttu-id="87fab-131">Každé jednotlivé databáze odkazuje jako **distribuční**.</span><span class="sxs-lookup"><span data-stu-id="87fab-131">Each individual database is referred to as a **distribution**.</span></span>  <span data-ttu-id="87fab-132">Při načítání dat do tabulek SQL Data Warehouse musí vědět, jak k rozdělení dat mezi těmito 60 distribuce.</span><span class="sxs-lookup"><span data-stu-id="87fab-132">When data is loaded into each table, SQL Data Warehouse has to know how to divide your data across these 60 distributions.</span></span>  

<span data-ttu-id="87fab-133">Metoda distribuční definovanou na úrovni tabulky a aktuálně existují dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="87fab-133">The distribution method is defined at the table level and currently there are two choices:</span></span>

1. <span data-ttu-id="87fab-134">**Kruhové dotazování** který distribuci dat rovnoměrně ale náhodně.</span><span class="sxs-lookup"><span data-stu-id="87fab-134">**Round robin** which distribute data evenly but randomly.</span></span>
2. <span data-ttu-id="87fab-135">**Hodnoty hash distribuované** která distribuuje data podle algoritmu hash hodnoty z jednoho sloupce</span><span class="sxs-lookup"><span data-stu-id="87fab-135">**Hash Distributed** which distributes data based on hashing values from a single column</span></span>

<span data-ttu-id="87fab-136">Ve výchozím nastavení, když nejsou definovány způsobu distribuce dat, tabulka budou distribuována pomocí **kruhové dotazování** na metodu.</span><span class="sxs-lookup"><span data-stu-id="87fab-136">By default, when you do not define a data distribution method, your table will be distributed using the **round robin** distribution method.</span></span>  <span data-ttu-id="87fab-137">Ale protože je stále sofistikovanější v implementaci, budete chtít zvažte použití **hash distribuované** dotaz tabulky minimalizovat přesun dat, která bude zase optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="87fab-137">However, as you become more sophisticated in your implementation, you will want to consider using **hash distributed** tables to minimize data movement which will in turn optimize query performance.</span></span>

### <a name="round-robin-tables"></a><span data-ttu-id="87fab-138">Kruhové dotazování tabulky</span><span class="sxs-lookup"><span data-stu-id="87fab-138">Round Robin Tables</span></span>
<span data-ttu-id="87fab-139">Pomocí metody kruhové dotazování distribuci dat je velmi dobře v tom, jak ho vyznívá.</span><span class="sxs-lookup"><span data-stu-id="87fab-139">Using the Round Robin method of distributing data is very much how it sounds.</span></span>  <span data-ttu-id="87fab-140">Podle vašich dat je načtena, každý řádek jednoduše posílá další distribuci.</span><span class="sxs-lookup"><span data-stu-id="87fab-140">As your data is loaded, each row is simply sent to the next distribution.</span></span>  <span data-ttu-id="87fab-141">Tato metoda distribuci dat bude vždy náhodně distribuovat data velmi rovnoměrně napříč všemi z rozdělení.</span><span class="sxs-lookup"><span data-stu-id="87fab-141">This method of distributing the data will always randomly distribute the data very evenly across all of the distributions.</span></span>  <span data-ttu-id="87fab-142">To znamená, že neexistuje žádné řazení provést během procesu kruhové dotazování, který nahradí vaše data.</span><span class="sxs-lookup"><span data-stu-id="87fab-142">That is, there is no sorting done during the round robin process which places your data.</span></span>  <span data-ttu-id="87fab-143">Kruhové dotazování distribuční se někdy nazývá náhodných hash z tohoto důvodu.</span><span class="sxs-lookup"><span data-stu-id="87fab-143">A round robin distribution is sometimes called a random hash for this reason.</span></span>  <span data-ttu-id="87fab-144">S tabulkou distribuované kruhového dotazování je potřeba pochopit data.</span><span class="sxs-lookup"><span data-stu-id="87fab-144">With a round-robin distributed table there is no need to understand the data.</span></span>  <span data-ttu-id="87fab-145">Z tohoto důvodu kruhového dotazování tabulky často je dobré načítání cíle.</span><span class="sxs-lookup"><span data-stu-id="87fab-145">For this reason, Round-Robin tables often make good loading targets.</span></span>

<span data-ttu-id="87fab-146">Ve výchozím nastavení pokud je vybrána žádná metoda distribuce, metodu distribuce kruhové dotazování se použije.</span><span class="sxs-lookup"><span data-stu-id="87fab-146">By default, if no distribution method is chosen, the round robin distribution method will be used.</span></span>  <span data-ttu-id="87fab-147">Při kruhové dotazování tabulky lze snadno použít, protože data se náhodně distribuuje do systému, které znamená, že systém nemůže zaručit, který distribuční každý řádek je však na.</span><span class="sxs-lookup"><span data-stu-id="87fab-147">However, while round robin tables are easy to use, because data is randomly distributed across the system it means that the system can't guarantee which distribution each row is on.</span></span>  <span data-ttu-id="87fab-148">V důsledku toho systém někdy potřebuje k vyvolání operace přesunu dat lepší uspořádání dat před překlad dotazu.</span><span class="sxs-lookup"><span data-stu-id="87fab-148">As a result, the system sometimes needs to invoke a data movement operation to better organize your data before it can resolve a query.</span></span>  <span data-ttu-id="87fab-149">Tento krok navíc může zpomalit své dotazy.</span><span class="sxs-lookup"><span data-stu-id="87fab-149">This extra step can slow down your queries.</span></span>

<span data-ttu-id="87fab-150">Zvažte použití kruhové dotazování distribuce pro tabulku v následujících scénářích:</span><span class="sxs-lookup"><span data-stu-id="87fab-150">Consider using Round Robin distribution for your table in the following scenarios:</span></span>

* <span data-ttu-id="87fab-151">Pro začátek jednoduché počáteční bod</span><span class="sxs-lookup"><span data-stu-id="87fab-151">When getting started as a simple starting point</span></span>
* <span data-ttu-id="87fab-152">Pokud neexistuje zřejmé spojující klíč</span><span class="sxs-lookup"><span data-stu-id="87fab-152">If there is no obvious joining key</span></span>
* <span data-ttu-id="87fab-153">Pokud není k dispozici vhodným kandidátem sloupce pro hodnoty hash distribuce tabulky</span><span class="sxs-lookup"><span data-stu-id="87fab-153">If there is not good candidate column for hash distributing the table</span></span>
* <span data-ttu-id="87fab-154">Pokud v tabulce nesdílí společný klíč spojení s jinými tabulkami</span><span class="sxs-lookup"><span data-stu-id="87fab-154">If the table does not share a common join key with other tables</span></span>
* <span data-ttu-id="87fab-155">Pokud je méně důležité než jiné spojení v dotazu spojení</span><span class="sxs-lookup"><span data-stu-id="87fab-155">If the join is less significant than other joins in the query</span></span>
* <span data-ttu-id="87fab-156">Pokud je tabulka dočasné pracovní tabulky</span><span class="sxs-lookup"><span data-stu-id="87fab-156">When the table is a temporary staging table</span></span>

<span data-ttu-id="87fab-157">Obě tyto příklady vytvoří tabulku, kruhové dotazování:</span><span class="sxs-lookup"><span data-stu-id="87fab-157">Both of these examples will create a Round Robin Table:</span></span>

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [!NOTE]
> <span data-ttu-id="87fab-158">Pokud kruhové dotazování je, že výchozí typ tabulky se explicitní v DDL je považovat za osvědčený postup, aby byly vymazat jiným záměry rozložení tabulky.</span><span class="sxs-lookup"><span data-stu-id="87fab-158">While round robin is the default table type being explicit in your DDL is considered a best practice so that the intentions of your table layout are clear to others.</span></span>
>
>

### <a name="hash-distributed-tables"></a><span data-ttu-id="87fab-159">Hodnoty hash distribuované tabulky</span><span class="sxs-lookup"><span data-stu-id="87fab-159">Hash Distributed Tables</span></span>
<span data-ttu-id="87fab-160">Použití **Hash distribuované** algoritmus distribuovat vaše tabulky můžete zlepšit výkon pro mnoho scénářů snížením přesun dat v době dotazu.</span><span class="sxs-lookup"><span data-stu-id="87fab-160">Using a **Hash distributed** algorithm to distribute your tables can improve performance for many scenarios by reducing data movement at query time.</span></span>  <span data-ttu-id="87fab-161">Hodnota hash distribuované tabulky jsou tabulky, které jsou rozděleny mezi distribuované databáze pomocí algoritmu hash na jeden sloupec, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="87fab-161">Hash distributed tables are tables which are divided between the distributed databases using a hashing algorithm on a single column which you select.</span></span>  <span data-ttu-id="87fab-162">Sloupec distribuce je co určuje, jak je rozdělit data do distribuované databáze.</span><span class="sxs-lookup"><span data-stu-id="87fab-162">The distribution column is what determines how the data is divided across your distributed databases.</span></span>  <span data-ttu-id="87fab-163">Funkce hash používá sloupec distribuční přiřadit distribuce řádků.</span><span class="sxs-lookup"><span data-stu-id="87fab-163">The hash function uses the distribution column to assign rows to distributions.</span></span>  <span data-ttu-id="87fab-164">Algoritmus hash a výsledný distribuce je deterministický.</span><span class="sxs-lookup"><span data-stu-id="87fab-164">The hashing algorithm and resulting distribution is deterministic.</span></span>  <span data-ttu-id="87fab-165">To je, bude má vždy stejnou hodnotu stejného typu dat do stejné distribuce.</span><span class="sxs-lookup"><span data-stu-id="87fab-165">That is the same value with the same data type will always has to the same distribution.</span></span>    

<span data-ttu-id="87fab-166">Tento příklad vytvoří tabulku rozdělit na id:</span><span class="sxs-lookup"><span data-stu-id="87fab-166">This example will create a table distributed on id:</span></span>

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a><span data-ttu-id="87fab-167">Vybrat distribuční sloupce</span><span class="sxs-lookup"><span data-stu-id="87fab-167">Select distribution column</span></span>
<span data-ttu-id="87fab-168">Pokud zvolíte **distribuovat algoritmu hash** tabulku, budete muset vybrat jednoho distribučního sloupec.</span><span class="sxs-lookup"><span data-stu-id="87fab-168">When you choose to **hash distribute** a table, you will need to select a single distribution column.</span></span>  <span data-ttu-id="87fab-169">Když vyberete distribuční sloupce, existují tři hlavní faktory vzít v úvahu.</span><span class="sxs-lookup"><span data-stu-id="87fab-169">When selecting a distribution column, there are three major factors to consider.</span></span>  

<span data-ttu-id="87fab-170">Vyberte jeden sloupec, který bude:</span><span class="sxs-lookup"><span data-stu-id="87fab-170">Select a single column which will:</span></span>

1. <span data-ttu-id="87fab-171">Není možné aktualizovat</span><span class="sxs-lookup"><span data-stu-id="87fab-171">Not be updated</span></span>
2. <span data-ttu-id="87fab-172">Distribuci dat rovnoměrně, zabraňující zkosení dat</span><span class="sxs-lookup"><span data-stu-id="87fab-172">Distribute data evenly, avoiding data skew</span></span>
3. <span data-ttu-id="87fab-173">Minimalizovat přesun dat</span><span class="sxs-lookup"><span data-stu-id="87fab-173">Minimize data movement</span></span>

### <a name="select-distribution-column-which-will-not-be-updated"></a><span data-ttu-id="87fab-174">Vybrat distribuční sloupec, který nebude aktualizován</span><span class="sxs-lookup"><span data-stu-id="87fab-174">Select distribution column which will not be updated</span></span>
<span data-ttu-id="87fab-175">Distribuční sloupce nejsou aktualizovat, proto, vyberte sloupec s statické hodnoty.</span><span class="sxs-lookup"><span data-stu-id="87fab-175">Distribution columns are not updatable, therefore, select a column with static values.</span></span>  <span data-ttu-id="87fab-176">Pokud sloupec bude potřeba aktualizovat, není obecně správné distribuční candidate.</span><span class="sxs-lookup"><span data-stu-id="87fab-176">If a column will need to be updated, it is generally not a good distribution candidate.</span></span>  <span data-ttu-id="87fab-177">Pokud je případ, kde je nutné aktualizovat distribuční sloupce, můžete to provést nejprve odstranit řádek a následného vložení nového řádku.</span><span class="sxs-lookup"><span data-stu-id="87fab-177">If there is a case where you must update a distribution column, this can be done by first deleting the row and then inserting a new row.</span></span>

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a><span data-ttu-id="87fab-178">Vybrat distribuční sloupec, který bude rovnoměrně distribuovat data</span><span class="sxs-lookup"><span data-stu-id="87fab-178">Select distribution column which will distribute data evenly</span></span>
<span data-ttu-id="87fab-179">Vzhledem k tomu, že distribuovaného systému provádí pouze rychlé jako jeho nejpomalejší distribuci, je důležité rozdělte práci rovnoměrně mezi distribucí dosáhnout vyrovnáváním provádění v systému.</span><span class="sxs-lookup"><span data-stu-id="87fab-179">Since a distributed system performs only as fast as its slowest distribution, it is important to divide the work evenly across the distributions in order to achieve balanced execution across the system.</span></span>  <span data-ttu-id="87fab-180">Způsob práce je rozdělena na distribuovaného systému je založena na tam, kde data pro každý distribuční platný.</span><span class="sxs-lookup"><span data-stu-id="87fab-180">The way the work is divided on a distributed system is based on where the data for each distribution lives.</span></span>  <span data-ttu-id="87fab-181">Díky tomu je velmi důležité vybrat sloupci vpravo distribuce pro distribuci dat tak, aby každý distribuční má stejnou práci a bude trvat stejnou dobu pro dokončení jeho část práce.</span><span class="sxs-lookup"><span data-stu-id="87fab-181">This makes it very important to select the right distribution column for distributing the data so that each distribution has equal work and will take the same time to complete its portion of the work.</span></span>  <span data-ttu-id="87fab-182">Při práci a rozděluje do systému, dat jsou rovnoměrně mezi distribucí.</span><span class="sxs-lookup"><span data-stu-id="87fab-182">When work is well divided across the system, the data is balanced across the distributions.</span></span>  <span data-ttu-id="87fab-183">Když data není vyrovnáváním rovnoměrně, říkáme to **data zkosení**.</span><span class="sxs-lookup"><span data-stu-id="87fab-183">When data is not evenly balanced, we call this **data skew**.</span></span>  

<span data-ttu-id="87fab-184">Rozdělení dat rovnoměrně a zamezit tak data zkosení, zvažte tyto informace při výběru vaší distribuční sloupce:</span><span class="sxs-lookup"><span data-stu-id="87fab-184">To divide data evenly and avoid data skew, consider the following when selecting your distribution column:</span></span>

1. <span data-ttu-id="87fab-185">Vyberte sloupce, který obsahuje velký počet jedinečných hodnot.</span><span class="sxs-lookup"><span data-stu-id="87fab-185">Select a column which contains a significant number of distinct values.</span></span>
2. <span data-ttu-id="87fab-186">Vyhněte se distribuci dat na sloupce s několika jedinečných hodnot.</span><span class="sxs-lookup"><span data-stu-id="87fab-186">Avoid distributing data on columns with a few distinct values.</span></span>
3. <span data-ttu-id="87fab-187">Vyhněte se distribuci dat na sloupce s vysoká frekvence hodnot Null.</span><span class="sxs-lookup"><span data-stu-id="87fab-187">Avoid distributing data on columns with a high frequency of nulls.</span></span>
4. <span data-ttu-id="87fab-188">Vyhněte se distribuci dat na sloupců s kalendářními daty.</span><span class="sxs-lookup"><span data-stu-id="87fab-188">Avoid distributing data on date columns.</span></span>

<span data-ttu-id="87fab-189">Vzhledem k tomu, že každá hodnota se rozdělí na 1 60 distribuce a zajistit rovnoměrné rozdělení budete chtít vyberte sloupec, který je vysoce jedinečný a obsahuje více než 60 jedinečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="87fab-189">Since each value is hashed to 1 of 60 distributions, to achieve even distribution you will want to select a column that is highly unique and contains more than 60 unique values.</span></span>  <span data-ttu-id="87fab-190">Pro ilustraci, představte si případ, kdy sloupec má jenom 40 jedinečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="87fab-190">To illustrate, consider a case where a column only has 40 unique values.</span></span>  <span data-ttu-id="87fab-191">Pokud v tomto sloupci jste vybrali jako distribučního klíče, data pro tuto tabulku by nebude zobrazovat na 40 distribuce maximálně ponechat 20 distribuce se žádná data a žádné zpracování udělat.</span><span class="sxs-lookup"><span data-stu-id="87fab-191">If this column was selected as the distribution key, the data for that table would land on 40 distributions at most, leaving 20 distributions with no data and no processing to do.</span></span>  <span data-ttu-id="87fab-192">Naopak dalších 40 distribuce by měla mít více práce k tomu, pokud data byla rovnoměrně šířit přes 60 distribuce.</span><span class="sxs-lookup"><span data-stu-id="87fab-192">Conversely, the other 40 distributions would have more work to do that if the data was evenly spread over 60 distributions.</span></span>  <span data-ttu-id="87fab-193">Tento scénář je příkladem zkosení data.</span><span class="sxs-lookup"><span data-stu-id="87fab-193">This scenario is an example of data skew.</span></span>

<span data-ttu-id="87fab-194">V systému MPP každého kroku dotazu čeká všechny distribuce pro dokončení jejich podíl práce.</span><span class="sxs-lookup"><span data-stu-id="87fab-194">In MPP system, each query step waits for all distributions to complete their share of the work.</span></span>  <span data-ttu-id="87fab-195">Pokud do jednoho distribučního provádí další práci než ostatní, pak prostředku jiných distribuce jsou v podstatě ke znehodnocení části právě čekání na zaneprázdněn rozdělení.</span><span class="sxs-lookup"><span data-stu-id="87fab-195">If one distribution is doing more work than the others, then the resource of the other distributions are essentially wasted just waiting on the busy distribution.</span></span>  <span data-ttu-id="87fab-196">Při práci není rovnoměrně rozloženy všechny distribuce, říkáme to **zpracování zkosení**.</span><span class="sxs-lookup"><span data-stu-id="87fab-196">When work is not evenly spread across all distributions, we call this **processing skew**.</span></span>  <span data-ttu-id="87fab-197">Zpracování zkosení způsobí, že dotazy na pomalejší, než pokud zatížení může být rovnoměrně rozloženy distribucí.</span><span class="sxs-lookup"><span data-stu-id="87fab-197">Processing skew will cause queries to run slower than if the workload can be evenly spread across the distributions.</span></span>  <span data-ttu-id="87fab-198">Data zkosení povede k zkosení zpracování.</span><span class="sxs-lookup"><span data-stu-id="87fab-198">Data skew will lead to processing skew.</span></span>

<span data-ttu-id="87fab-199">Vyhněte se distribuci na vysoce sloupec jako hodnoty null bude zobrazovat na stejné rozdělení.</span><span class="sxs-lookup"><span data-stu-id="87fab-199">Avoid distributing on highly nullable column as the null values will all land on the same distribution.</span></span> <span data-ttu-id="87fab-200">Distribuce ve sloupci Datum může také způsobit zkosení zpracování, protože všechna data pro konkrétního data bude zobrazovat na stejný distribuční.</span><span class="sxs-lookup"><span data-stu-id="87fab-200">Distributing on a date column can also cause processing skew because all data for a given date will land on the same distribution.</span></span> <span data-ttu-id="87fab-201">Pokud několik uživatelů provádí dotazy veškeré filtrování ve stejný den, pak bude to jenom 1 60 distribuce, všechny práce od zadaného data bude pouze na jednom distribučním.</span><span class="sxs-lookup"><span data-stu-id="87fab-201">If several users are executing queries all filtering on the same date, then only 1 of the 60 distributions will be doing all of the work since a given date will only be on one distribution.</span></span> <span data-ttu-id="87fab-202">V tomto scénáři bude spuštění dotazů pravděpodobně 60 x pomalejší než pokud data byly rovnoměrně rozloženy všechny z rozdělení.</span><span class="sxs-lookup"><span data-stu-id="87fab-202">In this scenario, the queries will likely run 60 times slower than if the data were equally spread over all of the distributions.</span></span>

<span data-ttu-id="87fab-203">Pokud neexistují žádné sloupce vhodným kandidátem, můžete použít kruhové dotazování jako metodu distribuce.</span><span class="sxs-lookup"><span data-stu-id="87fab-203">When no good candidate columns exist, then consider using round robin as the distribution method.</span></span>

### <a name="select-distribution-column-which-will-minimize-data-movement"></a><span data-ttu-id="87fab-204">Vybrat distribuční sloupec, který bude minimalizovat přesun dat</span><span class="sxs-lookup"><span data-stu-id="87fab-204">Select distribution column which will minimize data movement</span></span>
<span data-ttu-id="87fab-205">Minimalizace přesun dat výběrem sloupci vpravo distribuce je jedním z nejdůležitějších strategie pro optimalizaci výkonu SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="87fab-205">Minimizing data movement by selecting the right distribution column is one of the most important strategies for optimizing performance of your SQL Data Warehouse.</span></span>  <span data-ttu-id="87fab-206">Přesun dat nejčastěji mohou nastat, pokud jsou připojené k tabulky nebo agregace se provádí.</span><span class="sxs-lookup"><span data-stu-id="87fab-206">Data Movement most commonly arises when tables are joined or aggregations are performed.</span></span>  <span data-ttu-id="87fab-207">Sloupce použité v `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` a `HAVING` klauzule pro všechny provedl **dobrý** hash kandidáty distribuce.</span><span class="sxs-lookup"><span data-stu-id="87fab-207">Columns used in `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` and `HAVING` clauses all make for **good** hash distribution candidates.</span></span>

<span data-ttu-id="87fab-208">Na druhé straně sloupců v `WHERE` klauzule proveďte **není** provést pro sloupec kandidátů na dobrý hash vzhledem k tomu, že omezit, které distribuce účast v dotazu, která způsobila zpracování zkosení.</span><span class="sxs-lookup"><span data-stu-id="87fab-208">On the other hand, columns in the `WHERE` clause do **not** make for good hash column candidates because they limit which distributions participate in the query, causing processing skew.</span></span>  <span data-ttu-id="87fab-209">Dobrým příkladem sloupce, který může být tempting distribuovat na, ale často může způsobit zpracování zkosení je sloupec datum.</span><span class="sxs-lookup"><span data-stu-id="87fab-209">A good example of a column which might be tempting to distribute on, but often can cause this processing skew is a date column.</span></span>

<span data-ttu-id="87fab-210">Obecně platí Pokud máte dvě tabulky faktů velké často zahrnutých ve spojení, bude získáte většina výkon distribucí obě tabulky v jednom ze sloupce spojení.</span><span class="sxs-lookup"><span data-stu-id="87fab-210">Generally speaking, if you have two large fact tables frequently involved in a join, you will gain the most performance by distributing both tables on one of the join columns.</span></span>  <span data-ttu-id="87fab-211">Pokud máte tabulku, která je nikdy připojený k jiné tabulky faktů velký, podívejte se na sloupce, které jsou často v `GROUP BY` klauzule.</span><span class="sxs-lookup"><span data-stu-id="87fab-211">If you have a table that is never joined to another large fact table, then look to columns that are frequently in the `GROUP BY` clause.</span></span>

<span data-ttu-id="87fab-212">Existuje několik klíčů kritérií, které musí splnit, aby se zabránilo přesun dat během spojení:</span><span class="sxs-lookup"><span data-stu-id="87fab-212">There are a few key criteria which must be met to avoid data movement during a join:</span></span>

1. <span data-ttu-id="87fab-213">Příslušné tabulky ve spojení musí být hodnota hash rozdělit na **jeden** sloupců podílejících se připojení.</span><span class="sxs-lookup"><span data-stu-id="87fab-213">The tables involved in the join must be hash distributed on **one** of the columns participating in the join.</span></span>
2. <span data-ttu-id="87fab-214">Datové typy sloupce spojení se musí shodovat mezi obě tabulky.</span><span class="sxs-lookup"><span data-stu-id="87fab-214">The data types of the join columns must match between both tables.</span></span>
3. <span data-ttu-id="87fab-215">Sloupce musí být připojené pomocí operátoru rovná se.</span><span class="sxs-lookup"><span data-stu-id="87fab-215">The columns must be joined with an equals operator.</span></span>
4. <span data-ttu-id="87fab-216">Typ spojení nesmí být `CROSS JOIN`.</span><span class="sxs-lookup"><span data-stu-id="87fab-216">The join type may not be a `CROSS JOIN`.</span></span>

## <a name="troubleshooting-data-skew"></a><span data-ttu-id="87fab-217">Řešení potíží s zkosení dat</span><span class="sxs-lookup"><span data-stu-id="87fab-217">Troubleshooting data skew</span></span>
<span data-ttu-id="87fab-218">Při distribuci dat tabulky pomocí metody distribuce hash existuje pravděpodobnost, že bude nesouměrně rozdělí některých distribucích nepřiměřeně více dat než jiné.</span><span class="sxs-lookup"><span data-stu-id="87fab-218">When table data is distributed using the hash distribution method there is a chance that some distributions will be skewed to have disproportionately more data than others.</span></span> <span data-ttu-id="87fab-219">Zkosení nadměrné dat může ovlivnit výkon dotazů, protože konečný výsledek distribuovaného dotazu musí čekat na nejdéle probíhající distribuce ukončíte.</span><span class="sxs-lookup"><span data-stu-id="87fab-219">Excessive data skew can impact query performance because the final result of a distributed query must wait for the longest running distribution to finish.</span></span> <span data-ttu-id="87fab-220">V závislosti na stupeň zkosení data možná budete muset vyřešit ji.</span><span class="sxs-lookup"><span data-stu-id="87fab-220">Depending on the degree of the data skew you might need to address it.</span></span>

### <a name="identifying-skew"></a><span data-ttu-id="87fab-221">Identifikace zkosení</span><span class="sxs-lookup"><span data-stu-id="87fab-221">Identifying skew</span></span>
<span data-ttu-id="87fab-222">Jednoduchý způsob, jak identifikovat tabulku jako nesouměrně rozdělí je použití `DBCC PDW_SHOWSPACEUSED`.</span><span class="sxs-lookup"><span data-stu-id="87fab-222">A simple way to identify a table as skewed is to use `DBCC PDW_SHOWSPACEUSED`.</span></span>  <span data-ttu-id="87fab-223">Toto je velmi rychlý a jednoduchý způsob, jak zobrazit počet řádků tabulky, které jsou uložené v každé z 60 distribuce vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="87fab-223">This is a very quick and simple way to see the number of table rows that are stored in each of the 60 distributions of your database.</span></span>  <span data-ttu-id="87fab-224">Nezapomeňte, že většina vyrovnáváním výkonu, řádky v tabulce distribuované by měl být rovnoměrně rozloženy všechny distribuce.</span><span class="sxs-lookup"><span data-stu-id="87fab-224">Remember that for the most balanced performance, the rows in your distributed table should be spread evenly across all the distributions.</span></span>

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

<span data-ttu-id="87fab-225">Pokud je dotaz na zobrazení dynamické správy Azure SQL Data Warehouse (DMV) však můžete provádět více podrobné analýzy.</span><span class="sxs-lookup"><span data-stu-id="87fab-225">However, if you query the Azure SQL Data Warehouse dynamic management views (DMV) you can perform a more detailed analysis.</span></span>  <span data-ttu-id="87fab-226">Pokud chcete spustit, vytvořte zobrazení [dbo.vTableSizes] [ dbo.vTableSizes] zobrazit pomocí SQL z [tabulky přehled] [ Overview] článku.</span><span class="sxs-lookup"><span data-stu-id="87fab-226">To start, create the view [dbo.vTableSizes][dbo.vTableSizes] view using the SQL from [Table Overview][Overview] article.</span></span>  <span data-ttu-id="87fab-227">Po vytvoření zobrazení, spusťte tento dotaz k identifikaci, které tabulky mít víc než 10 % data zkosení.</span><span class="sxs-lookup"><span data-stu-id="87fab-227">Once the view is created, run this query to identify which tables have more than 10% data skew.</span></span>

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a><span data-ttu-id="87fab-228">Řešení zkosení dat</span><span class="sxs-lookup"><span data-stu-id="87fab-228">Resolving data skew</span></span>
<span data-ttu-id="87fab-229">Ne všechny zkosení je dost pro vlastní opravu.</span><span class="sxs-lookup"><span data-stu-id="87fab-229">Not all skew is enough to warrant a fix.</span></span>  <span data-ttu-id="87fab-230">V některých případech může výkon tabulky v některé dotazy převáží nad škodu dat zkosení.</span><span class="sxs-lookup"><span data-stu-id="87fab-230">In some cases, the performance of a table in some queries can outweigh the harm of data skew.</span></span>  <span data-ttu-id="87fab-231">Rozhodnout, pokud byste měli vyřešit dat zkosení v tabulce, měli byste se seznámit co nejvíce o datové svazky a dotazy v vaše úlohy.</span><span class="sxs-lookup"><span data-stu-id="87fab-231">To decide if you should resolve data skew in a table, you should understand as much as possible about the data volumes and queries in your workload.</span></span>   <span data-ttu-id="87fab-232">Jedním ze způsobů podívat se na dopad zkosení je postupujte podle kroků v [dotazu monitorování] [ Query Monitoring] článek k monitorování dopad na výkon dotazů zkosení a konkrétně dopad na jak dlouho dotazuje trvat dokončení pro jednotlivé distribuce.</span><span class="sxs-lookup"><span data-stu-id="87fab-232">One way to look at the impact of skew is to use the steps in the [Query Monitoring][Query Monitoring] article to monitor the impact of skew on query performance and specifically the impact to how long queries take to complete on the individual distributions.</span></span>

<span data-ttu-id="87fab-233">Distribuci dat je řádu rovnováhu mezi minimalizaci zkosení dat a současně minimalizujete její přesun dat hledání.</span><span class="sxs-lookup"><span data-stu-id="87fab-233">Distributing data is a matter of finding the right balance between minimizing data skew and minimizing data movement.</span></span> <span data-ttu-id="87fab-234">To může být proti cíle a někdy budete chtít zachovat data zkosení kvůli snížení přesun dat.</span><span class="sxs-lookup"><span data-stu-id="87fab-234">These can be opposing goals, and sometimes you will want to keep data skew in order to reduce data movement.</span></span> <span data-ttu-id="87fab-235">Například pokud sloupci distribuce je často sloupci sdílené v spojování a agregaci, můžete se být minimalizace přesun dat.</span><span class="sxs-lookup"><span data-stu-id="87fab-235">For example, when the distribution column is frequently the shared column in joins and aggregations, you will be minimizing data movement.</span></span> <span data-ttu-id="87fab-236">Výhodou vytvoření přesun minimální dat může převažují nad důsledky toho, že data zkreslit.</span><span class="sxs-lookup"><span data-stu-id="87fab-236">The benefit of having the minimal data movement might outweigh the impact of having data skew.</span></span>

<span data-ttu-id="87fab-237">Typické způsob řešení zkosení dat je znovu vytvořit tabulku se sloupcem jiný distribuční.</span><span class="sxs-lookup"><span data-stu-id="87fab-237">The typical way to resolve data skew is to re-create the table with a different distribution column.</span></span> <span data-ttu-id="87fab-238">Vzhledem k tomu, že neexistuje žádný způsob, jak změnit distribuční sloupce na existující tabulky, způsob, jak změnit rozdělení tabulky se ho znovu vytvořit s [] [funkce CTAS].</span><span class="sxs-lookup"><span data-stu-id="87fab-238">Since there is no way to change the distribution column on an existing table, the way to change the distribution of a table it to recreate it with a [CTAS][].</span></span>  <span data-ttu-id="87fab-239">Zde jsou dva příklady, jak vyřešit data zkosení:</span><span class="sxs-lookup"><span data-stu-id="87fab-239">Here are two examples of how resolve data skew:</span></span>

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a><span data-ttu-id="87fab-240">Příklad 1: Opětovným vytvořením tabulky se sloupcem nové distribuce</span><span class="sxs-lookup"><span data-stu-id="87fab-240">Example 1: Re-create the table with a new distribution column</span></span>
<span data-ttu-id="87fab-241">Tento příklad používá [] – [funkce CTAS] a znovu vytvořte tabulku se sloupcem distribuční různé hodnoty hash.</span><span class="sxs-lookup"><span data-stu-id="87fab-241">This example uses [CTAS][] to re-create a table with a different hash distribution column.</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a><span data-ttu-id="87fab-242">Příklad 2: Opětovným vytvořením tabulky pomocí distribuční kruhové dotazování</span><span class="sxs-lookup"><span data-stu-id="87fab-242">Example 2: Re-create the table using round robin distribution</span></span>
<span data-ttu-id="87fab-243">Tento příklad používá [] – [funkce CTAS] a znovu vytvořte tabulku s každým místo hash distribuce.</span><span class="sxs-lookup"><span data-stu-id="87fab-243">This example uses [CTAS][] to re-create a table with round robin instead of a hash distribution.</span></span> <span data-ttu-id="87fab-244">Tato změna způsobí i distribuci dat za cenu přesun dat vyšší.</span><span class="sxs-lookup"><span data-stu-id="87fab-244">This change will produce even data distribution at the cost of increased data movement.</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a><span data-ttu-id="87fab-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87fab-245">Next steps</span></span>
<span data-ttu-id="87fab-246">Další informace o návrh tabulky, najdete v článku [distribuovat][Distribute], [Index][Index], [oddílu][Partition], [datové typy][Data Types], [statistiky] [ Statistics] a [dočasných tabulek] [ Temporary] články.</span><span class="sxs-lookup"><span data-stu-id="87fab-246">To learn more about table design, see the [Distribute][Distribute], [Index][Index], [Partition][Partition], [Data Types][Data Types], [Statistics][Statistics] and [Temporary Tables][Temporary] articles.</span></span>

<span data-ttu-id="87fab-247">Přehled osvědčených postupů najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="87fab-247">For an overview of best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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
[Query Monitoring]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
