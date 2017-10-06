---
title: "Výběr aaaFeature v hello proces vědecké účely dat Team | Microsoft Docs"
description: "Popisuje účel hello výběr funkce a obsahuje příklady jejich role v hello data vylepšení proces strojové učení."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 878541f5-1df8-4368-889a-ced6852aba47
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: zhangya;bradsev
ms.openlocfilehash: 54af93c83e4cc6a3670b3ad62490e0f74082b4ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="feature-selection-in-hello-team-data-science-process-tdsp"></a>Výběr funkce v hello tým datové vědy procesu (TDSP)
Tento článek vysvětluje hello účely výběr funkce a obsahuje příklady jeho role v hello data vylepšení proces strojové učení. Tyto příklady jsou vykreslovány z Azure Machine Learning Studio. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Dobrý den, technici a výběr funkcí je jednou ze součástí sady hello tým datové vědy procesu (TDSP) uvedených v [co je hello proces vědecké účely dat Team?](data-science-process-overview.md). Funkce inženýrství a výběr jsou části hello **vyvíjet funkce** krok hello TDSP.

* **konstruování**: Tento proces pokusí toocreate z hello stávajících funkcí nezpracovaná hello data a algoritmus učení toohello prediktivní power tooincrease další příslušné funkce.
* **funkce Výběr**: Tento proces vybere hello klíče podmnožinu funkcí, původní data v pokusu o tooreduce hello dimenzionalitu hello školení problému.

Za normálních okolností **konstruování** je použité první toogenerate další funkce a pak hello **funkci Výběr** krok se provádí tooeliminate důležité, redundantní nebo vysoce korelační funkce.

## <a name="filtering-features-from-your-data---feature-selection"></a>Funkce filtrování z vašich dat – Výběr funkce
Výběr funkce je proces, který se běžně použije pro tvorbu hello datových sad školení pro prediktivní modelování úlohy, například klasifikační nebo regresní úlohy. cílem Hello je tooselect podmnožinu funkcí hello z hello původní datové sady, která snižují jeho dimenze pomocí minimální sadu funkcí toorepresent hello maximální množství odchylky v datech hello. Tuto podmnožinu funkcí, jsou pak hello pouze funkce toobe zahrnuté tootrain hello modelu. Výběr funkce má dva hlavní účely.

* Nejprve výběr funkce často zvyšuje přesnost klasifikace odstraněním důležité, redundantní nebo vysoce korelují funkce.
* Druhý snižuje číslo hello funkcí, které umožňuje zefektivnit proces školení modelu. To je obzvláště důležité pro inteligentních algoritmů, které jsou nákladné tootrain například support vector počítače.

I když výběr funkce Hledat tooreduce hello řadu funkcí v hello datová sada používá tootrain hello modelu, není obvykle označují tooby hello termín "dimenzionalitu snížení". Metody výběru funkce extrahujte podmnožinu původní funkcí v datech hello bez jejich změny.  Metody snížení dimenzionalitu využívají inženýrství funkce, které můžete transformace hello původní funkcí a proto je upravit. Příklady metod snížení dimenzionalitu: hlavní součást analýzy, kanonickém korelace analýzu a singulární rozložením. hodnota.

Mimo jiné jednu kategorii široce použité metody výběru funkcí v kontextu pod dohledem se nazývá "výběr filtru na základě funkce". Vyhodnocením hello korelace mezi každý atribut target funkce a hello použít tyto metody statistické měr tooassign funkce tooeach skóre. Funkce Hello, pak jsou seřazeny podle hello skóre, což může být použité toohelp nastavenou hello prahovou hodnotu pro zachování nebo odstranění konkrétní funkci. Příklady hello statistické míry použít v těchto metod: korelace osoba, vzájemné informace a kvadratických test chí hello.

V nástroji Azure Machine Learning Studio jsou modulů, které výběr funkce. Jak ukazuje následující obrázek hello, patří tyto moduly [na základě filtru výběr funkce] [ filter-based-feature-selection] a [Fisherovy lineární analýza Discriminant] [ fisher-linear-discriminant-analysis].

![Příklad výběru funkce](./media/machine-learning-data-science-select-features/feature-Selection.png)

Zvažte například hello použití hello [na základě filtru výběr funkce] [ filter-based-feature-selection] modulu. Hello za účelem usnadnění práce abychom mohli pokračovat toouse hello text dolování příklad uvedených výše. Předpokládejme, že má být toobuild regresní model po sadu funkcí, 256 jsou vytvořena prostřednictvím hello [Hashování] [ feature-hashing] modulu a tuto proměnnou odpovědi hello je hello "Sloupec1" a představuje knihy Zkontrolujte hodnocení rozsahu od 1 too5. Nastavením "Funkce vyhodnocování metoda" toobe "Pearson korelace" hello "Cílový sloupec" toobe "Sloupec1" a "Počet požadované funkce" too50 hello. Pak hello modul [na základě filtru výběr funkce] [ filter-based-feature-selection] vytvoří datovou sadu obsahující 50 funkcí společně s atribut target hello "Sloupec1". Hello následující obrázek ukazuje hello toku tohoto experimentu a hello vstupní parametry, které jsme právě popsané.

![Příklad výběru funkce](./media/machine-learning-data-science-select-features/feature-Selection1.png)

Hello následující obrázek znázorňuje hello výsledné datové sady. Jednotlivé funkce skóre pro magnitudu na základě na hello Pearson korelace mezi sám a hello atribut target "Sloupec1". Funkce Hello s nejvyšší skóre jsou zachovány.

![Příklad výběru funkce](./media/machine-learning-data-science-select-features/feature-Selection2.png)

Hello odpovídající skóre hello vybrané funkce se zobrazují v hello následující obrázek.

![Příklad výběru funkce](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Použitím to [na základě filtru výběr funkce] [ filter-based-feature-selection] modulu, 50 mimo 256 funkce jsou vybrané, protože budou mít hello nejvíce korelační funkce s hello Cílová proměnná "Sloupec1" podle vyhodnocování hello Metoda "Pearson korelace".

## <a name="conclusion"></a>Závěr
Funkce inženýrství a výběr funkce jsou dvě běžně analyzovány a zvýšit efektivitu hello hello cvičení proces, který se pokusí tooextract hello klíčové informace obsažené v datech hello vybrané funkce. Také zvyšují hello power vstupních dat tyto modely tooclassify hello přesně a výstupy toopredict zajímají více robustní. Funkce inženýrství a výběr můžete také kombinovat toomake hello learning výpočetně tractable. Dělá to pomocí rozšíření a potom snižuje hello řadu funkcí toocalibrate nebo train model potřeba. Matematický platí, že jsou hello funkce vybrané tootrain hello modelu minimálním počtem nezávislých proměnných, které popisují hello vzorů v datech hello a pak úspěšně předpovědi výstupy.

Všimněte si, že není vždy nutně tooperform výběr inženýrství nebo funkce funkce. Jestli je potřeba, nebo Ne, závisí na hello dat jsme nebo shromažďovat, hello algoritmus, který jsme vyberte a hello cíl hello experimentu.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

