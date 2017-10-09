---
title: "aaaConvert toomicroservices aplikací Azure Cloud Services | Microsoft Docs"
description: "Tato příručka porovná Cloud Services – webové a rolí pracovního procesu a toohelp bezstavové služby Service Fabric migrovat z cloudové služby tooService prostředků infrastruktury."
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
ms.openlocfilehash: c43b11623b2ba7f6069782a8b7e030c82572a6e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guide-tooconverting-web-and-worker-roles-tooservice-fabric-stateless-services"></a><span data-ttu-id="3f38f-103">Průvodce tooconverting Web a rolí pracovního procesu tooService Fabric bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="3f38f-103">Guide tooconverting Web and Worker Roles tooService Fabric stateless services</span></span>
<span data-ttu-id="3f38f-104">Tento článek popisuje, jak toomigrate vaše cloudové služby Web a rolí pracovního procesu tooService Fabric bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="3f38f-104">This article describes how toomigrate your Cloud Services Web and Worker Roles tooService Fabric stateless services.</span></span> <span data-ttu-id="3f38f-105">Toto je nejjednodušší cesta migrace hello z cloudové služby tooService prostředků infrastruktury pro aplikace, jejichž přehled architektury přechází toostay zhruba hello stejné.</span><span class="sxs-lookup"><span data-stu-id="3f38f-105">This is hello simplest migration path from Cloud Services tooService Fabric for applications whose overall architecture is going toostay roughly hello same.</span></span>

## <a name="cloud-service-project-tooservice-fabric-application-project"></a><span data-ttu-id="3f38f-106">Cloudové služby projektu tooService prostředků infrastruktury aplikace projektu</span><span class="sxs-lookup"><span data-stu-id="3f38f-106">Cloud Service project tooService Fabric application project</span></span>
 <span data-ttu-id="3f38f-107">Projekt cloudové služby a aplikace Service Fabric projektu mají podobnou strukturou a obě představují hello nasazení jednotky pro vaše aplikace – to znamená, že každý definují hello kompletní balíček, který je nasazený toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f38f-107">A Cloud Service project and a Service Fabric Application project have a similar structure and both represent hello deployment unit for your application - that is, they each define hello complete package that is deployed toorun your application.</span></span> <span data-ttu-id="3f38f-108">Cloudové služby projektu obsahuje jeden nebo více webu nebo rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="3f38f-108">A Cloud Service project contains one or more Web or Worker Roles.</span></span> <span data-ttu-id="3f38f-109">Podobně projektu aplikace Service Fabric obsahuje jednu nebo více služeb.</span><span class="sxs-lookup"><span data-stu-id="3f38f-109">Similarly, a Service Fabric Application project contains one or more services.</span></span> 

<span data-ttu-id="3f38f-110">Hello rozdíl je, že párům projekt cloudové služby hello hello nasazení aplikace se nasazení virtuálního počítače a proto obsahuje nastavení konfigurace virtuálního počítače v, zatímco projektu aplikace Service Fabric hello pouze definuje aplikaci, která bude nasazena Sada tooa existujících virtuálních počítačů v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3f38f-110">hello difference is that hello Cloud Service project couples hello application deployment with a VM deployment and thus contains VM configuration settings in it, whereas hello Service Fabric Application project only defines an application that will be deployed tooa set of existing VMs in a Service Fabric cluster.</span></span> <span data-ttu-id="3f38f-111">samotný cluster Service Fabric Hello je nasazen pouze jednou, buď prostřednictvím šablonu Resource Manageru nebo prostřednictvím hello portál Azure a více Service Fabric může být aplikace nasazeny tooit.</span><span class="sxs-lookup"><span data-stu-id="3f38f-111">hello Service Fabric cluster itself is only deployed once, either through an Resource Manager template or through hello Azure portal, and multiple Service Fabric applications can be deployed tooit.</span></span>

![Porovnání projektu Service Fabric a cloudové služby][3]

