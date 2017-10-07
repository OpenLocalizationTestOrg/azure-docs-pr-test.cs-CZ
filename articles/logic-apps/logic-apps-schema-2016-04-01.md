---
title: "aaaSchema aktualizace června-1-2016 - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: b0347fbbd692a93b63a2f8b741402a225450b35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a><span data-ttu-id="db235-103">Aktualizace schématu pro Azure Logic Apps – 1. června 2016</span><span class="sxs-lookup"><span data-stu-id="db235-103">Schema updates for Azure Logic Apps - June 1, 2016</span></span>

<span data-ttu-id="db235-104">Toto nové schéma a rozhraní API verze pro Azure Logic Apps zahrnuje klíčových vylepšení, které aplikace logiky více spolehlivé a snadnější toouse:</span><span class="sxs-lookup"><span data-stu-id="db235-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier toouse:</span></span>

* <span data-ttu-id="db235-105">[Obory](#scopes) umožňují seskupení nebo vnoření akce jako sadu akcí.</span><span class="sxs-lookup"><span data-stu-id="db235-105">[Scopes](#scopes) let you group or nest actions as a collection of actions.</span></span>
* <span data-ttu-id="db235-106">[Podmínky a smyčky](#conditions-loops) jsou nyní prvotřídní akce.</span><span class="sxs-lookup"><span data-stu-id="db235-106">[Conditions and loops](#conditions-loops) are now first-class actions.</span></span>
* <span data-ttu-id="db235-107">Přesnější řazení pro akce s hello `runAfter` vlastnost nahrazení`dependsOn`</span><span class="sxs-lookup"><span data-stu-id="db235-107">More precise ordering for running actions with hello `runAfter` property, replacing `dependsOn`</span></span>

<span data-ttu-id="db235-108">aplikace logiky z hello 1 srpen 2015 tooupgrade náhled schématu toohello 1. června 2016 schématu, [podívejte se na část upgrade hello](##upgrade-your-schema).</span><span class="sxs-lookup"><span data-stu-id="db235-108">tooupgrade your logic apps from hello August 1, 2015 preview schema toohello June 1, 2016 schema, [check out hello upgrade section](##upgrade-your-schema).</span></span>

<a name="scopes"></a>
## <a name="scopes"></a><span data-ttu-id="db235-109">Obory</span><span class="sxs-lookup"><span data-stu-id="db235-109">Scopes</span></span>

<span data-ttu-id="db235-110">Toto schéma zahrnuje obory, které umožňují akce skupiny společně nebo akce vnoření uvnitř sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="db235-110">This schema includes scopes, which let you group actions together, or nest actions inside each other.</span></span> <span data-ttu-id="db235-111">Podmínku může například obsahovat jiné podmínky.</span><span class="sxs-lookup"><span data-stu-id="db235-111">For example, a condition can contain another condition.</span></span> <span data-ttu-id="db235-112">Další informace o [obor syntaxe](../logic-apps/logic-apps-loops-and-scopes.md), nebo zkontrolujte tento příklad základní obor:</span><span class="sxs-lookup"><span data-stu-id="db235-112">Learn more about [scope syntax](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic scope example:</span></span>

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
## <a name="conditions-and-loops-changes"></a><span data-ttu-id="db235-113">Podmínky a smyčky změny</span><span class="sxs-lookup"><span data-stu-id="db235-113">Conditions and loops changes</span></span>

<span data-ttu-id="db235-114">Ve schématu předchozí verze, podmínky a smyčky byly parametry přidružené k jedné akce.</span><span class="sxs-lookup"><span data-stu-id="db235-114">In previous schema versions, conditions and loops were parameters associated with a single action.</span></span> <span data-ttu-id="db235-115">Toto schéma zruší toto omezení, aby podmínky a smyčky se nyní zobrazí jako typy akcí.</span><span class="sxs-lookup"><span data-stu-id="db235-115">This schema lifts this limitation, so conditions and loops now appear as action types.</span></span> <span data-ttu-id="db235-116">Další informace o [smyčky a oborů](../logic-apps/logic-apps-loops-and-scopes.md), nebo zkontrolujte tento základní příklad pro podmínku akce:</span><span class="sxs-lookup"><span data-stu-id="db235-116">Learn more about [loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic example for a condition action:</span></span>

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
## <a name="runafter-property"></a><span data-ttu-id="db235-117">Vlastnost 'runAfter.</span><span class="sxs-lookup"><span data-stu-id="db235-117">'runAfter' property</span></span>

<span data-ttu-id="db235-118">Hello `runAfter` vlastnost nahradí `dependsOn`, poskytuje další přesnost, když zadáte hello spustit pořadí pro akce na základě hello stavu předchozí akcí.</span><span class="sxs-lookup"><span data-stu-id="db235-118">hello `runAfter` property replaces `dependsOn`, providing more precision when you specify hello run order for actions based on hello status of previous actions.</span></span>

<span data-ttu-id="db235-119">Hello `dependsOn` vlastnost byla totožná s "hello akce byla spuštěna a bylo úspěšné", bez ohledu na to, jak často chtěli byste se nezdařilo. tooexecute akce, podle hello předchozí akce, jestli byla úspěšná nebo přeskočena.</span><span class="sxs-lookup"><span data-stu-id="db235-119">hello `dependsOn` property was synonymous with "hello action ran and was successful", no matter how many times you wanted tooexecute an action, based on whether hello previous action was successful, failed, or skipped.</span></span> <span data-ttu-id="db235-120">Hello `runAfter` vlastnost poskytuje tuto flexibilitu jako objekt, který určuje všechny hello názvy akcí, po které hello objektu spouští.</span><span class="sxs-lookup"><span data-stu-id="db235-120">hello `runAfter` property provides that flexibility as an object that specifies all hello action names after which hello object runs.</span></span> <span data-ttu-id="db235-121">Tato vlastnost také definuje pole stavy, které mohou být použity jako aktivační události.</span><span class="sxs-lookup"><span data-stu-id="db235-121">This property also defines an array of statuses that are acceptable as triggers.</span></span> <span data-ttu-id="db235-122">Například pokud byste chtěli toorun po kroku A úspěšné a také po kroku B úspěšná nebo neúspěšná, vytvoříte tím `runAfter` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="db235-122">For example, if you wanted toorun after step A succeeds and also after step B succeeds or fails, you construct this `runAfter` property:</span></span>

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a><span data-ttu-id="db235-123">Upgradu vašeho schématu</span><span class="sxs-lookup"><span data-stu-id="db235-123">Upgrade your schema</span></span>

<span data-ttu-id="db235-124">Upgrade toohello nové schéma pouze trvá několik kroků.</span><span class="sxs-lookup"><span data-stu-id="db235-124">Upgrading toohello new schema only takes a few steps.</span></span> <span data-ttu-id="db235-125">Hello procesu upgradu zahrnuje spuštění skriptu aktualizace hello ukládání jako novou aplikaci logiky a pokud chcete, které by mohly mít přepsal hello předchozí aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="db235-125">hello upgrade process includes running hello upgrade script, saving as a new logic app, and if you want, possibly overwriting hello previous logic app.</span></span>

1. <span data-ttu-id="db235-126">V hello portálu Azure otevřete aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="db235-126">In hello Azure portal, open your logic app.</span></span>

2. <span data-ttu-id="db235-127">Přejděte příliš**přehled**.</span><span class="sxs-lookup"><span data-stu-id="db235-127">Go too**Overview**.</span></span> <span data-ttu-id="db235-128">Na panelu nástrojů aplikace logiky hello, zvolte **aktualizovat schéma**.</span><span class="sxs-lookup"><span data-stu-id="db235-128">On hello logic app toolbar, choose **Update Schema**.</span></span>
   
    ![Zvolte Aktualizovat schéma][1]
   
    <span data-ttu-id="db235-130">Hello upgradovaný definice se vrátí, které můžete zkopírovat a vložit do definice prostředků v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="db235-130">hello upgraded definition is returned, which you can copy and paste into a resource definition if necessary.</span></span> 
    <span data-ttu-id="db235-131">Ale jsme **důrazně doporučujeme** zvolíte **uložit jako** toomake se, že všechny odkazy na připojení jsou platné v hello upgradovat aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="db235-131">However, we **strongly recommend** you choose **Save As** toomake sure that all connection references are valid in hello upgraded logic app.</span></span>

3. <span data-ttu-id="db235-132">V panelu nástrojů upgradu okno hello, zvolte **uložit jako**.</span><span class="sxs-lookup"><span data-stu-id="db235-132">In hello upgrade blade toolbar, choose **Save As**.</span></span>

4. <span data-ttu-id="db235-133">Zadejte název logiku hello a stav.</span><span class="sxs-lookup"><span data-stu-id="db235-133">Enter hello logic name and status.</span></span> <span data-ttu-id="db235-134">toodeploy aplikace logiky upgradovaný zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="db235-134">toodeploy your upgraded logic app, choose **Create**.</span></span>

5. <span data-ttu-id="db235-135">Potvrďte, že aplikace logiky upgradovaný funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="db235-135">Confirm that your upgraded logic app works as expected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="db235-136">Pokud používáte ruční nebo žádostí o aktivační událost, změní se adresa URL hello zpětného volání v nové aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="db235-136">If you are using a manual or request trigger, hello callback URL changes in your new logic app.</span></span> <span data-ttu-id="db235-137">Test hello nové adresy URL toomake zda hello k kompletní prostředí funguje.</span><span class="sxs-lookup"><span data-stu-id="db235-137">Test hello new URL toomake sure hello end-to-end experience works.</span></span> <span data-ttu-id="db235-138">toopreserve předchozí adresy URL, může klonovat přes existující aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="db235-138">toopreserve previous URLs, you can clone over your existing logic app.</span></span>

6. <span data-ttu-id="db235-139">*Volitelné* zvolte aplikaci logiky předchozích verzí hello nové schéma, na panelu nástrojů hello toooverwrite **klon**, další příliš**aktualizovat schéma**.</span><span class="sxs-lookup"><span data-stu-id="db235-139">*Optional* toooverwrite your previous logic app with hello new schema version, on hello toolbar, choose **Clone**, next too**Update Schema**.</span></span> <span data-ttu-id="db235-140">Tento krok je nutný pouze v případě, že chcete tookeep hello stejné ID nebo žádostí o aktivační události URL prostředku aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="db235-140">This step is necessary only if you want tookeep hello same resource ID or request trigger URL of your logic app.</span></span>

### <a name="upgrade-tool-notes"></a><span data-ttu-id="db235-141">Poznámky k upgradu nástroje</span><span class="sxs-lookup"><span data-stu-id="db235-141">Upgrade tool notes</span></span>

#### <a name="mapping-conditions"></a><span data-ttu-id="db235-142">Mapování podmínky</span><span class="sxs-lookup"><span data-stu-id="db235-142">Mapping conditions</span></span>

<span data-ttu-id="db235-143">V definici hello upgradovat nástroj hello umožňuje usilovně na seskupování true a false větve akce jako obor.</span><span class="sxs-lookup"><span data-stu-id="db235-143">In hello upgraded definition, hello tool makes a best effort at grouping true and false branch actions together as a scope.</span></span> <span data-ttu-id="db235-144">Konkrétně hello návrháře vzor `@equals(actions('a').status, 'Skipped')` mají zobrazit jako `else` akce.</span><span class="sxs-lookup"><span data-stu-id="db235-144">Specifically, hello designer pattern of `@equals(actions('a').status, 'Skipped')` should appear as an `else` action.</span></span> <span data-ttu-id="db235-145">Ale pokud hello nástroj zjistí nelze rozpoznat vzory, hello nástroj může vytvořit oddělené podmínky pro hello true a false větve hello.</span><span class="sxs-lookup"><span data-stu-id="db235-145">However, if hello tool detects unrecognizable patterns, hello tool might create separate conditions for both hello true and hello false branch.</span></span> <span data-ttu-id="db235-146">Můžete změnit mapování akcí po upgradu, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="db235-146">You can remap actions after upgrading, if necessary.</span></span>

#### <a name="foreach-loop-with-condition"></a><span data-ttu-id="db235-147">smyčka 'typu foreach' s podmínkou</span><span class="sxs-lookup"><span data-stu-id="db235-147">'foreach' loop with condition</span></span>

<span data-ttu-id="db235-148">V nové schéma hello, můžete použít hello filtru akce tooreplicate hello vzor `foreach` smyčky s podmínkou na položku, ale tato změna automaticky dojde při upgradu.</span><span class="sxs-lookup"><span data-stu-id="db235-148">In hello new schema, you can use hello filter action tooreplicate hello pattern of a `foreach` loop with a condition per item, but this change should automatically happen when you upgrade.</span></span> <span data-ttu-id="db235-149">Podmínka Hello stane filtr akce před hello smyčka typu foreach pro vrácení pouze pole položek, které vyhovují podmínce hello a tohoto pole je předána do hello foreach akce.</span><span class="sxs-lookup"><span data-stu-id="db235-149">hello condition becomes a filter action before hello foreach loop for returning only an array of items that match hello condition, and that array is passed into hello foreach action.</span></span> <span data-ttu-id="db235-150">Příklad, naleznete v části [smyčky a obory](../logic-apps/logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="db235-150">For an example, see [Loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md).</span></span>

#### <a name="resource-tags"></a><span data-ttu-id="db235-151">Značky prostředku</span><span class="sxs-lookup"><span data-stu-id="db235-151">Resource tags</span></span>

<span data-ttu-id="db235-152">Po upgradu se značky prostředku se odeberou, takže je nutné obnovit pro pracovní postup hello upgradovat.</span><span class="sxs-lookup"><span data-stu-id="db235-152">After you upgrade, resource tags are removed, so you must reset them for hello upgraded workflow.</span></span>

## <a name="other-changes"></a><span data-ttu-id="db235-153">Další změny</span><span class="sxs-lookup"><span data-stu-id="db235-153">Other changes</span></span>

### <a name="renamed-manual-trigger-toorequest-trigger"></a><span data-ttu-id="db235-154">Přejmenovat too'request "Ruční" aktivační událost ' aktivační událost</span><span class="sxs-lookup"><span data-stu-id="db235-154">Renamed 'manual' trigger too'request' trigger</span></span>

<span data-ttu-id="db235-155">Hello `manual` typ aktivační událost byla zastaralé a přejmenovat příliš`request` s typem `http`.</span><span class="sxs-lookup"><span data-stu-id="db235-155">hello `manual` trigger type was deprecated and renamed too`request` with type `http`.</span></span> <span data-ttu-id="db235-156">Tato změna vytvoří další konzistence pro hello druh vzor, který hello aktivační události je použité toobuild.</span><span class="sxs-lookup"><span data-stu-id="db235-156">This change creates more consistency for hello kind of pattern that hello trigger is used toobuild.</span></span>

### <a name="new-filter-action"></a><span data-ttu-id="db235-157">Nová akce 'filtru.</span><span class="sxs-lookup"><span data-stu-id="db235-157">New 'filter' action</span></span>

<span data-ttu-id="db235-158">toofilter velké pole dolů tooa menší sadu položek hello nové `filter` typu přijímá pole a podmínku, vyhodnotí hello podmínku pro každou položku a vrátí pole s položkami splnění podmínky hello.</span><span class="sxs-lookup"><span data-stu-id="db235-158">toofilter a large array down tooa smaller set of items, hello new `filter` type accepts an array and a condition, evaluates hello condition for each item, and returns an array with items meeting hello condition.</span></span>

### <a name="restrictions-for-foreach-and-until-actions"></a><span data-ttu-id="db235-159">Omezení pro "foreach" a "do" akce</span><span class="sxs-lookup"><span data-stu-id="db235-159">Restrictions for 'foreach' and 'until' actions</span></span>

<span data-ttu-id="db235-160">Hello `foreach` a `until` smyčky jsou omezené tooa jedné akce.</span><span class="sxs-lookup"><span data-stu-id="db235-160">hello `foreach` and `until` loop are restricted tooa single action.</span></span>

### <a name="new-trackedproperties-for-actions"></a><span data-ttu-id="db235-161">Nový 'trackedProperties' pro akce</span><span class="sxs-lookup"><span data-stu-id="db235-161">New 'trackedProperties' for actions</span></span>

<span data-ttu-id="db235-162">Akce nyní můžete mít další vlastnost s názvem `trackedProperties`, který je na stejné úrovni toohello `runAfter` a `type` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="db235-162">Actions can now have an additional property called `trackedProperties`, which is sibling toohello `runAfter` and `type` properties.</span></span> <span data-ttu-id="db235-163">Tento objekt určuje určité akce vstupy nebo výstupy, které chcete tooinclude v Azure diagnostiky telemetrii hello vygenerované jako součást pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="db235-163">This object specifies certain action inputs or outputs that you want tooinclude in hello Azure Diagnostic telemetry, emitted as part of a workflow.</span></span> <span data-ttu-id="db235-164">Například:</span><span class="sxs-lookup"><span data-stu-id="db235-164">For example:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="db235-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db235-165">Next Steps</span></span>
* [<span data-ttu-id="db235-166">Vytvoření definice pracovního postupu pro logic apps</span><span class="sxs-lookup"><span data-stu-id="db235-166">Create workflow definitions for logic apps</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="db235-167">Vytvoření šablony pro nasazení pro logic apps</span><span class="sxs-lookup"><span data-stu-id="db235-167">Create deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
