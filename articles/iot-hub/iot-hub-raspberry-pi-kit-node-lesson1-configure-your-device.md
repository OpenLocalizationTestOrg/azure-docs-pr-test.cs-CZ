---
title: "Připojení k Azure IoT - lekci 1 malin platformy (uzel): Konfigurace zařízení | Microsoft Docs"
description: Configure Raspberry Pi 3 for first-time use and install the Raspbian OS, a free operating system that is optimized for the Raspberry Pi hardware.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "instalace raspbian, raspbian stažení způsob pro instalaci raspbian, raspbian instalační program, instalace raspbian Malinová platformy, malinová pí instalace operačního systému, malinová pí sd karty instalace, malin pí připojení, připojení k Malinová pi, malinová pí připojení"
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
ms.openlocfilehash: b848c48157a2310f0eb1d6398f8b9aaa4395d47f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-device"></a>Konfigurace zařízení
## <a name="what-you-will-do"></a>Co provedete
Nakonfigurujte platformy pro první použití a nainstalujte Raspbian operačního systému. Raspbian je volné operačního systému, která je optimalizovaná pro hardware malin platformy. Pokud máte potíže, můžete hledat řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Postup instalace Raspbian v pí.
* Jak power až pí pomocí kabelu USB.
* Postup připojení platformy k síti pomocí kabelu Ethernet nebo bezdrátové sítě.
* Postup přidání DIODU do breadboard a připojte ho k pí.

## <a name="what-you-will-need"></a>Co budete potřebovat
Pro dokončení této operace, musíte z vaší malin pí 3 Starter Kit následujících částí:

* Panel malin pí 3
* Karta microSD 16 GB
* 2 amp napájení 5 volt s 6 stopy malých kabelu USB
* Breadboard
* Konektor vodičům
* 560 ohm odpor
* Rozptýlený DIODU 10 mm
* Kabel Ethernet

![Co v Startovní sady](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Budete také muset:

* Pevné nebo bezdrátové připojení pro platformy pro připojení k.
* USB-adaptér nebo miniSD karta SD vypálíte image operačního systému na kartě microSD.
* Počítač se systémem Windows, Mac nebo Linux. Počítač se používá k instalaci Raspbian na kartě microSD.
* Připojení k Internetu kvůli stahování nezbytné nástroje a software.

## <a name="install-raspbian-on-the-microsd-card"></a>Na kartě microSD nainstalovat Raspbian
Připravte karty microSD pro instalaci bitové kopie Raspbian.

1. Stáhněte si Raspbian.
   1. [Stáhněte si](https://www.raspberrypi.org/downloads/raspbian/) soubor .zip pro Raspbian Klára s pixelů.
   2. Extrahujte Raspbian image do složky v počítači.
2. Nainstalujte Raspbian microSD karta.
   1. [Stáhněte si](https://www.etcher.io) a nainstalujte nástroj zapisovací jednotka Etcher SD karty.
   2. Spusťte Etcher a vyberte Raspbian bitovou kopii, která jste extrahovali v kroku 1.
   3. Vyberte jednotku microSD karta.
      Všimněte si, že Etcher může jste již vybrali správnou jednotku.
   4. Klikněte na tlačítko **Flash** nainstalovat Raspbian do microSD karta.
   5. Karta microSD odeberte z počítače, po dokončení instalace.
      Je bezpečné karty microSD přímo odebrat, protože Etcher automaticky vysune nebo odpojí microSD karta po dokončení.
   6. Karta microSD vložte do pí.

![Vložit kartu SD.](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a>Zapnout platformy
Zapněte pí pomocí kabelu USB micro a napájení.

![Zapnout](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> Je důležité sady, který je minimálně použít napájení 2A a ujistěte se, že vaše malin má dostatek power fungovala správně.

## <a name="enable-ssh"></a>Povolit SSH
Od verze v listopadu 2016 Raspbian má server SSH ve výchozím nastavení zakázaná. Budete muset povolit ručně. Můžete se podívat do [oficiální pokyny](https://www.raspberrypi.org/documentation/remote-access/ssh/) nebo připojit monitorování a přejděte na **Předvolby -> konfigurace platformy malin** povolit SSH.

## <a name="connect-raspberry-pi-3-to-the-network"></a>Připojení k síti malin pí 3
Pi můžete připojit ke kabelové síti nebo k bezdrátové síti. Ujistěte se, že platformy je připojený ke stejné síti jako počítače. Například můžete pí připojit ke stejnému přepínači, který je počítač připojen k.

### <a name="connect-to-a-wired-network"></a>Připojení k drátové síti
Kabel Ethernet slouží k připojení platformy k drátové síti. Dva LED na platformy zapnout, pokud připojení.

![Připojení pomocí kabelu Ethernet](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-to-a-wireless-network"></a>Připojit k bezdrátové síti
Postupujte podle [pokyny](https://www.raspberrypi.org/learning/software-guide/wifi/) z Foundation pí malin pro připojení k bezdrátové síti pí. Tyto pokyny vyžadují, abyste nejdřív připojit k pí monitorování a klávesnice.

## <a name="connect-the-led-to-pi"></a>Připojení DIODU PI
Chcete-li tuto úlohu dokončit, použijte [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), vodičům konektor Indikátor a se odpor. Připojte je k [pro obecné účely vstupu a výstupu](https://www.raspberrypi.org/documentation/usage/gpio/) porty (GPIO) čísla pí.

![Breadboard DIODU a odpor](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Připojit kratší rameno DIODU k **GPIO zem (Pin 6)**.
2. Připojte k jeden úsek se odpor delší rameno Indikátor.
3. Připojit rameno odpor k **GPIO 4 (Pin 7)**.

Všimněte si, že Indikátor polarita je důležité. Toto nastavení polarita se běžně označuje jako aktivní nízká.

![Uspořádání kolíků](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Blahopřejeme! Úspěšně jste nakonfigurovali pí.

## <a name="summary"></a>Souhrn
V tomto článku jsme zjistili, jak nakonfigurovat tak, že instalace Raspbian, Pi připojení k síti a připojení DIODU PI pí. Všimněte si, že DIODU není ještě light nahoru. Dalším krokem je instalace nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na platformy.

![Je připravená hardwaru](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Další kroky
[Získat nástroje](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

