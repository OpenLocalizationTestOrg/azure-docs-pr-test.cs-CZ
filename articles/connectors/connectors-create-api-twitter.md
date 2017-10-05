---
title: "Naučte se používat konektor Twitter v aplikace logiky | Microsoft Docs"
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
ms.openlocfilehash: be8163043535833ce45b3d50939a537406cf8152
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-twitter-connector"></a><span data-ttu-id="2ad98-103">Začínáme s konektorem služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="2ad98-103">Get started with the Twitter connector</span></span>
<span data-ttu-id="2ad98-104">Konektor Twitter můžete:</span><span class="sxs-lookup"><span data-stu-id="2ad98-104">With the Twitter connector you can:</span></span>

* <span data-ttu-id="2ad98-105">Odesílání tweetů a tweetů get</span><span class="sxs-lookup"><span data-stu-id="2ad98-105">Post tweets and get tweets</span></span>
* <span data-ttu-id="2ad98-106">Časové osy přístup, přátele a délky</span><span class="sxs-lookup"><span data-stu-id="2ad98-106">Access timelines, friends and followers</span></span>
* <span data-ttu-id="2ad98-107">Proveďte jednu z dalších akcí a aktivační události popsané dole</span><span class="sxs-lookup"><span data-stu-id="2ad98-107">Perform any of the other actions and triggers described below</span></span>  

<span data-ttu-id="2ad98-108">Chcete-li použít [všechny konektory](apis-list.md), musíte nejprve vytvořit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="2ad98-108">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="2ad98-109">Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="2ad98-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>  

