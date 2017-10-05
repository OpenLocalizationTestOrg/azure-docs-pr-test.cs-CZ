---
title: "Připojení zařízení pomocí Node.js | Microsoft Docs"
description: "Popisuje, jak se připojit zařízení k Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v Node.js."
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
ms.openlocfilehash: 6459b6196eb7f4a083b67e5a421bcc0d51d39e5c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a><span data-ttu-id="f6596-103">Připojte zařízení k monitorování předkonfigurované řešení vzdáleného (Node.js)</span><span class="sxs-lookup"><span data-stu-id="f6596-103">Connect your device to the remote monitoring preconfigured solution (Node.js)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a><span data-ttu-id="f6596-104">Vytvoření ukázkové řešení node.js</span><span class="sxs-lookup"><span data-stu-id="f6596-104">Create a node.js sample solution</span></span>

<span data-ttu-id="f6596-105">Ujistěte se, že verze Node.js 0.11.5 nebo novější je nainstalován na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="f6596-105">Ensure that Node.js version 0.11.5 or later is installed on your development machine.</span></span> <span data-ttu-id="f6596-106">Můžete spustit `node --version` na příkazovém řádku, které chcete zkontrolovat verzi.</span><span class="sxs-lookup"><span data-stu-id="f6596-106">You can run `node --version` at the command line to check the version.</span></span>

1. <span data-ttu-id="f6596-107">Vytvořte složku s názvem **RemoteMonitoring** na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="f6596-107">Create a folder called **RemoteMonitoring** on your development machine.</span></span> <span data-ttu-id="f6596-108">Přejděte do této složky ve vašem prostředí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f6596-108">Navigate to this folder in your command-line environment.</span></span>

1. <span data-ttu-id="f6596-109">Spusťte následující příkazy ke stažení a instalaci balíčků, že které potřebujete k dokončení ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="f6596-109">Run the following commands to download and install the packages you need to complete the sample app:</span></span>

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="f6596-110">V **RemoteMonitoring** složky, vytvořte soubor s názvem **remote_monitoring.js**.</span><span class="sxs-lookup"><span data-stu-id="f6596-110">In the **RemoteMonitoring** folder, create a file called **remote_monitoring.js**.</span></span> <span data-ttu-id="f6596-111">Otevřete tento soubor v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="f6596-111">Open this file in a text editor.</span></span>

1. <span data-ttu-id="f6596-112">V **remote_monitoring.js** soubor, přidejte následující `require` příkazy:</span><span class="sxs-lookup"><span data-stu-id="f6596-112">In the **remote_monitoring.js** file, add the following `require` statements:</span></span>

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. <span data-ttu-id="f6596-113">Přidejte následující deklarace proměnných za příkazy `require`.</span><span class="sxs-lookup"><span data-stu-id="f6596-113">Add the following variable declarations after the `require` statements.</span></span> <span data-ttu-id="f6596-114">Nahraďte zástupné hodnoty [Device Id] (ID zařízení) a [Device Key] (Klíč zařízení) hodnotami, které jste si pro své zařízení poznamenali na řídicím panelu řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="f6596-114">Replace the placeholder values [Device Id] and [Device Key] with values you noted for your device in the remote monitoring solution dashboard.</span></span> <span data-ttu-id="f6596-115">K nahrazení hodnoty [IoTHub Name] (Název služby IoT Hub) použijte název hostitele služby IoT Hub z řídicího panelu řešení.</span><span class="sxs-lookup"><span data-stu-id="f6596-115">Use the IoT Hub Hostname from the solution dashboard to replace [IoTHub Name].</span></span> <span data-ttu-id="f6596-116">Pokud je například název hostitele vaší služby IoT Hub **contoso.azure-devices.net**, nahraďte hodnotu [IoTHub Name] za **contoso**:</span><span class="sxs-lookup"><span data-stu-id="f6596-116">For example, if your IoT Hub Hostname is **contoso.azure-devices.net**, replace [IoTHub Name] with **contoso**:</span></span>

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. <span data-ttu-id="f6596-117">Přidejte následující proměnné na definovat některé základní telemetrická data:</span><span class="sxs-lookup"><span data-stu-id="f6596-117">Add the following variables to define some base telemetry data:</span></span>

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. <span data-ttu-id="f6596-118">Přidejte následující podpůrná funkce Tisknout výsledky operace:</span><span class="sxs-lookup"><span data-stu-id="f6596-118">Add the following helper function to print operation results:</span></span>

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. <span data-ttu-id="f6596-119">Přidejte následující pomocné funkce sloužící k náhodné telemetrie hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f6596-119">Add the following helper function to use to randomize the telemetry values:</span></span>

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. <span data-ttu-id="f6596-120">Přidejte následující definice pro **DeviceInfo** objektu zařízení odesílá při spuštění:</span><span class="sxs-lookup"><span data-stu-id="f6596-120">Add the following definition for the **DeviceInfo** object the device sends on startup:</span></span>

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

