---
title: "Glosář nástroje elastické databáze | Microsoft Docs"
description: "Vysvětlení termínů používaných pro elastické databáze nástroje"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: a23a4e81-6706-452d-afc1-a550e5e47af9
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 0fda4bb948bbed1c14d468519ba67cce9bc4e6c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="elastic-database-tools-glossary"></a><span data-ttu-id="a8896-103">Glosář nástroje elastické databáze</span><span class="sxs-lookup"><span data-stu-id="a8896-103">Elastic Database tools glossary</span></span>
<span data-ttu-id="a8896-104">Následující termíny jsou definovány pro [nástroje elastické databáze](sql-database-elastic-scale-introduction.md), funkce Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a8896-104">The following terms are defined for the [Elastic Database tools](sql-database-elastic-scale-introduction.md), a feature of Azure SQL Database.</span></span> <span data-ttu-id="a8896-105">Nástroje se používají ke správě [mapuje horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md)a zahrnout [klientské knihovny](sql-database-elastic-database-client-library.md), [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md), [elastické fondy](sql-database-elastic-pool.md)a [dotazy](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a8896-105">The tools are used to manage [shard maps](sql-database-elastic-scale-shard-map-management.md), and include the [client library](sql-database-elastic-database-client-library.md), the [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md), [elastic pools](sql-database-elastic-pool.md), and [queries](sql-database-elastic-query-overview.md).</span></span> 

<span data-ttu-id="a8896-106">Tyto podmínky se používají v [přidání horizontálních, pomocí nástroje elastické databáze](sql-database-elastic-scale-add-a-shard.md) a [pomocí třídy RecoveryManager opravit problémy mapy horizontálního oddílu](sql-database-elastic-database-recovery-manager.md).</span><span class="sxs-lookup"><span data-stu-id="a8896-106">These terms are used in [Adding a shard using Elastic Database tools](sql-database-elastic-scale-add-a-shard.md) and [Using the RecoveryManager class to fix shard map problems](sql-database-elastic-database-recovery-manager.md).</span></span>

![Elastické škálování podmínky][1]

<span data-ttu-id="a8896-108">**Databáze**: Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="a8896-108">**Database**: An Azure SQL database.</span></span> 

<span data-ttu-id="a8896-109">**Data závislé směrování**: funkce, která umožňuje aplikaci připojit k horizontálních, přiřazen klíč konkrétní horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="a8896-109">**Data dependent routing**: The functionality that enables an application to connect to a shard given a specific sharding key.</span></span> <span data-ttu-id="a8896-110">V tématu [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md).</span><span class="sxs-lookup"><span data-stu-id="a8896-110">See [Data dependent routing](sql-database-elastic-scale-data-dependent-routing.md).</span></span> <span data-ttu-id="a8896-111">Porovnání  **[dotazu víc horizontálních](sql-database-elastic-scale-multishard-querying.md)**.</span><span class="sxs-lookup"><span data-stu-id="a8896-111">Compare to **[Multi-Shard Query](sql-database-elastic-scale-multishard-querying.md)**.</span></span>

<span data-ttu-id="a8896-112">**Globální horizontálních mapy**: mapy mezi horizontálního dělení klíčů a jejich odpovídajících horizontálních oddílů v rámci **horizontálního oddílu sadu**.</span><span class="sxs-lookup"><span data-stu-id="a8896-112">**Global shard map**: The map between sharding keys and their respective shards within a **shard set**.</span></span> <span data-ttu-id="a8896-113">Mapy globální horizontálního oddílu je uložen v **správce mapy horizontálního oddílu**.</span><span class="sxs-lookup"><span data-stu-id="a8896-113">The global shard map is stored in the **shard map manager**.</span></span> <span data-ttu-id="a8896-114">Porovnání **místní horizontálních mapy**.</span><span class="sxs-lookup"><span data-stu-id="a8896-114">Compare to **local shard map**.</span></span>

<span data-ttu-id="a8896-115">**Mapování horizontálních seznamu**: horizontálního oddílu mapy, ve které horizontálního dělení klíči jsou namapované jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="a8896-115">**List shard map**: A shard map in which sharding keys are mapped individually.</span></span> <span data-ttu-id="a8896-116">Porovnání **rozsahu horizontálního oddílu mapy**.</span><span class="sxs-lookup"><span data-stu-id="a8896-116">Compare to **Range Shard Map**.</span></span>   

