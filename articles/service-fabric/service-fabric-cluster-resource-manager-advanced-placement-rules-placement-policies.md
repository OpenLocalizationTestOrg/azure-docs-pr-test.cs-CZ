---
title: "aaaService správce prostředků clusteru prostředků infrastruktury – zásady umístění | Microsoft Docs"
description: "Přehled další umístění zásad a pravidel pro služby Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 5c2d19c6-dd40-4c4b-abd3-5c5ec0abed38
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: bb58642520085ab3000f3929cf9aea7a8f6e3070
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="placement-policies-for-service-fabric-services"></a>Umístění zásady pro služby service fabric
Zásady umístění se další pravidla, která může být umístění služby použít toogovern v některých scénářích konkrétní, méně běžné. Mezi příklady tyto scénáře patří:

- Cluster Service Fabric zahrnuje geografické vzdálenosti, například několik datových center v místě nebo mezi oblastmi Azure
- Vaše prostředí zahrnuje více oblastí řízení geopolitické nebo právních nebo některých jiných případ, kdy máte zásady hranice je nutné tooenforce
- Komunikace výkon nebo latenci rozhodnutí z důvodu toolarge vzdálenosti nebo použití pomalejší nebo méně spolehlivé síťová propojení
- Je třeba tookeep určité úlohy společně umístěná jako usilovně se s ostatními úlohami nebo v blízkosti toocustomers

Většinu těchto požadavků zarovnané s hello fyzické rozložení hello clusteru, reprezentována jako hello domén selhání clusteru hello. 

Hello pokročilé zásady umístění, které pomáhají adresu, kterou jsou tyto scénáře:

1. Neplatný domén
2. Požadované domén
3. Upřednostňovaných domén
4. Zakazuje se okolních repliky

Většina hello následující ovládací prvky se daly konfigurovat prostřednictvím vlastnosti uzlu a omezení umístění, ale některé jsou složitější. věcí toomake jednodušší, hello správce prostředků clusteru Service Fabric nabízí tyto zásady další umístění. Konfigurace zásad umístění na základě služby za názvem instance. Se dá se taky aktualizovat dynamicky.

## <a name="specifying-invalid-domains"></a>Neplatné zadání domén
Hello **InvalidDomain** umístění zásad vám umožní toospecify, že je neplatný pro konkrétní službu konkrétní domény selhání. Tato zásada zajišťuje, že konkrétní službu nikdy běží v určité oblasti, například důvodů geopolitické nebo podnikové zásady. Přes samostatné zásady je možné zadat více domén neplatný.

<center>
![Příklad domény je neplatný][Image1]
</center>

Kód:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

Prostředí PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Určení požadované domén
Hello požadované zásady umístění domény vyžaduje, aby služba hello pouze v zadané doméně hello. Lze zadat více domén požadované přes samostatné zásady.

<center>
![Příklad požadovanou doménu][Image2]
</center>

Kód:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

Prostředí PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-hello-primary-replicas-of-a-stateful-service"></a>Určení upřednostňované domény pro primární repliky hello stavové služby
Hello preferované primární doménu určuje tooplace domény selhání hello hello primární v. Hello primární skončilo v této doméně při všechno, co je v pořádku. Pokud hello domény nebo hello primární repliky selže nebo vypne, hello primární přesune toosome jiného umístění, nejlépe ve hello stejné domény. Pokud toto nové umístění není v doméně upřednostňované hello, hello správce prostředků clusteru přesune zpátky upřednostňované domény toohello co nejdříve. Přirozeně toto nastavení má smysl jenom pro stavové služby. Tato zásada je velmi užitečné v clusterech, které jsou rozloženy oblastí Azure nebo několik datových center ale máte služby, které dávají přednost umístění v určitých umístění. Základní udržet barvy zavřete tootheir uživatele nebo jiné služby, které pomáhá poskytovat nižší latenci, hlavně pro čtení, které jsou zpracovávány základní barvy ve výchozím nastavení.

