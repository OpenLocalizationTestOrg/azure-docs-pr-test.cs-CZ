---
title: "funkce analýzy okno tooStream aaaIntroduction | Microsoft Docs"
description: "Další informace o hello tři okno funkce v Stream Analytics (přeskakujícího, vše, klouzavé)."
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
ms.openlocfilehash: 7fc36eb9afb2b28e2791d925d26923145eb38074
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toostream-analytics-window-functions"></a><span data-ttu-id="67848-104">Funkce analýzy okno tooStream Úvod</span><span class="sxs-lookup"><span data-stu-id="67848-104">Introduction tooStream Analytics Window functions</span></span>
<span data-ttu-id="67848-105">V mnoha reálném čase streamování scénáře je nutné tooperform operace pouze na hello data obsažená v dočasné systému windows.</span><span class="sxs-lookup"><span data-stu-id="67848-105">In many real time streaming scenarios, it is necessary tooperform operations only on hello data contained in temporal windows.</span></span> <span data-ttu-id="67848-106">Nativní podpora pro oddílová funkce je klíčovou funkcí Azure Stream Analytics, která přemísťuje hello ručička na vývojáře produktivitu při vytváření úlohy zpracování komplexní datového proudu.</span><span class="sxs-lookup"><span data-stu-id="67848-106">Native support for windowing functions is a key feature of Azure Stream Analytics that moves hello needle on developer productivity in authoring complex stream processing jobs.</span></span> <span data-ttu-id="67848-107">Stream Analytics umožňuje vývojářům toouse [ **Přeskakujícího**](https://msdn.microsoft.com/library/dn835055.aspx), [ **Hopping** ](https://msdn.microsoft.com/library/dn835041.aspx) a [ **posuvné** ](https://msdn.microsoft.com/library/dn835051.aspx) dočasné operace tooperform systému windows na datový proud.</span><span class="sxs-lookup"><span data-stu-id="67848-107">Stream Analytics enables developers toouse [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) and [**Sliding**](https://msdn.microsoft.com/library/dn835051.aspx) windows tooperform temporal operations on streaming data.</span></span> <span data-ttu-id="67848-108">Je to, že všechny [okno](https://msdn.microsoft.com/library/dn835019.aspx) operations výstup výsledků v hello **end** okna hello.</span><span class="sxs-lookup"><span data-stu-id="67848-108">It is worth noting that all [Window](https://msdn.microsoft.com/library/dn835019.aspx) operations output results at hello **end** of hello window.</span></span> <span data-ttu-id="67848-109">výstup Hello okna hello bude jedinou událost podle hello používá agregační funkci.</span><span class="sxs-lookup"><span data-stu-id="67848-109">hello output of hello window will be single event based on hello aggregate function used.</span></span> <span data-ttu-id="67848-110">Hello událost může mít hello časové razítko konce hello okna hello a všechny funkce okna jsou definovány s pevnou délkou.</span><span class="sxs-lookup"><span data-stu-id="67848-110">hello event will have hello time stamp of hello end of hello window and all Window functions are defined with a fixed length.</span></span> <span data-ttu-id="67848-111">Nakonec je důležité toonote, který se má použít všechny funkce okna v [ **GROUP BY** ](https://msdn.microsoft.com/library/dn835023.aspx) klauzule.</span><span class="sxs-lookup"><span data-stu-id="67848-111">Lastly it is important toonote that all Window functions should be used in a [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) clause.</span></span>

![Stream Analytics okno funkce koncepty](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a><span data-ttu-id="67848-113">Přeskakující okno</span><span class="sxs-lookup"><span data-stu-id="67848-113">Tumbling Window</span></span>
<span data-ttu-id="67848-114">Přeskakující okno funkce jsou použité toosegment datový proud do různých čas segmentů a provádět funkce proti, jako je například následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="67848-114">Tumbling window functions are used toosegment a data stream into distinct time segments and perform a function against them, such as hello example below.</span></span> <span data-ttu-id="67848-115">Hello klíče differentiators z Přeskakující okno se, že zopakují, se nepřekrývají, a událost nemůže patřit toomore než jeden přeskakující okno.</span><span class="sxs-lookup"><span data-stu-id="67848-115">hello key differentiators of a Tumbling window are that they repeat, do not overlap and an event cannot belong toomore than one tumbling window.</span></span>

![Funkce okno analýzy datového proudu přeskakujícího Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a><span data-ttu-id="67848-117">Skákající okno</span><span class="sxs-lookup"><span data-stu-id="67848-117">Hopping Window</span></span>
<span data-ttu-id="67848-118">Skákající okno funkce směrování dál v čase prostřednictvím uplynutí určité doby.</span><span class="sxs-lookup"><span data-stu-id="67848-118">Hopping window functions hop forward in time by a fixed period.</span></span> <span data-ttu-id="67848-119">To může být snadno toothink z nich jako Přeskakující windows, které může dojít k překrytí, tak události můžou patřit toomore než jednu sadu výsledků Hopping okno.</span><span class="sxs-lookup"><span data-stu-id="67848-119">It may be easy toothink of them as Tumbling windows that can overlap, so events can belong toomore than one Hopping window result set.</span></span> <span data-ttu-id="67848-120">toomake okno Hopping hello stejné jako Přeskakující okno jednoduše by zadejte toobe velikost skoku hello hello stejná jako velikost okna hello.</span><span class="sxs-lookup"><span data-stu-id="67848-120">toomake a Hopping window hello same as a Tumbling window one would simply specify hello hop size toobe hello same as hello window size.</span></span> 

![Stream Analytics okno funkce skákající Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a><span data-ttu-id="67848-122">Posuvné okno</span><span class="sxs-lookup"><span data-stu-id="67848-122">Sliding Window</span></span>
<span data-ttu-id="67848-123">Posuvné okno funkce, na rozdíl od Přeskakujícího nebo posílání windows, vytvořit výstup **pouze** při výskytu události.</span><span class="sxs-lookup"><span data-stu-id="67848-123">Sliding window functions, unlike Tumbling or Hopping windows, produce an output **only**  when an event occurs.</span></span> <span data-ttu-id="67848-124">Každý okno bude mít aspoň jednu událost a okno hello nepřetržitě přesune dál € (Epsilon –).</span><span class="sxs-lookup"><span data-stu-id="67848-124">Every window will have at least one event and hello window continuously moves forward by an € (epsilon).</span></span> <span data-ttu-id="67848-125">Jako posílání Windows může patřit události toomore než jeden klouzavé interval.</span><span class="sxs-lookup"><span data-stu-id="67848-125">Like Hopping Windows, events can belong toomore than one Sliding Window.</span></span>

![Stream Analytics okno funkce klouzavé Úvod](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a><span data-ttu-id="67848-127">Získání nápovědy k okno funkce</span><span class="sxs-lookup"><span data-stu-id="67848-127">Getting help with Window functions</span></span>
<span data-ttu-id="67848-128">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="67848-128">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="67848-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67848-129">Next steps</span></span>
* [<span data-ttu-id="67848-130">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="67848-130">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="67848-131">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="67848-131">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="67848-132">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="67848-132">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="67848-133">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="67848-133">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="67848-134">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="67848-134">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

