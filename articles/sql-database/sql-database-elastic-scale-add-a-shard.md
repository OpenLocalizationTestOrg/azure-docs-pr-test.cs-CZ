---
title: "Přidání horizontálního oddílu pomocí nástroje elastické databáze | Microsoft Docs"
description: "Nastavit jak přidat nové horizontálních oddílů horizontálního oddílu pomocí rozhraní API elastické škálování."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 62a349db-bebe-406f-a120-2f1986f2b286
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 6a91ea2251ea3b748faba5c97765bfded9c00234
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a><span data-ttu-id="75d7b-103">Přidání horizontálního oddílu pomocí nástroje elastické databáze</span><span class="sxs-lookup"><span data-stu-id="75d7b-103">Adding a shard using Elastic Database tools</span></span>
## <a name="to-add-a-shard-for-a-new-range-or-key"></a><span data-ttu-id="75d7b-104">Chcete-li přidat horizontálního oddílu pro nový rozsah nebo klíč</span><span class="sxs-lookup"><span data-stu-id="75d7b-104">To add a shard for a new range or key</span></span>
<span data-ttu-id="75d7b-105">Aplikace často potřebují jednoduše přidat nové horizontálních oddílů pro zpracování dat, která je očekávána z nových klíčů nebo rozsahy klíčů pro mapu horizontálního oddílu, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="75d7b-105">Applications often need to simply add new shards to handle data that is expected from new keys or key ranges, for a shard map that already exists.</span></span> <span data-ttu-id="75d7b-106">Například aplikace horizontálně dělené podle ID klienta muset zřídit nové horizontálního oddílu pro nového klienta, nebo horizontálně dělené měsíční dat může být nutné nové horizontálních zřídit před zahájením každý nový měsíc.</span><span class="sxs-lookup"><span data-stu-id="75d7b-106">For example, an application sharded by Tenant ID may need to provision a new shard for a new tenant, or data sharded monthly may need a new shard provisioned before the start of each new month.</span></span> 

<span data-ttu-id="75d7b-107">Pokud nový rozsah hodnot klíče již není součástí existující mapování, je velmi jednoduchý a přidat nové horizontálního oddílu přidružit nový klíč nebo rozsahu do této horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="75d7b-107">If the new range of key values is not already part of an existing mapping, it is very simple to add the new shard and associate the new key or range to that shard.</span></span> 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a><span data-ttu-id="75d7b-108">Příklad: Přidání horizontálního oddílu a její rozsah do existující mapu horizontálního oddílu</span><span class="sxs-lookup"><span data-stu-id="75d7b-108">Example:  adding a shard and its range to an existing shard map</span></span>
<span data-ttu-id="75d7b-109">Této ukázce se používá [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) metody a vytvoří instanci [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.)třídy.</span><span class="sxs-lookup"><span data-stu-id="75d7b-109">This sample uses the [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) the [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) methods, and creates an instance of the [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) class.</span></span> <span data-ttu-id="75d7b-110">V ukázce níže, databáze s názvem **sample_shard_2** a všechny objekty nezbytné schématu do něj byly vytvořeny pro uložení rozsah [300, 400).</span><span class="sxs-lookup"><span data-stu-id="75d7b-110">In the sample below, a database named **sample_shard_2** and all necessary schema objects inside of it have been created to hold range [300, 400).</span></span>  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


<span data-ttu-id="75d7b-111">Jako alternativu můžete použít Powershell k vytvoření nového správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="75d7b-111">As an alternative, you can use Powershell to create a new Shard Map Manager.</span></span> <span data-ttu-id="75d7b-112">Příkladem je k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="75d7b-112">An example is available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a><span data-ttu-id="75d7b-113">Chcete-li přidat horizontálního oddílu pro prázdný součástí stávající rozsah</span><span class="sxs-lookup"><span data-stu-id="75d7b-113">To add a shard for an empty part of an existing range</span></span>
<span data-ttu-id="75d7b-114">V některých případech je již namapován rozsah horizontálního oddílu a částečně vyplněnou data, ale teď chcete nadcházející data přesměrováni na různých horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="75d7b-114">In some circumstances, you may have already mapped a range to a shard and partially filled it with data, but you now want upcoming data to be directed to a different shard.</span></span> <span data-ttu-id="75d7b-115">Například můžete horizontálního oddílu podle rozsahu den a muset horizontálního oddílu již přidělené 50 dnů, ale dne 24 chcete zobrazovat v různých horizontálního oddílu budoucí data.</span><span class="sxs-lookup"><span data-stu-id="75d7b-115">For example, you shard by day range and have already allocated 50 days to a shard, but on day 24, you want future data to land in a different shard.</span></span> <span data-ttu-id="75d7b-116">Elastické databáze [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md) mohou provést tuto operaci, ale pokud přesun dat není potřebné (například data pro rozsah dnů [25, 50), tj., den 25 včetně na 50 vylučují, ještě neexistuje) můžete to provést zcela přímo pomocí rozhraní API pro správu horizontálního oddílu mapy.</span><span class="sxs-lookup"><span data-stu-id="75d7b-116">The elastic database [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md) can perform this operation, but if data movement is not necessary (for example, data for the range of days [25, 50), i.e., day 25 inclusive to 50 exclusive, does not yet exist) you can perform this entirely using the Shard Map Management APIs directly.</span></span>

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a><span data-ttu-id="75d7b-117">Příklad: rozdělení rozsah a přiřazování prázdný část horizontálních se nově přidané</span><span class="sxs-lookup"><span data-stu-id="75d7b-117">Example: splitting a range and assigning the empty portion to a newly-added shard</span></span>
<span data-ttu-id="75d7b-118">Databáze s názvem "sample_shard_2" a všechny objekty nezbytné schématu do něj byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="75d7b-118">A database named “sample_shard_2” and all necessary schema objects inside of it have been created.</span></span>  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 

        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

<span data-ttu-id="75d7b-119">**Důležité**: použijte tento postup, jen pokud jste si jisti, že rozsah aktualizované mapování je prázdný.</span><span class="sxs-lookup"><span data-stu-id="75d7b-119">**Important**:  Use this technique only if you are certain that the range for the updated mapping is empty.</span></span>  <span data-ttu-id="75d7b-120">Výše uvedené metody Neprovádět kontrolu dat pro rozsah přesouvání, proto je nejlepší zahrnout kontroly v kódu.</span><span class="sxs-lookup"><span data-stu-id="75d7b-120">The methods above do not check data for the range being moved, so it is best to include checks in your code.</span></span>  <span data-ttu-id="75d7b-121">Pokud řádky existují v rozsahu přesouvání, nebude odpovídat distribuční skutečná data mapy aktualizované horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="75d7b-121">If rows exist in the range being moved, the actual data distribution will not match the updated shard map.</span></span> <span data-ttu-id="75d7b-122">Použití [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md) k provedení operace místo v těchto případech.</span><span class="sxs-lookup"><span data-stu-id="75d7b-122">Use the [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md) to perform the operation instead in these cases.</span></span>  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

