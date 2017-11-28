---
title: "aaaScheduled události Metadata službou Azure | Microsoft Docs"
description: "Předtím, než se stát, reagovat tooImpactful události na virtuálním počítači."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/10/2016
ms.author: zivr
ms.openlocfilehash: b5c0849958c3ab48fa9c2cbff7db62f2e9d7daf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-metadata-service---scheduled-events-preview"></a><span data-ttu-id="96bc2-103">Služba Azure Metadata - naplánované události (Preview)</span><span class="sxs-lookup"><span data-stu-id="96bc2-103">Azure Metadata Service - Scheduled Events (Preview)</span></span>

> [!NOTE] 
> <span data-ttu-id="96bc2-104">Verze Preview, jsou k dispozici tooyou probíhají hello podmínky, že souhlasíte toohello podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="96bc2-104">Previews are made available tooyou on hello condition that you agree toohello terms of use.</span></span> <span data-ttu-id="96bc2-105">Další informace najdete v [dodatečných podmínkách použití systémů Microsoft Azure Preview](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/).</span><span class="sxs-lookup"><span data-stu-id="96bc2-105">For more information, see [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/).</span></span>
>

<span data-ttu-id="96bc2-106">Naplánované události je jedním z subservices hello pod hello Metadata služby Azure.</span><span class="sxs-lookup"><span data-stu-id="96bc2-106">Scheduled Events is one of hello subservices under hello Azure Metadata Service.</span></span> <span data-ttu-id="96bc2-107">Zodpovídá za zpřístupnění informací o nadcházející události (například restartování) tak, aby vaše aplikace můžete připravit pro ně a omezit přerušení.</span><span class="sxs-lookup"><span data-stu-id="96bc2-107">It is responsible for surfacing information regarding upcoming events (for example, reboot) so your application can prepare for them and limit disruption.</span></span> <span data-ttu-id="96bc2-108">Je k dispozici pro všechny typy virtuálního počítače Azure, včetně PaaS a IaaS.</span><span class="sxs-lookup"><span data-stu-id="96bc2-108">It is available for all Azure Virtual Machine types including PaaS and IaaS.</span></span> <span data-ttu-id="96bc2-109">Naplánované události dává preventivní úlohami virtuálního počítače čas tooperform toominimize hello účinku událost.</span><span class="sxs-lookup"><span data-stu-id="96bc2-109">Scheduled Events gives your Virtual Machine time tooperform preventive tasks toominimize hello effect of an event.</span></span> 

## <a name="introduction---why-scheduled-events"></a><span data-ttu-id="96bc2-110">Úvod - proč naplánované události?</span><span class="sxs-lookup"><span data-stu-id="96bc2-110">Introduction - Why Scheduled Events?</span></span>

<span data-ttu-id="96bc2-111">Naplánované události je můžete provést kroky toolimit hello dopad platformy intiated údržby nebo akce zahájená uživatelem vaší služby.</span><span class="sxs-lookup"><span data-stu-id="96bc2-111">With Scheduled Events, you can take steps toolimit hello impact of platform-intiated maintenance or user-initiated actions on your service.</span></span> 

<span data-ttu-id="96bc2-112">S více instancemi úloh, které použití replikace techniky toomaintain stavu, může být ohrožena toooutages děje ve více instancích.</span><span class="sxs-lookup"><span data-stu-id="96bc2-112">Multi-instance workloads, which use replication techniques toomaintain state, may be vulnerable toooutages happening across multiple instances.</span></span> <span data-ttu-id="96bc2-113">Například výpadky může vést k nákladné úlohy (například rekonstrukci indexy) nebo i ke ztrátě replik.</span><span class="sxs-lookup"><span data-stu-id="96bc2-113">Such outages may result in expensive tasks (for example, rebuilding indexes) or even a replica loss.</span></span> 

