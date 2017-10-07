---
title: chyby aaaDiagnose - Azure Logic Apps | Microsoft Docs
description: "Běžné způsoby toounderstand, kde dochází k selhání aplikace logiky"
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
ms.openlocfilehash: 46d318625820034c95e6df3a71ab84c58f076dd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-logic-app-failures"></a><span data-ttu-id="cb1d4-103">Diagnostikování selhání aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="cb1d4-103">Diagnose logic app failures</span></span>
<span data-ttu-id="cb1d4-104">Pokud máte selhání nebo problémy s logic apps, existuje několik přístupů vám může pomoct lépe pochopit, kde hello selhání pocházejících z.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-104">If you experience issues or failures with your logic apps, there are a few approaches can help you better understand where hello failures are coming from.</span></span>  

## <a name="azure-portal-tools"></a><span data-ttu-id="cb1d4-105">Nástroje portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cb1d4-105">Azure portal tools</span></span>
<span data-ttu-id="cb1d4-106">Hello portál Azure poskytuje mnoho toodiagnose nástroje pro každou aplikaci logiky při každém kroku.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-106">hello Azure portal provides many tools toodiagnose each logic app at each step.</span></span>

### <a name="trigger-history"></a><span data-ttu-id="cb1d4-107">Aktivační událost historie</span><span class="sxs-lookup"><span data-stu-id="cb1d4-107">Trigger history</span></span>

<span data-ttu-id="cb1d4-108">Každá aplikace logiky má nejméně jedna aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-108">Each logic app has at least one trigger.</span></span> <span data-ttu-id="cb1d4-109">Pokud si všimnete, že aplikace se ohlásí, nejprve vyhledává v historii hello aktivační událost pro další informace.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-109">If you notice that apps aren't firing, look first at hello trigger history for more information.</span></span> <span data-ttu-id="cb1d4-110">Historie aktivační událost hello v hlavním okně app'ss hello logiky můžete přistupovat.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-110">You can access hello trigger history on hello logic app'ss main blade.</span></span>

![Vyhledání hello aktivační událost historie][1]

<span data-ttu-id="cb1d4-112">Historie Hello aktivační události obsahuje seznam všech pokusů o aktivační události, které aplikace logiky udělat.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-112">hello trigger history lists all trigger attempts that your logic app made.</span></span> <span data-ttu-id="cb1d4-113">Můžete kliknout na každý pokus o toodrill aktivační události do hello podrobnosti, konkrétně, všechny vstupů nebo výstupů, které hello pokus o aktivační události vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-113">You can click each trigger attempt toodrill into hello details, specifically, any inputs or outputs that hello trigger attempt generated.</span></span> <span data-ttu-id="cb1d4-114">Pokud zjistíte, se nezdařila aktivace, vyberte pokus hello aktivační události a zvolte hello **výstupy** odkaz tooreview všechny vygenerované chybové zprávy, například pro přihlašovací údaje serveru FTP, které nejsou platné.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-114">If you find failed triggers, select hello trigger attempt and choose hello **Outputs** link tooreview any generated error messages, for example, for FTP credentials that aren't valid.</span></span>

<span data-ttu-id="cb1d4-115">Hello různé stavy, které mohou být zobrazeny jsou:</span><span class="sxs-lookup"><span data-stu-id="cb1d4-115">hello different statuses you might see are:</span></span>

* <span data-ttu-id="cb1d4-116">**Vynecháno**.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-116">**Skipped**.</span></span> <span data-ttu-id="cb1d4-117">koncový bod Hello byl dotazů toocheck pro data a obdržel odpověď, nebyla dostupná žádná data.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-117">hello endpoint was polled toocheck for data and received a response that no data was available.</span></span>
* <span data-ttu-id="cb1d4-118">**Úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-118">**Succeeded**.</span></span> <span data-ttu-id="cb1d4-119">aktivační událost Hello obdržel odpověď, nebyla dostupná data.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-119">hello trigger received a response that data was available.</span></span> <span data-ttu-id="cb1d4-120">Tento stav může být výsledkem ruční aktivační událost, aktivační událost opakování nebo cyklického dotazování aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-120">This status might result from a manual trigger, a recurrence trigger, or a polling trigger.</span></span> <span data-ttu-id="cb1d4-121">Tento stav je obvykle přiložena hello **Fired** stav, ale možná, pokud máte v zobrazení kódu, která nebyla splněná podmínka nebo SplitOn příkaz.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-121">This status is usually accompanied by hello **Fired** status, but might not be if you have a condition or SplitOn command in code view that wasn't satisfied.</span></span>
* <span data-ttu-id="cb1d4-122">**Se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-122">**Failed**.</span></span> <span data-ttu-id="cb1d4-123">Byla generována chyba.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-123">An error was generated.</span></span>

