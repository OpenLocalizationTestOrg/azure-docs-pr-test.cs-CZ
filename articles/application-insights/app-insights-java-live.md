---
title: "aaaApplication statistiky pro jazyk Java webové aplikace, které jsou již nasazeny"
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
ms.openlocfilehash: 2b01cd61657522ccf1d2d97b2a29cdeb08ec9a18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a><span data-ttu-id="79644-103">Application Insights pro webové aplikace v jazyce Java, které jsou již za provozu</span><span class="sxs-lookup"><span data-stu-id="79644-103">Application Insights for Java web apps that are already live</span></span>


<span data-ttu-id="79644-104">Pokud máte webovou aplikaci, která je již spuštěna na serveru J2EE, můžete začít monitorovat její [Application Insights](app-insights-overview.md) bez hello potřebovat toomake změny kódu nebo nové kompilace projektu.</span><span class="sxs-lookup"><span data-stu-id="79644-104">If you have a web application that is already running on your J2EE server, you can start monitoring it with [Application Insights](app-insights-overview.md) without hello need toomake code changes or recompile your project.</span></span> <span data-ttu-id="79644-105">Pomocí této možnosti získáte informace o požadavcích HTTP odeslané tooyour server, neošetřených výjimek a čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="79644-105">With this option, you get information about HTTP requests sent tooyour server, unhandled exceptions, and performance counters.</span></span>