<span data-ttu-id="a8896-117">**Mapa místního horizontálních**: uložené na horizontálního oddílu, místní horizontálních mapy obsahuje mapování shardlets nacházející se na horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="a8896-117">**Local shard map**: Stored on a shard, the local shard map contains mappings for the shardlets that reside on the shard.</span></span>

<span data-ttu-id="a8896-118">**Dotaz s více horizontálních**: možnost vydávat dotazy na víc horizontálních oddílů; nastaví výsledky se vrátí pomocí sémantiky UNION ALL (také označované jako "fan-out dotaz").</span><span class="sxs-lookup"><span data-stu-id="a8896-118">**Multi-shard query**: The ability to issue a query against multiple shards; results sets are returned using UNION ALL semantics (also known as “fan-out query”).</span></span> <span data-ttu-id="a8896-119">Porovnání **závislé směrování dat**.</span><span class="sxs-lookup"><span data-stu-id="a8896-119">Compare to **data dependent routing**.</span></span>

<span data-ttu-id="a8896-120">**Víceklientské** a **jednoho klienta**: Zobrazí databázi jednoho klienta a víceklientské databáze:</span><span class="sxs-lookup"><span data-stu-id="a8896-120">**Multi-tenant** and **Single-tenant**: This shows a single-tenant database and a multi-tenant database:</span></span>

