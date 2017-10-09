---
title: "prostředky aaaPre zatížení na koncový bod Azure CDN | Microsoft Docs"
description: "Zjistěte, jak toopre zatížení mezipaměti obsahu na koncový bod Azure CDN."
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
ms.openlocfilehash: 08ac4b834f1ac8ce59d22e65fa8adea11bafcf17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a><span data-ttu-id="94d0a-103">Předběžné načtení prostředků v koncovém bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="94d0a-103">Pre-load assets on an Azure CDN endpoint</span></span>
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

<span data-ttu-id="94d0a-104">Ve výchozím nastavení prostředky nejprve mezipaměti, jako jsou požadovali.</span><span class="sxs-lookup"><span data-stu-id="94d0a-104">By default, assets are first cached as they are requested.</span></span> <span data-ttu-id="94d0a-105">To znamená, že první požadavek hello z každé oblasti může trvat déle, vzhledem k tomu, že nebude mít servery edge hello hello obsah do mezipaměti a bude nutné tooforward hello požadavek toohello zdrojový server.</span><span class="sxs-lookup"><span data-stu-id="94d0a-105">This means that hello first request from each region may take longer, since hello edge servers will not have hello content cached and will need tooforward hello request toohello origin server.</span></span> <span data-ttu-id="94d0a-106">Předběžné načítání obsahu zabraňuje tato první přístupů latence.</span><span class="sxs-lookup"><span data-stu-id="94d0a-106">Pre-loading content avoids this first hit latency.</span></span>

<span data-ttu-id="94d0a-107">Kromě toho tooproviding lepší zkušeností zákazníků, předem načítání vaše prostředky uložené v mezipaměti může také přinést snížení síťový provoz na původním serveru hello.</span><span class="sxs-lookup"><span data-stu-id="94d0a-107">In addition tooproviding a better customer experience, pre-loading your cached assets can also reduce network traffic on hello origin server.</span></span>

> [!NOTE]
> <span data-ttu-id="94d0a-108">Předběžné načítání prostředků je užitečné pro velké události nebo obsahu, který bude současně dostupné tooa velký počet uživatelů, jako je nová verze film nebo aktualizace softwaru.</span><span class="sxs-lookup"><span data-stu-id="94d0a-108">Pre-loading assets is useful for  large events or content that becomes simultaneously available tooa large number of users, such as a new movie release or a software update.</span></span>
> 
> 

<span data-ttu-id="94d0a-109">Tento kurz vás provede předběžné načítání obsahu v mezipaměti na všech uzlech edge Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="94d0a-109">This tutorial walks you through pre-loading cached content on all Azure CDN edge nodes.</span></span>

