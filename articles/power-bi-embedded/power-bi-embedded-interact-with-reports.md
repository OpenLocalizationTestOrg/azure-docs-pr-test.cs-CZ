---
title: "aaaInteract s sestav pomocí hello rozhraní API jazyka JavaScript | Microsoft Docs"
description: "Power BI Embedded, interakce se zprávami pomocí hello rozhraní API jazyka JavaScript"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: bdd885d3-1b00-4dcf-bdff-531eb1f97bfb
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: 657e4d5cee031bdda173ab3f451cc19b93ddb17b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="interact-with-power-bi-reports-using-hello-javascript-api"></a><span data-ttu-id="3f372-103">Ovládejte sestavy Power BI pomocí hello rozhraní API jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="3f372-103">Interact with Power BI reports using hello JavaScript API</span></span>
<span data-ttu-id="3f372-104">umožňuje rozhraní API jazyka JavaScript Power BI Hello je tooeasily vložení sestavy Power BI do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f372-104">hello Power BI JavaScript API enables you tooeasily embed Power BI reports into your applications.</span></span> <span data-ttu-id="3f372-105">S hello rozhraní API mohou vaše aplikace prostřednictvím kódu programu komunikovat s prvky různé sestavy jako stránky a filtry.</span><span class="sxs-lookup"><span data-stu-id="3f372-105">With hello API, your applications can programmatically interact with different report elements like pages and filters.</span></span> <span data-ttu-id="3f372-106">Díky této interaktivitě se sestavy Power BI stanou integrálnější součástí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f372-106">This interactivity makes Power BI reports a more integrated part of your application.</span></span>

<span data-ttu-id="3f372-107">Vložení sestavy Power BI ve vaší aplikaci pomocí elementu iframe, který je hostován v rámci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3f372-107">You embed a Power BI report in your application by using an iframe that is hosted as part of hello application.</span></span> <span data-ttu-id="3f372-108">Hello iframe slouží jako hranice mezi aplikací a hello sestavy, jak můžete vidět v hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="3f372-108">hello iframe acts as a boundary between your application and hello report, as you can see in hello following image.</span></span> 

![Vložený element iframe Power BI bez rozhraní API pro Javascript](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-1.png)

<span data-ttu-id="3f372-110">Hello iframe hello vložení bylo mnohem snazší proces se však není hello rozhraní API jazyka JavaScript hello sestavy a aplikace nemohou komunikovat s navzájem.</span><span class="sxs-lookup"><span data-stu-id="3f372-110">hello iframe makes hello embedding process a lot easier, but without hello JavaScript API hello report and your application can't interact with each other.</span></span> <span data-ttu-id="3f372-111">Kvůli chybějící interakce může být působí jako skutečně hello sestava není součástí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3f372-111">This lack of interaction can make it feel like hello report is not really part of hello application.</span></span> <span data-ttu-id="3f372-112">Hello sestavy a aplikaci skutečně potřebujete toocommunicate a zpět, stejně jako hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="3f372-112">hello report and application really need toocommunicate back and forth, as in hello following image.</span></span>

![Vložený element iframe Power BI s rozhraním API pro Javascript](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-2.png)

<span data-ttu-id="3f372-114">Hello rozhraní API jazyka JavaScript Power BI vám umožňuje toowrite kód, který můžete bezpečně předávat hello iframe hranic.</span><span class="sxs-lookup"><span data-stu-id="3f372-114">hello Power BI JavaScript API enables you toowrite code that can securely pass through hello iframe boundary.</span></span> <span data-ttu-id="3f372-115">Díky této může vaše aplikace tooprogrammatically provedení akce v sestavu a toolisten pro události z akcí, které uživatelé provádět v rámci sestavy hello.</span><span class="sxs-lookup"><span data-stu-id="3f372-115">This enables your application tooprogrammatically perform an action in a report, and toolisten for events from actions that users make within hello report.</span></span>

