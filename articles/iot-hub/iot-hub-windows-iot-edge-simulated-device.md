---
title: "Simulovat zařízení s Azure IoT hranou (Windows) | Microsoft Docs"
description: "Jak používat Azure IoT Edge v systému Windows k vytvoření simulovaného zařízení, které odesílá telemetrická data prostřednictvím Azure IoT vstupní brána do služby IoT hub."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: e7eb2931993daf3f0aecbd4a43d27ebd5adc10b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a><span data-ttu-id="4adea-103">Azure IoT Edge používat k odesílání zpráv typu zařízení cloud s simulované zařízení (Windows)</span><span class="sxs-lookup"><span data-stu-id="4adea-103">Use Azure IoT Edge to send device-to-cloud messages with a simulated device (Windows)</span></span>

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a><span data-ttu-id="4adea-104">Spuštění ukázky</span><span class="sxs-lookup"><span data-stu-id="4adea-104">How to run the sample</span></span>

<span data-ttu-id="4adea-105">**Build.cmd** skript generuje jeho výstup v **sestavení** složky ve vaší místní kopii **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="4adea-105">The **build.cmd** script generates its output in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="4adea-106">Tento výstup zahrnuje čtyři IoT Edge moduly používané v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="4adea-106">This output includes the four IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="4adea-107">Sestavení skriptu místa:</span><span class="sxs-lookup"><span data-stu-id="4adea-107">The build script places the:</span></span>

* <span data-ttu-id="4adea-108">**Logger.dll** v **sestavení\\moduly\\protokolovač\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="4adea-108">**logger.dll** in the **build\\modules\\logger\\Debug** folder.</span></span>
* <span data-ttu-id="4adea-109">**iothub.dll** v **sestavení\\moduly\\iothub\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="4adea-109">**iothub.dll** in the **build\\modules\\iothub\\Debug** folder.</span></span>
* <span data-ttu-id="4adea-110">**identity\_map.dll** v **sestavení\\moduly\\identitymap\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="4adea-110">**identity\_map.dll** in the **build\\modules\\identitymap\\Debug** folder.</span></span>
* <span data-ttu-id="4adea-111">**Simulované\_device.dll** v **sestavení\\moduly\\simulované\_zařízení\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="4adea-111">**simulated\_device.dll** in the **build\\modules\\simulated\_device\\Debug** folder.</span></span>

<span data-ttu-id="4adea-112">Použít tyto cesty pro **modulu cesta** hodnoty, jak je znázorněno v následujícím nastavení souboru JSON:</span><span class="sxs-lookup"><span data-stu-id="4adea-112">Use these paths for the **module path** values as shown in the following JSON settings file:</span></span>

<span data-ttu-id="4adea-113">Simulovaném\_zařízení\_cloudu\_nahrát\_ukázka proces má cestu k konfigurační soubor JSON jako argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4adea-113">The simulated\_device\_cloud\_upload\_sample process takes the path to a JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="4adea-114">Následující příklad souboru JSON je součástí sady SDK úložiště na **ukázky\\simulované\_zařízení\_cloudu\_nahrát\_ukázka\\src\\simulované\_zařízení\_cloudu\_nahrát\_ukázka\_win.json**.</span><span class="sxs-lookup"><span data-stu-id="4adea-114">The following example JSON file is provided in the SDK repository at **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_sample\_win.json**.</span></span> <span data-ttu-id="4adea-115">Tento konfigurační soubor funguje, jako je nezměníte skriptu buildu umístit IoT Edge moduly nebo ukázka spustitelné soubory v jiné než výchozí umístění.</span><span class="sxs-lookup"><span data-stu-id="4adea-115">This configuration file works as is unless you modify the build script to place the IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="4adea-116">Jsou modulu cesty relativní k adresáři kde simulované\_zařízení\_cloudu\_nahrát\_sample.exe nachází.</span><span class="sxs-lookup"><span data-stu-id="4adea-116">The module paths are relative to the directory where the simulated\_device\_cloud\_upload\_sample.exe is located.</span></span> <span data-ttu-id="4adea-117">Vzorový konfigurační soubor JSON výchozí zápis do 'deviceCloudUploadGatewaylog.log' v aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="4adea-117">The sample JSON configuration file defaults to writing to 'deviceCloudUploadGatewaylog.log' in your current working directory.</span></span>

