---
title: "aaaMigrate tooSQL vašeho schématu datového skladu | Microsoft Docs"
description: "Tipy pro migraci vašeho schématu tooAzure SQL Data Warehouse na vývoj řešení."
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
ms.openlocfilehash: 1309b743b78564575695038a4856d9d25a2b18d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-schemas-toosql-data-warehouse"></a><span data-ttu-id="088eb-103">Migrace vaší schémata tooSQL datového skladu</span><span class="sxs-lookup"><span data-stu-id="088eb-103">Migrate your schemas tooSQL Data Warehouse</span></span>
<span data-ttu-id="088eb-104">Pokyny k migraci vaší tooSQL schémata SQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="088eb-104">Guidance for migrating your SQL schemas tooSQL Data Warehouse.</span></span> 

## <a name="plan-your-schema-migration"></a><span data-ttu-id="088eb-105">Plánování migrace schématu</span><span class="sxs-lookup"><span data-stu-id="088eb-105">Plan your schema migration</span></span>

<span data-ttu-id="088eb-106">Při plánování migrace najdete v části hello [tabulky přehled] [ table overview] toobecome obeznámeni s aspekty návrhu tabulky například statistiky, distribuci, vytváření oddílů a indexování.</span><span class="sxs-lookup"><span data-stu-id="088eb-106">As you plan a migration, see hello [table overview][table overview] toobecome familiar with table design considerations such as statistics, distribution, partitioning, and indexing.</span></span>  <span data-ttu-id="088eb-107">Také uvádí některé [nepodporované funkce tabulky] [ unsupported table features] a jejich řešení.</span><span class="sxs-lookup"><span data-stu-id="088eb-107">It also lists some [unsupported table features][unsupported table features] and their workarounds.</span></span>

## <a name="use-user-defined-schemas-tooconsolidate-databases"></a><span data-ttu-id="088eb-108">Použít vlastní schémata tooconsolidate databáze</span><span class="sxs-lookup"><span data-stu-id="088eb-108">Use user-defined schemas tooconsolidate databases</span></span>

<span data-ttu-id="088eb-109">Vaše stávající úlohy pravděpodobně obsahuje více než jednu databázi.</span><span class="sxs-lookup"><span data-stu-id="088eb-109">Your existing workload probably has more than one database.</span></span> <span data-ttu-id="088eb-110">Datového skladu SQL serveru může například obsahovat pracovní databáze, databáze datového skladu a některé databáze datového tržiště.</span><span class="sxs-lookup"><span data-stu-id="088eb-110">For example, a SQL Server data warehouse might include a staging database, a data warehouse database, and some data mart databases.</span></span> <span data-ttu-id="088eb-111">V této topologii spouští každou databázi jako samostatné zatížení zásadám samostatné zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="088eb-111">In this topology, each database runs as a separate workload with separate security policies.</span></span>

<span data-ttu-id="088eb-112">Naopak spouští SQL Data Warehouse hello celého datového skladu zatížení v rámci jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="088eb-112">By contrast, SQL Data Warehouse runs hello entire data warehouse workload within one database.</span></span> <span data-ttu-id="088eb-113">Mezi databáze nejsou povoleny spojení.</span><span class="sxs-lookup"><span data-stu-id="088eb-113">Cross database joins are not permitted.</span></span> <span data-ttu-id="088eb-114">Proto SQL Data Warehouse očekává, že všechny tabulky použité ve hello datového skladu toobe uložené v rámci jedné databáze hello.</span><span class="sxs-lookup"><span data-stu-id="088eb-114">Therefore, SQL Data Warehouse expects all tables used by hello data warehouse toobe stored within hello one database.</span></span>

<span data-ttu-id="088eb-115">Doporučujeme používat vlastní schémata tooconsolidate vaše stávající úlohy do jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="088eb-115">We recommend using user-defined schemas tooconsolidate your existing workload into one database.</span></span> <span data-ttu-id="088eb-116">Příklady najdete v tématu [uživatelem definované schémata](sql-data-warehouse-develop-user-defined-schemas.md)</span><span class="sxs-lookup"><span data-stu-id="088eb-116">For examples, see [User-defined schemas](sql-data-warehouse-develop-user-defined-schemas.md)</span></span>

