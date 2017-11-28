---
title: "Převést aplikací Azure Cloud Services na mikroslužeb | Microsoft Docs"
description: "Tato příručka porovná bezstavové služby Cloud Services – webové a rolí pracovního procesu a Service Fabric můžete migrovat z cloudové služby do Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 5880ebb3-8b54-4be8-af4b-95a1bc082603
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 4ab1f83e88b262b1752300b2786340d9abca8154
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="guide-to-converting-web-and-worker-roles-to-service-fabric-stateless-services"></a><span data-ttu-id="31c97-103">Průvodce převodu Web a rolí pracovního procesu na bezstavové služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="31c97-103">Guide to converting Web and Worker Roles to Service Fabric stateless services</span></span>
<span data-ttu-id="31c97-104">Tento článek popisuje, jak migrovat Service Fabric bezstavové služby Cloud Services – webové a rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="31c97-104">This article describes how to migrate your Cloud Services Web and Worker Roles to Service Fabric stateless services.</span></span> <span data-ttu-id="31c97-105">Je to ta nejjednodušší cesta migrace z cloudové služby na Service Fabric pro aplikace, jejichž přehled architektury přechází zůstane zhruba stejná.</span><span class="sxs-lookup"><span data-stu-id="31c97-105">This is the simplest migration path from Cloud Services to Service Fabric for applications whose overall architecture is going to stay roughly the same.</span></span>

## <a name="cloud-service-project-to-service-fabric-application-project"></a><span data-ttu-id="31c97-106">Projekt cloudové služby na projekt aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="31c97-106">Cloud Service project to Service Fabric application project</span></span>
 <span data-ttu-id="31c97-107">Projekt cloudové služby a aplikace Service Fabric projektu mají podobnou strukturou a obě představují jednotky nasazení pro vaši aplikaci – to znamená, že každý definovat kompletní balíček, který je nasazen na spuštění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="31c97-107">A Cloud Service project and a Service Fabric Application project have a similar structure and both represent the deployment unit for your application - that is, they each define the complete package that is deployed to run your application.</span></span> <span data-ttu-id="31c97-108">Cloudové služby projektu obsahuje jeden nebo více webu nebo rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="31c97-108">A Cloud Service project contains one or more Web or Worker Roles.</span></span> <span data-ttu-id="31c97-109">Podobně projektu aplikace Service Fabric obsahuje jednu nebo více služeb.</span><span class="sxs-lookup"><span data-stu-id="31c97-109">Similarly, a Service Fabric Application project contains one or more services.</span></span> 

<span data-ttu-id="31c97-110">Rozdíl je, že projekt cloudové služby páry v odstupu nasazení s nasazení virtuálního počítače a proto obsahuje nastavení konfigurace virtuálního počítače v, zatímco aplikace Service Fabric projektu definuje pouze aplikace, která bude nasazena na sadu existující virtuální počítače v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="31c97-110">The difference is that the Cloud Service project couples the application deployment with a VM deployment and thus contains VM configuration settings in it, whereas the Service Fabric Application project only defines an application that will be deployed to a set of existing VMs in a Service Fabric cluster.</span></span> <span data-ttu-id="31c97-111">Samotný cluster Service Fabric je nasazen pouze jednou, prostřednictvím šablonu Resource Manageru nebo prostřednictvím portálu Azure a do zařízení můžete nasadit několik aplikací Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="31c97-111">The Service Fabric cluster itself is only deployed once, either through an Resource Manager template or through the Azure portal, and multiple Service Fabric applications can be deployed to it.</span></span>

![Porovnání projektu Service Fabric a cloudové služby][3]

