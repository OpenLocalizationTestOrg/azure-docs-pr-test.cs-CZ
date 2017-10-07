---
title: "zpracování zpráv aaaBatch jako skupiny nebo kolekce - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 2603db71ee0659d5b6bf5ce3d32f1b0d13c34194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a><span data-ttu-id="1b660-104">Odesílání, příjem a zpracování zpráv v aplikacích logiky služby batch</span><span class="sxs-lookup"><span data-stu-id="1b660-104">Send, receive, and batch process messages in logic apps</span></span>

<span data-ttu-id="1b660-105">zprávy tooprocess společně ve skupinách, může odesílat data položky nebo zprávy, tooa *batch*a pak tyto položky zpracovávat jako dávku.</span><span class="sxs-lookup"><span data-stu-id="1b660-105">tooprocess messages together in groups, you can send data items, or messages, tooa *batch*, and then process those items as a batch.</span></span> <span data-ttu-id="1b660-106">Tento přístup je užitečné, když chcete toomake zda datové položky jsou seskupené v určitým způsobem a společném zpracování.</span><span class="sxs-lookup"><span data-stu-id="1b660-106">This approach is useful when you want toomake sure data items are grouped in a specific way and are processed together.</span></span> 

<span data-ttu-id="1b660-107">Můžete vytvořit logiku aplikace, které zobrazí položky jako dávce pomocí hello **Batch** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="1b660-107">You can create logic apps that receive items as a batch by using hello **Batch** trigger.</span></span> <span data-ttu-id="1b660-108">Potom můžete vytvořit logiku aplikace, které odesílání položek tooa batch pomocí hello **Batch** akce.</span><span class="sxs-lookup"><span data-stu-id="1b660-108">You can then create logic apps that send items tooa batch by using hello **Batch** action.</span></span>

<span data-ttu-id="1b660-109">Toto téma ukazuje, jak můžete vytvořit dávkování řešení provedením těchto úkolů:</span><span class="sxs-lookup"><span data-stu-id="1b660-109">This topic shows how you can build a batching solution by performing these tasks:</span></span> 