![Databáze jeden a více klientů](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

<span data-ttu-id="a8896-122">Zde je reprezentace **horizontálně dělené** databáze jednoho a víc klientů.</span><span class="sxs-lookup"><span data-stu-id="a8896-122">Here is a representation of **sharded** single and multi-tenant databases.</span></span> 

![Databáze jeden a více klientů](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

<span data-ttu-id="a8896-124">**Mapování horizontálních rozsah**: horizontálního oddílu mapy, ve kterém je distribuční strategie horizontálního oddílu podle více oblastí souvislý hodnot.</span><span class="sxs-lookup"><span data-stu-id="a8896-124">**Range shard map**: A shard map in which the shard distribution strategy is based on multiple ranges of contiguous values.</span></span> 

<span data-ttu-id="a8896-125">**Referenční tabulky**: tabulek, které nejsou horizontálně dělené ale se replikují napříč horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="a8896-125">**Reference tables**: Tables that are not sharded but are replicated across shards.</span></span> <span data-ttu-id="a8896-126">Například zip kódy, které mohou být uloženy v referenční tabulce.</span><span class="sxs-lookup"><span data-stu-id="a8896-126">For example, zip codes can be stored in a reference table.</span></span> 

<span data-ttu-id="a8896-127">**Horizontálního oddílu**: Azure SQL database, která ukládá data z horizontálně dělené datové sady.</span><span class="sxs-lookup"><span data-stu-id="a8896-127">**Shard**: An Azure SQL database that stores data from a sharded data set.</span></span> 

<span data-ttu-id="a8896-128">**Pružnost horizontálního oddílu**: schopnost provádět i **vodorovné škálování** a **svislé škálování**.</span><span class="sxs-lookup"><span data-stu-id="a8896-128">**Shard elasticity**: The ability to perform both **horizontal scaling** and **vertical scaling**.</span></span>

<span data-ttu-id="a8896-129">**Horizontálně dělené tabulky**: tabulky, které jsou horizontálně dělené, tj., jejichž data se distribuuje do horizontálních oddílů na základě jejich hodnot klíče horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="a8896-129">**Sharded tables**: Tables that are sharded, i.e., whose data is distributed across shards based on their sharding key values.</span></span> 

<span data-ttu-id="a8896-130">**Klíč horizontálního dělení**: hodnota sloupce, který určuje, jak se data distribuuje do horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="a8896-130">**Sharding key**: A column value that determines how data is distributed across shards.</span></span> <span data-ttu-id="a8896-131">Typ hodnoty může být jedna z následujících: **int**, **bigint**, **varbinary**, nebo **uniqueidentifier**.</span><span class="sxs-lookup"><span data-stu-id="a8896-131">The value type can be one of the following: **int**, **bigint**, **varbinary**, or **uniqueidentifier**.</span></span> 

<span data-ttu-id="a8896-132">**Sada horizontálního oddílu**: kolekce horizontálních oddílů, které jsou označené stejné ID horizontálního oddílu mapu ve Správci mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="a8896-132">**Shard set**: The collection of shards that are attributed to the same shard map in the shard map manager.</span></span>  

<span data-ttu-id="a8896-133">**Shardlet**: všechna data související s jednu hodnotu klíče horizontálního dělení na horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="a8896-133">**Shardlet**: All of the data associated with a single value of a sharding key on a shard.</span></span> <span data-ttu-id="a8896-134">Shardlet je nejmenší jednotkou přesun dat je možné při Redistribuce horizontálně dělené tabulky.</span><span class="sxs-lookup"><span data-stu-id="a8896-134">A shardlet is the smallest unit of data movement possible when redistributing sharded tables.</span></span> 

<span data-ttu-id="a8896-135">**Mapování horizontálních**: sada mapování mezi horizontálního dělení klíčů a jejich odpovídajících horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="a8896-135">**Shard map**: The set of mappings between sharding keys and their respective shards.</span></span>

<span data-ttu-id="a8896-136">**Správce mapy horizontálního oddílu**: Správa objektů a dat úložiště, které obsahuje map(s) horizontálního oddílu, horizontálního oddílu umístění a mapování pro jednu nebo více sad horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="a8896-136">**Shard map manager**: A management object and data store that contains the shard map(s), shard locations, and mappings for one or more shard sets.</span></span>

![Mapování][2]

## <a name="verbs"></a><span data-ttu-id="a8896-138">Příkazy</span><span class="sxs-lookup"><span data-stu-id="a8896-138">Verbs</span></span>
<span data-ttu-id="a8896-139">**Vodorovné škálování**: operace škálování out (nebo v) kolekce horizontálních oddílů přidáním nebo odebráním horizontálních oddílů horizontálního oddílu mapu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="a8896-139">**Horizontal scaling**: The act of scaling out (or in) a collection of shards by adding or removing shards to a shard map, as shown below.</span></span>

![Vodorovného a svislého škálování][3]

<span data-ttu-id="a8896-141">**Sloučení**: operace přesunutí shardlets ze dvou horizontálních oddílů jeden horizontálního oddílu a odpovídajícím způsobem aktualizace mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="a8896-141">**Merge**: The act of moving shardlets from two shards to one shard and updating the shard map accordingly.</span></span>

<span data-ttu-id="a8896-142">**Přesunutí Shardlet**: operace přesunutí jednoho shardlet do různých horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="a8896-142">**Shardlet move**: The act of moving a single shardlet to a different shard.</span></span> 

<span data-ttu-id="a8896-143">**Horizontálního oddílu**: v rámci vodorovně dělení stejně jako strukturovaná data napříč více databází na základě klíče horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="a8896-143">**Shard**: The act of horizontally partitioning identically structured data across multiple databases based on a sharding key.</span></span>

<span data-ttu-id="a8896-144">**Rozdělení**: v rámci přesun několik shardlets z jednoho horizontálního oddílu na jiné (obvykle novou) horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="a8896-144">**Split**: The act of moving several shardlets from one shard to another (typically new) shard.</span></span> <span data-ttu-id="a8896-145">Uživatel zadal horizontálního dělení klíč jako bod rozdělení.</span><span class="sxs-lookup"><span data-stu-id="a8896-145">A sharding key is provided by the user as the split point.</span></span>

<span data-ttu-id="a8896-146">**Svislé škálování**: operace škálování nahoru (nebo dolů) úroveň výkonu jednotlivých horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="a8896-146">**Vertical Scaling**: The act of scaling up (or down) the performance level of an individual shard.</span></span> <span data-ttu-id="a8896-147">Například změna horizontálního oddílu z standardní, Premium (což vede k více výpočetní prostředky).</span><span class="sxs-lookup"><span data-stu-id="a8896-147">For example, changing a shard from Standard to Premium (which results in more computing resources).</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

