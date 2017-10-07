---
title: "aaaHow toochoose algoritmů strojového učení | Microsoft Docs"
description: "Jak toochoose algoritmů Azure Machine Learning dozorovaných, tak i u nedozorovaných informací v systému vytváření clusterů, klasifikační nebo regresní experimenty."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: a3b23d7f-f083-49c4-b6b1-3911cd69f1b4
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/25/2017
ms.author: garye
ms.openlocfilehash: 367b2278acc2435f27f9d24ead8199db58aca283
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochoose-algorithms-for-microsoft-azure-machine-learning"></a>Jak toochoose algoritmy pro Microsoft Azure Machine Learning
Hello odpověď toohello otázku "co machine learning algoritmus mám použít?" je vždy "Závisí." To závisí na velikosti hello, kvality a povaha hello data. Závisí na co chcete toodo s odpovědí hello. To závisí na tom, jak byl hello matematické algoritmu hello přeložit na pokyny pro hello počítač, který používáte. A závisí na tom, jak dlouho je k dispozici. I hello nejvíce došlo datových vědců nelze zjistit, který algoritmus provede než to zkusíte je nejvhodnější.

## <a name="hello-machine-learning-algorithm-cheat-sheet"></a>Hello Machine Learning algoritmus cheaty listu
Hello **Microsoft Azure Machine Learning algoritmus cheaty list** pomáhá zvolte hello vpravo počítač algoritmus učení pro vaše řešení prediktivní analýzy z knihovny Microsoft Azure Machine Learning hello algoritmů.
Tento článek vás provede toouse ho.

> [!NOTE]
> toodownload hello tahák a postupujte podle společně s tímto článkem přejděte příliš[strojového učení rychlý přehled algoritmů pro Microsoft Azure Machine Learning Studio](machine-learning-algorithm-cheat-sheet.md).
> 
> 

Tato tahák má velmi určitou cílovou skupinu v paměti: začátku dat vědecký pracovník s úrovni škole machine learning, která při toochoose toostart algoritmus s v Azure Machine Learning Studio. To znamená, že umožňuje některé Generalizace a oversimplifications, ale jeho bodů bezpečné směrem. Taky to znamená, že jsou velké množství algoritmy, které zde nejsou uvedeny. S růstem Azure Machine Learning tooencompass více kompletní sadu dostupných metod přidáme je.

Tato doporučení jsou kompilované zpětné vazby a od mnoha datových vědců a odborníky machine learning. Všechno, co jsme nebyla souhlas, ale I se zkusili tooharmonize naše názory do hrubý konsenzu. Většina příkazů hello o nesouhlasu začínat řetězcem "Závisí..."

### <a name="how-toouse-hello-cheat-sheet"></a>Jak toouse hello tahák
Přečtěte si hello cestu a algoritmus popisky v grafu hello jako "pro  *&lt;popisek cesty&gt;*, použijte  *&lt;algoritmus&gt;*." Například "pro *rychlost*, použijte *dvě třídy logistic regression*." Vztahuje se někdy více než jeden větev.
Někdy žádný z nich jsou vzájemně přizpůsobit. Budou se zamýšlené toobe pravidlo jezdec doporučení, proto nemusíte si dělat starosti o něm se přesný.
Několik datových vědců I mluvili s uvedená této hello pouze zda způsob, jak najít velmi nejlepší algoritmus hello je tootry všechny z nich.

