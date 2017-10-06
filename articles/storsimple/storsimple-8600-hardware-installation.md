---
title: "zařízení aaaInstall Microsoft Azure StorSimple 8600 | Microsoft Docs"
description: "Popisuje, jak toounpack, namontujte do racku a zapojení kabeláže zařízení StorSimple 8600 před nasazením a nakonfigurujte hello software."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3d82ba5f-3e34-40dc-9c33-50f952bc6be8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/24/2016
ms.author: alkohli
ms.openlocfilehash: 0fc0ddf076725fededdde33a260b950b72edc8db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Zařízení vybalit, namontovat do racku a zapojení kabeláže zařízení StorSimple 8600
## <a name="overview"></a>Přehled
Vaše Microsoft Azure StorSimple 8600 duální skříň zařízení a skládá se z primárního serveru a EBOD skříň. Tento kurz popisuje, jak toounpack, namontovat do racku a zapojit jeho kabeláž hello hardwaru zařízení StorSimple 8600, než začnete konfigurovat hello StorSimple softwaru.

## <a name="unpack-your-storsimple-8600-device"></a>Rozbalte zařízení StorSimple 8600
Hello následující kroky obsahují vymazat, podrobné pokyny, jak toounpack zařízení StorSimple 8600 úložiště. Toto zařízení je poskytuje dvě pole, jednu pro primární skříň hello a druhý pro hello EBOD skříň. Tyto dva seznamy jsou pak umístěny v jediné pole.

### <a name="prepare-toounpack-your-device"></a>Příprava toounpack zařízení
Než můžete zařízení vybalit zařízení, projděte si hello následující informace.

