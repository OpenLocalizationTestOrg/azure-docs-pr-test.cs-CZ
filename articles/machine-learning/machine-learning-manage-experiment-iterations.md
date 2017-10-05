---
title: "Správa iterací experimentu v nástroji Machine Learning Studio | Microsoft Docs"
description: "Správa iterací experimentu v nástroji Azure Machine Learning Studio"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a53530f-20d5-40ae-9b49-7b499ccb44b7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 0e32a02358d1901bb80f356b0289b02b8e98afdb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio"></a><span data-ttu-id="1cfe9-103">Správa iterací experimentů v nástroji Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="1cfe9-103">Manage experiment iterations in Azure Machine Learning Studio</span></span>
<span data-ttu-id="1cfe9-104">Vývoj model prediktivní analýzy je iterativní proces - úpravou různých funkcí a parametry experimentu výsledky zpřesňují, dokud nebudete přesvědčeni, že máte natrénován efektivní model.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-104">Developing a predictive analysis model is an iterative process - as you modify the various functions and parameters of your experiment, your results converge until you are satisfied that you have a trained, effective model.</span></span> <span data-ttu-id="1cfe9-105">Klíč pro tento proces je sledování různých iterací experimentu parametry a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-105">Key to this process is tracking the various iterations of your experiment parameters and configurations.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="1cfe9-106">Můžete zkontrolovat předchozích spuštění experimentů kdykoli chcete-li challenge, pokroku a nakonec potvrďte nebo Upřesnit předchozí předpoklady.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-106">You can review previous runs of your experiments at any time in order to challenge, revisit, and ultimately either confirm or refine previous assumptions.</span></span> <span data-ttu-id="1cfe9-107">Při spuštění experimentu, Machine Learning Studio uchovává historii spustit, včetně datovou sadu, modul a připojení k portu a parametry.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-107">When you run an experiment, Machine Learning Studio keeps a history of the run, including dataset, module, and port connections and parameters.</span></span> <span data-ttu-id="1cfe9-108">Tato historie taky zaznamená výsledky, informace o běhu programu, například spuštění a zastavení časy, zprávy protokolu a stav spuštění.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-108">This history also captures results, runtime information such as start and stop times, log messages, and execution status.</span></span> <span data-ttu-id="1cfe9-109">Můžete najít zpět na kterémkoli z těchto spustí kdykoli zkontrolovat časovou posloupnost experiment a mezilehlých výsledků.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-109">You can look back at any of these runs at any time to review the chronology of your experiment and intermediate results.</span></span> <span data-ttu-id="1cfe9-110">Předchozího spuštění experimentu můžete použít i ke spuštění na cestu ke vytvoření jednoduché, komplexní nebo i řešení modelování komplet do nové fázi dotaz a zjišťování.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-110">You can even use a previous run of your experiment to launch into a new phase of inquiry and discovery on your path to creating simple, complex, or even ensemble modeling solutions.</span></span>

> [!NOTE]
> <span data-ttu-id="1cfe9-111">Při zobrazení předchozího spuštění experimentu, že tato verze experimentu je uzamčen a nelze jej upravit.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-111">When you view a previous run of an experiment, that version of the experiment is locked and can't be edited.</span></span> <span data-ttu-id="1cfe9-112">Můžete však uložení kopie ho kliknutím na **uložit jako** a poskytuje nový název kopie.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-112">You can, however, save a copy of it by clicking **SAVE AS** and providing a new name for the copy.</span></span> <span data-ttu-id="1cfe9-113">Machine Learning Studio otevře novou kopii, která pak můžete upravit a spustit.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-113">Machine Learning Studio opens the new copy, which you can then edit and run.</span></span> <span data-ttu-id="1cfe9-114">Je k dispozici v této kopie experimentu **EXPERIMENTY** seznamu společně s všechny ostatní experimentů.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-114">This copy of your experiment is available in the **EXPERIMENTS** list along with all your other experiments.</span></span>
> 
> 

## <a name="viewing-the-prior-run"></a><span data-ttu-id="1cfe9-115">Zobrazení předchozího spuštění</span><span class="sxs-lookup"><span data-stu-id="1cfe9-115">Viewing the Prior Run</span></span>
<span data-ttu-id="1cfe9-116">Až budete mít otevřenou experimentu, který jste spustili alespoň jednou, předchozí spuštění experimentu můžete zobrazit kliknutím **předchozí spustit** v podokně vlastností.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-116">When you have an experiment open that you have run at least once, you can view the preceding run of the experiment by clicking **Prior Run** in the properties pane.</span></span>

<span data-ttu-id="1cfe9-117">Předpokládejme například, můžete vytvořit nový experiment a spustit verze v 11:23 11:42 a 11:55.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-117">For example, suppose you create an experiment and run versions of it at 11:23, 11:42, and 11:55.</span></span> <span data-ttu-id="1cfe9-118">Pokud otevřete posledním spuštění experimentu (11:55) a klikněte na tlačítko **předchozí spustit**, verze, které jste spustili na 11:42 je otevřen.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-118">If you open the last run of the experiment (11:55) and click **Prior Run**, the version you ran at 11:42 is opened.</span></span>

