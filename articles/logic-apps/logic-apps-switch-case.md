---
title: "Switch – příkaz pro různé akce v Azure Logic Apps | Microsoft Docs"
description: "Zvolte jiné akce lze provádět v logiku aplikace založené na výrazu hodnoty pomocí příkazu switch"
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
ms.openlocfilehash: 338b6a5b549d7bf81186550295608438ac4aee32
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a><span data-ttu-id="62814-104">Provádění různých akcí v aplikacích logiky s příkaz switch</span><span class="sxs-lookup"><span data-stu-id="62814-104">Perform different actions in logic apps with a switch statement</span></span>

<span data-ttu-id="62814-105">Při vytváření pracovního postupu, je často nutné provést různé akce na základě hodnoty objektu nebo výraz.</span><span class="sxs-lookup"><span data-stu-id="62814-105">When authoring a workflow, you often have to take different actions based on the value of an object or expression.</span></span> <span data-ttu-id="62814-106">Například můžete svou aplikaci logiky chovat různě v závislosti na kód stavu požadavku HTTP, nebo možnosti vybrané v e-mailu.</span><span class="sxs-lookup"><span data-stu-id="62814-106">For example, you might want your logic app to behave differently based on the status code of an HTTP request, or an option selected in an email.</span></span>

<span data-ttu-id="62814-107">Příkaz přepínač můžete použít k implementaci těchto scénářů.</span><span class="sxs-lookup"><span data-stu-id="62814-107">You can use a switch statement to implement these scenarios.</span></span> <span data-ttu-id="62814-108">Aplikace logiky můžete vyhodnotit token nebo výraz a zvolte případ se stejnou hodnotou provést zadané akce.</span><span class="sxs-lookup"><span data-stu-id="62814-108">Your logic app can evaluate a token or expression, and choose the case with the same value to execute the specified actions.</span></span> <span data-ttu-id="62814-109">Příkaz switch by měl odpovídat pouze jeden případ.</span><span class="sxs-lookup"><span data-stu-id="62814-109">Only one case should match the switch statement.</span></span>

> [!TIP]
> <span data-ttu-id="62814-110">Podobně jako všechny programovacích jazyků příkazech switch podporují pouze operátory rovnosti.</span><span class="sxs-lookup"><span data-stu-id="62814-110">Like all programming languages, switch statements support only equality operators.</span></span> <span data-ttu-id="62814-111">Pokud potřebujete další relační operátory, jako je třeba "větší než", použijte příkaz podmínky.</span><span class="sxs-lookup"><span data-stu-id="62814-111">If you need other relational operators, such as "greater than", use a condition statement.</span></span>
> <span data-ttu-id="62814-112">Aby chování při spuštění deterministický, případech musí obsahovat jedinečný a statické hodnotu namísto dynamického tokeny nebo výraz.</span><span class="sxs-lookup"><span data-stu-id="62814-112">To ensure deterministic execution behavior, cases must contain a unique and static value instead of dynamic tokens or expression.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62814-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="62814-113">Prerequisites</span></span>

