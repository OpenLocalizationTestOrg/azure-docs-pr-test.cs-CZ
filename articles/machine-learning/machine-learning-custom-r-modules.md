---
title: "aaaAuthor vlastních modulů R v Azure Machine Learning | Microsoft Docs"
description: "Rychlý start pro vytváření vlastních modulů R v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6cbc628a-7e60-42ce-9f90-20aaea7ba630
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 03/24/2017
ms.author: bradsev;ankarlof
ms.openlocfilehash: 8007c2abe20a4ab990f38b6d09bc4e6834ad2082
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="author-custom-r-modules-in-azure-machine-learning"></a>Vytváření vlastních modulů R ve službě Azure Machine Learning
Toto téma popisuje, jak tooauthor a nasadit vlastní modul R v Azure Machine Learning. Vysvětluje, co jsou vlastních modulů R a jaké soubory jsou použité toodefine je. Ukazuje, jak tooconstruct hello soubory, které definují modul a jak tooregister hello modul pro nasazení v pracovním prostoru Machine Learning. Hello elementy a atributy použité v definici hello vlastní modul hello jsou pak podrobněji popsané v. Jak toouse pomocné funkce a soubory a několik výstupů také popsané. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="what-is-a-custom-r-module"></a>Co je vlastní modul R?
A **vlastní modul** je modul definovaný uživatelem, který může být nahrán tooyour prostoru a provést v rámci experimentu Azure Machine Learning. A **vlastní modul R** je vlastní modul, který provede uživatelsky definované funkce R. **R** je programovací jazyk pro statistické výpočty a obrázky, které se často používá podle vědců statistikami a dat pro implementaci algoritmy. V současné době R je jediným podporovaným v vlastní moduly, ale podpory pro další jazyky je naplánována na budoucích verzích jazykem hello.

Vlastní moduly mají **prvotřídní stav** v Azure Machine Learning v hello smyslu, že je lze použít stejně jako ostatní moduly. Mohou být provedeny další moduly zahrnuté v publikované experimenty nebo vizualizace. Mít kontrolu nad hello algoritmus implementovaný modulem hello, hello používá toobe vstupní a výstupní porty, hello modelování parametry a další různé chování za běhu. Experimentu, který obsahuje vlastní moduly můžete také publikovat do hello Cortana Intelligence Gallery pro snadné sdílení.

## <a name="files-in-a-custom-r-module"></a>Soubory ve vlastní modul R
Vlastní modul R je definováno soubor .zip, který obsahuje minimálně dva soubory:

* A **zdrojový soubor** , která implementuje funkce hello R vystavené hello modulu
* **Soubor definice XML** rozhraní hello vlastní modul, který popisuje

Další pomocné soubory můžou být součástí hello souboru .zip, který poskytuje funkce, která je přístupná z vlastní modul hello. Tato možnost je podrobněji hello **argumenty** součástí hello referenčním oddílu **elementů v souboru definice XML hello** následující ukázka hello rychlý start.

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a>Příklad rychlý start: Definujte, balíčků a zaregistrovat vlastní modul R
Tento příklad ukazuje, jak tooconstruct hello soubory vyžadované vlastní modul R, zabalit do souboru zip a pak zaregistrovat hello modulu v pracovním prostoru Machine Learning. Hello příklad zip balíčku a ukázkové soubory si můžete stáhnout z [CustomAddRows.zip stáhnout soubor](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).

## <a name="hello-source-file"></a>zdrojový soubor Hello
Zvažte příklad hello **řádky přidat vlastní** modul, který upravuje hello standardní implementace hello **přidat řádky** modul používá tooconcatenate řádky (připomínky) z dvě datové sady (datových rámců). Hello standardní **přidat řádky** modulu připojí hello řádky hello druhý vstupní datové sady toohello konce hello první vstupní datové sady pomocí hello `rbind` algoritmus. Hello přizpůsobit `CustomAddRows` funkce podobně přijímá dvě datové sady, ale také přijme parametr Boolean odkládacího souboru jako další vstup. Pokud je parametr swap hello nastaven příliš**FALSE**, se vrátí hello sada dat jako hello standardní implementace. Ale pokud je parametr swap hello **TRUE**, funkce hello připojí řádky první vstupní datové sady toohello konce druhý datovou sadu hello místo. Hello CustomAddRows.R soubor, který obsahuje hello implementace hello R `CustomAddRows` funkce vystavené hello **řádky přidat vlastní** modul má následující kód R hello.

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

