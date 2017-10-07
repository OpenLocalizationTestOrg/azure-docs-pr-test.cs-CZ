---
title: aaaAdd hello Yammer konektor v Azure Logic Apps | Microsoft Docs
description: "Přehled hello Yammer konektor s parametry rozhraní REST API"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5ae0827-fbb3-45ec-8f45-ad1cc2e7eccc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: be75df770a8062d839926dff8c0195d2647f78d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-yammer-connector"></a><span data-ttu-id="ce956-103">Začínáme s konektorem Yammer hello</span><span class="sxs-lookup"><span data-stu-id="ce956-103">Get started with hello Yammer connector</span></span>
<span data-ttu-id="ce956-104">Připojte tooYammer tooaccess konverzací v podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="ce956-104">Connect tooYammer tooaccess conversations in your enterprise network.</span></span> <span data-ttu-id="ce956-105">Yammer můžete:</span><span class="sxs-lookup"><span data-stu-id="ce956-105">With Yammer, you can:</span></span>

* <span data-ttu-id="ce956-106">Sestavení vaší firmy toku na základě hello dat, které můžete získat z Yammer.</span><span class="sxs-lookup"><span data-stu-id="ce956-106">Build your business flow based on hello data you get from Yammer.</span></span> 
* <span data-ttu-id="ce956-107">Použití spustí pro, když není vaší následující nové zprávy skupinu nebo informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="ce956-107">Use triggers for when there is a new message in a group, or a feed your following.</span></span>
* <span data-ttu-id="ce956-108">Pomocí akce toopost zprávu, získáte všechny zprávy a další.</span><span class="sxs-lookup"><span data-stu-id="ce956-108">Use actions toopost a message, get all messages, and more.</span></span> <span data-ttu-id="ce956-109">Tyto akce se odpověď a pak proveďte výstup hello k dispozici pro další akce.</span><span class="sxs-lookup"><span data-stu-id="ce956-109">These actions get a response, and then make hello output available for other actions.</span></span> <span data-ttu-id="ce956-110">Když se objeví nová zpráva, můžete odeslat e-mailu pomocí Office 365.</span><span class="sxs-lookup"><span data-stu-id="ce956-110">For example, when a new message appears, you can send an email using Office 365.</span></span>

<span data-ttu-id="ce956-111">Začněte vytvořením aplikace logiky nyní; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="ce956-111">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-tooyammer"></a><span data-ttu-id="ce956-112">Vytvoření připojení tooYammer</span><span class="sxs-lookup"><span data-stu-id="ce956-112">Create a connection tooYammer</span></span>
<span data-ttu-id="ce956-113">toouse hello Yammer konektor, nejprve vytvoříte **připojení** pak zadejte hello podrobnosti pro tyto vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="ce956-113">toouse hello Yammer connector, you first create a **connection** then provide hello details for these properties:</span></span> 

| <span data-ttu-id="ce956-114">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ce956-114">Property</span></span> | <span data-ttu-id="ce956-115">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ce956-115">Required</span></span> | <span data-ttu-id="ce956-116">Popis</span><span class="sxs-lookup"><span data-stu-id="ce956-116">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ce956-117">Token</span><span class="sxs-lookup"><span data-stu-id="ce956-117">Token</span></span> |<span data-ttu-id="ce956-118">Ano</span><span class="sxs-lookup"><span data-stu-id="ce956-118">Yes</span></span> |<span data-ttu-id="ce956-119">Zadejte přihlašovací údaje Yammer</span><span class="sxs-lookup"><span data-stu-id="ce956-119">Provide Yammer Credentials</span></span> |

> [!INCLUDE [Steps toocreate a connection tooYammer](../../includes/connectors-create-api-yammer.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="ce956-120">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="ce956-120">Connector-specific details</span></span>

<span data-ttu-id="ce956-121">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/yammer/).</span><span class="sxs-lookup"><span data-stu-id="ce956-121">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/yammer/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="ce956-122">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="ce956-122">More connectors</span></span>
<span data-ttu-id="ce956-123">Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="ce956-123">Go back toohello [APIs list](apis-list.md).</span></span>
