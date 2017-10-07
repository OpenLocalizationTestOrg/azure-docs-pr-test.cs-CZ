---
title: "aaaReplace řadič zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak tooremove a nahraďte jeden nebo oba moduly řadiče zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e25b52b7-60f5-47f3-bffc-6c157d57ab5d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/03/2017
ms.author: alkohli
ms.openlocfilehash: ebf5c5830120857f69909113e3a111f4dda30e57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Nahraďte modul řadiče zařízení StorSimple
## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak tooremove a nahraďte jeden nebo oba řadiče modulů v zařízení StorSimple. Popisuje také hello základní logiku pro scénáře nahrazení jednoduchá a Duální řadiče hello.

> [!NOTE]
> Předchozí tooperforming nahrazení řadiče, doporučujeme vždy aktualizovat řadič nejnovější verzi firmwaru toohello vaše.
> 
> tooprevent poškodit tooyour zařízení StorSimple, není vysunutí hello řadiče, dokud hello LED zobrazují jako jeden z následujících hello:
> 
> * Všechny indikátory jsou OFF.
> * Indikátor 3, ![zelená ikona zaškrtnutí](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), a ![ikonu Red mezi](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) jsou přerušované, a jsou DIODU 0 a 7 DIODU **ON**.
> 
> 

Hello následující tabulka uvádí scénáře nahrazení řadiče hello podporována.