## <a name="walkthrough"></a><span data-ttu-id="94d0a-110">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="94d0a-110">Walkthrough</span></span>
1. <span data-ttu-id="94d0a-111">V hello [portálu Azure](https://portal.azure.com), procházet profil CDN toohello obsahující koncový bod hello chcete toopre zatížení.</span><span class="sxs-lookup"><span data-stu-id="94d0a-111">In hello [Azure Portal](https://portal.azure.com), browse toohello CDN profile containing hello endpoint you wish toopre-load.</span></span>  <span data-ttu-id="94d0a-112">Otevře se okno profil Hello.</span><span class="sxs-lookup"><span data-stu-id="94d0a-112">hello profile blade opens.</span></span>
2. <span data-ttu-id="94d0a-113">Klikněte na tlačítko hello koncového bodu v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="94d0a-113">Click hello endpoint in hello list.</span></span>  <span data-ttu-id="94d0a-114">Otevře se okno Hello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="94d0a-114">hello endpoint blade opens.</span></span>
3. <span data-ttu-id="94d0a-115">V okně koncový bod CDN hello klikněte na tlačítko zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="94d0a-115">From hello CDN endpoint blade, click hello load button.</span></span>
   
    ![Okno koncový bod CDN](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    <span data-ttu-id="94d0a-117">Otevře se okno zatížení Hello.</span><span class="sxs-lookup"><span data-stu-id="94d0a-117">hello Load blade opens.</span></span>
   
    ![Okno zatížení CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. <span data-ttu-id="94d0a-119">Zadejte úplnou cestu hello každého majetku chcete tooload (například `/pictures/kitten.png`) v hello **cesta** textové pole.</span><span class="sxs-lookup"><span data-stu-id="94d0a-119">Enter hello full path of each asset you wish tooload (e.g., `/pictures/kitten.png`) in hello **Path** textbox.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="94d0a-120">Další **cesta** textová pole se zobrazí, když zadáte text tooallow toobuild seznam více prostředků.</span><span class="sxs-lookup"><span data-stu-id="94d0a-120">More **Path** textboxes will appear after you enter text tooallow you toobuild a list of multiple assets.</span></span>  <span data-ttu-id="94d0a-121">Prostředky můžete odstranit ze seznamu hello kliknutím na tlačítko se třemi tečkami (...) hello.</span><span class="sxs-lookup"><span data-stu-id="94d0a-121">You can delete assets from hello list by clicking hello ellipsis (...) button.</span></span>
   > 
   > <span data-ttu-id="94d0a-122">Cesty musí být relativní adresa URL, která odpovídá hello následující [regulární výraz](https://msdn.microsoft.com/library/az24scfc.aspx):</span><span class="sxs-lookup"><span data-stu-id="94d0a-122">Paths must be a relative URL that fits hello following [regular expression](https://msdn.microsoft.com/library/az24scfc.aspx):</span></span>  
   > ><span data-ttu-id="94d0a-123">Načíst cestu k souboru jedním `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;</span><span class="sxs-lookup"><span data-stu-id="94d0a-123">Load a single file path `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;</span></span>  
   > ><span data-ttu-id="94d0a-124">Načíst jeden soubor s řetězec dotazu`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`</span><span class="sxs-lookup"><span data-stu-id="94d0a-124">Load a single file with query string `@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`</span></span>  
   > 
   > <span data-ttu-id="94d0a-125">Každý prostředek musí mít svůj vlastní cesta.</span><span class="sxs-lookup"><span data-stu-id="94d0a-125">Each asset must have its own path.</span></span>  <span data-ttu-id="94d0a-126">Není k dispozici žádná funkce zástupných znaků pro předběžné načítání prostředků.</span><span class="sxs-lookup"><span data-stu-id="94d0a-126">There is no wildcard functionality for pre-loading assets.</span></span>
   > 
   > 
   
    ![Tlačítko zatížení](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. <span data-ttu-id="94d0a-128">Klikněte na tlačítko hello **zatížení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="94d0a-128">Click hello **Load** button.</span></span>
   
    ![Tlačítko zatížení](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> <span data-ttu-id="94d0a-130">Existuje omezení 10 zatížení požadavků za minutu za profil CDN.</span><span class="sxs-lookup"><span data-stu-id="94d0a-130">There is a limitation of 10 load requests per minute per CDN profile.</span></span> <span data-ttu-id="94d0a-131">50 cest jsou povoleny na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="94d0a-131">50 paths are allowed per request.</span></span> <span data-ttu-id="94d0a-132">Každá cesta může mít délku cesty 1024 znaků.</span><span class="sxs-lookup"><span data-stu-id="94d0a-132">Each path has a path-length limit of 1024 characters.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="94d0a-133">Viz také</span><span class="sxs-lookup"><span data-stu-id="94d0a-133">See also</span></span>
* [<span data-ttu-id="94d0a-134">Vyprázdnění koncového bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="94d0a-134">Purge an Azure CDN endpoint</span></span>](cdn-purge-endpoint.md)
* [<span data-ttu-id="94d0a-135">Azure referenční dokumentace rozhraní API REST CDN - mazání nebo předběžné načtení koncový bod</span><span class="sxs-lookup"><span data-stu-id="94d0a-135">Azure CDN REST API reference - Purge or Pre-Load an Endpoint</span></span>](https://msdn.microsoft.com/library/mt634451.aspx)

