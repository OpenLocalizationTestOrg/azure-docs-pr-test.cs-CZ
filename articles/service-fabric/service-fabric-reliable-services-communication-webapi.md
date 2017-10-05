---
title: "Služba komunikaci s rozhraním ASP.NET Web API | Microsoft Docs"
description: "Zjistěte, jak implementovat podrobné komunikace služby pomocí rozhraní ASP.NET Web API OWIN samoobslužné hostování v spolehlivé Services API."
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
ms.openlocfilehash: 73b7e1c0cb93ae7c54780a3aab837b0e5bcdb0a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a><span data-ttu-id="59574-103">Začínáme: Service Fabric webového rozhraní API služby s vlastním hostování OWIN</span><span class="sxs-lookup"><span data-stu-id="59574-103">Get started: Service Fabric Web API services with OWIN self-hosting</span></span>
<span data-ttu-id="59574-104">Azure Service Fabric vloží napájení rukou při se rozhodování, jak chcete, aby vaše služby, aby komunikovat s uživateli a mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="59574-104">Azure Service Fabric puts the power in your hands when you're deciding how you want your services to communicate with users and with each other.</span></span> <span data-ttu-id="59574-105">Tento kurz se zaměřuje na implementaci komunikace služby pomocí rozhraní ASP.NET Web API Open Web Interface pro .NET (OWIN) samoobslužné hostování v Service Fabric spolehlivé Services API.</span><span class="sxs-lookup"><span data-stu-id="59574-105">This tutorial focuses on implementing service communication using ASP.NET Web API with Open Web Interface for .NET (OWIN) self-hosting in Service Fabric's Reliable Services API.</span></span> <span data-ttu-id="59574-106">Jsme budete pustíte hluboko do spolehlivé služby modulární komunikace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="59574-106">We'll delve deeply into the Reliable Services pluggable communication API.</span></span> <span data-ttu-id="59574-107">Také použijeme webového rozhraní API v Podrobný příklad návod, jak nastavit vlastní komunikace naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="59574-107">We'll also use Web API in a step-by-step example to show you how to set up a custom communication listener.</span></span>

## <a name="introduction-to-web-api-in-service-fabric"></a><span data-ttu-id="59574-108">Úvod do webového rozhraní API v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="59574-108">Introduction to Web API in Service Fabric</span></span>
<span data-ttu-id="59574-109">Rozhraní ASP.NET Web API je Oblíbené a výkonné rozhraní pro vytváření rozhraní API HTTP na rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="59574-109">ASP.NET Web API is a popular and powerful framework for building HTTP APIs on top of the .NET Framework.</span></span> <span data-ttu-id="59574-110">Pokud už nejste obeznámeni s rozhraní, přečtěte si téma [Začínáme s ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) Další informace.</span><span class="sxs-lookup"><span data-stu-id="59574-110">If you're not already familiar with the framework, see [Getting started with ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) to learn more.</span></span>

<span data-ttu-id="59574-111">Webové rozhraní API v Service Fabric je stejné rozhraní ASP.NET Web API znáte a rádi.</span><span class="sxs-lookup"><span data-stu-id="59574-111">Web API in Service Fabric is the same ASP.NET Web API you know and love.</span></span> <span data-ttu-id="59574-112">Rozdíl je, jak můžete *hostitele* aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="59574-112">The difference is in how you *host* a Web API application.</span></span> <span data-ttu-id="59574-113">Nebudete používat Microsoft Internetové informační služby (IIS).</span><span class="sxs-lookup"><span data-stu-id="59574-113">You won't be using Microsoft Internet Information Services (IIS).</span></span> <span data-ttu-id="59574-114">Abyste lépe pochopili rozdíl, můžeme ho rozdělit na dvě části:</span><span class="sxs-lookup"><span data-stu-id="59574-114">To better understand the difference, let's break it into two parts:</span></span>

1. <span data-ttu-id="59574-115">Aplikace webového rozhraní API (včetně řadiče a modely)</span><span class="sxs-lookup"><span data-stu-id="59574-115">The Web API application (including controllers and models)</span></span>
2. <span data-ttu-id="59574-116">Hostitele (na webový server, obvykle IIS)</span><span class="sxs-lookup"><span data-stu-id="59574-116">The host (the web server, usually IIS)</span></span>

<span data-ttu-id="59574-117">Vlastní aplikace webového rozhraní API se nezmění.</span><span class="sxs-lookup"><span data-stu-id="59574-117">A Web API application itself doesn't change.</span></span> <span data-ttu-id="59574-118">Se neliší od aplikací webového rozhraní API, které máte napsaný v minulosti a nyní byste měli mít jednoduše přesunout přes většinu kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="59574-118">It's no different from Web API applications you may have written in the past, and you should be able to simply move over most of your application code.</span></span> <span data-ttu-id="59574-119">Ale pokud jste si byla hostování ve službě IIS, kde může být jen málo liší od se používá pro hostitele aplikace.</span><span class="sxs-lookup"><span data-stu-id="59574-119">But if you've been hosting on IIS, where you host the application may be a little different from what you're used to.</span></span> <span data-ttu-id="59574-120">Předtím, než se nám získat hostování část, Začněme s něco další známé: aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="59574-120">Before we get to the hosting part, let's start with something more familiar: the Web API application.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="59574-121">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="59574-121">Create the application</span></span>
<span data-ttu-id="59574-122">Začněte vytvořením nové aplikace Service Fabric pomocí jednoho bezstavové služby v sadě Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="59574-122">Start by creating a new Service Fabric application with a single stateless service in Visual Studio 2015.</span></span>

