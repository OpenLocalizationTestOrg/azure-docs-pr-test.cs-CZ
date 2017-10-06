---
title: "aaaStateful diagnostiky spolehlivé služby | Microsoft Docs"
description: "Diagnostické funkce pro stavové služby Reliable Services"
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 6200800b858957c06039d9af062633b12a446318
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnostické funkce pro stavové služby Reliable Services
vysílá Hello třída Stateful spolehlivé služby StatefulServiceBase [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) události, které se dají použít toodebug hello služby, poskytují přehled o tom, jak je operační hello runtime a pomáhají při řešení problémů.

## <a name="eventsource-events"></a>EventSource události
Název EventSource Hello hello Stateful spolehlivé služby StatefulServiceBase třída je "Služba společnosti Microsoft ServiceFabric". Události z tohoto zdroje událostí se zobrazí ve [diagnostiky události](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) okno při hello služby se [ladit v sadě Visual Studio](service-fabric-debugging-your-application.md).

Příklady nástroje a technologie, které pomáhají v shromažďování nebo zobrazování událostí EventSource [nástroje PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Microsoft Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md)a [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Události
| Název události | ID události | Úroveň | Popis události |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Informační |Když se služba RunAsync úloha je spuštěna. |
| StatefulRunAsyncCancellation |2 |Informační |Když se služba RunAsync úloha byla zrušena |
| StatefulRunAsyncCompletion |3 |Informační |Když se po dokončení úkolu RunAsync služby |
| StatefulRunAsyncSlowCancellation |4 |Upozornění |Když se úloha RunAsync služby trvá příliš dlouho toocomplete zrušení |
| StatefulRunAsyncFailure |5 |Chyba |Když se vyvolá výjimku, úloha RunAsync služby |

## <a name="interpret-events"></a>Interpretace události
StatefulRunAsyncInvocation, StatefulRunAsyncCompletion a StatefulRunAsyncCancellation události jsou užitečné toohello služby zapisovače toounderstand hello životní cyklus služby--, jakož i hello časování pro při zahájení, zrušena nebo byla dokončena služby . To může být užitečné při ladění problémů služby nebo principy hello služby životního cyklu.

Služba zapisovače měli věnovat zvýšené pozornosti tooStatefulRunAsyncSlowCancellation a StatefulRunAsyncFailure události, protože indikují problémy se službou hello.

StatefulRunAsyncFailure jsou vydávány vždy, když úloha RunAsync() služby hello vyvolá výjimku. Výjimka vyvolaná obvykle označuje chyby nebo chyby ve službě hello. Kromě toho hello výjimka způsobí, že služba toofail hello, tak, aby byl přesunutý tooa jiný uzel. To může být náročná operace a můžou zdržet příchozí požadavky během přesouvání služby hello. Služba zapisovače zjistěte hello příčinou hello výjimky a, pokud je to možné, ji zmírnit.

StatefulRunAsyncSlowCancellation jsou vydávány vždy, když na žádost o zrušení úlohy RunAsync hello trvá déle než 4 sekundy. Pokud služby trvá příliš dlouho toocomplete zrušení, ovlivní schopnost hello toobe služby hello rychle restartována na jiný uzel. To může mít vliv na celkovou dostupnost služby hello hello.

## <a name="next-steps"></a>Další kroky
* [Zprostředkovatelé EventSource v nástroje PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
