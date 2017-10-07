---
title: "aaaStorSimple 8000 řady hardwarové součásti a stav | Microsoft Docs"
description: "Zjistěte, jak toomonitor hello hardwarové součásti zařízení StorSimple pomocí služby StorSimple Manager zařízení hello."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: alkohli
ms.openlocfilehash: 85b398e4b1a6b8921792b8945331325940082eb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomonitor-hardware-components-and-status"></a>Použití hello Správce zařízení StorSimple služby toomonitor hardwarové součásti a stav
## <a name="overview"></a>Přehled
Tento článek popisuje hello různé fyzické a logické součásti v zařízení řady StorSimple 8000 místně. Taky se dozvíte, jak toomonitor hello stav součásti zařízení pomocí hello **stav a hardwaru stavu** okno v hello služby StorSimple Manager zařízení.

Hello **stav a hardwaru stavu** okno zobrazuje stav hardwaru hello všechny součásti zařízení StorSimple hello.

V části hello seznam komponent pro 8100 existují tři části, které popisují:

* **Sdílené součásti** – tyto nejsou součástí hello řadiče, například diskové jednotky, skříň, PCM součásti a teploty PCM řádek napětí a řádek aktuální senzorů.
* **Součásti řadič 0** – hello součásti, které jsou umístěny na řadič 0, jako je třeba řadič, SAS expander a konektor, senzory teploty řadiče a hello různých síťových rozhraní.
* **Součásti řadiči 1** – hello součásti, které tvoří řadič 1, podobně jako toothose podrobné pro řadič 0.

Zařízení s 8600 má další součásti, které odpovídají toohello skříň rozšířené Bunch disky (EBOD). V části hello seznam součástí jsou pět oddíly. Z těchto existují tři části, které obsahují hello součásti v primární skříň hello a jsou identické toohello ty, které jsou popsané pro 8100. Existují dva další oddíly pro hello EBOD skříň, které popisují:

* **Součásti EBOD řadič 0** – hello součásti, které jsou umístěné na EBOD skříň 0, jako je například hello EBOD řadič, senzory teploty expander a konektor a řadič SAS.
* **EBOD řadič 1 součásti** – hello součásti, které tvoří EBOD skříň 1, podobně jako toothose podrobné pro EBOD skříň 0.
* **Sdílené součásti skříň EBOD** – hello součásti nacházející se v hello EBOD skříň a PCM, které nejsou součástí hello EBOD řadiče.

> [!NOTE]
> **stav hardwarových Hello není k dispozici pro zařízení StorSimple cloudu (8010/8020).**


## <a name="monitor-hello-hardware-status"></a>Monitorování stavu hardwaru hello
Proveďte následující kroky tooview hello stav hardwarových zařízení součásti hello:

1. Přejděte příliš**zařízení**, vyberte konkrétní zařízení StorSimple. Přejděte příliš**sledování > Stav hardwaru**.

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health1.png)

2. Vyhledejte hello **hardwarové součásti** části a pak vyberte z dostupných komponentách hello. Jednoduše klikněte na hello součást popisek tooexpand hello seznamu a zobrazit stav hello hello různé součásti zařízení. V tématu hello [seznam podrobné komponent pro primární skříň hello](#component-list-for-primary-enclosure-of-storsimple-device) a hello [seznam podrobné komponent pro hello EBOD skříň](#component-list-for-ebod-enclosure-of-storsimple-device).

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health2.png)

3. Použijte následující stav součásti hello barva schéma toointerpret kódování hello:
   
   * **Zelená kontrola** – označuje pořádku součást s **OK** stavu.
   * **Žlutý** – označuje sníženou součástí **upozornění** stavu.
   * **Červený vykřičník** – Denotes selhání komponenty, která má **selhání** stavu.
   * **Bílé černým textem** – označuje komponenty, která není k dispozici.
   
   Hello následující snímek obrazovky ukazuje zařízení, která obsahuje součásti v **OK**, **upozornění**, a **selhání** stavu.
       
   ![](./media/storsimple-8000-monitor-hardware-status/hw-health3.png)

   Se zvětšující hello **sdílené součásti seznamu**, uvidíme hello paměti NVRAM a hello clusteru jsou degradovány.

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health5.png)

   Se zvětšující hello **řadič 1 součásti** seznamu, uvidíte hello uzlu clusteru se nezdařilo.  

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health4.png)  

