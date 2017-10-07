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
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Škálování clusteru Service Fabric prostřednictvím kódu programu 

Základní informace o škálování clusteru Service Fabric v Azure jsou popsané v dokumentaci na [škálování clusterů](./service-fabric-cluster-scale-up-down.md). Tento článek popisuje, jak Service Fabric clustery jsou vytvořeny na základě sady škálování virtuálního počítače a je možné rozšířit ručně nebo pomocí pravidel automatického škálování. Tento dokument zjistí programové metody koordinující Azure škálování operací pro pokročilejší scénáře. 

## <a name="reasons-for-programmatic-scaling"></a>Důvody pro programové škálování
V mnoha scénářích škálování ručně nebo pomocí pravidel automatického škálování jsou dobré řešení. V dalších scénářích ale jejich nemusí být hello právo přizpůsobit. Potenciální přístupy toothese nevýhody patří:

- Ruční škálování vyžaduje toolog v a explicitně žádost o škálování operace. Pokud operace škálování se často vyžadují, nebo v nepředvídatelným časech, tento přístup nemusí být vhodné řešení.
- Pokud pravidel automatického škálování odebrání instance škálovací sadu virtuálních počítačů, budou neodebírejte automaticky znalosti o tomto uzlu z clusteru Service Fabric hello přidružené Pokud typ uzlu hello má úroveň odolnosti Silver nebo zlatý. Protože pravidel automatického škálování fungovat ve velkém měřítku hello nastavte úroveň (spíše než v hello Service Fabric úrovni), můžete pravidel automatického škálování odebrat uzly Service Fabric bez je řádně vypnutí. Odebrání tohoto článku neslušní uzlu ponechá 'neodstraněných' stav uzlu Service Fabric po škálování v operacích. Třeba tooperiodically vyčištění stavu odebraném uzlu v clusteru Service Fabric hello, fyzickou (nebo služby).
  - Všimněte si, že typ uzlu s úrovní odolnost zlatý nebo Silver bude automaticky vyčistit odebrat uzly.  
- I když jsou [mnoho metriky](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) podporovaná pravidel automatického škálování, je stále omezenou sadu. Pokud váš scénář vyžaduje škálování podle některé metrika nejsou zahrnuté v dané sadě, pravidel automatického škálování, nemusí být vhodný.

Podle těchto omezení, můžete tooimplement více přizpůsobené automatické škálování modelů. 

## <a name="scaling-apis"></a>Škálování rozhraní API
Rozhraní API Azure existují, které umožňují aplikace tooprogrammatically pracují s škálovací sady virtuálních počítačů a clusterů Service Fabric. Pokud pro váš scénář nepodporují existující možnosti automatického škálování, tato rozhraní API ho nastavit vlastní logiky škálování možné tooimplement. 

Jeden ze způsobů tooimplementing to "domovské ze" Automatické škálování funkce je tooadd nové bezstavové služby toohello Service Fabric aplikace toomanage operace škálování. V rámci služby hello `RunAsync` metody sadu aktivační události můžete zjistit, jestli škálování vyžaduje (včetně kontrolu parametry, jako je například maximální velikost clusteru a škálování cooldowns).   

Hello API používá pro interakce sady škálování virtuálního počítače (obě toocheck hello aktuální počet instancí virtuálního počítače a toomodify ji) je hello [fluent Azure Compute správy knihovny](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). Knihovna fluent výpočetní Hello poskytuje rozhraní API snadno použitelné pro interakci s sady škálování virtuálního počítače.

