---
title: "Pomocí konektoru serveru SharePoint Server ve vašich Logic Apps | Microsoft Docs"
description: "Začněte používat konektor Server služby SharePoint ve vašich Logic apps"
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
ms.openlocfilehash: 0f3274816e279a1aa57febaa2f8294914900799a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-sharepoint-connector"></a><span data-ttu-id="02915-103">Začínáme s konektorem služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="02915-103">Get started with the SharePoint connector</span></span>
<span data-ttu-id="02915-104">Konektor služby SharePoint poskytuje způsob práce se seznamy na webu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="02915-104">The SharePoint Connector provides an way to work with Lists on SharePoint.</span></span>

<span data-ttu-id="02915-105">Začněte vytvořením aplikace logiky; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="02915-105">Get started by creating a logic app; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-sharepoint"></a><span data-ttu-id="02915-106">Umožňuje vytvořit připojení do služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="02915-106">Create a connection to SharePoint</span></span>
<span data-ttu-id="02915-107">K používání konektoru služby SharePoint, je třeba nejprve vytvořit **připojení** pak zadejte podrobnosti pro tyto vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="02915-107">To use the SharePoint Connector , you first create a **connection** then provide the details for these properties:</span></span> 

| <span data-ttu-id="02915-108">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="02915-108">Property</span></span> | <span data-ttu-id="02915-109">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="02915-109">Required</span></span> | <span data-ttu-id="02915-110">Popis</span><span class="sxs-lookup"><span data-stu-id="02915-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="02915-111">Token</span><span class="sxs-lookup"><span data-stu-id="02915-111">Token</span></span> |<span data-ttu-id="02915-112">Ano</span><span class="sxs-lookup"><span data-stu-id="02915-112">Yes</span></span> |<span data-ttu-id="02915-113">Zadejte pověření serveru SharePoint</span><span class="sxs-lookup"><span data-stu-id="02915-113">Provide SharePoint Credentials</span></span> |

<span data-ttu-id="02915-114">Pro připojení k **SharePoint**, zadejte svoji identitu (uživatelského jména a hesla, čipové karty přihlašovací údaje, atd.) do služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="02915-114">To connect to **SharePoint**, enter your identity (username and password, smart card credentials, etc.) to SharePoint.</span></span> <span data-ttu-id="02915-115">Jakmile jste jste ověřena, můžete přejít k používání konektoru služby SharePoint v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="02915-115">Once you've been authenticated, you can proceed to use the SharePoint connector  in your logic app.</span></span> 

<span data-ttu-id="02915-116">Při na návrháře aplikace logiky, postupujte podle těchto kroků pro přihlášení do služby SharePoint k vytvoření připojení **připojení** pro použití v aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="02915-116">While on the designer of your logic app, follow these steps to sign into SharePoint to create the connection **connection** for use in your logic app:</span></span>

1. <span data-ttu-id="02915-117">Zadejte do pole vyhledávání služby SharePoint a počkat na výsledky vyhledávání na vrátí všechny položky se službou SharePoint v názvu:</span><span class="sxs-lookup"><span data-stu-id="02915-117">Enter SharePoint in the search box and wait for the search to return all entries with SharePoint in the name:</span></span>   
   ![Konfigurace služby SharePoint][1]  
2. <span data-ttu-id="02915-119">Vyberte **SharePoint – když dojde k vytvoření souboru**</span><span class="sxs-lookup"><span data-stu-id="02915-119">Select **SharePoint - When a file is created**</span></span>   
3. <span data-ttu-id="02915-120">Vyberte **přihlásit do služby SharePoint**:</span><span class="sxs-lookup"><span data-stu-id="02915-120">Select **Sign in to SharePoint**:</span></span>   
   <span data-ttu-id="02915-121">![Konfigurace služby SharePoint][2]</span><span class="sxs-lookup"><span data-stu-id="02915-121">![Configure SharePoint][2]</span></span>    
4. <span data-ttu-id="02915-122">Zadejte svoje přihlašovací údaje služby SharePoint pro přihlášení k ověření pomocí služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="02915-122">Provide your SharePoint credentials to sign in to authenticate with SharePoint</span></span>   
   ![Konfigurace služby SharePoint][3]     
5. <span data-ttu-id="02915-124">Po dokončení ověření budete přesměrováni na svou aplikaci logiky dokončit konfigurací služby SharePoint na **vytvoření souboru** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="02915-124">After the authentication completes you'll be redirected to your logic app to complete it by configuring SharePoint's **When a file is created** dialog.</span></span>          
   <span data-ttu-id="02915-125">![Konfigurace služby SharePoint][4]</span><span class="sxs-lookup"><span data-stu-id="02915-125">![Configure SharePoint][4]</span></span>  
6. <span data-ttu-id="02915-126">Poté můžete přidat další aktivační události a akce, které potřebujete k dokončení svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="02915-126">You can then add other triggers and actions that you need to complete your logic app.</span></span>   
7. <span data-ttu-id="02915-127">Uložte si práci, výběrem **Uložit** na panelu nabídek nahoře.</span><span class="sxs-lookup"><span data-stu-id="02915-127">Save your work by selecting **Save** on the menu bar above.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="02915-128">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="02915-128">Connector-specific details</span></span>

<span data-ttu-id="02915-129">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="02915-129">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/sharepoint/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="02915-130">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="02915-130">More connectors</span></span>
<span data-ttu-id="02915-131">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="02915-131">Go back to the [APIs list](apis-list.md).</span></span>

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
