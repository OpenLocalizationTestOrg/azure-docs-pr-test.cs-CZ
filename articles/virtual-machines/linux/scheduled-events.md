---
title: "aaaScheduled události pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Naplánované událostí pomocí služby Azure metadat hello pro na virtuální počítače s Linuxem."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: e153c8854725330acefd67d0ef542f6149170a7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-linux-vms"></a><span data-ttu-id="2cdbc-103">Služba Azure Metadata: Naplánované události (Preview) pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="2cdbc-103">Azure Metadata Service: Scheduled Events (Preview) for Linux VMs</span></span>

> [!NOTE] 
> <span data-ttu-id="2cdbc-104">Verze Preview, jsou k dispozici tooyou probíhají hello podmínky, že souhlasíte toohello podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-104">Previews are made available tooyou on hello condition that you agree toohello terms of use.</span></span> <span data-ttu-id="2cdbc-105">Další informace najdete v [dodatečných podmínkách použití systémů Microsoft Azure Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).</span><span class="sxs-lookup"><span data-stu-id="2cdbc-105">For more information, see [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).</span></span>
>

<span data-ttu-id="2cdbc-106">Naplánované události je jedním z subservices hello pod hello Metadata služby Azure.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-106">Scheduled Events is one of hello subservices under hello Azure Metadata Service.</span></span> <span data-ttu-id="2cdbc-107">Zodpovídá za zpřístupnění informací o nadcházející události (například restartování) tak, aby vaše aplikace můžete připravit pro ně a omezit přerušení.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-107">It is responsible for surfacing information regarding upcoming events (for example, reboot) so your application can prepare for them and limit disruption.</span></span> <span data-ttu-id="2cdbc-108">Je k dispozici pro všechny typy virtuálního počítače Azure, včetně PaaS a IaaS.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-108">It is available for all Azure Virtual Machine types including PaaS and IaaS.</span></span> <span data-ttu-id="2cdbc-109">Naplánované události dává preventivní úlohami virtuálního počítače čas tooperform toominimize hello účinku událost.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-109">Scheduled Events gives your Virtual Machine time tooperform preventive tasks toominimize hello effect of an event.</span></span> 

<span data-ttu-id="2cdbc-110">Naplánované události je k dispozici pro systém Windows a virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-110">Scheduled Events is available for both Windows and Linux VMs.</span></span> <span data-ttu-id="2cdbc-111">Informace o naplánované události v systému Windows najdete v tématu [naplánované události pro virtuální počítače Windows](../windows/scheduled-events.md).</span><span class="sxs-lookup"><span data-stu-id="2cdbc-111">For information about Scheduled Events on Windows, see [Scheduled Events for Windows VMs](../windows/scheduled-events.md).</span></span>

## <a name="why-scheduled-events"></a><span data-ttu-id="2cdbc-112">Proč naplánované události?</span><span class="sxs-lookup"><span data-stu-id="2cdbc-112">Why Scheduled Events?</span></span>

<span data-ttu-id="2cdbc-113">Naplánované události je můžete provést kroky toolimit hello dopad platformy intiated údržby nebo akce zahájená uživatelem vaší služby.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-113">With Scheduled Events, you can take steps toolimit hello impact of platform-intiated maintenance or user-initiated actions on your service.</span></span> 

<span data-ttu-id="2cdbc-114">S více instancemi úloh, které použití replikace techniky toomaintain stavu, může být ohrožena toooutages děje ve více instancích.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-114">Multi-instance workloads, which use replication techniques toomaintain state, may be vulnerable toooutages happening across multiple instances.</span></span> <span data-ttu-id="2cdbc-115">Například výpadky může vést k nákladné úlohy (například rekonstrukci indexy) nebo i ke ztrátě replik.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-115">Such outages may result in expensive tasks (for example, rebuilding indexes) or even a replica loss.</span></span> 

<span data-ttu-id="2cdbc-116">V mnoha jiných případech hello celkové dostupnosti služby, je možné zvýšit provedením řádné vypnutí pořadí dokončuje (nebo ruší) během letu transakce, přeřazení tooother úlohy virtuálních počítačů v clusteru hello (ruční převzetí služeb při selhání), nebo odebrání hello Virtuální počítač z fondu vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-116">In many other cases, hello overall service availability may be improved by performing a graceful shutdown sequence such as completing (or canceling) in-flight transactions, reassigning tasks tooother VMs in hello cluster (manual failover), or removing hello Virtual Machine from a network load balancer pool.</span></span> 

