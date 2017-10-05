---
title: "Přidejte konektor Office 365 Outlook ve vašich Logic Apps | Microsoft Docs"
description: "Vytvoření aplikace logiky s konektor Office 365 Povolit interakci s Office 365. Příklad: vytváření, úpravy a aktualizaci kontakty a položky kalendáře."
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
ms.openlocfilehash: 5335dae62e61659b68e8befb4ed0d404dffb800c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-office-365-outlook-connector"></a><span data-ttu-id="38e33-104">Začínáme s konektor Office 365 Outlook</span><span class="sxs-lookup"><span data-stu-id="38e33-104">Get started with the Office 365 Outlook connector</span></span>
<span data-ttu-id="38e33-105">Konektor Office 365 Outlook umožňuje interakci s aplikací Outlook v Office 365.</span><span class="sxs-lookup"><span data-stu-id="38e33-105">The Office 365 Outlook connector enables interaction with Outlook in Office 365.</span></span> <span data-ttu-id="38e33-106">Pomocí tohoto konektoru můžete vytvořit, upravit a aktualizovat a položky kalendáře, kontaktů a také získat, odeslání a odpovědi k e-mailu.</span><span class="sxs-lookup"><span data-stu-id="38e33-106">Use this connector to create, edit, and update contacts and calendar items, and also get, send, and reply to email.</span></span>

<span data-ttu-id="38e33-107">S aplikací Outlook Office 365 můžete:</span><span class="sxs-lookup"><span data-stu-id="38e33-107">With Office 365 Outlook, you:</span></span>

* <span data-ttu-id="38e33-108">Vytvořte pracovní postup pomocí funkce e-mailu a kalendář v rámci služeb Office 365.</span><span class="sxs-lookup"><span data-stu-id="38e33-108">Build your workflow using the email and calendar features within Office 365.</span></span> 
* <span data-ttu-id="38e33-109">Pomocí aktivační události spustit pracovní postup v případě, že je nový e-mail, když je aktualizována položky kalendáře a další.</span><span class="sxs-lookup"><span data-stu-id="38e33-109">Use triggers to start your workflow when there is a new email, when a calendar item is updated, and more.</span></span>
* <span data-ttu-id="38e33-110">Pomocí akcí můžete odesílat e-mailu, vytvořte novou událost kalendáře a další.</span><span class="sxs-lookup"><span data-stu-id="38e33-110">Use actions to send an email, create a new calendar event, and more.</span></span> <span data-ttu-id="38e33-111">Například po vytvoření nového objektu v Salesforce (aktivační události) poslat e-mailu aplikace Outlook Office 365 (akce).</span><span class="sxs-lookup"><span data-stu-id="38e33-111">For example, when there is a new object in Salesforce (a trigger), send an email to your Office 365 Outlook (an action).</span></span> 

<span data-ttu-id="38e33-112">Toto téma ukazuje, jak používat konektor Office 365 Outlook v aplikaci logiky a taky seznam triggery a akce.</span><span class="sxs-lookup"><span data-stu-id="38e33-112">This topic shows you how to use the Office 365 Outlook connector in a logic app, and also lists the triggers and actions.</span></span>

> [!NOTE]
> <span data-ttu-id="38e33-113">Tato verze článku se vztahuje na Logic Apps obecné dostupnosti (GA).</span><span class="sxs-lookup"><span data-stu-id="38e33-113">This version of the article applies to Logic Apps general availability (GA).</span></span>
> 
> 

