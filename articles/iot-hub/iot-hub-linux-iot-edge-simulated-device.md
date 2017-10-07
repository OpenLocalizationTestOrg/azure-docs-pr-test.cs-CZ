---
title: "aaaSimulate zařízení s Azure IoT okraj (Linux) | Microsoft Docs"
description: "Jak toouse Azure IoT Edge na Linux toocreate simulované zařízení, odesílá telemetrická data prostřednictvím okraj IoT Centrum IoT tooan brány."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 11e7bf28-ee3d-48d6-a386-eb506c7a31cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: 168fb8eda8671d02c63073bdf36dfcd88b397fe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-toosend-device-to-cloud-messages-with-a-simulated-device-linux"></a>Použití Azure IoT Edge toosend zařízení cloud zprávy s simulované zařízení (Linux)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-toorun-hello-sample"></a>Jak toorun hello ukázka

Hello **build.sh** skript generuje jeho výstup v hello **sestavení** složky ve vaší místní kopii hello **iot hranou** úložiště. Tento výstup zahrnuje hello čtyři IoT Edge moduly používané v této ukázce.

Hello sestavení skriptu míst:

* **liblogger.so** v hello **sestavení nebo moduly nebo protokolovacího nástroje** složky.
* **libiothub.so** v hello **sestavení nebo moduly nebo iothub** složky.
* **lib\_identity\_map.so** v hello **sestavení nebo moduly nebo identitymap** složky.
* **libsimulated\_device.so** v hello **sestavení, moduly nebo simulated\_zařízení** složky.

Použít tyto cesty pro hello **modulu cesta** hodnoty, jak je znázorněno v následující soubor JSON nastavení hello:

Hello simulated\_zařízení\_cloudu\_nahrát\_ukázka proces trvá hello konfigurační soubor JSON tooa cestu jako argument příkazového řádku. Hello následující příklad souboru JSON je součástí sady SDK úložiště hello na **ukázky\\simulované\_zařízení\_cloudu\_nahrát\_ukázka\\src\\ Simulované\_zařízení\_cloudu\_nahrát\_ukázka\_lin.json**. Tato konfigurace soubor funguje jako je nezměníte hello vytvořit skript tooplace hello IoT Edge moduly nebo ukázkové spustitelné soubory v jiné než výchozí umístění.

> [!NOTE]
> Hello modulu cest relativní toohello directory z kterém spouštíte hello simulated\_zařízení\_cloudu\_nahrát\_ukázka spustitelný soubor, není hello adresáře, kde se nachází hello spustitelný soubor. Výchozí konfigurační soubor JSON ukázka Hello toowriting too'deviceCloudUploadGatewaylog.log se v aktuální pracovní adresář.

V textovém editoru otevřete soubor hello **ukázky/simulated\_zařízení\_cloudu\_nahrát\_ukázkové, src/simulated\_zařízení\_cloudu\_nahrát\_lin.json** ve vaší místní kopii hello **iot hranou** úložiště. Tento soubor konfiguruje hello IoT Edge moduly v bráně ukázka hello:

* Hello **IoTHub** modulu připojí tooyour IoT hub. Můžete ji nakonfigurovat toosend data tooyour IoT hub. Konkrétně sadu hello **IoTHubName** toohello název služby IoT hub hodnoty a nastavte hello **IoTHubSuffix** hodnota příliš**azure devices.net**. Sada hello **přenosu** tooone hodnotu z: **HTTP**, **AMQP**, nebo **MQTT**. V současné době pouze **HTTP** sdílí jedno připojení protokolu TCP pro všechny zprávy zařízení. Pokud nastavíte hodnotu hello příliš**AMQP**, nebo **MQTT**, brány hello udržuje samostatné TCP připojení tooIoT rozbočovače pro každé zařízení.
* Hello **mapování** modulu mapování adresy MAC hello ID zařízení IoT Hub tooyour vaše Simulovaná zařízení. Ujistěte se, že **deviceId** ID hello shody hodnoty hello dvě zařízení, které jste přidali tooyour IoT hub a že hello **deviceKey** hodnoty obsahovat hello klíče ze dvou zařízení.
* Hello **BLE1** a **BLE2** moduly jsou hello simulované zařízení. Všimněte si, jak jejich MAC adres shodu hello adresy v hello **mapování** modulu.
* Hello **Protokolovač** modulu protokoly souboru tooa aktivity brány.
* Hello **modulu cesta** hodnoty v příkladu hello předpokládá spustit ukázkový hello z hello **sestavení** složky ve vaší místní kopii hello **iot hranou** úložiště.
* Hello **odkazy** pole v dolní části hello souboru JSON hello připojí hello **BLE1** a **BLE2** moduly toohello **mapování** modulu a hello **mapování** modulu toohello **IoTHub** modulu. Také zajistí, že všechny zprávy v hello protokolu **Protokolovač** modulu.

```json
{
    "modules": [
        {
            "name": "IotHub",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/iothub/libiothub.so"
            }
            },
            "args": {
              "IoTHubName": "<<insert here IoTHubName>>",
              "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
              "Transport": "HTTP"
            }
          },
        {
            "name": "mapping",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/identitymap/libidentity_map.so"
            }
            },
            "args": [
              {
                "macAddress": "01:01:01:01:01:01",
                "deviceId": "<<insert here deviceId>>",
                "deviceKey": "<<insert here deviceKey>>"
              },
              {
                "macAddress": "02:02:02:02:02:02",
                "deviceId": "<<insert here deviceId>>",
                "deviceKey": "<<insert here deviceKey>>"
              }
            ]
          },
        {
            "name": "BLE1",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/simulated_device/libsimulated_device.so"
            }
            },
            "args": {
              "macAddress": "01:01:01:01:01:01"
            }
          },
        {
            "name": "BLE2",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/simulated_device/libsimulated_device.so"
            }
            },
            "args": {
              "macAddress": "02:02:02:02:02:02"
            }
          },
        {
            "name": "Logger",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args": {
              "filename": "deviceCloudUploadGatewaylog.log"
            }
          }
    ],
    "links": [
        {
            "source": "*",
            "sink": "Logger"
        },
        {
            "source": "BLE1",
            "sink": "mapping"
        },
        {
            "source": "BLE2",
            "sink": "mapping"
        },
        {
            "source": "mapping",
            "sink": "IotHub"
        }
    ]
}
```

Uložte změny hello provedené toohello konfigurační soubor.

Ukázka toorun hello:

1. Ve vašem prostředí, přejděte toohello **iot-edge nebo sestavení** složky.
2. Spusťte následující příkaz hello:
   
    ```sh
    ./samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ../samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```
3. Můžete použít hello [explorer zařízení] [ lnk-device-explorer] nebo [iothub-explorer] [ lnk-iothub-explorer] nástroj toomonitor hello zprávy, které Centrum IoT získává z hello brána. Například pomocí iothub-explorer můžete monitorovat zpráv typu zařízení cloud pomocí hello následující příkaz:

    ```sh
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Další kroky

toogain rozsáhlejšími znalostmi Azure IoT okraj a experimentu s příklady kódu navštívit hello následující kurzy developer a prostředky:

* [Odesílání zpráv typu zařízení cloud z fyzického zařízení s Azure IoT Edge][lnk-physical-device]
* [Azure IoT Edge][lnk-iot-edge]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Zabezpečení řešení IoT z hello pozadí][lnk-securing]

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
