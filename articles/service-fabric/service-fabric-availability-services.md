---
title: "Dostupnost služeb Service Fabric | Microsoft Docs"
description: "Popisuje zjišťování chyb, převzetí služeb při selhání a obnovení pro služby"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 279ba4a4-f2ef-4e4e-b164-daefd10582e4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 41ff2c3129facb0eea9d896ce75d7343ae2a018e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="availability-of-service-fabric-services"></a>Dostupnost služeb Service Fabric
Tento článek poskytuje přehled o tom, jak Service Fabric udržuje dostupnosti služby.

## <a name="availability-of-service-fabric-stateless-services"></a>Dostupnost bezstavové služby Service Fabric
Služby Azure Service Fabric může být stavová nebo bezstavové. Bezstavové služby je služba aplikace, která nemá žádné [lokálního stavu](service-fabric-concepts-state.md) které musí být vysoce k dispozici nebo není spolehlivá.

Vytvoření bezstavové služby vyžaduje definování `InstanceCount`. Počet instancí definuje počet instancí bezstavové služby aplikační logiky, která by měla být spuštěná v clusteru. Zvýšení počtu instancí je doporučený způsob škálování bezstavové služby.

Pokud instance bezstavové s názvem služby nezdaří, je vytvořena nová instance na některé oprávněné uzlu v clusteru. Například instance bezstavové služby může dojít k selhání na Uzel1 a znovu vytvořen na počítač Uzel5.

## <a name="availability-of-service-fabric-stateful-services"></a>Dostupnost stavové služby Service Fabric
Stavová služba má některé stavu s ním spojená. V Service Fabric stavové služby je modelovaná jako sadu replik. Každá replika je spuštěné instance kódu služby, která má také kopii stavu pro danou službu. Operace čtení a zápisu jsou prováděny v jedné replice (označovaný jako primární). Změny stavu z zápisu operace jsou *replikovat* do jiných replik sady replik. (názvem aktivní sekundární databáze) a použít. 

Může existovat pouze jedna primární replika, ale může být více aktivní sekundární repliky. Počet aktivní sekundární repliky se dá konfigurovat, a vyšší počet replik tolerovat větší počet souběžných softwaru a selhání hardwaru.

Pokud se primární replika přestane fungovat, Service Fabric provede jeden aktivní sekundární repliky nový primární repliky. Tato aktivní sekundární replika už má v aktualizovaném stavu (prostřednictvím *replikace*), a můžete pokračovat ve zpracovávání pro další čtení a zápisu operace.

Tento koncept je buď primární, nebo aktivní sekundární repliky se označuje jako Role repliky.

### <a name="replica-roles"></a>Role repliky
Role repliky se používá ke správě životního cyklu stavu spravuje repliky. Požadavků na čtení repliku, jejichž role je primární služby. Primární také zpracovává všechny požadavky na zápis aktualizace stavu a replikovat změny. Tyto změny se použijí k aktivní sekundární databáze v replikách. Úloha aktivní sekundární je přijímat změny stavu, které primární repliky replikována a aktualizujte jeho zobrazení stavu.

> [!NOTE]
> Vyšší úrovně programování modelů, jako [Reliable Actors](service-fabric-reliable-actors-introduction.md) a [spolehlivé služby](service-fabric-reliable-services-introduction.md) skrýt koncept role repliky od vývojáře. V aktéři je zbytečné, při služby je z velké části jednodušší pro většinu scénářů představu o roli.
>

## <a name="next-steps"></a>Další kroky
Další informace o konceptech Service Fabric najdete v následujících článcích:

- [Škálování služby Service Fabric](service-fabric-concepts-scalability.md)
- [Vytváření oddílů služby Service Fabric](service-fabric-concepts-partitioning.md)
- [Definování a správu stavu](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)
