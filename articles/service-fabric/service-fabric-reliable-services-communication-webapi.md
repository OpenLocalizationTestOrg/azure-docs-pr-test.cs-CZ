---
title: "aaaService komunikace s hello rozhraní ASP.NET Web API | Microsoft Docs"
description: "Zjistěte, jak hello komunikace služby tooimplement podrobné pomocí rozhraní ASP.NET Web API s OWIN samoobslužné hostování v hello spolehlivé Services API."
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
ms.date: 02/10/2017
ms.author: vturecek
redirect_url: /azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore
ms.openlocfilehash: 3fb18fcb141ada0d79a0acda3dccbc7fb044346d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a><span data-ttu-id="b45ec-103">Začínáme: Service Fabric webového rozhraní API služby s vlastním hostování OWIN</span><span class="sxs-lookup"><span data-stu-id="b45ec-103">Get started: Service Fabric Web API services with OWIN self-hosting</span></span>
<span data-ttu-id="b45ec-104">Azure Service Fabric přináší hello výkon v ruce při se rozhodování, jak chcete vaši toocommunicate služby s uživateli a mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="b45ec-104">Azure Service Fabric puts hello power in your hands when you're deciding how you want your services toocommunicate with users and with each other.</span></span> <span data-ttu-id="b45ec-105">Tento kurz se zaměřuje na implementaci komunikace služby pomocí rozhraní ASP.NET Web API Open Web Interface pro .NET (OWIN) samoobslužné hostování v Service Fabric spolehlivé Services API.</span><span class="sxs-lookup"><span data-stu-id="b45ec-105">This tutorial focuses on implementing service communication using ASP.NET Web API with Open Web Interface for .NET (OWIN) self-hosting in Service Fabric's Reliable Services API.</span></span> <span data-ttu-id="b45ec-106">Jsme budete pustíte hluboko do hello spolehlivé služby modulární komunikace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b45ec-106">We'll delve deeply into hello Reliable Services pluggable communication API.</span></span> <span data-ttu-id="b45ec-107">Také použijeme webového rozhraní API v tooshow podrobný příklad můžete jak tooset si vlastní komunikace naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="b45ec-107">We'll also use Web API in a step-by-step example tooshow you how tooset up a custom communication listener.</span></span>

## <a name="introduction-tooweb-api-in-service-fabric"></a><span data-ttu-id="b45ec-108">Úvod tooWeb rozhraní API v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b45ec-108">Introduction tooWeb API in Service Fabric</span></span>
<span data-ttu-id="b45ec-109">Rozhraní ASP.NET Web API je Oblíbené a výkonné rozhraní pro vytváření rozhraní API HTTP nad hello rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b45ec-109">ASP.NET Web API is a popular and powerful framework for building HTTP APIs on top of hello .NET Framework.</span></span> <span data-ttu-id="b45ec-110">Pokud už nejste obeznámeni s hello framework, přečtěte si téma [Začínáme s ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="b45ec-110">If you're not already familiar with hello framework, see [Getting started with ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) toolearn more.</span></span>

<span data-ttu-id="b45ec-111">Webové rozhraní API v Service Fabric je hello stejné rozhraní ASP.NET Web API znáte a rádi.</span><span class="sxs-lookup"><span data-stu-id="b45ec-111">Web API in Service Fabric is hello same ASP.NET Web API you know and love.</span></span> <span data-ttu-id="b45ec-112">Hello rozdíl je, jak můžete *hostitele* aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b45ec-112">hello difference is in how you *host* a Web API application.</span></span> <span data-ttu-id="b45ec-113">Nebudete používat Microsoft Internetové informační služby (IIS).</span><span class="sxs-lookup"><span data-stu-id="b45ec-113">You won't be using Microsoft Internet Information Services (IIS).</span></span> <span data-ttu-id="b45ec-114">toobetter pochopit rozdíl hello, můžeme rozdělit na dvě části:</span><span class="sxs-lookup"><span data-stu-id="b45ec-114">toobetter understand hello difference, let's break it into two parts:</span></span>

1. <span data-ttu-id="b45ec-115">aplikace Hello webového rozhraní API (včetně řadiče a modely)</span><span class="sxs-lookup"><span data-stu-id="b45ec-115">hello Web API application (including controllers and models)</span></span>
2. <span data-ttu-id="b45ec-116">Hello hostitele (hello webový server, obvykle IIS)</span><span class="sxs-lookup"><span data-stu-id="b45ec-116">hello host (hello web server, usually IIS)</span></span>

<span data-ttu-id="b45ec-117">Vlastní aplikace webového rozhraní API se nezmění.</span><span class="sxs-lookup"><span data-stu-id="b45ec-117">A Web API application itself doesn't change.</span></span> <span data-ttu-id="b45ec-118">Se neliší od aplikací webového rozhraní API, které máte napsaný v posledních hello a musí být schopný toosimply přesunutí přes většinu kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="b45ec-118">It's no different from Web API applications you may have written in hello past, and you should be able toosimply move over most of your application code.</span></span> <span data-ttu-id="b45ec-119">Pokud jste si ve službě IIS, kde hostovat aplikace hello hostování, ale může být jen málo liší od co jste slouží jako.</span><span class="sxs-lookup"><span data-stu-id="b45ec-119">But if you've been hosting on IIS, where you host hello application may be a little different from what you're used to.</span></span> <span data-ttu-id="b45ec-120">Předtím, než získáme toohello hostování část, Začněme s něco další známé: hello aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b45ec-120">Before we get toohello hosting part, let's start with something more familiar: hello Web API application.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="b45ec-121">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="b45ec-121">Create hello application</span></span>
<span data-ttu-id="b45ec-122">Začněte vytvořením nové aplikace Service Fabric pomocí jednoho bezstavové služby v sadě Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="b45ec-122">Start by creating a new Service Fabric application with a single stateless service in Visual Studio 2015.</span></span>

