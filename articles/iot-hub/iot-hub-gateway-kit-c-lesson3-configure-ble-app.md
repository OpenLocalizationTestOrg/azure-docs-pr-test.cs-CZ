---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 3: spuštění ukázkové aplikace | Microsoft Docs"
description: "Spusťte zakázat ukázková aplikace tooreceive data z SensorTag zakázat a ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Povolit aplikaci, senzor monitorování aplikace, shromažďování dat snímačů, dat ze senzorů, toocloud data snímačů"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: b33e53a1-1df7-4412-ade1-45185aec5bef
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4a8acdeadd402ffc82d3b766e1ec03a77ddcebb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a>Nakonfigurujte a spusťte ukázkové aplikace zakázat

## <a name="what-you-will-do"></a>Co provedete

- Klon hello Ukázka úložiště. 
- Nastavte hello propojení mezi SensorTag a Intel NUC. 
- Použijte tooget hello rozhraní příkazového řádku Azure IoT hub a SensorTag informace pro ukázkovou aplikaci zakázat (Bluetooth nízkou energie). A nakonfigurujte a spusťte hello zakázat ukázkovou aplikaci. 

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V tomto článku se dozvíte:

- Jak tooconfigure a spuštění hello zakázat ukázkové aplikace.

## <a name="what-you-need"></a>Co potřebujete

Musí byl úspěšně dokončen

- [Vytvoření služby IoT hub a zaregistrujte SensorTag](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-hello-sample-repository-toohello-host-computer"></a>Klon hello Ukázka úložiště toohello hostitelském počítači

tooclone hello Ukázka úložiště, postupujte podle těchto kroků v hostitelském počítači hello:

1. Otevřete okno příkazového řádku v systému Windows nebo otevřete terminál v systému macOS nebo Ubuntu.
2. Spusťte následující příkazy hello:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-hello-connectivity-between-sensortag-and-intel-nuc"></a>Nastavit hello propojení mezi SensorTag a Intel NUC

tooset hello připojením pomocí těchto kroků v hostitelském počítači hello:

1. Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. Otevřete `config-gateway.json` ve Visual Studio Code spuštěním hello následující příkaz:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. Vyhledejte následující řádek kódu hello a nahraďte `[device hostname or IP address]` s hello IP adresu nebo název hostitele Intel NUC.
   ![snímek obrazovky konfigurace brány](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

4. Instalace pomocné nástroje na Intel NUC spuštěním hello následující příkaz:

   ```bash
   gulp install-tools
   ```

5. Zapněte SensorTag stisknutím tlačítka napájení hello jako hello následující obrázek a měla blikat DIODU hello zelená.

   ![zapnout Sensortag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. Kontrolovat zařízení SensorTag spuštěním hello následující příkazy:

   ```bash
   gulp discover-sensortag
   ```

7. Otestujte připojení hello mezi hello SensorTag a Intel NUC spuštěním hello následující příkaz:

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   Nahraďte `{mac address}` s hello adresu MAC, který jste získali v předchozím kroku hello.

## <a name="get-hello-connection-string-of-sensortag"></a>Získat připojovací řetězec hello SensorTag

tooget hello Azure IoT hub připojovací řetězec SensorTag, spusťte následující příkaz v hostitelském počítači hello hello:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`je název centra IoT hello, který jste použili. Použít iot brány jako hodnotu hello `{resource group name}` a použijte mydevice jako hodnota hello `{device id}` Pokud hodnota hello v lekci 2 nebyla změněna.

## <a name="configure-hello-ble-sample-application"></a>Konfigurace hello zakázat ukázkové aplikace

tooconfigure a spuštění hello zakázat ukázkovou aplikaci, pomocí těchto kroků v hostitelském počítači hello:

1. Otevřete `config-sensortag.json` ve Visual Studio Code spuštěním hello následující příkaz:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![snímek obrazovky konfigurace sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. Ujistěte se, hello následující nahrazení v hello kódu:
   - Nahraďte `[IoT hub name]` s hello název centra IoT, které jste použili.
   - Nahraďte `[IoT device connection string]` s hello připojovací řetězec SensorTag, který jste získali.
   - Nahraďte `[device_mac_address]` s hello adresu MAC hello SensorTag, který jste získali.

3. Spusťte hello zakázat ukázkovou aplikaci.

   toorun hello zakázat ukázkovou aplikaci, pomocí těchto kroků v hostitelském počítači hello:

   1. Zapněte SensorTag.

   2. Nasazení a spuštění hello zakázat ukázkovou aplikaci na Intel NUC spuštěním hello následující příkaz:
   
      ```bash
      gulp run
      ```

## <a name="verify-that-hello-ble-sample-application-works"></a>Ověřte, že funguje hello zakázat ukázkové aplikace

Teď byste měli vidět výstup jako hello následující:

![Zakázat ukázkové aplikace výstup](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

Ukázková aplikace Hello udržuje shromažďování dat teploty a ji odeslala tooyour IoT hub. Ukázková aplikace Hello automaticky ukončí po odeslání 40 sekund.

## <a name="summary"></a>Souhrn

Úspěšně jste nastavit hello propojení mezi SensorTag a Intel NUC a spusťte zakázat ukázkovou aplikaci, která shromažďuje a odesílá data ze služby IoT hub SensorTag tooyour. Jste připravené toolearn jak tooverify, který přijal služby IoT hub hello data.

## <a name="next-steps"></a>Další kroky
[Čtení zpráv ze služby IoT Hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)