4. Pokud narazíte na komponenty, která není součástí **stavu v pořádku** stavu, obraťte se na Microsoft Support. Pokud výstrahy jsou povolené na vašem zařízení, obdržíte e-mailové výstrahy. Pokud potřebujete tooreplace komponentu selhání hardwaru, přečtěte si [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).

## <a name="component-list-for-primary-enclosure-of-storsimple-device"></a>Seznam komponent pro primární skříň zařízení StorSimple
Hello následující tabulka obsahuje přehled hello fyzické a logické součásti, které jsou obsažené v hello primární skříň (existuje v 8100 i 8600) z vašeho místního zařízení StorSimple.

| Komponenta | Modul | Typ | Umístění | Pole replaceable jednotka (FRU)? | Popis |
| --- | --- | --- | --- | --- | --- |
| Jednotka ve slotu [0-11] |Diskové jednotky |Fyzické |Shared |Ano |Zobrazí jeden řádek pro každou hello SSD nebo hello HDD disky v primární skříň hello. |
| Senzor teploty vedlejším |Skříň |Fyzické |Shared |Ne |Míry hello teploty v rámci hello skříň. |
| Senzor teploty střední roviny |Skříň |Fyzické |Shared |Ne |Míry hello teplota rovinu střední hello. |
| Zvukové poplašné |Skříň |Fyzické |Shared |Ne |Určuje, zda je podsystém zvukové poplašné hello v rámci hello skříň funkční. |
| Skříň |Skříň |Fyzické |Shared |Ano |Označuje hello přítomnost skříň. |
| Nastavení skříň |Skříň |Fyzické |Shared |Ne |Odkazuje toohello přední panel hello skříní. |
| Řádek napěťovými senzory |PCM |Fyzické |Shared |Ne |Řada řádku napěťovými senzory mít jejich stavu zobrazí, které označuje, zda text hello měří napětí je v mezích tolerance. |
| Aktuální senzorů řádku |PCM |Fyzické |Shared |Ne |Řada senzorů aktuálního řádku mít jejich stavu zobrazí, která určuje, zda hello měřená aktuální v toleranci. |
| Senzory teploty v PCM |PCM |Fyzické |Shared |Ne |Řada senzory teploty, jako je vstupu a hotspotů senzorů mít jejich stavu zobrazí, která určuje, zda text hello měří teploty je v mezích tolerance. |
| Zdroj napájení [0-1] |PCM |Fyzické |Shared |Ano |Jeden řádek zobrazí pro jednotlivé zdroje napájení hello v hello dvě PCMs umístěný v hello zadní hello zařízení. |
| Chladicí [0-1] |PCM |Fyzické |Shared |Ano |Pro každou hello čtyři chladicí ventilátory, které se nacházejí v hello dvě PCMs se zobrazí jeden řádek. |
| Baterie [0-1] |PCM |Fyzické |Shared |Ano |Zobrazí jeden řádek pro každou hello zálohování baterie modulů, které jsou v hello PCM nainstalován. |
| Metis |Není k dispozici |Logické |Shared |Není k dispozici |Zobrazuje stav baterie hello hello: jestli potřebují poplatků a se blíží ukončenou životností. |
| Cluster |Není k dispozici |Logické |Shared |Není k dispozici |Zobrazí hello stav hello clusteru, který se vytvoří mezi dvěma moduly integrovaného řadiče hello. |
| Uzel clusteru |Není k dispozici |Logické |Shared |Není k dispozici |Označuje stav hello hello řadiče jako součást clusteru hello. |
| Kvorum clusteru |Není k dispozici |Logické | |Není k dispozici |Označuje hello přítomnost hello většina disku členství ve skupině hello HDD fondu úložiště. |
| Datový prostor pevný disk |Není k dispozici |Logické |Shared |Není k dispozici |Hello prostor úložiště, který se používá pro data do fondu úložiště hello jednotku pevného disku (HDD). |
| Správy místa na HDD |Není k dispozici |Logické |Shared |Není k dispozici |místo Hello vyhrazené v hello HDD fondu úložiště pro úlohy správy. |
| Místo na pevný disk kvora |Není k dispozici |Logické |Shared |Není k dispozici |místo Hello vyhrazené v hello HDD fondu úložiště pro kvorum clusteru. |
| Nahrazení místa na HDD |Není k dispozici |Logické |Shared |Není k dispozici |místo Hello vyhrazené v hello HDD fondu úložiště pro nahrazení řadiče. |
| Datový prostor SSD |Není k dispozici |Logické |Shared |Není k dispozici |Hello prostor úložiště pro data v jednotce SSD hello používá fond úložiště (SSD). |
| Místo paměti NVRAM SSD |Není k dispozici |Logické |Shared |Není k dispozici |Hello prostoru úložiště v hello fondu úložiště SSD, který je vyhrazen pro logiku paměti NVRAM. |
| Pevný disk fondu úložiště |Není k dispozici |Logické |Shared |Není k dispozici |Zobrazí hello stavu hello logické úložiště fondu, který je vytvořený z zařízení pevné disky. |
| Fond úložiště SSD |Není k dispozici |Logické |Shared |Není k dispozici |Zobrazí hello stavu hello logické úložiště fondu, který je vytvořený z zařízení SSD. |
| Řadič [0-1] [stavu] |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ano |Zobrazí stav hello hello řadiče, a zda je v režimu aktivní nebo pohotovostní v rámci hello skříň. |
| Senzory teploty v kontroleru |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Řada senzory teploty, například modul vstupně-výstupních operací, teploty procesoru, DIMM a PCIe senzorů mít jejich stavu zobrazí, který označuje, zda je hello teploty došlo v toleranci. |
| SAS expander |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello hello sériové připojené SCSI (SAS) expander, což je řadič toohello integrované úložiště používané tooconnect hello. |
| Konektor SAS [0-1] |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello každý konektor SAS, což je použité tooconnect integrované úložiště toohello SAS expander. |
| SBB střední roviny propojení |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello hello střední roviny konektor, který je použité tooconnect každé řadiče toohello střední plochy. |
| Jádra procesoru |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello hello jader procesoru v rámci každého řadiče. |
| Skříň electronics napájení |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello systému power hello používá hello skříň. |
| Skříň electronics diagnostiky |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello subsystémy diagnostiky hello poskytované hello řadiče. |
| Řadič pro správu základní desky (BMC) |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello hello řadič správy základní desky (BMC), což je procesor specializované služby, který monitoruje hello hardwarové zařízení prostřednictvím senzory a komunikuje s hello správce systému prostřednictvím nezávislé připojení. |
| Ethernet |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello jednotlivých hello síťových rozhraní, tedy hello správy a porty data k dispozici na hello řadiče. |
| PAMĚTI NVRAM |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello paměti NVRAM, paměť s náhodným přístupem stálé zálohovaná hello baterie, která má tooretain aplikace důležitých informací ve hello události výpadku napájení. |

