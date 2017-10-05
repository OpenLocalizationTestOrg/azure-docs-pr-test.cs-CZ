---
title: "Řešení potíží s trasování událostí | Microsoft Docs"
description: "Většina běžných potíží došlo při nasazení služeb v Microsoft Azure Service Fabric."
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
ms.openlocfilehash: e60bd4b9291cb2fc748921e42f11f54bb545984f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Řešení běžných problémů s při nasazení služby v Azure Service Fabric
Pokud používáte služby ve vašem počítači vývojáře, je snadno použitelný [Visual Studio je nástroje pro ladění](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Pro vzdálené clustery [sestav stavu](service-fabric-view-entities-aggregated-health.md) jsou vždy dobrým místem, kde spustit. Jsou nejjednodušších způsobů, jak dostat k sestavám pomocí prostředí PowerShell nebo [SFX](service-fabric-visualizing-your-cluster.md). Tento článek předpokládá, že ladíte vzdálený cluster a základní znalosti o tom, jak použít některou z těchto nástrojů.

## <a name="application-crash"></a>Aplikace havárií
"Oddílu je nižší než počet replik nebo instancí cílové" sestava je dobrá indikace toho, která selhává služby. Pokud chcete zjistit, kde služby selhává trvá trochu další šetření. Pokud vaše služba běží ve velkém měřítku, bude váš nejlepší přítel sadu dobře promyšlený trasování.  Doporučujeme vyzkoušet [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) pro shromažďování těchto trasování a pomocí řešení, jako třeba [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) pro zobrazení a hledání trasování.

![SFX oddílu stavu](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

### <a name="during-service-or-actor-initialization"></a>Během inicializace služby nebo objektu actor
Jakékoli výjimky před inicializací typ služby způsobí, že proces havárií. Pro tyto typy dojde k chybě v protokolu událostí aplikace se zobrazí chyba od vaší služby.
Jedná se o nejběžnější výjimky zobrazíte před inicializací službu.

***System.IO.FileNotFoundException***

Tato chyba je často z důvodu chybějící závislosti sestavení. Zkontrolujte vlastnost CopyLocal v sadě Visual Studio nebo do globální mezipaměti sestavení pro uzel.

***System.Runtime.InteropServices.COMException*** *v System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*

 To znamená, že název typu registrovanou službu neodpovídá service manifest.

[Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) lze nakonfigurovat pro automatické nahrání protokolu událostí aplikací pro všechny uzly.

### <a name="runasync-or-onactivateasync"></a>RunAsync() nebo OnActivateAsync()
V případě havárii během inicializace nebo systémem typ registrované služby nebo objektu actor výjimku, zachytila Azure Service Fabric. Můžete zobrazit tyto od poskytovatelů EventSource popsané v části "Další kroky".

## <a name="next-steps"></a>Další kroky
Další informace o stávající Diagnostika poskytované Service Fabric:

* [Spolehlivé aktéři diagnostiky](service-fabric-reliable-actors-diagnostics.md)
* [Spolehlivé služby diagnostiky](service-fabric-reliable-services-diagnostics.md)

