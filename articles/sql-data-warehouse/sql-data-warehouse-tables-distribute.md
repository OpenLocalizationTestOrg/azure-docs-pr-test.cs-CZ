---
title: aaaDistributing tabulek v SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 65093eeaeb00fef85aaa6070da2c976fed3f4bbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a><span data-ttu-id="d7241-103">Distribuce tabulek v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="d7241-103">Distributing tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="d7241-104">[Přehled][Overview]</span><span class="sxs-lookup"><span data-stu-id="d7241-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="d7241-105">[Datové typy][Data Types]</span><span class="sxs-lookup"><span data-stu-id="d7241-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="d7241-106">[Distribuce][Distribute]</span><span class="sxs-lookup"><span data-stu-id="d7241-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="d7241-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="d7241-107">[Index][Index]</span></span>
> * <span data-ttu-id="d7241-108">[Oddíl][Partition]</span><span class="sxs-lookup"><span data-stu-id="d7241-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="d7241-109">[Statistiky][Statistics]</span><span class="sxs-lookup"><span data-stu-id="d7241-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="d7241-110">[Dočasné][Temporary]</span><span class="sxs-lookup"><span data-stu-id="d7241-110">[Temporary][Temporary]</span></span>
>
>

<span data-ttu-id="d7241-111">Řešení SQL Data Warehouse je distribuovaný databázový systém, postavený na architektuře MPP (Massively Parallel Processing).</span><span class="sxs-lookup"><span data-stu-id="d7241-111">SQL Data Warehouse is a massively parallel processing (MPP) distributed database system.</span></span>  <span data-ttu-id="d7241-112">SQL Data Warehouse nabízí jedinečnou škálovatelnost, daleko za možnostmi jediného systému, a to díky dělení dat a schopnosti zpracování napříč více uzly.</span><span class="sxs-lookup"><span data-stu-id="d7241-112">By dividing data and processing capability across multiple nodes, SQL Data Warehouse can offer huge scalability - far beyond any single system.</span></span>  <span data-ttu-id="d7241-113">Při rozhodování o tom, jak toodistribute svá data do SQL Data Warehouse je jedním z nejdůležitějších hello faktory tooachieving optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="d7241-113">Deciding how toodistribute your data within your SQL Data Warehouse is one of hello most important factors tooachieving optimal performance.</span></span>   <span data-ttu-id="d7241-114">Hello klíče toooptimal výkonu je minimalizaci přesun dat a naopak je přesun dat klíče toominimizing hello Výběr strategie správné distribuční hello.</span><span class="sxs-lookup"><span data-stu-id="d7241-114">hello key toooptimal performance is minimizing data movement and in turn hello key toominimizing data movement is selecting hello right distribution strategy.</span></span>

## <a name="understanding-data-movement"></a><span data-ttu-id="d7241-115">Princip přesunu dat</span><span class="sxs-lookup"><span data-stu-id="d7241-115">Understanding data movement</span></span>
<span data-ttu-id="d7241-116">V systému MPP hello data z tabulek rozděluje do několika základní databáze.</span><span class="sxs-lookup"><span data-stu-id="d7241-116">In an MPP system, hello data from each table is divided across several underlying databases.</span></span>  <span data-ttu-id="d7241-117">Hello nejvíce optimalizované dotazy v systému MPP lze předat jednoduše prostřednictvím tooexecute na hello jednotlivé distribuované databáze bez jakéhokoli zásahu mezi hello jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="d7241-117">hello most optimized queries on an MPP system can simply be passed through tooexecute on hello individual distributed databases with no interaction between hello other databases.</span></span>  <span data-ttu-id="d7241-118">Řekněme například, že máte databázi s prodejní data, která obsahuje dvě tabulky, prodej a zákazníků.</span><span class="sxs-lookup"><span data-stu-id="d7241-118">For example, let's say you have a database with sales data which contains two tables, sales and customers.</span></span>  <span data-ttu-id="d7241-119">Pokud máte dotaz, který potřebuje toojoin vašeho zákazníka tabulku tooyour prodeje a budete provádět dělení prodeje a tabulky zákazníka až po zákaznické číslo uvedení každého zákazníka a to v samostatné databáze, bude možné v každém vyřešit všechny dotazy, které připojení prodeje a zákazníka databáze s žádná znalost hello jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="d7241-119">If you have a query that needs toojoin your sales table tooyour customer table and you divide both your sales and customer tables up by customer number, putting each customer in a separate database, any queries which join sales and customer can be solved within each database with no knowledge of hello other databases.</span></span>  <span data-ttu-id="d7241-120">Naproti tomu Pokud vaše data prodeje dělený pořadové číslo a zákaznických údajů číslem zákazníka, pak všechny danou databázi nebude mít hello odpovídajících dat pro každého zákazníka a proto pokud jste chtěli toojoin zákaznických údajů tooyour data prodeje, potřebovali byste tooget hello dat pro každého zákazníka a to z hello jiné databáze.</span><span class="sxs-lookup"><span data-stu-id="d7241-120">In contrast, if you divided your sales data by order number and your customer data by customer number, then any given database will not have hello corresponding data for each customer and thus if you wanted toojoin your sales data tooyour customer data, you would need tooget hello data for each customer from hello other databases.</span></span>  <span data-ttu-id="d7241-121">V tomto příkladu druhý přesun dat potřebovat toooccur toomove hello data toohello prodejní data zákazníků, takže hello dvou tabulek je možné připojit.</span><span class="sxs-lookup"><span data-stu-id="d7241-121">In this second example, data movement would need toooccur toomove hello customer data toohello sales data, so that hello two tables can be joined.</span></span>  

