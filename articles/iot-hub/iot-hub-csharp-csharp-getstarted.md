---
title: "Začínáme se službou Azure IoT Hub (.NET) | Dokumentace Microsoftu"
description: "Zjistěte, jak odesílat zprávy typu zařízení-cloud do služby Azure IoT Hub pomocí sad IoT SDK pro .NET. Vytvořte simulované zařízení a aplikace služeb pro registraci vašeho zařízení, odesílání zpráv a čtení zpráv ze služby IoT Hub."
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
ms.openlocfilehash: 69296eb9ac2a74a97b632d27733a6a06500b4abd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="connect-your-device-to-your-iot-hub-using-net"></a><span data-ttu-id="d5f2d-104">Připojení zařízení ke službě IoT Hub pomocí .NET</span><span class="sxs-lookup"><span data-stu-id="d5f2d-104">Connect your device to your IoT hub using .NET</span></span>

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="d5f2d-105">Na konci tohoto kurzu budete mít tři konzolové aplikace .NET:</span><span class="sxs-lookup"><span data-stu-id="d5f2d-105">At the end of this tutorial, you have three .NET console apps:</span></span>

* <span data-ttu-id="d5f2d-106">**CreateDeviceIdentity** vytváří identitu zařízení a přidružený klíč zabezpečení k připojení aplikace pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-106">**CreateDeviceIdentity**, which creates a device identity and associated security key to connect your device app.</span></span>
* <span data-ttu-id="d5f2d-107">**ReadDeviceToCloudMessages** zobrazuje telemetrická data odesílaná aplikací pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-107">**ReadDeviceToCloudMessages**, which displays the telemetry sent by your device app.</span></span>
* <span data-ttu-id="d5f2d-108">**SimulatedDevice** propojuje službu IoT Hub s dříve vytvořenou identitou zařízení a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu MQTT.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-108">**SimulatedDevice**, which connects to your IoT hub with the device identity created earlier, and sends a telemetry message every second by using the MQTT protocol.</span></span>

<span data-ttu-id="d5f2d-109">Řešení Visual Studio, které obsahuje tyto tři aplikace z Githubu, si můžete stáhnout nebo naklonovat.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-109">You can download or clone the Visual Studio solution, which contains the three apps from Github.</span></span>

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> <span data-ttu-id="d5f2d-110">Informace o sadách SDK služby Azure IoT Hub, s jejichž pomocí můžete sestavit aplikace, které poběží v zařízení, i back-end vašeho řešení, najdete v tématu [Sady SDK služby IoT Hub][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="d5f2d-110">For information about the Azure IoT SDKs that you can use to build both applications to run on devices, and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="d5f2d-111">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="d5f2d-111">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="d5f2d-112">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-112">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="d5f2d-113">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-113">An active Azure account.</span></span> <span data-ttu-id="d5f2d-114">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="d5f2d-114">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="d5f2d-115">Nyní jste vytvořili službu IoT Hub a máte název hostitele a připojovací řetězec služby IoT Hub, které potřebujete k dokončení kurzu.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-115">You have now created your IoT hub, and you have the host name and IoT Hub connection string that you need to complete the rest of this tutorial.</span></span>

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="d5f2d-116">Příjem zpráv typu zařízení-cloud</span><span class="sxs-lookup"><span data-stu-id="d5f2d-116">Receive device-to-cloud messages</span></span>
<span data-ttu-id="d5f2d-117">V této části vytvoříte konzolovou aplikaci .NET, která čte zprávy typu zařízení-cloud ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-117">In this section, you create a .NET console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="d5f2d-118">Služba IoT Hub zpřístupní koncový bod kompatibilní se službou [Azure Event Hubs][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-118">An IoT hub exposes an [Azure Event Hubs][lnk-event-hubs-overview]-compatible endpoint to enable you to read device-to-cloud messages.</span></span> <span data-ttu-id="d5f2d-119">Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-119">To keep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="d5f2d-120">Další informace o tom, jak zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].</span><span class="sxs-lookup"><span data-stu-id="d5f2d-120">To learn how to process device-to-cloud messages at scale, see the [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span> <span data-ttu-id="d5f2d-121">Další informace o zpracování zpráv ze služby Event Hubs najdete v kurzu [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial].</span><span class="sxs-lookup"><span data-stu-id="d5f2d-121">For more information about how to process messages from Event Hubs, see the [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial.</span></span> <span data-ttu-id="d5f2d-122">(Tento kurz se vztahuje na koncové body kompatibilní se službou IoT Hub Event Hub.)</span><span class="sxs-lookup"><span data-stu-id="d5f2d-122">(This tutorial is applicable to the IoT Hub Event Hub-compatible endpoints.)</span></span>

> [!NOTE]
> <span data-ttu-id="d5f2d-123">Koncový bod kompatibilní s centrem událostí pro čtení zpráv mezi zařízením a cloudem vždy používá protokol AMQP.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-123">The Event Hub-compatible endpoint for reading device-to-cloud messages always uses the AMQP protocol.</span></span>

1. <span data-ttu-id="d5f2d-124">V sadě Visual Studio přidejte ke stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-124">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution, by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="d5f2d-125">Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-125">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="d5f2d-126">Projekt nazvěte **ReadDeviceToCloudMessages**.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-126">Name the project **ReadDeviceToCloudMessages**.</span></span>

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10a]

