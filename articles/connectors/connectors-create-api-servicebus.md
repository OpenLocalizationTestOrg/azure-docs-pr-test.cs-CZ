---
title: "Naučte se používat konektor Azure Service Bus ve vašich logic apps | Microsoft Docs"
description: "Vytvoření aplikace logiky službou Azure App service. Připojení k Azure Service Bus odesílat a přijímat zprávy. Můžete provést akce, například odeslání do fronty, odesílat do tématu, přijímání z fronty a přijímat z předplatného."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/02/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1e2ce06f5993280dbdb67121849591e67f7979e9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-azure-service-bus-connector"></a><span data-ttu-id="c8912-105">Začínáme s Azure Service Bus konektoru</span><span class="sxs-lookup"><span data-stu-id="c8912-105">Get started with the Azure Service Bus connector</span></span>
<span data-ttu-id="c8912-106">Připojení k Azure Service Bus odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="c8912-106">Connect to Azure Service Bus to send and receive messages.</span></span> <span data-ttu-id="c8912-107">Můžete provést akce, například odeslání do fronty, odesílat do tématu, přijímání z fronty a přijímat z předplatného.</span><span class="sxs-lookup"><span data-stu-id="c8912-107">You can perform actions such as send to queue, send to topic, receive from queue, and receive from subscription.</span></span>

<span data-ttu-id="c8912-108">Chcete-li použít [všechny konektory](apis-list.md), musíte nejprve vytvořit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="c8912-108">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="c8912-109">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c8912-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-service-bus"></a><span data-ttu-id="c8912-110">Připojení k Service Bus</span><span class="sxs-lookup"><span data-stu-id="c8912-110">Connect to Service Bus</span></span>
<span data-ttu-id="c8912-111">Než se aplikace logiky k jakékoli služby, musíte nejprve vytvořit připojení ke službě.</span><span class="sxs-lookup"><span data-stu-id="c8912-111">Before your logic app can access any service, you first need to create a connection to the service.</span></span> <span data-ttu-id="c8912-112">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="c8912-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

> [!INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a><span data-ttu-id="c8912-113">Použít aktivační událost Service Bus</span><span class="sxs-lookup"><span data-stu-id="c8912-113">Use a Service Bus trigger</span></span>
<span data-ttu-id="c8912-114">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="c8912-114">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="c8912-115">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="c8912-115">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a><span data-ttu-id="c8912-116">Pomocí akce sběrnice</span><span class="sxs-lookup"><span data-stu-id="c8912-116">Use a Service Bus action</span></span>
<span data-ttu-id="c8912-117">Akce je operace prováděné definované v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="c8912-117">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="c8912-118">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="c8912-118">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

[!INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="connector-specific-details"></a><span data-ttu-id="c8912-119">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="c8912-119">Connector-specific details</span></span>

<span data-ttu-id="c8912-120">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/servicebus/).</span><span class="sxs-lookup"><span data-stu-id="c8912-120">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/servicebus/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="c8912-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8912-121">Next steps</span></span>
<span data-ttu-id="c8912-122">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c8912-122">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

