---
title: aaaService komunikace s hello ASP.NET Core | Microsoft Docs
description: "Zjistěte, jak toouse základní technologie ASP.NET v bezzstavovými i stavovými službami spolehlivé."
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
ms.openlocfilehash: 6e6a83ab04390150292f63de5d9b51d290284e50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-core-in-service-fabric-reliable-services"></a><span data-ttu-id="ec21d-103">Jádro ASP.NET v Service Fabric spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="ec21d-103">ASP.NET Core in Service Fabric Reliable Services</span></span>

<span data-ttu-id="ec21d-104">ASP.NET Core je nové open source a napříč platformami architektura pro vytváření moderní cloudové aplikace založené na připojené k Internetu, jako třeba webové aplikace, aplikace IoT a back-EndY mobilních.</span><span class="sxs-lookup"><span data-stu-id="ec21d-104">ASP.NET Core is a new open-source and cross-platform framework for building modern cloud-based Internet-connected applications, such as web apps, IoT apps, and mobile backends.</span></span> 

<span data-ttu-id="ec21d-105">Tento článek slouží podrobný průvodce toohosting ASP.NET Core služby v Service Fabric spolehlivé služby pomocí hello **Microsoft.ServiceFabric.AspNetCore.** * sadu balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="ec21d-105">This article is an in-depth guide toohosting ASP.NET Core services in Service Fabric Reliable Services using hello **Microsoft.ServiceFabric.AspNetCore.*** set of NuGet packages.</span></span>

<span data-ttu-id="ec21d-106">Úvodní kurz ASP.NET Core v Service Fabric a pokyny pro nastavení vývojového prostředí najdete v tématu [vytváření webového front-endu vaší aplikace pomocí ASP.NET Core](service-fabric-add-a-web-frontend.md).</span><span class="sxs-lookup"><span data-stu-id="ec21d-106">For an introductory tutorial on ASP.NET Core in Service Fabric and instructions on getting your development environment set up, see [Building a web front-end for your application using ASP.NET Core](service-fabric-add-a-web-frontend.md).</span></span>

<span data-ttu-id="ec21d-107">Hello zbývající části tohoto článku předpokládá, že jste již obeznámeni s ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ec21d-107">hello rest of this article assumes you are already familiar with ASP.NET Core.</span></span> <span data-ttu-id="ec21d-108">Pokud ne, doporučujeme čtení prostřednictvím hello [ASP.NET Core Základy](https://docs.microsoft.com/aspnet/core/fundamentals/index).</span><span class="sxs-lookup"><span data-stu-id="ec21d-108">If not, we recommend reading through hello [ASP.NET Core fundamentals](https://docs.microsoft.com/aspnet/core/fundamentals/index).</span></span>

## <a name="aspnet-core-in-hello-service-fabric-environment"></a><span data-ttu-id="ec21d-109">ASP.NET Core v prostředí Service Fabric hello</span><span class="sxs-lookup"><span data-stu-id="ec21d-109">ASP.NET Core in hello Service Fabric environment</span></span>

<span data-ttu-id="ec21d-110">Když aplikace ASP.NET Core můžete spustit na .NET Core nebo hello úplné rozhraní .NET Framework, Service Fabric služby aktuálně lze spustit jen u hello úplné rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ec21d-110">While ASP.NET Core apps can run on .NET Core or on hello full .NET Framework, Service Fabric services currently can only run on hello full .NET Framework.</span></span> <span data-ttu-id="ec21d-111">To znamená, když vytváříte služby ASP.NET Core Service Fabric, je potřeba stále cílit hello úplné rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ec21d-111">This means when you build an ASP.NET  Core Service Fabric service, you must still target hello full .NET Framework.</span></span>

<span data-ttu-id="ec21d-112">ASP.NET Core lze použít dvěma různými způsoby v Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="ec21d-112">ASP.NET Core can be used in two different ways in Service Fabric:</span></span>
 - <span data-ttu-id="ec21d-113">**Hostovaný jako spustitelný soubor hosta**.</span><span class="sxs-lookup"><span data-stu-id="ec21d-113">**Hosted as a guest executable**.</span></span> <span data-ttu-id="ec21d-114">Toto je primárně používané toorun stávající ASP.NET Core aplikace v Service Fabric beze změn kódu.</span><span class="sxs-lookup"><span data-stu-id="ec21d-114">This is primarily used toorun existing ASP.NET Core applications on Service Fabric with no code changes.</span></span>
 - <span data-ttu-id="ec21d-115">**Spustit uvnitř spolehlivě**.</span><span class="sxs-lookup"><span data-stu-id="ec21d-115">**Run inside a Reliable Service**.</span></span> <span data-ttu-id="ec21d-116">To umožňuje lepší integraci s hello modulu runtime Service Fabric a umožňuje stavová ASP.NET Core services.</span><span class="sxs-lookup"><span data-stu-id="ec21d-116">This allows better integration with hello Service Fabric runtime and allows stateful ASP.NET Core services.</span></span>

<span data-ttu-id="ec21d-117">Hello zbytek Tento článek vysvětluje, jak toouse ASP.NET Core uvnitř spolehlivou službu pomocí hello komponenty integrace ASP.NET Core, které jsou součástí s hello Service Fabric SDK.</span><span class="sxs-lookup"><span data-stu-id="ec21d-117">hello rest of this article explains how toouse ASP.NET Core inside a Reliable Service using hello ASP.NET Core integration components that ship with hello Service Fabric SDK.</span></span> 

## <a name="service-fabric-service-hosting"></a><span data-ttu-id="ec21d-118">Hostování služeb Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ec21d-118">Service Fabric service hosting</span></span>

<span data-ttu-id="ec21d-119">V Service Fabric, jeden nebo více instancí a/nebo repliky služby běžet v *procesu hostitele služby*, spustitelný soubor, který spouští službu kódu.</span><span class="sxs-lookup"><span data-stu-id="ec21d-119">In Service Fabric, one or more instances and/or replicas of your service run in a *service host process*, an executable file that runs your service code.</span></span> <span data-ttu-id="ec21d-120">Jako autor služby vlastníte hello služby hostitelský proces a Service Fabric aktivuje a sleduje za vás.</span><span class="sxs-lookup"><span data-stu-id="ec21d-120">You, as a service author, own hello service host process and Service Fabric activates and monitors it for you.</span></span>

<span data-ttu-id="ec21d-121">Tradiční ASP.NET (až tooMVC 5) je úzce párované tooIIS prostřednictvím System.Web.dll.</span><span class="sxs-lookup"><span data-stu-id="ec21d-121">Traditional ASP.NET (up tooMVC 5) is tightly coupled tooIIS through System.Web.dll.</span></span> <span data-ttu-id="ec21d-122">ASP.NET Core zajišťuje oddělení mezi hello webového serveru a webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec21d-122">ASP.NET Core provides a separation between hello web server and your web application.</span></span> <span data-ttu-id="ec21d-123">To umožňuje webové aplikace toobe přenosné mezi jiné webové servery a také umožňuje webové servery toobe *hostovanou na vlastním*, což znamená, že můžete spustit webový server v vlastní procesu, jako názvem na rozdíl od tooa proces, který je vlastněn vyhrazený webový software serveru jako je například IIS.</span><span class="sxs-lookup"><span data-stu-id="ec21d-123">This allows web applications toobe portable between different web servers and also allows web servers toobe *self-hosted*, which means you can start a web server in your own process, as opposed tooa process that is owned by dedicated web server software such as IIS.</span></span> 

