---
title: kurz aaaQuickstart pro jazyk R pro Machine Learning | Microsoft Docs
description: "Pomocí této R programování kurz tooget rychle začít s používáním hello R jazyk s Azure Machine Learning Studio toocreate prognózy řešení."
keywords: "Rychlý start, jazyk r, programovací jazyk r, programovací kurzu r"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 99a3a0fd-b359-481a-b236-66868deccd96
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: 9995f8728f4d7bf9a5c15412015e4cf769cdac96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-tutorial-for-hello-r-programming-language-for-azure-machine-learning"></a>Rychlý úvodní kurz pro hello R programovací jazyk pro Azure Machine Learning

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a>Úvod
Tento rychlý úvodní kurz vám pomůže rychle spustit rozšíření Azure Machine Learning pomocí programovacího jazyka hello R. Postupujte podle tohoto R programování kurz toocreate, testování a spouštění R kódu v rámci Azure Machine Learning. Při práci prostřednictvím kurzu vytvoříte pomocí jazyka hello R v Azure Machine Learning kompletního řešení prognózy.  

Microsoft Azure Machine Learning obsahuje mnoho výkonné machine learning a data moduly manipulaci s. jazyk R výkonné Hello popsán jako hello lingua franca obchodu Analytics. Manipulace s analytickými funkcemi a data v Azure Machine Learning brouka, lze rozšířit pomocí R. Tato kombinace poskytne hello škálovatelnost a snadné nasazení Azure Machine Learning hello flexibilitu a hloubkové analytics R.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="forecasting-and-hello-dataset"></a>Vytváření prognóz a hello datové sady
Prognózy je metoda analytical široce zaměstnání a velmi užitečné. Běžné používá rozsah od predikci prodeje sezónní položek, určení úrovní optimální inventáře, toopredicting makroekonomické proměnné. Prognózy se většinou děje s modely časové řady.

Časových řad dat jsou data ve kterém hello hodnoty mají čas index. index Hello čas může být pravidelné, např. každý měsíc nebo každou minutu nebo nestandardní. Model časové řady podle data časové řady. programovací jazyk Hello R obsahuje flexibilní framework a rozsáhlé analýzy pro data časové řady.

V této příručce rychlý start jsme bude pracovat s produkční dojnic kalifornské a ceny data. Tato data zahrnují měsíční informace o provozním hello mléčných produktů a hello cena mléka fat komoditním srovnávacího testu.

data Hello používané v tomto článku, spolu se skripty R, mohou být [stáhnout zde][download]. Tato data byla původně syntetizován z informací dostupných ze hello univerzity Wisconsin v http://future.aae.wisc.edu/tab/production.html.

### <a name="organization"></a>Organizace
Jsme proběhne prostřednictvím několik kroků, jak se dozvíte, jak toocreate, testování a spouštění kódu manipulaci s R analytics a data v prostředí Azure Machine Learning hello.  

* Nejprve se podíváme na hello základy používání hello R jazyk v prostředí Azure Machine Learning Studio hello.
* Potom jsme průběhu toodiscussing různé aspekty vstupně-výstupních operací pro data, kódu jazyka R a grafiky v prostředí Azure Machine Learning hello.
* První část hello naše řešení prognózy jsme pak bude vytvořit tak, že vytvoříte kód pro čištění dat a transformace.
* Naše data připravený provedeme analýzu hello korelací mezi několik hello proměnných v naší datové sadě.
* Nakonec vytvoříme předpovědi modelu sezónní časové řady pro produkci mléka.

## <a id="mlstudio"></a>Komunikovat s jazyk R v Machine Learning Studio
Tato část vás provede některé základní informace o interakci s hello R programovací jazyk v prostředí Machine Learning Studio hello. jazyk Hello R poskytuje výkonný nástroj toocreate přizpůsobit analytics a data manipulaci s moduly prostředí Azure Machine Learning hello.

Použiji Rstudia toodevelop, testování a ladění kódu jazyka R v malém měřítku. Tento kód je pak vyjímání a vkládání do [spustit skript jazyka R] [ execute-r-script] modulu v připravené toorun Machine Learning Studio.  

### <a name="hello-execute-r-script-module"></a>modul pro spuštění skriptu jazyka R Hello
V rámci Machine Learning Studio, R skripty se spouštějí v rámci hello [spustit skript jazyka R] [ execute-r-script] modulu. Příkladem hello [spustit skript jazyka R] [ execute-r-script] modulu v nástroji Machine Learning Studio je znázorněno na obrázku 1.

 ![Programovací jazyk R: modulu spuštění skriptu R hello vybrané v nástroji Machine Learning Studio][1]

*Obrázek 1. zobrazení modulu spuštění skriptu R hello vybrané prostředí Machine Learning Studio Hello.*

Odkazy tooFigure 1, podíváme se na některé z klíčových částí hello hello Machine Learning Studio prostředí pro práci s hello [spustit skript jazyka R] [ execute-r-script] modulu.

* v prostředním podokně hello jsou uvedeny Hello moduly v experimentu hello.
* Hello horní část hello pravém podokně okna tooview obsahuje a upravit skripty R.  
* Hello dolní části pravém podokně jsou uvedeny některé vlastnosti hello [spustit skript jazyka R][execute-r-script]. Protokoly chyb a výstupu hello můžete zobrazit kliknutím na příslušné body hello tento podokna.

Jsme bude samozřejmě být hovoříte o hello [spustit skript jazyka R] [ execute-r-script] podrobněji na hello zbytek tohoto dokumentu.

Při práci s funkcemi, komplexní R, doporučujeme úpravy, testování a ladění ve Rstudia. Stejně jako u jakékoli vývoj softwaru postupně rozšiřovat kódu a testování na malé jednoduchých testovacích případech. Potom kopírování a vkládání funkcí do hello R skript okno hello [spustit skript jazyka R] [ execute-r-script] modulu. Tento přístup umožňuje tooharness jak hello Rstudia integrované vývojové prostředí (IDE) a hello power Azure Machine Learning.  

#### <a name="execute-r-code"></a>Spuštění kódu jazyka R
Žádný kód R v hello [spustit skript jazyka R] [ execute-r-script] modulu spustí při spuštění hello experiment kliknutím na hello **spustit** tlačítko. Po dokončení provádění zaškrtnutí se zobrazí na hello [spustit skript jazyka R] [ execute-r-script] ikonu.

#### <a name="defensive-r-coding-for-azure-machine-learning"></a>Obranným R kódování pro Azure Machine Learning
Pokud vyvíjíte kódu jazyka R pro vyslovení, webové služby pomocí Azure Machine Learning, byste měli výborný naplánovat, jak váš kód se bude zabývat vstup neočekávaná data a výjimky. toomaintain přehlednost I nebyly zahrnuty hodně způsobem hello kontrola nebo zpracování výjimek v většinu příklady kódu hello vidět. Ale jak budeme pokračovat I získáte několik příkladů, funkce pomocí funkce pro zpracování výjimek na R.  

