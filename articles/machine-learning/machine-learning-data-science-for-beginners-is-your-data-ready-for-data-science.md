---
title: "Jsou data připravená pro vědecké zkoumání? Zkušební data - Azure Machine Learning | Microsoft Docs"
description: "Přečtěte si 4 kritéria pro data bude připravená pro vědecké zpracování dat. Vědecké zpracování dat pro začátečníky video 2 má konkrétní příklady usnadní vyhodnocení základní data."
keywords: "relevantní data vyhodnotit data, připravte dat, data kritéria, data připravena"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: d502062c-da70-4b21-9054-0bfd9902612e
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: c4a8bc11aec2f71796f589c0af54cc92253e5180
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="is-your-data-ready-for-data-science"></a><span data-ttu-id="f0264-106">Jsou data připravená pro vědecké zkoumání?</span><span class="sxs-lookup"><span data-stu-id="f0264-106">Is your data ready for data science?</span></span>
## <a name="video-2-data-science-for-beginners-series"></a><span data-ttu-id="f0264-107">Video 2: Vědecké zpracování dat pro začátečníky řady</span><span class="sxs-lookup"><span data-stu-id="f0264-107">Video 2: Data Science for Beginners series</span></span>
<span data-ttu-id="f0264-108">Zjistěte, jak vyhodnotit vaše data a ujistěte se, že splňuje základní kritéria bude připravená pro vědecké zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="f0264-108">Learn how to evaluate your data to make sure it meets basic criteria to be ready for data science.</span></span>

