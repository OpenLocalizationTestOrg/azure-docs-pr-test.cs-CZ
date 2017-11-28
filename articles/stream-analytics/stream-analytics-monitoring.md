---
title: "aaaUnderstanding monitorování úlohy Stream Analytics | Microsoft Docs"
description: "Principy sledování úlohy Stream Analytics"
keywords: "monitorování dotazu"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5f5cc00f-4a7b-491e-89e1-dbafea46d399
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 36819025c7b2ddbf4b9694522f1b4820407ca5f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-stream-analytics-job-monitoring-and-how-toomonitor-queries"></a><span data-ttu-id="cdcd3-104">Pochopení monitorování úlohy Stream Analytics a jak toomonitor dotazy</span><span class="sxs-lookup"><span data-stu-id="cdcd3-104">Understand Stream Analytics job monitoring and how toomonitor queries</span></span>

## <a name="introduction-hello-monitor-page"></a><span data-ttu-id="cdcd3-105">Úvod: stránka sledování hello</span><span class="sxs-lookup"><span data-stu-id="cdcd3-105">Introduction: hello monitor page</span></span>
<span data-ttu-id="cdcd3-106">Hello portálu Azure i surface klíčových metrik, které je možné použít toomonitor a řešení potíží s výkon dotazů a úlohy.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-106">hello Azure portal both surface key performance metrics that can be used toomonitor and troubleshoot your query and job performance.</span></span> <span data-ttu-id="cdcd3-107">toosee tyto metriky procházet úlohy služby Stream Analytics toohello vás zajímá zobrazení metriky pro a zobrazení hello **monitorování** části na stránce Přehled hello.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-107">toosee these metrics, browse toohello Stream Analytics job you are interested in seeing metrics for and view hello **Monitoring** section on hello Overview page.</span></span>  

