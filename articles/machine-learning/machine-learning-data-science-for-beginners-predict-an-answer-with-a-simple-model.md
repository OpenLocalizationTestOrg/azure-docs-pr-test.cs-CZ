---
title: "aaaPredict odpověď s jednoduché regresní model - Azure Machine Learning | Microsoft Docs"
description: "Jak toocreate jednoduché regresní model toopredict cenu v vědecké zpracování dat pro začátečníky video 4. Zahrnuje lineární regrese s cílová data."
keywords: "Vytvoření modelu, jednoduchého modelu, prediction ceníku, jednoduché regresní model"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: a28f1fab-e2d8-4663-aa7d-ca3530c8b525
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: d4270c2237c33b7e898b78a08b292bc9d62e49ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="predict-an-answer-with-a-simple-model"></a><span data-ttu-id="a863c-105">Předpovídání odpovědi pomocí jednoduchého modelu</span><span class="sxs-lookup"><span data-stu-id="a863c-105">Predict an answer with a simple model</span></span>
## <a name="video-4-data-science-for-beginners-series"></a><span data-ttu-id="a863c-106">Video 4: Vědecké zpracování dat pro začátečníky řady</span><span class="sxs-lookup"><span data-stu-id="a863c-106">Video 4: Data Science for Beginners series</span></span>
<span data-ttu-id="a863c-107">Zjistěte, jak toocreate jednoduchý regresní model toopredict hello cena kosočtverec v vědecké zpracování dat pro začátečníky video 4.</span><span class="sxs-lookup"><span data-stu-id="a863c-107">Learn how toocreate a simple regression model toopredict hello price of a diamond in Data Science for Beginners video 4.</span></span> <span data-ttu-id="a863c-108">Jsme budete zakreslit regresní model se cílová data.</span><span class="sxs-lookup"><span data-stu-id="a863c-108">We'll draw a regression model with target data.</span></span>

