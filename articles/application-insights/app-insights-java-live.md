---
title: "Application Insights pro webové aplikace v jazyce Java, které jsou již za provozu"
description: "Spustit monitorování webové aplikace, která je již spuštěna na serveru"
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 12f3dbb9-915f-4087-87c9-807286030b0b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/10/2016
ms.author: bwren
ms.openlocfilehash: a2731e3e44f8f3d104d8abc7dbe71fe3a4c3a690
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a><span data-ttu-id="5648d-103">Application Insights pro webové aplikace v jazyce Java, které jsou již za provozu</span><span class="sxs-lookup"><span data-stu-id="5648d-103">Application Insights for Java web apps that are already live</span></span>


<span data-ttu-id="5648d-104">Pokud máte webovou aplikaci, která je již spuštěna na serveru J2EE, můžete začít monitorovat její [Application Insights](app-insights-overview.md) bez nutnosti provádět změny kódu nebo nové kompilace projektu.</span><span class="sxs-lookup"><span data-stu-id="5648d-104">If you have a web application that is already running on your J2EE server, you can start monitoring it with [Application Insights](app-insights-overview.md) without the need to make code changes or recompile your project.</span></span> <span data-ttu-id="5648d-105">Pomocí této možnosti získáte informace o požadavky HTTP odeslané na serveru, neošetřených výjimek a čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="5648d-105">With this option, you get information about HTTP requests sent to your server, unhandled exceptions, and performance counters.</span></span>