<span data-ttu-id="ec21d-124">V pořadí toocombine služby Service Fabric a ASP.NET, jako spustitelný soubor hosta nebo v spolehlivě, musí být schopný toostart ASP.NET uvnitř vaší služby hostitelský proces.</span><span class="sxs-lookup"><span data-stu-id="ec21d-124">In order toocombine a Service Fabric service and ASP.NET, either as a guest executable or in a Reliable Service, you must be able toostart ASP.NET inside your service host process.</span></span> <span data-ttu-id="ec21d-125">Vlastní hostování ASP.NET Core vám umožní toodo to.</span><span class="sxs-lookup"><span data-stu-id="ec21d-125">ASP.NET Core self-hosting allows you toodo this.</span></span>

## <a name="hosting-aspnet-core-in-a-reliable-service"></a><span data-ttu-id="ec21d-126">Hostování ASP.NET Core v spolehlivě.</span><span class="sxs-lookup"><span data-stu-id="ec21d-126">Hosting ASP.NET Core in a Reliable Service</span></span>
<span data-ttu-id="ec21d-127">Obvykle vlastním hostováním aplikací ASP.NET Core vytvořte tomuto webovému hostiteli v vstupní bod aplikace, jako je například hello `static void Main()` metoda v `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="ec21d-127">Typically, self-hosted ASP.NET Core applications create a WebHost in an application's entry point, such as hello `static void Main()` method in `Program.cs`.</span></span> <span data-ttu-id="ec21d-128">Životní cyklus hello hello webového hostitele v tomto případě je vázané toohello životního cyklu hello procesu.</span><span class="sxs-lookup"><span data-stu-id="ec21d-128">In this case, hello lifecycle of hello WebHost is bound toohello lifecycle of hello process.</span></span>

![Hostování v procesu ASP.NET Core][0]

<span data-ttu-id="ec21d-130">Však vstupní bod aplikace hello není hello správném místě toocreate webového hostitele v spolehlivě, protože se používá pouze vstupní bod aplikace hello tooregister služby zadejte pomocí modulu runtime Service Fabric hello, tak, aby ji může vytvořit instance této služby typ.</span><span class="sxs-lookup"><span data-stu-id="ec21d-130">However, hello application entry point is not hello right place toocreate a WebHost in a Reliable Service, because hello application entry point is only used tooregister a service type with hello Service Fabric runtime, so that it may create instances of that service type.</span></span> <span data-ttu-id="ec21d-131">Hello webového hostitele má být vytvořena v spolehlivě sám sebe.</span><span class="sxs-lookup"><span data-stu-id="ec21d-131">hello WebHost should be created in a Reliable Service itself.</span></span> <span data-ttu-id="ec21d-132">V rámci procesu hostitele služby hello instance služby nebo repliky můžete projít více životní cykly.</span><span class="sxs-lookup"><span data-stu-id="ec21d-132">Within hello service host process, service instances and/or replicas can go through multiple lifecycles.</span></span> 

<span data-ttu-id="ec21d-133">Instance spolehlivé služby je reprezentována třídě služby odvozování z `StatelessService` nebo `StatefulService`.</span><span class="sxs-lookup"><span data-stu-id="ec21d-133">A Reliable Service instance is represented by your service class deriving from `StatelessService` or `StatefulService`.</span></span> <span data-ttu-id="ec21d-134">je součástí Hello komunikačního balíku pro službu `ICommunicationListener` implementace ve třídě služby.</span><span class="sxs-lookup"><span data-stu-id="ec21d-134">hello communication stack for a service is contained in an `ICommunicationListener` implementation in your service class.</span></span> <span data-ttu-id="ec21d-135">Hello `Microsoft.ServiceFabric.Services.AspNetCore.*` balíčky NuGet obsahovat implementace `ICommunicationListener` , spouštět a spravovat hello ASP.NET Core tomuto webovému hostiteli pro Kestrel nebo WebListener v spolehlivě.</span><span class="sxs-lookup"><span data-stu-id="ec21d-135">hello `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet packages contain implementations of `ICommunicationListener` that start and manage hello ASP.NET Core WebHost for either Kestrel or WebListener in a Reliable Service.</span></span>

![Hostování ASP.NET Core v spolehlivě.][1]

## <a name="aspnet-core-icommunicationlisteners"></a><span data-ttu-id="ec21d-137">ICommunicationListeners ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec21d-137">ASP.NET Core ICommunicationListeners</span></span>
<span data-ttu-id="ec21d-138">Hello `ICommunicationListener` implementace pro Kestrel a WebListener v hello `Microsoft.ServiceFabric.Services.AspNetCore.*` balíčky NuGet podobné použití vzorce ale provádět mírně odlišné akce konkrétní tooeach webový server.</span><span class="sxs-lookup"><span data-stu-id="ec21d-138">hello `ICommunicationListener` implementations for Kestrel and WebListener in hello  `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet packages have similar use patterns but perform slightly different actions specific tooeach web server.</span></span> 

<span data-ttu-id="ec21d-139">Obě naslouchací procesy komunikace poskytují konstruktor, který přebírá hello následující argumenty:</span><span class="sxs-lookup"><span data-stu-id="ec21d-139">Both communication listeners provide a constructor that takes hello following arguments:</span></span>
 - <span data-ttu-id="ec21d-140">**`ServiceContext serviceContext`**: hello `ServiceContext` objekt, který obsahuje informace o hello službou.</span><span class="sxs-lookup"><span data-stu-id="ec21d-140">**`ServiceContext serviceContext`**: hello `ServiceContext` object that contains information about hello running service.</span></span>
 - <span data-ttu-id="ec21d-141">**`string endpointName`**: název hello `Endpoint` konfigurace v ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="ec21d-141">**`string endpointName`**: hello name of an `Endpoint` configuration in ServiceManifest.xml.</span></span> <span data-ttu-id="ec21d-142">Toto je primárně kde hello dva naslouchací procesy komunikace liší: WebListener **vyžaduje** `Endpoint` konfigurace, zatímco Kestrel neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ec21d-142">This is primarily where hello two communication listeners differ: WebListener **requires** an `Endpoint` configuration, while Kestrel does not.</span></span>
 - <span data-ttu-id="ec21d-143">**`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: lambda, který implementujete, ve kterém se vytváří a zpět `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="ec21d-143">**`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: a lambda that you implement in which you create and return an `IWebHost`.</span></span> <span data-ttu-id="ec21d-144">To vám umožní tooconfigure `IWebHost` hello způsob za normálních okolností byste v aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ec21d-144">This allows you tooconfigure `IWebHost` hello way you normally would in an ASP.NET Core application.</span></span> <span data-ttu-id="ec21d-145">Hello lambda poskytuje adresy URL, která je generována pro v závislosti na integraci Service Fabric hello možnosti, můžete použít a hello `Endpoint` konfigurace, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="ec21d-145">hello lambda provides a URL which is generated for you depending on hello Service Fabric integration options you use and hello `Endpoint` configuration you provide.</span></span> <span data-ttu-id="ec21d-146">Aby adresa URL lze potom změnit nebo použít jako-je toostart hello webovým serverem.</span><span class="sxs-lookup"><span data-stu-id="ec21d-146">That URL can then be modified or used as-is toostart hello web server.</span></span>

