---
title: "Služba komunikace s ASP.NET Core | Microsoft Docs"
description: "Další informace o použití ASP.NET Core v bezzstavovými i stavovými službami spolehlivé."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 8aa4668d-cbb6-4225-bd2d-ab5925a868f2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/02/2017
ms.author: vturecek
ms.openlocfilehash: 8ac4d409f7363e8b4ae98be659a627ac8db8d787
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="aspnet-core-in-service-fabric-reliable-services"></a><span data-ttu-id="d87fd-103">Jádro ASP.NET v Service Fabric spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="d87fd-103">ASP.NET Core in Service Fabric Reliable Services</span></span>

<span data-ttu-id="d87fd-104">ASP.NET Core je nové open source a napříč platformami architektura pro vytváření moderní cloudové aplikace založené na připojené k Internetu, jako třeba webové aplikace, aplikace IoT a back-EndY mobilních.</span><span class="sxs-lookup"><span data-stu-id="d87fd-104">ASP.NET Core is a new open-source and cross-platform framework for building modern cloud-based Internet-connected applications, such as web apps, IoT apps, and mobile backends.</span></span> 

<span data-ttu-id="d87fd-105">Tento článek představuje podrobný průvodce pro hostování služeb ASP.NET Core v Service Fabric spolehlivé služby pomocí **Microsoft.ServiceFabric.AspNetCore.** * sadu balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="d87fd-105">This article is an in-depth guide to hosting ASP.NET Core services in Service Fabric Reliable Services using the **Microsoft.ServiceFabric.AspNetCore.*** set of NuGet packages.</span></span>

<span data-ttu-id="d87fd-106">Úvodní kurz ASP.NET Core v Service Fabric a pokyny pro nastavení vývojového prostředí najdete v tématu [vytváření webového front-endu vaší aplikace pomocí ASP.NET Core](service-fabric-add-a-web-frontend.md).</span><span class="sxs-lookup"><span data-stu-id="d87fd-106">For an introductory tutorial on ASP.NET Core in Service Fabric and instructions on getting your development environment set up, see [Building a web front-end for your application using ASP.NET Core](service-fabric-add-a-web-frontend.md).</span></span>

<span data-ttu-id="d87fd-107">Zbývající část tohoto článku předpokládá, že jste již obeznámeni s ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d87fd-107">The rest of this article assumes you are already familiar with ASP.NET Core.</span></span> <span data-ttu-id="d87fd-108">Pokud ne, doporučujeme čtení [ASP.NET Core Základy](https://docs.microsoft.com/aspnet/core/fundamentals/index).</span><span class="sxs-lookup"><span data-stu-id="d87fd-108">If not, we recommend reading through the [ASP.NET Core fundamentals](https://docs.microsoft.com/aspnet/core/fundamentals/index).</span></span>

## <a name="aspnet-core-in-the-service-fabric-environment"></a><span data-ttu-id="d87fd-109">ASP.NET Core v prostředí Service Fabric</span><span class="sxs-lookup"><span data-stu-id="d87fd-109">ASP.NET Core in the Service Fabric environment</span></span>

<span data-ttu-id="d87fd-110">Když aplikace ASP.NET Core můžete spustit na .NET Core nebo na úplné rozhraní .NET Framework, Service Fabric služby aktuálně lze spustit jen u úplné rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d87fd-110">While ASP.NET Core apps can run on .NET Core or on the full .NET Framework, Service Fabric services currently can only run on the full .NET Framework.</span></span> <span data-ttu-id="d87fd-111">To znamená, když vytváříte služby ASP.NET Core Service Fabric, musí stále cíle úplné rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d87fd-111">This means when you build an ASP.NET  Core Service Fabric service, you must still target the full .NET Framework.</span></span>

<span data-ttu-id="d87fd-112">ASP.NET Core lze použít dvěma různými způsoby v Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="d87fd-112">ASP.NET Core can be used in two different ways in Service Fabric:</span></span>
 - <span data-ttu-id="d87fd-113">**Hostovaný jako spustitelný soubor hosta**.</span><span class="sxs-lookup"><span data-stu-id="d87fd-113">**Hosted as a guest executable**.</span></span> <span data-ttu-id="d87fd-114">To slouží především ke spouštění existujících aplikací ASP.NET Core v Service Fabric beze změn kódu.</span><span class="sxs-lookup"><span data-stu-id="d87fd-114">This is primarily used to run existing ASP.NET Core applications on Service Fabric with no code changes.</span></span>
 - <span data-ttu-id="d87fd-115">**Spustit uvnitř spolehlivě**.</span><span class="sxs-lookup"><span data-stu-id="d87fd-115">**Run inside a Reliable Service**.</span></span> <span data-ttu-id="d87fd-116">To umožňuje lepší integraci s modulem runtime Service Fabric a umožňuje stavová ASP.NET Core services.</span><span class="sxs-lookup"><span data-stu-id="d87fd-116">This allows better integration with the Service Fabric runtime and allows stateful ASP.NET Core services.</span></span>

<span data-ttu-id="d87fd-117">Zbývající část tohoto článku vysvětluje, jak pomocí ASP.NET Core uvnitř spolehlivě součásti integračních služeb ASP.NET Core, které jsou součástí pomocí Service Fabric SDK.</span><span class="sxs-lookup"><span data-stu-id="d87fd-117">The rest of this article explains how to use ASP.NET Core inside a Reliable Service using the ASP.NET Core integration components that ship with the Service Fabric SDK.</span></span> 

## <a name="service-fabric-service-hosting"></a><span data-ttu-id="d87fd-118">Hostování služeb Service Fabric</span><span class="sxs-lookup"><span data-stu-id="d87fd-118">Service Fabric service hosting</span></span>

<span data-ttu-id="d87fd-119">V Service Fabric, jeden nebo více instancí a/nebo repliky služby běžet v *procesu hostitele služby*, spustitelný soubor, který spouští službu kódu.</span><span class="sxs-lookup"><span data-stu-id="d87fd-119">In Service Fabric, one or more instances and/or replicas of your service run in a *service host process*, an executable file that runs your service code.</span></span> <span data-ttu-id="d87fd-120">Můžete jako autor služby vlastní proces hostitele služby a aktivuje Service Fabric a sleduje za vás.</span><span class="sxs-lookup"><span data-stu-id="d87fd-120">You, as a service author, own the service host process and Service Fabric activates and monitors it for you.</span></span>

<span data-ttu-id="d87fd-121">Tradiční ASP.NET (až MVC 5) je úzce párované pro službu IIS prostřednictvím System.Web.dll.</span><span class="sxs-lookup"><span data-stu-id="d87fd-121">Traditional ASP.NET (up to MVC 5) is tightly coupled to IIS through System.Web.dll.</span></span> <span data-ttu-id="d87fd-122">ASP.NET Core zajišťuje oddělení mezi webovým serverem a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d87fd-122">ASP.NET Core provides a separation between the web server and your web application.</span></span> <span data-ttu-id="d87fd-123">To umožňuje webových aplikací přenosný mezi jiné webové servery a také umožňuje webové servery jako *hostovanou na vlastním*, což znamená, že můžete spustit webovém serveru vlastní proces a proces, který je vlastněn vyhrazený webový software serveru jako je například IIS.</span><span class="sxs-lookup"><span data-stu-id="d87fd-123">This allows web applications to be portable between different web servers and also allows web servers to be *self-hosted*, which means you can start a web server in your own process, as opposed to a process that is owned by dedicated web server software such as IIS.</span></span> 

