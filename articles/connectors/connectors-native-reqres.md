---
title: "Použít požadavek a odpověď akce | Microsoft Docs"
description: "Přehled žádostí a odpovědí aktivační události a akce v aplikaci Azure logiky"
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 566924a4-0988-4d86-9ecd-ad22507858c0
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: e45b07d709927af64cfba28dfb0d8ee9cb8893b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-request-and-response-components"></a><span data-ttu-id="25f22-103">Začínáme s komponentami žádosti a odpovědi</span><span class="sxs-lookup"><span data-stu-id="25f22-103">Get started with the request and response components</span></span>
<span data-ttu-id="25f22-104">S žádostí a odpovědí komponenty aplikace logiky můžete v reálném čase reakce na události.</span><span class="sxs-lookup"><span data-stu-id="25f22-104">With the request and response components in a logic app, you can respond in real time to events.</span></span>

<span data-ttu-id="25f22-105">Můžete například provést následující věci:</span><span class="sxs-lookup"><span data-stu-id="25f22-105">For example, you can:</span></span>

* <span data-ttu-id="25f22-106">Odpověď na požadavek HTTP s daty z místní databáze pomocí aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="25f22-106">Respond to an HTTP request with data from an on-premises database through a logic app.</span></span>
* <span data-ttu-id="25f22-107">Aktivovat aplikaci logiky z externí webhooku události.</span><span class="sxs-lookup"><span data-stu-id="25f22-107">Trigger a logic app from an external webhook event.</span></span>
* <span data-ttu-id="25f22-108">Volání aplikace logiky pomocí akce žádostí a odpovědí z v rámci jiné aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="25f22-108">Call a logic app with a request and response action from within another logic app.</span></span>

