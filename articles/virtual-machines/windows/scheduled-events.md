---
title: "Naplánované události pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Naplánované události pomocí služby Azure Metadata pro na virtuálních počítačích s Windows."
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
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: 7198fa8d1a512d10ca7022078aa2ea7bde3a4c02
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-windows-vms"></a><span data-ttu-id="7e9c7-103">Služba Azure Metadata: Naplánované události (Preview) pro virtuální počítače Windows</span><span class="sxs-lookup"><span data-stu-id="7e9c7-103">Azure Metadata Service: Scheduled Events (Preview) for Windows VMs</span></span>

> [!NOTE] 
> <span data-ttu-id="7e9c7-104">Verze Preview jsou k dispozici pro vás, za předpokladu, že souhlasíte s podmínkami použití.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-104">Previews are made available to you on the condition that you agree to the terms of use.</span></span> <span data-ttu-id="7e9c7-105">Další informace najdete v [dodatečných podmínkách použití systémů Microsoft Azure Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).</span><span class="sxs-lookup"><span data-stu-id="7e9c7-105">For more information, see [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).</span></span>
>

<span data-ttu-id="7e9c7-106">Naplánované události je jedním z subservices v rámci služby Azure metadat.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-106">Scheduled Events is one of the subservices under the Azure Metadata Service.</span></span> <span data-ttu-id="7e9c7-107">Zodpovídá za zpřístupnění informací o nadcházející události (například restartování) tak, aby vaše aplikace můžete připravit pro ně a omezit přerušení.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-107">It is responsible for surfacing information regarding upcoming events (for example, reboot) so your application can prepare for them and limit disruption.</span></span> <span data-ttu-id="7e9c7-108">Je k dispozici pro všechny typy virtuálního počítače Azure, včetně PaaS a IaaS.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-108">It is available for all Azure Virtual Machine types including PaaS and IaaS.</span></span> <span data-ttu-id="7e9c7-109">Naplánované události dává času váš virtuální počítač k provádění preventivní úloh, aby se minimalizoval vliv událost.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-109">Scheduled Events gives your Virtual Machine time to perform preventive tasks to minimize the effect of an event.</span></span> 

<span data-ttu-id="7e9c7-110">Naplánované události je k dispozici pro Linux a virtuální počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-110">Scheduled Events is available for both Linux and Windows VMs.</span></span> <span data-ttu-id="7e9c7-111">Informace o naplánované události v systému Linux najdete v tématu [naplánované události pro virtuální počítače s Linuxem](../windows/scheduled-events.md).</span><span class="sxs-lookup"><span data-stu-id="7e9c7-111">For information about Scheduled Events on Linux, see [Scheduled Events for Linux VMs](../windows/scheduled-events.md).</span></span>

## <a name="why-scheduled-events"></a><span data-ttu-id="7e9c7-112">Proč naplánované události?</span><span class="sxs-lookup"><span data-stu-id="7e9c7-112">Why Scheduled Events?</span></span>

<span data-ttu-id="7e9c7-113">S naplánované události může trvat kroky pro omezení dopad platformy intiated údržby nebo akce zahájená uživatelem na vaši službu.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-113">With Scheduled Events, you can take steps to limit the impact of platform-intiated maintenance or user-initiated actions on your service.</span></span> 

<span data-ttu-id="7e9c7-114">S více instancemi úloh, které použít techniky replikace pro uchování stavu, může být zranitelný vůči výpadků děje ve více instancích.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-114">Multi-instance workloads, which use replication techniques to maintain state, may be vulnerable to outages happening across multiple instances.</span></span> <span data-ttu-id="7e9c7-115">Například výpadky může vést k nákladné úlohy (například rekonstrukci indexy) nebo i ke ztrátě replik.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-115">Such outages may result in expensive tasks (for example, rebuilding indexes) or even a replica loss.</span></span> 

<span data-ttu-id="7e9c7-116">V mnoha jiných případech celkovým dostupnost služeb může zlepšit provedením řádné vypnutí pořadí dokončuje (nebo ruší) během letu transakce, přeřazení úlohy na ostatních virtuálních počítačů v clusteru (ruční převzetí služeb při selhání), nebo odebrání virtuální Počítač z fondu vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-116">In many other cases, the overall service availability may be improved by performing a graceful shutdown sequence such as completing (or canceling) in-flight transactions, reassigning tasks to other VMs in the cluster (manual failover), or removing the Virtual Machine from a network load balancer pool.</span></span> 