## <a name="what-can-you-do-with-hello-power-bi-javascript-api"></a><span data-ttu-id="3f372-116">Co se děje s hello rozhraní API jazyka JavaScript Power BI?</span><span class="sxs-lookup"><span data-stu-id="3f372-116">What can you do with hello Power BI JavaScript API?</span></span>
<span data-ttu-id="3f372-117">Hello JavaScript API můžete spravovat sestavy, přejděte toopages v sestavě, filtrování sestavy a zpracování události vložení.</span><span class="sxs-lookup"><span data-stu-id="3f372-117">With hello JavaScript API you can manage reports, navigate toopages in a report, filter a report, and handle embedding events.</span></span> <span data-ttu-id="3f372-118">Hello následující diagram znázorňuje hello struktura hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3f372-118">hello following diagram shows hello structure of hello API.</span></span>

![Diagram rozhraní API pro JavaScript Power BI](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-3.png)

### <a name="manage-reports"></a><span data-ttu-id="3f372-120">Správa sestav</span><span class="sxs-lookup"><span data-stu-id="3f372-120">Manage reports</span></span>
<span data-ttu-id="3f372-121">Hello Javascript rozhraní API umožňuje toomanage chování na úrovni sestavy a stránku hello:</span><span class="sxs-lookup"><span data-stu-id="3f372-121">hello Javascript API enables you toomanage behavior at hello report and page level:</span></span>

* <span data-ttu-id="3f372-122">Vložení konkrétní sestavy Power BI bezpečně ve vaší aplikaci – zkuste hello [vložení ukázkové aplikace](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)</span><span class="sxs-lookup"><span data-stu-id="3f372-122">Embed a specific Power BI Report securely in your application - try hello [embed demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)</span></span>
  * <span data-ttu-id="3f372-123">Nastavení přístupového tokenu</span><span class="sxs-lookup"><span data-stu-id="3f372-123">Set access token</span></span>
* <span data-ttu-id="3f372-124">Konfigurace sestav hello</span><span class="sxs-lookup"><span data-stu-id="3f372-124">Configure hello report</span></span>
  * <span data-ttu-id="3f372-125">Povolit a zakázat podokno filtru hello a podokno navigace stránky – zkuste hello [aktualizovat nastavení ukázkové aplikace](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)</span><span class="sxs-lookup"><span data-stu-id="3f372-125">Enable and disable hello filter pane and page navigation pane - try hello [update settings demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)</span></span>
  * <span data-ttu-id="3f372-126">Nastavit výchozí hodnoty pro stránky a filtry – zkuste hello [ukázkové sady výchozí hodnoty](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)</span><span class="sxs-lookup"><span data-stu-id="3f372-126">Set defaults for pages and filters - try hello [set defaults demo](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)</span></span>
* <span data-ttu-id="3f372-127">Nastavení a ukončení režimu zobrazení na celé obrazovce</span><span class="sxs-lookup"><span data-stu-id="3f372-127">Enter and exit full screen mode</span></span>

[<span data-ttu-id="3f372-128">Další informace o vkládání sestav</span><span class="sxs-lookup"><span data-stu-id="3f372-128">Learn more about embedding a report</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)

### <a name="navigate-toopages-in-a-report"></a><span data-ttu-id="3f372-129">Přejděte toopages v sestavě</span><span class="sxs-lookup"><span data-stu-id="3f372-129">Navigate toopages in a report</span></span>
<span data-ttu-id="3f372-130">rozhraní API jazyka JavaScript enbales Hello je toodiscover všechny stránky na sestavu a tooset hello aktuální stránce.</span><span class="sxs-lookup"><span data-stu-id="3f372-130">hello JavaScript API enbales you toodiscover all pages in a report and tooset hello current page.</span></span> <span data-ttu-id="3f372-131">Zkuste hello [navigační ukázkové aplikace](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).</span><span class="sxs-lookup"><span data-stu-id="3f372-131">Try hello [navigation demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).</span></span>

