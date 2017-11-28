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
# <a name="iot-hub-operations-monitoring"></a><span data-ttu-id="d8093-103">Operace služby IoT Hub monitorování</span><span class="sxs-lookup"><span data-stu-id="d8093-103">IoT Hub operations monitoring</span></span>

<span data-ttu-id="d8093-104">Operace služby IoT Hub monitorování umožňuje toomonitor hello stav operace ve službě IoT hub v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="d8093-104">IoT Hub operations monitoring enables you toomonitor hello status of operations on your IoT hub in real time.</span></span> <span data-ttu-id="d8093-105">IoT Hub sleduje události napříč několika kategorií operací.</span><span class="sxs-lookup"><span data-stu-id="d8093-105">IoT Hub tracks events across several categories of operations.</span></span> <span data-ttu-id="d8093-106">Můžete se taky rozhodnout do odesílání událostí z jednoho nebo více kategorií tooan koncového bodu služby IoT hub pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="d8093-106">You can opt into sending events from one or more categories tooan endpoint of your IoT hub for processing.</span></span> <span data-ttu-id="d8093-107">Můžete sledovat data hello chyby nebo nastavit složitější zpracování vámi data.</span><span class="sxs-lookup"><span data-stu-id="d8093-107">You can monitor hello data for errors or set up more complex processing based on data patterns.</span></span>

<span data-ttu-id="d8093-108">IoT Hub monitoruje šesti kategorie události:</span><span class="sxs-lookup"><span data-stu-id="d8093-108">IoT Hub monitors six categories of events:</span></span>

* <span data-ttu-id="d8093-109">Operace identity zařízení</span><span class="sxs-lookup"><span data-stu-id="d8093-109">Device identity operations</span></span>
* <span data-ttu-id="d8093-110">Zařízení telemetrie</span><span class="sxs-lookup"><span data-stu-id="d8093-110">Device telemetry</span></span>
* <span data-ttu-id="d8093-111">Zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="d8093-111">Cloud-to-device messages</span></span>
* <span data-ttu-id="d8093-112">Připojení</span><span class="sxs-lookup"><span data-stu-id="d8093-112">Connections</span></span>
* <span data-ttu-id="d8093-113">Nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="d8093-113">File uploads</span></span>
* <span data-ttu-id="d8093-114">Směrování zpráv</span><span class="sxs-lookup"><span data-stu-id="d8093-114">Message routing</span></span>

## <a name="how-tooenable-operations-monitoring"></a><span data-ttu-id="d8093-115">Jak operace tooenable monitorování</span><span class="sxs-lookup"><span data-stu-id="d8093-115">How tooenable operations monitoring</span></span>

1. <span data-ttu-id="d8093-116">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d8093-116">Create an IoT hub.</span></span> <span data-ttu-id="d8093-117">Pokyny naleznete v tom toocreate služby IoT hub v hello [Začínáme] [ lnk-get-started] průvodce.</span><span class="sxs-lookup"><span data-stu-id="d8093-117">You can find instructions on how toocreate an IoT hub in hello [Get Started][lnk-get-started] guide.</span></span>

1. <span data-ttu-id="d8093-118">Otevřete okno hello služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d8093-118">Open hello blade of your IoT hub.</span></span> <span data-ttu-id="d8093-119">Zde, klikněte na tlačítko **Operations monitorování**.</span><span class="sxs-lookup"><span data-stu-id="d8093-119">From there, click **Operations monitoring**.</span></span>

    ![Monitorování konfigurace portálu hello operace přístupu.][1]

1. <span data-ttu-id="d8093-121">Vyberte hello monitorování kategorií chcete toomonitor a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d8093-121">Select hello monitoring categories you wish toomonitor, and then click **Save**.</span></span> <span data-ttu-id="d8093-122">Hello události jsou k dispozici pro čtení z koncového bodu kompatibilního s centrem událostí hello uvedené v **nastavení sledování**.</span><span class="sxs-lookup"><span data-stu-id="d8093-122">hello events are available for reading from hello Event Hub-compatible endpoint listed in **Monitoring settings**.</span></span> <span data-ttu-id="d8093-123">Hello koncový bod centra IoT se nazývá `messages/operationsmonitoringevents`.</span><span class="sxs-lookup"><span data-stu-id="d8093-123">hello IoT Hub endpoint is called `messages/operationsmonitoringevents`.</span></span>

    ![Konfigurace operací monitorování ve službě IoT hub][2]

