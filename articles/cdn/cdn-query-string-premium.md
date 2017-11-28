---
title: "aaaControl chování ukládání do mezipaměti Azure CDN s řetězci dotazů - Premium | Microsoft Docs"
description: "Azure CDN při ukládání řetězců dotazů ovládací prvky, jak jsou soubory toobe do mezipaměti, které obsahují řetězce dotazu."
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
ms.openlocfilehash: 5c97cf0230ae13fd378bfce49481f3135a5ef101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a><span data-ttu-id="80284-103">Ovládací prvek Azure CDN ukládání do mezipaměti chování řetězce dotazu - Premium</span><span class="sxs-lookup"><span data-stu-id="80284-103">Control Azure CDN caching behavior with query strings - Premium</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="80284-104">Standard</span><span class="sxs-lookup"><span data-stu-id="80284-104">Standard</span></span>](cdn-query-string.md)
> * [<span data-ttu-id="80284-105">Azure CDN Premium od společnosti Verizon</span><span class="sxs-lookup"><span data-stu-id="80284-105">Azure CDN Premium from Verizon</span></span>](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="80284-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="80284-106">Overview</span></span>
<span data-ttu-id="80284-107">Ovládací prvky, jak jsou soubory v mezipaměti, které obsahují řetězce dotazu toobe ukládání řetězců s dotazy.</span><span class="sxs-lookup"><span data-stu-id="80284-107">Query string caching controls how files are toobe cached when they contain query strings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="80284-108">Hello Standard a Premium CDN produkty poskytují hello stejné funkce ukládání do mezipaměti řetězce dotazu, ale liší se hello uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="80284-108">hello Standard and Premium CDN products provide hello same query string caching functionality, but hello user interface differs.</span></span>  <span data-ttu-id="80284-109">Tento dokument popisuje hello rozhraní pro **Azure CDN Premium od společnosti Verizon**.</span><span class="sxs-lookup"><span data-stu-id="80284-109">This document describes hello interface for **Azure CDN Premium from Verizon**.</span></span>  <span data-ttu-id="80284-110">Pro dotaz řetězec ukládání do mezipaměti s **Azure CDN Standard od společnosti Akamai** a **Azure CDN Standard od společnosti Verizon**, najdete v části [řízení chování ukládání do mezipaměti CDN požadavky s řetězci dotazů](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="80284-110">For query string caching with **Azure CDN Standard from Akamai** and **Azure CDN Standard from Verizon**, see [Controlling caching behavior of CDN requests with query strings](cdn-query-string.md).</span></span>
> 
> 

<span data-ttu-id="80284-111">K dispozici jsou tři režimy:</span><span class="sxs-lookup"><span data-stu-id="80284-111">Three modes are available:</span></span>

* <span data-ttu-id="80284-112">**Standardní mezipaměti**: Toto je výchozí režim hello.</span><span class="sxs-lookup"><span data-stu-id="80284-112">**standard-cache**:  This is hello default mode.</span></span>  <span data-ttu-id="80284-113">Hello CDN hraniční uzel, projdou hello řetězec dotazu z počátku toohello hello žadatele na první požadavek hello a mezipaměti hello asset.</span><span class="sxs-lookup"><span data-stu-id="80284-113">hello CDN edge node will pass hello query string from hello requestor toohello origin on hello first request and cache hello asset.</span></span>  <span data-ttu-id="80284-114">Všechny následné žádosti pro tento prostředek, které se zpracovávají z uzlu edge hello bude ignorovat hello řetězec dotazu do vypršení platnosti mezipaměti asset hello.</span><span class="sxs-lookup"><span data-stu-id="80284-114">All subsequent requests for that asset that are served from hello edge node will ignore hello query string until hello cached asset expires.</span></span>
* <span data-ttu-id="80284-115">**Ne mezipaměti**: V tomto režimu žádostí s řetězci dotazu se neukládají do mezipaměti v uzlu edge hello CDN.</span><span class="sxs-lookup"><span data-stu-id="80284-115">**no-cache**:  In this mode, requests with query strings are not cached at hello CDN edge node.</span></span>  <span data-ttu-id="80284-116">Hello hraniční uzel načte hello asset přímo z počátku hello a předává je toohello žadatele s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="80284-116">hello edge node retrieves hello asset directly from hello origin and passes it toohello requestor with each request.</span></span>
* <span data-ttu-id="80284-117">**Jedinečný mezipaměti**: Tento režim považuje za jedinečný prostředek s vlastní mezipaměti každou žádost s řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="80284-117">**unique-cache**:  This mode treats each request with a query string as a unique asset with its own cache.</span></span>  <span data-ttu-id="80284-118">Například hello odpověď z počátku hello pro žádost o *foo.ashx?q=bar* by uloží do mezipaměti na hello hraniční uzel a vrátit pro následné mezipaměti s Tento stejný řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="80284-118">For example, hello response from hello origin for a request for *foo.ashx?q=bar* would be cached at hello edge node and returned for subsequent caches with that same query string.</span></span>  <span data-ttu-id="80284-119">Žádost o *foo.ashx?q=somethingelse* by do mezipaměti jako samostatné asset s vlastním toolive časem.</span><span class="sxs-lookup"><span data-stu-id="80284-119">A request for *foo.ashx?q=somethingelse* would be cached as a separate asset with its own time toolive.</span></span>

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a><span data-ttu-id="80284-120">Změna nastavení pro profily CDN premium ukládání řetězců s dotazy</span><span class="sxs-lookup"><span data-stu-id="80284-120">Changing query string caching settings for premium CDN profiles</span></span>
1. <span data-ttu-id="80284-121">Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="80284-121">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    <span data-ttu-id="80284-123">Otevře portál pro správu Hello CDN.</span><span class="sxs-lookup"><span data-stu-id="80284-123">hello CDN management portal opens.</span></span>
2. <span data-ttu-id="80284-124">Hover přes hello **HTTP velké** kartu a potom hover přes hello **nastavení mezipaměti** plovoucím panelem.</span><span class="sxs-lookup"><span data-stu-id="80284-124">Hover over hello **HTTP Large** tab, then hover over hello **Cache Settings** flyout.</span></span>  <span data-ttu-id="80284-125">Klikněte na **ukládání do mezipaměti řetězce dotazu**.</span><span class="sxs-lookup"><span data-stu-id="80284-125">Click on **Query-String Caching**.</span></span>
   
    <span data-ttu-id="80284-126">Zobrazí se možnosti ukládání do mezipaměti řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="80284-126">Query string caching options are displayed.</span></span>
   
    ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string-premium/cdn-query-string.png)
3. <span data-ttu-id="80284-128">Po provedení váš výběr, klikněte na hello **aktualizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="80284-128">After making your selection, click hello **Update** button.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="80284-129">změny nastavení Hello nemusí být okamžitě viditelné, jak dlouho trvá dobu hello registrace toopropagate prostřednictvím hello CDN.</span><span class="sxs-lookup"><span data-stu-id="80284-129">hello settings changes may not be immediately visible, as it takes time for hello registration toopropagate through hello CDN.</span></span>  <span data-ttu-id="80284-130">V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.</span><span class="sxs-lookup"><span data-stu-id="80284-130">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
> 
> 