<span data-ttu-id="2cdbc-117">Existují případy, kdy se žádat o pomoc správce o nadcházející události nebo protokolování takové události pomoci, vylepšení hello použitelnost aplikací hostovaných v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-117">There are cases where notifying an administrator about an upcoming event or logging such an event help improving hello serviceability of applications hosted in hello cloud.</span></span>

<span data-ttu-id="2cdbc-118">Případy použití Azure Metadata povrchy naplánované události služby v hello následující:</span><span class="sxs-lookup"><span data-stu-id="2cdbc-118">Azure Metadata Service surfaces Scheduled Events in hello following use cases:</span></span>
-   <span data-ttu-id="2cdbc-119">Platforma iniciované údržby (například zavádění hostitelským operačním systémem)</span><span class="sxs-lookup"><span data-stu-id="2cdbc-119">Platform initiated maintenance (for example, Host OS rollout)</span></span>
-   <span data-ttu-id="2cdbc-120">Uživatel spustil volání (například restartování uživatele nebo opětovně nasadí virtuální počítač)</span><span class="sxs-lookup"><span data-stu-id="2cdbc-120">User-initiated calls (for example, user restarts or redeploys a VM)</span></span>


## <a name="hello-basics"></a><span data-ttu-id="2cdbc-121">Základy Hello</span><span class="sxs-lookup"><span data-stu-id="2cdbc-121">hello basics</span></span>  

<span data-ttu-id="2cdbc-122">Služba Azure Metadata zpřístupní informace o spouštění virtuálních počítačů pomocí koncový bod REST, která je přístupná z v rámci hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-122">Azure Metadata service exposes information about running Virtual Machines using a REST Endpoint accessible from within hello VM.</span></span> <span data-ttu-id="2cdbc-123">Hello informace jsou k dispozici prostřednictvím směrovat IP tak, aby není nezveřejní hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-123">hello information is available via a non-routable IP so that it is not exposed outside hello VM.</span></span>

### <a name="scope"></a><span data-ttu-id="2cdbc-124">Rozsah</span><span class="sxs-lookup"><span data-stu-id="2cdbc-124">Scope</span></span>
<span data-ttu-id="2cdbc-125">Naplánované události jsou prezentované tooall virtuální počítače v cloudové službě nebo tooall virtuální počítače ve skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-125">Scheduled events are surfaced tooall Virtual Machines in a cloud service or tooall Virtual Machines in an Availability Set.</span></span> <span data-ttu-id="2cdbc-126">V důsledku toho byste měli zkontrolovat hello `Resources` pole tooidentify hello událostí, které virtuální počítače se bude dopad na toobe.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-126">As a result, you should check hello `Resources` field in hello event tooidentify which VMs are going toobe impacted.</span></span> 

### <a name="discovering-hello-endpoint"></a><span data-ttu-id="2cdbc-127">Koncový bod zjišťování hello</span><span class="sxs-lookup"><span data-stu-id="2cdbc-127">Discovering hello endpoint</span></span>
<span data-ttu-id="2cdbc-128">V případě hello, kde se má vytvořit virtuální počítač v rámci virtuální sítě (VNet), je k dispozici ze statické IP adresy směrovat, služba metadat hello `169.254.169.254`.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-128">In hello case where a Virtual Machine is created within a Virtual Network (VNet), hello metadata service is available from a static non-routable IP, `169.254.169.254`.</span></span>
<span data-ttu-id="2cdbc-129">Pokud není vytvořená hello virtuálního počítače v rámci virtuální sítě, hello výchozí případů pro cloudové služby a klasické virtuální počítače, je další logiku toouse koncového bodu vyžaduje toodiscover hello.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-129">If hello Virtual Machine is not created within a Virtual Network, hello default cases for cloud services and classic VMs, additional logic is required toodiscover hello endpoint toouse.</span></span> <span data-ttu-id="2cdbc-130">Jak příliš najdete ukázkové toolearn toothis[zjistit koncový bod hostitele hello](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span><span class="sxs-lookup"><span data-stu-id="2cdbc-130">Refer toothis sample toolearn how too[discover hello host endpoint](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span></span>

