---
title: "aaaAzure Service Fabric programový škálování | Microsoft Docs"
description: "Škálování clusteru služby Azure Service Fabric příchozí nebo odchozí prostřednictvím kódu programu, podle toocustom aktivační události"
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: mikerou
ms.openlocfilehash: a0c6499b1a143a173006248cf8a15380632637e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a><span data-ttu-id="c6a8a-103">Škálování clusteru Service Fabric prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="c6a8a-103">Scale a Service Fabric cluster programmatically</span></span> 

<span data-ttu-id="c6a8a-104">Základní informace o škálování clusteru Service Fabric v Azure jsou popsané v dokumentaci na [škálování clusterů](./service-fabric-cluster-scale-up-down.md).</span><span class="sxs-lookup"><span data-stu-id="c6a8a-104">Fundamentals of scaling a Service Fabric cluster in Azure are covered in documentation on [cluster scaling](./service-fabric-cluster-scale-up-down.md).</span></span> <span data-ttu-id="c6a8a-105">Tento článek popisuje, jak Service Fabric clustery jsou vytvořeny na základě sady škálování virtuálního počítače a je možné rozšířit ručně nebo pomocí pravidel automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-105">That article covers how Service Fabric clusters are built on top of virtual machine scale sets and can be scaled either manually or with auto-scale rules.</span></span> <span data-ttu-id="c6a8a-106">Tento dokument zjistí programové metody koordinující Azure škálování operací pro pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-106">This document looks at programmatic methods of coordinating Azure scaling operations for more advanced scenarios.</span></span> 

## <a name="reasons-for-programmatic-scaling"></a><span data-ttu-id="c6a8a-107">Důvody pro programové škálování</span><span class="sxs-lookup"><span data-stu-id="c6a8a-107">Reasons for programmatic scaling</span></span>
<span data-ttu-id="c6a8a-108">V mnoha scénářích škálování ručně nebo pomocí pravidel automatického škálování jsou dobré řešení.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-108">In many scenarios, scaling manually or via auto-scale rules are good solutions.</span></span> <span data-ttu-id="c6a8a-109">V dalších scénářích ale jejich nemusí být hello právo přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-109">In other scenarios, though, they may not be hello right fit.</span></span> <span data-ttu-id="c6a8a-110">Potenciální přístupy toothese nevýhody patří:</span><span class="sxs-lookup"><span data-stu-id="c6a8a-110">Potential drawbacks toothese approaches include:</span></span>

- <span data-ttu-id="c6a8a-111">Ruční škálování vyžaduje toolog v a explicitně žádost o škálování operace.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-111">Manually scaling requires you toolog in and explicitly request scaling operations.</span></span> <span data-ttu-id="c6a8a-112">Pokud operace škálování se často vyžadují, nebo v nepředvídatelným časech, tento přístup nemusí být vhodné řešení.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-112">If scaling operations are required frequently or at unpredictable times, this approach may not be a good solution.</span></span>
- <span data-ttu-id="c6a8a-113">Pokud pravidel automatického škálování odebrání instance škálovací sadu virtuálních počítačů, budou neodebírejte automaticky znalosti o tomto uzlu z clusteru Service Fabric hello přidružené Pokud typ uzlu hello má úroveň odolnosti Silver nebo zlatý.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-113">When auto-scale rules remove an instance from a virtual machine scale set, they do not automatically remove knowledge of that node from hello associated Service Fabric cluster unless hello node type has a durability level of Silver or Gold.</span></span> <span data-ttu-id="c6a8a-114">Protože pravidel automatického škálování fungovat ve velkém měřítku hello nastavte úroveň (spíše než v hello Service Fabric úrovni), můžete pravidel automatického škálování odebrat uzly Service Fabric bez je řádně vypnutí.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-114">Because auto-scale rules work at hello scale set level (rather than at hello Service Fabric level), auto-scale rules can remove Service Fabric nodes without shutting them down gracefully.</span></span> <span data-ttu-id="c6a8a-115">Odebrání tohoto článku neslušní uzlu ponechá 'neodstraněných' stav uzlu Service Fabric po škálování v operacích.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-115">This rude node removal will leave 'ghost' Service Fabric node state behind after scale-in operations.</span></span> <span data-ttu-id="c6a8a-116">Třeba tooperiodically vyčištění stavu odebraném uzlu v clusteru Service Fabric hello, fyzickou (nebo služby).</span><span class="sxs-lookup"><span data-stu-id="c6a8a-116">An individual (or a service) would need tooperiodically clean up removed node state in hello Service Fabric cluster.</span></span>
  - <span data-ttu-id="c6a8a-117">Všimněte si, že typ uzlu s úrovní odolnost zlatý nebo Silver bude automaticky vyčistit odebrat uzly.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-117">Note that a node type with a durability level of Gold or Silver will automatically clean up removed nodes.</span></span>  
