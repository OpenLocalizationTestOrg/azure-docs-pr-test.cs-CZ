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
# <a name="predict-an-answer-with-a-simple-model"></a>Předpovídání odpovědi pomocí jednoduchého modelu
## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Vědecké zpracování dat pro začátečníky řady
Zjistěte, jak toocreate jednoduchý regresní model toopredict hello cena kosočtverec v vědecké zpracování dat pro začátečníky video 4. Jsme budete zakreslit regresní model se cílová data.

hello tooget naplno hello řady, podívejte se na všechny. [Přejděte toohello seznamu videí](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a>Další videa z této série
*Vědecké zpracování dat pro začátečníky* je rychlý úvod vědecký toodata pět krátké videa.

* Video 1: [hello 5 otázky, odpovědi vědecké účely data](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 14 s min.)*
* Video 2: [vašich dat je připravený pro vědecké zpracování dat?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 56 sekundu min.)*
* Video 3: [zeptejte se vám pomůže odpovědět s daty](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 17 sekundu min.)*
* Video 4: Odpověď s jednoduchého modelu předpovědi
* Video 5: [zkopírujte vědecké zpracování dat jiní lidé pracovní toodo](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 sekundu min.)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Přepis: Předpovědi odpověď s jednoduchého modelu
Vítejte toohello čtvrtý video v hello "Datové vědy pro začátečníky" řady. V této jeden jsme sestavíte jednoduchého modelu a předpovědím.

A *modelu* je zjednodušený scénář o našich datech. I budete vám ukážou, I znamenat.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Shromažďovat relevantní, přesný, připojené, dostatek dat.
Řekněme, že chcete tooshop pro kosočtverec. Je nutné prstenec, který patřil toomy babičky s nastavením pro 1.35 karátová kosočtverec a chcete tooget představu o tom, jak bude cena. Trvat Poznámkový blok a pera do úložiště špercích hello a I zapište hello cena všech Kárová hello v případě hello a kolik naváží v carats. Počínaje první kosočtverec hello - 1.01 carats a 7,366 $.

Teď I projít a to udělat pro všechny hello jiných diamanty v úložišti hello.

![Sloupce kosočtverec dat](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Všimněte si, že naše seznam obsahuje dva sloupce. Každý sloupec má jiný atribut – váhy v carats a cena - a každý řádek je jeden datový bod, který představuje jednu kosočtverec.

Ve skutečnosti vytvořili jsme malá sada dat zde - tabulku. Všimněte si, že splňuje naše kritéria pro kvalitu:

* data Hello **relevantní** -váhy je výborný související tooprice
* Má **přesné** -jsme překontrolovat hello ceny, které jsme zapište
* Má **připojené** -nejsou žádné mezery v některém z těchto sloupců
* A protože uvidíme, má **dostatek** data tooanswer naše otázku

## <a name="ask-a-sharp-question"></a>Zeptejte se sharp
Nyní jsme budete sharp způsobem představovat naše otázku: "kolik bude stát toobuy 1.35 karátová kosočtverec?"

Naše seznamu nemá 1.35 karátová kosočtverec v něm, budeme mít toouse hello zbytek naše data tooget toohello otázku odpovědi.

## <a name="plot-hello-existing-data"></a>Vykreslení stávajících dat hello
Hello první, provedeme je kreslení na vodorovném řádku číslo názvem osu toochart hello váhu. rozsah Hello váhu hello je 0 too2, takže jsme budete kreslení čáry, které pokrývá, rozsah a umístí rysky pro každou půlhodinu karátová.

Potom jsme budete kreslení cenu hello toorecord svislé osy a připojte ho toohello váhy vodorovné osy. To bude v jednotkách dolarů. Máme teď sadu souřadnic osy.

![Váha a cena osy](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Jsme tato data teď už má tootake a aktivujte ji do *bodové vykreslení*. Toto je číselné datové sady toovisualize skvělý způsob.

Pro datový bod první hello jsme eyeball svislice 1.01 carats. Potom jsme eyeball na vodorovném řádku v 7,366 $. Kde splňují, jsme zakreslit tečku. To představuje naše první kosočtverec.

Nyní přejděte prostřednictvím každý kosočtverec v tomto seznamu a hello samé. Když jsme prostřednictvím, jedná se nám získat: bunch teček, jednu pro každý kosočtverec.

![Bodový graf](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-hello-model-through-hello-data-points"></a>Kreslení modelu hello prostřednictvím hello datových bodů
Nyní když se podíváte na hello tečky a squint, kolekce hello vypadá jako fat, přibližné řádku. Můžeme provést naše značky a kreslení úsečky jejím prostřednictvím.

Pomocí kreslení čáry, jsme vytvořili *modelu*. Představit jako trvá hello skutečných a provádění zneužívající vlastností prohlížeče Kreslený vtip verzi ho. Nyní Kreslený vtip hello je špatně – hello řádku není spojen všechny hello datových bodů. Ale je užitečné zjednodušení.

![Lineární regrese řádku](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Hello skutečnost, že všechny tečky hello neprocházejí přesně hello řádku je v pořádku. Datových vědců vysvětlují to tím, že nejsou hello model - hello řádku - a potom jednotlivých bodů má některé *šumu* nebo *odchylky* s ním spojená. Je hello základní ideální relace a pak je dobrý krupičnaté, skutečné den, který přidá šumu a nejistoty.

Protože jsme se snažíte tooanswer hello otázku *kolik?* tomu se říká *regrese*. A protože používáme přímku, je *lineární regrese*.

## <a name="use-hello-model-toofind-hello-answer"></a>Použít hello modelu toofind hello odpovědí
Máme modelu a jeho požádáme naše otázku teď: jaké budou 1.35 karátová kosočtverec náklady?

tooanswer naše otázku jsme oka 1.35 carats a kreslení svislé čáry. Tam, kde ji protne hello modelu řádku, jsme eyeball osou dolaru toohello vodorovném řádku. Volání vpravo na 10 000. Výložníku! Který je odpovědí hello: 1.35 karátová kosočtverec stojí přibližně 10 000.

![Najít hello odpověď na modelu hello](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Vytvořit interval spolehlivosti
Je přirozené toowonder jak přesné, je tato předpovědi. Je užitečné tooknow zda hello 1.35 karátová kosočtverec bude velmi zavřít příliš 10 000 nebo mnoho vyšší nebo nižší. toofigure tohoto limitu, můžeme kreslení obálku kolem hello regresní přímky, který obsahuje většinu hello tečky. Je volána tuto obálku naše *interval spolehlivosti*: nám poměrně jisti, že ceny spadal do této obálky, protože v posledních hello většina z nich má. Z kde hello 1.35 karátová řádku protne hello horní a dolní hello této obálky jsme můžete zakreslit dvě více vodorovné čáry.

![Interval spolehlivosti](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Nyní budeme moct něco o našem interval spolehlivosti: budeme moct mohli bez obav, hello cena 1.35 karátová kosočtverec je o $ 10 000 – ale může být co nejnižší $ 8 000 a může být až 12 000 $.

## <a name="were-done-with-no-math-or-computers"></a>Máme Hotovo, bez matematické nebo počítače
Jsme nebyla, jaké datových vědců získat placené toodo a jsme nebyla pouhým kreslení:

* Jsme neptal že jsme může odpovědět s daty
* Využili jsme *modelu* pomocí *lineární regrese*
* Jsme provedli *předpovědi*, kompletní sadou *interval spolehlivosti*

A jsme nepoužili matematické nebo počítače toodo ho.

Nyní když jsme měl měli další informace, jako je třeba...

* Hello vyjmout z kosočtverec hello
* Barva varianty (jak zavřít kosočtverec hello je toobeing bílé)
* Hello počet zahrnutí v kosočtverec hello

.. .potom jsme by měli více sloupců. V takovém případě se změní matematické užitečné. Pokud máte více než dva sloupce, je pevný toodraw tečky na papíře. matematické Hello umožňuje velmi vhodně podle daného řádku nebo že roviny tooyour data.

Navíc pokud místo jen někteří z nich diamanty, jsme měli dva tisíce nebo 2 000 a potom můžete provést které pracují mnohem rychleji s počítačem.

V současné době jsme jste věnovala tomu, jak toodo lineární regrese a My provedené pomocí data předpovědi.

Být jisti toocheck out hello jiných videa v "Datové vědy pro začátečníky" z Microsoft Azure Machine Learning.

## <a name="next-steps"></a>Další kroky
* [Zkuste prvního experimentu vědecké účely data nástroje Machine Learning Studio](machine-learning-create-experiment.md)
* [Získat tooMachine Úvod učení v Microsoft Azure](machine-learning-what-is-machine-learning.md)