## <a name="worker-role-toostateless-service"></a><span data-ttu-id="3f38f-113">Služba toostateless Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="3f38f-113">Worker Role toostateless service</span></span>
<span data-ttu-id="3f38f-114">Role pracovního procesu koncepčně, představuje bezstavového zatížení, což znamená, každá instance úlohy hello je stejný jako a požadavky se dají směrované tooany instance kdykoli.</span><span class="sxs-lookup"><span data-stu-id="3f38f-114">Conceptually, a Worker Role represents a stateless workload, meaning every instance of hello workload is identical and requests can be routed tooany instance at any time.</span></span> <span data-ttu-id="3f38f-115">Každá instance není očekávaný tooremember hello předchozí požadavek.</span><span class="sxs-lookup"><span data-stu-id="3f38f-115">Each instance is not expected tooremember hello previous request.</span></span> <span data-ttu-id="3f38f-116">Stav této úlohy hello funguje na spravuje úložiště služby externí stavu, například Azure Table Storage nebo Azure DB dokumentu.</span><span class="sxs-lookup"><span data-stu-id="3f38f-116">State that hello workload operates on is managed by an external state store, such as Azure Table Storage or Azure Document DB.</span></span> <span data-ttu-id="3f38f-117">V Service Fabric tento typ úlohy je reprezentována bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="3f38f-117">In Service Fabric, this type of workload is represented by a Stateless Service.</span></span> <span data-ttu-id="3f38f-118">Nejjednodušší způsob toomigrating Hello tooService Role pracovního procesu Fabric lze provést převod tooa kód Role pracovního procesu bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="3f38f-118">hello simplest approach toomigrating a Worker Role tooService Fabric can be done by converting Worker Role code tooa Stateless Service.</span></span>

![TooStateless Role pracovního procesu služby][4]

## <a name="web-role-toostateless-service"></a><span data-ttu-id="3f38f-120">Webová služba toostateless Role</span><span class="sxs-lookup"><span data-stu-id="3f38f-120">Web Role toostateless service</span></span>
<span data-ttu-id="3f38f-121">Podobné tooWorker Role, webové Role také reprezentuje bezstavového zatížení, a proto koncepčně příliš může být namapované tooa bezstavové služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3f38f-121">Similar tooWorker Role, a Web Role also represents a stateless workload, and so conceptually it too can be mapped tooa Service Fabric stateless service.</span></span> <span data-ttu-id="3f38f-122">Ale na rozdíl od webových rolí, Service Fabric nepodporuje službu IIS.</span><span class="sxs-lookup"><span data-stu-id="3f38f-122">However, unlike Web Roles, Service Fabric does not support IIS.</span></span> <span data-ttu-id="3f38f-123">toomigrate webovou aplikaci z bezstavové služby Role webový tooa vyžaduje první přesunutí tooa webové rozhraní, které může být samoobslužně hostovaná a není závislá na System.Web, například 1 jádro ASP.NET nebo služby IIS.</span><span class="sxs-lookup"><span data-stu-id="3f38f-123">toomigrate a web application from a Web Role tooa stateless service requires first moving tooa web framework that can be self-hosted and does not depend on IIS or System.Web, such as ASP.NET Core 1.</span></span>

| <span data-ttu-id="3f38f-124">**Aplikace**</span><span class="sxs-lookup"><span data-stu-id="3f38f-124">**Application**</span></span> | <span data-ttu-id="3f38f-125">**Podporuje se**</span><span class="sxs-lookup"><span data-stu-id="3f38f-125">**Supported**</span></span> | <span data-ttu-id="3f38f-126">**Cesty migrace**</span><span class="sxs-lookup"><span data-stu-id="3f38f-126">**Migration path**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3f38f-127">ASP.NET – webové formuláře</span><span class="sxs-lookup"><span data-stu-id="3f38f-127">ASP.NET Web Forms</span></span> |<span data-ttu-id="3f38f-128">Ne</span><span class="sxs-lookup"><span data-stu-id="3f38f-128">No</span></span> |<span data-ttu-id="3f38f-129">Převést tooASP.NET základní 1 MVC</span><span class="sxs-lookup"><span data-stu-id="3f38f-129">Convert tooASP.NET Core 1 MVC</span></span> |
| <span data-ttu-id="3f38f-130">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3f38f-130">ASP.NET MVC</span></span> |<span data-ttu-id="3f38f-131">Pomocí nástroje Migrace</span><span class="sxs-lookup"><span data-stu-id="3f38f-131">With Migration</span></span> |<span data-ttu-id="3f38f-132">Upgrade tooASP.NET základní 1 MVC</span><span class="sxs-lookup"><span data-stu-id="3f38f-132">Upgrade tooASP.NET Core 1 MVC</span></span> |
| <span data-ttu-id="3f38f-133">Webové rozhraní API technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3f38f-133">ASP.NET Web API</span></span> |<span data-ttu-id="3f38f-134">Pomocí nástroje Migrace</span><span class="sxs-lookup"><span data-stu-id="3f38f-134">With Migration</span></span> |<span data-ttu-id="3f38f-135">Použít vlastním hostováním server nebo ASP.NET Core 1</span><span class="sxs-lookup"><span data-stu-id="3f38f-135">Use self-hosted server or ASP.NET Core 1</span></span> |
| <span data-ttu-id="3f38f-136">ASP.NET Core 1</span><span class="sxs-lookup"><span data-stu-id="3f38f-136">ASP.NET Core 1</span></span> |<span data-ttu-id="3f38f-137">Ano</span><span class="sxs-lookup"><span data-stu-id="3f38f-137">Yes</span></span> |<span data-ttu-id="3f38f-138">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3f38f-138">N/A</span></span> |

