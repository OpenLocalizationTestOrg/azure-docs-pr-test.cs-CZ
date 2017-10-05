---
title: "Požádejte otázku dat může odpovědět - data vědecké účely úlohy - Azure Machine Learning | Microsoft Docs"
description: "Naučte se formulovali dotaz vědecké účely sharp data v vědecké zpracování dat pro začátečníky video 3. Obsahuje porovnání klasifikace a regrese otázky."
keywords: "datové vědy problémy, vědecké účely otázky ohledně dat, formulovali otázky, regrese otázky, klasifikace otázky, sharp otázku"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: 5b9501e3-9964-417a-8ffc-8913103da77b
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: 0495dbab72024e504ae33d35f16a212a2084bc10
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="ask-a-question-you-can-answer-with-data"></a><span data-ttu-id="18acf-105">Položení otázky, na kterou lze odpovědět pomocí dat</span><span class="sxs-lookup"><span data-stu-id="18acf-105">Ask a question you can answer with data</span></span>
## <a name="video-3-data-science-for-beginners-series"></a><span data-ttu-id="18acf-106">Video 3: Vědecké zpracování dat pro začátečníky řady</span><span class="sxs-lookup"><span data-stu-id="18acf-106">Video 3: Data Science for Beginners series</span></span>
<span data-ttu-id="18acf-107">Naučte se formulovali problému vědecké účely dat do svůj dotaz vědecké zpracování dat pro začátečníky video 3.</span><span class="sxs-lookup"><span data-stu-id="18acf-107">Learn how to formulate a data science problem into a question in Data Science for Beginners video 3.</span></span> <span data-ttu-id="18acf-108">Toto video obsahuje porovnání otázky pro klasifikaci a regrese algoritmy.</span><span class="sxs-lookup"><span data-stu-id="18acf-108">This video includes a comparison of questions for classification and regression algorithms.</span></span>