<span data-ttu-id="59574-123">Šablony sady Visual Studio pro bezstavové služby pomocí rozhraní Web API je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="59574-123">A Visual Studio template for a stateless service using Web API is available to you.</span></span> <span data-ttu-id="59574-124">V tomto kurzu využijeme projekt webového rozhraní API od začátku, jejímž výsledkem co byste získali jste vybrali této šablony.</span><span class="sxs-lookup"><span data-stu-id="59574-124">In this tutorial, we'll build a Web API project from scratch that results in what you'd get if you selected this template.</span></span>

<span data-ttu-id="59574-125">Vyberte prázdný projekt bezstavové služby se dozvíte, jak sestavit projekt webového rozhraní API od začátku, nebo můžete začít se šablonou bezstavové služby webového rozhraní API a jednoduše podle nich zorientujete.</span><span class="sxs-lookup"><span data-stu-id="59574-125">Select a blank Stateless Service project to learn how to build a Web API project from scratch, or you can start with the stateless service Web API template and simply follow along.</span></span>  

<span data-ttu-id="59574-126">Prvním krokem je stáhnout některé balíčky NuGet pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="59574-126">The first step is to pull in some NuGet packages for Web API.</span></span> <span data-ttu-id="59574-127">Balíček, který chcete použít je Microsoft.AspNet.WebApi.OwinSelfHost.</span><span class="sxs-lookup"><span data-stu-id="59574-127">The package we want to use is Microsoft.AspNet.WebApi.OwinSelfHost.</span></span> <span data-ttu-id="59574-128">Tento balíček obsahuje všechny potřebné balíčky webového rozhraní API a *hostitele* balíčky.</span><span class="sxs-lookup"><span data-stu-id="59574-128">This package includes all the necessary Web API packages and the *host* packages.</span></span> <span data-ttu-id="59574-129">To bude důležité později.</span><span class="sxs-lookup"><span data-stu-id="59574-129">This will be important later.</span></span>

<span data-ttu-id="59574-130">Po dokončení instalace balíčků, můžete začít vytváření základní strukturu projekt webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="59574-130">After the packages have been installed, you can begin building out the basic Web API project structure.</span></span> <span data-ttu-id="59574-131">Pokud jste použili webového rozhraní API, struktura projektu velmi povědomé.</span><span class="sxs-lookup"><span data-stu-id="59574-131">If you've used Web API, the project structure should look very familiar.</span></span> <span data-ttu-id="59574-132">Začněte přidáním `Controllers` directory a řadič jednoduché hodnoty:</span><span class="sxs-lookup"><span data-stu-id="59574-132">Start by adding a `Controllers` directory and a simple values controller:</span></span>

<span data-ttu-id="59574-133">**ValuesController.cs**</span><span class="sxs-lookup"><span data-stu-id="59574-133">**ValuesController.cs**</span></span>

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

<span data-ttu-id="59574-134">V dalším kroku přidejte třídu spuštění v kořenu projektu k registraci směrování, formátování a dalších nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="59574-134">Next, add a Startup class at the project root to register the routing, formatters, and any other configuration setup.</span></span> <span data-ttu-id="59574-135">Toto je také kde webového rozhraní API připojí k *hostitele*, který bude kdykoli znovu spustit později.</span><span class="sxs-lookup"><span data-stu-id="59574-135">This is also where Web API plugs in to the *host*, which will be revisited again later.</span></span> 

<span data-ttu-id="59574-136">**Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="59574-136">**Startup.cs**</span></span>

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

<span data-ttu-id="59574-137">Je to pro část aplikace.</span><span class="sxs-lookup"><span data-stu-id="59574-137">That's it for the application part.</span></span> <span data-ttu-id="59574-138">V tomto okamžiku nastavili jsme právě základní rozložení projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="59574-138">At this point, we've set up just the basic Web API project layout.</span></span> <span data-ttu-id="59574-139">Pokud by neměl vypadat výrazně liší od projekty webového rozhraní API, že jste vytvořili v minulosti nebo ze základní šablony webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="59574-139">So far, it shouldn't look much different from Web API projects you may have written in the past or from the basic Web API template.</span></span> <span data-ttu-id="59574-140">Obchodní logiky přejde v kontrolerech a modelech jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="59574-140">Your business logic goes in the controllers and models as usual.</span></span>

<span data-ttu-id="59574-141">Nyní co můžeme udělat o hostování tak, aby jeho ve skutečnosti spuštěním?</span><span class="sxs-lookup"><span data-stu-id="59574-141">Now what do we do about hosting so that we can actually run it?</span></span>

