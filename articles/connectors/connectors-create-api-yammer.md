---
title: "Přidejte konektor Yammer v Azure Logic Apps | Microsoft Docs"
description: "Přehled konektoru Yammer s parametry rozhraní REST API"
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
ms.openlocfilehash: c7a213343b4fb2b5a89a5052a459061b404a431c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-yammer-connector"></a><span data-ttu-id="cdb08-103">Začínáme s konektorem Yammer</span><span class="sxs-lookup"><span data-stu-id="cdb08-103">Get started with the Yammer connector</span></span>
<span data-ttu-id="cdb08-104">Připojení k Yammeru k přístupu konverzací v podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="cdb08-104">Connect to Yammer to access conversations in your enterprise network.</span></span> <span data-ttu-id="cdb08-105">Yammer můžete:</span><span class="sxs-lookup"><span data-stu-id="cdb08-105">With Yammer, you can:</span></span>

* <span data-ttu-id="cdb08-106">Sestavení vaší firmy toku na základě dat, které můžete získat z Yammer.</span><span class="sxs-lookup"><span data-stu-id="cdb08-106">Build your business flow based on the data you get from Yammer.</span></span> 
* <span data-ttu-id="cdb08-107">Použití spustí pro, když není vaší následující nové zprávy skupinu nebo informačního kanálu.</span><span class="sxs-lookup"><span data-stu-id="cdb08-107">Use triggers for when there is a new message in a group, or a feed your following.</span></span>
* <span data-ttu-id="cdb08-108">Chcete-li vytvořit zprávu, získáte všechny zprávy a další pomocí akcí.</span><span class="sxs-lookup"><span data-stu-id="cdb08-108">Use actions to post a message, get all messages, and more.</span></span> <span data-ttu-id="cdb08-109">Tyto akce se odpověď a pak proveďte výstup k dispozici pro další akce.</span><span class="sxs-lookup"><span data-stu-id="cdb08-109">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="cdb08-110">Když se objeví nová zpráva, můžete odeslat e-mailu pomocí Office 365.</span><span class="sxs-lookup"><span data-stu-id="cdb08-110">For example, when a new message appears, you can send an email using Office 365.</span></span>

<span data-ttu-id="cdb08-111">Začněte vytvořením aplikace logiky nyní; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="cdb08-111">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-yammer"></a><span data-ttu-id="cdb08-112">Umožňuje vytvořit připojení k Yammeru</span><span class="sxs-lookup"><span data-stu-id="cdb08-112">Create a connection to Yammer</span></span>
<span data-ttu-id="cdb08-113">K používání konektoru Yammer, je třeba nejprve vytvořit **připojení** pak zadejte podrobnosti pro tyto vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="cdb08-113">To use the Yammer connector, you first create a **connection** then provide the details for these properties:</span></span> 

| <span data-ttu-id="cdb08-114">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="cdb08-114">Property</span></span> | <span data-ttu-id="cdb08-115">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="cdb08-115">Required</span></span> | <span data-ttu-id="cdb08-116">Popis</span><span class="sxs-lookup"><span data-stu-id="cdb08-116">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cdb08-117">Token</span><span class="sxs-lookup"><span data-stu-id="cdb08-117">Token</span></span> |<span data-ttu-id="cdb08-118">Ano</span><span class="sxs-lookup"><span data-stu-id="cdb08-118">Yes</span></span> |<span data-ttu-id="cdb08-119">Zadejte přihlašovací údaje Yammer</span><span class="sxs-lookup"><span data-stu-id="cdb08-119">Provide Yammer Credentials</span></span> |

> [!INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="cdb08-120">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="cdb08-120">Connector-specific details</span></span>

<span data-ttu-id="cdb08-121">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/yammer/).</span><span class="sxs-lookup"><span data-stu-id="cdb08-121">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/yammer/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="cdb08-122">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="cdb08-122">More connectors</span></span>
<span data-ttu-id="cdb08-123">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="cdb08-123">Go back to the [APIs list](apis-list.md).</span></span>