<span data-ttu-id="7e9c7-117">Existují případy, kdy se žádat o pomoc správce o nadcházející události nebo protokolování takové události pomoci, vylepšení použitelnost aplikací hostovaných v cloudu.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-117">There are cases where notifying an administrator about an upcoming event or logging such an event help improving the serviceability of applications hosted in the cloud.</span></span>

<span data-ttu-id="7e9c7-118">Služba Azure metadat poskytuje naplánované události v následujících případech použití:</span><span class="sxs-lookup"><span data-stu-id="7e9c7-118">Azure Metadata Service surfaces Scheduled Events in the following use cases:</span></span>
-   <span data-ttu-id="7e9c7-119">Platforma iniciované údržby (například zavádění hostitelským operačním systémem)</span><span class="sxs-lookup"><span data-stu-id="7e9c7-119">Platform initiated maintenance (for example, Host OS rollout)</span></span>
-   <span data-ttu-id="7e9c7-120">Uživatel spustil volání (například restartování uživatele nebo opětovně nasadí virtuální počítač)</span><span class="sxs-lookup"><span data-stu-id="7e9c7-120">User-initiated calls (for example, user restarts or redeploys a VM)</span></span>


## <a name="the-basics"></a><span data-ttu-id="7e9c7-121">Základy</span><span class="sxs-lookup"><span data-stu-id="7e9c7-121">The basics</span></span>  

<span data-ttu-id="7e9c7-122">Služba Azure Metadata zpřístupní informace o spouštění virtuálních počítačů pomocí koncový bod REST, která je přístupná z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-122">Azure Metadata service exposes information about running Virtual Machines using a REST Endpoint accessible from within the VM.</span></span> <span data-ttu-id="7e9c7-123">Informace k dispozici prostřednictvím směrovat IP, takže není nezveřejní virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-123">The information is available via a non-routable IP so that it is not exposed outside the VM.</span></span>

### <a name="scope"></a><span data-ttu-id="7e9c7-124">Rozsah</span><span class="sxs-lookup"><span data-stu-id="7e9c7-124">Scope</span></span>
<span data-ttu-id="7e9c7-125">Naplánované události jsou prezentované pro všechny virtuální počítače v cloudové službě nebo pro všechny virtuální počítače ve skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-125">Scheduled events are surfaced to all Virtual Machines in a cloud service or to all Virtual Machines in an Availability Set.</span></span> <span data-ttu-id="7e9c7-126">V důsledku toho byste měli zkontrolovat `Resources` pole v události zjistit, jaké virtuální počítače budou mít vliv.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-126">As a result, you should check the `Resources` field in the event to identify which VMs are going to be impacted.</span></span> 

### <a name="discovering-the-endpoint"></a><span data-ttu-id="7e9c7-127">Koncový bod zjišťování</span><span class="sxs-lookup"><span data-stu-id="7e9c7-127">Discovering the endpoint</span></span>
<span data-ttu-id="7e9c7-128">V případě, kde se má vytvořit virtuální počítač v rámci virtuální sítě (VNet), je k dispozici ze statické IP adresy směrovat, služba metadat `169.254.169.254`.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-128">In the case where a Virtual Machine is created within a Virtual Network (VNet), the metadata service is available from a static non-routable IP, `169.254.169.254`.</span></span>
<span data-ttu-id="7e9c7-129">Pokud virtuální počítač není vytvořen v rámci virtuální sítě, výchozí případů pro cloudové služby a klasické virtuální počítače, je potřeba další logiku zjistit koncový bod používat.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-129">If the Virtual Machine is not created within a Virtual Network, the default cases for cloud services and classic VMs, additional logic is required to discover the endpoint to use.</span></span> <span data-ttu-id="7e9c7-130">Najdete v této ukázce další postup [zjistit koncový bod hostitele](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span><span class="sxs-lookup"><span data-stu-id="7e9c7-130">Refer to this sample to learn how to [discover the host endpoint](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span></span>

### <a name="versioning"></a><span data-ttu-id="7e9c7-131">Správa verzí</span><span class="sxs-lookup"><span data-stu-id="7e9c7-131">Versioning</span></span> 
<span data-ttu-id="7e9c7-132">Služba Instance metadat je verzí.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-132">The Instance Metadata Service is versioned.</span></span> <span data-ttu-id="7e9c7-133">Verze jsou povinné a aktuální verze je `2017-03-01`.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-133">Versions are mandatory and the current version is `2017-03-01`.</span></span>