<span data-ttu-id="96bc2-114">V mnoha jiných případech hello celkové dostupnosti služby, je možné zvýšit provedením řádné vypnutí pořadí dokončuje (nebo ruší) během letu transakce, přeřazení tooother úlohy virtuálních počítačů v clusteru hello (ruční převzetí služeb při selhání), nebo odebrání hello Virtuální počítač z fondu vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="96bc2-114">In many other cases, hello overall service availability may be improved by performing a graceful shutdown sequence such as completing (or canceling) in-flight transactions, reassigning tasks tooother VMs in hello cluster (manual failover), or removing hello Virtual Machine from a network load balancer pool.</span></span> 

<span data-ttu-id="96bc2-115">Existují případy, kdy se žádat o pomoc správce o nadcházející události nebo protokolování takové události pomoci, vylepšení hello použitelnost aplikací hostovaných v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="96bc2-115">There are cases where notifying an administrator about an upcoming event or logging such an event help improving hello serviceability of applications hosted in hello cloud.</span></span>

<span data-ttu-id="96bc2-116">Případy použití Azure Metadata povrchy naplánované události služby v hello následující:</span><span class="sxs-lookup"><span data-stu-id="96bc2-116">Azure Metadata Service surfaces Scheduled Events in hello following use cases:</span></span>
-   <span data-ttu-id="96bc2-117">Platforma iniciované údržby (například zavádění hostitelským operačním systémem)</span><span class="sxs-lookup"><span data-stu-id="96bc2-117">Platform initiated maintenance (for example, Host OS rollout)</span></span>
-   <span data-ttu-id="96bc2-118">Uživatel spustil volání (například restartování uživatele nebo opětovně nasadí virtuální počítač)</span><span class="sxs-lookup"><span data-stu-id="96bc2-118">User-initiated calls (for example, user restarts or redeploys a VM)</span></span>


## <a name="scheduled-events---hello-basics"></a><span data-ttu-id="96bc2-119">Naplánované události - hello základy</span><span class="sxs-lookup"><span data-stu-id="96bc2-119">Scheduled Events - hello Basics</span></span>  

<span data-ttu-id="96bc2-120">Služba Azure Metadata zpřístupní informace o spouštění virtuálních počítačů pomocí koncový bod REST, která je přístupná z v rámci hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="96bc2-120">Azure Metadata service exposes information about running Virtual Machines using a REST Endpoint accessible from within hello VM.</span></span> <span data-ttu-id="96bc2-121">Hello informace jsou k dispozici prostřednictvím směrovat IP tak, aby není nezveřejní hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="96bc2-121">hello information is available via a non-routable IP so that it is not exposed outside hello VM.</span></span>

### <a name="scope"></a><span data-ttu-id="96bc2-122">Rozsah</span><span class="sxs-lookup"><span data-stu-id="96bc2-122">Scope</span></span>
<span data-ttu-id="96bc2-123">Naplánované události jsou prezentované tooall virtuální počítače v cloudové službě nebo tooall virtuální počítače ve skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="96bc2-123">Scheduled events are surfaced tooall Virtual Machines in a cloud service or tooall Virtual Machines in an Availability Set.</span></span> <span data-ttu-id="96bc2-124">V důsledku toho byste měli zkontrolovat hello `Resources` pole tooidentify hello událostí, které virtuální počítače se bude dopad na toobe.</span><span class="sxs-lookup"><span data-stu-id="96bc2-124">As a result, you should check hello `Resources` field in hello event tooidentify which VMs are going toobe impacted.</span></span> 

