---
title: "aaaControl chování ukládání do mezipaměti Azure CDN řetězce dotazu | Microsoft Docs"
description: "Azure CDN při ukládání řetězců dotazů ovládací prvky, jak jsou soubory toobe do mezipaměti, které obsahují řetězce dotazu."
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
ms.openlocfilehash: e7a138b2decec624a29eb703ad9a291d19c44ee8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a><span data-ttu-id="58d83-103">Ovládací prvek Azure CDN ukládání do mezipaměti chování řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="58d83-103">Control Azure CDN caching behavior with query strings</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="58d83-104">Standard</span><span class="sxs-lookup"><span data-stu-id="58d83-104">Standard</span></span>](cdn-query-string.md)
> * [<span data-ttu-id="58d83-105">Azure CDN Premium od společnosti Verizon</span><span class="sxs-lookup"><span data-stu-id="58d83-105">Azure CDN Premium from Verizon</span></span>](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="58d83-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="58d83-106">Overview</span></span>
<span data-ttu-id="58d83-107">Ovládací prvky, jak jsou soubory v mezipaměti, které obsahují řetězce dotazu toobe ukládání řetězců s dotazy.</span><span class="sxs-lookup"><span data-stu-id="58d83-107">Query string caching controls how files are toobe cached when they contain query strings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="58d83-108">Hello Standard a Premium CDN produkty poskytují hello stejné funkce ukládání do mezipaměti řetězce dotazu, ale liší se hello uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="58d83-108">hello Standard and Premium CDN products provide hello same query string caching functionality, but hello user interface differs.</span></span>  <span data-ttu-id="58d83-109">Tento dokument popisuje hello rozhraní pro **Azure CDN Standard od společnosti Akamai** a **Azure CDN Standard od společnosti Verizon**.</span><span class="sxs-lookup"><span data-stu-id="58d83-109">This document describes hello interface for **Azure CDN Standard from Akamai** and **Azure CDN Standard from Verizon**.</span></span>  <span data-ttu-id="58d83-110">Pro dotaz řetězec ukládání do mezipaměti s **Azure CDN Premium od společnosti Verizon**, najdete v části [řízení chování ukládání do mezipaměti CDN požadavky s řetězci dotazů - Premium](cdn-query-string-premium.md).</span><span class="sxs-lookup"><span data-stu-id="58d83-110">For query string caching with **Azure CDN Premium from Verizon**, see [Controlling caching behavior of CDN requests with query strings - Premium](cdn-query-string-premium.md).</span></span>
> 
> 

<span data-ttu-id="58d83-111">K dispozici jsou tři režimy:</span><span class="sxs-lookup"><span data-stu-id="58d83-111">Three modes are available:</span></span>

* <span data-ttu-id="58d83-112">**Ignorovat řetězce dotazů**: Toto je výchozí režim hello.</span><span class="sxs-lookup"><span data-stu-id="58d83-112">**Ignore query strings**:  This is hello default mode.</span></span>  <span data-ttu-id="58d83-113">Hello CDN hraniční uzel, projdou hello řetězec dotazu z počátku toohello hello žadatele na první požadavek hello a mezipaměti hello asset.</span><span class="sxs-lookup"><span data-stu-id="58d83-113">hello CDN edge node will pass hello query string from hello requestor toohello origin on hello first request and cache hello asset.</span></span>  <span data-ttu-id="58d83-114">Všechny následné žádosti pro tento prostředek, které se zpracovávají z uzlu edge hello bude ignorovat hello řetězec dotazu do vypršení platnosti mezipaměti asset hello.</span><span class="sxs-lookup"><span data-stu-id="58d83-114">All subsequent requests for that asset that are served from hello edge node will ignore hello query string until hello cached asset expires.</span></span>
* <span data-ttu-id="58d83-115">**Nepoužívat ukládání do mezipaměti pro URL s řetězci dotazů**: V tomto režimu žádostí s řetězci dotazu se neukládají do mezipaměti v uzlu edge hello CDN.</span><span class="sxs-lookup"><span data-stu-id="58d83-115">**Bypass caching for URL with query strings**:  In this mode, requests with query strings are not cached at hello CDN edge node.</span></span>  <span data-ttu-id="58d83-116">Hello hraniční uzel načte hello asset přímo z počátku hello a předává je toohello žadatele s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="58d83-116">hello edge node retrieves hello asset directly from hello origin and passes it toohello requestor with each request.</span></span>
* <span data-ttu-id="58d83-117">**Ukládat do mezipaměti každou jedinečnou adresu URL**: Tento režim považuje za jedinečný prostředek s vlastní mezipaměti každou žádost s řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="58d83-117">**Cache every unique URL**:  This mode treats each request with a query string as a unique asset with its own cache.</span></span>  <span data-ttu-id="58d83-118">Například hello odpověď z počátku hello pro žádost o *foo.ashx?q=bar* by uloží do mezipaměti na hello hraniční uzel a vrátit pro následné mezipaměti s Tento stejný řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="58d83-118">For example, hello response from hello origin for a request for *foo.ashx?q=bar* would be cached at hello edge node and returned for subsequent caches with that same query string.</span></span>  <span data-ttu-id="58d83-119">Žádost o *foo.ashx?q=somethingelse* by do mezipaměti jako samostatné asset s vlastním toolive časem.</span><span class="sxs-lookup"><span data-stu-id="58d83-119">A request for *foo.ashx?q=somethingelse* would be cached as a separate asset with its own time toolive.</span></span>

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a><span data-ttu-id="58d83-120">Změna nastavení pro standardní profily CDN ukládání řetězců s dotazy</span><span class="sxs-lookup"><span data-stu-id="58d83-120">Changing query string caching settings for standard CDN profiles</span></span>
1. <span data-ttu-id="58d83-121">Z hello okno profil CDN klikněte na koncový bod CDN hello chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="58d83-121">From hello CDN profile blade, click hello CDN endpoint you wish toomanage.</span></span>
   
    ![Koncové body okno profil CDN](./media/cdn-query-string/cdn-endpoints.png)
   
    <span data-ttu-id="58d83-123">Otevře se okno koncový bod CDN Hello.</span><span class="sxs-lookup"><span data-stu-id="58d83-123">hello CDN endpoint blade opens.</span></span>
2. <span data-ttu-id="58d83-124">Klikněte na tlačítko hello **konfigurace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="58d83-124">Click hello **Configure** button.</span></span>
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-query-string/cdn-config-btn.png)
   
    <span data-ttu-id="58d83-126">Otevře se okno Konfigurace CDN Hello.</span><span class="sxs-lookup"><span data-stu-id="58d83-126">hello CDN Configuration blade opens.</span></span>
3. <span data-ttu-id="58d83-127">Vyberte nastavení hello **chování ukládání řetězců s dotazy** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="58d83-127">Select a setting from hello **Query string caching behavior** dropdown.</span></span>
   
    ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string/cdn-query-string.png)
4. <span data-ttu-id="58d83-129">Po provedení váš výběr, klikněte na hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="58d83-129">After making your selection, click hello **Save** button.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="58d83-130">změny nastavení Hello nemusí být okamžitě viditelné, jak dlouho trvá dobu hello registrace toopropagate prostřednictvím hello CDN.</span><span class="sxs-lookup"><span data-stu-id="58d83-130">hello settings changes may not be immediately visible, as it takes time for hello registration toopropagate through hello CDN.</span></span>  <span data-ttu-id="58d83-131">V případě profilů <b>Azure CDN společnosti Akamai</b> je šíření obvykle hotové během jedné minuty.</span><span class="sxs-lookup"><span data-stu-id="58d83-131">For <b>Azure CDN from Akamai</b> profiles, propagation will usually complete within one minute.</span></span>  <span data-ttu-id="58d83-132">V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.</span><span class="sxs-lookup"><span data-stu-id="58d83-132">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
> 
> 

