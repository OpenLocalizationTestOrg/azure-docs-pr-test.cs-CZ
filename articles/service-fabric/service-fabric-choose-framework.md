---
title: "Přehled modelu programování Service Fabric | Microsoft Docs"
description: "Service Fabric nabízí dvě rozhraní pro vytváření služeb: rozhraní objektu actor a rozhraní služby. Nabízejí jedinečné kompromis v jednoduchost a řízení."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: vturecek
ms.assetid: 974b2614-014e-4587-a947-28fcef28b382
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 237b8396b56fdec86cc005c121646556825d8e98
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="service-fabric-programming-model-overview"></a>Přehled modelu programování Service Fabric
Service Fabric nabízí několik způsobů, jak zapsat a spravovat vaše služby. Služby můžete plně využít výhod funkcí a architektur aplikací platformu pomocí rozhraní API služby prostředků infrastruktury. Služby mohou být také kompilované spustitelné programy vytvořené v libovolném jazyce nebo kód spuštěný v kontejneru jednoduše hostovaná v clusteru Service Fabric.

## <a name="guest-executables"></a>Spustitelné soubory hosta
A [spustitelný soubor hosta](service-fabric-deploy-existing-app.md) je existující, spustitelný libovolný soubor (napsané v libovolném jazyce), který lze spustit jako služby ve vaší aplikaci. Spustitelné soubory hosta nevolají přímo rozhraní API sady SDK služby prostředků infrastruktury. Ale budou i nadále využívat funkce nabízí platformu, jako je například služba možností rozpoznání se vlastní stavu a spouštění sestav pomocí volání rozhraní REST API vystavené Service Fabric. Mají také celou aplikaci životní cyklus podpory.

Začínáme s spustitelné soubory hosta nasazením první [spustitelná aplikace hosta](service-fabric-deploy-existing-app.md).

## <a name="containers"></a>Kontejnery
Ve výchozím nastavení Service Fabric nasadí a aktivuje služby jako procesy. Service Fabric lze také nasadit služby v [kontejnery](service-fabric-containers-overview.md). Service Fabric podporuje nasazení kontejnerů Linux a Windows kontejnery na Windows Server 2016. Kontejner imagí můžete vyžádat z jakékoli kontejneru úložiště a nasazené na tento počítač. Existující aplikace můžete nasadit jako exectuables hosta, Service Fabric bezstavové nebo stavová spolehlivé služeb nebo Reliable Actors v kontejnerech a je možné kombinovat služby v procesy a služby v kontejnerech ve stejné aplikaci.

[Další informace o containerizing vaše služby v systému Windows nebo Linux](service-fabric-deploy-container.md)

## <a name="reliable-services"></a>Reliable Services
Spolehlivé služby je šedé – rozhraní pro zápis služby, které integrovat platformy Service Fabric a využívat úplnou sadu funkcí platformy. Spolehlivé služby poskytují minimální sadu rozhraní API, které umožňují spravovat životní cyklus vaše služby modulu runtime Service Fabric a které umožňují vaší služby pro interakci s modulem runtime. Rozhraní je minimální, poskytuje úplnou kontrolu nad možnosti návrhu a implementace a může sloužit k hostování žádné jiné aplikace framework, jako je ASP.NET Core.

Spolehlivé služby mohou být bezstavové, podobně jako na většině platforem služby, jako jsou třeba webové servery, ve kterých každou instanci služby se vytvoří rovna a externí řešení, jako je například databáze Azure nebo Azure Table Storage je uložen stav.

Spolehlivé služby může být také stavová, výhradně pro Service Fabric, kde je uložen stav přímo v samotné pomocí spolehlivé kolekcí služby. Stav vytvoření vysoce dostupné prostřednictvím replikace a distribuovaných přes oddílů, všechny spravované automaticky pomocí Service Fabric.

[Další informace o službách Reliable Services](service-fabric-reliable-services-introduction.md) nebo začít [zápis vaší první službě spolehlivé](service-fabric-reliable-services-quick-start.md).

## <a name="aspnet-core"></a>Jádro ASP.NET
ASP.NET Core je nové open source a napříč platformami architektura pro vytváření moderní cloudové aplikace založené na připojené k Internetu, jako třeba webové aplikace, aplikace IoT a back-EndY mobilních. Service Fabric se integruje s ASP.NET Core, můžete napsat bezzstavovými i stavovými ASP.NET Core aplikací využívajících spolehlivé kolekcí a Service Fabric orchestration pokročilé možnosti.

[Další informace o ASP.NET Core v Service Fabric](service-fabric-reliable-services-communication-aspnetcore.md) nebo začít [zápis vaší první aplikace ASP.NET Core Service Fabric](service-fabric-add-a-web-frontend.md).

## <a name="reliable-actors"></a>Reliable Actors
Rozhraní framework spolehlivé objektu Actor postavená na spolehlivé služby, je aplikační framework, který implementuje vzor virtuální objektu Actor, na základě vzoru návrhu objektu actor. Rozhraní objektu Actor spolehlivé používá nezávislé jednotky výpočetních operací a stavu s jedním podprocesem provádění názvem aktéři. Rozhraní framework spolehlivé objektu Actor poskytuje integrované komunikace pro předem nastavte stav konfigurace trvalosti a Škálováním na více systémů a aktéři.

Reliable Actors samotné je aplikační rozhraní založené na spolehlivé služby, jsou plně integrované s platformy Service Fabric a výhody v úplné sadě funkcí, které nabízí platformou.

[Další informace o Reliable Actors](service-fabric-reliable-actors-introduction.md) nebo začít [zápis vaší první službě spolehlivé objektu Actor](service-fabric-reliable-actors-get-started.md)


[Vytvoření front-endové služby pomocí ASP.NET Core](service-fabric-add-a-web-frontend.md)

## <a name="next-steps"></a>Další kroky
[Přehled Service Fabric a kontejnery](service-fabric-containers-overview.md)

[Přehled spolehlivé služby](service-fabric-reliable-services-introduction.md)

[Spolehlivé aktéři – přehled](service-fabric-reliable-actors-introduction.md)

[Service Fabric a ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)




