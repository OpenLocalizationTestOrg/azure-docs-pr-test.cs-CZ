---
title: "aaaPurge koncový bod Azure CDN | Microsoft Docs"
description: "Zjistěte, jak toopurge všechny uložené v mezipaměti obsahu z koncového bodu Azure CDN."
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
ms.openlocfilehash: a09f4a49aa1e2d7655ecae44b5126c11c28fd599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="purge-an-azure-cdn-endpoint"></a><span data-ttu-id="d0d38-103">Vyprázdnění koncového bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="d0d38-103">Purge an Azure CDN endpoint</span></span>
## <a name="overview"></a><span data-ttu-id="d0d38-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="d0d38-104">Overview</span></span>
<span data-ttu-id="d0d38-105">Uzlů Azure CDN hraniční mezipaměti prostředky až do vypršení platnosti hello asset time to live (TTL).</span><span class="sxs-lookup"><span data-stu-id="d0d38-105">Azure CDN edge nodes will cache assets until hello asset's time-to-live (TTL) expires.</span></span>  <span data-ttu-id="d0d38-106">Po vypršení platnosti hello asset TTL, když klient požádá o prostředek hello z hello hraniční uzel, bude hello hraniční uzel načítat aktualizované novou kopii požadavek klienta hello asset tooserve hello a aktualizace mezipaměti hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="d0d38-106">After hello asset's TTL expires, when a client requests hello asset from hello edge node, hello edge node will retrieve a new updated copy of hello asset tooserve hello client request and store refresh hello cache.</span></span>

<span data-ttu-id="d0d38-107">Hello osvědčených postupů toomake se, že uživatelé vždycky získat nejnovější verzi vaše prostředky hello je tooversion vaše prostředky pro každou aktualizaci a publikovat je jako nové adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d0d38-107">hello best practice toomake sure your users always obtain hello latest copy of your assets is tooversion your assets for each update and publish them as new URLs.</span></span>  <span data-ttu-id="d0d38-108">CDN načte okamžitě hello nové prostředky pro další požadavky klientů hello.</span><span class="sxs-lookup"><span data-stu-id="d0d38-108">CDN will immediately retrieve hello new assets for hello next client requests.</span></span>  <span data-ttu-id="d0d38-109">Někdy může přát toopurge do mezipaměti obsah ze všech uzlů okraj a vynutit je všechny tooretrieve nové aktualizované prostředky.</span><span class="sxs-lookup"><span data-stu-id="d0d38-109">Sometimes you may wish toopurge cached content from all edge nodes and force them all tooretrieve new updated assets.</span></span>  <span data-ttu-id="d0d38-110">Může to být kvůli tooupdates tooyour webové aplikace nebo tooquickly aktualizace prostředky, které obsahují nesprávné informace.</span><span class="sxs-lookup"><span data-stu-id="d0d38-110">This might be due tooupdates tooyour web application, or tooquickly update assets that contain incorrect information.</span></span>

