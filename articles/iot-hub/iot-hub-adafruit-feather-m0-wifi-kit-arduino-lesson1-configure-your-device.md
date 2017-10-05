---
title: "Connect Arduino (C) k Azure IoT - lekci 1: Konfigurace zařízení | Microsoft Docs"
description: "Nakonfigurujte Adafruit prolnutí M0 Wi-Fi pro první použití."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino nastavit připojení k počítači, instalační program arduino, arduino panelu arduino"
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
ms.openlocfilehash: 9e319292e5d30dea7e45857e435825861aad1c84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-device"></a>Konfigurace zařízení
## <a name="what-you-will-do"></a>Co provedete
Sestavte Tabule, napájený ho nakonfigurujte Adafruit prolnutí M0 Wi-Fi Arduino panel pro první použití. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).

## <a name="what-you-need"></a>Co potřebujete
Pro dokončení této operace, potřebujete pro vaše Adafruit prolnutí M0 Wi-Fi Starter Kit následujících částí:

* Panel Adafruit prolnutí M0 Wi-Fi
* Micro B kabelem USB typ A

![Kit][kit]

Budete také muset:

* Počítač se systémem Windows, Mac nebo Linux.
* Bezdrátové připojení pro panel Arduino pro připojení k.
* Připojení k Internetu kvůli stahování nástroje Konfigurace.

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak sestavit panel Arduino a spotřeby pro následující lekce.
* Postup přidání oprávnění sériového portu v Ubuntu.

## <a name="connect-your-arduino-board-to-your-computer"></a>Panel Arduino připojte k počítači

1. Připojte kabel USB malých nejvyšší malých portu USB.

   ![Horní malých portu USB][top-micro-usb-port]

2. Druhém konci kabelu USB připojte k počítači.

   ![Počítač USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a>Přidání oprávnění sériového portu v Ubuntu

Pokud používáte Windows nebo systému macOS, můžete tuto část přeskočit. Ubuntu musíte následujících kroků zkontrolujte, zda že má uživatel normální linux oprávnění provoz na portu USB Arduino panel.

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

   "0" může být odlišný počet nebo více položek, může být vrácena. V prvním případě dat, potřebujeme je `uucp`, za sekundu je `dialout`, který je vlastníkem skupiny souboru.

2. Přidejte uživatele do do skupiny:

   ```bash
   sudo usermod -a -G group-name username
   ```

   Kde `group-name` se data uvedená v prvním kroku, a `username` je vaše uživatelské jméno linux.

3. Musíte se přihlásit v a znovu tato změna se projeví a dokončete instalaci.

## <a name="summary"></a>Souhrn
V tomto článku jsme zjistili, jak nakonfigurovat Arduino panel. Dalším krokem je instalace nezbytné nástroje a software v rámci přípravy na spuštění ukázkové aplikace na vaší kartě Arduino.

![Je připravená hardwaru][hardware-is-ready]

## <a name="next-steps"></a>Další kroky
[Získat nástroje][get-the-tools]
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md