## <a name="viewing-the-run-history"></a><span data-ttu-id="1cfe9-119">Zobrazení historie spouštění</span><span class="sxs-lookup"><span data-stu-id="1cfe9-119">Viewing the Run History</span></span>
<span data-ttu-id="1cfe9-120">Kliknutím můžete zobrazit všechny předchozí spustí experimentu **zobrazit historii běhů** v experimentu otevřete.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-120">You can view all the previous runs of an experiment by clicking **View Run History** in an open experiment.</span></span>

<span data-ttu-id="1cfe9-121">Předpokládejme například, můžete vytvořit nový experiment s [lineární regrese] [ linear-regression] modulu a chcete sledovat účinek změna hodnoty **rychlost učení** na vaše výsledky experimentu.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-121">For example, suppose you create an experiment with the [Linear Regression][linear-regression] module and you want to observe the effect of changing the value of **Learning rate** on your experiment results.</span></span> <span data-ttu-id="1cfe9-122">Můžete spustit experiment vícekrát s různými hodnotami pro tento parametr, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1cfe9-122">You run the experiment multiple times with different values for this parameter, as follows:</span></span>

| <span data-ttu-id="1cfe9-123">Hodnota míry učení</span><span class="sxs-lookup"><span data-stu-id="1cfe9-123">Learning Rate value</span></span> | <span data-ttu-id="1cfe9-124">Počáteční čas spuštění</span><span class="sxs-lookup"><span data-stu-id="1cfe9-124">Run start time</span></span> |
| --- | --- |
| <span data-ttu-id="1cfe9-125">0.1</span><span class="sxs-lookup"><span data-stu-id="1cfe9-125">0.1</span></span> |<span data-ttu-id="1cfe9-126">11/9/2014 4:18:58 pm</span><span class="sxs-lookup"><span data-stu-id="1cfe9-126">9/11/2014 4:18:58 pm</span></span> |
| <span data-ttu-id="1cfe9-127">0.2</span><span class="sxs-lookup"><span data-stu-id="1cfe9-127">0.2</span></span> |<span data-ttu-id="1cfe9-128">11/9/2014 4:24:33 pm</span><span class="sxs-lookup"><span data-stu-id="1cfe9-128">9/11/2014 4:24:33 pm</span></span> |
| <span data-ttu-id="1cfe9-129">0.4</span><span class="sxs-lookup"><span data-stu-id="1cfe9-129">0.4</span></span> |<span data-ttu-id="1cfe9-130">11/9/2014 4:28:36 pm</span><span class="sxs-lookup"><span data-stu-id="1cfe9-130">9/11/2014 4:28:36 pm</span></span> |
| <span data-ttu-id="1cfe9-131">0.5</span><span class="sxs-lookup"><span data-stu-id="1cfe9-131">0.5</span></span> |<span data-ttu-id="1cfe9-132">11/9/2014 4:33:31 pm</span><span class="sxs-lookup"><span data-stu-id="1cfe9-132">9/11/2014 4:33:31 pm</span></span> |

<span data-ttu-id="1cfe9-133">Pokud kliknete na tlačítko **zobrazit HISTORII BĚHŮ**, zobrazí se seznam všech těchto spuštění:</span><span class="sxs-lookup"><span data-stu-id="1cfe9-133">If you click **VIEW RUN HISTORY**, you see a list of all these runs:</span></span>

![Příklad historie spouštění][runhistory]

<span data-ttu-id="1cfe9-135">Kliknutím na jakýkoli z těchto používá pro zobrazení snímek experimentu v době, kdy jste ho spustili.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-135">Click any of these runs to view a snapshot of the experiment at the time you ran it.</span></span> <span data-ttu-id="1cfe9-136">Konfigurace, hodnoty parametrů, komentáře a výsledky jsou všechny zachována tak, abyste získali, ze kterých běží experimentu.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-136">The configuration, parameter values, comments, and results are all preserved to give you a complete record of that run of your experiment.</span></span>

> [!TIP]
> <span data-ttu-id="1cfe9-137">K dokumentu vaší iterací experimentu, můžete upravit název pokaždé, když spustíte ji, můžete aktualizovat **Souhrn** experimentu ve vlastnostech panelu a přidat nebo aktualizovat komentáře na jednotlivé moduly zaznamenat vaše změny.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-137">To document your iterations of the experiment, you can modify the title each time you run it, you can update the **Summary** of the experiment in the properties pane, and you can add or update comments on individual modules to record your changes.</span></span> <span data-ttu-id="1cfe9-138">Název, souhrn a modul komentáře se ukládají s každé spuštění experimentu.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-138">The title, summary, and module comments are saved with each run of the experiment.</span></span>
> 
> 

