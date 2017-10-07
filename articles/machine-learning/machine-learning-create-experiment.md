---
title: "aaaA jednoduchý experiment v nástroji Machine Learning Studio | Microsoft Docs"
description: "Tento kurz Machine Learningu vás provede jednoduchým experimentem z oblasti datové vědy. Jsme budete předvídání hello ceny automobilu pomocí regresní algoritmus."
keywords: "experiment,lineární regrese,algoritmy Machine Learningu,kurz Machine Learningu,techniky prediktivního modelování,experiment z oblasti datové vědy"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: fb215851d380acf7d0f4934a426283369f9c4ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Kurz Machine Learningu: Vytvoření prvního experimentu z oblasti datové vědy v nástroji Azure Machine Learning Studio

Pokud jste **Azure Machine Learning Studio** nikdy nepoužívali, je tento kurz právě pro vás.

V tomto kurzu budeme zabývat jak toouse Studio pro hello nejprve čas toocreate experimentu strojového učení. Hello experiment testovat analytical modelu, který bude předpovídat cenu automobilu podle různých proměnných, například značky a technických specifikací hello.

> [!NOTE]
> Tento kurz ukazuje základy hello modulů jak toodrag myší do experimentu, připojte je společně, spustit hello experiment a podívejte se na výsledky hello. Nebudete jsme, že toodiscuss hello obecné téma strojové učení nebo jak tooselect a používání hello 100 + předdefinované algoritmy a data manipulaci s moduly součástí Studio.
>
>Pokud jste nový learning toomachine, hello série videí [vědecké zpracování dat pro začátečníky](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) může být vhodné místo toostart. Tato série videí je learning toomachine skvělé Úvod pomocí jazyka pro každý den a koncepty.
>
>Pokud jste obeznámeni s machine learning, ale hledáte další obecné informace o Machine Learning Studio a hello algoritmů strojového učení, které obsahuje, zde jsou některé funkční prostředky:
>
- [Co je Machine Learning Studio?](machine-learning-what-is-ml-studio.md) - Toto téma obsahuje obecný přehled sady Studio.
- [Strojového učení základy s příklady algoritmus](machine-learning-basics-infographic-with-algorithm-examples.md) -tento infografice je užitečné, pokud chcete více informací o hello různé typy algoritmů strojového učení součástí Machine Learning Studio toolearn.
- [Počítač Learning průvodce](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) -Tento průvodce popisuje podobné informace jako hello infografice výše, ale interaktivní formát.
- [Strojového učení rychlý přehled algoritmů](machine-learning-algorithm-cheat-sheet.md) a [jak toochoose algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) – tento plakát ke stažení a doprovodné článku popisují hello Studio algoritmy podrobněji.
- [Machine Learning Studio: Algoritmus a pomáhají modulu](https://msdn.microsoft.com/library/azure/dn905974.aspx) -Toto je hello úplný odkaz pro všechny moduly ze studia, včetně algoritmy strojového učení

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>V čem je přínos nástroje Machine Learning Studio?

Machine Learning Studio umožňuje snadno tooset až experimentu pomocí naprogramovaných s techniky prediktivního modelování přetahování myší moduly.

Pomocí interaktivního vizuálního pracovního prostoru můžete přetáhnout ***datové sady*** a ***moduly*** analýz na interaktivní plátno. Připojení je společně tooform ***experimentovat*** spuštění v nástroji Machine Learning Studio.
Můžete ***vytvoření modelu***, ***trénování modelu hello***, a ***skóre a testování hello modelu***.

Můžete iterace návrhu modelu úpravy hello experiment a poskytuje systémem dokud hello výsledků, které hledáte. Když je model hotový, můžete ho publikovat jako ***webovou službu***, aby mu i ostatní mohli zasílat nová data a získávat na oplátku predikce.

## <a name="open-machine-learning-studio"></a>Otevřít Machine Learning Studio

tooget začít s Studio přejděte příliš[https://studio.azureml.net](https://studio.azureml.net). Pokud už jste se do nástroje Machine Learning Studio někdy přihlašovali, klikněte na odkaz **Přihlásit se**. Jinak klikněte na **Zaregistrovat** a vyberte si mezi bezplatnou a placenou možností.

![Přihlaste se tooMachine Learning Studio][sign-in-to-studio]
<br/>
***Přihlaste se tooMachine Learning Studio***

## <a name="five-steps-toocreate-an-experiment"></a>V pěti krocích toocreate experimentu

V tento kurz strojového učení budete postupovat podle pět základních kroků toobuild experimentu v Machine Learning Studio toocreate, train a stanovení skóre modelu:

- **Vytvoření modelu**
    - [Krok 1: Získání dat]
    - [Krok 2: Příprava hello dat]
    - [Krok 3: Definice příznaků]
- **Train hello model**
    - [Krok 4: Volba a použití algoritmu učení]
- **Skóre a testování hello modelu**
    - [Krok 5: Předpověď cen nových automobilů]

[Krok 1: Získání dat]: #step-1-get-data
[Krok 2: Příprava hello dat]: #step-2-prepare-the-data
[Krok 3: Definice příznaků]: #step-3-define-features
[Krok 4: Volba a použití algoritmu učení]: #step-4-choose-and-apply-a-learning-algorithm
[Krok 5: Předpověď cen nových automobilů]: #step-5-predict-new-automobile-prices

> [!TIP] 
> Můžete najít pracovní kopie hello následující experimentu v hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com). Přejděte příliš**[první vědecké zpracování dat experimentovat - předpověď ceny automobilu](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)**  a klikněte na tlačítko **Open in Studio** toodownload kopii hello experimentu do Machine Learning Pracovní prostor Studio.


## <a name="step-1-get-data"></a>Krok 1: Získání dat

První věc Hello potřebujete tooperform machine learning je data.
Machine Learning Studio obsahuje několik ukázkových datových sad, které můžete použít, nebo můžete data importovat z mnoha zdrojů. V tomto příkladu použijeme hello ukázkovou datovou sadu, **Automobile price data (Raw)**, který je zahrnut do pracovního prostoru.
Tato datová sada obsahuje záznamy řady různých automobilů, včetně informací o značce, modelu, technických specifikacích a ceně.

Zde je, jak tooget hello datové sady do experimentu.

1. Vytvořit nový experiment kliknutím na tlačítko **+ nový** v hello spodní části hello Machine Learning Studio, vyberte **EXPERIMENTOVAT**a potom vyberte **prázdný Experiment**.

2. Hello experimentu je zadána výchozí název, který se zobrazí v horní části hello hello plátna. Vyberte tento text a přejmenujte ji například toosomething smysluplného, **předpověď ceny automobilu**. Název Hello nepotřebuje toobe jedinečný.

    ![Přejmenujte hello experimentu][rename-experiment]

2. toohello nalevo od plátna hello experimentu je paleta datových sad a modulů. Typ **automobilu** hello vyhledávacího pole hello horní části této palety toofind hello datovou sadu s názvem bez přípony **Automobile price data (Raw)**. Přetáhněte plátno experimentu toohello tuto datovou sadu.

    ![Datové sady automobilů hello najděte a přetáhněte ji na plátno experimentu hello][type-automobile]
    <br/>
    ***Datové sady automobilů hello najděte a přetáhněte ji na plátno experimentu hello***

toosee co tato data vypadají, klikněte na výstupní port hello dole hello hello automobilů datové sady a potom vyberte **vizualizovat**.

![Klikněte na výstupní port hello a vyberte "Vizualizovat"][select-visualize]
<br/>
***Klikněte na výstupní port hello a vyberte "Vizualizovat"***

> [!TIP]
> Datových sad a modulů mít vstup a výstup porty reprezentována malé kroužky - vstupních portů v horní části hello, výstup porty v dolní části hello.
toocreate tok dat experimentu, budete výstupní port modulu jeden modul tooan vstupní port jiného připojení.
Kdykoli můžete kliknout na hello výstupní port modulu datové sady nebo modul toosee jaká data hello vypadá jako v tomto bodě v toku dat hello.

V této datové sadě ukázka každá instance automobilu je představována jedním řádkem a hello proměnných spojených s každou automobilu zobrazit jako sloupce. Pro konkrétní automobilu zadané hello proměnné, vytvoříme tootry toopredict hello cenu v sloupec nejvíce vpravo (sloupec 26, s názvem "cena").

![Zobrazení dat automobilů hello v hello okno vizualizace dat][visualize-auto-data]
<br/>
***Zobrazení dat automobilů hello v hello okno vizualizace dat***

Okno vizualizace zavřít hello kliknutím hello "**x**" v pravém horním rohu hello.

## <a name="step-2-prepare-hello-data"></a>Krok 2: Příprava hello dat

Před analýzou datové sady bývá zpravidla nutné sadu nějakým způsobem předzpracovat. Například můžete možná jste si všimli chybějících hodnot ve sloupcích různých řádků hello hello. Tyto chybějící hodnoty se musí toobe vyčistit, aby hello modelu můžete hello data správně analyzovat. V našem případě odstraníme všechny řádky, ve kterých některé hodnoty chybí. Navíc hello **normalized-losses** sloupec má velká část chybějící hodnoty, takže jsme budete vyloučit sloupce zcela z modelu hello.

> [!TIP]
> Čištění hello chybějících hodnot ze vstupních dat je předpokladem pro používání většinu modulů hello.

Nejprve přidáme modul, který odebere hello **normalized-losses** sloupec úplně, a potom přidáme jiný modul, který odstraní všechny řádky, ve kterých chybí data.

1. Typ **vyberte sloupce** hello vyhledávacího pole hello horní části hello modulu palety toofind hello [výběr sloupců v datové sadě] [ select-columns] modulu, přetáhněte ji na plátno experimentu toohello . Tento modul umožňuje tooselect, které sloupce dat, můžeme má tooinclude nebo vyloučení z hello modelu.

2. Připojit hello výstupní port modulu hello **Automobile price data (Raw)** datovou sadu toohello vstupní port hello [výběr sloupců v datové sadě] [ select-columns] modulu.

    ![Přidat na plátno experimentu toohello modul "Vyberte sloupců v datové sadě" hello a připojte ho][type-select-columns]
    <br/>
    ***Přidat na plátno experimentu toohello modul "Vyberte sloupců v datové sadě" hello a připojte ho***

3. Klikněte na tlačítko hello [výběr sloupců v datové sadě] [ select-columns] modulu a klikněte na tlačítko **spustit selektor sloupců** v hello **vlastnosti** podokně.

    - Na levé straně hello, klikněte na tlačítko **s pravidly**
    - V části **Začít s** klikněte na **Všechny sloupce**. To přesměruje [výběr sloupců v datové sadě] [ select-columns] toopass prostřednictvím všechny sloupce hello (s výjimkou tyto sloupce nám o tooexclude).
    - Hello rozevírací seznamy, vyberte **vyloučit** a **názvy sloupců**a klikněte do textového pole hello. Zobrazí se seznam sloupců. Vyberte **normalized-losses**, a je přidaný toohello textové pole.
    - Klikněte na tlačítko hello zaškrtnutí (OK) tooclose hello selektor na sloupců (v pravém dolním hello).

    ![Spustit selektor sloupců hello a vyloučit hello "normalized-losses"][launch-column-selector]
    <br/>
    ***Spustit selektor sloupců hello a vyloučit hello "normalized-losses"***

    Nyní hello podokno vlastností modulu **výběr sloupců v datové sadě** označuje všechny sloupce se bude předávat z hello datové sady kromě **normalized-losses**.

    ![Podokno properties Hello ukazuje, že je tento sloupec hello "normalized-losses" vyloučen][showing-excluded-column]
    <br/>
    ***Podokno properties Hello ukazuje, že je tento sloupec hello "normalized-losses" vyloučen***

    > [!TIP]
    Můžete přidat komentář modul tooa tak, že dvakrát kliknete na panel hello modulu a zadání textu. Díky tomu můžete ihned uvidíte, jaké hello modul provádí v experimentu. V takovém případě klikněte dvakrát na hello [výběr sloupců v datové sadě] [ select-columns] modulu a typu hello komentář "Vyloučit normalized ztráty."
    >
    >


    ![Klikněte dvakrát na modul tooadd komentář][add-comment]
    <br/>
    ***Klikněte dvakrát na modul tooadd komentář***

3. Přetáhněte hello [vyčištění chybějících dat] [ clean-missing-data] toohello modulu experimentovat plátno a připojte ho toohello [výběr sloupců v datové sadě] [ select-columns] modulu. V hello **vlastnosti** podokně, vyberte **odstranit celý řádek** pod **režim čištění**. To přesměruje [vyčištění chybějících dat] [ clean-missing-data] tooclean hello dat odstraněním řádků, které mají všechny chybějící hodnoty. Klikněte dvakrát na modul hello a zadejte komentář hello "Odebrat řádků s chybějícími hodnotami."

    ![Nastavit režim čištění hello příliš "odebrat celý řádek" hello "Vyčištění chybějících dat" modulu][set-remove-entire-row]
    <br/>
    ***Nastavit režim čištění hello příliš "odebrat celý řádek" hello "Vyčištění chybějících dat" modulu***

4. Spusťte hello experiment kliknutím **spustit** v hello dolní části stránky hello.

    Po dokončení spuštění experimentu hello mít všechny moduly hello tooindicate zelená značka zaškrtnutí, která se úspěšně dokončila. Všimněte si také hello **konec běhu** stav v pravém horním rohu hello.

![Po jeho spuštění, hello experiment by měl vypadat přibližně takto][early-experiment-run]
<br/>
***Po jeho spuštění, hello experiment by měl vypadat přibližně takto***

> [!TIP]
> Proč se jsme spustit hello experiment nyní? Ve spuštěné hello experimentu předat hello Definice sloupců pro naše data z datové sady hello prostřednictvím hello [výběr sloupců v datové sadě] [ select-columns] modul a prostřednictvím hello [vyčištění chybějících dat] [ clean-missing-data] modulu. To znamená, že všechny moduly, které jsme se připojit příliš[vyčištění chybějících dat] [ clean-missing-data] má také stejné informace.

Všechny, které jsme provedli v experimentu hello až toothis bodu jsou čisté hello data. Pokud chcete datovou sadu tooview hello čištění, klikněte na tlačítko hello zbývajících výstupní port modulu hello [vyčištění chybějících dat] [ clean-missing-data] modul a vyberte **vizualizovat**. Všimněte si, že hello **normalized-losses** sloupec již není součástí, a neexistují žádné chybějící hodnoty.

Teď, když hello data je čistá, jsme připravené toospecify jaké funkce jsme kliknete toouse v hello prediktivního modelu.

## <a name="step-3-define-features"></a>Krok 3: Definice příznaků

Ve strojovém učení se jako *příznaky* označují jednotlivé měřitelné vlastnosti něčeho, co vás zajímá. V naší datové sadě každý řádek představuje jeden automobil a každý sloupec je příznak daného automobilu.

Hledání dobrý sadu funkcí pro vytvoření prediktivního modelu vyžaduje experimentování a znalost problému hello chcete toosolve. Některé funkce, je lepší pro predikci hello cíl než jiné. Některé příznaky také mají silnou korelaci s jinými a je možné je odebrat. Například city-mpg a highway-mpg jsou úzce související tak, aby jsme udržovat jeden a odebrat hello jiných aniž by to výrazně ovlivnilo hello předpovědi.

Vytvořme model, který používá podmnožinu funkcí hello v naší datové sadě. Můžete téhle akci vrátit později a vybrat jiné příznaky, znovu spusťte hello experiment a v tématu, jestli nedostanete lepší výsledky. Ale toostart, zkuste to prosím ještě hello následující funkce:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Přetáhněte další [výběr sloupců v datové sadě] [ select-columns] toohello modulu experimentovat plátno. Připojte zbývající výstupní port modulu hello hello [vyčištění chybějících dat] [ clean-missing-data] vstup toohello modulu Dobrý den [výběr sloupců v datové sadě] [ select-columns] modulu.

    ![Připojit hello "Vyberte sloupců v datové sadě" modul toohello "Vyčištění chybějících dat" modulu][connect-clean-to-select]
    <br/>
    ***Připojit hello "Vyberte sloupců v datové sadě" modul toohello "Vyčištění chybějících dat" modulu***

2. Klikněte dvakrát na modul hello a zadejte "Výběr příznaků pro predikci."

2. Klikněte na tlačítko **spustit selektor sloupců** v hello **vlastnosti** podokně.

3. Klikněte na **S pravidly**.

4. V části **Začít s** klikněte na **Žádné sloupce**. V řádku filtru hello, vyberte **zahrnout** a **názvy sloupců** a vyberte náš seznam názvů sloupců hello textového pole. To přesměruje hello modulu toonot předávání žádné sloupce (funkce), s výjimkou těch, které jsou které určíme hello.

5. Klikněte na tlačítko zaškrtnutí (OK) hello.

    ![Vyberte tooinclude hello sloupců (funkce) v předpovědi hello][select-columns-to-include]
    <br/>
    ***Vyberte tooinclude hello sloupců (funkce) v předpovědi hello***

To vytváří filtrovanou sadu dat obsahující pouze funkce hello chceme toopass toohello učení algoritmus, který použijeme v dalším kroku hello. Později se můžete vrátit a zkusit jiný výběr příznaků.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Krok 4: Volba a použití algoritmu učení

Teď, když hello data jsou připravena, tvorba prediktivního modelu sestává z trénování a testování. Model hello tootrain naše data použijeme, a pak otestujeme hello model toosee jak blízko je možné toopredict ceny.
<!-- For now, don't worry about *why* we need tootrain and then test a model.-->

*Klasifikace* a *regrese* jsou dva typy technik strojového učení se supervizí. Klasifikace předpovídá odpověď na základě definované sady kategorií, třeba barvy (červená, modrá nebo zelená). Regrese je použité toopredict číslo.

Vzhledem k tomu, že chceme toopredict ceníku, který je číslo, použijeme regresní algoritmus. Pro tento příklad použijeme jednoduchý model *lineární regrese*.

> [!TIP]
> Pokud chcete více informací o různé typy algoritmů strojového učení toolearn a pokud toouse je první video hello může zobrazit v hello vědecké zpracování dat pro začátečníky řady [hello pět otázky, odpovědi vědecké účely data](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). Může také prohlédnout hello infografice [strojového učení základy s příklady algoritmus](machine-learning-basics-infographic-with-algorithm-examples.md), nebo se podívejte se na hello [strojového učení rychlý přehled algoritmů](machine-learning-algorithm-cheat-sheet.md).

Jsme cvičení modelu hello tím, že se sada dat, která zahrnuje hello ceny. Hello model kontroluje hello dat a vyhledat korelací mezi automobilu na funkce a jeho cenu. Potom otestujeme hello modelu – jsme budete poskytněte sadu funkcí pro automobilů, které jsme se seznámíte s a v tématu Jak zavřít hello modelu dodává toopredicting hello známé ceny.

Naše data použijeme pro trénování modelu hello i testování rozdělením hello data do samostatných trénování a testování datové sady.

1. Vyberte a přetáhněte ji hello [rozdělení dat] [ split] toohello modulu experimentovat plátno a připojte ho poslední toohello [výběr sloupců v datové sadě] [ select-columns] modul.

2. Klikněte na tlačítko hello [rozdělení dat] [ split] modulu tooselect ho. Najde hello **podíl řádků v hello nejprve výstupní datovou sadu** (v hello **vlastnosti** toohello podokně napravo od plátna hello) a nastavte ji too0.75. Tímto způsobem jsme budete používat 75 procent hello data tootrain hello modelu a Ponecháme 25 procent pro testování (později můžete experimentovat s použitím různých procenta).

    ![Sada hello rozdělte podílem too0.75 modul "Rozdělení dat" hello][set-split-data-percentage]
    <br/>
    ***Sada hello rozdělte podílem too0.75 modul "Rozdělení dat" hello***

    > [!TIP]
    > Změnou hello **náhodná počáteční hodnota** parametr, můžete vytvořit různé náhodné vzorky pro trénování a testování. Tento parametr řídí hello synchronizace replik indexů hello pseudonáhodného generátoru čísel.

2. Spusťte hello experiment. Při spuštění experimentu hello hello [výběr sloupců v datové sadě] [ select-columns] a [rozdělení dat] [ split] moduly předat toohello definice sloupce moduly jsme přidali další.  

3. hello tooselect učení algoritmu, rozbalte položku hello **Machine Learning** kategorie v hello modulu palety toohello vlevo hello plátno a potom rozbalte **inicializovat Model**. Zobrazí několik kategorií modulů, které mohou být algoritmů strojového učení použité tooinitialize. Pro tento experiment vyberte hello [lineární regrese] [ linear-regression] modulu v části hello **regrese** kategorii a přetáhněte jej na plátno experimentu toohello.
(Můžete také vyhledat hello modulu zadáním "lineární regrese" do pole Hledat palety hello).

4. Najděte a přetáhněte hello [Train Model] [ train-model] toohello modulu experimentovat plátno. Připojit hello výstup hello [lineární regrese] [ linear-regression] toohello modulu zbývajících vstup hello [Train Model] [ train-model] modul a připojte hello cvičení výstup dat (left port) modulu hello [rozdělení dat] [ split] modulu toohello správný vstup hello [Train Model] [ train-model] modul.

    ![Připojit hello "Train Model" modul tooboth hello "Lineární regrese" a "Rozdělení dat" moduly][connect-train-model]
    <br/>
    ***Připojit hello "Train Model" modul tooboth hello "Lineární regrese" a "Rozdělení dat" moduly***

5. Klikněte na tlačítko hello [Train Model] [ train-model] modulu, klikněte na tlačítko **spustit selektor sloupců** v hello **vlastnosti** podokně a pak vyberte hello **cena** sloupce. Toto je, že je naše model probíhající toopredict hodnota hello.

    Vyberte hello **cena** sloupec v selektor sloupců hello přesunutím z hello **dostupné sloupce** seznamu toohello **vybrané sloupce** seznamu.

    ![Vyberte sloupec hello ceny pro modul "Train Model" hello][select-price-column]
    <br/>
    ***Vyberte sloupec hello ceny pro modul "Train Model" hello***

6. Spusťte hello experiment.

Nyní je k dispozici vyškolení regresní model, který lze použít tooscore nové automobilů data toomake cena předpovědi.

![Po spuštění hello experiment by teď měl vypadat přibližně takto][second-experiment-run]
<br/>
***Po spuštění hello experiment by teď měl vypadat přibližně takto***

## <a name="step-5-predict-new-automobile-prices"></a>Krok 5: Předpověď cen nových automobilů

Teď, když jsme natrénovali hello model pomocí 75 procent dat, abychom je mohli použít tooscore hello zbylých 25 procent dat toosee hello, jak dobře model funguje.

1. Najděte a přetáhněte hello [Score Model] [ score-model] toohello modulu experimentovat plátno. Připojit hello výstup hello [Train Model] [ train-model] toohello modulu zbývajících vstupní port [Score Model][score-model]. Připojit hello výstup testovacích dat (pravý port) z hello [rozdělení dat] [ split] modulu toohello právo vstupní port [Score Model][score-model].

    ![Připojit hello "Určení skóre modelu" modul tooboth hello "Train Model" a "Rozdělení dat" moduly][connect-score-model]
    <br/>
    ***Připojit hello "Určení skóre modelu" modul tooboth hello "Train Model" a "Rozdělení dat" moduly***

2. Spusťte hello experiment a zobrazit výstup hello hello [Score Model] [ score-model] modulu (klikněte na výstupní port hello [Score Model] [ score-model] a vyberte **Vizualizovat**). Zobrazí výstup Hello hello predikované hodnoty ceny a známé hodnoty hello testovací data hello.  

    ![Výstup hello "" modulu pro stanovení skóre][score-model-output]
    <br/>
    ***Výstup hello "" modulu pro stanovení skóre***

3. Nakonec testujeme hello kvality výsledků hello. Vyberte a přetáhněte ji hello [Evaluate Model] [ evaluate-model] toohello modulu experimentovat plátno a připojit hello výstup hello [Score Model] [ score-model] modul toohello zbývajících vstup z [Evaluate Model][evaluate-model].

    > [!TIP]
    > Existují dva vstupní porty na hello [Evaluate Model] [ evaluate-model] modulu může být dva modely použité toocompare vedle sebe. Později, můžete přidat jiný algoritmus toohello experiment a používat [Evaluate Model] [ evaluate-model] toosee, která poskytuje lepší výsledky.

4. Spusťte hello experiment.

výstup hello tooview hello [Evaluate Model] [ evaluate-model] modulu, klikněte na výstupní port hello a pak vyberte **vizualizovat**.

![Výsledky vyhodnocení hello experimentu][evaluation-results]
<br/>
***Výsledky vyhodnocení hello experimentu***

pro náš model se zobrazuje Hello následující statistiky:

- **Střední absolutní chyba** (MAE): průměr absolutních chyb hello ( *chyba* je hello rozdíl mezi hello předpovědět hodnotu a skutečnou hodnotu hello).
- **Odmocnina střední kvadratické chyby** (RMSE): hello druhou odmocninu čísla hello průměr kvadratických chyb předpovědí na hello testovací datové sady.
- **Relativní absolutní chyba**: průměr absolutních chyb relativních toohello absolutnímu rozdílu mezi skutečnými hodnotami a průměrem všech skutečných hodnot hello hello.
- **Relativní kvadratická chyba**: průměr kvadratických chyb relativních toohello hello spolehlivosti rozdíl mezi hello skutečnými hodnotami a průměrem všech skutečných hodnot hello.
- **Koeficient spolehlivosti**: také označované jako hello **hodnota spolehlivosti R**, jedná se tedy statistická metrika označující, jak dobře model vyhovuje hello data.

Pro každou chyby hello je lepší statistiky, menší. Menší hodnota označuje, zda text hello předpověď přesněji odpovídá skutečným hodnotám hello. Pro **koeficientu spolehlivosti**, hello blíže jeho hodnota může být tooone (1.0), hello lepší hello předpovědi.

## <a name="final-experiment"></a>Konečný experiment

Hello konečný experiment by měl vypadat přibližně takto:

![Konečný experiment Hello][complete-linear-regression-experiment]
<br/>
***Konečný experiment Hello***

## <a name="next-steps"></a>Další kroky

Teď, když jste dokončili hello první kurz strojového učení a máte nastaven experiment, můžete pokračovat tooimprove hello modelu a poté ji nasadit jako prediktivní webovou službu.

- **Iteraci modelu hello tooimprove tootry** – například můžete změnit hello funkcích, které používáte ve vaší předpovědi. Nebo můžete upravit vlastnosti hello hello [lineární regrese] [ linear-regression] algoritmus nebo se pokuste úplně jiný algoritmus. I můžete přidat více experimentu strojového učení a algoritmy tooyour najednou a porovnat je pomocí hello [Evaluate Model] [ evaluate-model] modulu.
Příklad jak toocompare více modelů v jednom experimentu, najdete v části [porovnat Regresory](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) v hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).

    > [!TIP]
    > toocopy kteroukoli iteraci experimentu, použijte hello **uložit jako** tlačítko v hello dolní části stránky hello. Zobrazí všechny hello iterace experimentu kliknutím **zobrazit HISTORII BĚHŮ** v hello dolní části stránky hello. Další podrobnosti najdete v tématu [Správa iterací experimentů v nástroji Azure Machine Learning Studio][runhistory].