## <a name="component-list-for-ebod-enclosure-of-storsimple-device"></a>Seznam komponent pro EBOD skříň zařízení StorSimple
Hello následující tabulka obsahuje přehled hello fyzické a logické součásti, které jsou obsažené v hello EBOD skříň (dostupná pouze v modelu 8600) vašeho místního zařízení StorSimple.

| Komponenta | Modul | Typ | Umístění | FRU? | Popis |
| --- | --- | --- | --- | --- | --- |
| Jednotka ve slotu [0-11] |Diskové jednotky |Fyzické |Shared |Ano |Pro každou hello HDD disky v popředí hello hello EBOD skříň zobrazí jeden řádek. |
| Senzor teploty vedlejším |Skříň |Fyzické |Shared |Ne |Míry hello teploty v rámci hello skříň. |
| Senzor teploty střední roviny |Skříň |Fyzické |Shared |Ne |Míry hello teplota rovinu střední hello. |
| Zvukové poplašné |Skříň |Fyzické |Shared |Ne |Určuje, zda je podsystém zvukové poplašné hello v rámci hello skříň funkční. |
| Skříň |Skříň |Fyzické |Shared |Ano |Označuje hello přítomnost skříň. |
| Nastavení skříň |Skříň |Fyzické |Shared |Ne |Odkazuje toohello OPS nebo hello přední panel hello skříní. |
| Řádek napěťovými senzory |PCM |Fyzické |Shared |Ne |Řada řádku napěťovými senzory mít jejich stavu zobrazí, které označuje, zda text hello měří napětí je v mezích tolerance. |
| Aktuální senzorů řádku |PCM |Fyzické |Shared |Ne |Řada senzorů aktuálního řádku mít jejich stavu zobrazí, která určuje, zda hello měřená aktuální v toleranci. |
| Senzory teploty v PCM |PCM |Fyzické |Shared |Ne |Řada senzory teploty, jako je vstupu a hotspotů senzorů mít jejich stavu zobrazí, který označuje, zda text hello měří teploty je v mezích tolerance. |
| Zdroj napájení [0-1] |PCM |Fyzické |Shared |Ano |Jeden řádek zobrazí pro jednotlivé zdroje napájení hello v hello dvě PCMs umístěný v hello zadní hello zařízení. |
| Chladicí [0-1] |PCM |Fyzické |Shared |Ano |Pro každou hello čtyři chladicí ventilátory, které se nacházejí v hello dvě PCMs se zobrazí jeden řádek. |
| Místní úložiště [HDD] |Není k dispozici |Logické |Shared |Není k dispozici |Zobrazí hello stavu hello logické úložiště fondu, který je vytvořený z zařízení pevné disky. |
| Řadič [0-1] [stavu] |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ano |Zobrazí hello stavu hello řadičů v modulu EBOD hello. |
| Senzory teploty v EBOD |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Řada senzory teploty z každého řadiče mít jejich stavu zobrazí, který určuje, zda je hello teploty došlo v rámci tolerance. |
| SAS expander |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello hello SAS expander, což je řadič toohello integrované úložiště používané tooconnect hello. |
| Konektor SAS [0-2] |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello každý konektor SAS, což je použité tooconnect integrované úložiště toohello SAS expander. |
| SBB střední roviny propojení |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello hello střední roviny konektor, který je použité tooconnect každé řadiče toohello střední plochy. |
| Skříň electronics napájení |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello systému power hello používá hello skříň. |
| Skříň electronics diagnostiky |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello subsystémy diagnostiky hello poskytované hello řadiče. |
| Řadič toodevice připojení |VSTUPNĚ-VÝSTUPNÍCH OPERACÍ |Fyzické |Řadiče |Ne |Označuje stav hello hello připojení mezi hello EBOD vstupně-výstupních operací modulu a řadiče zařízení hello. |

## <a name="next-steps"></a>Další kroky
* toouse hello tooadminister service Manager zařízení StorSimple zařízení, přejděte příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).
* Pokud potřebujete tootroubleshoot komponenty zařízení, která je degradovaný nebo ve stavu, podívejte se příliš[StorSimple monitorování indikátory](storsimple-monitoring-indicators.md).
* tooreplace komponentu selhání hardwaru najdete v části [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).
* Pokud budete pokračovat tooexperience zařízení problémy, [kontaktovat Microsoft Support](storsimple-8000-contact-microsoft-support.md).

