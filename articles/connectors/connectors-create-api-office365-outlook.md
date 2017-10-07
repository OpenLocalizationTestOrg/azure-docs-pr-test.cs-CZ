---
title: "konektor Office 365 Outlook hello aaaAdd ve vašich Logic Apps | Microsoft Docs"
description: "Vytvoření aplikace logiky s Office 365 konektor tooenable interakci se službami Office 365. Příklad: vytváření, úpravy a aktualizaci kontakty a položky kalendáře."
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2f6cc2c-bba2-493a-b0ba-841785462a80
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 86a573c9c54701de3d3f0500d19eaf545e0710ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-office-365-outlook-connector"></a><span data-ttu-id="d616b-104">Začínáme s konektor Office 365 Outlook hello</span><span class="sxs-lookup"><span data-stu-id="d616b-104">Get started with hello Office 365 Outlook connector</span></span>
<span data-ttu-id="d616b-105">konektor Office 365 Outlook Hello umožňuje interakci s aplikací Outlook v Office 365.</span><span class="sxs-lookup"><span data-stu-id="d616b-105">hello Office 365 Outlook connector enables interaction with Outlook in Office 365.</span></span> <span data-ttu-id="d616b-106">Použít tento konektor toocreate, upravit a aktualizace kontakty a položky kalendáře a také získat, odeslání a odpovědi tooemail.</span><span class="sxs-lookup"><span data-stu-id="d616b-106">Use this connector toocreate, edit, and update contacts and calendar items, and also get, send, and reply tooemail.</span></span>

<span data-ttu-id="d616b-107">S aplikací Outlook Office 365 můžete:</span><span class="sxs-lookup"><span data-stu-id="d616b-107">With Office 365 Outlook, you:</span></span>

* <span data-ttu-id="d616b-108">Vytvořte pracovní postup pomocí funkce e-mailu a kalendář hello v rámci služeb Office 365.</span><span class="sxs-lookup"><span data-stu-id="d616b-108">Build your workflow using hello email and calendar features within Office 365.</span></span> 
* <span data-ttu-id="d616b-109">Použití aktivuje toostart pracovní postup, když dojde nové e-mailu, při aktualizaci položky kalendáře a další.</span><span class="sxs-lookup"><span data-stu-id="d616b-109">Use triggers toostart your workflow when there is a new email, when a calendar item is updated, and more.</span></span>
* <span data-ttu-id="d616b-110">Pomocí akce toosend e-mailu, vytvořte novou událost kalendáře a další.</span><span class="sxs-lookup"><span data-stu-id="d616b-110">Use actions toosend an email, create a new calendar event, and more.</span></span> <span data-ttu-id="d616b-111">Například když je v Salesforce (aktivační událost) nový objekt, odeslat e-mailu tooyour Outlook Office 365 (akce).</span><span class="sxs-lookup"><span data-stu-id="d616b-111">For example, when there is a new object in Salesforce (a trigger), send an email tooyour Office 365 Outlook (an action).</span></span> 

<span data-ttu-id="d616b-112">Toto téma ukazuje, jak toouse hello konektor Office 365 Outlook v aplikaci logiky, a také seznamy hello triggery a akce.</span><span class="sxs-lookup"><span data-stu-id="d616b-112">This topic shows you how toouse hello Office 365 Outlook connector in a logic app, and also lists hello triggers and actions.</span></span>

> [!NOTE]
> <span data-ttu-id="d616b-113">Tato verze článku hello vztahuje tooLogic aplikace obecné dostupnosti (GA).</span><span class="sxs-lookup"><span data-stu-id="d616b-113">This version of hello article applies tooLogic Apps general availability (GA).</span></span>
> 
> 