<span data-ttu-id="79644-106">Budete potřebovat předplatné příliš[Microsoft Azure](https://azure.com).</span><span class="sxs-lookup"><span data-stu-id="79644-106">You'll need a subscription too[Microsoft Azure](https://azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="79644-107">Postup Hello na této stránce přidá hello SDK tooyour webové aplikace za běhu.</span><span class="sxs-lookup"><span data-stu-id="79644-107">hello procedure on this page adds hello SDK tooyour web app at runtime.</span></span> <span data-ttu-id="79644-108">Tento modul runtime instrumentace je užitečné, pokud si chcete tooupdate nebo znovu sestavte vašeho zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="79644-108">This runtime instrumentation is useful if you don't want tooupdate or rebuild your source code.</span></span> <span data-ttu-id="79644-109">Pokud je to možné, doporučujeme, ale [přidat hello SDK toohello zdrojový kód](app-insights-java-get-started.md) místo.</span><span class="sxs-lookup"><span data-stu-id="79644-109">But if you can, we recommend you [add hello SDK toohello source code](app-insights-java-get-started.md) instead.</span></span> <span data-ttu-id="79644-110">Získáte další možnosti, jako je například zápis aktivity uživatelů tootrack kódu.</span><span class="sxs-lookup"><span data-stu-id="79644-110">That gives you more options such as writing code tootrack user activity.</span></span>
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a><span data-ttu-id="79644-111">1. Získejte klíč instrumentace Application Insights</span><span class="sxs-lookup"><span data-stu-id="79644-111">1. Get an Application Insights instrumentation key</span></span>
1. <span data-ttu-id="79644-112">Přihlaste se toohello [portálu Microsoft Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="79644-112">Sign in toohello [Microsoft Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="79644-113">Vytvořte nový prostředek Application Insights a nastavte hello aplikace typu tooJava webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="79644-113">Create a new Application Insights resource and set hello application type tooJava web application.</span></span>
   
    ![Zadejte název, vyberte webovou aplikaci Java a klikněte na možnost Vytvořit](./media/app-insights-java-live/02-create.png)

    <span data-ttu-id="79644-115">Hello prostředků se vytvoří během pár sekund.</span><span class="sxs-lookup"><span data-stu-id="79644-115">hello resource is created in a few seconds.</span></span>

4. <span data-ttu-id="79644-116">Otevřete hello nového prostředku a získání svůj klíč instrumentace.</span><span class="sxs-lookup"><span data-stu-id="79644-116">Open hello new resource and get its instrumentation key.</span></span> <span data-ttu-id="79644-117">Budete potřebovat toopaste tento klíč do projektu kódu za chvíli.</span><span class="sxs-lookup"><span data-stu-id="79644-117">You'll need toopaste this key into your code project shortly.</span></span>
   
    ![V hello přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte hello klíč instrumentace](./media/app-insights-java-live/03-key.png)

## <a name="2-download-hello-sdk"></a><span data-ttu-id="79644-119">2. Stáhnout hello SDK</span><span class="sxs-lookup"><span data-stu-id="79644-119">2. Download hello SDK</span></span>
1. <span data-ttu-id="79644-120">Stáhnout hello [Application Insights SDK pro jazyk Java](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="79644-120">Download hello [Application Insights SDK for Java](https://aka.ms/aijavasdk).</span></span> 
2. <span data-ttu-id="79644-121">Na serveru extrahujte hello SDK obsah toohello adresář, ve kterém jsou načtená binární soubory vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="79644-121">On your server, extract hello SDK contents toohello directory from which your project binaries are loaded.</span></span> <span data-ttu-id="79644-122">Pokud používáte Tomcat, se tento adresář by obvykle v části`webapps/<your_app_name>/WEB-INF/lib`</span><span class="sxs-lookup"><span data-stu-id="79644-122">If you’re using Tomcat, this directory would typically be under `webapps/<your_app_name>/WEB-INF/lib`</span></span>

<span data-ttu-id="79644-123">Všimněte si, že budete potřebovat toorepeat to na každou instanci serveru a pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="79644-123">Note that you need toorepeat this on each server instance, and for each app.</span></span>

## <a name="3-add-an-application-insights-xml-file"></a><span data-ttu-id="79644-124">3. Přidání souboru xml Application Insights</span><span class="sxs-lookup"><span data-stu-id="79644-124">3. Add an Application Insights xml file</span></span>
<span data-ttu-id="79644-125">Vytvořte soubor ApplicationInsights.xml hello složky, ve které jste přidali hello SDK.</span><span class="sxs-lookup"><span data-stu-id="79644-125">Create ApplicationInsights.xml in hello folder in which you added hello SDK.</span></span> <span data-ttu-id="79644-126">Umístit do ní hello následující XML.</span><span class="sxs-lookup"><span data-stu-id="79644-126">Put into it hello following XML.</span></span>

<span data-ttu-id="79644-127">Nahraďte klíč instrumentace hello, který jste získali z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="79644-127">Substitute hello instrumentation key that you got from hello Azure portal.</span></span>

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- hello key from hello portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data tooeach event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

* <span data-ttu-id="79644-128">klíč instrumentace Hello se odesílají společně s každou položkou telemetrie a říká toodisplay Application Insights je ve vašem prostředku.</span><span class="sxs-lookup"><span data-stu-id="79644-128">hello instrumentation key is sent along with every item of telemetry and tells Application Insights toodisplay it in your resource.</span></span>
* <span data-ttu-id="79644-129">Hello požadavek komponenty HTTP je volitelný.</span><span class="sxs-lookup"><span data-stu-id="79644-129">hello HTTP Request component is optional.</span></span> <span data-ttu-id="79644-130">Automaticky odesílá telemetrii týkající se požadavků a odpovědí časy toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="79644-130">It automatically sends telemetry about requests and response times toohello portal.</span></span>
* <span data-ttu-id="79644-131">Korelace událostí je komponenty požadavku HTTP toohello přidání.</span><span class="sxs-lookup"><span data-stu-id="79644-131">Events correlation is an addition toohello HTTP request component.</span></span> <span data-ttu-id="79644-132">Přiřadí požadavek tooeach identifikátor přijatých hello serveru a přidá tento identifikátor jako položka tooevery vlastnost telemetrie jako hello vlastnost 'Operation.Id'.</span><span class="sxs-lookup"><span data-stu-id="79644-132">It assigns an identifier tooeach request received by hello server, and adds this identifier as a property tooevery item of telemetry as hello property 'Operation.Id'.</span></span> <span data-ttu-id="79644-133">Umožňuje toocorrelate hello telemetrii související s každou žádostí nastavením filtru v [diagnostické vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="79644-133">It allows you toocorrelate hello telemetry associated with each request by setting a filter in [diagnostic search](app-insights-diagnostic-search.md).</span></span>

## <a name="4-add-an-http-filter"></a><span data-ttu-id="79644-134">4. Přidat filtr HTTP</span><span class="sxs-lookup"><span data-stu-id="79644-134">4. Add an HTTP filter</span></span>
<span data-ttu-id="79644-135">Vyhledejte a otevřete soubor web.xml hello v projektu a merge hello následující fragment kódu v rámci uzlu hello webové aplikace, které jsou nakonfigurované filtry aplikace.</span><span class="sxs-lookup"><span data-stu-id="79644-135">Locate and open hello web.xml file in your project, and merge hello following snippet of code under hello web-app node, where your application filters are configured.</span></span>

<span data-ttu-id="79644-136">tooget hello nejpřesnější výsledky, hello filtr by měly být namapované před všemi ostatními filtry.</span><span class="sxs-lookup"><span data-stu-id="79644-136">tooget hello most accurate results, hello filter should be mapped before all other filters.</span></span>

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

## <a name="5-check-firewall-exceptions"></a><span data-ttu-id="79644-137">5. Kontrola výjimky brány firewall</span><span class="sxs-lookup"><span data-stu-id="79644-137">5. Check firewall exceptions</span></span>
<span data-ttu-id="79644-138">Může být nutné příliš[nastavit výjimky, odchozích dat toosend](app-insights-ip-addresses.md).</span><span class="sxs-lookup"><span data-stu-id="79644-138">You might need too[set exceptions toosend outgoing data](app-insights-ip-addresses.md).</span></span>

## <a name="6-restart-your-web-app"></a><span data-ttu-id="79644-139">6. Restartování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="79644-139">6. Restart your web app</span></span>
## <a name="7-view-your-telemetry-in-application-insights"></a><span data-ttu-id="79644-140">7. Zobrazte telemetrii ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="79644-140">7. View your telemetry in Application Insights</span></span>
<span data-ttu-id="79644-141">Vrátí prostředek Application Insights tooyour [portálu Microsoft Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="79644-141">Return tooyour Application Insights resource in [Microsoft Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="79644-142">Telemetrická data o požadavcích HTTP se zobrazí v okně Přehled hello.</span><span class="sxs-lookup"><span data-stu-id="79644-142">Telemetry about HTTP requests appears on hello overview blade.</span></span> <span data-ttu-id="79644-143">(Pokud zde nejsou, počkejte několik sekund a pak klikněte na tlačítko Aktualizovat.)</span><span class="sxs-lookup"><span data-stu-id="79644-143">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![ukázková data](./media/app-insights-java-live/5-results.png)

<span data-ttu-id="79644-145">Klikněte na tlačítko prostřednictvím jakékoli grafu toosee podrobnější metriky.</span><span class="sxs-lookup"><span data-stu-id="79644-145">Click through any chart toosee more detailed metrics.</span></span> 

![](./media/app-insights-java-live/6-barchart.png)

<span data-ttu-id="79644-146">A při zobrazení vlastností hello požadavku, uvidíte hello telemetrické události související s například požadavky a výjimkami.</span><span class="sxs-lookup"><span data-stu-id="79644-146">And when viewing hello properties of a request, you can see hello telemetry events associated with it such as requests and exceptions.</span></span>

![](./media/app-insights-java-live/7-instance.png)

[<span data-ttu-id="79644-147">Další informace o metrikách.</span><span class="sxs-lookup"><span data-stu-id="79644-147">Learn more about metrics.</span></span>](app-insights-metrics-explorer.md)

## <a name="next-steps"></a><span data-ttu-id="79644-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79644-148">Next steps</span></span>
* <span data-ttu-id="79644-149">[Přidat telemetrii tooyour webové stránky](app-insights-javascript.md) toomonitor stránky zobrazení a metrik uživatele.</span><span class="sxs-lookup"><span data-stu-id="79644-149">[Add telemetry tooyour web pages](app-insights-javascript.md) toomonitor page views and user metrics.</span></span>
* <span data-ttu-id="79644-150">[Nastavit testy webu](app-insights-monitor-web-app-availability.md) toomake, že vaše aplikace zůstává aktivní a reagující.</span><span class="sxs-lookup"><span data-stu-id="79644-150">[Set up web tests](app-insights-monitor-web-app-availability.md) toomake sure your application stays live and responsive.</span></span>
* [<span data-ttu-id="79644-151">Zaznamenat trasování protokolu</span><span class="sxs-lookup"><span data-stu-id="79644-151">Capture log traces</span></span>](app-insights-java-trace-logs.md)
* <span data-ttu-id="79644-152">[Hledání událostí a protokolů](app-insights-diagnostic-search.md) toohelp diagnostikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="79644-152">[Search events and logs](app-insights-diagnostic-search.md) toohelp diagnose problems.</span></span>

