---
title: "Jednoduchý experiment v nástroji Machine Learning Studio | Dokumentace Microsoftu"
description: "Tento kurz Machine Learningu vás provede jednoduchým experimentem z oblasti datové vědy. Pomocí regresního algoritmu předpovíme cenu automobilu."
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
ms.openlocfilehash: 0539e9d04c61d35de56a29d350c07654c095c576
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a><span data-ttu-id="402eb-105">Kurz Machine Learningu: Vytvoření prvního experimentu z oblasti datové vědy v nástroji Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="402eb-105">Machine learning tutorial: Create your first data science experiment in Azure Machine Learning Studio</span></span>

<span data-ttu-id="402eb-106">Pokud jste **Azure Machine Learning Studio** nikdy nepoužívali, je tento kurz právě pro vás.</span><span class="sxs-lookup"><span data-stu-id="402eb-106">If you've never used **Azure Machine Learning Studio** before, this tutorial is for you.</span></span>

<span data-ttu-id="402eb-107">V tomto kurzu vás provedeme prvním použitím sady Studio a ukážeme vám, jak vytvořit experiment strojového učení.</span><span class="sxs-lookup"><span data-stu-id="402eb-107">In this tutorial, we'll walk through how to use Studio for the first time to create a machine learning experiment.</span></span> <span data-ttu-id="402eb-108">Tento experiment otestuje analytický model, který bude předpovídat cenu automobilu podle různých proměnných, třeba značky a technických specifikací.</span><span class="sxs-lookup"><span data-stu-id="402eb-108">The experiment will test an analytical model that predicts the price of an automobile based on different variables such as make and technical specifications.</span></span>

