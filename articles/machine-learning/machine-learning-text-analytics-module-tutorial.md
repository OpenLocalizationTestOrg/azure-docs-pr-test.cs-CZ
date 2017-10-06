---
title: modely analytics aaaCreate text v Azure Machine Learning Studio | Microsoft Docs
description: "Jak analýza textu toocreate modelů v Azure Machine Learning Studio pomocí moduly pro předzpracování text, N-gram nebo hashování"
services: machine-learning
documentationcenter: 
author: rastala
manager: jhubbard
editor: 
ms.assetid: 08cd6723-3ae6-4e99-a924-e650942e461b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: roastala
ms.openlocfilehash: e3799f37ba54bb2ec8815ecf5ed34e145ffb20e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Vytvoření modelů analýzy textu v nástroji Azure Machine Learning Studio
Můžete použít Azure Machine Learning toobuild a zprovoznit modely analytics text. Tyto modely vám může pomoct vyřešit, například problémy analýzy dokumentu klasifikace nebo postojích.

V analýzy text experimentu byste obvykle:

1. Vyčistěte a předběžně zpracovat datovou sadu textu
2. Extrahování vektory číselné funkce z předem zpracovaných textu
3. Train klasifikační nebo regresní model
4. Stanovení skóre a ověření modelu hello
5. Nasazení modelu tooproduction hello

V tomto kurzu zjistíte podle jsme provede model analysis postojích pomocí Amazon kniha recenze datovou sadu, tyto kroky (najdete v tomto dokumentu research "biografie, Bollywood, výložníku polí a Blenders: přizpůsobení a domény pro klasifikaci postojích" Jan Blitzer, označit Dredze a Fernando Pereira; Přidružení výpočetní Linguistics (ACL), 2007.) Tato datová sada se skládá z Zkontrolujte skóre (1 – 2 nebo 4 až 5) a vlastní text. Hello cílem je toopredict hello Zkontrolujte skóre: nízkou (1 - 2) nebo vysokou (4 až 5).

Můžete najít experimenty, které jsou popsané v tomto kurzu na webu Cortana Intelligence Gallery:

[Předpověď recenze adresáře](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Předpověď kniha recenze - prediktivní Experiment](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Krok 1: Vyčištění a předběžně zpracovat datovou sadu textu
Začneme hello experimentu vydělením hello Zkontrolujte skóre do kategorií nízkou a vysokou kbelíků tooformulate hello problém jako dvě třídy klasifikace. Používáme [upravit Metadata](https://msdn.microsoft.com/library/azure/dn905986.aspx) a [skupiny kategorií hodnoty](https://msdn.microsoft.com/library/azure/dn906014.aspx) moduly.

![Vytvoření popisku](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Potom jsme vyčistit pomocí textu hello [předběžně zpracovat Text](https://msdn.microsoft.com/library/azure/mt762915.aspx) modulu. čištění Hello snižuje hello šumu v datové sadě hello, pomáhají najít hello nejdůležitější funkce a zlepšení hello přesnost hello konečné modelu. Jsme odebrat stopslova - běžná slova, jako je například "na" nebo "a" - a čísla, speciální znaky, znaky duplicitní, e-mailové adresy a adresy URL. Jsme také převést hello text toolowercase, lemmatize hello slova a zjišťovat hranice věty, které jsou pak indikován "|||" symbol v předem zpracovaných text.

![Předběžné zpracování textu](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Co dělat, když chcete toouse vlastní seznam stopslova? Můžete ho v předat jako volitelné vstup. Můžete také použít vlastní C# syntaxi regulárního výrazu tooreplace dílčích řetězců a odeberte slova slovní: podstatná jména, operace nebo přídavných jmen.

Po dokončení předzpracování hello jsme rozdělit hello data do vlaku a testování sad.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Krok 2: Extrahování vektory číselné funkce z předem zpracovaných textu
toobuild model textových dat, obvykle mají tooconvert textové do vektory číselné funkce. V tomto příkladu používáme [funkce N-Gram extrahovat z textu](https://msdn.microsoft.com/library/azure/mt762916.aspx) toosuch datový formát pro modul tootransform hello textu. Tento modul trvá sloupec oddělené prázdný znak slova a vypočítá slovník výrazů nebo N-gram slov, které se zobrazují v datovou sadu. Potom udává, kolik časy každé aplikace word nebo N-gram, se zobrazí všechny záznamy a vytvoří funkci vektory od těch, počty. V tomto kurzu jsme nastavené N-gram velikost too2, takže naše vektory funkce zahrnují jednoho slova a kombinací dvou dalších slov.

![Extrahování N-gram](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

Použijeme TF * počty IDF (termín frekvence inverzní dokumentu frekvenci) vážení tooN-gram. Tento postup přidá váhu slova, která zobrazí často v jeden záznam, ale vyskytují jen vzácně napříč celou datovou sadu hello. Další možnosti zahrnují binární TF a graf vážení.

Funkce text mají často vysokou dimenzionalitu. Například pokud vaše svátek má 100 000 jedinečných slov, váš prostor funkce by mít dimenze 100 000 nebo více pokud používají N-gram. modul extrahovat funkce N-Gram Hello poskytuje sadu možností tooreduce hello dimenzionalitu. Můžete zvolit tooexclude slova, která krátký nebo dlouhý, nebo příliš neobvyklé nebo příliš časté toohave významné předpovězené hodnoty. V tomto kurzu jsme vyloučit N-gram, které se zobrazují v méně než 5 záznamy nebo ve víc než 80 % záznamy.

Navíc můžete použít funkci Výběr tooselect jenom ty funkce, které jsou nejvíce hello korelační s cílových předpovědi. Používáme chí-kvadrát funkce Výběr tooselect 1000 funkce. Slovník hello vybrané slova nebo N-gram můžete zobrazit kliknutím pravého výstup hello modulu extrakce N-gram.

Jako toousing alternativní způsob extrahovat N-Gram funkce můžete použít modul funkce algoritmu hash. Všimněte si ale, že [Hashování](https://msdn.microsoft.com/library/azure/dn906018.aspx) nemá sestavení ve funkci Výběr možnosti nebo TF * IDF vážení.

## <a name="step-3-train-classification-or-regression-model"></a>Krok 3: Train klasifikační nebo regresní model
Nyní textu hello byl transformovaných toonumeric funkce sloupců. Hello datovou sadu stále obsahuje řetězec sloupců z předchozí fázích, proto používáme výběr sloupců v datové sadě tooexclude je.

Potom pomocí [Logistic Regression Two-Class](https://msdn.microsoft.com/library/azure/dn905994.aspx) toopredict naše cíl: vysoká nebo Nízká Zkontrolujte skóre. V tomto okamžiku problém analytics textu hello transformaci do regulární klasifikace problému. Můžete použít hello nástrojů dostupných v Azure Machine Learning tooimprove hello modelu. Například můžete experimentovat s jinou třídění toofind na tom, jak přesné výsledky přidělit, nebo použijte hyperparameter ladění tooimprove hello přesnost.

![Train a skóre](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-hello-model"></a>Krok 4: Stanovení skóre a ověření modelu hello
Jak by hello trained model ověřit? Jsme skóre proti hello testovací datové sady a vyhodnocení přesnost hello. Hello model však zjistili hello termínů N-gram a jejich váhu z datové sady hello školení. Proto jsme měli používat tuto termínů a tyto váhu při extrahování funkce z testovací data, jako byl proti toocreating hello termínů znovu. Proto jsme přidejte toohello modulu extrahovat N-Gram funkce vyhodnocování větve hello experiment, připojit termínů výstup hello z větve školení a nastavte hello termínů režimu pouze tooread. Můžeme také zakažte hello filtrování N-gram frekvenci nastavení instance hello too1 minimální a maximální too100 % a vypnout hello výběr funkce.

Po hello textového sloupce v testu data byla úspěšně transformaci toonumeric funkce sloupce, jsme vyloučit hello řetězec, který sloupců z předchozích fází, jako v školení větev. Potom používáme předpovědi toomake modul určení skóre modelu a přesnost hello tooevaluate modulu Evaluate Model.

## <a name="step-5-deploy-hello-model-tooproduction"></a>Krok 5: Nasazení tooproduction modelu hello
Hello model je téměř Hotovo toobe nasazené tooproduction. Při nasazení jako webové služby, přebírá textové řetězce jako vstup a vrátí předpovědi "vysoká" nebo "nízká". Používá se naučili hello N-gram termínů tootransform hello text toofeatures a cvičení modelu toomake logistic regression předpovědi z těchto funkcí. 

tooset až hello prediktivní experiment, jsme nejprve uložte hello N-gram termínů jako datové sady a hello natrénovali model logistic regression z hello školení větev hello experimentu. Potom jsme uložit hello experimentu pomocí "Uložit jako" toocreate experimentu graf pro prediktivní experiment. Jsme odebrat hello rozdělení dat modul a hello školení větev z hello experimentu. Nemůžeme se potom připojují hello uložili dřív N-gram termínů a modelu tooExtract N-Gram funkce a Score Model modulů, v uvedeném pořadí. Můžeme také odebrat modulu Evaluate Model hello.

Nemůžeme vložit výběr sloupců v datové sadě modulu před předběžně zpracovat Text modulu tooremove hello popisek sloupce a zrušte výběr možnost "Připojení skóre sloupec toodataset" v modulu skóre. Tímto způsobem hello webová služba nevyžaduje popisek hello se pokouší toopredict a není echo hello vstupní funkce v odpovědi.

![Prediktivní Experiment](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Máme teď experimentu, který umožňuje publikovat jako webovou službu a volat pomocí požadavků a odpovědí nebo dávkového spuštění rozhraní API.

## <a name="next-steps"></a>Další kroky
Další informace o modulech analytics text z [dokumentace MSDN](https://msdn.microsoft.com/library/azure/dn905886.aspx).

