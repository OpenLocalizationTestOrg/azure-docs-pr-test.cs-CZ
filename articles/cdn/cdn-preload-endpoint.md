---
title: "Předběžné načtení prostředků na koncový bod Azure CDN | Microsoft Docs"
description: "Zjistěte, jak předem načíst obsah uložený v mezipaměti na koncový bod Azure CDN."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 1f2dcd9a91bb6e883cbef06373c1acd98bf8d45f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a><span data-ttu-id="13ad2-103">Předběžné načtení prostředků v koncovém bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="13ad2-103">Pre-load assets on an Azure CDN endpoint</span></span>
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

<span data-ttu-id="13ad2-104">Ve výchozím nastavení prostředky nejprve mezipaměti, jako jsou požadovali.</span><span class="sxs-lookup"><span data-stu-id="13ad2-104">By default, assets are first cached as they are requested.</span></span> <span data-ttu-id="13ad2-105">To znamená, že první požadavek každou oblast, může trvat déle, vzhledem k tomu, že nebudete mít servery edge obsah do mezipaměti a bude třeba předat požadavek na původní server.</span><span class="sxs-lookup"><span data-stu-id="13ad2-105">This means that the first request from each region may take longer, since the edge servers will not have the content cached and will need to forward the request to the origin server.</span></span> <span data-ttu-id="13ad2-106">Předběžné načítání obsahu zabraňuje tato první přístupů latence.</span><span class="sxs-lookup"><span data-stu-id="13ad2-106">Pre-loading content avoids this first hit latency.</span></span>

<span data-ttu-id="13ad2-107">Kromě zajištění lepší zkušeností zákazníků, předem načítání vaše prostředky uložené v mezipaměti může také přinést snížení síťový provoz na původním serveru.</span><span class="sxs-lookup"><span data-stu-id="13ad2-107">In addition to providing a better customer experience, pre-loading your cached assets can also reduce network traffic on the origin server.</span></span>

> [!NOTE]
> <span data-ttu-id="13ad2-108">Předběžné načítání prostředků je užitečné pro velké události nebo obsah, který současně dostupné pro velký počet uživatelů, jako je nová verze film nebo aktualizace softwaru.</span><span class="sxs-lookup"><span data-stu-id="13ad2-108">Pre-loading assets is useful for  large events or content that becomes simultaneously available to a large number of users, such as a new movie release or a software update.</span></span>
> 
> 

<span data-ttu-id="13ad2-109">Tento kurz vás provede předběžné načítání obsahu v mezipaměti na všech uzlech edge Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="13ad2-109">This tutorial walks you through pre-loading cached content on all Azure CDN edge nodes.</span></span>

