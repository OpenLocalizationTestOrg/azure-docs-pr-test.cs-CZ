---
title: "Přidejte konektor Facebook ve vašich Logic Apps | Microsoft Docs"
description: "Přehled konektoru Facebook s parametry rozhraní REST API"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f4d6f0ed-c09b-488c-be1c-8cf2b5b1d4b8
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: mandia; ladocs
ms.openlocfilehash: e10a30ccef3e81cb3d7749696453d82b8958d076
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-facebook-connector"></a><span data-ttu-id="7c78f-103">Začínáme s konektorem služby Facebook</span><span class="sxs-lookup"><span data-stu-id="7c78f-103">Get started with the Facebook connector</span></span>
<span data-ttu-id="7c78f-104">Připojení k síti Facebook a příspěvků na timeline, získat kanálů stránek a další.</span><span class="sxs-lookup"><span data-stu-id="7c78f-104">Connect to Facebook and post to a timeline, get a page feed, and more.</span></span> <span data-ttu-id="7c78f-105">Facebook můžete:</span><span class="sxs-lookup"><span data-stu-id="7c78f-105">With Facebook, you can:</span></span>

* <span data-ttu-id="7c78f-106">Sestavení vaší firmy toku na základě dat, které máte ze sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="7c78f-106">Build your business flow based on the data you get from Facebook.</span></span> 
* <span data-ttu-id="7c78f-107">Používejte aktivační událost, když je obdržena nového příspěvku.</span><span class="sxs-lookup"><span data-stu-id="7c78f-107">Use a trigger when a new post is received.</span></span>
* <span data-ttu-id="7c78f-108">Použití akce, které odeslání na časové ose získat kanálů stránek a další.</span><span class="sxs-lookup"><span data-stu-id="7c78f-108">Use actions that post to your timeline, get a page feed, and more.</span></span> <span data-ttu-id="7c78f-109">Tyto akce se odpověď a pak proveďte výstup k dispozici pro další akce.</span><span class="sxs-lookup"><span data-stu-id="7c78f-109">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="7c78f-110">Například po nového příspěvku na časové ose můžete provést tento post a push na váš informační kanál sítě Twitter.</span><span class="sxs-lookup"><span data-stu-id="7c78f-110">For example, when there is a new post on your timeline, you can take that post and push it to your Twitter feed.</span></span> 

<span data-ttu-id="7c78f-111">Můžete začít s vytvářením aplikace logiky teď najdete v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="7c78f-111">You can get started by creating a logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-facebook"></a><span data-ttu-id="7c78f-112">Umožňuje vytvořit připojení k síti Facebook</span><span class="sxs-lookup"><span data-stu-id="7c78f-112">Create a connection to Facebook</span></span>
<span data-ttu-id="7c78f-113">Když přidáte tento konektor aplikace logiky, musíte je nejdříve autorizovat logic apps, abyste připojení k vaší službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="7c78f-113">When you add this connector to your logic apps, you must authorize logic apps to connect to your Facebook.</span></span>

1. <span data-ttu-id="7c78f-114">Přihlaste se k účtu služby Facebook</span><span class="sxs-lookup"><span data-stu-id="7c78f-114">Sign in to your Facebook account</span></span>
2. <span data-ttu-id="7c78f-115">Vyberte **Authorize**a povolit logic apps, abyste připojit a používat vaší sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="7c78f-115">Select **Authorize**, and allow your logic apps to connect and use your Facebook.</span></span> 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 


## <a name="connector-specific-details"></a><span data-ttu-id="7c78f-116">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="7c78f-116">Connector-specific details</span></span>

<span data-ttu-id="7c78f-117">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/facebook/).</span><span class="sxs-lookup"><span data-stu-id="7c78f-117">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/facebook/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="7c78f-118">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="7c78f-118">More connectors</span></span>
<span data-ttu-id="7c78f-119">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="7c78f-119">Go back to the [APIs list](apis-list.md).</span></span>