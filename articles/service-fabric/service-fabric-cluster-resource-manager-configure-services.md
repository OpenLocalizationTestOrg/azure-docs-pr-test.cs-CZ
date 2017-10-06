---
title: "nastavení metriky a umístění aaaSpecify v Azure mikroslužeb | Microsoft Docs"
description: "Zadáním metriky, omezení umístění a další zásady umístění, která popisují služby Service Fabric."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 16e135c1-a00a-4c6f-9302-6651a090571a
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: c633518b5dbf0c7b84e0d46c06bf1f92365d184d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Konfigurace nastavení správce prostředků clusteru služby Service Fabric
Hello správce prostředků clusteru Service Fabric umožňuje jemně odstupňovanou kontrolu nad hello pravidla, která řídí každé jednotlivé s názvem služby. Každé pojmenované služby lze určit pravidla, jak by měla být přidělená v clusteru hello. Každé pojmenované služby můžete definovat i hello sadu metriky, že požaduje tooreport, včetně toho, jak důležité jsou toothat služby. Konfigurace služeb, rozděleny do tří různých úloh:

1. Konfigurace omezení umístění
2. Konfigurace metriky
3. Konfigurace zásady rozšířené umístění a ostatní pravidla (méně běžné)

## <a name="placement-constraints"></a>Omezení umístění
Omezení umístění jsou použité toocontrol, které uzly v clusteru hello služby můžete ve skutečnosti spustit. Obvykle konkrétní s názvem instance služby nebo všechny služby toorun daného typu omezené na konkrétní typ uzlu. Omezení umístění je rozšiřitelný. Můžete definovat libovolnou sadu vlastnosti na typ uzlu a potom vyberte pro ně s omezeními při vytváření služby. Můžete také změnit omezení umístění služby, když je spuštěná. To vám umožní toorespond toochanges v clusteru hello nebo hello požadavky služby hello. Hello daný uzel můžete také aktualizovat vlastnosti dynamicky v clusteru hello. Další informace o omezení umístění a jak tooconfigure je možné najít v [v tomto článku](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)

## <a name="metrics"></a>Metriky
Metriky jsou hello sadu prostředků, které potřebuje uvedená služba s názvem. Konfigurace metriky služby zahrnuje kolik prostředku každé stavová repliky nebo bezstavové instance této služby spotřebuje ve výchozím nastavení. Metriky taky zahrnovat váhu určující, jak důležité vyrovnávání této metrika je toothat služba, v případě, že jsou kompromisy nezbytné.

## <a name="advanced-placement-rules"></a>Pravidla pro pokročilé umístění
Existují jiné typy pravidla pro umístění, které jsou užitečné v méně běžné scénáře. Tady je několik příkladů:
- Omezení, které pomáhají s mnoha místech pracujícími clustery
- Některé architektury aplikace

Prostřednictvím korelací nebo zásady jsou nakonfigurované další pravidla pro umístění.

## <a name="next-steps"></a>Další kroky
- Metriky se, jak hello správce prostředků clusteru Service Fabric spravuje využívání a kapacity v clusteru hello. Další informace o metrikách toolearn a jak tooconfigure, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)
- Spřažení je jeden režim, které můžete konfigurovat pro vaše služby. Není běžné, ale pokud to potřebujete informace o najdete ho [sem](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
- Existuje mnoho různých umístění pravidla, která se dá konfigurovat na vaše služba toohandle další scénáře. Můžete zjistit o těchto zásadách různých umístění [sem](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
- Začít od začátku hello a [získat Úvod toohello správce prostředků clusteru Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
- toofind si o tom, jak hello správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru hello, podívejte se na článek hello na [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)
- Hello správce prostředků clusteru má mnoho možností pro popisující hello clusteru. toofind Další informace o jejich, projděte si tento článek na [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)