- <span data-ttu-id="c6a8a-118">I když jsou [mnoho metriky](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) podporovaná pravidel automatického škálování, je stále omezenou sadu.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-118">Although there are [many metrics](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) supported by auto-scale rules, it is still a limited set.</span></span> <span data-ttu-id="c6a8a-119">Pokud váš scénář vyžaduje škálování podle některé metrika nejsou zahrnuté v dané sadě, pravidel automatického škálování, nemusí být vhodný.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-119">If your scenario calls for scaling based on some metric not covered in that set, then auto-scale rules may not be a good option.</span></span>

<span data-ttu-id="c6a8a-120">Podle těchto omezení, můžete tooimplement více přizpůsobené automatické škálování modelů.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-120">Based on these limitations, you may wish tooimplement more customized automatic scaling models.</span></span> 

## <a name="scaling-apis"></a><span data-ttu-id="c6a8a-121">Škálování rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c6a8a-121">Scaling APIs</span></span>
<span data-ttu-id="c6a8a-122">Rozhraní API Azure existují, které umožňují aplikace tooprogrammatically pracují s škálovací sady virtuálních počítačů a clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-122">Azure APIs exist which allow applications tooprogrammatically work with virtual machine scale sets and Service Fabric clusters.</span></span> <span data-ttu-id="c6a8a-123">Pokud pro váš scénář nepodporují existující možnosti automatického škálování, tato rozhraní API ho nastavit vlastní logiky škálování možné tooimplement.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-123">If existing auto-scale options don't work for your scenario, these APIs make it possible tooimplement custom scaling logic.</span></span> 

<span data-ttu-id="c6a8a-124">Jeden ze způsobů tooimplementing to "domovské ze" Automatické škálování funkce je tooadd nové bezstavové služby toohello Service Fabric aplikace toomanage operace škálování.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-124">One approach tooimplementing this 'home-made' auto-scaling functionality is tooadd a new stateless service toohello Service Fabric application toomanage scaling operations.</span></span> <span data-ttu-id="c6a8a-125">V rámci služby hello `RunAsync` metody sadu aktivační události můžete zjistit, jestli škálování vyžaduje (včetně kontrolu parametry, jako je například maximální velikost clusteru a škálování cooldowns).</span><span class="sxs-lookup"><span data-stu-id="c6a8a-125">Within hello service's `RunAsync` method, a set of triggers can determine if scaling is required (including checking parameters such as maximum cluster size and scaling cooldowns).</span></span>   

<span data-ttu-id="c6a8a-126">Hello API používá pro interakce sady škálování virtuálního počítače (obě toocheck hello aktuální počet instancí virtuálního počítače a toomodify ji) je hello [fluent Azure Compute správy knihovny](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/).</span><span class="sxs-lookup"><span data-stu-id="c6a8a-126">hello API used for virtual machine scale set interactions (both toocheck hello current number of virtual machine instances and toomodify it) is hello [fluent Azure Management Compute library](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/).</span></span> <span data-ttu-id="c6a8a-127">Knihovna fluent výpočetní Hello poskytuje rozhraní API snadno použitelné pro interakci s sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-127">hello fluent compute library provides an easy-to-use API for interacting with virtual machine scale sets.</span></span>

<span data-ttu-id="c6a8a-128">použít toointeract s samotný, cluster Service Fabric hello [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).</span><span class="sxs-lookup"><span data-stu-id="c6a8a-128">toointeract with hello Service Fabric cluster itself, use [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).</span></span>

