---
title: "aaaMachine učení rychlý přehled algoritmů | Microsoft Docs"
description: "Tisknutelná strojového učení rychlý přehled algoritmů umožňuje vybrat hello správné algoritmus pro prediktivní model v Azure Machine Learning Studio."
keywords: "rychlý přehled algoritmů, tahák, algoritmu strojového učení"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e1dc31ec-1acb-463f-ba77-de565d4ddf4d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: garye
ms.openlocfilehash: 2bedd77bfc65128a90c6128743016415dd8609d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-algorithm-cheat-sheet-for-microsoft-azure-machine-learning-studio"></a>Tahák k algoritmům služby Machine Learning pro Microsoft Azure Machine Learning Studio
Hello **aktualizace Microsoft Azure Machine Learning algoritmus cheaty list** umožňuje vybrat hello správné algoritmus pro model prediktivní analýzy.

[Azure Machine Learning Studio](https://studio.azureml.net/) má velké knihovny algoritmů z hello ***regrese***, ***klasifikace***, ***clustering***a ***detekce anomálií*** rodiny. Každá je navrženou tooaddress jiný druh problému machine learning.

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>Stáhnout: Strojového učení algoritmu – tahák
**Stáhnout hello tahák zde: [Machine Learning algoritmus cheaty list (11 × 17 palců.)](http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf)**

![Machine Learning algoritmus tahák: Zjistěte, jak toochoose algoritmus Machine Learning.][cheat-sheet]

[cheat-sheet]: ./media/machine-learning-algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet-small_v_0_6-01.png

Stáhnout a vytisknout hello Machine Learning algoritmus cheaty stylů v tookeep velikosti tabloid měli a get potřebujete pomoc s výběrem algoritmu.

> [!NOTE]
> Najdete v článku hello [jak toochoose algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) pro podrobné příručce toousing tento tahák.
> 
> 

## <a name="more-help-with-algorithms"></a>Další pomoc s algoritmy
* Pomoc při použití této tahák pro výběr správné algoritmus hello plus podrobnější diskuzi o hello různé typy algoritmů machine learningu a způsobu jejich použití najdete v tématu [jak toochoose algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).
* Infografice ke stažení, který popisuje algoritmy a obsahuje příklady, najdete v části [ke stažení Infografice: strojového učení základy s příklady algoritmus](machine-learning-basics-infographic-with-algorithm-examples.md).
* Seznam podle kategorie všechny hello algoritmy strojového učení dostupné v nástroji Machine Learning Studio najdete v tématu [inicializovat Model] [ initialize-model] v nápovědě modulu a hello Machine Learning Studio algoritmus.
* Dokončení abecední seznam algoritmů a modulů v Machine Learning Studio najdete v tématu [seznam A-Z modulů Machine Learning Studio] [ a-z-list] v Machine Learning Studio algoritmus a pomoci modulu.
* toodownload a tisk diagram, který bude stručně charakterizovat hello možností nástroje Machine Learning Studio, najdete v [diagram s přehledem funkcí Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="notes-and-terminology-definitions-for-hello-machine-learning-algorithm-cheat-sheet"></a>Poznámky a definice terminologie pro hello algoritmus strojového učení tahák

* návrhy Hello nenabízí tento rychlý přehled algoritmů jsou přibližné pravidla z jezdce. Některé můžete ohnuty a některé můžou být flagrantly došlo k porušení. To je zamýšlené toosuggest počáteční bod. Nemusíte být obávat toorun head-to-head konfliktům mezi několik algoritmů na vaše data. Neexistuje žádné jednoduše substitute pro pochopení hello Principy jednotlivých algoritmů a pochopení hello systému, která vygenerovala data.

* Každý algoritmu strojového učení má svůj vlastní styl nebo *induktivní odchylka*. Pro určitý problém může být vhodné několik algoritmů a jeden algoritmus může být lépe odpovídaly než jiné. Ale není vždy možné tooknow předem což je nejlepší hello. V takových případech jsou několik algoritmů uvedené v hello tahák společně. Vhodná strategie by být tootry jeden algoritmus a pokud výsledky hello ještě nejsou uspokojivé, zkuste jiné hello. Tady je příklad z hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) experimentu několik algoritmů proti hello by se stejným datům a porovnává hello výsledky: [porovnat více třída třídění: písmeno rozpoznávání ](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

* Existují tři hlavní kategorie machine learning: **učení se supervizí**, **supervize**, a **posílení learning**.

  * V **učení se supervizí**, každý datový bod je označené nebo ve spojení s kategorie nebo hodnoty, které vás zajímají.  Příklad kategorií popisku je přiřazování bitovou kopii jako 'cat' nebo 'PSA.  Příklad popisku hodnota je hello prodej ceny automobilu použitých přidružené. Hello cílem pod dohledem learning je toostudy mnoho označené příklady takovéto a potom toobe možné toomake předpovědi o budoucích datových bodů. Například identifikace nové fotografie s hello správné zvíře nebo přiřadit přesné prodej ceny tooother používat aut. Toto je Oblíbené a užitečné typ strojové učení. Všechny moduly hello v Azure Machine Learning jsou pod dohledem učení algoritmy s výjimkou [K-Means Clustering][k-means-clustering].

  * V **supervize**, datové body mají žádné popisky s nimi spojených. Místo toho hello cílem bez dohledu učení algoritmu je tooorganize hello data v některé stejným způsobem jako nebo toodescribe jeho struktura. To může znamenat seskupení do clusterů, protože K-means, nebo při hledání různé způsoby prohlížení komplexní data tak, aby se jednodušší.

  * V **posílení learning**, algoritmus hello získá toochoose akce v odpovědi tooeach datového bodu. Je běžný postup v robotics, kde hello sadu odečty snímačů v jednom bodě v čase je hodnota datového bodu, a algoritmus hello musíte zvolit hello robot další akce. Je také přírodní pro Internet věcí aplikace. algoritmus učení Hello dále přijímá signálu účet, který byl po krátkou dobu později, která určuje, jak dobrý hello rozhodnutí. Na základě hello algoritmus upravuje jeho strategií pořadí tooachieve hello nejvyšší potřebu. Aktuálně neexistují žádné posílení učení algoritmu modulů v Azure ML.

* **Metody Bayesova** hello předpokládá statisticky nezávislé datových bodů. To znamená, že hello unmodeled variabilita jeden datový bod je bez korelace nejsou s ostatními, který je nelze předpovědět. Například pokud jsou data hello dále zaznamenávána hello počet minut, dokud nebude hello další podzemní dráha train dorazí, dvě měření pořízených za den od sebe jsou statisticky nezávislé. Ale dvě měření pořízených minutu od sebe nejsou statisticky nezávislé – hello jednu hodnotu vysoce prediktivní hello hodnoty hello jiné.

* **Rozhodovací strom regrese boosted** využívá funkce překrývají nebo interakce mezi funkcí. Aby prostředky, které v jakékoli dané data bodu, hello hodnotu jedna z funkcí je poněkud prediktivní hello hodnoty jiného. Například v denních dat vysokou/low teploty znalost nízkou teploty hello hello den vám umožní toomake přiměřené odhad v hello vysoké. se poněkud redundantní Hello informace obsažené v hello dvě funkce.

* Klasifikace dat do více než dvou kategorií lze provést pomocí buď třídění ze své podstaty více tříd, nebo kombinace sady two-class třídění do **komplet**. V hello komplet přístup, je samostatnou třídění two-class pro každou třídu – každé z nich hello data dělí do dvou kategorií: "Tato třída" a "není tato třída." Potom tyto třídění hlasovat o správné přiřazení hello hello datového bodu. Je to hello provozní Princip za [One-vs-All Multiclass][one-vs-all-multiclass].

* Několik metod, včetně logistic regression a počítač bodu Bayes hello, předpokládá **lineární třída hranice**. To znamená, že předpokládá, že hello hranice mezi třídami jsou přibližně přímky (nebo hyperplanes v hello obecnější případ). Často je jím charakteristika dat. hello že nevíte, až poté, co jste se zkusili tooseparate ji, ale je něco, co obvykle můžete poznat předem vizualizace. Pokud hranice třída hello vypadají velmi nestandardní, přilepit s rozhodovací stromy, decision Džungle, podporu vektoru počítače nebo neuronové sítě.

* Neuronové sítě lze použít s kategorií proměnné tak, že vytvoříte **fiktivní proměnné** pro každou kategorii jeho nastavení too1 v případech, kdy se použije hello kategorie, 0, kde není.


<!-- This is how you can embed a link in an image in HTML. Don't know how toodo this in markdown.
<a href="http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet.pdf">
<img src="C:\Users\garye\azure-docs-pr\articles\media\machine-learning-algorithm-cheat-sheet\cheat-sheet-small.png">
</a>
-->

<!-- Module References -->
[a-z-list]: https://msdn.microsoft.com/library/azure/dn906033.aspx
[initialize-model]: https://msdn.microsoft.com/library/azure/0c67013c-bfbc-428b-87f3-f552d8dd41f6/
[k-means-clustering]: https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/
[one-vs-all-multiclass]: https://msdn.microsoft.com/library/azure/7191efae-b4b1-4d03-a6f8-7205f87be664/