> [!NOTE]
> <span data-ttu-id="d8093-125">Výběr **podrobné** monitorování hello **připojení** kategorie způsobí, že IoT Hub toogenerate hlášení další diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="d8093-125">Selecting **Verbose** monitoring for hello **Connections** category causes IoT Hub toogenerate additional diagnostics messages.</span></span> <span data-ttu-id="d8093-126">U všech ostatních kategorií hello **podrobné** změny nastavení hello množství informací IoT Hub obsahuje v každém chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="d8093-126">For all other categories, hello **Verbose** setting changes hello quantity of information IoT Hub includes in each error message.</span></span>

## <a name="event-categories-and-how-toouse-them"></a><span data-ttu-id="d8093-127">Kategorie události a jak toouse je</span><span class="sxs-lookup"><span data-stu-id="d8093-127">Event categories and how toouse them</span></span>

<span data-ttu-id="d8093-128">Každý operations monitorování sleduje kategorie má jiný typ interakci s IoT Hub a každou kategorii monitorování schéma, které definuje, jak jsou strukturovaná události v této kategorii.</span><span class="sxs-lookup"><span data-stu-id="d8093-128">Each operations monitoring category tracks a different type of interaction with IoT Hub, and each monitoring category has a schema that defines how events in that category are structured.</span></span>

### <a name="device-identity-operations"></a><span data-ttu-id="d8093-129">Operace identity zařízení</span><span class="sxs-lookup"><span data-stu-id="d8093-129">Device identity operations</span></span>

<span data-ttu-id="d8093-130">kategorie operations identity zařízení Hello sleduje chyby, ke kterým dochází, když se pokusíte toocreate, aktualizovat nebo odstranit položku, která v registru identit služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d8093-130">hello device identity operations category tracks errors that occur when you attempt toocreate, update, or delete an entry in your IoT hub's identity registry.</span></span> <span data-ttu-id="d8093-131">Sledování této kategorie jsou užitečné pro zřizování scénáře.</span><span class="sxs-lookup"><span data-stu-id="d8093-131">Tracking this category is useful for provisioning scenarios.</span></span>

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

### <a name="device-telemetry"></a><span data-ttu-id="d8093-132">Zařízení telemetrie</span><span class="sxs-lookup"><span data-stu-id="d8093-132">Device telemetry</span></span>

<span data-ttu-id="d8093-133">kategorie telemetrie zařízení Hello sleduje chybách, ke kterým dochází při hello IoT hub a jsou související toohello telemetrie kanálu.</span><span class="sxs-lookup"><span data-stu-id="d8093-133">hello device telemetry category tracks errors that occur at hello IoT hub and are related toohello telemetry pipeline.</span></span> <span data-ttu-id="d8093-134">Tato kategorie zahrnuje chyb vzniklých při odesílání telemetrie událostí (například omezení šířky pásma) a příjem telemetrické události (například neoprávněným čtečky).</span><span class="sxs-lookup"><span data-stu-id="d8093-134">This category includes errors that occur when sending telemetry events (such as throttling) and receiving telemetry events (such as unauthorized reader).</span></span> <span data-ttu-id="d8093-135">Tuto kategorii nelze catch chyby způsobené kód spuštěný v samotném hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="d8093-135">This category cannot catch errors caused by code running on hello device itself.</span></span>

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

### <a name="cloud-to-device-commands"></a><span data-ttu-id="d8093-136">Příkazy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="d8093-136">Cloud-to-device commands</span></span>

