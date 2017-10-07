---
title: "vestavěným koncovým bodem aaaUnderstand hello Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – popisuje, jak toouse hello integrovaných zpráv typu zařízení cloud prečíst koncový bod kompatibilní s centrem událostí."
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
ms.openlocfilehash: 15484c1b1828151ffcae5f4a1407264374223da1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="read-device-to-cloud-messages-from-hello-built-in-endpoint"></a><span data-ttu-id="d49c4-103">Číst zprávy typu zařízení cloud ze vestavěným koncovým bodem hello</span><span class="sxs-lookup"><span data-stu-id="d49c4-103">Read device-to-cloud messages from hello built-in endpoint</span></span>

<span data-ttu-id="d49c4-104">Ve výchozím nastavení, zprávy jsou směrované toohello vestavěným koncovým bodem service přístupem (**zprávy nebo události**), který je kompatibilní s [Event Hubs][lnk-event-hubs].</span><span class="sxs-lookup"><span data-stu-id="d49c4-104">By default, messages are routed toohello built-in service-facing endpoint (**messages/events**), that is compatible with [Event Hubs][lnk-event-hubs].</span></span> <span data-ttu-id="d49c4-105">Tento koncový bod je momentálně dostupná jenom v případě použití hello [AMQP] [ lnk-amqp] protokolu na portu 5671.</span><span class="sxs-lookup"><span data-stu-id="d49c4-105">This endpoint is currently only exposed using hello [AMQP][lnk-amqp] protocol on port 5671.</span></span> <span data-ttu-id="d49c4-106">IoT hub zpřístupní hello následující vlastnosti tooenable jste toocontrol hello předdefinované zasílání zpráv koncového bodu kompatibilního s centrem událostí **zprávy nebo události**.</span><span class="sxs-lookup"><span data-stu-id="d49c4-106">An IoT hub exposes hello following properties tooenable you toocontrol hello built-in Event Hub-compatible messaging endpoint **messages/events**.</span></span>

| <span data-ttu-id="d49c4-107">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="d49c4-107">Property</span></span>            | <span data-ttu-id="d49c4-108">Popis</span><span class="sxs-lookup"><span data-stu-id="d49c4-108">Description</span></span> |
| ------------------- | ----------- |
| <span data-ttu-id="d49c4-109">**Počet oddílů**</span><span class="sxs-lookup"><span data-stu-id="d49c4-109">**Partition count**</span></span> | <span data-ttu-id="d49c4-110">Nastavte tuto vlastnost Po vytvoření toodefine hello počtu [oddíly] [ lnk-event-hub-partitions] pro přijímání událostí zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="d49c4-110">Set this property at creation toodefine hello number of [partitions][lnk-event-hub-partitions] for device-to-cloud event ingestion.</span></span> |
| <span data-ttu-id="d49c4-111">**Doba uchování**</span><span class="sxs-lookup"><span data-stu-id="d49c4-111">**Retention time**</span></span>  | <span data-ttu-id="d49c4-112">Tato vlastnost určuje, jak dlouho ve dnech, po které zprávy jsou uchována službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d49c4-112">This property specifies how long in days messages are retained by IoT Hub.</span></span> <span data-ttu-id="d49c4-113">Výchozí hodnota Hello je jeden den, ale může být vyšší tooseven dnů.</span><span class="sxs-lookup"><span data-stu-id="d49c4-113">hello default is one day, but it can be increased tooseven days.</span></span> |

<span data-ttu-id="d49c4-114">IoT Hub můžete také skupiny příjemců toomanage na hello integrované zařízení cloud přijímat koncový bod.</span><span class="sxs-lookup"><span data-stu-id="d49c4-114">IoT Hub also enables you toomanage consumer groups on hello built-in device-to-cloud receive endpoint.</span></span>

<span data-ttu-id="d49c4-115">Ve výchozím nastavení jsou všechny zprávy, které neodpovídají explicitně pravidel směrování zpráv zapisují toohello vestavěným koncovým bodem.</span><span class="sxs-lookup"><span data-stu-id="d49c4-115">By default, all messages that do not explicitly match a message routing rule are written toohello built-in endpoint.</span></span> <span data-ttu-id="d49c4-116">Pokud zakážete toto záložní směrování, jsou zprávy, které neodpovídají explicitně všechna pravidla pro směrování zpráv vyřadit.</span><span class="sxs-lookup"><span data-stu-id="d49c4-116">If you disable this fallback route, messages that do not explicitly match any message routing rules are dropped.</span></span>

