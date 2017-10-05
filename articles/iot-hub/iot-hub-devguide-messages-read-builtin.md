---
title: "Pochopení předdefinovaný koncový bod Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – popisuje, jak používat integrované, prečíst zařízení cloud zprávy koncový bod kompatibilní s centrem událostí."
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
ms.openlocfilehash: fcc3743028e369fdc42b71887d49fb41fba2c0dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a><span data-ttu-id="9bfcc-103">Číst zprávy typu zařízení cloud z předdefinovaných koncového bodu</span><span class="sxs-lookup"><span data-stu-id="9bfcc-103">Read device-to-cloud messages from the built-in endpoint</span></span>

<span data-ttu-id="9bfcc-104">Ve výchozím nastavení, zprávy jsou směrovány na předdefinovaný koncový bod služby směřujících (**zprávy nebo události**), který je kompatibilní s [Event Hubs][lnk-event-hubs].</span><span class="sxs-lookup"><span data-stu-id="9bfcc-104">By default, messages are routed to the built-in service-facing endpoint (**messages/events**), that is compatible with [Event Hubs][lnk-event-hubs].</span></span> <span data-ttu-id="9bfcc-105">Tento koncový bod je aktuálně jenom zveřejněné pomocí [AMQP] [ lnk-amqp] protokolu na portu 5671.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-105">This endpoint is currently only exposed using the [AMQP][lnk-amqp] protocol on port 5671.</span></span> <span data-ttu-id="9bfcc-106">IoT hub zpřístupní následující vlastnosti pro vám umožňují řídit předdefinované zasílání zpráv koncový bod kompatibilní s centrem událostí **zprávy nebo události**.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-106">An IoT hub exposes the following properties to enable you to control the built-in Event Hub-compatible messaging endpoint **messages/events**.</span></span>

| <span data-ttu-id="9bfcc-107">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9bfcc-107">Property</span></span>            | <span data-ttu-id="9bfcc-108">Popis</span><span class="sxs-lookup"><span data-stu-id="9bfcc-108">Description</span></span> |
| ------------------- | ----------- |
| <span data-ttu-id="9bfcc-109">**Počet oddílů**</span><span class="sxs-lookup"><span data-stu-id="9bfcc-109">**Partition count**</span></span> | <span data-ttu-id="9bfcc-110">Tuto vlastnost nastavit při vytváření zadat počet [oddíly] [ lnk-event-hub-partitions] pro přijímání událostí zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-110">Set this property at creation to define the number of [partitions][lnk-event-hub-partitions] for device-to-cloud event ingestion.</span></span> |
| <span data-ttu-id="9bfcc-111">**Doba uchování**</span><span class="sxs-lookup"><span data-stu-id="9bfcc-111">**Retention time**</span></span>  | <span data-ttu-id="9bfcc-112">Tato vlastnost určuje, jak dlouho ve dnech, po které zprávy jsou uchována službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-112">This property specifies how long in days messages are retained by IoT Hub.</span></span> <span data-ttu-id="9bfcc-113">Výchozí hodnota je jeden den, ale je možné zvýšit na sedm dní.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-113">The default is one day, but it can be increased to seven days.</span></span> |

<span data-ttu-id="9bfcc-114">IoT Hub můžete také spravovat skupiny příjemců na integrované zařízení cloud přijímat koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-114">IoT Hub also enables you to manage consumer groups on the built-in device-to-cloud receive endpoint.</span></span>

<span data-ttu-id="9bfcc-115">Ve výchozím nastavení jsou všechny zprávy, které neodpovídají explicitně pravidel směrování zpráv zapisují na předdefinovaný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-115">By default, all messages that do not explicitly match a message routing rule are written to the built-in endpoint.</span></span> <span data-ttu-id="9bfcc-116">Pokud zakážete toto záložní směrování, jsou zprávy, které neodpovídají explicitně všechna pravidla pro směrování zpráv vyřadit.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-116">If you disable this fallback route, messages that do not explicitly match any message routing rules are dropped.</span></span>

