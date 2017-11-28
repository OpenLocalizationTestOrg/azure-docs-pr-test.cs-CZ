---
title: aaaHow tooScale Azure Redis Cache | Microsoft Docs
description: "Zjistěte, jak tooscale vaší služby Azure Redis instancích mezipaměti"
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
ms.openlocfilehash: 8d7c015a539f872913056392aa080bf3f445bd03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-azure-redis-cache"></a><span data-ttu-id="40694-103">Jak tooScale Azure mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="40694-103">How tooScale Azure Redis Cache</span></span>
<span data-ttu-id="40694-104">Azure Redis Cache má jiný mezipaměti nabídky, které poskytují flexibilitu při výběru hello velikost mezipaměti a funkcí.</span><span class="sxs-lookup"><span data-stu-id="40694-104">Azure Redis Cache has different cache offerings, which provide flexibility in hello choice of cache size and features.</span></span> <span data-ttu-id="40694-105">Po vytvoření mezipaměti můžete škálovat hello velikost a hello cenová úroveň mezipaměti hello, pokud se změní hello požadavky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="40694-105">After a cache is created, you can scale hello size and hello pricing tier of hello cache if hello requirements of your application change.</span></span> <span data-ttu-id="40694-106">Tento článek ukazuje, jak tooscale svoji mezipaměť hello portál Azure a pomocí nástroje například Azure PowerShell a rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="40694-106">This article shows you how tooscale your cache in both hello Azure portal and using tools such as Azure PowerShell and Azure CLI.</span></span>

## <a name="when-tooscale"></a><span data-ttu-id="40694-107">Když tooscale</span><span class="sxs-lookup"><span data-stu-id="40694-107">When tooscale</span></span>
<span data-ttu-id="40694-108">Můžete použít hello [monitorování](cache-how-to-monitor.md) funkce Azure Redis Cache toomonitor hello stav a výkon vaší mezipaměti a pomůže vám určit, kdy tooscale hello mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="40694-108">You can use hello [monitoring](cache-how-to-monitor.md) features of Azure Redis Cache toomonitor hello health and performance of your cache and help determine when tooscale hello cache.</span></span> 

<span data-ttu-id="40694-109">Můžete monitorovat následující hello metriky toohelp určete, zda je nutné tooscale.</span><span class="sxs-lookup"><span data-stu-id="40694-109">You can monitor hello following metrics toohelp determine if you need tooscale.</span></span>

* <span data-ttu-id="40694-110">Zatížení serveru redis</span><span class="sxs-lookup"><span data-stu-id="40694-110">Redis Server Load</span></span>
* <span data-ttu-id="40694-111">Využití paměti</span><span class="sxs-lookup"><span data-stu-id="40694-111">Memory Usage</span></span>
* <span data-ttu-id="40694-112">Šířka pásma sítě</span><span class="sxs-lookup"><span data-stu-id="40694-112">Network Bandwidth</span></span>
* <span data-ttu-id="40694-113">Využití procesoru</span><span class="sxs-lookup"><span data-stu-id="40694-113">CPU Usage</span></span>

<span data-ttu-id="40694-114">Pokud zjistíte, že mezipaměť již splňuje požadavky vaší aplikace, je možné škálovat tooa větší nebo menší mezipaměti cenovou úroveň, která je pro vaši aplikaci nejvhodnější.</span><span class="sxs-lookup"><span data-stu-id="40694-114">If you determine that your cache is no longer meeting your application's requirements, you can scale tooa larger or smaller cache pricing tier that is right for your application.</span></span> <span data-ttu-id="40694-115">Další informace o určení, které mezipaměti cenová úroveň toouse najdete v tématu [jaké mezipaměť Redis nabídky a velikosti použít](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span><span class="sxs-lookup"><span data-stu-id="40694-115">For more information on determining which cache pricing tier toouse, see [What Redis Cache offering and size should I use](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span></span>

