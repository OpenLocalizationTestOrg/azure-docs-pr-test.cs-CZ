---
title: "Zpracování zpráv služby batch jako skupiny nebo kolekce - Azure Logic Apps | Microsoft Docs"
description: "Odesílat a přijímat zprávy pro dávkové zpracování v aplikacích logiky"
keywords: "dávkové zpracování balíku"
author: jonfancey
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/7/2017
ms.author: LADocs; estfan; jonfan
ms.openlocfilehash: 480ffce5dbe7c25181bb0ba5639de884e98ff4e6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a><span data-ttu-id="febf6-104">Odesílání, příjem a zpracování zpráv v aplikacích logiky služby batch</span><span class="sxs-lookup"><span data-stu-id="febf6-104">Send, receive, and batch process messages in logic apps</span></span>

<span data-ttu-id="febf6-105">Zpracování zpráv společně ve skupinách, můžete odeslat datových položek, nebo zprávy, na *batch*a pak tyto položky zpracovávat jako dávku.</span><span class="sxs-lookup"><span data-stu-id="febf6-105">To process messages together in groups, you can send data items, or messages, to a *batch*, and then process those items as a batch.</span></span> <span data-ttu-id="febf6-106">Tento přístup je užitečné, když chcete zkontrolujte, zda jsou seskupené v určitým způsobem datové položky a společném zpracování.</span><span class="sxs-lookup"><span data-stu-id="febf6-106">This approach is useful when you want to make sure data items are grouped in a specific way and are processed together.</span></span> 

<span data-ttu-id="febf6-107">Můžete vytvořit logiku aplikace, které zobrazí položky jako dávce pomocí **Batch** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="febf6-107">You can create logic apps that receive items as a batch by using the **Batch** trigger.</span></span> <span data-ttu-id="febf6-108">Potom můžete vytvořit logiku aplikace, které položky poslat dávce pomocí **Batch** akce.</span><span class="sxs-lookup"><span data-stu-id="febf6-108">You can then create logic apps that send items to a batch by using the **Batch** action.</span></span>

<span data-ttu-id="febf6-109">Toto téma ukazuje, jak můžete vytvořit dávkování řešení provedením těchto úkolů:</span><span class="sxs-lookup"><span data-stu-id="febf6-109">This topic shows how you can build a batching solution by performing these tasks:</span></span> 

