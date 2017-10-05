---
title: "Nastavit výstrahy ve službě Azure Application Insights | Microsoft Docs"
description: "Upozorňování o pomalé odezvy, výjimky a další výkonu nebo použití změn ve vaší webové aplikaci."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: f8ebde72-f819-4ba5-afa2-31dbd49509a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: c8386ffc5d68260eeb75edf7efb77db1163dcef8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="set-alerts-in-application-insights"></a><span data-ttu-id="d3a12-103">Nastavit výstrahy ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="d3a12-103">Set Alerts in Application Insights</span></span>
<span data-ttu-id="d3a12-104">[Azure Application Insights] [ start] vás může upozornit na změny v metriky výkonu a využití ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d3a12-104">[Azure Application Insights][start] can alert you to changes in performance or usage metrics in your web app.</span></span> 

<span data-ttu-id="d3a12-105">Application Insights monitoruje vaše živé aplikace na [širokou škálu platformy] [ platforms] vám pomohou diagnostikovat problémy s výkonem a porozumět různým způsobům využití.</span><span class="sxs-lookup"><span data-stu-id="d3a12-105">Application Insights monitors your live app on a [wide variety of platforms][platforms] to help you diagnose performance issues and understand usage patterns.</span></span>

<span data-ttu-id="d3a12-106">Existují tři typy výstrah:</span><span class="sxs-lookup"><span data-stu-id="d3a12-106">There are three kinds of alerts:</span></span>

* <span data-ttu-id="d3a12-107">**Metriky výstrahy** zjistíte, kdy metriky protne prahovou hodnotu po nějakou dobu – například dobu odezvy, počty výjimky, využití procesoru nebo zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="d3a12-107">**Metric alerts** tell you when a metric crosses a threshold value for some period - such as response times, exception counts, CPU usage, or page views.</span></span> 
* <span data-ttu-id="d3a12-108">[**Webové testy** ] [ availability] zjistíte, pokud váš webový server je k dispozici na Internetu nebo odpovídá pomalu.</span><span class="sxs-lookup"><span data-stu-id="d3a12-108">[**Web tests**][availability] tell you when your site is unavailable on the internet, or responding slowly.</span></span> <span data-ttu-id="d3a12-109">[Další informace][availability].</span><span class="sxs-lookup"><span data-stu-id="d3a12-109">[Learn more][availability].</span></span>
* <span data-ttu-id="d3a12-110">[**Proaktivní diagnostiky** ](app-insights-proactive-diagnostics.md) jsou konfigurovány automaticky k upozornění na neobvyklé výkonu vzory.</span><span class="sxs-lookup"><span data-stu-id="d3a12-110">[**Proactive diagnostics**](app-insights-proactive-diagnostics.md) are configured automatically to notify you about unusual performance patterns.</span></span>

<span data-ttu-id="d3a12-111">Zaměříme na metriky výstrahy v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d3a12-111">We focus on metric alerts in this article.</span></span>

## <a name="set-a-metric-alert"></a><span data-ttu-id="d3a12-112">Nastavení metriky výstrahy</span><span class="sxs-lookup"><span data-stu-id="d3a12-112">Set a Metric alert</span></span>
<span data-ttu-id="d3a12-113">Otevřete okno pravidla výstrah a pak pomocí tlačítka Přidat.</span><span class="sxs-lookup"><span data-stu-id="d3a12-113">Open the Alert rules blade, and then use the add button.</span></span> 

![V okně pravidla výstrah vyberte Přidat výstrahy.](./media/app-insights-alerts/01-set-metric.png)

