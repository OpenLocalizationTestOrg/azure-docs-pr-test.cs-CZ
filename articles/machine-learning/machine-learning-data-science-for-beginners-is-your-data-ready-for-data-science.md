---
title: "aaaIs dat připravené pro vědecké zpracování dat? Zkušební data - Azure Machine Learning | Microsoft Docs"
description: "Přečtěte si hello 4 kritéria pro data toobe připravené pro vědecké zpracování dat. Vědecké zpracování dat pro začátečníky video 2 má toohelp konkrétní příklady s vyhodnocením základní data."
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
ms.openlocfilehash: ef6bb680ace771537157dbdd50a4ccce0a3eb7ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="is-your-data-ready-for-data-science"></a><span data-ttu-id="e6c75-106">Jsou data připravená pro vědecké zkoumání?</span><span class="sxs-lookup"><span data-stu-id="e6c75-106">Is your data ready for data science?</span></span>
## <a name="video-2-data-science-for-beginners-series"></a><span data-ttu-id="e6c75-107">Video 2: Vědecké zpracování dat pro začátečníky řady</span><span class="sxs-lookup"><span data-stu-id="e6c75-107">Video 2: Data Science for Beginners series</span></span>
<span data-ttu-id="e6c75-108">Zjistěte, jak tooevaluate toomake vaše data, zda splňuje základní kritéria toobe připravené pro vědecké zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="e6c75-108">Learn how tooevaluate your data toomake sure it meets basic criteria toobe ready for data science.</span></span>