<span data-ttu-id="d87fd-124">Ke kombinování služby Service Fabric a ASP.NET, jako spustitelný soubor hosta nebo v spolehlivě, musí být možné spustit ASP.NET uvnitř vaší služby hostitelský proces.</span><span class="sxs-lookup"><span data-stu-id="d87fd-124">In order to combine a Service Fabric service and ASP.NET, either as a guest executable or in a Reliable Service, you must be able to start ASP.NET inside your service host process.</span></span> <span data-ttu-id="d87fd-125">Vlastní hostování ASP.NET Core můžete to udělat.</span><span class="sxs-lookup"><span data-stu-id="d87fd-125">ASP.NET Core self-hosting allows you to do this.</span></span>

## <a name="hosting-aspnet-core-in-a-reliable-service"></a><span data-ttu-id="d87fd-126">Hostování ASP.NET Core v spolehlivě.</span><span class="sxs-lookup"><span data-stu-id="d87fd-126">Hosting ASP.NET Core in a Reliable Service</span></span>
<span data-ttu-id="d87fd-127">Obvykle vlastním hostováním aplikací ASP.NET Core vytvořte tomuto webovému hostiteli v vstupní bod aplikace, například `static void Main()` metoda v `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="d87fd-127">Typically, self-hosted ASP.NET Core applications create a WebHost in an application's entry point, such as the `static void Main()` method in `Program.cs`.</span></span> <span data-ttu-id="d87fd-128">V takovém případě je vázána životní cyklus k tomuto webovému hostiteli životního cyklu procesu.</span><span class="sxs-lookup"><span data-stu-id="d87fd-128">In this case, the lifecycle of the WebHost is bound to the lifecycle of the process.</span></span>

![Hostování v procesu ASP.NET Core][0]

<span data-ttu-id="d87fd-130">Ale vstupní bod aplikace není na správném místě vytvořit tomuto webovému hostiteli spolehlivé služby, protože vstupní bod aplikace slouží pouze k registraci typ služby s modulem runtime Service Fabric, aby může vytvořit instance tohoto typu služby.</span><span class="sxs-lookup"><span data-stu-id="d87fd-130">However, the application entry point is not the right place to create a WebHost in a Reliable Service, because the application entry point is only used to register a service type with the Service Fabric runtime, so that it may create instances of that service type.</span></span> <span data-ttu-id="d87fd-131">K tomuto webovému hostiteli by měl být vytvořen v spolehlivě sám sebe.</span><span class="sxs-lookup"><span data-stu-id="d87fd-131">The WebHost should be created in a Reliable Service itself.</span></span> <span data-ttu-id="d87fd-132">V rámci procesu hostitele služby instance služby nebo repliky můžete projít více životní cykly.</span><span class="sxs-lookup"><span data-stu-id="d87fd-132">Within the service host process, service instances and/or replicas can go through multiple lifecycles.</span></span> 

<span data-ttu-id="d87fd-133">Instance spolehlivé služby je reprezentována třídě služby odvozování z `StatelessService` nebo `StatefulService`.</span><span class="sxs-lookup"><span data-stu-id="d87fd-133">A Reliable Service instance is represented by your service class deriving from `StatelessService` or `StatefulService`.</span></span> <span data-ttu-id="d87fd-134">Je součástí komunikačního balíku pro službu `ICommunicationListener` implementace ve třídě služby.</span><span class="sxs-lookup"><span data-stu-id="d87fd-134">The communication stack for a service is contained in an `ICommunicationListener` implementation in your service class.</span></span> <span data-ttu-id="d87fd-135">`Microsoft.ServiceFabric.Services.AspNetCore.*` Balíčky NuGet obsahovat implementace `ICommunicationListener` , spouštět a spravovat k tomuto webovému hostiteli ASP.NET Core Kestrel nebo WebListener v spolehlivě.</span><span class="sxs-lookup"><span data-stu-id="d87fd-135">The `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet packages contain implementations of `ICommunicationListener` that start and manage the ASP.NET Core WebHost for either Kestrel or WebListener in a Reliable Service.</span></span>

![Hostování ASP.NET Core v spolehlivě.][1]

## <a name="aspnet-core-icommunicationlisteners"></a><span data-ttu-id="d87fd-137">ICommunicationListeners ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d87fd-137">ASP.NET Core ICommunicationListeners</span></span>
<span data-ttu-id="d87fd-138">`ICommunicationListener` Implementace pro Kestrel a WebListener v `Microsoft.ServiceFabric.Services.AspNetCore.*` balíčky NuGet podobné použití vzorce ale provádět mírně odlišné akce, které jsou specifické pro každý webový server.</span><span class="sxs-lookup"><span data-stu-id="d87fd-138">The `ICommunicationListener` implementations for Kestrel and WebListener in the  `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet packages have similar use patterns but perform slightly different actions specific to each web server.</span></span> 

<span data-ttu-id="d87fd-139">Obě naslouchací procesy komunikace poskytují konstruktor, který má následující argumenty:</span><span class="sxs-lookup"><span data-stu-id="d87fd-139">Both communication listeners provide a constructor that takes the following arguments:</span></span>
 - <span data-ttu-id="d87fd-140">**`ServiceContext serviceContext`**: `ServiceContext` Objekt, který obsahuje informace o spuštěné služby.</span><span class="sxs-lookup"><span data-stu-id="d87fd-140">**`ServiceContext serviceContext`**: The `ServiceContext` object that contains information about the running service.</span></span>
 - <span data-ttu-id="d87fd-141">**`string endpointName`**: název `Endpoint` konfigurace v ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="d87fd-141">**`string endpointName`**: the name of an `Endpoint` configuration in ServiceManifest.xml.</span></span> <span data-ttu-id="d87fd-142">Toto je primárně kde naslouchací procesy komunikace dvě liší: WebListener **vyžaduje** `Endpoint` konfigurace, zatímco Kestrel neexistuje.</span><span class="sxs-lookup"><span data-stu-id="d87fd-142">This is primarily where the two communication listeners differ: WebListener **requires** an `Endpoint` configuration, while Kestrel does not.</span></span>
 - <span data-ttu-id="d87fd-143">**`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: lambda, který implementujete, ve kterém se vytváří a zpět `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="d87fd-143">**`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: a lambda that you implement in which you create and return an `IWebHost`.</span></span> <span data-ttu-id="d87fd-144">To vám umožňuje nakonfigurovat `IWebHost` způsob, jakým byste obvykle v aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d87fd-144">This allows you to configure `IWebHost` the way you normally would in an ASP.NET Core application.</span></span> <span data-ttu-id="d87fd-145">Argument lambda obsahuje adresu URL, která se generují můžete v závislosti na integraci Service Fabric možnostech, můžete použít a `Endpoint` konfigurace, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="d87fd-145">The lambda provides a URL which is generated for you depending on the Service Fabric integration options you use and the `Endpoint` configuration you provide.</span></span> <span data-ttu-id="d87fd-146">Aby adresa URL lze potom změnit nebo použít jako-je spustí webový server.</span><span class="sxs-lookup"><span data-stu-id="d87fd-146">That URL can then be modified or used as-is to start the web server.</span></span>

