---
title: konektor aaaSMTP v Azure Logic Apps | Microsoft Docs
description: "Vytvoření aplikace logiky službou Azure App service. Připojte tooSMTP toosend e-mailu."
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
ms.openlocfilehash: 36bb836851014d24f2e069fda8376ad7a08c943b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-smtp-connector"></a><span data-ttu-id="ab149-104">Začínáme s konektor SMTP hello</span><span class="sxs-lookup"><span data-stu-id="ab149-104">Get started with hello SMTP connector</span></span>
<span data-ttu-id="ab149-105">Připojte tooSMTP toosend e-mailu.</span><span class="sxs-lookup"><span data-stu-id="ab149-105">Connect tooSMTP toosend email.</span></span>

<span data-ttu-id="ab149-106">toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="ab149-106">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="ab149-107">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="ab149-107">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosmtp"></a><span data-ttu-id="ab149-108">Připojit tooSMTP</span><span class="sxs-lookup"><span data-stu-id="ab149-108">Connect tooSMTP</span></span>
<span data-ttu-id="ab149-109">Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby.</span><span class="sxs-lookup"><span data-stu-id="ab149-109">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="ab149-110">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="ab149-110">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="ab149-111">Například tooconnect tooSMTP, musíte nejdřív serveru SMTP *připojení*.</span><span class="sxs-lookup"><span data-stu-id="ab149-111">For example, tooconnect tooSMTP, you first need an SMTP *connection*.</span></span> <span data-ttu-id="ab149-112">toocreate připojení, zadejte přihlašovací údaje hello normálně používat tooaccess hello služby, ke kterým se připojujete.</span><span class="sxs-lookup"><span data-stu-id="ab149-112">toocreate a connection, enter hello credentials you normally use tooaccess hello service you connect to.</span></span> <span data-ttu-id="ab149-113">Ano v příkladu hello SMTP, zadejte název připojení tooyour hello přihlašovací údaje, adresy serveru SMTP a uživatelské přihlašovací informace toocreate hello připojení tooSMTP.</span><span class="sxs-lookup"><span data-stu-id="ab149-113">So, in hello SMTP example, enter hello credentials tooyour connection name, SMTP server address, and user login information toocreate hello connection tooSMTP.</span></span>  

