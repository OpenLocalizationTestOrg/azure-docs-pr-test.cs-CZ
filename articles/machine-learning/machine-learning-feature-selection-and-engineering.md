---
title: "aaaFeature inženýrství a výběr v Azure Machine Learning | Microsoft Docs"
description: "Popisuje účely hello funkce Výběr a konstruování funkce a obsahuje příklady jejich role v hello proces rozšíření dat machine learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 9ceb524d-842e-4f77-9eae-a18e599442d6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2017
ms.author: zhangya;bradsev
ROBOTS: NOINDEX
redirect_url: machine-learning-data-science-create-features
redirect_document_id: True
ms.openlocfilehash: e3e59329bf46f334396f5975b4e656137362d7ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Konstrukce a výběr funkcí ve službě Azure Machine Learning
Toto téma vysvětluje hello účely funkce inženýrství a výběr funkce v rozšíření dat procesu hello strojové učení. Ukazuje, co tyto procesy zahrnovat pomocí příklady poskytované Azure Machine Learning Studio.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Hello Cvičná data používané pro strojové učení se dá vylepšit často hello výběr nebo extrakce funkcí z hello nezpracovaná data shromážděná. Příklad inženýrství funkce v kontextu hello učení, jak je tooclassify hello bitové kopie ručně psané znaky hustotu bitové mapy vytvářejí na základě data distribuce nezpracovaná bit hello. Tato mapa vám může pomoci efektivnější než nezpracovaná distribuční hello nalézt hello hrany hello znaků.

Funkce inženýrství a vybrané zvýšení efektivity hello hello školení procesu, který se pokusí tooextract hello klíčové informace obsažené v datech hello. Také zvyšují hello power vstupních dat tyto modely tooclassify hello přesně a výstupy toopredict zajímají více robustní. Funkce inženýrství a výběr můžete také kombinovat toomake hello learning výpočetně tractable. Dělá to pomocí rozšíření a potom snižuje hello řadu funkcí toocalibrate nebo train model potřeba. Matematický platí, že jsou hello funkce vybrané tootrain hello modelu minimálním počtem nezávislých proměnných, které popisují hello vzorů v datech hello a pak úspěšně předpovědi výstupy.

Hello inženýrství a výběr funkcí je jednou ze součástí větší procesu, který se obvykle skládá z čtyři kroky:

* Shromažďování dat
* Rozšíření dat
* Konstrukce modelu
* Po zpracování.

Technici a výběr tvoří hello data vylepšení kroku strojové učení. Tři aspekty tohoto procesu může být rozlišující pro naše účely:

* **Předběžné zpracování dat**: Tento proces je tooensure pokusů, který hello shromážděná data vyčištění a konzistentní. Obsahuje úlohy, jako je integrace více datových sad, zpracování chybějící data, zpracování nekonzistentní data a převádění datových typů.
* **Konstruování**: Tento proces pokusí další relevantní funkce toocreate z existující nezpracovaná funkce hello v hello dat a tooincrease prediktivní power toohello algoritmus učení.
* **Funkce Výběr**: Tento proces vybere hello klíče podmnožinu původní data funkce tooreduce hello dimenzionalitu hello školení problému.

Toto téma popisuje pouze hello funkce inženýrství a funkce Výběr aspekty procesu vylepšení dat hello. Další informace o kroku předběžného zpracování dat hello najdete v tématu [předběžné zpracování dat v Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).

## <a name="creating-features-from-your-data--feature-engineering"></a>Vytváření funkcí z vašich dat – konstruování
Hello Cvičná data se skládá z matice tvořený příklady (záznamy nebo připomínky, které jsou uložené v řádcích), z nichž každý má sadu funkcí (proměnné nebo pole, které jsou uloženy ve sloupcích). Funkce Hello zadaný v hello experimentální návrhu jsou očekávané toocharacterize hello vzorů v datech hello. I když řadu hello nezpracovaná data, která pole můžou být přímo součástí hello vybranou funkci sady použité tootrain model, další inženýrství funkce často potřebují toobe vytvářejí na základě funkcí hello v toogenerate nezpracovaná data hello rozšířené trénovací datové sady.

