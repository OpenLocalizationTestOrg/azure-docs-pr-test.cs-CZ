---
title: "aaaHow na to... ve službě Azure Application Insights | Microsoft Docs"
description: "Nejčastější dotazy týkající se ve službě Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: bwren
ms.openlocfilehash: 89294c3583b7c4e7998143be6d359f2deb3c8f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-i--in-application-insights"></a><span data-ttu-id="bcec4-103">Jak mám udělat ... pomocí Application Insights?</span><span class="sxs-lookup"><span data-stu-id="bcec4-103">How do I ... in Application Insights?</span></span>
## <a name="get-an-email-when-"></a><span data-ttu-id="bcec4-104">Získat e-mailem při...</span><span class="sxs-lookup"><span data-stu-id="bcec4-104">Get an email when ...</span></span>
### <a name="email-if-my-site-goes-down"></a><span data-ttu-id="bcec4-105">Pokud web přestane fungovat e-mailu</span><span class="sxs-lookup"><span data-stu-id="bcec4-105">Email if my site goes down</span></span>
<span data-ttu-id="bcec4-106">Nastavit [dostupnosti webového testu](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="bcec4-106">Set an [availability web test](app-insights-monitor-web-app-availability.md).</span></span>

### <a name="email-if-my-site-is-overloaded"></a><span data-ttu-id="bcec4-107">E-mailu, pokud je přetížena mé lokality</span><span class="sxs-lookup"><span data-stu-id="bcec4-107">Email if my site is overloaded</span></span>
<span data-ttu-id="bcec4-108">Nastavit [výstraha](app-insights-alerts.md) na **doba odezvy serveru**.</span><span class="sxs-lookup"><span data-stu-id="bcec4-108">Set an [alert](app-insights-alerts.md) on **Server response time**.</span></span> <span data-ttu-id="bcec4-109">Prahová hodnota mezi 1 a 2 sekundami by měly fungovat.</span><span class="sxs-lookup"><span data-stu-id="bcec4-109">A threshold between 1 and 2 seconds should work.</span></span>

![](./media/app-insights-how-do-i/030-server.png)

<span data-ttu-id="bcec4-110">Aplikace může také zobrazit známky kmen vrácením selhání kódy.</span><span class="sxs-lookup"><span data-stu-id="bcec4-110">Your app might also show signs of strain by returning failure codes.</span></span> <span data-ttu-id="bcec4-111">Nastavit výstrahy na **neúspěšné požadavky**.</span><span class="sxs-lookup"><span data-stu-id="bcec4-111">Set an alert on **Failed requests**.</span></span>

<span data-ttu-id="bcec4-112">Pokud chcete tooset výstrahu na **výjimky serveru**, můžete mít toodo [některé další nastavení](app-insights-asp-net-exceptions.md) v datech toosee pořadí.</span><span class="sxs-lookup"><span data-stu-id="bcec4-112">If you want tooset an alert on **Server exceptions**, you might have toodo [some additional setup](app-insights-asp-net-exceptions.md) in order toosee data.</span></span>

### <a name="email-on-exceptions"></a><span data-ttu-id="bcec4-113">E-mailu na výjimky</span><span class="sxs-lookup"><span data-stu-id="bcec4-113">Email on exceptions</span></span>
1. [<span data-ttu-id="bcec4-114">Nastavili monitorování výjimek</span><span class="sxs-lookup"><span data-stu-id="bcec4-114">Set up exception monitoring</span></span>](app-insights-asp-net-exceptions.md)
2. <span data-ttu-id="bcec4-115">[Nastavit výstrahy](app-insights-alerts.md) na hello výjimka počet metrika</span><span class="sxs-lookup"><span data-stu-id="bcec4-115">[Set an alert](app-insights-alerts.md) on hello Exception count metric</span></span>

### <a name="email-on-an-event-in-my-app"></a><span data-ttu-id="bcec4-116">E-mailu na události v mé aplikace</span><span class="sxs-lookup"><span data-stu-id="bcec4-116">Email on an event in my app</span></span>
<span data-ttu-id="bcec4-117">Předpokládejme, že byste chtěli tooget e-mailu, když dojde k určité události.</span><span class="sxs-lookup"><span data-stu-id="bcec4-117">Let's suppose you'd like tooget an email when a specific event occurs.</span></span> <span data-ttu-id="bcec4-118">Application Insights neposkytuje tato zařízení přímo, ale může [odešle výstrahu, když metriky překračuje prahovou hodnotu](app-insights-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="bcec4-118">Application Insights doesn't provide this facility directly, but it can [send an alert when a metric crosses a threshold](app-insights-alerts.md).</span></span>

<span data-ttu-id="bcec4-119">Výstrahy lze nastavit u [vlastní metriky](app-insights-api-custom-events-metrics.md#trackmetric), i když není vlastních událostí.</span><span class="sxs-lookup"><span data-stu-id="bcec4-119">Alerts can be set on [custom metrics](app-insights-api-custom-events-metrics.md#trackmetric), though not custom events.</span></span> <span data-ttu-id="bcec4-120">Některé tooincrease kód zápisu metriky při výskytu události hello:</span><span class="sxs-lookup"><span data-stu-id="bcec4-120">Write some code tooincrease a metric when hello event occurs:</span></span>

    telemetry.TrackMetric("Alarm", 10);

<span data-ttu-id="bcec4-121">nebo:</span><span class="sxs-lookup"><span data-stu-id="bcec4-121">or:</span></span>

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

<span data-ttu-id="bcec4-122">Vzhledem k tomu, že výstrahy dvou stavů, máte toosend nízkou hodnotu při zvažování hello výstraha toohave byl ukončen:</span><span class="sxs-lookup"><span data-stu-id="bcec4-122">Because alerts have two states, you have toosend a low value when you consider hello alert toohave ended:</span></span>

    telemetry.TrackMetric("Alarm", 0.5);

<span data-ttu-id="bcec4-123">Vytvoření grafu v [metriky explorer](app-insights-metrics-explorer.md) toosee vaše výstrahy:</span><span class="sxs-lookup"><span data-stu-id="bcec4-123">Create a chart in [metric explorer](app-insights-metrics-explorer.md) toosee your alarm:</span></span>

![](./media/app-insights-how-do-i/010-alarm.png)

<span data-ttu-id="bcec4-124">Nyní nastavte výstrahy toofire při hello metrika překročí hodnotu mid na krátkou dobu:</span><span class="sxs-lookup"><span data-stu-id="bcec4-124">Now set an alert toofire when hello metric goes above a mid value for a short period:</span></span>

![](./media/app-insights-how-do-i/020-threshold.png)

<span data-ttu-id="bcec4-125">Nastavit hello průměrování období toohello minimální.</span><span class="sxs-lookup"><span data-stu-id="bcec4-125">Set hello averaging period toohello minimum.</span></span>

<span data-ttu-id="bcec4-126">E-mailů získáte, když překročí hello metrika i pod prahovou hodnotou hello.</span><span class="sxs-lookup"><span data-stu-id="bcec4-126">You'll get emails both when hello metric goes above and below hello threshold.</span></span>

<span data-ttu-id="bcec4-127">Některé tooconsider body:</span><span class="sxs-lookup"><span data-stu-id="bcec4-127">Some points tooconsider:</span></span>

* <span data-ttu-id="bcec4-128">Výstraha má dva stavy ("upozornění" a "v pořádku").</span><span class="sxs-lookup"><span data-stu-id="bcec4-128">An alert has two states ("alert" and "healthy").</span></span> <span data-ttu-id="bcec4-129">Stav Hello vyhodnotí jenom v případě, že je obdržena metriky.</span><span class="sxs-lookup"><span data-stu-id="bcec4-129">hello state is evaluated only when a metric is received.</span></span>
* <span data-ttu-id="bcec4-130">E-mail je odeslán, pouze v případě změny stavu hello.</span><span class="sxs-lookup"><span data-stu-id="bcec4-130">An email is sent only when hello state changes.</span></span> <span data-ttu-id="bcec4-131">To je důvod, proč máte toosend vysoké a nízké hodnoty metriky.</span><span class="sxs-lookup"><span data-stu-id="bcec4-131">This is why you have toosend both high and low-value metrics.</span></span>
* <span data-ttu-id="bcec4-132">tooevaluate hello výstrahy hello průměr je převzat hodnot hello přijatých přes hello předcházející období.</span><span class="sxs-lookup"><span data-stu-id="bcec4-132">tooevaluate hello alert, hello average is taken of hello received values over hello preceding period.</span></span> <span data-ttu-id="bcec4-133">Aby byla odeslána e-mailů častěji, než nastavený interval hello proběhne pokaždé, když byl přijat metriky.</span><span class="sxs-lookup"><span data-stu-id="bcec4-133">This occurs every time a metric is received, so emails can be sent more frequently than hello period you set.</span></span>
* <span data-ttu-id="bcec4-134">Vzhledem k tomu, že se odesílají e-mailů, "upozornění" a "v pořádku", můžete chtít tooconsider znovu myslím jednorázové událost jako podmínku dvou stavů.</span><span class="sxs-lookup"><span data-stu-id="bcec4-134">Since emails are sent both on "alert" and "healthy", you might want tooconsider re-thinking your one-shot event as a two-state condition.</span></span> <span data-ttu-id="bcec4-135">Například místo "Úloha byla dokončena" události, máte podmínku "úloha v průběhu", kde získat e-mailů v hello počáteční a koncové úlohy.</span><span class="sxs-lookup"><span data-stu-id="bcec4-135">For example, instead of a "job completed" event, have a "job in progress" condition, where you get emails at hello start and end of a job.</span></span>

### <a name="set-up-alerts-automatically"></a><span data-ttu-id="bcec4-136">Nastavit výstrahy automaticky</span><span class="sxs-lookup"><span data-stu-id="bcec4-136">Set up alerts automatically</span></span>
[<span data-ttu-id="bcec4-137">Pomocí prostředí PowerShell toocreate nové výstrahy</span><span class="sxs-lookup"><span data-stu-id="bcec4-137">Use PowerShell toocreate new alerts</span></span>](app-insights-alerts.md#automation)

## <a name="use-powershell-toomanage-application-insights"></a><span data-ttu-id="bcec4-138">Pomocí prostředí PowerShell tooManage Application Insights</span><span class="sxs-lookup"><span data-stu-id="bcec4-138">Use PowerShell tooManage Application Insights</span></span>
* [<span data-ttu-id="bcec4-139">Vytvoření nové prostředky</span><span class="sxs-lookup"><span data-stu-id="bcec4-139">Create new resources</span></span>](app-insights-powershell-script-create-resource.md)
* [<span data-ttu-id="bcec4-140">Vytvoření nové výstrahy</span><span class="sxs-lookup"><span data-stu-id="bcec4-140">Create new alerts</span></span>](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a><span data-ttu-id="bcec4-141">Samostatné telemetrická data z různých verzí</span><span class="sxs-lookup"><span data-stu-id="bcec4-141">Separate telemetry from different versions</span></span>

* <span data-ttu-id="bcec4-142">Více rolí v aplikaci: pomocí jednoho prostředku Application Insights a filtrovat cloud_Rolename.</span><span class="sxs-lookup"><span data-stu-id="bcec4-142">Multiple roles in an app: Use a single Application Insights resource, and filter on cloud_Rolename.</span></span> [<span data-ttu-id="bcec4-143">Další informace</span><span class="sxs-lookup"><span data-stu-id="bcec4-143">Learn more</span></span>](app-insights-monitor-multi-role-apps.md)
* <span data-ttu-id="bcec4-144">Oddělení vývoj, testování a verze: použití různých prostředků Application Insights.</span><span class="sxs-lookup"><span data-stu-id="bcec4-144">Separating development, test, and release versions: Use different Application Insights resources.</span></span> <span data-ttu-id="bcec4-145">Vyberte si klíčů instrumentace hello ze souboru web.config. [Další informace](app-insights-separate-resources.md)</span><span class="sxs-lookup"><span data-stu-id="bcec4-145">Pick up hello instrumentation keys from web.config. [Learn more](app-insights-separate-resources.md)</span></span>
* <span data-ttu-id="bcec4-146">Vytváření sestav sestavení verze: Přidání vlastnosti pomocí inicializátoru telemetrie.</span><span class="sxs-lookup"><span data-stu-id="bcec4-146">Reporting build versions: Add a property using a telemetry initializer.</span></span> [<span data-ttu-id="bcec4-147">Další informace</span><span class="sxs-lookup"><span data-stu-id="bcec4-147">Learn more</span></span>](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a><span data-ttu-id="bcec4-148">Monitorování back-end serverů a aplikace klasické pracovní plochy</span><span class="sxs-lookup"><span data-stu-id="bcec4-148">Monitor backend servers and desktop apps</span></span>
<span data-ttu-id="bcec4-149">[Modul Windows Server SDK hello použití](app-insights-windows-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="bcec4-149">[Use hello Windows Server SDK module](app-insights-windows-desktop.md).</span></span>

## <a name="visualize-data"></a><span data-ttu-id="bcec4-150">Vizualizaci dat</span><span class="sxs-lookup"><span data-stu-id="bcec4-150">Visualize data</span></span>
#### <a name="dashboard-with-metrics-from-multiple-apps"></a><span data-ttu-id="bcec4-151">Řídicí panel se metriky z více aplikací</span><span class="sxs-lookup"><span data-stu-id="bcec4-151">Dashboard with metrics from multiple apps</span></span>
* <span data-ttu-id="bcec4-152">V [Explorer metrika](app-insights-metrics-explorer.md), graf přizpůsobit a uložit jako oblíbenou položku.</span><span class="sxs-lookup"><span data-stu-id="bcec4-152">In [Metric Explorer](app-insights-metrics-explorer.md), customize your chart and save it as a favorite.</span></span> <span data-ttu-id="bcec4-153">Připnete toohello řídicí panel Azure.</span><span class="sxs-lookup"><span data-stu-id="bcec4-153">Pin it toohello Azure dashboard.</span></span>

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a><span data-ttu-id="bcec4-154">Řídicí panel se data z jiných zdrojů a Application Insights</span><span class="sxs-lookup"><span data-stu-id="bcec4-154">Dashboard with data from other sources and Application Insights</span></span>
* <span data-ttu-id="bcec4-155">[Export telemetrie tooPower BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="bcec4-155">[Export telemetry tooPower BI](app-insights-export-power-bi.md).</span></span>

<span data-ttu-id="bcec4-156">Nebo</span><span class="sxs-lookup"><span data-stu-id="bcec4-156">Or</span></span>

* <span data-ttu-id="bcec4-157">Používání služby SharePoint jako řídicí panel, zobrazení dat ve webové části služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="bcec4-157">Use SharePoint as your dashboard, displaying data in SharePoint web parts.</span></span> <span data-ttu-id="bcec4-158">[Použít průběžné export a Stream Analytics tooexport tooSQL](app-insights-code-sample-export-sql-stream-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="bcec4-158">[Use continuous export and Stream Analytics tooexport tooSQL](app-insights-code-sample-export-sql-stream-analytics.md).</span></span>  <span data-ttu-id="bcec4-159">Použít databázi hello tooexamine PowerView a vytvoření webové části služby SharePoint pro PowerView.</span><span class="sxs-lookup"><span data-stu-id="bcec4-159">Use PowerView tooexamine hello database, and create a SharePoint web part for PowerView.</span></span>

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a><span data-ttu-id="bcec4-160">Filtrovat anonymní nebo ověřeného uživatele</span><span class="sxs-lookup"><span data-stu-id="bcec4-160">Filter out anonymous or authenticated users</span></span>
<span data-ttu-id="bcec4-161">Pokud vaši uživatelé přihlásí, můžete nastavit hello [ověřit id uživatele](app-insights-api-custom-events-metrics.md#authenticated-users). (Ho nedojde automaticky.)</span><span class="sxs-lookup"><span data-stu-id="bcec4-161">If your users sign in, you can set hello [authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users). (It doesn't happen automatically.)</span></span>

<span data-ttu-id="bcec4-162">Pak můžete:</span><span class="sxs-lookup"><span data-stu-id="bcec4-162">You can then:</span></span>

* <span data-ttu-id="bcec4-163">Hledání ID konkrétního uživatele</span><span class="sxs-lookup"><span data-stu-id="bcec4-163">Search on specific user ids</span></span>

![](./media/app-insights-how-do-i/110-search.png)

* <span data-ttu-id="bcec4-164">Filtrovat metriky tooeither anonymní nebo ověřeného uživatele</span><span class="sxs-lookup"><span data-stu-id="bcec4-164">Filter metrics tooeither anonymous or authenticated users</span></span>

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a><span data-ttu-id="bcec4-165">Upravit názvy vlastností nebo hodnot</span><span class="sxs-lookup"><span data-stu-id="bcec4-165">Modify property names or values</span></span>
<span data-ttu-id="bcec4-166">Vytvoření [filtru](app-insights-api-filtering-sampling.md#filtering).</span><span class="sxs-lookup"><span data-stu-id="bcec4-166">Create a [filter](app-insights-api-filtering-sampling.md#filtering).</span></span> <span data-ttu-id="bcec4-167">Díky tomu můžete upravit nebo filtrování telemetrie před odesláním z vaší aplikace tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="bcec4-167">This lets you modify or filter telemetry before it is sent from your app tooApplication Insights.</span></span>

## <a name="list-specific-users-and-their-usage"></a><span data-ttu-id="bcec4-168">Seznam konkrétních uživatelů a jejich využití</span><span class="sxs-lookup"><span data-stu-id="bcec4-168">List specific users and their usage</span></span>
<span data-ttu-id="bcec4-169">Pokud chcete příliš[vyhledávání pro konkrétní uživatele](#search-specific-users), můžete nastavit hello [ověřit id uživatele](app-insights-api-custom-events-metrics.md#authenticated-users).</span><span class="sxs-lookup"><span data-stu-id="bcec4-169">If you just want too[search for specific users](#search-specific-users), you can set hello [authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users).</span></span>

<span data-ttu-id="bcec4-170">Pokud chcete seznam uživatelů s daty, jako je například jaké stránky se podívejte se na nebo jak často se přihlásit, máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="bcec4-170">If you want a list of users with data such as what pages they look at or how often they log in, you have two options:</span></span>

* <span data-ttu-id="bcec4-171">[Id sady ověřený uživatel](app-insights-api-custom-events-metrics.md#authenticated-users), [export databáze. tooa](app-insights-code-sample-export-sql-stream-analytics.md) a pomocí vhodného nástroje tooanalyze existuje uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="bcec4-171">[Set authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users), [export tooa database](app-insights-code-sample-export-sql-stream-analytics.md) and use suitable tools tooanalyze your user data there.</span></span>
* <span data-ttu-id="bcec4-172">Pokud máte pouze malý počet uživatelů, odesílat vlastní události nebo metriky, pomocí hello data týkající se jako hello hodnota metriky nebo název události a id uživatele hello nastavení jako vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bcec4-172">If you have only a small number of users, send custom events or metrics, using hello data of interest as hello metric value or event name, and setting hello user id as a property.</span></span> <span data-ttu-id="bcec4-173">zobrazení stránky tooanalyze, nahraďte hello volání trackPageView standardní jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bcec4-173">tooanalyze page views, replace hello standard JavaScript trackPageView call.</span></span> <span data-ttu-id="bcec4-174">telemetrických dat na straně serveru tooanalyze, používat telemetrie inicializátoru tooadd hello uživatelské id tooall telemetrii serveru.</span><span class="sxs-lookup"><span data-stu-id="bcec4-174">tooanalyze server-side telemetry, use a telemetry initializer tooadd hello user id tooall server telemetry.</span></span> <span data-ttu-id="bcec4-175">Pak můžete filtrovat a segment metriky a hledání na id uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="bcec4-175">You can then filter and segment metrics and searches on hello user id.</span></span>

## <a name="reduce-traffic-from-my-app-tooapplication-insights"></a><span data-ttu-id="bcec4-176">Omezit přenos z mé aplikace tooApplication statistiky</span><span class="sxs-lookup"><span data-stu-id="bcec4-176">Reduce traffic from my app tooApplication Insights</span></span>
* <span data-ttu-id="bcec4-177">V [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), zakažte všechny moduly, nepotřebujete, kolekce pro čítač výkonu takové hello.</span><span class="sxs-lookup"><span data-stu-id="bcec4-177">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), disable any modules you don't need, such hello performance counter collector.</span></span>
* <span data-ttu-id="bcec4-178">Použití [vzorkování a filtrování](app-insights-api-filtering-sampling.md) v hello SDK.</span><span class="sxs-lookup"><span data-stu-id="bcec4-178">Use [Sampling and filtering](app-insights-api-filtering-sampling.md) at hello SDK.</span></span>
* <span data-ttu-id="bcec4-179">Na webových stránkách omezte hello počet volání Ajax hlášených pro každé stránky zobrazení.</span><span class="sxs-lookup"><span data-stu-id="bcec4-179">In your web pages, Limit hello number of Ajax calls reported for every page view.</span></span> <span data-ttu-id="bcec4-180">Ve fragmentu hello skriptu po `instrumentationKey:...` , vložte: `,maxAjaxCallsPerView:3` (nebo vhodný číslo).</span><span class="sxs-lookup"><span data-stu-id="bcec4-180">In hello script snippet after `instrumentationKey:...` , insert: `,maxAjaxCallsPerView:3` (or a suitable number).</span></span>
* <span data-ttu-id="bcec4-181">Pokud používáte [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), výpočetní hello agregace dávek hodnot metriky před odesláním hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="bcec4-181">If you're using [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), compute hello aggregate of batches of metric values before sending hello result.</span></span> <span data-ttu-id="bcec4-182">Přetížení TrackMetric(), která poskytuje pro tento není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bcec4-182">There's an overload of TrackMetric() that provides for that.</span></span>

<span data-ttu-id="bcec4-183">Další informace o [ceny a kvóty](app-insights-pricing.md).</span><span class="sxs-lookup"><span data-stu-id="bcec4-183">Learn more about [pricing and quotas](app-insights-pricing.md).</span></span>

## <a name="disable-telemetry"></a><span data-ttu-id="bcec4-184">Zakázat telemetrii</span><span class="sxs-lookup"><span data-stu-id="bcec4-184">Disable telemetry</span></span>
<span data-ttu-id="bcec4-185">příliš**dynamicky zastavit a spustit** hello shromažďování a předávání telemetrie ze serveru hello:</span><span class="sxs-lookup"><span data-stu-id="bcec4-185">too**dynamically stop and start** hello collection and transmission of telemetry from hello server:</span></span>

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



<span data-ttu-id="bcec4-186">příliš**zakázat vybrané standardní Kolektory** – například čítače výkonu, požadavky HTTP nebo závislosti – odstranit nebo komentář hello relevantní řádků v [souboru ApplicationInsights.config](app-insights-api-custom-events-metrics.md). Můžete tak učinit, například pokud chcete toosend TrackRequest data.</span><span class="sxs-lookup"><span data-stu-id="bcec4-186">too**disable selected standard collectors** - for example, performance counters, HTTP requests, or dependencies - delete or comment out hello relevant lines in [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). You could do this, for example, if you want toosend your own TrackRequest data.</span></span>

## <a name="view-system-performance-counters"></a><span data-ttu-id="bcec4-187">Čítače výkonu systému zobrazení</span><span class="sxs-lookup"><span data-stu-id="bcec4-187">View system performance counters</span></span>
<span data-ttu-id="bcec4-188">Mezi hello metriky, které můžete zobrazit v Průzkumníku metrik jsou sady čítačů výkonu systému.</span><span class="sxs-lookup"><span data-stu-id="bcec4-188">Among hello metrics you can show in metrics explorer are a set of system performance counters.</span></span> <span data-ttu-id="bcec4-189">Je předdefinovaný okno s názvem **servery** který zobrazí několik z nich.</span><span class="sxs-lookup"><span data-stu-id="bcec4-189">There's a predefined blade titled **Servers** that displays several of them.</span></span>

![Otevřete prostředek Application Insights a klikněte na servery](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a><span data-ttu-id="bcec4-191">Pokud se zobrazí žádná data čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="bcec4-191">If you see no performance counter data</span></span>
* <span data-ttu-id="bcec4-192">**Server služby IIS** vlastní počítače nebo na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="bcec4-192">**IIS server** on your own machine or on a VM.</span></span> <span data-ttu-id="bcec4-193">[Nainstalujte monitorování stavu](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="bcec4-193">[Install Status Monitor](app-insights-monitor-performance-live-website-now.md).</span></span>
* <span data-ttu-id="bcec4-194">**Webu Azure** -ještě nepodporujeme čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="bcec4-194">**Azure web site** - we don't support performance counters yet.</span></span> <span data-ttu-id="bcec4-195">Existuje několik metriky, které můžete získat jako standardní součást hello webu Azure ovládací panely.</span><span class="sxs-lookup"><span data-stu-id="bcec4-195">There are several metrics you can get as a standard part of hello Azure web site control panel.</span></span>
* <span data-ttu-id="bcec4-196">**UNIX server** - [nainstalujte collectd](app-insights-java-collectd.md)</span><span class="sxs-lookup"><span data-stu-id="bcec4-196">**Unix server** - [Install collectd](app-insights-java-collectd.md)</span></span>

### <a name="toodisplay-more-performance-counters"></a><span data-ttu-id="bcec4-197">toodisplay další čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="bcec4-197">toodisplay more performance counters</span></span>
* <span data-ttu-id="bcec4-198">První, [přidejte nový graf](app-insights-metrics-explorer.md) a zjistit, zda hello čítač je v hello základní nastavení, nabízíme.</span><span class="sxs-lookup"><span data-stu-id="bcec4-198">First, [add a new chart](app-insights-metrics-explorer.md) and see if hello counter is in hello basic set that we offer.</span></span>
* <span data-ttu-id="bcec4-199">Pokud ne, [přidat sada toohello čítačů hello shromažďují modul čítače výkonu hello](app-insights-performance-counters.md).</span><span class="sxs-lookup"><span data-stu-id="bcec4-199">If not, [add hello counter toohello set collected by hello performance counter module](app-insights-performance-counters.md).</span></span>