<span data-ttu-id="b45ec-123">Šablony sady Visual Studio pro bezstavové služby pomocí rozhraní Web API je k dispozici tooyou.</span><span class="sxs-lookup"><span data-stu-id="b45ec-123">A Visual Studio template for a stateless service using Web API is available tooyou.</span></span> <span data-ttu-id="b45ec-124">V tomto kurzu využijeme projekt webového rozhraní API od začátku, jejímž výsledkem co byste získali jste vybrali této šablony.</span><span class="sxs-lookup"><span data-stu-id="b45ec-124">In this tutorial, we'll build a Web API project from scratch that results in what you'd get if you selected this template.</span></span>

<span data-ttu-id="b45ec-125">Vyberte prázdné toolearn projektu bezstavové služby, jak můžete toobuild projekt webového rozhraní API od začátku, nebo můžete začít s hello bezstavové služby šablony webového rozhraní API a jednoduše podle nich zorientujete.</span><span class="sxs-lookup"><span data-stu-id="b45ec-125">Select a blank Stateless Service project toolearn how toobuild a Web API project from scratch, or you can start with hello stateless service Web API template and simply follow along.</span></span>  

<span data-ttu-id="b45ec-126">prvním krokem Hello je toopull v některé balíčky NuGet pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b45ec-126">hello first step is toopull in some NuGet packages for Web API.</span></span> <span data-ttu-id="b45ec-127">balíček Hello chceme toouse je Microsoft.AspNet.WebApi.OwinSelfHost.</span><span class="sxs-lookup"><span data-stu-id="b45ec-127">hello package we want toouse is Microsoft.AspNet.WebApi.OwinSelfHost.</span></span> <span data-ttu-id="b45ec-128">Tento balíček obsahuje všechny potřebné balíčky webového rozhraní API hello a hello *hostitele* balíčky.</span><span class="sxs-lookup"><span data-stu-id="b45ec-128">This package includes all hello necessary Web API packages and hello *host* packages.</span></span> <span data-ttu-id="b45ec-129">To bude důležité později.</span><span class="sxs-lookup"><span data-stu-id="b45ec-129">This will be important later.</span></span>

<span data-ttu-id="b45ec-130">Po dokončení instalace hello balíčků, můžete začít vytváření hello základní strukturu projekt webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b45ec-130">After hello packages have been installed, you can begin building out hello basic Web API project structure.</span></span> <span data-ttu-id="b45ec-131">Pokud jste použili webového rozhraní API, struktura projektu hello velmi povědomé.</span><span class="sxs-lookup"><span data-stu-id="b45ec-131">If you've used Web API, hello project structure should look very familiar.</span></span> <span data-ttu-id="b45ec-132">Začněte přidáním `Controllers` directory a řadič jednoduché hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b45ec-132">Start by adding a `Controllers` directory and a simple values controller:</span></span>

<span data-ttu-id="b45ec-133">**ValuesController.cs**</span><span class="sxs-lookup"><span data-stu-id="b45ec-133">**ValuesController.cs**</span></span>

