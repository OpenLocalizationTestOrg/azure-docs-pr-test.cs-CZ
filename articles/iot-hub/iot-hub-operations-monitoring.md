---
title: "Operace v Azure IoT Hub monitorování | Microsoft Docs"
description: "Jak používat Azure IoT Hub operations monitorování ke sledování stavu operací ve službě IoT hub v reálném čase."
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
ms.openlocfilehash: b6de5c5df5f9401a41be152bfa06eb994594e83d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="iot-hub-operations-monitoring"></a><span data-ttu-id="06101-103">Operace služby IoT Hub monitorování</span><span class="sxs-lookup"><span data-stu-id="06101-103">IoT Hub operations monitoring</span></span>

<span data-ttu-id="06101-104">Operace služby IoT Hub monitorování umožňuje sledovat stav operace ve službě IoT hub v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="06101-104">IoT Hub operations monitoring enables you to monitor the status of operations on your IoT hub in real time.</span></span> <span data-ttu-id="06101-105">IoT Hub sleduje události napříč několika kategorií operací.</span><span class="sxs-lookup"><span data-stu-id="06101-105">IoT Hub tracks events across several categories of operations.</span></span> <span data-ttu-id="06101-106">Můžete se taky rozhodnout do odesílání událostí z jedné nebo více kategorií pro koncový bod služby IoT hub pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="06101-106">You can opt into sending events from one or more categories to an endpoint of your IoT hub for processing.</span></span> <span data-ttu-id="06101-107">Můžete sledovat data chyby nebo nastavit složitější zpracování vámi data.</span><span class="sxs-lookup"><span data-stu-id="06101-107">You can monitor the data for errors or set up more complex processing based on data patterns.</span></span>

<span data-ttu-id="06101-108">IoT Hub monitoruje šesti kategorie události:</span><span class="sxs-lookup"><span data-stu-id="06101-108">IoT Hub monitors six categories of events:</span></span>

* <span data-ttu-id="06101-109">Operace identity zařízení</span><span class="sxs-lookup"><span data-stu-id="06101-109">Device identity operations</span></span>
* <span data-ttu-id="06101-110">Zařízení telemetrie</span><span class="sxs-lookup"><span data-stu-id="06101-110">Device telemetry</span></span>
* <span data-ttu-id="06101-111">Zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="06101-111">Cloud-to-device messages</span></span>
* <span data-ttu-id="06101-112">Připojení</span><span class="sxs-lookup"><span data-stu-id="06101-112">Connections</span></span>
* <span data-ttu-id="06101-113">Nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="06101-113">File uploads</span></span>
* <span data-ttu-id="06101-114">Směrování zpráv</span><span class="sxs-lookup"><span data-stu-id="06101-114">Message routing</span></span>

## <a name="how-to-enable-operations-monitoring"></a><span data-ttu-id="06101-115">Postup povolení operací monitorování</span><span class="sxs-lookup"><span data-stu-id="06101-115">How to enable operations monitoring</span></span>

1. <span data-ttu-id="06101-116">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="06101-116">Create an IoT hub.</span></span> <span data-ttu-id="06101-117">Pokyny k vytvoření služby IoT hub v naleznete [Začínáme] [ lnk-get-started] průvodce.</span><span class="sxs-lookup"><span data-stu-id="06101-117">You can find instructions on how to create an IoT hub in the [Get Started][lnk-get-started] guide.</span></span>

1. <span data-ttu-id="06101-118">Otevřete okno služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="06101-118">Open the blade of your IoT hub.</span></span> <span data-ttu-id="06101-119">Zde, klikněte na tlačítko **Operations monitorování**.</span><span class="sxs-lookup"><span data-stu-id="06101-119">From there, click **Operations monitoring**.</span></span>

    ![Monitorování konfigurace na portálu pro operace přístupu.][1]

