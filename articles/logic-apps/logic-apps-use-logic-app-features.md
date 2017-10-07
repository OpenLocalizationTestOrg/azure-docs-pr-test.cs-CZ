---
title: "aaaAdd podmínky a spustit pracovní postupy - Azure Logic Apps | Microsoft Docs"
description: "Řídí, jak spouštět pracovní postupy v Azure Logic Apps přidáním podmíněnou logiku, aktivační události, akce a parametrů."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: 76d5e44590ffa14cf70d7a93b99a241d286d555b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-logic-apps-features"></a><span data-ttu-id="11f72-103">Pomocí funkcí Logic Apps</span><span class="sxs-lookup"><span data-stu-id="11f72-103">Use Logic Apps features</span></span>

<span data-ttu-id="11f72-104">V [předchozí téma](../logic-apps/logic-apps-create-a-logic-app.md), jste vytvořili svou první aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="11f72-104">In a [previous topic](../logic-apps/logic-apps-create-a-logic-app.md), you created your first logic app.</span></span> <span data-ttu-id="11f72-105">toocontrol svou aplikaci logiky pracovního postupu, můžete zadat různé cesty pro vaše toorun logiku aplikace a jak příliš zpracování dat v polích, kolekce a dávek.</span><span class="sxs-lookup"><span data-stu-id="11f72-105">toocontrol your logic app's workflow, you can specify different paths for your logic app toorun and how too process data in arrays, collections, and batches.</span></span> <span data-ttu-id="11f72-106">V pracovním postupu aplikace logiky můžete zahrnout tyto prvky:</span><span class="sxs-lookup"><span data-stu-id="11f72-106">You can include these elements in your logic app workflow:</span></span>

* <span data-ttu-id="11f72-107">Podmínky a [přepínač příkazy](../logic-apps/logic-apps-switch-case.md) umožní aplikaci logiky spustit různé akce, které jsou založené na tom, zda jsou splněny určité podmínky.</span><span class="sxs-lookup"><span data-stu-id="11f72-107">Conditions and [switch statements](../logic-apps/logic-apps-switch-case.md) let your logic app run different actions based on whether specific conditions are met.</span></span>

* <span data-ttu-id="11f72-108">[Smyčky](../logic-apps/logic-apps-loops-and-scopes.md) umožní aplikaci logiky opakovaně spustit kroky.</span><span class="sxs-lookup"><span data-stu-id="11f72-108">[Loops](../logic-apps/logic-apps-loops-and-scopes.md) let your logic app run steps repeatedly.</span></span> <span data-ttu-id="11f72-109">Například můžete opakovat akce přes pole při použití **for_each –** smyčky.</span><span class="sxs-lookup"><span data-stu-id="11f72-109">For example, you can repeat actions over an array when you use a **For_each** loop.</span></span> <span data-ttu-id="11f72-110">Nebo akce můžete opakovat, dokud je splněna podmínka, pokud použijete **dokud** smyčky.</span><span class="sxs-lookup"><span data-stu-id="11f72-110">Or you can repeat actions until a condition is met when you use an **Until** loop.</span></span>

* <span data-ttu-id="11f72-111">[Obory](../logic-apps/logic-apps-loops-and-scopes.md) umožňují můžete seskupit sérii akcí, například tooimplement výjimek.</span><span class="sxs-lookup"><span data-stu-id="11f72-111">[Scopes](../logic-apps/logic-apps-loops-and-scopes.md) let you group series of actions together, for example, tooimplement exception handling.</span></span>

* <span data-ttu-id="11f72-112">[Debatching](../logic-apps/logic-apps-loops-and-scopes.md) umožní aplikaci logiky spuštění samostatné pracovní postupy pro položky v poli při použití hello **SplitOn** příkaz.</span><span class="sxs-lookup"><span data-stu-id="11f72-112">[Debatching](../logic-apps/logic-apps-loops-and-scopes.md) lets your logic app start separate workflows for items in an array when you use hello **SplitOn** command.</span></span>

<span data-ttu-id="11f72-113">Toto téma představuje další koncepty pro vytvoření aplikace logiky:</span><span class="sxs-lookup"><span data-stu-id="11f72-113">This topic introduces other concepts for building your logic app:</span></span>

