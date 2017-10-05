---
title: Konektor Githubu v Azure Logic Apps | Microsoft Docs
description: "Vytvoření aplikace logiky službou Azure App service. GitHub je hostitelem služby založené na webu úložiště Git. Nabízí všechny distribuované revize řízení a zdrojový kód (SCM) funkce správy Git, jakož i přidání vlastní funkce."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: d4614b0ceff0ec0d36dbb1a136551f985f2fc1a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-github-connector"></a><span data-ttu-id="fbaec-105">Začínáme s konektorem Githubu</span><span class="sxs-lookup"><span data-stu-id="fbaec-105">Get started with the GitHub connector</span></span>
<span data-ttu-id="fbaec-106">GitHub je hostitelem služby založené na webu úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="fbaec-106">GitHub is a web-based Git repository hosting service.</span></span> <span data-ttu-id="fbaec-107">Nabízí všechny distribuované revize řízení a zdrojový kód (SCM) funkce správy Git, jakož i přidání vlastní funkce.</span><span class="sxs-lookup"><span data-stu-id="fbaec-107">It offers all of the distributed revision control and source code management (SCM) functionality of Git as well as adding its own features.</span></span>

<span data-ttu-id="fbaec-108">Můžete začít s vytvářením aplikace logiky teď najdete v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="fbaec-108">You can get started by creating a Logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-github"></a><span data-ttu-id="fbaec-109">Umožňuje vytvořit připojení ke Githubu</span><span class="sxs-lookup"><span data-stu-id="fbaec-109">Create a connection to GitHub</span></span>
<span data-ttu-id="fbaec-110">Postup vytvoření aplikace logiky se Githubu, musíte nejdřív vytvořit **připojení** pak zadejte podrobnosti pro následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="fbaec-110">To create Logic apps with GitHub, you must first create a **connection** then provide the details for the following properties:</span></span> 

| <span data-ttu-id="fbaec-111">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="fbaec-111">Property</span></span> | <span data-ttu-id="fbaec-112">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fbaec-112">Required</span></span> | <span data-ttu-id="fbaec-113">Popis</span><span class="sxs-lookup"><span data-stu-id="fbaec-113">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fbaec-114">Token</span><span class="sxs-lookup"><span data-stu-id="fbaec-114">Token</span></span> |<span data-ttu-id="fbaec-115">Ano</span><span class="sxs-lookup"><span data-stu-id="fbaec-115">Yes</span></span> |<span data-ttu-id="fbaec-116">Zadejte přihlašovací údaje Githubu</span><span class="sxs-lookup"><span data-stu-id="fbaec-116">Provide GitHub Credentials</span></span> |

<span data-ttu-id="fbaec-117">Po vytvoření připojení, můžete ke spouštění akcí a naslouchat aktivační události popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="fbaec-117">After you create the connection, you can use it to execute the actions and listen for the triggers described in this article.</span></span> 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="fbaec-118">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="fbaec-118">Connector-specific details</span></span>

<span data-ttu-id="fbaec-119">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/github/).</span><span class="sxs-lookup"><span data-stu-id="fbaec-119">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/github/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="fbaec-120">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="fbaec-120">More connectors</span></span>
<span data-ttu-id="fbaec-121">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="fbaec-121">Go back to the [APIs list](apis-list.md).</span></span>