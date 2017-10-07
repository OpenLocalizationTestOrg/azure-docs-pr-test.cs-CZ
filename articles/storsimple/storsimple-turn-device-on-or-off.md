---
title: "aaaTurn zařízení řady StorSimple 8000 zapnout nebo vypnout | Microsoft Docs"
description: "Vysvětluje, jak tooturn na nové zařízení StorSimple, zapněte na zařízení, které byl vypnut, nebo došlo ke ztrátě napájení a vypnout spuštěné zařízení."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 8e9c6e6c-965c-4a81-81bd-e1c523a14c82
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85434bde9377e330cd6ba4797fd5fd68bcee944d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="turn-on-or-turn-off-your-storsimple-8000-series-device"></a>Zapněte nebo vypněte zařízení řady StorSimple 8000
## <a name="overview"></a>Přehled
Vypínání Microsoft Azure StorSimple zařízení není vyžadován jako součást operace normální systému. Ale musíte tooturn na nové zařízení nebo zařízení, která měla toobe vypnout. Obecně platí vypnutí se vyžaduje v případech, ve kterých je musí nahradit selhání hardwaru, fyzicky přesunout jednotku nebo trvat zařízení mimo provoz. Tento kurz popisuje hello požadované postup pro zapnutí a vypnutí zařízení StorSimple v různých scénářích.

## <a name="turn-on-a-new-device"></a>Zapnout nové zařízení
Hello postup pro zapnutí zařízení StorSimple pro hello poprvé lišit v závislosti na tom, jestli je zařízení hello 8100 nebo 8600 model. Hello 8100 má jeden primární skříň, kdežto hello 8600 zařízení duální skříň s primární skříně a EBOD skříň. Hello podrobné kroky pro oba modely jsou popsané v následující části hello.

