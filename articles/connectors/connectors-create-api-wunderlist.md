---
title: Konektor Wunderlist i n Azure logic apps | Microsoft Docs
description: "Umožňuje vytvořit připojení k Wunderlist a používají toto připojení k vytvoření pracovního postupu v aplikacích logiky."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: e4773ecf-3ad3-44b4-a1b5-ee5f58baeadd
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 899110992cc52ca5edf1706320fd5570473de784
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-wunderlist-connector"></a><span data-ttu-id="4eb0a-103">Začínáme s konektorem Wunderlist</span><span class="sxs-lookup"><span data-stu-id="4eb0a-103">Get started with the Wunderlist connector</span></span>
<span data-ttu-id="4eb0a-104">Wunderlist zadejte manager todo seznamu a úlohy umožníte uživatelům získat jejich věcem Hotovo.</span><span class="sxs-lookup"><span data-stu-id="4eb0a-104">Wunderlist provide a todo list and task manager to help people get their stuff done.</span></span>  <span data-ttu-id="4eb0a-105">Zda sdílíte s blízkým jeden seznam na nákup, práci na projektu nebo plánování dovolenou, Wunderlist snadno zachycení, sdílet a dokončit vaše to¬dos.</span><span class="sxs-lookup"><span data-stu-id="4eb0a-105">Whether you’re sharing a grocery list with a loved one, working on a project, or planning a vacation, Wunderlist makes it easy to capture, share, and complete your to¬dos.</span></span> <span data-ttu-id="4eb0a-106">Wunderlist se okamžitě synchronizuje mezi telefon, tablet a počítači, všechny úlohy nabízí přístup odkudkoli.</span><span class="sxs-lookup"><span data-stu-id="4eb0a-106">Wunderlist instantly syncs between your phone, tablet and computer, so you can access all your tasks from anywhere.</span></span>

<span data-ttu-id="4eb0a-107">Začněte vytvořením aplikace logiky nyní; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="4eb0a-107">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-wunderlist"></a><span data-ttu-id="4eb0a-108">Umožňuje vytvořit připojení k Wunderlist</span><span class="sxs-lookup"><span data-stu-id="4eb0a-108">Create a connection to Wunderlist</span></span>
<span data-ttu-id="4eb0a-109">Postup vytvoření aplikace logiky se Wunderlist, musíte nejdřív vytvořit **připojení** pak zadejte podrobnosti pro následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="4eb0a-109">To create Logic apps with Wunderlist, you must first create a **connection** then provide the details for the following properties:</span></span>

| <span data-ttu-id="4eb0a-110">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4eb0a-110">Property</span></span> | <span data-ttu-id="4eb0a-111">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="4eb0a-111">Required</span></span> | <span data-ttu-id="4eb0a-112">Popis</span><span class="sxs-lookup"><span data-stu-id="4eb0a-112">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4eb0a-113">Token</span><span class="sxs-lookup"><span data-stu-id="4eb0a-113">Token</span></span> |<span data-ttu-id="4eb0a-114">Ano</span><span class="sxs-lookup"><span data-stu-id="4eb0a-114">Yes</span></span> |<span data-ttu-id="4eb0a-115">Zadejte přihlašovací údaje Wunderlist</span><span class="sxs-lookup"><span data-stu-id="4eb0a-115">Provide Wunderlist Credentials</span></span> |

<span data-ttu-id="4eb0a-116">Po vytvoření připojení, můžete ke spouštění akcí a naslouchat aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="4eb0a-116">After you create the connection, you can use it to execute the actions and listen for the triggers.</span></span>

> [!INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="4eb0a-117">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="4eb0a-117">Connector-specific details</span></span>

<span data-ttu-id="4eb0a-118">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/wunderlist/).</span><span class="sxs-lookup"><span data-stu-id="4eb0a-118">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/wunderlist/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="4eb0a-119">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="4eb0a-119">More connectors</span></span>
<span data-ttu-id="4eb0a-120">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="4eb0a-120">Go back to the [APIs list](apis-list.md).</span></span>