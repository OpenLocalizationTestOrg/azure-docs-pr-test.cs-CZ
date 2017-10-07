---
title: "aaaSimulate zařízení s Azure IoT okraj (Windows) | Microsoft Docs"
description: "Jak toouse Azure IoT Edge na Windows toocreate simulované zařízení, odesílá telemetrická data prostřednictvím služby Azure IoT hraniční brány tooan IoT hub."
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
ms.openlocfilehash: ddbe85eb956e9934e80e2e80e09f77b24cf54856
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-toosend-device-to-cloud-messages-with-a-simulated-device-windows"></a><span data-ttu-id="15736-103">Použití Azure IoT Edge toosend zařízení cloud zprávy s simulované zařízení (Windows)</span><span class="sxs-lookup"><span data-stu-id="15736-103">Use Azure IoT Edge toosend device-to-cloud messages with a simulated device (Windows)</span></span>

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-toorun-hello-sample"></a><span data-ttu-id="15736-104">Jak toorun hello ukázka</span><span class="sxs-lookup"><span data-stu-id="15736-104">How toorun hello sample</span></span>

<span data-ttu-id="15736-105">Hello **build.cmd** skript generuje jeho výstup v hello **sestavení** složky ve vaší místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="15736-105">hello **build.cmd** script generates its output in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="15736-106">Tento výstup zahrnuje hello čtyři IoT Edge moduly používané v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="15736-106">This output includes hello four IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="15736-107">Hello sestavení skriptu míst:</span><span class="sxs-lookup"><span data-stu-id="15736-107">hello build script places the:</span></span>

* <span data-ttu-id="15736-108">**Logger.dll** v hello **sestavení\\moduly\\protokolovač\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="15736-108">**logger.dll** in hello **build\\modules\\logger\\Debug** folder.</span></span>
* <span data-ttu-id="15736-109">**iothub.dll** v hello **sestavení\\moduly\\iothub\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="15736-109">**iothub.dll** in hello **build\\modules\\iothub\\Debug** folder.</span></span>
* <span data-ttu-id="15736-110">**identity\_map.dll** v hello **sestavení\\moduly\\identitymap\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="15736-110">**identity\_map.dll** in hello **build\\modules\\identitymap\\Debug** folder.</span></span>
* <span data-ttu-id="15736-111">**Simulované\_device.dll** v hello **sestavení\\moduly\\simulované\_zařízení\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="15736-111">**simulated\_device.dll** in hello **build\\modules\\simulated\_device\\Debug** folder.</span></span>

<span data-ttu-id="15736-112">Použít tyto cesty pro hello **modulu cesta** hodnoty, jak je znázorněno v následující soubor JSON nastavení hello:</span><span class="sxs-lookup"><span data-stu-id="15736-112">Use these paths for hello **module path** values as shown in hello following JSON settings file:</span></span>

<span data-ttu-id="15736-113">Hello simulated\_zařízení\_cloudu\_nahrát\_ukázka proces trvá hello konfigurační soubor JSON tooa cestu jako argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="15736-113">hello simulated\_device\_cloud\_upload\_sample process takes hello path tooa JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="15736-114">Hello následující příklad souboru JSON je součástí sady SDK úložiště hello na **ukázky\\simulované\_zařízení\_cloudu\_nahrát\_ukázka\\src\\ Simulované\_zařízení\_cloudu\_nahrát\_ukázka\_win.json**.</span><span class="sxs-lookup"><span data-stu-id="15736-114">hello following example JSON file is provided in hello SDK repository at **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_sample\_win.json**.</span></span> <span data-ttu-id="15736-115">Tato konfigurace soubor funguje jako je nezměníte hello vytvořit skript tooplace hello IoT Edge moduly nebo ukázkové spustitelné soubory v jiné než výchozí umístění.</span><span class="sxs-lookup"><span data-stu-id="15736-115">This configuration file works as is unless you modify hello build script tooplace hello IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="15736-116">Hello modulu cest relativní toohello adresáře, kde hello simulated\_zařízení\_cloudu\_nahrát\_sample.exe nachází.</span><span class="sxs-lookup"><span data-stu-id="15736-116">hello module paths are relative toohello directory where hello simulated\_device\_cloud\_upload\_sample.exe is located.</span></span> <span data-ttu-id="15736-117">Výchozí konfigurační soubor JSON ukázka Hello toowriting too'deviceCloudUploadGatewaylog.log se v aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="15736-117">hello sample JSON configuration file defaults toowriting too'deviceCloudUploadGatewaylog.log' in your current working directory.</span></span>

