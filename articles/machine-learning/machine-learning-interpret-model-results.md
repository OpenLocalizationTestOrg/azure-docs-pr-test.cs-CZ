---
title: "model aaaInterpret za následek Machine Learning | Microsoft Docs"
description: "Jak nastavit toochoose hello optimální parametr algoritmu pomocí a vizualizace score model výstupy."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6230e5ab-a5c0-4c21-a061-47675ba3342c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 52161b1aa5ff3e7a63fc4b1bfb7c5e354eabcc50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="interpret-model-results-in-azure-machine-learning"></a>Interpretovat výsledky modelu v Azure Machine Learning
Toto téma vysvětluje, jak toovisualize a interpretovat výsledky předpovědi v Azure Machine Learning Studio. Po Trénink modelu a provádí předpovědi nad jeho ("vypočítat skóre modelu hello"), potřebujete toounderstand a interpretovat hello předpovědi výsledek.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Existují čtyři hlavní druhy strojového učení modely v Azure Machine Learning:

* klasifikace
* Clustering
* Regrese
* Doporučené systémy

Hello moduly používané pro předpověď nad tyto modely jsou:

* [Určení skóre modelu] [ score-model] modulu pro klasifikaci a regrese
* [Přiřadit tooClusters] [ assign-to-clusters] modulu pro clustering
* [Stanovení skóre doporučené Matchbox] [ score-matchbox-recommender] systémů doporučení

Tento dokument popisuje, jak toointerpret předpovědi výsledky pro každý z těchto modulů. Přehled těchto modulů najdete v tématu [jak toochoose parametry toooptimize algoritmy v Azure Machine Learning](machine-learning-algorithm-parameters-optimize.md).

Toto téma nabízí interpretace předpovědi, ale ne zkušební modelu. Další informace o tom, tooevaluate k modelu, najdete v části [jak tooevaluate modelu výkon v Azure Machine Learning](machine-learning-evaluate-model-performance.md).

Pokud jsou nové tooAzure Machine Learning a potřebujete nápovědu, vytvoření jednoduchého experimentu tooget, spuštění, najdete v části [vytvoření jednoduchého experimentu v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md) v Azure Machine Learning Studio.

## <a name="classification"></a>klasifikace
Existují dvě podkategorie klasifikaci problémy:

* Problémy s pouze dvěma třídami (dvě třídy nebo binární klasifikace)
* Problémy s více než dvě třídy (třídy více klasifikace)

Azure Machine Learning má jiné moduly toodeal s každou z těchto typů klasifikace, ale jsou podobné hello metody pro interpretace jejich výsledky předpovědi.

### <a name="two-class-classification"></a>Klasifikace dva – třída
**Příklad experimentu**

