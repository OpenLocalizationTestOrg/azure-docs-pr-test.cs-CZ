---
title: "Diagnostikování selhání - Azure Logic Apps | Microsoft Docs"
description: "Běžné způsoby, abyste pochopili, kde dochází k selhání aplikace logiky"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 814e6f93088cdd96b0a663d2a7494b5a11470d99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="diagnose-logic-app-failures"></a><span data-ttu-id="31869-103">Diagnostikování selhání aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="31869-103">Diagnose logic app failures</span></span>
<span data-ttu-id="31869-104">Pokud máte selhání nebo problémy s logic apps, existuje několik přístupů vám může pomoct lépe pochopit, kde jsou chyby pocházejících z.</span><span class="sxs-lookup"><span data-stu-id="31869-104">If you experience issues or failures with your logic apps, there are a few approaches can help you better understand where the failures are coming from.</span></span>  

## <a name="azure-portal-tools"></a><span data-ttu-id="31869-105">Nástroje portálu Azure</span><span class="sxs-lookup"><span data-stu-id="31869-105">Azure portal tools</span></span>
<span data-ttu-id="31869-106">Portál Azure poskytuje celou řadu nástrojů pro diagnostiku každé logiku aplikace při každém kroku.</span><span class="sxs-lookup"><span data-stu-id="31869-106">The Azure portal provides many tools to diagnose each logic app at each step.</span></span>

### <a name="trigger-history"></a><span data-ttu-id="31869-107">Aktivační událost historie</span><span class="sxs-lookup"><span data-stu-id="31869-107">Trigger history</span></span>

<span data-ttu-id="31869-108">Každá aplikace logiky má nejméně jedna aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="31869-108">Each logic app has at least one trigger.</span></span> <span data-ttu-id="31869-109">Pokud si všimnete, že aplikace se ohlásí, nejprve vyhledává v historii aktivační událost pro další informace.</span><span class="sxs-lookup"><span data-stu-id="31869-109">If you notice that apps aren't firing, look first at the trigger history for more information.</span></span> <span data-ttu-id="31869-110">Historie aktivační události v hlavním okně app'ss logiky můžete přistupovat.</span><span class="sxs-lookup"><span data-stu-id="31869-110">You can access the trigger history on the logic app'ss main blade.</span></span>

![Vyhledání historie aktivační události][1]

<span data-ttu-id="31869-112">Historie aktivační události obsahuje seznam všech pokusů o aktivační události, které aplikace logiky udělat.</span><span class="sxs-lookup"><span data-stu-id="31869-112">The trigger history lists all trigger attempts that your logic app made.</span></span> <span data-ttu-id="31869-113">Můžete kliknout na jednotlivé pokusy o aktivační události přejít na podrobnosti, konkrétně žádný vstupů nebo výstupů, které generuje pokus o aktivační události.</span><span class="sxs-lookup"><span data-stu-id="31869-113">You can click each trigger attempt to drill into the details, specifically, any inputs or outputs that the trigger attempt generated.</span></span> <span data-ttu-id="31869-114">Pokud zjistíte, se nezdařila aktivace, vyberte pokus o aktivační události a zvolte **výstupy** odkaz zobrazíte všechny vygenerované chybové zprávy, například přihlašovací údaje serveru FTP, které nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="31869-114">If you find failed triggers, select the trigger attempt and choose the **Outputs** link to review any generated error messages, for example, for FTP credentials that aren't valid.</span></span>

<span data-ttu-id="31869-115">Různé stavy, mohou být zobrazeny jsou:</span><span class="sxs-lookup"><span data-stu-id="31869-115">The different statuses you might see are:</span></span>

* <span data-ttu-id="31869-116">**Vynecháno**.</span><span class="sxs-lookup"><span data-stu-id="31869-116">**Skipped**.</span></span> <span data-ttu-id="31869-117">Koncový bod byl dotazování vyhledávat data a obdržel odpověď, nebyla dostupná žádná data.</span><span class="sxs-lookup"><span data-stu-id="31869-117">The endpoint was polled to check for data and received a response that no data was available.</span></span>
* <span data-ttu-id="31869-118">**Úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="31869-118">**Succeeded**.</span></span> <span data-ttu-id="31869-119">Aktivační událost obdržel odpověď, nebyla dostupná data.</span><span class="sxs-lookup"><span data-stu-id="31869-119">The trigger received a response that data was available.</span></span> <span data-ttu-id="31869-120">Tento stav může být výsledkem ruční aktivační událost, aktivační událost opakování nebo cyklického dotazování aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="31869-120">This status might result from a manual trigger, a recurrence trigger, or a polling trigger.</span></span> <span data-ttu-id="31869-121">Tento stav je obvykle přiložena **Fired** stav, ale možná, pokud máte v zobrazení kódu, která nebyla splněná podmínka nebo SplitOn příkaz.</span><span class="sxs-lookup"><span data-stu-id="31869-121">This status is usually accompanied by the **Fired** status, but might not be if you have a condition or SplitOn command in code view that wasn't satisfied.</span></span>
* <span data-ttu-id="31869-122">**Se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="31869-122">**Failed**.</span></span> <span data-ttu-id="31869-123">Byla generována chyba.</span><span class="sxs-lookup"><span data-stu-id="31869-123">An error was generated.</span></span>

