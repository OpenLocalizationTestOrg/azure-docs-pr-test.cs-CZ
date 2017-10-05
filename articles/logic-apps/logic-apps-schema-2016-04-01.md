---
title: "Schéma se aktualizuje června-1-2016 - Azure Logic Apps | Microsoft Docs"
description: "Vytvoření definic JSON pro Azure Logic Apps s verzí schématu 2016-06-01"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/25/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 43df04d6478e44c82c88b17d916cfc9fe4afc03e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a><span data-ttu-id="fc301-103">Aktualizace schématu pro Azure Logic Apps – 1. června 2016</span><span class="sxs-lookup"><span data-stu-id="fc301-103">Schema updates for Azure Logic Apps - June 1, 2016</span></span>

<span data-ttu-id="fc301-104">Toto nové schéma a rozhraní API verze pro Azure Logic Apps zahrnuje klíčových vylepšení, které aplikace logiky spolehlivější a jednodušší použít:</span><span class="sxs-lookup"><span data-stu-id="fc301-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier to use:</span></span>

* <span data-ttu-id="fc301-105">[Obory](#scopes) umožňují seskupení nebo vnoření akce jako sadu akcí.</span><span class="sxs-lookup"><span data-stu-id="fc301-105">[Scopes](#scopes) let you group or nest actions as a collection of actions.</span></span>
* <span data-ttu-id="fc301-106">[Podmínky a smyčky](#conditions-loops) jsou nyní prvotřídní akce.</span><span class="sxs-lookup"><span data-stu-id="fc301-106">[Conditions and loops](#conditions-loops) are now first-class actions.</span></span>
* <span data-ttu-id="fc301-107">Přesnější řazení pro spouštění akcí, které se `runAfter` vlastnost nahrazení`dependsOn`</span><span class="sxs-lookup"><span data-stu-id="fc301-107">More precise ordering for running actions with the `runAfter` property, replacing `dependsOn`</span></span>

<span data-ttu-id="fc301-108">K upgradu aplikace logiky na schéma 1. června 2016 schéma preview 1 srpen 2015 [podívejte se na část upgrade](##upgrade-your-schema).</span><span class="sxs-lookup"><span data-stu-id="fc301-108">To upgrade your logic apps from the August 1, 2015 preview schema to the June 1, 2016 schema, [check out the upgrade section](##upgrade-your-schema).</span></span>

<a name="scopes"></a>
## <a name="scopes"></a><span data-ttu-id="fc301-109">Obory</span><span class="sxs-lookup"><span data-stu-id="fc301-109">Scopes</span></span>

<span data-ttu-id="fc301-110">Toto schéma zahrnuje obory, které umožňují akce skupiny společně nebo akce vnoření uvnitř sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="fc301-110">This schema includes scopes, which let you group actions together, or nest actions inside each other.</span></span> <span data-ttu-id="fc301-111">Podmínku může například obsahovat jiné podmínky.</span><span class="sxs-lookup"><span data-stu-id="fc301-111">For example, a condition can contain another condition.</span></span> <span data-ttu-id="fc301-112">Další informace o [obor syntaxe](../logic-apps/logic-apps-loops-and-scopes.md), nebo zkontrolujte tento příklad základní obor:</span><span class="sxs-lookup"><span data-stu-id="fc301-112">Learn more about [scope syntax](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic scope example:</span></span>

```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

<a name="conditions-loops"></a>
## <a name="conditions-and-loops-changes"></a><span data-ttu-id="fc301-113">Podmínky a smyčky změny</span><span class="sxs-lookup"><span data-stu-id="fc301-113">Conditions and loops changes</span></span>

<span data-ttu-id="fc301-114">Ve schématu předchozí verze, podmínky a smyčky byly parametry přidružené k jedné akce.</span><span class="sxs-lookup"><span data-stu-id="fc301-114">In previous schema versions, conditions and loops were parameters associated with a single action.</span></span> <span data-ttu-id="fc301-115">Toto schéma zruší toto omezení, aby podmínky a smyčky se nyní zobrazí jako typy akcí.</span><span class="sxs-lookup"><span data-stu-id="fc301-115">This schema lifts this limitation, so conditions and loops now appear as action types.</span></span> <span data-ttu-id="fc301-116">Další informace o [smyčky a oborů](../logic-apps/logic-apps-loops-and-scopes.md), nebo zkontrolujte tento základní příklad pro podmínku akce:</span><span class="sxs-lookup"><span data-stu-id="fc301-116">Learn more about [loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic example for a condition action:</span></span>

```
{
    "If_trigger_is_some-trigger": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'some-trigger')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_another-trigger": "..."
        }      
    }
}
```

<a name="run-after"></a>
## <a name="runafter-property"></a><span data-ttu-id="fc301-117">Vlastnost 'runAfter.</span><span class="sxs-lookup"><span data-stu-id="fc301-117">'runAfter' property</span></span>

<span data-ttu-id="fc301-118">`runAfter` Vlastnost nahradí `dependsOn`, poskytuje další přesnost, když zadáte pořadí spouštění pro akce na základě stavu předchozí akcí.</span><span class="sxs-lookup"><span data-stu-id="fc301-118">The `runAfter` property replaces `dependsOn`, providing more precision when you specify the run order for actions based on the status of previous actions.</span></span>

<span data-ttu-id="fc301-119">`dependsOn` Vlastnost byla totožná s "akce byla spuštěna a bylo úspěšné", bez ohledu na počet opakování, které jste chtěli provést akci, v závislosti na tom, jestli byla úspěšná, předchozí akce se nezdařilo nebo přeskočen.</span><span class="sxs-lookup"><span data-stu-id="fc301-119">The `dependsOn` property was synonymous with "the action ran and was successful", no matter how many times you wanted to execute an action, based on whether the previous action was successful, failed, or skipped.</span></span> <span data-ttu-id="fc301-120">`runAfter` Vlastnost poskytuje tuto flexibilitu jako objekt, který určuje všechny názvy akcí, po jejichž uplynutí se spouští objekt.</span><span class="sxs-lookup"><span data-stu-id="fc301-120">The `runAfter` property provides that flexibility as an object that specifies all the action names after which the object runs.</span></span> <span data-ttu-id="fc301-121">Tato vlastnost také definuje pole stavy, které mohou být použity jako aktivační události.</span><span class="sxs-lookup"><span data-stu-id="fc301-121">This property also defines an array of statuses that are acceptable as triggers.</span></span> <span data-ttu-id="fc301-122">Například pokud chcete spustit po kroku A úspěšné a také po kroku B úspěšná nebo neúspěšná, vytvoříte tím `runAfter` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="fc301-122">For example, if you wanted to run after step A succeeds and also after step B succeeds or fails, you construct this `runAfter` property:</span></span>

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a><span data-ttu-id="fc301-123">Upgradu vašeho schématu</span><span class="sxs-lookup"><span data-stu-id="fc301-123">Upgrade your schema</span></span>

<span data-ttu-id="fc301-124">Upgrade na nové schéma trvá jenom pár kroků.</span><span class="sxs-lookup"><span data-stu-id="fc301-124">Upgrading to the new schema only takes a few steps.</span></span> <span data-ttu-id="fc301-125">Proces upgradu zahrnuje spuštěním skriptu upgradu, uložit jako novou aplikaci logiky a pokud chcete, které by mohly mít přepsal předchozí aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="fc301-125">The upgrade process includes running the upgrade script, saving as a new logic app, and if you want, possibly overwriting the previous logic app.</span></span>

1. <span data-ttu-id="fc301-126">Na portálu Azure otevřete aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="fc301-126">In the Azure portal, open your logic app.</span></span>

2. <span data-ttu-id="fc301-127">Přejděte na **přehled**.</span><span class="sxs-lookup"><span data-stu-id="fc301-127">Go to **Overview**.</span></span> <span data-ttu-id="fc301-128">Na panelu nástrojů aplikace logiky, zvolte **aktualizovat schéma**.</span><span class="sxs-lookup"><span data-stu-id="fc301-128">On the logic app toolbar, choose **Update Schema**.</span></span>
   
    ![Zvolte Aktualizovat schéma][1]
   
    <span data-ttu-id="fc301-130">Upgradovaná definice se vrátí, které můžete zkopírovat a vložit do definice prostředků v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="fc301-130">The upgraded definition is returned, which you can copy and paste into a resource definition if necessary.</span></span> 
    <span data-ttu-id="fc301-131">Ale jsme **důrazně doporučujeme** zvolíte **uložit jako** a ujistěte se, že všechny odkazy na připojení jsou platné v aplikaci logiky upgradovaný.</span><span class="sxs-lookup"><span data-stu-id="fc301-131">However, we **strongly recommend** you choose **Save As** to make sure that all connection references are valid in the upgraded logic app.</span></span>

3. <span data-ttu-id="fc301-132">Na panelu nástrojů okna upgradu zvolte **uložit jako**.</span><span class="sxs-lookup"><span data-stu-id="fc301-132">In the upgrade blade toolbar, choose **Save As**.</span></span>

4. <span data-ttu-id="fc301-133">Zadejte název logiku a stav.</span><span class="sxs-lookup"><span data-stu-id="fc301-133">Enter the logic name and status.</span></span> <span data-ttu-id="fc301-134">Chcete-li nasadit aplikaci logiky upgradovaný, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fc301-134">To deploy your upgraded logic app, choose **Create**.</span></span>

5. <span data-ttu-id="fc301-135">Potvrďte, že aplikace logiky upgradovaný funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="fc301-135">Confirm that your upgraded logic app works as expected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="fc301-136">Pokud používáte ruční nebo žádostí o aktivační událost, se změní adresa URL zpětného volání v nové aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="fc301-136">If you are using a manual or request trigger, the callback URL changes in your new logic app.</span></span> <span data-ttu-id="fc301-137">Otestujte novou adresu URL a ujistěte, že funguje prostředí začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="fc301-137">Test the new URL to make sure the end-to-end experience works.</span></span> <span data-ttu-id="fc301-138">Pokud chcete zachovat předchozí adresy URL, může klonovat přes existující aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="fc301-138">To preserve previous URLs, you can clone over your existing logic app.</span></span>

6. <span data-ttu-id="fc301-139">*Volitelné* přepsat předchozí aplikaci logiky v nové verzi schématu, na panelu nástrojů vyberte **klon**vedle možnosti **aktualizovat schéma**.</span><span class="sxs-lookup"><span data-stu-id="fc301-139">*Optional* To overwrite your previous logic app with the new schema version, on the toolbar, choose **Clone**, next to **Update Schema**.</span></span> <span data-ttu-id="fc301-140">Tento krok je nutné pouze v případě, že chcete zachovat stejné ID prostředku nebo aktivační událost adresa URL aplikace logiky požadavku.</span><span class="sxs-lookup"><span data-stu-id="fc301-140">This step is necessary only if you want to keep the same resource ID or request trigger URL of your logic app.</span></span>

### <a name="upgrade-tool-notes"></a><span data-ttu-id="fc301-141">Poznámky k upgradu nástroje</span><span class="sxs-lookup"><span data-stu-id="fc301-141">Upgrade tool notes</span></span>

#### <a name="mapping-conditions"></a><span data-ttu-id="fc301-142">Mapování podmínky</span><span class="sxs-lookup"><span data-stu-id="fc301-142">Mapping conditions</span></span>

<span data-ttu-id="fc301-143">V definici upgradovaný nástroj provede usilovně na seskupování true a false větve akce jako obor.</span><span class="sxs-lookup"><span data-stu-id="fc301-143">In the upgraded definition, the tool makes a best effort at grouping true and false branch actions together as a scope.</span></span> <span data-ttu-id="fc301-144">Konkrétně návrháře vzor `@equals(actions('a').status, 'Skipped')` mají zobrazit jako `else` akce.</span><span class="sxs-lookup"><span data-stu-id="fc301-144">Specifically, the designer pattern of `@equals(actions('a').status, 'Skipped')` should appear as an `else` action.</span></span> <span data-ttu-id="fc301-145">Ale pokud nástroj zjistí nelze rozpoznat vzory, nástroj může vytvořit oddělené podmínky pro hodnotu true a false větev.</span><span class="sxs-lookup"><span data-stu-id="fc301-145">However, if the tool detects unrecognizable patterns, the tool might create separate conditions for both the true and the false branch.</span></span> <span data-ttu-id="fc301-146">Můžete změnit mapování akcí po upgradu, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="fc301-146">You can remap actions after upgrading, if necessary.</span></span>

#### <a name="foreach-loop-with-condition"></a><span data-ttu-id="fc301-147">smyčka 'typu foreach' s podmínkou</span><span class="sxs-lookup"><span data-stu-id="fc301-147">'foreach' loop with condition</span></span>

<span data-ttu-id="fc301-148">V novém schématu, můžete použít k replikaci vzor akci filtru `foreach` smyčky s podmínkou na položku, ale tato změna automaticky dojde při upgradu.</span><span class="sxs-lookup"><span data-stu-id="fc301-148">In the new schema, you can use the filter action to replicate the pattern of a `foreach` loop with a condition per item, but this change should automatically happen when you upgrade.</span></span> <span data-ttu-id="fc301-149">Stav se změní na filtr akce před smyčka typu foreach pro vrácení pouze pole položek, které splňují podmínku a tohoto pole je předána do foreach akce.</span><span class="sxs-lookup"><span data-stu-id="fc301-149">The condition becomes a filter action before the foreach loop for returning only an array of items that match the condition, and that array is passed into the foreach action.</span></span> <span data-ttu-id="fc301-150">Příklad, naleznete v části [smyčky a obory](../logic-apps/logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="fc301-150">For an example, see [Loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md).</span></span>

#### <a name="resource-tags"></a><span data-ttu-id="fc301-151">Značky prostředku</span><span class="sxs-lookup"><span data-stu-id="fc301-151">Resource tags</span></span>

<span data-ttu-id="fc301-152">Po upgradu se značky prostředku se odeberou, takže je nutné obnovit upgradovaný pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="fc301-152">After you upgrade, resource tags are removed, so you must reset them for the upgraded workflow.</span></span>

## <a name="other-changes"></a><span data-ttu-id="fc301-153">Další změny</span><span class="sxs-lookup"><span data-stu-id="fc301-153">Other changes</span></span>

### <a name="renamed-manual-trigger-to-request-trigger"></a><span data-ttu-id="fc301-154">Přejmenovat aktivační události "Ruční" k aktivační události "žádostí"</span><span class="sxs-lookup"><span data-stu-id="fc301-154">Renamed 'manual' trigger to 'request' trigger</span></span>

<span data-ttu-id="fc301-155">`manual` Typ aktivační událost byla zastaralé a přejmenován na `request` s typem `http`.</span><span class="sxs-lookup"><span data-stu-id="fc301-155">The `manual` trigger type was deprecated and renamed to `request` with type `http`.</span></span> <span data-ttu-id="fc301-156">Tato změna vytvoří další konzistenci pro druh vzor, která má aktivační procedura slouží k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="fc301-156">This change creates more consistency for the kind of pattern that the trigger is used to build.</span></span>

### <a name="new-filter-action"></a><span data-ttu-id="fc301-157">Nová akce 'filtru.</span><span class="sxs-lookup"><span data-stu-id="fc301-157">New 'filter' action</span></span>

<span data-ttu-id="fc301-158">Chcete-li filtrovat velké pole dolů menší sadu položek, nové `filter` typu přijímá pole a podmínku, vyhodnocuje podmínku pro každou položku a vrátí pole s položkami, které splňují podmínku.</span><span class="sxs-lookup"><span data-stu-id="fc301-158">To filter a large array down to a smaller set of items, the new `filter` type accepts an array and a condition, evaluates the condition for each item, and returns an array with items meeting the condition.</span></span>

### <a name="restrictions-for-foreach-and-until-actions"></a><span data-ttu-id="fc301-159">Omezení pro "foreach" a "do" akce</span><span class="sxs-lookup"><span data-stu-id="fc301-159">Restrictions for 'foreach' and 'until' actions</span></span>

<span data-ttu-id="fc301-160">`foreach` a `until` smyčky jsou omezeny na jednu akci.</span><span class="sxs-lookup"><span data-stu-id="fc301-160">The `foreach` and `until` loop are restricted to a single action.</span></span>

### <a name="new-trackedproperties-for-actions"></a><span data-ttu-id="fc301-161">Nový 'trackedProperties' pro akce</span><span class="sxs-lookup"><span data-stu-id="fc301-161">New 'trackedProperties' for actions</span></span>

<span data-ttu-id="fc301-162">Akce nyní můžete mít další vlastnost s názvem `trackedProperties`, který je na stejné úrovni k `runAfter` a `type` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fc301-162">Actions can now have an additional property called `trackedProperties`, which is sibling to the `runAfter` and `type` properties.</span></span> <span data-ttu-id="fc301-163">Tento objekt určuje určité akce vstupů nebo výstupů, které chcete zahrnout do Azure diagnostiky telemetrii, vygenerované jako součást pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="fc301-163">This object specifies certain action inputs or outputs that you want to include in the Azure Diagnostic telemetry, emitted as part of a workflow.</span></span> <span data-ttu-id="fc301-164">Například:</span><span class="sxs-lookup"><span data-stu-id="fc301-164">For example:</span></span>

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="fc301-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc301-165">Next Steps</span></span>
* [<span data-ttu-id="fc301-166">Vytvoření definice pracovního postupu pro logic apps</span><span class="sxs-lookup"><span data-stu-id="fc301-166">Create workflow definitions for logic apps</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="fc301-167">Vytvoření šablony pro nasazení pro logic apps</span><span class="sxs-lookup"><span data-stu-id="fc301-167">Create deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