Pokud potřebujete podrobnější zacházení s zpracovávání výjimek v jazyce R, I doporučujeme, abyste si přečetli hello hello knihy podle Wickham uvedené v příslušných částech [příloha B – další čtení](#appendixb).

#### <a name="debug-and-test-r-in-machine-learning-studio"></a>Ladění a testování R v Machine Learning Studio
tooreiterate, I doporučujeme testování a ladění kódu R v malém měřítku v Rstudia. Ale existují případy, kdy budete potřebovat tootrack dolů problémy kód R v hello [spustit skript jazyka R] [ execute-r-script] sám sebe. Kromě toho je vhodné toocheck výsledky v Machine Learning Studio.

Výstup hello provádění kódu R a na platformě Azure Machine Learning hello především v souboru výstup.log nebyl nalezen. Některé další informace se zobrazí v error.log.  

Pokud při běhu kódu R dojde k chybě v nástroji Machine Learning Studio, vaše první postupu by měl být toolook v error.log. Tento soubor může obsahovat užitečné chybové zprávy toohelp pochopit a vaše chybu opravit. tooview error.log, kliknutím na tlačítko **zobrazení v protokolu chyb** na hello **podokno properties** pro hello [spustit skript jazyka R] [ execute-r-script] obsahující chyby hello.

Například byl spuštěn v následujícím kódu jazyka R nedefinované proměnné y, hello [spustit skript jazyka R] [ execute-r-script] modul:

    x <- 1.0
    z <- x + y

Tento kód se nezdaří tooexecute, což vede k chybě. Kliknutím na **zobrazení v protokolu chyb** na hello **podokno properties** vytváří hello zobrazení znázorňuje obrázek 2.

  ![Chybová zpráva otevíraném][2]

*Obrázek 2. Chybová zpráva automaticky otevírané okno.*

Zdá se, potřebujeme toolook v souboru výstup.log toosee hello R chybová zpráva. Klikněte na hello [spustit skript jazyka R] [ execute-r-script] a potom klikněte na hello **zobrazení souboru výstup.log** položky hello **podokno properties** toohello vpravo. Otevře se nové okno prohlížeče a zobrazuje následující hello.

    [Critical]     Error: Error 0063: hello following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

Tato chybová zpráva obsahuje žádný výskyt nečekaných událostí a jednoznačně identifikuje hello problém.

Hodnota hello tooinspect všech objektů v R, můžete vytisknout tyto hodnoty toohello souboru výstup.log souboru. Hello pravidla pro zkoumání objekt hodnoty jsou v podstatě stejný hello jako interaktivní relace R zařízením. Pokud zadáte název proměnné na řádek, například hodnota hello hello objektu bude tištěné toohello souboru výstup.log souboru.  

#### <a name="packages-in-machine-learning-studio"></a>Balíčky v nástroji Machine Learning Studio
Azure Machine Learning se dodává s více než 350 předinstalované balíčky jazyk R. Můžete použít následující kód v hello hello [spustit skript jazyka R] [ execute-r-script] modulu tooretrieve seznam hello předinstalována balíčky.

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

Pokud nerozumíte hello poslední řádek tohoto kódu v době hello, přečtěte si. Hello zbytek tohoto dokumentu hojně pojednává využití R v prostředí Azure Machine Learning hello.

### <a name="introduction-toorstudio"></a>TooRStudio Úvod
Rstudia je často používaný IDE pro R. Pro úpravy, testování a ladění některé hello R kód použitý v této úvodní příručce použiji Rstudia. Jakmile kódu jazyka R otestované a připravené, jednoduše kopírování a vkládání z editoru Rstudia hello do nástroje Machine Learning Studio [spustit skript jazyka R] [ execute-r-script] modulu.  

Pokud nemáte hello R programovací jazyk nainstalovaný na počítači klientů, doporučujeme, že udělejte to teď. Ke stažení zdarma jazyka R s otevřeným zdrojem jsou k dispozici na hello komplexní R archivu sítě (CRAN) v [http://www.r-project.org/](http://www.r-project.org/). Nejsou k dispozici pro Windows, Mac OS a Linux a UNIX stáhne. Zvolte blízkým zrcadlení a postupujte podle pokynů ke stažení hello. Kromě toho CRAN obsahuje širokou řadu užitečné balíčky manipulaci s analytickými funkcemi a data.

Pokud jste nový tooRStudio, by měl stáhněte a nainstalujte verzi pro stolní počítače hello. Můžete najít hello Rstudia soubory ke stažení pro Windows, Mac OS a Linux a UNIX v http://www.rstudio.com/products/RStudio/. Postupujte podle pokynů hello tooinstall Rstudia k dispozici na stolního počítače.  

Kurz Úvod tooRStudio je k dispozici na https://support.rstudio.com/hc/sections/200107586-Using-RStudio.

Poskytují I některé další informace o používání Rstudia v [příloha A][appendixa].  

## <a id="scriptmodule"></a>Získání dat a deaktivovat modulu spuštění skriptu R hello
V této části probereme, jak načíst data do a z hello [spustit skript jazyka R] [ execute-r-script] modulu. Jsme zkontrolujete jak toohandle různé datové typy číst do a z hello [spustit skript jazyka R] [ execute-r-script] modulu.

Hello kompletní kód pro tento oddíl je v souboru zip hello, které jste dříve stáhli.

### <a name="load-and-check-data-in-machine-learning-studio"></a>Načtení a zkontrolujte, zda data v nástroji Machine Learning Studio
#### <a id="loading"></a>Načíst hello datové sady
Spustíme načtením hello **csdairydata.csv** souboru do Azure Machine Learning Studio.

* Spuštění prostředí Azure Machine Learning Studio.
* Klikněte na **+ nový** v hello nižší levé straně obrazovky a vyberte **datovou sadu**.
* Vyberte **z místního souboru**a potom **Procházet** tooselect hello souboru.
* Ujistěte se, zda jste vybrali **souboru obecné CSV s hlavičkou (.csv)** jako typ hello hello datové sady.
* Klikněte na tlačítko zaškrtnutí hello.
* Až se nahrají hello datovou sadu, měli byste vidět nová datová sada hello kliknutím na hello **datové sady** kartě.  

#### <a name="create-an-experiment"></a>Vytvoření experimentu
Teď, když máme některá data v nástroji Machine Learning Studio, potřebujeme toocreate analýzu hello toodo experimentu.  

* Klikněte na **+ nový** v hello snížit vlevo a vyberte **experimentu**, pak **prázdný Experiment**.
* Můžete tak, že vyberete název experimentu a úpravy, hello **na vytvořit experimentu...**  nadpis v horní části hello hello stránky. Například změna příliš**certifikační Autority mlékárny Analysis**.
* Na levé straně hello hello experimentu stránky rozbalte **uložit datové sady**a potom **Moje datové sady**. Měli byste vidět hello **cadairydata.csv** který jste dříve nahráli.
* Přetažení hello **datovou sadu csdairydata.csv** do experimentu hello.
* V hello **vyhledávání experimentovat položky** pole na hello horní části levého podokna hello, typ [spustit skript jazyka R][execute-r-script]. Zobrazí se modul hello jsou uvedeny v seznamu hledání hello.
* Přetažení hello [spustit skript jazyka R] [ execute-r-script] modulu do vaší palety.  
* Připojit hello výstup hello **datovou sadu csdairydata.csv** krajní levé vstup toohello (**Dataset1**) z hello [spustit skript jazyka R][execute-r-script].
* **Nezapomeňte tooclick na 'Uložit'!**  

V tomto okamžiku by měl experimentu vypadat podobně jako obrázek 3.

![Hello certifikační Autority mlékárny Analysis experimentovat s datovou sadu a modul pro spuštění skriptu jazyka R][3]

*Obrázek 3. Hello certifikační Autority mlékárny Analysis experimentovat s datovou sadu a spustit skript jazyka R modulu.*

#### <a name="check-on-hello-data"></a>Zkontrolujte data hello
Pojďme Podíváme se na hello data, která mají byl načten do našich experimentu. V experimentu hello, klikněte na výstup hello hello **datovou sadu cadairydata.csv** a vyberte **vizualizovat**. Měli byste vidět něco podobného jako na obrázku 4.  

![Souhrn hello cadairydata.csv datové sady][4]

*Obrázek 4. Shrnutí hello cadairydata.csv datovou sadu.*

V tomto zobrazení vidíte mnoho užitečných informací. My uvidíme text hello první několik řádků z této datové sady. Pokud jsme vyberte sloupec, hello statistiky části zobrazíte další informace o sloupci hello. Typ funkce řádek hello příkladu nám jaké typy dat Azure Machine Learning Studio přiřazené toohello sloupce. Rychle zkontrolovat tímto způsobem je kontrolu dobrý správností než začneme toodo jakékoli závažné pracovní.

### <a name="first-r-script"></a>První skript jazyka R
Umožňuje vytvořit jednoduché první skriptu tooexperiment R s v Azure Machine Learning Studio. I vytvoření a otestování hello následující skript v Rstudia.  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    str(cadairydata)
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

Nyní je třeba tootransfer tento skript tooAzure Machine Learning Studio. Může stačí vyjmout a vložit. Ale v takovém případě bude přenést Moje skript jazyka R prostřednictvím soubor zip.

### <a name="data-input-toohello-execute-r-script-module"></a>Modul spustit skript jazyka R toohello vstupních dat
Pojďme Podíváme se na hello vstupy toohello [spustit skript jazyka R] [ execute-r-script] modulu. V tomto příkladu jsme bude číst data dojnic kalifornské hello do hello [spustit skript jazyka R] [ execute-r-script] modulu.  

Existují tři možné vstupy pro hello [spustit skript jazyka R] [ execute-r-script] modulu. Může používat kterékoli nebo všechny tyto vstupy, v závislosti na vaší aplikace. Je také perfektně přiměřené toouse R skript, který nemá žádný vstup vůbec.  

Podívejme se na každý z těchto vstupů přecházející z levé tooright. Uvidíte hello názvy jednotlivých hello vstupy umístěním kurzoru přes hello vstup a čtení hello popisku.  

#### <a name="script-bundle"></a>Skript sady
Hello vstupu skriptu sady vám umožní toopass hello obsahu souboru zip do [spustit skript jazyka R] [ execute-r-script] modulu. Můžete použít jednu z následujících příkazů tooread hello obsahu souboru zip hello do vašeho kódu jazyka R hello.

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [!NOTE]
> Azure Machine Learning zpracovává soubory v hello zip, jako kdyby se hello src / adresáře, proto musíte tooprefix názvy váš soubor s tímto názvem adresáře. Například pokud hello zip obsahuje soubory hello `yourfile.R` a `yourData.rdata` hello kořenové hello zip, by adres jako `src/yourfile.R` a `src/yourData.rdata` při použití `source` a `load`.
> 
> 

Už jsme probírali načítání datové sady v [načítání datovou sadu hello](#loading). Po vytvoření a testování hello R skript uvedené v předchozí části hello hello následující:

1. Uložte skript hello R do. R soubor. Můžu volat Moje soubor skriptu "simpleplot. R". Zde je hello obsah.
   
        ## Only one of hello following two lines should be used
        ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
        ## If in RStudio, use hello second line with read.csv()
        cadairydata <- maml.mapInputPort(1)
        # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
        str(cadairydata)
        pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
        ## hello following line should be executed only when running in
        ## Azure Machine Learning Studio
        maml.mapOutputPort('cadairydata')
2. Vytvořte soubor zip a zkopírujte skript do tohoto souboru zip. V systému Windows, klikněte pravým tlačítkem na soubor hello a vyberte **poslat**a potom **komprimované složky**. Tím se vytvoří nový soubor zip obsahující hello "simpleplot. Soubor R".
3. Přidat váš soubor toohello **datové sady** v nástroji Machine Learning Studio, určení typu hello jako **zip**. Teď byste měli vidět soubor zip hello v vaše datové sady.
4. Přetažení soubor zip hello z **datové sady** na hello **ML Studio plátno**.
5. Připojit hello výstup hello **zip data** ikonu toohello **skript sady** vstupní z hello [spustit skript jazyka R] [ execute-r-script] modulu.
6. Typ hello `source()` funkce nahraďte názvem souboru zip do hello kódu – okno pro hello [spustit skript jazyka R] [ execute-r-script] modulu. V případě Moje zadali `source("src/simpleplot.R")`.  
7. Ujistěte se, kliknete na tlačítko **Uložit**.

Po dokončení těchto kroků se hello [spustit skript jazyka R] [ execute-r-script] modul provede hello R skript v souboru zip hello při spuštění experimentu hello. V tomto okamžiku by měl experimentu vypadat podobně jako obrázek 5.

![Experiment pomocí komprimované skript jazyka R][6]

*Obrázek 5. Experiment pomocí komprimované R skript.*

#### <a name="dataset1"></a>Dataset1
Obdélníková tabulku data tooyour R kód můžete předat pomocí hello Dataset1 vstup. V našich hello jednoduchého skriptu `maml.mapInputPort(1)` funkce čte hello data z portu 1. Tato data přiřazen název proměnné tooa dataframe ve vašem kódu. V našem jednoduchého skriptu provede hello první řádek kódu hello přiřazení.

    cadairydata <- maml.mapInputPort(1)

Spusťte experimentu tak, že kliknete na hello **spustit** tlačítko. Po dokončení provádění hello, klikněte na hello [spustit skript jazyka R] [ execute-r-script] modul a potom klikněte na **zobrazit výstup protokol** v podokně Vlastnosti hello. Nová stránka by se zobrazit v prohlížeči zobrazující hello obsah souboru souboru výstup.log hello. Když přejděte dolů by měl vidět něco podobného jako následující hello.

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

Dále dolů hello stránka je podrobnější informace o hello sloupce, které bude vypadat podobně jako následující hello.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput]
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput]
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month            : chr  "Jan" "Feb" "Mar" "Apr" ...
    [ModuleOutput]
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput]
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput]
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput]
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...

