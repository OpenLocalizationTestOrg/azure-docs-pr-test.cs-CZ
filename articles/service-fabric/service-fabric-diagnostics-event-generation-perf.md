---
title: "Monitorování výkonu služby infrastruktury aaaAzure | Microsoft Docs"
description: "Další informace o čítače výkonu pro monitorování a Diagnostika Azure Service Fabric clusterů."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 54d4c62b7250a1f70b0898ba07ae5a37716f4cf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-metrics"></a>Metriky výkonu

Metriky by měl být výkonu shromážděných toounderstand hello clusteru a také hello aplikace běžící v ní. U clusterů Service Fabric doporučujeme shromažďování hello následující čítače výkonu.

## <a name="nodes"></a>Uzly

Pro hello počítače v clusteru vezměte v úvahu shromažďování hello následující čítače výkonu toobetter pochopit hello zatížení na každém počítači a proveďte příslušné clusteru škálování rozhodnutí.

| Kategorie čítače | Název čítače |
| --- | --- |
| Fyzický disk (na disku) | Střední Délka fronty disku pro čtení |
| Fyzický disk (na disku) | Střední Délka fronty disku zápisu |
| Fyzický disk (na disku) | Střední Doba disku/čtení |
| Fyzický disk (na disku) | Střední Doba disku/zápis |
| Fyzický disk (na disku) | Čtení disku/s |
| Fyzický disk (na disku) | Čtení z disku bajtů/s |
| Fyzický disk (na disku) | Zápis disku/s |
| Fyzický disk (na disku) | Bajty zapisování na disk/s |
| Memory (Paměť) | Počet MB k dispozici |
| Soubor stránkování | % Využití |
| Process(Total) | % Času procesoru |
| Proces (pro službu) | % Času procesoru |
| Proces (pro službu) | ID procesu |
| Proces (pro službu) | Soukromé bajty |
| Proces (pro službu) | Počet vláken |
| Proces (pro službu) | Virtuální bajty |
| Proces (pro službu) | Pracovní sada |
| Proces (pro službu) | Pracovní sada – privátní |

## <a name="net-applications-and-services"></a>Aplikace .NET a služby

Shromažďujte následující čítače, pokud provádíte nasazení clusteru tooyour služby .NET hello. 

| Kategorie čítače | Název čítače |
| --- | --- |
| Využívání paměti rozhraním .NET CLR (pro službu) | ID procesu |
| Využívání paměti rozhraním .NET CLR (pro službu) | # Celkový počet potvrzených bajtů |
| Využívání paměti rozhraním .NET CLR (pro službu) | # Celkový počet vyhrazené bajtů |
| Využívání paměti rozhraním .NET CLR (pro službu) | Počet bajtů ve všech haldách |
| Využívání paměti rozhraním .NET CLR (pro službu) | Počet úklidů 0 |
| Využívání paměti rozhraním .NET CLR (pro službu) | # 1. generace kolekce |
| Využívání paměti rozhraním .NET CLR (pro službu) | # Úklidů 2. generace |
| Využívání paměti rozhraním .NET CLR (pro službu) | Čas |

### <a name="service-fabrics-custom-performance-counters"></a>Čítače výkonu vlastní Service Fabric

Service Fabric generuje vyžadovat značné množství vlastní čítače výkonu. Pokud máte hello SDK nainstalovat, v aplikaci sledování výkonu uvidíte kompletní seznam hello na počítač se systémem Windows (Start > Sledování výkonu). 

V aplikacích hello nasazujete tooyour clusteru, pokud používáte Reliable Actors, přidejte countes z `Service Fabric Actor` a `Service Fabric Actor Method` kategorie (viz [Service Fabric spolehlivé aktéři diagnostiky](service-fabric-reliable-actors-diagnostics.md)).

Pokud používáte spolehlivé služby, podobně jako máme `Service Fabric Service` a `Service Fabric Service Method` kategorie čítače, které mají shromažďovat čítače z. 

Pokud používáte spolehlivé kolekcí, doporučujeme přidání hello `Avg. Transaction ms/Commit` z hello `Service Fabric Transactional Replicator` toocollect hello průměrná potvrzení latence za metrika transakce.


## <a name="next-steps"></a>Další kroky

* Další informace o [generování událostí na úrovni platformy hello](service-fabric-diagnostics-event-generation-infra.md) v Service Fabric
* Shromažďování metrik výkonu prostřednictvím [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)