<span data-ttu-id="d49c4-117">Můžete upravit dobu uchování hello, buď prostřednictvím kódu programu prostřednictvím hello [zprostředkovatele prostředků služby IoT Hub rozhraní REST API][lnk-resource-provider-apis], nebo pomocí hello [portál Azure] [ lnk-management-portal].</span><span class="sxs-lookup"><span data-stu-id="d49c4-117">You can modify hello retention time, either programmatically through hello [IoT Hub resource provider REST APIs][lnk-resource-provider-apis], or by using hello [Azure portal][lnk-management-portal].</span></span>

<span data-ttu-id="d49c4-118">IoT Hub zpřístupní hello **zprávy nebo události** vestavěným koncovým bodem pro back-end služby přijatých vašeho centra zpráv typu zařízení cloud tooread hello.</span><span class="sxs-lookup"><span data-stu-id="d49c4-118">IoT Hub exposes hello **messages/events** built-in endpoint for your back-end services tooread hello device-to-cloud messages received by your hub.</span></span> <span data-ttu-id="d49c4-119">Tento koncový bod je událost kompatibilní s centrem, což vám umožní toouse žádné služby Event Hubs hello mechanismy hello podporuje pro čtení zpráv.</span><span class="sxs-lookup"><span data-stu-id="d49c4-119">This endpoint is Event Hub-compatible, which enables you toouse any of hello mechanisms hello Event Hubs service supports for reading messages.</span></span>

## <a name="read-from-hello-built-in-endpoint"></a><span data-ttu-id="d49c4-120">Čtení z koncového bodu předdefinované hello</span><span class="sxs-lookup"><span data-stu-id="d49c4-120">Read from hello built-in endpoint</span></span>

<span data-ttu-id="d49c4-121">Při použití hello [Azure Service Bus SDK pro .NET] [ lnk-servicebus-sdk] nebo hello [Event Hubs - Event Processor Host][lnk-eventprocessorhost], můžete použít jakékoli připojení, IoT Hub řetězce s hello správná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="d49c4-121">When you use hello [Azure Service Bus SDK for .NET][lnk-servicebus-sdk] or hello [Event Hubs - Event Processor Host][lnk-eventprocessorhost], you can use any IoT Hub connection strings with hello correct permissions.</span></span> <span data-ttu-id="d49c4-122">Potom pomocí **zprávy nebo události** jako název centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="d49c4-122">Then use **messages/events** as hello Event Hub name.</span></span>

<span data-ttu-id="d49c4-123">Při použití sady SDK (nebo Integrace produktu), nebudou o IoT Hub, musíte koncový bod kompatibilní s centrem událostí a název kompatibilní s centrem událostí z nastavení hello IoT Hub v hello [portál Azure] [ lnk-management-portal]:</span><span class="sxs-lookup"><span data-stu-id="d49c4-123">When you use SDKs (or product integrations) that are unaware of IoT Hub, you must retrieve an Event Hub-compatible endpoint and Event Hub-compatible name from hello IoT Hub settings in hello [Azure portal][lnk-management-portal]:</span></span>

1. <span data-ttu-id="d49c4-124">V okně centra IoT hello, klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="d49c4-124">In hello IoT hub blade, click **Endpoints**.</span></span>
1. <span data-ttu-id="d49c4-125">V hello **předdefinované koncové body** klikněte na tlačítko **události**.</span><span class="sxs-lookup"><span data-stu-id="d49c4-125">In hello **Built-in endpoints** section, click **Events**.</span></span> <span data-ttu-id="d49c4-126">Hello okno obsahuje hello následující hodnoty: **koncový bod kompatibilní s centrem událostí**, **název kompatibilní s centrem událostí**, **oddíly**, **dobu uchování**, a **skupiny příjemců**.</span><span class="sxs-lookup"><span data-stu-id="d49c4-126">hello blade contains hello following values: **Event Hub-compatible endpoint**, **Event Hub-compatible name**, **Partitions**, **Retention time**, and **Consumer groups**.</span></span>

    ![Nastavení zařízení cloud][img-eventhubcompatible]

<span data-ttu-id="d49c4-128">Hello sady SDK centra IoT vyžaduje hello název koncového bodu služby IoT Hub, což je **zprávy nebo události** jak je znázorněno v hello **koncové body** okno.</span><span class="sxs-lookup"><span data-stu-id="d49c4-128">hello IoT Hub SDK requires hello IoT Hub endpoint name, which is **messages/events** as shown in hello **Endpoints** blade.</span></span>