<span data-ttu-id="25f22-109">Chcete-li začít používat žádostí a odpovědí akce v aplikaci logiky, přečtěte si téma [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="25f22-109">To get started using the request and response actions in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-the-http-request-trigger"></a><span data-ttu-id="25f22-110">Použít aktivační událost požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="25f22-110">Use the HTTP Request trigger</span></span>
<span data-ttu-id="25f22-111">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="25f22-111">A trigger is an event that can be used to start the workflow that is defined in a logic app.</span></span> <span data-ttu-id="25f22-112">[Další informace o aktivační události](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="25f22-112">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="25f22-113">Zde je v sekvenci příklad toho, jak nastavit požadavku HTTP v Návrháři logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="25f22-113">Here’s an example sequence of how to set up an HTTP request in the Logic App Designer.</span></span>

1. <span data-ttu-id="25f22-114">Přidat aktivační událost **požadavku - obdrží žádost HTTP při** v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="25f22-114">Add the trigger **Request - When an HTTP request is received** in your logic app.</span></span> <span data-ttu-id="25f22-115">Volitelně můžete zadat schématu JSON (pomocí některého nástroje, například [JSONSchema.net](http://jsonschema.net)) pro tělo žádosti.</span><span class="sxs-lookup"><span data-stu-id="25f22-115">You can optionally provide a JSON schema (by using a tool like [JSONSchema.net](http://jsonschema.net)) for the request body.</span></span> <span data-ttu-id="25f22-116">To umožňuje návrháři ke generování tokenů pro vlastnosti v požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="25f22-116">This allows the designer to generate tokens for properties in the HTTP request.</span></span>
2. <span data-ttu-id="25f22-117">Přidáte další akci, takže můžete uložit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="25f22-117">Add another action so that you can save the logic app.</span></span>
3. <span data-ttu-id="25f22-118">Po uložení aplikaci logiky, můžete získat adresu URL požadavku HTTP z karty požadavku.</span><span class="sxs-lookup"><span data-stu-id="25f22-118">After saving the logic app, you can get the HTTP request URL from the request card.</span></span>
4. <span data-ttu-id="25f22-119">HTTP POST (můžete použít nástroje, jako je [Postman](https://www.getpostman.com/)) na adresu URL aktivuje aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="25f22-119">An HTTP POST (you can use a tool like [Postman](https://www.getpostman.com/)) to the URL triggers the logic app.</span></span>

> [!NOTE]
> <span data-ttu-id="25f22-120">Pokud nedefinujete akce odpovědi, `202 ACCEPTED` okamžitě vrátila odpověď na volajícího.</span><span class="sxs-lookup"><span data-stu-id="25f22-120">If you don't define a response action, a `202 ACCEPTED` response is immediately returned to the caller.</span></span> <span data-ttu-id="25f22-121">Akce odpovědi můžete použít k přizpůsobení odpověď.</span><span class="sxs-lookup"><span data-stu-id="25f22-121">You can use the response action to customize a response.</span></span>
> 
> 

![Aktivační událost odpovědi](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a><span data-ttu-id="25f22-123">Pomocí akce HTTP odpovědi</span><span class="sxs-lookup"><span data-stu-id="25f22-123">Use the HTTP Response action</span></span>
<span data-ttu-id="25f22-124">Akce odpovědi HTTP je platný pouze při použití v pracovním postupu, která se spustí při požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="25f22-124">The HTTP Response action is only valid when you use it in a workflow that is triggered by an HTTP request.</span></span> <span data-ttu-id="25f22-125">Pokud nedefinujete akce odpovědi, `202 ACCEPTED` okamžitě vrátila odpověď na volajícího.</span><span class="sxs-lookup"><span data-stu-id="25f22-125">If you don't define a response action, a `202 ACCEPTED` response is immediately returned to the caller.</span></span>  <span data-ttu-id="25f22-126">V kterémkoliv kroku v rámci pracovního postupu můžete přidat akce odpovědi.</span><span class="sxs-lookup"><span data-stu-id="25f22-126">You can add a response action at any step within the workflow.</span></span> <span data-ttu-id="25f22-127">Aplikace logiky pouze udržuje příchozího požadavku otevřete jednu minutu pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="25f22-127">The logic app only keeps the incoming request open for one minute for a response.</span></span>  <span data-ttu-id="25f22-128">Za minutu, pokud žádná odpověď byla odeslána z pracovního postupu (a akce odpovědi v definici existuje) `504 GATEWAY TIMEOUT` je vrácen volajícímu.</span><span class="sxs-lookup"><span data-stu-id="25f22-128">After one minute, if no response was sent from the workflow (and a response action exists in the definition), a `504 GATEWAY TIMEOUT` is returned to the caller.</span></span>

<span data-ttu-id="25f22-129">Zde je postup přidání akce odpovědi HTTP:</span><span class="sxs-lookup"><span data-stu-id="25f22-129">Here's how to add an HTTP Response action:</span></span>

1. <span data-ttu-id="25f22-130">Vyberte **nový krok** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="25f22-130">Select the **New Step** button.</span></span>
2. <span data-ttu-id="25f22-131">Zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="25f22-131">Choose **Add an action**.</span></span>
3. <span data-ttu-id="25f22-132">Zadejte do vyhledávacího pole Akce **odpovědi** seznam akce odpovědi.</span><span class="sxs-lookup"><span data-stu-id="25f22-132">In the action search box, type **response** to list the Response action.</span></span>
   
    ![Vyberte akci, která odpovědi](./media/connectors-native-reqres/using-action-1.png)
4. <span data-ttu-id="25f22-134">Přidejte do žádné parametry, které jsou požadovány pro zprávu odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="25f22-134">Add in any parameters that are required for the HTTP response message.</span></span>
   
    ![Dokončení akce odpovědi](./media/connectors-native-reqres/using-action-2.png)
5. <span data-ttu-id="25f22-136">Klikněte levém horním panelu nástrojů na Uložit a aplikace logiky jak uložíte a publikovat (aktivovat).</span><span class="sxs-lookup"><span data-stu-id="25f22-136">Click the upper-left corner of the toolbar to save, and your logic app will both save and publish (activate).</span></span>

## <a name="request-trigger"></a><span data-ttu-id="25f22-137">Žádost o aktivační události</span><span class="sxs-lookup"><span data-stu-id="25f22-137">Request trigger</span></span>
<span data-ttu-id="25f22-138">Tady jsou uvedené podrobnosti pro aktivační událost, která tento konektor podporuje.</span><span class="sxs-lookup"><span data-stu-id="25f22-138">Here are the details for the trigger that this connector supports.</span></span> <span data-ttu-id="25f22-139">Je aktivační událost jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="25f22-139">There is a single request trigger.</span></span>

| <span data-ttu-id="25f22-140">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="25f22-140">Trigger</span></span> | <span data-ttu-id="25f22-141">Popis</span><span class="sxs-lookup"><span data-stu-id="25f22-141">Description</span></span> |
| --- | --- |
| <span data-ttu-id="25f22-142">Žádost</span><span class="sxs-lookup"><span data-stu-id="25f22-142">Request</span></span> |<span data-ttu-id="25f22-143">Nastane, když obdrží požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="25f22-143">Occurs when an HTTP request is received</span></span> |

## <a name="response-action"></a><span data-ttu-id="25f22-144">Akce odpovědi</span><span class="sxs-lookup"><span data-stu-id="25f22-144">Response action</span></span>
<span data-ttu-id="25f22-145">Tady jsou uvedené podrobnosti pro akci, která tento konektor podporuje.</span><span class="sxs-lookup"><span data-stu-id="25f22-145">Here are the details for the action that this connector supports.</span></span> <span data-ttu-id="25f22-146">Není k dispozici odpověď jedné akci, která lze použít pouze když je přiložena aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="25f22-146">There is a single response action that can only be used when it is accompanied by a request trigger.</span></span>

| <span data-ttu-id="25f22-147">Akce</span><span class="sxs-lookup"><span data-stu-id="25f22-147">Action</span></span> | <span data-ttu-id="25f22-148">Popis</span><span class="sxs-lookup"><span data-stu-id="25f22-148">Description</span></span> |
| --- | --- |
| <span data-ttu-id="25f22-149">Odpověď</span><span class="sxs-lookup"><span data-stu-id="25f22-149">Response</span></span> |<span data-ttu-id="25f22-150">Vrátí odpověď korelační požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="25f22-150">Returns a response to the correlated HTTP request</span></span> |

### <a name="trigger-and-action-details"></a><span data-ttu-id="25f22-151">Podrobnosti o aktivační události a akce</span><span class="sxs-lookup"><span data-stu-id="25f22-151">Trigger and action details</span></span>
<span data-ttu-id="25f22-152">Následující tabulky popisují vstupní pole pro aktivační události a akce, a odpovídající výstupní podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="25f22-152">The following tables describe the input fields for the trigger and action, and the corresponding output details.</span></span>

#### <a name="request-trigger"></a><span data-ttu-id="25f22-153">Žádost o aktivační události</span><span class="sxs-lookup"><span data-stu-id="25f22-153">Request trigger</span></span>
<span data-ttu-id="25f22-154">Zde je vstupní pole pro aktivační událost z příchozí žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="25f22-154">The following is an input field for the trigger from an incoming HTTP request.</span></span>

| <span data-ttu-id="25f22-155">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="25f22-155">Display name</span></span> | <span data-ttu-id="25f22-156">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="25f22-156">Property name</span></span> | <span data-ttu-id="25f22-157">Popis</span><span class="sxs-lookup"><span data-stu-id="25f22-157">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="25f22-158">Schématu JSON</span><span class="sxs-lookup"><span data-stu-id="25f22-158">JSON Schema</span></span> |<span data-ttu-id="25f22-159">Schéma</span><span class="sxs-lookup"><span data-stu-id="25f22-159">schema</span></span> |<span data-ttu-id="25f22-160">JSON schéma požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="25f22-160">The JSON schema of the HTTP request body</span></span> |

<br>

<span data-ttu-id="25f22-161">**Podrobnosti o výstupu**</span><span class="sxs-lookup"><span data-stu-id="25f22-161">**Output details**</span></span>

<span data-ttu-id="25f22-162">Níže jsou uvedeny podrobnosti výstup pro daný požadavek.</span><span class="sxs-lookup"><span data-stu-id="25f22-162">The following are output details for the request.</span></span>

| <span data-ttu-id="25f22-163">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="25f22-163">Property name</span></span> | <span data-ttu-id="25f22-164">Datový typ</span><span class="sxs-lookup"><span data-stu-id="25f22-164">Data type</span></span> | <span data-ttu-id="25f22-165">Popis</span><span class="sxs-lookup"><span data-stu-id="25f22-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="25f22-166">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="25f22-166">Headers</span></span> |<span data-ttu-id="25f22-167">Objekt</span><span class="sxs-lookup"><span data-stu-id="25f22-167">object</span></span> |<span data-ttu-id="25f22-168">Hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="25f22-168">Request headers</span></span> |
| <span data-ttu-id="25f22-169">Tělo</span><span class="sxs-lookup"><span data-stu-id="25f22-169">Body</span></span> |<span data-ttu-id="25f22-170">Objekt</span><span class="sxs-lookup"><span data-stu-id="25f22-170">object</span></span> |<span data-ttu-id="25f22-171">Objekt žádosti</span><span class="sxs-lookup"><span data-stu-id="25f22-171">Request object</span></span> |

#### <a name="response-action"></a><span data-ttu-id="25f22-172">Akce odpovědi</span><span class="sxs-lookup"><span data-stu-id="25f22-172">Response action</span></span>
<span data-ttu-id="25f22-173">Níže jsou uvedeny vstupních polí pro akce HTTP odpovědi.</span><span class="sxs-lookup"><span data-stu-id="25f22-173">The following are input fields for the HTTP Response action.</span></span> <span data-ttu-id="25f22-174">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="25f22-174">A * means that it is a required field.</span></span>

| <span data-ttu-id="25f22-175">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="25f22-175">Display name</span></span> | <span data-ttu-id="25f22-176">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="25f22-176">Property name</span></span> | <span data-ttu-id="25f22-177">Popis</span><span class="sxs-lookup"><span data-stu-id="25f22-177">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="25f22-178">Stavový kód *</span><span class="sxs-lookup"><span data-stu-id="25f22-178">Status Code*</span></span> |<span data-ttu-id="25f22-179">statusCode</span><span class="sxs-lookup"><span data-stu-id="25f22-179">statusCode</span></span> |<span data-ttu-id="25f22-180">Stavový kód protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="25f22-180">The HTTP status code</span></span> |
| <span data-ttu-id="25f22-181">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="25f22-181">Headers</span></span> |<span data-ttu-id="25f22-182">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="25f22-182">headers</span></span> |<span data-ttu-id="25f22-183">Objekt JSON všechny hlavičky odpovědi zahrnout</span><span class="sxs-lookup"><span data-stu-id="25f22-183">A JSON object of any response headers to include</span></span> |
| <span data-ttu-id="25f22-184">Tělo</span><span class="sxs-lookup"><span data-stu-id="25f22-184">Body</span></span> |<span data-ttu-id="25f22-185">Text</span><span class="sxs-lookup"><span data-stu-id="25f22-185">body</span></span> |<span data-ttu-id="25f22-186">Text odpovědi</span><span class="sxs-lookup"><span data-stu-id="25f22-186">The response body</span></span> |

## <a name="next-steps"></a><span data-ttu-id="25f22-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25f22-187">Next steps</span></span>
<span data-ttu-id="25f22-188">Teď vyzkoušet platformu a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="25f22-188">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="25f22-189">Ostatní konektory k dispozici v aplikace logiky můžete prozkoumat pohledem na našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="25f22-189">You can explore the other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