#### <a name="start-a-trigger-manually"></a><span data-ttu-id="31869-124">Ruční spuštění aktivační události</span><span class="sxs-lookup"><span data-stu-id="31869-124">Start a trigger manually</span></span>

<span data-ttu-id="31869-125">Pokud chcete aplikaci logiky zkontrolujte aktivační procedury k dispozici okamžitě bez čekání na další opakování, klikněte na tlačítko **vyberte aktivační událost** v hlavním okně vynuťte kontrolu.</span><span class="sxs-lookup"><span data-stu-id="31869-125">If you want the logic app to check for an available trigger immediately without waiting for the next recurrence, click **Select Trigger** on the main blade to force a check.</span></span> <span data-ttu-id="31869-126">Kliknutím na tento odkaz se aktivační událostí Dropbox například pracovní postup k okamžitému dotázání Dropbox pro nové soubory.</span><span class="sxs-lookup"><span data-stu-id="31869-126">For example, clicking this link with a Dropbox trigger causes the workflow to immediately poll Dropbox for new files.</span></span>

### <a name="run-history"></a><span data-ttu-id="31869-127">Historie spouštění</span><span class="sxs-lookup"><span data-stu-id="31869-127">Run history</span></span>

<span data-ttu-id="31869-128">Všechny aktivní aktivační události výsledkem spustit.</span><span class="sxs-lookup"><span data-stu-id="31869-128">Every fired trigger results in a run.</span></span> <span data-ttu-id="31869-129">Informace o běhu můžete přistupovat z hlavní okno, který obsahuje mnoho podrobnosti, které vám může pomoct pochopit, co se stalo během pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="31869-129">You can access run information from the main blade, which contains many details that can help you understand what happened during the workflow.</span></span>

![Vyhledání historie spouštění][2]

<span data-ttu-id="31869-131">Spustit zobrazí jedno z následujících stavů:</span><span class="sxs-lookup"><span data-stu-id="31869-131">A run displays one of the following statuses:</span></span>