### <a name="versioning"></a><span data-ttu-id="2cdbc-131">Správa verzí</span><span class="sxs-lookup"><span data-stu-id="2cdbc-131">Versioning</span></span> 
<span data-ttu-id="2cdbc-132">Hello Instance služby metadat je verzí.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-132">hello Instance Metadata Service is versioned.</span></span> <span data-ttu-id="2cdbc-133">Verze jsou povinné a hello aktuální verze je `2017-03-01`.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-133">Versions are mandatory and hello current version is `2017-03-01`.</span></span>

> [!NOTE] 
> <span data-ttu-id="2cdbc-134">Předchozí verze preview naplánované událostí podporovaných {nejnovější} jako hello api-version.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-134">Previous preview releases of scheduled events supported {latest} as hello api-version.</span></span> <span data-ttu-id="2cdbc-135">Tento formát se už nepodporuje a bude v budoucí hello nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-135">This format is no longer supported and will be deprecated in hello future.</span></span>

### <a name="using-headers"></a><span data-ttu-id="2cdbc-136">Používání hlaviček</span><span class="sxs-lookup"><span data-stu-id="2cdbc-136">Using headers</span></span>
<span data-ttu-id="2cdbc-137">Když dotazujete hello Metadata služby, je nutné zadat hello záhlaví `Metadata: true` tooensure hello požadavek nebyl přesměrován náhodně.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-137">When you query hello Metadata Service, you must provide hello header `Metadata: true` tooensure hello request was not unintentionally redirected.</span></span>

### <a name="enabling-scheduled-events"></a><span data-ttu-id="2cdbc-138">Povolení naplánované události</span><span class="sxs-lookup"><span data-stu-id="2cdbc-138">Enabling Scheduled Events</span></span>
<span data-ttu-id="2cdbc-139">Hello poprvé, co musíte provést žádost pro naplánované události Azure implicitně povolí hello funkci na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-139">hello first time you make a request for scheduled events, Azure implicitly enables hello feature on your Virtual Machine.</span></span> <span data-ttu-id="2cdbc-140">V důsledku toho byste měli očekávat zpožděné odpovědi v prvním volání z až tootwo minut.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-140">As a result, you should expect a delayed response in your first call of up tootwo minutes.</span></span>

### <a name="user-initiated-maintenance"></a><span data-ttu-id="2cdbc-141">Údržby iniciované uživatelem</span><span class="sxs-lookup"><span data-stu-id="2cdbc-141">User initiated maintenance</span></span>
<span data-ttu-id="2cdbc-142">Uživatel spustil údržby virtuálního počítače prostřednictvím hello portál Azure, rozhraní API, rozhraní příkazového řádku, nebo prostředí PowerShell, které jsou výsledkem plánovaná událost.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-142">User initiated virtual machine maintenance via hello Azure portal, API, CLI, or PowerShell results in a scheduled event.</span></span> <span data-ttu-id="2cdbc-143">To vám umožní tootest hello údržby přípravy logiku v aplikaci a umožňuje tooprepare vaší aplikace pro údržbu inicializované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-143">This allows you tootest hello maintenance preparation logic in your application and allows your application tooprepare for user initiated maintenance.</span></span>

<span data-ttu-id="2cdbc-144">Restartování virtuálního počítače plány událost s typem `Reboot`.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-144">Restarting a virtual machine schedules an event with type `Reboot`.</span></span> <span data-ttu-id="2cdbc-145">Opětovné nasazení virtuálního počítače plány událost s typem `Redeploy`.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-145">Redeploying a virtual machine schedules an event with type `Redeploy`.</span></span>

> [!NOTE] 
> <span data-ttu-id="2cdbc-146">Aktuálně může být současně naplánována maximálně 10 operací údržby inicializované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-146">Currently a maximum of 10 user initiated maintenance operations can be simultaneously scheduled.</span></span> <span data-ttu-id="2cdbc-147">Tento limit bude zmírnit před naplánované události obecné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-147">This limit will be relaxed before Scheduled Events general availability.</span></span>

