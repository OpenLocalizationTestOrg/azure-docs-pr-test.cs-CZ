---
title: "Azure Service Fabric programový škálování | Microsoft Docs"
description: "Škálování clusteru služby Azure Service Fabric příchozí nebo odchozí prostřednictvím kódu programu, podle vlastní aktivační události"
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
ms.openlocfilehash: 46b0b62f92abbac57bc27bbcdd5821eafedf5519
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a><span data-ttu-id="208cc-103">Škálování clusteru Service Fabric prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="208cc-103">Scale a Service Fabric cluster programmatically</span></span> 

<span data-ttu-id="208cc-104">Základní informace o škálování clusteru Service Fabric v Azure jsou popsané v dokumentaci na [škálování clusterů](./service-fabric-cluster-scale-up-down.md).</span><span class="sxs-lookup"><span data-stu-id="208cc-104">Fundamentals of scaling a Service Fabric cluster in Azure are covered in documentation on [cluster scaling](./service-fabric-cluster-scale-up-down.md).</span></span> <span data-ttu-id="208cc-105">Tento článek popisuje, jak Service Fabric clustery jsou vytvořeny na základě sady škálování virtuálního počítače a je možné rozšířit ručně nebo pomocí pravidel automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="208cc-105">That article covers how Service Fabric clusters are built on top of virtual machine scale sets and can be scaled either manually or with auto-scale rules.</span></span> <span data-ttu-id="208cc-106">Tento dokument zjistí programové metody koordinující Azure škálování operací pro pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="208cc-106">This document looks at programmatic methods of coordinating Azure scaling operations for more advanced scenarios.</span></span> 

## <a name="reasons-for-programmatic-scaling"></a><span data-ttu-id="208cc-107">Důvody pro programové škálování</span><span class="sxs-lookup"><span data-stu-id="208cc-107">Reasons for programmatic scaling</span></span>
<span data-ttu-id="208cc-108">V mnoha scénářích škálování ručně nebo pomocí pravidel automatického škálování jsou dobré řešení.</span><span class="sxs-lookup"><span data-stu-id="208cc-108">In many scenarios, scaling manually or via auto-scale rules are good solutions.</span></span> <span data-ttu-id="208cc-109">V dalších scénářích ale jejich nemusí být vpravo fit.</span><span class="sxs-lookup"><span data-stu-id="208cc-109">In other scenarios, though, they may not be the right fit.</span></span> <span data-ttu-id="208cc-110">Potenciální nedostatky do těchto přístupů patří:</span><span class="sxs-lookup"><span data-stu-id="208cc-110">Potential drawbacks to these approaches include:</span></span>

- <span data-ttu-id="208cc-111">Ruční škálování vyžaduje, abyste se přihlaste a explicitní žádost o škálování operace.</span><span class="sxs-lookup"><span data-stu-id="208cc-111">Manually scaling requires you to log in and explicitly request scaling operations.</span></span> <span data-ttu-id="208cc-112">Pokud operace škálování se často vyžadují, nebo v nepředvídatelným časech, tento přístup nemusí být vhodné řešení.</span><span class="sxs-lookup"><span data-stu-id="208cc-112">If scaling operations are required frequently or at unpredictable times, this approach may not be a good solution.</span></span>
- <span data-ttu-id="208cc-113">Při pravidel automatického škálování odebrání instance škálovací sadu virtuálních počítačů, jejich neodebírejte automaticky znalosti o tomto uzlu z clusteru Service Fabric přidružené Pokud typ uzlu má úroveň odolnosti Silver nebo zlatý.</span><span class="sxs-lookup"><span data-stu-id="208cc-113">When auto-scale rules remove an instance from a virtual machine scale set, they do not automatically remove knowledge of that node from the associated Service Fabric cluster unless the node type has a durability level of Silver or Gold.</span></span> <span data-ttu-id="208cc-114">Protože pravidel automatického škálování fungovat ve velkém měřítku, nastavte úroveň (nikoli na úrovni Service Fabric), automatickému škálování pravidla můžete odebrat uzly Service Fabric bez jejich řádně vypínání.</span><span class="sxs-lookup"><span data-stu-id="208cc-114">Because auto-scale rules work at the scale set level (rather than at the Service Fabric level), auto-scale rules can remove Service Fabric nodes without shutting them down gracefully.</span></span> <span data-ttu-id="208cc-115">Odebrání tohoto článku neslušní uzlu ponechá 'neodstraněných' stav uzlu Service Fabric po škálování v operacích.</span><span class="sxs-lookup"><span data-stu-id="208cc-115">This rude node removal will leave 'ghost' Service Fabric node state behind after scale-in operations.</span></span> <span data-ttu-id="208cc-116">Pravidelně vyčistěte stav odebraném uzlu v clusteru Service Fabric potřebovat fyzickou (nebo služby).</span><span class="sxs-lookup"><span data-stu-id="208cc-116">An individual (or a service) would need to periodically clean up removed node state in the Service Fabric cluster.</span></span>
  - <span data-ttu-id="208cc-117">Všimněte si, že typ uzlu s úrovní odolnost zlatý nebo Silver bude automaticky vyčistit odebrat uzly.</span><span class="sxs-lookup"><span data-stu-id="208cc-117">Note that a node type with a durability level of Gold or Silver will automatically clean up removed nodes.</span></span>  
