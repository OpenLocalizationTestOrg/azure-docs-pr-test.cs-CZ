---
title: "Řízení ukládání do mezipaměti řetězce dotazu chování Azure CDN | Microsoft Docs"
description: "Azure CDN při ukládání řetězců dotazů ovládací prvky, jak jsou soubory do mezipaměti, které obsahují řetězce dotazu."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 8d79626fa8516f226a82d3dac693c2033904c91d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a><span data-ttu-id="80496-103">Ovládací prvek Azure CDN ukládání do mezipaměti chování řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="80496-103">Control Azure CDN caching behavior with query strings</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="80496-104">Standard</span><span class="sxs-lookup"><span data-stu-id="80496-104">Standard</span></span>](cdn-query-string.md)
> * [<span data-ttu-id="80496-105">Azure CDN Premium od společnosti Verizon</span><span class="sxs-lookup"><span data-stu-id="80496-105">Azure CDN Premium from Verizon</span></span>](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="80496-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="80496-106">Overview</span></span>
<span data-ttu-id="80496-107">Řetězec dotazu ukládání do mezipaměti ovládací prvky, jak jsou soubory do mezipaměti, které obsahují řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="80496-107">Query string caching controls how files are to be cached when they contain query strings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="80496-108">Standard a Premium CDN produkty poskytovat stejné funkce mezipaměti řetězec dotazu, ale uživatelské rozhraní se liší.</span><span class="sxs-lookup"><span data-stu-id="80496-108">The Standard and Premium CDN products provide the same query string caching functionality, but the user interface differs.</span></span>  <span data-ttu-id="80496-109">Tento dokument popisuje rozhraní pro **Azure CDN Standard od společnosti Akamai** a **Azure CDN Standard od společnosti Verizon**.</span><span class="sxs-lookup"><span data-stu-id="80496-109">This document describes the interface for **Azure CDN Standard from Akamai** and **Azure CDN Standard from Verizon**.</span></span>  <span data-ttu-id="80496-110">Pro dotaz řetězec ukládání do mezipaměti s **Azure CDN Premium od společnosti Verizon**, najdete v části [řízení chování ukládání do mezipaměti CDN požadavky s řetězci dotazů - Premium](cdn-query-string-premium.md).</span><span class="sxs-lookup"><span data-stu-id="80496-110">For query string caching with **Azure CDN Premium from Verizon**, see [Controlling caching behavior of CDN requests with query strings - Premium](cdn-query-string-premium.md).</span></span>
> 
> 

<span data-ttu-id="80496-111">K dispozici jsou tři režimy:</span><span class="sxs-lookup"><span data-stu-id="80496-111">Three modes are available:</span></span>

* <span data-ttu-id="80496-112">**Ignorovat řetězce dotazů**: Toto je výchozí režim.</span><span class="sxs-lookup"><span data-stu-id="80496-112">**Ignore query strings**:  This is the default mode.</span></span>  <span data-ttu-id="80496-113">CDN hraničního uzlu předá řetězec dotazu z žadatel počátek na první požadavek a mezipaměti asset.</span><span class="sxs-lookup"><span data-stu-id="80496-113">The CDN edge node will pass the query string from the requestor to the origin on the first request and cache the asset.</span></span>  <span data-ttu-id="80496-114">Všechny následné žádosti pro tento prostředek, které jsou obsluhovány z uzlu edge bude ignorovat řetězec dotazu do vypršení platnosti mezipaměti asset.</span><span class="sxs-lookup"><span data-stu-id="80496-114">All subsequent requests for that asset that are served from the edge node will ignore the query string until the cached asset expires.</span></span>
* <span data-ttu-id="80496-115">**Nepoužívat ukládání do mezipaměti pro URL s řetězci dotazů**: V tomto režimu žádostí s řetězci dotazu se neukládají do mezipaměti v uzlu edge CDN.</span><span class="sxs-lookup"><span data-stu-id="80496-115">**Bypass caching for URL with query strings**:  In this mode, requests with query strings are not cached at the CDN edge node.</span></span>  <span data-ttu-id="80496-116">Hraničního uzlu načte asset přímo z tohoto počátku a předává je pro žadatele s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="80496-116">The edge node retrieves the asset directly from the origin and passes it to the requestor with each request.</span></span>
* <span data-ttu-id="80496-117">**Ukládat do mezipaměti každou jedinečnou adresu URL**: Tento režim považuje za jedinečný prostředek s vlastní mezipaměti každou žádost s řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="80496-117">**Cache every unique URL**:  This mode treats each request with a query string as a unique asset with its own cache.</span></span>  <span data-ttu-id="80496-118">Například odpověď z tohoto počátku pro žádost o *foo.ashx?q=bar* by uloží do mezipaměti na uzlu edge a vrátit pro následné mezipaměti s Tento stejný řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="80496-118">For example, the response from the origin for a request for *foo.ashx?q=bar* would be cached at the edge node and returned for subsequent caches with that same query string.</span></span>  <span data-ttu-id="80496-119">Žádost o *foo.ashx?q=somethingelse* by do mezipaměti jako samostatné asset s vlastním časem TTL.</span><span class="sxs-lookup"><span data-stu-id="80496-119">A request for *foo.ashx?q=somethingelse* would be cached as a separate asset with its own time to live.</span></span>

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a><span data-ttu-id="80496-120">Změna nastavení pro standardní profily CDN ukládání řetězců s dotazy</span><span class="sxs-lookup"><span data-stu-id="80496-120">Changing query string caching settings for standard CDN profiles</span></span>
1. <span data-ttu-id="80496-121">Z okno profil CDN klikněte na koncový bod CDN, které chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="80496-121">From the CDN profile blade, click the CDN endpoint you wish to manage.</span></span>
   
    ![Koncové body okno profil CDN](./media/cdn-query-string/cdn-endpoints.png)
   
    <span data-ttu-id="80496-123">Otevře se okno koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="80496-123">The CDN endpoint blade opens.</span></span>
2. <span data-ttu-id="80496-124">Klikněte **konfigurace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="80496-124">Click the **Configure** button.</span></span>
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-query-string/cdn-config-btn.png)
   
    <span data-ttu-id="80496-126">Otevře se okno Konfigurace CDN.</span><span class="sxs-lookup"><span data-stu-id="80496-126">The CDN Configuration blade opens.</span></span>
3. <span data-ttu-id="80496-127">Vyberte nastavení z **chování ukládání řetězců s dotazy** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="80496-127">Select a setting from the **Query string caching behavior** dropdown.</span></span>
   
    ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string/cdn-query-string.png)
4. <span data-ttu-id="80496-129">Po provedení váš výběr, klikněte na **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="80496-129">After making your selection, click the **Save** button.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="80496-130">Změny nastavení nemusí být okamžitě viditelné, protože trvá, než se registrace rozšíří v rámci CDN.</span><span class="sxs-lookup"><span data-stu-id="80496-130">The settings changes may not be immediately visible, as it takes time for the registration to propagate through the CDN.</span></span>  <span data-ttu-id="80496-131">V případě profilů <b>Azure CDN společnosti Akamai</b> je šíření obvykle hotové během jedné minuty.</span><span class="sxs-lookup"><span data-stu-id="80496-131">For <b>Azure CDN from Akamai</b> profiles, propagation will usually complete within one minute.</span></span>  <span data-ttu-id="80496-132">V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.</span><span class="sxs-lookup"><span data-stu-id="80496-132">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
> 
> 

