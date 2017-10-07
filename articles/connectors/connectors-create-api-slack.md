---
title: aaaUse hello Slack konektor v Azure logic apps | Microsoft Docs
description: "Připojit tooSlack ve vašich logic apps"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 6599d7b69d2147425c9fab978c5d0f93e5605f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-slack-connector"></a><span data-ttu-id="46a81-103">Začínáme s konektorem Slack hello</span><span class="sxs-lookup"><span data-stu-id="46a81-103">Get started with hello Slack connector</span></span>
<span data-ttu-id="46a81-104">Slack je nástroj komunikace týmu, který spojuje všechny team komunikace v jednom umístění, okamžitě vyhledávat a bez ohledu na přejdete k dispozici.</span><span class="sxs-lookup"><span data-stu-id="46a81-104">Slack is a team communication tool, that brings together all of your team communications in one place, instantly searchable and available wherever you go.</span></span> 

<span data-ttu-id="46a81-105">Začněte vytvořením aplikace logiky nyní; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="46a81-105">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-tooslack"></a><span data-ttu-id="46a81-106">Vytvoření připojení tooSlack</span><span class="sxs-lookup"><span data-stu-id="46a81-106">Create a connection tooSlack</span></span>
<span data-ttu-id="46a81-107">toouse hello Slack konektor, nejprve vytvoříte **připojení** pak zadejte hello podrobnosti pro tyto vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="46a81-107">toouse hello Slack connector, you first create a **connection** then provide hello details for these properties:</span></span> 

| <span data-ttu-id="46a81-108">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="46a81-108">Property</span></span> | <span data-ttu-id="46a81-109">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="46a81-109">Required</span></span> | <span data-ttu-id="46a81-110">Popis</span><span class="sxs-lookup"><span data-stu-id="46a81-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="46a81-111">Token</span><span class="sxs-lookup"><span data-stu-id="46a81-111">Token</span></span> |<span data-ttu-id="46a81-112">Ano</span><span class="sxs-lookup"><span data-stu-id="46a81-112">Yes</span></span> |<span data-ttu-id="46a81-113">Zadejte přihlašovací údaje Slack</span><span class="sxs-lookup"><span data-stu-id="46a81-113">Provide Slack Credentials</span></span> |

<span data-ttu-id="46a81-114">Postupujte podle těchto kroků toosign do systému Slack a dokončení hello konfiguraci hello Slack **připojení** v aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="46a81-114">Follow these steps toosign into Slack, and complete hello configuration of hello Slack **connection** in your logic app:</span></span>

1. <span data-ttu-id="46a81-115">Vyberte **opakování**</span><span class="sxs-lookup"><span data-stu-id="46a81-115">Select **Recurrence**</span></span>
2. <span data-ttu-id="46a81-116">Vyberte **frekvence** a zadejte **intervalu**</span><span class="sxs-lookup"><span data-stu-id="46a81-116">Select a **Frequency** and enter an **Interval**</span></span>
3. <span data-ttu-id="46a81-117">Vyberte **přidat akci**</span><span class="sxs-lookup"><span data-stu-id="46a81-117">Select **Add an action**</span></span>  
   <span data-ttu-id="46a81-118">![Konfigurace systému Slack][1]</span><span class="sxs-lookup"><span data-stu-id="46a81-118">![Configure Slack][1]</span></span>  
4. <span data-ttu-id="46a81-119">Zadejte Slack hello vyhledávacího pole a počkejte hello vyhledávání tooreturn všech položek s Slack v názvu hello</span><span class="sxs-lookup"><span data-stu-id="46a81-119">Enter Slack in hello search box and wait for hello search tooreturn all entries with Slack in hello name</span></span>
5. <span data-ttu-id="46a81-120">Vyberte **Slack - Post zpráv**</span><span class="sxs-lookup"><span data-stu-id="46a81-120">Select **Slack - Post message**</span></span>
6. <span data-ttu-id="46a81-121">Vyberte **přihlášení tooSlack**:</span><span class="sxs-lookup"><span data-stu-id="46a81-121">Select **Sign in tooSlack**:</span></span>  
   <span data-ttu-id="46a81-122">![Konfigurace systému Slack][2]</span><span class="sxs-lookup"><span data-stu-id="46a81-122">![Configure Slack][2]</span></span>
7. <span data-ttu-id="46a81-123">Zadejte vaše toosign Slack přihlašovací údaje v aplikaci tooauthorize hello</span><span class="sxs-lookup"><span data-stu-id="46a81-123">Provide your Slack credentials toosign in tooauthorize hello  application</span></span>    
   ![Konfigurace systému Slack][3]  
8. <span data-ttu-id="46a81-125">Budete přesměrovaného tooyour organizace přihlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="46a81-125">You'll be redirected tooyour organization's Log in page.</span></span> <span data-ttu-id="46a81-126">**Autorizovat** Slack toointeract s svou aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="46a81-126">**Authorize** Slack toointeract with your logic app:</span></span>      
   <span data-ttu-id="46a81-127">![Konfigurace systému Slack][5]</span><span class="sxs-lookup"><span data-stu-id="46a81-127">![Configure Slack][5]</span></span> 
9. <span data-ttu-id="46a81-128">Po dokončení autorizace hello budete přesměrovaného tooyour logiku aplikace toocomplete ji tak, že nakonfigurujete hello **Slack – získat všechny zprávy** části.</span><span class="sxs-lookup"><span data-stu-id="46a81-128">After hello authorization completes you'll be redirected tooyour logic app toocomplete it by configuring hello **Slack - Get all messages** section.</span></span> <span data-ttu-id="46a81-129">Přidejte další aktivační události a akce, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="46a81-129">Add other triggers and actions that you need.</span></span>  
   <span data-ttu-id="46a81-130">![Konfigurace systému Slack][6]</span><span class="sxs-lookup"><span data-stu-id="46a81-130">![Configure Slack][6]</span></span>
10. <span data-ttu-id="46a81-131">Uložte si práci, výběrem **Uložit** v řádku nabídek hello výše.</span><span class="sxs-lookup"><span data-stu-id="46a81-131">Save your work by selecting **Save** on hello menu bar above.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="46a81-132">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="46a81-132">Connector-specific details</span></span>

<span data-ttu-id="46a81-133">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/slack/).</span><span class="sxs-lookup"><span data-stu-id="46a81-133">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/slack/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="46a81-134">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="46a81-134">More connectors</span></span>
<span data-ttu-id="46a81-135">Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="46a81-135">Go back toohello [APIs list](apis-list.md).</span></span>

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