> [!NOTE] 
> <span data-ttu-id="7e9c7-134">Předchozí verze preview naplánované událostí podporovaných {nejnovější} jako verze rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-134">Previous preview releases of scheduled events supported {latest} as the api-version.</span></span> <span data-ttu-id="7e9c7-135">Tento formát se už nepodporuje a bude v budoucnu zastaralá.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-135">This format is no longer supported and will be deprecated in the future.</span></span>

### <a name="using-headers"></a><span data-ttu-id="7e9c7-136">Používání hlaviček</span><span class="sxs-lookup"><span data-stu-id="7e9c7-136">Using headers</span></span>
<span data-ttu-id="7e9c7-137">Při dotazu Metadata služby, je nutné zadat hlavičku `Metadata: true` zajistit požadavek nebyl přesměrován náhodně.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-137">When you query the Metadata Service, you must provide the header `Metadata: true` to ensure the request was not unintentionally redirected.</span></span>

### <a name="enabling-scheduled-events"></a><span data-ttu-id="7e9c7-138">Povolení naplánované události</span><span class="sxs-lookup"><span data-stu-id="7e9c7-138">Enabling Scheduled Events</span></span>
<span data-ttu-id="7e9c7-139">Při prvním může požádat o naplánované události Azure implicitně povolí funkci na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-139">The first time you make a request for scheduled events, Azure implicitly enables the feature on your Virtual Machine.</span></span> <span data-ttu-id="7e9c7-140">V důsledku toho byste měli očekávat zpožděné odpovědi v první volání až dvě minuty.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-140">As a result, you should expect a delayed response in your first call of up to two minutes.</span></span>

### <a name="user-initiated-maintenance"></a><span data-ttu-id="7e9c7-141">Údržby iniciované uživatelem</span><span class="sxs-lookup"><span data-stu-id="7e9c7-141">User initiated maintenance</span></span>
<span data-ttu-id="7e9c7-142">Uživatel spustil údržby virtuálního počítače prostřednictvím portálu Azure, rozhraní API, rozhraní příkazového řádku, nebo prostředí PowerShell, které jsou výsledkem plánovaná událost.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-142">User initiated virtual machine maintenance via the Azure portal, API, CLI, or PowerShell results in a scheduled event.</span></span> <span data-ttu-id="7e9c7-143">To umožňuje otestovat logiku přípravy údržby v aplikaci a umožňuje aplikaci připravit pro údržbu inicializované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-143">This allows you to test the maintenance preparation logic in your application and allows your application to prepare for user initiated maintenance.</span></span>

<span data-ttu-id="7e9c7-144">Restartování virtuálního počítače plány událost s typem `Reboot`.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-144">Restarting a virtual machine schedules an event with type `Reboot`.</span></span> <span data-ttu-id="7e9c7-145">Opětovné nasazení virtuálního počítače plány událost s typem `Redeploy`.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-145">Redeploying a virtual machine schedules an event with type `Redeploy`.</span></span>

> [!NOTE] 
> <span data-ttu-id="7e9c7-146">Aktuálně může být současně naplánována maximálně 10 operací údržby inicializované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-146">Currently a maximum of 10 user initiated maintenance operations can be simultaneously scheduled.</span></span> <span data-ttu-id="7e9c7-147">Tento limit bude zmírnit před naplánované události obecné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-147">This limit will be relaxed before Scheduled Events general availability.</span></span>

> [!NOTE] 
> <span data-ttu-id="7e9c7-148">Údržby iniciované uživatelem, což vede k naplánované události se momentálně nedá konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-148">Currently user initiated maintenance resulting in Scheduled Events is not configurable.</span></span> <span data-ttu-id="7e9c7-149">Možnosti konfigurace: je plánovaná pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-149">Configurability is planned for a future release.</span></span>

## <a name="using-the-api"></a><span data-ttu-id="7e9c7-150">Pomocí rozhraní API</span><span class="sxs-lookup"><span data-stu-id="7e9c7-150">Using the API</span></span>

### <a name="query-for-events"></a><span data-ttu-id="7e9c7-151">Dotaz pro události</span><span class="sxs-lookup"><span data-stu-id="7e9c7-151">Query for events</span></span>
<span data-ttu-id="7e9c7-152">Jednoduše tak, že toto volání se můžete dotazovat pro naplánované události:</span><span class="sxs-lookup"><span data-stu-id="7e9c7-152">You can query for Scheduled Events simply by making the following call:</span></span>

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

