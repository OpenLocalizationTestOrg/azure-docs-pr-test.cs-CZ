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
# <a name="connect-your-device-tooyour-iot-hub-using-net"></a><span data-ttu-id="23a07-104">Připojení zařízení tooyour IoT hub pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="23a07-104">Connect your device tooyour IoT hub using .NET</span></span>

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="23a07-105">Na konci hello tohoto kurzu máte tři aplikace konzoly .NET:</span><span class="sxs-lookup"><span data-stu-id="23a07-105">At hello end of this tutorial, you have three .NET console apps:</span></span>

* <span data-ttu-id="23a07-106">**CreateDeviceIdentity**, který vytvoří identitu zařízení a přiřazený bezpečnostní klíč tooconnect aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="23a07-106">**CreateDeviceIdentity**, which creates a device identity and associated security key tooconnect your device app.</span></span>
* <span data-ttu-id="23a07-107">**ReadDeviceToCloudMessages**, který zobrazuje hello telemetrické zprávy odesílané aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="23a07-107">**ReadDeviceToCloudMessages**, which displays hello telemetry sent by your device app.</span></span>
* <span data-ttu-id="23a07-108">**SimulatedDevice**, který připojí tooyour IoT hub s dříve vytvořenou identitou zařízení hello a odešle zprávu telemetrie každou sekundu pomocí protokolu MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="23a07-108">**SimulatedDevice**, which connects tooyour IoT hub with hello device identity created earlier, and sends a telemetry message every second by using hello MQTT protocol.</span></span>

