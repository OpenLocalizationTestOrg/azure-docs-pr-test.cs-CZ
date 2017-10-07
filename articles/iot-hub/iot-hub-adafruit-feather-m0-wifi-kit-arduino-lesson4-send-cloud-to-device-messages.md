---
title: "Connect Arduino (C) tooAzure IoT - Lekce 4: Cloud zařízení | Microsoft Docs"
description: "Ukázková aplikace spustí na Adafruit prolnutí M0 Wi-Fi a monitorování příchozích zpráv ze služby IoT hub. Nová úloha gulp odešle zprávy tooAdafruit prolnutí M0 Wi-Fi z vaší IoT hub tooblink hello Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "ovládací prvek arduino vedla z webové, arduino řízení vedla přes web"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: a0bf53fb-29fb-485f-ba4a-6c715057b1a2
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: dcddd61ff684f49436103675938d719cb227c409
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a>Spuštění ukázkové aplikace tooreceive zprávy typu cloud zařízení
V tomto článku nasazení ukázkové aplikace na vaší kartě Adafruit prolnutí M0 Wi-Fi Arduino.

Ukázková aplikace Hello monitoruje příchozích zpráv ze služby IoT hub. Také spustit úlohu gulp na váš počítač toosend zprávy tooyour Arduino Tabule ze služby IoT hub. Zprávy hello přijetí hello ukázkovou aplikaci bliká DIODU hello. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-do"></a>Co provedete
* Připojte hello ukázkové aplikace tooyour IoT hub.
* Nasazení a spuštění hello ukázkovou aplikaci.
* Odesílat zprávy z vaší IoT hub tooyour Arduino Tabule tooblink hello Indikátor.

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak toomonitor příchozích zpráv ze služby IoT hub.
* Jak toosend cloud zařízení zprávy z vaší IoT hub tooyour Arduino panelu.

## <a name="what-you-need"></a>Co potřebujete
* Panel Arduino, nastavte pro použití. jak zjistit, tooset až vaší karty Arduino toolearn [konfigurace zařízení][configure-your-device].
* Služby IoT hub, který se vytvoří ve vašem předplatném Azure. toolearn jak toocreate služby IoT hub, najdete v části [vytvoření služby Azure IoT Hub][create-your-azure-iot-hub].

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a>Připojit hello ukázkové aplikace tooyour IoT hub

1. Ujistěte se, že jste ve složce úložišti hello `iot-hub-c-feather-m0-getting-started`.

   Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:

   ```bash
   cd Lesson4
   code .
   ```

   Všimněte si hello `app.ino` souboru v hello `app` podsložky. Hello `app.ino` soubor je soubor hello zdroj klíče, který obsahuje hello kód toomonitor příchozí zprávy ze služby IoT hub hello. Hello `blinkLED` funkce bliká DIODU hello.

   ![Struktura úložišti v ukázkové aplikaci hello][repo-structure]

2. Získejte hello sériového portu zařízení hello s hello zařízení zjišťování rozhraní příkazového řádku:

   ```bash
   devdisco list --usb
   ```

   Měli byste zobrazený výstup, který je podobný toohello následující a najít hello usb COM port pro panel Arduino:

   ![Zjišťování zařízení][device-discovery]

3. Soubor otevřete hello `config.json` ve složce lekce hello a hodnotu vstupního hello hello najít číslo portu COM:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > Pro port hello COM na platformě Windows má formát hello `COM1, COM2, ...`. V systému macOS nebo Ubuntu, bude začínat `/dev/`.

4. Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. Ujistěte se, hello následující nahrazení v hello `config-arduino.json` souboru:

   Pokud jste dokončili postup hello v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] v tomto počítači se dědí všechny konfigurace hello, takže hello krok toohello úloh nasazení, můžete přeskočit a spuštění ukázkové aplikace hello. Pokud jste dokončili postup hello v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] na jiném počítači, musíte tooreplace hello zástupné symboly v hello `config-arduino.json` souboru. Hello `config-arduino.json` soubor je v podsložce hello domovskou složku.

   ![Obsah souboru config arduino.json hello][config-arduino-json]

   * Nahraďte **[Wi-Fi SSID]** s vaší SSID Wi-Fi, který připojený toohello Internetu.
   * Nahraďte **[Wi-Fi heslo]** pomocí hesla Wi-Fi. Odeberte hello řetězec, pokud Wi-Fi nevyžaduje heslo.
   * Nahraďte **[řetězec připojení zařízení IoT]** s hello zařízení připojovací řetězec, který můžete získat spuštěním hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` příkaz.
   * Nahraďte **[IoT hub, připojovací řetězec]** s hello IoT hub připojovací řetězec, který můžete získat spuštěním hello `az iot hub show-connection-string --name {my hub name}` příkaz.

## <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello na panel Arduino spuštěním hello následující příkazy:

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

příkaz gulp Hello nasadí hello ukázkové aplikace tooyour Arduino panelu. Pak je spuštěna aplikace hello na panel Arduino a samostatná úloha na hostiteli toosend 20 blikání zprávy tooyour Arduino základní desky z služby IoT hub.

Po spuštění ukázkové aplikace hello začne naslouchat toomessages ze služby IoT hub. Mezitím hello gulp úloh odešle několik "blink" zprávy z vaší IoT hub tooyour Arduino panelu. Pro každou zprávu blikání, který hello Tabule obdrží, hello ukázkové aplikace volá hello `blinkLED` funkce tooblink hello Indikátor.

Měli byste vidět hello DIODU jako úloha gulp hello odešle 20 zprávy z vaší IoT hub tooyour Arduino Tabule blink každé dvě sekundy. Hello poslední jedním je zpráva "stop", která ukončí hello spuštění aplikace.

![Ukázkovou aplikaci s gulp příkazu a blink – zprávy][sample-application]

## <a name="summary"></a>Souhrn
Z vaší IoT hub tooyour Arduino Tabule tooblink hello DIODU úspěšně odeslat zprávy. Další úlohou Hello je volitelné: Změna hello zapnout a vypnout chování hello Indikátor.

## <a name="next-steps"></a>Další kroky
[Změnit hello zapnout a vypnout chování hello DIODU][change-the-on-and-off-led-behavior]


<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/repo_structure_arduino.png
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[create-an-azure-function-app-and-storage-account]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/config-arduino.png
[sample-application]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson4/gulp_blink_arduino.png
[change-the-on-and-off-led-behavior]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-change-led-behavior.md