<span data-ttu-id="15736-118">V textovém editoru otevřete soubor hello **ukázky\\simulované\_zařízení\_cloudu\_nahrát\_ukázka\\src\\simulované\_zařízení \_cloudu\_nahrát\_win.json** ve vaší místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="15736-118">In a text editor, open hello file **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_win.json** in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="15736-119">Tento soubor konfiguruje hello IoT Edge moduly v bráně ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="15736-119">This file configures hello IoT Edge modules in hello sample gateway:</span></span>

* <span data-ttu-id="15736-120">Hello **IoTHub** modulu připojí tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="15736-120">hello **IoTHub** module connects tooyour IoT hub.</span></span> <span data-ttu-id="15736-121">Můžete ji nakonfigurovat toosend data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="15736-121">You configure it toosend data tooyour IoT hub.</span></span> <span data-ttu-id="15736-122">Konkrétně sadu hello **IoTHubName** toohello název služby IoT hub hodnoty a nastavte hello **IoTHubSuffix** hodnota příliš**azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="15736-122">Specifically, set hello **IoTHubName** value toohello name of your IoT hub and set hello **IoTHubSuffix** value too**azure-devices.net**.</span></span> <span data-ttu-id="15736-123">Sada hello **přenosu** tooone hodnotu z: **HTTP**, **AMQP**, nebo **MQTT**.</span><span class="sxs-lookup"><span data-stu-id="15736-123">Set hello **Transport** value tooone of: **HTTP**, **AMQP**, or **MQTT**.</span></span> <span data-ttu-id="15736-124">V současné době pouze **HTTP** sdílí jedno připojení protokolu TCP pro všechny zprávy zařízení.</span><span class="sxs-lookup"><span data-stu-id="15736-124">Currently, only **HTTP** shares one TCP connection for all device messages.</span></span> <span data-ttu-id="15736-125">Pokud nastavíte hodnotu hello příliš**AMQP**, nebo **MQTT**, brány hello udržuje samostatné TCP připojení tooIoT rozbočovače pro každé zařízení.</span><span class="sxs-lookup"><span data-stu-id="15736-125">If you set hello value too**AMQP**, or **MQTT**, hello gateway maintains a separate TCP connection tooIoT Hub for each device.</span></span>
* <span data-ttu-id="15736-126">Hello **mapování** modulu mapování adresy MAC hello ID zařízení IoT Hub tooyour vaše Simulovaná zařízení.</span><span class="sxs-lookup"><span data-stu-id="15736-126">hello **mapping** module maps hello MAC addresses of your simulated devices tooyour IoT Hub device ids.</span></span> <span data-ttu-id="15736-127">Ujistěte se, že **deviceId** ID hello shody hodnoty hello dvě zařízení, které jste přidali tooyour IoT hub a že hello **deviceKey** hodnoty obsahovat hello klíče ze dvou zařízení.</span><span class="sxs-lookup"><span data-stu-id="15736-127">Make sure that **deviceId** values match hello ids of hello two devices you added tooyour IoT hub, and that hello **deviceKey** values contain hello keys of your two devices.</span></span>
* <span data-ttu-id="15736-128">Hello **BLE1** a **BLE2** moduly jsou hello simulované zařízení.</span><span class="sxs-lookup"><span data-stu-id="15736-128">hello **BLE1** and **BLE2** modules are hello simulated devices.</span></span> <span data-ttu-id="15736-129">Všimněte si, jak adresy MAC modulu hello odpovídat hello adresy v hello **mapování** modulu.</span><span class="sxs-lookup"><span data-stu-id="15736-129">Note how hello module MAC addresses match hello addresses in hello **mapping** module.</span></span>
* <span data-ttu-id="15736-130">Hello **Protokolovač** modulu protokoly souboru tooa aktivity brány.</span><span class="sxs-lookup"><span data-stu-id="15736-130">hello **Logger** module logs your gateway activity tooa file.</span></span>
* <span data-ttu-id="15736-131">Hello **modulu cesta** hodnoty zobrazené v následující ukázka hello jsou relativní toohello adresáře, kde hello simulated\_zařízení\_cloudu\_nahrát\_sample.exe nachází.</span><span class="sxs-lookup"><span data-stu-id="15736-131">hello **module path** values shown in hello following example are relative toohello directory where hello simulated\_device\_cloud\_upload\_sample.exe is located.</span></span>
* <span data-ttu-id="15736-132">Hello **odkazy** pole v dolní části hello souboru JSON hello připojí hello **BLE1** a **BLE2** moduly toohello **mapování** modulu a hello **mapování** modulu toohello **IoTHub** modulu.</span><span class="sxs-lookup"><span data-stu-id="15736-132">hello **links** array at hello bottom of hello JSON file connects hello **BLE1** and **BLE2** modules toohello **mapping** module, and hello **mapping** module toohello **IoTHub** module.</span></span> <span data-ttu-id="15736-133">Také zajistí, že všechny zprávy v hello protokolu **Protokolovač** modulu.</span><span class="sxs-lookup"><span data-stu-id="15736-133">It also ensures that all messages are logged by hello **Logger** module.</span></span>

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