<span data-ttu-id="23a07-109">Můžete stáhnout nebo naklonovat hello řešení sady Visual Studio, který obsahuje tři aplikace hello z Githubu.</span><span class="sxs-lookup"><span data-stu-id="23a07-109">You can download or clone hello Visual Studio solution, which contains hello three apps from Github.</span></span>

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> <span data-ttu-id="23a07-110">Informace o hello SDK služby Azure IoT, které můžete použít toobuild toorun aplikace na zařízení a back-end vašeho řešení, najdete v části [SDK služby Azure IoT][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="23a07-110">For information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices, and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="23a07-111">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="23a07-111">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="23a07-112">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="23a07-112">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="23a07-113">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="23a07-113">An active Azure account.</span></span> <span data-ttu-id="23a07-114">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="23a07-114">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="23a07-115">Nyní jste vytvořili službu IoT hub a máte název hostitele hello a připojovací řetězec služby IoT Hub, je nutné, aby toocomplete hello zbytek tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="23a07-115">You have now created your IoT hub, and you have hello host name and IoT Hub connection string that you need toocomplete hello rest of this tutorial.</span></span>

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="23a07-116">Příjem zpráv typu zařízení-cloud</span><span class="sxs-lookup"><span data-stu-id="23a07-116">Receive device-to-cloud messages</span></span>
<span data-ttu-id="23a07-117">V této části vytvoříte konzolovou aplikaci .NET, která čte zprávy typu zařízení-cloud ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="23a07-117">In this section, you create a .NET console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="23a07-118">IoT hub zpřístupní [Azure Event Hubs][lnk-event-hubs-overview]-koncový bod kompatibilní tooenable jste tooread zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="23a07-118">An IoT hub exposes an [Azure Event Hubs][lnk-event-hubs-overview]-compatible endpoint tooenable you tooread device-to-cloud messages.</span></span> <span data-ttu-id="23a07-119">věcí tookeep jednoduchý, v tomto kurzu vytvoří základní čtečka, která není vhodná pro vysoce výkonná nasazení.</span><span class="sxs-lookup"><span data-stu-id="23a07-119">tookeep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="23a07-120">toolearn jak tooprocess zařízení cloud zpráv ve velkém měřítku, najdete v části hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="23a07-120">toolearn how tooprocess device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span> <span data-ttu-id="23a07-121">Další informace o tom, jak tooprocess zpráv ze služby Event Hubs naleznete v tématu hello [Začínáme se službou Event Hubs] [ lnk-eventhubs-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="23a07-121">For more information about how tooprocess messages from Event Hubs, see hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial.</span></span> <span data-ttu-id="23a07-122">(V tomto kurzu je koncových bodů použít toohello kompatibilní s centrem událostí služby IoT Hub.)</span><span class="sxs-lookup"><span data-stu-id="23a07-122">(This tutorial is applicable toohello IoT Hub Event Hub-compatible endpoints.)</span></span>

> [!NOTE]
> <span data-ttu-id="23a07-123">Hello koncový bod kompatibilní s centrem událostí pro čtení zpráv typu zařízení cloud vždy používá protokol AMQP hello.</span><span class="sxs-lookup"><span data-stu-id="23a07-123">hello Event Hub-compatible endpoint for reading device-to-cloud messages always uses hello AMQP protocol.</span></span>

1. <span data-ttu-id="23a07-124">V sadě Visual Studio, přidejte Visual C# Windows klasický desktopový projekt toohello aktuální řešení, pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="23a07-124">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution, by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="23a07-125">Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="23a07-125">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="23a07-126">Název projektu hello **ReadDeviceToCloudMessages**.</span><span class="sxs-lookup"><span data-stu-id="23a07-126">Name hello project **ReadDeviceToCloudMessages**.</span></span>

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10a]

2. <span data-ttu-id="23a07-128">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ReadDeviceToCloudMessages** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="23a07-128">In Solution Explorer, right-click hello **ReadDeviceToCloudMessages** project, and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="23a07-129">V hello **Správce balíčků NuGet** vyhledejte **WindowsAzure.ServiceBus**, vyberte **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="23a07-129">In hello **NuGet Package Manager** window, search for **WindowsAzure.ServiceBus**, select **Install**, and accept hello terms of use.</span></span> <span data-ttu-id="23a07-130">Tento postup stáhne, nainstaluje a přidá odkaz příliš[Azure Service Bus][lnk-servicebus-nuget], se všemi jeho závislostmi.</span><span class="sxs-lookup"><span data-stu-id="23a07-130">This procedure downloads, installs, and adds a reference too[Azure Service Bus][lnk-servicebus-nuget], with all its dependencies.</span></span> <span data-ttu-id="23a07-131">Tento balíček umožní koncový bod hello aplikace tooconnect toohello kompatibilní s centrem událostí ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="23a07-131">This package enables hello application tooconnect toohello Event Hub-compatible endpoint on your IoT hub.</span></span>

4. <span data-ttu-id="23a07-132">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="23a07-132">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. <span data-ttu-id="23a07-133">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="23a07-133">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="23a07-134">Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, kterou jste vytvořili v části "Vytvoření služby IoT hub" hello.</span><span class="sxs-lookup"><span data-stu-id="23a07-134">Replace hello placeholder value with hello IoT Hub connection string for hello hub you created in hello "Create an IoT hub" section.</span></span>

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. <span data-ttu-id="23a07-135">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="23a07-135">Add hello following method toohello **Program** class:</span></span>

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

    <span data-ttu-id="23a07-136">Tato metoda používá **EventHubReceiver** instance tooreceive zprávy ze všech hello IoT hub zařízení cloud přijímat oddíly.</span><span class="sxs-lookup"><span data-stu-id="23a07-136">This method uses an **EventHubReceiver** instance tooreceive messages from all hello IoT hub device-to-cloud receive partitions.</span></span> <span data-ttu-id="23a07-137">Všimněte si, jak předat `DateTime.Now` parametr při vytváření hello **EventHubReceiver** objektu, tak, aby přijímal pouze zprávy odeslané po spuštění.</span><span class="sxs-lookup"><span data-stu-id="23a07-137">Notice how you pass a `DateTime.Now` parameter when you create hello **EventHubReceiver** object, so that it only receives messages sent after it starts.</span></span> <span data-ttu-id="23a07-138">Tento filtr je užitečné v testovacím prostředí, takže uvidíte aktuální sadu zpráv hello.</span><span class="sxs-lookup"><span data-stu-id="23a07-138">This filter is useful in a test environment so you can see hello current set of messages.</span></span> <span data-ttu-id="23a07-139">V produkčním prostředí měli kód zpracovávat všechny zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="23a07-139">In a production environment, your code should make sure that it processes all hello messages.</span></span> <span data-ttu-id="23a07-140">Další informace najdete v tématu hello kurzu [jak tooprocess zpráv typu zařízení cloud IoT Hub][lnk-process-d2c-tutorial].</span><span class="sxs-lookup"><span data-stu-id="23a07-140">For more information, see hello tutorial [How tooprocess IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial].</span></span>

7. <span data-ttu-id="23a07-141">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="23a07-141">Finally, add hello following lines toohello **Main** method:</span></span>

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

## <a name="create-a-device-app"></a><span data-ttu-id="23a07-142">Vytvoření aplikace pro zařízení</span><span class="sxs-lookup"><span data-stu-id="23a07-142">Create a device app</span></span>

<span data-ttu-id="23a07-143">V této části vytvoříte konzolovou aplikaci .NET, která simuluje zařízení odesílající zprávy typu zařízení cloud tooan IoT hub.</span><span class="sxs-lookup"><span data-stu-id="23a07-143">In this section, you create a .NET console app that simulates a device that sends device-to-cloud messages tooan IoT hub.</span></span>

1. <span data-ttu-id="23a07-144">V sadě Visual Studio, přidejte Visual C# Windows klasický desktopový projekt toohello aktuální řešení, pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="23a07-144">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution, by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="23a07-145">Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="23a07-145">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="23a07-146">Název projektu hello **SimulatedDevice**.</span><span class="sxs-lookup"><span data-stu-id="23a07-146">Name hello project **SimulatedDevice**.</span></span>

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10b]

