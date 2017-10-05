---
title: "Simulovat zařízení s Azure IoT hranou (Linux) | Microsoft Docs"
description: "Jak používat Azure IoT Edge v systému Linux k vytvoření simulovaného zařízení, které odesílá telemetrická data prostřednictvím IoT vstupní brána do služby IoT hub."
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
ms.openlocfilehash: 5349960373ae6815862c5f79a69dd6d5d9d624ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-linux"></a><span data-ttu-id="83a36-103">Azure IoT Edge používat k odesílání zpráv typu zařízení cloud s simulované zařízení (Linux)</span><span class="sxs-lookup"><span data-stu-id="83a36-103">Use Azure IoT Edge to send device-to-cloud messages with a simulated device (Linux)</span></span>

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a><span data-ttu-id="83a36-104">Spuštění ukázky</span><span class="sxs-lookup"><span data-stu-id="83a36-104">How to run the sample</span></span>

<span data-ttu-id="83a36-105">Skript **build.sh** generuje výstup ve složce **build** v místní kopii úložiště **iot-edge**.</span><span class="sxs-lookup"><span data-stu-id="83a36-105">The **build.sh** script generates its output in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="83a36-106">Tento výstup zahrnuje čtyři IoT Edge moduly používané v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="83a36-106">This output includes the four IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="83a36-107">Sestavení skriptu místa:</span><span class="sxs-lookup"><span data-stu-id="83a36-107">The build script places the:</span></span>

* <span data-ttu-id="83a36-108">**liblogger.so** v **sestavení nebo moduly nebo protokolovacího nástroje** složky.</span><span class="sxs-lookup"><span data-stu-id="83a36-108">**liblogger.so** in the **build/modules/logger** folder.</span></span>
* <span data-ttu-id="83a36-109">**libiothub.so** v **sestavení nebo moduly nebo iothub** složky.</span><span class="sxs-lookup"><span data-stu-id="83a36-109">**libiothub.so** in the **build/modules/iothub** folder.</span></span>
* <span data-ttu-id="83a36-110">**lib\_identity\_map.so** v **sestavení nebo moduly nebo identitymap** složky.</span><span class="sxs-lookup"><span data-stu-id="83a36-110">**lib\_identity\_map.so** in the **build/modules/identitymap** folder.</span></span>
* <span data-ttu-id="83a36-111">**libsimulated\_device.so** v **sestavení, moduly nebo simulated\_zařízení** složky.</span><span class="sxs-lookup"><span data-stu-id="83a36-111">**libsimulated\_device.so** in the **build/modules/simulated\_device** folder.</span></span>

<span data-ttu-id="83a36-112">Použít tyto cesty pro **modulu cesta** hodnoty, jak je znázorněno v následujícím nastavení souboru JSON:</span><span class="sxs-lookup"><span data-stu-id="83a36-112">Use these paths for the **module path** values as shown in the following JSON settings file:</span></span>

<span data-ttu-id="83a36-113">Simulovaném\_zařízení\_cloudu\_nahrát\_ukázka proces má cestu k konfigurační soubor JSON jako argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="83a36-113">The simulated\_device\_cloud\_upload\_sample process takes the path to a JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="83a36-114">Následující příklad souboru JSON je součástí sady SDK úložiště na **ukázky\\simulované\_zařízení\_cloudu\_nahrát\_ukázka\\src\\ Simulované\_zařízení\_cloudu\_nahrát\_ukázka\_lin.json**.</span><span class="sxs-lookup"><span data-stu-id="83a36-114">The following example JSON file is provided in the SDK repository at **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_sample\_lin.json**.</span></span> <span data-ttu-id="83a36-115">Tento konfigurační soubor funguje, jako je nezměníte skriptu buildu umístit IoT Edge moduly nebo ukázka spustitelné soubory v jiné než výchozí umístění.</span><span class="sxs-lookup"><span data-stu-id="83a36-115">This configuration file works as is unless you modify the build script to place the IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="83a36-116">Modul cesty jsou relativní vzhledem k adresáři z kterém spouštíte simulovaném\_zařízení\_cloudu\_nahrát\_ukázka spustitelný soubor, ne adresář, kde se nachází spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="83a36-116">The module paths are relative to the directory from where you run the simulated\_device\_cloud\_upload\_sample executable, not the directory where the executable is located.</span></span> <span data-ttu-id="83a36-117">Vzorový konfigurační soubor JSON výchozí zápis do 'deviceCloudUploadGatewaylog.log' v aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="83a36-117">The sample JSON configuration file defaults to writing to 'deviceCloudUploadGatewaylog.log' in your current working directory.</span></span>

