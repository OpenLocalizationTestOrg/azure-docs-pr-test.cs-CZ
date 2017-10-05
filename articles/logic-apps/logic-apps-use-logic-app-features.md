---
title: "Přidání podmínek a spustit pracovní postupy - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: e632c48ed31e82536db55a9c54438bece0c38fd4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-logic-apps-features"></a><span data-ttu-id="b0f3c-103">Pomocí funkcí Logic Apps</span><span class="sxs-lookup"><span data-stu-id="b0f3c-103">Use Logic Apps features</span></span>

<span data-ttu-id="b0f3c-104">V [předchozí téma](../logic-apps/logic-apps-create-a-logic-app.md), jste vytvořili svou první aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-104">In a [previous topic](../logic-apps/logic-apps-create-a-logic-app.md), you created your first logic app.</span></span> <span data-ttu-id="b0f3c-105">Chcete-li řídit svou aplikaci logiky pracovního postupu, můžete zadat různé cesty pro svou aplikaci logiky ke spuštění a postupy zpracování dat v polích, kolekce a dávek.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-105">To control your logic app's workflow, you can specify different paths for your logic app to run and how to process data in arrays, collections, and batches.</span></span> <span data-ttu-id="b0f3c-106">V pracovním postupu aplikace logiky můžete zahrnout tyto prvky:</span><span class="sxs-lookup"><span data-stu-id="b0f3c-106">You can include these elements in your logic app workflow:</span></span>

* <span data-ttu-id="b0f3c-107">Podmínky a [přepínač příkazy](../logic-apps/logic-apps-switch-case.md) umožní aplikaci logiky spustit různé akce, které jsou založené na tom, zda jsou splněny určité podmínky.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-107">Conditions and [switch statements](../logic-apps/logic-apps-switch-case.md) let your logic app run different actions based on whether specific conditions are met.</span></span>

* <span data-ttu-id="b0f3c-108">[Smyčky](../logic-apps/logic-apps-loops-and-scopes.md) umožní aplikaci logiky opakovaně spustit kroky.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-108">[Loops](../logic-apps/logic-apps-loops-and-scopes.md) let your logic app run steps repeatedly.</span></span> <span data-ttu-id="b0f3c-109">Například můžete opakovat akce přes pole při použití **for_each –** smyčky.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-109">For example, you can repeat actions over an array when you use a **For_each** loop.</span></span> <span data-ttu-id="b0f3c-110">Nebo akce můžete opakovat, dokud je splněna podmínka, pokud použijete **dokud** smyčky.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-110">Or you can repeat actions until a condition is met when you use an **Until** loop.</span></span>

* <span data-ttu-id="b0f3c-111">[Obory](../logic-apps/logic-apps-loops-and-scopes.md) umožňují můžete seskupit sérii akcí, například k implementaci výjimek.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-111">[Scopes](../logic-apps/logic-apps-loops-and-scopes.md) let you group series of actions together, for example, to implement exception handling.</span></span>

* <span data-ttu-id="b0f3c-112">[Debatching](../logic-apps/logic-apps-loops-and-scopes.md) umožní aplikaci logiky spuštění samostatné pracovní postupy pro položky v poli, při použití **SplitOn** příkaz.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-112">[Debatching](../logic-apps/logic-apps-loops-and-scopes.md) lets your logic app start separate workflows for items in an array when you use the **SplitOn** command.</span></span>

<span data-ttu-id="b0f3c-113">Toto téma představuje další koncepty pro vytvoření aplikace logiky:</span><span class="sxs-lookup"><span data-stu-id="b0f3c-113">This topic introduces other concepts for building your logic app:</span></span>

* <span data-ttu-id="b0f3c-114">Zobrazení kódu, chcete-li upravit existující aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="b0f3c-114">Code view to edit an existing logic app</span></span>
* <span data-ttu-id="b0f3c-115">Možnosti pro spuštění pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="b0f3c-115">Options for starting a workflow</span></span>

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a><span data-ttu-id="b0f3c-116">Podmínky: Spustit kroky až po splnění podmínku</span><span class="sxs-lookup"><span data-stu-id="b0f3c-116">Conditions: Run steps only after meeting a condition</span></span>