## <a name="entry-point-api-and-lifecycle"></a><span data-ttu-id="3f38f-139">Vstupní bod rozhraní API a životního cyklu</span><span class="sxs-lookup"><span data-stu-id="3f38f-139">Entry point API and lifecycle</span></span>
<span data-ttu-id="3f38f-140">Rozhraní API nabídka podobně jako vstupní body služby Role pracovního procesu a Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="3f38f-140">Worker Role and Service Fabric service APIs offer similar entry points:</span></span> 

| <span data-ttu-id="3f38f-141">**Vstupní bod**</span><span class="sxs-lookup"><span data-stu-id="3f38f-141">**Entry Point**</span></span> | <span data-ttu-id="3f38f-142">**Role pracovního procesu**</span><span class="sxs-lookup"><span data-stu-id="3f38f-142">**Worker Role**</span></span> | <span data-ttu-id="3f38f-143">**Služba Service Fabric**</span><span class="sxs-lookup"><span data-stu-id="3f38f-143">**Service Fabric service**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3f38f-144">Zpracování</span><span class="sxs-lookup"><span data-stu-id="3f38f-144">Processing</span></span> |`Run()` |`RunAsync()` |
| <span data-ttu-id="3f38f-145">Spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3f38f-145">VM start</span></span> |`OnStart()` |<span data-ttu-id="3f38f-146">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3f38f-146">N/A</span></span> |
| <span data-ttu-id="3f38f-147">Zastavení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3f38f-147">VM stop</span></span> |`OnStop()` |<span data-ttu-id="3f38f-148">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3f38f-148">N/A</span></span> |
| <span data-ttu-id="3f38f-149">Otevřete naslouchací proces pro požadavky klientů</span><span class="sxs-lookup"><span data-stu-id="3f38f-149">Open listener for client requests</span></span> |<span data-ttu-id="3f38f-150">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3f38f-150">N/A</span></span> |<ul><li> <span data-ttu-id="3f38f-151">`CreateServiceInstanceListener()`pro bezstavové</span><span class="sxs-lookup"><span data-stu-id="3f38f-151">`CreateServiceInstanceListener()` for stateless</span></span></li><li><span data-ttu-id="3f38f-152">`CreateServiceReplicaListener()`pro stateful</span><span class="sxs-lookup"><span data-stu-id="3f38f-152">`CreateServiceReplicaListener()` for stateful</span></span></li></ul> |

### <a name="worker-role"></a><span data-ttu-id="3f38f-153">Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="3f38f-153">Worker Role</span></span>
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

### <a name="service-fabric-stateless-service"></a><span data-ttu-id="3f38f-154">Bezstavové služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3f38f-154">Service Fabric Stateless Service</span></span>
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