## <a name="service-fabric-integration-middleware"></a><span data-ttu-id="d87fd-147">Middleware integrace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="d87fd-147">Service Fabric integration middleware</span></span>
<span data-ttu-id="d87fd-148">`Microsoft.ServiceFabric.Services.AspNetCore` Balíček NuGet obsahuje `UseServiceFabricIntegration` rozšiřující metody na `IWebHostBuilder` , přidá middleware podporující služby prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="d87fd-148">The `Microsoft.ServiceFabric.Services.AspNetCore` NuGet package includes the `UseServiceFabricIntegration` extension method on `IWebHostBuilder` that adds Service Fabric-aware middleware.</span></span> <span data-ttu-id="d87fd-149">Tento middleware nakonfiguruje Kestrel nebo WebListener `ICommunicationListener` při registraci adresy URL jedinečné služby s Service Fabric Naming Service a poté ověří požadavky klientů, aby se klienti připojují k službě správné.</span><span class="sxs-lookup"><span data-stu-id="d87fd-149">This middleware configures the Kestrel or WebListener `ICommunicationListener` to register a unique service URL with the Service Fabric Naming Service and then validates client requests to ensure clients are connecting to the right service.</span></span> <span data-ttu-id="d87fd-150">To je nezbytné v prostředí s sdílené hostitele, jako jsou Service Fabric, kde můžete spustit na stejný fyzický nebo virtuální počítač více webových aplikací, ale nepoužívejte názvy hostitelů jedinečný, čímž zabráníte klientům omylem připojení ke službě nesprávný.</span><span class="sxs-lookup"><span data-stu-id="d87fd-150">This is necessary in a shared-host environment such as Service Fabric, where multiple web applications can run on the same physical or virtual machine but do not use unique host names, to prevent clients from mistakenly connecting to the wrong service.</span></span> <span data-ttu-id="d87fd-151">Tento scénář je podrobně popsaná v další v další části.</span><span class="sxs-lookup"><span data-stu-id="d87fd-151">This scenario is described in more detail in the next section.</span></span>

### <a name="a-case-of-mistaken-identity"></a><span data-ttu-id="d87fd-152">V případě chybné identifikace</span><span class="sxs-lookup"><span data-stu-id="d87fd-152">A case of mistaken identity</span></span>
<span data-ttu-id="d87fd-153">Repliky služby, bez ohledu na protokol, naslouchat na kombinaci jedinečné IP: port.</span><span class="sxs-lookup"><span data-stu-id="d87fd-153">Service replicas, regardless of protocol, listen on a unique IP:port combination.</span></span> <span data-ttu-id="d87fd-154">Jakmile repliku služby bylo zahájeno naslouchání na koncový bod IP: port, sestav služby prostředků infrastruktury služby DNS kde můžete zjistit pomocí klienty nebo jiné služby pro tuto adresu koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="d87fd-154">Once a service replica has started listening on an IP:port endpoint, it reports that endpoint address to the Service Fabric Naming Service where it can be discovered by clients or other services.</span></span> <span data-ttu-id="d87fd-155">Pokud služby používat porty dynamicky přiřadit aplikace, může repliku služby shodou použijte stejný koncový bod IP: port jiné službě, která byla předtím na stejný fyzický nebo virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d87fd-155">If services use dynamically-assigned application ports, a service replica may coincidentally use the same IP:port endpoint of another service that was previously on the same physical or virtual machine.</span></span> <span data-ttu-id="d87fd-156">To může způsobit klienta tak, aby mistakely připojit ke službě nesprávný.</span><span class="sxs-lookup"><span data-stu-id="d87fd-156">This can cause a client to mistakely connect to the wrong service.</span></span> <span data-ttu-id="d87fd-157">K tomu může dojít, pokud dojde k následujícímu pořadí událostí:</span><span class="sxs-lookup"><span data-stu-id="d87fd-157">This can happen if the following sequence of events occur:</span></span>

 1. <span data-ttu-id="d87fd-158">Služby A naslouchá na 10.0.0.1:30000 přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="d87fd-158">Service A listens on 10.0.0.1:30000 over HTTP.</span></span> 
 2. <span data-ttu-id="d87fd-159">Klienta přeloží A služby a získá adresu 10.0.0.1:30000</span><span class="sxs-lookup"><span data-stu-id="d87fd-159">Client resolves Service A and gets address 10.0.0.1:30000</span></span>
 3. <span data-ttu-id="d87fd-160">Služby A přesune do jiného uzlu.</span><span class="sxs-lookup"><span data-stu-id="d87fd-160">Service A moves to a different node.</span></span>
 4. <span data-ttu-id="d87fd-161">Služba B je umístěn na 10.0.0.1 a shodou používá stejný port 30000.</span><span class="sxs-lookup"><span data-stu-id="d87fd-161">Service B is placed on 10.0.0.1 and coincidentally uses the same port 30000.</span></span>
 5. <span data-ttu-id="d87fd-162">Klient se pokusí připojit k služby A s 10.0.0.1:30000 adres v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d87fd-162">Client attempts to connect to service A with cached address 10.0.0.1:30000.</span></span>
 6. <span data-ttu-id="d87fd-163">Klient je nyní úspěšně připojen na službu, která není porozumění ho B je připojena k nesprávné službě.</span><span class="sxs-lookup"><span data-stu-id="d87fd-163">Client is now successfully connected to service B not realizing it is connected to the wrong service.</span></span>

<span data-ttu-id="d87fd-164">To může způsobit chyby v náhodných intervalech, které může být obtížné diagnostikovat.</span><span class="sxs-lookup"><span data-stu-id="d87fd-164">This can cause bugs at random times that can be difficult to diagnose.</span></span> 

### <a name="using-unique-service-urls"></a><span data-ttu-id="d87fd-165">Pomocí adresy URL jedinečné služby</span><span class="sxs-lookup"><span data-stu-id="d87fd-165">Using unique service URLs</span></span>
<span data-ttu-id="d87fd-166">Chcete-li tomu zabránit, můžete služby účtovat koncový bod služby DNS s jedinečným identifikátorem a potom ověřit tento jedinečný identifikátor během požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="d87fd-166">To prevent this, services can post an endpoint to the Naming Service with a unique identifier, and then validate that unique identifier during client requests.</span></span> <span data-ttu-id="d87fd-167">Toto je akce spolupráci mezi službami v případě důvěryhodného prostředí nepřátelských klienta.</span><span class="sxs-lookup"><span data-stu-id="d87fd-167">This is a cooperative action between services in a non-hostile-tenant trusted environment.</span></span> <span data-ttu-id="d87fd-168">To neposkytuje zabezpečené služby ověřování v prostředí s nepřátelských klienta.</span><span class="sxs-lookup"><span data-stu-id="d87fd-168">This does not provide secure service authentication in a hostile-tenant environment.</span></span>

<span data-ttu-id="d87fd-169">V případě důvěryhodného prostředí middleware, který přidává `UseServiceFabricIntegration` metoda automaticky připojí jedinečný identifikátor pro adresu, která je odeslána do služby DNS a ověří, že identifikátor na každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="d87fd-169">In a trusted environment, the middleware that's added by the `UseServiceFabricIntegration` method automatically appends a unique identifier to the address that is posted to the Naming Service and validates that identifier on each request.</span></span> <span data-ttu-id="d87fd-170">Pokud identifikátor neodpovídá, middleware okamžitě vrátí odpověď HTTP 410 zmizel.</span><span class="sxs-lookup"><span data-stu-id="d87fd-170">If the identifier does not match, the middleware immediately returns an HTTP 410 Gone response.</span></span>

<span data-ttu-id="d87fd-171">Služby využívající port dynamicky přiřadit měli používat tento middlewaru.</span><span class="sxs-lookup"><span data-stu-id="d87fd-171">Services that use a dynamically-assigned port should make use of this middleware.</span></span>

