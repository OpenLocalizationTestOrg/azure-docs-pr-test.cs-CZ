---
title: "aaaAnalyzing zákazníka změn pomocí Machine Learning | Microsoft Docs"
description: "Případová studie vývoje model integrované k analýze a vyhodnocování zákazníka změn"
services: machine-learning
documentationcenter: 
author: jeannt
manager: jhubbard
editor: cgronlun
ms.assetid: 1333ffe2-59b8-4f40-9be7-3bf1173fc38d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeannt
ms.openlocfilehash: 070e6a2ebe4f2fe439a42ffe1a3fa9d6d3788d62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Analýza výpovědí zákazníků pomocí služby Azure Machine Learning
## <a name="overview"></a>Přehled
Tento článek představuje odkaz na implementaci projektu analysis změn zákazníka, vytvořené pomocí Azure Machine Learning. V tomto článku probereme přidružené obecné modely pro komplexní řešení hello problém změn průmyslových zákazníka. Měříme hello přesnost modelů, které jsou vytvořeny pomocí Machine Learning a jsme vyhodnocení pokyny pro vývoj Další.  

### <a name="acknowledgements"></a>Potvrzení
Tohoto experimentu byl vyvinutý a otestovat Serge Berger, vědecký pracovník objekt zabezpečení dat společnosti Microsoft a Roger Barga, dříve správce produktu pro Microsoft Azure Machine Learning. tým dokumentace k Azure Hello Děkujeme za jejich uznává svoje znalosti a Děkujeme, že je tento dokument white paper sdílení.