<span data-ttu-id="c6a8a-129">Samozřejmě hello škálování kód nepotřebuje toorun jako službu v toobe clusteru hello škálovat.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-129">Of course, hello scaling code doesn't need toorun as a service in hello cluster toobe scaled.</span></span> <span data-ttu-id="c6a8a-130">Obě `IAzure` a `FabricClient` může připojit tootheir související prostředky Azure vzdáleně, takže hello škálování služby může být snadno konzolové aplikace nebo služba systému Windows spuštěna z aplikace Service Fabric mimo hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-130">Both `IAzure` and `FabricClient` can connect tootheir associated Azure resources remotely, so hello scaling service could easily be a console application or Windows service running from outside hello Service Fabric application.</span></span> 

## <a name="credential-management"></a><span data-ttu-id="c6a8a-131">Správa přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="c6a8a-131">Credential management</span></span>
<span data-ttu-id="c6a8a-132">Jeden výzvy zápisu služby toohandle škálování je, že je služba hello prostředky sady škálování dokáže tooaccess virtuálního počítače bez interaktivního přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-132">One challenge of writing a service toohandle scaling is that hello service must be able tooaccess virtual machine scale set resources without an interactive login.</span></span> <span data-ttu-id="c6a8a-133">Přístup k hello Service Fabric clusteru je snadno Pokud hello škálování služby upravuje vlastní aplikace Service Fabric, ale jsou přihlašovací údaje potřebné tooaccess hello sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-133">Accessing hello Service Fabric cluster is easy if hello scaling service is modifying its own Service Fabric application, but credentials are needed tooaccess hello scale set.</span></span> <span data-ttu-id="c6a8a-134">toolog v, můžete použít [instanční objekt](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) vytvořené pomocí hello [Azure CLI 2.0](https://github.com/azure/azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c6a8a-134">toolog in, you can use a [service principal](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) created with hello [Azure CLI 2.0](https://github.com/azure/azure-cli).</span></span>

<span data-ttu-id="c6a8a-135">Hlavní název služby může být vytvořen pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c6a8a-135">A service principal can be created with hello following steps:</span></span>

1. <span data-ttu-id="c6a8a-136">Přihlaste se toohello rozhraní příkazového řádku Azure (`az login`) jako uživatel s škálování virtuálních počítačů přístup toohello nastavit</span><span class="sxs-lookup"><span data-stu-id="c6a8a-136">Log in toohello Azure CLI (`az login`) as a user with access toohello virtual machine scale set</span></span>
2. <span data-ttu-id="c6a8a-137">Vytvoření hello instančního objektu s`az ad sp create-for-rbac`</span><span class="sxs-lookup"><span data-stu-id="c6a8a-137">Create hello service principal with `az ad sp create-for-rbac`</span></span>
    1. <span data-ttu-id="c6a8a-138">Poznamenejte si ID aplikace hello (nazývané "ID klienta, jinde), jméno, heslo a klienta pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-138">Make note of hello appId (called 'client ID' elsewhere), name, password, and tenant for later use.</span></span>
    2. <span data-ttu-id="c6a8a-139">Budete také potřebovat vaše ID odběru, který lze zobrazit pomocí`az account list`</span><span class="sxs-lookup"><span data-stu-id="c6a8a-139">You will also need your subscription ID, which can be viewed with `az account list`</span></span>

<span data-ttu-id="c6a8a-140">Hello fluent výpočetní knihovny umožní přihlásit se pomocí těchto přihlašovacích údajů takto (Všimněte si, že základní fluent Azure typy jako `IAzure` v hello [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) balíčku):</span><span class="sxs-lookup"><span data-stu-id="c6a8a-140">hello fluent compute library can log in using these credentials as follows (note that core fluent Azure types like `IAzure` are in hello [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) package):</span></span>

```C#
var credentials = new AzureCredentials(new ServicePrincipalLoginInformation {
                ClientId = AzureClientId,
                ClientSecret = 
                AzureClientKey }, AzureTenantId, AzureEnvironment.AzureGlobalCloud);
IAzure AzureClient = Azure.Authenticate(credentials).WithSubscription(AzureSubscriptionId);

if (AzureClient?.SubscriptionId == AzureSubscriptionId)
{
    ServiceEventSource.Current.ServiceMessage(Context, "Successfully logged into Azure");
}
else
{
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed toologin tooAzure");
}
```

<span data-ttu-id="c6a8a-141">Po přihlášení, počet instancí sady škálování může být dotazován prostřednictvím `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-141">Once logged in, scale set instance count can be queried via `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.</span></span>

## <a name="scaling-out"></a><span data-ttu-id="c6a8a-142">Horizontální navýšení kapacity</span><span class="sxs-lookup"><span data-stu-id="c6a8a-142">Scaling out</span></span>
<span data-ttu-id="c6a8a-143">Pomocí hello fluent Azure výpočetní SDK, instancí lze přidat toohello škálování virtuálních počítačů s několika volání-</span><span class="sxs-lookup"><span data-stu-id="c6a8a-143">Using hello fluent Azure compute SDK, instances can be added toohello virtual machine scale set with just a few calls -</span></span>

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

<span data-ttu-id="c6a8a-144">Alternativně velikost sady škálování virtuálního počítače můžete také spravovat pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-144">Alternatively, virtual machine scale set size can also be managed with PowerShell cmdlets.</span></span> <span data-ttu-id="c6a8a-145">[`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss)můžete načíst objektu sady škálování virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-145">[`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss) can retrieve hello virtual machine scale set object.</span></span> <span data-ttu-id="c6a8a-146">Hello aktuální kapacita bude uložen v hello `.sku.capacity` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-146">hello current capacity will be stored in hello `.sku.capacity` property.</span></span> <span data-ttu-id="c6a8a-147">Po hodnota změna hello kapacity toohello požadovaného, mohou být aktualizovány škálování virtuálních počítačů hello nastavit v Azure s hello [ `Update-AzureRmVmss` ](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-147">After changing hello capacity toohello desired value, hello virtual machine scale set in Azure can be updated with hello [`Update-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) command.</span></span>

<span data-ttu-id="c6a8a-148">Jako při přidávání uzlu ručně, přidání nastavit instance škále by měly být všechny to je potřebné toostart zahrnuje nový uzel Service Fabric vzhledem k tomu, že šablona sady škálování hello rozšíření tooautomatically Připojte nový cluster Service Fabric toohello instance.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-148">As when adding a node manually, adding a scale set instance should be all that's needed toostart a new Service Fabric node since hello scale set template includes extensions tooautomatically join new instances toohello Service Fabric cluster.</span></span> 

## <a name="scaling-in"></a><span data-ttu-id="c6a8a-149">Nastavení velikosti v</span><span class="sxs-lookup"><span data-stu-id="c6a8a-149">Scaling in</span></span>

<span data-ttu-id="c6a8a-150">Změna velikosti v je podobné tooscaling limitu. škálovací sada Hello skutečné virtuálních počítačů, změny jsou prakticky hello stejné.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-150">Scaling in is similar tooscaling out. hello actual virtual machine scale set changes are practically hello same.</span></span> <span data-ttu-id="c6a8a-151">Ale, jak bylo popsáno dříve, Service Fabric pouze automaticky vyčistí odebrané uzly se stálostí zlatý nebo Silver.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-151">But, as was discussed previously, Service Fabric only automatically cleans up removed nodes with a durability of Gold or Silver.</span></span> <span data-ttu-id="c6a8a-152">Proto v hello bronzová odolnost škálování v případě, je nutné toointeract s hello Service Fabric clusteru tooshut dolů toobe hello uzel odebrán a pak tooremove její stav.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-152">So, in hello Bronze-durability scale-in case, it's necessary toointeract with hello Service Fabric cluster tooshut down hello node toobe removed and then tooremove its state.</span></span>

<span data-ttu-id="c6a8a-153">Příprava uzlu hello pro vypnutí zahrnuje hledání hello uzlu toobe odebrat (hello nedávno přidané uzel) a její deaktivace ho.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-153">Preparing hello node for shutdown involves finding hello node toobe removed (hello most recently added node) and deactivating it.</span></span> <span data-ttu-id="c6a8a-154">Pro uzly – počáteční hodnoty novější uzlů najdete tak, že porovnáte `NodeInstanceId`.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-154">For non-seed nodes, newer nodes can be found by comparing `NodeInstanceId`.</span></span> 

```C#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

<span data-ttu-id="c6a8a-155">Mějte na paměti, *počáteční hodnoty* uzly nevypadají tooalways podle konvence hello nejdřív odstranit větší ID instance jsou.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-155">Be aware that *seed* nodes don't seem tooalways follow hello convention that greater instance IDs are removed first.</span></span>

<span data-ttu-id="c6a8a-156">Jakmile toobe hello uzel odebrán nenajde, můžete deaktivovat a odebrané pomocí stejné hello `FabricClient` instance a hello `IAzure` instance z dříve.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-156">Once hello node toobe removed is found, it can be deactivated and removed using hello same `FabricClient` instance and hello `IAzure` instance from earlier.</span></span>

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove hello node from hello Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up tooa timeout) for hello node toogracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

<span data-ttu-id="c6a8a-157">Jako s škálování, rutiny prostředí PowerShell pro úpravy škálování virtuálních počítačů sady kapacity lze také zde Pokud skriptování přístup je vhodnější.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-157">As with scaling out, PowerShell cmdlets for modifying virtual machine scale set capacity can also be used here if a scripting approach is preferable.</span></span> <span data-ttu-id="c6a8a-158">Odebraný hello instanci virtuálního počítače stav uzlu Service Fabric se může odebrat.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-158">Once hello virtual machine instance is removed, Service Fabric node state can be removed.</span></span>

```C#
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="potential-drawbacks"></a><span data-ttu-id="c6a8a-159">Potenciální nedostatky</span><span class="sxs-lookup"><span data-stu-id="c6a8a-159">Potential drawbacks</span></span>

<span data-ttu-id="c6a8a-160">Jak je předvedeno v hello předcházející fragmenty kódu, vytváření škálování služby poskytuje nejvyšší úrovni hello řízení a možnosti přizpůsobení přes aplikace je škálování chování.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-160">As demonstrated in hello preceding code snippets, creating your own scaling service provides hello highest degree of control and customizability over your application's scaling behavior.</span></span> <span data-ttu-id="c6a8a-161">To může být užitečné pro scénářům, které vyžadují přesnou kontrolu nad při nebo jak aplikace škáluje příchozí nebo odchozí. Tento ovládací prvek se ale dodává s kompromis složitost kódu.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-161">This can be useful for scenarios requiring precise control over when or how an application scales in or out. However, this control comes with a tradeoff of code complexity.</span></span> <span data-ttu-id="c6a8a-162">Tento přístup znamená, je nutné, aby tooown škálování kód, který je netriviální.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-162">Using this approach means that you need tooown scaling code, which is non-trivial.</span></span>

<span data-ttu-id="c6a8a-163">Jak by měl postupovat, Service Fabric škálování závisí na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-163">How you should approach Service Fabric scaling depends on your scenario.</span></span> <span data-ttu-id="c6a8a-164">Pokud škálování neobvyklé, hello uzly tooadd nebo odeberte možnost ručně je pravděpodobně dostatečná.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-164">If scaling is uncommon, hello ability tooadd or remove nodes manually is probably sufficient.</span></span> <span data-ttu-id="c6a8a-165">Složitější scénáře, pravidel automatického škálování a sady SDK prostřednictvím kódu programu vystavení hello možnost tooscale nabízí výkonné alternativy.</span><span class="sxs-lookup"><span data-stu-id="c6a8a-165">For more complex scenarios, auto-scale rules and SDKs exposing hello ability tooscale programmatically offer powerful alternatives.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6a8a-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6a8a-166">Next steps</span></span>

<span data-ttu-id="c6a8a-167">tooget spuštěna implementace vlastní logiky automatické škálování, seznamte se s hello následující koncepty a užitečné rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="c6a8a-167">tooget started implementing your own auto-scaling logic, familiarize yourself with hello following concepts and useful APIs:</span></span>

- [<span data-ttu-id="c6a8a-168">Škálování ručně nebo pomocí pravidel automatického škálování</span><span class="sxs-lookup"><span data-stu-id="c6a8a-168">Scaling manually or with auto-scale rules</span></span>](./service-fabric-cluster-scale-up-down.md)
- <span data-ttu-id="c6a8a-169">[Fluent správy knihovny Azure pro .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (je to užitečné pro interakci s cluster Service Fabric základní sady škálování virtuálního počítače)</span><span class="sxs-lookup"><span data-stu-id="c6a8a-169">[Fluent Azure Management Libraries for .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (useful for interacting with a Service Fabric cluster's underlying virtual machine scale sets)</span></span>
- <span data-ttu-id="c6a8a-170">[System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (je to užitečné pro interakci s cluster Service Fabric a jeho uzly)</span><span class="sxs-lookup"><span data-stu-id="c6a8a-170">[System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (useful for interacting with a Service Fabric cluster and its nodes)</span></span>