* <span data-ttu-id="d3a12-116">Nastavte prostředků než ostatní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d3a12-116">Set the resource before the other properties.</span></span> <span data-ttu-id="d3a12-117">**Vyberte prostředek, "(součásti)"** Pokud chcete nastavit výstrahy na metriky výkonu a využití.</span><span class="sxs-lookup"><span data-stu-id="d3a12-117">**Choose the "(components)" resource** if you want to set alerts on performance or usage metrics.</span></span>
* <span data-ttu-id="d3a12-118">Název, který přiřadíte výstrahy musí být jedinečný v rámci skupiny prostředků (ne jenom aplikace).</span><span class="sxs-lookup"><span data-stu-id="d3a12-118">The name that you give to the alert must be unique within the resource group (not just your application).</span></span>
* <span data-ttu-id="d3a12-119">Pečlivě si uvědomit a jednotky, ve kterých budete vyzváni k zadání prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d3a12-119">Be careful to note the units in which you're asked to enter the threshold value.</span></span>
* <span data-ttu-id="d3a12-120">Pokud zaškrtnete políčko "E-mailu vlastníky...", výstrahy se posílají e-mailem pro každého, kdo má přístup do této skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="d3a12-120">If you check the box "Email owners...", alerts are sent by email to everyone who has access to this resource group.</span></span> <span data-ttu-id="d3a12-121">Rozšířit tato sada osoby, přidejte je do [skupiny prostředků nebo předplatného](app-insights-resources-roles-access-control.md) (ne prostředků).</span><span class="sxs-lookup"><span data-stu-id="d3a12-121">To expand this set of people, add them to the [resource group or subscription](app-insights-resources-roles-access-control.md) (not the resource).</span></span>
* <span data-ttu-id="d3a12-122">Pokud zadáte "Další e-mailů", výstrahy se posílají pro tyto skupiny (jestli je zaškrtnuté políčko "e-mailu vlastníky...") nebo jednotlivce.</span><span class="sxs-lookup"><span data-stu-id="d3a12-122">If you specify "Additional emails", alerts are sent to those individuals or groups (whether or not you checked the "email owners..." box).</span></span> 
* <span data-ttu-id="d3a12-123">Nastavte [webhooku adresu](../monitoring-and-diagnostics/insights-webhooks-alerts.md) Pokud jste vytvořili webovou aplikaci, která reaguje na výstrahy.</span><span class="sxs-lookup"><span data-stu-id="d3a12-123">Set a [webhook address](../monitoring-and-diagnostics/insights-webhooks-alerts.md) if you have set up a web app that responds to alerts.</span></span> <span data-ttu-id="d3a12-124">Je volána při aktivaci výstrahy i při vyřešení.</span><span class="sxs-lookup"><span data-stu-id="d3a12-124">It is called both when the alert is Activated and when it is Resolved.</span></span> <span data-ttu-id="d3a12-125">(Ale Všimněte si, že se v současné době, parametry dotazu nejsou předána jako vlastnosti webhooku.)</span><span class="sxs-lookup"><span data-stu-id="d3a12-125">(But note that at present, query parameters are not passed through as webhook properties.)</span></span>
* <span data-ttu-id="d3a12-126">Můžete zakázat nebo povolit upozornění: zobrazení tlačítek v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="d3a12-126">You can Disable or Enable the alert: see the buttons at the top of the blade.</span></span>

<span data-ttu-id="d3a12-127">*Tlačítko Přidat oznámení se nezobrazí.*</span><span class="sxs-lookup"><span data-stu-id="d3a12-127">*I don't see the Add Alert button.*</span></span> 

* <span data-ttu-id="d3a12-128">Používáte účet organizace?</span><span class="sxs-lookup"><span data-stu-id="d3a12-128">Are you using an organizational account?</span></span> <span data-ttu-id="d3a12-129">Výstrahy můžete nastavit, pokud máte vlastníka nebo přispěvatele přístup k tomuto prostředku aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3a12-129">You can set alerts if you have owner or contributor access to this application resource.</span></span> <span data-ttu-id="d3a12-130">Podívejte se na v okně řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="d3a12-130">Take a look at the Access Control blade.</span></span> <span data-ttu-id="d3a12-131">[Další informace o řízení přístupu][roles].</span><span class="sxs-lookup"><span data-stu-id="d3a12-131">[Learn about access control][roles].</span></span>

