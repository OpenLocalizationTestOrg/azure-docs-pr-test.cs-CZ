---
title: "Přidejte konektor Twilio v Azure Logic apps | Microsoft Docs"
description: "Přehled konektoru Twilio s parametry rozhraní REST API"
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
ms.openlocfilehash: a790ac51b0fea7e3fa379d20e0e094e7ce0d7696
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-twilio-connector"></a><span data-ttu-id="46723-103">Začínáme s konektorem Twilio</span><span class="sxs-lookup"><span data-stu-id="46723-103">Get started with the Twilio connector</span></span>
<span data-ttu-id="46723-104">Připojení k Twiliu odesílat a přijímat globální IP, služby SMS a MMS zpráv.</span><span class="sxs-lookup"><span data-stu-id="46723-104">Connect to Twilio to send and receive global SMS, MMS, and IP messages.</span></span> <span data-ttu-id="46723-105">Twilio můžete:</span><span class="sxs-lookup"><span data-stu-id="46723-105">With Twilio, you can:</span></span>

* <span data-ttu-id="46723-106">Sestavení vaší firmy toku na základě dat, které můžete získat z Twilio.</span><span class="sxs-lookup"><span data-stu-id="46723-106">Build your business flow based on the data you get from Twilio.</span></span> 
* <span data-ttu-id="46723-107">Pomocí akcí, které získají zprávu, seznam zpráv a další.</span><span class="sxs-lookup"><span data-stu-id="46723-107">Use actions that get a message, list messages, and more.</span></span> <span data-ttu-id="46723-108">Tyto akce se odpověď a pak proveďte výstup k dispozici pro další akce.</span><span class="sxs-lookup"><span data-stu-id="46723-108">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="46723-109">Například když získáte novou zprávu Twilio, můžete provést tuto zprávu a jeho použití pracovního postupu služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="46723-109">For example, when  you get a new Twilio message, you can take this message and use it a Service Bus workflow.</span></span> 

<span data-ttu-id="46723-110">Začněte vytvořením aplikace logiky; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="46723-110">Get started by creating a logic app; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-twilio"></a><span data-ttu-id="46723-111">Umožňuje vytvořit připojení k Twiliu</span><span class="sxs-lookup"><span data-stu-id="46723-111">Create a connection to Twilio</span></span>
<span data-ttu-id="46723-112">Když přidáte tento konektor aplikace logiky, zadejte následující hodnoty Twilio:</span><span class="sxs-lookup"><span data-stu-id="46723-112">When you add this Connector to your logic apps, enter the following Twilio values:</span></span>

| <span data-ttu-id="46723-113">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="46723-113">Property</span></span> | <span data-ttu-id="46723-114">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="46723-114">Required</span></span> | <span data-ttu-id="46723-115">Popis</span><span class="sxs-lookup"><span data-stu-id="46723-115">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="46723-116">ID účtu</span><span class="sxs-lookup"><span data-stu-id="46723-116">Account ID</span></span> |<span data-ttu-id="46723-117">Ano</span><span class="sxs-lookup"><span data-stu-id="46723-117">Yes</span></span> |<span data-ttu-id="46723-118">Zadejte své ID účtu Twilio</span><span class="sxs-lookup"><span data-stu-id="46723-118">Enter your Twilio account ID</span></span> |
| <span data-ttu-id="46723-119">Přístupový Token</span><span class="sxs-lookup"><span data-stu-id="46723-119">Access Token</span></span> |<span data-ttu-id="46723-120">Ano</span><span class="sxs-lookup"><span data-stu-id="46723-120">Yes</span></span> |<span data-ttu-id="46723-121">Zadejte přístupový token Twilio</span><span class="sxs-lookup"><span data-stu-id="46723-121">Enter your Twilio access token</span></span> |

> [!INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

<span data-ttu-id="46723-122">Pokud nemáte přístupový token Twilio, přečtěte si téma [identitu uživatele & přístupové tokeny](https://www.twilio.com/docs/api/chat/guides/identity).</span><span class="sxs-lookup"><span data-stu-id="46723-122">If you don't have a Twilio access token, see [User Identity & Access Tokens](https://www.twilio.com/docs/api/chat/guides/identity).</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="46723-123">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="46723-123">Connector-specific details</span></span>

<span data-ttu-id="46723-124">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/twilio/).</span><span class="sxs-lookup"><span data-stu-id="46723-124">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/twilio/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="46723-125">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="46723-125">More connectors</span></span>
<span data-ttu-id="46723-126">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="46723-126">Go back to the [APIs list](apis-list.md).</span></span>