## <a name="service-hosting"></a><span data-ttu-id="59574-142">Služby hostování</span><span class="sxs-lookup"><span data-stu-id="59574-142">Service hosting</span></span>
<span data-ttu-id="59574-143">V Service Fabric služby běží v *procesu hostitele služby*, spustitelný soubor, který spouští službu kódu.</span><span class="sxs-lookup"><span data-stu-id="59574-143">In Service Fabric, your service runs in a *service host process*, an executable file that runs your service code.</span></span> <span data-ttu-id="59574-144">Při zápisu služby pomocí rozhraní API spolehlivé služby projektu služby právě zkompiluje na spustitelný soubor, který zaregistruje typ vaší služby a spustí váš kód.</span><span class="sxs-lookup"><span data-stu-id="59574-144">When you write a service by using the Reliable Services API, your service project just compiles to an executable file that registers your service type and runs your code.</span></span> <span data-ttu-id="59574-145">To platí ve většině případů při zápisu služby v Service Fabric v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="59574-145">This is true in most cases when you write a service on Service Fabric in .NET.</span></span> <span data-ttu-id="59574-146">Při otevření souboru Program.cs v projektu bezstavové služby, byste měli vidět:</span><span class="sxs-lookup"><span data-stu-id="59574-146">When you open Program.cs in the stateless service project, you should see:</span></span>

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

<span data-ttu-id="59574-147">Pokud toto vypadá nápadně jako vstupní bod do konzolovou aplikaci, která je, protože je.</span><span class="sxs-lookup"><span data-stu-id="59574-147">If that looks suspiciously like the entry point to a console application, that's because it is.</span></span>

<span data-ttu-id="59574-148">Další podrobnosti o procesu hostitele služby a registraci služby jsou nad rámec tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="59574-148">Further details about the service host process and service registration are beyond the scope of this article.</span></span> <span data-ttu-id="59574-149">Je důležité vědět, pro, ale teď, když *kódu služby běží ve svém vlastním procesu*.</span><span class="sxs-lookup"><span data-stu-id="59574-149">But it's important to know for now that *your service code is running in its own process*.</span></span>

