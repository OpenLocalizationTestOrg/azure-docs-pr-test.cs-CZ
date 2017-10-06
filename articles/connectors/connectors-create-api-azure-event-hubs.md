---
title: "aaaSet až monitorování událostí s Azure Event Hubs pro Azure Logic Apps | Microsoft Docs"
description: "Sledování událostí tooreceive datových proudů dat a odesílání událostí pro Azure Logic Apps s Azure Event Hubs"
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
ms.openlocfilehash: 4aad2c2ac1134b4d4d440019b4773559e49be122
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-receive-and-send-events-with-hello-event-hubs-connector"></a><span data-ttu-id="62700-104">Monitorování, příjem a odesílání událostí s konektorem služby Event Hubs hello</span><span class="sxs-lookup"><span data-stu-id="62700-104">Monitor, receive, and send events with hello Event Hubs connector</span></span>

<span data-ttu-id="62700-105">tooset nahoru monitorování událostí tak, aby aplikace logiky můžete zjišťovat události, přijímat události a odesílat události, připojit tooan [centra událostí Azure](https://azure.microsoft.com/services/event-hubs) z aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="62700-105">tooset up an event monitor so that your logic app can detect events, receive events, and send events, connect tooan [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) from your logic app.</span></span> <span data-ttu-id="62700-106">Další informace o [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="62700-106">Learn more about [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>

## <a name="requirements"></a><span data-ttu-id="62700-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="62700-107">Requirements</span></span>

* <span data-ttu-id="62700-108">Máte toohave [Event Hubs obor názvů a centra událostí](../event-hubs/event-hubs-create.md) v Azure.</span><span class="sxs-lookup"><span data-stu-id="62700-108">You have toohave an [Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md) in Azure.</span></span> <span data-ttu-id="62700-109">Další informace [jak toocreate služby Event Hubs obor názvů a centra událostí](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="62700-109">Learn [how toocreate an Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md).</span></span> 

* <span data-ttu-id="62700-110">toouse [všechny konektory](https://docs.microsoft.com/azure/connectors/apis-list) v aplikaci logiky máte toocreate aplikace logiky poprvé.</span><span class="sxs-lookup"><span data-stu-id="62700-110">toouse [any connector](https://docs.microsoft.com/azure/connectors/apis-list) in your logic app, you have toocreate a logic app first.</span></span> <span data-ttu-id="62700-111">Další informace [jak toocreate aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="62700-111">Learn [how toocreate a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-hello-connection-string"></a><span data-ttu-id="62700-112">Zkontrolujte oprávnění k oboru názvů služby Event Hubs a najít hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="62700-112">Check Event Hubs namespace permissions and find hello connection string</span></span>

<span data-ttu-id="62700-113">Pro vaše aplikace tooaccess logiku žádné služby, máte toocreate [ *připojení* ](./connectors-overview.md) mezi logiku aplikace a hello služby, pokud jste tak ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="62700-113">For your logic app tooaccess any service, you have toocreate a [*connection*](./connectors-overview.md) between your logic app and hello service, if you haven't already.</span></span> <span data-ttu-id="62700-114">Toto připojení autorizuje data tooaccess logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="62700-114">This connection authorizes your logic app tooaccess data.</span></span>
<span data-ttu-id="62700-115">Vaše tooaccess aplikace logiky Centrum událostí, je nutné toohave **spravovat** oprávnění a hello připojovací řetězec pro obor názvů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="62700-115">For your logic app tooaccess your Event Hub, you have toohave **Manage** permissions and hello connection string for your Event Hubs namespace.</span></span>

<span data-ttu-id="62700-116">toocheck oprávnění a get hello připojovací řetězec, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="62700-116">toocheck your permissions and get hello connection string, follow these steps.</span></span>

1.  <span data-ttu-id="62700-117">Přihlaste se toohello [portál Azure](https://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="62700-117">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span> 

2.  <span data-ttu-id="62700-118">Přejděte tooyour Event Hubs *obor názvů*, není hello konkrétní Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="62700-118">Go tooyour Event Hubs *namespace*, not hello specific Event Hub.</span></span> <span data-ttu-id="62700-119">V okně hello obor názvů v části **nastavení**, zvolte **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="62700-119">On hello namespace blade, under **Settings**, choose **Shared access policies**.</span></span> <span data-ttu-id="62700-120">V části **deklarace identity**, zkontrolujte, zda máte **spravovat** oprávnění pro tento obor názvů.</span><span class="sxs-lookup"><span data-stu-id="62700-120">Under **Claims**, check that you have **Manage** permissions for that namespace.</span></span>

    ![Správa oprávnění pro obor názvů centra událostí](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  <span data-ttu-id="62700-122">Zvolte toocopy hello připojovací řetězec pro obor názvů hello Event Hubs, **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="62700-122">toocopy hello connection string for hello Event Hubs namespace, choose **RootManageSharedAccessKey**.</span></span> <span data-ttu-id="62700-123">Další tooyour primární klíče připojovací řetězec, zvolte tlačítko Kopírovat hello.</span><span class="sxs-lookup"><span data-stu-id="62700-123">Next tooyour primary key connection string, choose hello copy button.</span></span>

    ![Zkopírujte Event Hubs obor názvů připojovacího řetězce](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > <span data-ttu-id="62700-125">tooconfirm, zda je připojovací řetězec přidružené vašeho oboru názvů služby Event Hubs nebo s konkrétní Centrum událostí, zkontrolujte hello připojovací řetězec pro hello `EntityPath` parametr.</span><span class="sxs-lookup"><span data-stu-id="62700-125">tooconfirm whether your connection string is associated with your Event Hubs namespace or with a specific Event Hub, check hello connection string for hello `EntityPath` parameter.</span></span> <span data-ttu-id="62700-126">Pokud zjistíte, tento parametr, hello připojovací řetězec je pro konkrétní Centrum událostí "entita" a není správný řetězec toouse hello s svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="62700-126">If you find this parameter, hello connection string is for a specific Event Hub "entity", and is not hello correct string toouse with your logic app.</span></span>

4.  <span data-ttu-id="62700-127">Nyní když se zobrazí výzva k zadání pověření po přidání služby Event Hubs aktivační události nebo akce tooyour logiku aplikace, můžete připojit tooyour Event Hubs oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="62700-127">Now when you're prompted for credentials after adding an Event Hubs trigger or action tooyour logic app, you can connect tooyour Event Hubs namespace.</span></span> <span data-ttu-id="62700-128">Zadejte název připojení, zadejte hello připojovací řetězec, který jste zkopírovali a zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="62700-128">Give your connection a name, enter hello connection string that you copied, and choose **Create**.</span></span>

    ![Zadejte připojovací řetězec pro obor názvů služby Event Hubs](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    <span data-ttu-id="62700-130">Po vytvoření připojení, název připojení hello objevit v aktivační události Event Hubs hello nebo akce.</span><span class="sxs-lookup"><span data-stu-id="62700-130">After you create your connection, hello connection name should appear in hello Event Hubs trigger or action.</span></span> 
    <span data-ttu-id="62700-131">Poté můžete dál s hello další kroky v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="62700-131">You can then continue with hello other steps in your logic app.</span></span>

    ![Připojení obor názvů centra událostí vytvořit](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a><span data-ttu-id="62700-133">Spustit pracovní postup v případě, že Centrum událostí obdrží nové události</span><span class="sxs-lookup"><span data-stu-id="62700-133">Start workflow when your Event Hub receives new events</span></span>

<span data-ttu-id="62700-134">A [ *aktivační událost* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) je událost, která se spouští v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="62700-134">A [*trigger*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) is an event that starts a workflow in your logic app.</span></span> <span data-ttu-id="62700-135">Spuštění pracovního postupu odeslání nové události jsou tooyour centra událostí, postupujte podle těchto kroků pro přidání hello aktivační událost, která zjistí tuto událost.</span><span class="sxs-lookup"><span data-stu-id="62700-135">To start a workflow when new events are sent tooyour Event Hub, follow these steps for adding hello trigger that detects this event.</span></span>

1.  <span data-ttu-id="62700-136">V hello [portál Azure](https://portal.azure.com "portál Azure"), přejděte existující aplikace logiky tooyour nebo vytvoření aplikace logiky prázdné.</span><span class="sxs-lookup"><span data-stu-id="62700-136">In hello [Azure portal](https://portal.azure.com "Azure portal"), go tooyour existing logic app or create a blank logic app.</span></span>

2.  <span data-ttu-id="62700-137">Hello vyhledávacího pole pro hello návrhář aplikace logiky, zadejte `event hubs` filtru.</span><span class="sxs-lookup"><span data-stu-id="62700-137">In hello search box for hello Logic App Designer, enter `event hubs` for your filter.</span></span> <span data-ttu-id="62700-138">Vyberte této aktivační události: **když jsou události dostupné v Centru událostí**</span><span class="sxs-lookup"><span data-stu-id="62700-138">Select this trigger: **When events are available in Event Hub**</span></span>

    ![Vyberte aktivační událost pro Jakmile Centrum událostí obdrží nové události](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    <span data-ttu-id="62700-140">Pokud ještě nemáte obor názvů služby Event Hubs tooyour připojení, se zobrazí výzva toocreate nyní toto připojení.</span><span class="sxs-lookup"><span data-stu-id="62700-140">If you don't already have a connection tooyour Event Hubs namespace, you're prompted toocreate this connection now.</span></span> <span data-ttu-id="62700-141">Zadejte název připojení a zadejte hello připojovací řetězec pro obor názvů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="62700-141">Give your connection a name, and enter hello connection string for your Event Hubs namespace.</span></span> 
    <span data-ttu-id="62700-142">V případě potřeby další [jak toofind připojovací řetězec](#permissions-connection-string).</span><span class="sxs-lookup"><span data-stu-id="62700-142">If necessary, learn [how toofind your connection string](#permissions-connection-string).</span></span>

    ![Zadejte připojovací řetězec pro obor názvů služby Event Hubs](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    <span data-ttu-id="62700-144">Po vytvoření připojení hello hello nastavení pro hello **při událost v dostupné v Centru událostí** zobrazí aktivační události.</span><span class="sxs-lookup"><span data-stu-id="62700-144">After you create hello connection, hello settings for hello **When an event in available in an Event Hub** trigger appear.</span></span>

    ![Aktivační událost nastavení po Centrum událostí obdrží nové události](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  <span data-ttu-id="62700-146">Zadejte nebo vyberte název hello hello centra událostí, které chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="62700-146">Enter or select hello name for hello Event Hub that you want toomonitor.</span></span> <span data-ttu-id="62700-147">Vyberte hello frekvence a intervalu pro požadovanou četnost toocheck hello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="62700-147">Select hello frequency and interval for how often you want toocheck hello Event Hub.</span></span>

    > [!TIP]
    > <span data-ttu-id="62700-148">toooptionally vyberte skupinu uživatelů pro čtení událostí, zvolte **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="62700-148">toooptionally select a consumer group for reading events, choose **Show advanced options**.</span></span> 

    ![Zadejte centra událostí nebo skupiny uživatelů](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    <span data-ttu-id="62700-150">Jste nyní nastavili toostart aktivační událost pracovního postupu pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="62700-150">You've now set up a trigger toostart a workflow for your logic app.</span></span> 
    <span data-ttu-id="62700-151">Aplikace logiky jsou kontrolovány hello zadaný centra událostí podle nastaveného plánu hello.</span><span class="sxs-lookup"><span data-stu-id="62700-151">Your logic app checks hello specified Event Hub based on hello schedule that you set.</span></span> 
    <span data-ttu-id="62700-152">Pokud vaše aplikace najde nové události v hello centra událostí, aktivační události hello běží další akce nebo aktivační události v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="62700-152">If your app finds new events in hello Event Hub, hello trigger runs other actions or triggers in your logic app.</span></span>

## <a name="send-events-tooyour-event-hub-from-your-logic-app"></a><span data-ttu-id="62700-153">Odesílání událostí tooyour centra událostí z aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="62700-153">Send events tooyour Event Hub from your logic app</span></span>

<span data-ttu-id="62700-154">[*Akce*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) je úloha, kterou provádí pracovní postup vaší aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="62700-154">An [*action*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) is a task performed by your logic app workflow.</span></span> <span data-ttu-id="62700-155">Po přidání aplikace logiky tooyour aktivační událost, můžete přidat akci tooperform operace s daty generovanými této aktivační události.</span><span class="sxs-lookup"><span data-stu-id="62700-155">After you add a trigger tooyour logic app, you can add an action tooperform operations with data generated by that trigger.</span></span> <span data-ttu-id="62700-156">toosend událostí tooyour centra událostí z vaší aplikace logiky, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="62700-156">toosend an event tooyour Event Hub from your logic app, follow these steps.</span></span>

1.  <span data-ttu-id="62700-157">V návrháři aplikace logiky, vyberte v části vaší aktivační událostí aplikace logiky **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="62700-157">In Logic App Designer, under your logic app trigger, choose **New step** > **Add an action**.</span></span>

    ![Pak zvolte "Nový krok", "Přidat akci."](./media/connectors-create-api-azure-event-hubs/add-action.png)

    <span data-ttu-id="62700-159">Nyní můžete najít a vybrat tooperform akce.</span><span class="sxs-lookup"><span data-stu-id="62700-159">Now you can find and select an action tooperform.</span></span> 
    <span data-ttu-id="62700-160">I když můžete vybrat všechny akce, například chceme hello Event Hubs akce toosend události.</span><span class="sxs-lookup"><span data-stu-id="62700-160">Although you can select any action, for this example, we want hello Event Hubs action toosend events.</span></span>

2.  <span data-ttu-id="62700-161">Hello vyhledávacího pole zadejte `event hubs` filtru.</span><span class="sxs-lookup"><span data-stu-id="62700-161">In hello search box, enter `event hubs` for your filter.</span></span>
<span data-ttu-id="62700-162">Vyberte tuto akci: **odesílání událostí**</span><span class="sxs-lookup"><span data-stu-id="62700-162">Select this action: **Send event**</span></span>

    ![Vyberte "Event Hubs - odesílání událostí" akce](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  <span data-ttu-id="62700-164">Zadejte podrobnosti hello požadované hello události, jako je například název hello hello centra událostí, kde chcete toosend hello událost.</span><span class="sxs-lookup"><span data-stu-id="62700-164">Enter hello required details for hello event, such as hello name for hello Event Hub where you want toosend hello event.</span></span> <span data-ttu-id="62700-165">Zadejte další volitelné podrobnosti o hello události, jako je třeba obsah pro tuto událost.</span><span class="sxs-lookup"><span data-stu-id="62700-165">Enter any other optional details about hello event, such as content for that event.</span></span>

    > [!TIP]
    > <span data-ttu-id="62700-166">toooptionally zadejte oddílu centra událostí hello kde toosend hello událostí, zvolte **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="62700-166">toooptionally specify hello Event Hub partition where toosend hello event, choose **Show advanced options**.</span></span> 

    ![Zadejte název centra událostí a podrobnosti volitelné události](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  <span data-ttu-id="62700-168">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="62700-168">Save your changes.</span></span>

    ![Uložení aplikace logiky](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    <span data-ttu-id="62700-170">Jste nyní nastavili události toosend akce z vaší aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="62700-170">You've now set up an action toosend events from your logic app.</span></span> 

## <a name="connector-specific-details"></a><span data-ttu-id="62700-171">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="62700-171">Connector-specific details</span></span>

<span data-ttu-id="62700-172">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/eventhubs/).</span><span class="sxs-lookup"><span data-stu-id="62700-172">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/eventhubs/).</span></span> 

## <a name="get-help"></a><span data-ttu-id="62700-173">Podpora</span><span class="sxs-lookup"><span data-stu-id="62700-173">Get help</span></span>

<span data-ttu-id="62700-174">tooask otázky, odpovědi na dotazy a zjistit, jaké jiných uživatelů Azure Logic Apps dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="62700-174">tooask questions, answer questions, and see what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="62700-175">toohelp zlepšit konektory a aplikace logiky, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="62700-175">toohelp improve Logic Apps and connectors, vote on or submit ideas at hello [Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="62700-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62700-176">Next steps</span></span>

*  [<span data-ttu-id="62700-177">Najít další konektory pro Azure Logic apps</span><span class="sxs-lookup"><span data-stu-id="62700-177">Find other connectors for Azure Logic apps</span></span>](./apis-list.md)