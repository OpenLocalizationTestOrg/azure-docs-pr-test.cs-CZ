---
title: "výkon modelu aaaEvaluate v Machine Learning | Microsoft Docs"
description: "Vysvětluje, jak tooevaluate modelu výkon v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 5dc5348a-4488-4536-99eb-ff105be9b160
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: bradsev;garye
ms.openlocfilehash: 03477368758dbb13aa6f54c5d27fb215615d1f9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooevaluate-model-performance-in-azure-machine-learning"></a>Jak tooevaluate modelu výkon v Azure Machine Learning
Tento článek ukazuje, jak tooevaluate hello výkonu modelu v Azure Machine Learning Studio a nabízí stručné vysvětlení hello metriky, které jsou k dispozici pro tuto úlohu. Tři běžné scénáře pod dohledem learning uvádíme: 

* Regrese
* binární klasifikace 
* více třídami klasifikace

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Vyhodnocení výkonu hello modelu je jedním z fází základní hello v procesu vědecké účely dat hello. Označuje, jak úspěšné hello vyhodnocování (předpovědi) datové sady byl pomocí modulu trained model. 

Azure Machine Learning podporuje model vyhodnocení prostřednictvím dvou jeho hlavní strojového učení moduly: [Evaluate Model] [ evaluate-model] a [křížové ověření modelu][cross-validate-model]. Tyto moduly vám umožňují toosee jak modelu provádí z hlediska počtu metriky, které běžně se používají v machine learning a statistiky.

## <a name="evaluation-vs-cross-validation"></a>Vyhodnocení vs. Křížové ověření
Vyhodnocení a křížového ověření jsou standardní způsoby toomeasure hello výkonu modelu. Obě vygenerovat zkušební metriky, které můžete zkontrolovat nebo porovnán těch, které ostatní modely.

[Vyhodnocení modelu] [ evaluate-model] očekává scored datovou sadu jako vstup (nebo 2 v případě byste chtěli toocompare hello výkon 2 odlišnými modely). To znamená, že budete potřebovat tootrain model pomocí hello [Train Model] [ train-model] předpovědi modul a zkontrolujte u některých datové sady pomocí hello [Score Model] [ score-model] modulu, než je možné vyhodnotit hello výsledky. Hello hodnocení je založeno na hello skóre pro magnitudu popisky nebo pravděpodobnostech společně s hello true štítků, které jsou výstupem hello [Score Model] [ score-model] modulu.

Alternativně můžete použít křížového ověření tooperform číslo train – score vyhodnotit operací (10 složení) automaticky na jinou podmnožinou vstupních dat hello. vstupní data Hello je rozdělená do 10 částí, kde jeden je vyhrazená pro testování a hello jiných 9 pro školení. Tento proces se opakuje 10krát a průměr se hello vyhodnocení metriky. To pomáhá při určení, jak dobře model by generalize toonew datové sady. Hello [křížové ověření modelu] [ cross-validate-model] modul přijímá ve model nevyškoleným a některé označené datovou sadu a výstupy výsledky hodnocení hello jednotlivých hello 10 složení, kromě toohello průměrem výsledky.

V následujících částech hello, jsme sestavení jednoduché regrese a klasifikace modely a vyhodnotit jejich výkonu pomocí obou hello [Evaluate Model] [ evaluate-model] a hello [křížové ověření Model] [ cross-validate-model] moduly.

## <a name="evaluating-a-regression-model"></a>Vyhodnocení regresní Model
Předpokládejme, že chceme toopredict ceny automobilu na použití některých funkcí, jako je například dimenze, koňských sil, modul specifikací a tak dále. Tento problém typické regrese je, kde hello Cílová proměnná (*cena*) je nepřetržitá číselná hodnota. Jsme můžete začlenit model jednoduché lineární regrese, že dané funkce hello hodnoty určité automobilu, můžete předpovědi hello cena této Auto. Můžete použít tento regresní model tooscore hello jsme natrénovali na stejnou datovou. Jakmile budeme mít hello předpokládaných ceny pro všechny hello aut, jsme vyhodnotit hello výkonu hello modelu prohlížením kolik hello předpovědi odchylují od skutečné ceny hello v průměru. tooillustrate toho používáme hello *Automobile price data (Raw) datovou sadu* k dispozici v hello **uložit datové sady** část v Azure Machine Learning Studio.