<span data-ttu-id="18acf-109">Získejte maximum z řady, můžete sledujte všechny.</span><span class="sxs-lookup"><span data-stu-id="18acf-109">To get the most out of the series, watch them all.</span></span> <span data-ttu-id="18acf-110">[Přejděte do seznamu videí](#other-videos-in-this-series)
</span><span class="sxs-lookup"><span data-stu-id="18acf-110">[Go to the list of videos](#other-videos-in-this-series)
</span></span><br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Data-science-for-beginners-Ask-a-question-you-can-answer-with-data/player]
>
>

## <a name="other-videos-in-this-series"></a><span data-ttu-id="18acf-111">Další videa z této série</span><span class="sxs-lookup"><span data-stu-id="18acf-111">Other videos in this series</span></span>
<span data-ttu-id="18acf-112">*Vědecké zpracování dat pro začátečníky* je rychlý úvod do vědecké zpracování dat v pěti krátké videa.</span><span class="sxs-lookup"><span data-stu-id="18acf-112">*Data Science for Beginners* is a quick introduction to data science in five short videos.</span></span>

* <span data-ttu-id="18acf-113">Video 1: [5 otázky, odpovědi vědecké účely data](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 14 s min.)*</span><span class="sxs-lookup"><span data-stu-id="18acf-113">Video 1: [The 5 questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*</span></span>
* <span data-ttu-id="18acf-114">Video 2: [vašich dat je připravený pro vědecké zpracování dat?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md)</span><span class="sxs-lookup"><span data-stu-id="18acf-114">Video 2: [Is your data ready for data science?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md)</span></span> <span data-ttu-id="18acf-115">*(4 56 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="18acf-115">*(4 min 56 sec)*</span></span>
* <span data-ttu-id="18acf-116">Video 3: Položte dotaz, který vám pomůže odpovědět s daty</span><span class="sxs-lookup"><span data-stu-id="18acf-116">Video 3: Ask a question you can answer with data</span></span>
* <span data-ttu-id="18acf-117">Video 4: [předpovědi odpověď s jednoduchého modelu](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 42 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="18acf-117">Video 4: [Predict an answer with a simple model](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*</span></span>
* <span data-ttu-id="18acf-118">Video 5: [zkopírujte jiní lidé práce uděláte vědecké zpracování dat](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="18acf-118">Video 5: [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*</span></span>

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a><span data-ttu-id="18acf-119">Přepis: Položte dotaz, který vám pomůže odpovědět s daty</span><span class="sxs-lookup"><span data-stu-id="18acf-119">Transcript: Ask a question you can answer with data</span></span>
<span data-ttu-id="18acf-120">Vítá vás třetí video v řadě "Vědecké zpracování dat pro začátečníky."</span><span class="sxs-lookup"><span data-stu-id="18acf-120">Welcome to the third video in the series "Data Science for Beginners."</span></span>  

<span data-ttu-id="18acf-121">V této jeden získáte tipy pro formulování dotaz, který vám pomůže odpovědět s daty.</span><span class="sxs-lookup"><span data-stu-id="18acf-121">In this one, you'll get some tips for formulating a question you can answer with data.</span></span>

<span data-ttu-id="18acf-122">Další využití toto video, může získat, pokud nejprve sledovat dvě starší videa z této série: "vědecké zpracování dat 5 otázky může odpovědět na" a "Je vaše data jsou připravena k vědecké zpracování dat?"</span><span class="sxs-lookup"><span data-stu-id="18acf-122">You might get more out of this video, if you first watch the two earlier videos in this series: "The 5 questions data science can answer" and "Is your data is ready for data science?"</span></span>

## <a name="ask-a-sharp-question"></a><span data-ttu-id="18acf-123">Zeptejte se sharp</span><span class="sxs-lookup"><span data-stu-id="18acf-123">Ask a sharp question</span></span>
<span data-ttu-id="18acf-124">Jste už jsme mluvili o tom, jak vědecké zpracování dat pomocí názvů (také nazývané kategorie nebo popisky) a čísel k předvídání odpověď na otázku.</span><span class="sxs-lookup"><span data-stu-id="18acf-124">We've talked about how data science is the process of using names (also called categories or labels) and numbers to predict an answer to a question.</span></span> <span data-ttu-id="18acf-125">Ale nemůže být jen maskou pro jakýkoli otázku; je třeba *sharp otázku.*</span><span class="sxs-lookup"><span data-stu-id="18acf-125">But it can't be just any question; it has to be a *sharp question.*</span></span>

<span data-ttu-id="18acf-126">Nepřesných dotaz nemusí odpovídat názvu nebo číslem.</span><span class="sxs-lookup"><span data-stu-id="18acf-126">A vague question doesn't have to be answered with a name or a number.</span></span> <span data-ttu-id="18acf-127">Musí být sharp otázku.</span><span class="sxs-lookup"><span data-stu-id="18acf-127">A sharp question must.</span></span>

<span data-ttu-id="18acf-128">Představte si, že jste našli magic svítilny s genie, kdo bude pravdivě zodpovědět všechny otázku.</span><span class="sxs-lookup"><span data-stu-id="18acf-128">Imagine you found a magic lamp with a genie who will truthfully answer any question you ask.</span></span> <span data-ttu-id="18acf-129">Ale je mischievous genie a mohl budete pokusí provést jeho odpovědí jako nepřesných a matoucí, jak si můžete rychle získat s.</span><span class="sxs-lookup"><span data-stu-id="18acf-129">But it's a mischievous genie, and he'll try to make his answer as vague and confusing as he can get away with.</span></span> <span data-ttu-id="18acf-130">Chcete připnout mu s vzduchotěsným proto mu nelze pomoci ale zjistit, co chcete vědět, dotaz.</span><span class="sxs-lookup"><span data-stu-id="18acf-130">You want to pin him down with a question so airtight that he can't help but tell you what you want to know.</span></span>

<span data-ttu-id="18acf-131">Pokud byste chtěli zeptejte nepřesných, jako jsou "Co se má stát s Moje stock?", může odpovědět genie, "změní za cenu".</span><span class="sxs-lookup"><span data-stu-id="18acf-131">If you were to ask a vague question, like "What's going to happen with my stock?", the genie might answer, "The price will change".</span></span> <span data-ttu-id="18acf-132">Který je pravdivou odpovědí, ale je velmi užitečné.</span><span class="sxs-lookup"><span data-stu-id="18acf-132">That's a truthful answer, but it's not very helpful.</span></span>

<span data-ttu-id="18acf-133">Ale pokud byste chtěli sharp zeptejte, jako je "Co Moje stock prodej ceny budou příští týden?", nelze genie pomoci ale získáte konkrétní odpovědět a předpovídat cenu prodej.</span><span class="sxs-lookup"><span data-stu-id="18acf-133">But if you were to ask a sharp question, like "What will my stock's sale price be next week?", the genie can't help but give you a specific answer and predict a sale price.</span></span>

## <a name="examples-of-your-answer-target-data"></a><span data-ttu-id="18acf-134">Příklady odpověď: cílová data</span><span class="sxs-lookup"><span data-stu-id="18acf-134">Examples of your answer: Target data</span></span>
<span data-ttu-id="18acf-135">Jakmile jste formulovali dotaz, zkontrolujte, zda je nutné příklady odpověď ve vašich datech.</span><span class="sxs-lookup"><span data-stu-id="18acf-135">Once you formulate your question, check to see whether you have examples of the answer in your data.</span></span>

<span data-ttu-id="18acf-136">Pokud je naše otázku "Co Moje stock prodej cena bude příští týden?"</span><span class="sxs-lookup"><span data-stu-id="18acf-136">If our question is "What will my stock's sale price be next week?"</span></span> <span data-ttu-id="18acf-137">potom máme Ujistěte se, že naše data zahrnují cenu akcií historie.</span><span class="sxs-lookup"><span data-stu-id="18acf-137">then we have to make sure our data includes the stock price history.</span></span>

<span data-ttu-id="18acf-138">Pokud je naše otázku "které car v mé firemního vozového bude nejprve nezdaří?"</span><span class="sxs-lookup"><span data-stu-id="18acf-138">If our question is "Which car in my fleet is going to fail first?"</span></span> <span data-ttu-id="18acf-139">potom máme Ujistěte se, že naše data zahrnují informace o předchozích chybách.</span><span class="sxs-lookup"><span data-stu-id="18acf-139">then we have to make sure our data includes information about previous failures.</span></span>

![Cílová data - příklady odpověď.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/target-data.png)

<span data-ttu-id="18acf-142">Tyto příklady odpovědi se označují jako cíl.</span><span class="sxs-lookup"><span data-stu-id="18acf-142">These examples of answers are called a target.</span></span> <span data-ttu-id="18acf-143">Cílem je, co se pokoušíme předpovídat budoucí datové body, jestli je kategorii nebo číslo.</span><span class="sxs-lookup"><span data-stu-id="18acf-143">A target is what we are trying to predict about future data points, whether it's a category or a number.</span></span>

<span data-ttu-id="18acf-144">Pokud nemáte k dispozici žádná data target, budete muset získat některé.</span><span class="sxs-lookup"><span data-stu-id="18acf-144">If you don't have any target data, you'll need to get some.</span></span> <span data-ttu-id="18acf-145">Nebudete moci odpovídající vaší otázce bez ní.</span><span class="sxs-lookup"><span data-stu-id="18acf-145">You won't be able to answer your question without it.</span></span>

## <a name="reformulate-your-question"></a><span data-ttu-id="18acf-146">Byla znovu formulována váš dotaz</span><span class="sxs-lookup"><span data-stu-id="18acf-146">Reformulate your question</span></span>
<span data-ttu-id="18acf-147">Někdy může změna znění váš dotaz získat užitečnější odpovědí.</span><span class="sxs-lookup"><span data-stu-id="18acf-147">Sometimes you can reword your question to get a more useful answer.</span></span>

<span data-ttu-id="18acf-148">Na otázku "Je tento datový bod A nebo B?"</span><span class="sxs-lookup"><span data-stu-id="18acf-148">The question "Is this data point A or B?"</span></span> <span data-ttu-id="18acf-149">předpovídá kategorie (nebo název nebo popisek) určitého objektu.</span><span class="sxs-lookup"><span data-stu-id="18acf-149">predicts the category (or name or label) of something.</span></span> <span data-ttu-id="18acf-150">K hovor, používáme *klasifikační algoritmus*.</span><span class="sxs-lookup"><span data-stu-id="18acf-150">To answer it, we use a *classification algorithm*.</span></span>

<span data-ttu-id="18acf-151">Otázka "Kolik?"</span><span class="sxs-lookup"><span data-stu-id="18acf-151">The question "How much?"</span></span> <span data-ttu-id="18acf-152">nebo "Kolik?"</span><span class="sxs-lookup"><span data-stu-id="18acf-152">or "How many?"</span></span> <span data-ttu-id="18acf-153">předpovídá dobu.</span><span class="sxs-lookup"><span data-stu-id="18acf-153">predicts an amount.</span></span> <span data-ttu-id="18acf-154">K hovor používáme *regresní algoritmus*.</span><span class="sxs-lookup"><span data-stu-id="18acf-154">To answer it we use a *regression algorithm*.</span></span>

<span data-ttu-id="18acf-155">Informace o tom, jak tyto můžete převést jsme, podíváme se na otázku, "které zprávy scénáře je nejvíce zajímavé pro tento čtečky?"</span><span class="sxs-lookup"><span data-stu-id="18acf-155">To see how we can transform these, let's look at the question, "Which news story is the most interesting to this reader?"</span></span> <span data-ttu-id="18acf-156">Zjišťuje předpovědi jednu volbu z mnoha možností – jinými slovy "Je tento A nebo B nebo C nebo D?"</span><span class="sxs-lookup"><span data-stu-id="18acf-156">It asks for a prediction of a single choice from many possibilities - in other words "Is this A or B or C or D?"</span></span> <span data-ttu-id="18acf-157">- a využije klasifikační algoritmus.</span><span class="sxs-lookup"><span data-stu-id="18acf-157">- and would use a classification algorithm.</span></span>

<span data-ttu-id="18acf-158">Ale touto otázkou mohou být snazší s dotazem, zda Změna znění jej jako "jak zajímavé je každý článek v tomto seznamu k této čtečky?"</span><span class="sxs-lookup"><span data-stu-id="18acf-158">But, this question may be easier to answer if you reword it as "How interesting is each story on this list to this reader?"</span></span> <span data-ttu-id="18acf-159">Teď můžete udělit jednotlivých článků číselné skóre a pak je snadné identifikovat článku nejvyšší vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="18acf-159">Now you can give each article a numerical score, and then it's easy to identify the highest-scoring article.</span></span> <span data-ttu-id="18acf-160">Toto je změnit formulaci otázky klasifikace do regrese otázku nebo kolik?</span><span class="sxs-lookup"><span data-stu-id="18acf-160">This is a rephrasing of the classification question into a regression question or How much?</span></span>

![Byla znovu formulována svůj dotaz.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/classification-question-vs-regression-question.png)

<span data-ttu-id="18acf-163">Jak požádat, že dotaz je potvrzením, na který algoritmus můžou dát odpověď.</span><span class="sxs-lookup"><span data-stu-id="18acf-163">How you ask a question is a clue to which algorithm can give you an answer.</span></span>

<span data-ttu-id="18acf-164">Zjistíte, že jsou některé rodiny algoritmů – jako jsou v našem příkladu scénáře news - úzce související.</span><span class="sxs-lookup"><span data-stu-id="18acf-164">You'll find that certain families of algorithms - like the ones in our news story example - are closely related.</span></span> <span data-ttu-id="18acf-165">Byla znovu formulována svůj dotaz, použít algoritmus, který vám dává nejužitečnější odpověď.</span><span class="sxs-lookup"><span data-stu-id="18acf-165">You can reformulate your question to use the algorithm that gives you the most useful answer.</span></span>

<span data-ttu-id="18acf-166">Ale, nejdůležitější požádejte tuto sharp otázku - otázku, která vám pomůže odpovědět s daty.</span><span class="sxs-lookup"><span data-stu-id="18acf-166">But, most important, ask that sharp question - the question that you can answer with data.</span></span> <span data-ttu-id="18acf-167">A zkontrolujte, zda že máte správná data na hovor.</span><span class="sxs-lookup"><span data-stu-id="18acf-167">And be sure you have the right data to answer it.</span></span>

<span data-ttu-id="18acf-168">Jste už jsme mluvili o některých základních zásad pro dotaz s dotazem, že vám pomůže odpovědět s daty.</span><span class="sxs-lookup"><span data-stu-id="18acf-168">We've talked about some basic principles for asking a question you can answer with data.</span></span>

<span data-ttu-id="18acf-169">Ujistěte se, podívejte se na ostatní videa v "Datové vědy pro začátečníky" z Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="18acf-169">Be sure to check out the other videos in "Data Science for Beginners" from Microsoft Azure Machine Learning.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18acf-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="18acf-170">Next steps</span></span>
* [<span data-ttu-id="18acf-171">Zkuste prvního experimentu vědecké účely data nástroje Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="18acf-171">Try a first data science experiment with Machine Learning Studio</span></span>](machine-learning-create-experiment.md)
* [<span data-ttu-id="18acf-172">Získejte Úvod do Machine Learning v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="18acf-172">Get an introduction to Machine Learning on Microsoft Azure</span></span>](machine-learning-what-is-machine-learning.md)
