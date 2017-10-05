---
title: "Azure Service Fabric prostředků řízení pro kontejnery a služby | Microsoft Docs"
description: "Azure Service Fabric můžete zadat omezení prostředků pro služby spuštěné uvnitř nebo mimo kontejnery."
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
ms.openlocfilehash: 88d44953ad83f9e7401fd087a39842e4a3790124
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="resource-governance"></a>Zásady správného řízení prostředků 

Při spuštění více služeb na stejném uzlu nebo clusteru, je možné, že jedna služba může spotřebují více prostředků starving dalších služeb. Tento problém se označuje jako aktivní sousedním problém. Service Fabric umožňuje vývojáři zadejte rezervace a omezení pro službu zajistit prostředků a také omezit jeho využití prostředků. 

## <a name="resource-governance-metrics"></a>Metrika prostředků zásad správného řízení 

Zásady správného řízení prostředků je podporována v Service Fabric na [balíček služby](service-fabric-application-model.md). Prostředky, které jsou přiřazeny k balíčku služby lze dále rozdělit mezi balíčky kódu. Zadané limity prostředku přístup také znamenat rezervace prostředků. Service Fabric podporuje určení procesoru a paměti na balíček služby, pomocí dvě předdefinované [metriky](service-fabric-cluster-resource-manager-metrics.md):

* Procesor (název metriky `ServiceFabric:/_CpuCores`): jádro je logická jádra, která je k dispozici na hostitelském počítači, a všechny jader pro všechny uzly jsou vyváženy stejné.
* Paměť (název metriky `ServiceFabric:/_MemoryInMB`): paměti je vyjádřeno v megabajtech a se mapuje na fyzické paměti, která je k dispozici na počítači.

Záruky vyhrazené logicky pouze jsou k dispozici – modul runtime odmítne otevření nové balíčky služeb, které jsou překročil dostupné prostředky. Ale pokud jiný spustitelný soubor nebo kontejneru je umístěn na uzlu, který může být v rozporu původní záruky vyhrazené.

Pro tyto dvě metriky [správce prostředků clusteru](service-fabric-cluster-resource-manager-cluster-description.md) sleduje celkové clusteru kapacitu, zatížení na každém uzlu v clusteru, a zbývající prostředků v clusteru. Tyto dvě metriky odpovídají na jiné uživatele nebo vlastní metriky a všechny existující funkce lze použít s nimi:
* Cluster může být [vyrovnáváním](service-fabric-cluster-resource-manager-balancing.md) podle tyto dvě metriky (výchozí nastavení).
* Cluster může být [defragmentovat](service-fabric-cluster-resource-manager-defragmentation-metrics.md) podle tyto dvě metriky.
* Když [popisující cluster](service-fabric-cluster-resource-manager-cluster-description.md), ve vyrovnávací paměti kapacity lze nastavit pro tyto dvě metriky.

[Dynamické načtení reporting](service-fabric-cluster-resource-manager-metrics.md) není podporována pro tyto metriky a načte pro tyto metriky, které jsou definovány v okamžiku vytvoření.

## <a name="cluster-set-up-for-enabling-resource-governance"></a>Cluster sady pro povolení zásad správného řízení prostředků

Kapacita musí být definovány ručně v každém typu uzlu v clusteru takto:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
Zásady správného řízení prostředků je povolen pouze na uživatele služby, nikoli na všechny systémové služby. Při zadávání kapacitu, některé jader a paměti musí být vlevo volné pro systémových služeb. Pro optimální výkon toto nastavení by také být zapnut v manifestu clusteru: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a>Zadání zásad správného řízení prostředků 

Omezení zásad správného řízení prostředků jsou určené v manifest aplikace (ServiceManifestImport oddílu), jak je znázorněno v následujícím příkladu:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <Parameters>
  </Parameters>
  <!--
  ServicePackageA has the number of CPU cores defined, but doesn't have the MemoryInMB defined.
  In this case, Service Fabric will sum the limits on code packages and uses the sum as 
  the overall ServicePackage limit.
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
  
V tomto příkladu balíček služby ServicePackageA získá jednoho jádra na uzlech, kde je umístěn. Tento balíček služby obsahuje dva balíčky kódu (CodeA1 a CodeA2) a obě zadejte `CpuShares` parametr. Poměr CpuShares 512:256 vydělí základní mezi dvěma kód balíčky. Proto v tomto příkladu CodeA1 získá 2 / 3 jádro a CodeA2 získá jednu třetinu jádro (a rezervace konfigurace soft záruku stejného). V případě, že pokud nejsou zadané CpuShares pro balíčky kódu, Service Fabric vydělí jader rovnoměrně mezi nimi.

Omezení paměti jsou absolutní, takže oba balíčky kódu jsou omezeny na 1024 MB paměti (a rezervace konfigurace soft záruku stejného). Balíčky kódu (kontejnery a procesy) nejsou schopné přidělit víc paměti, než je toto omezení, a případný pokus o takové přidělení má za následek výjimku z důvodu nedostatku paměti. Aby vynucení omezení prostředků fungovala, musí být omezení paměti zadaná pro všechny balíčky kódu v rámci balíčku služby.


## <a name="next-steps"></a>Další kroky
* Další informace o službě Správce prostředků clusteru, přečtěte si to [článku](service-fabric-cluster-resource-manager-introduction.md).
* Další informace o modelu aplikace, balíčky služeb, balíčky kódu a jak mapování repliky na ně načíst tato [článku](service-fabric-application-model.md).