1. <span data-ttu-id="06101-121">Vyberte monitorování kategorie, které chcete monitorovat a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="06101-121">Select the monitoring categories you wish to monitor, and then click **Save**.</span></span> <span data-ttu-id="06101-122">Události jsou k dispozici pro čtení z koncového bodu kompatibilního s centrem událostí uvedený v **nastavení sledování**.</span><span class="sxs-lookup"><span data-stu-id="06101-122">The events are available for reading from the Event Hub-compatible endpoint listed in **Monitoring settings**.</span></span> <span data-ttu-id="06101-123">Koncový bod centra IoT se nazývá `messages/operationsmonitoringevents`.</span><span class="sxs-lookup"><span data-stu-id="06101-123">The IoT Hub endpoint is called `messages/operationsmonitoringevents`.</span></span>

    ![Konfigurace operací monitorování ve službě IoT hub][2]

> [!NOTE]
> <span data-ttu-id="06101-125">Výběr **podrobné** monitorování **připojení** kategorie způsobí, že Centrum IoT k vygenerování hlášení další diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="06101-125">Selecting **Verbose** monitoring for the **Connections** category causes IoT Hub to generate additional diagnostics messages.</span></span> <span data-ttu-id="06101-126">U všech ostatních kategorií **podrobné** změny nastavení množství informací IoT Hub obsahuje v každém chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="06101-126">For all other categories, the **Verbose** setting changes the quantity of information IoT Hub includes in each error message.</span></span>

## <a name="event-categories-and-how-to-use-them"></a><span data-ttu-id="06101-127">Kategorie události a jejich použití</span><span class="sxs-lookup"><span data-stu-id="06101-127">Event categories and how to use them</span></span>

<span data-ttu-id="06101-128">Každý operations monitorování sleduje kategorie má jiný typ interakci s IoT Hub a každou kategorii monitorování schéma, které definuje, jak jsou strukturovaná události v této kategorii.</span><span class="sxs-lookup"><span data-stu-id="06101-128">Each operations monitoring category tracks a different type of interaction with IoT Hub, and each monitoring category has a schema that defines how events in that category are structured.</span></span>

### <a name="device-identity-operations"></a><span data-ttu-id="06101-129">Operace identity zařízení</span><span class="sxs-lookup"><span data-stu-id="06101-129">Device identity operations</span></span>

<span data-ttu-id="06101-130">Kategorie operations identity zařízení sleduje chyby, ke kterým dochází při pokusu o vytvoření, aktualizace nebo odstranění položku v registru identit služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="06101-130">The device identity operations category tracks errors that occur when you attempt to create, update, or delete an entry in your IoT hub's identity registry.</span></span> <span data-ttu-id="06101-131">Sledování této kategorie jsou užitečné pro zřizování scénáře.</span><span class="sxs-lookup"><span data-stu-id="06101-131">Tracking this category is useful for provisioning scenarios.</span></span>

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

### <a name="device-telemetry"></a><span data-ttu-id="06101-132">Zařízení telemetrie</span><span class="sxs-lookup"><span data-stu-id="06101-132">Device telemetry</span></span>

<span data-ttu-id="06101-133">Kategorie zařízení telemetrie sleduje chybách, ke kterým dochází při služby IoT hub a souvisí s telemetrie kanálu.</span><span class="sxs-lookup"><span data-stu-id="06101-133">The device telemetry category tracks errors that occur at the IoT hub and are related to the telemetry pipeline.</span></span> <span data-ttu-id="06101-134">Tato kategorie zahrnuje chyb vzniklých při odesílání telemetrie událostí (například omezení šířky pásma) a příjem telemetrické události (například neoprávněným čtečky).</span><span class="sxs-lookup"><span data-stu-id="06101-134">This category includes errors that occur when sending telemetry events (such as throttling) and receiving telemetry events (such as unauthorized reader).</span></span> <span data-ttu-id="06101-135">Tuto kategorii nelze catch chyby způsobené kód spuštěný v samotném zařízení.</span><span class="sxs-lookup"><span data-stu-id="06101-135">This category cannot catch errors caused by code running on the device itself.</span></span>

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

