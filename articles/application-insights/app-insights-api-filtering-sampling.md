---
title: "aaaFiltering a předzpracování v hello Azure Application Insights SDK | Microsoft Docs"
description: "Zápis inicializátory Telemetrie a Telemetrie procesorů pro hello SDK toofilter nebo přidání vlastnosti toohello dat před odesláním telemetrie hello toohello portál Application Insights."
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
ms.openlocfilehash: 51b9db69b2375b8799718f1b0e1af77620dc2692
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="filtering-and-preprocessing-telemetry-in-hello-application-insights-sdk"></a><span data-ttu-id="5b4e2-103">Filtrování a předběžného zpracování telemetrie hello Application Insights SDK</span><span class="sxs-lookup"><span data-stu-id="5b4e2-103">Filtering and preprocessing telemetry in hello Application Insights SDK</span></span>


<span data-ttu-id="5b4e2-104">Můžete napsat a konfigurovat moduly plug-in pro hello Application Insights SDK toocustomize jak telemetrie bude zachycen a před odesláním toohello služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-104">You can write and configure plug-ins for hello Application Insights SDK toocustomize how telemetry is captured and processed before it is sent toohello Application Insights service.</span></span>

* <span data-ttu-id="5b4e2-105">[Vzorkování](app-insights-sampling.md) snižuje objem hello telemetrie bez ovlivnění statistice.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-105">[Sampling](app-insights-sampling.md) reduces hello volume of telemetry without affecting your statistics.</span></span> <span data-ttu-id="5b4e2-106">Udržuje společně související datových bodů tak, aby můžete procházet mezi nimi při diagnostikování problému.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-106">It keeps together related data points so that you can navigate between them when diagnosing a problem.</span></span> <span data-ttu-id="5b4e2-107">Hello portálu celkový počet hello je vynásobená toocompensate pro hello vzorkování.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-107">In hello portal, hello total counts are multiplied toocompensate for hello sampling.</span></span>
* <span data-ttu-id="5b4e2-108">Filtrování s procesory Telemetrie [pro technologii ASP.NET](#filtering) nebo [Java](app-insights-java-filter-telemetry.md) umožňuje vybrat nebo upravit telemetrie hello SDK před odesláním toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-108">Filtering with Telemetry Processors [for ASP.NET](#filtering) or [Java](app-insights-java-filter-telemetry.md) lets you select or modify telemetry in hello SDK before it is sent toohello server.</span></span> <span data-ttu-id="5b4e2-109">Například může snížit hello svazku telemetrie tak, že vyloučíte z robotů požadavky.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-109">For example, you could reduce hello volume of telemetry by excluding requests from robots.</span></span> <span data-ttu-id="5b4e2-110">Ale filtrování je více základní provoz tooreducing přístup než vzorkování.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-110">But filtering is a more basic approach tooreducing traffic than sampling.</span></span> <span data-ttu-id="5b4e2-111">Umožňuje větší kontrolu nad co se přenášejí, ale máte toobe vědět, že ovlivňuje statistice – například pokud můžete filtrovat všechny úspěšné požadavky.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-111">It allows you more control over what is transmitted, but you have toobe aware that it affects your statistics - for example, if you filter out all successful requests.</span></span>
* <span data-ttu-id="5b4e2-112">[Inicializátory telemetrie přidávat vlastnosti](#add-properties) tooany telemetrická data odesílaná z vaší aplikace, včetně telemetrie z hello standardní moduly.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-112">[Telemetry Initializers add properties](#add-properties) tooany telemetry sent from your app, including telemetry from hello standard modules.</span></span> <span data-ttu-id="5b4e2-113">Například můžete přidat vypočítané hodnoty; nebo čísla verzí pomocí datových hello toofilter hello portálu.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-113">For example, you could add calculated values; or version numbers by which toofilter hello data in hello portal.</span></span>
* <span data-ttu-id="5b4e2-114">[Hello rozhraní API sady SDK](app-insights-api-custom-events-metrics.md) je použité toosend vlastní události a metriky.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-114">[hello SDK API](app-insights-api-custom-events-metrics.md) is used toosend custom events and metrics.</span></span>

<span data-ttu-id="5b4e2-115">Než začnete, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="5b4e2-115">Before you start:</span></span>

* <span data-ttu-id="5b4e2-116">Nainstalujte službu hello Application Insights [SDK pro technologii ASP.NET](app-insights-asp-net.md) nebo [SDK pro jazyk Java](app-insights-java-get-started.md) ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-116">Install hello Application Insights [SDK for ASP.NET](app-insights-asp-net.md) or [SDK for Java](app-insights-java-get-started.md) in your app.</span></span>

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a><span data-ttu-id="5b4e2-117">Filtrování: ITelemetryProcessor</span><span class="sxs-lookup"><span data-stu-id="5b4e2-117">Filtering: ITelemetryProcessor</span></span>
<span data-ttu-id="5b4e2-118">Tento postup vám dává větší kontrolu nad co je zahrnout nebo vyloučit z datový proud telemetrie hello.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-118">This technique gives you more direct control over what is included or excluded from hello telemetry stream.</span></span> <span data-ttu-id="5b4e2-119">Můžete jej použít ve spojení s vzorkování, nebo samostatně.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-119">You can use it in conjunction with Sampling, or separately.</span></span>

<span data-ttu-id="5b4e2-120">telemetrie toofilter zápisu procesor telemetrie a zaregistrovat ji pomocí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-120">toofilter telemetry, you write a telemetry processor and register it with hello SDK.</span></span> <span data-ttu-id="5b4e2-121">Všechny telemetrická prochází procesor, a můžete toodrop z hello Streamovat nebo přidání vlastností.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-121">All telemetry goes through your processor, and you can choose toodrop it from hello stream, or add properties.</span></span> <span data-ttu-id="5b4e2-122">To zahrnuje telemetrie z hello standardní moduly, například kolekce požadavku HTTP hello a hello závislosti kolekcí a také telemetrických dat, který jste napsali sami.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-122">This includes telemetry from hello standard modules such as hello HTTP request collector and hello dependency collector, as well as telemetry you have written yourself.</span></span> <span data-ttu-id="5b4e2-123">Můžete například odfiltrovat telemetrická data týkající se požadavků z robotů nebo volání úspěšné závislostí.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-123">You can, for example, filter out telemetry about requests from robots, or successful dependency calls.</span></span>

> [!WARNING]
> <span data-ttu-id="5b4e2-124">Filtrování hello telemetrická data odesílaná z hello SDK pomocí procesorů zkreslit hello statistiky hello portálu a nastavit jej jako obtížné toofollow související položky.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-124">Filtering hello telemetry sent from hello SDK using processors can skew hello statistics that you see in hello portal, and make it difficult toofollow related items.</span></span>
>
> <span data-ttu-id="5b4e2-125">Místo toho zvažte použití [vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="5b4e2-125">Instead, consider using [sampling](app-insights-sampling.md).</span></span>
>
>

### <a name="create-a-telemetry-processor-c"></a><span data-ttu-id="5b4e2-126">Vytvoření telemetrie procesoru (C#)</span><span class="sxs-lookup"><span data-stu-id="5b4e2-126">Create a telemetry processor (C#)</span></span>
1. <span data-ttu-id="5b4e2-127">Ověřte, že hello Application Insights SDK do projektu je verze 2.0.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-127">Verify that hello Application Insights SDK in your project is  version 2.0.0 or later.</span></span> <span data-ttu-id="5b4e2-128">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení Visual Studio a zvolte spravovat balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-128">Right-click your project in Visual Studio Solution Explorer and choose Manage NuGet Packages.</span></span> <span data-ttu-id="5b4e2-129">V Správce balíčků NuGet zkontrolujte Microsoft.ApplicationInsights.Web.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-129">In NuGet package manager, check Microsoft.ApplicationInsights.Web.</span></span>
2. <span data-ttu-id="5b4e2-130">toocreate filtr, implementovat ITelemetryProcessor.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-130">toocreate a filter, implement ITelemetryProcessor.</span></span> <span data-ttu-id="5b4e2-131">Toto je jiný bod rozšiřitelnost jako modul telemetrie, inicializátoru telemetrie a telemetrie kanál.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-131">This is another extensibility point like telemetry module, telemetry initializer, and telemetry channel.</span></span>

    <span data-ttu-id="5b4e2-132">Všimněte si, že Telemetrie procesory vytvořit řetěz zpracování.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-132">Notice that Telemetry Processors construct a chain of processing.</span></span> <span data-ttu-id="5b4e2-133">Můžete vytvořit instanci telemetrie procesor, předáte další procesor toohello odkaz v řetězu hello.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-133">When you instantiate a telemetry processor, you pass a link toohello next processor in hello chain.</span></span> <span data-ttu-id="5b4e2-134">Když bod telemetrická data předána metoda proces toohello, dělá svou práci a pak volání hello další procesor Telemetrie v řetězu hello.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-134">When a telemetry data point is passed toohello Process method, it does its work and then calls hello next Telemetry Processor in hello chain.</span></span>

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {

        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors tooeach other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // toofilter out an item, just return
            if (!OKtoSend(item)) { return; }
            // Modify hello item if required
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
1. <span data-ttu-id="5b4e2-135">Vložte tuto položku v souboru ApplicationInsights.config:</span><span class="sxs-lookup"><span data-stu-id="5b4e2-135">Insert this in ApplicationInsights.config:</span></span>

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

<span data-ttu-id="5b4e2-136">(Toto je hello stejné části, kde inicializovat vzorkování filtr.)</span><span class="sxs-lookup"><span data-stu-id="5b4e2-136">(This is hello same section where you initialize a sampling filter.)</span></span>

<span data-ttu-id="5b4e2-137">Můžete předat hodnoty řetězce ze souboru .config hello tím, že poskytuje veřejné s názvem vlastnosti v třídě.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-137">You can pass string values from hello .config file by providing public named properties in your class.</span></span>

> [!WARNING]
> <span data-ttu-id="5b4e2-138">Vezměte v potaz, název typu hello toomatch a všechny názvy vlastností v třídě toohello soubor .config hello a názvy vlastností v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-138">Take care toomatch hello type name and any property names in hello .config file toohello class and property names in hello code.</span></span> <span data-ttu-id="5b4e2-139">Pokud soubor .config hello odkazuje na neexistující typ nebo vlastnost, hello SDK bezobslužně podařit toosend žádné telemetrie.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-139">If hello .config file references a non-existent type or property, hello SDK may silently fail toosend any telemetry.</span></span>
>
>

<span data-ttu-id="5b4e2-140">**Alternativně** můžete inicializovat hello filtru v kódu.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-140">**Alternatively,** you can initialize hello filter in code.</span></span> <span data-ttu-id="5b4e2-141">Ve třídě vhodný inicializace – například AppStart v Global.asax.cs - vložte do řetězu hello procesor:</span><span class="sxs-lookup"><span data-stu-id="5b4e2-141">In a suitable initialization class - for example AppStart in Global.asax.cs - insert your processor into hello chain:</span></span>

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="5b4e2-142">TelemetryClients vytvořené po tomto bodu použije vaše procesory.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-142">TelemetryClients created after this point will use your processors.</span></span>

### <a name="example-filters"></a><span data-ttu-id="5b4e2-143">Příklad filtrů</span><span class="sxs-lookup"><span data-stu-id="5b4e2-143">Example filters</span></span>
#### <a name="synthetic-requests"></a><span data-ttu-id="5b4e2-144">Syntetické požadavků</span><span class="sxs-lookup"><span data-stu-id="5b4e2-144">Synthetic requests</span></span>
<span data-ttu-id="5b4e2-145">Filtrovat robotů a webové testy.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-145">Filter out bots and web tests.</span></span> <span data-ttu-id="5b4e2-146">I když Průzkumníku metrik poskytuje hello možnost toofilter out syntetické zdroje, tato možnost je filtrování na hello SDK snižuje zatížení.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-146">Although Metrics Explorer gives you hello option toofilter out synthetic sources, this option reduces traffic by filtering them at hello SDK.</span></span>

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else:
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a><span data-ttu-id="5b4e2-147">Ověřování se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="5b4e2-147">Failed authentication</span></span>
<span data-ttu-id="5b4e2-148">Filtrování požadavků s odpovědi "401".</span><span class="sxs-lookup"><span data-stu-id="5b4e2-148">Filter out requests with a "401" response.</span></span>

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // toofilter out an item, just terminate hello chain:
        return;
    }
    // Send everything else:
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a><span data-ttu-id="5b4e2-149">Odfiltrovat rychlé vzdálené závislostí volání</span><span class="sxs-lookup"><span data-stu-id="5b4e2-149">Filter out fast remote dependency calls</span></span>
<span data-ttu-id="5b4e2-150">Pokud chcete pouze toodiagnose volání, které jsou pomalé, filtrujte ty fast hello.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-150">If you only want toodiagnose calls that are slow, filter out hello fast ones.</span></span>

> [!NOTE]
> <span data-ttu-id="5b4e2-151">To bude zkreslit hello statistiky, které vidíte na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-151">This will skew hello statistics you see on hello portal.</span></span> <span data-ttu-id="5b4e2-152">Graf závislostí Hello bude vypadat, jako by volání závislostí hello všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-152">hello dependency chart will look as if hello dependency calls are all failures.</span></span>
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

#### <a name="diagnose-dependency-issues"></a><span data-ttu-id="5b4e2-153">Diagnóza problémů se závislostí</span><span class="sxs-lookup"><span data-stu-id="5b4e2-153">Diagnose dependency issues</span></span>
<span data-ttu-id="5b4e2-154">[Tomto blogu](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) popisuje problémy závislost projektu toodiagnose automaticky odesláním toodependencies regulární příkazy ping.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-154">[This blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) describes a project toodiagnose dependency issues by automatically sending regular pings toodependencies.</span></span>


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a><span data-ttu-id="5b4e2-155">Přidat vlastnosti: ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="5b4e2-155">Add properties: ITelemetryInitializer</span></span>
<span data-ttu-id="5b4e2-156">Pomocí telemetrie inicializátory toodefine globální vlastnosti, které jsou odesílány všechny telemetrická; a toooverride vybraná chování hello standardní telemetrie modulů.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-156">Use telemetry initializers toodefine global properties that are sent with all telemetry; and toooverride selected behavior of hello standard telemetry modules.</span></span>

<span data-ttu-id="5b4e2-157">Například hello Application Insights pro webové balíček shromažďuje telemetrická data o požadavcích HTTP.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-157">For example, hello Application Insights for Web package collects telemetry about HTTP requests.</span></span> <span data-ttu-id="5b4e2-158">Ve výchozím nastavení, označuje jako neúspěšný každá žádost s kódem odpovědi > = 400.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-158">By default, it flags as failed any request with a response code >= 400.</span></span> <span data-ttu-id="5b4e2-159">Ale pokud chcete, aby tootreat 400 jako úspěšné, můžete zadat inicializátoru telemetrická data, která nastaví vlastnost úspěch hello.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-159">But if you want tootreat 400 as a success, you can provide a telemetry initializer that sets hello Success property.</span></span>

<span data-ttu-id="5b4e2-160">Pokud zadáte inicializátoru telemetrie, nazývá se vždy, když žádné z hello Track*() metody je volána.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-160">If you provide a telemetry initializer, it is called whenever any of hello Track*() methods is called.</span></span> <span data-ttu-id="5b4e2-161">To zahrnuje metody, které jsou volány hello standardní telemetrie moduly.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-161">This includes methods called by hello standard telemetry modules.</span></span> <span data-ttu-id="5b4e2-162">Podle konvence nenastavujte tyto moduly jakákoli vlastnost, která již byla nastavena pomocí inicializátoru.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-162">By convention, these modules do not set any property that has already been set by an initializer.</span></span>

<span data-ttu-id="5b4e2-163">**Zadejte vaše inicializátoru**</span><span class="sxs-lookup"><span data-stu-id="5b4e2-163">**Define your initializer**</span></span>

<span data-ttu-id="5b4e2-164">*C#*</span><span class="sxs-lookup"><span data-stu-id="5b4e2-164">*C#*</span></span>

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides hello default SDK
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
                // If we set hello Success property, hello SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us toofilter these requests in hello portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave hello SDK tooset hello Success property      
        }
      }
    }
