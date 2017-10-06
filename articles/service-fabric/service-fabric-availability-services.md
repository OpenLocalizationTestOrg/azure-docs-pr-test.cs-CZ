---
title: "aaaAvailability služby Service Fabric | Microsoft Docs"
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
ms.openlocfilehash: c443aadfe31a1413359b08d34c4b7dd5db4edd16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="availability-of-service-fabric-services"></a>Dostupnost služeb Service Fabric
Tento článek poskytuje přehled o tom, jak Service Fabric udržuje dostupnosti služby.

## <a name="availability-of-service-fabric-stateless-services"></a>Dostupnost bezstavové služby Service Fabric
Služby Azure Service Fabric může být stavová nebo bezstavové. Bezstavové služby je služba aplikace, která nemá žádné [lokálního stavu](service-fabric-concepts-state.md) potřebného toobe vysoce k dispozici nebo není spolehlivá.

Vytvoření bezstavové služby vyžaduje definování `InstanceCount`. počet instancí Hello definuje hello počet instancí hello bezstavové služby aplikační logiky, která by měla být spuštěná v clusteru hello. Zvýšením počtu hello instancí je hello doporučená způsob škálování bezstavové služby.

Pokud instance bezstavové s názvem služby nezdaří, je vytvořena nová instance na některé oprávněné uzlu v clusteru hello. Například instance bezstavové služby může dojít k selhání na Uzel1 a znovu vytvořen na počítač Uzel5.

## <a name="availability-of-service-fabric-stateful-services"></a>Dostupnost stavové služby Service Fabric
Stavová služba má některé stavu s ním spojená. V Service Fabric stavové služby je modelovaná jako sadu replik. Každá replika je spuštěné instance hello kódu hello služby, která má také kopii hello stavu pro danou službu. Operace čtení a zápisu jsou prováděny v jedné replice (nazývané hello primární). Změny toostate z operace zápisu jsou *replikovat* toohello replikami v sady replik hello (označovaný jako aktivní sekundární databáze) a použít. 

Může existovat pouze jedna primární replika, ale může být více aktivní sekundární repliky. Hello počet aktivní sekundární repliky se dají konfigurovat a vyšší počet replik tolerovat větší počet souběžných softwaru a selhání hardwaru.

Pokud se primární repliky hello přestane fungovat, Service Fabric provede jeden hello aktivní sekundární repliky hello nové primární replice. Tato aktivní sekundární replika už má verzi hello aktualizovat stav hello (prostřednictvím *replikace*), a můžete pokračovat ve zpracovávání pro další čtení a zápisu operace.

Tento koncept je buď primární, nebo aktivní sekundární repliky se označuje jako hello Role repliky.

### <a name="replica-roles"></a>Role repliky
Hello role repliky je použité toomanage hello životní cyklus hello stavu spravuje repliky. Požadavků na čtení repliku, jejichž role je primární služby. Hello primární také zpracovává všechny požadavky na zápis aktualizace stavu a replikace změn hello. Tyto změny jsou použité toohello aktivní sekundární databáze v hello sady replik. Úloha Hello Active sekundárního objektu je tooreceive změny stavu, které hello primární repliky replikována a aktualizujte jeho zobrazení stavu hello.

> [!NOTE]
> Vyšší úrovně programování modelů, jako [Reliable Actors](service-fabric-reliable-actors-introduction.md) a [spolehlivé služby](service-fabric-reliable-services-introduction.md) skrýt hello konceptu role repliky z hello developer. V aktéři je zbytečné, při služby je z velké části jednodušší pro většinu scénářů hello představu o roli.
>

## <a name="next-steps"></a>Další kroky
Další informace o konceptech Service Fabric najdete v tématu hello následující články:

- [Škálování služby Service Fabric](service-fabric-concepts-scalability.md)
- [Vytváření oddílů služby Service Fabric](service-fabric-concepts-partitioning.md)
- [Definování a správu stavu](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)