<span data-ttu-id="7e9c7-153">Odpověď obsahuje pole naplánované události.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-153">A response contains an array of scheduled events.</span></span> <span data-ttu-id="7e9c7-154">Prázdné pole znamená, že aktuálně neexistují žádné události naplánované.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-154">An empty array means that there are currently no events scheduled.</span></span>
<span data-ttu-id="7e9c7-155">V případě, kde je naplánované události, odpověď obsahuje řadu událostí:</span><span class="sxs-lookup"><span data-stu-id="7e9c7-155">In the case where there are scheduled events, the response contains an array of events:</span></span> 
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

### <a name="event-properties"></a><span data-ttu-id="7e9c7-156">Vlastnosti události</span><span class="sxs-lookup"><span data-stu-id="7e9c7-156">Event properties</span></span>
|<span data-ttu-id="7e9c7-157">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7e9c7-157">Property</span></span>  |  <span data-ttu-id="7e9c7-158">Popis</span><span class="sxs-lookup"><span data-stu-id="7e9c7-158">Description</span></span> |
| - | - |
| <span data-ttu-id="7e9c7-159">ID události</span><span class="sxs-lookup"><span data-stu-id="7e9c7-159">EventId</span></span> | <span data-ttu-id="7e9c7-160">Globálně jedinečný identifikátor pro tuto událost.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-160">Globally unique identifier for this event.</span></span> <br><br> <span data-ttu-id="7e9c7-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7e9c7-161">Example:</span></span> <br><ul><li><span data-ttu-id="7e9c7-162">602d9444-d2cd-49c7-8624-8643e7171297</span><span class="sxs-lookup"><span data-stu-id="7e9c7-162">602d9444-d2cd-49c7-8624-8643e7171297</span></span>  |
| <span data-ttu-id="7e9c7-163">Typ události</span><span class="sxs-lookup"><span data-stu-id="7e9c7-163">EventType</span></span> | <span data-ttu-id="7e9c7-164">Dopad, který způsobí, že tato událost.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-164">Impact this event causes.</span></span> <br><br> <span data-ttu-id="7e9c7-165">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="7e9c7-165">Values:</span></span> <br><ul><li> <span data-ttu-id="7e9c7-166">`Freeze`: Oprava virtuální počítač se pozastavit několik sekund.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-166">`Freeze`: The Virtual Machine is scheduled to pause for few seconds.</span></span> <span data-ttu-id="7e9c7-167">Procesor je pozastavená, ale neexistuje žádný vliv na paměti, otevřených souborů nebo připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-167">The CPU is suspended, but there is no impact on memory, open files, or network connections.</span></span> <li><span data-ttu-id="7e9c7-168">`Reboot`: Restartování je naplánováno virtuálního počítače (dočasnou paměti dojde ke ztrátě).</span><span class="sxs-lookup"><span data-stu-id="7e9c7-168">`Reboot`: The Virtual Machine is scheduled for reboot (non-persistent memory is lost).</span></span> <li><span data-ttu-id="7e9c7-169">`Redeploy`: Je naplánován virtuální počítač přesunout do jiného uzlu (dočasné disky jsou ztraceny).</span><span class="sxs-lookup"><span data-stu-id="7e9c7-169">`Redeploy`: The Virtual Machine is scheduled to move to another node (ephemeral disks are lost).</span></span> |
| <span data-ttu-id="7e9c7-170">ResourceType</span><span class="sxs-lookup"><span data-stu-id="7e9c7-170">ResourceType</span></span> | <span data-ttu-id="7e9c7-171">Typ prostředku, který má dopad na tuto událost.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-171">Type of resource this event impacts.</span></span> <br><br> <span data-ttu-id="7e9c7-172">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="7e9c7-172">Values:</span></span> <ul><li>`VirtualMachine`|
| <span data-ttu-id="7e9c7-173">Zdroje</span><span class="sxs-lookup"><span data-stu-id="7e9c7-173">Resources</span></span>| <span data-ttu-id="7e9c7-174">Seznam prostředků, které má dopad na tuto událost.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-174">List of resources this event impacts.</span></span> <span data-ttu-id="7e9c7-175">Představuje záruku obsahovat maximálně jeden počítače [aktualizace domény](manage-availability.md), ale nemusí obsahovat všechny počítače ve UD.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-175">This is guaranteed to contain machines from at most one [Update Domain](manage-availability.md), but may not contain all machines in the UD.</span></span> <br><br> <span data-ttu-id="7e9c7-176">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7e9c7-176">Example:</span></span> <br><ul><li> <span data-ttu-id="7e9c7-177">["FrontEnd_IN_0", "BackEnd_IN_0"]</span><span class="sxs-lookup"><span data-stu-id="7e9c7-177">["FrontEnd_IN_0", "BackEnd_IN_0"]</span></span> |
| <span data-ttu-id="7e9c7-178">Stav události.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-178">Event Status</span></span> | <span data-ttu-id="7e9c7-179">Stav této události.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-179">Status of this event.</span></span> <br><br> <span data-ttu-id="7e9c7-180">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="7e9c7-180">Values:</span></span> <ul><li><span data-ttu-id="7e9c7-181">`Scheduled`: Tato událost je naplánováno spuštění po dobu uvedenou v `NotBefore` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-181">`Scheduled`: This event is scheduled to start after the time specified in the `NotBefore` property.</span></span><li><span data-ttu-id="7e9c7-182">`Started`: Tato událost byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-182">`Started`: This event has started.</span></span></ul> <span data-ttu-id="7e9c7-183">Ne `Completed` nebo se někdy poskytuje podobné stav, události se nelze vrátit už po dokončení události.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-183">No `Completed` or similar status is ever provided; the event will no longer be returned when the event is completed.</span></span>
| <span data-ttu-id="7e9c7-184">Neplatí před</span><span class="sxs-lookup"><span data-stu-id="7e9c7-184">NotBefore</span></span>| <span data-ttu-id="7e9c7-185">Doba, po jejímž uplynutí může spustit tuto událost.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-185">Time after which this event may start.</span></span> <br><br> <span data-ttu-id="7e9c7-186">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7e9c7-186">Example:</span></span> <br><ul><li> <span data-ttu-id="7e9c7-187">2016-09-19T18:29:47Z</span><span class="sxs-lookup"><span data-stu-id="7e9c7-187">2016-09-19T18:29:47Z</span></span>  |