## <a name="worker-role-to-stateless-service"></a><span data-ttu-id="31c97-113">Role pracovního procesu pro bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="31c97-113">Worker Role to stateless service</span></span>
<span data-ttu-id="31c97-114">Role pracovního procesu koncepčně, představuje bezstavového zatížení, což znamená, každá instance úlohy je stejný jako a můžete kdykoli směrovat požadavky na jakoukoli instanci.</span><span class="sxs-lookup"><span data-stu-id="31c97-114">Conceptually, a Worker Role represents a stateless workload, meaning every instance of the workload is identical and requests can be routed to any instance at any time.</span></span> <span data-ttu-id="31c97-115">Každá instance neočekává se, mějte na paměti, předchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="31c97-115">Each instance is not expected to remember the previous request.</span></span> <span data-ttu-id="31c97-116">Stav, který zpracovává zatížení spravuje úložiště služby externí stavu, například Azure Table Storage nebo Azure DB dokumentu.</span><span class="sxs-lookup"><span data-stu-id="31c97-116">State that the workload operates on is managed by an external state store, such as Azure Table Storage or Azure Document DB.</span></span> <span data-ttu-id="31c97-117">V Service Fabric tento typ úlohy je reprezentována bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="31c97-117">In Service Fabric, this type of workload is represented by a Stateless Service.</span></span> <span data-ttu-id="31c97-118">Nejjednodušším přístupem při migraci Role pracovního procesu na Service Fabric lze provést převod kód Role pracovního procesu pro bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="31c97-118">The simplest approach to migrating a Worker Role to Service Fabric can be done by converting Worker Role code to a Stateless Service.</span></span>

![Role pracovního procesu pro bezstavové služby][4]

## <a name="web-role-to-stateless-service"></a><span data-ttu-id="31c97-120">Webovou roli na bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="31c97-120">Web Role to stateless service</span></span>
<span data-ttu-id="31c97-121">Podobně jako u Role pracovního procesu, webové Role také reprezentuje bezstavového zatížení, a proto koncepčně je příliš lze mapovat na bezstavové služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="31c97-121">Similar to Worker Role, a Web Role also represents a stateless workload, and so conceptually it too can be mapped to a Service Fabric stateless service.</span></span> <span data-ttu-id="31c97-122">Ale na rozdíl od webových rolí, Service Fabric nepodporuje službu IIS.</span><span class="sxs-lookup"><span data-stu-id="31c97-122">However, unlike Web Roles, Service Fabric does not support IIS.</span></span> <span data-ttu-id="31c97-123">K migraci webové aplikace z webové Role bezstavové služby vyžaduje první přesun webové rozhraní, které může být samoobslužně hostovaná a není závislá na System.Web, například 1 jádro ASP.NET nebo služby IIS.</span><span class="sxs-lookup"><span data-stu-id="31c97-123">To migrate a web application from a Web Role to a stateless service requires first moving to a web framework that can be self-hosted and does not depend on IIS or System.Web, such as ASP.NET Core 1.</span></span>

| <span data-ttu-id="31c97-124">**Aplikace**</span><span class="sxs-lookup"><span data-stu-id="31c97-124">**Application**</span></span> | <span data-ttu-id="31c97-125">**Podporuje se**</span><span class="sxs-lookup"><span data-stu-id="31c97-125">**Supported**</span></span> | <span data-ttu-id="31c97-126">**Cesty migrace**</span><span class="sxs-lookup"><span data-stu-id="31c97-126">**Migration path**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="31c97-127">ASP.NET – webové formuláře</span><span class="sxs-lookup"><span data-stu-id="31c97-127">ASP.NET Web Forms</span></span> |<span data-ttu-id="31c97-128">Ne</span><span class="sxs-lookup"><span data-stu-id="31c97-128">No</span></span> |<span data-ttu-id="31c97-129">Převést na MVC ASP.NET Core 1</span><span class="sxs-lookup"><span data-stu-id="31c97-129">Convert to ASP.NET Core 1 MVC</span></span> |
| <span data-ttu-id="31c97-130">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="31c97-130">ASP.NET MVC</span></span> |<span data-ttu-id="31c97-131">Pomocí nástroje Migrace</span><span class="sxs-lookup"><span data-stu-id="31c97-131">With Migration</span></span> |<span data-ttu-id="31c97-132">Upgrade na technologii ASP.NET pro základní 1 MVC</span><span class="sxs-lookup"><span data-stu-id="31c97-132">Upgrade to ASP.NET Core 1 MVC</span></span> |
| <span data-ttu-id="31c97-133">Webové rozhraní API technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="31c97-133">ASP.NET Web API</span></span> |<span data-ttu-id="31c97-134">Pomocí nástroje Migrace</span><span class="sxs-lookup"><span data-stu-id="31c97-134">With Migration</span></span> |<span data-ttu-id="31c97-135">Použít vlastním hostováním server nebo ASP.NET Core 1</span><span class="sxs-lookup"><span data-stu-id="31c97-135">Use self-hosted server or ASP.NET Core 1</span></span> |
| <span data-ttu-id="31c97-136">ASP.NET Core 1</span><span class="sxs-lookup"><span data-stu-id="31c97-136">ASP.NET Core 1</span></span> |<span data-ttu-id="31c97-137">Ano</span><span class="sxs-lookup"><span data-stu-id="31c97-137">Yes</span></span> |<span data-ttu-id="31c97-138">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="31c97-138">N/A</span></span> |

