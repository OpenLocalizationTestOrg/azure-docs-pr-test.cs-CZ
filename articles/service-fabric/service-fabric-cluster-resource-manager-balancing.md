---
title: aaaBalance cluster Azure Service Fabric | Microsoft Docs
description: "Úvod toobalancing clusteru pomocí hello správce prostředků clusteru Service Fabric."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 030b1465-6616-4c0b-8bc7-24ed47d054c0
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 5f7ad2f5cf4cfb3751a860f5293b03d2d5266d99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="balancing-your-service-fabric-cluster"></a>Vyrovnávání váš cluster service fabric
Hello správce prostředků clusteru Service Fabric podporuje dynamické zatížení změny, reagující tooadditions nebo odstraňování uzly nebo služeb. Také automaticky opraví narušení omezení a proaktivně znovu vytvoří rovnováhu hello clusteru. Ale jsou jak často tyto akce provést a co je aktivuje?

Existují tři různé kategorie pracovní této hello, které provádí správce prostředků clusteru. Jsou:

1. Umístění – tato fáze se zabývá umístění žádné stavové repliky nebo bezstavové instancí, které chybí. Umístění zahrnuje nové služby a zpracování stavových repliky nebo bezstavové instancí, které selhaly. Tady jsou zpracovávány odstranění a vyřazení replik nebo instancí.
2. Omezení kontroluje – tato fáze kontroluje a opravuje porušení omezení různých umístění hello (pravidla) v rámci systému hello. Příklady pravidel jsou třeba zajistit, že uzly nejsou přes kapacity a že jsou splněny omezení umístění služby.
3. Vyrovnávání – tato fáze ověří toosee Pokud vyrovnává je nutné hello nakonfigurovaný podle potřeby úroveň vyrovnání pro jiné metriky. Pokud ano pokusí toofind uspořádání v hello clusteru tedy více vyrovnáváním.

## <a name="configuring-cluster-resource-manager-timers"></a>Konfigurace správce prostředků clusteru časovače
Hello první sadu ovládacích prvků kolem vyrovnávání jsou sady časovače. Tyto časovače řídí, jak často hello správce prostředků clusteru prozkoumá hello clusteru a provede nápravné akce.

Každý z těchto různých typů hello opravy, které má správce prostředků clusteru řídí jiné časovače, která řídí její interval. Při každém časovače hello je naplánován. Ve výchozím nastavení hello Resource Manager:

* kontroluje stav a použije aktualizace (jako záznam, který je uzlem dolů) každých 1/10 sekund
* Nastaví příznak zkontrolujte umístění hello 
* Nastaví příznak kontrola omezení hello za sekundu
* Nastaví hello vyrovnávání příznak každých pět sekund.

