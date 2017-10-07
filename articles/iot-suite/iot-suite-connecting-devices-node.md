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
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-nodejs"></a>Připojit vaše zařízení toohello (Node.js) předkonfigurovanému řešení vzdáleného monitorování
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a>Vytvoření ukázkové řešení node.js

Ujistěte se, že verze Node.js 0.11.5 nebo novější je nainstalován na vývojovém počítači. Můžete spustit `node --version` hello příkazového řádku toocheck hello verzi.

1. Vytvořte složku s názvem **RemoteMonitoring** na vývojovém počítači. Přejděte toothis složky ve vašem prostředí příkazového řádku.

1. Spuštění hello následující příkazy toodownload a nainstalovat balíčky hello potřebujete toocomplete hello ukázkovou aplikaci:

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. V hello **RemoteMonitoring** složky, vytvořte soubor s názvem **remote_monitoring.js**. Otevřete tento soubor v textovém editoru.

1. V hello **remote_monitoring.js** soubor, přidejte následující hello `require` příkazy:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. Přidejte následující deklarace proměnných po hello hello `require` příkazy. Nahraďte zástupný symbol hodnoty hello [Id zařízení] a [klíč zařízení] s hodnotami, které jste si poznamenali pro vaše zařízení v panelu řešení vzdáleného monitorování hello. Použijte hello název hostitele centra IoT z tooreplace řídicí panel řešení hello [IoTHub Name]. Pokud je například název hostitele vaší služby IoT Hub **contoso.azure-devices.net**, nahraďte hodnotu [IoTHub Name] za **contoso**:

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Přidejte následující proměnné toodefine hello některé základní telemetrická data:

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. Přidejte následující výsledky operace tooprint pomocné funkce hello:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Přidejte následující pomocné funkce toouse toorandomize hello telemetrie hodnoty hello:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Přidejte následující definice hello hello **DeviceInfo** objekt hello zařízení odesílá při spuštění:

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

1. Přidejte následující hello Definice dvojče zařízení hello hlášené hodnoty. Tato definice obsahuje popisy hello přímé metod, které podporuje hello zařízení:

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

1. Přidejte následující funkce toohandle hello hello **restartovat** přímé volání metody:

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

1. Přidejte následující funkce toohandle hello hello **InitiateFirmwareUpdate** přímé volání metody. Tato metoda přímé používá parametr toospecify hello umístění ve hello firmware image toodownload a iniciuje asynchronní hello aktualizaci firmwaru na hello zařízení:

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

1. Přidejte následující kód toocreate instanci klienta hello:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Přidejte následující kód do hello:

    * Otevře připojení hello.
    * Odeslat hello **DeviceInfo** objektu.
    * Nastavte obslužnou rutinu pro požadované vlastnosti.
    * Odešlete oznámenou vlastnosti.
    * Zaregistrujte obslužné rutiny pro přímé metody hello.
    * Zahájit odesílání telemetrie.

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

1. Uložit změny toohello hello **remote_monitoring.js** souboru.

1. Spusťte následující příkaz v příkazovém řádku toolaunch hello ukázkové aplikace hello:
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
