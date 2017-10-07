---
title: "aaaApplication scénáře a návrhu | Microsoft Docs"
description: "Přehled kategorií cloudové aplikace v Service Fabric. Popisuje návrh aplikace, které používá stavová a Bezstavová services."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 3a8ca6ea-b8e9-4bc3-9e20-262437d2528e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/02/2017
ms.author: mfussell
ms.openlocfilehash: e36d5b2d21a6a1e3e85c9b21190072616e4921e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-scenarios"></a>Scénáře aplikací Service Fabric
Azure Service Fabric nabízí spolehlivé a flexibilní platformu, která vám umožní toowrite a spustit mnoho typů obchodních aplikací a služeb. Tyto aplikace a mikroslužeb může být bezstavové nebo stavová a jsou vyrovnávání prostředků mezi virtuálními toomaximize efektivitu. jedinečná architektura Hello Service Fabric umožňuje tooperform poblíž analýza dat v reálném čase, výpočtů v paměti, paralelní transakce a událostí zpracování ve svých aplikacích. Je možné snadno škálovat aplikace nahoru nebo dolů (skutečně příchozí nebo odchozí), v závislosti na měnící požadavky na prostředky.

Platforma Service Fabric Hello v Azure je ideální pro následující kategorie aplikace hello:

* **Vysoce dostupné služby**: služby Service Fabric poskytují rychlý převzetí služeb při selhání tak, že vytvoříte více replik sekundární službu. Pokud uzel, proces nebo jednotlivých služeb přestane fungovat kvůli toohardware nebo jiné chyby, jedním z sekundární repliky hello je propagovaných tooa primární replice minimální ztrátě služeb.
* **Škálovatelných služeb**: jednotlivých služeb můžou být dělené, což umožňuje škálovat na více systémů v clusteru hello toobe stavu. Kromě toho jednotlivé služby můžete vytvořit a odebrat chodu hello. Služby můžete rychle a snadno škálovat na více systémů z několik instancí na několik uzlů toothousands instancí na velký počet uzlů a pak škálovat v znovu, v závislosti na vašich prostředků. Můžete použít Service Fabric toobuild těchto služeb a spravovat jejich kompletní životní cykly.
* **Výpočty na nestatické data**: Service Fabric vám umožní toobuild dat, vstupu a výstupu a výpočetní náročné stavových aplikací. Service Fabric umožňuje společné umístění hello zpracování (výpočtů) a data v aplikacích. Za normálních okolností Pokud vaše aplikace vyžaduje toodata přístup, je latence sítě, které jsou spojené s vrstvou externích dat mezipaměti nebo úložiště. S stavové služby Service Fabric je eliminovat této latence, povolení další původce čte a zapisuje. Řekněme například, že máte aplikaci, která provádí téměř v reálném čase doporučení vybrané možnosti pro zákazníky s času jejich návratu požadavek menší než 100 milisekund. Hello latenci a výkonu charakteristik služeb Service Fabric (kde výpočtu hello výběru doporučení je umístěna společně s hello dat a pravidla) poskytuje přizpůsobivý zkušeností uživatele toohello ve srovnání s standardní implementace hello model s toofetch hello potřebná data ze vzdáleného úložiště.  
* **Interaktivní aplikace založené na relaci**: Service Fabric je užitečné, pokud vaše aplikace, jako je například online herní nebo zasílání rychlých zpráv a vyžadují, nízkou latencí čtení a zápisy. Service Fabric umožňuje toobuild můžete tyto interaktivní, stavová aplikace bez nutnosti toocreate samostatné úložiště nebo mezipaměti, podle potřeby pro bezstavové aplikace. (To zvyšuje latence a potenciálně zavádí konzistence problémy.).
* **Analýza dat a pracovních postupů**: hello Rychlé čtení a zápisy Service Fabric umožňují aplikacím, které musí spolehlivě zpracovávat události nebo datových proudů. Service Fabric také umožňuje aplikacím, které popisují kanálů pro zpracování, kde musí být výsledky, spolehlivé a předaný na toohello další zpracování fáze bez ztráty. Mezi ně patří transakcí a finanční systémy, kde jsou data konzistence a výpočtů záruky nezbytné.
* **Shromažďování dat, zpracování a IoT**: vzhledem k tomu, že Service Fabric zpracovává velkém měřítku a má nízkou latencí prostřednictvím jeho stavové služby, je ideální pro zpracování dat na miliony zařízení, kde jsou data hello hello zařízení a výpočetní hello společně umístěné.
Jsme viděli několik zákazníci, kteří mají vestavěné systémy IoT pomocí Service Fabric včetně [BMW](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/24/service-fabric-customer-profile-bmw-technology-corporation/), [Schneider elektrický](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/05/service-fabric-customer-profile-schneider-electric/) a [OK sítě systémy](https://blogs.msdn.microsoft.com/azureservicefabric/2016/06/20/service-fabric-customer-profile-mesh-systems/).

## <a name="application-design-case-studies"></a>Případové studie návrhu aplikace
Počet případové studie znázorňující, jak Service Fabric je použité toodesign aplikace jsou publikovány na hello [blog týmu Service Fabric](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/) a hello [mikroslužeb řešení lokality](https://azure.microsoft.com/solutions/microservice-applications/).

## <a name="design-applications-composed-of-stateless-and-stateful-microservices"></a>Návrh aplikace skládá z bezstavové a stavové mikroslužeb
Vytváření aplikací u rolí pracovního procesu Azure Cloud Service je příkladem bezstavové služby. Naproti tomu stavová mikroslužeb zachovat jejich autoritativní stavu nad rámec hello požadavku a odpovědi. To poskytuje vysokou dostupnost a konzistence hello stavu prostřednictvím jednoduché rozhraní API, které poskytují transakční záruky zajištěna replikace. Stavové služby Service Fabric demokratizujte vysokou dostupnost, převedení tooall typy aplikací, nikoli pouze databází a dalších datových úložišť. Toto je přirozené průběh. Aplikací mít již přesunout pomocí čistě relačních databází pro vysokou dostupnost tooNoSQL databáze. Nyní aplikace hello, samotné může mít jejich "horkých" stav a data spravovaná v nich pro zvýšení výkonu další, aniž by došlo ke ztrátě spolehlivost, konzistence a dostupnosti.

Při vytváření aplikace, který se skládá z mikroslužeb, obvykle mají kombinaci bezstavové webových aplikací (ASP.NET, Node.js, atd.) volání do firemní bezstavové a stavové služby střední vrstvy, všechny nasazené do hello stejný cluster Service Fabric pomocí příkazů nasazení Service Fabric hello. Každá z těchto služeb je nezávislé s ohledem tooscale, spolehlivost a využití prostředků, výrazně zvýšení flexibility v rámci správy vývoj a životního cyklu.

Stavová mikroslužeb zjednodušit návrhů aplikace, protože odeberou hello potřebu další fronty hello a mezipaměti, které byly obvyklým požadovaná tooaddress hello dostupnosti a latence požadavky čistě bezstavové aplikace. Vzhledem k tomu, že vysoce dostupné a nízkou latencí jsou přirozeně stavové služby, to znamená, že jsou v aplikaci jako celek méně přesunutí toomanage částí. Hello následující diagramy znázorňují hello rozdíly mezi návrhu aplikace, která je bezstavové a ten, který je stavový. Díky hello [spolehlivé služby](service-fabric-reliable-services-introduction.md) a [Reliable Actors](service-fabric-reliable-actors-introduction.md) programovací modely, stavové služby snížit složitost aplikace při dosažení vysoké prostupnosti a nízké latence.

## <a name="an-application-built-using-stateless-services"></a>Aplikace vyvíjené v bezstavové služby
![Aplikace pomocí bezstavové služby][Image1]

## <a name="an-application-built-using-stateful-services"></a>Aplikace vyvíjené v stavové služby
![Aplikace pomocí bezstavové služby][Image2]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky

* Naslouchání příliš[Zákaznické případové studie](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=qDJnf86yC_5206218965
)
* Přečtěte si informace o [Zákaznické případové studie](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/)
* Další informace o [vzory a scénáře](service-fabric-patterns-and-scenarios.md)

* Začínáme vytváření bezzstavovými i stavovými službami s hello Service Fabric [spolehlivé služby](service-fabric-reliable-services-quick-start.md) a [spolehlivé aktéři](service-fabric-reliable-actors-get-started.md) programovací modely.
* Viz také hello následující témata:
  * [Chci se dozvědět o mikroslužeb](service-fabric-overview-microservices.md)
  * [Definovat a spravovat stav služby](service-fabric-concepts-state.md)
  * [Dostupnost služeb Service Fabric](service-fabric-availability-services.md)
  * [Škálování služby Service Fabric](service-fabric-concepts-scalability.md)
  * [Oddíl služby Service Fabric](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
