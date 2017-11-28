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
redirect_document_id: True
ms.openlocfilehash: 4c5f0eee4aa04ce823321d52985374c52850f0d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a><span data-ttu-id="52d43-103">Doporučení Azure Machine Learning – integrace v jazyce JavaScript</span><span class="sxs-lookup"><span data-stu-id="52d43-103">Azure Machine Learning Recommendations - JavaScript Integration</span></span>
> [!NOTE]
> <span data-ttu-id="52d43-104">Měli byste začít používat hello kognitivní služby API doporučení místo tuto verzi.</span><span class="sxs-lookup"><span data-stu-id="52d43-104">You should start using hello Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="52d43-105">Hello kognitivní službu doporučení budou nahrazení této služby, a všechny nové funkce hello bude vyvinutý existuje.</span><span class="sxs-lookup"><span data-stu-id="52d43-105">hello Recommendations Cognitive Service will be replacing this service, and all hello new features will be developed there.</span></span> <span data-ttu-id="52d43-106">Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.</span><span class="sxs-lookup"><span data-stu-id="52d43-106">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="52d43-107">Další informace o [toohello migrace nové kognitivní služby](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="52d43-107">Learn more about [Migrating toohello new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="52d43-108">Tento dokument zobrazit v ní jak toointegrate váš web pomocí jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="52d43-108">This document depict how toointegrate your site using JavaScript.</span></span> <span data-ttu-id="52d43-109">Hello JavaScript vám umožní toosend získávání dat událostí a doporučení tooconsume po sestavení modelu doporučení.</span><span class="sxs-lookup"><span data-stu-id="52d43-109">hello JavaScript enables you toosend Data Acquisition events and tooconsume recommendations once you build a recommendation model.</span></span> <span data-ttu-id="52d43-110">Všechny operace, které se provádí prostřednictvím JS lze také provést z na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="52d43-110">All operations done via JS can be also done from server side.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a><span data-ttu-id="52d43-111">1. Obecné – přehled</span><span class="sxs-lookup"><span data-stu-id="52d43-111">1. General Overview</span></span>
<span data-ttu-id="52d43-112">Integrace serveru s Azure ML doporučení se skládají na fáze 2:</span><span class="sxs-lookup"><span data-stu-id="52d43-112">Integrating your site with Azure ML Recommendations consist on 2 Phases:</span></span>

1. <span data-ttu-id="52d43-113">Odesílání událostí do Azure ML doporučení.</span><span class="sxs-lookup"><span data-stu-id="52d43-113">Send Events into Azure ML Recommendations.</span></span> <span data-ttu-id="52d43-114">Tato akce povolí toobuild model doporučení.</span><span class="sxs-lookup"><span data-stu-id="52d43-114">This will enable toobuild a recommendation model.</span></span>
2. <span data-ttu-id="52d43-115">Využívat hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="52d43-115">Consume hello recommendations.</span></span> <span data-ttu-id="52d43-116">Jakmile je vytvořen hello modelu můžete využívat hello doporučení.</span><span class="sxs-lookup"><span data-stu-id="52d43-116">After hello model is built you can consume hello recommendations.</span></span> <span data-ttu-id="52d43-117">(Tento dokument nevysvětlují, jak toobuild modelu, přečtěte si hello úvodní příručka tooget Další informace o tom,).</span><span class="sxs-lookup"><span data-stu-id="52d43-117">(This document does not explain how toobuild a model, read hello quick start guide tooget more information on how).</span></span>

<span data-ttu-id="52d43-118"><ins>Fáze I</ins></span><span class="sxs-lookup"><span data-stu-id="52d43-118"><ins>Phase I</ins></span></span>

<span data-ttu-id="52d43-119">V hello první fázi vložit do stránek html malé knihovna JavaScript, která umožňuje hello stránky toosend událostí, kdy k nim dojde na stránce html hello do Azure ML doporučení servery (prostřednictvím trhu dat):</span><span class="sxs-lookup"><span data-stu-id="52d43-119">In hello first phase you insert into your html pages a small JavaScript library that enables hello page toosend events as they occur on hello html page into Azure ML Recommendations servers (via Data Market):</span></span>

![Výkres1][1]

<span data-ttu-id="52d43-121"><ins>Fáze II</ins></span><span class="sxs-lookup"><span data-stu-id="52d43-121"><ins>Phase II</ins></span></span>

<span data-ttu-id="52d43-122">V hello druhé fáze, když chcete tooshow hello doporučení na stránce hello vyberete jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="52d43-122">In hello second phase when you want tooshow hello recommendations on hello page you select one of hello following options:</span></span>

<span data-ttu-id="52d43-123">1 serveru (ve fázi hello vykreslování stránky) volá doporučení tooget Azure ML doporučení serveru (prostřednictvím trhu Data).</span><span class="sxs-lookup"><span data-stu-id="52d43-123">1.Your server (on hello phase of page rendering) calls Azure ML Recommendations Server (via Data Market) tooget recommendations.</span></span> <span data-ttu-id="52d43-124">Hello výsledky budou zahrnovat seznam id položky. Váš server potřebuje tooenrich hello výsledky s položkami hello metadata (např. obrázky, popis) a odesílat hello vytvořit stránku toohello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="52d43-124">hello results include a list of items id. Your server needs tooenrich hello results with hello items Meta data (e.g. images, description) and send hello created page toohello browser.</span></span>

![Drawing2][2]

<span data-ttu-id="52d43-126">2. hello Další možností je toouse hello malý JavaScript soubor z fáze jeden tooget jednoduché doporučené položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="52d43-126">2.hello other option is toouse hello small JavaScript file from phase one tooget a simple list of recommended items.</span></span> <span data-ttu-id="52d43-127">je zde přijatá data Hello zeštíhlen než hello, jeden v první možnosti hello.</span><span class="sxs-lookup"><span data-stu-id="52d43-127">hello data received here is leaner than hello one in hello first option.</span></span>

![Drawing3][3]

## <a name="2-prerequisites"></a><span data-ttu-id="52d43-129">2. Požadavky</span><span class="sxs-lookup"><span data-stu-id="52d43-129">2. Prerequisites</span></span>
1. <span data-ttu-id="52d43-130">Vytvořte nový model pomocí hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="52d43-130">Create a new model using hello APIs.</span></span> <span data-ttu-id="52d43-131">O tom najdete úvodní příručka hello toodo ho.</span><span class="sxs-lookup"><span data-stu-id="52d43-131">See hello Quick start guide on how toodo it.</span></span>
2. <span data-ttu-id="52d43-132">Zakódovat váš &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; pomocí base64.</span><span class="sxs-lookup"><span data-stu-id="52d43-132">Encode your &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; with base64.</span></span> <span data-ttu-id="52d43-133">(Bude se používat pro hello základní ověřování tooenable hello JS kód toocall hello rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="52d43-133">(This will be used for hello basic authentication tooenable hello JS code toocall hello APIs).</span></span>

## <a name="3-send-data-acquisition-events-using-javascript"></a><span data-ttu-id="52d43-134">3. Odesílání událostí získávání dat pomocí jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="52d43-134">3. Send Data Acquisition events using JavaScript</span></span>
<span data-ttu-id="52d43-135">Následující kroky Hello usnadňují odesílání událostí:</span><span class="sxs-lookup"><span data-stu-id="52d43-135">hello following steps facilitate sending events:</span></span>

1. <span data-ttu-id="52d43-136">Zahrnují knihovny JQuery ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="52d43-136">Include JQuery library in your code.</span></span> <span data-ttu-id="52d43-137">Můžete ji stáhnout z nuget v hello následující adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52d43-137">You can download it from nuget in hello following URL.</span></span>
   
     <span data-ttu-id="52d43-138">http://www.nuget.org/Packages/jQuery/1.8.2</span><span class="sxs-lookup"><span data-stu-id="52d43-138">http://www.nuget.org/packages/jQuery/1.8.2</span></span>
2. <span data-ttu-id="52d43-139">Zahrnout hello skriptu Java doporučení knihovny z hello následující adresu URL: http://aka.ms/RecoJSLib1</span><span class="sxs-lookup"><span data-stu-id="52d43-139">Include hello Recommendations Java Script library from hello following URL: http://aka.ms/RecoJSLib1</span></span>
3. <span data-ttu-id="52d43-140">Inicializace knihovny Azure ML doporučení s příslušnými parametry hello.</span><span class="sxs-lookup"><span data-stu-id="52d43-140">Initialize Azure ML Recommendations library with hello appropriate parameters.</span></span>
   
     <span data-ttu-id="52d43-141"><script>AzureMLRecommendationsStart ("<base64encoding of username:key>", "< model_id >"); </script> 
4. Odesílání hello příslušné události.</span><span class="sxs-lookup"><span data-stu-id="52d43-141"><script> AzureMLRecommendationsStart("<base64encoding of username:key>", "<model_id>"); </script>
4. Send hello appropriate event.</span></span> <span data-ttu-id="52d43-142">Najdete v podrobné části níže na všechny typu události (příklad click – událost) <script> Pokud (typeof AzureMLRecommendationsEvent == "undefined") {</span><span class="sxs-lookup"><span data-stu-id="52d43-142">See detailed section below on all type of events (example of click event)  <script> if (typeof AzureMLRecommendationsEvent=="undefined") {</span></span>         
                     <span data-ttu-id="52d43-143">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script></span><span class="sxs-lookup"><span data-stu-id="52d43-143">AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" }); </script></span></span>

### <a name="31----limitations-and-browser-support"></a><span data-ttu-id="52d43-144">3.1.</span><span class="sxs-lookup"><span data-stu-id="52d43-144">3.1.</span></span>    <span data-ttu-id="52d43-145">Omezení a podpora prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="52d43-145">Limitations and Browser Support</span></span>
<span data-ttu-id="52d43-146">Toto je odkaz na implementaci a je zadána, jako je.</span><span class="sxs-lookup"><span data-stu-id="52d43-146">This is a reference implementation and it is given as is.</span></span> <span data-ttu-id="52d43-147">Má podporovat všechny hlavní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="52d43-147">It should support all major browsers.</span></span>

### <a name="32----type-of-events"></a><span data-ttu-id="52d43-148">3.2.</span><span class="sxs-lookup"><span data-stu-id="52d43-148">3.2.</span></span>    <span data-ttu-id="52d43-149">Typ události</span><span class="sxs-lookup"><span data-stu-id="52d43-149">Type of Events</span></span>
<span data-ttu-id="52d43-150">Existují 5 typy událostí, které hello knihovně podporuje: klikněte na, klikněte na doporučení, přidejte tooShop košíku, odeberte z košíku obchod a nákup.</span><span class="sxs-lookup"><span data-stu-id="52d43-150">There are 5 types of event that hello library supports: Click, Recommendation Click, Add tooShop Cart, Remove from Shop Cart and Purchase.</span></span> <span data-ttu-id="52d43-151">Není k dispozici další událost, která je volána přihlášení použité tooset hello uživatelský kontext.</span><span class="sxs-lookup"><span data-stu-id="52d43-151">There is an additional event that is used tooset hello user context called Login.</span></span>

#### <a name="321-click-event"></a><span data-ttu-id="52d43-152">3.2.1.</span><span class="sxs-lookup"><span data-stu-id="52d43-152">3.2.1.</span></span> <span data-ttu-id="52d43-153">Click – událost</span><span class="sxs-lookup"><span data-stu-id="52d43-153">Click Event</span></span>
<span data-ttu-id="52d43-154">Tato událost je třeba použít kdykoli uživatel klikli na položku.</span><span class="sxs-lookup"><span data-stu-id="52d43-154">This event should be used any time a user clicked on an item.</span></span> <span data-ttu-id="52d43-155">Obvykle, když uživatel klikne na položku Nová stránka se otevře s podrobnostmi položky hello; na této stránce musí být tato událost aktivuje.</span><span class="sxs-lookup"><span data-stu-id="52d43-155">Usually when user clicks on an item a new page is opened with hello item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="52d43-156">Parametry:</span><span class="sxs-lookup"><span data-stu-id="52d43-156">Parameters:</span></span>

* <span data-ttu-id="52d43-157">události (řetězec, povinná) "Kliknutím"</span><span class="sxs-lookup"><span data-stu-id="52d43-157">event (string, mandatory) - “click”</span></span>
* <span data-ttu-id="52d43-158">Položka jedinečný identifikátor položky hello (řetězec, povinné)</span><span class="sxs-lookup"><span data-stu-id="52d43-158">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="52d43-159">Název položky (řetězec, volitelný) hello název položky hello</span><span class="sxs-lookup"><span data-stu-id="52d43-159">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="52d43-160">Popis zboží (řetězec, volitelný) hello popis položky hello</span><span class="sxs-lookup"><span data-stu-id="52d43-160">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="52d43-161">itemCategory (řetězec, volitelný) hello kategorie hello položky</span><span class="sxs-lookup"><span data-stu-id="52d43-161">itemCategory (string, optional) - hello category of hello item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

<span data-ttu-id="52d43-162">Nebo s volitelnými daty:</span><span class="sxs-lookup"><span data-stu-id="52d43-162">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


#### <a name="322-recommendation-click-event"></a><span data-ttu-id="52d43-163">3.2.2.</span><span class="sxs-lookup"><span data-stu-id="52d43-163">3.2.2.</span></span> <span data-ttu-id="52d43-164">Doporučení Click – událost</span><span class="sxs-lookup"><span data-stu-id="52d43-164">Recommendation Click Event</span></span>
<span data-ttu-id="52d43-165">Tato událost je třeba použít kdykoli uživatel klikli na položku, který jste získali od Azure ML doporučení jako doporučenou položka.</span><span class="sxs-lookup"><span data-stu-id="52d43-165">This event should be used any time a user clicked on an item that was received from Azure ML Recommendations as a recommended item.</span></span> <span data-ttu-id="52d43-166">Obvykle, když uživatel klikne na položku Nová stránka se otevře s podrobnostmi položky hello; na této stránce musí být tato událost aktivuje.</span><span class="sxs-lookup"><span data-stu-id="52d43-166">Usually when user clicks on an item a new page is opened with hello item details; in this page this event should be triggered.</span></span>

<span data-ttu-id="52d43-167">Parametry:</span><span class="sxs-lookup"><span data-stu-id="52d43-167">Parameters:</span></span>

* <span data-ttu-id="52d43-168">události (řetězec, povinná) "recommendationclick"</span><span class="sxs-lookup"><span data-stu-id="52d43-168">event (string, mandatory) - “recommendationclick”</span></span>
* <span data-ttu-id="52d43-169">Položka jedinečný identifikátor položky hello (řetězec, povinné)</span><span class="sxs-lookup"><span data-stu-id="52d43-169">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="52d43-170">Název položky (řetězec, volitelný) hello název položky hello</span><span class="sxs-lookup"><span data-stu-id="52d43-170">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="52d43-171">Popis zboží (řetězec, volitelný) hello popis položky hello</span><span class="sxs-lookup"><span data-stu-id="52d43-171">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="52d43-172">itemCategory (řetězec, volitelný) hello kategorie hello položky</span><span class="sxs-lookup"><span data-stu-id="52d43-172">itemCategory (string, optional) - hello category of hello item</span></span>
* <span data-ttu-id="52d43-173">semen (pole řetězců, volitelný) - hello semen, které generuje hello doporučení dotazu.</span><span class="sxs-lookup"><span data-stu-id="52d43-173">seeds (string array, optional) - hello seeds that generated hello recommendation query.</span></span>
* <span data-ttu-id="52d43-174">recoList (pole řetězců, volitelný) - hello výsledek požadavku hello doporučení, která vygenerovala hello položku, která označeného.</span><span class="sxs-lookup"><span data-stu-id="52d43-174">recoList (string array, optional) - hello result of hello recommendation request that generated hello item that was clicked.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

<span data-ttu-id="52d43-175">Nebo s volitelnými daty:</span><span class="sxs-lookup"><span data-stu-id="52d43-175">Or with optional data:</span></span>

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]                 });
        </script>


#### <a name="323-add-shopping-cart-event"></a><span data-ttu-id="52d43-176">3.2.3.</span><span class="sxs-lookup"><span data-stu-id="52d43-176">3.2.3.</span></span> <span data-ttu-id="52d43-177">Přidat událost nákupní košík</span><span class="sxs-lookup"><span data-stu-id="52d43-177">Add Shopping Cart Event</span></span>
<span data-ttu-id="52d43-178">Tato událost má použít při přidání uživatele hello toohello položky nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="52d43-178">This event should be used when hello user add an item toohello shopping cart.</span></span>
<span data-ttu-id="52d43-179">Parametry:</span><span class="sxs-lookup"><span data-stu-id="52d43-179">Parameters:</span></span>

* <span data-ttu-id="52d43-180">události (řetězec, povinná) "addshopcart"</span><span class="sxs-lookup"><span data-stu-id="52d43-180">event (string, mandatory) - “addshopcart”</span></span>
* <span data-ttu-id="52d43-181">Položka jedinečný identifikátor položky hello (řetězec, povinné)</span><span class="sxs-lookup"><span data-stu-id="52d43-181">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="52d43-182">Název položky (řetězec, volitelný) hello název položky hello</span><span class="sxs-lookup"><span data-stu-id="52d43-182">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="52d43-183">Popis zboží (řetězec, volitelný) hello popis položky hello</span><span class="sxs-lookup"><span data-stu-id="52d43-183">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="52d43-184">itemCategory (řetězec, volitelný) hello kategorie hello položky</span><span class="sxs-lookup"><span data-stu-id="52d43-184">itemCategory (string, optional) - hello category of hello item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a><span data-ttu-id="52d43-185">3.2.4.</span><span class="sxs-lookup"><span data-stu-id="52d43-185">3.2.4.</span></span> <span data-ttu-id="52d43-186">Odebrat nákupního košíku událostí</span><span class="sxs-lookup"><span data-stu-id="52d43-186">Remove Shopping Cart Event</span></span>
<span data-ttu-id="52d43-187">Tato událost se mají použít při hello uživatel odebere k položce toohello nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="52d43-187">This event should be used when hello user removes an item toohello shopping cart.</span></span>

<span data-ttu-id="52d43-188">Parametry:</span><span class="sxs-lookup"><span data-stu-id="52d43-188">Parameters:</span></span>

* <span data-ttu-id="52d43-189">události (řetězec, povinná) "removeshopcart"</span><span class="sxs-lookup"><span data-stu-id="52d43-189">event (string, mandatory) - “removeshopcart”</span></span>
* <span data-ttu-id="52d43-190">Položka jedinečný identifikátor položky hello (řetězec, povinné)</span><span class="sxs-lookup"><span data-stu-id="52d43-190">item (string, mandatory) - Unique identifier of hello item</span></span>
* <span data-ttu-id="52d43-191">Název položky (řetězec, volitelný) hello název položky hello</span><span class="sxs-lookup"><span data-stu-id="52d43-191">itemName (string, optional) - hello name of hello item</span></span>
* <span data-ttu-id="52d43-192">Popis zboží (řetězec, volitelný) hello popis položky hello</span><span class="sxs-lookup"><span data-stu-id="52d43-192">itemDescription (string, optional) - hello description of hello item</span></span>
* <span data-ttu-id="52d43-193">itemCategory (řetězec, volitelný) hello kategorie hello položky</span><span class="sxs-lookup"><span data-stu-id="52d43-193">itemCategory (string, optional) - hello category of hello item</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a><span data-ttu-id="52d43-194">3.2.5.</span><span class="sxs-lookup"><span data-stu-id="52d43-194">3.2.5.</span></span> <span data-ttu-id="52d43-195">Nákup událostí</span><span class="sxs-lookup"><span data-stu-id="52d43-195">Purchase Event</span></span>
<span data-ttu-id="52d43-196">Tato událost je třeba použít při zakoupení hello uživatele jeho nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="52d43-196">This event should be used when hello user purchased his shopping cart.</span></span>

<span data-ttu-id="52d43-197">Parametry:</span><span class="sxs-lookup"><span data-stu-id="52d43-197">Parameters:</span></span>

* <span data-ttu-id="52d43-198">události (řetězec) "zakoupit"</span><span class="sxs-lookup"><span data-stu-id="52d43-198">event (string) - “purchase”</span></span>
* <span data-ttu-id="52d43-199">položky (zakoupili []) – pole, která uchovává záznam pro každou položku zakoupili.</span><span class="sxs-lookup"><span data-stu-id="52d43-199">items ( Purchased[] ) - Array holding an entry for each item purchased.</span></span><br><br>
  <span data-ttu-id="52d43-200">Zakoupené formát:</span><span class="sxs-lookup"><span data-stu-id="52d43-200">Purchased format:</span></span>
  * <span data-ttu-id="52d43-201">Položka (řetězec) Jedinečný identifikátor položky hello.</span><span class="sxs-lookup"><span data-stu-id="52d43-201">item (string) - Unique identifier of hello item.</span></span>
  * <span data-ttu-id="52d43-202">Count (int nebo string) - počet položek, které byly zakoupeny.</span><span class="sxs-lookup"><span data-stu-id="52d43-202">count (int or string) - number of items that were purchased.</span></span>
  * <span data-ttu-id="52d43-203">cena (float nebo řetězec) - volitelné pole - hello cena hello položky.</span><span class="sxs-lookup"><span data-stu-id="52d43-203">price (float or string) - optional field - hello price of hello item.</span></span>

<span data-ttu-id="52d43-204">Následující příklad Hello ukazuje zakoupení 3 položky (33, 34, 35), dva s všechna pole naplněno (položky, počet, cena) a jeden (položka 34) bez cenu.</span><span class="sxs-lookup"><span data-stu-id="52d43-204">hello example below shows purchase of 3 items (33, 34, 35), two with all fields populated (item, count, price) and one (item 34) without a price.</span></span>

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a><span data-ttu-id="52d43-205">3.2.6.</span><span class="sxs-lookup"><span data-stu-id="52d43-205">3.2.6.</span></span> <span data-ttu-id="52d43-206">Události přihlášení uživatele</span><span class="sxs-lookup"><span data-stu-id="52d43-206">User Login Event</span></span>
<span data-ttu-id="52d43-207">Azure ML doporučení události vytvoří knihovny a používání souborů cookie v pořadí tooidentify události, které pocházejí z hello stejný prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="52d43-207">Azure ML Recommendations Event library creates and use a cookie in order tooidentify events that came from hello same browser.</span></span> <span data-ttu-id="52d43-208">V pořadí tooimprove hello modelu umožňuje výsledky Azure ML doporučení tooset jedinečnou identifikaci uživatele, který přepíše soubor cookie využití hello.</span><span class="sxs-lookup"><span data-stu-id="52d43-208">In order tooimprove hello model results Azure ML Recommendations enables tooset a user unique identification that will override hello cookie usage.</span></span>

<span data-ttu-id="52d43-209">Tato událost je třeba použít po hello uživatele přihlášení tooyour lokality.</span><span class="sxs-lookup"><span data-stu-id="52d43-209">This event should be used after hello user login tooyour site.</span></span>

<span data-ttu-id="52d43-210">Parametry:</span><span class="sxs-lookup"><span data-stu-id="52d43-210">Parameters:</span></span>

* <span data-ttu-id="52d43-211">události (řetězec) "userlogin"</span><span class="sxs-lookup"><span data-stu-id="52d43-211">event (string) - “userlogin”</span></span>
* <span data-ttu-id="52d43-212">uživatel (řetězec) jedinečnou identifikaci uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="52d43-212">user (string) - unique identification of hello user.</span></span>
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a><span data-ttu-id="52d43-213">4. Využívat doporučení prostřednictvím jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="52d43-213">4. Consume Recommendations via JavaScript</span></span>
<span data-ttu-id="52d43-214">Hello kód, který využívá hello doporučení je aktivován některé událostí jazyka JavaScript webovou stránku hello klienta.</span><span class="sxs-lookup"><span data-stu-id="52d43-214">hello code that consumes hello recommendation is triggered by some JavaScript event by hello client’s webpage.</span></span> <span data-ttu-id="52d43-215">Hello doporučení odpověď obsahuje hello doporučené ID položky, jejich názvy a svoje hodnocení.</span><span class="sxs-lookup"><span data-stu-id="52d43-215">hello recommendation response includes hello recommended items Ids, their names and their ratings.</span></span> <span data-ttu-id="52d43-216">Je nejlepší toouse, které tato možnost jenom pro zobrazení seznamu hello doporučená položky - složitější zpracování (jako je například přidávání metadata položky hello) by mělo být provedeno na hello integrace straně serveru.</span><span class="sxs-lookup"><span data-stu-id="52d43-216">It’s best toouse this option only for a list display of hello recommended items - more complex handling (such as adding hello item’s metadata) should be done on hello server side integration.</span></span>

### <a name="41-consume-recommendations"></a><span data-ttu-id="52d43-217">4.1 využívat doporučení</span><span class="sxs-lookup"><span data-stu-id="52d43-217">4.1 Consume Recommendations</span></span>
<span data-ttu-id="52d43-218">tooconsume doporučení, které že budete potřebovat tooinclude hello požadované knihovny jazyka JavaScript v stránky a toocall AzureMLRecommendationsStart.</span><span class="sxs-lookup"><span data-stu-id="52d43-218">tooconsume recommendations you need tooinclude hello required JavaScript libraries in your page and toocall AzureMLRecommendationsStart.</span></span> <span data-ttu-id="52d43-219">Najdete v části 2.</span><span class="sxs-lookup"><span data-stu-id="52d43-219">See section 2.</span></span>

<span data-ttu-id="52d43-220">tooconsume doporučení pro jednu nebo více položek, je třeba volat metodu toocall: AzureMLRecommendationsGetI2IRecommendation.</span><span class="sxs-lookup"><span data-stu-id="52d43-220">tooconsume recommendations for one or more items you need toocall a method called: AzureMLRecommendationsGetI2IRecommendation.</span></span>

<span data-ttu-id="52d43-221">Parametry:</span><span class="sxs-lookup"><span data-stu-id="52d43-221">Parameters:</span></span>

* <span data-ttu-id="52d43-222">položky (pole řetězce) – jeden nebo více položek tooget doporučení pro.</span><span class="sxs-lookup"><span data-stu-id="52d43-222">items (array of strings) - One or more items tooget recommendations for.</span></span> <span data-ttu-id="52d43-223">Pokud Fbt sestavení využívat, pak můžete nastavit zde jen jednu položku.</span><span class="sxs-lookup"><span data-stu-id="52d43-223">If you consume an Fbt build then you can set here only one item.</span></span>
* <span data-ttu-id="52d43-224">numberOfResults (int) - počet požadovaných výsledků.</span><span class="sxs-lookup"><span data-stu-id="52d43-224">numberOfResults (int) - number of required results.</span></span>
* <span data-ttu-id="52d43-225">includeMetadata (logická hodnota, volitelný) – Pokud nastavit too'true "označuje, že hello metadata pole musí být naplněn ve výsledku hello.</span><span class="sxs-lookup"><span data-stu-id="52d43-225">includeMetadata (boolean, optional) - if set too‘true’ indicates that hello metadata field must be populated in hello result.</span></span>
* <span data-ttu-id="52d43-226">Funkce – funkce, která bude zpracovávat hello doporučení zpracování vrátila.</span><span class="sxs-lookup"><span data-stu-id="52d43-226">Processing function - a function that will handle hello recommendations returned.</span></span> <span data-ttu-id="52d43-227">Hello data jsou vrácena jako pole:</span><span class="sxs-lookup"><span data-stu-id="52d43-227">hello data is returned as an array of:</span></span>
  * <span data-ttu-id="52d43-228">Položka – položka jedinečné id</span><span class="sxs-lookup"><span data-stu-id="52d43-228">Item - item unique id</span></span>
  * <span data-ttu-id="52d43-229">název – název položky (pokud existují v katalogu)</span><span class="sxs-lookup"><span data-stu-id="52d43-229">name - item name (if exist in catalog)</span></span>
  * <span data-ttu-id="52d43-230">hodnocení - doporučení hodnocení</span><span class="sxs-lookup"><span data-stu-id="52d43-230">rating - recommendation rating</span></span>
  * <span data-ttu-id="52d43-231">metadata – řetězec, který představuje hello metadata položky hello</span><span class="sxs-lookup"><span data-stu-id="52d43-231">metadata - a string that represents hello metadata of hello item</span></span>

<span data-ttu-id="52d43-232">Příklad: hello následující kód požadavky 8 doporučení pro položku "64f6eb0d-947a-4c18-a16c-888da9e228ba" (a ne zadáním includeMetadata - ho implicitně upozorněním, že se nevyžaduje žádná metadata), pak řetězení hello výsledky do vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="52d43-232">Example: hello following code requests 8 recommendations for item "64f6eb0d-947a-4c18-a16c-888da9e228ba" (and by not specifying includeMetadata - it implicitly says that no metadata is required), it then concatenate hello results into a buffer.</span></span>

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
