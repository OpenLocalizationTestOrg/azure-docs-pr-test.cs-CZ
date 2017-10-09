---
title: "aaaAutomate Azure Log Analytics zpracovává s Flow Microsoft"
description: "Zjistěte, jak lze pomocí Microsoft Flow tooquickly automatizovat opakované procesy pomocí konektoru Azure Log Analytics hello."
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
ms.openlocfilehash: 52026df968682842cc9f8d55f6f9875c5f9c10b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-log-analytics-processes-with-hello-connector-for-microsoft-flow"></a><span data-ttu-id="dda14-103">Automatizovat procesy analýzy protokolů hello konektor pro Flow Microsoft</span><span class="sxs-lookup"><span data-stu-id="dda14-103">Automate Log Analytics processes with hello connector for Microsoft Flow</span></span>
<span data-ttu-id="dda14-104">[Microsoft Flow](https://ms.flow.microsoft.com) vám umožní toocreate automatizované pracovní postupy pomocí stovky akce pro celou řadu služeb.</span><span class="sxs-lookup"><span data-stu-id="dda14-104">[Microsoft Flow](https://ms.flow.microsoft.com) allows you toocreate automated workflows using hundreds of actions for a variety of services.</span></span> <span data-ttu-id="dda14-105">Výstup z jednu akci je možné použít jako vstupní tooanother, což vám toocreate integrace mezi různými službami.</span><span class="sxs-lookup"><span data-stu-id="dda14-105">Output from one action can be used as input tooanother allowing you toocreate integration between different services.</span></span>  <span data-ttu-id="dda14-106">konektor Hello analýzy protokolů Azure pro Microsoft Flow povolit toobuild pracovní postupy, které zahrnují data načtená při prohledávání protokolu v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="dda14-106">hello Azure Log Analytics connector for Microsoft Flow allow you toobuild workflows that include data retrieved by log searches in Log Analytics.</span></span>

<span data-ttu-id="dda14-107">Můžete například použít Microsoft Flow toouse analýzy protokolů data e-mailem oznámení z Office 365, vytvoření chyby ve Visual Studio Team Services nebo příspěvek ve Slack.</span><span class="sxs-lookup"><span data-stu-id="dda14-107">For example, you can use Microsoft Flow toouse Log Analytics data in an email notification from Office 365, create a bug in Visual Studio Team Services, or post a Slack message.</span></span>  <span data-ttu-id="dda14-108">Při jednoduchého plánu, nebo některá z akcí v připojené služby, jako je při doručení e-mailu nebo tweet, můžete aktivovat pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="dda14-108">You can trigger a workflow by a simple schedule or from some action in a connected service such as when a mail or a tweet is received.</span></span>  


> [!NOTE]
> <span data-ttu-id="dda14-109">konektor Hello analýzy protokolů Azure pro Microsoft Flow vyžaduje prostoru toobe upgradovat toohello nové analýzy protokolů dotazu jazyka.</span><span class="sxs-lookup"><span data-stu-id="dda14-109">hello Azure Log Analytics connector for Microsoft Flow requires your workspace toobe upgraded toohello new Log Analytics query language.</span></span> <span data-ttu-id="dda14-110">Můžete získat další informace o nový jazyk hello a získat hello postup tooupgrade pracovního prostoru v [Upgrade vyhledávání protokolu toonew pracovní prostor analýzy protokolů Azure](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="dda14-110">You can learn more about hello new language and get hello procedure tooupgrade your workspace at [Upgrade your Azure Log Analytics workspace toonew log search](log-analytics-log-search-upgrade.md).</span></span>  

<span data-ttu-id="dda14-111">Hello kurzu v tomto článku se dozvíte, jak toocreate toku, který automaticky odesílá hello výsledky vyhledávání protokolu analýzy protokolů e-mailem, pouze příklad, jak můžete použít analýzy protokolů v Flow společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="dda14-111">hello tutorial in this article shows you how toocreate a flow that automatically sends hello results of a Log Analytics log search by email, just one example of how you can use Log Analytics in Microsoft Flow.</span></span> 


## <a name="step-1-create-a-flow"></a><span data-ttu-id="dda14-112">Krok 1: Vytvoření toku</span><span class="sxs-lookup"><span data-stu-id="dda14-112">Step 1: Create a flow</span></span>
1. <span data-ttu-id="dda14-113">Přihlaste se příliš[Microsoft Flow](http://flow.microsoft.com)a vyberte **Moje toků**.</span><span class="sxs-lookup"><span data-stu-id="dda14-113">Sign in too[Microsoft Flow](http://flow.microsoft.com), and select **My Flows**.</span></span>
2. <span data-ttu-id="dda14-114">Klikněte na tlačítko **+ vytvořit z prázdné**.</span><span class="sxs-lookup"><span data-stu-id="dda14-114">Click **+ Create from blank**.</span></span>

## <a name="step-2-create-a-trigger-for-your-flow"></a><span data-ttu-id="dda14-115">Krok 2: Vytvoření aktivační událost pro vaše tok</span><span class="sxs-lookup"><span data-stu-id="dda14-115">Step 2: Create a trigger for your flow</span></span>
1. <span data-ttu-id="dda14-116">Klikněte na tlačítko **vyhledávání stovky konektory a aktivační události**.</span><span class="sxs-lookup"><span data-stu-id="dda14-116">Click **Search hundreds of connectors and triggers**.</span></span>
2. <span data-ttu-id="dda14-117">Typ **plán** hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="dda14-117">Type **Schedule** in hello search box.</span></span>
3. <span data-ttu-id="dda14-118">Vyberte **plán**a potom vyberte **plán - opakování**.</span><span class="sxs-lookup"><span data-stu-id="dda14-118">Select **Schedule**, and then select **Schedule - Recurrence**.</span></span>
4. <span data-ttu-id="dda14-119">V hello **frekvence** zaškrtněte **den** a v hello **Interval** zadejte **1**.</span><span class="sxs-lookup"><span data-stu-id="dda14-119">In hello **Frequency** box select **Day** and in hello **Interval** box, enter **1**.</span></span><br><br><span data-ttu-id="dda14-120">![Dialogové okno Microsoft Flow aktivační události](media/log-analytics-flow-tutorial/flow01.png)</span><span class="sxs-lookup"><span data-stu-id="dda14-120">![Microsoft Flow trigger dialog box](media/log-analytics-flow-tutorial/flow01.png)</span></span>


## <a name="step-3-add-a-log-analytics-action"></a><span data-ttu-id="dda14-121">Krok 3: Přidání akce analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="dda14-121">Step 3: Add a Log Analytics action</span></span>
1. <span data-ttu-id="dda14-122">Klikněte na tlačítko **+ nový krok**a potom klikněte na **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="dda14-122">Click **+ New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="dda14-123">Vyhledejte **protokolu analýzy**.</span><span class="sxs-lookup"><span data-stu-id="dda14-123">Search for **Log Analytics**.</span></span>
3. <span data-ttu-id="dda14-124">Klikněte na tlačítko **Azure Log Analytics – spuštění dotazu a vizualizace výsledků**.</span><span class="sxs-lookup"><span data-stu-id="dda14-124">Click **Azure Log Analytics – Run query and visualize results**.</span></span><br><br><span data-ttu-id="dda14-125">![Spusťte okno dotazu analýzy protokolů](media/log-analytics-flow-tutorial/flow02.png)</span><span class="sxs-lookup"><span data-stu-id="dda14-125">![Log Analytics run query window](media/log-analytics-flow-tutorial/flow02.png)</span></span>

## <a name="step-4-configure-hello-log-analytics-action"></a><span data-ttu-id="dda14-126">Krok 4: Konfigurace analýzy protokolů akce hello</span><span class="sxs-lookup"><span data-stu-id="dda14-126">Step 4: Configure hello Log Analytics action</span></span>

1. <span data-ttu-id="dda14-127">Zadejte podrobnosti hello vašeho pracovního prostoru, včetně hello ID předplatné, skupinu prostředků a název pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="dda14-127">Specify hello details for your workspace including hello Subscription ID, Resource Group, and Workspace Name.</span></span>
2. <span data-ttu-id="dda14-128">Přidejte následující analýzy protokolů dotazu toohello hello **dotazu** okno.</span><span class="sxs-lookup"><span data-stu-id="dda14-128">Add hello following Log Analytics query toohello **Query** window.</span></span>  <span data-ttu-id="dda14-129">Toto je ukázkový dotaz a můžete nahradit všechny jiné, vrací data.</span><span class="sxs-lookup"><span data-stu-id="dda14-129">This is only a sample query, and you can replace with any other that returns data.</span></span>
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computerindow. 
```

2. <span data-ttu-id="dda14-130">Vyberte **tabulky HTML** pro hello **typ grafu**.</span><span class="sxs-lookup"><span data-stu-id="dda14-130">Select **HTML Table** for hello **Chart Type**.</span></span><br><br><span data-ttu-id="dda14-131">![Akce analýzy protokolů](media/log-analytics-flow-tutorial/flow03.png)</span><span class="sxs-lookup"><span data-stu-id="dda14-131">![Log Analytics action](media/log-analytics-flow-tutorial/flow03.png)</span></span>

## <a name="step-5-configure-hello-flow-toosend-email"></a><span data-ttu-id="dda14-132">Krok 5: Konfigurace hello toku toosend e-mailu</span><span class="sxs-lookup"><span data-stu-id="dda14-132">Step 5: Configure hello flow toosend email</span></span>

1. <span data-ttu-id="dda14-133">Klikněte na tlačítko **nový krok**a potom klikněte na **+ přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="dda14-133">Click **New step**, and then click **+ Add an action**.</span></span>
2. <span data-ttu-id="dda14-134">Vyhledejte **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="dda14-134">Search for **Office 365 Outlook**.</span></span>
3. <span data-ttu-id="dda14-135">Klikněte na tlačítko **Office 365 Outlook – e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="dda14-135">Click **Office 365 Outlook – Send an email**.</span></span><br><br><span data-ttu-id="dda14-136">![Okno Výběr Outlook Office 365](media/log-analytics-flow-tutorial/flow04.png)</span><span class="sxs-lookup"><span data-stu-id="dda14-136">![Office 365 Outlook selection window](media/log-analytics-flow-tutorial/flow04.png)</span></span>

4. <span data-ttu-id="dda14-137">Zadejte hello e-mailová adresa příjemce v hello **k** okno a předmět e-mailu hello v **subjektu**.</span><span class="sxs-lookup"><span data-stu-id="dda14-137">Specify hello email address of a recipient in hello **To** window and a subject for hello email in **Subject**.</span></span>
5. <span data-ttu-id="dda14-138">Klikněte kamkoli do hello **textu** pole.</span><span class="sxs-lookup"><span data-stu-id="dda14-138">Click anywhere in hello **Body** box.</span></span>  <span data-ttu-id="dda14-139">A **dynamický obsah** otevře se okno s hodnotami z předchozí akce.</span><span class="sxs-lookup"><span data-stu-id="dda14-139">A **Dynamic content** window opens with values from previous actions.</span></span>  
6. <span data-ttu-id="dda14-140">Vyberte **textu**.</span><span class="sxs-lookup"><span data-stu-id="dda14-140">Select **Body**.</span></span>  <span data-ttu-id="dda14-141">Toto je hello výsledky dotazu hello v hello analýzy protokolů akce.</span><span class="sxs-lookup"><span data-stu-id="dda14-141">This is hello results of hello query in hello Log Analytics action.</span></span>
6. <span data-ttu-id="dda14-142">Klikněte na tlačítko **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="dda14-142">Click **Show advanced options**.</span></span>
7. <span data-ttu-id="dda14-143">V hello **je HTML** vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="dda14-143">In hello **Is HTML** box, select **Yes**.</span></span><br><br><span data-ttu-id="dda14-144">![Okno Konfigurace e-mailu Office 365](media/log-analytics-flow-tutorial/flow05.png)</span><span class="sxs-lookup"><span data-stu-id="dda14-144">![Office 365 email configuration window](media/log-analytics-flow-tutorial/flow05.png)</span></span>

## <a name="step-6-save-and-test-your-flow"></a><span data-ttu-id="dda14-145">Krok 6: Uložit a testování vaší toku</span><span class="sxs-lookup"><span data-stu-id="dda14-145">Step 6: Save and test your flow</span></span>
1. <span data-ttu-id="dda14-146">V hello **toku název** pole, přidejte název vaší toku a pak klikněte na tlačítko **vytvořit toku**.</span><span class="sxs-lookup"><span data-stu-id="dda14-146">In hello **Flow name** box, add a name for your flow, and then click **Create flow**.</span></span><br><br><span data-ttu-id="dda14-147">![Uložit toku](media/log-analytics-flow-tutorial/flow06.png)</span><span class="sxs-lookup"><span data-stu-id="dda14-147">![Save flow](media/log-analytics-flow-tutorial/flow06.png)</span></span>
2. <span data-ttu-id="dda14-148">tok Hello je nyní vytvořen a spustí za den, což je hello plán, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="dda14-148">hello flow is now created and will run after a day which is hello schedule you specified.</span></span> 
3. <span data-ttu-id="dda14-149">tok hello tooimmediately testu, klikněte na tlačítko **spustit nyní** a potom **spustit toku**.</span><span class="sxs-lookup"><span data-stu-id="dda14-149">tooimmediately test hello flow, click **Run Now** and then **Run flow**.</span></span><br><br><span data-ttu-id="dda14-150">![Spustit toku](media/log-analytics-flow-tutorial/flow07.png)</span><span class="sxs-lookup"><span data-stu-id="dda14-150">![Run flow](media/log-analytics-flow-tutorial/flow07.png)</span></span>
3. <span data-ttu-id="dda14-151">Po dokončení hello toku zkontrolujte e-mailu hello hello příjemce, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="dda14-151">When hello flow completes, check hello mail of hello recipient that you specified.</span></span>  <span data-ttu-id="dda14-152">Byste měli obdržet e-mail se podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="dda14-152">You should have received a mail with a body similar toohello following:</span></span><br><br>![Ukázkového e-mailu](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a><span data-ttu-id="dda14-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dda14-154">Next steps</span></span>

- <span data-ttu-id="dda14-155">Další informace o [přihlásit analýzy protokolů hledání](log-analytics-log-search-new.md).</span><span class="sxs-lookup"><span data-stu-id="dda14-155">Learn more about [log searches in Log Analytics](log-analytics-log-search-new.md).</span></span>
- <span data-ttu-id="dda14-156">Další informace o [Microsoft Flow](https://ms.flow.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="dda14-156">Learn more about [Microsoft Flow](https://ms.flow.microsoft.com).</span></span>



