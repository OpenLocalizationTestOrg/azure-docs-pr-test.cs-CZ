---
title: "Glosář nástroje databáze aaaElastic | Microsoft Docs"
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
ms.openlocfilehash: d6573aad9a097e07135b0a64d1dafec19bb8cc7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-tools-glossary"></a><span data-ttu-id="0598e-103">Glosář nástroje elastické databáze</span><span class="sxs-lookup"><span data-stu-id="0598e-103">Elastic Database tools glossary</span></span>
<span data-ttu-id="0598e-104">Hello následující termíny jsou definovány pro hello [nástroje elastické databáze](sql-database-elastic-scale-introduction.md), funkce Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="0598e-104">hello following terms are defined for hello [Elastic Database tools](sql-database-elastic-scale-introduction.md), a feature of Azure SQL Database.</span></span> <span data-ttu-id="0598e-105">Hello nástroje jsou použité toomanage [mapuje horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md)a zahrnovat hello [klientské knihovny](sql-database-elastic-database-client-library.md), hello [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md), [elastické fondy](sql-database-elastic-pool.md), a [dotazy](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0598e-105">hello tools are used toomanage [shard maps](sql-database-elastic-scale-shard-map-management.md), and include hello [client library](sql-database-elastic-database-client-library.md), hello [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md), [elastic pools](sql-database-elastic-pool.md), and [queries](sql-database-elastic-query-overview.md).</span></span> 

<span data-ttu-id="0598e-106">Tyto podmínky se používají v [přidání horizontálních, pomocí nástroje elastické databáze](sql-database-elastic-scale-add-a-shard.md) a [pomocí hello RecoveryManager třída toofix horizontálního oddílu mapy problémy](sql-database-elastic-database-recovery-manager.md).</span><span class="sxs-lookup"><span data-stu-id="0598e-106">These terms are used in [Adding a shard using Elastic Database tools](sql-database-elastic-scale-add-a-shard.md) and [Using hello RecoveryManager class toofix shard map problems](sql-database-elastic-database-recovery-manager.md).</span></span>

![Elastické škálování podmínky][1]

<span data-ttu-id="0598e-108">**Databáze**: Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="0598e-108">**Database**: An Azure SQL database.</span></span> 

<span data-ttu-id="0598e-109">**Data závislé směrování**: hello funkci, která umožňuje horizontálních tooa tooconnect aplikace přiřazen klíč konkrétní horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="0598e-109">**Data dependent routing**: hello functionality that enables an application tooconnect tooa shard given a specific sharding key.</span></span> <span data-ttu-id="0598e-110">V tématu [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md).</span><span class="sxs-lookup"><span data-stu-id="0598e-110">See [Data dependent routing](sql-database-elastic-scale-data-dependent-routing.md).</span></span> <span data-ttu-id="0598e-111">Porovnání příliš**[dotazu víc horizontálních](sql-database-elastic-scale-multishard-querying.md)**.</span><span class="sxs-lookup"><span data-stu-id="0598e-111">Compare too**[Multi-Shard Query](sql-database-elastic-scale-multishard-querying.md)**.</span></span>

<span data-ttu-id="0598e-112">**Globální horizontálních mapy**: hello mapování mezi horizontálního dělení klíčů a jejich odpovídajících horizontálních oddílů v rámci **horizontálního oddílu sadu**.</span><span class="sxs-lookup"><span data-stu-id="0598e-112">**Global shard map**: hello map between sharding keys and their respective shards within a **shard set**.</span></span> <span data-ttu-id="0598e-113">Mapa globální horizontálních Hello je uložen v hello **správce mapy horizontálního oddílu**.</span><span class="sxs-lookup"><span data-stu-id="0598e-113">hello global shard map is stored in hello **shard map manager**.</span></span> <span data-ttu-id="0598e-114">Porovnání příliš**místní horizontálních mapy**.</span><span class="sxs-lookup"><span data-stu-id="0598e-114">Compare too**local shard map**.</span></span>

<span data-ttu-id="0598e-115">**Mapování horizontálních seznamu**: horizontálního oddílu mapy, ve které horizontálního dělení klíči jsou namapované jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="0598e-115">**List shard map**: A shard map in which sharding keys are mapped individually.</span></span> <span data-ttu-id="0598e-116">Porovnání příliš**rozsah horizontálního oddílu mapy**.</span><span class="sxs-lookup"><span data-stu-id="0598e-116">Compare too**Range Shard Map**.</span></span>   

<span data-ttu-id="0598e-117">**Mapa místního horizontálních**: uložené na horizontálního oddílu, hello místní horizontálních mapy obsahuje mapování hello shardlets nacházející se na hello horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="0598e-117">**Local shard map**: Stored on a shard, hello local shard map contains mappings for hello shardlets that reside on hello shard.</span></span>

<span data-ttu-id="0598e-118">**Dotaz s více horizontálních**: hello možnost tooissue dotaz víc horizontálních oddílů; nastaví výsledky se vrátí pomocí sémantiky UNION ALL (také označované jako "fan-out dotaz").</span><span class="sxs-lookup"><span data-stu-id="0598e-118">**Multi-shard query**: hello ability tooissue a query against multiple shards; results sets are returned using UNION ALL semantics (also known as “fan-out query”).</span></span> <span data-ttu-id="0598e-119">Porovnání příliš**závislé směrování dat**.</span><span class="sxs-lookup"><span data-stu-id="0598e-119">Compare too**data dependent routing**.</span></span>

<span data-ttu-id="0598e-120">**Víceklientské** a **jednoho klienta**: Zobrazí databázi jednoho klienta a víceklientské databáze:</span><span class="sxs-lookup"><span data-stu-id="0598e-120">**Multi-tenant** and **Single-tenant**: This shows a single-tenant database and a multi-tenant database:</span></span>

![Databáze jeden a více klientů](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

<span data-ttu-id="0598e-122">Zde je reprezentace **horizontálně dělené** databáze jednoho a víc klientů.</span><span class="sxs-lookup"><span data-stu-id="0598e-122">Here is a representation of **sharded** single and multi-tenant databases.</span></span> 

![Databáze jeden a více klientů](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

<span data-ttu-id="0598e-124">**Mapování horizontálních rozsah**: horizontálního oddílu mapy, ve které hello horizontálního oddílu distribuční strategie vychází z více oblastí souvislý hodnot.</span><span class="sxs-lookup"><span data-stu-id="0598e-124">**Range shard map**: A shard map in which hello shard distribution strategy is based on multiple ranges of contiguous values.</span></span> 

<span data-ttu-id="0598e-125">**Referenční tabulky**: tabulek, které nejsou horizontálně dělené ale se replikují napříč horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="0598e-125">**Reference tables**: Tables that are not sharded but are replicated across shards.</span></span> <span data-ttu-id="0598e-126">Například zip kódy, které mohou být uloženy v referenční tabulce.</span><span class="sxs-lookup"><span data-stu-id="0598e-126">For example, zip codes can be stored in a reference table.</span></span> 

<span data-ttu-id="0598e-127">**Horizontálního oddílu**: Azure SQL database, která ukládá data z horizontálně dělené datové sady.</span><span class="sxs-lookup"><span data-stu-id="0598e-127">**Shard**: An Azure SQL database that stores data from a sharded data set.</span></span> 

<span data-ttu-id="0598e-128">**Pružnost horizontálního oddílu**: hello možnost tooperform **vodorovné škálování** a **svislé škálování**.</span><span class="sxs-lookup"><span data-stu-id="0598e-128">**Shard elasticity**: hello ability tooperform both **horizontal scaling** and **vertical scaling**.</span></span>

<span data-ttu-id="0598e-129">**Horizontálně dělené tabulky**: tabulky, které jsou horizontálně dělené, tj., jejichž data se distribuuje do horizontálních oddílů na základě jejich hodnot klíče horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="0598e-129">**Sharded tables**: Tables that are sharded, i.e., whose data is distributed across shards based on their sharding key values.</span></span> 

<span data-ttu-id="0598e-130">**Klíč horizontálního dělení**: hodnota sloupce, který určuje, jak se data distribuuje do horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="0598e-130">**Sharding key**: A column value that determines how data is distributed across shards.</span></span> <span data-ttu-id="0598e-131">Hello typ hodnoty může mít jednu z následujících hello: **int**, **bigint**, **varbinary**, nebo **uniqueidentifier**.</span><span class="sxs-lookup"><span data-stu-id="0598e-131">hello value type can be one of hello following: **int**, **bigint**, **varbinary**, or **uniqueidentifier**.</span></span> 

<span data-ttu-id="0598e-132">**Sada horizontálního oddílu**: hello kolekce horizontálních oddílů, které jsou s atributy toohello stejné ID horizontálního oddílu mapy ve hello horizontálního oddílu mapa správce.</span><span class="sxs-lookup"><span data-stu-id="0598e-132">**Shard set**: hello collection of shards that are attributed toohello same shard map in hello shard map manager.</span></span>  

<span data-ttu-id="0598e-133">**Shardlet**: všechna data hello přidružené jednu hodnotu klíče horizontálního dělení na horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="0598e-133">**Shardlet**: All of hello data associated with a single value of a sharding key on a shard.</span></span> <span data-ttu-id="0598e-134">Shardlet je nejmenší jednotka hello přesun dat je možné při Redistribuce horizontálně dělené tabulky.</span><span class="sxs-lookup"><span data-stu-id="0598e-134">A shardlet is hello smallest unit of data movement possible when redistributing sharded tables.</span></span> 

<span data-ttu-id="0598e-135">**Mapování horizontálních**: hello sada mapování mezi horizontálního dělení klíčů a jejich odpovídajících horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="0598e-135">**Shard map**: hello set of mappings between sharding keys and their respective shards.</span></span>

<span data-ttu-id="0598e-136">**Správce mapy horizontálního oddílu**: Správa objektů a dat úložiště, které obsahuje hello horizontálního oddílu map(s), horizontálního oddílu umístění a mapování pro jednu nebo více sad horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="0598e-136">**Shard map manager**: A management object and data store that contains hello shard map(s), shard locations, and mappings for one or more shard sets.</span></span>

![Mapování][2]

## <a name="verbs"></a><span data-ttu-id="0598e-138">Příkazy</span><span class="sxs-lookup"><span data-stu-id="0598e-138">Verbs</span></span>
<span data-ttu-id="0598e-139">**Vodorovné škálování**: hello operace škálování out (nebo v) kolekce horizontálních oddílů přidáním nebo odebráním horizontálních oddílů tooa horizontálního oddílu mapu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="0598e-139">**Horizontal scaling**: hello act of scaling out (or in) a collection of shards by adding or removing shards tooa shard map, as shown below.</span></span>

![Vodorovného a svislého škálování][3]

<span data-ttu-id="0598e-141">**Sloučení**: hello operace přesunutí shardlets ze dvou horizontálních oddílů tooone horizontálních a odpovídajícím způsobem aktualizace hello horizontálního oddílu mapy.</span><span class="sxs-lookup"><span data-stu-id="0598e-141">**Merge**: hello act of moving shardlets from two shards tooone shard and updating hello shard map accordingly.</span></span>

<span data-ttu-id="0598e-142">**Přesunutí Shardlet**: hello act přesunu jednom shardlet tooa různých horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="0598e-142">**Shardlet move**: hello act of moving a single shardlet tooa different shard.</span></span> 

<span data-ttu-id="0598e-143">**Horizontálního oddílu**: hello act vodorovně oddíly stejně jako strukturovaná data napříč více databází na základě klíče horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="0598e-143">**Shard**: hello act of horizontally partitioning identically structured data across multiple databases based on a sharding key.</span></span>

<span data-ttu-id="0598e-144">**Rozdělení**: hello v rámci přechod z jednoho horizontálních tooanother (obvykle novou) horizontálních několik shardlets.</span><span class="sxs-lookup"><span data-stu-id="0598e-144">**Split**: hello act of moving several shardlets from one shard tooanother (typically new) shard.</span></span> <span data-ttu-id="0598e-145">Klíč horizontálního dělení je poskytovaná v rámci hello uživatele hello rozdělení bodu.</span><span class="sxs-lookup"><span data-stu-id="0598e-145">A sharding key is provided by hello user as hello split point.</span></span>

<span data-ttu-id="0598e-146">**Svislé škálování**: hello operace škálování nahoru (nebo dolů) hello úroveň výkonu jednotlivých horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="0598e-146">**Vertical Scaling**: hello act of scaling up (or down) hello performance level of an individual shard.</span></span> <span data-ttu-id="0598e-147">Například změna horizontálního oddílu z standardní tooPremium (což vede k více výpočetní prostředky).</span><span class="sxs-lookup"><span data-stu-id="0598e-147">For example, changing a shard from Standard tooPremium (which results in more computing resources).</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

