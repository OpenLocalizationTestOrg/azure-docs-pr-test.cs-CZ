---
title: "Filtrování a předběžného zpracování v Azure Application Insights SDK | Microsoft Docs"
description: "Zápis procesorů Telemetrie a inicializátory telemetrická data sady SDK, filtrovat nebo přidání vlastnosti do data před odesláním telemetrie na portál Application Insights."
services: application-insights
documentationcenter: 
author: beckylino
manager: carmonm
ms.assetid: 38a9e454-43d5-4dba-a0f0-bd7cd75fb97b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 11/23/2016
ms.author: bwren
ms.openlocfilehash: 17e66775dd2cd1c858594102f1ddb32e2fbbccc8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a><span data-ttu-id="c60a3-103">Filtrování a předběžného zpracování telemetrie Application Insights SDK</span><span class="sxs-lookup"><span data-stu-id="c60a3-103">Filtering and preprocessing telemetry in the Application Insights SDK</span></span>


<span data-ttu-id="c60a3-104">Můžete napsat a konfigurovat moduly plug-in pro Application Insights SDK pro přizpůsobení jak telemetrie bude zachycen a před odesláním do služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="c60a3-104">You can write and configure plug-ins for the Application Insights SDK to customize how telemetry is captured and processed before it is sent to the Application Insights service.</span></span>