> [!TIP]
> <span data-ttu-id="d0d38-111">Všimněte si, že vyprazdňování pouze vymaže hello do mezipaměti obsah na servery edge hello CDN.</span><span class="sxs-lookup"><span data-stu-id="d0d38-111">Note that purging only clears hello cached content on hello CDN edge servers.</span></span>  <span data-ttu-id="d0d38-112">Všechny podřízené mezipaměti, jako je například proxy servery a místní prohlížeče mezipaměti, může stále obsahující kopii hello soubor v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="d0d38-112">Any downstream caches, such as proxy servers and local browser caches, may still hold a cached copy of hello file.</span></span>  <span data-ttu-id="d0d38-113">Je důležité tooremember to když nastavíte souboru time to live.</span><span class="sxs-lookup"><span data-stu-id="d0d38-113">It's important tooremember this when you set a file's time-to-live.</span></span>  <span data-ttu-id="d0d38-114">Podřízené klienta toorequest hello nejnovější verzi souboru můžete vynutit tím, že je jedinečný název pokaždé, když ho aktualizujete nebo přímým [řetězce dotazu do mezipaměti](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="d0d38-114">You can force a downstream client toorequest hello latest version of your file by giving it a unique name every time you update it, or by taking advantage of [query string caching](cdn-query-string.md).</span></span>  
> 
> 

<span data-ttu-id="d0d38-115">Tento kurz vás provede vyprazdňování prostředky ze všech uzlů edge koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="d0d38-115">This tutorial walks you through purging assets from all edge nodes of an endpoint.</span></span>

## <a name="walkthrough"></a><span data-ttu-id="d0d38-116">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="d0d38-116">Walkthrough</span></span>
1. <span data-ttu-id="d0d38-117">V hello [portálu Azure](https://portal.azure.com), procházet profil CDN toohello obsahující koncový bod hello chcete toopurge.</span><span class="sxs-lookup"><span data-stu-id="d0d38-117">In hello [Azure Portal](https://portal.azure.com), browse toohello CDN profile containing hello endpoint you wish toopurge.</span></span>
2. <span data-ttu-id="d0d38-118">Z hello okno profil CDN klikněte na tlačítko vyprázdnění hello.</span><span class="sxs-lookup"><span data-stu-id="d0d38-118">From hello CDN profile blade, click hello purge button.</span></span>
   
    ![Okno profil CDN](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    <span data-ttu-id="d0d38-120">Otevře se okno vyprázdnění Hello.</span><span class="sxs-lookup"><span data-stu-id="d0d38-120">hello Purge blade opens.</span></span>
   
    ![Okno vyprázdnění CDN](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. <span data-ttu-id="d0d38-122">Na hello Vymazat okno, vyberte adresu služby hello chcete toopurge z rozevíracího seznamu hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d0d38-122">On hello Purge blade, select hello service address you wish toopurge from hello URL dropdown.</span></span>
   
    ![Vyprázdnění formuláře](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > <span data-ttu-id="d0d38-124">Můžete také získat toohello vyprázdnění okno kliknutím hello **mazání** tlačítka v okně koncový bod CDN hello.</span><span class="sxs-lookup"><span data-stu-id="d0d38-124">You can also get toohello Purge blade by clicking hello **Purge** button on hello CDN endpoint blade.</span></span>  <span data-ttu-id="d0d38-125">V takovém případě hello **URL** pole bude předem vyplněny adresu služby hello této konkrétní koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="d0d38-125">In that case, hello **URL** field will be pre-populated with hello service address of that specific endpoint.</span></span>
   > 
   > 
4. <span data-ttu-id="d0d38-126">Vyberte, jaké prostředky chcete toopurge z hello uzly okraj.</span><span class="sxs-lookup"><span data-stu-id="d0d38-126">Select what assets you wish toopurge from hello edge nodes.</span></span>  <span data-ttu-id="d0d38-127">Pokud chcete tooclear všechny prostředky, klikněte na tlačítko hello **vyprázdnit všechno** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="d0d38-127">If you wish tooclear all assets, click hello **Purge all** checkbox.</span></span>  <span data-ttu-id="d0d38-128">Jinak, zadejte cestu hello každého majetku chcete toopurge v hello **cesta** textové pole.</span><span class="sxs-lookup"><span data-stu-id="d0d38-128">Otherwise, type hello path of each asset you wish toopurge in hello **Path** textbox.</span></span> <span data-ttu-id="d0d38-129">Následující formáty, které jsou podporovány v cestě hello.</span><span class="sxs-lookup"><span data-stu-id="d0d38-129">Below formats are supported in hello path.</span></span>
    1. <span data-ttu-id="d0d38-130">**Vyprázdnění jedné adresy URL**: vyprázdnění jednotlivý prostředek zadáním hello úplnou adresu URL, s nebo bez hello příponu souboru, například`/pictures/strasbourg.png`;`/pictures/strasbourg`</span><span class="sxs-lookup"><span data-stu-id="d0d38-130">**Single URL purge**: Purge individual asset by specifying hello full URL, with or without hello file extension, e.g.,`/pictures/strasbourg.png`; `/pictures/strasbourg`</span></span>
    2. <span data-ttu-id="d0d38-131">**Zástupné vyprázdnění**: hvězdičky (\*) může být použita jako zástupný znak.</span><span class="sxs-lookup"><span data-stu-id="d0d38-131">**Wildcard purge**: Asterisk (\*) may be used as a wildcard.</span></span> <span data-ttu-id="d0d38-132">Vyprázdnit všechny složky, podsložky a soubory v koncovém bodě s `/*` hello cestu nebo vyprázdnit všechny podsložky a soubory v konkrétní složce, a to zadáním hello složky následuje `/*`, například`/pictures/*`.</span><span class="sxs-lookup"><span data-stu-id="d0d38-132">Purge all folders, sub-folders and files under an endpoint with `/*` in hello path or purge all sub-folders and files under a specific folder by specifying hello folder followed by `/*`, e.g.,`/pictures/*`.</span></span>  <span data-ttu-id="d0d38-133">Všimněte si, že zástupné vyprázdnění nepodporuje Azure CDN společnosti Akamai aktuálně.</span><span class="sxs-lookup"><span data-stu-id="d0d38-133">Note that wildcard purge is not supported by Azure CDN from Akamai currently.</span></span> 
    3. <span data-ttu-id="d0d38-134">**Kořenové domény vyprázdnění**: kořenové hello vyprázdnění koncového bodu hello s "/" v cestě hello.</span><span class="sxs-lookup"><span data-stu-id="d0d38-134">**Root domain purge**: Purge hello root of hello endpoint with "/" in hello path.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="d0d38-135">Cesty pro vyprázdnění musí být zadána a musí být relativní adresa URL, který umístit hello následující [regulární výraz](https://msdn.microsoft.com/library/az24scfc.aspx).</span><span class="sxs-lookup"><span data-stu-id="d0d38-135">Paths must be specified for purge and must be a relative URL that fit hello following [regular expression](https://msdn.microsoft.com/library/az24scfc.aspx).</span></span> <span data-ttu-id="d0d38-136">**Vyprázdnit všechny** a **zástupné vyprázdnění** nepodporuje **Azure CDN společnosti Akamai** aktuálně.</span><span class="sxs-lookup"><span data-stu-id="d0d38-136">**Purge all** and **Wildcard purge** not supported by **Azure CDN from Akamai** currently.</span></span>
   > > <span data-ttu-id="d0d38-137">Vyprázdnění jedné adresy URL`@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span><span class="sxs-lookup"><span data-stu-id="d0d38-137">Single URL purge `@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span></span>  
   > > <span data-ttu-id="d0d38-138">Řetězec dotazu`@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span><span class="sxs-lookup"><span data-stu-id="d0d38-138">Query string `@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span></span>  
   > > <span data-ttu-id="d0d38-139">Zástupné vyprázdnění `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span><span class="sxs-lookup"><span data-stu-id="d0d38-139">Wildcard purge `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span></span> 
   > 
   > <span data-ttu-id="d0d38-140">Další **cesta** textová pole se zobrazí, když zadáte text tooallow toobuild seznam více prostředků.</span><span class="sxs-lookup"><span data-stu-id="d0d38-140">More **Path** textboxes will appear after you enter text tooallow you toobuild a list of multiple assets.</span></span>  <span data-ttu-id="d0d38-141">Prostředky můžete odstranit ze seznamu hello kliknutím na tlačítko se třemi tečkami (...) hello.</span><span class="sxs-lookup"><span data-stu-id="d0d38-141">You can delete assets from hello list by clicking hello ellipsis (...) button.</span></span>
   > 
5. <span data-ttu-id="d0d38-142">Klikněte na tlačítko hello **mazání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d0d38-142">Click hello **Purge** button.</span></span>
   
    ![Vyprázdnění tlačítko](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> <span data-ttu-id="d0d38-144">Vyprázdnění požadavky trvat přibližně 2 – 3 minuty tooprocess s **Azure CDN společnosti Verizon** (Standard a Premium) a přibližně 7 minut s **Azure CDN společnosti Akamai**.</span><span class="sxs-lookup"><span data-stu-id="d0d38-144">Purge requests take approximately 2-3 minutes tooprocess with **Azure CDN from Verizon** (Standard and Premium), and approximately 7 minutes with **Azure CDN from Akamai**.</span></span>  <span data-ttu-id="d0d38-145">Limit souběžných 50 mazání požadavky v daném okamžiku je Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="d0d38-145">Azure CDN has a limit of 50 concurrent purge requests at any given time.</span></span> 
> 
> 

## <a name="see-also"></a><span data-ttu-id="d0d38-146">Viz také</span><span class="sxs-lookup"><span data-stu-id="d0d38-146">See also</span></span>
* [<span data-ttu-id="d0d38-147">Předběžné načtení prostředků v koncovém bodu Azure CDN</span><span class="sxs-lookup"><span data-stu-id="d0d38-147">Pre-load assets on an Azure CDN endpoint</span></span>](cdn-preload-endpoint.md)
* [<span data-ttu-id="d0d38-148">Azure referenční dokumentace rozhraní API REST CDN - mazání nebo předběžné načtení koncový bod</span><span class="sxs-lookup"><span data-stu-id="d0d38-148">Azure CDN REST API reference - Purge or Pre-Load an Endpoint</span></span>](https://msdn.microsoft.com/library/mt634451.aspx)