> [!NOTE]
> <span data-ttu-id="402eb-109">V tomto kurzu si ukážeme základy toho, jak pomocí myši přetáhnout moduly do experimentu, vzájemně je propojit, spustit experiment a prohlédnout si výsledky.</span><span class="sxs-lookup"><span data-stu-id="402eb-109">This tutorial shows you the basics of how to drag-and-drop modules onto your experiment, connect them together, run the experiment, and look at the results.</span></span> <span data-ttu-id="402eb-110">Nebudeme se zabývat obecnými tématy strojového učení ani tím, jak vybírat a využívat více než 100 předdefinovaných algoritmů a modulů pro manipulaci s daty, které jsou součástí sady Studio.</span><span class="sxs-lookup"><span data-stu-id="402eb-110">We're not going to discuss the general topic of machine learning or how to select and use the 100+ built-in algorithms and data manipulation modules included in Studio.</span></span>
>
><span data-ttu-id="402eb-111">Pokud se strojovým učením teprve začínáte, může pro vás být dobrým výchozím bodem seriál videí [Data Science for Beginners (Datová věda pro začátečníky)](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span><span class="sxs-lookup"><span data-stu-id="402eb-111">If you're new to machine learning, the video series [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) might be a good place to start.</span></span> <span data-ttu-id="402eb-112">Tento seriál videí představuje vynikající úvod do strojového učení s využitím jazyka a konceptů každodenního života.</span><span class="sxs-lookup"><span data-stu-id="402eb-112">This video series is a great introduction to machine learning using everyday language and concepts.</span></span>
>
><span data-ttu-id="402eb-113">Pokud už strojové učení znáte, ale hledáte obecnější informace o sadě Machine Learning Studio a algoritmech strojového učení, které obsahuje, vhodné zdroje informací můžete najít tady:</span><span class="sxs-lookup"><span data-stu-id="402eb-113">If you're familiar with machine learning, but you're looking for more general information about Machine Learning Studio, and the machine learning algorithms it contains, here are some good resources:</span></span>
>
- [<span data-ttu-id="402eb-114">Co je Machine Learning Studio?</span><span class="sxs-lookup"><span data-stu-id="402eb-114">What is Machine Learning Studio?</span></span>](machine-learning-what-is-ml-studio.md) <span data-ttu-id="402eb-115">- Toto téma obsahuje obecný přehled sady Studio.</span><span class="sxs-lookup"><span data-stu-id="402eb-115">- This is a high-level overview of Studio.</span></span>
- <span data-ttu-id="402eb-116">[Základy služby Machine Learning s příklady algoritmů](machine-learning-basics-infographic-with-algorithm-examples.md) – Tato infografika je užitečná k tomu, abyste se blíže seznámili s různými algoritmy strojového učení, které jsou součástí sady Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="402eb-116">[Machine learning basics with algorithm examples](machine-learning-basics-infographic-with-algorithm-examples.md) - This infographic is useful if you want to learn more about the different types of machine learning algorithms included with Machine Learning Studio.</span></span>
- <span data-ttu-id="402eb-117">[Průvodce službou Machine Learning](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) – Tato příručka zahrnuje podobné informace jako infografika uvedená výše, ale v interaktivním formátu.</span><span class="sxs-lookup"><span data-stu-id="402eb-117">[Machine Learning Guide](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) - This guide covers similar information as the infographic above, but in an interactive format.</span></span>
- <span data-ttu-id="402eb-118">[Stručný přehled algoritmů strojového učení](machine-learning-algorithm-cheat-sheet.md) a [Jak zvolit algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) – Tento plakát ke stažení a doprovodný článek podrobně rozebírají algoritmy sady Studio.</span><span class="sxs-lookup"><span data-stu-id="402eb-118">[Machine learning algorithm cheat sheet](machine-learning-algorithm-cheat-sheet.md) and [How to choose algorithms for Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) - This downloadable poster and accompanying article discuss the Studio algorithms in depth.</span></span>
- <span data-ttu-id="402eb-119">[Machine Learning Studio: Nápověda k algoritmům a modulům](https://msdn.microsoft.com/library/azure/dn905974.aspx) – Tato kompletní reference pro všechny moduly sady Studio obsahuje i algoritmy strojového učení.</span><span class="sxs-lookup"><span data-stu-id="402eb-119">[Machine Learning Studio: Algorithm and Module Help](https://msdn.microsoft.com/library/azure/dn905974.aspx) - This is the complete reference for all Studio modules, including machine learning algorithms,</span></span>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a><span data-ttu-id="402eb-120">V čem je přínos nástroje Machine Learning Studio?</span><span class="sxs-lookup"><span data-stu-id="402eb-120">How does Machine Learning Studio help?</span></span>

<span data-ttu-id="402eb-121">Machine Learning Studio usnadňuje vytvoření pokusu přetahováním předem naprogramovaných modulů s technikami prediktivního modelování.</span><span class="sxs-lookup"><span data-stu-id="402eb-121">Machine Learning Studio makes it easy to set up an experiment using drag-and-drop modules preprogrammed with predictive modeling techniques.</span></span>

<span data-ttu-id="402eb-122">Pomocí interaktivního vizuálního pracovního prostoru můžete přetáhnout ***datové sady*** a ***moduly*** analýz na interaktivní plátno.</span><span class="sxs-lookup"><span data-stu-id="402eb-122">Using an interactive, visual workspace, you drag-and-drop ***datasets*** and analysis ***modules*** onto an interactive canvas.</span></span> <span data-ttu-id="402eb-123">Vzájemně je propojíte a vytvoříte ***experiment***, který spustíte v sadě Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="402eb-123">You connect them together to form an ***experiment*** that you run in Machine Learning Studio.</span></span>
<span data-ttu-id="402eb-124">***Vytvoříte model***, ***natrénujete ho*** a potom ***stanovíte skóre a otestujete ho***.</span><span class="sxs-lookup"><span data-stu-id="402eb-124">You ***create a model***, ***train the model***, and ***score and test the model***.</span></span>

<span data-ttu-id="402eb-125">Můžete iterovat návrh modelu, upravovat experiment a spouštět ho, dokud nedává výsledky, jaké potřebujete.</span><span class="sxs-lookup"><span data-stu-id="402eb-125">You can iterate on your model design, editing the experiment and running it until it gives you the results you're looking for.</span></span> <span data-ttu-id="402eb-126">Když je model hotový, můžete ho publikovat jako ***webovou službu***, aby mu i ostatní mohli zasílat nová data a získávat na oplátku predikce.</span><span class="sxs-lookup"><span data-stu-id="402eb-126">When your model is ready, you can publish it as a ***web service*** so that others can send it new data and get predictions in return.</span></span>

## <a name="open-machine-learning-studio"></a><span data-ttu-id="402eb-127">Otevřít Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="402eb-127">Open Machine Learning Studio</span></span>

<span data-ttu-id="402eb-128">Pokud chcete začít se sadou Studio, přejděte na [https://studio.azureml.net](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="402eb-128">To get started with Studio, go to [https://studio.azureml.net](https://studio.azureml.net).</span></span> <span data-ttu-id="402eb-129">Pokud už jste se do nástroje Machine Learning Studio někdy přihlašovali, klikněte na odkaz **Přihlásit se**.</span><span class="sxs-lookup"><span data-stu-id="402eb-129">If you’ve signed into Machine Learning Studio before, click **Sign In**.</span></span> <span data-ttu-id="402eb-130">Jinak klikněte na **Zaregistrovat** a vyberte si mezi bezplatnou a placenou možností.</span><span class="sxs-lookup"><span data-stu-id="402eb-130">Otherwise, click **Sign up here** and choose between free and paid options.</span></span>

<span data-ttu-id="402eb-131">![Přihlásit se k sadě Machine Learning Studio][sign-in-to-studio]
</span><span class="sxs-lookup"><span data-stu-id="402eb-131">![Sign in to Machine Learning Studio][sign-in-to-studio]
</span></span><br/><span data-ttu-id="402eb-132">
***Přihlásit se k sadě Machine Learning Studio***</span><span class="sxs-lookup"><span data-stu-id="402eb-132">
***Sign in to Machine Learning Studio***</span></span>

## <a name="five-steps-to-create-an-experiment"></a><span data-ttu-id="402eb-133">Vytvoření experimentu v pěti krocích</span><span class="sxs-lookup"><span data-stu-id="402eb-133">Five steps to create an experiment</span></span>

<span data-ttu-id="402eb-134">V tomto kurzu strojového učení vytvoříte experiment provedením pěti základních kroků v nástroji Machine Learning Studio, v rámci kterých sestavíte model, natrénujete ho a stanovíte jeho skóre:</span><span class="sxs-lookup"><span data-stu-id="402eb-134">In this machine learning tutorial, you'll follow five basic steps to build an experiment in Machine Learning Studio to create, train, and score your model:</span></span>

- <span data-ttu-id="402eb-135">**Vytvoření modelu**</span><span class="sxs-lookup"><span data-stu-id="402eb-135">**Create a model**</span></span>
    - <span data-ttu-id="402eb-136">[Krok 1: Získání dat]</span><span class="sxs-lookup"><span data-stu-id="402eb-136">[Step 1: Get data]</span></span>
    - <span data-ttu-id="402eb-137">[Krok 2: Příprava dat]</span><span class="sxs-lookup"><span data-stu-id="402eb-137">[Step 2: Prepare the data]</span></span>
    - <span data-ttu-id="402eb-138">[Krok 3: Definice příznaků]</span><span class="sxs-lookup"><span data-stu-id="402eb-138">[Step 3: Define features]</span></span>
- <span data-ttu-id="402eb-139">**Trénování modelu**</span><span class="sxs-lookup"><span data-stu-id="402eb-139">**Train the model**</span></span>
    - <span data-ttu-id="402eb-140">[Krok 4: Volba a použití algoritmu učení]</span><span class="sxs-lookup"><span data-stu-id="402eb-140">[Step 4: Choose and apply a learning algorithm]</span></span>
- <span data-ttu-id="402eb-141">**Stanovení skóre a otestování modelu**</span><span class="sxs-lookup"><span data-stu-id="402eb-141">**Score and test the model**</span></span>
    - <span data-ttu-id="402eb-142">[Krok 5: Předpověď cen nových automobilů]</span><span class="sxs-lookup"><span data-stu-id="402eb-142">[Step 5: Predict new automobile prices]</span></span>

<span data-ttu-id="402eb-143">[Krok 1: Získání dat]: #step-1-get-data</span><span class="sxs-lookup"><span data-stu-id="402eb-143">[Step 1: Get data]: #step-1-get-data</span></span>
<span data-ttu-id="402eb-144">[Krok 2: Příprava dat]: #step-2-prepare-the-data</span><span class="sxs-lookup"><span data-stu-id="402eb-144">[Step 2: Prepare the data]: #step-2-prepare-the-data</span></span>
<span data-ttu-id="402eb-145">[Krok 3: Definice příznaků]: #step-3-define-features</span><span class="sxs-lookup"><span data-stu-id="402eb-145">[Step 3: Define features]: #step-3-define-features</span></span>
<span data-ttu-id="402eb-146">[Krok 4: Volba a použití algoritmu učení]: #step-4-choose-and-apply-a-learning-algorithm</span><span class="sxs-lookup"><span data-stu-id="402eb-146">[Step 4: Choose and apply a learning algorithm]: #step-4-choose-and-apply-a-learning-algorithm</span></span>
<span data-ttu-id="402eb-147">[Krok 5: Předpověď cen nových automobilů]: #step-5-predict-new-automobile-prices</span><span class="sxs-lookup"><span data-stu-id="402eb-147">[Step 5: Predict new automobile prices]: #step-5-predict-new-automobile-prices</span></span>

> [!TIP] 
> <span data-ttu-id="402eb-148">Pracovní kopii následujícího experimentu najdete na webu [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="402eb-148">You can find a working copy of the following experiment in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="402eb-149">Přejděte k části **[První experiment z oblasti datové vědy – predikce ceny automobilu](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)**, klikněte na **Open in Studio** (Otevřít v sadě Studio) a stáhněte kopii experimentu do pracovního prostoru Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="402eb-149">Go to **[Your first data science experiment - Automobile price prediction](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)** and click **Open in Studio** to download a copy of the experiment into your Machine Learning Studio workspace.</span></span>


## <a name="step-1-get-data"></a><span data-ttu-id="402eb-150">Krok 1: Získání dat</span><span class="sxs-lookup"><span data-stu-id="402eb-150">Step 1: Get data</span></span>

<span data-ttu-id="402eb-151">První věc, kterou budete pro strojové učení potřebovat, jsou data.</span><span class="sxs-lookup"><span data-stu-id="402eb-151">The first thing you need to perform machine learning is data.</span></span>
<span data-ttu-id="402eb-152">Machine Learning Studio obsahuje několik ukázkových datových sad, které můžete použít, nebo můžete data importovat z mnoha zdrojů.</span><span class="sxs-lookup"><span data-stu-id="402eb-152">There are several sample datasets included with Machine Learning Studio that you can use, or you can import data from many sources.</span></span> <span data-ttu-id="402eb-153">V tomto příkladu použijeme ukázkovou datovou sadu **Automobile price data (Raw)**, která je součástí vašeho pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="402eb-153">For this example, we'll use the sample dataset, **Automobile price data (Raw)**, that's included in your workspace.</span></span>
<span data-ttu-id="402eb-154">Tato datová sada obsahuje záznamy řady různých automobilů, včetně informací o značce, modelu, technických specifikacích a ceně.</span><span class="sxs-lookup"><span data-stu-id="402eb-154">This dataset includes entries for various individual automobiles, including information such as make, model, technical specifications, and price.</span></span>

<span data-ttu-id="402eb-155">Tuto datovou sadu dostanete do svého experimentu takto.</span><span class="sxs-lookup"><span data-stu-id="402eb-155">Here's how to get the dataset into your experiment.</span></span>

1. <span data-ttu-id="402eb-156">Kliknutím na **+NOVÝ** ve spodní části okna nástroje Machine Learning Studio vytvořte nový experiment a vyberte **EXPERIMENT** a **Prázdný experiment**.</span><span class="sxs-lookup"><span data-stu-id="402eb-156">Create a new experiment by clicking **+NEW** at the bottom of the Machine Learning Studio window, select **EXPERIMENT**, and then select **Blank Experiment**.</span></span>

2. <span data-ttu-id="402eb-157">Experimentu se přiřadí výchozí název, který se zobrazí v horní části plátna.</span><span class="sxs-lookup"><span data-stu-id="402eb-157">The experiment is given a default name that you can see at the top of the canvas.</span></span> <span data-ttu-id="402eb-158">Vyberte tento text a přejmenujte jej na něco smysluplného, například **Predikce ceny automobilu**.</span><span class="sxs-lookup"><span data-stu-id="402eb-158">Select this text and rename it to something meaningful, for example, **Automobile price prediction**.</span></span> <span data-ttu-id="402eb-159">Název nemusí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="402eb-159">The name doesn't need to be unique.</span></span>

    ![Přejmenování experimentu][rename-experiment]

2. <span data-ttu-id="402eb-161">Nalevo od plátna experimentu je paleta datových sad a modulů.</span><span class="sxs-lookup"><span data-stu-id="402eb-161">To the left of the experiment canvas is a palette of datasets and modules.</span></span> <span data-ttu-id="402eb-162">Do pole Hledat v horní části palety zadejte **automobile**. Vyhledá se datová sada **Automobile price data (Raw)**.</span><span class="sxs-lookup"><span data-stu-id="402eb-162">Type **automobile** in the Search box at the top of this palette to find the dataset labeled **Automobile price data (Raw)**.</span></span> <span data-ttu-id="402eb-163">Přetáhněte tuto datovou sadu na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="402eb-163">Drag this dataset to the experiment canvas.</span></span>

    <span data-ttu-id="402eb-164">![Vyhledejte datovou sadu automobilů a přetáhněte ji na plátno experimentu.][type-automobile]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-164">![Find the automobile dataset and drag it onto the experiment canvas][type-automobile]
    </span></span><br/>
 <span data-ttu-id="402eb-165">***Najít datové sady automobilů a přetáhněte ji na plátno experimentu***</span><span class="sxs-lookup"><span data-stu-id="402eb-165">***Find the automobile dataset and drag it onto the experiment canvas***</span></span>

<span data-ttu-id="402eb-166">Pokud se chcete podívat, jak tato data vypadají, klikněte na výstupní port ve spodní části datové sady automobilů a vyberte **Vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="402eb-166">To see what this data looks like, click the output port at the bottom of the automobile dataset, and then select **Visualize**.</span></span>

<span data-ttu-id="402eb-167">![Klikněte na výstupní port a vyberte Vizualizovat.][select-visualize]
</span><span class="sxs-lookup"><span data-stu-id="402eb-167">![Click the output port and select "Visualize"][select-visualize]
</span></span><br/><span data-ttu-id="402eb-168">
***Klikněte na výstupní port a vyberte Vizualizovat.***</span><span class="sxs-lookup"><span data-stu-id="402eb-168">
***Click the output port and select "Visualize"***</span></span>

> [!TIP]
> <span data-ttu-id="402eb-169">Vstupní a výstupní porty datových sad a modulů jsou reprezentované malými kroužky – vstupní porty v horní části, výstupní porty v dolní části.</span><span class="sxs-lookup"><span data-stu-id="402eb-169">Datasets and modules have input and output ports represented by small circles - input ports at the top, output ports at the bottom.</span></span>
<span data-ttu-id="402eb-170">Pokud chcete vytvořit tok dat prostřednictvím experimentu, připojte výstupní port jednoho modulu ke vstupnímu portu jiného.</span><span class="sxs-lookup"><span data-stu-id="402eb-170">To create a flow of data through your experiment, you'll connect an output port of one module to an input port of another.</span></span>
<span data-ttu-id="402eb-171">V libovolném okamžiku můžete kliknout na výstupní port datové sady nebo modulu a prohlédnout si, jak v tomto bodě vypadá tok dat.</span><span class="sxs-lookup"><span data-stu-id="402eb-171">At any time, you can click the output port of a dataset or module to see what the data looks like at that point in the data flow.</span></span>

<span data-ttu-id="402eb-172">V této ukázkové datové sadě každou instanci automobilu představuje jeden řádek a proměnné přidružené k automobilům se zobrazují jako sloupce.</span><span class="sxs-lookup"><span data-stu-id="402eb-172">In this sample dataset, each instance of an automobile appears as a row, and the variables associated with each automobile appear as columns.</span></span> <span data-ttu-id="402eb-173">Na základě hodnot proměnných pro konkrétní automobil se pokusíme odhadnout cenu ve sloupci nejvíce vpravo (sloupec 26 se záhlavím price).</span><span class="sxs-lookup"><span data-stu-id="402eb-173">Given the variables for a specific automobile, we're going to try to predict the price in far-right column (column 26, titled "price").</span></span>

<span data-ttu-id="402eb-174">![Zobrazte data automobilů v okně vizualizace dat.][visualize-auto-data]
</span><span class="sxs-lookup"><span data-stu-id="402eb-174">![View the automobile data in the data visualization window][visualize-auto-data]
</span></span><br/><span data-ttu-id="402eb-175">
***Zobrazte data automobilů v okně vizualizace dat.***</span><span class="sxs-lookup"><span data-stu-id="402eb-175">
***View the automobile data in the data visualization window***</span></span>

<span data-ttu-id="402eb-176">Kliknutím na **x** v pravém horním rohu zavřete okno vizualizace.</span><span class="sxs-lookup"><span data-stu-id="402eb-176">Close the visualization window by clicking the "**x**" in the upper-right corner.</span></span>

## <a name="step-2-prepare-the-data"></a><span data-ttu-id="402eb-177">Krok 2: Příprava dat</span><span class="sxs-lookup"><span data-stu-id="402eb-177">Step 2: Prepare the data</span></span>

<span data-ttu-id="402eb-178">Před analýzou datové sady bývá zpravidla nutné sadu nějakým způsobem předzpracovat.</span><span class="sxs-lookup"><span data-stu-id="402eb-178">A dataset usually requires some preprocessing before it can be analyzed.</span></span> <span data-ttu-id="402eb-179">Možná jste si ve sloupcích různých řádků všimli chybějících hodnot.</span><span class="sxs-lookup"><span data-stu-id="402eb-179">For example, you might have noticed the missing values present in the columns of various rows.</span></span> <span data-ttu-id="402eb-180">Tyto chybějící hodnoty se musí vyčistit, aby model mohl data správně analyzovat.</span><span class="sxs-lookup"><span data-stu-id="402eb-180">These missing values need to be cleaned so the model can analyze the data correctly.</span></span> <span data-ttu-id="402eb-181">V našem případě odstraníme všechny řádky, ve kterých některé hodnoty chybí.</span><span class="sxs-lookup"><span data-stu-id="402eb-181">In our case, we'll remove any rows that have missing values.</span></span> <span data-ttu-id="402eb-182">Hodnoty ve sloupci **normalized-losses** navíc z velké části chybí, proto tento sloupec v modelu zcela vynecháme.</span><span class="sxs-lookup"><span data-stu-id="402eb-182">Also, the **normalized-losses** column has a large proportion of missing values, so we'll exclude that column from the model altogether.</span></span>

> [!TIP]
> <span data-ttu-id="402eb-183">Vyčištění chybějících hodnot ze vstupních dat je pro většinu modulů nutností.</span><span class="sxs-lookup"><span data-stu-id="402eb-183">Cleaning the missing values from input data is a prerequisite for using most of the modules.</span></span>

<span data-ttu-id="402eb-184">Nejdříve přidáme modul, který úplně odebere sloupec **normalized-losses** a potom přidáme další modul, který odebere všechny řádky, ve kterých chybějí data.</span><span class="sxs-lookup"><span data-stu-id="402eb-184">First we add a module that removes the **normalized-losses** column completely, and then we add another module that removes any row that has missing data.</span></span>

1. <span data-ttu-id="402eb-185">Do pole Hledat v horní části palety modulů zadejte **výběr sloupců**. Vyhledá se modul [Výběr sloupců v datové sadě ][select-columns]. Ten potom přetáhněte na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="402eb-185">Type **select columns** in the Search box at the top of the module palette to find the [Select Columns in Dataset][select-columns] module, then drag it to the experiment canvas.</span></span> <span data-ttu-id="402eb-186">Tento modul umožňuje vybrat, které sloupce dat chceme zahrnout do modelu, nebo je z modelu naopak vyloučit.</span><span class="sxs-lookup"><span data-stu-id="402eb-186">This module allows us to select which columns of data we want to include or exclude in the model.</span></span>

2. <span data-ttu-id="402eb-187">Připojte výstupní port datové sady **Automobile price data (Raw)** ke vstupnímu portu modulu [Výběr sloupců v datové sadě][select-columns].</span><span class="sxs-lookup"><span data-stu-id="402eb-187">Connect the output port of the **Automobile price data (Raw)** dataset to the input port of the [Select Columns in Dataset][select-columns] module.</span></span>

    <span data-ttu-id="402eb-188">![Přidejte modul Výběr sloupců v datové sadě na plátno experimentu a připojte ho.][type-select-columns]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-188">![Add the "Select Columns in Dataset" module to the experiment canvas and connect it][type-select-columns]
    </span></span><br/>
 <span data-ttu-id="402eb-189">***Přidat modul "Vyberte sloupců v datové sadě" na plátno experimentu a propojte jej***</span><span class="sxs-lookup"><span data-stu-id="402eb-189">***Add the "Select Columns in Dataset" module to the experiment canvas and connect it***</span></span>

3. <span data-ttu-id="402eb-190">Klikněte na modul [Výběr sloupců v datové sadě][select-columns] a v podokně **Vlastnosti** klikněte na **Spustit selektor sloupců**.</span><span class="sxs-lookup"><span data-stu-id="402eb-190">Click the [Select Columns in Dataset][select-columns] module and click **Launch column selector** in the **Properties** pane.</span></span>

    - <span data-ttu-id="402eb-191">Vlevo klikněte na **S pravidly**.</span><span class="sxs-lookup"><span data-stu-id="402eb-191">On the left, click **With rules**</span></span>
    - <span data-ttu-id="402eb-192">V části **Začít s** klikněte na **Všechny sloupce**.</span><span class="sxs-lookup"><span data-stu-id="402eb-192">Under **Begin With**, click **All columns**.</span></span> <span data-ttu-id="402eb-193">Tím modul [Výběr sloupců v datové sadě][select-columns] dostává instrukci, aby prošel všechny sloupce (kromě sloupců, které vyloučíme).</span><span class="sxs-lookup"><span data-stu-id="402eb-193">This directs [Select Columns in Dataset][select-columns] to pass through all the columns (except those columns we're about to exclude).</span></span>
    - <span data-ttu-id="402eb-194">V rozevíracích seznamech vyberte **Vyloučit** a **názvy sloupců** a klikněte do textového pole.</span><span class="sxs-lookup"><span data-stu-id="402eb-194">From the drop-downs, select **Exclude** and **column names**, and then click inside the text box.</span></span> <span data-ttu-id="402eb-195">Zobrazí se seznam sloupců.</span><span class="sxs-lookup"><span data-stu-id="402eb-195">A list of columns is displayed.</span></span> <span data-ttu-id="402eb-196">Vyberte sloupec **normalized-losses**, který se tak přidá do textového pole.</span><span class="sxs-lookup"><span data-stu-id="402eb-196">Select **normalized-losses**, and it's added to the text box.</span></span>
    - <span data-ttu-id="402eb-197">Kliknutím na tlačítko se značkou zaškrtnutí (OK) zavřete selektor sloupců (vpravo dole).</span><span class="sxs-lookup"><span data-stu-id="402eb-197">Click the check mark (OK) button to close the column selector (on the lower-right).</span></span>

    <span data-ttu-id="402eb-198">![Spusťte selektor sloupců a vylučte sloupec normalized-losses.][launch-column-selector]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-198">![Launch the column selector and exclude the "normalized-losses" column][launch-column-selector]
    </span></span><br/>
 <span data-ttu-id="402eb-199">***Spustit selektor sloupců a vyloučit "normalized-losses"***</span><span class="sxs-lookup"><span data-stu-id="402eb-199">***Launch the column selector and exclude the "normalized-losses" column***</span></span>

    <span data-ttu-id="402eb-200">Podokno vlastností modulu **Výběr sloupců v datové sadě** teď indikuje, že modul bude procházet všechny sloupce datové sady kromě **normalized-losses**.</span><span class="sxs-lookup"><span data-stu-id="402eb-200">Now the properties pane for **Select Columns in Dataset** indicates that it will pass through all columns from the dataset except **normalized-losses**.</span></span>

    <span data-ttu-id="402eb-201">![Podokno vlastností indikuje, že sloupec normalized-losses je vyloučený.][showing-excluded-column]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-201">![The properties pane shows that the "normalized-losses" column is excluded][showing-excluded-column]
    </span></span><br/>
 <span data-ttu-id="402eb-202">***V podokně Vlastnosti ukazuje, že je vyloučena sloupci "normalized-losses"***</span><span class="sxs-lookup"><span data-stu-id="402eb-202">***The properties pane shows that the "normalized-losses" column is excluded***</span></span>

    > [!TIP]
    <span data-ttu-id="402eb-203">Kliknutím dvakrát na modul a zadáním textu je možné přidat k modulu komentář.</span><span class="sxs-lookup"><span data-stu-id="402eb-203">You can add a comment to a module by double-clicking the module and entering text.</span></span> <span data-ttu-id="402eb-204">To vám může pomoci rychle poznat, jaký je účel modulu v experimentu.</span><span class="sxs-lookup"><span data-stu-id="402eb-204">This can help you see at a glance what the module is doing in your experiment.</span></span> <span data-ttu-id="402eb-205">V tomto případě klikněte dvakrát na modul [Výběr sloupců v datové sadě][select-columns] a zadejte komentář Vyloučit normalized-losses.</span><span class="sxs-lookup"><span data-stu-id="402eb-205">In this case double-click the [Select Columns in Dataset][select-columns] module and type the comment "Exclude normalized losses."</span></span>
    >
    >


    <span data-ttu-id="402eb-206">![Klikněte dvakrát na modul a přidejte komentář.][add-comment]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-206">![Double-click a module to add a comment][add-comment]
    </span></span><br/>
 <span data-ttu-id="402eb-207">***Klikněte dvakrát na modul přidání komentáře***</span><span class="sxs-lookup"><span data-stu-id="402eb-207">***Double-click a module to add a comment***</span></span>

3. <span data-ttu-id="402eb-208">Přetáhněte modul [Vyčištění chybějících dat][clean-missing-data] na plátno experimentu a propojte jej s modulem [Výběr sloupců v datové sadě][select-columns].</span><span class="sxs-lookup"><span data-stu-id="402eb-208">Drag the [Clean Missing Data][clean-missing-data] module to the experiment canvas and connect it to the [Select Columns in Dataset][select-columns] module.</span></span> <span data-ttu-id="402eb-209">V podokně **Vlastnosti** vyberte v části **Režim čištění** možnost **Odstranit celý řádek**.</span><span class="sxs-lookup"><span data-stu-id="402eb-209">In the **Properties** pane, select **Remove entire row** under **Cleaning mode**.</span></span> <span data-ttu-id="402eb-210">Tím modul [Vyčištění chybějících dat][clean-missing-data] dostává pokyn k vyčištění dat odstraněním řádků, ve kterých chybí některé hodnoty.</span><span class="sxs-lookup"><span data-stu-id="402eb-210">This directs [Clean Missing Data][clean-missing-data] to clean the data by removing rows that have any missing values.</span></span> <span data-ttu-id="402eb-211">Klikněte dvakrát na modul a zadejte komentář Odstranění řádků s chybějícími hodnotami.</span><span class="sxs-lookup"><span data-stu-id="402eb-211">Double-click the module and type the comment "Remove missing value rows."</span></span>

    <span data-ttu-id="402eb-212">![Pro modul Vyčištění chybějících dat nastavte režim čištění na Odstranit celý řádek.][set-remove-entire-row]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-212">![Set the cleaning mode to "Remove entire row" for the "Clean Missing Data" module][set-remove-entire-row]
    </span></span><br/>
 <span data-ttu-id="402eb-213">***Nastavte čisticí režim na "Odebrat celý řádek" pro modul "Vyčištění chybějících dat"***</span><span class="sxs-lookup"><span data-stu-id="402eb-213">***Set the cleaning mode to "Remove entire row" for the "Clean Missing Data" module***</span></span>

4. <span data-ttu-id="402eb-214">Spusťte experiment kliknutím na **SPUSTIT** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="402eb-214">Run the experiment by clicking **RUN** at the bottom of the page.</span></span>

    <span data-ttu-id="402eb-215">Až se spuštění experimentu dokončí, u všech modulů se zobrazí zelená značka zaškrtnutí, která označuje, že jejich činnost úspěšně skončila.</span><span class="sxs-lookup"><span data-stu-id="402eb-215">When the experiment has finished running, all the modules have a green check mark to indicate that they finished successfully.</span></span> <span data-ttu-id="402eb-216">Všimněte si také stavu **Konec běhu** v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="402eb-216">Notice also the **Finished running** status in the upper-right corner.</span></span>

<span data-ttu-id="402eb-217">![Po spuštění by experiment měl vypadat asi takhle nějak.][early-experiment-run]
</span><span class="sxs-lookup"><span data-stu-id="402eb-217">![After running it, the experiment should look something like this][early-experiment-run]
</span></span><br/><span data-ttu-id="402eb-218">
***Po spuštění by experiment měl vypadat asi takhle nějak.***</span><span class="sxs-lookup"><span data-stu-id="402eb-218">
***After running it, the experiment should look something like this***</span></span>

> [!TIP]
> <span data-ttu-id="402eb-219">Proč jsme experiment teď spustili?</span><span class="sxs-lookup"><span data-stu-id="402eb-219">Why did we run the experiment now?</span></span> <span data-ttu-id="402eb-220">Spuštěním experimentu jsme zajistili, aby definice sloupců pro naše data prošly z původní datové sady přes modul [Výběr sloupců v datové sadě][select-columns] a přes modul [Vyčištění chybějících dat][clean-missing-data].</span><span class="sxs-lookup"><span data-stu-id="402eb-220">By running the experiment, the column definitions for our data pass from the dataset, through the [Select Columns in Dataset][select-columns] module, and through the [Clean Missing Data][clean-missing-data] module.</span></span> <span data-ttu-id="402eb-221">To znamená, že všechny moduly, které připojíme k modulu [Vyčištění chybějících dat][clean-missing-data], budou také mít tytéž informace.</span><span class="sxs-lookup"><span data-stu-id="402eb-221">This means that any modules we connect to [Clean Missing Data][clean-missing-data] will also have this same information.</span></span>

<span data-ttu-id="402eb-222">Jediné, co jsme do této chvíle v experimentu udělali, bylo vyčištění dat.</span><span class="sxs-lookup"><span data-stu-id="402eb-222">All we have done in the experiment up to this point is clean the data.</span></span> <span data-ttu-id="402eb-223">Pokud si chcete zobrazit vyčištěnou datovou sadu, klikněte na levý výstupní port modulu [Vyčištění chybějících dat][clean-missing-data] a vyberte **Vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="402eb-223">If you want to view the cleaned dataset, click the left output port of the [Clean Missing Data][clean-missing-data] module and select **Visualize**.</span></span> <span data-ttu-id="402eb-224">Všimněte si, že již není zobrazen sloupec **normalized-losses** a že nechybí žádné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="402eb-224">Notice that the **normalized-losses** column is no longer included, and there are no missing values.</span></span>

<span data-ttu-id="402eb-225">Nyní když jsou data vyčištěna, můžete určit, které příznaky použijeme v prediktivním modelu.</span><span class="sxs-lookup"><span data-stu-id="402eb-225">Now that the data is clean, we're ready to specify what features we're going to use in the predictive model.</span></span>

## <a name="step-3-define-features"></a><span data-ttu-id="402eb-226">Krok 3: Definice příznaků</span><span class="sxs-lookup"><span data-stu-id="402eb-226">Step 3: Define features</span></span>

<span data-ttu-id="402eb-227">Ve strojovém učení se jako *příznaky* označují jednotlivé měřitelné vlastnosti něčeho, co vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="402eb-227">In machine learning, *features* are individual measurable properties of something you’re interested in.</span></span> <span data-ttu-id="402eb-228">V naší datové sadě každý řádek představuje jeden automobil a každý sloupec je příznak daného automobilu.</span><span class="sxs-lookup"><span data-stu-id="402eb-228">In our dataset, each row represents one automobile, and each column is a feature of that automobile.</span></span>

<span data-ttu-id="402eb-229">Nalezení správné sady příznaků pro vytvoření prediktivního modelu vyžaduje experimentování a znalost problému, který chcete vyřešit.</span><span class="sxs-lookup"><span data-stu-id="402eb-229">Finding a good set of features for creating a predictive model requires experimentation and knowledge about the problem you want to solve.</span></span> <span data-ttu-id="402eb-230">Některé příznaky jsou pro predikci cíle vhodnější než jiné.</span><span class="sxs-lookup"><span data-stu-id="402eb-230">Some features are better for predicting the target than others.</span></span> <span data-ttu-id="402eb-231">Některé příznaky také mají silnou korelaci s jinými a je možné je odebrat.</span><span class="sxs-lookup"><span data-stu-id="402eb-231">Also, some features have a strong correlation with other features and can be removed.</span></span> <span data-ttu-id="402eb-232">Například příznaky city-mpg a highway-mpg jsou vzájemně těsně propojené, takže stačí jeden z nich odebrat a ponechat jenom ten druhý, aniž by to vytvářenou predikci výrazně ovlivnilo.</span><span class="sxs-lookup"><span data-stu-id="402eb-232">For example, city-mpg and highway-mpg are closely related so we can keep one and remove the other without significantly affecting the prediction.</span></span>

<span data-ttu-id="402eb-233">Vytvořme model, který používá podmnožinu příznaků naší datové sady.</span><span class="sxs-lookup"><span data-stu-id="402eb-233">Let's build a model that uses a subset of the features in our dataset.</span></span> <span data-ttu-id="402eb-234">Později můžete vybrat jiné příznaky, spustit experiment znovu a zjistit, jestli nedostanete lepší výsledky.</span><span class="sxs-lookup"><span data-stu-id="402eb-234">You can come back later and select different features, run the experiment again, and see if you get better results.</span></span> <span data-ttu-id="402eb-235">Nejdřív ale vyzkoušíme tyto funkce:</span><span class="sxs-lookup"><span data-stu-id="402eb-235">But to start, let's try the following features:</span></span>

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. <span data-ttu-id="402eb-236">Na plátno experimentu přetáhněte další modul [Výběr sloupců v datové sadě][select-columns].</span><span class="sxs-lookup"><span data-stu-id="402eb-236">Drag another [Select Columns in Dataset][select-columns] module to the experiment canvas.</span></span> <span data-ttu-id="402eb-237">Propojte levý výstupní port modulu [Vyčištění chybějících dat][clean-missing-data] se vstupem modulu [Výběr sloupců v datové sadě][select-columns].</span><span class="sxs-lookup"><span data-stu-id="402eb-237">Connect the left output port of the [Clean Missing Data][clean-missing-data] module to the input of the [Select Columns in Dataset][select-columns] module.</span></span>

    <span data-ttu-id="402eb-238">![Připojte modul Výběr sloupců v datové sadě k modulu Vyčištění chybějících dat.][connect-clean-to-select]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-238">![Connect the "Select Columns in Dataset" module to the "Clean Missing Data" module][connect-clean-to-select]
    </span></span><br/>
 <span data-ttu-id="402eb-239">***Modul "Vyberte sloupců v datové sadě" připojení k modulu "Vyčištění chybějících dat"***</span><span class="sxs-lookup"><span data-stu-id="402eb-239">***Connect the "Select Columns in Dataset" module to the "Clean Missing Data" module***</span></span>

2. <span data-ttu-id="402eb-240">Poklikejte na modul a zadejte Výběr příznaků pro predikci.</span><span class="sxs-lookup"><span data-stu-id="402eb-240">Double-click the module and type "Select features for prediction."</span></span>

2. <span data-ttu-id="402eb-241">V podokně **Vlastnosti** klikněte na **Spustit selektor sloupců**.</span><span class="sxs-lookup"><span data-stu-id="402eb-241">Click **Launch column selector** in the **Properties** pane.</span></span>

3. <span data-ttu-id="402eb-242">Klikněte na **S pravidly**.</span><span class="sxs-lookup"><span data-stu-id="402eb-242">Click **With rules**.</span></span>

4. <span data-ttu-id="402eb-243">V části **Začít s** klikněte na **Žádné sloupce**.</span><span class="sxs-lookup"><span data-stu-id="402eb-243">Under **Begin With**, click **No columns**.</span></span> <span data-ttu-id="402eb-244">V řádku filtru vyberte **Zahrnout** a **názvy sloupců** a vyberte náš seznam názvů sloupců v textovém poli.</span><span class="sxs-lookup"><span data-stu-id="402eb-244">In the filter row, select **Include** and **column names** and select our list of column names in the text box.</span></span> <span data-ttu-id="402eb-245">Tím dáváte modul instrukci, aby neprocházel žádné sloupce (příznaky) s výjimkou těch, které určíme.</span><span class="sxs-lookup"><span data-stu-id="402eb-245">This directs the module to not pass through any columns (features) except the ones that we specify.</span></span>

5. <span data-ttu-id="402eb-246">Klikněte na tlačítko zaškrtnutí (OK).</span><span class="sxs-lookup"><span data-stu-id="402eb-246">Click the check mark (OK) button.</span></span>

    <span data-ttu-id="402eb-247">![Vyberte sloupce (příznaky), které se mají zahrnout do predikce.][select-columns-to-include]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-247">![Select the columns (features) to include in the prediction][select-columns-to-include]
    </span></span><br/>
 <span data-ttu-id="402eb-248">***Vyberte sloupce (funkce), které chcete zahrnout do předpovědi***</span><span class="sxs-lookup"><span data-stu-id="402eb-248">***Select the columns (features) to include in the prediction***</span></span>

<span data-ttu-id="402eb-249">Výsledkem je filtrovaná datová sada obsahující jenom ty příznaky, které chceme předat algoritmu učení, který použijeme v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="402eb-249">This produces a filtered dataset containing only the features we want to pass to the learning algorithm we'll use in the next step.</span></span> <span data-ttu-id="402eb-250">Později se můžete vrátit a zkusit jiný výběr příznaků.</span><span class="sxs-lookup"><span data-stu-id="402eb-250">Later, you can return and try again with a different selection of features.</span></span>

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a><span data-ttu-id="402eb-251">Krok 4: Volba a použití algoritmu učení</span><span class="sxs-lookup"><span data-stu-id="402eb-251">Step 4: Choose and apply a learning algorithm</span></span>

<span data-ttu-id="402eb-252">Nyní když jsou data připravena, tvorba prediktivního modelu sestává z trénování a testování.</span><span class="sxs-lookup"><span data-stu-id="402eb-252">Now that the data is ready, constructing a predictive model consists of training and testing.</span></span> <span data-ttu-id="402eb-253">Naše data použijeme pro trénování modelu. Potom model otestujeme a zjistíme, jak přesně dokáže předpovídat ceny.</span><span class="sxs-lookup"><span data-stu-id="402eb-253">We'll use our data to train the model, and then we'll test the model to see how closely it's able to predict prices.</span></span>
<!-- For now, don't worry about *why* we need to train and then test a model.-->

<span data-ttu-id="402eb-254">*Klasifikace* a *regrese* jsou dva typy technik strojového učení se supervizí.</span><span class="sxs-lookup"><span data-stu-id="402eb-254">*Classification* and *regression* are two types of supervised machine learning algorithms.</span></span> <span data-ttu-id="402eb-255">Klasifikace předpovídá odpověď na základě definované sady kategorií, třeba barvy (červená, modrá nebo zelená).</span><span class="sxs-lookup"><span data-stu-id="402eb-255">Classification predicts an answer from a defined set of categories, such as a color (red, blue, or green).</span></span> <span data-ttu-id="402eb-256">Regrese se používá k předpovědi čísel.</span><span class="sxs-lookup"><span data-stu-id="402eb-256">Regression is used to predict a number.</span></span>

<span data-ttu-id="402eb-257">Chceme předpovědět cenu, což je číslo, a tak použijeme regresní algoritmus.</span><span class="sxs-lookup"><span data-stu-id="402eb-257">Because we want to predict price, which is a number, we'll use a regression algorithm.</span></span> <span data-ttu-id="402eb-258">Pro tento příklad použijeme jednoduchý model *lineární regrese*.</span><span class="sxs-lookup"><span data-stu-id="402eb-258">For this example, we'll use a simple *linear regression* model.</span></span>

> [!TIP]
> <span data-ttu-id="402eb-259">Pokud se chcete o různých typech algoritmů strojového učení a jejich použití dozvědět víc, můžete si pustit první video ze seriálu Data Science for Beginners (Datová věda pro začátečníky), [The five questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) (Prvních pět dotazů, na které datová věda poskytuje odpověď).</span><span class="sxs-lookup"><span data-stu-id="402eb-259">If you want to learn more about different types of machine learning algorithms and when to use them, you might view the first video in the Data Science for Beginners series, [The five questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span></span> <span data-ttu-id="402eb-260">Můžete se také podívat na infografiku [Základy služby Machine Learning s příklady algoritmů](machine-learning-basics-infographic-with-algorithm-examples.md) nebo si prohlédnout [Stručný přehled algoritmů strojového učení ](machine-learning-algorithm-cheat-sheet.md).</span><span class="sxs-lookup"><span data-stu-id="402eb-260">You might also look at the infographic [Machine learning basics with algorithm examples](machine-learning-basics-infographic-with-algorithm-examples.md), or check out the [Machine learning algorithm cheat sheet](machine-learning-algorithm-cheat-sheet.md).</span></span>

<span data-ttu-id="402eb-261">Model trénujeme tím, že mu poskytneme sadu dat, která zahrnují cenu.</span><span class="sxs-lookup"><span data-stu-id="402eb-261">We train the model by giving it a set of data that includes the price.</span></span> <span data-ttu-id="402eb-262">Model projde data a hledá korelaci mezi příznaky automobilu a jeho cenou.</span><span class="sxs-lookup"><span data-stu-id="402eb-262">The model scans the data and look for correlations between an automobile's features and its price.</span></span> <span data-ttu-id="402eb-263">Potom model otestujeme. Poskytneme mu sadu příznaků pro automobily, které známe, a uvidíme, do jaké míry se predikce modelu blíží známé ceně.</span><span class="sxs-lookup"><span data-stu-id="402eb-263">Then we'll test the model - we'll give it a set of features for automobiles we're familiar with and see how close the model comes to predicting the known price.</span></span>

<span data-ttu-id="402eb-264">Naše data můžeme použít jak pro trénování modelu, tak pro jeho otestování. Dají se totiž rozdělit na samostatné sady pro trénování a testování.</span><span class="sxs-lookup"><span data-stu-id="402eb-264">We'll use our data for both training the model and testing it by splitting the data into separate training and testing datasets.</span></span>

1. <span data-ttu-id="402eb-265">Vyberte a přetáhněte na plátno experimentu modul [Rozdělení dat][split] a propojte jej s posledním modulem [Výběr sloupců v datové sadě][select-columns].</span><span class="sxs-lookup"><span data-stu-id="402eb-265">Select and drag the [Split Data][split] module to the experiment canvas and connect it to the last [Select Columns in Dataset][select-columns] module.</span></span>

2. <span data-ttu-id="402eb-266">Klikněte na modul [Rozdělení dat][split]. Modul se vybere.</span><span class="sxs-lookup"><span data-stu-id="402eb-266">Click the [Split Data][split] module to select it.</span></span> <span data-ttu-id="402eb-267">Vyhledejte **Podíl řádků v první výstupní sadě dat** (v podokně **Vlastnosti** napravo od plátna) a nastavte ho na 0,75.</span><span class="sxs-lookup"><span data-stu-id="402eb-267">Find the **Fraction of rows in the first output dataset** (in the **Properties** pane to the right of the canvas) and set it to 0.75.</span></span> <span data-ttu-id="402eb-268">Takto použijeme 75 procent dat pro trénování modelu a 25 procent si ponecháme na testování (později můžete experimentovat s různými procentními podíly).</span><span class="sxs-lookup"><span data-stu-id="402eb-268">This way, we'll use 75 percent of the data to train the model, and hold back 25 percent for testing (later, you can experiment with using different percentages).</span></span>

    <span data-ttu-id="402eb-269">![Nastavte podíl pro rozdělení modulu Rozdělení dat na 0,75.][set-split-data-percentage]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-269">![Set the split fraction of the "Split Data" module to 0.75][set-split-data-percentage]
    </span></span><br/>
 <span data-ttu-id="402eb-270">***Nastavte podíl rozdělení modul "Rozdělení dat" na 0,75***</span><span class="sxs-lookup"><span data-stu-id="402eb-270">***Set the split fraction of the "Split Data" module to 0.75***</span></span>

    > [!TIP]
    > <span data-ttu-id="402eb-271">Změnou parametru **Náhodná počáteční hodnota** je možné pro trénování a testování vytvořit různé náhodné vzorky.</span><span class="sxs-lookup"><span data-stu-id="402eb-271">By changing the **Random seed** parameter, you can produce different random samples for training and testing.</span></span> <span data-ttu-id="402eb-272">Tento parametr řídí nastavování počáteční hodnoty pseudonáhodného generátoru čísel.</span><span class="sxs-lookup"><span data-stu-id="402eb-272">This parameter controls the seeding of the pseudo-random number generator.</span></span>

2. <span data-ttu-id="402eb-273">Spusťte experiment.</span><span class="sxs-lookup"><span data-stu-id="402eb-273">Run the experiment.</span></span> <span data-ttu-id="402eb-274">Při spuštění experimentu moduly [Výběr sloupců v datové sadě][select-columns] a [Rozdělení dat][split] předají definice sloupců do modulů, které přidáme jako další.</span><span class="sxs-lookup"><span data-stu-id="402eb-274">When the experiment is run, the [Select Columns in Dataset][select-columns] and [Split Data][split] modules pass column definitions to the modules we'll be adding next.</span></span>  

3. <span data-ttu-id="402eb-275">Nyní vyberte algoritmus učení. Na paletě modulů nalevo od plátna rozbalte kategorii **Strojové učení** a pak **Inicializovat model**.</span><span class="sxs-lookup"><span data-stu-id="402eb-275">To select the learning algorithm, expand the **Machine Learning** category in the module palette to the left of the canvas, and then expand **Initialize Model**.</span></span> <span data-ttu-id="402eb-276">Tímto se zobrazí několik kategorií modulů, které je možné použít k inicializaci algoritmů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="402eb-276">This displays several categories of modules that can be used to initialize machine learning algorithms.</span></span> <span data-ttu-id="402eb-277">Pro tento experiment vyberte modul [Lineární regrese][linear-regression] v kategorii **Regrese** a přetáhněte ho na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="402eb-277">For this experiment, select the [Linear Regression][linear-regression] module under the **Regression** category, and drag it to the experiment canvas.</span></span>
<span data-ttu-id="402eb-278">(Tento modul můžete najít i tak, že do pole Hledat palety zadáte lineární regrese.)</span><span class="sxs-lookup"><span data-stu-id="402eb-278">(You can also find the module by typing "linear regression" in the palette Search box.)</span></span>

4. <span data-ttu-id="402eb-279">Najděte modul [Trénování modelu][train-model] a přetáhněte ho na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="402eb-279">Find and drag the [Train Model][train-model] module to the experiment canvas.</span></span> <span data-ttu-id="402eb-280">Propojte výstup modulu [Lineární regrese][linear-regression] s levým vstupem modulu [Trénování modelu][train-model] a potom propojte výstup trénovacích dat (levý port) modulu [Rozdělení dat][split] s pravým vstupem modulu [Trénování modelu][train-model].</span><span class="sxs-lookup"><span data-stu-id="402eb-280">Connect the output of the [Linear Regression][linear-regression] module to the left input of the [Train Model][train-model] module, and connect the training data output (left port) of the [Split Data][split] module to the right input of the [Train Model][train-model] module.</span></span>

    <span data-ttu-id="402eb-281">![Připojte modul Trénování modelu k modulům Lineární regrese a Rozdělení dat.][connect-train-model]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-281">![Connect the "Train Model" module to both the "Linear Regression" and "Split Data" modules][connect-train-model]
    </span></span><br/>
 <span data-ttu-id="402eb-282">***Připojení modulu "Train Model" na "Lineární regrese" i "Rozdělení dat" moduly***</span><span class="sxs-lookup"><span data-stu-id="402eb-282">***Connect the "Train Model" module to both the "Linear Regression" and "Split Data" modules***</span></span>

5. <span data-ttu-id="402eb-283">Klikněte na modul [Trénování modelu][train-model], v podokně **Vlastnosti** klikněte na **Spustit selektor sloupců** a vyberte sloupec **price**.</span><span class="sxs-lookup"><span data-stu-id="402eb-283">Click the [Train Model][train-model] module, click **Launch column selector** in the **Properties** pane, and then select the **price** column.</span></span> <span data-ttu-id="402eb-284">Toto je hodnota, kterou náš model bude předpovídat.</span><span class="sxs-lookup"><span data-stu-id="402eb-284">This is the value that our model is going to predict.</span></span>

    <span data-ttu-id="402eb-285">V selektoru sloupců vyberete sloupec **price** – přesunete ho ze seznamu **Dostupné sloupce** do seznamu **Vybrané sloupce**.</span><span class="sxs-lookup"><span data-stu-id="402eb-285">You select the **price** column in the column selector by moving it from the **Available columns** list to the **Selected columns** list.</span></span>

    <span data-ttu-id="402eb-286">![Pro modul Trénování modelu vyberte sloupec price.][select-price-column]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-286">![Select the price column for the "Train Model" module][select-price-column]
    </span></span><br/>
 <span data-ttu-id="402eb-287">***Vyberte sloupec ceny pro modul "Train Model"***</span><span class="sxs-lookup"><span data-stu-id="402eb-287">***Select the price column for the "Train Model" module***</span></span>

6. <span data-ttu-id="402eb-288">Spusťte experiment.</span><span class="sxs-lookup"><span data-stu-id="402eb-288">Run the experiment.</span></span>

<span data-ttu-id="402eb-289">Výsledkem je natrénovaný model, který je možné použít ke stanovení skóre pro nová data automobilů a k následné predikci cen.</span><span class="sxs-lookup"><span data-stu-id="402eb-289">We now have a trained regression model that can be used to score new automobile data to make price predictions.</span></span>

<span data-ttu-id="402eb-290">![Po spuštění by experiment měl vypadat asi takhle nějak.][second-experiment-run]
</span><span class="sxs-lookup"><span data-stu-id="402eb-290">![After running, the experiment should now look something like this][second-experiment-run]
</span></span><br/><span data-ttu-id="402eb-291">
***Po spuštění by experiment měl vypadat asi takhle nějak.***</span><span class="sxs-lookup"><span data-stu-id="402eb-291">
***After running, the experiment should now look something like this***</span></span>

## <a name="step-5-predict-new-automobile-prices"></a><span data-ttu-id="402eb-292">Krok 5: Předpověď cen nových automobilů</span><span class="sxs-lookup"><span data-stu-id="402eb-292">Step 5: Predict new automobile prices</span></span>

<span data-ttu-id="402eb-293">Nyní když jsme natrénovali model pomocí 75 procent dat, můžeme model použít ke stanovení skóre u zbylých 25 procent dat a zjistit, jak dobře model funguje.</span><span class="sxs-lookup"><span data-stu-id="402eb-293">Now that we've trained the model using 75 percent of our data, we can use it to score the other 25 percent of the data to see how well our model functions.</span></span>

1. <span data-ttu-id="402eb-294">Najděte modul [Určení skóre modelu][score-model] a přetáhněte ho na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="402eb-294">Find and drag the [Score Model][score-model] module to the experiment canvas.</span></span> <span data-ttu-id="402eb-295">Propojte výstup modulu [Trénování modelu][train-model] s levým vstupním portem modulu [Určení skóre modelu][score-model].</span><span class="sxs-lookup"><span data-stu-id="402eb-295">Connect the output of the [Train Model][train-model] module to the left input port of [Score Model][score-model].</span></span> <span data-ttu-id="402eb-296">Propojte výstup testovacích dat (pravý port) modulu [Rozdělení dat][split] s pravým vstupním portem modulu [Určení skóre modelu][score-model].</span><span class="sxs-lookup"><span data-stu-id="402eb-296">Connect the test data output (right port) of the [Split Data][split] module to the right input port of [Score Model][score-model].</span></span>

    <span data-ttu-id="402eb-297">![Propojte modul Určení skóre modelu s moduly Lineární regrese a Rozdělení dat.][connect-score-model]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-297">![Connect the "Score Model" module to both the "Train Model" and "Split Data" modules][connect-score-model]
    </span></span><br/>
 <span data-ttu-id="402eb-298">***Připojit k "Train Model" i "Rozdělení dat" moduly modul "Určení skóre modelu"***</span><span class="sxs-lookup"><span data-stu-id="402eb-298">***Connect the "Score Model" module to both the "Train Model" and "Split Data" modules***</span></span>

2. <span data-ttu-id="402eb-299">Spusťte experiment a zobrazte výstup modulu [Určení skóre modelu][score-model] (klikněte na výstupní port modulu [Určení skóre modelu][score-model] a vyberte **Vizualizovat**).</span><span class="sxs-lookup"><span data-stu-id="402eb-299">Run the experiment and view the output from the [Score Model][score-model] module (click the output port of [Score Model][score-model] and select **Visualize**).</span></span> <span data-ttu-id="402eb-300">Na výstupu se zobrazí predikované hodnoty ceny a známé hodnoty v testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="402eb-300">The output shows the predicted values for price and the known values from the test data.</span></span>  

    <span data-ttu-id="402eb-301">![Výstup modulu Určení skóre modelu][score-model-output]
    </span><span class="sxs-lookup"><span data-stu-id="402eb-301">![Output of the "Score Model" module][score-model-output]
    </span></span><br/>
 <span data-ttu-id="402eb-302">***Výstup modul "Určení skóre modelu"***</span><span class="sxs-lookup"><span data-stu-id="402eb-302">***Output of the "Score Model" module***</span></span>

3. <span data-ttu-id="402eb-303">Nakonec otestujeme kvalitu výsledků.</span><span class="sxs-lookup"><span data-stu-id="402eb-303">Finally, we test the quality of the results.</span></span> <span data-ttu-id="402eb-304">Najděte modul [Vyhodnocení modelu][evaluate-model], přetáhněte ho na plátno experimentu a propojte výstup modulu [Určení skóre modelu][score-model] s levým vstupem modulu [Vyhodnocení modelu][evaluate-model].</span><span class="sxs-lookup"><span data-stu-id="402eb-304">Select and drag the [Evaluate Model][evaluate-model] module to the experiment canvas, and connect the output of the [Score Model][score-model] module to the left input of [Evaluate Model][evaluate-model].</span></span>

    > [!TIP]
    > <span data-ttu-id="402eb-305">Modul [Vyhodnocení modelu][evaluate-model] má dva vstupní porty, protože se dá využít k porovnání dvou modelů vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="402eb-305">There are two input ports on the [Evaluate Model][evaluate-model] module because it can be used to compare two models side by side.</span></span> <span data-ttu-id="402eb-306">Později můžete do experimentu přidat další algoritmus a pomocí modulu [Vyhodnocení modelu][evaluate-model] zjistit, který z nich dává lepší výsledky.</span><span class="sxs-lookup"><span data-stu-id="402eb-306">Later, you can add another algorithm to the experiment and use [Evaluate Model][evaluate-model] to see which one gives better results.</span></span>

4. <span data-ttu-id="402eb-307">Spusťte experiment.</span><span class="sxs-lookup"><span data-stu-id="402eb-307">Run the experiment.</span></span>

<span data-ttu-id="402eb-308">Zobrazte výstup modulu [Vyhodnocení modelu][evaluate-model] tak, že kliknete na výstupní port a vyberete **Vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="402eb-308">To view the output from the [Evaluate Model][evaluate-model] module, click the output port, and then select **Visualize**.</span></span>

<span data-ttu-id="402eb-309">![Výsledky vyhodnocení pro experiment][evaluation-results]
</span><span class="sxs-lookup"><span data-stu-id="402eb-309">![Evaluation results for the experiment][evaluation-results]
</span></span><br/><span data-ttu-id="402eb-310">
***Výsledky vyhodnocení pro experiment***</span><span class="sxs-lookup"><span data-stu-id="402eb-310">
***Evaluation results for the experiment***</span></span>

<span data-ttu-id="402eb-311">Pro náš model se zobrazí následující statistiky:</span><span class="sxs-lookup"><span data-stu-id="402eb-311">The following statistics are shown for our model:</span></span>

- <span data-ttu-id="402eb-312">**Střední absolutní chyba** (MAE): Průměr absolutních chyb (*chyba* je rozdíl mezi předpovězenou a skutečnou hodnotu)</span><span class="sxs-lookup"><span data-stu-id="402eb-312">**Mean Absolute Error** (MAE): The average of absolute errors (an *error* is the difference between the predicted value and the actual value).</span></span>
- <span data-ttu-id="402eb-313">**Odmocnina střední kvadratické chyby** (RMSE): Druhá odmocnina průměru kvadratických chyb předpovědí na základě testovací datové sady</span><span class="sxs-lookup"><span data-stu-id="402eb-313">**Root Mean Squared Error** (RMSE): The square root of the average of squared errors of predictions made on the test dataset.</span></span>
- <span data-ttu-id="402eb-314">**Relativní absolutní chyba**: Průměr absolutních chyb relativních k absolutnímu rozdílu mezi skutečnými hodnotami a průměrem všech skutečných hodnot</span><span class="sxs-lookup"><span data-stu-id="402eb-314">**Relative Absolute Error**: The average of absolute errors relative to the absolute difference between actual values and the average of all actual values.</span></span>
- <span data-ttu-id="402eb-315">**Relativní kvadratická chyba**: Průměr kvadratických chyb relativních ke kvadratickému rozdílu mezi skutečnými hodnotami a průměrem všech skutečných hodnot</span><span class="sxs-lookup"><span data-stu-id="402eb-315">**Relative Squared Error**: The average of squared errors relative to the squared difference between the actual values and the average of all actual values.</span></span>
- <span data-ttu-id="402eb-316">**Koeficient spolehlivosti**: Znám také jako **hodnota spolehlivosti R**, tedy statistická metrika označující kvalitu přizpůsobení modelu datům</span><span class="sxs-lookup"><span data-stu-id="402eb-316">**Coefficient of Determination**: Also known as the **R squared value**, this is a statistical metric indicating how well a model fits the data.</span></span>

<span data-ttu-id="402eb-317">Pro každou statistiku chyb platí, že menší hodnota je lepší.</span><span class="sxs-lookup"><span data-stu-id="402eb-317">For each of the error statistics, smaller is better.</span></span> <span data-ttu-id="402eb-318">Menší hodnota označuje, že předpověď přesněji odpovídá skutečným hodnotám.</span><span class="sxs-lookup"><span data-stu-id="402eb-318">A smaller value indicates that the predictions more closely match the actual values.</span></span> <span data-ttu-id="402eb-319">V případě **koeficientu spolehlivosti** platí, že čím bližší je jeho hodnota hodnotě jedna (1,0), tím lepší jsou předpovědi.</span><span class="sxs-lookup"><span data-stu-id="402eb-319">For **Coefficient of Determination**, the closer its value is to one (1.0), the better the predictions.</span></span>

## <a name="final-experiment"></a><span data-ttu-id="402eb-320">Konečný experiment</span><span class="sxs-lookup"><span data-stu-id="402eb-320">Final experiment</span></span>

<span data-ttu-id="402eb-321">Konečný experiment by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="402eb-321">The final experiment should look something like this:</span></span>

<span data-ttu-id="402eb-322">![Konečný experiment][complete-linear-regression-experiment]
</span><span class="sxs-lookup"><span data-stu-id="402eb-322">![The final experiment][complete-linear-regression-experiment]
</span></span><br/><span data-ttu-id="402eb-323">
***Konečný experiment***</span><span class="sxs-lookup"><span data-stu-id="402eb-323">
***The final experiment***</span></span>

## <a name="next-steps"></a><span data-ttu-id="402eb-324">Další kroky</span><span class="sxs-lookup"><span data-stu-id="402eb-324">Next steps</span></span>

<span data-ttu-id="402eb-325">Právě jste dokončili první kurz strojového učení a máte vytvořený experiment. Teď můžete pokračovat, pokusit se model vylepšit a potom ho nasadit jako prediktivní webovou službu.</span><span class="sxs-lookup"><span data-stu-id="402eb-325">Now that you've completed the first machine learning tutorial and have your experiment set up, you can continue to improve the model and then deploy it as a predictive web service.</span></span>

- <span data-ttu-id="402eb-326">**Iterace pro vylepšení modelu** – Můžete například změnit příznaky, které ve své predikci používáte.</span><span class="sxs-lookup"><span data-stu-id="402eb-326">**Iterate to try to improve the model** - For example, you can change the features you use in your prediction.</span></span> <span data-ttu-id="402eb-327">Dále můžete upravit vlastnosti algoritmu [Lineární regrese][linear-regression] nebo zkusit úplně jiný algoritmus.</span><span class="sxs-lookup"><span data-stu-id="402eb-327">Or you can modify the properties of the [Linear Regression][linear-regression] algorithm or try a different algorithm altogether.</span></span> <span data-ttu-id="402eb-328">Dokonce je možné přidat do experimentu několik algoritmů strojového učení najednou a porovnat dva z nich pomocí modulu [Vyhodnocení modelu][evaluate-model].</span><span class="sxs-lookup"><span data-stu-id="402eb-328">You can even add multiple machine learning algorithms to your experiment at one time and compare two of them by using the [Evaluate Model][evaluate-model] module.</span></span>
<span data-ttu-id="402eb-329">Příklad porovnávání několik modelů v jednom experimentu najdete v tématu věnovaném [porovnání regresorů ](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) na webu [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="402eb-329">For an example of how to compare multiple models in a single experiment, see [Compare Regressors](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span>

    > [!TIP]
    > <span data-ttu-id="402eb-330">Pomocí tlačítka **ULOŽIT JAKO** v dolní části stránky je možné zkopírovat kteroukoli iteraci experimentu.</span><span class="sxs-lookup"><span data-stu-id="402eb-330">To copy any iteration of your experiment, use the **SAVE AS** button at the bottom of the page.</span></span> <span data-ttu-id="402eb-331">Všechny iterace experimentu si můžete zobrazit kliknutím na **ZOBRAZIT HISTORII SPUŠTĚNÍ** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="402eb-331">You can see all the iterations of your experiment by clicking **VIEW RUN HISTORY** at the bottom of the page.</span></span> <span data-ttu-id="402eb-332">Další podrobnosti najdete v tématu [Správa iterací experimentů v nástroji Azure Machine Learning Studio][runhistory].</span><span class="sxs-lookup"><span data-stu-id="402eb-332">For more details, see [Manage experiment iterations in Azure Machine Learning Studio][runhistory].</span></span>

[runhistory]: machine-learning-manage-experiment-iterations.md

- <span data-ttu-id="402eb-333">**Nasazení modelu jako prediktivní webové služby** – Jakmile budete se svým modelem spokojeni, můžete ho nasadit jako webovou službu, která se dá použít k předvídání cen automobilů na základě nových dat.</span><span class="sxs-lookup"><span data-stu-id="402eb-333">**Deploy the model as a predictive web service** - When you're satisfied with your model, you can deploy it as a web service to be used to predict automobile prices by using new data.</span></span> <span data-ttu-id="402eb-334">Další podrobnosti najdete v tématu [Nasazení webové služby Azure Machine Learning][publish].</span><span class="sxs-lookup"><span data-stu-id="402eb-334">For more details, see [Deploy an Azure Machine Learning web service][publish].</span></span>

[publish]: machine-learning-publish-a-machine-learning-web-service.md

<span data-ttu-id="402eb-335">Chcete se dozvědět víc?</span><span class="sxs-lookup"><span data-stu-id="402eb-335">Want to learn more?</span></span> <span data-ttu-id="402eb-336">Rozsáhlejší a podrobnější návod k technikám pro vytváření, natrénování, stanovení skóre a nasazení modelu najdete v tématu [Vývoj prediktivního řešení pomocí služby Azure Machine Learning][walkthrough].</span><span class="sxs-lookup"><span data-stu-id="402eb-336">For a more extensive and detailed walkthrough of the process of creating, training, scoring, and deploying a model, see [Develop a predictive solution by using Azure Machine Learning][walkthrough].</span></span>

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

<!-- temporarily switching GIFs to PNGs to remove animation --> 
[type-automobile]:./media/machine-learning-create-experiment/type-automobile.png
[type-select-columns]:./media/machine-learning-create-experiment/type-select-columns.png
[launch-column-selector]:./media/machine-learning-create-experiment/launch-column-selector.png
[add-comment]:./media/machine-learning-create-experiment/add-comment.png
[connect-clean-to-select]:./media/machine-learning-create-experiment/connect-clean-to-select.png

[set-split-data-percentage]:./media/machine-learning-create-experiment/set-split-data-percentage.png

<!-- temporarily switching GIFs to PNGs to remove animation --> 
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
