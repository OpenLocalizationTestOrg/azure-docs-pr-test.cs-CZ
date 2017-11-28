---
title: aaaLearn jak toouse hello SharePoint Online konektoru na konzole aplikace logiky | Microsoft Docs
description: "Vytvoření aplikace logiky s konektorem služby SharePoint Online hello toomange seznamy na webu služby SharePoint."
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
ms.openlocfilehash: fc2f2745f0d174eae6165e84fd8b6656b0aac44b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sharepoint-online-connector"></a><span data-ttu-id="61052-103">Začínáme s konektorem služby SharePoint Online hello</span><span class="sxs-lookup"><span data-stu-id="61052-103">Get started with hello SharePoint Online connector</span></span>
<span data-ttu-id="61052-104">Použijte hello SharePoint Online konektor toomanage, které jsou uvedené služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="61052-104">Use hello SharePoint Online connector toomanage SharePoint lists.</span></span>  

<span data-ttu-id="61052-105">toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="61052-105">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="61052-106">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="61052-106">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosharepoint-online"></a><span data-ttu-id="61052-107">Připojit tooSharePoint Online</span><span class="sxs-lookup"><span data-stu-id="61052-107">Connect tooSharePoint Online</span></span>
<span data-ttu-id="61052-108">Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby.</span><span class="sxs-lookup"><span data-stu-id="61052-108">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="61052-109">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="61052-109">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-toosharepoint-online"></a><span data-ttu-id="61052-110">Vytvoření připojení tooSharePoint Online</span><span class="sxs-lookup"><span data-stu-id="61052-110">Create a connection tooSharePoint Online</span></span>
> [!INCLUDE [Steps toocreate a connection tooSharePoint](../../includes/connectors-create-api-sharepointonline.md)]


## <a name="use-a-sharepoint-online-trigger"></a><span data-ttu-id="61052-111">Použít aktivační události služby SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="61052-111">Use a SharePoint Online trigger</span></span>
<span data-ttu-id="61052-112">Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="61052-112">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="61052-113">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="61052-113">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate a SharePoint Online trigger](../../includes/connectors-create-api-sharepointonline-trigger.md)]


## <a name="use-a-sharepoint-online-action"></a><span data-ttu-id="61052-114">Pomocí služby SharePoint Online akce</span><span class="sxs-lookup"><span data-stu-id="61052-114">Use a SharePoint Online action</span></span>
<span data-ttu-id="61052-115">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="61052-115">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="61052-116">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="61052-116">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate a SharePoint Online action](../../includes/connectors-create-api-sharepointonline-action.md)]


## <a name="connector-specific-details"></a><span data-ttu-id="61052-117">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="61052-117">Connector-specific details</span></span>

<span data-ttu-id="61052-118">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="61052-118">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sharepoint/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="61052-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61052-119">Next Steps</span></span>
[<span data-ttu-id="61052-120">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="61052-120">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