<span data-ttu-id="f0264-109">Získejte maximum z řady, můžete sledujte všechny.</span><span class="sxs-lookup"><span data-stu-id="f0264-109">To get the most out of the series, watch them all.</span></span> <span data-ttu-id="f0264-110">[Přejděte do seznamu videí](#other-videos-in-this-series)
</span><span class="sxs-lookup"><span data-stu-id="f0264-110">[Go to the list of videos](#other-videos-in-this-series)
</span></span><br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a><span data-ttu-id="f0264-111">Další videa z této série</span><span class="sxs-lookup"><span data-stu-id="f0264-111">Other videos in this series</span></span>
<span data-ttu-id="f0264-112">*Vědecké zpracování dat pro začátečníky* je rychlý úvod do vědecké zpracování dat v pěti krátké videa.</span><span class="sxs-lookup"><span data-stu-id="f0264-112">*Data Science for Beginners* is a quick introduction to data science in five short videos.</span></span>

* <span data-ttu-id="f0264-113">Video 1: [5 otázky, odpovědi vědecké účely data](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 14 s min.)*</span><span class="sxs-lookup"><span data-stu-id="f0264-113">Video 1: [The 5 questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*</span></span>
* <span data-ttu-id="f0264-114">Video 2: Je vaše data připravená pro vědecké zpracování dat?</span><span class="sxs-lookup"><span data-stu-id="f0264-114">Video 2: Is your data ready for data science?</span></span>
* <span data-ttu-id="f0264-115">Video 3: [zeptejte se vám pomůže odpovědět s daty](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 17 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="f0264-115">Video 3: [Ask a question you can answer with data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*</span></span>
* <span data-ttu-id="f0264-116">Video 4: [předpovědi odpověď s jednoduchého modelu](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 42 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="f0264-116">Video 4: [Predict an answer with a simple model](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*</span></span>
* <span data-ttu-id="f0264-117">Video 5: [zkopírujte jiní lidé práce uděláte vědecké zpracování dat](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="f0264-117">Video 5: [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*</span></span>

## <a name="transcript-is-your-data-ready-for-data-science"></a><span data-ttu-id="f0264-118">Přepis: Je vaše data připravená pro vědecké zpracování dat?</span><span class="sxs-lookup"><span data-stu-id="f0264-118">Transcript: Is your data ready for data science?</span></span>
<span data-ttu-id="f0264-119">Vítá vás "Je dat připravené pro vědecké zpracování dat?"</span><span class="sxs-lookup"><span data-stu-id="f0264-119">Welcome to "Is your data ready for data science?"</span></span> <span data-ttu-id="f0264-120">druhý video v řadě *vědecké zpracování dat pro začátečníky*.</span><span class="sxs-lookup"><span data-stu-id="f0264-120">the second video in the series *Data Science for Beginners*.</span></span>  

<span data-ttu-id="f0264-121">Než odpovědi, které chcete, můžete udělit vědecké zpracování dat, musíte jí přidělit suroviny některé vysoce kvalitní pro práci s.</span><span class="sxs-lookup"><span data-stu-id="f0264-121">Before data science can give you the answers you want, you have to give it some high-quality raw materials to work with.</span></span> <span data-ttu-id="f0264-122">Stejně jako provedení pizza, tím lépe složek, které se začíná s, tím lepší konečné produktu.</span><span class="sxs-lookup"><span data-stu-id="f0264-122">Just like making a pizza, the better the ingredients you start with, the better the final product.</span></span> 

## <a name="criteria-for-data"></a><span data-ttu-id="f0264-123">Kritéria pro data</span><span class="sxs-lookup"><span data-stu-id="f0264-123">Criteria for data</span></span>
<span data-ttu-id="f0264-124">Ano v případě vědecké zpracování dat, nejsou některé faktory, které je potřeba pro vyžádání obsahu společně.</span><span class="sxs-lookup"><span data-stu-id="f0264-124">So, in the case of data science, there are some ingredients that we need to pull together.</span></span>

<span data-ttu-id="f0264-125">Potřebujeme data, která jsou:</span><span class="sxs-lookup"><span data-stu-id="f0264-125">We need data that is:</span></span>

* <span data-ttu-id="f0264-126">Relevantní</span><span class="sxs-lookup"><span data-stu-id="f0264-126">Relevant</span></span>
* <span data-ttu-id="f0264-127">připojení</span><span class="sxs-lookup"><span data-stu-id="f0264-127">Connected</span></span>
* <span data-ttu-id="f0264-128">Přesná</span><span class="sxs-lookup"><span data-stu-id="f0264-128">Accurate</span></span>
* <span data-ttu-id="f0264-129">Dost pro práci s</span><span class="sxs-lookup"><span data-stu-id="f0264-129">Enough to work with</span></span>

## <a name="is-your-data-relevant"></a><span data-ttu-id="f0264-130">Je relevantní data?</span><span class="sxs-lookup"><span data-stu-id="f0264-130">Is your data relevant?</span></span>
<span data-ttu-id="f0264-131">Proto je první složka - potřebujeme data, která jsou relevantní.</span><span class="sxs-lookup"><span data-stu-id="f0264-131">So the first ingredient - we need data that's relevant.</span></span>

![Relevantní data oproti nahromadění irelevantních dat – vyhodnocení dat.](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

<span data-ttu-id="f0264-133">Podívejte se na tabulky na levé straně.</span><span class="sxs-lookup"><span data-stu-id="f0264-133">Look at the table on the left.</span></span> <span data-ttu-id="f0264-134">Jsme splněny sedm lidé mimo řádky Boston, měří jejich krve alkohol úroveň, Red Sox průměr batting v jejich poslední hře a cena mléka v úložišti nejbližší pohodlí.</span><span class="sxs-lookup"><span data-stu-id="f0264-134">We met seven people outside of Boston bars, measured their blood alcohol level, the Red Sox batting average in their last game, and the price of milk in the nearest convenience store.</span></span>

<span data-ttu-id="f0264-135">Toto jsou všechny perfektně legitimní data.</span><span class="sxs-lookup"><span data-stu-id="f0264-135">This is all perfectly legitimate data.</span></span> <span data-ttu-id="f0264-136">Pouze chyby je, že není relevantní.</span><span class="sxs-lookup"><span data-stu-id="f0264-136">It’s only fault is that it isn’t relevant.</span></span> <span data-ttu-id="f0264-137">Není žádný zřejmé vztah mezi tato čísla.</span><span class="sxs-lookup"><span data-stu-id="f0264-137">There's no obvious relationship between these numbers.</span></span> <span data-ttu-id="f0264-138">Pokud vám I Dal aktuální cena mléka a průměr batting Red Sox, neexistuje způsob může uhodnout Moje krve obsahu.</span><span class="sxs-lookup"><span data-stu-id="f0264-138">If I gave you the current price of milk and the Red Sox batting average, there's no way you could guess my blood alcohol content.</span></span>

<span data-ttu-id="f0264-139">Nyní podívejte se na tabulky na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="f0264-139">Now look at the table on the right.</span></span> <span data-ttu-id="f0264-140">Tato doba jsme měří každá osoba body velkokapacitních a počítá počet nápojů jste měly.</span><span class="sxs-lookup"><span data-stu-id="f0264-140">This time we measured each person’s body mass and counted the number of drinks they’ve had.</span></span>  <span data-ttu-id="f0264-141">Čísla v jednotlivých řádcích jsou nyní relevantní pro sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="f0264-141">The numbers in each row are now relevant to each other.</span></span> <span data-ttu-id="f0264-142">Pokud vám I Dal Moje textu velkokapacitních a počet Margaritas I jste měli, můžete dokonce vytvářet odhad v mé krve alkohol obsahu.</span><span class="sxs-lookup"><span data-stu-id="f0264-142">If I gave you my body mass and the number of Margaritas I've had, you could make a guess at my blood alcohol content.</span></span>

## <a name="do-you-have-connected-data"></a><span data-ttu-id="f0264-143">Připojení dat?</span><span class="sxs-lookup"><span data-stu-id="f0264-143">Do you have connected data?</span></span>
<span data-ttu-id="f0264-144">Další složky jsou připojené data.</span><span class="sxs-lookup"><span data-stu-id="f0264-144">The next ingredient is connected data.</span></span>

![Připojení dat oproti odpojené data – data kritéria data připravena](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

<span data-ttu-id="f0264-146">Tady jsou některé relevantní data na kvalitu hamburgers: gril teploty, patty váhy a hodnocení v místní jídlo katalogu.</span><span class="sxs-lookup"><span data-stu-id="f0264-146">Here is some relevant data on the quality of hamburgers: grill temperature, patty weight, and rating in the local food magazine.</span></span> <span data-ttu-id="f0264-147">Ale Všimněte si, mezer v tabulce na levé straně.</span><span class="sxs-lookup"><span data-stu-id="f0264-147">But notice the gaps in the table on the left.</span></span>

<span data-ttu-id="f0264-148">Většina datové sady chybí některé hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f0264-148">Most data sets are missing some values.</span></span> <span data-ttu-id="f0264-149">Se běžně používají díry takto a existují způsoby, jak je obejít.</span><span class="sxs-lookup"><span data-stu-id="f0264-149">It's common to have holes like this and there are ways to work around them.</span></span> <span data-ttu-id="f0264-150">Ale pokud je příliš mnoho chybí, začne vypadat mezi sýr vaše data.</span><span class="sxs-lookup"><span data-stu-id="f0264-150">But if there's too much missing, your data begins to look like Swiss cheese.</span></span>

<span data-ttu-id="f0264-151">Pokud se podíváte na tabulky na levé straně, je mnoho chybějící data, je pevný spolu s jakýmkoli vztah mezi mřížkou teploty a patty váhy.</span><span class="sxs-lookup"><span data-stu-id="f0264-151">If you look at the table on the left, there's so much missing data, it's hard to come up with any kind of relationship between grill temperature and patty weight.</span></span> <span data-ttu-id="f0264-152">Toto je příklad odpojené data.</span><span class="sxs-lookup"><span data-stu-id="f0264-152">This is an example of disconnected data.</span></span>

<span data-ttu-id="f0264-153">V tabulce na pravé straně, ale je plný a dokončete – příklad připojené data.</span><span class="sxs-lookup"><span data-stu-id="f0264-153">The table on the right, though, is full and complete - an example of connected data.</span></span>

## <a name="is-your-data-accurate"></a><span data-ttu-id="f0264-154">Je přesná data?</span><span class="sxs-lookup"><span data-stu-id="f0264-154">Is your data accurate?</span></span>
<span data-ttu-id="f0264-155">Další složky, které potřebujeme je přesnost.</span><span class="sxs-lookup"><span data-stu-id="f0264-155">The next ingredient we need is accuracy.</span></span> <span data-ttu-id="f0264-156">Tady jsou čtyři cíle, které jsme chtěli dosáhl se šipkami.</span><span class="sxs-lookup"><span data-stu-id="f0264-156">Here are four targets that we’d like to hit with arrows.</span></span>

![Přesná data oproti nepřesných dat. - kritéria dat](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

<span data-ttu-id="f0264-158">Podívejte se na cíl v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="f0264-158">Look at the target in the upper right.</span></span> <span data-ttu-id="f0264-159">My úzkou seskupení právo kolem terč.</span><span class="sxs-lookup"><span data-stu-id="f0264-159">We’ve got a tight grouping right around the bullseye.</span></span> <span data-ttu-id="f0264-160">Který je samozřejmě přesná.</span><span class="sxs-lookup"><span data-stu-id="f0264-160">That, of course, is accurate.</span></span> <span data-ttu-id="f0264-161">Oddly v jazyce vědecké zpracování dat, naše výkonu na pravé straně cíl pod ním také považuje za přesná.</span><span class="sxs-lookup"><span data-stu-id="f0264-161">Oddly, in the language of data science, our performance on the target right below it is also considered accurate.</span></span>

<span data-ttu-id="f0264-162">Pokud byste chtěli zmapování center tyto šipek, zobrazí se, se velmi nachází blízko terč.</span><span class="sxs-lookup"><span data-stu-id="f0264-162">If you were to map out the center of these arrows, you'd see that it's very close to the bullseye.</span></span> <span data-ttu-id="f0264-163">Šipky jsou všechny kolem cíl, šíření, se považují za nepřesný, ale budou se soustředí na terč, aby se považovat za přesná.</span><span class="sxs-lookup"><span data-stu-id="f0264-163">The arrows are spread out all around the target, so they're considered imprecise, but they're centered around the bullseye, so they're considered accurate.</span></span>

<span data-ttu-id="f0264-164">Nyní se podívejte na levém cíl.</span><span class="sxs-lookup"><span data-stu-id="f0264-164">Now look at the upper-left target.</span></span> <span data-ttu-id="f0264-165">Zde naše šipky dosáhl velmi blízko sebe úzkou seskupení.</span><span class="sxs-lookup"><span data-stu-id="f0264-165">Here our arrows hit very close together, a tight grouping.</span></span> <span data-ttu-id="f0264-166">Jsou to přesné, ale jsou nesprávné, protože je mimo terč centru.</span><span class="sxs-lookup"><span data-stu-id="f0264-166">They're precise, but they're inaccurate because the center is way off the bullseye.</span></span> <span data-ttu-id="f0264-167">A samozřejmě šipky v okraje v levém dolním cíl jsou nesprávné a nepřesný.</span><span class="sxs-lookup"><span data-stu-id="f0264-167">And, of course, the arrows in the bottom-left target are both inaccurate and imprecise.</span></span> <span data-ttu-id="f0264-168">Tato archer potřebuje další postup.</span><span class="sxs-lookup"><span data-stu-id="f0264-168">This archer needs more practice.</span></span>

## <a name="do-you-have-enough-data-to-work-with"></a><span data-ttu-id="f0264-169">Máte dostatek dat pro práci s?</span><span class="sxs-lookup"><span data-stu-id="f0264-169">Do you have enough data to work with?</span></span>
<span data-ttu-id="f0264-170">Nakonec složka #4 -, je potřeba mít dostatek data.</span><span class="sxs-lookup"><span data-stu-id="f0264-170">Finally, ingredient #4 - we need to have enough data.</span></span>

![Máte dostatek dat pro analýzu?](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

<span data-ttu-id="f0264-173">Představte si každý datový bod v tabulce, že je tahu štětce v malování.</span><span class="sxs-lookup"><span data-stu-id="f0264-173">Think of each data point in your table as being a brush stroke in a painting.</span></span> <span data-ttu-id="f0264-174">Pokud máte pouze několik z nich, může být poměrně přibližné Malování – je obtížné zjistit, co je.</span><span class="sxs-lookup"><span data-stu-id="f0264-174">If you have only a few of them, the painting can be pretty fuzzy - it's hard to tell what it is.</span></span>

<span data-ttu-id="f0264-175">Pokud přidáte některé další tahy štětce, vaše Malování spustí získat trochu ostřejší.</span><span class="sxs-lookup"><span data-stu-id="f0264-175">If you add some more brush strokes, then your painting starts to get a little sharper.</span></span>

<span data-ttu-id="f0264-176">Až budete mít sotva dostatek tahy, uvidíte jenom dost pro širokou rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="f0264-176">When you have barely enough strokes, you can see just enough to make some broad decisions.</span></span> <span data-ttu-id="f0264-177">Je někde, který může chcete navštívit?</span><span class="sxs-lookup"><span data-stu-id="f0264-177">Is it somewhere I might want to visit?</span></span> <span data-ttu-id="f0264-178">Vypadá to, jasně, která vypadá jako čisté horních – Ano, který je, kde bude na dovolenou.</span><span class="sxs-lookup"><span data-stu-id="f0264-178">It looks bright, that looks like clean water – yes, that’s where I’m going on vacation.</span></span>

<span data-ttu-id="f0264-179">Při přidávání více dat, obrázek bude jasnější a je možné provádět podrobnější rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="f0264-179">As you add more data, the picture becomes clearer and you can make more detailed decisions.</span></span> <span data-ttu-id="f0264-180">Teď můžou se podívat na tři hotels na levém bank.</span><span class="sxs-lookup"><span data-stu-id="f0264-180">Now I can look at the three hotels on the left bank.</span></span> <span data-ttu-id="f0264-181">Víte, opravdu líbí architektury funkce v popředí.</span><span class="sxs-lookup"><span data-stu-id="f0264-181">You know, I really like the architectural features of the one in the foreground.</span></span> <span data-ttu-id="f0264-182">I budete zůstat, na třetí podlaží.</span><span class="sxs-lookup"><span data-stu-id="f0264-182">I’ll stay there, on the third floor.</span></span>

<span data-ttu-id="f0264-183">Data, která jsou relevantní, připojené, přesný a dostatečně jsme mít všechny složky, je potřeba provést některé vědecké zpracování dat vysoké kvality.</span><span class="sxs-lookup"><span data-stu-id="f0264-183">With data that's relevant, connected, accurate, and enough, we have all the ingredients we need to do some high-quality data science.</span></span>

<span data-ttu-id="f0264-184">Nezapomeňte si projděte si další čtyři videa v *vědecké zpracování dat pro začátečníky* z Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="f0264-184">Be sure to check out the other four videos in *Data Science for Beginners* from Microsoft Azure Machine Learning.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0264-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0264-185">Next steps</span></span>
* [<span data-ttu-id="f0264-186">Zkuste prvního experimentu vědecké účely data nástroje Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="f0264-186">Try a first data science experiment with Machine Learning Studio</span></span>](machine-learning-create-experiment.md)
* [<span data-ttu-id="f0264-187">Získejte Úvod do Machine Learning v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f0264-187">Get an introduction to Machine Learning on Microsoft Azure</span></span>](machine-learning-what-is-machine-learning.md)
