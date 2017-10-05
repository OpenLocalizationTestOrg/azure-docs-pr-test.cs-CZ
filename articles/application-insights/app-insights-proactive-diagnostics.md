---
title: "Inteligentní detekce ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: f203b2a532ea721d9797c67a4750896e3ab2b9f7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="smart-detection-in-application-insights"></a><span data-ttu-id="c9766-103">Inteligentní detekce ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="c9766-103">Smart Detection in Application Insights</span></span>
 <span data-ttu-id="c9766-104">Inteligentní detekce automaticky zobrazí upozornění na potenciální problémy s výkonem ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c9766-104">Smart Detection automatically warns you of potential performance problems in your web application.</span></span> <span data-ttu-id="c9766-105">Provede proaktivní analýzu telemetrii, kterou vaše aplikace odesílá do [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c9766-105">It performs proactive analysis of the telemetry that your app sends to [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="c9766-106">Pokud je nečekané zvýšení sazby selhání nebo neobvyklé vzory výkon klienta nebo serveru, zobrazí se upozornění.</span><span class="sxs-lookup"><span data-stu-id="c9766-106">If there is a sudden rise in failure rates, or abnormal patterns in client or server performance, you get an alert.</span></span> <span data-ttu-id="c9766-107">Tato funkce vyžaduje žádná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c9766-107">This feature needs no configuration.</span></span> <span data-ttu-id="c9766-108">Pokud vaše aplikace odešle dost telemetrických dat funguje.</span><span class="sxs-lookup"><span data-stu-id="c9766-108">It operates if your application sends enough telemetry.</span></span>

<span data-ttu-id="c9766-109">Inteligentní detekce výstrahy můžete přistupovat z e-mailů, které se zobrazí a z okna Inteligentní detekce.</span><span class="sxs-lookup"><span data-stu-id="c9766-109">You can access Smart Detection alerts both from the emails you receive, and from the Smart Detection blade.</span></span>

## <a name="review-your-smart-detections"></a><span data-ttu-id="c9766-110">Zkontrolujte vaše Inteligentní detekce</span><span class="sxs-lookup"><span data-stu-id="c9766-110">Review your Smart Detections</span></span>
<span data-ttu-id="c9766-111">Může zjistit detekce dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="c9766-111">You can discover detections in two ways:</span></span>

* <span data-ttu-id="c9766-112">**Obdržíte e-mailu** z Application Insights.</span><span class="sxs-lookup"><span data-stu-id="c9766-112">**You receive an email** from Application Insights.</span></span> <span data-ttu-id="c9766-113">Tady je typický příklad:</span><span class="sxs-lookup"><span data-stu-id="c9766-113">Here's a typical example:</span></span>
  
    ![E-mailové výstrahy](./media/app-insights-proactive-diagnostics/03.png)
  
    <span data-ttu-id="c9766-115">Klikněte na tlačítko big otevřete podrobněji na portálu.</span><span class="sxs-lookup"><span data-stu-id="c9766-115">Click the big button to open more detail in the portal.</span></span>
* <span data-ttu-id="c9766-116">**Na dlaždici Inteligentní detekce** na přehled vaší aplikace se zobrazí okno počet poslední výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c9766-116">**The Smart Detection tile** on your app's overview blade shows a count of recent alerts.</span></span> <span data-ttu-id="c9766-117">Klikněte na dlaždici zobrazíte seznam poslední výstrahy.</span><span class="sxs-lookup"><span data-stu-id="c9766-117">Click the tile to see a list of recent alerts.</span></span>

![Zobrazení posledních detekce](./media/app-insights-proactive-diagnostics/04.png)

<span data-ttu-id="c9766-119">Vyberte výstrahu zobrazíte její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c9766-119">Select an alert to see its details.</span></span>

## <a name="what-problems-are-detected"></a><span data-ttu-id="c9766-120">Jaké problémy jsou rozpoznána?</span><span class="sxs-lookup"><span data-stu-id="c9766-120">What problems are detected?</span></span>
<span data-ttu-id="c9766-121">Existují tři druhy zjišťování:</span><span class="sxs-lookup"><span data-stu-id="c9766-121">There are three kinds of detection:</span></span>

* <span data-ttu-id="c9766-122">[Inteligentní detekce - selhání anomálií](app-insights-proactive-failure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="c9766-122">[Smart detection - Failure Anomalies](app-insights-proactive-failure-diagnostics.md).</span></span> <span data-ttu-id="c9766-123">Používáme strojového učení nastavit očekávaný počet neúspěšných požadavků pro vaši aplikaci korelace s zatížení a dalších faktorů.</span><span class="sxs-lookup"><span data-stu-id="c9766-123">We use machine learning to set the expected rate of failed requests for your app, correlating with load and other factors.</span></span> <span data-ttu-id="c9766-124">Pokud je míra selhání proběhne mimo očekávaný obálky, můžeme zasílat výstrahu.</span><span class="sxs-lookup"><span data-stu-id="c9766-124">If the failure rate goes outside the expected envelope, we send an alert.</span></span>
* <span data-ttu-id="c9766-125">[Inteligentní detekce - anomálie výkonu](app-insights-proactive-performance-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="c9766-125">[Smart detection - Performance Anomalies](app-insights-proactive-performance-diagnostics.md).</span></span> <span data-ttu-id="c9766-126">Pokud doba odezvy trvání operace nebo závislostí je zpomalení nástroje porovnání se směrným plánem historických nebo pokud jsme identifikovat neobvyklé vzor doba odezvy nebo čas načítání stránky zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="c9766-126">You get notifications if response time of an operation or dependency duration is slowing down compared to historical baseline or if we identify an anomalous pattern in response time or page load time.</span></span>   
* <span data-ttu-id="c9766-127">[Inteligentní detekce - Azure Cloud Service problémy](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/).</span><span class="sxs-lookup"><span data-stu-id="c9766-127">[Smart detection - Azure Cloud Service issues](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/).</span></span> <span data-ttu-id="c9766-128">Zobrazí výstrahy, pokud vaše aplikace je hostovaná v Azure Cloud Services a instanci role má selhání spuštění, často recyklace nebo dojde k chybě modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="c9766-128">You get alerts if your app is hosted in Azure Cloud Services and a role instance has startup failures, frequent recycling, or runtime crashes.</span></span>

<span data-ttu-id="c9766-129">(Nápovědu v každém upozornění odkazů můžete na odpovídající články.)</span><span class="sxs-lookup"><span data-stu-id="c9766-129">(The help links in each notification take you to the relevant articles.)</span></span>

## <a name="video"></a><span data-ttu-id="c9766-130">Video</span><span class="sxs-lookup"><span data-stu-id="c9766-130">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="c9766-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9766-131">Next steps</span></span>
<span data-ttu-id="c9766-132">Tyto diagnostické nástroje můžete zkontrolovat telemetrie z vaší aplikace:</span><span class="sxs-lookup"><span data-stu-id="c9766-132">These diagnostic tools help you inspect the telemetry from your app:</span></span>

* [<span data-ttu-id="c9766-133">Metriky explorer</span><span class="sxs-lookup"><span data-stu-id="c9766-133">Metric explorer</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="c9766-134">Průzkumník služby Search</span><span class="sxs-lookup"><span data-stu-id="c9766-134">Search explorer</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="c9766-135">Analýza - účinný dotazovací jazyk</span><span class="sxs-lookup"><span data-stu-id="c9766-135">Analytics - powerful query language</span></span>](app-insights-analytics-tour.md)

<span data-ttu-id="c9766-136">Inteligentní detekce je zcela automatické.</span><span class="sxs-lookup"><span data-stu-id="c9766-136">Smart Detection is completely automatic.</span></span> <span data-ttu-id="c9766-137">Ale možná chcete nastavit některé další výstrahy?</span><span class="sxs-lookup"><span data-stu-id="c9766-137">But maybe you'd like to set up some more alerts?</span></span>

* [<span data-ttu-id="c9766-138">Ručně konfigurované metriky výstrahy</span><span class="sxs-lookup"><span data-stu-id="c9766-138">Manually configured metric alerts</span></span>](app-insights-alerts.md)
* [<span data-ttu-id="c9766-139">Testy dostupnosti webu</span><span class="sxs-lookup"><span data-stu-id="c9766-139">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md) 