Tyto výsledky se většinou podle očekávání, s 228 připomínky a 9 sloupců v hello dataframe. Jsme můžete zobrazit názvy sloupců hello, hello R datový typ a ukázku jednotlivých sloupců.

> [!NOTE]
> Tento stejný tištěné výstup je pohodlně dostupná z hello R zařízení výstup hello [spustit skript jazyka R] [ execute-r-script] modulu. Podrobně probereme hello výstupy hello [spustit skript jazyka R] [ execute-r-script] modulu v další části hello.  
> 
> 

#### <a name="dataset2"></a>Dataset2
Hello chování hello Dataset2 vstupu je identické toothat Dataset1. Pomocí tento vstup lze předat v druhé tabulce obdélníková dat do vašeho kódu jazyka R. Hello funkce `maml.mapInputPort(2)`, s argumentem hello 2, je použít toopass tato data.  

### <a name="execute-r-script-outputs"></a>Spustit skript jazyka R výstupy
#### <a name="output-a-dataframe"></a>Výstup a dataframe
Výstup můžete obsah hello dataframe R jako obdélníková tabulku přes port hello výsledek Dataset1 pomocí hello `maml.mapOutputPort()` funkce. V našem jednoduchý skript jazyka R se provádí pomocí hello následující řádek.

    maml.mapOutputPort('cadairydata')

Po spuštěné hello experiment, klikněte na hello výsledek Dataset1 výstupní port a potom klikněte na **vizualizovat**. Měli byste vidět něco podobného jako obrázek 6.

![Hello vizualizaci hello výstup hello kalifornské dojnic dat][7]

*Obrázek 6. Hello vizualizaci hello výstup hello kalifornské dojnic data.*

Tento výstup vypadá identické toohello vstup přesně tak, jak očekávali jsme.  

### <a name="r-device-output"></a>Výstup R zařízení
Hello zařízení výstup hello [spustit skript jazyka R] [ execute-r-script] modul obsahuje zprávy a grafický výstup. Standardní výstupní zařízení a standardní cíl chybové zprávy i z R jsou odesílány toohello R zařízení výstupní port.  

tooview hello zařízení R výstup, klikněte na hello portu a potom na **vizualizovat**. Vidíme hello standardní výstupní zařízení a standardní chyba ze skriptu hello R na obrázku 7.

![Standardní výstupní zařízení a standardní chyba z hello port zařízení R][8]

*Na obrázku 7. Standardní výstupní zařízení a standardní chyba z hello R zařízení portu.*

Posouvání dolů jsme najdete v části hello grafického výstupu z našich skript jazyka R na obrázku 8.  

![Grafika výstup hello port zařízení R][9]

*Obrázek 8. Grafika výstup z hello R zařízení portu.*  

## <a id="filtering"></a>Filtrování dat a transformace
V této části provedeme některé základní data filtrování a operace transformace na hello kalifornské dojnic data. Hello konci této části máme data ve formátu, který je vhodný pro sestavování analytického modelu.  

Přesněji řečeno, v této části provedeme několik běžných čištění a transformace úlohy dat: zadejte transformaci, filtrování dataframes, přidání nové počítaných sloupcích a hodnota transformace. Tato pozadí pomáhají řešit hello mnoho variant v reálných problémy.

Hello dokončení R kód pro tento oddíl je k dispozici v souboru zip hello, které jste dříve stáhli.

### <a name="type-transformations"></a>Typ transformace
Teď, když jsme můžete číst data dojnic kalifornské hello do kódu hello R v hello [spustit skript jazyka R] [ execute-r-script] modulu, potřebujeme tooensure, že má hello data ve sloupcích hello hello určený typ a formát.  