* <span data-ttu-id="febf6-110">[Vytvoření aplikace logiky, která přijímá a shromažďuje položky jako dávku](#batch-receiver).</span><span class="sxs-lookup"><span data-stu-id="febf6-110">[Create a logic app that receives and collects items as a batch](#batch-receiver).</span></span> <span data-ttu-id="febf6-111">Tato aplikace logiky "batch příjemce", určuje kritéria batch název a verze pro splnění předtím, než aplikaci logiky příjemce uvolní a zpracovává položky.</span><span class="sxs-lookup"><span data-stu-id="febf6-111">This "batch receiver" logic app specifies the batch name and release criteria to meet before the receiver logic app releases and processes items.</span></span> 

* <span data-ttu-id="febf6-112">[Vytvoření aplikace logiky, která odesílá položky do dávky](#batch-sender).</span><span class="sxs-lookup"><span data-stu-id="febf6-112">[Create a logic app that sends items to a batch](#batch-sender).</span></span> <span data-ttu-id="febf6-113">Tato aplikace logiky "batch sender" Určuje, kde k odeslání položek, které musí být existující aplikace logiky příjemce batch.</span><span class="sxs-lookup"><span data-stu-id="febf6-113">This "batch sender" logic app specifies where to send items, which must be an existing batch receiver logic app.</span></span> <span data-ttu-id="febf6-114">Můžete také zadat jedinečný klíč, jako je číslo zákazníka, "oddílu" nebo rozdělte cílový batch do podmnožin na základě tohoto klíče.</span><span class="sxs-lookup"><span data-stu-id="febf6-114">You can also specify a unique key, like a customer number, to "partition", or divide, the target batch into subsets based on that key.</span></span> <span data-ttu-id="febf6-115">Tímto způsobem, všechny položky s tímto klíčem sběru a zpracování společně.</span><span class="sxs-lookup"><span data-stu-id="febf6-115">That way, all items with that key are collected and processed together.</span></span> 

## <a name="requirements"></a><span data-ttu-id="febf6-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="febf6-116">Requirements</span></span>

<span data-ttu-id="febf6-117">Postup popsaný v tomto příkladu, budete potřebovat tyto položky:</span><span class="sxs-lookup"><span data-stu-id="febf6-117">To follow this example, you need these items:</span></span>

* <span data-ttu-id="febf6-118">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="febf6-118">An Azure subscription.</span></span> <span data-ttu-id="febf6-119">Pokud předplatné nemáte, můžete [začít s bezplatným účtem Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="febf6-119">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="febf6-120">Jinak si můžete [zaregistrovat předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="febf6-120">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

* <span data-ttu-id="febf6-121">Základní znalosti o [postup vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)</span><span class="sxs-lookup"><span data-stu-id="febf6-121">Basic knowledge about [how to create logic apps](../logic-apps/logic-apps-create-a-logic-app.md)</span></span> 

* <span data-ttu-id="febf6-122">E-mailový účet s žádným [e-mailu zprostředkovatele nepodporuje Azure Logic Apps](../connectors/apis-list.md)</span><span class="sxs-lookup"><span data-stu-id="febf6-122">An email account with any [email provider supported by Azure Logic Apps](../connectors/apis-list.md)</span></span>

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a><span data-ttu-id="febf6-123">Vytvoření aplikace logiky, které přijímají zprávy jako dávky</span><span class="sxs-lookup"><span data-stu-id="febf6-123">Create logic apps that receive messages as a batch</span></span>

<span data-ttu-id="febf6-124">Předtím, než mohou zasílat zprávy do dávky, je nutné nejprve vytvořit aplikace logiky "batch příjemce" s **Batch** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="febf6-124">Before you can send messages to a batch, you must first create a "batch receiver" logic app with the **Batch** trigger.</span></span> <span data-ttu-id="febf6-125">Tímto způsobem, tato aplikace logiky příjemce můžete vybrat, když vytvoříte aplikaci logiky odesílatele.</span><span class="sxs-lookup"><span data-stu-id="febf6-125">That way, you can select this receiver logic app when you create the sender logic app.</span></span> <span data-ttu-id="febf6-126">Pro příjemce je třeba zadat název batch, verze kritéria a další nastavení.</span><span class="sxs-lookup"><span data-stu-id="febf6-126">For the receiver, you specify the batch name, release criteria, and other settings.</span></span> 

<span data-ttu-id="febf6-127">Aplikace logiky odesílatele potřebovat věděli, kde k odeslání položek, zatímco aplikace logiky příjemce nepotřebujete znát odesílatelé.</span><span class="sxs-lookup"><span data-stu-id="febf6-127">Sender logic apps need know where to send items, while receiver logic apps don't need to know anything about the senders.</span></span>

1. <span data-ttu-id="febf6-128">V [portál Azure](https://portal.azure.com), vytvoření aplikace logiky s tímto názvem: "BatchReceiver"</span><span class="sxs-lookup"><span data-stu-id="febf6-128">In the [Azure portal](https://portal.azure.com), create a logic app with this name: "BatchReceiver"</span></span> 

2. <span data-ttu-id="febf6-129">V návrháři aplikace logiky, přidejte **Batch** aktivační události, které spustí pracovní postup aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="febf6-129">In Logic Apps Designer, add the **Batch** trigger, which starts your logic app workflow.</span></span> <span data-ttu-id="febf6-130">Do vyhledávacího pole zadejte "batch" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="febf6-130">In the search box, enter "batch" as your filter.</span></span> <span data-ttu-id="febf6-131">Vyberte této aktivační události: **Batch – zprávy dávkových**</span><span class="sxs-lookup"><span data-stu-id="febf6-131">Select this trigger: **Batch – Batch messages**</span></span>

   ![Přidat aktivační událost Batch](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. <span data-ttu-id="febf6-133">Zadejte název pro dávku a zadejte kritéria pro uvolnění batch, například:</span><span class="sxs-lookup"><span data-stu-id="febf6-133">Provide a name for the batch, and specify criteria for releasing the batch, for example:</span></span>

   * <span data-ttu-id="febf6-134">**Název služby batch**: název sloužící k identifikaci dávky, která je "TestBatch" v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="febf6-134">**Batch Name**: The name used to identify the batch, which is "TestBatch" in this example.</span></span>
   * <span data-ttu-id="febf6-135">**Počet zpráv**: počet zpráv pro uložení jako dávku před uvolněním pro zpracování, což je "5" v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="febf6-135">**Message Count**: The number of messages to hold as a batch before releasing for processing, which is "5" in this example.</span></span>

   ![Zadejte podrobnosti dávky aktivační události](./media/logic-apps-batch-process-send-receive-messages/receive-batch-trigger-details.png)

4. <span data-ttu-id="febf6-137">Přidáte akci, která se pošle e-mailu, když se aktivuje batch aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="febf6-137">Add another action that sends an email when the batch trigger fires.</span></span> <span data-ttu-id="febf6-138">Pokaždé, když dávky má pět položek, aplikace logiky odešle e-mail.</span><span class="sxs-lookup"><span data-stu-id="febf6-138">Each time the batch has five items, the logic app sends an email.</span></span>

   1. <span data-ttu-id="febf6-139">V části aktivační událost batch, zvolte **+ nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="febf6-139">Under the batch trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="febf6-140">Do vyhledávacího pole zadejte "e-mailu" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="febf6-140">In the search box, enter "email" as your filter.</span></span>
   <span data-ttu-id="febf6-141">Založená na zprostředkovateli e-mailu, vyberte konektor e-mailu.</span><span class="sxs-lookup"><span data-stu-id="febf6-141">Based on your email provider, select an email connector.</span></span>
   
      <span data-ttu-id="febf6-142">Například pokud máte pracovní nebo školní účet, vyberte konektor Office 365 Outlook.</span><span class="sxs-lookup"><span data-stu-id="febf6-142">For example, if you have a work or school account, select the Office 365 Outlook connector.</span></span> 
      <span data-ttu-id="febf6-143">Pokud máte účet Gmail, vyberte konektor z Gmailu.</span><span class="sxs-lookup"><span data-stu-id="febf6-143">If you have a Gmail account, select the Gmail connector.</span></span>

   3. <span data-ttu-id="febf6-144">Vyberte tuto akci pro vaše konektor:  **{*poskytovatele e-mailu*}-odeslat e-mailu **</span><span class="sxs-lookup"><span data-stu-id="febf6-144">Select this action for your connector: **{*email provider*} - Send an email**</span></span>

      <span data-ttu-id="febf6-145">Například:</span><span class="sxs-lookup"><span data-stu-id="febf6-145">For example:</span></span>

      ![Vyberte možnost odeslat e-mail panel. pro váš poskytovatel e-mailu](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. <span data-ttu-id="febf6-147">Pokud jste nevytvořili dříve připojení pro poskytovatele e-mailu, zadejte svoje přihlašovací údaje e-mailu pro ověřování při zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="febf6-147">If you didn't previously create a connection for your email provider, provide your email credentials for authentication when prompted.</span></span> <span data-ttu-id="febf6-148">Další informace o [ověřování přihlašovacích údajů e-mailu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="febf6-148">Learn more about [authenticating your email credentials](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

6. <span data-ttu-id="febf6-149">Nastavte vlastnosti pro akci, kterou jste právě přidali.</span><span class="sxs-lookup"><span data-stu-id="febf6-149">Set the properties for the action you just added.</span></span>

   * <span data-ttu-id="febf6-150">V **k** zadejte příjemce e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="febf6-150">In the **To** box, enter the recipient's email address.</span></span> 
   <span data-ttu-id="febf6-151">Pro účely testování můžete použít vlastní e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="febf6-151">For testing purposes, you can use your own email address.</span></span>

   * <span data-ttu-id="febf6-152">V **subjektu** pole, když **dynamický obsah** se zobrazí seznam, vyberte **název oddílu** pole.</span><span class="sxs-lookup"><span data-stu-id="febf6-152">In the **Subject** box, when the **Dynamic content** list appears, select the **Partition Name** field.</span></span>

     ![Ze seznamu "Dynamický obsah" Vyberte "Název oddílu"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     <span data-ttu-id="febf6-154">V další části můžete zadat klíč jedinečný oddílu, který rozděluje batch cíl do logické sad, kde můžete odesílat zprávy.</span><span class="sxs-lookup"><span data-stu-id="febf6-154">In a later section, you can specify a unique partition key that divides the target batch into logical sets to where you can send messages.</span></span> 
     <span data-ttu-id="febf6-155">Každá sada má jedinečné číslo, které je generován aplikace logiky odesílatele.</span><span class="sxs-lookup"><span data-stu-id="febf6-155">Each set has a unique number that's generated by the sender logic app.</span></span> 
     <span data-ttu-id="febf6-156">Tato funkce umožňuje používat jeden batch s více podmnožin a definovat každý podmnožina s názvem, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="febf6-156">This capability lets you use a single batch with multiple subsets and define each subset with the name that you provide.</span></span>

   * <span data-ttu-id="febf6-157">V **textu** pole, když **dynamický obsah** se zobrazí seznam, vyberte **Id zprávy** pole.</span><span class="sxs-lookup"><span data-stu-id="febf6-157">In the **Body** box, when the **Dynamic content** list appears, select the **Message Id** field.</span></span>

     !["Text" Vyberte možnost "Id zprávy"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     <span data-ttu-id="febf6-159">Vstup pro odeslání e-mailu akce je pole, a proto návrháře automaticky přidá **pro každou** cykly kolem **e-mailovou zprávu** akce.</span><span class="sxs-lookup"><span data-stu-id="febf6-159">Because the input for the send email action is an array, the designer automatically adds a **For each** loop around the **Send an email** action.</span></span> 
     <span data-ttu-id="febf6-160">Tato smyčka provádí vnitřní akce na každou položku v dávce.</span><span class="sxs-lookup"><span data-stu-id="febf6-160">This loop performs the inner action on each item in the batch.</span></span> 
     <span data-ttu-id="febf6-161">Aby se aktivační událostí batch nastavena na pět položek, můžete získat čas pět e-maily aktivační událost je aktivována.</span><span class="sxs-lookup"><span data-stu-id="febf6-161">So, with the batch trigger set to five items, you get five emails each time the trigger fires.</span></span>

7.  <span data-ttu-id="febf6-162">Vytvoření aplikace logiky příjemce batch, uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="febf6-162">Now that you created a batch receiver logic app, save your logic app.</span></span>

    ![Uložení aplikace logiky](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-to-a-batch"></a><span data-ttu-id="febf6-164">Vytvoření aplikace logiky, které odesílají zprávy do dávky</span><span class="sxs-lookup"><span data-stu-id="febf6-164">Create logic apps that send messages to a batch</span></span>

<span data-ttu-id="febf6-165">Teď vytvořte jeden nebo více aplikace logiky, které odesílají položky do dávky definované aplikace logiky příjemce.</span><span class="sxs-lookup"><span data-stu-id="febf6-165">Now create one or more logic apps that send items to the batch defined by the receiver logic app.</span></span> <span data-ttu-id="febf6-166">Pro odesílatele je třeba zadat aplikaci logiky příjemce a název batch, obsah zprávy a další nastavení.</span><span class="sxs-lookup"><span data-stu-id="febf6-166">For the sender, you specify the receiver logic app and batch name, message content, and any other settings.</span></span> <span data-ttu-id="febf6-167">Volitelně můžete zadat klíč oddílu jedinečný k rozdělení dávky do podmnožin shromažďovat položky s tímto klíčem.</span><span class="sxs-lookup"><span data-stu-id="febf6-167">You can optionally provide a unique partition key to divide the batch into subsets to collect items with that key.</span></span>

<span data-ttu-id="febf6-168">Aplikace logiky odesílatele potřebovat věděli, kde k odeslání položek, zatímco aplikace logiky příjemce nepotřebujete znát odesílatelé.</span><span class="sxs-lookup"><span data-stu-id="febf6-168">Sender logic apps need know where to send items, while receiver logic apps don't need to know anything about the senders.</span></span>

1. <span data-ttu-id="febf6-169">Vytvořit jinou aplikaci logiky s tímto názvem: "BatchSender"</span><span class="sxs-lookup"><span data-stu-id="febf6-169">Create another logic app with this name: "BatchSender"</span></span>

   1. <span data-ttu-id="febf6-170">Do vyhledávacího pole zadejte "recurrence" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="febf6-170">In the search box, enter "recurrence" as your filter.</span></span> 
   <span data-ttu-id="febf6-171">Vyberte této aktivační události: **plán - opakování**</span><span class="sxs-lookup"><span data-stu-id="febf6-171">Select this trigger: **Schedule - Recurrence**</span></span>

      ![Přidat aktivační události "Plán-Recurrence"](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. <span data-ttu-id="febf6-173">Nastavení frekvence a intervalu spuštění odesílatel aplikace logiky každou minutu.</span><span class="sxs-lookup"><span data-stu-id="febf6-173">Set the frequency and interval to run the sender logic app every minute.</span></span>

      ![Nastavení frekvence a intervalu opakování aktivační události](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. <span data-ttu-id="febf6-175">Přidejte nový krok pro odesílání zpráv do dávky.</span><span class="sxs-lookup"><span data-stu-id="febf6-175">Add a new step for sending messages to a batch.</span></span>

   1. <span data-ttu-id="febf6-176">V části aktivační událost opakování, zvolte **+ nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="febf6-176">Under the recurrence trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="febf6-177">Do vyhledávacího pole zadejte "batch" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="febf6-177">In the search box, enter "batch" as your filter.</span></span> 

   3. <span data-ttu-id="febf6-178">Vyberte tuto akci: **odesílání zpráv do dávky – zvolte Logic Apps pracovního postupu se aktivační událostí batch**</span><span class="sxs-lookup"><span data-stu-id="febf6-178">Select this action: **Send messages to batch – Choose a Logic Apps workflow with batch trigger**</span></span>

      ![Vyberte "Odesílání zpráv do dávky"](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. <span data-ttu-id="febf6-180">Nyní vyberte svou aplikaci logiky "BatchReceiver", který jste dříve vytvořili, které nyní se zobrazí jako akce.</span><span class="sxs-lookup"><span data-stu-id="febf6-180">Now select your "BatchReceiver" logic app that you previously created, which now appears as an action.</span></span>

      ![Vyberte aplikaci logiky "batch příjemce"](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > <span data-ttu-id="febf6-182">V seznamu jsou také uvedeny všechny ostatní aplikace logiky, které mají batch aktivační události.</span><span class="sxs-lookup"><span data-stu-id="febf6-182">The list also shows any other logic apps that have batch triggers.</span></span>

3. <span data-ttu-id="febf6-183">Nastavte vlastnosti dávky.</span><span class="sxs-lookup"><span data-stu-id="febf6-183">Set the batch properties.</span></span>

   * <span data-ttu-id="febf6-184">**Název služby batch**: název dávkového definované aplikace logiky příjemce, který je v tomto příkladu "TestBatch" a byl ověřen za běhu.</span><span class="sxs-lookup"><span data-stu-id="febf6-184">**Batch Name**: The batch name defined by the receiver logic app, which is "TestBatch" in this example and is validated at runtime.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="febf6-185">Ujistěte se, že neměnit název batch, které se musí shodovat s názvem dávky, která je zadána aplikace logiky příjemce.</span><span class="sxs-lookup"><span data-stu-id="febf6-185">Make sure that you don't change the batch name, which must match the batch name that's specified by the receiver logic app.</span></span>
     > <span data-ttu-id="febf6-186">Změna názvu batch způsobí, že odesílatel aplikace logiky k selhání.</span><span class="sxs-lookup"><span data-stu-id="febf6-186">Changing the batch name causes the sender logic app to fail.</span></span>

   * <span data-ttu-id="febf6-187">**Obsah zprávy**: zpráva obsah, který chcete odeslat.</span><span class="sxs-lookup"><span data-stu-id="febf6-187">**Message Content**: The message content that you want to send.</span></span> 
   <span data-ttu-id="febf6-188">V tomto příkladu přidejte tento výraz, který se vloží do obsah zprávy, který odešlete do dávky k aktuálnímu datu a času:</span><span class="sxs-lookup"><span data-stu-id="febf6-188">For this example, add this expression that inserts the current date and time into the message content that you send to the batch:</span></span>

     1. <span data-ttu-id="febf6-189">Když **dynamický obsah** seznamu se zobrazí, zvolte **výraz**.</span><span class="sxs-lookup"><span data-stu-id="febf6-189">When the **Dynamic content** list appears, choose **Expression**.</span></span> 
     2. <span data-ttu-id="febf6-190">Zadejte výraz **utcnow()**a zvolte **OK**.</span><span class="sxs-lookup"><span data-stu-id="febf6-190">Enter the expression **utcnow()**, and choose **OK**.</span></span> 

        ![V "Zpráva obsah" Vyberte "Výraz".](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. <span data-ttu-id="febf6-193">Nyní nastavte oddíl pro dávku.</span><span class="sxs-lookup"><span data-stu-id="febf6-193">Now set up a partition for the batch.</span></span> <span data-ttu-id="febf6-194">V akci "BatchReceiver", zvolte **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="febf6-194">In the "BatchReceiver" action, choose **Show advanced options**.</span></span>

   * <span data-ttu-id="febf6-195">**Název oddílu**: volitelné oddílu jedinečný klíč pro použití pro dělení batch cíl.</span><span class="sxs-lookup"><span data-stu-id="febf6-195">**Partition Name**: An optional unique partition key to use for dividing the target batch.</span></span> <span data-ttu-id="febf6-196">V tomto příkladu přidáte výraz, který generuje náhodné číslo v intervalu jeden a pět.</span><span class="sxs-lookup"><span data-stu-id="febf6-196">For this example, add an expression that generates a random number between one and five.</span></span>
   
     1. <span data-ttu-id="febf6-197">Když **dynamický obsah** seznamu se zobrazí, zvolte **výraz**.</span><span class="sxs-lookup"><span data-stu-id="febf6-197">When the **Dynamic content** list appears, choose **Expression**.</span></span>
     2. <span data-ttu-id="febf6-198">Zadejte tento výraz: **rand(1,6)**</span><span class="sxs-lookup"><span data-stu-id="febf6-198">Enter this expression: **rand(1,6)**</span></span>

        ![Nastavení pro dávku cílový oddíl](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        <span data-ttu-id="febf6-200">To **rand –** funkce generuje v rozmezí jednu až pět.</span><span class="sxs-lookup"><span data-stu-id="febf6-200">This **rand** function generates a number between one and five.</span></span> 
        <span data-ttu-id="febf6-201">Proto jsou dělení tuto dávku do pěti číslem oddílů, které dynamicky nastaví tento výraz.</span><span class="sxs-lookup"><span data-stu-id="febf6-201">So you are dividing this batch into five numbered partitions, which this expression dynamically sets.</span></span>

   * <span data-ttu-id="febf6-202">**Id zprávy**: identifikátor volitelné zpráv a je vytvářenému identifikátoru GUID, pokud je prázdný.</span><span class="sxs-lookup"><span data-stu-id="febf6-202">**Message Id**: An optional message identifier and is a generated GUID when empty.</span></span> 
   <span data-ttu-id="febf6-203">V tomto příkladu ponechte toto pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="febf6-203">For this example, leave this box blank.</span></span>

5. <span data-ttu-id="febf6-204">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="febf6-204">Save your logic app.</span></span> <span data-ttu-id="febf6-205">Aplikace logiky odesílatele nyní vypadá podobně jako tento příklad:</span><span class="sxs-lookup"><span data-stu-id="febf6-205">Your sender logic app now looks similar to this example:</span></span>

   ![Uložte svou aplikaci logiky odesílatele](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a><span data-ttu-id="febf6-207">Testování aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="febf6-207">Test your logic apps</span></span>

<span data-ttu-id="febf6-208">Chcete-li otestovat dávkování řešení, ponechejte aplikace logiky spuštěná pro několik minut.</span><span class="sxs-lookup"><span data-stu-id="febf6-208">To test your batching solution, leave your logic apps running for a few minutes.</span></span> <span data-ttu-id="febf6-209">Později, brzy spuštění získání e-mailů v 5, všechny se stejným klíčem oddílu skupiny.</span><span class="sxs-lookup"><span data-stu-id="febf6-209">Soon, you start getting emails in groups of five, all with the same partition key.</span></span>

<span data-ttu-id="febf6-210">Aplikace logiky BatchSender spouští každou minutu, vygeneruje náhodné číslo v intervalu jeden a 5 a používá toto generované číslo jako klíč oddílu pro dávku cíl, které jsou odesílány zprávy.</span><span class="sxs-lookup"><span data-stu-id="febf6-210">Your BatchSender logic app runs every minute, generates a random number between one and five, and uses this generated number as the partition key for the target batch where messages are sent.</span></span> <span data-ttu-id="febf6-211">Pokaždé, když dávky má pět položek se stejným klíčem oddílu aplikace logiky BatchReceiver aktivuje a odešle e-mailu pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="febf6-211">Each time the batch has five items with the same partition key, your BatchReceiver logic app fires and sends mail for each message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="febf6-212">Po dokončení testování, ujistěte se, že zakážete aplikace logiky BatchSender zastavit odesílání zpráv a předcházeli přetížení vaší doručené poště.</span><span class="sxs-lookup"><span data-stu-id="febf6-212">When you're done testing, make sure that you disable the BatchSender logic app to stop sending messages and avoid overloading your inbox.</span></span>

## <a name="next-steps"></a><span data-ttu-id="febf6-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="febf6-213">Next steps</span></span>

* [<span data-ttu-id="febf6-214">Sestavení na definice aplikace logiky pomocí JSON</span><span class="sxs-lookup"><span data-stu-id="febf6-214">Build on logic app definitions by using JSON</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="febf6-215">Sestavení bez serveru aplikace v sadě Visual Studio s funkcemi a Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="febf6-215">Build a serverless app in Visual Studio with Azure Logic Apps and Functions</span></span>](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [<span data-ttu-id="febf6-216">Zpracování výjimek a protokolování chyb pro logic apps</span><span class="sxs-lookup"><span data-stu-id="febf6-216">Exception handling and error logging for logic apps</span></span>](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
