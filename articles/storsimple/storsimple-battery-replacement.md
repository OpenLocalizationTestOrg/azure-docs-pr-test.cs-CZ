---
title: "aaaReplace baterie na zařízení Microsoft Azure StorSimple | Microsoft Docs"
description: "Popisuje, jak nahradit tooremove a udržovat hello modulu zálohování baterie zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3c8a6654-4826-4883-aad8-75f332347c53
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 542774a5f451ec7ad2bd442f88598df318d8b285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-backup-battery-module-on-your-storsimple-device"></a>Nahraďte hello modulu zálohování baterie zařízení StorSimple
## <a name="overview"></a>Přehled
primární skříň Hello napájení a chlazení modulu (PCM) na zařízení s Microsoft Azure StorSimple má balík další stav baterie. Tento balíček poskytuje power tak, aby hello zařízení StorSimple můžete uložit data Pokud dojde ke ztrátě AC power toohello primární skříň. Tato sada pack baterie je hello odkazované tooas *zálohování baterie modulu*. modul zálohování baterie Hello existuje pouze pro primární skříň hello v zařízení StorSimple (hello EBOD skříň neobsahuje modul zálohování baterie). 

Tento kurz vysvětluje postup:

* Modul zálohování baterie hello odebrat 
* Nainstalujte nový modul zálohování baterie
* Udržovat hello zálohování baterie modulu

> [!IMPORTANT]
> Před odebírání a nahrazování modul zálohování baterie zkontrolovat informace o zabezpečení hello v hello [Úvod tooStorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="remove-hello-backup-battery-module"></a>Modul zálohování baterie hello odebrat
modul Hello zálohování baterie pro zařízení StorSimple je periferní Výměnná jednotka. Před instalací v hello PCM, modul baterie hello by měly být uložené v původním balení. Proveďte následující kroky tooremove hello zálohování baterie hello.

#### <a name="tooremove-hello-backup-battery-module"></a>modul zálohování baterie tooremove hello
1. V hello portál Azure classic, přejděte příliš**zařízení** > **údržby** > **stavu hardwaru**. V části **sdílené součásti**, podívejte se na stav baterie hello hello.
2. Identifikujte hello PCM, ve které hello baterie selhal. Obrázek 1 zobrazuje hello zadní hello zařízení StorSimple.
   
    ![Propojovací rozhraní systému modulů skříň primární zařízení](./media/storsimple-battery-replacement/IC740994.png)
   
    **Obrázek 1** zadní zobrazující modulů PCM a řadiče primární zařízení
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Řadič 0 |
   | 4 |Řadič 1 |
   
    Jak je znázorněno číslem 3 v hello obrázek 2, hello monitorování indikátor VEDLA na PCM 0, která odpovídá příliš**baterie selhání** by měl být lit.
   
    ![Propojovací rozhraní systému zařízení PCM monitorování kláves](./media/storsimple-battery-replacement/IC740992.png)
   
    **Obrázek 2** zpět of PCM zobrazující hello monitorování kláves
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |Výpadku napájení ze sítě |
   | 2 |Ventilátor selhání |
   | 3 |Selhání stav baterie. |
   | 4 |PCM OK |
   | 5 |Řadič domény výpadku proudu |
   | 6 |Dobrý stav baterie |
3. tooremove hello PCM s selhání baterie, postupujte podle kroků hello v [odebrat PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).
4. S hello PCM odebrány, navýšení a otáčení hello baterie modul zpracování směrem nahoru, jak je uvedeno v hello následující obrázek a vyžádat si tooremove hello baterie.
   
    ![Odebráním PCM stav baterie.](./media/storsimple-battery-replacement/IC741019.png)
   
    **Obrázek 3** odebráním hello PCM hello baterie
5. Umístěte hello modulu hello periferní výměnná Jednotka balení.
6. Vrátí hello vadný jednotky tooMicrosoft správnou údržbu a zpracování.

## <a name="install-a-new-backup-battery-module"></a>Nainstalujte nový modul zálohování baterie
Proveďte následující kroky tooinstall hello nahrazení baterie modulu v hello PCM v primární skříň hello zařízení StorSimple hello.

#### <a name="tooinstall-hello-battery-module"></a>tooinstall hello baterie modulu
1. Umístěte hello zálohování baterie modulu hello správnou orientaci v hello PCM.
2. Stiskněte klávesu dolů hello baterie modul zpracování všech hello způsob tooseat hello konektor.
3. Nahraďte text hello PCM v primární skříň hello podle následujících pokynů hello v [nahrazení energii a chlazení modulu zařízení StorSimple](storsimple-power-cooling-module-replacement.md).
4. Po dokončení hello nahrazení přejděte příliš**zařízení** > **údržby** > **stavu hardwaru** v hello portál Azure classic. Zkontrolujte stav hello toomake baterie hello se, zda text hello instalace byla úspěšná. Zelený stav označuje, že baterie hello je v pořádku.

## <a name="maintain-hello-backup-battery-module"></a>Udržovat hello zálohování baterie modulu
V zařízení StorSimple modul zálohování baterie hello poskytuje řadič toohello spotřeby během ztrátě power. To umožňuje hello StorSimple zařízení toosave důležitá data předchozí tooshutting dolů řízené způsobem. S dvě plně účtovat baterie v hello PCMs hello systému může zpracovávat dvě po sobě jdoucích ztrátu události.

V hello portál Azure classic, hello **stavu hardwaru** na hello **údržby** stránky určuje, zda pracuje správně hello baterie nebo se blíží hello end životnosti. je indikován stav baterie Hello **baterie v PCM 0** nebo **baterie v PCM 1** pod **sdílené součásti**. Tato stránka se zobrazí **SNÍŽENÝ** stavu pro ukončenou životností blíží, a **se nezdařilo** pro koncové životnosti dostupný. 

> [!NOTE]
> může hlásit Hello baterie **se nezdařilo** když ho jednoduše toobe účtovat potřebuje.
> 
> 

Pokud hello **SNÍŽENÝ** stav se zobrazí, doporučujeme hello následující postup:

* Hello systému pravděpodobně došlo k poslední výpadku napájení nebo hello baterie může probíhat periodické údržby. Sledujte hello systému 12 hodin, než budete pokračovat.
  
  * Pokud stav hello je stále **SNÍŽENÝ** po 12 hodinách napájení tooAC nepřetržité připojení s hello řadiče a PCMs spuštěna, pak hello stav baterie musí toobe nahradit. Prosím [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) pro modul zálohování baterie nahrazení.
  * Pokud hello stav se změní na OK po 12 hodinách, baterie hello je funkční a potřeba jenom údržby poplatků.
* Pokud nedošlo k přidružené ke ztrátě napájení ze sítě a hello PCM je zapnutý a připojený tooAC power, baterie hello musí toobe nahradit. [Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) tooorder modul zálohování baterie nahrazení.

> [!IMPORTANT]
> Uvolnění hello se nezdařila, baterie podle toonational a místní předpisy. 
> 
> 

## <a name="next-steps"></a>Další kroky
Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).

