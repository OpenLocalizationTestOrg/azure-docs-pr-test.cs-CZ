---
title: "aaaAzure řízení prostředků infrastruktury služby pro kontejnery a služby | Microsoft Docs"
description: "Azure Service Fabric umožňuje toospecify limitů prostředků pro služby spuštěné uvnitř nebo mimo kontejnery."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 34e368211d98ff6b5b294c9c8b3af5ca30eeb20c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resource-governance"></a>Zásady správného řízení prostředků 

Při spuštění více hello stejného uzlu nebo clusteru, je možné, že jedna služba může spotřebují více prostředků starving dalších služeb. Tento problém se označují tooas hello aktivní sousedním problém. Service Fabric umožňuje hello vývojáře toospecify rezervace a omezení na prostředky tooguarantee služby a také omezit jeho využití prostředků. 

## <a name="resource-governance-metrics"></a>Metrika prostředků zásad správného řízení 

Zásady správného řízení prostředků je podporována v Service Fabric na [balíček služby](service-fabric-application-model.md). Hello prostředky, které jsou přiřazeny tooService balíček lze dále rozdělit mezi balíčky kódu. zadané limity Hello prostředků přístup také znamenat hello rezervace hello prostředků. Service Fabric podporuje určení procesoru a paměti na balíček služby, pomocí dvě předdefinované [metriky](service-fabric-cluster-resource-manager-metrics.md):

* Procesor (název metriky `ServiceFabric:/_CpuCores`): jádro je logické jádra, která je dostupná na hello hostitelský počítač a všechny jader pro všechny uzly jsou vyváženy hello stejné.
* Paměť (název metriky `ServiceFabric:/_MemoryInMB`): paměti je vyjádřeno v megabajtech a mapuje toophysical paměti, který je dostupný na počítači hello.

Záruky vyhrazené logicky pouze jsou k dispozici - hello runtime odmítne otevření nové služby, které jsou překročil dostupné prostředky balíčky. Ale pokud jiný spustitelný soubor nebo kontejneru je umístěn na uzlu hello, který může porušení záruky vyhrazené původní hello.

Tyto dvě metriky hello [správce prostředků clusteru](service-fabric-cluster-resource-manager-cluster-description.md) sleduje celkové clusteru kapacitu, hello zatížení na každém uzlu v clusteru hello a zbývající prostředků v clusteru hello. Tyto dvě metriky jsou ekvivalentní tooany jiný uživatel nebo vlastní metriky a všechny existující funkce lze použít s nimi:
* Cluster může být [vyrovnáváním](service-fabric-cluster-resource-manager-balancing.md) podle metriky toothese dva (výchozí nastavení).
* Cluster může být [defragmentovat](service-fabric-cluster-resource-manager-defragmentation-metrics.md) podle toothese dva metriky.
* Když [popisující cluster](service-fabric-cluster-resource-manager-cluster-description.md), ve vyrovnávací paměti kapacity lze nastavit pro tyto dvě metriky.

[Dynamické načtení reporting](service-fabric-cluster-resource-manager-metrics.md) není podporována pro tyto metriky a načte pro tyto metriky, které jsou definovány v okamžiku vytvoření.

## <a name="cluster-set-up-for-enabling-resource-governance"></a>Cluster sady pro povolení zásad správného řízení prostředků

Kapacita musí být definovány ručně v každém typu uzlu v clusteru hello takto:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
Zásady správného řízení prostředků je povolen pouze na uživatele služby, nikoli na všechny systémové služby. Při zadávání kapacitu, některé jader a paměti musí být vlevo volné pro systémových služeb. Pro optimální výkon hello následující nastavení by také být zapnut v manifestu clusteru hello: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a>Zadání zásad správného řízení prostředků 

Omezení zásad správného řízení prostředků jsou určené v manifestu aplikace hello (ServiceManifestImport část), jak ukazuje následující příklad hello:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <Parameters>
  </Parameters>
  <!--
  ServicePackageA has hello number of CPU cores defined, but doesn't have hello MemoryInMB defined.
  In this case, Service Fabric will sum hello limits on code packages and uses hello sum as 
  hello overall ServicePackage limit.
  -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1000" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="1000" />
    </Policies>
  </ServiceManifestImport>
```
  
V tomto příkladu získá balíček služby ServicePackageA hello uzly, kde je umístěn jednoho jádra. Tento balíček služby obsahuje dva balíčky kódu (CodeA1 a CodeA2) a obě zadejte hello `CpuShares` parametr. Hello podíl CpuShares 512:256 vydělí hello základní napříč hello dva balíčky pro kód. Proto v tomto příkladu CodeA1 získá 2 / 3 jádro, CodeA2 získá jednu třetinu jádro (a konfigurace soft záruku rezervace hello stejné). V případě, že když CpuShares nejsou zadané pro balíčky kódu, Service Fabric vydělí hello jader rovnoměrně mezi nimi.

Omezení paměti jsou absolutní, takže oba balíčky kódu jsou omezené too1024 MB paměti (a konfigurace soft záruku rezervace z stejné hello). Kód balíčky (kontejnerů a procesy) nejsou možné tooallocate, které větší spotřebu paměti než tento limit a pokus toodo tak vede k výjimce z důvodu nedostatku paměti. Pro toowork vynucení omezení prostředků musí mít všechny balíčky kódu v rámci balíčku služby zadaná paměťová omezení.


## <a name="next-steps"></a>Další kroky
* toolearn více o clusteru Resource Manager, přečtěte si to [článku](service-fabric-cluster-resource-manager-introduction.md).
* toolearn Další informace o modelu aplikace, balíčky služeb, kód balíčky a jak repliky mapování toothem načíst tato [článku](service-fabric-application-model.md).