<span data-ttu-id="d49c4-129">Pokud vyžaduje hello používáte sady SDK **Hostname** nebo **Namespace** hodnotu, odeberte hello schéma z hello **koncový bod kompatibilní s centrem událostí**.</span><span class="sxs-lookup"><span data-stu-id="d49c4-129">If hello SDK you are using requires a **Hostname** or **Namespace** value, remove hello scheme from hello **Event Hub-compatible endpoint**.</span></span> <span data-ttu-id="d49c4-130">Například, pokud je váš koncový bod kompatibilní s centrem událostí **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, hello **Hostname** by  **iothub-ns-myiothub-1234.servicebus.windows.net**a hello **Namespace** by **iothub-ns-myiothub-1234**.</span><span class="sxs-lookup"><span data-stu-id="d49c4-130">For example, if your Event Hub-compatible endpoint is **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, hello **Hostname** would be **iothub-ns-myiothub-1234.servicebus.windows.net**, and hello **Namespace** would be **iothub-ns-myiothub-1234**.</span></span>

<span data-ttu-id="d49c4-131">Pak můžete použít všechny zásady sdíleného přístupu, který má hello **ServiceConnect** toohello tooconnect oprávnění zadaný centra událostí.</span><span class="sxs-lookup"><span data-stu-id="d49c4-131">You can then use any shared access policy that has hello **ServiceConnect** permissions tooconnect toohello specified Event Hub.</span></span>

<span data-ttu-id="d49c4-132">Pokud potřebujete toobuild připojovací řetězec Centru událostí pomocí hello předchozí informace, použijte následující vzor hello:</span><span class="sxs-lookup"><span data-stu-id="d49c4-132">If you need toobuild an Event Hub connection string by using hello previous information, use hello following pattern:</span></span>

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

<span data-ttu-id="d49c4-133">Hello sady SDK a integrace, které můžete použít s koncovými body kompatibilní s centrem událostí, které IoT Hub zpřístupní zahrnuje hello položky v hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="d49c4-133">hello SDKs and integrations that you can use with Event Hub-compatible endpoints that IoT Hub exposes includes hello items in hello following list:</span></span>

* <span data-ttu-id="d49c4-134">[Java Event Hubs klienta](https://github.com/hdinsight/eventhubs-client).</span><span class="sxs-lookup"><span data-stu-id="d49c4-134">[Java Event Hubs client](https://github.com/hdinsight/eventhubs-client).</span></span>
* <span data-ttu-id="d49c4-135">[Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="d49c4-135">[Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md).</span></span> <span data-ttu-id="d49c4-136">Můžete zobrazit hello [spout zdroje](https://github.com/apache/storm/tree/master/external/storm-eventhubs) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="d49c4-136">You can view hello [spout source](https://github.com/apache/storm/tree/master/external/storm-eventhubs) on GitHub.</span></span>
* <span data-ttu-id="d49c4-137">[Apache Spark integrace](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).</span><span class="sxs-lookup"><span data-stu-id="d49c4-137">[Apache Spark integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d49c4-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d49c4-138">Next steps</span></span>

<span data-ttu-id="d49c4-139">Další informace o koncové body centra IoT najdete v tématu [koncové body centra IoT][lnk-endpoints].</span><span class="sxs-lookup"><span data-stu-id="d49c4-139">For more information about IoT Hub endpoints, see [IoT Hub endpoints][lnk-endpoints].</span></span>

<span data-ttu-id="d49c4-140">Hello [Začínáme] [ lnk-get-started] kurzy vám ukážou, jak zpráv typu zařízení cloud toosend z simulované zařízení a čtení zpráv hello z vestavěným koncovým bodem hello.</span><span class="sxs-lookup"><span data-stu-id="d49c4-140">hello [Get Started][lnk-get-started] tutorials show you how toosend device-to-cloud messages from simulated devices and read hello messages from hello built-in endpoint.</span></span> <span data-ttu-id="d49c4-141">Další podrobnosti naleznete v hello [zpráv typu zařízení cloud proces IoT Hub pomocí trasy] [ lnk-d2c-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="d49c4-141">For more detail, see hello [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial.</span></span>

<span data-ttu-id="d49c4-142">Pokud chcete, aby tooroute vaše zařízení cloud zprávy toocustom koncové body, najdete v části [použít vlastní koncové body a směrování zpráv pro zprávy typu zařízení cloud][lnk-custom].</span><span class="sxs-lookup"><span data-stu-id="d49c4-142">If you want tooroute your device-to-cloud messages toocustom endpoints, see [Use message routes and custom endpoints for device-to-cloud messages][lnk-custom].</span></span>

[img-eventhubcompatible]: ./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png

[lnk-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-management-portal]: https://portal.azure.com
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-event-hub-partitions]: ../event-hubs/event-hubs-features.md#partitions
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx
[lnk-amqp]: https://www.amqp.org/
