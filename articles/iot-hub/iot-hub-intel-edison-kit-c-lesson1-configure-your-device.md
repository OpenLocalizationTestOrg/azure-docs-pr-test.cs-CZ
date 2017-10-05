---
title: "Connect Intel Edison (C) k Azure IoT - lekci 1: Konfigurace zařízení | Microsoft Docs"
description: "Nakonfigurujte Intel Edison pro první použití."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino nastavit připojení k počítači, instalační program arduino, arduino panelu arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: bb8aa45b-d3ff-4438-b9d6-a9725a45ade1
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0c92d9f7ba63b0a0929ff95599fd757ea425ef1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-intel-edison"></a>Konfigurace vaší Edison Intel
## <a name="what-you-will-do"></a>Co provedete
Nakonfigurujte Intel Edison pro první použití sestavte Tabule, je napájený a instalace nástroje pro konfiguraci na vaší ploše operační systém určený k flash Edison na firmwaru, nastavte jeho heslo a připojte ho k Wi-Fi. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak sestavit Edison panelu a spotřeby nahoru.
* Jak flash Edison na firmwaru, nastavte heslo a připojení Wi-Fi.

## <a name="what-you-need"></a>Co potřebujete
Pro dokončení této operace, musíte z vaší Intel Edison Starter Kit následujících částí:

* Intel® Edison modulu
* Panel Arduino rozšíření
* Jako mezeru mezi řádky nebo šrouby, zahrnout v balení, včetně dvou šrouby připevnit modulu Tabule rozšíření a čtyři sady šrouby a plastové vymezovače.
* Micro B kabelem USB typ A
* Zdroj napájení přímo aktuální (DC). Zdroj napájení by měl být hodnocení následujícím způsobem:
  - ŘADIČ DOMÉNY 7 15V
  - Alespoň 1500mA
  - PIN kód center nebo vnitřní by měla být kladná pólu napájení

  ![Co v Startovní sady](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

Budete také muset:

* Počítač se systémem Windows, Mac nebo Linux.
* Bezdrátové připojení pro Edison pro připojení k.
* Připojení k Internetu kvůli stahování nástroje Konfigurace.

## <a name="assemble-your-board"></a>Sestavte panel

Tato část obsahuje kroky pro připojení k vaší karty rozšíření modulu Intel® Edison.

1. Místní modul Intel® Edison v rámci bílé osnovy na vaší kartě rozšíření zarovnání díry na modul s šrouby na kartě rozšíření.

2. Klepnout na modul pod slova `What will you make?` dokud si myslíte, že během.

   ![Sestavte Tabule 2](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. Dva šestnáctkových matice (součástí balíčku) použijte k zabezpečení modulu Tabule rozšíření.

   ![Sestavte Tabule 3](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. Vložte šroubu v jednom z díry čtyři rohu na kartě rozšíření. Otočením a posílit mezi bílé plastové nosníků do šroubek.

   ![Sestavte Tabule 4](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. Pro další tři rohu nosníků opakujte.

   ![Sestavte Tabule 5](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

Nyní je několik panel.

   ![sestavený panelu](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a>Napájení až Edison

1. Zařaďte napájení.

   ![Zařadit napájení](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. Zelená DIODU (s názvem bez přípony DS1 na panelu rozšíření Arduino *), by měla zůstat po a light.

3. Počkejte, než jedna minuta pro panel na dokončení spuštění.

   > [!NOTE]
   > Pokud nemáte ke zdroji napájení řadiče domény, může stále spotřeby Tabule přes USB port. V tématu `Connect Edison to your computer` podrobnosti. Pohánějící panel tímto způsobem může způsobit nepředvídatelné chování z vaší karty, zejména v případě, že pomocí sítě Wi-Fi nebo řídí motory.

## <a name="connect-edison-to-your-computer"></a>Připojte k počítači Edison

1. Přepněte dolů mikrospínače směrem dva malých USB porty, takže Edison je v režimu zařízení. Rozdíly mezi zařízení režim a režim hostitele, prosím odkazovat [zde](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Přepněte dolů mikrospínače](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. Připojte kabel USB malých nejvyšší malých portu USB.

   ![Horní malých portu USB](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. Druhém konci kabelu USB připojte k počítači.

   ![Počítač USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. Budete vědět, že panel úplné inicializace když se počítač připojí na nový disk (podobně jako vkládání SD karty do počítače).

## <a name="download-and-run-the-configuration-tool"></a>Stáhněte a spusťte nástroj Konfigurace
Získat nejnovější nástroje Konfigurace z [tento odkaz](https://software.intel.com/en-us/iot/hardware/edison/downloads) uvedené v části `Installers` záhlaví. Spusťte nástroj a postupujte podle jeho na obrazovce pokyny, kde je potřeba klepnutím na tlačítko Další

### <a name="flash-firmware"></a>Flash firmwaru
1. Na `Set up options` klikněte na tlačítko `Flash Firmware`.
2. Vyberte bitovou kopii na flash na panel jedním z následujících akcí:
   - Chcete-li stáhnout a flash panel s nejnovější bitové kopie firmwaru, které jsou k dispozici z Intel, vyberte `Download the latest image version xxxx`.
   - Chcete-li flash panel s bitovou kopií již uložení v počítači, vyberte `Select the local image`. Vyhledejte a vyberte bitovou kopii, kterou chcete flash pro panel.
3. Nástroj instalační program se pokusí o flash panel. Celý proces blikající může trvat až 10 minut.

### <a name="set-password"></a>Nastavit heslo
1. Na `Set up options` klikněte na tlačítko `Enable Security`.
2. Můžete nastavit vlastní název pro panel Intel® Edison. Tato položka je nepovinná.
3. Zadejte heslo pro panel a pak klikněte na `Set password`.
4. Známku výpadku heslo, které se později používá.

### <a name="connect-wi-fi"></a>Připojení Wi-Fi
1. Na `Set up options` klikněte na tlačítko `Connect Wi-Fi`. Počkejte, až na jednu minutu jako počítač hledá dostupných sítí Wi-Fi.
2. Z `Detected Networks` rozevíracího seznamu vyberte vaší sítě.
3. Z `Security` rozevíracího seznamu vyberte typ zabezpečení sítě.
4. Poskytnout vaše údaje přihlašovací jméno a heslo a pak klikněte na `Configure Wi-Fi`.
5. Známku výpadku IP adresy, která se později používá.

> [!NOTE]
> Ujistěte se, že Edison je připojený ke stejné síti jako počítače. Váš počítač připojí k vaší Edison pomocí IP adresy.

Blahopřejeme! Úspěšně jste nakonfigurovali Edison.

## <a name="summary"></a>Souhrn
V tomto článku jsme zjistili, jak sestavit Tabule Edison, flash jeho firmwaru, instalační program heslo a připojte ho k Wi-Fi pomocí nástroje Konfigurace. Všimněte si, že DIODU není ještě light nahoru. Dalším krokem je instalace nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na Edison.

## <a name="next-steps"></a>Další kroky
[Získat nástroje][get-the-tools]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md