## <a name="entry-point-api-and-lifecycle"></a><span data-ttu-id="31c97-139">Vstupní bod rozhraní API a životního cyklu</span><span class="sxs-lookup"><span data-stu-id="31c97-139">Entry point API and lifecycle</span></span>
<span data-ttu-id="31c97-140">Rozhraní API nabídka podobně jako vstupní body služby Role pracovního procesu a Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="31c97-140">Worker Role and Service Fabric service APIs offer similar entry points:</span></span> 

| <span data-ttu-id="31c97-141">**Vstupní bod**</span><span class="sxs-lookup"><span data-stu-id="31c97-141">**Entry Point**</span></span> | <span data-ttu-id="31c97-142">**Role pracovního procesu**</span><span class="sxs-lookup"><span data-stu-id="31c97-142">**Worker Role**</span></span> | <span data-ttu-id="31c97-143">**Služba Service Fabric**</span><span class="sxs-lookup"><span data-stu-id="31c97-143">**Service Fabric service**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="31c97-144">Zpracování</span><span class="sxs-lookup"><span data-stu-id="31c97-144">Processing</span></span> |`Run()` |`RunAsync()` |
| <span data-ttu-id="31c97-145">Spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="31c97-145">VM start</span></span> |`OnStart()` |<span data-ttu-id="31c97-146">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="31c97-146">N/A</span></span> |
| <span data-ttu-id="31c97-147">Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="31c97-147">VM stop</span></span> |`OnStop()` |<span data-ttu-id="31c97-148">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="31c97-148">N/A</span></span> |
| <span data-ttu-id="31c97-149">Otevřete naslouchací proces pro požadavky klientů</span><span class="sxs-lookup"><span data-stu-id="31c97-149">Open listener for client requests</span></span> |<span data-ttu-id="31c97-150">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="31c97-150">N/A</span></span> |<ul><li> <span data-ttu-id="31c97-151">`CreateServiceInstanceListener()`pro bezstavové</span><span class="sxs-lookup"><span data-stu-id="31c97-151">`CreateServiceInstanceListener()` for stateless</span></span></li><li><span data-ttu-id="31c97-152">`CreateServiceReplicaListener()`pro stateful</span><span class="sxs-lookup"><span data-stu-id="31c97-152">`CreateServiceReplicaListener()` for stateful</span></span></li></ul> |

### <a name="worker-role"></a><span data-ttu-id="31c97-153">Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="31c97-153">Worker Role</span></span>
```C#

using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
        }

        public override bool OnStart()
        {
        }

        public override void OnStop()
        {
        }
    }
}

```

### <a name="service-fabric-stateless-service"></a><span data-ttu-id="31c97-154">Bezstavové služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="31c97-154">Service Fabric Stateless Service</span></span>
```C#

using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace Stateless1
{
    public class Stateless1 : StatelessService
    {
        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
        }

        protected override Task RunAsync(CancellationToken cancelServiceInstance)
        {
        }
    }
}

```

