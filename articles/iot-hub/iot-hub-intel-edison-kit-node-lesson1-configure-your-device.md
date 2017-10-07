---
title: "Připojit Intel Edison (uzel) tooAzure IoT - lekci 1: Konfigurace zařízení | Microsoft Docs"
description: "Nakonfigurujte Intel Edison pro první použití."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino nastavit připojení arduino toopc, instalační program arduino, arduino panelu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 372c9b6d-e701-4ff6-8151-d262aa76aa55
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c0609542a49924c979f71297d808c09acefca0bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-intel-edison"></a>Konfigurace vaší Edison Intel
## <a name="what-you-will-do"></a>Co provedete
Nakonfigurujte Intel Edison pro první použití sestavte hello Tabule, napájený ji a instalaci nástroje configuration tooyour plochy OS tooflash Edison na firmwaru, nastavte jeho heslo a připojte ho tooWi-Fi. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak tooassemble Edison board a zapněte nahoru.
* Tooflash Edison firmwaru, jak nastavit heslo a připojení Wi-Fi.

## <a name="what-you-need"></a>Co potřebujete
toocomplete tuto operaci je třeba hello následujících částí z vaší Intel Edison Starter Kit:

* Intel® Edison modulu
* Panel Arduino rozšíření
* Jako mezeru mezi řádky nebo šrouby součástí hello balení, včetně dvěma šrouby toofasten hello modulu toohello rozšíření panelu a čtyři sady šrouby a plastové vymezovače.
* Micro B tooType kabelu A USB
* Zdroj napájení přímo aktuální (DC). Zdroj napájení by měl být hodnocení následujícím způsobem:
  - ŘADIČ DOMÉNY 7 15V
  - Alespoň 1500mA
  - Hello center nebo vnitřní pin musí být kladná pólu hello hello napájení

  ![Co v Startovní sady](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

Budete také muset:

* Počítač se systémem Windows, Mac nebo Linux.
* Bezdrátové připojení pro Edison tooconnect k.
* Internetu toodownload konfigurační nástroj pro připojení.

## <a name="assemble-your-board"></a>Sestavte panel

Tato část obsahuje kroky tooattach panel Intel® Edison modulu tooyour rozšíření.

1. Místní hello Intel® Edison modul v rámci hello bílé outline na vaší kartě rozšíření zarovnání hello díry v modulu hello s hello šrouby na panelu rozšíření hello.

2. Klepnout na hello modulu pod hello slova `What will you make?` dokud si myslíte, že během.

   ![Sestavte Tabule 2](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. Použijte hello dva šestnáctkových matice (zahrnutý v balíčku hello) toosecure hello modulu toohello rozšíření panelu.

   ![Sestavte Tabule 3](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. Vložte šroubu v jednom z hello čtyři díry rohu na panelu rozšíření hello. Otočením a posílit mezi hello bílé plastové nosníků do šroubovacím hello.

   ![Sestavte Tabule 4](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. Tento postup opakujte pro hello nosníků další tři rohu.

   ![Sestavte Tabule 5](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

Nyní je několik panel.

   ![sestavený panelu](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a>Napájení až Edison

1. Zařaďte hello napájení.

   ![Zařadit napájení](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. Zelená DIODU (s názvem bez přípony DS1 na hello Tabule rozšíření Arduino *), by měla zůstat po a light.

3. Počkejte jednu minutu toofinish Tabule hello dalším spuštění.

   > [!NOTE]
   > Pokud nemáte ke zdroji napájení řadiče domény, můžete stále panelu power hello přes USB port. V tématu `Connect Edison tooyour computer` podrobnosti. Pohánějící panel tímto způsobem může způsobit nepředvídatelné chování z vaší karty, zejména v případě, že pomocí sítě Wi-Fi nebo řídí motory.

## <a name="connect-edison-tooyour-computer"></a>Připojte počítač tooyour Edison

1. Přepnutí dolů hello mikrospínače směrem hello dva malých USB porty, takže Edison je v režimu zařízení. Rozdíly mezi zařízení režim a režim hostitele, prosím odkazovat [zde](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Přepněte dolů mikrospínače hello](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. Připojte kabel USB malých hello portu USB horním malých hello.

   ![Horní malých portu USB](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. Moduly hello druhém konci kabelu USB k počítači.

   ![Počítač USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. Budete vědět, že panel úplné inicializace když se počítač připojí na nový disk (podobně jako vkládání SD karty do počítače).

## <a name="download-and-run-hello-configuration-tool"></a>Stáhněte a spusťte konfigurační nástroj hello
Získat nejnovější nástroje Konfigurace hello z [tento odkaz](https://software.intel.com/en-us/iot/hardware/edison/downloads) uvedené v části hello `Installers` záhlaví. Spusťte nástroj hello a postupujte podle jeho na obrazovce pokyny, kde je potřeba klepnutím na tlačítko Další

### <a name="flash-firmware"></a>Flash firmwaru
1. Na hello `Set up options` klikněte na tlačítko `Flash Firmware`.
2. Vyberte bitovou kopii tooflash hello na panel jedním z následujících hello:
   - toodownload a flash panel s hello nejnovější firmware bitové kopie k dispozici z Intel, vyberte `Download hello latest image version xxxx`.
   - Vyberte panel s bitovou kopií již uložení v počítači, tooflash `Select hello local image`. Procházejte tooand hello vyberte bitovou kopii tooflash tooyour panelu.
3. Instalační program nástroje Hello pokusí tooflash panel. celý proces blikající Hello může trvat až too10 minut.

### <a name="set-password"></a>Nastavit heslo
1. Na hello `Set up options` klikněte na tlačítko `Enable Security`.
2. Můžete nastavit vlastní název pro panel Intel® Edison. Tato položka je nepovinná.
3. Zadejte heslo pro panel a pak klikněte na `Set password`.
4. Známku výpadku hello hesla, které se později používá.

### <a name="connect-wi-fi"></a>Připojení Wi-Fi
1. Na hello `Set up options` klikněte na tlačítko `Connect Wi-Fi`. Počkejte, až minutu tooone jako kontrolách počítači k dispozici sítě Wi-Fi.
2. Z hello `Detected Networks` rozevíracího seznamu vyberte vaší sítě.
3. Z hello `Security` rozevíracího seznamu, vyberte hello sítě typ zabezpečení.
4. Poskytnout vaše údaje přihlašovací jméno a heslo a pak klikněte na `Configure Wi-Fi`.
5. Známku výpadku hello IP adresy, která se později používá.

> [!NOTE]
> Ujistěte se, že Edison je připojený toohello stejné sítě jako váš počítač. Váš počítač připojí tooyour Edison pomocí hello IP adresy.

Blahopřejeme! Úspěšně jste nakonfigurovali Edison.

## <a name="summary"></a>Souhrn
V tomto článku když jste se naučili jak tooassemble hello Tabule Edison, flash jeho firmwaru, instalační program heslo a připojte ho pomocí nástroje Konfigurace tooWi-Fi. Všimněte si, že hello DIODU není ještě light nahoru. Další úlohou Hello je tooinstall hello nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na Edison.

## <a name="next-steps"></a>Další kroky
[Získat nástroje hello][get-the-tools]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md