```csharp
using System.Collections.Generic;
using System.Web.Http;

namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

<span data-ttu-id="b45ec-134">V dalším kroku přidejte třídu spuštění na hello projektu kořenové tooregister hello směrování, formátování a dalších nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b45ec-134">Next, add a Startup class at hello project root tooregister hello routing, formatters, and any other configuration setup.</span></span> <span data-ttu-id="b45ec-135">Toto je také kde webového rozhraní API připojí toohello *hostitele*, který bude kdykoli znovu spustit později.</span><span class="sxs-lookup"><span data-stu-id="b45ec-135">This is also where Web API plugs in toohello *host*, which will be revisited again later.</span></span> 

<span data-ttu-id="b45ec-136">**Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="b45ec-136">**Startup.cs**</span></span>

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

<span data-ttu-id="b45ec-137">Je to pro část aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b45ec-137">That's it for hello application part.</span></span> <span data-ttu-id="b45ec-138">V tomto okamžiku nastavili jsme právě hello základní webového rozhraní API rozložení projektu.</span><span class="sxs-lookup"><span data-stu-id="b45ec-138">At this point, we've set up just hello basic Web API project layout.</span></span> <span data-ttu-id="b45ec-139">Pokud by neměl vypadat výrazně liší od projekty webového rozhraní API, že jste vytvořili v minulosti hello nebo z šablony webového rozhraní API základní hello.</span><span class="sxs-lookup"><span data-stu-id="b45ec-139">So far, it shouldn't look much different from Web API projects you may have written in hello past or from hello basic Web API template.</span></span> <span data-ttu-id="b45ec-140">Obchodní logika je třeba do hello řadiče a modely jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="b45ec-140">Your business logic goes in hello controllers and models as usual.</span></span>

<span data-ttu-id="b45ec-141">Nyní co můžeme udělat o hostování tak, aby jeho ve skutečnosti spuštěním?</span><span class="sxs-lookup"><span data-stu-id="b45ec-141">Now what do we do about hosting so that we can actually run it?</span></span>

## <a name="service-hosting"></a><span data-ttu-id="b45ec-142">Služby hostování</span><span class="sxs-lookup"><span data-stu-id="b45ec-142">Service hosting</span></span>
<span data-ttu-id="b45ec-143">V Service Fabric služby běží v *procesu hostitele služby*, spustitelný soubor, který spouští službu kódu.</span><span class="sxs-lookup"><span data-stu-id="b45ec-143">In Service Fabric, your service runs in a *service host process*, an executable file that runs your service code.</span></span> <span data-ttu-id="b45ec-144">Při zápisu služby pomocí hello spolehlivé rozhraní API služby projektu služby právě zkompiluje tooan spustitelný soubor, který zaregistruje typ vaší služby a spustí váš kód.</span><span class="sxs-lookup"><span data-stu-id="b45ec-144">When you write a service by using hello Reliable Services API, your service project just compiles tooan executable file that registers your service type and runs your code.</span></span> <span data-ttu-id="b45ec-145">To platí ve většině případů při zápisu služby v Service Fabric v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="b45ec-145">This is true in most cases when you write a service on Service Fabric in .NET.</span></span> <span data-ttu-id="b45ec-146">Při otevření souboru Program.cs v projektu hello bezstavové služby, byste měli vidět:</span><span class="sxs-lookup"><span data-stu-id="b45ec-146">When you open Program.cs in hello stateless service project, you should see:</span></span>

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

<span data-ttu-id="b45ec-147">Pokud toto vypadá nápadně jako hello vstupní bod tooa konzolovou aplikaci, která je, protože je.</span><span class="sxs-lookup"><span data-stu-id="b45ec-147">If that looks suspiciously like hello entry point tooa console application, that's because it is.</span></span>

<span data-ttu-id="b45ec-148">Další podrobnosti o hello proces hostitele služby a registraci služby jsou nad rámec tohoto článku hello.</span><span class="sxs-lookup"><span data-stu-id="b45ec-148">Further details about hello service host process and service registration are beyond hello scope of this article.</span></span> <span data-ttu-id="b45ec-149">Je důležité tooknow pro, ale teď, když *kódu služby běží ve svém vlastním procesu*.</span><span class="sxs-lookup"><span data-stu-id="b45ec-149">But it's important tooknow for now that *your service code is running in its own process*.</span></span>

## <a name="self-host-web-api-with-an-owin-host"></a><span data-ttu-id="b45ec-150">Hostování na vlastním serveru webového rozhraní API s hostitelem OWIN</span><span class="sxs-lookup"><span data-stu-id="b45ec-150">Self-host Web API with an OWIN host</span></span>
<span data-ttu-id="b45ec-151">Vzhledem k tomu, že je kód aplikace webového rozhraní API hostované ve svém vlastním procesu, jak můžete spojit se tooa webový server?</span><span class="sxs-lookup"><span data-stu-id="b45ec-151">Given that your Web API application code is hosted in its own process, how do you hook it up tooa web server?</span></span> <span data-ttu-id="b45ec-152">Zadejte [OWIN](http://owin.org/).</span><span class="sxs-lookup"><span data-stu-id="b45ec-152">Enter [OWIN](http://owin.org/).</span></span> <span data-ttu-id="b45ec-153">OWIN je jednoduše smlouva mezi webových aplikací .NET a webovými servery.</span><span class="sxs-lookup"><span data-stu-id="b45ec-153">OWIN is simply a contract between .NET web applications and web servers.</span></span> <span data-ttu-id="b45ec-154">Tradičně při ASP.NET (až tooMVC 5) se používá, hello webové aplikace je úzce párované tooIIS prostřednictvím System.Web.</span><span class="sxs-lookup"><span data-stu-id="b45ec-154">Traditionally when ASP.NET (up tooMVC 5) is used, hello web application is tightly coupled tooIIS through System.Web.</span></span> <span data-ttu-id="b45ec-155">Však webového rozhraní API implementuje OWIN, takže píšete webovou aplikaci, která odpojené z hello webový server, který je hostitelem.</span><span class="sxs-lookup"><span data-stu-id="b45ec-155">However, Web API implements OWIN, so you can write a web application that is decoupled from hello web server that hosts it.</span></span> <span data-ttu-id="b45ec-156">Z toho důvodu můžete použít *hostovanou na vlastním* OWIN webový server, který můžete spustit v vlastního procesu.</span><span class="sxs-lookup"><span data-stu-id="b45ec-156">Because of this, you can use a *self-hosted* OWIN web server that you can start in your own process.</span></span> <span data-ttu-id="b45ec-157">To odpovídá perfektně s Service Fabric hello hostování se model, který jsme právě popsané.</span><span class="sxs-lookup"><span data-stu-id="b45ec-157">This fits perfectly with hello Service Fabric hosting model we just described.</span></span>

<span data-ttu-id="b45ec-158">V tomto článku použijeme Katana jako hello OWIN hostitele pro hello aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b45ec-158">In this article, we'll use Katana as hello OWIN host for hello Web API application.</span></span> <span data-ttu-id="b45ec-159">Katana je implementace hostitele OWIN open source založený na [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) a hello Windows [rozhraní API serveru HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="b45ec-159">Katana is an open-source OWIN host implementation built on [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) and hello Windows [HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="b45ec-160">Další informace o Katana, přejděte toohello toolearn [Katana lokality](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span><span class="sxs-lookup"><span data-stu-id="b45ec-160">toolearn more about Katana, go toohello [Katana site](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span></span> <span data-ttu-id="b45ec-161">Rychlý přehled o toouse Katana tooself hostitele webového rozhraní API, najdete v části [OWIN použití tooSelf hostitele ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span><span class="sxs-lookup"><span data-stu-id="b45ec-161">For a quick overview of how toouse Katana tooself-host Web API, see [Use OWIN tooSelf-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span></span>
> 
> 

## <a name="set-up-hello-web-server"></a><span data-ttu-id="b45ec-162">Nastavit webový server hello</span><span class="sxs-lookup"><span data-stu-id="b45ec-162">Set up hello web server</span></span>
<span data-ttu-id="b45ec-163">Hello spolehlivé rozhraní API služby poskytuje ke vstupnímu bodu komunikace, kde můžete zařadit zásobníky komunikace, které umožňují uživatelům a klienti tooconnect toohello služby:</span><span class="sxs-lookup"><span data-stu-id="b45ec-163">hello Reliable Services API provides a communication entry point where you can plug in communication stacks that allow users and clients tooconnect toohello service:</span></span>

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

<span data-ttu-id="b45ec-164">Hello webový server (a další komunikačního balíku, které můžete použít v budoucí, jako je například technologie WebSockets hello) by měl použít hello ICommunicationListener rozhraní toointegrate správně s hello systémem.</span><span class="sxs-lookup"><span data-stu-id="b45ec-164">hello web server (and any other communication stack you use in hello future, such as WebSockets) should use hello ICommunicationListener interface toointegrate correctly with hello system.</span></span> <span data-ttu-id="b45ec-165">důvody Hello začnou více zřejmá v hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="b45ec-165">hello reasons for this will become more apparent in hello following steps.</span></span>

<span data-ttu-id="b45ec-166">Nejdřív vytvořte třídu s názvem OwinCommunicationListener, který implementuje ICommunicationListener:</span><span class="sxs-lookup"><span data-stu-id="b45ec-166">First, create a class called OwinCommunicationListener that implements ICommunicationListener:</span></span>

<span data-ttu-id="b45ec-167">**OwinCommunicationListener.cs**</span><span class="sxs-lookup"><span data-stu-id="b45ec-167">**OwinCommunicationListener.cs**</span></span>

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

<span data-ttu-id="b45ec-168">rozhraní ICommunicationListener Hello poskytuje tři metody toomanage naslouchací proces komunikace služby:</span><span class="sxs-lookup"><span data-stu-id="b45ec-168">hello ICommunicationListener interface provides three methods toomanage a communication listener for your service:</span></span>

* <span data-ttu-id="b45ec-169">*OpenAsync*.</span><span class="sxs-lookup"><span data-stu-id="b45ec-169">*OpenAsync*.</span></span> <span data-ttu-id="b45ec-170">Zahájit naslouchání požadavkům.</span><span class="sxs-lookup"><span data-stu-id="b45ec-170">Start listening for requests.</span></span>
* <span data-ttu-id="b45ec-171">*CloseAsync*.</span><span class="sxs-lookup"><span data-stu-id="b45ec-171">*CloseAsync*.</span></span> <span data-ttu-id="b45ec-172">Zastavil naslouchání požadavkům, dokončit všechny během letu žádosti a korektně vypnout.</span><span class="sxs-lookup"><span data-stu-id="b45ec-172">Stop listening for requests, finish any in-flight requests, and shut down gracefully.</span></span>
* <span data-ttu-id="b45ec-173">*Abort –*.</span><span class="sxs-lookup"><span data-stu-id="b45ec-173">*Abort*.</span></span> <span data-ttu-id="b45ec-174">Všechno, co zrušte a zastavte okamžitě.</span><span class="sxs-lookup"><span data-stu-id="b45ec-174">Cancel everything and stop immediately.</span></span>

<span data-ttu-id="b45ec-175">Soukromá třída členy tooget spustili, přidat, pro naslouchací proces hello věcí bude potřebovat toofunction.</span><span class="sxs-lookup"><span data-stu-id="b45ec-175">tooget started, add private class members for things hello listener will need toofunction.</span></span> <span data-ttu-id="b45ec-176">Tyto se inicializovat pomocí konstruktoru hello a později použije při nastavování hello naslouchání adresy URL.</span><span class="sxs-lookup"><span data-stu-id="b45ec-176">These will be initialized through hello constructor and used later when you set up hello listening URL.</span></span>

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }


    ...

```