- <span data-ttu-id="208cc-118">I když jsou [mnoho metriky](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) podporovaná pravidel automatického škálování, je stále omezenou sadu.</span><span class="sxs-lookup"><span data-stu-id="208cc-118">Although there are [many metrics](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) supported by auto-scale rules, it is still a limited set.</span></span> <span data-ttu-id="208cc-119">Pokud váš scénář vyžaduje škálování podle některé metrika nejsou zahrnuté v dané sadě, pravidel automatického škálování, nemusí být vhodný.</span><span class="sxs-lookup"><span data-stu-id="208cc-119">If your scenario calls for scaling based on some metric not covered in that set, then auto-scale rules may not be a good option.</span></span>

<span data-ttu-id="208cc-120">Podle těchto omezení, můžete implementovat více přizpůsobené automatické škálování modelů.</span><span class="sxs-lookup"><span data-stu-id="208cc-120">Based on these limitations, you may wish to implement more customized automatic scaling models.</span></span> 

## <a name="scaling-apis"></a><span data-ttu-id="208cc-121">Škálování rozhraní API</span><span class="sxs-lookup"><span data-stu-id="208cc-121">Scaling APIs</span></span>
<span data-ttu-id="208cc-122">Rozhraní API Azure existují, které umožňují aplikacím prostřednictvím kódu programu pracovat s virtuálním počítačem škálování sady a clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="208cc-122">Azure APIs exist which allow applications to programmatically work with virtual machine scale sets and Service Fabric clusters.</span></span> <span data-ttu-id="208cc-123">Pokud pro váš scénář nepodporují existující možnosti automatického škálování, tato rozhraní API umožňují k implementaci vlastní logiky škálování.</span><span class="sxs-lookup"><span data-stu-id="208cc-123">If existing auto-scale options don't work for your scenario, these APIs make it possible to implement custom scaling logic.</span></span> 

<span data-ttu-id="208cc-124">Jeden ze způsobů implementace tato "domovské ze" funkce automatického škálování je přidání nové bezstavové služby do aplikace Service Fabric ke správě operací škálování.</span><span class="sxs-lookup"><span data-stu-id="208cc-124">One approach to implementing this 'home-made' auto-scaling functionality is to add a new stateless service to the Service Fabric application to manage scaling operations.</span></span> <span data-ttu-id="208cc-125">V rámci služby `RunAsync` metody sadu aktivační události můžete zjistit, jestli škálování vyžaduje (včetně kontrolu parametry, jako je například maximální velikost clusteru a škálování cooldowns).</span><span class="sxs-lookup"><span data-stu-id="208cc-125">Within the service's `RunAsync` method, a set of triggers can determine if scaling is required (including checking parameters such as maximum cluster size and scaling cooldowns).</span></span>   

<span data-ttu-id="208cc-126">Rozhraní API používá pro interakce sady škálování virtuálního počítače, (obojí ke kontrole aktuální počet instancí virtuálního počítače a upravit ho) je [fluent Azure Compute správy knihovny](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/).</span><span class="sxs-lookup"><span data-stu-id="208cc-126">The API used for virtual machine scale set interactions (both to check the current number of virtual machine instances and to modify it) is the [fluent Azure Management Compute library](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/).</span></span> <span data-ttu-id="208cc-127">Knihovna fluent výpočetní poskytuje rozhraní API snadno použitelné pro interakci s sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="208cc-127">The fluent compute library provides an easy-to-use API for interacting with virtual machine scale sets.</span></span>

<span data-ttu-id="208cc-128">Chcete-li pracovat s samotného clusteru Service Fabric, použijte [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).</span><span class="sxs-lookup"><span data-stu-id="208cc-128">To interact with the Service Fabric cluster itself, use [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).</span></span>