Klasifikace hello iris květy je například dvě třídy klasifikace problému. Úloha Hello je květy iris tooclassify podle jejich funkce. Hello Iris datové sady v Azure Machine Learning je podmnožinou hello oblíbených [Iris datové sady](http://en.wikipedia.org/wiki/Iris_flower_data_set) obsahující instance jen dvě květinové druhů (třídy 0 a 1). Existují čtyři funkce pro každý květina (sepal délka, šířka sepal, Okvětní lístek délku a šířku Okvětní lístek).

![Snímek obrazovky iris experimentu](./media/machine-learning-interpret-model-results/1.png)

Obrázek 1. Iris two-class klasifikace problému experimentu

Experiment byl provádět toosolve tyto potíže odstranit, jak je znázorněno na obrázku 1. Two-class boosted decision stromu modelu byla vycvičena a vypočítat skóre. Teď můžete vizualizovat hello předpovědi výsledky z hello [Score Model] [ score-model] modulu kliknutím hello výstupní port modulu hello [Score Model] [ score-model]modul a potom kliknutím na **vizualizovat**.

![Modul určení skóre modelu](./media/machine-learning-interpret-model-results/1_1.png)

Otevře hello vyhodnocování výsledků, jak je znázorněno na obrázku 2.

![Výsledky iris two-class klasifikace experimentu](./media/machine-learning-interpret-model-results/2.png)

Obrázek 2. Vizualizace výsledků skóre v klasifikaci two-class

**Výsledek vyhodnocení**

V tabulce výsledků hello je šest sloupců. Hello levém čtyři sloupce jsou čtyři funkce hello. Hello právo dva sloupce, popisky vyhodnocení a skóre pro Magnitudu Pravděpodobnostech jsou výsledky předpovědi hello. Hello skóre pro Magnitudu Pravděpodobnostech sloupec zobrazuje hello pravděpodobnost, že květiny patří toohello kladné třídy (třídy 1). Například hello v hello sloupce (0.028571) znamená, že 0.028571 pravděpodobnost, že hello první květina patří tooClass 1 se první číslo. Hello popisky vyhodnocení sloupec zobrazuje hello předpovědět třídu pro každý květina. To je založené na sloupec skóre pro Magnitudu Pravděpodobnostech hello. Pokud je větší než 0,5 hello skóre pro magnitudu pravděpodobnosti květiny, je předpovědět jako třída 1. Jinak je předpovědět jako třída 0.

**Publikování webové služby**

Po hello předpovědi výsledky byly porozuměl jsem jim a zvukových, která, hello experimentu lze publikovat jako webovou službu, aby mohli nasadit v různých aplikacích a volání na všechny nové květina iris tooobtain třída předpovědi. toolearn způsobu toochange školení experimentovat do vyhodnocování experiment a publikujete ji jako webové služby, najdete v části [publikovat webovou službu Azure Machine Learning hello](machine-learning-walkthrough-5-publish-web-service.md). Tento postup obsahuje vyhodnocování experiment, jak je vidět na obrázku 3.

![Snímek obrazovky vyhodnocování experimentu](./media/machine-learning-interpret-model-results/3.png)

Obrázek 3. Vyhodnocování hello iris two-class klasifikace problému experimentu

Teď musíte tooset hello vstup a výstup pro hello webovou službu. Hello vstup je hello pravý vstupní port [Score Model][score-model], což je hello Iris květina funkce vstup. Hello výběr výstup hello závisí na tom, jestli vás zajímá v hello předpovědět – třída (scored popisek), hello skóre pro magnitudu pravděpodobnosti nebo obojí. V tomto příkladu se předpokládá, že máte zájem obě. tooselect hello potřeby výstupních sloupcích, použití [výběr sloupců v datové sadě] [ select-columns] modulu. Klikněte na tlačítko [výběr sloupců v datové sadě][select-columns], klikněte na tlačítko **spustit selektor sloupců**a vyberte **popisky vyhodnocení** a **Scored Pravděpodobnostech**. Po nastavení hello výstupní port modulu [výběr sloupců v datové sadě] [ select-columns] a znovu spustit, byste měli mít připravené toopublish hello vyhodnocování experimentu jako webovou službu kliknutím **publikovat WEB Služba**. Konečný experiment Hello vypadá jako na obrázku 4.

![Hello iris two-class klasifikace experimentu](./media/machine-learning-interpret-model-results/4.png)

Obrázek 4. Konečný experiment vyhodnocování iris two-class klasifikace problému

Po spuštění hello webové služby a zadejte hodnoty některé funkce testovací instance, vrátí výsledek hello dvou čísel. první číslo Hello je hello skóre pro magnitudu popisek a hello druhou je hello skóre pro magnitudu pravděpodobnosti. Tato květina je předpovědět jako třída 1 s 0.9655 pravděpodobnosti.

![Interpretace score model testů](./media/machine-learning-interpret-model-results/4_1.png)

![Vyhodnocování výsledků testu](./media/machine-learning-interpret-model-results/5.png)

Obrázek 5. Webové služby výsledek iris two-class klasifikace

### <a name="multi-class-classification"></a>Klasifikace více – třída
**Příklad experimentu**

V této experimentu můžete provést úlohu písmeno rozpoznávání jako příklad více třídami klasifikace. Hello třídění pokusí toopredict určitou písmeno (třída) na základě některé ručně psané atribut hodnot z bitové kopie ručně psané hello extrahovat.

![Příklad rozpoznávání písmeno](./media/machine-learning-interpret-model-results/5_1.png)

V hello Cvičná data jsou 16 funkce, které se extrahují z bitové kopie ručně psané písmeno. Hello 26 písmena formuláři naše 26 třídy. Obrázek, že 6 ukazuje experimentu, který bude cvičení více třídami klasifikaci modelu pro rozpoznávání písmeno a předvídání na hello stejné sady na testovací datové sady funkcí.

![Písmeno rozpoznávání více třídami klasifikace experimentu](./media/machine-learning-interpret-model-results/6.png)

Obrázek 6. Písmeno rozpoznávání více třídami klasifikace problému experimentu

Vizualizace výsledků hello hello [Score Model] [ score-model] modulu kliknutím hello výstupní port modulu [Score Model] [ score-model] modul a potom Kliknutím na tlačítko **vizualizovat**, obsah byste měli vidět, jak je znázorněno na obrázku 7.

![Určení skóre modelu výsledky](./media/machine-learning-interpret-model-results/7.png)

Na obrázku 7. Vizualizace výsledků skóre modelu v klasifikaci s více – třída

**Výsledek vyhodnocení**

levé 16 sloupců Hello představují hello funkce hodnoty hello testovací sada. Hello sloupce s názvy jako skóre pro Magnitudu Pravděpodobnostech pro třídu "XX" jsou stejně jako hello skóre pro Magnitudu Pravděpodobnostech sloupce v případě two-class hello. Zobrazují, že hello pravděpodobnost, že hello odpovídající záznam spadá do určité třídy. Například pro první položku hello, není 0.003571 pravděpodobnost, že se jedná "A", 0.000451 pravděpodobnost, že je "B" a tak dále. poslední sloupec Hello (popisky Scored) je hello stejné jako popisky vyhodnocení v případě two-class hello. Vybere třídu hello s hello největší skóre pro magnitudu pravděpodobnosti jako hello předpokládaných třídu hello odpovídající záznam. Pro první položku hello, hello skóre pro magnitudu popisek je například "F" vzhledem k tomu, že má největší pravděpodobnosti toobe hello "F" (0.916995).

**Publikování webové služby**

Můžete také získat hello skóre pro magnitudu popisek pro každou položku a hello pravděpodobnost hello skóre pro magnitudu popisek. základní logika Hello je toofind hello největší pravděpodobnosti mezi všechny hello skóre pro magnitudu pravděpodobnostech. toodo, budete potřebovat toouse hello [spustit skript jazyka R] [ execute-r-script] modulu. Hello kódu jazyka R je vidět na obrázku 8 a hello výsledek hello experimentu je vidět na obrázku 9.

![Ukázkový kód R](./media/machine-learning-interpret-model-results/8.png)

Obrázek 8. Kód R pro extrahování popisky vyhodnocení a hello přidružené pravděpodobnostech popisků hello

![Výsledek experimentu](./media/machine-learning-interpret-model-results/9.png)

Obrázek 9. Konečný experiment vyhodnocování hello písmeno rozpoznávání více třídami klasifikace problému

Po publikování a spuštění hello webové služby a zadejte některé funkce vstupní hodnoty, vrátí hello výsledek vypadá jako obrázek 10. Toto ručně psané písmeno s jeho extrahované 16 funkce, je s 0.9715 pravděpodobnost předpovězené toobe "T".

![Interpretace modul skóre test](./media/machine-learning-interpret-model-results/9_1.png)

![Výsledek testu](./media/machine-learning-interpret-model-results/10.png)

Obrázek 10. Webové služby výsledek více třídami klasifikace

## <a name="regression"></a>Regrese
Regrese problémy se liší od klasifikaci problémy. V problém klasifikace které se snažíte toopredict diskrétní třídy, jako je například které třídy iris květina patří do. Ale jak můžete vidět v hello následující ukázka regrese problému, které se snažíte toopredict průběžné proměnná, jako například hello ceny automobilu.

**Příklad experimentu**

Použijte předpověď cen automobilů jako příkladu pro regresní. Pokoušíte toopredict hello ceny automobilu na základě jeho funkcí, včetně Ujistěte se, typ paliva, typ těla zprávy a kolečka jednotky. Obrázek 11 ukazuje Hello experimentu.

![Experiment regrese automobilů cena](./media/machine-learning-interpret-model-results/11.png)

Obrázek 11. Experiment problém regrese automobilů cena

Detekční hello [Score Model] [ score-model] modul, výsledkem hello vypadá jako obrázek 12.

![Vyhodnocování výsledky pro problém předpověď cen automobilů](./media/machine-learning-interpret-model-results/12.png)

Obrázek 12. Vyhodnocování výsledek problém předpověď cen automobilů hello

**Výsledek vyhodnocení**

Scored popisky je sloupec výsledků hello v této vyhodnocování výsledek. čísla Hello jsou hello předpokládaných ceny pro každý Auto.

**Publikování webové služby**

Můžete publikovat hello experimentu regrese do webové služby a pojmenujte ji pro předpověď cen automobilů v hello stejným způsobem jako v hello two-class klasifikace případy použití.

![Vyhodnocování experimentu pro problém regrese automobilů cena](./media/machine-learning-interpret-model-results/13.png)

Obrázek 13. Vyhodnocování experimentu problémem regrese automobilů cena

Spuštění hello webové služby, hello vrátila výsledek vypadá jako obrázek 14. Hello předpokládaných ceny pro tento car je $15,085.52.

![Interpretace vyhodnocování modulu testování](./media/machine-learning-interpret-model-results/13_1.png)

![Vyhodnocování výsledky modulu](./media/machine-learning-interpret-model-results/14.png)

Obrázek 14. Webové služby výsledek problémem regrese automobilů cena

## <a name="clustering"></a>Clustering
**Příklad experimentu**

Použijeme hello Iris datovou sadu znovu toobuild clustering experimentu. Zde můžete filtrovat se popisky hello třídy v sadě dat hello tak, aby pouze funkce a lze použít pro clustering. V této iris případ použití, zadejte hello počet clusterů toobe dva během hello školení, což znamená, že by clusteru hello květy do dvou tříd. Hello experiment vidíte na obrázku 15.

![Experiment Iris clustering problém](./media/machine-learning-interpret-model-results/15.png)

Obrázek 15. Experiment Iris clustering problém

Clustering se liší od klasifikace, v tom, že hello trénovací datové sady nemá základů pravdivosti popisky samostatně. Clustering skupiny hello školení instancí datové sady do různých clusterů. Během školení hello hello modelu popisky hello položky podle učení hello rozdíly mezi jejich funkce. Potom můžete použít hello trained model toofurther klasifikovat budoucí položky. Existují dvě části hello výsledku, které jsme zajímají v rámci clustering problém. první část Hello je označování hello trénovací datové sady a hello druhý je klasifikace nové datové sady s hello trained model.

Hello první část výsledku hello můžete vizualizovat kliknutím hello zbývajících výstupní port modulu [Train Model clusteringu] [ train-clustering-model] a pak levým na **vizualizovat**. vizualizace Hello vidíte na obrázku 16.

![Clustering výsledek](./media/machine-learning-interpret-model-results/16.png)

Obrázek 16. Vizualizace clustering výsledek hello trénovací datové sady

výsledek Hello druhou část hello clustering nové položky s hello trained model clusteringu, se zobrazí v obrázek 17.

![Vizualizace výsledků clustering](./media/machine-learning-interpret-model-results/17.png)

Obrázek 17. Vizualizace clustering výsledek pro nové sady dat

**Výsledek vyhodnocení**

I když hello výsledky hello dvě části kmen z různých experimentu fázích, hledají hello stejné a vyhodnocovány v hello stejným způsobem. Hello první čtyři sloupce jsou funkce. poslední sloupec Hello přiřazení, je výsledek předpovědi hello. hello Hello položky přiřazené stejné číslo jsou předpovědět toobe v hello stejné clusteru, který je sdílejí podobnosti nějakým způsobem (Tento experiment používá metrika Euclidean vzdálenost výchozí hello). Protože jste určili hello počet clusterů toobe 2, jsou označeny hello položek v přiřazení 0 nebo 1.

**Publikování webové služby**

Můžete publikovat hello clustering experimentu do webové služby a pojmenujte ji pro clustering předpovědi hello stejným způsobem, jak v klasifikaci two-class hello případ použití.

![Vyhodnocování experimentu pro clustering problém iris](./media/machine-learning-interpret-model-results/18.png)

Obrázek 18. Vyhodnocování experimentu problémem iris clustering

Po spuštění hello webová služba vrátila hello výsledek vypadá jako obrázek 19. Tato květina je předpokládaných toobe v clusteru 0.

![Test interpretovat vyhodnocování modulu](./media/machine-learning-interpret-model-results/18_1.png)

![Vyhodnocování výsledek modulu](./media/machine-learning-interpret-model-results/19.png)

Obrázek 19. Webové služby výsledek iris two-class klasifikace

## <a name="recommender-system"></a>Doporučené systému
**Příklad experimentu**

Pro systémy doporučené, můžete problém doporučení restaurace hello jako příklad: Doporučujete restaurace pro zákazníky na základě jejich historie hodnocení. vstupní data Hello se skládá ze tří částí:

* Restaurace hodnocení zákazníků
* Data funkce zákazníka
* Data funkce restaurace

Existuje několik věcí, můžeme dělat s hello [Train Matchbox doporučené] [ train-matchbox-recommender] modulu v Azure Machine Learning:

* Předpověď hodnocení pro daného uživatele a položek
* Doporučujeme položky tooa zadané uživatele
* Najít uživatele související tooa zadané uživatele
* Vyhledání položky související tooa zadané položky

Můžete zvolit, co chcete toodo výběrem z možností hello čtyři v hello **doporučené předpovědi druh** nabídky. Zde si můžete projít všechny čtyři scénáře.

![Doporučené matchbox](./media/machine-learning-interpret-model-results/19_1.png)

Typické experimentu Azure Machine Learning pro systém doporučené vypadá jako obrázek 20. Informace o tom, jak toouse systému tyto doporučené moduly najdete v části [Train matchbox doporučené] [ train-matchbox-recommender] a [skóre matchbox doporučené] [ score-matchbox-recommender].

![Doporučené systému experimentu](./media/machine-learning-interpret-model-results/20.png)

Obrázek 20. Doporučené systému experimentu

**Výsledek vyhodnocení**

**Předpověď hodnocení pro daného uživatele a položek**

Výběrem **hodnocení předpovědi** pod **doporučené předpovědi druh**, jsou doporučené hello žádostí hello toopredict systému hodnocení pro daného uživatele a položku. Hello vizualizace hello [skóre Matchbox doporučené] [ score-matchbox-recommender] výstup vypadá jako obrázek 21.

![Stanovení skóre výsledek hello doporučené systému – hodnocení předpovědi](./media/machine-learning-interpret-model-results/21.png)

Obrázek 21. Vizualizace výsledků skóre hello hello doporučené systému – předpovědi hodnocení

první dva sloupce Hello jsou páry položka uživatele hello poskytované hello vstupní data. třetí sloupec Hello je hello předpokládaných hodnocení uživatele určité položky. Například v prvním řádku hello předpovědět zákazníka, je U1048 toorate restaurace 135026 jako 2.

**Doporučujeme položky tooa zadané uživatele**

Výběrem **položky doporučení** pod **doporučené předpovědi typ**, nemůže ověřit hello doporučené systému toorecommend položky tooa zadaný uživatel. Hello poslední parametr toochoose v tomto scénáři je *doporučená výběr položek*. Hello možnost **z hodnocení položky (pro vyhodnocení modelu)** je určen pro vyhodnocení modelu během procesu školení hello. Pro tuto fázi předpovědi vybereme možnost **z všechny položky**. Hello vizualizace hello [skóre Matchbox doporučené] [ score-matchbox-recommender] výstup vypadá jako obrázek 22.

![Výsledek skóre doporučené systému – položka doporučení](./media/machine-learning-interpret-model-results/22.png)

Obrázek 22. Vizualizace výsledků skóre hello doporučené systému – položka doporučení

Hello první hello půl sloupce představuje hello zadané položky toorecommend ID uživatele, jak poskytované hello vstupní data. Hello jiných pět sloupce představují doporučené toohello uživatele v sestupném pořadí podle relevance položky hello. Například v prvním řádku hello hello je doporučeno restaurace pro zákazníka U1048 je 134986, za nímž následuje 135018, 134975, 135021 a 132862.

**Najít uživatele související tooa zadané uživatele**

Výběrem **související uživatelé** pod **doporučené předpovědi druh**, nemůže ověřit hello doporučené systém toofind související uživatelé tooa zadaný uživatel. Související se hello uživatelů, kteří mají podobné předvolby. Hello poslední parametr toochoose v tomto scénáři je *související výběr uživatele*. Hello možnost **z uživatelů, hodnocení položky (pro vyhodnocení modelu)** je určen pro vyhodnocení modelu během procesu školení hello. Zvolte **ze všech uživatelů** pro tuto fázi předpovědi. Hello vizualizace hello [skóre Matchbox doporučené] [ score-matchbox-recommender] výstup vypadá jako obrázek 23.

![Výsledek skóre doporučené systému – související uživatelé](./media/machine-learning-interpret-model-results/23.png)

Obrázek 23. Vizualizace výsledků skóre hello doporučené systému – související uživatelé

Hello nejprve z hello půl sloupce ukazuje hello zadané uživatelské ID potřeby toofind související uživatelů, jako poskytované vstupní data. Hello jiného úložiště pět sloupců hello předpokládaných související uživatelé hello uživatele v sestupném pořadí podle závažnosti. Například v hello první řádek, je nejdůležitější zákazníka hello zákazníka U1048 nástroje U1051, za nímž následuje U1066, U1044, U1017 a U1072.

**Vyhledání položky související tooa zadané položky**

Výběrem **související položky** pod **doporučené předpovědi druh**, jsou doporučené hello žádostí systému toofind související položky tooa danou položku. Související položky jsou hello položky nejpravděpodobnější toobe líbilo podle hello stejné uživatele. Hello poslední parametr toochoose v tomto scénáři je *související výběr položek*. Hello možnost **z hodnocení položky (pro vyhodnocení modelu)** je určen pro vyhodnocení modelu během procesu školení hello. Vybereme možnost **z všechny položky** pro tuto fázi předpovědi. Hello vizualizace hello [skóre Matchbox doporučené] [ score-matchbox-recommender] výstup vypadá jako obrázek 24.

![Výsledek skóre doporučené systému – související položky](./media/machine-learning-interpret-model-results/24.png)

Obrázek 24. Vizualizace výsledků skóre hello doporučené systému – související položky

Hello první hello půl sloupce představuje hello zadané ID položek potřeby toofind související položky, jako poskytované hello vstupní data. Hello jiného úložiště pět sloupců hello předpokládaných související položky hello položky v sestupném pořadí z hlediska relevance. Například v prvním řádku hello je hello nejdůležitější položku pro položku 135026 135074, za nímž následuje 135035, 132875, 135055 a 134992.

**Publikování webové služby**

Hello proces publikování těchto experimenty jako webové služby tooget předpovědi je podobné pro všechny čtyři scénáře hello. Zde jsme trvat hello druhý scénář (doporučujeme položky tooa zadané uživatele) jako příklad. Můžete postupovat podle hello stejný postup s hello další tři.

Ukládání hello Trénink doporučené systému jako modulu trained model a filtrování tooa hello vstupní data ve sloupci ID jednoho uživatele jako požadovaný, můžete spojit hello experimentu jako obrázek 25 a publikujete ji jako webovou službu.

![Vyhodnocování experimentu hello restaurace doporučení problému](./media/machine-learning-interpret-model-results/25.png)

Obrázek 25. Vyhodnocování experimentu hello restaurace doporučení problému

Spuštění hello webové služby, hello vrátila výsledek zdá 26 obrázek. Hello pět doporučené restaurace pro uživatele U1048 jsou 134986, 135018, 134975, 135021 a 132862.

![Ukázka doporučené systémové služby](./media/machine-learning-interpret-model-results/25_1.png)

![Ukázkový experiment výsledky](./media/machine-learning-interpret-model-results/26.png)

Obrázek 26. Webové služby výsledek restaurace doporučení problému

<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