<span data-ttu-id="31c97-155">Mají obě primární "spustit" přepsání ve kterém se má začít zpracování.</span><span class="sxs-lookup"><span data-stu-id="31c97-155">Both have a primary "Run" override in which to begin processing.</span></span> <span data-ttu-id="31c97-156">Combine služby Service Fabric `Run`, `Start`, a `Stop` do jeden vstupní bod, `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="31c97-156">Service Fabric services  combine `Run`, `Start`, and `Stop` into a single entry point, `RunAsync`.</span></span> <span data-ttu-id="31c97-157">Služby by měl začínat práce při `RunAsync` spustí a by se měla zastavit při práci `RunAsync` signalizace CancellationToken metody.</span><span class="sxs-lookup"><span data-stu-id="31c97-157">Your service should begin working when `RunAsync` starts, and should stop working when the `RunAsync` method's CancellationToken is signaled.</span></span> 

<span data-ttu-id="31c97-158">Existuje několik hlavní rozdíly mezi životního cyklu a životního cyklu služeb rolí pracovního procesu a Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="31c97-158">There are several key differences between the lifecycle and lifetime of Worker Roles and Service Fabric services:</span></span>

* <span data-ttu-id="31c97-159">**Životní cyklus:** největších rozdíl je, že Role pracovního procesu je virtuální počítač, a proto je životního cyklu vázaný na virtuální počítač, který zahrnuje události spustí nebo zastaví virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="31c97-159">**Lifecycle:** The biggest difference is that a Worker Role is a VM and so its lifecycle is tied to the VM, which includes events for when the VM starts and stops.</span></span> <span data-ttu-id="31c97-160">Služba Service Fabric má životního cyklu, která je oddělená od životního cyklu virtuálních počítačů, takže neobsahuje události spuštění nebo zastavení, hostitel virtuálního počítače nebo počítače, protože nesouvisí.</span><span class="sxs-lookup"><span data-stu-id="31c97-160">A Service Fabric service has a lifecycle that is separate from the VM lifecycle, so it does not include events for when the host VM or machine starts and stop, as they are not related.</span></span>
* <span data-ttu-id="31c97-161">**Doba života:** instance Role pracovního procesu bude recyklovat, pokud `Run` metoda ukončí.</span><span class="sxs-lookup"><span data-stu-id="31c97-161">**Lifetime:** A Worker Role instance will recycle if the `Run` method exits.</span></span> <span data-ttu-id="31c97-162">`RunAsync` Na dokončení ale můžete spustit metoda ve službě Service Fabric a instance služby zůstanou nahoru.</span><span class="sxs-lookup"><span data-stu-id="31c97-162">The `RunAsync` method in a Service Fabric service however can run to completion and the service instance will stay up.</span></span> 

<span data-ttu-id="31c97-163">Service Fabric představuje vstupní bod instalaci volitelné komunikace pro služby, které naslouchat žádostem klienta.</span><span class="sxs-lookup"><span data-stu-id="31c97-163">Service Fabric provides an optional communication setup entry point for services that listen for client requests.</span></span> <span data-ttu-id="31c97-164">Vstupní bod RunAsync i komunikace jsou volitelné přepsání v Service Fabric služeb – vaše služba rozhodnout komunikovat pouze na požadavky klientů, nebo jenom spustit smyčku zpracování, nebo obojí -, které je důvod, proč je dovoleno metodě RunAsync ukončení bez restartování instance služby, protože může být nadále naslouchat žádostem klienta.</span><span class="sxs-lookup"><span data-stu-id="31c97-164">Both the RunAsync and communication entry point are optional overrides in Service Fabric services - your service may choose to only listen to client requests, or only run a processing loop, or both - which is why the RunAsync method is allowed to exit without restarting the service instance, because it may continue to listen for client requests.</span></span>

## <a name="application-api-and-environment"></a><span data-ttu-id="31c97-165">Aplikace rozhraní API a prostředí</span><span class="sxs-lookup"><span data-stu-id="31c97-165">Application API and environment</span></span>
<span data-ttu-id="31c97-166">Cloudové služby prostředí rozhraní API poskytuje informace a funkce pro aktuální instanci virtuálního počítače a také informace o další instance rolí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="31c97-166">The Cloud Services environment API provides information and functionality for the current VM instance as well as information about other VM role instances.</span></span> <span data-ttu-id="31c97-167">Obsahuje informace o jeho modulu runtime Service Fabric a některé informace o uzlu služby je v aktuálně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="31c97-167">Service Fabric provides information related to its runtime and some information about the node a service is currently running on.</span></span> 

| <span data-ttu-id="31c97-168">**Úloha prostředí**</span><span class="sxs-lookup"><span data-stu-id="31c97-168">**Environment Task**</span></span> | <span data-ttu-id="31c97-169">**Cloud Services**</span><span class="sxs-lookup"><span data-stu-id="31c97-169">**Cloud Services**</span></span> | <span data-ttu-id="31c97-170">**Service Fabric**</span><span class="sxs-lookup"><span data-stu-id="31c97-170">**Service Fabric**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="31c97-171">Nastavení konfigurace a oznámení o změně</span><span class="sxs-lookup"><span data-stu-id="31c97-171">Configuration Settings and change notification</span></span> |`RoleEnvironment` |`CodePackageActivationContext` |
| <span data-ttu-id="31c97-172">Lokální úložiště</span><span class="sxs-lookup"><span data-stu-id="31c97-172">Local Storage</span></span> |`RoleEnvironment` |`CodePackageActivationContext` |
| <span data-ttu-id="31c97-173">Informace o koncovém</span><span class="sxs-lookup"><span data-stu-id="31c97-173">Endpoint Information</span></span> |`RoleInstance` <ul><li><span data-ttu-id="31c97-174">Aktuální instance:`RoleEnvironment.CurrentRoleInstance`</span><span class="sxs-lookup"><span data-stu-id="31c97-174">Current instance: `RoleEnvironment.CurrentRoleInstance`</span></span></li><li><span data-ttu-id="31c97-175">Další role a instance:`RoleEnvironment.Roles`</span><span class="sxs-lookup"><span data-stu-id="31c97-175">Other roles and instance: `RoleEnvironment.Roles`</span></span></li> |<ul><li><span data-ttu-id="31c97-176">`NodeContext`pro aktuální uzel adresu</span><span class="sxs-lookup"><span data-stu-id="31c97-176">`NodeContext` for current Node address</span></span></li><li><span data-ttu-id="31c97-177">`FabricClient`a `ServicePartitionResolver` pro koncový bod zjišťování služby</span><span class="sxs-lookup"><span data-stu-id="31c97-177">`FabricClient` and `ServicePartitionResolver` for service endpoint discovery</span></span></li> |
| <span data-ttu-id="31c97-178">Emulace prostředí</span><span class="sxs-lookup"><span data-stu-id="31c97-178">Environment Emulation</span></span> |`RoleEnvironment.IsEmulated` |<span data-ttu-id="31c97-179">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="31c97-179">N/A</span></span> |
| <span data-ttu-id="31c97-180">Událost souběžných změny</span><span class="sxs-lookup"><span data-stu-id="31c97-180">Simultaneous change event</span></span> |`RoleEnvironment` |<span data-ttu-id="31c97-181">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="31c97-181">N/A</span></span> |

## <a name="configuration-settings"></a><span data-ttu-id="31c97-182">Nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="31c97-182">Configuration settings</span></span>
<span data-ttu-id="31c97-183">Nastavení konfigurace v cloudové služby jsou nastavené pro roli virtuálního počítače a pro všechny instance dané role virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="31c97-183">Configuration settings in Cloud Services are set for a VM role and apply to all instances of that VM role.</span></span> <span data-ttu-id="31c97-184">Tato nastavení jsou páry klíč hodnota nastavená v souborech ServiceConfiguration.*.cscfg a je možné přistupovat přímo prostřednictvím RoleEnvironment.</span><span class="sxs-lookup"><span data-stu-id="31c97-184">These settings are key-value pairs set in ServiceConfiguration.*.cscfg files and can be accessed directly through RoleEnvironment.</span></span> <span data-ttu-id="31c97-185">V Service Fabric nastavení jednotlivě pro každou službu a pro každou aplikaci, nikoli k virtuálnímu počítači, protože virtuální počítač může být hostitelem více služeb a aplikací.</span><span class="sxs-lookup"><span data-stu-id="31c97-185">In Service Fabric, settings apply individually to each service and to each application, rather than to a VM, because a VM can host multiple services and applications.</span></span> <span data-ttu-id="31c97-186">Služba se skládá ze tří balíčky:</span><span class="sxs-lookup"><span data-stu-id="31c97-186">A service is composed of three packages:</span></span>

* <span data-ttu-id="31c97-187">**Kód:** obsahuje služby spustitelné soubory, binární soubory, knihovny DLL a všechny soubory, které služba potřebuje ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="31c97-187">**Code:** contains the service executables, binaries, DLLs, and any other files a service needs to run.</span></span>
* <span data-ttu-id="31c97-188">**Konfigurace:** všechny konfigurační soubory a nastavení služby.</span><span class="sxs-lookup"><span data-stu-id="31c97-188">**Config:** all configuration files and settings for a service.</span></span>
* <span data-ttu-id="31c97-189">**Data:** statických dat soubory spojené s touto službou.</span><span class="sxs-lookup"><span data-stu-id="31c97-189">**Data:** static data files associated with the service.</span></span>

<span data-ttu-id="31c97-190">Každý z těchto balíčků může být nezávisle verzí a upgradovaný.</span><span class="sxs-lookup"><span data-stu-id="31c97-190">Each of these packages can be independently versioned and upgraded.</span></span> <span data-ttu-id="31c97-191">Podobně jako cloudové služby, konfigurační balíček je možné programově přistupovat pomocí rozhraní API a události jsou k dispozici oznámení službu ke změně konfigurace balíčku.</span><span class="sxs-lookup"><span data-stu-id="31c97-191">Similar to Cloud Services, a config package can be accessed programmatically through an API and events are available to notify the service of a config package change.</span></span> <span data-ttu-id="31c97-192">Soubor souborech Settings.xml slouží pro klíč hodnota konfigurace a programový přístup podobná části Nastavení aplikace v souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="31c97-192">A Settings.xml file can be used for key-value configuration and programmatic access similar to the app settings section of an App.config file.</span></span> <span data-ttu-id="31c97-193">Ale na rozdíl od cloudové služby, konfigurační balíček Service Fabric může obsahovat všechny konfigurační soubory v libovolném formátu toho, jestli je XML, JSON, YAML nebo vlastní binární formát.</span><span class="sxs-lookup"><span data-stu-id="31c97-193">However, unlike Cloud Services, a Service Fabric config package can contain any configuration files in any format, whether it's XML, JSON, YAML, or a custom binary format.</span></span> 

### <a name="accessing-configuration"></a><span data-ttu-id="31c97-194">Přístup ke konfiguraci</span><span class="sxs-lookup"><span data-stu-id="31c97-194">Accessing configuration</span></span>
#### <a name="cloud-services"></a><span data-ttu-id="31c97-195">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="31c97-195">Cloud Services</span></span>
<span data-ttu-id="31c97-196">Nastavení konfigurace ze ServiceConfiguration.*.cscfg je možné přistupovat prostřednictvím `RoleEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="31c97-196">Configuration settings from ServiceConfiguration.*.cscfg can be accessed through `RoleEnvironment`.</span></span> <span data-ttu-id="31c97-197">Tato nastavení jsou globálně dostupnou pro všechny instance rolí v jednom nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="31c97-197">These settings are globally available to all role instances in the same Cloud Service deployment.</span></span>

