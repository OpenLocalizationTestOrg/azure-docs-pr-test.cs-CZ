---
title: "statistiky aaaReal čas v Azure CDN | Microsoft Docs"
description: "Statistiky v reálném čase poskytuje data o výkonu hello Azure CDN v reálném čase, při doručování obsahu tooyour klientů."
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
ms.openlocfilehash: 68900a5092b767e45c1fdf9cef2cd03f55f38a6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a><span data-ttu-id="a0440-103">Statistiky v reálném čase v Microsoft Azure CDN</span><span class="sxs-lookup"><span data-stu-id="a0440-103">Real-time stats in Microsoft Azure CDN</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="a0440-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="a0440-104">Overview</span></span>
<span data-ttu-id="a0440-105">Tento dokument popisuje statistiky v reálném čase v Microsoft Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="a0440-105">This document explains real-time stats in Microsoft Azure CDN.</span></span>  <span data-ttu-id="a0440-106">Tato funkce poskytuje data v reálném čase, například šířku pásma, mezipaměti stavy a souběžných připojení tooyour CDN profilu při doručování obsahu tooyour klientů.</span><span class="sxs-lookup"><span data-stu-id="a0440-106">This functionality provides real-time data, such as bandwidth, cache statuses, and concurrent connections tooyour CDN profile when delivering content tooyour clients.</span></span> <span data-ttu-id="a0440-107">To umožňuje nepřetržité monitorování stavu hello služby kdykoli, včetně přejděte živé události.</span><span class="sxs-lookup"><span data-stu-id="a0440-107">This enables continuous monitoring of hello health of your service at any time, including go-live events.</span></span>

<span data-ttu-id="a0440-108">k dispozici jsou následující grafy Hello:</span><span class="sxs-lookup"><span data-stu-id="a0440-108">hello following graphs are available:</span></span>