<span data-ttu-id="d7241-122">Přesun dat není vždy chybný věcí, někdy je nezbytné toosolve dotazu.</span><span class="sxs-lookup"><span data-stu-id="d7241-122">Data movement isn't always a bad thing, sometimes it's necessary toosolve a query.</span></span>  <span data-ttu-id="d7241-123">Ale když tento další krok se vyhnout, přirozeně dotazu bude pracovat rychleji.</span><span class="sxs-lookup"><span data-stu-id="d7241-123">But when this extra step can be avoided, naturally your query will run faster.</span></span>  <span data-ttu-id="d7241-124">Přesun dat nejčastěji mohou nastat, pokud jsou připojené k tabulky nebo agregace se provádí.</span><span class="sxs-lookup"><span data-stu-id="d7241-124">Data Movement most commonly arises when tables are joined or aggregations are performed.</span></span>  <span data-ttu-id="d7241-125">Často musíte toodo obě, takže když bude pravděpodobně možné toooptimize pro jeden scénář, jako je připojení, je stále nutné toohelp přesun dat, které řešit pro hello další scénáře, jako je agregace.</span><span class="sxs-lookup"><span data-stu-id="d7241-125">Often you need toodo both, so while you may be able toooptimize for one scenario, like a join, you still need data movement toohelp you solve for hello other scenario, like an aggregation.</span></span>  <span data-ttu-id="d7241-126">Hello efektu je přijít na to, což je méně práce.</span><span class="sxs-lookup"><span data-stu-id="d7241-126">hello trick is figuring out which is less work.</span></span>  <span data-ttu-id="d7241-127">Ve většině případů distribuci tabulky faktů velký na běžně připojené k sloupci je hello co nejúčinnější metodu pro snížení hello většina přesun dat.</span><span class="sxs-lookup"><span data-stu-id="d7241-127">In most cases, distributing large fact tables on a commonly joined column is hello most effective method for reducing hello most data movement.</span></span>  <span data-ttu-id="d7241-128">Distribuci dat na sloupce spojení je přesun dat tooreduce metoda mnohem častější než distribuci dat na sloupce, které se účastní agregace.</span><span class="sxs-lookup"><span data-stu-id="d7241-128">Distributing data on join columns is a much more common method tooreduce data movement than distributing data on columns involved in an aggregation.</span></span>

## <a name="select-distribution-method"></a><span data-ttu-id="d7241-129">Vyberte možnost distribution – metoda</span><span class="sxs-lookup"><span data-stu-id="d7241-129">Select distribution method</span></span>
<span data-ttu-id="d7241-130">SQL Data Warehouse pozadí hello, rozděluje data do 60 databáze.</span><span class="sxs-lookup"><span data-stu-id="d7241-130">Behind hello scenes, SQL Data Warehouse divides your data into 60 databases.</span></span>  <span data-ttu-id="d7241-131">Každé jednotlivé databáze je odkazované tooas **distribuční**.</span><span class="sxs-lookup"><span data-stu-id="d7241-131">Each individual database is referred tooas a **distribution**.</span></span>  <span data-ttu-id="d7241-132">Při načítání dat do tabulek SQL Data Warehouse má tooknow jak toodivide vaše data v těchto 60 distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-132">When data is loaded into each table, SQL Data Warehouse has tooknow how toodivide your data across these 60 distributions.</span></span>  

<span data-ttu-id="d7241-133">Dobrý den distribution – metoda definovanou na úrovni tabulky hello a aktuálně existují dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="d7241-133">hello distribution method is defined at hello table level and currently there are two choices:</span></span>

1. <span data-ttu-id="d7241-134">**Kruhové dotazování** který distribuci dat rovnoměrně ale náhodně.</span><span class="sxs-lookup"><span data-stu-id="d7241-134">**Round robin** which distribute data evenly but randomly.</span></span>
2. <span data-ttu-id="d7241-135">**Hodnoty hash distribuované** která distribuuje data podle algoritmu hash hodnoty z jednoho sloupce</span><span class="sxs-lookup"><span data-stu-id="d7241-135">**Hash Distributed** which distributes data based on hashing values from a single column</span></span>

<span data-ttu-id="d7241-136">Ve výchozím nastavení, když nejsou definovány způsobu distribuce dat, tabulka budou distribuována pomocí hello **kruhové dotazování** na metodu.</span><span class="sxs-lookup"><span data-stu-id="d7241-136">By default, when you do not define a data distribution method, your table will be distributed using hello **round robin** distribution method.</span></span>  <span data-ttu-id="d7241-137">Ale, až se sofistikovanější v implementaci, můžete pomocí tooconsider **hash distribuované** tabulky toominimize přesun dat, která bude zase optimalizovat výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="d7241-137">However, as you become more sophisticated in your implementation, you will want tooconsider using **hash distributed** tables toominimize data movement which will in turn optimize query performance.</span></span>

