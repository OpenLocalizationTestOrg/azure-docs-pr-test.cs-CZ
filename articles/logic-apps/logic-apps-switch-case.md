---
title: "příkaz aaaSwitch pro různé akce v Azure Logic Apps | Microsoft Docs"
description: "Zvolte tooperform různé akce v logiku aplikace založené na výrazu hodnoty pomocí příkazu switch"
services: logic-apps
keywords: "Switch – příkaz"
author: derek1ee
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: 09ed7e4a752003aba157e9156bf4dc89ef86f5ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a><span data-ttu-id="61901-104">Provádění různých akcí v aplikacích logiky s příkaz switch</span><span class="sxs-lookup"><span data-stu-id="61901-104">Perform different actions in logic apps with a switch statement</span></span>

<span data-ttu-id="61901-105">Při vytváření pracovního postupu, máte často tootake různé akce na základě hodnoty hello objektu nebo výraz.</span><span class="sxs-lookup"><span data-stu-id="61901-105">When authoring a workflow, you often have tootake different actions based on hello value of an object or expression.</span></span> <span data-ttu-id="61901-106">Můžete například vaše toobehave aplikace logiky různě v závislosti na hello kód stavu požadavku HTTP, nebo možnosti vybrané v e-mailu.</span><span class="sxs-lookup"><span data-stu-id="61901-106">For example, you might want your logic app toobehave differently based on hello status code of an HTTP request, or an option selected in an email.</span></span>

<span data-ttu-id="61901-107">Příkaz tooimplement přepínač můžete použít tyto scénáře.</span><span class="sxs-lookup"><span data-stu-id="61901-107">You can use a switch statement tooimplement these scenarios.</span></span> <span data-ttu-id="61901-108">Aplikace logiky můžete vyhodnotit token nebo výraz a zvolte hello případu se hello stejnou hodnotu tooexecute hello zadané akce.</span><span class="sxs-lookup"><span data-stu-id="61901-108">Your logic app can evaluate a token or expression, and choose hello case with hello same value tooexecute hello specified actions.</span></span> <span data-ttu-id="61901-109">Příkaz switch hello by měl odpovídat pouze jeden případ.</span><span class="sxs-lookup"><span data-stu-id="61901-109">Only one case should match hello switch statement.</span></span>

> [!TIP]
> <span data-ttu-id="61901-110">Podobně jako všechny programovacích jazyků příkazech switch podporují pouze operátory rovnosti.</span><span class="sxs-lookup"><span data-stu-id="61901-110">Like all programming languages, switch statements support only equality operators.</span></span> <span data-ttu-id="61901-111">Pokud potřebujete další relační operátory, jako je třeba "větší než", použijte příkaz podmínky.</span><span class="sxs-lookup"><span data-stu-id="61901-111">If you need other relational operators, such as "greater than", use a condition statement.</span></span>
> <span data-ttu-id="61901-112">chování při spuštění deterministickou tooensure, případů musí obsahovat jedinečný a statické hodnotu místo dynamické tokeny nebo výraz.</span><span class="sxs-lookup"><span data-stu-id="61901-112">tooensure deterministic execution behavior, cases must contain a unique and static value instead of dynamic tokens or expression.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61901-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="61901-113">Prerequisites</span></span>

