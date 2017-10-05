---
title: "Principy úlohy Stream Analytics monitorování | Microsoft Docs"
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
ms.openlocfilehash: 13d96807a5591ec88deda19ea73cfedc07078433
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a><span data-ttu-id="30b49-104">Porozumění sledování úlohy Stream Analytics a monitorování dotazy</span><span class="sxs-lookup"><span data-stu-id="30b49-104">Understand Stream Analytics job monitoring and how to monitor queries</span></span>

## <a name="introduction-the-monitor-page"></a><span data-ttu-id="30b49-105">Úvod: Stránce monitorování</span><span class="sxs-lookup"><span data-stu-id="30b49-105">Introduction: The monitor page</span></span>
<span data-ttu-id="30b49-106">Azure portálu obě surface klíčové metriky, které lze použít ke sledování a řešení potíží s výkon dotazů a úlohy.</span><span class="sxs-lookup"><span data-stu-id="30b49-106">The Azure portal both surface key performance metrics that can be used to monitor and troubleshoot your query and job performance.</span></span> <span data-ttu-id="30b49-107">Pokud chcete zobrazit tyto metriky, přejděte do úlohy Stream Analytics jsou nepotřebujete vidět metriky pro a zobrazit **monitorování** části na stránce Přehled.</span><span class="sxs-lookup"><span data-stu-id="30b49-107">To see these metrics, browse to the Stream Analytics job you are interested in seeing metrics for and view the **Monitoring** section on the Overview page.</span></span>  