Níže jsou příklady konfigurace hello řídících tyto časovače:

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PLBRefreshGap",
          "value": "0.10"
      },
      {
          "name": "MinPlacementInterval",
          "value": "1.0"
      },
      {
          "name": "MinConstraintCheckInterval",
          "value": "1.0"
      },
      {
          "name": "MinLoadBalancingInterval",
          "value": "5.0"
      }
    ]
  }
]
```

Dnes hello clusteru Resource Manager provádí pouze jednu z těchto akcí najednou, postupně. Z tohoto důvodu jsme naleznete toothese časovače jako "minimální intervaly" a hello akce, které získat provedeny, když přejít časovačů hello jako "nastavení příznaky". Například hello, který má na starosti clusteru Resource Manager čeká na vyřízení požadavků služby toocreate před hello cluster vyrovnávání. Jak je vidět ve hello výchozí časové intervaly zadaný hello správce prostředků clusteru kontroluje pro všechno, co ji toodo potřebám často. Obvykle znamená, že je malý hello sadu změny provedené v průběhu každého kroku. Provedení malých změn často umožňuje hello správce prostředků clusteru toobe přizpůsobivý při problémech v clusteru hello. Hello výchozí časovače poskytují některé dávkování od řadu hello stejné typy událostí, které jsou obvykle toooccur současně. 

Například, když nepovede uzly mohou provádět tak celý domén selhání najednou. Všechny tyto chyby jsou zaznamenána během hello další stav aktualizace po hello *PLBRefreshGap*. Hello opravy jsou určeny v průběhu hello následující umístění, kontrola omezení a vyrovnávání spustí. Správce prostředků clusteru není ve výchozím nastavení hello kontrolu prostřednictvím čas změny v clusteru hello a zkusit tooaddress všechny změny najednou. Díky tomu by vést toobursts změn.

Hello správce prostředků clusteru také musí některé další informace o toodetermine Pokud hello clusteru imbalanced. Pro tento máme dva další požadované konfigurace: *BalancingThresholds* a *ActivityThresholds*.

## <a name="balancing-thresholds"></a>Vyrovnávání prahové hodnoty
Prahovou hodnotu vyrovnávání je hello hlavní řízení pro spuštění vyrovnává. Hello vyrovnávání prahová hodnota pro metriku je _poměr_. Pokud hello zatížení pro metriku na hello nejvíce načíst uzlu dělený hello množství zatížení na hello alespoň načíst uzlu překročí tuto metriku *BalancingThreshold*, pak je imbalanced hello clusteru. V důsledku vyrovnávání je spouštěná hello další čas hello kontroly správce prostředků clusteru. Hello *MinLoadBalancingInterval* časovače definuje, jak často hello clusteru Resource Manager by měla zkontrolujte, zda vyrovnává je nezbytné. Kontrola neznamená, že se nic nestane. 

Vyrovnávání prahové hodnoty jsou definovány na základě-metrika jako součást definice hello clusteru. Další informace o metrikách, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:

```json
"fabricSettings": [
  {
    "name": "MetricBalancingThresholds",
    "parameters": [
      {
          "name": "MetricName1",
          "value": "2"
      },
      {
          "name": "MetricName2",
          "value": "3.5"
      }
    ]
  }
]
```

<center>
![Příklad vyrovnávání prahová hodnota][Image1]
</center>

V tomto příkladu je každá služba využívání jednu jednotku některé metriky. V příkladu nejvyšší hello hello maximální zatížení na uzlu je pět a minimální hello je dva. Řekněme, že tento hello vyrovnávání prahová hodnota pro tato metrika je tři. Vzhledem k tomu, že poměr hello v clusteru hello je 5/2 = 2.5 a který je menší než hello zadaných vyrovnávání prahovou hodnotu tři, hello clusteru jsou rovnoměrně. Žádné vyrovnávání se aktivuje, když kontroluje hello správce prostředků clusteru.

V příkladu dolní hello hello maximální zatížení na uzlu je 10, zatímco hello je minimálně dva týdny, výsledkem je poměr pět. Pět je větší než hello určené vyrovnávání prahovou hodnotu tři pro tuto metriku. V důsledku toho budou rebalancing spustit naplánované další čas hello vyrovnávání aktivuje časovače. Některé zatížení v situaci, jako je to je obvykle distribuované tooNode3. Protože hello správce prostředků clusteru Service Fabric nepoužívá typu greedy přístup, některé zatížení mohou být také distribuované tooNode2. 

<center>
![Akce příklad vyrovnávání prahová hodnota][Image2]
</center>

> [!NOTE]
> "Vyrovnávání" zpracovává dva různé strategie pro správu zatížení v clusteru. Hello výchozí strategie, která hello používá správce prostředků clusteru je toodistribute zatížení mezi hello uzly v clusteru hello. Hello další strategie je [defragmentace](service-fabric-cluster-resource-manager-defragmentation-metrics.md). Defragmentace provádí během hello stejné vyrovnávání spustit. Hello vyrovnávání a defragmentace strategie lze použít pro jiné metriky v rámci hello stejného clusteru. Služba může mít vyrovnávání a defragmentace metriky. Defragmentace metrik hello poměr hello načte v aktivační události clusteru hello nové vyvážení v případě, že je _pod_ hello vyrovnávání prahovou hodnotu. 
>

Získání níže hello vyrovnávání prahová hodnota není explicitní cíle. Vyrovnávání prahové hodnoty jsou právě *aktivační událost*. Při vyrovnávání spustí, určuje hello správce prostředků clusteru jaká vylepšení mohl zajistit, pokud existuje. Právě, protože vyrovnávání vyhledávání je spuštěna neznamená, že nic přesune. Někdy hello clusteru je toocorrect imbalanced ale příliš omezené. Můžete taky vyžadovat vylepšení hello pohybů, které jsou příliš [nákladná](service-fabric-cluster-resource-manager-movement-cost.md)).

## <a name="activity-thresholds"></a>Aktivita prahové hodnoty
V některých případech i když jsou relativně imbalanced uzly, hello *celkový* množství zatížení v clusteru hello je nízká. Nedostatečná Hello zatížení může být přechodný vyhrazené IP adresy, nebo protože hello clusteru je nový a právě získávání připravili. V obou případech nemusí chcete toospend čas vyrovnávání hello clusteru, protože je moc toobe získávají. Pokud hello cluster podstoupila vyrovnávání by tráví síť a výpočetní prostředky toomove věcí kolem bez provedení všechny velké *absolutní* rozdíl. Přesune tooavoid nepotřebné, je další ovládací prvek označuje jako aktivita prahové hodnoty. Prahové hodnoty aktivity vám umožní toospecify některé absolutní dolní mez aktivity. Pokud žádný uzel nad touto prahovou hodnotou, vyrovnávání není aktivované i v případě hello vyrovnávání dosáhla prahová hodnota.

Řekněme, že jsme zachovat naše vyrovnávání prahovou hodnotu tři pro tuto metriku. Také se stát, že máme 1536 prahovou hodnotu aktivity. V prvním případě hello při imbalanced za hello hello clusteru vyrovnávání prahová hodnota došlo je žádný uzel splňuje prahové hodnoty aktivity, nic se stane. V příkladu dolní hello Uzel1 je nad hello aktivity prahovou hodnotu. Vzhledem k tomu, že oba hello vyrovnávání prahovou hodnotu a hello aktivity prahovou hodnotu pro metriku hello překročen, vyrovnávání naplánován. Jako příklad Podíváme se na následující diagram hello: 

<center>
![Příklad aktivity prahová hodnota][Image3]
</center>

Stejně jako vyrovnávání prahové hodnoty aktivity prahové hodnoty jsou definované za metriku prostřednictvím definice hello clusteru:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:

```json
"fabricSettings": [
  {
    "name": "MetricActivityThresholds",
    "parameters": [
      {
          "name": "Memory",
          "value": "1536"
      }
    ]
  }
]
```

Prahové hodnoty vyrovnávání a aktivity jsou oba konkrétní metrika vázanou tooa - vyrovnávání se spustí pouze v případě, že oba hello vyrovnávání prahovou hodnotu a aktivity je prahová hodnota pro hello stejnou metriku.

## <a name="balancing-services-together"></a>Společně vyrovnávání služby
Zda je hello cluster imbalanced nebo ne je platné pro celý cluster rozhodnutí. Způsob hello jsme přejděte o její opravu je však přesunutí repliky jednotlivé služby a instance kolem. To dává smysl, ne? Pokud paměti je skládaný na jednom uzlu, více replik nebo instancí může být jednou z příčin tooit. Uchycení nevyváženosti hello může vyžadovat přesunutí hello stavová repliky nebo bezstavové instancí, které použít imbalanced metrika hello.

Občas, když služba, která nebyla samotné imbalanced získá přesunout (Nezapomeňte hello diskuzi o místní a globální provede dříve). Proč by služby získat přesunuta, když všechno, co byly vyrovnáváním metriky služby? Podíváme se na příklad:

- Řekněme, že existují čtyři služby, Service1, Service2, Service3 a Service4. 
- Service1 sestavy metrik Metric1 a Metric2. 
- Service2 sestavy metrik Metric2 a Metric3. 
- Service3 sestavy metrik Metric3 a Metric4.
- Service4 hlásí metriky Metric99. 

Surely se zobrazí, kde vytvoříme zde: existuje řetězec! Nemáme skutečně čtyři nezávislé služby, máme tři služby, které se vztahují a ten, který je vypnutý o sobě.

<center>
![Společně vyrovnávání služby][Image4]
</center>

Z důvodu tento řetězec je možné, že byl zjištěn rozdíl v metriky 1 – 4 může způsobit repliky nebo instance, které patří tooservices 1 – 3 toomove kolem. Taky víme, že byl zjištěn rozdíl v metriky 1, 2 nebo 3 nelze způsobit pohybů v Service4. Od přesunutí hello repliky by být žádný bod nebo absolutně nic nestane. instance, které patří tooService4 kolem můžete vyrovnávat hello tooimpact metrik 1 – 3.

Hello správce prostředků clusteru automaticky hodnoty na jaké služby jsou související. Přidání, odebrání nebo změna hello metriky pro služby může ovlivnit jejich vztahů. Například mezi dvěma spustí vyrovnávání Service2 pravděpodobně aktualizované tooremove Metric2. Tím se přeruší hello řetězu mezi Service1 a Service2. Teď místo dvě skupiny související služby, existují tři:

<center>
![Společně vyrovnávání služby][Image5]
</center>

## <a name="next-steps"></a>Další kroky
* Metriky se, jak hello správce prostředků clusteru Service Fabric spravuje využívání a kapacity v clusteru hello. Další informace o metrikách toolearn a jak tooconfigure, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)
* Náklady na přesunutí je jeden ze způsobů signalizace toohello správce prostředků clusteru, že jsou určité služby toomove dražší než jiné. Další informace o náklady na přesunutí najdete příliš[v tomto článku](service-fabric-cluster-resource-manager-movement-cost.md)
* Hello správce prostředků clusteru má několik omezení, můžete nakonfigurovat tooslow dolů změn v clusteru hello. Nejsou běžně potřebné, ale chcete-li se dozvíte je [sem](service-fabric-cluster-resource-manager-advanced-throttling.md)

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