- <span data-ttu-id="61901-114">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="61901-114">An active Azure subscription.</span></span> <span data-ttu-id="61901-115">Pokud nemáte předplatné Azure aktivní [vytvořit bezplatný účet](https://azure.microsoft.com/free/), nebo zkuste [volné aplikace logiky pro](https://tryappservice.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="61901-115">If you don't have an active Azure subscription, [create a free account](https://azure.microsoft.com/free/), or try [Logic Apps for free](https://tryappservice.azure.com/).</span></span>
- [<span data-ttu-id="61901-116">Základní znalosti o aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="61901-116">Basic knowledge about logic apps</span></span>](logic-apps-what-are-logic-apps.md)

## <a name="add-a-switch-statement-tooyour-workflow"></a><span data-ttu-id="61901-117">Přidat pracovní postup tooyour přepínače – příkaz</span><span class="sxs-lookup"><span data-stu-id="61901-117">Add a switch statement tooyour workflow</span></span>

<span data-ttu-id="61901-118">Jak funguje příkazu switch tooshow, tento příklad vytvoří aplikace logiky, zda soubory monitorování nahrán tooDropbox.</span><span class="sxs-lookup"><span data-stu-id="61901-118">tooshow how a switch statement works, this example creates a logic app that monitors files uploaded tooDropbox.</span></span> <span data-ttu-id="61901-119">Když jsou odeslány hello nové soubory, aplikace logiky hello odešle e-mailu tooan schvalovatel, kterého se tedy rozhodne zda tootransfer tooSharePoint těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="61901-119">When hello new files are uploaded, hello logic app sends email tooan approver who chooses whether tootransfer those files tooSharePoint.</span></span> <span data-ttu-id="61901-120">aplikace Hello používá příkaz přepínač, který provádí různé akce na základě hello hodnoty, které hello schvalovatel vybere.</span><span class="sxs-lookup"><span data-stu-id="61901-120">hello app uses a switch statement that performs different actions based on hello value that hello approver selects.</span></span>

1. <span data-ttu-id="61901-121">Vytvoření aplikace logiky a vyberte této aktivační události: **Dropboxu - Pokud dojde k vytvoření souboru**.</span><span class="sxs-lookup"><span data-stu-id="61901-121">Create a logic app, and select this trigger: **Dropbox - When a file is created**.</span></span>

   ![Použití Dropboxu - Pokud dojde k vytvoření souboru aktivační události](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. <span data-ttu-id="61901-123">V části hello aktivační událost, přidejte tuto akci: **Outlook.com - odesílání schválení e-mailu**</span><span class="sxs-lookup"><span data-stu-id="61901-123">Under hello trigger, add this action: **Outlook.com - Send approval email**</span></span>

   > [!TIP]
   > <span data-ttu-id="61901-124">Aplikace logiky také podporují odesílání e-mailu scénáře schválení z účtu Office 365 Outlook.</span><span class="sxs-lookup"><span data-stu-id="61901-124">Logic apps also support sending approval email scenarios from an Office 365 Outlook account.</span></span>

   - <span data-ttu-id="61901-125">Pokud nemáte existující připojení, se zobrazí výzva toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="61901-125">If you don't have an existing connection, you're prompted toocreate one.</span></span>
   - <span data-ttu-id="61901-126">Vyplňte hello požadované pole.</span><span class="sxs-lookup"><span data-stu-id="61901-126">Fill in hello required fields.</span></span> <span data-ttu-id="61901-127">Například v položce **k**, zadejte e-mailové adresy hello hello schvalovatel e-mailem.</span><span class="sxs-lookup"><span data-stu-id="61901-127">For example, under **To**, specify hello email address for sending hello approver email.</span></span>
   - <span data-ttu-id="61901-128">V části **možností uživatele**, zadejte `Approve, Reject`.</span><span class="sxs-lookup"><span data-stu-id="61901-128">Under **User Options**, enter `Approve, Reject`.</span></span>

   ![Konfigurace připojení](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. <span data-ttu-id="61901-130">Přidejte příkaz, přepínač.</span><span class="sxs-lookup"><span data-stu-id="61901-130">Add a switch statement.</span></span>

   - <span data-ttu-id="61901-131">Vyberte **+ nový krok** > **... Další** > **přidání přepínače případu**.</span><span class="sxs-lookup"><span data-stu-id="61901-131">Select **+ New step** > **... More** > **Add a switch case**.</span></span> 
   - <span data-ttu-id="61901-132">Teď chceme tooselect hello akce tooperform podle hello `SelectedOptions` výstup z hello *odeslání e-mailu schválení* akce.</span><span class="sxs-lookup"><span data-stu-id="61901-132">Now we want tooselect hello action tooperform based on hello `SelectedOptions` output from hello *Send approval email* action.</span></span> 
   <span data-ttu-id="61901-133">Toto pole můžete najít v hello **přidávat dynamický obsah** selektor.</span><span class="sxs-lookup"><span data-stu-id="61901-133">You can find this field in hello **Add dynamic content** selector.</span></span>
   - <span data-ttu-id="61901-134">Použití *případ 1* toohandle hello schvalovatel výběrem `Approve`.</span><span class="sxs-lookup"><span data-stu-id="61901-134">Use *Case 1* toohandle when hello approver selects `Approve`.</span></span>
     - <span data-ttu-id="61901-135">Pokud, zkopírujte hello původní soubor tooSharePoint Online s hello [ **SharePoint Online – vytvoření souboru** akce](../connectors/connectors-create-api-sharepointonline.md).</span><span class="sxs-lookup"><span data-stu-id="61901-135">If approved, copy hello original file tooSharePoint Online with hello [**SharePoint Online - Create file** action](../connectors/connectors-create-api-sharepointonline.md).</span></span>
     - <span data-ttu-id="61901-136">Přidejte další akci hello případu toonotify uživatelé, že nový soubor je k dispozici na webu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="61901-136">Add another action within hello case toonotify users that a new file is available on SharePoint.</span></span>
   - <span data-ttu-id="61901-137">Přidejte jiný případu toohandle, pokud uživatel vybere `Reject`.</span><span class="sxs-lookup"><span data-stu-id="61901-137">Add another case toohandle when user selects `Reject`.</span></span>
     - <span data-ttu-id="61901-138">Pokud zamítnutí, odesílat e-mailové oznámení, informuje o další schvalovatele, že soubor hello je odmítnut a není nutná žádná další akce.</span><span class="sxs-lookup"><span data-stu-id="61901-138">If rejected, send a notification email informing other approvers that hello file is rejected and no further action is required.</span></span>
   - <span data-ttu-id="61901-139">`SelectedOptions`poskytuje pouze dvě možnosti, takže jsme můžete nechat hello **výchozí** případ prázdný.</span><span class="sxs-lookup"><span data-stu-id="61901-139">`SelectedOptions` provides only two options, so we can leave hello **Default** case empty.</span></span>

   ![Switch – příkaz](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > <span data-ttu-id="61901-141">Příkaz switch musí mít alespoň jeden případ v případě výchozí toohello přidání.</span><span class="sxs-lookup"><span data-stu-id="61901-141">A switch statement needs at least one case in addition toohello default case.</span></span>

4. <span data-ttu-id="61901-142">Po příkazu switch hello, odstranit původní soubor nahrát tooDropbox hello přidáním tuto akci: **Dropbox – odstranit soubor**</span><span class="sxs-lookup"><span data-stu-id="61901-142">After hello switch statement, delete hello original file uploaded tooDropbox by adding this action: **Dropbox - Delete file**</span></span>

5. <span data-ttu-id="61901-143">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="61901-143">Save your logic app.</span></span> <span data-ttu-id="61901-144">Testování aplikace s tím, že nahrajete soubor tooDropbox.</span><span class="sxs-lookup"><span data-stu-id="61901-144">Test your app by uploading a file tooDropbox.</span></span> <span data-ttu-id="61901-145">Měli byste obdržet e-mailu schválení za chvíli.</span><span class="sxs-lookup"><span data-stu-id="61901-145">You should receive an approval email shortly.</span></span> <span data-ttu-id="61901-146">Vyberte možnost a sledovat chování hello.</span><span class="sxs-lookup"><span data-stu-id="61901-146">Select an option, and observe hello behavior.</span></span>

   > [!TIP]
   > <span data-ttu-id="61901-147">Podívat, jak příliš[sledování funkce logic apps](logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="61901-147">Check out how too[monitor your logic apps](logic-apps-monitor-your-logic-apps.md).</span></span>

## <a name="understand-hello-code-behind-switch-statements"></a><span data-ttu-id="61901-148">Pochopení kódu hello příkazech switch</span><span class="sxs-lookup"><span data-stu-id="61901-148">Understand hello code behind switch statements</span></span>

<span data-ttu-id="61901-149">Teď, když jste úspěšně vytvořili aplikaci logiky, pomocí příkazu switch, podíváme se na definice kódu hello za příkaz switch hello.</span><span class="sxs-lookup"><span data-stu-id="61901-149">Now that you successfully created a logic app using a switch statement, let's look at hello code definition behind hello switch statement.</span></span>

```json
"Switch": {
    "type": "Switch",
    "expression": "@body('Send_approval_email')?['SelectedOption']",
    "cases": {
        "Case 1" : {
            "case" : "Approved",
            "actions" : {}
        },
        "Case 2" : {
            "case" : "Rejected",
            "actions" : {}
        }
    },
    "default": {
        "actions": {}
    },
    "runAfter": {
        "Send_approval_email": [
            "Succeeded"
        ]
    }
}
```

* <span data-ttu-id="61901-150">`"Switch"`je název hello tohoto příkazu hello přepínače, které můžete přejmenovat čitelnější.</span><span class="sxs-lookup"><span data-stu-id="61901-150">`"Switch"` is hello name of hello switch statement, which you can rename for readability.</span></span> 
* <span data-ttu-id="61901-151">`"type": "Switch"`Označuje, že akce hello je příkaz switch.</span><span class="sxs-lookup"><span data-stu-id="61901-151">`"type": "Switch"` indicates that hello action is a switch statement.</span></span> 
* <span data-ttu-id="61901-152">`"expression"`vyhodnotí pro každý případ deklarovaný později v definici hello je hello schvalovatel vybranou možnost v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="61901-152">`"expression"` is hello approver's selected option in this example and is evaluated against each case declared later in hello definition.</span></span> 
* <span data-ttu-id="61901-153">`"cases"`může obsahovat libovolný počet případů.</span><span class="sxs-lookup"><span data-stu-id="61901-153">`"cases"` can contain any number of cases.</span></span> <span data-ttu-id="61901-154">Pro každý případ `"Case *"` je výchozí název hello hello případu, kdy můžete přejmenovat čitelnější.</span><span class="sxs-lookup"><span data-stu-id="61901-154">For each case, `"Case *"` is hello default name of hello case, which you can rename for readability.</span></span> 
<span data-ttu-id="61901-155">`"case"`Určuje hello případu štítku, kterou hello používá přepínač výraz pro porovnání a musí být jedinečný a konstantní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="61901-155">`"case"` specifies hello case label, which hello switch expression uses for comparison, and must be a constant and unique value.</span></span> <span data-ttu-id="61901-156">Pokud žádný hello případů neodpovídá hello přepínač výrazu akce v rámci `"default"` provedení.</span><span class="sxs-lookup"><span data-stu-id="61901-156">If none of hello cases match hello switch expression, actions under `"default"` are executed.</span></span>

## <a name="get-help"></a><span data-ttu-id="61901-157">Podpora</span><span class="sxs-lookup"><span data-stu-id="61901-157">Get help</span></span>

<span data-ttu-id="61901-158">tooask otázky, odpovědi na dotazy a zjistit, jaké jiných uživatelů Azure Logic Apps dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="61901-158">tooask questions, answer questions, and see what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="61901-159">toohelp zlepšení Azure Logic Apps a konektory, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Azure Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="61901-159">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="61901-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61901-160">Next steps</span></span>

- <span data-ttu-id="61901-161">Zjistěte, jak příliš[přidat podmínky](logic-apps-use-logic-app-features.md)</span><span class="sxs-lookup"><span data-stu-id="61901-161">Learn how too[add conditions](logic-apps-use-logic-app-features.md)</span></span>
- <span data-ttu-id="61901-162">Další informace o [chyb a výjimek](logic-apps-exception-handling.md)</span><span class="sxs-lookup"><span data-stu-id="61901-162">Learn about [error and exception handling](logic-apps-exception-handling.md)</span></span>
- <span data-ttu-id="61901-163">Prozkoumat více [možnosti jazyka pracovního postupu](logic-apps-author-definitions.md)</span><span class="sxs-lookup"><span data-stu-id="61901-163">Explore more [workflow language capabilities](logic-apps-author-definitions.md)</span></span>