R je dynamicky zadávaných jazyk, což znamená, že datové typy jsou má z jednoho tooanother podle potřeby. Hello atomic datových typů v R zahrnují číselné literály, logické a znak. typ multi-Factor Hello je použité toocompactly úložiště kategorizovaná data. Mnohem Další informace o typech dat najdete v odkazech na hello v [příloha B – další čtení](#appendixb).

Pokud tabulková data je pro čtení do R z externího zdroje, je vždy vhodné toocheck hello výsledná typy ve sloupcích hello. Může být vhodné sloupec – znak typu, ale v mnoha případech to se zobrazí jako faktor nebo naopak. V ostatních případech a sloupec, který si myslíte, že by měl být číselný je reprezentována textová data, například '1,23' než 1,23 jako plovoucí bodu číslo.  

Naštěstí je snadno tooconvert jeden typ tooanother tak dlouho, dokud je možné mapování. Například 'Nevada' nelze převést číselnou hodnotu, ale můžete ji převést tooa faktor (kategorií proměnné). Například můžete převést číselnou 1 znak '1' nebo koeficient.  

Hello syntaxe pro některý z těchto převody je jednoduchý: `as.datatype()`. Tyto funkce pro převod typů zahrnují následující hello.

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

Prohlížení hello datové typy sloupců hello zadáme v předchozí části hello: jsou všechny sloupce typu číselné, s výjimkou hello sloupec s názvem 'Měsíc', který je – znak typu. Umožňuje převést tento faktor tooa a hello výsledky testu.  

Odstraněný hello řádek, který vytvoří hello scatterplot matice a přidat řádek převádění Multi-Factor tooa sloupec "Měsíc" hello. V experimentu se právě kopírování a vkládání hello R kódu do kódu okno hello hello [spustit skript jazyka R] [ execute-r-script] modulu. Můžete také aktualizovat soubor zip hello a nahrajte ho tooAzure Machine Learning Studio, ale tato akce trvá několik kroků.  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(cadairydata$Month)
    str(cadairydata) # Check hello result
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

Umožňuje spustit tento kód a podívejte se na výstup protokolu hello hello R skript. Hello relevantní data z protokolu hello je vidět na obrázku 9.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 14 levels "Apr","April",..: 6 5 9 1 11 8 7 3 14 13 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*Obrázek 9. Shrnutí hello dataframe s Multi-Factor proměnné.*

Nyní by mělo být uvedeno Hello typ pro měsíc '**Multi-Factor s 14 úrovně**'. Tento problém je vzhledem k tomu, že existují pouze po dobu 12 měsíců v roce hello. Můžete také zkontrolovat toosee, který hello typu v **vizualizovat** hello datovou sadu výsledků je port,**Categorical**'.

Hello problém je, že hello sloupec nebyl byla programového systematičtěji měsíc. V některých případech se nazývá měsíc duben a k ostatním se zkracuje dubna. Ořízne hello řetězec too3 znaků jsme můžete tento problém vyřešit. Hello řádek kódu teď vypadá hello následující:

    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

Spusťte hello experiment a zobrazit protokol výstup hello. Hello očekává, že výsledky se zobrazí na obrázku 10.  

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*Obrázek 10. Shrnutí hello dataframe s správný počet úrovní faktor.*

Naše Multi-Factor proměnná teď má hello potřeby 12 úrovně.

### <a name="basic-data-frame-filtering"></a>Filtrování rámce základní data
R dataframes podporovat výkonné možnosti filtrování. Datové sady, může být podsady pomocí logických filtry na řádky nebo sloupce. V mnoha případech se bude vyžadovat složitá kritéria filtru. odkazy na Hello v [příloha B – další čtení](#appendixb) obsahovat rozsáhlé příklady filtrování dataframes.  

Je jeden bit filtrování by měl provedeme v naší datové sadě. Pokud se podíváte na hello sloupců v hello cadairydata dataframe, zobrazí se dva nepotřebných sloupců. Hello první sloupec obsahuje jenom číslo řádku, které není velmi užitečné. druhý sloupec Hello Year.Month, obsahuje redundantní informace. Tyto sloupce jsme snadno můžete vyloučit pomocí následující kód R hello.

> [!NOTE]
> Od teď na v této části, bude jenom zobrazit můžete hello další kód přidám v hello [spustit skript jazyka R] [ execute-r-script] modulu. Přidat každý nový řádek **před** hello `str()` funkce. Tato funkce tooverify používám v Azure Machine Learning Studio Moje výsledky.
> 
> 

Přidat následující řádek kódu toomy R v hello hello [spustit skript jazyka R] [ execute-r-script] modulu.

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

Spusťte tento kód v experimentu a zkontrolujte výsledek hello z protokolu výstup hello. Tyto výsledky se zobrazí v obrázek 11.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  7 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*Obrázek 11. Souhrn dataframe hello se dvěma sloupci odebrat.*

Dobrá zpráva! Nemůžeme získat hello očekávané výsledky.

### <a name="add-a-new-column"></a>Umožňuje přidat nový sloupec
toocreate časové řady modely bude vhodné toohave sloupec obsahující hello měsíců od začátku hello hello časové řady. Vytvoříme nový sloupec, Month.Count'.

toohelp uspořádání hello kód vytvoříme naše první jednoduché funkce `num.month()`. Pak budou použity této funkce toocreate nový sloupec v hello dataframe. nový kód Hello je následující.

    ## Create a new column with hello month count
    ## Function toofind hello number of months from hello first
    ## month of hello time series
    num.month <- function(Year, Month) {
      ## Find hello starting year
      min.year  <- min(Year)

      ## Compute hello number of months from hello start of hello time series
      12 * (Year - min.year) + Month - 1
    }

    ## Compute hello new column for hello dataframe
    cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)

Teď spustit experiment hello aktualizovat a použít hello výstup protokolu tooview hello výsledky. Tyto výsledky se zobrazí obrázek 12.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*Obrázek 12. Shrnutí hello dataframe hello další sloupec.*

Vypadá to vše funguje. V našem dataframe máme hello nový sloupec s hello očekávaných hodnot.

### <a name="value-transformations"></a>Hodnota transformace
V této části provedeme některé jednoduché transformace na hello hodnoty v některé z našich dataframe hello sloupce. Hello R jazyk podporuje transformace téměř libovolná hodnota. odkazy na Hello v [příloha B – další čtení](#appendixb) obsahovat rozsáhlé příklady.

Pokud si prohlédnete hello hodnoty v hello souhrnných informací o našem dataframe měli byste vidět něco liché sem. Další zmrzlinová než mléka vytváří v kalifornské? Ne, samozřejmě není, jak to nemá smysl sad jako tento fakt může být toosome nás zmrzlinová lovers. jednotky Hello se liší. cena Hello je jednotek nám libra mléka je v jednotkách 1 milion libra USA, zmrzlinová je v jednotkách 1 000 nám galony a byt sýr je v jednotkách 1 000 libra USA. Za předpokladu, že zmrzlinová váží asi 6,5 libra za spotřeby, můžeme snadno hello tyto hodnoty tak, aby byly všechny ve stejné jednotky 1000 libra tooconvert násobení.

Pro náš model prognózy používáme multiplikativní model pro trendu a sezónní úpravu tato data. Transformace protokolu nám umožňuje toouse model lineární ke zjednodušení tohoto procesu. Použijeme hello protokolu transformací v hello stejnou funkci, kdy se používá násobitel hello.

V hello následující kód, lze definovat novou funkci, `log.transform()`a použijte ho toohello řádky obsahující hello číselné hodnoty. Hello R `Map()` funkce je použité tooapply hello `log.transform()` funkce toohello vybrané sloupce hello dataframe. `Map()`je příliš podobné`apply()` , ale umožňuje více než jeden seznam argumentů toohello funkce. Všimněte si, že poskytuje seznam multiplikátory hello druhý argument toohello `log.transform()` funkce. Hello `na.omit()` funkce slouží jako trochu z čištění tooensure nemáme chybí nebo nedefinované hodnoty v hello dataframe.

    log.transform <- function(invec, multiplier = 1) {
      ## Function for hello transformation, which is hello log
      ## of hello input value times a multiplier

      warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                           "ERROR: Arguments toofunction log.transform must be greate than zero",
                           "ERROR: Aggurment multiplier toofuncition log.transform must be a scaler",
                           "ERROR: Invalid time seies value encountered in function log.transform"
                           )

      ## Check hello input arguments
      if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
      if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
      if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

      ## Wrap hello multiplication in tryCatch
      ## If there is an exception, print hello warningmessage to
      ## standard error and return NA
      tryCatch(log(multiplier * invec),
               error = function(e){warning(warningmessages[4]); NA})
    }


    ## Apply hello transformation function toohello 4 columns
    ## of hello dataframe with production data
    multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
    cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

    ## Get rid of any rows with NA values
    cadairydata <- na.omit(cadairydata)  

Je s bit situaci v hello `log.transform()` funkce. Většina tohoto kódu je kontrola potenciálních problémů spojených s argumenty hello nebo týkající se výjimky, které mohou stále nastat během hello výpočty. Tento kód jenom pár řádků ve skutečnosti hello výpočty.

cílem Hello Obranným programování hello je tooprevent hello selhání jedné funkce, která zabraňuje zpracování pokračovat. K náhlému selhání dlouho běžící analysis může být poměrně frustrující pro uživatele. tooavoid, které se této situaci, výchozí, musí být zvolena návratové hodnoty, které omezí poškodit toodownstream zpracování. Zpráva je také vytvořené tooalert uživatele, kteří se něco pokazilo přešel.

Pokud si nejste použité toodefensive programování v R, tento kód se může zdát trochu čtenáře. I vás provede důležitými kroky hello:

1. Vektor čtyři zprávy je definována. Tyto zprávy jsou použité toocommunicate informace o některých hello možné chyby a výjimky, které se můžou vyskytnout s tímto kódem.
2. Návratu NA hodnotu v obou případech. Existuje mnoho dalších možností, které by mohly mít méně vedlejší účinky. Vektor nul nebo hello původní vstupní vektoru, může například návratu.
3. Kontroluje se spouštějí na hello argumenty toohello funkce. V každém případě pokud je detekována chyba, je vrácen výchozí hodnotu a zprávy je produkovaný hello `warning()` funkce. Používám `warning()` místo `stop()` jako hello pozdější ukončí provádění, přesně co pokouším tooavoid. Všimněte si, že I mají zapisovat tento kód v procedurální styl, jako v tomto případě funkční přístup mailech vypadalo komplexní a skrytého.
4. výpočty protokolu Hello je uzavřen do `tryCatch()` tak, aby výjimky nezpůsobí tooprocessing náhlému zastavení. Bez `tryCatch()` aktivováno R funkce povede signál k zastavení, která zajišťuje právě, většina chyb.

Spustit tento kód R v experimentu a podívejte se na hello vytisknout výstup do souboru souboru výstup.log hello. Nyní uvidíte hello transformuje hodnoty hello čtyři sloupce v hello přihlašovat, jak ukazuje obrázek 13.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

*Obrázek 13. Souhrn hello transformuje hodnoty v hello dataframe.*

Vidíte, že byly transformovány hello hodnoty. Nyní produkce mléka výrazně přesahuje všechny ostatní mléčný výrobek produkční, vrací, že jsme teď vyhledávání škálované protokolu.

V tomto okamžiku je naše data vyčištěna a jsme připraveni pro některé modelování. Prohlížení hello vizualizace shrnutí hello výstupní datovou sadu výsledků z našich [spustit skript jazyka R] [ execute-r-script] modulu, zobrazí se vám sloupec "Měsíc" hello je 'Categorical' s 12 jedinečné hodnoty, znovu, stejně jako jsme chtěli .

## <a id="timeseries"></a>Časové řady objekty a analýzy korelace
V této části jsme se několika základních objektů řady čas R zkoumat a analyzovat hello korelací mezi určité proměnné hello. Naším cílem je toooutput dataframe obsahující dobrý pairwise korelace informací o na několik pomalou.

Hello dokončení R kód pro tento oddíl je v souboru zip hello, které jste dříve stáhli.

### <a name="time-series-objects-in-r"></a>Časové řady objekty v R
Jak už jsme už zmínili, časové řady jsou řady hodnot dat indexované podle času. R časové řady objekty jsou použité toocreate a spravovat hello čas index. Existuje několik výhod toousing časové řady objekty. Časové řady objekty vás zbaví hello mnoho podrobnosti o správě hello časových řad index hodnoty, které jsou zapouzdřené v objektu hello. Kromě toho časové řady objekty povolit toouse hello mnoho času řady metod pro vykreslení, tisk, modelování a podobně.

Hello POSIXct time series třída se často používá a je poměrně jednoduché. Tato třída časové řady míry čas z hello začátek hello epoch, 1. ledna 1970. V tomto příkladu použijeme POSIXct časové řady objekty. Další často používaný R časové řady tříd objektů zahrnují zoo a xts, extensible časové řady.
<!-- Additional information on R time series objects is provided in hello references in Section 5.7. [commenting because this section doesn't exist, even in hello original] -->

### <a name="time-series-object-example"></a>Příklad objekt časové řady
Můžeme začít pracovat s našem příkladu. Přetáhnout myší **nové** [spustit skript jazyka R] [ execute-r-script] modulu do experimentu. Připojit hello výsledek Dataset1 výstupní port hello existující [spustit skript jazyka R] [ execute-r-script] modulu toohello Dataset1 vstupní port hello nové [spustit skript jazyka R] [ execute-r-script] modulu.

Jak bylo možné příklady první hello, jako jsme průběh prostřednictvím hello například v některé body, které se zobrazí I pouze hello přírůstkové další řádky kódu jazyka R při každém kroku.  

#### <a name="reading-hello-dataframe"></a>Čtení hello dataframe
Jako první krok můžeme načtení dataframe a ujistěte se, že se nám získat hello očekávané výsledky. Hello následující kód by měl provést úlohy hello.

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check hello results

Nyní spusťte hello experiment. Hello protokolu nový tvar spustit skript jazyka R hello by měl vypadat jako obrázek 14.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...

*Obrázek 14. Shrnutí hello dataframe v modulu spustit skript jazyka R hello.*

Tato data jsou hello očekává typů a formát. Upozorňujeme, že sloupec "Měsíc" hello je typ faktoru a očekává se, že hello počet úrovní.

#### <a name="creating-a-time-series-object"></a>Vytvoření objektu časové řady
Potřebujeme tooadd dataframe tooour objekt řady a čas. Nahraďte hello následující text, který přidá nový sloupec třídy POSIXct hello aktuálního kódu.

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check hello results

Teď zkontrolujte protokol hello. By měl vypadat jako obrázek 15.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Obrázek 15. Shrnutí hello dataframe s objektem časové řady.*

Jsme můžete zjistit z hello souhrnné že Hello nové sloupce je ve skutečnosti třídy POSIXct.

### <a name="exploring-and-transforming-hello-data"></a>Zkoumání a transformace dat hello
Podíváme se na určité proměnné hello v této datové sadě. Matice scatterplot je dobře tooproduce rychle zkontrolovat. I mě nahrazení hello `str()` funkce v předchozí kód R hello s hello následující řádek.

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

Spustit tento kód a zobrazit, co se stane. vykreslení Hello vytvořeného v hello port R zařízení by měl vypadat jako obrázek 16.

![Scatterplot matice vybrané proměnné][17]

*Obrázek 16. Matice Scatterplot vybrané proměnné.*

Je některé odd-looking struktura hello vztahy mezi tyto proměnné. Možná to mohou nastat z trendů v datech hello a hello fakt, že jsme nebyly standardizované hello proměnné.

### <a name="correlation-analysis"></a>Analýza korelace
tooperform korelace analýzy, že potřebujeme tooboth zrušte trendů a standardizovat hello proměnné. Můžeme jednoduše použít hello R `scale()` funkci, která centra i škáluje proměnné. Tato funkce může také pracovat rychleji. Ale chci tooshow je příkladem Obranným programing v jazyce R.

Hello `ts.detrend()` provádí následující funkce obou těchto operací. Hello následující dva řádky kódu zrušte trendů hello data a pak standardizovat hello hodnoty.

    ts.detrend <- function(ts, Time, min.length = 3){
      ## Function toode-trend and standardize a time series

      ## Define some messages if they are NULL  
      messages <- c('ERROR: ts.detrend requires arguments ts and Time toohave hello same length',
                    'ERROR: ts.detrend requires argument ts toobe of type numeric',
                    paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                    'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                    'ERROR: Detrend regression has failed in ts.detrend',
                    'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
      )
      # Create a vector of zeros tooreturn as a default in some cases
      zerovec  <- rep(length(ts), 0.0)

      # hello input arguments are not of hello same length, return ts and quit
      if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

      # If hello ts is not numeric, just return a zero vector and quit
      if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

      # If hello ts is too short, just return it and quit
      if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

      ## Check that hello Time variable is of class POSIXct
      if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

      ## De-trend hello time series by using a linear model
      ts.frame  <- data.frame(ts = ts, Time = Time)
      tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
               error = function(e){warning(messages[5]); zerovec})

      tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
                 ts <- ts/stdev},
                error = function(e){warning(messages[6]); zerovec})

      ts
    }  
    ## Apply hello detrend.ts function toohello variables of interest
    df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

    ## Plot hello results toolook at hello relationships
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")