### <a name="hello-xml-definition-file"></a>Soubor definice XML Hello
tooexpose to `CustomAddRows` funkce jako modul služby Azure Machine Learning, soubor definice XML musí být vytvořen toospecify jak hello **řádky přidat vlastní** modul by měl vzhled a chování. 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother. Dataset 2 is concatenated tooDataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>

    <!-- Specify hello base language, script file and R function toouse for this module. -->        
        <Language name="R" 
         sourceFile="CustomAddRows.R" 
         entryPoint="CustomAddRows" />  

    <!-- Define module input and output ports -->
    <!-- Note: hello values of hello id attributes in hello Input and Arg elements must match hello parameter names in hello R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>hello combined dataset</Description>
            </Output>
        </Ports>

    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>


Je důležité toonote, který hello hodnotu hello **id** atributy hello **vstup** a **Arg** elementů v souboru XML hello musí odpovídat názvy parametrů funkce hello hello R kód v souboru CustomAddRows.R hello přesně: (*dataset1*, *dataset2*, a *swap* v příkladu hello). Podobně hello hodnotu hello **entryPoint** atribut hello **jazyk** element musí přesně shodovat hello název funkce hello ve skriptu hello R: (*CustomAddRows* v příkladu hello). 

Naproti tomu hello **id** atribut pro hello **výstup** element neodpovídá tooany proměnné ve skriptu hello R. Když se vyžaduje více než jeden výstup, jednoduše vrácení seznamu z funkce hello R s výsledky umístit *v hello stejné pořadí* jako **výstupy** elementy jsou deklarované v souboru XML hello.

### <a name="package-and-register-hello-module"></a>Balíček a registrace modulu hello
Uložit tyto dva soubory jako *CustomAddRows.R* a *CustomAddRows.xml* a pak zip hello dva soubory společně na *CustomAddRows.zip* souboru.

je v pracovním prostoru Machine Learning, přejděte tooyour pracovního prostoru v hello Machine Learning Studio, klikněte na tlačítko hello tooregister **+ nový** tlačítko hello dolní a zvolte **modulu -> z balíček ZIP** tooupload Hello nové **řádky přidat vlastní** modulu.

![Nahrát Zip](./media/machine-learning-custom-r-modules/upload-from-zip-package.png)

Hello **řádky přidat vlastní** modul je nyní připraven toobe přístup experimentů Machine Learning.

## <a name="elements-in-hello-xml-definition-file"></a>Elementy v souboru definice XML hello
### <a name="module-elements"></a>Modul elementy
Hello **modulu** element je použité toodefine vlastní modul v souboru XML hello. Více modulů lze definovat v jednom souboru XML pomocí několika **modulu** elementy. Každý modulu v pracovním prostoru musí mít jedinečný název. Registrovat vlastní modul s hello stejný název jako existující vlastní modul a jeho nahradí existující modul hello hello nový. Vlastní moduly lze však zaregistrována hello stejný název jako existující modul Azure Machine Learning. Pokud ano, se objeví v hello **vlastní** kategorii palety modulů hello.

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother...</Description>/> 


V rámci hello **modulu** elementu, můžete určit dva další volitelné prvky:

* **vlastníka** element, který se vloží do modulu hello  
* **popis** element, který obsahuje text, který se zobrazí v rychlé nápovědy pro modul hello a pokud ponecháte hello modulu v hello Machine Learning uživatelského rozhraní.

Pravidla pro omezení znaků v elementech modulu hello:

* Hello hodnotu hello **název** atribut v hello **modulu** element nesmí být delší než 64 znaků. 
* Hello obsah hello **popis** element nesmí být delší než 128 znaků.
* Hello obsah hello **vlastníka** element nesmí být delší než 32 znaků.

Výsledky modulu může být deterministický nebo nondeterministic.* * ve výchozím nastavení, všechny moduly jsou považovány za toobe deterministický. To znamená, zadána neměnné sadu vstupní parametry a data modulu hello by měl vrátit hello stejné výsledky eacRAND nebo functionh čas, který je spuštěn. Vzhledem k tomuto chování, Azure Machine Learning Studio pouze opakovaně moduly, které jsou označeny jako deterministický, pokud parametr nebo hello vstupní data změnila. Vrací výsledky do mezipaměti hello taky poskytuje mnohem rychlejší provádění experimenty.

