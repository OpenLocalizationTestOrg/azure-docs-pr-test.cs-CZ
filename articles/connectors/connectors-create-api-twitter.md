---
title: aaaLearn jak toouse hello konektor Twitter v aplikace logiky | Microsoft Docs
description: "Přehled konektor Twitter s parametry rozhraní REST API"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: ead4e4dc95bf894fd72b908c5375b407ba27642d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-twitter-connector"></a><span data-ttu-id="d3691-103">Začínáme s konektor Twitter hello</span><span class="sxs-lookup"><span data-stu-id="d3691-103">Get started with hello Twitter connector</span></span>
<span data-ttu-id="d3691-104">Konektor Twitter hello můžete:</span><span class="sxs-lookup"><span data-stu-id="d3691-104">With hello Twitter connector you can:</span></span>

* <span data-ttu-id="d3691-105">Odesílání tweetů a tweetů get</span><span class="sxs-lookup"><span data-stu-id="d3691-105">Post tweets and get tweets</span></span>
* <span data-ttu-id="d3691-106">Časové osy přístup, přátele a délky</span><span class="sxs-lookup"><span data-stu-id="d3691-106">Access timelines, friends and followers</span></span>
* <span data-ttu-id="d3691-107">Proveďte jednu z hello jiných akcí a aktivační události popsané dole</span><span class="sxs-lookup"><span data-stu-id="d3691-107">Perform any of hello other actions and triggers described below</span></span>  

<span data-ttu-id="d3691-108">toouse [všechny konektory](apis-list.md), musíte nejprve toocreate aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="d3691-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="d3691-109">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d3691-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>  

