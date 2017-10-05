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
# <a name="is-your-data-ready-for-data-science"></a>Jsou data připravená pro vědecké zkoumání?
## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Vědecké zpracování dat pro začátečníky řady
Zjistěte, jak vyhodnotit vaše data a ujistěte se, že splňuje základní kritéria bude připravená pro vědecké zpracování dat.

Získejte maximum z řady, můžete sledujte všechny. [Přejděte do seznamu videí](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a>Další videa z této série
*Vědecké zpracování dat pro začátečníky* je rychlý úvod do vědecké zpracování dat v pěti krátké videa.

* Video 1: [5 otázky, odpovědi vědecké účely data](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 14 s min.)*
* Video 2: Je vaše data připravená pro vědecké zpracování dat?
* Video 3: [zeptejte se vám pomůže odpovědět s daty](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 17 sekundu min.)*
* Video 4: [předpovědi odpověď s jednoduchého modelu](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 42 sekundu min.)*
* Video 5: [zkopírujte jiní lidé práce uděláte vědecké zpracování dat](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 sekundu min.)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Přepis: Je vaše data připravená pro vědecké zpracování dat?
Vítá vás "Je dat připravené pro vědecké zpracování dat?" druhý video v řadě *vědecké zpracování dat pro začátečníky*.  

Než odpovědi, které chcete, můžete udělit vědecké zpracování dat, musíte jí přidělit suroviny některé vysoce kvalitní pro práci s. Stejně jako provedení pizza, tím lépe složek, které se začíná s, tím lepší konečné produktu. 

## <a name="criteria-for-data"></a>Kritéria pro data
Ano v případě vědecké zpracování dat, nejsou některé faktory, které je potřeba pro vyžádání obsahu společně.

Potřebujeme data, která jsou:

* Relevantní
* připojení
* Přesná
* Dost pro práci s

## <a name="is-your-data-relevant"></a>Je relevantní data?
Proto je první složka - potřebujeme data, která jsou relevantní.

![Relevantní data oproti nahromadění irelevantních dat – vyhodnocení dat.](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

Podívejte se na tabulky na levé straně. Jsme splněny sedm lidé mimo řádky Boston, měří jejich krve alkohol úroveň, Red Sox průměr batting v jejich poslední hře a cena mléka v úložišti nejbližší pohodlí.

Toto jsou všechny perfektně legitimní data. Pouze chyby je, že není relevantní. Není žádný zřejmé vztah mezi tato čísla. Pokud vám I Dal aktuální cena mléka a průměr batting Red Sox, neexistuje způsob může uhodnout Moje krve obsahu.

Nyní podívejte se na tabulky na pravé straně. Tato doba jsme měří každá osoba body velkokapacitních a počítá počet nápojů jste měly.  Čísla v jednotlivých řádcích jsou nyní relevantní pro sebe navzájem. Pokud vám I Dal Moje textu velkokapacitních a počet Margaritas I jste měli, můžete dokonce vytvářet odhad v mé krve alkohol obsahu.

## <a name="do-you-have-connected-data"></a>Připojení dat?
Další složky jsou připojené data.

![Připojení dat oproti odpojené data – data kritéria data připravena](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

Tady jsou některé relevantní data na kvalitu hamburgers: gril teploty, patty váhy a hodnocení v místní jídlo katalogu. Ale Všimněte si, mezer v tabulce na levé straně.

Většina datové sady chybí některé hodnoty. Se běžně používají díry takto a existují způsoby, jak je obejít. Ale pokud je příliš mnoho chybí, začne vypadat mezi sýr vaše data.

Pokud se podíváte na tabulky na levé straně, je mnoho chybějící data, je pevný spolu s jakýmkoli vztah mezi mřížkou teploty a patty váhy. Toto je příklad odpojené data.

V tabulce na pravé straně, ale je plný a dokončete – příklad připojené data.

## <a name="is-your-data-accurate"></a>Je přesná data?
Další složky, které potřebujeme je přesnost. Tady jsou čtyři cíle, které jsme chtěli dosáhl se šipkami.

![Přesná data oproti nepřesných dat. - kritéria dat](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

Podívejte se na cíl v pravém horním rohu. My úzkou seskupení právo kolem terč. Který je samozřejmě přesná. Oddly v jazyce vědecké zpracování dat, naše výkonu na pravé straně cíl pod ním také považuje za přesná.

Pokud byste chtěli zmapování center tyto šipek, zobrazí se, se velmi nachází blízko terč. Šipky jsou všechny kolem cíl, šíření, se považují za nepřesný, ale budou se soustředí na terč, aby se považovat za přesná.

Nyní se podívejte na levém cíl. Zde naše šipky dosáhl velmi blízko sebe úzkou seskupení. Jsou to přesné, ale jsou nesprávné, protože je mimo terč centru. A samozřejmě šipky v okraje v levém dolním cíl jsou nesprávné a nepřesný. Tato archer potřebuje další postup.

## <a name="do-you-have-enough-data-to-work-with"></a>Máte dostatek dat pro práci s?
Nakonec složka #4 -, je potřeba mít dostatek data.

![Máte dostatek dat pro analýzu? Vyhodnocování dat](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

Představte si každý datový bod v tabulce, že je tahu štětce v malování. Pokud máte pouze několik z nich, může být poměrně přibližné Malování – je obtížné zjistit, co je.

Pokud přidáte některé další tahy štětce, vaše Malování spustí získat trochu ostřejší.

Až budete mít sotva dostatek tahy, uvidíte jenom dost pro širokou rozhodnutí. Je někde, který může chcete navštívit? Vypadá to, jasně, která vypadá jako čisté horních – Ano, který je, kde bude na dovolenou.

Při přidávání více dat, obrázek bude jasnější a je možné provádět podrobnější rozhodnutí. Teď můžou se podívat na tři hotels na levém bank. Víte, opravdu líbí architektury funkce v popředí. I budete zůstat, na třetí podlaží.

Data, která jsou relevantní, připojené, přesný a dostatečně jsme mít všechny složky, je potřeba provést některé vědecké zpracování dat vysoké kvality.

Nezapomeňte si projděte si další čtyři videa v *vědecké zpracování dat pro začátečníky* z Microsoft Azure Machine Learning.

## <a name="next-steps"></a>Další kroky
* [Zkuste prvního experimentu vědecké účely data nástroje Machine Learning Studio](machine-learning-create-experiment.md)
* [Získejte Úvod do Machine Learning v Microsoft Azure](machine-learning-what-is-machine-learning.md)