## <a name="service-fabric-integration-middleware"></a><span data-ttu-id="ec21d-147">Middleware integrace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ec21d-147">Service Fabric integration middleware</span></span>
<span data-ttu-id="ec21d-148">Hello `Microsoft.ServiceFabric.Services.AspNetCore` balíček NuGet obsahuje hello `UseServiceFabricIntegration` rozšiřující metody na `IWebHostBuilder` , přidá middleware podporující služby prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="ec21d-148">hello `Microsoft.ServiceFabric.Services.AspNetCore` NuGet package includes hello `UseServiceFabricIntegration` extension method on `IWebHostBuilder` that adds Service Fabric-aware middleware.</span></span> <span data-ttu-id="ec21d-149">Tento middleware nakonfiguruje hello Kestrel nebo WebListener `ICommunicationListener` tooregister s adresou URL jedinečné služby hello Service Fabric Naming Service a poté ověří klienta požadavky tooensure klienti se připojují toohello správné služby.</span><span class="sxs-lookup"><span data-stu-id="ec21d-149">This middleware configures hello Kestrel or WebListener `ICommunicationListener` tooregister a unique service URL with hello Service Fabric Naming Service and then validates client requests tooensure clients are connecting toohello right service.</span></span> <span data-ttu-id="ec21d-150">To je nezbytné v prostředí s sdílené hostitele, jako jsou Service Fabric, kde můžete spustit na hello stejný fyzický nebo virtuální počítač více webových aplikací, ale nepoužívejte názvy hostitelů jedinečný, klienti tooprevent omylem připojení toohello nesprávnou službu.</span><span class="sxs-lookup"><span data-stu-id="ec21d-150">This is necessary in a shared-host environment such as Service Fabric, where multiple web applications can run on hello same physical or virtual machine but do not use unique host names, tooprevent clients from mistakenly connecting toohello wrong service.</span></span> <span data-ttu-id="ec21d-151">Tento scénář je podrobně popsaná v další v další části hello.</span><span class="sxs-lookup"><span data-stu-id="ec21d-151">This scenario is described in more detail in hello next section.</span></span>

### <a name="a-case-of-mistaken-identity"></a><span data-ttu-id="ec21d-152">V případě chybné identifikace</span><span class="sxs-lookup"><span data-stu-id="ec21d-152">A case of mistaken identity</span></span>
<span data-ttu-id="ec21d-153">Repliky služby, bez ohledu na protokol, naslouchat na kombinaci jedinečné IP: port.</span><span class="sxs-lookup"><span data-stu-id="ec21d-153">Service replicas, regardless of protocol, listen on a unique IP:port combination.</span></span> <span data-ttu-id="ec21d-154">Jakmile repliku služby bylo zahájeno naslouchání na koncový bod IP: port, hlásí adresu toohello tento koncový bod prostředků infrastruktury pojmenování služby kde mohou být zjištěny klienti nebo jiné služby.</span><span class="sxs-lookup"><span data-stu-id="ec21d-154">Once a service replica has started listening on an IP:port endpoint, it reports that endpoint address toohello Service Fabric Naming Service where it can be discovered by clients or other services.</span></span> <span data-ttu-id="ec21d-155">Pokud služby používají, které porty dynamicky přiřadit aplikaci, repliku služby může použít shodou hello stejný koncový bod IP: port jiné službě, která byla předtím na hello stejný fyzický nebo virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ec21d-155">If services use dynamically-assigned application ports, a service replica may coincidentally use hello same IP:port endpoint of another service that was previously on hello same physical or virtual machine.</span></span> <span data-ttu-id="ec21d-156">To může způsobit klienta toomistakely připojit toohello nesprávnou službu.</span><span class="sxs-lookup"><span data-stu-id="ec21d-156">This can cause a client toomistakely connect toohello wrong service.</span></span> <span data-ttu-id="ec21d-157">K tomu může dojít, pokud dojde k hello následující posloupnost událostí:</span><span class="sxs-lookup"><span data-stu-id="ec21d-157">This can happen if hello following sequence of events occur:</span></span>

 1. <span data-ttu-id="ec21d-158">Služby A naslouchá na 10.0.0.1:30000 přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="ec21d-158">Service A listens on 10.0.0.1:30000 over HTTP.</span></span> 
 2. <span data-ttu-id="ec21d-159">Klienta přeloží A služby a získá adresu 10.0.0.1:30000</span><span class="sxs-lookup"><span data-stu-id="ec21d-159">Client resolves Service A and gets address 10.0.0.1:30000</span></span>
 3. <span data-ttu-id="ec21d-160">Služby A přesune tooa jiný uzel.</span><span class="sxs-lookup"><span data-stu-id="ec21d-160">Service A moves tooa different node.</span></span>
 4. <span data-ttu-id="ec21d-161">Služba B je umístěn na 10.0.0.1 a shodou hello používá stejný port 30000.</span><span class="sxs-lookup"><span data-stu-id="ec21d-161">Service B is placed on 10.0.0.1 and coincidentally uses hello same port 30000.</span></span>
 5. <span data-ttu-id="ec21d-162">Klient se pokusí o tooconnect tooservice A s 10.0.0.1:30000 adres v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ec21d-162">Client attempts tooconnect tooservice A with cached address 10.0.0.1:30000.</span></span>
 6. <span data-ttu-id="ec21d-163">Klient je nyní připojen úspěšně připojeno tooservice B není porozumění ho toohello nesprávnou službu.</span><span class="sxs-lookup"><span data-stu-id="ec21d-163">Client is now successfully connected tooservice B not realizing it is connected toohello wrong service.</span></span>

<span data-ttu-id="ec21d-164">To může způsobit chyby v náhodných intervalech, které může být obtížné toodiagnose.</span><span class="sxs-lookup"><span data-stu-id="ec21d-164">This can cause bugs at random times that can be difficult toodiagnose.</span></span> 

### <a name="using-unique-service-urls"></a><span data-ttu-id="ec21d-165">Pomocí adresy URL jedinečné služby</span><span class="sxs-lookup"><span data-stu-id="ec21d-165">Using unique service URLs</span></span>
<span data-ttu-id="ec21d-166">tooprevent služby, můžete odeslat koncový bod toohello Naming Service s jedinečným identifikátorem a její ověření tento jedinečný identifikátor během požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="ec21d-166">tooprevent this, services can post an endpoint toohello Naming Service with a unique identifier, and then validate that unique identifier during client requests.</span></span> <span data-ttu-id="ec21d-167">Toto je akce spolupráci mezi službami v případě důvěryhodného prostředí nepřátelských klienta.</span><span class="sxs-lookup"><span data-stu-id="ec21d-167">This is a cooperative action between services in a non-hostile-tenant trusted environment.</span></span> <span data-ttu-id="ec21d-168">To neposkytuje zabezpečené služby ověřování v prostředí s nepřátelských klienta.</span><span class="sxs-lookup"><span data-stu-id="ec21d-168">This does not provide secure service authentication in a hostile-tenant environment.</span></span>

<span data-ttu-id="ec21d-169">V případě důvěryhodného prostředí hello middleware, který přidává hello `UseServiceFabricIntegration` metoda automaticky připojí adresu toohello jedinečný identifikátor, která je odeslána toohello zásady vytváření názvů služby a ověří, že identifikátor na každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="ec21d-169">In a trusted environment, hello middleware that's added by hello `UseServiceFabricIntegration` method automatically appends a unique identifier toohello address that is posted toohello Naming Service and validates that identifier on each request.</span></span> <span data-ttu-id="ec21d-170">Pokud hello identifikátor neodpovídá, hello middleware okamžitě vrátí odpověď HTTP 410 zmizel.</span><span class="sxs-lookup"><span data-stu-id="ec21d-170">If hello identifier does not match, hello middleware immediately returns an HTTP 410 Gone response.</span></span>

<span data-ttu-id="ec21d-171">Služby využívající port dynamicky přiřadit měli používat tento middlewaru.</span><span class="sxs-lookup"><span data-stu-id="ec21d-171">Services that use a dynamically-assigned port should make use of this middleware.</span></span>