## <a name="connect-tootwitter"></a><span data-ttu-id="d3691-110">Připojit tooTwitter</span><span class="sxs-lookup"><span data-stu-id="d3691-110">Connect tooTwitter</span></span>
<span data-ttu-id="d3691-111">Než se aplikace logiky k jakékoli služby, musíte nejprve toocreate *připojení* toohello služby.</span><span class="sxs-lookup"><span data-stu-id="d3691-111">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="d3691-112">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="d3691-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-tootwitter"></a><span data-ttu-id="d3691-113">Vytvoření připojení tooTwitter</span><span class="sxs-lookup"><span data-stu-id="d3691-113">Create a connection tooTwitter</span></span>
> [!INCLUDE [Steps toocreate a connection tooTwitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a><span data-ttu-id="d3691-114">Použít aktivační událost služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="d3691-114">Use a Twitter trigger</span></span>
<span data-ttu-id="d3691-115">Aktivační událost je událost, která může být pracovní postup hello použité toostart definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="d3691-115">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="d3691-116">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="d3691-116">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="d3691-117">V tomto příkladu I vám ukáže, jak toouse hello **při odeslání nové tweet** aktivovat toosearch pro #Seattle a pokud najde #Seattle, aktualizujte soubor v Dropbox hello text z hello tweet.</span><span class="sxs-lookup"><span data-stu-id="d3691-117">In this example, I will show you how toouse hello **When a new tweet is posted**  trigger toosearch for #Seattle and, if #Seattle is found, update a file in Dropbox with hello text from hello tweet.</span></span> <span data-ttu-id="d3691-118">V příklad enterprise může vyhledat hello název vaší společnosti a aktualizace databáze SQL z hello tweet textu hello.</span><span class="sxs-lookup"><span data-stu-id="d3691-118">In an enterprise example, you could search for hello name of your company and update a SQL database with hello text from hello tweet.</span></span>

1. <span data-ttu-id="d3691-119">Zadejte *twitter* hello vyhledávacího pole v designeru aplikace logiky hello zvolte hello **Twitter - při odeslání nové tweet** aktivační události</span><span class="sxs-lookup"><span data-stu-id="d3691-119">Enter *twitter* in hello search box on hello logic apps designer then select hello **Twitter - When a new tweet is posted**  trigger</span></span>   
   <span data-ttu-id="d3691-120">![Obrázek aktivační události služby Twitter 1](./media/connectors-create-api-twitter/trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-120">![Twitter trigger image 1](./media/connectors-create-api-twitter/trigger-1.png)</span></span>  
2. <span data-ttu-id="d3691-121">Zadejte *#Seattle* v hello **hledaný Text** ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="d3691-121">Enter *#Seattle* in hello **Search Text** control</span></span>  
   <span data-ttu-id="d3691-122">![Obrázek aktivační události služby Twitter 2](./media/connectors-create-api-twitter/trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-122">![Twitter trigger image 2](./media/connectors-create-api-twitter/trigger-2.png)</span></span> 

<span data-ttu-id="d3691-123">Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost, která zahájí spuštění hello jiných triggery a akce v pracovním postupu hello.</span><span class="sxs-lookup"><span data-stu-id="d3691-123">At this point, your logic app has been configured with a trigger that will begin a run of hello other triggers and actions in hello workflow.</span></span> 

> [!NOTE]
> <span data-ttu-id="d3691-124">Pro logiku aplikace toobe funkční musí obsahovat nejméně jedna aktivační událost a jedna akce.</span><span class="sxs-lookup"><span data-stu-id="d3691-124">For a logic app toobe functional, it must contain at least one trigger and one action.</span></span> <span data-ttu-id="d3691-125">Postupujte podle kroků hello v další části tooadd hello akce.</span><span class="sxs-lookup"><span data-stu-id="d3691-125">Follow hello steps in hello next section tooadd an action.</span></span>  
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="d3691-126">Přidat podmínku</span><span class="sxs-lookup"><span data-stu-id="d3691-126">Add a condition</span></span>
<span data-ttu-id="d3691-127">Vzhledem k tomu, že jsme zajímá pouze tweetů od uživatelů s více než 50 uživateli, podmínku, která potvrdí hello počet délky je nejprve nutné přidat toohello aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="d3691-127">Since we are only interested in tweets from users with more than 50 users, a condition that confirms hello number of followers must first be added toohello logic app.</span></span>  

1. <span data-ttu-id="d3691-128">Vyberte **+ nový krok** tooadd hello akce chcete tootake při #Seattle se nachází v nové tweet</span><span class="sxs-lookup"><span data-stu-id="d3691-128">Select **+ New step** tooadd hello action you would like tootake when #Seattle is found in a new tweet</span></span>  
   <span data-ttu-id="d3691-129">![Obrázek akce Twitter 1](../../includes/media/connectors-create-api-twitter/action-1.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-129">![Twitter action image 1](../../includes/media/connectors-create-api-twitter/action-1.png)</span></span>  
2. <span data-ttu-id="d3691-130">Vyberte hello **přidat podmínku** odkaz.</span><span class="sxs-lookup"><span data-stu-id="d3691-130">Select hello **Add a condition** link.</span></span>  
   <span data-ttu-id="d3691-131">![Obrázek stavu Twitter 1](../../includes/media/connectors-create-api-twitter/condition-1.png) </span><span class="sxs-lookup"><span data-stu-id="d3691-131">![Twitter condition image 1](../../includes/media/connectors-create-api-twitter/condition-1.png) </span></span>  
   <span data-ttu-id="d3691-132">Tím se otevře hello **podmínku** řízení, kde můžete zkontrolovat podmínky, jako *rovná*, *je menší než*, *je větší než*, *obsahuje*atd.</span><span class="sxs-lookup"><span data-stu-id="d3691-132">This opens hello **Condition** control where you can check conditions such as *is equal to*, *is less than*, *is greater than*, *contains*, etc.</span></span>  
   <span data-ttu-id="d3691-133">![Obrázek stavu Twitter 2](../../includes/media/connectors-create-api-twitter/condition-2.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-133">![Twitter condition image 2](../../includes/media/connectors-create-api-twitter/condition-2.png)</span></span>   
3. <span data-ttu-id="d3691-134">Vyberte hello **zvolte hodnotu** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="d3691-134">Select hello **Choose a value** control.</span></span>  
   <span data-ttu-id="d3691-135">U tohoto ovládacího prvku můžete vybrat jednu nebo více vlastností hello z libovolné předchozí akce nebo aktivační události jako hello hodnotu, jehož podmínka bude vyhodnotí tootrue, nebo hodnotu NEPRAVDA.</span><span class="sxs-lookup"><span data-stu-id="d3691-135">In this control, you can select one or more of hello properties from any previous actions or triggers as hello value whose condition will be evaluated tootrue or false.</span></span>
   <span data-ttu-id="d3691-136">![Obrázek stavu Twitter 3](../../includes/media/connectors-create-api-twitter/condition-3.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-136">![Twitter condition image 3](../../includes/media/connectors-create-api-twitter/condition-3.png)</span></span>   
4. <span data-ttu-id="d3691-137">Vyberte hello **...**  tooexpand hello seznam vlastností, aby se zobrazily všechny hello vlastnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d3691-137">Select hello **...** tooexpand hello list of properties so you can see all hello properties that are available.</span></span>        
   <span data-ttu-id="d3691-138">![Obrázek stavu Twitter 4](../../includes/media/connectors-create-api-twitter/condition-4.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-138">![Twitter condition image 4](../../includes/media/connectors-create-api-twitter/condition-4.png)</span></span>   
5. <span data-ttu-id="d3691-139">Vyberte hello **délky počet** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d3691-139">Select hello **Followers count** property.</span></span>    
   <span data-ttu-id="d3691-140">![Obrázek stavu Twitter 5](../../includes/media/connectors-create-api-twitter/condition-5.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-140">![Twitter condition image 5](../../includes/media/connectors-create-api-twitter/condition-5.png)</span></span>   
6. <span data-ttu-id="d3691-141">Všimněte si počtu vlastnost je nyní v ovládacím prvku hello hodnota délky hello.</span><span class="sxs-lookup"><span data-stu-id="d3691-141">Notice hello Followers count property is now in hello value control.</span></span>    
   ![Obrázek stavu Twitter 6](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. <span data-ttu-id="d3691-143">Vyberte **je větší než** hello operátory seznamu.</span><span class="sxs-lookup"><span data-stu-id="d3691-143">Select **is greater than** from hello operators list.</span></span>    
   <span data-ttu-id="d3691-144">![Obrázek stavu Twitter 7](../../includes/media/connectors-create-api-twitter/condition-7.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-144">![Twitter condition image 7](../../includes/media/connectors-create-api-twitter/condition-7.png)</span></span>   
8. <span data-ttu-id="d3691-145">Zadejte 50, hello operand pro hello *je větší než* operátor.</span><span class="sxs-lookup"><span data-stu-id="d3691-145">Enter 50 as hello operand for hello *is greater than* operator.</span></span>  
   <span data-ttu-id="d3691-146">Podmínka Hello je nyní přidána.</span><span class="sxs-lookup"><span data-stu-id="d3691-146">hello condition is now added.</span></span> <span data-ttu-id="d3691-147">Uložte si práci pomocí hello **Uložit** odkaz v nabídce hello výše.</span><span class="sxs-lookup"><span data-stu-id="d3691-147">Save your work using hello **Save** link on hello menu above.</span></span>    
   <span data-ttu-id="d3691-148">![Twitter podmínku obrázek 8](../../includes/media/connectors-create-api-twitter/condition-8.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-148">![Twitter condition image 8](../../includes/media/connectors-create-api-twitter/condition-8.png)</span></span>   

## <a name="use-a-twitter-action"></a><span data-ttu-id="d3691-149">Pomocí akce služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="d3691-149">Use a Twitter action</span></span>
<span data-ttu-id="d3691-150">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="d3691-150">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="d3691-151">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="d3691-151">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="d3691-152">Teď, když jste přidali aktivační událost, postupujte podle těchto kroků tooadd akci, která bude odeslána nové tweet s hello obsah hello tweetů nalezen hello aktivační procedura.</span><span class="sxs-lookup"><span data-stu-id="d3691-152">Now that you have added a trigger, follow these steps tooadd an action that will post a new tweet with hello contents of hello tweets found by hello trigger.</span></span> <span data-ttu-id="d3691-153">Pro účely tohoto návodu hello budou odeslány pouze tweetů od uživatelů s více než 50 délky.</span><span class="sxs-lookup"><span data-stu-id="d3691-153">For hello purposes of this walk-through only tweets from users with more than 50 followers will be posted.</span></span>  

<span data-ttu-id="d3691-154">V dalším kroku hello přidáte Twitter akci, která bude odeslána tweet pomocí některé z vlastností hello každý tweet, který byl odeslán uživatelem, který má více než 50 délky.</span><span class="sxs-lookup"><span data-stu-id="d3691-154">In hello next step, you will add a Twitter action that will post a tweet using some of hello properties of each tweet that has been posted by a user who has more than 50 followers.</span></span>  

1. <span data-ttu-id="d3691-155">Vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="d3691-155">Select **Add an action**.</span></span> <span data-ttu-id="d3691-156">Otevře se ovládací prvek vyhledávání hello kde můžete vyhledat další akce a aktivační události.</span><span class="sxs-lookup"><span data-stu-id="d3691-156">This opens hello search control where you can search for other actions and triggers.</span></span>  
   <span data-ttu-id="d3691-157">![Obrázek stavu Twitter 9](../../includes/media/connectors-create-api-twitter/condition-9.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-157">![Twitter condition image 9](../../includes/media/connectors-create-api-twitter/condition-9.png)</span></span>   
2. <span data-ttu-id="d3691-158">Zadejte *twitter* do vyhledávacího pole hello vyberte hello **Twitter – Post tweet** akce.</span><span class="sxs-lookup"><span data-stu-id="d3691-158">Enter *twitter* into hello search box then select hello **Twitter - Post a tweet** action.</span></span> <span data-ttu-id="d3691-159">Tím se otevře hello **Post tweet** řídit, kdy bude zadejte všechny podrobnosti hello tweet odeslání.</span><span class="sxs-lookup"><span data-stu-id="d3691-159">This opens hello **Post a tweet** control where you will enter all details for hello tweet being posted.</span></span>      
   <span data-ttu-id="d3691-160">![Twitter akce obrázek 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-160">![Twitter action image 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)</span></span>   
3. <span data-ttu-id="d3691-161">Vyberte hello **Tweet text** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="d3691-161">Select hello **Tweet text** control.</span></span> <span data-ttu-id="d3691-162">Všechny výstupy z předchozí akce a aktivačních událostí v aplikaci logiky hello jsou nyní zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="d3691-162">All outputs from previous actions and triggers in hello logic app are now visible.</span></span> <span data-ttu-id="d3691-163">Můžete vybrat některý z těchto a použít je jako část textu hello tweet o nové tweet hello.</span><span class="sxs-lookup"><span data-stu-id="d3691-163">You can select any of these and use them as part of hello tweet text of hello new tweet.</span></span>     
   <span data-ttu-id="d3691-164">![Obrázek akce Twitter 2](../../includes/media/connectors-create-api-twitter/action-2.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-164">![Twitter action image 2](../../includes/media/connectors-create-api-twitter/action-2.png)</span></span>   
4. <span data-ttu-id="d3691-165">Vyberte **uživatelské jméno**</span><span class="sxs-lookup"><span data-stu-id="d3691-165">Select **User name**</span></span>   
5. <span data-ttu-id="d3691-166">Zadejte *říká:* v ovládacím prvku text tweet hello.</span><span class="sxs-lookup"><span data-stu-id="d3691-166">Enter *says:* in hello tweet text control.</span></span> <span data-ttu-id="d3691-167">To lze proveďte pouze po uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="d3691-167">Do this just after User name.</span></span>  
6. <span data-ttu-id="d3691-168">Vyberte *Tweet text*.</span><span class="sxs-lookup"><span data-stu-id="d3691-168">Select *Tweet text*.</span></span>       
   <span data-ttu-id="d3691-169">![Obrázek akce Twitter 3](../../includes/media/connectors-create-api-twitter/action-3.png)</span><span class="sxs-lookup"><span data-stu-id="d3691-169">![Twitter action image 3](../../includes/media/connectors-create-api-twitter/action-3.png)</span></span>   
7. <span data-ttu-id="d3691-170">Uložte si práci a odeslala tweet s hello #Seattle hashtag tooactivate pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="d3691-170">Save your work and send a tweet with hello #Seattle hashtag tooactivate your workflow.</span></span>  


## <a name="connector-specific-details"></a><span data-ttu-id="d3691-171">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="d3691-171">Connector-specific details</span></span>

<span data-ttu-id="d3691-172">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/twitterconnector/).</span><span class="sxs-lookup"><span data-stu-id="d3691-172">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/twitterconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d3691-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3691-173">Next steps</span></span>
[<span data-ttu-id="d3691-174">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="d3691-174">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

