---
title: "aaaGet spuštění s Azure IoT Hub (.NET) | Microsoft Docs"
description: "Zjistěte, jak toosend zařízení cloud zprávy tooAzure IoT Hub pomocí sady SDK služby IoT pro .NET. Vytvoření simulovaného zařízení a služby aplikace tooregister zařízení, odesílání zpráv a čtení zpráv ze služby IoT hub."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56cf14687411898ea0fa4ebb1782e18b3930809c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-tooyour-iot-hub-using-net"></a>Připojení zařízení tooyour IoT hub pomocí rozhraní .NET

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci hello tohoto kurzu máte tři aplikace konzoly .NET:

* **CreateDeviceIdentity**, který vytvoří identitu zařízení a přiřazený bezpečnostní klíč tooconnect aplikace zařízení.
* **ReadDeviceToCloudMessages**, který zobrazuje hello telemetrické zprávy odesílané aplikace zařízení.
* **SimulatedDevice**, který připojí tooyour IoT hub s dříve vytvořenou identitou zařízení hello a odešle zprávu telemetrie každou sekundu pomocí protokolu MQTT hello.

Můžete stáhnout nebo naklonovat hello řešení sady Visual Studio, který obsahuje tři aplikace hello z Githubu.

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> Informace o hello SDK služby Azure IoT, které můžete použít toobuild toorun aplikace na zařízení a back-end vašeho řešení, najdete v části [SDK služby Azure IoT][lnk-hub-sdks].

toocomplete tohoto kurzu budete potřebovat hello následující:

* Visual Studio 2015 nebo Visual Studio 2017.
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nyní jste vytvořili službu IoT hub a máte název hostitele hello a připojovací řetězec služby IoT Hub, je nutné, aby toocomplete hello zbytek tohoto kurzu.

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a>Příjem zpráv typu zařízení-cloud
V této části vytvoříte konzolovou aplikaci .NET, která čte zprávy typu zařízení-cloud ze služby IoT Hub. IoT hub zpřístupní [Azure Event Hubs][lnk-event-hubs-overview]-koncový bod kompatibilní tooenable jste tooread zpráv typu zařízení cloud. věcí tookeep jednoduchý, v tomto kurzu vytvoří základní čtečka, která není vhodná pro vysoce výkonná nasazení. toolearn jak tooprocess zařízení cloud zpráv ve velkém měřítku, najdete v části hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurzu. Další informace o tom, jak tooprocess zpráv ze služby Event Hubs naleznete v tématu hello [Začínáme se službou Event Hubs] [ lnk-eventhubs-tutorial] kurzu. (V tomto kurzu je koncových bodů použít toohello kompatibilní s centrem událostí služby IoT Hub.)

> [!NOTE]
> Hello koncový bod kompatibilní s centrem událostí pro čtení zpráv typu zařízení cloud vždy používá protokol AMQP hello.

1. V sadě Visual Studio, přidejte Visual C# Windows klasický desktopový projekt toohello aktuální řešení, pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu. Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější. Název projektu hello **ReadDeviceToCloudMessages**.

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10a]

2. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ReadDeviceToCloudMessages** projektu a pak klikněte na **spravovat balíčky NuGet**.

3. V hello **Správce balíčků NuGet** vyhledejte **WindowsAzure.ServiceBus**, vyberte **nainstalovat**a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz příliš[Azure Service Bus][lnk-servicebus-nuget], se všemi jeho závislostmi. Tento balíček umožní koncový bod hello aplikace tooconnect toohello kompatibilní s centrem událostí ve službě IoT hub.

4. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, kterou jste vytvořili v části "Vytvoření služby IoT hub" hello.

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. Přidejte následující metodu toohello hello **Program** třídy:

    ```csharp
    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested) break;
            EventData eventData = await eventHubReceiver.ReceiveAsync();
            if (eventData == null) continue;

            string data = Encoding.UTF8.GetString(eventData.GetBytes());
            Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
        }
    }
    ```

    Tato metoda používá **EventHubReceiver** instance tooreceive zprávy ze všech hello IoT hub zařízení cloud přijímat oddíly. Všimněte si, jak předat `DateTime.Now` parametr při vytváření hello **EventHubReceiver** objektu, tak, aby přijímal pouze zprávy odeslané po spuštění. Tento filtr je užitečné v testovacím prostředí, takže uvidíte aktuální sadu zpráv hello. V produkčním prostředí měli kód zpracovávat všechny zprávy hello. Další informace najdete v tématu hello kurzu [jak tooprocess zpráv typu zařízení cloud IoT Hub][lnk-process-d2c-tutorial].

7. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:

    ```csharp
    Console.WriteLine("Receive messages. Ctrl-C tooexit.\n");
    eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

    var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

    CancellationTokenSource cts = new CancellationTokenSource();

    System.Console.CancelKeyPress += (s, e) =>
    {
        e.Cancel = true;
        cts.Cancel();
        Console.WriteLine("Exiting...");
    };

    var tasks = new List<Task>();
    foreach (string partition in d2cPartitions)
    {
        tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
    }  
    Task.WaitAll(tasks.ToArray());
    ```

## <a name="create-a-device-app"></a>Vytvoření aplikace pro zařízení

