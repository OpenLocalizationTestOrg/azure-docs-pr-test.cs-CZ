---
title: "Azure Service Fabric výkonu monitorování | Microsoft Docs"
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
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: b19a2db85b2e1cc4c5f79f6b0dee97965f40ef88
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
---
# <a name="performance-metrics"></a>Metriky výkonu

Metriky by měl být shromažďovány pochopit výkon vašeho clusteru, jakož i aplikace běžící v ní. U clusterů Service Fabric doporučujeme shromažďování následující čítače výkonu.

## <a name="nodes"></a>Uzly

Pro počítače v clusteru vezměte v úvahu shromažďování následující čítače výkonu pro lepší pochopení zatížení na každém počítači a proveďte příslušné clusteru škálování rozhodnutí.

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
| Processor(Total) | % Času procesoru |
| Proces (pro službu) | % Času procesoru |
| Proces (pro službu) | ID procesu |
| Proces (pro službu) | Soukromé bajty |
| Proces (pro službu) | Počet vláken |
| Proces (pro službu) | Virtuální bajty |
| Proces (pro službu) | Pracovní sada |
| Proces (pro službu) | Pracovní sada – privátní |
| Interface(all-instances) sítě | Délka fronty výstupu |
| Interface(all-instances) sítě | Odchozí zrušených paketů |
| Interface(all-instances) sítě | Vyřazené přijaté pakety |
| Interface(all-instances) sítě | Chyby odchozích paketů |
| Interface(all-instances) sítě | Chyby přijaté pakety |

## <a name="net-applications-and-services"></a>Aplikace .NET a služby

Shromážděte následující čítače, pokud nasazujete služeb .NET do clusteru. 

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

Service Fabric generuje vyžadovat značné množství vlastní čítače výkonu. Pokud máte sadu SDK nainstalovat, v aplikaci sledování výkonu uvidíte kompletní seznam na počítač se systémem Windows (Start > Sledování výkonu). 

V aplikacích nasazujete do clusteru, pokud používáte Reliable Actors, přidejte countes z `Service Fabric Actor` a `Service Fabric Actor Method` kategorie (viz [Service Fabric spolehlivé aktéři diagnostiky](service-fabric-reliable-actors-diagnostics.md)).

Pokud používáte spolehlivé služby, podobně jako máme `Service Fabric Service` a `Service Fabric Service Method` kategorie čítače, které mají shromažďovat čítače z. 

Pokud používáte spolehlivé kolekcí, doporučujeme přidání `Avg. Transaction ms/Commit` z `Service Fabric Transactional Replicator` ke shromažďování latenci průměrná potvrzení za metrika transakce.


## <a name="next-steps"></a>Další kroky

* Další informace o [generování událostí na úrovni platformy](service-fabric-diagnostics-event-generation-infra.md) v Service Fabric
* Shromažďování metrik výkonu prostřednictvím [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)