## <a name="walkthrough"></a><span data-ttu-id="13ad2-110">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="13ad2-110">Walkthrough</span></span>
1. <span data-ttu-id="13ad2-111">V [portálu Azure](https://portal.azure.com), vyhledejte profil CDN obsahující chcete předběžné načtení koncový bod.</span><span class="sxs-lookup"><span data-stu-id="13ad2-111">In the [Azure Portal](https://portal.azure.com), browse to the CDN profile containing the endpoint you wish to pre-load.</span></span>  <span data-ttu-id="13ad2-112">Otevře se okno profil.</span><span class="sxs-lookup"><span data-stu-id="13ad2-112">The profile blade opens.</span></span>
2. <span data-ttu-id="13ad2-113">Klikněte na koncový bod v seznamu.</span><span class="sxs-lookup"><span data-stu-id="13ad2-113">Click the endpoint in the list.</span></span>  <span data-ttu-id="13ad2-114">Otevře se okno koncový bod.</span><span class="sxs-lookup"><span data-stu-id="13ad2-114">The endpoint blade opens.</span></span>
3. <span data-ttu-id="13ad2-115">V okně koncového bodu CDN klikněte na tlačítko zatížení.</span><span class="sxs-lookup"><span data-stu-id="13ad2-115">From the CDN endpoint blade, click the load button.</span></span>
   
    ![Okno koncový bod CDN](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    <span data-ttu-id="13ad2-117">Otevře se okno zatížení.</span><span class="sxs-lookup"><span data-stu-id="13ad2-117">The Load blade opens.</span></span>
   
    ![Okno zatížení CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. <span data-ttu-id="13ad2-119">Zadejte úplnou cestu každý prostředek, který chcete načíst (například `/pictures/kitten.png`) v **cesta** textové pole.</span><span class="sxs-lookup"><span data-stu-id="13ad2-119">Enter the full path of each asset you wish to load (e.g., `/pictures/kitten.png`) in the **Path** textbox.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="13ad2-120">Další **cesta** textová pole se zobrazí, když zadáte text, který vám umožní sestavit seznam více prostředků.</span><span class="sxs-lookup"><span data-stu-id="13ad2-120">More **Path** textboxes will appear after you enter text to allow you to build a list of multiple assets.</span></span>  <span data-ttu-id="13ad2-121">Prostředky můžete odstranit ze seznamu kliknutím na tlačítko se třemi tečkami (...).</span><span class="sxs-lookup"><span data-stu-id="13ad2-121">You can delete assets from the list by clicking the ellipsis (...) button.</span></span>
   > 
   > <span data-ttu-id="13ad2-122">Cesty musí být relativní adresa URL, která odpovídá následující [regulární výraz](https://msdn.microsoft.com/library/az24scfc.aspx):</span><span class="sxs-lookup"><span data-stu-id="13ad2-122">Paths must be a relative URL that fits the following [regular expression](https://msdn.microsoft.com/library/az24scfc.aspx):</span></span>  
   > ><span data-ttu-id="13ad2-123">Načíst cestu k souboru jedním `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;</span><span class="sxs-lookup"><span data-stu-id="13ad2-123">Load a single file path `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;</span></span>  
   > ><span data-ttu-id="13ad2-124">Načíst jeden soubor s řetězec dotazu`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`</span><span class="sxs-lookup"><span data-stu-id="13ad2-124">Load a single file with query string `@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`</span></span>  
   > 
   > <span data-ttu-id="13ad2-125">Každý prostředek musí mít svůj vlastní cesta.</span><span class="sxs-lookup"><span data-stu-id="13ad2-125">Each asset must have its own path.</span></span>  <span data-ttu-id="13ad2-126">Není k dispozici žádná funkce zástupných znaků pro předběžné načítání prostředků.</span><span class="sxs-lookup"><span data-stu-id="13ad2-126">There is no wildcard functionality for pre-loading assets.</span></span>
   > 
   > 
   
    ![Tlačítko zatížení](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. <span data-ttu-id="13ad2-128">Klikněte **zatížení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="13ad2-128">Click the **Load** button.</span></span>
   
    ![Tlačítko zatížení](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> <span data-ttu-id="13ad2-130">Existuje omezení 10 zatížení požadavků za minutu za profil CDN.</span><span class="sxs-lookup"><span data-stu-id="13ad2-130">There is a limitation of 10 load requests per minute per CDN profile.</span></span> <span data-ttu-id="13ad2-131">50 cest jsou povoleny na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="13ad2-131">50 paths are allowed per request.</span></span> <span data-ttu-id="13ad2-132">Každá cesta může mít délku cesty 1024 znaků.</span><span class="sxs-lookup"><span data-stu-id="13ad2-132">Each path has a path-length limit of 1024 characters.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="13ad2-133">Viz také</span><span class="sxs-lookup"><span data-stu-id="13ad2-133">See also</span></span>
* [<span data-ttu-id="13ad2-134">Vyprázdnění koncového bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="13ad2-134">Purge an Azure CDN endpoint</span></span>](cdn-purge-endpoint.md)
* [<span data-ttu-id="13ad2-135">Azure referenční dokumentace rozhraní API REST CDN - mazání nebo předběžné načtení koncový bod</span><span class="sxs-lookup"><span data-stu-id="13ad2-135">Azure CDN REST API reference - Purge or Pre-Load an Endpoint</span></span>](https://msdn.microsoft.com/library/mt634451.aspx)

