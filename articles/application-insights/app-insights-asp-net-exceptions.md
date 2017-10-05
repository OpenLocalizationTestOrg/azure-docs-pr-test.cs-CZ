---
title: "Diagnostikovat chyby a výjimky v webové aplikace pomocí služby Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 7eeacdc6677ccdebb1653e94a163ecb47090b7ee
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a><span data-ttu-id="51bed-103">Diagnostikovat výjimky ve webových aplikacích pomocí služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="51bed-103">Diagnose exceptions in your web apps with Application Insights</span></span>
<span data-ttu-id="51bed-104">Výjimky v svou živou webovou aplikaci oznamuje [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="51bed-104">Exceptions in your live web app are reported by [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="51bed-105">Neúspěšných požadavků mohou korelovat s výjimky a dalších událostí na klienta a serveru, takže můžete rychle diagnostikovat příčin.</span><span class="sxs-lookup"><span data-stu-id="51bed-105">You can correlate failed requests with exceptions and other events at both the client and server, so that you can quickly diagnose the causes.</span></span>

## <a name="set-up-exception-reporting"></a><span data-ttu-id="51bed-106">Nastavit generování sestav výjimky</span><span class="sxs-lookup"><span data-stu-id="51bed-106">Set up exception reporting</span></span>
* <span data-ttu-id="51bed-107">Mít výjimky nahlášené z vaší aplikace. server:</span><span class="sxs-lookup"><span data-stu-id="51bed-107">To have exceptions reported from your server app:</span></span>
  * <span data-ttu-id="51bed-108">Nainstalujte [Application Insights SDK](app-insights-asp-net.md) v kódu aplikace, nebo</span><span class="sxs-lookup"><span data-stu-id="51bed-108">Install [Application Insights SDK](app-insights-asp-net.md) in your app code, or</span></span>
  * <span data-ttu-id="51bed-109">Webové servery služby IIS: Spusťte [agenta Application Insights](app-insights-monitor-performance-live-website-now.md); nebo</span><span class="sxs-lookup"><span data-stu-id="51bed-109">IIS web servers: Run [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); or</span></span>
  * <span data-ttu-id="51bed-110">Službě Azure web apps: Přidat [rozšíření Application Insights](app-insights-azure-web-apps.md)</span><span class="sxs-lookup"><span data-stu-id="51bed-110">Azure web apps: Add the [Application Insights Extension](app-insights-azure-web-apps.md)</span></span>
  * <span data-ttu-id="51bed-111">Webové aplikace v jazyce Java: Nainstalujte [agenta Java](app-insights-java-agent.md)</span><span class="sxs-lookup"><span data-stu-id="51bed-111">Java web apps: Install the [Java agent](app-insights-java-agent.md)</span></span>
* <span data-ttu-id="51bed-112">Nainstalujte [fragment kódu JavaScript](app-insights-javascript.md) na webových stránkách zachycení výjimky v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="51bed-112">Install the [JavaScript snippet](app-insights-javascript.md) in your web pages to catch browser exceptions.</span></span>
* <span data-ttu-id="51bed-113">Některé architektury aplikace nebo s některými nastaveními budete muset provést některé další kroky pro zachycení více výjimek:</span><span class="sxs-lookup"><span data-stu-id="51bed-113">In some application frameworks or with some settings, you need to take some extra steps to catch more exceptions:</span></span>
  * [<span data-ttu-id="51bed-114">Webové formuláře</span><span class="sxs-lookup"><span data-stu-id="51bed-114">Web forms</span></span>](#web-forms)
  * [<span data-ttu-id="51bed-115">MVC</span><span class="sxs-lookup"><span data-stu-id="51bed-115">MVC</span></span>](#mvc)
  * [<span data-ttu-id="51bed-116">1.* webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="51bed-116">Web API 1.*</span></span>](#web-api-1)
  * [<span data-ttu-id="51bed-117">2.* webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="51bed-117">Web API 2.*</span></span>](#web-api-2)
  * [<span data-ttu-id="51bed-118">WCF</span><span class="sxs-lookup"><span data-stu-id="51bed-118">WCF</span></span>](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a><span data-ttu-id="51bed-119">Diagnostikování výjimky pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51bed-119">Diagnosing exceptions using Visual Studio</span></span>
<span data-ttu-id="51bed-120">Otevřete aplikaci řešení v sadě Visual Studio, které pomáhají při ladění.</span><span class="sxs-lookup"><span data-stu-id="51bed-120">Open the app solution in Visual Studio to help with debugging.</span></span>

<span data-ttu-id="51bed-121">Spusťte aplikaci na vašem serveru nebo na počítači pro vývoj pomocí F5.</span><span class="sxs-lookup"><span data-stu-id="51bed-121">Run the app, either on your server or on your development machine by using F5.</span></span>

<span data-ttu-id="51bed-122">Otevřete okno hledání Application Insights v sadě Visual Studio a nastavte ji chcete zobrazit události z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="51bed-122">Open the Application Insights Search window in Visual Studio, and set it to display events from your app.</span></span> <span data-ttu-id="51bed-123">Při ladění, můžete k tomu jednoduše kliknutím na tlačítko Application Insights.</span><span class="sxs-lookup"><span data-stu-id="51bed-123">While you're debugging, you can do this just by clicking the Application Insights button.</span></span>

![Klikněte pravým tlačítkem na projekt a vyberte Application Insights, otevřete.](./media/app-insights-asp-net-exceptions/34.png)

<span data-ttu-id="51bed-125">Všimněte si, že můžete filtrovat sestavě zobrazily jenom výjimky.</span><span class="sxs-lookup"><span data-stu-id="51bed-125">Notice that you can filter the report to show just exceptions.</span></span>

<span data-ttu-id="51bed-126">*Žádné výjimky zobrazující? V tématu [zachycení výjimky](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="51bed-126">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>

<span data-ttu-id="51bed-127">Klikněte na sestavu výjimky a zobrazit jeho trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="51bed-127">Click an exception report to show its stack trace.</span></span>
<span data-ttu-id="51bed-128">Klikněte na odkaz řádku v trasování zásobníku, otevřete soubor odpovídající kód.</span><span class="sxs-lookup"><span data-stu-id="51bed-128">Click a line reference in the stack trace, to open the relevant code file.</span></span>  

<span data-ttu-id="51bed-129">V kódu Všimněte si, že Codelensu zobrazuje data o výjimky:</span><span class="sxs-lookup"><span data-stu-id="51bed-129">In the code, notice that CodeLens shows data about the exceptions:</span></span>

![Oznámení Codelensu výjimek.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-the-azure-portal"></a><span data-ttu-id="51bed-131">Diagnostikování chyb pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="51bed-131">Diagnosing failures using the Azure portal</span></span>
<span data-ttu-id="51bed-132">Z přehledu nástroje Application Insights vaší aplikace dlaždici selhání ukazuje grafy výjimek a neúspěšné požadavky HTTP, společně s seznam požadavek adresy URL, které způsobí selhání nejčastěji se vyskytující.</span><span class="sxs-lookup"><span data-stu-id="51bed-132">From the Application Insights overview of your app, the Failures tile shows you charts of exceptions and failed HTTP requests, together with a list of the request URLs that cause the most frequent failures.</span></span>

![Vyberte nastavení, selhání](./media/app-insights-asp-net-exceptions/012-start.png)

<span data-ttu-id="51bed-134">Klikněte na prostřednictvím jednoho z typů selhání výjimka v seznamu získat u jednotlivých výskytů výjimky, kde můžete zobrazit podrobnosti a trasováním zásobníku:</span><span class="sxs-lookup"><span data-stu-id="51bed-134">Click through one of the failed exception types in the list to get to individual occurrences of the exception, where you can see the details and stack trace:</span></span>

![Vyberte instanci chybné žádosti a v části Podrobnosti o výjimce, získat na instance výjimky.](./media/app-insights-asp-net-exceptions/030-req-drill.png)

<span data-ttu-id="51bed-136">**Alternativně** můžete spustit ze seznamu požadavků a najít výjimky s ním souvisejí.</span><span class="sxs-lookup"><span data-stu-id="51bed-136">**Alternatively,** you can start from the list of requests and find exceptions related to it.</span></span>

<span data-ttu-id="51bed-137">*Žádné výjimky zobrazující? V tématu [zachycení výjimky](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="51bed-137">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>


## <a name="custom-tracing-and-log-data"></a><span data-ttu-id="51bed-138">Vlastní trasování a data protokolu</span><span class="sxs-lookup"><span data-stu-id="51bed-138">Custom tracing and log data</span></span>
<span data-ttu-id="51bed-139">K získání diagnostických dat specifické pro vaši aplikaci, můžete vložit kódu pro odeslání vlastní telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="51bed-139">To get diagnostic data specific to your app, you can insert code to send your own telemetry data.</span></span> <span data-ttu-id="51bed-140">Toto zobrazí ve vyhledávání diagnostiky společně se žádosti, zobrazení stránky a další automaticky shromažďovat data.</span><span class="sxs-lookup"><span data-stu-id="51bed-140">This displayed in diagnostic search alongside the request, page view and other automatically-collected data.</span></span>

<span data-ttu-id="51bed-141">Máte několik možností:</span><span class="sxs-lookup"><span data-stu-id="51bed-141">You have several options:</span></span>

* <span data-ttu-id="51bed-142">[TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) se obvykle používá ke sledování vzorů využití, ale také odesílá data se zobrazí v části vlastní události ve vyhledávání diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="51bed-142">[TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) is typically used for monitoring usage patterns, but the data it sends also appears under Custom Events in diagnostic search.</span></span> <span data-ttu-id="51bed-143">Události jsou pojmenované a přenášet vlastnosti řetězce a číselné metriky, ve kterém můžete [filtrovat diagnostických hledání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="51bed-143">Events are named, and can carry string properties and numeric metrics on which you can [filter your diagnostic searches](app-insights-diagnostic-search.md).</span></span>
* <span data-ttu-id="51bed-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) umožňuje odesílat delší data, jako jsou informace o POST.</span><span class="sxs-lookup"><span data-stu-id="51bed-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) lets you send longer data such as POST information.</span></span>
* <span data-ttu-id="51bed-145">[TrackException()](#exceptions) odešle trasování zásobníku.</span><span class="sxs-lookup"><span data-stu-id="51bed-145">[TrackException()](#exceptions) sends stack traces.</span></span> <span data-ttu-id="51bed-146">[Další informace o výjimky](#exceptions).</span><span class="sxs-lookup"><span data-stu-id="51bed-146">[More about exceptions](#exceptions).</span></span>
* <span data-ttu-id="51bed-147">Pokud už používáte rozhraní protokolování, jako je Log4Net nebo NLog, můžete [zaznamenat tyto protokoly](app-insights-asp-net-trace-logs.md) a zobrazovat ve vyhledávání diagnostiky společně se data požadavku a výjimky.</span><span class="sxs-lookup"><span data-stu-id="51bed-147">If you already use a logging framework like Log4Net or NLog, you can [capture those logs](app-insights-asp-net-trace-logs.md) and see them in diagnostic search alongside request and exception data.</span></span>

<span data-ttu-id="51bed-148">Pokud chcete zobrazit tyto události, otevřete [vyhledávání](app-insights-diagnostic-search.md), otevřete filtr a potom vyberte vlastní události, trasování nebo výjimky.</span><span class="sxs-lookup"><span data-stu-id="51bed-148">To see these events, open [Search](app-insights-diagnostic-search.md), open Filter, and then choose Custom Event, Trace, or Exception.</span></span>

![Procházení](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> <span data-ttu-id="51bed-150">Pokud vaše aplikace generuje mnoho telemetrických dat, sníží modul adaptivního vzorkování automaticky objem dat odesílaných na portál tím, že budou odesílány pouze reprezentativní vzorky událostí.</span><span class="sxs-lookup"><span data-stu-id="51bed-150">If your app generates a lot of telemetry, the adaptive sampling module will automatically reduce the volume that is sent to the portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="51bed-151">Události, které jsou součástí stejné operace bude vybraná nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi.</span><span class="sxs-lookup"><span data-stu-id="51bed-151">Events that are part of the same operation will be selected or deselected as a group, so that you can navigate between related events.</span></span> [<span data-ttu-id="51bed-152">Další informace o vzorkování.</span><span class="sxs-lookup"><span data-stu-id="51bed-152">Learn about sampling.</span></span>](app-insights-sampling.md)
>
>

### <a name="how-to-see-request-post-data"></a><span data-ttu-id="51bed-153">Jak zjistit, data požadavku POST</span><span class="sxs-lookup"><span data-stu-id="51bed-153">How to see request POST data</span></span>
<span data-ttu-id="51bed-154">Podrobnosti požadavku neobsahují data odeslaná do vaší aplikace v volání POST.</span><span class="sxs-lookup"><span data-stu-id="51bed-154">Request details don't include the data sent to your app in a POST call.</span></span> <span data-ttu-id="51bed-155">Aby tato data nahlášené:</span><span class="sxs-lookup"><span data-stu-id="51bed-155">To have this data reported:</span></span>

* <span data-ttu-id="51bed-156">[Instalace sady SDK](app-insights-asp-net.md) ve vašem projektu aplikace.</span><span class="sxs-lookup"><span data-stu-id="51bed-156">[Install the SDK](app-insights-asp-net.md) in your application project.</span></span>
* <span data-ttu-id="51bed-157">Vložení kódu v aplikaci k volání [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span><span class="sxs-lookup"><span data-stu-id="51bed-157">Insert code in your application to call [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span></span> <span data-ttu-id="51bed-158">Odesílání dat POST v parametru zprávy.</span><span class="sxs-lookup"><span data-stu-id="51bed-158">Send the POST data in the message parameter.</span></span> <span data-ttu-id="51bed-159">Neexistuje omezení povolenou velikost, takže pokuste odeslat jenom základní data.</span><span class="sxs-lookup"><span data-stu-id="51bed-159">There is a limit to the permitted size, so you should try to send just the essential data.</span></span>
* <span data-ttu-id="51bed-160">Pokud byste prozkoumat chybných požadavků, najdete přidružené trasování.</span><span class="sxs-lookup"><span data-stu-id="51bed-160">When you investigate a failed request, find the associated traces.</span></span>  

![Procházení](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <span data-ttu-id="51bed-162"><a name="exceptions"></a>Zachytávání výjimek a související diagnostických dat</span><span class="sxs-lookup"><span data-stu-id="51bed-162"><a name="exceptions"></a> Capturing exceptions and related diagnostic data</span></span>
<span data-ttu-id="51bed-163">Na první pohled neuvidíte na portálu všechny výjimky, které způsobí selhání ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="51bed-163">At first, you won't see in the portal all the exceptions that cause failures in your app.</span></span> <span data-ttu-id="51bed-164">Zobrazí všechny výjimky prohlížeče (Pokud používáte [JavaScript SDK](app-insights-javascript.md) na webových stránkách).</span><span class="sxs-lookup"><span data-stu-id="51bed-164">You'll see any browser exceptions (if you're using the [JavaScript SDK](app-insights-javascript.md) in your web pages).</span></span> <span data-ttu-id="51bed-165">Ale většina serveru výjimky jsou zachyceny službou IIS a máte k zápisu verze kódu k jejich zobrazení.</span><span class="sxs-lookup"><span data-stu-id="51bed-165">But most server exceptions are caught by IIS and you have to write a bit of code to see them.</span></span>

<span data-ttu-id="51bed-166">Můžete:</span><span class="sxs-lookup"><span data-stu-id="51bed-166">You can:</span></span>

* <span data-ttu-id="51bed-167">**Protokolování výjimek explicitně** vložením kódu do obslužné rutiny výjimek nahlásit výjimky.</span><span class="sxs-lookup"><span data-stu-id="51bed-167">**Log exceptions explicitly** by inserting code in exception handlers to report the exceptions.</span></span>
* <span data-ttu-id="51bed-168">**Zachytit výjimky automaticky** nakonfigurováním požadované rozhraní ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51bed-168">**Capture exceptions automatically** by configuring your ASP.NET framework.</span></span> <span data-ttu-id="51bed-169">Potřebné dodatky se liší pro různé typy framework.</span><span class="sxs-lookup"><span data-stu-id="51bed-169">The necessary additions are different for different types of framework.</span></span>

## <a name="reporting-exceptions-explicitly"></a><span data-ttu-id="51bed-170">Explicitně Reporting výjimky</span><span class="sxs-lookup"><span data-stu-id="51bed-170">Reporting exceptions explicitly</span></span>
<span data-ttu-id="51bed-171">Nejjednodušší způsob, jak se má vložit volání pro TrackException() do obslužné rutiny výjimek.</span><span class="sxs-lookup"><span data-stu-id="51bed-171">The simplest way is to insert a call to TrackException() in an exception handler.</span></span>

<span data-ttu-id="51bed-172">JavaScript</span><span class="sxs-lookup"><span data-stu-id="51bed-172">JavaScript</span></span>

    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }

<span data-ttu-id="51bed-173">C#</span><span class="sxs-lookup"><span data-stu-id="51bed-173">C#</span></span>

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

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

<span data-ttu-id="51bed-174">JAZYKA VISUAL BASIC</span><span class="sxs-lookup"><span data-stu-id="51bed-174">VB</span></span>

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

      ' Send the exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

<span data-ttu-id="51bed-175">Vlastnosti a měření parametry jsou volitelné, ale jsou užitečné pro [filtrování a přidání](app-insights-diagnostic-search.md) doplňující informace.</span><span class="sxs-lookup"><span data-stu-id="51bed-175">The properties and measurements parameters are optional, but are useful for [filtering and adding](app-insights-diagnostic-search.md) extra information.</span></span> <span data-ttu-id="51bed-176">Například pokud máte aplikaci, která můžete spustit několik hry, může najít všechny sestavy výjimky související s konkrétní hru.</span><span class="sxs-lookup"><span data-stu-id="51bed-176">For example, if you have an app that can run several games, you could find all the exception reports related to a particular game.</span></span> <span data-ttu-id="51bed-177">Můžete přidat libovolný počet položek, jako má každý slovníku.</span><span class="sxs-lookup"><span data-stu-id="51bed-177">You can add as many items as you like to each dictionary.</span></span>

## <a name="browser-exceptions"></a><span data-ttu-id="51bed-178">Výjimky prohlížečů</span><span class="sxs-lookup"><span data-stu-id="51bed-178">Browser exceptions</span></span>
<span data-ttu-id="51bed-179">Většina prohlížeče výjimek jsou hlášeny.</span><span class="sxs-lookup"><span data-stu-id="51bed-179">Most browser exceptions are reported.</span></span>

<span data-ttu-id="51bed-180">Pokud vaše webová stránka obsahuje soubory skriptu ze sítě pro doručování obsahu nebo v jiných doménách, ujistěte se, vaše značky script má atribut ```crossorigin="anonymous"```, a že server odešle [hlavičky CORS](http://enable-cors.org/).</span><span class="sxs-lookup"><span data-stu-id="51bed-180">If your web page includes script files from content delivery networks or other domains, ensure your script tag has the attribute ```crossorigin="anonymous"```,  and that the server sends [CORS headers](http://enable-cors.org/).</span></span> <span data-ttu-id="51bed-181">To vám umožní získat trasování zásobníku a podrobnosti pro neošetřených výjimek jazyka JavaScript z těchto prostředků.</span><span class="sxs-lookup"><span data-stu-id="51bed-181">This will allow you to get a stack trace and detail for unhandled JavaScript exceptions from these resources.</span></span>

## <a name="web-forms"></a><span data-ttu-id="51bed-182">Webové formuláře</span><span class="sxs-lookup"><span data-stu-id="51bed-182">Web forms</span></span>
<span data-ttu-id="51bed-183">U webových formulářů bude možné ke shromažďování výjimky, když nejsou žádné přesměrování nakonfigurované CustomErrors modulu protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="51bed-183">For web forms, the HTTP Module will be able to collect the exceptions when there are no redirects configured with CustomErrors.</span></span>

<span data-ttu-id="51bed-184">Ale pokud máte aktivní přesměrování, přidejte následující řádky do funkce Application_Error Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="51bed-184">But if you have active redirects, add the following lines to the Application_Error function in Global.asax.cs.</span></span> <span data-ttu-id="51bed-185">(Pokud jste ještě nemáte, přidejte souboru Global.asax.)</span><span class="sxs-lookup"><span data-stu-id="51bed-185">(Add a Global.asax file if you don't already have one.)</span></span>

<span data-ttu-id="51bed-186">*C#*</span><span class="sxs-lookup"><span data-stu-id="51bed-186">*C#*</span></span>

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## <a name="mvc"></a><span data-ttu-id="51bed-187">MVC</span><span class="sxs-lookup"><span data-stu-id="51bed-187">MVC</span></span>
<span data-ttu-id="51bed-188">Pokud [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) konfigurace je `Off`, bude k dispozici pro výjimky [modulu HTTP](https://msdn.microsoft.com/library/ms178468.aspx) ke shromažďování.</span><span class="sxs-lookup"><span data-stu-id="51bed-188">If the [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) configuration is `Off`, then exceptions will be available for the [HTTP Module](https://msdn.microsoft.com/library/ms178468.aspx) to collect.</span></span> <span data-ttu-id="51bed-189">Ale pokud je `RemoteOnly` (výchozí), nebo `On`, bude výjimka nezaškrtnuté a nejsou k dispozici pro službu Application Insights automaticky shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="51bed-189">However, if it is `RemoteOnly` (default), or `On`, then the exception will be cleared and not available for Application Insights to automatically collect.</span></span> <span data-ttu-id="51bed-190">Můžete to opravíme přepsáním [System.Web.Mvc.HandleErrorAttribute – třída](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx)a použití přepsaného třídu, jak je uvedeno pro různé verze MVC níže ([githubu zdroj](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span><span class="sxs-lookup"><span data-stu-id="51bed-190">You can fix that by overriding the [System.Web.Mvc.HandleErrorAttribute class](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), and applying the overridden class as shown for the different MVC versions below ([github source](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span></span>

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
                //If customError is Off, then AI HTTPModule will report the exception
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

#### <a name="mvc-2"></a><span data-ttu-id="51bed-191">MVC 2</span><span class="sxs-lookup"><span data-stu-id="51bed-191">MVC 2</span></span>
<span data-ttu-id="51bed-192">Atribut HandleError nahraďte váš nový atribut v řadičů.</span><span class="sxs-lookup"><span data-stu-id="51bed-192">Replace the HandleError attribute with your new attribute in your controllers.</span></span>

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[<span data-ttu-id="51bed-193">Ukázka</span><span class="sxs-lookup"><span data-stu-id="51bed-193">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a><span data-ttu-id="51bed-194">MVC 3</span><span class="sxs-lookup"><span data-stu-id="51bed-194">MVC 3</span></span>
<span data-ttu-id="51bed-195">Zaregistrovat `AiHandleErrorAttribute` jako globální filtr ve funkci Global.asax.cs:</span><span class="sxs-lookup"><span data-stu-id="51bed-195">Register `AiHandleErrorAttribute` as a global filter in Global.asax.cs:</span></span>

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[<span data-ttu-id="51bed-196">Ukázka</span><span class="sxs-lookup"><span data-stu-id="51bed-196">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a><span data-ttu-id="51bed-197">MVC 4, MVC5</span><span class="sxs-lookup"><span data-stu-id="51bed-197">MVC 4, MVC5</span></span>
<span data-ttu-id="51bed-198">Zaregistrujte AiHandleErrorAttribute jako globální filtr ve funkci FilterConfig.cs:</span><span class="sxs-lookup"><span data-stu-id="51bed-198">Register AiHandleErrorAttribute as a global filter in FilterConfig.cs:</span></span>

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with the override to track unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[<span data-ttu-id="51bed-199">Ukázka</span><span class="sxs-lookup"><span data-stu-id="51bed-199">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api-1x"></a><span data-ttu-id="51bed-200">Webové rozhraní API 1.x</span><span class="sxs-lookup"><span data-stu-id="51bed-200">Web API 1.x</span></span>
<span data-ttu-id="51bed-201">Přepište System.Web.Http.Filters.ExceptionFilterAttribute:</span><span class="sxs-lookup"><span data-stu-id="51bed-201">Override System.Web.Http.Filters.ExceptionFilterAttribute:</span></span>

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

<span data-ttu-id="51bed-202">Můžete přidat tento atribut přepsaného na konkrétní řadiče, nebo ho přidat do konfigurace globálních filtrů v třídy WebApiConfig:</span><span class="sxs-lookup"><span data-stu-id="51bed-202">You could add this overridden attribute to specific controllers, or add it to the global filter configuration in the WebApiConfig class:</span></span>

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

[<span data-ttu-id="51bed-203">Ukázka</span><span class="sxs-lookup"><span data-stu-id="51bed-203">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

<span data-ttu-id="51bed-204">Existuje několik případů, které nelze zpracovat filtry výjimek.</span><span class="sxs-lookup"><span data-stu-id="51bed-204">There are a number of cases that the exception filters cannot handle.</span></span> <span data-ttu-id="51bed-205">Například:</span><span class="sxs-lookup"><span data-stu-id="51bed-205">For example:</span></span>

* <span data-ttu-id="51bed-206">Výjimky vydané z konstruktorů řadiče.</span><span class="sxs-lookup"><span data-stu-id="51bed-206">Exceptions thrown from controller constructors.</span></span>
* <span data-ttu-id="51bed-207">Výjimek vyvolaných z obslužné rutiny zpráv.</span><span class="sxs-lookup"><span data-stu-id="51bed-207">Exceptions thrown from message handlers.</span></span>
* <span data-ttu-id="51bed-208">Výjimky vydané během trasování.</span><span class="sxs-lookup"><span data-stu-id="51bed-208">Exceptions thrown during routing.</span></span>
* <span data-ttu-id="51bed-209">Výjimky vydané během serializace obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="51bed-209">Exceptions thrown during response content serialization.</span></span>

## <a name="web-api-2x"></a><span data-ttu-id="51bed-210">Webové rozhraní API 2.x</span><span class="sxs-lookup"><span data-stu-id="51bed-210">Web API 2.x</span></span>
<span data-ttu-id="51bed-211">Přidání implementace IExceptionLogger:</span><span class="sxs-lookup"><span data-stu-id="51bed-211">Add an implementation of IExceptionLogger:</span></span>

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

<span data-ttu-id="51bed-212">Přidejte tuto pro služby WebApiConfig:</span><span class="sxs-lookup"><span data-stu-id="51bed-212">Add this to the services in WebApiConfig:</span></span>

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
  <span data-ttu-id="51bed-213">}</span><span class="sxs-lookup"><span data-stu-id="51bed-213">}</span></span>

[<span data-ttu-id="51bed-214">Ukázka</span><span class="sxs-lookup"><span data-stu-id="51bed-214">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

<span data-ttu-id="51bed-215">Jako alternativy které může:</span><span class="sxs-lookup"><span data-stu-id="51bed-215">As alternatives, you could:</span></span>

1. <span data-ttu-id="51bed-216">Nahraďte pouze ExceptionHandler vlastní implementaci IExceptionHandler.</span><span class="sxs-lookup"><span data-stu-id="51bed-216">Replace the only ExceptionHandler with a custom implementation of IExceptionHandler.</span></span> <span data-ttu-id="51bed-217">Volá se, jenom když rozhraní je možné zvolit které zprávu odpovědi na odeslání (při připojení není přerušena pro instanci)</span><span class="sxs-lookup"><span data-stu-id="51bed-217">This is only called when the framework is still able to choose which response message to send (not when the connection is aborted for instance)</span></span>
2. <span data-ttu-id="51bed-218">Filtry výjimek (jak je popsáno v části na řadičích 1.x webového rozhraní API výše) – ve všech případech není volána.</span><span class="sxs-lookup"><span data-stu-id="51bed-218">Exception Filters (as described in the section on Web API 1.x controllers above) - not called in all cases.</span></span>

## <a name="wcf"></a><span data-ttu-id="51bed-219">WCF</span><span class="sxs-lookup"><span data-stu-id="51bed-219">WCF</span></span>
<span data-ttu-id="51bed-220">Přidejte třídu, která rozšiřuje atribut a implementuje IErrorHandler a IServiceBehavior.</span><span class="sxs-lookup"><span data-stu-id="51bed-220">Add a class that extends Attribute and implements IErrorHandler and IServiceBehavior.</span></span>

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

<span data-ttu-id="51bed-221">Přidejte atribut do implementace služby:</span><span class="sxs-lookup"><span data-stu-id="51bed-221">Add the attribute to the service implementations:</span></span>

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[<span data-ttu-id="51bed-222">Ukázka</span><span class="sxs-lookup"><span data-stu-id="51bed-222">Sample</span></span>](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a><span data-ttu-id="51bed-223">Čítače výkonu výjimky</span><span class="sxs-lookup"><span data-stu-id="51bed-223">Exception performance counters</span></span>
<span data-ttu-id="51bed-224">Pokud máte [nainstalovat agenta Application Insights](app-insights-monitor-performance-live-website-now.md) na serveru, můžete získat graf míry výjimky měří v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="51bed-224">If you have [installed the Application Insights Agent](app-insights-monitor-performance-live-website-now.md) on your server, you can get a chart of the exceptions rate, measured by .NET.</span></span> <span data-ttu-id="51bed-225">To zahrnuje zpracovávaný i neošetřené výjimky .NET.</span><span class="sxs-lookup"><span data-stu-id="51bed-225">This includes both handled and unhandled .NET exceptions.</span></span>

<span data-ttu-id="51bed-226">Otevřete okno Průzkumníka metrika, přidejte nový graf a vyberte **výjimka míra**, uvedené v části čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="51bed-226">Open a Metric Explorer blade, add a new chart, and select **Exception rate**, listed under Performance Counters.</span></span>

<span data-ttu-id="51bed-227">Rozhraní .NET framework vypočítá rychlost počítání počet výjimek v intervalu a vydělí délku intervalu.</span><span class="sxs-lookup"><span data-stu-id="51bed-227">The .NET framework calculates the rate by counting the number of exceptions in an interval and dividing by the length of the interval.</span></span>

<span data-ttu-id="51bed-228">Všimněte si, že bude liší od počtu "Výjimky" vypočítána pomocí portálu služby Application Insights počítání TrackException sestavy.</span><span class="sxs-lookup"><span data-stu-id="51bed-228">Note that it will be different from the 'Exceptions' count calculated by the Application Insights portal by counting TrackException reports.</span></span> <span data-ttu-id="51bed-229">Během intervalů vzorkování se liší a sady SDK neodešle TrackException sestavy pro všechny zpracovat a neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="51bed-229">The sampling intervals are different, and the SDK doesn't send TrackException reports for all handled and unhandled exceptions.</span></span>

## <a name="video"></a><span data-ttu-id="51bed-230">Video</span><span class="sxs-lookup"><span data-stu-id="51bed-230">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a><span data-ttu-id="51bed-231">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51bed-231">Next steps</span></span>
* [<span data-ttu-id="51bed-232">Sledování REST, SQL a jiné volání závislosti</span><span class="sxs-lookup"><span data-stu-id="51bed-232">Monitor REST, SQL and other calls to dependencies</span></span>](app-insights-asp-net-dependencies.md)
* [<span data-ttu-id="51bed-233">Monitorování časů načtení stránky, výjimek prohlížeče a volání AJAX</span><span class="sxs-lookup"><span data-stu-id="51bed-233">Monitor page load times, browser exceptions, and AJAX calls</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="51bed-234">Čítače sledování výkonu</span><span class="sxs-lookup"><span data-stu-id="51bed-234">Monitor performance counters</span></span>](app-insights-performance-counters.md)
