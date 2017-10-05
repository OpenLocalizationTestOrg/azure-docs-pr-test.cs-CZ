---
title: "Pochopení vlastní koncové body Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - pravidel směrování pro směrování zpráv typu zařízení cloud do vlastní koncové body."
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
ms.openlocfilehash: a21f1c61f344f96e2e03422e41fd8c5f7f841a0c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a><span data-ttu-id="94446-103">Použít vlastní koncové body a směrování zpráv pro zprávy typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="94446-103">Use message routes and custom endpoints for device-to-cloud messages</span></span>

<span data-ttu-id="94446-104">IoT Hub umožňuje směrovat [zpráv typu zařízení cloud] [ lnk-device-to-cloud] na koncové body služby směřujících centra IoT podle vlastnosti zprávy.</span><span class="sxs-lookup"><span data-stu-id="94446-104">IoT Hub enables you to route [device-to-cloud messages][lnk-device-to-cloud] to IoT Hub service-facing endpoints based on message properties.</span></span> <span data-ttu-id="94446-105">Pravidla směrování poskytují flexibilitu pro odesílání zpráv, kde musí přejít na zpracování zpráv bez nutnosti dalších služeb nebo napsat další kód.</span><span class="sxs-lookup"><span data-stu-id="94446-105">Routing rules give you the flexibility to send messages where they need to go without the need for additional services to process messages or to write additional code.</span></span> <span data-ttu-id="94446-106">Každé pravidlo směrování, která můžete konfigurovat má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="94446-106">Each routing rule you configure has the following properties:</span></span>

| <span data-ttu-id="94446-107">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="94446-107">Property</span></span>      | <span data-ttu-id="94446-108">Popis</span><span class="sxs-lookup"><span data-stu-id="94446-108">Description</span></span> |
| ------------- | ----------- |
| <span data-ttu-id="94446-109">**Název**</span><span class="sxs-lookup"><span data-stu-id="94446-109">**Name**</span></span>      | <span data-ttu-id="94446-110">Jedinečný název, který identifikuje pravidlo.</span><span class="sxs-lookup"><span data-stu-id="94446-110">The unique name that identifies the rule.</span></span> |
| <span data-ttu-id="94446-111">**Zdroj**</span><span class="sxs-lookup"><span data-stu-id="94446-111">**Source**</span></span>    | <span data-ttu-id="94446-112">Původ datový proud do být reagovali na ni.</span><span class="sxs-lookup"><span data-stu-id="94446-112">The origin of the data stream to be acted upon.</span></span> <span data-ttu-id="94446-113">Například zařízení telemetrie.</span><span class="sxs-lookup"><span data-stu-id="94446-113">For example, device telemetry.</span></span> |
| <span data-ttu-id="94446-114">**Podmínka**</span><span class="sxs-lookup"><span data-stu-id="94446-114">**Condition**</span></span> | <span data-ttu-id="94446-115">Výraz dotazu pro směrování pravidlo, které je spouštění hlavičky a text zprávy a používá k určení, zda je shoda pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="94446-115">The query expression for the routing rule that is run against the message's headers and body and used to determine whether it is a match for the endpoint.</span></span> <span data-ttu-id="94446-116">Další informace o vytváření podmínku trasy, najdete v článku [odkaz - dotazovacího jazyka pro úlohy a dvojčata zařízení][lnk-devguide-query-language].</span><span class="sxs-lookup"><span data-stu-id="94446-116">For more information about constructing a route condition, see the [Reference - query language for device twins and jobs][lnk-devguide-query-language].</span></span> |
| <span data-ttu-id="94446-117">**Koncový bod**</span><span class="sxs-lookup"><span data-stu-id="94446-117">**Endpoint**</span></span>  | <span data-ttu-id="94446-118">Název koncového bodu, kde IoT Hub odešle zprávy, které splňují podmínku.</span><span class="sxs-lookup"><span data-stu-id="94446-118">The name of the endpoint where IoT Hub sends messages that match the condition.</span></span> <span data-ttu-id="94446-119">Koncové body musí být ve stejné oblasti jako službu IoT hub, jinak vám může být účtovat mezi oblastmi zápisy.</span><span class="sxs-lookup"><span data-stu-id="94446-119">Endpoints should be in the same region as the IoT hub, otherwise you may be charged for cross-region writes.</span></span> |