### <a name="creating-hello-experiment"></a>Vytváření hello experimentu
Přidejte následující moduly tooyour prostoru v nástroji Azure Machine Learning Studio hello:

* Automobilů price data (Raw)
* [Lineární regrese][linear-regression]
* [Train Model][train-model]
* [Určení skóre modelu][score-model]
* [Vyhodnocení modelu][evaluate-model]

Připojte hello porty, jak je znázorněno níže v obrázku 1 a sadu hello popisek sloupec hello [Train Model] [ train-model] modulu příliš*cena*.

![Vyhodnocení regresní Model](media/machine-learning-evaluate-model-performance/1.png)

Obrázek 1. Vyhodnocení regresní Model.

### <a name="inspecting-hello-evaluation-results"></a>Probíhá kontrola výsledků vyhodnocení hello
Po spuštěné hello experiment, můžete kliknutím na výstupní port hello hello [Evaluate Model] [ evaluate-model] modul a vyberte *vizualizovat* výsledky hodnocení toosee hello. Hello vyhodnocení metriky, které jsou k dispozici pro regresní modely jsou: *znamenat absolutní chyba*, *kořenové znamenat absolutní chyba*, *relativní absolutní chyba*,  *Relativní kvadratická chyba*a hello *koeficientu spolehlivosti*.

hello Hello termín "Chyba" představuje hello rozdíl mezi předpovězenou a skutečnou hodnotou hello. Hello absolutní hodnotu nebo hello odmocnina tohoto rozdílu jsou obvykle počítaný toocapture hello celkový odhad chyba napříč všemi instancemi jako hello rozdíl mezi hello předpovědět a může být v některých případech záporná hodnota true. Hello chyba metriky měření hello prediktivní výkon z hlediska hello střední odchylku jeho předpovědi z hodnot true hello regresní model. Nižší chybové hodnoty znamená, že je hello model přesnější při provádění předpovědi. Obecné chybě metrika 0 znamená, že hello modelu perfektně vyhovuje hello data.

Hello koeficient spolehlivosti, která je také označována jako R spolehlivosti, je také standardní způsob měření, jak dobře hello model vyhovuje hello data. Můžete se interpretuje jako část hello variace vysvětlené hello modelu. A vyšší v tomto případě je lepší míry, kde 1 určuje vzájemně přizpůsobit.

![Lineární regrese vyhodnocení metriky](media/machine-learning-evaluate-model-performance/2.png)

Obrázek 2. Lineární regrese vyhodnocení metriky.

### <a name="using-cross-validation"></a>Pomocí křížové ověření
Jak už bylo zmíněno dříve, můžete provést opakovaný školení, vyhodnocování a vyhodnocení automaticky pomocí hello [křížové ověření modelu] [ cross-validate-model] modulu. V takovém případě stačí je datová sada, model nevyškoleným a [křížové ověření modelu] [ cross-validate-model] modulu (viz následující obrázek). Všimněte si, je nutné tooset hello popisek sloupce příliš*cena* v hello [křížové ověření modelu] [ cross-validate-model] vlastnosti modulu.

![Mezi ověřování regresní Model](media/machine-learning-evaluate-model-performance/3.png)

Obrázek 3. Mezi ověřování regresní Model.

Po spuštěné hello experimentu si můžete prohlédnout výsledky hodnocení hello kliknutím na hello správné výstupní port modulu hello [křížové ověření modelu] [ cross-validate-model] modulu. To poskytne podrobné zobrazení hello metriky pro každé iteraci (násobek) a hello průměrem výsledky jednotlivých hello metriky (obrázek 4).

![Křížové ověření výsledky regresní Model](media/machine-learning-evaluate-model-performance/4.png)

Obrázek 4. Křížové ověření výsledky regresní Model.

