---
title: "aaaReplace PCM na vašem zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Vysvětluje, jak tooremove a nahradit text hello energii a chlazení modulu (PCM) v zařízení StorSimple"
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 474fd09787c5361a81efda4de74356027ac60f47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Nahrazení energii a chlazení modulu zařízení StorSimple
## <a name="overview"></a>Přehled
zdroj napájení a chladicí ventilátory, které jsou řízena pomocí hello primární a skříně EBOD se skládá Hello napájení a chlazení modulu (PCM) v zařízení s Microsoft Azure StorSimple. Existuje jenom jeden model PCM, který je certifikované pro každou skříň. primární skříň Hello je certifikované pro 764 W PCM a skříň EBOD hello je certifikované pro 580 W PCM. I když jsou různé hello PCMs pro primární skříň hello a hello EBOD skříň, hello nahrazení postup se shoduje.

Tento kurz vysvětluje postup:

* Odebrat PCM
* Nainstalovat náhradní PCM

> [!IMPORTANT]
> Před odebráním a nahraďte PCM, zkontrolujte informace zabezpečení hello v [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).


## <a name="before-you-replace-a-pcm"></a>Před nahrazením PCM
Mějte na paměti následující důležité skutečnosti, před nahrazením vaší PCM Dobrý den:

* Pokud hello zdroj napájení z hello PCM selže, nechte hello vadný modul nainstalován, ale odebrat hello napájecí kabel. ventilátor Hello bude pokračovat tooreceive napájení z hello skříň a bude pokračovat tooprovide správné chlazení. V případě selhání hello ventilátor hello PCM musí toobe nahradit okamžitě.
* Před odebráním hello PCM, odpojte hello power od hello PCM vypnutím hlavní přepínač hello (pokud existuje) nebo fyzicky odebráním hello napájecí kabel. To poskytuje systém tooyour upozornění hrozí vypnutí napájení.
* Ujistěte se, že tento hello jiných PCM je funkční pro pokračování operace systému před nahrazení hello vadný PCM. Vadný PCM se musí nahradit odpovídajícími plně funkční PCM co nejdříve.
* Nahrazení modulu PCM trvá pouze několik minut toocomplete, ale jeho musí být dokončeny v rámci 10 minut odebrání přehřívání tooprevent PCM hello se nezdařilo.
* Všimněte si, že hello nahrazení 764 W PCM moduly dodaný z objektu pro vytváření hello neobsahují hello zálohování baterie modulu. Bude potřebovat tooremove hello baterie z vaší vadný PCM a vložte ji do hello nahrazení modulu předchozí tooperforming hello nahrazení. Další informace najdete v tématu Jak příliš[vyjměte a vložte modul zálohování baterie](storsimple-8000-battery-replacement.md).

## <a name="remove-a-pcm"></a>Odebrat PCM
Postupujte podle těchto pokynů, když jsou připravené tooremove napájení a chlazení modulu (PCM) ze zařízení s Microsoft Azure StorSimple.

> [!NOTE]
> Před odebráním vaší PCM, ověřte, zda máte správné nahrazení (764 W pro primární skříň hello) nebo 580 W pro hello EBOD skříň.

#### <a name="tooremove-a-pcm"></a>tooremove PCM
1. V hello portál Azure classic, klikněte na **Nastavení > Sledování > Stav hardwaru**. Zkontrolujte stav hello součástí hello PCM pod **sdílené součásti** tooidentify, který PCM se nezdařilo:
   
   * Pokud se nezdařila napájení v PCM 0, hello stav **zdroj napájení v PCM 0** červená.
   * Pokud napájení v PCM 1 se nezdařila, hello stav **zdroj napájení v PCM 1** červená.
   * Pokud hello ventilátor v PCM 1 se nezdařila, hello stav buď **chlazení 0 pro PCM 0** nebo **chlazení 1 pro PCM 0** červená.