### <a name="round-robin-tables"></a><span data-ttu-id="d7241-138">Kruhové dotazování tabulky</span><span class="sxs-lookup"><span data-stu-id="d7241-138">Round Robin Tables</span></span>
<span data-ttu-id="d7241-139">Použití hello kruhové dotazování metoda distribuci dat je velmi dobře v tom, jak ho vyznívá.</span><span class="sxs-lookup"><span data-stu-id="d7241-139">Using hello Round Robin method of distributing data is very much how it sounds.</span></span>  <span data-ttu-id="d7241-140">Při načtení dat je každý řádek jednoduše odeslána toohello další distribuční.</span><span class="sxs-lookup"><span data-stu-id="d7241-140">As your data is loaded, each row is simply sent toohello next distribution.</span></span>  <span data-ttu-id="d7241-141">Tato metoda distribuci dat hello bude vždy náhodně distribuci dat hello velmi rovnoměrně napříč všemi hello distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-141">This method of distributing hello data will always randomly distribute hello data very evenly across all of hello distributions.</span></span>  <span data-ttu-id="d7241-142">To znamená, že neexistuje žádné řazení done během hello round robin procesu, který uloží data.</span><span class="sxs-lookup"><span data-stu-id="d7241-142">That is, there is no sorting done during hello round robin process which places your data.</span></span>  <span data-ttu-id="d7241-143">Kruhové dotazování distribuční se někdy nazývá náhodných hash z tohoto důvodu.</span><span class="sxs-lookup"><span data-stu-id="d7241-143">A round robin distribution is sometimes called a random hash for this reason.</span></span>  <span data-ttu-id="d7241-144">S tabulkou distribuované kruhového dotazování nejsou žádná data hello toounderstand potřeba.</span><span class="sxs-lookup"><span data-stu-id="d7241-144">With a round-robin distributed table there is no need toounderstand hello data.</span></span>  <span data-ttu-id="d7241-145">Z tohoto důvodu kruhového dotazování tabulky často je dobré načítání cíle.</span><span class="sxs-lookup"><span data-stu-id="d7241-145">For this reason, Round-Robin tables often make good loading targets.</span></span>

<span data-ttu-id="d7241-146">Ve výchozím nastavení pokud je vybrána žádná metoda distribuce, hello kruhové dotazování distribuční metoda se použije.</span><span class="sxs-lookup"><span data-stu-id="d7241-146">By default, if no distribution method is chosen, hello round robin distribution method will be used.</span></span>  <span data-ttu-id="d7241-147">Kruhové dotazování tabulky jsou snadno toouse, protože data se náhodně distribuuje do systému hello znamená to, že hello systém nemůže zaručit, které distribuční každý řádek je však na.</span><span class="sxs-lookup"><span data-stu-id="d7241-147">However, while round robin tables are easy toouse, because data is randomly distributed across hello system it means that hello system can't guarantee which distribution each row is on.</span></span>  <span data-ttu-id="d7241-148">Výsledek, někdy systému hello stačit tooinvoke toobetter operace přesunu dat organizování vašich dat před překlad dotazu.</span><span class="sxs-lookup"><span data-stu-id="d7241-148">As a result, hello system sometimes needs tooinvoke a data movement operation toobetter organize your data before it can resolve a query.</span></span>  <span data-ttu-id="d7241-149">Tento krok navíc může zpomalit své dotazy.</span><span class="sxs-lookup"><span data-stu-id="d7241-149">This extra step can slow down your queries.</span></span>

<span data-ttu-id="d7241-150">Zvažte použití kruhové dotazování distribuce pro tabulku v hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="d7241-150">Consider using Round Robin distribution for your table in hello following scenarios:</span></span>

* <span data-ttu-id="d7241-151">Pro začátek jednoduché počáteční bod</span><span class="sxs-lookup"><span data-stu-id="d7241-151">When getting started as a simple starting point</span></span>
* <span data-ttu-id="d7241-152">Pokud neexistuje zřejmé spojující klíč</span><span class="sxs-lookup"><span data-stu-id="d7241-152">If there is no obvious joining key</span></span>
* <span data-ttu-id="d7241-153">Pokud není k dispozici sloupec vhodným kandidátem pro distribuci hello tabulku hash</span><span class="sxs-lookup"><span data-stu-id="d7241-153">If there is not good candidate column for hash distributing hello table</span></span>
* <span data-ttu-id="d7241-154">Pokud hello tabulky není sdílet společný klíč spojení s jinými tabulkami</span><span class="sxs-lookup"><span data-stu-id="d7241-154">If hello table does not share a common join key with other tables</span></span>
* <span data-ttu-id="d7241-155">Pokud je méně důležité než jiné spojení v dotazu hello hello spojení</span><span class="sxs-lookup"><span data-stu-id="d7241-155">If hello join is less significant than other joins in hello query</span></span>
* <span data-ttu-id="d7241-156">Pokud je tabulka hello dočasné pracovní tabulky</span><span class="sxs-lookup"><span data-stu-id="d7241-156">When hello table is a temporary staging table</span></span>

<span data-ttu-id="d7241-157">Obě tyto příklady vytvoří tabulku, kruhové dotazování:</span><span class="sxs-lookup"><span data-stu-id="d7241-157">Both of these examples will create a Round Robin Table:</span></span>

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
> <span data-ttu-id="d7241-158">Pokud kruhové dotazování je, že typ tabulky výchozí hello probíhá explicitní v DDL je považovat za osvědčený postup, aby byly zrušte tooothers záměry hello rozložení tabulky.</span><span class="sxs-lookup"><span data-stu-id="d7241-158">While round robin is hello default table type being explicit in your DDL is considered a best practice so that hello intentions of your table layout are clear tooothers.</span></span>
>
>