1. <span data-ttu-id="f6596-121">Přidejte následující definice dvojče zařízení hlášené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f6596-121">Add the following definition for the device twin reported values.</span></span> <span data-ttu-id="f6596-122">Tato definice obsahuje popisy přímé metod, které podporuje zařízení:</span><span class="sxs-lookup"><span data-stu-id="f6596-122">This definition includes descriptions of the direct methods the device supports:</span></span>

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
            "Reboot": "Reboot the device",
            "InitiateFirmwareUpdate--FwPackageURI-string": "Updates device Firmware. Use parameter FwPackageURI to specifiy the URI of the firmware file"
        },
    }
    ```

1. <span data-ttu-id="f6596-123">Přidejte následující funkci pro zpracování **restartovat** přímé volání metody:</span><span class="sxs-lookup"><span data-stu-id="f6596-123">Add the following function to handle the **Reboot** direct method call:</span></span>

    ```nodejs
    function onReboot(request, response) {
        // Implement actual logic here.
        console.log('Simulated reboot...');

        // Complete the response
        response.send(200, "Rebooting device", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

1. <span data-ttu-id="f6596-124">Přidejte následující funkci pro zpracování **InitiateFirmwareUpdate** přímé volání metody.</span><span class="sxs-lookup"><span data-stu-id="f6596-124">Add the following function to handle the **InitiateFirmwareUpdate** direct method call.</span></span> <span data-ttu-id="f6596-125">Tato metoda přímé používá parametr k určení umístění bitové kopie firmware ke stažení a iniciuje firmwaru v zařízení asynchronní aktualizace:</span><span class="sxs-lookup"><span data-stu-id="f6596-125">This direct method uses a parameter to specify the location of the firmware image to download, and initiates the firmware update on the device asynchronously:</span></span>

    ```nodejs
    function onInitiateFirmwareUpdate(request, response) {
        console.log('Simulated firmware update initiated, using: ' + request.payload.FwPackageURI);

        // Complete the response
        response.send(200, "Firmware update initiated", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });

        // Add logic here to perform the firmware update asynchronously
    }
    ```

1. <span data-ttu-id="f6596-126">Přidejte následující kód k vytvoření instance klienta:</span><span class="sxs-lookup"><span data-stu-id="f6596-126">Add the following code to create a client instance:</span></span>

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. <span data-ttu-id="f6596-127">Přidejte následující kód do:</span><span class="sxs-lookup"><span data-stu-id="f6596-127">Add the following code to:</span></span>

    * <span data-ttu-id="f6596-128">Otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="f6596-128">Open the connection.</span></span>
    * <span data-ttu-id="f6596-129">Odeslat **DeviceInfo** objektu.</span><span class="sxs-lookup"><span data-stu-id="f6596-129">Send the **DeviceInfo** object.</span></span>
    * <span data-ttu-id="f6596-130">Nastavte obslužnou rutinu pro požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f6596-130">Set up a handler for desired properties.</span></span>
    * <span data-ttu-id="f6596-131">Odešlete oznámenou vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f6596-131">Send reported properties.</span></span>
    * <span data-ttu-id="f6596-132">Zaregistrujte obslužné rutiny pro přímé metody.</span><span class="sxs-lookup"><span data-stu-id="f6596-132">Register handlers for the direct methods.</span></span>
    * <span data-ttu-id="f6596-133">Zahájit odesílání telemetrie.</span><span class="sxs-lookup"><span data-stu-id="f6596-133">Start sending telemetry.</span></span>

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

1. <span data-ttu-id="f6596-134">Uložit změny **remote_monitoring.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="f6596-134">Save the changes to the **remote_monitoring.js** file.</span></span>

1. <span data-ttu-id="f6596-135">Spusťte následující příkaz na příkazovém řádku spusťte ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="f6596-135">Run the following command at a command prompt to launch the sample application:</span></span>
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
