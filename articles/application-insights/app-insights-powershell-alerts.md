---
title: "výstrahy tooset aaaUse prostředí Powershell ve službě Application Insights | Microsoft Docs"
description: "Automatické konfiguraci služby Application Insights tooget e-mailů o metriky změny."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 05d6a9e0-77a2-4a35-9052-a7768d23a196
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: bwren
ms.openlocfilehash: d68e5f9511bb4015f59175724bc1a4a04ecf43e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooset-alerts-in-application-insights"></a><span data-ttu-id="3d6b9-103">Pomocí prostředí PowerShell tooset výstrahy ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="3d6b9-103">Use PowerShell tooset alerts in Application Insights</span></span>
<span data-ttu-id="3d6b9-104">Můžete automatizovat konfiguraci hello [výstrahy](app-insights-alerts.md) v [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3d6b9-104">You can automate hello configuration of [alerts](app-insights-alerts.md) in [Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="3d6b9-105">Kromě toho můžete [nastavit webhooky tooautomate upozornění tooan odpovědi](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="3d6b9-105">In addition, you can [set webhooks tooautomate your response tooan alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

> [!NOTE]
> <span data-ttu-id="3d6b9-106">Pokud chcete toocreate prostředky a výstrahy v hello stejný čas, zvažte [pomocí šablony Azure Resource Manager](app-insights-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="3d6b9-106">If you want toocreate resources and alerts at hello same time, consider [using an Azure Resource Manager template](app-insights-powershell.md).</span></span>
>
>

## <a name="one-time-setup"></a><span data-ttu-id="3d6b9-107">Jednorázové instalace</span><span class="sxs-lookup"><span data-stu-id="3d6b9-107">One-time setup</span></span>
<span data-ttu-id="3d6b9-108">Pokud jste nepoužili prostředí PowerShell s předplatným Azure před:</span><span class="sxs-lookup"><span data-stu-id="3d6b9-108">If you haven't used PowerShell with your Azure subscription before:</span></span>

<span data-ttu-id="3d6b9-109">Nainstalujte modul Azure Powershell hello na hello počítači, kde se má toorun hello skripty.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-109">Install hello Azure Powershell module on hello machine where you want toorun hello scripts.</span></span>

* <span data-ttu-id="3d6b9-110">Nainstalujte [instalačního programu webové platformy (verze 5 nebo novější)](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d6b9-110">Install [Microsoft Web Platform Installer (v5 or higher)](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="3d6b9-111">Použít tooinstall Microsoft Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="3d6b9-111">Use it tooinstall Microsoft Azure Powershell</span></span>

## <a name="connect-tooazure"></a><span data-ttu-id="3d6b9-112">Připojit tooAzure</span><span class="sxs-lookup"><span data-stu-id="3d6b9-112">Connect tooAzure</span></span>
<span data-ttu-id="3d6b9-113">Otevřete prostředí Azure PowerShell a [připojení odběru tooyour](/powershell/azure/overview):</span><span class="sxs-lookup"><span data-stu-id="3d6b9-113">Start Azure PowerShell and [connect tooyour subscription](/powershell/azure/overview):</span></span>

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a><span data-ttu-id="3d6b9-114">Získání výstrahy</span><span class="sxs-lookup"><span data-stu-id="3d6b9-114">Get alerts</span></span>
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a><span data-ttu-id="3d6b9-115">Přidání oznámení</span><span class="sxs-lookup"><span data-stu-id="3d6b9-115">Add alert</span></span>
    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US" // must be East US at present
     -RuleType Metric



## <a name="example-1"></a><span data-ttu-id="3d6b9-116">Příklad 1</span><span class="sxs-lookup"><span data-stu-id="3d6b9-116">Example 1</span></span>
<span data-ttu-id="3d6b9-117">Poslat mi e-mail, pokud hello server odpovědi tooHTTP požadavků, která je průměrem více než 5 minut, je nižší než 1 sekunda.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-117">Email me if hello server's response tooHTTP requests, averaged over 5 minutes, is slower than 1 second.</span></span> <span data-ttu-id="3d6b9-118">Moje prostředek Application Insights se nazývá IceCreamWebApp a je ve skupině prostředků společnosti Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-118">My Application Insights resource is called IceCreamWebApp, and it is in resource group Fabrikam.</span></span> <span data-ttu-id="3d6b9-119">Jsem hello vlastník hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-119">I am hello owner of hello Azure subscription.</span></span>

<span data-ttu-id="3d6b9-120">Hello GUID je ID předplatného hello (ne hello klíč instrumentace aplikace hello).</span><span class="sxs-lookup"><span data-stu-id="3d6b9-120">hello GUID is hello subscription ID (not hello instrumentation key of hello application).</span></span>

    Add-AlertRule -Name "slow responses" `
     -Description "email me if hello server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a><span data-ttu-id="3d6b9-121">Příklad 2</span><span class="sxs-lookup"><span data-stu-id="3d6b9-121">Example 2</span></span>
<span data-ttu-id="3d6b9-122">Je nutné aplikaci, ve které používám [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) tooreport metriky s názvem "salesPerHour."</span><span class="sxs-lookup"><span data-stu-id="3d6b9-122">I have an application in which I use [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) tooreport a metric named "salesPerHour."</span></span> <span data-ttu-id="3d6b9-123">Odešlete e-mail toomy kolegy Pokud "salesPerHour" klesne pod 100, průměr po dobu 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-123">Send an email toomy colleagues if "salesPerHour" drops below 100, averaged over 24 hours.</span></span>

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

<span data-ttu-id="3d6b9-124">Hello stejného pravidla lze použít pro metriku hello hlášené pomocí hello [měření parametr](app-insights-api-custom-events-metrics.md#properties) jiné sledování volání například TrackEvent nebo trackPageView.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-124">hello same rule can be used for hello metric reported by using hello [measurement parameter](app-insights-api-custom-events-metrics.md#properties) of another tracking call such as TrackEvent or trackPageView.</span></span>

## <a name="metric-names"></a><span data-ttu-id="3d6b9-125">Metriky názvy</span><span class="sxs-lookup"><span data-stu-id="3d6b9-125">Metric names</span></span>
| <span data-ttu-id="3d6b9-126">Název metriky</span><span class="sxs-lookup"><span data-stu-id="3d6b9-126">Metric name</span></span> | <span data-ttu-id="3d6b9-127">Název obrazovky</span><span class="sxs-lookup"><span data-stu-id="3d6b9-127">Screen name</span></span> | <span data-ttu-id="3d6b9-128">Popis</span><span class="sxs-lookup"><span data-stu-id="3d6b9-128">Description</span></span> |
| --- | --- | --- |
| `basicExceptionBrowser.count` |<span data-ttu-id="3d6b9-129">Výjimky prohlížečů</span><span class="sxs-lookup"><span data-stu-id="3d6b9-129">Browser exceptions</span></span> |<span data-ttu-id="3d6b9-130">Počet nezachycená výjimky vydané v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-130">Count of uncaught exceptions thrown in hello browser.</span></span> |
| `basicExceptionServer.count` |<span data-ttu-id="3d6b9-131">Výjimky serveru</span><span class="sxs-lookup"><span data-stu-id="3d6b9-131">Server exceptions</span></span> |<span data-ttu-id="3d6b9-132">Počet neošetřené výjimky vyvolané aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3d6b9-132">Count of unhandled exceptions thrown by hello app</span></span> |
| `clientPerformance.clientProcess.value` |<span data-ttu-id="3d6b9-133">Doba zpracování klienta</span><span class="sxs-lookup"><span data-stu-id="3d6b9-133">Client processing time</span></span> |<span data-ttu-id="3d6b9-134">Doba mezi přijetí hello posledního bajtu dokumentu, dokud je načtena hello DOM.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-134">Time between receiving hello last byte of a document until hello DOM is loaded.</span></span> <span data-ttu-id="3d6b9-135">Asynchronních připojení může být stále aktivní.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-135">Async requests may still be processing.</span></span> |
| `clientPerformance.networkConnection.value` |<span data-ttu-id="3d6b9-136">Doba připojení sítě zatížení stránky</span><span class="sxs-lookup"><span data-stu-id="3d6b9-136">Page load network connect time</span></span> |<span data-ttu-id="3d6b9-137">Čas hello prohlížeče trvá tooconnect toohello sítě.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-137">Time hello browser takes tooconnect toohello network.</span></span> <span data-ttu-id="3d6b9-138">Může být 0, pokud do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-138">Can be 0 if cached.</span></span> |
| `clientPerformance.receiveRequest.value` |<span data-ttu-id="3d6b9-139">Přijetí doba odezvy</span><span class="sxs-lookup"><span data-stu-id="3d6b9-139">Receiving response time</span></span> |<span data-ttu-id="3d6b9-140">Doba mezi prohlížeče odeslání žádosti o toostarting tooreceive odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-140">Time between browser sending request toostarting tooreceive response.</span></span> |
| `clientPerformance.sendRequest.value` |<span data-ttu-id="3d6b9-141">Odeslat doba požadavku</span><span class="sxs-lookup"><span data-stu-id="3d6b9-141">Send request time</span></span> |<span data-ttu-id="3d6b9-142">Doba, kterou požadavek toosend prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-142">Time taken by browser toosend request.</span></span> |
| `clientPerformance.total.value` |<span data-ttu-id="3d6b9-143">Čas načítání stránky prohlížeče</span><span class="sxs-lookup"><span data-stu-id="3d6b9-143">Browser page load time</span></span> |<span data-ttu-id="3d6b9-144">Čas od požadavku uživatele, dokud se načtou DOM, předlohy se styly, skripty a obrázků.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-144">Time from user request until DOM, stylesheets, scripts and images are loaded.</span></span> |
| `performanceCounter.available_bytes.value` |<span data-ttu-id="3d6b9-145">Dostupná paměť</span><span class="sxs-lookup"><span data-stu-id="3d6b9-145">Available memory</span></span> |<span data-ttu-id="3d6b9-146">Fyzické paměti k dispozici pro zpracování nebo pro použití systémem.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-146">Physical memory immediately available for a process or for system use.</span></span> |
| `performanceCounter.io_data_bytes_per_sec.value` |<span data-ttu-id="3d6b9-147">Rychlost zpracování vstupně-výstupní operace</span><span class="sxs-lookup"><span data-stu-id="3d6b9-147">Process IO Rate</span></span> |<span data-ttu-id="3d6b9-148">Celkový počet bajtů na druhý pro čtení a napsané toofiles, sítě a zařízení.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-148">Total bytes per second read and written toofiles, network and devices.</span></span> |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |<span data-ttu-id="3d6b9-149">míra výjimky</span><span class="sxs-lookup"><span data-stu-id="3d6b9-149">exception rate</span></span> |<span data-ttu-id="3d6b9-150">Výjimek vyvolaných za sekundu.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-150">Exceptions thrown per second.</span></span> |
| `performanceCounter.percentage_processor_time.value` |<span data-ttu-id="3d6b9-151">Proces procesoru</span><span class="sxs-lookup"><span data-stu-id="3d6b9-151">Process CPU</span></span> |<span data-ttu-id="3d6b9-152">Hello procentuální hodnotu uplynulého času podprocesy procesu používané hello procesoru tooexecution pokyny pro proces aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-152">hello percentage of elapsed time of all process threads used by hello processor tooexecution instructions for hello applications process.</span></span> |
| `performanceCounter.percentage_processor_total.value` |<span data-ttu-id="3d6b9-153">Čas procesoru</span><span class="sxs-lookup"><span data-stu-id="3d6b9-153">Processor time</span></span> |<span data-ttu-id="3d6b9-154">Hello procento času, který hello procesor stráví v jiných než nečinných vláken.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-154">hello percentage of time that hello processor spends in non-Idle threads.</span></span> |
| `performanceCounter.process_private_bytes.value` |<span data-ttu-id="3d6b9-155">Proces nesdílených bajtů</span><span class="sxs-lookup"><span data-stu-id="3d6b9-155">Process private bytes</span></span> |<span data-ttu-id="3d6b9-156">Paměť výhradně přiřazená toohello sledovat procesy aplikace.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-156">Memory exclusively assigned toohello monitored application's processes.</span></span> |
| `performanceCounter.request_execution_time.value` |<span data-ttu-id="3d6b9-157">Čas provádění požadavku ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3d6b9-157">ASP.NET request execution time</span></span> |<span data-ttu-id="3d6b9-158">Čas provedení posledního požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-158">Execution time of hello most recent request.</span></span> |
| `performanceCounter.requests_in_application_queue.value` |<span data-ttu-id="3d6b9-159">ASP.NET požadavků ve frontě provádění</span><span class="sxs-lookup"><span data-stu-id="3d6b9-159">ASP.NET requests in execution queue</span></span> |<span data-ttu-id="3d6b9-160">Délka fronty požadavků aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-160">Length of hello application request queue.</span></span> |
| `performanceCounter.requests_per_sec.value` |<span data-ttu-id="3d6b9-161">Rychlost požadavků ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3d6b9-161">ASP.NET request rate</span></span> |<span data-ttu-id="3d6b9-162">Rychlost, jakou všechny požadavky aplikace toohello za sekundu z technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-162">Rate of all requests toohello application per second from ASP.NET.</span></span> |
| `remoteDependencyFailed.durationMetric.count` |<span data-ttu-id="3d6b9-163">Selhání závislostí</span><span class="sxs-lookup"><span data-stu-id="3d6b9-163">Dependency failures</span></span> |<span data-ttu-id="3d6b9-164">Počet selhání volání zdrojů tooexternal hello serverových aplikací.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-164">Count of failed calls made by hello server application tooexternal resources.</span></span> |
| `request.duration` |<span data-ttu-id="3d6b9-165">Doba odezvy serveru</span><span class="sxs-lookup"><span data-stu-id="3d6b9-165">Server response time</span></span> |<span data-ttu-id="3d6b9-166">Doba mezi přijetí požadavku HTTP a dokončením odeslání odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-166">Time between receiving an HTTP request and finishing sending hello response.</span></span> |
| `request.rate` |<span data-ttu-id="3d6b9-167">Rychlost požadavků</span><span class="sxs-lookup"><span data-stu-id="3d6b9-167">Request rate</span></span> |<span data-ttu-id="3d6b9-168">Rychlost, jakou všechny požadavky aplikace toohello za sekundu.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-168">Rate of all requests toohello application per second.</span></span> |
| `requestFailed.count` |<span data-ttu-id="3d6b9-169">Neúspěšné požadavky</span><span class="sxs-lookup"><span data-stu-id="3d6b9-169">Failed requests</span></span> |<span data-ttu-id="3d6b9-170">Počet HTTP požadavků, které kód odpovědi > = 400</span><span class="sxs-lookup"><span data-stu-id="3d6b9-170">Count of HTTP requests that resulted in a response code >= 400</span></span> |
| `view.count` |<span data-ttu-id="3d6b9-171">Zobrazení stránky</span><span class="sxs-lookup"><span data-stu-id="3d6b9-171">Page views</span></span> |<span data-ttu-id="3d6b9-172">Počet žádostí uživatele klienta pro webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-172">Count of client user requests for a web page.</span></span> <span data-ttu-id="3d6b9-173">Syntetické provoz odfiltrována.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-173">Synthetic traffic is filtered out.</span></span> |
| <span data-ttu-id="3d6b9-174">{váš vlastní název metriky}</span><span class="sxs-lookup"><span data-stu-id="3d6b9-174">{your custom metric name}</span></span> |<span data-ttu-id="3d6b9-175">{Název metriky}</span><span class="sxs-lookup"><span data-stu-id="3d6b9-175">{Your metric name}</span></span> |<span data-ttu-id="3d6b9-176">Metriky hodnota hlášené [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) nebo v hello [parametr měření volání sledování](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="3d6b9-176">Your metric value reported by [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) or in hello [measurements parameter of a tracking call](app-insights-api-custom-events-metrics.md#properties).</span></span> |

<span data-ttu-id="3d6b9-177">metriky Hello jsou odesílány moduly různých telemetrie:</span><span class="sxs-lookup"><span data-stu-id="3d6b9-177">hello metrics are sent by different telemetry modules:</span></span>

| <span data-ttu-id="3d6b9-178">Metriky skupiny</span><span class="sxs-lookup"><span data-stu-id="3d6b9-178">Metric group</span></span> | <span data-ttu-id="3d6b9-179">Kolekce modulu</span><span class="sxs-lookup"><span data-stu-id="3d6b9-179">Collector module</span></span> |
| --- | --- |
| <span data-ttu-id="3d6b9-180">basicExceptionBrowser,</span><span class="sxs-lookup"><span data-stu-id="3d6b9-180">basicExceptionBrowser,</span></span><br/><span data-ttu-id="3d6b9-181">clientPerformance,</span><span class="sxs-lookup"><span data-stu-id="3d6b9-181">clientPerformance,</span></span><br/><span data-ttu-id="3d6b9-182">zobrazit</span><span class="sxs-lookup"><span data-stu-id="3d6b9-182">view</span></span> |[<span data-ttu-id="3d6b9-183">JavaScript prohlížeče</span><span class="sxs-lookup"><span data-stu-id="3d6b9-183">Browser JavaScript</span></span>](app-insights-javascript.md) |
| <span data-ttu-id="3d6b9-184">performanceCounter</span><span class="sxs-lookup"><span data-stu-id="3d6b9-184">performanceCounter</span></span> |[<span data-ttu-id="3d6b9-185">Výkon</span><span class="sxs-lookup"><span data-stu-id="3d6b9-185">Performance</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="3d6b9-186">remoteDependencyFailed</span><span class="sxs-lookup"><span data-stu-id="3d6b9-186">remoteDependencyFailed</span></span> |[<span data-ttu-id="3d6b9-187">Závislost</span><span class="sxs-lookup"><span data-stu-id="3d6b9-187">Dependency</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="3d6b9-188">žádost,</span><span class="sxs-lookup"><span data-stu-id="3d6b9-188">request,</span></span><br/><span data-ttu-id="3d6b9-189">requestFailed</span><span class="sxs-lookup"><span data-stu-id="3d6b9-189">requestFailed</span></span> |[<span data-ttu-id="3d6b9-190">Požadavek na serveru</span><span class="sxs-lookup"><span data-stu-id="3d6b9-190">Server request</span></span>](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a><span data-ttu-id="3d6b9-191">Webhooky</span><span class="sxs-lookup"><span data-stu-id="3d6b9-191">Webhooks</span></span>
<span data-ttu-id="3d6b9-192">Můžete [automatizovat upozornění tooan odpovědi](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="3d6b9-192">You can [automate your response tooan alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="3d6b9-193">Azure bude volat webovou adresu podle vaší volby, když je vydána výstraha.</span><span class="sxs-lookup"><span data-stu-id="3d6b9-193">Azure will call a web address of your choice when an alert is raised.</span></span>

## <a name="see-also"></a><span data-ttu-id="3d6b9-194">Viz také</span><span class="sxs-lookup"><span data-stu-id="3d6b9-194">See also</span></span>
* [<span data-ttu-id="3d6b9-195">Skript tooconfigure Application Insights</span><span class="sxs-lookup"><span data-stu-id="3d6b9-195">Script tooconfigure Application Insights</span></span>](app-insights-powershell-script-create-resource.md)
* [<span data-ttu-id="3d6b9-196">Vytvoření služby Application Insights a web test prostředky ze šablon</span><span class="sxs-lookup"><span data-stu-id="3d6b9-196">Create Application Insights and web test resources from templates</span></span>](app-insights-powershell.md)
* [<span data-ttu-id="3d6b9-197">Automatizovat spojovacích Statistika tooApplication diagnostické nástroje sady Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="3d6b9-197">Automate coupling Microsoft Azure Diagnostics tooApplication Insights</span></span>](app-insights-powershell-azure-diagnostics.md)
* [<span data-ttu-id="3d6b9-198">Automatizovat upozornění tooan odpovědi</span><span class="sxs-lookup"><span data-stu-id="3d6b9-198">Automate your response tooan alert</span></span>](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
