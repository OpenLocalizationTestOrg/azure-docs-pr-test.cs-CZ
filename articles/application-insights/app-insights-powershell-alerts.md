---
title: "Pomocí prostředí Powershell nastavit výstrahy ve službě Application Insights | Microsoft Docs"
description: "Automatizovat konfiguraci Application Insights získat e-mailů o metriky změny."
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
ms.openlocfilehash: 64675c51abf80daa3a55220f910aa8fdee1042ca
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a><span data-ttu-id="411bc-103">Použití prostředí PowerShell k nastavení výstrahy v nástroji Application Insights</span><span class="sxs-lookup"><span data-stu-id="411bc-103">Use PowerShell to set alerts in Application Insights</span></span>
<span data-ttu-id="411bc-104">Můžete automatizovat konfiguraci [výstrahy](app-insights-alerts.md) v [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="411bc-104">You can automate the configuration of [alerts](app-insights-alerts.md) in [Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="411bc-105">Kromě toho můžete [nastavit webhooky k automatizaci vaše odpověď na výstrahu](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="411bc-105">In addition, you can [set webhooks to automate your response to an alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

> [!NOTE]
> <span data-ttu-id="411bc-106">Pokud chcete vytvořit prostředky a výstrahy ve stejnou dobu, zvažte [pomocí šablony Azure Resource Manager](app-insights-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="411bc-106">If you want to create resources and alerts at the same time, consider [using an Azure Resource Manager template](app-insights-powershell.md).</span></span>
>
>

## <a name="one-time-setup"></a><span data-ttu-id="411bc-107">Jednorázové instalace</span><span class="sxs-lookup"><span data-stu-id="411bc-107">One-time setup</span></span>
<span data-ttu-id="411bc-108">Pokud jste nepoužili prostředí PowerShell s předplatným Azure před:</span><span class="sxs-lookup"><span data-stu-id="411bc-108">If you haven't used PowerShell with your Azure subscription before:</span></span>

<span data-ttu-id="411bc-109">Instalace modulu Azure Powershell na počítači, kde chcete spustit skripty.</span><span class="sxs-lookup"><span data-stu-id="411bc-109">Install the Azure Powershell module on the machine where you want to run the scripts.</span></span>

* <span data-ttu-id="411bc-110">Nainstalujte [instalačního programu webové platformy (verze 5 nebo novější)](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="411bc-110">Install [Microsoft Web Platform Installer (v5 or higher)](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="411bc-111">Jeho použití k instalaci aplikace Microsoft Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="411bc-111">Use it to install Microsoft Azure Powershell</span></span>

## <a name="connect-to-azure"></a><span data-ttu-id="411bc-112">Připojení k Azure</span><span class="sxs-lookup"><span data-stu-id="411bc-112">Connect to Azure</span></span>
<span data-ttu-id="411bc-113">Otevřete prostředí Azure PowerShell a [připojení k vašemu předplatnému](/powershell/azure/overview):</span><span class="sxs-lookup"><span data-stu-id="411bc-113">Start Azure PowerShell and [connect to your subscription](/powershell/azure/overview):</span></span>

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a><span data-ttu-id="411bc-114">Získání výstrahy</span><span class="sxs-lookup"><span data-stu-id="411bc-114">Get alerts</span></span>
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a><span data-ttu-id="411bc-115">Přidání oznámení</span><span class="sxs-lookup"><span data-stu-id="411bc-115">Add alert</span></span>
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



## <a name="example-1"></a><span data-ttu-id="411bc-116">Příklad 1</span><span class="sxs-lookup"><span data-stu-id="411bc-116">Example 1</span></span>
<span data-ttu-id="411bc-117">Poslat mi e-mail, pokud odpověď serveru na požadavky HTTP, průměr 5 minut, je nižší než 1 sekunda.</span><span class="sxs-lookup"><span data-stu-id="411bc-117">Email me if the server's response to HTTP requests, averaged over 5 minutes, is slower than 1 second.</span></span> <span data-ttu-id="411bc-118">Moje prostředek Application Insights se nazývá IceCreamWebApp a je ve skupině prostředků společnosti Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="411bc-118">My Application Insights resource is called IceCreamWebApp, and it is in resource group Fabrikam.</span></span> <span data-ttu-id="411bc-119">Jsem vlastník předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="411bc-119">I am the owner of the Azure subscription.</span></span>

<span data-ttu-id="411bc-120">Identifikátor GUID je ID předplatného (ne klíč instrumentace aplikace).</span><span class="sxs-lookup"><span data-stu-id="411bc-120">The GUID is the subscription ID (not the instrumentation key of the application).</span></span>

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a><span data-ttu-id="411bc-121">Příklad 2</span><span class="sxs-lookup"><span data-stu-id="411bc-121">Example 2</span></span>
<span data-ttu-id="411bc-122">Je nutné aplikaci, ve které používám [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) nahlásit metriky s názvem "salesPerHour."</span><span class="sxs-lookup"><span data-stu-id="411bc-122">I have an application in which I use [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) to report a metric named "salesPerHour."</span></span> <span data-ttu-id="411bc-123">Odesílání že e-mailu na kolegové Pokud "salesPerHour" klesne pod 100, průměr po dobu 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="411bc-123">Send an email to my colleagues if "salesPerHour" drops below 100, averaged over 24 hours.</span></span>

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

<span data-ttu-id="411bc-124">Stejného pravidla lze použít pro metriku hlášené pomocí [měření parametr](app-insights-api-custom-events-metrics.md#properties) jiné sledování volání například TrackEvent nebo trackPageView.</span><span class="sxs-lookup"><span data-stu-id="411bc-124">The same rule can be used for the metric reported by using the [measurement parameter](app-insights-api-custom-events-metrics.md#properties) of another tracking call such as TrackEvent or trackPageView.</span></span>

## <a name="metric-names"></a><span data-ttu-id="411bc-125">Metriky názvy</span><span class="sxs-lookup"><span data-stu-id="411bc-125">Metric names</span></span>
| <span data-ttu-id="411bc-126">Název metriky</span><span class="sxs-lookup"><span data-stu-id="411bc-126">Metric name</span></span> | <span data-ttu-id="411bc-127">Název obrazovky</span><span class="sxs-lookup"><span data-stu-id="411bc-127">Screen name</span></span> | <span data-ttu-id="411bc-128">Popis</span><span class="sxs-lookup"><span data-stu-id="411bc-128">Description</span></span> |
| --- | --- | --- |
| `basicExceptionBrowser.count` |<span data-ttu-id="411bc-129">Výjimky prohlížečů</span><span class="sxs-lookup"><span data-stu-id="411bc-129">Browser exceptions</span></span> |<span data-ttu-id="411bc-130">Počet nezachycená výjimky vydané v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="411bc-130">Count of uncaught exceptions thrown in the browser.</span></span> |
| `basicExceptionServer.count` |<span data-ttu-id="411bc-131">Výjimky serveru</span><span class="sxs-lookup"><span data-stu-id="411bc-131">Server exceptions</span></span> |<span data-ttu-id="411bc-132">Počet neošetřené výjimky vyvolané aplikace</span><span class="sxs-lookup"><span data-stu-id="411bc-132">Count of unhandled exceptions thrown by the app</span></span> |
| `clientPerformance.clientProcess.value` |<span data-ttu-id="411bc-133">Doba zpracování klienta</span><span class="sxs-lookup"><span data-stu-id="411bc-133">Client processing time</span></span> |<span data-ttu-id="411bc-134">Doba mezi přijetí poslední bajt dokumentu, dokud nebude modelu DOM vložen.</span><span class="sxs-lookup"><span data-stu-id="411bc-134">Time between receiving the last byte of a document until the DOM is loaded.</span></span> <span data-ttu-id="411bc-135">Asynchronních připojení může být stále aktivní.</span><span class="sxs-lookup"><span data-stu-id="411bc-135">Async requests may still be processing.</span></span> |
| `clientPerformance.networkConnection.value` |<span data-ttu-id="411bc-136">Doba připojení sítě zatížení stránky</span><span class="sxs-lookup"><span data-stu-id="411bc-136">Page load network connect time</span></span> |<span data-ttu-id="411bc-137">Čas, kdy prohlížeč potřebné k připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="411bc-137">Time the browser takes to connect to the network.</span></span> <span data-ttu-id="411bc-138">Může být 0, pokud do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="411bc-138">Can be 0 if cached.</span></span> |
| `clientPerformance.receiveRequest.value` |<span data-ttu-id="411bc-139">Přijetí doba odezvy</span><span class="sxs-lookup"><span data-stu-id="411bc-139">Receiving response time</span></span> |<span data-ttu-id="411bc-140">Doba mezi prohlížeče odesílání požadavku na spuštění na obdrží odpověď.</span><span class="sxs-lookup"><span data-stu-id="411bc-140">Time between browser sending request to starting to receive response.</span></span> |
| `clientPerformance.sendRequest.value` |<span data-ttu-id="411bc-141">Odeslat doba požadavku</span><span class="sxs-lookup"><span data-stu-id="411bc-141">Send request time</span></span> |<span data-ttu-id="411bc-142">Doba, za kterou prohlížeč k odeslání požadavku.</span><span class="sxs-lookup"><span data-stu-id="411bc-142">Time taken by browser to send request.</span></span> |
| `clientPerformance.total.value` |<span data-ttu-id="411bc-143">Čas načítání stránky prohlížeče</span><span class="sxs-lookup"><span data-stu-id="411bc-143">Browser page load time</span></span> |<span data-ttu-id="411bc-144">Čas od požadavku uživatele, dokud se načtou DOM, předlohy se styly, skripty a obrázků.</span><span class="sxs-lookup"><span data-stu-id="411bc-144">Time from user request until DOM, stylesheets, scripts and images are loaded.</span></span> |
| `performanceCounter.available_bytes.value` |<span data-ttu-id="411bc-145">Dostupná paměť</span><span class="sxs-lookup"><span data-stu-id="411bc-145">Available memory</span></span> |<span data-ttu-id="411bc-146">Fyzické paměti k dispozici pro zpracování nebo pro použití systémem.</span><span class="sxs-lookup"><span data-stu-id="411bc-146">Physical memory immediately available for a process or for system use.</span></span> |
| `performanceCounter.io_data_bytes_per_sec.value` |<span data-ttu-id="411bc-147">Rychlost zpracování vstupně-výstupní operace</span><span class="sxs-lookup"><span data-stu-id="411bc-147">Process IO Rate</span></span> |<span data-ttu-id="411bc-148">Celkový počet bajtů za sekundu číst a zapisovat do souborů, sítě a zařízení.</span><span class="sxs-lookup"><span data-stu-id="411bc-148">Total bytes per second read and written to files, network and devices.</span></span> |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |<span data-ttu-id="411bc-149">míra výjimky</span><span class="sxs-lookup"><span data-stu-id="411bc-149">exception rate</span></span> |<span data-ttu-id="411bc-150">Výjimek vyvolaných za sekundu.</span><span class="sxs-lookup"><span data-stu-id="411bc-150">Exceptions thrown per second.</span></span> |
| `performanceCounter.percentage_processor_time.value` |<span data-ttu-id="411bc-151">Proces procesoru</span><span class="sxs-lookup"><span data-stu-id="411bc-151">Process CPU</span></span> |<span data-ttu-id="411bc-152">Procentuální hodnotu uplynulého času, podprocesy procesu používá procesor pro spouštění instrukcí pro proces aplikace.</span><span class="sxs-lookup"><span data-stu-id="411bc-152">The percentage of elapsed time of all process threads used by the processor to execution instructions for the applications process.</span></span> |
| `performanceCounter.percentage_processor_total.value` |<span data-ttu-id="411bc-153">Čas procesoru</span><span class="sxs-lookup"><span data-stu-id="411bc-153">Processor time</span></span> |<span data-ttu-id="411bc-154">Procento času, kterou procesor stráví jiných než nečinných vláken.</span><span class="sxs-lookup"><span data-stu-id="411bc-154">The percentage of time that the processor spends in non-Idle threads.</span></span> |
| `performanceCounter.process_private_bytes.value` |<span data-ttu-id="411bc-155">Proces nesdílených bajtů</span><span class="sxs-lookup"><span data-stu-id="411bc-155">Process private bytes</span></span> |<span data-ttu-id="411bc-156">Paměť výhradně přiřazená procesy v monitorované aplikaci.</span><span class="sxs-lookup"><span data-stu-id="411bc-156">Memory exclusively assigned to the monitored application's processes.</span></span> |
| `performanceCounter.request_execution_time.value` |<span data-ttu-id="411bc-157">Čas provádění požadavku ASP.NET</span><span class="sxs-lookup"><span data-stu-id="411bc-157">ASP.NET request execution time</span></span> |<span data-ttu-id="411bc-158">Čas provedení posledního požadavku.</span><span class="sxs-lookup"><span data-stu-id="411bc-158">Execution time of the most recent request.</span></span> |
| `performanceCounter.requests_in_application_queue.value` |<span data-ttu-id="411bc-159">ASP.NET požadavků ve frontě provádění</span><span class="sxs-lookup"><span data-stu-id="411bc-159">ASP.NET requests in execution queue</span></span> |<span data-ttu-id="411bc-160">Délka frontě požadavků aplikace.</span><span class="sxs-lookup"><span data-stu-id="411bc-160">Length of the application request queue.</span></span> |
| `performanceCounter.requests_per_sec.value` |<span data-ttu-id="411bc-161">Rychlost požadavků ASP.NET</span><span class="sxs-lookup"><span data-stu-id="411bc-161">ASP.NET request rate</span></span> |<span data-ttu-id="411bc-162">Počet všech požadavků na pro aplikaci za sekundu z technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="411bc-162">Rate of all requests to the application per second from ASP.NET.</span></span> |
| `remoteDependencyFailed.durationMetric.count` |<span data-ttu-id="411bc-163">Selhání závislostí</span><span class="sxs-lookup"><span data-stu-id="411bc-163">Dependency failures</span></span> |<span data-ttu-id="411bc-164">Počet selhání volání od serverové aplikace na externí zdroje.</span><span class="sxs-lookup"><span data-stu-id="411bc-164">Count of failed calls made by the server application to external resources.</span></span> |
| `request.duration` |<span data-ttu-id="411bc-165">Doba odezvy serveru</span><span class="sxs-lookup"><span data-stu-id="411bc-165">Server response time</span></span> |<span data-ttu-id="411bc-166">Doba mezi přijetí požadavku HTTP a dokončením odeslání odpovědi.</span><span class="sxs-lookup"><span data-stu-id="411bc-166">Time between receiving an HTTP request and finishing sending the response.</span></span> |
| `request.rate` |<span data-ttu-id="411bc-167">Rychlost požadavků</span><span class="sxs-lookup"><span data-stu-id="411bc-167">Request rate</span></span> |<span data-ttu-id="411bc-168">Počet všech požadavků na pro aplikaci za sekundu.</span><span class="sxs-lookup"><span data-stu-id="411bc-168">Rate of all requests to the application per second.</span></span> |
| `requestFailed.count` |<span data-ttu-id="411bc-169">Neúspěšné požadavky</span><span class="sxs-lookup"><span data-stu-id="411bc-169">Failed requests</span></span> |<span data-ttu-id="411bc-170">Počet HTTP požadavků, které kód odpovědi > = 400</span><span class="sxs-lookup"><span data-stu-id="411bc-170">Count of HTTP requests that resulted in a response code >= 400</span></span> |
| `view.count` |<span data-ttu-id="411bc-171">Zobrazení stránky</span><span class="sxs-lookup"><span data-stu-id="411bc-171">Page views</span></span> |<span data-ttu-id="411bc-172">Počet žádostí uživatele klienta pro webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="411bc-172">Count of client user requests for a web page.</span></span> <span data-ttu-id="411bc-173">Syntetické provoz odfiltrována.</span><span class="sxs-lookup"><span data-stu-id="411bc-173">Synthetic traffic is filtered out.</span></span> |
| <span data-ttu-id="411bc-174">{váš vlastní název metriky}</span><span class="sxs-lookup"><span data-stu-id="411bc-174">{your custom metric name}</span></span> |<span data-ttu-id="411bc-175">{Název metriky}</span><span class="sxs-lookup"><span data-stu-id="411bc-175">{Your metric name}</span></span> |<span data-ttu-id="411bc-176">Metriky hodnota hlášené [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) nebo [parametr měření volání sledování](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="411bc-176">Your metric value reported by [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) or in the [measurements parameter of a tracking call](app-insights-api-custom-events-metrics.md#properties).</span></span> |

<span data-ttu-id="411bc-177">Metriky se odesílají moduly různých telemetrie:</span><span class="sxs-lookup"><span data-stu-id="411bc-177">The metrics are sent by different telemetry modules:</span></span>

| <span data-ttu-id="411bc-178">Metriky skupiny</span><span class="sxs-lookup"><span data-stu-id="411bc-178">Metric group</span></span> | <span data-ttu-id="411bc-179">Kolekce modulu</span><span class="sxs-lookup"><span data-stu-id="411bc-179">Collector module</span></span> |
| --- | --- |
| <span data-ttu-id="411bc-180">basicExceptionBrowser,</span><span class="sxs-lookup"><span data-stu-id="411bc-180">basicExceptionBrowser,</span></span><br/><span data-ttu-id="411bc-181">clientPerformance,</span><span class="sxs-lookup"><span data-stu-id="411bc-181">clientPerformance,</span></span><br/><span data-ttu-id="411bc-182">zobrazit</span><span class="sxs-lookup"><span data-stu-id="411bc-182">view</span></span> |[<span data-ttu-id="411bc-183">JavaScript prohlížeče</span><span class="sxs-lookup"><span data-stu-id="411bc-183">Browser JavaScript</span></span>](app-insights-javascript.md) |
| <span data-ttu-id="411bc-184">performanceCounter</span><span class="sxs-lookup"><span data-stu-id="411bc-184">performanceCounter</span></span> |[<span data-ttu-id="411bc-185">Výkon</span><span class="sxs-lookup"><span data-stu-id="411bc-185">Performance</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="411bc-186">remoteDependencyFailed</span><span class="sxs-lookup"><span data-stu-id="411bc-186">remoteDependencyFailed</span></span> |[<span data-ttu-id="411bc-187">Závislost</span><span class="sxs-lookup"><span data-stu-id="411bc-187">Dependency</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="411bc-188">žádost,</span><span class="sxs-lookup"><span data-stu-id="411bc-188">request,</span></span><br/><span data-ttu-id="411bc-189">requestFailed</span><span class="sxs-lookup"><span data-stu-id="411bc-189">requestFailed</span></span> |[<span data-ttu-id="411bc-190">Požadavek na serveru</span><span class="sxs-lookup"><span data-stu-id="411bc-190">Server request</span></span>](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a><span data-ttu-id="411bc-191">Webhooky</span><span class="sxs-lookup"><span data-stu-id="411bc-191">Webhooks</span></span>
<span data-ttu-id="411bc-192">Můžete [automatizovat vaše odpověď na výstrahu](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="411bc-192">You can [automate your response to an alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="411bc-193">Azure bude volat webovou adresu podle vaší volby, když je vydána výstraha.</span><span class="sxs-lookup"><span data-stu-id="411bc-193">Azure will call a web address of your choice when an alert is raised.</span></span>

## <a name="see-also"></a><span data-ttu-id="411bc-194">Viz také</span><span class="sxs-lookup"><span data-stu-id="411bc-194">See also</span></span>
* [<span data-ttu-id="411bc-195">Skript, který nakonfiguruje Application Insights</span><span class="sxs-lookup"><span data-stu-id="411bc-195">Script to configure Application Insights</span></span>](app-insights-powershell-script-create-resource.md)
* [<span data-ttu-id="411bc-196">Vytvoření služby Application Insights a web test prostředky ze šablon</span><span class="sxs-lookup"><span data-stu-id="411bc-196">Create Application Insights and web test resources from templates</span></span>](app-insights-powershell.md)
* [<span data-ttu-id="411bc-197">Automatizovat spojovacích Microsoft Azure Diagnostics Application insights</span><span class="sxs-lookup"><span data-stu-id="411bc-197">Automate coupling Microsoft Azure Diagnostics to Application Insights</span></span>](app-insights-powershell-azure-diagnostics.md)
* [<span data-ttu-id="411bc-198">Automatizovat vaše odpověď na výstrahu</span><span class="sxs-lookup"><span data-stu-id="411bc-198">Automate your response to an alert</span></span>](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