[runhistory]: machine-learning-manage-experiment-iterations.md

- **Nasazení modelu hello jako prediktivní webovou službu** – Pokud jste se svým modelem spokojeni můžete nasadit jako toobe webové služby použít cen automobilů toopredict pomocí nová data. Další podrobnosti najdete v tématu [Nasazení webové služby Azure Machine Learning][publish].

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Chcete toolearn další? Rozsáhlejší a podrobnější návod hello procesu vytváření, trénování, vyhodnocování a nasazení modelu najdete v tématu [vývoji prediktivního řešení pomocí Azure Machine Learning][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[sign-in-to-studio]: ./media/machine-learning-create-experiment/sign-in-to-studio.png
[rename-experiment]: ./media/machine-learning-create-experiment/rename-experiment.png
[visualize-auto-data]:./media/machine-learning-create-experiment/visualize-auto-data.png
[select-visualize]: ./media/machine-learning-create-experiment/select-visualize.png
[showing-excluded-column]:./media/machine-learning-create-experiment/showing-excluded-column.png
[set-remove-entire-row]:./media/machine-learning-create-experiment/set-remove-entire-row.png
[early-experiment-run]:./media/machine-learning-create-experiment/early-experiment-run.png
[select-columns-to-include]:./media/machine-learning-create-experiment/select-columns-to-include.png
[second-experiment-run]:./media/machine-learning-create-experiment/second-experiment-run.png
[connect-score-model]:./media/machine-learning-create-experiment/connect-score-model.png
[evaluation-results]:./media/machine-learning-create-experiment/evaluation-results.png
[complete-linear-regression-experiment]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
[type-automobile]:./media/machine-learning-create-experiment/type-automobile.png
[type-select-columns]:./media/machine-learning-create-experiment/type-select-columns.png
[launch-column-selector]:./media/machine-learning-create-experiment/launch-column-selector.png
[add-comment]:./media/machine-learning-create-experiment/add-comment.png
[connect-clean-to-select]:./media/machine-learning-create-experiment/connect-clean-to-select.png

[set-split-data-percentage]:./media/machine-learning-create-experiment/set-split-data-percentage.png

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
[connect-train-model]:./media/machine-learning-create-experiment/connect-train-model.png
[select-price-column]:./media/machine-learning-create-experiment/select-price-column.png

[score-model-output]:./media/machine-learning-create-experiment/score-model-output.png

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
