---
title: "Připojit Arduino tooAzure IoT - lekci 1: nasazení aplikace | Microsoft Docs"
description: "Klonování hello ukázkovou aplikaci Arduino z Githubu a spusťte gulp toodeploy této aplikace tooyour Adafruit prolnutí M0 Wi-Fi. Tato ukázková aplikace bliká hello GPIO"
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
ms.openlocfilehash: 5bf8e4ae88e070aeacf34bfc43b8d2daeeb1a2fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a>Vytvoření a nasazení aplikace hello blikání
## <a name="what-you-will-do"></a>Co provedete
Klonování hello ukázkovou aplikaci Arduino z Githubu a používat hello gulp nástroj toodeploy hello ukázkové aplikace tooyour Adafruit prolnutí M0 Wi-Fi Arduino panelu. Hello ukázkové aplikace bliká hello GPIO č. 13 barod VEDLA každé dvě sekundy.

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting-page].

## <a name="what-you-will-learn"></a>Co se dozvíte
* Jak toodeploy a spuštění hello ukázkové aplikace na vaší kartě Arduino.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili hello následující operace:

* [Konfigurace zařízení][configure-your-device]
* [Získat nástroje hello][get-the-tools]

## <a name="open-hello-sample-application"></a>Ukázkové aplikace otevřete hello
tooopen hello ukázkové aplikace, postupujte takto:

1. Klonování úložiště ukázkové hello z Githubu spuštěním hello následující příkaz:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-feather-m0-getting-started.git
   ```
2. Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:

   ```bash
   cd iot-hub-c-feather-m0-getting-started
   cd Lesson1
   code .
   ```

   ![Struktura úložiště][repo-structure]

Hello `app.ino` souboru v hello `app` podsložky je hello klíče zdrojový soubor, který obsahuje hello kód toocontrol hello Indikátor.

### <a name="install-application-dependencies"></a>Instalace závislostí aplikací
Nainstalujte hello knihovny a z ostatních modulů, které potřebujete pro ukázkovou aplikaci hello spuštěním hello následující příkaz:

```bash
npm install
```

## <a name="configure-hello-device-connection"></a>Nakonfigurujte připojení zařízení hello
tooconfigure hello připojení zařízení, postupujte takto:

1. Získejte hello sériového portu zařízení hello s hello zařízení zjišťování rozhraní příkazového řádku:

   ```bash
   devdisco list --usb
   ```

   Měli byste zobrazený výstup, který je podobný následující toohello a najít hello usb COM port pro panel Arduino: ![zjišťování zařízení][device-discovery]

2. Soubor otevřete hello `config.json` v hello lekce složky a přidat hello hodnotu hello najít číslo portu COM:

   ```json
   {
       "device_port" : "COM1"
   }
   ```
   ![Config.JSON][config-json]
   > [!NOTE]
   > Pro port hello COM na platformě Windows má formát hello `COM1, COM2, ...`. V systému macOS nebo Ubuntu začíná `/dev/`.

## <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
### <a name="install-hello-required-tools-for-your-arduino-board"></a>Instalace nástrojů pro hello požadované pro panel Arduino

Nainstalujte hello sady SDK Azure IoT Hub pro panel Arduino spuštěním hello následující příkaz:

```bash
gulp install-tools
```

Tato úloha může trvat dlouhou dobu toocomplete, v závislosti na připojení k síti.

> [!NOTE]
> Ukončete prosím hello spuštěna instance Arduino IDE při spuštění úlohy gulp: `install-tools`, `run`.

### <a name="deploy-and-run-hello-sample-app"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello spuštěním hello následující příkaz:

```bash
gulp run

# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

### <a name="verify-hello-app-works"></a>Ověřte hello aplikace funguje
Pokud nevidíte hello DIODU blikat, přečtěte si téma hello [Průvodce odstraňováním potíží s] [ troubleshooting-page] řešení toocommon problémů.

![Indikátor blikat][led-blinking]

## <a name="summary"></a>Souhrn
Nainstalujete hello požadované nástroje toowork s panel Arduino a nasadit ukázkové aplikace tooyour Arduino Tabule tooblink hello Indikátor. Teď můžete vytvořit, nasadit a spusťte jinou ukázkovou aplikaci, která se připojí vaše Arduino Tabule tooAzure toosend IoT Hub a příjem zpráv.

## <a name="next-steps"></a>Další kroky
[Získat hello Azure nástroje][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting-page]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-blink-arduino-mac.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[led-blinking]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/led_blinking.png
[get-the-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md