* <span data-ttu-id="1b660-110">[Vytvoření aplikace logiky, která přijímá a shromažďuje položky jako dávku](#batch-receiver).</span><span class="sxs-lookup"><span data-stu-id="1b660-110">[Create a logic app that receives and collects items as a batch](#batch-receiver).</span></span> <span data-ttu-id="1b660-111">Tato aplikace logiky "batch příjemce" Určuje hello batch název a verzi kritéria toomeet předtím, než aplikace logiky příjemce hello uvolní a zpracovává položky.</span><span class="sxs-lookup"><span data-stu-id="1b660-111">This "batch receiver" logic app specifies hello batch name and release criteria toomeet before hello receiver logic app releases and processes items.</span></span> 

* <span data-ttu-id="1b660-112">[Vytvoření aplikace logiky, který odesílá položky tooa batch](#batch-sender).</span><span class="sxs-lookup"><span data-stu-id="1b660-112">[Create a logic app that sends items tooa batch](#batch-sender).</span></span> <span data-ttu-id="1b660-113">Tato aplikace logiky "batch sender" Určuje, kde toosend položek, které musí být existující aplikace logiky příjemce batch.</span><span class="sxs-lookup"><span data-stu-id="1b660-113">This "batch sender" logic app specifies where toosend items, which must be an existing batch receiver logic app.</span></span> <span data-ttu-id="1b660-114">Můžete také zadat jedinečný klíč, jako je číslo zákazníka, příliš "oddílu" nebo rozdělte, hello cíl batch do podmnožin na základě tohoto klíče.</span><span class="sxs-lookup"><span data-stu-id="1b660-114">You can also specify a unique key, like a customer number, too"partition", or divide, hello target batch into subsets based on that key.</span></span> <span data-ttu-id="1b660-115">Tímto způsobem, všechny položky s tímto klíčem sběru a zpracování společně.</span><span class="sxs-lookup"><span data-stu-id="1b660-115">That way, all items with that key are collected and processed together.</span></span> 

## <a name="requirements"></a><span data-ttu-id="1b660-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1b660-116">Requirements</span></span>

<span data-ttu-id="1b660-117">toofollow v tomto příkladu budete potřebovat tyto položky:</span><span class="sxs-lookup"><span data-stu-id="1b660-117">toofollow this example, you need these items:</span></span>

* <span data-ttu-id="1b660-118">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="1b660-118">An Azure subscription.</span></span> <span data-ttu-id="1b660-119">Pokud předplatné nemáte, můžete [začít s bezplatným účtem Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="1b660-119">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="1b660-120">Jinak si můžete [zaregistrovat předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="1b660-120">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

* <span data-ttu-id="1b660-121">Základní znalosti o [jak toocreate logic apps](../logic-apps/logic-apps-create-a-logic-app.md)</span><span class="sxs-lookup"><span data-stu-id="1b660-121">Basic knowledge about [how toocreate logic apps](../logic-apps/logic-apps-create-a-logic-app.md)</span></span> 

* <span data-ttu-id="1b660-122">E-mailový účet s žádným [e-mailu zprostředkovatele nepodporuje Azure Logic Apps](../connectors/apis-list.md)</span><span class="sxs-lookup"><span data-stu-id="1b660-122">An email account with any [email provider supported by Azure Logic Apps](../connectors/apis-list.md)</span></span>

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a><span data-ttu-id="1b660-123">Vytvoření aplikace logiky, které přijímají zprávy jako dávky</span><span class="sxs-lookup"><span data-stu-id="1b660-123">Create logic apps that receive messages as a batch</span></span>

<span data-ttu-id="1b660-124">Před odesláním zprávy tooa batch, je nutné nejprve vytvořit aplikace logiky "batch příjemce" s hello **Batch** aktivační události.</span><span class="sxs-lookup"><span data-stu-id="1b660-124">Before you can send messages tooa batch, you must first create a "batch receiver" logic app with hello **Batch** trigger.</span></span> <span data-ttu-id="1b660-125">Tímto způsobem tuto aplikaci logiky příjemce můžete vybrat, když vytvoříte aplikaci logiky odesílatele hello.</span><span class="sxs-lookup"><span data-stu-id="1b660-125">That way, you can select this receiver logic app when you create hello sender logic app.</span></span> <span data-ttu-id="1b660-126">Pro hello příjemce zadejte název dávkového hello, verze kritéria a další nastavení.</span><span class="sxs-lookup"><span data-stu-id="1b660-126">For hello receiver, you specify hello batch name, release criteria, and other settings.</span></span> 

<span data-ttu-id="1b660-127">Aplikace logiky odesílatele potřebovat vědět, kde toosend položky, zatímco aplikace logiky příjemce nepotřebujete tooknow cokoli o odesílatelé hello.</span><span class="sxs-lookup"><span data-stu-id="1b660-127">Sender logic apps need know where toosend items, while receiver logic apps don't need tooknow anything about hello senders.</span></span>

1. <span data-ttu-id="1b660-128">V hello [portál Azure](https://portal.azure.com), vytvoření aplikace logiky s tímto názvem: "BatchReceiver"</span><span class="sxs-lookup"><span data-stu-id="1b660-128">In hello [Azure portal](https://portal.azure.com), create a logic app with this name: "BatchReceiver"</span></span> 

2. <span data-ttu-id="1b660-129">V návrháři aplikace logiky, přidejte hello **Batch** aktivační události, které spustí pracovní postup aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="1b660-129">In Logic Apps Designer, add hello **Batch** trigger, which starts your logic app workflow.</span></span> <span data-ttu-id="1b660-130">Hello vyhledávacího pole zadejte "batch" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="1b660-130">In hello search box, enter "batch" as your filter.</span></span> <span data-ttu-id="1b660-131">Vyberte této aktivační události: **Batch – zprávy dávkových**</span><span class="sxs-lookup"><span data-stu-id="1b660-131">Select this trigger: **Batch – Batch messages**</span></span>

   ![Přidat aktivační událost Batch](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. <span data-ttu-id="1b660-133">Zadejte název pro dávku hello a zadejte kritéria pro uvolnění hello batch, například:</span><span class="sxs-lookup"><span data-stu-id="1b660-133">Provide a name for hello batch, and specify criteria for releasing hello batch, for example:</span></span>

   * <span data-ttu-id="1b660-134">**Název služby batch**: hello název používaný tooidentify hello batch, což je "TestBatch" v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="1b660-134">**Batch Name**: hello name used tooidentify hello batch, which is "TestBatch" in this example.</span></span>
   * <span data-ttu-id="1b660-135">**Počet zpráv**: hello počet zpráv toohold jako dávku před uvolněním pro zpracování, což je "5" v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="1b660-135">**Message Count**: hello number of messages toohold as a batch before releasing for processing, which is "5" in this example.</span></span>

   ![Zadejte podrobnosti dávky aktivační události](./media/logic-apps-batch-process-send-receive-messages/receive-batch-trigger-details.png)

4. <span data-ttu-id="1b660-137">Přidáte akci, která se pošle e-mailu, když se aktivuje hello batch aktivační události.</span><span class="sxs-lookup"><span data-stu-id="1b660-137">Add another action that sends an email when hello batch trigger fires.</span></span> <span data-ttu-id="1b660-138">Každé dávky hello čas má pět položek, aplikace logiky hello odešle e-mail.</span><span class="sxs-lookup"><span data-stu-id="1b660-138">Each time hello batch has five items, hello logic app sends an email.</span></span>

   1. <span data-ttu-id="1b660-139">V části hello batch aktivační událost, zvolte **+ nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="1b660-139">Under hello batch trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="1b660-140">Hello vyhledávacího pole zadejte "e-mailu" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="1b660-140">In hello search box, enter "email" as your filter.</span></span>
   <span data-ttu-id="1b660-141">Založená na zprostředkovateli e-mailu, vyberte konektor e-mailu.</span><span class="sxs-lookup"><span data-stu-id="1b660-141">Based on your email provider, select an email connector.</span></span>
   
      <span data-ttu-id="1b660-142">Například pokud máte pracovní nebo školní účet, vyberte konektor Office 365 Outlook hello.</span><span class="sxs-lookup"><span data-stu-id="1b660-142">For example, if you have a work or school account, select hello Office 365 Outlook connector.</span></span> 
      <span data-ttu-id="1b660-143">Pokud máte účet Gmail, vyberte konektor z Gmailu hello.</span><span class="sxs-lookup"><span data-stu-id="1b660-143">If you have a Gmail account, select hello Gmail connector.</span></span>

   3. <span data-ttu-id="1b660-144">Vyberte tuto akci pro vaše konektor:  **{*poskytovatele e-mailu*}-odeslat e-mailu **</span><span class="sxs-lookup"><span data-stu-id="1b660-144">Select this action for your connector: **{*email provider*} - Send an email**</span></span>

      <span data-ttu-id="1b660-145">Například:</span><span class="sxs-lookup"><span data-stu-id="1b660-145">For example:</span></span>

      ![Vyberte možnost odeslat e-mail panel. pro váš poskytovatel e-mailu](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. <span data-ttu-id="1b660-147">Pokud jste nevytvořili dříve připojení pro poskytovatele e-mailu, zadejte svoje přihlašovací údaje e-mailu pro ověřování při zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="1b660-147">If you didn't previously create a connection for your email provider, provide your email credentials for authentication when prompted.</span></span> <span data-ttu-id="1b660-148">Další informace o [ověřování přihlašovacích údajů e-mailu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1b660-148">Learn more about [authenticating your email credentials](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

6. <span data-ttu-id="1b660-149">Nastavte vlastnosti hello hello akci, kterou jste právě přidali.</span><span class="sxs-lookup"><span data-stu-id="1b660-149">Set hello properties for hello action you just added.</span></span>

   * <span data-ttu-id="1b660-150">V hello **k** zadejte hello příjemce e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="1b660-150">In hello **To** box, enter hello recipient's email address.</span></span> 
   <span data-ttu-id="1b660-151">Pro účely testování můžete použít vlastní e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="1b660-151">For testing purposes, you can use your own email address.</span></span>

   * <span data-ttu-id="1b660-152">V hello **subjektu** pole, když hello **dynamický obsah** se zobrazí seznam, vyberte hello **název oddílu** pole.</span><span class="sxs-lookup"><span data-stu-id="1b660-152">In hello **Subject** box, when hello **Dynamic content** list appears, select hello **Partition Name** field.</span></span>

     ![Ze seznamu "Dynamický obsah" hello vyberte "oddíl název"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     <span data-ttu-id="1b660-154">V další části můžete zadat klíč oddílu jedinečný, vydělí hello cíl batch do logické nastaví toowhere mohou zasílat zprávy.</span><span class="sxs-lookup"><span data-stu-id="1b660-154">In a later section, you can specify a unique partition key that divides hello target batch into logical sets toowhere you can send messages.</span></span> 
     <span data-ttu-id="1b660-155">Každá sada má jedinečné číslo, které je generován aplikace logiky hello odesílatele.</span><span class="sxs-lookup"><span data-stu-id="1b660-155">Each set has a unique number that's generated by hello sender logic app.</span></span> 
     <span data-ttu-id="1b660-156">Tato funkce umožňuje používat jeden batch s více podmnožin a definovat každý podmnožina s hello název, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="1b660-156">This capability lets you use a single batch with multiple subsets and define each subset with hello name that you provide.</span></span>

   * <span data-ttu-id="1b660-157">V hello **textu** pole, když hello **dynamický obsah** se zobrazí seznam, vyberte hello **Id zprávy** pole.</span><span class="sxs-lookup"><span data-stu-id="1b660-157">In hello **Body** box, when hello **Dynamic content** list appears, select hello **Message Id** field.</span></span>

     !["Text" Vyberte možnost "Id zprávy"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     <span data-ttu-id="1b660-159">Protože hello vstup pro hello odesílání e-mailu akce je pole, Návrhář hello automaticky přidá **pro každou** smyčky kolem hello **e-mailovou zprávu** akce.</span><span class="sxs-lookup"><span data-stu-id="1b660-159">Because hello input for hello send email action is an array, hello designer automatically adds a **For each** loop around hello **Send an email** action.</span></span> 
     <span data-ttu-id="1b660-160">Tato smyčka provede hello vnitřní akce na každou položku v dávce hello.</span><span class="sxs-lookup"><span data-stu-id="1b660-160">This loop performs hello inner action on each item in hello batch.</span></span> 
     <span data-ttu-id="1b660-161">Ano s hello batch aktivační událost sadu toofive položek, získáte pět e-mailů, které aktivuje jednotlivé aktivační události hello čas.</span><span class="sxs-lookup"><span data-stu-id="1b660-161">So, with hello batch trigger set toofive items, you get five emails each time hello trigger fires.</span></span>

7.  <span data-ttu-id="1b660-162">Vytvoření aplikace logiky příjemce batch, uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1b660-162">Now that you created a batch receiver logic app, save your logic app.</span></span>

    ![Uložení aplikace logiky](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-tooa-batch"></a><span data-ttu-id="1b660-164">Vytvoření aplikace logiky, které odesílají zprávy tooa batch</span><span class="sxs-lookup"><span data-stu-id="1b660-164">Create logic apps that send messages tooa batch</span></span>

<span data-ttu-id="1b660-165">Teď vytvořte jeden nebo více aplikací logiky, které odeslat dávku toohello položky definované aplikace logiky příjemce hello.</span><span class="sxs-lookup"><span data-stu-id="1b660-165">Now create one or more logic apps that send items toohello batch defined by hello receiver logic app.</span></span> <span data-ttu-id="1b660-166">Pro odesílatele hello zadejte aplikaci logiky hello příjemce a název dávkového, obsah zprávy a další nastavení.</span><span class="sxs-lookup"><span data-stu-id="1b660-166">For hello sender, you specify hello receiver logic app and batch name, message content, and any other settings.</span></span> <span data-ttu-id="1b660-167">Volitelně můžete zadat jedinečný oddílu klíče toodivide hello dávky do podmnožin toocollect položky s tímto klíčem.</span><span class="sxs-lookup"><span data-stu-id="1b660-167">You can optionally provide a unique partition key toodivide hello batch into subsets toocollect items with that key.</span></span>

<span data-ttu-id="1b660-168">Aplikace logiky odesílatele potřebovat vědět, kde toosend položky, zatímco aplikace logiky příjemce nepotřebujete tooknow cokoli o odesílatelé hello.</span><span class="sxs-lookup"><span data-stu-id="1b660-168">Sender logic apps need know where toosend items, while receiver logic apps don't need tooknow anything about hello senders.</span></span>

1. <span data-ttu-id="1b660-169">Vytvořit jinou aplikaci logiky s tímto názvem: "BatchSender"</span><span class="sxs-lookup"><span data-stu-id="1b660-169">Create another logic app with this name: "BatchSender"</span></span>

   1. <span data-ttu-id="1b660-170">Hello vyhledávacího pole zadejte "recurrence" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="1b660-170">In hello search box, enter "recurrence" as your filter.</span></span> 
   <span data-ttu-id="1b660-171">Vyberte této aktivační události: **plán - opakování**</span><span class="sxs-lookup"><span data-stu-id="1b660-171">Select this trigger: **Schedule - Recurrence**</span></span>

      ![Přidat aktivační událost hello "Plán-Recurrence"](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. <span data-ttu-id="1b660-173">Nastavte četnost hello a aplikace logiky interval toorun hello odesílatele každou minutu.</span><span class="sxs-lookup"><span data-stu-id="1b660-173">Set hello frequency and interval toorun hello sender logic app every minute.</span></span>

      ![Nastavení frekvence a intervalu opakování aktivační události](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. <span data-ttu-id="1b660-175">Přidáte nový krok pro odesílání zpráv tooa dávky.</span><span class="sxs-lookup"><span data-stu-id="1b660-175">Add a new step for sending messages tooa batch.</span></span>

   1. <span data-ttu-id="1b660-176">V části aktivační událost hello opakování, zvolte **+ nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="1b660-176">Under hello recurrence trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="1b660-177">Hello vyhledávacího pole zadejte "batch" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="1b660-177">In hello search box, enter "batch" as your filter.</span></span> 

   3. <span data-ttu-id="1b660-178">Vyberte tuto akci: **odesílat zprávy toobatch – zvolte Logic Apps pracovního postupu se aktivační událostí batch**</span><span class="sxs-lookup"><span data-stu-id="1b660-178">Select this action: **Send messages toobatch – Choose a Logic Apps workflow with batch trigger**</span></span>

      ![Vyberte "Odesílat zprávy toobatch"](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. <span data-ttu-id="1b660-180">Nyní vyberte svou aplikaci logiky "BatchReceiver", který jste dříve vytvořili, které nyní se zobrazí jako akce.</span><span class="sxs-lookup"><span data-stu-id="1b660-180">Now select your "BatchReceiver" logic app that you previously created, which now appears as an action.</span></span>

      ![Vyberte aplikaci logiky "batch příjemce"](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > <span data-ttu-id="1b660-182">seznam Hello také ukazuje všechny ostatní aplikace logiky, které mají batch aktivační události.</span><span class="sxs-lookup"><span data-stu-id="1b660-182">hello list also shows any other logic apps that have batch triggers.</span></span>

3. <span data-ttu-id="1b660-183">Nastavit vlastnosti batch hello.</span><span class="sxs-lookup"><span data-stu-id="1b660-183">Set hello batch properties.</span></span>

   * <span data-ttu-id="1b660-184">**Název služby batch**: název dávkového hello definované hello příjemce logiku aplikace, která je v tomto příkladu "TestBatch" a byl ověřen za běhu.</span><span class="sxs-lookup"><span data-stu-id="1b660-184">**Batch Name**: hello batch name defined by hello receiver logic app, which is "TestBatch" in this example and is validated at runtime.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="1b660-185">Zajistěte, aby hello batch název, který musí odpovídat názvu hello dávky, která je zadána aplikace logiky příjemce hello neměnit.</span><span class="sxs-lookup"><span data-stu-id="1b660-185">Make sure that you don't change hello batch name, which must match hello batch name that's specified by hello receiver logic app.</span></span>
     > <span data-ttu-id="1b660-186">Změna název dávkového hello způsobí, že odesílatel hello toofail aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="1b660-186">Changing hello batch name causes hello sender logic app toofail.</span></span>

   * <span data-ttu-id="1b660-187">**Obsah zprávy**: hello obsah zprávy, které chcete toosend.</span><span class="sxs-lookup"><span data-stu-id="1b660-187">**Message Content**: hello message content that you want toosend.</span></span> 
   <span data-ttu-id="1b660-188">V tomto příkladu přidejte tento výraz vloží hello aktuální datum a čas do obsahu, odešlete toohello batch uvítací zprávu:</span><span class="sxs-lookup"><span data-stu-id="1b660-188">For this example, add this expression that inserts hello current date and time into hello message content that you send toohello batch:</span></span>

     1. <span data-ttu-id="1b660-189">Když hello **dynamický obsah** seznamu se zobrazí, zvolte **výraz**.</span><span class="sxs-lookup"><span data-stu-id="1b660-189">When hello **Dynamic content** list appears, choose **Expression**.</span></span> 
     2. <span data-ttu-id="1b660-190">Zadejte výraz hello **utcnow()**a zvolte **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b660-190">Enter hello expression **utcnow()**, and choose **OK**.</span></span> 

        ![V "Zpráva obsah" Vyberte "Výraz".](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. <span data-ttu-id="1b660-193">Nyní nastavte oddíl pro dávku hello.</span><span class="sxs-lookup"><span data-stu-id="1b660-193">Now set up a partition for hello batch.</span></span> <span data-ttu-id="1b660-194">V akci "BatchReceiver" hello, zvolte **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="1b660-194">In hello "BatchReceiver" action, choose **Show advanced options**.</span></span>

   * <span data-ttu-id="1b660-195">**Název oddílu**: klíče toouse pro dělení hello cíl dávky volitelné jedinečný oddílu.</span><span class="sxs-lookup"><span data-stu-id="1b660-195">**Partition Name**: An optional unique partition key toouse for dividing hello target batch.</span></span> <span data-ttu-id="1b660-196">V tomto příkladu přidáte výraz, který generuje náhodné číslo v intervalu jeden a pět.</span><span class="sxs-lookup"><span data-stu-id="1b660-196">For this example, add an expression that generates a random number between one and five.</span></span>
   
     1. <span data-ttu-id="1b660-197">Když hello **dynamický obsah** seznamu se zobrazí, zvolte **výraz**.</span><span class="sxs-lookup"><span data-stu-id="1b660-197">When hello **Dynamic content** list appears, choose **Expression**.</span></span>
     2. <span data-ttu-id="1b660-198">Zadejte tento výraz: **rand(1,6)**</span><span class="sxs-lookup"><span data-stu-id="1b660-198">Enter this expression: **rand(1,6)**</span></span>

        ![Nastavení pro dávku cílový oddíl](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        <span data-ttu-id="1b660-200">To **rand –** funkce generuje v rozmezí jednu až pět.</span><span class="sxs-lookup"><span data-stu-id="1b660-200">This **rand** function generates a number between one and five.</span></span> 
        <span data-ttu-id="1b660-201">Proto jsou dělení tuto dávku do pěti číslem oddílů, které dynamicky nastaví tento výraz.</span><span class="sxs-lookup"><span data-stu-id="1b660-201">So you are dividing this batch into five numbered partitions, which this expression dynamically sets.</span></span>

   * <span data-ttu-id="1b660-202">**Id zprávy**: identifikátor volitelné zpráv a je vytvářenému identifikátoru GUID, pokud je prázdný.</span><span class="sxs-lookup"><span data-stu-id="1b660-202">**Message Id**: An optional message identifier and is a generated GUID when empty.</span></span> 
   <span data-ttu-id="1b660-203">V tomto příkladu ponechte toto pole prázdné.</span><span class="sxs-lookup"><span data-stu-id="1b660-203">For this example, leave this box blank.</span></span>

5. <span data-ttu-id="1b660-204">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1b660-204">Save your logic app.</span></span> <span data-ttu-id="1b660-205">Aplikace logiky odesílatele nyní vypadá podobně jako příklad toothis:</span><span class="sxs-lookup"><span data-stu-id="1b660-205">Your sender logic app now looks similar toothis example:</span></span>

   ![Uložte svou aplikaci logiky odesílatele](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a><span data-ttu-id="1b660-207">Testování aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="1b660-207">Test your logic apps</span></span>

<span data-ttu-id="1b660-208">tootest vaše dávkování řešení, nechte aplikace logiky spuštěná pro několik minut.</span><span class="sxs-lookup"><span data-stu-id="1b660-208">tootest your batching solution, leave your logic apps running for a few minutes.</span></span> <span data-ttu-id="1b660-209">Později, brzy začnete získání e-mailů v pěti skupiny, všechny s hello stejný klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="1b660-209">Soon, you start getting emails in groups of five, all with hello same partition key.</span></span>

<span data-ttu-id="1b660-210">Aplikace logiky BatchSender spouští každou minutu, vygeneruje náhodné číslo v intervalu jeden a 5 a používá toto generované číslo jako klíč oddílu hello pro dávku cíl hello, které jsou odesílány zprávy.</span><span class="sxs-lookup"><span data-stu-id="1b660-210">Your BatchSender logic app runs every minute, generates a random number between one and five, and uses this generated number as hello partition key for hello target batch where messages are sent.</span></span> <span data-ttu-id="1b660-211">Pokaždé, když hello batch má pět položek s hello stejným klíčem oddílu aplikace logiky BatchReceiver aktivuje a odešle e-mailu pro každou zprávu.</span><span class="sxs-lookup"><span data-stu-id="1b660-211">Each time hello batch has five items with hello same partition key, your BatchReceiver logic app fires and sends mail for each message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1b660-212">Po dokončení testování, zkontrolujte, zda zakázat hello BatchSender logiku aplikace toostop zasílání zpráv a předcházeli přetížení vaší doručené poště.</span><span class="sxs-lookup"><span data-stu-id="1b660-212">When you're done testing, make sure that you disable hello BatchSender logic app toostop sending messages and avoid overloading your inbox.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b660-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b660-213">Next steps</span></span>

* [<span data-ttu-id="1b660-214">Sestavení na definice aplikace logiky pomocí JSON</span><span class="sxs-lookup"><span data-stu-id="1b660-214">Build on logic app definitions by using JSON</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="1b660-215">Sestavení bez serveru aplikace v sadě Visual Studio s funkcemi a Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="1b660-215">Build a serverless app in Visual Studio with Azure Logic Apps and Functions</span></span>](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [<span data-ttu-id="1b660-216">Zpracování výjimek a protokolování chyb pro logic apps</span><span class="sxs-lookup"><span data-stu-id="1b660-216">Exception handling and error logging for logic apps</span></span>](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