| Případ | Scénář nahrazení | Příslušný postup popsaný |
|:--- |:--- |:--- |
| 1 |Jeden řadič je ve stavu selhání, hello další řadič je v pořádku a aktivní. |[Jednotné nahrazení řadiče](#replace-a-single-controller), který popisuje hello [logiku za náhradní jediného kontroleru](#single-controller-replacement-logic), a také hello [kroky nahrazení](#single-controller-replacement-steps). |
| 2 |Oba řadiče hello se nezdařilo a vyžadovat nahrazení. Hello skříň, disky a skříně disku jsou v pořádku. |[Dva řadiče nahrazení](#replace-both-controllers), který popisuje hello [logiku za náhradní duální řadiče](#dual-controller-replacement-logic), a také hello [kroky nahrazení](#dual-controller-replacement-steps). |
| 3 |Řadičů z hello stejné zařízení nebo z různých zařízení jsou vzájemně zaměněny. Hello skříň, disky a skříně disku jsou v pořádku. |Zobrazí se zpráva upozornění neshoda slot. |
| 4 |Na jednom řadiči chybí a hello jiné řadiče selže. |[Dva řadiče nahrazení](#replace-both-controllers), který popisuje hello [logiku za náhradní duální řadiče](#dual-controller-replacement-logic), a také hello [kroky nahrazení](#dual-controller-replacement-steps). |
| 5 |Jeden nebo oba řadiče se nezdařilo. Hello zařízení nelze přistupovat prostřednictvím konzoly sériového portu hello nebo vzdálenou komunikaci prostředí Windows PowerShell. |[Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) postup nahrazení ruční řadiče. |
| 6 |mají řadiče Hello verzi různých sestavení, který může být z důvodu:<ul><li>Mají řadiče verze jiný software.</li><li>Verze firmwaru různých mají řadiče.</li></ul> |Pokud verze softwaru řadiče hello liší, hello nahrazení logiku rozpozná, že a aktualizace hello verze softwaru ve hello nahrazení řadiče.<br><br>Pokud verzí firmwaru řadiče hello se liší a je starší verze firmwaru hello **není** automaticky rozšiřitelný, se zobrazí zpráva s upozorněním v hello portál Azure classic. Doporučujeme vyhledávání aktualizací a nainstalovat aktualizace firmwaru hello.</br></br>Pokud verzí firmwaru řadiče hello se liší a je starší verze firmwaru hello automaticky rozšiřitelný, hello řadiče nahrazení logiku to zjistí a po spuštění řadič hello hello firmwaru, bude automaticky aktualizovat. |

Je nutné tooremove modul řadiče, pokud se nezdařila. Jeden nebo oba moduly hello řadič může selhat, což může vést ke jediného kontroleru náhrada nebo nahrazení dva řadiče. Náhradní postupy a logiku hello za je najdete v tématu hello následující:

* [Výměna jednoho kontroleru](#replace-a-single-controller)
* [Nahraďte oba řadiče](#replace-both-controllers)
* [Odebrání zařízení](#remove-a-controller)
* [Vložit řadič](#insert-a-controller)
* [Identifikovat řadič active hello na zařízení](#identify-the-active-controller-on-your-device)

> [!IMPORTANT]
> Před odebráním a nahrazení řadiče, zkontrolujte informace zabezpečení hello v [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="replace-a-single-controller"></a>Nahraďte jediného kontroleru
Pokud jedním z řadičů hello dva hello Microsoft Azure StorSimple zařízení se nezdařila, nepracuje správně nebo chybí, je potřeba tooreplace jediného kontroleru. 

### <a name="single-controller-replacement-logic"></a>Nahrazení logiku jediného kontroleru
V náhradní jediného kontroleru byste měli nejdřív odebrat hello řadič se nezdařilo. (hello zbývající řadič v zařízení hello je hello aktivní.) Při vložení hello nahrazení řadiče hello provedou tyto akce:

1. Nahrazení řadiče Hello okamžitě spustí komunikaci s hello zařízení StorSimple.
2. Snímek hello virtuální pevný disk (VHD) pro řadič active hello se zkopíruje na hello nahrazení řadiče.
3. snímek Hello upraví tak, aby při nahrazení řadiče hello spuštění z tohoto virtuálního pevného disku, bude rozpoznán jako pohotovostní řadiče.
4. Po dokončení úprav hello hello nahrazení řadiče se spustí jako pohotovostní řadič hello.
5. Pokud jsou oba řadiče hello spuštěna, hello clusteru přechodu do režimu online.

### <a name="single-controller-replacement-steps"></a>Kroky nahrazení jediného kontroleru
Proveďte následující kroky, pokud dojde k selhání jednoho z řadičů hello v zařízení s Microsoft Azure StorSimple hello. (hello jiný řadič musí být aktivní a spuštěná. Pokud oba řadiče selhat nebo nebude fungovat správně, přejděte příliš[kroky nahrazení duální řadiče](#dual-controller-replacement-steps).)

> [!NOTE]
> Může trvat 30 – 45 minut hello řadiče toorestart a zcela obnovit z procedury nahrazení hello jediného kontroleru. Hello celkovou dobu pro celý postup hello, včetně připojení hello kabely, je přibližně 2 hodiny.
> 
> 

#### <a name="tooremove-a-single-failed-controller-module"></a>tooremove modul jeden řadič se nezdařilo
1. V hello portál Azure classic, přejděte toohello služby StorSimple Manager, klikněte na tlačítko hello **zařízení** kartě a pak klikněte na název hello hello zařízení, které chcete toomonitor.
2. Přejděte příliš**údržby > stavu hardwaru**. Hello řadič 0 nebo 1 řadiče musí být ve stavu red, což naznačuje selhání.
   
   > [!NOTE]
   > Hello selhání řadiče v náhradní jediného kontroleru je vždy řadič pohotovostní.
   > 
   > 
3. Obrázek 1 a hello následující tabulky toolocate hello selhání řadiče modulu použijte.  
   
    ![Propojovací rozhraní systému modulů skříň primární zařízení](./media/storsimple-controller-replacement/IC740994.png)
   
    **Obrázek 1** zařízení StorSimple of zpět
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Řadič 0 |
   | 4 |Řadič 1 |
4. Na řadiči hello se nezdařila odebrání hello připojené síťové kabely hello data porty. Pokud používáte 8600 model, také odeberte hello SAS kabelů, které připojovat hello řadič toohello EBOD řadiče.
5. Postupujte podle kroků hello v [odebrání řadiče](#remove-a-controller) tooremove hello se nezdařilo řadiče. 
6. Instalace hello factory náhrada v hello stejné pozici z řadič se nezdařilo, který hello byla odebrána. Tím se spustí hello jediného kontroleru nahrazení logiku. Další informace najdete v tématu [jednotné logiku nahrazení řadiče](#single-controller-replacement-logic).
7. Při hello jediného kontroleru nahrazení logiku hodnoty hello pozadí, znovu se připojte kabely hello. Trvat tooconnect pozor, které všechny kabely hello přesně hello stejným způsobem, aby byly připojené před hello nahrazení.
8. Po restartování řadiče hello zkontrolujte hello **stav řadiče** a hello **clusteru stav** v hello Azure classic tooverify portálu, který hello řadiče back tooa stavu v pořádku a je v pohotovostním režimu .

> [!NOTE]
> Pokud sledujete hello zařízení prostřednictvím konzoly sériového portu hello, může se zobrazit více restartování při hello řadiče se obnovení z procedury nahrazení hello. Když se hello nabídce konzoly sériového portu, pak víte, nahrazení hello je dokončena. Pokud hello nabídka se nezobrazí v rámci dvou hodin od hello řadiče nahrazení, [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).
>
> Spuštění aktualizace 4, můžete také použít rutinu hello `Get-HCSControllerReplacementStatus` v rozhraní Windows PowerShell hello hello zařízení toomonitor hello stavu procesu nahrazení řadiče hello.
> 

## <a name="replace-both-controllers"></a>Nahraďte oba řadiče
Pokud oba řadiče zařízení Microsoft Azure StorSimple hello se nezdařilo, jsou nepracuje správně nebo chybí, je potřeba tooreplace oba řadiče. 

### <a name="dual-controller-replacement-logic"></a>Dva řadiče nahrazení logiky
V náhradní duální řadiče nejprve odeberte oba selhání řadiče a potom vložte náhrady. Při hello dva nahrazení řadiče jsou vloženy, hello provedou tyto akce:

1. Nahrazení řadiče Hello do přihrádky 0 zkontroluje hello následující:
   
   1. Ji používá aktuální verze hello firmwaru a softwaru?
   2. Je součástí clusteru hello?
   3. Je řadič sdílené hello systémem a je v clusteru?
      
      Pokud žádná z těchto podmínek jsou splněny, hello řadiče hledá hello nejnovější denní zálohování (umístěné v hello **nonDOMstorage** na jednotce S). Hello řadič zkopíruje ze zálohy hello hello nejnovější snímek hello virtuálního pevného disku.
2. Řadič Hello do přihrádky 0 používá tooimage hello snímku, sám sebe.
3. Mezitím hello řadiče do přihrádky 1 čeká řadič 0 toocomplete hello vytvoření bitové kopie a spusťte.
4. Po spuštění řadič 0, zjistí řadič 1 hello clusteru vytvořený řadič 0, který aktivuje hello jediného kontroleru nahrazení logiku. Další informace najdete v tématu [jednotné logiku nahrazení řadiče](#single-controller-replacement-logic).
5. Později bude spuštěna oba řadiče a hello clusteru přejde do režimu online.

> [!IMPORTANT]
> Následující dva řadiče náhradní po dokončení konfigurace zařízení StorSimple hello, je nutné podniknout ruční zálohování hello zařízení. Denní zálohy konfigurace zařízení nejsou spustí až po uplynutí 24 hodin. Práce s [Microsoft Support](storsimple-contact-microsoft-support.md) toomake ručního zálohování vašeho zařízení.
> 
> 

### <a name="dual-controller-replacement-steps"></a>Dva řadiče kroky nahrazení
Tento pracovní postup je potřeba při současné hello řadičů v zařízení s Microsoft Azure StorSimple se nezdařilo. Toto může nastat v datacentru, ve kterém hello chladicí systém přestane fungovat, a v důsledku toho oba řadiče hello selhání během krátké doby. V závislosti na tom, jestli zařízení StorSimple hello je vypnutý nebo v a zda používáte 8600 nebo model 8100 jinou sadu kroků se vyžaduje.

> [!IMPORTANT]
> Může trvat hodinu too1 45 minut pro hello řadiče toorestart a zcela obnovit z procedury nahrazení duální řadiče. Hello celkovou dobu pro celý postup hello, včetně připojení hello kabely, je přibližně 2,5 hodin.
> 
> 

#### <a name="tooreplace-both-controller-modules"></a>tooreplace oba řadiče moduly
1. Pokud zařízení hello je vypnutý, tento krok přeskočit a pokračovat dalším krokem toohello. Pokud je zapnutá hello zařízení, vypněte hello zařízení.
   
   1. Pokud používáte 8600 model, nejprve vypnout primární skříň hello a pak vypněte hello EBOD skříň.
   2. Počkejte, dokud hello zařízení má úplně vypnout. Všechny hello LED v hello zadní hello zařízení bude vypnuto.
2. Odeberte všechny síťové kabely hello, které jsou připojené toohello data porty. Pokud používáte 8600 model, také odeberte hello SAS kabelů, které připojovat hello primární skříň toohello EBOD skříň.
3. Odebrání zařízení StorSimple hello oba řadiče. Další informace najdete v tématu [odebrání řadiče](#remove-a-controller).
4. Nejprve vložte hello nahrazení objektu pro vytváření pro řadič 0 a potom vložte řadič 1. Další informace najdete v tématu [vložit řadič](#insert-a-controller). Tím se spustí hello duální řadiče nahrazení logiku. Další informace najdete v tématu [duální řadiče nahrazení logiku](#dual-controller-replacement-logic).
5. Při hello řadiče nahrazení logiku hodnoty hello pozadí, znovu se připojte kabely hello. Trvat tooconnect pozor, které všechny kabely hello přesně hello stejným způsobem, aby byly připojené před hello nahrazení. V tématu hello podrobné pokyny pro váš model v hello kabel vaše zařízení části [instalace zařízení StorSimple 8100](storsimple-8100-hardware-installation.md) nebo [instalace zařízení StorSimple 8600](storsimple-8600-hardware-installation.md).
6. Zapněte zařízení StorSimple hello. Pokud používáte 8600 model:
   
   1. Ujistěte se, že tento hello EBOD skříň zapnutý první.
   2. Počkejte, dokud běží hello EBOD skříň.
   3. Zapněte primární skříň hello.
   4. Po první řadič hello restartuje a je ve stavu v pořádku, bude spuštěna hello systému.
      
      > [!NOTE]
      > Pokud sledujete hello zařízení prostřednictvím konzoly sériového portu hello, může se zobrazit více restartování při hello řadiče se obnovení z procedury nahrazení hello. Jakmile se zobrazí hello nabídce konzoly sériového portu, pak víte, nahrazení hello je dokončena. Pokud hello nabídka se nezobrazí v rámci 2,5 hodin od hello řadiče nahrazení, [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).
      > 
      > 

## <a name="remove-a-controller"></a>Odebrání zařízení
Použijte následující postup tooremove modul vadný řadič ze zařízení StorSimple hello.

> [!NOTE]
> Následující ilustrace Hello jsou pro řadič 0. Pro řadič 1 ty by vrátit zpět.
> 
> 

#### <a name="tooremove-a-controller-module"></a>tooremove modul řadiče
1. Pochopit hello modulu západky mezi Flash a ukazováčkem.
2. Jemně vměstnat západky řadiče hello vaší Flash a ukazováčkem toorelease společně.
   
    ![Uvolnění západky řadiče](./media/storsimple-controller-replacement/IC741047.png)
   
    **Obrázek 2** uvolnění řadiče západky
3. Použijte hello západky jako řadič popisovač tooslide hello mimo hello skříň.
   
    ![Klouzavé řadiče mimo skříň](./media/storsimple-controller-replacement/IC741048.png)
   
    **Obrázek 3** posuvné hello řadiče mimo hello skříň

## <a name="insert-a-controller"></a>Vložit řadič
Použijte následující postup tooinstall modul zadaný objekt factory kontroleru po odebrání modul vadný z zařízení StorSimple hello.

#### <a name="tooinstall-a-controller-module"></a>tooinstall modul řadiče
1. Toosee zaškrtněte, pokud neexistuje žádné poškození toohello rozhraní konektorů. Všechny kódy PIN konektor hello poškození nebo ohnuty neinstalujte hello modulu.
2. Modul řadiče hello snímku do skříně hello při hello západky plně vydání. 
   
    ![Klouzavé řadiče do skříně](./media/storsimple-controller-replacement/IC741053.png)
   
    **Obrázek 4** posuvné řadiče do skříně hello
3. Na hello byl vložen modul řadiče začněte zavření západky hello při zpracování pokračuje toopush hello řadiče modulu do skříně hello. Hello západky bude tooguide hello řadiče zapojení do místní.
   
    ![Zavření západky řadiče](./media/storsimple-controller-replacement/IC741054.png)
   
    **Obrázek 5** zavření západky řadiče hello
4. Jste hotovi, když přichytí západky hello. Hello **OK** DIODU by teď měly být na.  
   
   > [!NOTE]
   > Může to trvat až minut too5 hello řadiče a hello DIODU tooactivate.
   > 
   > 
5. tooverify, který nahrazení hello je úspěšné, v hello Azure classic přejděte příliš**zařízení** > **údržby** > **stavu hardwaru**a ujistěte se, že jsou v pořádku řadič 0 a řadič 1 (stav je zelená).

## <a name="identify-hello-active-controller-on-your-device"></a>Identifikovat řadič active hello na zařízení
Je v mnoha situacích, jako je registrace nebo řadič nahrazení prvního zařízení, které od vás vyžadují toolocate hello aktivní řadiče zařízení StorSimple. Řadič active Hello zpracovává všechny hello disku firmwaru a síťové operace. Můžete použít některou z hello následující metody tooidentify hello aktivního řadiče:

* [Použití hello Azure classic portálu tooidentify hello aktivní řadiče](#use-the-azure-classic-portal-to-identify-the-active-controller)
* [Pomocí prostředí Windows PowerShell pro řadič služby StorSimple tooidentify hello active](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)
* [Zkontrolujte hello fyzického zařízení tooidentify hello aktivního řadiče](#check-the-physical-device-to-identify-the-active-controller)

Každý z těchto postupů je popsána dále.

### <a name="use-hello-azure-classic-portal-tooidentify-hello-active-controller"></a>Použití hello Azure classic portálu tooidentify hello aktivní řadiče
V hello portál Azure classic, přejděte příliš**zařízení** > **údržby**a posuňte se toohello **řadiče** části. Zde můžete ověřit, řadič, který je aktivní.

![Vyhledejte active řadič v portálu Azure classic](./media/storsimple-controller-replacement/IC752072.png)

**Obrázek 6** řadič active hello Azure znázorňující portálu classic

### <a name="use-windows-powershell-for-storsimple-tooidentify-hello-active-controller"></a>Pomocí prostředí Windows PowerShell pro řadič služby StorSimple tooidentify hello active
Při přístupu k zařízení prostřednictvím konzoly sériového portu hello, se zobrazí zpráva hlavičky. zpráva hlavičky Hello obsahuje informace o základní zařízení, jako je například hello model, název, verzi nainstalovaného softwaru a stav hello řadič, ke které přistupujete. Hello následující obrázek ukazuje příklad zpráva hlavičky:

![Zpráva sériové hlavičky](./media/storsimple-controller-replacement/IC741098.png)

**Obrázek 7** Banner zpráva zobrazující řadič 0 jako aktivní

Hello banner zpráva toodetermine můžete použít, ať už jste hello řadiče připojené toois aktivní nebo pasivní.

### <a name="check-hello-physical-device-tooidentify-hello-active-controller"></a>Zkontrolujte hello fyzického zařízení tooidentify hello aktivního řadiče
Řadič active tooidentify hello na zařízení, vyhledejte hello blue DIODU výše na hello zadní primární skříň hello port hello DATA 5.

Pokud tento indikátor LED bliká, hello řadiče je aktivní a hello jiný řadič je v pohotovostním režimu. Použijte následující diagram hello a tabulky jako pomocný.

![Propojovací rozhraní systému primární skříň zařízení s datové porty](./media/storsimple-controller-replacement/IC741055.png)

**Obrázek 8** zadní primární skříň s porty dat a monitorování LED

| Štítek | Popis |
|:--- |:--- |
| 1-6 |DATA 0 – 5 síťové porty |
| 7 |Modré DIODU |

## <a name="next-steps"></a>Další kroky
Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).