## <a name="implement-openasync"></a><span data-ttu-id="b45ec-177">Implementace OpenAsync</span><span class="sxs-lookup"><span data-stu-id="b45ec-177">Implement OpenAsync</span></span>
<span data-ttu-id="b45ec-178">tooset hello webový server, budete potřebovat dva údaje:</span><span class="sxs-lookup"><span data-stu-id="b45ec-178">tooset up hello web server, you need two pieces of information:</span></span>

* <span data-ttu-id="b45ec-179">*Předponu adresy URL cesta*.</span><span class="sxs-lookup"><span data-stu-id="b45ec-179">*A URL path prefix*.</span></span> <span data-ttu-id="b45ec-180">I když tato položka je nepovinná, je vhodné pro tooset můžete tento až teď tak, aby ve vaší aplikaci můžete bezpečně hostovat několik webových služeb.</span><span class="sxs-lookup"><span data-stu-id="b45ec-180">Although this is optional, it's good for you tooset this up now so that you can safely host multiple web services in your application.</span></span>
* <span data-ttu-id="b45ec-181">*Port*.</span><span class="sxs-lookup"><span data-stu-id="b45ec-181">*A port*.</span></span>

<span data-ttu-id="b45ec-182">Předtím, než získáte port pro hello webový server, je důležité pochopit, že Service Fabric poskytuje aplikační vrstvu, která funguje jako vyrovnávací paměť mezi aplikací a hello základní operační systém, který běží na.</span><span class="sxs-lookup"><span data-stu-id="b45ec-182">Before you get a port for hello web server, it's important that you understand that Service Fabric provides an application layer that acts as a buffer between your application and hello underlying operating system that it runs on.</span></span> <span data-ttu-id="b45ec-183">Jako takový Service Fabric nabízí způsob tooconfigure *koncové body* pro vaše služby.</span><span class="sxs-lookup"><span data-stu-id="b45ec-183">As such, Service Fabric provides a way tooconfigure *endpoints* for your services.</span></span> <span data-ttu-id="b45ec-184">Service Fabric zajišťuje, aby byly k dispozici pro vaše služba toouse koncové body.</span><span class="sxs-lookup"><span data-stu-id="b45ec-184">Service Fabric ensures that endpoints are available for your service toouse.</span></span> <span data-ttu-id="b45ec-185">Tímto způsobem, že nemáte tooconfigure je sami v hello základní prostředí operačního systému.</span><span class="sxs-lookup"><span data-stu-id="b45ec-185">This way, you don't have tooconfigure them yourself in hello underlying OS environment.</span></span> <span data-ttu-id="b45ec-186">Snadno můžete hostovat aplikace Service Fabric v různých prostředích bez nutnosti toomake aplikace tooyour žádné změny.</span><span class="sxs-lookup"><span data-stu-id="b45ec-186">You can easily host your Service Fabric application in different environments without having toomake any changes tooyour application.</span></span> <span data-ttu-id="b45ec-187">(Například je možné hostovat hello stejnou aplikaci v Azure nebo ve svém vlastním datovém centru.)</span><span class="sxs-lookup"><span data-stu-id="b45ec-187">(For example, you can host hello same application in Azure or in your own datacenter.)</span></span>

