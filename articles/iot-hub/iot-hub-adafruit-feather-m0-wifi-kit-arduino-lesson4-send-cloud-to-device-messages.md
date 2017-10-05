---
title: "Připojení k Azure IoT - Lekce 4 Arduino (C): Cloud zařízení | Microsoft Docs"
description: "Ukázková aplikace spustí na Adafruit prolnutí M0 Wi-Fi a monitorování příchozích zpráv ze služby IoT hub. Nová úloha gulp odesílá zprávy Adafruit prolnutí M0 Wi-Fi ze služby IoT hub blikání Indikátor."
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
ms.openlocfilehash: b4f4c1d86b10b64c104ac812d1f650d723322be8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a>Spuštění ukázkové aplikace pro příjem zpráv typu cloud zařízení
V tomto článku nasazení ukázkové aplikace na vaší kartě Adafruit prolnutí M0 Wi-Fi Arduino.

Ukázkovou aplikaci monitoruje příchozích zpráv ze služby IoT hub. Můžete taky spustit úlohu gulp ve vašem počítači k odesílání zpráv do vaší karty Arduino ze služby IoT hub. Zprávy přijetí ukázkovou aplikaci bliká Indikátor. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-do"></a>Co provedete
* Připojte ukázkovou aplikaci do služby IoT hub.
* Nasazení a spuštění ukázkové aplikace.
* Odesílat zprávy ze služby IoT hub panel Arduino blikání Indikátor.

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Postup sledování příchozích zpráv ze služby IoT hub.
* Postupy pro odesílání zpráv typu cloud zařízení ze služby IoT hub pro panel Arduino.

## <a name="what-you-need"></a>Co potřebujete
* Panel Arduino, nastavte pro použití. Zjistěte, jak nastavit panel Arduino, najdete v tématu [konfigurace zařízení][configure-your-device].
* Služby IoT hub, který se vytvoří ve vašem předplatném Azure. Informace o vytvoření služby IoT hub, najdete v tématu [vytvoření služby Azure IoT Hub][create-your-azure-iot-hub].

## <a name="connect-the-sample-application-to-your-iot-hub"></a>Připojit ukázkovou aplikaci do služby IoT hub

1. Ujistěte se, že jste ve složce úložišti `iot-hub-c-feather-m0-getting-started`.

   Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:

   ```bash
   cd Lesson4
   code .
   ```

   Upozornění `app.ino` v soubor `app` podsložky. `app.ino` Soubor je soubor zdroj klíče, který obsahuje kód ke sledování příchozích zpráv ze služby IoT hub. `blinkLED` Funkce bliká Indikátor.

   ![Struktura úložišti v ukázkové aplikace][repo-structure]

2. Získání sériového portu zařízení pomocí rozhraní příkazového řádku zjišťování zařízení:

   ```bash
   devdisco list --usb
   ```

   Měli byste zobrazený výstup, který je podobný následujícímu a najít usb COM port pro panel Arduino:

   ![Zjišťování zařízení][device-discovery]

3. Otevřete soubor `config.json` ve složce lekce a vstupní hodnotu nalezen číslo portu COM:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > Pro port COM, na platformě Windows má formát `COM1, COM2, ...`. V systému macOS nebo Ubuntu, bude začínat `/dev/`.

4. Inicializace konfiguračního souboru spuštěním následujících příkazů:

   ```bash
   # For Windows command prompt
   npm install
   gulp init
   gulp install-tools
   ```

5. Zkontrolujte následující nahrazení v `config-arduino.json` souboru:

   Pokud jste dokončili kroky v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] v tomto počítači se dědí všechny konfigurace, takže můžete přeskočit na krok úlohy nasazení a spuštění ukázkové aplikace. Pokud jste dokončili kroky v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] na jiném počítači, budete muset nahraďte zástupné symboly v `config-arduino.json` souboru. `config-arduino.json` Soubor je v podsložce domovskou složku.

   ![Obsah souboru config arduino.json][config-arduino-json]

   * Nahraďte **[Wi-Fi SSID]** s vaší SSID Wi-Fi, který připojení k Internetu.
   * Nahraďte **[Wi-Fi heslo]** pomocí hesla Wi-Fi. Odeberte řetězec, pokud Wi-Fi nevyžaduje heslo.
   * Nahraďte **[řetězec připojení zařízení IoT]** připojovacím řetězcem zařízení, kterou můžete získat spuštěním `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` příkaz.
   * Nahraďte **[IoT hub, připojovací řetězec]** s IoT hub připojovací řetězec, který získáte spuštěním `az iot hub show-connection-string --name {my hub name}` příkaz.

## <a name="deploy-and-run-the-sample-application"></a>Nasazení a spuštění ukázkové aplikace
Nasazení a spuštění ukázkové aplikace na panel Arduino spuštěním následujících příkazů:

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

Příkaz gulp nasadí ukázkovou aplikaci pro panel Arduino. Pak spustí aplikaci na panel Arduino a samostatná úloha v hostitelském počítači odesílat zprávy 20 blikání panel Arduino ze služby IoT hub.

Po spuštění ukázkové aplikace, začne naslouchání zpráv ze služby IoT hub. Úloha gulp mezitím, odešle do panel Arduino několik "blink" zpráv ze služby IoT hub. Pro každou blikání zprávu, která přijímá panelu, volá ukázkovou aplikaci `blinkLED` funkce blikání Indikátor.

Jako úlohu gulp odešle 20 zpráv ze služby IoT hub panel Arduino, měli byste vidět blikání DIODU každé dvě sekundy. Poslední je "stop" zprávy, která zabrání spuštění aplikace.

![Ukázkovou aplikaci s gulp příkazu a blink – zprávy][sample-application]

## <a name="summary"></a>Souhrn
Úspěšně jste odeslali zprávy ze služby IoT hub do vaší karty Arduino blikání Indikátor. Dalším krokem je volitelné: Změna zapnuto a vypnuto chování Indikátor.

## <a name="next-steps"></a>Další kroky
[Změnit zapnuto a vypnuto chování Indikátor][change-the-on-and-off-led-behavior]


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