Je s bit situaci v hello `ts.detrend()` funkce. Většina tohoto kódu je kontrola potenciálních problémů spojených s argumenty hello nebo týkající se výjimky, které mohou stále nastat během hello výpočty. Tento kód jenom pár řádků ve skutečnosti hello výpočty.

Už jsme projednat příkladem Obranným programování v [hodnota transformace](#valuetransformations). Obě bloky výpočtu je uzavřen do `tryCatch()`. Některé chyby má smysl tooreturn hello původní vstupní vektoru a v ostatních případech návratu vektoru nul.  

Všimněte si, že hello lineární regrese používá pro zrušte trendů je to regrese časové řady. Proměnná předpověď Hello je objekt časové řady.  

Jednou `ts.detrend()` je definována ho použijeme toohello proměnné zájem o náš dataframe. Jsme musí coerce hello výsledný seznam vytvořený `lapply()` toodata dataframe pomocí `as.data.frame()`. Z důvodu Obranným aspektů `ts.detrend()`, tooprocess selhání jedné z proměnných hello nezabrání opravte zpracování hello ostatní.  

poslední řádek kódu Hello vytvoří pairwise scatterplot. Po spuštění kódu hello R, jsou výsledky hello hello scatterplot uvedené v obrázek 17.

![Pairwise scatterplot zrušte trendu a standardizovaném časové řady][18]

*Obrázek 17. Pairwise scatterplot zrušte trendu a standardizovaném časové řady.*

Tyto výsledky toothose znázorňuje obrázek 16, můžete porovnat. S hello trend odebrána a hello proměnné standardizované, vidíme mnohem menší struktury hello vztahy mezi tyto proměnné.

Hello kód toocompute hello korelací jako objekty PVJS R je následující.

    ## A function toocompute pairwise correlations from a
    ## list of time series value vectors
    pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
      ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
    }

    ## A list of hello pairwise indices
    corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

    ## Compute hello list of ccf objects
    cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

    cadairycorrelations