použít toointeract s samotný, cluster Service Fabric hello [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

Samozřejmě hello škálování kód nepotřebuje toorun jako službu v toobe clusteru hello škálovat. Obě `IAzure` a `FabricClient` může připojit tootheir související prostředky Azure vzdáleně, takže hello škálování služby může být snadno konzolové aplikace nebo služba systému Windows spuštěna z aplikace Service Fabric mimo hello. 

## <a name="credential-management"></a>Správa přihlašovacích údajů
Jeden výzvy zápisu služby toohandle škálování je, že je služba hello prostředky sady škálování dokáže tooaccess virtuálního počítače bez interaktivního přihlášení. Přístup k hello Service Fabric clusteru je snadno Pokud hello škálování služby upravuje vlastní aplikace Service Fabric, ale jsou přihlašovací údaje potřebné tooaccess hello sadě škálování. toolog v, můžete použít [instanční objekt](https://github.com/Azure/azure-sdk-for-net/blob/Fluent/AUTH.md#creating-a-service-principal-in-azure) vytvořené pomocí hello [Azure CLI 2.0](https://github.com/azure/azure-cli).

Hlavní název služby může být vytvořen pomocí hello následující kroky:

1. Přihlaste se toohello rozhraní příkazového řádku Azure (`az login`) jako uživatel s škálování virtuálních počítačů přístup toohello nastavit
2. Vytvoření hello instančního objektu s`az ad sp create-for-rbac`
    1. Poznamenejte si ID aplikace hello (nazývané "ID klienta, jinde), jméno, heslo a klienta pro pozdější použití.
    2. Budete také potřebovat vaše ID odběru, který lze zobrazit pomocí`az account list`

Hello fluent výpočetní knihovny umožní přihlásit se pomocí těchto přihlašovacích údajů takto (Všimněte si, že základní fluent Azure typy jako `IAzure` v hello [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) balíčku):

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

Po přihlášení, počet instancí sady škálování může být dotazován prostřednictvím `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.

## <a name="scaling-out"></a>Horizontální navýšení kapacity
Pomocí hello fluent Azure výpočetní SDK, instancí lze přidat toohello škálování virtuálních počítačů s několika volání-

```C#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

Alternativně velikost sady škálování virtuálního počítače můžete také spravovat pomocí rutin prostředí PowerShell. [`Get-AzureRmVmss`](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmss)můžete načíst objektu sady škálování virtuálního počítače hello. Hello aktuální kapacita bude uložen v hello `.sku.capacity` vlastnost. Po hodnota změna hello kapacity toohello požadovaného, mohou být aktualizovány škálování virtuálních počítačů hello nastavit v Azure s hello [ `Update-AzureRmVmss` ](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/update-azurermvmss) příkaz.

Jako při přidávání uzlu ručně, přidání nastavit instance škále by měly být všechny to je potřebné toostart zahrnuje nový uzel Service Fabric vzhledem k tomu, že šablona sady škálování hello rozšíření tooautomatically Připojte nový cluster Service Fabric toohello instance. 

## <a name="scaling-in"></a>Nastavení velikosti v

Změna velikosti v je podobné tooscaling limitu. škálovací sada Hello skutečné virtuálních počítačů, změny jsou prakticky hello stejné. Ale, jak bylo popsáno dříve, Service Fabric pouze automaticky vyčistí odebrané uzly se stálostí zlatý nebo Silver. Proto v hello bronzová odolnost škálování v případě, je nutné toointeract s hello Service Fabric clusteru tooshut dolů toobe hello uzel odebrán a pak tooremove její stav.

Příprava uzlu hello pro vypnutí zahrnuje hledání hello uzlu toobe odebrat (hello nedávno přidané uzel) a její deaktivace ho. Pro uzly – počáteční hodnoty novější uzlů najdete tak, že porovnáte `NodeInstanceId`. 

```C#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

Mějte na paměti, *počáteční hodnoty* uzly nevypadají tooalways podle konvence hello nejdřív odstranit větší ID instance jsou.

Jakmile toobe hello uzel odebrán nenajde, můžete deaktivovat a odebrané pomocí stejné hello `FabricClient` instance a hello `IAzure` instance z dříve.

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

Jako s škálování, rutiny prostředí PowerShell pro úpravy škálování virtuálních počítačů sady kapacity lze také zde Pokud skriptování přístup je vhodnější. Odebraný hello instanci virtuálního počítače stav uzlu Service Fabric se může odebrat.

```C#
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="potential-drawbacks"></a>Potenciální nedostatky

Jak je předvedeno v hello předcházející fragmenty kódu, vytváření škálování služby poskytuje nejvyšší úrovni hello řízení a možnosti přizpůsobení přes aplikace je škálování chování. To může být užitečné pro scénářům, které vyžadují přesnou kontrolu nad při nebo jak aplikace škáluje příchozí nebo odchozí. Tento ovládací prvek se ale dodává s kompromis složitost kódu. Tento přístup znamená, je nutné, aby tooown škálování kód, který je netriviální.

Jak by měl postupovat, Service Fabric škálování závisí na vašem scénáři. Pokud škálování neobvyklé, hello uzly tooadd nebo odeberte možnost ručně je pravděpodobně dostatečná. Složitější scénáře, pravidel automatického škálování a sady SDK prostřednictvím kódu programu vystavení hello možnost tooscale nabízí výkonné alternativy.

## <a name="next-steps"></a>Další kroky

tooget spuštěna implementace vlastní logiky automatické škálování, seznamte se s hello následující koncepty a užitečné rozhraní API:

- [Škálování ručně nebo pomocí pravidel automatického škálování](./service-fabric-cluster-scale-up-down.md)
- [Fluent správy knihovny Azure pro .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (je to užitečné pro interakci s cluster Service Fabric základní sady škálování virtuálního počítače)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (je to užitečné pro interakci s cluster Service Fabric a jeho uzly)