### <a name="hash-distributed-tables"></a><span data-ttu-id="d7241-159">Hodnoty hash distribuované tabulky</span><span class="sxs-lookup"><span data-stu-id="d7241-159">Hash Distributed Tables</span></span>
<span data-ttu-id="d7241-160">Použití **Hash distribuované** algoritmus toodistribute vaše tabulky můžete zlepšit výkon pro mnoho scénářů snížením přesun dat v době dotazu.</span><span class="sxs-lookup"><span data-stu-id="d7241-160">Using a **Hash distributed** algorithm toodistribute your tables can improve performance for many scenarios by reducing data movement at query time.</span></span>  <span data-ttu-id="d7241-161">Hodnota hash, který distribuované tabulky jsou tabulky, které jsou rozděleny mezi hello distribuované databáze pomocí algoritmu hash na jeden sloupec, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="d7241-161">Hash distributed tables are tables which are divided between hello distributed databases using a hashing algorithm on a single column which you select.</span></span>  <span data-ttu-id="d7241-162">Hello distribuční sloupce je určen jak rozděluje hello data do distribuované databáze.</span><span class="sxs-lookup"><span data-stu-id="d7241-162">hello distribution column is what determines how hello data is divided across your distributed databases.</span></span>  <span data-ttu-id="d7241-163">Funkce hash Hello používá hello distribuční sloupce tooassign řádky toodistributions.</span><span class="sxs-lookup"><span data-stu-id="d7241-163">hello hash function uses hello distribution column tooassign rows toodistributions.</span></span>  <span data-ttu-id="d7241-164">Hello algoritmu hash a výsledný distribuce je deterministický.</span><span class="sxs-lookup"><span data-stu-id="d7241-164">hello hashing algorithm and resulting distribution is deterministic.</span></span>  <span data-ttu-id="d7241-165">To znamená hello toohello má stejnou hodnotu s hello stejný datový typ. bude vždy stejné distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-165">That is hello same value with hello same data type will always has toohello same distribution.</span></span>    

<span data-ttu-id="d7241-166">Tento příklad vytvoří tabulku rozdělit na id:</span><span class="sxs-lookup"><span data-stu-id="d7241-166">This example will create a table distributed on id:</span></span>

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

## <a name="select-distribution-column"></a><span data-ttu-id="d7241-167">Vybrat distribuční sloupce</span><span class="sxs-lookup"><span data-stu-id="d7241-167">Select distribution column</span></span>
<span data-ttu-id="d7241-168">Pokud vyberete příliš**distribuovat algoritmu hash** tabulku, budete potřebovat tooselect jednoho distribučního sloupec.</span><span class="sxs-lookup"><span data-stu-id="d7241-168">When you choose too**hash distribute** a table, you will need tooselect a single distribution column.</span></span>  <span data-ttu-id="d7241-169">Když vyberete distribuční sloupce, existují tři hlavní faktory tooconsider.</span><span class="sxs-lookup"><span data-stu-id="d7241-169">When selecting a distribution column, there are three major factors tooconsider.</span></span>  

<span data-ttu-id="d7241-170">Vyberte jeden sloupec, který bude:</span><span class="sxs-lookup"><span data-stu-id="d7241-170">Select a single column which will:</span></span>

1. <span data-ttu-id="d7241-171">Není možné aktualizovat</span><span class="sxs-lookup"><span data-stu-id="d7241-171">Not be updated</span></span>
2. <span data-ttu-id="d7241-172">Distribuci dat rovnoměrně, zabraňující zkosení dat</span><span class="sxs-lookup"><span data-stu-id="d7241-172">Distribute data evenly, avoiding data skew</span></span>
3. <span data-ttu-id="d7241-173">Minimalizovat přesun dat</span><span class="sxs-lookup"><span data-stu-id="d7241-173">Minimize data movement</span></span>

### <a name="select-distribution-column-which-will-not-be-updated"></a><span data-ttu-id="d7241-174">Vybrat distribuční sloupec, který nebude aktualizován</span><span class="sxs-lookup"><span data-stu-id="d7241-174">Select distribution column which will not be updated</span></span>
<span data-ttu-id="d7241-175">Distribuční sloupce nejsou aktualizovat, proto, vyberte sloupec s statické hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d7241-175">Distribution columns are not updatable, therefore, select a column with static values.</span></span>  <span data-ttu-id="d7241-176">Pokud sloupec bude nutné aktualizovat toobe, není obecně správné distribuční candidate.</span><span class="sxs-lookup"><span data-stu-id="d7241-176">If a column will need toobe updated, it is generally not a good distribution candidate.</span></span>  <span data-ttu-id="d7241-177">Pokud je případ, kde je nutné aktualizovat distribuční sloupce, můžete to provést nejdřív odstranit řádek hello a následného vložení nového řádku.</span><span class="sxs-lookup"><span data-stu-id="d7241-177">If there is a case where you must update a distribution column, this can be done by first deleting hello row and then inserting a new row.</span></span>

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a><span data-ttu-id="d7241-178">Vybrat distribuční sloupec, který bude rovnoměrně distribuovat data</span><span class="sxs-lookup"><span data-stu-id="d7241-178">Select distribution column which will distribute data evenly</span></span>
<span data-ttu-id="d7241-179">Vzhledem k tomu, že distribuovaného systému provádí pouze rychlé jako jeho nejpomalejší distribuci, je důležité toodivide hello pracovní rovnoměrně mezi hello distribuce v pořadí spouštění tooachieve vyrovnáváním napříč hello systému.</span><span class="sxs-lookup"><span data-stu-id="d7241-179">Since a distributed system performs only as fast as its slowest distribution, it is important toodivide hello work evenly across hello distributions in order tooachieve balanced execution across hello system.</span></span>  <span data-ttu-id="d7241-180">způsob Hello hello práce je rozdělena na distribuovaného systému je založena na tam, kde hello dat pro každý distribuci platný.</span><span class="sxs-lookup"><span data-stu-id="d7241-180">hello way hello work is divided on a distributed system is based on where hello data for each distribution lives.</span></span>  <span data-ttu-id="d7241-181">Díky tomu je velmi důležité tooselect hello správné distribuční sloupce pro distribuci dat hello tak, aby každý distribuční má stejnou práci a bude trvat hello stejný čas toocomplete jeho část práce hello.</span><span class="sxs-lookup"><span data-stu-id="d7241-181">This makes it very important tooselect hello right distribution column for distributing hello data so that each distribution has equal work and will take hello same time toocomplete its portion of hello work.</span></span>  <span data-ttu-id="d7241-182">Při práci a rozděluje do hello systému, dat hello vyvážená hello distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-182">When work is well divided across hello system, hello data is balanced across hello distributions.</span></span>  <span data-ttu-id="d7241-183">Když data není vyrovnáváním rovnoměrně, říkáme to **data zkosení**.</span><span class="sxs-lookup"><span data-stu-id="d7241-183">When data is not evenly balanced, we call this **data skew**.</span></span>  

