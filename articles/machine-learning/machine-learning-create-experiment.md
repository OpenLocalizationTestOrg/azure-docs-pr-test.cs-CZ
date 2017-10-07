---
title: "aaaA jednoduchý experiment v nástroji Machine Learning Studio | Microsoft Docs"
description: "Tento kurz Machine Learningu vás provede jednoduchým experimentem z oblasti datové vědy. Jsme budete předvídání hello ceny automobilu pomocí regresní algoritmus."
keywords: "experiment,lineární regrese,algoritmy Machine Learningu,kurz Machine Learningu,techniky prediktivního modelování,experiment z oblasti datové vědy"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: fb215851d380acf7d0f4934a426283369f9c4ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a><span data-ttu-id="ee873-105">Kurz Machine Learningu: Vytvoření prvního experimentu z oblasti datové vědy v nástroji Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="ee873-105">Machine learning tutorial: Create your first data science experiment in Azure Machine Learning Studio</span></span>

<span data-ttu-id="ee873-106">Pokud jste **Azure Machine Learning Studio** nikdy nepoužívali, je tento kurz právě pro vás.</span><span class="sxs-lookup"><span data-stu-id="ee873-106">If you've never used **Azure Machine Learning Studio** before, this tutorial is for you.</span></span>

<span data-ttu-id="ee873-107">V tomto kurzu budeme zabývat jak toouse Studio pro hello nejprve čas toocreate experimentu strojového učení.</span><span class="sxs-lookup"><span data-stu-id="ee873-107">In this tutorial, we'll walk through how toouse Studio for hello first time toocreate a machine learning experiment.</span></span> <span data-ttu-id="ee873-108">Hello experiment testovat analytical modelu, který bude předpovídat cenu automobilu podle různých proměnných, například značky a technických specifikací hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-108">hello experiment will test an analytical model that predicts hello price of an automobile based on different variables such as make and technical specifications.</span></span>