Jsou funkce, které jsou deterministický, jako je například rand – nebo funkci, která vrátí hello aktuální datum nebo čas. Pokud modul používá nedeterministická funkce, můžete určit, že hello modul bude nedeterministickou podle nastavení hello volitelné **isDeterministic** atribut příliš**FALSE**. Díky tomu se budou, že Tenhle modul hello se znovu spustí při každém spuštění experimentu hello i nezměněné hello vstup modulu a parametry. 

### <a name="language-definition"></a>Definice jazyka
Hello **jazyk** element v souboru definice XML je použité toospecify hello vlastní modul jazyk. V současné době R je hello pouze podporovaný jazyk. Hello hodnotu hello **zdrojový soubor** atributu musí být název hello hello R souboru, který obsahuje hello funkce toocall při spuštění modulu hello. Tento soubor musí být součástí hello zip balíčku. Hello hodnotu hello **entryPoint** atribut je název hello hello funkce volané a platný funkci definovanou s v hello zdrojový soubor se musí shodovat.

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a>Porty
Hello vstupní a výstupní porty pro vlastní modul jsou určené v podřízených elementů hello **porty** část definičního soubor XML hello. Určuje pořadí Hello tyto prvky hello rozložení zkušeného (UX) uživatelé. prvním podřízeným objektem Hello **vstupní** nebo **výstup** uvedené v hello **porty** element souboru XML hello stane hello nejvíce vlevo vstupní port v hello Machine Learning UX
Každý vstupní a výstupní port je pravděpodobně volitelný **popis** podřízený element, který určuje textu hello zobrazí, když najedete myší hello hello port v hello Machine Learning uživatelského rozhraní.

**Porty pravidla**:

* Maximální počet **vstupní a výstupní porty** je 8 pro každou.

### <a name="input-elements"></a>Elementy vstupu
Vstupní porty povolí funkci tooyour R toopass dat a pracovního prostoru. Hello **datové typy** jsou podporovány pro vstupní porty jsou následujícím způsobem: 

**DataTable:** tento typ je předán tooyour R funkce jako data.frame. Ve skutečnosti všechny typy (například CSV soubory nebo soubory ARFF), které jsou podporovány nástrojem Machine Learning a že jsou kompatibilní s **DataTable** jsou převedené tooa data.frame automaticky. 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

Hello **id** atribut spojený s každou **DataTable** vstupní port musí mít jedinečnou hodnotu a tato hodnota musí odpovídat jeho odpovídající parametr v R funkce s názvem.
Volitelné **DataTable** porty, které nejsou předány jako vstup v experimentu mít hodnotu hello **NULL** funkce předaný toohello R a volitelné zip porty jsou ignorovány, pokud hello vstup není připojen. Hello **isOptional** atribut je volitelný pro obě hello **DataTable** a **Zip** typy a je *false* ve výchozím nastavení.

**ZIP:** vlastní moduly může přijmout soubor zip jako vstup. Tento vstup je vybaleno do hello R pracovní adresář vaší – funkce

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files toobe extracted toohello R working directory.</Description>
           </Input>

Pro vlastních modulů R hello id pro Zip port nemá toomatch žádné parametry hello R funkce. Je to proto, že je soubor zip hello automaticky extrahované toohello R pracovní adresář.

**Vstupní pravidla:**

* Hello hodnotu hello **id** atribut hello **vstup** element musí být platný název proměnné R.
* Hello hodnotu hello **id** atribut hello **vstup** element nesmí být delší než 64 znaků.
* Hello hodnotu hello **název** atribut hello **vstup** element nesmí být delší než 64 znaků.
* Hello obsah hello **popis** element nesmí být delší než 128 znaků.
* Hello hodnotu hello **typ** atribut hello **vstup** element musí být *Zip* nebo *DataTable*.
* Hello hodnotu hello **isOptional** atribut hello **vstup** element není požadovaná (a je *false* ve výchozím nastavení není-li zadána); ale pokud je zadán, musí být *true* nebo *false*.

### <a name="output-elements"></a>Výstup elementy
**Standardní výstupní porty:** výstupních portů jsou namapované toohello návratové hodnoty z funkce R, který můžete použít následující moduly. *DataTable* hello pouze standardní výstupní port typu v současné době podporován. (Podpora pro *inteligentních algoritmů* a *transformuje* je chystaný.) A *DataTable* výstup je definován jako:

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

