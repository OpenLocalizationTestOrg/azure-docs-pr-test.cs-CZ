---
title: "aaaClean a připravit data pro Azure Machine Learning | Microsoft Docs"
description: "Předběžné zpracování a vyčištění dat tooprepare ho pro machine learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: bdf659ec-4881-4324-8b9c-747cbfa0c3cd
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 3e3c3e4b0cfb9187f5820d7165e6ee1ea013ba02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tasks-tooprepare-data-for-enhanced-machine-learning"></a>Tooprepare data úlohy pro rozšířené machine learning
Předběžné zpracování a vyčištění dat jsou důležité úkoly, které obvykle musí být provedeny před datové sady je možné efektivně pro machine learning. Nezpracovaná data, je často aktivní nebo nespolehlivé a může být chybějící hodnoty. Pomocí těchto údajů pro modelování může vytvářet zavádějící výsledky. Tyto úlohy jsou součástí hello tým datové vědy procesu (TDSP) a obvykle postupujte podle počáteční zkoumání toodiscover použít datovou sadu a plán hello předzpracováním vyžaduje. Podrobné pokyny k procesu TDSP hello, najdete v části hello kroků uvedených v hello [proces vědecké účely dat Team](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Předběžné zpracování a úlohy čištění, jako úloha zkoumání hello data, lze provádět v celé řadě prostředí, jako je například SQL nebo Hive nebo Azure Machine Learning Studio a pomocí různých nástrojů a jazyky, jako je R nebo Python, v závislosti, kde je vaše data uložená a jejich formátování. Vzhledem k tomu, že se předpokládá několikeré ve své podstatě TDSP, tyto úlohy můžete provádět v jednotlivých kroků v pracovním postupu hello hello procesu.

Tento článek představuje různé zpracování dat koncepty a úlohy, které lze provádět před nebo po příjem dat do Azure Machine Learning.

Příklad zkoumání dat a předběžné zpracování provést uvnitř Azure Machine Learning studio, najdete v části hello [předběžné zpracování dat v Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) videa.

## <a name="why-pre-process-and-clean-data"></a>Proč předběžně zpracovat a vyčistit data?
Shromáždění skutečných dat z různých zdrojů a procesy a může obsahovat nesrovnalostí nebo poškozená data ohrožení hello kvalitu hello datovou sadu. Hello typické data quality problémy, které vznikají jsou:

* **Nedokončené**: dat chybí atributy nebo obsahující chybějící hodnoty.
* **Aktivní**: Data obsahují chybné záznamy nebo odlehlé hodnoty.
* **Nekonzistentní**: Data obsahují konfliktní záznamy nebo nesrovnalostí.

Kvality dat je předpokladem pro prediktivní modely kvality. tooavoid "paměti v paměti se" a zlepšení kvality dat a proto modelu výkonu, je nutné tooconduct toospot obrazovky data stavu, dat již v rané fázi problémy a rozhodněte o hello odpovídající zpracování dat a čištění kroky.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Jaké jsou některé obrazovky stavu typické dat, které budou použity?
Kontrolou jsme můžete zkontrolovat obecné kvality hello dat:

* Hello počet **záznamy**.
* Hello počet **atributy** (nebo **funkce**).
* atribut Hello **datové typy** (nominální, pořadí nebo souvislé).
* Hello počet **chybějící hodnoty**.
* **Správnosti** dat hello.
  * Pokud hello data jsou v TSV nebo sdílený svazek clusteru, zkontrolujte, že oddělovačů hello sloupce a řádku oddělovačů vždy správně oddělit sloupce a řádky.
  * Je-li hello data ve formátu HTML nebo XML, zkontrolujte, zda hello data je ve správném formátu závislosti na jejich příslušné standardy.
  * Analýza může být také nutné v pořadí tooextract strukturovaná informace z částečně strukturovaných nebo nestrukturovaných dat.
* **Zaznamenává nekonzistentní data**. Zkontrolujte, jsou povolené hello rozsah hodnot. Například pokud hello dat obsahuje student GPA, zkontrolujte, zda text hello GPA je v hello určený rozsah vyslovení 0 ~ 4.

Pokud narazíte na problémy s daty, **kroky zpracování** jsou nezbytné, což často zahrnuje vyčištění chybějících hodnot, data normalizaci, diskretizační, tooremove zpracování textu nebo nahrazení embedded znaků, což může mít vliv na společné zarovnání dat různé datové typy, pole a dalších.

**Azure Machine Learning spotřebuje ve správném formátu tabulková data**.  Pokud hello dat již ve formě tabulky, předběžné zpracování dat lze provést přímo pomocí Azure Machine Learning v hello Machine Learning Studio.  Pokud data nejsou ve formě tabulky, může být požadováno indikované, který je v analýze XML v pořadí tooconvert hello data tootabular formuláře.  

## <a name="what-are-some-of-hello-major-tasks-in-data-pre-processing"></a>Jaké jsou některé z hlavních úloh hello v předběžné zpracování dat?
* **Čištění dat.**: vyplňte nebo chybějící hodnoty detekovat a odstraňovat aktivní data a extrémních.
* **Transformace dat**: normalizovat dimenzí tooreduce dat a šumu.
* **Data snížení**: ukázková data záznamy nebo atributy pro snazší manipulaci s daty.
* **Data diskretizační**: převést souvislé atributy toocategorical atributy pro snadné použití pomocí metod určité machine learning.
* **Text čištění**: odebrat vložené znaky, které může způsobit, že chybné zarovnání dat, například embedded karty v souboru tabulátorem data vložených nové řádky, které může rozdělit záznamy atd.

v níže uvedených částech Hello podrobnosti některé z těchto kroků zpracování dat.

## <a name="how-toodeal-with-missing-values"></a>Jak toodeal s chybějící hodnoty?
toodeal s chybějící hodnoty, je vhodné toofirst identifikovat hello důvod pro hello chybějící hodnoty toobetter popisovač hello problém. Typické chybí hodnota zpracování metody jsou následující:

* **Odstranění**: odebrat záznamy s chybějící hodnoty
* **Fiktivní nahrazení**: chybějící hodnoty nahraďte fiktivní hodnoty: např, *neznámé* kategorií nebo 0 pro číselné hodnoty.
* **Znamenat nahrazení**: Pokud chybějící data hello číselné, nahraďte hello chybějící hodnoty střední hello.
* **Časté nahrazení**: Pokud chybějící data hello kategorií, nahraďte hello chybějící hodnoty nejčastěji se vyskytující položku hello
* **Nahrazení regrese**: použití regrese metoda tooreplace chybějící hodnoty který poklesl hodnotami.  

## <a name="how-toonormalize-data"></a>Jak toonormalize dat?
Data normalizaci znovu škáluje číselné hodnoty tooa zadaný rozsah. Metody normalizaci oblíbených dat patří:

* **Min-Max normalizaci**: Lineárně transformace hello data tooa rozsahu, říkají, mezi 0 a 1, kde hello minimální hodnota je škálovat too0 a too1 maximální hodnotu.
* **Z – score normalizaci**: škálování dat na základě střední a směrodatnou odchylku: dělit hello rozdíl mezi hello dat a střední hello hello směrodatnou odchylku.
* **Decimal škálování**: škálovat hello dat, protože přesunutí desetinnou hello hodnota atributu hello.  

## <a name="how-toodiscretize-data"></a>Jak toodiscretize dat?
Data můžete diskrétní převedením průběžné hodnoty toonominal atributy nebo intervaly. Některé z mnoha možností to jsou:

* **Přihrádkování rovná-Width**: rozdělení do skupiny N hello stejná velikost a přiřaďte hello hodnoty, které spadají do přihrádky s číslem bin hello hello rozsah všech možných hodnot atributu.
* **Výška rovná Přihrádkování**: rozdělení hello rozsah všech možných hodnot atributu do skupiny, každý obsahující hello stejný počet instancí a potom přiřadit hello hodnoty, které spadají do přihrádky s hello bin číslo.  

## <a name="how-tooreduce-data"></a>Jak tooreduce dat?
Existují různé metody tooreduce data velikost pro snazší data zpracování. V závislosti na velikosti a hello domény data můžete použít následující metody hello:

* **Zaznamenejte vzorkování**: ukázkové hello záznamů dat a vyberte pouze reprezentativní podmnožinu hello z dat hello.
* **Atribut vzorkování**: Vyberte jenom podmnožinu hello nejdůležitější atributy z dat hello.  
* **Agregace**: rozdělení hello dat do skupin a uložit čísla hello pro každou skupinu. Například hello denní výnosy, který může být čísla řetězu restaurace přes hello posledních letech 20 agregován toomonthly výnosy tooreduce hello velikost dat hello.  

## <a name="how-tooclean-text-data"></a>Jak tooclean textová data?
**Textová pole v tabulkovém data** může obsahovat znaky, které ovlivňují sloupce zarovnání nebo záznam hranice. Například vložených karty v synchronizace souboru tabulátorem příčina sloupce a vložených znaky nového řádku rozdělit záznamů řádků. Nesprávné kódování zpracování při zápisu nebo čtení textu textu vede tooinformation dojít ke ztrátě, nechtěnému Úvod nečitelné znaky, například hodnoty Null, a může také vliv text analýzu. V pořadí tooclean textových polí pro správné zarovnání nebo tooextract strukturovaná data z textu částečně strukturovaných nebo nestrukturovaných dat může být nutné pečlivé analýzy a úpravy.

**Zkoumání dat** nabízí zobrazení o předčasné do hello data. Počet problémy dat může neodkrytých během tohoto kroku a odpovídající metody mohou být použité tooaddress těchto problémů.  Je důležité tooask otázky, například hello zdroj problému hello a jak hello problém může mít zavedený. To zároveň pomáhá rozhodněte o hello zpracování dat kroky tohoto toobe nutné provést tooresolve je. Hello druh statistiky, že jeden zaměřen tooderive z hello dat může být také úsilí zpracování dat použité tooprioritize hello.

## <a name="references"></a>Odkazy
> *Dolování dat: Konceptech a technikách*, Third Edition, Nováková Kaufmann, 2011, Hanu Jiawei, Micheline Kamber a Jian Pei
> 
> 

