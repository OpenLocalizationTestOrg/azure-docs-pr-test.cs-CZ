---
title: "aaaManage experimentovat iterací v nástroji Machine Learning Studio | Microsoft Docs"
description: "Jak toomanage experimentovat iterací v nástroji Azure Machine Learning Studio"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a53530f-20d5-40ae-9b49-7b499ccb44b7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: bd30c048ce063811b1b2de8ce6d71e99ba975713
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio"></a>Správa iterací experimentů v nástroji Azure Machine Learning Studio
Vývoj model prediktivní analýzy je iterativní proces - úpravou hello různé funkce a parametry experimentu výsledky zpřesňují, dokud nebudete přesvědčeni, že máte natrénován efektivní model. Klíč toothis proces je sledování hello různých iterací experimentu parametry a konfigurace.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Můžete zkontrolovat předchozích spuštění z experimentů v pořadí toochallenge kdykoli, pokroku a nakonec potvrďte nebo Upřesnit předchozí předpoklady. Při spuštění experimentu, Machine Learning Studio uchovává historii hello spustit, včetně datovou sadu, modul a připojení k portu a parametry. Tato historie taky zaznamená výsledky, informace o běhu programu, například spuštění a zastavení časy, zprávy protokolu a stav spuštění. Můžete si prohlédnout zpět některé z těchto běží na všech čas tooreview hello časovou posloupnost experiment a mezilehlých výsledků. Předchozím spuštění vaší experimentu toolaunch do nové fázi dotaz a zjišťování můžete použít i na vaše cesta toocreating jednoduchý, komplexní nebo dokonce komplet modelování řešení.

> [!NOTE]
> Při zobrazení předchozího spuštění experimentu, že tato verze hello experimentu uzamčeno a nelze jej upravit. Můžete však uložení kopie ho kliknutím na **uložit jako** a poskytuje nový název pro kopírování hello. Machine Learning Studio otevře nové kopie hello, která pak můžete upravit a spustit. Této kopii experimentu je k dispozici v hello **EXPERIMENTY** seznamu společně s všechny ostatní experimentů.
> 
> 

## <a name="viewing-hello-prior-run"></a>Zobrazení hello předchozí spustit
Až budete mít otevřenou experimentu, který jste spustili alespoň jednou, můžete zobrazit hello předcházející spuštění hello experiment kliknutím **předchozí spustit** v podokně Vlastnosti hello.

Předpokládejme například, můžete vytvořit nový experiment a spustit verze v 11:23 11:42 a 11:55. Pokud otevřete hello posledním spuštění experimentu hello (11:55) a klikněte na tlačítko **předchozí spustit**, jste spustili na 11:42 verze hello je otevřen.

## <a name="viewing-hello-run-history"></a>Zobrazení hello spustit historie
Kliknutím můžete zobrazit všechny hello předchozích spuštění experimentu **zobrazit historii běhů** v experimentu otevřete.

Předpokládejme například, že vytvoříte experiment s hello [lineární regrese] [ linear-regression] modulu a chcete, aby tooobserve hello efekt změny hello hodnotu **rychlost učení** na výsledky experimentu. Můžete spustit hello experiment vícekrát s různými hodnotami pro tento parametr, následujícím způsobem:

| Hodnota míry učení | Počáteční čas spuštění |
| --- | --- |
| 0.1 |11/9/2014 4:18:58 pm |
| 0.2 |11/9/2014 4:24:33 pm |
| 0.4 |11/9/2014 4:28:36 pm |
| 0.5 |11/9/2014 4:33:31 pm |

Pokud kliknete na tlačítko **zobrazit HISTORII BĚHŮ**, zobrazí se seznam všech těchto spuštění:

![Příklad historie spouštění][runhistory]

Kliknutím na jakýkoli z těchto spustí tooview snímek hello experimentovat ve hello dobu, kdy jste ho spustili. Hello konfigurace, hodnoty parametrů, komentáře a výsledky jsou všechny zachovaných toogive je úplný záznam této spuštění experimentu.

> [!TIP]
> toodocument vaše iterací experimentu hello, můžete upravit název hello pokaždé, když spustíte ji, můžete aktualizovat hello **Souhrn** hello experiment v podokně Vlastnosti hello a můžete přidat nebo aktualizovat komentáře na jednotlivé moduly toorecord změny. název, souhrn a modul komentáře Hello se ukládají s každé spuštění experimentu hello.
> 
> 

Hello seznam experimenty v hello **EXPERIMENTY** karta v nástroji Machine Learning Studio vždy zobrazuje hello nejnovější verzi experimentu. Pokud otevřete předchozím spuštění experimentu hello (pomocí **předchozí spustit** nebo **zobrazit HISTORII BĚHŮ**), můžete se vrátit verzi konceptu toohello kliknutím na **zobrazit HISTORII BĚHŮ** a výběrem Hello iterace, který má **stavu** z **upravit**.

## <a name="iterating-on-a-previous-run"></a>Iterace v předchozího spuštění
Když kliknete na tlačítko **předchozí spustit** nebo **zobrazit HISTORII BĚHŮ** a otevřete předchozího spuštění, dokončení experimentu můžete zobrazit v režimu jen pro čtení.

Pokud chcete toobegin iterace experimentu počínaje hello způsob, jak jste nakonfigurovali pro předchozí spustit, musíte spustit hello otevírání a kliknutím na **uložit jako**. Tím se vytvoří nový experiment, s použitím nový název, prázdný historie spouštění a všechny součásti hello a hodnoty parametru hello předchozí spustit. Tento nový experiment, je uvedena ve hello **EXPERIMENTY** ve hello Machine Learning Studio domovské stránky a vy můžete upravit a spustit, inicializaci novou spusťte historie pro tento iteraci experimentu. 

Předpokládejme například, že máte hello experiment spustit historie uvedené v předchozí části hello. Chcete tooobserve, co se stane, když nastavíte hello **rychlost učení** too0.4 parametr a opakujte různé hodnoty pro hello **počet školení epoch** parametr.

1. Klikněte na tlačítko **zobrazit HISTORII BĚHŮ** a otevřete hello iteraci experimentu hello, který jste spustili ve 4:28:36 (ve kterém nastavíte too0.4 hodnota parametru hello).
2. Klikněte na tlačítko **uložit jako**.
3. Zadejte nový název a klikněte na hello **OK** zaškrtnutí. Se vytvoří novou kopii hello experimentu.
4. Upravit hello **počet školení epoch** parametr.
5. Klikněte na tlačítko **spustit**.

Teď můžete pokračovat toomodify a spusťte tuto verzi experimentu, vytváření nových toorecord historie spouštění práci.

<!-- Images -->
[runhistory]:./media/machine-learning-manage-experiment-iterations/viewrunhistory.jpg


<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