<span data-ttu-id="d7241-184">toodivide data rovnoměrně a vyhnout se data zkosení, zvažte následující hello při výběru vaší distribuční sloupce:</span><span class="sxs-lookup"><span data-stu-id="d7241-184">toodivide data evenly and avoid data skew, consider hello following when selecting your distribution column:</span></span>

1. <span data-ttu-id="d7241-185">Vyberte sloupce, který obsahuje velký počet jedinečných hodnot.</span><span class="sxs-lookup"><span data-stu-id="d7241-185">Select a column which contains a significant number of distinct values.</span></span>
2. <span data-ttu-id="d7241-186">Vyhněte se distribuci dat na sloupce s několika jedinečných hodnot.</span><span class="sxs-lookup"><span data-stu-id="d7241-186">Avoid distributing data on columns with a few distinct values.</span></span>
3. <span data-ttu-id="d7241-187">Vyhněte se distribuci dat na sloupce s vysoká frekvence hodnot Null.</span><span class="sxs-lookup"><span data-stu-id="d7241-187">Avoid distributing data on columns with a high frequency of nulls.</span></span>
4. <span data-ttu-id="d7241-188">Vyhněte se distribuci dat na sloupců s kalendářními daty.</span><span class="sxs-lookup"><span data-stu-id="d7241-188">Avoid distributing data on date columns.</span></span>

<span data-ttu-id="d7241-189">Protože každá hodnota hash too1 60 distribuce, tooachieve rovnoměrné rozdělení můžete tooselect sloupec, který je vysoce jedinečný a obsahuje více než 60 jedinečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d7241-189">Since each value is hashed too1 of 60 distributions, tooachieve even distribution you will want tooselect a column that is highly unique and contains more than 60 unique values.</span></span>  <span data-ttu-id="d7241-190">tooillustrate, představte si případ, kdy sloupec má jenom 40 jedinečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d7241-190">tooillustrate, consider a case where a column only has 40 unique values.</span></span>  <span data-ttu-id="d7241-191">Pokud v tomto sloupci jste vybrali jako hello distribučního klíče, hello data pro tuto tabulku by nebude zobrazovat na 40 distribuce maximálně ponechat 20 distribuce se žádná data a žádné toodo zpracování.</span><span class="sxs-lookup"><span data-stu-id="d7241-191">If this column was selected as hello distribution key, hello data for that table would land on 40 distributions at most, leaving 20 distributions with no data and no processing toodo.</span></span>  <span data-ttu-id="d7241-192">Naopak hello dalších 40 distribuce by měla mít více pracovní toodo, že pokud hello byla data rovnoměrně šířit přes 60 distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-192">Conversely, hello other 40 distributions would have more work toodo that if hello data was evenly spread over 60 distributions.</span></span>  <span data-ttu-id="d7241-193">Tento scénář je příkladem zkosení data.</span><span class="sxs-lookup"><span data-stu-id="d7241-193">This scenario is an example of data skew.</span></span>

<span data-ttu-id="d7241-194">V systému MPP každého kroku dotazu čeká na všechny toocomplete distribuce jejich sdílení práce hello.</span><span class="sxs-lookup"><span data-stu-id="d7241-194">In MPP system, each query step waits for all distributions toocomplete their share of hello work.</span></span>  <span data-ttu-id="d7241-195">Pokud do jednoho distribučního je to více práce, než ostatní hello, hello prostředků hello jiných distribuce jsou v podstatě ke znehodnocení části právě čekání na zaneprázdněn distribuční hello.</span><span class="sxs-lookup"><span data-stu-id="d7241-195">If one distribution is doing more work than hello others, then hello resource of hello other distributions are essentially wasted just waiting on hello busy distribution.</span></span>  <span data-ttu-id="d7241-196">Při práci není rovnoměrně rozloženy všechny distribuce, říkáme to **zpracování zkosení**.</span><span class="sxs-lookup"><span data-stu-id="d7241-196">When work is not evenly spread across all distributions, we call this **processing skew**.</span></span>  <span data-ttu-id="d7241-197">Zpracování zkosení způsobí, že dotazy toorun nižší než pokud hello zatížení může být rovnoměrně rozloženy hello distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-197">Processing skew will cause queries toorun slower than if hello workload can be evenly spread across hello distributions.</span></span>  <span data-ttu-id="d7241-198">Data zkosení povede tooprocessing zkosení.</span><span class="sxs-lookup"><span data-stu-id="d7241-198">Data skew will lead tooprocessing skew.</span></span>