<span data-ttu-id="15736-134">Uložte změny hello provedené toohello konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="15736-134">Save hello changes you made toohello configuration file.</span></span>

<span data-ttu-id="15736-135">Ukázka toorun hello:</span><span class="sxs-lookup"><span data-stu-id="15736-135">toorun hello sample:</span></span>

1. <span data-ttu-id="15736-136">Na příkazovém řádku přejděte toohello **sestavení** složky ve vaší místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="15736-136">At a command prompt, navigate toohello **build** folder in your local copy of hello **iot-edge** repository.</span></span>
2. <span data-ttu-id="15736-137">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="15736-137">Run hello following command:</span></span>
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. <span data-ttu-id="15736-138">Můžete použít hello [explorer zařízení] [ lnk-device-explorer] nebo [iothub-explorer] [ lnk-iothub-explorer] nástroj toomonitor hello zprávy, které Centrum IoT získává z hello brána.</span><span class="sxs-lookup"><span data-stu-id="15736-138">You can use hello [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool toomonitor hello messages that IoT hub receives from hello gateway.</span></span> <span data-ttu-id="15736-139">Například pomocí iothub-explorer můžete monitorovat zpráv typu zařízení cloud pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="15736-139">For example, using iothub-explorer you can monitor device-to-cloud messages using hello following command:</span></span>

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a><span data-ttu-id="15736-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15736-140">Next steps</span></span>

<span data-ttu-id="15736-141">toogain rozsáhlejšími znalostmi IoT okraj a experimentu s příklady kódu navštívit hello následující kurzy developer a prostředky:</span><span class="sxs-lookup"><span data-stu-id="15736-141">toogain a more advanced understanding of IoT Edge and experiment with some code examples, visit hello following developer tutorials and resources:</span></span>

* <span data-ttu-id="15736-142">[Odesílání zpráv typu zařízení cloud z fyzického zařízení s hranou IoT][lnk-physical-device]</span><span class="sxs-lookup"><span data-stu-id="15736-142">[Send device-to-cloud messages from a physical device with IoT Edge][lnk-physical-device]</span></span>
* <span data-ttu-id="15736-143">[Azure IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="15736-143">[Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="15736-144">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="15736-144">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="15736-145">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="15736-145">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="15736-146">[Zabezpečení řešení IoT z hello pozadí][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="15736-146">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md