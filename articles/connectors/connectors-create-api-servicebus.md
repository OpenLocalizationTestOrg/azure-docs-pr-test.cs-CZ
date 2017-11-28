---
title: "aaaLearn toouse hello Azure Service Bus konektor ve vašich logic apps | Microsoft Docs"
description: "Vytvoření aplikace logiky službou Azure App service. Připojit toosend tooAzure Service Bus a přijímat zprávy. Můžete provedení akcí, například odeslání tooqueue, odeslání tootopic, přijímání z fronty a přijímat z předplatného."
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
ms.openlocfilehash: c815ac167c3106ade470ce139d119085558a9497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-azure-service-bus-connector"></a><span data-ttu-id="a8466-105">Začínáme s Azure Service Bus konektor hello</span><span class="sxs-lookup"><span data-stu-id="a8466-105">Get started with hello Azure Service Bus connector</span></span>
<span data-ttu-id="a8466-106">Připojit toosend tooAzure Service Bus a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="a8466-106">Connect tooAzure Service Bus toosend and receive messages.</span></span> <span data-ttu-id="a8466-107">Můžete provedení akcí, například odeslání tooqueue, odeslání tootopic, přijímání z fronty a přijímat z předplatného.</span><span class="sxs-lookup"><span data-stu-id="a8466-107">You can perform actions such as send tooqueue, send tootopic, receive from queue, and receive from subscription.</span></span>

<span data-ttu-id="a8466-108">toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="a8466-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="a8466-109">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="a8466-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooservice-bus"></a><span data-ttu-id="a8466-110">Připojit tooService sběrnice</span><span class="sxs-lookup"><span data-stu-id="a8466-110">Connect tooService Bus</span></span>
<span data-ttu-id="a8466-111">Než se aplikace logiky k jakékoli služby, je nutné nejprve toocreate služby toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="a8466-111">Before your logic app can access any service, you first need toocreate a connection toohello service.</span></span> <span data-ttu-id="a8466-112">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="a8466-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

> [!INCLUDE [Steps toocreate a connection tooAzure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a><span data-ttu-id="a8466-113">Použít aktivační událost Service Bus</span><span class="sxs-lookup"><span data-stu-id="a8466-113">Use a Service Bus trigger</span></span>
<span data-ttu-id="a8466-114">Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="a8466-114">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="a8466-115">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="a8466-115">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a><span data-ttu-id="a8466-116">Pomocí akce sběrnice</span><span class="sxs-lookup"><span data-stu-id="a8466-116">Use a Service Bus action</span></span>
<span data-ttu-id="a8466-117">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="a8466-117">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="a8466-118">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="a8466-118">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

[!INCLUDE [Steps toocreate a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="connector-specific-details"></a><span data-ttu-id="a8466-119">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="a8466-119">Connector-specific details</span></span>

<span data-ttu-id="a8466-120">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/servicebus/).</span><span class="sxs-lookup"><span data-stu-id="a8466-120">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/servicebus/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a8466-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a8466-121">Next steps</span></span>
<span data-ttu-id="a8466-122">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="a8466-122">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

