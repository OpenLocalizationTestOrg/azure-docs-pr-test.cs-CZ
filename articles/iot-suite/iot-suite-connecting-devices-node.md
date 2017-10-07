---
title: "aaaConnect zařízení pomocí Node.js | Microsoft Docs"
description: "Popisuje, jak tooconnect toohello zařízení Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v Node.js."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 80bf2b70f15f539bfce4f135d533c46dd2b3f5a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-nodejs"></a><span data-ttu-id="511a4-103">Připojit vaše zařízení toohello (Node.js) předkonfigurovanému řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="511a4-103">Connect your device toohello remote monitoring preconfigured solution (Node.js)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a><span data-ttu-id="511a4-104">Vytvoření ukázkové řešení node.js</span><span class="sxs-lookup"><span data-stu-id="511a4-104">Create a node.js sample solution</span></span>

<span data-ttu-id="511a4-105">Ujistěte se, že verze Node.js 0.11.5 nebo novější je nainstalován na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="511a4-105">Ensure that Node.js version 0.11.5 or later is installed on your development machine.</span></span> <span data-ttu-id="511a4-106">Můžete spustit `node --version` hello příkazového řádku toocheck hello verzi.</span><span class="sxs-lookup"><span data-stu-id="511a4-106">You can run `node --version` at hello command line toocheck hello version.</span></span>

1. <span data-ttu-id="511a4-107">Vytvořte složku s názvem **RemoteMonitoring** na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="511a4-107">Create a folder called **RemoteMonitoring** on your development machine.</span></span> <span data-ttu-id="511a4-108">Přejděte toothis složky ve vašem prostředí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="511a4-108">Navigate toothis folder in your command-line environment.</span></span>

1. <span data-ttu-id="511a4-109">Spuštění hello následující příkazy toodownload a nainstalovat balíčky hello potřebujete toocomplete hello ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="511a4-109">Run hello following commands toodownload and install hello packages you need toocomplete hello sample app:</span></span>

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="511a4-110">V hello **RemoteMonitoring** složky, vytvořte soubor s názvem **remote_monitoring.js**.</span><span class="sxs-lookup"><span data-stu-id="511a4-110">In hello **RemoteMonitoring** folder, create a file called **remote_monitoring.js**.</span></span> <span data-ttu-id="511a4-111">Otevřete tento soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="511a4-111">Open this file in a text editor.</span></span>

1. <span data-ttu-id="511a4-112">V hello **remote_monitoring.js** soubor, přidejte následující hello `require` příkazy:</span><span class="sxs-lookup"><span data-stu-id="511a4-112">In hello **remote_monitoring.js** file, add hello following `require` statements:</span></span>

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. <span data-ttu-id="511a4-113">Přidejte následující deklarace proměnných po hello hello `require` příkazy.</span><span class="sxs-lookup"><span data-stu-id="511a4-113">Add hello following variable declarations after hello `require` statements.</span></span> <span data-ttu-id="511a4-114">Nahraďte zástupný symbol hodnoty hello [Id zařízení] a [klíč zařízení] s hodnotami, které jste si poznamenali pro vaše zařízení v panelu řešení vzdáleného monitorování hello.</span><span class="sxs-lookup"><span data-stu-id="511a4-114">Replace hello placeholder values [Device Id] and [Device Key] with values you noted for your device in hello remote monitoring solution dashboard.</span></span> <span data-ttu-id="511a4-115">Použijte hello název hostitele centra IoT z tooreplace řídicí panel řešení hello [IoTHub Name].</span><span class="sxs-lookup"><span data-stu-id="511a4-115">Use hello IoT Hub Hostname from hello solution dashboard tooreplace [IoTHub Name].</span></span> <span data-ttu-id="511a4-116">Pokud je například název hostitele vaší služby IoT Hub **contoso.azure-devices.net**, nahraďte hodnotu [IoTHub Name] za **contoso**:</span><span class="sxs-lookup"><span data-stu-id="511a4-116">For example, if your IoT Hub Hostname is **contoso.azure-devices.net**, replace [IoTHub Name] with **contoso**:</span></span>

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. <span data-ttu-id="511a4-117">Přidejte následující proměnné toodefine hello některé základní telemetrická data:</span><span class="sxs-lookup"><span data-stu-id="511a4-117">Add hello following variables toodefine some base telemetry data:</span></span>

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. <span data-ttu-id="511a4-118">Přidejte následující výsledky operace tooprint pomocné funkce hello:</span><span class="sxs-lookup"><span data-stu-id="511a4-118">Add hello following helper function tooprint operation results:</span></span>

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. <span data-ttu-id="511a4-119">Přidejte následující pomocné funkce toouse toorandomize hello telemetrie hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="511a4-119">Add hello following helper function toouse toorandomize hello telemetry values:</span></span>

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. <span data-ttu-id="511a4-120">Přidejte následující definice hello hello **DeviceInfo** objekt hello zařízení odesílá při spuštění:</span><span class="sxs-lookup"><span data-stu-id="511a4-120">Add hello following definition for hello **DeviceInfo** object hello device sends on startup:</span></span>

    ```nodejs
    var deviceMetaData = {
        'ObjectType': 'DeviceInfo',
        'IsSimulatedDevice': 0,
        'Version': '1.0',
        'DeviceProperties': {
            'DeviceID': deviceId,
            'HubEnabledState': 1
        }
    };
    ```