<span data-ttu-id="4adea-118">V textovém editoru otevřete soubor **ukázky\\simulované\_zařízení\_cloudu\_nahrát\_ukázka\\src\\simulované\_zařízení\_cloudu\_nahrát\_win.json** ve vaší místní kopii **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="4adea-118">In a text editor, open the file **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_win.json** in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="4adea-119">Tento soubor konfiguruje moduly IoT hraniční brány ukázka:</span><span class="sxs-lookup"><span data-stu-id="4adea-119">This file configures the IoT Edge modules in the sample gateway:</span></span>

* <span data-ttu-id="4adea-120">**IoTHub** modulu připojuje ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="4adea-120">The **IoTHub** module connects to your IoT hub.</span></span> <span data-ttu-id="4adea-121">Je třeba nakonfigurovat odesílat data do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="4adea-121">You configure it to send data to your IoT hub.</span></span> <span data-ttu-id="4adea-122">Konkrétně nastavit **IoTHubName** hodnoty na název své služby IoT hub a nastavte **IoTHubSuffix** hodnotu **azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="4adea-122">Specifically, set the **IoTHubName** value to the name of your IoT hub and set the **IoTHubSuffix** value to **azure-devices.net**.</span></span> <span data-ttu-id="4adea-123">Nastavte **přenosu** hodnotu pro jeden z: **HTTP**, **AMQP**, nebo **MQTT**.</span><span class="sxs-lookup"><span data-stu-id="4adea-123">Set the **Transport** value to one of: **HTTP**, **AMQP**, or **MQTT**.</span></span> <span data-ttu-id="4adea-124">V současné době pouze **HTTP** sdílí jedno připojení protokolu TCP pro všechny zprávy zařízení.</span><span class="sxs-lookup"><span data-stu-id="4adea-124">Currently, only **HTTP** shares one TCP connection for all device messages.</span></span> <span data-ttu-id="4adea-125">Pokud nastavíte hodnotu na **AMQP**, nebo **MQTT**, bránu udržuje samostatného připojení TCP ke službě IoT Hub pro každé zařízení.</span><span class="sxs-lookup"><span data-stu-id="4adea-125">If you set the value to **AMQP**, or **MQTT**, the gateway maintains a separate TCP connection to IoT Hub for each device.</span></span>
* <span data-ttu-id="4adea-126">**Mapování** modulu mapuje adresy MAC Simulovaná zařízení na vaše ID zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="4adea-126">The **mapping** module maps the MAC addresses of your simulated devices to your IoT Hub device ids.</span></span> <span data-ttu-id="4adea-127">Ujistěte se, že **deviceId** hodnoty odpovídají identifikátory ID dvě zařízení, které jste přidali do služby IoT hub a že **deviceKey** hodnoty obsahovat klíče ze dvou zařízení.</span><span class="sxs-lookup"><span data-stu-id="4adea-127">Make sure that **deviceId** values match the ids of the two devices you added to your IoT hub, and that the **deviceKey** values contain the keys of your two devices.</span></span>
* <span data-ttu-id="4adea-128">**BLE1** a **BLE2** moduly jsou Simulovaná zařízení.</span><span class="sxs-lookup"><span data-stu-id="4adea-128">The **BLE1** and **BLE2** modules are the simulated devices.</span></span> <span data-ttu-id="4adea-129">Všimněte si, jak adresy MAC modulu shodovat s adresami v **mapování** modulu.</span><span class="sxs-lookup"><span data-stu-id="4adea-129">Note how the module MAC addresses match the addresses in the **mapping** module.</span></span>
* <span data-ttu-id="4adea-130">**Protokolovač** modulu ukládá informace o vaší brány činnosti do souboru.</span><span class="sxs-lookup"><span data-stu-id="4adea-130">The **Logger** module logs your gateway activity to a file.</span></span>
* <span data-ttu-id="4adea-131">**Modulu cesta** hodnoty zobrazené v následujícím příkladu jsou relativní vzhledem k adresáři kde simulované\_zařízení\_cloudu\_nahrát\_sample.exe nachází.</span><span class="sxs-lookup"><span data-stu-id="4adea-131">The **module path** values shown in the following example are relative to the directory where the simulated\_device\_cloud\_upload\_sample.exe is located.</span></span>
* <span data-ttu-id="4adea-132">**Odkazy** pole v dolní části souboru JSON připojí **BLE1** a **BLE2** moduly, které **mapování** modul a **mapování** modulu **IoTHub** modulu.</span><span class="sxs-lookup"><span data-stu-id="4adea-132">The **links** array at the bottom of the JSON file connects the **BLE1** and **BLE2** modules to the **mapping** module, and the **mapping** module to the **IoTHub** module.</span></span> <span data-ttu-id="4adea-133">Také zajistí, že všechny zprávy v protokolu **Protokolovač** modulu.</span><span class="sxs-lookup"><span data-stu-id="4adea-133">It also ensures that all messages are logged by the **Logger** module.</span></span>