<span data-ttu-id="83a36-118">V textovém editoru otevřete soubor **ukázky/simulated\_zařízení\_cloudu\_nahrát\_ukázkové, src/simulated\_zařízení\_cloudu\_nahrát\_lin.json** ve vaší místní kopii **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="83a36-118">In a text editor, open the file **samples/simulated\_device\_cloud\_upload\_sample/src/simulated\_device\_cloud\_upload\_lin.json** in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="83a36-119">Tento soubor konfiguruje moduly IoT hraniční brány ukázka:</span><span class="sxs-lookup"><span data-stu-id="83a36-119">This file configures the IoT Edge modules in the sample gateway:</span></span>

* <span data-ttu-id="83a36-120">**IoTHub** modulu připojuje ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="83a36-120">The **IoTHub** module connects to your IoT hub.</span></span> <span data-ttu-id="83a36-121">Je třeba nakonfigurovat odesílat data do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="83a36-121">You configure it to send data to your IoT hub.</span></span> <span data-ttu-id="83a36-122">Konkrétně nastavit **IoTHubName** hodnoty na název své služby IoT hub a nastavte **IoTHubSuffix** hodnotu **azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="83a36-122">Specifically, set the **IoTHubName** value to the name of your IoT hub and set the **IoTHubSuffix** value to **azure-devices.net**.</span></span> <span data-ttu-id="83a36-123">Nastavte **přenosu** hodnotu pro jeden z: **HTTP**, **AMQP**, nebo **MQTT**.</span><span class="sxs-lookup"><span data-stu-id="83a36-123">Set the **Transport** value to one of: **HTTP**, **AMQP**, or **MQTT**.</span></span> <span data-ttu-id="83a36-124">V současné době pouze **HTTP** sdílí jedno připojení protokolu TCP pro všechny zprávy zařízení.</span><span class="sxs-lookup"><span data-stu-id="83a36-124">Currently, only **HTTP** shares one TCP connection for all device messages.</span></span> <span data-ttu-id="83a36-125">Pokud nastavíte hodnotu na **AMQP**, nebo **MQTT**, bránu udržuje samostatného připojení TCP ke službě IoT Hub pro každé zařízení.</span><span class="sxs-lookup"><span data-stu-id="83a36-125">If you set the value to **AMQP**, or **MQTT**, the gateway maintains a separate TCP connection to IoT Hub for each device.</span></span>
* <span data-ttu-id="83a36-126">**Mapování** modulu mapuje adresy MAC Simulovaná zařízení na vaše ID zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="83a36-126">The **mapping** module maps the MAC addresses of your simulated devices to your IoT Hub device ids.</span></span> <span data-ttu-id="83a36-127">Ujistěte se, že **deviceId** hodnoty odpovídají identifikátory ID dvě zařízení, které jste přidali do služby IoT hub a že **deviceKey** hodnoty obsahovat klíče ze dvou zařízení.</span><span class="sxs-lookup"><span data-stu-id="83a36-127">Make sure that **deviceId** values match the ids of the two devices you added to your IoT hub, and that the **deviceKey** values contain the keys of your two devices.</span></span>
* <span data-ttu-id="83a36-128">**BLE1** a **BLE2** moduly jsou Simulovaná zařízení.</span><span class="sxs-lookup"><span data-stu-id="83a36-128">The **BLE1** and **BLE2** modules are the simulated devices.</span></span> <span data-ttu-id="83a36-129">Všimněte si, jak jejich adresy MAC shodovat s adresami v **mapování** modulu.</span><span class="sxs-lookup"><span data-stu-id="83a36-129">Note how their MAC addresses match the addresses in the **mapping** module.</span></span>
* <span data-ttu-id="83a36-130">**Protokolovač** modulu ukládá informace o vaší brány činnosti do souboru.</span><span class="sxs-lookup"><span data-stu-id="83a36-130">The **Logger** module logs your gateway activity to a file.</span></span>
* <span data-ttu-id="83a36-131">**Modulu cesta** hodnoty ukazuje příklad předpokládá, že spuštěním ukázky z **sestavení** složky ve vaší místní kopii **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="83a36-131">The **module path** values shown in the example assume that you run the sample from the **build** folder in your local copy of the **iot-edge** repository.</span></span>
* <span data-ttu-id="83a36-132">**Odkazy** pole v dolní části souboru JSON připojí **BLE1** a **BLE2** moduly, které **mapování** modul a **mapování** modulu **IoTHub** modulu.</span><span class="sxs-lookup"><span data-stu-id="83a36-132">The **links** array at the bottom of the JSON file connects the **BLE1** and **BLE2** modules to the **mapping** module, and the **mapping** module to the **IoTHub** module.</span></span> <span data-ttu-id="83a36-133">Také zajistí, že všechny zprávy v protokolu **Protokolovač** modulu.</span><span class="sxs-lookup"><span data-stu-id="83a36-133">It also ensures that all messages are logged by the **Logger** module.</span></span>

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