* [<span data-ttu-id="a0440-109">Šířka pásma</span><span class="sxs-lookup"><span data-stu-id="a0440-109">Bandwidth</span></span>](#bandwidth)
* [<span data-ttu-id="a0440-110">Stavové kódy</span><span class="sxs-lookup"><span data-stu-id="a0440-110">Status Codes</span></span>](#status-codes)
* [<span data-ttu-id="a0440-111">Stavy mezipaměti</span><span class="sxs-lookup"><span data-stu-id="a0440-111">Cache Statuses</span></span>](#cache-statuses)
* [<span data-ttu-id="a0440-112">Připojení</span><span class="sxs-lookup"><span data-stu-id="a0440-112">Connections</span></span>](#connections)

## <a name="accessing-real-time-stats"></a><span data-ttu-id="a0440-113">Přístup k statistiky v reálném čase</span><span class="sxs-lookup"><span data-stu-id="a0440-113">Accessing real-time stats</span></span>
1. <span data-ttu-id="a0440-114">V hello [portálu Azure](https://portal.azure.com), procházet tooyour profil CDN.</span><span class="sxs-lookup"><span data-stu-id="a0440-114">In hello [Azure Portal](https://portal.azure.com), browse tooyour CDN profile.</span></span>
   
    ![Okno profil CDN](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. <span data-ttu-id="a0440-116">Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a0440-116">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    <span data-ttu-id="a0440-118">Otevře portál pro správu Hello CDN.</span><span class="sxs-lookup"><span data-stu-id="a0440-118">hello CDN management portal opens.</span></span>
3. <span data-ttu-id="a0440-119">Hover přes hello **Analytics** kartu a potom hover přes hello **statistiky v reálném čase** plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="a0440-119">Hover over hello **Analytics** tab, then hover over hello **Real-Time Stats** flyout.</span></span>  <span data-ttu-id="a0440-120">Klikněte na **velkého objektu HTTP**.</span><span class="sxs-lookup"><span data-stu-id="a0440-120">Click on **HTTP Large Object**.</span></span>
   
    ![Portál pro správu CDN](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    <span data-ttu-id="a0440-122">Zobrazí se grafy Hello statistiky v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="a0440-122">hello real-time stats graphs are displayed.</span></span>

<span data-ttu-id="a0440-123">Každý z hello grafy zobrazuje v reálném čase statistiku pro hello vybrané časové období, spuštění, pokud stránka hello načte.</span><span class="sxs-lookup"><span data-stu-id="a0440-123">Each of hello graphs displays real-time statistics for hello selected time span, starting when hello page loads.</span></span>  <span data-ttu-id="a0440-124">grafy Hello aktualizovat automaticky každých několik sekund.</span><span class="sxs-lookup"><span data-stu-id="a0440-124">hello graphs update automatically every few seconds.</span></span>  <span data-ttu-id="a0440-125">Hello **aktualizovat graf** tlačítko, pokud existuje, dojde k vymazání hello grafu, po které se zobrazí jenom hello vybraná data.</span><span class="sxs-lookup"><span data-stu-id="a0440-125">hello **Refresh Graph** button, if present, will clear hello graph, after which it will only display hello selected data.</span></span>

## <a name="bandwidth"></a><span data-ttu-id="a0440-126">Šířka pásma</span><span class="sxs-lookup"><span data-stu-id="a0440-126">Bandwidth</span></span>
![Graf šířky pásma](./media/cdn-real-time-stats/cdn-bandwidth.png)

<span data-ttu-id="a0440-128">Hello **šířky pásma** grafu zobrazí hello šířku pásma použít pro aktuální platformě hello přes hello vybrané časové období.</span><span class="sxs-lookup"><span data-stu-id="a0440-128">hello **Bandwidth** graph displays hello amount of bandwidth used for hello current platform over hello selected time span.</span></span> <span data-ttu-id="a0440-129">Hello šedou barvou část grafu hello označuje využití šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="a0440-129">hello shaded portion of hello graph indicates bandwidth usage.</span></span> <span data-ttu-id="a0440-130">přesné množství šířky pásma, které jsou právě používány Hello se zobrazí pod hello spojnicový graf.</span><span class="sxs-lookup"><span data-stu-id="a0440-130">hello exact amount of bandwidth currently being used is displayed directly below hello line graph.</span></span>

## <a name="status-codes"></a><span data-ttu-id="a0440-131">Stavové kódy</span><span class="sxs-lookup"><span data-stu-id="a0440-131">Status Codes</span></span>
![Graf kódu stavu](./media/cdn-real-time-stats/cdn-status-codes.png)

<span data-ttu-id="a0440-133">Hello **stavové kódy** grafu určuje, jak často se provádí některé kódy odpovědi HTTP přes hello vybrané časové období.</span><span class="sxs-lookup"><span data-stu-id="a0440-133">hello **Status Codes** graph indicates how often certain HTTP response codes are occurring over hello selected time span.</span></span>

> [!TIP]
> <span data-ttu-id="a0440-134">Popis jednotlivých možností kód stavu HTTP najdete v tématu [stavové kódy HTTP CDN Azure](https://msdn.microsoft.com/library/mt759238.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0440-134">For a description of each HTTP status code option, see [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx).</span></span>
> 
> 

<span data-ttu-id="a0440-135">Zobrazí se seznam stavové kódy HTTP přímo nad hello grafu.</span><span class="sxs-lookup"><span data-stu-id="a0440-135">A list of HTTP status codes is displayed directly above hello graph.</span></span> <span data-ttu-id="a0440-136">Tento seznam označuje každý kód stavu, které můžou být součástí hello spojnicový graf a hello aktuální počet výskytů za sekundu pro tento stavový kód.</span><span class="sxs-lookup"><span data-stu-id="a0440-136">This list indicates each status code that can be included in hello line graph and hello current number of occurrences per second for that status code.</span></span> <span data-ttu-id="a0440-137">Ve výchozím nastavení zobrazí se řádek pro každou z těchto stavové kódy v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="a0440-137">By default, a line is displayed for each of these status codes in hello graph.</span></span> <span data-ttu-id="a0440-138">Můžete však zvolit tooonly monitorování hello stavové kódy, které mají zvláštní význam pro konfiguraci CDN.</span><span class="sxs-lookup"><span data-stu-id="a0440-138">However, you can choose tooonly monitor hello status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="a0440-139">toodo, zkontrolujte hello potřeby stavové kódy a zrušte zaškrtnutí všech ostatních možností a potom klikněte na tlačítko **aktualizovat graf**.</span><span class="sxs-lookup"><span data-stu-id="a0440-139">toodo this, check hello desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="a0440-140">Můžete dočasně skrýt data protokolu pro konkrétní stavový kód.</span><span class="sxs-lookup"><span data-stu-id="a0440-140">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="a0440-141">Z hello legendy přímo pod hello grafu klikněte na tlačítko chcete toohide hello stavový kód.</span><span class="sxs-lookup"><span data-stu-id="a0440-141">From hello legend directly below hello graph, click hello status code you want toohide.</span></span> <span data-ttu-id="a0440-142">Hello stavový kód bude okamžitě skrytá z hello grafu.</span><span class="sxs-lookup"><span data-stu-id="a0440-142">hello status code will be immediately hidden from hello graph.</span></span> <span data-ttu-id="a0440-143">Kliknutím na tento stavový kód znovu způsobí, že tuto možnost toobe zobrazí znovu.</span><span class="sxs-lookup"><span data-stu-id="a0440-143">Clicking that status code again will cause that option toobe displayed again.</span></span>

## <a name="cache-statuses"></a><span data-ttu-id="a0440-144">Stavy mezipaměti</span><span class="sxs-lookup"><span data-stu-id="a0440-144">Cache Statuses</span></span>
![Stavy graf do mezipaměti](./media/cdn-real-time-stats/cdn-cache-status.png)

<span data-ttu-id="a0440-146">Hello **mezipaměti stavy** grafu určuje, jak často se provádí některé typy stavů mezipaměti přes hello vybrané časové období.</span><span class="sxs-lookup"><span data-stu-id="a0440-146">hello **Cache Statuses** graph indicates how often certain types of cache statuses are occurring over hello selected time span.</span></span> 

> [!TIP]
> <span data-ttu-id="a0440-147">Popis jednotlivých možností kód stavu mezipaměti najdete v tématu [Azure CDN mezipaměti stavové kódy](https://msdn.microsoft.com/library/mt759237.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0440-147">For a description of each cache status code option, see [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx).</span></span>
> 
> 

<span data-ttu-id="a0440-148">Zobrazí se seznam mezipaměti stavové kódy přímo nad hello grafu.</span><span class="sxs-lookup"><span data-stu-id="a0440-148">A list of cache status codes is displayed directly above hello graph.</span></span> <span data-ttu-id="a0440-149">Tento seznam označuje každý kód stavu, které můžou být součástí hello spojnicový graf a hello aktuální počet výskytů za sekundu pro tento stavový kód.</span><span class="sxs-lookup"><span data-stu-id="a0440-149">This list indicates each status code that can be included in hello line graph and hello current number of occurrences per second for that status code.</span></span> <span data-ttu-id="a0440-150">Ve výchozím nastavení zobrazí se řádek pro každou z těchto stavové kódy v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="a0440-150">By default, a line is displayed for each of these status codes in hello graph.</span></span> <span data-ttu-id="a0440-151">Můžete však zvolit tooonly monitorování hello stavové kódy, které mají zvláštní význam pro konfiguraci CDN.</span><span class="sxs-lookup"><span data-stu-id="a0440-151">However, you can choose tooonly monitor hello status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="a0440-152">toodo, zkontrolujte hello potřeby stavové kódy a zrušte zaškrtnutí všech ostatních možností a potom klikněte na tlačítko **aktualizovat graf**.</span><span class="sxs-lookup"><span data-stu-id="a0440-152">toodo this, check hello desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="a0440-153">Můžete dočasně skrýt data protokolu pro konkrétní stavový kód.</span><span class="sxs-lookup"><span data-stu-id="a0440-153">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="a0440-154">Z hello legendy přímo pod hello grafu klikněte na tlačítko chcete toohide hello stavový kód.</span><span class="sxs-lookup"><span data-stu-id="a0440-154">From hello legend directly below hello graph, click hello status code you want toohide.</span></span> <span data-ttu-id="a0440-155">Hello stavový kód bude okamžitě skrytá z hello grafu.</span><span class="sxs-lookup"><span data-stu-id="a0440-155">hello status code will be immediately hidden from hello graph.</span></span> <span data-ttu-id="a0440-156">Kliknutím na tento stavový kód znovu způsobí, že tuto možnost toobe zobrazí znovu.</span><span class="sxs-lookup"><span data-stu-id="a0440-156">Clicking that status code again will cause that option toobe displayed again.</span></span>

## <a name="connections"></a><span data-ttu-id="a0440-157">Připojení</span><span class="sxs-lookup"><span data-stu-id="a0440-157">Connections</span></span>
![Graf připojení](./media/cdn-real-time-stats/cdn-connections.png)

<span data-ttu-id="a0440-159">Tento graf Určuje, kolik připojení byly zavedené tooyour hraniční servery.</span><span class="sxs-lookup"><span data-stu-id="a0440-159">This graph indicates how many connections have been established tooyour edge servers.</span></span> <span data-ttu-id="a0440-160">Každý požadavek pro určitý prostředek, který prochází výsledky našich CDN v připojení.</span><span class="sxs-lookup"><span data-stu-id="a0440-160">Each request for an asset that passes through our CDN results in a connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0440-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a0440-161">Next Steps</span></span>
* <span data-ttu-id="a0440-162">Upozorňování pomocí [výstrah v reálném čase v Azure CDN](cdn-real-time-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="a0440-162">Get notified with [Real-time alerts in Azure CDN](cdn-real-time-alerts.md)</span></span>
* <span data-ttu-id="a0440-163">Dig hlubší s [Rozšířené sestavy HTTP](cdn-advanced-http-reports.md)</span><span class="sxs-lookup"><span data-stu-id="a0440-163">Dig deeper with [advanced HTTP reports](cdn-advanced-http-reports.md)</span></span>
* <span data-ttu-id="a0440-164">Analýza [vzorce používání](cdn-analyze-usage-patterns.md)</span><span class="sxs-lookup"><span data-stu-id="a0440-164">Analyze [usage patterns](cdn-analyze-usage-patterns.md)</span></span>