> [!NOTE] 
> <span data-ttu-id="2cdbc-148">Údržby iniciované uživatelem, což vede k naplánované události se momentálně nedá konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-148">Currently user initiated maintenance resulting in Scheduled Events is not configurable.</span></span> <span data-ttu-id="2cdbc-149">Možnosti konfigurace: je plánovaná pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-149">Configurability is planned for a future release.</span></span>

## <a name="using-hello-api"></a><span data-ttu-id="2cdbc-150">Pomocí rozhraní API hello</span><span class="sxs-lookup"><span data-stu-id="2cdbc-150">Using hello API</span></span>

### <a name="query-for-events"></a><span data-ttu-id="2cdbc-151">Dotaz pro události</span><span class="sxs-lookup"><span data-stu-id="2cdbc-151">Query for events</span></span>
<span data-ttu-id="2cdbc-152">Pro naplánované události můžete dotazovat jednoduše tak, že hello následující volání:</span><span class="sxs-lookup"><span data-stu-id="2cdbc-152">You can query for Scheduled Events simply by making hello following call:</span></span>

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

<span data-ttu-id="2cdbc-153">Odpověď obsahuje pole naplánované události.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-153">A response contains an array of scheduled events.</span></span> <span data-ttu-id="2cdbc-154">Prázdné pole znamená, že aktuálně neexistují žádné události naplánované.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-154">An empty array means that there are currently no events scheduled.</span></span>
<span data-ttu-id="2cdbc-155">V případě hello tam, kde jsou naplánované události, hello odpovědi obsahuje řadu událostí:</span><span class="sxs-lookup"><span data-stu-id="2cdbc-155">In hello case where there are scheduled events, hello response contains an array of events:</span></span> 
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