<span data-ttu-id="d87fd-172">Služby, které používají pevné jedinečné číslo portu není nutné tento problém v prostředí spolupráci.</span><span class="sxs-lookup"><span data-stu-id="d87fd-172">Services that use a fixed unique port do not have this problem in a cooperative environment.</span></span> <span data-ttu-id="d87fd-173">Pevné jedinečné číslo portu se obvykle používá pro externě směřujících služby, které potřebují dobře známé port pro klientské aplikace pro připojení k.</span><span class="sxs-lookup"><span data-stu-id="d87fd-173">A fixed unique port is typically used for externally-facing services that need a well-known port for client applications to connect to.</span></span> <span data-ttu-id="d87fd-174">Například většina internetových webových aplikací používat port 80 nebo 443 pro webové prohlížeče připojení.</span><span class="sxs-lookup"><span data-stu-id="d87fd-174">For example, most Internet-facing web applications will use port 80 or 443 for web browser connections.</span></span> <span data-ttu-id="d87fd-175">V takovém případě by nemělo být povoleno jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="d87fd-175">In this case, the unique identifier should not be enabled.</span></span>

<span data-ttu-id="d87fd-176">Následující diagram zobrazuje tok požadavku s middlewarem povoleno:</span><span class="sxs-lookup"><span data-stu-id="d87fd-176">The following diagram shows the request flow with the middleware enabled:</span></span>

![Integrace služby Fabric ASP.NET Core][2]

<span data-ttu-id="d87fd-178">Kestrel a WebListener `ICommunicationListener` implementace použít tento mechanismus stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="d87fd-178">Both Kestrel and WebListener `ICommunicationListener` implementations use this mechanism in exactly the same way.</span></span> <span data-ttu-id="d87fd-179">I když WebListener může odlišovat interně požadavků podle jedinečné cesty adresy URL pomocí základní *http.sys* funkce, která je funkce Sdílení portů *není* používá WebListener `ICommunicationListener` implementace vzhledem k tomu, které způsobí HTTP 503 a HTTP 404 chyba stavové kódy ve scénáři popsané výše.</span><span class="sxs-lookup"><span data-stu-id="d87fd-179">Although WebListener can internally differentiate requests based on unique URL paths using the underlying *http.sys* port sharing feature, that functionality is *not* used by the WebListener `ICommunicationListener` implementation because that will result in HTTP 503 and HTTP 404 error status codes in the scenario described earlier.</span></span> <span data-ttu-id="d87fd-180">Naopak pak bude velmi obtížné pro klienty určit záměr chyby, jako HTTP 503 a HTTP 404 již běžně se používají k označení dalších chyb.</span><span class="sxs-lookup"><span data-stu-id="d87fd-180">That in turn makes it very difficult for clients to determine the intent of the error, as HTTP 503 and HTTP 404 are already commonly used to indicate other errors.</span></span> <span data-ttu-id="d87fd-181">Proto Kestrel i WebListener `ICommunicationListener` implementace standardizovat na middleware poskytované `UseServiceFabricIntegration` metoda rozšíření tak, aby klienti nutné provést pouze koncového bodu služby znovu vyřešit akce HTTP 410 odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d87fd-181">Thus, both Kestrel and WebListener `ICommunicationListener` implementations standardize on the middleware provided by the `UseServiceFabricIntegration` extension method so that clients only need to perform a service endpoint re-resolve action on HTTP 410 responses.</span></span>

## <a name="weblistener-in-reliable-services"></a><span data-ttu-id="d87fd-182">WebListener v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="d87fd-182">WebListener in Reliable Services</span></span>
<span data-ttu-id="d87fd-183">WebListener mohou být používány spolehlivě importováním **Microsoft.ServiceFabric.AspNetCore.WebListener** balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="d87fd-183">WebListener can be used in a Reliable Service by importing the **Microsoft.ServiceFabric.AspNetCore.WebListener** NuGet package.</span></span> <span data-ttu-id="d87fd-184">Tento balíček obsahuje `WebListenerCommunicationListener`, implementace `ICommunicationListener`, který vám umožní vytvořit ASP.NET Core tomuto webovému hostiteli uvnitř spolehlivě pomocí WebListener jako webový server.</span><span class="sxs-lookup"><span data-stu-id="d87fd-184">This package contains `WebListenerCommunicationListener`, an implementation of `ICommunicationListener`, that allows you to create an ASP.NET Core WebHost inside a Reliable Service using WebListener as the web server.</span></span>

<span data-ttu-id="d87fd-185">WebListener je založený na [rozhraní API systému Windows HTTP serveru](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="d87fd-185">WebListener is built on the [Windows HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx).</span></span> <span data-ttu-id="d87fd-186">Tato služba využívá *http.sys* ovladač jádra používaný službou IIS ke zpracování požadavků HTTP a směrovat je pro procesy spuštěné webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d87fd-186">This uses the *http.sys* kernel driver used by IIS to process HTTP requests and route them to processes running web applications.</span></span> <span data-ttu-id="d87fd-187">To umožňuje více procesů na stejný fyzický nebo virtuální počítač na hostitele webové aplikace na stejném portu, od sebe jednoznačně rozlišeny jedinečná cesta adresy URL nebo název hostitele.</span><span class="sxs-lookup"><span data-stu-id="d87fd-187">This allows multiple processes on the same physical or virtual machine to host web applications on the same port, disambiguated by either a unique URL path or hostname.</span></span> <span data-ttu-id="d87fd-188">Tyto funkce jsou užitečné v Service Fabric pro hostování více webů ve stejném clusteru.</span><span class="sxs-lookup"><span data-stu-id="d87fd-188">These features are useful in Service Fabric for hosting multiple websites in the same cluster.</span></span>

<span data-ttu-id="d87fd-189">Následující diagram znázorňuje, jak WebListener používá *http.sys* ovladač jádra v systému Windows pro sdílení portů:</span><span class="sxs-lookup"><span data-stu-id="d87fd-189">The following diagram illustrates how WebListener uses the *http.sys* kernel driver on Windows for port sharing:</span></span>

![ovladač HTTP.sys][3]

### <a name="weblistener-in-a-stateless-service"></a><span data-ttu-id="d87fd-191">WebListener v bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="d87fd-191">WebListener in a stateless service</span></span>
<span data-ttu-id="d87fd-192">Použít `WebListener` v bezstavové služby přepsat `CreateServiceInstanceListeners` metoda a vraťte se `WebListenerCommunicationListener` instance:</span><span class="sxs-lookup"><span data-stu-id="d87fd-192">To use `WebListener` in a stateless service, override the `CreateServiceInstanceListeners` method and return a `WebListenerCommunicationListener` instance:</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseWebListener()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build()))
    };
}
```

### <a name="weblistener-in-a-stateful-service"></a><span data-ttu-id="d87fd-193">WebListener v stavové služby</span><span class="sxs-lookup"><span data-stu-id="d87fd-193">WebListener in a stateful service</span></span>

<span data-ttu-id="d87fd-194">`WebListenerCommunicationListener`není aktuálně určený pro použití v stavové služby z důvodu komplikace se základní *http.sys* funkce Sdílení portů.</span><span class="sxs-lookup"><span data-stu-id="d87fd-194">`WebListenerCommunicationListener` is currently not designed for use in stateful services due to complications with the underlying *http.sys* port sharing feature.</span></span> <span data-ttu-id="d87fd-195">Další informace najdete v následující části na dynamický port přidělení s WebListener.</span><span class="sxs-lookup"><span data-stu-id="d87fd-195">For more information, see the following section on dynamic port allocation with WebListener.</span></span> <span data-ttu-id="d87fd-196">Pro stavové služby je Kestrel doporučené webového serveru.</span><span class="sxs-lookup"><span data-stu-id="d87fd-196">For stateful services, Kestrel is the recommended web server.</span></span>

### <a name="endpoint-configuration"></a><span data-ttu-id="d87fd-197">Konfigurace koncového bodu</span><span class="sxs-lookup"><span data-stu-id="d87fd-197">Endpoint configuration</span></span>

<span data-ttu-id="d87fd-198">`Endpoint` Konfigurace je nutná pro webové servery, které používají rozhraní API systému Windows HTTP serveru, včetně WebListener.</span><span class="sxs-lookup"><span data-stu-id="d87fd-198">An `Endpoint` configuration is required for web servers that use the Windows HTTP Server API, including WebListener.</span></span> <span data-ttu-id="d87fd-199">Webové servery, které používají rozhraní API systému Windows HTTP serveru, musíte nejprve rezervovat jejich adresu URL s *http.sys* (to se obvykle dosahuje [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) nástroj).</span><span class="sxs-lookup"><span data-stu-id="d87fd-199">Web servers that use the Windows HTTP Server API must first reserve their URL with *http.sys* (this is normally accomplished with the [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) tool).</span></span> <span data-ttu-id="d87fd-200">Tato akce vyžaduje zvýšená oprávnění, které nemají vašim službám ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d87fd-200">This action requires elevated privileges that your services by default do not have.</span></span> <span data-ttu-id="d87fd-201">Možnosti "http" nebo "https" `Protocol` vlastnost `Endpoint` konfigurace v *ServiceManifest.xml* používají určený speciálně pro dá pokyn modulu runtime Service Fabric registrace adresy URL s  *ovladač HTTP.sys* na váš jménem pomocí [ *silné zástupné* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) předponu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d87fd-201">The "http" or "https" options for the `Protocol` property of the `Endpoint` configuration in *ServiceManifest.xml* are used specifically to instruct the Service Fabric runtime to register a URL with *http.sys* on your behalf using the [*strong wildcard*](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL prefix.</span></span>

<span data-ttu-id="d87fd-202">Například můžete vyhradit `http://+:80` pro služby, je třeba použít následující konfigurace v ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="d87fd-202">For example, to reserve `http://+:80` for a service, the following configuration should be used in ServiceManifest.xml:</span></span>

