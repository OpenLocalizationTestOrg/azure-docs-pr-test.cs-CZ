---
title: "Komunikovat s žádný koncový bod přes protokol HTTP - Azure Logic Apps | Microsoft Docs"
description: "Vytvoření aplikace logiky, která může komunikovat s žádný koncový bod přes protokol HTTP"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: d422a07a27ffa62a673bd2d471ae4fc837251dee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-http-action"></a><span data-ttu-id="1d1a8-103">Začínáme s akce HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-103">Get started with the HTTP action</span></span>

<span data-ttu-id="1d1a8-104">Pomocí akce HTTP můžete rozšířit pracovní postupy pro vaši organizaci a komunikaci pomocí protokolu HTTP pro libovolný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-104">With the HTTP action, you can extend workflows for your organization and communicate to any endpoint over HTTP.</span></span>

<span data-ttu-id="1d1a8-105">Můžete:</span><span class="sxs-lookup"><span data-stu-id="1d1a8-105">You can:</span></span>

* <span data-ttu-id="1d1a8-106">Vytvořte logiku aplikace pracovní postupy, které aktivovat (aktivační události), kdy web, který spravujete přestane fungovat.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-106">Create logic app workflows that activate (trigger) when a website that you manage goes down.</span></span>
* <span data-ttu-id="1d1a8-107">Komunikovat s žádný koncový bod přes protokol HTTP rozšířit vaše pracovní postupy k dalším službám.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-107">Communicate to any endpoint over HTTP to extend your workflows into other services.</span></span>

