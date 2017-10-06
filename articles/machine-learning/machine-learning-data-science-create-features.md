---
title: "aaaFeature technickému týmu v vědecké zpracování dat | Microsoft Docs"
description: "Popisuje účely hello funkce inženýrství a obsahuje příklady jeho role v hello data vylepšení proces strojové učení."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 3fde69e8-5e7b-49ad-b3fb-ab8ef6503a4d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: zhangya;bradsev
ms.openlocfilehash: af40ea9cc9395bc87fe695eeaef26aa71e0ec9e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="feature-engineering-in-data-science"></a>Funkce analýzy v vědecké zpracování dat
Toto téma popisuje účely hello funkce inženýrství a obsahuje příklady jeho role v hello data vylepšení proces strojové učení. Příklady Hello používá tooillustrate tohoto procesu jsou vykreslovány z Azure Machine Learning Studio. 

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

To **nabídky** odkazy tootopics, které popisují, jak funkce toocreate pro data v různých prostředích. Tato úloha je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Funkce engineering pokusy o tooincrease hello předpovídat učení algoritmy vytvořením funkce z nezpracovaná data, která zefektivnit hello učení procesu. Dobrý den, technici a výběr funkcí je jednou ze součástí sady hello TDSP uvedených v hello [co je hello procesu vědecké účely Team datového cyklu?](data-science-process-overview.md) Funkce inženýrství a výběr jsou části hello **vyvíjet funkce** krok hello TDSP. 

* **konstruování**: Tento proces pokusí další relevantní funkce toocreate z existující nezpracovaná funkce hello v datech hello a tooincrease hello předpovídat algoritmus učení hello.
* **funkce Výběr**: Tento proces vybere hello klíče podmnožinu funkcí, původní data v pokusu o tooreduce hello dimenzionalitu hello školení problému.

Za normálních okolností **konstruování** je použité první toogenerate další funkce a pak hello **funkci Výběr** krok se provádí tooeliminate důležité, redundantní nebo vysoce korelační funkce.

Hello Cvičná data používané pro strojové učení se dá vylepšit často podle extrakce funkcí z hello nezpracovaná data shromážděná. Příklad inženýrství funkce v kontextu hello učení jak tooclassify hello bitové kopie ručně psané znaky je vytvoření trochu hustotu mapy vytvářejí na základě data distribuce nezpracovaná bit hello. Tato mapa vám může pomoci efektivnější než jednoduše přímo pomocí nezpracovaná distribuční hello nalézt hello hrany hello znaků.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="creating-features-from-your-data---feature-engineering"></a>Vytváření funkcí z vašich dat – konstruování
Hello Cvičná data se skládá z matice tvořený příklady (záznamy nebo připomínky, které jsou uložené v řádcích), z nichž každý má sadu funkcí (proměnné nebo pole, které jsou uloženy ve sloupcích). Funkce Hello zadaný v hello experimentální návrhu jsou očekávané toocharacterize hello vzorů v datech hello. I když řadu hello nezpracovaná data, která pole můžou být přímo součástí hello vybranou funkci sady použité tootrain model, je často případ hello, vyžadující toobe vytvářejí na základě funkcí hello v toogenerate nezpracovaná data hello rozšířené (inženýrství) další funkce Datová sada školení.

Jaký druh funkce mají být vytvořeny datovou sadu hello tooenhance při tréninku modelu? Inženýrství funkce, které zlepšují hello školení poskytují informace, které lépe odlišuje hello vzorů v datech hello. Očekáváme, že hello nové funkce tooprovide Další informace, které není sada funkcí původního nebo existující jasně zaznamenané nebo snadno zřejmá v hello. Ale tento proces je něco obrázky. Rozhodnutí o zvukové a produktivitu často vyžadují znalosti některé domény.

Při spouštění pomocí Azure Machine Learning, je nejjednodušší toograsp tento proces namítají pomocí ukázky součástí hello Studio. Zde jsou uvedeny dva příklady:

