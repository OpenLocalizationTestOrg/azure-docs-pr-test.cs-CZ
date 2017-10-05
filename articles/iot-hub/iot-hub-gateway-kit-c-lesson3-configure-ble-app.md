---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 3: spuštění ukázkové aplikace | Microsoft Docs"
description: "Spuštění ukázkové aplikace zakázat přijímat data z SensorTag zakázat a ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "zakázat aplikace, senzor monitorování aplikace, shromažďování dat senzor, data ze senzorů, data snímače do cloudu"
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
ms.openlocfilehash: f6fa158dbe1d48be7d493efa6217e1e0a759d2f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-run-a-ble-sample-application"></a>Nakonfigurujte a spusťte ukázkové aplikace zakázat

## <a name="what-you-will-do"></a>Co provedete

- Naklonujte úložiště ukázka. 
- Nastavte propojení mezi SensorTag a Intel NUC. 
- Použijte rozhraní příkazového řádku Azure k získání IoT hub a informace o SensorTag pro ukázkovou aplikaci zakázat (Bluetooth nízkou energie). A nakonfigurujte a spusťte ukázkové aplikace zakázat. 

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V tomto článku se dozvíte:

- Postup konfigurace a pak spusťte ukázkovou aplikaci zakázat.

## <a name="what-you-need"></a>Co potřebujete

Musí byl úspěšně dokončen

- [Vytvoření služby IoT hub a zaregistrujte SensorTag](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="clone-the-sample-repository-to-the-host-computer"></a>Naklonujte úložiště ukázkové k hostitelskému počítači

Klonovat úložiště ukázkové, postupujte takto na hostitelském počítači:

1. Otevřete okno příkazového řádku v systému Windows nebo otevřete terminál v systému macOS nebo Ubuntu.
2. Spusťte následující příkazy:

   ```bash
   git clone https://github.com/Azure-samples/iot-hub-c-intel-nuc-gateway-getting-started
   cd iot-hub-c-intel-nuc-gateway-getting-started
   ```

## <a name="set-up-the-connectivity-between-sensortag-and-intel-nuc"></a>Nastavit propojení mezi SensorTag a Intel NUC

Pokud chcete nastavit připojení, postupujte podle těchto kroků v hostitelském počítači:

1. Inicializace konfiguračního souboru spuštěním následujících příkazů:

   ```bash
   cd Lesson3
   npm install
   gulp init
   ```

2. Otevřete `config-gateway.json` ve Visual Studio Code spuštěním následujícího příkazu:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-gateway.json
   ```

3. Vyhledejte následující řádek kódu a nahraďte `[device hostname or IP address]` s IP adresu nebo název hostitele Intel NUC.
   ![snímek obrazovky konfigurace brány](media/iot-hub-gateway-kit-lessons/lesson3/config_gateway.png)

4. Instalace pomocné nástroje na Intel NUC spuštěním následujícího příkazu:

   ```bash
   gulp install-tools
   ```

5. Zapněte SensorTag stisknutím tlačítka napájení jako na následujícím obrázku a zelená DIODU měla blikat.

   ![zapnout Sensortag](media/iot-hub-gateway-kit-lessons/lesson3/turn on_off sensortag.jpg)

6. Kontrolovat zařízení SensorTag spuštěním následujících příkazů:

   ```bash
   gulp discover-sensortag
   ```

7. Testovací připojení mezi SensorTag a Intel NUC spuštěním následujícího příkazu:

   ```bash
   gulp test-connectivity --mac {mac address}
   ```

   Nahraďte `{mac address}` s adresou MAC, který jste získali v předchozím kroku.

## <a name="get-the-connection-string-of-sensortag"></a>Získat připojovací řetězec SensorTag

Chcete-li získat Azure IoT hub připojovací řetězec SensorTag, spusťte následující příkaz na hostitelském počítači:

```bash
az iot device show-connection-string --hub-name {IoT hub name} --device-id mydevice --resource-group iot-gateway
```

`{IoT hub name}`je název centra IoT, které jste použili. Použít iot brány jako hodnotu `{resource group name}` a použít jako hodnotu mydevice `{device id}` Pokud nebylo změňte hodnotu v lekci 2.

## <a name="configure-the-ble-sample-application"></a>Nakonfigurujte ukázkovou aplikaci zakázat

Pro konfiguraci a spuštění ukázkové aplikace zakázat, postupujte podle těchto kroků na hostitelském počítači:

1. Otevřete `config-sensortag.json` ve Visual Studio Code spuštěním následujícího příkazu:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-sensortag.json
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-sensortag.json
   ```

   ![snímek obrazovky konfigurace sensortag](media/iot-hub-gateway-kit-lessons/lesson3/config_sensortag.png)

2. Zkontrolujte následující nahrazení v kódu:
   - Nahraďte `[IoT hub name]` názvem IoT hub, který jste použili.
   - Nahraďte `[IoT device connection string]` připojovacím řetězcem SensorTag, který jste získali.
   - Nahraďte `[device_mac_address]` s adresou MAC SensorTag, který jste získali.

3. Spuštění ukázkové aplikace zakázat.

   Pokud chcete spustit zakázat ukázkovou aplikaci, postupujte takto na hostitelském počítači:

   1. Zapněte SensorTag.

   2. Nasazení a spuštění ukázkové aplikace lit na Intel NUC spuštěním následujícího příkazu:
   
      ```bash
      gulp run
      ```

## <a name="verify-that-the-ble-sample-application-works"></a>Ověřte, že funguje zakázat ukázkové aplikace

Teď byste měli vidět výstup takto:

![Zakázat ukázkové aplikace výstup](media/iot-hub-gateway-kit-lessons/lesson3/BLE_running.png)

Ukázkovou aplikaci udržuje shromažďování dat teploty a odeslaných do služby IoT hub. Ukázkové aplikace automaticky ukončí po odeslání 40 sekund.

## <a name="summary"></a>Souhrn

Úspěšně jste nastavit propojení mezi SensorTag a Intel NUC a spusťte zakázat ukázkovou aplikaci, která shromažďuje a odesílá data z SensorTag do služby IoT hub. Jste připravení zjistěte, jak ověřit, že služby IoT hub obdržel data.

## <a name="next-steps"></a>Další kroky
[Čtení zpráv ze služby IoT Hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)