* <span data-ttu-id="c60a3-105">[Vzorkování](app-insights-sampling.md) snižuje telemetrie, aniž by to ovlivnilo vaše statistiky.</span><span class="sxs-lookup"><span data-stu-id="c60a3-105">[Sampling](app-insights-sampling.md) reduces the volume of telemetry without affecting your statistics.</span></span> <span data-ttu-id="c60a3-106">Udržuje společně související datových bodů tak, aby můžete procházet mezi nimi při diagnostikování problému.</span><span class="sxs-lookup"><span data-stu-id="c60a3-106">It keeps together related data points so that you can navigate between them when diagnosing a problem.</span></span> <span data-ttu-id="c60a3-107">Celkové počty jsou na portálu, násobí odpovídajícím způsobem pro odběr.</span><span class="sxs-lookup"><span data-stu-id="c60a3-107">In the portal, the total counts are multiplied to compensate for the sampling.</span></span>
* <span data-ttu-id="c60a3-108">Filtrování s procesory Telemetrie [pro technologii ASP.NET](#filtering) nebo [Java](app-insights-java-filter-telemetry.md) umožňuje vybrat nebo upravit telemetrie sady SDK před odesláním na server.</span><span class="sxs-lookup"><span data-stu-id="c60a3-108">Filtering with Telemetry Processors [for ASP.NET](#filtering) or [Java](app-insights-java-filter-telemetry.md) lets you select or modify telemetry in the SDK before it is sent to the server.</span></span> <span data-ttu-id="c60a3-109">Například může snížit objem telemetrie tak, že vyloučíte z robotů požadavky.</span><span class="sxs-lookup"><span data-stu-id="c60a3-109">For example, you could reduce the volume of telemetry by excluding requests from robots.</span></span> <span data-ttu-id="c60a3-110">Ale filtrování se více základní postup pro omezení provozu než vzorkování.</span><span class="sxs-lookup"><span data-stu-id="c60a3-110">But filtering is a more basic approach to reducing traffic than sampling.</span></span> <span data-ttu-id="c60a3-111">Umožňuje větší kontrolu nad co se přenášejí, ale máte potřeba mít na paměti, že ovlivňuje statistice – například pokud můžete filtrovat všechny úspěšné požadavky.</span><span class="sxs-lookup"><span data-stu-id="c60a3-111">It allows you more control over what is transmitted, but you have to be aware that it affects your statistics - for example, if you filter out all successful requests.</span></span>
* <span data-ttu-id="c60a3-112">[Inicializátory telemetrie přidávat vlastnosti](#add-properties) pro všechny telemetrická data odesílaná z vaší aplikace, včetně telemetrie z standardní moduly.</span><span class="sxs-lookup"><span data-stu-id="c60a3-112">[Telemetry Initializers add properties](#add-properties) to any telemetry sent from your app, including telemetry from the standard modules.</span></span> <span data-ttu-id="c60a3-113">Například můžete přidat vypočítané hodnoty; nebo čísla verzí, podle kterého chcete filtrovat data v portálu.</span><span class="sxs-lookup"><span data-stu-id="c60a3-113">For example, you could add calculated values; or version numbers by which to filter the data in the portal.</span></span>
* <span data-ttu-id="c60a3-114">[Rozhraní API sady SDK](app-insights-api-custom-events-metrics.md) se používá k odeslání vlastní události a metriky.</span><span class="sxs-lookup"><span data-stu-id="c60a3-114">[The SDK API](app-insights-api-custom-events-metrics.md) is used to send custom events and metrics.</span></span>

<span data-ttu-id="c60a3-115">Než začnete, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="c60a3-115">Before you start:</span></span>

* <span data-ttu-id="c60a3-116">Nainstalujte službu Application Insights [SDK pro technologii ASP.NET](app-insights-asp-net.md) nebo [SDK pro jazyk Java](app-insights-java-get-started.md) ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c60a3-116">Install the Application Insights [SDK for ASP.NET](app-insights-asp-net.md) or [SDK for Java](app-insights-java-get-started.md) in your app.</span></span>

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a><span data-ttu-id="c60a3-117">Filtrování: ITelemetryProcessor</span><span class="sxs-lookup"><span data-stu-id="c60a3-117">Filtering: ITelemetryProcessor</span></span>
<span data-ttu-id="c60a3-118">Tento postup vám dává větší kontrolu nad co je zahrnout nebo vyloučit z datový proud telemetrie.</span><span class="sxs-lookup"><span data-stu-id="c60a3-118">This technique gives you more direct control over what is included or excluded from the telemetry stream.</span></span> <span data-ttu-id="c60a3-119">Můžete jej použít ve spojení s vzorkování, nebo samostatně.</span><span class="sxs-lookup"><span data-stu-id="c60a3-119">You can use it in conjunction with Sampling, or separately.</span></span>

<span data-ttu-id="c60a3-120">Pro filtrování telemetrie, zápis procesor telemetrie a zaregistrovat ji pomocí sady SDK.</span><span class="sxs-lookup"><span data-stu-id="c60a3-120">To filter telemetry, you write a telemetry processor and register it with the SDK.</span></span> <span data-ttu-id="c60a3-121">Všechny telemetrická prochází procesor a můžete ho vyřadit z datového proudu, nebo přidejte vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c60a3-121">All telemetry goes through your processor, and you can choose to drop it from the stream, or add properties.</span></span> <span data-ttu-id="c60a3-122">To zahrnuje telemetrie z standardní moduly, například kolekce požadavku HTTP a collector závislost, a také telemetrických dat, který jste napsali sami.</span><span class="sxs-lookup"><span data-stu-id="c60a3-122">This includes telemetry from the standard modules such as the HTTP request collector and the dependency collector, as well as telemetry you have written yourself.</span></span> <span data-ttu-id="c60a3-123">Můžete například odfiltrovat telemetrická data týkající se požadavků z robotů nebo volání úspěšné závislostí.</span><span class="sxs-lookup"><span data-stu-id="c60a3-123">You can, for example, filter out telemetry about requests from robots, or successful dependency calls.</span></span>

> [!WARNING]
> <span data-ttu-id="c60a3-124">Filtrování telemetrická data odesílaná ze sady SDK pomocí procesorů můžete zkreslit statistiky, které se zobrazí na portálu a je obtížné sledovat související položky.</span><span class="sxs-lookup"><span data-stu-id="c60a3-124">Filtering the telemetry sent from the SDK using processors can skew the statistics that you see in the portal, and make it difficult to follow related items.</span></span>
>
> <span data-ttu-id="c60a3-125">Místo toho zvažte použití [vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="c60a3-125">Instead, consider using [sampling](app-insights-sampling.md).</span></span>
>
>

### <a name="create-a-telemetry-processor-c"></a><span data-ttu-id="c60a3-126">Vytvoření telemetrie procesoru (C#)</span><span class="sxs-lookup"><span data-stu-id="c60a3-126">Create a telemetry processor (C#)</span></span>
1. <span data-ttu-id="c60a3-127">Ověřte, zda je ve vašem projektu Application Insights SDK verze 2.0.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c60a3-127">Verify that the Application Insights SDK in your project is  version 2.0.0 or later.</span></span> <span data-ttu-id="c60a3-128">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení Visual Studio a zvolte spravovat balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="c60a3-128">Right-click your project in Visual Studio Solution Explorer and choose Manage NuGet Packages.</span></span> <span data-ttu-id="c60a3-129">V Správce balíčků NuGet zkontrolujte Microsoft.ApplicationInsights.Web.</span><span class="sxs-lookup"><span data-stu-id="c60a3-129">In NuGet package manager, check Microsoft.ApplicationInsights.Web.</span></span>
2. <span data-ttu-id="c60a3-130">Chcete-li vytvořit filtr, implementujte ITelemetryProcessor.</span><span class="sxs-lookup"><span data-stu-id="c60a3-130">To create a filter, implement ITelemetryProcessor.</span></span> <span data-ttu-id="c60a3-131">Toto je jiný bod rozšiřitelnost jako modul telemetrie, inicializátoru telemetrie a telemetrie kanál.</span><span class="sxs-lookup"><span data-stu-id="c60a3-131">This is another extensibility point like telemetry module, telemetry initializer, and telemetry channel.</span></span>

    <span data-ttu-id="c60a3-132">Všimněte si, že Telemetrie procesory vytvořit řetěz zpracování.</span><span class="sxs-lookup"><span data-stu-id="c60a3-132">Notice that Telemetry Processors construct a chain of processing.</span></span> <span data-ttu-id="c60a3-133">Instanci můžete vytvořit procesor telemetrie, předáte odkaz na další procesoru v řetězu.</span><span class="sxs-lookup"><span data-stu-id="c60a3-133">When you instantiate a telemetry processor, you pass a link to the next processor in the chain.</span></span> <span data-ttu-id="c60a3-134">Když je bod data telemetrie předaný metodě proces, nemá svou práci a pak zavolá další procesor Telemetrie v řetězu.</span><span class="sxs-lookup"><span data-stu-id="c60a3-134">When a telemetry data point is passed to the Process method, it does its work and then calls the next Telemetry Processor in the chain.</span></span>

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {

        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return
            if (!OKtoSend(item)) { return; }
            // Modify the item if required
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }

    ```
1. <span data-ttu-id="c60a3-135">Vložte tuto položku v souboru ApplicationInsights.config:</span><span class="sxs-lookup"><span data-stu-id="c60a3-135">Insert this in ApplicationInsights.config:</span></span>

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

<span data-ttu-id="c60a3-136">(To je do stejné části, kde inicializovat vzorkování filtr.)</span><span class="sxs-lookup"><span data-stu-id="c60a3-136">(This is the same section where you initialize a sampling filter.)</span></span>

<span data-ttu-id="c60a3-137">Můžete předat hodnoty řetězce ze souboru .config tím, že poskytuje veřejné s názvem vlastnosti v třídě.</span><span class="sxs-lookup"><span data-stu-id="c60a3-137">You can pass string values from the .config file by providing public named properties in your class.</span></span>

> [!WARNING]
> <span data-ttu-id="c60a3-138">Vezměte v potaz tak, aby odpovídaly název typu a všechny názvy vlastností v souboru config názvy třídy a vlastnosti v kódu.</span><span class="sxs-lookup"><span data-stu-id="c60a3-138">Take care to match the type name and any property names in the .config file to the class and property names in the code.</span></span> <span data-ttu-id="c60a3-139">Pokud soubor .config odkazuje na neexistující typ nebo vlastnost, sada SDK může bez upozornění nepodaří odesílat všechny telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="c60a3-139">If the .config file references a non-existent type or property, the SDK may silently fail to send any telemetry.</span></span>
>
>

<span data-ttu-id="c60a3-140">**Alternativně** můžete inicializovat filtru v kódu.</span><span class="sxs-lookup"><span data-stu-id="c60a3-140">**Alternatively,** you can initialize the filter in code.</span></span> <span data-ttu-id="c60a3-141">Ve třídě vhodný inicializace – například AppStart v Global.asax.cs - vložte do řetězu procesor:</span><span class="sxs-lookup"><span data-stu-id="c60a3-141">In a suitable initialization class - for example AppStart in Global.asax.cs - insert your processor into the chain:</span></span>

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="c60a3-142">TelemetryClients vytvořené po tomto bodu použije vaše procesory.</span><span class="sxs-lookup"><span data-stu-id="c60a3-142">TelemetryClients created after this point will use your processors.</span></span>

### <a name="example-filters"></a><span data-ttu-id="c60a3-143">Příklad filtrů</span><span class="sxs-lookup"><span data-stu-id="c60a3-143">Example filters</span></span>
#### <a name="synthetic-requests"></a><span data-ttu-id="c60a3-144">Syntetické požadavků</span><span class="sxs-lookup"><span data-stu-id="c60a3-144">Synthetic requests</span></span>
<span data-ttu-id="c60a3-145">Filtrovat robotů a webové testy.</span><span class="sxs-lookup"><span data-stu-id="c60a3-145">Filter out bots and web tests.</span></span> <span data-ttu-id="c60a3-146">I když Průzkumníku metrik poskytuje možnost filtrovat syntetické zdroje, tato možnost snižuje zatížení filtrování je v sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="c60a3-146">Although Metrics Explorer gives you the option to filter out synthetic sources, this option reduces traffic by filtering them at the SDK.</span></span>

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else:
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a><span data-ttu-id="c60a3-147">Ověřování se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="c60a3-147">Failed authentication</span></span>
<span data-ttu-id="c60a3-148">Filtrování požadavků s odpovědi "401".</span><span class="sxs-lookup"><span data-stu-id="c60a3-148">Filter out requests with a "401" response.</span></span>

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain:
        return;
    }
    // Send everything else:
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a><span data-ttu-id="c60a3-149">Odfiltrovat rychlé vzdálené závislostí volání</span><span class="sxs-lookup"><span data-stu-id="c60a3-149">Filter out fast remote dependency calls</span></span>
<span data-ttu-id="c60a3-150">Pokud chcete diagnostikovat volání, které jsou pomalé, odfiltrovat ty fast.</span><span class="sxs-lookup"><span data-stu-id="c60a3-150">If you only want to diagnose calls that are slow, filter out the fast ones.</span></span>

> [!NOTE]
> <span data-ttu-id="c60a3-151">To bude zkreslit statistiky, které vidíte na portálu.</span><span class="sxs-lookup"><span data-stu-id="c60a3-151">This will skew the statistics you see on the portal.</span></span> <span data-ttu-id="c60a3-152">Graf závislostí bude vypadat jako, pokud jsou všechny chyby při volání závislostí.</span><span class="sxs-lookup"><span data-stu-id="c60a3-152">The dependency chart will look as if the dependency calls are all failures.</span></span>
>
>

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;

    if (request != null && request.Duration.TotalMilliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a><span data-ttu-id="c60a3-153">Diagnóza problémů se závislostí</span><span class="sxs-lookup"><span data-stu-id="c60a3-153">Diagnose dependency issues</span></span>
<span data-ttu-id="c60a3-154">[Tomto blogu](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) popisuje projekt k vyřešení problémů se závislostí automaticky odesláním regulární příkazy ping závislosti.</span><span class="sxs-lookup"><span data-stu-id="c60a3-154">[This blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) describes a project to diagnose dependency issues by automatically sending regular pings to dependencies.</span></span>


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a><span data-ttu-id="c60a3-155">Přidat vlastnosti: ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="c60a3-155">Add properties: ITelemetryInitializer</span></span>
<span data-ttu-id="c60a3-156">Inicializátory telemetrická data použít k definování globální vlastnosti, které se odesílají s všechny telemetrická; a přepsání vybrané chování modulů standardní telemetrie.</span><span class="sxs-lookup"><span data-stu-id="c60a3-156">Use telemetry initializers to define global properties that are sent with all telemetry; and to override selected behavior of the standard telemetry modules.</span></span>

<span data-ttu-id="c60a3-157">Například Application Insights pro webové balíček shromažďuje telemetrická data o požadavcích HTTP.</span><span class="sxs-lookup"><span data-stu-id="c60a3-157">For example, the Application Insights for Web package collects telemetry about HTTP requests.</span></span> <span data-ttu-id="c60a3-158">Ve výchozím nastavení, označuje jako neúspěšný každá žádost s kódem odpovědi > = 400.</span><span class="sxs-lookup"><span data-stu-id="c60a3-158">By default, it flags as failed any request with a response code >= 400.</span></span> <span data-ttu-id="c60a3-159">Ale pokud budete chtít 400 považovat za úspěšné, můžete zadat inicializátoru telemetrická data, která nastaví vlastnost úspěch.</span><span class="sxs-lookup"><span data-stu-id="c60a3-159">But if you want to treat 400 as a success, you can provide a telemetry initializer that sets the Success property.</span></span>

<span data-ttu-id="c60a3-160">Pokud zadáte inicializátoru telemetrie, nazývá metod Track*() vždy, když je volána.</span><span class="sxs-lookup"><span data-stu-id="c60a3-160">If you provide a telemetry initializer, it is called whenever any of the Track*() methods is called.</span></span> <span data-ttu-id="c60a3-161">To zahrnuje metody, které jsou volány moduly standardní telemetrie.</span><span class="sxs-lookup"><span data-stu-id="c60a3-161">This includes methods called by the standard telemetry modules.</span></span> <span data-ttu-id="c60a3-162">Podle konvence nenastavujte tyto moduly jakákoli vlastnost, která již byla nastavena pomocí inicializátoru.</span><span class="sxs-lookup"><span data-stu-id="c60a3-162">By convention, these modules do not set any property that has already been set by an initializer.</span></span>

<span data-ttu-id="c60a3-163">**Zadejte vaše inicializátoru**</span><span class="sxs-lookup"><span data-stu-id="c60a3-163">**Define your initializer**</span></span>

<span data-ttu-id="c60a3-164">*C#*</span><span class="sxs-lookup"><span data-stu-id="c60a3-164">*C#*</span></span>

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK
       * behavior of treating response codes >= 400 as failed requests
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

<span data-ttu-id="c60a3-165">**Načíst vaše inicializátoru**</span><span class="sxs-lookup"><span data-stu-id="c60a3-165">**Load your initializer**</span></span>

<span data-ttu-id="c60a3-166">V souboru ApplicationInsights.config:</span><span class="sxs-lookup"><span data-stu-id="c60a3-166">In ApplicationInsights.config:</span></span>

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

<span data-ttu-id="c60a3-167">*Alternativně* můžete vytvořit instanci inicializátoru v kódu, například v souboru Global.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="c60a3-167">*Alternatively,* you can instantiate the initializer in code, for example in Global.aspx.cs:</span></span>

```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[<span data-ttu-id="c60a3-168">Další informace naleznete v této ukázky.</span><span class="sxs-lookup"><span data-stu-id="c60a3-168">See more of this sample.</span></span>](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="javascript-telemetry-initializers"></a><span data-ttu-id="c60a3-169">Inicializátory telemetrie JavaScript</span><span class="sxs-lookup"><span data-stu-id="c60a3-169">JavaScript telemetry initializers</span></span>
<span data-ttu-id="c60a3-170">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="c60a3-170">*JavaScript*</span></span>

<span data-ttu-id="c60a3-171">Vložte inicializátoru telemetrie hned po inicializaci kód, který jste získali z portálu:</span><span class="sxs-lookup"><span data-stu-id="c60a3-171">Insert a telemetry initializer immediately after the initialization code that you got from the portal:</span></span>

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

<span data-ttu-id="c60a3-172">Souhrn k dispozici na telemetryItem bez vlastní vlastnosti, najdete v části [Application Insights Exportovat datový Model](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="c60a3-172">For a summary of the non-custom properties available on the telemetryItem, see [Application Insights Export Data Model](app-insights-export-data-model.md).</span></span>

<span data-ttu-id="c60a3-173">Můžete přidat libovolný počet inicializátory, jak se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="c60a3-173">You can add as many initializers as you like.</span></span>

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a><span data-ttu-id="c60a3-174">ITelemetryProcessor a ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="c60a3-174">ITelemetryProcessor and ITelemetryInitializer</span></span>
<span data-ttu-id="c60a3-175">Jaký je rozdíl mezi procesory telemetrie a inicializátory telemetrie?</span><span class="sxs-lookup"><span data-stu-id="c60a3-175">What's the difference between telemetry processors and telemetry initializers?</span></span>

* <span data-ttu-id="c60a3-176">Existují některé překrytí v co můžete dělat s nimi: jak lze použít k přidání vlastností do telemetrie.</span><span class="sxs-lookup"><span data-stu-id="c60a3-176">There are some overlaps in what you can do with them: both can be used to add properties to telemetry.</span></span>
* <span data-ttu-id="c60a3-177">Vždy spustit před TelemetryProcessors TelemetryInitializers.</span><span class="sxs-lookup"><span data-stu-id="c60a3-177">TelemetryInitializers always run before TelemetryProcessors.</span></span>
* <span data-ttu-id="c60a3-178">TelemetryProcessors umožňují zcela nahradit nebo zahodit položku telemetrie.</span><span class="sxs-lookup"><span data-stu-id="c60a3-178">TelemetryProcessors allow you to completely replace or discard a telemetry item.</span></span>
* <span data-ttu-id="c60a3-179">TelemetryProcessors nemáte zpracovat telemetrická data čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="c60a3-179">TelemetryProcessors don't process performance counter telemetry.</span></span>


## <a name="reference-docs"></a><span data-ttu-id="c60a3-180">Referenční dokumenty</span><span class="sxs-lookup"><span data-stu-id="c60a3-180">Reference docs</span></span>
* [<span data-ttu-id="c60a3-181">Přehled rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c60a3-181">API Overview</span></span>](app-insights-api-custom-events-metrics.md)
* [<span data-ttu-id="c60a3-182">Rozhraní ASP.NET – reference</span><span class="sxs-lookup"><span data-stu-id="c60a3-182">ASP.NET reference</span></span>](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a><span data-ttu-id="c60a3-183">Kód SDK</span><span class="sxs-lookup"><span data-stu-id="c60a3-183">SDK Code</span></span>
* [<span data-ttu-id="c60a3-184">ASP.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="c60a3-184">ASP.NET Core SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [<span data-ttu-id="c60a3-185">ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="c60a3-185">ASP.NET 5</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [<span data-ttu-id="c60a3-186">JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="c60a3-186">JavaScript SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-JS)

## <span data-ttu-id="c60a3-187"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="c60a3-187"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="c60a3-188">Hledání událostí a protokolů</span><span class="sxs-lookup"><span data-stu-id="c60a3-188">Search events and logs</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="c60a3-189">Vzorkování</span><span class="sxs-lookup"><span data-stu-id="c60a3-189">Sampling</span></span>](app-insights-sampling.md)
* [<span data-ttu-id="c60a3-190">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="c60a3-190">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)
