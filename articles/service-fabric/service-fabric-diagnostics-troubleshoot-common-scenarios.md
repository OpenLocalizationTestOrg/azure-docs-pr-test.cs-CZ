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
# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a><span data-ttu-id="e4253-103">Řešení běžných problémů s při nasazení služby v Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="e4253-103">Troubleshoot common issues when you deploy services on Azure Service Fabric</span></span>
<span data-ttu-id="e4253-104">Pokud používáte služby ve vašem počítači vývojáře, je snadno použitelný [Visual Studio je nástroje pro ladění](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="e4253-104">When you're running services on your developer computer, it is easy to use [Visual Studio's debugging tools](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span> <span data-ttu-id="e4253-105">Pro vzdálené clustery [sestav stavu](service-fabric-view-entities-aggregated-health.md) jsou vždy dobrým místem, kde spustit.</span><span class="sxs-lookup"><span data-stu-id="e4253-105">For remote clusters, [health reports](service-fabric-view-entities-aggregated-health.md) are always a good place to start.</span></span> <span data-ttu-id="e4253-106">Jsou nejjednodušších způsobů, jak dostat k sestavám pomocí prostředí PowerShell nebo [SFX](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="e4253-106">The easiest ways to access these reports are through PowerShell or [SFX](service-fabric-visualizing-your-cluster.md).</span></span> <span data-ttu-id="e4253-107">Tento článek předpokládá, že ladíte vzdálený cluster a základní znalosti o tom, jak použít některou z těchto nástrojů.</span><span class="sxs-lookup"><span data-stu-id="e4253-107">This article assumes that you are debugging a remote cluster and have a basic understanding of how to use either of these tools.</span></span>

## <a name="application-crash"></a><span data-ttu-id="e4253-108">Aplikace havárií</span><span class="sxs-lookup"><span data-stu-id="e4253-108">Application crash</span></span>
<span data-ttu-id="e4253-109">"Oddílu je nižší než počet replik nebo instancí cílové" sestava je dobrá indikace toho, která selhává služby.</span><span class="sxs-lookup"><span data-stu-id="e4253-109">The "Partition is below target replica or instance count" report is a good indication that your service is crashing.</span></span> <span data-ttu-id="e4253-110">Pokud chcete zjistit, kde služby selhává trvá trochu další šetření.</span><span class="sxs-lookup"><span data-stu-id="e4253-110">To find out where your service is crashing takes a little more investigation.</span></span> <span data-ttu-id="e4253-111">Pokud vaše služba běží ve velkém měřítku, bude váš nejlepší přítel sadu dobře promyšlený trasování.</span><span class="sxs-lookup"><span data-stu-id="e4253-111">When your service is running at scale, your best friend will be a set of well-thought-out traces.</span></span>  <span data-ttu-id="e4253-112">Doporučujeme vyzkoušet [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) pro shromažďování těchto trasování a pomocí řešení, jako třeba [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) pro zobrazení a hledání trasování.</span><span class="sxs-lookup"><span data-stu-id="e4253-112">We suggest that you try [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) for collecting those traces and using a solution such as [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) for viewing and searching the traces.</span></span>

![SFX oddílu stavu](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

### <a name="during-service-or-actor-initialization"></a><span data-ttu-id="e4253-114">Během inicializace služby nebo objektu actor</span><span class="sxs-lookup"><span data-stu-id="e4253-114">During service or actor initialization</span></span>
<span data-ttu-id="e4253-115">Jakékoli výjimky před inicializací typ služby způsobí, že proces havárií.</span><span class="sxs-lookup"><span data-stu-id="e4253-115">Any exceptions before the service type is initialized will cause the process to crash.</span></span> <span data-ttu-id="e4253-116">Pro tyto typy dojde k chybě v protokolu událostí aplikace se zobrazí chyba od vaší služby.</span><span class="sxs-lookup"><span data-stu-id="e4253-116">For these types of crashes, the application event log will show the error from your service.</span></span>
<span data-ttu-id="e4253-117">Jedná se o nejběžnější výjimky zobrazíte před inicializací službu.</span><span class="sxs-lookup"><span data-stu-id="e4253-117">These are the most common exceptions to see before the service is initialized.</span></span>

<span data-ttu-id="e4253-118">***System.IO.FileNotFoundException***</span><span class="sxs-lookup"><span data-stu-id="e4253-118">***System.IO.FileNotFoundException***</span></span>

<span data-ttu-id="e4253-119">Tato chyba je často z důvodu chybějící závislosti sestavení.</span><span class="sxs-lookup"><span data-stu-id="e4253-119">This error is often due to missing assembly dependencies.</span></span> <span data-ttu-id="e4253-120">Zkontrolujte vlastnost CopyLocal v sadě Visual Studio nebo do globální mezipaměti sestavení pro uzel.</span><span class="sxs-lookup"><span data-stu-id="e4253-120">Check the CopyLocal property in Visual Studio or the global assembly cache for the node.</span></span>

<span data-ttu-id="e4253-121">***System.Runtime.InteropServices.COMException*** *v System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*</span><span class="sxs-lookup"><span data-stu-id="e4253-121">***System.Runtime.InteropServices.COMException*** *at System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory(IntPtr, IFabricStatefulServiceFactory)*</span></span>

 <span data-ttu-id="e4253-122">To znamená, že název typu registrovanou službu neodpovídá service manifest.</span><span class="sxs-lookup"><span data-stu-id="e4253-122">This indicates that the registered service type name does not match the service manifest.</span></span>

<span data-ttu-id="e4253-123">[Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) lze nakonfigurovat pro automatické nahrání protokolu událostí aplikací pro všechny uzly.</span><span class="sxs-lookup"><span data-stu-id="e4253-123">[Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) can be configured to upload the application event log for all your nodes automatically.</span></span>

### <a name="runasync-or-onactivateasync"></a><span data-ttu-id="e4253-124">RunAsync() nebo OnActivateAsync()</span><span class="sxs-lookup"><span data-stu-id="e4253-124">RunAsync() or OnActivateAsync()</span></span>
<span data-ttu-id="e4253-125">V případě havárii během inicializace nebo systémem typ registrované služby nebo objektu actor výjimku, zachytila Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="e4253-125">If the crash happens during the initialization or running of your registered service type or actor, the exception will be caught by Azure Service Fabric.</span></span> <span data-ttu-id="e4253-126">Můžete zobrazit tyto od poskytovatelů EventSource popsané v části "Další kroky".</span><span class="sxs-lookup"><span data-stu-id="e4253-126">You can view these from the EventSource providers detailed in the "Next steps" section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4253-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e4253-127">Next steps</span></span>
<span data-ttu-id="e4253-128">Další informace o stávající Diagnostika poskytované Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="e4253-128">Learn more about existing diagnostics provided by Service Fabric:</span></span>

* [<span data-ttu-id="e4253-129">Spolehlivé aktéři diagnostiky</span><span class="sxs-lookup"><span data-stu-id="e4253-129">Reliable Actors diagnostics</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="e4253-130">Spolehlivé služby diagnostiky</span><span class="sxs-lookup"><span data-stu-id="e4253-130">Reliable Services diagnostics</span></span>](service-fabric-reliable-services-diagnostics.md)