1. <span data-ttu-id="511a4-121">Přidejte následující hello Definice dvojče zařízení hello hlášené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="511a4-121">Add hello following definition for hello device twin reported values.</span></span> <span data-ttu-id="511a4-122">Tato definice obsahuje popisy hello přímé metod, které podporuje hello zařízení:</span><span class="sxs-lookup"><span data-stu-id="511a4-122">This definition includes descriptions of hello direct methods hello device supports:</span></span>

    ```nodejs
    var reportedProperties = {
        "Device": {
            "DeviceState": "normal",
            "Location": {
                "Latitude": 47.642877,
                "Longitude": -122.125497
            }
        },
        "Config": {
            "TemperatureMeanValue": 56.7,
            "TelemetryInterval": 45
        },
        "System": {
            "Manufacturer": "Contoso Inc.",
            "FirmwareVersion": "2.22",
            "InstalledRAM": "8 MB",
            "ModelNumber": "DB-14",
            "Platform": "Plat 9.75",
            "Processor": "i3-9",
            "SerialNumber": "SER99"
        },
        "Location": {
            "Latitude": 47.642877,
            "Longitude": -122.125497
        },
        "SupportedMethods": {
            "Reboot": "Reboot hello device",
            "InitiateFirmwareUpdate--FwPackageURI-string": "Updates device Firmware. Use parameter FwPackageURI toospecifiy hello URI of hello firmware file"
        },
    }
    ```