![Ikona upozornění](./media/storsimple-safety/IC740879.png)![velkou váhy ikonu](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **upozornění!**

1. Ujistěte se, abyste měli k dispozici toomanage osoby dvě hello váhu hello zařízení, pokud jsou její zpracování ručně. Kompletně nakonfigurovaný skříň můžete naváží až too32 kg (70 kg.).
2. Umístěte pole hello na povrchu plochý, úrovně.

V dalším kroku dokončete následující kroky toounpack hello zařízení.

#### <a name="toounpack-your-device"></a>toounpack zařízení
1. Zkontrolujte pole hello a hello balení pěnou pro crushes kusy, horních poškození nebo jiných zřejmé poškození. Pokud políčko hello nebo balení vážně poškozen, neotevírejte hello pole. Prosím [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) toohelp můžete vyhodnotit, jestli je zařízení hello v dobrém stavu.
2. Otevřete hello vnější pole a pak vyjměte hello dvě pole odpovídající tooprimary a EBOD skříně. Můžete teď rozbalte hello primární a EBOD skříně. Hello následující obrázek znázorňuje zobrazení hello vybaleno jednoho hello skříně.
   
    ![Rozbalte zařízení úložiště](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)
   
    **Rozbalené zobrazení vašeho zařízení úložiště**
   
   | Štítek | Popis |
   | --- | --- |
   |   1 |Okolních pole |
   |   2 |SAS kabely (v Příslušenství a kabely panelu) |
   |   3 |Dolní pěnou |
   |   4 |Zařízení |
   |   5 |Horní pěnou |
   |   6 |Příslušenství pole |
3. Po rozbalení hello dvě pole, ujistěte se, zda máte:
   
   * 1 primární skříň (hello primární skříně a EBOD skříň jsou dvě samostatné polí)
   * 1 EBOD skříň
   * 4 napájecích kabelů, 2 v každém poli
   * 2 SAS kabely (tooconnect hello primární skříň tooEBOD skříň)
   * 1 kříženého kabelu Ethernet
   * 2 kabely konzoly sériového portu
   * 1 sériové USB převaděč pro sériový přístup
   * 4 QSFP-na-SFP + adaptéry pro použití s 10 GbE síťová rozhraní
   * 2 rack sadách připojení (4 které straně s připojení hardwaru, 2 pro primární skříň hello a EBOD skříň), 1 v každém poli
   * Získávání dokumentaci Začínáme
     
     Pokud jste neobdrželi žádné položky hello uvedený výše, [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).  

dalším krokem Hello je toorack připojení zařízení.

## <a name="rack-mount-your-storsimple-8600-device"></a>Zařízení StorSimple 8600 rack připojení
Postupujte podle hello další kroky tooinstall zařízení StorSimple 8600 úložiště v standardní stojanu 19 palců s přední a zadní příspěvky. Toto zařízení se dodává s dvěma skříně: primární skříně a EBOD skříň. Obě tyto potřebovat toobe skříňová.

instalace Hello se skládá z více kroků, z nichž každý je podrobněji hello následující postupy.

> [!IMPORTANT]
> Zařízení StorSimple musí být skříňová pro správný provoz.
> 
> 

### <a name="site-preparation"></a>Příprava lokality
skříně Hello musí být nainstalovány ve standardní rack 19 palců, který má přední a zadní příspěvky. Použijte následující postup tooprepare pro instalaci rack hello.

#### <a name="tooprepare-hello-site-for-rack-installation"></a>tooprepare hello lokality pro instalaci do racku
1. Ujistěte se, že hello primární a skříně EBOD jsou bezpečně na ploché, stabilní a úrovně pracovní prostor řady (nebo podobnou).
2. Ověřte této hello lokality, kde chcete tooset až má standardní AC napájení z nezávislých zdroje nebo racková skříň jednotky distribuční napájení (PDU) s nepřerušitelný zdroj napájení (UPS).
3. Zkontrolujte, zda že je k dispozici na hello rack, ve kterém chcete toomount hello skříně patice jeden 4U (2 u 2 X).

![Ikona upozornění](./media/storsimple-safety/IC740879.png)![velkou váhy ikonu](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **upozornění!**

 Ujistěte se, abyste měli dva osoby k dispozici toomanage hello váhy, pokud jsou zpracování nastavení zařízení hello ručně. Kompletně nakonfigurovaný skříň můžete naváží až too32 kg (70 kg.).

### <a name="rack-prerequisites"></a>Rack požadavky
Hello skříních jsou určené pro instalaci v CAB s standardní 19 palců stojanu:

* Minimální hloubka 27.84 palce z rack post toopost
* Maximální váhu 32 kg hello zařízení
* Maximální zpětný tlak 5 Pascal (0,5 mm horních měřidla)

### <a name="rack-mounting-rail-kit"></a>Připojování rack liště kit
Sadu připojení, které bude poskytnuta pro použití s hello 19 palců rack soubor CAB. Dobrý den, které byly testovány toohandle hello maximální skříň váhy. Tyto které také umožní instalaci více skříní bez ztráty prostor v rámci racku hello. Nejprve nainstalujte hello EBOD skříň.

#### <a name="tooinstall-hello-ebod-enclosure-on-hello-rails"></a>hello tooinstall EBOD skříň, na které hello
1. Tento krok proveďte jenom v případě, že vnitřní které nejsou nainstalovány na vašem zařízení. Obvykle hello vnitřní které jsou nainstalovány na objekt pro vytváření hello. Pokud nejsou nainstalovány které, nainstalujte hello liště levé a pravé liště snímky toohello stranách hello skříň skříní. Jsou připojit pomocí šesti metriky šrouby na každé straně. toohelp s orientaci hello liště snímky jsou označeny **LH – přední** a **RH – přední**, a hello end, který je opatřen směrem hello zadní části hello skříň má vřetenovitým end.
   
    ![Připojení liště snímky tooenclosure skříň](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
   
    **Připojení liště snímky toohello stranách hello skříň**
   
   | Štítek | Popis |
   | --- | --- |
   |  1 |M 3 × 4 šrouby tlačítko head |
   |  2 |Skříň snímky |
2. Připojte liště hello levé a pravé liště sestavení toohello rack v CAB svislé členy. jsou označeny Hello závorky **LH**, **RH**, a **této straně se** tooguide vás správnou orientaci.
3. Vyhledejte hello liště PIN v hello přední a zadní hello liště sestavení. Rozšíření hello liště toofit mezi hello rack příspěvky a vložte do hello příspěvek přední a zadní rack svislé člen díry hello PIN. Ujistěte se, že sestavení liště hello je úroveň.
4. Zabezpečte hello liště sestavení toohello rack svislé pomocí dvou metriky šroubů hello za předpokladu. Použijte jeden šroub na popředí hello a jeden na zadní hello.
5. Opakujte tyto kroky u hello jiných liště sestavení.
   
     ![Připojení snímky liště toorack CAB](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Připojení rack toohello liště sestavení**
   
   | Štítek | Popis |
   | --- | --- |
   |   1 |Upínací šroubovacím |
   |   2 |Šroubovacím post front rack hranaté hierarchie |
   |   3 |Levé front liště umístění PIN |
   |   4 |Upínací šroubovacím |
   |   5 |Levé zadní liště umístění PIN |

### <a name="mounting-hello-ebod-enclosure-in-hello-rack"></a>Připojení hello EBOD skříň v racku hello
Pomocí které hello rack, které byly nainstalovány právě, proveďte následující kroky toomount hello EBOD skříň v racku hello hello.

#### <a name="toomount-hello-ebod-enclosure"></a>hello toomount EBOD skříň
1. Asistent navýšení hello skříň a zarovnané s které rack hello.
2. Pečlivě vložit hello skříň, do které hello a pak poslat ho úplně do racku hello CAB.
   
    ![Vkládání zařízení do racku hello](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Připojení hello skříň v racku hello**
3. Odeberte přidáváním hello CAP k vzdálené ploše volné hello v levém horním a pravém předního panelu CAP k vzdálené ploše. Hello příruby CAP k vzdálené ploše snap jednoduše na přírub hello.
4. Zabezpečte hello skříň do racku hello nainstalováním jeden zadaný šroub Phillips head prostřednictvím každý příruby vlevo a vpravo.
5. Stisknutím kláves je do pozice a uchycení je do místní instalaci hello příruby CAP k vzdálené ploše.
   
     ![Instalace příruby CAP k vzdálené ploše](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Instalace hello příruby CAP k vzdálené ploše**
   
   | Štítek | Popis |
   | --- | --- |
   |   1 |Skříň upevnění šroubovacím |

### <a name="mounting-hello-primary-enclosure-in-hello-rack"></a>Připojení hello primární skříň v racku hello
Po dokončení připojení hello EBOD skříň, budete potřebovat toomount hello primární skříň následující hello stejný postup.

> [!NOTE]
> * Je možné toohave několik prázdný sloty v hello rack mezi primární skříň hello a skříň EBOD hello.
> * Použijte hello zadané 2m SAS kabel tooconnect hello primární skříň toohello EBOD skříň.
> * Neexistují žádná omezení hello relativní umístění hello head jednotky toohello EBOD jednotky. Proto primární skříň hello mohou být umístěny v horní slotu hello a skříň EBOD hello níže – nebo naopak.
> 
> 

dalším krokem Hello je toocable zařízení pro napájení, sítě a sériového přístupu.

## <a name="cable-your-storsimple-8600-device"></a>Zapojení kabeláže zařízení StorSimple 8600
Hello následující postupy popisují, jak toocable zařízení StorSimple 8600 pro napájení, sítě a sériové připojení.

### <a name="prerequisites"></a>Požadavky
Než začnete toocable zařízení, budete potřebovat:

* Primární skříň a hello EBOD skříň, zcela vybaleno.
* 4 napájecích kabelů (2 Každý hello primární a hello EBOD skříň), které byly dodány s vaším zařízením
* 2 SAS kabely, které jsou součástí hello zařízení tooconnect hello EBOD skříň toohello primární skříň
* Přístup k too2 jednotek pro distribuci napájení (PDU) (doporučeno)
* Síťové kabely
* Zadat sériové kabely
* Převaděč sériové USB s hello příslušný ovladač nainstalován ve vašem počítači (v případě potřeby)
* Poskytuje 4 QSFP-na-SFP + adaptéry pro použití s 10 GbE síťová rozhraní
* [Podporovaný hardware pro hello 10 GbE síťová rozhraní zařízení StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS a kabeláž napájení
Vaše zařízení má skříni primární i EBOD skříň. To vyžaduje toobe jednotky hello společně zapojena jeho kabeláž připojení Serial Attached (SCSI SAS) a napájení.

Při nastavování tohoto zařízení pro hello poprvé, proveďte kroky hello SAS kabelů nejprve a pak dokončení hello kroky pro kabeláž napájení.

[!INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Síťové kabely
Zařízení je v konfiguraci aktivní pohotovostní: v každém okamžiku je aktivní jeden modul řadiče a zpracování všech diskových a síťových operací při hello jiné řadiče modul je do úsporného režimu. Pokud dojde k selhání řadiče, hello pohotovostní řadiče ihned aktivuje a bude pokračovat všechny operace hello disku a sítě.

toosupport tento redundantní řadiče převzetí služeb při selhání, budete potřebovat toocable zařízení sítě, jak je znázorněno v hello následující kroky.

#### <a name="toocable-for-network-connection"></a>toocable pro síťové připojení
1. Vaše zařízení má šest síťových rozhraní v každém řadiči: čtyři 1 GB/s a dvě 10 GB/s Ethernet porty. Naleznete toohello následující ilustrace tooidentify hello data porty na hello propojovací rozhraní systému vašeho zařízení.
   
     ![Propojovací rozhraní systému 8600 zařízení](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Zpět z vašeho zařízení zobrazující hello porty dat**
   
   | Štítek | Popis |
   | --- | --- |
   |   0,1,4,5 |1 GbE síťová rozhraní |
   |   2,3 |10 GbE síťová rozhraní |
   |   6 |Sériové porty |
2. Viz následující diagram pro síťové kabely hello. (konfigurace minimální sítě hello zobrazené plné modré čáry. Pro vysokou dostupnost a výkon další nezbytné konfigurace je zobrazen linkami tečkovaná.)

![Zapojení kabeláže zařízení 4U pro síť](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Sítě kabelů pro vaše zařízení**

| Štítek | Popis |
| --- | --- |
| A |LAN s přístupem k Internetu |
| B |Řadič 0 |
| C |PCM 0 |
| D |Řadič 1 |
| E |PCM 1 |
| F |EBOD řadič 0 |
| G |EBOD řadiči 1 |
| H, I |Hostitele (například souborové servery) |
| 0-5 |Síťová rozhraní |
| 6 |Primární skříň |
| 7 |EBOD skříň |

Pokud kabelů hello zařízení, vyžaduje minimální konfigurace hello:

* Aspoň dvě rozhraní sítě připojené v každém řadiči s jeden pro přístup do cloudu a jeden pro iSCSI. Hello DATA 0 portu je automaticky povolené a nakonfigurované prostřednictvím konzoly sériového portu hello hello zařízení. Kromě DATA 0 jiný port data také musí toobe nakonfigurovaný pomocí hello portál Azure classic. V takovém případě připojte DATA 0 port toohello primární LAN (síť s přístupem k Internetu). Hello data, která může být porty připojené segment sítě LAN (VLAN) tooSAN nebo iSCSI hello sítě, v závislosti na hello určené role.
* Stejná rozhraní na každý řadič připojené toohello stejné Pokud dojde k selhání řadič sítě tooensure dostupnosti. Například pokud zvolíte tooconnect DATA 0 a DATA 3 pro jednu hello řadičů, budete potřebovat tooconnect hello odpovídající DATA 0 a DATA 3 na hello jiný řadič.

Mějte na paměti pro vysokou dostupnost a výkon:

* Pokud je to možné, nakonfigurujte v každém řadiči pár síťového rozhraní pro přístup do cloudu (1 GbE) a jinou dvojici pro iSCSI (10 GbE doporučené).
* Pokud je to možné, připojte síťová rozhraní z každé tootwo různým přepínačům tooensure dostupnost řadiče proti selhání přepínače. Hello obrázku je znázorněn hello dvě 10 GbE síťová rozhraní, DATA 2 a DATA 3 z každého řadiče připojené tootwo různým přepínačům. Další informace najdete v části toohello **síťových rozhraní** pod hello [požadavků na vysokou dostupnost pro zařízení StorSimple](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> Pokud používáte SFP + vysílače s 10 GbE síťové rozhraní, použijte hello poskytuje QSFP-SFP + adaptéry. Další informace, přejděte příliš[podporovaném hardwaru pro zařízení StorSimple dostupná síťová rozhraní hello 10 GbE](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Kabeláž sériového portu
Proveďte následující kroky toocable hello sériového portu.

#### <a name="toocable-for-serial-connection"></a>toocable pro sériové připojení
1. Vaše zařízení má sériového portu v každém řadiči, která je identifikovaná ikona klíče. sériové porty hello toolocate naleznete toohello obrázku, které se zobrazují data porty hello hello zadní zařízení.
2. Identifikujte řadič active hello na vaše zařízení propojovacího rozhraní. Blikající blue DIODU označuje, že je tento řadič hello aktivní.
3. Použít sériový kabel hello poskytuje (v případě potřeby, hello USB sériové převaděč pro přenosný) a připojit konzolu nebo počítač (s emulaci terminálu toohello zařízení) toohello active řadiče hello sériového portu.
4. Instalace ovladačů sériové USB hello (součástí zařízení hello) ve vašem počítači.
5. Sériové připojení hello nastavíte takto:
   
   * Přenosová 115 200
   * 8 datových bitů
   * 1 stop-bit
   * Žádná parita
   * Nastavit řízení toku příliš**None**
6. Ověřte, zda text hello připojení funguje stisknutím klávesy Enter v konzole hello. By se zobrazit nabídce konzoly sériového portu.

> [!NOTE]
> **Správa lights-Out:** při hello zařízení je nainstalován ve vzdálené datovém centru nebo v počítači místnosti s omezeným přístupem, ujistěte se, že hello sériové připojení tooboth řadiče jsou vždy konzoly sériového portu přepínače připojený tooa nebo podobné zařízení. To umožňuje out-of-band vzdáleného řízení a podporu operací v případě přerušení sítě nebo neočekávaných chyb.
> 
> 

Dokončení kabelů vašeho zařízení z hlediska výkonu, přístup k síti, a sériové connection.hello dalším krokem je tooconfigure hello softwaru na vašem zařízení.

## <a name="next-steps"></a>Další kroky
Nyní jste připraveni příliš[nasaďte a nakonfigurujte místní zařízení StorSimple](storsimple-deployment-walkthrough-u2.md).

