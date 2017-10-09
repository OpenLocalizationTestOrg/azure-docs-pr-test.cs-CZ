---
title: "Připojit malin platformy (uzel) tooAzure IoT - lekci 1: Konfigurace zařízení | Microsoft Docs"
description: "Nakonfigurujte malin pí 3 pro první použití a nainstalujte hello Raspbian OS, volné operační systém, který je optimalizovaná pro hello malin pí hardwaru."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "instalace raspbian, raspbian stažení způsob tooinstall raspbian raspbian instalační program, malinová platformy instalace raspbian Malinová pí instalace operačního systému, malinová pí sd karty instalace, malinová pí připojení, připojit tooraspberry pi, malinová pí připojení"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 43f7c2cf-f1a5-4dd5-93f0-7e546c6dc91e
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 504a4d2a3f29717f955530812442cce2a78a6448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-device"></a>Konfigurace zařízení
## <a name="what-you-will-do"></a>Co provedete
Nakonfigurujte platformy pro první použití a nainstalujte hello Raspbian operačního systému. Raspbian je volné operační systém, která je optimalizovaná pro hello malin pí hardwaru. Pokud máte potíže, můžete hledat řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak tooinstall Raspbian na pí.
* Jak toopower až pí pomocí kabelu USB.
* Jak tooconnect pí toohello sítě pomocí kabelu Ethernet nebo bezdrátové sítě.
* Jak tooadd toohello DIODU breadboard a připojte ho tooPi.

## <a name="what-you-will-need"></a>Co budete potřebovat
toocomplete tuto operaci je třeba hello následujících částí z vaší malin pí 3 Starter Kit:

* Hello Tabule malin pí 3
* Karta microSD 16 GB Hello
* Hello 5 volt 2 amp napájení pomocí kabelu USB malých 6 stopy hello
* Hello breadboard
* Konektor vodičům
* 560 ohm odpor
* Rozptýlený DIODU 10 mm
* Hello kabel Ethernet

![Co v Startovní sady](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Budete také muset:

* Pevné nebo bezdrátové připojení pro platformy tooconnect k.
* USB SD adaptér nebo miniSD karty tooburn hello image operačního systému na kartě microSD hello.
* Počítač se systémem Windows, Mac nebo Linux. počítač Hello je použité tooinstall Raspbian na kartě microSD hello.
* Toodownload připojení Internetu hello nezbytné nástroje a software.

## <a name="install-raspbian-on-hello-microsd-card"></a>Nainstalujte Raspbian na kartě microSD hello
Připravte hello microSD karta pro instalaci bitové kopie Raspbian hello.

1. Stáhněte si Raspbian.
   1. [Stáhněte si](https://www.raspberrypi.org/downloads/raspbian/) hello soubor .zip pro Raspbian Klára s pixelů.
   2. Extrahujte hello Raspbian image tooa složky v počítači.
2. Nainstalujte Raspbian toohello microSD karta.
   1. [Stáhněte si](https://www.etcher.io) a nainstalujte nástroj hello Etcher SD karty zapisovací jednotka.
   2. Spusťte Etcher a vyberte bitovou kopii hello Raspbian, které jste extrahovali v kroku 1.
   3. Vyberte jednotku karty microSD hello.
      Všimněte si, že Etcher může jste již vybrali správnou jednotku hello.
   4. Klikněte na tlačítko **Flash** tooinstall Raspbian toohello microSD karta.
   5. Po dokončení instalace odeberte hello microSD karta z vašeho počítače.
      Je bezpečné tooremove hello microSD karta přímo protože Etcher automaticky vysune nebo odpojí hello microSD karta po dokončení.
   6. Karta microSD hello vložte do pí.

![Vložit hello SD karty](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a>Zapnout platformy
Zapněte pí pomocí kabelu USB malých hello a hello napájení.

![Zapnout](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> Je důležité toouse hello napájení v hello kit, která je aspoň 2A toomake se, že vaše malin má dostatek power toowork správně.

## <a name="enable-ssh"></a>Povolit SSH
Od verze hello verze v listopadu 2016 má Raspbian hello SSH serveru ve výchozím nastavení zakázaná. Je třeba tooenable ji ručně. Může odkazovat toohello [oficiální pokyny](https://www.raspberrypi.org/documentation/remote-access/ssh/) nebo připojit monitorování a přejděte příliš**Předvolby -> konfigurace platformy malin** tooenable SSH.

## <a name="connect-raspberry-pi-3-toohello-network"></a>Připojit síť toohello malin pí 3
Pi tooa drátové síti nebo tooa bezdrátové síti se můžete připojit. Ujistěte se, že platformy je připojený toohello stejné sítě jako váš počítač. Například se můžete připojit toohello Pi, stejný přepínač, že je počítač připojen k.

### <a name="connect-tooa-wired-network"></a>Připojit tooa drátové síti
Použijte hello Ethernet kabel tooconnect pí tooyour drátové síti. Hello dvě LED na platformy zapnout, pokud hello připojení.

![Připojení pomocí kabelu Ethernet](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-tooa-wireless-network"></a>Připojit tooa bezdrátové sítě
Postupujte podle hello [pokyny](https://www.raspberrypi.org/learning/software-guide/wifi/) z hello malin platformy Foundation tooconnect pí tooyour bezdrátové sítě. Tyto pokyny vyžadují toofirst připojit monitorování a tooPi klávesnice.

## <a name="connect-hello-led-toopi"></a>Připojit hello DIODU tooPi
toocomplete tento úkol, použijte hello [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), hello vodičům konektor, hello DIODU a hello odpor. Připojte je toohello [pro obecné účely vstupu a výstupu](https://www.raspberrypi.org/documentation/usage/gpio/) porty (GPIO) čísla pí.

![Breadboard DIODU a odpor](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Připojit hello kratší rameno hello DIODU příliš**GPIO zem (Pin 6)**.
2. Připojte hello delší rameno hello DIODU tooone rameno odpor hello.
3. Připojit hello jiných rameno hello odpor příliš**GPIO 4 (Pin 7)**.

Všimněte si, že hello DIODU polarita je důležité. Toto nastavení polarita se běžně označuje jako aktivní nízká.

![Uspořádání kolíků](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Blahopřejeme! Úspěšně jste nakonfigurovali pí.

## <a name="summary"></a>Souhrn
V tomto článku, když jste se naučili jak tooconfigure pí Raspbian připojování sítě tooa Pi, instalace a připojením tooPi DIODU. Všimněte si, že hello DIODU není ještě light nahoru. Další úlohou Hello je tooinstall hello nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na platformy.

![Je připravená hardwaru](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Další kroky
[Získat nástroje hello](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

