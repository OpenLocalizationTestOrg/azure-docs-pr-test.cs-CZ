---
title: "Vyprázdnění koncového bodu Azure CDN | Microsoft Docs"
description: "Zjistěte, jak k vyprázdnění všech obsah uložený v mezipaměti z koncového bodu Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: b035c232bb58d653960190d4974cc3789d55a51d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="purge-an-azure-cdn-endpoint"></a><span data-ttu-id="f9f98-103">Vyprázdnění koncového bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="f9f98-103">Purge an Azure CDN endpoint</span></span>
## <a name="overview"></a><span data-ttu-id="f9f98-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f9f98-104">Overview</span></span>
<span data-ttu-id="f9f98-105">Uzlů Azure CDN hraniční mezipaměti prostředky až do vypršení platnosti assetu time to live (TTL).</span><span class="sxs-lookup"><span data-stu-id="f9f98-105">Azure CDN edge nodes will cache assets until the asset's time-to-live (TTL) expires.</span></span>  <span data-ttu-id="f9f98-106">Po vypršení platnosti TTL assetu, když klient požádá o prostředku z uzlu edge, hraničního uzlu načte aktualizované novou kopii asset obsluhovat požadavky klientů a úložiště aktualizace mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f9f98-106">After the asset's TTL expires, when a client requests the asset from the edge node, the edge node will retrieve a new updated copy of the asset to serve the client request and store refresh the cache.</span></span>

<span data-ttu-id="f9f98-107">Doporučujeme, abyste měli jistotu, že uživatelé vždycky získat nejnovější verzi vaše prostředky je vaše prostředky pro jednotlivé aktualizace na verzi a publikovat je jako nové adresy URL.</span><span class="sxs-lookup"><span data-stu-id="f9f98-107">The best practice to make sure your users always obtain the latest copy of your assets is to version your assets for each update and publish them as new URLs.</span></span>  <span data-ttu-id="f9f98-108">CDN načte okamžitě nové prostředky pro další požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="f9f98-108">CDN will immediately retrieve the new assets for the next client requests.</span></span>  <span data-ttu-id="f9f98-109">V některých případech můžete chtít vymazat obsah uložený v mezipaměti ze všech uzlů okraj a nutí je všechny načíst nové aktualizované prostředky.</span><span class="sxs-lookup"><span data-stu-id="f9f98-109">Sometimes you may wish to purge cached content from all edge nodes and force them all to retrieve new updated assets.</span></span>  <span data-ttu-id="f9f98-110">Důvodem může být aktualizace webové aplikace, nebo rychle aktualizace prostředky, které obsahují nesprávné informace.</span><span class="sxs-lookup"><span data-stu-id="f9f98-110">This might be due to updates to your web application, or to quickly update assets that contain incorrect information.</span></span>