<span data-ttu-id="83a36-134">Uložte změny, které jste do konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="83a36-134">Save the changes you made to the configuration file.</span></span>

<span data-ttu-id="83a36-135">Spustit ukázku:</span><span class="sxs-lookup"><span data-stu-id="83a36-135">To run the sample:</span></span>

1. <span data-ttu-id="83a36-136">Ve vašem prostředí, přejděte na **iot-edge nebo sestavení** složky.</span><span class="sxs-lookup"><span data-stu-id="83a36-136">In your shell, navigate to the **iot-edge/build** folder.</span></span>
2. <span data-ttu-id="83a36-137">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="83a36-137">Run the following command:</span></span>
   
    ```sh
    ./samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ../samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```
3. <span data-ttu-id="83a36-138">Můžete použít [explorer zařízení] [ lnk-device-explorer] nebo [iothub-explorer] [ lnk-iothub-explorer] nástroje ke sledování zprávy, které Centrum IoT získává z brány.</span><span class="sxs-lookup"><span data-stu-id="83a36-138">You can use the [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool to monitor the messages that IoT hub receives from the gateway.</span></span> <span data-ttu-id="83a36-139">Například pomocí iothub-explorer můžete monitorovat zpráv typu zařízení cloud pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="83a36-139">For example, using iothub-explorer you can monitor device-to-cloud messages using the following command:</span></span>

    ```sh
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a><span data-ttu-id="83a36-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83a36-140">Next steps</span></span>

<span data-ttu-id="83a36-141">Získání rozsáhlejšími znalostmi Azure IoT okraj a Experimentujte s příklady kódu, najdete následujících kurzech developer a prostředky:</span><span class="sxs-lookup"><span data-stu-id="83a36-141">To gain a more advanced understanding of Azure IoT Edge and experiment with some code examples, visit the following developer tutorials and resources:</span></span>

* <span data-ttu-id="83a36-142">[Odesílání zpráv typu zařízení cloud z fyzického zařízení s Azure IoT Edge][lnk-physical-device]</span><span class="sxs-lookup"><span data-stu-id="83a36-142">[Send device-to-cloud messages from a physical device with Azure IoT Edge][lnk-physical-device]</span></span>
* <span data-ttu-id="83a36-143">[Azure IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="83a36-143">[Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="83a36-144">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="83a36-144">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="83a36-145">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="83a36-145">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="83a36-146">[Zabezpečení řešení IoT od základů nahoru][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="83a36-146">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