2. <span data-ttu-id="23a07-148">V Průzkumníku řešení klikněte pravým tlačítkem na hello **SimulatedDevice** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="23a07-148">In Solution Explorer, right-click hello **SimulatedDevice** project, and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="23a07-149">V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **Microsoft.Azure.Devices.Client**, vyberte **nainstalovat** tooinstall hello **Microsoft.Azure.Devices.Client** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="23a07-149">In hello **NuGet Package Manager** window, select **Browse**, search for **Microsoft.Azure.Devices.Client**, select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="23a07-150">Tento postup stáhne, nainstaluje a přidá odkaz toohello [balíček NuGet sady SDK pro zařízení Azure IoT] [ lnk-device-nuget] a jeho závislosti.</span><span class="sxs-lookup"><span data-stu-id="23a07-150">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK NuGet package][lnk-device-nuget] and its dependencies.</span></span>

4. <span data-ttu-id="23a07-151">Přidejte následující hello `using` příkaz hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="23a07-151">Add hello following `using` statement at hello top of hello **Program.cs** file:</span></span>

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. <span data-ttu-id="23a07-152">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="23a07-152">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="23a07-153">SUBSTITUTE `{iot hub hostname}` hello IoT hub názvem hostitele, který jste získali v části "Vytvoření služby IoT hub" hello.</span><span class="sxs-lookup"><span data-stu-id="23a07-153">Substitute `{iot hub hostname}` with hello IoT hub host name you retrieved in hello "Create an IoT hub" section.</span></span> <span data-ttu-id="23a07-154">SUBSTITUTE `{device key}` klíčem hello zařízení, který jste získali v části "Vytvoření identity zařízení" hello.</span><span class="sxs-lookup"><span data-stu-id="23a07-154">Substitute `{device key}` with hello device key you retrieved in hello "Create a device identity" section.</span></span>

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. <span data-ttu-id="23a07-155">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="23a07-155">Add hello following method toohello **Program** class:</span></span>

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

    <span data-ttu-id="23a07-156">Tato metoda odesílá novou zprávu typu zařízení-cloud každou sekundu.</span><span class="sxs-lookup"><span data-stu-id="23a07-156">This method sends a new device-to-cloud message every second.</span></span> <span data-ttu-id="23a07-157">Hello zpráva obsahuje objekt serializovaný JSON, s ID zařízení hello a náhodně vygenerované čísla toosimulate senzor teploty a vlhkosti senzoru.</span><span class="sxs-lookup"><span data-stu-id="23a07-157">hello message contains a JSON-serialized object, with hello device ID and randomly generated numbers toosimulate a temperature sensor, and a humidity sensor.</span></span>

7. <span data-ttu-id="23a07-158">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="23a07-158">Finally, add hello following lines toohello **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);

    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    <span data-ttu-id="23a07-159">Ve výchozím nastavení, hello **vytvořit** metoda v rozhraní .NET Framework aplikace vytvoří **DeviceClient** instanci, která používá toocommunicate protokol AMQP hello službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="23a07-159">By default, hello **Create** method in a .NET Framework app creates a **DeviceClient** instance that uses hello AMQP protocol toocommunicate with IoT Hub.</span></span> <span data-ttu-id="23a07-160">hello toouse protokol MQTT nebo HTTP, používat hello přepsání hello **vytvořit** metoda, která vám umožní toospecify hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="23a07-160">toouse hello MQTT or HTTP protocol, use hello override of hello **Create** method that enables you toospecify hello protocol.</span></span> <span data-ttu-id="23a07-161">UWP a PCL klienti používat protokol HTTP hello ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="23a07-161">UWP and PCL clients use hello HTTP protocol by default.</span></span> <span data-ttu-id="23a07-162">Pokud používáte protokol hello HTTP, měli byste také přidat hello **Microsoft.AspNet.WebApi.Client** NuGet balíček tooyour projektu tooinclude hello **System.Net.Http.Formatting** oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="23a07-162">If you use hello HTTP protocol, you should also add hello **Microsoft.AspNet.WebApi.Client** NuGet package tooyour project tooinclude hello **System.Net.Http.Formatting** namespace.</span></span>

