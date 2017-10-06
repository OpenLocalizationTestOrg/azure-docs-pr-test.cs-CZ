---
title: "aaaAdding a horizontálního oddílu pomocí nástroje elastické databáze | Microsoft Docs"
description: "Jak nastavit toouse elastické škálování rozhraní API tooadd nové tooa horizontálních horizontálních oddílů."
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
ms.openlocfilehash: f44b59578376d1238b3012a3cb52339978079f0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a><span data-ttu-id="42164-103">Přidání horizontálního oddílu pomocí nástroje elastické databáze</span><span class="sxs-lookup"><span data-stu-id="42164-103">Adding a shard using Elastic Database tools</span></span>
## <a name="tooadd-a-shard-for-a-new-range-or-key"></a><span data-ttu-id="42164-104">tooadd horizontálního oddílu pro nový rozsah nebo klíč</span><span class="sxs-lookup"><span data-stu-id="42164-104">tooadd a shard for a new range or key</span></span>
<span data-ttu-id="42164-105">Aplikace často potřebují toosimply přidat nová data toohandle horizontálních oddílů, kterou je očekáván z nových klíčů nebo rozsahy klíčů pro ID horizontálního oddílu mapu, která již existuje.</span><span class="sxs-lookup"><span data-stu-id="42164-105">Applications often need toosimply add new shards toohandle data that is expected from new keys or key ranges, for a shard map that already exists.</span></span> <span data-ttu-id="42164-106">Například aplikace horizontálně dělené podle ID klienta může potřebovat tooprovision nové horizontálního oddílu pro nového klienta, nebo horizontálně dělené měsíční dat může být nutné nové horizontálních zřídit před hello začátek každý nový měsíc.</span><span class="sxs-lookup"><span data-stu-id="42164-106">For example, an application sharded by Tenant ID may need tooprovision a new shard for a new tenant, or data sharded monthly may need a new shard provisioned before hello start of each new month.</span></span> 

<span data-ttu-id="42164-107">Pokud hello nový rozsah hodnot klíče již není součástí existující mapování, je velmi jednoduchý tooadd hello nové horizontálního oddílu a přidružení hello nový klíč nebo rozsah toothat horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="42164-107">If hello new range of key values is not already part of an existing mapping, it is very simple tooadd hello new shard and associate hello new key or range toothat shard.</span></span> 

### <a name="example--adding-a-shard-and-its-range-tooan-existing-shard-map"></a><span data-ttu-id="42164-108">Příklad: Přidání horizontálního oddílu a jeho rozsah tooan existující horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="42164-108">Example:  adding a shard and its range tooan existing shard map</span></span>
<span data-ttu-id="42164-109">Tato ukázka používá hello [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) hello [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) metody a vytvoří instanci hello [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) třídy.</span><span class="sxs-lookup"><span data-stu-id="42164-109">This sample uses hello [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) hello [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) methods, and creates an instance of hello [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) class.</span></span> <span data-ttu-id="42164-110">V ukázce hello níže, databáze s názvem **sample_shard_2** a všechny objekty nezbytné schématu do něj byly vytvořeny toohold rozsah [300, 400).</span><span class="sxs-lookup"><span data-stu-id="42164-110">In hello sample below, a database named **sample_shard_2** and all necessary schema objects inside of it have been created toohold range [300, 400).</span></span>  

    // sm is a RangeShardMap object.
    // Add a new shard toohold hello range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create hello mapping and associate it with hello new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


<span data-ttu-id="42164-111">Jako alternativu můžete použít Powershell toocreate nového správce mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="42164-111">As an alternative, you can use Powershell toocreate a new Shard Map Manager.</span></span> <span data-ttu-id="42164-112">Příkladem je k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="42164-112">An example is available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

## <a name="tooadd-a-shard-for-an-empty-part-of-an-existing-range"></a><span data-ttu-id="42164-113">tooadd horizontálního oddílu pro prázdný součástí stávající rozsah</span><span class="sxs-lookup"><span data-stu-id="42164-113">tooadd a shard for an empty part of an existing range</span></span>
<span data-ttu-id="42164-114">V některých případech je již namapována rozsah tooa horizontálních a částečně vyplněnou data, ale teď chcete nadcházející data toobe směrovanou tooa různých horizontálních.</span><span class="sxs-lookup"><span data-stu-id="42164-114">In some circumstances, you may have already mapped a range tooa shard and partially filled it with data, but you now want upcoming data toobe directed tooa different shard.</span></span> <span data-ttu-id="42164-115">Například můžete horizontálního oddílu den rozsahu a již přiděleno horizontálního oddílu tooa 50 dnů, ale dne 24 chcete tooland budoucí data v různých horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="42164-115">For example, you shard by day range and have already allocated 50 days tooa shard, but on day 24, you want future data tooland in a different shard.</span></span> <span data-ttu-id="42164-116">elastické databáze Hello [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md) mohou provést tuto operaci, ale pokud přesun dat není potřebné (například data pro hello rozsah dnů [25, 50), tj., včetně too50 den 25 vylučují, ještě neexistuje) můžete provést rozhraní API pro správu mapy horizontálního oddílu pomocí zcela hello přímo.</span><span class="sxs-lookup"><span data-stu-id="42164-116">hello elastic database [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md) can perform this operation, but if data movement is not necessary (for example, data for hello range of days [25, 50), i.e., day 25 inclusive too50 exclusive, does not yet exist) you can perform this entirely using hello Shard Map Management APIs directly.</span></span>

### <a name="example-splitting-a-range-and-assigning-hello-empty-portion-tooa-newly-added-shard"></a><span data-ttu-id="42164-117">Příklad: rozdělení rozsah a přiřazení hello prázdný část tooa nově přidané horizontálních</span><span class="sxs-lookup"><span data-stu-id="42164-117">Example: splitting a range and assigning hello empty portion tooa newly-added shard</span></span>
<span data-ttu-id="42164-118">Databáze s názvem "sample_shard_2" a všechny objekty nezbytné schématu do něj byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="42164-118">A database named “sample_shard_2” and all necessary schema objects inside of it have been created.</span></span>  

    // sm is a RangeShardMap object.
    // Add a new shard toohold hello range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 

        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split hello Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) toodifferent shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline tooa different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

<span data-ttu-id="42164-119">**Důležité**: použijte tento postup, jen pokud jste některé, které hello rozsah pro mapování hello aktualizovat je prázdný.</span><span class="sxs-lookup"><span data-stu-id="42164-119">**Important**:  Use this technique only if you are certain that hello range for hello updated mapping is empty.</span></span>  <span data-ttu-id="42164-120">metody Hello výše Neprovádět kontrolu pro rozsah hello přesouvání dat, proto je vhodné tooinclude zkontroluje ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="42164-120">hello methods above do not check data for hello range being moved, so it is best tooinclude checks in your code.</span></span>  <span data-ttu-id="42164-121">Pokud řádky existují v rozsahu hello přesouvání, nebude odpovídat hello skutečná data distribuční hello aktualizované horizontálních mapy.</span><span class="sxs-lookup"><span data-stu-id="42164-121">If rows exist in hello range being moved, hello actual data distribution will not match hello updated shard map.</span></span> <span data-ttu-id="42164-122">Použití hello [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md) tooperform hello operace místo v těchto případech.</span><span class="sxs-lookup"><span data-stu-id="42164-122">Use hello [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md) tooperform hello operation instead in these cases.</span></span>  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

