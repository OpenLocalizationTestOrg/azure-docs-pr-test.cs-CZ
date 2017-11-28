---
title: "aaaLearn toouse hello konektor služby Salesforce ve vašich logic apps | Microsoft Docs"
description: "Vytvoření aplikace logiky službou Azure App service. Hello konektor služby Salesforce poskytuje toowork rozhraní API s objekty služby Salesforce."
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
ms.openlocfilehash: b14b41fa8a4648b4f0090472dc0f9575bf13a2ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-salesforce-connector"></a><span data-ttu-id="9e7ab-104">Začínáme s konektor služby Salesforce hello</span><span class="sxs-lookup"><span data-stu-id="9e7ab-104">Get started with hello Salesforce connector</span></span>
<span data-ttu-id="9e7ab-105">Hello konektor služby Salesforce poskytuje toowork rozhraní API s objekty služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="9e7ab-105">hello Salesforce Connector provides an API toowork with Salesforce objects.</span></span>

<span data-ttu-id="9e7ab-106">toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="9e7ab-106">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="9e7ab-107">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9e7ab-107">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosalesforce-connector"></a><span data-ttu-id="9e7ab-108">Připojit tooSalesforce konektoru</span><span class="sxs-lookup"><span data-stu-id="9e7ab-108">Connect tooSalesforce connector</span></span>
<span data-ttu-id="9e7ab-109">Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby.</span><span class="sxs-lookup"><span data-stu-id="9e7ab-109">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="9e7ab-110">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="9e7ab-110">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-toosalesforce-connector"></a><span data-ttu-id="9e7ab-111">Vytvořit konektor tooSalesforce připojení</span><span class="sxs-lookup"><span data-stu-id="9e7ab-111">Create a connection tooSalesforce connector</span></span>
> [!INCLUDE [Steps toocreate a connection tooSalesforce Connector](../../includes/connectors-create-api-salesforce.md)]
> 
> 

## <a name="use-a-salesforce-connector-trigger"></a><span data-ttu-id="9e7ab-112">Použít aktivační událost konektor služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="9e7ab-112">Use a Salesforce connector trigger</span></span>
<span data-ttu-id="9e7ab-113">Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9e7ab-113">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="9e7ab-114">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="9e7ab-114">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

> [!INCLUDE [Steps toocreate a Salesforce trigger](../../includes/connectors-create-api-salesforce-trigger.md)]
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="9e7ab-115">Přidat podmínku</span><span class="sxs-lookup"><span data-stu-id="9e7ab-115">Add a condition</span></span>
> [!INCLUDE [Steps toocreate a Salesforce condition](../../includes/connectors-create-api-salesforce-condition.md)]
> 
> 

## <a name="use-a-salesforce-connector-action"></a><span data-ttu-id="9e7ab-116">Použít akci konektor služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="9e7ab-116">Use a Salesforce connector action</span></span>
<span data-ttu-id="9e7ab-117">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9e7ab-117">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="9e7ab-118">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="9e7ab-118">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

> [!INCLUDE [Steps toocreate a Salesforce action](../../includes/connectors-create-api-salesforce-action.md)]
> 
> 

## <a name="connector-specific-details"></a><span data-ttu-id="9e7ab-119">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="9e7ab-119">Connector-specific details</span></span>

<span data-ttu-id="9e7ab-120">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/salesforce/).</span><span class="sxs-lookup"><span data-stu-id="9e7ab-120">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/salesforce/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9e7ab-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9e7ab-121">Next steps</span></span>
[<span data-ttu-id="9e7ab-122">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="9e7ab-122">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