![Monitorování odkaz](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

<span data-ttu-id="cdcd3-109">jak je znázorněno, zobrazí se okno Hello:</span><span class="sxs-lookup"><span data-stu-id="cdcd3-109">hello window will appear as shown:</span></span>

![Monitorování úlohy řídicí panel](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a><span data-ttu-id="cdcd3-111">Metriky, které jsou k dispozici pro Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cdcd3-111">Metrics available for Stream Analytics</span></span>
| <span data-ttu-id="cdcd3-112">Metrika</span><span class="sxs-lookup"><span data-stu-id="cdcd3-112">Metric</span></span>                 | <span data-ttu-id="cdcd3-113">Definice</span><span class="sxs-lookup"><span data-stu-id="cdcd3-113">Definition</span></span>                               |
| ---------------------- | ---------------------------------------- |
| <span data-ttu-id="cdcd3-114">% Využití SU</span><span class="sxs-lookup"><span data-stu-id="cdcd3-114">SU % Utilization</span></span>       | <span data-ttu-id="cdcd3-115">Hello využití hello jednotka streamování přiřadit tooa úlohu z karty škálování hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-115">hello utilization of hello Streaming Unit(s) assigned tooa job from hello Scale tab of hello job.</span></span> <span data-ttu-id="cdcd3-116">Tento ukazatel přístup 80 % nebo výše, je vysoká pravděpodobnost, že zpracování událostí může být zpoždění nebo zastavená, provedení průběh.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-116">Should this indicator reach 80%, or above, there is high probability that event processing may be delayed or stopped making progress.</span></span> |
| <span data-ttu-id="cdcd3-117">Vstupní události</span><span class="sxs-lookup"><span data-stu-id="cdcd3-117">Input Events</span></span>           | <span data-ttu-id="cdcd3-118">Množství dat přijatých hello úlohy služby Stream Analytics v počet událostí.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-118">Amount of data received by hello Stream Analytics job, in number of events.</span></span> <span data-ttu-id="cdcd3-119">To může být použité toovalidate, že události jsou odesílány toohello vstupní zdroj.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-119">This can be used toovalidate that events are being sent toohello input source.</span></span> |
| <span data-ttu-id="cdcd3-120">Výstup události</span><span class="sxs-lookup"><span data-stu-id="cdcd3-120">Output Events</span></span>          | <span data-ttu-id="cdcd3-121">Množství dat odesílaných hello Stream Analytics úlohy toohello výstup cíl, v počet událostí.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-121">Amount of data sent by hello Stream Analytics job toohello output target, in number of events.</span></span> |
| <span data-ttu-id="cdcd3-122">Události mimo pořadí</span><span class="sxs-lookup"><span data-stu-id="cdcd3-122">Out-of-Order Events</span></span>    | <span data-ttu-id="cdcd3-123">Počet událostí přijatých mimo pořadí, které byly vyřazeny nebo zadané upravené časové razítko podle hello zásady řazení událostí.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-123">Number of events received out of order that were either dropped or given an adjusted timestamp, based on hello Event Ordering Policy.</span></span> <span data-ttu-id="cdcd3-124">To může mít vliv hello konfigurace nastavení interval Tolerance pořádku hello.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-124">This can be impacted by hello configuration of hello Out of Order Tolerance Window setting.</span></span> |
| <span data-ttu-id="cdcd3-125">Chyby při převodu dat</span><span class="sxs-lookup"><span data-stu-id="cdcd3-125">Data Conversion Errors</span></span> | <span data-ttu-id="cdcd3-126">Počet chyb při převodu dat způsobené úloha Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-126">Number of data conversion errors incurred by a Stream Analytics job.</span></span> |
| <span data-ttu-id="cdcd3-127">Chyby za běhu</span><span class="sxs-lookup"><span data-stu-id="cdcd3-127">Runtime Errors</span></span>         | <span data-ttu-id="cdcd3-128">Hello celkový počet chyb, které dojít během provádění úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-128">hello total number of errors that happen during execution of a Stream Analytics job.</span></span> |
| <span data-ttu-id="cdcd3-129">Pozdní vstupní události</span><span class="sxs-lookup"><span data-stu-id="cdcd3-129">Late Input Events</span></span>      | <span data-ttu-id="cdcd3-130">Počet událostí přicházejících pozdní ze hello zdroj, který buď byla vyřazena nebo jejich časové razítko bylo změněno, v závislosti na konfiguraci zásady řazení událostí hello hello pozdní interval Tolerance dodání nastavení.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-130">Number of events arriving late from hello source which have either been dropped or their timestamp has been adjusted, based on hello Event Ordering Policy configuration of hello Late Arrival Tolerance Window setting.</span></span> |
| <span data-ttu-id="cdcd3-131">Požadavky funkce</span><span class="sxs-lookup"><span data-stu-id="cdcd3-131">Function Requests</span></span>      | <span data-ttu-id="cdcd3-132">Počet volání toohello funkce Azure Machine Learning (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="cdcd3-132">Number of calls toohello Azure Machine Learning function (if present).</span></span> |
| <span data-ttu-id="cdcd3-133">Požadavky neúspěšné funkce</span><span class="sxs-lookup"><span data-stu-id="cdcd3-133">Failed Function Requests</span></span> | <span data-ttu-id="cdcd3-134">Počet selhání volání funkce Azure Machine Learning (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="cdcd3-134">Number of failed Azure Machine Learning function calls (if present).</span></span> |
| <span data-ttu-id="cdcd3-135">Události – funkce</span><span class="sxs-lookup"><span data-stu-id="cdcd3-135">Function Events</span></span>        | <span data-ttu-id="cdcd3-136">Počet událostí odeslaných funkce Azure Machine Learning toohello (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="cdcd3-136">Number of events sent toohello Azure Machine Learning function (if present).</span></span> |
| <span data-ttu-id="cdcd3-137">Vstupní událost bajtů</span><span class="sxs-lookup"><span data-stu-id="cdcd3-137">Input Event Bytes</span></span>      | <span data-ttu-id="cdcd3-138">Množství dat přijatých hello úlohy služby Stream Analytics v bajtech.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-138">Amount of data received by hello Stream Analytics job, in bytes.</span></span> <span data-ttu-id="cdcd3-139">To může být použité toovalidate, že události jsou odesílány toohello vstupní zdroj.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-139">This can be used toovalidate that events are being sent toohello input source.</span></span> |


## <a name="customizing-monitoring-in-hello-azure-portal"></a><span data-ttu-id="cdcd3-140">Přizpůsobení sledování v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cdcd3-140">Customizing Monitoring in hello Azure portal</span></span>
<span data-ttu-id="cdcd3-141">Můžete upravit hello typ grafu, metriky zobrazené a čas rozsah v nastavení upravit graf hello.</span><span class="sxs-lookup"><span data-stu-id="cdcd3-141">You can adjust hello type of chart, metrics shown, and time range in hello Edit Chart settings.</span></span> <span data-ttu-id="cdcd3-142">Podrobnosti najdete v tématu [jak tooCustomize monitorování](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="cdcd3-142">For details, see [How tooCustomize Monitoring](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span></span>

  ![Graf sledování času dotazu](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="get-help"></a><span data-ttu-id="cdcd3-144">Podpora</span><span class="sxs-lookup"><span data-stu-id="cdcd3-144">Get help</span></span>
<span data-ttu-id="cdcd3-145">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="cdcd3-145">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="cdcd3-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cdcd3-146">Next steps</span></span>
* [<span data-ttu-id="cdcd3-147">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cdcd3-147">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="cdcd3-148">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cdcd3-148">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="cdcd3-149">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cdcd3-149">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="cdcd3-150">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="cdcd3-150">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="cdcd3-151">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cdcd3-151">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