### <a name="discovering-hello-endpoint"></a><span data-ttu-id="96bc2-125">Zjišťování hello koncový bod</span><span class="sxs-lookup"><span data-stu-id="96bc2-125">Discovering hello Endpoint</span></span>
<span data-ttu-id="96bc2-126">V případě hello, kde se má vytvořit virtuální počítač v rámci virtuální sítě (VNet), je k dispozici ze statické IP adresy směrovat, služba metadat hello `169.254.169.254`.</span><span class="sxs-lookup"><span data-stu-id="96bc2-126">In hello case where a Virtual Machine is created within a Virtual Network (VNet), hello metadata service is available from a static non-routable IP, `169.254.169.254`.</span></span>
<span data-ttu-id="96bc2-127">Pokud není vytvořená hello virtuálního počítače v rámci virtuální sítě, hello výchozí případů pro cloudové služby a klasické virtuální počítače, je další logiku toouse koncového bodu vyžaduje toodiscover hello.</span><span class="sxs-lookup"><span data-stu-id="96bc2-127">If hello Virtual Machine is not created within a Virtual Network, hello default cases for cloud services and classic VMs, additional logic is required toodiscover hello endpoint toouse.</span></span> <span data-ttu-id="96bc2-128">Jak příliš najdete ukázkové toolearn toothis[zjistit koncový bod hostitele hello](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span><span class="sxs-lookup"><span data-stu-id="96bc2-128">Refer toothis sample toolearn how too[discover hello host endpoint](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span></span>

### <a name="versioning"></a><span data-ttu-id="96bc2-129">Správa verzí</span><span class="sxs-lookup"><span data-stu-id="96bc2-129">Versioning</span></span> 
<span data-ttu-id="96bc2-130">Hello Instance služby metadat je verzí.</span><span class="sxs-lookup"><span data-stu-id="96bc2-130">hello Instance Metadata Service is versioned.</span></span> <span data-ttu-id="96bc2-131">Verze jsou povinné a hello aktuální verze je `2017-03-01`.</span><span class="sxs-lookup"><span data-stu-id="96bc2-131">Versions are mandatory and hello current version is `2017-03-01`.</span></span>

> [!NOTE] 
> <span data-ttu-id="96bc2-132">Předchozí verze preview naplánované událostí podporovaných {nejnovější} jako hello api-version.</span><span class="sxs-lookup"><span data-stu-id="96bc2-132">Previous preview releases of scheduled events supported {latest} as hello api-version.</span></span> <span data-ttu-id="96bc2-133">Tento formát se už nepodporuje a bude v budoucí hello nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="96bc2-133">This format is no longer supported and will be deprecated in hello future.</span></span>

### <a name="using-headers"></a><span data-ttu-id="96bc2-134">Používání hlaviček</span><span class="sxs-lookup"><span data-stu-id="96bc2-134">Using Headers</span></span>
<span data-ttu-id="96bc2-135">Když dotazujete hello Metadata služby, je nutné zadat hello záhlaví `Metadata: true` tooensure hello požadavek nebyl přesměrován náhodně.</span><span class="sxs-lookup"><span data-stu-id="96bc2-135">When you query hello Metadata Service, you must provide hello header `Metadata: true` tooensure hello request was not unintentionally redirected.</span></span>

### <a name="enabling-scheduled-events"></a><span data-ttu-id="96bc2-136">Povolení naplánované události</span><span class="sxs-lookup"><span data-stu-id="96bc2-136">Enabling Scheduled Events</span></span>
<span data-ttu-id="96bc2-137">Hello poprvé, co musíte provést žádost pro naplánované události Azure implicitně povolí hello funkci na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="96bc2-137">hello first time you make a request for scheduled events, Azure implicitly enables hello feature on your Virtual Machine.</span></span> <span data-ttu-id="96bc2-138">V důsledku toho byste měli očekávat zpožděné odpovědi v prvním volání z až tootwo minut.</span><span class="sxs-lookup"><span data-stu-id="96bc2-138">As a result, you should expect a delayed response in your first call of up tootwo minutes.</span></span>

### <a name="user-initiated-maintenance"></a><span data-ttu-id="96bc2-139">Uživatel spustil údržby</span><span class="sxs-lookup"><span data-stu-id="96bc2-139">User Initiated Maintenance</span></span>
<span data-ttu-id="96bc2-140">Uživatel spustil údržby virtuálního počítače prostřednictvím hello portál Azure, rozhraní API, rozhraní příkazového řádku, nebo prostředí PowerShell bude mít za následek naplánované události.</span><span class="sxs-lookup"><span data-stu-id="96bc2-140">User initiated virtual machine maintenance via hello Azure portal, API, CLI, or PowerShell will result in Scheduled Events.</span></span> <span data-ttu-id="96bc2-141">To vám umožní tootest hello údržby přípravy logiku v aplikaci a umožňuje tooprepare vaší aplikace pro údržbu inicializované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="96bc2-141">This allows you tootest hello maintenance preparation logic in your application and allows your application tooprepare for user initiated maintenance.</span></span>

<span data-ttu-id="96bc2-142">Restartování virtuálního počítače se naplánuje událost s typem `Reboot`.</span><span class="sxs-lookup"><span data-stu-id="96bc2-142">Restarting a virtual machine will schedule an event with type `Reboot`.</span></span> <span data-ttu-id="96bc2-143">Opětovné nasazení virtuálního počítače se naplánuje událost s typem `Redeploy`.</span><span class="sxs-lookup"><span data-stu-id="96bc2-143">Redeploying a virtual machine will schedule an event with type `Redeploy`.</span></span>

> [!NOTE] 
> <span data-ttu-id="96bc2-144">Aktuálně může být současně naplánována maximálně 10 operací údržby inicializované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="96bc2-144">Currently a maximum of 10 user initiated maintenance operations can be simultaneously scheduled.</span></span> <span data-ttu-id="96bc2-145">Tento limit bude zmírnit před naplánované události obecné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="96bc2-145">This limit will be relaxed before Scheduled Events General Availability.</span></span>

> [!NOTE] 
> <span data-ttu-id="96bc2-146">Údržby iniciované uživatelem, což vede k naplánované události se momentálně nedá konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="96bc2-146">Currently user initiated maintenance resulting in Scheduled Events is not configurable.</span></span> <span data-ttu-id="96bc2-147">Možnosti konfigurace: je plánovaná pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="96bc2-147">Configurability is planned for a future release.</span></span>

## <a name="using-hello-api"></a><span data-ttu-id="96bc2-148">Pomocí rozhraní API hello</span><span class="sxs-lookup"><span data-stu-id="96bc2-148">Using hello API</span></span>

### <a name="query-for-events"></a><span data-ttu-id="96bc2-149">Dotaz pro události</span><span class="sxs-lookup"><span data-stu-id="96bc2-149">Query for events</span></span>
<span data-ttu-id="96bc2-150">Pro naplánované události můžete dotazovat jednoduše tak, že hello následující volání:</span><span class="sxs-lookup"><span data-stu-id="96bc2-150">You can query for Scheduled Events simply by making hello following call:</span></span>

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

<span data-ttu-id="96bc2-151">Odpověď obsahuje pole naplánované události.</span><span class="sxs-lookup"><span data-stu-id="96bc2-151">A response contains an array of scheduled events.</span></span> <span data-ttu-id="96bc2-152">Prázdné pole znamená, že aktuálně neexistují žádné události naplánované.</span><span class="sxs-lookup"><span data-stu-id="96bc2-152">An empty array means that there are currently no events scheduled.</span></span>
<span data-ttu-id="96bc2-153">V případě hello tam, kde jsou naplánované události, hello odpovědi obsahuje řadu událostí:</span><span class="sxs-lookup"><span data-stu-id="96bc2-153">In hello case where there are scheduled events, hello response contains an array of events:</span></span> 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a><span data-ttu-id="96bc2-154">Vlastnosti události</span><span class="sxs-lookup"><span data-stu-id="96bc2-154">Event Properties</span></span>
|<span data-ttu-id="96bc2-155">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="96bc2-155">Property</span></span>  |  <span data-ttu-id="96bc2-156">Popis</span><span class="sxs-lookup"><span data-stu-id="96bc2-156">Description</span></span> |
| - | - |
| <span data-ttu-id="96bc2-157">ID události</span><span class="sxs-lookup"><span data-stu-id="96bc2-157">EventId</span></span> | <span data-ttu-id="96bc2-158">Globálně jedinečný identifikátor pro tuto událost.</span><span class="sxs-lookup"><span data-stu-id="96bc2-158">Globally unique identifier for this event.</span></span> <br><br> <span data-ttu-id="96bc2-159">Příklad:</span><span class="sxs-lookup"><span data-stu-id="96bc2-159">Example:</span></span> <br><ul><li><span data-ttu-id="96bc2-160">602d9444-d2cd-49c7-8624-8643e7171297</span><span class="sxs-lookup"><span data-stu-id="96bc2-160">602d9444-d2cd-49c7-8624-8643e7171297</span></span>  |
| <span data-ttu-id="96bc2-161">Typ události</span><span class="sxs-lookup"><span data-stu-id="96bc2-161">EventType</span></span> | <span data-ttu-id="96bc2-162">Dopad, který způsobí, že tato událost.</span><span class="sxs-lookup"><span data-stu-id="96bc2-162">Impact this event causes.</span></span> <br><br> <span data-ttu-id="96bc2-163">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="96bc2-163">Values:</span></span> <br><ul><li> <span data-ttu-id="96bc2-164">`Freeze`: hello virtuálního počítače je naplánované toopause několik sekund.</span><span class="sxs-lookup"><span data-stu-id="96bc2-164">`Freeze`: hello Virtual Machine is scheduled toopause for few seconds.</span></span> <span data-ttu-id="96bc2-165">Hello procesoru bude pozastaveno, avšak neexistuje žádný vliv na paměti, otevřených souborů nebo připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="96bc2-165">hello CPU will be suspended, but there is no impact on memory, open files, or network connections.</span></span> <li><span data-ttu-id="96bc2-166">`Reboot`: hello restartování je naplánováno virtuálního počítače (dočasnou paměti dojde ke ztrátě).</span><span class="sxs-lookup"><span data-stu-id="96bc2-166">`Reboot`: hello Virtual Machine is scheduled for reboot (non-persistent memory is lost).</span></span> <li><span data-ttu-id="96bc2-167">`Redeploy`: hello virtuálního počítače je naplánované toomove tooanother uzlu (dočasné disky jsou ztraceny).</span><span class="sxs-lookup"><span data-stu-id="96bc2-167">`Redeploy`: hello Virtual Machine is scheduled toomove tooanother node (ephemeral disks are lost).</span></span> |
| <span data-ttu-id="96bc2-168">ResourceType</span><span class="sxs-lookup"><span data-stu-id="96bc2-168">ResourceType</span></span> | <span data-ttu-id="96bc2-169">Typ prostředku, který má dopad na tuto událost.</span><span class="sxs-lookup"><span data-stu-id="96bc2-169">Type of resource this event impacts.</span></span> <br><br> <span data-ttu-id="96bc2-170">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="96bc2-170">Values:</span></span> <ul><li>`VirtualMachine`|
| <span data-ttu-id="96bc2-171">Zdroje</span><span class="sxs-lookup"><span data-stu-id="96bc2-171">Resources</span></span>| <span data-ttu-id="96bc2-172">Seznam prostředků, které má dopad na tuto událost.</span><span class="sxs-lookup"><span data-stu-id="96bc2-172">List of resources this event impacts.</span></span> <span data-ttu-id="96bc2-173">Představuje záruku toocontain počítače z maximálně jeden [aktualizace domény](windows/manage-availability.md), ale nemusí obsahovat všechny počítače v hello UD.</span><span class="sxs-lookup"><span data-stu-id="96bc2-173">This is guaranteed toocontain machines from at most one [Update Domain](windows/manage-availability.md), but may not contain all machines in hello UD.</span></span> <br><br> <span data-ttu-id="96bc2-174">Příklad:</span><span class="sxs-lookup"><span data-stu-id="96bc2-174">Example:</span></span> <br><ul><li> <span data-ttu-id="96bc2-175">["FrontEnd_IN_0", "BackEnd_IN_0"]</span><span class="sxs-lookup"><span data-stu-id="96bc2-175">["FrontEnd_IN_0", "BackEnd_IN_0"]</span></span> |
| <span data-ttu-id="96bc2-176">Stav události.</span><span class="sxs-lookup"><span data-stu-id="96bc2-176">Event Status</span></span> | <span data-ttu-id="96bc2-177">Stav této události.</span><span class="sxs-lookup"><span data-stu-id="96bc2-177">Status of this event.</span></span> <br><br> <span data-ttu-id="96bc2-178">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="96bc2-178">Values:</span></span> <ul><li><span data-ttu-id="96bc2-179">`Scheduled`: Tato událost je naplánované toostart po dobu hello uvedenou v hello `NotBefore` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="96bc2-179">`Scheduled`: This event is scheduled toostart after hello time specified in hello `NotBefore` property.</span></span><li><span data-ttu-id="96bc2-180">`Started`: Tato událost byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="96bc2-180">`Started`: This event has started.</span></span></ul> <span data-ttu-id="96bc2-181">Ne `Completed` nebo se někdy poskytuje podobné stav; hello událostí bude vrácen už po dokončení hello událostí.</span><span class="sxs-lookup"><span data-stu-id="96bc2-181">No `Completed` or similar status is ever provided; hello event will no longer be returned when hello event is completed.</span></span>
| <span data-ttu-id="96bc2-182">Neplatí před</span><span class="sxs-lookup"><span data-stu-id="96bc2-182">NotBefore</span></span>| <span data-ttu-id="96bc2-183">Doba, po jejímž uplynutí může spustit tuto událost.</span><span class="sxs-lookup"><span data-stu-id="96bc2-183">Time after which this event may start.</span></span> <br><br> <span data-ttu-id="96bc2-184">Příklad:</span><span class="sxs-lookup"><span data-stu-id="96bc2-184">Example:</span></span> <br><ul><li> <span data-ttu-id="96bc2-185">2016-09-19T18:29:47Z</span><span class="sxs-lookup"><span data-stu-id="96bc2-185">2016-09-19T18:29:47Z</span></span>  |

### <a name="event-scheduling"></a><span data-ttu-id="96bc2-186">Plánování událostí</span><span class="sxs-lookup"><span data-stu-id="96bc2-186">Event Scheduling</span></span>
<span data-ttu-id="96bc2-187">Každá událost je naplánováno minimální množství času v budoucnu hello podle typu události.</span><span class="sxs-lookup"><span data-stu-id="96bc2-187">Each event is scheduled a minimum amount of time in hello future based on event type.</span></span> <span data-ttu-id="96bc2-188">Tentokrát se odrazí v události `NotBefore` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="96bc2-188">This time is reflected in an event's `NotBefore` property.</span></span> 

|<span data-ttu-id="96bc2-189">Typ události</span><span class="sxs-lookup"><span data-stu-id="96bc2-189">EventType</span></span>  | <span data-ttu-id="96bc2-190">Minimální oznámení</span><span class="sxs-lookup"><span data-stu-id="96bc2-190">Minimum Notice</span></span> |
| - | - |
| <span data-ttu-id="96bc2-191">Zablokování</span><span class="sxs-lookup"><span data-stu-id="96bc2-191">Freeze</span></span>| <span data-ttu-id="96bc2-192">15 minut</span><span class="sxs-lookup"><span data-stu-id="96bc2-192">15 minutes</span></span> |
| <span data-ttu-id="96bc2-193">Restartování</span><span class="sxs-lookup"><span data-stu-id="96bc2-193">Reboot</span></span> | <span data-ttu-id="96bc2-194">15 minut</span><span class="sxs-lookup"><span data-stu-id="96bc2-194">15 minutes</span></span> |
| <span data-ttu-id="96bc2-195">Opětovné nasazení</span><span class="sxs-lookup"><span data-stu-id="96bc2-195">Redeploy</span></span> | <span data-ttu-id="96bc2-196">10 minut</span><span class="sxs-lookup"><span data-stu-id="96bc2-196">10 minutes</span></span> |

### <a name="starting-an-event-expedite"></a><span data-ttu-id="96bc2-197">Událost spuštění (urychlit)</span><span class="sxs-lookup"><span data-stu-id="96bc2-197">Starting an event (expedite)</span></span>

<span data-ttu-id="96bc2-198">Jakmile jste se naučili nadcházející události a dokončit logika pro řádné vypnutí, můžete schválit hello nevyřízené události tak, že `POST` volání toohello metadata služby s hello `EventId`.</span><span class="sxs-lookup"><span data-stu-id="96bc2-198">Once you have learned of an upcoming event and completed your logic for graceful shutdown, you can approve hello outstanding event by making a `POST` call toohello metadata service with hello `EventId`.</span></span> <span data-ttu-id="96bc2-199">To znamená, že ho zmenšit minimální oznámení hello tooAzure čas (Pokud je to možné).</span><span class="sxs-lookup"><span data-stu-id="96bc2-199">This indicates tooAzure that it can shorten hello minimum notification time (when possible).</span></span> 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> <span data-ttu-id="96bc2-200">To je událost v úvahu umožní tooproceed hello událostí pro všechny `Resources` hello události, ne jenom hello virtuálního počítače, které uznává hello událostí.</span><span class="sxs-lookup"><span data-stu-id="96bc2-200">Acknowledging a event will allow hello event tooproceed for all `Resources` in hello event, not just hello virtual machine that acknowledges hello event.</span></span> <span data-ttu-id="96bc2-201">Proto můžete tooelect vedoucí toocoordinate hello potvrzení, který může být stejně jednoduché jako první počítač hello v hello `Resources` pole.</span><span class="sxs-lookup"><span data-stu-id="96bc2-201">You may therefore choose tooelect a leader toocoordinate hello acknowledgement, which may be as simple as hello first machine in hello `Resources` field.</span></span>

## <a name="samples"></a><span data-ttu-id="96bc2-202">Ukázky</span><span class="sxs-lookup"><span data-stu-id="96bc2-202">Samples</span></span>

### <a name="powershell-sample"></a><span data-ttu-id="96bc2-203">Ukázkové prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="96bc2-203">PowerShell Sample</span></span> 

<span data-ttu-id="96bc2-204">Hello následující ukázkové dotazy hello služby metadat pro naplánované události a schválí všechny nevyřízené události.</span><span class="sxs-lookup"><span data-stu-id="96bc2-204">hello following sample queries hello metadata service for scheduled events and approves each outstanding event.</span></span>

```PowerShell
# How tooget scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How tooapprove a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create hello Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert tooJSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with hello following: `n" $approvalString

    # Post approval string tooscheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function HandleScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up hello scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 

# Get events
$scheduledEvents = GetScheduledEvents $scheduledEventURI

# Handle events however is best for your service
HandleScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        ApproveScheduledEvent $event.EventId $scheduledEvents.DocumentIncarnation $scheduledEventURI 
    }
}
``` 


### <a name="c-sample"></a><span data-ttu-id="96bc2-205">C\# vzorku</span><span class="sxs-lookup"><span data-stu-id="96bc2-205">C\# Sample</span></span> 

<span data-ttu-id="96bc2-206">Hello následující ukázka je jednoduchý klient, který komunikuje se službou hello metadat.</span><span class="sxs-lookup"><span data-stu-id="96bc2-206">hello following sample is of a simple client that communicates with hello metadata service.</span></span>

```csharp
public class ScheduledEventsClient
{
    private readonly string scheduledEventsEndpoint;
    private readonly string defaultIpAddress = "169.254.169.254"; 