![Monitorování odkaz](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

<span data-ttu-id="30b49-109">Jak je znázorněno, objeví se okno:</span><span class="sxs-lookup"><span data-stu-id="30b49-109">The window will appear as shown:</span></span>

![Monitorování úlohy řídicí panel](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a><span data-ttu-id="30b49-111">Metriky, které jsou k dispozici pro Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="30b49-111">Metrics available for Stream Analytics</span></span>
| <span data-ttu-id="30b49-112">Metrika</span><span class="sxs-lookup"><span data-stu-id="30b49-112">Metric</span></span>                 | <span data-ttu-id="30b49-113">Definice</span><span class="sxs-lookup"><span data-stu-id="30b49-113">Definition</span></span>                               |
| ---------------------- | ---------------------------------------- |
| <span data-ttu-id="30b49-114">% Využití SU</span><span class="sxs-lookup"><span data-stu-id="30b49-114">SU % Utilization</span></span>       | <span data-ttu-id="30b49-115">Využití jednotek Streaming přiřazen do úlohy na kartě škálování úlohy.</span><span class="sxs-lookup"><span data-stu-id="30b49-115">The utilization of the Streaming Unit(s) assigned to a job from the Scale tab of the job.</span></span> <span data-ttu-id="30b49-116">Tento ukazatel přístup 80 % nebo výše, je vysoká pravděpodobnost, že zpracování událostí může být zpoždění nebo zastavená, provedení průběh.</span><span class="sxs-lookup"><span data-stu-id="30b49-116">Should this indicator reach 80%, or above, there is high probability that event processing may be delayed or stopped making progress.</span></span> |
| <span data-ttu-id="30b49-117">Vstupní události</span><span class="sxs-lookup"><span data-stu-id="30b49-117">Input Events</span></span>           | <span data-ttu-id="30b49-118">Množství dat přijatých úlohu služby Stream Analytics v počet událostí.</span><span class="sxs-lookup"><span data-stu-id="30b49-118">Amount of data received by the Stream Analytics job, in number of events.</span></span> <span data-ttu-id="30b49-119">To slouží k ověření, že události jsou odesílány do vstupního zdroje.</span><span class="sxs-lookup"><span data-stu-id="30b49-119">This can be used to validate that events are being sent to the input source.</span></span> |
| <span data-ttu-id="30b49-120">Výstup události</span><span class="sxs-lookup"><span data-stu-id="30b49-120">Output Events</span></span>          | <span data-ttu-id="30b49-121">Množství dat odesílaných v úloze Stream Analytics cíl výstupu, počtu událostí.</span><span class="sxs-lookup"><span data-stu-id="30b49-121">Amount of data sent by the Stream Analytics job to the output target, in number of events.</span></span> |
| <span data-ttu-id="30b49-122">Události mimo pořadí</span><span class="sxs-lookup"><span data-stu-id="30b49-122">Out-of-Order Events</span></span>    | <span data-ttu-id="30b49-123">Počet událostí přijatých mimo pořadí, které byly vyřazeny nebo zadané upravenou časového razítka na základě zásad řazení událostí.</span><span class="sxs-lookup"><span data-stu-id="30b49-123">Number of events received out of order that were either dropped or given an adjusted timestamp, based on the Event Ordering Policy.</span></span> <span data-ttu-id="30b49-124">To může mít vliv konfigurace nastavení interval Tolerance v pořádku.</span><span class="sxs-lookup"><span data-stu-id="30b49-124">This can be impacted by the configuration of the Out of Order Tolerance Window setting.</span></span> |
| <span data-ttu-id="30b49-125">Chyby při převodu dat</span><span class="sxs-lookup"><span data-stu-id="30b49-125">Data Conversion Errors</span></span> | <span data-ttu-id="30b49-126">Počet chyb při převodu dat způsobené úloha Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="30b49-126">Number of data conversion errors incurred by a Stream Analytics job.</span></span> |
| <span data-ttu-id="30b49-127">Chyby za běhu</span><span class="sxs-lookup"><span data-stu-id="30b49-127">Runtime Errors</span></span>         | <span data-ttu-id="30b49-128">Celkový počet chyb, které dojít během provádění úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="30b49-128">The total number of errors that happen during execution of a Stream Analytics job.</span></span> |
| <span data-ttu-id="30b49-129">Pozdní vstupní události</span><span class="sxs-lookup"><span data-stu-id="30b49-129">Late Input Events</span></span>      | <span data-ttu-id="30b49-130">Počet událostí přicházejících pozdní ze zdroje, které buď byla vyřazena nebo jejich časové razítko bylo změněno, na základě zásady řazení událostí Konfigurace nastavení pozdní interval Tolerance doručení.</span><span class="sxs-lookup"><span data-stu-id="30b49-130">Number of events arriving late from the source which have either been dropped or their timestamp has been adjusted, based on the Event Ordering Policy configuration of the Late Arrival Tolerance Window setting.</span></span> |
| <span data-ttu-id="30b49-131">Požadavky funkce</span><span class="sxs-lookup"><span data-stu-id="30b49-131">Function Requests</span></span>      | <span data-ttu-id="30b49-132">Počet volání funkce Azure Machine Learning (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="30b49-132">Number of calls to the Azure Machine Learning function (if present).</span></span> |
| <span data-ttu-id="30b49-133">Požadavky neúspěšné funkce</span><span class="sxs-lookup"><span data-stu-id="30b49-133">Failed Function Requests</span></span> | <span data-ttu-id="30b49-134">Počet selhání volání funkce Azure Machine Learning (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="30b49-134">Number of failed Azure Machine Learning function calls (if present).</span></span> |
| <span data-ttu-id="30b49-135">Události – funkce</span><span class="sxs-lookup"><span data-stu-id="30b49-135">Function Events</span></span>        | <span data-ttu-id="30b49-136">Počet událostí odeslaných do funkce Azure Machine Learning (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="30b49-136">Number of events sent to the Azure Machine Learning function (if present).</span></span> |
| <span data-ttu-id="30b49-137">Vstupní událost bajtů</span><span class="sxs-lookup"><span data-stu-id="30b49-137">Input Event Bytes</span></span>      | <span data-ttu-id="30b49-138">Množství dat přijatých úlohu služby Stream Analytics v bajtech.</span><span class="sxs-lookup"><span data-stu-id="30b49-138">Amount of data received by the Stream Analytics job, in bytes.</span></span> <span data-ttu-id="30b49-139">To slouží k ověření, že události jsou odesílány do vstupního zdroje.</span><span class="sxs-lookup"><span data-stu-id="30b49-139">This can be used to validate that events are being sent to the input source.</span></span> |


## <a name="customizing-monitoring-in-the-azure-portal"></a><span data-ttu-id="30b49-140">Přizpůsobení sledování na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="30b49-140">Customizing Monitoring in the Azure portal</span></span>
<span data-ttu-id="30b49-141">Můžete upravit typ grafu, metriky zobrazené a čas rozsah v nastavení upravit graf.</span><span class="sxs-lookup"><span data-stu-id="30b49-141">You can adjust the type of chart, metrics shown, and time range in the Edit Chart settings.</span></span> <span data-ttu-id="30b49-142">Podrobnosti najdete v tématu [postup přizpůsobení monitorování](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="30b49-142">For details, see [How to Customize Monitoring](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span></span>

  ![Graf sledování času dotazu](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="get-help"></a><span data-ttu-id="30b49-144">Podpora</span><span class="sxs-lookup"><span data-stu-id="30b49-144">Get help</span></span>
<span data-ttu-id="30b49-145">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="30b49-145">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="30b49-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30b49-146">Next steps</span></span>
* [<span data-ttu-id="30b49-147">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="30b49-147">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="30b49-148">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="30b49-148">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="30b49-149">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="30b49-149">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="30b49-150">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="30b49-150">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="30b49-151">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="30b49-151">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

