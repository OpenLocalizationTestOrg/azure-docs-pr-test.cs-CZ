---
title: "úlohy aaaSchedule službou Azure IoT Hub (uzel) | Microsoft Docs"
description: "Jak tooschedule Azure IoT Hub úlohy tooinvoke přímá metoda na několika zařízeních. SDK služby Azure IoT hello se používá pro Node.js tooimplement hello simulované zařízení aplikace a úlohy hello toorun aplikace služby."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: juanpere
ms.openlocfilehash: be293362447fbcddaa3433b66f208f22545fe0c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-node"></a>Úlohy plán a všesměrového vysílání (uzel)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub je plně spravovaná služba, která umožňuje back-end aplikačním toocreate a sledování úloh, které naplánovat a aktualizovat miliony zařízení.  Úlohy lze použít pro hello následující akce:

* Aktualizace požadovaných vlastností
* Aktualizace značky
* Vyvolání metody přímé

Koncepčně úlohu zabalí jednu z těchto akcí a sleduje hello průběh provádění na skupiny zařízení, který je definován v dotazu twin zařízení.  Například back-end aplikace můžete použít tooinvoke úlohy metoda restartu na 10 000 zařízení, určeného zařízení twin dotazu a naplánovány na datum v budoucnosti.  Aplikace pak můžete sledovat průběh každé z těchto zařízení přijímat a provést metodu restartování hello.

Další informace o každém z těchto funkcí v těchto článcích:

* Dvojče zařízení a vlastností: [začít pracovat s dvojčata zařízení] [ lnk-get-started-twin] a [kurz: jak dvojče zařízení toouse vlastnosti][lnk-twin-props]
* přímé metody: [Příručka vývojáře pro službu IoT Hub - přímé metody] [ lnk-dev-methods] a [kurz: přímé metody][lnk-c2d-methods]

V tomto kurzu získáte informace o následujících postupech:

* Vytvoření aplikace simulovaného zařízení, která má přímá metoda, která umožňuje **lockDoor** které je možné volat v back-end hello řešení.
* Vytvořte konzolovou aplikaci softwaru Node.js hello tohoto volání **lockDoor** přímá metoda v aplikaci simulovaného zařízení hello pomocí úlohy a aktualizace hello požadovaných vlastností pomocí úlohy zařízení.

Na konci hello tohoto kurzu máte dvě aplikace konzoly Node.js:

**simDevice.js**, který připojí tooyour IoT hub s identitou zařízení hello a přijímá **lockDoor** přímá metoda.

**scheduleJobService.js**, která volá přímá metoda hello simulované zařízení aplikace a aktualizace hello zařízení požadované vlastnosti pro dvojici pomocí úlohy.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Node.js verze 0.12.x nebo novější, <br/>  [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Node.js pro tento kurz v systému Windows nebo Linux.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, která reaguje tooa přímá metoda volá hello cloudu, což aktivuje restartu simulované zařízení a hello používá hlášené vlastnosti tooenable zařízení twin dotazy tooidentify zařízení a při jejich poslední restartován.

1. Vytvořit novou prázdnou složku s názvem **simDevice**.  V hello **simDevice** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.  Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **simDevice** složky, spusťte následující příkaz tooinstall hello hello **azure-iot-device** balíčku sady SDK zařízení a **azure-iot zařízení mqtt** balíček:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru, vytvořte novou **simDevice.js** souboru v hello **simDevice** složky.
4. Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **simDevice.js** souboru:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Přidat **connectionString** proměnné a použít ho toocreate **klienta** instance.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Přidejte následující funkce toohandle hello hello **lockDoor** metoda.
   
    ```
    var onLockDoor = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```
7. Přidejte následující kód tooregister hello obslužné rutiny pro hello hello **lockDoor** metoda.
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect tooIotHub client.');
        }  else {
            console.log('Client connected tooIoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
8. Uložte a zavřete hello **simDevice.js** souboru.

> [!NOTE]
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Plán úlohy pro volání přímá metoda a aktualizace vlastností dvojče zařízení
V této části vytvoříte konzolovou aplikaci softwaru Node.js, který iniciuje vzdálené **lockDoor** na zařízení pomocí vlastností přímé metoda a aktualizace hello dvojče zařízení společnosti.

1. Vytvořit novou prázdnou složku s názvem **scheduleJobService**.  V hello **scheduleJobService** složky, vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku hello.  Přijměte všechny výchozí hodnoty hello:
   
    ```
    npm init
    ```
2. Na příkazovém řádku v hello **scheduleJobService** složky, spusťte následující příkaz tooinstall hello hello **azure-iothub** balíčku sady SDK zařízení a **azure-iot zařízení mqtt**balíčku:
   
    ```
    npm install azure-iothub uuid --save
    ```
3. Pomocí textového editoru, vytvořte novou **scheduleJobService.js** souboru v hello **scheduleJobService** složky.
4. Přidejte následující hello "vyžadovat" příkazy při spuštění hello hello **dmpatterns_gscheduleJobServiceetstarted_service.js** souboru:
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. Přidejte následující deklarace proměnných hello a nahraďte zástupné hodnoty hello:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. Přidejte následující funkce, které budou použité toomonitor hello provádění úlohy hello hello:
   
    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```
7. Přidejte následující kód tooschedule hello úlohu, která volá metodu zařízení hello hello:
   
    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable tooprocess method
    };
   
    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                queryCondition,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
8. Přidejte následující kód tooschedule hello úlohy tooupdate hello dvojče zařízení hello:
   
    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };
   
    var twinJobId = uuid.v4();
   
    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                queryCondition,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
9. Uložte a zavřete hello **scheduleJobService.js** souboru.

## <a name="run-hello-applications"></a>Spouštění aplikací hello
Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku hello v hello **simDevice** složky, spusťte následující příkaz toobegin naslouchání pro přímá metoda hello restartování hello.
   
    ```
    node simDevice.js
    ```
2. Na příkazovém řádku hello v hello **scheduleJobService** složky, spusťte následující příkaz tootrigger hello úlohy toolock hello dveře a aktualizace hello twin hello
   
    ```
    node scheduleJobService.js
    ```
3. Zobrazí hello zařízení odpovědi toohello přímá metoda v konzole hello.

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste použili úlohy tooschedule zařízení tooa přímá metoda a hello aktualizace vlastností dvojče zařízení hello.

toocontinue Začínáme se službou IoT Hub a vzory správy zařízení, jako je vzdálené přes hello letecké firmware aktualizace, najdete v části:

[Kurz: Jak toodo firmwarem aktualizovat][lnk-fwupdate]

najdete v části Začínáme se službou IoT Hub, toocontinue [Začínáme se službou Azure IoT Edge][lnk-iot-edge].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