    // Set up hello scheduled events URI for a VNET-enabled VM
    public ScheduledEventsClient()
    {
        scheduledEventsEndpoint = string.Format("http://{0}/metadata/scheduledevents?api-version=2017-03-01", defaultIpAddress);
    }

    // Get events
    public string GetScheduledEvents()
    {
        Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Metadata", "true");
            return webClient.DownloadString(cloudControlUri);
        }   
    }

    // Approve events
    public void ApproveScheduledEvents(string jsonPost)
    {
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Content-Type", "application/json");
            webClient.UploadString(scheduledEventsEndpoint, jsonPost);
        }
    }
}
```

<span data-ttu-id="96bc2-207">Naplánované události může být reprezentován pomocí hello následující datové struktury:</span><span class="sxs-lookup"><span data-stu-id="96bc2-207">Scheduled Events can be represented using hello following data structures:</span></span>

```csharp
public class ScheduledEventsDocument
{
    public string DocumentIncarnation;
    public List<CloudControlEvent> Events { get; set; }
}

public class CloudControlEvent
{
    public string EventId { get; set; }
    public string EventStatus { get; set; }
    public string EventType { get; set; }
    public string ResourceType { get; set; }
    public List<string> Resources { get; set; }
    public DateTime? NotBefore { get; set; }
}