Výstupy ve vlastních modulů R text hello hodnotu hello **id** atribut nemá toocorrespond s nic v hello R skript, ale musí být jedinečný. Pro jeden modul výstupu, musí být hello návratovou hodnotou z funkce hello R *data.frame*. V pořadí toooutput více než jeden objekt podporovaného typu, hello odpovídající výstupních portů musíte toobe zadaný v souboru definice XML hello a objekty hello musí toobe vrácena jako seznam. objekty výstup Hello přiřazené toooutput porty z levé tooright, které odráží hello pořadí, ve kterém jsou umístěny hello objekty v hello vrátil seznam.

Například, pokud chcete, aby toomodify hello **řádky přidat vlastní** toooutput modulu hello původní dvě datové sady, *dataset1* a *dataset2*, kromě připojený k nové toohello datové sady, *datovou sadu*, (v pořadí, z levé tooright jako: *datovou sadu*, *dataset1*, *dataset2*), pak zadejte hello výstup porty v souboru CustomAddRows.xml hello následujícím způsobem:

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


A vrátí hello seznam objektů v seznamu ve správném pořadí hello v 'CustomAddRows.R':

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

**Vizualizace výstup:** můžete také určit na výstupní port typu *vizualizace*, které zobrazí výstup hello hello R grafiky zařízení a konzoly výstup. Tento port není součástí funkce výstup hello R a nebudou v konfliktu s hello pořadí hello další výstup typy portu. tooadd vizualizace port toohello vlastní moduly, přidejte **výstup** element s hodnotou *vizualizace* pro jeho **typ** atribut:

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View hello R console graphics device output.</Description>
    </Output>

**Výstup pravidla:**

* Hello hodnotu hello **id** atribut hello **výstup** element musí být platný název proměnné R.
* Hello hodnotu hello **id** atribut hello **výstup** element nesmí být delší než 32 znaků.
* Hello hodnotu hello **název** atribut hello **výstup** element nesmí být delší než 64 znaků.
* Hello hodnotu hello **typ** atribut hello **výstup** element musí být *vizualizace*.

### <a name="arguments"></a>Argumenty
Další data mohou být předána toohello R funkce prostřednictvím modulu parametry, které jsou definovány v hello **argumenty** elementu. Tyto parametry se zobrazí v podokně nejvíce vpravo vlastnosti hello hello Machine Learning uživatelského rozhraní při vybrání hello modulu. Argumenty může být jakýkoli z typů hello podporované nebo můžete vytvořit vlastní výčet v případě potřeby. Podobné toohello **porty** elementů **argumenty** elementy může mít volitelný **popis** element, který určuje text hello, která se zobrazuje při přesunutí ukazatele myši hello přes hello název parametru.
Volitelné vlastnosti pro modul, jako je například výchozí hodnota, minValue a maxValue přidáním tooany argument jako atributy tooa **vlastnosti** elementu. Platný vlastnosti hello **vlastnosti** element závisí na typu argument hello a jsou popsané s typy argumentů hello podporované v další části hello. Argumenty s hello **isOptional** vlastností nastavenou příliš**"true"** nevyžadují hello uživatele tooenter hodnotu. Pokud hodnota není k dispozici toohello argument, není předán hello argument funkce toohello vstupního bodu. Argumenty hello vstupní bod funkce, které mají toobe volitelné nutné explicitně zpracovávat hello funkce, například přiřadit výchozí hodnotu NULL v hello vstupní bod funkce definici. Nepovinný argument pouze vynutí hello jiná omezení argument, tj. min nebo max, pokud je hodnota zadaná uživatelem hello.
Stejně jako u vstupy a výstupy, je velmi důležité, že každý z parametrů hello mít jedinečné id hodnoty s nimi spojených. V našem úvodní příklad hello přidružené id a parametru byla *swap*.

### <a name="arg-element"></a>Arg – element
Parametr modulu je definován pomocí hello **Arg** podřízený element hello **argumenty** část definičního soubor XML hello. Stejně jako u hello podřízených elementů v hello **porty** část, hello řazení parametrů v hello **argumenty** oddíl definuje hello rozložení v hello UX Hello parametry jsou shora dolů v hello uživatelského rozhraní v hello stejné pořadí, ve kterém jsou definovány v souboru XML hello. Hello typy podporované nástrojem Machine Learning pro parametry jsou zde uvedeny. 

