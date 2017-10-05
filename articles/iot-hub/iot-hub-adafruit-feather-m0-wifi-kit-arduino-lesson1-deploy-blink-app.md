---
title: "Arduino připojit k Azure IoT - Lekce 1: nasazení aplikace | Microsoft Docs"
description: "Naklonujte ukázkovou aplikaci Arduino z Githubu a spusťte gulp k nasazení této aplikace na vašem Adafruit prolnutí M0 Wi-Fi. Tato ukázková aplikace bliká GPIO"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino vedla projekty, arduino vedla blikání, arduino vedla blikání kód, program blikání arduino, příklad arduino blikání"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: b0a7d076-d580-4686-9f7d-c0712750b615
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4431808ac6182d194e841c087c8f89f1a12b1911
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a>Vytvoření a nasazení aplikace blink
## <a name="what-you-will-do"></a>Co provedete
Naklonujte ukázkovou aplikaci Arduino z Githubu a pomocí nástroje gulp nasadit ukázkovou aplikaci pro panel Adafruit prolnutí M0 Wi-Fi Arduino. Aplikace bliká ukázka GPIO č. 13 na barod VEDLA každé dvě sekundy.

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting-page].

## <a name="what-you-will-learn"></a>Co se dozvíte
* Postup nasazení a spuštění ukázkové aplikace na panel Arduino.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili následující operace:

* [Konfigurace zařízení][configure-your-device]
* [Získat nástroje][get-the-tools]

## <a name="open-the-sample-application"></a>Otevřete ukázkové aplikace
Chcete-li otevřít ukázkové aplikace, postupujte takto:

1. Naklonujte úložiště ukázkové z Githubu spuštěním následujícího příkazu:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Struktura úložiště][repo-structure]

`app.ino` v soubor `app` podsložky je klíče zdrojového souboru, který obsahuje kód, který ovládacího prvku Indikátor.

### <a name="install-application-dependencies"></a>Instalace závislostí aplikací
Nainstalujte v knihovnách a dalších modulů, které potřebujete pro ukázkovou aplikaci tak, že spustíte následující příkaz:

```bash
npm install
```

## <a name="configure-the-device-connection"></a>Nakonfigurujte připojení zařízení
Konfigurace připojení zařízení, postupujte takto:

1. Získání sériového portu zařízení pomocí rozhraní příkazového řádku zjišťování zařízení:

   ```bash
   devdisco list --usb
   ```

   Měli byste zobrazený výstup, který je podobný následujícímu a najít usb COM port pro panel Arduino: ![zjišťování zařízení][device-discovery]

2. Otevřete soubor `config.json` ve složce lekce a přidejte hodnotu nalezen číslo portu COM:

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > Pro port COM, na platformě Windows má formát `COM1, COM2, ...`. V systému macOS nebo Ubuntu začíná `/dev/`.

## <a name="deploy-and-run-the-sample-application"></a>Nasazení a spuštění ukázkové aplikace
### <a name="install-the-required-tools-for-your-arduino-board"></a>Nainstalujte požadované nástroje pro panel Arduino

Nainstalujte sadu SDK Azure IoT Hub pro panel Arduino spuštěním následujícího příkazu:

```bash
gulp install-tools
```

Tato úloha může trvat dlouhou dobu v závislosti na připojení k síti.

> [!NOTE]
> Při spuštění úlohy gulp prosím Ukončete spuštěnou instanci Arduino IDE: `install-tools`, `run`.

### <a name="deploy-and-run-the-sample-app"></a>Nasazení a spuštění ukázkové aplikace
Nasazení a spuštění ukázkové aplikace tak, že spustíte následující příkaz:

```bash
gulp run

# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-the-app-works"></a>Ověřte aplikace funguje
Pokud nevidíte DIODU blikat, najdete v článku [Průvodce odstraňováním potíží s] [ troubleshooting-page] pro řešení běžných potíží.

![Indikátor blikat][led-blinking]

## <a name="summary"></a>Souhrn
Nainstalujete požadované nástroje pro práci s panel Arduino a nasadit ukázkovou aplikaci pro panel Arduino blikání Indikátor. Teď můžete vytvořit, nasadit a spustit jinou ukázkovou aplikaci, která panel Arduino se připojuje ke službě Azure IoT Hub odesílat a přijímat zprávy.

## <a name="next-steps"></a>Další kroky
[Získat nástroje Azure][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md