### <a name="event-scheduling"></a><span data-ttu-id="7e9c7-188">Plánování událostí</span><span class="sxs-lookup"><span data-stu-id="7e9c7-188">Event scheduling</span></span>
<span data-ttu-id="7e9c7-189">Každá událost je naplánováno minimální množství času v budoucnu podle typu události.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-189">Each event is scheduled a minimum amount of time in the future based on event type.</span></span> <span data-ttu-id="7e9c7-190">Tentokrát se odrazí v události `NotBefore` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-190">This time is reflected in an event's `NotBefore` property.</span></span> 

|<span data-ttu-id="7e9c7-191">Typ události</span><span class="sxs-lookup"><span data-stu-id="7e9c7-191">EventType</span></span>  | <span data-ttu-id="7e9c7-192">Minimální oznámení</span><span class="sxs-lookup"><span data-stu-id="7e9c7-192">Minimum Notice</span></span> |
| - | - |
| <span data-ttu-id="7e9c7-193">Zablokování</span><span class="sxs-lookup"><span data-stu-id="7e9c7-193">Freeze</span></span>| <span data-ttu-id="7e9c7-194">15 minut</span><span class="sxs-lookup"><span data-stu-id="7e9c7-194">15 minutes</span></span> |
| <span data-ttu-id="7e9c7-195">Restartování</span><span class="sxs-lookup"><span data-stu-id="7e9c7-195">Reboot</span></span> | <span data-ttu-id="7e9c7-196">15 minut</span><span class="sxs-lookup"><span data-stu-id="7e9c7-196">15 minutes</span></span> |
| <span data-ttu-id="7e9c7-197">Opětovné nasazení</span><span class="sxs-lookup"><span data-stu-id="7e9c7-197">Redeploy</span></span> | <span data-ttu-id="7e9c7-198">10 minut</span><span class="sxs-lookup"><span data-stu-id="7e9c7-198">10 minutes</span></span> |

### <a name="starting-an-event"></a><span data-ttu-id="7e9c7-199">Událost spuštění</span><span class="sxs-lookup"><span data-stu-id="7e9c7-199">Starting an event</span></span> 

