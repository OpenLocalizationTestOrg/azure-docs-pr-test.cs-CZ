---
title: "aaaExecute Pythonových skriptů strojového učení | Microsoft Docs"
description: "Obsahuje přehled návrhu zásadami pro Podpora skriptů Python v Azure Machine Learning a základní informace o využití scénáře, možnosti a omezení."
keywords: "Python machine learningu pandas, python pandas, skriptů python, spuštění skriptů python"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ee9eb764-0d3e-4104-a797-19fc29345d39
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bradsev
ms.openlocfilehash: 8d23aaa972a46cb1a07ea0f18cc1e24933fe3e6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Spouštění skriptů strojového učení v Pythonu v nástroji Azure Machine Learning Studio

Toto téma popisuje zásady designu hello základní hello aktuální podpora skriptů Python v Azure Machine Learning. Hello zadaný hlavní funkce jsou také uvedeny, včetně:

- spuštění scénáře základní informace o využití
- stanovení skóre experimentu ve webové službě
- Podpora pro import existujícího kódu
- Export vizualizace
- proveďte výběr pod dohledem funkce
- pochopení určitá omezení

[Python](https://www.python.org/) je nepostradatelným prostředkem hello nástroj hrudník mnoho datových vědců. Obsahuje:

* Elegantní a stručným syntaxe, 
* Podpora více platforem 
* velká kolekce výkonné knihovny, a 
* Vyspělá vývojové nástroje. 

Python se používá ve všech fázích obvykle používaných v machine learning modelování pracovního postupu:

- ingestování dat a zpracování 
- Funkce vytváření
- model školení 
- ověření modelu
- nasazení modelů hello

Azure Machine Learning Studio podporuje vnoření skriptů Python do různých částí počítače učení experiment a také bezproblémově publikováním jako webové služby v Microsoft Azure.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Principy návrhu skriptů Python v Machine Learning

Hello tooPython primární rozhraní v nástroji Azure Machine Learning Studio je prostřednictvím hello [Execute Python Script] [ execute-python-script] modulu na obrázku 1.

![image1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

Obrázek 1. Hello **Execute Python Script** modulu.

Hello [Execute Python Script] [ execute-python-script] modulu v Azure ML Studio přijímá až toothree vstupy a vytvoří si tootwo výstupy (popsané v následující části hello), jako jeho analogovým R hello [ Spuštění skriptu R] [ execute-r-script] modulu. Hello toobe kód Python provést zadání do pole parametr hello jako speciálně pojmenovanou vstupní bod volaná funkce `azureml_main`. Zde jsou hello klíče návrhu zásady použít tooimplement tento modul:

1. *Musí být idiomatickou pro Python uživatele.* Většina uživatelů Python dvoufaktorového svůj kód jako funkce uvnitř modulů. Proto uvedení spoustu spustitelné příkazy v modulu nejvyšší úrovně je relativně vzácné. V důsledku toho hello skriptu zadejte také vezme speciálně pojmenovanou funkce Python jako názvem na rozdíl od toojust posloupnost příkazy. Hello objekty v hello funkce jsou typy standardní knihovny jazyka Python, jako [Pandas](http://pandas.pydata.org/) datových rámců a [NumPy](http://www.numpy.org/) pole.
2. *Musí mít zachováním mezi místním a cloudovým spuštěních.* Hello hello tooexecute back-end použít kód Python je založena na [Anaconda](https://store.continuum.io/cshop/anaconda/), nejběžněji používané scientific distribuci jazyka Python a platformy. Součástí zavřít too200 hello nejběžnější balíčků Python. Proto můžete datových vědců ladění a kvalifikaci svůj kód na jejich místním prostředí Azure Machine Learning kompatibilní Anaconda. Použít existující vývojové prostředí, například [IPython](http://ipython.org/) poznámkového bloku nebo [Python Tools pro Visual Studio](http://aka.ms/ptvs), toorun ji jako součást Azure ML experimentu. Hello `azureml_main` vstupní bod je vanilla Python funkce a tak *** bez kódu pro konkrétní Azure ML se můžou vytvořit nebo hello SDK nainstalována.
3. *Musí být bezproblémově složení s další moduly Azure Machine Learning.* Hello [Execute Python Script] [ execute-python-script] modul přijímá jako vstupy a výstupy, standardní datové sady Azure Machine Learning. základní architektury Hello transparentně a efektivně přemosťuje moduly runtime hello Azure ML a Python. Proto Python můžete použít ve spojení s stávajících pracovními postupy Azure ML, včetně těch, které volají do R a SQLite. Ve výsledku vědecký pracovník data tvoří pracovních který:
   * použití jazyka Python a Pandas pro data předem zpracování a čištění
   * informačního kanálu hello tooa SQL transformaci dat, připojení více funkcí tooform datové sady
   * cvičení modelů pomocí hello algoritmy v Azure Machine Learning 
   * vyhodnocení a po zpracování výsledků hello pomocí R.


## <a name="basic-usage-scenarios-in-ml-for-python-scripts"></a>Základní informace o využití scénáře v ML skriptů Python

V této části jsme některé základní použití hello hello zjišťování [Execute Python Script] [ execute-python-script] modulu. Modul Python toohello vstupy jsou zveřejněné jako Pandas datové rámce. Hello funkce musí vracet jeden snímek dat Pandas zabalené uvnitř Python [pořadí](https://docs.python.org/2/c-api/sequence.html) například řazené kolekce členů, seznamu nebo NumPy pole. první prvek Hello toto pořadí je pak vrácen v hello první výstupní port modulu hello. Toto schéma je znázorněno na obrázku 2.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

Obrázek 2. Mapování vstupních portů tooparameters a vrátí hodnotu toooutput portu.

Podrobnější sémantiku jak vstupních portů hello získat namapované tooparameters hello `azureml_main` funkce jsou uvedené v tabulce 1:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tabulka 1. Mapování parametrů toofunction vstupních portů.

je poziční Hello mapování mezi vstupních portů a parametry funkce:

- Hello první připojené vstupní port je namapované toohello první parametr funkce hello. 
- druhý vstup Hello (Pokud je připojeno) je namapované toohello druhý parametr funkce hello.

V tématu *Python pro analýzu dat* (O'Reilly, 2012) podle západní McKinney pro další informace o Python Pandas a na tom, jak může být dat použité toomanipulate efektivnější. 


## <a name="translation-of-input-and-output-types"></a>Překlad vstupní a výstupní typů 
Vstupní datové sady v Azure ML jsou snímky převedený toodata v Pandas. Výstupní datové rámce se převedou back tooAzure ML datové sady. jsou prováděny následující převody Hello:

1. Řetězec a číselné sloupce se převedou jako-je a jsou převedené too'NA chybějící hodnoty v datové sadě ' hodnoty v Pandas. Hello stejné dojde k převodu na zpět způsobem hello (NA hodnoty v Pandas jsou převedené toomissing hodnoty v Azure ML).
2. Index vektorů v Pandas nejsou podporovány v Azure ML. Všechny snímky vstupní data v hello Python funkce mít vždy 64-bit číselné index z 0 toohello počet řádků minus 1. 
3. Datové sady Azure ML nemůže mít duplicitní názvy sloupců a názvy sloupců, které nejsou typu řetězec. Pokud již výstupní data rámečku obsahuje jiné než číselné sloupce, hello framework volání `str` na názvy sloupců hello. Stejně tak všechny duplicitní názvy sloupců jsou automaticky pozměnění tooinsure hello názvy jsou jedinečné. přidá přípona Hello (2) toohello první duplicitní, (3) druhý duplicitní toohello a tak dále.


## <a name="operationalizing-python-scripts"></a>Zprovozňování skriptů Python

Všechny [Execute Python Script] [ execute-python-script] moduly používané v vyhodnocování experimentu jsou volány při publikovat jako webovou službu. Obrázek 3 ukazuje například vyhodnocování experimentu, který obsahuje hello kódu tooevaluate jeden výraz Python. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

Obrázek 3. Webové služby pro vyhodnocení výrazu jazyka Python.

Webové služby vytvořené z tohoto experimentu:

- přijímá jako vstup výraz Python (jako řetězec)
- odešle ji překladač Pythonu toohello 
- Vrátí tabulku obsahující hello výraz a hello vyhodnotit výsledek.


## <a name="importing-existing-python-script-modules"></a>Import existující moduly skriptu jazyka Python

Běžné případ použití pro mnoho datových vědců je tooincorporate existující skriptů Python do Azure ML experimenty. Místo aby se, že veškerý kód zřetězených a vložit do jednoho skriptu pole, hello [Execute Python Script] [ execute-python-script] modul přijímá soubor zip, který obsahuje moduly jazyka Python na třetí vstupní port hello. soubor Hello je rozbalené rámcem hello provádění za běhu a hello obsahu se přidají cesta knihovny toohello překladač Pythonu hello. Hello `azureml_main` vstupní bod funkce můžete poté importovat tyto moduly přímo.

Jako příklad zvažte hello soubor Hello.py obsahující jednoduché funkci "Hello, World".

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

Obrázek 4. Uživatelem definované funkce v souboru Hello.py.

V dalším kroku vytvoříme soubor Hello.zip, který obsahuje Hello.py:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

Obrázek 5. Soubor ZIP obsahující uživatelem definované kód Python.

Nahrajte soubor zip hello jako datové sady do Azure Machine Learning Studio. Pak vytvořte a spusťte experimentu, který používá hello Python kód v souboru Hello.zip hello tak, že ho připojíte toohello třetí vstupní port hello **Execute Python Script** modulu, jak je vidět na tomto obrázku.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

Obrázek 6. Ukázkový experiment s uživatelem definovaný kód Python uloženo jako soubor zip.

Hello modulu výstup ukazuje, že soubor zip hello nezabalené a že fungují hello `print_hello` byl spuštěn.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)

Na obrázku 7. Uživatelem definované funkce používá uvnitř hello [Execute Python Script] [ execute-python-script] modulu.


## <a name="working-with-visualizations"></a>Práce s vizualizacemi

Pozemků vytvořený MatplotLib, který můžete vizualizovat v prohlížeči hello se může vracet hello [Execute Python Script][execute-python-script]. Ale hello pozemků nejsou automaticky přesměrovaného tooimages jako při použití R. Takže hello uživatel musí explicitně uložit všechny pozemků tooPNG soubory Pokud jsou toobe vrátil zpět tooAzure Machine Learning. 

toogenerate obrázky z MatplotLib, musíte provést následující postup hello:

* přepínač back-end hello příliš "%{AGG/" z hello výchozí na základě rt zobrazovací jednotky 
* Vytvořit nový objekt obrázku 
* získat hello osy a generovat všechny pozemků do ní 
* Uložte soubor PNG tooa obrázek hello 

Tento proces je znázorněn v následujícím obrázek 8, která vytvoří matice bodové vykreslení pomocí funkce scatter_matrix hello v Pandas hello.

![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

Obrázek 8. Kód toosave, které MatplotLib obrázky tooimages.

Obrázek 9 ukazuje experimentu používá skript hello uvedený výše, že tooreturn ukazuje zeměpisný prostřednictvím hello druhý výstupní port.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

Obrázek 9. Vizualizace pozemků generované z kódu jazyka Python.

Je možné tooreturn více obrázků jejich uložením do různých obrázků, hello Azure Machine Learning runtime příjmem všechny bitové kopie a je zřetězí pro vizualizaci.


## <a name="advanced-examples"></a>Pokročilé příklady

Hello Anaconda prostředí nainstalován v Azure Machine Learning obsahuje běžné balíčků, jako jsou NumPy, SciPy a další Scikits. Tyto balíčky můžete efektivně použít pro různé úlohy zpracování dat v machine learning kanálu. Jako příklad hello následující experiment a skript ilustrují použití hello inteligentních algoritmů komplet v další Scikits toocompute funkce důležitosti skóre pro datovou sadu. Výběr použité tooperform pod dohledem funkce před se předány do jiného modelu ML může být Hello skóre.

Tady je hello Python funkce použitá toocompute hello důležitosti skóre a funkce hello pořadí podle skóre hello:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

Obrázek 10. Funkce funkce toorank podle skóre.
 Hello následující experimentu pak vypočítá a vrátí hello důležitosti skóre funkcí v datové sadě "Pima indickém Diabetes" hello v Azure Machine Learning:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    

Obrázek 11. Funkce toorank experimentu v datové sadě Pima indickém Diabetes hello.

## <a name="limitations"></a>Omezení
Hello [Execute Python Script] [ execute-python-script] má v současné době hello následující omezení:

1. *V izolovaném prostoru provádění.* modul Python runtime Hello je aktuálně v izolovaném prostoru a v důsledku toho neumožňuje přístup toohello sítě nebo toohello místního systému souborů trvalé způsobem. Všechny soubory, které jsou uloženy v místním počítači jsou izolované a odstranit po dokončení modulu hello. Hello kód Python nemůže získat přístup k většině adresářů v počítači hello, na kterém běží na, se výjimka hello hello aktuálního adresáře a jejích podadresářů.
2. *Chybí sofistikované vývoji a ladění podpory.* modul Python Hello aktuálně nepodporuje IDE funkce jako je například technologie intellisense a ladění. Také v případě selhání modulu hello za běhu hello úplné trasování zásobníku Python je k dispozici. Je však nutné ji zobrazit v protokolu výstup hello hello modulu. Doporučujeme aktuálně vyvíjet a ladit Python skripty v prostředí, jako je například IPython a pak importovat hello kódu do modulu hello.
3. *Single – datový výstup rámce.* Hello Python vstupní bod je pouze povolené tooreturn rámec jednoho datového jako výstup. Není aktuálně možné tooreturn, který libovolný Python objekty, jako je trénované modely přímo zpět toohello Azure Machine Learning runtime. Jako [spustit skript jazyka R][execute-r-script], který má hello stejné omezení, je možné v mnoha případech toopickle objektů do bajt pole a pak, vraťte uvnitř datové rámce.
4. *Nemohou toocustomize Python instalace*. V současné době hello pouze způsob tooadd vlastní moduly jazyka Python je přes mechanismus soubor zip hello popsané výše. To je vhodná pro malé moduly, je nevhodnou pro velké moduly (především těch s nativních knihoven DLL) nebo velký počet modulů. 

## <a name="conclusions"></a>Závěrů
Hello [Execute Python Script] [ execute-python-script] modulu umožňuje vědecký pracovník data existující kód Python tooincorporate do pracovních postupů hostovaných v cloudu machine learning v Azure Machine Learning a tooseamlessly zprovoznění je jako součást webové služby. modul skriptu Python Hello přirozeně spolupracuje s dalších modulů v Azure Machine Learning. modul Hello lze použít pro celou řadu úloh z zpracování toopre zkoumání dat a funkce extrakce a poté tooevaluation a po zpracování výsledků hello. modul runtime back-end Hello používá pro spuštění je založena na Anacondu, distribuci jazyka Python dobře otestované a často používaný. Tento back-end zjednodušuje pro vás tooon Tabule existující kód prostředky do cloudu hello.

Očekáváme, že další funkce toohello tooprovide [Execute Python Script] [ execute-python-script] modulu, jako hello možnost tootrain a zprovoznit modely v Pythonu a tooadd lepší podporu hello vývoj a ladění kódu v Azure Machine Learning Studio.

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello [středisku pro vývojáře Python](https://azure.microsoft.com/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
