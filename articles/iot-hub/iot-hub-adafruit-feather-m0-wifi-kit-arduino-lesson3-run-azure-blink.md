---
title: "Connect Arduino (C) k Azure IoT - Lekce 3: spuštění ukázkových | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace pro Adafruit prolnutí M0 WiFi, která odesílá zprávy do služby IoT hub a bliká Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "cloudové služby IOT, arduino odesílat data do cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 92cce319-2b17-4c9b-889d-deac959e3e7c
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0c17fe74dbd78abca955f7789a1674ed6333367f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud
## <a name="what-you-will-do"></a>Co provedete
Tento článek vám ukáže, jak nasadit a spustit ukázkovou aplikaci na vaší kartě Adafruit prolnutí M0 Wi-Fi Arduino, která odesílá zprávy do služby IoT hub.

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
Se dozvíte, jak používat nástroj gulp k nasazení a spuštění ukázkové aplikace Arduino na panel Arduino.

## <a name="what-you-need"></a>Co potřebujete
* Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a účet úložiště pro zpracování a ukládání zpráv centra IoT][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Získat IoT hub a zařízení připojovací řetězce
Připojovací řetězec zařízení slouží k připojení vaší karty Arduino do služby IoT hub. IoT hub připojovací řetězec se používá pro připojení služby IoT hub pro identitu zařízení, která představuje panel Arduino ve službě IoT hub.

* Seznam všech centra IoT ve vaší skupině prostředků spuštěním následujícího příkazu příkazového řádku Azure CLI:

```bash
az iot hub list -g iot-sample --query [].name
```

Použití `iot-sample` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu.

* Spuštěním následujícího příkazu příkazového řádku Azure CLI získáte připojovací řetězec centra IoT:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`je název, který jste zadali při vytvoření služby IoT hub a registraci Arduino panel.

* Získáte připojovací řetězec zařízení tak, že spustíte následující příkaz:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

Použití `mym0wifi` jako hodnotu `{device id}` Pokud nebylo změňte hodnotu.
## <a name="configure-the-device-connection"></a>Nakonfigurujte připojení zařízení
Konfigurace připojení zařízení, postupujte takto:

1. Získání sériového portu zařízení pomocí rozhraní příkazového řádku zjišťování zařízení:

   ```bash
   devdisco list --usb
   ```

   Měli byste zobrazený výstup, který je podobný následujícímu a najít usb COM port pro panel Arduino:

   ![Zjišťování zařízení][device-discovery]

2. Otevřete soubor `config.json` ve složce lekce a přidejte hodnotu nalezen číslo portu COM:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > Pro port COM, na platformě Windows má formát `COM1, COM2, ...`. V systému macOS nebo Ubuntu začíná `/dev/`.

3. Inicializace konfiguračního souboru spuštěním následujících příkazů:

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. Otevřete konfigurační soubor zařízení `config-arduino.json` ve Visual Studio Code spuštěním následujícího příkazu:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![Konfigurace arduino.json][config-arduino-json]

5. Zkontrolujte následující nahrazení v `config-arduino.json` souboru:

   * Nahraďte **[Wi-Fi SSID]** s vaší SSID Wi-Fi, který připojení k Internetu.
   * Nahraďte **[Wi-Fi heslo]** pomocí hesla Wi-Fi. Odeberte řetězec, pokud Wi-Fi nevyžaduje heslo.
   * Nahraďte **[řetězec připojení zařízení IoT]** s `device connection string` jste získali.
   * Nahraďte **[IoT hub, připojovací řetězec]** s `iot hub connection string` jste získali.

   > [!NOTE]
   > Nepotřebujete `azure_storage_connection_string` v tomto článku. Dodržte jako je.

## <a name="deploy-and-run-the-sample-application"></a>Nasazení a spuštění ukázkové aplikace
Nasazení a spuštění ukázkové aplikace na panel Arduino spuštěním následujícího příkazu:

```bash
gulp run
# You can monitor the serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> Spuštění úlohy gulp výchozí `install-tools` a `run` úlohy postupně. Když jste [blikání aplikace nasazená][deployed-the-blink-app], jste spustili tyto úlohy samostatně.

## <a name="verify-that-the-sample-application-works"></a>Ověřte, že funguje ukázkové aplikace
Měli byste vidět GPIO #0 integrovaného DIODU blikat, které každý dvou sekund. Pokaždé, když DIODU bliká, ukázkové aplikace odešle zprávu do služby IoT hub a ověřuje, že zpráva byla úspěšně odeslána do služby IoT hub. Kromě toho každou zprávu přijme služby IoT hub je vytištěna v okně konzoly. Ukázkové aplikace automaticky ukončí po odeslání 20 zpráv.

![Ukázkovou aplikaci s odeslané a přijaté zprávy][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Souhrn
Jste nasazení a spuštění nové aplikace ukázka blikání na panel Arduino k odesílání zpráv typu zařízení cloud do služby IoT hub. Zprávy si můžete nyní sledovat, jak se zapisují na účet úložiště.

## <a name="next-steps"></a>Další kroky
[Čtení zprávy uchovávané v úložišti Azure][read-messages-persisted-in-azure-storage]
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[device-discovery]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/device_discovery.png
[config-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/vscode-config-mac.png
[config-arduino-json]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/config-arduino.png
[deployed-the-blink-app]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_run_arduino.png
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md