```xml
<ServiceManifest ... >
    ...
    <Resources>
        <Endpoints>
            <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>

</ServiceManifest>
```

<span data-ttu-id="d87fd-203">A název koncového bodu musí být předán `WebListenerCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="d87fd-203">And the endpoint name must be passed to the `WebListenerCommunicationListener` constructor:</span></span>

```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     return new WebHostBuilder()
         .UseWebListener()
         .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
         .UseUrls(url)
         .Build();
 })
```

#### <a name="use-weblistener-with-a-static-port"></a><span data-ttu-id="d87fd-204">WebListener pomocí statického portu</span><span class="sxs-lookup"><span data-stu-id="d87fd-204">Use WebListener with a static port</span></span>
<span data-ttu-id="d87fd-205">Pokud chcete používat s WebListener statický port, zadejte číslo portu v `Endpoint` konfigurace:</span><span class="sxs-lookup"><span data-stu-id="d87fd-205">To use a static port with WebListener, provide the port number in the `Endpoint` configuration:</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-weblistener-with-a-dynamic-port"></a><span data-ttu-id="d87fd-206">Pomocí WebListener dynamický port</span><span class="sxs-lookup"><span data-stu-id="d87fd-206">Use WebListener with a dynamic port</span></span>
<span data-ttu-id="d87fd-207">Chcete-li používat dynamicky přiřazeného portu s WebListener, vynechejte `Port` vlastnost v `Endpoint` konfigurace:</span><span class="sxs-lookup"><span data-stu-id="d87fd-207">To use a dynamically assigned port with WebListener, omit the `Port` property in the `Endpoint` configuration:</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="d87fd-208">Všimněte si, že dynamický port přidělena `Endpoint` konfigurace poskytuje jenom jeden port *za hostitelský proces*.</span><span class="sxs-lookup"><span data-stu-id="d87fd-208">Note that a dynamic port allocated by an `Endpoint` configuration only provides one port *per host process*.</span></span> <span data-ttu-id="d87fd-209">Aktuální model hostování Service Fabric umožňuje více instancí služby a repliky pro hostování v rámci jednoho procesu, což znamená, že každé z nich budou sdílet stejný port při přidělení prostřednictvím `Endpoint` konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d87fd-209">The current Service Fabric hosting model allows multiple service instances and/or replicas to be hosted in the same process, meaning that each one will share the same port when allocated through the `Endpoint` configuration.</span></span> <span data-ttu-id="d87fd-210">Více instancí WebListener můžete sdílet port pomocí základní *http.sys* nepodporuje sdílení funkce, ale které portů `WebListenerCommunicationListener` z důvodu komplikace přináší pro požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="d87fd-210">Multiple WebListener instances can share a port using the underlying *http.sys* port sharing feature, but that is not supported by `WebListenerCommunicationListener` due to the complications it introduces for client requests.</span></span> <span data-ttu-id="d87fd-211">Pro používání dynamických portů je Kestrel doporučené webového serveru.</span><span class="sxs-lookup"><span data-stu-id="d87fd-211">For dynamic port usage, Kestrel is the recommended web server.</span></span>

## <a name="kestrel-in-reliable-services"></a><span data-ttu-id="d87fd-212">Kestrel v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="d87fd-212">Kestrel in Reliable Services</span></span>
<span data-ttu-id="d87fd-213">Kestrel mohou být používány spolehlivě importováním **Microsoft.ServiceFabric.AspNetCore.Kestrel** balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="d87fd-213">Kestrel can be used in a Reliable Service by importing the **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet package.</span></span> <span data-ttu-id="d87fd-214">Tento balíček obsahuje `KestrelCommunicationListener`, implementace `ICommunicationListener`, který vám umožní vytvořit ASP.NET Core tomuto webovému hostiteli uvnitř spolehlivě pomocí Kestrel jako webový server.</span><span class="sxs-lookup"><span data-stu-id="d87fd-214">This package contains `KestrelCommunicationListener`, an implementation of `ICommunicationListener`, that allows you to create an ASP.NET Core WebHost inside a Reliable Service using Kestrel as the web server.</span></span>

<span data-ttu-id="d87fd-215">Kestrel je že napříč platformami webový server pro ASP.NET Core podle libuv, knihovny a platformy asynchronní vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="d87fd-215">Kestrel is a cross-platform web server for ASP.NET Core based on libuv, a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="d87fd-216">Na rozdíl od WebListener, Kestrel nepoužívá manažera centralizované koncový bod, jako *http.sys*.</span><span class="sxs-lookup"><span data-stu-id="d87fd-216">Unlike WebListener, Kestrel does not use a centralized endpoint manager such as *http.sys*.</span></span> <span data-ttu-id="d87fd-217">A na rozdíl od WebListener, Kestrel nepodporuje sdílení portů mezi více procesy.</span><span class="sxs-lookup"><span data-stu-id="d87fd-217">And unlike WebListener, Kestrel does not support port sharing between multiple processes.</span></span> <span data-ttu-id="d87fd-218">Každá instance Kestrel musíte použít jedinečnou port.</span><span class="sxs-lookup"><span data-stu-id="d87fd-218">Each instance of Kestrel must use a unique port.</span></span>

![kestrel][4]

### <a name="kestrel-in-a-stateless-service"></a><span data-ttu-id="d87fd-220">Kestrel v bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="d87fd-220">Kestrel in a stateless service</span></span>
<span data-ttu-id="d87fd-221">Použít `Kestrel` v bezstavové služby přepsat `CreateServiceInstanceListeners` metoda a vraťte se `KestrelCommunicationListener` instance:</span><span class="sxs-lookup"><span data-stu-id="d87fd-221">To use `Kestrel` in a stateless service, override the `CreateServiceInstanceListeners` method and return a `KestrelCommunicationListener` instance:</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

### <a name="kestrel-in-a-stateful-service"></a><span data-ttu-id="d87fd-222">Kestrel v stavové služby</span><span class="sxs-lookup"><span data-stu-id="d87fd-222">Kestrel in a stateful service</span></span>
<span data-ttu-id="d87fd-223">Použít `Kestrel` v stavové služby přepsat `CreateServiceReplicaListeners` metoda a vraťte se `KestrelCommunicationListener` instance:</span><span class="sxs-lookup"><span data-stu-id="d87fd-223">To use `Kestrel` in a stateful service, override the `CreateServiceReplicaListeners` method and return a `KestrelCommunicationListener` instance:</span></span>

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new ServiceReplicaListener[]
    {
        new ServiceReplicaListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                         services => services
                             .AddSingleton<StatefulServiceContext>(serviceContext)
                             .AddSingleton<IReliableStateManager>(this.StateManager))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

<span data-ttu-id="d87fd-224">V tomto příkladu instanci typu singleton daného `IReliableStateManager` zajišťuje kontejneru pro vkládání závislosti tomuto webovému hostiteli.</span><span class="sxs-lookup"><span data-stu-id="d87fd-224">In this example, a singleton instance of `IReliableStateManager` is provided to the WebHost dependency injection container.</span></span> <span data-ttu-id="d87fd-225">To není nezbytně nutné, ale umožňuje používat `IReliableStateManager` a spolehlivé kolekcí ve vaší metody akce kontroleru MVC.</span><span class="sxs-lookup"><span data-stu-id="d87fd-225">This is not strictly necessary, but it allows you to use `IReliableStateManager` and Reliable Collections in your MVC controller action methods.</span></span>

<span data-ttu-id="d87fd-226">Všimněte si, že `Endpoint` se název konfigurace **není** poskytované `KestrelCommunicationListener` v stavové služby.</span><span class="sxs-lookup"><span data-stu-id="d87fd-226">Note that an `Endpoint` configuration name is **not** provided to `KestrelCommunicationListener` in a stateful service.</span></span> <span data-ttu-id="d87fd-227">To je vysvětlené podrobněji v následující části.</span><span class="sxs-lookup"><span data-stu-id="d87fd-227">This is explained in more detail in the following section.</span></span>

### <a name="endpoint-configuration"></a><span data-ttu-id="d87fd-228">Konfigurace koncového bodu</span><span class="sxs-lookup"><span data-stu-id="d87fd-228">Endpoint configuration</span></span>
<span data-ttu-id="d87fd-229">`Endpoint` Konfigurace není potřeba použít Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d87fd-229">An `Endpoint` configuration is not required to use Kestrel.</span></span> 

<span data-ttu-id="d87fd-230">Kestrel je jednoduchý samostatný webový server; na rozdíl od WebListener (nebo HttpListener), není nutné `Endpoint` konfigurace v *ServiceManifest.xml* protože nevyžaduje registraci adresy URL před spuštěním.</span><span class="sxs-lookup"><span data-stu-id="d87fd-230">Kestrel is a simple stand-alone web server; unlike WebListener (or HttpListener), it does not need an `Endpoint` configuration in *ServiceManifest.xml* because it does not require URL registration prior to starting.</span></span> 

#### <a name="use-kestrel-with-a-static-port"></a><span data-ttu-id="d87fd-231">Kestrel pomocí statického portu</span><span class="sxs-lookup"><span data-stu-id="d87fd-231">Use Kestrel with a static port</span></span>
<span data-ttu-id="d87fd-232">Statický port se dá nakonfigurovat v `Endpoint` konfigurace ServiceManifest.xml pro použití s Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d87fd-232">A static port can be configured in the `Endpoint` configuration of ServiceManifest.xml for use with Kestrel.</span></span> <span data-ttu-id="d87fd-233">I když to není nezbytně nutné, poskytuje dvě potenciální výhody:</span><span class="sxs-lookup"><span data-stu-id="d87fd-233">Although this is not strictly necessary, it provides two potential benefits:</span></span>
 1. <span data-ttu-id="d87fd-234">Pokud port nespadá rozsah portů aplikace, protože je otevřen přes bránu firewall operačního systému pomocí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d87fd-234">If the port does not fall in the application port range, it is opened through the OS firewall by Service Fabric.</span></span>
 2. <span data-ttu-id="d87fd-235">Adresy URL poskytnuté na vás `KestrelCommunicationListener` bude tento port použít.</span><span class="sxs-lookup"><span data-stu-id="d87fd-235">The URL provided to you through `KestrelCommunicationListener` will use this port.</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="d87fd-236">Pokud `Endpoint` je nakonfigurován, jeho název, musí být předán do `KestrelCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="d87fd-236">If an `Endpoint` is configured, its name must be passed into the `KestrelCommunicationListener` constructor:</span></span> 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

