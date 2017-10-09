---
title: "zasílání zpráv typu zařízení cloud Azure IoT Hub aaaUnderstand | Microsoft Docs"
description: "Příručka vývojáře – jak toouse zařízení cloud zasílání zpráv službou IoT Hub. Obsahuje informace o odesílání telemetrických dat i bez telemtry data a pomocí směrování zpráv toodeliver."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 07dc8a6be747365c7efbc528ab2762b0d9790758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-device-to-cloud-messages-tooiot-hub"></a><span data-ttu-id="49521-104">Odesílání zpráv typu zařízení cloud tooIoT rozbočovače</span><span class="sxs-lookup"><span data-stu-id="49521-104">Send device-to-cloud messages tooIoT Hub</span></span>

<span data-ttu-id="49521-105">toosend časové řady telemetrie a výstrahy z zařízení tooyour back-end vašeho řešení, odesílání zpráv typu zařízení cloud ze služby IoT hub tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="49521-105">toosend time-series telemetry and alerts from your devices tooyour solution back end, send device-to-cloud messages from your device tooyour IoT hub.</span></span> <span data-ttu-id="49521-106">Informace o dalších zařízení cloud možnosti podporované službou IoT Hub, najdete v části [pokyny komunikace zařízení cloud][lnk-d2c-guidance].</span><span class="sxs-lookup"><span data-stu-id="49521-106">For a discussion of other device-to-cloud options supported by IoT Hub, see [Device-to-cloud communications guidance][lnk-d2c-guidance].</span></span>

<span data-ttu-id="49521-107">Odesílání zpráv typu zařízení cloud prostřednictvím koncového bodu směřujících zařízení (**/devices/ {deviceId} / zprávy/události**).</span><span class="sxs-lookup"><span data-stu-id="49521-107">You send device-to-cloud messages through a device-facing endpoint (**/devices/{deviceId}/messages/events**).</span></span> <span data-ttu-id="49521-108">Pravidla směrování pak směrovat vaše zprávy tooone koncových bodů služby směřujících hello ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="49521-108">Routing rules then route your messages tooone of hello service-facing endpoints on your IoT hub.</span></span> <span data-ttu-id="49521-109">Pravidla směrování použití hello záhlaví a tělo zprávy typu zařízení cloud hello předávaných mezi vašeho centra toodetermine kde tooroute je.</span><span class="sxs-lookup"><span data-stu-id="49521-109">Routing rules use hello headers and body of hello device-to-cloud messages flowing through your hub toodetermine where tooroute them.</span></span> <span data-ttu-id="49521-110">Ve výchozím nastavení, zprávy jsou směrované toohello vestavěným koncovým bodem service přístupem (**zprávy nebo události**), který je kompatibilní s [Event Hubs][lnk-event-hubs].</span><span class="sxs-lookup"><span data-stu-id="49521-110">By default, messages are routed toohello built-in service-facing endpoint (**messages/events**), that is compatible with [Event Hubs][lnk-event-hubs].</span></span> <span data-ttu-id="49521-111">Proto můžete použít standardní [integrace služby Event Hubs a sady SDK] [ lnk-compatible-endpoint] zpráv typu zařízení cloud tooreceive ve vašem řešení back-end.</span><span class="sxs-lookup"><span data-stu-id="49521-111">Therefore, you can use standard [Event Hubs integration and SDKs][lnk-compatible-endpoint] tooreceive device-to-cloud messages in your solution back end.</span></span>

<span data-ttu-id="49521-112">IoT Hub implementuje zařízení cloud zasílání zpráv pomocí streamování vzorcem zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="49521-112">IoT Hub implements device-to-cloud messaging using a streaming messaging pattern.</span></span> <span data-ttu-id="49521-113">Zprávy typu zařízení cloud IoT Hub se více podobají [Event Hubs] [ lnk-event-hubs] *události* než [Service Bus] [ lnk-servicebus] *zprávy* v, aby nedocházelo k velkému počtu událostí prošla hello služby, který může číst několik čtečky.</span><span class="sxs-lookup"><span data-stu-id="49521-113">IoT Hub's device-to-cloud messages are more like [Event Hubs][lnk-event-hubs] *events* than [Service Bus][lnk-servicebus] *messages* in that there is a high volume of events passing through hello service that can be read by multiple readers.</span></span>

<span data-ttu-id="49521-114">Zařízení cloud zasílání zpráv službou IoT Hub má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="49521-114">Device-to-cloud messaging with IoT Hub has hello following characteristics:</span></span>

