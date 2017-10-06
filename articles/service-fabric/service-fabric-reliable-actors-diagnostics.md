---
title: "aaaActors diagnostiky a monitorování | Microsoft Docs"
description: "Tento článek popisuje funkce v modulu runtime Service Fabric Reliable Actors hello, včetně hello události a čítače výkonu vysílaných ho pro sledování výkonu a Diagnostika hello."
services: service-fabric
documentationcenter: .net
author: abhishekram
manager: timlt
editor: vturecek
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: abhisram
ms.openlocfilehash: 5b266d67875722feef5c5be8861bda6d8132a7d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Diagnostika a sledování výkonu služby Reliable Actors
vysílá Hello Reliable Actors runtime [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) události a [čítače výkonu](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Tyto poskytují přehled o tom, jak pracuje hello runtime a pomáhají při řešení potíží a monitorování výkonu.

## <a name="eventsource-events"></a>EventSource události
Název zprostředkovatele EventSource Hello runtime Reliable Actors hello je "Microsoft-ServiceFabric-aktéři". Události z tohoto zdroje událostí se zobrazí ve hello [diagnostiky události](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) okno při hello objektu actor aplikace se [ladit v sadě Visual Studio](service-fabric-debugging-your-application.md).

Příklady nástroje a technologie, které pomáhají v shromažďování nebo zobrazování událostí EventSource [nástroje PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md), [sémantické protokolování](https://msdn.microsoft.com/library/dn774980.aspx)a hello [ Knihovna Microsoft TraceEvent](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Klíčová slova
Všechny události, které patří toohello spolehlivé EventSource aktéři jsou přidruženy k jedné nebo více klíčových slov. To umožňuje filtrování událostí, které byly shromážděny. jsou definovány Hello následující – klíčové slovo bits.

| Bit | Popis |
| --- | --- |
| 0x1 |Nastavit o důležitých událostech, které shrnují hello operaci modulu runtime hello aktéři prostředků infrastruktury. |
| 0x2 |Sadu událostí, které popisují volání objektu actor metod. Další informace najdete v tématu hello [úvodní téma na aktéři](service-fabric-reliable-actors-introduction.md). |
| 0x4 |Nastavení stavu tooactor související události. Další informace najdete v tématu hello na [správy stavu objektu actor](service-fabric-reliable-actors-state-management.md). |
| 0x8 |Sada události související na základě tooturn souběžnost v objektu actor hello. Další informace najdete v tématu hello na [souběžnosti](service-fabric-reliable-actors-introduction.md#concurrency). |

## <a name="performance-counters"></a>Čítače výkonu
modul runtime Reliable Actors Hello definuje hello následující kategorie čítače výkonu.

| Kategorie | Popis |
| --- | --- |
| Prostředky infrastruktury služby objektu Actor |Čítače specifické tooAzure aktéři Service Fabric, například doba trvání stavu objektu actor toosave |
| Metoda objektu Actor pro Service Fabric |Čítače specifické toomethods implementované aktéři Service Fabric, například jak často je volána metoda objektu actor |

Každý hello vyšší kategorie má jeden nebo více čítačů.

Hello [sledování výkonu systému Windows](https://technet.microsoft.com/library/cc749249.aspx) aplikace, která je k dispozici v operačním systému Windows hello ve výchozím nastavení může být použité toocollect a zobrazení data čítače výkonu. [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) je další možností pro shromažďování dat čítačů výkonu a pak ho nahrát tooAzure tabulky.

### <a name="performance-counter-instance-names"></a>Názvy instancí čítače výkonu
Cluster, který má velký počet služeb objektu actor nebo oddílů služby objektu actor bude mít velký počet instancí čítače výkonu objektu actor. Hello názvy instancí čítače výkonu může pomoci při identifikaci hello konkrétní [oddílu](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) a metoda objektu actor (pokud existuje) této instance čítače výkonu hello je přidružen.

#### <a name="service-fabric-actor-category"></a>Kategorie prostředků infrastruktury služby objektu Actor
Pro kategorii hello `Service Fabric Actor`, názvy instancí čítače hello jsou ve formátu hello:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* je hello řetězcovou reprezentaci hello je přidružen ID oddílu Service Fabric, která hello instance čítače výkonu. ID oddílu Hello je identifikátor GUID a jeho řetězcovou reprezentaci vygeneruje prostřednictvím hello [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) metoda s specifikátor formátu "D".

*ActorRuntimeInternalID* je hello řetězcovou reprezentaci 64bitové celé číslo, který je generovaný hello Fabric aktéři runtime pro interní použití. Toto je zahrnuta v instance čítače výkonu hello název tooensure jeho jedinečnosti a aby nedošlo ke konfliktu s další názvy instancí čítače výkonu. Uživatelé by neměl pokusí toointerpret tuto část hello název instance čítače výkonu.

Hello tady je příklad názvu instance čítače čítačů, které patří toohello `Service Fabric Actor` kategorie:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

Ve výše uvedeném příkladu hello `2740af29-78aa-44bc-a20b-7e60fb783264` je hello řetězcovou reprezentaci ID oddílu hello Service Fabric, a `635650083799324046` je použijte hello 64-bit ID, které se generuje pro interní hello runtime.

#### <a name="service-fabric-actor-method-category"></a>Metoda objektu Actor pro Service Fabric kategorie
Pro kategorii hello `Service Fabric Actor Method`, názvy instancí čítače hello jsou ve formátu hello:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* je přidružen hello název hello objektu actor metody, která hello instance čítače výkonu. Formát Hello hello název metody je určen na základě některé postupu v modulu runtime hello aktéři prostředků infrastruktury, který vyrovnává hello čitelnost hello název s omezeními na maximální délku hello názvy instancí čítače výkonu hello v systému Windows.

*ActorsRuntimeMethodId* je hello řetězcovou reprezentaci 32bitové celé číslo, který je generovaný hello Fabric aktéři runtime pro interní použití. Toto je zahrnuta v instance čítače výkonu hello název tooensure jeho jedinečnosti a aby nedošlo ke konfliktu s další názvy instancí čítače výkonu. Uživatelé by neměl pokusí toointerpret tuto část hello název instance čítače výkonu.

*ServiceFabricPartitionID* je hello řetězcovou reprezentaci hello je přidružen ID oddílu Service Fabric, která hello instance čítače výkonu. ID oddílu Hello je identifikátor GUID a jeho řetězcovou reprezentaci vygeneruje prostřednictvím hello [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) metoda s specifikátor formátu "D".

*ActorRuntimeInternalID* je hello řetězcovou reprezentaci 64bitové celé číslo, který je generovaný hello Fabric aktéři runtime pro interní použití. Toto je zahrnuta v instance čítače výkonu hello název tooensure jeho jedinečnosti a aby nedošlo ke konfliktu s další názvy instancí čítače výkonu. Uživatelé by neměl pokusí toointerpret tuto část hello název instance čítače výkonu.

Hello tady je příklad názvu instance čítače čítačů, které patří toohello `Service Fabric Actor Method` kategorie:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

Ve výše uvedeném příkladu hello `ivoicemailboxactor.leavemessageasync` je název metody hello, `2` hello 32bitové ID generované pro interní hello runtime používat, je `89383d32-e57e-4a9b-a6ad-57c6792aa521` je hello řetězcovou reprezentaci ID oddílu hello Service Fabric, a `635650083804480486` je ID hello 64-bit generuje pro interní použití hello runtime.

## <a name="list-of-events-and-performance-counters"></a>Seznam událostí a čítačů výkonu
### <a name="actor-method-events-and-performance-counters"></a>Události metod objektu actor a čítače výkonu
Hello Reliable Actors runtime vysílá hello následující události související příliš[metody objektu actor](service-fabric-reliable-actors-introduction.md).

| Název události | ID události | Úroveň | – Klíčové slovo | Popis |
| --- | --- | --- | --- | --- |
| ActorMethodStart |7 |Verbose |0x2 |Modul runtime aktéři je o tooinvoke metoda objektu actor. |
| ActorMethodStop |8 |Verbose |0x2 |Metoda objektu actor byl dokončen. To znamená vrátila metoda objektu actor toohello asynchronního volání hello runtime a dokončení úlohy hello vráceném metodou objektu actor hello. |
| ActorMethodThrewException |9 |Upozornění |0x3 |Došlo k výjimce během zpracování hello metody objektu actor během metoda objektu actor toohello asynchronního volání hello runtime nebo během provádění hello hello úloh vrácených metoda objektu actor hello. Tato událost ukazuje nějaká chyba v hello objektu actor kódu, který potřebuje šetření. |

modul runtime Reliable Actors Hello publikuje hello následující čítače výkonu související toohello provádění metody objektu actor.

| Název kategorie | Název čítače | Popis |
| --- | --- | --- |
| Metoda objektu Actor pro Service Fabric |Volání za sekundu |Počet pokusů, které hello metody služby objektu actor vyvolala za sekundu |
| Metoda objektu Actor pro Service Fabric |Průměrný počet milisekund na vyvolání |Metoda služby doba tooexecute hello objektu actor v milisekundách |
| Metoda objektu Actor pro Service Fabric |Výjimek vyvolaných za sekundu |Počet pokusů, které hello metody služby objektu actor výjimku za sekundu |

### <a name="concurrency-events-and-performance-counters"></a>Concurrency události a čítače výkonu
Hello Reliable Actors runtime vysílá hello následující události související příliš[souběžnosti](service-fabric-reliable-actors-introduction.md#concurrency).

| Název události | ID události | Úroveň | – Klíčové slovo | Popis |
| --- | --- | --- | --- | --- |
| ActorMethodCallsWaitingForLock |12 |Verbose |0x8 |Tato událost se zapíše na hello začátku každé nové zapnout v objektu actor. Obsahuje hello počet nevyřízených volání objektu actor čekajících tooacquire hello na získání zámku objektu actor který vynucuje na základě zapnout souběžnosti. |

modul runtime Reliable Actors Hello publikuje hello následující související tooconcurrency čítače výkonu.

| Název kategorie | Název čítače | Popis |
| --- | --- | --- |
| Prostředky infrastruktury služby objektu Actor |Počet volání objektu actor čekajících na jeho zámek |Počet nevyřízených volání objektu actor čekajících tooacquire hello na získání zámku objektu actor který vynucuje na základě zapnout souběžnosti |
| Prostředky infrastruktury služby objektu Actor |Průměrný počet milisekund čekání na zámek |Čas (v milisekundách) tooacquire hello na získání zámku objektu actor který vynucuje na základě zapnout souběžnosti |
| Prostředky infrastruktury služby objektu Actor |Průměrný počet milisekund blokování zámku objektu actor |Čas (v milisekundách), pro které hello je blokován zámek na objektu actor |

### <a name="actor-state-management-events-and-performance-counters"></a>Události správy stavu objektu actor a čítače výkonu
Hello Reliable Actors runtime vysílá hello následující události související příliš[správy stavu objektu actor](service-fabric-reliable-actors-state-management.md).

| Název události | ID události | Úroveň | – Klíčové slovo | Popis |
| --- | --- | --- | --- | --- |
| ActorSaveStateStart |10 |Verbose |0x4 |Modul runtime aktéři je o stavu objektu actor toosave hello. |
| ActorSaveStateStop |11 |Verbose |0x4 |Modul runtime aktéři dokončil ukládání stavu objektu actor hello. |

modul runtime Reliable Actors Hello publikuje hello následující správy stavu související tooactor čítače výkonu.

| Název kategorie | Název čítače | Popis |
| --- | --- | --- |
| Prostředky infrastruktury služby objektu Actor |Průměrný počet milisekund na operaci uložení stavu |Doba trvání stavu toosave objektu actor v milisekundách |
| Prostředky infrastruktury služby objektu Actor |Průměrný počet milisekund na operaci načtení stavu |Doba trvání stavu tooload objektu actor v milisekundách |

### <a name="events-related-tooactor-replicas"></a>Události související tooactor repliky
Hello Reliable Actors runtime vysílá hello následující události související příliš[objektu actor repliky](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

| Název události | ID události | Úroveň | – Klíčové slovo | Popis |
| --- | --- | --- | --- | --- |
| ReplicaChangeRoleToPrimary |1 |Informační |0x1 |Repliky objektu actor změnit role tooPrimary. To znamená, že hello aktéři pro tento oddíl bude vytvořen uvnitř této repliky. |
| ReplicaChangeRoleFromPrimary |2 |Informační |0x1 |Repliky objektu actor změnit toonon primární roli. To znamená, že hello aktéři pro tento oddíl bude vytvořen již uvnitř této repliky. Žádné nové požadavky budou doručeny tooactors už vytvořený v rámci této repliky. Po dokončení všech žádostí probíhá budou Hello aktéři zničena. |

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Objektu actor aktivace a deaktivace události a čítače výkonu
Hello Reliable Actors runtime vysílá hello následující události související příliš[objektu actor aktivace a deaktivace](service-fabric-reliable-actors-lifecycle.md).

| Název události | ID události | Úroveň | – Klíčové slovo | Popis |
| --- | --- | --- | --- | --- |
| ActorActivated |5 |Informační |0x1 |Objekt actor byl aktivován. |
| ActorDeactivated |6 |Informační |0x1 |Objekt actor bylo deaktivováno. |

modul runtime Reliable Actors Hello publikuje hello následující čítače výkonu související s tooactor aktivace a deaktivace.

| Název kategorie | Název čítače | Popis |
| --- | --- | --- |
| Prostředky infrastruktury služby objektu Actor |Průměrný počet milisekund metody OnActivateAsync |Doba tooexecute provádění metody OnActivateAsync v milisekundách |

### <a name="actor-request-processing-performance-counters"></a>Čítače výkonu zpracování žádosti objektu actor
Když klient vyvolá metodu prostřednictvím objektu actor proxy, výsledkem zprávu požadavku odesílány prostřednictvím služby objektu actor toohello síťových hello. Služba Hello zpracuje zprávu požadavku hello a odešle odpověď zpět toohello klienta. modul runtime Reliable Actors Hello publikuje hello následující čítače výkonu, související tooactor zpracování žádosti.

| Název kategorie | Název čítače | Popis |
| --- | --- | --- |
| Prostředky infrastruktury služby objektu Actor |Počet zbývajících požadavků |Počet požadavků zpracovaných ve službě hello |
| Prostředky infrastruktury služby objektu Actor |Průměrný počet milisekund na žádost |Doba trvání (v milisekundách) pomocí služby tooprocess hello žádost |
| Prostředky infrastruktury služby objektu Actor |Průměrný počet milisekund deserializace požadavků |Čas (v milisekundách) toodeserialize objektu actor zprávu požadavku při obdržení u služby hello |
| Prostředky infrastruktury služby objektu Actor |Průměrný počet milisekund serializace odpovědí |Zpráva odpovědi čas přijatá (v milisekundách) tooserialize hello objektu actor v hello služby před odesláním odpovědi hello toohello klienta |

## <a name="next-steps"></a>Další kroky
* [Jak používat Reliable Actors hello platformy Service Fabric](service-fabric-reliable-actors-platform.md)
* [Referenční dokumentace rozhraní API objektu actor](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Ukázka kódu](https://github.com/Azure/servicefabric-samples)
* [Zprostředkovatelé EventSource v nástroje PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