V této části vytvoříte konzolovou aplikaci .NET, která simuluje zařízení odesílající zprávy typu zařízení cloud tooan IoT hub.

1. V sadě Visual Studio, přidejte Visual C# Windows klasický desktopový projekt toohello aktuální řešení, pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu. Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější. Název projektu hello **SimulatedDevice**.

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10b]

2. V Průzkumníku řešení klikněte pravým tlačítkem na hello **SimulatedDevice** projektu a pak klikněte na **spravovat balíčky NuGet**.

3. V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **Microsoft.Azure.Devices.Client**, vyberte **nainstalovat** tooinstall hello **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz toohello [balíček NuGet sady SDK pro zařízení Azure IoT] [ lnk-device-nuget] a jeho závislosti.

4. Přidejte následující hello `using` příkaz hello horní části hello **Program.cs** souboru:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. Přidejte následující pole toohello hello **Program** třídy. SUBSTITUTE `{iot hub hostname}` hello IoT hub názvem hostitele, který jste získali v části "Vytvoření služby IoT hub" hello. SUBSTITUTE `{device key}` klíčem hello zařízení, který jste získali v části "Vytvoření identity zařízení" hello.

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. Přidejte následující metodu toohello hello **Program** třídy:

    ```csharp
    private static async void SendDeviceToCloudMessagesAsync()
    {
        double minTemperature = 20;
        double minHumidity = 60;
        int messageId = 1;
        Random rand = new Random();

        while (true)
        {
            double currentTemperature = minTemperature + rand.NextDouble() * 15;
            double currentHumidity = minHumidity + rand.NextDouble() * 20;

            var telemetryDataPoint = new
            {
                messageId = messageId++,
                deviceId = "myFirstDevice",
                temperature = currentTemperature,
                humidity = currentHumidity
            };
            var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
            var message = new Message(Encoding.ASCII.GetBytes(messageString));
            message.Properties.Add("temperatureAlert", (currentTemperature > 30) ? "true" : "false");

            await deviceClient.SendEventAsync(message);
            Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

            await Task.Delay(1000);
        }
    }
    ```

    Tato metoda odesílá novou zprávu typu zařízení-cloud každou sekundu. Hello zpráva obsahuje objekt serializovaný JSON, s ID zařízení hello a náhodně vygenerované čísla toosimulate senzor teploty a vlhkosti senzoru.

7. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);

    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    Ve výchozím nastavení, hello **vytvořit** metoda v rozhraní .NET Framework aplikace vytvoří **DeviceClient** instanci, která používá toocommunicate protokol AMQP hello službou IoT Hub. hello toouse protokol MQTT nebo HTTP, používat hello přepsání hello **vytvořit** metoda, která vám umožní toospecify hello protokolu. UWP a PCL klienti používat protokol HTTP hello ve výchozím nastavení. Pokud používáte protokol hello HTTP, měli byste také přidat hello **Microsoft.AspNet.WebApi.Client** NuGet balíček tooyour projektu tooinclude hello **System.Net.Http.Formatting** oboru názvů.

Tento kurz vás provede kroky toocreate hello služby IoT Hub zařízení aplikaci. Můžete taky hello [připojená služba pro službu Azure IoT Hub] [ lnk-connected-service] tooadd rozšíření sady Visual Studio, hello aplikaci zařízení tooyour nezbytného kódu.

> [!NOTE]
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].

## <a name="run-hello-apps"></a>Spuštění aplikace hello

Nyní je připraven toorun hello aplikace.

1. V sadě Visual Studio v Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom klikněte na tlačítko **Nastavit projekty po spuštění**. Vyberte **více projektů po spuštění**a potom vyberte **spustit** jako hello akci pro oba hello **ReadDeviceToCloudMessages** a **SimulatedDevice** projekty.

    ![Vlastnosti projektu po spuštění][41]

2. Stiskněte klávesu **F5** toostart obě aplikace. výstup konzoly z hello Hello **SimulatedDevice** zprávy hello aplikace zobrazí vaše aplikace zařízení odesílá tooyour IoT hub. výstup konzoly z hello Hello **ReadDeviceToCloudMessages** aplikace zobrazí hello zprávy, které IoT hub přijímá.

    ![Výstup konzoly z aplikací][42]

3. Hello **využití** dlaždici v hello [portál Azure] [ lnk-portal] ukazuje hello počet zpráv odeslaných toohello služby IoT hub:

    ![Dlaždice Využití na portálu Azure Portal][43]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali služby IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Použili jste toto zařízení identity tooenable hello zařízení aplikaci toosend zpráv typu zařízení cloud toohello Centrum IoT. Můžete také vytvořit aplikaci, která zobrazuje hello zprávy přijaté službou hello IoT hub.

toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:

* [Připojení zařízení][lnk-connect-device]
* [Začínáme se správou zařízení][lnk-device-management]
* [Začínáme se službou IoT Edge][lnk-iot-edge]

toolearn jak tooextend zpráv IoT řešení a proces zařízení cloud ve velkém měřítku, najdete v části hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurzu.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10a]: ./media/iot-hub-csharp-csharp-getstarted/create-receive-csharp1.png
[10b]: ./media/iot-hub-csharp-csharp-getstarted/create-device-csharp1.png


<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
