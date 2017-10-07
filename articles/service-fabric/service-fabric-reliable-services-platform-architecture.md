---
title: "Architektura služby aaaReliable | Microsoft Docs"
description: "Přehled architektury hello spolehlivá služba pro stavová a Bezstavová služby"
services: service-fabric
documentationcenter: .net
author: AlanWarwick
manager: timlt
editor: vturecek
ms.assetid: af002ae6-7f6d-4769-b049-82aa1ba0891b
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/30/2016
ms.author: alanwar
redirect_url: /azure/service-fabric/service-fabric-reliable-services-introduction
ms.openlocfilehash: d2d0ec9600275ae248ab7717be269cc7204a1e4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Architektura pro stavová a Bezstavová spolehlivé služby
Spolehlivé služby Azure Service Fabric může být stavová nebo bezstavové. Každý typ služby běží v rámci konkrétní architektury. Tyto architektury jsou popsané v tomto článku.
V tématu hello [přehled spolehlivé služby](service-fabric-reliable-services-introduction.md) Další informace o hello rozdíly mezi stavová a Bezstavová služby.

## <a name="stateful-reliable-services"></a>Stavová spolehlivé služby
### <a name="architecture-of-a-stateful-service"></a>Architektura stavové služby
![Diagram architektury stavové služby](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Stavové spolehlivé služby
Stavové spolehlivé služby lze odvozovat od hello StatefulService nebo StatefulServiceBase třídy. Obě tyto základní třídy jsou poskytovány Service Fabric. Nabízejí různé úrovně podpory a abstrakci pro stavové služby toointerface hello s Service Fabric – a tooparticipate jako služba v rámci clusteru Service Fabric hello.

StatefulService je odvozena z StatefulServiceBase. StatefulServiceBase services nabízí větší flexibilitu, ale vyžaduje další znalost interní hello položky Service Fabric.
Najdete v části hello [přehled spolehlivé služby](service-fabric-reliable-services-introduction.md) a [spolehlivá služba advanced využití](service-fabric-reliable-services-advanced-usage.md) pro další informace o hello specifika zápis služeb pomocí třídy StatefulService a StatefulServiceBase hello .

Obě třídy base spravovat hello životnost a role implementace služby hello. implementace služby Hello může přepsat virtuální metody buď základní třídy, pokud implementace služby hello má pracovní toodo v těchto fázích životního cyklu implementace služby hello – nebo pokud chce toocreate objekt komunikaci naslouchacího procesu. Všimněte si, že i když implementace služby může implementovat vlastní objekt naslouchací proces komunikace vystavení ICommunicationListener, v diagramu hello výše, naslouchací proces komunikace hello je implementováno modulem Service Fabric – jako hello implementace služby používá komunikace naslouchací proces, který je implementováno modulem Service Fabric.

Stavová spolehlivá služba používá hello spolehlivé stavu manager tootake výhod spolehlivé kolekce. Spolehlivé kolekce jsou místní datové struktury, které jsou vysoce dostupné toohello služby--, která je, jsou vždy k dispozici, bez ohledu na převzetí služeb při selhání služby. Každý typ spolehlivé kolekce je implementováno modulem zprostředkovatele spolehlivé stavu.
Další informace o spolehlivé kolekcí najdete v tématu hello [spolehlivé kolekce přehled](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Správce spolehlivé stavu a zprostředkovatelů stavu
Správce spolehlivé stavu Hello je hello objekt, který spravuje zprostředkovatelů spolehlivé stavu. Má hello toocreate funkce, odstranit, výčet a zajistěte, aby zprostředkovatelů spolehlivé stavu hello trvalý a vysokou dostupností. Instanci poskytovatele spolehlivé stavu představuje instanci trvalý a vysoce dostupné datová struktura, jako je například slovník nebo fronty.

Každý poskytovatel spolehlivé stavu zpřístupňuje rozhraní, které používá toointeract stavové služby s zprostředkovatele spolehlivé stavu hello. IReliableDictionary je třeba použít toointerface hello spolehlivé slovník, při IReliableQueue použité toointerface s frontou spolehlivé hello. Všichni poskytovatelé spolehlivé stavu implementovat rozhraní IReliableState hello.

Správce spolehlivé stavu Hello má rozhraní s názvem IReliableStateManager, která umožňuje přístup tooit z stavové služby. Zprostředkovatelů stavu tooreliable rozhraní, jsou vráceny prostřednictvím IReliableStateManager.

Správce spolehlivé stavu Hello používá modul plug-in architektura tak, aby nové typy spolehlivé kolekcí může být zapojen dynamicky.

slovník Hello spolehlivé a spolehlivé fronty jsou založena na hello implementace výkonné a verzí rozdílové úložiště.

### <a name="transactional-replicator"></a>Transakční Replikátor
Hello transakční Replikátor komponenta je zodpovědná za zajištění, že hello stavu služby (to znamená, hello stav v rámci správce spolehlivé stavu hello a spolehlivé kolekce hello) je konzistentní napříč všechny repliky spuštěna služba hello. Také zajistí, že stav hello je uchován v hello protokolu. rozhraní správce Hello spolehlivé stavu s hello transakční Replikátor prostřednictvím privátní mechanismus.

transakční Replikátor Hello používá stavu toocommunicate protokol sítě s další repliky instance služby hello tak, aby všechny repliky informace aktuální stav.

transakční Replikátor Hello používá informace o protokolu toopersist stavu tak, aby informace o stavu hello odolává procesu nebo uzel dojde k chybě. protokol toohello rozhraní Hello je prostřednictvím privátní mechanismus.

### <a name="log"></a>Protokol
součásti protokolu Hello poskytuje vysoce výkonné trvalého úložiště, který může být optimalizované pro zápis toospinning nebo SSD disky.  návrh Hello hello protokolu je pro trvalé úložiště hello (tj, pevné disky) toobe místní toohello uzlů, které běží hello stavové služby. To umožňuje nízkou latenci a vysokou propustnost, jako trvalé úložiště porovnání tooremote, která není místní toohello uzlu.

součásti protokolu Hello používá víc souborů protokolů. Neexistuje uzel celou sdílené protokolového souboru, který všechny repliky používat k ukládání dat stavu může poskytnout hello nejnižší latenci a nejvyšší propustnost. Ve výchozím nastavení hello sdílené protokolu je umístěn v adresáři pracovní uzlu hello Service Fabric, ale může být také nakonfigurované toobe umístěn na jiné umístění, v ideálním případě na disk vyhrazený pro pouze hello sdílené protokolu. Každá replika služby hello má také soubor protokolu vyhrazené a hello vyhrazené protokolu je umístěn v rámci služby hello pracovní adresář. Neexistuje žádný mechanismus tooconfigure hello vyhrazené protokolu toobe umístěn na jiné umístění.

sdílené protokolu Hello přechodném oblast pro informace o stavu repliky hello se při hello vyhrazené protokol hello konečného umístění, kde je trvalá. V tomto návrhu informace o stavu hello je první napsané toohello sdílený soubor protokolu a líné přesunout toohello vyhrazené souboru protokolu v pozadí hello. Tímto způsobem hello zápisu toohello sdílené protokolu by měla mít hello nejnižší latenci a nejvyšší propustnost, což umožňuje rychlejší průběh toomake služby hello.

Čte a zapisuje toohello sdílené protokolu se provádí přes přímé vstupně-výstupní operace toopreallocated místa na disku hello hello sdíleného souboru protokolu. tooallow optimální využití místa na disku na jednotce hello s vyhrazenou protokoly, hello vyhrazené soubor protokolu se vytvoří jako zhuštěného souboru systému souborů NTFS. Všimněte si, že to vám umožní předimenzování místa na disku a hello operačního systému se zobrazí hello vyhrazené soubory protokolů pomocí mnohem více místa na disku než je ve skutečnosti použít.

Kromě zajištění dostatečného minimální uživatelského režimu rozhraní toohello protokolu hello protokolu zapisuje jako ovladač režimu jádra. Spuštění se ovladač režimu jádra, hello protokolu poskytnout nejvyšší výkon hello tooall služby, které ho používají.

Další informace o konfiguraci protokolu hello najdete v tématu [konfigurace stavová spolehlivé služby](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Bezstavové spolehlivé služby
### <a name="architecture-of-a-stateless-service"></a>Architektura bezstavové služby
![Diagram architektury bezstavové služby](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Bezstavové spolehlivé služby
Implementace bezstavové služby odvozen od třídy StatelessServiceBase nebo hello StatelessService. Hello StatelessServiceBase třída umožňuje více flexibility než hello StatelessService třídy.
Obě třídy base spravovat hello životnost a role služby.

implementace služby Hello může přepsat virtuální metody buď základní třídy, pokud má služba hello pracovní toodo v těchto bodech v průběhu životního cyklu hello služby--nebo pokud chce toocreate objekt komunikaci naslouchacího procesu. Všimněte si, že i když služba hello může implementovat vlastní objekt naslouchací proces komunikace vystavení ICommunicationListener, v diagramu hello výše, naslouchací proces komunikace hello je implementováno modulem Service Fabric, jako že implementace služby používá komunikaci ve formě naslouchací proces, který je implementováno modulem Service Fabric.

Najdete v části hello [přehled spolehlivé služby](service-fabric-reliable-services-introduction.md) a [spolehlivá služba advanced využití](service-fabric-reliable-services-advanced-usage.md) pro podrobnější informace hello zápisu služeb pomocí třídy StatelessService a StatelessServiceBase hello .

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
Další informace o Service Fabric najdete v tématu:

[Přehled spolehlivé služby](service-fabric-reliable-services-introduction.md)

[Rychlý start](service-fabric-reliable-services-quick-start.md)

[Přehled spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md)

[Spolehlivé služby advanced využití](service-fabric-reliable-services-advanced-usage.md)

[Konfigurace spolehlivé služby](service-fabric-reliable-services-configuration.md)  

