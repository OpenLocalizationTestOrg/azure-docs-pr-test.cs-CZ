---
title: "dokáže odpovědět na otázku data - aaaAsk datové vědy problémy - Azure Machine Learning | Microsoft Docs"
description: "Zjistěte, jak otázka tooformulate vědecké zpracování sharp dat v vědecké zpracování dat pro začátečníky video 3. Obsahuje porovnání klasifikace a regrese otázky."
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
ms.openlocfilehash: 00c328f51e6d9ff6654b5966eb97d6762582f7e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="ask-a-question-you-can-answer-with-data"></a>Položení otázky, na kterou lze odpovědět pomocí dat
## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Vědecké zpracování dat pro začátečníky řady
Zjistěte, jak tooformulate problému vědecké účely dat do svůj dotaz vědecké zpracování dat pro začátečníky video 3. Toto video obsahuje porovnání otázky pro klasifikaci a regrese algoritmy.

hello tooget naplno hello řady, podívejte se na všechny. [Přejděte toohello seznamu videí](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Data-science-for-beginners-Ask-a-question-you-can-answer-with-data/player]
>
>

## <a name="other-videos-in-this-series"></a>Další videa z této série
*Vědecké zpracování dat pro začátečníky* je rychlý úvod vědecký toodata pět krátké videa.

* Video 1: [hello 5 otázky, odpovědi vědecké účely data](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 14 s min.)*
* Video 2: [vašich dat je připravený pro vědecké zpracování dat?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 56 sekundu min.)*
* Video 3: Položte dotaz, který vám pomůže odpovědět s daty
* Video 4: [předpovědi odpověď s jednoduchého modelu](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 42 sekundu min.)*
* Video 5: [zkopírujte vědecké zpracování dat jiní lidé pracovní toodo](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 sekundu min.)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Přepis: Položte dotaz, který vám pomůže odpovědět s daty
Vítejte toohello třetí video v řadě hello "Vědecké zpracování dat pro začátečníky."  

V této jeden získáte tipy pro formulování dotaz, který vám pomůže odpovědět s daty.

Další využití toto video, může získat, pokud nejprve sledovat dvě starší videa z této série hello: "vědecké zpracování dat otázky hello 5 může odpovědět na" a "Je vaše data jsou připravena k vědecké zpracování dat?"

## <a name="ask-a-sharp-question"></a>Zeptejte se sharp
Jste už jsme mluvili o tom, jak vědecké zpracování dat je proces hello pomocí názvů (také nazývané kategorie nebo popisky) a čísla toopredict tooa otázku odpovědi. Ale nemůže být jen maskou pro jakýkoli otázku; má toobe *sharp otázku.*

Nepřesných dotaz nemá toobe odpovědi s názvem nebo číslem. Musí být sharp otázku.

Představte si, že jste našli magic svítilny s genie, kdo bude pravdivě zodpovědět všechny otázku. Ale je mischievous genie a mohl budete zkuste toomake jeho odpovědí jako nepřesných a matoucí jako mu můžete rychle získat s. Chcete, aby toopin mu dolů s dotaz vzduchotěsným proto mu nelze pomoci ale zjistíte co chcete tooknow.

Pokud jste tooask nepřesných otázku, jako jsou "Co se děje toohappen s Moje stock?", může odpovědět hello genie, "změní cena hello". Který je pravdivou odpovědí, ale je velmi užitečné.

Ale pokud byste byli tooask sharp otázku, jako "Co Moje stock prodej ceny budou příští týden?", hello genie nemůže pomoci ale získáte konkrétní odpovědět a předpovídat cenu prodej.

## <a name="examples-of-your-answer-target-data"></a>Příklady odpověď: cílová data
Jakmile jste formulovali dotaz, toosee zkontrolujte, zda máte příklady hello odpovědí ve vašich datech.

Pokud je naše otázku "Co Moje stock prodej cena bude příští týden?" potom máme toomake se, že naše data zahrnují cenu akcií historie hello.

Pokud je naše otázku "které car v mé firemního vozového je toofail probíhající nejprve?" potom máme toomake se, že naše data zahrnují informace o předchozích chybách.

![Cílová data - příklady odpověď. Formulovali dotaz vědecké účely data.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/target-data.png)

Tyto příklady odpovědi se označují jako cíl. Cíl je Snažíme se pokouší toopredict o budoucí datové body, jestli je kategorii nebo číslo.

Pokud nemáte k dispozici žádná data target, budete potřebovat tooget některé. Můžete nebudou moct tooanswer váš dotaz bez.

## <a name="reformulate-your-question"></a>Byla znovu formulována váš dotaz
Někdy může změna znění vaši otázku tooget užitečnější odpovědí.

Otázka Hello "je tento datový bod A nebo B?" předpovídá kategorie hello (nebo název nebo popis) určitého objektu. tooanswer, používáme *klasifikační algoritmus*.

Hello otázka "Kolik?" nebo "Kolik?" předpovídá dobu. tooanswer ho používáme *regresní algoritmus*.

toosee jak jsme můžete převést tyto, podíváme se na hello otázku, "které zprávy scénáře je nejvíce zajímavé čtečky toothis hello?" Zjišťuje předpovědi jednu volbu z mnoha možností – jinými slovy "Je tento A nebo B nebo C nebo D?" - a využije klasifikační algoritmus.

Ale tuto otázku může být snazší tooanswer, pokud je jako Změna znění "jak zajímavé je každý článek na tento seznam toothis čtečky?" Nyní můžete udělit jednotlivých článků číselné skóre a pak je snadno tooidentify hello nejvyšší vyhodnocování článku. Toto je změnit formulaci hello klasifikace otázky do regrese otázku nebo kolik?

![Byla znovu formulována svůj dotaz. Klasifikace otázka oproti regrese otázku.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/classification-question-vs-regression-question.png)

Jak požádat, že dotaz je algoritmus toowhich potvrzením můžou dát odpověď.

Zjistíte, že jsou některé rodiny algoritmů - jako hello ty, které jsou v našem příkladu scénáře news - úzce související. Můžete byla znovu formulována vaši otázku toouse hello algoritmus, který vám dává hello nejužitečnější odpovědí.

Ale, nejdůležitější požádejte tuto sharp otázku - hello otázku, která vám pomůže odpovědět s daty. A zkontrolujte, zda máte správná data tooanswer hello ho.

Jste už jsme mluvili o některých základních zásad pro dotaz s dotazem, že vám pomůže odpovědět s daty.

Být jisti toocheck out hello jiných videa v "Datové vědy pro začátečníky" z Microsoft Azure Machine Learning.

## <a name="next-steps"></a>Další kroky
* [Zkuste prvního experimentu vědecké účely data nástroje Machine Learning Studio](machine-learning-create-experiment.md)
* [Získat tooMachine Úvod učení v Microsoft Azure](machine-learning-what-is-machine-learning.md)