## <a name="self-host-web-api-with-an-owin-host"></a><span data-ttu-id="59574-150">Hostování na vlastním serveru webového rozhraní API s hostitelem OWIN</span><span class="sxs-lookup"><span data-stu-id="59574-150">Self-host Web API with an OWIN host</span></span>
<span data-ttu-id="59574-151">Vzhledem k tomu, že je kód aplikace webového rozhraní API hostované ve svém vlastním procesu, jak vám propojte ho na webový server?</span><span class="sxs-lookup"><span data-stu-id="59574-151">Given that your Web API application code is hosted in its own process, how do you hook it up to a web server?</span></span> <span data-ttu-id="59574-152">Zadejte [OWIN](http://owin.org/).</span><span class="sxs-lookup"><span data-stu-id="59574-152">Enter [OWIN](http://owin.org/).</span></span> <span data-ttu-id="59574-153">OWIN je jednoduše smlouva mezi webových aplikací .NET a webovými servery.</span><span class="sxs-lookup"><span data-stu-id="59574-153">OWIN is simply a contract between .NET web applications and web servers.</span></span> <span data-ttu-id="59574-154">Tradičně při ASP.NET (až MVC 5) se používá, webové aplikace je úzce párované pro službu IIS prostřednictvím System.Web.</span><span class="sxs-lookup"><span data-stu-id="59574-154">Traditionally when ASP.NET (up to MVC 5) is used, the web application is tightly coupled to IIS through System.Web.</span></span> <span data-ttu-id="59574-155">Ale webového rozhraní API implementuje OWIN, tak z webového serveru, který je hostitelem ho můžete napsat webovou aplikaci, která odpojené.</span><span class="sxs-lookup"><span data-stu-id="59574-155">However, Web API implements OWIN, so you can write a web application that is decoupled from the web server that hosts it.</span></span> <span data-ttu-id="59574-156">Z toho důvodu můžete použít *hostovanou na vlastním* OWIN webový server, který můžete spustit v vlastního procesu.</span><span class="sxs-lookup"><span data-stu-id="59574-156">Because of this, you can use a *self-hosted* OWIN web server that you can start in your own process.</span></span> <span data-ttu-id="59574-157">To odpovídá perfektně s modelem hostování Service Fabric, kterou jsme právě popsané.</span><span class="sxs-lookup"><span data-stu-id="59574-157">This fits perfectly with the Service Fabric hosting model we just described.</span></span>

<span data-ttu-id="59574-158">V tomto článku použijeme Katana jako hostitel OWIN pro aplikaci webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="59574-158">In this article, we'll use Katana as the OWIN host for the Web API application.</span></span> <span data-ttu-id="59574-159">Katana je implementace hostitele OWIN open source založený na [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) a Windows [rozhraní API serveru HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="59574-159">Katana is an open-source OWIN host implementation built on [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) and the Windows [HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="59574-160">Další informace o Katana, přejděte na [Katana lokality](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span><span class="sxs-lookup"><span data-stu-id="59574-160">To learn more about Katana, go to the [Katana site](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span></span> <span data-ttu-id="59574-161">Rychlý přehled o tom, jak používat Katana pro hostování na vlastním serveru webového rozhraní API, najdete v části [použít OWIN Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span><span class="sxs-lookup"><span data-stu-id="59574-161">For a quick overview of how to use Katana to self-host Web API, see [Use OWIN to Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span></span>
> 
> 

## <a name="set-up-the-web-server"></a><span data-ttu-id="59574-162">Nastavit webový server</span><span class="sxs-lookup"><span data-stu-id="59574-162">Set up the web server</span></span>
<span data-ttu-id="59574-163">Spolehlivé služby API poskytuje ke vstupnímu bodu komunikace, kde můžete zařadit zásobníky komunikace, které umožňují uživatelům a klienty pro připojení ke službě:</span><span class="sxs-lookup"><span data-stu-id="59574-163">The Reliable Services API provides a communication entry point where you can plug in communication stacks that allow users and clients to connect to the service:</span></span>

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

<span data-ttu-id="59574-164">Webový server (a další komunikačního balíku, které můžete použít v budoucnu, jako je například technologie WebSockets) by měl použít rozhraní ICommunicationListener správně integrovat s v systému.</span><span class="sxs-lookup"><span data-stu-id="59574-164">The web server (and any other communication stack you use in the future, such as WebSockets) should use the ICommunicationListener interface to integrate correctly with the system.</span></span> <span data-ttu-id="59574-165">Důvody této začnou více zřejmá v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="59574-165">The reasons for this will become more apparent in the following steps.</span></span>

<span data-ttu-id="59574-166">Nejdřív vytvořte třídu s názvem OwinCommunicationListener, který implementuje ICommunicationListener:</span><span class="sxs-lookup"><span data-stu-id="59574-166">First, create a class called OwinCommunicationListener that implements ICommunicationListener:</span></span>

<span data-ttu-id="59574-167">**OwinCommunicationListener.cs**</span><span class="sxs-lookup"><span data-stu-id="59574-167">**OwinCommunicationListener.cs**</span></span>

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

<span data-ttu-id="59574-168">Rozhraní ICommunicationListener poskytuje tři metody pro správu naslouchací proces komunikace služby:</span><span class="sxs-lookup"><span data-stu-id="59574-168">The ICommunicationListener interface provides three methods to manage a communication listener for your service:</span></span>

* <span data-ttu-id="59574-169">*OpenAsync*.</span><span class="sxs-lookup"><span data-stu-id="59574-169">*OpenAsync*.</span></span> <span data-ttu-id="59574-170">Zahájit naslouchání požadavkům.</span><span class="sxs-lookup"><span data-stu-id="59574-170">Start listening for requests.</span></span>
* <span data-ttu-id="59574-171">*CloseAsync*.</span><span class="sxs-lookup"><span data-stu-id="59574-171">*CloseAsync*.</span></span> <span data-ttu-id="59574-172">Zastavil naslouchání požadavkům, dokončit všechny během letu žádosti a korektně vypnout.</span><span class="sxs-lookup"><span data-stu-id="59574-172">Stop listening for requests, finish any in-flight requests, and shut down gracefully.</span></span>
* <span data-ttu-id="59574-173">*Abort –*.</span><span class="sxs-lookup"><span data-stu-id="59574-173">*Abort*.</span></span> <span data-ttu-id="59574-174">Všechno, co zrušte a zastavte okamžitě.</span><span class="sxs-lookup"><span data-stu-id="59574-174">Cancel everything and stop immediately.</span></span>

<span data-ttu-id="59574-175">Pokud chcete začít, přidejte členy privátní třídy pro naslouchací proces bude potřeba funkce věcí.</span><span class="sxs-lookup"><span data-stu-id="59574-175">To get started, add private class members for things the listener will need to function.</span></span> <span data-ttu-id="59574-176">Tyto se inicializovat pomocí konstruktoru a později použije při nastavování naslouchání adresy URL.</span><span class="sxs-lookup"><span data-stu-id="59574-176">These will be initialized through the constructor and used later when you set up the listening URL.</span></span>

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

## <a name="implement-openasync"></a><span data-ttu-id="59574-177">Implementace OpenAsync</span><span class="sxs-lookup"><span data-stu-id="59574-177">Implement OpenAsync</span></span>
<span data-ttu-id="59574-178">Chcete-li nastavit webový server, je třeba dva údaje:</span><span class="sxs-lookup"><span data-stu-id="59574-178">To set up the web server, you need two pieces of information:</span></span>

* <span data-ttu-id="59574-179">*Předponu adresy URL cesta*.</span><span class="sxs-lookup"><span data-stu-id="59574-179">*A URL path prefix*.</span></span> <span data-ttu-id="59574-180">I když tato položka je nepovinná, že je vhodné, můžete to teď nastavte, aby bylo možné bezpečně hostovat několik webových služeb ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="59574-180">Although this is optional, it's good for you to set this up now so that you can safely host multiple web services in your application.</span></span>
* <span data-ttu-id="59574-181">*Port*.</span><span class="sxs-lookup"><span data-stu-id="59574-181">*A port*.</span></span>

<span data-ttu-id="59574-182">Než získáte port na webovém serveru, je důležité pochopit, že Service Fabric poskytuje aplikační vrstvu, která funguje jako vyrovnávací paměť mezi vaší aplikace a její příslušný operační systém, který běží na.</span><span class="sxs-lookup"><span data-stu-id="59574-182">Before you get a port for the web server, it's important that you understand that Service Fabric provides an application layer that acts as a buffer between your application and the underlying operating system that it runs on.</span></span> <span data-ttu-id="59574-183">Jako takový Service Fabric nabízí způsob, jak nakonfigurovat *koncové body* pro vaše služby.</span><span class="sxs-lookup"><span data-stu-id="59574-183">As such, Service Fabric provides a way to configure *endpoints* for your services.</span></span> <span data-ttu-id="59574-184">Service Fabric zajistí, že koncové body jsou k dispozici pro vaši službu používat.</span><span class="sxs-lookup"><span data-stu-id="59574-184">Service Fabric ensures that endpoints are available for your service to use.</span></span> <span data-ttu-id="59574-185">Tímto způsobem nemáte je konfigurovat sami v základní prostředí operačního systému.</span><span class="sxs-lookup"><span data-stu-id="59574-185">This way, you don't have to configure them yourself in the underlying OS environment.</span></span> <span data-ttu-id="59574-186">Snadno můžete hostovat aplikace Service Fabric v různých prostředích bez nutnosti provádět všechny změny do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="59574-186">You can easily host your Service Fabric application in different environments without having to make any changes to your application.</span></span> <span data-ttu-id="59574-187">(Například je možné hostovat stejnou aplikaci v Azure nebo ve svém vlastním datovém centru.)</span><span class="sxs-lookup"><span data-stu-id="59574-187">(For example, you can host the same application in Azure or in your own datacenter.)</span></span>

<span data-ttu-id="59574-188">Konfigurace koncového bodu protokolu HTTP v PackageRoot\ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="59574-188">Configure an HTTP endpoint in PackageRoot\ServiceManifest.xml:</span></span>

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

<span data-ttu-id="59574-189">Tento krok je důležité, protože proces hostitele služby běží pověřeními s omezeným přístupem (síťové služby v systému Windows).</span><span class="sxs-lookup"><span data-stu-id="59574-189">This step is important because the service host process runs under restricted credentials (Network Service on Windows).</span></span> <span data-ttu-id="59574-190">To znamená, že služby nebudete mít přístup k nastavení koncový bod HTTP na svůj vlastní.</span><span class="sxs-lookup"><span data-stu-id="59574-190">This means that your service won't have access to set up an HTTP endpoint on its own.</span></span> <span data-ttu-id="59574-191">Pomocí konfigurace koncového bodu Service Fabric zná k nastavení seznamu řízení správné přístupu (ACL) pro adresu URL, který bude naslouchat službu.</span><span class="sxs-lookup"><span data-stu-id="59574-191">By using the endpoint configuration, Service Fabric knows to set up the proper access control list (ACL) for the URL that the service will listen on.</span></span> <span data-ttu-id="59574-192">Service Fabric také poskytuje standardní místo pro konfiguraci koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="59574-192">Service Fabric also provides a standard place to configure endpoints.</span></span>

<span data-ttu-id="59574-193">Zpět v OwinCommunicationListener.cs můžete začít implementace OpenAsync.</span><span class="sxs-lookup"><span data-stu-id="59574-193">Back in OwinCommunicationListener.cs, you can start implementing OpenAsync.</span></span> <span data-ttu-id="59574-194">Toto je, kde spustit webový server.</span><span class="sxs-lookup"><span data-stu-id="59574-194">This is where you start the web server.</span></span> <span data-ttu-id="59574-195">Nejdřív získat informace o koncový bod a vytvořit adresu URL, kterou služba bude naslouchat na portu.</span><span class="sxs-lookup"><span data-stu-id="59574-195">First, get the endpoint information and create the URL that the service will listen on.</span></span> <span data-ttu-id="59574-196">Adresa URL se liší v závislosti na tom, jestli se používá naslouchací proces v bezstavové služby nebo stavové služby.</span><span class="sxs-lookup"><span data-stu-id="59574-196">The URL will be different depending on whether the listener is used in a stateless service or a stateful service.</span></span> <span data-ttu-id="59574-197">Pro stavové služby je potřeba vytvořit jedinečnou adresu pro každou repliku stavové služby, kterou naslouchá na naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="59574-197">For a stateful service, the listener needs to create a unique address for every stateful service replica it listens on.</span></span> <span data-ttu-id="59574-198">Adresa pro bezstavové služby, může být mnohem jednodušší.</span><span class="sxs-lookup"><span data-stu-id="59574-198">For stateless services, the address can be much simpler.</span></span> 

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

<span data-ttu-id="59574-199">Všimněte si, že se tady používá "http://+".</span><span class="sxs-lookup"><span data-stu-id="59574-199">Note that "http://+" is used here.</span></span> <span data-ttu-id="59574-200">Toto je zajistit, že webový server naslouchá na všechny dostupné adresy, včetně místního hostitele, plně kvalifikovaný název domény a IP adresu počítače.</span><span class="sxs-lookup"><span data-stu-id="59574-200">This is to make sure that the web server is listening on all available addresses, including localhost, FQDN, and the machine IP.</span></span>

<span data-ttu-id="59574-201">Implementace OpenAsync je jedním z nejdůležitějších důvodů, proč webového serveru (nebo všechny komunikačního balíku) je implementována jako ICommunicationListener, nikoli jen s ho otevřít přímo z `RunAsync()` ve službě.</span><span class="sxs-lookup"><span data-stu-id="59574-201">The OpenAsync implementation is one of the most important reasons why the web server (or any communication stack) is implemented as an ICommunicationListener, rather than just having it open directly from `RunAsync()` in the service.</span></span> <span data-ttu-id="59574-202">Vrácená hodnota z OpenAsync je adresa, která naslouchá na webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="59574-202">The return value from OpenAsync is the address that the web server is listening on.</span></span> <span data-ttu-id="59574-203">Pokud tato adresa se vrátí do systému, registruje adresu pomocí služby.</span><span class="sxs-lookup"><span data-stu-id="59574-203">When this address is returned to the system, it registers the address with the service.</span></span> <span data-ttu-id="59574-204">Service Fabric poskytuje rozhraní API, která umožňuje klientům a dalším službám, a pak požádejte podle názvu služby pro tuto adresu.</span><span class="sxs-lookup"><span data-stu-id="59574-204">Service Fabric provides an API that allows clients and other services to then ask for this address by service name.</span></span> <span data-ttu-id="59574-205">To je důležité, protože není statickou adresu služby.</span><span class="sxs-lookup"><span data-stu-id="59574-205">This is important because the service address is not static.</span></span> <span data-ttu-id="59574-206">Služby se přesouvají v clusteru pro účely vyrovnávání a dostupnosti prostředků.</span><span class="sxs-lookup"><span data-stu-id="59574-206">Services are moved around in the cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="59574-207">Toto je mechanismus, který umožňuje klientům přeložit adresu naslouchání pro službu.</span><span class="sxs-lookup"><span data-stu-id="59574-207">This is the mechanism that allows clients to resolve the listening address for a service.</span></span>

<span data-ttu-id="59574-208">OpenAsync si uvědomit, spustí webový server a vrátí adresu, která naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="59574-208">With that in mind, OpenAsync starts the web server and returns the address that it's listening on.</span></span> <span data-ttu-id="59574-209">Všimněte si, že naslouchá na "http://+", ale před OpenAsync vrátí adresu, "+" je nahrazena IP adresu nebo plně kvalifikovaný název domény uzlu je aktuálně v.</span><span class="sxs-lookup"><span data-stu-id="59574-209">Note that it listens on "http://+", but before OpenAsync returns the address, the "+" is replaced with the IP or FQDN of the node it is currently on.</span></span> <span data-ttu-id="59574-210">Adresa, vrátí tato metoda je, co není zaregistrována v systému.</span><span class="sxs-lookup"><span data-stu-id="59574-210">The address that is returned by this method is what's registered with the system.</span></span> <span data-ttu-id="59574-211">Je také co klienti a dalším službám uvidí při vyzvou pro adresu služby.</span><span class="sxs-lookup"><span data-stu-id="59574-211">It's also what clients and other services see when they ask for a service's address.</span></span> <span data-ttu-id="59574-212">Aby klienti mohli k nim připojit správně potřebují skutečná IP nebo plně kvalifikovaný název domény v adrese.</span><span class="sxs-lookup"><span data-stu-id="59574-212">For clients to correctly connect to it, they need an actual IP or FQDN in the address.</span></span>

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
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

<span data-ttu-id="59574-213">Všimněte si, že tato třída při spuštění, který byl předán v OwinCommunicationListener v konstruktoru odkazuje.</span><span class="sxs-lookup"><span data-stu-id="59574-213">Note that this references the Startup class that was passed in to the OwinCommunicationListener in the constructor.</span></span> <span data-ttu-id="59574-214">Tato instance spuštění se používá pro webový server k navázání připojení aplikace webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="59574-214">This startup instance is used by the web server to bootstrap the Web API application.</span></span>

<span data-ttu-id="59574-215">`ServiceEventSource.Current.Message()` Řádku se zobrazí v okně diagnostické události později, při spuštění aplikace pro potvrzení, že webový server byl úspěšně spuštěn.</span><span class="sxs-lookup"><span data-stu-id="59574-215">The `ServiceEventSource.Current.Message()` line will appear in the Diagnostic Events window later, when you run the application to confirm that the web server has started successfully.</span></span>

## <a name="implement-closeasync-and-abort"></a><span data-ttu-id="59574-216">Implementace CloseAsync a přerušení</span><span class="sxs-lookup"><span data-stu-id="59574-216">Implement CloseAsync and Abort</span></span>
<span data-ttu-id="59574-217">Nakonec implementujte CloseAsync a přerušení zastavení webového serveru.</span><span class="sxs-lookup"><span data-stu-id="59574-217">Finally, implement both CloseAsync and Abort to stop the web server.</span></span> <span data-ttu-id="59574-218">Uvolnění popisovač serveru, který jste vytvořili během OpenAsync můžete zastavit webový server.</span><span class="sxs-lookup"><span data-stu-id="59574-218">The web server can be stopped by disposing the server handle that was created during OpenAsync.</span></span>

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

<span data-ttu-id="59574-219">V tomto příkladu implementace CloseAsync a přerušení jednoduše zastavení webového serveru.</span><span class="sxs-lookup"><span data-stu-id="59574-219">In this implementation example, both CloseAsync and Abort simply stop the web server.</span></span> <span data-ttu-id="59574-220">Můžete zvolit provádět více řádně koordinované vypnutí webového serveru v CloseAsync.</span><span class="sxs-lookup"><span data-stu-id="59574-220">You may opt to perform a more gracefully coordinated shutdown of the web server in CloseAsync.</span></span> <span data-ttu-id="59574-221">Ukončení může například čekat na během letu žádosti o splnit, aby návratový.</span><span class="sxs-lookup"><span data-stu-id="59574-221">For example, the shutdown could wait for in-flight requests to be completed before the return.</span></span>

## <a name="start-the-web-server"></a><span data-ttu-id="59574-222">Spustí webový server</span><span class="sxs-lookup"><span data-stu-id="59574-222">Start the web server</span></span>
<span data-ttu-id="59574-223">Nyní jste připraveni vytvořit a vrátit instanci třídy OwinCommunicationListener Start pro server webového.</span><span class="sxs-lookup"><span data-stu-id="59574-223">You're now ready to create and return an instance of OwinCommunicationListener to start the web server.</span></span> <span data-ttu-id="59574-224">Zpět v třídě služby (WebService.cs) přepsat `CreateServiceInstanceListeners()` metoda:</span><span class="sxs-lookup"><span data-stu-id="59574-224">Back in the Service class (WebService.cs), override the `CreateServiceInstanceListeners()` method:</span></span>

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

<span data-ttu-id="59574-225">To je, kdy webového rozhraní API *aplikace* a OWIN *hostitele* nakonec splňovat.</span><span class="sxs-lookup"><span data-stu-id="59574-225">This is where the Web API *application* and the OWIN *host* finally meet.</span></span> <span data-ttu-id="59574-226">Hostitel (OwinCommunicationListener) je zadána instance *aplikace* (webového rozhraní API) prostřednictvím třída při spuštění.</span><span class="sxs-lookup"><span data-stu-id="59574-226">The host (OwinCommunicationListener) is given an instance of the *application* (Web API) via the Startup class.</span></span> <span data-ttu-id="59574-227">Service Fabric spravuje pak životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="59574-227">Service Fabric then manages its lifecycle.</span></span> <span data-ttu-id="59574-228">Tento stejný vzor obvykle platí s žádné komunikačního balíku.</span><span class="sxs-lookup"><span data-stu-id="59574-228">This same pattern can typically be followed with any communication stack.</span></span>

## <a name="put-it-all-together"></a><span data-ttu-id="59574-229">Spojení všech součástí dohromady</span><span class="sxs-lookup"><span data-stu-id="59574-229">Put it all together</span></span>
<span data-ttu-id="59574-230">V tomto příkladu, nemusíte dělat nic `RunAsync()` proto, že přepsání můžete jednoduše odeberou.</span><span class="sxs-lookup"><span data-stu-id="59574-230">In this example, you don't need to do anything in the `RunAsync()` method, so that override can simply be removed.</span></span>

<span data-ttu-id="59574-231">Implementace konečné služby by měl být velmi jednoduché.</span><span class="sxs-lookup"><span data-stu-id="59574-231">The final service implementation should be very simple.</span></span> <span data-ttu-id="59574-232">Stačí, když ho vytvořit naslouchací proces komunikace:</span><span class="sxs-lookup"><span data-stu-id="59574-232">It only needs to create the communication listener:</span></span>

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

<span data-ttu-id="59574-233">Kompletní `OwinCommunicationListener` třídy:</span><span class="sxs-lookup"><span data-stu-id="59574-233">The complete `OwinCommunicationListener` class:</span></span>

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
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

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

<span data-ttu-id="59574-234">Teď, když jste umístili všechny části na místě, projekt by měl vypadat jako typický aplikace webového rozhraní API s spolehlivé rozhraní API služby vstupní body a hostiteli OWIN.</span><span class="sxs-lookup"><span data-stu-id="59574-234">Now that you have put all the pieces in place, your project should look like a typical Web API application with Reliable Services API entry points and an OWIN host.</span></span>

## <a name="run-and-connect-through-a-web-browser"></a><span data-ttu-id="59574-235">Spuštění a připojení přes webový prohlížeč</span><span class="sxs-lookup"><span data-stu-id="59574-235">Run and connect through a web browser</span></span>
<span data-ttu-id="59574-236">Pokud jste neprovedli, [nastavení vývojového prostředí](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="59574-236">If you haven't done so, [set up your development environment](service-fabric-get-started.md).</span></span>

<span data-ttu-id="59574-237">Teď můžete vytvářet a nasazovat služby.</span><span class="sxs-lookup"><span data-stu-id="59574-237">You can now build and deploy your service.</span></span> <span data-ttu-id="59574-238">Stiskněte klávesu **F5** v sadě Visual Studio pro vytváření a nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="59574-238">Press **F5** in Visual Studio to build and deploy the application.</span></span> <span data-ttu-id="59574-239">V okně diagnostických událostí zobrazí zprávu, která označuje, že webový server otevřen na http://localhost:8281 /.</span><span class="sxs-lookup"><span data-stu-id="59574-239">In the Diagnostic Events window, you should see a message that indicates that the web server opened on http://localhost:8281/.</span></span>

> [!NOTE]
> <span data-ttu-id="59574-240">Pokud port již byla otevřena jiným procesem na počítači, může se zobrazit chyba sem.</span><span class="sxs-lookup"><span data-stu-id="59574-240">If the port has already been opened by another process on your machine, you may see an error here.</span></span> <span data-ttu-id="59574-241">To znamená, že naslouchací proces nelze otevřít.</span><span class="sxs-lookup"><span data-stu-id="59574-241">This indicates that the listener couldn't be opened.</span></span> <span data-ttu-id="59574-242">Pokud je to tento případ, zkuste použít jiný port pro konfiguraci koncového bodu v ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="59574-242">If that's the case, try using a different port for the endpoint configuration in ServiceManifest.xml.</span></span>
> 
> 

<span data-ttu-id="59574-243">Jakmile je služba spuštěná, spusťte prohlížeč a přejděte do [http://localhost:8281/api/hodnoty](http://localhost:8281/api/values) to vyzkoušíte.</span><span class="sxs-lookup"><span data-stu-id="59574-243">Once the service is running, open a browser and navigate to [http://localhost:8281/api/values](http://localhost:8281/api/values) to test it.</span></span>

## <a name="scale-it-out"></a><span data-ttu-id="59574-244">Škálovat tak</span><span class="sxs-lookup"><span data-stu-id="59574-244">Scale it out</span></span>
<span data-ttu-id="59574-245">Škálování bezstavové webové aplikace obvykle znamená přidáním další počítače a otáčí do webové aplikace na ně.</span><span class="sxs-lookup"><span data-stu-id="59574-245">Scaling out stateless web apps typically means adding more machines and spinning up the web apps on them.</span></span> <span data-ttu-id="59574-246">Jádro Orchestrace Service Fabric to můžete provést za vás, při každém přidání nových uzlů do clusteru.</span><span class="sxs-lookup"><span data-stu-id="59574-246">Service Fabric's orchestration engine can do this for you whenever new nodes are added to a cluster.</span></span> <span data-ttu-id="59574-247">Při vytváření instancí bezstavové služby, můžete zadat počet instancí, které chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="59574-247">When you create instances of a stateless service, you can specify the number of instances you want to create.</span></span> <span data-ttu-id="59574-248">Service Fabric umístí tohoto počtu instancí na uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="59574-248">Service Fabric places that number of instances on nodes in the cluster.</span></span> <span data-ttu-id="59574-249">A umožňuje nezapomeňte vytvořit více než jedna instance na jednoho libovolného uzlu.</span><span class="sxs-lookup"><span data-stu-id="59574-249">And it makes sure not to create more than one instance on any one node.</span></span> <span data-ttu-id="59574-250">Můžete také určit, aby Service Fabric vždy vytvořit instanci na každý uzel zadáním **-1** pro počet instancí.</span><span class="sxs-lookup"><span data-stu-id="59574-250">You can also instruct Service Fabric to always create an instance on every node by specifying **-1** for the instance count.</span></span> <span data-ttu-id="59574-251">To zaručuje, že při každém přidání uzlů pro horizontální škálování vašeho clusteru, bude instance bezstavové služby vytvořena na nových uzlů.</span><span class="sxs-lookup"><span data-stu-id="59574-251">This guarantees that whenever you add nodes to scale out your cluster, an instance of your stateless service will be created on the new nodes.</span></span> <span data-ttu-id="59574-252">Tato hodnota je vlastnost instance služby tak, že nastavíte při vytváření instance služby.</span><span class="sxs-lookup"><span data-stu-id="59574-252">This value is a property of the service instance, so it is set when you create a service instance.</span></span> <span data-ttu-id="59574-253">Můžete to udělat pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="59574-253">You can do this through PowerShell:</span></span>

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

<span data-ttu-id="59574-254">Můžete také k tomu při definování výchozí služba v projektu sady Visual Studio bezstavové služby:</span><span class="sxs-lookup"><span data-stu-id="59574-254">You can also do this when you define a default service in a Visual Studio stateless service project:</span></span>

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

<span data-ttu-id="59574-255">Další informace o tom, jak vytvořit aplikace a instance služby najdete v tématu [nasazení aplikace](service-fabric-deploy-remove-applications.md).</span><span class="sxs-lookup"><span data-stu-id="59574-255">For more information on how to create application and service instances, see [Deploy an application](service-fabric-deploy-remove-applications.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="59574-256">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59574-256">Next steps</span></span>
[<span data-ttu-id="59574-257">Ladění aplikace Service Fabric pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59574-257">Debug your Service Fabric application by using Visual Studio</span></span>](service-fabric-debugging-your-application.md)