## <a name="scale-a-cache"></a><span data-ttu-id="40694-116">Změnit velikost mezipaměti</span><span class="sxs-lookup"><span data-stu-id="40694-116">Scale a cache</span></span>
<span data-ttu-id="40694-117">tooscale vaší mezipaměti [procházet mezipaměti toohello](cache-configure.md#configure-redis-cache-settings) v hello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **škálování** z hello **prostředků nabídky**.</span><span class="sxs-lookup"><span data-stu-id="40694-117">tooscale your cache, [browse toohello cache](cache-configure.md#configure-redis-cache-settings) in hello [Azure portal](https://portal.azure.com) and click **Scale** from hello **Resource menu**.</span></span>

![Měřítko](./media/cache-how-to-scale/redis-cache-scale-menu.png)

<span data-ttu-id="40694-119">Vyberte hello potřeby cenová úroveň z hello **vyberte cenová úroveň** a klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="40694-119">Select hello desired pricing tier from hello **Select pricing tier** blade and click **Select**.</span></span>

![Cenová úroveň][redis-cache-pricing-tier-blade]


<span data-ttu-id="40694-121">Je možné škálovat tooa jinou cenovou úroveň s hello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="40694-121">You can scale tooa different pricing tier with hello following restrictions:</span></span>

* <span data-ttu-id="40694-122">Nelze škálovat z vyšší cenová úroveň tooa nižší cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="40694-122">You can't scale from a higher pricing tier tooa lower pricing tier.</span></span>
  * <span data-ttu-id="40694-123">Nelze škálovat od **Premium** mezipaměti dolů tooa **standardní** nebo **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="40694-123">You can't scale from a **Premium** cache down tooa **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="40694-124">Nelze škálovat od **standardní** mezipaměti dolů tooa **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="40694-124">You can't scale from a **Standard** cache down tooa **Basic** cache.</span></span>
* <span data-ttu-id="40694-125">Je možné škálovat od **základní** mezipaměti tooa **standardní** mezipaměti, ale nemůže změnit velikost hello v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="40694-125">You can scale from a **Basic** cache tooa **Standard** cache but you can't change hello size at hello same time.</span></span> <span data-ttu-id="40694-126">Pokud potřebujete jinou velikost, můžete provést následné velikost toohello potřeby operace škálování.</span><span class="sxs-lookup"><span data-stu-id="40694-126">If you need a different size, you can do a subsequent scaling operation toohello desired size.</span></span>
* <span data-ttu-id="40694-127">Nelze škálovat od **základní** mezipaměti přímo tooa **Premium** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="40694-127">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="40694-128">Musí škálování z **základní** příliš**standardní** v rámci jedné operace škálování a potom z **standardní** příliš**Premium** v následných škálování operace.</span><span class="sxs-lookup"><span data-stu-id="40694-128">You must scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="40694-129">Nelze škálovat z větší velikost dolů toohello **C0 (250 MB)** velikost.</span><span class="sxs-lookup"><span data-stu-id="40694-129">You can't scale from a larger size down toohello **C0 (250 MB)** size.</span></span>
 
<span data-ttu-id="40694-130">Při hello mezipaměti se škáluje toohello nové cenovou úroveň, **škálování** stav se zobrazí v hello **Redis Cache** okno.</span><span class="sxs-lookup"><span data-stu-id="40694-130">While hello cache is scaling toohello new pricing tier, a **Scaling** status is displayed in hello **Redis Cache** blade.</span></span>

![Škálování][redis-cache-scaling]

<span data-ttu-id="40694-132">Po dokončení škálování hello stav se změní z **škálování** příliš**systémem**.</span><span class="sxs-lookup"><span data-stu-id="40694-132">When scaling is complete, hello status changes from **Scaling** too**Running**.</span></span>

## <a name="how-tooautomate-a-scaling-operation"></a><span data-ttu-id="40694-133">Jak tooautomate škálování operace</span><span class="sxs-lookup"><span data-stu-id="40694-133">How tooautomate a scaling operation</span></span>
<span data-ttu-id="40694-134">V přidání tooscaling hello vaše instance mezipaměti v portálu Azure, můžete škálovat pomocí rutin prostředí PowerShell, rozhraní příkazového řádku Azure a pomocí hello Microsoft Azure Management knihovny (MAML).</span><span class="sxs-lookup"><span data-stu-id="40694-134">In addition tooscaling your cache instances in hello Azure portal, you can scale using PowerShell cmdlets, Azure CLI, and by using hello Microsoft Azure Management Libraries (MAML).</span></span> 

* [<span data-ttu-id="40694-135">Škálování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="40694-135">Scale using PowerShell</span></span>](#scale-using-powershell)
* [<span data-ttu-id="40694-136">Škálování pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="40694-136">Scale using Azure CLI</span></span>](#scale-using-azure-cli)
* [<span data-ttu-id="40694-137">Škálování pomocí MAML</span><span class="sxs-lookup"><span data-stu-id="40694-137">Scale using MAML</span></span>](#scale-using-maml)

### <a name="scale-using-powershell"></a><span data-ttu-id="40694-138">Škálování pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="40694-138">Scale using PowerShell</span></span>
<span data-ttu-id="40694-139">Je možné škálovat vaše instance služby Azure Redis Cache pomocí prostředí PowerShell pomocí hello [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) rutiny při hello `Size`, `Sku`, nebo `ShardCount` jsou upraveny vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="40694-139">You can scale your Azure Redis Cache instances with PowerShell by using hello [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet when hello `Size`, `Sku`, or `ShardCount` properties are modified.</span></span> <span data-ttu-id="40694-140">Hello následující příklad ukazuje, jak tooscale mezipaměti s názvem `myCache` tooa 2,5 GB mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="40694-140">hello following example shows how tooscale a cache named `myCache` tooa 2.5 GB cache.</span></span> 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

<span data-ttu-id="40694-141">Další informace o škálování pomocí prostředí PowerShell najdete v tématu [tooscale Redis mezipaměti pomocí prostředí Powershell](cache-howto-manage-redis-cache-powershell.md#scale).</span><span class="sxs-lookup"><span data-stu-id="40694-141">For more information on scaling with PowerShell, see [tooscale a Redis cache using Powershell](cache-howto-manage-redis-cache-powershell.md#scale).</span></span>

### <a name="scale-using-azure-cli"></a><span data-ttu-id="40694-142">Škálování pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="40694-142">Scale using Azure CLI</span></span>
<span data-ttu-id="40694-143">volání vaší instancí Azure Redis Cache pomocí rozhraní příkazového řádku Azure, tooscale hello `azure rediscache set` příkazu a předejte jí hello potřeby změny konfigurace, které zahrnují nové velikosti, sku nebo velikost clusteru, v závislosti na hello potřeby operace škálování.</span><span class="sxs-lookup"><span data-stu-id="40694-143">tooscale your Azure Redis Cache instances using Azure CLI, call hello `azure rediscache set` command and pass in hello desired configuration changes that include a new size, sku, or cluster size, depending on hello desired scaling operation.</span></span>

<span data-ttu-id="40694-144">Další informace o škálování s Azure CLI, najdete v části [změnit nastavení existujícího Redis Cache](cache-manage-cli.md#scale).</span><span class="sxs-lookup"><span data-stu-id="40694-144">For more information on scaling with Azure CLI, see [Change settings of an existing Redis Cache](cache-manage-cli.md#scale).</span></span>

### <a name="scale-using-maml"></a><span data-ttu-id="40694-145">Škálování pomocí MAML</span><span class="sxs-lookup"><span data-stu-id="40694-145">Scale using MAML</span></span>
<span data-ttu-id="40694-146">tooscale vaší instancí Azure Redis Cache pomocí hello [Microsoft Azure Management knihovny (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), volání hello `IRedisOperations.CreateOrUpdate` metoda a předejte jí hello novou velikost hello `RedisProperties.SKU.Capacity`.</span><span class="sxs-lookup"><span data-stu-id="40694-146">tooscale your Azure Redis Cache instances using hello [Microsoft Azure Management Libraries (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), call hello `IRedisOperations.CreateOrUpdate` method and pass in hello new size for hello `RedisProperties.SKU.Capacity`.</span></span>

    static void Main(string[] args)
    {
        // For instructions on getting hello access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // tooscale, set a new size for hello redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

<span data-ttu-id="40694-147">Další informace najdete v tématu hello [spravovat Redis Cache pomocí MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) ukázka.</span><span class="sxs-lookup"><span data-stu-id="40694-147">For more information, see hello [Manage Redis Cache using MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) sample.</span></span>

## <a name="scaling-faq"></a><span data-ttu-id="40694-148">Škálování – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="40694-148">Scaling FAQ</span></span>
<span data-ttu-id="40694-149">Hello následující seznam obsahuje odpovědi toocommonly kladené dotazy týkající se Azure Redis Cache škálování.</span><span class="sxs-lookup"><span data-stu-id="40694-149">hello following list contains answers toocommonly asked questions about Azure Redis Cache scaling.</span></span>

* [<span data-ttu-id="40694-150">Možné škálovat na, z a do mezipaměti Premium?</span><span class="sxs-lookup"><span data-stu-id="40694-150">Can I scale to, from, or within a Premium cache?</span></span>](#can-i-scale-to-from-or-within-a-premium-cache)
* [<span data-ttu-id="40694-151">Po škálování, je nutné provést toochange mé klíče název nebo přístupu k mezipaměti?</span><span class="sxs-lookup"><span data-stu-id="40694-151">After scaling, do I have toochange my cache name or access keys?</span></span>](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [<span data-ttu-id="40694-152">Jak funguje škálování</span><span class="sxs-lookup"><span data-stu-id="40694-152">How does scaling work?</span></span>](#how-does-scaling-work)
* [<span data-ttu-id="40694-153">Bude možné ztrátě dat z mé mezipaměti při škálování?</span><span class="sxs-lookup"><span data-stu-id="40694-153">Will I lose data from my cache during scaling?</span></span>](#will-i-lose-data-from-my-cache-during-scaling)
* [<span data-ttu-id="40694-154">Je databází. vlastní nastavení ovlivněných během změny měřítka?</span><span class="sxs-lookup"><span data-stu-id="40694-154">Is my custom databases setting affected during scaling?</span></span>](#is-my-custom-databases-setting-affected-during-scaling)
* [<span data-ttu-id="40694-155">Moje mezipaměti bude k dispozici během změny měřítka?</span><span class="sxs-lookup"><span data-stu-id="40694-155">Will my cache be available during scaling?</span></span>](#will-my-cache-be-available-during-scaling)
* [<span data-ttu-id="40694-156">Operace, které nejsou podporovány</span><span class="sxs-lookup"><span data-stu-id="40694-156">Operations that are not supported</span></span>](#operations-that-are-not-supported)
* [<span data-ttu-id="40694-157">Jak dlouho škálování trvá?</span><span class="sxs-lookup"><span data-stu-id="40694-157">How long does scaling take?</span></span>](#how-long-does-scaling-take)
* [<span data-ttu-id="40694-158">Jak poznám, škálování po dokončení?</span><span class="sxs-lookup"><span data-stu-id="40694-158">How can I tell when scaling is complete?</span></span>](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a><span data-ttu-id="40694-159">Možné škálovat na, z a do mezipaměti Premium?</span><span class="sxs-lookup"><span data-stu-id="40694-159">Can I scale to, from, or within a Premium cache?</span></span>
* <span data-ttu-id="40694-160">Nelze škálovat od **Premium** mezipaměti dolů tooa **základní** nebo **standardní** cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="40694-160">You can't scale from a **Premium** cache down tooa **Basic** or **Standard** pricing tier.</span></span>
* <span data-ttu-id="40694-161">Je možné škálovat od jednoho **Premium** cenová úroveň tooanother mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="40694-161">You can scale from one **Premium** cache pricing tier tooanother.</span></span>
* <span data-ttu-id="40694-162">Nelze škálovat od **základní** mezipaměti přímo tooa **Premium** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="40694-162">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="40694-163">Je nutné nejprve škálovat z **základní** příliš**standardní** v rámci jedné operace škálování a potom z **standardní** příliš**Premium** v následné operace škálování.</span><span class="sxs-lookup"><span data-stu-id="40694-163">You must first scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="40694-164">Pokud jste povolili clustering při vytváření vašeho **Premium** mezipaměti, můžete [změnit velikost clusteru hello](cache-how-to-premium-clustering.md#cluster-size).</span><span class="sxs-lookup"><span data-stu-id="40694-164">If you enabled clustering when you created your **Premium** cache, you can [change hello cluster size](cache-how-to-premium-clustering.md#cluster-size).</span></span> <span data-ttu-id="40694-165">Pokud vaše mezipaměť byl vytvořen bez clusteringu povoleno, nelze nakonfigurovat clustering později.</span><span class="sxs-lookup"><span data-stu-id="40694-165">If your cache was created without clustering enabled, you can't configure clustering at a later time.</span></span>
  
  <span data-ttu-id="40694-166">Další informace najdete v tématu [jak tooconfigure clusteringu pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-clustering.md).</span><span class="sxs-lookup"><span data-stu-id="40694-166">For more information, see [How tooconfigure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>

### <a name="after-scaling-do-i-have-toochange-my-cache-name-or-access-keys"></a><span data-ttu-id="40694-167">Po škálování, je nutné provést toochange mé klíče název nebo přístupu k mezipaměti?</span><span class="sxs-lookup"><span data-stu-id="40694-167">After scaling, do I have toochange my cache name or access keys?</span></span>
<span data-ttu-id="40694-168">Ne, název mezipaměti a klíče jsou beze změny během operace škálování.</span><span class="sxs-lookup"><span data-stu-id="40694-168">No, your cache name and keys are unchanged during a scaling operation.</span></span>

### <a name="how-does-scaling-work"></a><span data-ttu-id="40694-169">Jak funguje škálování</span><span class="sxs-lookup"><span data-stu-id="40694-169">How does scaling work?</span></span>
* <span data-ttu-id="40694-170">Když **základní** mezipaměti bude změněno měřítko tooa různou velikost je vypnutý a zřizovat nové mezipaměti pomocí hello novou velikost.</span><span class="sxs-lookup"><span data-stu-id="40694-170">When a **Basic** cache is scaled tooa different size, it is shut down and a new cache is provisioned using hello new size.</span></span> <span data-ttu-id="40694-171">Během této doby hello mezipaměti není k dispozici a dojde ke ztrátě všech dat v mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="40694-171">During this time, hello cache is unavailable and all data in hello cache is lost.</span></span>
* <span data-ttu-id="40694-172">Když **základní** mezipaměť je škálovat tooa **standardní** mezipaměti repliky mezipaměti je zřízený a hello data budou zkopírována z hello primární mezipaměti toohello repliky mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="40694-172">When a **Basic** cache is scaled tooa **Standard** cache, a replica cache is provisioned and hello data is copied from hello primary cache toohello replica cache.</span></span> <span data-ttu-id="40694-173">Hello mezipaměti zůstává k dispozici během hello škálování procesu.</span><span class="sxs-lookup"><span data-stu-id="40694-173">hello cache remains available during hello scaling process.</span></span>
* <span data-ttu-id="40694-174">Když **standardní** mezipaměť je škálovat tooa jinou velikost nebo tooa **Premium** mezipaměti, jednu z replik hello se vypne a znovu zřídit toohello nová velikost a hello data přenosu a hello jiných repliky provede převzetí služeb při selhání, než je znovu zřízený, podobný proces toohello, ke kterému dochází při selhání jednoho z uzlů mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="40694-174">When a **Standard** cache is scaled tooa different size or tooa **Premium** cache, one of hello replicas is shut down and re-provisioned toohello new size and hello data transferred over, and then hello other replica performs a failover before it is re-provisioned, similar toohello process that occurs during a failure of one of hello cache nodes.</span></span>

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a><span data-ttu-id="40694-175">Bude možné ztrátě dat z mé mezipaměti při škálování?</span><span class="sxs-lookup"><span data-stu-id="40694-175">Will I lose data from my cache during scaling?</span></span>
* <span data-ttu-id="40694-176">Když **základní** mezipaměti je škálovat tooa novou velikost, všechna data se ztratí a hello mezipaměti není k dispozici během hello operace škálování.</span><span class="sxs-lookup"><span data-stu-id="40694-176">When a **Basic** cache is scaled tooa new size, all data is lost and hello cache is unavailable during hello scaling operation.</span></span>
* <span data-ttu-id="40694-177">Když **základní** mezipaměť je škálovat tooa **standardní** mezipaměti, hello data v mezipaměti hello je obvykle zachovat.</span><span class="sxs-lookup"><span data-stu-id="40694-177">When a **Basic** cache is scaled tooa **Standard** cache, hello data in hello cache is typically preserved.</span></span>
* <span data-ttu-id="40694-178">Když **standardní** mezipaměť je škálovat tooa větší velikost nebo vrstvy, nebo **Premium** mezipaměti bude změněno měřítko tooa větší velikost, všechna data je obvykle zachovaná.</span><span class="sxs-lookup"><span data-stu-id="40694-178">When a **Standard** cache is scaled tooa larger size or tier, or a **Premium** cache is scaled tooa larger size, all data is typically preserved.</span></span> <span data-ttu-id="40694-179">Při zvětšení velikosti **standardní** nebo **Premium** mezipaměti dolů tooa menší velikost, data mohou být ztraceny v závislosti na tom, kolik dat je v mezipaměti hello související s novou velikost toohello, když bude změněno měřítko.</span><span class="sxs-lookup"><span data-stu-id="40694-179">When scaling a **Standard** or **Premium** cache down tooa smaller size, data may be lost depending on how much data is in hello cache related toohello new size when it is scaled.</span></span> <span data-ttu-id="40694-180">Pokud dojde ke ztrátě dat při škálování směrem dolů, klíče jsou vyřazování pomocí hello [allkeys lru](http://redis.io/topics/lru-cache) zásady vyřazení.</span><span class="sxs-lookup"><span data-stu-id="40694-180">If data is lost when scaling down, keys are evicted using hello [allkeys-lru](http://redis.io/topics/lru-cache) eviction policy.</span></span> 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a><span data-ttu-id="40694-181">Je databází. vlastní nastavení ovlivněných během změny měřítka?</span><span class="sxs-lookup"><span data-stu-id="40694-181">Is my custom databases setting affected during scaling?</span></span>
<span data-ttu-id="40694-182">Některé cenové úrovně mají různé [databáze omezení](cache-configure.md#databases), takže existují některé aspekty při škálování směrem dolů v případě nakonfigurovat vlastní hodnotu hello `databases` nastavení během vytváření mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="40694-182">Some pricing tiers have different [databases limits](cache-configure.md#databases), so there are some considerations when scaling down if you configured a custom value for hello `databases` setting during cache creation.</span></span>

* <span data-ttu-id="40694-183">Při příjmu tooa cenová úroveň s nižší `databases` limit než současná vrstva hello:</span><span class="sxs-lookup"><span data-stu-id="40694-183">When scaling tooa pricing tier with a lower `databases` limit than hello current tier:</span></span>
  * <span data-ttu-id="40694-184">Pokud používáte výchozí číslo hello `databases` tedy 16 pro všechny cenové úrovně, všechna data budou zachována.</span><span class="sxs-lookup"><span data-stu-id="40694-184">If you are using hello default number of `databases` which is 16 for all pricing tiers, no data is lost.</span></span>
  * <span data-ttu-id="40694-185">Pokud používáte vlastní počet `databases` který spadá do hello limity pro toowhich vrstvy hello jsou změny velikosti, to `databases` se zachovává nastavení a dojde ke ztrátě žádná data.</span><span class="sxs-lookup"><span data-stu-id="40694-185">If you are using a custom number of `databases` that falls within hello limits for hello tier toowhich you are scaling, this `databases` setting is retained and no data is lost.</span></span>
  * <span data-ttu-id="40694-186">Pokud používáte vlastní počet `databases` který překračuje omezení hello hello novou vrstvu, hello `databases` nastavení je snížený toohello omezení hello novou vrstvu a dojde ke ztrátě všech dat v databázích hello odebrat.</span><span class="sxs-lookup"><span data-stu-id="40694-186">If you are using a custom number of `databases` that exceeds hello limits of hello new tier, hello `databases` setting is lowered toohello limits of hello new tier and all data in hello removed databases is lost.</span></span>
* <span data-ttu-id="40694-187">Při příjmu tooa cenová úroveň s hello stejná nebo vyšší `databases` limit než současná vrstva hello vaší `databases` se zachovává nastavení a dojde ke ztrátě žádná data.</span><span class="sxs-lookup"><span data-stu-id="40694-187">When scaling tooa pricing tier with hello same or higher `databases` limit than hello current tier your `databases` setting is retained and no data is lost.</span></span>

<span data-ttu-id="40694-188">Všimněte si, že když Standard a Premium mezipamětí má SLA 99,9 % dostupnosti, bez ztráty dat smlouvy SLA.</span><span class="sxs-lookup"><span data-stu-id="40694-188">Note that while Standard and Premium caches have a 99.9% SLA for availability, there is no SLA for data loss.</span></span>

### <a name="will-my-cache-be-available-during-scaling"></a><span data-ttu-id="40694-189">Moje mezipaměti bude k dispozici během změny měřítka?</span><span class="sxs-lookup"><span data-stu-id="40694-189">Will my cache be available during scaling?</span></span>
* <span data-ttu-id="40694-190">**Standardní** a **Premium** mezipamětí zůstanou k dispozici během hello operace škálování.</span><span class="sxs-lookup"><span data-stu-id="40694-190">**Standard** and **Premium** caches remain available during hello scaling operation.</span></span>
* <span data-ttu-id="40694-191">**Základní** jsou offline během změny měřítka operations tooa jinou velikost mezipaměti, ale zůstanou dostupné při škálování z **základní** příliš**standardní**.</span><span class="sxs-lookup"><span data-stu-id="40694-191">**Basic** caches are offline during scaling operations tooa different size, but remain available when scaling from **Basic** too**Standard**.</span></span>

### <a name="operations-that-are-not-supported"></a><span data-ttu-id="40694-192">Operace, které nejsou podporovány</span><span class="sxs-lookup"><span data-stu-id="40694-192">Operations that are not supported</span></span>
* <span data-ttu-id="40694-193">Nelze škálovat z vyšší cenová úroveň tooa nižší cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="40694-193">You can't scale from a higher pricing tier tooa lower pricing tier.</span></span>
  * <span data-ttu-id="40694-194">Nelze škálovat od **Premium** mezipaměti dolů tooa **standardní** nebo **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="40694-194">You can't scale from a **Premium** cache down tooa **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="40694-195">Nelze škálovat od **standardní** mezipaměti dolů tooa **základní** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="40694-195">You can't scale from a **Standard** cache down tooa **Basic** cache.</span></span>
* <span data-ttu-id="40694-196">Je možné škálovat od **základní** mezipaměti tooa **standardní** mezipaměti, ale nemůže změnit velikost hello v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="40694-196">You can scale from a **Basic** cache tooa **Standard** cache but you can't change hello size at hello same time.</span></span> <span data-ttu-id="40694-197">Pokud potřebujete jinou velikost, můžete provést následné velikost toohello potřeby operace škálování.</span><span class="sxs-lookup"><span data-stu-id="40694-197">If you need a different size, you can do a subsequent scaling operation toohello desired size.</span></span>
* <span data-ttu-id="40694-198">Nelze škálovat od **základní** mezipaměti přímo tooa **Premium** mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="40694-198">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="40694-199">Je nutné nejprve škálovat z **základní** příliš**standardní** v rámci jedné operace škálování a potom z **standardní** příliš**Premium** v následné operace škálování.</span><span class="sxs-lookup"><span data-stu-id="40694-199">You must first scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="40694-200">Nelze škálovat z větší velikost dolů toohello **C0 (250 MB)** velikost.</span><span class="sxs-lookup"><span data-stu-id="40694-200">You can't scale from a larger size down toohello **C0 (250 MB)** size.</span></span>

<span data-ttu-id="40694-201">Pokud škálování operace selže, hello služba se pokusí toorevert hello operace a hello mezipaměti bude používat původní velikost toohello.</span><span class="sxs-lookup"><span data-stu-id="40694-201">If a scaling operation fails, hello service will try toorevert hello operation and hello cache will revert toohello original size.</span></span>

### <a name="how-long-does-scaling-take"></a><span data-ttu-id="40694-202">Jak dlouho škálování trvá?</span><span class="sxs-lookup"><span data-stu-id="40694-202">How long does scaling take?</span></span>
<span data-ttu-id="40694-203">Škálování trvá přibližně 20 minut v závislosti na tom, kolik dat je v mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="40694-203">Scaling takes approximately 20 minutes, depending on how much data is in hello cache.</span></span>

### <a name="how-can-i-tell-when-scaling-is-complete"></a><span data-ttu-id="40694-204">Jak poznám, škálování po dokončení?</span><span class="sxs-lookup"><span data-stu-id="40694-204">How can I tell when scaling is complete?</span></span>
<span data-ttu-id="40694-205">V portálu Azure hello se zobrazí hello škálování operace probíhá.</span><span class="sxs-lookup"><span data-stu-id="40694-205">In hello Azure portal you can see hello scaling operation in progress.</span></span> <span data-ttu-id="40694-206">Po dokončení škálování hello hello mezipaměti změně stavu příliš**systémem**.</span><span class="sxs-lookup"><span data-stu-id="40694-206">When scaling is complete, hello status of hello cache changes too**Running**.</span></span>

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