<span data-ttu-id="3f38f-155">Mají obě primární "spustit" přepsání ve které toobegin zpracování.</span><span class="sxs-lookup"><span data-stu-id="3f38f-155">Both have a primary "Run" override in which toobegin processing.</span></span> <span data-ttu-id="3f38f-156">Combine služby Service Fabric `Run`, `Start`, a `Stop` do jeden vstupní bod, `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="3f38f-156">Service Fabric services  combine `Run`, `Start`, and `Stop` into a single entry point, `RunAsync`.</span></span> <span data-ttu-id="3f38f-157">Služby by měl začínat práce při `RunAsync` spustí a by se měla zastavit pracovat, když hello `RunAsync` signalizace CancellationToken metody.</span><span class="sxs-lookup"><span data-stu-id="3f38f-157">Your service should begin working when `RunAsync` starts, and should stop working when hello `RunAsync` method's CancellationToken is signaled.</span></span> 

<span data-ttu-id="3f38f-158">Existuje několik hlavní rozdíly mezi hello životního cyklu a životního cyklu služeb rolí pracovního procesu a Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="3f38f-158">There are several key differences between hello lifecycle and lifetime of Worker Roles and Service Fabric services:</span></span>

* <span data-ttu-id="3f38f-159">**Životní cyklus:** hello největších rozdílem je, že Role pracovního procesu je virtuální počítač a stejně tak životního cyklu vázanou toohello virtuální počítač, který zahrnuje události spustí nebo zastaví hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3f38f-159">**Lifecycle:** hello biggest difference is that a Worker Role is a VM and so its lifecycle is tied toohello VM, which includes events for when hello VM starts and stops.</span></span> <span data-ttu-id="3f38f-160">Služba Service Fabric má životního cyklu, která je oddělená od životního cyklu hello virtuálních počítačů, takže nebude obsahovat událostí při hello hostitele virtuálního počítače nebo počítače spuštění a zastavení, protože nesouvisí.</span><span class="sxs-lookup"><span data-stu-id="3f38f-160">A Service Fabric service has a lifecycle that is separate from hello VM lifecycle, so it does not include events for when hello host VM or machine starts and stop, as they are not related.</span></span>
* <span data-ttu-id="3f38f-161">**Doba života:** instance Role pracovního procesu bude recyklovat, pokud hello `Run` metoda ukončí.</span><span class="sxs-lookup"><span data-stu-id="3f38f-161">**Lifetime:** A Worker Role instance will recycle if hello `Run` method exits.</span></span> <span data-ttu-id="3f38f-162">Hello `RunAsync` metoda ve službě Service Fabric ale můžete spustit toocompletion a instance služby hello zůstanou nahoru.</span><span class="sxs-lookup"><span data-stu-id="3f38f-162">hello `RunAsync` method in a Service Fabric service however can run toocompletion and hello service instance will stay up.</span></span> 

<span data-ttu-id="3f38f-163">Service Fabric představuje vstupní bod instalaci volitelné komunikace pro služby, které naslouchat žádostem klienta.</span><span class="sxs-lookup"><span data-stu-id="3f38f-163">Service Fabric provides an optional communication setup entry point for services that listen for client requests.</span></span> <span data-ttu-id="3f38f-164">Hello RunAsync i komunikace vstupní bod jsou volitelné přepsání v Service Fabric služeb - služby může zvolit tooonly naslouchání tooclient požadavky, nebo spustit pouze smyčku zpracování, nebo obě -, které je důvod, proč hello metodě RunAsync je povoleno tooexit bez restartování instance služby hello, protože ho může pokračovat toolisten pro požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="3f38f-164">Both hello RunAsync and communication entry point are optional overrides in Service Fabric services - your service may choose tooonly listen tooclient requests, or only run a processing loop, or both - which is why hello RunAsync method is allowed tooexit without restarting hello service instance, because it may continue toolisten for client requests.</span></span>

## <a name="application-api-and-environment"></a><span data-ttu-id="3f38f-165">Aplikace rozhraní API a prostředí</span><span class="sxs-lookup"><span data-stu-id="3f38f-165">Application API and environment</span></span>
<span data-ttu-id="3f38f-166">Hello cloudové služby prostředí rozhraní API poskytuje informace a funkce pro hello aktuální instance virtuálního počítače a také informace o další instance rolí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3f38f-166">hello Cloud Services environment API provides information and functionality for hello current VM instance as well as information about other VM role instances.</span></span> <span data-ttu-id="3f38f-167">Service Fabric obsahuje informace související s tooits runtime a některé informace o hello uzlu služby je v aktuálně spuštěna.</span><span class="sxs-lookup"><span data-stu-id="3f38f-167">Service Fabric provides information related tooits runtime and some information about hello node a service is currently running on.</span></span> 

| <span data-ttu-id="3f38f-168">**Úloha prostředí**</span><span class="sxs-lookup"><span data-stu-id="3f38f-168">**Environment Task**</span></span> | <span data-ttu-id="3f38f-169">**Cloud Services**</span><span class="sxs-lookup"><span data-stu-id="3f38f-169">**Cloud Services**</span></span> | <span data-ttu-id="3f38f-170">**Service Fabric**</span><span class="sxs-lookup"><span data-stu-id="3f38f-170">**Service Fabric**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3f38f-171">Nastavení konfigurace a oznámení o změně</span><span class="sxs-lookup"><span data-stu-id="3f38f-171">Configuration Settings and change notification</span></span> |`RoleEnvironment` |`CodePackageActivationContext` |
| <span data-ttu-id="3f38f-172">Lokální úložiště</span><span class="sxs-lookup"><span data-stu-id="3f38f-172">Local Storage</span></span> |`RoleEnvironment` |`CodePackageActivationContext` |
| <span data-ttu-id="3f38f-173">Informace o koncovém</span><span class="sxs-lookup"><span data-stu-id="3f38f-173">Endpoint Information</span></span> |`RoleInstance` <ul><li><span data-ttu-id="3f38f-174">Aktuální instance:`RoleEnvironment.CurrentRoleInstance`</span><span class="sxs-lookup"><span data-stu-id="3f38f-174">Current instance: `RoleEnvironment.CurrentRoleInstance`</span></span></li><li><span data-ttu-id="3f38f-175">Další role a instance:`RoleEnvironment.Roles`</span><span class="sxs-lookup"><span data-stu-id="3f38f-175">Other roles and instance: `RoleEnvironment.Roles`</span></span></li> |<ul><li><span data-ttu-id="3f38f-176">`NodeContext`pro aktuální uzel adresu</span><span class="sxs-lookup"><span data-stu-id="3f38f-176">`NodeContext` for current Node address</span></span></li><li><span data-ttu-id="3f38f-177">`FabricClient`a `ServicePartitionResolver` pro koncový bod zjišťování služby</span><span class="sxs-lookup"><span data-stu-id="3f38f-177">`FabricClient` and `ServicePartitionResolver` for service endpoint discovery</span></span></li> |
| <span data-ttu-id="3f38f-178">Emulace prostředí</span><span class="sxs-lookup"><span data-stu-id="3f38f-178">Environment Emulation</span></span> |`RoleEnvironment.IsEmulated` |<span data-ttu-id="3f38f-179">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3f38f-179">N/A</span></span> |
| <span data-ttu-id="3f38f-180">Událost souběžných změny</span><span class="sxs-lookup"><span data-stu-id="3f38f-180">Simultaneous change event</span></span> |`RoleEnvironment` |<span data-ttu-id="3f38f-181">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3f38f-181">N/A</span></span> |

## <a name="configuration-settings"></a><span data-ttu-id="3f38f-182">Nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="3f38f-182">Configuration settings</span></span>
<span data-ttu-id="3f38f-183">Nastavení konfigurace v cloudové služby jsou nastavené pro roli virtuálního počítače a použít tooall instance dané role virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3f38f-183">Configuration settings in Cloud Services are set for a VM role and apply tooall instances of that VM role.</span></span> <span data-ttu-id="3f38f-184">Tato nastavení jsou páry klíč hodnota nastavená v souborech ServiceConfiguration.*.cscfg a je možné přistupovat přímo prostřednictvím RoleEnvironment.</span><span class="sxs-lookup"><span data-stu-id="3f38f-184">These settings are key-value pairs set in ServiceConfiguration.*.cscfg files and can be accessed directly through RoleEnvironment.</span></span> <span data-ttu-id="3f38f-185">V Service Fabric nastavení se týká jednotlivě tooeach služby a aplikace tooeach, nikoli tooa virtuální počítač, protože virtuální počítač může být hostitelem více služeb a aplikací.</span><span class="sxs-lookup"><span data-stu-id="3f38f-185">In Service Fabric, settings apply individually tooeach service and tooeach application, rather than tooa VM, because a VM can host multiple services and applications.</span></span> <span data-ttu-id="3f38f-186">Služba se skládá ze tří balíčky:</span><span class="sxs-lookup"><span data-stu-id="3f38f-186">A service is composed of three packages:</span></span>

* <span data-ttu-id="3f38f-187">**Kód:** obsahuje hello služby spustitelné soubory, binární soubory, knihovny DLL a další soubory služba potřebuje toorun.</span><span class="sxs-lookup"><span data-stu-id="3f38f-187">**Code:** contains hello service executables, binaries, DLLs, and any other files a service needs toorun.</span></span>
* <span data-ttu-id="3f38f-188">**Konfigurace:** všechny konfigurační soubory a nastavení služby.</span><span class="sxs-lookup"><span data-stu-id="3f38f-188">**Config:** all configuration files and settings for a service.</span></span>
* <span data-ttu-id="3f38f-189">**Data:** statických dat soubory spojené s hello služby.</span><span class="sxs-lookup"><span data-stu-id="3f38f-189">**Data:** static data files associated with hello service.</span></span>

<span data-ttu-id="3f38f-190">Každý z těchto balíčků může být nezávisle verzí a upgradovaný.</span><span class="sxs-lookup"><span data-stu-id="3f38f-190">Each of these packages can be independently versioned and upgraded.</span></span> <span data-ttu-id="3f38f-191">Podobné tooCloud služby, konfigurační balíček je možné programově přistupovat pomocí rozhraní API a události jsou k dispozici toonotify hello službu ke změně konfigurace balíčku.</span><span class="sxs-lookup"><span data-stu-id="3f38f-191">Similar tooCloud Services, a config package can be accessed programmatically through an API and events are available toonotify hello service of a config package change.</span></span> <span data-ttu-id="3f38f-192">Soubor souborech Settings.xml slouží pro klíč hodnota konfigurace a programový přístup podobné toohello aplikace v oddílu nastavení souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="3f38f-192">A Settings.xml file can be used for key-value configuration and programmatic access similar toohello app settings section of an App.config file.</span></span> <span data-ttu-id="3f38f-193">Ale na rozdíl od cloudové služby, konfigurační balíček Service Fabric může obsahovat všechny konfigurační soubory v libovolném formátu toho, jestli je XML, JSON, YAML nebo vlastní binární formát.</span><span class="sxs-lookup"><span data-stu-id="3f38f-193">However, unlike Cloud Services, a Service Fabric config package can contain any configuration files in any format, whether it's XML, JSON, YAML, or a custom binary format.</span></span> 

### <a name="accessing-configuration"></a><span data-ttu-id="3f38f-194">Přístup ke konfiguraci</span><span class="sxs-lookup"><span data-stu-id="3f38f-194">Accessing configuration</span></span>
#### <a name="cloud-services"></a><span data-ttu-id="3f38f-195">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="3f38f-195">Cloud Services</span></span>
<span data-ttu-id="3f38f-196">Nastavení konfigurace ze ServiceConfiguration.*.cscfg je možné přistupovat prostřednictvím `RoleEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="3f38f-196">Configuration settings from ServiceConfiguration.*.cscfg can be accessed through `RoleEnvironment`.</span></span> <span data-ttu-id="3f38f-197">Tato nastavení jsou globálně dostupnou tooall instancí rolí ve hello stejného nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="3f38f-197">These settings are globally available tooall role instances in hello same Cloud Service deployment.</span></span>

