---
title: "aaaGuide toohello Net # Neuronové sítě Specification Language | Microsoft Docs"
description: "Syntaxe pro hello Net # neuronové sítě specifikace jazyka, společně s příklady jak toocreate vlastní neuronové sítě modelu v Microsoft Azure ML pomocí Net #"
services: machine-learning
documentationcenter: 
author: jeannt
manager: jhubbard
editor: cgronlun
ms.assetid: cfd1454b-47df-4745-b064-ce5f9b3be303
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeannt
ms.openlocfilehash: 3493247ecc39ca3a1382510ad520d7017159ff62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guide-toonet-neural-network-specification-language-for-azure-machine-learning"></a>Průvodce jazyk specifikace neuronové sítě tooNet # pro Azure Machine Learning
## <a name="overview"></a>Přehled
NET # je jazyk vyvinuté společností Microsoft, který je použité toodefine neuronové sítě architektury. Můžete použít Net # v modulech neuronové sítě v Microsoft Azure Machine Learning.

<!-- This function doesn't currentlyappear in hello MicrosoftML documentation. If it is added in a future update, we can uncomment this text.

, or in hello `rxNeuralNetwork()` function in [MicrosoftML](https://msdn.microsoft.com/microsoft-r/microsoftml/microsoftml). 

-->

V tomto článku se dozvíte, že základní koncepty toodevelop vlastní neuronové sítě: 

* Požadavky na neuronové sítě a jak toodefine hello primární součásti
* Syntaxe Hello a klíčová slova z hello Net # specifikace jazyka
* Příklady vlastních neuronové sítě vytvořené pomocí Net # 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="neural-network-basics"></a>Základy neuronové sítě
Struktura neuronové sítě se skládá z ***uzly*** , jsou uspořádány do ***vrstvy***a vyvážené ***připojení*** (nebo ***okraje***) mezi Hello uzly. směrovou Hello připojení a má každé připojení ***zdroj*** uzlu a ***cílové*** uzlu.  

Každý ***trainable vrstvy*** (skrytý nebo vrstvu výstup) má jednu nebo více ***sady připojení***. Sady připojení se skládá z vrstvy zdroje a specifikaci hello připojení z této zdrojové vrstvy. Všechna připojení hello ve sdílené složce dané sady hello stejné ***zdrojové vrstvy*** a hello stejné ***cílové vrstvě***. V Net # považuje připojení sady jako vrstva cílové sady toohello patřící.  

NET # podporuje různé druhy sad připojení, která vám umožňuje přizpůsobit způsob hello vstupy jsou namapované toohidden vrstvy a namapované toohello výstupy.   

Výchozí Hello nebo standardní sady **úplné sady**, v každý uzel v hello zdrojové vrstvy je uzel připojených tooevery ve vrstvě cílové hello.  

Kromě toho Net # podporuje hello následující čtyři typy sad typ připojení:  

* **Filtrované sady**. Hello uživatele můžete definovat predikát pomocí hello umístění hello zdrojový vrstvy uzel a hello cílový vrstvy uzel. Uzly jsou připojené vždy, když hello predikát má hodnotu True.
* **Convolutional sady**. Hello uživatel může definovat malé sousedství uzlů ve vrstvě zdroj hello. Každý uzel ve vrstvě cílové hello je okolí připojených tooone uzlů ve vrstvě zdroj hello.
* **Sdružování sady** a **odpovědi normalizaci sady**. Jedná se o podobnou tooconvolutional sady v této hello uživatele definuje malé sousedství uzlů ve vrstvě zdroj hello. Hello rozdílem je, že nejsou trainable hello váhu hello okraje v těchto sad. Místo toho použita předdefinované funkce toohello zdrojový uzel hodnoty toodetermine hello cílový uzel hodnotu.  

Pomocí Net # toodefine hello struktura neuronové sítě je možné toodefine komplexní struktury například hluboké neuronové sítě nebo convolutions libovolný dimenzí, které jsou známé tooimprove programování pro data, jako jsou bitové kopie, zvuk a video.  

## <a name="supported-customizations"></a>Podporované vlastní nastavení
Architektura Hello modelů neuronové sítě, které vytvoříte v Azure Machine Learning lze hojně přizpůsobit pomocí Net #. Můžete:  

* Vytvořte skryté vrstvy a řízení hello počet uzlů v každé vrstvě.
* Zadejte, jak jsou toobe připojené tooeach další vrstvy.
* Definujte struktury speciální připojení, jako je například convolutions a váhy sdílení sady.
* Zadejte jiný aktivace funkce.  

Podrobné informace o syntaxi jazyka hello specifikace, najdete v části [struktura specifikace](#Structure-specifications).  

Příklady definice neuronové sítě pro některé běžné strojového učení úlohy z nezpracovávají toocomplex, najdete v části [příklady](#Examples-of-Net#-usage).  

## <a name="general-requirements"></a>Obecné požadavky
* Musí být přesně jeden výstup vrstvě, alespoň jeden vstupní a v počtu nula či více skryté vrstvy. 
* Jednotlivé úrovně má pevný počet uzlů, koncepčně uspořádané obdélníková pole libovolný dimenzí. 
* Vstupní vrstvy mít žádné přidružené vyškolení parametry a představují hello místa, kde instance data do sítě hello. 
* Trainable vrstvy (hello skryté a výstupní vrstvy) mají přiřazeny vyškolení parametry, označuje jako váhu a tendence z. 
* zdrojové a cílové uzly Hello musí být v samostatné vrstvy. 
* Připojení musí být Acyklické; jinými slovy nemůže být řetězec připojení úvodní back toohello počáteční zdrojový uzel.
* vrstva výstup Hello nesmí být zdroje vrstvu sady připojení.  

## <a name="structure-specifications"></a>Struktura specifikace
Struktura specifikaci neuronové sítě se skládá ze tří částí: hello **deklarace konstanty**, hello **layer prohlášení**, hello **připojení deklarace**. K dispozici je také volitelný **sdílet deklarace** části. Hello oddílů lze zadat v libovolném pořadí.  

## <a name="constant-declaration"></a>Deklarace konstant
Deklarace konstanty je volitelný. Poskytuje znamená toodefine hodnoty používané jinde v definici hello neuronové sítě. příkaz deklarace Hello se skládá z identifikátor následuje znak rovná a výraz hodnotu.   

Například následující příkaz hello definuje konstanta **x**:  

    Const X = 28;  

toodefine dvou nebo více konstant současně, uzavřete hello identifikátor názvy a hodnoty do složených závorek a oddělte je středníky. Například:  

    Const { X = 28; Y = 4; }  

pravé straně Hello každé přiřazení výrazu může být celé číslo, reálné číslo, logickou hodnotu (True nebo False) nebo matematickém výrazu. Například:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Deklarace vrstvy
vyžaduje se Hello layer prohlášení. Určuje velikost hello a zdroj hello vrstvy, včetně sady připojení a atributy. Dobrý den deklarace příkaz začíná hello název vrstvy hello (vstup, skrytá nebo výstupní), za nímž následuje hello dimenze hello vrstvy (řazená kolekce členů kladná celá čísla). Například:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

* produkt Hello hello dimenzí je hello počet uzlů ve vrstvě hello. V tomto příkladu se dvěma rozměry [5,20], což znamená, že se 100 uzly ve vrstvě hello.
* Hello vrstvy lze deklarovat v libovolném pořadí, s jednou výjimkou: Pokud je definován více než jeden vstupní vrstvy, hello pořadí, ve které jsou deklarovány musí odpovídat pořadí hello funkcí ve vstupní data hello.  

toospecify, které bude hello počet uzlů ve vrstvě určen automaticky, použijte hello **automaticky** – klíčové slovo. Hello **automaticky** – klíčové slovo má jiný důsledky, v závislosti na hello vrstvy:  

* V deklaraci vstupní vrstvy hello počet uzlů je hello řadu funkcí v hello vstupní data.
* V deklaraci skryté vrstvě hello počet uzlů je hello čísla, která je zadána hodnota parametru hello pro **počet skryté uzly**. 
* V deklaraci vrstvy výstup hello počet uzlů je 2 pro dvě třídy klasifikaci, 1 pro regresní a rovna toohello počet výstupních uzlů pro více třídami klasifikaci.   

Například hello následující definice sítě umožňuje hello velikost všech vrstev toobe automaticky určit:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


Layer prohlášení pro trainable vrstvu (hello skrytý nebo výstupní vrstev) může volitelně obsahovat hello výstup funkce (také nazývané aktivace funkce), výchozí nastavení je příliš**sigmoid** klasifikační modely a **lineární** pro regresní modely. (I když používáte výchozí hello, vám může explicitně stavu hello aktivace funkce, v případě potřeby pro přehlednost.)

Hello výstup podporuje následující funkce:  

* sigmoid
* lineární
* softmax
* rlinear
* hranaté
* Sqrt
* srlinear
* Abs
* TANH 
* brlinear  

Například následující deklarace hello používá hello **softmax** funkce:  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Deklarace připojení
Okamžitě po definování hello trainable vrstvu, je potřeba deklarovat připojení mezi hello vrstvy, které jste definovali. Deklarace sady připojení Hello začíná textem hello – klíčové slovo **z**, za nímž následují hello název zdroje sady hello vrstvu a hello typ toocreate sady připojení.   

V současné době jsou podporovány pět druhy sad připojení:  

* **Úplné** sad, indikován hello – klíčové slovo **všechny**
* **Filtrované** sad, indikován hello – klíčové slovo **kde**, za nímž následují výraz predikátu
* **Convolutional** sad, indikován hello – klíčové slovo **convolve**, za nímž následují hello konvoluce atributy
* **Sdružování** sad, indikován klíčová slova hello **maximálního počtu fondu** nebo **znamenat fondu**
* **Odpověď normalizaci** sad, indikován hello – klíčové slovo **norm odpovědi**      

## <a name="full-bundles"></a>Úplné sady
Sady úplný připojovací zahrnuje připojení z každého uzlu v hello zdrojový vrstvy tooeach uzel ve vrstvě cílové hello. Toto je hello výchozí typ připojení.  

## <a name="filtered-bundles"></a>Filtrované sady
Specifikaci sady filtrovaném připojení zahrnuje predikátu, vyjádřené syntakticky, velmi podobně jako výrazu lambda C#. Hello následující příklad definuje dvě filtrované sady:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

* V hello predikát pro *ByRow*, **s** je parametr představující index do pole obdélníková hello uzlů hello vstupní vrstvy, *pixelů*, a **d**  je parametr představující index do pole hello uzlů ve skryté vrstvě hello, *ByRow*. Hello typ i **s** a **d** je celých čísel délky dvě řazené kolekce členů. Koncepčně **s** rozsahy přes všechny páry celých čísel s *0 < = [0] s < 10* a *0 < = s[1] < 20*, a **d**  rozsahy přes všechny páry celých čísel, s *0 < = [0] d < 10* a *0 < = d[1] < 12*. 
* Na pravé straně hello predikátem výrazu hello je podmínku. V tomto příkladu se pro každou hodnotu **s** a **d** tak, aby hello podmínka vyhodnocena jako True, je okraj z hello zdroj vrstvy uzlu toohello cílové vrstvy uzlu. Proto tento výraz filtru značí této sady hello obsahuje připojení z uzlu hello definované **s** toohello uzlu definované **d** ve všech případech, kde s [0] je rovna tood [0].  

Volitelně můžete zadat sadu váhu pro filtrovanou sadu. hodnota pro hello Hello **váhu** atribut musí být číslo s plovoucí čárkou hodnoty bodu s délkou, která odpovídá hello počet připojení definované sady hello řazená kolekce členů. Ve výchozím nastavení jsou váhu náhodně vygenerované.  

Hodnoty váhy jsou seskupené podle hello cílový uzel indexu. To znamená, pokud je připojený hello první cílový uzel trvalo zdroj uzly hello nejprve *tisíc* elementy hello **váhu** hello váhu pro hello první cílový uzel, v pořadí zdrojů indexu jsou řazené kolekce členů. Hello totéž platí i pro zbývající uzly cílové hello.  

Je možné toospecify váhu přímo jako konstantní hodnoty. Například pokud jste se naučili dříve hello váhu, můžete je zadat jako konstanty pomocí této syntaxe:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional sady
Pokud hello Cvičná data homogenní struktura, convolutional připojení jsou běžně používané toolearn nejdůležitější funkce hello data. Například v bitové kopii, data zvuku a videa, prostorových nebo dočasné dimenzionalitu může být poměrně uniform.  

Convolutional sady využívají obdélníková **jádra** , se posouvá prostřednictvím hello dimenzí. V podstatě každý jádra definuje sadu váhu použijí v místní sousedství, označují tooas **aplikací jádra**. Každá aplikace jádra odpovídá tooa uzlu ve vrstvě hello zdroje, které se označují tooas hello **centrálního uzlu**. Hello váhu jádro jsou sdílena mezi mnoha připojení. V sady convolutional je obdélníková každý jádra a všechny aplikace jádra jsou hello stejná velikost.  

Convolutional sady podporují hello následující atributy:

**InputShape** definuje hello dimenzionalitu hello vrstvy zdroj pro účely hello tento convolutional sady. Hello hodnota musí být kladná celá čísla řazená kolekce členů. Hello produktu celých čísel hello se musí rovnat hodnotě hello počet uzlů v hello zdrojové vrstvy, ale jinak, nemusí dimenzionalitu hello toomatch deklarovat pro hello zdrojové vrstvy. Délka Hello tento řazené kolekce členů stane hello **Arita** hodnotu convolutional sadě hello. (Obvykle Arita odkazuje toohello počet argumentů nebo operandy, které může provádět funkce.)  

toodefine hello tvar a umístění hello jádra, použít atributy hello **KernelShape**, **Stride**, **odsazení**, **LowerPad**, a **UpperPad**:   

* **KernelShape**: (povinné) dimenzionalitě hello definuje každý jádra convolutional sadě hello. Hello hodnota musí být kladná celá čísla o délce, který se rovná hello Arita sady hello řazená kolekce členů. Jednotlivé komponenty této řazené kolekce členů musí být větší než odpovídající komponenta hello **InputShape**. 
* **STRIDE**: (volitelné) definuje hello klouzavé krok velikosti hello konvoluce (jeden krok velikost každé dimenze), který je hello vzdálenost mezi uzly centrální hello. Hello hodnota musí být kladná celá čísla o délce, který je hello Arita sady hello řazená kolekce členů. Jednotlivé komponenty této řazené kolekce členů musí být větší než odpovídající komponenta hello **KernelShape**. Hello výchozí hodnota je řazené kolekce členů s všechny součásti rovna tooone. 
* **Sdílení**: (volitelné) definuje hello váhy sdílení Každá dimenze konvoluce hello. Hello hodnota může být jedna logická hodnota nebo řazená kolekce členů s délku, která je hello Arita sady hello logické hodnoty. Jednu logickou hodnotu je rozšířené toobe řazená kolekce členů správnou délku hello se všemi součástmi toohello rovna zadané hodnotě. Hello výchozí hodnota je řazené kolekce členů, která se skládá z všechny hodnoty True. 
* **MapCount**: (volitelné) definuje hello počet funkce mapuje convolutional sadě hello. Hello hodnota může být jeden kladné celé číslo nebo kladných celých čísel s délku, která je hello aritu hello sady řazené kolekce členů. Jeden celočíselná hodnota je rozšířeno toobe řazená kolekce členů hello správnou délku s hello první součástí stejné toohello zadaná hodnota a všechny zbývající součásti rovna tooone hello. Hello výchozí hodnota je 1. Celkový počet funkce mapy Hello je produkt hello součástí hello hello řazené kolekce členů. Hello řešení z celkový počet mezi komponentami hello určuje způsob seskupení hello funkce mapy hodnoty hello cílové uzly. 
* **Váhu**: (volitelné) definuje hello počáteční váhu sadě hello. Hello hodnota musí být číslo s plovoucí čárkou bodu hodnoty o délce, která je hello počet jader časy hello váhu za jádra, jak jsou definovány dále v tomto článku řazená kolekce členů. výchozí váhu Hello se náhodně vygenerované.  

Existují dvě sady vlastností, které řídí odsazení, vlastnosti hello se vzájemně vylučují:

* **Odsazení**: (volitelné) určuje, zda text hello vstupní vyplní pomocí **výchozí schéma odsazení**. Hello hodnota může být jednu logickou hodnotu, nebo může být logická hodnota hodnot s délku, která je hello aritu hello sady řazené kolekce členů. Jednu logickou hodnotu je rozšířené toobe řazená kolekce členů správnou délku hello se všemi součástmi toohello rovna zadané hodnotě. Pokud hello dimenze hodnotu True, zdroj hello logicky doplněno v této dimenzi s aplikacemi další jádra toosupport buňky s hodnotou nula, tak, aby hello centrální uzly hello první a poslední jádra v této dimenzi jsou hello první a poslední uzly v této dimenzi ve vrstvě zdroj hello. Proto hello počet "fiktivní" uzlů v Každá dimenze je přesně určit toofit automaticky, *(InputShape [d] - 1) / Stride [d] + 1* jádra do vrstvy hello vyplní zdroje. Pokud je hodnota hello dimenze hodnotu False, hello jádra jsou definovány tak, aby hello počet uzlů na každé straně, které jsou ponechány na stejné hello (až tooa rozdíl 1). Hello výchozí hodnota tohoto atributu je řazené kolekce členů s všechny součásti rovna tooFalse.
* **UpperPad** a **LowerPad**: (volitelné) zadejte větší kontrolu nad hello množství toouse odsazení. **Důležité:** tyto atributy lze definovat, pokud a pouze v případě hello **odsazení** vlastnost výše je ***není*** definované. Hello hodnoty musí být celé číslo s hodnotou řazené kolekce členů s délky, které jsou hello Arita sady hello. Jsou-li tyto atributy, "fiktivní" uzly se přidají toohello nižší a horní konec Každá dimenze hello vstup vrstvě. Hello počet uzlů přidány toohello nižší a horní končí v Každá dimenze je dáno **LowerPad**[i] a **UpperPad**[i] v uvedeném pořadí. musí být splněné tooensure zda jádra odpovídají pouze příliš "Skutečná" uzly a není příliš "fiktivní" uzly hello následující podmínky:
  * Jednotlivé komponenty **LowerPad** musí být striktně menší než KernelShape [d] / 2. 
  * Jednotlivé komponenty **UpperPad** musí být větší než KernelShape [d] / 2. 
  * Výchozí hodnota Hello těchto atributů je řazené kolekce členů s všechny součásti rovna too0. 

nastavení Hello **odsazení** = true umožňuje podle potřeby mnohem odsazení, jako je tookeep hello "center" hello jádra uvnitř hello "skutečných" vstup. Tato operace změní hello matematické trochu pro výpočty velikost výstup hello. Obecně platí, výstup hello velikost *D* se vypočítá jako *D = (I - tisíc) nebo S + 1*, kde *I* je vstupní velikost hello, *tisíc* je velikost hello jádra, *S* je hello stride a  */*  je dělení celého čísla (zaokrouhlí směrem k nule). Pokud nastavíte UpperPad = [1, 1], vstupní hello velikost *I* je efektivně 29 a proto *D = (29-5) nebo 2 + 1 = 13*. Ale když **odsazení** = true, v podstatě *I* získá bumped podle *K - 1*; proto *D = ((28 + 4) – 5) nebo 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14*. Zadáním hodnoty pro **UpperPad** a **LowerPad** získat lepší kontrolu nad hello odsazení než pokud jste právě nastavili **odsazení** = true.

Další informace o convolutional sítě a jejich využití naleznete v článcích:  

* [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html)
* [http://Research.microsoft.com/pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
* [http://People.csail.MIT.edu/jvb/papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Sdružování sady
A **sdružování sady** platí geometrie podobné tooconvolutional připojení, ale používá předdefinované funkce toosource uzlu hodnoty tooderive hello cílový uzel hodnotu. Sdružování sady proto mít žádný trainable stav (váhu nebo tendence z). Sdružování sady podporu všech hello convolutional atributy s výjimkou **sdílení**, **MapCount**, a **váhu**.  

Obvykle se nepřekrývají hello jádra souhrnu přiléhající sdružování jednotkami. Pokud Stride [d] je rovna tooKernelShape [d] v každé dimenzi, je vrstva hello získat hello tradiční místní sdružování vrstvu, která se běžně používané v convolutional neuronové sítě. Každý cílový uzel vypočítá hello maximální nebo hello střední hello činnosti jeho jádra ve vrstvě zdroj hello.  

Hello následující příklad znázorňuje sdružování sady: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

* Hello Arita sady hello je 3 (hello délka řazené kolekce členů hello **InputShape**, **KernelShape**, a **Stride**). 
* Hello počet uzlů ve vrstvě zdroj hello je *5 * 24 * 24 = 2880*. 
* Toto je tradiční místní sdružování vrstvu, protože **KernelShape** a **Stride** jsou stejné. 
* Hello počet uzlů ve vrstvě cílové hello je *5 * 12 * 12 = 1440*.  

Další informace o sdružování vrstev najdete v těchto článcích:  

* [http://www.cs.Toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (část 3.4)
* [http://cs.Nyu.edu/~koray/publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
* [http://cs.Nyu.edu/~koray/publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)

## <a name="response-normalization-bundles"></a>Odpověď normalizaci sady
**Odpověď normalizaci** je místní normalizaci schéma, které bylo poprvé dostupné ve Geoffrey Hinton, a další v dokumentu hello [ImageNet Classiﬁcation s hluboké Neuronové sítě Convolutional](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Odpověď normalizace je použité tooaid Generalizace v neuronové sítě. Když jeden neuron se aktivuje na velmi vysokou aktivace úrovni, potlačí vrstva normalizaci místní odpovědi hello aktivace úroveň hello kolem neurons. To se provádí pomocí tří parametry (***α***, ***β***, a ***tisíc***) a convolutional struktury (nebo tvar Okolní počítače). Každý neuron ve vrstvě cílové hello ***y*** odpovídá tooa neuron ***x*** ve vrstvě zdroj hello. Hello aktivace úroveň ***y*** je dán hello následující vzorec, kde ***f*** je hello neuron, aktivace a ***Nx*** jádra hello (nebo hello sadu, která obsahuje hello neurons v okolí hello z ***x***), jak je definované hello convolutional strukturu:  

![][1]  

Odpověď sady normalizaci podporují všechny atributy convolutional hello s výjimkou **sdílení**, **MapCount**, a **váhu**.  

* Pokud hello jádra obsahuje neurons v hello mapovat stejné jako ***x***, schéma normalizaci hello je odkazované tooas **stejné mapování normalizaci**. toodefine stejné mapování normalizaci hello první souřadnice v **InputShape** musí mít hello hodnotu 1.
* Pokud hello jádra obsahuje neurons v hello prostorových stejně jako při ***x***, ale hello neurons jsou v další mapy, se nazývá hello normalizaci schéma **napříč mapuje normalizaci**. Tento typ odpovědi normalizaci implementuje formu laterální zabránění vycházející hello v nalezen typ skutečné neurons vytváření soutěž o úrovně big aktivace mezi výstupy neuron počítaný na různých mapy. toodefine napříč mapuje normalizaci, první souřadnice hello musí být celé číslo větší než 1 a větší než počet hello mapy a hello zbytek hello souřadnice musí mít hello hodnotu 1.  

Protože odpovědi normalizaci sady použít hodnotu předdefinované funkce toosource uzlu hodnoty toodetermine hello cílový uzel, nemají žádný trainable stav (váhu nebo tendence z).   

**Výstrahy**: hello uzlů ve vrstvě cílové hello odpovídají tooneurons, které jsou centrální uzly hello hello jádra. Například pokud KernelShape [d] je liché, pak *KernelShape [d] / 2* odpovídá toohello centrální jádra uzlu. Pokud *KernelShape [d]* je i, hello centrálního uzlu se na *KernelShape [d] / 2-1*. Proto pokud **odsazení**[d] skončí s výsledkem False, hello první a poslední hello *KernelShape [d] / 2* uzly nemají odpovídající uzly ve vrstvě cílové hello. tooavoid této situaci definovat **odsazení** jako [true nastavena hodnota true, true...,].  

Kromě toho toohello čtyři atributy dříve popisované, odpověď normalizaci sady také podpora hello následující atributy:  

* **Alpha**: (povinné) určuje hodnota s plovoucí desetinnou čárkou, která odpovídá příliš***α*** ve vzorci předchozí hello. 
* **Beta**: (povinné) určuje hodnota s plovoucí desetinnou čárkou, která odpovídá příliš***β*** ve vzorci předchozí hello. 
* **Posun**: (volitelné) určuje hodnota s plovoucí desetinnou čárkou, která odpovídá příliš***tisíc*** ve vzorci předchozí hello. Výchozí hodnota too1.  

Hello následující příklad definuje sady normalizaci odpovědi pomocí tyto atributy:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

* Hello zdroj vrstva obsahuje pět maps, každý s dimenzí aof 12 x 12, ale v uzlech 1440. 
* Hello hodnotu **KernelShape** označuje, že se jedná o stejnou vrstvu normalizaci mapy, kde hello servery jsou obdélníku 3 x 3. 
* Hello výchozí hodnotu **odsazení** hodnotu False, proto hello cílové vrstvě má jenom 10 uzly v Každá dimenze. tooinclude jeden uzel v hello cílové vrstva, která odpovídá tooevery uzlu ve vrstvě hello zdroje, přidejte odsazení = [hodnotu true, true, true]; a změňte velikost hello RN1 příliš [5, 12, 12].  

## <a name="share-declaration"></a>Deklarace sdílené složky
NET # volitelně podporuje definování víc sad s sdílené váhou. Hello váhu jakékoli dvě sady lze sdílet v případě, že jejich struktury jsou hello stejné. Hello se syntaxí definuje sady s váhou sdílené:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }

    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name

    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec

    bundle-spec:
       layer-name    =>     layer-name

    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec

    bias-spec:
        1    =>    layer-name

    layer-name:
        identifier  

Například hello následující prohlášení sdílené složky určuje názvy hello vrstev, která udává, že by měla být sdílena váhu i tendence z:  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

* vstupní Funkce Hello jsou rozděleny do dvou rovna velikosti vstupní vrstev. 
* Hello skryté vrstvy pak výpočetní vyšší úrovně funkcí na dvou vstupních vrstev hello. 
* Hello sdílenou složku – deklarace Určuje, že *H1* a *H2* musí být vypočteny v hello stejným způsobem jako z jejich odpovídajících vstupy.  

Alternativně to může být určen pomocí dvou samostatných deklarace sdílené složky následujícím způsobem:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

Hello krátkých úseků můžete použít pouze v případě, že vrstvy hello obsahují v jedné sadě. Obecně platí sdílení je možné, pouze pokud je relevantní struktura hello identické, což znamená, že mají hello stejná velikost stejné convolutional geometry a tak dále.  

## <a name="examples-of-net-usage"></a>Příklady Net # využití
Tato část obsahuje příklady použití Net # tooadd skryté vrstvy, definovat hello způsobu, jakým komunikovat s jinými vrstvami skryté vrstvy a sestavení convolutional sítě.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Definovat jednoduchý vlastní neuronové sítě: příkladu "Hello World"
Jednoduchý příklad ukazuje, jak toocreate a neuronové sítě model, který má jeden skryté vrstvě.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

Hello příklad ukazuje některé základní příkazy následujícím způsobem:  

* Hello první řádek definuje vstupní vrstvy hello (s názvem *Data*). Při použití hello **automaticky** – klíčové slovo, hello neuronové sítě automaticky obsahuje všechny funkce sloupce vstupní příklady hello. 
* Hello druhý řádek vytvoří hello skryté vrstvě. Název Hello *H* je přiřazen skryté vrstvě toohello, který má 200 uzly. Tuto vrstvu je plně připojené toohello vstupní vrstvy.
* třetí řádek Hello definuje vrstvy výstup hello (s názvem *O*), který obsahuje 10 výstupních uzlů. Pokud hello neuronové sítě se používá pro klasifikaci, je jeden uzel výstup na třídu. Hello – klíčové slovo **sigmoid** označuje, že funkce výstup hello je použité toohello výstup vrstvy.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Definovat více skryté vrstvy: Příklad vize počítače
Hello následující příklad ukazuje, jak toodefine trochu složitější neuronové sítě s více vlastní skryté vrstvy.  

    // Define hello input layers 
    input Pixels [10, 20];
    input MetaData [7];

    // Define hello first two hidden layers, using data only from hello Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;

    // Define hello third hidden layer, which uses as source hello hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }

    // Define hello output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

Tento příklad znázorňuje několik funkcí specifikace jazyka hello neuronové sítě:  

* Struktura Hello má dva vstupní vrstev, *pixelů* a *MetaData*.
* Hello *pixelů* vrstvy je vrstva zdroje pro dvě sady připojení, s vrstvami cílové *ByRow* a *ByCol*.
* Hello vrstvy *shromažďovat* a *výsledek* jsou cílové vrstvy do více svazků připojení.
* vrstvy výstup Hello *výsledek*, je vrstva cílové vede ke dvěma připojení sady; jedna s hello druhé úrovně skrytá (shromáždění) jako vrstva cílové a hello další vrstva vstupní hello (MetaData) jako cílové vrstvě.
* Hello skryté vrstvy *ByRow* a *ByCol*, zadejte filtrované připojení pomocí predikátu výrazy. Přesněji řečeno, hello uzlu v *ByRow* v [x, y] je připojených toohello uzlů v *pixelů* mají první souřadnic, x hello první index souřadnic rovna toohello uzlu. Podobně hello uzlu v *ByCol v [x, y] je připojených toohello uzlů v _Pixels* mají hello druhý souřadnice index v rámci jednoho uzlu hello druhý souřadnice, y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Zadejte síť convolutional pro více třídami klasifikace: Příklad rozpoznávání číslice
definice Hello hello následující sítě je navrženou toorecognize čísla a ilustruje některé pokročilé techniky pro přizpůsobení neuronové sítě.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


* Struktura Hello má jeden vstupní vrstvu, *Image*.
* Hello – klíčové slovo **convolve** označuje, že hello vrstev s názvem *Conv1* a *Conv2* jsou convolutional vrstvy. Každý z těchto layer prohlášení je následovaný seznamem atributů konvoluce hello.
* Hello net má třetí skryté vrstvy *Hid3*, která je plně připojené toohello druhý skryté vrstvě, *Conv2*.
* vrstvy výstup Hello *číslice*, je připojený jenom toohello třetí skryté vrstvě, *Hid3*. Hello – klíčové slovo **všechny** označuje, že vrstvy výstup hello plně připojeno příliš*Hid3*.
* Hello aritu konvoluce hello je tři (hello délka řazené kolekce členů hello **InputShape**, **KernelShape**, **Stride**, a **sdílení**). 
* Hello počet váhu za jádra je *1 + **KernelShape**\[0] * **KernelShape**\[1] * **KernelShape** \[2] = 1 + 1 * 5 * 5 = 26. Nebo 26 * 50 = 1300*.
* Hello uzly v každé skryté vrstvě můžete vypočítat následujícím způsobem:
  * **NodeCount**\[0] = (5 - 1) nebo 1 + 1 = 5.
  * **NodeCount**\[1] = (13-5) nebo 2 + 1 = 5. 
  * **NodeCount**\[2] = (13-5) nebo 2 + 1 = 5. 
* Hello celkový počet uzlů, lze vypočítat pomocí hello deklarovaný dimenzionalitu hello vrstvy, [50, 5, 5], následujícím způsobem:  ***MapCount** * **NodeCount** \[ 0] * **NodeCount**\[1] * **NodeCount**\[10 * 5 * 5 * 5 = 2]*
* Protože **sdílení**[d] skončí s výsledkem False pouze pro *d == 0*, hello počet jader je  ***MapCount** * **NodeCount** \[0] = 10 * 5 = 50*. 

## <a name="acknowledgements"></a>Potvrzení
Hello jazyk Net # pro přizpůsobení hello architektura neuronové sítě byla vyvinuta v Microsoftu Shon Katzenberger (Architekti, Machine Learning) a Alexey Kamenev (pracovník softwaru, Microsoft Research). Se používá interně pro strojové učení, projekty a aplikace od analýzy tootext detekce bitové kopie. Další informace najdete v tématu [Neuronové sítě v Azure ML – Úvod tooNet #](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)

[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif

