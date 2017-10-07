---
title: "operace služby IoT Hub aaaAzure monitorování | Microsoft Docs"
description: "Jak operace Azure IoT Hub toouse monitorování toomonitor hello stav operace ve službě IoT hub v reálném čase."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a299f3a5-b14d-4586-9c3b-44aea14ed013
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.openlocfilehash: a0b233ef2d9bd0827e19fa30fdbdd49b2b61b813
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="iot-hub-operations-monitoring"></a>Operace služby IoT Hub monitorování

Operace služby IoT Hub monitorování umožňuje toomonitor hello stav operace ve službě IoT hub v reálném čase. IoT Hub sleduje události napříč několika kategorií operací. Můžete se taky rozhodnout do odesílání událostí z jednoho nebo více kategorií tooan koncového bodu služby IoT hub pro zpracování. Můžete sledovat data hello chyby nebo nastavit složitější zpracování vámi data.

IoT Hub monitoruje šesti kategorie události:

* Operace identity zařízení
* Zařízení telemetrie
* Zprávy typu cloud zařízení
* Připojení
* Nahrávání souborů
* Směrování zpráv

## <a name="how-tooenable-operations-monitoring"></a>Jak operace tooenable monitorování

1. Vytvoření služby IoT hub. Pokyny naleznete v tom toocreate služby IoT hub v hello [Začínáme] [ lnk-get-started] průvodce.

1. Otevřete okno hello služby IoT hub. Zde, klikněte na tlačítko **Operations monitorování**.

    ![Monitorování konfigurace portálu hello operace přístupu.][1]

1. Vyberte hello monitorování kategorií chcete toomonitor a potom klikněte na **Uložit**. Hello události jsou k dispozici pro čtení z koncového bodu kompatibilního s centrem událostí hello uvedené v **nastavení sledování**. Hello koncový bod centra IoT se nazývá `messages/operationsmonitoringevents`.

    ![Konfigurace operací monitorování ve službě IoT hub][2]

> [!NOTE]
> Výběr **podrobné** monitorování hello **připojení** kategorie způsobí, že IoT Hub toogenerate hlášení další diagnostiky. U všech ostatních kategorií hello **podrobné** změny nastavení hello množství informací IoT Hub obsahuje v každém chybová zpráva.

## <a name="event-categories-and-how-toouse-them"></a>Kategorie události a jak toouse je

Každý operations monitorování sleduje kategorie má jiný typ interakci s IoT Hub a každou kategorii monitorování schéma, které definuje, jak jsou strukturovaná události v této kategorii.

### <a name="device-identity-operations"></a>Operace identity zařízení

kategorie operations identity zařízení Hello sleduje chyby, ke kterým dochází, když se pokusíte toocreate, aktualizovat nebo odstranit položku, která v registru identit služby IoT hub. Sledování této kategorie jsou užitečné pro zřizování scénáře.

```json
{
    "time": "UTC timestamp",
        "operationName": "create",
        "category": "DeviceIdentityOperations",
        "level": "Error",
        "statusCode": 4XX,
        "statusDescription": "MessageDescription",
        "deviceId": "device-ID",
        "durationMs": 1234,
        "userAgent": "userAgent",
        "sharedAccessPolicy": "accessPolicy"
}
```

### <a name="device-telemetry"></a>Zařízení telemetrie

kategorie telemetrie zařízení Hello sleduje chybách, ke kterým dochází při hello IoT hub a jsou související toohello telemetrie kanálu. Tato kategorie zahrnuje chyb vzniklých při odesílání telemetrie událostí (například omezení šířky pásma) a příjem telemetrické události (například neoprávněným čtečky). Tuto kategorii nelze catch chyby způsobené kód spuštěný v samotném hello zařízení.