<span data-ttu-id="9bfcc-117">Můžete upravit dobu uchování buď programově pomocí [zprostředkovatele prostředků služby IoT Hub rozhraní REST API][lnk-resource-provider-apis], nebo pomocí [portál Azure][lnk-management-portal].</span><span class="sxs-lookup"><span data-stu-id="9bfcc-117">You can modify the retention time, either programmatically through the [IoT Hub resource provider REST APIs][lnk-resource-provider-apis], or by using the [Azure portal][lnk-management-portal].</span></span>

<span data-ttu-id="9bfcc-118">IoT Hub zpřístupní **zprávy nebo události** předdefinovaný koncový bod pro váš back endové služby ke čtení zpráv typu zařízení cloud přijatých rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-118">IoT Hub exposes the **messages/events** built-in endpoint for your back-end services to read the device-to-cloud messages received by your hub.</span></span> <span data-ttu-id="9bfcc-119">Tento koncový bod je událost kompatibilní s centrem, což vám umožní použít některý z mechanismů služby Event Hubs podporuje pro čtení zpráv.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-119">This endpoint is Event Hub-compatible, which enables you to use any of the mechanisms the Event Hubs service supports for reading messages.</span></span>

## <a name="read-from-the-built-in-endpoint"></a><span data-ttu-id="9bfcc-120">Čtení z předdefinovaných koncového bodu</span><span class="sxs-lookup"><span data-stu-id="9bfcc-120">Read from the built-in endpoint</span></span>

<span data-ttu-id="9bfcc-121">Při použití [Azure Service Bus SDK pro .NET] [ lnk-servicebus-sdk] nebo [Event Hubs - Event Processor Host][lnk-eventprocessorhost], můžete použít libovolné púřipojovací řetězce IoT Hub se správnými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-121">When you use the [Azure Service Bus SDK for .NET][lnk-servicebus-sdk] or the [Event Hubs - Event Processor Host][lnk-eventprocessorhost], you can use any IoT Hub connection strings with the correct permissions.</span></span> <span data-ttu-id="9bfcc-122">Potom pomocí **zprávy nebo události** jako název centra událostí.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-122">Then use **messages/events** as the Event Hub name.</span></span>

<span data-ttu-id="9bfcc-123">Při použití sady SDK (nebo Integrace produktu), nebudou o IoT Hub, musíte koncový bod kompatibilní s centrem událostí a název kompatibilní s centrem událostí z nastavení na IoT Hub [portál Azure][lnk-management-portal]:</span><span class="sxs-lookup"><span data-stu-id="9bfcc-123">When you use SDKs (or product integrations) that are unaware of IoT Hub, you must retrieve an Event Hub-compatible endpoint and Event Hub-compatible name from the IoT Hub settings in the [Azure portal][lnk-management-portal]:</span></span>

1. <span data-ttu-id="9bfcc-124">V okně centra IoT klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-124">In the IoT hub blade, click **Endpoints**.</span></span>
1. <span data-ttu-id="9bfcc-125">V **předdefinované koncové body** klikněte na tlačítko **události**.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-125">In the **Built-in endpoints** section, click **Events**.</span></span> <span data-ttu-id="9bfcc-126">V okně obsahuje následující hodnoty: **koncový bod kompatibilní s centrem událostí**, **název kompatibilní s centrem událostí**, **oddíly**, **dobu uchování**, a **skupiny příjemců**.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-126">The blade contains the following values: **Event Hub-compatible endpoint**, **Event Hub-compatible name**, **Partitions**, **Retention time**, and **Consumer groups**.</span></span>

    ![Nastavení zařízení cloud][img-eventhubcompatible]

<span data-ttu-id="9bfcc-128">SDK centra IoT vyžaduje název koncového bodu služby IoT Hub, která je **zprávy nebo události** jak je znázorněno **koncové body** okno.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-128">The IoT Hub SDK requires the IoT Hub endpoint name, which is **messages/events** as shown in the **Endpoints** blade.</span></span>