2. <span data-ttu-id="d5f2d-128">V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **ReadDeviceToCloudMessages** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-128">In Solution Explorer, right-click the **ReadDeviceToCloudMessages** project, and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="d5f2d-129">V okně **Správce balíčků NuGet** vyhledejte **WindowsAzure.ServiceBus**, vyberte možnost **Instalovat** a přijměte podmínky používání.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-129">In the **NuGet Package Manager** window, search for **WindowsAzure.ServiceBus**, select **Install**, and accept the terms of use.</span></span> <span data-ttu-id="d5f2d-130">Tímto postupem se stáhne a nainstaluje služba [Azure Service Bus][lnk-servicebus-nuget] a všechny její závislosti a přidá se na ni odkaz.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-130">This procedure downloads, installs, and adds a reference to [Azure Service Bus][lnk-servicebus-nuget], with all its dependencies.</span></span> <span data-ttu-id="d5f2d-131">Tento balíček umožní aplikaci připojení ke koncovému bodu kompatibilnímu se službou Event Hubs ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-131">This package enables the application to connect to the Event Hub-compatible endpoint on your IoT hub.</span></span>

4. <span data-ttu-id="d5f2d-132">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="d5f2d-132">Add the following `using` statements at the top of the **Program.cs** file:</span></span>

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. <span data-ttu-id="d5f2d-133">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-133">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="d5f2d-134">Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v části Vytvoření služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-134">Replace the placeholder value with the IoT Hub connection string for the hub you created in the "Create an IoT hub" section.</span></span>

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. <span data-ttu-id="d5f2d-135">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="d5f2d-135">Add the following method to the **Program** class:</span></span>

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

    <span data-ttu-id="d5f2d-136">Tato metoda používá instanci **EventHubReceiver** k příjmu zpráv ze všech oddílů pro příjem zpráv typu zařízení-cloud ve službě IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="d5f2d-136">This method uses an **EventHubReceiver** instance to receive messages from all the IoT hub device-to-cloud receive partitions.</span></span> <span data-ttu-id="d5f2d-137">Všimněte si, jak při vytváření objektu **EventHubReceiver** předáte parametr `DateTime.Now`, aby objekt přijímal pouze zprávy odeslané po spuštění.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-137">Notice how you pass a `DateTime.Now` parameter when you create the **EventHubReceiver** object, so that it only receives messages sent after it starts.</span></span> <span data-ttu-id="d5f2d-138">Tento filtr je užitečný v testovacím prostředí, protože uvidíte aktuální sadu zpráv.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-138">This filter is useful in a test environment so you can see the current set of messages.</span></span> <span data-ttu-id="d5f2d-139">V produkčním prostředí byste se měli ujistit, že váš kód zpracovává všechny zprávy.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-139">In a production environment, your code should make sure that it processes all the messages.</span></span> <span data-ttu-id="d5f2d-140">Další informace najdete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial].</span><span class="sxs-lookup"><span data-stu-id="d5f2d-140">For more information, see the tutorial [How to process IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial].</span></span>

7. <span data-ttu-id="d5f2d-141">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="d5f2d-141">Finally, add the following lines to the **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
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

## <a name="create-a-device-app"></a><span data-ttu-id="d5f2d-142">Vytvoření aplikace pro zařízení</span><span class="sxs-lookup"><span data-stu-id="d5f2d-142">Create a device app</span></span>

