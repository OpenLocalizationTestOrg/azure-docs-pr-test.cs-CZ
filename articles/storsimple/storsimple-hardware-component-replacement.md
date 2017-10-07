---
title: "výměna hardwaru součástí aaaStorSimple | Microsoft Docs"
description: "Popisuje, jak nahradit toosafely hello PCMs, baterie, moduly řadiče, řadiče EBOD, diskové jednotky a skříně zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e8087ba7-0b66-4f59-8988-e53aad52ee21
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 472d9dc1c31b61550fe079cc9b9419510487db3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-hardware-component-on-your-storsimple-8000-series-device"></a>Nahraďte hardwarová komponenta na vašem zařízení řady StorSimple 8000

## <a name="overview"></a>Přehled
Hello hardwarové součásti nahrazení kurzy popisují hello hardwaru součástí vaší společnosti Microsoft Azure StorSimple 8000 řady zařízení a hello kroky nezbytné tooremove a nahradíte je. Tento článek popisuje hello zabezpečení ikony, poskytuje ukazatele toohello podrobné kurzy a seznamy hello součásti, které jsou nahraditelné.

> [!IMPORTANT]
> Před pokusem o tooremove nebo nahradit všechny součásti, StorSimple, ujistěte se, abyste si prošli hello [zabezpečení ikonu konvence](#safety-icon-conventions) a dalších [bezpečnostní opatření](storsimple-safety.md).
> 
> 

### <a name="safety-icon-conventions"></a>Konvence ikona zabezpečení
Hello následující tabulka popisuje hello zabezpečení ikony používané v těchto kurzech. Věnujte pozornost zavřít toothese zabezpečení ikony, jak projít kroky tooremove hello a nahraďte komponenty zařízení.

| Ikona | Text | Další informace |
|:--- |:--- |:--- |
| ![Ikona upozornění](./media/storsimple-hardware-component-replacement/Warning.png) |**NEBEZPEČÍ!** |Označuje nebezpečné situace, která, pokud není vyhnout, bude výsledkem smrt nebo vážné škody. Signál slovo je omezená toohello nejvíce extrémních situacích. |
| ![Ikona upozornění](./media/storsimple-hardware-component-replacement/Warning.png) |**UPOZORNĚNÍ!** |Označuje nebezpečné situace, která, pokud není vyhnout, může mít za následek smrt nebo vážné škody. |
| ![Ikona upozornění](./media/storsimple-hardware-component-replacement/Caution.png) |**UPOZORNĚNÍ!** |Označuje nebezpečné situace, která, pokud není vyhnout, může mít za následek menší nebo střední škody. |
| ![Ikona upozornění](./media/storsimple-hardware-component-replacement/NoticeIcon.png) |**UPOZORNĚNÍ:** |Označuje informace, které jsou považovány za důležité, ale není nebezpečí související. |
| ![Ikona elektrický rázu](./media/storsimple-hardware-component-replacement/Electric.png) |**Elektrický rázu nebezpečí** |Označuje napětí vysoké. |
| ![Ikona velkou váhy](./media/storsimple-hardware-component-replacement/Weight.png) |**Velkou váhy** | |
| ![Žádná obsluhovatelná částí ikona uživatele](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png) |**Žádná Obsluhovatelná části uživatele** |K přístup, pokud správně cvičení. |
| ![Ikona pokyny pro čtení](./media/storsimple-hardware-component-replacement/ReadInstructions.png) |**Číst všechny pokyny nejprve** | |
| ![Nebezpečí ikona tipu](./media/storsimple-hardware-component-replacement/TipHazard.png) |**Tip nebezpečí** | |

### <a name="before-you-begin"></a>Než začnete
Seznamte se s hello bezpečnostní informace o vašem zařízení a bezpečnost ikony používané v tomto kurzu. Přejděte příliš[bezpečně instalaci a provoz zařízení StorSimple](storsimple-safety.md) úplné informace. Se, zda text hello tooreview [bezpečnostní opatření](storsimple-safety.md#handling-precautions) před zpracovat zařízení StorSimple. 

Před provedením tooreplace komponentu, zvažte hello následující informace.

![Ikona upozornění](./media/storsimple-hardware-component-replacement/Warning.png) ![elektrický rázu ikonu](./media/storsimple-hardware-component-replacement/Electric.png) **upozornění!** 

* Pozadí sami správně pomocí elektrostatické vyřízení nebo antistatická mat při zpracování moduly a součástí zařízení StorSimple.
* Není touch žádné zapojení. Použití hello zadat obslužné rutiny a příručky při zpracování součásti, které mohou být zpřístupněny zapojení.

![Ikona upozornění](./media/storsimple-hardware-component-replacement/Warning.png) ![Všimněte si, ikona](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **oznámení:**

Když nahradíte modul, **nikdy neopustí prázdný bay v hello zadní části hello skříň**. Před odebráním hello problém část získejte náhrada nebo prázdné modulu.

## <a name="hardware-component-replacement-procedures"></a>Hardwarové součásti náhradní postupy
Vaše zařízení řady StorSimple 8000 se skládá z několika moduly plug-in v hello primární a EBOD skříně. Hello 8100 má jeden primární skříň, kdežto hello 8600 zařízení duální skříň s primární skříně a EBOD skříň.

Hello hlavní hardwarové součásti v zařízení jsou shrnuté v následujících tabulkách hello. Kliknutím na odkaz hello v hello **postup nahrazení** sloupec toogo toohello přidružené kurzu.

| Komponenty | # Přítomen | Modul plug-in? | Postup při nahrazení |
|:--- |:--- |:--- |:--- |
| Skříň |1 |Ne |[Nahraďte hello skříň zařízení StorSimple](storsimple-chassis-replacement.md) |
| Primární řadiče |2 |Ano |[Nahraďte modul řadiče zařízení StorSimple](storsimple-controller-replacement.md) |
| 764W napájení a chlazení moduly (PCMs) |2 |Ano |[Nahrazení energii a chlazení modulu zařízení StorSimple](storsimple-power-cooling-module-replacement.md) |
| Zálohování baterie |2 |Ano |[Nahraďte hello modulu zálohování baterie zařízení StorSimple](storsimple-battery-replacement.md) |
| Diskové jednotky |12 |Ano |[Místo disku v zařízení StorSimple](storsimple-disk-drive-replacement.md) |

**Tabulka 1** hardwarové součásti v primární skříň hello

primární skříň Hello a skříň EBOD hello se liší v jejich vstupně-výstupní moduly. Navíc hello PCMs mají různé příkon. Hello PCMs v primární skříň hello 764 W, zatímco v hello EBOD skříň jsou 580 k. PCMs hello v hello primární skříň obsahovat také modul zálohování baterie.

| Komponenty | # Přítomen | Modul plug-in? | Postup při nahrazení |
|:--- |:--- |:--- |:--- |
| Skříň |1 |Ne |[Nahraďte hello skříň zařízení StorSimple](storsimple-chassis-replacement.md) |
| EBOD řadiče |2 |Ano |[Nahraďte řadič EBOD zařízení StorSimple](storsimple-ebod-controller-replacement.md) |
| 580W napájení a chlazení moduly (PCMs) |2 |Ano |[Nahrazení energii a chlazení modulu zařízení StorSimple](storsimple-power-cooling-module-replacement.md) |
| Diskové jednotky |12 |Ano |[Místo disku v zařízení StorSimple](storsimple-disk-drive-replacement.md) |

**Tabulka 2** hardwarové součásti v hello EBOD skříň

moduly plug-in Hello na hello zařízení jsou vyznačené na následující diagramy přední a zadní hello. Tyto diagramy toodetermine hello umístění hello můžete použít různé moduly plug-in Pokud náhradní se vyžaduje. Hello front diagram znázorňuje hello diskových jednotek a hello zadní schémata hello EBOD skříně a primární skříň hello zobrazují hello moduly plug-in.

![Frontplane zařízení s diskové jednotky](./media/storsimple-hardware-component-replacement/IC741028.png)

**Obrázek 1** Front hello zařízení

| Štítek | Popis |
|:--- |:--- |
| 0 - 11 |Diskové jednotky (celkem 12) |

Primární skříň hello i hello EBOD skříň mají moduly, poskytovatel jednotky. Skříň Hello je umístěno dvanáct 3,5" diskové jednotky uspořádané ve formátu 3 ve 4.

![Propojovací rozhraní systému modulů skříň primární zařízení](./media/storsimple-hardware-component-replacement/IC740994.png)

**Obrázek 2** zadní primární skříň hello

| Štítek | Popis |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |Řadič 0 |
| 4 |Řadič 1 |

![Propojovací rozhraní systému zařízení EBOD skříň moduly plug-in](./media/storsimple-hardware-component-replacement/IC769599.png)

**Obrázek 3** zadní hello EBOD skříň

| Štítek | Popis |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |EBOD řadič 0 |
| 4 |EBOD řadiči 1 |

## <a name="field-replaceable-units"></a>Nahraditelné jednotky pole
Hello následující pole nahraditelné jednotky (FRU) jsou k dispozici pro zařízení StorSimple:

* Skříň (včetně hello integrované operace panelu)
* 764 W AC PCM
* 580 W AC PCM
* Pevný disk s modulem poskytovatel jednotky
* Modul řadiče
* Modul EBOD řadiče
* Modul zálohování baterie
* Rack připojení liště kit

Prosím [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) tooorder některé z těchto jednotek nahrazení.

## <a name="next-steps"></a>Další kroky
Zkontrolujte všechny [bezpečnostní informace](storsimple-safety.md) před dalším pokusem tooreplace hardwarová komponenta StorSimple.

