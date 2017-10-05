---
title: "Automatizovat procesy analýzy protokolů Azure s Flow Microsoft"
description: "Zjistěte, jak Microsoft Flow můžete rychle automatizovat opakované procesy pomocí konektoru Azure Log Analytics."
services: log-analytics
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: log-analytics
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: bwren
ms.openlocfilehash: 4955f90de06cd720d78c5bb60c0adcd7dc633245
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="automate-log-analytics-processes-with-the-connector-for-microsoft-flow"></a><span data-ttu-id="d0a05-103">Automatizovat procesy analýzy protokolů pomocí konektoru pro Flow Microsoft</span><span class="sxs-lookup"><span data-stu-id="d0a05-103">Automate Log Analytics processes with the connector for Microsoft Flow</span></span>
<span data-ttu-id="d0a05-104">[Microsoft Flow](https://ms.flow.microsoft.com) vám umožní vytvořit automatizované pracovní postupy pomocí stovky akce pro celou řadu služeb.</span><span class="sxs-lookup"><span data-stu-id="d0a05-104">[Microsoft Flow](https://ms.flow.microsoft.com) allows you to create automated workflows using hundreds of actions for a variety of services.</span></span> <span data-ttu-id="d0a05-105">Výstup z jednu akci je možné použít jako vstup pro jiné umožňuje vytvářet integrace mezi různými službami.</span><span class="sxs-lookup"><span data-stu-id="d0a05-105">Output from one action can be used as input to another allowing you to create integration between different services.</span></span>  <span data-ttu-id="d0a05-106">Konektor analýzy protokolů Azure pro Microsoft Flow umožňují vytvářet pracovní postupy, které zahrnují data načtená při prohledávání protokolu v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="d0a05-106">The Azure Log Analytics connector for Microsoft Flow allow you to build workflows that include data retrieved by log searches in Log Analytics.</span></span>

<span data-ttu-id="d0a05-107">Například můžete použít Microsoft Flow používat data analýzy protokolů e-mailem oznámení z Office 365, vytvoření chyby ve Visual Studio Team Services, nebo příspěvek ve Slack.</span><span class="sxs-lookup"><span data-stu-id="d0a05-107">For example, you can use Microsoft Flow to use Log Analytics data in an email notification from Office 365, create a bug in Visual Studio Team Services, or post a Slack message.</span></span>  <span data-ttu-id="d0a05-108">Při jednoduchého plánu, nebo některá z akcí v připojené služby, jako je při doručení e-mailu nebo tweet, můžete aktivovat pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="d0a05-108">You can trigger a workflow by a simple schedule or from some action in a connected service such as when a mail or a tweet is received.</span></span>  


> [!NOTE]
> <span data-ttu-id="d0a05-109">Konektor analýzy protokolů Azure pro Microsoft Flow vyžaduje pracovního prostoru budou upgradovány na novou dotazovacího jazyka pro analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="d0a05-109">The Azure Log Analytics connector for Microsoft Flow requires your workspace to be upgraded to the new Log Analytics query language.</span></span> <span data-ttu-id="d0a05-110">Můžete získat další informace o nový jazyk a získat postup upgradu pracovního prostoru v [upgradu pracovní prostor analýzy protokolů Azure na nové hledání protokolu](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="d0a05-110">You can learn more about the new language and get the procedure to upgrade your workspace at [Upgrade your Azure Log Analytics workspace to new log search](log-analytics-log-search-upgrade.md).</span></span>  

<span data-ttu-id="d0a05-111">Tento kurz v tomto článku se dozvíte, jak vytvořit toku, který automaticky odesílá výsledky vyhledávání protokolu analýzy protokolů e-mailem, pouze příklad, jak můžete použít analýzy protokolů v Flow Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d0a05-111">The tutorial in this article shows you how to create a flow that automatically sends the results of a Log Analytics log search by email, just one example of how you can use Log Analytics in Microsoft Flow.</span></span> 


## <a name="step-1-create-a-flow"></a><span data-ttu-id="d0a05-112">Krok 1: Vytvoření toku</span><span class="sxs-lookup"><span data-stu-id="d0a05-112">Step 1: Create a flow</span></span>
1. <span data-ttu-id="d0a05-113">Přihlaste se k [Microsoft Flow](http://flow.microsoft.com)a vyberte **Moje toků**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-113">Sign in to [Microsoft Flow](http://flow.microsoft.com), and select **My Flows**.</span></span>
2. <span data-ttu-id="d0a05-114">Klikněte na tlačítko **+ vytvořit z prázdné**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-114">Click **+ Create from blank**.</span></span>

## <a name="step-2-create-a-trigger-for-your-flow"></a><span data-ttu-id="d0a05-115">Krok 2: Vytvoření aktivační událost pro vaše tok</span><span class="sxs-lookup"><span data-stu-id="d0a05-115">Step 2: Create a trigger for your flow</span></span>
1. <span data-ttu-id="d0a05-116">Klikněte na tlačítko **vyhledávání stovky konektory a aktivační události**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-116">Click **Search hundreds of connectors and triggers**.</span></span>
2. <span data-ttu-id="d0a05-117">Typ **plán** do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="d0a05-117">Type **Schedule** in the search box.</span></span>
3. <span data-ttu-id="d0a05-118">Vyberte **plán**a potom vyberte **plán - opakování**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-118">Select **Schedule**, and then select **Schedule - Recurrence**.</span></span>
4. <span data-ttu-id="d0a05-119">V **frekvence** zaškrtněte **den** a v **Interval** zadejte **1**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-119">In the **Frequency** box select **Day** and in the **Interval** box, enter **1**.</span></span><br><br><span data-ttu-id="d0a05-120">![Dialogové okno Microsoft Flow aktivační události](media/log-analytics-flow-tutorial/flow01.png)</span><span class="sxs-lookup"><span data-stu-id="d0a05-120">![Microsoft Flow trigger dialog box](media/log-analytics-flow-tutorial/flow01.png)</span></span>


## <a name="step-3-add-a-log-analytics-action"></a><span data-ttu-id="d0a05-121">Krok 3: Přidání akce analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="d0a05-121">Step 3: Add a Log Analytics action</span></span>
1. <span data-ttu-id="d0a05-122">Klikněte na tlačítko **+ nový krok**a potom klikněte na **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-122">Click **+ New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="d0a05-123">Vyhledejte **protokolu analýzy**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-123">Search for **Log Analytics**.</span></span>
3. <span data-ttu-id="d0a05-124">Klikněte na tlačítko **Azure Log Analytics – spuštění dotazu a vizualizace výsledků**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-124">Click **Azure Log Analytics – Run query and visualize results**.</span></span><br><br><span data-ttu-id="d0a05-125">![Spusťte okno dotazu analýzy protokolů](media/log-analytics-flow-tutorial/flow02.png)</span><span class="sxs-lookup"><span data-stu-id="d0a05-125">![Log Analytics run query window](media/log-analytics-flow-tutorial/flow02.png)</span></span>

## <a name="step-4-configure-the-log-analytics-action"></a><span data-ttu-id="d0a05-126">Krok 4: Konfigurace analýzy protokolů akce</span><span class="sxs-lookup"><span data-stu-id="d0a05-126">Step 4: Configure the Log Analytics action</span></span>

1. <span data-ttu-id="d0a05-127">Zadejte podrobnosti pro pracovní prostor, včetně ID předplatné, skupinu prostředků a název pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="d0a05-127">Specify the details for your workspace including the Subscription ID, Resource Group, and Workspace Name.</span></span>
2. <span data-ttu-id="d0a05-128">Přidejte následující dotaz analýzy protokolů, který **dotazu** okno.</span><span class="sxs-lookup"><span data-stu-id="d0a05-128">Add the following Log Analytics query to the **Query** window.</span></span>  <span data-ttu-id="d0a05-129">Toto je ukázkový dotaz a můžete nahradit všechny jiné, vrací data.</span><span class="sxs-lookup"><span data-stu-id="d0a05-129">This is only a sample query, and you can replace with any other that returns data.</span></span>
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computerindow. 
```

2. <span data-ttu-id="d0a05-130">Vyberte **tabulky HTML** pro **typ grafu**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-130">Select **HTML Table** for the **Chart Type**.</span></span><br><br><span data-ttu-id="d0a05-131">![Akce analýzy protokolů](media/log-analytics-flow-tutorial/flow03.png)</span><span class="sxs-lookup"><span data-stu-id="d0a05-131">![Log Analytics action](media/log-analytics-flow-tutorial/flow03.png)</span></span>

## <a name="step-5-configure-the-flow-to-send-email"></a><span data-ttu-id="d0a05-132">Krok 5: Konfigurace toku k odeslání e-mailu</span><span class="sxs-lookup"><span data-stu-id="d0a05-132">Step 5: Configure the flow to send email</span></span>

1. <span data-ttu-id="d0a05-133">Klikněte na tlačítko **nový krok**a potom klikněte na **+ přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-133">Click **New step**, and then click **+ Add an action**.</span></span>
2. <span data-ttu-id="d0a05-134">Vyhledejte **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-134">Search for **Office 365 Outlook**.</span></span>
3. <span data-ttu-id="d0a05-135">Klikněte na tlačítko **Office 365 Outlook – e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-135">Click **Office 365 Outlook – Send an email**.</span></span><br><br><span data-ttu-id="d0a05-136">![Okno Výběr Outlook Office 365](media/log-analytics-flow-tutorial/flow04.png)</span><span class="sxs-lookup"><span data-stu-id="d0a05-136">![Office 365 Outlook selection window](media/log-analytics-flow-tutorial/flow04.png)</span></span>

4. <span data-ttu-id="d0a05-137">Zadejte e-mailová adresa příjemce v **k** okno a předmět e-mailu v **subjektu**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-137">Specify the email address of a recipient in the **To** window and a subject for the email in **Subject**.</span></span>
5. <span data-ttu-id="d0a05-138">Klikněte kamkoli do **textu** pole.</span><span class="sxs-lookup"><span data-stu-id="d0a05-138">Click anywhere in the **Body** box.</span></span>  <span data-ttu-id="d0a05-139">A **dynamický obsah** otevře se okno s hodnotami z předchozí akce.</span><span class="sxs-lookup"><span data-stu-id="d0a05-139">A **Dynamic content** window opens with values from previous actions.</span></span>  
6. <span data-ttu-id="d0a05-140">Vyberte **textu**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-140">Select **Body**.</span></span>  <span data-ttu-id="d0a05-141">Toto je výsledky dotazu v akci pro analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="d0a05-141">This is the results of the query in the Log Analytics action.</span></span>
6. <span data-ttu-id="d0a05-142">Klikněte na tlačítko **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-142">Click **Show advanced options**.</span></span>
7. <span data-ttu-id="d0a05-143">V **je HTML** vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-143">In the **Is HTML** box, select **Yes**.</span></span><br><br><span data-ttu-id="d0a05-144">![Okno Konfigurace e-mailu Office 365](media/log-analytics-flow-tutorial/flow05.png)</span><span class="sxs-lookup"><span data-stu-id="d0a05-144">![Office 365 email configuration window](media/log-analytics-flow-tutorial/flow05.png)</span></span>

## <a name="step-6-save-and-test-your-flow"></a><span data-ttu-id="d0a05-145">Krok 6: Uložit a testování vaší toku</span><span class="sxs-lookup"><span data-stu-id="d0a05-145">Step 6: Save and test your flow</span></span>
1. <span data-ttu-id="d0a05-146">V **toku název** pole, přidejte název vaší toku a pak klikněte na tlačítko **vytvořit toku**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-146">In the **Flow name** box, add a name for your flow, and then click **Create flow**.</span></span><br><br><span data-ttu-id="d0a05-147">![Uložit toku](media/log-analytics-flow-tutorial/flow06.png)</span><span class="sxs-lookup"><span data-stu-id="d0a05-147">![Save flow](media/log-analytics-flow-tutorial/flow06.png)</span></span>
2. <span data-ttu-id="d0a05-148">Tok teď se vytvoří a spustí za den, což je plán, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="d0a05-148">The flow is now created and will run after a day which is the schedule you specified.</span></span> 
3. <span data-ttu-id="d0a05-149">Chcete-li hned otestovat toku, klikněte na tlačítko **spustit nyní** a potom **spustit toku**.</span><span class="sxs-lookup"><span data-stu-id="d0a05-149">To immediately test the flow, click **Run Now** and then **Run flow**.</span></span><br><br><span data-ttu-id="d0a05-150">![Spustit toku](media/log-analytics-flow-tutorial/flow07.png)</span><span class="sxs-lookup"><span data-stu-id="d0a05-150">![Run flow](media/log-analytics-flow-tutorial/flow07.png)</span></span>
3. <span data-ttu-id="d0a05-151">Po dokončení toku zkontrolujte e-mailu příjemce, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="d0a05-151">When the flow completes, check the mail of the recipient that you specified.</span></span>  <span data-ttu-id="d0a05-152">Byste měli obdržet e-mail se text podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="d0a05-152">You should have received a mail with a body similar to the following:</span></span><br><br>![Ukázkového e-mailu](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a><span data-ttu-id="d0a05-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0a05-154">Next steps</span></span>

- <span data-ttu-id="d0a05-155">Další informace o [přihlásit analýzy protokolů hledání](log-analytics-log-search-new.md).</span><span class="sxs-lookup"><span data-stu-id="d0a05-155">Learn more about [log searches in Log Analytics](log-analytics-log-search-new.md).</span></span>
- <span data-ttu-id="d0a05-156">Další informace o [Microsoft Flow](https://ms.flow.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="d0a05-156">Learn more about [Microsoft Flow](https://ms.flow.microsoft.com).</span></span>



