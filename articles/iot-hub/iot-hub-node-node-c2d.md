---
title: "zprávy aaaCloud zařízení s Azure IoT Hub (uzel) | Microsoft Docs"
description: "Jak toosend cloud zařízení zpráv tooa zařízení ze služby Azure IoT hub pomocí hello Azure IoT sady SDK pro Node.js. Upravte zprávy typu cloud zařízení tooreceive aplikace simulovaného zařízení a změny zpráv back-end aplikace hello toosend cloud zařízení."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3ca8a78f-ade2-46e8-8a49-d5d599cdf1f1
ms.service: iot-hub
ms.devlang: javascript
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 1ccae0cada52193c2abb91504c086cac226e93da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a>Odesílání zpráv typu cloud zařízení službou IoT Hub (uzel)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Úvod
Azure IoT Hub je plně spravovaná služba, která pomáhá povolit spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení a back-end řešení. Hello [Začínáme se službou IoT Hub] kurzu se dozvíte, jak zřídit identitu zařízení v ní toocreate služby IoT hub a kódu aplikaci ze simulovaného zařízení, která odesílá zprávy typu zařízení cloud.

V tomto kurzu vychází [Začínáme se službou IoT Hub]. Jak ukazuje na:

* Z back-end vašeho řešení odesílání zpráv typu cloud zařízení tooa jedno zařízení prostřednictvím služby IoT Hub.
* Příjem zpráv typu cloud zařízení na zařízení.
* Z back-end vašeho řešení, žádosti o potvrzení o doručení (*zpětné vazby*) pro zprávy odeslané tooa zařízení ze služby IoT Hub.

Další informace o zprávy typu cloud zařízení můžete najít v hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].

Na konci hello tohoto kurzu můžete spustit dvě aplikace konzoly Node.js:

* **SimulatedDevice**, upravenou verzi hello aplikace vytvořená v [Začínáme se službou IoT Hub], který připojí tooyour IoT hub a přijímá zprávy typu cloud zařízení.
* **SendCloudToDeviceMessage**, který odesílá aplikace simulovaného zařízení toohello zpráv typu cloud zařízení prostřednictvím služby IoT Hub a pak přijímá jeho potvrzení o doručení.

> [!NOTE]
> IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky (například C, Javy a JavaScriptu) prostřednictvím SDK pro zařízení Azure IoT. Podrobné pokyny o tom, jak tooconnect kurzu vaše zařízení toothis kódu a obecně tooAzure IoT Hub, najdete v hello [Azure střediska pro vývojáře IoT].
> 
> 

toocomplete tohoto kurzu budete potřebovat hello následující:

* Node.js verze 0.10.x nebo novější.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

## <a name="receive-messages-in-hello-simulated-device-app"></a>Příjem zpráv v aplikaci simulovaného zařízení hello
V této části upravíte jste vytvořili v aplikaci simulovaného zařízení hello [Začínáme se službou IoT Hub] tooreceive zprávy typu cloud zařízení ze služby IoT hub hello.

1. Pomocí textového editoru otevřete soubor SimulatedDevice.js hello.
2. Upravit hello **connectCallback** funkce toohandle zprávy odeslané ze služby IoT Hub. V tomto příkladu hello zařízení vždy vyvolá hello **dokončení** funkce toonotify IoT Hub, že zpracovala uvítací zprávu. Vaše nové verze hello **connectCallback** funkce vypadá hello následující fragment kódu:
   
    ```javascript
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it toohello IoT Hub every second
        setInterval(function(){
            var temperature = 20 + (Math.random() * 15);
            var humidity = 60 + (Math.random() * 20);            
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
            var message = new Message(data);
            message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
   
   > [!NOTE]
   > Pokud používáte protokol HTTP místo MQTT nebo AMQP jako hello přenosu, hello **DeviceClient** instance vyhledává zprávy ze služby IoT Hub zřídka (méně než každých 25 minut). Další informace o hello rozdíly mezi podpora MQTT, AMQP a HTTP a omezení služby IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a>Odeslání zprávy typu cloud zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která odešle aplikace simulovaného zařízení toohello zprávy typu cloud zařízení. ID zařízení hello zařízení, které jste přidali v hello potřebovat hello [Začínáme se službou IoT Hub] kurzu. Můžete také potřebovat hello připojovací řetězec služby IoT Hub pro vaše centrum, které můžete najít v hello [portál Azure].

1. Vytvořit prázdnou složku s názvem **sendcloudtodevicemessage**. V hello **sendcloudtodevicemessage** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```shell
    npm init
    ```
2. Na příkazovém řádku v hello **sendcloudtodevicemessage** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku:
   
    ```shell
    npm install azure-iothub --save
    ```
3. Pomocí textového editoru, vytvořte **SendCloudToDeviceMessage.js** souboru v hello **sendcloudtodevicemessage** složky.
4. Přidejte následující hello `require` příkazy v hello začátek hello **SendCloudToDeviceMessage.js** souboru:
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. Přidejte následující kód příliš hello**SendCloudToDeviceMessage.js** souboru. Nahraďte hodnotu zástupného symbolu hello "{iot hub připojovací řetězec}" hello připojovací řetězec služby IoT Hub pro hello rozbočovače, které jste vytvořili v hello [Začínáme se službou IoT Hub] kurzu. Nahraďte zástupný symbol hello "{zařízení id} s ID zařízení hello hello zařízení, které jste přidali v hello [Začínáme se službou IoT Hub] kurzu:
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. Přidejte následující funkce tooprint operaci výsledky toohello konzoly hello:
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Přidejte následující funkce tooprint doručení zpětnou vazbu zprávy toohello konzoly hello:
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. Přidejte následující hello kód toosend zařízení tooyour zprávy a zpracování zprávy hello zpětné vazby, pokud zařízení hello uznává uvítací zprávu cloud zařízení:
   
    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud toodevice message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```
9. Uložte a zavřete **SendCloudToDeviceMessage.js** souboru.

## <a name="run-hello-applications"></a>Spouštění aplikací hello
Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku hello v hello **simulateddevice** spusťte hello následující příkaz toosend telemetrie tooIoT rozbočovače a toolisten pro zprávy typu cloud zařízení:
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![Spusťte aplikaci simulovaného zařízení hello][img-simulated-device]
2. Na příkazovém řádku v hello **sendcloudtodevicemessage** složky, spusťte následující příkaz toosend hello zprávy typu cloud zařízení a počkat na zpětnou vazbu hello potvrzení:
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![Spusťte hello aplikace toosend hello cloudu do zařízení příkaz][img-send-command]
   
   > [!NOTE]
   > Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN hello [přechodných chyb].
   > 
   > 

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili jak toosend a příjem zpráv typu cloud zařízení. 

Příklady toosee dokončení začátku do konce řešení, které pomocí služby IoT Hub, viz [Azure IoT Suite].

toolearn Další informace o vývoji řešení službou IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Začínáme se službou IoT Hub]: iot-hub-node-node-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md
[Azure střediska pro vývojáře IoT]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[portál Azure]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
