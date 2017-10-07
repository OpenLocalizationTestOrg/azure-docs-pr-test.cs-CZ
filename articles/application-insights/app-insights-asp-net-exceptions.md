---
title: "aaaDiagnose chyby a výjimky v webové aplikace pomocí služby Azure Application Insights | Microsoft Docs"
description: "Zachytit výjimky z aplikací ASP.NET společně s telemetrická žádost."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d1e98390-3ce4-4d04-9351-144314a42aa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 8930e6d2b29f83ea635c4ecb7afd11fc1d97d085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a><span data-ttu-id="30f84-103">Diagnostikovat výjimky ve webových aplikacích pomocí služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="30f84-103">Diagnose exceptions in your web apps with Application Insights</span></span>
<span data-ttu-id="30f84-104">Výjimky v svou živou webovou aplikaci oznamuje [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="30f84-104">Exceptions in your live web app are reported by [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="30f84-105">Neúspěšných požadavků mohou korelovat s výjimkami a další události v hello klient i server, takže můžete rychle diagnostikovat hello příčiny.</span><span class="sxs-lookup"><span data-stu-id="30f84-105">You can correlate failed requests with exceptions and other events at both hello client and server, so that you can quickly diagnose hello causes.</span></span>

## <a name="set-up-exception-reporting"></a><span data-ttu-id="30f84-106">Nastavit generování sestav výjimky</span><span class="sxs-lookup"><span data-stu-id="30f84-106">Set up exception reporting</span></span>
* <span data-ttu-id="30f84-107">výjimky toohave nahlášené z vaší aplikace. server:</span><span class="sxs-lookup"><span data-stu-id="30f84-107">toohave exceptions reported from your server app:</span></span>
  * <span data-ttu-id="30f84-108">Nainstalujte [Application Insights SDK](app-insights-asp-net.md) v kódu aplikace, nebo</span><span class="sxs-lookup"><span data-stu-id="30f84-108">Install [Application Insights SDK](app-insights-asp-net.md) in your app code, or</span></span>
  * <span data-ttu-id="30f84-109">Webové servery služby IIS: Spusťte [agenta Application Insights](app-insights-monitor-performance-live-website-now.md); nebo</span><span class="sxs-lookup"><span data-stu-id="30f84-109">IIS web servers: Run [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); or</span></span>
  * <span data-ttu-id="30f84-110">Službě Azure web apps: přidejte hello [rozšíření Application Insights](app-insights-azure-web-apps.md)</span><span class="sxs-lookup"><span data-stu-id="30f84-110">Azure web apps: Add hello [Application Insights Extension](app-insights-azure-web-apps.md)</span></span>
  * <span data-ttu-id="30f84-111">Webové aplikace v jazyce Java: instalace hello [agenta Java](app-insights-java-agent.md)</span><span class="sxs-lookup"><span data-stu-id="30f84-111">Java web apps: Install hello [Java agent](app-insights-java-agent.md)</span></span>
* <span data-ttu-id="30f84-112">Nainstalujte hello [fragment kódu JavaScript](app-insights-javascript.md) ve vaší webové stránky toocatch prohlížeče výjimky.</span><span class="sxs-lookup"><span data-stu-id="30f84-112">Install hello [JavaScript snippet](app-insights-javascript.md) in your web pages toocatch browser exceptions.</span></span>
* <span data-ttu-id="30f84-113">V některých aplikační architektury, nebo s některými nastaveními, musíte tootake některé dodatečné kroky toocatch více výjimek:</span><span class="sxs-lookup"><span data-stu-id="30f84-113">In some application frameworks or with some settings, you need tootake some extra steps toocatch more exceptions:</span></span>
  * [<span data-ttu-id="30f84-114">Webové formuláře</span><span class="sxs-lookup"><span data-stu-id="30f84-114">Web forms</span></span>](#web-forms)
  * [<span data-ttu-id="30f84-115">MVC</span><span class="sxs-lookup"><span data-stu-id="30f84-115">MVC</span></span>](#mvc)
  * [<span data-ttu-id="30f84-116">1.* webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="30f84-116">Web API 1.*</span></span>](#web-api-1)
  * [<span data-ttu-id="30f84-117">2.* webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="30f84-117">Web API 2.*</span></span>](#web-api-2)
  * [<span data-ttu-id="30f84-118">WCF</span><span class="sxs-lookup"><span data-stu-id="30f84-118">WCF</span></span>](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a><span data-ttu-id="30f84-119">Diagnostikování výjimky pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30f84-119">Diagnosing exceptions using Visual Studio</span></span>
<span data-ttu-id="30f84-120">Otevřete v sadě Visual Studio toohelp s laděním řešení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="30f84-120">Open hello app solution in Visual Studio toohelp with debugging.</span></span>

<span data-ttu-id="30f84-121">Spusťte aplikaci hello na vašem serveru nebo na počítači pro vývoj pomocí F5.</span><span class="sxs-lookup"><span data-stu-id="30f84-121">Run hello app, either on your server or on your development machine by using F5.</span></span>

<span data-ttu-id="30f84-122">Otevřete okno hello hledání Application Insights v sadě Visual Studio a nastavte ji toodisplay události z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="30f84-122">Open hello Application Insights Search window in Visual Studio, and set it toodisplay events from your app.</span></span> <span data-ttu-id="30f84-123">Při ladění, můžete k tomu jenom kliknutím na tlačítko Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="30f84-123">While you're debugging, you can do this just by clicking hello Application Insights button.</span></span>

![Klikněte pravým tlačítkem na projekt hello a vyberte Application Insights, otevřete.](./media/app-insights-asp-net-exceptions/34.png)

<span data-ttu-id="30f84-125">Všimněte si, že můžete filtrovat pouze výjimky tooshow hello sestavy.</span><span class="sxs-lookup"><span data-stu-id="30f84-125">Notice that you can filter hello report tooshow just exceptions.</span></span>

<span data-ttu-id="30f84-126">*Žádné výjimky zobrazující? V tématu [zachycení výjimky](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="30f84-126">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>

<span data-ttu-id="30f84-127">Klikněte na jeho trasování zásobníku tooshow sestavy k výjimce.</span><span class="sxs-lookup"><span data-stu-id="30f84-127">Click an exception report tooshow its stack trace.</span></span>
<span data-ttu-id="30f84-128">Klikněte na odkaz řádku v trasování zásobníku hello, tooopen hello relevantní kódu souborů.</span><span class="sxs-lookup"><span data-stu-id="30f84-128">Click a line reference in hello stack trace, tooopen hello relevant code file.</span></span>  

<span data-ttu-id="30f84-129">V kódu hello Všimněte si, že Codelensu zobrazuje data o výjimkách hello:</span><span class="sxs-lookup"><span data-stu-id="30f84-129">In hello code, notice that CodeLens shows data about hello exceptions:</span></span>

![Oznámení Codelensu výjimek.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-hello-azure-portal"></a><span data-ttu-id="30f84-131">Diagnostikování chyb pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="30f84-131">Diagnosing failures using hello Azure portal</span></span>
<span data-ttu-id="30f84-132">Z přehledu Application Insights hello vaší aplikace dlaždici selhání hello ukazuje grafy výjimek a k selhání požadavků HTTP, společně s seznam hello žádosti adresy URL, které způsobí selhání nejčastěji se vyskytující hello.</span><span class="sxs-lookup"><span data-stu-id="30f84-132">From hello Application Insights overview of your app, hello Failures tile shows you charts of exceptions and failed HTTP requests, together with a list of hello request URLs that cause hello most frequent failures.</span></span>

![Vyberte nastavení, selhání](./media/app-insights-asp-net-exceptions/012-start.png)

<span data-ttu-id="30f84-134">Klikněte na tlačítko prostřednictvím jednoho z hello se nezdařilo typy výjimek v hello seznamu tooget tooindividual výskytů hello výjimky, kde můžete zobrazit podrobnosti hello a trasování zásobníku:</span><span class="sxs-lookup"><span data-stu-id="30f84-134">Click through one of hello failed exception types in hello list tooget tooindividual occurrences of hello exception, where you can see hello details and stack trace:</span></span>

![Vyberte instanci chybné žádosti a v části Podrobnosti o výjimce, získat tooinstances hello výjimky.](./media/app-insights-asp-net-exceptions/030-req-drill.png)

<span data-ttu-id="30f84-136">**Alternativně** můžete spustit z hello seznam požadavků a najít související tooit výjimky.</span><span class="sxs-lookup"><span data-stu-id="30f84-136">**Alternatively,** you can start from hello list of requests and find exceptions related tooit.</span></span>

<span data-ttu-id="30f84-137">*Žádné výjimky zobrazující? V tématu [zachycení výjimky](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="30f84-137">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>


## <a name="custom-tracing-and-log-data"></a><span data-ttu-id="30f84-138">Vlastní trasování a data protokolu</span><span class="sxs-lookup"><span data-stu-id="30f84-138">Custom tracing and log data</span></span>
<span data-ttu-id="30f84-139">tooget diagnostických dat konkrétní tooyour aplikace, můžete vložit toosend kód vlastní telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="30f84-139">tooget diagnostic data specific tooyour app, you can insert code toosend your own telemetry data.</span></span> <span data-ttu-id="30f84-140">Toto zobrazí ve vyhledávání diagnostiky spolu s hello požadavku, zobrazení stránky a další automaticky shromažďovat data.</span><span class="sxs-lookup"><span data-stu-id="30f84-140">This displayed in diagnostic search alongside hello request, page view and other automatically-collected data.</span></span>

<span data-ttu-id="30f84-141">Máte několik možností:</span><span class="sxs-lookup"><span data-stu-id="30f84-141">You have several options:</span></span>

* <span data-ttu-id="30f84-142">[TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) se obvykle používá ke sledování vzorů využití, ale také odesílá data se zobrazí v části vlastní události ve vyhledávání diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="30f84-142">[TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) is typically used for monitoring usage patterns, but hello data it sends also appears under Custom Events in diagnostic search.</span></span> <span data-ttu-id="30f84-143">Události jsou pojmenované a přenášet vlastnosti řetězce a číselné metriky, ve kterém můžete [filtrovat diagnostických hledání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="30f84-143">Events are named, and can carry string properties and numeric metrics on which you can [filter your diagnostic searches](app-insights-diagnostic-search.md).</span></span>
* <span data-ttu-id="30f84-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) umožňuje odesílat delší data, jako jsou informace o POST.</span><span class="sxs-lookup"><span data-stu-id="30f84-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) lets you send longer data such as POST information.</span></span>
* <span data-ttu-id="30f84-145">[TrackException()](#exceptions) odešle trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="30f84-145">[TrackException()](#exceptions) sends stack traces.</span></span> <span data-ttu-id="30f84-146">[Další informace o výjimky](#exceptions).</span><span class="sxs-lookup"><span data-stu-id="30f84-146">[More about exceptions](#exceptions).</span></span>
* <span data-ttu-id="30f84-147">Pokud už používáte rozhraní protokolování, jako je Log4Net nebo NLog, můžete [zaznamenat tyto protokoly](app-insights-asp-net-trace-logs.md) a zobrazovat ve vyhledávání diagnostiky společně se data požadavku a výjimky.</span><span class="sxs-lookup"><span data-stu-id="30f84-147">If you already use a logging framework like Log4Net or NLog, you can [capture those logs](app-insights-asp-net-trace-logs.md) and see them in diagnostic search alongside request and exception data.</span></span>

<span data-ttu-id="30f84-148">Otevřete tyto události toosee [vyhledávání](app-insights-diagnostic-search.md), otevřete filtr a potom vyberte vlastní události, trasování nebo výjimky.</span><span class="sxs-lookup"><span data-stu-id="30f84-148">toosee these events, open [Search](app-insights-diagnostic-search.md), open Filter, and then choose Custom Event, Trace, or Exception.</span></span>

![Procházení](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> <span data-ttu-id="30f84-150">Pokud vaše aplikace generuje mnoho telemetrie, hello modul adaptivního vzorkování automaticky sníží hello svazek, který je odeslán toohello portál odesláním pouze reprezentativní části události.</span><span class="sxs-lookup"><span data-stu-id="30f84-150">If your app generates a lot of telemetry, hello adaptive sampling module will automatically reduce hello volume that is sent toohello portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="30f84-151">Události, které jsou součástí hello stejné operace bude vybrán nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi.</span><span class="sxs-lookup"><span data-stu-id="30f84-151">Events that are part of hello same operation will be selected or deselected as a group, so that you can navigate between related events.</span></span> [<span data-ttu-id="30f84-152">Další informace o vzorkování.</span><span class="sxs-lookup"><span data-stu-id="30f84-152">Learn about sampling.</span></span>](app-insights-sampling.md)
>
>

### <a name="how-toosee-request-post-data"></a><span data-ttu-id="30f84-153">Jak toosee POST data žádosti</span><span class="sxs-lookup"><span data-stu-id="30f84-153">How toosee request POST data</span></span>
<span data-ttu-id="30f84-154">Podrobnosti požadavku zahrnutí hello data jsou odeslaná aplikace tooyour volání POST.</span><span class="sxs-lookup"><span data-stu-id="30f84-154">Request details don't include hello data sent tooyour app in a POST call.</span></span> <span data-ttu-id="30f84-155">toohave nahlášené tato data:</span><span class="sxs-lookup"><span data-stu-id="30f84-155">toohave this data reported:</span></span>

* <span data-ttu-id="30f84-156">[Nainstalujte hello SDK](app-insights-asp-net.md) ve vašem projektu aplikace.</span><span class="sxs-lookup"><span data-stu-id="30f84-156">[Install hello SDK](app-insights-asp-net.md) in your application project.</span></span>
* <span data-ttu-id="30f84-157">Vložení kódu do vaší aplikace toocall [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span><span class="sxs-lookup"><span data-stu-id="30f84-157">Insert code in your application toocall [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span></span> <span data-ttu-id="30f84-158">V parametru hello zprávy odesílat hello POST data.</span><span class="sxs-lookup"><span data-stu-id="30f84-158">Send hello POST data in hello message parameter.</span></span> <span data-ttu-id="30f84-159">Neexistuje omezení toohello povolená velikost, takže vyzkoušejte toosend jenom hello základní data.</span><span class="sxs-lookup"><span data-stu-id="30f84-159">There is a limit toohello permitted size, so you should try toosend just hello essential data.</span></span>
* <span data-ttu-id="30f84-160">Při řešení chybné žádosti najdete hello přidružené trasování.</span><span class="sxs-lookup"><span data-stu-id="30f84-160">When you investigate a failed request, find hello associated traces.</span></span>  

![Procházení](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <span data-ttu-id="30f84-162"><a name="exceptions"></a>Zachytávání výjimek a související diagnostických dat</span><span class="sxs-lookup"><span data-stu-id="30f84-162"><a name="exceptions"></a> Capturing exceptions and related diagnostic data</span></span>
<span data-ttu-id="30f84-163">Na první pohled neuvidíte portálu hello všechny hello výjimky, které způsobí selhání ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="30f84-163">At first, you won't see in hello portal all hello exceptions that cause failures in your app.</span></span> <span data-ttu-id="30f84-164">Zobrazí všechny výjimky prohlížeče (Pokud používáte hello [JavaScript SDK](app-insights-javascript.md) na webových stránkách).</span><span class="sxs-lookup"><span data-stu-id="30f84-164">You'll see any browser exceptions (if you're using hello [JavaScript SDK](app-insights-javascript.md) in your web pages).</span></span> <span data-ttu-id="30f84-165">Ale většina serveru výjimky jsou zachyceny službou IIS a máte toowrite kousek toosee kód je.</span><span class="sxs-lookup"><span data-stu-id="30f84-165">But most server exceptions are caught by IIS and you have toowrite a bit of code toosee them.</span></span>

<span data-ttu-id="30f84-166">Můžete:</span><span class="sxs-lookup"><span data-stu-id="30f84-166">You can:</span></span>

* <span data-ttu-id="30f84-167">**Protokolování výjimek explicitně** vložením kódu ve výjimce obslužné rutiny tooreport hello výjimky.</span><span class="sxs-lookup"><span data-stu-id="30f84-167">**Log exceptions explicitly** by inserting code in exception handlers tooreport hello exceptions.</span></span>
* <span data-ttu-id="30f84-168">**Zachytit výjimky automaticky** nakonfigurováním požadované rozhraní ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="30f84-168">**Capture exceptions automatically** by configuring your ASP.NET framework.</span></span> <span data-ttu-id="30f84-169">potřebné dodatky Hello se liší pro různé typy framework.</span><span class="sxs-lookup"><span data-stu-id="30f84-169">hello necessary additions are different for different types of framework.</span></span>

## <a name="reporting-exceptions-explicitly"></a><span data-ttu-id="30f84-170">Explicitně Reporting výjimky</span><span class="sxs-lookup"><span data-stu-id="30f84-170">Reporting exceptions explicitly</span></span>
<span data-ttu-id="30f84-171">Hello nejjednodušší způsob je tooinsert tooTrackException() volání v obslužné rutiny výjimek.</span><span class="sxs-lookup"><span data-stu-id="30f84-171">hello simplest way is tooinsert a call tooTrackException() in an exception handler.</span></span>

<span data-ttu-id="30f84-172">JavaScript</span><span class="sxs-lookup"><span data-stu-id="30f84-172">JavaScript</span></span>

    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }

<span data-ttu-id="30f84-173">C#</span><span class="sxs-lookup"><span data-stu-id="30f84-173">C#</span></span>

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send hello exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

<span data-ttu-id="30f84-174">JAZYKA VISUAL BASIC</span><span class="sxs-lookup"><span data-stu-id="30f84-174">VB</span></span>

    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send hello exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

<span data-ttu-id="30f84-175">Hello vlastnosti a měření parametry jsou volitelné, ale jsou užitečné pro [filtrování a přidání](app-insights-diagnostic-search.md) doplňující informace.</span><span class="sxs-lookup"><span data-stu-id="30f84-175">hello properties and measurements parameters are optional, but are useful for [filtering and adding](app-insights-diagnostic-search.md) extra information.</span></span> <span data-ttu-id="30f84-176">Například pokud máte aplikaci, která můžete spustit několik hry, může najít všechny hello výjimka sestavy související tooa konkrétní hra.</span><span class="sxs-lookup"><span data-stu-id="30f84-176">For example, if you have an app that can run several games, you could find all hello exception reports related tooa particular game.</span></span> <span data-ttu-id="30f84-177">Můžete přidat libovolný počet položek jako jste jako slovník tooeach.</span><span class="sxs-lookup"><span data-stu-id="30f84-177">You can add as many items as you like tooeach dictionary.</span></span>

## <a name="browser-exceptions"></a><span data-ttu-id="30f84-178">Výjimky prohlížečů</span><span class="sxs-lookup"><span data-stu-id="30f84-178">Browser exceptions</span></span>
<span data-ttu-id="30f84-179">Většina prohlížeče výjimek jsou hlášeny.</span><span class="sxs-lookup"><span data-stu-id="30f84-179">Most browser exceptions are reported.</span></span>

<span data-ttu-id="30f84-180">Pokud vaše webová stránka obsahuje soubory skriptu ze sítě pro doručování obsahu nebo v jiných doménách, ujistěte se, vaše značky script má atribut hello ```crossorigin="anonymous"```, a odešle tento server hello [hlavičky CORS](http://enable-cors.org/).</span><span class="sxs-lookup"><span data-stu-id="30f84-180">If your web page includes script files from content delivery networks or other domains, ensure your script tag has hello attribute ```crossorigin="anonymous"```,  and that hello server sends [CORS headers](http://enable-cors.org/).</span></span> <span data-ttu-id="30f84-181">To vám umožní tooget trasování zásobníku a podrobností pro neošetřených výjimek jazyka JavaScript z těchto prostředků.</span><span class="sxs-lookup"><span data-stu-id="30f84-181">This will allow you tooget a stack trace and detail for unhandled JavaScript exceptions from these resources.</span></span>

## <a name="web-forms"></a><span data-ttu-id="30f84-182">Webové formuláře</span><span class="sxs-lookup"><span data-stu-id="30f84-182">Web forms</span></span>
<span data-ttu-id="30f84-183">Pro webové formuláře hello modulu HTTP bude možné toocollect hello výjimky Pokud nejsou žádné přesměrování nakonfigurované CustomErrors.</span><span class="sxs-lookup"><span data-stu-id="30f84-183">For web forms, hello HTTP Module will be able toocollect hello exceptions when there are no redirects configured with CustomErrors.</span></span>

<span data-ttu-id="30f84-184">Ale pokud máte aktivní přesměrování, přidejte následující řádky toohello Application_Error funkce v Global.asax.cs hello.</span><span class="sxs-lookup"><span data-stu-id="30f84-184">But if you have active redirects, add hello following lines toohello Application_Error function in Global.asax.cs.</span></span> <span data-ttu-id="30f84-185">(Pokud jste ještě nemáte, přidejte souboru Global.asax.)</span><span class="sxs-lookup"><span data-stu-id="30f84-185">(Add a Global.asax file if you don't already have one.)</span></span>

<span data-ttu-id="30f84-186">*C#*</span><span class="sxs-lookup"><span data-stu-id="30f84-186">*C#*</span></span>

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## <a name="mvc"></a><span data-ttu-id="30f84-187">MVC</span><span class="sxs-lookup"><span data-stu-id="30f84-187">MVC</span></span>
<span data-ttu-id="30f84-188">Pokud hello [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) konfigurace je `Off`, bude k dispozici pro hello výjimky [modulu HTTP](https://msdn.microsoft.com/library/ms178468.aspx) toocollect.</span><span class="sxs-lookup"><span data-stu-id="30f84-188">If hello [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) configuration is `Off`, then exceptions will be available for hello [HTTP Module](https://msdn.microsoft.com/library/ms178468.aspx) toocollect.</span></span> <span data-ttu-id="30f84-189">Ale pokud je `RemoteOnly` (výchozí), nebo `On`, výjimka hello se odstraní a není k dispozici pro službu Application Insights tooautomatically shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="30f84-189">However, if it is `RemoteOnly` (default), or `On`, then hello exception will be cleared and not available for Application Insights tooautomatically collect.</span></span> <span data-ttu-id="30f84-190">Můžete to opravíme přepsáním hello [System.Web.Mvc.HandleErrorAttribute třída](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx)a použití třídy hello přepsat, jak je uvedeno pro hello různé MVC verze níže ([githubu zdroj](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span><span class="sxs-lookup"><span data-stu-id="30f84-190">You can fix that by overriding hello [System.Web.Mvc.HandleErrorAttribute class](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), and applying hello overridden class as shown for hello different MVC versions below ([github source](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span></span>

    using System;
    using System.Web.Mvc;
    using Microsoft.ApplicationInsights;

    namespace MVC2App.Controllers
    {
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
      public class AiHandleErrorAttribute : HandleErrorAttribute
      {
        public override void OnException(ExceptionContext filterContext)
        {
            if (filterContext != null && filterContext.HttpContext != null && filterContext.Exception != null)
            {
                //If customError is Off, then AI HTTPModule will report hello exception
                if (filterContext.HttpContext.IsCustomErrorEnabled)
                {   //or reuse instance (recommended!). see note above  
                    var ai = new TelemetryClient();
                    ai.TrackException(filterContext.Exception);
                }
            }
            base.OnException(filterContext);
        }
      }
    }

#### <a name="mvc-2"></a><span data-ttu-id="30f84-191">MVC 2</span><span class="sxs-lookup"><span data-stu-id="30f84-191">MVC 2</span></span>
<span data-ttu-id="30f84-192">Atribut HandleError hello nahraďte váš nový atribut v řadičů.</span><span class="sxs-lookup"><span data-stu-id="30f84-192">Replace hello HandleError attribute with your new attribute in your controllers.</span></span>

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[<span data-ttu-id="30f84-193">Ukázka</span><span class="sxs-lookup"><span data-stu-id="30f84-193">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a><span data-ttu-id="30f84-194">MVC 3</span><span class="sxs-lookup"><span data-stu-id="30f84-194">MVC 3</span></span>
<span data-ttu-id="30f84-195">Zaregistrovat `AiHandleErrorAttribute` jako globální filtr ve funkci Global.asax.cs:</span><span class="sxs-lookup"><span data-stu-id="30f84-195">Register `AiHandleErrorAttribute` as a global filter in Global.asax.cs:</span></span>

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[<span data-ttu-id="30f84-196">Ukázka</span><span class="sxs-lookup"><span data-stu-id="30f84-196">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a><span data-ttu-id="30f84-197">MVC 4, MVC5</span><span class="sxs-lookup"><span data-stu-id="30f84-197">MVC 4, MVC5</span></span>
<span data-ttu-id="30f84-198">Zaregistrujte AiHandleErrorAttribute jako globální filtr ve funkci FilterConfig.cs:</span><span class="sxs-lookup"><span data-stu-id="30f84-198">Register AiHandleErrorAttribute as a global filter in FilterConfig.cs:</span></span>

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with hello override tootrack unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[<span data-ttu-id="30f84-199">Ukázka</span><span class="sxs-lookup"><span data-stu-id="30f84-199">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api-1x"></a><span data-ttu-id="30f84-200">Webové rozhraní API 1.x</span><span class="sxs-lookup"><span data-stu-id="30f84-200">Web API 1.x</span></span>
<span data-ttu-id="30f84-201">Přepište System.Web.Http.Filters.ExceptionFilterAttribute:</span><span class="sxs-lookup"><span data-stu-id="30f84-201">Override System.Web.Http.Filters.ExceptionFilterAttribute:</span></span>

    using System.Web.Http.Filters;
    using Microsoft.ApplicationInsights;

    namespace WebAPI.App_Start
    {
      public class AiExceptionFilterAttribute : ExceptionFilterAttribute
      {
        public override void OnException(HttpActionExecutedContext actionExecutedContext)
        {
            if (actionExecutedContext != null && actionExecutedContext.Exception != null)
            {  //or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(actionExecutedContext.Exception);    
            }
            base.OnException(actionExecutedContext);
        }
      }
    }

<span data-ttu-id="30f84-202">Můžete přidat tento atribut přepsaného toospecific řadiče, nebo jeho přidání konfigurace globálních filtrů toohello do třídy WebApiConfig hello:</span><span class="sxs-lookup"><span data-stu-id="30f84-202">You could add this overridden attribute toospecific controllers, or add it toohello global filter configuration in hello WebApiConfig class:</span></span>

    using System.Web.Http;
    using WebApi1.x.App_Start;

    namespace WebApi1.x
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(name: "DefaultApi", routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional });
            ...
            config.EnableSystemDiagnosticsTracing();

            // Capture exceptions for Application Insights:
            config.Filters.Add(new AiExceptionFilterAttribute());
        }
      }
    }

[<span data-ttu-id="30f84-203">Ukázka</span><span class="sxs-lookup"><span data-stu-id="30f84-203">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

<span data-ttu-id="30f84-204">Existuje několik případů, které filtry výjimek hello nelze zpracovat.</span><span class="sxs-lookup"><span data-stu-id="30f84-204">There are a number of cases that hello exception filters cannot handle.</span></span> <span data-ttu-id="30f84-205">Například:</span><span class="sxs-lookup"><span data-stu-id="30f84-205">For example:</span></span>

* <span data-ttu-id="30f84-206">Výjimky vydané z konstruktorů řadiče.</span><span class="sxs-lookup"><span data-stu-id="30f84-206">Exceptions thrown from controller constructors.</span></span>
* <span data-ttu-id="30f84-207">Výjimek vyvolaných z obslužné rutiny zpráv.</span><span class="sxs-lookup"><span data-stu-id="30f84-207">Exceptions thrown from message handlers.</span></span>
* <span data-ttu-id="30f84-208">Výjimky vydané během trasování.</span><span class="sxs-lookup"><span data-stu-id="30f84-208">Exceptions thrown during routing.</span></span>
* <span data-ttu-id="30f84-209">Výjimky vydané během serializace obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="30f84-209">Exceptions thrown during response content serialization.</span></span>

## <a name="web-api-2x"></a><span data-ttu-id="30f84-210">Webové rozhraní API 2.x</span><span class="sxs-lookup"><span data-stu-id="30f84-210">Web API 2.x</span></span>
<span data-ttu-id="30f84-211">Přidání implementace IExceptionLogger:</span><span class="sxs-lookup"><span data-stu-id="30f84-211">Add an implementation of IExceptionLogger:</span></span>

    using System.Web.Http.ExceptionHandling;
    using Microsoft.ApplicationInsights;

    namespace ProductsAppPureWebAPI.App_Start
    {
      public class AiExceptionLogger : ExceptionLogger
      {
        public override void Log(ExceptionLoggerContext context)
        {
            if (context !=null && context.Exception != null)
            {//or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(context.Exception);
            }
            base.Log(context);
        }
      }
    }

<span data-ttu-id="30f84-212">Přidejte tuto toohello služby WebApiConfig:</span><span class="sxs-lookup"><span data-stu-id="30f84-212">Add this toohello services in WebApiConfig:</span></span>

    using System.Web.Http;
    using System.Web.Http.ExceptionHandling;
    using ProductsAppPureWebAPI.App_Start;

    namespace WebApi2WithMVC
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
            config.Services.Add(typeof(IExceptionLogger), new AiExceptionLogger());
        }
      }
  <span data-ttu-id="30f84-213">}</span><span class="sxs-lookup"><span data-stu-id="30f84-213">}</span></span>

[<span data-ttu-id="30f84-214">Ukázka</span><span class="sxs-lookup"><span data-stu-id="30f84-214">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

<span data-ttu-id="30f84-215">Jako alternativy které může:</span><span class="sxs-lookup"><span data-stu-id="30f84-215">As alternatives, you could:</span></span>

1. <span data-ttu-id="30f84-216">Nahraďte text hello pouze ExceptionHandler s vlastní implementaci IExceptionHandler.</span><span class="sxs-lookup"><span data-stu-id="30f84-216">Replace hello only ExceptionHandler with a custom implementation of IExceptionHandler.</span></span> <span data-ttu-id="30f84-217">Volá se, jenom když hello framework je stále možné toochoose které odpovědi zprávy toosend (ne, pokud pro instanci přerušena hello připojení)</span><span class="sxs-lookup"><span data-stu-id="30f84-217">This is only called when hello framework is still able toochoose which response message toosend (not when hello connection is aborted for instance)</span></span>
2. <span data-ttu-id="30f84-218">Filtry výjimek (jak je popsáno v části hello na řadičích 1.x webového rozhraní API výše) – ve všech případech není volána.</span><span class="sxs-lookup"><span data-stu-id="30f84-218">Exception Filters (as described in hello section on Web API 1.x controllers above) - not called in all cases.</span></span>

## <a name="wcf"></a><span data-ttu-id="30f84-219">WCF</span><span class="sxs-lookup"><span data-stu-id="30f84-219">WCF</span></span>
<span data-ttu-id="30f84-220">Přidejte třídu, která rozšiřuje atribut a implementuje IErrorHandler a IServiceBehavior.</span><span class="sxs-lookup"><span data-stu-id="30f84-220">Add a class that extends Attribute and implements IErrorHandler and IServiceBehavior.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.ServiceModel.Description;
    using System.ServiceModel.Dispatcher;
    using System.Web;
    using Microsoft.ApplicationInsights;

    namespace WcfService4.ErrorHandling
    {
      public class AiLogExceptionAttribute : Attribute, IErrorHandler, IServiceBehavior
      {
        public void AddBindingParameters(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase,
            System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints,
            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)
        {
        }

        public void ApplyDispatchBehavior(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
            foreach (ChannelDispatcher disp in serviceHostBase.ChannelDispatchers)
            {
                disp.ErrorHandlers.Add(this);
            }
        }

        public void Validate(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
        }

        bool IErrorHandler.HandleError(Exception error)
        {//or reuse instance (recommended!). see note above
            var ai = new TelemetryClient();

            ai.TrackException(error);
            return false;
        }

        void IErrorHandler.ProvideFault(Exception error,
            System.ServiceModel.Channels.MessageVersion version,
            ref System.ServiceModel.Channels.Message fault)
        {
        }
      }
    }

<span data-ttu-id="30f84-221">Přidání implementace služby toohello atribut hello:</span><span class="sxs-lookup"><span data-stu-id="30f84-221">Add hello attribute toohello service implementations:</span></span>

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[<span data-ttu-id="30f84-222">Ukázka</span><span class="sxs-lookup"><span data-stu-id="30f84-222">Sample</span></span>](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a><span data-ttu-id="30f84-223">Čítače výkonu výjimky</span><span class="sxs-lookup"><span data-stu-id="30f84-223">Exception performance counters</span></span>
<span data-ttu-id="30f84-224">Pokud máte [nainstalován hello agenta Application Insights](app-insights-monitor-performance-live-website-now.md) na serveru, můžete získat graf výjimky míry hello měří v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="30f84-224">If you have [installed hello Application Insights Agent](app-insights-monitor-performance-live-website-now.md) on your server, you can get a chart of hello exceptions rate, measured by .NET.</span></span> <span data-ttu-id="30f84-225">To zahrnuje zpracovávaný i neošetřené výjimky .NET.</span><span class="sxs-lookup"><span data-stu-id="30f84-225">This includes both handled and unhandled .NET exceptions.</span></span>

<span data-ttu-id="30f84-226">Otevřete okno Průzkumníka metrika, přidejte nový graf a vyberte **výjimka míra**, uvedené v části čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="30f84-226">Open a Metric Explorer blade, add a new chart, and select **Exception rate**, listed under Performance Counters.</span></span>

<span data-ttu-id="30f84-227">rozhraní .NET framework Hello vypočítá hello rychlost počítání hello počet výjimek v intervalu a vydělí hello délka intervalu hello.</span><span class="sxs-lookup"><span data-stu-id="30f84-227">hello .NET framework calculates hello rate by counting hello number of exceptions in an interval and dividing by hello length of hello interval.</span></span>

<span data-ttu-id="30f84-228">Všimněte si, že bude liší od počtu hello výjimky vypočítána portálem Application Insights hello počítání TrackException sestavy.</span><span class="sxs-lookup"><span data-stu-id="30f84-228">Note that it will be different from hello 'Exceptions' count calculated by hello Application Insights portal by counting TrackException reports.</span></span> <span data-ttu-id="30f84-229">během intervalů vzorkování Hello se liší a hello SDK neodešle TrackException sestavy pro všechny zpracovávaný a neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="30f84-229">hello sampling intervals are different, and hello SDK doesn't send TrackException reports for all handled and unhandled exceptions.</span></span>

## <a name="video"></a><span data-ttu-id="30f84-230">Video</span><span class="sxs-lookup"><span data-stu-id="30f84-230">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a><span data-ttu-id="30f84-231">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30f84-231">Next steps</span></span>
* [<span data-ttu-id="30f84-232">Sledování REST, SQL a jiné toodependencies volání</span><span class="sxs-lookup"><span data-stu-id="30f84-232">Monitor REST, SQL and other calls toodependencies</span></span>](app-insights-asp-net-dependencies.md)
* [<span data-ttu-id="30f84-233">Monitorování časů načtení stránky, výjimek prohlížeče a volání AJAX</span><span class="sxs-lookup"><span data-stu-id="30f84-233">Monitor page load times, browser exceptions, and AJAX calls</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="30f84-234">Čítače sledování výkonu</span><span class="sxs-lookup"><span data-stu-id="30f84-234">Monitor performance counters</span></span>](app-insights-performance-counters.md)
