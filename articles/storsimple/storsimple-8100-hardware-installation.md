---
title: "zařízení aaaInstall Microsoft Azure StorSimple 8100 | Microsoft Docs"
description: "Popisuje, jak toounpack, namontujte do racku a zapojení kabeláže zařízení StorSimple 8100 před nasazením a nakonfigurujte hello software."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 6098a01e-c031-488a-a8d7-0b607ce665e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 76b94e57318b6c25dc3333ae73edc9e4752b7d1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Zařízení vybalit, namontovat do racku a zapojení kabeláže zařízení StorSimple 8100
## <a name="overview"></a>Přehled
Vaše Microsoft Azure StorSimple 8100 je jeden skříň, skříňová zařízení. Tento kurz popisuje, jak toounpack, namontovat do racku a zapojit jeho kabeláž hello hardwaru zařízení StorSimple 8100, před konfigurací a nasazení zařízení StorSimple hello.

## <a name="unpack-your-storsimple-8100-device"></a>Rozbalte zařízení StorSimple 8100
Hello následující kroky obsahují vymazat, podrobné pokyny, jak toounpack zařízení StorSimple 8100 úložiště. Toto zařízení je dodáván v jediné pole.

### <a name="prepare-toounpack-your-device"></a>Příprava toounpack zařízení
Než můžete zařízení vybalit zařízení, projděte si hello následující informace.

