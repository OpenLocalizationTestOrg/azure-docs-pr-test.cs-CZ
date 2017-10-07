---
title: "aaaGet začít s Azure IoT Hub (uzel) | Microsoft Docs"
description: "Zjistěte, jak toosend zařízení cloud zprávy tooAzure IoT Hub pro Node.js pomocí sady SDK služby IoT. Vytvoření simulovaného zařízení a služby aplikace tooregister zařízení, odesílání zpráv a čtení zpráv ze služby IoT hub."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 56618522-9a31-42c6-94bf-55e2233b39ac
ms.service: iot-hub
ms.devlang: javascript
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0747895365f2359a9c38ea1e85a5881d6efec0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-node"></a>Připojení simulovaného zařízení tooyour IoT hub pomocí uzlu
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci hello tohoto kurzu máte tři aplikace konzoly Node.js:

* **CreateDeviceIdentity.js**, který vytvoří identitu zařízení a přiřazený bezpečnostní klíč tooconnect aplikace simulovaného zařízení.
* **ReadDeviceToCloudMessages.js**, který zobrazuje hello telemetrické zprávy odesílané aplikace simulovaného zařízení.
* **SimulatedDevice.js**, který připojí tooyour IoT hub s dříve vytvořenou identitou zařízení hello a odešle zprávu telemetrie každý druhý pomocí hello MQTT protokolu.

> [!NOTE]
> článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.
> 
> 

toocomplete tohoto kurzu budete potřebovat hello následující:

* Node.js verze 0.10.x nebo novější.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nyní jste vytvořili svůj IoT Hub. Máte hello název hostitele služby IoT Hub a hello připojovací řetězec služby IoT Hub, je nutné, aby toocomplete hello zbytek tohoto kurzu.

## <a name="create-a-device-identity"></a>Vytvoření identity zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která vytvoří identitu zařízení v registru identit hello ve službě IoT hub. Zařízení lze připojit pouze tooIoT rozbočovače, pokud má záznam v registru identit hello. Další informace najdete v tématu hello **registru identit** části hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity]. Při spuštění této konzolové aplikace vygeneruje jedinečné ID zařízení a klíč, že vaše zařízení použít tooidentify samotné při odešle zařízení cloud zprávy tooIoT rozbočovače.

1. Vytvořte novou prázdnou složku s názvem **createdeviceidentity**. V hello **createdeviceidentity** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **createdeviceidentity** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku sady SDK služby:
   
    ```
    npm install azure-iothub --save
    ```
3. Pomocí textového editoru, vytvořte **CreateDeviceIdentity.js** souboru v hello **createdeviceidentity** složky.
4. Přidejte následující hello `require` příkaz při spuštění hello hello **CreateDeviceIdentity.js** souboru:
   
    ```
    'use strict';
   
    var iothub = require('azure-iothub');
    ```
5. Přidejte následující kód toohello hello **CreateDeviceIdentity.js** a nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, které jste vytvořili v předchozí části hello: 
   
    ```
    var connectionString = '{iothub connection string}';
   
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```
6. Přidejte následující kód toocreate definice zařízení v registru identit hello ve službě IoT hub hello. Tento kód vytvoří zařízení, pokud ID zařízení hello neexistuje v registru identit hello, v opačném případě vrátí klíč hello hello existující zařízení:
   
    ```
    var device = {
      deviceId: 'myFirstNodeDevice'
    }
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });
   
    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device ID: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.symmetricKey.primaryKey);
      }
    }
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

7. Soubor **CreateDeviceIdentity.js** uložte a zavřete.
8. toorun hello **createdeviceidentity** aplikace, spustit následující příkaz na příkazovém řádku hello ve složce createdeviceidentity hello hello:
   
    ```
    node CreateDeviceIdentity.js 
    ```
9. Poznamenejte si hello **ID zařízení** a **klíč zařízení**. Budete potřebovat tyto hodnoty později při vytváření aplikace, která se připojuje tooIoT rozbočovače jako zařízení.

> [!NOTE]
> Hello registru identit služby IoT Hub ukládá jenom zařízení identity tooenable zabezpečený přístup toohello IoT hub. Ukládá ID a klíče toouse zařízení jako zabezpečovací přihlašovací údaje a příznak povoleno/zakázáno, které můžete toodisable přístup k jednotlivým zařízením. Pokud aplikace potřebuje toostore další metadata specifická pro zařízení, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v tématu hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].
> 
> 

<a id="D2C_node"></a>
## <a name="receive-device-to-cloud-messages"></a>Příjem zpráv typu zařízení-cloud
V této části vytvoříte konzolovou aplikaci Node.js, která čte zprávy typu zařízení-cloud ze služby IoT Hub. IoT hub zpřístupní [Event Hubs][lnk-event-hubs-overview]-koncový bod kompatibilní tooenable jste tooread zpráv typu zařízení cloud. věcí tookeep jednoduchý, v tomto kurzu vytvoří základní čtečka, která není vhodná pro vysoce výkonná nasazení. Hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurz ukazuje, jak tooprocess zařízení cloud zpráv ve velkém měřítku. Hello [Začínáme se službou Event Hubs] [ lnk-eventhubs-tutorial] kurz obsahuje další informace o tom, jak tooprocess zpráv ze služby Event Hubs a je použít toohello koncové body kompatibilní s centrem událostí služby IoT Hub.

> [!NOTE]
> Hello koncový bod kompatibilní s centrem událostí pro čtení zpráv typu zařízení cloud vždy používá protokol AMQP hello.
> 
> 

1. Vytvořte prázdnou složku s názvem **readdevicetocloudmessages**. V hello **readdevicetocloudmessages** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **readdevicetocloudmessages** složky, spusťte následující příkaz tooinstall hello hello **centra událostí azure** balíčku:
   
    ```
    npm install azure-event-hubs --save
    ```
3. Pomocí textového editoru, vytvořte **ReadDeviceToCloudMessages.js** souboru v hello **readdevicetocloudmessages** složky.
4. Přidejte následující hello `require` příkazy v hello začátek hello **ReadDeviceToCloudMessages.js** souboru:
   
    ```
    'use strict';
   
    var EventHubClient = require('azure-event-hubs').Client;
    ```
5. Přidejte následující deklarace proměnné hello a nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro vaše centrum:
   
    ```
    var connectionString = '{iothub connection string}';
    ```
6. Přidejte následující dvě funkce, které vytisknout výstup konzoly toohello hello:
   
    ```
    var printError = function (err) {
      console.log(err.message);
    };
   
    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```
7. Přidejte následující kód toocreate hello hello **EventHubClient**, otevřete hello připojení tooyour IoT Hub a vytvoření příjemce pro každý oddíl. Tato aplikace používá filtr, při vytváření přijímače tak, aby hello přijímač četl pouze zprávy odeslané tooIoT Hub až po spuštění hello příjemce. Tento filtr je užitečné v testovacím prostředí, takže se zobrazí právě hello aktuální sadu zpráv. V produkčním prostředí měli kód zpracovávat všechny zprávy hello. Další informace najdete v tématu hello [jak tooprocess zpráv typu zařízení cloud IoT Hub] [ lnk-process-d2c-tutorial] kurzu:
   
    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```
8. Uložte a zavřete hello **ReadDeviceToCloudMessages.js** souboru.

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která simuluje zařízení odesílající zprávy typu zařízení cloud tooan IoT hub.

1. Vytvořte prázdnou složku s názvem **simulateddevice**. V hello **simulateddevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello. Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt**balíčku:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru, vytvořte **SimulatedDevice.js** souboru v hello **simulateddevice** složky.
4. Přidejte následující hello `require` příkazy v hello začátek hello **SimulatedDevice.js** souboru:
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. Přidat **connectionString** proměnné a použít ho toocreate **klienta** instance. Nahraďte **{youriothostname}** s názvem hello hello IoT hub vytvořili hello *vytvoření služby IoT Hub* části. Nahraďte **{yourdevicekey}** s hodnotou klíče hello zařízení jste vygenerovali v hello *vytvoření identity zařízení* části:
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
   
    var client = clientFromConnectionString(connectionString);
    ```
6. Přidejte následující funkce toodisplay výstup z aplikace hello hello:
   
    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Vytvořte zpětné volání a použijte hello **setInterval** funkce každou sekundu toosend zpráva tooyour IoT hub:
   
    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
   
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
8. Otevřete hello připojení tooyour IoT Hub a začněte posílat zprávy:
   
    ```
    client.open(connectCallback);
    ```
9. Uložte a zavřete hello **SimulatedDevice.js** souboru.

> [!NOTE]
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].
> 
> 

## <a name="run-hello-apps"></a>Spuštění aplikace hello
Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku v hello **readdevicetocloudmessages** složky, spusťte následující příkaz toobegin monitorovat služba IoT hub hello:
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![Zařízení cloud toomonitor zprávy pro Node.js IoT Hub služby aplikace][7]
2. Na příkazovém řádku v hello **simulateddevice** složky, spusťte následující příkaz toobegin odesílání telemetrických dat tooyour IoT hub hello:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![Node.js IoT Hub zařízení aplikaci toosend zařízení cloud zprávy][8]
3. Hello **využití** dlaždici v hello [portál Azure] [ lnk-portal] ukazuje hello počet zpráv odeslaných toohello služby IoT hub:
   
    ![Azure portálu využití dlaždice zobrazuje počet zpráv odeslaných tooIoT rozbočovače][43]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci toosend zpráv typu zařízení cloud toohello Centrum IoT. Můžete také vytvořit aplikaci, která zobrazuje hello zprávy přijaté službou hello IoT hub. 

toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:

* [Připojení zařízení][lnk-connect-device]
* [Začínáme se správou zařízení][lnk-device-management]
* [Začínáme se službou Azure IoT Edge][lnk-iot-edge]

toolearn jak tooextend zpráv IoT řešení a proces zařízení cloud ve velkém měřítku, najdete v části hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurzu.
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]


<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
