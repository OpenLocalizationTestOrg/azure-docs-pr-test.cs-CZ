---
title: "Migrace vašeho schématu do SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro migraci schématu do Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 538b60c9-a07f-49bf-9ea3-1082ed6699fb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 07ca2321852e276502187e768177e7e82bdfd080
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-schemas-to-sql-data-warehouse"></a><span data-ttu-id="51292-103">Migrace vaší schémata do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="51292-103">Migrate your schemas to SQL Data Warehouse</span></span>
<span data-ttu-id="51292-104">Pokyny k migraci vaší schémata SQL do SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="51292-104">Guidance for migrating your SQL schemas to SQL Data Warehouse.</span></span> 

## <a name="plan-your-schema-migration"></a><span data-ttu-id="51292-105">Plánování migrace schématu</span><span class="sxs-lookup"><span data-stu-id="51292-105">Plan your schema migration</span></span>

<span data-ttu-id="51292-106">Při plánování migrace najdete v článku [tabulky přehled] [ table overview] se seznámit s aspekty návrhu tabulky například statistiky, distribuci, vytváření oddílů a indexování.</span><span class="sxs-lookup"><span data-stu-id="51292-106">As you plan a migration, see the [table overview][table overview] to become familiar with table design considerations such as statistics, distribution, partitioning, and indexing.</span></span>  <span data-ttu-id="51292-107">Také uvádí některé [nepodporované funkce tabulky] [ unsupported table features] a jejich řešení.</span><span class="sxs-lookup"><span data-stu-id="51292-107">It also lists some [unsupported table features][unsupported table features] and their workarounds.</span></span>

## <a name="use-user-defined-schemas-to-consolidate-databases"></a><span data-ttu-id="51292-108">Použít vlastní schémata pro konsolidaci databází</span><span class="sxs-lookup"><span data-stu-id="51292-108">Use user-defined schemas to consolidate databases</span></span>

<span data-ttu-id="51292-109">Vaše stávající úlohy pravděpodobně obsahuje více než jednu databázi.</span><span class="sxs-lookup"><span data-stu-id="51292-109">Your existing workload probably has more than one database.</span></span> <span data-ttu-id="51292-110">Datového skladu SQL serveru může například obsahovat pracovní databáze, databáze datového skladu a některé databáze datového tržiště.</span><span class="sxs-lookup"><span data-stu-id="51292-110">For example, a SQL Server data warehouse might include a staging database, a data warehouse database, and some data mart databases.</span></span> <span data-ttu-id="51292-111">V této topologii spouští každou databázi jako samostatné zatížení zásadám samostatné zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="51292-111">In this topology, each database runs as a separate workload with separate security policies.</span></span>

<span data-ttu-id="51292-112">Naopak SQL Data Warehouse spouští úlohy celého datového skladu v rámci jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="51292-112">By contrast, SQL Data Warehouse runs the entire data warehouse workload within one database.</span></span> <span data-ttu-id="51292-113">Mezi databáze nejsou povoleny spojení.</span><span class="sxs-lookup"><span data-stu-id="51292-113">Cross database joins are not permitted.</span></span> <span data-ttu-id="51292-114">Proto SQL Data Warehouse očekává, že všechny tabulky použité v datovém skladu ukládaly v rámci jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="51292-114">Therefore, SQL Data Warehouse expects all tables used by the data warehouse to be stored within the one database.</span></span>

<span data-ttu-id="51292-115">Doporučujeme používat vlastní schémata pro konsolidaci vaše stávající úlohy do jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="51292-115">We recommend using user-defined schemas to consolidate your existing workload into one database.</span></span> <span data-ttu-id="51292-116">Příklady najdete v tématu [uživatelem definované schémata](sql-data-warehouse-develop-user-defined-schemas.md)</span><span class="sxs-lookup"><span data-stu-id="51292-116">For examples, see [User-defined schemas](sql-data-warehouse-develop-user-defined-schemas.md)</span></span>