## <a name="use-compatible-data-types"></a><span data-ttu-id="088eb-117">Použít kompatibilní datové typy</span><span class="sxs-lookup"><span data-stu-id="088eb-117">Use compatible data types</span></span>
<span data-ttu-id="088eb-118">Upravte vaše toobe typy dat kompatibilní s SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="088eb-118">Modify your data types toobe compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="088eb-119">Seznam podporované a nepodporované datové typy, naleznete v části [datové typy][data types].</span><span class="sxs-lookup"><span data-stu-id="088eb-119">For a list of supported and unsupported data types, see [data types][data types].</span></span> <span data-ttu-id="088eb-120">Toto téma poskytuje řešení pro hello nepodporované typy.</span><span class="sxs-lookup"><span data-stu-id="088eb-120">That topic gives workarounds for hello unsupported types.</span></span> <span data-ttu-id="088eb-121">Poskytuje také dotazu tooidentify existující typy, které nejsou podporované v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="088eb-121">It also provides a query tooidentify existing types that are not supported in SQL Data Warehouse.</span></span>

## <a name="minimize-row-size"></a><span data-ttu-id="088eb-122">Minimální velikost řádku</span><span class="sxs-lookup"><span data-stu-id="088eb-122">Minimize row size</span></span>
<span data-ttu-id="088eb-123">Pro nejlepší výkon minimalizujte hello délku řádku tabulky.</span><span class="sxs-lookup"><span data-stu-id="088eb-123">For best performance, minimize hello row length of your tables.</span></span> <span data-ttu-id="088eb-124">Vzhledem k tomu, že kratší délky řádek vést toobetter výkonu, použijte hello nejmenší typy dat, které fungovat pro vaše data.</span><span class="sxs-lookup"><span data-stu-id="088eb-124">Since shorter row lengths lead toobetter performance, use hello smallest data types that work for your data.</span></span> 

<span data-ttu-id="088eb-125">Šířka řádku tabulky má PolyBase omezení 1 MB.</span><span class="sxs-lookup"><span data-stu-id="088eb-125">For table row width, PolyBase has a 1 MB limit.</span></span>  <span data-ttu-id="088eb-126">Pokud máte v plánu tooload dat do SQL Data Warehouse pomocí PolyBase, aktualizujte vaše tabulky toohave maximální šířky menší než 1 MB.</span><span class="sxs-lookup"><span data-stu-id="088eb-126">If you plan tooload data into SQL Data Warehouse with PolyBase, update your tables toohave maximum row widths of less than 1 MB.</span></span> 

<!--
- For example, this table uses variable length data but hello largest possible size of hello row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and hello defined row width is less than one MB. When loading rows, PolyBase allocates hello full length of hello variable-length data. hello full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-hello-distribution-option"></a><span data-ttu-id="088eb-127">Zadejte možnosti distribuce hello</span><span class="sxs-lookup"><span data-stu-id="088eb-127">Specify hello distribution option</span></span>
<span data-ttu-id="088eb-128">SQL Data Warehouse je systém distribuovanou databázi.</span><span class="sxs-lookup"><span data-stu-id="088eb-128">SQL Data Warehouse is a distributed database system.</span></span> <span data-ttu-id="088eb-129">Každá tabulka je distribuovat nebo replikovat mezi hello výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="088eb-129">Each table is distributed or replicated across hello Compute nodes.</span></span> <span data-ttu-id="088eb-130">Není možnost tabulky, který umožňuje určit, jak toodistribute hello data.</span><span class="sxs-lookup"><span data-stu-id="088eb-130">There's a table option that lets you specify how toodistribute hello data.</span></span> <span data-ttu-id="088eb-131">Hello jsou možnosti kruhové dotazování, replikují, nebo distribuovat algoritmu hash.</span><span class="sxs-lookup"><span data-stu-id="088eb-131">hello choices are  round-robin, replicated, or hash distributed.</span></span> <span data-ttu-id="088eb-132">Každý má výhody a nevýhody.</span><span class="sxs-lookup"><span data-stu-id="088eb-132">Each has pros and cons.</span></span> <span data-ttu-id="088eb-133">Pokud nezadáte možnosti distribuce hello, SQL Data Warehouse použije jako výchozí hello kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="088eb-133">If you don't specify hello distribution option, SQL Data Warehouse will use round-robin as hello default.</span></span>

