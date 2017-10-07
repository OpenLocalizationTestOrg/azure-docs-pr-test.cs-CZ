---
title: "aaaGet spuštění se správou zařízení Azure IoT Hub (uzel) | Microsoft Docs"
description: "Jak toouse tooinitiate správy zařízení IoT Hub vzdáleném zařízení restartovat. Používáte hello Azure IoT SDK pro Node.js tooimplement aplikaci simulovaného zařízení, která zahrnuje přímá metoda a aplikační služby, která volá metodu přímé hello."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: juanpere
ms.openlocfilehash: 5dd1878e71231850fb95f4170b823f1e86c3ee83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-node"></a>Začínáme se správou zařízení (uzel)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

V tomto kurzu získáte informace o následujících postupech:

* Použijte hello portálu toocreate Azure IoT Hub a vytvoření identity zařízení ve službě IoT hub.
* Vytvoření aplikace simulovaného zařízení, která obsahuje přímý metodu, která restartování zařízení. Přímé metody jsou vyvolány z cloudu hello.
* Vytvořte konzolovou aplikaci softwaru Node.js, která volá metodu přímé hello restartování v hello aplikaci simulovaného zařízení prostřednictvím služby IoT hub.

Na konci hello tohoto kurzu máte dvě aplikace konzoly Node.js:

**dmpatterns_getstarted_device.js**, který připojí tooyour IoT hub s dříve, vytvořenou identitou zařízení hello obdrží přímá metoda restartování, simuluje restartu fyzické a sestavy hello čas pro poslední restartování hello.

**dmpatterns_getstarted_service.js**, která volá metodu přímé v aplikaci simulovaného zařízení hello, zobrazí hello odpovědi a zobrazí hello aktualizovat hlášené vlastnosti.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Node.js verze 0.12.x nebo novější, <br/>  [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části

* Vytvořte konzolovou aplikaci Node.js, která odpovídá tooa přímá metoda volá hello cloudu
* Aktivační události restartu simulovaného zařízení
* Použití hello hlášené vlastnosti tooenable zařízení twin dotazy tooidentify zařízení a při jejich poslední restartovat

1. Vytvořte prázdnou složku s názvem **manageddevice**.  V hello **manageddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.  Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **manageddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt**balíčku:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru, vytvořte **dmpatterns_getstarted_device.js** souboru v hello **manageddevice** složky.
4. Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_getstarted_device.js** souboru:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Přidat **connectionString** proměnné a použít ho toocreate **klienta** instance.  Nahraďte hello připojovacího řetězce připojovacím řetězcem zařízení.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Přidejte následující funkce tooimplement hello přímá metoda na zařízení hello hello
   
    ```
    var onReboot = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report hello reboot before hello physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. Otevřete Centrum IoT tooyour hello připojení a spuštění nástroje pro sledování přímá metoda hello:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. Uložte a zavřete hello **dmpatterns_getstarted_device.js** souboru.

> [!NOTE]
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a>Aktivační události restartu vzdálené na zařízení hello pomocí přímá metoda
V této části vytvoříte konzolovou aplikaci softwaru Node.js, který iniciuje vzdálené restartování na zařízení pomocí přímého metody. aplikace Hello používá zařízení twin dotazy toodiscover hello čas posledního restartování pro toto zařízení.

1. Vytvořit prázdnou složku s názvem **triggerrebootondevice**.  V hello **triggerrebootondevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.  Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **triggerrebootondevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku sady SDK zařízení a **azure-iot zařízení mqtt** balíčku:
   
    ```
    npm install azure-iothub --save
    ```
3. Pomocí textového editoru, vytvořte **dmpatterns_getstarted_service.js** souboru v hello **triggerrebootondevice** složky.
4. Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_getstarted_service.js** souboru:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Přidejte následující deklarace proměnných hello a nahraďte zástupné hodnoty hello:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. Přidejte následující funkce tooinvoke hello zařízení metoda tooreboot hello cílové zařízení hello:
   
    ```
    var startRebootDevice = function(twin) {
   
        var methodName = "reboot";
   
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
   
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked hello device tooreboot.");  
            }
        });
    };
    ```
7. Přidejte následující hello funkce tooquery hello zařízení a získat hello čas posledního restartování:
   
    ```
    var queryTwinLastReboot = function() {
   
        registry.getTwin(deviceToReboot, function(err, twin){
   
            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device tooreport last reboot time.');
        });
    };
    ```
8. Přidejte následující funkce hello toocall kódu, které aktivují hello hello restartovat přímá metoda a dotaz na hello poslední restartovat čas:
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. Uložte a zavřete hello **dmpatterns_getstarted_service.js** souboru.

## <a name="run-hello-apps"></a>Spuštění aplikace hello
Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku hello v hello **manageddevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Na příkazovém řádku hello v hello **triggerrebootondevice** složky, spusťte následující příkaz tootrigger hello vzdálené hello restartujte počítač a dotaz hello zařízení twin toofind hello restartovat poslední čas.
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. Zobrazí hello zařízení odpovědi toohello přímá metoda v konzole hello.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups toomanage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