```C#

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a><span data-ttu-id="31c97-198">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="31c97-198">Service Fabric</span></span>
<span data-ttu-id="31c97-199">Každá služba má svůj vlastní balíček individuální konfigurace.</span><span class="sxs-lookup"><span data-stu-id="31c97-199">Each service has its own individual configuration package.</span></span> <span data-ttu-id="31c97-200">Všechny aplikace v clusteru je dostupné žádné předdefinované mechanismus pro globální nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="31c97-200">There is no built-in mechanism for global configuration settings accessible by all applications in a cluster.</span></span> <span data-ttu-id="31c97-201">Pokud používáte Service Fabric speciální souborech Settings.xml konfigurační soubor v rámci konfigurace balíčku, hodnoty v souborech Settings.xml lze přepsat na úrovni aplikace, aby možné nastavení konfigurace na úrovni aplikace.</span><span class="sxs-lookup"><span data-stu-id="31c97-201">When using Service Fabric's special Settings.xml configuration file within a configuration package, values in Settings.xml can be overwritten at the application level, making application-level configuration settings possible.</span></span>

<span data-ttu-id="31c97-202">Nastavení konfigurace se přistupuje v rámci každá instance služby prostřednictvím služby `CodePackageActivationContext`.</span><span class="sxs-lookup"><span data-stu-id="31c97-202">Configuration settings are accesses within each service instance through the service's `CodePackageActivationContext`.</span></span>

```C#

ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");