```C#

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a><span data-ttu-id="3f38f-198">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3f38f-198">Service Fabric</span></span>
<span data-ttu-id="3f38f-199">Každá služba má svůj vlastní balíček individuální konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3f38f-199">Each service has its own individual configuration package.</span></span> <span data-ttu-id="3f38f-200">Všechny aplikace v clusteru je dostupné žádné předdefinované mechanismus pro globální nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3f38f-200">There is no built-in mechanism for global configuration settings accessible by all applications in a cluster.</span></span> <span data-ttu-id="3f38f-201">Pokud používáte Service Fabric speciální souborech Settings.xml konfigurační soubor v rámci konfigurace balíčku, hodnoty v souborech Settings.xml lze přepsat na úrovni aplikace hello, aby možné nastavení konfigurace na úrovni aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f38f-201">When using Service Fabric's special Settings.xml configuration file within a configuration package, values in Settings.xml can be overwritten at hello application level, making application-level configuration settings possible.</span></span>

<span data-ttu-id="3f38f-202">Nastavení konfigurace se přistupuje v rámci každá instance služby prostřednictvím služby hello `CodePackageActivationContext`.</span><span class="sxs-lookup"><span data-stu-id="3f38f-202">Configuration settings are accesses within each service instance through hello service's `CodePackageActivationContext`.</span></span>

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

### <a name="configuration-update-events"></a><span data-ttu-id="3f38f-203">Události aktualizace konfigurace</span><span class="sxs-lookup"><span data-stu-id="3f38f-203">Configuration update events</span></span>
#### <a name="cloud-services"></a><span data-ttu-id="3f38f-204">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="3f38f-204">Cloud Services</span></span>
<span data-ttu-id="3f38f-205">Hello `RoleEnvironment.Changed` událostí je použité toonotify v hello prostředí, například o změně konfigurace dojde při změně všech instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="3f38f-205">hello `RoleEnvironment.Changed` event is used toonotify all role instances when a change occurs in hello environment, such as a configuration change.</span></span> <span data-ttu-id="3f38f-206">Toto je aktualizace konfigurace použité tooconsume bez recyklace instance rolí nebo restartování pracovní proces.</span><span class="sxs-lookup"><span data-stu-id="3f38f-206">This is used tooconsume configuration updates without recycling role instances or restarting a worker process.</span></span>

```C#

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get hello list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a><span data-ttu-id="3f38f-207">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3f38f-207">Service Fabric</span></span>
<span data-ttu-id="3f38f-208">Každý hello tři typy balíčků ve službě - kód, konfigurace a Data - mít události, které instanci služby balíček aktualizace, přidat nebo odebrat.</span><span class="sxs-lookup"><span data-stu-id="3f38f-208">Each of hello three package types in a service - Code, Config, and Data - have events that notify a service instance when a package is updated, added, or removed.</span></span> <span data-ttu-id="3f38f-209">Služby mohou obsahovat více balíčků každého typu.</span><span class="sxs-lookup"><span data-stu-id="3f38f-209">A service can contain multiple packages of each type.</span></span> <span data-ttu-id="3f38f-210">Služba může mít například více balíčků konfigurace se každý jednotlivě verzí a rozšiřitelný.</span><span class="sxs-lookup"><span data-stu-id="3f38f-210">For example, a service may have multiple config packages, each individually versioned and upgradeable.</span></span> 

