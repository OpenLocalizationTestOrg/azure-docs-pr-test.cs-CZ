---
title: "Naučte se používat konektor služby Salesforce ve vašich logic apps | Microsoft Docs"
description: "Vytvoření aplikace logiky službou Azure App service. Konektor Salesforce poskytuje rozhraní API pro práci s objekty Salesforce."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 54fe5af8-7d2a-4da8-94e7-15d029e029bf
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/05/2016
ms.author: mandia; ladocs
ms.openlocfilehash: c2e2efd356382df9404f5c4ed54f24758b2cd22b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-salesforce-connector"></a><span data-ttu-id="12591-104">Začínáme s konektor služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="12591-104">Get started with the Salesforce connector</span></span>
<span data-ttu-id="12591-105">Konektor Salesforce poskytuje rozhraní API pro práci s objekty Salesforce.</span><span class="sxs-lookup"><span data-stu-id="12591-105">The Salesforce Connector provides an API to work with Salesforce objects.</span></span>

<span data-ttu-id="12591-106">Chcete-li použít [všechny konektory](apis-list.md), musíte nejprve vytvořit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="12591-106">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="12591-107">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="12591-107">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-salesforce-connector"></a><span data-ttu-id="12591-108">Připojit ke konektoru služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="12591-108">Connect to Salesforce connector</span></span>
<span data-ttu-id="12591-109">Než se aplikace logiky k jakékoli služby, musíte nejprve vytvořit *připojení* ke službě.</span><span class="sxs-lookup"><span data-stu-id="12591-109">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="12591-110">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="12591-110">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-salesforce-connector"></a><span data-ttu-id="12591-111">Umožňuje vytvořit připojení k konektor služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="12591-111">Create a connection to Salesforce connector</span></span>
> [!INCLUDE [Steps to create a connection to Salesforce Connector](../../includes/connectors-create-api-salesforce.md)]
> 
> 

## <a name="use-a-salesforce-connector-trigger"></a><span data-ttu-id="12591-112">Použít aktivační událost konektor služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="12591-112">Use a Salesforce connector trigger</span></span>
<span data-ttu-id="12591-113">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="12591-113">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="12591-114">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="12591-114">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

> [!INCLUDE [Steps to create a Salesforce trigger](../../includes/connectors-create-api-salesforce-trigger.md)]
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="12591-115">Přidat podmínku</span><span class="sxs-lookup"><span data-stu-id="12591-115">Add a condition</span></span>
> [!INCLUDE [Steps to create a Salesforce condition](../../includes/connectors-create-api-salesforce-condition.md)]
> 
> 

## <a name="use-a-salesforce-connector-action"></a><span data-ttu-id="12591-116">Použít akci konektor služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="12591-116">Use a Salesforce connector action</span></span>
<span data-ttu-id="12591-117">Akce je operace prováděné definované v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="12591-117">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="12591-118">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="12591-118">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

> [!INCLUDE [Steps to create a Salesforce action](../../includes/connectors-create-api-salesforce-action.md)]
> 
> 

## <a name="connector-specific-details"></a><span data-ttu-id="12591-119">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="12591-119">Connector-specific details</span></span>

<span data-ttu-id="12591-120">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/salesforce/).</span><span class="sxs-lookup"><span data-stu-id="12591-120">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/salesforce/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="12591-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12591-121">Next steps</span></span>
[<span data-ttu-id="12591-122">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="12591-122">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