<span data-ttu-id="ec21d-172">Služby, které používají pevné jedinečné číslo portu není nutné tento problém v prostředí spolupráci.</span><span class="sxs-lookup"><span data-stu-id="ec21d-172">Services that use a fixed unique port do not have this problem in a cooperative environment.</span></span> <span data-ttu-id="ec21d-173">Pevné jedinečné číslo portu se obvykle používá pro externě směřujících služby, které pro klientské aplikace tooconnect k potřebovat dobře známého portu.</span><span class="sxs-lookup"><span data-stu-id="ec21d-173">A fixed unique port is typically used for externally-facing services that need a well-known port for client applications tooconnect to.</span></span> <span data-ttu-id="ec21d-174">Například většina internetových webových aplikací používat port 80 nebo 443 pro webové prohlížeče připojení.</span><span class="sxs-lookup"><span data-stu-id="ec21d-174">For example, most Internet-facing web applications will use port 80 or 443 for web browser connections.</span></span> <span data-ttu-id="ec21d-175">V takovém případě by nemělo být povoleno hello jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ec21d-175">In this case, hello unique identifier should not be enabled.</span></span>

<span data-ttu-id="ec21d-176">Hello následující diagram znázorňuje tok požadavku hello s hello middleware povoleno:</span><span class="sxs-lookup"><span data-stu-id="ec21d-176">hello following diagram shows hello request flow with hello middleware enabled:</span></span>

![Integrace služby Fabric ASP.NET Core][2]

<span data-ttu-id="ec21d-178">Kestrel a WebListener `ICommunicationListener` implementace použít tento mechanismus v přesně hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="ec21d-178">Both Kestrel and WebListener `ICommunicationListener` implementations use this mechanism in exactly hello same way.</span></span> <span data-ttu-id="ec21d-179">I když WebListener může odlišovat interně požadavků podle jedinečné cesty adresy URL pomocí hello základní *http.sys* funkce, která je funkce Sdílení portů *není* používané hello WebListener `ICommunicationListener` implementace vzhledem k tomu, které způsobí HTTP 503 a HTTP 404 chybové kódy stavu v hello scénář popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="ec21d-179">Although WebListener can internally differentiate requests based on unique URL paths using hello underlying *http.sys* port sharing feature, that functionality is *not* used by hello WebListener `ICommunicationListener` implementation because that will result in HTTP 503 and HTTP 404 error status codes in hello scenario described earlier.</span></span> <span data-ttu-id="ec21d-180">Naopak pak bude velmi obtížné pro klienty toodetermine hello záměr hello chyby, jako HTTP 503 a HTTP 404, se již běžně používané tooindicate dalších chyb.</span><span class="sxs-lookup"><span data-stu-id="ec21d-180">That in turn makes it very difficult for clients toodetermine hello intent of hello error, as HTTP 503 and HTTP 404 are already commonly used tooindicate other errors.</span></span> <span data-ttu-id="ec21d-181">Proto Kestrel i WebListener `ICommunicationListener` implementace standardizovat na hello middleware poskytované hello `UseServiceFabricIntegration` metoda rozšíření tak, aby klienti, stačí tooperform koncového bodu služby znovu vyřešit akce HTTP 410 odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ec21d-181">Thus, both Kestrel and WebListener `ICommunicationListener` implementations standardize on hello middleware provided by hello `UseServiceFabricIntegration` extension method so that clients only need tooperform a service endpoint re-resolve action on HTTP 410 responses.</span></span>

## <a name="weblistener-in-reliable-services"></a><span data-ttu-id="ec21d-182">WebListener v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="ec21d-182">WebListener in Reliable Services</span></span>
<span data-ttu-id="ec21d-183">WebListener mohou být používány spolehlivě importováním hello **Microsoft.ServiceFabric.AspNetCore.WebListener** balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="ec21d-183">WebListener can be used in a Reliable Service by importing hello **Microsoft.ServiceFabric.AspNetCore.WebListener** NuGet package.</span></span> <span data-ttu-id="ec21d-184">Tento balíček obsahuje `WebListenerCommunicationListener`, implementace `ICommunicationListener`, což vám umožní toocreate ASP.NET Core tomuto webovému hostiteli uvnitř spolehlivě pomocí WebListener jako hello webový server.</span><span class="sxs-lookup"><span data-stu-id="ec21d-184">This package contains `WebListenerCommunicationListener`, an implementation of `ICommunicationListener`, that allows you toocreate an ASP.NET Core WebHost inside a Reliable Service using WebListener as hello web server.</span></span>