<span data-ttu-id="d5f2d-143">V této části vytvoříte konzolovou aplikaci .NET, která simuluje zařízení odesílající zprávy typu zařízení-cloud do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-143">In this section, you create a .NET console app that simulates a device that sends device-to-cloud messages to an IoT hub.</span></span>

1. <span data-ttu-id="d5f2d-144">V sadě Visual Studio přidejte ke stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-144">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution, by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="d5f2d-145">Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-145">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="d5f2d-146">Projekt pojmenujte **SimulatedDevice**.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-146">Name the project **SimulatedDevice**.</span></span>

    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10b]

2. <span data-ttu-id="d5f2d-148">V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **SimulatedDevice** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-148">In Solution Explorer, right-click the **SimulatedDevice** project, and then click **Manage NuGet Packages**.</span></span>

3. <span data-ttu-id="d5f2d-149">V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **Microsoft.Azure.Devices.Client**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices.Client** a přijměte podmínky používání.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-149">In the **NuGet Package Manager** window, select **Browse**, search for **Microsoft.Azure.Devices.Client**, select **Install** to install the **Microsoft.Azure.Devices.Client** package, and accept the terms of use.</span></span> <span data-ttu-id="d5f2d-150">Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro zařízení Azure IoT][lnk-device-nuget] a jeho závislosti a přidá se na něj odkaz.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-150">This procedure downloads, installs, and adds a reference to the [Azure IoT device SDK NuGet package][lnk-device-nuget] and its dependencies.</span></span>

4. <span data-ttu-id="d5f2d-151">Do horní části souboru **Program.cs** přidejte následující příkaz `using`:</span><span class="sxs-lookup"><span data-stu-id="d5f2d-151">Add the following `using` statement at the top of the **Program.cs** file:</span></span>

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. <span data-ttu-id="d5f2d-152">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-152">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="d5f2d-153">Nahraďte `{iot hub hostname}` názvem hostitel centra IoT, který jste získali v části Vytvoření centra IoT.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-153">Substitute `{iot hub hostname}` with the IoT hub host name you retrieved in the "Create an IoT hub" section.</span></span> <span data-ttu-id="d5f2d-154">Nahraďte `{device key}` klíčem zařízení, který jste získali v části Vytvoření identity zařízení.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-154">Substitute `{device key}` with the device key you retrieved in the "Create a device identity" section.</span></span>

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. <span data-ttu-id="d5f2d-155">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="d5f2d-155">Add the following method to the **Program** class:</span></span>

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

    <span data-ttu-id="d5f2d-156">Tato metoda odesílá novou zprávu typu zařízení-cloud každou sekundu.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-156">This method sends a new device-to-cloud message every second.</span></span> <span data-ttu-id="d5f2d-157">Zpráva obsahuje objekt serializovaný do formátu JSON, s ID zařízení a náhodně generovanými čísly, který simuluje snímač teploty a snímač vlhkosti.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-157">The message contains a JSON-serialized object, with the device ID and randomly generated numbers to simulate a temperature sensor, and a humidity sensor.</span></span>

7. <span data-ttu-id="d5f2d-158">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="d5f2d-158">Finally, add the following lines to the **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);

    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    <span data-ttu-id="d5f2d-159">Ve výchozím nastavení metoda **Create** v aplikaci .NET Framework vytvoří instanci **DeviceClient**, která se službou IoT Hub komunikuje pomocí protokolu AMQP.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-159">By default, the **Create** method in a .NET Framework app creates a **DeviceClient** instance that uses the AMQP protocol to communicate with IoT Hub.</span></span> <span data-ttu-id="d5f2d-160">Pokud chcete používat protokol MQTT nebo HTTP, použijte přepis metody **Create**, který umožňuje určit protokol.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-160">To use the MQTT or HTTP protocol, use the override of the **Create** method that enables you to specify the protocol.</span></span> <span data-ttu-id="d5f2d-161">Klienti UPW a PCL ve výchozím nastavení používají protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-161">UWP and PCL clients use the HTTP protocol by default.</span></span> <span data-ttu-id="d5f2d-162">Pokud používáte protokol HTTP, měli byste do svého projektu přidat také balíček NuGet **Microsoft.AspNet.WebApi.Client**, aby projekt zahrnoval obor názvů **System.Net.Http.Formatting**.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-162">If you use the HTTP protocol, you should also add the **Microsoft.AspNet.WebApi.Client** NuGet package to your project to include the **System.Net.Http.Formatting** namespace.</span></span>