<span data-ttu-id="d7241-199">Nedávejte na vysoce sloupec jako hello hodnoty null budou všechny zobrazovat hello stejné distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-199">Avoid distributing on highly nullable column as hello null values will all land on hello same distribution.</span></span> <span data-ttu-id="d7241-200">Distribuce ve sloupci Datum může také způsobit zpracování zkosení vzhledem k tomu, že všechna data pro konkrétního data budou zobrazovat hello stejné distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-200">Distributing on a date column can also cause processing skew because all data for a given date will land on hello same distribution.</span></span> <span data-ttu-id="d7241-201">Pokud několik uživatelů provádí dotazy, které jsou všechny filtrování na hello stejné datum, pak pouze 1 hello 60 distribuce bude by všechny hello práce od konkrétního data budou pouze na jednom distribučním.</span><span class="sxs-lookup"><span data-stu-id="d7241-201">If several users are executing queries all filtering on hello same date, then only 1 of hello 60 distributions will be doing all of hello work since a given date will only be on one distribution.</span></span> <span data-ttu-id="d7241-202">V tomto scénáři pravděpodobně hello dotazy se spustí 60 x pomalejší než pokud hello dat byly rovnoměrně rozloženy všechny hello distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-202">In this scenario, hello queries will likely run 60 times slower than if hello data were equally spread over all of hello distributions.</span></span>

<span data-ttu-id="d7241-203">Pokud neexistují žádné sloupce vhodným kandidátem, můžete použít kruhové dotazování jako metodu distribuce hello.</span><span class="sxs-lookup"><span data-stu-id="d7241-203">When no good candidate columns exist, then consider using round robin as hello distribution method.</span></span>

### <a name="select-distribution-column-which-will-minimize-data-movement"></a><span data-ttu-id="d7241-204">Vybrat distribuční sloupec, který bude minimalizovat přesun dat</span><span class="sxs-lookup"><span data-stu-id="d7241-204">Select distribution column which will minimize data movement</span></span>
<span data-ttu-id="d7241-205">Minimalizace přesun dat zvolením hello správné distribuční sloupce je jedním z hello nejdůležitější strategie pro optimalizaci výkonu SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d7241-205">Minimizing data movement by selecting hello right distribution column is one of hello most important strategies for optimizing performance of your SQL Data Warehouse.</span></span>  <span data-ttu-id="d7241-206">Přesun dat nejčastěji mohou nastat, pokud jsou připojené k tabulky nebo agregace se provádí.</span><span class="sxs-lookup"><span data-stu-id="d7241-206">Data Movement most commonly arises when tables are joined or aggregations are performed.</span></span>  <span data-ttu-id="d7241-207">Sloupce použité v `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` a `HAVING` klauzule pro všechny provedl **dobrý** hash kandidáty distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-207">Columns used in `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` and `HAVING` clauses all make for **good** hash distribution candidates.</span></span>

<span data-ttu-id="d7241-208">Na druhé straně sloupců v hello hello `WHERE` klauzule proveďte **není** provést pro sloupec kandidátů na dobrý hash vzhledem k tomu, že omezit, které distribuce účastnit hello dotazu, která způsobila zpracování zkosení.</span><span class="sxs-lookup"><span data-stu-id="d7241-208">On hello other hand, columns in hello `WHERE` clause do **not** make for good hash column candidates because they limit which distributions participate in hello query, causing processing skew.</span></span>  <span data-ttu-id="d7241-209">Dobrým příkladem sloupce, který může být tempting toodistribute na, ale často může způsobit zpracování zkosení je sloupec datum.</span><span class="sxs-lookup"><span data-stu-id="d7241-209">A good example of a column which might be tempting toodistribute on, but often can cause this processing skew is a date column.</span></span>

<span data-ttu-id="d7241-210">Obecně platí Pokud máte dvě tabulky faktů velké často zahrnutých ve spojení, bude získáte hello většina výkonu distribucí obě tabulky v jednom ze sloupce spojení hello.</span><span class="sxs-lookup"><span data-stu-id="d7241-210">Generally speaking, if you have two large fact tables frequently involved in a join, you will gain hello most performance by distributing both tables on one of hello join columns.</span></span>  <span data-ttu-id="d7241-211">Pokud máte tabulku, která je nikdy tabulky faktů velké připojené k tooanother, pak hledejte toocolumns, které jsou často v hello `GROUP BY` klauzule.</span><span class="sxs-lookup"><span data-stu-id="d7241-211">If you have a table that is never joined tooanother large fact table, then look toocolumns that are frequently in hello `GROUP BY` clause.</span></span>

<span data-ttu-id="d7241-212">Existuje několik klíčů kritérií, které musí být nesplnění tooavoid přesun dat během spojení:</span><span class="sxs-lookup"><span data-stu-id="d7241-212">There are a few key criteria which must be met tooavoid data movement during a join:</span></span>

