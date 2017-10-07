---
title: "aaaThrottling ve Správci prostředků clusteru Service Fabric hello | Microsoft Docs"
description: "Další omezení hello tooconfigure poskytované hello správce prostředků clusteru Service Fabric."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4a44678b-a5aa-4d30-958f-dc4332ebfb63
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: f418536911d3e3814e78a4d9f057dfb867ca7c63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="throttling-hello-service-fabric-cluster-resource-manager"></a>Omezení hello správce prostředků clusteru Service Fabric
I když jste nakonfigurovali hello správce prostředků clusteru správně, může získat dojde k narušení hello clusteru. Například může být současně uzel a selhání domény selhání - co by mohlo dojít, pokud, ke kterým došlo během upgradu? Hello clusteru Resource Manager vždy se pokusí toofix všechno využívání prostředků clusteru hello pokusu o tooreorganize a opravte hello clusteru. Omezení zajišťuje backstop tak, aby hello cluster může používat prostředky toostabilize – uzly hello vraťte, retušovat hello síťové oddíly, nasadí opravené bits.

toohelp pomocí těchto nastavení neovlivní situacích hello správce prostředků clusteru Service Fabric zahrnuje několik omezení. Tato omezení jsou všechny bourací kladiva poměrně velké. Obecně se nesmí změnit bez pečlivé plánování a testování.

Pokud změníte hello clusteru správce prostředků pro omezení, musí je ladit tooyour očekávané skutečné zátěže. Můžete určit, že potřebujete toohave některé omezí generovaný na místě, i v případě, znamená to, že hello clusteru trvá déle, toostabilize v některých situacích. Testování je požadovaná toodetermine hello správné hodnoty pro omezení. Omezení potřebovat toobe dostatečně vysoký tooallow hello clusteru toorespond toochanges v přiměřené době a dolní dostatek tooactually zabránit příliš mnoho spotřeby prostředků. 

Většinu času hello jsme viděli zákazníkům použít omezení, byla úspěšně, protože již byly v prostředí omezené prostředků. Některé příklady by byla omezena šířka pásma sítě pro jednotlivé uzly nebo disky, které nejsou možné toobuild mnoho stavových replik v paralelní z důvodu omezení toothroughput. Bez omezení může operace zahlcovat těchto prostředků, způsobuje operations toofail nebo pomalé. V těchto situacích zákazníků použít omezení a věděl, že se měla rozšíření hello množství času, které by byly třeba hello clusteru tooreach stabilního stavu. Zákazníci také za to, že se může stát, že běží na nižší celkovou spolehlivost, zatímco se byly omezeny.


## <a name="configuring-hello-throttles"></a>Konfigurace omezení hello

Service Fabric má dva mechanismy pro omezení počtu hello pohybů repliky. Hello výchozího mechanismu, který existoval Service Fabric 5.7 představuje omezení jako absolutní počet přesune povoleny. Není to funguje pro clustery všech velikostí. Konkrétně pro velkých clusterech hello výchozí hodnota může být příliš malá, výrazně zpomalení vyrovnávání i v případě, že je to nutné, přitom má neplatí v menší clustery. Tento mechanismus předchozí byla nahrazena na základě procenta omezení, která škáluje líp s dynamické clustery v číslo, které hello služeb a uzly pravidelně měnit.

omezení Hello jsou založené na procento hello počet replik v clusterech hello. Omezení Percetage na základě povolení 5násobku pravidla hello: "nepřesouvejte více než 10 % repliky v intervalu 10 minut", například.

nastavení konfigurace Hello na základě procenta omezení jsou:

  - GlobalMovementThrottleThresholdPercentage – maximální počet pohybů v clusteru povolené kdykoli, vyjádřené jako procento celkového počtu replik v clusteru hello. 0 znamená bez omezení. Hello výchozí hodnota je 0. Pokud toto nastavení a GlobalMovementThrottleThreshold jsou nastaveny, pak hello více konzervativní limit se používá.
  - GlobalMovementThrottleThresholdPercentageForPlacement – maximální počet povolené fázi hello umístění, vyjádřené jako procento celkového počtu replik v clusteru hello pohybů typu. 0 znamená bez omezení. Hello výchozí hodnota je 0. Pokud toto nastavení a GlobalMovementThrottleThresholdForPlacement jsou nastaveny, pak hello více konzervativní limit se používá.
  - GlobalMovementThrottleThresholdPercentageForBalancing – maximální počet povolených během hello vyrovnávání fáze, vyjádřené jako procento celkového počtu replik v clusteru hello pohybů typu. 0 znamená bez omezení. Hello výchozí hodnota je 0. Pokud toto nastavení a GlobalMovementThrottleThresholdForBalancing jsou nastaveny, pak hello více konzervativní limit se používá.

Při zadávání hello omezení procento, zadáte jako hodnotu 0,05 5 %. Hello interval, ve kterém se řídí těchto omezení je hello GlobalMovementThrottleCountingInterval, které se určuje v sekundách.


``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThresholdPercentage" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForPlacement" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForBalancing" Value="0" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
</Section>
```

pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "GlobalMovementThrottleThresholdPercentage",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForPlacement",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForBalancing",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleCountingInterval",
          "value": "600"
      }
    ]
  }
]
```

### <a name="default-count-based-throttles"></a>Výchozí počet na základě omezení
Tyto informace jsou poskytovány v případě, že máte starší clustery nebo zachovat přitom tyto konfigurace clusterů, které od té doby byly upgradovány. Obecně se doporučuje, ty jsou nahrazeny hello na základě procenta omezení výše. Vzhledem k tomu, že ve výchozím nastavení vypnutá omezování na základě procenta, zůstanou tato omezení hello výchozí omezení pro cluster s podporou, dokud jsou zakázána a nahradí omezení na základě procenta hello. 

  - GlobalMovementThrottleThreshold – toto nastavení určuje celkový počet hello pohybů v clusteru hello delší dobu. Hello množství času se zadávají v sekundách jako hello GlobalMovementThrottleCountingInterval. hello GlobalMovementThrottleThreshold Hello výchozí hodnota je 1000 a výchozí hodnota hello hello GlobalMovementThrottleCountingInterval je 600.
  - MovementPerPartitionThrottleThreshold – toto nastavení určuje celkový počet hello pohybů pro všechny služby oddíl delší dobu. Hello množství času se zadávají v sekundách jako hello MovementPerPartitionThrottleCountingInterval. hello MovementPerPartitionThrottleThreshold Hello výchozí hodnota je 50 a hello MovementPerPartitionThrottleCountingInterval hello výchozí hodnota je 600.

Hello konfigurace těchto omezení způsobem hello stejný vzor jako procento na základě omezení hello.

## <a name="next-steps"></a>Další kroky
- toofind si o tom, jak hello správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru hello, podívejte se na článek hello na [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)
- Hello správce prostředků clusteru má mnoho možností pro popisující hello clusteru. toofind Další informace o jejich, projděte si tento článek na [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)