Spuštění tento kód vytvoří hello protokolu znázorňuje obrázek 18.

    [ModuleOutput] Loading objects:
    [ModuleOutput]   port1
    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] [[1]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.148 0.358 0.317 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[2]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.395 -0.186 -0.238 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[3]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.059 -0.089 -0.127 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[4]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.140 0.294 0.293 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[5]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.002 -0.074 -0.124 

*Obrázek 18. Seznam PVJS objekty z dobrý pairwise korelace analýzy.*

Je hodnota korelace pro každý prodleva. Žádná z těchto hodnot korelace není dostatečně velké na to toobe významné. Jsme proto uzavřít, že jsme každou proměnnou modelu nezávisle.

### <a name="output-a-dataframe"></a>Výstup a dataframe
Pairwise korelací hello jsme mít počítá jako seznam R PVJS objektů. To představuje bit problému jako hello datovou sadu výsledků výstupní port ve skutečnosti vyžaduje dataframe. Navíc hello PVJS objektu je sám seznam a chceme, pouze hello hodnoty v hello první prvek seznamu, hello korelací v hello různé pomalou.

Následující kód extrahuje hello prodleva hodnoty ze seznamu hello PVJS objekty, které jsou sami seznamy Hello.

    df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

    c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
    r.names  <- c("Corr Cot Cheese - Ice Cream",
                  "Corr Cot Cheese - Milk Prod",
                  "Corr Cot Cheese - Fat Price",
                  "Corr Ice Cream - Mik Prod",
                  "Corr Ice Cream - Fat Price",
                  "Corr Milk Prod - Fat Price")

    ## Build a dataframe with hello row names column and the
    ## correlation data frame and assign hello column names
    outframe <- cbind(r.names, df.correlations)
    colnames(outframe) <- c.names
    outframe


    ## WARNING!
    ## hello following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

první řádek kódu Hello je trochu složité a vysvětlení mohou pomoci porozumět. Práce z hello uvnitř odhlašování máme hello následující:

1. Hello '**[[**'operátor s argumentem hello'**1**se vybere hello vektor korelací v hello pomalou z první prvek seznamu objekt PVJS hello hello.
2. Hello `do.call()` funkce se vztahuje hello `rbind()` funkce přes hello prvky hello seznamu vrátí podle `lapply()`.
3. Hello `data.frame()` funkce převede hello výsledku vytvořeného `do.call()` tooa dataframe.

Všimněte si, že názvy hello řádek ve sloupci hello dataframe. Díky tomu zachovává hello řádek názvy, když jsou výstup hello [spustit skript jazyka R][execute-r-script].

Spuštění kódu hello vytváří výstup hello znázorňuje obrázek 19 při I **vizualizovat** hello výstup v hello port datovou sadu výsledků. názvy řádků Hello jsou hello prvního sloupce, tak, jak má.

![Výstup výsledků z hello korelace analýzy][20]

*Obrázek 19. Výsledky výstup z hello korelace analýzy.*

## <a id="seasonalforecasting"></a>Příklad časové řady: sezónní prognózy
Naše data je teď ve formě vhodné pro analýzu a bylo zjištěno, že neexistují žádné významné korelací mezi hello proměnné. Umožňuje přesunout a vytvořte časové řady modelu prognózy. Pomocí tohoto modelu jsme se prognózy pro hello kalifornské mléka produkční 12 měsíců od 2013.

Naše předpovědi modelu bude mít dvě součásti, součást trendu a sezónní součást. dokončení prognózy Hello je produkt hello tyto dvě součásti. Tento typ modelu se označuje jako multiplikativní modelu. alternativní Hello je sčítání model. Už jsme provedli protokolu transformace toohello proměnné zájmu, což může tractable této analýze.

Hello dokončení R kód pro tento oddíl je v souboru zip hello, které jste dříve stáhli.

### <a name="creating-hello-dataframe-for-analysis"></a>Vytváření hello dataframe pro analýzu
Začněte přidáním **nové** [spustit skript jazyka R] [ execute-r-script] modulu tooyour experimentu. Připojit hello **datovou sadu výsledků** výstup hello existující [spustit skript jazyka R] [ execute-r-script] modulu toohello **Dataset1** vstupní hello nového modulu. výsledek Hello by měl vypadat podobně jako obrázek 20.

![Hello experimentovat s hello nový přidán modul spustit skript jazyka R][21]

*Obrázek 20. Hello experimentovat s nového modulu spustit skript jazyka R hello přidat.*

Jako hello korelace analýzy, kterou jsme právě dokončili, potřebujeme tooadd sloupec s objektem POSIXct časové řady. Hello následující kód a to stejně.

    # If running in Machine Learning Studio, uncomment hello first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

Spusťte tento kód a podívejte se na hello protokolu. výsledek Hello by měl vypadat jako obrázek 21.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Obrázek 21. Přehled hello dataframe.*

S tímto výsledkem jsme jsou připravené toostart Naše analýzy.

### <a name="create-a-training-dataset"></a>Vytvoření datové sady školení
S dataframe hello sestavený potřebujeme toocreate školení datové sady. Tato data budou zahrnovat všechny hello připomínky, s výjimkou hello posledních 12 hello roku 2013, což je naše testovací datové sady. Hello následující kód podmnožin hello dataframe a vytvoří pozemků hello dojnic produkčního prostředí a cena proměnných. Potom vytvořit pozemků hello čtyři produkčního prostředí a cena proměnné. Anonymní funkce je použité toodefine rozšiřuje pro vykreslení a pak provádějí iterace hello seznam hello další dva argumenty s `Map()`. Pokud přemýšlíte o, pro smyčky by mít fungovala bez problémů v tomto poli, jsou správná. Ale vzhledem k tomu, že je funkční jazyk R I mě ukazuje funkční přístup.

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

Spuštění kódu hello vytvoří hello z výstupu R zařízení hello znázorňuje obrázek 22 ukazuje zeměpisný řadu časové řady. Všimněte si, že časová osa hello je v jednotkách kalendářních dat, dobrý výhodou hello časové řady vykreslení metoda.

![První z řady pozemků čas kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-161.png)

![Druhý čas řady pozemků kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-162.png)

![Třetí čas řady pozemků kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-163.png)

![Čtvrtý čas řady pozemků kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-164.png)

*Obrázek 22. Čas řady pozemků produkci mléka kalifornské a cena data.*

### <a name="a-trend-model"></a>Model trendu
S vytvořili objekt řady čas a nutnosti seznámili hello dat, Začněme tooconstruct trend model hello kalifornské mléka provozními daty. Jsme to lze provést pomocí regrese časové řady. Je však vymazat z hello výkresu, jsme bude potřebovat víc než sklon a zachycení tooaccurately modelu hello zjištěnými trend hello Cvičná data.

Zadané hello v menším měřítku hello dat, bude sestavení hello model pro trend Rstudia a pak kopírování a vkládání hello výsledný model do Azure Machine Learning. Rstudia poskytuje interaktivní prostředí pro tento typ interaktivní analýzu.

Jako první pokus o bude proveden pokus polynomické regrese s zajišťuje až too3. Je-li skutečné riziko z přepsání hodí tyto druhy modelů. Proto je nejlepší tooavoid nejvyšších podmínky. Hello `I()` funkce omezuje výklad obsah hello (interpretuje hello obsah, jako je") a umožňuje vám toowrite oznámena interpretovaný funkce v regresní rovnice.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

Tím se vygeneruje následující hello.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3),
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12667 -0.02730  0.00236  0.02943  0.10586
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.33e+00   1.45e-01   43.60   <2e-16 ***
    ## Time              1.63e-09   1.72e-10    9.47   <2e-16 ***
    ## I(Month.Count^2) -1.71e-06   4.89e-06   -0.35    0.726
    ## I(Month.Count^3) -3.24e-08   1.49e-08   -2.17    0.031 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0418 on 212 degrees of freedom
    ## Multiple R-squared:  0.941,    Adjusted R-squared:  0.94
    ## F-statistic: 1.12e+03 on 3 and 212 DF,  p-value: <2e-16

Z hodnot P (Pr (> | t |)) v tento výstup vidíme, že hello kvadratických termín nemusí být důležité. Použiji hello `update()` funkce toomodify tento model pomocí vyřazování hello spolehlivosti termín.

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

Tím se vygeneruje následující hello.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12597 -0.02659  0.00185  0.02963  0.10696
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.38e+00   4.07e-02   156.6   <2e-16 ***
    ## Time              1.57e-09   4.32e-11    36.3   <2e-16 ***
    ## I(Month.Count^3) -3.76e-08   2.50e-09   -15.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0417 on 213 degrees of freedom
    ## Multiple R-squared:  0.941,  Adjusted R-squared:  0.94
    ## F-statistic: 1.69e+03 on 2 and 213 DF,  p-value: <2e-16

To vypadá lepší. Všechny podmínky hello jsou významná. Ale hello 2e-16 hodnota je výchozí hodnota a by neměly být příliš vážně brány.  

Jako testu správností vytvoříme s křivky trend hello vidět se časové řady graf hello kalifornské dojnic provozními daty. Následující kód v hello Azure Machine Learning hello je přidali [spustit skript jazyka R] [ execute-r-script] modelu (ne Rstudia) toocreate hello modelu a ujistěte se, vykreslení. výsledek Hello vidíte na obrázku 23.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![Kalifornské mléka provozními daty s zobrazený model trendu](./media/machine-learning-r-quickstart/unnamed-chunk-18.png)

*Obrázek 23. Kalifornské mléka provozními daty s trend vzoru.*

Zdá se, hello trend modelu dostatečně dobře vyhovuje hello data. Další nezdá toobe důkaz přečerpání vhodnosti, například v křivce modelu hello rychlé pohyby lichá.  

### <a name="seasonal-model"></a>Sezónní modelu
S modelem trend v dolním jsme vyžadují toopush na a zahrnují hello sezónní účinky. Použijeme hello měsíc roku hello jako fiktivní proměnné v hello lineární model toocapture hello po měsících vliv. Poznámka: když zavedete Multi-Factor proměnné do modelu, nesmí počítaný hello zachycení. Pokud není to uděláte, je-li přepsání zadané hello vzorec a R se vyřadit jeden hello potřeby faktory však pro průnik hello.

Vzhledem k tomu, že máme model uspokojivé trend můžeme použít hello `update()` funkce tooadd hello nové podmínky toohello existující model. Hello -1 ve vzorci aktualizace hello zahodí průnik hello. Pokračování v Rstudia pro chvíli hello:

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

Tím se vygeneruje následující hello.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3) + Month - 1,
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.06879 -0.01693  0.00346  0.01543  0.08726
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## Time              1.57e-09   2.72e-11    57.7   <2e-16 ***
    ## I(Month.Count^3) -3.74e-08   1.57e-09   -23.8   <2e-16 ***
    ## MonthApr          6.40e+00   2.63e-02   243.3   <2e-16 ***
    ## MonthAug          6.38e+00   2.63e-02   242.2   <2e-16 ***
    ## MonthDec          6.38e+00   2.64e-02   241.9   <2e-16 ***
    ## MonthFeb          6.31e+00   2.63e-02   240.1   <2e-16 ***
    ## MonthJan          6.39e+00   2.63e-02   243.1   <2e-16 ***
    ## MonthJul          6.39e+00   2.63e-02   242.6   <2e-16 ***
    ## MonthJun          6.38e+00   2.63e-02   242.4   <2e-16 ***
    ## MonthMar          6.42e+00   2.63e-02   244.2   <2e-16 ***
    ## MonthMay          6.43e+00   2.63e-02   244.3   <2e-16 ***
    ## MonthNov          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## MonthOct          6.37e+00   2.63e-02   241.8   <2e-16 ***
    ## MonthSep          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0263 on 202 degrees of freedom
    ## Multiple R-squared:     1,    Adjusted R-squared:     1
    ## F-statistic: 1.42e+06 on 14 and 202 DF,  p-value: <2e-16