<span data-ttu-id="d5f2d-163">Tento kurz vás provede postupem vytvoření aplikace pro zařízení služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-163">This tutorial takes you through the steps to create an IoT Hub device app.</span></span> <span data-ttu-id="d5f2d-164">K přidání nezbytného kódu do aplikace zařízení můžete také použít rozšíření [Připojená služba pro službu Azure IoT Hub][lnk-connected-service] sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-164">You can also use the [Connected Service for Azure IoT Hub][lnk-connected-service] Visual Studio extension to add the necessary code to your device app.</span></span>

> [!NOTE]
> <span data-ttu-id="d5f2d-165">Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-165">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="d5f2d-166">V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-166">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="d5f2d-167">Spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="d5f2d-167">Run the apps</span></span>

<span data-ttu-id="d5f2d-168">Nyní jste připraveni aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-168">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="d5f2d-169">V sadě Visual Studio v Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom klikněte na tlačítko **Nastavit projekty po spuštění**.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-169">In Visual Studio, in Solution Explorer, right-click your solution, and then click **Set StartUp projects**.</span></span> <span data-ttu-id="d5f2d-170">Vyberte možnost **Více projektů po spuštění** a poté příkaz **Spustit** jako akci pro oba projekty **ReadDeviceToCloudMessages** a **SimulatedDevice**.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-170">Select **Multiple startup projects**, and then select **Start** as the action for both the **ReadDeviceToCloudMessages** and **SimulatedDevice** projects.</span></span>

    ![Vlastnosti projektu po spuštění][41]

2. <span data-ttu-id="d5f2d-172">Stisknutím klávesy **F5** spusťte obě aplikace.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-172">Press **F5** to start both apps running.</span></span> <span data-ttu-id="d5f2d-173">Výstup konzoly z aplikace **SimulatedDevice** zobrazuje zprávy, které aplikace pro zařízení odesílá do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-173">The console output from the **SimulatedDevice** app shows the messages your device app sends to your IoT hub.</span></span> <span data-ttu-id="d5f2d-174">Výstup konzoly z aplikace **ReadDeviceToCloudMessages** zobrazuje zprávy, které služba IoT Hub přijímá.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-174">The console output from the **ReadDeviceToCloudMessages** app shows the messages that your IoT hub receives.</span></span>

    ![Výstup konzoly z aplikací][42]

3. <span data-ttu-id="d5f2d-176">Na dlaždici **Využití** na webu [Azure Portal][lnk-portal] se zobrazuje počet zpráv odeslaných do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="d5f2d-176">The **Usage** tile in the [Azure portal][lnk-portal] shows the number of messages sent to the IoT hub:</span></span>

    ![Dlaždice Využití na portálu Azure Portal][43]

## <a name="next-steps"></a><span data-ttu-id="d5f2d-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5f2d-178">Next steps</span></span>

<span data-ttu-id="d5f2d-179">V tomto kurzu jste na webu Azure Portal nakonfigurovali službu IoT Hub a pak jste v registru identit této služby vytvořili identitu zařízení.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-179">In this tutorial, you configured an IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="d5f2d-180">Pomocí identity zařízení jste aplikaci pro zařízení povolili odesílání zpráv typu zařízení-cloud do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-180">You used this device identity to enable the device app to send device-to-cloud messages to the IoT hub.</span></span> <span data-ttu-id="d5f2d-181">Také jste vytvořili aplikaci, která zobrazuje zprávy přijaté službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d5f2d-181">You also created an app that displays the messages received by the IoT hub.</span></span>

<span data-ttu-id="d5f2d-182">Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:</span><span class="sxs-lookup"><span data-stu-id="d5f2d-182">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="d5f2d-183">[Připojení zařízení][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="d5f2d-183">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="d5f2d-184">[Začínáme se správou zařízení][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="d5f2d-184">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="d5f2d-185">[Začínáme se službou IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="d5f2d-185">[Getting started with IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="d5f2d-186">Další informace o tom, jak rozšířit vaše řešení internetu věcí a zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].</span><span class="sxs-lookup"><span data-stu-id="d5f2d-186">To learn how to extend your IoT solution and process device-to-cloud messages at scale, see the [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>

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