<span data-ttu-id="d8093-137">kategorie příkazy typu cloud zařízení Hello sleduje chybách, ke kterým dochází při hello IoT hub a jsou kanálu související toohello zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="d8093-137">hello cloud-to-device commands category tracks errors that occur at hello IoT hub and are related toohello cloud-to-device message pipeline.</span></span> <span data-ttu-id="d8093-138">Tato kategorie zahrnuje chyb vzniklých při odesílání zpráv typu cloud zařízení (například neautorizovaného odesílatele), přijímání zpráv typu cloud zařízení (například počet doručení překročena) a přijímání zpráv typu cloud zařízení zpětnou vazbu (například zpětné vazby jeho platnost).</span><span class="sxs-lookup"><span data-stu-id="d8093-138">This category includes errors that occur when sending cloud-to-device messages (such as unauthorized sender), receiving cloud-to-device messages (such as delivery count exceeded), and receiving cloud-to-device message feedback (such as feedback expired).</span></span> <span data-ttu-id="d8093-139">Tuto kategorii nezachytí chyby ze zařízení, která nesprávně zpracovává zprávy typu cloud zařízení, pokud zpráva hello cloud zařízení byla úspěšně doručeno.</span><span class="sxs-lookup"><span data-stu-id="d8093-139">This category does not catch errors from a device that improperly handles a cloud-to-device message if hello cloud-to-device message was delivered successfully.</span></span>

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

### <a name="connections"></a><span data-ttu-id="d8093-140">Připojení</span><span class="sxs-lookup"><span data-stu-id="d8093-140">Connections</span></span>

<span data-ttu-id="d8093-141">kategorie připojení Hello sleduje chyb vzniklých při zařízení připojení nebo odpojení od služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d8093-141">hello connections category tracks errors that occur when devices connect or disconnect from an IoT hub.</span></span> <span data-ttu-id="d8093-142">Sledování této kategorie jsou užitečné pro identifikaci pokusy o neoprávněné připojení a sledování při ztrátě pro zařízení v oblastech malé možnosti připojení připojení.</span><span class="sxs-lookup"><span data-stu-id="d8093-142">Tracking this category is useful for identifying unauthorized connection attempts and for tracking when a connection is lost for devices in areas of poor connectivity.</span></span>

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

### <a name="file-uploads"></a><span data-ttu-id="d8093-143">Nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="d8093-143">File uploads</span></span>

<span data-ttu-id="d8093-144">kategorie nahrávání souboru Hello sleduje chybách, ke kterým dochází při hello IoT hub a jsou související toofile odesílání funkce.</span><span class="sxs-lookup"><span data-stu-id="d8093-144">hello file upload category tracks errors that occur at hello IoT hub and are related toofile upload functionality.</span></span> <span data-ttu-id="d8093-145">Tato kategorie zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="d8093-145">This category includes:</span></span>

* <span data-ttu-id="d8093-146">Chyby, které se hello identifikátor URI SAS, jako je například vypršení platnosti před zařízení upozorní hello rozbočovače dokončené odeslání.</span><span class="sxs-lookup"><span data-stu-id="d8093-146">Errors that occur with hello SAS URI, such as when it expires before a device notifies hello hub of a completed upload.</span></span>
* <span data-ttu-id="d8093-147">Nepodařilo nahrávání hlášené hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="d8093-147">Failed uploads reported by hello device.</span></span>
* <span data-ttu-id="d8093-148">Chyby, které nastat, pokud soubor nebyl nalezen v úložišti během vytváření zpráv oznámení služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d8093-148">Errors that occur when a file is not found in storage during IoT Hub notification message creation.</span></span>

<span data-ttu-id="d8093-149">Tuto kategorii nelze catch chyb, které přímo nastat při hello zařízení odesílá toostorage souboru.</span><span class="sxs-lookup"><span data-stu-id="d8093-149">This category cannot catch errors that directly occur while hello device is uploading a file toostorage.</span></span>

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

### <a name="message-routing"></a><span data-ttu-id="d8093-150">Směrování zpráv</span><span class="sxs-lookup"><span data-stu-id="d8093-150">Message routing</span></span>