<span data-ttu-id="d87fd-237">Pokud `Endpoint` není použita konfigurace, vypuštění názvu v `KestrelCommunicationListener` konstruktor.</span><span class="sxs-lookup"><span data-stu-id="d87fd-237">If an `Endpoint` configuration is not used, omit the name in the `KestrelCommunicationListener` constructor.</span></span> <span data-ttu-id="d87fd-238">V takovém případě se použije dynamický port.</span><span class="sxs-lookup"><span data-stu-id="d87fd-238">In this case, a dynamic port will be used.</span></span> <span data-ttu-id="d87fd-239">Najdete v části Další informace.</span><span class="sxs-lookup"><span data-stu-id="d87fd-239">See the next section for more information.</span></span>

#### <a name="use-kestrel-with-a-dynamic-port"></a><span data-ttu-id="d87fd-240">Pomocí Kestrel dynamický port</span><span class="sxs-lookup"><span data-stu-id="d87fd-240">Use Kestrel with a dynamic port</span></span>
<span data-ttu-id="d87fd-241">Kestrel nemůžete použít automatické port přiřazení z `Endpoint` konfigurace v ServiceManifest.xml, protože port automatické přiřazení z `Endpoint` konfigurace přiřadí jedinečné port na *hostitelském procesu*, a proces jeden hostitel může obsahovat několik instancí Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d87fd-241">Kestrel cannot use the automatic port assignment from the `Endpoint` configuration in ServiceManifest.xml, because automatic port assignment from an `Endpoint` configuration assigns a unique port per *host process*, and a single host process can contain multiple Kestrel instances.</span></span> <span data-ttu-id="d87fd-242">Vzhledem k tomu, že Kestrel nepodporuje sdílení portů, to nebude fungovat jako každá instance Kestrel musí být otevřen na portu jedinečné.</span><span class="sxs-lookup"><span data-stu-id="d87fd-242">Since Kestrel does not support port sharing, this does not work as each Kestrel instance must be opened on a unique port.</span></span>