### <a name="event-properties"></a><span data-ttu-id="2cdbc-156">Vlastnosti události</span><span class="sxs-lookup"><span data-stu-id="2cdbc-156">Event properties</span></span>
|<span data-ttu-id="2cdbc-157">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2cdbc-157">Property</span></span>  |  <span data-ttu-id="2cdbc-158">Popis</span><span class="sxs-lookup"><span data-stu-id="2cdbc-158">Description</span></span> |
| - | - |
| <span data-ttu-id="2cdbc-159">ID události</span><span class="sxs-lookup"><span data-stu-id="2cdbc-159">EventId</span></span> | <span data-ttu-id="2cdbc-160">Globálně jedinečný identifikátor pro tuto událost.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-160">Globally unique identifier for this event.</span></span> <br><br> <span data-ttu-id="2cdbc-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2cdbc-161">Example:</span></span> <br><ul><li><span data-ttu-id="2cdbc-162">602d9444-d2cd-49c7-8624-8643e7171297</span><span class="sxs-lookup"><span data-stu-id="2cdbc-162">602d9444-d2cd-49c7-8624-8643e7171297</span></span>  |
| <span data-ttu-id="2cdbc-163">Typ události</span><span class="sxs-lookup"><span data-stu-id="2cdbc-163">EventType</span></span> | <span data-ttu-id="2cdbc-164">Dopad, který způsobí, že tato událost.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-164">Impact this event causes.</span></span> <br><br> <span data-ttu-id="2cdbc-165">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2cdbc-165">Values:</span></span> <br><ul><li> <span data-ttu-id="2cdbc-166">`Freeze`: hello virtuálního počítače je naplánované toopause několik sekund.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-166">`Freeze`: hello Virtual Machine is scheduled toopause for few seconds.</span></span> <span data-ttu-id="2cdbc-167">Hello procesoru je pozastavená, ale neexistuje žádný vliv na paměti, otevřených souborů nebo připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-167">hello CPU is suspended, but there is no impact on memory, open files, or network connections.</span></span> <li><span data-ttu-id="2cdbc-168">`Reboot`: hello restartování je naplánováno virtuálního počítače (dočasnou paměti dojde ke ztrátě).</span><span class="sxs-lookup"><span data-stu-id="2cdbc-168">`Reboot`: hello Virtual Machine is scheduled for reboot (non-persistent memory is lost).</span></span> <li><span data-ttu-id="2cdbc-169">`Redeploy`: hello virtuálního počítače je naplánované toomove tooanother uzlu (dočasné disky jsou ztraceny).</span><span class="sxs-lookup"><span data-stu-id="2cdbc-169">`Redeploy`: hello Virtual Machine is scheduled toomove tooanother node (ephemeral disks are lost).</span></span> |
| <span data-ttu-id="2cdbc-170">ResourceType</span><span class="sxs-lookup"><span data-stu-id="2cdbc-170">ResourceType</span></span> | <span data-ttu-id="2cdbc-171">Typ prostředku, který má dopad na tuto událost.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-171">Type of resource this event impacts.</span></span> <br><br> <span data-ttu-id="2cdbc-172">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2cdbc-172">Values:</span></span> <ul><li>`VirtualMachine`|
| <span data-ttu-id="2cdbc-173">Zdroje</span><span class="sxs-lookup"><span data-stu-id="2cdbc-173">Resources</span></span>| <span data-ttu-id="2cdbc-174">Seznam prostředků, které má dopad na tuto událost.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-174">List of resources this event impacts.</span></span> <span data-ttu-id="2cdbc-175">Představuje záruku toocontain počítače z maximálně jeden [aktualizace domény](manage-availability.md), ale nemusí obsahovat všechny počítače v hello UD.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-175">This is guaranteed toocontain machines from at most one [Update Domain](manage-availability.md), but may not contain all machines in hello UD.</span></span> <br><br> <span data-ttu-id="2cdbc-176">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2cdbc-176">Example:</span></span> <br><ul><li> <span data-ttu-id="2cdbc-177">["FrontEnd_IN_0", "BackEnd_IN_0"]</span><span class="sxs-lookup"><span data-stu-id="2cdbc-177">["FrontEnd_IN_0", "BackEnd_IN_0"]</span></span> |
| <span data-ttu-id="2cdbc-178">Stav události.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-178">Event Status</span></span> | <span data-ttu-id="2cdbc-179">Stav této události.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-179">Status of this event.</span></span> <br><br> <span data-ttu-id="2cdbc-180">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2cdbc-180">Values:</span></span> <ul><li><span data-ttu-id="2cdbc-181">`Scheduled`: Tato událost je naplánované toostart po dobu hello uvedenou v hello `NotBefore` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-181">`Scheduled`: This event is scheduled toostart after hello time specified in hello `NotBefore` property.</span></span><li><span data-ttu-id="2cdbc-182">`Started`: Tato událost byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-182">`Started`: This event has started.</span></span></ul> <span data-ttu-id="2cdbc-183">Ne `Completed` nebo se někdy poskytuje podobné stav; hello událostí bude vrácen už po dokončení hello událostí.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-183">No `Completed` or similar status is ever provided; hello event will no longer be returned when hello event is completed.</span></span>
| <span data-ttu-id="2cdbc-184">Neplatí před</span><span class="sxs-lookup"><span data-stu-id="2cdbc-184">NotBefore</span></span>| <span data-ttu-id="2cdbc-185">Doba, po jejímž uplynutí může spustit tuto událost.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-185">Time after which this event may start.</span></span> <br><br> <span data-ttu-id="2cdbc-186">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2cdbc-186">Example:</span></span> <br><ul><li> <span data-ttu-id="2cdbc-187">2016-09-19T18:29:47Z</span><span class="sxs-lookup"><span data-stu-id="2cdbc-187">2016-09-19T18:29:47Z</span></span>  |

### <a name="event-scheduling"></a><span data-ttu-id="2cdbc-188">Plánování událostí</span><span class="sxs-lookup"><span data-stu-id="2cdbc-188">Event scheduling</span></span>
<span data-ttu-id="2cdbc-189">Každá událost je naplánováno minimální množství času v budoucnu hello podle typu události.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-189">Each event is scheduled a minimum amount of time in hello future based on event type.</span></span> <span data-ttu-id="2cdbc-190">Tentokrát se odrazí v události `NotBefore` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-190">This time is reflected in an event's `NotBefore` property.</span></span> 