[<span data-ttu-id="3f372-132">Další informace o navigaci stránkami</span><span class="sxs-lookup"><span data-stu-id="3f372-132">Learn more about page navigation</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a><span data-ttu-id="3f372-133">Filtrování sestavy</span><span class="sxs-lookup"><span data-stu-id="3f372-133">Filter a report</span></span>
<span data-ttu-id="3f372-134">Hello JavaScript API poskytuje základní a pokročilé schopnosti filtrování pro vložené sestavy a stránky sestavy.</span><span class="sxs-lookup"><span data-stu-id="3f372-134">hello JavaScript API provides basic and advanced filtering capabilities for embedded reports and report pages.</span></span> <span data-ttu-id="3f372-135">Zkuste hello [filtrování ukázkové aplikace](http://azure-samples.github.io/powerbi-angular-client/#/scenario4)a zkontrolujte sem některé úvodní kód.</span><span class="sxs-lookup"><span data-stu-id="3f372-135">Try hello [filtering demo application](http://azure-samples.github.io/powerbi-angular-client/#/scenario4), and review some introductory code here.</span></span>  

#### <a name="basic-filters"></a><span data-ttu-id="3f372-136">Základní filtry</span><span class="sxs-lookup"><span data-stu-id="3f372-136">Basic filters</span></span>
<span data-ttu-id="3f372-137">Základní filtr je umístěn na úrovni sloupce nebo hierarchie a obsahuje seznam hodnot tooinclude nebo vyloučení.</span><span class="sxs-lookup"><span data-stu-id="3f372-137">A basic filter is placed on a column or hierarchy level and contains a list of values tooinclude or exclude.</span></span>

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a><span data-ttu-id="3f372-138">Rozšířené filtry</span><span class="sxs-lookup"><span data-stu-id="3f372-138">Advanced filters</span></span>
<span data-ttu-id="3f372-139">Rozšířené filtry pomocí logického operátoru hello a nebo OR a přijměte podmínky jedno nebo dvě, každou s vlastní operátor a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3f372-139">Advanced filters use hello logical operator AND or OR, and accept one or two conditions, each with their own operator and value.</span></span> <span data-ttu-id="3f372-140">Podporované podmínky:</span><span class="sxs-lookup"><span data-stu-id="3f372-140">Supported conditions are:</span></span>

* <span data-ttu-id="3f372-141">Žádný</span><span class="sxs-lookup"><span data-stu-id="3f372-141">None</span></span>
* <span data-ttu-id="3f372-142">LessThan</span><span class="sxs-lookup"><span data-stu-id="3f372-142">LessThan</span></span>
* <span data-ttu-id="3f372-143">LessThanOrEqual</span><span class="sxs-lookup"><span data-stu-id="3f372-143">LessThanOrEqual</span></span>
* <span data-ttu-id="3f372-144">GreaterThan</span><span class="sxs-lookup"><span data-stu-id="3f372-144">GreaterThan</span></span>
* <span data-ttu-id="3f372-145">GreaterThanOrEqual</span><span class="sxs-lookup"><span data-stu-id="3f372-145">GreaterThanOrEqual</span></span>
* <span data-ttu-id="3f372-146">Contains</span><span class="sxs-lookup"><span data-stu-id="3f372-146">Contains</span></span>
* <span data-ttu-id="3f372-147">DoesNotContain</span><span class="sxs-lookup"><span data-stu-id="3f372-147">DoesNotContain</span></span>
* <span data-ttu-id="3f372-148">StartsWith</span><span class="sxs-lookup"><span data-stu-id="3f372-148">StartsWith</span></span>
* <span data-ttu-id="3f372-149">DoesNotStartWith</span><span class="sxs-lookup"><span data-stu-id="3f372-149">DoesNotStartWith</span></span>
* <span data-ttu-id="3f372-150">Is</span><span class="sxs-lookup"><span data-stu-id="3f372-150">Is</span></span>
* <span data-ttu-id="3f372-151">IsNot</span><span class="sxs-lookup"><span data-stu-id="3f372-151">IsNot</span></span>
* <span data-ttu-id="3f372-152">IsBlank</span><span class="sxs-lookup"><span data-stu-id="3f372-152">IsBlank</span></span>
* <span data-ttu-id="3f372-153">IsNotBlank</span><span class="sxs-lookup"><span data-stu-id="3f372-153">IsNotBlank</span></span>

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[<span data-ttu-id="3f372-154">Další informace o filtrování</span><span class="sxs-lookup"><span data-stu-id="3f372-154">Learn more about filtering</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)

### <a name="handling-events"></a><span data-ttu-id="3f372-155">Zpracování událostí</span><span class="sxs-lookup"><span data-stu-id="3f372-155">Handling events</span></span>
<span data-ttu-id="3f372-156">Kromě toho toosending informace do hello iframe, vaše aplikace můžete také získat informace o hello následující události pocházející ze hello iframe:</span><span class="sxs-lookup"><span data-stu-id="3f372-156">In addition toosending information into hello iframe, your application can also receive information on hello following events coming from hello iframe:</span></span>

* <span data-ttu-id="3f372-157">Embed</span><span class="sxs-lookup"><span data-stu-id="3f372-157">Embed</span></span>
  * <span data-ttu-id="3f372-158">loaded</span><span class="sxs-lookup"><span data-stu-id="3f372-158">loaded</span></span>
  * <span data-ttu-id="3f372-159">error</span><span class="sxs-lookup"><span data-stu-id="3f372-159">error</span></span>
* <span data-ttu-id="3f372-160">Reports</span><span class="sxs-lookup"><span data-stu-id="3f372-160">Reports</span></span>
  * <span data-ttu-id="3f372-161">pageChanged</span><span class="sxs-lookup"><span data-stu-id="3f372-161">pageChanged</span></span>
  * <span data-ttu-id="3f372-162">dataSelected (už brzy)</span><span class="sxs-lookup"><span data-stu-id="3f372-162">dataSelected (coming soon)</span></span>

[<span data-ttu-id="3f372-163">Další informace o zpracování událostí</span><span class="sxs-lookup"><span data-stu-id="3f372-163">Learn more about handling events</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)

## <a name="next-steps"></a><span data-ttu-id="3f372-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f372-164">Next steps</span></span>
<span data-ttu-id="3f372-165">Další informace o hello Power BI JavaScript rozhraní API projděte si hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="3f372-165">For more information about hello Power BI JavaScript API, check out hello following links:</span></span>

* [<span data-ttu-id="3f372-166">Wikiweb rozhraní API pro JavaScript</span><span class="sxs-lookup"><span data-stu-id="3f372-166">JavaScript API Wiki</span></span>](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
* [<span data-ttu-id="3f372-167">Referenční informace k objektovému modelu</span><span class="sxs-lookup"><span data-stu-id="3f372-167">Object model reference</span></span>](https://microsoft.github.io/powerbi-models/modules/_models_.html)
* <span data-ttu-id="3f372-168">Ukázky</span><span class="sxs-lookup"><span data-stu-id="3f372-168">Samples</span></span>
  * [<span data-ttu-id="3f372-169">Angular</span><span class="sxs-lookup"><span data-stu-id="3f372-169">Angular</span></span>](http://azure-samples.github.io/powerbi-angular-client)
  * [<span data-ttu-id="3f372-170">Ember</span><span class="sxs-lookup"><span data-stu-id="3f372-170">Ember</span></span>](https://github.com/Microsoft/powerbi-ember)
* [<span data-ttu-id="3f372-171">Živá ukázka</span><span class="sxs-lookup"><span data-stu-id="3f372-171">Live demo</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)