<span data-ttu-id="e6c75-109">hello tooget naplno hello řady, podívejte se na všechny.</span><span class="sxs-lookup"><span data-stu-id="e6c75-109">tooget hello most out of hello series, watch them all.</span></span> <span data-ttu-id="e6c75-110">[Přejděte toohello seznamu videí](#other-videos-in-this-series)
</span><span class="sxs-lookup"><span data-stu-id="e6c75-110">[Go toohello list of videos](#other-videos-in-this-series)
</span></span><br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a><span data-ttu-id="e6c75-111">Další videa z této série</span><span class="sxs-lookup"><span data-stu-id="e6c75-111">Other videos in this series</span></span>
<span data-ttu-id="e6c75-112">*Vědecké zpracování dat pro začátečníky* je rychlý úvod vědecký toodata pět krátké videa.</span><span class="sxs-lookup"><span data-stu-id="e6c75-112">*Data Science for Beginners* is a quick introduction toodata science in five short videos.</span></span>

* <span data-ttu-id="e6c75-113">Video 1: [hello 5 otázky, odpovědi vědecké účely data](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 14 s min.)*</span><span class="sxs-lookup"><span data-stu-id="e6c75-113">Video 1: [hello 5 questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*</span></span>
* <span data-ttu-id="e6c75-114">Video 2: Je vaše data připravená pro vědecké zpracování dat?</span><span class="sxs-lookup"><span data-stu-id="e6c75-114">Video 2: Is your data ready for data science?</span></span>
* <span data-ttu-id="e6c75-115">Video 3: [zeptejte se vám pomůže odpovědět s daty](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 17 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="e6c75-115">Video 3: [Ask a question you can answer with data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*</span></span>
* <span data-ttu-id="e6c75-116">Video 4: [předpovědi odpověď s jednoduchého modelu](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 42 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="e6c75-116">Video 4: [Predict an answer with a simple model](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*</span></span>
* <span data-ttu-id="e6c75-117">Video 5: [zkopírujte vědecké zpracování dat jiní lidé pracovní toodo](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="e6c75-117">Video 5: [Copy other people's work toodo data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*</span></span>

## <a name="transcript-is-your-data-ready-for-data-science"></a><span data-ttu-id="e6c75-118">Přepis: Je vaše data připravená pro vědecké zpracování dat?</span><span class="sxs-lookup"><span data-stu-id="e6c75-118">Transcript: Is your data ready for data science?</span></span>
<span data-ttu-id="e6c75-119">Vítá příliš "je dat připravené pro vědecké zpracování dat?"</span><span class="sxs-lookup"><span data-stu-id="e6c75-119">Welcome too"Is your data ready for data science?"</span></span> <span data-ttu-id="e6c75-120">Hello druhý video v řadě hello *vědecké zpracování dat pro začátečníky*.</span><span class="sxs-lookup"><span data-stu-id="e6c75-120">hello second video in hello series *Data Science for Beginners*.</span></span>  

<span data-ttu-id="e6c75-121">Předtím, než může poskytnout vědecké zpracování dat hello odpovědi mají, je třeba toogive ho některé vysoce kvalitní suroviny toowork s.</span><span class="sxs-lookup"><span data-stu-id="e6c75-121">Before data science can give you hello answers you want, you have toogive it some high-quality raw materials toowork with.</span></span> <span data-ttu-id="e6c75-122">Stejně jako provádění pizza, hello hello lepší hello přísady začínat, lépe hello konečné produktu.</span><span class="sxs-lookup"><span data-stu-id="e6c75-122">Just like making a pizza, hello better hello ingredients you start with, hello better hello final product.</span></span> 

## <a name="criteria-for-data"></a><span data-ttu-id="e6c75-123">Kritéria pro data</span><span class="sxs-lookup"><span data-stu-id="e6c75-123">Criteria for data</span></span>
<span data-ttu-id="e6c75-124">Ano v případě hello vědecké zpracování dat, existují některé přísady, potřebujeme toopull společně.</span><span class="sxs-lookup"><span data-stu-id="e6c75-124">So, in hello case of data science, there are some ingredients that we need toopull together.</span></span>

<span data-ttu-id="e6c75-125">Potřebujeme data, která jsou:</span><span class="sxs-lookup"><span data-stu-id="e6c75-125">We need data that is:</span></span>

* <span data-ttu-id="e6c75-126">Relevantní</span><span class="sxs-lookup"><span data-stu-id="e6c75-126">Relevant</span></span>
* <span data-ttu-id="e6c75-127">připojení</span><span class="sxs-lookup"><span data-stu-id="e6c75-127">Connected</span></span>
* <span data-ttu-id="e6c75-128">Přesná</span><span class="sxs-lookup"><span data-stu-id="e6c75-128">Accurate</span></span>
* <span data-ttu-id="e6c75-129">Dostatek toowork s</span><span class="sxs-lookup"><span data-stu-id="e6c75-129">Enough toowork with</span></span>

## <a name="is-your-data-relevant"></a><span data-ttu-id="e6c75-130">Je relevantní data?</span><span class="sxs-lookup"><span data-stu-id="e6c75-130">Is your data relevant?</span></span>
<span data-ttu-id="e6c75-131">Takže první složka hello - potřebujeme data, která jsou relevantní.</span><span class="sxs-lookup"><span data-stu-id="e6c75-131">So hello first ingredient - we need data that's relevant.</span></span>

![Relevantní data oproti nahromadění irelevantních dat – vyhodnocení dat.](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

<span data-ttu-id="e6c75-133">Podívejte se na hello tabulky na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="e6c75-133">Look at hello table on hello left.</span></span> <span data-ttu-id="e6c75-134">Jsme splněny sedm lidé mimo řádky Boston, měří jejich krve alkohol úroveň, hello Red Sox batting průměr v jejich poslední hře a cena hello mléka v hello nejbližší pohodlí úložiště.</span><span class="sxs-lookup"><span data-stu-id="e6c75-134">We met seven people outside of Boston bars, measured their blood alcohol level, hello Red Sox batting average in their last game, and hello price of milk in hello nearest convenience store.</span></span>

<span data-ttu-id="e6c75-135">Toto jsou všechny perfektně legitimní data.</span><span class="sxs-lookup"><span data-stu-id="e6c75-135">This is all perfectly legitimate data.</span></span> <span data-ttu-id="e6c75-136">Pouze chyby je, že není relevantní.</span><span class="sxs-lookup"><span data-stu-id="e6c75-136">It’s only fault is that it isn’t relevant.</span></span> <span data-ttu-id="e6c75-137">Není žádný zřejmé vztah mezi tato čísla.</span><span class="sxs-lookup"><span data-stu-id="e6c75-137">There's no obvious relationship between these numbers.</span></span> <span data-ttu-id="e6c75-138">Pokud I hello aktuální cena mléka a hello Red Sox batting průměru, neexistuje žádný způsob, který by mohl snadno uhodnout Moje krve obsahu.</span><span class="sxs-lookup"><span data-stu-id="e6c75-138">If I gave you hello current price of milk and hello Red Sox batting average, there's no way you could guess my blood alcohol content.</span></span>

<span data-ttu-id="e6c75-139">Nyní prohlédněte hello tabulky na pravé hello.</span><span class="sxs-lookup"><span data-stu-id="e6c75-139">Now look at hello table on hello right.</span></span> <span data-ttu-id="e6c75-140">Tato doba jsme měří každá osoba textu hello velkokapacitního a počtu výskytů počet nápojů, které jste měly.</span><span class="sxs-lookup"><span data-stu-id="e6c75-140">This time we measured each person’s body mass and counted hello number of drinks they’ve had.</span></span>  <span data-ttu-id="e6c75-141">Hello čísla v jednotlivých řádcích jsou teď další relevantní tooeach.</span><span class="sxs-lookup"><span data-stu-id="e6c75-141">hello numbers in each row are now relevant tooeach other.</span></span> <span data-ttu-id="e6c75-142">Pokud vám I Dal Moje textu velkokapacitních a hello počet Margaritas I jste měli, můžete dokonce vytvářet odhad v mé krve alkohol obsahu.</span><span class="sxs-lookup"><span data-stu-id="e6c75-142">If I gave you my body mass and hello number of Margaritas I've had, you could make a guess at my blood alcohol content.</span></span>

## <a name="do-you-have-connected-data"></a><span data-ttu-id="e6c75-143">Připojení dat?</span><span class="sxs-lookup"><span data-stu-id="e6c75-143">Do you have connected data?</span></span>
<span data-ttu-id="e6c75-144">Další složky Hello jsou připojené data.</span><span class="sxs-lookup"><span data-stu-id="e6c75-144">hello next ingredient is connected data.</span></span>

![Připojení dat oproti odpojené data – data kritéria data připravena](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

<span data-ttu-id="e6c75-146">Tady jsou některé relevantní data na kvalitu hello hamburgers: gril teploty, patty váhy a hodnocení v hello místní jídlo katalogu.</span><span class="sxs-lookup"><span data-stu-id="e6c75-146">Here is some relevant data on hello quality of hamburgers: grill temperature, patty weight, and rating in hello local food magazine.</span></span> <span data-ttu-id="e6c75-147">Ale Všimněte si hello mezery v hello tabulky na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="e6c75-147">But notice hello gaps in hello table on hello left.</span></span>

<span data-ttu-id="e6c75-148">Většina datové sady chybí některé hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e6c75-148">Most data sets are missing some values.</span></span> <span data-ttu-id="e6c75-149">Je běžné toohave díry takto a existují způsoby toowork je obcházet.</span><span class="sxs-lookup"><span data-stu-id="e6c75-149">It's common toohave holes like this and there are ways toowork around them.</span></span> <span data-ttu-id="e6c75-150">Ale pokud je příliš mnoho chybí, data se zahájí toolook jako sýr mezi.</span><span class="sxs-lookup"><span data-stu-id="e6c75-150">But if there's too much missing, your data begins toolook like Swiss cheese.</span></span>

<span data-ttu-id="e6c75-151">Pokud se podíváte na hello tabulky na levé straně hello, je proto mnohem chybějící data, je pevný toocome až s jakýmkoli vztah mezi gril teploty a patty váhy.</span><span class="sxs-lookup"><span data-stu-id="e6c75-151">If you look at hello table on hello left, there's so much missing data, it's hard toocome up with any kind of relationship between grill temperature and patty weight.</span></span> <span data-ttu-id="e6c75-152">Toto je příklad odpojené data.</span><span class="sxs-lookup"><span data-stu-id="e6c75-152">This is an example of disconnected data.</span></span>

<span data-ttu-id="e6c75-153">Hello tabulky na pravé straně hello ale je plný a dokončete – příklad připojené data.</span><span class="sxs-lookup"><span data-stu-id="e6c75-153">hello table on hello right, though, is full and complete - an example of connected data.</span></span>

## <a name="is-your-data-accurate"></a><span data-ttu-id="e6c75-154">Je přesná data?</span><span class="sxs-lookup"><span data-stu-id="e6c75-154">Is your data accurate?</span></span>
<span data-ttu-id="e6c75-155">Hello další složky, které potřebujeme je přesnost.</span><span class="sxs-lookup"><span data-stu-id="e6c75-155">hello next ingredient we need is accuracy.</span></span> <span data-ttu-id="e6c75-156">Tady jsou čtyři cíle, rádi bychom znali toohit se šipkami.</span><span class="sxs-lookup"><span data-stu-id="e6c75-156">Here are four targets that we’d like toohit with arrows.</span></span>

![Přesná data oproti nepřesných dat. - kritéria dat](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

<span data-ttu-id="e6c75-158">Podívejte se na cíl hello v pravé horní části hello.</span><span class="sxs-lookup"><span data-stu-id="e6c75-158">Look at hello target in hello upper right.</span></span> <span data-ttu-id="e6c75-159">My úzkou seskupení právo kolem terč hello.</span><span class="sxs-lookup"><span data-stu-id="e6c75-159">We’ve got a tight grouping right around hello bullseye.</span></span> <span data-ttu-id="e6c75-160">Který je samozřejmě přesná.</span><span class="sxs-lookup"><span data-stu-id="e6c75-160">That, of course, is accurate.</span></span> <span data-ttu-id="e6c75-161">Oddly v jazyku hello vědecké zpracování dat, naše výkonu cílové vpravo hello pod ním také považuje za přesná.</span><span class="sxs-lookup"><span data-stu-id="e6c75-161">Oddly, in hello language of data science, our performance on hello target right below it is also considered accurate.</span></span>

<span data-ttu-id="e6c75-162">Pokud byste byli toomap se na centrum hello tyto šipek, zobrazí se, že se jedná o velmi zavřít toohello terč.</span><span class="sxs-lookup"><span data-stu-id="e6c75-162">If you were toomap out hello center of these arrows, you'd see that it's very close toohello bullseye.</span></span> <span data-ttu-id="e6c75-163">Hello dvojice šipek, které jsou všechny kolem hello cíl, šíření, se považují za nepřesný, ale budou se soustředí na hello terč, se považují za přesná.</span><span class="sxs-lookup"><span data-stu-id="e6c75-163">hello arrows are spread out all around hello target, so they're considered imprecise, but they're centered around hello bullseye, so they're considered accurate.</span></span>

<span data-ttu-id="e6c75-164">Nyní se podívejte na cílové levém hello.</span><span class="sxs-lookup"><span data-stu-id="e6c75-164">Now look at hello upper-left target.</span></span> <span data-ttu-id="e6c75-165">Zde naše šipky dosáhl velmi blízko sebe úzkou seskupení.</span><span class="sxs-lookup"><span data-stu-id="e6c75-165">Here our arrows hit very close together, a tight grouping.</span></span> <span data-ttu-id="e6c75-166">Jsou přesné, ale protože je mimo hello terč hello centra jsou nesprávné.</span><span class="sxs-lookup"><span data-stu-id="e6c75-166">They're precise, but they're inaccurate because hello center is way off hello bullseye.</span></span> <span data-ttu-id="e6c75-167">A samozřejmě hello šipky v hello okraje v levém dolním cíl jsou nesprávné a nepřesný.</span><span class="sxs-lookup"><span data-stu-id="e6c75-167">And, of course, hello arrows in hello bottom-left target are both inaccurate and imprecise.</span></span> <span data-ttu-id="e6c75-168">Tato archer potřebuje další postup.</span><span class="sxs-lookup"><span data-stu-id="e6c75-168">This archer needs more practice.</span></span>

## <a name="do-you-have-enough-data-toowork-with"></a><span data-ttu-id="e6c75-169">Máte dostatek dat toowork s?</span><span class="sxs-lookup"><span data-stu-id="e6c75-169">Do you have enough data toowork with?</span></span>
<span data-ttu-id="e6c75-170">Nakonec složka #4 - potřebujeme toohave dostatek dat.</span><span class="sxs-lookup"><span data-stu-id="e6c75-170">Finally, ingredient #4 - we need toohave enough data.</span></span>

![Máte dostatek dat pro analýzu?](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

<span data-ttu-id="e6c75-173">Představte si každý datový bod v tabulce, že je tahu štětce v malování.</span><span class="sxs-lookup"><span data-stu-id="e6c75-173">Think of each data point in your table as being a brush stroke in a painting.</span></span> <span data-ttu-id="e6c75-174">Pokud máte pouze několik z nich, může být poměrně přibližné hello Malování – je pevný tootell co je.</span><span class="sxs-lookup"><span data-stu-id="e6c75-174">If you have only a few of them, hello painting can be pretty fuzzy - it's hard tootell what it is.</span></span>

<span data-ttu-id="e6c75-175">Pokud přidáte některé další tahy štětce, vaše Malování spustí tooget málo ostřejší.</span><span class="sxs-lookup"><span data-stu-id="e6c75-175">If you add some more brush strokes, then your painting starts tooget a little sharper.</span></span>

<span data-ttu-id="e6c75-176">Až budete mít sotva dostatek tahy, uvidíte jenom dostatek toomake některé široký rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="e6c75-176">When you have barely enough strokes, you can see just enough toomake some broad decisions.</span></span> <span data-ttu-id="e6c75-177">Je někde že chci může toovisit?</span><span class="sxs-lookup"><span data-stu-id="e6c75-177">Is it somewhere I might want toovisit?</span></span> <span data-ttu-id="e6c75-178">Vypadá to, jasně, která vypadá jako čisté horních – Ano, který je, kde bude na dovolenou.</span><span class="sxs-lookup"><span data-stu-id="e6c75-178">It looks bright, that looks like clean water – yes, that’s where I’m going on vacation.</span></span>

<span data-ttu-id="e6c75-179">Při přidávání více dat, hello obrázek bude jasnější a je možné provádět podrobnější rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="e6c75-179">As you add more data, hello picture becomes clearer and you can make more detailed decisions.</span></span> <span data-ttu-id="e6c75-180">Nyní můžete vypadají hello tři hotels na levém bank hello.</span><span class="sxs-lookup"><span data-stu-id="e6c75-180">Now I can look at hello three hotels on hello left bank.</span></span> <span data-ttu-id="e6c75-181">Víte, opravdu líbí hello architektury funkce hello, jeden v popředí hello.</span><span class="sxs-lookup"><span data-stu-id="e6c75-181">You know, I really like hello architectural features of hello one in hello foreground.</span></span> <span data-ttu-id="e6c75-182">I budete zůstat, na třetí podlaží hello.</span><span class="sxs-lookup"><span data-stu-id="e6c75-182">I’ll stay there, on hello third floor.</span></span>

<span data-ttu-id="e6c75-183">S daty, která je relevantní, připojené, přesné a dostatek, budeme mít všechny složky hello potřebujeme toodo některé vědecké zpracování dat vysoké kvality.</span><span class="sxs-lookup"><span data-stu-id="e6c75-183">With data that's relevant, connected, accurate, and enough, we have all hello ingredients we need toodo some high-quality data science.</span></span>

<span data-ttu-id="e6c75-184">Být jisti toocheck out hello jiné čtyři videa v *vědecké zpracování dat pro začátečníky* z Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="e6c75-184">Be sure toocheck out hello other four videos in *Data Science for Beginners* from Microsoft Azure Machine Learning.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6c75-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e6c75-185">Next steps</span></span>
* [<span data-ttu-id="e6c75-186">Zkuste prvního experimentu vědecké účely data nástroje Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="e6c75-186">Try a first data science experiment with Machine Learning Studio</span></span>](machine-learning-create-experiment.md)
* [<span data-ttu-id="e6c75-187">Získat tooMachine Úvod učení v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="e6c75-187">Get an introduction tooMachine Learning on Microsoft Azure</span></span>](machine-learning-what-is-machine-learning.md)