* <span data-ttu-id="49521-115">Zprávy typu zařízení cloud jsou odolné a udržení ve výchozím nastavení služby IoT hub **zprávy nebo události** koncový bod pro až tooseven dnů.</span><span class="sxs-lookup"><span data-stu-id="49521-115">Device-to-cloud messages are durable and retained in an IoT hub's default **messages/events** endpoint for up tooseven days.</span></span>
* <span data-ttu-id="49521-116">Zprávy typu zařízení cloud může být maximálně 256 KB a mohou být seskupeny do balíků, které odesílá toooptimize.</span><span class="sxs-lookup"><span data-stu-id="49521-116">Device-to-cloud messages can be at most 256 KB, and can be grouped in batches toooptimize sends.</span></span> <span data-ttu-id="49521-117">Dávky může být maximálně 256 KB.</span><span class="sxs-lookup"><span data-stu-id="49521-117">Batches can be at most 256 KB.</span></span>
* <span data-ttu-id="49521-118">Jak je popsáno v hello [řízení přístupu tooIoT rozbočovače] [ lnk-devguide-security] oddíl, IoT Hub umožňuje na zařízení ověřování a řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="49521-118">As explained in hello [Control access tooIoT Hub][lnk-devguide-security] section, IoT Hub enables per-device authentication and access control.</span></span>
* <span data-ttu-id="49521-119">Centrum IoT vám umožní toocreate až too10 vlastní koncové body.</span><span class="sxs-lookup"><span data-stu-id="49521-119">IoT Hub allows you toocreate up too10 custom endpoints.</span></span> <span data-ttu-id="49521-120">Koncové body toohello podle trasy nakonfigurované ve službě IoT hub doručování zpráv.</span><span class="sxs-lookup"><span data-stu-id="49521-120">Messages are delivered toohello endpoints based on routes configured on your IoT hub.</span></span> <span data-ttu-id="49521-121">Další informace najdete v tématu [pravidla směrování](#routing-rules).</span><span class="sxs-lookup"><span data-stu-id="49521-121">For more information, see [Routing rules](#routing-rules).</span></span>
* <span data-ttu-id="49521-122">IoT Hub umožňuje miliony současně připojených zařízení (viz [kvóty a omezení][lnk-quotas]).</span><span class="sxs-lookup"><span data-stu-id="49521-122">IoT Hub enables millions of simultaneously connected devices (see [Quotas and throttling][lnk-quotas]).</span></span>
* <span data-ttu-id="49521-123">IoT Hub neumožňuje libovolný dělení.</span><span class="sxs-lookup"><span data-stu-id="49521-123">IoT Hub does not allow arbitrary partitioning.</span></span> <span data-ttu-id="49521-124">Zprávy typu zařízení cloud jsou rozdělena na oddíly na základě jejich zdrojového **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="49521-124">Device-to-cloud messages are partitioned based on their originating **deviceId**.</span></span>

<span data-ttu-id="49521-125">Další informace o hello rozdíly mezi hello IoT Hub a Event Hubs služby najdete v tématu [porovnání služeb Azure IoT Hub a Azure Event Hubs][lnk-comparison].</span><span class="sxs-lookup"><span data-stu-id="49521-125">For more information about hello differences between hello IoT Hub and Event Hubs services, see [Comparison of Azure IoT Hub and Azure Event Hubs][lnk-comparison].</span></span>

## <a name="send-non-telemetry-traffic"></a><span data-ttu-id="49521-126">Odesílat provoz telemetrická data</span><span class="sxs-lookup"><span data-stu-id="49521-126">Send non-telemetry traffic</span></span>

<span data-ttu-id="49521-127">Často kromě tootelemetry datových bodů, zařízení odesílat zprávy a požadavky, které vyžadují samostatné spuštění a zpracování v back-end hello řešení.</span><span class="sxs-lookup"><span data-stu-id="49521-127">Often, in addition tootelemetry data points, devices send messages and requests that require separate execution and handling in hello solution back end.</span></span> <span data-ttu-id="49521-128">Například kritické výstrahy, které musí aktivovat určité akci v hello back-end.</span><span class="sxs-lookup"><span data-stu-id="49521-128">For example, critical alerts that must trigger a specific action in hello back end.</span></span> <span data-ttu-id="49521-129">Můžete napsat snadno [pravidlo směrování] [ lnk-devguide-custom] toosend tyto typy koncového bodu tooan zprávy vyhrazené tootheir zpracování na základě buď hlavičku na uvítací zprávu nebo hodnotu v textu zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="49521-129">You can easily write a [routing rule][lnk-devguide-custom] toosend these types of messages tooan endpoint dedicated tootheir processing based on either a header on hello message or a value in hello message body.</span></span>

<span data-ttu-id="49521-130">Další informace o hello nejlepší způsob, jak tooprocess tento druh zprávy naleznete v tématu hello [kurz: jak tooprocess zpráv typu zařízení cloud IoT Hub] [ lnk-d2c-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="49521-130">For more information about hello best way tooprocess this kind of message, see hello [Tutorial: How tooprocess IoT Hub device-to-cloud messages][lnk-d2c-tutorial] tutorial.</span></span>

## <a name="route-device-to-cloud-messages"></a><span data-ttu-id="49521-131">Směrování zprávy typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="49521-131">Route device-to-cloud messages</span></span>

<span data-ttu-id="49521-132">Máte dvě možnosti pro směrování zpráv typu zařízení cloud tooyour back-end aplikace:</span><span class="sxs-lookup"><span data-stu-id="49521-132">You have two options for routing device-to-cloud messages tooyour back-end apps:</span></span>

* <span data-ttu-id="49521-133">Použijte integrovanou hello [koncový bod kompatibilní s centrem událostí] [ lnk-compatible-endpoint] tooenable back-end aplikace tooread hello zařízení cloud zprávy přijaté službou hello rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="49521-133">Use hello built-in [Event Hub-compatible endpoint][lnk-compatible-endpoint] tooenable back-end apps tooread hello device-to-cloud messages received by hello hub.</span></span> <span data-ttu-id="49521-134">toolearn o hello předdefinované kompatibilní s centrem událostí koncový bod, najdete v části [číst zprávy typu zařízení cloud ze vestavěným koncovým bodem hello][lnk-devguide-builtin].</span><span class="sxs-lookup"><span data-stu-id="49521-134">toolearn about hello built-in Event Hub-compatible endpoint, see [Read device-to-cloud messages from hello built-in endpoint][lnk-devguide-builtin].</span></span>
* <span data-ttu-id="49521-135">Použití směrování koncové body toocustom pravidla toosend zpráv ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="49521-135">Use routing rules toosend messages toocustom endpoints in your IoT hub.</span></span> <span data-ttu-id="49521-136">Vlastní koncové body povolit zprávy typu zařízení cloud tooread si back-end aplikace pomocí Event Hubs, fronty sběrnice nebo témata Service Bus.</span><span class="sxs-lookup"><span data-stu-id="49521-136">Custom endpoints enable your back-end apps tooread device-to-cloud messages using Event Hubs, Service Bus queues, or Service Bus topics.</span></span> <span data-ttu-id="49521-137">toolearn o směrování a vlastní koncové body, najdete v části [použít vlastní koncové body a pravidla směrování pro zprávy typu zařízení cloud][lnk-devguide-custom].</span><span class="sxs-lookup"><span data-stu-id="49521-137">toolearn about routing and custom endpoints, see [Use custom endpoints and routing rules for device-to-cloud messages][lnk-devguide-custom].</span></span>

## <a name="anti-spoofing-properties"></a><span data-ttu-id="49521-138">Ochranu proti falšování vlastnosti</span><span class="sxs-lookup"><span data-stu-id="49521-138">Anti-spoofing properties</span></span>

<span data-ttu-id="49521-139">zařízení tooavoid falšování identity ve zpráv typu zařízení cloud, IoT Hub razítka všechny zprávy s hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="49521-139">tooavoid device spoofing in device-to-cloud messages, IoT Hub stamps all messages with hello following properties:</span></span>

* <span data-ttu-id="49521-140">**ConnectionDeviceId**</span><span class="sxs-lookup"><span data-stu-id="49521-140">**ConnectionDeviceId**</span></span>
* <span data-ttu-id="49521-141">**ConnectionDeviceGenerationId**</span><span class="sxs-lookup"><span data-stu-id="49521-141">**ConnectionDeviceGenerationId**</span></span>
* <span data-ttu-id="49521-142">**ConnectionAuthMethod**</span><span class="sxs-lookup"><span data-stu-id="49521-142">**ConnectionAuthMethod**</span></span>

<span data-ttu-id="49521-143">Hello první dva obsahovat hello **deviceId** a **generationId** z hello pocházející zařízení, jako za [vlastnosti identity zařízení][lnk-device-properties].</span><span class="sxs-lookup"><span data-stu-id="49521-143">hello first two contain hello **deviceId** and **generationId** of hello originating device, as per [Device identity properties][lnk-device-properties].</span></span>

<span data-ttu-id="49521-144">Hello **ConnectionAuthMethod** vlastnost obsahuje objekt serializován do formátu JSON, s hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="49521-144">hello **ConnectionAuthMethod** property contains a JSON serialized object, with hello following properties:</span></span>

```json
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a><span data-ttu-id="49521-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49521-145">Next steps</span></span>

<span data-ttu-id="49521-146">Informace o hello sady SDK můžete použít zpráv typu zařízení cloud toosend najdete v tématu [SDK služby Azure IoT][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="49521-146">For information about hello SDKs you can use toosend device-to-cloud messages, see [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="49521-147">Hello [Začínáme] [ lnk-get-started] kurzy vám ukážou, jak toosend zařízení cloud zprávy ze simulovaného a fyzické zařízení.</span><span class="sxs-lookup"><span data-stu-id="49521-147">hello [Get Started][lnk-get-started] tutorials show you how toosend device-to-cloud messages from both simulated and physical devices.</span></span> <span data-ttu-id="49521-148">Další podrobnosti naleznete v hello [zpráv typu zařízení cloud proces IoT Hub pomocí trasy] [ lnk-d2c-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="49521-148">For more detail, see hello [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial.</span></span>

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: iot-hub-get-started.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
