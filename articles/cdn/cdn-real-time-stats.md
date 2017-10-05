---
title: "Statistiky v reálném čase v Azure CDN | Microsoft Docs"
description: "Statistiky v reálném čase poskytují v reálném čase údaje o výkonu Azure CDN při doručování obsahu vašim klientům."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c7989340-1172-4315-acbb-186ba34dd52a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e9b9522de6b2c54dc794b00100ffe358296ecfdd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a><span data-ttu-id="3123f-103">Statistiky v reálném čase v Microsoft Azure CDN</span><span class="sxs-lookup"><span data-stu-id="3123f-103">Real-time stats in Microsoft Azure CDN</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="3123f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3123f-104">Overview</span></span>
<span data-ttu-id="3123f-105">Tento dokument popisuje statistiky v reálném čase v Microsoft Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="3123f-105">This document explains real-time stats in Microsoft Azure CDN.</span></span>  <span data-ttu-id="3123f-106">Tato funkce poskytuje data v reálném čase, například šířky pásma, mezipaměti stavy a souběžných připojení na svůj profil CDN při doručování obsahu vašim klientům.</span><span class="sxs-lookup"><span data-stu-id="3123f-106">This functionality provides real-time data, such as bandwidth, cache statuses, and concurrent connections to your CDN profile when delivering content to your clients.</span></span> <span data-ttu-id="3123f-107">To umožňuje nepřetržité monitorování stavu služby v každém okamžiku včetně přejděte živé události.</span><span class="sxs-lookup"><span data-stu-id="3123f-107">This enables continuous monitoring of the health of your service at any time, including go-live events.</span></span>

<span data-ttu-id="3123f-108">K dispozici jsou následující grafy:</span><span class="sxs-lookup"><span data-stu-id="3123f-108">The following graphs are available:</span></span>

