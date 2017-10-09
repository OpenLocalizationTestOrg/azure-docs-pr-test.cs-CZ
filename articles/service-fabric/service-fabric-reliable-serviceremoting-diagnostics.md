---
title: "aaaAzure ServiceFabric diagnostiky a monitorování | Microsoft Docs"
description: "Tento článek popisuje hello funkce monitorování výkonu v modulu runtime Service Fabric spolehlivé ServiceRemoting hello, jako čítače výkonu vygenerované tímto."
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: suchiagicha
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: suchiagicha
ms.openlocfilehash: 64db9a890bd59a1326e587d14b89c007b71a9059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-service-remoting"></a>Diagnostika a sledování výkonu pro vzdálenou komunikaci spolehlivé služby
vysílá Hello spolehlivé ServiceRemoting runtime [čítače výkonu](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Tyto poskytují přehled o tom, jak pracuje hello ServiceRemoting a pomoci při řešení potíží a monitorování výkonu.


## <a name="performance-counters"></a>Čítače výkonu
modul runtime spolehlivé ServiceRemoting Hello definuje hello následující kategorie čítače výkonu:

| Kategorie | Popis |
| --- | --- |
| Služba Fabric |Čítače specifické tooAzure vzdálené komunikace služby Service Fabric, například tooprocess Průměrná doba požadavku |
| Metoda služby Service Fabric |Čítače specifické toomethods implementované Fabric vzdálené komunikace služby, například jak často je volána metoda služby |

Každý z předchozích kategorií hello má jeden nebo více čítačů.

Hello [sledování výkonu systému Windows](https://technet.microsoft.com/library/cc749249.aspx) aplikace, která je k dispozici v operačním systému Windows hello ve výchozím nastavení může být použité toocollect a zobrazení data čítače výkonu. [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) je další možností pro shromažďování dat čítačů výkonu a pak ho nahrát tooAzure tabulky.

### <a name="performance-counter-instance-names"></a>Názvy instancí čítače výkonu
Cluster, který má velký počet ServiceRemoting služby nebo oddíly mít velký počet instancí čítače výkonu. Hello instance čítače výkonu, které názvy může pomoci při identifikaci hello konkrétní oddíl a metoda služby (pokud existuje) této instance čítače výkonu hello je přidružen.

#### <a name="service-fabric-service-category"></a>Kategorie Service Fabric Service
Pro kategorii hello `Service Fabric Service`, názvy instancí čítače hello jsou ve formátu hello:

`ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*ServiceFabricPartitionID* je hello řetězcovou reprezentaci hello je přidružen ID oddílu Service Fabric, která hello instance čítače výkonu. ID oddílu Hello je identifikátor GUID a jeho řetězcovou reprezentaci vygeneruje prostřednictvím hello [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) metoda s specifikátor formátu "D".

*ServiceReplicaOrInstanceId* je hello řetězcovou reprezentaci hello je přidružen ID prostředků infrastruktury repliky nebo Instance služby, který hello instance čítače výkonu.

*ServiceRuntimeInternalID* je hello řetězcovou reprezentaci 64bitové celé číslo, který je generovaný modulu runtime Fabric Service hello jeho interní použití. Toto je zahrnuta v instance čítače výkonu hello název tooensure jeho jedinečnosti a aby nedošlo ke konfliktu s další názvy instancí čítače výkonu. Uživatelé by neměl pokusí toointerpret tuto část hello název instance čítače výkonu.

Hello tady je příklad názvu instance čítače čítačů, které patří toohello `Service Fabric Service` kategorie:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046_5008379932`

V předchozím příkladu hello `2740af29-78aa-44bc-a20b-7e60fb783264` je hello řetězcovou reprezentaci ID oddílu hello Service Fabric, `635650083799324046` je řetězcová reprezentace repliky nebo InstanceId a `5008379932` je hello 64-bit ID, které se generuje pro interní hello runtime použití.

#### <a name="service-fabric-service-method-category"></a>Kategorie metody služby Service Fabric
Pro kategorii hello `Service Fabric Service Method`, názvy instancí čítače hello jsou ve formátu hello:

`MethodName_ServiceRuntimeMethodId_ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*MethodName* je přidružen hello název metody hello služby, která hello instance čítače výkonu. Formát Hello hello název metody je určen na základě některé logiky v hello runtime Fabric Service, který vyrovnává hello čitelnost hello název s omezeními na maximální délku hello názvy instancí čítače výkonu hello v systému Windows.

*ServiceRuntimeMethodId* je hello řetězcovou reprezentaci 32bitové celé číslo, který je generovaný modulu runtime Fabric Service hello jeho interní použití. Toto je zahrnuta v instance čítače výkonu hello název tooensure jeho jedinečnosti a aby nedošlo ke konfliktu s další názvy instancí čítače výkonu. Uživatelé by neměl pokusí toointerpret tuto část hello název instance čítače výkonu.

*ServiceFabricPartitionID* je hello řetězcovou reprezentaci hello je přidružen ID oddílu Service Fabric, která hello instance čítače výkonu. ID oddílu Hello je identifikátor GUID a jeho řetězcovou reprezentaci vygeneruje prostřednictvím hello [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) metoda s specifikátor formátu "D".

*ServiceReplicaOrInstanceId* je hello řetězcovou reprezentaci hello je přidružen ID prostředků infrastruktury repliky nebo Instance služby, který hello instance čítače výkonu.

*ServiceRuntimeInternalID* je hello řetězcovou reprezentaci 64bitové celé číslo, který je generovaný modulu runtime Fabric Service hello jeho interní použití. Toto je zahrnuta v instance čítače výkonu hello název tooensure jeho jedinečnosti a aby nedošlo ke konfliktu s další názvy instancí čítače výkonu. Uživatelé by neměl pokusí toointerpret tuto část hello název instance čítače výkonu.

Hello tady je příklad názvu instance čítače čítačů, které patří toohello `Service Fabric Service Method` kategorie:

`ivoicemailboxservice.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486_5008380`

V předchozím příkladu hello `ivoicemailboxservice.leavemessageasync` je hello název metody, `2` hello 32bitové ID generované pro interní hello runtime používat, je `89383d32-e57e-4a9b-a6ad-57c6792aa521` je hello řetězcovou reprezentaci ID oddílu hello Service Fabric,`635650083804480486` je řetězec hello reprezentace hello ID repliky nebo instanci služby prostředků infrastruktury a `5008380` je ID 64-bit hello generované pro interní hello runtime používat.

## <a name="list-of-performance-counters"></a>Seznam čítačů výkonu
### <a name="service-method-performance-counters"></a>Čítače výkonu služby – metoda

Spolehlivé služby runtime Hello publikuje hello následující čítače výkonu související toohello provádění metody služeb.

| Název kategorie | Název čítače | Popis |
| --- | --- | --- |
| Metoda služby Service Fabric |Volání za sekundu |Počet pokusů, které je vyvolána metoda služby hello za sekundu |
| Metoda služby Service Fabric |Průměrný počet milisekund na vyvolání |Doba trvání metody služby hello tooexecute v milisekundách |
| Metoda služby Service Fabric |Výjimek vyvolaných za sekundu |Počet pokusů, které hello metody služby došlo k výjimce za sekundu |

### <a name="service-request-processing-performance-counters"></a>Čítače výkonu zpracování žádosti o služby
Když klient vyvolá metodu prostřednictvím objekt proxy služby, výsledkem zprávu požadavku odesílány prostřednictvím hello síťové toohello vzdálené komunikace služby. Služba Hello zpracuje zprávu požadavku hello a odešle odpověď zpět toohello klienta. modul runtime spolehlivé ServiceRemoting Hello publikuje hello následující čítače výkonu, související tooservice zpracování žádosti.

| Název kategorie | Název čítače | Popis |
| --- | --- | --- |
| Služba Fabric |Počet zbývajících požadavků |Počet požadavků zpracovaných ve službě hello |
| Služba Fabric |Průměrný počet milisekund na žádost |Doba trvání (v milisekundách) pomocí služby tooprocess hello žádost |
| Služba Fabric |Průměrný počet milisekund deserializace požadavků |Čas (v milisekundách) toodeserialize služby zprávu požadavku při obdržení u služby hello |
| Služba Fabric |Průměrný počet milisekund serializace odpovědí |Zpráva odpovědi čas přijatá (v milisekundách) tooserialize hello služby u služby hello před odesláním odpovědi hello toohello klienta |

## <a name="next-steps"></a>Další kroky
* [Ukázka kódu](https://github.com/Azure/servicefabric-samples)
* [Zprostředkovatelé EventSource v nástroje PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
