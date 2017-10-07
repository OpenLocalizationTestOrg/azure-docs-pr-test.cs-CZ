---
title: "Přehled modelu programování Fabric aaaService | Microsoft Docs"
description: "Service Fabric nabízí dvě rozhraní pro vytváření služeb: hello objektu actor framework a hello services framework. Nabízejí jedinečné kompromis v jednoduchost a řízení."
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
ms.date: 07/02/2017
ms.author: vturecek
ms.openlocfilehash: b48af2a7b41935bdf0e4594c765f363e520c254e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-programming-model-overview"></a>Přehled modelu programování Service Fabric
Service Fabric nabízí několik způsobů toowrite a spravovat vaše služby. Služby můžete zvolit toouse hello rozhraní API pro Service Fabric tootake všech výhod funkcí hello platformy a architektur aplikací. Služby mohou být také kompilované spustitelné programy vytvořené v libovolném jazyce nebo kód spuštěný v kontejneru jednoduše hostovaná v clusteru Service Fabric.

## <a name="guest-executables"></a>Spustitelné soubory hosta
A [spustitelný soubor hosta](service-fabric-deploy-existing-app.md) je existující, spustitelný libovolný soubor (napsané v libovolném jazyce), který lze spustit jako služby ve vaší aplikaci. Spustitelné soubory hosta nevolají přímo hello rozhraní API sady SDK služby prostředků infrastruktury. Ale přesto využívat výhod funkce hello platformy nabízí, třeba možnosti rozpoznání služby, vlastní stavu a zatížení reporting voláním rozhraní API REST vystavené Service Fabric. Mají také celou aplikaci životní cyklus podpory.

Začínáme s spustitelné soubory hosta nasazením první [spustitelná aplikace hosta](service-fabric-deploy-existing-app.md).

## <a name="containers"></a>Kontejnery
Ve výchozím nastavení Service Fabric nasadí a aktivuje služby jako procesy. Service Fabric lze také nasadit služby v [kontejnery](service-fabric-containers-overview.md). Service Fabric podporuje nasazení kontejnerů Linux a Windows kontejnery na Windows Server 2016. Kontejner Image je mohly vyžádat z jakékoli kontejneru úložiště a nasadit toohello počítače. Existující aplikace můžete nasadit jako exectuables hosta, Service Fabric bezstavové nebo stavová spolehlivé služby nebo Reliable Actors v kontejnerech a můžete kombinovat služeb v procesů a služeb v kontejnerech v hello stejná aplikace.

[Další informace o containerizing vaše služby v systému Windows nebo Linux](service-fabric-deploy-container.md)

## <a name="reliable-services"></a>Reliable Services
Spolehlivé služby je šedé – rozhraní pro zápis služby, které integrovat platformy Service Fabric hello a využívat hello úplnou sadu funkcí platformy. Spolehlivé služby poskytují minimální sadu rozhraní API, které umožňují životního cyklu hello Service Fabric runtime toomanage hello vašich služeb a které umožňují vaší služby toointeract s hello runtime. Architektura aplikace Hello je minimální, poskytuje úplné řízení celého možnosti návrhu a implementace a může být použité toohost žádné jiné aplikace framework, jako je ASP.NET Core.

Spolehlivé služby mohou být toomost bezstavové, podobně jako služba platformy, jako jsou třeba webové servery, ve kterých se vytvoří každou instanci služby hello rovna a externí řešení, jako je například databáze Azure nebo Azure Table Storage je uložen stav.

Spolehlivé služby může být také stavová, výhradní tooService prostředků infrastruktury, kde je uložen stav přímo v samotné pomocí spolehlivé kolekcí služby hello. Stav vytvoření vysoce dostupné prostřednictvím replikace a distribuovaných přes oddílů, všechny spravované automaticky pomocí Service Fabric.

[Další informace o službách Reliable Services](service-fabric-reliable-services-introduction.md) nebo začít [zápis vaší první službě spolehlivé](service-fabric-reliable-services-quick-start.md).

## <a name="reliable-actors"></a>Reliable Actors
Postavená na spolehlivé služby, je hello spolehlivé objektu Actor framework aplikační framework, který implementuje vzor virtuální objektu Actor hello, na základě vzoru návrhu objektu actor hello. framework spolehlivé objektu Actor Hello používá nezávislé jednotky výpočetních operací a stavu s jedním podprocesem provádění názvem aktéři. Hello spolehlivé objektu Actor framework poskytuje integrované komunikace pro předem nastavte stav konfigurace trvalosti a Škálováním na více systémů a aktéři.

Reliable Actors samotné je aplikační rozhraní založené na spolehlivé služby, jsou plně integrované s hello platformy Service Fabric a výhody z hello úplnou sadu funkcí, které nabízí platformou hello.

[Další informace o Reliable Actors](service-fabric-reliable-actors-introduction.md) nebo začít [zápis vaší první službě spolehlivé objektu Actor](service-fabric-reliable-actors-get-started.md)

## <a name="aspnet-core"></a>Jádro ASP.NET
Service Fabric se integruje s [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) pro vytváření webových a rozhraní API služeb, který může být součástí vaší aplikace. 

[Vytvoření front-endové služby pomocí ASP.NET Core](service-fabric-add-a-web-frontend.md)

## <a name="next-steps"></a>Další kroky
[Přehled Service Fabric a kontejnery](service-fabric-containers-overview.md)

[Přehled spolehlivé služby](service-fabric-reliable-services-introduction.md)

[Přehled spolehlivé služby](service-fabric-reliable-actors-introduction.md)

[Service Fabric a ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)