<span data-ttu-id="94446-120">Do jedné zprávy může odpovídat na více pravidel směrování, ve kterých případ IoT Hub doručení zprávy do koncového bodu spojené s každou odpovídající pravidlo podmínku.</span><span class="sxs-lookup"><span data-stu-id="94446-120">A single message may match the condition on multiple routing rules, in which case IoT Hub delivers the message to the endpoint associated with each matched rule.</span></span> <span data-ttu-id="94446-121">Centrum IoT také automaticky deduplicates doručení zpráv, takže pokud zpráva odpovídá více pravidel, které mají stejný cíl, je pouze zapsán do tohoto cíle jednou.</span><span class="sxs-lookup"><span data-stu-id="94446-121">IoT Hub also automatically deduplicates message delivery, so if a message matches multiple rules that all have the same destination, it is only written to that destination once.</span></span>

<span data-ttu-id="94446-122">Centrum IoT má výchozí [vestavěným koncovým bodem][lnk-built-in].</span><span class="sxs-lookup"><span data-stu-id="94446-122">An IoT hub has a default [built-in endpoint][lnk-built-in].</span></span> <span data-ttu-id="94446-123">Můžete vytvořit vlastní koncové body pro směrování zpráv do pomocí jiných služeb v rámci vašeho předplatného propojení k rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="94446-123">You can create custom endpoints to route messages to by linking other services in your subscription to the hub.</span></span> <span data-ttu-id="94446-124">IoT Hub aktuálně podporuje Event Hubs, fronty Service Bus a témat sběrnice Service Bus jako vlastní koncové body.</span><span class="sxs-lookup"><span data-stu-id="94446-124">IoT Hub currently supports Event Hubs, Service Bus queues, and Service Bus topics as custom endpoints.</span></span>

> [!WARNING]
> <span data-ttu-id="94446-125">Fronty sběrnice a témata s **relací** nebo **duplicitní detekce** povoleno nejsou podporovány jako vlastní koncové body.</span><span class="sxs-lookup"><span data-stu-id="94446-125">Service Bus queues and topics with **Sessions** or **Duplicate Detection** enabled are not supported as custom endpoints.</span></span>

<span data-ttu-id="94446-126">Další informace o vytváření vlastní koncové body v centru IoT najdete v tématu [koncové body centra IoT][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="94446-126">For more information about creating custom endpoints in IoT Hub, see [IoT Hub endpoints][lnk-devguide-endpoints].</span></span>

<span data-ttu-id="94446-127">Další informace o čtení ze vlastní koncové body najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="94446-127">For more information about reading from custom endpoints, see:</span></span>

* <span data-ttu-id="94446-128">Čtení z [služby Event Hubs][lnk-getstarted-eh].</span><span class="sxs-lookup"><span data-stu-id="94446-128">Reading from [Event Hubs][lnk-getstarted-eh].</span></span>
* <span data-ttu-id="94446-129">Čtení z [fronty Service Bus][lnk-getstarted-queue].</span><span class="sxs-lookup"><span data-stu-id="94446-129">Reading from [Service Bus queues][lnk-getstarted-queue].</span></span>
* <span data-ttu-id="94446-130">Čtení z [témat sběrnice Service Bus][lnk-getstarted-topic].</span><span class="sxs-lookup"><span data-stu-id="94446-130">Reading from [Service Bus topics][lnk-getstarted-topic].</span></span>

### <a name="next-steps"></a><span data-ttu-id="94446-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94446-131">Next steps</span></span>

<span data-ttu-id="94446-132">Další informace o koncové body centra IoT najdete v tématu [koncové body centra IoT][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="94446-132">For more information about IoT Hub endpoints, see [IoT Hub endpoints][lnk-devguide-endpoints].</span></span>

<span data-ttu-id="94446-133">Další informace o dotazovací jazyk používáte k definování pravidel směrování, najdete v části [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv][lnk-devguide-query-language].</span><span class="sxs-lookup"><span data-stu-id="94446-133">For more information about the query language you use to define routing rules, see [IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query-language].</span></span>

<span data-ttu-id="94446-134">[Zpráv typu zařízení cloud proces IoT Hub pomocí trasy] [ lnk-d2c-tutorial] kurzu se dozvíte, jak používat pravidla směrování a vlastní koncové body.</span><span class="sxs-lookup"><span data-stu-id="94446-134">The [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial shows you how to use routing rules and custom endpoints.</span></span>

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