<span data-ttu-id="208cc-129">Samozřejmě kód škálování nepotřebuje pro spuštění jako služba v clusteru a škálovat.</span><span class="sxs-lookup"><span data-stu-id="208cc-129">Of course, the scaling code doesn't need to run as a service in the cluster to be scaled.</span></span> <span data-ttu-id="208cc-130">Obě `IAzure` a `FabricClient` může připojit k jejich přidružené prostředky Azure vzdáleně, tak službu škálování může být snadno konzolové aplikace nebo služba systému Windows spuštěna z mimo aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="208cc-130">Both `IAzure` and `FabricClient` can connect to their associated Azure resources remotely, so the scaling service could easily be a console application or Windows service running from outside the Service Fabric application.</span></span> 

## <a name="credential-management"></a><span data-ttu-id="208cc-131">Správa přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="208cc-131">Credential management</span></span>
<span data-ttu-id="208cc-132">Jeden výzvy zápisu služby pro zpracování škálování je, že je služba mít přístup k prostředkům sady škálování virtuálního počítače bez interaktivního přihlášení.</span><span class="sxs-lookup"><span data-stu-id="208cc-132">One challenge of writing a service to handle scaling is that the service must be able to access virtual machine scale set resources without an interactive login.</span></span> <span data-ttu-id="208cc-133">Přístup ke clusteru Service Fabric je snadné, pokud škálování služby upravuje vlastní aplikace Service Fabric, ale jsou přihlašovací údaje potřebné pro přístup k sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="208cc-133">Accessing the Service Fabric cluster is easy if the scaling service is modifying its own Service Fabric application, but credentials are needed to access the scale set.</span></span> <span data-ttu-id="208cc-134">K přihlášení, můžete použít [instanční objekt](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) vytvořené pomocí [Azure CLI 2.0](https://github.com/azure/azure-cli).</span><span class="sxs-lookup"><span data-stu-id="208cc-134">To log in, you can use a [service principal](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) created with the [Azure CLI 2.0](https://github.com/azure/azure-cli).</span></span>

<span data-ttu-id="208cc-135">Hlavní název služby může být vytvořen pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="208cc-135">A service principal can be created with the following steps:</span></span>

1. <span data-ttu-id="208cc-136">Přihlaste se k Azure CLI (`az login`) nastavit jako uživatel s přístupem k škálování virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="208cc-136">Log in to the Azure CLI (`az login`) as a user with access to the virtual machine scale set</span></span>
2. <span data-ttu-id="208cc-137">Vytvoření objektu zabezpečení pomocí služby`az ad sp create-for-rbac`</span><span class="sxs-lookup"><span data-stu-id="208cc-137">Create the service principal with `az ad sp create-for-rbac`</span></span>
    1. <span data-ttu-id="208cc-138">Poznamenejte si appId (nazývané "ID klienta, jinde), jméno, heslo a klienta pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="208cc-138">Make note of the appId (called 'client ID' elsewhere), name, password, and tenant for later use.</span></span>
    2. <span data-ttu-id="208cc-139">Budete také potřebovat vaše ID odběru, který lze zobrazit pomocí`az account list`</span><span class="sxs-lookup"><span data-stu-id="208cc-139">You will also need your subscription ID, which can be viewed with `az account list`</span></span>

<span data-ttu-id="208cc-140">Knihovna fluent výpočetní umožní přihlásit se pomocí těchto přihlašovacích údajů takto (Všimněte si, že základní fluent Azure typy jako `IAzure` v [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) balíčku):</span><span class="sxs-lookup"><span data-stu-id="208cc-140">The fluent compute library can log in using these credentials as follows (note that core fluent Azure types like `IAzure` are in the [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) package):</span></span>

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
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed to login to Azure");
}
```

<span data-ttu-id="208cc-141">Po přihlášení, počet instancí sady škálování může být dotazován prostřednictvím `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.</span><span class="sxs-lookup"><span data-stu-id="208cc-141">Once logged in, scale set instance count can be queried via `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.</span></span>

## <a name="scaling-out"></a><span data-ttu-id="208cc-142">Horizontální navýšení kapacity</span><span class="sxs-lookup"><span data-stu-id="208cc-142">Scaling out</span></span>
<span data-ttu-id="208cc-143">Použití fluent Azure výpočetní SDK, instancí mohou být přidány do sad s několika volání - škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="208cc-143">Using the fluent Azure compute SDK, instances can be added to the virtual machine scale set with just a few calls -</span></span>

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

<span data-ttu-id="208cc-144">Alternativně velikost sady škálování virtuálního počítače můžete také spravovat pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="208cc-144">Alternatively, virtual machine scale set size can also be managed with PowerShell cmdlets.</span></span> <span data-ttu-id="208cc-145">[`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss)můžete načíst objekt sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="208cc-145">[`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss) can retrieve the virtual machine scale set object.</span></span> <span data-ttu-id="208cc-146">Aktuální kapacita bude uložen v `.sku.capacity` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="208cc-146">The current capacity will be stored in the `.sku.capacity` property.</span></span> <span data-ttu-id="208cc-147">Po změně kapacitu na požadovanou hodnotu, můžete aktualizovat v Azure sad škálování virtuálního počítače [ `Update-AzureRmVmss` ](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) příkaz.</span><span class="sxs-lookup"><span data-stu-id="208cc-147">After changing the capacity to the desired value, the virtual machine scale set in Azure can be updated with the [`Update-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) command.</span></span>