public class ScheduledEventsApproval
{
    public string DocumentIncarnation;
    public List<StartRequest> StartRequests = new List<StartRequest>();
}

public class StartRequest
{
    [JsonProperty("EventId")]
    private string eventId;

    public StartRequest(string eventId)
    {
        this.eventId = eventId;
    }
}
```

<span data-ttu-id="96bc2-208">Hello následující ukázkové dotazy hello služby metadat pro naplánované události a schválí všechny nevyřízené události.</span><span class="sxs-lookup"><span data-stu-id="96bc2-208">hello following sample queries hello metadata service for scheduled events and approves each outstanding event.</span></span>

```csharp
public class Program
{
    static ScheduledEventsClient client;

    static void Main(string[] args)
    {
        client = new ScheduledEventsClient();

        while (true)
        {
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter tooapprove executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval()
            {
                DocumentIncarnation = scheduledEventsDocument.DocumentIncarnation
            };
        
            foreach (CloudControlEvent event in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(event.EventId));
            }

            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.ApproveScheduledEvents(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter toorepeat\n\n");
            Console.ReadLine();
            Console.Clear();
        }
    }

    private static void HandleEvents(List<CloudControlEvent> events)
    {
        // Add logic for handling events here
    }
}
```

### <a name="python-sample"></a><span data-ttu-id="96bc2-209">Ukázka Pythonu</span><span class="sxs-lookup"><span data-stu-id="96bc2-209">Python Sample</span></span> 

<span data-ttu-id="96bc2-210">Hello následující ukázkové dotazy hello služby metadat pro naplánované události a schválí všechny nevyřízené události.</span><span class="sxs-lookup"><span data-stu-id="96bc2-210">hello following sample queries hello metadata service for scheduled events and approves each outstanding event.</span></span>

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            # Add logic for handling events here

def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)
   
if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a><span data-ttu-id="96bc2-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96bc2-211">Next Steps</span></span> 

- <span data-ttu-id="96bc2-212">Další informace o rozhraní API dostupná v hello hello [instance služby metadat](virtual-machines-instancemetadataservice-overview.md).</span><span class="sxs-lookup"><span data-stu-id="96bc2-212">Read more about hello APIs available in hello [instance metadata service](virtual-machines-instancemetadataservice-overview.md).</span></span>
- <span data-ttu-id="96bc2-213">Další informace o [plánované údržby pro virtuální počítače s Windows v Azure](windows/planned-maintenance.md).</span><span class="sxs-lookup"><span data-stu-id="96bc2-213">Learn about [planned maintenance for Windows virtual machines in Azure](windows/planned-maintenance.md).</span></span>
- <span data-ttu-id="96bc2-214">Další informace o [plánované údržby pro virtuální počítače s Linuxem v Azure](linux/planned-maintenance.md).</span><span class="sxs-lookup"><span data-stu-id="96bc2-214">Learn about [planned maintenance for Linux virtual machines in Azure](linux/planned-maintenance.md).</span></span>