<span data-ttu-id="ec21d-185">WebListener je založený na hello [rozhraní API systému Windows HTTP serveru](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="ec21d-185">WebListener is built on hello [Windows HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx).</span></span> <span data-ttu-id="ec21d-186">Tato služba využívá hello *http.sys* ovladač jádra, které používá IIS tooprocess HTTP požadavků a směrovat je tooprocesses spuštění webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec21d-186">This uses hello *http.sys* kernel driver used by IIS tooprocess HTTP requests and route them tooprocesses running web applications.</span></span> <span data-ttu-id="ec21d-187">Díky tomu mohou více procesů v hello stejný fyzický nebo virtuální počítač toohost webové aplikace na hello stejný port, od sebe jednoznačně rozlišeny jedinečná cesta adresy URL nebo název hostitele.</span><span class="sxs-lookup"><span data-stu-id="ec21d-187">This allows multiple processes on hello same physical or virtual machine toohost web applications on hello same port, disambiguated by either a unique URL path or hostname.</span></span> <span data-ttu-id="ec21d-188">Tyto funkce jsou užitečné v Service Fabric pro hostování více webů v hello stejného clusteru.</span><span class="sxs-lookup"><span data-stu-id="ec21d-188">These features are useful in Service Fabric for hosting multiple websites in hello same cluster.</span></span>

<span data-ttu-id="ec21d-189">Hello následující diagram znázorňuje, jak WebListener používá hello *http.sys* ovladač jádra v systému Windows pro sdílení portů:</span><span class="sxs-lookup"><span data-stu-id="ec21d-189">hello following diagram illustrates how WebListener uses hello *http.sys* kernel driver on Windows for port sharing:</span></span>

![ovladač HTTP.sys][3]

### <a name="weblistener-in-a-stateless-service"></a><span data-ttu-id="ec21d-191">WebListener v bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="ec21d-191">WebListener in a stateless service</span></span>
<span data-ttu-id="ec21d-192">toouse `WebListener` v bezstavové služby přepsat hello `CreateServiceInstanceListeners` metoda a vraťte se `WebListenerCommunicationListener` instance:</span><span class="sxs-lookup"><span data-stu-id="ec21d-192">toouse `WebListener` in a stateless service, override hello `CreateServiceInstanceListeners` method and return a `WebListenerCommunicationListener` instance:</span></span>

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

### <a name="weblistener-in-a-stateful-service"></a><span data-ttu-id="ec21d-193">WebListener v stavové služby</span><span class="sxs-lookup"><span data-stu-id="ec21d-193">WebListener in a stateful service</span></span>

<span data-ttu-id="ec21d-194">`WebListenerCommunicationListener`není aktuálně určený pro použití v stavové služby kvůli toocomplications s hello základní *http.sys* funkce Sdílení portů.</span><span class="sxs-lookup"><span data-stu-id="ec21d-194">`WebListenerCommunicationListener` is currently not designed for use in stateful services due toocomplications with hello underlying *http.sys* port sharing feature.</span></span> <span data-ttu-id="ec21d-195">Další informace najdete v následující části na dynamický port přidělení s WebListener hello.</span><span class="sxs-lookup"><span data-stu-id="ec21d-195">For more information, see hello following section on dynamic port allocation with WebListener.</span></span> <span data-ttu-id="ec21d-196">Pro stavové služby je Kestrel hello doporučená webový server.</span><span class="sxs-lookup"><span data-stu-id="ec21d-196">For stateful services, Kestrel is hello recommended web server.</span></span>

### <a name="endpoint-configuration"></a><span data-ttu-id="ec21d-197">Konfigurace koncového bodu</span><span class="sxs-lookup"><span data-stu-id="ec21d-197">Endpoint configuration</span></span>

<span data-ttu-id="ec21d-198">`Endpoint` Konfigurace je nutná pro webové servery, které používají Windows HTTP serveru rozhraní API, včetně WebListener hello.</span><span class="sxs-lookup"><span data-stu-id="ec21d-198">An `Endpoint` configuration is required for web servers that use hello Windows HTTP Server API, including WebListener.</span></span> <span data-ttu-id="ec21d-199">Webové servery, které používají hello rozhraní API systému Windows HTTP serveru, musíte nejprve rezervovat jejich adresu URL s *http.sys* (to se obvykle dosahuje hello [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) nástroj).</span><span class="sxs-lookup"><span data-stu-id="ec21d-199">Web servers that use hello Windows HTTP Server API must first reserve their URL with *http.sys* (this is normally accomplished with hello [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) tool).</span></span> <span data-ttu-id="ec21d-200">Tato akce vyžaduje zvýšená oprávnění, které nemají vašim službám ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ec21d-200">This action requires elevated privileges that your services by default do not have.</span></span> <span data-ttu-id="ec21d-201">Hello "http" nebo "https" možnosti hello `Protocol` vlastnost hello `Endpoint` konfigurace v *ServiceManifest.xml* používají konkrétně tooinstruct hello tooregister modulu runtime Service Fabric adresu URL s *http.sys* vaším jménem pomocí hello [ *silné zástupné* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) předponu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="ec21d-201">hello "http" or "https" options for hello `Protocol` property of hello `Endpoint` configuration in *ServiceManifest.xml* are used specifically tooinstruct hello Service Fabric runtime tooregister a URL with *http.sys* on your behalf using hello [*strong wildcard*](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL prefix.</span></span>

<span data-ttu-id="ec21d-202">Například tooreserve `http://+:80` pro služby, hello následující konfigurace je třeba použít v ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="ec21d-202">For example, tooreserve `http://+:80` for a service, hello following configuration should be used in ServiceManifest.xml:</span></span>

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

<span data-ttu-id="ec21d-203">A hello název koncového bodu musí být předán toohello `WebListenerCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="ec21d-203">And hello endpoint name must be passed toohello `WebListenerCommunicationListener` constructor:</span></span>

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

#### <a name="use-weblistener-with-a-static-port"></a><span data-ttu-id="ec21d-204">WebListener pomocí statického portu</span><span class="sxs-lookup"><span data-stu-id="ec21d-204">Use WebListener with a static port</span></span>
<span data-ttu-id="ec21d-205">toouse statický port s WebListener, zadejte číslo portu hello v hello `Endpoint` konfigurace:</span><span class="sxs-lookup"><span data-stu-id="ec21d-205">toouse a static port with WebListener, provide hello port number in hello `Endpoint` configuration:</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-weblistener-with-a-dynamic-port"></a><span data-ttu-id="ec21d-206">Pomocí WebListener dynamický port</span><span class="sxs-lookup"><span data-stu-id="ec21d-206">Use WebListener with a dynamic port</span></span>
<span data-ttu-id="ec21d-207">toouse dynamicky přiřazeného portu s WebListener, vynechejte hello `Port` vlastnost hello `Endpoint` konfigurace:</span><span class="sxs-lookup"><span data-stu-id="ec21d-207">toouse a dynamically assigned port with WebListener, omit hello `Port` property in hello `Endpoint` configuration:</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="ec21d-208">Všimněte si, že dynamický port přidělena `Endpoint` konfigurace poskytuje jenom jeden port *za hostitelský proces*.</span><span class="sxs-lookup"><span data-stu-id="ec21d-208">Note that a dynamic port allocated by an `Endpoint` configuration only provides one port *per host process*.</span></span> <span data-ttu-id="ec21d-209">Hello aktuální Service Fabric model hostování umožňuje více služby instancí a/nebo repliky toobe hostované v hello stejný proces, což znamená, že každé z nich budou sdílet stejný port při přidělení prostřednictvím hello hello `Endpoint` konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ec21d-209">hello current Service Fabric hosting model allows multiple service instances and/or replicas toobe hosted in hello same process, meaning that each one will share hello same port when allocated through hello `Endpoint` configuration.</span></span> <span data-ttu-id="ec21d-210">Více instancí WebListener můžete sdílet port pomocí hello základní *http.sys* nepodporuje sdílení funkce, ale které portů `WebListenerCommunicationListener` kvůli toohello komplikace přináší pro požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="ec21d-210">Multiple WebListener instances can share a port using hello underlying *http.sys* port sharing feature, but that is not supported by `WebListenerCommunicationListener` due toohello complications it introduces for client requests.</span></span> <span data-ttu-id="ec21d-211">Pro používání dynamických portů je Kestrel hello doporučená webový server.</span><span class="sxs-lookup"><span data-stu-id="ec21d-211">For dynamic port usage, Kestrel is hello recommended web server.</span></span>

## <a name="kestrel-in-reliable-services"></a><span data-ttu-id="ec21d-212">Kestrel v spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="ec21d-212">Kestrel in Reliable Services</span></span>
<span data-ttu-id="ec21d-213">Kestrel mohou být používány spolehlivě importováním hello **Microsoft.ServiceFabric.AspNetCore.Kestrel** balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="ec21d-213">Kestrel can be used in a Reliable Service by importing hello **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet package.</span></span> <span data-ttu-id="ec21d-214">Tento balíček obsahuje `KestrelCommunicationListener`, implementace `ICommunicationListener`, což vám umožní toocreate ASP.NET Core tomuto webovému hostiteli uvnitř spolehlivě pomocí Kestrel jako hello webový server.</span><span class="sxs-lookup"><span data-stu-id="ec21d-214">This package contains `KestrelCommunicationListener`, an implementation of `ICommunicationListener`, that allows you toocreate an ASP.NET Core WebHost inside a Reliable Service using Kestrel as hello web server.</span></span>

<span data-ttu-id="ec21d-215">Kestrel je že napříč platformami webový server pro ASP.NET Core podle libuv, knihovny a platformy asynchronní vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="ec21d-215">Kestrel is a cross-platform web server for ASP.NET Core based on libuv, a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="ec21d-216">Na rozdíl od WebListener, Kestrel nepoužívá manažera centralizované koncový bod, jako *http.sys*.</span><span class="sxs-lookup"><span data-stu-id="ec21d-216">Unlike WebListener, Kestrel does not use a centralized endpoint manager such as *http.sys*.</span></span> <span data-ttu-id="ec21d-217">A na rozdíl od WebListener, Kestrel nepodporuje sdílení portů mezi více procesy.</span><span class="sxs-lookup"><span data-stu-id="ec21d-217">And unlike WebListener, Kestrel does not support port sharing between multiple processes.</span></span> <span data-ttu-id="ec21d-218">Každá instance Kestrel musíte použít jedinečnou port.</span><span class="sxs-lookup"><span data-stu-id="ec21d-218">Each instance of Kestrel must use a unique port.</span></span>

![kestrel][4]

### <a name="kestrel-in-a-stateless-service"></a><span data-ttu-id="ec21d-220">Kestrel v bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="ec21d-220">Kestrel in a stateless service</span></span>
<span data-ttu-id="ec21d-221">toouse `Kestrel` v bezstavové služby přepsat hello `CreateServiceInstanceListeners` metoda a vraťte se `KestrelCommunicationListener` instance:</span><span class="sxs-lookup"><span data-stu-id="ec21d-221">toouse `Kestrel` in a stateless service, override hello `CreateServiceInstanceListeners` method and return a `KestrelCommunicationListener` instance:</span></span>

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

### <a name="kestrel-in-a-stateful-service"></a><span data-ttu-id="ec21d-222">Kestrel v stavové služby</span><span class="sxs-lookup"><span data-stu-id="ec21d-222">Kestrel in a stateful service</span></span>
<span data-ttu-id="ec21d-223">toouse `Kestrel` v stavové služby přepsat hello `CreateServiceReplicaListeners` metoda a vraťte se `KestrelCommunicationListener` instance:</span><span class="sxs-lookup"><span data-stu-id="ec21d-223">toouse `Kestrel` in a stateful service, override hello `CreateServiceReplicaListeners` method and return a `KestrelCommunicationListener` instance:</span></span>

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

<span data-ttu-id="ec21d-224">V tomto příkladu instanci typu singleton daného `IReliableStateManager` je k dispozici kontejneru pro vkládání závislosti toohello tomuto webovému hostiteli.</span><span class="sxs-lookup"><span data-stu-id="ec21d-224">In this example, a singleton instance of `IReliableStateManager` is provided toohello WebHost dependency injection container.</span></span> <span data-ttu-id="ec21d-225">To není nezbytně nutné, ale umožňuje toouse `IReliableStateManager` a spolehlivé kolekcí ve vaší metody akce kontroleru MVC.</span><span class="sxs-lookup"><span data-stu-id="ec21d-225">This is not strictly necessary, but it allows you toouse `IReliableStateManager` and Reliable Collections in your MVC controller action methods.</span></span>

<span data-ttu-id="ec21d-226">Všimněte si, že `Endpoint` se název konfigurace **není** zadali příliš`KestrelCommunicationListener` v stavové služby.</span><span class="sxs-lookup"><span data-stu-id="ec21d-226">Note that an `Endpoint` configuration name is **not** provided too`KestrelCommunicationListener` in a stateful service.</span></span> <span data-ttu-id="ec21d-227">To je vysvětleno v následující části hello podrobněji.</span><span class="sxs-lookup"><span data-stu-id="ec21d-227">This is explained in more detail in hello following section.</span></span>

### <a name="endpoint-configuration"></a><span data-ttu-id="ec21d-228">Konfigurace koncového bodu</span><span class="sxs-lookup"><span data-stu-id="ec21d-228">Endpoint configuration</span></span>
<span data-ttu-id="ec21d-229">`Endpoint` Konfigurace není požadovaná toouse Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ec21d-229">An `Endpoint` configuration is not required toouse Kestrel.</span></span> 

<span data-ttu-id="ec21d-230">Kestrel je jednoduchý samostatný webový server; na rozdíl od WebListener (nebo HttpListener), není nutné `Endpoint` konfigurace v *ServiceManifest.xml* protože nevyžaduje předchozí toostarting adresa URL registrace.</span><span class="sxs-lookup"><span data-stu-id="ec21d-230">Kestrel is a simple stand-alone web server; unlike WebListener (or HttpListener), it does not need an `Endpoint` configuration in *ServiceManifest.xml* because it does not require URL registration prior toostarting.</span></span> 

#### <a name="use-kestrel-with-a-static-port"></a><span data-ttu-id="ec21d-231">Kestrel pomocí statického portu</span><span class="sxs-lookup"><span data-stu-id="ec21d-231">Use Kestrel with a static port</span></span>
<span data-ttu-id="ec21d-232">Statický port se dá nakonfigurovat v hello `Endpoint` konfigurace ServiceManifest.xml pro použití s Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ec21d-232">A static port can be configured in hello `Endpoint` configuration of ServiceManifest.xml for use with Kestrel.</span></span> <span data-ttu-id="ec21d-233">I když to není nezbytně nutné, poskytuje dvě potenciální výhody:</span><span class="sxs-lookup"><span data-stu-id="ec21d-233">Although this is not strictly necessary, it provides two potential benefits:</span></span>
 1. <span data-ttu-id="ec21d-234">Pokud hello port nespadá rozsah portů aplikace hello, protože je otevřen přes bránu firewall hello operačního systému pomocí Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ec21d-234">If hello port does not fall in hello application port range, it is opened through hello OS firewall by Service Fabric.</span></span>
 2. <span data-ttu-id="ec21d-235">Zadaná adresa URL tooyou prostřednictvím Hello `KestrelCommunicationListener` bude tento port použít.</span><span class="sxs-lookup"><span data-stu-id="ec21d-235">hello URL provided tooyou through `KestrelCommunicationListener` will use this port.</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="ec21d-236">Pokud `Endpoint` je nakonfigurován, jeho název, musí být předán do hello `KestrelCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="ec21d-236">If an `Endpoint` is configured, its name must be passed into hello `KestrelCommunicationListener` constructor:</span></span> 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

<span data-ttu-id="ec21d-237">Pokud `Endpoint` není použita konfigurace, vynechejte hello název v hello `KestrelCommunicationListener` konstruktor.</span><span class="sxs-lookup"><span data-stu-id="ec21d-237">If an `Endpoint` configuration is not used, omit hello name in hello `KestrelCommunicationListener` constructor.</span></span> <span data-ttu-id="ec21d-238">V takovém případě se použije dynamický port.</span><span class="sxs-lookup"><span data-stu-id="ec21d-238">In this case, a dynamic port will be used.</span></span> <span data-ttu-id="ec21d-239">Viz další část hello Další informace.</span><span class="sxs-lookup"><span data-stu-id="ec21d-239">See hello next section for more information.</span></span>

#### <a name="use-kestrel-with-a-dynamic-port"></a><span data-ttu-id="ec21d-240">Pomocí Kestrel dynamický port</span><span class="sxs-lookup"><span data-stu-id="ec21d-240">Use Kestrel with a dynamic port</span></span>
<span data-ttu-id="ec21d-241">Kestrel nemůžete použít automatické port přiřazení hello z hello `Endpoint` konfigurace v ServiceManifest.xml, protože port automatické přiřazení z `Endpoint` konfigurace přiřadí jedinečné port na *hostitelském procesu* , a jeden hostitelský proces může obsahovat několik instancí Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ec21d-241">Kestrel cannot use hello automatic port assignment from hello `Endpoint` configuration in ServiceManifest.xml, because automatic port assignment from an `Endpoint` configuration assigns a unique port per *host process*, and a single host process can contain multiple Kestrel instances.</span></span> <span data-ttu-id="ec21d-242">Vzhledem k tomu, že Kestrel nepodporuje sdílení portů, to nebude fungovat jako každá instance Kestrel musí být otevřen na portu jedinečné.</span><span class="sxs-lookup"><span data-stu-id="ec21d-242">Since Kestrel does not support port sharing, this does not work as each Kestrel instance must be opened on a unique port.</span></span>

<span data-ttu-id="ec21d-243">přiřazení dynamický port toouse s Kestrel, jednoduše vynechejte hello `Endpoint` konfigurace v ServiceManifest.xml zcela a nepředávejte toohello název koncového bodu `KestrelCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="ec21d-243">toouse dynamic port assignment with Kestrel, simply omit hello `Endpoint` configuration in ServiceManifest.xml entirely, and do not pass an endpoint name toohello `KestrelCommunicationListener` constructor:</span></span>

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

<span data-ttu-id="ec21d-244">V této konfiguraci `KestrelCommunicationListener` automaticky vybere nepoužitého portu z rozsahu portů aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ec21d-244">In this configuration, `KestrelCommunicationListener` will automatically select an unused port from hello application port range.</span></span>

## <a name="scenarios-and-configurations"></a><span data-ttu-id="ec21d-245">Scénáře a konfigurace</span><span class="sxs-lookup"><span data-stu-id="ec21d-245">Scenarios and configurations</span></span>
<span data-ttu-id="ec21d-246">Tato část popisuje hello následující scénáře a poskytuje hello doporučuje kombinace webového serveru, konfiguraci portů, možností integrace Service Fabric a ostatní nastavení tooachieve správně funguje služby:</span><span class="sxs-lookup"><span data-stu-id="ec21d-246">This section describes hello following scenarios and provides hello recommended combination of web server, port configuration, Service Fabric integration options, and miscellaneous settings tooachieve a properly functioning service:</span></span>
 - <span data-ttu-id="ec21d-247">Externě zveřejněné bezstavové služby ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec21d-247">Externally exposed ASP.NET Core stateless service</span></span>
 - <span data-ttu-id="ec21d-248">Pouze interní ASP.NET Core bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="ec21d-248">Internal-only ASP.NET Core stateless service</span></span>
 - <span data-ttu-id="ec21d-249">Pouze interní ASP.NET Core stavové služby</span><span class="sxs-lookup"><span data-stu-id="ec21d-249">Internal-only ASP.NET Core stateful service</span></span>

<span data-ttu-id="ec21d-250">**Externě zveřejněné** služba je ten, který zpřístupní koncový bod dosažitelný z mimo cluster hello, obvykle pomocí služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ec21d-250">An **externally exposed** service is one that exposes an endpoint reachable from outside hello cluster, usually through a load balancer.</span></span>

<span data-ttu-id="ec21d-251">**Jen interní** služby je jedním jejichž koncový bod je pouze dosažitelný z v rámci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ec21d-251">An **internal-only** service is one whose endpoint is only reachable from within hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="ec21d-252">Stavové služby, které by neměly být obecně koncové body zveřejněné toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="ec21d-252">Stateful service endpoints generally should not be exposed toohello Internet.</span></span> <span data-ttu-id="ec21d-253">Clustery, které jsou za nástroje pro vyrovnávání zatížení, které neberou v řešení služby Service Fabric, jako je například hello Vyrovnávání zatížení Azure, budou stavové služby nelze tooexpose, protože nástroj pro vyrovnávání zatížení hello nebude možné toolocate a směrovat provoz toohello replika odpovídající stavové služby.</span><span class="sxs-lookup"><span data-stu-id="ec21d-253">Clusters that are behind load balancers that are unaware of Service Fabric service resolution, such as hello Azure Load Balancer, will be unable tooexpose stateful services because hello load balancer will not be able toolocate and route traffic toohello appropriate stateful service replica.</span></span> 

### <a name="externally-exposed-aspnet-core-stateless-services"></a><span data-ttu-id="ec21d-254">Externě zveřejněné bezstavové služby ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec21d-254">Externally exposed ASP.NET Core stateless services</span></span>
<span data-ttu-id="ec21d-255">WebListener je doporučeno webového serveru pro front-endové služby, které zveřejňují externí, internetových koncových bodů protokolu HTTP na Windows hello.</span><span class="sxs-lookup"><span data-stu-id="ec21d-255">WebListener is hello recommended web server for front-end services that expose external, Internet-facing HTTP endpoints on Windows.</span></span> <span data-ttu-id="ec21d-256">Poskytuje lepší ochranu proti útokům a podporuje funkce, které Kestrel neexistuje, jako jsou ověřování systému Windows a sdílení portů.</span><span class="sxs-lookup"><span data-stu-id="ec21d-256">It provides better protection against attacks and supports features that Kestrel does not, such as Windows Authentication and port sharing.</span></span> 

<span data-ttu-id="ec21d-257">V současné době není podporovaný kestrel jako server edge (internetový).</span><span class="sxs-lookup"><span data-stu-id="ec21d-257">Kestrel is not supported as an edge (Internet-facing) server at this time.</span></span> <span data-ttu-id="ec21d-258">Musí použít reverzní proxy server, například služby IIS nebo Nginx hello toohandle provoz z veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="ec21d-258">A reverse proxy server such as IIS or Nginx must be used toohandle traffic from hello public Internet.</span></span>
 
<span data-ttu-id="ec21d-259">Kdy použít zveřejněné toohello Internetu, bezstavové služby dobře známé a stabilní koncový bod, který je dostupný prostřednictvím Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ec21d-259">When exposed toohello Internet, a stateless service should use a well-known and stable endpoint that is reachable through a load balancer.</span></span> <span data-ttu-id="ec21d-260">Toto je adresa URL hello bude poskytovat toousers vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec21d-260">This is hello URL you will provide toousers of your application.</span></span> <span data-ttu-id="ec21d-261">doporučuje se Hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="ec21d-261">hello following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="ec21d-262">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="ec21d-262">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec21d-263">Webový server</span><span class="sxs-lookup"><span data-stu-id="ec21d-263">Web server</span></span> | <span data-ttu-id="ec21d-264">WebListener</span><span class="sxs-lookup"><span data-stu-id="ec21d-264">WebListener</span></span> | <span data-ttu-id="ec21d-265">Pokud je služba hello pouze zveřejněné tooa důvěryhodné síti, takové intranetu, může být použit Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ec21d-265">If hello service is only exposed tooa trusted network, such an intranet, Kestrel may be used.</span></span> <span data-ttu-id="ec21d-266">Jinak WebListener je možnost hello upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="ec21d-266">Otherwise, WebListener is hello preferred option.</span></span> |
| <span data-ttu-id="ec21d-267">Konfigurace portu</span><span class="sxs-lookup"><span data-stu-id="ec21d-267">Port configuration</span></span> | <span data-ttu-id="ec21d-268">Statické</span><span class="sxs-lookup"><span data-stu-id="ec21d-268">static</span></span> | <span data-ttu-id="ec21d-269">Dobře známé statický port by měl být nakonfigurovaný v hello `Endpoints` konfigurace ServiceManifest.xml, jako třeba 80 pro protokol HTTP nebo 443 pro protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ec21d-269">A well-known static port should be configured in hello `Endpoints` configuration of ServiceManifest.xml, such as 80 for HTTP or 443 for HTTPS.</span></span> |
| <span data-ttu-id="ec21d-270">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="ec21d-270">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="ec21d-271">Žádný</span><span class="sxs-lookup"><span data-stu-id="ec21d-271">None</span></span> | <span data-ttu-id="ec21d-272">Hello `ServiceFabricIntegrationOptions.None` by měl být použit při konfiguraci Service Fabric integrace middleware tak, aby služba hello nepokusí toovalidate příchozích požadavků na jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ec21d-272">hello `ServiceFabricIntegrationOptions.None` option should be used when configuring Service Fabric integration middleware so that hello service does not attempt toovalidate incoming requests for a unique identifier.</span></span> <span data-ttu-id="ec21d-273">Externí uživatele vaší aplikace nebude vědět hello jedinečné identifikační informace používané hello middleware.</span><span class="sxs-lookup"><span data-stu-id="ec21d-273">External users of your application will not know hello unique identifying information used by hello middleware.</span></span> |
| <span data-ttu-id="ec21d-274">Počet instancí</span><span class="sxs-lookup"><span data-stu-id="ec21d-274">Instance Count</span></span> | <span data-ttu-id="ec21d-275">-1</span><span class="sxs-lookup"><span data-stu-id="ec21d-275">-1</span></span> | <span data-ttu-id="ec21d-276">V typické případy použití, musí být počet instancí hello nastavení nastavená příliš "-1", aby instance k dispozici na všech uzlech, jež přijímat přenosy z pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ec21d-276">In typical use cases, hello instance count setting should be set too"-1" so that an instance is available on all nodes that receive traffic from a load balancer.</span></span> |

<span data-ttu-id="ec21d-277">Pokud více externě zveřejněné služeb sdílet hello stejná sada uzlů, použije jedinečné, ale stabilní cestu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="ec21d-277">If multiple externally exposed services share hello same set of nodes, a unique but stable URL path should be used.</span></span> <span data-ttu-id="ec21d-278">To můžete udělat změnou zadaná při konfiguraci IWebHost adresa URL hello.</span><span class="sxs-lookup"><span data-stu-id="ec21d-278">This can be accomplished by modifying hello URL provided when configuring IWebHost.</span></span> <span data-ttu-id="ec21d-279">Všimněte si, že to se týká pouze tooWebListener.</span><span class="sxs-lookup"><span data-stu-id="ec21d-279">Note this applies tooWebListener only.</span></span>

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

### <a name="internal-only-stateless-aspnet-core-service"></a><span data-ttu-id="ec21d-280">Pouze interní bezstavové služby ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec21d-280">Internal-only stateless ASP.NET Core service</span></span>
<span data-ttu-id="ec21d-281">Bezstavové služby, které jsou pouze volat v rámci clusteru hello by měl použít jedinečné adresy URL a dynamicky přiřadit porty tooensure spolupráci mezi více služeb.</span><span class="sxs-lookup"><span data-stu-id="ec21d-281">Stateless services that are only called from within hello cluster should use unique URLs and dynamically assigned ports tooensure cooperation between multiple services.</span></span> <span data-ttu-id="ec21d-282">doporučuje se Hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="ec21d-282">hello following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="ec21d-283">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="ec21d-283">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec21d-284">Webový server</span><span class="sxs-lookup"><span data-stu-id="ec21d-284">Web server</span></span> | <span data-ttu-id="ec21d-285">kestrel</span><span class="sxs-lookup"><span data-stu-id="ec21d-285">Kestrel</span></span> | <span data-ttu-id="ec21d-286">I když WebListener mohou být použity pro interní bezstavové služby, je Kestrel hello doporučená tooallow serveru více tooshare instancí služby hostitele.</span><span class="sxs-lookup"><span data-stu-id="ec21d-286">Although WebListener may be used for internal stateless services, Kestrel is hello recommended server tooallow multiple service instances tooshare a host.</span></span>  |
| <span data-ttu-id="ec21d-287">Konfigurace portu</span><span class="sxs-lookup"><span data-stu-id="ec21d-287">Port configuration</span></span> | <span data-ttu-id="ec21d-288">dynamicky přiřadit</span><span class="sxs-lookup"><span data-stu-id="ec21d-288">dynamically assigned</span></span> | <span data-ttu-id="ec21d-289">Víc replik stavové služby může sdílet proces hostitele nebo hostitelského operačního systému a proto bude nutné odlišné porty.</span><span class="sxs-lookup"><span data-stu-id="ec21d-289">Multiple replicas of a stateful service may share a host process or host operating system and thus will need unique ports.</span></span> |
| <span data-ttu-id="ec21d-290">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="ec21d-290">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="ec21d-291">UseUniqueServiceUrl</span><span class="sxs-lookup"><span data-stu-id="ec21d-291">UseUniqueServiceUrl</span></span> | <span data-ttu-id="ec21d-292">S přiřazením dynamický port toto nastavení zabrání hello zaměněny identity popsané výše.</span><span class="sxs-lookup"><span data-stu-id="ec21d-292">With dynamic port assignment, this setting prevents hello mistaken identity issue described earlier.</span></span> |
| <span data-ttu-id="ec21d-293">InstanceCount</span><span class="sxs-lookup"><span data-stu-id="ec21d-293">InstanceCount</span></span> | <span data-ttu-id="ec21d-294">všechny</span><span class="sxs-lookup"><span data-stu-id="ec21d-294">any</span></span> | <span data-ttu-id="ec21d-295">počet instancí Hello nastavení můžete nastavit tooany hodnota nezbytné toooperate hello služby.</span><span class="sxs-lookup"><span data-stu-id="ec21d-295">hello instance count setting can be set tooany value necessary toooperate hello service.</span></span> |

### <a name="internal-only-stateful-aspnet-core-service"></a><span data-ttu-id="ec21d-296">Pouze interní stavová služba ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec21d-296">Internal-only stateful ASP.NET Core service</span></span>
<span data-ttu-id="ec21d-297">Stavové služby, které jsou pouze volat v rámci clusteru hello měli používat porty dynamicky přiřazené tooensure spolupráci mezi více služeb.</span><span class="sxs-lookup"><span data-stu-id="ec21d-297">Stateful services that are only called from within hello cluster should use dynamically assigned ports tooensure cooperation between multiple services.</span></span> <span data-ttu-id="ec21d-298">doporučuje se Hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="ec21d-298">hello following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="ec21d-299">**Poznámky k**</span><span class="sxs-lookup"><span data-stu-id="ec21d-299">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec21d-300">Webový server</span><span class="sxs-lookup"><span data-stu-id="ec21d-300">Web server</span></span> | <span data-ttu-id="ec21d-301">kestrel</span><span class="sxs-lookup"><span data-stu-id="ec21d-301">Kestrel</span></span> | <span data-ttu-id="ec21d-302">Hello `WebListenerCommunicationListener` není určen pro stavové služby, ve kterých repliky sdílet hostitelském procesu.</span><span class="sxs-lookup"><span data-stu-id="ec21d-302">hello `WebListenerCommunicationListener` is not designed for use by stateful services in which replicas share a host process.</span></span> |
| <span data-ttu-id="ec21d-303">Konfigurace portu</span><span class="sxs-lookup"><span data-stu-id="ec21d-303">Port configuration</span></span> | <span data-ttu-id="ec21d-304">dynamicky přiřadit</span><span class="sxs-lookup"><span data-stu-id="ec21d-304">dynamically assigned</span></span> | <span data-ttu-id="ec21d-305">Víc replik stavové služby může sdílet proces hostitele nebo hostitelského operačního systému a proto bude nutné odlišné porty.</span><span class="sxs-lookup"><span data-stu-id="ec21d-305">Multiple replicas of a stateful service may share a host process or host operating system and thus will need unique ports.</span></span> |
| <span data-ttu-id="ec21d-306">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="ec21d-306">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="ec21d-307">UseUniqueServiceUrl</span><span class="sxs-lookup"><span data-stu-id="ec21d-307">UseUniqueServiceUrl</span></span> | <span data-ttu-id="ec21d-308">S přiřazením dynamický port toto nastavení zabrání hello zaměněny identity popsané výše.</span><span class="sxs-lookup"><span data-stu-id="ec21d-308">With dynamic port assignment, this setting prevents hello mistaken identity issue described earlier.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ec21d-309">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec21d-309">Next steps</span></span>
[<span data-ttu-id="ec21d-310">Ladění aplikace Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec21d-310">Debug your Service Fabric application by using Visual Studio</span></span>](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