<span data-ttu-id="d8093-151">kategorie směrování zpráv Hello sleduje chyb vzniklých při vyhodnocení zpráva trasy a stav koncového bodu zaznamenatelného službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d8093-151">hello message routing category tracks errors that occur during message route evaluation and endpoint health as perceived by IoT Hub.</span></span> <span data-ttu-id="d8093-152">Tato kategorie zahrnuje události, například pokud se pravidlo vyhodnotí jako příliš "undefined", když IoT Hub označí koncový bod zpráv a všechny ostatní chyby přijaté z koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="d8093-152">This category includes events such as when a rule evaluates too"undefined", when IoT Hub marks an endpoint as dead, and any other errors received from an endpoint.</span></span> <span data-ttu-id="d8093-153">Tato kategorie nezahrnuje konkrétní chyby týkající se zpráv hello, sami (například zařízení omezení chyby), které jsou v části kategorie "zařízení telemetrická" hello.</span><span class="sxs-lookup"><span data-stu-id="d8093-153">This category does not include specific errors about hello messages themselves (such as device throttling errors), which are reported under hello "device telemetry" category.</span></span>

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

## <a name="view-events"></a><span data-ttu-id="d8093-154">zobrazení událostí</span><span class="sxs-lookup"><span data-stu-id="d8093-154">View events</span></span>

<span data-ttu-id="d8093-155">Můžete použít hello *iothub-explorer* tooquickly nástroj testování, aby služby IoT hub je generování události monitorování.</span><span class="sxs-lookup"><span data-stu-id="d8093-155">You can use hello *iothub-explorer* tool tooquickly test that your IoT hub is generating monitoring events.</span></span> <span data-ttu-id="d8093-156">tooinstall hello nástroje najdete v pokynech hello v hello [iothub-explorer] [ lnk-iothub-explorer] úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="d8093-156">tooinstall hello tool, see hello instructions in hello [iothub-explorer][lnk-iothub-explorer] GitHub repository.</span></span>

1. <span data-ttu-id="d8093-157">Ujistěte se, zda text hello **připojení** monitorování kategorie je nastaven příliš**podrobné** hello portálu.</span><span class="sxs-lookup"><span data-stu-id="d8093-157">Make sure hello **Connections** monitoring category is set too**Verbose** in hello portal.</span></span>

1. <span data-ttu-id="d8093-158">V příkazovém řádku spusťte následující příkaz tooread z koncového bodu monitorování hello hello:</span><span class="sxs-lookup"><span data-stu-id="d8093-158">At a command-prompt, run hello following command tooread from hello monitoring endpoint:</span></span>

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="d8093-159">V jiné příkazového řádku spusťte následující příkaz toosimulate hello zařízení odesílání zpráv typu zařízení cloud:</span><span class="sxs-lookup"><span data-stu-id="d8093-159">In another command-prompt, run hello following command toosimulate a device sending device-to-cloud messages:</span></span>

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. <span data-ttu-id="d8093-160">Hello první příkazového řádku ukazuje události monitorování hello hello simulované zařízení připojí tooyour služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d8093-160">hello first command-prompt shows hello monitoring events as hello simulated device connects tooyour IoT hub.</span></span>

## <a name="connect-toohello-monitoring-endpoint"></a><span data-ttu-id="d8093-161">Připojit toohello koncového bodu monitorování</span><span class="sxs-lookup"><span data-stu-id="d8093-161">Connect toohello monitoring endpoint</span></span>