* Příklad regrese [předpovědi hello počet kolo pronájem](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) v pod dohledem experimentu, kde jsou známé hello cílové hodnoty
* Pomocí příkladu text dolování klasifikace [Hashování](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

## <a name="example-1-adding-temporal-features-for-regression-model"></a>Příklad 1: Přidání dočasné funkce pro regresní Model
Umožňuje použití hello experimentu "vyžádání prognózy z kol" v Azure Machine Learning Studio toodemonstrate jak tooengineer funkce pro úlohu regrese. Hello cílem tohoto experimentu je toopredict hello vyžádání hello kol, který je hello počet kolo pronájem v rámci konkrétní měsíc/den/hodinu. Hello datovou sadu "kolo pronájem UCI datovou sadu" slouží jako hello nezpracovaná vstupní data. Tato datová sada vychází reálná data z hello Bikeshare velké společnosti, která udržuje síť pronájem kolo v Washington DC ve Spojených státech amerických hello. Datová sada Hello představuje hello počet kolo pronájem v rámci konkrétní hodinu dne v letech hello 2011 a roce 2012 a obsahuje 17379 řádků a sloupců 17. Sada funkcí nezpracovaná Hello obsahuje počasí (teplotní/vlhkosti/větru rychlost) a typ hello hello den (svátek nebo den v týdnu). pole toopredict Hello je "cnt", počet, který reprezentuje hello kolo pronájem v rámci konkrétní hodinu a které rozsahy rozmezí od 1 too977.

S cílem hello vytváření efektivní funkce hello Cvičná data, jsou čtyři modely regrese sestaveny pomocí hello stejný algoritmus, ale s čtyř různých školení datové sady. datové sady představují Hello čtyři hello stejné nezpracovaná vstupní data, ale se zvyšující číslo funkce nastavit. Tyto funkce jsou rozděleny do čtyř kategorií:

1. A = počasí + svátek + den v týdnu + hello předpokládaných den víkendu funkce
2. B = počet kol, které byly pronajaté v každé z hello předchozí 12 hodin
3. C = počet kol, které byly pronajaté v každé z hello předchozích 12 dnů v hello stejné hodinu
4. D = počet kol, které byly pronajaté v každé z hello předchozí 12 týdnů v hello stejné hodinu a hello stejný den

Kromě toho sada A funkce, které již existují v původní nezpracovaná data hello, hello další tři sady funkcí se vytvoří pomocí funkce hello technici procesu. Funkci nastavit B zachycení poslední vyžádání hello kol. Funkci nastavit C zachycení hello vyžádání kol v konkrétní hodinu. Funkci zachycení vyžádání D kol nastavit na konkrétní hodiny a určitý den v týdnu hello. Hello čtyři školení datové sady každý zahrnuje funkce sada A, A + B, A + B + C a A + B + C + D.

V hello experimentu Azure Machine Learning jsou tyto čtyři datové sady školení formátu prostřednictvím čtyři větve z hello předem zpracovaných vstupní datové sady. S výjimkou hello vlevo většina větve, každý z těchto větve obsahuje [spustit skript jazyka R](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) modul, ve kterém sada odvozené funkcí (funkci nastavit B, C a D) jsou datové sady v uvedeném pořadí vytvořený a připojením toohello importovat. Hello následující obrázek ukazuje, že hello R skript používá sada funkcí toocreate B v druhé levém větve hello.

![Vytvoření funkce](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

Hello porovnání výsledků výkonu hello hello čtyři modely jsou shrnuty v následující tabulka hello. Nejlepších výsledků dosáhnete Hello se zobrazí funkce A + B + C. Všimněte si, že hello snížení míry chyba, když sada dalších funkcí jsou součástí hello Cvičná data. Ověří, naše předpoklad, že sada funkcí hello B, C poskytovat další relevantní informace pro úlohu regrese hello. Ale přidává funkce hello D nezdá tooprovide jakékoli další snížení míra chyb hello.

![porovnání výsledků](./media/machine-learning-data-science-create-features/result1.png)

## <a name="example2"></a>Příklad 2: Vytvoření funkce v textu dolování
Funkce inženýrství je široce použity v úlohy související tootext dolování, jako je klasifikace a postojích analýzy dokumentu. Například když chceme tooclassify dokumenty do několika kategorií, typické předpokladem je, že hello slova nebo fráze součástí jeden doc kategorie jsou méně pravděpodobné, že toooccur v jiné kategorii dokumentů. V jiné slova frekvence hello rozdělení slova nebo fráze hello je možné toocharacterize jiného dokumentu kategorií. V aplikacích dolování text protože jednotlivé obsah textu obvykle slouží jako vstupní data hello, funkce hello technici proces je potřebné toocreate hello funkcí zahrnující frekvencí slovo nebo frázi.

tooachieve tato úloha, techniku zvanou **hashování** je použité tooefficiently zapnout funkce libovolný text do indexů. Místo přidružení každý text funkce (slova nebo fráze) tooa konkrétní index, tato metoda funkce použití funkcí toohello funkce hash a použitím jejich hodnoty hash jako indexy, které přímo.

V Azure Machine Learning je [Hashování](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modul, který vytvoří tyto slovo nebo frázi funkce pohodlně. Následující obrázek znázorňuje příklad použití tohoto modulu. vstupní datové sady Hello obsahuje dva sloupce: hello kniha hodnocení rozsahu od 1 too5 a hello skutečný kontrolní obsah. cílem Hello tohoto [Hashování](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modulu je tooretrieve spoustu nových funkcí, které se zobrazí četnost výskytu hello hello odpovídající slova a phrase(s) v rámci recenze konkrétní knihy hello. toouse tento modul, potřebujeme toocomplete hello následující kroky:

* Nejdřív vyberte hello sloupec, který obsahuje vstupní textu hello ("Col2" v tomto příkladu).
* Druhý, nastavte hello too8 "Hashing bitsize", což znamená, 2 ^ 8 = 256 funkcí se vytvoří. Hello word či fázi v veškerého textu hello bude indexy hash too256. Parametr Hello "Hashing bitsize" v rozsahu od 1 too31. Hello slova / phrase(s) jsou méně pravděpodobné, že toobe hash do hello stejné indexu, pokud jeho nastavení toobe vyšší číslo.
* Třetí nastavte parametr "N-gram" too2 hello. To získá četnost výskytu hello unigrams (funkce pro každý jednoslovné) a bigrams (funkce pro každý pár slov sousední) ze vstupního textu hello. Hello parametr "N-gram" rozmezí od 0 too10, která určuje maximální počet po sobě jdoucích slova toobe hello součástí funkce.  

![Modul "Funkci Hashing"](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

Hello následující obrázek znázorňuje co hello tyto nové funkce vypadají.

![Příklad "Funkci Hashing"](./media/machine-learning-data-science-create-features/feature-Hashing2.png)

## <a name="conclusion"></a>Závěr
Funkce inženýrství a vybrané zvýšení efektivity hello hello školení procesu, který se pokusí tooextract hello klíčové informace obsažené v datech hello. Také zvyšují hello power vstupních dat tyto modely tooclassify hello přesně a výstupy toopredict zajímají více robustní. Funkce inženýrství a výběr můžete také kombinovat toomake hello learning výpočetně tractable. Dělá to pomocí rozšíření a potom snižuje hello řadu funkcí toocalibrate nebo train model potřeba. Matematický platí, že jsou hello funkce vybrané tootrain hello modelu minimálním počtem nezávislých proměnných, které popisují hello vzorů v datech hello a pak úspěšně předpovědi výstupy.

Všimněte si, že není vždy nutně tooperform výběr inženýrství nebo funkce funkce. Jestli je potřeba, nebo Ne, závisí na hello dat jsme nebo shromažďovat, hello algoritmus, který jsme vyberte a hello cíl hello experimentu.