- <span data-ttu-id="62814-114">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="62814-114">An active Azure subscription.</span></span> <span data-ttu-id="62814-115">Pokud nemáte předplatné Azure aktivní [vytvořit bezplatný účet](https://azure.microsoft.com/free/), nebo zkuste [volné aplikace logiky pro](https://tryappservice.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="62814-115">If you don't have an active Azure subscription, [create a free account](https://azure.microsoft.com/free/), or try [Logic Apps for free](https://tryappservice.azure.com/).</span></span>
- [<span data-ttu-id="62814-116">Základní znalosti o aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="62814-116">Basic knowledge about logic apps</span></span>](logic-apps-what-are-logic-apps.md)

## <a name="add-a-switch-statement-to-your-workflow"></a><span data-ttu-id="62814-117">Do pracovního postupu přidat příkaz switch</span><span class="sxs-lookup"><span data-stu-id="62814-117">Add a switch statement to your workflow</span></span>

<span data-ttu-id="62814-118">Pokud chcete zobrazit, jak funguje příkazu switch, tento příklad vytvoří aplikace logiky, která monitoruje soubory odesílané do Dropboxu.</span><span class="sxs-lookup"><span data-stu-id="62814-118">To show how a switch statement works, this example creates a logic app that monitors files uploaded to Dropbox.</span></span> <span data-ttu-id="62814-119">Když jsou odeslány nové soubory, aplikace logiky odešle e-mailu schvalovatel, který rozhodne, zda se má přenést těchto souborů do služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="62814-119">When the new files are uploaded, the logic app sends email to an approver who chooses whether to transfer those files to SharePoint.</span></span> <span data-ttu-id="62814-120">Aplikace používá příkaz přepínač, který provádí různé akce na základě hodnoty, která vybere položku schvalovatel.</span><span class="sxs-lookup"><span data-stu-id="62814-120">The app uses a switch statement that performs different actions based on the value that the approver selects.</span></span>

1. <span data-ttu-id="62814-121">Vytvoření aplikace logiky a vyberte této aktivační události: **Dropboxu - Pokud dojde k vytvoření souboru**.</span><span class="sxs-lookup"><span data-stu-id="62814-121">Create a logic app, and select this trigger: **Dropbox - When a file is created**.</span></span>

   ![Použití Dropboxu - Pokud dojde k vytvoření souboru aktivační události](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. <span data-ttu-id="62814-123">V části aktivační událost, přidejte tuto akci: **Outlook.com - odesílání schválení e-mailu**</span><span class="sxs-lookup"><span data-stu-id="62814-123">Under the trigger, add this action: **Outlook.com - Send approval email**</span></span>

   > [!TIP]
   > <span data-ttu-id="62814-124">Aplikace logiky také podporují odesílání e-mailu scénáře schválení z účtu Office 365 Outlook.</span><span class="sxs-lookup"><span data-stu-id="62814-124">Logic apps also support sending approval email scenarios from an Office 365 Outlook account.</span></span>

   - <span data-ttu-id="62814-125">Pokud nemáte existující připojení, se zobrazí výzva k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="62814-125">If you don't have an existing connection, you're prompted to create one.</span></span>
   - <span data-ttu-id="62814-126">Vyplňte požadovaná pole.</span><span class="sxs-lookup"><span data-stu-id="62814-126">Fill in the required fields.</span></span> <span data-ttu-id="62814-127">Například v položce **k**, zadejte e-mailovou adresu pro odesílání e-mailu schvalovatel.</span><span class="sxs-lookup"><span data-stu-id="62814-127">For example, under **To**, specify the email address for sending the approver email.</span></span>
   - <span data-ttu-id="62814-128">V části **možností uživatele**, zadejte `Approve, Reject`.</span><span class="sxs-lookup"><span data-stu-id="62814-128">Under **User Options**, enter `Approve, Reject`.</span></span>

   ![Konfigurace připojení](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. <span data-ttu-id="62814-130">Přidejte příkaz, přepínač.</span><span class="sxs-lookup"><span data-stu-id="62814-130">Add a switch statement.</span></span>

   - <span data-ttu-id="62814-131">Vyberte **+ nový krok** > **... Další** > **přidání přepínače případu**.</span><span class="sxs-lookup"><span data-stu-id="62814-131">Select **+ New step** > **... More** > **Add a switch case**.</span></span> 
   - <span data-ttu-id="62814-132">Teď chceme vyberte akce k provedení na základě `SelectedOptions` výstup z *odeslání e-mailu schválení* akce.</span><span class="sxs-lookup"><span data-stu-id="62814-132">Now we want to select the action to perform based on the `SelectedOptions` output from the *Send approval email* action.</span></span> 
   <span data-ttu-id="62814-133">Můžete najít v tomto poli **přidávat dynamický obsah** selektor.</span><span class="sxs-lookup"><span data-stu-id="62814-133">You can find this field in the **Add dynamic content** selector.</span></span>
   - <span data-ttu-id="62814-134">Použití *případ 1* zpracování, pokud se vybere schvalovatel `Approve`.</span><span class="sxs-lookup"><span data-stu-id="62814-134">Use *Case 1* to handle when the approver selects `Approve`.</span></span>
     - <span data-ttu-id="62814-135">Pokud, zkopírujte původní soubor k SharePoint Online pomocí [ **SharePoint Online – vytvoření souboru** akce](../connectors/connectors-create-api-sharepointonline.md).</span><span class="sxs-lookup"><span data-stu-id="62814-135">If approved, copy the original file to SharePoint Online with the [**SharePoint Online - Create file** action](../connectors/connectors-create-api-sharepointonline.md).</span></span>
     - <span data-ttu-id="62814-136">Přidejte další akci v případě uživatelům oznámit, že nový soubor je k dispozici na webu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="62814-136">Add another action within the case to notify users that a new file is available on SharePoint.</span></span>
   - <span data-ttu-id="62814-137">Přidání jiného případu zpracování, pokud uživatel vybere `Reject`.</span><span class="sxs-lookup"><span data-stu-id="62814-137">Add another case to handle when user selects `Reject`.</span></span>
     - <span data-ttu-id="62814-138">Pokud zamítnutí, odesílat e-mailové oznámení, jiné schvalovatelů oznamující, že soubor byl odmítnut a není nutná žádná další akce.</span><span class="sxs-lookup"><span data-stu-id="62814-138">If rejected, send a notification email informing other approvers that the file is rejected and no further action is required.</span></span>
   - <span data-ttu-id="62814-139">`SelectedOptions`poskytuje pouze dvě možnosti, takže jsme můžete nechat **výchozí** případ prázdný.</span><span class="sxs-lookup"><span data-stu-id="62814-139">`SelectedOptions` provides only two options, so we can leave the **Default** case empty.</span></span>

   ![Switch – příkaz](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > <span data-ttu-id="62814-141">Příkaz switch musí mít alespoň jeden případ kromě případu výchozí.</span><span class="sxs-lookup"><span data-stu-id="62814-141">A switch statement needs at least one case in addition to the default case.</span></span>

4. <span data-ttu-id="62814-142">Po příkazu switch odstranit původní soubor nahrán do Dropboxu přidáním tuto akci: **Dropbox – odstranit soubor**</span><span class="sxs-lookup"><span data-stu-id="62814-142">After the switch statement, delete the original file uploaded to Dropbox by adding this action: **Dropbox - Delete file**</span></span>

5. <span data-ttu-id="62814-143">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="62814-143">Save your logic app.</span></span> <span data-ttu-id="62814-144">Testování aplikace s tím, že nahrajete soubor na Dropbox.</span><span class="sxs-lookup"><span data-stu-id="62814-144">Test your app by uploading a file to Dropbox.</span></span> <span data-ttu-id="62814-145">Měli byste obdržet e-mailu schválení za chvíli.</span><span class="sxs-lookup"><span data-stu-id="62814-145">You should receive an approval email shortly.</span></span> <span data-ttu-id="62814-146">Vyberte možnost a sledovat chování.</span><span class="sxs-lookup"><span data-stu-id="62814-146">Select an option, and observe the behavior.</span></span>

   > [!TIP]
   > <span data-ttu-id="62814-147">Podívejte se na postup [sledování funkce logic apps](logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="62814-147">Check out how to [monitor your logic apps](logic-apps-monitor-your-logic-apps.md).</span></span>

## <a name="understand-the-code-behind-switch-statements"></a><span data-ttu-id="62814-148">Pochopení kódu příkazech switch</span><span class="sxs-lookup"><span data-stu-id="62814-148">Understand the code behind switch statements</span></span>

<span data-ttu-id="62814-149">Teď, když jste úspěšně vytvořili aplikaci logiky, pomocí příkazu switch, podíváme se na definici kód za příkazem switch.</span><span class="sxs-lookup"><span data-stu-id="62814-149">Now that you successfully created a logic app using a switch statement, let's look at the code definition behind the switch statement.</span></span>

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

* <span data-ttu-id="62814-150">`"Switch"`představuje název příkazu přepínače, které můžete přejmenovat čitelnější.</span><span class="sxs-lookup"><span data-stu-id="62814-150">`"Switch"` is the name of the switch statement, which you can rename for readability.</span></span> 
* <span data-ttu-id="62814-151">`"type": "Switch"`Označuje, že akce je příkaz switch.</span><span class="sxs-lookup"><span data-stu-id="62814-151">`"type": "Switch"` indicates that the action is a switch statement.</span></span> 
* <span data-ttu-id="62814-152">`"expression"`vyhodnotí pro každý případ deklarovaný později v definici je jeho vybranou možnost v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="62814-152">`"expression"` is the approver's selected option in this example and is evaluated against each case declared later in the definition.</span></span> 
* <span data-ttu-id="62814-153">`"cases"`může obsahovat libovolný počet případů.</span><span class="sxs-lookup"><span data-stu-id="62814-153">`"cases"` can contain any number of cases.</span></span> <span data-ttu-id="62814-154">Pro každý případ `"Case *"` je výchozí název případu, kdy můžete přejmenovat čitelnější.</span><span class="sxs-lookup"><span data-stu-id="62814-154">For each case, `"Case *"` is the default name of the case, which you can rename for readability.</span></span> 
<span data-ttu-id="62814-155">`"case"`Určuje případu popisku, který používá přepínač výraz pro porovnání, a musí být jedinečný a konstantní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="62814-155">`"case"` specifies the case label, which the switch expression uses for comparison, and must be a constant and unique value.</span></span> <span data-ttu-id="62814-156">Pokud žádný z případů odpovídat výrazu přepínače, akce v rámci `"default"` provedení.</span><span class="sxs-lookup"><span data-stu-id="62814-156">If none of the cases match the switch expression, actions under `"default"` are executed.</span></span>

## <a name="get-help"></a><span data-ttu-id="62814-157">Podpora</span><span class="sxs-lookup"><span data-stu-id="62814-157">Get help</span></span>

<span data-ttu-id="62814-158">Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="62814-158">To ask questions, answer questions, and see what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="62814-159">Pokud chcete pomoci při vylepšování Azure Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Azure Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="62814-159">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="62814-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62814-160">Next steps</span></span>

- <span data-ttu-id="62814-161">Zjistěte, jak [přidat podmínky](logic-apps-use-logic-app-features.md)</span><span class="sxs-lookup"><span data-stu-id="62814-161">Learn how to [add conditions](logic-apps-use-logic-app-features.md)</span></span>
- <span data-ttu-id="62814-162">Další informace o [chyb a výjimek](logic-apps-exception-handling.md)</span><span class="sxs-lookup"><span data-stu-id="62814-162">Learn about [error and exception handling](logic-apps-exception-handling.md)</span></span>
- <span data-ttu-id="62814-163">Prozkoumat více [možnosti jazyka pracovního postupu](logic-apps-author-definitions.md)</span><span class="sxs-lookup"><span data-stu-id="62814-163">Explore more [workflow language capabilities](logic-apps-author-definitions.md)</span></span>