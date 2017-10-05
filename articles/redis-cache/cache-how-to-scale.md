---
title: "Postup škálování Azure Redis Cache | Microsoft Docs"
description: "Zjistěte, jak se škálovat vaše instance služby Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 350db214-3b7c-4877-bd43-fef6df2db96c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: sdanie
ms.openlocfilehash: 91b3580491a1e3504a3891b66606a9bd18c0638f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-scale-azure-redis-cache"></a><span data-ttu-id="1241c-103">Postup škálování Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="1241c-103">How to Scale Azure Redis Cache</span></span>
<span data-ttu-id="1241c-104">Azure Redis Cache má jiný mezipaměti nabídky, které poskytují flexibilitu při výběru velikost mezipaměti a funkce.</span><span class="sxs-lookup"><span data-stu-id="1241c-104">Azure Redis Cache has different cache offerings, which provide flexibility in the choice of cache size and features.</span></span> <span data-ttu-id="1241c-105">Po vytvoření mezipaměti je možné škálovat na velikost a cenovou úroveň mezipaměti, pokud se změní požadavky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1241c-105">After a cache is created, you can scale the size and the pricing tier of the cache if the requirements of your application change.</span></span> <span data-ttu-id="1241c-106">Tento článek ukazuje, jak se škálovat vaši mezipaměť na portálu Azure a pomocí nástrojů, jako je Azure PowerShell a rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1241c-106">This article shows you how to scale your cache in both the Azure portal and using tools such as Azure PowerShell and Azure CLI.</span></span>

## <a name="when-to-scale"></a><span data-ttu-id="1241c-107">Kdy škálovat</span><span class="sxs-lookup"><span data-stu-id="1241c-107">When to scale</span></span>
<span data-ttu-id="1241c-108">Můžete použít [monitorování](cache-how-to-monitor.md) funkce Azure Redis Cache ke sledování stavu a výkonu vaší mezipaměti a zjistit, kdy škálovat mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-108">You can use the [monitoring](cache-how-to-monitor.md) features of Azure Redis Cache to monitor the health and performance of your cache and help determine when to scale the cache.</span></span> 

<span data-ttu-id="1241c-109">Můžete monitorovat následující metriky, které vám pomohou určit, pokud potřebujete škálování.</span><span class="sxs-lookup"><span data-stu-id="1241c-109">You can monitor the following metrics to help determine if you need to scale.</span></span>

* <span data-ttu-id="1241c-110">Zatížení serveru redis</span><span class="sxs-lookup"><span data-stu-id="1241c-110">Redis Server Load</span></span>
* <span data-ttu-id="1241c-111">Využití paměti</span><span class="sxs-lookup"><span data-stu-id="1241c-111">Memory Usage</span></span>
* <span data-ttu-id="1241c-112">Šířka pásma sítě</span><span class="sxs-lookup"><span data-stu-id="1241c-112">Network Bandwidth</span></span>
* <span data-ttu-id="1241c-113">Využití procesoru</span><span class="sxs-lookup"><span data-stu-id="1241c-113">CPU Usage</span></span>

<span data-ttu-id="1241c-114">Pokud zjistíte, že mezipaměť již splňuje požadavky vaší aplikace, je možné škálovat do mezipaměti větší nebo menší cenovou úroveň, která je pro vaši aplikaci nejvhodnější.</span><span class="sxs-lookup"><span data-stu-id="1241c-114">If you determine that your cache is no longer meeting your application's requirements, you can scale to a larger or smaller cache pricing tier that is right for your application.</span></span> <span data-ttu-id="1241c-115">Další informace o zjištění, které mezipaměti cenová úroveň používat najdete v tématu [jaké mezipaměť Redis nabídky a velikosti použít](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span><span class="sxs-lookup"><span data-stu-id="1241c-115">For more information on determining which cache pricing tier to use, see [What Redis Cache offering and size should I use](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span></span>

