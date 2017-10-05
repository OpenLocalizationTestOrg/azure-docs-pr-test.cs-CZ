---
title: "Architektura spolehlivé služby | Microsoft Docs"
description: "Přehled architektury spolehlivá služba pro stavová a Bezstavová služby"
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
ms.openlocfilehash: a00a16945356b9731485554e06df46528b5c7bb2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Architektura pro stavová a Bezstavová spolehlivé služby
Spolehlivé služby Azure Service Fabric může být stavová nebo bezstavové. Každý typ služby běží v rámci konkrétní architektury. Tyto architektury jsou popsané v tomto článku.
Najdete v článku [přehled spolehlivé služby](service-fabric-reliable-services-introduction.md) Další informace o rozdílech mezi stavová a Bezstavová služby.

## <a name="stateful-reliable-services"></a>Stavová spolehlivé služby
### <a name="architecture-of-a-stateful-service"></a>Architektura stavové služby
![Diagram architektury stavové služby](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Stavové spolehlivé služby
Stavové spolehlivé služby můžete odvozena od třídy buď StatefulService nebo StatefulServiceBase. Obě tyto základní třídy jsou poskytovány Service Fabric. Nabízejí různé úrovně podpory a abstrakci pro stavové služby rozhraní pro aplikaci Service Fabric – a aby se jako služba v rámci clusteru Service Fabric.

StatefulService je odvozena z StatefulServiceBase. StatefulServiceBase services nabízí větší flexibilitu, ale vyžaduje další vysvětlení interních Service Fabric.
Najdete v článku [přehled spolehlivé služby](service-fabric-reliable-services-introduction.md) a [spolehlivá služba advanced využití](service-fabric-reliable-services-advanced-usage.md) Další informace o specifikacích zápis služeb pomocí třídy StatefulService a StatefulServiceBase.

Obě třídy base správu životního cyklu a role implementace služby. Implementace služby může přepsat virtuální metody buď základní třídy, pokud má implementace služby, aby se v těchto bodech v životním cyklu implementace služby--nebo pokud chce vytvořit objekt naslouchacího procesu komunikace. Všimněte si, že i když implementace služby může implementovat vlastní objekt naslouchací proces komunikace vystavení ICommunicationListener, v diagramu výše, naslouchací proces komunikace je implementováno modulem Service Fabric – jako u implementace služby komunikace naslouchací proces, který je implementováno modulem Service Fabric.

Stavová spolehlivá služba používá správce spolehlivé stavu využívat výhod spolehlivé kolekce. Místní datové struktury, které jsou vysoce dostupné pro službu--, která jsou spolehlivé kolekce, jsou vždy k dispozici, bez ohledu na převzetí služeb při selhání služby. Každý typ spolehlivé kolekce je implementováno modulem zprostředkovatele spolehlivé stavu.
Další informace o spolehlivé kolekcí najdete v tématu [spolehlivé kolekce přehled](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Správce spolehlivé stavu a zprostředkovatelů stavu
Správce spolehlivé stavu je objekt, který spravuje zprostředkovatelů spolehlivé stavu. Obsahuje funkci pro vytvoření, odstranění, výčet a zajistěte, aby zprostředkovatelů spolehlivé stavu trvalé a vysoce dostupné. Instanci poskytovatele spolehlivé stavu představuje instanci trvalý a vysoce dostupné datová struktura, jako je například slovník nebo fronty.

Každý poskytovatel spolehlivé stavu zpřístupňuje rozhraní, které se používají k interakci s poskytovatelem spolehlivé stavu stavové služby. Například IReliableDictionary slouží k rozhraní spolehlivé slovník, přičemž IReliableQueue se používá k rozhraní s spolehlivé fronty. Všichni poskytovatelé spolehlivé stavu implementovat rozhraní IReliableState.

Správce spolehlivé stavu má rozhraní s názvem IReliableStateManager, který umožňuje přístup k němu z stavové služby. Rozhraní, které se zprostředkovatelů spolehlivé stavu, jsou vráceny prostřednictvím IReliableStateManager.

Správce spolehlivé stavu používá modul plug-in architektura tak, aby nové typy spolehlivé kolekcí může být zapojen dynamicky.

Slovník spolehlivé a spolehlivé fronty jsou založena na provádění vysoce výkonné, verzí rozdílové úložiště.

### <a name="transactional-replicator"></a>Transakční Replikátor
Součást transakční Replikátor zodpovídá za zajištění, že stav služby (to znamená, stav v rámci správce spolehlivé stavu a spolehlivé kolekce) je konzistentní napříč všechny repliky spuštěna služba. Také zajistí, že stav je uchován v protokolu. Rozhraní správce spolehlivé stavu s transakční Replikátor prostřednictvím privátní mechanismus.

Transakční Replikátor používá síťový protokol pro komunikaci stavu s ostatními replikami v instanci služby tak, aby všechny repliky informace aktuální stav.

Transakční Replikátor používá protokolu pro zachování informací o stavu tak, aby odolává informace o stavu procesu nebo dojde k chybě uzlu. Rozhraní v protokolu je prostřednictvím privátní mechanismus.

### <a name="log"></a>Protokol
Součásti protokolu poskytuje vysoce výkonné trvalého úložiště, který lze optimalizovat pro zápis na otáčejícího nebo SSD disky.  Návrh protokolu je pro trvalé úložiště (tj, pevné disky) jako místní pro uzly, které běží stavové služby. To umožňuje nízkou latenci a vysokou propustnost, porovnání vzdálené trvalé úložiště, která není místní do uzlu.

Součásti protokolu používá víc souborů protokolů. Neexistuje uzel celou sdílené protokolového souboru, který všech replik použijte ho k ukládání dat stavu poskytuje nejnižší latenci a nejvyšší propustnost. Ve výchozím nastavení je protokol sdílené umístěn v adresáři pracovní uzel Service Fabric, ale může být také nakonfigurována umístit na jiném umístění, v ideálním případě na disk vyhrazený pro pouze protokol sdílené. Každou repliku pro službu má také soubor protokolu vyhrazené a vyhrazené protokolu je umístěn v rámci služby pracovní adresář. Neexistuje žádný mechanismus konfigurace vyhrazené protokolu, který chcete být umístěny na jiné umístění.

Protokol sdílené je přechodná oblast pro informace o stavu repliky, je soubor protokolu vyhrazené konečného umístění, kde je trvalé. V tomto návrhu je informace o stavu nejprve do sdíleného souboru protokolu zapíše a pak líné přesunut do souboru protokolu vyhrazené na pozadí. Tímto způsobem by mít zápis do sdílené protokolu nejnižší latenci a nejvyšší propustnost, což umožňuje službě rychleji průběh.

Čtení a zápis do sdílené protokolu se provádí přes přímé vstupně-výstupní operace pro předběžně přidělené místo na disku pro soubor sdílený protokolu. Umožňuje optimální využívání místa na disku na jednotce s vyhrazenou protokoly se vytvoří soubor protokolu vyhrazený jako zhuštěného souboru systému souborů NTFS. Všimněte si, že to vám umožní předimenzování místa na disku a operačního systému se zobrazí soubory vyhrazené protokolů pomocí mnohem více místa na disku než je ve skutečnosti použít.

Kromě zajištění dostatečného minimální uživatelského režimu rozhraní do protokolu protokol je zapsán jako ovladač režimu jádra. Spuštění se ovladač režimu jádra, protokol poskytnout nejvyšší výkon ke všem službám, které ho používají.

Další informace o konfiguraci protokolu najdete v tématu [konfigurace stavová spolehlivé služby](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Bezstavové spolehlivé služby
### <a name="architecture-of-a-stateless-service"></a>Architektura bezstavové služby
![Diagram architektury bezstavové služby](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Bezstavové spolehlivé služby
Implementace bezstavové služby odvozena od třídy StatelessService nebo StatelessServiceBase. Třída StatelessServiceBase umožňuje více flexibility než StatelessService třídy.
Obě třídy base správu životního cyklu a role služby.

Implementace služby může přepsat virtuální metody buď základní třídy, pokud služba má práce uděláte v těchto bodech v průběhu životního cyklu služby--nebo pokud chce vytvořit objekt naslouchacího procesu komunikace. Všimněte si, že i když služba může implementovat vlastní objekt naslouchací proces komunikace vystavení ICommunicationListener, v diagramu výše, naslouchací proces komunikace je implementováno modulem Service Fabric jako implementace této služby používá komunikaci ve formě naslouchací proces, který je implementováno modulem Service Fabric.

Najdete v článku [přehled spolehlivé služby](service-fabric-reliable-services-introduction.md) a [spolehlivá služba advanced využití](service-fabric-reliable-services-advanced-usage.md) Další informace o specifikacích zápis služeb pomocí třídy StatelessService a StatelessServiceBase.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky
Další informace o Service Fabric najdete v tématu:

[Přehled spolehlivé služby](service-fabric-reliable-services-introduction.md)

[Rychlý start](service-fabric-reliable-services-quick-start.md)

[Přehled spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md)

[Spolehlivé služby advanced využití](service-fabric-reliable-services-advanced-usage.md)

[Konfigurace spolehlivé služby](service-fabric-reliable-services-configuration.md)  

