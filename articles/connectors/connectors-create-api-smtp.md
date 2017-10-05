---
title: Konektor SMTP v Azure Logic Apps | Microsoft Docs
description: "Vytvoření aplikace logiky službou Azure App service. Pokud potřebujete odeslat e-mail, připojte se pomocí protokolu SMTP."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1cf96bbf8bd215d7ddb3c99860a5cb4e668be3c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-smtp-connector"></a><span data-ttu-id="9d907-104">Začínáme s konektor SMTP</span><span class="sxs-lookup"><span data-stu-id="9d907-104">Get started with the SMTP connector</span></span>
<span data-ttu-id="9d907-105">Pokud potřebujete odeslat e-mail, připojte se pomocí protokolu SMTP.</span><span class="sxs-lookup"><span data-stu-id="9d907-105">Connect to SMTP to send email.</span></span>

<span data-ttu-id="9d907-106">Chcete-li použít [všechny konektory](apis-list.md), musíte nejprve vytvořit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9d907-106">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="9d907-107">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9d907-107">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-smtp"></a><span data-ttu-id="9d907-108">Připojení k SMTP</span><span class="sxs-lookup"><span data-stu-id="9d907-108">Connect to SMTP</span></span>
<span data-ttu-id="9d907-109">Než se aplikace logiky k jakékoli služby, musíte nejprve vytvořit *připojení* ke službě.</span><span class="sxs-lookup"><span data-stu-id="9d907-109">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="9d907-110">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="9d907-110">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="9d907-111">Například pro připojení k SMTP, musíte nejprve serveru SMTP *připojení*.</span><span class="sxs-lookup"><span data-stu-id="9d907-111">For example, to connect to SMTP, you first need an SMTP *connection*.</span></span> <span data-ttu-id="9d907-112">Vytvoření připojení, zadejte přihlašovací údaje, které standardně používáte k přístupu ke službě, ke kterým se připojujete.</span><span class="sxs-lookup"><span data-stu-id="9d907-112">To create a connection, enter the credentials you normally use to access the service you connect to.</span></span> <span data-ttu-id="9d907-113">Ano v příkladu SMTP zadejte přihlašovací údaje k název připojení, adresu serveru SMTP a přihlašovací informace uživatele vytvořit připojení k SMTP.</span><span class="sxs-lookup"><span data-stu-id="9d907-113">So, in the SMTP example, enter the credentials to your connection name, SMTP server address, and user login information to create the connection to SMTP.</span></span>  