<span data-ttu-id="9bfcc-129">Pokud používáte sady SDK vyžaduje **Hostname** nebo **Namespace** hodnotu, odeberte schéma z **koncový bod kompatibilní s centrem událostí**.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-129">If the SDK you are using requires a **Hostname** or **Namespace** value, remove the scheme from the **Event Hub-compatible endpoint**.</span></span> <span data-ttu-id="9bfcc-130">Například, pokud je váš koncový bod kompatibilní s centrem událostí **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, **Hostname** by **iothub-ns-myiothub-1234.servicebus.windows.net**a **Namespace** by **iothub-ns-myiothub-1234**.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-130">For example, if your Event Hub-compatible endpoint is **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, the **Hostname** would be **iothub-ns-myiothub-1234.servicebus.windows.net**, and the **Namespace** would be **iothub-ns-myiothub-1234**.</span></span>

<span data-ttu-id="9bfcc-131">Pak můžete použít všechny zásady sdíleného přístupu, který má **ServiceConnect** oprávnění pro připojení k zadané centra událostí.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-131">You can then use any shared access policy that has the **ServiceConnect** permissions to connect to the specified Event Hub.</span></span>

<span data-ttu-id="9bfcc-132">Pokud potřebujete vytvořit připojovací řetězec Centru událostí pomocí předchozí informace, použijte následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="9bfcc-132">If you need to build an Event Hub connection string by using the previous information, use the following pattern:</span></span>

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

<span data-ttu-id="9bfcc-133">Sady SDK a integrace, které můžete použít s koncovými body kompatibilní s centrem událostí, které IoT Hub zpřístupní zahrnuje položky v následujícím seznamu:</span><span class="sxs-lookup"><span data-stu-id="9bfcc-133">The SDKs and integrations that you can use with Event Hub-compatible endpoints that IoT Hub exposes includes the items in the following list:</span></span>

* <span data-ttu-id="9bfcc-134">[Java Event Hubs klienta](https://github.com/hdinsight/eventhubs-client).</span><span class="sxs-lookup"><span data-stu-id="9bfcc-134">[Java Event Hubs client](https://github.com/hdinsight/eventhubs-client).</span></span>
* <span data-ttu-id="9bfcc-135">[Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="9bfcc-135">[Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md).</span></span> <span data-ttu-id="9bfcc-136">Můžete zobrazit [spout zdroje](https://github.com/apache/storm/tree/master/external/storm-eventhubs) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-136">You can view the [spout source](https://github.com/apache/storm/tree/master/external/storm-eventhubs) on GitHub.</span></span>
* <span data-ttu-id="9bfcc-137">[Apache Spark integrace](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).</span><span class="sxs-lookup"><span data-stu-id="9bfcc-137">[Apache Spark integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9bfcc-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9bfcc-138">Next steps</span></span>

<span data-ttu-id="9bfcc-139">Další informace o koncové body centra IoT najdete v tématu [koncové body centra IoT][lnk-endpoints].</span><span class="sxs-lookup"><span data-stu-id="9bfcc-139">For more information about IoT Hub endpoints, see [IoT Hub endpoints][lnk-endpoints].</span></span>

<span data-ttu-id="9bfcc-140">[Začínáme] [ lnk-get-started] kurzy vám ukážou, jak odesílat zprávy typu zařízení cloud ze simulovaného zařízení a čtení zpráv z předdefinovaných koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-140">The [Get Started][lnk-get-started] tutorials show you how to send device-to-cloud messages from simulated devices and read the messages from the built-in endpoint.</span></span> <span data-ttu-id="9bfcc-141">Další podrobnosti najdete [zpráv typu zařízení cloud proces IoT Hub pomocí trasy] [ lnk-d2c-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="9bfcc-141">For more detail, see the [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial.</span></span>

<span data-ttu-id="9bfcc-142">Pokud chcete směrovat vaše zprávy typu zařízení cloud do vlastní koncové body, přečtěte si téma [použít vlastní koncové body a směrování zpráv pro zprávy typu zařízení cloud][lnk-custom].</span><span class="sxs-lookup"><span data-stu-id="9bfcc-142">If you want to route your device-to-cloud messages to custom endpoints, see [Use message routes and custom endpoints for device-to-cloud messages][lnk-custom].</span></span>

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