> [!NOTE]
> <span data-ttu-id="d3a12-132">V okně výstrahy zjistíte, že je již sadu výstrah: [proaktivní diagnostiky](app-insights-proactive-failure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="d3a12-132">In the alerts blade, you see that there's already an alert set up: [Proactive Diagnostics](app-insights-proactive-failure-diagnostics.md).</span></span> <span data-ttu-id="d3a12-133">Automatické výstrahy monitoruje jeden konkrétní metriky, žádost míra selhání.</span><span class="sxs-lookup"><span data-stu-id="d3a12-133">The automatic alert monitors one particular metric, request failure rate.</span></span> <span data-ttu-id="d3a12-134">Pokud se rozhodnete zakázat proaktivní výstrahy, nemusíte nastavit vlastní upozornění na selhání rychlost požadavků.</span><span class="sxs-lookup"><span data-stu-id="d3a12-134">Unless you decide to disable the proactive alert, you don't need to set your own alert on request failure rate.</span></span> 
> 
> 

## <a name="see-your-alerts"></a><span data-ttu-id="d3a12-135">Zobrazit oznámení</span><span class="sxs-lookup"><span data-stu-id="d3a12-135">See your alerts</span></span>
<span data-ttu-id="d3a12-136">Zobrazí se e-mailu, když do stavu výstrahy změny mezi neaktivní a aktivní.</span><span class="sxs-lookup"><span data-stu-id="d3a12-136">You get an email when an alert changes state between inactive and active.</span></span> 

<span data-ttu-id="d3a12-137">Aktuální stav každé výstraze se zobrazí v okně pravidla výstrah.</span><span class="sxs-lookup"><span data-stu-id="d3a12-137">The current state of each alert is shown in the Alert rules blade.</span></span>

<span data-ttu-id="d3a12-138">Je souhrn poslední aktivitu v upozorněních rozevíracího seznamu:</span><span class="sxs-lookup"><span data-stu-id="d3a12-138">There's a summary of recent activity in the alerts drop-down:</span></span>

![Rozevírací seznam výstrah](./media/app-insights-alerts/010-alert-drop.png)

<span data-ttu-id="d3a12-140">Historie změn stavů, se v protokolu aktivit:</span><span class="sxs-lookup"><span data-stu-id="d3a12-140">The history of state changes is in the Activity Log:</span></span>

![V okně Přehled klikněte na tlačítko Nastavení, protokoly auditu](./media/app-insights-alerts/09-alerts.png)

## <a name="how-alerts-work"></a><span data-ttu-id="d3a12-142">Jak výstrahy fungují</span><span class="sxs-lookup"><span data-stu-id="d3a12-142">How alerts work</span></span>
* <span data-ttu-id="d3a12-143">Výstraha má tři stavy: "Nikdy neaktivoval", "Aktivované" a "Přeložit".</span><span class="sxs-lookup"><span data-stu-id="d3a12-143">An alert has three states: "Never activated", "Activated", and "Resolved."</span></span> <span data-ttu-id="d3a12-144">Aktivovaný znamená podmínku, kterou jste zadali byla nastavena hodnota true, pokud bylo naposled vyhodnoceno.</span><span class="sxs-lookup"><span data-stu-id="d3a12-144">Activated means the condition you specified was true, when it was last evaluated.</span></span>
* <span data-ttu-id="d3a12-145">Oznámení se vygeneruje, když výstrahu změny stavu.</span><span class="sxs-lookup"><span data-stu-id="d3a12-145">A notification is generated when an alert changes state.</span></span> <span data-ttu-id="d3a12-146">(Pokud podmínka upozornění již byla hodnota true, pokud jste vytvořili výstrahu, nemusí získáte oznámení dokud podmínku přejde false.)</span><span class="sxs-lookup"><span data-stu-id="d3a12-146">(If the alert condition was already true when you created the alert, you might not get a notification until the condition goes false.)</span></span>
* <span data-ttu-id="d3a12-147">Jednotlivá oznámení vygeneruje e-mailu v případě, že je zaškrtnuté políčko e-mailů, nebo zadat e-mailové adresy.</span><span class="sxs-lookup"><span data-stu-id="d3a12-147">Each notification generates an email if you checked the emails box, or provided email addresses.</span></span> <span data-ttu-id="d3a12-148">Můžete také zobrazit v rozevíracím seznamu oznámení.</span><span class="sxs-lookup"><span data-stu-id="d3a12-148">You can also look at the Notifications drop-down list.</span></span>
* <span data-ttu-id="d3a12-149">Výstraha je vyhodnocován pokaždé, když dorazí metriky, ale není jinak.</span><span class="sxs-lookup"><span data-stu-id="d3a12-149">An alert is evaluated each time a metric arrives, but not otherwise.</span></span>
* <span data-ttu-id="d3a12-150">Hodnocení agreguje metriku předchozí období a porovná je prahová hodnota, na zjištění nového stavu.</span><span class="sxs-lookup"><span data-stu-id="d3a12-150">The evaluation aggregates the metric over the preceding period, and then compares it to the threshold to determine the new state.</span></span>
* <span data-ttu-id="d3a12-151">Dobu, po kterou zvolíte, určuje interval, za které se shromažďují metriky.</span><span class="sxs-lookup"><span data-stu-id="d3a12-151">The period that you choose specifies the interval over which metrics are aggregated.</span></span> <span data-ttu-id="d3a12-152">Nemá vliv, jak často je vyhodnocován výstrahy: to závisí na četnosti příchodem metrik.</span><span class="sxs-lookup"><span data-stu-id="d3a12-152">It doesn't affect how often the alert is evaluated: that depends on the frequency of arrival of metrics.</span></span>
* <span data-ttu-id="d3a12-153">Pokud žádná data doručení pro konkrétní metriku nějakou dobu, mezeru má jiný účinky na výstrahy vyhodnocení a grafy v Průzkumníku metriky.</span><span class="sxs-lookup"><span data-stu-id="d3a12-153">If no data arrives for a particular metric for some time, the gap has different effects on alert evaluation and on the charts in metric explorer.</span></span> <span data-ttu-id="d3a12-154">V Průzkumníku metriky Pokud je vidět žádná data po dobu delší než interval vzorkování grafu, graf zobrazuje hodnotu 0.</span><span class="sxs-lookup"><span data-stu-id="d3a12-154">In metric explorer, if no data is seen for longer than the chart's sampling interval, the chart shows a value of 0.</span></span> <span data-ttu-id="d3a12-155">Ale výstrahy založené na stejné metriky není znovu vyhodnocena, a výstrah stavu zůstává beze změny.</span><span class="sxs-lookup"><span data-stu-id="d3a12-155">But an alert based on the same metric is not be reevaluated, and the alert's state remains unchanged.</span></span> 
  
    <span data-ttu-id="d3a12-156">Pokud data nakonec dorazí, grafu přejde zpět na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="d3a12-156">When data eventually arrives, the chart jumps back to a non-zero value.</span></span> <span data-ttu-id="d3a12-157">Vyhodnotí výstrahy na základě dat po dobu určenou je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d3a12-157">The alert evaluates based on the data available for the period you specified.</span></span> <span data-ttu-id="d3a12-158">Nový bod dat je pouze jeden, který je k dispozici v období, agregace je založen na právě na datových bodů.</span><span class="sxs-lookup"><span data-stu-id="d3a12-158">If the new data point is the only one available in the period, the aggregate is based just on that data point.</span></span>
* <span data-ttu-id="d3a12-159">Výstrahu můžete často blikat mezi stavy výstrah a v pořádku, i když nastavíte dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="d3a12-159">An alert can flicker frequently between alert and healthy states, even if you set a long period.</span></span> <span data-ttu-id="d3a12-160">To může dojít, pokud hodnota metriky bude umístěn kolem prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d3a12-160">This can happen if the metric value hovers around the threshold.</span></span> <span data-ttu-id="d3a12-161">Neexistuje žádné hystereze v prahové hodnotě: přechod výstraha se odehrává na stejnou hodnotu jako přechodu na v pořádku.</span><span class="sxs-lookup"><span data-stu-id="d3a12-161">There is no hysteresis in the threshold: the transition to alert happens at the same value as the transition to healthy.</span></span>

## <a name="what-are-good-alerts-to-set"></a><span data-ttu-id="d3a12-162">Jaké jsou dobrou výstrahy nastavit?</span><span class="sxs-lookup"><span data-stu-id="d3a12-162">What are good alerts to set?</span></span>
<span data-ttu-id="d3a12-163">To závisí na vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3a12-163">It depends on your application.</span></span> <span data-ttu-id="d3a12-164">Abyste mohli začít je nejlepší nastavit příliš mnoho metriky.</span><span class="sxs-lookup"><span data-stu-id="d3a12-164">To start with, it's best not to set too many metrics.</span></span> <span data-ttu-id="d3a12-165">Věnujte nějaký čas prohlížení vaší metriky grafy, když aplikace běží na podívat, jak se chová normálně.</span><span class="sxs-lookup"><span data-stu-id="d3a12-165">Spend some time looking at your metric charts while your app is running, to get a feel for how it behaves normally.</span></span> <span data-ttu-id="d3a12-166">Tento postup vám pomůže najít způsobů, jak zlepšit jeho výkon.</span><span class="sxs-lookup"><span data-stu-id="d3a12-166">This practice helps you find ways to improve its performance.</span></span> <span data-ttu-id="d3a12-167">Potom nastavte výstrahy říct, kdy metriky výlet za hranice normální zóny.</span><span class="sxs-lookup"><span data-stu-id="d3a12-167">Then set up alerts to tell you when the metrics go outside the normal zone.</span></span> 

<span data-ttu-id="d3a12-168">Oblíbené výstrahy patří:</span><span class="sxs-lookup"><span data-stu-id="d3a12-168">Popular alerts include:</span></span>

* <span data-ttu-id="d3a12-169">[Prohlížeč metriky][client], zejména prohlížeče **časů načtení stránky**, jsou vhodné pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3a12-169">[Browser metrics][client], especially Browser **page load times**, are good for web applications.</span></span> <span data-ttu-id="d3a12-170">Pokud stránka má mnoho skriptů, je vhodné vyhledat **výjimky prohlížeče**.</span><span class="sxs-lookup"><span data-stu-id="d3a12-170">If your page has many scripts, you should look for **browser exceptions**.</span></span> <span data-ttu-id="d3a12-171">Chcete-li získat tyto metriky a výstrahy, budete muset nastavit [monitorování webové stránky][client].</span><span class="sxs-lookup"><span data-stu-id="d3a12-171">In order to get these metrics and alerts, you have to set up [web page monitoring][client].</span></span>
* <span data-ttu-id="d3a12-172">**Doba odezvy serveru** pro webové aplikace na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="d3a12-172">**Server response time** for the server side of web applications.</span></span> <span data-ttu-id="d3a12-173">A také nastavení výstrah, dohlížet na tato metrika chcete zobrazit, pokud ho se liší podle nepřiměřeně vysoké požadavků: variace, může znamenat, vaše aplikace může být nedostatek prostředků.</span><span class="sxs-lookup"><span data-stu-id="d3a12-173">As well as setting up alerts, keep an eye on this metric to see if it varies disproportionately with high request rates: variation might indicate that your app is running out of resources.</span></span> 
* <span data-ttu-id="d3a12-174">**Výjimky serveru** – Pokud chcete vidět, budete muset provést některé [další nastavení](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="d3a12-174">**Server exceptions** - to see them, you have to do some [additional setup](app-insights-asp-net-exceptions.md).</span></span>

<span data-ttu-id="d3a12-175">Nezapomeňte, že [proaktivní selhání míra diagnostiky](app-insights-proactive-failure-diagnostics.md) automaticky monitorovat rychlost, jakou aplikace reaguje na požadavky s kódy selhání.</span><span class="sxs-lookup"><span data-stu-id="d3a12-175">Don't forget that [proactive failure rate diagnostics](app-insights-proactive-failure-diagnostics.md) automatically monitor the rate at which your app responds to requests with failure codes.</span></span> 

## <a name="automation"></a><span data-ttu-id="d3a12-176">Automation</span><span class="sxs-lookup"><span data-stu-id="d3a12-176">Automation</span></span>
* [<span data-ttu-id="d3a12-177">Použití Powershellu k automatizaci nastavení výstrah</span><span class="sxs-lookup"><span data-stu-id="d3a12-177">Use PowerShell to automate setting up alerts</span></span>](app-insights-powershell-alerts.md)
* [<span data-ttu-id="d3a12-178">Pomocí webhooků automatizovat reagovat na výstrahy</span><span class="sxs-lookup"><span data-stu-id="d3a12-178">Use webhooks to automate responding to alerts</span></span>](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="video"></a><span data-ttu-id="d3a12-179">Video</span><span class="sxs-lookup"><span data-stu-id="d3a12-179">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="see-also"></a><span data-ttu-id="d3a12-180">Viz také</span><span class="sxs-lookup"><span data-stu-id="d3a12-180">See also</span></span>
* [<span data-ttu-id="d3a12-181">Testy dostupnosti webu</span><span class="sxs-lookup"><span data-stu-id="d3a12-181">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md)
* [<span data-ttu-id="d3a12-182">Automatizovat nastavení výstrah</span><span class="sxs-lookup"><span data-stu-id="d3a12-182">Automate setting up alerts</span></span>](app-insights-powershell-alerts.md)
* [<span data-ttu-id="d3a12-183">Proaktivní diagnostiky</span><span class="sxs-lookup"><span data-stu-id="d3a12-183">Proactive diagnostics</span></span>](app-insights-proactive-diagnostics.md) 

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[platforms]: app-insights-platforms.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

