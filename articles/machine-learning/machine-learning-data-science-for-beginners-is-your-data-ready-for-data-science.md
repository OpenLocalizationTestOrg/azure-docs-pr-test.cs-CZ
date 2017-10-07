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
# <a name="is-your-data-ready-for-data-science"></a>Jsou data připravená pro vědecké zkoumání?
## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Vědecké zpracování dat pro začátečníky řady
Zjistěte, jak tooevaluate toomake vaše data, zda splňuje základní kritéria toobe připravené pro vědecké zpracování dat.

hello tooget naplno hello řady, podívejte se na všechny. [Přejděte toohello seznamu videí](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a>Další videa z této série
*Vědecké zpracování dat pro začátečníky* je rychlý úvod vědecký toodata pět krátké videa.

* Video 1: [hello 5 otázky, odpovědi vědecké účely data](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 14 s min.)*
* Video 2: Je vaše data připravená pro vědecké zpracování dat?
* Video 3: [zeptejte se vám pomůže odpovědět s daty](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 17 sekundu min.)*
* Video 4: [předpovědi odpověď s jednoduchého modelu](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 42 sekundu min.)*
* Video 5: [zkopírujte vědecké zpracování dat jiní lidé pracovní toodo](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 sekundu min.)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Přepis: Je vaše data připravená pro vědecké zpracování dat?
Vítá příliš "je dat připravené pro vědecké zpracování dat?" Hello druhý video v řadě hello *vědecké zpracování dat pro začátečníky*.  

Předtím, než může poskytnout vědecké zpracování dat hello odpovědi mají, je třeba toogive ho některé vysoce kvalitní suroviny toowork s. Stejně jako provádění pizza, hello hello lepší hello přísady začínat, lépe hello konečné produktu. 

## <a name="criteria-for-data"></a>Kritéria pro data
Ano v případě hello vědecké zpracování dat, existují některé přísady, potřebujeme toopull společně.

Potřebujeme data, která jsou:

* Relevantní
* připojení
* Přesná
* Dostatek toowork s

## <a name="is-your-data-relevant"></a>Je relevantní data?
Takže první složka hello - potřebujeme data, která jsou relevantní.

![Relevantní data oproti nahromadění irelevantních dat – vyhodnocení dat.](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

Podívejte se na hello tabulky na levé straně hello. Jsme splněny sedm lidé mimo řádky Boston, měří jejich krve alkohol úroveň, hello Red Sox batting průměr v jejich poslední hře a cena hello mléka v hello nejbližší pohodlí úložiště.

Toto jsou všechny perfektně legitimní data. Pouze chyby je, že není relevantní. Není žádný zřejmé vztah mezi tato čísla. Pokud I hello aktuální cena mléka a hello Red Sox batting průměru, neexistuje žádný způsob, který by mohl snadno uhodnout Moje krve obsahu.

Nyní prohlédněte hello tabulky na pravé hello. Tato doba jsme měří každá osoba textu hello velkokapacitního a počtu výskytů počet nápojů, které jste měly.  Hello čísla v jednotlivých řádcích jsou teď další relevantní tooeach. Pokud vám I Dal Moje textu velkokapacitních a hello počet Margaritas I jste měli, můžete dokonce vytvářet odhad v mé krve alkohol obsahu.

## <a name="do-you-have-connected-data"></a>Připojení dat?
Další složky Hello jsou připojené data.

![Připojení dat oproti odpojené data – data kritéria data připravena](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

Tady jsou některé relevantní data na kvalitu hello hamburgers: gril teploty, patty váhy a hodnocení v hello místní jídlo katalogu. Ale Všimněte si hello mezery v hello tabulky na levé straně hello.

Většina datové sady chybí některé hodnoty. Je běžné toohave díry takto a existují způsoby toowork je obcházet. Ale pokud je příliš mnoho chybí, data se zahájí toolook jako sýr mezi.

Pokud se podíváte na hello tabulky na levé straně hello, je proto mnohem chybějící data, je pevný toocome až s jakýmkoli vztah mezi gril teploty a patty váhy. Toto je příklad odpojené data.

Hello tabulky na pravé straně hello ale je plný a dokončete – příklad připojené data.

## <a name="is-your-data-accurate"></a>Je přesná data?
Hello další složky, které potřebujeme je přesnost. Tady jsou čtyři cíle, rádi bychom znali toohit se šipkami.

![Přesná data oproti nepřesných dat. - kritéria dat](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

Podívejte se na cíl hello v pravé horní části hello. My úzkou seskupení právo kolem terč hello. Který je samozřejmě přesná. Oddly v jazyku hello vědecké zpracování dat, naše výkonu cílové vpravo hello pod ním také považuje za přesná.

Pokud byste byli toomap se na centrum hello tyto šipek, zobrazí se, že se jedná o velmi zavřít toohello terč. Hello dvojice šipek, které jsou všechny kolem hello cíl, šíření, se považují za nepřesný, ale budou se soustředí na hello terč, se považují za přesná.

Nyní se podívejte na cílové levém hello. Zde naše šipky dosáhl velmi blízko sebe úzkou seskupení. Jsou přesné, ale protože je mimo hello terč hello centra jsou nesprávné. A samozřejmě hello šipky v hello okraje v levém dolním cíl jsou nesprávné a nepřesný. Tato archer potřebuje další postup.

## <a name="do-you-have-enough-data-toowork-with"></a>Máte dostatek dat toowork s?
Nakonec složka #4 - potřebujeme toohave dostatek dat.

![Máte dostatek dat pro analýzu? Vyhodnocování dat](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

Představte si každý datový bod v tabulce, že je tahu štětce v malování. Pokud máte pouze několik z nich, může být poměrně přibližné hello Malování – je pevný tootell co je.

Pokud přidáte některé další tahy štětce, vaše Malování spustí tooget málo ostřejší.

Až budete mít sotva dostatek tahy, uvidíte jenom dostatek toomake některé široký rozhodnutí. Je někde že chci může toovisit? Vypadá to, jasně, která vypadá jako čisté horních – Ano, který je, kde bude na dovolenou.

Při přidávání více dat, hello obrázek bude jasnější a je možné provádět podrobnější rozhodnutí. Nyní můžete vypadají hello tři hotels na levém bank hello. Víte, opravdu líbí hello architektury funkce hello, jeden v popředí hello. I budete zůstat, na třetí podlaží hello.

S daty, která je relevantní, připojené, přesné a dostatek, budeme mít všechny složky hello potřebujeme toodo některé vědecké zpracování dat vysoké kvality.

Být jisti toocheck out hello jiné čtyři videa v *vědecké zpracování dat pro začátečníky* z Microsoft Azure Machine Learning.

## <a name="next-steps"></a>Další kroky
* [Zkuste prvního experimentu vědecké účely data nástroje Machine Learning Studio](machine-learning-create-experiment.md)
* [Získat tooMachine Úvod učení v Microsoft Azure](machine-learning-what-is-machine-learning.md)