Tady je příklad z hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) experimentu několik algoritmů proti hello by se stejným datům a porovnává hello výsledky: [porovnat více třída třídění: písmeno rozpoznávání ](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

> [!TIP]
> toodownload a tisk diagram, který bude stručně charakterizovat hello možností nástroje Machine Learning Studio, najdete v [diagram s přehledem funkcí Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).
> 
> 

## <a name="flavors-of-machine-learning"></a>Charakteristikami machine learning
### <a name="supervised"></a>Pod dohledem.
Algoritmy pod dohledem učení provádět předpovědi na základě sady příklady. Pro instanci historických uložených ceny lze použít toohazard pokusů v budoucnu ceny. Každý příklad používá pro školení je označené hodnotou hello zájmu – v tomto případě hello uložených cena. Algoritmus pod dohledem učení Hledat v těchto hodnota popisky. Může použít všechny informace, které by mohly být důležité – hello dne týdne hello, hello sezóny, finanční hello firemní data, hello typ odvětví, hello přítomnost rušivý geopolitické události – a jednotlivých algoritmů vypadá pro různé typy schémat. Po nalezení hello nejlepší vzor ho můžete hello algoritmus používá, předpovědi toomake vzor bez popisku testovacích dat – budoucí ceny.

Pod dohledem learning je Oblíbené a užitečné typ strojové učení. S jednou výjimkou všechny moduly hello v Azure Machine Learning jsou pod dohledem učení algoritmy. Existuje několik typů konkrétní pod dohledem učení, které představuje v rámci Azure Machine Learning: detekce anomálií, klasifikace a regrese.

* **Klasifikace**. Když hello dat probíhá použité toopredict kategorii, pod dohledem učení se také nazývá klasifikace. Je tomu hello při přiřazování bitovou kopii jako obrázek 'cat' nebo 'PSA. Pokud jsou dostupné pouze dvě možnosti, se nazývá **two-class** nebo **binomický klasifikace**. Když existují další kategorie, jako, předpověď hello Vítěz hello NCAA března Madness turnaj, potíže se označuje jako **více třída klasifikace**.
* **Regrese**. Pokud hodnota je právě předpovědět, stejně jako u uložených ceny, se nazývá pod dohledem learning regrese.
* **Detekce anomálií**. Někdy hello cílem je tooidentify datové body, které jsou jednoduše neobvyklé. Při zjišťování podvodů například všechny vzorky útraty hodně neobvyklý platební karty se podezřelá. mnoho, takže a hello příklady školení tak několik, že není vhodná toolearn jaké podvodné aktivity vypadá jako možná odchylek Hello. Přístup, který přebírá detekce anomálií je toosimply zjistěte, jaké normální aktivity bude vypadat takto (pomocí-podvodné transakce historie) a najděte všechno, co se výrazně liší.

### <a name="unsupervised"></a>Bez dohledu
V bez dohledu learning datové body mají žádné popisky s nimi spojených. Místo toho bez dohledu cílem hello učení algoritmu je uspořádání dat hello v některé stejným způsobem jako nebo toodescribe jeho strukturu. To může znamenat seskupení do clusterů nebo při hledání různé způsoby prohlížení komplexní data tak, aby se jednodušší nebo více uspořádány.

### <a name="reinforcement-learning"></a>Posílení učení
V posílení learning hello algoritmus získá toochoose akce v odpovědi tooeach datového bodu. algoritmus učení Hello dále přijímá signálu účet, který byl po krátkou dobu později, která určuje, jak dobrý hello rozhodnutí.
Na základě hello algoritmus upravuje jeho strategií pořadí tooachieve hello nejvyšší potřebu. Aktuálně neexistují žádné posílení učení algoritmu modulů v Azure Machine Learning. Posílení learning je běžné v robotics, kde hello sadu odečty snímačů v jednom bodě v čase je hodnota datového bodu, a algoritmus hello musíte zvolit hello robot další akce. Je také, že přírodní vhodné pro Internet věcí aplikace.

## <a name="considerations-when-choosing-an-algorithm"></a>Důležité informace k výběru algoritmus
### <a name="accuracy"></a>Přesnost
Získávání odpovědí nejpřesnější hello možná není vždy nutné.
Někdy je dostačující, v závislosti na tom, co chcete použít pro sblížení. Pokud tento způsob hello případ, může být schopný toocut vaše doba zpracování výrazně podle provedením vykrvovacího vpichu s další přibližnou metody. Další výhodou další přibližnou metody je, že se přirozeně mívají předejdete [overfitting](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Čas školení
počet minut Hello nebo hodiny nezbytné tootrain model liší značnou část algoritmy. Cvičení čas často úzce souvisí přesnost – obvykle jednu doprovází hello jiné. Kromě toho některé algoritmy jsou citlivější toohello počet datových bodů než jiné.
Po omezenou dobu je jednotka hello vybrat algoritmus, zejména v případě, že je velká hello datové sady.

### <a name="linearity"></a>Linearity
Ujistěte se, velké množství algoritmy strojového učení použít linearity. Lineární klasifikace algoritmy předpokládá, že třídy je možné oddělit přímku (nebo její vyšší dimenzí analogovým). Patří logistic regression a podporují vektoru počítače (jak je implementované v Azure Machine Learning).
Lineární regrese algoritmy předpokládají, podívejte se na data trendy přímce. Tyto předpoklady nejsou chybný pro některé problémy, ale na jiných přinášejí přesnost.

![Hranice non lineární – třída][1]

***Lineární bez třída hranic*** *-spoléhat na lineární klasifikační algoritmus by mělo za následek nízkou přesnost*

![Data s nelineární trendu][2]

***Data s nelineární trend*** *-pomocí jiné metody lineární regrese by vygeneroval mnohem větší chyby, než je nutné*

Bez ohledu na jejich nebezpečích lineární algoritmy jsou velmi oblíbených jako první řádek útoku. Zpravidla se toobe algorithmically jednoduchým a rychlým ke cvičení.

### <a name="number-of-parameters"></a>Počet parametrů
Parametry jsou hello knoflíky vědecký pracovník data získá tooturn při nastavování algoritmu. Jsou čísla, která ovlivňují chování hello algoritmus, například tolerance chyb nebo počet iterací nebo možnosti mezi varianty chování hello algoritmus. čas Hello školení a přesnost algoritmu hello někdy může být velmi citlivé toogetting jenom hello správné nastavení. Algoritmy s parametry velké počty obvykle vyžadují hello nejvíce zkušební verze a chyba toofind dobrý kombinaci.

Případně, není [parametr (vymetání) komínů](machine-learning-algorithm-parameters-optimize.md) bloku modulu v Azure Machine Learning, která automaticky se pokusí všechny kombinace parametrů v jakémkoli členitosti zvolíte. Když je to skvělý způsob toomake Opravdu jste předané hello parametr místa, hello čas potřebný tootrain model zvyšuje exponenciálnímu s hello počet parametrů.

Hello vzhůru je, že má mnoho parametrů obvykle znamená, že algoritmus má větší flexibilitu. Často může dosáhnout velmi dobré přesnost. Pokud že zjistíte hello správnou kombinaci nastavení parametrů.

### <a name="number-of-features"></a>Počet funkcí
Pro některé typy dat může být hello řadu funkcí velké porovnání toohello počet datových bodů. Toto je často případ hello s genetika nebo textová data. Hello velký počet funkcí můžete bog dolů některé learning algoritmy, provedení školení čas unfeasibly dlouho. Podpora vektoru počítače jsou zvláště skvěle hodí toothis případu (viz níže).

### <a name="special-cases"></a>Zvláštní případy
Některé algoritmy učení zkontrolujte určité předpoklady o hello struktura dat hello nebo hello požadovaných výsledků. Pokud můžete najít ten, který vyhovuje vašim potřebám, se vám může poskytnout užitečnější výsledky, přesnější předpovědi nebo školení rychlejší.

| **Algoritmus** | **Přesnost** | **Čas školení** | **Linearity** | **Parametry** | **Poznámky k** |
| --- |:---:|:---:|:---:|:---:| --- |
| **Klasifikace dva – třída** | | | | | |
| [logistic regression](https://msdn.microsoft.com/library/azure/dn905994.aspx) | |● |● |5 | |
| [rozhodnutí doménové struktury](https://msdn.microsoft.com/library/azure/dn906008.aspx) |● |○ | |6 | |
| [rozhodovací Džungle](https://msdn.microsoft.com/library/azure/dn905976.aspx) |● |○ | |6 |Nároky nedostatku paměti |
| [Vylepšené rozhodovací strom](https://msdn.microsoft.com/library/azure/dn906025.aspx) |● |○ | |6 |Nároky velké paměti |
| [neuronové sítě](https://msdn.microsoft.com/library/azure/dn905947.aspx) |● | | |9 |[Je možné další přizpůsobení](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [zprůměrovanou perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) |○ |○ |● |4 | |
| [Support vector machine](https://msdn.microsoft.com/library/azure/dn905835.aspx) | |○ |● |5 |Dobré pro velké funkce sady |
| [počítač vektoru místně přímé podpory](https://msdn.microsoft.com/library/azure/dn913070.aspx) |○ | | |8 |Dobré pro velké funkce sady |
| [Počítač bodu Bayes.](https://msdn.microsoft.com/library/azure/dn905930.aspx) | |○ |● |3 | |
| **Klasifikace více – třída** | | | | | |
| [logistic regression](https://msdn.microsoft.com/library/azure/dn905853.aspx) | |● |● |5 | |
| [rozhodnutí doménové struktury](https://msdn.microsoft.com/library/azure/dn906015.aspx) |● |○ | |6 | |
| [rozhodovací Džungle](https://msdn.microsoft.com/library/azure/dn905963.aspx) |● |○ | |6 |Nároky nedostatku paměti |
| [neuronové sítě](https://msdn.microsoft.com/library/azure/dn906030.aspx) |● | | |9 |[Je možné další přizpůsobení](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [One-v-all](https://msdn.microsoft.com/library/azure/dn905887.aspx) |- |- |- |- |Zobrazit vlastnosti vybrané metodě two-class hello |
| **Regrese** | | | | | |
| [lineární](https://msdn.microsoft.com/library/azure/dn905978.aspx) | |● |● |4 | |
| [Bayesova lineární](https://msdn.microsoft.com/library/azure/dn906022.aspx) | |○ |● |2 | |
| [rozhodnutí doménové struktury](https://msdn.microsoft.com/library/azure/dn905862.aspx) |● |○ | |6 | |
| [Vylepšené rozhodovací strom](https://msdn.microsoft.com/library/azure/dn905801.aspx) |● |○ | |5 |Nároky velké paměti |
| [quantile rychlé doménové struktury](https://msdn.microsoft.com/library/azure/dn913093.aspx) |● |○ | |9 |Distribuce spíše než předpovědi bodu |
| [neuronové sítě](https://msdn.microsoft.com/library/azure/dn905924.aspx) |● | | |9 |[Je možné další přizpůsobení](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [Poissonovo rozdělení](https://msdn.microsoft.com/library/azure/dn905988.aspx) | | |● |5 |Technicky protokolu – lineární. Pro predikci počty |
| [pořadí](https://msdn.microsoft.com/library/azure/dn906029.aspx) | | | |0 |Pro predikci pořadí řazení |
| **Detekce anomálií** | | | | | |
| [Support vector machine](https://msdn.microsoft.com/library/azure/dn913103.aspx) |○ |○ | |2 |Zvlášť vhodné pro velké funkce sady |
| [Detekce anomálií založený na PCA](https://msdn.microsoft.com/library/azure/dn913102.aspx) | |○ |● |3 | |
| [K-means](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/) | |○ |● |4 |Clustering algoritmus |

**Vlastnosti algoritmu:**

**●** -zobrazuje vynikající přesnost, časy rychlé školení a použití hello linearity

**○** -ukazuje dobrý přesnost a časy, střední školení

## <a name="algorithm-notes"></a>Algoritmus poznámky
### <a name="linear-regression"></a>Lineární regrese
Jak je uvedeno nahoře, [lineární regrese](https://msdn.microsoft.com/library/azure/dn905978.aspx) vyhovuje řádek (nebo rovině nebo hyperplane) toohello datové sady. Je centrem, jednoduchým a rychlým, ale může být příliš zneužívající vlastností prohlížeče pro některé problémy.
Zde můžete zkontrolovat [lineární regrese kurzu](machine-learning-linear-regression-in-azure.md).

![Data s lineární trend][3]

***Data s lineární trend***

### <a name="logistic-regression"></a>Logistic regression
I když ho 'regrese' confusingly obsahuje v názvu text hello, logistic regression je ve skutečnosti výkonný nástroj pro [two-class](https://msdn.microsoft.com/library/azure/dn905994.aspx) a [multiclass](https://msdn.microsoft.com/library/azure/dn905853.aspx) klasifikace. Je rychlé a jednoduché. Hello fakt, který používá k na '-tvarované křivky místo přímku umožňuje přirozené přizpůsobit pro dělení dat do skupin. Hranice lineární třída poskytuje logistic regression, tak při použití, ujistěte se, že lineární aproximace je něco, co můžete za provozu s.

![Logistic regression tootwo třídy dat s jedním funkcí][4]

***Data tootwo třídy logistic regression s jedním funkce*** *-hello bodu, na které hello logistic křivka je právě zavřít tooboth třídy je hranicí – třída*

### <a name="trees-forests-and-jungles"></a>Džungle, stromy a doménové struktury
Decision doménovými strukturami ([regrese](https://msdn.microsoft.com/library/azure/dn905862.aspx), [two-class](https://msdn.microsoft.com/library/azure/dn906008.aspx), a [multiclass](https://msdn.microsoft.com/library/azure/dn906015.aspx)), decision Džungle ([two-class](https://msdn.microsoft.com/library/azure/dn905976.aspx) a [ multiclass](https://msdn.microsoft.com/library/azure/dn905963.aspx)) a boosted decision trees ([regrese](https://msdn.microsoft.com/library/azure/dn905801.aspx) a [two-class](https://msdn.microsoft.com/library/azure/dn906025.aspx)) jsou všechny na základě rozhodovacích stromů, základní strojového učení koncept. Existuje mnoho variant rozhodovací stromy, ale všechny stejnou věc udělat – místo funkce hello rozdělte oblasti s většinou hello stejný popisek. To mohou být oblasti konzistentní kategorie nebo konstantní hodnoty, v závislosti na tom, jestli dělají klasifikační nebo regresní.

![Rozhodovací strom rozděluje prostor funkce][5]

***Rozhodovací strom rozděluje prostor funkce do oblasti hodnot zhruba uniform***

Funkce místa lze rozdělit na nahodile malé oblasti, proto je snadno tooimagine je rozdělen jemně dostatek toohave jeden datový bod každou oblast. Toto je příklad extrémně overfitting. V pořadí tooavoid to velké sady stromy se vytvářejí s zvláštní pozornost matematickém prováděné, že nejsou korelační stromy hello. průměr Hello této "rozhodnutí doménové struktury" je stromové struktury, která zabraňuje overfitting. Rozhodnutí doménové struktury mohou pomocí velké množství paměti. Rozhodovací Džungle jsou hodnotu typu variant, která využívá méně paměti na náklady hello trochu delší dobu školení.

Vylepšené rozhodovací stromy vyhnout overfitting omezením kolikrát se můžete rozdělit a jak několik datových bodů jsou povoleny v každé oblasti. Algoritmus vytvoří posloupnost stromy, z nichž každý se učí kompenzovat chyby hello zanechaný hello stromu před. Výsledkem Hello je velmi přesná student, který obvykle toouse velké množství paměti. Úplná technická popis hello, podívejte se na [původního dokumentu na Friedman](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Rychlý doménové struktury quantile regrese](https://msdn.microsoft.com/library/azure/dn913093.aspx) se odlišuje od rozhodovací stromy pro hello zvláštní případ, kam chcete vědět, ne jenom hello typické (střední) hodnotu hello dat v rámci oblasti, ale také jeho distribuci hello tvar quantiles.

### <a name="neural-networks-and-perceptrons"></a>Neuronové sítě a perceptrons
Neuronové sítě jsou mozku inspirovaných učení algoritmy pokrývajících [multiclass](https://msdn.microsoft.com/library/azure/dn906030.aspx), [two-class](https://msdn.microsoft.com/library/azure/dn905947.aspx), a [regrese](https://msdn.microsoft.com/library/azure/dn905924.aspx) problémy. Se mohou nekonečné různé, ale hello neuronové sítě v rámci Azure Machine Learning jsou všechny hello formu směrovanou necyklické grafy. Která znamená, že vstupní funkce jsou předány dál (nikdy zpátky) prostřednictvím pořadí vrstev před se převedena na výstupy. V každé vrstvě jsou v různých kombinacích, předány do další vrstva hello a sčítají váha vstupy. Tato kombinace jednoduché výpočty má za následek toolearn možnost Pokročilé třída hranice a data trendy zdánlivě podle magic. Vrstvený mnoho sítí toto řazení provést hello "hloubkové vzdělávání", který paliva mnoho reporting technické a vědecké účely fiction.

Tento vysoký výkon nepřejde do stavu zdarma, ale. Neuronové sítě může trvat dlouhou dobu tootrain, hlavně pro velké sady dat s mnoha funkcí. Mají také další parametry než většina algoritmů, které znamená parametr (vymetání) komínů značnou část rozšíří hello školení čas.
A pro tyto overachievers, kteří chtějí příliš[zadat své vlastní struktury sítě](http://go.microsoft.com/fwlink/?LinkId=402867), možnosti jsou inexhaustible.

![Hranice poznat neuronové sítě][6]
***hranice hello se naučili neuronové sítě může být složité a nestandardní***

Hello [two-class průměrem perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) je neuronové sítě odpovědí tooskyrocketing školení vícekrát. Využívá strukturu sítě, která poskytuje lineární třída hranice. Je téměř primitivní standardy dnešní, ale má dlouhou práce robustní a je dostatečně malé, toolearn rychle.

### <a name="svms"></a>SVMs
Podpora vektoru počítače (SVMs) najít hello hranice, která odděluje třídy podle jako celý okraj míře. Pokud dvě třídy hello nelze jasně oddělené, najít hello algoritmy hello nejlepší hranic, které mohou. Zapsaný v Azure Machine Learning, hello [two-class SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) tomu s pouze přímce. (V řeči SVM použije lineární jádra.) Vzhledem k tomu, že umožňuje tento lineární aproximace, je možné toorun docela rychle. Kde je skutečně září je s daty bohaté funkce, jako text nebo genomických. V těchto případech jsou SVMs možné tooseparate třídy rychleji a s menší overfitting než většinu dalších algoritmů kromě toorequiring pouze mírné množství paměti.

![Hranice podpory vector machine – třída][7]

***Okraj hello oddělujících dvě třídy maximalizuje hranici typické podporu počítač vector – třída***

Jiného produktu nástroje Microsoft Research hello [two-class místně hloubkové SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) je – lineární varianta SVM, který bude mít většinu hello rychlost a paměti efektivity hello lineární verze. Je ideální pro případy, kdy lineární přístup hello nedává dostatečně přesné odpovědi. Vývojáři Hello udržováno ho rychlého rozdělením hello problém do spoustu malé lineární SVM potíže. Čtení hello [úplný popis](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) hello podrobnosti o tom, jak se vyžádat vypnout platí to.

Pomocí inteligentní rozšíření nelineární SVMs, hello [SVM jedna třída](https://msdn.microsoft.com/library/azure/dn913103.aspx) nevykresluje hranici, která úzce popisuje hello celé datové sady. Je užitečné pro zjišťování anomálií. Všechny nové datové body, které daleko spadal mimo tuto hranici je dost neobvyklé toobe pozoruhodné.

### <a name="bayesian-methods"></a>Bayesova metody
Metody Bayesova mají vysokou žádoucí kvality: vyhnou overfitting. Je to tím, že některé předpoklady předem informace o hello pravděpodobně distribuci hello odpovědí. Jiné byproduct tohoto přístupu je, ke kterým mají velmi málo parametrů. Azure Machine Learning má i Bayesova algoritmy pro obě klasifikaci ([Two-class Bayes point machine](https://msdn.microsoft.com/library/azure/dn905930.aspx)) a regrese ([lineární regrese Bayesova](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Všimněte si, že tyto předpokládá, že hello dat můžete rozdělení nebo přizpůsobit s přímce.

Na historické Poznámka byly počítače bodu se Bayes vyvinuté v Microsoft Research. Mají některé výjimečně Krásný teoretické pracovní za ně. dotčené student Hello je řízené toohello [původní článek v JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) a [pronikavého blog podle Jan Bishop](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Specializované algoritmy
Pokud máte velmi konkrétní cílem může být jednoduché. V rámci hello kolekce Azure Machine Learning jsou algoritmy, které se specializují na:

- RANK předpovědi ([pořadí regrese](https://msdn.microsoft.com/library/azure/dn906029.aspx)),
- počet předpovědi ([Poissonovo rozdělení regrese](https://msdn.microsoft.com/library/azure/dn905988.aspx)),
- detekce anomálií (jeden na základě [hlavní součásti analysis](https://msdn.microsoft.com/library/azure/dn913102.aspx) a jeden na základě [support vector machine](https://msdn.microsoft.com/library/azure/dn913103.aspx)s)
- Clustering ([K-means](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/))

![Detekce anomálií založený na PCA][8]

***Detekce anomálií založený na PCA*** *-hello valná většina dat hello spadá do stereotypical distribuční; výrazně odchylují od tohoto distribuční body jsou podezřelé*

![Datové sady, které jsou seskupeny pomocí K-means][9]

***Datové sady jsou rozděleny do pěti clustery pomocí K-means***

Je také kompletu [více třídami třídění one-v-all](https://msdn.microsoft.com/library/azure/dn905887.aspx), které zalomení hello N třída klasifikace problému do N-1 two-class klasifikaci problémy. Hello přesnost, čas školení a linearity vlastnosti jsou určeny třídění two-class hello používá.

![Dvě třídy třídění kombinaci tooform třídění tři – třída][10]

***Pár two-class třídění kombinovat tooform třídění tři – třída***

Azure Machine Learning také zahrnuje přístup tooa výkonné strojové učení framework pod označením hello [Vowpal k dispozici](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf).
Zobrazit prosíme, abyste kategorizaci tady, protože další klasifikace a regrese problémy a i další z částečně bez popisku data. Můžete ji nakonfigurovat toouse kterékoli z několika učení algoritmy, ztrátě funkce a algoritmy optimalizace. Byl navržen z hello pozadí až toobe efektivní, paralelní a velmi rychlé. Zpracovává ridiculously velké funkce sady s malým množstvím zřejmá úsilí.
Spuštění a vedla podle Microsoft Research na vlastní Jan Langford, je zobrazit vzorec jednu položku v poli stock car algoritmů. Ne každý problém vyhovuje zobrazit, ale bezdrátové sítě může být vhodné vaší chvíli tooclimb křivku na jeho rozhraní. Je také k dispozici jako [samostatné otevřít zdrojový kód](https://github.com/JohnLangford/vowpal_wabbit) v několika jazycích.

## <a name="more-help-with-algorithms"></a>Další pomoc s algoritmy
* Infografice ke stažení, který popisuje algoritmy a obsahuje příklady, najdete v části [ke stažení Infografice: strojového učení základy s příklady algoritmus](machine-learning-basics-infographic-with-algorithm-examples.md).
* Seznam podle kategorie algoritmů všechny hello strojového učení, které jsou k dispozici v Azure Machine Learning Studio najdete v tématu [inicializovat Model] [ initialize-model] v nápovědě modulu a hello Machine Learning Studio algoritmus.
* Dokončení abecední seznam algoritmů a modulů v Azure Machine Learning Studio najdete v tématu [seznam A-Z modulů Machine Learning Studio] [ a-z-list] v Machine Learning Studio algoritmus a pomoci modulu.
* toodownload a tisk diagram, který bude stručně charakterizovat hello možnosti Azure Machine Learning Studio, najdete v [diagram s přehledem funkcí Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).


<!-- Reference links -->
[initialize-model]: https://msdn.microsoft.com/library/azure/dn905812.aspx
[a-z-list]: https://msdn.microsoft.com/library/azure/dn906033.aspx

<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
