---
title: "aaaUsing lineární regrese v Machine Learning | Microsoft Docs"
description: "Porovnání modelů lineární regrese v aplikaci Excel a v nástroji Azure Machine Learning Studio"
metakeywords: 
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 417ae6ab-de4f-4bdd-957a-d96133234656
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: kbaroni;garye
ms.openlocfilehash: 8716040ad296053a72fb06c7c9660a186123ac15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-linear-regression-in-azure-machine-learning"></a>Používání lineární regrese ve službě Azure Machine Learning
> *Kate Baroni* a *Ben Boatman* jsou podnikové řešení architekty ve společnosti Microsoft Data Statistika špičkové Center. V tomto článku popisují jejich prostředí migraci existujícího regrese analysis suite tooa cloudové řešení pomocí Azure Machine Learning. 
> 
> 

&nbsp; 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="goal"></a>Cílem
Naše projektu začít s dva cíle v paměti: 

1. Použít prediktivní analýzy tooimprove hello přesnost naší organizace měsíční výnosy projekce 
2. Pomocí Azure Machine Learning tooconfirm, optimalizace, zvýšení rychlosti a škálování naše výsledků. 

Například mnoho firem naší organizace projde měsíční výnosy prognózy procesu. Náš malé tým obchodní analytici úkolem bylo pomocí Azure Machine Learning toosupport hello procesu a zvyšte tak přesnost předpovědi. tým Hello stráví několik měsíců, shromažďování dat z více zdrojů a s atributy datového hello prostřednictvím statistické analýzy identifikace prognózy prodeje relevantní tooservices klíčových atributů. dalším krokem Hello se toobegin při vytváření prototypu statistické regrese modely na hello dat v aplikaci Excel. Během pár týdnů jsme měli regresní model aplikace Excel, který byl outperforming hello aktuální pole a finanční prognózy procesy. To se stala výsledek předpovědi hello směrného plánu. 

Jsme pak převzal hello další krok toomoving naše prediktivní analýzy tooAzure Machine Learning toofind na tom, jak může zlepšit Machine Learning prediktivní výkonu.

## <a name="achieving-predictive-performance-parity"></a>Dosažení paritní prediktivní výkonu
Naše první prioritou se tooachieve rozdíly mezi regrese modely Machine Learning a Excel. Zadaný text hello stejná data a hello stejné rozdělení pro trénování a testování dat, jsme chtěli tooachieve prediktivní výkonu rozdíly mezi aplikace Excel a Machine Learning. Původně se nepovedlo. model aplikace Excel Hello outperformed model Machine Learning hello. selhání Hello byl z důvodu nedostatku tooa Principy základní nástroje hello nastavení v Machine Learning. Po synchronizaci s hello Machine Learning produktový tým jsme získávají lépe porozumět hello základní nastavení požadovaná pro naše sady dat a dosáhnout rozdíly mezi dva modely hello. 

### <a name="create-regression-model-in-excel"></a>Vytvořit regresní model v aplikaci Excel
Naše aplikace Excel Regrese používá model hello standardní lineární regrese v hello Excel analytické nalezen. 

Jsme vypočítat *% střední absolutní chyba* a použít ho jako hello měření výkonu pro hello model. Trvalo tooarrive 3 měsíce v pracovní model pomocí aplikace Excel. Můžeme uvést do režimu většinu hello learning do hello experimentu Machine Learning Studio, která byla nakonec výhodné v Principy požadavků.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Vytvoření porovnatelný z hlediska experimentu v nástroji Azure Machine Learning
Jsme postupovali podle těchto kroků toocreate naše experimentu v nástroji Machine Learning Studio: 

1. Nahrané hello datovou sadu jako tooMachine soubor sdílený svazek clusteru Learning Studio (velmi malý soubor)
2. Vytvoří nový experiment a použít hello [výběr sloupců v datové sadě] [ select-columns] tooselect modulu hello stejné funkce dat používané v aplikaci Excel 
3. Použít hello [rozdělení dat] [ split] modulu (s *relativní výraz* režim) toodivide hello data do stejné datové sady školení hello, jako kdyby byly v aplikaci Excel 
4. Experimented s hello [lineární regrese] [ linear-regression] modulu (výchozí možnosti pouze), zdokumentované a porovnání hello výsledky tooour Excel regresní model

### <a name="review-initial-results"></a>Zkontrolujte počáteční výsledky
Na první pohled model aplikace Excel hello jasně outperformed model Machine Learning Studio hello: 

|  | Excel | Studio |
| --- |:---:|:---:|
| Výkon | | |
| <ul style="list-style-type: none;"><li>Upraví hranaté R</li></ul> |0.96 |Není k dispozici |
| <ul style="list-style-type: none;"><li>Koeficient <br />Rozhodnutí</li></ul> |Není k dispozici |0.78<br />(nízkou přesnost) |
| Střední absolutní chyba |$9. 5M |$ 19.4 M |
| Střední absolutní chyba (%) |6.03% |12.2% |

Když jsme narazili naše proces a výsledky hello vývojáři a datových vědců na tým Machine Learning hello, poskytnou rychle některé užitečné tipy. 

* Při použití hello [lineární regrese] [ linear-regression] modulu v nástroji Machine Learning Studio, jsou k dispozici dvě metody:
  * Online klesání přechodu: Může být vhodnější pro problémy s větším měřítku
  * Obyčejnou nejmenší čtverce: Toto je hello metodu, kterou většina lidí zamyslet nad při zazní lineární regrese. Pro malé datové sady může být obyčejnou čtverce nejmenší více optimální volbou.
* Zvažte, postupně je upravujte hello L2 regulaci váhy parametr tooimprove výkonu. Je ve výchozím nastavení too0.001, ale pro naše malé datové sady jsme ji nastavit too0.005 tooimprove výkonu. 

### <a name="mystery-solved"></a>Mystery vyřeší!
Když jsme použili hello doporučení, jsme dosáhnout hello stejného standardních hodnot výkonu v nástroji Machine Learning Studio stejně jako u aplikace Excel: 

|  | Excel | Studio (výchozí) | Studio s nejmenší čtverce |
| --- |:---:|:---:|:---:|
| Hodnota s popiskem |Skutečné hodnoty (číslice) |stejné |stejné |
| Student |Excel -> datové analýzy -> regrese |Lineární regrese. |Lineární regrese |
| Možnosti student |Není k dispozici |Výchozí hodnoty |obyčejnou nejmenší čtverce<br />L2 = 0,005 |
| Datové sady |26 řádky, funkce 3, 1 popisek. Všechny číselný. |stejné |stejné |
| Rozdělení: Train |Excel trénink na hello nejprve 18 řádků, otestovali na hello poslední 8 řádků. |stejné |stejné |
| Rozdělení: Test |V aplikaci Excel použít vzorec regrese toohello poslední 8 řádků |stejné |stejné |
| **Výkon** | | | |
| Upraví hranaté R |0.96 |Není k dispozici | |
| Koeficient spolehlivosti |Není k dispozici |0.78 |0.952049 |
| Střední absolutní chyba |$9. 5M |$ 19.4 M |$9. 5M |
| Střední absolutní chyba (%) |<span style="background-color: 00FF00;"> 6.03%</span> |12.2% |<span style="background-color: 00FF00;"> 6.03%</span> |

Kromě toho hello Excel koeficienty porovnání dobře toohello funkce váhu v hello Azure trained model:

|  | Koeficienty aplikace Excel | Váhu funkcí služby Azure |
| --- |:---:|:---:|
| Krádež nebo odchylka |19470209.88 |19328500 |
| Funkce A |0.832653063 |0.834156 |
| Součást B |11071967.08 |11007300 |
| Funkce C |25383318.09 |25140800 |

## <a name="next-steps"></a>Další kroky
Jsme chtěli webové službě Machine Learning tooconsume hello v aplikaci Excel. Naše obchodní analytici závisí na aplikaci Excel a My potřeby způsob toocall hello Machine Learning webové služby s řádek dat v aplikaci Excel a mějte ho vrátit hello předpovědět tooExcel hodnotu. 

Můžeme také chtěli toooptimize našeho modelu pomocí možnosti hello a algoritmy, které jsou k dispozici v nástroji Machine Learning Studio.

### <a name="integration-with-excel"></a>Integrace s aplikace Excel
Naše řešení se toooperationalize naše Machine Learning regresní model vytvořením webové služby ze hello trained model. Během několika minut byla vytvořena hello webové služby a může říkáme mu přímo z aplikace Excel tooreturn výnosy předpovězené hodnoty. 

Hello *řídicího panelu webové služby* část obsahuje ke stažení sešitu aplikace Excel. Hello sešit obsahuje předem formátovaný hello webové rozhraní API a schémat informace o služby vložených. Když kliknete na tlačítko *stáhnout sešitu aplikace Excel*, otevře se sešit hello a jste jej uložili tooyour místního počítače. 