Jaký druh funkce mají být vytvořeny tooenhance hello datové sady při tréninku modelu? Inženýrství funkce, které zlepšují hello školení poskytují informace, které lépe odlišuje hello vzorů v datech hello. Očekáváte, že hello nové funkce tooprovide Další informace, které není jasně zaznamenané nebo snadno zřejmá v původní hello nebo existující funkce, ale tento proces je něco obrázky. Rozhodnutí o zvukové a produktivitu často vyžadují znalosti některé domény.

Při spouštění pomocí Azure Machine Learning, je nejjednodušší toograsp tento proces namítají pomocí ukázky součástí Machine Learning Studio. Zde jsou uvedeny dva příklady:

* Příklad regrese ([předpovědi hello počet kolo pronájem](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) v pod dohledem experimentu, kde jsou známé hello cílové hodnoty
* Pomocí příklad text dolování klasifikace [Hashování][feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Příklad 1: Přidání dočasné funkce pro regresní model
toodemonstrate způsobu tooengineer funkcí pro úlohu regrese použijeme hello experimentu "vyžádání prognózy z kol" v nástroji Azure Machine Learning Studio. Hello cílem tohoto experimentu je toopredict hello vyžádání hello kol, který je hello počet kolo pronájem v rámci konkrétní měsíc, den nebo hodinu. datové sady Hello **kolo pronájem UCI datové sady** slouží jako hello nezpracovaná vstupní data.

Tato datová sada je založena na reálná data z hello Bikeshare velké společnosti, která udržuje síť pronájem kolo v Washington DC ve Spojených státech amerických hello. datové sady Hello představuje hello počet kolo pronájem v rámci konkrétní hodinu dne, 2011 too2012, která obsahuje 17379 řádků a sloupců 17. Sada funkcí nezpracovaná Hello obsahuje počasí (teploty, vlhkosti, rychlosti větru) a typ hello hello den (svátek nebo den v týdnu). pole toopredict Hello je **cnt**, počet představující hello kolo pronájem v rámci konkrétní hodinu a které rozsahy z 1 too977.

tooconstruct efektivní funkce hello Cvičná data, čtyři regrese modely jsou vytvořeny pomocí hello stejné algoritmus, ale s čtyř různých Cvičná data nastaví. datové sady představují Hello čtyři hello stejné nezpracovaná vstupní data, ale se zvyšující číslo funkce nastavit. Tyto funkce jsou rozděleny do čtyř kategorií:

1. A = počasí + svátek + den v týdnu + hello předpokládaných den víkendu funkce
2. B = počet kol, které byly pronajaté v každé z hello předchozí 12 hodin
3. C = počet kol, které byly pronajaté v každé z hello předchozích 12 dnů v hello stejné hodinu
4. D = počet kol, které byly pronajaté v každé z hello předchozí 12 týdnů v hello stejné hodinu a hello stejný den

Kromě toho sada A funkce, která již existuje v původní nezpracovaná data hello, hello další tři sady funkcí se vytvoří pomocí funkce hello technici procesu. Funkci nastavit B zachycení hello posledního vyžádání hello kol. Funkci nastavit C zachycení hello vyžádání kol v konkrétní hodinu. Funkci zachycení vyžádání D kol nastavit na konkrétní hodiny a určitý den v týdnu hello. Každý z datových sad školení hello čtyři zahrnuje sady funkcí A, A + B, A + B + C a A + B + C + D, v uvedeném pořadí.

V hello experimentu Azure Machine Learning jsou tyto čtyři sady dat školení formátu prostřednictvím čtyři větve hello předem zpracovaných vstupní datové sadě. S výjimkou hello krajní levé větve, každý z těchto větve obsahuje [spustit skript jazyka R] [ execute-r-script] v uvedeném pořadí sestavený a připojí modul, ve kterém sada odvozené funkcí (funkce nastaví B, C a D) toohello importovat datové sady. Hello následující obrázek ukazuje, že hello R skript používá sada funkcí toocreate B v druhé levém větve hello.

![Vytvořit sadu funkcí](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

Hello následující tabulka shrnuje hello porovnání výsledků výkonu hello čtyři modely hello. Nejlepších výsledků dosáhnete Hello se zobrazí funkce A + B + C. Všimněte si, že míra chyb hello sníží při zahrnuje další funkce sady hello Cvičná data. Tím se ověří naše předpoklad, který hello sady funkcí, které B a C poskytují další relevantní informace pro úlohu regrese hello. Přidání sada funkcí hello D pravděpodobně tooprovide jakékoli další snížení míra chyb hello.

![Porovnejte výsledky výkonu](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Příklad 2: Vytvoření funkce v textu dolování
Funkce inženýrství je široce použity v úlohy související tootext dolování, jako je klasifikace a postojích analýzy dokumentu. Například když chcete tooclassify dokumenty do několika kategorií, typické předpokladem je, že hello slova nebo fráze součástí jeden dokument kategorie jsou méně pravděpodobné, že toooccur v jiné kategorii dokumentu. Jinými slovy hello frekvence rozdělení hello slovo nebo frázi není možné toocharacterize jiného dokumentu kategorií. V aplikacích dolování textu hello funkce technici proces je potřebné toocreate hello funkcí zahrnující frekvencí slovo nebo frázi, protože jednotlivé obsah textu obvykle slouží jako hello vstupní data.

tooachieve tato úloha, techniku zvanou *hashování* je použité tooefficiently zapnout funkce libovolný text do indexů. Místo přidružení každý text funkce (slova nebo fráze) tooa konkrétní index, tato metoda funkce s použitím funkce toohello funkce hash a pomocí jejich hodnoty hash jako indexy, které přímo.

V Azure Machine Learning je [Hashování] [ feature-hashing] modul, který vytvoří tyto funkce slovo nebo frázi. Hello následující obrázek znázorňuje příklad použití tohoto modulu. Hello vstupní datové sady obsahuje dva sloupce: hodnocení kniha hello od 1 too5 a hello skutečný kontrolní obsahu. cílem Hello tohoto [Hashování] [ feature-hashing] modul je tooretrieve nové funkce, které ukazují četnost výskytu hello hello odpovídající slova nebo fráze v rámci recenze konkrétní knihy hello. toouse tento modul je třeba toocomplete hello následující kroky:

1. Vyberte hello sloupec, který obsahuje vstupní textu hello (**Col2** v tomto příkladu).
2. Nastavit *algoritmu hash bitsize* too8, což znamená, 2 ^ 8 = 256 funkcí se vytvoří. Hello slovo nebo frázi v textu hello je pak too256 indexy s použitím algoritmu hash. Hello parametr *algoritmu hash bitsize* rozmezí od 1 too31. Pokud parametr hello nastavená tooa větší číslo, hello slova nebo fráze je pravděpodobnější toobe hash do hello stejný index.
3. Nastavte parametr hello *N-gram* too2. Tato četnost výskytu hello unigrams (funkce pro každý jednoslovné) a bigrams (funkce pro každý pár slov sousední) načte ze vstupního textu hello. Hello parametr *N-gram* rozmezí od 0 too10, která určuje maximální počet po sobě jdoucích slova toobe součástí funkce hello.  

![Funkce hash modulu](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

Hello následující obrázek ukazuje, jak vypadají tyto nové funkce.

![Příklad funkce hash](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Filtrování funkce z vašich dat – Výběr funkce
*Funkce Výběr* je proces, který se často použitá toohello konstrukce školení datové sady pro prediktivní modelování úlohy, například klasifikační nebo regresní úlohy. cílem Hello je tooselect podmnožinu funkcí hello z hello původní datovou sadou, která omezuje jeho dimenze pomocí minimální sadu funkcí toorepresent hello maximální množství odchylky v datech hello. Tuto podmnožinu funkcí, obsahuje hello pouze funkce toobe zahrnuté tootrain hello modelu. Výběr funkce má dva hlavní účely:

* Výběr funkce často zvyšuje přesnost klasifikace odstraněním důležité, redundantní nebo vysoce korelují funkce.
* Funkce Výběr snížení hello řadu funkcí, které umožňuje zefektivnit proces školení hello modelu. To je obzvláště důležité pro inteligentních algoritmů, které jsou nákladné tootrain například support vector počítače.

I když výběr funkce bude hledat tooreduce hello řadu funkcí v hello datové sady používá tootrain hello modelu, nejedná se obvykle označuje termín hello tooby *snížení dimenzionalitu.* Metody výběru funkce extrahujte podmnožinu původní funkcí v datech hello bez jejich změny.  Metody snížení dimenzionalitu využívají inženýrství funkce, které můžete transformace hello původní funkcí a proto je upravit. Příklady dimenzionalitu snížení metody: Analýza hlavní komponenty, kanonický korelace analýzu a rozložením singulární hodnotu.

Jednu kategorii široce použité metody výběru funkcí v pod dohledem kontextu je výběr funkce na základě filtru. Vyhodnocením hello korelace mezi každý atribut target funkce a hello použít tyto metody statistické měr tooassign funkce tooeach skóre. Hello funkcí, jsou pak seřazeny podle hello skóre, které můžete použít pro zachování nebo odstranění konkrétní funkci tooset hello prahovou hodnotu. Příklady hello statistické míry použít v těchto metod: Pearson korelace, vzájemné informace a test chí-kvadrát hello.

Azure Machine Learning Studio poskytuje moduly pro výběr funkce. Jak ukazuje následující obrázek hello, patří tyto moduly [na základě filtru výběr funkce] [ filter-based-feature-selection] a [Fisherovy lineární analýza Discriminant] [ fisher-linear-discriminant-analysis].

![Příklad výběru funkce](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)

Například použít hello [na základě filtru výběr funkce] [ filter-based-feature-selection] modul s příklad dolování textu hello popsané dříve. Předpokládejme, že chcete toobuild regresní model po vytvoření sadu funkcí, 256 prostřednictvím hello [Hashování] [ feature-hashing] modulu a tuto proměnnou odpovědi hello je **Sloupec1**a představuje kniha zkontrolujte hodnocení od 1 too5. Nastavit **funkci vyhodnocování metoda** příliš**Pearson korelace**, **cílový sloupec** příliš**Sloupec1**, a **počet požadovaných funkce** příliš**50**. modul Hello [na základě filtru výběr funkce] [ filter-based-feature-selection] pak vytvoří sadu dat obsahující 50 funkcí společně s atribut target hello **Sloupec1**. Hello následující obrázek ukazuje hello toku tohoto experimentu a hello vstupní parametry.

![Příklad výběru funkce](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

Hello následující obrázek znázorňuje hello výsledné datové sady. Jednotlivé funkce skóre pro magnitudu na základě na hello Pearson korelace mezi samostatně a hello atribut target **Sloupec1**. Funkce Hello s nejvyšší skóre jsou zachovány.

![Funkci na základě filtru výběr datových sad](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Následující obrázek ukazuje Hello hello odpovídající skóre hello vybrané funkce.

![Vybrané funkce skóre](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Použitím to [na základě filtru výběr funkce] [ filter-based-feature-selection] modulu, 50 mimo 256 funkce jsou vybrané, protože mají hello většinu funkcí korelační s hello Cílová proměnná **Sloupec1** podle hello vyhodnocování metoda **Pearson korelace**.

## <a name="conclusion"></a>Závěr
Funkce inženýrství a výběr funkce jsou dva kroky prováděných tooprepare hello Cvičná data při sestavování model machine learning. Za normálních okolností inženýrství funkce je použité první toogenerate další funkce, a poté kroku výběru funkce hello provádět tooeliminate důležité, redundantní nebo vysoce korelační funkce.

Není vždy nutně tooperform výběr inženýrství nebo funkce funkce. Jestli je to potřeba, závisí na hello dat máte nebo shromažďovat, hello algoritmus, který vyberete a hello cíl hello experimentu.

<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
