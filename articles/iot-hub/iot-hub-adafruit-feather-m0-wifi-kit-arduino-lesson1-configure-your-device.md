---
title: "Connect Arduino (C) tooAzure IoT - lekci 1: Konfigurace zařízení | Microsoft Docs"
description: "Nakonfigurujte Adafruit prolnutí M0 Wi-Fi pro první použití."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino nastavit připojení arduino toopc, instalační program arduino, arduino panelu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: f5b334f0-a148-41aa-b374-ce7b9f5b305a
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 30b764e8ff6221995456283a226e79f064b2d74e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-device"></a>Konfigurace zařízení
## <a name="what-you-will-do"></a>Co provedete
Sestavte hello Tabule, napájený ho nakonfigurujte Adafruit prolnutí M0 Wi-Fi Arduino panel pro první použití. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).

## <a name="what-you-need"></a>Co potřebujete
toocomplete tuto operaci je třeba hello vaší Adafruit prolnutí M0 Wi-Fi Starter Kit následujících částí:

* Hello Tabule Adafruit prolnutí M0 Wi-Fi
* Micro B tooType kabelu A USB

![Kit][kit]

Budete také muset:

* Počítač se systémem Windows, Mac nebo Linux.
* Bezdrátové připojení pro vaše tooconnect Arduino Tabule pro.
* Internetu toodownload konfigurační nástroj pro připojení.

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak tooassemble Arduino panelu a power odpovídající hello následující lekci.
* Jak tooadd oprávnění Ubuntu sériového portu.

## <a name="connect-your-arduino-board-tooyour-computer"></a>Připojení počítače Arduino panelu tooyour

1. Připojte kabel USB malých hello portu USB horním malých hello.

   ![Horní malých portu USB][top-micro-usb-port]

2. Moduly hello druhém konci kabelu USB k počítači.

   ![Počítač USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a>Přidání oprávnění sériového portu v Ubuntu

Pokud používáte Windows nebo systému macOS, můžete tuto část přeskočit. Ubuntu musíte hello následující kroky toomake se, že má uživatel normální linux hello hello oprávnění toooperate na portu USB hello Arduino panel.

1. Nyní jako normální uživatelské z terminálu:

   ```bash
   ls -l /dev/ttyUSB*
   # Or
   ls -l /dev/ttyACM*
   ```

   Zobrazí se něco podobného jako:

   ```bash
   crw-rw---- 1 root uucp 188, 0 5 apr 23.01 ttyUSB0
   # Or
   crw-rw---- 1 root dialout 188, 0 5 apr 23.01 ttyACM0
   ```

   Hello "0" může být odlišný počet nebo více položek, může být vrácena. V první data případu hello hello potřebujeme je `uucp`, v hello druhou je `dialout`, což je vlastník skupiny hello hello souboru.

2. Přidáte skupinu uživatelů toohello toohello:

   ```bash
   sudo usermod -a -G group-name username
   ```

   Kde `group-name` se nenašla v prvním kroku hello hello data a `username` je vaše uživatelské jméno linux.

3. Budete znovu potřebovat toolog v a pro tuto změnu tootake účinek a hello dokončení instalace.

## <a name="summary"></a>Souhrn
V tomto článku, když jste se naučili jak tooconfigure Arduino panel. Další úlohou Hello je tooinstall hello nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na vaší kartě Arduino.

![Je připravená hardwaru][hardware-is-ready]

## <a name="next-steps"></a>Další kroky
[Získat nástroje hello][get-the-tools]
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md