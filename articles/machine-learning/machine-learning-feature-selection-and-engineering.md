---
title: "Funkce inženýrství a výběr v Azure Machine Learning | Microsoft Docs"
description: "Popisuje účely funkce Výběr a konstruování funkce a obsahuje příklady jejich role v rozšíření dat proces strojové učení."
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
redirect_document_id: TRUE
ms.openlocfilehash: 51a5d8fed492cb9301e048c2b6a721e4573a47d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Konstrukce a výběr funkcí ve službě Azure Machine Learning
Toto téma vysvětluje účely funkce inženýrství a výběr funkce v rozšíření dat procesu strojové učení. Ukazuje, co tyto procesy zahrnovat pomocí příklady poskytované Azure Machine Learning Studio.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Jsou Cvičná data používané pro strojové učení se dá vylepšit často výběr nebo extrakce funkcí z nezpracovaná data shromážděná. Příklad inženýrství funkce v kontextu naučit, jak klasifikovat bitové kopie ručně psané znaky je bit hustotu mapu vytvářejí na základě data distribuce nezpracovaná bit. Tato mapa může pomoci najít okrajů znaky efektivnější než nezpracovaná rozdělení.

Funkce inženýrství a vybrané zvýšit efektivitu proces školení, která se pokusí o extrahování klíčových informací obsažených v datech. Také zvyšují výkon těchto modelů přesně klasifikovat vstupních dat a k předvídání více robustní výsledky, které vás zajímají. Funkce inženýrství a výběr můžete také kombinovat aby výukové výpočetně tractable. Dělá to tak rozšíření a pak snižuje počet funkcí, které jsou potřeba k nakalibrovat nebo cvičení modelu. Funkce vybrané pro trénování modelu matematicky platí, že jsou minimální sadu nezávislých proměnných, které popisují vzorů v datech a pak úspěšně předpovědi výstupy.

Technici a výběr funkcí je jednou ze součástí větší procesu, který se obvykle skládá z čtyři kroky:

* Shromažďování dat
* Rozšíření dat
* Konstrukce modelu
* Po zpracování.

Technici a výběr tvoří krok vylepšení dat machine learning. Tři aspekty tohoto procesu může být rozlišující pro naše účely:

* **Předběžné zpracování dat**: Tento proces se pokusí zajistěte, aby byl na shromážděná data vyčištění a konzistentní. Obsahuje úlohy, jako je integrace více datových sad, zpracování chybějící data, zpracování nekonzistentní data a převádění datových typů.
* **Konstruování**: Tento proces se pokusí vytvořit další relevantní funkce z existující nezpracovaná funkce v datech a ke zvýšení výkonu prediktivní k algoritmus učení.
* **Funkce Výběr**: Tento proces vybere klíče podmnožinu funkcí původní data ke snížení dimenzionalitu problému školení.

Toto téma popisuje pouze inženýrství funkce a funkce Výběr aspektů proces vylepšení data. Další informace o kroku předběžného zpracování dat najdete v tématu [předběžné zpracování dat v Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).

## <a name="creating-features-from-your-data--feature-engineering"></a>Vytváření funkcí z vašich dat – konstruování
Jsou Cvičná data se skládá z matice tvořený příklady (záznamy nebo připomínky, které jsou uložené v řádcích), z nichž každý má sadu funkcí (proměnné nebo pole, které jsou uloženy ve sloupcích). Očekává se, že funkce zadané v experimentální návrhu charakterizovat vzorů v datech. I když mnoho polí nezpracovaná data mohou být přímo součástí sadu vybranou funkci použity při cvičení modelu, další inženýrství funkce často potřebují zkonstruovat z funkcí v nezpracovaná data pro generování rozšířené trénovací datové sady.

Jaký druh funkce by měl být vytvořen k vylepšení v datové sadě při tréninku modelu? Inženýrství funkce, které zlepšují školení poskytují informace, které lépe odlišuje vzorů v datech. Očekáváte, že nové funkce, které chcete poskytnout další informace, které není zaznamenaná jasně nebo snadno zřejmá v sadě původního nebo existující funkce, ale tento proces je něco obrázky. Rozhodnutí o zvukové a produktivitu často vyžadují znalosti některé domény.

Při spouštění pomocí Azure Machine Learning, je to nejjednodušší provádět uchopit tento proces namítají pomocí ukázky v Machine Learning Studio. Zde jsou uvedeny dva příklady:

* Příklad regrese ([předpovědi počet kolo pronájem](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) v pod dohledem experimentu, kde jsou známé cílové hodnoty
* Pomocí příklad text dolování klasifikace [Hashování][feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Příklad 1: Přidání dočasné funkce pro regresní model
K ukazují, jak analyzovat funkce pro úlohu regrese, použijeme experimentu "vyžádání prognózy z kol" v nástroji Azure Machine Learning Studio. Cílem tohoto experimentu je k předvídání požadavků na kol, který je počet kolo pronájem v rámci konkrétní měsíc, den nebo hodinu. V datové sadě **kolo pronájem UCI datové sady** slouží jako nezpracovaná vstupní data.

Tato datová sada je založena na skutečná data od společnosti Bikeshare velké, která udržuje síť kolo pronájem v Washington DC ve Spojených státech amerických. Sada dat představuje počet kolo pronájem v rámci konkrétní hodinu dne, 2011 2012, která obsahuje 17379 řádků a sloupců 17. Nezpracovaná funkce sada obsahuje počasí (teploty, vlhkosti, rychlosti větru) a typ den (svátek nebo den v týdnu). Pole k předpovědi je **cnt**, počet představující pronájem kolo v rámci konkrétní hodinu a který pohybuje od 1 do 977.

Vytvořit efektivní funkce jsou Cvičná data, jsou čtyři modely regrese vytvořené pomocí stejného algoritmu, ale s čtyř různých školení datových sad. Čtyři datové sady představují stejnou nezpracovaná vstupní data, ale se zvyšující číslo funkce nastavit. Tyto funkce jsou rozděleny do čtyř kategorií:

1. A = počasí + svátek + den v týdnu + předpokládaných den víkendu funkce
2. B = počet kol, které byly pronajaté v každé z předchozích 12 hodin
3. C = počet kol, které byly pronajaté v každé z předchozích 12 dnů v tutéž hodinu
4. D = počet kol, které byly pronajaté v každé z předchozí 12 týdnů v tutéž hodinu a den

Kromě toho sada A funkce, která již existuje v původní nezpracovaná data, další tři sady funkcí se vytvoří pomocí funkce technici procesu. Funkci nastavit B zachycení poslední poptávka po kol. Funkci nastavit C zachycení vyžádání kol v konkrétní hodinu. Funkci nastavit D zachycení vyžádání kol na konkrétní hodiny a určitý den v týdnu. Všechny čtyři datových sad školení zahrnuje sady funkcí A, A + B, A + B + C a A + B + C + D, v uvedeném pořadí.

V rámci experimentu Azure Machine Learning jsou tyto čtyři sady dat školení formátu prostřednictvím čtyři větve v předem zpracovaných vstupní datové sadě. S výjimkou krajní levé větve, každý z těchto větve obsahuje [spustit skript jazyka R] [ execute-r-script] modul, ve kterém sada odvozené funkcí (funkce nastaví B, C a D) v uvedeném pořadí sestavený a připojeno k importované datové sady. Následující obrázek ukazuje skriptu jazyka R použít k vytvoření sada funkcí B v druhé levém větve.

![Vytvořit sadu funkcí](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

Následující tabulka shrnuje porovnání výsledků výkonu čtyři modely. K dosažení nejlepších výsledků se zobrazí funkce A + B + C. Všimněte si, že snižuje míra chyb při zahrnuje další funkce sady jsou Cvičná data. Ověříte naše předpoklad, že sady funkcí B a C poskytovat další relevantní informace pro regresní úlohu. Přidání sadu funkcí D zřejmě není zadejte jakékoli další snížení míra chyb.

![Porovnejte výsledky výkonu](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Příklad 2: Vytvoření funkce v textu dolování
Funkce inženýrství je široce použity v úlohy související s dolování text, například klasifikace a postojích analýzy dokumentu. Například pokud chcete klasifikovat dokumenty do několika kategorií, typické předpokladem je, že slova nebo fráze zahrnuté v jedné kategorii dokumentu jsou méně pravděpodobné, ke kterým došlo v jiné kategorii dokumentu. Jinými slovy frekvence slovo nebo frázi rozdělení se moci charakterizovat dokumentu různých kategorií. V aplikacích dolování text je nutné vytvořit funkcí zahrnující frekvencí slovo nebo frázi, protože jednotlivé obsah textu obvykle slouží jako vstupní data funkce technici procesu.

K dosažení této úlohy, o techniku názvem *hashování* se použije pro efektivní zapnout funkce libovolný text do indexů. Místo na konkrétní index, tato metoda funkce použitím funkce hash na funkce a jejich hodnoty hash jako indexy, které přímo pomocí přidružení každou funkci text (slova nebo fráze).

V Azure Machine Learning je [Hashování] [ feature-hashing] modul, který vytvoří tyto funkce slovo nebo frázi. Následující obrázek znázorňuje příklad použití tohoto modulu. Vstupní datová sada obsahuje dva sloupce: hodnocení adresáře v rozsahu od 1 do 5 a skutečnou zkontrolovat obsah. Cílem tohoto [Hashování] [ feature-hashing] modul je načíst nové funkce, které se zobrazí četnost výskytu odpovídající slova nebo fráze v rámci konkrétního seznamu zkontrolujte. Chcete-li použít tento modul, proveďte následující kroky:

1. Vyberte sloupec, který obsahuje vstupní text (**Col2** v tomto příkladu).
2. Nastavit *algoritmu hash bitsize* na 8, což znamená, 2 ^ 8 = 256 funkcí se vytvoří. Slovo nebo frázi v textu se pak rozdělí 256 indexy. Parametr *algoritmu hash bitsize* rozmezí od 1 do 31. Pokud parametr je nastaven na větší číslo, jsou méně pravděpodobné, že se rozdělily do stejné indexu hesla či fráze.
3. Nastavte parametr *N-gram* 2. Tato četnost výskytu unigrams (funkce pro každý jednoslovné) a bigrams (funkce pro každý pár slov sousední) načte ze vstupního textu. Parametr *N-gram* rozmezí od 0 do 10, která určuje maximální počet po sobě jdoucích slova mají být zahrnuty do funkce.  

![Funkce hash modulu](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

Následující obrázek ukazuje, jak vypadají tyto nové funkce.

![Příklad funkce hash](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Filtrování funkce z vašich dat – Výběr funkce
*Funkce Výběr* je proces, který se běžně použije ke konstrukci školení datové sady pro prediktivní modelování úlohy, například klasifikační nebo regresní úlohy. Cílem je vybrat podmnožinu funkcí z původní sady dat, která omezuje jeho dimenze pomocí minimální sadu funkcí, představují maximální množství odchylky v datech. Tuto podmnožinu funkcí, obsahuje pouze funkce, které chcete zahrnout pro trénování modelu. Výběr funkce má dva hlavní účely:

* Výběr funkce často zvyšuje přesnost klasifikace odstraněním důležité, redundantní nebo vysoce korelují funkce.
* Výběr funkce snižuje počet funkcí, který umožňuje zefektivnit proces školení modelu. To je obzvláště důležité pro inteligentních algoritmů, které jsou nákladné ke cvičení například support vector počítače.

I když bude hledat výběr funkce. omezte počet funkcí v sadě dat, které jsou použity při cvičení modelu, ho není odkazuje obvykle termín *snížení dimenzionalitu.* Metody výběru funkce extrahujte podmnožinu původní funkcí v datech bez jejich změny.  Metody snížení dimenzionalitu využívají inženýrství funkce, které můžete transformace původní funkcí a proto je upravit. Příklady dimenzionalitu snížení metody: Analýza hlavní komponenty, kanonický korelace analýzu a rozložením singulární hodnotu.

Jednu kategorii široce použité metody výběru funkcí v pod dohledem kontextu je výběr funkce na základě filtru. Vyhodnocením korelace mezi jednotlivé funkce a atribut target použít tyto metody statistické míru přiřadit skóre pro každou funkci. Funkce, pak jsou seřazeny podle skóre, který můžete použít k nastavení prahové hodnoty pro zachování nebo odstranění konkrétní funkci. Příklady statistické míry použít v těchto metod: Pearson korelace, vzájemné informace a test chí-kvadrát.

Azure Machine Learning Studio poskytuje moduly pro výběr funkce. Jak je znázorněno na následujícím obrázku, patří tyto moduly [na základě filtru výběr funkce] [ filter-based-feature-selection] a [Fisherovy lineární analýza Discriminant][fisher-linear-discriminant-analysis].

![Příklad výběru funkce](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)

Například použít [na základě filtru výběr funkce] [ filter-based-feature-selection] modul s textovém příkladu dolování popsané dříve. Předpokládejme, že budete chtít vytvořit regresní model po vytvoření sadu funkcí, 256 prostřednictvím [Hashování] [ feature-hashing] modulu a že je proměnná odpovědi **Sloupec1** a představuje kniha zkontrolujte hodnocení od 1 do 5. Nastavit **funkci vyhodnocování metoda** k **Pearson korelace**, **cílový sloupec** k **Sloupec1**, a **počet požadované funkce** k **50**. Modul [na základě filtru výběr funkce] [ filter-based-feature-selection] pak vytvoří sadu dat obsahující 50 funkcí společně s atribut target **Sloupec1**. Následující obrázek znázorňuje tok tohoto experimentu a vstupní parametry.

![Příklad výběru funkce](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

Následující obrázek znázorňuje výsledné datových sad. Jednotlivé funkce skóre pro magnitudu na základě na Pearson korelace mezi samostatně a atribut target **Sloupec1**. Funkce s nejvyšší skóre jsou zachovány.

![Funkci na základě filtru výběr datových sad](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Následující obrázek znázorňuje odpovídající skóre vybraných funkcí.

![Vybrané funkce skóre](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Použitím to [na základě filtru výběr funkce] [ filter-based-feature-selection] modulu, 50 mimo 256 funkce jsou vybrané, protože mají nejvíc funkcí korelační s cílovou proměnnou **Sloupec1** založené na metodě vyhodnocování **Pearson korelace**.

## <a name="conclusion"></a>Závěr
Funkce inženýrství a výběr funkce jsou dva kroky, které se běžně provádějí při sestavování model machine learning Příprava Cvičná data. Za normálních okolností funkce inženýrství se použije první ke generování další funkce, a pak se provádí v kroku výběr funkce eliminovat důležité, redundantní nebo vysoce korelační funkce.

Není vždy nutně provést výběr funkce inženýrství nebo funkce. Jestli je potřeba závisí na mít nebo shromažďovat data, na algoritmus, který vyberete a cíl experimentu.

<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