### <a name="create-a-connection-toosmtp"></a><span data-ttu-id="ab149-114">Vytvoření připojení tooSMTP</span><span class="sxs-lookup"><span data-stu-id="ab149-114">Create a connection tooSMTP</span></span>
> [!INCLUDE [Steps toocreate a connection tooSMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a><span data-ttu-id="ab149-115">Aktivační událost pomocí serveru SMTP</span><span class="sxs-lookup"><span data-stu-id="ab149-115">Use an SMTP trigger</span></span>
<span data-ttu-id="ab149-116">Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="ab149-116">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="ab149-117">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="ab149-117">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="ab149-118">V tomto příkladu, protože SMTP nemá aktivační událost své vlastní, použijeme hello **Salesforce – když je vytvořen objekt** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="ab149-118">In this example, because SMTP does not have a trigger of its own, we'll use hello **Salesforce - When an object is created** trigger.</span></span> <span data-ttu-id="ab149-119">Tento aktivační událost se aktivuje, když je vytvořen nový objekt v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="ab149-119">This trigger activates when a new object is created in Salesforce.</span></span> <span data-ttu-id="ab149-120">Pro náš příklad nastavíme ho tak, aby nové zájemce pokaždé, když je vytvořen v Salesforce, *odeslání e-mailu* akci dojde prostřednictvím konektoru hello SMTP s upozornění při vytváření nové zájemce hello.</span><span class="sxs-lookup"><span data-stu-id="ab149-120">For our example, we'll set it up such that every time a new lead is created in Salesforce, a *send email* action occurs via hello SMTP connector with a notification of hello new lead being created.</span></span>

1. <span data-ttu-id="ab149-121">Zadejte *salesforce* hello vyhledávacího pole v designeru aplikace logiky hello zvolte hello **Salesforce – když je vytvořen objekt** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="ab149-121">Enter *salesforce* in hello search box on hello logic apps designer then select hello **Salesforce - When an object is created** trigger.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. <span data-ttu-id="ab149-122">Hello **když je vytvořen objekt** ovládací prvek je zobrazen.</span><span class="sxs-lookup"><span data-stu-id="ab149-122">hello **When an object is created** control is displayed.</span></span>
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. <span data-ttu-id="ab149-123">Vyberte hello **typ objektu** vyberte *vést* hello seznamu objektů.</span><span class="sxs-lookup"><span data-stu-id="ab149-123">Select hello **Object Type** then select *Lead* from hello list of objects.</span></span> <span data-ttu-id="ab149-124">V tomto kroku jsou indikující, že vytváříte aktivační událost, která vás upozorní aplikace logiky, vždy, když se vytvoří nové zájemce v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="ab149-124">In this step you are indicating that you are creating a trigger that will notify your logic app whenever a new lead is created in Salesforce.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. <span data-ttu-id="ab149-125">Hello aktivační událost byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="ab149-125">hello trigger has been created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a><span data-ttu-id="ab149-126">Použít akci SMTP</span><span class="sxs-lookup"><span data-stu-id="ab149-126">Use an SMTP action</span></span>
<span data-ttu-id="ab149-127">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="ab149-127">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="ab149-128">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="ab149-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="ab149-129">Teď, když hello aktivační událost byla přidána, postupujte podle těchto kroků tooadd serveru SMTP akci, která se stane, když se vytvoří nové zájemce v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="ab149-129">Now that hello trigger has been added, follow these steps tooadd an SMTP action that will occur when a new lead is created in Salesforce.</span></span>

1. <span data-ttu-id="ab149-130">Vyberte **+ nový krok** tooadd hello akce chcete tootake při vytváření nové zájemce.</span><span class="sxs-lookup"><span data-stu-id="ab149-130">Select **+ New Step** tooadd hello action you would like tootake when a new lead is created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. <span data-ttu-id="ab149-131">Vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="ab149-131">Select **Add an action**.</span></span> <span data-ttu-id="ab149-132">Pole pro vyhledávání otevře hello kde můžete vyhledat všechny akce jste chtěli tootake.</span><span class="sxs-lookup"><span data-stu-id="ab149-132">This opens hello search box where you can search for any action you would like tootake.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. <span data-ttu-id="ab149-133">Zadejte *smtp* toosearch pro akce související tooSMTP.</span><span class="sxs-lookup"><span data-stu-id="ab149-133">Enter *smtp* toosearch for actions related tooSMTP.</span></span>  
4. <span data-ttu-id="ab149-134">Vyberte **SMTP - odeslat E-mail** jako hello tootake akce při vytváření nové zájemce hello.</span><span class="sxs-lookup"><span data-stu-id="ab149-134">Select **SMTP - Send Email** as hello action tootake when hello new lead is created.</span></span> <span data-ttu-id="ab149-135">Otevře se řídicí blok Hello akce.</span><span class="sxs-lookup"><span data-stu-id="ab149-135">hello action control block opens.</span></span> <span data-ttu-id="ab149-136">Budete mít tooestablish připojení smtp v bloku návrháře hello Pokud jste tak dosud neučinili dříve.</span><span class="sxs-lookup"><span data-stu-id="ab149-136">You will have tooestablish your smtp connection in hello designer block if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. <span data-ttu-id="ab149-137">Zadejte příslušné informace požadované e-mailu na hello **SMTP - odeslat E-mail** bloku.</span><span class="sxs-lookup"><span data-stu-id="ab149-137">Input your desired email information in hello **SMTP - Send Email** block.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. <span data-ttu-id="ab149-138">Uložte práci v pořadí tooactivate pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="ab149-138">Save your work in order tooactivate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="ab149-139">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="ab149-139">Connector-specific details</span></span>

<span data-ttu-id="ab149-140">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/smtpconnector/).</span><span class="sxs-lookup"><span data-stu-id="ab149-140">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/smtpconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="ab149-141">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="ab149-141">More connectors</span></span>
<span data-ttu-id="ab149-142">Přejděte zpět toohello [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="ab149-142">Go back toohello [APIs list](apis-list.md).</span></span>