> [!NOTE]
> Hello data pro tento experiment není veřejně dostupné. Příklad, jak toobuild strojové učení modelu pro analýzu změn, naleznete v tématu: [prodejní změn šablony modelu](https://gallery.cortanaintelligence.com/Collection/Retail-Customer-Churn-Prediction-Template-1) v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/)
> 
> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-problem-of-customer-churn"></a>Hello problém zákazníka změn
Firmy hello příjemce trhu a ve všech odvětvích enterprise mají toodeal s změn. Někdy je příliš změn a vliv rozhodnutí o zásadách. tradiční řešení Hello je toopredict vysokou tendenci churners a řešení jejich potřeb prostřednictvím služby concierge marketingových kampaní, nebo použitím zvláštní výjimky. Tato řešení se může lišit z odvětví tooindustry nebo dokonce z tooanother clusteru konkrétní příjemce v rámci jednoho oboru (například telecommunications).

běžné faktor Hello je, třeba firmy toominimize úsilí uchování tyto speciální zákazníka. Fyzická metodika by proto být tooscore každý zákazník s hello pravděpodobnost změn a vyřešte hello hlavních ty, které jsou. Hlavní zákazníci Hello může být hello nejvíce ziskové ty; ve složitějších scénářích, například je funkce zisku vzhledem během hello výběr kandidáty pro speciální výjimku. Tyto aspekty jsou však pouze část hello komplexní strategie pro práci s změn. Podniky mají také tootake do účtu riziko (a související rizika tolerance) hello úroveň a náklady na hello zásahu a segmentace vyhovující zákazníka.  

## <a name="industry-outlook-and-approaches"></a>Odvětví outlook a přístupů
Sofistikované zpracování změn je znak vyspělá oboru. Hello classic příkladem je hello telecommunications odvětví, kde Odběratelé, kteří jsou známé toofrequently přepínat z jednoho poskytovatele tooanother. Tato dobrovolná změn je hlavním zájmem. Kromě toho zprostředkovatelé shromáždily významné znalosti o *změn ovladače*, které jsou hello faktory tooswitch zákazníkům této jednotky.

Například telefonu nebo zařízení volba je dobře známé faktorem změn v hello firmy mobilního telefonu. V důsledku toho oblíbených zásad je toosubsidize hello cena sluchátka pro nové odběratele a poplatků úplné ceny tooexisting zákazníci pro upgrade. V minulosti tato zásada vedlo toocustomers přepínání z jednoho poskytovatele tooanother tooget nové slevy, které se pak má výzva zprostředkovatelé toorefine jejich strategie.

Vysoká volatility v telefonu nabídky je faktor, který velmi rychle by způsobila neplatnost modely změn, které jsou založeny na aktuální telefonního sluchátka modelů. Kromě toho mobilních telefonů nejsou jenom telekomunikace zařízení; jsou i příkazy způsobem (zvažte hello iPhone) a tyto sociálních prognostické jsou mimo rozsah hello regulární telecommunications datových sad.

Hello net výsledek pro modelování je, že nelze navrhnout zásadu zvukové jednoduše tak, že odstraňuje známé důvody změn. Ve skutečnosti strategie průběžné modelování, včetně classic modely, které vyčíslení kategorií proměnné (například rozhodovací stromy), je **povinné**.

Použití velkých datových sad na zákazníky, fungují organizace analýzy velkých objemů dat (zejména detekce změn na základě velkých objemů dat) jako problémem toohello efektivní přístup. Můžete najít další informace o hello velkých objemů dat přístup toohello problém změn v hello doporučení v oddílu ETL.  

## <a name="methodology-toomodel-customer-churn"></a>Metodologie toomodel zákazníka změn
Běžných potíží proces toosolve zákazníka změn je znázorněn v obrázky 1 – 3:  

1. Model riziko vám umožní tooconsider vliv akce pravděpodobnosti a rizika.
2. Model zásahu vám umožní tooconsider jak hello úroveň zásahu mohou ovlivnit hello pravděpodobnost změn a hello množství zákazníků hodnotu doby života (CLV).
3. Tato analýza různě konfigurovat tooa kvalitativní analýze, který je eskalované tooa proaktivní marketingovou kampaň, která cílí zákaznické segmenty toodeliver hello optimální nabídky.  

![][1]

Tento přístup dál vypadající je hello nejlepší způsob, jak tootreat změn, ale se dodává s složitost: máme toodevelop více modelu archetype a trasování závislosti mezi modely hello. Hello interakce mezi modely můžete zapouzdřené, jak je znázorněno v následujícím diagramu hello:  

![][2]

*Obrázek 4: Unified více modelu archetype*  

Interakce mezi modely hello je klíč, pokud jsme toodeliver uchování toocustomer celkový přístup. Každý model nutně snižuje časem; Architektura hello je proto implicitní smyčky (podobně jako toohello archetype hello OSTRÉ DM standard dolování dat, která nastavuje [***3***]).  

Hello celkové cyklus riziko. rozhodnutí marketing segmentace nebo rozložené je stále zobecněný struktura, což je použít toomany obchodních problémů. Analýza změn je jednoduše silné zástupce této skupiny problémů, protože je jádro vykazuje se všechny vlastnosti hello komplexní podnikové problému, který neumožňuje zjednodušené prediktivní řešení. Hello sociálních aspektů hello moderní přístup toochurn nejsou zvlášť zvýrazněných v hello přístup, ale sociální aspekty hello se zapouzdřené v archetype modelování hello, jako by byly v kteréhokoli modelu služeb.  

Zde zajímavé přidání je analýzy velkých objemů dat. Dnešní telekomunikace a prodejní firmách shromažďovat vyčerpávající data o zákazníků a snadno jsme můžete předvídáte, že hello potřebovat pro více modelu připojení bude běžné trendu, danou rozvíjející trendy, jako hello Internet věcí a všudypřítomná zařízení, která umožňují obchodní tooemploy inteligentní řešení v několika vrstev.  

 

## <a name="implementing-hello-modeling-archetype-in-machine-learning-studio"></a>Implementace hello modelování archetype v nástroji Machine Learning Studio
Zadané hello problém stejně, co je nejlepší způsob, jak tooimplement hello integrované modelování a vyhodnocování přístup? V této části popisuje, jak jsme udělat to pomocí Azure Machine Learning Studio.  

více modelu přístup Hello je musí při navrhování globální archetype pro změn. I hello vyhodnocování (prediktivní) součást hello přístupu by měla být více modelu.  

Hello následující diagram znázorňuje hello prototyp, že jsme vytvořili, který používá čtyři vyhodnocování algoritmy v Machine Learning Studio toopredict změn. Důvod Hello použití více modelu přístup je pouze toocreate komplet třídění tooincrease přesnost, ale také tooprotect proti přečerpání přizpůsobování a výběr tooimprove doporučený funkce.  

![][3]

*Obrázek 5: Prototyp změn modelování přístup*  

Hello následující části obsahují další podrobnosti o hello prototypu vyhodnocování model, který jsme implementovaná pomocí Machine Learning Studio.  

### <a name="data-selection-and-preparation"></a>Příprava a výběr dat
Hello data používá toobuild hello modely a skóre zákazníkům byl získán z svislé řešení CRM, s ochrana osobních údajů hello data matoucí tooprotect zákazníků. Hello dat obsahuje informace o 8000 odběry ve hello USA, a kombinuje tři zdroje: zřizování data (předplatné metadata), data aktivity (použití systému hello) a data podpory zákazníků. Hello dat neobsahuje žádné obchodní související informace o zákaznících hello; nezahrnuje například věrného metadata nebo platební skóre.  

Pro jednoduchost ETL a čištění procesy data jsou mimo rozsah protože předpokládáme, že již má Příprava dat neudělalo jinde.   

Výběr funkce pro modelování je založena na předběžné násobek vyhodnocování hello sadu prognostické, součástí hello proces, který používá modul hello náhodných doménové struktury. Pro implementaci hello v nástroji Machine Learning Studio jsme vypočítat hello střední, střední a rozsahy reprezentativní funkce. Například jsme přidali agregace pro hello kvalitativní data, například minimální a maximální hodnoty pro aktivity uživatelů.    

Také jsme zachytit dočasné informace pro hello posledních šest měsíců. Jsme analyzovali data po dobu jednoho roku a navázat jsme, že i v případě, že bylo statisticky významný trendy, hello vliv na změn značně dojde ke snížení po dobu šesti měsíců.  

celý proces této hello, včetně ETL, výběr funkce, je nejdůležitější bod Hello a modelování byla implementována v nástroji Machine Learning Studio, použití zdrojů dat v Microsoft Azure.   

Hello následující obrázky znázorňují hello data, která byla použita.  

![][4]

*Obrázek 6: Výňatek ze zdroje dat (matoucí)*  

![][5]

*Obrázek 7: Funkce extrahována ze zdroje dat*
 

> Všimněte si, že tato data jsou privátní, a proto nelze sdílet hello modelu a data.
> Ale pro podobné model pomocí veřejně dostupnými daty, najdete v této ukázce experiment v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/): [Telco zákazníka změn](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Další informace o tom, jak můžete implementovat model analysis změn pomocí Cortana Intelligence Suite toolearn, doporučujeme také [toto video](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) podle hlavní programový manažer západní Hyong Tok. 
> 
> 

### <a name="algorithms-used-in-hello-prototype"></a>Algoritmy používané v hello prototypu
Použili jsme hello následující čtyři algoritmy strojového učení toobuild hello prototypu (žádné přizpůsobení):  

1. Logistic regression (LR)
2. Vylepšené rozhodovací strom (BT)
3. Zprůměrovanou perceptron (AP)
4. Support vector machine (SVM)  

Hello následující diagram znázorňuje část hello experimentu návrhovou plochu, který označuje hello pořadí, ve které hello byly vytvořeny modely:  

![][6]  

*Obrázek 8: Vytváření modelů v nástroji Machine Learning Studio*  

### <a name="scoring-methods"></a>Vyhodnocování metody
Jsme skóre pro magnitudu hello čtyři modely pomocí datovou sadu s popiskem školení.  

Odeslali jsme také hello vyhodnocování tooa porovnatelný z hlediska modelu datové sady vytvořené pomocí edice plochy hello SAS Enterprise analýzy 12. Jsme měří hello přesnost modelu hello SAS a všechny čtyři modely Machine Learning Studio.  

## <a name="results"></a>Výsledky
V této části nabízejí jsme našich zjištění o hello přesnost hello modely, v závislosti na hello vyhodnocování datovou sadu.  

### <a name="accuracy-and-precision-of-scoring"></a>Přesnost a přesnost vyhodnocování
Obecně platí hello implementace v Azure Machine Learning je za SAS přesnost asi 10 až 15 % (oblasti pod křivky nebo AUC).  

Hello nejdůležitější metriky v změn je však rychlost chybnou hello: tedy z hlavních churners hello jako předpokládaných pomocí třídění hello, který je ve skutečnosti nebyla **není** změn a ještě nebyly přijaty zvláštní zacházení? hello Následující diagram porovná tato chybnou rychlost pro všechny modely hello:  

![][7]

*Obrázek 9: Passau prototypu oblast pod křivkou*

### <a name="using-auc-toocompare-results"></a>Pomocí AUC toocompare výsledky
Oblasti v rámci křivky (AUC) je metrika, který představuje globální míru *separability* mezi hello distribuce skóre pro plnění kladné a záporné. Je podobné toohello tradiční grafu příjemce operátor vlastnosti (ROC), ale jeden důležitý rozdíl je, že metrika AUC hello není potřeba toochoose prahovou hodnotu. Místo toho shrnuje výsledky hello přes **všechny** možné volby. Naproti tomu hello tradiční ROC graf obsahuje kladné míra hello na hello svislé osy a hello false kladné sazby na vodorovné ose hello a prahová hodnota hello klasifikace se liší.   

AUC obvykle se používá jako míru vhodné pro různé algoritmy (nebo jiné systémy) protože umožňuje modely toobe porovnání prostřednictvím jejich AUC hodnoty. Toto je Oblíbené přístup v odvětví, jako je například meteorologie a biosciences. Proto AUC představuje oblíbených nástroj pro vyhodnocování třídění výkonu.  

### <a name="comparing-misclassification-rates"></a>Porovnání sazby chybnou klasifikaci
Počty chybnou hello v dotyčném datovou sadu hello jsme porovnání pomocí dat CRM hello přibližně 8 000 jednotek předplatných.  

* míra chybnou Hello SAS se 10 až 15 %.
* míra chybnou Hello Machine Learning Studio se 15-20 % pro churners prvních 200 300 hello.  

V hello telecommunications odvětví je důležité tooaddress jen zákazníků, kteří mají hello nejvyšší toochurn riziko podle jejich nabízení služby concierge nebo jiné speciální zacházení. V tomto ohledu dosahuje hello Machine Learning Studio implementace výsledky srovnatelné s modelu SAS hello.  

Podle hello, stejný token, přesnost je důležitější než přesnost, protože jsme nejvíce zajímají správně klasifikace potenciální churners.  

Hello z Wikipedia následující diagram znázorňuje vztah hello v živá, snadno pochopit obrázek:  

![][8]

*Obrázek 10: Kompromis mezi přesnost*

### <a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Přesnost výsledků pro model vylepšené rozhodovací strom
Následující graf zobrazuje hello nezpracovaná Hello výsledkem vyhodnocování pomocí hello Machine Learning prototypu pro hello boosted rozhodovacího stromu modelu, který se stane toobe hello nejpřesnější mezi čtyři modely hello:  

![][9]

*Obrázek 11: Vylepšené rozhodovací strom modelu vlastnosti*

## <a name="performance-comparison"></a>Porovnání výkonu
Jsme porovná hello rychlost, kdy byla data za použití modelů Machine Learning Studio hello a porovnatelný z hlediska model vytvořily s použitím hello plochy edici 12.1 analýzy Enterprise SAS.  

Hello následující tabulka shrnuje výkon hello hello algoritmy:  

*Tabulka 1. Obecný výkon (přesnost) algoritmů hello*

| LR | BT | AP | SVM |
| --- | --- | --- | --- |
| Průměrná modelu |Hello nejlepší modelu |Nevedou podle očekávání |Průměrná modelu |

modely Hello hostované v nástroji Machine Learning Studio outperformed SAS 15-25 % pro rychlost zpracování, ale přesnost byla z velké části na nominální.  

## <a name="discussion-and-recommendations"></a>Diskusní a doporučení
V hello telecommunications odvětví, několik postupů této služby tooanalyze změn, včetně:  

* Odvození metriky pro čtyři základní kategorií:
  * **Entita (například předplatné)**. Zřídit základní informace o odběru hello nebo zákazníkovi, který je předmětem hello změn.
  * **Aktivita**. Získejte všechny informace o možných využití, které je toohello související entity, například hello počet přihlášení.
  * **Zákaznická podpora**. Shromažďovat informace z tooindicate protokoly podpory zákazníků, zda hello odběru byla problémy nebo interakce s zákaznickou podporu.
  * **Produktivní a obchodních dat**. Získat žádné možné informace o hello zákazníků (například může být k dispozici nebo pevné tootrack).
* Použijte Výběr funkce toodrive význam. To znamená, že tento hello boosted rozhodovacího stromu modelu je vždy Slíbení přístup.  

Hello použití těchto čtyř kategorií vytváří iluzi hello který jednoduchou *deterministickou* přístup, založený na indexy vytvořen na přiměřenou faktorech podle kategorií, měla by stačit tooidentify zákazníkům znamenat nebezpečí změn. Bohužel i když tento pojem zdá se, že pravděpodobné, je false pochopení. Důvod Hello je, že změn je efekt dočasné a hello faktory, které přispívají toochurn se obvykle nachází ve přechodný stavy. Co vede na zákazníka tooconsider ponechat dnes můžou být různé zítra a jistě bude jiný šest měsíců od tohoto okamžiku. Proto *pravděpodobnosti* je nezbytné v modelu.  

Tato důležité pozorování bývá často přehlížen v podnikání, které obvykle upřednostní tooanalytics přístup orientovaný intelligence obchodní, nejčastěji, protože se jedná snadněji prodeje a zavést přehledné automatizace.  

Ale hello potenciálu samoobslužné analytické funkce pomocí Machine Learning Studio, že je hello čtyř kategorií informací, divize nebo oddělení, se stupněm cenné zdroj pro machine learning o změn.  

Další zajímavé funkce už v Azure Machine Learning je hello možnost tooadd úložiště toohello vlastní modul předdefinované modulů, které jsou již k dispozici. Tato funkce, které v podstatě vytvoří tooselect možnost knihovny a vytvoření šablon pro vertikální skupiny. Jde důležité rozdílem Azure Machine Learning v Marketplace hello.  

Věříme, že toocontinue v tomto tématu v hello toobig budoucí, zejména související data analýzy.
  

## <a name="conclusion"></a>Závěr
Tento dokument popisuje rozumný způsob tootackling hello běžné problém zákazníka změn pomocí obecné rozhraní. Jsme považována za prototypu pro vyhodnocování modely a implementovat pomocí Azure Machine Learning. Nakonec jsme hodnoceno hello přesnost a výkonu hello prototypu řešení s ohledem toocomparable algoritmy v SAS.  

**Další informace:**  

Tento dokument vám pomohou? Prosím sdělte svůj názor. Řekněte nám na škále od 1 (špatné) too5 (vynikající), jak hodnotíte tento dokument a proč jste udělili ho tato hodnocení? Například:  

* Jsou vám hodnocení ho vysoké kvůli toohaving dobrými příklady, vynikající snímků obrazovek, zrušte zápis nebo jiného důvodu?
* Jsou vám hodnocení ho nízkou kvůli toopoor příklady, snímky obrazovky přibližné nebo jasné zápisu?  

Tato zpětná vazba pomůže nám vylepšit kvalitu hello dokumenty white paper, které jsme vydání.   

[Pošlete svůj názor](mailto:sqlfback@microsoft.com).
 

## <a name="references"></a>Odkazy
[1] prediktivní analýzy: nad rámec hello předpovědi k. McKnight, informace správy, červenec/srpen 2011, p.18-20.  

[2] Wikipedia článek: [přesnost](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [OSTRÉ DM 1.0: Průvodce dolování dat krok za krokem](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [Marketing velkých objemů dat: zákazníky oslovíte efektivněji a jednotky hodnota](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco změn šablony modelu](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) 
 

## <a name="appendix"></a>Příloha
![][10]

*Obrázek 12: Snímek prezentace na prototypu změn*

[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