### <a name="cloud-to-device-commands"></a><span data-ttu-id="06101-136">Příkazy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="06101-136">Cloud-to-device commands</span></span>

<span data-ttu-id="06101-137">Kategorie příkazy typu cloud zařízení sleduje chybách, ke kterým dochází při služby IoT hub a souvisí s kanálu zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="06101-137">The cloud-to-device commands category tracks errors that occur at the IoT hub and are related to the cloud-to-device message pipeline.</span></span> <span data-ttu-id="06101-138">Tato kategorie zahrnuje chyb vzniklých při odesílání zpráv typu cloud zařízení (například neautorizovaného odesílatele), přijímání zpráv typu cloud zařízení (například počet doručení překročena) a přijímání zpráv typu cloud zařízení zpětnou vazbu (například zpětné vazby jeho platnost).</span><span class="sxs-lookup"><span data-stu-id="06101-138">This category includes errors that occur when sending cloud-to-device messages (such as unauthorized sender), receiving cloud-to-device messages (such as delivery count exceeded), and receiving cloud-to-device message feedback (such as feedback expired).</span></span> <span data-ttu-id="06101-139">Tuto kategorii nezachytí chyby ze zařízení, která nesprávně zpracovává zprávy typu cloud zařízení, pokud zpráva cloud zařízení byla úspěšně doručeno.</span><span class="sxs-lookup"><span data-stu-id="06101-139">This category does not catch errors from a device that improperly handles a cloud-to-device message if the cloud-to-device message was delivered successfully.</span></span>

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

### <a name="connections"></a><span data-ttu-id="06101-140">Připojení</span><span class="sxs-lookup"><span data-stu-id="06101-140">Connections</span></span>

<span data-ttu-id="06101-141">Kategorie připojení sleduje chyb vzniklých při zařízení připojení nebo odpojení od služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="06101-141">The connections category tracks errors that occur when devices connect or disconnect from an IoT hub.</span></span> <span data-ttu-id="06101-142">Sledování této kategorie jsou užitečné pro identifikaci pokusy o neoprávněné připojení a sledování při ztrátě pro zařízení v oblastech malé možnosti připojení připojení.</span><span class="sxs-lookup"><span data-stu-id="06101-142">Tracking this category is useful for identifying unauthorized connection attempts and for tracking when a connection is lost for devices in areas of poor connectivity.</span></span>

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

### <a name="file-uploads"></a><span data-ttu-id="06101-143">Nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="06101-143">File uploads</span></span>

<span data-ttu-id="06101-144">Kategorie nahrávání souboru sleduje chybách, ke kterým dochází při služby IoT hub a souvisí s funkcí nahrávání souboru.</span><span class="sxs-lookup"><span data-stu-id="06101-144">The file upload category tracks errors that occur at the IoT hub and are related to file upload functionality.</span></span> <span data-ttu-id="06101-145">Tato kategorie zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="06101-145">This category includes:</span></span>

* <span data-ttu-id="06101-146">Chyby, které se identifikátor URI SAS, jako je například vypršení platnosti před zařízení upozorní rozbočovače dokončená nahrávání.</span><span class="sxs-lookup"><span data-stu-id="06101-146">Errors that occur with the SAS URI, such as when it expires before a device notifies the hub of a completed upload.</span></span>
* <span data-ttu-id="06101-147">Nepodařilo nahrávání údajů ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="06101-147">Failed uploads reported by the device.</span></span>
* <span data-ttu-id="06101-148">Chyby, které nastat, pokud soubor nebyl nalezen v úložišti během vytváření zpráv oznámení služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="06101-148">Errors that occur when a file is not found in storage during IoT Hub notification message creation.</span></span>

<span data-ttu-id="06101-149">Tuto kategorii nelze catch chyb, které přímo nastat, když je zařízení nahrání souboru do úložiště.</span><span class="sxs-lookup"><span data-stu-id="06101-149">This category cannot catch errors that directly occur while the device is uploading a file to storage.</span></span>

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

