---
title: "Naučte se používat konektor služby SharePoint Online v aplikace logiky | Microsoft Docs"
description: "Vytvoření aplikace logiky s konektorem služby SharePoint Online ke správě seznamů na webu služby SharePoint."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: e0ec3149-507a-409d-8e7b-d5fbded006ce
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 96fc1347604c0c6cc2c2463a5dbd83b560183a16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-sharepoint-online-connector"></a><span data-ttu-id="80178-103">Začínáme s konektorem služby SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="80178-103">Get started with the SharePoint Online connector</span></span>
<span data-ttu-id="80178-104">Konektor služby SharePoint Online použijte ke správě seznamy služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="80178-104">Use the SharePoint Online connector to manage SharePoint lists.</span></span>  

<span data-ttu-id="80178-105">Chcete-li použít [všechny konektory](apis-list.md), musíte nejprve vytvořit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="80178-105">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="80178-106">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="80178-106">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-sharepoint-online"></a><span data-ttu-id="80178-107">Připojení do služby SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="80178-107">Connect to SharePoint Online</span></span>
<span data-ttu-id="80178-108">Než se aplikace logiky k jakékoli služby, musíte nejprve vytvořit *připojení* ke službě.</span><span class="sxs-lookup"><span data-stu-id="80178-108">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="80178-109">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="80178-109">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-sharepoint-online"></a><span data-ttu-id="80178-110">Umožňuje vytvořit připojení ke službě SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="80178-110">Create a connection to SharePoint Online</span></span>
> [!INCLUDE [Steps to create a connection to SharePoint](../../includes/connectors-create-api-sharepointonline.md)]


## <a name="use-a-sharepoint-online-trigger"></a><span data-ttu-id="80178-111">Použít aktivační události služby SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="80178-111">Use a SharePoint Online trigger</span></span>
<span data-ttu-id="80178-112">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="80178-112">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="80178-113">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="80178-113">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps to create a SharePoint Online trigger](../../includes/connectors-create-api-sharepointonline-trigger.md)]


## <a name="use-a-sharepoint-online-action"></a><span data-ttu-id="80178-114">Pomocí služby SharePoint Online akce</span><span class="sxs-lookup"><span data-stu-id="80178-114">Use a SharePoint Online action</span></span>
<span data-ttu-id="80178-115">Akce je operace prováděné definované v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="80178-115">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="80178-116">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="80178-116">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps to create a SharePoint Online action](../../includes/connectors-create-api-sharepointonline-action.md)]


## <a name="connector-specific-details"></a><span data-ttu-id="80178-117">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="80178-117">Connector-specific details</span></span>

<span data-ttu-id="80178-118">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="80178-118">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/sharepoint/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="80178-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80178-119">Next Steps</span></span>
[<span data-ttu-id="80178-120">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="80178-120">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

