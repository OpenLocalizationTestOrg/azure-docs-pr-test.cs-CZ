---
title: "Simulované zařízení & brány Azure IoT - Lekce 3: spuštění ukázkové aplikace | Microsoft Docs"
description: "Spustit rozbočovači simulované zařízení ukázkové aplikace toosend teploty data tooyour IoT"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: toocloud dat
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
ms.openlocfilehash: bc2c97919e95e4e3977a8b6ac75162bf2b5017be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-simulated-device-sample-app"></a>Nakonfigurujte a spusťte ukázkovou aplikaci simulovaného zařízení

## <a name="what-you-will-do"></a>Co provedete

- Klon hello Ukázka úložiště.
- Použijte tooget hello rozhraní příkazového řádku Azure IoT hub a informace o logické zařízení pro ukázkovou aplikaci simulovaného zařízení. Nakonfigurujte a spusťte ukázkové aplikace hello simulované zařízení.

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V tomto článku se dozvíte:

- Jak tooconfigure a spuštění hello simulované zařízení ukázkovou aplikaci.

## <a name="what-you-need"></a>Co potřebujete

Musí byl úspěšně dokončen

- [Vytvoření služby IoT Hub a registrace zařízení](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a>Klon hello Ukázka úložiště toohello hostitelském počítači

tooclone hello Ukázka úložiště, postupujte podle těchto kroků v hostitelském počítači hello:

1. Otevřete příkazový řádek v systému Windows nebo otevřete terminál v systému macOS nebo Ubuntu.
2. Spusťte následující příkazy hello:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="configure-hello-simulated-device-and-your-nuc"></a>Konfigurace hello simulované zařízení a vaše NUC

1. Konfigurační soubor otevřete hello `config.json` ve Visual Studio Code spuštěním hello následující příkaz:

   ```bash
   code config.json
   ```

2. Nahraďte `"has_sensortag": true` s`"has_sensortag": false`

   ![Nemáte zařízení TI SensorTag konfigurace](media/iot-hub-gateway-kit-lessons/lesson3/config_no_sensortag.png)

3. Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

4. Otevřete `config-gateway.json` ve Visual Studio Code spuštěním hello následující příkaz:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

5. Vyhledejte následující řádek kódu hello a nahraďte `[device hostname or IP address]` se IP adresa nebo název hostitele hello Intel NUC.
   ![snímek obrazovky konfigurace brány](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

## <a name="get-hello-connection-string-of-your-iot-hub-logical-device"></a>Získat připojovací řetězec hello logické zařízení IoT hub

tooget hello Azure IoT hub připojovací řetězec logické zařízení, spusťte následující příkaz v hostitelském počítači hello hello:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`je název centra IoT hello, který jste použili. Použít iot brány jako hodnotu hello `{resource group name}` a použijte mydevice jako hodnota hello `{device id}` Pokud hodnota hello v lekci 2 nebyla změněna.

## <a name="configure-hello-simulated-device-cloud-upload-sample-application"></a>Konfigurace hello simulované zařízení cloudu nahrávání ukázkovou aplikaci

tooconfigure a spuštění hello simulované zařízení cloud nahrát ukázková aplikace, postupujte podle těchto kroků v hostitelském počítači hello:

1. Otevřete `config-sensortag.json` ve Visual Studio Code spuštěním hello následující příkaz:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![snímek obrazovky konfigurace sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_simulated_device.png)

2. Ujistěte se, hello následující nahrazení v hello kódu:
   - Nahraďte `[IoT hub name]` názvem hello IoT hub.
   - Nahraďte `[IoT device connection string]` s hello připojovací řetězec logické zařízení IoT hub.

3. Spusťte aplikaci hello.

   Nasazení a spuštění aplikace hello spuštěním hello následující příkaz:

   ```bash
   gulp run
   ```

## <a name="verify-hello-sample-application-works"></a>Ověřte hello ukázkové aplikace funguje

Teď byste měli vidět výstup podobný hello následující:

![výstupu ukázkové aplikace simulovaného zařízení](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_simudev.png)

aplikace Hello odešle teploty data tooyour IoT hub, která trvá po dobu 40 sekund.

## <a name="summary"></a>Souhrn

Úspěšně jste nakonfigurovali a spuštění hello simulované zařízení cloudu nahrávání ukázkovou aplikaci, který odesílá data tooyour IoT hub s simulované zařízení.

## <a name="next-steps"></a>Další kroky
[Čtení zpráv ze služby IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)