<span data-ttu-id="3f38f-211">Tyto události jsou k dispozici tooconsume změny v balíčky služeb bez restartování instance služby hello.</span><span class="sxs-lookup"><span data-stu-id="3f38f-211">These events are available tooconsume changes in service packages without restarting hello service instance.</span></span>

```C#

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a><span data-ttu-id="3f38f-212">Spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="3f38f-212">Startup tasks</span></span>
<span data-ttu-id="3f38f-213">Spuštění úlohy se akcí, které předtím, než aplikaci spustí.</span><span class="sxs-lookup"><span data-stu-id="3f38f-213">Startup tasks are actions that are taken before an application starts.</span></span> <span data-ttu-id="3f38f-214">Úloha spuštění je obvykle používanými toorun skripty instalace použitím zvýšených oprávnění.</span><span class="sxs-lookup"><span data-stu-id="3f38f-214">A startup task is typically used toorun setup scripts using elevated privileges.</span></span> <span data-ttu-id="3f38f-215">Cloudové služby a Service Fabric podporu spuštění úloh.</span><span class="sxs-lookup"><span data-stu-id="3f38f-215">Both Cloud Services and Service Fabric support start-up tasks.</span></span> <span data-ttu-id="3f38f-216">Hello hlavní rozdíl je, že v cloudové služby, úloha spuštění se vázanou tooa virtuální počítač je součástí instanci role, zatímco v Service Fabric je úloha spuštění služby vázanou tooa, která není vázaná tooany konkrétní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3f38f-216">hello main difference is that in Cloud Services, a startup task is tied tooa VM because it is part of a role instance, whereas in Service Fabric a startup task is tied tooa service, which is not tied tooany particular VM.</span></span>

| <span data-ttu-id="3f38f-217">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="3f38f-217">Cloud Services</span></span> | <span data-ttu-id="3f38f-218">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3f38f-218">Service Fabric</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3f38f-219">Umístění konfigurace</span><span class="sxs-lookup"><span data-stu-id="3f38f-219">Configuration location</span></span> |<span data-ttu-id="3f38f-220">ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="3f38f-220">ServiceDefinition.csdef</span></span> |
| <span data-ttu-id="3f38f-221">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="3f38f-221">Privileges</span></span> |<span data-ttu-id="3f38f-222">"omezený" nebo "se zvýšenými oprávněními"</span><span class="sxs-lookup"><span data-stu-id="3f38f-222">"limited" or "elevated"</span></span> |
| <span data-ttu-id="3f38f-223">Pořadí úloh</span><span class="sxs-lookup"><span data-stu-id="3f38f-223">Sequencing</span></span> |<span data-ttu-id="3f38f-224">"jednoduché", "pozadí", "popředí"</span><span class="sxs-lookup"><span data-stu-id="3f38f-224">"simple", "background", "foreground"</span></span> |

### <a name="cloud-services"></a><span data-ttu-id="3f38f-225">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="3f38f-225">Cloud Services</span></span>
<span data-ttu-id="3f38f-226">V cloudové služby je konfigurovat vstupní bod spuštění pro jednotlivé role v ServiceDefinition.csdef.</span><span class="sxs-lookup"><span data-stu-id="3f38f-226">In Cloud Services a startup entry point is configured per role in ServiceDefinition.csdef.</span></span> 

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

### <a name="service-fabric"></a><span data-ttu-id="3f38f-227">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3f38f-227">Service Fabric</span></span>
<span data-ttu-id="3f38f-228">V Service Fabric je nakonfigurovaný vstupní bod spuštění pro službu v ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="3f38f-228">In Service Fabric a startup entry point is configured per service in ServiceManifest.xml:</span></span>

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

## <a name="a-note-about-development-environment"></a><span data-ttu-id="3f38f-229">Poznámka o vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="3f38f-229">A note about development environment</span></span>
<span data-ttu-id="3f38f-230">Cloudové služby a služby infrastruktury jsou integrované pomocí sady Visual Studio s šablony projektů a podpora pro ladění, konfigurace a nasazení místně a tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3f38f-230">Both Cloud Services and Service Fabric are integrated with Visual Studio with project templates and support for debugging, configuring, and deploying both locally and tooAzure.</span></span> <span data-ttu-id="3f38f-231">Cloudové služby a Service Fabric také poskytuje prostředí runtime místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="3f38f-231">Both Cloud Services and Service Fabric also provide a local development runtime environment.</span></span> <span data-ttu-id="3f38f-232">Hello rozdílem je, že při hello cloudové služby modulu runtime vývoj emuluje hello prostředí Azure, ve kterém běží, Service Fabric nepoužívá emulátor – používá hello dokončení modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3f38f-232">hello difference is that while hello Cloud Service development runtime emulates hello Azure environment on which it runs, Service Fabric does not use an emulator - it uses hello complete Service Fabric runtime.</span></span> <span data-ttu-id="3f38f-233">Hello Service Fabric prostředí, ve kterém můžete spustit na místním vývojovém počítači je hello stejné prostředí, které běží v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3f38f-233">hello Service Fabric environment you run on your local development machine is hello same environment that runs in production.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f38f-234">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f38f-234">Next steps</span></span>
<span data-ttu-id="3f38f-235">Další informace o Service Fabric Reliable Services a hello základní rozdíly mezi cloudové služby a toounderstand Architektura aplikace Service Fabric jak tootake výhod hello úplná sada funkcí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3f38f-235">Read more about Service Fabric Reliable Services and hello fundamental differences between Cloud Services and Service Fabric application architecture toounderstand how tootake advantage of hello full set of Service Fabric features.</span></span>

* [<span data-ttu-id="3f38f-236">Začínáme se službami Reliable Services Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3f38f-236">Getting started with Service Fabric Reliable Services</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="3f38f-237">Koncepční Průvodce toohello rozdíly mezi cloudové služby a Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3f38f-237">Conceptual guide toohello differences between Cloud Services and Service Fabric</span></span>](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