<span data-ttu-id="a863c-109">hello tooget naplno hello řady, podívejte se na všechny.</span><span class="sxs-lookup"><span data-stu-id="a863c-109">tooget hello most out of hello series, watch them all.</span></span> <span data-ttu-id="a863c-110">[Přejděte toohello seznamu videí](#other-videos-in-this-series)
</span><span class="sxs-lookup"><span data-stu-id="a863c-110">[Go toohello list of videos](#other-videos-in-this-series)
</span></span><br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a><span data-ttu-id="a863c-111">Další videa z této série</span><span class="sxs-lookup"><span data-stu-id="a863c-111">Other videos in this series</span></span>
<span data-ttu-id="a863c-112">*Vědecké zpracování dat pro začátečníky* je rychlý úvod vědecký toodata pět krátké videa.</span><span class="sxs-lookup"><span data-stu-id="a863c-112">*Data Science for Beginners* is a quick introduction toodata science in five short videos.</span></span>

* <span data-ttu-id="a863c-113">Video 1: [hello 5 otázky, odpovědi vědecké účely data](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 14 s min.)*</span><span class="sxs-lookup"><span data-stu-id="a863c-113">Video 1: [hello 5 questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*</span></span>
* <span data-ttu-id="a863c-114">Video 2: [vašich dat je připravený pro vědecké zpracování dat?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md)</span><span class="sxs-lookup"><span data-stu-id="a863c-114">Video 2: [Is your data ready for data science?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md)</span></span> <span data-ttu-id="a863c-115">*(4 56 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="a863c-115">*(4 min 56 sec)*</span></span>
* <span data-ttu-id="a863c-116">Video 3: [zeptejte se vám pomůže odpovědět s daty](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 17 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="a863c-116">Video 3: [Ask a question you can answer with data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*</span></span>
* <span data-ttu-id="a863c-117">Video 4: Odpověď s jednoduchého modelu předpovědi</span><span class="sxs-lookup"><span data-stu-id="a863c-117">Video 4: Predict an answer with a simple model</span></span>
* <span data-ttu-id="a863c-118">Video 5: [zkopírujte vědecké zpracování dat jiní lidé pracovní toodo](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 sekundu min.)*</span><span class="sxs-lookup"><span data-stu-id="a863c-118">Video 5: [Copy other people's work toodo data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*</span></span>

## <a name="transcript-predict-an-answer-with-a-simple-model"></a><span data-ttu-id="a863c-119">Přepis: Předpovědi odpověď s jednoduchého modelu</span><span class="sxs-lookup"><span data-stu-id="a863c-119">Transcript: Predict an answer with a simple model</span></span>
<span data-ttu-id="a863c-120">Vítejte toohello čtvrtý video v hello "Datové vědy pro začátečníky" řady.</span><span class="sxs-lookup"><span data-stu-id="a863c-120">Welcome toohello fourth video in hello "Data Science for Beginners" series.</span></span> <span data-ttu-id="a863c-121">V této jeden jsme sestavíte jednoduchého modelu a předpovědím.</span><span class="sxs-lookup"><span data-stu-id="a863c-121">In this one, we'll build a simple model and make a prediction.</span></span>

<span data-ttu-id="a863c-122">A *modelu* je zjednodušený scénář o našich datech.</span><span class="sxs-lookup"><span data-stu-id="a863c-122">A *model* is a simplified story about our data.</span></span> <span data-ttu-id="a863c-123">I budete vám ukážou, I znamenat.</span><span class="sxs-lookup"><span data-stu-id="a863c-123">I'll show you what I mean.</span></span>

## <a name="collect-relevant-accurate-connected-enough-data"></a><span data-ttu-id="a863c-124">Shromažďovat relevantní, přesný, připojené, dostatek dat.</span><span class="sxs-lookup"><span data-stu-id="a863c-124">Collect relevant, accurate, connected, enough data</span></span>
<span data-ttu-id="a863c-125">Řekněme, že chcete tooshop pro kosočtverec.</span><span class="sxs-lookup"><span data-stu-id="a863c-125">Say I want tooshop for a diamond.</span></span> <span data-ttu-id="a863c-126">Je nutné prstenec, který patřil toomy babičky s nastavením pro 1.35 karátová kosočtverec a chcete tooget představu o tom, jak bude cena.</span><span class="sxs-lookup"><span data-stu-id="a863c-126">I have a ring that belonged toomy grandmother with a setting for a 1.35 carat diamond, and I want tooget an idea of how much it will cost.</span></span> <span data-ttu-id="a863c-127">Trvat Poznámkový blok a pera do úložiště špercích hello a I zapište hello cena všech Kárová hello v případě hello a kolik naváží v carats.</span><span class="sxs-lookup"><span data-stu-id="a863c-127">I take a notepad and pen into hello jewelry store, and I write down hello price of all of hello diamonds in hello case and how much they weigh in carats.</span></span> <span data-ttu-id="a863c-128">Počínaje první kosočtverec hello - 1.01 carats a 7,366 $.</span><span class="sxs-lookup"><span data-stu-id="a863c-128">Starting with hello first diamond - it's 1.01 carats and $7,366.</span></span>

<span data-ttu-id="a863c-129">Teď I projít a to udělat pro všechny hello jiných diamanty v úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="a863c-129">Now I go through and do this for all hello other diamonds in hello store.</span></span>

![Sloupce kosočtverec dat](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

<span data-ttu-id="a863c-131">Všimněte si, že naše seznam obsahuje dva sloupce.</span><span class="sxs-lookup"><span data-stu-id="a863c-131">Notice that our list has two columns.</span></span> <span data-ttu-id="a863c-132">Každý sloupec má jiný atribut – váhy v carats a cena - a každý řádek je jeden datový bod, který představuje jednu kosočtverec.</span><span class="sxs-lookup"><span data-stu-id="a863c-132">Each column has a different attribute - weight in carats and price - and each row is a single data point that represents a single diamond.</span></span>

<span data-ttu-id="a863c-133">Ve skutečnosti vytvořili jsme malá sada dat zde - tabulku.</span><span class="sxs-lookup"><span data-stu-id="a863c-133">We've actually created a small data set here - a table.</span></span> <span data-ttu-id="a863c-134">Všimněte si, že splňuje naše kritéria pro kvalitu:</span><span class="sxs-lookup"><span data-stu-id="a863c-134">Notice that it meets our criteria for quality:</span></span>

* <span data-ttu-id="a863c-135">data Hello **relevantní** -váhy je výborný související tooprice</span><span class="sxs-lookup"><span data-stu-id="a863c-135">hello data is **relevant** - weight is definitely related tooprice</span></span>
* <span data-ttu-id="a863c-136">Má **přesné** -jsme překontrolovat hello ceny, které jsme zapište</span><span class="sxs-lookup"><span data-stu-id="a863c-136">It's **accurate** - we double-checked hello prices that we write down</span></span>
* <span data-ttu-id="a863c-137">Má **připojené** -nejsou žádné mezery v některém z těchto sloupců</span><span class="sxs-lookup"><span data-stu-id="a863c-137">It's **connected** - there are no blank spaces in either of these columns</span></span>
* <span data-ttu-id="a863c-138">A protože uvidíme, má **dostatek** data tooanswer naše otázku</span><span class="sxs-lookup"><span data-stu-id="a863c-138">And, as we'll see, it's **enough** data tooanswer our question</span></span>

## <a name="ask-a-sharp-question"></a><span data-ttu-id="a863c-139">Zeptejte se sharp</span><span class="sxs-lookup"><span data-stu-id="a863c-139">Ask a sharp question</span></span>
<span data-ttu-id="a863c-140">Nyní jsme budete sharp způsobem představovat naše otázku: "kolik bude stát toobuy 1.35 karátová kosočtverec?"</span><span class="sxs-lookup"><span data-stu-id="a863c-140">Now we'll pose our question in a sharp way: "How much will it cost toobuy a 1.35 carat diamond?"</span></span>

<span data-ttu-id="a863c-141">Naše seznamu nemá 1.35 karátová kosočtverec v něm, budeme mít toouse hello zbytek naše data tooget toohello otázku odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a863c-141">Our list doesn't have a 1.35 carat diamond in it, so we'll have toouse hello rest of our data tooget an answer toohello question.</span></span>

## <a name="plot-hello-existing-data"></a><span data-ttu-id="a863c-142">Vykreslení stávajících dat hello</span><span class="sxs-lookup"><span data-stu-id="a863c-142">Plot hello existing data</span></span>
<span data-ttu-id="a863c-143">Hello první, provedeme je kreslení na vodorovném řádku číslo názvem osu toochart hello váhu.</span><span class="sxs-lookup"><span data-stu-id="a863c-143">hello first thing we'll do is draw a horizontal number line, called an axis, toochart hello weights.</span></span> <span data-ttu-id="a863c-144">rozsah Hello váhu hello je 0 too2, takže jsme budete kreslení čáry, které pokrývá, rozsah a umístí rysky pro každou půlhodinu karátová.</span><span class="sxs-lookup"><span data-stu-id="a863c-144">hello range of hello weights is 0 too2, so we'll draw a line that covers that range and put ticks for each half carat.</span></span>

<span data-ttu-id="a863c-145">Potom jsme budete kreslení cenu hello toorecord svislé osy a připojte ho toohello váhy vodorovné osy.</span><span class="sxs-lookup"><span data-stu-id="a863c-145">Next we'll draw a vertical axis toorecord hello price and connect it toohello horizontal weight axis.</span></span> <span data-ttu-id="a863c-146">To bude v jednotkách dolarů.</span><span class="sxs-lookup"><span data-stu-id="a863c-146">This will be in units of dollars.</span></span> <span data-ttu-id="a863c-147">Máme teď sadu souřadnic osy.</span><span class="sxs-lookup"><span data-stu-id="a863c-147">Now we have a set of coordinate axes.</span></span>

![Váha a cena osy](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

<span data-ttu-id="a863c-149">Jsme tato data teď už má tootake a aktivujte ji do *bodové vykreslení*.</span><span class="sxs-lookup"><span data-stu-id="a863c-149">We're going tootake this data now and turn it into a *scatter plot*.</span></span> <span data-ttu-id="a863c-150">Toto je číselné datové sady toovisualize skvělý způsob.</span><span class="sxs-lookup"><span data-stu-id="a863c-150">This is a great way toovisualize numerical data sets.</span></span>

<span data-ttu-id="a863c-151">Pro datový bod první hello jsme eyeball svislice 1.01 carats.</span><span class="sxs-lookup"><span data-stu-id="a863c-151">For hello first data point, we eyeball a vertical line at 1.01 carats.</span></span> <span data-ttu-id="a863c-152">Potom jsme eyeball na vodorovném řádku v 7,366 $.</span><span class="sxs-lookup"><span data-stu-id="a863c-152">Then, we eyeball a horizontal line at $7,366.</span></span> <span data-ttu-id="a863c-153">Kde splňují, jsme zakreslit tečku.</span><span class="sxs-lookup"><span data-stu-id="a863c-153">Where they meet, we draw a dot.</span></span> <span data-ttu-id="a863c-154">To představuje naše první kosočtverec.</span><span class="sxs-lookup"><span data-stu-id="a863c-154">This represents our first diamond.</span></span>

<span data-ttu-id="a863c-155">Nyní přejděte prostřednictvím každý kosočtverec v tomto seznamu a hello samé.</span><span class="sxs-lookup"><span data-stu-id="a863c-155">Now we go through each diamond on this list and do hello same thing.</span></span> <span data-ttu-id="a863c-156">Když jsme prostřednictvím, jedná se nám získat: bunch teček, jednu pro každý kosočtverec.</span><span class="sxs-lookup"><span data-stu-id="a863c-156">When we're through, this is what we get: a bunch of dots, one for each diamond.</span></span>

![Bodový graf](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-hello-model-through-hello-data-points"></a><span data-ttu-id="a863c-158">Kreslení modelu hello prostřednictvím hello datových bodů</span><span class="sxs-lookup"><span data-stu-id="a863c-158">Draw hello model through hello data points</span></span>
<span data-ttu-id="a863c-159">Nyní když se podíváte na hello tečky a squint, kolekce hello vypadá jako fat, přibližné řádku.</span><span class="sxs-lookup"><span data-stu-id="a863c-159">Now if you look at hello dots and squint, hello collection looks like a fat, fuzzy line.</span></span> <span data-ttu-id="a863c-160">Můžeme provést naše značky a kreslení úsečky jejím prostřednictvím.</span><span class="sxs-lookup"><span data-stu-id="a863c-160">We can take our marker and draw a straight line through it.</span></span>

<span data-ttu-id="a863c-161">Pomocí kreslení čáry, jsme vytvořili *modelu*.</span><span class="sxs-lookup"><span data-stu-id="a863c-161">By drawing a line, we created a *model*.</span></span> <span data-ttu-id="a863c-162">Představit jako trvá hello skutečných a provádění zneužívající vlastností prohlížeče Kreslený vtip verzi ho.</span><span class="sxs-lookup"><span data-stu-id="a863c-162">Think of this as taking hello real world and making a simplistic cartoon version of it.</span></span> <span data-ttu-id="a863c-163">Nyní Kreslený vtip hello je špatně – hello řádku není spojen všechny hello datových bodů.</span><span class="sxs-lookup"><span data-stu-id="a863c-163">Now hello cartoon is wrong - hello line doesn't go through all hello data points.</span></span> <span data-ttu-id="a863c-164">Ale je užitečné zjednodušení.</span><span class="sxs-lookup"><span data-stu-id="a863c-164">But, it's a useful simplification.</span></span>

![Lineární regrese řádku](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

<span data-ttu-id="a863c-166">Hello skutečnost, že všechny tečky hello neprocházejí přesně hello řádku je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="a863c-166">hello fact that all hello dots don't go exactly through hello line is OK.</span></span> <span data-ttu-id="a863c-167">Datových vědců vysvětlují to tím, že nejsou hello model - hello řádku - a potom jednotlivých bodů má některé *šumu* nebo *odchylky* s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="a863c-167">Data scientists explain this by saying that there's hello model - that's hello line - and then each dot has some *noise* or *variance* associated with it.</span></span> <span data-ttu-id="a863c-168">Je hello základní ideální relace a pak je dobrý krupičnaté, skutečné den, který přidá šumu a nejistoty.</span><span class="sxs-lookup"><span data-stu-id="a863c-168">There's hello underlying perfect relationship, and then there's hello gritty, real world that adds noise and uncertainty.</span></span>

<span data-ttu-id="a863c-169">Protože jsme se snažíte tooanswer hello otázku *kolik?* tomu se říká *regrese*.</span><span class="sxs-lookup"><span data-stu-id="a863c-169">Because we're trying tooanswer hello question *How much?* this is called a *regression*.</span></span> <span data-ttu-id="a863c-170">A protože používáme přímku, je *lineární regrese*.</span><span class="sxs-lookup"><span data-stu-id="a863c-170">And because we're using a straight line, it's a *linear regression*.</span></span>

## <a name="use-hello-model-toofind-hello-answer"></a><span data-ttu-id="a863c-171">Použít hello modelu toofind hello odpovědí</span><span class="sxs-lookup"><span data-stu-id="a863c-171">Use hello model toofind hello answer</span></span>
<span data-ttu-id="a863c-172">Máme modelu a jeho požádáme naše otázku teď: jaké budou 1.35 karátová kosočtverec náklady?</span><span class="sxs-lookup"><span data-stu-id="a863c-172">Now we have a model and we ask it our question: How much will a 1.35 carat diamond cost?</span></span>

<span data-ttu-id="a863c-173">tooanswer naše otázku jsme oka 1.35 carats a kreslení svislé čáry.</span><span class="sxs-lookup"><span data-stu-id="a863c-173">tooanswer our question, we eyeball 1.35 carats and draw a vertical line.</span></span> <span data-ttu-id="a863c-174">Tam, kde ji protne hello modelu řádku, jsme eyeball osou dolaru toohello vodorovném řádku.</span><span class="sxs-lookup"><span data-stu-id="a863c-174">Where it crosses hello model line, we eyeball a horizontal line toohello dollar axis.</span></span> <span data-ttu-id="a863c-175">Volání vpravo na 10 000.</span><span class="sxs-lookup"><span data-stu-id="a863c-175">It hits right at 10,000.</span></span> <span data-ttu-id="a863c-176">Výložníku!</span><span class="sxs-lookup"><span data-stu-id="a863c-176">Boom!</span></span> <span data-ttu-id="a863c-177">Který je odpovědí hello: 1.35 karátová kosočtverec stojí přibližně 10 000.</span><span class="sxs-lookup"><span data-stu-id="a863c-177">That's hello answer: A 1.35 carat diamond costs about $10,000.</span></span>

![Najít hello odpověď na modelu hello](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a><span data-ttu-id="a863c-179">Vytvořit interval spolehlivosti</span><span class="sxs-lookup"><span data-stu-id="a863c-179">Create a confidence interval</span></span>
<span data-ttu-id="a863c-180">Je přirozené toowonder jak přesné, je tato předpovědi.</span><span class="sxs-lookup"><span data-stu-id="a863c-180">It's natural toowonder how precise this prediction is.</span></span> <span data-ttu-id="a863c-181">Je užitečné tooknow zda hello 1.35 karátová kosočtverec bude velmi zavřít příliš 10 000 nebo mnoho vyšší nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="a863c-181">It's useful tooknow whether hello 1.35 carat diamond will be very close too$10,000, or a lot higher or lower.</span></span> <span data-ttu-id="a863c-182">toofigure tohoto limitu, můžeme kreslení obálku kolem hello regresní přímky, který obsahuje většinu hello tečky.</span><span class="sxs-lookup"><span data-stu-id="a863c-182">toofigure this out, let's draw an envelope around hello regression line that includes most of hello dots.</span></span> <span data-ttu-id="a863c-183">Je volána tuto obálku naše *interval spolehlivosti*: nám poměrně jisti, že ceny spadal do této obálky, protože v posledních hello většina z nich má.</span><span class="sxs-lookup"><span data-stu-id="a863c-183">This envelope is called our *confidence interval*: We're pretty confident that prices fall within this envelope, because in hello past most of them have.</span></span> <span data-ttu-id="a863c-184">Z kde hello 1.35 karátová řádku protne hello horní a dolní hello této obálky jsme můžete zakreslit dvě více vodorovné čáry.</span><span class="sxs-lookup"><span data-stu-id="a863c-184">We can draw two more horizontal lines from where hello 1.35 carat line crosses hello top and hello bottom of that envelope.</span></span>

![Interval spolehlivosti](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

<span data-ttu-id="a863c-186">Nyní budeme moct něco o našem interval spolehlivosti: budeme moct mohli bez obav, hello cena 1.35 karátová kosočtverec je o $ 10 000 – ale může být co nejnižší $ 8 000 a může být až 12 000 $.</span><span class="sxs-lookup"><span data-stu-id="a863c-186">Now we can say something about our confidence interval:  We can say confidently that hello price of a 1.35 carat diamond is about $10,000 - but it might be as low as $8,000 and it might be as high as $12,000.</span></span>

## <a name="were-done-with-no-math-or-computers"></a><span data-ttu-id="a863c-187">Máme Hotovo, bez matematické nebo počítače</span><span class="sxs-lookup"><span data-stu-id="a863c-187">We're done, with no math or computers</span></span>
<span data-ttu-id="a863c-188">Jsme nebyla, jaké datových vědců získat placené toodo a jsme nebyla pouhým kreslení:</span><span class="sxs-lookup"><span data-stu-id="a863c-188">We did what data scientists get paid toodo, and we did it just by drawing:</span></span>

* <span data-ttu-id="a863c-189">Jsme neptal že jsme může odpovědět s daty</span><span class="sxs-lookup"><span data-stu-id="a863c-189">We asked a question that we could answer with data</span></span>
* <span data-ttu-id="a863c-190">Využili jsme *modelu* pomocí *lineární regrese*</span><span class="sxs-lookup"><span data-stu-id="a863c-190">We built a *model* using *linear regression*</span></span>
* <span data-ttu-id="a863c-191">Jsme provedli *předpovědi*, kompletní sadou *interval spolehlivosti*</span><span class="sxs-lookup"><span data-stu-id="a863c-191">We made a *prediction*, complete with a *confidence interval*</span></span>

<span data-ttu-id="a863c-192">A jsme nepoužili matematické nebo počítače toodo ho.</span><span class="sxs-lookup"><span data-stu-id="a863c-192">And we didn't use math or computers toodo it.</span></span>

<span data-ttu-id="a863c-193">Nyní když jsme měl měli další informace, jako je třeba...</span><span class="sxs-lookup"><span data-stu-id="a863c-193">Now if we'd had more information, like...</span></span>

* <span data-ttu-id="a863c-194">Hello vyjmout z kosočtverec hello</span><span class="sxs-lookup"><span data-stu-id="a863c-194">hello cut of hello diamond</span></span>
* <span data-ttu-id="a863c-195">Barva varianty (jak zavřít kosočtverec hello je toobeing bílé)</span><span class="sxs-lookup"><span data-stu-id="a863c-195">color variations (how close hello diamond is toobeing white)</span></span>
* <span data-ttu-id="a863c-196">Hello počet zahrnutí v kosočtverec hello</span><span class="sxs-lookup"><span data-stu-id="a863c-196">hello number of inclusions in hello diamond</span></span>

<span data-ttu-id="a863c-197">.. .potom jsme by měli více sloupců.</span><span class="sxs-lookup"><span data-stu-id="a863c-197">...then we would have had more columns.</span></span> <span data-ttu-id="a863c-198">V takovém případě se změní matematické užitečné.</span><span class="sxs-lookup"><span data-stu-id="a863c-198">In that case, math becomes helpful.</span></span> <span data-ttu-id="a863c-199">Pokud máte více než dva sloupce, je pevný toodraw tečky na papíře.</span><span class="sxs-lookup"><span data-stu-id="a863c-199">If you have more than two columns, it's hard toodraw dots on paper.</span></span> <span data-ttu-id="a863c-200">matematické Hello umožňuje velmi vhodně podle daného řádku nebo že roviny tooyour data.</span><span class="sxs-lookup"><span data-stu-id="a863c-200">hello math lets you fit that line or that plane tooyour data very nicely.</span></span>

<span data-ttu-id="a863c-201">Navíc pokud místo jen někteří z nich diamanty, jsme měli dva tisíce nebo 2 000 a potom můžete provést které pracují mnohem rychleji s počítačem.</span><span class="sxs-lookup"><span data-stu-id="a863c-201">Also, if instead of just a handful of diamonds, we had two thousand or two million, then you can do that work much faster with a computer.</span></span>

<span data-ttu-id="a863c-202">V současné době jsme jste věnovala tomu, jak toodo lineární regrese a My provedené pomocí data předpovědi.</span><span class="sxs-lookup"><span data-stu-id="a863c-202">Today, we've talked about how toodo linear regression, and we made a prediction using data.</span></span>

<span data-ttu-id="a863c-203">Být jisti toocheck out hello jiných videa v "Datové vědy pro začátečníky" z Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="a863c-203">Be sure toocheck out hello other videos in "Data Science for Beginners" from Microsoft Azure Machine Learning.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a863c-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a863c-204">Next steps</span></span>
* [<span data-ttu-id="a863c-205">Zkuste prvního experimentu vědecké účely data nástroje Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="a863c-205">Try a first data science experiment with Machine Learning Studio</span></span>](machine-learning-create-experiment.md)
* [<span data-ttu-id="a863c-206">Získat tooMachine Úvod učení v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="a863c-206">Get an introduction tooMachine Learning on Microsoft Azure</span></span>](machine-learning-what-is-machine-learning.md)
