---
title: aaaAdd hello Twilio konektor v Azure Logic apps | Microsoft Docs
description: "Přehled hello Twilio konektor s parametry rozhraní REST API"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: b2b487f34bc76bee24b4237a71ee767d0d22ff7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-twilio-connector"></a><span data-ttu-id="1c761-103">Začínáme s konektorem Twilio hello</span><span class="sxs-lookup"><span data-stu-id="1c761-103">Get started with hello Twilio connector</span></span>
<span data-ttu-id="1c761-104">Připojit tooTwilio toosend a přijímat globální IP, služby SMS a MMS zprávy.</span><span class="sxs-lookup"><span data-stu-id="1c761-104">Connect tooTwilio toosend and receive global SMS, MMS, and IP messages.</span></span> <span data-ttu-id="1c761-105">Twilio můžete:</span><span class="sxs-lookup"><span data-stu-id="1c761-105">With Twilio, you can:</span></span>

* <span data-ttu-id="1c761-106">Sestavení vaší firmy toku na základě hello dat, které můžete získat z Twilio.</span><span class="sxs-lookup"><span data-stu-id="1c761-106">Build your business flow based on hello data you get from Twilio.</span></span> 
* <span data-ttu-id="1c761-107">Pomocí akcí, které získají zprávu, seznam zpráv a další.</span><span class="sxs-lookup"><span data-stu-id="1c761-107">Use actions that get a message, list messages, and more.</span></span> <span data-ttu-id="1c761-108">Tyto akce se odpověď a pak proveďte výstup hello k dispozici pro další akce.</span><span class="sxs-lookup"><span data-stu-id="1c761-108">These actions get a response, and then make hello output available for other actions.</span></span> <span data-ttu-id="1c761-109">Například když získáte novou zprávu Twilio, můžete provést tuto zprávu a jeho použití pracovního postupu služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1c761-109">For example, when  you get a new Twilio message, you can take this message and use it a Service Bus workflow.</span></span> 

<span data-ttu-id="1c761-110">Začněte vytvořením aplikace logiky; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1c761-110">Get started by creating a logic app; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-tootwilio"></a><span data-ttu-id="1c761-111">Vytvoření připojení tooTwilio</span><span class="sxs-lookup"><span data-stu-id="1c761-111">Create a connection tooTwilio</span></span>
<span data-ttu-id="1c761-112">Když přidáte tento konektor tooyour logiku aplikace, zadejte následující hodnoty Twilio hello:</span><span class="sxs-lookup"><span data-stu-id="1c761-112">When you add this Connector tooyour logic apps, enter hello following Twilio values:</span></span>

| <span data-ttu-id="1c761-113">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="1c761-113">Property</span></span> | <span data-ttu-id="1c761-114">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="1c761-114">Required</span></span> | <span data-ttu-id="1c761-115">Popis</span><span class="sxs-lookup"><span data-stu-id="1c761-115">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1c761-116">ID účtu</span><span class="sxs-lookup"><span data-stu-id="1c761-116">Account ID</span></span> |<span data-ttu-id="1c761-117">Ano</span><span class="sxs-lookup"><span data-stu-id="1c761-117">Yes</span></span> |<span data-ttu-id="1c761-118">Zadejte své ID účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="1c761-118">Enter your Twilio account ID</span></span> |
| <span data-ttu-id="1c761-119">Přístupový Token</span><span class="sxs-lookup"><span data-stu-id="1c761-119">Access Token</span></span> |<span data-ttu-id="1c761-120">Ano</span><span class="sxs-lookup"><span data-stu-id="1c761-120">Yes</span></span> |<span data-ttu-id="1c761-121">Zadejte přístupový token Twilio</span><span class="sxs-lookup"><span data-stu-id="1c761-121">Enter your Twilio access token</span></span> |

> [!INCLUDE [Steps toocreate a connection tooTwilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

<span data-ttu-id="1c761-122">Pokud nemáte přístupový token Twilio, přečtěte si téma [identitu uživatele & přístupové tokeny](https://www.twilio.com/docs/api/chat/guides/identity).</span><span class="sxs-lookup"><span data-stu-id="1c761-122">If you don't have a Twilio access token, see [User Identity & Access Tokens](https://www.twilio.com/docs/api/chat/guides/identity).</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="1c761-123">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="1c761-123">Connector-specific details</span></span>

<span data-ttu-id="1c761-124">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/twilio/).</span><span class="sxs-lookup"><span data-stu-id="1c761-124">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/twilio/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="1c761-125">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="1c761-125">More connectors</span></span>
<span data-ttu-id="1c761-126">Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="1c761-126">Go back toohello [APIs list](apis-list.md).</span></span>