### <a name="message-routing"></a><span data-ttu-id="06101-150">Směrování zpráv</span><span class="sxs-lookup"><span data-stu-id="06101-150">Message routing</span></span>

<span data-ttu-id="06101-151">Kategorie směrování zpráv sleduje chyb vzniklých při vyhodnocení zpráva trasy a stav koncového bodu zaznamenatelného službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="06101-151">The message routing category tracks errors that occur during message route evaluation and endpoint health as perceived by IoT Hub.</span></span> <span data-ttu-id="06101-152">Tato kategorie zahrnuje události, například pokud se pravidlo vyhodnotí jako "undefined", když IoT Hub označí koncový bod zpráv a všechny ostatní chyby přijaté z koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="06101-152">This category includes events such as when a rule evaluates to "undefined", when IoT Hub marks an endpoint as dead, and any other errors received from an endpoint.</span></span> <span data-ttu-id="06101-153">Tato kategorie nezahrnuje konkrétní chyby týkající se zpráv sami (například zařízení omezení chyby), které jsou v části kategorie "zařízení telemetrická".</span><span class="sxs-lookup"><span data-stu-id="06101-153">This category does not include specific errors about the messages themselves (such as device throttling errors), which are reported under the "device telemetry" category.</span></span>

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

## <a name="view-events"></a><span data-ttu-id="06101-154">zobrazení událostí</span><span class="sxs-lookup"><span data-stu-id="06101-154">View events</span></span>

<span data-ttu-id="06101-155">Můžete použít *iothub-explorer* nástroj rychle otestovat, zda služby IoT hub je generování události monitorování.</span><span class="sxs-lookup"><span data-stu-id="06101-155">You can use the *iothub-explorer* tool to quickly test that your IoT hub is generating monitoring events.</span></span> <span data-ttu-id="06101-156">Chcete-li nainstalovat nástroj, postupujte podle pokynů v [iothub-explorer] [ lnk-iothub-explorer] úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="06101-156">To install the tool, see the instructions in the [iothub-explorer][lnk-iothub-explorer] GitHub repository.</span></span>

1. <span data-ttu-id="06101-157">Zajistěte, aby **připojení** monitorování kategorie je nastaven na **podrobné** na portálu.</span><span class="sxs-lookup"><span data-stu-id="06101-157">Make sure the **Connections** monitoring category is set to **Verbose** in the portal.</span></span>

1. <span data-ttu-id="06101-158">V příkazovém řádku spusťte následující příkaz pro čtení z koncového bodu monitorování:</span><span class="sxs-lookup"><span data-stu-id="06101-158">At a command-prompt, run the following command to read from the monitoring endpoint:</span></span>

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="06101-159">V jiné příkazového řádku spusťte následující příkaz k simulaci zařízení odesílání zpráv typu zařízení cloud:</span><span class="sxs-lookup"><span data-stu-id="06101-159">In another command-prompt, run the following command to simulate a device sending device-to-cloud messages:</span></span>

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="06101-160">První příkazového řádku ukazuje události monitorování simulované zařízení připojí ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="06101-160">The first command-prompt shows the monitoring events as the simulated device connects to your IoT hub.</span></span>

## <a name="connect-to-the-monitoring-endpoint"></a><span data-ttu-id="06101-161">Připojení ke koncovému bodu monitorování</span><span class="sxs-lookup"><span data-stu-id="06101-161">Connect to the monitoring endpoint</span></span>