## <a name="scale-a-cache"></a><span data-ttu-id="1241c-116">Změnit velikost mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1241c-116">Scale a cache</span></span>
<span data-ttu-id="1241c-117">Škálování vaší mezipaměti [přejděte do mezipaměti](cache-configure.md#configure-redis-cache-settings) v [portál Azure](https://portal.azure.com) a klikněte na tlačítko **škálování** z **prostředků nabídky**.</span><span class="sxs-lookup"><span data-stu-id="1241c-117">To scale your cache, [browse to the cache](cache-configure.md#configure-redis-cache-settings) in the [Azure portal](https://portal.azure.com) and click **Scale** from the **Resource menu**.</span></span>

![Měřítko](./media/cache-how-to-scale/redis-cache-scale-menu.png)

<span data-ttu-id="1241c-119">Vyberte požadovanou cenovou úroveň z **vyberte cenová úroveň** a klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="1241c-119">Select the desired pricing tier from the **Select pricing tier** blade and click **Select**.</span></span>

![Cenová úroveň][redis-cache-pricing-tier-blade]


<span data-ttu-id="1241c-121">Je možné škálovat na jinou cenovou úroveň, s následujícími omezeními:</span><span class="sxs-lookup"><span data-stu-id="1241c-121">You can scale to a different pricing tier with the following restrictions:</span></span>

* <span data-ttu-id="1241c-122">Z používat vyšší cenová úroveň je nelze škálovat na nižší cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="1241c-122">You can't scale from a higher pricing tier to a lower pricing tier.</span></span>
  * <span data-ttu-id="1241c-123">Nelze škálovat od **Premium** dolů do mezipaměti **standardní** nebo **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-123">You can't scale from a **Premium** cache down to a **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="1241c-124">Nelze škálovat od **standardní** dolů do mezipaměti **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-124">You can't scale from a **Standard** cache down to a **Basic** cache.</span></span>
* <span data-ttu-id="1241c-125">Je možné škálovat od **základní** mezipaměti tak, aby **standardní** mezipaměti, ale nemůže změnit velikost ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="1241c-125">You can scale from a **Basic** cache to a **Standard** cache but you can't change the size at the same time.</span></span> <span data-ttu-id="1241c-126">Pokud potřebujete jinou velikost, můžete provést následující operaci škálování na požadovanou velikost.</span><span class="sxs-lookup"><span data-stu-id="1241c-126">If you need a different size, you can do a subsequent scaling operation to the desired size.</span></span>
* <span data-ttu-id="1241c-127">Nelze škálovat od **základní** přímo do mezipaměti **Premium** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-127">You can't scale from a **Basic** cache directly to a **Premium** cache.</span></span> <span data-ttu-id="1241c-128">Musí škálování z **základní** k **standardní** v rámci jedné operace škálování a potom z **standardní** k **Premium** v následných operaci škálování.</span><span class="sxs-lookup"><span data-stu-id="1241c-128">You must scale from **Basic** to **Standard** in one scaling operation, and then from **Standard** to **Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="1241c-129">Nelze škálovat z větší velikost dolů na **C0 (250 MB)** velikost.</span><span class="sxs-lookup"><span data-stu-id="1241c-129">You can't scale from a larger size down to the **C0 (250 MB)** size.</span></span>
 
<span data-ttu-id="1241c-130">Při mezipaměti je škálování na nové cenovou úroveň, **škálování** stav se zobrazí v **Redis Cache** okno.</span><span class="sxs-lookup"><span data-stu-id="1241c-130">While the cache is scaling to the new pricing tier, a **Scaling** status is displayed in the **Redis Cache** blade.</span></span>

![Škálování][redis-cache-scaling]

<span data-ttu-id="1241c-132">Po dokončení škálování se stav změní z **škálování** k **systémem**.</span><span class="sxs-lookup"><span data-stu-id="1241c-132">When scaling is complete, the status changes from **Scaling** to **Running**.</span></span>

## <a name="how-to-automate-a-scaling-operation"></a><span data-ttu-id="1241c-133">Jak automatizovat škálování operace</span><span class="sxs-lookup"><span data-stu-id="1241c-133">How to automate a scaling operation</span></span>
<span data-ttu-id="1241c-134">Kromě škálování vaší instance služby cache na portálu Azure, je možné škálovat pomocí rutin prostředí PowerShell, rozhraní příkazového řádku Azure a pomocí Microsoft Azure Management knihovny (MAML).</span><span class="sxs-lookup"><span data-stu-id="1241c-134">In addition to scaling your cache instances in the Azure portal, you can scale using PowerShell cmdlets, Azure CLI, and by using the Microsoft Azure Management Libraries (MAML).</span></span> 

* [<span data-ttu-id="1241c-135">Škálování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1241c-135">Scale using PowerShell</span></span>](#scale-using-powershell)
* [<span data-ttu-id="1241c-136">Škálování pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="1241c-136">Scale using Azure CLI</span></span>](#scale-using-azure-cli)
* [<span data-ttu-id="1241c-137">Škálování pomocí MAML</span><span class="sxs-lookup"><span data-stu-id="1241c-137">Scale using MAML</span></span>](#scale-using-maml)

### <a name="scale-using-powershell"></a><span data-ttu-id="1241c-138">Škálování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1241c-138">Scale using PowerShell</span></span>
<span data-ttu-id="1241c-139">Je možné škálovat vaše instance služby Azure Redis Cache pomocí prostředí PowerShell pomocí [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) rutiny při `Size`, `Sku`, nebo `ShardCount` jsou upraveny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1241c-139">You can scale your Azure Redis Cache instances with PowerShell by using the [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet when the `Size`, `Sku`, or `ShardCount` properties are modified.</span></span> <span data-ttu-id="1241c-140">Následující příklad ukazuje postup škálování mezipaměti s názvem `myCache` do mezipaměti 2,5 GB.</span><span class="sxs-lookup"><span data-stu-id="1241c-140">The following example shows how to scale a cache named `myCache` to a 2.5 GB cache.</span></span> 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

<span data-ttu-id="1241c-141">Další informace o škálování pomocí prostředí PowerShell najdete v tématu [škálování mezipaměti Redis pomocí prostředí Powershell](cache-howto-manage-redis-cache-powershell.md#scale).</span><span class="sxs-lookup"><span data-stu-id="1241c-141">For more information on scaling with PowerShell, see [To scale a Redis cache using Powershell](cache-howto-manage-redis-cache-powershell.md#scale).</span></span>

### <a name="scale-using-azure-cli"></a><span data-ttu-id="1241c-142">Škálování pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="1241c-142">Scale using Azure CLI</span></span>
<span data-ttu-id="1241c-143">Chcete-li škálovat vaše instance služby Azure Redis Cache pomocí rozhraní příkazového řádku Azure, volejte `azure rediscache set` příkazů a předat změny požadované konfigurace, které zahrnují novou velikost, sku nebo velikost clusteru, v závislosti na požadované operace škálování.</span><span class="sxs-lookup"><span data-stu-id="1241c-143">To scale your Azure Redis Cache instances using Azure CLI, call the `azure rediscache set` command and pass in the desired configuration changes that include a new size, sku, or cluster size, depending on the desired scaling operation.</span></span>

<span data-ttu-id="1241c-144">Další informace o škálování s Azure CLI, najdete v části [změnit nastavení existujícího Redis Cache](cache-manage-cli.md#scale).</span><span class="sxs-lookup"><span data-stu-id="1241c-144">For more information on scaling with Azure CLI, see [Change settings of an existing Redis Cache](cache-manage-cli.md#scale).</span></span>

### <a name="scale-using-maml"></a><span data-ttu-id="1241c-145">Škálování pomocí MAML</span><span class="sxs-lookup"><span data-stu-id="1241c-145">Scale using MAML</span></span>
<span data-ttu-id="1241c-146">Škálování Azure Redis Cache instance pomocí [Microsoft Azure Management knihovny (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), volání `IRedisOperations.CreateOrUpdate` metoda a předejte jí novou velikost `RedisProperties.SKU.Capacity`.</span><span class="sxs-lookup"><span data-stu-id="1241c-146">To scale your Azure Redis Cache instances using the [Microsoft Azure Management Libraries (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), call the `IRedisOperations.CreateOrUpdate` method and pass in the new size for the `RedisProperties.SKU.Capacity`.</span></span>

    static void Main(string[] args)
    {
        // For instructions on getting the access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // To scale, set a new size for the redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

<span data-ttu-id="1241c-147">Další informace najdete v tématu [spravovat Redis Cache pomocí MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) ukázka.</span><span class="sxs-lookup"><span data-stu-id="1241c-147">For more information, see the [Manage Redis Cache using MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) sample.</span></span>

## <a name="scaling-faq"></a><span data-ttu-id="1241c-148">Škálování – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="1241c-148">Scaling FAQ</span></span>
<span data-ttu-id="1241c-149">Následující seznam obsahuje odpovědi na nejčastější dotazy o škálování Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="1241c-149">The following list contains answers to commonly asked questions about Azure Redis Cache scaling.</span></span>

* [<span data-ttu-id="1241c-150">Možné škálovat na, z a do mezipaměti Premium?</span><span class="sxs-lookup"><span data-stu-id="1241c-150">Can I scale to, from, or within a Premium cache?</span></span>](#can-i-scale-to-from-or-within-a-premium-cache)
* [<span data-ttu-id="1241c-151">Po škálování, je nutné změnit mé klíče název nebo přístupu k mezipaměti?</span><span class="sxs-lookup"><span data-stu-id="1241c-151">After scaling, do I have to change my cache name or access keys?</span></span>](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [<span data-ttu-id="1241c-152">Jak funguje škálování</span><span class="sxs-lookup"><span data-stu-id="1241c-152">How does scaling work?</span></span>](#how-does-scaling-work)
* [<span data-ttu-id="1241c-153">Bude možné ztrátě dat z mé mezipaměti při škálování?</span><span class="sxs-lookup"><span data-stu-id="1241c-153">Will I lose data from my cache during scaling?</span></span>](#will-i-lose-data-from-my-cache-during-scaling)
* [<span data-ttu-id="1241c-154">Je databází. vlastní nastavení ovlivněných během změny měřítka?</span><span class="sxs-lookup"><span data-stu-id="1241c-154">Is my custom databases setting affected during scaling?</span></span>](#is-my-custom-databases-setting-affected-during-scaling)
* [<span data-ttu-id="1241c-155">Moje mezipaměti bude k dispozici během změny měřítka?</span><span class="sxs-lookup"><span data-stu-id="1241c-155">Will my cache be available during scaling?</span></span>](#will-my-cache-be-available-during-scaling)
* [<span data-ttu-id="1241c-156">Operace, které nejsou podporovány</span><span class="sxs-lookup"><span data-stu-id="1241c-156">Operations that are not supported</span></span>](#operations-that-are-not-supported)
* [<span data-ttu-id="1241c-157">Jak dlouho škálování trvá?</span><span class="sxs-lookup"><span data-stu-id="1241c-157">How long does scaling take?</span></span>](#how-long-does-scaling-take)
* [<span data-ttu-id="1241c-158">Jak poznám, škálování po dokončení?</span><span class="sxs-lookup"><span data-stu-id="1241c-158">How can I tell when scaling is complete?</span></span>](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a><span data-ttu-id="1241c-159">Možné škálovat na, z a do mezipaměti Premium?</span><span class="sxs-lookup"><span data-stu-id="1241c-159">Can I scale to, from, or within a Premium cache?</span></span>
* <span data-ttu-id="1241c-160">Nelze škálovat od **Premium** dolů do mezipaměti **základní** nebo **standardní** cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="1241c-160">You can't scale from a **Premium** cache down to a **Basic** or **Standard** pricing tier.</span></span>
* <span data-ttu-id="1241c-161">Je možné škálovat od jednoho **Premium** mezipaměti cenová úroveň na jiný.</span><span class="sxs-lookup"><span data-stu-id="1241c-161">You can scale from one **Premium** cache pricing tier to another.</span></span>
* <span data-ttu-id="1241c-162">Nelze škálovat od **základní** přímo do mezipaměti **Premium** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-162">You can't scale from a **Basic** cache directly to a **Premium** cache.</span></span> <span data-ttu-id="1241c-163">Je nutné nejprve škálovat z **základní** k **standardní** v rámci jedné operace škálování a potom z **standardní** k **Premium** následných operací škálování.</span><span class="sxs-lookup"><span data-stu-id="1241c-163">You must first scale from **Basic** to **Standard** in one scaling operation, and then from **Standard** to **Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="1241c-164">Pokud jste povolili clustering při vytváření vašeho **Premium** mezipaměti, můžete [změnit velikost clusteru](cache-how-to-premium-clustering.md#cluster-size).</span><span class="sxs-lookup"><span data-stu-id="1241c-164">If you enabled clustering when you created your **Premium** cache, you can [change the cluster size](cache-how-to-premium-clustering.md#cluster-size).</span></span> <span data-ttu-id="1241c-165">Pokud vaše mezipaměť byl vytvořen bez clusteringu povoleno, nelze nakonfigurovat clustering později.</span><span class="sxs-lookup"><span data-stu-id="1241c-165">If your cache was created without clustering enabled, you can't configure clustering at a later time.</span></span>
  
  <span data-ttu-id="1241c-166">Další informace najdete v článku [Postup konfigurace clusterů pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-clustering.md).</span><span class="sxs-lookup"><span data-stu-id="1241c-166">For more information, see [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a><span data-ttu-id="1241c-167">Po škálování, je nutné změnit mé klíče název nebo přístupu k mezipaměti?</span><span class="sxs-lookup"><span data-stu-id="1241c-167">After scaling, do I have to change my cache name or access keys?</span></span>
<span data-ttu-id="1241c-168">Ne, název mezipaměti a klíče jsou beze změny během operace škálování.</span><span class="sxs-lookup"><span data-stu-id="1241c-168">No, your cache name and keys are unchanged during a scaling operation.</span></span>

### <a name="how-does-scaling-work"></a><span data-ttu-id="1241c-169">Jak funguje škálování</span><span class="sxs-lookup"><span data-stu-id="1241c-169">How does scaling work?</span></span>
* <span data-ttu-id="1241c-170">Když **základní** mezipaměti se mění podle různých velikosti, se vypne a zřizovat nové mezipaměti pomocí nové velikosti.</span><span class="sxs-lookup"><span data-stu-id="1241c-170">When a **Basic** cache is scaled to a different size, it is shut down and a new cache is provisioned using the new size.</span></span> <span data-ttu-id="1241c-171">Během této doby je k dispozici mezipaměti a dojde ke ztrátě všech dat v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-171">During this time, the cache is unavailable and all data in the cache is lost.</span></span>
* <span data-ttu-id="1241c-172">Když **základní** mezipaměti rozšířit tak, aby **standardní** mezipaměti repliky mezipaměti je zřízený a data budou zkopírována z primární mezipaměti do mezipaměti repliky.</span><span class="sxs-lookup"><span data-stu-id="1241c-172">When a **Basic** cache is scaled to a **Standard** cache, a replica cache is provisioned and the data is copied from the primary cache to the replica cache.</span></span> <span data-ttu-id="1241c-173">Mezipaměti v zůstal dostupný během procesu škálování.</span><span class="sxs-lookup"><span data-stu-id="1241c-173">The cache remains available during the scaling process.</span></span>
* <span data-ttu-id="1241c-174">Když **standardní** mezipaměti je škálovat různé velikost nebo na **Premium** mezipaměti, jednu z replik se vypnout a znovu zřídit nové velikosti a přenosu dat, a potom provede další repliky převzetí služeb při selhání předtím, než je znovu zřízený, podobný procesu, ke kterému dochází při selhání jednoho z uzlů mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-174">When a **Standard** cache is scaled to a different size or to a **Premium** cache, one of the replicas is shut down and re-provisioned to the new size and the data transferred over, and then the other replica performs a failover before it is re-provisioned, similar to the process that occurs during a failure of one of the cache nodes.</span></span>

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a><span data-ttu-id="1241c-175">Bude možné ztrátě dat z mé mezipaměti při škálování?</span><span class="sxs-lookup"><span data-stu-id="1241c-175">Will I lose data from my cache during scaling?</span></span>
* <span data-ttu-id="1241c-176">Když **základní** mezipaměti se mění podle nové velikosti, dojde ke ztrátě všech dat a mezipaměti není k dispozici v průběhu operace škálování.</span><span class="sxs-lookup"><span data-stu-id="1241c-176">When a **Basic** cache is scaled to a new size, all data is lost and the cache is unavailable during the scaling operation.</span></span>
* <span data-ttu-id="1241c-177">Když **základní** mezipaměti rozšířit tak, aby **standardní** mezipaměti, data v mezipaměti je obvykle zachovaná.</span><span class="sxs-lookup"><span data-stu-id="1241c-177">When a **Basic** cache is scaled to a **Standard** cache, the data in the cache is typically preserved.</span></span>
* <span data-ttu-id="1241c-178">Když **standardní** mezipaměti je škálovat na větší velikost nebo vrstvy, nebo **Premium** mezipaměti je škálovat na větší velikost, je obvykle zachovají všechna data.</span><span class="sxs-lookup"><span data-stu-id="1241c-178">When a **Standard** cache is scaled to a larger size or tier, or a **Premium** cache is scaled to a larger size, all data is typically preserved.</span></span> <span data-ttu-id="1241c-179">Při zvětšení velikosti **standardní** nebo **Premium** mezipaměti na menší velikost, data mohou být ztraceny v závislosti na tom, kolik dat je v mezipaměti související s novou velikost, když bude změněno měřítko.</span><span class="sxs-lookup"><span data-stu-id="1241c-179">When scaling a **Standard** or **Premium** cache down to a smaller size, data may be lost depending on how much data is in the cache related to the new size when it is scaled.</span></span> <span data-ttu-id="1241c-180">Pokud dojde ke ztrátě dat při škálování směrem dolů, jsou vyřazování klíče pomocí [allkeys lru](http://redis.io/topics/lru-cache) zásady vyřazení.</span><span class="sxs-lookup"><span data-stu-id="1241c-180">If data is lost when scaling down, keys are evicted using the [allkeys-lru](http://redis.io/topics/lru-cache) eviction policy.</span></span> 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a><span data-ttu-id="1241c-181">Je databází. vlastní nastavení ovlivněných během změny měřítka?</span><span class="sxs-lookup"><span data-stu-id="1241c-181">Is my custom databases setting affected during scaling?</span></span>
<span data-ttu-id="1241c-182">Některé cenové úrovně mají různé [databáze omezení](cache-configure.md#databases), takže existují některé aspekty při škálování směrem dolů v případě nakonfigurovat vlastní hodnotu `databases` nastavení během vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-182">Some pricing tiers have different [databases limits](cache-configure.md#databases), so there are some considerations when scaling down if you configured a custom value for the `databases` setting during cache creation.</span></span>

* <span data-ttu-id="1241c-183">Při zvětšení velikosti na cenovou úroveň s nižší `databases` limit než současná vrstva:</span><span class="sxs-lookup"><span data-stu-id="1241c-183">When scaling to a pricing tier with a lower `databases` limit than the current tier:</span></span>
  * <span data-ttu-id="1241c-184">Pokud používáte výchozí počet `databases` tedy 16 pro všechny cenové úrovně, všechna data budou zachována.</span><span class="sxs-lookup"><span data-stu-id="1241c-184">If you are using the default number of `databases` which is 16 for all pricing tiers, no data is lost.</span></span>
  * <span data-ttu-id="1241c-185">Pokud používáte vlastní počet `databases` který spadá do limity pro vrstvu, do které jste se škálování, to `databases` se zachovává nastavení a dojde ke ztrátě žádná data.</span><span class="sxs-lookup"><span data-stu-id="1241c-185">If you are using a custom number of `databases` that falls within the limits for the tier to which you are scaling, this `databases` setting is retained and no data is lost.</span></span>
  * <span data-ttu-id="1241c-186">Pokud používáte vlastní počet `databases` který překračuje omezení novou vrstvu `databases` nastavení je snížena na omezení novou vrstvu a dojde ke ztrátě všech dat v odebrané databáze.</span><span class="sxs-lookup"><span data-stu-id="1241c-186">If you are using a custom number of `databases` that exceeds the limits of the new tier, the `databases` setting is lowered to the limits of the new tier and all data in the removed databases is lost.</span></span>
* <span data-ttu-id="1241c-187">Při škálování cenové úrovně se stejným nebo vyšším `databases` limit než současná vrstva vaší `databases` se zachovává nastavení a dojde ke ztrátě žádná data.</span><span class="sxs-lookup"><span data-stu-id="1241c-187">When scaling to a pricing tier with the same or higher `databases` limit than the current tier your `databases` setting is retained and no data is lost.</span></span>

<span data-ttu-id="1241c-188">Všimněte si, že když Standard a Premium mezipamětí má SLA 99,9 % dostupnosti, bez ztráty dat smlouvy SLA.</span><span class="sxs-lookup"><span data-stu-id="1241c-188">Note that while Standard and Premium caches have a 99.9% SLA for availability, there is no SLA for data loss.</span></span>

### <a name="will-my-cache-be-available-during-scaling"></a><span data-ttu-id="1241c-189">Moje mezipaměti bude k dispozici během změny měřítka?</span><span class="sxs-lookup"><span data-stu-id="1241c-189">Will my cache be available during scaling?</span></span>
* <span data-ttu-id="1241c-190">**Standardní** a **Premium** mezipamětí zůstanou dostupné během operace škálování.</span><span class="sxs-lookup"><span data-stu-id="1241c-190">**Standard** and **Premium** caches remain available during the scaling operation.</span></span>
* <span data-ttu-id="1241c-191">**Základní** jsou offline během změny měřítka operace, které mají různou velikost mezipaměti, ale zůstanou dostupné při škálování z **základní** k **standardní**.</span><span class="sxs-lookup"><span data-stu-id="1241c-191">**Basic** caches are offline during scaling operations to a different size, but remain available when scaling from **Basic** to **Standard**.</span></span>

### <a name="operations-that-are-not-supported"></a><span data-ttu-id="1241c-192">Operace, které nejsou podporovány</span><span class="sxs-lookup"><span data-stu-id="1241c-192">Operations that are not supported</span></span>
* <span data-ttu-id="1241c-193">Z používat vyšší cenová úroveň je nelze škálovat na nižší cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="1241c-193">You can't scale from a higher pricing tier to a lower pricing tier.</span></span>
  * <span data-ttu-id="1241c-194">Nelze škálovat od **Premium** dolů do mezipaměti **standardní** nebo **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-194">You can't scale from a **Premium** cache down to a **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="1241c-195">Nelze škálovat od **standardní** dolů do mezipaměti **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-195">You can't scale from a **Standard** cache down to a **Basic** cache.</span></span>
* <span data-ttu-id="1241c-196">Je možné škálovat od **základní** mezipaměti tak, aby **standardní** mezipaměti, ale nemůže změnit velikost ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="1241c-196">You can scale from a **Basic** cache to a **Standard** cache but you can't change the size at the same time.</span></span> <span data-ttu-id="1241c-197">Pokud potřebujete jinou velikost, můžete provést následující operaci škálování na požadovanou velikost.</span><span class="sxs-lookup"><span data-stu-id="1241c-197">If you need a different size, you can do a subsequent scaling operation to the desired size.</span></span>
* <span data-ttu-id="1241c-198">Nelze škálovat od **základní** přímo do mezipaměti **Premium** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-198">You can't scale from a **Basic** cache directly to a **Premium** cache.</span></span> <span data-ttu-id="1241c-199">Je nutné nejprve škálovat z **základní** k **standardní** v rámci jedné operace škálování a potom z **standardní** k **Premium** následných operací škálování.</span><span class="sxs-lookup"><span data-stu-id="1241c-199">You must first scale from **Basic** to **Standard** in one scaling operation, and then from **Standard** to **Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="1241c-200">Nelze škálovat z větší velikost dolů na **C0 (250 MB)** velikost.</span><span class="sxs-lookup"><span data-stu-id="1241c-200">You can't scale from a larger size down to the **C0 (250 MB)** size.</span></span>

<span data-ttu-id="1241c-201">Pokud škálování operace selže, služba se pokusí vrátit operaci a mezipaměti se vrátí k původní velikost.</span><span class="sxs-lookup"><span data-stu-id="1241c-201">If a scaling operation fails, the service will try to revert the operation and the cache will revert to the original size.</span></span>

### <a name="how-long-does-scaling-take"></a><span data-ttu-id="1241c-202">Jak dlouho škálování trvá?</span><span class="sxs-lookup"><span data-stu-id="1241c-202">How long does scaling take?</span></span>
<span data-ttu-id="1241c-203">Škálování trvá přibližně 20 minut v závislosti na tom, kolik dat je v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1241c-203">Scaling takes approximately 20 minutes, depending on how much data is in the cache.</span></span>

### <a name="how-can-i-tell-when-scaling-is-complete"></a><span data-ttu-id="1241c-204">Jak poznám, škálování po dokončení?</span><span class="sxs-lookup"><span data-stu-id="1241c-204">How can I tell when scaling is complete?</span></span>
<span data-ttu-id="1241c-205">Na portálu Azure najdete v probíhající operaci škálování.</span><span class="sxs-lookup"><span data-stu-id="1241c-205">In the Azure portal you can see the scaling operation in progress.</span></span> <span data-ttu-id="1241c-206">Po dokončení škálování stav mezipaměti se změní na **systémem**.</span><span class="sxs-lookup"><span data-stu-id="1241c-206">When scaling is complete, the status of the cache changes to **Running**.</span></span>

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