<span data-ttu-id="d8093-162">Hello monitorování koncového bodu ve službě IoT hub je koncový bod kompatibilní s centrem událostí.</span><span class="sxs-lookup"><span data-stu-id="d8093-162">hello monitoring endpoint on your IoT hub is an Event Hub-compatible endpoint.</span></span> <span data-ttu-id="d8093-163">Můžete použít jakéhokoli mechanismu, který funguje s Event Hubs tooread monitorování zprávy z tohoto koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="d8093-163">You can use any mechanism that works with Event Hubs tooread monitoring messages from this endpoint.</span></span> <span data-ttu-id="d8093-164">Hello následující ukázka vytvoří základní čtečka, která není vhodná pro vysoce výkonná nasazení.</span><span class="sxs-lookup"><span data-stu-id="d8093-164">hello following sample creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="d8093-165">Další informace o tom, jak tooprocess zpráv ze služby Event Hubs naleznete v tématu hello [Začínáme se službou Event Hubs] [ lnk-eventhubs-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="d8093-165">For more information about how tooprocess messages from Event Hubs, see hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial.</span></span>

<span data-ttu-id="d8093-166">tooconnect toohello koncového bodu monitorování, je třeba název připojovacího řetězce a hello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="d8093-166">tooconnect toohello monitoring endpoint, you need a connection string and hello endpoint name.</span></span> <span data-ttu-id="d8093-167">Hello následující kroky ukazují, jak toofind hello nezbytných hodnot hello portálu:</span><span class="sxs-lookup"><span data-stu-id="d8093-167">hello following steps show you how toofind hello necessary values in hello portal:</span></span>

1. <span data-ttu-id="d8093-168">Hello portálu přejděte tooyour okna prostředků služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d8093-168">In hello portal, navigate tooyour IoT Hub resource blade.</span></span>

1. <span data-ttu-id="d8093-169">Zvolte **Operations monitorování**a poznamenejte si hello **název kompatibilní s centrem událostí** a **koncový bod kompatibilní s centrem událostí** hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d8093-169">Choose **Operations monitoring**, and make a note of hello **Event Hub-compatible name** and **Event Hub-compatible endpoint** values:</span></span>

    ![Hodnoty koncový bod kompatibilní s centrem událostí][img-endpoints]

1. <span data-ttu-id="d8093-171">Zvolte **zásady sdíleného přístupu**, zvolte **služby**.</span><span class="sxs-lookup"><span data-stu-id="d8093-171">Choose **Shared access policies**, then choose **service**.</span></span> <span data-ttu-id="d8093-172">Poznamenejte si hello **primární klíč** hodnotu:</span><span class="sxs-lookup"><span data-stu-id="d8093-172">Make a note of hello **Primary key** value:</span></span>

    ![Primární klíč pro zásady sdíleného přístupu služby][img-service-key]

<span data-ttu-id="d8093-174">Hello následující ukázka kódu C# jsou převzaty ze sady Visual Studio **Windows Classic Desktop** konzolovou aplikaci C#.</span><span class="sxs-lookup"><span data-stu-id="d8093-174">hello following C# code sample is taken from a Visual Studio **Windows Classic Desktop** C# console app.</span></span> <span data-ttu-id="d8093-175">Hello projekt má hello **WindowsAzure.ServiceBus** nainstalovat balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="d8093-175">hello project has hello **WindowsAzure.ServiceBus** NuGet package installed.</span></span>

* <span data-ttu-id="d8093-176">Nahraďte zástupný symbol připojovacího řetězce hello připojovací řetězec, který používá hello **koncový bod kompatibilní s centrem událostí** a služba **primární klíč** hodnoty, které jste si poznamenali dříve, jak je znázorněno v následující hello Příklad:</span><span class="sxs-lookup"><span data-stu-id="d8093-176">Replace hello connection string placeholder with a connection string that uses hello **Event Hub-compatible endpoint** and service **Primary key** values you noted previously as shown in hello following example:</span></span>

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* <span data-ttu-id="d8093-177">Nahraďte hello monitorování název koncového bodu zástupný symbol hello **název kompatibilní s centrem událostí** hodnoty, které jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="d8093-177">Replace hello monitoring endpoint name placeholder with hello **Event Hub-compatible name** value you noted previously.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="d8093-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8093-178">Next steps</span></span>
<span data-ttu-id="d8093-179">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="d8093-179">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d8093-180">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="d8093-180">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="d8093-181">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="d8093-181">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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