|<span data-ttu-id="2cdbc-191">Typ události</span><span class="sxs-lookup"><span data-stu-id="2cdbc-191">EventType</span></span>  | <span data-ttu-id="2cdbc-192">Minimální oznámení</span><span class="sxs-lookup"><span data-stu-id="2cdbc-192">Minimum Notice</span></span> |
| - | - |
| <span data-ttu-id="2cdbc-193">Zablokování</span><span class="sxs-lookup"><span data-stu-id="2cdbc-193">Freeze</span></span>| <span data-ttu-id="2cdbc-194">15 minut</span><span class="sxs-lookup"><span data-stu-id="2cdbc-194">15 minutes</span></span> |
| <span data-ttu-id="2cdbc-195">Restartování</span><span class="sxs-lookup"><span data-stu-id="2cdbc-195">Reboot</span></span> | <span data-ttu-id="2cdbc-196">15 minut</span><span class="sxs-lookup"><span data-stu-id="2cdbc-196">15 minutes</span></span> |
| <span data-ttu-id="2cdbc-197">Opětovné nasazení</span><span class="sxs-lookup"><span data-stu-id="2cdbc-197">Redeploy</span></span> | <span data-ttu-id="2cdbc-198">10 minut</span><span class="sxs-lookup"><span data-stu-id="2cdbc-198">10 minutes</span></span> |

### <a name="starting-an-event"></a><span data-ttu-id="2cdbc-199">Událost spuštění</span><span class="sxs-lookup"><span data-stu-id="2cdbc-199">Starting an event</span></span> 

<span data-ttu-id="2cdbc-200">Jakmile jste se naučili nadcházející události a dokončit logika pro řádné vypnutí, můžete schválit hello nevyřízené události tak, že `POST` volání toohello metadata služby s hello `EventId`.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-200">Once you have learned of an upcoming event and completed your logic for graceful shutdown, you can approve hello outstanding event by making a `POST` call toohello metadata service with hello `EventId`.</span></span> <span data-ttu-id="2cdbc-201">To znamená, že ho zmenšit minimální oznámení hello tooAzure čas (Pokud je to možné).</span><span class="sxs-lookup"><span data-stu-id="2cdbc-201">This indicates tooAzure that it can shorten hello minimum notification time (when possible).</span></span> 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> <span data-ttu-id="2cdbc-202">To v úvahu událost umožňuje tooproceed hello událostí pro všechny `Resources` hello události, ne jenom hello virtuálního počítače, které uznává hello událostí.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-202">Acknowledging an event allows hello event tooproceed for all `Resources` in hello event, not just hello virtual machine that acknowledges hello event.</span></span> <span data-ttu-id="2cdbc-203">Proto můžete tooelect vedoucí toocoordinate hello potvrzení, který může být stejně jednoduché jako první počítač hello v hello `Resources` pole.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-203">You may therefore choose tooelect a leader toocoordinate hello acknowledgement, which may be as simple as hello first machine in hello `Resources` field.</span></span>




## <a name="python-sample"></a><span data-ttu-id="2cdbc-204">Ukázka Pythonu</span><span class="sxs-lookup"><span data-stu-id="2cdbc-204">Python sample</span></span> 

<span data-ttu-id="2cdbc-205">Hello následující ukázkové dotazy hello služby metadat pro naplánované události a schválí všechny nevyřízené události.</span><span class="sxs-lookup"><span data-stu-id="2cdbc-205">hello following sample queries hello metadata service for scheduled events and approves each outstanding event.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="2cdbc-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2cdbc-206">Next steps</span></span> 

- <span data-ttu-id="2cdbc-207">Další informace o rozhraní API dostupná v hello hello [Instance Metadata služby](instance-metadata-service.md).</span><span class="sxs-lookup"><span data-stu-id="2cdbc-207">Read more about hello APIs available in hello [Instance Metadata service](instance-metadata-service.md).</span></span>
- <span data-ttu-id="2cdbc-208">Další informace o [plánované údržby pro virtuální počítače s Linuxem v Azure](planned-maintenance.md).</span><span class="sxs-lookup"><span data-stu-id="2cdbc-208">Learn about [planned maintenance for Linux virtual machines in Azure](planned-maintenance.md).</span></span>