- <span data-ttu-id="088eb-134">Výchozí hello je kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="088eb-134">Round-robin is hello default.</span></span> <span data-ttu-id="088eb-135">Je nejjednodušší toouse hello a načte data hello tak rychlý jako možná, ale spojení budou vyžadovat přesun dat, což zpomalí výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="088eb-135">It is hello simplest toouse, and loads hello data as fast as possible, but joins will require data movement which slows query performance.</span></span>
- <span data-ttu-id="088eb-136">Replikované uloží kopie hello tabulky na každém výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="088eb-136">Replicated stores a copy of hello table on each Compute node.</span></span> <span data-ttu-id="088eb-137">Replikované tabulky jsou původce, protože nevyžadují přesun dat pro spojování a agregaci.</span><span class="sxs-lookup"><span data-stu-id="088eb-137">Replicated tables are performant because they do not require data movement for joins and aggregations.</span></span> <span data-ttu-id="088eb-138">Vyžadovat dodatečné úložiště a proto nejvhodnější pro menší tabulky.</span><span class="sxs-lookup"><span data-stu-id="088eb-138">They do require extra storage, and therefore work best for smaller tables.</span></span>
- <span data-ttu-id="088eb-139">Hodnota hash distribuované distribuuje hello řádků pro všechny uzly hello prostřednictvím funkce hash.</span><span class="sxs-lookup"><span data-stu-id="088eb-139">Hash distributed distributes hello rows across all hello nodes via a hash function.</span></span> <span data-ttu-id="088eb-140">Hodnota hash distribuované tabulky jsou hello vysílat služby SQL Data Warehouse vzhledem k tomu, že jsou navrženou tooprovide vysokého výkonu dotazu na velké tabulky.</span><span class="sxs-lookup"><span data-stu-id="088eb-140">Hash distributed tables are hello heart of SQL Data Warehouse since they are designed tooprovide high query performance on large tables.</span></span> <span data-ttu-id="088eb-141">Tato možnost vyžaduje některé plánování tooselect hello nejlepší sloupce v datových toodistribute hello.</span><span class="sxs-lookup"><span data-stu-id="088eb-141">This option requires some planning tooselect hello best column on which toodistribute hello data.</span></span> <span data-ttu-id="088eb-142">Ale když nebude hello nejlepší sloupec hello poprvé, můžete snadno znovu distribuovat hello dat na jiný sloupec.</span><span class="sxs-lookup"><span data-stu-id="088eb-142">However, if you don't choose hello best column hello first time, you can easily re-distribute hello data on a different column.</span></span> 

<span data-ttu-id="088eb-143">najdete v části toochoose hello nejvhodnější distribuční možnosti pro každou tabulku [distribuované tabulky](sql-data-warehouse-tables-distribute.md).</span><span class="sxs-lookup"><span data-stu-id="088eb-143">toochoose hello best distribution option for each table, see [Distributed tables](sql-data-warehouse-tables-distribute.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="088eb-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="088eb-144">Next steps</span></span>
<span data-ttu-id="088eb-145">Po úspěšné migraci vaší tooSQL schéma databáze datového skladu pokračujte tooone hello následující články:</span><span class="sxs-lookup"><span data-stu-id="088eb-145">Once you have successfully migrated your database schema tooSQL Data Warehouse, proceed tooone of hello following articles:</span></span>

* <span data-ttu-id="088eb-146">[Migrace dat][Migrate your data]</span><span class="sxs-lookup"><span data-stu-id="088eb-146">[Migrate your data][Migrate your data]</span></span>
* <span data-ttu-id="088eb-147">[Migrace vašeho kódu][Migrate your code]</span><span class="sxs-lookup"><span data-stu-id="088eb-147">[Migrate your code][Migrate your code]</span></span>

<span data-ttu-id="088eb-148">Další informace o osvědčených postupech pro SQL Data Warehouse najdete v tématu hello [osvědčené postupy] [ best practices] článku.</span><span class="sxs-lookup"><span data-stu-id="088eb-148">For more about SQL Data Warehouse best practices, see hello [best practices][best practices] article.</span></span>

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