2. Vyhledejte hello selhání PCM na hello zpět z primární skříň hello. Pokud používáte 8600 model, určete podle hello systémové jednotky identifikační číslo zobrazené v zobrazení předního panelu DIODU hello primární skříň hello. Hello výchozí ID jednotky zobrazené na primární skříň hello je **00**, kdežto hello výchozí ID jednotky zobrazené na hello EBOD skříň **01**. Hello následující obrázek a tabulka popisují hello přední panel hello DIODU zobrazení.
   
    ![ID systému na přední panel OPS](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     **Obrázek 1** přední panel hello zařízení  
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |Tlačítko pro ztlumení |
   | 2 |Napájení systému |
   | 3 |Selhání modulu |
   | 4 |Logické chyby |
   | 5 |Zobrazení ID jednotky |
3. Hello monitorování kláves v hello zadní primární skříň hello lze také použít tooidentify hello PCM vadný. V tématu hello následující diagram a jak tabulky toounderstand toouse hello LED toolocate hello PCM vadný. Například pokud hello VEDLA odpovídající toohello **ventilátor nezdaří** je lit, ventilátor hello se nezdařilo. Podobně pokud hello zobrazí průvodce odpovídající příliš**AC selhání** je lit, napájení hello se nezdařilo. 
   
    ![Propojovací rozhraní zařízení PCM monitorování ukazatele LED](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     **Obrázek 2** zpět of PCM s indikátor LED
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |Výpadku napájení ze sítě |
   | 2 |Ventilátor selhání |
   | 3 |Selhání stav baterie. |
   | 4 |PCM OK |
   | 5 |Řadič domény výpadku proudu |
   | 6 |Dobrý stav baterie |
4. Následující diagram hello zadní hello StorSimple zařízení toolocate hello se nezdařilo PCM modulu toohello naleznete v nápovědě. PCM 0 je na levé straně hello a PCM 1 je na správné hello. Hello tabulky, který následuje dále vysvětluje hello moduly.
   
     ![Propojovací rozhraní systému modulů skříň primární zařízení](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     **Obrázek 3** zadní zařízení s moduly plug-in 
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Řadič 0 |
   | 4 |Řadič 1 |
5. Zapnout hello vadný PCM a odpojit kabel napájení power hello. Nyní můžete odebrat hello PCM.
6. Pochopit hello západky a hello straně hello PCM zpracování mezi Flash a ukazováčkem a vměstnat je společně tooopen hello popisovač.
   
    ![Otevírání PCM popisovač](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    **Obrázek 4** otevírání hello PCM zpracování
7. Hello úchytu zpracování a odeberte hello PCM.
   
    ![Odebrání zařízení PCM](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    **Obrázek 5** odebrání hello PCM

## <a name="install-a-replacement-pcm"></a>Nainstalovat náhradní PCM
Postupujte podle těchto pokynů tooinstall PCM v zařízení StorSimple. Ujistěte se, že jste vložili hello zálohování baterie modulu předchozí tooinstalling hello nahrazení PCM (platí too764 pouze W PCMs). Další informace najdete v tématu Jak příliš[vyjměte a vložte modul zálohování baterie](storsimple-8000-battery-replacement.md).

#### <a name="tooinstall-a-pcm"></a>tooinstall PCM
1. Ověřte, zda máte správné nahrazení hello PCM pro tento skříň. primární skříň Hello musí 764 W PCM a hello EBOD skříň musí 580 W PCM. Byste neměli toouse hello 580 PCM W v primární skříň hello nebo hello 764 PCM W v hello EBOD skříň. Následující obrázek ukazuje, kdy tooidentify tyto informace na hello popisku, který je připojeno toohello PCM Hello.
   
    ![Popisek PCM zařízení](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    **Obrázek 6** PCM popisek
2. Zkontrolujte, zda škody toohello skříň, věnujte zvláštní pozornost toohello konektory. 
   
   > [!NOTE]
   > **Pokud jsou všechny kódy PIN konektor ohnuty není nainstalovaný modul hello.**
   > 
   > 
3. S hello PCM zpracování v hello otevřete pozici, modul hello snímku hello skříň.
   
    ![Instalace zařízení PCM](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    **Obrázek 7** instalace hello PCM
4. Ruční zavření hello PCM popisovač. Klikněte na by měla být slyšet jako zapojí západky popisovač hello.
   
   > [!NOTE]
   > tooensure, který konektor mít zapojení kódy PIN, můžete se na popisovač hello bez uvolnění západky hello jemně tug hello. Pokud hello PCM snímky, znamená to, že tuto západku hello se zavřel před hello konektory pověření.
   
5. Připojte zdroj energie hello power kabely toohello a toohello PCM.
6. Zabezpečte hello kmen balících pomoc.
7. Zapněte hello PCM.
8. Ověřte, že hello nahrazení byla úspěšná: v hello portál Azure služby StorSimple Manager zařízení, přejděte tooyour zařízení a potom příliš**Nastavení > Sledování > Stav hardwaru**. V části hello **sdílené součásti**, by měly být hello stav hello PCM.
   
   > [!NOTE]
   > To může trvat několik minut, než hello nahrazení PCM toocompletely inicializovat.

## <a name="next-steps"></a>Další kroky
Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-8000-hardware-component-replacement.md).