Vidíme, že hello modelu už má pro průnik a 12 měsíc důležité faktory. Toto je přesně jsme chtěli toosee.

Provedeme jiný čas řady výkresu z hello kalifornské produkci mléka data toosee, jak dobře funguje hello sezónní modelu. Následující kód v hello Azure Machine Learning hello je přidali [spustit skript jazyka R] [ execute-r-script] toocreate hello modelu a ujistěte se, vykreslení.

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

Spuštění tohoto kódu v Azure Machine Learning vytvoří hello výkresu znázorňuje obrázek 24.

![Produkce mléka kalifornské s modelem včetně sezónní efekty](./media/machine-learning-r-quickstart/unnamed-chunk-20.png)

*Obrázek 24. Produkce mléka kalifornské s modelem včetně sezónní účinky.*

Hello shody toohello data zobrazená v 24 obrázek je spíš podporovat. Vyhledejte přiměřené hello sezónní efekt (měsíční variace) i hello trend.

Jako další kontrolu na našem modelu můžeme Podíváme se na toto políčko, budou hello. Hello následující kód výpočtů hello předpovězené hodnoty z našich dva modely, vypočítá hello zbytky hello sezónní modelu a poté ukazuje zeměpisný tyto zbytky pro hello Cvičná data.

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot hello residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

vykreslení zbytkové Hello se zobrazí v obrázku 25.

![Toto políčko, budou hello sezónní modelu pro hello Cvičná data](./media/machine-learning-r-quickstart/unnamed-chunk-21.png)

*Obrázek 25. Toto políčko, budou hello sezónní modelu pro hello Cvičná data.*

Tyto zbytky vypadat přiměřené. Neexistuje žádná konkrétní struktura, s výjimkou hello účinku poklesu hello 2008-2009, který našeho modelu neanalyzuje zvlášť dobře.

vykreslení Hello znázorňuje obrázek 25 je užitečné pro zjišťování v hello toto políčko, budou všechny vzory závislá na čase. Hello explicitní přístup výpočetních a vykreslení hello toto políčko, budou I používá umístí hello toto políčko, budou v pořadí časů na vykreslení hello. Pokud na hello druhé straně I měl vykreslí `milk.lm$residuals`, vykreslení hello by byly v pořadí čas.

Můžete také použít `plot.lm()` tooproduce řady diagnostických pozemků.

    ## Show hello diagnostic plots for hello model
    plot(milk.lm2, ask = FALSE)

Tento kód vytvoří řady diagnostických pozemků znázorňuje obrázek 26.

![První diagnostiky pozemků pro model sezónní hello](./media/machine-learning-r-quickstart/unnamed-chunk-221.png)

![Druhý diagnostiky pozemků pro model sezónní hello](./media/machine-learning-r-quickstart/unnamed-chunk-222.png)

![Třetí diagnostiky pozemků pro model sezónní hello](./media/machine-learning-r-quickstart/unnamed-chunk-223.png)

![Čtvrtý diagnostiky pozemků pro model sezónní hello](./media/machine-learning-r-quickstart/unnamed-chunk-224.png)

*Obrázek 26. Diagnostika ukazuje zeměpisný hello sezónní modelu.*

Existuje několik vysoké míry bodů, které jsou určené v těchto pozemků, ale nic toocause velmi významný. Navíc jsme můžete zobrazit z hello normální Q-Q výkresu že hello toto políčko, budou se zavřít toonormally distribuována, důležité předpokládá pro lineární modely.

### <a name="forecasting-and-model-evaluation"></a>Vyhodnocení prognózy a modelu
Existuje pouze jeden další toocomplete toodo věc našem příkladu. Budeme potřebovat toocompute prognózy a měření hello chyba proti hello skutečná data. Naše prognózy bude pro hello 12 měsíců od 2013. Jsme můžete vypočítat k chybě měr pro prognózy toohello skutečné dat, která není součástí naší datové sadě školení. Kromě toho jsme můžete porovnat výkon na hello 18 let školení data toohello dobu 12 měsíců testovacích dat.  

