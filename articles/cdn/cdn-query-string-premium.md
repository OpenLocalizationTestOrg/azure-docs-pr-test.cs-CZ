---
title: "Řízení ukládání do mezipaměti řetězce dotazu - Premium chování Azure CDN | Microsoft Docs"
description: "Azure CDN při ukládání řetězců dotazů ovládací prvky, jak jsou soubory do mezipaměti, které obsahují řetězce dotazu."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 145067c2ce50b41c4783f4de4052c0e7cb529fc7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a><span data-ttu-id="5e1ac-103">Ovládací prvek Azure CDN ukládání do mezipaměti chování řetězce dotazu - Premium</span><span class="sxs-lookup"><span data-stu-id="5e1ac-103">Control Azure CDN caching behavior with query strings - Premium</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5e1ac-104">Standard</span><span class="sxs-lookup"><span data-stu-id="5e1ac-104">Standard</span></span>](cdn-query-string.md)
> * [<span data-ttu-id="5e1ac-105">Azure CDN Premium od společnosti Verizon</span><span class="sxs-lookup"><span data-stu-id="5e1ac-105">Azure CDN Premium from Verizon</span></span>](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="5e1ac-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="5e1ac-106">Overview</span></span>
<span data-ttu-id="5e1ac-107">Řetězec dotazu ukládání do mezipaměti ovládací prvky, jak jsou soubory do mezipaměti, které obsahují řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-107">Query string caching controls how files are to be cached when they contain query strings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5e1ac-108">Standard a Premium CDN produkty poskytovat stejné funkce mezipaměti řetězec dotazu, ale uživatelské rozhraní se liší.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-108">The Standard and Premium CDN products provide the same query string caching functionality, but the user interface differs.</span></span>  <span data-ttu-id="5e1ac-109">Tento dokument popisuje rozhraní pro **Azure CDN Premium od společnosti Verizon**.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-109">This document describes the interface for **Azure CDN Premium from Verizon**.</span></span>  <span data-ttu-id="5e1ac-110">Pro dotaz řetězec ukládání do mezipaměti s **Azure CDN Standard od společnosti Akamai** a **Azure CDN Standard od společnosti Verizon**, najdete v části [řízení chování ukládání do mezipaměti CDN požadavky s řetězci dotazů](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="5e1ac-110">For query string caching with **Azure CDN Standard from Akamai** and **Azure CDN Standard from Verizon**, see [Controlling caching behavior of CDN requests with query strings](cdn-query-string.md).</span></span>
> 
> 

<span data-ttu-id="5e1ac-111">K dispozici jsou tři režimy:</span><span class="sxs-lookup"><span data-stu-id="5e1ac-111">Three modes are available:</span></span>

* <span data-ttu-id="5e1ac-112">**Standardní mezipaměti**: Toto je výchozí režim.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-112">**standard-cache**:  This is the default mode.</span></span>  <span data-ttu-id="5e1ac-113">CDN hraničního uzlu předá řetězec dotazu z žadatel počátek na první požadavek a mezipaměti asset.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-113">The CDN edge node will pass the query string from the requestor to the origin on the first request and cache the asset.</span></span>  <span data-ttu-id="5e1ac-114">Všechny následné žádosti pro tento prostředek, které jsou obsluhovány z uzlu edge bude ignorovat řetězec dotazu do vypršení platnosti mezipaměti asset.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-114">All subsequent requests for that asset that are served from the edge node will ignore the query string until the cached asset expires.</span></span>
* <span data-ttu-id="5e1ac-115">**Ne mezipaměti**: V tomto režimu žádostí s řetězci dotazu se neukládají do mezipaměti v uzlu edge CDN.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-115">**no-cache**:  In this mode, requests with query strings are not cached at the CDN edge node.</span></span>  <span data-ttu-id="5e1ac-116">Hraničního uzlu načte asset přímo z tohoto počátku a předává je pro žadatele s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-116">The edge node retrieves the asset directly from the origin and passes it to the requestor with each request.</span></span>
* <span data-ttu-id="5e1ac-117">**Jedinečný mezipaměti**: Tento režim považuje za jedinečný prostředek s vlastní mezipaměti každou žádost s řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-117">**unique-cache**:  This mode treats each request with a query string as a unique asset with its own cache.</span></span>  <span data-ttu-id="5e1ac-118">Například odpověď z tohoto počátku pro žádost o *foo.ashx?q=bar* by uloží do mezipaměti na uzlu edge a vrátit pro následné mezipaměti s Tento stejný řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-118">For example, the response from the origin for a request for *foo.ashx?q=bar* would be cached at the edge node and returned for subsequent caches with that same query string.</span></span>  <span data-ttu-id="5e1ac-119">Žádost o *foo.ashx?q=somethingelse* by do mezipaměti jako samostatné asset s vlastním časem TTL.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-119">A request for *foo.ashx?q=somethingelse* would be cached as a separate asset with its own time to live.</span></span>

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a><span data-ttu-id="5e1ac-120">Změna nastavení pro profily CDN premium ukládání řetězců s dotazy</span><span class="sxs-lookup"><span data-stu-id="5e1ac-120">Changing query string caching settings for premium CDN profiles</span></span>
1. <span data-ttu-id="5e1ac-121">Okno profil CDN, klikněte **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-121">From the CDN profile blade, click the **Manage** button.</span></span>
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    <span data-ttu-id="5e1ac-123">Otevře se na portálu pro správu CDN.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-123">The CDN management portal opens.</span></span>
2. <span data-ttu-id="5e1ac-124">Najeďte myší **HTTP velké** a potom přejděte myší **nastavení mezipaměti** plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-124">Hover over the **HTTP Large** tab, then hover over the **Cache Settings** flyout.</span></span>  <span data-ttu-id="5e1ac-125">Klikněte na **ukládání do mezipaměti řetězce dotazu**.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-125">Click on **Query-String Caching**.</span></span>
   
    <span data-ttu-id="5e1ac-126">Zobrazí se možnosti ukládání do mezipaměti řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-126">Query string caching options are displayed.</span></span>
   
    ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string-premium/cdn-query-string.png)
3. <span data-ttu-id="5e1ac-128">Po provedení váš výběr, klikněte na **aktualizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-128">After making your selection, click the **Update** button.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5e1ac-129">Změny nastavení nemusí být okamžitě viditelné, protože trvá, než se registrace rozšíří v rámci CDN.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-129">The settings changes may not be immediately visible, as it takes time for the registration to propagate through the CDN.</span></span>  <span data-ttu-id="5e1ac-130">V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.</span><span class="sxs-lookup"><span data-stu-id="5e1ac-130">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
> 
> 