![][1]

S hello sešit otevřít zkopírujte předdefinované parametry do části parametr hello blue, jak je uvedeno níže. Po zadání parametrů hello Excel volá out toohello webové službě Machine Learning a hello předpokládaných scored popisky se zobrazí v části hello zelená předpovězené hodnoty. sešit Hello bude toocreate předpovědi parametrů na základě modelu vyškolení pro všechny položky řádku zadaný v poli parametrů. Další informace o tom, jak se toouse tuto funkci naleznete v tématu [využívají webové služby Azure Machine Learning z Excelu](machine-learning-consuming-from-excel.md). 

![][2]

### <a name="optimization-and-further-experiments"></a>Optimalizace a další pokusy.
Teď, když jsme měli směrný plán s modelem naše aplikace Excel, můžeme přesunout napřed toooptimize naše Model strojového učení lineární regrese. Použili jsme hello modulu [na základě filtru výběr funkce] [ filter-based-feature-selection] tooimprove na našem výběr elementy počáteční data a pomáhá nám dosáhnout zlepšení výkonu % 4.6 znamenat absolutní chyba. Pro budoucí projekty budeme používat tuto funkci, která může uložit nám týdny iterace v rámci dat atributy toofind hello správnou sadu funkcí toouse pro modelování. 

Dalším plánujeme tooinclude další algoritmy jako [Bayesova] [ bayesian-linear-regression] nebo [Boosted Decision Trees] [ boosted-decision-tree-regression] v našem toocompare experimentu výkon. 

Pokud chcete tooexperiment s regrese, je dobré datovou sadu tootry hello energie efektivitu regrese ukázkovou datovou sadu, která má spoustu číselné atributy. Datová sada Hello je dodáván jako součást hello ukázkových datových sad v nástroji Machine Learning Studio. Můžete použít celou řadu učení moduly toopredict vytápění zatížení nebo chlazení zatížení. porovnání výkonu různých regrese zjišťuje proti hello energetické úspornosti datovou sadu predikci pro hello cíle proměnné chlazení zatížení je graf Hello níže: 

| Model | Střední absolutní chyba | Střední kořenové spolehlivosti chyby | Relativní absolutní chyba | Relativní spolehlivosti chyby | Koeficient spolehlivosti |
| --- | --- | --- | --- | --- | --- |
| Vylepšené rozhodovací strom |0.930113 |1.4239 |0.106647 |0.021662 |0.978338 |
| Lineární regrese (přechodu klesání) |2.035693 |2.98006 |0.233414 |0.094881 |0.905119 |
| Regrese neuronové sítě |1.548195 |2.114617 |0.177517 |0.047774 |0.952226 |
| Lineární regrese (obyčejnou nejmenší čtverce) |1.428273 |1.984461 |0.163767 |0.042074 |0.957926 |

## <a name="key-takeaways"></a>Klíče Takeaways
Jsme se naučili mnoho z regrese spuštěné aplikace Excel a experimenty Azure Machine Learning paralelně. Vytváření standardních hodnot modelu hello v aplikaci Excel a porovná se toomodels pomocí Machine Learning [lineární regrese] [ linear-regression] pomohl nám další Azure Machine Learning a jsme zjištěné příležitosti tooimprove dat Výběr a model výkonu. 

Také nalezen, je vhodné toouse [na základě filtru výběr funkce] [ filter-based-feature-selection] tooaccelerate budoucí předpovědi projekty. S použitím funkce Výběr tooyour data, můžete vytvořit model vylepšené v Machine Learning s lepší celkový výkon. 

Hello možnost tootransfer hello prediktivní analýzy prognózy z Machine Learning tooExcel systemically umožňuje významné zvýšení v hello možnost toosuccessfully poskytnout výsledky skupiny uživatelů tooa široký firmy. 

## <a name="resources"></a>Zdroje
Zde jsou některé prostředky pro umožňují pracovat s regrese: 

* Regrese v aplikaci Excel. Pokud jste se nikdy zkusili Regrese v aplikaci Excel, v tomto kurzu lze snadno: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Regrese vs prognózy. Tyler Chessman napsali článku na blogu, která vysvětluje, jak toodo časové řady prognózy v aplikaci Excel, který obsahuje popis dobrý začátečníka lineární regrese. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts) 
* Obyčejnou čtverce nejmenší lineární regrese: Nedostatky, problémy a nástrahy. Úvod a diskuzi o regrese: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/)

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