1. <span data-ttu-id="d7241-213">Hello tabulky zahrnutých v hello připojení musí být hodnota hash rozdělit na **jeden** hello sloupců podílejících se na hello spojení.</span><span class="sxs-lookup"><span data-stu-id="d7241-213">hello tables involved in hello join must be hash distributed on **one** of hello columns participating in hello join.</span></span>
2. <span data-ttu-id="d7241-214">Hello datové typy sloupce spojení hello musí odpovídat mezi obě tabulky.</span><span class="sxs-lookup"><span data-stu-id="d7241-214">hello data types of hello join columns must match between both tables.</span></span>
3. <span data-ttu-id="d7241-215">Hello sloupce musí být připojené pomocí operátoru rovná se.</span><span class="sxs-lookup"><span data-stu-id="d7241-215">hello columns must be joined with an equals operator.</span></span>
4. <span data-ttu-id="d7241-216">Hello typ spojení nesmí být `CROSS JOIN`.</span><span class="sxs-lookup"><span data-stu-id="d7241-216">hello join type may not be a `CROSS JOIN`.</span></span>

## <a name="troubleshooting-data-skew"></a><span data-ttu-id="d7241-217">Řešení potíží s zkosení dat</span><span class="sxs-lookup"><span data-stu-id="d7241-217">Troubleshooting data skew</span></span>
<span data-ttu-id="d7241-218">Při distribuci dat tabulky pomocí metody distribuce hash hello existuje šance, že bude některých distribucích nesouměrně rozdělí toohave nepřiměřeně více dat než jiné.</span><span class="sxs-lookup"><span data-stu-id="d7241-218">When table data is distributed using hello hash distribution method there is a chance that some distributions will be skewed toohave disproportionately more data than others.</span></span> <span data-ttu-id="d7241-219">Nadměrné data zkosení může ovlivnit výkon dotazů, protože hello konečný výsledek distribuovaného dotazu musí čekat na toofinish distribuční nejdelší spuštěné hello.</span><span class="sxs-lookup"><span data-stu-id="d7241-219">Excessive data skew can impact query performance because hello final result of a distributed query must wait for hello longest running distribution toofinish.</span></span> <span data-ttu-id="d7241-220">V závislosti na hello stupeň hello data zkosení, že může být nutné tooaddress ho.</span><span class="sxs-lookup"><span data-stu-id="d7241-220">Depending on hello degree of hello data skew you might need tooaddress it.</span></span>

### <a name="identifying-skew"></a><span data-ttu-id="d7241-221">Identifikace zkosení</span><span class="sxs-lookup"><span data-stu-id="d7241-221">Identifying skew</span></span>
<span data-ttu-id="d7241-222">Jednoduchý způsob tooidentify tabulku jako nesouměrně rozdělí je toouse `DBCC PDW_SHOWSPACEUSED`.</span><span class="sxs-lookup"><span data-stu-id="d7241-222">A simple way tooidentify a table as skewed is toouse `DBCC PDW_SHOWSPACEUSED`.</span></span>  <span data-ttu-id="d7241-223">To je velmi rychlý a jednoduchý způsob toosee hello počet řádků tabulky, které jsou uložené v každé z distribuce hello 60 vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="d7241-223">This is a very quick and simple way toosee hello number of table rows that are stored in each of hello 60 distributions of your database.</span></span>  <span data-ttu-id="d7241-224">Mějte na paměti, že hello nejvíce vyrovnáváním výkonu hello řádky v tabulce distribuované by měl být rovnoměrně rozloženy všechny hello distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-224">Remember that for hello most balanced performance, hello rows in your distributed table should be spread evenly across all hello distributions.</span></span>

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

<span data-ttu-id="d7241-225">Pokud dotaz zobrazení dynamické správy Azure SQL Data Warehouse hello (DMV) však můžete provádět více podrobné analýzy.</span><span class="sxs-lookup"><span data-stu-id="d7241-225">However, if you query hello Azure SQL Data Warehouse dynamic management views (DMV) you can perform a more detailed analysis.</span></span>  <span data-ttu-id="d7241-226">toostart, vytvořit zobrazení hello [dbo.vTableSizes] [ dbo.vTableSizes] zobrazit pomocí hello SQL z [tabulky přehled] [ Overview] článku.</span><span class="sxs-lookup"><span data-stu-id="d7241-226">toostart, create hello view [dbo.vTableSizes][dbo.vTableSizes] view using hello SQL from [Table Overview][Overview] article.</span></span>  <span data-ttu-id="d7241-227">Po vytvoření hello zobrazení, spusťte tento dotaz tooidentify, které tabulky mít víc než 10 % data zkosení.</span><span class="sxs-lookup"><span data-stu-id="d7241-227">Once hello view is created, run this query tooidentify which tables have more than 10% data skew.</span></span>

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

### <a name="resolving-data-skew"></a><span data-ttu-id="d7241-228">Řešení zkosení dat</span><span class="sxs-lookup"><span data-stu-id="d7241-228">Resolving data skew</span></span>
<span data-ttu-id="d7241-229">Všechny zkosení je dostatek toowarrant oprava.</span><span class="sxs-lookup"><span data-stu-id="d7241-229">Not all skew is enough toowarrant a fix.</span></span>  <span data-ttu-id="d7241-230">V některých případech může výkon hello tabulky v některé dotazy převáží nad hello škodu dat zkosení.</span><span class="sxs-lookup"><span data-stu-id="d7241-230">In some cases, hello performance of a table in some queries can outweigh hello harm of data skew.</span></span>  <span data-ttu-id="d7241-231">Pokud byste měli vyřešit dat toodecide zkreslit v tabulce, byste měli porozumět v vaše úlohy co nejvíce o hello datové svazky a dotazy.</span><span class="sxs-lookup"><span data-stu-id="d7241-231">toodecide if you should resolve data skew in a table, you should understand as much as possible about hello data volumes and queries in your workload.</span></span>   <span data-ttu-id="d7241-232">Jedním ze způsobů toolook v hello dopad zkosení je toouse hello kroky v hello [dotazu monitorování] [ Query Monitoring] článek toomonitor hello dopad na výkon dotazů zkosení a konkrétně hello dopad toohow dlouhé dotazy toocomplete převezmou jednotlivé distribuce hello.</span><span class="sxs-lookup"><span data-stu-id="d7241-232">One way toolook at hello impact of skew is toouse hello steps in hello [Query Monitoring][Query Monitoring] article toomonitor hello impact of skew on query performance and specifically hello impact toohow long queries take toocomplete on hello individual distributions.</span></span>

