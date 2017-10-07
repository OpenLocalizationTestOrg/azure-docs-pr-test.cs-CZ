---
title: "aaaReplace řadič StorSimple EBOD | Microsoft Docs"
description: "Vysvětluje, jak tooremove a nahraďte jeden nebo oba řadiče EBOD na zařízení StorSimple 8600."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 8cbfa507-1a56-4e24-99dd-7db9abd3b850
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 5d29de2ee30bfdd70910050eee5cfa1d293d444f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Nahraďte řadič EBOD zařízení StorSimple
## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak tooreplace modul vadný řadič EBOD na zařízení s Microsoft Azure StorSimple. tooreplace modul EBOD řadiče, budete muset:

* Odebrání hello vadný EBOD řadiče
* Nainstalovat nový řadič EBOD

Vezměte v úvahu hello před zahájením následující informace:

* Prázdné EBOD moduly musí být vložený do všechny nepoužívané sloty. Skříň Hello nebude cool správně, pokud slotu je ponechány otevřené.
* je řadič EBOD Hello za provozu a můžete ho odebrat nebo nahradit. Neodebírejte selhání modulu, dokud nebudete mít nahrazení. Když zahájíte proces nahrazení hello, musíte ji dokončíte během 10 minut.

> [!IMPORTANT]
> Před pokusem o tooremove nebo nahradit všechny součásti, StorSimple, ujistěte se, abyste si prošli hello [zabezpečení ikonu konvence](storsimple-safety.md#safety-icon-conventions) a dalších [bezpečnostní opatření](storsimple-safety.md).
> 
> 

## <a name="remove-an-ebod-controller"></a>Odebrat řadič EBOD
Před nahrazení hello chyba modulu EBOD řadiče v zařízení StorSimple, ujistěte se, že hello jiné řadiče modul EBOD je aktivní a spuštěná. Hello následující postup a tabulka vysvětlují, jak tooremove hello EBOD řadiče modulu.

#### <a name="tooremove-an-ebod-module"></a>tooremove modul EBOD
1. Hello otevřete portál Azure classic.
2. Přejděte příliš**zařízení** > **údržby** > **stavu hardwaru**a ověřte, že stav hello hello VEDLA pro hello active EBOD Řadič modulu je zobrazen zeleně a je red hello DIODU pro modul řadiče EBOD hello se nezdařilo.
3. Vyhledejte modul řadiče EBOD hello se nezdařila v hello zadní hello zařízení.
4. Odeberte hello kabely, které připojují hello EBOD řadiče modulu toohello řadiče před přepnutím hello EBOD modulu mimo hello systému.
5. Poznamenejte si hello přesný SAS portu hello EBOD řadiče modul, který není připojený toohello řadiče. Po nahrazení hello EBOD modulu, bude se konfigurace toothis systému požadované toorestore hello. 
   
   > [!NOTE]
   > Obvykle to bude Port A, který je označený jako **hostitele v** v hello následující diagram.
   > 
   > 
   
    ![Propojovací rozhraní EBOD řadiče](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     **Obrázek 1** zpět of EBOD modulu
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |Selhání DIODU |
   | 2 |Napájení DIODU |
   | 3 |SAS konektory |
   | 4 |LED SAS |
   | 5 |Sériové porty pouze pro objekt pro vytváření |
   | 6 |Port (hostitel v) |
   | 7 |Port B (hostitel out) |
   | 8 |Port C (pouze pro objekt pro vytváření použití) |

## <a name="install-a-new-ebod-controller"></a>Nainstalovat nový řadič EBOD
Hello následující postup a tabulka vysvětlují, jak tooinstall modul EBOD řadiče v zařízení StorSimple.

#### <a name="tooinstall-an-ebod-controller"></a>tooinstall řadič EBOD
1. Zkontrolujte zařízení EBOD hello škody, zejména toohello rozhraní konektoru. Neinstalujte nový řadič EBOD hello, pokud jsou ohnuty všechny kódy PIN.
2. S hello zámky v hello otevřete pozici, modul hello snímku hello skříň, dokud zámky hello zapojení.
   
    ![Instalaci EBOD řadiče](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    **Obrázek 2** instalace hello EBOD řadiče modulu
3. Zavřít hello západky. Klikněte na tlačítko by měla být slyšet jako zapojí západky hello.
   
    ![Uvolnění EBOD západky](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    **Obrázek 3** zavření hello EBOD modulu západky
4. Znovu připojte kabely hello. Použijte přesnou konfiguraci hello, který se nachází před hello nahrazení. Viz následující diagram hello a tabulky Podrobnosti o tom, kabely tooconnect hello.
   
    ![Zapojení kabeláže zařízení 4U pro napájení](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    **Obrázek 4**. Opětovné připojení kabely
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |Primární skříň |
   | 2 |PCM 0 |
   | 3 |PCM 1 |
   | 4 |Řadič 0 |
   | 5 |Řadič 1 |
   | 6 |EBOD řadič 0 |
   | 7 |EBOD řadiči 1 |
   | 8 |EBOD skříň |
   | 9 |Jednotek pro distribuci napájení |

## <a name="next-steps"></a>Další kroky
Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).