<span data-ttu-id="1cfe9-139">Seznam experimenty v **EXPERIMENTY** karta v nástroji Machine Learning Studio vždy zobrazuje nejnovější verzi experimentu.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-139">The list of experiments in the **EXPERIMENTS** tab in Machine Learning Studio always displays the latest version of an experiment.</span></span> <span data-ttu-id="1cfe9-140">Pokud otevřete předchozího spuštění experimentu (pomocí **předchozí spustit** nebo **zobrazit HISTORII BĚHŮ**), mohli vrátit k verzi konceptu kliknutím **zobrazit HISTORII BĚHŮ** a výběr iterace, který má **stavu** z **upravit**.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-140">If you open a previous run of the experiment (using **Prior Run** or **VIEW RUN HISTORY**), you can return to the draft version by clicking **VIEW RUN HISTORY** and selecting the iteration that has a **STATE** of **Editable**.</span></span>

## <a name="iterating-on-a-previous-run"></a><span data-ttu-id="1cfe9-141">Iterace v předchozího spuštění</span><span class="sxs-lookup"><span data-stu-id="1cfe9-141">Iterating on a Previous Run</span></span>
<span data-ttu-id="1cfe9-142">Když kliknete na tlačítko **předchozí spustit** nebo **zobrazit HISTORII BĚHŮ** a otevřete předchozího spuštění, dokončení experimentu můžete zobrazit v režimu jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-142">When you click **Prior Run** or **VIEW RUN HISTORY** and open a previous run, you can view a finished experiment in read-only mode.</span></span>

<span data-ttu-id="1cfe9-143">Pokud chcete začít iterace experimentu počínaje způsob, jak jste nakonfigurovali pro předchozího spuštění, můžete k tomu otevřením spustit a kliknutím na **uložit jako**.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-143">If you want to begin an iteration of your experiment starting with the way you configured it for a previous run, you can do this by opening the run and clicking **SAVE AS**.</span></span> <span data-ttu-id="1cfe9-144">Tím se vytvoří nový experiment, s nový název, prázdnou historie, spouštění a spusťte všechny součásti a předchozí hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-144">This creates a new experiment, with a new title, an empty run history, and all the components and parameter values of the previous run.</span></span> <span data-ttu-id="1cfe9-145">Tento nový experiment, je uvedena ve **EXPERIMENTY** ve domovské stránce Machine Learning Studio a můžete upravit a spustit, inicializaci novou spusťte historie pro tento iteraci experimentu.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-145">This new experiment is listed in the **EXPERIMENTS** tab in the Machine Learning Studio home page, and you can modify and run it, initiating a new run history for this iteration of your experiment.</span></span> 

<span data-ttu-id="1cfe9-146">Předpokládejme například, že máte experiment spustit historie uvedené v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-146">For example, suppose you have the experiment run history shown in the previous section.</span></span> <span data-ttu-id="1cfe9-147">Chcete pozorovat, co se stane, když nastavíte **rychlost učení** do 0.4 a zkuste to různé hodnoty pro parametr **počet školení epoch** parametr.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-147">You want to observe what happens when you set the **Learning rate** parameter to 0.4, and try different values for the **Number of training epochs** parameter.</span></span>

1. <span data-ttu-id="1cfe9-148">Klikněte na tlačítko **zobrazit HISTORII BĚHŮ** a otevřete iterace experimentu, který jste spustili ve 4:28:36 (ve kterém nastavíte hodnotu parametru na 0.4).</span><span class="sxs-lookup"><span data-stu-id="1cfe9-148">Click **VIEW RUN HISTORY** and open the iteration of the experiment that you ran at 4:28:36 pm (in which you set the parameter value to 0.4).</span></span>
2. <span data-ttu-id="1cfe9-149">Klikněte na tlačítko **uložit jako**.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-149">Click **SAVE AS**.</span></span>
3. <span data-ttu-id="1cfe9-150">Zadejte nový název a klikněte na **OK** zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-150">Enter a new title and click the **OK** checkmark.</span></span> <span data-ttu-id="1cfe9-151">Se vytvoří novou kopii tohoto experimentu.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-151">A new copy of the experiment is created.</span></span>
4. <span data-ttu-id="1cfe9-152">Změnit **počet školení epoch** parametr.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-152">Modify the **Number of training epochs** parameter.</span></span>
5. <span data-ttu-id="1cfe9-153">Klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-153">Click **RUN**.</span></span>

<span data-ttu-id="1cfe9-154">Můžete teď můžete pokračovat upravte a spusťte tuto verzi experimentu, vytváření nové historie spouštění k zaznamenání práci.</span><span class="sxs-lookup"><span data-stu-id="1cfe9-154">You can now continue to modify and run this version of your experiment, building a new run history to record your work.</span></span>

<!-- Images -->
[runhistory]:./media/machine-learning-manage-experiment-iterations/viewrunhistory.jpg


<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
