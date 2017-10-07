---
title: aaaDefragmentation metriky v Azure Service Fabric | Microsoft Docs
description: "Přehled použití defragmentace nebo balení jako strategie pro metriky v Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: e5ebfae5-c8f7-4d6c-9173-3e22a9730552
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: d09045a6cf196d2771f1a0794637f4579d3eb96b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentace metriky a zatížení v Service Fabric
strategie výchozí Hello portálu Service Fabric clusteru Resource Manager pro správu metriky zatížení v clusteru hello je toodistribute hello zatížení. Zajištění, že uzly jsou rovnoměrně využité zabraňuje úrovněmi horkého a studeného body, které vést tooboth kolizí a nevyužité prostředky. Distribuce zatížení v clusteru hello je také hello nejbezpečnější z hlediska zbývajících selhání, protože zajišťuje selhání nebude vyjměte vysoké procento daného zatížení. 

Hello správce prostředků clusteru Service Fabric podporuje různé strategie pro správu zatížení, která je defragmentace. Defragmentace znamená, že namísto pokusu o využití hello toodistribute metriky napříč hello clusteru, je jsou konsolidovány. Konsolidace je právě inverzi hello výchozí vyrovnávání strategie – místo minimalizovat hello průměrná směrodatnou odchylku metriky zatížení, hello správce prostředků clusteru pokusí tooincrease ho.

## <a name="when-toouse-defragmentation"></a>Když toouse Defragmentace
Distribuce zatížení v clusteru hello spotřebuje některé hello prostředků na každém uzlu. Některé úlohy vytvoření služby, které jsou velmi velké a využívat většinu nebo všechny uzlu. V těchto případech je možné, že pokud jsou velké získávání vytvoření úlohy, které není k dispozici dostatek místo na všechny toorun uzlu je. Rozsáhlejší úlohy nejsou potíže s Service Fabric; v těchto případech hello clusteru Resource Manager určí, že potřebuje tooreorganize hello clusteru toomake místo pro tento velké zatížení. Ale v hello zatím této úlohy má toowait toobe naplánované v clusteru hello.

Pokud máte mnoho služeb a stavu toomove kolem, může to trvat dlouho toobe velké zatížení hello umístit do clusteru hello. Toto je pravděpodobnější, pokud dalších zatížení v clusteru hello také velké a proto trvat déle tooreorganize. tým Service Fabric Hello měří času vytvoření v simulace tohoto scénáře. Zjistili jsme, že trvalo déle, mnohem vytváření velké služeb, při využití clusteru získali výše 30 až 50 %. toohandle tento scénář zavedli jsme defragmentace jako strategie vyrovnávání. Zjistili jsme, že pro větší zatížení, zejména ty, kde je důležité, defragmentace skutečně pomohly tyto nové úlohy vytváření získat naplánován v clusteru hello.

Defragmentace metriky toohave hello správce prostředků clusteru tooproactively zkuste toocondense hello zatížení hello služeb můžete nakonfigurovat do menšího počtu uzlů. To pomáhá zajistit, že je téměř vždy prostor pro velké služeb bez reorganizace hello clusteru. Nemá tooreorganize hello clusteru umožňuje vytváření rozsáhlých úloh s rychle.

Většina lidí nepotřebují defragmentace. Služby jsou obvykle být malé, takže není pevný toofind prostor pro je v clusteru hello. Když reorganizaci je možné, přejde rychle znovu protože většina služeb jsou malé a přesunout rychle a paralelně. Ale pokud máte velké služby a potřeby rychle vytvořit můžete hello defragmentace strategie je pro vás. Probereme hello kompromisy defragmentace další použití. 

## <a name="defragmentation-tradeoffs"></a>Defragmentace kompromisy
Defragmentace může zvýšit impactfulness chyb, protože běží další služby na uzlech, které nesplní. Defragmentace můžete taky zvýšit náklady, protože prostředků v clusteru hello musí uchovávat v rezervy, čekání na vytvoření hello z rozsáhlejší úlohy.

Hello následující obrázek poskytuje vizuální reprezentace dva clustery, ten, který je defragmentovat a ten, který není. 

<center>
![Porovnání vyrovnáváním a defragmentovat clustery][Image1]
</center>

V případě hello vyrovnáváním vezměte v úvahu počet hello pohybů, které by bylo nutné tooplace mezi největší objekty služby hello. V clusteru defragmentované hello může být umístěny na uzlů čtyř nebo pěti hello velkých úloh bez nutnosti toowait pro jiné služby toomove.

## <a name="defragmentation-pros-and-cons"></a>Defragmentace výhody a nevýhody
Jaké jsou proto tyto koncepční kompromisy? Tady je rychlý tabulku věcí toothink o:

| Defragmentace specialisté | Cons Defragmentace |
| --- | --- |
| Umožňuje rychlejší vytváření velké služeb |Koncentráty načíst do menšího počtu uzlů, zvýšení kolizí |
| Umožňuje snížit přesun dat během vytváření |Selhání může mít vliv na další služby a způsobit další změny |
| Umožňuje bohaté popis požadavků a recyklace místa |Složitější celkové konfiguraci správy prostředků |

Je možné kombinovat defragmentované a normální metriky v hello stejného clusteru. Hello správce prostředků clusteru pokusů tooconsolidate hello defragmentace metriky co nejvíce při rozšíří hello ostatní. výsledky Hello kombinování defragmentace a vyrovnávání strategie závisí na několika různými faktory, včetně:
  - počet Hello vyrovnávání metriky oproti hello počet defragmentace metriky
  - Tom, zda všechny služby používá oba typy metriky 
  - metriky váhu Hello
  - načte aktuální metrika
  
Experimentování je požadovaná toodetermine hello přesnou konfiguraci nezbytné. Před povolením defragmentace metriky v produkčním prostředí doporučujeme důkladné měření vašich úloh. To je užitečné především při kombinování defragmentace a vyrovnáváním metriky v rámci hello stejnou službu. 

## <a name="configuring-defragmentation-metrics"></a>Konfigurace defragmentace metriky
Konfigurace metriky defragmentace je globální rozhodnutí v clusteru hello a jednotlivé metriky můžete vybrat pro defragmentaci. Hello následující konfigurace fragmenty kódu ukazují, jak tooconfigure metriky pro defragmentace. V tomto případě "Metric1" je nakonfigurovaná jako defragmentace metriku, zatímco "Metric2" bude dále toobe vyrovnáváním normálně. 

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:

```json
"fabricSettings": [
  {
    "name": "DefragmentationMetrics",
    "parameters": [
      {
          "name": "Metric1",
          "value": "true"
      },
      {
          "name": "Metric2",
          "value": "false"
      }
    ]
  }
]
```


## <a name="next-steps"></a>Další kroky
- Hello správce prostředků clusteru má man možnosti popisující hello clusteru. toofind Další informace o jejich, projděte si tento článek na [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)
- Metriky se, jak hello správce prostředků clusteru Service Fabric spravuje využívání a kapacity v clusteru hello. Další informace o metrikách toolearn a jak tooconfigure, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