![Ikona upozornění](./media/storsimple-safety/IC740879.png)![velkou váhy ikonu](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **upozornění!**

1. Ujistěte se, abyste měli k dispozici toomanage osoby dvě hello váhu hello skříň, pokud jsou její zpracování ručně. Kompletně nakonfigurovaný skříň můžete naváží až too32 kg (70 kg.).
2. Umístěte pole hello na povrchu plochý, úrovně.

V dalším kroku dokončete následující kroky toounpack hello zařízení.

#### <a name="toounpack-your-device"></a>toounpack zařízení
1. Zkontrolujte pole hello a hello balení pěnou pro crushes kusy, horních poškození nebo jiných zřejmé poškození. Pokud políčko hello nebo balení vážně poškozen, neotevírejte hello pole. Prosím [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) toohelp můžete vyhodnotit, jestli je zařízení hello v dobrém stavu.
2. Rozbalte hello pole. Hello následující obrázek znázorňuje zobrazení hello vybaleno zařízení StorSimple.
   
     ![Rozbalte zařízení úložiště](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)
   
    **Rozbalené zobrazení vašeho zařízení úložiště**
   
   | Štítek | Popis |
   | --- | --- |
   |   1 |Okolních pole |
   |   2 |Dolní pěnou |
   |   3 |Zařízení |
   |   4 |Horní pěnou |
   |   5 |Příslušenství pole |
3. Po rozbalení hello pole, ujistěte se, zda máte:
   
   * zařízení, která 1 jedné skříni.
   * 2 napájecích kabelů
   * 1 kříženého kabelu Ethernet
   * 2 kabely konzoly sériového portu
   * 1 sériové USB převaděč pro sériový přístup
   * 1 šroubovák T10 manipulací
   * 4 QSFP-na-SFP + adaptéry pro použití s 10 GbE síťová rozhraní
   * 1 rack připojení kit (2 straně které s připojení hardwaru)
   * Získávání dokumentaci Začínáme
     
     Pokud jste neobdrželi žádné položky hello uvedený výše, [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).

dalším krokem Hello je toorack připojení zařízení.

## <a name="rack-mount-your-storsimple-8100-device"></a>Zařízení StorSimple 8100 rack připojení
Postupujte podle hello další kroky tooinstall zařízení StorSimple 8100 úložiště v standardní stojanu 19 palců s přední a zadní příspěvky. zařízení StorSimple 8100 Hello má jeden primární skříň.

instalace Hello se skládá z více kroků, z nichž každý je podrobněji hello následující postupy.

> [!IMPORTANT]
> Zařízení StorSimple musí být skříňová pro správný provoz.
> 
> 

### <a name="prepare-hello-site"></a>Příprava hello lokality
Hello zařízení musí být nainstalovány ve standardní rack 19 palců, který má přední a zadní příspěvky. Použijte následující postup tooprepare pro instalaci rack hello.

#### <a name="tooprepare-hello-site-for-rack-installation"></a>tooprepare hello lokality pro instalaci do racku
1. Ujistěte se, že toto zařízení hello bezpečně postavená na ploché, stabilní a úrovně pracovní plochy (nebo podobnou).
2. Ověřte, zda hello lokality, kde chcete tooset až má standardní napájení z nezávislých zdroje nebo zadat jednotka distribuci napájení (PDU) rack se nepřerušitelný zdroj napájení (UPS).
3. Zajistěte, aby že tento jeden 2 u slotu je k dispozici na hello rack, ve kterém chcete toomount hello zařízení.

![Ikona upozornění](./media/storsimple-safety/IC740879.png)![velkou váhy ikonu](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **upozornění!**

Ujistěte se, abyste měli dva osoby k dispozici toomanage hello váhy, pokud jsou zpracování nastavení zařízení hello ručně. Kompletně nakonfigurovaný skříň můžete naváží až too32 kg (70 kg.).

### <a name="rack-prerequisites"></a>Rack požadavky
Skříň 8100 Hello je navržen tak, že pro instalaci v standardní 19 palců stojanu CAB se:

* Minimální hloubka 27.84 palce z rack post toopost.
* Maximální váhu 32 kg hello zařízení
* Maximální zpětný tlak 5 Pascal (0,5 mm horních měřidla).

### <a name="rack-mounting-rail-kit"></a>Připojování rack liště kit
Sadu připojení, které se poskytuje pro použití s hello 19 palců rack soubor CAB. Dobrý den, které byly testovány toohandle hello maximální skříň váhy. Tyto které také umožní instalaci více skříní beze ztrát prostor v rámci racku hello.

#### <a name="tooinstall-hello-device-on-hello-rails"></a>tooinstall hello zařízení, na které hello
1. Tento krok proveďte jenom v případě, že vnitřní které nejsou nainstalovány na vašem zařízení. Obvykle hello vnitřní které jsou nainstalovány na objekt pro vytváření hello. Pokud nejsou nainstalovány které, nainstalujte hello liště levé a pravé liště snímky toohello stranách hello skříň skříní. Jsou připojit pomocí šesti metriky šrouby na každé straně. toohelp s orientaci hello liště snímky jsou označeny **LH – přední** a **RH – přední**, a hello end, který je opatřen směrem hello zadní části hello skříň má vřetenovitým end.<br/>
   
    ![Připojení liště snímky tooenclosure skříň](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Připojení vnitřní liště snímky toohello stranách hello skříň**
   
    Štítek | Popis
    ----- | -----------
    1     | M 3 × 4 šrouby tlačítko head
    2     | Skříň snímky

2. Připojte hello vnější levém liště a vnější pravé liště sestavení toohello rack v CAB svislé členy. jsou označeny Hello závorky **LH**, **RH**, a **této straně se** tooguide vás hello opravte orientace.
3. Vyhledejte hello liště PIN v hello přední a zadní hello liště sestavení. Rozšíření hello liště toofit mezi hello rack příspěvky a vložte do hello přední a zadní rack post svislé člen díry hello PIN. Ujistěte se, že sestavení liště hello je úroveň.
4. Použijte dva hello poskytuje metriky šrouby toosecure hello liště sestavení toohello rack svislé členů. Použijte jeden šroub na popředí hello a jeden na zadní hello.
5. Opakujte tyto kroky u hello jiných liště sestavení.<br/>
   
     ![Připojení snímky liště toorack CAB](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Připojení rack toohello vnější liště sestavení**
   
   | Štítek | Popis |
   | --- | --- |
   |   1 |Upínací šroubovacím |
   |   2 |Šroubovacím post front rack hranaté hierarchie |
   |   3 |Levé liště front umístění PIN |
   |   4 |Upínací šroubovacím |
   |   5 |Levé liště zadní umístění PIN |

### <a name="mounting-hello-device-in-hello-rack"></a>Připojení zařízení hello v racku hello
Pomocí které hello rack, které byly nainstalovány právě, proveďte následující kroky toomount hello zařízení do racku hello hello.

#### <a name="toomount-hello-device"></a>toomount hello zařízení
1. Asistent navýšení hello skříň a zarovnané s které rack hello.
2. Pečlivě vložit hello zařízení, do které hello a pak push hello zařízení zcela do racku hello CAB.<br/>
   
    ![Vkládání zařízení do racku hello](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Připojení zařízení hello v racku hello**
3. Odeberte přidáváním hello CAP k vzdálené ploše volné hello v levém horním a pravém předního panelu CAP k vzdálené ploše. Hello příruby CAP k vzdálené ploše snap jednoduše na přírub hello.
4. Zabezpečte hello skříň v racku hello nainstalováním jeden zadaný šroub Phillips head prostřednictvím každý příruby vlevo a vpravo.
5. Stisknutím kláves je do pozice a uchycení je na místě instalace hello příruby CAP k vzdálené ploše.<br/>
   
     ![Instalace příruby CAP k vzdálené ploše](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Instalace hello příruby CAP k vzdálené ploše**
   
   | Štítek | Popis |
   | --- | --- |
   |   1 |Skříň upevnění šroubovacím |

dalším krokem Hello je toocable zařízení pro napájení, sítě a sériového přístupu.

## <a name="cable-your-storsimple-8100-device"></a>Zapojení kabeláže zařízení StorSimple 8100
Hello následující postupy popisují, jak toocable zařízení StorSimple 8100 kabeláž napájení, sítě a sériové připojení.

### <a name="prerequisites"></a>Požadavky
Než začnete hello kabelů vašeho zařízení, budete potřebovat:

* Zařízení úložiště, zcela vybaleno a racku.
* 2 napájecích kabelů dodaných s vaším zařízením
* Přístup k too2 jednotek pro distribuci napájení (doporučeno).
* Síťové kabely
* Zadat sériové kabely
* Převaděč sériového portu USB s hello příslušný ovladač nainstalován ve vašem počítači (v případě potřeby)
* Poskytuje 4 QSFP-na-SFP + adaptéry pro použití s 10 GbE síťová rozhraní
* [Podporovaný hardware pro hello 10 GbE síťová rozhraní zařízení StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="power-cabling"></a>Kabeláž napájení
Zařízení obsahuje redundantní napájení a chlazení moduly (PCMs). Jak PCMs musí být nainstalovaný a připojený toodifferent power zdroje tooensure vysokou dostupnost.

Proveďte následující kroky toocable hello zařízení pro napájení.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Síťové kabely
Zařízení je konfigurace aktivní pohotovostní: v každém okamžiku je aktivní jeden modul řadiče a zpracování všech diskových a síťových operací při hello jiné řadiče modul je do úsporného režimu. Pokud řadič selže, řadič pohotovostní hello se ihned aktivuje a bude pokračovat všechny operace hello disku a sítě.

toosupport tento redundantní řadiče převzetí služeb při selhání, budete potřebovat toocable zařízení sítě, jak je popsáno v hello následující kroky.

#### <a name="toocable-for-network-connection"></a>toocable pro síťové připojení
1. Vaše zařízení má šest síťových rozhraní v každém řadiči: čtyři 1 GB/s a dvě 10 GB/s Ethernet porty. Identifikujte hello různých dat porty na hello propojovací rozhraní systému vašeho zařízení.
   
    ![Propojovací rozhraní systému 8100 zařízení](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Zpět z hello zařízení zobrazení dat portů**
   
   | Štítek | Popis |
   | --- | --- |
   |   0,1,4,5 |1 GbE síťová rozhraní |
   |   2,3 |10 GbE síťová rozhraní |
   |   6 |Sériové porty |
2. Viz následující diagram pro síťové kabely hello. (konfigurace minimální sítě hello zobrazené plné modré čáry. Další konfigurace požadované pro vysokou dostupnost a výkon se zobrazí linkami tečkovaná.)

    ![Zapojení kabeláže 2 u zařízení pro síť](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Sítě kabelů pro vaše zařízení**

   |Štítek | Popis |
   |----- | ----------- |
   | A    | LAN s přístupem k Internetu |
   | B    | Řadič 0 |
   | C    | PCM 0 |
   | D    | Řadič 1 |
   | E    | PCM 1 |
   | F, G | Hostitelé |
   | 0-5  | Síťová rozhraní |



Pokud kabelů hello zařízení, vyžaduje minimální konfigurace hello:

* Aspoň dvě rozhraní sítě připojené v každém řadiči s jeden pro přístup do cloudu a jeden pro iSCSI. Hello DATA 0 portu je automaticky povolené a nakonfigurované prostřednictvím konzoly sériového portu hello hello zařízení. Kromě DATA 0 jiný port data také musí toobe nakonfigurovaný pomocí hello portál Azure classic. V takovém případě připojte DATA 0 port toohello primární LAN (síť s přístupem k Internetu). Hello data, která může být porty připojené segment sítě LAN (VLAN) tooSAN nebo iSCSI hello sítě, v závislosti na hello určené role.
* Stejná rozhraní na každý řadič připojené toohello stejné Pokud dojde k selhání řadič sítě tooensure dostupnosti. Například pokud zvolíte tooconnect DATA 0 a DATA 3 pro jednu hello řadičů, budete potřebovat tooconnect hello odpovídající DATA 0 a DATA 3 na hello jiný řadič.

Mějte na paměti pro vysokou dostupnost a výkon:

* Pokud je to možné, nakonfigurujte v každém řadiči pár síťového rozhraní pro přístup do cloudu (1 GbE) a jinou dvojici pro iSCSI (10 GbE doporučené).
* Pokud je to možné, připojte síťová rozhraní z každé tootwo různým přepínačům tooensure dostupnost řadiče proti selhání přepínače. Hello obrázku je znázorněn hello dvě 10 GbE síťová rozhraní, DATA 2 a DATA 3 z každého řadiče připojené tootwo různým přepínačům.

Další informace najdete v části toohello **síťových rozhraní** pod hello [požadavků na vysokou dostupnost pro zařízení StorSimple](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> Pokud používáte SFP + vysílače s 10 GbE síťové rozhraní, použijte hello k dispozici QSFP-SFP + adaptéry. Další informace, přejděte příliš[podporovaném hardwaru pro zařízení StorSimple dostupná síťová rozhraní hello 10 GbE](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Kabeláž sériového portu
Proveďte následující kroky toocable hello sériového portu.

#### <a name="toocable-for-serial-connection"></a>toocable pro sériové připojení
1. Vaše zařízení má sériového portu v každém řadiči, která je identifikovaná ikona klíče. Odkazovat toohello obrázku v hello [síťové kabely](#network-cabling) části toolocate hello sériových portů v hello propojovací rozhraní systému vašeho zařízení.
2. Identifikujte řadič active hello na vaše zařízení propojovacího rozhraní. Blikající blue DIODU označuje, že je tento řadič hello aktivní.
3. Použít sériové kabely hello poskytuje (v případě potřeby, hello USB sériové převaděč pro vašeho přenosného počítače) a připojit konzolu nebo počítač (s emulaci terminálu toohello zařízení) toohello active řadiče hello sériového portu.
4. Instalace ovladačů sériové USB hello (součástí zařízení hello) ve vašem počítači.
5. Sériové připojení hello nastavíte takto: 115 200 přenosová, 8 datových bitů, 1 stop-bit, žádná parita a řízení toku nastavit tooNone.
6. Ověřte, zda text hello připojení funguje stisknutím klávesy Enter v konzole hello. By se zobrazit nabídce konzoly sériového portu.

> [!NOTE]
> **Správa lights-Out**: Pokud hello zařízení je nainstalovaná ve vzdálené datovém centru nebo v počítači místnosti s omezeným přístupem, ujistěte se, že hello sériové připojení tooboth řadiče jsou vždy konzoly sériového portu přepínače připojený tooa nebo podobné zařízení. To umožňuje out-of-band vzdáleného řízení a podporu operací, pokud existují síťová přerušení nebo neočekávaných chyb.
> 
> 

Zařízení je nyní zapojena jeho kabeláž napájení, přístup k síti a sériové připojení. dalším krokem Hello je tooconfigure hello softwaru a nasazení zařízení.

## <a name="next-steps"></a>Další kroky
Zjistěte, jak příliš[nasaďte a nakonfigurujte místní zařízení StorSimple](storsimple-deployment-walkthrough-u2.md).