<span data-ttu-id="06101-162">Monitorování koncový bod ve službě IoT hub je koncový bod kompatibilní s centrem událostí.</span><span class="sxs-lookup"><span data-stu-id="06101-162">The monitoring endpoint on your IoT hub is an Event Hub-compatible endpoint.</span></span> <span data-ttu-id="06101-163">Můžete použít všechny mechanismus, který funguje se službou Event Hubs umožní číst zprávy typu monitorování z tento koncový bod.</span><span class="sxs-lookup"><span data-stu-id="06101-163">You can use any mechanism that works with Event Hubs to read monitoring messages from this endpoint.</span></span> <span data-ttu-id="06101-164">Následující příklad vytvoří základní čtečka, která není vhodná pro vysoce výkonná nasazení.</span><span class="sxs-lookup"><span data-stu-id="06101-164">The following sample creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="06101-165">Další informace o zpracování zpráv ze služby Event Hubs najdete v kurzu [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial].</span><span class="sxs-lookup"><span data-stu-id="06101-165">For more information about how to process messages from Event Hubs, see the [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial.</span></span>

<span data-ttu-id="06101-166">Pro připojení k monitorování koncového bodu, budete potřebovat připojovací řetězec a název koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="06101-166">To connect to the monitoring endpoint, you need a connection string and the endpoint name.</span></span> <span data-ttu-id="06101-167">Následující kroky vám ukážou, jak najít potřebné hodnoty na portálu:</span><span class="sxs-lookup"><span data-stu-id="06101-167">The following steps show you how to find the necessary values in the portal:</span></span>

1. <span data-ttu-id="06101-168">Na portálu přejděte do okna prostředků vaší služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="06101-168">In the portal, navigate to your IoT Hub resource blade.</span></span>

1. <span data-ttu-id="06101-169">Zvolte **Operations monitorování**a poznamenejte si **název kompatibilní s centrem událostí** a **koncový bod kompatibilní s centrem událostí** hodnoty:</span><span class="sxs-lookup"><span data-stu-id="06101-169">Choose **Operations monitoring**, and make a note of the **Event Hub-compatible name** and **Event Hub-compatible endpoint** values:</span></span>

    ![Hodnoty koncový bod kompatibilní s centrem událostí][img-endpoints]

1. <span data-ttu-id="06101-171">Zvolte **zásady sdíleného přístupu**, zvolte **služby**.</span><span class="sxs-lookup"><span data-stu-id="06101-171">Choose **Shared access policies**, then choose **service**.</span></span> <span data-ttu-id="06101-172">Poznamenejte si **primární klíč** hodnotu:</span><span class="sxs-lookup"><span data-stu-id="06101-172">Make a note of the **Primary key** value:</span></span>

    ![Primární klíč pro zásady sdíleného přístupu služby][img-service-key]

<span data-ttu-id="06101-174">Následující ukázka kódu C# jsou převzaty ze sady Visual Studio **Windows Classic Desktop** konzolovou aplikaci C#.</span><span class="sxs-lookup"><span data-stu-id="06101-174">The following C# code sample is taken from a Visual Studio **Windows Classic Desktop** C# console app.</span></span> <span data-ttu-id="06101-175">Tento projekt **WindowsAzure.ServiceBus** nainstalovat balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="06101-175">The project has the **WindowsAzure.ServiceBus** NuGet package installed.</span></span>

* <span data-ttu-id="06101-176">Nahraďte zástupný symbol připojovacího řetězce připojovací řetězec, který používá **koncový bod kompatibilní s centrem událostí** a služba **primární klíč** hodnoty, které jste si poznamenali dříve, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="06101-176">Replace the connection string placeholder with a connection string that uses the **Event Hub-compatible endpoint** and service **Primary key** values you noted previously as shown in the following example:</span></span>

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* <span data-ttu-id="06101-177">Nahraďte monitorování zástupný symbol název koncového bodu se **název kompatibilní s centrem událostí** hodnoty, které jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="06101-177">Replace the monitoring endpoint name placeholder with the **Event Hub-compatible name** value you noted previously.</span></span>

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");

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

## <a name="next-steps"></a><span data-ttu-id="06101-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="06101-178">Next steps</span></span>
<span data-ttu-id="06101-179">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="06101-179">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="06101-180">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="06101-180">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="06101-181">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="06101-181">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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