* [Nové zařízení s pouze primární skříň](#new-device-with-primary-enclosure-only)
* [Nové zařízení s EBOD skříň](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Nové zařízení s pouze primární skříň
model Hello StorSimple 8100 je jeden skříň zařízení. Zařízení obsahuje redundantní napájení a chlazení moduly (PCMs). Jak PCMs musí být nainstalovaný a připojený toodifferent power zdroje tooensure vysokou dostupnost.

Proveďte následující kroky toocable hello zařízení pro napájení.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

> [!NOTE]
> Pro dokončení zařízení nastavení a kabeláž pokyny, přejděte příliš[instalace zařízení StorSimple 8100](storsimple-8100-hardware-installation.md). Zajistěte, aby přesně postupujte podle pokynů hello.
> 
> 

### <a name="new-device-with-ebod-enclosure"></a>Nové zařízení s EBOD skříň
model Hello StorSimple 8600 má skříni primární i EBOD skříň. To vyžaduje toobe jednotky hello společně zapojena jeho kabeláž připojení Serial Attached (SCSI SAS) a napájení.

Při nastavování tohoto zařízení pro hello poprvé, proveďte kroky hello SAS kabelů nejprve a pak dokončení hello kroky pro kabeláž napájení.

[!INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

> [!NOTE]
> Pro dokončení zařízení nastavení a kabeláž pokyny, přejděte příliš[instalace zařízení StorSimple 8600](storsimple-8600-hardware-installation.md). Zajistěte, aby přesně postupujte podle pokynů hello.

## <a name="turn-on-a-device-after-shutdown"></a>Zapnout zařízení po vypnutí
Hello postup pro zapnutí zařízení StorSimple, poté, co byl vypnut se liší v závislosti na tom, jestli je zařízení hello 8100 nebo 8600 model. Hello 8100 má jeden primární skříň, kdežto hello 8600 zařízení duální skříň s primární skříně a EBOD skříň.

* [Zařízení s pouze primární skříň](#device-with-primary-enclosure-only)
* [Zařízení s EBOD skříň](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Zařízení s pouze primární skříň
Po vypnutí použijte následující postup tooturn v zařízení StorSimple s primární skříně a žádné EBOD skříň hello.

#### <a name="tooturn-on-a-device-with-a-primary-enclosure-only"></a>tooturn na zařízení s pouze primární skříň
1. Ujistěte se, že hello power přepne na obou napájení a chlazení moduly (PCMs) jsou v pozici OFF hello. Pokud hello přepínače nejsou v pozici OFF hello, pak je otočte toohello OFF pozice a počkejte hello indikátory toogo vypnout.
2. Zapněte hello zařízení překlopení hello napájení přepínače na obou PCMs toohello ON pozici. zařízení Hello měli zapnout.
3. Kontrola hello následující tooverify, který hello zařízení je zcela na:
   
   1. Hello OK LED na obou PCM moduly jsou zelená.
   2. Stav Hello LED na oba řadiče jsou plnou zeleně.
   3. Dobrý den, který bliká blue DIODU na jednom z řadičů hello označující, že je tento řadič hello aktivní.
      
      Pokud nejsou splněny některé z těchto podmínek, pak zařízení není v pořádku. Prosím [kontaktovat Microsoft Support](storsimple-8000-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Zařízení s EBOD skříň
Po vypnutí použijte následující postup tooturn v zařízení StorSimple s primární skříně a skříň EBOD hello. Každý krok proveďte přesně tak, jak je popsáno v pořadí. Selhání toodo tak může vést ke ztrátě dat.

#### <a name="tooturn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>tooturn na zařízení s primární a EBOD skříň
1. Zajistěte, aby byl tento hello EBOD skříň připojené toohello primární skříň. Další informace najdete v tématu [instalace zařízení StorSimple 8600](storsimple-8600-hardware-installation.md).
2. Ujistěte se, že hello napájení a chlazení moduly (PCMs) na obou hello EBOD a primární skříně jsou v pozici hello OFF. Pokud hello přepínače nejsou v pozici OFF hello, pak je otočte toohello OFF pozice a počkejte hello indikátory toogo vypnout.
3. Zapněte hello EBOD skříň první překlopení hello napájení přepínače na obou PCMs toohello ON pozici. by měly být Hello PCM LED. Zelená EBOD řadič DIODU na tuto jednotku označuje, že skříň EBOD hello je na.
4. Zapněte primární skříň hello překlopení hello napájení přepínače na obou PCMs toohello ON pozici. na by teď měly být Hello celý systém.
5. Ověřte, zda hello SAS LED budou zelené, což zajistí, že hello připojení mezi hello EBOD skříň a je vhodný pro primární skříň hello.

## <a name="turn-on-a-device-after-a-power-loss"></a>Zapnout zařízení po výpadku napájení
Vypnout výpadku napájení nebo přerušení zařízení StorSimple. výpadku napájení Hello může dojít na jednom z hello napájení nebo oba zdroje napájení. Postup obnovení Hello se liší v závislosti na tom, jestli je zařízení hello 8100 nebo 8600 model. Hello 8100 má jeden primární skříň, kdežto hello 8600 zařízení duální skříň s primární skříně a EBOD skříň. Tato část popisuje postup pro zotavení hello pro jednotlivé scénáře.

* [Zařízení s pouze primární skříň](#8100)
* [Zařízení s EBOD skříň](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Zařízení s pouze primární skříň<a name="8100">
Hello systému můžete dál jeho normální provoz, pokud je tooone ztrátě napájení z jeho napájení. Ale tooensure vysokou dostupnost hello zařízení, obnovení power toohello napájení co nejdříve.

Pokud dojde k výpadku napájení nebo napájení přerušení na oba zdroje napájení, hello systém se vypne uspořádání a řízené způsobem. Když se obnoví hello power hello systému budou automaticky zapnout.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Zařízení s EBOD skříň<a name="8600">
#### <a name="power-loss-on-one-power-supply"></a>Zadejte výpadku napájení na jednu napájení
Hello systému můžete dál jeho normální provoz, pokud je tooone ztrátě napájení z jeho napájení na primární skříň hello nebo hello EBOD skříň. Ale tooensure vysokou dostupnost hello zařízení, obnovte napájení power toohello co nejdříve.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Výpadku napájení na obou napájení na primární server a EBOD skříně
Pokud není power výpadku nebo napájení přerušení na oba zdroje napájení, hello EBOD skříň ihned vypne a primární skříň hello vypne uspořádání a řízené způsobem. Po obnovení napájení, automaticky se spustí hello zařízení.

Napájení hello vypnut ručně, proveďte následující kroky toorestore power toohello systému hello.

1. Zapněte hello EBOD skříň.
2. Po hello EBOD skříň na zapněte primární skříň hello.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Výpadku napájení na obou napájení na EBOD skříň
Při nastavování kabely, ujistěte se, že hello EBOD se nikdy připojené samostatně tooa oddělení protokolu PDU. Pokud hello EBOD a primární skříň selhat v hello současně, obnoví hello systému.

Pokud jenom hello EBOD skříň selže na oba zdroje napájení, nebude automaticky obnovit hello systému. Proveďte následující kroky tooturn systému hello hello a obnovte ji tooa stavu v pořádku:

1. Pokud je zapnutá hello primární skříň, vypněte napájení a chlazení moduly (PCMs).
2. Počkejte několik minut, než hello systému tooshut dolů.
3. Zapněte hello EBOD skříň.
4. Po hello EBOD skříň na zapněte primární skříň hello.

## <a name="turn-on-a-device-after-hello-primary-and-ebod-enclosure-connection-is-lost"></a>Zapnout v zařízení po hello primární a dojde ke ztrátě připojení EBOD skříň
Pokud dojde ke ztrátě mezi hello pohotovostní kontroler a odpovídající kontroler EBOD hello hello připojení, pokračuje v zařízení hello toowork. Pokud dojde ke ztrátě připojení hello mezi hello systému aktivního řadiče a hello odpovídající EBOD řadiče, mělo dojít k převzetí služeb při selhání a hello zařízení by měly pokračovat ve toowork jako normální.

Pokud jsou odebrány i kabely Serial Attached (SCSI SAS) nebo je porušeno hello připojení mezi hello EBOD skříň a hello primární skříň, hello zařízení přestane fungovat. V tomto okamžiku proveďte následující kroky hello.

### <a name="tooturn-on-hello-device-after-connection-is-lost"></a>tooturn na hello zařízení po ztrátě připojení
1. Přístup hello zadní hello zařízení.
2. Pokud hello SAS kabel mezi hello EBOD skříň a primární skříň hello se přeruší, SAS dráhy všechny LED na hello EBOD skříň bude vypnuto.
3. Vypnutí napájení a chlazení moduly (PCMs) na skříň EBOD hello a primární skříň hello.
4. Počkejte, dokud všechny indikátory hello na hello zadní obou hello skříně vypnout.
5. Vložte hello SAS kabely a zkontrolujte, zda je funkční připojení mezi hello EBOD skříň a primární skříň hello.
6. Zapněte hello EBOD skříň první překlopení obou PCM přepínače toohello ON pozici.
7. Zajistěte, aby byl hello EBOD skříň na kontrolou, jestli hello zelená DIODU je ON.
8. Zapněte primární skříň hello.
9. Zajistěte, aby byl primární skříň hello na kontrolou, že je hello řadiče zelená DIODU na.
10. Ověřte, že tento hello EBOD skříň připojení s primární skříň hello je vhodný kontrolou, že hello SAS dráhy LED (čtyři na jeden kontroler EBOD) jsou všechny na.

> [!IMPORTANT]
> Pokud jsou vadný hello SAS kabely nebo hello připojení hello EBOD skříň primární skříň hello není funkční, když zapnete hello systému, protože budou přenášeny do režimu obnovení. Prosím [kontaktovat Microsoft Support](storsimple-8000-contact-microsoft-support.md) v takovém případě.


## <a name="turn-off-a-running-device"></a>Vypnout spuštěné zařízení
Spuštěné zařízení StorSimple může být nutné toobe ukončí, pokud se během přesunu, vyřazeno z provozu, nebo má chybně fungující komponenty, která potřebuje toobe nahradit. Postup Hello se liší v závislosti na tom, jestli je zařízení StorSimple hello 8100 nebo 8600 model. Hello 8100 má jeden primární skříň, kdežto hello 8600 zařízení duální skříň s primární skříně a EBOD skříň. V této části podrobně hello kroky tooshut dolů spuštěné zařízení.

* [Zařízení s primární skříň](#8100a)
* [Zařízení s EBOD skříň](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Zařízení s primární skříň<a name="8100a">
tooshut dolů hello zařízení uspořádání a řízené způsobem, můžete provést prostřednictvím hello portál Azure classic nebo hello Windows Powershellu pro StorSimple. 

> [!IMPORTANT]
> Nelze vypnout spuštěné zařízení pomocí tlačítka napájení hello hello zadní hello zařízení.
> 
> Před ukončením hello zařízení, ujistěte se, zda jsou všechny součásti zařízení hello v pořádku. V hello portál Azure classic, přejděte příliš**zařízení** > **údržby** > **stavu hardwaru**a ověřte, že stav všech hello součástí je zobrazen zeleně. To platí pouze pro systém v pořádku. Pokud systém hello je vrácení vypnout tooreplace chybně fungující součást, zobrazí se nezdařila (červený) nebo snížení (žlutý) stav hello příslušných součástí hello **stavu hardwaru**.
> 
> 

Po získání přístupu hello Windows Powershellu pro StorSimple nebo hello portál Azure classic, postupujte podle kroků hello v [vypnout zařízení StorSimple](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Zařízení s EBOD skříň<a name="8600a">
> [!IMPORTANT]
> Před ukončením hello primární skříně a hello EBOD skříň, zajistěte, aby byly všechny součásti zařízení hello v pořádku. V hello portálu Azure, přejděte příliš**zařízení** > **monitorování** > **stavu hardwaru**a ověřte, zda jsou všechny součásti hello v pořádku.


#### <a name="tooshut-down-a-running-device-with-ebod-enclosure"></a>tooshut dolů spuštěné zařízení s EBOD skříň
1. Proveďte všechny kroky hello uvedené v [vypnout zařízení StorSimple](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device) pro primární skříň hello.
2. Po hello primární skříň je vypnutý, ukončení hello EBOD překlopení vypnutí napájení a chlazení modulu (PCM) přepínače.
3. tooverify, který hello EBOD byl vypnut, zkontrolujte všechny světel na hello zadní hello EBOD skříň jsou vypnuté.

> [!NOTE]
> Hello SAS kabely, které jsou používané tooconnect hello EBOD skříň toohello primární skříň by se neměly odebírat až po vypnutí systému hello.

## <a name="next-steps"></a>Další kroky
[Kontaktujte Microsoft Support](storsimple-8000-contact-microsoft-support.md) Pokud narazíte na problémy při zapnutí nebo vypnutí zařízení StorSimple.

