---
title: "aaaTroubleshooting s trasování událostí | Microsoft Docs"
description: "Hello nejběžnějších problémů při nasazování služeb v Microsoft Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: mattrowmsft
manager: timlt
editor: 
ms.assetid: 5eb8ef21-da04-4ac8-8b9a-5f7ff1e0a180
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/31/2016
ms.author: mattrow
redirect_url: /azure/service-fabric/service-fabric-diagnostics-overview
ms.openlocfilehash: f5adb7e15fa1e2c964bbbc5726246630c95e13f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Řešení běžných problémů s při nasazení služby v Azure Service Fabric
Pokud používáte služby ve vašem počítači vývojáře, je snadné toouse [Visual Studio je nástroje pro ladění](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Pro vzdálené clustery [sestav stavu](service-fabric-view-entities-aggregated-health.md) jsou vždy toostart vhodné místo. Hello nejjednodušší tooaccess způsoby, jak jsou tyto sestavy pomocí Powershellu nebo [SFX](service-fabric-visualizing-your-cluster.md). Tento článek předpokládá, že ladíte vzdálený cluster a základní znalosti o toouse jedna z těchto nástrojů.

## <a name="application-crash"></a>Aplikace havárií
Hello "oddílu je nižší než počet replik nebo instancí cílové" sestava je dobrá indikace toho, která selhává služby. toofind se kdy selhává služby trvá trochu další šetření. Pokud vaše služba běží ve velkém měřítku, bude váš nejlepší přítel sadu dobře promyšlený trasování.  Doporučujeme vyzkoušet [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) pro shromažďování těchto trasování a pomocí řešení, jako třeba [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) pro zobrazení a hledání hello trasování.

![SFX oddílu stavu](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

### <a name="during-service-or-actor-initialization"></a>Během inicializace služby nebo objektu actor
Jakékoli výjimky před inicializací typ služby hello způsobí, že proces toocrash hello. Pro tyto typy havárií hello protokolu událostí aplikace se zobrazí chyba hello od vaší služby.
Toto jsou hello nejběžnější výjimky toosee před inicializací hello služby.

***System.IO.FileNotFoundException***

Tato chyba je často z důvodu závislosti toomissing sestavení. Zkontrolujte vlastnost CopyLocal hello v sadě Visual Studio nebo hello globální mezipaměť sestavení pro uzel hello.

***System.Runtime.InteropServices.COMException*** *v System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*

 To znamená, že název typu registrovanou službu hello neodpovídá hello service manifest.

[Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) může být automaticky protokolu událostí aplikace hello tooupload nakonfigurované pro všechny uzly.

### <a name="runasync-or-onactivateasync"></a>RunAsync() nebo OnActivateAsync()
V případě havárie hello během inicializace hello nebo systémem typ registrované služby nebo objektu actor hello výjimky, zachytila Azure Service Fabric. Můžete zobrazit tyto od poskytovatelů EventSource hello popsané v části "Další kroky" hello.

## <a name="next-steps"></a>Další kroky
Další informace o stávající Diagnostika poskytované Service Fabric:

* [Spolehlivé aktéři diagnostiky](service-fabric-reliable-actors-diagnostics.md)
* [Spolehlivé služby diagnostiky](service-fabric-reliable-services-diagnostics.md)

