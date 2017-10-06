---
title: "aaaUse IoT brány tooconnect tooAzure zařízení IoT Hub | Microsoft Docs"
description: "Zjistěte, jak cloudové toouse Intel NUC jako tooconnect brány IoT TI SensorTag a odesílání senzor data tooAzure IoT Hub v hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Brána IOT připojit toocloud zařízení"
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 418af34bf29992d46b76ae59ef548744808664c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-iot-gateway-tooconnect-things-toohello-cloud---sensortag-tooazure-iot-hub"></a>Použít IoT brány tooconnect věcí toohello cloudu - SensorTag tooAzure IoT Hub

> [!NOTE]
> Než začnete tento kurz, ujistěte se, když jste dokončili [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md). V [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), můžete nastavit zařízení Intel NUC hello jako bránu IoT.

## <a name="what-you-will-learn"></a>Co se dozvíte

Zjistíte, jak toouse IoT brány tooconnect Texas Instruments SensorTag (CC2650STK) tooAzure IoT Hub. Brána IoT Hello odešle teploty a vlhkosti data shromážděná z hello SensorTag tooAzure IoT Hub.

## <a name="what-you-will-do"></a>Co provedete

- Vytvoření služby IoT hub.
- Registrovat zařízení ve hello IoT hub pro hello SensorTag.
- Povolte hello připojení mezi bránou hello IoT a hello SensorTag.
- Spusťte zakázat ukázkové aplikace toosend SensorTag data tooyour IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Kurz [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) dokončit, ve kterém můžete nastavit Intel NUC jako bránu IoT.
- * Aktivní předplatné Azure. Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.
- Klient SSH, který běží v hostitelském počítači. V systému Windows se doporučuje puTTY. Linux a systému macOS již obsahují klientem SSH.
- Hello IP adresa a hello uživatelské jméno a heslo tooaccess hello brány z klienta SSH hello.
- Připojení k Internetu.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> Zde je zaregistrovat toto nové zařízení pro vaše SensorTag

## <a name="enable-hello-connection-between-hello-iot-gateway-and-hello-sensortag"></a>Povolit hello připojení mezi bránou hello IoT a hello SensorTag

V této části provedete hello následující úlohy:

- Získáte adresu MAC hello hello SensorTag pro připojení Bluetooth.
- Navázání připojení Bluetooth z hello IoT brány toohello SensorTag.

### <a name="get-hello-mac-address-of-hello-sensortag-for-bluetooth-connection"></a>Získat adresu MAC hello hello SensorTag pro připojení Bluetooth

1. Na hostitelském počítači hello spusťte hello klient SSH a připojit toohello IoT bránu.
1. Odblokování Bluetooth spuštěním hello následující příkaz:

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. Spusťte službu hello Bluetooth na hello IoT brány a zadejte tooconfigure prostředí Bluetooth hello Bluetooth spuštěním následujících příkazů:

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. Zapnutí řadiče Bluetooth hello spuštěním hello následující příkaz v hello prostředí Bluetooth:

   ```bash
   power on
   ```

   ![Zapnutí v hello IoT bránu s bluetoothctl hello řadič Bluetooth](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. Spustí vyhledávání nedaleko zařízeními Bluetooth spuštěním hello následující příkaz:

   ```bash
   scan on
   ```

   ![Kontrola nedaleko Bluetooth zařízení s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. Stiskněte tlačítko na hello SensorTag párování hello. Hello zelená VEDLA na bliká SensorTag hello.
1. V hello Bluetooth prostředí měli byste vidět hello SensorTag je nalezen. Poznamenejte si adresu MAC hello SensorTag hello. V tomto příkladu je hello adresu MAC hello SensorTag `24:71:89:C0:7F:82`.
1. Vypněte kontrolu hello spuštěním hello následující příkaz:

   ```bash
   scan off
   ```

   ![Ukončí skenování nedaleko Bluetooth zařízení s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-hello-iot-gateway-toohello-sensortag"></a>Navázání připojení Bluetooth z hello IoT brány toohello SensorTag

1. Připojte toohello SensorTag spuštěním hello následující příkaz:

   ```bash
   connect <MAC address>
   ```

   ![Spojte se s bluetoothctl toohello SensorTag](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. Odpojte od hello SensorTag a ukončete prostředí Bluetooth hello spuštěním hello následující příkazy:

   ```bash
   disconnect
   exit
   ```

   ![Odpojení od hello SensorTag s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

Úspěšně jste povolili hello připojení mezi hello SensorTag a hello IoT brány.

## <a name="run-a-ble-sample-application-toosend-sensortag-data-tooyour-iot-hub"></a>Spuštění zakázat ukázkové aplikace toosend SensorTag data tooyour IoT hub

Azure IoT Edge zajišťuje Hello ukázkovou aplikaci Bluetooth nízká energie (Povolit). Ukázková aplikace Hello shromažďuje data z zakázat připojení a odeslat hello data tooyou IoT hub. toorun hello ukázkovou aplikaci, musíte:

1. Nakonfigurujte hello ukázkovou aplikaci.
1. Spusťte hello ukázkovou aplikaci na hello IoT brány.

### <a name="configure-hello-sample-application"></a>Konfigurace hello ukázkové aplikace

1. Složky přejděte toohello hello ukázkovou aplikaci spuštěním hello následující příkaz:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. Otevřete konfigurační soubor hello spuštěním hello následující příkaz:

   ```bash
   vi ble_gateway.json
   ```

1. V hello konfigurační soubor zadejte hello následující hodnoty:

   **IoTHubName**: hello název služby IoT hub.

   **IoTHubSuffix**: získání IoTHubSuffix z hello primární klíč hello zařízení připojovací řetězec, který jste si poznamenali dolů. Zkontrolujte, zda můžete získat hello primární klíč hello zařízení připojovací řetězec, není hello primární klíč připojovací řetězec centra IoT. primární klíč Hello hello zařízení připojovacího řetězce je ve formátu hello `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.

   **Přenos**: hello výchozí hodnota je `amqp`. Tato hodnota se uvádí hello protokol během transpotation. Může to být `http`, `amqp`, nebo `mqtt`.

   **macAddress**: hello adresu MAC hello SensorTag, kterou jste si poznamenali dolů.

   **deviceID**: ID hello zařízení, které jste vytvořili ve službě IoT hub.

   **deviceKey**: primární klíč hello hello zařízení připojovacího řetězce.

   ![Soubor konfigurace dokončení hello hello zakázat ukázkové aplikace](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. Stiskněte klávesu `ESC` a typ `:wq` toosave hello souboru.

### <a name="run-hello-sample-application"></a>Spuštění ukázkové aplikace hello

1. Ujistěte se, zda text hello, které SensorTag je zapnutý.
1. Spusťte následující příkaz hello:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Další kroky

[Použít bránu IoT pro transformaci dat snímačů s hranou Azure IoT](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
