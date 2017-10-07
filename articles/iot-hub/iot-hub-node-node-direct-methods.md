---
title: "aaaAzure IoT Hub přímé metody (uzel) | Microsoft Docs"
description: "Jak toouse Azure IoT Hub přímé metody. SDK služby Azure IoT hello se používá pro Node.js tooimplement aplikaci simulovaného zařízení, která zahrnuje přímá metoda a aplikační služby, která volá metodu přímé hello."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ea9c73ca-7778-4e38-a8f1-0bee9d142f04
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 12300ba451816fec1f80163b633f6b6e411d9e5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a>Použití přímé metody na zařízení IoT s Node.js
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Na konci hello tohoto kurzu máte dvě aplikace konzoly Node.js:

* **CallMethodOnDevice.js**, která volá metodu v aplikaci simulovaného zařízení hello a zobrazí hello odpovědi.
* **SimulatedDevice.js**, který připojí tooyour IoT hub s dříve vytvořenou identitou zařízení hello a reaguje toohello metodu s názvem hello cloudem.

> [!NOTE]
> článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.
> 
> 

toocomplete tohoto kurzu budete potřebovat hello následující:

* Node.js verze 0.10.x nebo novější.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci Node.js, která odpovídá tooa metodu s názvem hello cloudem.

1. Vytvořte novou prázdnou složku s názvem **simulateddevice**. V hello **simulateddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt**balíčku:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru, vytvořte novou **SimulatedDevice.js** souboru v hello **simulateddevice** složky.
4. Přidejte následující hello `require` příkazy v hello začátek hello **SimulatedDevice.js** souboru:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Přidat **connectionString** proměnné a použít ho toocreate **DeviceClient** instance. Nahraďte **{zařízení připojovací řetězec}** s řetězcem připojení zařízení hello jste vygenerovali v hello *vytvoření identity zařízení* části:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Přidejte následující metodu hello tooimplement funkce v zařízení hello hello:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written toolog.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Otevřete Centrum IoT tooyour hello připojení a spusťte inicializovat hello metoda naslouchací proces:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Uložte a zavřete hello **SimulatedDevice.js** souboru.

> [!NOTE]
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například opakování připojení), dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].
> 
> 

## <a name="call-a-method-on-a-device"></a>Volání metody na zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která volá metodu v aplikaci simulovaného zařízení hello a potom zobrazí hello odpovědi.

1. Vytvořit novou prázdnou složku s názvem **callmethodondevice**. V hello **callmethodondevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **callmethodondevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku:
   
    ```
    npm install azure-iothub --save
    ```
3. Pomocí textového editoru, vytvořte **CallMethodOnDevice.js** souboru v hello **callmethodondevice** složky.
4. Přidejte následující hello `require` příkazy v hello začátek hello **CallMethodOnDevice.js** souboru:
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. Přidejte následující deklarace proměnné hello a nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro vaše centrum:
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. Vytvoření centra IoT tooyour hello hello klienta tooopen připojení.
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. Přidejte následující funkce tooinvoke hello zařízení metoda tiskových hello zařízení odpovědi toohello konzoly a hello:
   
    ```
    var methodParams = {
        methodName: methodName,
        payload: 'hello world',
        timeoutInSeconds: 30
    };
   
    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed tooinvoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```
8. Uložte a zavřete hello **CallMethodOnDevice.js** souboru.

## <a name="run-hello-apps"></a>Spuštění aplikace hello
Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz toostart přijímá metoda volání ze služby IoT Hub hello:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. Na příkazovém řádku v hello **callmethodondevice** složky, spusťte následující příkaz toobegin monitorovat služba IoT hub hello:
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. Zobrazí se zařízení hello reagovat toohello metoda tiskem uvítací zprávu a hello aplikace, který označuje hello metoda zobrazení hello odpověď z hello zařízení:
   
    ![][9]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci tooreact toomethods vyvolané hello cloudu. Můžete také vytvořit aplikaci, která volá metody na hello zařízení a zobrazí hello odezvu hello zařízení. 

toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:

* [Začínáme s centrem IoT]
* [Plánování úloh na několika zařízeních][lnk-devguide-jobs]

toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-node-node-direct-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-node-node-direct-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Začínáme s centrem IoT]: iot-hub-node-node-getstarted.md
