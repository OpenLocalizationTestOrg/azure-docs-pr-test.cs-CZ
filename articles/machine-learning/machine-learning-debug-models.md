---
title: aaaDebug modelu v Azure Machine Learning | Microsoft Docs
description: "Jak toodebug chyby vytvořené Train Model a Score Model moduly v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 629dc45e-ac1e-4b7d-b120-08813dc448be
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bradsev;garye
ms.openlocfilehash: ee38ca8ce38d4fc7add5ba70c80ab9bb2ceaf1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-model-in-azure-machine-learning"></a>Ladění modelu ve službě Azure Machine Learning

Tento článek vysvětluje hello potenciální z důvodů, proč některá z následujících selhání dvou hello může být došlo při spuštění modelu:

* Hello [Train Model] [ train-model] modulu vytvoří chybu 
* Hello [Score Model] [ score-model] modulu produkuje nesprávné výsledky 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a>Modulu Train Model vytvoří chybu

![image1](./media/machine-learning-debug-models/train_model-1.png)

Hello [Train Model] [ train-model] modulu očekává dva vstupy:

1. Typ Hello model strojového učení z kolekce hello modelů, které poskytuje Azure Machine Learning.
2. Hello Cvičná data se sloupcem zadaného popisku, který určuje hello proměnné toopredict (hello ostatních sloupců se předpokládá, že funkce toobe).

Tento modul může vést k chybě v následujících případech hello:

1. Popisek sloupce Hello je nesprávně zadán. To může nastat, když je více než jeden sloupec vybraný jako hello popisek nebo je vybrán nesprávný sloupec indexu. Hello druhém případě byste například použít, pokud index sloupce 30 používá s vstupní datové sady, která má pouze 25 sloupce.

2. Hello datová sada neobsahuje žádné sloupce funkce. Například pokud vstupní datové sady hello má jen jeden sloupec, který je označen jako hello popisek sloupce, by žádné funkce, které toobuild hello modelu. V takovém případě hello [Train Model] [ train-model] modulu vytvoří chybu.

3. Hello vstupní datové sady (funkce nebo popis) obsahuje Infinity jako hodnotu.

## <a name="score-model-module-produces-incorrect-results"></a>Modul určení skóre modelu produkuje nesprávné výsledky

![image2](./media/machine-learning-debug-models/train_test-2.png)

V vytvoří typické školení nebo testování experiment pro učení pod dohledem, hello [rozdělení dat] [ split] modulu rozděluje hello původní datové sady do dvou částí: jednou ze součástí je použité tootrain hello modelu a používá se jednou ze součástí tooscore, jak dobře provede hello trained model. Hello trained model je pak použít tooscore hello testovací data, po který jsou výsledky hello vyhodnotí toodetermine hello přesnost hello modelu.

Hello [Score Model] [ score-model] modulu vyžaduje dva vstupy:

1. Výstup trained model z hello [Train Model] [ train-model] modulu.
2. Vyhodnocování datovou sadu, která se liší od datovou sadu hello používá tootrain hello modelu.

To je možné, že i když hello experimentu úspěšné, hello [Score Model] [ score-model] modulu produkuje nesprávné výsledky. Několik scénářů může způsobit, že tento toohappen:

1. Pokud hello Zadaný popisek je kategorií a regresní model je trénink na hello data, nesprávné výstup podle hello [Score Model] [ score-model] modulu. To je proto regrese vyžaduje proměnnou průběžné odpovědi. V takovém případě by bylo vhodnější toouse model klasifikace. 

2. Podobně pokud je model klasifikace trénink na datovou sadu s plovoucí desetinnou čárkou v hello popisek sloupce, může způsobit nežádoucí výsledky. To je proto klasifikace vyžaduje diskrétní odpovědi proměnné, která umožňuje pouze hodnoty rozsahu v rámci omezené a obvykle poněkud malé sady tříd.

3. Pokud hello vyhodnocování datová sada neobsahuje všechny hello funkce, které používá tootrain hello modelu, hello [Score Model] [ score-model] vytvoří chybu.

4. Pokud řádek v hello vyhodnocování datová sada obsahuje hodnotu chybí, nebo hodnotu nekonečné pro některý z jeho funkce, hello [Score Model] [ score-model] nebudou vytvořeny žádné odpovídající toothat řádek výstupu.

5. Hello [Score Model] [ score-model] může vytvořit identické výstupy pro všechny řádky v hello vyhodnocování datovou sadu. Tato situace může nastat, například při pokusu o zařazení pomocí rozhodnutí doménové struktury, pokud hello minimální počet vzorků na uzel typu list, které jste vybrali toobe více než hello počet školení příklady, které jsou k dispozici.

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

