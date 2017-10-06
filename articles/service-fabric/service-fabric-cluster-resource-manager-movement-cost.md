---
title: "Správce prostředků clusteru Service Fabric: pohyb náklady | Microsoft Docs"
description: "Přehled náklady na přesunutí pro služby Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f022f258-7bc0-4db4-aa85-8c6c8344da32
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 65d4ac73efffcf7b25b1e95da6f9012a9238cb75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-movement-cost"></a>Náklady na přesunutí služby
Faktor, který hello správce prostředků clusteru Service Fabric zvažuje při pokusu o toodetermine, jaké změny toomake tooa clusteru je hello náklady na tyto změny. Hello představu o "náklady" se prodávají vypnout proti kolik hello clusteru je možné zlepšit. Náklady se započítá při přesunu služby pro vyrovnání, Defragmentace a další požadavky. cílem Hello je toomeet hello požadavky v hello minimálně způsob rušivý nebo nákladné. 

Přesunutí čas procesoru náklady na služby a šířka pásma sítě na minimální. Pro stavové služby vyžaduje kopírování hello stav těchto služeb, využívání další paměti a disku. Minimalizovat náklady hello řešení této hello správce prostředků Azure Service Fabric clusteru se dodává s pomáhá zajistit, že nejsou prostředky clusteru hello stráví zbytečně. Však také nechcete, aby tooignore řešení, které by výrazně zlepšit hello přidělení prostředků v clusteru hello.

Hello správce prostředků clusteru má dva způsoby, kterými computing náklady a omezení je, že při pokusu o toomanage hello clusteru. První mechanismus Hello je jednoduše počítání každých přesunu, která by byla. Pokud dvě řešení se generují se o hello stejné vyrovnávat (skóre), pak hello správce prostředků clusteru upřednostní hello, jeden s hello nejnižší náklady (celkový počet přesune).

Tato strategie funguje správně. Ale stejně jako u výchozí nebo statické zatížení, není pravděpodobné v jakékoli komplexní systému, zda jsou si rovny přesune všechny. Některé jsou pravděpodobně toobe mnohem nákladnější.

## <a name="setting-move-costs"></a>Náklady na přesunutí nastavení 
Náklady na přesunutí výchozí hello pro službu můžete určit, když je vytvořeno:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

C#: 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up hello rest of hello ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Můžete také zadat nebo aktualizovat MoveCost dynamicky pro službu po vytvoření služby hello: 

PowerShell: 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a>Dynamicky zadání náklady na přesunutí na základě za repliky

Hello předchozí fragmenty kódu jsou všechny pro zadání MoveCost najednou celou službu samotnou službu mimo hello. Však přesunout náklady je velmi užitečné je při hello náklady na přesunutí objektu specifické služby změní přes jeho životnost. Vzhledem k tomu, že pravděpodobně hello sami služeb mít hello nejlepší představu o tom, jak nákladná jsou toomove daném okamžiku, je rozhraní API pro služby tooreport vlastní jednotlivých přesunutí náklady za běhu. 

C#:

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a>Dopad náklady na přesunutí
MoveCost má čtyři úrovně: nula, nízké, střední a vysokou. MoveCosts jsou relativní tooeach jiných, s výjimkou nula. Nulové náklady na přesunutí znamená, že pohyb je zdarma a neměli započítává hello skóre hello řešení. Nastavení vaší přesunutí náklady tooHigh nemá *není* zaručit repliky hello zůstává na jednom místě.

<center>
![Náklady na přesunutí jako faktor při výběru replik pro přesun][Image1]
</center>

MoveCost vám pomůže najít řešení hello že příčina celkové hello minimálně přerušení a jsou nejjednodušší tooachieve při stále přicházejících na ekvivalentní vyrovnávání. Služby představu o náklady může být relativní toomany věcí. Hello nejběžnější faktory při výpočtu vaše náklady na přesunutí jsou:

- Hello množství dat, zda má služba hello toomove nebo stav.
- Hello náklady na odpojení klientů. Přesunutí primární replice je obvykle dražší než hello náklady na přesunutí sekundární repliku.
- Hello náklady na přerušení operace během letu. Některé operace v hello data ukládat úroveň nebo jsou nákladná operace provedené v odpovědi tooa klienta volání. Po určité míry, které nechcete toostop je pokud nemusíte. Proto při operaci hello se děje, můžete zvýšit náklady na přesunutí hello této služby objektu tooreduce hello pravděpodobnost, že ji přesune. Pokud se provádí operaci hello, nastavíte back toonormal hello náklady.

## <a name="enabling-move-cost-in-your-cluster"></a>Povolení náklady na přesunutí v clusteru
Aby hello podrobnější MoveCosts toobe vzít v úvahu, MoveCost musí být povolen v clusteru. Bez tohoto nastavení hello výchozí režim počítání přesune se používá pro výpočet MoveCost a MoveCost sestavy jsou ignorovány.


ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "UseMoveCostReports",
          "value": "true"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Další kroky
- Správce prostředků clusteru Service Fabric používá v clusteru hello využívání toomanage metriky a kapacity. Další informace o metrikách toolearn a jak tooconfigure, podívejte se na [Správa spotřeby prostředků a zatížení v Service Fabric s metriky](service-fabric-cluster-resource-manager-metrics.md).
- toolearn o tom, jak hello správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru hello, podívejte se na [vyrovnávání cluster Service Fabric](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
