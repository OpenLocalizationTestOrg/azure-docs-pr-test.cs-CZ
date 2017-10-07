---
title: "Akce požadavku a odpovědi aaaUse | Microsoft Docs"
description: "Přehled hello žádostí a odpovědí aktivační události a akce v aplikaci Azure logiky"
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
ms.openlocfilehash: 24c378cc12d5f3f65116d5e59278236186a99662
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-request-and-response-components"></a><span data-ttu-id="e89a5-103">Začínáme s komponentami hello žádosti a odpovědi</span><span class="sxs-lookup"><span data-stu-id="e89a5-103">Get started with hello request and response components</span></span>
<span data-ttu-id="e89a5-104">S hello součásti v aplikaci logiky žádostí a odpovědí může reagovat v reálném čase tooevents.</span><span class="sxs-lookup"><span data-stu-id="e89a5-104">With hello request and response components in a logic app, you can respond in real time tooevents.</span></span>

<span data-ttu-id="e89a5-105">Můžete například provést následující věci:</span><span class="sxs-lookup"><span data-stu-id="e89a5-105">For example, you can:</span></span>

* <span data-ttu-id="e89a5-106">Odpověď žádost HTTP tooan s daty z místní databáze pomocí aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="e89a5-106">Respond tooan HTTP request with data from an on-premises database through a logic app.</span></span>
* <span data-ttu-id="e89a5-107">Aktivovat aplikaci logiky z externí webhooku události.</span><span class="sxs-lookup"><span data-stu-id="e89a5-107">Trigger a logic app from an external webhook event.</span></span>
* <span data-ttu-id="e89a5-108">Volání aplikace logiky pomocí akce žádostí a odpovědí z v rámci jiné aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="e89a5-108">Call a logic app with a request and response action from within another logic app.</span></span>