#### <a name="start-a-trigger-manually"></a><span data-ttu-id="cb1d4-124">Ruční spuštění aktivační události</span><span class="sxs-lookup"><span data-stu-id="cb1d4-124">Start a trigger manually</span></span>

<span data-ttu-id="cb1d4-125">Pokud chcete pro aktivační procedury k dispozici okamžitě bez čekání na další opakování hello hello logiku aplikace toocheck, klikněte na tlačítko **vyberte aktivační událost** v hlavním okně tooforce hello kontrolu.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-125">If you want hello logic app toocheck for an available trigger immediately without waiting for hello next recurrence, click **Select Trigger** on hello main blade tooforce a check.</span></span> <span data-ttu-id="cb1d4-126">Například kliknutím na tento odkaz se aktivační událostí Dropbox způsobí, že hello pracovního postupu tooimmediately dotazování Dropbox pro nové soubory.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-126">For example, clicking this link with a Dropbox trigger causes hello workflow tooimmediately poll Dropbox for new files.</span></span>

### <a name="run-history"></a><span data-ttu-id="cb1d4-127">Historie spouštění</span><span class="sxs-lookup"><span data-stu-id="cb1d4-127">Run history</span></span>

<span data-ttu-id="cb1d4-128">Všechny aktivní aktivační události výsledkem spustit.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-128">Every fired trigger results in a run.</span></span> <span data-ttu-id="cb1d4-129">Informace o běhu můžete přistupovat z hlavní okno hello, který obsahuje mnoho podrobnosti, které vám může pomoct pochopit, co se stalo během pracovního postupu hello.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-129">You can access run information from hello main blade, which contains many details that can help you understand what happened during hello workflow.</span></span>

![Vyhledání hello historie spouštění][2]

<span data-ttu-id="cb1d4-131">Spustit zobrazí jedno z hello následující stavy:</span><span class="sxs-lookup"><span data-stu-id="cb1d4-131">A run displays one of hello following statuses:</span></span>

* <span data-ttu-id="cb1d4-132">**Úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-132">**Succeeded**.</span></span> <span data-ttu-id="cb1d4-133">Všechny akce bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-133">All actions succeeded.</span></span> <span data-ttu-id="cb1d4-134">Pokud došlo k selhání, že selhání zajišťoval akci, která došlo k chybě později v pracovním postupu hello.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-134">If a failure happened, that failure was handled by an action that occurred later in hello workflow.</span></span> <span data-ttu-id="cb1d4-135">To znamená selhání hello zajišťoval akci, která byla nastavena toorun po selhání akce.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-135">That is, hello failure was handled by an action that was set toorun after a failed action.</span></span>
* <span data-ttu-id="cb1d4-136">**Se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-136">**Failed**.</span></span> <span data-ttu-id="cb1d4-137">Nejméně jedna akce měl selhání, který nebyl zpracován akcí později v pracovním postupu hello.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-137">At least one action had a failure that was not handled by an action later in hello workflow.</span></span>
* <span data-ttu-id="cb1d4-138">**Zrušena**.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-138">**Cancelled**.</span></span> <span data-ttu-id="cb1d4-139">pracovní postup Hello byl spuštěn, ale přijal žádost o zrušení.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-139">hello workflow was running but received a cancel request.</span></span>
* <span data-ttu-id="cb1d4-140">**Spuštění**.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-140">**Running**.</span></span> <span data-ttu-id="cb1d4-141">pracovní postup Hello je právě spuštěna.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-141">hello workflow is currently running.</span></span> <span data-ttu-id="cb1d4-142">Tento stav může dojít k omezenému pracovních postupů, nebo z důvodu hello aktuální ceny plánu.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-142">This status might occur for throttled workflows, or because of hello current pricing plan.</span></span> <span data-ttu-id="cb1d4-143">Podrobnosti najdete v tématu [omezení akce na stránce s cenami hello](https://azure.microsoft.com/pricing/details/app-service/plans/).</span><span class="sxs-lookup"><span data-stu-id="cb1d4-143">For details, see [action limits on hello pricing page](https://azure.microsoft.com/pricing/details/app-service/plans/).</span></span> <span data-ttu-id="cb1d4-144">Konfigurace diagnostiky (hello grafů, které se zobrazují v části historie spouštění hello) také může poskytnout informace o všech omezení událostí, které dojít.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-144">Configuring diagnostics (hello charts that appear under hello run history) also can provide information about any throttle events that happen.</span></span>