Se používá počet metriky výkonu hello toomeasure modelů časové řady. V našem případě použijeme chyba hello kořenové směrodatná (RMS). Hello následující funkce vypočítá chybě hello RMS mezi dvou řad.  

    RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
      ## Function toocompute hello RMS error or difference between two
      ## series or vectors

      messages <- c("ERROR: Input arguments toofunction RMS.error of wrong type encountered",
                    "ERROR: Input vector toofunction RMS.error is too short",
                    "ERROR: Input vectors toofunction RMS.error must be of same length",
                    "WARNING: Funtion rms.error has received invald input time series.")

      ## Check hello arguments
      if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
        warning(messages[1])
        return(NA)}

      if(length(series1) < min.length) {
        warning(messages[2])
        return(NA)}

      if((length(series1) != length(series2))) {
           warning(messages[3])
        return(NA)}

      ## If is.log is TRUE exponentiate hello values, else just copy
      if(is.log) {
        tryCatch( {
          temp1 <- exp(series1)
          temp2 <- exp(series2) },
          error = function(e){warning(messages[4]); NA}
        )
      } else {
        temp1 <- series1
        temp2 <- series2
      }

     ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute hello RMS error in a dataframe
      tryCatch( {
        sqrt(sum((temp1 - temp2)^2) / length(temp1))},
        error = function(e){warning(messages[4]); NA})
    }

Stejně jako u hello `log.transform()` hello funkce jsme popsané v části "Hodnoty transformace", je poměrně velké množství kontrolu a výjimky obnovení kód chyby v této funkci. Principy Hello těmto nekompatibilitám jsou hello stejné. Hello práci na dvou místech uzavřen do `tryCatch()`. Nejprve hello časové řady jsou exponentiated, protože Pracujeme s protokoly hello hello hodnot. Druhý se počítá hello skutečné chybové RMS.  

Vybaven funkce toomeasure hello chybu RMS, umožňuje vytvářet a výstupní dataframe, obsahující chyby hello RMS. Jsme bude obsahovat podmínky pro model trend hello samostatně a úplný model hello sezónní faktory. Hello následující kód hello úlohy pomocí hello dva lineární modely, které jsme mít sestavený.

    ## Compute hello RMS error in a dataframe
    ## Include hello row names in hello first column so they will
    ## appear in hello output of hello Execute R Script
    RMS.df  <-  data.frame(
    rowNames = c("Trend Model", "Seasonal Model"),
      Traing = c(
      RMS.error(predict1[1:216], cadairydata$Milk.Prod[1:216]),
      RMS.error(predict2[1:216], cadairydata$Milk.Prod[1:216])),
      Forecast = c(
        RMS.error(predict1[217:228], cadairydata$Milk.Prod[217:228]),
        RMS.error(predict2[217:228], cadairydata$Milk.Prod[217:228]))
    )
    RMS.df

    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('RMS.df')

Spuštění tohoto kódu vytváří výstup hello znázorňuje obrázek 27 na hello výstupním portem datové sady výsledků.

![Porovnání RMS chyb pro modely hello][26]

*Obrázek 27. Porovnání RMS chyb pro modely hello.*

Z těchto výsledků vidíte, že přidání hello sezónní faktory toohello model snižuje chyby RMS hello výrazně. Příliš logicky hello RMS chybě hello cvičení dat je bit menší než hello prognózy.

## <a id="appendixa"></a>Příloha A: Průvodce tooRStudio
Rstudia je velmi dobře zdokumentovat, tak v tomto dodatku I zajistí některé odkazy toohello klíče oddílů tooget hello Rstudia dokumentace, kterou jste zahájili.

1. Vytváření projektů
   
   Můžete uspořádat a spravovat váš kód R do projektů pomocí Rstudia. Hello dokumentace, která používá projekty lze najít na https://support.rstudio.com/hc/articles/200526207-Using-Projects.
   
   Doporučujeme I postupujte podle těchto pokynů a vytvořte projekt hello R příklady kódu v tomto dokumentu.  
2. Úpravy a spouštění kódu jazyka R
   
   Rstudia poskytuje integrované prostředí pro úpravy a provádění kódu jazyka R. Dokumentace lze najít na https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.
3. Ladění
   
   Rstudia zahrnuje výkonné možnosti ladění. Dokumentace pro tyto funkce jsou v https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.
   
   řešení potíží Funkce Hello zarážek jsou popsány v https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.

## <a id="appendixb"></a>Příloha B: Další čtení
Tato R programování hello kurz obsahuje základní informace o tom, co jste třeba toouse hello jazyk R s Azure Machine Learning Studio. Pokud nejste obeznámeni s R, jsou k dispozici na CRAN dva úvodní informace:

* R pro začátečníky podle Emmanuel Paradis je vhodné místo toostart v http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.  
* Úvod tooR n. dokončeno Venables et. Al. Klient se přepne do trochu další hloubku, http://cran.r-project.org/doc/manuals/R-intro.html.

Neexistují mnoho knih na R, který můžete začít pracovat. Zde najdete několik, které užitečné:

* Hello obrázky programování R: A prohlídka z statistické softwaru návrh podle Norman Matloff je vynikající Úvod tooprogramming v jazyce R.  
* R kuchařka podle Paul Teetor poskytuje problém a jeho řešení přístup toousing R.  
* R v akce Robert Kabacoff je další užitečné úvodní adresáře. Hello doprovodné rychlé R webu je užitečné prostředek v http://www.statmethods.net/.
* R Inferno podle Patrik popáleniny je, že je k dispozici zdarma http://www.burns-stat.com/documents/books/the-r-inferno/ překvapivě vážný adresáře, která pracuje s počtem složité a obtížně témata, která může být zjistil při programování v R. hello adresáře.
* Pokud chcete podrobné informace do Pokročilá témata v R, podívejte se na hello kniha Upřesnit R podle Hadley Wickham. je k dispozici zdarma v http://adv-r.had.co.nz/ Hello online verze této příručky.

Katalogu R časové řady balíčků naleznete v zobrazení úlohy CRAN hello pro analýzu časových řad: http://cran.r-project.org/web/views/TimeSeries.html. Informace o určité časové řady objekt balíčky by měl naleznete v dokumentaci toohello pro tento balíček.

Příručka Hello úvodní časové řady s R Paul Cowpertwait a Andrew Metcalfe obsahuje úvod toousing R pro analýzu časových řad. Mnoho více teoretické texty R příklady.

Některé skvělé prostředků z Internetu:

* DataCamp: DataCamp učí R v hello pohodlí prohlížeč s video lekce a kódování cvičení. Existují interaktivní kurzy o hello nejnovější R technik a balíčků. Přijmout hello volné interaktivní R kurz na https://www.datacamp.com/courses/introduction-to-r
* Průvodce Začínáme pracovat s R z Programiz https://www.programiz.com/r-programming
* Rychlý kurz R podle Jan černé z Clarkson univerzity http://www.cyclismo.org/tutorial/R/
* 60 + R prostředky uvedené v http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html

<!--Image references-->
[1]: ./media/machine-learning-r-quickstart/fig1.png
[2]: ./media/machine-learning-r-quickstart/fig2.png
[3]: ./media/machine-learning-r-quickstart/fig3.png
[4]: ./media/machine-learning-r-quickstart/fig4.png
[5]: ./media/machine-learning-r-quickstart/fig5.png
[6]: ./media/machine-learning-r-quickstart/fig6.png
[7]: ./media/machine-learning-r-quickstart/fig7.png
[8]: ./media/machine-learning-r-quickstart/fig8.png
[9]: ./media/machine-learning-r-quickstart/fig9.png
[10]: ./media/machine-learning-r-quickstart/fig10.png
[11]: ./media/machine-learning-r-quickstart/fig11.png
[12]: ./media/machine-learning-r-quickstart/fig12.png
[13]: ./media/machine-learning-r-quickstart/fig13.png
[14]: ./media/machine-learning-r-quickstart/fig14.png
[15]: ./media/machine-learning-r-quickstart/fig15.png
[16]: ./media/machine-learning-r-quickstart/fig16.png
[17]: ./media/machine-learning-r-quickstart/fig17.png
[18]: ./media/machine-learning-r-quickstart/fig18.png
[19]: ./media/machine-learning-r-quickstart/fig19.png
[20]: ./media/machine-learning-r-quickstart/fig20.png
[21]: ./media/machine-learning-r-quickstart/fig21.png
[22]: ./media/machine-learning-r-quickstart/fig22.png

[26]: ./media/machine-learning-r-quickstart/fig26.png

<!--links-->
[appendixa]: #appendixa
[download]: https://azurebigdatatutorials.blob.core.windows.net/rquickstart/RFiles.zip


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
