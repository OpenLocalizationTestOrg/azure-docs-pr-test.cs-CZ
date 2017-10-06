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
# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a><span data-ttu-id="477d5-103">Řešení běžných problémů s při nasazení služby v Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="477d5-103">Troubleshoot common issues when you deploy services on Azure Service Fabric</span></span>
<span data-ttu-id="477d5-104">Pokud používáte služby ve vašem počítači vývojáře, je snadné toouse [Visual Studio je nástroje pro ladění](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="477d5-104">When you're running services on your developer computer, it is easy toouse [Visual Studio's debugging tools](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span> <span data-ttu-id="477d5-105">Pro vzdálené clustery [sestav stavu](service-fabric-view-entities-aggregated-health.md) jsou vždy toostart vhodné místo.</span><span class="sxs-lookup"><span data-stu-id="477d5-105">For remote clusters, [health reports](service-fabric-view-entities-aggregated-health.md) are always a good place toostart.</span></span> <span data-ttu-id="477d5-106">Hello nejjednodušší tooaccess způsoby, jak jsou tyto sestavy pomocí Powershellu nebo [SFX](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="477d5-106">hello easiest ways tooaccess these reports are through PowerShell or [SFX](service-fabric-visualizing-your-cluster.md).</span></span> <span data-ttu-id="477d5-107">Tento článek předpokládá, že ladíte vzdálený cluster a základní znalosti o toouse jedna z těchto nástrojů.</span><span class="sxs-lookup"><span data-stu-id="477d5-107">This article assumes that you are debugging a remote cluster and have a basic understanding of how toouse either of these tools.</span></span>

## <a name="application-crash"></a><span data-ttu-id="477d5-108">Aplikace havárií</span><span class="sxs-lookup"><span data-stu-id="477d5-108">Application crash</span></span>
<span data-ttu-id="477d5-109">Hello "oddílu je nižší než počet replik nebo instancí cílové" sestava je dobrá indikace toho, která selhává služby.</span><span class="sxs-lookup"><span data-stu-id="477d5-109">hello "Partition is below target replica or instance count" report is a good indication that your service is crashing.</span></span> <span data-ttu-id="477d5-110">toofind se kdy selhává služby trvá trochu další šetření.</span><span class="sxs-lookup"><span data-stu-id="477d5-110">toofind out where your service is crashing takes a little more investigation.</span></span> <span data-ttu-id="477d5-111">Pokud vaše služba běží ve velkém měřítku, bude váš nejlepší přítel sadu dobře promyšlený trasování.</span><span class="sxs-lookup"><span data-stu-id="477d5-111">When your service is running at scale, your best friend will be a set of well-thought-out traces.</span></span>  <span data-ttu-id="477d5-112">Doporučujeme vyzkoušet [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) pro shromažďování těchto trasování a pomocí řešení, jako třeba [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) pro zobrazení a hledání hello trasování.</span><span class="sxs-lookup"><span data-stu-id="477d5-112">We suggest that you try [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) for collecting those traces and using a solution such as [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) for viewing and searching hello traces.</span></span>

![SFX oddílu stavu](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

### <a name="during-service-or-actor-initialization"></a><span data-ttu-id="477d5-114">Během inicializace služby nebo objektu actor</span><span class="sxs-lookup"><span data-stu-id="477d5-114">During service or actor initialization</span></span>
<span data-ttu-id="477d5-115">Jakékoli výjimky před inicializací typ služby hello způsobí, že proces toocrash hello.</span><span class="sxs-lookup"><span data-stu-id="477d5-115">Any exceptions before hello service type is initialized will cause hello process toocrash.</span></span> <span data-ttu-id="477d5-116">Pro tyto typy havárií hello protokolu událostí aplikace se zobrazí chyba hello od vaší služby.</span><span class="sxs-lookup"><span data-stu-id="477d5-116">For these types of crashes, hello application event log will show hello error from your service.</span></span>
<span data-ttu-id="477d5-117">Toto jsou hello nejběžnější výjimky toosee před inicializací hello služby.</span><span class="sxs-lookup"><span data-stu-id="477d5-117">These are hello most common exceptions toosee before hello service is initialized.</span></span>

<span data-ttu-id="477d5-118">***System.IO.FileNotFoundException***</span><span class="sxs-lookup"><span data-stu-id="477d5-118">***System.IO.FileNotFoundException***</span></span>

<span data-ttu-id="477d5-119">Tato chyba je často z důvodu závislosti toomissing sestavení.</span><span class="sxs-lookup"><span data-stu-id="477d5-119">This error is often due toomissing assembly dependencies.</span></span> <span data-ttu-id="477d5-120">Zkontrolujte vlastnost CopyLocal hello v sadě Visual Studio nebo hello globální mezipaměť sestavení pro uzel hello.</span><span class="sxs-lookup"><span data-stu-id="477d5-120">Check hello CopyLocal property in Visual Studio or hello global assembly cache for hello node.</span></span>

<span data-ttu-id="477d5-121">***System.Runtime.InteropServices.COMException*** *v System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*</span><span class="sxs-lookup"><span data-stu-id="477d5-121">***System.Runtime.InteropServices.COMException*** *at System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory(IntPtr, IFabricStatefulServiceFactory)*</span></span>

 <span data-ttu-id="477d5-122">To znamená, že název typu registrovanou službu hello neodpovídá hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="477d5-122">This indicates that hello registered service type name does not match hello service manifest.</span></span>

<span data-ttu-id="477d5-123">[Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) může být automaticky protokolu událostí aplikace hello tooupload nakonfigurované pro všechny uzly.</span><span class="sxs-lookup"><span data-stu-id="477d5-123">[Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) can be configured tooupload hello application event log for all your nodes automatically.</span></span>

### <a name="runasync-or-onactivateasync"></a><span data-ttu-id="477d5-124">RunAsync() nebo OnActivateAsync()</span><span class="sxs-lookup"><span data-stu-id="477d5-124">RunAsync() or OnActivateAsync()</span></span>
<span data-ttu-id="477d5-125">V případě havárie hello během inicializace hello nebo systémem typ registrované služby nebo objektu actor hello výjimky, zachytila Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="477d5-125">If hello crash happens during hello initialization or running of your registered service type or actor, hello exception will be caught by Azure Service Fabric.</span></span> <span data-ttu-id="477d5-126">Můžete zobrazit tyto od poskytovatelů EventSource hello popsané v části "Další kroky" hello.</span><span class="sxs-lookup"><span data-stu-id="477d5-126">You can view these from hello EventSource providers detailed in hello "Next steps" section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="477d5-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="477d5-127">Next steps</span></span>
<span data-ttu-id="477d5-128">Další informace o stávající Diagnostika poskytované Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="477d5-128">Learn more about existing diagnostics provided by Service Fabric:</span></span>

* [<span data-ttu-id="477d5-129">Spolehlivé aktéři diagnostiky</span><span class="sxs-lookup"><span data-stu-id="477d5-129">Reliable Actors diagnostics</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="477d5-130">Spolehlivé služby diagnostiky</span><span class="sxs-lookup"><span data-stu-id="477d5-130">Reliable Services diagnostics</span></span>](service-fabric-reliable-services-diagnostics.md)

