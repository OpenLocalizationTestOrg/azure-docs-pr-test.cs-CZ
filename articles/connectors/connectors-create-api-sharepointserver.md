---
title: "aaaUse hello serveru konektoru služby SharePoint ve vašich Logic Apps | Microsoft Docs"
description: "Začněte používat hello hello konektor Server služby SharePoint ve vašich Logic apps"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 3b814f42611e4971ff5c94ae3b021829217911dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sharepoint-connector"></a><span data-ttu-id="15943-103">Začínáme s konektorem služby SharePoint hello</span><span class="sxs-lookup"><span data-stu-id="15943-103">Get started with hello SharePoint connector</span></span>
<span data-ttu-id="15943-104">Hello konektor služby SharePoint poskytuje toowork způsob, jak se seznamy služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="15943-104">hello SharePoint Connector provides an way toowork with Lists on SharePoint.</span></span>

<span data-ttu-id="15943-105">Začněte vytvořením aplikace logiky; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="15943-105">Get started by creating a logic app; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-toosharepoint"></a><span data-ttu-id="15943-106">Vytvoření připojení tooSharePoint</span><span class="sxs-lookup"><span data-stu-id="15943-106">Create a connection tooSharePoint</span></span>
<span data-ttu-id="15943-107">toouse hello konektor služby SharePoint, je nejprve vytvořit **připojení** pak zadejte hello podrobnosti pro tyto vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="15943-107">toouse hello SharePoint Connector , you first create a **connection** then provide hello details for these properties:</span></span> 

| <span data-ttu-id="15943-108">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="15943-108">Property</span></span> | <span data-ttu-id="15943-109">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="15943-109">Required</span></span> | <span data-ttu-id="15943-110">Popis</span><span class="sxs-lookup"><span data-stu-id="15943-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="15943-111">Token</span><span class="sxs-lookup"><span data-stu-id="15943-111">Token</span></span> |<span data-ttu-id="15943-112">Ano</span><span class="sxs-lookup"><span data-stu-id="15943-112">Yes</span></span> |<span data-ttu-id="15943-113">Zadejte pověření serveru SharePoint</span><span class="sxs-lookup"><span data-stu-id="15943-113">Provide SharePoint Credentials</span></span> |

<span data-ttu-id="15943-114">tooconnect příliš**SharePoint**, zadejte tooSharePoint vaší identity (uživatelské jméno a heslo, pověření čipové karty, atd.).</span><span class="sxs-lookup"><span data-stu-id="15943-114">tooconnect too**SharePoint**, enter your identity (username and password, smart card credentials, etc.) tooSharePoint.</span></span> <span data-ttu-id="15943-115">Po jste si ověřit, můžete v aplikaci logiky toouse hello SharePoint konektor.</span><span class="sxs-lookup"><span data-stu-id="15943-115">Once you've been authenticated, you can proceed toouse hello SharePoint connector  in your logic app.</span></span> 

<span data-ttu-id="15943-116">Při na hello návrhář aplikace logiky, postupujte podle těchto kroků toosign do služby SharePoint toocreate hello připojení **připojení** pro použití v aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="15943-116">While on hello designer of your logic app, follow these steps toosign into SharePoint toocreate hello connection **connection** for use in your logic app:</span></span>

1. <span data-ttu-id="15943-117">Hello vyhledávacího pole zadejte SharePoint a počkejte tooreturn hello vyhledávání všech položek s služby SharePoint v názvu hello:</span><span class="sxs-lookup"><span data-stu-id="15943-117">Enter SharePoint in hello search box and wait for hello search tooreturn all entries with SharePoint in hello name:</span></span>   
   ![Konfigurace služby SharePoint][1]  
2. <span data-ttu-id="15943-119">Vyberte **SharePoint – když dojde k vytvoření souboru**</span><span class="sxs-lookup"><span data-stu-id="15943-119">Select **SharePoint - When a file is created**</span></span>   
3. <span data-ttu-id="15943-120">Vyberte **přihlášení tooSharePoint**:</span><span class="sxs-lookup"><span data-stu-id="15943-120">Select **Sign in tooSharePoint**:</span></span>   
   <span data-ttu-id="15943-121">![Konfigurace služby SharePoint][2]</span><span class="sxs-lookup"><span data-stu-id="15943-121">![Configure SharePoint][2]</span></span>    
4. <span data-ttu-id="15943-122">Vaše přihlašovací údaje toosign služby SharePoint v tooauthenticate poskytnout služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="15943-122">Provide your SharePoint credentials toosign in tooauthenticate with SharePoint</span></span>   
   ![Konfigurace služby SharePoint][3]     
5. <span data-ttu-id="15943-124">Po dokončení ověřování hello budete přesměrovaného tooyour logiku aplikace toocomplete ho konfigurací Sharepointu **vytvoření souboru** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="15943-124">After hello authentication completes you'll be redirected tooyour logic app toocomplete it by configuring SharePoint's **When a file is created** dialog.</span></span>          
   <span data-ttu-id="15943-125">![Konfigurace služby SharePoint][4]</span><span class="sxs-lookup"><span data-stu-id="15943-125">![Configure SharePoint][4]</span></span>  
6. <span data-ttu-id="15943-126">Poté můžete přidat další triggery a akce, je nutné toocomplete svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="15943-126">You can then add other triggers and actions that you need toocomplete your logic app.</span></span>   
7. <span data-ttu-id="15943-127">Uložte si práci, výběrem **Uložit** v řádku nabídek hello výše.</span><span class="sxs-lookup"><span data-stu-id="15943-127">Save your work by selecting **Save** on hello menu bar above.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="15943-128">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="15943-128">Connector-specific details</span></span>

<span data-ttu-id="15943-129">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="15943-129">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sharepoint/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="15943-130">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="15943-130">More connectors</span></span>
<span data-ttu-id="15943-131">Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="15943-131">Go back toohello [APIs list](apis-list.md).</span></span>

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