<span data-ttu-id="b45ec-188">Konfigurace koncového bodu protokolu HTTP v PackageRoot\ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="b45ec-188">Configure an HTTP endpoint in PackageRoot\ServiceManifest.xml:</span></span>

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

<span data-ttu-id="b45ec-189">Tento krok je důležité, protože proces hostitele služby hello spouští pověřeními s omezeným přístupem (síťové služby v systému Windows).</span><span class="sxs-lookup"><span data-stu-id="b45ec-189">This step is important because hello service host process runs under restricted credentials (Network Service on Windows).</span></span> <span data-ttu-id="b45ec-190">To znamená, že služby nebudete mít přístup tooset až koncový bod HTTP svoje vlastní.</span><span class="sxs-lookup"><span data-stu-id="b45ec-190">This means that your service won't have access tooset up an HTTP endpoint on its own.</span></span> <span data-ttu-id="b45ec-191">Service Fabric pomocí konfigurace koncového bodu hello zná tooset hello řádného přístup ovládací prvek seznamu (ACL) pro hello, který bude naslouchat adresa URL, která hello služby.</span><span class="sxs-lookup"><span data-stu-id="b45ec-191">By using hello endpoint configuration, Service Fabric knows tooset up hello proper access control list (ACL) for hello URL that hello service will listen on.</span></span> <span data-ttu-id="b45ec-192">Service Fabric také poskytuje standardní místo tooconfigure koncové body.</span><span class="sxs-lookup"><span data-stu-id="b45ec-192">Service Fabric also provides a standard place tooconfigure endpoints.</span></span>

<span data-ttu-id="b45ec-193">Zpět v OwinCommunicationListener.cs můžete začít implementace OpenAsync.</span><span class="sxs-lookup"><span data-stu-id="b45ec-193">Back in OwinCommunicationListener.cs, you can start implementing OpenAsync.</span></span> <span data-ttu-id="b45ec-194">Toto je, kde spustit hello webový server.</span><span class="sxs-lookup"><span data-stu-id="b45ec-194">This is where you start hello web server.</span></span> <span data-ttu-id="b45ec-195">Nejdřív získat informace o koncovém hello a vytvořit hello adresu URL, který bude naslouchat hello služby.</span><span class="sxs-lookup"><span data-stu-id="b45ec-195">First, get hello endpoint information and create hello URL that hello service will listen on.</span></span> <span data-ttu-id="b45ec-196">Adresa URL Hello se liší v závislosti na tom, jestli se používá naslouchací proces hello v bezstavové služby nebo stavové služby.</span><span class="sxs-lookup"><span data-stu-id="b45ec-196">hello URL will be different depending on whether hello listener is used in a stateless service or a stateful service.</span></span> <span data-ttu-id="b45ec-197">Pro stavové služby musí naslouchací proces hello toocreate jedinečný pro každé repliky stavové služby se sleduje v adresu.</span><span class="sxs-lookup"><span data-stu-id="b45ec-197">For a stateful service, hello listener needs toocreate a unique address for every stateful service replica it listens on.</span></span> <span data-ttu-id="b45ec-198">Pro bezstavové služby může být adresa hello mnohem jednodušší.</span><span class="sxs-lookup"><span data-stu-id="b45ec-198">For stateless services, hello address can be much simpler.</span></span> 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }

    ...

