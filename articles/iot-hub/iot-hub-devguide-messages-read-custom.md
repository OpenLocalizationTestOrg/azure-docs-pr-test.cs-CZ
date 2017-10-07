---
title: "Vlastní koncové body Azure IoT Hub aaaUnderstand | Microsoft Docs"
description: "Příručka vývojáře - použití směrování pravidla tooroute zpráv typu zařízení cloud toocustom koncové body."
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
ms.openlocfilehash: daa9cfb35d0853e316bbf469b034d4dadbd4e85d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a><span data-ttu-id="f1393-103">Použít vlastní koncové body a směrování zpráv pro zprávy typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="f1393-103">Use message routes and custom endpoints for device-to-cloud messages</span></span>

<span data-ttu-id="f1393-104">Centrum IoT vám umožní tooroute [zpráv typu zařízení cloud] [ lnk-device-to-cloud] tooIoT centra služby přístupem koncových bodů podle vlastnosti zprávy.</span><span class="sxs-lookup"><span data-stu-id="f1393-104">IoT Hub enables you tooroute [device-to-cloud messages][lnk-device-to-cloud] tooIoT Hub service-facing endpoints based on message properties.</span></span> <span data-ttu-id="f1393-105">Směrování pravidla udělení hello flexibilitu toosend zprávy, které potřebují toogo bez hello potřebovat pro další služby tooprocess zprávy nebo toowrite další kód.</span><span class="sxs-lookup"><span data-stu-id="f1393-105">Routing rules give you hello flexibility toosend messages where they need toogo without hello need for additional services tooprocess messages or toowrite additional code.</span></span> <span data-ttu-id="f1393-106">Každé pravidlo směrování, která můžete konfigurovat má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f1393-106">Each routing rule you configure has hello following properties:</span></span>

| <span data-ttu-id="f1393-107">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f1393-107">Property</span></span>      | <span data-ttu-id="f1393-108">Popis</span><span class="sxs-lookup"><span data-stu-id="f1393-108">Description</span></span> |
| ------------- | ----------- |
| <span data-ttu-id="f1393-109">**Název**</span><span class="sxs-lookup"><span data-stu-id="f1393-109">**Name**</span></span>      | <span data-ttu-id="f1393-110">Hello jedinečný název, který identifikuje pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="f1393-110">hello unique name that identifies hello rule.</span></span> |
| <span data-ttu-id="f1393-111">**Zdroj**</span><span class="sxs-lookup"><span data-stu-id="f1393-111">**Source**</span></span>    | <span data-ttu-id="f1393-112">Hello původu dat hello stream toobe reagovali na ni.</span><span class="sxs-lookup"><span data-stu-id="f1393-112">hello origin of hello data stream toobe acted upon.</span></span> <span data-ttu-id="f1393-113">Například zařízení telemetrie.</span><span class="sxs-lookup"><span data-stu-id="f1393-113">For example, device telemetry.</span></span> |
| <span data-ttu-id="f1393-114">**Podmínka**</span><span class="sxs-lookup"><span data-stu-id="f1393-114">**Condition**</span></span> | <span data-ttu-id="f1393-115">Hello výraz dotazu pro hello směrování pravidlo, které běží na uvítací zprávu hlavičky a text a použít toodetermine, zda je shoda pro koncový bod hello.</span><span class="sxs-lookup"><span data-stu-id="f1393-115">hello query expression for hello routing rule that is run against hello message's headers and body and used toodetermine whether it is a match for hello endpoint.</span></span> <span data-ttu-id="f1393-116">Další informace o vytváření podmínku trasy, najdete v části hello [odkaz - dotazovacího jazyka pro úlohy a dvojčata zařízení][lnk-devguide-query-language].</span><span class="sxs-lookup"><span data-stu-id="f1393-116">For more information about constructing a route condition, see hello [Reference - query language for device twins and jobs][lnk-devguide-query-language].</span></span> |
| <span data-ttu-id="f1393-117">**Koncový bod**</span><span class="sxs-lookup"><span data-stu-id="f1393-117">**Endpoint**</span></span>  | <span data-ttu-id="f1393-118">Název Hello hello koncového bodu, kde IoT Hub odešle zprávy, které vyhovují podmínce hello.</span><span class="sxs-lookup"><span data-stu-id="f1393-118">hello name of hello endpoint where IoT Hub sends messages that match hello condition.</span></span> <span data-ttu-id="f1393-119">Koncové body musí být v hello stejné oblasti jako hello IoT hub, jinak může podléhat pro zapíše mezi oblastmi.</span><span class="sxs-lookup"><span data-stu-id="f1393-119">Endpoints should be in hello same region as hello IoT hub, otherwise you may be charged for cross-region writes.</span></span> |