* <span data-ttu-id="11f72-114">Kód zobrazení tooedit existující aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="11f72-114">Code view tooedit an existing logic app</span></span>
* <span data-ttu-id="11f72-115">Možnosti pro spuštění pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="11f72-115">Options for starting a workflow</span></span>

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a><span data-ttu-id="11f72-116">Podmínky: Spustit kroky až po splnění podmínku</span><span class="sxs-lookup"><span data-stu-id="11f72-116">Conditions: Run steps only after meeting a condition</span></span>

<span data-ttu-id="11f72-117">toohave aplikace logiky spustit kroky jenom v případě, že data splňuje určitá kritéria, můžete přidat podmínku, která porovná data v pracovním postupu hello proti konkrétní pole nebo hodnoty.</span><span class="sxs-lookup"><span data-stu-id="11f72-117">toohave your logic app run steps only when data meets specific criteria, you can add a condition that compares data in hello workflow against specific fields or values.</span></span>

<span data-ttu-id="11f72-118">Předpokládejme například, že máte aplikaci logiky, který odesílá příliš mnoho e-mailům v příspěvcích na informačního kanálu RSS na web.</span><span class="sxs-lookup"><span data-stu-id="11f72-118">For example, suppose you have a logic app that sends you too many emails for posts on a website's RSS feed.</span></span> <span data-ttu-id="11f72-119">Můžete přidat, že podmínka, aby aplikace logiky odešle e-mailu jenom v případě, že hello nového příspěvku patří tooa určité kategorie.</span><span class="sxs-lookup"><span data-stu-id="11f72-119">You can add a condition so that your logic app sends email only when hello new post belongs tooa specific category.</span></span>