<span data-ttu-id="d7241-233">Distribuci dat je řádu hello rovnováhu mezi minimalizaci zkosení dat a současně minimalizujete její přesun dat hledání.</span><span class="sxs-lookup"><span data-stu-id="d7241-233">Distributing data is a matter of finding hello right balance between minimizing data skew and minimizing data movement.</span></span> <span data-ttu-id="d7241-234">To může být proti cíle a někdy budete chtít tookeep data zkosení v přesunu dat tooreduce pořadí.</span><span class="sxs-lookup"><span data-stu-id="d7241-234">These can be opposing goals, and sometimes you will want tookeep data skew in order tooreduce data movement.</span></span> <span data-ttu-id="d7241-235">Například když hello distribuční sloupec je často hello sdílené v spojování a agregaci, můžete se se současně minimalizujete její přesun dat.</span><span class="sxs-lookup"><span data-stu-id="d7241-235">For example, when hello distribution column is frequently hello shared column in joins and aggregations, you will be minimizing data movement.</span></span> <span data-ttu-id="d7241-236">Hello Výhodou vytvoření přesun dat minimální hello vyváží hello dopad toho, že data zkreslit.</span><span class="sxs-lookup"><span data-stu-id="d7241-236">hello benefit of having hello minimal data movement might outweigh hello impact of having data skew.</span></span>

<span data-ttu-id="d7241-237">obvyklým způsobem Hello tooresolve data zkosení je toore-vytvořit hello tabulku se sloupcem jiný distribuční.</span><span class="sxs-lookup"><span data-stu-id="d7241-237">hello typical way tooresolve data skew is toore-create hello table with a different distribution column.</span></span> <span data-ttu-id="d7241-238">Vzhledem k tomu, že neexistuje žádný způsob toochange hello distribuční sloupce na existující tabulky, hello způsob toochange hello distribuční tabulky ho toorecreate její [] [funkce CTAS].</span><span class="sxs-lookup"><span data-stu-id="d7241-238">Since there is no way toochange hello distribution column on an existing table, hello way toochange hello distribution of a table it toorecreate it with a [CTAS][].</span></span>  <span data-ttu-id="d7241-239">Zde jsou dva příklady, jak vyřešit data zkosení:</span><span class="sxs-lookup"><span data-stu-id="d7241-239">Here are two examples of how resolve data skew:</span></span>

### <a name="example-1-re-create-hello-table-with-a-new-distribution-column"></a><span data-ttu-id="d7241-240">Příklad 1: Vytvořte znovu hello tabulky se sloupcem nový distribuční</span><span class="sxs-lookup"><span data-stu-id="d7241-240">Example 1: Re-create hello table with a new distribution column</span></span>
<span data-ttu-id="d7241-241">Tento příklad používá toore [] – [funkce CTAS]-vytvoří tabulku se sloupcem distribuční různé hodnoty hash.</span><span class="sxs-lookup"><span data-stu-id="d7241-241">This example uses [CTAS][] toore-create a table with a different hash distribution column.</span></span>

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

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] too[FactInternetSales];
```

### <a name="example-2-re-create-hello-table-using-round-robin-distribution"></a><span data-ttu-id="d7241-242">Příklad 2: Znovu vytvořit tabulku hello použití distribučních bodů kruhové dotazování</span><span class="sxs-lookup"><span data-stu-id="d7241-242">Example 2: Re-create hello table using round robin distribution</span></span>
<span data-ttu-id="d7241-243">Tento příklad používá toore [] – [funkce CTAS]-vytvořte si tabulku s každým místo hash distribuce.</span><span class="sxs-lookup"><span data-stu-id="d7241-243">This example uses [CTAS][] toore-create a table with round robin instead of a hash distribution.</span></span> <span data-ttu-id="d7241-244">Tato změna způsobí distribuce i data hello náklady na přesun dat vyšší.</span><span class="sxs-lookup"><span data-stu-id="d7241-244">This change will produce even data distribution at hello cost of increased data movement.</span></span>

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

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] too[FactInternetSales];
```

## <a name="next-steps"></a><span data-ttu-id="d7241-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7241-245">Next steps</span></span>
<span data-ttu-id="d7241-246">toolearn Další informace o návrh tabulky, najdete v části hello [distribuovat][Distribute], [Index][Index], [oddílu] [ Partition], [Datové typy][Data Types], [statistiky] [ Statistics] a [dočasných tabulek] [ Temporary] články.</span><span class="sxs-lookup"><span data-stu-id="d7241-246">toolearn more about table design, see hello [Distribute][Distribute], [Index][Index], [Partition][Partition], [Data Types][Data Types], [Statistics][Statistics] and [Temporary Tables][Temporary] articles.</span></span>

<span data-ttu-id="d7241-247">Přehled osvědčených postupů najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="d7241-247">For an overview of best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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