// Access Settings.xml
KeyedCollection<string, ConfigurationProperty> parameters = configPackage.Settings.Sections["MyConfigSection"].Parameters;

string value = parameters["Key"]?.Value;

// Access custom configuration file:
using (StreamReader reader = new StreamReader(Path.Combine(configPackage.Path, "CustomConfig.json")))
{
    MySettings settings = JsonConvert.DeserializeObject<MySettings>(reader.ReadToEnd());
}

```

### <a name="configuration-update-events"></a><span data-ttu-id="31c97-203">Události aktualizace konfigurace</span><span class="sxs-lookup"><span data-stu-id="31c97-203">Configuration update events</span></span>
#### <a name="cloud-services"></a><span data-ttu-id="31c97-204">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="31c97-204">Cloud Services</span></span>
<span data-ttu-id="31c97-205">`RoleEnvironment.Changed` Událostí se používá k upozornění všechny instance rolí, když dojde ke změně v prostředí, například o změně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="31c97-205">The `RoleEnvironment.Changed` event is used to notify all role instances when a change occurs in the environment, such as a configuration change.</span></span> <span data-ttu-id="31c97-206">To umožňuje využívat aktualizace konfigurace bez recyklace instance rolí nebo restartování pracovní proces.</span><span class="sxs-lookup"><span data-stu-id="31c97-206">This is used to consume configuration updates without recycling role instances or restarting a worker process.</span></span>

```C#

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get the list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a><span data-ttu-id="31c97-207">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="31c97-207">Service Fabric</span></span>
<span data-ttu-id="31c97-208">Každý typ tři balíček ve službě - kód, konfigurace a Data - mít události, které instanci služby balíček aktualizace, přidat nebo odebrat.</span><span class="sxs-lookup"><span data-stu-id="31c97-208">Each of the three package types in a service - Code, Config, and Data - have events that notify a service instance when a package is updated, added, or removed.</span></span> <span data-ttu-id="31c97-209">Služby mohou obsahovat více balíčků každého typu.</span><span class="sxs-lookup"><span data-stu-id="31c97-209">A service can contain multiple packages of each type.</span></span> <span data-ttu-id="31c97-210">Služba může mít například více balíčků konfigurace se každý jednotlivě verzí a rozšiřitelný.</span><span class="sxs-lookup"><span data-stu-id="31c97-210">For example, a service may have multiple config packages, each individually versioned and upgradeable.</span></span> 