<span data-ttu-id="e89a5-109">tooget spuštění pomocí hello žádostí a odpovědí akce v aplikaci logiky, aplikaci najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="e89a5-109">tooget started using hello request and response actions in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-http-request-trigger"></a><span data-ttu-id="e89a5-110">Použití hello aktivační událost požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="e89a5-110">Use hello HTTP Request trigger</span></span>
<span data-ttu-id="e89a5-111">Aktivační událost je událost, která lze použít toostart hello workflow, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="e89a5-111">A trigger is an event that can be used toostart hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="e89a5-112">[Další informace o aktivační události](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e89a5-112">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="e89a5-113">Zde je příklad posloupnosti jak tooset zálohu protokolu HTTP žádosti v hello návrhář aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="e89a5-113">Here’s an example sequence of how tooset up an HTTP request in hello Logic App Designer.</span></span>

1. <span data-ttu-id="e89a5-114">Přidat aktivační událost hello **požadavku - obdrží žádost HTTP při** v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="e89a5-114">Add hello trigger **Request - When an HTTP request is received** in your logic app.</span></span> <span data-ttu-id="e89a5-115">Volitelně můžete zadat schématu JSON (pomocí některého nástroje, například [JSONSchema.net](http://jsonschema.net)) tělo žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="e89a5-115">You can optionally provide a JSON schema (by using a tool like [JSONSchema.net](http://jsonschema.net)) for hello request body.</span></span> <span data-ttu-id="e89a5-116">To umožňuje hello návrháře toogenerate tokeny pro vlastnosti v žádosti o hello protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e89a5-116">This allows hello designer toogenerate tokens for properties in hello HTTP request.</span></span>
2. <span data-ttu-id="e89a5-117">Přidáte další akci, takže můžete uložit aplikaci logiky hello.</span><span class="sxs-lookup"><span data-stu-id="e89a5-117">Add another action so that you can save hello logic app.</span></span>
3. <span data-ttu-id="e89a5-118">Po uložení hello aplikace logiky, můžete získat hello HTTP požadavku na adresu URL z karty hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="e89a5-118">After saving hello logic app, you can get hello HTTP request URL from hello request card.</span></span>
4. <span data-ttu-id="e89a5-119">HTTP POST (můžete použít nástroje, jako je [Postman](https://www.getpostman.com/)) aktivace adresy URL toohello hello aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="e89a5-119">An HTTP POST (you can use a tool like [Postman](https://www.getpostman.com/)) toohello URL triggers hello logic app.</span></span>

> [!NOTE]
> <span data-ttu-id="e89a5-120">Pokud nedefinujete akce odpovědi, `202 ACCEPTED` odpověď se vrátí okamžitě toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="e89a5-120">If you don't define a response action, a `202 ACCEPTED` response is immediately returned toohello caller.</span></span> <span data-ttu-id="e89a5-121">Můžete použít hello odpovědi akce toocustomize odpověď.</span><span class="sxs-lookup"><span data-stu-id="e89a5-121">You can use hello response action toocustomize a response.</span></span>
> 
> 

![Aktivační událost odpovědi](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-hello-http-response-action"></a><span data-ttu-id="e89a5-123">Použití hello akce odpovědi HTTP</span><span class="sxs-lookup"><span data-stu-id="e89a5-123">Use hello HTTP Response action</span></span>
<span data-ttu-id="e89a5-124">Hello odpověď HTTP akce je platná pouze při použití v pracovním postupu, která se spustí při požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="e89a5-124">hello HTTP Response action is only valid when you use it in a workflow that is triggered by an HTTP request.</span></span> <span data-ttu-id="e89a5-125">Pokud nedefinujete akce odpovědi, `202 ACCEPTED` odpověď se vrátí okamžitě toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="e89a5-125">If you don't define a response action, a `202 ACCEPTED` response is immediately returned toohello caller.</span></span>  <span data-ttu-id="e89a5-126">Akce odpovědi můžete přidat v kterémkoliv kroku v rámci pracovního postupu hello.</span><span class="sxs-lookup"><span data-stu-id="e89a5-126">You can add a response action at any step within hello workflow.</span></span> <span data-ttu-id="e89a5-127">aplikace logiky Hello pouze udržuje příchozího požadavku hello otevřete jednu minutu pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="e89a5-127">hello logic app only keeps hello incoming request open for one minute for a response.</span></span>  <span data-ttu-id="e89a5-128">Za minutu, pokud žádná odpověď byla odeslána z pracovního postupu hello (a existuje akce odpovědi v definici hello) `504 GATEWAY TIMEOUT` je vrácen toohello volajícího.</span><span class="sxs-lookup"><span data-stu-id="e89a5-128">After one minute, if no response was sent from hello workflow (and a response action exists in hello definition), a `504 GATEWAY TIMEOUT` is returned toohello caller.</span></span>

<span data-ttu-id="e89a5-129">Tady je způsob tooadd akce odpovědi HTTP:</span><span class="sxs-lookup"><span data-stu-id="e89a5-129">Here's how tooadd an HTTP Response action:</span></span>

1. <span data-ttu-id="e89a5-130">Vyberte hello **nový krok** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e89a5-130">Select hello **New Step** button.</span></span>
2. <span data-ttu-id="e89a5-131">Zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="e89a5-131">Choose **Add an action**.</span></span>
3. <span data-ttu-id="e89a5-132">Hello akce vyhledávacího pole zadejte **odpovědi** toolist hello akce odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e89a5-132">In hello action search box, type **response** toolist hello Response action.</span></span>
   
    ![Vyberte akci odpovědi hello](./media/connectors-native-reqres/using-action-1.png)
4. <span data-ttu-id="e89a5-134">Přidejte do žádné parametry, které jsou požadovány pro zprávu odpovědi HTTP hello.</span><span class="sxs-lookup"><span data-stu-id="e89a5-134">Add in any parameters that are required for hello HTTP response message.</span></span>
   
    ![Akce odpovědi dokončení hello](./media/connectors-native-reqres/using-action-2.png)
5. <span data-ttu-id="e89a5-136">Klikněte na tlačítko hello levého horního rohu toosave hello panelu nástrojů a vaše logiku aplikace uloží obou a publikování (aktivovat).</span><span class="sxs-lookup"><span data-stu-id="e89a5-136">Click hello upper-left corner of hello toolbar toosave, and your logic app will both save and publish (activate).</span></span>

## <a name="request-trigger"></a><span data-ttu-id="e89a5-137">Žádost o aktivační události</span><span class="sxs-lookup"><span data-stu-id="e89a5-137">Request trigger</span></span>
<span data-ttu-id="e89a5-138">Tady jsou hello podrobnosti hello aktivační událost, která tento konektor podporuje.</span><span class="sxs-lookup"><span data-stu-id="e89a5-138">Here are hello details for hello trigger that this connector supports.</span></span> <span data-ttu-id="e89a5-139">Je aktivační událost jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="e89a5-139">There is a single request trigger.</span></span>

| <span data-ttu-id="e89a5-140">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="e89a5-140">Trigger</span></span> | <span data-ttu-id="e89a5-141">Popis</span><span class="sxs-lookup"><span data-stu-id="e89a5-141">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e89a5-142">Žádost</span><span class="sxs-lookup"><span data-stu-id="e89a5-142">Request</span></span> |<span data-ttu-id="e89a5-143">Nastane, když obdrží požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="e89a5-143">Occurs when an HTTP request is received</span></span> |

## <a name="response-action"></a><span data-ttu-id="e89a5-144">Akce odpovědi</span><span class="sxs-lookup"><span data-stu-id="e89a5-144">Response action</span></span>
<span data-ttu-id="e89a5-145">Tady jsou hello podrobnosti hello akce, který podporuje tento konektor.</span><span class="sxs-lookup"><span data-stu-id="e89a5-145">Here are hello details for hello action that this connector supports.</span></span> <span data-ttu-id="e89a5-146">Není k dispozici odpověď jedné akci, která lze použít pouze když je přiložena aktivační událost požadavku.</span><span class="sxs-lookup"><span data-stu-id="e89a5-146">There is a single response action that can only be used when it is accompanied by a request trigger.</span></span>

| <span data-ttu-id="e89a5-147">Akce</span><span class="sxs-lookup"><span data-stu-id="e89a5-147">Action</span></span> | <span data-ttu-id="e89a5-148">Popis</span><span class="sxs-lookup"><span data-stu-id="e89a5-148">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e89a5-149">Odpověď</span><span class="sxs-lookup"><span data-stu-id="e89a5-149">Response</span></span> |<span data-ttu-id="e89a5-150">Vrátí že odpověď toohello korelační požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="e89a5-150">Returns a response toohello correlated HTTP request</span></span> |

### <a name="trigger-and-action-details"></a><span data-ttu-id="e89a5-151">Podrobnosti o aktivační události a akce</span><span class="sxs-lookup"><span data-stu-id="e89a5-151">Trigger and action details</span></span>
<span data-ttu-id="e89a5-152">Hello následující tabulky popisují hello vstupní pole pro aktivační událost hello a akce a odpovídající podrobnosti výstup hello.</span><span class="sxs-lookup"><span data-stu-id="e89a5-152">hello following tables describe hello input fields for hello trigger and action, and hello corresponding output details.</span></span>

#### <a name="request-trigger"></a><span data-ttu-id="e89a5-153">Žádost o aktivační události</span><span class="sxs-lookup"><span data-stu-id="e89a5-153">Request trigger</span></span>
<span data-ttu-id="e89a5-154">Hello následuje vstupní pole pro aktivační událost hello z příchozí žádosti protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e89a5-154">hello following is an input field for hello trigger from an incoming HTTP request.</span></span>

| <span data-ttu-id="e89a5-155">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="e89a5-155">Display name</span></span> | <span data-ttu-id="e89a5-156">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e89a5-156">Property name</span></span> | <span data-ttu-id="e89a5-157">Popis</span><span class="sxs-lookup"><span data-stu-id="e89a5-157">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e89a5-158">Schématu JSON</span><span class="sxs-lookup"><span data-stu-id="e89a5-158">JSON Schema</span></span> |<span data-ttu-id="e89a5-159">Schéma</span><span class="sxs-lookup"><span data-stu-id="e89a5-159">schema</span></span> |<span data-ttu-id="e89a5-160">schéma JSON Hello obsahu žádosti HTTP hello</span><span class="sxs-lookup"><span data-stu-id="e89a5-160">hello JSON schema of hello HTTP request body</span></span> |

<br>

<span data-ttu-id="e89a5-161">**Podrobnosti o výstupu**</span><span class="sxs-lookup"><span data-stu-id="e89a5-161">**Output details**</span></span>

<span data-ttu-id="e89a5-162">Hello následují výstup podrobnosti požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="e89a5-162">hello following are output details for hello request.</span></span>

| <span data-ttu-id="e89a5-163">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e89a5-163">Property name</span></span> | <span data-ttu-id="e89a5-164">Datový typ</span><span class="sxs-lookup"><span data-stu-id="e89a5-164">Data type</span></span> | <span data-ttu-id="e89a5-165">Popis</span><span class="sxs-lookup"><span data-stu-id="e89a5-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e89a5-166">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="e89a5-166">Headers</span></span> |<span data-ttu-id="e89a5-167">Objekt</span><span class="sxs-lookup"><span data-stu-id="e89a5-167">object</span></span> |<span data-ttu-id="e89a5-168">Hlavičky požadavku</span><span class="sxs-lookup"><span data-stu-id="e89a5-168">Request headers</span></span> |
| <span data-ttu-id="e89a5-169">Tělo</span><span class="sxs-lookup"><span data-stu-id="e89a5-169">Body</span></span> |<span data-ttu-id="e89a5-170">Objekt</span><span class="sxs-lookup"><span data-stu-id="e89a5-170">object</span></span> |<span data-ttu-id="e89a5-171">Objekt žádosti</span><span class="sxs-lookup"><span data-stu-id="e89a5-171">Request object</span></span> |

#### <a name="response-action"></a><span data-ttu-id="e89a5-172">Akce odpovědi</span><span class="sxs-lookup"><span data-stu-id="e89a5-172">Response action</span></span>
<span data-ttu-id="e89a5-173">Hello následují vstupní pole pro hello akce odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e89a5-173">hello following are input fields for hello HTTP Response action.</span></span> <span data-ttu-id="e89a5-174">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="e89a5-174">A * means that it is a required field.</span></span>

| <span data-ttu-id="e89a5-175">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="e89a5-175">Display name</span></span> | <span data-ttu-id="e89a5-176">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e89a5-176">Property name</span></span> | <span data-ttu-id="e89a5-177">Popis</span><span class="sxs-lookup"><span data-stu-id="e89a5-177">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e89a5-178">Stavový kód *</span><span class="sxs-lookup"><span data-stu-id="e89a5-178">Status Code*</span></span> |<span data-ttu-id="e89a5-179">statusCode</span><span class="sxs-lookup"><span data-stu-id="e89a5-179">statusCode</span></span> |<span data-ttu-id="e89a5-180">Hello stavový kód protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="e89a5-180">hello HTTP status code</span></span> |
| <span data-ttu-id="e89a5-181">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="e89a5-181">Headers</span></span> |<span data-ttu-id="e89a5-182">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="e89a5-182">headers</span></span> |<span data-ttu-id="e89a5-183">Objekt JSON žádné tooinclude hlavičky odpovědi</span><span class="sxs-lookup"><span data-stu-id="e89a5-183">A JSON object of any response headers tooinclude</span></span> |
| <span data-ttu-id="e89a5-184">Tělo</span><span class="sxs-lookup"><span data-stu-id="e89a5-184">Body</span></span> |<span data-ttu-id="e89a5-185">Text</span><span class="sxs-lookup"><span data-stu-id="e89a5-185">body</span></span> |<span data-ttu-id="e89a5-186">text odpovědi Hello</span><span class="sxs-lookup"><span data-stu-id="e89a5-186">hello response body</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e89a5-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e89a5-187">Next steps</span></span>
<span data-ttu-id="e89a5-188">Teď vyzkoušet hello platformy a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="e89a5-188">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="e89a5-189">Můžete prozkoumat hello dalších dostupných konektorů v aplikacích logiky pohledem na našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="e89a5-189">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