<span data-ttu-id="208cc-148">Jako při přidávání uzlu ručně, musí být přidání nastavit instance škále všechno, co je potřeba spustit nový uzel Service Fabric od měřítka nastavení šablony zahrnuje rozšíření automaticky propojit nové instance služby cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="208cc-148">As when adding a node manually, adding a scale set instance should be all that's needed to start a new Service Fabric node since the scale set template includes extensions to automatically join new instances to the Service Fabric cluster.</span></span> 

## <a name="scaling-in"></a><span data-ttu-id="208cc-149">Nastavení velikosti v</span><span class="sxs-lookup"><span data-stu-id="208cc-149">Scaling in</span></span>

<span data-ttu-id="208cc-150">Změna velikosti v je podobná škálování.</span><span class="sxs-lookup"><span data-stu-id="208cc-150">Scaling in is similar to scaling out.</span></span> <span data-ttu-id="208cc-151">Skutečné škálovací sady virtuálních počítačů změny jsou téměř stejné.</span><span class="sxs-lookup"><span data-stu-id="208cc-151">The actual virtual machine scale set changes are practically the same.</span></span> <span data-ttu-id="208cc-152">Ale, jak bylo popsáno dříve, Service Fabric pouze automaticky vyčistí odebrané uzly se stálostí zlatý nebo Silver.</span><span class="sxs-lookup"><span data-stu-id="208cc-152">But, as was discussed previously, Service Fabric only automatically cleans up removed nodes with a durability of Gold or Silver.</span></span> <span data-ttu-id="208cc-153">Ano, v bronzová odolnost škálování v případě, je nutné k interakci se cluster Service Fabric vypnutí uzlu, který má být odebrána a pak odeberte jeho stav.</span><span class="sxs-lookup"><span data-stu-id="208cc-153">So, in the Bronze-durability scale-in case, it's necessary to interact with the Service Fabric cluster to shut down the node to be removed and then to remove its state.</span></span>

<span data-ttu-id="208cc-154">Příprava uzlu pro vypnutí zahrnuje hledání, že uzel, který má být odebrána (nedávno přidané uzel) a její deaktivace ho.</span><span class="sxs-lookup"><span data-stu-id="208cc-154">Preparing the node for shutdown involves finding the node to be removed (the most recently added node) and deactivating it.</span></span> <span data-ttu-id="208cc-155">Pro uzly – počáteční hodnoty novější uzlů najdete tak, že porovnáte `NodeInstanceId`.</span><span class="sxs-lookup"><span data-stu-id="208cc-155">For non-seed nodes, newer nodes can be found by comparing `NodeInstanceId`.</span></span> 