## <a name="use-compatible-data-types"></a><span data-ttu-id="51292-117">Použít kompatibilní datové typy</span><span class="sxs-lookup"><span data-stu-id="51292-117">Use compatible data types</span></span>
<span data-ttu-id="51292-118">Upravte svým datovým typům, aby byl kompatibilní s SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="51292-118">Modify your data types to be compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="51292-119">Seznam podporované a nepodporované datové typy, naleznete v části [datové typy][data types].</span><span class="sxs-lookup"><span data-stu-id="51292-119">For a list of supported and unsupported data types, see [data types][data types].</span></span> <span data-ttu-id="51292-120">Toto téma nabízí řešení pro nepodporované typy.</span><span class="sxs-lookup"><span data-stu-id="51292-120">That topic gives workarounds for the unsupported types.</span></span> <span data-ttu-id="51292-121">Poskytuje také dotaz k identifikaci existující typy, které nejsou podporované v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="51292-121">It also provides a query to identify existing types that are not supported in SQL Data Warehouse.</span></span>

## <a name="minimize-row-size"></a><span data-ttu-id="51292-122">Minimální velikost řádku</span><span class="sxs-lookup"><span data-stu-id="51292-122">Minimize row size</span></span>
<span data-ttu-id="51292-123">Pro nejlepší výkon Minimalizujte délku řádku tabulky.</span><span class="sxs-lookup"><span data-stu-id="51292-123">For best performance, minimize the row length of your tables.</span></span> <span data-ttu-id="51292-124">Vzhledem k tomu, že kratší délky řádek vést k dosažení vyššího výkonu, použijte nejmenší datové typy, které fungují pro vaše data.</span><span class="sxs-lookup"><span data-stu-id="51292-124">Since shorter row lengths lead to better performance, use the smallest data types that work for your data.</span></span> 

<span data-ttu-id="51292-125">Šířka řádku tabulky má PolyBase omezení 1 MB.</span><span class="sxs-lookup"><span data-stu-id="51292-125">For table row width, PolyBase has a 1 MB limit.</span></span>  <span data-ttu-id="51292-126">Pokud budete chtít načíst data do SQL Data Warehouse pomocí PolyBase, aktualizujte tabulky tak, aby měl maximální šířky menší než 1 MB.</span><span class="sxs-lookup"><span data-stu-id="51292-126">If you plan to load data into SQL Data Warehouse with PolyBase, update your tables to have maximum row widths of less than 1 MB.</span></span> 

<!--
- For example, this table uses variable length data but the largest possible size of the row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and the defined row width is less than one MB. When loading rows, PolyBase allocates the full length of the variable-length data. The full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-the-distribution-option"></a><span data-ttu-id="51292-127">Zadejte možnosti distribuce</span><span class="sxs-lookup"><span data-stu-id="51292-127">Specify the distribution option</span></span>
<span data-ttu-id="51292-128">SQL Data Warehouse je systém distribuovanou databázi.</span><span class="sxs-lookup"><span data-stu-id="51292-128">SQL Data Warehouse is a distributed database system.</span></span> <span data-ttu-id="51292-129">Každá tabulka je distribuovat nebo replikovat mezi výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="51292-129">Each table is distributed or replicated across the Compute nodes.</span></span> <span data-ttu-id="51292-130">Není možnost tabulky, který umožňuje určit, jak se bude distribuovat data.</span><span class="sxs-lookup"><span data-stu-id="51292-130">There's a table option that lets you specify how to distribute the data.</span></span> <span data-ttu-id="51292-131">Mezi volby patří kruhového dotazování, replikují, nebo distribuovat algoritmu hash.</span><span class="sxs-lookup"><span data-stu-id="51292-131">The choices are  round-robin, replicated, or hash distributed.</span></span> <span data-ttu-id="51292-132">Každý má výhody a nevýhody.</span><span class="sxs-lookup"><span data-stu-id="51292-132">Each has pros and cons.</span></span> <span data-ttu-id="51292-133">Pokud nezadáte možnosti distribuce, použije SQL Data Warehouse pomocí kruhového dotazování jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="51292-133">If you don't specify the distribution option, SQL Data Warehouse will use round-robin as the default.</span></span>

