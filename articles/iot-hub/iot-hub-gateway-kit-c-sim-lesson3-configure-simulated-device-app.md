---
title: "Simulované zařízení & brány Azure IoT - Lekce 3: spuštění ukázkové aplikace | Microsoft Docs"
description: "Spuštění ukázkové aplikace simulovaného zařízení k odesílání dat teploty do služby IoT hub"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: data do cloudu
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5d051d99-9749-4150-b3c8-573b0bda9c52
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7df2d730c38a9f715e0fd57b4d436724a5727760
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a>Nakonfigurujte a spusťte ukázkovou aplikaci simulovaného zařízení

## <a name="what-you-will-do"></a>Co provedete

- Naklonujte úložiště ukázka.
- Použijte rozhraní příkazového řádku Azure k získání IoT hub a informace o logické zařízení pro ukázkovou aplikaci simulovaného zařízení. Nakonfigurujte a spusťte ukázkovou aplikaci simulovaného zařízení.

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V tomto článku se dozvíte:

- Postup konfigurace a pak spusťte ukázkovou aplikaci simulovaného zařízení.

## <a name="what-you-need"></a>Co potřebujete

Musí byl úspěšně dokončen

- [Vytvoření služby IoT Hub a registrace zařízení](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a>Naklonujte úložiště ukázkové k hostitelskému počítači

Klonovat úložiště ukázkové, postupujte takto na hostitelském počítači:

1. Otevřete příkazový řádek v systému Windows nebo otevřete terminál v systému macOS nebo Ubuntu.
2. Spusťte následující příkazy:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-the-simulated-device-and-your-nuc"></a>Konfigurace simulované zařízení a vaše NUC

1. Otevřete konfigurační soubor `config.json` ve Visual Studio Code spuštěním následujícího příkazu:

   ```bash
   code config.json
   ```

2. Nahraďte `"has_sensortag": true` s`"has_sensortag": false`

   ![Nemáte zařízení TI SensorTag konfigurace](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. Inicializace konfiguračního souboru spuštěním následujících příkazů:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. Otevřete `config-gateway.json` ve Visual Studio Code spuštěním následujícího příkazu:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. Vyhledejte následující řádek kódu a nahraďte `[device hostname or IP address]` se IP adresa nebo název hostitele Intel NUC.
   ![snímek obrazovky konfigurace brány](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

## <a name="get-the-connection-string-of-your-iot-hub-logical-device"></a>Získat připojovací řetězec logické zařízení IoT hub

Chcete-li získat Azure IoT hub připojovací řetězec logického zařízení, spusťte následující příkaz na hostitelském počítači:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`je název centra IoT, které jste použili. Použít iot brány jako hodnotu `{resource group name}` a použít jako hodnotu mydevice `{device id}` Pokud nebylo změňte hodnotu v lekci 2.

## <a name="configure-the-simulated-device-cloud-upload-sample-application"></a>Nakonfigurujte ukázkovou aplikaci simulovaného zařízení cloudu nahrávání

Pro konfiguraci a spuštění ukázkové aplikace simulovaného zařízení cloudu nahrávání, postupujte podle těchto kroků na hostitelském počítači:

1. Otevřete `config-sensortag.json` ve Visual Studio Code spuštěním následujícího příkazu:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![snímek obrazovky konfigurace sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. Zkontrolujte následující nahrazení v kódu:
   - Nahraďte `[IoT hub name]` názvem IoT hub.
   - Nahraďte `[IoT device connection string]` připojovacím řetězcem logické zařízení IoT hub.

3. Spusťte aplikaci.

   Nasazení a spuštění aplikace tak, že spustíte následující příkaz:

   ```bash
   gulp run
   ```

## <a name="verify-the-sample-application-works"></a>Ověřte ukázkové aplikace pracuje

Teď byste měli vidět výstup podobný tomuto:

![výstupu ukázkové aplikace simulovaného zařízení](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

Aplikace odešle teploty data do služby IoT hub, která trvá po dobu 40 sekund.

## <a name="summary"></a>Souhrn

Úspěšně jste nakonfigurované a spusťte simulované zařízení cloudu nahrávání ukázkové aplikace, který odesílá data do služby IoT hub s simulované zařízení.

## <a name="next-steps"></a>Další kroky
[Čtení zpráv ze služby IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)