<span data-ttu-id="7e9c7-200">Jakmile jste se naučili nadcházející události a dokončit logika pro řádné vypnutí, můžete schválit nevyřízené události tak, že `POST` volání na metadata služby s `EventId`.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-200">Once you have learned of an upcoming event and completed your logic for graceful shutdown, you can approve the outstanding event by making a `POST` call to the metadata service with the `EventId`.</span></span> <span data-ttu-id="7e9c7-201">To znamená do Azure, aby se zkrátil minimální oznámení čas (Pokud je to možné).</span><span class="sxs-lookup"><span data-stu-id="7e9c7-201">This indicates to Azure that it can shorten the minimum notification time (when possible).</span></span> 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> <span data-ttu-id="7e9c7-202">To v úvahu událost umožňuje událostí, aby bylo možné pokračovat pro všechny `Resources` v případě, že, ne jenom virtuální počítač, který uznává události.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-202">Acknowledging an event allows the event to proceed for all `Resources` in the event, not just the virtual machine that acknowledges the event.</span></span> <span data-ttu-id="7e9c7-203">Proto můžete zvolit vedoucí ke koordinaci potvrzení, který může být stejně jednoduché jako první počítač v `Resources` pole.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-203">You may therefore choose to elect a leader to coordinate the acknowledgement, which may be as simple as the first machine in the `Resources` field.</span></span>


## <a name="powershell-sample"></a><span data-ttu-id="7e9c7-204">Ukázkové prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7e9c7-204">PowerShell sample</span></span> 

<span data-ttu-id="7e9c7-205">Následující příklad dotazu na metadata službu pro naplánované události a schválí všechny nevyřízené události.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-205">The following sample queries the metadata service for scheduled events and approves each outstanding event.</span></span>

```PowerShell
# How to get scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert to JSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with the following: `n" $approvalString

    # Post approval string to scheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function HandleScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events URI for a VNET-enabled VM
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


## <a name="c-sample"></a><span data-ttu-id="7e9c7-206">C\# vzorku</span><span class="sxs-lookup"><span data-stu-id="7e9c7-206">C\# sample</span></span> 

<span data-ttu-id="7e9c7-207">Následující příklad je jednoduchý klient, který komunikuje se službou metadat.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-207">The following sample is of a simple client that communicates with the metadata service.</span></span>

```csharp
public class ScheduledEventsClient
{
    private readonly string scheduledEventsEndpoint;
    private readonly string defaultIpAddress = "169.254.169.254"; 

    // Set up the scheduled events URI for a VNET-enabled VM
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

<span data-ttu-id="7e9c7-208">Naplánované události může být reprezentován pomocí následující datové struktury:</span><span class="sxs-lookup"><span data-stu-id="7e9c7-208">Scheduled Events can be represented using the following data structures:</span></span>

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

<span data-ttu-id="7e9c7-209">Následující příklad dotazu na metadata službu pro naplánované události a schválí všechny nevyřízené události.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-209">The following sample queries the metadata service for scheduled events and approves each outstanding event.</span></span>

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
            Console.WriteLine("Press Enter to approve executing events\n");
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

            Console.WriteLine("Complete. Press enter to repeat\n\n");
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

## <a name="python-sample"></a><span data-ttu-id="7e9c7-210">Ukázka Pythonu</span><span class="sxs-lookup"><span data-stu-id="7e9c7-210">Python sample</span></span> 

<span data-ttu-id="7e9c7-211">Následující příklad dotazu na metadata službu pro naplánované události a schválí všechny nevyřízené události.</span><span class="sxs-lookup"><span data-stu-id="7e9c7-211">The following sample queries the metadata service for scheduled events and approves each outstanding event.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="7e9c7-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e9c7-212">Next steps</span></span> 

- <span data-ttu-id="7e9c7-213">Další informace o rozhraní API dostupná v [Instance Metadata služby](instance-metadata-service.md).</span><span class="sxs-lookup"><span data-stu-id="7e9c7-213">Read more about the APIs available in the [Instance Metadata service](instance-metadata-service.md).</span></span>
- <span data-ttu-id="7e9c7-214">Další informace o [plánované údržby pro virtuální počítače s Windows v Azure](planned-maintenance.md).</span><span class="sxs-lookup"><span data-stu-id="7e9c7-214">Learn about [planned maintenance for Windows virtual machines in Azure](planned-maintenance.md).</span></span>