> [!TIP]
> <span data-ttu-id="f9f98-111">Všimněte si, že vyprazdňování pouze vymaže obsah uložený v mezipaměti na serveru edge CDN.</span><span class="sxs-lookup"><span data-stu-id="f9f98-111">Note that purging only clears the cached content on the CDN edge servers.</span></span>  <span data-ttu-id="f9f98-112">Všechny podřízené mezipaměti, jako je například proxy servery a místní prohlížeče mezipaměti, může stále obsahující kopii souboru v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="f9f98-112">Any downstream caches, such as proxy servers and local browser caches, may still hold a cached copy of the file.</span></span>  <span data-ttu-id="f9f98-113">Je důležité pamatovat na to při nastavení souboru time to live.</span><span class="sxs-lookup"><span data-stu-id="f9f98-113">It's important to remember this when you set a file's time-to-live.</span></span>  <span data-ttu-id="f9f98-114">Můžete vynutit podřízené klienta k žádosti o nejnovější verzi souboru tím, že je jedinečný název pokaždé, když ho aktualizujete, nebo přímým [řetězce dotazu do mezipaměti](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="f9f98-114">You can force a downstream client to request the latest version of your file by giving it a unique name every time you update it, or by taking advantage of [query string caching](cdn-query-string.md).</span></span>  
> 
> 

<span data-ttu-id="f9f98-115">Tento kurz vás provede vyprazdňování prostředky ze všech uzlů edge koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="f9f98-115">This tutorial walks you through purging assets from all edge nodes of an endpoint.</span></span>

## <a name="walkthrough"></a><span data-ttu-id="f9f98-116">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="f9f98-116">Walkthrough</span></span>
1. <span data-ttu-id="f9f98-117">V [portálu Azure](https://portal.azure.com), vyhledejte profil CDN obsahující koncový bod, které chcete vymazat.</span><span class="sxs-lookup"><span data-stu-id="f9f98-117">In the [Azure Portal](https://portal.azure.com), browse to the CDN profile containing the endpoint you wish to purge.</span></span>
2. <span data-ttu-id="f9f98-118">V okně profil CDN klikněte na tlačítko vyprázdnění.</span><span class="sxs-lookup"><span data-stu-id="f9f98-118">From the CDN profile blade, click the purge button.</span></span>
   
    ![Okno profil CDN](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    <span data-ttu-id="f9f98-120">Otevře se okno vyprázdnění.</span><span class="sxs-lookup"><span data-stu-id="f9f98-120">The Purge blade opens.</span></span>
   
    ![Okno vyprázdnění CDN](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. <span data-ttu-id="f9f98-122">V okně vyprázdnění vyberte adresu služby, který si přejete vymazat z rozevíracího seznamu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="f9f98-122">On the Purge blade, select the service address you wish to purge from the URL dropdown.</span></span>
   
    ![Vyprázdnění formuláře](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > <span data-ttu-id="f9f98-124">Můžete se do okna vyprázdnění kliknutím také získat **mazání** tlačítka v okně koncového bodu CDN.</span><span class="sxs-lookup"><span data-stu-id="f9f98-124">You can also get to the Purge blade by clicking the **Purge** button on the CDN endpoint blade.</span></span>  <span data-ttu-id="f9f98-125">V takovém případě **URL** pole bude předem vyplněny adresu služby tento konkrétní koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="f9f98-125">In that case, the **URL** field will be pre-populated with the service address of that specific endpoint.</span></span>
   > 
   > 
4. <span data-ttu-id="f9f98-126">Vyberte, jaké prostředky, které chcete vymazat z uzlů okraj.</span><span class="sxs-lookup"><span data-stu-id="f9f98-126">Select what assets you wish to purge from the edge nodes.</span></span>  <span data-ttu-id="f9f98-127">Pokud chcete vymazat všechny prostředky, klikněte **vyprázdnit všechno** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="f9f98-127">If you wish to clear all assets, click the **Purge all** checkbox.</span></span>  <span data-ttu-id="f9f98-128">Jinak, zadejte cestu každý prostředek, který si přejete vymazat v **cesta** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f9f98-128">Otherwise, type the path of each asset you wish to purge in the **Path** textbox.</span></span> <span data-ttu-id="f9f98-129">Následující formáty, které jsou podporovány v cestě.</span><span class="sxs-lookup"><span data-stu-id="f9f98-129">Below formats are supported in the path.</span></span>
    1. <span data-ttu-id="f9f98-130">**Vyprázdnění jedné adresy URL**: vyprázdnění jednotlivý prostředek zadáním úplnou adresu URL s nebo bez přípony souboru, například`/pictures/strasbourg.png`;`/pictures/strasbourg`</span><span class="sxs-lookup"><span data-stu-id="f9f98-130">**Single URL purge**: Purge individual asset by specifying the full URL, with or without the file extension, e.g.,`/pictures/strasbourg.png`; `/pictures/strasbourg`</span></span>
    2. <span data-ttu-id="f9f98-131">**Zástupné vyprázdnění**: hvězdičky (\*) může být použita jako zástupný znak.</span><span class="sxs-lookup"><span data-stu-id="f9f98-131">**Wildcard purge**: Asterisk (\*) may be used as a wildcard.</span></span> <span data-ttu-id="f9f98-132">Vyprázdnit všechny složky, podsložky a soubory v koncovém bodě s `/*` v cestě nebo vyprázdnění všechny podsložky a soubory v konkrétní složce zadáním složce následuje `/*`, například`/pictures/*`.</span><span class="sxs-lookup"><span data-stu-id="f9f98-132">Purge all folders, sub-folders and files under an endpoint with `/*` in the path or purge all sub-folders and files under a specific folder by specifying the folder followed by `/*`, e.g.,`/pictures/*`.</span></span>  <span data-ttu-id="f9f98-133">Všimněte si, že zástupné vyprázdnění nepodporuje Azure CDN společnosti Akamai aktuálně.</span><span class="sxs-lookup"><span data-stu-id="f9f98-133">Note that wildcard purge is not supported by Azure CDN from Akamai currently.</span></span> 
    3. <span data-ttu-id="f9f98-134">**Kořenové domény vyprázdnění**: vyprázdnění kořenovém koncový bod s "/" v cestě.</span><span class="sxs-lookup"><span data-stu-id="f9f98-134">**Root domain purge**: Purge the root of the endpoint with "/" in the path.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="f9f98-135">Cesty pro vyprázdnění musí být zadána a musí být relativní adresa URL, která podle následující [regulární výraz](https://msdn.microsoft.com/library/az24scfc.aspx).</span><span class="sxs-lookup"><span data-stu-id="f9f98-135">Paths must be specified for purge and must be a relative URL that fit the following [regular expression](https://msdn.microsoft.com/library/az24scfc.aspx).</span></span> <span data-ttu-id="f9f98-136">**Vyprázdnit všechny** a **zástupné vyprázdnění** nepodporuje **Azure CDN společnosti Akamai** aktuálně.</span><span class="sxs-lookup"><span data-stu-id="f9f98-136">**Purge all** and **Wildcard purge** not supported by **Azure CDN from Akamai** currently.</span></span>
   > > <span data-ttu-id="f9f98-137">Vyprázdnění jedné adresy URL`@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span><span class="sxs-lookup"><span data-stu-id="f9f98-137">Single URL purge `@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span></span>  
   > > <span data-ttu-id="f9f98-138">Řetězec dotazu`@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span><span class="sxs-lookup"><span data-stu-id="f9f98-138">Query string `@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span></span>  
   > > <span data-ttu-id="f9f98-139">Zástupné vyprázdnění `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span><span class="sxs-lookup"><span data-stu-id="f9f98-139">Wildcard purge `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span></span> 
   > 
   > <span data-ttu-id="f9f98-140">Další **cesta** textová pole se zobrazí, když zadáte text, který vám umožní sestavit seznam více prostředků.</span><span class="sxs-lookup"><span data-stu-id="f9f98-140">More **Path** textboxes will appear after you enter text to allow you to build a list of multiple assets.</span></span>  <span data-ttu-id="f9f98-141">Prostředky můžete odstranit ze seznamu kliknutím na tlačítko se třemi tečkami (...).</span><span class="sxs-lookup"><span data-stu-id="f9f98-141">You can delete assets from the list by clicking the ellipsis (...) button.</span></span>
   > 
5. <span data-ttu-id="f9f98-142">Klikněte **mazání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f9f98-142">Click the **Purge** button.</span></span>
   
    ![Vyprázdnění tlačítko](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> <span data-ttu-id="f9f98-144">Žádosti o vymazání trvat přibližně 2 – 3 minut zpracovat **Azure CDN společnosti Verizon** (Standard a Premium) a přibližně 7 minut s **Azure CDN společnosti Akamai**.</span><span class="sxs-lookup"><span data-stu-id="f9f98-144">Purge requests take approximately 2-3 minutes to process with **Azure CDN from Verizon** (Standard and Premium), and approximately 7 minutes with **Azure CDN from Akamai**.</span></span>  <span data-ttu-id="f9f98-145">Limit souběžných 50 mazání požadavky v daném okamžiku je Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="f9f98-145">Azure CDN has a limit of 50 concurrent purge requests at any given time.</span></span> 
> 
> 

## <a name="see-also"></a><span data-ttu-id="f9f98-146">Viz také</span><span class="sxs-lookup"><span data-stu-id="f9f98-146">See also</span></span>
* [<span data-ttu-id="f9f98-147">Předběžné načtení prostředků v koncovém bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="f9f98-147">Pre-load assets on an Azure CDN endpoint</span></span>](cdn-preload-endpoint.md)
* [<span data-ttu-id="f9f98-148">Azure referenční dokumentace rozhraní API REST CDN - mazání nebo předběžné načtení koncový bod</span><span class="sxs-lookup"><span data-stu-id="f9f98-148">Azure CDN REST API reference - Purge or Pre-Load an Endpoint</span></span>](https://msdn.microsoft.com/library/mt634451.aspx)