<span data-ttu-id="d616b-114">toolearn Další informace o Logic Apps, najdete v části [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d616b-114">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toooffice-365"></a><span data-ttu-id="d616b-115">Připojit tooOffice 365</span><span class="sxs-lookup"><span data-stu-id="d616b-115">Connect tooOffice 365</span></span>
<span data-ttu-id="d616b-116">Než se aplikace logiky k jakékoli služby, nejprve vytvořit *připojení* toohello služby.</span><span class="sxs-lookup"><span data-stu-id="d616b-116">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="d616b-117">Připojení poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="d616b-117">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="d616b-118">Například tooconnect tooOffice 365 Outlook, musíte nejprve Office 365 *připojení*.</span><span class="sxs-lookup"><span data-stu-id="d616b-118">For example, tooconnect tooOffice 365 Outlook, you first need an Office 365 *connection*.</span></span> <span data-ttu-id="d616b-119">toocreate připojení, zadejte přihlašovací údaje hello normálně používat tooaccess hello službu, kterou chcete tooconnect k.</span><span class="sxs-lookup"><span data-stu-id="d616b-119">toocreate a connection, enter hello credentials you normally use tooaccess hello service you wish tooconnect to.</span></span> <span data-ttu-id="d616b-120">Proto s Office 365 Outlook, zadejte přihlašovací údaje tooyour hello připojení hello toocreate účet Office 365.</span><span class="sxs-lookup"><span data-stu-id="d616b-120">So with Office 365 Outlook, enter hello credentials tooyour Office 365 account toocreate hello connection.</span></span>

## <a name="create-hello-connection"></a><span data-ttu-id="d616b-121">Vytvoření připojení hello</span><span class="sxs-lookup"><span data-stu-id="d616b-121">Create hello connection</span></span>
> [!INCLUDE [Steps toocreate a connection tooOffice 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="d616b-122">Použít aktivační událost</span><span class="sxs-lookup"><span data-stu-id="d616b-122">Use a trigger</span></span>
<span data-ttu-id="d616b-123">Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="d616b-123">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="d616b-124">Aktivační události "dotazování" hello služby intervalu a frekvenci, který chcete.</span><span class="sxs-lookup"><span data-stu-id="d616b-124">Triggers "poll" hello service at an interval and frequency that you want.</span></span> <span data-ttu-id="d616b-125">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="d616b-125">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="d616b-126">V aplikaci logiky hello zadejte "office 365" tooget seznam hello aktivační události:</span><span class="sxs-lookup"><span data-stu-id="d616b-126">In hello logic app, type "office 365" tooget a list of hello triggers:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. <span data-ttu-id="d616b-127">Vyberte **Office 365 Outlook - během spouštění brzy nadcházející události**.</span><span class="sxs-lookup"><span data-stu-id="d616b-127">Select **Office 365 Outlook - When an upcoming event is starting soon**.</span></span> <span data-ttu-id="d616b-128">Pokud připojení již existuje, vyberte z rozevíracího seznamu hello kalendáře.</span><span class="sxs-lookup"><span data-stu-id="d616b-128">If a connection already exists, then select a calendar from hello drop-down list.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    <span data-ttu-id="d616b-129">Pokud jste výzvami toosign v, zadejte přihlašovací hello podrobnosti toocreate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="d616b-129">If you are prompted toosign in, then enter hello sign in details toocreate hello connection.</span></span> <span data-ttu-id="d616b-130">[Vytvoření připojení hello](connectors-create-api-office365-outlook.md#create-the-connection) v tomto tématu jsou uvedeny kroky hello.</span><span class="sxs-lookup"><span data-stu-id="d616b-130">[Create hello connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic lists hello steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="d616b-131">V tomto příkladu se spouští aplikace logiky hello při aktualizaci události kalendáře.</span><span class="sxs-lookup"><span data-stu-id="d616b-131">In this example, hello logic app runs when a calendar event is updated.</span></span> <span data-ttu-id="d616b-132">toosee hello výsledky této aktivační události, přidejte další akci, která vám pošle textovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="d616b-132">toosee hello results of this trigger, add another action that sends you a text message.</span></span> <span data-ttu-id="d616b-133">Například přidejte hello Twilio *odeslat zprávu* akci, která spouští texty případě hello kalendář události za 15 minut.</span><span class="sxs-lookup"><span data-stu-id="d616b-133">For example, add hello Twilio *Send message* action that texts you when hello calendar event is starting in 15 minutes.</span></span> 
   > 
   > 
3. <span data-ttu-id="d616b-134">Vyberte hello **upravit** tlačítko a nastavte hello **frekvence** a **Interval** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d616b-134">Select hello **Edit** button and set hello **Frequency** and **Interval** values.</span></span> <span data-ttu-id="d616b-135">Například pokud chcete hello aktivační událost toopoll každých 15 minut, pak nastavte hello **frekvence** příliš**minutu**a sadu hello **Interval** příliš**15**.</span><span class="sxs-lookup"><span data-stu-id="d616b-135">For example, if you want hello trigger toopoll every 15 minutes, then set hello **Frequency** too**Minute**, and set hello **Interval** too**15**.</span></span> 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. <span data-ttu-id="d616b-136">**Uložit** změny (levém horním rohu hello panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="d616b-136">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="d616b-137">Aplikace logiky je uloženo a automaticky povolí.</span><span class="sxs-lookup"><span data-stu-id="d616b-137">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="d616b-138">Použít akci.</span><span class="sxs-lookup"><span data-stu-id="d616b-138">Use an action</span></span>
<span data-ttu-id="d616b-139">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="d616b-139">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="d616b-140">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="d616b-140">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="d616b-141">Vyberte znaménko plus hello.</span><span class="sxs-lookup"><span data-stu-id="d616b-141">Select hello plus sign.</span></span> <span data-ttu-id="d616b-142">Zobrazí několik možností: **přidat akci**, **přidat podmínku**, nebo jeden z hello **Další** možnosti.</span><span class="sxs-lookup"><span data-stu-id="d616b-142">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. <span data-ttu-id="d616b-143">Zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="d616b-143">Choose **Add an action**.</span></span>
3. <span data-ttu-id="d616b-144">Hello textového pole zadejte "office 365" tooget seznam všechny dostupné akce hello.</span><span class="sxs-lookup"><span data-stu-id="d616b-144">In hello text box, type “office 365” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. <span data-ttu-id="d616b-145">V našem příkladu zvolte **Office 365 Outlook - vytvoření kontaktu**.</span><span class="sxs-lookup"><span data-stu-id="d616b-145">In our example, choose **Office 365 Outlook - Create contact**.</span></span> <span data-ttu-id="d616b-146">Pokud připojení již existuje, pak zvolte hello **ID složku**, **křestní jméno**a další vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="d616b-146">If a connection already exists, then choose hello **Folder ID**, **Given Name**, and other properties:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    <span data-ttu-id="d616b-147">Pokud budete vyzváni k hello informace o připojení, zadejte hello podrobnosti toocreate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="d616b-147">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="d616b-148">[Vytvoření připojení hello](connectors-create-api-office365-outlook.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d616b-148">[Create hello connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="d616b-149">V tomto příkladu vytvoříme nový kontakt v aplikaci Outlook Office 365.</span><span class="sxs-lookup"><span data-stu-id="d616b-149">In this example, we create a new contact in Office 365 Outlook.</span></span> <span data-ttu-id="d616b-150">Můžete použít výstup z jiného kontaktu hello toocreate aktivační události.</span><span class="sxs-lookup"><span data-stu-id="d616b-150">You can use output from another trigger toocreate hello contact.</span></span> <span data-ttu-id="d616b-151">Například přidejte hello SalesForce *když je vytvořen objekt* aktivační události.</span><span class="sxs-lookup"><span data-stu-id="d616b-151">For example, add hello SalesForce *When an object is created* trigger.</span></span> <span data-ttu-id="d616b-152">Pak přidejte hello Office 365 Outlook *vytvoření kontaktu* akci, která používá hello SalesForce polí toocreate hello nové nového kontaktu v Office 365.</span><span class="sxs-lookup"><span data-stu-id="d616b-152">Then add hello Office 365 Outlook *Create contact* action that uses hello SalesForce fields toocreate hello new new contact in Office 365.</span></span> 
   > 
   > 
5. <span data-ttu-id="d616b-153">**Uložit** změny (levém horním rohu hello panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="d616b-153">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="d616b-154">Aplikace logiky je uloženo a automaticky povolí.</span><span class="sxs-lookup"><span data-stu-id="d616b-154">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="d616b-155">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="d616b-155">Connector-specific details</span></span>

<span data-ttu-id="d616b-156">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/office365connector/).</span><span class="sxs-lookup"><span data-stu-id="d616b-156">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/office365connector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d616b-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d616b-157">Next Steps</span></span>
<span data-ttu-id="d616b-158">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d616b-158">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="d616b-159">Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="d616b-159">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