### <a name="create-a-connection-to-smtp"></a><span data-ttu-id="9d907-114">Umožňuje vytvořit připojení k SMTP</span><span class="sxs-lookup"><span data-stu-id="9d907-114">Create a connection to SMTP</span></span>
> [!INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a><span data-ttu-id="9d907-115">Aktivační událost pomocí serveru SMTP</span><span class="sxs-lookup"><span data-stu-id="9d907-115">Use an SMTP trigger</span></span>
<span data-ttu-id="9d907-116">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9d907-116">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="9d907-117">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="9d907-117">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="9d907-118">V tomto příkladu, protože SMTP nemá aktivační událost své vlastní, použijeme **Salesforce – když je vytvořen objekt** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="9d907-118">In this example, because SMTP does not have a trigger of its own, we'll use the **Salesforce - When an object is created** trigger.</span></span> <span data-ttu-id="9d907-119">Tento aktivační událost se aktivuje, když je vytvořen nový objekt v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="9d907-119">This trigger activates when a new object is created in Salesforce.</span></span> <span data-ttu-id="9d907-120">Pro náš příklad nastavíme ho tak, aby nové zájemce pokaždé, když je vytvořen v Salesforce, *odeslání e-mailu* akci dojde prostřednictvím konektor SMTP s upozornění při vytváření nové zájemce.</span><span class="sxs-lookup"><span data-stu-id="9d907-120">For our example, we'll set it up such that every time a new lead is created in Salesforce, a *send email* action occurs via the SMTP connector with a notification of the new lead being created.</span></span>

1. <span data-ttu-id="9d907-121">Zadejte *salesforce* do vyhledávacího pole v designeru aplikace logiky zvolte **Salesforce – když je vytvořen objekt** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="9d907-121">Enter *salesforce* in the search box on the logic apps designer then select the **Salesforce - When an object is created** trigger.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. <span data-ttu-id="9d907-122">**Když je vytvořen objekt** ovládací prvek je zobrazen.</span><span class="sxs-lookup"><span data-stu-id="9d907-122">The **When an object is created** control is displayed.</span></span>
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. <span data-ttu-id="9d907-123">Vyberte **typ objektu** vyberte *vést* ze seznamu objektů.</span><span class="sxs-lookup"><span data-stu-id="9d907-123">Select the **Object Type** then select *Lead* from the list of objects.</span></span> <span data-ttu-id="9d907-124">V tomto kroku jsou indikující, že vytváříte aktivační událost, která vás upozorní aplikace logiky, vždy, když se vytvoří nové zájemce v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="9d907-124">In this step you are indicating that you are creating a trigger that will notify your logic app whenever a new lead is created in Salesforce.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. <span data-ttu-id="9d907-125">Aktivační událost byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="9d907-125">The trigger has been created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a><span data-ttu-id="9d907-126">Použít akci SMTP</span><span class="sxs-lookup"><span data-stu-id="9d907-126">Use an SMTP action</span></span>
<span data-ttu-id="9d907-127">Akce je operace prováděné definované v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="9d907-127">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="9d907-128">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="9d907-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="9d907-129">Teď, když byla přidána aktivační událost, použijte následující postup přidání akce protokolu SMTP, která se stane, když se vytvoří nové zájemce v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="9d907-129">Now that the trigger has been added, follow these steps to add an SMTP action that will occur when a new lead is created in Salesforce.</span></span>

1. <span data-ttu-id="9d907-130">Vyberte **+ nový krok** přidat akci chcete provést, když se vytvoří nové zájemce.</span><span class="sxs-lookup"><span data-stu-id="9d907-130">Select **+ New Step** to add the action you would like to take when a new lead is created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. <span data-ttu-id="9d907-131">Vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="9d907-131">Select **Add an action**.</span></span> <span data-ttu-id="9d907-132">Otevře se tato, které chcete do vyhledávacího pole, kde můžete vyhledat všechny akce můžete provést.</span><span class="sxs-lookup"><span data-stu-id="9d907-132">This opens the search box where you can search for any action you would like to take.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. <span data-ttu-id="9d907-133">Zadejte *smtp* k vyhledání akcí souvisejících s SMTP.</span><span class="sxs-lookup"><span data-stu-id="9d907-133">Enter *smtp* to search for actions related to SMTP.</span></span>  
4. <span data-ttu-id="9d907-134">Vyberte **SMTP - odeslat E-mail** jako pro případ, kdy se vytvoří nové zájemce.</span><span class="sxs-lookup"><span data-stu-id="9d907-134">Select **SMTP - Send Email** as the action to take when the new lead is created.</span></span> <span data-ttu-id="9d907-135">Otevře se řídicí blok akce.</span><span class="sxs-lookup"><span data-stu-id="9d907-135">The action control block opens.</span></span> <span data-ttu-id="9d907-136">Budete mít k navázání připojení k smtp v návrháře bloku, pokud jste tak dosud neučinili dříve.</span><span class="sxs-lookup"><span data-stu-id="9d907-136">You will have to establish your smtp connection in the designer block if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. <span data-ttu-id="9d907-137">Zadejte příslušné informace požadované e-mailu na **SMTP - odeslat E-mail** bloku.</span><span class="sxs-lookup"><span data-stu-id="9d907-137">Input your desired email information in the **SMTP - Send Email** block.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. <span data-ttu-id="9d907-138">Uložte si práci, aby bylo možné aktivovat pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="9d907-138">Save your work in order to activate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="9d907-139">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="9d907-139">Connector-specific details</span></span>

<span data-ttu-id="9d907-140">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/smtpconnector/).</span><span class="sxs-lookup"><span data-stu-id="9d907-140">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/smtpconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="9d907-141">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="9d907-141">More connectors</span></span>
<span data-ttu-id="9d907-142">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="9d907-142">Go back to the [APIs list](apis-list.md).</span></span>