* [<span data-ttu-id="3123f-109">Šířka pásma</span><span class="sxs-lookup"><span data-stu-id="3123f-109">Bandwidth</span></span>](#bandwidth)
* [<span data-ttu-id="3123f-110">Stavové kódy</span><span class="sxs-lookup"><span data-stu-id="3123f-110">Status Codes</span></span>](#status-codes)
* [<span data-ttu-id="3123f-111">Stavy mezipaměti</span><span class="sxs-lookup"><span data-stu-id="3123f-111">Cache Statuses</span></span>](#cache-statuses)
* [<span data-ttu-id="3123f-112">Připojení</span><span class="sxs-lookup"><span data-stu-id="3123f-112">Connections</span></span>](#connections)

## <a name="accessing-real-time-stats"></a><span data-ttu-id="3123f-113">Přístup k statistiky v reálném čase</span><span class="sxs-lookup"><span data-stu-id="3123f-113">Accessing real-time stats</span></span>
1. <span data-ttu-id="3123f-114">V [portálu Azure](https://portal.azure.com), přejděte na svůj profil CDN.</span><span class="sxs-lookup"><span data-stu-id="3123f-114">In the [Azure Portal](https://portal.azure.com), browse to your CDN profile.</span></span>
   
    ![Okno profil CDN](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. <span data-ttu-id="3123f-116">Okno profil CDN, klikněte **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3123f-116">From the CDN profile blade, click the **Manage** button.</span></span>
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    <span data-ttu-id="3123f-118">Otevře se na portálu pro správu CDN.</span><span class="sxs-lookup"><span data-stu-id="3123f-118">The CDN management portal opens.</span></span>
3. <span data-ttu-id="3123f-119">Najeďte myší **Analytics** a potom přejděte myší **statistiky v reálném čase** plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="3123f-119">Hover over the **Analytics** tab, then hover over the **Real-Time Stats** flyout.</span></span>  <span data-ttu-id="3123f-120">Klikněte na **velkého objektu HTTP**.</span><span class="sxs-lookup"><span data-stu-id="3123f-120">Click on **HTTP Large Object**.</span></span>
   
    ![Portál pro správu CDN](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    <span data-ttu-id="3123f-122">Se zobrazí v grafech statistiky v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="3123f-122">The real-time stats graphs are displayed.</span></span>

<span data-ttu-id="3123f-123">Každý z v grafech zobrazuje v reálném čase statistiku pro vybrané časové rozpětí, spuštění, pokud stránka načte.</span><span class="sxs-lookup"><span data-stu-id="3123f-123">Each of the graphs displays real-time statistics for the selected time span, starting when the page loads.</span></span>  <span data-ttu-id="3123f-124">V grafech aktualizovat automaticky každých několik sekund.</span><span class="sxs-lookup"><span data-stu-id="3123f-124">The graphs update automatically every few seconds.</span></span>  <span data-ttu-id="3123f-125">**Aktualizovat graf** tlačítko, pokud existuje, dojde k vymazání grafu, po které se zobrazí pouze vybraná data.</span><span class="sxs-lookup"><span data-stu-id="3123f-125">The **Refresh Graph** button, if present, will clear the graph, after which it will only display the selected data.</span></span>

## <a name="bandwidth"></a><span data-ttu-id="3123f-126">Šířka pásma</span><span class="sxs-lookup"><span data-stu-id="3123f-126">Bandwidth</span></span>
![Graf šířky pásma](./media/cdn-real-time-stats/cdn-bandwidth.png)

<span data-ttu-id="3123f-128">**Šířky pásma** grafu zobrazí šířku pásma, které používají pro aktuální platforma pro vybrané časové rozpětí.</span><span class="sxs-lookup"><span data-stu-id="3123f-128">The **Bandwidth** graph displays the amount of bandwidth used for the current platform over the selected time span.</span></span> <span data-ttu-id="3123f-129">Šedou barvou část grafu označuje využití šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="3123f-129">The shaded portion of the graph indicates bandwidth usage.</span></span> <span data-ttu-id="3123f-130">Zobrazí se přesný šířku pásma, které jsou právě používány přímo pod spojnicový graf.</span><span class="sxs-lookup"><span data-stu-id="3123f-130">The exact amount of bandwidth currently being used is displayed directly below the line graph.</span></span>

## <a name="status-codes"></a><span data-ttu-id="3123f-131">Stavové kódy</span><span class="sxs-lookup"><span data-stu-id="3123f-131">Status Codes</span></span>
![Graf kódu stavu](./media/cdn-real-time-stats/cdn-status-codes.png)

<span data-ttu-id="3123f-133">**Stavové kódy** grafu určuje, jak často se provádí některé kódy odpovědi HTTP přes vybrané časové rozpětí.</span><span class="sxs-lookup"><span data-stu-id="3123f-133">The **Status Codes** graph indicates how often certain HTTP response codes are occurring over the selected time span.</span></span>

> [!TIP]
> <span data-ttu-id="3123f-134">Popis jednotlivých možností kód stavu HTTP najdete v tématu [stavové kódy HTTP CDN Azure](https://msdn.microsoft.com/library/mt759238.aspx).</span><span class="sxs-lookup"><span data-stu-id="3123f-134">For a description of each HTTP status code option, see [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx).</span></span>
> 
> 

<span data-ttu-id="3123f-135">Zobrazí se seznam stavové kódy HTTP přímo nad grafu.</span><span class="sxs-lookup"><span data-stu-id="3123f-135">A list of HTTP status codes is displayed directly above the graph.</span></span> <span data-ttu-id="3123f-136">Tento seznam označuje každý stavový kód, který může být součástí spojnicový graf a aktuální počet výskytů za sekundu pro tento stavový kód.</span><span class="sxs-lookup"><span data-stu-id="3123f-136">This list indicates each status code that can be included in the line graph and the current number of occurrences per second for that status code.</span></span> <span data-ttu-id="3123f-137">Ve výchozím nastavení zobrazí se řádek pro každou z těchto stavové kódy v grafu.</span><span class="sxs-lookup"><span data-stu-id="3123f-137">By default, a line is displayed for each of these status codes in the graph.</span></span> <span data-ttu-id="3123f-138">Však můžete monitorovat pouze stavové kódy, které mají zvláštní význam pro konfiguraci CDN.</span><span class="sxs-lookup"><span data-stu-id="3123f-138">However, you can choose to only monitor the status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="3123f-139">K tomu, zkontrolujte požadované stavové kódy a zrušte zaškrtnutí všech ostatních možností a pak klikněte na **aktualizovat graf**.</span><span class="sxs-lookup"><span data-stu-id="3123f-139">To do this, check the desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="3123f-140">Můžete dočasně skrýt data protokolu pro konkrétní stavový kód.</span><span class="sxs-lookup"><span data-stu-id="3123f-140">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="3123f-141">V legendě přímo pod grafem klikněte na kód stavu, které chcete skrýt.</span><span class="sxs-lookup"><span data-stu-id="3123f-141">From the legend directly below the graph, click the status code you want to hide.</span></span> <span data-ttu-id="3123f-142">Stavový kód bude okamžitě skrytá z grafu.</span><span class="sxs-lookup"><span data-stu-id="3123f-142">The status code will be immediately hidden from the graph.</span></span> <span data-ttu-id="3123f-143">Kliknutím na tento stavový kód znovu způsobí, že tato možnost, který se má zobrazit znovu.</span><span class="sxs-lookup"><span data-stu-id="3123f-143">Clicking that status code again will cause that option to be displayed again.</span></span>

## <a name="cache-statuses"></a><span data-ttu-id="3123f-144">Stavy mezipaměti</span><span class="sxs-lookup"><span data-stu-id="3123f-144">Cache Statuses</span></span>
![Stavy graf do mezipaměti](./media/cdn-real-time-stats/cdn-cache-status.png)

<span data-ttu-id="3123f-146">**Mezipaměti stavy** grafu určuje, jak často se provádí některé typy stavů mezipaměti přes vybrané časové rozpětí.</span><span class="sxs-lookup"><span data-stu-id="3123f-146">The **Cache Statuses** graph indicates how often certain types of cache statuses are occurring over the selected time span.</span></span> 

> [!TIP]
> <span data-ttu-id="3123f-147">Popis jednotlivých možností kód stavu mezipaměti najdete v tématu [Azure CDN mezipaměti stavové kódy](https://msdn.microsoft.com/library/mt759237.aspx).</span><span class="sxs-lookup"><span data-stu-id="3123f-147">For a description of each cache status code option, see [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx).</span></span>
> 
> 

<span data-ttu-id="3123f-148">Zobrazí se seznam mezipaměti stavové kódy přímo nad grafu.</span><span class="sxs-lookup"><span data-stu-id="3123f-148">A list of cache status codes is displayed directly above the graph.</span></span> <span data-ttu-id="3123f-149">Tento seznam označuje každý stavový kód, který může být součástí spojnicový graf a aktuální počet výskytů za sekundu pro tento stavový kód.</span><span class="sxs-lookup"><span data-stu-id="3123f-149">This list indicates each status code that can be included in the line graph and the current number of occurrences per second for that status code.</span></span> <span data-ttu-id="3123f-150">Ve výchozím nastavení zobrazí se řádek pro každou z těchto stavové kódy v grafu.</span><span class="sxs-lookup"><span data-stu-id="3123f-150">By default, a line is displayed for each of these status codes in the graph.</span></span> <span data-ttu-id="3123f-151">Však můžete monitorovat pouze stavové kódy, které mají zvláštní význam pro konfiguraci CDN.</span><span class="sxs-lookup"><span data-stu-id="3123f-151">However, you can choose to only monitor the status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="3123f-152">K tomu, zkontrolujte požadované stavové kódy a zrušte zaškrtnutí všech ostatních možností a pak klikněte na **aktualizovat graf**.</span><span class="sxs-lookup"><span data-stu-id="3123f-152">To do this, check the desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="3123f-153">Můžete dočasně skrýt data protokolu pro konkrétní stavový kód.</span><span class="sxs-lookup"><span data-stu-id="3123f-153">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="3123f-154">V legendě přímo pod grafem klikněte na kód stavu, které chcete skrýt.</span><span class="sxs-lookup"><span data-stu-id="3123f-154">From the legend directly below the graph, click the status code you want to hide.</span></span> <span data-ttu-id="3123f-155">Stavový kód bude okamžitě skrytá z grafu.</span><span class="sxs-lookup"><span data-stu-id="3123f-155">The status code will be immediately hidden from the graph.</span></span> <span data-ttu-id="3123f-156">Kliknutím na tento stavový kód znovu způsobí, že tato možnost, který se má zobrazit znovu.</span><span class="sxs-lookup"><span data-stu-id="3123f-156">Clicking that status code again will cause that option to be displayed again.</span></span>

## <a name="connections"></a><span data-ttu-id="3123f-157">Připojení</span><span class="sxs-lookup"><span data-stu-id="3123f-157">Connections</span></span>
![Graf připojení](./media/cdn-real-time-stats/cdn-connections.png)

<span data-ttu-id="3123f-159">Tento graf Určuje, kolik připojení byly vytvořeny pro vaše servery edge.</span><span class="sxs-lookup"><span data-stu-id="3123f-159">This graph indicates how many connections have been established to your edge servers.</span></span> <span data-ttu-id="3123f-160">Každý požadavek pro určitý prostředek, který prochází výsledky našich CDN v připojení.</span><span class="sxs-lookup"><span data-stu-id="3123f-160">Each request for an asset that passes through our CDN results in a connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3123f-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3123f-161">Next Steps</span></span>
* <span data-ttu-id="3123f-162">Upozorňování pomocí [výstrah v reálném čase v Azure CDN](cdn-real-time-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="3123f-162">Get notified with [Real-time alerts in Azure CDN](cdn-real-time-alerts.md)</span></span>
* <span data-ttu-id="3123f-163">Dig hlubší s [Rozšířené sestavy HTTP](cdn-advanced-http-reports.md)</span><span class="sxs-lookup"><span data-stu-id="3123f-163">Dig deeper with [advanced HTTP reports](cdn-advanced-http-reports.md)</span></span>
* <span data-ttu-id="3123f-164">Analýza [vzorce používání](cdn-analyze-usage-patterns.md)</span><span class="sxs-lookup"><span data-stu-id="3123f-164">Analyze [usage patterns](cdn-analyze-usage-patterns.md)</span></span>