<span data-ttu-id="23a07-163">Tento kurz vás provede kroky toocreate hello služby IoT Hub zařízení aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23a07-163">This tutorial takes you through hello steps toocreate an IoT Hub device app.</span></span> <span data-ttu-id="23a07-164">Můžete taky hello [připojená služba pro službu Azure IoT Hub] [ lnk-connected-service] tooadd rozšíření sady Visual Studio, hello aplikaci zařízení tooyour nezbytného kódu.</span><span class="sxs-lookup"><span data-stu-id="23a07-164">You can also use hello [Connected Service for Azure IoT Hub][lnk-connected-service] Visual Studio extension tooadd hello necessary code tooyour device app.</span></span>

> [!NOTE]
> <span data-ttu-id="23a07-165">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="23a07-165">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="23a07-166">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="23a07-166">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="23a07-167">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="23a07-167">Run hello apps</span></span>

<span data-ttu-id="23a07-168">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="23a07-168">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="23a07-169">V sadě Visual Studio v Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom klikněte na tlačítko **Nastavit projekty po spuštění**.</span><span class="sxs-lookup"><span data-stu-id="23a07-169">In Visual Studio, in Solution Explorer, right-click your solution, and then click **Set StartUp projects**.</span></span> <span data-ttu-id="23a07-170">Vyberte **více projektů po spuštění**a potom vyberte **spustit** jako hello akci pro oba hello **ReadDeviceToCloudMessages** a **SimulatedDevice** projekty.</span><span class="sxs-lookup"><span data-stu-id="23a07-170">Select **Multiple startup projects**, and then select **Start** as hello action for both hello **ReadDeviceToCloudMessages** and **SimulatedDevice** projects.</span></span>

    ![Vlastnosti projektu po spuštění][41]

2. <span data-ttu-id="23a07-172">Stiskněte klávesu **F5** toostart obě aplikace.</span><span class="sxs-lookup"><span data-stu-id="23a07-172">Press **F5** toostart both apps running.</span></span> <span data-ttu-id="23a07-173">výstup konzoly z hello Hello **SimulatedDevice** zprávy hello aplikace zobrazí vaše aplikace zařízení odesílá tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="23a07-173">hello console output from hello **SimulatedDevice** app shows hello messages your device app sends tooyour IoT hub.</span></span> <span data-ttu-id="23a07-174">výstup konzoly z hello Hello **ReadDeviceToCloudMessages** aplikace zobrazí hello zprávy, které IoT hub přijímá.</span><span class="sxs-lookup"><span data-stu-id="23a07-174">hello console output from hello **ReadDeviceToCloudMessages** app shows hello messages that your IoT hub receives.</span></span>

    ![Výstup konzoly z aplikací][42]

3. <span data-ttu-id="23a07-176">Hello **využití** dlaždici v hello [portál Azure] [ lnk-portal] ukazuje hello počet zpráv odeslaných toohello služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="23a07-176">hello **Usage** tile in hello [Azure portal][lnk-portal] shows hello number of messages sent toohello IoT hub:</span></span>

    ![Dlaždice Využití na portálu Azure Portal][43]

## <a name="next-steps"></a><span data-ttu-id="23a07-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23a07-178">Next steps</span></span>

<span data-ttu-id="23a07-179">V tomto kurzu jste nakonfigurovali služby IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="23a07-179">In this tutorial, you configured an IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="23a07-180">Použili jste toto zařízení identity tooenable hello zařízení aplikaci toosend zpráv typu zařízení cloud toohello Centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="23a07-180">You used this device identity tooenable hello device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="23a07-181">Můžete také vytvořit aplikaci, která zobrazuje hello zprávy přijaté službou hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="23a07-181">You also created an app that displays hello messages received by hello IoT hub.</span></span>

<span data-ttu-id="23a07-182">toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:</span><span class="sxs-lookup"><span data-stu-id="23a07-182">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="23a07-183">[Připojení zařízení][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="23a07-183">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="23a07-184">[Začínáme se správou zařízení][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="23a07-184">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="23a07-185">[Začínáme se službou IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="23a07-185">[Getting started with IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="23a07-186">toolearn jak tooextend zpráv IoT řešení a proces zařízení cloud ve velkém měřítku, najdete v části hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="23a07-186">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>

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