<span data-ttu-id="d87fd-243">Pokud chcete použít dynamický port přiřazení s Kestrel, jednoduše vynechejte `Endpoint` konfigurace v ServiceManifest.xml zcela a nepředávejte název koncového bodu na `KestrelCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="d87fd-243">To use dynamic port assignment with Kestrel, simply omit the `Endpoint` configuration in ServiceManifest.xml entirely, and do not pass an endpoint name to the `KestrelCommunicationListener` constructor:</span></span>

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

<span data-ttu-id="d87fd-244">V této konfiguraci `KestrelCommunicationListener` automaticky vybere nepoužitého portu z rozsahu portů aplikace.</span><span class="sxs-lookup"><span data-stu-id="d87fd-244">In this configuration, `KestrelCommunicationListener` will automatically select an unused port from the application port range.</span></span>

## <a name="scenarios-and-configurations"></a><span data-ttu-id="d87fd-245">Scénáře a konfigurace</span><span class="sxs-lookup"><span data-stu-id="d87fd-245">Scenarios and configurations</span></span>
<span data-ttu-id="d87fd-246">Tato část popisuje následující scénáře a poskytuje doporučené kombinace webového serveru, konfiguraci portů, možností integrace Service Fabric a ostatní nastavení dosáhnout správně funguje služby:</span><span class="sxs-lookup"><span data-stu-id="d87fd-246">This section describes the following scenarios and provides the recommended combination of web server, port configuration, Service Fabric integration options, and miscellaneous settings to achieve a properly functioning service:</span></span>
 - <span data-ttu-id="d87fd-247">Externě zveřejněné bezstavové služby ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d87fd-247">Externally exposed ASP.NET Core stateless service</span></span>
 - <span data-ttu-id="d87fd-248">Pouze interní ASP.NET Core bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="d87fd-248">Internal-only ASP.NET Core stateless service</span></span>
 - <span data-ttu-id="d87fd-249">Pouze interní ASP.NET Core stavové služby</span><span class="sxs-lookup"><span data-stu-id="d87fd-249">Internal-only ASP.NET Core stateful service</span></span>

<span data-ttu-id="d87fd-250">**Externě zveřejněné** služba je ten, který zpřístupní koncový bod dosažitelný z mimo cluster, obvykle pomocí služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d87fd-250">An **externally exposed** service is one that exposes an endpoint reachable from outside the cluster, usually through a load balancer.</span></span>

<span data-ttu-id="d87fd-251">**Jen interní** služby je jedním jejichž koncový bod je pouze dosažitelný z v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="d87fd-251">An **internal-only** service is one whose endpoint is only reachable from within the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="d87fd-252">Koncové body stavové služby obecně by neměly přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="d87fd-252">Stateful service endpoints generally should not be exposed to the Internet.</span></span> <span data-ttu-id="d87fd-253">Clustery, které jsou za nástroje pro vyrovnávání zatížení, které neberou v řešení služby Service Fabric, jako je například nástroj pro vyrovnávání zatížení Azure nelze vystavit stavové služby, protože nebude moci vyhledat a směrování provozu na příslušné nástroje pro vyrovnávání zatížení replika stavové služby.</span><span class="sxs-lookup"><span data-stu-id="d87fd-253">Clusters that are behind load balancers that are unaware of Service Fabric service resolution, such as the Azure Load Balancer, will be unable to expose stateful services because the load balancer will not be able to locate and route traffic to the appropriate stateful service replica.</span></span> 

### <a name="externally-exposed-aspnet-core-stateless-services"></a><span data-ttu-id="d87fd-254">Externě zveřejněné bezstavové služby ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d87fd-254">Externally exposed ASP.NET Core stateless services</span></span>
<span data-ttu-id="d87fd-255">WebListener je doporučené webovém serveru pro front-endové služby, které zveřejňují externí, internetových koncových bodů protokolu HTTP v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="d87fd-255">WebListener is the recommended web server for front-end services that expose external, Internet-facing HTTP endpoints on Windows.</span></span> <span data-ttu-id="d87fd-256">Poskytuje lepší ochranu proti útokům a podporuje funkce, které Kestrel neexistuje, jako jsou ověřování systému Windows a sdílení portů.</span><span class="sxs-lookup"><span data-stu-id="d87fd-256">It provides better protection against attacks and supports features that Kestrel does not, such as Windows Authentication and port sharing.</span></span> 

<span data-ttu-id="d87fd-257">V současné době není podporovaný kestrel jako server edge (internetový).</span><span class="sxs-lookup"><span data-stu-id="d87fd-257">Kestrel is not supported as an edge (Internet-facing) server at this time.</span></span> <span data-ttu-id="d87fd-258">Reverzní proxy server, například služby IIS nebo Nginx musí být použitý pro zpracování provozu z veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="d87fd-258">A reverse proxy server such as IIS or Nginx must be used to handle traffic from the public Internet.</span></span>
 
<span data-ttu-id="d87fd-259">Pokud přístup k Internetu, bezstavové služby by měl použít dobře známé a stabilní koncový bod, který je dostupný prostřednictvím Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d87fd-259">When exposed to the Internet, a stateless service should use a well-known and stable endpoint that is reachable through a load balancer.</span></span> <span data-ttu-id="d87fd-260">Toto je adresa URL bude poskytovat uživatelům vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d87fd-260">This is the URL you will provide to users of your application.</span></span> <span data-ttu-id="d87fd-261">Doporučuje se následující konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="d87fd-261">The following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="d87fd-262">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="d87fd-262">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d87fd-263">Webový server</span><span class="sxs-lookup"><span data-stu-id="d87fd-263">Web server</span></span> | <span data-ttu-id="d87fd-264">WebListener</span><span class="sxs-lookup"><span data-stu-id="d87fd-264">WebListener</span></span> | <span data-ttu-id="d87fd-265">Pokud služba je dostupná jenom v případě k důvěryhodné síti, takové intranetu, může být použit Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d87fd-265">If the service is only exposed to a trusted network, such an intranet, Kestrel may be used.</span></span> <span data-ttu-id="d87fd-266">Jinak WebListener je upřednostňovanou možnost.</span><span class="sxs-lookup"><span data-stu-id="d87fd-266">Otherwise, WebListener is the preferred option.</span></span> |
| <span data-ttu-id="d87fd-267">Konfigurace portu</span><span class="sxs-lookup"><span data-stu-id="d87fd-267">Port configuration</span></span> | <span data-ttu-id="d87fd-268">Statické</span><span class="sxs-lookup"><span data-stu-id="d87fd-268">static</span></span> | <span data-ttu-id="d87fd-269">Dobře známé statický port by měl být nakonfigurovaný v `Endpoints` konfigurace ServiceManifest.xml, jako třeba 80 pro protokol HTTP nebo 443 pro protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d87fd-269">A well-known static port should be configured in the `Endpoints` configuration of ServiceManifest.xml, such as 80 for HTTP or 443 for HTTPS.</span></span> |
| <span data-ttu-id="d87fd-270">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="d87fd-270">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="d87fd-271">Žádný</span><span class="sxs-lookup"><span data-stu-id="d87fd-271">None</span></span> | <span data-ttu-id="d87fd-272">`ServiceFabricIntegrationOptions.None` By měl být použit při konfiguraci Service Fabric integrace middleware tak, aby služba nebude pokoušet o ověření příchozích požadavků na jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="d87fd-272">The `ServiceFabricIntegrationOptions.None` option should be used when configuring Service Fabric integration middleware so that the service does not attempt to validate incoming requests for a unique identifier.</span></span> <span data-ttu-id="d87fd-273">Externí uživatele vaší aplikace nebude vědět jedinečné identifikační informace používané middleware.</span><span class="sxs-lookup"><span data-stu-id="d87fd-273">External users of your application will not know the unique identifying information used by the middleware.</span></span> |
| <span data-ttu-id="d87fd-274">Počet instancí</span><span class="sxs-lookup"><span data-stu-id="d87fd-274">Instance Count</span></span> | <span data-ttu-id="d87fd-275">-1</span><span class="sxs-lookup"><span data-stu-id="d87fd-275">-1</span></span> | <span data-ttu-id="d87fd-276">V typické případy použití musí být počet instancí nastavení nastavena "-1", aby instance k dispozici na všech uzlech, jež přijímat přenosy z pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d87fd-276">In typical use cases, the instance count setting should be set to "-1" so that an instance is available on all nodes that receive traffic from a load balancer.</span></span> |

<span data-ttu-id="d87fd-277">Pokud více externě zveřejněné služeb sdílet stejnou sadu uzlů, použije jedinečné, ale stabilní cestu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d87fd-277">If multiple externally exposed services share the same set of nodes, a unique but stable URL path should be used.</span></span> <span data-ttu-id="d87fd-278">To můžete udělat změnou adresy URL poskytnuté při konfiguraci IWebHost.</span><span class="sxs-lookup"><span data-stu-id="d87fd-278">This can be accomplished by modifying the URL provided when configuring IWebHost.</span></span> <span data-ttu-id="d87fd-279">Všimněte si, že platí pouze pro WebListener.</span><span class="sxs-lookup"><span data-stu-id="d87fd-279">Note this applies to WebListener only.</span></span>

 ```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     url += "/MyUniqueServicePath";
 
     return new WebHostBuilder()
         .UseWebListener()
         ...
         .UseUrls(url)
         .Build();
 })
 ```

### <a name="internal-only-stateless-aspnet-core-service"></a><span data-ttu-id="d87fd-280">Pouze interní bezstavové služby ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d87fd-280">Internal-only stateless ASP.NET Core service</span></span>
<span data-ttu-id="d87fd-281">Bezstavové služby, které jsou pouze volat v rámci clusteru musí používat jedinečné adresy URL a dynamicky přiřadit porty k zajištění spolupráce mezi více služeb.</span><span class="sxs-lookup"><span data-stu-id="d87fd-281">Stateless services that are only called from within the cluster should use unique URLs and dynamically assigned ports to ensure cooperation between multiple services.</span></span> <span data-ttu-id="d87fd-282">Doporučuje se následující konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="d87fd-282">The following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="d87fd-283">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="d87fd-283">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d87fd-284">Webový server</span><span class="sxs-lookup"><span data-stu-id="d87fd-284">Web server</span></span> | <span data-ttu-id="d87fd-285">kestrel</span><span class="sxs-lookup"><span data-stu-id="d87fd-285">Kestrel</span></span> | <span data-ttu-id="d87fd-286">I když WebListener mohou být použity pro interní bezstavové služby, je Kestrel doporučené serveru tak, aby víc instancí služby pro sdílení hostitele.</span><span class="sxs-lookup"><span data-stu-id="d87fd-286">Although WebListener may be used for internal stateless services, Kestrel is the recommended server to allow multiple service instances to share a host.</span></span>  |
| <span data-ttu-id="d87fd-287">Konfigurace portu</span><span class="sxs-lookup"><span data-stu-id="d87fd-287">Port configuration</span></span> | <span data-ttu-id="d87fd-288">dynamicky přiřadit</span><span class="sxs-lookup"><span data-stu-id="d87fd-288">dynamically assigned</span></span> | <span data-ttu-id="d87fd-289">Víc replik stavové služby může sdílet proces hostitele nebo hostitelského operačního systému a proto bude nutné odlišné porty.</span><span class="sxs-lookup"><span data-stu-id="d87fd-289">Multiple replicas of a stateful service may share a host process or host operating system and thus will need unique ports.</span></span> |
| <span data-ttu-id="d87fd-290">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="d87fd-290">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="d87fd-291">UseUniqueServiceUrl</span><span class="sxs-lookup"><span data-stu-id="d87fd-291">UseUniqueServiceUrl</span></span> | <span data-ttu-id="d87fd-292">S přiřazením dynamický port toto nastavení zabrání chybné identifikace problém popsané výše.</span><span class="sxs-lookup"><span data-stu-id="d87fd-292">With dynamic port assignment, this setting prevents the mistaken identity issue described earlier.</span></span> |
| <span data-ttu-id="d87fd-293">InstanceCount</span><span class="sxs-lookup"><span data-stu-id="d87fd-293">InstanceCount</span></span> | <span data-ttu-id="d87fd-294">všechny</span><span class="sxs-lookup"><span data-stu-id="d87fd-294">any</span></span> | <span data-ttu-id="d87fd-295">Počet instancí nastavení lze nastavit na jakoukoli hodnotu potřebné pro provoz služby.</span><span class="sxs-lookup"><span data-stu-id="d87fd-295">The instance count setting can be set to any value necessary to operate the service.</span></span> |

### <a name="internal-only-stateful-aspnet-core-service"></a><span data-ttu-id="d87fd-296">Pouze interní stavová služba ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d87fd-296">Internal-only stateful ASP.NET Core service</span></span>
<span data-ttu-id="d87fd-297">Stavové služby, které jsou pouze volat v rámci clusteru používejte porty dynamicky přiřazené k zajištění spolupráce mezi více služeb.</span><span class="sxs-lookup"><span data-stu-id="d87fd-297">Stateful services that are only called from within the cluster should use dynamically assigned ports to ensure cooperation between multiple services.</span></span> <span data-ttu-id="d87fd-298">Doporučuje se následující konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="d87fd-298">The following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="d87fd-299">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="d87fd-299">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d87fd-300">Webový server</span><span class="sxs-lookup"><span data-stu-id="d87fd-300">Web server</span></span> | <span data-ttu-id="d87fd-301">kestrel</span><span class="sxs-lookup"><span data-stu-id="d87fd-301">Kestrel</span></span> | <span data-ttu-id="d87fd-302">`WebListenerCommunicationListener` Není určen pro stavové služby, ve kterých repliky sdílet hostitelském procesu.</span><span class="sxs-lookup"><span data-stu-id="d87fd-302">The `WebListenerCommunicationListener` is not designed for use by stateful services in which replicas share a host process.</span></span> |
| <span data-ttu-id="d87fd-303">Konfigurace portu</span><span class="sxs-lookup"><span data-stu-id="d87fd-303">Port configuration</span></span> | <span data-ttu-id="d87fd-304">dynamicky přiřadit</span><span class="sxs-lookup"><span data-stu-id="d87fd-304">dynamically assigned</span></span> | <span data-ttu-id="d87fd-305">Víc replik stavové služby může sdílet proces hostitele nebo hostitelského operačního systému a proto bude nutné odlišné porty.</span><span class="sxs-lookup"><span data-stu-id="d87fd-305">Multiple replicas of a stateful service may share a host process or host operating system and thus will need unique ports.</span></span> |
| <span data-ttu-id="d87fd-306">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="d87fd-306">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="d87fd-307">UseUniqueServiceUrl</span><span class="sxs-lookup"><span data-stu-id="d87fd-307">UseUniqueServiceUrl</span></span> | <span data-ttu-id="d87fd-308">S přiřazením dynamický port toto nastavení zabrání chybné identifikace problém popsané výše.</span><span class="sxs-lookup"><span data-stu-id="d87fd-308">With dynamic port assignment, this setting prevents the mistaken identity issue described earlier.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d87fd-309">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d87fd-309">Next steps</span></span>
[<span data-ttu-id="d87fd-310">Ladění aplikace Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d87fd-310">Debug your Service Fabric application by using Visual Studio</span></span>](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