<span data-ttu-id="31c97-211">Tyto události jsou k dispozici využívat změny v balíčky služeb bez restartování instance služby.</span><span class="sxs-lookup"><span data-stu-id="31c97-211">These events are available to consume changes in service packages without restarting the service instance.</span></span>

```C#

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a><span data-ttu-id="31c97-212">Spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="31c97-212">Startup tasks</span></span>
<span data-ttu-id="31c97-213">Spuštění úlohy se akcí, které předtím, než aplikaci spustí.</span><span class="sxs-lookup"><span data-stu-id="31c97-213">Startup tasks are actions that are taken before an application starts.</span></span> <span data-ttu-id="31c97-214">Úloha spuštění se obvykle používá ke spouštění skriptů instalace použitím zvýšených oprávnění.</span><span class="sxs-lookup"><span data-stu-id="31c97-214">A startup task is typically used to run setup scripts using elevated privileges.</span></span> <span data-ttu-id="31c97-215">Cloudové služby a Service Fabric podporu spuštění úloh.</span><span class="sxs-lookup"><span data-stu-id="31c97-215">Both Cloud Services and Service Fabric support start-up tasks.</span></span> <span data-ttu-id="31c97-216">Hlavní rozdíl je, že v cloudové služby, úloha spuštění je vázaný na virtuální počítač protože je součástí instanci role, zatímco v Service Fabric úloha spuštění je vázaný na služby, která není vázaný na žádné konkrétní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="31c97-216">The main difference is that in Cloud Services, a startup task is tied to a VM because it is part of a role instance, whereas in Service Fabric a startup task is tied to a service, which is not tied to any particular VM.</span></span>

| <span data-ttu-id="31c97-217">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="31c97-217">Cloud Services</span></span> | <span data-ttu-id="31c97-218">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="31c97-218">Service Fabric</span></span> |
| --- | --- | --- |
| <span data-ttu-id="31c97-219">Umístění konfigurace</span><span class="sxs-lookup"><span data-stu-id="31c97-219">Configuration location</span></span> |<span data-ttu-id="31c97-220">ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="31c97-220">ServiceDefinition.csdef</span></span> |
| <span data-ttu-id="31c97-221">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="31c97-221">Privileges</span></span> |<span data-ttu-id="31c97-222">"omezený" nebo "se zvýšenými oprávněními"</span><span class="sxs-lookup"><span data-stu-id="31c97-222">"limited" or "elevated"</span></span> |
| <span data-ttu-id="31c97-223">Pořadí úloh</span><span class="sxs-lookup"><span data-stu-id="31c97-223">Sequencing</span></span> |<span data-ttu-id="31c97-224">"jednoduché", "pozadí", "popředí"</span><span class="sxs-lookup"><span data-stu-id="31c97-224">"simple", "background", "foreground"</span></span> |

### <a name="cloud-services"></a><span data-ttu-id="31c97-225">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="31c97-225">Cloud Services</span></span>
<span data-ttu-id="31c97-226">V cloudové služby je konfigurovat vstupní bod spuštění pro jednotlivé role v ServiceDefinition.csdef.</span><span class="sxs-lookup"><span data-stu-id="31c97-226">In Cloud Services a startup entry point is configured per role in ServiceDefinition.csdef.</span></span> 

```xml

