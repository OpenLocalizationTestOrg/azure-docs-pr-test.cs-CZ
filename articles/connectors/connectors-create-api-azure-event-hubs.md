---
title: "Nastavení monitorování událostí s Azure Event Hubs pro Azure Logic Apps | Microsoft Docs"
description: "Monitorování datových proudů pro příjem událostí a odesílání událostí pro Azure Logic Apps s Azure Event Hubs"
services: logic-apps
keywords: "datový proud, monitorování událostí, služba event hubs"
author: ecfan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: estfan; LADocs
ms.openlocfilehash: 2ca27fb8269d1796fb1181fc4d0a8744a592d548
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a><span data-ttu-id="24ee3-104">Monitorování, příjem a odesílání událostí s konektorem služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="24ee3-104">Monitor, receive, and send events with the Event Hubs connector</span></span>

<span data-ttu-id="24ee3-105">Chcete-li nastavit monitorování událostí tak, aby aplikace logiky můžete zjišťovat události, přijímat události a odesílat události, připojte se ke [centra událostí Azure](https://azure.microsoft.com/services/event-hubs) z aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="24ee3-105">To set up an event monitor so that your logic app can detect events, receive events, and send events, connect to an [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) from your logic app.</span></span> <span data-ttu-id="24ee3-106">Další informace o [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="24ee3-106">Learn more about [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>

## <a name="requirements"></a><span data-ttu-id="24ee3-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="24ee3-107">Requirements</span></span>

* <span data-ttu-id="24ee3-108">Musíte mít [Event Hubs obor názvů a centra událostí](../event-hubs/event-hubs-create.md) v Azure.</span><span class="sxs-lookup"><span data-stu-id="24ee3-108">You have to have an [Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md) in Azure.</span></span> <span data-ttu-id="24ee3-109">Další informace [postup vytvoření oboru názvů služby Event Hubs a Centrum událostí](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="24ee3-109">Learn [how to create an Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md).</span></span> 

* <span data-ttu-id="24ee3-110">Chcete-li použít [všechny konektory](https://docs.microsoft.com/azure/connectors/apis-list) ve vaší aplikaci logiky, musíte nejprve vytvořte aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="24ee3-110">To use [any connector](https://docs.microsoft.com/azure/connectors/apis-list) in your logic app, you have to create a logic app first.</span></span> <span data-ttu-id="24ee3-111">Další informace [postup vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="24ee3-111">Learn [how to create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-the-connection-string"></a><span data-ttu-id="24ee3-112">Zkontrolujte oprávnění k oboru názvů služby Event Hubs a najděte připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="24ee3-112">Check Event Hubs namespace permissions and find the connection string</span></span>

<span data-ttu-id="24ee3-113">Aplikace logiky pro přístup k žádné službě, je nutné vytvořit [ *připojení* ](./connectors-overview.md) mezi svou aplikaci logiky a služby, pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="24ee3-113">For your logic app to access any service, you have to create a [*connection*](./connectors-overview.md) between your logic app and the service, if you haven't already.</span></span> <span data-ttu-id="24ee3-114">Toto připojení autorizuje aplikace logiky pro přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="24ee3-114">This connection authorizes your logic app to access data.</span></span>
<span data-ttu-id="24ee3-115">Aplikace logiky pro přístup k vašeho centra událostí, musíte mít **spravovat** oprávnění a připojovací řetězec pro obor názvů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="24ee3-115">For your logic app to access your Event Hub, you have to have **Manage** permissions and the connection string for your Event Hubs namespace.</span></span>

<span data-ttu-id="24ee3-116">Zkontrolujte svá oprávnění a získat připojovací řetězec, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="24ee3-116">To check your permissions and get the connection string, follow these steps.</span></span>

1.  <span data-ttu-id="24ee3-117">Přihlaste se na web [Azure Portal](https://portal.azure.com "Azure Portal").</span><span class="sxs-lookup"><span data-stu-id="24ee3-117">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span> 

2.  <span data-ttu-id="24ee3-118">Přejděte do vašeho centra událostí *obor názvů*, nikoli konkrétní Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="24ee3-118">Go to your Event Hubs *namespace*, not the specific Event Hub.</span></span> <span data-ttu-id="24ee3-119">V okně oboru názvů v části **nastavení**, zvolte **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="24ee3-119">On the namespace blade, under **Settings**, choose **Shared access policies**.</span></span> <span data-ttu-id="24ee3-120">V části **deklarace identity**, zkontrolujte, zda máte **spravovat** oprávnění pro tento obor názvů.</span><span class="sxs-lookup"><span data-stu-id="24ee3-120">Under **Claims**, check that you have **Manage** permissions for that namespace.</span></span>

    ![Správa oprávnění pro obor názvů centra událostí](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  <span data-ttu-id="24ee3-122">Chcete-li zkopírujte připojovací řetězec pro obor názvů služby Event Hubs, zvolte **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="24ee3-122">To copy the connection string for the Event Hubs namespace, choose **RootManageSharedAccessKey**.</span></span> <span data-ttu-id="24ee3-123">Vedle primární klíče připojovacího řetězce vyberte tlačítko Kopírovat.</span><span class="sxs-lookup"><span data-stu-id="24ee3-123">Next to your primary key connection string, choose the copy button.</span></span>

    ![Zkopírujte Event Hubs obor názvů připojovacího řetězce](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > <span data-ttu-id="24ee3-125">Chcete-li ověřit, zda je připojovací řetězec přidružené vašeho oboru názvů služby Event Hubs nebo s konkrétní Centrum událostí, zkontrolujte připojovací řetězec pro `EntityPath` parametr.</span><span class="sxs-lookup"><span data-stu-id="24ee3-125">To confirm whether your connection string is associated with your Event Hubs namespace or with a specific Event Hub, check the connection string for the `EntityPath` parameter.</span></span> <span data-ttu-id="24ee3-126">Pokud zjistíte, tento parametr, připojovací řetězec je pro konkrétní Centrum událostí "entita" a není správný řetězec, který má používat s svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="24ee3-126">If you find this parameter, the connection string is for a specific Event Hub "entity", and is not the correct string to use with your logic app.</span></span>

4.  <span data-ttu-id="24ee3-127">Nyní když se zobrazí výzva k zadání pověření po přidání Event Hubs aktivační události nebo akce do aplikace logiky, můžete připojit k oboru názvů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="24ee3-127">Now when you're prompted for credentials after adding an Event Hubs trigger or action to your logic app, you can connect to your Event Hubs namespace.</span></span> <span data-ttu-id="24ee3-128">Zadejte název připojení, zadejte připojovací řetězec, který jste zkopírovali a zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="24ee3-128">Give your connection a name, enter the connection string that you copied, and choose **Create**.</span></span>

    ![Zadejte připojovací řetězec pro obor názvů služby Event Hubs](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    <span data-ttu-id="24ee3-130">Po vytvoření připojení, název připojení by se zobrazit události rozbočovače aktivační události nebo akce.</span><span class="sxs-lookup"><span data-stu-id="24ee3-130">After you create your connection, the connection name should appear in the Event Hubs trigger or action.</span></span> 
    <span data-ttu-id="24ee3-131">Pak můžete pokračovat v další kroky v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="24ee3-131">You can then continue with the other steps in your logic app.</span></span>

    ![Připojení obor názvů centra událostí vytvořit](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a><span data-ttu-id="24ee3-133">Spustit pracovní postup v případě, že Centrum událostí obdrží nové události</span><span class="sxs-lookup"><span data-stu-id="24ee3-133">Start workflow when your Event Hub receives new events</span></span>

<span data-ttu-id="24ee3-134">A [ *aktivační událost* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) je událost, která se spouští v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="24ee3-134">A [*trigger*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) is an event that starts a workflow in your logic app.</span></span> <span data-ttu-id="24ee3-135">Spuštění pracovního postupu odeslání nové události do vašeho centra událostí, postupujte podle těchto kroků pro přidání aktivační událost, která zjistí tuto událost.</span><span class="sxs-lookup"><span data-stu-id="24ee3-135">To start a workflow when new events are sent to your Event Hub, follow these steps for adding the trigger that detects this event.</span></span>

1.  <span data-ttu-id="24ee3-136">V [portál Azure](https://portal.azure.com "portál Azure"), přejděte do existující aplikace logiky nebo vytvoření aplikace logiky prázdné.</span><span class="sxs-lookup"><span data-stu-id="24ee3-136">In the [Azure portal](https://portal.azure.com "Azure portal"), go to your existing logic app or create a blank logic app.</span></span>

2.  <span data-ttu-id="24ee3-137">Do vyhledávacího pole pro návrhář aplikace logiky, zadejte `event hubs` filtru.</span><span class="sxs-lookup"><span data-stu-id="24ee3-137">In the search box for the Logic App Designer, enter `event hubs` for your filter.</span></span> <span data-ttu-id="24ee3-138">Vyberte této aktivační události: **když jsou události dostupné v Centru událostí**</span><span class="sxs-lookup"><span data-stu-id="24ee3-138">Select this trigger: **When events are available in Event Hub**</span></span>

    ![Vyberte aktivační událost pro Jakmile Centrum událostí obdrží nové události](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    <span data-ttu-id="24ee3-140">Pokud již nemáte připojení k oboru názvů služby Event Hubs, se zobrazí výzva k vytvoření nyní toto připojení.</span><span class="sxs-lookup"><span data-stu-id="24ee3-140">If you don't already have a connection to your Event Hubs namespace, you're prompted to create this connection now.</span></span> <span data-ttu-id="24ee3-141">Zadejte název připojení a zadat připojovací řetězec pro obor názvů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="24ee3-141">Give your connection a name, and enter the connection string for your Event Hubs namespace.</span></span> 
    <span data-ttu-id="24ee3-142">V případě potřeby další [jak najít připojovací řetězec](#permissions-connection-string).</span><span class="sxs-lookup"><span data-stu-id="24ee3-142">If necessary, learn [how to find your connection string](#permissions-connection-string).</span></span>

    ![Zadejte připojovací řetězec pro obor názvů služby Event Hubs](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    <span data-ttu-id="24ee3-144">Po vytvoření připojení, nastavení **při událost v dostupné v Centru událostí** zobrazí aktivační události.</span><span class="sxs-lookup"><span data-stu-id="24ee3-144">After you create the connection, the settings for the **When an event in available in an Event Hub** trigger appear.</span></span>

    ![Aktivační událost nastavení po Centrum událostí obdrží nové události](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  <span data-ttu-id="24ee3-146">Zadejte nebo vyberte název centra událostí, které chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="24ee3-146">Enter or select the name for the Event Hub that you want to monitor.</span></span> <span data-ttu-id="24ee3-147">Vyberte četnost a interval jak často chcete zkontrolovat centra událostí.</span><span class="sxs-lookup"><span data-stu-id="24ee3-147">Select the frequency and interval for how often you want to check the Event Hub.</span></span>

    > [!TIP]
    > <span data-ttu-id="24ee3-148">Chcete-li můžete volitelně vybrat skupinu uživatelů pro čtení událostí, zvolte **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="24ee3-148">To optionally select a consumer group for reading events, choose **Show advanced options**.</span></span> 

    ![Zadejte centra událostí nebo skupiny uživatelů](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    <span data-ttu-id="24ee3-150">Jste nyní nastavili aktivační událost spuštění pracovního postupu pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="24ee3-150">You've now set up a trigger to start a workflow for your logic app.</span></span> 
    <span data-ttu-id="24ee3-151">Aplikace logiky ověří zadaný centra událostí podle plánu, který nastavíte.</span><span class="sxs-lookup"><span data-stu-id="24ee3-151">Your logic app checks the specified Event Hub based on the schedule that you set.</span></span> 
    <span data-ttu-id="24ee3-152">Pokud vaše aplikace vyhledá nové události události rozbočovače, aktivační událost spustí další akce nebo aktivuje v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="24ee3-152">If your app finds new events in the Event Hub, the trigger runs other actions or triggers in your logic app.</span></span>

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a><span data-ttu-id="24ee3-153">Odesílání událostí do vašeho centra událostí z aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="24ee3-153">Send events to your Event Hub from your logic app</span></span>

<span data-ttu-id="24ee3-154">[*Akce*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) je úloha, kterou provádí pracovní postup vaší aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="24ee3-154">An [*action*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) is a task performed by your logic app workflow.</span></span> <span data-ttu-id="24ee3-155">Když do aplikace logiky přidáte trigger, můžete přidat akci, která bude provádět určitou operaci s daty generovanými triggerem.</span><span class="sxs-lookup"><span data-stu-id="24ee3-155">After you add a trigger to your logic app, you can add an action to perform operations with data generated by that trigger.</span></span> <span data-ttu-id="24ee3-156">Pro odeslání události do vašeho centra událostí z aplikace logiky, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="24ee3-156">To send an event to your Event Hub from your logic app, follow these steps.</span></span>

1.  <span data-ttu-id="24ee3-157">V návrháři aplikace logiky, vyberte v části vaší aktivační událostí aplikace logiky **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="24ee3-157">In Logic App Designer, under your logic app trigger, choose **New step** > **Add an action**.</span></span>

    ![Pak zvolte "Nový krok", "Přidat akci."](./media/connectors-create-api-azure-event-hubs/add-action.png)

    <span data-ttu-id="24ee3-159">Nyní můžete najít a vyberte akci, kterou chcete provést.</span><span class="sxs-lookup"><span data-stu-id="24ee3-159">Now you can find and select an action to perform.</span></span> 
    <span data-ttu-id="24ee3-160">I když můžete vybrat všechny akce, například chceme akce Event Hubs odesílat události.</span><span class="sxs-lookup"><span data-stu-id="24ee3-160">Although you can select any action, for this example, we want the Event Hubs action to send events.</span></span>

2.  <span data-ttu-id="24ee3-161">Do vyhledávacího pole zadejte `event hubs` filtru.</span><span class="sxs-lookup"><span data-stu-id="24ee3-161">In the search box, enter `event hubs` for your filter.</span></span>
<span data-ttu-id="24ee3-162">Vyberte tuto akci: **odesílání událostí**</span><span class="sxs-lookup"><span data-stu-id="24ee3-162">Select this action: **Send event**</span></span>

    ![Vyberte "Event Hubs - odesílání událostí" akce](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  <span data-ttu-id="24ee3-164">Zadejte požadované podrobnosti události, jako je například název centra událostí, ve které chcete odeslat událost.</span><span class="sxs-lookup"><span data-stu-id="24ee3-164">Enter the required details for the event, such as the name for the Event Hub where you want to send the event.</span></span> <span data-ttu-id="24ee3-165">Zadejte další volitelné podrobnosti o události, jako je třeba obsah pro tuto událost.</span><span class="sxs-lookup"><span data-stu-id="24ee3-165">Enter any other optional details about the event, such as content for that event.</span></span>

    > [!TIP]
    > <span data-ttu-id="24ee3-166">Volitelně zadejte oddílu centra událostí, kam chcete odeslat událost, zvolte **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="24ee3-166">To optionally specify the Event Hub partition where to send the event, choose **Show advanced options**.</span></span> 

    ![Zadejte název centra událostí a podrobnosti volitelné události](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  <span data-ttu-id="24ee3-168">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="24ee3-168">Save your changes.</span></span>

    ![Uložení aplikace logiky](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    <span data-ttu-id="24ee3-170">Jste nyní nastavili akce pro odeslání událostí ze svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="24ee3-170">You've now set up an action to send events from your logic app.</span></span> 

## <a name="connector-specific-details"></a><span data-ttu-id="24ee3-171">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="24ee3-171">Connector-specific details</span></span>

<span data-ttu-id="24ee3-172">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/eventhubs/).</span><span class="sxs-lookup"><span data-stu-id="24ee3-172">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/eventhubs/).</span></span> 

## <a name="get-help"></a><span data-ttu-id="24ee3-173">Podpora</span><span class="sxs-lookup"><span data-stu-id="24ee3-173">Get help</span></span>

<span data-ttu-id="24ee3-174">Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="24ee3-174">To ask questions, answer questions, and see what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="24ee3-175">Pokud chcete pomoci při vylepšování Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="24ee3-175">To help improve Logic Apps and connectors, vote on or submit ideas at the [Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="24ee3-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24ee3-176">Next steps</span></span>

*  [<span data-ttu-id="24ee3-177">Najít další konektory pro Azure Logic apps</span><span class="sxs-lookup"><span data-stu-id="24ee3-177">Find other connectors for Azure Logic apps</span></span>](./apis-list.md)