<span data-ttu-id="b0f3c-117">Pokud chcete, aby aplikace logiky spustit kroky jenom v případě, že data splňuje určitá kritéria, můžete přidat podmínku, která porovná data v pracovním postupu proti konkrétní pole nebo hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-117">To have your logic app run steps only when data meets specific criteria, you can add a condition that compares data in the workflow against specific fields or values.</span></span>

<span data-ttu-id="b0f3c-118">Předpokládejme například, že máte aplikaci logiky, který odesílá příliš mnoho e-mailům v příspěvcích na informačního kanálu RSS na web.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-118">For example, suppose you have a logic app that sends you too many emails for posts on a website's RSS feed.</span></span> <span data-ttu-id="b0f3c-119">Podmínku můžete přidat tak, aby aplikace logiky odešle e-mailu jenom v případě, že nového příspěvku patří do určité kategorie.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-119">You can add a condition so that your logic app sends email only when the new post belongs to a specific category.</span></span>

1. <span data-ttu-id="b0f3c-120">V [portál Azure](https://portal.azure.com), najít a otevřít aplikaci logiky v návrháři aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-120">In the [Azure portal](https://portal.azure.com), find and open your logic app in Logic App Designer.</span></span>

2. <span data-ttu-id="b0f3c-121">Do umístění pracovního postupu, který chcete přidáte podmínku.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-121">Add a condition to the workflow location that you want.</span></span> 

   <span data-ttu-id="b0f3c-122">Chcete-li přidat podmínku mezi existující kroky v pracovním postupu aplikace logiky, přesuňte ukazatel přes na šipku, ve které chcete přidat podmínku.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-122">To add the condition between existing steps in the logic app workflow, move the pointer over the arrow where you want to add the condition.</span></span> 
   <span data-ttu-id="b0f3c-123">Vyberte **znaménko plus** (**+**), pak zvolte **přidat podmínku**.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-123">Choose the **plus sign** (**+**), then choose **Add a condition**.</span></span> <span data-ttu-id="b0f3c-124">Například:</span><span class="sxs-lookup"><span data-stu-id="b0f3c-124">For example:</span></span>

   ![Přidejte podmínku do aplikace logiky](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > <span data-ttu-id="b0f3c-126">Pokud chcete přidat podmínku na konci aktuálního pracovního postupu, přejděte k dolnímu okraji svou aplikaci logiky a zvolte **+ nový krok**.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-126">If you want to add a condition at the end of your current workflow, go to the bottom of your logic app, and choose **+ New step**.</span></span>

3. <span data-ttu-id="b0f3c-127">Nyní můžete Definujte podmínku.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-127">Now define the condition.</span></span> <span data-ttu-id="b0f3c-128">Zadejte zdrojové pole, které chcete vyhodnotit, operaci provést a cílová hodnota nebo pole.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-128">Specify the source field that you want to evaluate, the operation to perform, and the target value or field.</span></span> <span data-ttu-id="b0f3c-129">Chcete-li přidat existující pole na vaše podmínky, zvolte z **dynamického obsahu seznamu přidat**.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-129">To add existing fields to your condition, choose from the **Add dynamic content list**.</span></span>

   <span data-ttu-id="b0f3c-130">Například:</span><span class="sxs-lookup"><span data-stu-id="b0f3c-130">For example:</span></span>

   ![Upravit podmínky v základní režimu](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   <span data-ttu-id="b0f3c-132">Tady je Dokončená podmínka:</span><span class="sxs-lookup"><span data-stu-id="b0f3c-132">Here's the complete condition:</span></span>

   ![Dokončení podmínky](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > <span data-ttu-id="b0f3c-134">Chcete-li definovat podmínku v kódu, zvolte **upravit v rozšířeném režimu**.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-134">To define the condition in code, choose **Edit in advanced mode**.</span></span> <span data-ttu-id="b0f3c-135">Například:</span><span class="sxs-lookup"><span data-stu-id="b0f3c-135">For example:</span></span>
   > 
   > ![Upravit podmínky v kódu](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. <span data-ttu-id="b0f3c-137">V části **Pokud Ano** a **ne v případě**, přidávat postupy založené na tom, zda je splněna podmínka.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-137">Under **IF YES** and **IF NO**, add the steps to perform based on whether the condition is met.</span></span>

   <span data-ttu-id="b0f3c-138">Například:</span><span class="sxs-lookup"><span data-stu-id="b0f3c-138">For example:</span></span>

   ![Podmínky se Ano a žádné cesty](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > <span data-ttu-id="b0f3c-140">Můžete přetáhnout existující akce do **Pokud Ano** a **ne v případě** cesty.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-140">You can drag existing actions into the **IF YES** and **IF NO** paths.</span></span>

5. <span data-ttu-id="b0f3c-141">Když jste hotovi, uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-141">When you're done, save your logic app.</span></span>

<span data-ttu-id="b0f3c-142">Pouze v případě, že v příspěvcích splňují vaše podmínky se teď se e-mailů.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-142">Now you get emails only when the posts meet your condition.</span></span>

## <a name="repeat-actions-over-a-list-with-foreach"></a><span data-ttu-id="b0f3c-143">Opakování akce pro seznam pomocí příkazu forEach</span><span class="sxs-lookup"><span data-stu-id="b0f3c-143">Repeat actions over a list with forEach</span></span>

<span data-ttu-id="b0f3c-144">Smyčka typu forEach určuje pole přes opakování akce.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-144">The forEach loop specifies an array to repeat an action over.</span></span> <span data-ttu-id="b0f3c-145">Pokud není pole, tok se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-145">If it is not an array, the flow fails.</span></span> <span data-ttu-id="b0f3c-146">Například pokud máte action1, který jako výstup pole zprávy a chcete odeslat každou zprávu, můžete zahrnout tento příkaz forEach ve vlastnostech akci:`forEach : "@action('action1').outputs.messages"`</span><span class="sxs-lookup"><span data-stu-id="b0f3c-146">For example, if you have action1 that outputs an array of messages, and you want to send each message, you can include this forEach statement in the properties of your action: `forEach : "@action('action1').outputs.messages"`</span></span>

## <a name="edit-the-code-definition-for-a-logic-app"></a><span data-ttu-id="b0f3c-147">Upravit kód definici aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="b0f3c-147">Edit the code definition for a logic app</span></span>

<span data-ttu-id="b0f3c-148">I když máte návrháře aplikace logiky, lze přímo upravit kód, který definuje logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-148">Although you have the Logic App Designer, you can directly edit the code that defines a logic app.</span></span>

1. <span data-ttu-id="b0f3c-149">Na panelu příkazů vyberte **Code zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-149">On the command bar, choose **Code view**.</span></span>

    <span data-ttu-id="b0f3c-150">Úplné editor otevře a zobrazuje definici, kterou jste upravili.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-150">A full editor opens and shows the definition you edited.</span></span>

    ![Zobrazení kódu](media/logic-apps-use-logic-app-features/codeview.png)

    <span data-ttu-id="b0f3c-152">V textovém editoru můžete zkopírovat a vložit libovolného počtu akcí v rámci stejné aplikaci logiky nebo mezi aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-152">In the text editor, you can copy and paste any number of actions within the same logic app or between logic apps.</span></span> 
    <span data-ttu-id="b0f3c-153">Můžete také snadno přidávat nebo odebírat celé oddíly z definice a definice můžete také sdílet s ostatními.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-153">You can also easily add or remove entire sections from the definition, and you can also share definitions with others.</span></span>

2. <span data-ttu-id="b0f3c-154">Pokud chcete uložit provedené úpravy, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-154">To save your edits, choose **Save**.</span></span>

## <a name="parameters"></a><span data-ttu-id="b0f3c-155">Parametry</span><span class="sxs-lookup"><span data-stu-id="b0f3c-155">Parameters</span></span>

<span data-ttu-id="b0f3c-156">Některé funkce Logic Apps jsou k dispozici pouze v zobrazení kódu, například parametry.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-156">Some Logic Apps capabilities are available only in code view, for example, parameters.</span></span> <span data-ttu-id="b0f3c-157">Parametry usnadňují znovu použít hodnoty v celé aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-157">Parameters make it easy to reuse values throughout your logic app.</span></span> <span data-ttu-id="b0f3c-158">Například pokud máte e-mailovou adresu, kterou chcete použít v několik akcí, měli byste tuto e-mailovou adresu jako parametr.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-158">For example, if you have an email address that you want use in several actions, you should define that email address as a parameter.</span></span>

<span data-ttu-id="b0f3c-159">Parametry jsou vhodné pro stahování na hodnoty, které budete chtít nejspíš se měnit hodně.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-159">Parameters are good for pulling out values that you are likely to change a lot.</span></span> <span data-ttu-id="b0f3c-160">Jsou užitečné zejména v případě, že je nutné přepsat parametry v různých prostředích.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-160">They are especially useful when you need to override parameters in different environments.</span></span> <span data-ttu-id="b0f3c-161">Další postupy k přepsání parametry podle prostředí najdete v tématu [vytváření definic aplikace logiky](../logic-apps/logic-apps-author-definitions.md) a [dokumentace k REST API](https://docs.microsoft.com/rest/api/logic).</span><span class="sxs-lookup"><span data-stu-id="b0f3c-161">To learn how to override parameters based on environment, see [Author logic app definitions](../logic-apps/logic-apps-author-definitions.md) and [REST API documentation](https://docs.microsoft.com/rest/api/logic).</span></span>

<span data-ttu-id="b0f3c-162">Tento příklad ukazuje, jak se bude aktualizovat stávající aplikaci logiky tak, aby parametry lze použít pro termín dotazu.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-162">This example shows how to update your existing logic app so that you can use parameters for the query term.</span></span>

1. <span data-ttu-id="b0f3c-163">V zobrazení kódu najdete `parameters : {}` objektu a přidejte `currentFeedUrl` objektu:</span><span class="sxs-lookup"><span data-stu-id="b0f3c-163">In code view, find the `parameters : {}` object, and add a `currentFeedUrl` object:</span></span>

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. <span data-ttu-id="b0f3c-164">Přejděte na `When_a_feed-item_is_published` akce, Najít `queries` části a nahradit hodnotu dotazu:`"feedUrl": "#@{parameters('currentFeedUrl')}"`</span><span class="sxs-lookup"><span data-stu-id="b0f3c-164">Go to the `When_a_feed-item_is_published` action, find the `queries` section, and replace the query value with : `"feedUrl": "#@{parameters('currentFeedUrl')}"`</span></span> 

    <span data-ttu-id="b0f3c-165">Pro připojení dvě nebo více řetězce, můžete také použít `concat` funkce.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-165">To join two or more strings, you can also use the `concat` function.</span></span> 
    <span data-ttu-id="b0f3c-166">Například `"@concat('#',parameters('currentFeedUrl'))"` funguje stejně jako výše.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-166">For example, `"@concat('#',parameters('currentFeedUrl'))"` works the same as the above.</span></span>

3.  <span data-ttu-id="b0f3c-167">Až budete hotoví, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-167">When you're done, choose **Save**.</span></span> 

    <span data-ttu-id="b0f3c-168">Teď můžete změnit předáním jinou adresu URL prostřednictvím informačního kanálu RSS webu `currentFeedURL` objektu.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-168">Now you can change the website's RSS feed by passing a different URL through the `currentFeedURL` object.</span></span>

<span data-ttu-id="b0f3c-169">Další informace o [postupy k vytváření definic aplikace logiky](../logic-apps/logic-apps-author-definitions.md).</span><span class="sxs-lookup"><span data-stu-id="b0f3c-169">Learn more about [how to author logic app definitions](../logic-apps/logic-apps-author-definitions.md).</span></span>

## <a name="start-logic-app-workflows"></a><span data-ttu-id="b0f3c-170">Spusťte aplikaci logiky pracovních postupů</span><span class="sxs-lookup"><span data-stu-id="b0f3c-170">Start logic app workflows</span></span>

<span data-ttu-id="b0f3c-171">Existuje několik možností pro spuštění pracovního postupu definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-171">You have different options for starting the workflow defined in your logic app.</span></span> <span data-ttu-id="b0f3c-172">Pracovního postupu na vyžádání můžete vždy spustit v [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="b0f3c-172">You can always start a workflow on-demand in the [Azure portal].</span></span>

### <a name="recurrence-triggers"></a><span data-ttu-id="b0f3c-173">Aktivuje opakování</span><span class="sxs-lookup"><span data-stu-id="b0f3c-173">Recurrence triggers</span></span>

<span data-ttu-id="b0f3c-174">Opakování aktivační událost se spustí na časový interval, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-174">A recurrence trigger runs at an interval that you specify.</span></span> <span data-ttu-id="b0f3c-175">Pokud je aktivační událost podmíněnou logiku, aktivační události Určuje, jestli je potřeba spustit pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-175">When the trigger has conditional logic, the trigger determines whether the workflow needs to run.</span></span> <span data-ttu-id="b0f3c-176">Aktivační událost určuje pracovního postupu se budou spouštět vrácením `200` stavový kód.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-176">A trigger indicates the workflow should run by returning a `200` status code.</span></span> <span data-ttu-id="b0f3c-177">Když pracovní postup není nutné spustit, vrátí aktivační události `202` stavový kód.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-177">When the workflow doesn't need to run, the trigger returns a `202` status code.</span></span>

### <a name="callback-using-rest-apis"></a><span data-ttu-id="b0f3c-178">Zpětné volání pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="b0f3c-178">Callback using REST APIs</span></span>

<span data-ttu-id="b0f3c-179">Chcete-li spustit pracovní postup, služby zavolat koncový bod logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-179">To start a workflow, services can call a logic app endpoint.</span></span> <span data-ttu-id="b0f3c-180">Chcete-li spustit tento druh logiku aplikace na vyžádání, zvolte **spustit nyní** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="b0f3c-180">To start this kind of logic app on-demand, choose **Run now** on the command bar.</span></span> <span data-ttu-id="b0f3c-181">V tématu [spustit pracovní postupy voláním logiku aplikace koncové body jako triggery](../logic-apps/logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="b0f3c-181">See [Start workflows by calling logic app endpoints as triggers](../logic-apps/logic-apps-http-endpoint.md).</span></span> 

<!-- Shared links -->
<span data-ttu-id="b0f3c-182">[portál Azure]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="b0f3c-182">[Azure portal]: https://portal.azure.com</span></span>

## <a name="next-steps"></a><span data-ttu-id="b0f3c-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b0f3c-183">Next steps</span></span>

* [<span data-ttu-id="b0f3c-184">Příkazech Switch</span><span class="sxs-lookup"><span data-stu-id="b0f3c-184">Switch statements</span></span>](../logic-apps/logic-apps-switch-case.md) 
* [<span data-ttu-id="b0f3c-185">Smyčky, obory a rozdělení dávek</span><span class="sxs-lookup"><span data-stu-id="b0f3c-185">Loops, scopes, and debatching</span></span>](../logic-apps/logic-apps-loops-and-scopes.md)
* [<span data-ttu-id="b0f3c-186">Vytváření definic aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="b0f3c-186">Author logic app definitions</span></span>](../logic-apps/logic-apps-author-definitions.md)