## <a name="connect-to-twitter"></a><span data-ttu-id="2ad98-110">Připojení k Twitteru</span><span class="sxs-lookup"><span data-stu-id="2ad98-110">Connect to Twitter</span></span>
<span data-ttu-id="2ad98-111">Než se aplikace logiky k jakékoli služby, musíte nejprve vytvořit *připojení* ke službě.</span><span class="sxs-lookup"><span data-stu-id="2ad98-111">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="2ad98-112">A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="2ad98-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-twitter"></a><span data-ttu-id="2ad98-113">Umožňuje vytvořit připojení k Twitteru</span><span class="sxs-lookup"><span data-stu-id="2ad98-113">Create a connection to Twitter</span></span>
> [!INCLUDE [Steps to create a connection to Twitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a><span data-ttu-id="2ad98-114">Použít aktivační událost služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="2ad98-114">Use a Twitter trigger</span></span>
<span data-ttu-id="2ad98-115">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="2ad98-115">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="2ad98-116">[Další informace o aktivační události](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="2ad98-116">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="2ad98-117">V tomto příkladu I vám ukáže, jak používat **při odeslání nové tweet** aktivační událost k hledání #Seattle a pokud najde #Seattle, aktualizujte soubor v Dropbox text z tweet.</span><span class="sxs-lookup"><span data-stu-id="2ad98-117">In this example, I will show you how to use the **When a new tweet is posted**  trigger to search for #Seattle and, if #Seattle is found, update a file in Dropbox with the text from the tweet.</span></span> <span data-ttu-id="2ad98-118">V příklad enterprise může vyhledat název vaší společnosti a aktualizace databáze SQL z tweet textu.</span><span class="sxs-lookup"><span data-stu-id="2ad98-118">In an enterprise example, you could search for the name of your company and update a SQL database with the text from the tweet.</span></span>

1. <span data-ttu-id="2ad98-119">Zadejte *twitter* do vyhledávacího pole v designeru aplikace logiky zvolte **Twitter - při odeslání nové tweet** aktivační události</span><span class="sxs-lookup"><span data-stu-id="2ad98-119">Enter *twitter* in the search box on the logic apps designer then select the **Twitter - When a new tweet is posted**  trigger</span></span>   
   <span data-ttu-id="2ad98-120">![Obrázek aktivační události služby Twitter 1](./media/connectors-create-api-twitter/trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-120">![Twitter trigger image 1](./media/connectors-create-api-twitter/trigger-1.png)</span></span>  
2. <span data-ttu-id="2ad98-121">Zadejte *#Seattle* v **hledaný Text** ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="2ad98-121">Enter *#Seattle* in the **Search Text** control</span></span>  
   <span data-ttu-id="2ad98-122">![Obrázek aktivační události služby Twitter 2](./media/connectors-create-api-twitter/trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-122">![Twitter trigger image 2](./media/connectors-create-api-twitter/trigger-2.png)</span></span> 

<span data-ttu-id="2ad98-123">Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost, která zahájí spuštění ostatních triggery a akce v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="2ad98-123">At this point, your logic app has been configured with a trigger that will begin a run of the other triggers and actions in the workflow.</span></span> 

> [!NOTE]
> <span data-ttu-id="2ad98-124">Pro aplikace logiky být funkční musí obsahovat nejméně jedna aktivační událost a jedna akce.</span><span class="sxs-lookup"><span data-stu-id="2ad98-124">For a logic app to be functional, it must contain at least one trigger and one action.</span></span> <span data-ttu-id="2ad98-125">Postupujte podle kroků v další části a přidejte akci.</span><span class="sxs-lookup"><span data-stu-id="2ad98-125">Follow the steps in the next section to add an action.</span></span>  
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="2ad98-126">Přidat podmínku</span><span class="sxs-lookup"><span data-stu-id="2ad98-126">Add a condition</span></span>
<span data-ttu-id="2ad98-127">Vzhledem k tomu, že jsme zajímá pouze tweetů od uživatelů s více než 50 uživateli, podmínku, která potvrdí počet délky přidat do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="2ad98-127">Since we are only interested in tweets from users with more than 50 users, a condition that confirms the number of followers must first be added to the logic app.</span></span>  

1. <span data-ttu-id="2ad98-128">Vyberte **+ nový krok** přidat akci chcete provést, když #Seattle se nachází v nové tweet</span><span class="sxs-lookup"><span data-stu-id="2ad98-128">Select **+ New step** to add the action you would like to take when #Seattle is found in a new tweet</span></span>  
   <span data-ttu-id="2ad98-129">![Obrázek akce Twitter 1](../../includes/media/connectors-create-api-twitter/action-1.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-129">![Twitter action image 1](../../includes/media/connectors-create-api-twitter/action-1.png)</span></span>  
2. <span data-ttu-id="2ad98-130">Vyberte **přidat podmínku** odkaz.</span><span class="sxs-lookup"><span data-stu-id="2ad98-130">Select the **Add a condition** link.</span></span>  
   <span data-ttu-id="2ad98-131">![Obrázek stavu Twitter 1](../../includes/media/connectors-create-api-twitter/condition-1.png) </span><span class="sxs-lookup"><span data-stu-id="2ad98-131">![Twitter condition image 1](../../includes/media/connectors-create-api-twitter/condition-1.png) </span></span>  
   <span data-ttu-id="2ad98-132">Tím se otevře **podmínku** řízení, kde můžete zkontrolovat podmínky, jako *rovná*, *je menší než*, *je větší než*, *obsahuje*atd.</span><span class="sxs-lookup"><span data-stu-id="2ad98-132">This opens the **Condition** control where you can check conditions such as *is equal to*, *is less than*, *is greater than*, *contains*, etc.</span></span>  
   <span data-ttu-id="2ad98-133">![Obrázek stavu Twitter 2](../../includes/media/connectors-create-api-twitter/condition-2.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-133">![Twitter condition image 2](../../includes/media/connectors-create-api-twitter/condition-2.png)</span></span>   
3. <span data-ttu-id="2ad98-134">Vyberte **zvolte hodnotu** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="2ad98-134">Select the **Choose a value** control.</span></span>  
   <span data-ttu-id="2ad98-135">U tohoto ovládacího prvku můžete vyberete jeden nebo více vlastností v jakémkoli předchozí akce nebo aktivační události jako hodnota, jejíž podmínka bude vyhodnocena jako true nebo false.</span><span class="sxs-lookup"><span data-stu-id="2ad98-135">In this control, you can select one or more of the properties from any previous actions or triggers as the value whose condition will be evaluated to true or false.</span></span>
   <span data-ttu-id="2ad98-136">![Obrázek stavu Twitter 3](../../includes/media/connectors-create-api-twitter/condition-3.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-136">![Twitter condition image 3](../../includes/media/connectors-create-api-twitter/condition-3.png)</span></span>   
4. <span data-ttu-id="2ad98-137">Vyberte **...**  rozbalte seznam vlastností, aby se zobrazily všechny vlastnosti, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2ad98-137">Select the **...** to expand the list of properties so you can see all the properties that are available.</span></span>        
   <span data-ttu-id="2ad98-138">![Obrázek stavu Twitter 4](../../includes/media/connectors-create-api-twitter/condition-4.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-138">![Twitter condition image 4](../../includes/media/connectors-create-api-twitter/condition-4.png)</span></span>   
5. <span data-ttu-id="2ad98-139">Vyberte **délky počet** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2ad98-139">Select the **Followers count** property.</span></span>    
   <span data-ttu-id="2ad98-140">![Obrázek stavu Twitter 5](../../includes/media/connectors-create-api-twitter/condition-5.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-140">![Twitter condition image 5](../../includes/media/connectors-create-api-twitter/condition-5.png)</span></span>   
6. <span data-ttu-id="2ad98-141">Všimněte si vlastnost počet délky je nyní v ovládacím prvku hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2ad98-141">Notice the Followers count property is now in the value control.</span></span>    
   ![Obrázek stavu Twitter 6](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. <span data-ttu-id="2ad98-143">Vyberte **je větší než** ze seznamu operátory.</span><span class="sxs-lookup"><span data-stu-id="2ad98-143">Select **is greater than** from the operators list.</span></span>    
   <span data-ttu-id="2ad98-144">![Obrázek stavu Twitter 7](../../includes/media/connectors-create-api-twitter/condition-7.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-144">![Twitter condition image 7](../../includes/media/connectors-create-api-twitter/condition-7.png)</span></span>   
8. <span data-ttu-id="2ad98-145">Zadejte 50 jako operand pro *je větší než* operátor.</span><span class="sxs-lookup"><span data-stu-id="2ad98-145">Enter 50 as the operand for the *is greater than* operator.</span></span>  
   <span data-ttu-id="2ad98-146">Podmínka je nyní přidána.</span><span class="sxs-lookup"><span data-stu-id="2ad98-146">The condition is now added.</span></span> <span data-ttu-id="2ad98-147">Uložit vaší práci pomocí **Uložit** odkaz v nabídce výše.</span><span class="sxs-lookup"><span data-stu-id="2ad98-147">Save your work using the **Save** link on the menu above.</span></span>    
   <span data-ttu-id="2ad98-148">![Twitter podmínku obrázek 8](../../includes/media/connectors-create-api-twitter/condition-8.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-148">![Twitter condition image 8](../../includes/media/connectors-create-api-twitter/condition-8.png)</span></span>   

## <a name="use-a-twitter-action"></a><span data-ttu-id="2ad98-149">Pomocí akce služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="2ad98-149">Use a Twitter action</span></span>
<span data-ttu-id="2ad98-150">Akce je operace prováděné definované v aplikaci logiky pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="2ad98-150">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="2ad98-151">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="2ad98-151">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="2ad98-152">Teď, když jste přidali aktivační událost, použijte následující postup přidání akce, která bude odeslána nové tweet s obsahem tweetů nalezena. aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="2ad98-152">Now that you have added a trigger, follow these steps to add an action that will post a new tweet with the contents of the tweets found by the trigger.</span></span> <span data-ttu-id="2ad98-153">Pro účely tohoto návodu budou odeslány pouze tweetů od uživatelů s více než 50 délky.</span><span class="sxs-lookup"><span data-stu-id="2ad98-153">For the purposes of this walk-through only tweets from users with more than 50 followers will be posted.</span></span>  

<span data-ttu-id="2ad98-154">V dalším kroku přidáte Twitter akci, která bude odeslána tweet pomocí některé z vlastností jednotlivých tweet, který byl odeslán uživatelem, který má více než 50 délky.</span><span class="sxs-lookup"><span data-stu-id="2ad98-154">In the next step, you will add a Twitter action that will post a tweet using some of the properties of each tweet that has been posted by a user who has more than 50 followers.</span></span>  

1. <span data-ttu-id="2ad98-155">Vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="2ad98-155">Select **Add an action**.</span></span> <span data-ttu-id="2ad98-156">Otevře se ovládací prvek vyhledávání, kde můžete vyhledat další akce a aktivační události.</span><span class="sxs-lookup"><span data-stu-id="2ad98-156">This opens the search control where you can search for other actions and triggers.</span></span>  
   <span data-ttu-id="2ad98-157">![Obrázek stavu Twitter 9](../../includes/media/connectors-create-api-twitter/condition-9.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-157">![Twitter condition image 9](../../includes/media/connectors-create-api-twitter/condition-9.png)</span></span>   
2. <span data-ttu-id="2ad98-158">Zadejte *twitter* do pole pro vyhledávání vyberte **Twitter – Post tweet** akce.</span><span class="sxs-lookup"><span data-stu-id="2ad98-158">Enter *twitter* into the search box then select the **Twitter - Post a tweet** action.</span></span> <span data-ttu-id="2ad98-159">Tím se otevře **Post tweet** řídit, kdy bude zadejte všechny podrobnosti tweet odeslání.</span><span class="sxs-lookup"><span data-stu-id="2ad98-159">This opens the **Post a tweet** control where you will enter all details for the tweet being posted.</span></span>      
   <span data-ttu-id="2ad98-160">![Twitter akce obrázek 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-160">![Twitter action image 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)</span></span>   
3. <span data-ttu-id="2ad98-161">Vyberte **Tweet text** ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="2ad98-161">Select the **Tweet text** control.</span></span> <span data-ttu-id="2ad98-162">Všechny výstupy z předchozí akce a aktivačních událostí v aplikaci logiky jsou nyní zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="2ad98-162">All outputs from previous actions and triggers in the logic app are now visible.</span></span> <span data-ttu-id="2ad98-163">Můžete vybrat některý z těchto a použít je jako součást tweet text nové tweet.</span><span class="sxs-lookup"><span data-stu-id="2ad98-163">You can select any of these and use them as part of the tweet text of the new tweet.</span></span>     
   <span data-ttu-id="2ad98-164">![Obrázek akce Twitter 2](../../includes/media/connectors-create-api-twitter/action-2.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-164">![Twitter action image 2](../../includes/media/connectors-create-api-twitter/action-2.png)</span></span>   
4. <span data-ttu-id="2ad98-165">Vyberte **uživatelské jméno**</span><span class="sxs-lookup"><span data-stu-id="2ad98-165">Select **User name**</span></span>   
5. <span data-ttu-id="2ad98-166">Zadejte *říká:* v ovládacím prvku tweet text.</span><span class="sxs-lookup"><span data-stu-id="2ad98-166">Enter *says:* in the tweet text control.</span></span> <span data-ttu-id="2ad98-167">To lze proveďte pouze po uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="2ad98-167">Do this just after User name.</span></span>  
6. <span data-ttu-id="2ad98-168">Vyberte *Tweet text*.</span><span class="sxs-lookup"><span data-stu-id="2ad98-168">Select *Tweet text*.</span></span>       
   <span data-ttu-id="2ad98-169">![Obrázek akce Twitter 3](../../includes/media/connectors-create-api-twitter/action-3.png)</span><span class="sxs-lookup"><span data-stu-id="2ad98-169">![Twitter action image 3](../../includes/media/connectors-create-api-twitter/action-3.png)</span></span>   
7. <span data-ttu-id="2ad98-170">Uložte si práci a odeslala tweet s hashtag #Seattle aktivovat pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="2ad98-170">Save your work and send a tweet with the #Seattle hashtag to activate your workflow.</span></span>  


## <a name="connector-specific-details"></a><span data-ttu-id="2ad98-171">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="2ad98-171">Connector-specific details</span></span>

<span data-ttu-id="2ad98-172">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/twitterconnector/).</span><span class="sxs-lookup"><span data-stu-id="2ad98-172">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/twitterconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2ad98-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ad98-173">Next steps</span></span>
[<span data-ttu-id="2ad98-174">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="2ad98-174">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