```

<span data-ttu-id="b45ec-199">Všimněte si, že se tady používá "http://+".</span><span class="sxs-lookup"><span data-stu-id="b45ec-199">Note that "http://+" is used here.</span></span> <span data-ttu-id="b45ec-200">Toto je toomake se, že tento webový server hello naslouchá na všechny dostupné adresy, včetně místního hostitele, plně kvalifikovaný název domény a IP adresy počítače hello.</span><span class="sxs-lookup"><span data-stu-id="b45ec-200">This is toomake sure that hello web server is listening on all available addresses, including localhost, FQDN, and hello machine IP.</span></span>

<span data-ttu-id="b45ec-201">Hello OpenAsync implementace je jedním z hello nejdůležitější z důvodů, proč hello webového serveru (nebo všechny komunikačního balíku) je implementována jako ICommunicationListener, nikoli jen s ho otevřít přímo z `RunAsync()` ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="b45ec-201">hello OpenAsync implementation is one of hello most important reasons why hello web server (or any communication stack) is implemented as an ICommunicationListener, rather than just having it open directly from `RunAsync()` in hello service.</span></span> <span data-ttu-id="b45ec-202">Hello vrácená hodnota z OpenAsync je hello adresu, která hello webový server naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="b45ec-202">hello return value from OpenAsync is hello address that hello web server is listening on.</span></span> <span data-ttu-id="b45ec-203">Pokud tato adresa se vrátí toohello systému, zaregistruje hello adresu službou hello.</span><span class="sxs-lookup"><span data-stu-id="b45ec-203">When this address is returned toohello system, it registers hello address with hello service.</span></span> <span data-ttu-id="b45ec-204">Service Fabric poskytuje rozhraní API, která umožňuje klientským a dalších služeb toothen požádejte pro tuto adresu podle názvu služby.</span><span class="sxs-lookup"><span data-stu-id="b45ec-204">Service Fabric provides an API that allows clients and other services toothen ask for this address by service name.</span></span> <span data-ttu-id="b45ec-205">To je důležité, protože není statickou adresu služby hello.</span><span class="sxs-lookup"><span data-stu-id="b45ec-205">This is important because hello service address is not static.</span></span> <span data-ttu-id="b45ec-206">Služby přesouvání hello clusteru za účelem vyrovnávání a dostupnosti prostředků.</span><span class="sxs-lookup"><span data-stu-id="b45ec-206">Services are moved around in hello cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="b45ec-207">Toto je hello mechanismus, který umožňuje klientům tooresolve hello naslouchání adresu pro službu.</span><span class="sxs-lookup"><span data-stu-id="b45ec-207">This is hello mechanism that allows clients tooresolve hello listening address for a service.</span></span>

<span data-ttu-id="b45ec-208">OpenAsync si uvědomit, spustí hello webový server a vrátí hello adresu, která naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="b45ec-208">With that in mind, OpenAsync starts hello web server and returns hello address that it's listening on.</span></span> <span data-ttu-id="b45ec-209">Všimněte si, že naslouchá na "http://+", ale před OpenAsync vrátí hello adresu, hello "+" je nahrazena hello IP adresu nebo plně kvalifikovaný název domény hello uzel, který je aktuálně v.</span><span class="sxs-lookup"><span data-stu-id="b45ec-209">Note that it listens on "http://+", but before OpenAsync returns hello address, hello "+" is replaced with hello IP or FQDN of hello node it is currently on.</span></span> <span data-ttu-id="b45ec-210">Hello adresu, kterou vrátí tato metoda je, co není zaregistrována hello systému.</span><span class="sxs-lookup"><span data-stu-id="b45ec-210">hello address that is returned by this method is what's registered with hello system.</span></span> <span data-ttu-id="b45ec-211">Je také co klienti a dalším službám uvidí při vyzvou pro adresu služby.</span><span class="sxs-lookup"><span data-stu-id="b45ec-211">It's also what clients and other services see when they ask for a service's address.</span></span> <span data-ttu-id="b45ec-212">Pro klienty toocorrectly připojení tooit, potřebují adresu hello skutečná IP nebo plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="b45ec-212">For clients toocorrectly connect tooit, they need an actual IP or FQDN in hello address.</span></span>

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed tooopen endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

<span data-ttu-id="b45ec-213">Všimněte si, že to odkazuje hello spuštění třídu, která byla předána toohello OwinCommunicationListener v konstruktoru hello.</span><span class="sxs-lookup"><span data-stu-id="b45ec-213">Note that this references hello Startup class that was passed in toohello OwinCommunicationListener in hello constructor.</span></span> <span data-ttu-id="b45ec-214">Tato instance spuštění používá hello webový server toobootstrap hello aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b45ec-214">This startup instance is used by hello web server toobootstrap hello Web API application.</span></span>

<span data-ttu-id="b45ec-215">Hello `ServiceEventSource.Current.Message()` řádku se zobrazí v okně diagnostické události hello později, při spuštění aplikace tooconfirm hello tento hello webový server byl úspěšně spuštěn.</span><span class="sxs-lookup"><span data-stu-id="b45ec-215">hello `ServiceEventSource.Current.Message()` line will appear in hello Diagnostic Events window later, when you run hello application tooconfirm that hello web server has started successfully.</span></span>

## <a name="implement-closeasync-and-abort"></a><span data-ttu-id="b45ec-216">Implementace CloseAsync a přerušení</span><span class="sxs-lookup"><span data-stu-id="b45ec-216">Implement CloseAsync and Abort</span></span>
<span data-ttu-id="b45ec-217">Nakonec implementovat CloseAsync a přerušení toostop hello webový server.</span><span class="sxs-lookup"><span data-stu-id="b45ec-217">Finally, implement both CloseAsync and Abort toostop hello web server.</span></span> <span data-ttu-id="b45ec-218">uvolnění popisovač hello serveru, který jste vytvořili během OpenAsync můžete zastavit Hello webový server.</span><span class="sxs-lookup"><span data-stu-id="b45ec-218">hello web server can be stopped by disposing hello server handle that was created during OpenAsync.</span></span>

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

<span data-ttu-id="b45ec-219">V tomto příkladu implementace CloseAsync a přerušení jednoduše zastavit hello webový server.</span><span class="sxs-lookup"><span data-stu-id="b45ec-219">In this implementation example, both CloseAsync and Abort simply stop hello web server.</span></span> <span data-ttu-id="b45ec-220">Můžete zvolit tooperform více řádně koordinované vypnutí hello webového serveru v CloseAsync.</span><span class="sxs-lookup"><span data-stu-id="b45ec-220">You may opt tooperform a more gracefully coordinated shutdown of hello web server in CloseAsync.</span></span> <span data-ttu-id="b45ec-221">Vypnutí hello může například čekat na během letu požadavky toobe dokončit před hello vrátit.</span><span class="sxs-lookup"><span data-stu-id="b45ec-221">For example, hello shutdown could wait for in-flight requests toobe completed before hello return.</span></span>

## <a name="start-hello-web-server"></a><span data-ttu-id="b45ec-222">Spustí webový server hello</span><span class="sxs-lookup"><span data-stu-id="b45ec-222">Start hello web server</span></span>
<span data-ttu-id="b45ec-223">Teď už připravený toocreate a vrátit instanci třídy OwinCommunicationListener toostart hello webový server.</span><span class="sxs-lookup"><span data-stu-id="b45ec-223">You're now ready toocreate and return an instance of OwinCommunicationListener toostart hello web server.</span></span> <span data-ttu-id="b45ec-224">Zpět v hello třída služby (WebService.cs), přepište hello `CreateServiceInstanceListeners()` metoda:</span><span class="sxs-lookup"><span data-stu-id="b45ec-224">Back in hello Service class (WebService.cs), override hello `CreateServiceInstanceListeners()` method:</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

<span data-ttu-id="b45ec-225">Kde to je hello webového rozhraní API *aplikace* a hello OWIN *hostitele* nakonec splňovat.</span><span class="sxs-lookup"><span data-stu-id="b45ec-225">This is where hello Web API *application* and hello OWIN *host* finally meet.</span></span> <span data-ttu-id="b45ec-226">Hello hostitele (OwinCommunicationListener) je zadána instance hello *aplikace* (webového rozhraní API) prostřednictvím hello třída při spuštění.</span><span class="sxs-lookup"><span data-stu-id="b45ec-226">hello host (OwinCommunicationListener) is given an instance of hello *application* (Web API) via hello Startup class.</span></span> <span data-ttu-id="b45ec-227">Service Fabric spravuje pak životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="b45ec-227">Service Fabric then manages its lifecycle.</span></span> <span data-ttu-id="b45ec-228">Tento stejný vzor obvykle platí s žádné komunikačního balíku.</span><span class="sxs-lookup"><span data-stu-id="b45ec-228">This same pattern can typically be followed with any communication stack.</span></span>

## <a name="put-it-all-together"></a><span data-ttu-id="b45ec-229">Spojení všech součástí dohromady</span><span class="sxs-lookup"><span data-stu-id="b45ec-229">Put it all together</span></span>
<span data-ttu-id="b45ec-230">V tomto příkladu nepotřebujete toodo ničím v hello `RunAsync()` proto, že přepsání můžete jednoduše odeberou.</span><span class="sxs-lookup"><span data-stu-id="b45ec-230">In this example, you don't need toodo anything in hello `RunAsync()` method, so that override can simply be removed.</span></span>

<span data-ttu-id="b45ec-231">implementace Hello konečné služby by měl být velmi jednoduché.</span><span class="sxs-lookup"><span data-stu-id="b45ec-231">hello final service implementation should be very simple.</span></span> <span data-ttu-id="b45ec-232">Potřebuje pouze toocreate hello komunikace naslouchací proces:</span><span class="sxs-lookup"><span data-stu-id="b45ec-232">It only needs toocreate hello communication listener:</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

<span data-ttu-id="b45ec-233">Hello dokončení `OwinCommunicationListener` třídy:</span><span class="sxs-lookup"><span data-stu-id="b45ec-233">hello complete `OwinCommunicationListener` class:</span></span>

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed tooopen endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

<span data-ttu-id="b45ec-234">Teď, když jste umístili všech částí hello na místě, projekt by měl vypadat jako typický aplikace webového rozhraní API s spolehlivé rozhraní API služby vstupní body a hostiteli OWIN.</span><span class="sxs-lookup"><span data-stu-id="b45ec-234">Now that you have put all hello pieces in place, your project should look like a typical Web API application with Reliable Services API entry points and an OWIN host.</span></span>

## <a name="run-and-connect-through-a-web-browser"></a><span data-ttu-id="b45ec-235">Spuštění a připojení přes webový prohlížeč</span><span class="sxs-lookup"><span data-stu-id="b45ec-235">Run and connect through a web browser</span></span>
<span data-ttu-id="b45ec-236">Pokud jste neprovedli, [nastavení vývojového prostředí](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b45ec-236">If you haven't done so, [set up your development environment](service-fabric-get-started.md).</span></span>

<span data-ttu-id="b45ec-237">Teď můžete vytvářet a nasazovat služby.</span><span class="sxs-lookup"><span data-stu-id="b45ec-237">You can now build and deploy your service.</span></span> <span data-ttu-id="b45ec-238">Stiskněte klávesu **F5** v sadě Visual Studio toobuild a nasazení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b45ec-238">Press **F5** in Visual Studio toobuild and deploy hello application.</span></span> <span data-ttu-id="b45ec-239">V okně diagnostické události hello zobrazí zprávu, která ukazuje na tento webový server hello otevřít na http://localhost:8281 /.</span><span class="sxs-lookup"><span data-stu-id="b45ec-239">In hello Diagnostic Events window, you should see a message that indicates that hello web server opened on http://localhost:8281/.</span></span>

> [!NOTE]
> <span data-ttu-id="b45ec-240">Pokud hello port již byla otevřena jiným procesem na počítači, může se zobrazit chyba sem.</span><span class="sxs-lookup"><span data-stu-id="b45ec-240">If hello port has already been opened by another process on your machine, you may see an error here.</span></span> <span data-ttu-id="b45ec-241">To znamená, že tento naslouchací proces hello nelze otevřít.</span><span class="sxs-lookup"><span data-stu-id="b45ec-241">This indicates that hello listener couldn't be opened.</span></span> <span data-ttu-id="b45ec-242">Pokud se hello případ, zkuste použít jiný port pro hello konfigurace koncového bodu v ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="b45ec-242">If that's hello case, try using a different port for hello endpoint configuration in ServiceManifest.xml.</span></span>
> 
> 

<span data-ttu-id="b45ec-243">Jakmile je spuštěna služba hello otevřete prohlížeč a přejděte příliš[http://localhost:8281/api/hodnoty](http://localhost:8281/api/values) tootest ho.</span><span class="sxs-lookup"><span data-stu-id="b45ec-243">Once hello service is running, open a browser and navigate too[http://localhost:8281/api/values](http://localhost:8281/api/values) tootest it.</span></span>

## <a name="scale-it-out"></a><span data-ttu-id="b45ec-244">Škálovat tak</span><span class="sxs-lookup"><span data-stu-id="b45ec-244">Scale it out</span></span>
<span data-ttu-id="b45ec-245">Škálování bezstavové webové aplikace obvykle znamená přidáním další počítače a otáčí až hello webové aplikace na ně.</span><span class="sxs-lookup"><span data-stu-id="b45ec-245">Scaling out stateless web apps typically means adding more machines and spinning up hello web apps on them.</span></span> <span data-ttu-id="b45ec-246">Jádro Orchestrace Service Fabric to můžete provést za vás, při každém přidání nových uzlů clusteru tooa.</span><span class="sxs-lookup"><span data-stu-id="b45ec-246">Service Fabric's orchestration engine can do this for you whenever new nodes are added tooa cluster.</span></span> <span data-ttu-id="b45ec-247">Při vytváření instancí bezstavové služby, můžete zadat číslo hello instancí chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="b45ec-247">When you create instances of a stateless service, you can specify hello number of instances you want toocreate.</span></span> <span data-ttu-id="b45ec-248">Service Fabric umístí na uzly v clusteru hello tento počet instancí.</span><span class="sxs-lookup"><span data-stu-id="b45ec-248">Service Fabric places that number of instances on nodes in hello cluster.</span></span> <span data-ttu-id="b45ec-249">A zkontroluje, zda není toocreate více než jednu instanci na jednoho libovolného uzlu.</span><span class="sxs-lookup"><span data-stu-id="b45ec-249">And it makes sure not toocreate more than one instance on any one node.</span></span> <span data-ttu-id="b45ec-250">Můžete také určit, aby tooalways Service Fabric vytvořit instanci v každém uzlu tak, že zadáte **-1** pro počet instancí hello.</span><span class="sxs-lookup"><span data-stu-id="b45ec-250">You can also instruct Service Fabric tooalways create an instance on every node by specifying **-1** for hello instance count.</span></span> <span data-ttu-id="b45ec-251">To zaručuje, že při každém přidání tooscale uzly na cluster, instanci bezstavové služby vytvořit na nové uzly hello.</span><span class="sxs-lookup"><span data-stu-id="b45ec-251">This guarantees that whenever you add nodes tooscale out your cluster, an instance of your stateless service will be created on hello new nodes.</span></span> <span data-ttu-id="b45ec-252">Tato hodnota je vlastnost hello instance služby, tak, že nastavíte při vytváření instance služby.</span><span class="sxs-lookup"><span data-stu-id="b45ec-252">This value is a property of hello service instance, so it is set when you create a service instance.</span></span> <span data-ttu-id="b45ec-253">Můžete to udělat pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b45ec-253">You can do this through PowerShell:</span></span>

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

<span data-ttu-id="b45ec-254">Můžete také k tomu při definování výchozí služba v projektu sady Visual Studio bezstavové služby:</span><span class="sxs-lookup"><span data-stu-id="b45ec-254">You can also do this when you define a default service in a Visual Studio stateless service project:</span></span>

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

<span data-ttu-id="b45ec-255">Další informace o způsobu toocreate aplikace a instance služby, najdete v části [nasazení aplikace](service-fabric-deploy-remove-applications.md).</span><span class="sxs-lookup"><span data-stu-id="b45ec-255">For more information on how toocreate application and service instances, see [Deploy an application](service-fabric-deploy-remove-applications.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b45ec-256">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b45ec-256">Next steps</span></span>
[<span data-ttu-id="b45ec-257">Ladění aplikace Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b45ec-257">Debug your Service Fabric application by using Visual Studio</span></span>](service-fabric-debugging-your-application.md)

