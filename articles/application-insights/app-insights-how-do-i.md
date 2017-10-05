---
title: "Jak na to... ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: ef63e06c0621753e0a706d6efb709b943e38ee42
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-do-i--in-application-insights"></a><span data-ttu-id="1ef46-103">Jak mám udělat ... pomocí Application Insights?</span><span class="sxs-lookup"><span data-stu-id="1ef46-103">How do I ... in Application Insights?</span></span>
## <a name="get-an-email-when-"></a><span data-ttu-id="1ef46-104">Získat e-mailem při...</span><span class="sxs-lookup"><span data-stu-id="1ef46-104">Get an email when ...</span></span>
### <a name="email-if-my-site-goes-down"></a><span data-ttu-id="1ef46-105">Pokud web přestane fungovat e-mailu</span><span class="sxs-lookup"><span data-stu-id="1ef46-105">Email if my site goes down</span></span>
<span data-ttu-id="1ef46-106">Nastavit [dostupnosti webového testu](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="1ef46-106">Set an [availability web test](app-insights-monitor-web-app-availability.md).</span></span>

### <a name="email-if-my-site-is-overloaded"></a><span data-ttu-id="1ef46-107">E-mailu, pokud je přetížena mé lokality</span><span class="sxs-lookup"><span data-stu-id="1ef46-107">Email if my site is overloaded</span></span>
<span data-ttu-id="1ef46-108">Nastavit [výstraha](app-insights-alerts.md) na **doba odezvy serveru**.</span><span class="sxs-lookup"><span data-stu-id="1ef46-108">Set an [alert](app-insights-alerts.md) on **Server response time**.</span></span> <span data-ttu-id="1ef46-109">Prahová hodnota mezi 1 a 2 sekundami by měly fungovat.</span><span class="sxs-lookup"><span data-stu-id="1ef46-109">A threshold between 1 and 2 seconds should work.</span></span>

![](./media/app-insights-how-do-i/030-server.png)

<span data-ttu-id="1ef46-110">Aplikace může také zobrazit známky kmen vrácením selhání kódy.</span><span class="sxs-lookup"><span data-stu-id="1ef46-110">Your app might also show signs of strain by returning failure codes.</span></span> <span data-ttu-id="1ef46-111">Nastavit výstrahy na **neúspěšné požadavky**.</span><span class="sxs-lookup"><span data-stu-id="1ef46-111">Set an alert on **Failed requests**.</span></span>

<span data-ttu-id="1ef46-112">Pokud chcete nastavit upozornění na **výjimky serveru**, možná budete muset udělat [některé další nastavení](app-insights-asp-net-exceptions.md) Chcete-li zobrazit data.</span><span class="sxs-lookup"><span data-stu-id="1ef46-112">If you want to set an alert on **Server exceptions**, you might have to do [some additional setup](app-insights-asp-net-exceptions.md) in order to see data.</span></span>

### <a name="email-on-exceptions"></a><span data-ttu-id="1ef46-113">E-mailu na výjimky</span><span class="sxs-lookup"><span data-stu-id="1ef46-113">Email on exceptions</span></span>
1. [<span data-ttu-id="1ef46-114">Nastavili monitorování výjimek</span><span class="sxs-lookup"><span data-stu-id="1ef46-114">Set up exception monitoring</span></span>](app-insights-asp-net-exceptions.md)
2. <span data-ttu-id="1ef46-115">[Nastavit výstrahy](app-insights-alerts.md) na výjimku počet metrika</span><span class="sxs-lookup"><span data-stu-id="1ef46-115">[Set an alert](app-insights-alerts.md) on the Exception count metric</span></span>

### <a name="email-on-an-event-in-my-app"></a><span data-ttu-id="1ef46-116">E-mailu na události v mé aplikace</span><span class="sxs-lookup"><span data-stu-id="1ef46-116">Email on an event in my app</span></span>
<span data-ttu-id="1ef46-117">Předpokládejme, že chcete získat e-mailu, když dojde k určité události.</span><span class="sxs-lookup"><span data-stu-id="1ef46-117">Let's suppose you'd like to get an email when a specific event occurs.</span></span> <span data-ttu-id="1ef46-118">Application Insights neposkytuje tato zařízení přímo, ale může [odešle výstrahu, když metriky překračuje prahovou hodnotu](app-insights-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="1ef46-118">Application Insights doesn't provide this facility directly, but it can [send an alert when a metric crosses a threshold](app-insights-alerts.md).</span></span>

<span data-ttu-id="1ef46-119">Výstrahy lze nastavit u [vlastní metriky](app-insights-api-custom-events-metrics.md#trackmetric), i když není vlastních událostí.</span><span class="sxs-lookup"><span data-stu-id="1ef46-119">Alerts can be set on [custom metrics](app-insights-api-custom-events-metrics.md#trackmetric), though not custom events.</span></span> <span data-ttu-id="1ef46-120">Napište kód, který zvýšit metriky, když dojde k události:</span><span class="sxs-lookup"><span data-stu-id="1ef46-120">Write some code to increase a metric when the event occurs:</span></span>

    telemetry.TrackMetric("Alarm", 10);

<span data-ttu-id="1ef46-121">nebo:</span><span class="sxs-lookup"><span data-stu-id="1ef46-121">or:</span></span>

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

<span data-ttu-id="1ef46-122">Vzhledem k tomu, že výstrahy dvou stavů, budete muset odeslat nízkou hodnotu, při zvažování výstrahu, kterou chcete být ukončeny:</span><span class="sxs-lookup"><span data-stu-id="1ef46-122">Because alerts have two states, you have to send a low value when you consider the alert to have ended:</span></span>

    telemetry.TrackMetric("Alarm", 0.5);

<span data-ttu-id="1ef46-123">Vytvoření grafu v [metriky explorer](app-insights-metrics-explorer.md) zobrazíte vaší výstrahy:</span><span class="sxs-lookup"><span data-stu-id="1ef46-123">Create a chart in [metric explorer](app-insights-metrics-explorer.md) to see your alarm:</span></span>

![](./media/app-insights-how-do-i/010-alarm.png)

<span data-ttu-id="1ef46-124">Nyní nastavte výstrahu má provést při metriku překročí hodnotu mid na krátkou dobu:</span><span class="sxs-lookup"><span data-stu-id="1ef46-124">Now set an alert to fire when the metric goes above a mid value for a short period:</span></span>

![](./media/app-insights-how-do-i/020-threshold.png)

<span data-ttu-id="1ef46-125">Průměrný interval nastavte na minimum.</span><span class="sxs-lookup"><span data-stu-id="1ef46-125">Set the averaging period to the minimum.</span></span>

<span data-ttu-id="1ef46-126">E-mailů získáte, když překročí metriku i pod prahovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="1ef46-126">You'll get emails both when the metric goes above and below the threshold.</span></span>

<span data-ttu-id="1ef46-127">Některé body vzít v úvahu:</span><span class="sxs-lookup"><span data-stu-id="1ef46-127">Some points to consider:</span></span>

* <span data-ttu-id="1ef46-128">Výstraha má dva stavy ("upozornění" a "v pořádku").</span><span class="sxs-lookup"><span data-stu-id="1ef46-128">An alert has two states ("alert" and "healthy").</span></span> <span data-ttu-id="1ef46-129">Stav se vyhodnocují jenom v případě, že je obdržena metriky.</span><span class="sxs-lookup"><span data-stu-id="1ef46-129">The state is evaluated only when a metric is received.</span></span>
* <span data-ttu-id="1ef46-130">E-mail je odeslán, pouze pokud se stav změní.</span><span class="sxs-lookup"><span data-stu-id="1ef46-130">An email is sent only when the state changes.</span></span> <span data-ttu-id="1ef46-131">Toto je proč budete muset odeslat obě vysoké a nízké hodnoty metriky.</span><span class="sxs-lookup"><span data-stu-id="1ef46-131">This is why you have to send both high and low-value metrics.</span></span>
* <span data-ttu-id="1ef46-132">Abyste mohli vyhodnotit výstrahy, průměr prováděné přijaté hodnoty předchozí období.</span><span class="sxs-lookup"><span data-stu-id="1ef46-132">To evaluate the alert, the average is taken of the received values over the preceding period.</span></span> <span data-ttu-id="1ef46-133">Aby byla odeslána e-mailů častěji, než nastavený interval proběhne pokaždé, když byl přijat metriky.</span><span class="sxs-lookup"><span data-stu-id="1ef46-133">This occurs every time a metric is received, so emails can be sent more frequently than the period you set.</span></span>
* <span data-ttu-id="1ef46-134">Vzhledem k tomu, že se odesílají e-mailů, "upozornění" a "v pořádku", můžete znovu myslím jednorázové událost jako podmínku dvou stavů.</span><span class="sxs-lookup"><span data-stu-id="1ef46-134">Since emails are sent both on "alert" and "healthy", you might want to consider re-thinking your one-shot event as a two-state condition.</span></span> <span data-ttu-id="1ef46-135">Například místo události "úloha dokončena" máte podmínku "úloha v průběhu", kde získat e-mailů při spuštění a ukončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="1ef46-135">For example, instead of a "job completed" event, have a "job in progress" condition, where you get emails at the start and end of a job.</span></span>

### <a name="set-up-alerts-automatically"></a><span data-ttu-id="1ef46-136">Nastavit výstrahy automaticky</span><span class="sxs-lookup"><span data-stu-id="1ef46-136">Set up alerts automatically</span></span>
[<span data-ttu-id="1ef46-137">Pomocí prostředí PowerShell vytvořit nové výstrahy</span><span class="sxs-lookup"><span data-stu-id="1ef46-137">Use PowerShell to create new alerts</span></span>](app-insights-alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a><span data-ttu-id="1ef46-138">Pomocí prostředí PowerShell pro správu služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="1ef46-138">Use PowerShell to Manage Application Insights</span></span>
* [<span data-ttu-id="1ef46-139">Vytvoření nové prostředky</span><span class="sxs-lookup"><span data-stu-id="1ef46-139">Create new resources</span></span>](app-insights-powershell-script-create-resource.md)
* [<span data-ttu-id="1ef46-140">Vytvoření nové výstrahy</span><span class="sxs-lookup"><span data-stu-id="1ef46-140">Create new alerts</span></span>](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a><span data-ttu-id="1ef46-141">Samostatné telemetrická data z různých verzí</span><span class="sxs-lookup"><span data-stu-id="1ef46-141">Separate telemetry from different versions</span></span>

* <span data-ttu-id="1ef46-142">Více rolí v aplikaci: pomocí jednoho prostředku Application Insights a filtrovat cloud_Rolename.</span><span class="sxs-lookup"><span data-stu-id="1ef46-142">Multiple roles in an app: Use a single Application Insights resource, and filter on cloud_Rolename.</span></span> [<span data-ttu-id="1ef46-143">Další informace</span><span class="sxs-lookup"><span data-stu-id="1ef46-143">Learn more</span></span>](app-insights-monitor-multi-role-apps.md)
* <span data-ttu-id="1ef46-144">Oddělení vývoj, testování a verze: použití různých prostředků Application Insights.</span><span class="sxs-lookup"><span data-stu-id="1ef46-144">Separating development, test, and release versions: Use different Application Insights resources.</span></span> <span data-ttu-id="1ef46-145">Vyberte si klíčů instrumentace ze souboru web.config. [Další informace](app-insights-separate-resources.md)</span><span class="sxs-lookup"><span data-stu-id="1ef46-145">Pick up the instrumentation keys from web.config. [Learn more](app-insights-separate-resources.md)</span></span>
* <span data-ttu-id="1ef46-146">Vytváření sestav sestavení verze: Přidání vlastnosti pomocí inicializátoru telemetrie.</span><span class="sxs-lookup"><span data-stu-id="1ef46-146">Reporting build versions: Add a property using a telemetry initializer.</span></span> [<span data-ttu-id="1ef46-147">Další informace</span><span class="sxs-lookup"><span data-stu-id="1ef46-147">Learn more</span></span>](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a><span data-ttu-id="1ef46-148">Monitorování back-end serverů a aplikace klasické pracovní plochy</span><span class="sxs-lookup"><span data-stu-id="1ef46-148">Monitor backend servers and desktop apps</span></span>
<span data-ttu-id="1ef46-149">[Použít modul Windows Server SDK](app-insights-windows-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="1ef46-149">[Use the Windows Server SDK module](app-insights-windows-desktop.md).</span></span>

## <a name="visualize-data"></a><span data-ttu-id="1ef46-150">Vizualizaci dat</span><span class="sxs-lookup"><span data-stu-id="1ef46-150">Visualize data</span></span>
#### <a name="dashboard-with-metrics-from-multiple-apps"></a><span data-ttu-id="1ef46-151">Řídicí panel se metriky z více aplikací</span><span class="sxs-lookup"><span data-stu-id="1ef46-151">Dashboard with metrics from multiple apps</span></span>
* <span data-ttu-id="1ef46-152">V [Explorer metrika](app-insights-metrics-explorer.md), graf přizpůsobit a uložit jako oblíbenou položku.</span><span class="sxs-lookup"><span data-stu-id="1ef46-152">In [Metric Explorer](app-insights-metrics-explorer.md), customize your chart and save it as a favorite.</span></span> <span data-ttu-id="1ef46-153">Připnete na řídicí panel Azure.</span><span class="sxs-lookup"><span data-stu-id="1ef46-153">Pin it to the Azure dashboard.</span></span>

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a><span data-ttu-id="1ef46-154">Řídicí panel se data z jiných zdrojů a Application Insights</span><span class="sxs-lookup"><span data-stu-id="1ef46-154">Dashboard with data from other sources and Application Insights</span></span>
* <span data-ttu-id="1ef46-155">[Export telemetrie do Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="1ef46-155">[Export telemetry to Power BI](app-insights-export-power-bi.md).</span></span>

<span data-ttu-id="1ef46-156">Nebo</span><span class="sxs-lookup"><span data-stu-id="1ef46-156">Or</span></span>

* <span data-ttu-id="1ef46-157">Používání služby SharePoint jako řídicí panel, zobrazení dat ve webové části služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="1ef46-157">Use SharePoint as your dashboard, displaying data in SharePoint web parts.</span></span> <span data-ttu-id="1ef46-158">[Použít průběžné export a Stream Analytics exportovat do SQL](app-insights-code-sample-export-sql-stream-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="1ef46-158">[Use continuous export and Stream Analytics to export to SQL](app-insights-code-sample-export-sql-stream-analytics.md).</span></span>  <span data-ttu-id="1ef46-159">Kontrola databáze pomocí PowerView a vytvoření webové části služby SharePoint pro PowerView.</span><span class="sxs-lookup"><span data-stu-id="1ef46-159">Use PowerView to examine the database, and create a SharePoint web part for PowerView.</span></span>

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a><span data-ttu-id="1ef46-160">Filtrovat anonymní nebo ověřeného uživatele</span><span class="sxs-lookup"><span data-stu-id="1ef46-160">Filter out anonymous or authenticated users</span></span>
<span data-ttu-id="1ef46-161">Pokud vaši uživatelé přihlásí, můžete nastavit [ověřit id uživatele](app-insights-api-custom-events-metrics.md#authenticated-users). (Ho nedojde automaticky.)</span><span class="sxs-lookup"><span data-stu-id="1ef46-161">If your users sign in, you can set the [authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users). (It doesn't happen automatically.)</span></span>

<span data-ttu-id="1ef46-162">Pak můžete:</span><span class="sxs-lookup"><span data-stu-id="1ef46-162">You can then:</span></span>

* <span data-ttu-id="1ef46-163">Hledání ID konkrétního uživatele</span><span class="sxs-lookup"><span data-stu-id="1ef46-163">Search on specific user ids</span></span>

![](./media/app-insights-how-do-i/110-search.png)

* <span data-ttu-id="1ef46-164">Filtr metriky pro anonymní nebo ověřeného uživatele</span><span class="sxs-lookup"><span data-stu-id="1ef46-164">Filter metrics to either anonymous or authenticated users</span></span>

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a><span data-ttu-id="1ef46-165">Upravit názvy vlastností nebo hodnot</span><span class="sxs-lookup"><span data-stu-id="1ef46-165">Modify property names or values</span></span>
<span data-ttu-id="1ef46-166">Vytvoření [filtru](app-insights-api-filtering-sampling.md#filtering).</span><span class="sxs-lookup"><span data-stu-id="1ef46-166">Create a [filter](app-insights-api-filtering-sampling.md#filtering).</span></span> <span data-ttu-id="1ef46-167">Díky tomu můžete upravit nebo filtrování telemetrie před odesláním z vaší aplikace do služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="1ef46-167">This lets you modify or filter telemetry before it is sent from your app to Application Insights.</span></span>

## <a name="list-specific-users-and-their-usage"></a><span data-ttu-id="1ef46-168">Seznam konkrétních uživatelů a jejich využití</span><span class="sxs-lookup"><span data-stu-id="1ef46-168">List specific users and their usage</span></span>
<span data-ttu-id="1ef46-169">Pokud chcete [vyhledávání pro konkrétní uživatele](#search-specific-users), můžete nastavit [ověřit id uživatele](app-insights-api-custom-events-metrics.md#authenticated-users).</span><span class="sxs-lookup"><span data-stu-id="1ef46-169">If you just want to [search for specific users](#search-specific-users), you can set the [authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users).</span></span>

<span data-ttu-id="1ef46-170">Pokud chcete seznam uživatelů s daty, jako je například jaké stránky se podívejte se na nebo jak často se přihlásit, máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="1ef46-170">If you want a list of users with data such as what pages they look at or how often they log in, you have two options:</span></span>

* <span data-ttu-id="1ef46-171">[Id sady ověřený uživatel](app-insights-api-custom-events-metrics.md#authenticated-users), [exportovat do databáze](app-insights-code-sample-export-sql-stream-analytics.md) a použití nástrojů vhodný k analýze dat uživatele existuje.</span><span class="sxs-lookup"><span data-stu-id="1ef46-171">[Set authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users), [export to a database](app-insights-code-sample-export-sql-stream-analytics.md) and use suitable tools to analyze your user data there.</span></span>
* <span data-ttu-id="1ef46-172">Pokud máte pouze malý počet uživatelů, odesílat vlastní události nebo metriky, používají data týkající se jako název metriky hodnotu nebo událost a nastavení jako vlastnost id uživatele.</span><span class="sxs-lookup"><span data-stu-id="1ef46-172">If you have only a small number of users, send custom events or metrics, using the data of interest as the metric value or event name, and setting the user id as a property.</span></span> <span data-ttu-id="1ef46-173">Chcete-li analyzovat zobrazení stránky, nahraďte standardní trackPageView volání jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1ef46-173">To analyze page views, replace the standard JavaScript trackPageView call.</span></span> <span data-ttu-id="1ef46-174">K analýze telemetrických dat na straně serveru, pomocí inicializátoru telemetrie přidejte id uživatele pro všechny telemetrii serveru.</span><span class="sxs-lookup"><span data-stu-id="1ef46-174">To analyze server-side telemetry, use a telemetry initializer to add the user id to all server telemetry.</span></span> <span data-ttu-id="1ef46-175">Pak můžete filtrovat a segment metriky a hledání na id uživatele.</span><span class="sxs-lookup"><span data-stu-id="1ef46-175">You can then filter and segment metrics and searches on the user id.</span></span>

## <a name="reduce-traffic-from-my-app-to-application-insights"></a><span data-ttu-id="1ef46-176">Omezit přenos z mé aplikace do služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="1ef46-176">Reduce traffic from my app to Application Insights</span></span>
* <span data-ttu-id="1ef46-177">V [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), zakažte všechny moduly, nepotřebujete, takové do kolekce pro čítač výkonu.</span><span class="sxs-lookup"><span data-stu-id="1ef46-177">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), disable any modules you don't need, such the performance counter collector.</span></span>
* <span data-ttu-id="1ef46-178">Použití [vzorkování a filtrování](app-insights-api-filtering-sampling.md) v sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="1ef46-178">Use [Sampling and filtering](app-insights-api-filtering-sampling.md) at the SDK.</span></span>
* <span data-ttu-id="1ef46-179">Na webových stránkách omezte počet volání Ajax hlášených pro každé stránky zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1ef46-179">In your web pages, Limit the number of Ajax calls reported for every page view.</span></span> <span data-ttu-id="1ef46-180">V tomto fragmentu kódu skriptu po `instrumentationKey:...` , vložte: `,maxAjaxCallsPerView:3` (nebo vhodný číslo).</span><span class="sxs-lookup"><span data-stu-id="1ef46-180">In the script snippet after `instrumentationKey:...` , insert: `,maxAjaxCallsPerView:3` (or a suitable number).</span></span>
* <span data-ttu-id="1ef46-181">Pokud používáte [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), výpočetní agregace dávek hodnot metriky před odesláním výsledek.</span><span class="sxs-lookup"><span data-stu-id="1ef46-181">If you're using [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), compute the aggregate of batches of metric values before sending the result.</span></span> <span data-ttu-id="1ef46-182">Přetížení TrackMetric(), která poskytuje pro tento není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="1ef46-182">There's an overload of TrackMetric() that provides for that.</span></span>

<span data-ttu-id="1ef46-183">Další informace o [ceny a kvóty](app-insights-pricing.md).</span><span class="sxs-lookup"><span data-stu-id="1ef46-183">Learn more about [pricing and quotas](app-insights-pricing.md).</span></span>

## <a name="disable-telemetry"></a><span data-ttu-id="1ef46-184">Zakázat telemetrii</span><span class="sxs-lookup"><span data-stu-id="1ef46-184">Disable telemetry</span></span>
<span data-ttu-id="1ef46-185">K **dynamicky zastavit a spustit** shromažďování a předávání telemetrie ze serveru:</span><span class="sxs-lookup"><span data-stu-id="1ef46-185">To **dynamically stop and start** the collection and transmission of telemetry from the server:</span></span>

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



<span data-ttu-id="1ef46-186">K **zakázat vybrané standardní Kolektory** – například čítače výkonu, požadavky HTTP nebo závislosti – odstranit nebo komentář příslušné řádky v [souboru ApplicationInsights.config](app-insights-api-custom-events-metrics.md). Můžete tak učinit, například pokud chcete odeslat vlastní TrackRequest data.</span><span class="sxs-lookup"><span data-stu-id="1ef46-186">To **disable selected standard collectors** - for example, performance counters, HTTP requests, or dependencies - delete or comment out the relevant lines in [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). You could do this, for example, if you want to send your own TrackRequest data.</span></span>

## <a name="view-system-performance-counters"></a><span data-ttu-id="1ef46-187">Čítače výkonu systému zobrazení</span><span class="sxs-lookup"><span data-stu-id="1ef46-187">View system performance counters</span></span>
<span data-ttu-id="1ef46-188">Mezi metriky, které můžete zobrazit v Průzkumníku metrik jsou sady systému čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="1ef46-188">Among the metrics you can show in metrics explorer are a set of system performance counters.</span></span> <span data-ttu-id="1ef46-189">Je předdefinovaný okno s názvem **servery** který zobrazí několik z nich.</span><span class="sxs-lookup"><span data-stu-id="1ef46-189">There's a predefined blade titled **Servers** that displays several of them.</span></span>

![Otevřete prostředek Application Insights a klikněte na servery](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a><span data-ttu-id="1ef46-191">Pokud se zobrazí žádná data čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="1ef46-191">If you see no performance counter data</span></span>
* <span data-ttu-id="1ef46-192">**Server služby IIS** vlastní počítače nebo na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="1ef46-192">**IIS server** on your own machine or on a VM.</span></span> <span data-ttu-id="1ef46-193">[Nainstalujte monitorování stavu](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="1ef46-193">[Install Status Monitor](app-insights-monitor-performance-live-website-now.md).</span></span>
* <span data-ttu-id="1ef46-194">**Webu Azure** -ještě nepodporujeme čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="1ef46-194">**Azure web site** - we don't support performance counters yet.</span></span> <span data-ttu-id="1ef46-195">Existuje několik metriky, které můžete získat jako standardní součást ovládacího panelu webu Azure.</span><span class="sxs-lookup"><span data-stu-id="1ef46-195">There are several metrics you can get as a standard part of the Azure web site control panel.</span></span>
* <span data-ttu-id="1ef46-196">**UNIX server** - [nainstalujte collectd](app-insights-java-collectd.md)</span><span class="sxs-lookup"><span data-stu-id="1ef46-196">**Unix server** - [Install collectd](app-insights-java-collectd.md)</span></span>

### <a name="to-display-more-performance-counters"></a><span data-ttu-id="1ef46-197">Chcete-li zobrazit další čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="1ef46-197">To display more performance counters</span></span>
* <span data-ttu-id="1ef46-198">První, [přidejte nový graf](app-insights-metrics-explorer.md) a zjistit, jestli čítač základní sady, které nabízíme.</span><span class="sxs-lookup"><span data-stu-id="1ef46-198">First, [add a new chart](app-insights-metrics-explorer.md) and see if the counter is in the basic set that we offer.</span></span>
* <span data-ttu-id="1ef46-199">Pokud ne, [přidat čítač do sady shromažďují modul čítače výkonu](app-insights-performance-counters.md).</span><span class="sxs-lookup"><span data-stu-id="1ef46-199">If not, [add the counter to the set collected by the performance counter module](app-insights-performance-counters.md).</span></span>