> [!NOTE]
> <span data-ttu-id="ee873-109">Tento kurz ukazuje základy hello modulů jak toodrag myší do experimentu, připojte je společně, spustit hello experiment a podívejte se na výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-109">This tutorial shows you hello basics of how toodrag-and-drop modules onto your experiment, connect them together, run hello experiment, and look at hello results.</span></span> <span data-ttu-id="ee873-110">Nebudete jsme, že toodiscuss hello obecné téma strojové učení nebo jak tooselect a používání hello 100 + předdefinované algoritmy a data manipulaci s moduly součástí Studio.</span><span class="sxs-lookup"><span data-stu-id="ee873-110">We're not going toodiscuss hello general topic of machine learning or how tooselect and use hello 100+ built-in algorithms and data manipulation modules included in Studio.</span></span>
>
><span data-ttu-id="ee873-111">Pokud jste nový learning toomachine, hello série videí [vědecké zpracování dat pro začátečníky](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) může být vhodné místo toostart.</span><span class="sxs-lookup"><span data-stu-id="ee873-111">If you're new toomachine learning, hello video series [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) might be a good place toostart.</span></span> <span data-ttu-id="ee873-112">Tato série videí je learning toomachine skvělé Úvod pomocí jazyka pro každý den a koncepty.</span><span class="sxs-lookup"><span data-stu-id="ee873-112">This video series is a great introduction toomachine learning using everyday language and concepts.</span></span>
>
><span data-ttu-id="ee873-113">Pokud jste obeznámeni s machine learning, ale hledáte další obecné informace o Machine Learning Studio a hello algoritmů strojového učení, které obsahuje, zde jsou některé funkční prostředky:</span><span class="sxs-lookup"><span data-stu-id="ee873-113">If you're familiar with machine learning, but you're looking for more general information about Machine Learning Studio, and hello machine learning algorithms it contains, here are some good resources:</span></span>
>
- [<span data-ttu-id="ee873-114">Co je Machine Learning Studio?</span><span class="sxs-lookup"><span data-stu-id="ee873-114">What is Machine Learning Studio?</span></span>](machine-learning-what-is-ml-studio.md) <span data-ttu-id="ee873-115">- Toto téma obsahuje obecný přehled sady Studio.</span><span class="sxs-lookup"><span data-stu-id="ee873-115">- This is a high-level overview of Studio.</span></span>
- <span data-ttu-id="ee873-116">[Strojového učení základy s příklady algoritmus](machine-learning-basics-infographic-with-algorithm-examples.md) -tento infografice je užitečné, pokud chcete více informací o hello různé typy algoritmů strojového učení součástí Machine Learning Studio toolearn.</span><span class="sxs-lookup"><span data-stu-id="ee873-116">[Machine learning basics with algorithm examples](machine-learning-basics-infographic-with-algorithm-examples.md) - This infographic is useful if you want toolearn more about hello different types of machine learning algorithms included with Machine Learning Studio.</span></span>
- <span data-ttu-id="ee873-117">[Počítač Learning průvodce](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) -Tento průvodce popisuje podobné informace jako hello infografice výše, ale interaktivní formát.</span><span class="sxs-lookup"><span data-stu-id="ee873-117">[Machine Learning Guide](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) - This guide covers similar information as hello infographic above, but in an interactive format.</span></span>
- <span data-ttu-id="ee873-118">[Strojového učení rychlý přehled algoritmů](machine-learning-algorithm-cheat-sheet.md) a [jak toochoose algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) – tento plakát ke stažení a doprovodné článku popisují hello Studio algoritmy podrobněji.</span><span class="sxs-lookup"><span data-stu-id="ee873-118">[Machine learning algorithm cheat sheet](machine-learning-algorithm-cheat-sheet.md) and [How toochoose algorithms for Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) - This downloadable poster and accompanying article discuss hello Studio algorithms in depth.</span></span>
- <span data-ttu-id="ee873-119">[Machine Learning Studio: Algoritmus a pomáhají modulu](https://msdn.microsoft.com/library/azure/dn905974.aspx) -Toto je hello úplný odkaz pro všechny moduly ze studia, včetně algoritmy strojového učení</span><span class="sxs-lookup"><span data-stu-id="ee873-119">[Machine Learning Studio: Algorithm and Module Help](https://msdn.microsoft.com/library/azure/dn905974.aspx) - This is hello complete reference for all Studio modules, including machine learning algorithms,</span></span>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a><span data-ttu-id="ee873-120">V čem je přínos nástroje Machine Learning Studio?</span><span class="sxs-lookup"><span data-stu-id="ee873-120">How does Machine Learning Studio help?</span></span>

<span data-ttu-id="ee873-121">Machine Learning Studio umožňuje snadno tooset až experimentu pomocí naprogramovaných s techniky prediktivního modelování přetahování myší moduly.</span><span class="sxs-lookup"><span data-stu-id="ee873-121">Machine Learning Studio makes it easy tooset up an experiment using drag-and-drop modules preprogrammed with predictive modeling techniques.</span></span>

<span data-ttu-id="ee873-122">Pomocí interaktivního vizuálního pracovního prostoru můžete přetáhnout ***datové sady*** a ***moduly*** analýz na interaktivní plátno.</span><span class="sxs-lookup"><span data-stu-id="ee873-122">Using an interactive, visual workspace, you drag-and-drop ***datasets*** and analysis ***modules*** onto an interactive canvas.</span></span> <span data-ttu-id="ee873-123">Připojení je společně tooform ***experimentovat*** spuštění v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="ee873-123">You connect them together tooform an ***experiment*** that you run in Machine Learning Studio.</span></span>
<span data-ttu-id="ee873-124">Můžete ***vytvoření modelu***, ***trénování modelu hello***, a ***skóre a testování hello modelu***.</span><span class="sxs-lookup"><span data-stu-id="ee873-124">You ***create a model***, ***train hello model***, and ***score and test hello model***.</span></span>

<span data-ttu-id="ee873-125">Můžete iterace návrhu modelu úpravy hello experiment a poskytuje systémem dokud hello výsledků, které hledáte.</span><span class="sxs-lookup"><span data-stu-id="ee873-125">You can iterate on your model design, editing hello experiment and running it until it gives you hello results you're looking for.</span></span> <span data-ttu-id="ee873-126">Když je model hotový, můžete ho publikovat jako ***webovou službu***, aby mu i ostatní mohli zasílat nová data a získávat na oplátku predikce.</span><span class="sxs-lookup"><span data-stu-id="ee873-126">When your model is ready, you can publish it as a ***web service*** so that others can send it new data and get predictions in return.</span></span>

## <a name="open-machine-learning-studio"></a><span data-ttu-id="ee873-127">Otevřít Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="ee873-127">Open Machine Learning Studio</span></span>

<span data-ttu-id="ee873-128">tooget začít s Studio přejděte příliš[https://studio.azureml.net](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="ee873-128">tooget started with Studio, go too[https://studio.azureml.net](https://studio.azureml.net).</span></span> <span data-ttu-id="ee873-129">Pokud už jste se do nástroje Machine Learning Studio někdy přihlašovali, klikněte na odkaz **Přihlásit se**.</span><span class="sxs-lookup"><span data-stu-id="ee873-129">If you’ve signed into Machine Learning Studio before, click **Sign In**.</span></span> <span data-ttu-id="ee873-130">Jinak klikněte na **Zaregistrovat** a vyberte si mezi bezplatnou a placenou možností.</span><span class="sxs-lookup"><span data-stu-id="ee873-130">Otherwise, click **Sign up here** and choose between free and paid options.</span></span>

<span data-ttu-id="ee873-131">![Přihlaste se tooMachine Learning Studio][sign-in-to-studio]
</span><span class="sxs-lookup"><span data-stu-id="ee873-131">![Sign in tooMachine Learning Studio][sign-in-to-studio]
</span></span><br/><span data-ttu-id="ee873-132">
***Přihlaste se tooMachine Learning Studio***</span><span class="sxs-lookup"><span data-stu-id="ee873-132">
***Sign in tooMachine Learning Studio***</span></span>

## <a name="five-steps-toocreate-an-experiment"></a><span data-ttu-id="ee873-133">V pěti krocích toocreate experimentu</span><span class="sxs-lookup"><span data-stu-id="ee873-133">Five steps toocreate an experiment</span></span>

<span data-ttu-id="ee873-134">V tento kurz strojového učení budete postupovat podle pět základních kroků toobuild experimentu v Machine Learning Studio toocreate, train a stanovení skóre modelu:</span><span class="sxs-lookup"><span data-stu-id="ee873-134">In this machine learning tutorial, you'll follow five basic steps toobuild an experiment in Machine Learning Studio toocreate, train, and score your model:</span></span>

- <span data-ttu-id="ee873-135">**Vytvoření modelu**</span><span class="sxs-lookup"><span data-stu-id="ee873-135">**Create a model**</span></span>
    - <span data-ttu-id="ee873-136">[Krok 1: Získání dat]</span><span class="sxs-lookup"><span data-stu-id="ee873-136">[Step 1: Get data]</span></span>
    - <span data-ttu-id="ee873-137">[Krok 2: Příprava hello dat]</span><span class="sxs-lookup"><span data-stu-id="ee873-137">[Step 2: Prepare hello data]</span></span>
    - <span data-ttu-id="ee873-138">[Krok 3: Definice příznaků]</span><span class="sxs-lookup"><span data-stu-id="ee873-138">[Step 3: Define features]</span></span>
- <span data-ttu-id="ee873-139">**Train hello model**</span><span class="sxs-lookup"><span data-stu-id="ee873-139">**Train hello model**</span></span>
    - <span data-ttu-id="ee873-140">[Krok 4: Volba a použití algoritmu učení]</span><span class="sxs-lookup"><span data-stu-id="ee873-140">[Step 4: Choose and apply a learning algorithm]</span></span>
- <span data-ttu-id="ee873-141">**Skóre a testování hello modelu**</span><span class="sxs-lookup"><span data-stu-id="ee873-141">**Score and test hello model**</span></span>
    - <span data-ttu-id="ee873-142">[Krok 5: Předpověď cen nových automobilů]</span><span class="sxs-lookup"><span data-stu-id="ee873-142">[Step 5: Predict new automobile prices]</span></span>

[Krok 1: Získání dat]: #step-1-get-data
[Krok 2: Příprava hello dat]: #step-2-prepare-the-data
[Krok 3: Definice příznaků]: #step-3-define-features
[Krok 4: Volba a použití algoritmu učení]: #step-4-choose-and-apply-a-learning-algorithm
[Krok 5: Předpověď cen nových automobilů]: #step-5-predict-new-automobile-prices

> [!TIP] 
> <span data-ttu-id="ee873-148">Můžete najít pracovní kopie hello následující experimentu v hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="ee873-148">You can find a working copy of hello following experiment in hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="ee873-149">Přejděte příliš**[první vědecké zpracování dat experimentovat - předpověď ceny automobilu](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)**  a klikněte na tlačítko **Open in Studio** toodownload kopii hello experimentu do Machine Learning Pracovní prostor Studio.</span><span class="sxs-lookup"><span data-stu-id="ee873-149">Go too**[Your first data science experiment - Automobile price prediction](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)** and click **Open in Studio** toodownload a copy of hello experiment into your Machine Learning Studio workspace.</span></span>


## <a name="step-1-get-data"></a><span data-ttu-id="ee873-150">Krok 1: Získání dat</span><span class="sxs-lookup"><span data-stu-id="ee873-150">Step 1: Get data</span></span>

<span data-ttu-id="ee873-151">První věc Hello potřebujete tooperform machine learning je data.</span><span class="sxs-lookup"><span data-stu-id="ee873-151">hello first thing you need tooperform machine learning is data.</span></span>
<span data-ttu-id="ee873-152">Machine Learning Studio obsahuje několik ukázkových datových sad, které můžete použít, nebo můžete data importovat z mnoha zdrojů.</span><span class="sxs-lookup"><span data-stu-id="ee873-152">There are several sample datasets included with Machine Learning Studio that you can use, or you can import data from many sources.</span></span> <span data-ttu-id="ee873-153">V tomto příkladu použijeme hello ukázkovou datovou sadu, **Automobile price data (Raw)**, který je zahrnut do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="ee873-153">For this example, we'll use hello sample dataset, **Automobile price data (Raw)**, that's included in your workspace.</span></span>
<span data-ttu-id="ee873-154">Tato datová sada obsahuje záznamy řady různých automobilů, včetně informací o značce, modelu, technických specifikacích a ceně.</span><span class="sxs-lookup"><span data-stu-id="ee873-154">This dataset includes entries for various individual automobiles, including information such as make, model, technical specifications, and price.</span></span>

<span data-ttu-id="ee873-155">Zde je, jak tooget hello datové sady do experimentu.</span><span class="sxs-lookup"><span data-stu-id="ee873-155">Here's how tooget hello dataset into your experiment.</span></span>

1. <span data-ttu-id="ee873-156">Vytvořit nový experiment kliknutím na tlačítko **+ nový** v hello spodní části hello Machine Learning Studio, vyberte **EXPERIMENTOVAT**a potom vyberte **prázdný Experiment**.</span><span class="sxs-lookup"><span data-stu-id="ee873-156">Create a new experiment by clicking **+NEW** at hello bottom of hello Machine Learning Studio window, select **EXPERIMENT**, and then select **Blank Experiment**.</span></span>

2. <span data-ttu-id="ee873-157">Hello experimentu je zadána výchozí název, který se zobrazí v horní části hello hello plátna.</span><span class="sxs-lookup"><span data-stu-id="ee873-157">hello experiment is given a default name that you can see at hello top of hello canvas.</span></span> <span data-ttu-id="ee873-158">Vyberte tento text a přejmenujte ji například toosomething smysluplného, **předpověď ceny automobilu**.</span><span class="sxs-lookup"><span data-stu-id="ee873-158">Select this text and rename it toosomething meaningful, for example, **Automobile price prediction**.</span></span> <span data-ttu-id="ee873-159">Název Hello nepotřebuje toobe jedinečný.</span><span class="sxs-lookup"><span data-stu-id="ee873-159">hello name doesn't need toobe unique.</span></span>

    ![Přejmenujte hello experimentu][rename-experiment]

2. <span data-ttu-id="ee873-161">toohello nalevo od plátna hello experimentu je paleta datových sad a modulů.</span><span class="sxs-lookup"><span data-stu-id="ee873-161">toohello left of hello experiment canvas is a palette of datasets and modules.</span></span> <span data-ttu-id="ee873-162">Typ **automobilu** hello vyhledávacího pole hello horní části této palety toofind hello datovou sadu s názvem bez přípony **Automobile price data (Raw)**.</span><span class="sxs-lookup"><span data-stu-id="ee873-162">Type **automobile** in hello Search box at hello top of this palette toofind hello dataset labeled **Automobile price data (Raw)**.</span></span> <span data-ttu-id="ee873-163">Přetáhněte plátno experimentu toohello tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="ee873-163">Drag this dataset toohello experiment canvas.</span></span>

    <span data-ttu-id="ee873-164">![Datové sady automobilů hello najděte a přetáhněte ji na plátno experimentu hello][type-automobile]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-164">![Find hello automobile dataset and drag it onto hello experiment canvas][type-automobile]
    </span></span><br/>
 <span data-ttu-id="ee873-165">***Datové sady automobilů hello najděte a přetáhněte ji na plátno experimentu hello***</span><span class="sxs-lookup"><span data-stu-id="ee873-165">***Find hello automobile dataset and drag it onto hello experiment canvas***</span></span>

<span data-ttu-id="ee873-166">toosee co tato data vypadají, klikněte na výstupní port hello dole hello hello automobilů datové sady a potom vyberte **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="ee873-166">toosee what this data looks like, click hello output port at hello bottom of hello automobile dataset, and then select **Visualize**.</span></span>

<span data-ttu-id="ee873-167">![Klikněte na výstupní port hello a vyberte "Vizualizovat"][select-visualize]
</span><span class="sxs-lookup"><span data-stu-id="ee873-167">![Click hello output port and select "Visualize"][select-visualize]
</span></span><br/><span data-ttu-id="ee873-168">
***Klikněte na výstupní port hello a vyberte "Vizualizovat"***</span><span class="sxs-lookup"><span data-stu-id="ee873-168">
***Click hello output port and select "Visualize"***</span></span>

> [!TIP]
> <span data-ttu-id="ee873-169">Datových sad a modulů mít vstup a výstup porty reprezentována malé kroužky - vstupních portů v horní části hello, výstup porty v dolní části hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-169">Datasets and modules have input and output ports represented by small circles - input ports at hello top, output ports at hello bottom.</span></span>
<span data-ttu-id="ee873-170">toocreate tok dat experimentu, budete výstupní port modulu jeden modul tooan vstupní port jiného připojení.</span><span class="sxs-lookup"><span data-stu-id="ee873-170">toocreate a flow of data through your experiment, you'll connect an output port of one module tooan input port of another.</span></span>
<span data-ttu-id="ee873-171">Kdykoli můžete kliknout na hello výstupní port modulu datové sady nebo modul toosee jaká data hello vypadá jako v tomto bodě v toku dat hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-171">At any time, you can click hello output port of a dataset or module toosee what hello data looks like at that point in hello data flow.</span></span>

<span data-ttu-id="ee873-172">V této datové sadě ukázka každá instance automobilu je představována jedním řádkem a hello proměnných spojených s každou automobilu zobrazit jako sloupce.</span><span class="sxs-lookup"><span data-stu-id="ee873-172">In this sample dataset, each instance of an automobile appears as a row, and hello variables associated with each automobile appear as columns.</span></span> <span data-ttu-id="ee873-173">Pro konkrétní automobilu zadané hello proměnné, vytvoříme tootry toopredict hello cenu v sloupec nejvíce vpravo (sloupec 26, s názvem "cena").</span><span class="sxs-lookup"><span data-stu-id="ee873-173">Given hello variables for a specific automobile, we're going tootry toopredict hello price in far-right column (column 26, titled "price").</span></span>

<span data-ttu-id="ee873-174">![Zobrazení dat automobilů hello v hello okno vizualizace dat][visualize-auto-data]
</span><span class="sxs-lookup"><span data-stu-id="ee873-174">![View hello automobile data in hello data visualization window][visualize-auto-data]
</span></span><br/><span data-ttu-id="ee873-175">
***Zobrazení dat automobilů hello v hello okno vizualizace dat***</span><span class="sxs-lookup"><span data-stu-id="ee873-175">
***View hello automobile data in hello data visualization window***</span></span>

<span data-ttu-id="ee873-176">Okno vizualizace zavřít hello kliknutím hello "**x**" v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-176">Close hello visualization window by clicking hello "**x**" in hello upper-right corner.</span></span>

## <a name="step-2-prepare-hello-data"></a><span data-ttu-id="ee873-177">Krok 2: Příprava hello dat</span><span class="sxs-lookup"><span data-stu-id="ee873-177">Step 2: Prepare hello data</span></span>

<span data-ttu-id="ee873-178">Před analýzou datové sady bývá zpravidla nutné sadu nějakým způsobem předzpracovat.</span><span class="sxs-lookup"><span data-stu-id="ee873-178">A dataset usually requires some preprocessing before it can be analyzed.</span></span> <span data-ttu-id="ee873-179">Například můžete možná jste si všimli chybějících hodnot ve sloupcích různých řádků hello hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-179">For example, you might have noticed hello missing values present in hello columns of various rows.</span></span> <span data-ttu-id="ee873-180">Tyto chybějící hodnoty se musí toobe vyčistit, aby hello modelu můžete hello data správně analyzovat.</span><span class="sxs-lookup"><span data-stu-id="ee873-180">These missing values need toobe cleaned so hello model can analyze hello data correctly.</span></span> <span data-ttu-id="ee873-181">V našem případě odstraníme všechny řádky, ve kterých některé hodnoty chybí.</span><span class="sxs-lookup"><span data-stu-id="ee873-181">In our case, we'll remove any rows that have missing values.</span></span> <span data-ttu-id="ee873-182">Navíc hello **normalized-losses** sloupec má velká část chybějící hodnoty, takže jsme budete vyloučit sloupce zcela z modelu hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-182">Also, hello **normalized-losses** column has a large proportion of missing values, so we'll exclude that column from hello model altogether.</span></span>

> [!TIP]
> <span data-ttu-id="ee873-183">Čištění hello chybějících hodnot ze vstupních dat je předpokladem pro používání většinu modulů hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-183">Cleaning hello missing values from input data is a prerequisite for using most of hello modules.</span></span>

<span data-ttu-id="ee873-184">Nejprve přidáme modul, který odebere hello **normalized-losses** sloupec úplně, a potom přidáme jiný modul, který odstraní všechny řádky, ve kterých chybí data.</span><span class="sxs-lookup"><span data-stu-id="ee873-184">First we add a module that removes hello **normalized-losses** column completely, and then we add another module that removes any row that has missing data.</span></span>

1. <span data-ttu-id="ee873-185">Typ **vyberte sloupce** hello vyhledávacího pole hello horní části hello modulu palety toofind hello [výběr sloupců v datové sadě] [ select-columns] modulu, přetáhněte ji na plátno experimentu toohello .</span><span class="sxs-lookup"><span data-stu-id="ee873-185">Type **select columns** in hello Search box at hello top of hello module palette toofind hello [Select Columns in Dataset][select-columns] module, then drag it toohello experiment canvas.</span></span> <span data-ttu-id="ee873-186">Tento modul umožňuje tooselect, které sloupce dat, můžeme má tooinclude nebo vyloučení z hello modelu.</span><span class="sxs-lookup"><span data-stu-id="ee873-186">This module allows us tooselect which columns of data we want tooinclude or exclude in hello model.</span></span>

2. <span data-ttu-id="ee873-187">Připojit hello výstupní port modulu hello **Automobile price data (Raw)** datovou sadu toohello vstupní port hello [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="ee873-187">Connect hello output port of hello **Automobile price data (Raw)** dataset toohello input port of hello [Select Columns in Dataset][select-columns] module.</span></span>

    <span data-ttu-id="ee873-188">![Přidat na plátno experimentu toohello modul "Vyberte sloupců v datové sadě" hello a připojte ho][type-select-columns]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-188">![Add hello "Select Columns in Dataset" module toohello experiment canvas and connect it][type-select-columns]
    </span></span><br/>
 <span data-ttu-id="ee873-189">***Přidat na plátno experimentu toohello modul "Vyberte sloupců v datové sadě" hello a připojte ho***</span><span class="sxs-lookup"><span data-stu-id="ee873-189">***Add hello "Select Columns in Dataset" module toohello experiment canvas and connect it***</span></span>

3. <span data-ttu-id="ee873-190">Klikněte na tlačítko hello [výběr sloupců v datové sadě] [ select-columns] modulu a klikněte na tlačítko **spustit selektor sloupců** v hello **vlastnosti** podokně.</span><span class="sxs-lookup"><span data-stu-id="ee873-190">Click hello [Select Columns in Dataset][select-columns] module and click **Launch column selector** in hello **Properties** pane.</span></span>

    - <span data-ttu-id="ee873-191">Na levé straně hello, klikněte na tlačítko **s pravidly**</span><span class="sxs-lookup"><span data-stu-id="ee873-191">On hello left, click **With rules**</span></span>
    - <span data-ttu-id="ee873-192">V části **Začít s** klikněte na **Všechny sloupce**.</span><span class="sxs-lookup"><span data-stu-id="ee873-192">Under **Begin With**, click **All columns**.</span></span> <span data-ttu-id="ee873-193">To přesměruje [výběr sloupců v datové sadě] [ select-columns] toopass prostřednictvím všechny sloupce hello (s výjimkou tyto sloupce nám o tooexclude).</span><span class="sxs-lookup"><span data-stu-id="ee873-193">This directs [Select Columns in Dataset][select-columns] toopass through all hello columns (except those columns we're about tooexclude).</span></span>
    - <span data-ttu-id="ee873-194">Hello rozevírací seznamy, vyberte **vyloučit** a **názvy sloupců**a klikněte do textového pole hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-194">From hello drop-downs, select **Exclude** and **column names**, and then click inside hello text box.</span></span> <span data-ttu-id="ee873-195">Zobrazí se seznam sloupců.</span><span class="sxs-lookup"><span data-stu-id="ee873-195">A list of columns is displayed.</span></span> <span data-ttu-id="ee873-196">Vyberte **normalized-losses**, a je přidaný toohello textové pole.</span><span class="sxs-lookup"><span data-stu-id="ee873-196">Select **normalized-losses**, and it's added toohello text box.</span></span>
    - <span data-ttu-id="ee873-197">Klikněte na tlačítko hello zaškrtnutí (OK) tooclose hello selektor na sloupců (v pravém dolním hello).</span><span class="sxs-lookup"><span data-stu-id="ee873-197">Click hello check mark (OK) button tooclose hello column selector (on hello lower-right).</span></span>

    <span data-ttu-id="ee873-198">![Spustit selektor sloupců hello a vyloučit hello "normalized-losses"][launch-column-selector]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-198">![Launch hello column selector and exclude hello "normalized-losses" column][launch-column-selector]
    </span></span><br/>
 <span data-ttu-id="ee873-199">***Spustit selektor sloupců hello a vyloučit hello "normalized-losses"***</span><span class="sxs-lookup"><span data-stu-id="ee873-199">***Launch hello column selector and exclude hello "normalized-losses" column***</span></span>

    <span data-ttu-id="ee873-200">Nyní hello podokno vlastností modulu **výběr sloupců v datové sadě** označuje všechny sloupce se bude předávat z hello datové sady kromě **normalized-losses**.</span><span class="sxs-lookup"><span data-stu-id="ee873-200">Now hello properties pane for **Select Columns in Dataset** indicates that it will pass through all columns from hello dataset except **normalized-losses**.</span></span>

    <span data-ttu-id="ee873-201">![Podokno properties Hello ukazuje, že je tento sloupec hello "normalized-losses" vyloučen][showing-excluded-column]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-201">![hello properties pane shows that hello "normalized-losses" column is excluded][showing-excluded-column]
    </span></span><br/>
 <span data-ttu-id="ee873-202">***Podokno properties Hello ukazuje, že je tento sloupec hello "normalized-losses" vyloučen***</span><span class="sxs-lookup"><span data-stu-id="ee873-202">***hello properties pane shows that hello "normalized-losses" column is excluded***</span></span>

    > [!TIP]
    <span data-ttu-id="ee873-203">Můžete přidat komentář modul tooa tak, že dvakrát kliknete na panel hello modulu a zadání textu.</span><span class="sxs-lookup"><span data-stu-id="ee873-203">You can add a comment tooa module by double-clicking hello module and entering text.</span></span> <span data-ttu-id="ee873-204">Díky tomu můžete ihned uvidíte, jaké hello modul provádí v experimentu.</span><span class="sxs-lookup"><span data-stu-id="ee873-204">This can help you see at a glance what hello module is doing in your experiment.</span></span> <span data-ttu-id="ee873-205">V takovém případě klikněte dvakrát na hello [výběr sloupců v datové sadě] [ select-columns] modulu a typu hello komentář "Vyloučit normalized ztráty."</span><span class="sxs-lookup"><span data-stu-id="ee873-205">In this case double-click hello [Select Columns in Dataset][select-columns] module and type hello comment "Exclude normalized losses."</span></span>
    >
    >


    <span data-ttu-id="ee873-206">![Klikněte dvakrát na modul tooadd komentář][add-comment]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-206">![Double-click a module tooadd a comment][add-comment]
    </span></span><br/>
 <span data-ttu-id="ee873-207">***Klikněte dvakrát na modul tooadd komentář***</span><span class="sxs-lookup"><span data-stu-id="ee873-207">***Double-click a module tooadd a comment***</span></span>

3. <span data-ttu-id="ee873-208">Přetáhněte hello [vyčištění chybějících dat] [ clean-missing-data] toohello modulu experimentovat plátno a připojte ho toohello [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="ee873-208">Drag hello [Clean Missing Data][clean-missing-data] module toohello experiment canvas and connect it toohello [Select Columns in Dataset][select-columns] module.</span></span> <span data-ttu-id="ee873-209">V hello **vlastnosti** podokně, vyberte **odstranit celý řádek** pod **režim čištění**.</span><span class="sxs-lookup"><span data-stu-id="ee873-209">In hello **Properties** pane, select **Remove entire row** under **Cleaning mode**.</span></span> <span data-ttu-id="ee873-210">To přesměruje [vyčištění chybějících dat] [ clean-missing-data] tooclean hello dat odstraněním řádků, které mají všechny chybějící hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ee873-210">This directs [Clean Missing Data][clean-missing-data] tooclean hello data by removing rows that have any missing values.</span></span> <span data-ttu-id="ee873-211">Klikněte dvakrát na modul hello a zadejte komentář hello "Odebrat řádků s chybějícími hodnotami."</span><span class="sxs-lookup"><span data-stu-id="ee873-211">Double-click hello module and type hello comment "Remove missing value rows."</span></span>

    <span data-ttu-id="ee873-212">![Nastavit režim čištění hello příliš "odebrat celý řádek" hello "Vyčištění chybějících dat" modulu][set-remove-entire-row]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-212">![Set hello cleaning mode too"Remove entire row" for hello "Clean Missing Data" module][set-remove-entire-row]
    </span></span><br/>
 <span data-ttu-id="ee873-213">***Nastavit režim čištění hello příliš "odebrat celý řádek" hello "Vyčištění chybějících dat" modulu***</span><span class="sxs-lookup"><span data-stu-id="ee873-213">***Set hello cleaning mode too"Remove entire row" for hello "Clean Missing Data" module***</span></span>

4. <span data-ttu-id="ee873-214">Spusťte hello experiment kliknutím **spustit** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-214">Run hello experiment by clicking **RUN** at hello bottom of hello page.</span></span>

    <span data-ttu-id="ee873-215">Po dokončení spuštění experimentu hello mít všechny moduly hello tooindicate zelená značka zaškrtnutí, která se úspěšně dokončila.</span><span class="sxs-lookup"><span data-stu-id="ee873-215">When hello experiment has finished running, all hello modules have a green check mark tooindicate that they finished successfully.</span></span> <span data-ttu-id="ee873-216">Všimněte si také hello **konec běhu** stav v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-216">Notice also hello **Finished running** status in hello upper-right corner.</span></span>

<span data-ttu-id="ee873-217">![Po jeho spuštění, hello experiment by měl vypadat přibližně takto][early-experiment-run]
</span><span class="sxs-lookup"><span data-stu-id="ee873-217">![After running it, hello experiment should look something like this][early-experiment-run]
</span></span><br/><span data-ttu-id="ee873-218">
***Po jeho spuštění, hello experiment by měl vypadat přibližně takto***</span><span class="sxs-lookup"><span data-stu-id="ee873-218">
***After running it, hello experiment should look something like this***</span></span>

> [!TIP]
> <span data-ttu-id="ee873-219">Proč se jsme spustit hello experiment nyní?</span><span class="sxs-lookup"><span data-stu-id="ee873-219">Why did we run hello experiment now?</span></span> <span data-ttu-id="ee873-220">Ve spuštěné hello experimentu předat hello Definice sloupců pro naše data z datové sady hello prostřednictvím hello [výběr sloupců v datové sadě] [ select-columns] modul a prostřednictvím hello [vyčištění chybějících dat] [ clean-missing-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="ee873-220">By running hello experiment, hello column definitions for our data pass from hello dataset, through hello [Select Columns in Dataset][select-columns] module, and through hello [Clean Missing Data][clean-missing-data] module.</span></span> <span data-ttu-id="ee873-221">To znamená, že všechny moduly, které jsme se připojit příliš[vyčištění chybějících dat] [ clean-missing-data] má také stejné informace.</span><span class="sxs-lookup"><span data-stu-id="ee873-221">This means that any modules we connect too[Clean Missing Data][clean-missing-data] will also have this same information.</span></span>

<span data-ttu-id="ee873-222">Všechny, které jsme provedli v experimentu hello až toothis bodu jsou čisté hello data.</span><span class="sxs-lookup"><span data-stu-id="ee873-222">All we have done in hello experiment up toothis point is clean hello data.</span></span> <span data-ttu-id="ee873-223">Pokud chcete datovou sadu tooview hello čištění, klikněte na tlačítko hello zbývajících výstupní port modulu hello [vyčištění chybějících dat] [ clean-missing-data] modul a vyberte **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="ee873-223">If you want tooview hello cleaned dataset, click hello left output port of hello [Clean Missing Data][clean-missing-data] module and select **Visualize**.</span></span> <span data-ttu-id="ee873-224">Všimněte si, že hello **normalized-losses** sloupec již není součástí, a neexistují žádné chybějící hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ee873-224">Notice that hello **normalized-losses** column is no longer included, and there are no missing values.</span></span>

<span data-ttu-id="ee873-225">Teď, když hello data je čistá, jsme připravené toospecify jaké funkce jsme kliknete toouse v hello prediktivního modelu.</span><span class="sxs-lookup"><span data-stu-id="ee873-225">Now that hello data is clean, we're ready toospecify what features we're going toouse in hello predictive model.</span></span>

## <a name="step-3-define-features"></a><span data-ttu-id="ee873-226">Krok 3: Definice příznaků</span><span class="sxs-lookup"><span data-stu-id="ee873-226">Step 3: Define features</span></span>

<span data-ttu-id="ee873-227">Ve strojovém učení se jako *příznaky* označují jednotlivé měřitelné vlastnosti něčeho, co vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="ee873-227">In machine learning, *features* are individual measurable properties of something you’re interested in.</span></span> <span data-ttu-id="ee873-228">V naší datové sadě každý řádek představuje jeden automobil a každý sloupec je příznak daného automobilu.</span><span class="sxs-lookup"><span data-stu-id="ee873-228">In our dataset, each row represents one automobile, and each column is a feature of that automobile.</span></span>

<span data-ttu-id="ee873-229">Hledání dobrý sadu funkcí pro vytvoření prediktivního modelu vyžaduje experimentování a znalost problému hello chcete toosolve.</span><span class="sxs-lookup"><span data-stu-id="ee873-229">Finding a good set of features for creating a predictive model requires experimentation and knowledge about hello problem you want toosolve.</span></span> <span data-ttu-id="ee873-230">Některé funkce, je lepší pro predikci hello cíl než jiné.</span><span class="sxs-lookup"><span data-stu-id="ee873-230">Some features are better for predicting hello target than others.</span></span> <span data-ttu-id="ee873-231">Některé příznaky také mají silnou korelaci s jinými a je možné je odebrat.</span><span class="sxs-lookup"><span data-stu-id="ee873-231">Also, some features have a strong correlation with other features and can be removed.</span></span> <span data-ttu-id="ee873-232">Například city-mpg a highway-mpg jsou úzce související tak, aby jsme udržovat jeden a odebrat hello jiných aniž by to výrazně ovlivnilo hello předpovědi.</span><span class="sxs-lookup"><span data-stu-id="ee873-232">For example, city-mpg and highway-mpg are closely related so we can keep one and remove hello other without significantly affecting hello prediction.</span></span>

<span data-ttu-id="ee873-233">Vytvořme model, který používá podmnožinu funkcí hello v naší datové sadě.</span><span class="sxs-lookup"><span data-stu-id="ee873-233">Let's build a model that uses a subset of hello features in our dataset.</span></span> <span data-ttu-id="ee873-234">Můžete téhle akci vrátit později a vybrat jiné příznaky, znovu spusťte hello experiment a v tématu, jestli nedostanete lepší výsledky.</span><span class="sxs-lookup"><span data-stu-id="ee873-234">You can come back later and select different features, run hello experiment again, and see if you get better results.</span></span> <span data-ttu-id="ee873-235">Ale toostart, zkuste to prosím ještě hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="ee873-235">But toostart, let's try hello following features:</span></span>

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. <span data-ttu-id="ee873-236">Přetáhněte další [výběr sloupců v datové sadě] [ select-columns] toohello modulu experimentovat plátno.</span><span class="sxs-lookup"><span data-stu-id="ee873-236">Drag another [Select Columns in Dataset][select-columns] module toohello experiment canvas.</span></span> <span data-ttu-id="ee873-237">Připojte zbývající výstupní port modulu hello hello [vyčištění chybějících dat] [ clean-missing-data] vstup toohello modulu Dobrý den [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="ee873-237">Connect hello left output port of hello [Clean Missing Data][clean-missing-data] module toohello input of hello [Select Columns in Dataset][select-columns] module.</span></span>

    <span data-ttu-id="ee873-238">![Připojit hello "Vyberte sloupců v datové sadě" modul toohello "Vyčištění chybějících dat" modulu][connect-clean-to-select]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-238">![Connect hello "Select Columns in Dataset" module toohello "Clean Missing Data" module][connect-clean-to-select]
    </span></span><br/>
 <span data-ttu-id="ee873-239">***Připojit hello "Vyberte sloupců v datové sadě" modul toohello "Vyčištění chybějících dat" modulu***</span><span class="sxs-lookup"><span data-stu-id="ee873-239">***Connect hello "Select Columns in Dataset" module toohello "Clean Missing Data" module***</span></span>

2. <span data-ttu-id="ee873-240">Klikněte dvakrát na modul hello a zadejte "Výběr příznaků pro predikci."</span><span class="sxs-lookup"><span data-stu-id="ee873-240">Double-click hello module and type "Select features for prediction."</span></span>

2. <span data-ttu-id="ee873-241">Klikněte na tlačítko **spustit selektor sloupců** v hello **vlastnosti** podokně.</span><span class="sxs-lookup"><span data-stu-id="ee873-241">Click **Launch column selector** in hello **Properties** pane.</span></span>

3. <span data-ttu-id="ee873-242">Klikněte na **S pravidly**.</span><span class="sxs-lookup"><span data-stu-id="ee873-242">Click **With rules**.</span></span>

4. <span data-ttu-id="ee873-243">V části **Začít s** klikněte na **Žádné sloupce**.</span><span class="sxs-lookup"><span data-stu-id="ee873-243">Under **Begin With**, click **No columns**.</span></span> <span data-ttu-id="ee873-244">V řádku filtru hello, vyberte **zahrnout** a **názvy sloupců** a vyberte náš seznam názvů sloupců hello textového pole.</span><span class="sxs-lookup"><span data-stu-id="ee873-244">In hello filter row, select **Include** and **column names** and select our list of column names in hello text box.</span></span> <span data-ttu-id="ee873-245">To přesměruje hello modulu toonot předávání žádné sloupce (funkce), s výjimkou těch, které jsou které určíme hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-245">This directs hello module toonot pass through any columns (features) except hello ones that we specify.</span></span>

5. <span data-ttu-id="ee873-246">Klikněte na tlačítko zaškrtnutí (OK) hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-246">Click hello check mark (OK) button.</span></span>

    <span data-ttu-id="ee873-247">![Vyberte tooinclude hello sloupců (funkce) v předpovědi hello][select-columns-to-include]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-247">![Select hello columns (features) tooinclude in hello prediction][select-columns-to-include]
    </span></span><br/>
 <span data-ttu-id="ee873-248">***Vyberte tooinclude hello sloupců (funkce) v předpovědi hello***</span><span class="sxs-lookup"><span data-stu-id="ee873-248">***Select hello columns (features) tooinclude in hello prediction***</span></span>

<span data-ttu-id="ee873-249">To vytváří filtrovanou sadu dat obsahující pouze funkce hello chceme toopass toohello učení algoritmus, který použijeme v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-249">This produces a filtered dataset containing only hello features we want toopass toohello learning algorithm we'll use in hello next step.</span></span> <span data-ttu-id="ee873-250">Později se můžete vrátit a zkusit jiný výběr příznaků.</span><span class="sxs-lookup"><span data-stu-id="ee873-250">Later, you can return and try again with a different selection of features.</span></span>

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a><span data-ttu-id="ee873-251">Krok 4: Volba a použití algoritmu učení</span><span class="sxs-lookup"><span data-stu-id="ee873-251">Step 4: Choose and apply a learning algorithm</span></span>

<span data-ttu-id="ee873-252">Teď, když hello data jsou připravena, tvorba prediktivního modelu sestává z trénování a testování.</span><span class="sxs-lookup"><span data-stu-id="ee873-252">Now that hello data is ready, constructing a predictive model consists of training and testing.</span></span> <span data-ttu-id="ee873-253">Model hello tootrain naše data použijeme, a pak otestujeme hello model toosee jak blízko je možné toopredict ceny.</span><span class="sxs-lookup"><span data-stu-id="ee873-253">We'll use our data tootrain hello model, and then we'll test hello model toosee how closely it's able toopredict prices.</span></span>
<!-- For now, don't worry about *why* we need tootrain and then test a model.-->

<span data-ttu-id="ee873-254">*Klasifikace* a *regrese* jsou dva typy technik strojového učení se supervizí.</span><span class="sxs-lookup"><span data-stu-id="ee873-254">*Classification* and *regression* are two types of supervised machine learning algorithms.</span></span> <span data-ttu-id="ee873-255">Klasifikace předpovídá odpověď na základě definované sady kategorií, třeba barvy (červená, modrá nebo zelená).</span><span class="sxs-lookup"><span data-stu-id="ee873-255">Classification predicts an answer from a defined set of categories, such as a color (red, blue, or green).</span></span> <span data-ttu-id="ee873-256">Regrese je použité toopredict číslo.</span><span class="sxs-lookup"><span data-stu-id="ee873-256">Regression is used toopredict a number.</span></span>

<span data-ttu-id="ee873-257">Vzhledem k tomu, že chceme toopredict ceníku, který je číslo, použijeme regresní algoritmus.</span><span class="sxs-lookup"><span data-stu-id="ee873-257">Because we want toopredict price, which is a number, we'll use a regression algorithm.</span></span> <span data-ttu-id="ee873-258">Pro tento příklad použijeme jednoduchý model *lineární regrese*.</span><span class="sxs-lookup"><span data-stu-id="ee873-258">For this example, we'll use a simple *linear regression* model.</span></span>

> [!TIP]
> <span data-ttu-id="ee873-259">Pokud chcete více informací o různé typy algoritmů strojového učení toolearn a pokud toouse je první video hello může zobrazit v hello vědecké zpracování dat pro začátečníky řady [hello pět otázky, odpovědi vědecké účely data](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span><span class="sxs-lookup"><span data-stu-id="ee873-259">If you want toolearn more about different types of machine learning algorithms and when toouse them, you might view hello first video in hello Data Science for Beginners series, [hello five questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span></span> <span data-ttu-id="ee873-260">Může také prohlédnout hello infografice [strojového učení základy s příklady algoritmus](machine-learning-basics-infographic-with-algorithm-examples.md), nebo se podívejte se na hello [strojového učení rychlý přehled algoritmů](machine-learning-algorithm-cheat-sheet.md).</span><span class="sxs-lookup"><span data-stu-id="ee873-260">You might also look at hello infographic [Machine learning basics with algorithm examples](machine-learning-basics-infographic-with-algorithm-examples.md), or check out hello [Machine learning algorithm cheat sheet](machine-learning-algorithm-cheat-sheet.md).</span></span>

<span data-ttu-id="ee873-261">Jsme cvičení modelu hello tím, že se sada dat, která zahrnuje hello ceny.</span><span class="sxs-lookup"><span data-stu-id="ee873-261">We train hello model by giving it a set of data that includes hello price.</span></span> <span data-ttu-id="ee873-262">Hello model kontroluje hello dat a vyhledat korelací mezi automobilu na funkce a jeho cenu.</span><span class="sxs-lookup"><span data-stu-id="ee873-262">hello model scans hello data and look for correlations between an automobile's features and its price.</span></span> <span data-ttu-id="ee873-263">Potom otestujeme hello modelu – jsme budete poskytněte sadu funkcí pro automobilů, které jsme se seznámíte s a v tématu Jak zavřít hello modelu dodává toopredicting hello známé ceny.</span><span class="sxs-lookup"><span data-stu-id="ee873-263">Then we'll test hello model - we'll give it a set of features for automobiles we're familiar with and see how close hello model comes toopredicting hello known price.</span></span>

<span data-ttu-id="ee873-264">Naše data použijeme pro trénování modelu hello i testování rozdělením hello data do samostatných trénování a testování datové sady.</span><span class="sxs-lookup"><span data-stu-id="ee873-264">We'll use our data for both training hello model and testing it by splitting hello data into separate training and testing datasets.</span></span>

1. <span data-ttu-id="ee873-265">Vyberte a přetáhněte ji hello [rozdělení dat] [ split] toohello modulu experimentovat plátno a připojte ho poslední toohello [výběr sloupců v datové sadě] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="ee873-265">Select and drag hello [Split Data][split] module toohello experiment canvas and connect it toohello last [Select Columns in Dataset][select-columns] module.</span></span>

2. <span data-ttu-id="ee873-266">Klikněte na tlačítko hello [rozdělení dat] [ split] modulu tooselect ho.</span><span class="sxs-lookup"><span data-stu-id="ee873-266">Click hello [Split Data][split] module tooselect it.</span></span> <span data-ttu-id="ee873-267">Najde hello **podíl řádků v hello nejprve výstupní datovou sadu** (v hello **vlastnosti** toohello podokně napravo od plátna hello) a nastavte ji too0.75.</span><span class="sxs-lookup"><span data-stu-id="ee873-267">Find hello **Fraction of rows in hello first output dataset** (in hello **Properties** pane toohello right of hello canvas) and set it too0.75.</span></span> <span data-ttu-id="ee873-268">Tímto způsobem jsme budete používat 75 procent hello data tootrain hello modelu a Ponecháme 25 procent pro testování (později můžete experimentovat s použitím různých procenta).</span><span class="sxs-lookup"><span data-stu-id="ee873-268">This way, we'll use 75 percent of hello data tootrain hello model, and hold back 25 percent for testing (later, you can experiment with using different percentages).</span></span>

    <span data-ttu-id="ee873-269">![Sada hello rozdělte podílem too0.75 modul "Rozdělení dat" hello][set-split-data-percentage]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-269">![Set hello split fraction of hello "Split Data" module too0.75][set-split-data-percentage]
    </span></span><br/>
 <span data-ttu-id="ee873-270">***Sada hello rozdělte podílem too0.75 modul "Rozdělení dat" hello***</span><span class="sxs-lookup"><span data-stu-id="ee873-270">***Set hello split fraction of hello "Split Data" module too0.75***</span></span>

    > [!TIP]
    > <span data-ttu-id="ee873-271">Změnou hello **náhodná počáteční hodnota** parametr, můžete vytvořit různé náhodné vzorky pro trénování a testování.</span><span class="sxs-lookup"><span data-stu-id="ee873-271">By changing hello **Random seed** parameter, you can produce different random samples for training and testing.</span></span> <span data-ttu-id="ee873-272">Tento parametr řídí hello synchronizace replik indexů hello pseudonáhodného generátoru čísel.</span><span class="sxs-lookup"><span data-stu-id="ee873-272">This parameter controls hello seeding of hello pseudo-random number generator.</span></span>

2. <span data-ttu-id="ee873-273">Spusťte hello experiment.</span><span class="sxs-lookup"><span data-stu-id="ee873-273">Run hello experiment.</span></span> <span data-ttu-id="ee873-274">Při spuštění experimentu hello hello [výběr sloupců v datové sadě] [ select-columns] a [rozdělení dat] [ split] moduly předat toohello definice sloupce moduly jsme přidali další.</span><span class="sxs-lookup"><span data-stu-id="ee873-274">When hello experiment is run, hello [Select Columns in Dataset][select-columns] and [Split Data][split] modules pass column definitions toohello modules we'll be adding next.</span></span>  

3. <span data-ttu-id="ee873-275">hello tooselect učení algoritmu, rozbalte položku hello **Machine Learning** kategorie v hello modulu palety toohello vlevo hello plátno a potom rozbalte **inicializovat Model**.</span><span class="sxs-lookup"><span data-stu-id="ee873-275">tooselect hello learning algorithm, expand hello **Machine Learning** category in hello module palette toohello left of hello canvas, and then expand **Initialize Model**.</span></span> <span data-ttu-id="ee873-276">Zobrazí několik kategorií modulů, které mohou být algoritmů strojového učení použité tooinitialize.</span><span class="sxs-lookup"><span data-stu-id="ee873-276">This displays several categories of modules that can be used tooinitialize machine learning algorithms.</span></span> <span data-ttu-id="ee873-277">Pro tento experiment vyberte hello [lineární regrese] [ linear-regression] modulu v části hello **regrese** kategorii a přetáhněte jej na plátno experimentu toohello.</span><span class="sxs-lookup"><span data-stu-id="ee873-277">For this experiment, select hello [Linear Regression][linear-regression] module under hello **Regression** category, and drag it toohello experiment canvas.</span></span>
<span data-ttu-id="ee873-278">(Můžete také vyhledat hello modulu zadáním "lineární regrese" do pole Hledat palety hello).</span><span class="sxs-lookup"><span data-stu-id="ee873-278">(You can also find hello module by typing "linear regression" in hello palette Search box.)</span></span>

4. <span data-ttu-id="ee873-279">Najděte a přetáhněte hello [Train Model] [ train-model] toohello modulu experimentovat plátno.</span><span class="sxs-lookup"><span data-stu-id="ee873-279">Find and drag hello [Train Model][train-model] module toohello experiment canvas.</span></span> <span data-ttu-id="ee873-280">Připojit hello výstup hello [lineární regrese] [ linear-regression] toohello modulu zbývajících vstup hello [Train Model] [ train-model] modul a připojte hello cvičení výstup dat (left port) modulu hello [rozdělení dat] [ split] modulu toohello správný vstup hello [Train Model] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="ee873-280">Connect hello output of hello [Linear Regression][linear-regression] module toohello left input of hello [Train Model][train-model] module, and connect hello training data output (left port) of hello [Split Data][split] module toohello right input of hello [Train Model][train-model] module.</span></span>

    <span data-ttu-id="ee873-281">![Připojit hello "Train Model" modul tooboth hello "Lineární regrese" a "Rozdělení dat" moduly][connect-train-model]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-281">![Connect hello "Train Model" module tooboth hello "Linear Regression" and "Split Data" modules][connect-train-model]
    </span></span><br/>
 <span data-ttu-id="ee873-282">***Připojit hello "Train Model" modul tooboth hello "Lineární regrese" a "Rozdělení dat" moduly***</span><span class="sxs-lookup"><span data-stu-id="ee873-282">***Connect hello "Train Model" module tooboth hello "Linear Regression" and "Split Data" modules***</span></span>

5. <span data-ttu-id="ee873-283">Klikněte na tlačítko hello [Train Model] [ train-model] modulu, klikněte na tlačítko **spustit selektor sloupců** v hello **vlastnosti** podokně a pak vyberte hello **cena** sloupce.</span><span class="sxs-lookup"><span data-stu-id="ee873-283">Click hello [Train Model][train-model] module, click **Launch column selector** in hello **Properties** pane, and then select hello **price** column.</span></span> <span data-ttu-id="ee873-284">Toto je, že je naše model probíhající toopredict hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-284">This is hello value that our model is going toopredict.</span></span>

    <span data-ttu-id="ee873-285">Vyberte hello **cena** sloupec v selektor sloupců hello přesunutím z hello **dostupné sloupce** seznamu toohello **vybrané sloupce** seznamu.</span><span class="sxs-lookup"><span data-stu-id="ee873-285">You select hello **price** column in hello column selector by moving it from hello **Available columns** list toohello **Selected columns** list.</span></span>

    <span data-ttu-id="ee873-286">![Vyberte sloupec hello ceny pro modul "Train Model" hello][select-price-column]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-286">![Select hello price column for hello "Train Model" module][select-price-column]
    </span></span><br/>
 <span data-ttu-id="ee873-287">***Vyberte sloupec hello ceny pro modul "Train Model" hello***</span><span class="sxs-lookup"><span data-stu-id="ee873-287">***Select hello price column for hello "Train Model" module***</span></span>

6. <span data-ttu-id="ee873-288">Spusťte hello experiment.</span><span class="sxs-lookup"><span data-stu-id="ee873-288">Run hello experiment.</span></span>

<span data-ttu-id="ee873-289">Nyní je k dispozici vyškolení regresní model, který lze použít tooscore nové automobilů data toomake cena předpovědi.</span><span class="sxs-lookup"><span data-stu-id="ee873-289">We now have a trained regression model that can be used tooscore new automobile data toomake price predictions.</span></span>

<span data-ttu-id="ee873-290">![Po spuštění hello experiment by teď měl vypadat přibližně takto][second-experiment-run]
</span><span class="sxs-lookup"><span data-stu-id="ee873-290">![After running, hello experiment should now look something like this][second-experiment-run]
</span></span><br/><span data-ttu-id="ee873-291">
***Po spuštění hello experiment by teď měl vypadat přibližně takto***</span><span class="sxs-lookup"><span data-stu-id="ee873-291">
***After running, hello experiment should now look something like this***</span></span>

## <a name="step-5-predict-new-automobile-prices"></a><span data-ttu-id="ee873-292">Krok 5: Předpověď cen nových automobilů</span><span class="sxs-lookup"><span data-stu-id="ee873-292">Step 5: Predict new automobile prices</span></span>

<span data-ttu-id="ee873-293">Teď, když jsme natrénovali hello model pomocí 75 procent dat, abychom je mohli použít tooscore hello zbylých 25 procent dat toosee hello, jak dobře model funguje.</span><span class="sxs-lookup"><span data-stu-id="ee873-293">Now that we've trained hello model using 75 percent of our data, we can use it tooscore hello other 25 percent of hello data toosee how well our model functions.</span></span>

1. <span data-ttu-id="ee873-294">Najděte a přetáhněte hello [Score Model] [ score-model] toohello modulu experimentovat plátno.</span><span class="sxs-lookup"><span data-stu-id="ee873-294">Find and drag hello [Score Model][score-model] module toohello experiment canvas.</span></span> <span data-ttu-id="ee873-295">Připojit hello výstup hello [Train Model] [ train-model] toohello modulu zbývajících vstupní port [Score Model][score-model].</span><span class="sxs-lookup"><span data-stu-id="ee873-295">Connect hello output of hello [Train Model][train-model] module toohello left input port of [Score Model][score-model].</span></span> <span data-ttu-id="ee873-296">Připojit hello výstup testovacích dat (pravý port) z hello [rozdělení dat] [ split] modulu toohello právo vstupní port [Score Model][score-model].</span><span class="sxs-lookup"><span data-stu-id="ee873-296">Connect hello test data output (right port) of hello [Split Data][split] module toohello right input port of [Score Model][score-model].</span></span>

    <span data-ttu-id="ee873-297">![Připojit hello "Určení skóre modelu" modul tooboth hello "Train Model" a "Rozdělení dat" moduly][connect-score-model]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-297">![Connect hello "Score Model" module tooboth hello "Train Model" and "Split Data" modules][connect-score-model]
    </span></span><br/>
 <span data-ttu-id="ee873-298">***Připojit hello "Určení skóre modelu" modul tooboth hello "Train Model" a "Rozdělení dat" moduly***</span><span class="sxs-lookup"><span data-stu-id="ee873-298">***Connect hello "Score Model" module tooboth hello "Train Model" and "Split Data" modules***</span></span>

2. <span data-ttu-id="ee873-299">Spusťte hello experiment a zobrazit výstup hello hello [Score Model] [ score-model] modulu (klikněte na výstupní port hello [Score Model] [ score-model] a vyberte **Vizualizovat**).</span><span class="sxs-lookup"><span data-stu-id="ee873-299">Run hello experiment and view hello output from hello [Score Model][score-model] module (click hello output port of [Score Model][score-model] and select **Visualize**).</span></span> <span data-ttu-id="ee873-300">Zobrazí výstup Hello hello predikované hodnoty ceny a známé hodnoty hello testovací data hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-300">hello output shows hello predicted values for price and hello known values from hello test data.</span></span>  

    <span data-ttu-id="ee873-301">![Výstup hello "" modulu pro stanovení skóre][score-model-output]
    </span><span class="sxs-lookup"><span data-stu-id="ee873-301">![Output of hello "Score Model" module][score-model-output]
    </span></span><br/>
 <span data-ttu-id="ee873-302">***Výstup hello "" modulu pro stanovení skóre***</span><span class="sxs-lookup"><span data-stu-id="ee873-302">***Output of hello "Score Model" module***</span></span>

3. <span data-ttu-id="ee873-303">Nakonec testujeme hello kvality výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-303">Finally, we test hello quality of hello results.</span></span> <span data-ttu-id="ee873-304">Vyberte a přetáhněte ji hello [Evaluate Model] [ evaluate-model] toohello modulu experimentovat plátno a připojit hello výstup hello [Score Model] [ score-model] modul toohello zbývajících vstup z [Evaluate Model][evaluate-model].</span><span class="sxs-lookup"><span data-stu-id="ee873-304">Select and drag hello [Evaluate Model][evaluate-model] module toohello experiment canvas, and connect hello output of hello [Score Model][score-model] module toohello left input of [Evaluate Model][evaluate-model].</span></span>

    > [!TIP]
    > <span data-ttu-id="ee873-305">Existují dva vstupní porty na hello [Evaluate Model] [ evaluate-model] modulu může být dva modely použité toocompare vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="ee873-305">There are two input ports on hello [Evaluate Model][evaluate-model] module because it can be used toocompare two models side by side.</span></span> <span data-ttu-id="ee873-306">Později, můžete přidat jiný algoritmus toohello experiment a používat [Evaluate Model] [ evaluate-model] toosee, která poskytuje lepší výsledky.</span><span class="sxs-lookup"><span data-stu-id="ee873-306">Later, you can add another algorithm toohello experiment and use [Evaluate Model][evaluate-model] toosee which one gives better results.</span></span>

4. <span data-ttu-id="ee873-307">Spusťte hello experiment.</span><span class="sxs-lookup"><span data-stu-id="ee873-307">Run hello experiment.</span></span>

<span data-ttu-id="ee873-308">výstup hello tooview hello [Evaluate Model] [ evaluate-model] modulu, klikněte na výstupní port hello a pak vyberte **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="ee873-308">tooview hello output from hello [Evaluate Model][evaluate-model] module, click hello output port, and then select **Visualize**.</span></span>

<span data-ttu-id="ee873-309">![Výsledky vyhodnocení hello experimentu][evaluation-results]
</span><span class="sxs-lookup"><span data-stu-id="ee873-309">![Evaluation results for hello experiment][evaluation-results]
</span></span><br/><span data-ttu-id="ee873-310">
***Výsledky vyhodnocení hello experimentu***</span><span class="sxs-lookup"><span data-stu-id="ee873-310">
***Evaluation results for hello experiment***</span></span>

<span data-ttu-id="ee873-311">pro náš model se zobrazuje Hello následující statistiky:</span><span class="sxs-lookup"><span data-stu-id="ee873-311">hello following statistics are shown for our model:</span></span>

- <span data-ttu-id="ee873-312">**Střední absolutní chyba** (MAE): průměr absolutních chyb hello ( *chyba* je hello rozdíl mezi hello předpovědět hodnotu a skutečnou hodnotu hello).</span><span class="sxs-lookup"><span data-stu-id="ee873-312">**Mean Absolute Error** (MAE): hello average of absolute errors (an *error* is hello difference between hello predicted value and hello actual value).</span></span>
- <span data-ttu-id="ee873-313">**Odmocnina střední kvadratické chyby** (RMSE): hello druhou odmocninu čísla hello průměr kvadratických chyb předpovědí na hello testovací datové sady.</span><span class="sxs-lookup"><span data-stu-id="ee873-313">**Root Mean Squared Error** (RMSE): hello square root of hello average of squared errors of predictions made on hello test dataset.</span></span>
- <span data-ttu-id="ee873-314">**Relativní absolutní chyba**: průměr absolutních chyb relativních toohello absolutnímu rozdílu mezi skutečnými hodnotami a průměrem všech skutečných hodnot hello hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-314">**Relative Absolute Error**: hello average of absolute errors relative toohello absolute difference between actual values and hello average of all actual values.</span></span>
- <span data-ttu-id="ee873-315">**Relativní kvadratická chyba**: průměr kvadratických chyb relativních toohello hello spolehlivosti rozdíl mezi hello skutečnými hodnotami a průměrem všech skutečných hodnot hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-315">**Relative Squared Error**: hello average of squared errors relative toohello squared difference between hello actual values and hello average of all actual values.</span></span>
- <span data-ttu-id="ee873-316">**Koeficient spolehlivosti**: také označované jako hello **hodnota spolehlivosti R**, jedná se tedy statistická metrika označující, jak dobře model vyhovuje hello data.</span><span class="sxs-lookup"><span data-stu-id="ee873-316">**Coefficient of Determination**: Also known as hello **R squared value**, this is a statistical metric indicating how well a model fits hello data.</span></span>

<span data-ttu-id="ee873-317">Pro každou chyby hello je lepší statistiky, menší.</span><span class="sxs-lookup"><span data-stu-id="ee873-317">For each of hello error statistics, smaller is better.</span></span> <span data-ttu-id="ee873-318">Menší hodnota označuje, zda text hello předpověď přesněji odpovídá skutečným hodnotám hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-318">A smaller value indicates that hello predictions more closely match hello actual values.</span></span> <span data-ttu-id="ee873-319">Pro **koeficientu spolehlivosti**, hello blíže jeho hodnota může být tooone (1.0), hello lepší hello předpovědi.</span><span class="sxs-lookup"><span data-stu-id="ee873-319">For **Coefficient of Determination**, hello closer its value is tooone (1.0), hello better hello predictions.</span></span>

## <a name="final-experiment"></a><span data-ttu-id="ee873-320">Konečný experiment</span><span class="sxs-lookup"><span data-stu-id="ee873-320">Final experiment</span></span>

<span data-ttu-id="ee873-321">Hello konečný experiment by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="ee873-321">hello final experiment should look something like this:</span></span>

<span data-ttu-id="ee873-322">![Konečný experiment Hello][complete-linear-regression-experiment]
</span><span class="sxs-lookup"><span data-stu-id="ee873-322">![hello final experiment][complete-linear-regression-experiment]
</span></span><br/><span data-ttu-id="ee873-323">
***Konečný experiment Hello***</span><span class="sxs-lookup"><span data-stu-id="ee873-323">
***hello final experiment***</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee873-324">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ee873-324">Next steps</span></span>

<span data-ttu-id="ee873-325">Teď, když jste dokončili hello první kurz strojového učení a máte nastaven experiment, můžete pokračovat tooimprove hello modelu a poté ji nasadit jako prediktivní webovou službu.</span><span class="sxs-lookup"><span data-stu-id="ee873-325">Now that you've completed hello first machine learning tutorial and have your experiment set up, you can continue tooimprove hello model and then deploy it as a predictive web service.</span></span>

- <span data-ttu-id="ee873-326">**Iteraci modelu hello tooimprove tootry** – například můžete změnit hello funkcích, které používáte ve vaší předpovědi.</span><span class="sxs-lookup"><span data-stu-id="ee873-326">**Iterate tootry tooimprove hello model** - For example, you can change hello features you use in your prediction.</span></span> <span data-ttu-id="ee873-327">Nebo můžete upravit vlastnosti hello hello [lineární regrese] [ linear-regression] algoritmus nebo se pokuste úplně jiný algoritmus.</span><span class="sxs-lookup"><span data-stu-id="ee873-327">Or you can modify hello properties of hello [Linear Regression][linear-regression] algorithm or try a different algorithm altogether.</span></span> <span data-ttu-id="ee873-328">I můžete přidat více experimentu strojového učení a algoritmy tooyour najednou a porovnat je pomocí hello [Evaluate Model] [ evaluate-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="ee873-328">You can even add multiple machine learning algorithms tooyour experiment at one time and compare two of them by using hello [Evaluate Model][evaluate-model] module.</span></span>
<span data-ttu-id="ee873-329">Příklad jak toocompare více modelů v jednom experimentu, najdete v části [porovnat Regresory](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) v hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="ee873-329">For an example of how toocompare multiple models in a single experiment, see [Compare Regressors](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) in hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span>

    > [!TIP]
    > <span data-ttu-id="ee873-330">toocopy kteroukoli iteraci experimentu, použijte hello **uložit jako** tlačítko v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-330">toocopy any iteration of your experiment, use hello **SAVE AS** button at hello bottom of hello page.</span></span> <span data-ttu-id="ee873-331">Zobrazí všechny hello iterace experimentu kliknutím **zobrazit HISTORII BĚHŮ** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="ee873-331">You can see all hello iterations of your experiment by clicking **VIEW RUN HISTORY** at hello bottom of hello page.</span></span> <span data-ttu-id="ee873-332">Další podrobnosti najdete v tématu [Správa iterací experimentů v nástroji Azure Machine Learning Studio][runhistory].</span><span class="sxs-lookup"><span data-stu-id="ee873-332">For more details, see [Manage experiment iterations in Azure Machine Learning Studio][runhistory].</span></span>

[runhistory]: machine-learning-manage-experiment-iterations.md

- <span data-ttu-id="ee873-333">**Nasazení modelu hello jako prediktivní webovou službu** – Pokud jste se svým modelem spokojeni můžete nasadit jako toobe webové služby použít cen automobilů toopredict pomocí nová data.</span><span class="sxs-lookup"><span data-stu-id="ee873-333">**Deploy hello model as a predictive web service** - When you're satisfied with your model, you can deploy it as a web service toobe used toopredict automobile prices by using new data.</span></span> <span data-ttu-id="ee873-334">Další podrobnosti najdete v tématu [Nasazení webové služby Azure Machine Learning][publish].</span><span class="sxs-lookup"><span data-stu-id="ee873-334">For more details, see [Deploy an Azure Machine Learning web service][publish].</span></span>

[publish]: machine-learning-publish-a-machine-learning-web-service.md

<span data-ttu-id="ee873-335">Chcete toolearn další?</span><span class="sxs-lookup"><span data-stu-id="ee873-335">Want toolearn more?</span></span> <span data-ttu-id="ee873-336">Rozsáhlejší a podrobnější návod hello procesu vytváření, trénování, vyhodnocování a nasazení modelu najdete v tématu [vývoji prediktivního řešení pomocí Azure Machine Learning][walkthrough].</span><span class="sxs-lookup"><span data-stu-id="ee873-336">For a more extensive and detailed walkthrough of hello process of creating, training, scoring, and deploying a model, see [Develop a predictive solution by using Azure Machine Learning][walkthrough].</span></span>

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[sign-in-to-studio]: ./media/machine-learning-create-experiment/sign-in-to-studio.png
[rename-experiment]: ./media/machine-learning-create-experiment/rename-experiment.png
[visualize-auto-data]:./media/machine-learning-create-experiment/visualize-auto-data.png
[select-visualize]: ./media/machine-learning-create-experiment/select-visualize.png
[showing-excluded-column]:./media/machine-learning-create-experiment/showing-excluded-column.png
[set-remove-entire-row]:./media/machine-learning-create-experiment/set-remove-entire-row.png
[early-experiment-run]:./media/machine-learning-create-experiment/early-experiment-run.png
[select-columns-to-include]:./media/machine-learning-create-experiment/select-columns-to-include.png
[second-experiment-run]:./media/machine-learning-create-experiment/second-experiment-run.png
[connect-score-model]:./media/machine-learning-create-experiment/connect-score-model.png
[evaluation-results]:./media/machine-learning-create-experiment/evaluation-results.png
[complete-linear-regression-experiment]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
[type-automobile]:./media/machine-learning-create-experiment/type-automobile.png
[type-select-columns]:./media/machine-learning-create-experiment/type-select-columns.png
[launch-column-selector]:./media/machine-learning-create-experiment/launch-column-selector.png
[add-comment]:./media/machine-learning-create-experiment/add-comment.png
[connect-clean-to-select]:./media/machine-learning-create-experiment/connect-clean-to-select.png

[set-split-data-percentage]:./media/machine-learning-create-experiment/set-split-data-percentage.png

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
[connect-train-model]:./media/machine-learning-create-experiment/connect-train-model.png
[select-price-column]:./media/machine-learning-create-experiment/select-price-column.png

[score-model-output]:./media/machine-learning-create-experiment/score-model-output.png

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