1. <span data-ttu-id="11f72-120">V hello [portál Azure](https://portal.azure.com), najít a otevřít aplikaci logiky v návrháři aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="11f72-120">In hello [Azure portal](https://portal.azure.com), find and open your logic app in Logic App Designer.</span></span>

2. <span data-ttu-id="11f72-121">Umístění pracovního postupu toohello podmínky, které chcete přidáte.</span><span class="sxs-lookup"><span data-stu-id="11f72-121">Add a condition toohello workflow location that you want.</span></span> 

   <span data-ttu-id="11f72-122">Podmínka hello tooadd mezi existující kroky v pracovním postupu logiku aplikace hello, přesuňte ukazatel hello hello šipku místo tooadd hello podmínku.</span><span class="sxs-lookup"><span data-stu-id="11f72-122">tooadd hello condition between existing steps in hello logic app workflow, move hello pointer over hello arrow where you want tooadd hello condition.</span></span> 
   <span data-ttu-id="11f72-123">Zvolte hello **znaménko plus** (**+**), pak zvolte **přidat podmínku**.</span><span class="sxs-lookup"><span data-stu-id="11f72-123">Choose hello **plus sign** (**+**), then choose **Add a condition**.</span></span> <span data-ttu-id="11f72-124">Například:</span><span class="sxs-lookup"><span data-stu-id="11f72-124">For example:</span></span>

   ![Přidat podmínku toologic aplikaci](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > <span data-ttu-id="11f72-126">Pokud chcete tooadd podmínku na konci hello aktuálního pracovního postupu, přejděte toohello dolní části svou aplikaci logiky a znovu vyberte **+ nový krok**.</span><span class="sxs-lookup"><span data-stu-id="11f72-126">If you want tooadd a condition at hello end of your current workflow, go toohello bottom of your logic app, and choose **+ New step**.</span></span>

3. <span data-ttu-id="11f72-127">Nyní Definujte podmínku hello.</span><span class="sxs-lookup"><span data-stu-id="11f72-127">Now define hello condition.</span></span> <span data-ttu-id="11f72-128">Zadejte pole zdroje hello chcete tooevaluate, tooperform hello operace a hodnota cíle hello nebo pole.</span><span class="sxs-lookup"><span data-stu-id="11f72-128">Specify hello source field that you want tooevaluate, hello operation tooperform, and hello target value or field.</span></span> <span data-ttu-id="11f72-129">existující tooadd polí tooyour podmínky, vyberte z hello **dynamického obsahu seznamu přidat**.</span><span class="sxs-lookup"><span data-stu-id="11f72-129">tooadd existing fields tooyour condition, choose from hello **Add dynamic content list**.</span></span>

   <span data-ttu-id="11f72-130">Například:</span><span class="sxs-lookup"><span data-stu-id="11f72-130">For example:</span></span>

   ![Upravit podmínky v základní režimu](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   <span data-ttu-id="11f72-132">Tady je hello dokončení podmínku:</span><span class="sxs-lookup"><span data-stu-id="11f72-132">Here's hello complete condition:</span></span>

   ![Dokončení podmínky](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > <span data-ttu-id="11f72-134">Vyberte podmínku hello toodefine v kódu **upravit v rozšířeném režimu**.</span><span class="sxs-lookup"><span data-stu-id="11f72-134">toodefine hello condition in code, choose **Edit in advanced mode**.</span></span> <span data-ttu-id="11f72-135">Například:</span><span class="sxs-lookup"><span data-stu-id="11f72-135">For example:</span></span>
   > 
   > ![Upravit podmínky v kódu](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. <span data-ttu-id="11f72-137">V části **Pokud Ano** a **ne v případě**, přidejte tooperform kroky hello založené na tom, zda je splněna podmínka hello.</span><span class="sxs-lookup"><span data-stu-id="11f72-137">Under **IF YES** and **IF NO**, add hello steps tooperform based on whether hello condition is met.</span></span>

   <span data-ttu-id="11f72-138">Například:</span><span class="sxs-lookup"><span data-stu-id="11f72-138">For example:</span></span>

   ![Podmínky se Ano a žádné cesty](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > <span data-ttu-id="11f72-140">Přetáhněte stávající akce do hello **Pokud Ano** a **ne v případě** cesty.</span><span class="sxs-lookup"><span data-stu-id="11f72-140">You can drag existing actions into hello **IF YES** and **IF NO** paths.</span></span>

5. <span data-ttu-id="11f72-141">Když jste hotovi, uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="11f72-141">When you're done, save your logic app.</span></span>

<span data-ttu-id="11f72-142">Pouze v případě, že hello příspěvcích splňují vaše podmínky se teď se e-mailů.</span><span class="sxs-lookup"><span data-stu-id="11f72-142">Now you get emails only when hello posts meet your condition.</span></span>

## <a name="repeat-actions-over-a-list-with-foreach"></a><span data-ttu-id="11f72-143">Opakování akce pro seznam pomocí příkazu forEach</span><span class="sxs-lookup"><span data-stu-id="11f72-143">Repeat actions over a list with forEach</span></span>

<span data-ttu-id="11f72-144">smyčka typu forEach Hello určuje přes pole toorepeat akce.</span><span class="sxs-lookup"><span data-stu-id="11f72-144">hello forEach loop specifies an array toorepeat an action over.</span></span> <span data-ttu-id="11f72-145">Pokud není pole, tok hello se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="11f72-145">If it is not an array, hello flow fails.</span></span> <span data-ttu-id="11f72-146">Například pokud máte action1, který jako výstup pole zprávy a chcete toosend každou zprávu, můžete zahrnout tento příkaz forEach ve vlastnostech hello vaše akce:`forEach : "@action('action1').outputs.messages"`</span><span class="sxs-lookup"><span data-stu-id="11f72-146">For example, if you have action1 that outputs an array of messages, and you want toosend each message, you can include this forEach statement in hello properties of your action: `forEach : "@action('action1').outputs.messages"`</span></span>

## <a name="edit-hello-code-definition-for-a-logic-app"></a><span data-ttu-id="11f72-147">Upravte definici hello kódu pro aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="11f72-147">Edit hello code definition for a logic app</span></span>

<span data-ttu-id="11f72-148">I když máte hello návrhář aplikace logiky, můžete upravit přímo hello kód, který definuje logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="11f72-148">Although you have hello Logic App Designer, you can directly edit hello code that defines a logic app.</span></span>

1. <span data-ttu-id="11f72-149">Na panelu příkazů hello, zvolte **Code zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="11f72-149">On hello command bar, choose **Code view**.</span></span>

    <span data-ttu-id="11f72-150">Úplné editor otevře a zobrazí hello definice, které jste upravili.</span><span class="sxs-lookup"><span data-stu-id="11f72-150">A full editor opens and shows hello definition you edited.</span></span>

    ![Zobrazení kódu](media/logic-apps-use-logic-app-features/codeview.png)

    <span data-ttu-id="11f72-152">V hello textový editor, můžete zkopírovat a vložit libovolného počtu akcí v rámci hello stejné aplikace logiky nebo mezi aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="11f72-152">In hello text editor, you can copy and paste any number of actions within hello same logic app or between logic apps.</span></span> 
    <span data-ttu-id="11f72-153">Můžete také snadno přidávat nebo odebírat celé oddíly z definice hello a definice můžete také sdílet s ostatními.</span><span class="sxs-lookup"><span data-stu-id="11f72-153">You can also easily add or remove entire sections from hello definition, and you can also share definitions with others.</span></span>

2. <span data-ttu-id="11f72-154">Zvolte provedené úpravy toosave **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="11f72-154">toosave your edits, choose **Save**.</span></span>

## <a name="parameters"></a><span data-ttu-id="11f72-155">Parametry</span><span class="sxs-lookup"><span data-stu-id="11f72-155">Parameters</span></span>

<span data-ttu-id="11f72-156">Některé funkce Logic Apps jsou k dispozici pouze v zobrazení kódu, například parametry.</span><span class="sxs-lookup"><span data-stu-id="11f72-156">Some Logic Apps capabilities are available only in code view, for example, parameters.</span></span> <span data-ttu-id="11f72-157">Parametry umožňují snadno tooreuse hodnoty v celé aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="11f72-157">Parameters make it easy tooreuse values throughout your logic app.</span></span> <span data-ttu-id="11f72-158">Například pokud máte e-mailovou adresu, kterou chcete použít v několik akcí, měli byste tuto e-mailovou adresu jako parametr.</span><span class="sxs-lookup"><span data-stu-id="11f72-158">For example, if you have an email address that you want use in several actions, you should define that email address as a parameter.</span></span>

<span data-ttu-id="11f72-159">Parametry jsou vhodné pro stahování na hodnoty jsou pravděpodobně toochange mnoho.</span><span class="sxs-lookup"><span data-stu-id="11f72-159">Parameters are good for pulling out values that you are likely toochange a lot.</span></span> <span data-ttu-id="11f72-160">Jsou užitečné zejména v případě, že potřebujete toooverride parametry v různých prostředích.</span><span class="sxs-lookup"><span data-stu-id="11f72-160">They are especially useful when you need toooverride parameters in different environments.</span></span> <span data-ttu-id="11f72-161">jak zjistit, na základě prostředí, parametry toooverride toolearn [vytváření definic aplikace logiky](../logic-apps/logic-apps-author-definitions.md) a [dokumentace k REST API](https://docs.microsoft.com/rest/api/logic).</span><span class="sxs-lookup"><span data-stu-id="11f72-161">toolearn how toooverride parameters based on environment, see [Author logic app definitions](../logic-apps/logic-apps-author-definitions.md) and [REST API documentation](https://docs.microsoft.com/rest/api/logic).</span></span>

<span data-ttu-id="11f72-162">Tento příklad ukazuje, jak tooupdate existující aplikace logiky tak, aby parametry lze použít pro termín dotazu, hello.</span><span class="sxs-lookup"><span data-stu-id="11f72-162">This example shows how tooupdate your existing logic app so that you can use parameters for hello query term.</span></span>

1. <span data-ttu-id="11f72-163">V zobrazení kódu, najde hello `parameters : {}` objektu a přidejte `currentFeedUrl` objektu:</span><span class="sxs-lookup"><span data-stu-id="11f72-163">In code view, find hello `parameters : {}` object, and add a `currentFeedUrl` object:</span></span>

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. <span data-ttu-id="11f72-164">Přejděte toohello `When_a_feed-item_is_published` akce, najít hello `queries` části a nahradit hodnotu dotazu hello:`"feedUrl": "#@{parameters('currentFeedUrl')}"`</span><span class="sxs-lookup"><span data-stu-id="11f72-164">Go toohello `When_a_feed-item_is_published` action, find hello `queries` section, and replace hello query value with : `"feedUrl": "#@{parameters('currentFeedUrl')}"`</span></span> 

    <span data-ttu-id="11f72-165">toojoin dva nebo víc řetězců, můžete použít také hello `concat` funkce.</span><span class="sxs-lookup"><span data-stu-id="11f72-165">toojoin two or more strings, you can also use hello `concat` function.</span></span> 
    <span data-ttu-id="11f72-166">Například `"@concat('#',parameters('currentFeedUrl'))"` funguje hello stejné jako hello výše.</span><span class="sxs-lookup"><span data-stu-id="11f72-166">For example, `"@concat('#',parameters('currentFeedUrl'))"` works hello same as hello above.</span></span>

3.  <span data-ttu-id="11f72-167">Až budete hotoví, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="11f72-167">When you're done, choose **Save**.</span></span> 

    <span data-ttu-id="11f72-168">Teď můžete změnit hello webu kanálu RSS předáním jinou adresu URL prostřednictvím hello `currentFeedURL` objektu.</span><span class="sxs-lookup"><span data-stu-id="11f72-168">Now you can change hello website's RSS feed by passing a different URL through hello `currentFeedURL` object.</span></span>

<span data-ttu-id="11f72-169">Další informace o [jak definice aplikace logiky tooauthor](../logic-apps/logic-apps-author-definitions.md).</span><span class="sxs-lookup"><span data-stu-id="11f72-169">Learn more about [how tooauthor logic app definitions](../logic-apps/logic-apps-author-definitions.md).</span></span>

## <a name="start-logic-app-workflows"></a><span data-ttu-id="11f72-170">Spusťte aplikaci logiky pracovních postupů</span><span class="sxs-lookup"><span data-stu-id="11f72-170">Start logic app workflows</span></span>

<span data-ttu-id="11f72-171">Existuje několik možností pro spuštění pracovního postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="11f72-171">You have different options for starting hello workflow defined in your logic app.</span></span> <span data-ttu-id="11f72-172">Pracovního postupu na vyžádání můžete vždy spustit v hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="11f72-172">You can always start a workflow on-demand in hello [Azure portal].</span></span>

### <a name="recurrence-triggers"></a><span data-ttu-id="11f72-173">Aktivuje opakování</span><span class="sxs-lookup"><span data-stu-id="11f72-173">Recurrence triggers</span></span>

<span data-ttu-id="11f72-174">Opakování aktivační událost se spustí na časový interval, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="11f72-174">A recurrence trigger runs at an interval that you specify.</span></span> <span data-ttu-id="11f72-175">Pokud je aktivační událost hello podmíněnou logiku, aktivační události hello Určuje, zda hello pracovního postupu musí toorun.</span><span class="sxs-lookup"><span data-stu-id="11f72-175">When hello trigger has conditional logic, hello trigger determines whether hello workflow needs toorun.</span></span> <span data-ttu-id="11f72-176">Aktivační událost označuje hello pracovní postup spuštěn vrácením `200` stavový kód.</span><span class="sxs-lookup"><span data-stu-id="11f72-176">A trigger indicates hello workflow should run by returning a `200` status code.</span></span> <span data-ttu-id="11f72-177">Když pracovní postup hello nepotřebuje toorun, vrátí hello aktivační události `202` stavový kód.</span><span class="sxs-lookup"><span data-stu-id="11f72-177">When hello workflow doesn't need toorun, hello trigger returns a `202` status code.</span></span>

### <a name="callback-using-rest-apis"></a><span data-ttu-id="11f72-178">Zpětné volání pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="11f72-178">Callback using REST APIs</span></span>

<span data-ttu-id="11f72-179">toostart pracovní postup služby můžete volat koncový bod logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="11f72-179">toostart a workflow, services can call a logic app endpoint.</span></span> <span data-ttu-id="11f72-180">Vyberte tento druh logiku aplikace na vyžádání, toostart **spustit nyní** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="11f72-180">toostart this kind of logic app on-demand, choose **Run now** on hello command bar.</span></span> <span data-ttu-id="11f72-181">V tématu [spustit pracovní postupy voláním logiku aplikace koncové body jako triggery](../logic-apps/logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="11f72-181">See [Start workflows by calling logic app endpoints as triggers](../logic-apps/logic-apps-http-endpoint.md).</span></span> 

<!-- Shared links -->
[portál Azure]: https://portal.azure.com

## <a name="next-steps"></a><span data-ttu-id="11f72-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11f72-183">Next steps</span></span>

* [<span data-ttu-id="11f72-184">Příkazech Switch</span><span class="sxs-lookup"><span data-stu-id="11f72-184">Switch statements</span></span>](../logic-apps/logic-apps-switch-case.md) 
* [<span data-ttu-id="11f72-185">Smyčky, obory a rozdělení dávek</span><span class="sxs-lookup"><span data-stu-id="11f72-185">Loops, scopes, and debatching</span></span>](../logic-apps/logic-apps-loops-and-scopes.md)
* [<span data-ttu-id="11f72-186">Vytváření definic aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="11f72-186">Author logic app definitions</span></span>](../logic-apps/logic-apps-author-definitions.md)