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
ms.date: 10/17/2017
ms.author: mikerou
ms.openlocfilehash: 1744e3c49ac06abe9e1067d507fd56d694201ffc
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Škálování clusteru Service Fabric prostřednictvím kódu programu 

Základní informace o škálování clusteru Service Fabric v Azure jsou popsané v dokumentaci na [škálování clusterů](./service-fabric-cluster-scale-up-down.md). Tento článek popisuje, jak Service Fabric clustery jsou vytvořeny na základě sady škálování virtuálního počítače a je možné rozšířit ručně nebo pomocí pravidel automatického škálování. Tento dokument zjistí programové metody koordinující Azure škálování operací pro pokročilejší scénáře. 

## <a name="reasons-for-programmatic-scaling"></a>Důvody pro programové škálování
V mnoha scénářích škálování ručně nebo pomocí pravidel automatického škálování jsou dobré řešení. V dalších scénářích ale jejich nemusí být vpravo fit. Potenciální nedostatky do těchto přístupů patří:

- Ruční škálování vyžaduje, abyste se přihlaste a explicitní žádost o škálování operace. Pokud operace škálování se často vyžadují, nebo v nepředvídatelným časech, tento přístup nemusí být vhodné řešení.
- Při pravidel automatického škálování odebrání instance škálovací sadu virtuálních počítačů, jejich neodebírejte automaticky znalosti o tomto uzlu z clusteru Service Fabric přidružené Pokud typ uzlu má úroveň odolnosti Silver nebo zlatý. Protože pravidel automatického škálování fungovat ve velkém měřítku, nastavte úroveň (nikoli na úrovni Service Fabric), automatickému škálování pravidla můžete odebrat uzly Service Fabric bez jejich řádně vypínání. Odebrání tohoto článku neslušní uzlu ponechá 'neodstraněných' stav uzlu Service Fabric po škálování v operacích. Pravidelně vyčistěte stav odebraném uzlu v clusteru Service Fabric potřebovat fyzickou (nebo služby).
  - Typ uzlu s úrovní odolnost zlatý nebo Silver automaticky vyčistí odebrané uzly, je potřeba žádné další čištění.
- I když jsou [mnoho metriky](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) podporovaná pravidel automatického škálování, je stále omezenou sadu. Pokud váš scénář vyžaduje škálování podle některé metrika nejsou zahrnuté v dané sadě, pravidel automatického škálování, nemusí být vhodný.

Podle těchto omezení, můžete implementovat více přizpůsobené automatické škálování modelů. 

## <a name="scaling-apis"></a>Škálování rozhraní API
Rozhraní API Azure existují, které umožňují aplikacím prostřednictvím kódu programu pracovat s virtuálním počítačem škálování sady a clusterů Service Fabric. Pokud pro váš scénář nepodporují existující možnosti automatického škálování, tato rozhraní API umožňují k implementaci vlastní logiky škálování. 

Jeden ze způsobů implementace tato "domovské ze" funkce automatického škálování je přidání nové bezstavové služby do aplikace Service Fabric ke správě operací škálování. V rámci služby `RunAsync` metody sadu aktivační události můžete zjistit, jestli škálování vyžaduje (včetně kontrolu parametry, jako je například maximální velikost clusteru a škálování cooldowns).   

Rozhraní API používá pro interakce sady škálování virtuálního počítače, (obojí ke kontrole aktuální počet instancí virtuálního počítače a upravit ho) je [fluent Azure Compute správy knihovny](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). Knihovna fluent výpočetní poskytuje rozhraní API snadno použitelné pro interakci s sady škálování virtuálního počítače.

