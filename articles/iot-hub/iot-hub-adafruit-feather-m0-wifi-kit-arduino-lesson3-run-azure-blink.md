---
title: "Connect Arduino (C) tooAzure IoT - Lekce 3: spuštění ukázkových | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace tooAdafruit prolnutí M0 WiFi, která odesílá zprávy tooyour IoT hub a bliká DIODU hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "cloudové služby IOT, arduino odesílat data toocloud"
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
ms.openlocfilehash: ddca015a3655f8a1a9de2a00e718ec0d28a5affb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a>Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud
## <a name="what-you-will-do"></a>Co provedete
Tento článek vám ukáže, jak toodeploy a spusťte ukázkové aplikace na vašem Adafruit prolnutí M0 Wi-Fi Arduino board že odešle zprávy tooyour IoT hub.

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
Se dozvíte, jak toouse hello gulp toodeploy nástroje a spusťte hello ukázkové Arduino aplikace na vaší kartě Arduino.

## <a name="what-you-need"></a>Co potřebujete
* Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a úložiště účtu úložiště a tooprocess IoT hub zprávy][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Získat IoT hub a zařízení připojovací řetězce
Hello zařízení připojovací řetězec je použité tooconnect Arduino Tabule tooyour IoT hub. Hello IoT hub připojovací řetězec je použité tooconnect vaše IoT hub toohello zařízení identita, která představuje váš Arduino board hello IoT hub.

* Seznam všech centra IoT ve vaší skupině prostředků tak, že spustíte následující příkaz rozhraní příkazového řádku Azure hello:

```bash
az iot hub list -g iot-sample --query [].name
```

Použití `iot-sample` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.

* Spuštěním následujícího příkazu příkazového řádku Azure CLI hello získáte hello IoT hub, připojovací řetězec:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`je název hello, které jste zadali při vytvoření služby IoT hub a registraci Arduino panel.

* Získáte hello zařízení připojovací řetězec tak, že spustíte následující příkaz hello:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id mym0wifi
```

Použití `mym0wifi` jako hodnota hello `{device id}` Pokud hodnota hello nebyla změněna.
## <a name="configure-hello-device-connection"></a>Nakonfigurujte připojení zařízení hello
tooconfigure hello připojení zařízení, postupujte takto:

1. Získejte hello sériového portu zařízení hello s hello zařízení zjišťování rozhraní příkazového řádku:

   ```bash
   devdisco list --usb
   ```

   Měli byste zobrazený výstup, který je podobný toohello následující a najít hello usb COM port pro panel Arduino:

   ![Zjišťování zařízení][device-discovery]

2. Soubor otevřete hello `config.json` v hello lekce složky a přidat hello hodnotu hello najít číslo portu COM:

   ```json
   {
       "device_port" : "COM1"
   }
   ```

   ![Config.JSON][config-json]

   > [!NOTE]
   > Pro port hello COM na platformě Windows má formát hello `COM1, COM2, ...`. V systému macOS nebo Ubuntu začíná `/dev/`.

3. Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:

   ```bash
   npm install
   gulp init
   gulp install-tools
   ```
4. Otevřete hello zařízení konfigurační soubor `config-arduino.json` ve Visual Studio Code spuštěním hello následující příkaz:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```

   ![Konfigurace arduino.json][config-arduino-json]

5. Ujistěte se, hello následující nahrazení v hello `config-arduino.json` souboru:

   * Nahraďte **[Wi-Fi SSID]** s vaší SSID Wi-Fi, který připojený toohello Internetu.
   * Nahraďte **[Wi-Fi heslo]** pomocí hesla Wi-Fi. Odeberte hello řetězec, pokud Wi-Fi nevyžaduje heslo.
   * Nahraďte **[řetězec připojení zařízení IoT]** s hello `device connection string` jste získali.
   * Nahraďte **[IoT hub, připojovací řetězec]** s hello `iot hub connection string` jste získali.

   > [!NOTE]
   > Nepotřebujete `azure_storage_connection_string` v tomto článku. Dodržte jako je.

## <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello na panel Arduino spuštěním hello následující příkaz:

```bash
gulp run
# You can monitor hello serial port by running listen task:
gulp listen

# Or you can combine above two gulp tasks into one:
gulp run --listen
```

> [!NOTE]
> Hello výchozí gulp úloha spustí `install-tools` a `run` úlohy postupně. Pokud jste [hello blikání aplikace nasazená][deployed-the-blink-app], jste spustili tyto úlohy samostatně.

## <a name="verify-that-hello-sample-application-works"></a>Ověřte, že funguje hello ukázkové aplikace
Měli byste vidět integrovaného blikat DIODU hello GPIO #0 každé dvě sekundy. Pokaždé, když hello DIODU bliká, hello ukázkové aplikace odešle zpráva tooyour IoT hub a ověřuje, zda je že tento hello byla úspěšně odeslána zpráva tooyour IoT hub. Kromě toho každou zprávu přijme hello IoT hub je vytištěna v okně konzoly hello. Ukázková aplikace Hello automaticky ukončí po odeslání 20 zpráv.

![Ukázkovou aplikaci s odeslané a přijaté zprávy][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Souhrn
Jste nasazení a spusťte hello nové blikání ukázkovou aplikaci ve službě Arduino Tabule toosend zpráv typu zařízení cloud tooyour IoT hub. Vaše zprávy se teď sledovat, jak jsou napsány toohello účet úložiště.

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