* <span data-ttu-id="31869-132">**Úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="31869-132">**Succeeded**.</span></span> <span data-ttu-id="31869-133">Všechny akce bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="31869-133">All actions succeeded.</span></span> <span data-ttu-id="31869-134">Pokud došlo k selhání, že selhání zajišťoval akci, která došlo k chybě později v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="31869-134">If a failure happened, that failure was handled by an action that occurred later in the workflow.</span></span> <span data-ttu-id="31869-135">To znamená selhání zajišťoval akci, která byla nastavena pro spuštění po selhání akce.</span><span class="sxs-lookup"><span data-stu-id="31869-135">That is, the failure was handled by an action that was set to run after a failed action.</span></span>
* <span data-ttu-id="31869-136">**Se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="31869-136">**Failed**.</span></span> <span data-ttu-id="31869-137">Nejméně jedna akce měl selhání, který nebyl zpracován akcí později v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="31869-137">At least one action had a failure that was not handled by an action later in the workflow.</span></span>
* <span data-ttu-id="31869-138">**Zrušena**.</span><span class="sxs-lookup"><span data-stu-id="31869-138">**Cancelled**.</span></span> <span data-ttu-id="31869-139">Pracovní postup byl spuštěn, ale přijal žádost o zrušení.</span><span class="sxs-lookup"><span data-stu-id="31869-139">The workflow was running but received a cancel request.</span></span>
* <span data-ttu-id="31869-140">**Spuštění**.</span><span class="sxs-lookup"><span data-stu-id="31869-140">**Running**.</span></span> <span data-ttu-id="31869-141">Pracovní postup je právě spuštěna.</span><span class="sxs-lookup"><span data-stu-id="31869-141">The workflow is currently running.</span></span> <span data-ttu-id="31869-142">Tento stav může dojít k omezenému pracovních postupů, nebo z důvodu aktuálního cenový plán.</span><span class="sxs-lookup"><span data-stu-id="31869-142">This status might occur for throttled workflows, or because of the current pricing plan.</span></span> <span data-ttu-id="31869-143">Podrobnosti najdete v tématu [akce omezení na stránce s cenami](https://azure.microsoft.com/pricing/details/app-service/plans/).</span><span class="sxs-lookup"><span data-stu-id="31869-143">For details, see [action limits on the pricing page](https://azure.microsoft.com/pricing/details/app-service/plans/).</span></span> <span data-ttu-id="31869-144">Konfigurace diagnostiky (grafů, které se zobrazují v části historie spouštění) také může poskytnout informace o všech omezení událostí, které dojít.</span><span class="sxs-lookup"><span data-stu-id="31869-144">Configuring diagnostics (the charts that appear under the run history) also can provide information about any throttle events that happen.</span></span>

<span data-ttu-id="31869-145">Když se díváte historie spouštění, můžete zobrazit další podrobnosti Další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="31869-145">When you are looking at a run history, you can drill in for more details.</span></span>  

#### <a name="trigger-outputs"></a><span data-ttu-id="31869-146">Výstupy aktivační události</span><span class="sxs-lookup"><span data-stu-id="31869-146">Trigger outputs</span></span>

<span data-ttu-id="31869-147">Aktivační událost výstupy zobrazit data, která pochází aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="31869-147">Trigger outputs show the data that came from the trigger.</span></span> <span data-ttu-id="31869-148">Tyto výstupů vám pomohou určit, zda všechny vlastnosti vrátila podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="31869-148">These outputs can help you determine whether all properties returned as expected.</span></span>

> [!NOTE]
> <span data-ttu-id="31869-149">Pokud se zobrazí veškerý obsah, který neznáte, zjistěte, jak Azure Logic Apps [zpracovává jiné typy obsahu](../logic-apps/logic-apps-content-type.md).</span><span class="sxs-lookup"><span data-stu-id="31869-149">If you see any content that you don't understand, learn how Azure Logic Apps [handles different content types](../logic-apps/logic-apps-content-type.md).</span></span>
> 

![Příklady výstup aktivační události][3]

#### <a name="action-inputs-and-outputs"></a><span data-ttu-id="31869-151">Akce vstupy a výstupy</span><span class="sxs-lookup"><span data-stu-id="31869-151">Action inputs and outputs</span></span>

<span data-ttu-id="31869-152">Můžete rozbalit vstupy a výstupy, které přijaly akce.</span><span class="sxs-lookup"><span data-stu-id="31869-152">You can drill into the inputs and outputs that an action received.</span></span> <span data-ttu-id="31869-153">Tato data jsou užitečné pro pochopení velikost a tvar výstupy a také pro hledání chybové zprávy, které byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="31869-153">This data is useful for understanding the size and shape of the outputs, and also for finding any error messages that might have been generated.</span></span>

![Akce vstupy a výstupy][4]

## <a name="debug-workflow-runtime"></a><span data-ttu-id="31869-155">Ladění modulu runtime pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="31869-155">Debug workflow runtime</span></span>

<span data-ttu-id="31869-156">Společně s monitorování vstupy, výstupy a aktivuje spuštění, můžete přidat do pracovního postupu, které pomáhají s laděním některé kroky.</span><span class="sxs-lookup"><span data-stu-id="31869-156">Along with monitoring the inputs, outputs, and triggers of a run, you could add some steps to a workflow that help with debugging.</span></span> 
<span data-ttu-id="31869-157">[RequestBin](http://requestb.in) je výkonný nástroj, který můžete přidat jako krok v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="31869-157">[RequestBin](http://requestb.in) is a powerful tool that you can add as a step in a workflow.</span></span> <span data-ttu-id="31869-158">Pomocí RequestBin můžete nastavit kontrolor požadavku HTTP určit přesnou velikost, tvar a formát požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="31869-158">By using RequestBin, you can set up an HTTP request inspector to determine the exact size, shape, and format of an HTTP request.</span></span> <span data-ttu-id="31869-159">Můžete vytvořit RequestBin a vložte adresu URL aplikace logiky akce HTTP POST s obsah textu, který chcete otestovat, například, výraz nebo jiné krok výstup.</span><span class="sxs-lookup"><span data-stu-id="31869-159">You can create a RequestBin and paste the URL in a logic app HTTP POST action with body content that you want to test, for example, an expression or another step output.</span></span> <span data-ttu-id="31869-160">Po spuštění aplikace logiky můžete obnovit vaše RequestBin zobrazíte jak žádosti byl vytvořen při vygenerování z modulu Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="31869-160">After you run the logic app, you can refresh your RequestBin to see how the request was formed when generated from the Logic Apps engine.</span></span>

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