<span data-ttu-id="cb1d4-145">Když se díváte historie spouštění, můžete zobrazit další podrobnosti Další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-145">When you are looking at a run history, you can drill in for more details.</span></span>  

#### <a name="trigger-outputs"></a><span data-ttu-id="cb1d4-146">Výstupy aktivační události</span><span class="sxs-lookup"><span data-stu-id="cb1d4-146">Trigger outputs</span></span>

<span data-ttu-id="cb1d4-147">Aktivační událost výstupy zobrazit hello data, která pochází hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-147">Trigger outputs show hello data that came from hello trigger.</span></span> <span data-ttu-id="cb1d4-148">Tyto výstupů vám pomohou určit, zda všechny vlastnosti vrátila podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-148">These outputs can help you determine whether all properties returned as expected.</span></span>

> [!NOTE]
> <span data-ttu-id="cb1d4-149">Pokud se zobrazí veškerý obsah, který neznáte, zjistěte, jak Azure Logic Apps [zpracovává jiné typy obsahu](../logic-apps/logic-apps-content-type.md).</span><span class="sxs-lookup"><span data-stu-id="cb1d4-149">If you see any content that you don't understand, learn how Azure Logic Apps [handles different content types](../logic-apps/logic-apps-content-type.md).</span></span>
> 

![Příklady výstup aktivační události][3]

#### <a name="action-inputs-and-outputs"></a><span data-ttu-id="cb1d4-151">Akce vstupy a výstupy</span><span class="sxs-lookup"><span data-stu-id="cb1d4-151">Action inputs and outputs</span></span>

<span data-ttu-id="cb1d4-152">Můžete rozbalit hello vstupy a výstupy, které přijaly akce.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-152">You can drill into hello inputs and outputs that an action received.</span></span> <span data-ttu-id="cb1d4-153">Tato data jsou užitečné pro pochopení hello velikost a tvar hello výstupy a také pro hledání chybové zprávy, které byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-153">This data is useful for understanding hello size and shape of hello outputs, and also for finding any error messages that might have been generated.</span></span>

![Akce vstupy a výstupy][4]

## <a name="debug-workflow-runtime"></a><span data-ttu-id="cb1d4-155">Ladění modulu runtime pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="cb1d4-155">Debug workflow runtime</span></span>

<span data-ttu-id="cb1d4-156">Společně s hello vstupy, výstupy a aktivuje spuštění monitorování, můžete přidat tooa pracovního postupu některé kroky, které pomáhají s laděním.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-156">Along with monitoring hello inputs, outputs, and triggers of a run, you could add some steps tooa workflow that help with debugging.</span></span> 
<span data-ttu-id="cb1d4-157">[RequestBin](http://requestb.in) je výkonný nástroj, který můžete přidat jako krok v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-157">[RequestBin](http://requestb.in) is a powerful tool that you can add as a step in a workflow.</span></span> <span data-ttu-id="cb1d4-158">Pomocí RequestBin můžete nastavit protokolu HTTP žádosti inspector toodetermine hello přesnou velikost, tvar a formát požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-158">By using RequestBin, you can set up an HTTP request inspector toodetermine hello exact size, shape, and format of an HTTP request.</span></span> <span data-ttu-id="cb1d4-159">Můžete vytvořit RequestBin a vložte adresu URL hello v aplikaci logiky akce HTTP POST s obsah textu, které chcete tootest, například, výraz nebo jiné krok výstup.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-159">You can create a RequestBin and paste hello URL in a logic app HTTP POST action with body content that you want tootest, for example, an expression or another step output.</span></span> <span data-ttu-id="cb1d4-160">Po spuštění aplikace logiky hello můžete obnovit vaše RequestBin toosee jak hello požadavek byl vytvořen při vygenerování ze stroje hello Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="cb1d4-160">After you run hello logic app, you can refresh your RequestBin toosee how hello request was formed when generated from hello Logic Apps engine.</span></span>

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