<span data-ttu-id="5648d-106">Budete potřebovat předplatné [Microsoft Azure](https://azure.com).</span><span class="sxs-lookup"><span data-stu-id="5648d-106">You'll need a subscription to [Microsoft Azure](https://azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="5648d-107">Postup na této stránce přidá SDK do vaší webové aplikace za běhu.</span><span class="sxs-lookup"><span data-stu-id="5648d-107">The procedure on this page adds the SDK to your web app at runtime.</span></span> <span data-ttu-id="5648d-108">Tento modul runtime instrumentace je užitečné, pokud nechcete aktualizovat nebo znovu sestavte vašeho zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="5648d-108">This runtime instrumentation is useful if you don't want to update or rebuild your source code.</span></span> <span data-ttu-id="5648d-109">Pokud je to možné, doporučujeme, ale [přidejte sadu SDK ke zdrojovému kódu](app-insights-java-get-started.md) místo.</span><span class="sxs-lookup"><span data-stu-id="5648d-109">But if you can, we recommend you [add the SDK to the source code](app-insights-java-get-started.md) instead.</span></span> <span data-ttu-id="5648d-110">Získáte další možnosti, jako je například psaní kódu pro sledování činnosti uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5648d-110">That gives you more options such as writing code to track user activity.</span></span>
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a><span data-ttu-id="5648d-111">1. Získejte klíč instrumentace Application Insights</span><span class="sxs-lookup"><span data-stu-id="5648d-111">1. Get an Application Insights instrumentation key</span></span>
1. <span data-ttu-id="5648d-112">Přihlaste se k [portálu Microsoft Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="5648d-112">Sign in to the [Microsoft Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="5648d-113">Vytvořte nový prostředek Application Insights a nastavte typ aplikace na webovou aplikaci Java.</span><span class="sxs-lookup"><span data-stu-id="5648d-113">Create a new Application Insights resource and set the application type to Java web application.</span></span>
   
    ![Zadejte název, vyberte webovou aplikaci Java a klikněte na možnost Vytvořit](./media/app-insights-java-live/02-create.png)

    <span data-ttu-id="5648d-115">Prostředek se vytvoří během pár sekund.</span><span class="sxs-lookup"><span data-stu-id="5648d-115">The resource is created in a few seconds.</span></span>

4. <span data-ttu-id="5648d-116">Otevřete nový prostředek a získat svůj klíč instrumentace.</span><span class="sxs-lookup"><span data-stu-id="5648d-116">Open the new resource and get its instrumentation key.</span></span> <span data-ttu-id="5648d-117">Tento klíč budete muset za chvíli vložit do projektu kódu.</span><span class="sxs-lookup"><span data-stu-id="5648d-117">You'll need to paste this key into your code project shortly.</span></span>
   
    ![V přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte klíč instrumentace](./media/app-insights-java-live/03-key.png)

## <a name="2-download-the-sdk"></a><span data-ttu-id="5648d-119">2. Stažení sady SDK</span><span class="sxs-lookup"><span data-stu-id="5648d-119">2. Download the SDK</span></span>
1. <span data-ttu-id="5648d-120">Stáhněte si [Application Insights SDK pro jazyk Java](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="5648d-120">Download the [Application Insights SDK for Java](https://aka.ms/aijavasdk).</span></span> 
2. <span data-ttu-id="5648d-121">Na serveru extrahujte obsah sady SDK k adresáři, ze které jsou načtena binární soubory vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="5648d-121">On your server, extract the SDK contents to the directory from which your project binaries are loaded.</span></span> <span data-ttu-id="5648d-122">Pokud používáte Tomcat, se tento adresář by obvykle v části`webapps/<your_app_name>/WEB-INF/lib`</span><span class="sxs-lookup"><span data-stu-id="5648d-122">If you’re using Tomcat, this directory would typically be under `webapps/<your_app_name>/WEB-INF/lib`</span></span>

<span data-ttu-id="5648d-123">Všimněte si, že je potřeba tento postup opakujte pro každou instanci serveru a pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5648d-123">Note that you need to repeat this on each server instance, and for each app.</span></span>

## <a name="3-add-an-application-insights-xml-file"></a><span data-ttu-id="5648d-124">3. Přidání souboru xml Application Insights</span><span class="sxs-lookup"><span data-stu-id="5648d-124">3. Add an Application Insights xml file</span></span>
<span data-ttu-id="5648d-125">Vytvořte soubor ApplicationInsights.xml do složky, ve kterém můžete přidat sadu SDK.</span><span class="sxs-lookup"><span data-stu-id="5648d-125">Create ApplicationInsights.xml in the folder in which you added the SDK.</span></span> <span data-ttu-id="5648d-126">Umístěte do ní následující kód XML.</span><span class="sxs-lookup"><span data-stu-id="5648d-126">Put into it the following XML.</span></span>

<span data-ttu-id="5648d-127">Nahraďte klíč instrumentace, který jste dostali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5648d-127">Substitute the instrumentation key that you got from the Azure portal.</span></span>

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

* <span data-ttu-id="5648d-128">Klíč instrumentace se zasílá společně s každou položkou telemetrie a říká službě Application Insights, aby ho zobrazila v prostředku.</span><span class="sxs-lookup"><span data-stu-id="5648d-128">The instrumentation key is sent along with every item of telemetry and tells Application Insights to display it in your resource.</span></span>
* <span data-ttu-id="5648d-129">Požadavek komponenty HTTP je volitelný.</span><span class="sxs-lookup"><span data-stu-id="5648d-129">The HTTP Request component is optional.</span></span> <span data-ttu-id="5648d-130">Automaticky odesílá telemetrii týkající se žádostí a časů odezvy na portál.</span><span class="sxs-lookup"><span data-stu-id="5648d-130">It automatically sends telemetry about requests and response times to the portal.</span></span>
* <span data-ttu-id="5648d-131">Korelace událostí je doplněk komponenty požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="5648d-131">Events correlation is an addition to the HTTP request component.</span></span> <span data-ttu-id="5648d-132">Přiřadí identifikátor každé žádosti přijaté serverem a přidá ho jako vlastnost každé položce telemetrie jako vlastnost Operation.Id.</span><span class="sxs-lookup"><span data-stu-id="5648d-132">It assigns an identifier to each request received by the server, and adds this identifier as a property to every item of telemetry as the property 'Operation.Id'.</span></span> <span data-ttu-id="5648d-133">Umožňuje korelovat telemetrii související s každou žádostí nastavením filtru v [diagnostické vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="5648d-133">It allows you to correlate the telemetry associated with each request by setting a filter in [diagnostic search](app-insights-diagnostic-search.md).</span></span>

## <a name="4-add-an-http-filter"></a><span data-ttu-id="5648d-134">4. Přidat filtr HTTP</span><span class="sxs-lookup"><span data-stu-id="5648d-134">4. Add an HTTP filter</span></span>
<span data-ttu-id="5648d-135">Vyhledejte a otevřete soubor web.xml ve vašem projektu a slučte následující fragment kódu pod uzlem webové aplikace, které jsou nakonfigurované filtry aplikace.</span><span class="sxs-lookup"><span data-stu-id="5648d-135">Locate and open the web.xml file in your project, and merge the following snippet of code under the web-app node, where your application filters are configured.</span></span>

<span data-ttu-id="5648d-136">Chcete-li získat nejpřesnější výsledky, musí být filtr namapován před všemi ostatními filtry.</span><span class="sxs-lookup"><span data-stu-id="5648d-136">To get the most accurate results, the filter should be mapped before all other filters.</span></span>

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
```

## <a name="5-check-firewall-exceptions"></a><span data-ttu-id="5648d-137">5. Kontrola výjimky brány firewall</span><span class="sxs-lookup"><span data-stu-id="5648d-137">5. Check firewall exceptions</span></span>
<span data-ttu-id="5648d-138">Možná budete muset [nastavit výjimky, které odesílat odchozí data](app-insights-ip-addresses.md).</span><span class="sxs-lookup"><span data-stu-id="5648d-138">You might need to [set exceptions to send outgoing data](app-insights-ip-addresses.md).</span></span>

## <a name="6-restart-your-web-app"></a><span data-ttu-id="5648d-139">6. Restartování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="5648d-139">6. Restart your web app</span></span>
## <a name="7-view-your-telemetry-in-application-insights"></a><span data-ttu-id="5648d-140">7. Zobrazte telemetrii ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="5648d-140">7. View your telemetry in Application Insights</span></span>
<span data-ttu-id="5648d-141">Vraťte se do prostředku Application Insights na web [Microsoft Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5648d-141">Return to your Application Insights resource in [Microsoft Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="5648d-142">Telemetrická data o požadavcích HTTP se zobrazí v okně Přehled.</span><span class="sxs-lookup"><span data-stu-id="5648d-142">Telemetry about HTTP requests appears on the overview blade.</span></span> <span data-ttu-id="5648d-143">(Pokud zde nejsou, počkejte několik sekund a pak klikněte na tlačítko Aktualizovat.)</span><span class="sxs-lookup"><span data-stu-id="5648d-143">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![ukázková data](./media/app-insights-java-live/5-results.png)

<span data-ttu-id="5648d-145">Proklikejte se prostřednictvím jakékoli grafu pro zobrazení podrobnějších metrik.</span><span class="sxs-lookup"><span data-stu-id="5648d-145">Click through any chart to see more detailed metrics.</span></span> 

![](./media/app-insights-java-live/6-barchart.png)

<span data-ttu-id="5648d-146">A při zobrazení vlastností požadavku, uvidíte telemetrické události související s například požadavky a výjimkami.</span><span class="sxs-lookup"><span data-stu-id="5648d-146">And when viewing the properties of a request, you can see the telemetry events associated with it such as requests and exceptions.</span></span>

![](./media/app-insights-java-live/7-instance.png)

[<span data-ttu-id="5648d-147">Další informace o metrikách.</span><span class="sxs-lookup"><span data-stu-id="5648d-147">Learn more about metrics.</span></span>](app-insights-metrics-explorer.md)

## <a name="next-steps"></a><span data-ttu-id="5648d-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5648d-148">Next steps</span></span>
* <span data-ttu-id="5648d-149">[Přidání telemetrických údajů do webových stránek](app-insights-javascript.md) monitorování zobrazení stránek a metrik uživatele.</span><span class="sxs-lookup"><span data-stu-id="5648d-149">[Add telemetry to your web pages](app-insights-javascript.md) to monitor page views and user metrics.</span></span>
* <span data-ttu-id="5648d-150">[Nastavit testy webu](app-insights-monitor-web-app-availability.md) a ujistěte se vaše aplikace zůstává aktivní a reagující.</span><span class="sxs-lookup"><span data-stu-id="5648d-150">[Set up web tests](app-insights-monitor-web-app-availability.md) to make sure your application stays live and responsive.</span></span>
* [<span data-ttu-id="5648d-151">Zaznamenat trasování protokolu</span><span class="sxs-lookup"><span data-stu-id="5648d-151">Capture log traces</span></span>](app-insights-java-trace-logs.md)
* <span data-ttu-id="5648d-152">[Hledání událostí a protokolů](app-insights-diagnostic-search.md) k diagnostice potíží.</span><span class="sxs-lookup"><span data-stu-id="5648d-152">[Search events and logs](app-insights-diagnostic-search.md) to help diagnose problems.</span></span>

