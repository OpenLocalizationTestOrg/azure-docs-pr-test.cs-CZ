---
title: "Machine Learning doporučení: Integrace jazyka JavaScript | Microsoft Docs"
description: "Dokumentace k Azure Machine Learning doporučení - JavaScript integrace-"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: bbbb5bb6-489d-4a62-a2ae-f36237e9e2e1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 8f27962d097bffc2a03de80244ae41d6573a4bf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a><span data-ttu-id="9a857-103">Doporučení Azure Machine Learning – integrace v jazyce JavaScript</span><span class="sxs-lookup"><span data-stu-id="9a857-103">Azure Machine Learning Recommendations - JavaScript Integration</span></span>
> [!NOTE]
> <span data-ttu-id="9a857-104">Měli byste začít používat službu doporučení rozhraní API kognitivní místo tuto verzi.</span><span class="sxs-lookup"><span data-stu-id="9a857-104">You should start using the Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="9a857-105">Službu kognitivní doporučení budou nahrazení této služby, a všechny nové funkce bude vyvinutý existuje.</span><span class="sxs-lookup"><span data-stu-id="9a857-105">The Recommendations Cognitive Service will be replacing this service, and all the new features will be developed there.</span></span> <span data-ttu-id="9a857-106">Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.</span><span class="sxs-lookup"><span data-stu-id="9a857-106">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="9a857-107">Další informace o [migraci na novou službu kognitivní](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="9a857-107">Learn more about [Migrating to the new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="9a857-108">Tento dokument zobrazit v ní jak integrovat svůj web pomocí jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9a857-108">This document depict how to integrate your site using JavaScript.</span></span> <span data-ttu-id="9a857-109">Jazyk JavaScript umožňuje odesílat události získávání dat a využívat doporučení po sestavení modelu doporučení.</span><span class="sxs-lookup"><span data-stu-id="9a857-109">The JavaScript enables you to send Data Acquisition events and to consume recommendations once you build a recommendation model.</span></span> <span data-ttu-id="9a857-110">Všechny operace, které se provádí prostřednictvím JS lze také provést z na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="9a857-110">All operations done via JS can be also done from server side.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a><span data-ttu-id="9a857-111">1. Obecné – přehled</span><span class="sxs-lookup"><span data-stu-id="9a857-111">1. General Overview</span></span>
<span data-ttu-id="9a857-112">Integrace serveru s Azure ML doporučení se skládají na fáze 2:</span><span class="sxs-lookup"><span data-stu-id="9a857-112">Integrating your site with Azure ML Recommendations consist on 2 Phases:</span></span>

1. <span data-ttu-id="9a857-113">Odesílání událostí do Azure ML doporučení.</span><span class="sxs-lookup"><span data-stu-id="9a857-113">Send Events into Azure ML Recommendations.</span></span> <span data-ttu-id="9a857-114">To vám umožní vytvořit model doporučení.</span><span class="sxs-lookup"><span data-stu-id="9a857-114">This will enable to build a recommendation model.</span></span>
2. <span data-ttu-id="9a857-115">Využívat doporučení.</span><span class="sxs-lookup"><span data-stu-id="9a857-115">Consume the recommendations.</span></span> <span data-ttu-id="9a857-116">Jakmile je vytvořen modelu můžete využívat doporučení.</span><span class="sxs-lookup"><span data-stu-id="9a857-116">After the model is built you can consume the recommendations.</span></span> <span data-ttu-id="9a857-117">(Tento dokument nevysvětluje, jak vytvořit model, přečíst úvodní příručka získat další informace o tom,).</span><span class="sxs-lookup"><span data-stu-id="9a857-117">(This document does not explain how to build a model, read the quick start guide to get more information on how).</span></span>

<span data-ttu-id="9a857-118"><ins>Fáze I</ins></span><span class="sxs-lookup"><span data-stu-id="9a857-118"><ins>Phase I</ins></span></span>

<span data-ttu-id="9a857-119">V první fázi můžete vložit do stránek html malé knihovna JavaScript, která umožňuje stránce odesílat události, které se používají na stránce html do Azure ML doporučení servery (prostřednictvím trhu dat):</span><span class="sxs-lookup"><span data-stu-id="9a857-119">In the first phase you insert into your html pages a small JavaScript library that enables the page to send events as they occur on the html page into Azure ML Recommendations servers (via Data Market):</span></span>

![Výkres1][1]

<span data-ttu-id="9a857-121"><ins>Fáze II</ins></span><span class="sxs-lookup"><span data-stu-id="9a857-121"><ins>Phase II</ins></span></span>

<span data-ttu-id="9a857-122">V druhé fázi, když chcete zobrazit doporučení na stránce vyberete jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="9a857-122">In the second phase when you want to show the recommendations on the page you select one of the following options:</span></span>

<span data-ttu-id="9a857-123">1 serveru (ve fázi vykreslování stránky) volá Azure ML doporučení serveru (prostřednictvím Data trhu) získat doporučení.</span><span class="sxs-lookup"><span data-stu-id="9a857-123">1.Your server (on the phase of page rendering) calls Azure ML Recommendations Server (via Data Market) to get recommendations.</span></span> <span data-ttu-id="9a857-124">Výsledky budou zahrnovat seznam id položky.</span><span class="sxs-lookup"><span data-stu-id="9a857-124">The results include a list of items id.</span></span> <span data-ttu-id="9a857-125">Server je potřeba rozšířit výsledky s položkami metadata (např. obrázky, popis) a odešle do prohlížeče, vytvořené stránky.</span><span class="sxs-lookup"><span data-stu-id="9a857-125">Your server needs to enrich the results with the items Meta data (e.g. images, description) and send the created page to the browser.</span></span>

![Drawing2][2]

<span data-ttu-id="9a857-127">2. Další možností je použití malý soubor JavaScript z první fáze jednoduchý seznam Doporučené položky.</span><span class="sxs-lookup"><span data-stu-id="9a857-127">2.The other option is to use the small JavaScript file from phase one to get a simple list of recommended items.</span></span> <span data-ttu-id="9a857-128">Data přijatá tady je zeštíhlen než ten, který v první možnosti.</span><span class="sxs-lookup"><span data-stu-id="9a857-128">The data received here is leaner than the one in the first option.</span></span>

![Drawing3][3]

## <a name="2-prerequisites"></a><span data-ttu-id="9a857-130">2. Požadavky</span><span class="sxs-lookup"><span data-stu-id="9a857-130">2. Prerequisites</span></span>
1. <span data-ttu-id="9a857-131">Vytvořte nový model pomocí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9a857-131">Create a new model using the APIs.</span></span> <span data-ttu-id="9a857-132">O tom, jak to provést naleznete v Průvodci rychlý start.</span><span class="sxs-lookup"><span data-stu-id="9a857-132">See the Quick start guide on how to do it.</span></span>
2. <span data-ttu-id="9a857-133">Zakódovat váš &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; pomocí base64.</span><span class="sxs-lookup"><span data-stu-id="9a857-133">Encode your &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; with base64.</span></span> <span data-ttu-id="9a857-134">(Bude se používat pro základní ověřování pro povolení JS kódu pro volání rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="9a857-134">(This will be used for the basic authentication to enable the JS code to call the APIs).</span></span>

## <a name="3-send-data-acquisition-events-using-javascript"></a><span data-ttu-id="9a857-135">3. Odesílání událostí získávání dat pomocí jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="9a857-135">3. Send Data Acquisition events using JavaScript</span></span>
<span data-ttu-id="9a857-136">Následující kroky usnadňují odesílání událostí:</span><span class="sxs-lookup"><span data-stu-id="9a857-136">The following steps facilitate sending events:</span></span>

1. <span data-ttu-id="9a857-137">Zahrnují knihovny JQuery ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="9a857-137">Include JQuery library in your code.</span></span> <span data-ttu-id="9a857-138">Můžete ji stáhnout z nuget v následující adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9a857-138">You can download it from nuget in the following URL.</span></span>
   
     <span data-ttu-id="9a857-139">http://www.nuget.org/Packages/jQuery/1.8.2</span><span class="sxs-lookup"><span data-stu-id="9a857-139">http://www.nuget.org/packages/jQuery/1.8.2</span></span>
2. <span data-ttu-id="9a857-140">Zahrnout knihovně doporučení Java skriptu z následující adresy URL: http://aka.ms/RecoJSLib1</span><span class="sxs-lookup"><span data-stu-id="9a857-140">Include the Recommendations Java Script library from the following URL: http://aka.ms/RecoJSLib1</span></span>
3. <span data-ttu-id="9a857-141">Inicializace knihovny Azure ML doporučení s příslušnými parametry.</span><span class="sxs-lookup"><span data-stu-id="9a857-141">Initialize Azure ML Recommendations library with the appropriate parameters.</span></span>
   
     <span data-ttu-id="9a857-142"><script>AzureMLRecommendationsStart ("<base64encoding of username:key>", "< model_id >"); </script> 
4. Odeslat příslušné události.</span><span class="sxs-lookup"><span data-stu-id="9a857-142"><script> AzureMLRecommendationsStart("<base64encoding of username:key>", "<model_id>"); </script>
4. Send the appropriate event.</span></span> <span data-ttu-id="9a857-143">Najdete v podrobné části níže na všechny typu události (příklad click – událost) <script> Pokud (typeof AzureMLRecommendationsEvent == "undefined") {</span><span class="sxs-lookup"><span data-stu-id="9a857-143">See detailed section below on all type of events (example of click event)  <script> if (typeof AzureMLRecommendationsEvent=="undefined") {</span></span>         
                     <span data-ttu-id="9a857-144">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script></span><span class="sxs-lookup"><span data-stu-id="9a857-144">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" }); </script></span></span>

### <a name="31----limitations-and-browser-support"></a><span data-ttu-id="9a857-145">3.1.</span><span class="sxs-lookup"><span data-stu-id="9a857-145">3.1.</span></span>    <span data-ttu-id="9a857-146">Omezení a podpora prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="9a857-146">Limitations and Browser Support</span></span>
<span data-ttu-id="9a857-147">Toto je odkaz na implementaci a je zadána, jako je.</span><span class="sxs-lookup"><span data-stu-id="9a857-147">This is a reference implementation and it is given as is.</span></span> <span data-ttu-id="9a857-148">Má podporovat všechny hlavní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9a857-148">It should support all major browsers.</span></span>

### <a name="32----type-of-events"></a><span data-ttu-id="9a857-149">3.2.</span><span class="sxs-lookup"><span data-stu-id="9a857-149">3.2.</span></span>    <span data-ttu-id="9a857-150">Typ události</span><span class="sxs-lookup"><span data-stu-id="9a857-150">Type of Events</span></span>
<span data-ttu-id="9a857-151">Existují 5 typy událostí, který podporuje knihovny: klikněte na tlačítko, klikněte na doporučení, přidat do košíku obchod, odeberte z košíku obchod a nákup.</span><span class="sxs-lookup"><span data-stu-id="9a857-151">There are 5 types of event that the library supports: Click, Recommendation Click, Add to Shop Cart, Remove from Shop Cart and Purchase.</span></span> <span data-ttu-id="9a857-152">Není k dispozici další událost, která se používá k nastavení uživatelský kontext názvem přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9a857-152">There is an additional event that is used to set the user context called Login.</span></span>

#### <a name="321-click-event"></a><span data-ttu-id="9a857-153">3.2.1.</span><span class="sxs-lookup"><span data-stu-id="9a857-153">3.2.1.</span></span> <span data-ttu-id="9a857-154">Click – událost</span><span class="sxs-lookup"><span data-stu-id="9a857-154">Click Event</span></span>
<span data-ttu-id="9a857-155">Tato událost je třeba použít kdykoli uživatel klikli na položku.</span><span class="sxs-lookup"><span data-stu-id="9a857-155">This event should be used any time a user clicked on an item.</span></span> <span data-ttu-id="9a857-156">Obvykle, když uživatel klikne na položku Nová stránka se otevře s podrobnostmi položky; na této stránce musí být tato událost aktivuje.</span><span class="sxs-lookup"><span data-stu-id="9a857-156">Usually when user clicks on an item a new page is opened with the item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="9a857-157">Parametry:</span><span class="sxs-lookup"><span data-stu-id="9a857-157">Parameters:</span></span>

* <span data-ttu-id="9a857-158">události (řetězec, povinná) "Kliknutím"</span><span class="sxs-lookup"><span data-stu-id="9a857-158">event (string, mandatory) - “click”</span></span>
* <span data-ttu-id="9a857-159">Položka jedinečný identifikátor položky (řetězec, povinné)</span><span class="sxs-lookup"><span data-stu-id="9a857-159">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="9a857-160">Název položky (řetězec, volitelné) název položky</span><span class="sxs-lookup"><span data-stu-id="9a857-160">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="9a857-161">Popis zboží (řetězec, volitelný) – popis položky</span><span class="sxs-lookup"><span data-stu-id="9a857-161">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="9a857-162">itemCategory (řetězec, volitelný) kategorie položky</span><span class="sxs-lookup"><span data-stu-id="9a857-162">itemCategory (string, optional) - the category of the item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

<span data-ttu-id="9a857-163">Nebo s volitelnými daty:</span><span class="sxs-lookup"><span data-stu-id="9a857-163">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


#### <a name="322-recommendation-click-event"></a><span data-ttu-id="9a857-164">3.2.2.</span><span class="sxs-lookup"><span data-stu-id="9a857-164">3.2.2.</span></span> <span data-ttu-id="9a857-165">Doporučení Click – událost</span><span class="sxs-lookup"><span data-stu-id="9a857-165">Recommendation Click Event</span></span>
<span data-ttu-id="9a857-166">Tato událost je třeba použít kdykoli uživatel klikli na položku, který jste získali od Azure ML doporučení jako doporučenou položka.</span><span class="sxs-lookup"><span data-stu-id="9a857-166">This event should be used any time a user clicked on an item that was received from Azure ML Recommendations as a recommended item.</span></span> <span data-ttu-id="9a857-167">Obvykle, když uživatel klikne na položku Nová stránka se otevře s podrobnostmi položky; na této stránce musí být tato událost aktivuje.</span><span class="sxs-lookup"><span data-stu-id="9a857-167">Usually when user clicks on an item a new page is opened with the item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="9a857-168">Parametry:</span><span class="sxs-lookup"><span data-stu-id="9a857-168">Parameters:</span></span>

* <span data-ttu-id="9a857-169">události (řetězec, povinná) "recommendationclick"</span><span class="sxs-lookup"><span data-stu-id="9a857-169">event (string, mandatory) - “recommendationclick”</span></span>
* <span data-ttu-id="9a857-170">Položka jedinečný identifikátor položky (řetězec, povinné)</span><span class="sxs-lookup"><span data-stu-id="9a857-170">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="9a857-171">Název položky (řetězec, volitelné) název položky</span><span class="sxs-lookup"><span data-stu-id="9a857-171">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="9a857-172">Popis zboží (řetězec, volitelný) – popis položky</span><span class="sxs-lookup"><span data-stu-id="9a857-172">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="9a857-173">itemCategory (řetězec, volitelný) kategorie položky</span><span class="sxs-lookup"><span data-stu-id="9a857-173">itemCategory (string, optional) - the category of the item</span></span>
* <span data-ttu-id="9a857-174">doplňuje (pole řetězců, volitelný) - semen, které generuje dotaz doporučení.</span><span class="sxs-lookup"><span data-stu-id="9a857-174">seeds (string array, optional) - the seeds that generated the recommendation query.</span></span>
* <span data-ttu-id="9a857-175">recoList (pole řetězců, volitelný) - výsledek požadavku doporučení, generovaný označeného položku.</span><span class="sxs-lookup"><span data-stu-id="9a857-175">recoList (string array, optional) - the result of the recommendation request that generated the item that was clicked.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

<span data-ttu-id="9a857-176">Nebo s volitelnými daty:</span><span class="sxs-lookup"><span data-stu-id="9a857-176">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]                 });
        </script>


#### <a name="323-add-shopping-cart-event"></a><span data-ttu-id="9a857-177">3.2.3.</span><span class="sxs-lookup"><span data-stu-id="9a857-177">3.2.3.</span></span> <span data-ttu-id="9a857-178">Přidat událost nákupní košík</span><span class="sxs-lookup"><span data-stu-id="9a857-178">Add Shopping Cart Event</span></span>
<span data-ttu-id="9a857-179">Tato událost má použít při přidání položky do nákupního košíku uživatele.</span><span class="sxs-lookup"><span data-stu-id="9a857-179">This event should be used when the user add an item to the shopping cart.</span></span>
<span data-ttu-id="9a857-180">Parametry:</span><span class="sxs-lookup"><span data-stu-id="9a857-180">Parameters:</span></span>

* <span data-ttu-id="9a857-181">události (řetězec, povinná) "addshopcart"</span><span class="sxs-lookup"><span data-stu-id="9a857-181">event (string, mandatory) - “addshopcart”</span></span>
* <span data-ttu-id="9a857-182">Položka jedinečný identifikátor položky (řetězec, povinné)</span><span class="sxs-lookup"><span data-stu-id="9a857-182">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="9a857-183">Název položky (řetězec, volitelné) název položky</span><span class="sxs-lookup"><span data-stu-id="9a857-183">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="9a857-184">Popis zboží (řetězec, volitelný) – popis položky</span><span class="sxs-lookup"><span data-stu-id="9a857-184">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="9a857-185">itemCategory (řetězec, volitelný) kategorie položky</span><span class="sxs-lookup"><span data-stu-id="9a857-185">itemCategory (string, optional) - the category of the item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a><span data-ttu-id="9a857-186">3.2.4.</span><span class="sxs-lookup"><span data-stu-id="9a857-186">3.2.4.</span></span> <span data-ttu-id="9a857-187">Odebrat nákupního košíku událostí</span><span class="sxs-lookup"><span data-stu-id="9a857-187">Remove Shopping Cart Event</span></span>
<span data-ttu-id="9a857-188">Tato událost se mají použít při uživatel odebere položku, kterou chcete nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="9a857-188">This event should be used when the user removes an item to the shopping cart.</span></span>

<span data-ttu-id="9a857-189">Parametry:</span><span class="sxs-lookup"><span data-stu-id="9a857-189">Parameters:</span></span>

* <span data-ttu-id="9a857-190">události (řetězec, povinná) "removeshopcart"</span><span class="sxs-lookup"><span data-stu-id="9a857-190">event (string, mandatory) - “removeshopcart”</span></span>
* <span data-ttu-id="9a857-191">Položka jedinečný identifikátor položky (řetězec, povinné)</span><span class="sxs-lookup"><span data-stu-id="9a857-191">item (string, mandatory) - Unique identifier of the item</span></span>
* <span data-ttu-id="9a857-192">Název položky (řetězec, volitelné) název položky</span><span class="sxs-lookup"><span data-stu-id="9a857-192">itemName (string, optional) - the name of the item</span></span>
* <span data-ttu-id="9a857-193">Popis zboží (řetězec, volitelný) – popis položky</span><span class="sxs-lookup"><span data-stu-id="9a857-193">itemDescription (string, optional) - the description of the item</span></span>
* <span data-ttu-id="9a857-194">itemCategory (řetězec, volitelný) kategorie položky</span><span class="sxs-lookup"><span data-stu-id="9a857-194">itemCategory (string, optional) - the category of the item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a><span data-ttu-id="9a857-195">3.2.5.</span><span class="sxs-lookup"><span data-stu-id="9a857-195">3.2.5.</span></span> <span data-ttu-id="9a857-196">Nákup událostí</span><span class="sxs-lookup"><span data-stu-id="9a857-196">Purchase Event</span></span>
<span data-ttu-id="9a857-197">Tato událost je třeba použít při zakoupení uživatele jeho nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="9a857-197">This event should be used when the user purchased his shopping cart.</span></span>

<span data-ttu-id="9a857-198">Parametry:</span><span class="sxs-lookup"><span data-stu-id="9a857-198">Parameters:</span></span>

* <span data-ttu-id="9a857-199">události (řetězec) "zakoupit"</span><span class="sxs-lookup"><span data-stu-id="9a857-199">event (string) - “purchase”</span></span>
* <span data-ttu-id="9a857-200">položky (zakoupili []) – pole, která uchovává záznam pro každou položku zakoupili.</span><span class="sxs-lookup"><span data-stu-id="9a857-200">items ( Purchased[] ) - Array holding an entry for each item purchased.</span></span><br><br>
  <span data-ttu-id="9a857-201">Zakoupené formát:</span><span class="sxs-lookup"><span data-stu-id="9a857-201">Purchased format:</span></span>
  * <span data-ttu-id="9a857-202">Položka (řetězec) Jedinečný identifikátor položky.</span><span class="sxs-lookup"><span data-stu-id="9a857-202">item (string) - Unique identifier of the item.</span></span>
  * <span data-ttu-id="9a857-203">Count (int nebo string) - počet položek, které byly zakoupeny.</span><span class="sxs-lookup"><span data-stu-id="9a857-203">count (int or string) - number of items that were purchased.</span></span>
  * <span data-ttu-id="9a857-204">cena (float nebo řetězec) - volitelné pole - cenu položky.</span><span class="sxs-lookup"><span data-stu-id="9a857-204">price (float or string) - optional field - the price of the item.</span></span>

<span data-ttu-id="9a857-205">Následující příklad ukazuje zakoupení 3 položky (33, 34, 35), dva s všechna pole naplněno (položky, počet, cena) a jeden (položka 34) bez cenu.</span><span class="sxs-lookup"><span data-stu-id="9a857-205">The example below shows purchase of 3 items (33, 34, 35), two with all fields populated (item, count, price) and one (item 34) without a price.</span></span>

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a><span data-ttu-id="9a857-206">3.2.6.</span><span class="sxs-lookup"><span data-stu-id="9a857-206">3.2.6.</span></span> <span data-ttu-id="9a857-207">Události přihlášení uživatele</span><span class="sxs-lookup"><span data-stu-id="9a857-207">User Login Event</span></span>
<span data-ttu-id="9a857-208">Knihovna Azure ML doporučení událostí vytvoří a používat soubor cookie za účelem zjištění události, které pocházejí ze stejného prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="9a857-208">Azure ML Recommendations Event library creates and use a cookie in order to identify events that came from the same browser.</span></span> <span data-ttu-id="9a857-209">Za účelem zlepšení výsledky modelu Azure ML doporučení umožňuje nastavit jedinečnou identifikaci uživatele, který přepíše soubor cookie využití.</span><span class="sxs-lookup"><span data-stu-id="9a857-209">In order to improve the model results Azure ML Recommendations enables to set a user unique identification that will override the cookie usage.</span></span>

<span data-ttu-id="9a857-210">Tato událost je třeba použít po přihlášení uživatele na váš web.</span><span class="sxs-lookup"><span data-stu-id="9a857-210">This event should be used after the user login to your site.</span></span>

<span data-ttu-id="9a857-211">Parametry:</span><span class="sxs-lookup"><span data-stu-id="9a857-211">Parameters:</span></span>

* <span data-ttu-id="9a857-212">události (řetězec) "userlogin"</span><span class="sxs-lookup"><span data-stu-id="9a857-212">event (string) - “userlogin”</span></span>
* <span data-ttu-id="9a857-213">uživatel (řetězec) jedinečnou identifikaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="9a857-213">user (string) - unique identification of the user.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a><span data-ttu-id="9a857-214">4. Využívat doporučení prostřednictvím jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="9a857-214">4. Consume Recommendations via JavaScript</span></span>
<span data-ttu-id="9a857-215">Kód, který využívá doporučení je aktivován některé událostí jazyka JavaScript klienta webové stránky.</span><span class="sxs-lookup"><span data-stu-id="9a857-215">The code that consumes the recommendation is triggered by some JavaScript event by the client’s webpage.</span></span> <span data-ttu-id="9a857-216">Odpověď doporučení obsahuje ID položky doporučené, jejich názvy a svoje hodnocení.</span><span class="sxs-lookup"><span data-stu-id="9a857-216">The recommendation response includes the recommended items Ids, their names and their ratings.</span></span> <span data-ttu-id="9a857-217">Je vhodné tuto možnost použít pouze pro zobrazení seznamu položek doporučené – složitější zpracování (jako je například přidávání položky metadat) by mělo být provedeno na straně integrace serveru.</span><span class="sxs-lookup"><span data-stu-id="9a857-217">It’s best to use this option only for a list display of the recommended items - more complex handling (such as adding the item’s metadata) should be done on the server side integration.</span></span>

### <a name="41-consume-recommendations"></a><span data-ttu-id="9a857-218">4.1 využívat doporučení</span><span class="sxs-lookup"><span data-stu-id="9a857-218">4.1 Consume Recommendations</span></span>
<span data-ttu-id="9a857-219">Využívat doporučení, které je třeba zahrnout požadované knihovny jazyka JavaScript v stránku a volat AzureMLRecommendationsStart.</span><span class="sxs-lookup"><span data-stu-id="9a857-219">To consume recommendations you need to include the required JavaScript libraries in your page and to call AzureMLRecommendationsStart.</span></span> <span data-ttu-id="9a857-220">Najdete v části 2.</span><span class="sxs-lookup"><span data-stu-id="9a857-220">See section 2.</span></span>

<span data-ttu-id="9a857-221">Využívat doporučení pro jednu nebo více položek, které je třeba volat metodu s názvem: AzureMLRecommendationsGetI2IRecommendation.</span><span class="sxs-lookup"><span data-stu-id="9a857-221">To consume recommendations for one or more items you need to call a method called: AzureMLRecommendationsGetI2IRecommendation.</span></span>

<span data-ttu-id="9a857-222">Parametry:</span><span class="sxs-lookup"><span data-stu-id="9a857-222">Parameters:</span></span>

* <span data-ttu-id="9a857-223">položky (pole řetězce) - získat doporučení pro jednu nebo více položek.</span><span class="sxs-lookup"><span data-stu-id="9a857-223">items (array of strings) - One or more items to get recommendations for.</span></span> <span data-ttu-id="9a857-224">Pokud Fbt sestavení využívat, pak můžete nastavit zde jen jednu položku.</span><span class="sxs-lookup"><span data-stu-id="9a857-224">If you consume an Fbt build then you can set here only one item.</span></span>
* <span data-ttu-id="9a857-225">numberOfResults (int) - počet požadovaných výsledků.</span><span class="sxs-lookup"><span data-stu-id="9a857-225">numberOfResults (int) - number of required results.</span></span>
* <span data-ttu-id="9a857-226">includeMetadata (logická hodnota, volitelný) – když je nastaven na hodnotu true' značí, že metadata pole musí být naplněn ve výsledku.</span><span class="sxs-lookup"><span data-stu-id="9a857-226">includeMetadata (boolean, optional) - if set to ‘true’ indicates that the metadata field must be populated in the result.</span></span>
* <span data-ttu-id="9a857-227">Funkce – funkce, která bude zpracovávat doporučení zpracování vrátila.</span><span class="sxs-lookup"><span data-stu-id="9a857-227">Processing function - a function that will handle the recommendations returned.</span></span> <span data-ttu-id="9a857-228">Data jsou vrácena jako pole:</span><span class="sxs-lookup"><span data-stu-id="9a857-228">The data is returned as an array of:</span></span>
  * <span data-ttu-id="9a857-229">Položka – položka jedinečné id</span><span class="sxs-lookup"><span data-stu-id="9a857-229">Item - item unique id</span></span>
  * <span data-ttu-id="9a857-230">název – název položky (pokud existují v katalogu)</span><span class="sxs-lookup"><span data-stu-id="9a857-230">name - item name (if exist in catalog)</span></span>
  * <span data-ttu-id="9a857-231">hodnocení - doporučení hodnocení</span><span class="sxs-lookup"><span data-stu-id="9a857-231">rating - recommendation rating</span></span>
  * <span data-ttu-id="9a857-232">metadata – řetězec, který představuje metadata položky</span><span class="sxs-lookup"><span data-stu-id="9a857-232">metadata - a string that represents the metadata of the item</span></span>

<span data-ttu-id="9a857-233">Příklad: Následující kód požadavky 8 doporučení pro položku "64f6eb0d-947a-4c18-a16c-888da9e228ba" (a ne zadáním includeMetadata - ho implicitně upozorněním, že se nevyžaduje žádná metadata), pak řetězení výsledky do vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="9a857-233">Example: The following code requests 8 recommendations for item "64f6eb0d-947a-4c18-a16c-888da9e228ba" (and by not specifying includeMetadata - it implicitly says that no metadata is required), it then concatenate the results into a buffer.</span></span>

        <script>
             var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                 var buff = "";
                 for (var ii = 0; ii < reco.length; ii++) {
                       buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                 }
                 alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