<span data-ttu-id="f1393-120">Do jedné zprávy může odpovídat na více pravidel směrování, ve kterých případ IoT Hub zajišťuje hello toohello koncový bod zprávy spojené s každou odpovídající pravidlo podmínku hello.</span><span class="sxs-lookup"><span data-stu-id="f1393-120">A single message may match hello condition on multiple routing rules, in which case IoT Hub delivers hello message toohello endpoint associated with each matched rule.</span></span> <span data-ttu-id="f1393-121">Centrum IoT také automaticky deduplicates doručení zpráv, takže pokud zpráva odpovídá více pravidel, které mají hello stejný cíl pouze zapsané toothat cílové jednou.</span><span class="sxs-lookup"><span data-stu-id="f1393-121">IoT Hub also automatically deduplicates message delivery, so if a message matches multiple rules that all have hello same destination, it is only written toothat destination once.</span></span>

<span data-ttu-id="f1393-122">Centrum IoT má výchozí [vestavěným koncovým bodem][lnk-built-in].</span><span class="sxs-lookup"><span data-stu-id="f1393-122">An IoT hub has a default [built-in endpoint][lnk-built-in].</span></span> <span data-ttu-id="f1393-123">Můžete vytvořit vlastní koncové body tooroute zprávy tooby propojování s jinými službami v centru toohello předplatné.</span><span class="sxs-lookup"><span data-stu-id="f1393-123">You can create custom endpoints tooroute messages tooby linking other services in your subscription toohello hub.</span></span> <span data-ttu-id="f1393-124">IoT Hub aktuálně podporuje Event Hubs, fronty Service Bus a témat sběrnice Service Bus jako vlastní koncové body.</span><span class="sxs-lookup"><span data-stu-id="f1393-124">IoT Hub currently supports Event Hubs, Service Bus queues, and Service Bus topics as custom endpoints.</span></span>

> [!WARNING]
> <span data-ttu-id="f1393-125">Fronty sběrnice a témata s **relací** nebo **duplicitní detekce** povoleno nejsou podporovány jako vlastní koncové body.</span><span class="sxs-lookup"><span data-stu-id="f1393-125">Service Bus queues and topics with **Sessions** or **Duplicate Detection** enabled are not supported as custom endpoints.</span></span>

<span data-ttu-id="f1393-126">Další informace o vytváření vlastní koncové body v centru IoT najdete v tématu [koncové body centra IoT][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="f1393-126">For more information about creating custom endpoints in IoT Hub, see [IoT Hub endpoints][lnk-devguide-endpoints].</span></span>

<span data-ttu-id="f1393-127">Další informace o čtení ze vlastní koncové body najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="f1393-127">For more information about reading from custom endpoints, see:</span></span>

* <span data-ttu-id="f1393-128">Čtení z [služby Event Hubs][lnk-getstarted-eh].</span><span class="sxs-lookup"><span data-stu-id="f1393-128">Reading from [Event Hubs][lnk-getstarted-eh].</span></span>
* <span data-ttu-id="f1393-129">Čtení z [fronty Service Bus][lnk-getstarted-queue].</span><span class="sxs-lookup"><span data-stu-id="f1393-129">Reading from [Service Bus queues][lnk-getstarted-queue].</span></span>
* <span data-ttu-id="f1393-130">Čtení z [témat sběrnice Service Bus][lnk-getstarted-topic].</span><span class="sxs-lookup"><span data-stu-id="f1393-130">Reading from [Service Bus topics][lnk-getstarted-topic].</span></span>

### <a name="next-steps"></a><span data-ttu-id="f1393-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1393-131">Next steps</span></span>

<span data-ttu-id="f1393-132">Další informace o koncové body centra IoT najdete v tématu [koncové body centra IoT][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="f1393-132">For more information about IoT Hub endpoints, see [IoT Hub endpoints][lnk-devguide-endpoints].</span></span>

<span data-ttu-id="f1393-133">Další informace o hello dotazovací jazyk použít pravidla směrování toodefine, najdete v části [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv][lnk-devguide-query-language].</span><span class="sxs-lookup"><span data-stu-id="f1393-133">For more information about hello query language you use toodefine routing rules, see [IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query-language].</span></span>

<span data-ttu-id="f1393-134">Hello [zpráv typu zařízení cloud proces IoT Hub pomocí trasy] [ lnk-d2c-tutorial] kurz ukazuje, jak směrování toouse pravidla a vlastní koncové body.</span><span class="sxs-lookup"><span data-stu-id="f1393-134">hello [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial shows you how toouse routing rules and custom endpoints.</span></span>

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