```

<span data-ttu-id="5b4e2-165">**Načíst vaše inicializátoru**</span><span class="sxs-lookup"><span data-stu-id="5b4e2-165">**Load your initializer**</span></span>

<span data-ttu-id="5b4e2-166">V souboru ApplicationInsights.config:</span><span class="sxs-lookup"><span data-stu-id="5b4e2-166">In ApplicationInsights.config:</span></span>

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

<span data-ttu-id="5b4e2-167">*Alternativně* můžete vytvořit instanci hello inicializátoru v kódu, například v souboru Global.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="5b4e2-167">*Alternatively,* you can instantiate hello initializer in code, for example in Global.aspx.cs:</span></span>

```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[<span data-ttu-id="5b4e2-168">Další informace naleznete v této ukázky.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-168">See more of this sample.</span></span>](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="javascript-telemetry-initializers"></a><span data-ttu-id="5b4e2-169">Inicializátory telemetrie JavaScript</span><span class="sxs-lookup"><span data-stu-id="5b4e2-169">JavaScript telemetry initializers</span></span>
<span data-ttu-id="5b4e2-170">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="5b4e2-170">*JavaScript*</span></span>

<span data-ttu-id="5b4e2-171">Vložte inicializátoru telemetrie ihned po hello inicializace kód, který jste získali z portálu hello:</span><span class="sxs-lookup"><span data-stu-id="5b4e2-171">Insert a telemetry initializer immediately after hello initialization code that you got from hello portal:</span></span>

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

                // toocheck hello telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // tooset custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // tooset custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

<span data-ttu-id="5b4e2-172">Souhrn hello bez vlastní vlastnosti v hello telemetryItem k dispozici, najdete v části [Application Insights Exportovat datový Model](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="5b4e2-172">For a summary of hello non-custom properties available on hello telemetryItem, see [Application Insights Export Data Model](app-insights-export-data-model.md).</span></span>

<span data-ttu-id="5b4e2-173">Můžete přidat libovolný počet inicializátory, jak se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-173">You can add as many initializers as you like.</span></span>

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a><span data-ttu-id="5b4e2-174">ITelemetryProcessor a ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="5b4e2-174">ITelemetryProcessor and ITelemetryInitializer</span></span>
<span data-ttu-id="5b4e2-175">Co je hello rozdíl mezi procesory telemetrie a inicializátory telemetrie?</span><span class="sxs-lookup"><span data-stu-id="5b4e2-175">What's hello difference between telemetry processors and telemetry initializers?</span></span>

* <span data-ttu-id="5b4e2-176">Existují některé překrytí v co můžete dělat s nimi: lze použít tooadd vlastnosti tootelemetry i.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-176">There are some overlaps in what you can do with them: both can be used tooadd properties tootelemetry.</span></span>
* <span data-ttu-id="5b4e2-177">Vždy spustit před TelemetryProcessors TelemetryInitializers.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-177">TelemetryInitializers always run before TelemetryProcessors.</span></span>
* <span data-ttu-id="5b4e2-178">TelemetryProcessors umožňují toocompletely nahradit nebo zrušení položku telemetrie.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-178">TelemetryProcessors allow you toocompletely replace or discard a telemetry item.</span></span>
* <span data-ttu-id="5b4e2-179">TelemetryProcessors nemáte zpracovat telemetrická data čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="5b4e2-179">TelemetryProcessors don't process performance counter telemetry.</span></span>


## <a name="reference-docs"></a><span data-ttu-id="5b4e2-180">Referenční dokumenty</span><span class="sxs-lookup"><span data-stu-id="5b4e2-180">Reference docs</span></span>
* [<span data-ttu-id="5b4e2-181">Přehled rozhraní API</span><span class="sxs-lookup"><span data-stu-id="5b4e2-181">API Overview</span></span>](app-insights-api-custom-events-metrics.md)
* [<span data-ttu-id="5b4e2-182">Rozhraní ASP.NET – reference</span><span class="sxs-lookup"><span data-stu-id="5b4e2-182">ASP.NET reference</span></span>](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a><span data-ttu-id="5b4e2-183">Kód SDK</span><span class="sxs-lookup"><span data-stu-id="5b4e2-183">SDK Code</span></span>
* [<span data-ttu-id="5b4e2-184">ASP.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="5b4e2-184">ASP.NET Core SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [<span data-ttu-id="5b4e2-185">ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="5b4e2-185">ASP.NET 5</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [<span data-ttu-id="5b4e2-186">JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="5b4e2-186">JavaScript SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-JS)

## <span data-ttu-id="5b4e2-187"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b4e2-187"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="5b4e2-188">Hledání událostí a protokolů</span><span class="sxs-lookup"><span data-stu-id="5b4e2-188">Search events and logs</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="5b4e2-189">Vzorkování</span><span class="sxs-lookup"><span data-stu-id="5b4e2-189">Sampling</span></span>](app-insights-sampling.md)
* [<span data-ttu-id="5b4e2-190">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="5b4e2-190">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)