Chcete-li pracovat s samotného clusteru Service Fabric, použijte [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

Samozřejmě kód škálování nepotřebuje pro spuštění jako služba v clusteru a škálovat. Obě `IAzure` a `FabricClient` může připojit k jejich přidružené prostředky Azure vzdáleně, tak službu škálování může být snadno konzolové aplikace nebo služba systému Windows spuštěna z mimo aplikace Service Fabric. 

## <a name="credential-management"></a>Správa přihlašovacích údajů
Jeden výzvy zápisu služby pro zpracování škálování je, že je služba mít přístup k prostředkům sady škálování virtuálního počítače bez interaktivního přihlášení. Přístup ke clusteru Service Fabric je snadné, pokud škálování služby upravuje vlastní aplikace Service Fabric, ale jsou přihlašovací údaje potřebné pro přístup k sadě škálování. K přihlášení, můžete použít [instanční objekt](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli) vytvořené pomocí [Azure CLI 2.0](https://github.com/azure/azure-cli).

Hlavní název služby může být vytvořen pomocí následujících kroků:

1. Přihlaste se k Azure CLI (`az login`) nastavit jako uživatel s přístupem k škálování virtuálních počítačů
2. Vytvoření objektu zabezpečení pomocí služby`az ad sp create-for-rbac`
    1. Poznamenejte si appId (nazývané "ID klienta, jinde), jméno, heslo a klienta pro pozdější použití.
    2. Budete také potřebovat vaše ID odběru, který lze zobrazit pomocí`az account list`

Knihovna fluent výpočetní umožní přihlásit se pomocí těchto přihlašovacích údajů takto (Všimněte si, že základní fluent Azure typy jako `IAzure` v [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) balíčku):

```csharp
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

Po přihlášení, počet instancí sady škálování může být dotazován prostřednictvím `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.

## <a name="scaling-out"></a>Horizontální navýšení kapacity
Použití fluent Azure výpočetní SDK, instancí mohou být přidány do sad s několika volání - škálování virtuálního počítače

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

Alternativně velikost sady škálování virtuálního počítače můžete také spravovat pomocí rutin prostředí PowerShell. [`Get-AzureRmVmss`](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss)můžete načíst objekt sady škálování virtuálního počítače. Aktuální kapacita je k dispozici prostřednictvím `.sku.capacity` vlastnost. Po změně kapacitu na požadovanou hodnotu, můžete aktualizovat v Azure sad škálování virtuálního počítače [ `Update-AzureRmVmss` ](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) příkaz.

Jako při přidávání uzlu ručně, musí být přidání nastavit instance škále všechno, co je potřeba spustit nový uzel Service Fabric od měřítka nastavení šablony zahrnuje rozšíření automaticky propojit nové instance služby cluster Service Fabric. 

## <a name="scaling-in"></a>Nastavení velikosti v

Změna velikosti v je podobná škálování. Skutečné škálovací sady virtuálních počítačů změny jsou téměř stejné. Ale, jak bylo popsáno dříve, Service Fabric pouze automaticky vyčistí odebrané uzly se stálostí zlatý nebo Silver. Ano, v bronzová odolnost škálování v případě, je nutné k interakci se cluster Service Fabric vypnutí uzlu, který má být odebrána a pak odeberte jeho stav.

Příprava uzlu pro vypnutí zahrnuje hledání, že uzel, který má být odebrána (nedávno přidané uzel) a její deaktivace ho. Pro uzly – počáteční hodnoty novější uzlů najdete tak, že porovnáte `NodeInstanceId`. 

```csharp
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

Uzly počáteční hodnoty se liší a není nutně podle konvence, že jsou větší ID instance nejdřív odstranit.

Jakmile je nalezen uzel, který má být odebrána, můžete deaktivovat a odebrat pomocí stejné `FabricClient` instance a `IAzure` instance z dříve.

```csharp
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

Jako s škálování, rutiny prostředí PowerShell pro úpravy škálování virtuálních počítačů sady kapacity lze také zde Pokud skriptování přístup je vhodnější. Odebraný instanci virtuálního počítače stav uzlu Service Fabric se může odebrat.

```csharp
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="potential-drawbacks"></a>Potenciální nedostatky

Jak je předvedeno v předchozích fragmentů kódu, vytvoření vlastního škálování služby zajišťuje, že je na nejvyšší úrovni řízení a možnosti přizpůsobení přes aplikaci škálování chování. To může být užitečné pro scénářům, které vyžadují přesnou kontrolu nad při nebo jak aplikace škáluje příchozí nebo odchozí. Tento ovládací prvek se ale dodává s kompromis složitost kódu. Tento přístup, znamená to, budete muset vlastní škálování kód, který je netriviální.

Jak by měl postupovat, Service Fabric škálování závisí na vašem scénáři. Pokud škálování neobvyklé, možnost přidávat nebo odebírat uzly ručně je pravděpodobně dostatečná. Složitější scénáře pravidel automatického škálování a sady SDK vystavení schopnost škálování prostřednictvím kódu programu nabízí výkonné alternativy.

## <a name="next-steps"></a>Další postup

Abyste mohli začít implementace vlastní logiky automatické škálování, seznamte se s následující koncepty a užitečné rozhraní API:

- [Škálování ručně nebo pomocí pravidel automatického škálování](./service-fabric-cluster-scale-up-down.md)
- [Fluent správy knihovny Azure pro .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (je to užitečné pro interakci s cluster Service Fabric základní sady škálování virtuálního počítače)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (je to užitečné pro interakci s cluster Service Fabric a jeho uzly)