**int** – parametru typu (32 bitů) celé číslo.

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* *Volitelné vlastnosti*: **min**, **maximální**, **výchozí** a **isOptional**

**dvojité** – parametr typu double.

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* *Volitelné vlastnosti*: **min**, **maximální**, **výchozí** a **isOptional**

**BOOL** – logického parametru, která je reprezentována zaškrtávací políčko UX

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* *Volitelné vlastnosti*: **výchozí** -false v případě není nastavena.

**řetězec**: standardní řetězec

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* *Volitelné vlastnosti*: **výchozí** a **isOptional**

**ColumnPicker**: Parametr výběr sloupce. Tento typ se vykreslí v hello UX jako zvolit sloupce. Hello **vlastnost** element je id použité sem toospecify hello hello portu, ze které se vybírají sloupce, které musí být hello cílový port typ *DataTable*. výsledek Hello hello sloupců výběru se předá toohello R funkce jako seznam řetězců obsahující názvy sloupců hello vybrané. 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* *Požadované vlastnosti*: **portId** -odpovídá hello id elementu vstupu s typem *DataTable*.
* *Volitelné vlastnosti*:
  
  * **allowedTypes** -typy filtrů hello sloupec, ze kterého můžete vybrat. Platné hodnoty patří: 
    
    * číselné
    * Logická hodnota
    * Kategorické
    * Řetězec
    * Štítek
    * Funkce
    * Hodnocení
    * Všechny
  * **výchozí** -platný výchozí možnosti pro výběr sloupce hello patří: 
    
    * Žádný
    * NumericFeature
    * NumericLabel
    * NumericScore
    * NumericAll
    * BooleanFeature
    * BooleanLabel
    * BooleanScore
    * BooleanAll
    * CategoricalFeature
    * CategoricalLabel
    * CategoricalScore
    * CategoricalAll
    * StringFeature
    * StringLabel
    * StringScore
    * StringAll
    * AllLabel
    * AllFeature
    * AllScore
    * Všechny

**Rozevírací seznam**: seznam uživatelská výčtového (rozevírací). Hello rozevírací seznam položek, které jsou určené v rámci hello **vlastnosti** pomocí elementu **položky** elementu. Hello **id** pro každou **položky** musí být jedinečný a platná proměnná R. Hello hodnotu hello **název** z **položky** slouží jako text hello, který se zobrazí i hello hodnotu, která je předána toohello R funkce.

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* *Volitelné vlastnosti*:
  * **výchozí** – hello hodnota hello výchozí vlastnost, musí být shodná s hodnotou id z jednoho z hello **položky** elementy.

### <a name="auxiliary-files"></a>Pomocné soubory
Všechny soubory, které je umístěn v souboru ZIP vlastní modul je toobe má k dispozici pro použití v době spuštění. Všechny adresáře struktury přítomen se zachovají. To znamená, že stejný soubor zdrojové funguje hello místně a v Azure Machine Learning provádění. 

> [!NOTE]
> Všimněte si, zda jsou všechny soubory extrahované too'src' directory proto musí mít všechny cesty ' src /' předponu.
> 
> 

Předpokládejme například že chcete tooremove žádné řádky s NAs z hello datové sady a taky před výstup do CustomAddRows odeberte všechny duplicitní řádky, a jste již zapsány R funkce, která nemá v souboru RemoveDupNARows.R:

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
Zdrojem může být hello pomocného souboru RemoveDupNARows.R v hello CustomAddRows funkce:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
             } else { 
                  dataset <- rbind(dataset1, dataset2)) 
             } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

V dalším kroku nahrajte soubor zip obsahující 'CustomAddRows.R', 'CustomAddRows.xml' a 'RemoveDupNARows.R' jako vlastní modul R.

## <a name="execution-environment"></a>Prostředí pro spuštění
Hello prostředí pro spuštění skriptu hello R hello používá stejnou verzi R jako hello **spustit skript jazyka R** modulu a můžete použít hello stejné výchozí balíčky. Můžete také přidat další balíčky tooyour vlastní modul R zahrnutím hello vlastní modul zip balíčku. Jenom je načte ve vašem skriptu R stejně jako v prostředí R. 

**Omezení prostředí pro spuštění hello** zahrnují:

* Systém souborů bez trvalé: soubory zapsané při spuštění vlastního modulu hello nejsou trvalé napříč více spustí hello stejném modulu.
* Žádný přístup k síti