<span data-ttu-id="38e33-114">Další informace o Logic Apps najdete v tématu [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="38e33-114">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-office-365"></a><span data-ttu-id="38e33-115">Připojení k Office 365</span><span class="sxs-lookup"><span data-stu-id="38e33-115">Connect to Office 365</span></span>
<span data-ttu-id="38e33-116">Než se aplikace logiky k jakékoli služby, nejprve vytvořit *připojení* ke službě.</span><span class="sxs-lookup"><span data-stu-id="38e33-116">Before your logic app can access any service, you first create a *connection* to the service.</span></span> <span data-ttu-id="38e33-117">Připojení poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="38e33-117">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="38e33-118">Například pokud chcete připojit k Office 365 Outlook, musíte nejprve Office 365 *připojení*.</span><span class="sxs-lookup"><span data-stu-id="38e33-118">For example, to connect to Office 365 Outlook, you first need an Office 365 *connection*.</span></span> <span data-ttu-id="38e33-119">Chcete-li vytvořit připojení, zadejte přihlašovací údaje, které standardně používáte k přístupu ke službě, který chcete připojit k.</span><span class="sxs-lookup"><span data-stu-id="38e33-119">To create a connection, enter the credentials you normally use to access the service you wish to connect to.</span></span> <span data-ttu-id="38e33-120">Proto s Office 365 Outlook, zadejte přihlašovací údaje k účtu služeb Office 365 k vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="38e33-120">So with Office 365 Outlook, enter the credentials to your Office 365 account to create the connection.</span></span>

## <a name="create-the-connection"></a><span data-ttu-id="38e33-121">Vytvoření připojení</span><span class="sxs-lookup"><span data-stu-id="38e33-121">Create the connection</span></span>
> [!INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="38e33-122">Použít aktivační událost</span><span class="sxs-lookup"><span data-stu-id="38e33-122">Use a trigger</span></span>
<span data-ttu-id="38e33-123">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="38e33-123">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="38e33-124">Aktivační události "dotazování" službu v intervalem a frekvenci, který chcete.</span><span class="sxs-lookup"><span data-stu-id="38e33-124">Triggers "poll" the service at an interval and frequency that you want.</span></span> <span data-ttu-id="38e33-125">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="38e33-125">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="38e33-126">V aplikaci logiky zadejte "office 365" získat seznam aktivačních událostí:</span><span class="sxs-lookup"><span data-stu-id="38e33-126">In the logic app, type "office 365" to get a list of the triggers:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. <span data-ttu-id="38e33-127">Vyberte **Office 365 Outlook - během spouštění brzy nadcházející události**.</span><span class="sxs-lookup"><span data-stu-id="38e33-127">Select **Office 365 Outlook - When an upcoming event is starting soon**.</span></span> <span data-ttu-id="38e33-128">Pokud připojení již existuje, pak vyberte kalendář z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="38e33-128">If a connection already exists, then select a calendar from the drop-down list.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    <span data-ttu-id="38e33-129">Pokud budete vyzváni k přihlášení, pak zadejte přihlašovací údaje pro vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="38e33-129">If you are prompted to sign in, then enter the sign in details to create the connection.</span></span> <span data-ttu-id="38e33-130">[Vytvoření připojení](connectors-create-api-office365-outlook.md#create-the-connection) v tomto tématu jsou uvedeny kroky.</span><span class="sxs-lookup"><span data-stu-id="38e33-130">[Create the connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic lists the steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="38e33-131">V tomto příkladu spustí aplikaci logiky při aktualizaci události kalendáře.</span><span class="sxs-lookup"><span data-stu-id="38e33-131">In this example, the logic app runs when a calendar event is updated.</span></span> <span data-ttu-id="38e33-132">Pokud chcete zobrazit výsledky této aktivační události, přidejte další akci, která vám pošle textovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="38e33-132">To see the results of this trigger, add another action that sends you a text message.</span></span> <span data-ttu-id="38e33-133">Například přidat Twilio *odeslat zprávu* akce této texty můžete během spouštění událostí kalendáře za 15 minut.</span><span class="sxs-lookup"><span data-stu-id="38e33-133">For example, add the Twilio *Send message* action that texts you when the calendar event is starting in 15 minutes.</span></span> 
   > 
   > 
3. <span data-ttu-id="38e33-134">Vyberte **upravit** tlačítko a nastavte **frekvence** a **Interval** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="38e33-134">Select the **Edit** button and set the **Frequency** and **Interval** values.</span></span> <span data-ttu-id="38e33-135">Například pokud chcete, aby aktivační událost pro cyklické dotazování každých 15 minut, nastavte **frekvence** k **minutu**a nastavte **Interval** k **15**.</span><span class="sxs-lookup"><span data-stu-id="38e33-135">For example, if you want the trigger to poll every 15 minutes, then set the **Frequency** to **Minute**, and set the **Interval** to **15**.</span></span> 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. <span data-ttu-id="38e33-136">**Uložit** změny (levém horním rohu panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="38e33-136">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="38e33-137">Aplikace logiky je uloženo a automaticky povolí.</span><span class="sxs-lookup"><span data-stu-id="38e33-137">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="38e33-138">Použít akci.</span><span class="sxs-lookup"><span data-stu-id="38e33-138">Use an action</span></span>
<span data-ttu-id="38e33-139">Akce je operace prováděné definované v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="38e33-139">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="38e33-140">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="38e33-140">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="38e33-141">Vyberte znaménko plus.</span><span class="sxs-lookup"><span data-stu-id="38e33-141">Select the plus sign.</span></span> <span data-ttu-id="38e33-142">Zobrazí několik možností: **přidat akci**, **přidat podmínku**, nebo jeden z **Další** možnosti.</span><span class="sxs-lookup"><span data-stu-id="38e33-142">You see several choices: **Add an action**, **Add a condition**, or one of the **More** options.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. <span data-ttu-id="38e33-143">Zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="38e33-143">Choose **Add an action**.</span></span>
3. <span data-ttu-id="38e33-144">Do textového pole zadejte "office 365" získat seznam všechny dostupné akce.</span><span class="sxs-lookup"><span data-stu-id="38e33-144">In the text box, type “office 365” to get a list of all the available actions.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. <span data-ttu-id="38e33-145">V našem příkladu zvolte **Office 365 Outlook - vytvoření kontaktu**.</span><span class="sxs-lookup"><span data-stu-id="38e33-145">In our example, choose **Office 365 Outlook - Create contact**.</span></span> <span data-ttu-id="38e33-146">Pokud připojení již existuje, zvolte **ID složku**, **křestní jméno**a další vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="38e33-146">If a connection already exists, then choose the **Folder ID**, **Given Name**, and other properties:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    <span data-ttu-id="38e33-147">Pokud se zobrazí výzva pro informace o připojení, zadejte podrobnosti k vytvoření připojení.</span><span class="sxs-lookup"><span data-stu-id="38e33-147">If you are prompted for the connection information, then enter the details to create the connection.</span></span> <span data-ttu-id="38e33-148">[Vytvoření připojení](connectors-create-api-office365-outlook.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="38e33-148">[Create the connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="38e33-149">V tomto příkladu vytvoříme nový kontakt v aplikaci Outlook Office 365.</span><span class="sxs-lookup"><span data-stu-id="38e33-149">In this example, we create a new contact in Office 365 Outlook.</span></span> <span data-ttu-id="38e33-150">Výstup z jiné trigger slouží k vytvoření kontakt.</span><span class="sxs-lookup"><span data-stu-id="38e33-150">You can use output from another trigger to create the contact.</span></span> <span data-ttu-id="38e33-151">Například přidejte SalesForce *když je vytvořen objekt* aktivační události.</span><span class="sxs-lookup"><span data-stu-id="38e33-151">For example, add the SalesForce *When an object is created* trigger.</span></span> <span data-ttu-id="38e33-152">Pak přidejte Office 365 Outlook *vytvoření kontaktu* akci, která používá pole SalesForce k vytvoření nové nového kontaktu v Office 365.</span><span class="sxs-lookup"><span data-stu-id="38e33-152">Then add the Office 365 Outlook *Create contact* action that uses the SalesForce fields to create the new new contact in Office 365.</span></span> 
   > 
   > 
5. <span data-ttu-id="38e33-153">**Uložit** změny (levém horním rohu panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="38e33-153">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="38e33-154">Aplikace logiky je uloženo a automaticky povolí.</span><span class="sxs-lookup"><span data-stu-id="38e33-154">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="38e33-155">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="38e33-155">Connector-specific details</span></span>

<span data-ttu-id="38e33-156">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/office365connector/).</span><span class="sxs-lookup"><span data-stu-id="38e33-156">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/office365connector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="38e33-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38e33-157">Next Steps</span></span>
<span data-ttu-id="38e33-158">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="38e33-158">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="38e33-159">Prozkoumejte dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="38e33-159">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