```json
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
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
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
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
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
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
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
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
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

<span data-ttu-id="4adea-134">Uložte změny, které jste do konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="4adea-134">Save the changes you made to the configuration file.</span></span>

<span data-ttu-id="4adea-135">Spustit ukázku:</span><span class="sxs-lookup"><span data-stu-id="4adea-135">To run the sample:</span></span>

1. <span data-ttu-id="4adea-136">Na příkazovém řádku přejděte do **sestavení** složky ve vaší místní kopii **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="4adea-136">At a command prompt, navigate to the **build** folder in your local copy of the **iot-edge** repository.</span></span>
2. <span data-ttu-id="4adea-137">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4adea-137">Run the following command:</span></span>
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. <span data-ttu-id="4adea-138">Můžete použít [explorer zařízení] [ lnk-device-explorer] nebo [iothub-explorer] [ lnk-iothub-explorer] nástroje ke sledování zprávy, které Centrum IoT získává z brány.</span><span class="sxs-lookup"><span data-stu-id="4adea-138">You can use the [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool to monitor the messages that IoT hub receives from the gateway.</span></span> <span data-ttu-id="4adea-139">Například pomocí iothub-explorer můžete monitorovat zpráv typu zařízení cloud pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="4adea-139">For example, using iothub-explorer you can monitor device-to-cloud messages using the following command:</span></span>

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a><span data-ttu-id="4adea-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4adea-140">Next steps</span></span>

<span data-ttu-id="4adea-141">Získání rozsáhlejšími znalostmi IoT okraj a Experimentujte s příklady kódu, najdete následujících kurzech developer a prostředky:</span><span class="sxs-lookup"><span data-stu-id="4adea-141">To gain a more advanced understanding of IoT Edge and experiment with some code examples, visit the following developer tutorials and resources:</span></span>

* <span data-ttu-id="4adea-142">[Odesílání zpráv typu zařízení cloud z fyzického zařízení s hranou IoT][lnk-physical-device]</span><span class="sxs-lookup"><span data-stu-id="4adea-142">[Send device-to-cloud messages from a physical device with IoT Edge][lnk-physical-device]</span></span>
* <span data-ttu-id="4adea-143">[Azure IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="4adea-143">[Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="4adea-144">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="4adea-144">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="4adea-145">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="4adea-145">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="4adea-146">[Zabezpečení řešení IoT od základů nahoru][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="4adea-146">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md