```C#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

<span data-ttu-id="208cc-156">Mějte na paměti, *počáteční hodnoty* uzly nevypadají vždy dodržovat konvence, že jsou větší ID instance nejdřív odstranit.</span><span class="sxs-lookup"><span data-stu-id="208cc-156">Be aware that *seed* nodes don't seem to always follow the convention that greater instance IDs are removed first.</span></span>

<span data-ttu-id="208cc-157">Jakmile je nalezen uzel, který má být odebrána, můžete deaktivovat a odebrat pomocí stejné `FabricClient` instance a `IAzure` instance z dříve.</span><span class="sxs-lookup"><span data-stu-id="208cc-157">Once the node to be removed is found, it can be deactivated and removed using the same `FabricClient` instance and the `IAzure` instance from earlier.</span></span>

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shutdown
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

<span data-ttu-id="208cc-158">Jako s škálování, rutiny prostředí PowerShell pro úpravy škálování virtuálních počítačů sady kapacity lze také zde Pokud skriptování přístup je vhodnější.</span><span class="sxs-lookup"><span data-stu-id="208cc-158">As with scaling out, PowerShell cmdlets for modifying virtual machine scale set capacity can also be used here if a scripting approach is preferable.</span></span> <span data-ttu-id="208cc-159">Odebraný instanci virtuálního počítače stav uzlu Service Fabric se může odebrat.</span><span class="sxs-lookup"><span data-stu-id="208cc-159">Once the virtual machine instance is removed, Service Fabric node state can be removed.</span></span>

```C#
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="potential-drawbacks"></a><span data-ttu-id="208cc-160">Potenciální nedostatky</span><span class="sxs-lookup"><span data-stu-id="208cc-160">Potential drawbacks</span></span>

<span data-ttu-id="208cc-161">Jak je předvedeno v předchozích fragmentů kódu, vytvoření vlastního škálování služby zajišťuje, že je na nejvyšší úrovni řízení a možnosti přizpůsobení přes aplikaci škálování chování.</span><span class="sxs-lookup"><span data-stu-id="208cc-161">As demonstrated in the preceding code snippets, creating your own scaling service provides the highest degree of control and customizability over your application's scaling behavior.</span></span> <span data-ttu-id="208cc-162">To může být užitečné pro scénářům, které vyžadují přesnou kontrolu nad při nebo jak aplikace škáluje příchozí nebo odchozí.</span><span class="sxs-lookup"><span data-stu-id="208cc-162">This can be useful for scenarios requiring precise control over when or how an application scales in or out.</span></span> <span data-ttu-id="208cc-163">Tento ovládací prvek se ale dodává s kompromis složitost kódu.</span><span class="sxs-lookup"><span data-stu-id="208cc-163">However, this control comes with a tradeoff of code complexity.</span></span> <span data-ttu-id="208cc-164">Tento přístup, znamená to, budete muset vlastní škálování kód, který je netriviální.</span><span class="sxs-lookup"><span data-stu-id="208cc-164">Using this approach means that you need to own scaling code, which is non-trivial.</span></span>

<span data-ttu-id="208cc-165">Jak by měl postupovat, Service Fabric škálování závisí na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="208cc-165">How you should approach Service Fabric scaling depends on your scenario.</span></span> <span data-ttu-id="208cc-166">Pokud škálování neobvyklé, možnost přidávat nebo odebírat uzly ručně je pravděpodobně dostatečná.</span><span class="sxs-lookup"><span data-stu-id="208cc-166">If scaling is uncommon, the ability to add or remove nodes manually is probably sufficient.</span></span> <span data-ttu-id="208cc-167">Složitější scénáře pravidel automatického škálování a sady SDK vystavení schopnost škálování prostřednictvím kódu programu nabízí výkonné alternativy.</span><span class="sxs-lookup"><span data-stu-id="208cc-167">For more complex scenarios, auto-scale rules and SDKs exposing the ability to scale programmatically offer powerful alternatives.</span></span>

## <a name="next-steps"></a><span data-ttu-id="208cc-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="208cc-168">Next steps</span></span>

<span data-ttu-id="208cc-169">Abyste mohli začít implementace vlastní logiky automatické škálování, seznamte se s následující koncepty a užitečné rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="208cc-169">To get started implementing your own auto-scaling logic, familiarize yourself with the following concepts and useful APIs:</span></span>

- [<span data-ttu-id="208cc-170">Škálování ručně nebo pomocí pravidel automatického škálování</span><span class="sxs-lookup"><span data-stu-id="208cc-170">Scaling manually or with auto-scale rules</span></span>](./service-fabric-cluster-scale-up-down.md)
- <span data-ttu-id="208cc-171">[Fluent správy knihovny Azure pro .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (je to užitečné pro interakci s cluster Service Fabric základní sady škálování virtuálního počítače)</span><span class="sxs-lookup"><span data-stu-id="208cc-171">[Fluent Azure Management Libraries for .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (useful for interacting with a Service Fabric cluster's underlying virtual machine scale sets)</span></span>
- <span data-ttu-id="208cc-172">[System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (je to užitečné pro interakci s cluster Service Fabric a jeho uzly)</span><span class="sxs-lookup"><span data-stu-id="208cc-172">[System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (useful for interacting with a Service Fabric cluster and its nodes)</span></span>
