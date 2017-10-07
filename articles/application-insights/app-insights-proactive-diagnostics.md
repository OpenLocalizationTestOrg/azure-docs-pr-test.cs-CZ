---
title: "aaaSmart detekce ve službě Azure Application Insights | Microsoft Docs"
description: "Application Insights provede hloubkovou analýzu automatické vaší aplikace telemetrie a zobrazí upozornění na potenciální problémy."
services: application-insights
documentationcenter: windows
author: rakefetj
manager: carmonm
ms.assetid: 2eeb4a35-c7a1-49f7-9b68-4f4b860938b2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: bwren
ms.openlocfilehash: f794476088fc69154eda2077b7a5cdc769fab3a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="smart-detection-in-application-insights"></a><span data-ttu-id="c2daa-103">Inteligentní detekce ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="c2daa-103">Smart Detection in Application Insights</span></span>
 <span data-ttu-id="c2daa-104">Inteligentní detekce automaticky zobrazí upozornění na potenciální problémy s výkonem ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c2daa-104">Smart Detection automatically warns you of potential performance problems in your web application.</span></span> <span data-ttu-id="c2daa-105">Provede proaktivní analýzu hello telemetrii, kterou vaše aplikace odesílá příliš[Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c2daa-105">It performs proactive analysis of hello telemetry that your app sends too[Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="c2daa-106">Pokud je nečekané zvýšení sazby selhání nebo neobvyklé vzory výkon klienta nebo serveru, zobrazí se upozornění.</span><span class="sxs-lookup"><span data-stu-id="c2daa-106">If there is a sudden rise in failure rates, or abnormal patterns in client or server performance, you get an alert.</span></span> <span data-ttu-id="c2daa-107">Tato funkce vyžaduje žádná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c2daa-107">This feature needs no configuration.</span></span> <span data-ttu-id="c2daa-108">Pokud vaše aplikace odešle dost telemetrických dat funguje.</span><span class="sxs-lookup"><span data-stu-id="c2daa-108">It operates if your application sends enough telemetry.</span></span>

<span data-ttu-id="c2daa-109">Inteligentní detekce výstrahy můžete přistupovat z hello e-mailů, které se zobrazí a z okna Inteligentní detekce hello.</span><span class="sxs-lookup"><span data-stu-id="c2daa-109">You can access Smart Detection alerts both from hello emails you receive, and from hello Smart Detection blade.</span></span>

## <a name="review-your-smart-detections"></a><span data-ttu-id="c2daa-110">Zkontrolujte vaše Inteligentní detekce</span><span class="sxs-lookup"><span data-stu-id="c2daa-110">Review your Smart Detections</span></span>
<span data-ttu-id="c2daa-111">Může zjistit detekce dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="c2daa-111">You can discover detections in two ways:</span></span>

* <span data-ttu-id="c2daa-112">**Obdržíte e-mailu** z Application Insights.</span><span class="sxs-lookup"><span data-stu-id="c2daa-112">**You receive an email** from Application Insights.</span></span> <span data-ttu-id="c2daa-113">Tady je typický příklad:</span><span class="sxs-lookup"><span data-stu-id="c2daa-113">Here's a typical example:</span></span>
  
    ![E-mailové výstrahy](./media/app-insights-proactive-diagnostics/03.png)
  
    <span data-ttu-id="c2daa-115">Klikněte na tlačítko hello big tlačítko tooopen podrobněji hello portálu.</span><span class="sxs-lookup"><span data-stu-id="c2daa-115">Click hello big button tooopen more detail in hello portal.</span></span>
* <span data-ttu-id="c2daa-116">**dlaždice Inteligentní detekce Hello** na přehled vaší aplikace se zobrazí okno počet poslední výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c2daa-116">**hello Smart Detection tile** on your app's overview blade shows a count of recent alerts.</span></span> <span data-ttu-id="c2daa-117">Klikněte na dlaždici toosee hello seznam nedávné.</span><span class="sxs-lookup"><span data-stu-id="c2daa-117">Click hello tile toosee a list of recent alerts.</span></span>

![Zobrazení posledních detekce](./media/app-insights-proactive-diagnostics/04.png)

<span data-ttu-id="c2daa-119">Vyberte výstrahy toosee její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c2daa-119">Select an alert toosee its details.</span></span>

## <a name="what-problems-are-detected"></a><span data-ttu-id="c2daa-120">Jaké problémy jsou rozpoznána?</span><span class="sxs-lookup"><span data-stu-id="c2daa-120">What problems are detected?</span></span>
<span data-ttu-id="c2daa-121">Existují tři druhy zjišťování:</span><span class="sxs-lookup"><span data-stu-id="c2daa-121">There are three kinds of detection:</span></span>

* <span data-ttu-id="c2daa-122">[Inteligentní detekce - selhání anomálií](app-insights-proactive-failure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="c2daa-122">[Smart detection - Failure Anomalies](app-insights-proactive-failure-diagnostics.md).</span></span> <span data-ttu-id="c2daa-123">Používáme machine learning tooset hello očekávaný počet neúspěšných požadavků pro aplikace, korelace s zatížení a dalších faktorů.</span><span class="sxs-lookup"><span data-stu-id="c2daa-123">We use machine learning tooset hello expected rate of failed requests for your app, correlating with load and other factors.</span></span> <span data-ttu-id="c2daa-124">Pokud je míra selhání hello ocitne mimo očekávaný obálky hello, můžeme zasílat výstrahu.</span><span class="sxs-lookup"><span data-stu-id="c2daa-124">If hello failure rate goes outside hello expected envelope, we send an alert.</span></span>
* <span data-ttu-id="c2daa-125">[Inteligentní detekce - anomálie výkonu](app-insights-proactive-performance-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="c2daa-125">[Smart detection - Performance Anomalies](app-insights-proactive-performance-diagnostics.md).</span></span> <span data-ttu-id="c2daa-126">Pokud doba odezvy trvání operace nebo závislostí je zpomalení porovnání toohistorical směrného plánu, nebo pokud jsme identifikovat neobvyklé vzor doba odezvy nebo čas načítání stránky zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="c2daa-126">You get notifications if response time of an operation or dependency duration is slowing down compared toohistorical baseline or if we identify an anomalous pattern in response time or page load time.</span></span>   
* <span data-ttu-id="c2daa-127">[Inteligentní detekce - Azure Cloud Service problémy](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/).</span><span class="sxs-lookup"><span data-stu-id="c2daa-127">[Smart detection - Azure Cloud Service issues](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/).</span></span> <span data-ttu-id="c2daa-128">Zobrazí výstrahy, pokud vaše aplikace je hostovaná v Azure Cloud Services a instanci role má selhání spuštění, často recyklace nebo dojde k chybě modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="c2daa-128">You get alerts if your app is hosted in Azure Cloud Services and a role instance has startup failures, frequent recycling, or runtime crashes.</span></span>

<span data-ttu-id="c2daa-129">(hello nápovědy v jednotlivých oznámení odkazů můžete toohello související články.)</span><span class="sxs-lookup"><span data-stu-id="c2daa-129">(hello help links in each notification take you toohello relevant articles.)</span></span>

## <a name="video"></a><span data-ttu-id="c2daa-130">Video</span><span class="sxs-lookup"><span data-stu-id="c2daa-130">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="c2daa-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2daa-131">Next steps</span></span>
<span data-ttu-id="c2daa-132">Tyto diagnostické nástroje můžete zkontrolovat hello telemetrie z vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="c2daa-132">These diagnostic tools help you inspect hello telemetry from your app:</span></span>

* [<span data-ttu-id="c2daa-133">Metriky explorer</span><span class="sxs-lookup"><span data-stu-id="c2daa-133">Metric explorer</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="c2daa-134">Průzkumník služby Search</span><span class="sxs-lookup"><span data-stu-id="c2daa-134">Search explorer</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="c2daa-135">Analýza - účinný dotazovací jazyk</span><span class="sxs-lookup"><span data-stu-id="c2daa-135">Analytics - powerful query language</span></span>](app-insights-analytics-tour.md)

<span data-ttu-id="c2daa-136">Inteligentní detekce je zcela automatické.</span><span class="sxs-lookup"><span data-stu-id="c2daa-136">Smart Detection is completely automatic.</span></span> <span data-ttu-id="c2daa-137">Ale možná byste chtěli tooset si některé další výstrahy?</span><span class="sxs-lookup"><span data-stu-id="c2daa-137">But maybe you'd like tooset up some more alerts?</span></span>

* [<span data-ttu-id="c2daa-138">Ručně konfigurované metriky výstrahy</span><span class="sxs-lookup"><span data-stu-id="c2daa-138">Manually configured metric alerts</span></span>](app-insights-alerts.md)
* [<span data-ttu-id="c2daa-139">Testy dostupnosti webu</span><span class="sxs-lookup"><span data-stu-id="c2daa-139">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md) 