<ServiceDefinition>
    <Startup>
        <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
            <Environment>
                <Variable name="MyVersionNumber" value="1.0.0.0" />
            </Environment>
        </Task>
    </Startup>
    ...
</ServiceDefinition>

```

### <a name="service-fabric"></a><span data-ttu-id="31c97-227">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="31c97-227">Service Fabric</span></span>
<span data-ttu-id="31c97-228">V Service Fabric je nakonfigurovaný vstupní bod spuštění pro službu v ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="31c97-228">In Service Fabric a startup entry point is configured per service in ServiceManifest.xml:</span></span>

```xml

<ServiceManifest>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Startup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    ...
</ServiceManifest>

``` 

## <a name="a-note-about-development-environment"></a><span data-ttu-id="31c97-229">Poznámka o vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="31c97-229">A note about development environment</span></span>
<span data-ttu-id="31c97-230">Cloudové služby a služby infrastruktury jsou integrované s Visual Studio pomocí šablony projektů a podpora pro ladění, konfigurace a nasazení místně a do Azure.</span><span class="sxs-lookup"><span data-stu-id="31c97-230">Both Cloud Services and Service Fabric are integrated with Visual Studio with project templates and support for debugging, configuring, and deploying both locally and to Azure.</span></span> <span data-ttu-id="31c97-231">Cloudové služby a Service Fabric také poskytuje prostředí runtime místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="31c97-231">Both Cloud Services and Service Fabric also provide a local development runtime environment.</span></span> <span data-ttu-id="31c97-232">Rozdílem je, že při běhu vývoj cloudové služby emuluje prostředí Azure, ve kterém běží, Service Fabric nepoužívá emulátor – používá dokončení modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="31c97-232">The difference is that while the Cloud Service development runtime emulates the Azure environment on which it runs, Service Fabric does not use an emulator - it uses the complete Service Fabric runtime.</span></span> <span data-ttu-id="31c97-233">Service Fabric prostředí, ve kterém můžete spustit na místním vývojovém počítači je stejné prostředí, ve kterém běží v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="31c97-233">The Service Fabric environment you run on your local development machine is the same environment that runs in production.</span></span>

## <a name="next-steps"></a><span data-ttu-id="31c97-234">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31c97-234">Next steps</span></span>
<span data-ttu-id="31c97-235">Další informace o spolehlivé služby Service Fabric a základní rozdíly mezi cloudové služby a architektury aplikací Service Fabric, abyste pochopili, jak chcete využít výhod úplnou sadu funkcí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="31c97-235">Read more about Service Fabric Reliable Services and the fundamental differences between Cloud Services and Service Fabric application architecture to understand how to take advantage of the full set of Service Fabric features.</span></span>

* [<span data-ttu-id="31c97-236">Začínáme se službami Reliable Services Service Fabric</span><span class="sxs-lookup"><span data-stu-id="31c97-236">Getting started with Service Fabric Reliable Services</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="31c97-237">Koncepční Průvodce rozdíly mezi cloudovými službami a Service Fabric</span><span class="sxs-lookup"><span data-stu-id="31c97-237">Conceptual guide to the differences between Cloud Services and Service Fabric</span></span>](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