<span data-ttu-id="1d1a8-108">Chcete-li začít používat akce HTTP v aplikaci logiky, přečtěte si téma [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1d1a8-108">To get started using the HTTP action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-the-http-trigger"></a><span data-ttu-id="1d1a8-109">Pomocí triggeru protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-109">Use the HTTP trigger</span></span>
<span data-ttu-id="1d1a8-110">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-110">A trigger is an event that can be used to start the workflow that is defined in a logic app.</span></span> <span data-ttu-id="1d1a8-111">[Další informace o aktivační události](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1d1a8-111">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="1d1a8-112">Zde je v sekvenci příklad toho, jak nastavit triggeru protokolu HTTP v Návrháři logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-112">Here’s an example sequence of how to set up the HTTP trigger in the Logic App Designer.</span></span>

1. <span data-ttu-id="1d1a8-113">Přidejte triggeru protokolu HTTP v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-113">Add the HTTP trigger in your logic app.</span></span>
2. <span data-ttu-id="1d1a8-114">Zadejte parametry pro koncový bod HTTP, které chcete dotazovat.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-114">Fill in the parameters for the HTTP endpoint that you want to poll.</span></span>
3. <span data-ttu-id="1d1a8-115">Změna intervalu opakování na tom, jak často má dotazovat.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-115">Modify the recurrence interval on how frequently it should poll.</span></span>

   <span data-ttu-id="1d1a8-116">Aplikace logiky teď aktivuje s obsahem, který je vrácen během každé kontroly.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-116">The logic app now fires with any content that is returned during each check.</span></span>

   ![Trigger HTTP](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a><span data-ttu-id="1d1a8-118">Jak funguje triggeru protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-118">How the HTTP trigger works</span></span>

<span data-ttu-id="1d1a8-119">Aktivace protokolu HTTP odešle volání koncový bod protokolu HTTP na intervalu opakování.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-119">The HTTP trigger sends a call to HTTP endpoint on a recurring interval.</span></span> <span data-ttu-id="1d1a8-120">Ve výchozím kódu odpovědi HTTP, která je nižší než 300 způsobí, že aplikace logiky ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-120">By default, any HTTP response code that is lower than 300 causes a logic app to run.</span></span> <span data-ttu-id="1d1a8-121">Chcete-li určit, zda by měly aktivovat aplikaci logiky, můžete upravit aplikaci logiky v zobrazení kódu a přidejte podmínku, která vyhodnotí po volání protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-121">To specify whether the logic app should fire, you can edit the logic app in code view, and add a condition that evaluates after the HTTP call.</span></span> <span data-ttu-id="1d1a8-122">Tady je příklad HTTP aktivační událost, která aktivuje se v případě vrácený kód stavu je větší než nebo rovno `400`.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-122">Here's an example of an HTTP trigger that fires when the returned status code is greater than or equal to `400`.</span></span>

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

<span data-ttu-id="1d1a8-123">Úplné podrobnosti o parametrech aktivace protokolu HTTP jsou k dispozici na [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).</span><span class="sxs-lookup"><span data-stu-id="1d1a8-123">Full details about the HTTP trigger parameters are available on [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).</span></span>

## <a name="use-the-http-action"></a><span data-ttu-id="1d1a8-124">Pomocí akce HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-124">Use the HTTP action</span></span>

<span data-ttu-id="1d1a8-125">Akce je operace, která se provádí v pracovním postupu, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-125">An action is an operation that is carried out by the workflow that is defined in a logic app.</span></span> 
<span data-ttu-id="1d1a8-126">[Další informace o akcích](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1d1a8-126">[Learn more about actions](connectors-overview.md).</span></span>

1. <span data-ttu-id="1d1a8-127">Zvolte **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-127">Choose **New Step** > **Add an action**.</span></span>
3. <span data-ttu-id="1d1a8-128">Zadejte do vyhledávacího pole Akce **http** seznam akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-128">In the action search box, type **http** to list the HTTP actions.</span></span>
   
    ![Vyberte akci HTTP](./media/connectors-native-http/using-action-1.png)

4. <span data-ttu-id="1d1a8-130">Přidejte všechny požadované parametry pro volání protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-130">Add any required parameters for the HTTP call.</span></span>
   
    ![Dokončení akce HTTP](./media/connectors-native-http/using-action-2.png)

5. <span data-ttu-id="1d1a8-132">Na panelu nástrojů návrháře, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-132">On the designer toolbar, click **Save**.</span></span> <span data-ttu-id="1d1a8-133">Aplikace logiky uložení a publikování (aktivovaný) ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-133">Your logic app is saved and published (activated) at the same time.</span></span>

## <a name="http-trigger"></a><span data-ttu-id="1d1a8-134">Trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-134">HTTP trigger</span></span>
<span data-ttu-id="1d1a8-135">Tady jsou uvedené podrobnosti pro aktivační událost, která tento konektor podporuje.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-135">Here are the details for the trigger that this connector supports.</span></span> <span data-ttu-id="1d1a8-136">Konektor protokolu HTTP má jedna aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-136">The HTTP connector has one trigger.</span></span>

| <span data-ttu-id="1d1a8-137">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="1d1a8-137">Trigger</span></span> | <span data-ttu-id="1d1a8-138">Popis</span><span class="sxs-lookup"><span data-stu-id="1d1a8-138">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1d1a8-139">HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-139">HTTP</span></span> |<span data-ttu-id="1d1a8-140">Provede volání protokolu HTTP a vrátí obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-140">Makes an HTTP call and returns the response content.</span></span> |

## <a name="http-action"></a><span data-ttu-id="1d1a8-141">Akce HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-141">HTTP action</span></span>
<span data-ttu-id="1d1a8-142">Tady jsou uvedené podrobnosti pro akci, která tento konektor podporuje.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-142">Here are the details for the action that this connector supports.</span></span> <span data-ttu-id="1d1a8-143">Konektor protokolu HTTP má jednu možné akci.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-143">The HTTP connector has one possible action.</span></span>

| <span data-ttu-id="1d1a8-144">Akce</span><span class="sxs-lookup"><span data-stu-id="1d1a8-144">Action</span></span> | <span data-ttu-id="1d1a8-145">Popis</span><span class="sxs-lookup"><span data-stu-id="1d1a8-145">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1d1a8-146">HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-146">HTTP</span></span> |<span data-ttu-id="1d1a8-147">Provede volání protokolu HTTP a vrátí obsahu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-147">Makes an HTTP call and returns the response content.</span></span> |

## <a name="http-details"></a><span data-ttu-id="1d1a8-148">Podrobnosti protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-148">HTTP details</span></span>
<span data-ttu-id="1d1a8-149">Následující tabulky popisují požadované a volitelné vstupních polí pro akce a odpovídající výstup podrobnosti, které jsou spojené s použitím akce.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-149">The following tables describe the required and optional input fields for the action and the corresponding output details that are associated with using the action.</span></span>

#### <a name="http-request"></a><span data-ttu-id="1d1a8-150">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-150">HTTP request</span></span>
<span data-ttu-id="1d1a8-151">Níže jsou uvedeny vstupních polí pro akce, která umožňuje odchozí požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-151">The following are input fields for the action, which makes an HTTP outbound request.</span></span>
<span data-ttu-id="1d1a8-152">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-152">A * means that it is a required field.</span></span>

| <span data-ttu-id="1d1a8-153">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="1d1a8-153">Display name</span></span> | <span data-ttu-id="1d1a8-154">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1d1a8-154">Property name</span></span> | <span data-ttu-id="1d1a8-155">Popis</span><span class="sxs-lookup"><span data-stu-id="1d1a8-155">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d1a8-156">Metoda *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-156">Method*</span></span> |<span data-ttu-id="1d1a8-157">– Metoda</span><span class="sxs-lookup"><span data-stu-id="1d1a8-157">method</span></span> |<span data-ttu-id="1d1a8-158">Příkaz protokolu HTTP k použití</span><span class="sxs-lookup"><span data-stu-id="1d1a8-158">The HTTP verb to use</span></span> |
| <span data-ttu-id="1d1a8-159">IDENTIFIKÁTOR URI *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-159">URI*</span></span> |<span data-ttu-id="1d1a8-160">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="1d1a8-160">uri</span></span> |<span data-ttu-id="1d1a8-161">Identifikátor URI pro požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-161">The URI for the HTTP request</span></span> |
| <span data-ttu-id="1d1a8-162">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="1d1a8-162">Headers</span></span> |<span data-ttu-id="1d1a8-163">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="1d1a8-163">headers</span></span> |<span data-ttu-id="1d1a8-164">Objekt JSON zahrnují hlaviček protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-164">A JSON object of HTTP headers to include</span></span> |
| <span data-ttu-id="1d1a8-165">Tělo</span><span class="sxs-lookup"><span data-stu-id="1d1a8-165">Body</span></span> |<span data-ttu-id="1d1a8-166">Text</span><span class="sxs-lookup"><span data-stu-id="1d1a8-166">body</span></span> |<span data-ttu-id="1d1a8-167">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-167">The HTTP request body</span></span> |
| <span data-ttu-id="1d1a8-168">Authentication</span><span class="sxs-lookup"><span data-stu-id="1d1a8-168">Authentication</span></span> |<span data-ttu-id="1d1a8-169">Ověřování</span><span class="sxs-lookup"><span data-stu-id="1d1a8-169">authentication</span></span> |<span data-ttu-id="1d1a8-170">Podrobnosti v [ověřování](#authentication) části</span><span class="sxs-lookup"><span data-stu-id="1d1a8-170">Details in the [Authentication](#authentication) section</span></span> |

<br>

#### <a name="output-details"></a><span data-ttu-id="1d1a8-171">Podrobnosti o výstupu</span><span class="sxs-lookup"><span data-stu-id="1d1a8-171">Output details</span></span>
<span data-ttu-id="1d1a8-172">Níže jsou uvedeny podrobnosti výstup pro odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-172">The following are output details for the HTTP response.</span></span>

| <span data-ttu-id="1d1a8-173">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1d1a8-173">Property name</span></span> | <span data-ttu-id="1d1a8-174">Datový typ</span><span class="sxs-lookup"><span data-stu-id="1d1a8-174">Data type</span></span> | <span data-ttu-id="1d1a8-175">Popis</span><span class="sxs-lookup"><span data-stu-id="1d1a8-175">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d1a8-176">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="1d1a8-176">Headers</span></span> |<span data-ttu-id="1d1a8-177">Objekt</span><span class="sxs-lookup"><span data-stu-id="1d1a8-177">object</span></span> |<span data-ttu-id="1d1a8-178">Hlavičky odpovědi</span><span class="sxs-lookup"><span data-stu-id="1d1a8-178">Response headers</span></span> |
| <span data-ttu-id="1d1a8-179">Tělo</span><span class="sxs-lookup"><span data-stu-id="1d1a8-179">Body</span></span> |<span data-ttu-id="1d1a8-180">Objekt</span><span class="sxs-lookup"><span data-stu-id="1d1a8-180">object</span></span> |<span data-ttu-id="1d1a8-181">Objekt odpovědi</span><span class="sxs-lookup"><span data-stu-id="1d1a8-181">Response object</span></span> |
| <span data-ttu-id="1d1a8-182">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="1d1a8-182">Status Code</span></span> |<span data-ttu-id="1d1a8-183">celá čísla</span><span class="sxs-lookup"><span data-stu-id="1d1a8-183">int</span></span> |<span data-ttu-id="1d1a8-184">Stavový kód protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="1d1a8-184">HTTP status code</span></span> |

## <a name="authentication"></a><span data-ttu-id="1d1a8-185">Authentication</span><span class="sxs-lookup"><span data-stu-id="1d1a8-185">Authentication</span></span>
<span data-ttu-id="1d1a8-186">Funkce Logic Apps umožňuje používat různé typy ověřování proti koncových bodů protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-186">The Logic Apps feature allows you to use different types of authentication against HTTP endpoints.</span></span> <span data-ttu-id="1d1a8-187">Můžete použít tento ověřování s **HTTP**,  **[HTTP + Swagger](connectors-native-http-swagger.md)**, a  **[Webhooku protokolu HTTP](connectors-native-webhook.md)**  konektory.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-187">You can use this authentication with the **HTTP**, **[HTTP + Swagger](connectors-native-http-swagger.md)**, and **[HTTP Webhook](connectors-native-webhook.md)** connectors.</span></span> <span data-ttu-id="1d1a8-188">Následující typy ověřování se dají konfigurovat:</span><span class="sxs-lookup"><span data-stu-id="1d1a8-188">The following types of authentication are configurable:</span></span>

* [<span data-ttu-id="1d1a8-189">Základní ověřování</span><span class="sxs-lookup"><span data-stu-id="1d1a8-189">Basic authentication</span></span>](#basic-authentication)
* [<span data-ttu-id="1d1a8-190">Ověřování certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="1d1a8-190">Client certificate authentication</span></span>](#client-certificate-authentication)
* [<span data-ttu-id="1d1a8-191">Ověřování Azure Active Directory (Azure AD) OAuth</span><span class="sxs-lookup"><span data-stu-id="1d1a8-191">Azure Active Directory (Azure AD) OAuth authentication</span></span>](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a><span data-ttu-id="1d1a8-192">Základní ověřování</span><span class="sxs-lookup"><span data-stu-id="1d1a8-192">Basic authentication</span></span>

<span data-ttu-id="1d1a8-193">Následující objekt ověřování je potřeba pro základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-193">The following authentication object is needed for basic authentication.</span></span>
<span data-ttu-id="1d1a8-194">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-194">A * means that it is a required field.</span></span>

| <span data-ttu-id="1d1a8-195">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1d1a8-195">Property name</span></span> | <span data-ttu-id="1d1a8-196">Datový typ</span><span class="sxs-lookup"><span data-stu-id="1d1a8-196">Data type</span></span> | <span data-ttu-id="1d1a8-197">Popis</span><span class="sxs-lookup"><span data-stu-id="1d1a8-197">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d1a8-198">Typ *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-198">Type*</span></span> |<span data-ttu-id="1d1a8-199">type</span><span class="sxs-lookup"><span data-stu-id="1d1a8-199">type</span></span> |<span data-ttu-id="1d1a8-200">Typ ověřování (musí být `Basic` pro základní ověřování)</span><span class="sxs-lookup"><span data-stu-id="1d1a8-200">Type of authentication (must be `Basic` for basic authentication)</span></span> |
| <span data-ttu-id="1d1a8-201">Uživatelské jméno *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-201">Username*</span></span> |<span data-ttu-id="1d1a8-202">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="1d1a8-202">username</span></span> |<span data-ttu-id="1d1a8-203">Uživatelské jméno k ověření</span><span class="sxs-lookup"><span data-stu-id="1d1a8-203">User name to authenticate</span></span> |
| <span data-ttu-id="1d1a8-204">Heslo *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-204">Password*</span></span> |<span data-ttu-id="1d1a8-205">heslo</span><span class="sxs-lookup"><span data-stu-id="1d1a8-205">password</span></span> |<span data-ttu-id="1d1a8-206">Heslo k ověření</span><span class="sxs-lookup"><span data-stu-id="1d1a8-206">Password to authenticate</span></span> |

> [!TIP]
> <span data-ttu-id="1d1a8-207">Pokud chcete použít heslo, které nelze načíst z definice, použití `securestring` parametr a `@parameters()`  
>  [funkce definice pracovního postupu](http://aka.ms/logicappdocs).</span><span class="sxs-lookup"><span data-stu-id="1d1a8-207">If you want to use a password that cannot be retrieved from the definition, use a `securestring` parameter and the `@parameters()` 
> [workflow definition function](http://aka.ms/logicappdocs).</span></span>

<span data-ttu-id="1d1a8-208">Například:</span><span class="sxs-lookup"><span data-stu-id="1d1a8-208">For example:</span></span>

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a><span data-ttu-id="1d1a8-209">Ověřování klientských certifikátů</span><span class="sxs-lookup"><span data-stu-id="1d1a8-209">Client certificate authentication</span></span>

<span data-ttu-id="1d1a8-210">Následující objekt ověřování je potřeba pro ověřování pomocí certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-210">The following authentication object is needed for client certificate authentication.</span></span> <span data-ttu-id="1d1a8-211">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-211">A * means that it is a required field.</span></span>

| <span data-ttu-id="1d1a8-212">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1d1a8-212">Property name</span></span> | <span data-ttu-id="1d1a8-213">Datový typ</span><span class="sxs-lookup"><span data-stu-id="1d1a8-213">Data type</span></span> | <span data-ttu-id="1d1a8-214">Popis</span><span class="sxs-lookup"><span data-stu-id="1d1a8-214">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d1a8-215">Typ *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-215">Type*</span></span> |<span data-ttu-id="1d1a8-216">type</span><span class="sxs-lookup"><span data-stu-id="1d1a8-216">type</span></span> |<span data-ttu-id="1d1a8-217">Typ ověřování (musí být `ClientCertificate` pro klientské certifikáty SSL)</span><span class="sxs-lookup"><span data-stu-id="1d1a8-217">The type of authentication (must be `ClientCertificate` for SSL client certificates)</span></span> |
| <span data-ttu-id="1d1a8-218">SOUBOR PFX *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-218">PFX*</span></span> |<span data-ttu-id="1d1a8-219">Soubor PFX</span><span class="sxs-lookup"><span data-stu-id="1d1a8-219">pfx</span></span> |<span data-ttu-id="1d1a8-220">Obsah kódováním Base64 soubor Personal Information Exchange (PFX)</span><span class="sxs-lookup"><span data-stu-id="1d1a8-220">The Base64-encoded contents of the Personal Information Exchange (PFX) file</span></span> |
| <span data-ttu-id="1d1a8-221">Heslo *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-221">Password*</span></span> |<span data-ttu-id="1d1a8-222">heslo</span><span class="sxs-lookup"><span data-stu-id="1d1a8-222">password</span></span> |<span data-ttu-id="1d1a8-223">Heslo pro přístup k souboru PFX</span><span class="sxs-lookup"><span data-stu-id="1d1a8-223">The password to access the PFX file</span></span> |

> [!TIP]
> <span data-ttu-id="1d1a8-224">Chcete-li použít parametr, který nebude možné číst v definici po uložení aplikaci logiky, můžete použít `securestring` parametr a `@parameters()`  
>  [funkce definice pracovního postupu](http://aka.ms/logicappdocs).</span><span class="sxs-lookup"><span data-stu-id="1d1a8-224">To use a parameter that won't be readable in the definition after saving the logic app, you can use a `securestring` parameter and the `@parameters()` 
> [workflow definition function](http://aka.ms/logicappdocs).</span></span>

<span data-ttu-id="1d1a8-225">Například:</span><span class="sxs-lookup"><span data-stu-id="1d1a8-225">For example:</span></span>

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a><span data-ttu-id="1d1a8-226">Ověřování služby Azure AD OAuth</span><span class="sxs-lookup"><span data-stu-id="1d1a8-226">Azure AD OAuth authentication</span></span>
<span data-ttu-id="1d1a8-227">Následující objekt ověřování je potřeba pro ověřování Azure AD OAuth.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-227">The following authentication object is needed for Azure AD OAuth authentication.</span></span> <span data-ttu-id="1d1a8-228">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-228">A * means that it is a required field.</span></span>

| <span data-ttu-id="1d1a8-229">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1d1a8-229">Property name</span></span> | <span data-ttu-id="1d1a8-230">Datový typ</span><span class="sxs-lookup"><span data-stu-id="1d1a8-230">Data type</span></span> | <span data-ttu-id="1d1a8-231">Popis</span><span class="sxs-lookup"><span data-stu-id="1d1a8-231">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d1a8-232">Typ *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-232">Type*</span></span> |<span data-ttu-id="1d1a8-233">type</span><span class="sxs-lookup"><span data-stu-id="1d1a8-233">type</span></span> |<span data-ttu-id="1d1a8-234">Typ ověřování (musí být `ActiveDirectoryOAuth` pro Azure AD OAuth)</span><span class="sxs-lookup"><span data-stu-id="1d1a8-234">The type of authentication (must be `ActiveDirectoryOAuth` for Azure AD OAuth)</span></span> |
| <span data-ttu-id="1d1a8-235">Klienta *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-235">Tenant*</span></span> |<span data-ttu-id="1d1a8-236">Klienta</span><span class="sxs-lookup"><span data-stu-id="1d1a8-236">tenant</span></span> |<span data-ttu-id="1d1a8-237">Identifikátor klienta pro klienta Azure AD</span><span class="sxs-lookup"><span data-stu-id="1d1a8-237">The tenant identifier for the Azure AD tenant</span></span> |
| <span data-ttu-id="1d1a8-238">Cílová skupina *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-238">Audience*</span></span> |<span data-ttu-id="1d1a8-239">Cílová skupina</span><span class="sxs-lookup"><span data-stu-id="1d1a8-239">audience</span></span> |<span data-ttu-id="1d1a8-240">Prostředek se požaduje autorizaci používat.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-240">The resource you are requesting authorization to use.</span></span> <span data-ttu-id="1d1a8-241">Příklad: `https://management.core.windows.net/`</span><span class="sxs-lookup"><span data-stu-id="1d1a8-241">For example: `https://management.core.windows.net/`</span></span> |
| <span data-ttu-id="1d1a8-242">Klient ID *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-242">Client ID*</span></span> |<span data-ttu-id="1d1a8-243">clientId</span><span class="sxs-lookup"><span data-stu-id="1d1a8-243">clientId</span></span> |<span data-ttu-id="1d1a8-244">Identifikátor klienta pro aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="1d1a8-244">The client identifier for the Azure AD application</span></span> |
| <span data-ttu-id="1d1a8-245">Tajný klíč *</span><span class="sxs-lookup"><span data-stu-id="1d1a8-245">Secret*</span></span> |<span data-ttu-id="1d1a8-246">tajný klíč</span><span class="sxs-lookup"><span data-stu-id="1d1a8-246">secret</span></span> |<span data-ttu-id="1d1a8-247">Tajný klíč klienta, který požaduje tokenu</span><span class="sxs-lookup"><span data-stu-id="1d1a8-247">The secret of the client that is requesting the token</span></span> |

> [!TIP]
> <span data-ttu-id="1d1a8-248">Můžete použít `securestring` parametr a `@parameters()` [funkce definice pracovního postupu](http://aka.ms/logicappdocs) použít parametr, který nebude možné číst v definici po uložení.</span><span class="sxs-lookup"><span data-stu-id="1d1a8-248">You can use a `securestring` parameter and the `@parameters()` [workflow definition function](http://aka.ms/logicappdocs) to use a parameter that won't be readable in the definition after saving.</span></span>
> 
> 

<span data-ttu-id="1d1a8-249">Například:</span><span class="sxs-lookup"><span data-stu-id="1d1a8-249">For example:</span></span>

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a><span data-ttu-id="1d1a8-250">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d1a8-250">Next steps</span></span>
<span data-ttu-id="1d1a8-251">Teď vyzkoušet platformu a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1d1a8-251">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="1d1a8-252">Ostatní konektory k dispozici v Logic Apps můžete prozkoumat pohledem na našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="1d1a8-252">You can explore the other available connectors in Logic Apps by looking at our [APIs list](apis-list.md).</span></span>