## <a name="evaluating-a-binary-classification-model"></a>Vyhodnocení modelu binární klasifikace
Ve scénáři binární klasifikace hello Cílová proměnná má jenom dvě možné výsledky, například: {0, 1} nebo {false, true}, {záporná, kladné}. Předpokládejme, jsou uvedeny datové sady pro dospělé zaměstnancům některé demografické údaje a jejich zaměstnání proměnné a úroveň příjmů toopredict hello, binární proměnné s hodnotami hello se zobrazí výzva {"< = 50 tisíc", "> 50 tisíc"}. Jinými slovy, hello záporné třída reprezentuje hello zaměstnanci, kteří žádají menší než nebo rovna too50K hello kladné třída reprezentuje všechny ostatní zaměstnanci a roce. Jako hello regrese scénáři jsme by trénování modelu, stanovení skóre některá data a vyhodnoťte výsledky hello. Hello zde hlavní rozdíl je volba hello metriky, které se vypočítá Azure Machine Learning a výstupy. tooillustrate hello příjem úrovně předpovědi scénáři budeme používat hello [pro dospělé](http://archive.ics.uci.edu/ml/datasets/Adult) toocreate datové sady Azure Machine Learning experiment a vyhodnocovat u nich hello výkon two-class logistic regresní model, binární běžně používané Klasifikátor.

### <a name="creating-hello-experiment"></a>Vytváření hello experimentu
Přidejte následující moduly tooyour prostoru v nástroji Azure Machine Learning Studio hello:

* Datové sady pro dospělé úplné zjišťování příjem binární klasifikace
* [Two-Class Logistic Regression][two-class-logistic-regression]
* [Train Model][train-model]
* [Určení skóre modelu][score-model]
* [Vyhodnocení modelu][evaluate-model]

Připojte hello porty, jak je znázorněno níže obrázek 5 a sadu hello popisek sloupce hello [Train Model] [ train-model] modulu příliš*příjem*.

![Vyhodnocení modelu binární klasifikace](media/machine-learning-evaluate-model-performance/5.png)

Obrázek 5. Vyhodnocení modelu binární klasifikace.

### <a name="inspecting-hello-evaluation-results"></a>Probíhá kontrola výsledků vyhodnocení hello
Po spuštěné hello experiment, můžete kliknutím na výstupní port hello hello [Evaluate Model] [ evaluate-model] modul a vyberte *vizualizovat* výsledky hodnocení hello toosee (obrázek 7). Hello vyhodnocení metriky, které jsou k dispozici pro binární klasifikaci modely jsou: *přesnost*, *přesnost*, *odvolat*, *F1 skóre*, a *AUC*. Kromě toho modul hello výstupy nedorozuměním matice zobrazující počet hello true pozitivních, chybná hlášení, falešně pozitivních a true negativy, a také *ROC*, *přesnost nebo odvolání*a *Navýšení* křivek.

Přesnost je jednoduše hello podíl správně klasifikovaný instancí. Je obvykle hello první metriku, které si prohlédnete při vyhodnocování třídění. Když hello testovací data je však nevyvážené (kde většinu instancí hello patří tooone třídy hello), nebo vás zajímá více hello výkonu na jednu z tříd hello, přesnost není skutečně zaznamenat hello účinnosti třídění. V případě úrovně klasifikace příjem hello předpokládá testujete na některá data, kde % 99 instance hello představovat osobu, která vám menší než nebo rovna too50K za jeden rok. Je možné tooachieve 0.99 přesnost Odhadnutím toho, jaká třída hello "< = 50 tisíc" pro všechny instance. Hello třídění v takovém případě se zobrazí toobe provádění celkový dobrý úlohy, ale ve skutečnosti se nezdaří tooclassify žádné hello vysokými příjmy jednotlivce (hello 1 %) správně.

Z tohoto důvodu je užitečné toocompute další metriky, které zaznamenat více konkrétních aspektů hello vyhodnocení. Před přechodem do hello podrobnosti o tyto metriky, je důležité toounderstand hello nedorozuměním matice vyhodnocení binární klasifikace. Třída Hello, popisky v sadě hello školení může trvat na pouze 2 možné hodnoty, které jsme obvykle naleznete tooas kladné a záporné. Hello kladné a záporné instancí, které třídění předpovídá správně se nazývají true pozitivních (TP) a true negativy (TN). Podobně hello nesprávně klasifikované instancí se nazývají falešně pozitivních (FP) a výsledkům (FN). matice nedorozuměním Hello je jednoduše tabulka zobrazující hello počet instancí, které spadají pod každé z těchto 4 kategorií. Azure Machine Learning automaticky rozhoduje, který hello dvě třídy v datové sadě hello je kladné třída hello. Pokud hello třída popisky jsou logická hodnota nebo celá čísla, hello 'true' nebo '1' s popiskem instancí přiřazené kladné třída hello. Pokud hello popisky jsou řetězce, jako v případě hello sady hello příjem dat jsou popisky hello abecedním a první úroveň hello je zvolen toobe hello záporné třída při druhou úroveň hello je kladné třída hello.

![Matice nedorozuměním binární klasifikace](media/machine-learning-evaluate-model-performance/6a.png)

Obrázek 6. Binární klasifikační matice nejasnostem.

Návratem toohello příjem klasifikace problému, by chceme tooask několik otázek hodnocení, které nám pomůže pochopit výkon hello třídění hello používá. Je velmi přirozené dotaz: ' mimo hello jednotlivce, kterým hello modelu předpokládaných toobe získávat > 50 tisíc (transakční program + FP), kolik byly správnou klasifikaci (TP)? " Tento dotaz může odpovědi prohlížením hello **přesnost** hello modelu, který je hello podíl položky, které jsou klasifikovány správně: TP/(TP+FP). Další běžné otázky je "mimo všechny hello vysoké zaměstnance earning s příjmem > 50 tisíc (transakční program + FN), kolik hello třídění klasifikovat správně (TP)". Toto je ve skutečnosti hello **odvolat**, nebo hello true kladné rychlost: TP/(TP+FN) třídění hello. Můžete si všimnout, že je zřejmé kompromis mezi přesnost a odvolání. Například zadány relativně vyrovnáváním datovou sadu, třídění, který bude předpovídat většinou kladné instancí, by měla mít vysokou odvolání, ale spíš nízkou přesnost libovolný počet instancí záporné hello by misclassified výsledkem velký počet falešně pozitivních zjištění. toosee vykreslení o tom, jak se tyto dvě metriky liší, můžete kliknutím na křivku 'Přesnost nebo odvolání' hello v stránku výstup výsledků vyhodnocení hello (nahoře vlevo součástí obrázek 7).

![Výsledky vyhodnocení binární klasifikace](media/machine-learning-evaluate-model-performance/7.png)

Na obrázku 7. Výsledky vyhodnocení binární klasifikace.

Další související metriku, která se často používá je hello **F1 skóre**, což trvá přesnost i pro vyvolání v úvahu. Je hello průměr těchto 2 metrik a jako takový je počítaný: F1 = 2 (přesnost x odvolání) / (přesnost + odvolání). skóre F1 Hello je k testování dobře toosummarize hello v jediné číslo, ale vždycky je dobrým zvykem toolook na přesnost i pro vyvolání společně toobetter pochopit, jak se chová třídění.

Kromě toho jeden můžete prohlédnout true kladné míra hello oproti false kladné rychlost hello v hello **příjemce operační vlastnosti (ROC)** křivky a hello odpovídající **oblasti pod hello křivky (AUC)** hodnotu. Hello blíže tento křivka je toohello horní levého horního rohu, hello lepší hello třídění na výkon (který je maximální využití hello true kladné rychlost a současně minimalizujete její hello false kladné rychlost). Křivek, které jsou zavřít toohello diagonálních z hello výkresu, výsledek z třídění, které jsou obvykle toomake předpovědi, které jsou zavřete uhodnutí toorandom.

### <a name="using-cross-validation"></a>Pomocí křížové ověření
Jako v příkladu regrese hello jsme provedení křížového ověření toorepeatedly train, stanovení skóre a vyhodnotit různé podmnožiny dat hello automaticky. Podobně, můžeme použít hello [křížové ověření modelu] [ cross-validate-model] modulu, model nevyškoleným logistic regression a datové sady. Hello popisek sloupce musí být nastaven příliš*příjem* v hello [křížové ověření modelu] [ cross-validate-model] vlastnosti modulu. Po spuštění experimentu hello a kliknete na pravém hello výstupní port hello [křížové ověření modelu] [ cross-validate-model] modulu, uvidíme hello binární klasifikace metrika hodnoty pro každý násobek kromě toohello střední a směrodatné odchylky jednotlivých. 

![Křížové ověření modelu binární klasifikace](media/machine-learning-evaluate-model-performance/8.png)

Obrázek 8. Mezi ověřování modelu binární klasifikace.

![Křížové ověření výsledky binární třídění](media/machine-learning-evaluate-model-performance/9.png)

Obrázek 9. Křížové ověření výsledky binární třídění.

## <a name="evaluating-a-multiclass-classification-model"></a>Vyhodnocení modelu více třídami klasifikace
V této experimentu použijeme hello oblíbených [Iris](http://archive.ics.uci.edu/ml/datasets/Iris "Iris") datové sady, který obsahuje instance 3 různé typy (třídy) hello iris závodu. Existují 4 hodnot funkce (sepal délky a šířky a délky a šířky Okvětní lístek) pro každou instanci. V předchozích experimenty hello jsme natrénovali a modely otestované hello pomocí hello stejné datové sady. Zde použijeme hello [rozdělení dat] [ split] modulu toocreate 2 podmnožiny dat hello cvičení na hello nejprve a stanovení skóre a vyhodnocení na hello druhý. Hello Iris datová sada veřejně dostupné v hello [UCI Machine Learning úložiště](http://archive.ics.uci.edu/ml/index.html)a můžete je stáhnout pomocí [importovat Data] [ import-data] modulu.

### <a name="creating-hello-experiment"></a>Vytváření hello experimentu
Přidejte následující moduly tooyour prostoru v nástroji Azure Machine Learning Studio hello:

* [Umožňuje importovat Data][import-data]
* [Doménové struktury rozhodnutí multiclass][multiclass-decision-forest]
* [Rozdělení dat][split]
* [Train Model][train-model]
* [Určení skóre modelu][score-model]
* [Vyhodnocení modelu][evaluate-model]

Připojte hello porty, jak je znázorněno níže na obrázku 10.

Nastavit hello popisek sloupcový index hello [Train Model] [ train-model] too5 modulu. datovou sadu Hello je žádný řádek záhlaví, ale víme hello třídy, které jsou popisky ve sloupci pátý hello.

Klikněte na hello [importovat Data] [ import-data] modulu a sadu hello *zdroj dat* vlastnost příliš*adresa URL webové prostřednictvím protokolu HTTP*a hello *adresy URL*  toohttp://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data.

Sada hello podíl toobe instance použitý pro školení v hello [rozdělení dat] [ split] modulu (0,7 např.).

![Vyhodnocení více třídami třídění](media/machine-learning-evaluate-model-performance/10.png)

Obrázek 10. Vyhodnocení více třídami třídění

### <a name="inspecting-hello-evaluation-results"></a>Probíhá kontrola výsledků vyhodnocení hello
Spusťte hello experiment a klikněte na výstupní port hello [Evaluate Model][evaluate-model]. výsledky vyhodnocení Hello uvádíme v hello formuláře matice nejasnostem, v tomto případě. v matici Hello zobrazí hello skutečnost a předpokládaných instancí pro všechny 3 třídy.

![Výsledky vyhodnocení více třídami klasifikace](media/machine-learning-evaluate-model-performance/11.png)

Obrázek 11. Výsledky vyhodnocení více třídami klasifikace.

### <a name="using-cross-validation"></a>Pomocí křížové ověření
Jak už bylo zmíněno dříve, můžete provést opakovaný školení, vyhodnocování a vyhodnocení automaticky pomocí hello [křížové ověření modelu] [ cross-validate-model] modulu. Potřebovali byste datovou sadu, model nevyškoleným a [křížové ověření modelu] [ cross-validate-model] modulu (viz následující obrázek). Znovu potřebujete tooset hello popisek sloupec hello [křížové ověření modelu] [ cross-validate-model] modulu (index sloupce 5 v tomto případě). Po spuštění experimentu hello a kliknutím na pravé hello výstupní port hello [křížové ověření modelu][cross-validate-model], si můžete prohlédnout hello metriky hodnoty pro každý fold a také text hello, střední a směrodatné odchylky. metriky Hello zobrazí tady jsou podobné toohello hello ty, které jsou popsané v případě binární klasifikace hello. Ale Všimněte si, že v více třídami klasifikaci, výpočetních negativy true pozitivních hello a chybná pozitivních nebo hlášení se provádí počítání na základě na třídu, protože neexistuje žádná třída celkové kladné a záporné. Například při výpočetní hello přesnost nebo odvolání hello 'Iris-setosa' třídy, se předpokládá, že toto je hello kladné třídy a všechny ostatní jako záporné.

![Křížové ověření modelu více třídami klasifikace](media/machine-learning-evaluate-model-performance/12.png)

Obrázek 12. Mezi ověřování modelu více třídami klasifikace.

![Výsledků křížového ověřování modelu více třídami klasifikace](media/machine-learning-evaluate-model-performance/13.png)

Obrázek 13. Výsledků křížového ověřování modelu více třídami klasifikace.

<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/