```json
{
        "messageSizeInBytes": 1234,
        "batching": 0,
        "protocol": "Amqp",
        "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
        "time": "UTC timestamp",
        "operationName": "ingress",
        "category": "DeviceTelemetry",
        "level": "Error",
        "statusCode": 4XX,
        "statusType": 4XX001,
        "statusDescription": "MessageDescription",
        "deviceId": "device-ID",
        "EventProcessedUtcTime": "UTC timestamp",
        "PartitionId": 1,
        "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="cloud-to-device-commands"></a>Příkazy typu cloud zařízení

kategorie příkazy typu cloud zařízení Hello sleduje chybách, ke kterým dochází při hello IoT hub a jsou kanálu související toohello zpráv typu cloud zařízení. Tato kategorie zahrnuje chyb vzniklých při odesílání zpráv typu cloud zařízení (například neautorizovaného odesílatele), přijímání zpráv typu cloud zařízení (například počet doručení překročena) a přijímání zpráv typu cloud zařízení zpětnou vazbu (například zpětné vazby jeho platnost). Tuto kategorii nezachytí chyby ze zařízení, která nesprávně zpracovává zprávy typu cloud zařízení, pokud zpráva hello cloud zařízení byla úspěšně doručeno.

```json
{
    "messageSizeInBytes": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "deliveryAcknowledgement": 0,
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "C2DCommands",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": “UTC timestamp"
}
```

### <a name="connections"></a>Připojení

kategorie připojení Hello sleduje chyb vzniklých při zařízení připojení nebo odpojení od služby IoT hub. Sledování této kategorie jsou užitečné pro identifikaci pokusy o neoprávněné připojení a sledování při ztrátě pro zařízení v oblastech malé možnosti připojení připojení.

```json
{
    "durationMs": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID"
}
```

### <a name="file-uploads"></a>Nahrávání souborů

kategorie nahrávání souboru Hello sleduje chybách, ke kterým dochází při hello IoT hub a jsou související toofile odesílání funkce. Tato kategorie zahrnuje:

* Chyby, které se hello identifikátor URI SAS, jako je například vypršení platnosti před zařízení upozorní hello rozbočovače dokončené odeslání.
* Nepodařilo nahrávání hlášené hello zařízení.
* Chyby, které nastat, pokud soubor nebyl nalezen v úložišti během vytváření zpráv oznámení služby IoT Hub.

Tuto kategorii nelze catch chyb, které přímo nastat při hello zařízení odesílá toostorage souboru.

```json
{
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "HTTP",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "fileUpload",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "blobUri": "http//bloburi.com",
    "durationMs": 1234
}
```

### <a name="message-routing"></a>Směrování zpráv

kategorie směrování zpráv Hello sleduje chyb vzniklých při vyhodnocení zpráva trasy a stav koncového bodu zaznamenatelného službou IoT Hub. Tato kategorie zahrnuje události, například pokud se pravidlo vyhodnotí jako příliš "undefined", když IoT Hub označí koncový bod zpráv a všechny ostatní chyby přijaté z koncového bodu. Tato kategorie nezahrnuje konkrétní chyby týkající se zpráv hello, sami (například zařízení omezení chyby), které jsou v části kategorie "zařízení telemetrická" hello.

```json
{
    "messageSizeInBytes": 1234,
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "routes",
    "level": "Error",
    "deviceId": "device-ID",
    "messageId": "ID of message",
    "routeName": "myroute",
    "endpointName": "myendpoint",
    "details": "ExternalEndpointDisabled"
}
```

## <a name="view-events"></a>zobrazení událostí

Můžete použít hello *iothub-explorer* tooquickly nástroj testování, aby služby IoT hub je generování události monitorování. tooinstall hello nástroje najdete v pokynech hello v hello [iothub-explorer] [ lnk-iothub-explorer] úložiště GitHub.

1. Ujistěte se, zda text hello **připojení** monitorování kategorie je nastaven příliš**podrobné** hello portálu.

1. V příkazovém řádku spusťte následující příkaz tooread z koncového bodu monitorování hello hello:

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. V jiné příkazového řádku spusťte následující příkaz toosimulate hello zařízení odesílání zpráv typu zařízení cloud:

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. Hello první příkazového řádku ukazuje události monitorování hello hello simulované zařízení připojí tooyour služby IoT hub.

## <a name="connect-toohello-monitoring-endpoint"></a>Připojit toohello koncového bodu monitorování

Hello monitorování koncového bodu ve službě IoT hub je koncový bod kompatibilní s centrem událostí. Můžete použít jakéhokoli mechanismu, který funguje s Event Hubs tooread monitorování zprávy z tohoto koncového bodu. Hello následující ukázka vytvoří základní čtečka, která není vhodná pro vysoce výkonná nasazení. Další informace o tom, jak tooprocess zpráv ze služby Event Hubs naleznete v tématu hello [Začínáme se službou Event Hubs] [ lnk-eventhubs-tutorial] kurzu.

tooconnect toohello koncového bodu monitorování, je třeba název připojovacího řetězce a hello koncový bod. Hello následující kroky ukazují, jak toofind hello nezbytných hodnot hello portálu:

1. Hello portálu přejděte tooyour okna prostředků služby IoT Hub.

1. Zvolte **Operations monitorování**a poznamenejte si hello **název kompatibilní s centrem událostí** a **koncový bod kompatibilní s centrem událostí** hodnoty:

    ![Hodnoty koncový bod kompatibilní s centrem událostí][img-endpoints]

1. Zvolte **zásady sdíleného přístupu**, zvolte **služby**. Poznamenejte si hello **primární klíč** hodnotu:

    ![Primární klíč pro zásady sdíleného přístupu služby][img-service-key]

Hello následující ukázka kódu C# jsou převzaty ze sady Visual Studio **Windows Classic Desktop** konzolovou aplikaci C#. Hello projekt má hello **WindowsAzure.ServiceBus** nainstalovat balíček NuGet.

* Nahraďte zástupný symbol připojovacího řetězce hello připojovací řetězec, který používá hello **koncový bod kompatibilní s centrem událostí** a služba **primární klíč** hodnoty, které jste si poznamenali dříve, jak je znázorněno v následující hello Příklad:

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* Nahraďte hello monitorování název koncového bodu zástupný symbol hello **název kompatibilní s centrem událostí** hodnoty, které jste si poznamenali dříve.

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key tooexit.\n");

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();

        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }

        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }

            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));

            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }
    }
}
```

## <a name="next-steps"></a>Další kroky
toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Simulaci zařízení s Azure IoT Edge][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png
[img-endpoints]: media/iot-hub-operations-monitoring/monitoring-endpoint.png
[img-service-key]: media/iot-hub-operations-monitoring/service-key.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md