1. <span data-ttu-id="511a4-123">Přidejte následující funkce toohandle hello hello **restartovat** přímé volání metody:</span><span class="sxs-lookup"><span data-stu-id="511a4-123">Add hello following function toohandle hello **Reboot** direct method call:</span></span>

    ```nodejs
    function onReboot(request, response) {
        // Implement actual logic here.
        console.log('Simulated reboot...');

        // Complete hello response
        response.send(200, "Rebooting device", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

1. <span data-ttu-id="511a4-124">Přidejte následující funkce toohandle hello hello **InitiateFirmwareUpdate** přímé volání metody.</span><span class="sxs-lookup"><span data-stu-id="511a4-124">Add hello following function toohandle hello **InitiateFirmwareUpdate** direct method call.</span></span> <span data-ttu-id="511a4-125">Tato metoda přímé používá parametr toospecify hello umístění ve hello firmware image toodownload a iniciuje asynchronní hello aktualizaci firmwaru na hello zařízení:</span><span class="sxs-lookup"><span data-stu-id="511a4-125">This direct method uses a parameter toospecify hello location of hello firmware image toodownload, and initiates hello firmware update on hello device asynchronously:</span></span>

    ```nodejs
    function onInitiateFirmwareUpdate(request, response) {
        console.log('Simulated firmware update initiated, using: ' + request.payload.FwPackageURI);

        // Complete hello response
        response.send(200, "Firmware update initiated", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });

        // Add logic here tooperform hello firmware update asynchronously
    }
    ```

1. <span data-ttu-id="511a4-126">Přidejte následující kód toocreate instanci klienta hello:</span><span class="sxs-lookup"><span data-stu-id="511a4-126">Add hello following code toocreate a client instance:</span></span>

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. <span data-ttu-id="511a4-127">Přidejte následující kód do hello:</span><span class="sxs-lookup"><span data-stu-id="511a4-127">Add hello following code to:</span></span>

    * <span data-ttu-id="511a4-128">Otevře připojení hello.</span><span class="sxs-lookup"><span data-stu-id="511a4-128">Open hello connection.</span></span>
    * <span data-ttu-id="511a4-129">Odeslat hello **DeviceInfo** objektu.</span><span class="sxs-lookup"><span data-stu-id="511a4-129">Send hello **DeviceInfo** object.</span></span>
    * <span data-ttu-id="511a4-130">Nastavte obslužnou rutinu pro požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="511a4-130">Set up a handler for desired properties.</span></span>
    * <span data-ttu-id="511a4-131">Odešlete oznámenou vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="511a4-131">Send reported properties.</span></span>
    * <span data-ttu-id="511a4-132">Zaregistrujte obslužné rutiny pro přímé metody hello.</span><span class="sxs-lookup"><span data-stu-id="511a4-132">Register handlers for hello direct methods.</span></span>
    * <span data-ttu-id="511a4-133">Zahájit odesílání telemetrie.</span><span class="sxs-lookup"><span data-stu-id="511a4-133">Start sending telemetry.</span></span>

    ```nodejs
    client.open(function (err) {
        if (err) {
            printErrorFor('open')(err);
        } else {
            console.log('Sending device metadata:\n' + JSON.stringify(deviceMetaData));
            client.sendEvent(new Message(JSON.stringify(deviceMetaData)), printErrorFor('send metadata'));

            // Create device twin
            client.getTwin(function(err, twin) {
                if (err) {
                    console.error('Could not get device twin');
                } else {
                    console.log('Device twin created');

                    twin.on('properties.desired', function(delta) {
                        console.log('Received new desired properties:');
                        console.log(JSON.stringify(delta));
                    });

                    // Send reported properties
                    twin.properties.reported.update(reportedProperties, function(err) {
                        if (err) throw err;
                        console.log('twin state reported');
                    });

                    // Register handlers for direct methods
                    client.onDeviceMethod('Reboot', onReboot);
                    client.onDeviceMethod('InitiateFirmwareUpdate', onInitiateFirmwareUpdate);
                }
            });

            // Start sending telemetry
            var sendInterval = setInterval(function () {
                temperature += generateRandomIncrement();
                externalTemperature += generateRandomIncrement();
                humidity += generateRandomIncrement();

                var data = JSON.stringify({
                    'DeviceID': deviceId,
                    'Temperature': temperature,
                    'Humidity': humidity,
                    'ExternalTemperature': externalTemperature
                });

                console.log('Sending device event data:\n' + data);
                client.sendEvent(new Message(data), printErrorFor('send event'));
            }, 5000);

            client.on('error', function (err) {
                printErrorFor('client')(err);
                if (sendInterval) clearInterval(sendInterval);
                client.close(printErrorFor('client.close'));
            });
        }
    });
    ```

1. <span data-ttu-id="511a4-134">Uložit změny toohello hello **remote_monitoring.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="511a4-134">Save hello changes toohello **remote_monitoring.js** file.</span></span>

1. <span data-ttu-id="511a4-135">Spusťte následující příkaz v příkazovém řádku toolaunch hello ukázkové aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="511a4-135">Run hello following command at a command prompt toolaunch hello sample application:</span></span>
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