<center>
![Upřednostňovaný primární domény a převzetí služeb při selhání][Image3]
</center>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

Prostředí PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a>Vyžadování distribučních repliky a zakazuje okolních
Repliky jsou _normálně_ distribuován napříč doménami selhání a upgradu, když je v pořádku hello clusteru. Existují však případy, kdy může se stát víc než jednu repliku pro daný oddíl dočasně sbalené do jedné domény. Předpokládejme například, že tento cluster hello má devět uzly v tři domén selhání, fd: / 0, fd: / 1 a fd: / 2. Dále předpokládejme, že vaše služba má tři repliky. Řekněme, že hello uzly, které byly používány pro tyto repliky v fd: / 1 a fd: / 2 byl vypnut. Za normálních okolností hello správce prostředků clusteru přejete jiné uzly v těchto stejné domény selhání. V takovém případě Řekněme z důvodu problémů toocapacity žádné jiné uzly v těchto doménách hello byl platný. Pokud hello správce prostředků clusteru sestavení nahrazení pro tyto repliky, se bude mít toochoose uzly v fd: / 0. To však _,_ vytvoří situaci porušena hello omezení domény selhání. Balení repliky zvyšuje hello pravděpodobnost, že hello sady replik celou může přejděte a ztratí. 

> [!NOTE]
> Další informace o omezení a omezení priority obecně platí, podívejte se na [v tomto tématu](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).
>

Pokud někdy vidíte zprávu stavu, jako "`hello Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating hello Constraint: FaultDomain`", pak jste stisknutí tlačítka tato podmínka nebo něco podobného jako ho. Obvykle pouze jednu nebo dvě repliky jsou společně zabaleny dočasně. Tak dlouho, dokud je méně než kvorum repliky v dané doméně, jste bezpečné. Okolních je taková situace vzácná, ale může dojít a obvykle jsou tyto situace přechodný vzhledem k tomu, že uzly hello vraťte. Pokud uzly hello zůstat dolů a hello správce prostředků clusteru musí toobuild náhrady, obvykle existuje jiné uzly v doménách selhání ideální hello.

Některé úlohy by raději vždy s hello cílový počet replik, i když jsou zabaleny do menšího počtu domén. Tyto úlohy jsou ale proti selhání celkový počet souběžných trvalé domény a obvykle můžete obnovit místní stavu. Další úlohy by starší než riziko správnost nebo ke ztrátě dat. místo trvat hello výpadku. Většina produkční úlohy spustit s víc než tři repliky, víc než třemi domén selhání a mnoho platný uzlů na domény selhání. Z toho důvodu hello výchozí chování umožňuje okolních domény ve výchozím nastavení. Hello výchozí chování umožňuje normální vyrovnávání a převzetí služeb při selhání toohandle tyto výjimečných případech i v případě, že to znamená okolních dočasné domény.

Pokud chcete toodisable těchto okolních pro danou úlohu, můžete zadat hello `RequireDomainDistribution` zásady ve službě hello. Pokud je tato zásada nastavená, hello správce prostředků clusteru zajišťuje žádné dvě repliky ze stejného oddílu spustit v hello stejné poruch nebo upgradu domény hello.

Kód:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

Prostředí PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Teď, jej by být možné toouse tyto konfigurace pro služby v clusteru, který nebyl předané geograficky? Vám může, ale není skvělé důvod příliš. Hello konfigurace vyžaduje, neplatný a upřednostňované domény je nutno Pokud hello scénáře, které je vyžadují. Ho neznamená, že všechny smysl tootry tooforce toorun daného zatížení v jedné rack nebo tooprefer některé segment místním clusteru oproti jinému. Různé hardwarové konfigurace by měl být rozloženy domén selhání a zpracovávají pomocí normální umístění omezení a vlastnosti uzlu.

## <a name="next-steps"></a>Další kroky
- Další informace o konfiguraci služby [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