- <span data-ttu-id="51292-134">Kruhové dotazování je výchozí.</span><span class="sxs-lookup"><span data-stu-id="51292-134">Round-robin is the default.</span></span> <span data-ttu-id="51292-135">Je nejjednodušším způsobem, a zatížením tak rychlý jako možný, ale spojení dat bude vyžadovat přesun dat, což zpomalí výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="51292-135">It is the simplest to use, and loads the data as fast as possible, but joins will require data movement which slows query performance.</span></span>
- <span data-ttu-id="51292-136">Replikované uloží kopie tabulky na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="51292-136">Replicated stores a copy of the table on each Compute node.</span></span> <span data-ttu-id="51292-137">Replikované tabulky jsou původce, protože nevyžadují přesun dat pro spojování a agregaci.</span><span class="sxs-lookup"><span data-stu-id="51292-137">Replicated tables are performant because they do not require data movement for joins and aggregations.</span></span> <span data-ttu-id="51292-138">Vyžadovat dodatečné úložiště a proto nejvhodnější pro menší tabulky.</span><span class="sxs-lookup"><span data-stu-id="51292-138">They do require extra storage, and therefore work best for smaller tables.</span></span>
- <span data-ttu-id="51292-139">Hodnota hash distribuované rozděluje řádky na všechny uzly prostřednictvím funkce hash.</span><span class="sxs-lookup"><span data-stu-id="51292-139">Hash distributed distributes the rows across all the nodes via a hash function.</span></span> <span data-ttu-id="51292-140">Hodnota hash distribuované tabulky jsou srdcem SQL Data Warehouse vzhledem k tomu, že jsou navrženy k poskytování vysokého výkonu dotazu na velké tabulky.</span><span class="sxs-lookup"><span data-stu-id="51292-140">Hash distributed tables are the heart of SQL Data Warehouse since they are designed to provide high query performance on large tables.</span></span> <span data-ttu-id="51292-141">Tato možnost vyžaduje některé plánování vybrat nejlepší sloupec, na které chcete distribuovat data.</span><span class="sxs-lookup"><span data-stu-id="51292-141">This option requires some planning to select the best column on which to distribute the data.</span></span> <span data-ttu-id="51292-142">Ale když nebude sloupci nejlepší poprvé, můžete snadno znovu distribuovat dat na jiný sloupec.</span><span class="sxs-lookup"><span data-stu-id="51292-142">However, if you don't choose the best column the first time, you can easily re-distribute the data on a different column.</span></span> 

<span data-ttu-id="51292-143">Chcete-li zvolit nejlepší možnost distribuce pro každou tabulku, přečtěte si téma [distribuované tabulky](sql-data-warehouse-tables-distribute.md).</span><span class="sxs-lookup"><span data-stu-id="51292-143">To choose the best distribution option for each table, see [Distributed tables](sql-data-warehouse-tables-distribute.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="51292-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51292-144">Next steps</span></span>
<span data-ttu-id="51292-145">Po úspěšné migraci svého schématu databáze do SQL Data Warehouse, pokračujte na jednu z následujících článků:</span><span class="sxs-lookup"><span data-stu-id="51292-145">Once you have successfully migrated your database schema to SQL Data Warehouse, proceed to one of the following articles:</span></span>

* <span data-ttu-id="51292-146">[Migrace dat][Migrate your data]</span><span class="sxs-lookup"><span data-stu-id="51292-146">[Migrate your data][Migrate your data]</span></span>
* <span data-ttu-id="51292-147">[Migrace vašeho kódu][Migrate your code]</span><span class="sxs-lookup"><span data-stu-id="51292-147">[Migrate your code][Migrate your code]</span></span>

<span data-ttu-id="51292-148">Další informace o osvědčených postupech pro SQL Data Warehouse najdete [osvědčené postupy] [ best practices] článku.</span><span class="sxs-lookup"><span data-stu-id="51292-148">For more about SQL Data Warehouse best practices, see the [best practices][best practices] article.</span></span>

<!--Image references-->

<!--Article references-->
[Migrate your code]: ./sql-data-warehouse-migrate-code.md
[Migrate your data]: ./sql-data-warehouse-migrate-data.md
[best practices]: ./sql-data-warehouse-best-practices.md
[table overview]: ./sql-data-warehouse-tables-overview.md
[unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[data types]: ./sql-data-warehouse-tables-data-types.md
[unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types

<!--MSDN references-->


<!--Other Web references-->
