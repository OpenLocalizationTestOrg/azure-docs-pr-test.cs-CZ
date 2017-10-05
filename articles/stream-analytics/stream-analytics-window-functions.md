---
title: "Úvod do funkce okno Stream Analytics | Microsoft Docs"
description: "Další informace o tři funkce okna v Stream Analytics (přeskakujícího, vše, klouzavé)."
keywords: "přeskakujícího okna, posuvném okně, posílání okna"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0d8d8717-5d23-43f0-b475-af078ab4627d
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 09915ad747153210f655b152355c551046066a88
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-stream-analytics-window-functions"></a><span data-ttu-id="083a8-104">Úvod do okna Stream Analytics funkce</span><span class="sxs-lookup"><span data-stu-id="083a8-104">Introduction to Stream Analytics Window functions</span></span>
<span data-ttu-id="083a8-105">V mnoha reálném čase streamování scénáře je potřeba provádět operace pouze na data obsažená v dočasné systému windows.</span><span class="sxs-lookup"><span data-stu-id="083a8-105">In many real time streaming scenarios, it is necessary to perform operations only on the data contained in temporal windows.</span></span> <span data-ttu-id="083a8-106">Nativní podpora pro oddílová funkce je klíčovou funkcí Azure Stream Analytics, která přemísťuje se ručička na vývojáře produktivitu při vytváření úlohy zpracování komplexní datového proudu.</span><span class="sxs-lookup"><span data-stu-id="083a8-106">Native support for windowing functions is a key feature of Azure Stream Analytics that moves the needle on developer productivity in authoring complex stream processing jobs.</span></span> <span data-ttu-id="083a8-107">Stream Analytics umožňuje vývojářům používat [ **Přeskakujícího**](https://msdn.microsoft.com/library/dn835055.aspx), [ **Hopping** ](https://msdn.microsoft.com/library/dn835041.aspx) a [ **posuvné** ](https://msdn.microsoft.com/library/dn835051.aspx) windows k provádění dočasné operací na datový proud.</span><span class="sxs-lookup"><span data-stu-id="083a8-107">Stream Analytics enables developers to use [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) and [**Sliding**](https://msdn.microsoft.com/library/dn835051.aspx) windows to perform temporal operations on streaming data.</span></span> <span data-ttu-id="083a8-108">Je to, že všechny [okno](https://msdn.microsoft.com/library/dn835019.aspx) operations výstup výsledků na **end** okna.</span><span class="sxs-lookup"><span data-stu-id="083a8-108">It is worth noting that all [Window](https://msdn.microsoft.com/library/dn835019.aspx) operations output results at the **end** of the window.</span></span> <span data-ttu-id="083a8-109">Výstup okna bude jedinou událost podle používá agregační funkci.</span><span class="sxs-lookup"><span data-stu-id="083a8-109">The output of the window will be single event based on the aggregate function used.</span></span> <span data-ttu-id="083a8-110">Události budou mít časové razítko konce okna a všechny funkce okna jsou definovány s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="083a8-110">The event will have the time stamp of the end of the window and all Window functions are defined with a fixed length.</span></span> <span data-ttu-id="083a8-111">Nakonec je důležité si uvědomit, že je třeba používat všechny funkce okno v [ **GROUP BY** ](https://msdn.microsoft.com/library/dn835023.aspx) klauzule.</span><span class="sxs-lookup"><span data-stu-id="083a8-111">Lastly it is important to note that all Window functions should be used in a [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) clause.</span></span>

![Stream Analytics okno funkce koncepty](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a><span data-ttu-id="083a8-113">Přeskakující okno</span><span class="sxs-lookup"><span data-stu-id="083a8-113">Tumbling Window</span></span>
<span data-ttu-id="083a8-114">Přeskakujícího okna, které funkce se používají pro segment datový proud do různých čas segmentů a provádět funkce proti, jako je například následující příklad.</span><span class="sxs-lookup"><span data-stu-id="083a8-114">Tumbling window functions are used to segment a data stream into distinct time segments and perform a function against them, such as the example below.</span></span> <span data-ttu-id="083a8-115">Klíče differentiators z Přeskakující okno se, že zopakují, se nepřekrývají, a událost nemůže patřit do více než jeden přeskakující okno.</span><span class="sxs-lookup"><span data-stu-id="083a8-115">The key differentiators of a Tumbling window are that they repeat, do not overlap and an event cannot belong to more than one tumbling window.</span></span>

![Funkce okno analýzy datového proudu přeskakujícího Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a><span data-ttu-id="083a8-117">Skákající okno</span><span class="sxs-lookup"><span data-stu-id="083a8-117">Hopping Window</span></span>
<span data-ttu-id="083a8-118">Skákající okno funkce směrování dál v čase prostřednictvím uplynutí určité doby.</span><span class="sxs-lookup"><span data-stu-id="083a8-118">Hopping window functions hop forward in time by a fixed period.</span></span> <span data-ttu-id="083a8-119">To může být snadno si je představit jako Přeskakující windows, které může dojít k překrytí, takže události můžou patřit do více než jednu sadu výsledků Hopping okno.</span><span class="sxs-lookup"><span data-stu-id="083a8-119">It may be easy to think of them as Tumbling windows that can overlap, so events can belong to more than one Hopping window result set.</span></span> <span data-ttu-id="083a8-120">Vytvoření okna Hopping stejné jako Přeskakující okno jeden by jednoduše zadejte velikost skoku tak, aby byla stejná jako velikost okna.</span><span class="sxs-lookup"><span data-stu-id="083a8-120">To make a Hopping window the same as a Tumbling window one would simply specify the hop size to be the same as the window size.</span></span> 

![Stream Analytics okno funkce skákající Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a><span data-ttu-id="083a8-122">Posuvné okno</span><span class="sxs-lookup"><span data-stu-id="083a8-122">Sliding Window</span></span>
<span data-ttu-id="083a8-123">Posuvné okno funkce, na rozdíl od Přeskakujícího nebo posílání windows, vytvořit výstup **pouze** při výskytu události.</span><span class="sxs-lookup"><span data-stu-id="083a8-123">Sliding window functions, unlike Tumbling or Hopping windows, produce an output **only**  when an event occurs.</span></span> <span data-ttu-id="083a8-124">Každý okno bude mít aspoň jednu událost a okna nepřetržitě přesune dál € (Epsilon –).</span><span class="sxs-lookup"><span data-stu-id="083a8-124">Every window will have at least one event and the window continuously moves forward by an € (epsilon).</span></span> <span data-ttu-id="083a8-125">Jako posílání Windows události může patřit do více než jeden klouzavé okna.</span><span class="sxs-lookup"><span data-stu-id="083a8-125">Like Hopping Windows, events can belong to more than one Sliding Window.</span></span>

![Stream Analytics okno funkce klouzavé Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a><span data-ttu-id="083a8-127">Získání nápovědy k okno funkce</span><span class="sxs-lookup"><span data-stu-id="083a8-127">Getting help with Window functions</span></span>
<span data-ttu-id="083a8-128">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="083a8-128">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="083a8-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="083a8-129">Next steps</span></span>
* [<span data-ttu-id="083a8-130">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="083a8-130">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="083a8-131">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="083a8-131">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="083a8-132">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="083a8-132">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="083a8-133">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="083a8-133">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="083a8-134">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="083a8-134">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

