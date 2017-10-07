---
title: "aaaCommunicate s žádný koncový bod přes protokol HTTP - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 9793601839437a2b880bdb81e15881270cacc963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http-action"></a><span data-ttu-id="9cc21-103">Začínáme s hello akce HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-103">Get started with hello HTTP action</span></span>

<span data-ttu-id="9cc21-104">S hello akce HTTP můžete rozšířit pracovní postupy pro vaši organizaci a koncový bod tooany komunikaci pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9cc21-104">With hello HTTP action, you can extend workflows for your organization and communicate tooany endpoint over HTTP.</span></span>

<span data-ttu-id="9cc21-105">Můžete:</span><span class="sxs-lookup"><span data-stu-id="9cc21-105">You can:</span></span>

* <span data-ttu-id="9cc21-106">Vytvořte logiku aplikace pracovní postupy, které aktivovat (aktivační události), kdy web, který spravujete přestane fungovat.</span><span class="sxs-lookup"><span data-stu-id="9cc21-106">Create logic app workflows that activate (trigger) when a website that you manage goes down.</span></span>
* <span data-ttu-id="9cc21-107">Komunikují koncový bod tooany přes HTTP tooextend vaše pracovní postupy k dalším službám.</span><span class="sxs-lookup"><span data-stu-id="9cc21-107">Communicate tooany endpoint over HTTP tooextend your workflows into other services.</span></span>

<span data-ttu-id="9cc21-108">tooget spuštění pomocí hello HTTP akce v aplikaci logiky, aplikaci najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9cc21-108">tooget started using hello HTTP action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-http-trigger"></a><span data-ttu-id="9cc21-109">Použít aktivační událost hello HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-109">Use hello HTTP trigger</span></span>
<span data-ttu-id="9cc21-110">Aktivační událost je událost, která lze použít toostart hello workflow, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9cc21-110">A trigger is an event that can be used toostart hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="9cc21-111">[Další informace o aktivační události](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9cc21-111">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="9cc21-112">Zde je příklad posloupnosti jak aktivovat tooset až hello HTTP v hello návrhář aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="9cc21-112">Here’s an example sequence of how tooset up hello HTTP trigger in hello Logic App Designer.</span></span>

1. <span data-ttu-id="9cc21-113">Přidejte hello triggeru protokolu HTTP v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9cc21-113">Add hello HTTP trigger in your logic app.</span></span>
2. <span data-ttu-id="9cc21-114">Vyplňte hello parametry pro koncový bod hello HTTP, které chcete toopoll.</span><span class="sxs-lookup"><span data-stu-id="9cc21-114">Fill in hello parameters for hello HTTP endpoint that you want toopoll.</span></span>
3. <span data-ttu-id="9cc21-115">Změna intervalu opakování hello na tom, jak často má dotazovat.</span><span class="sxs-lookup"><span data-stu-id="9cc21-115">Modify hello recurrence interval on how frequently it should poll.</span></span>

   <span data-ttu-id="9cc21-116">aplikace logiky Hello teď aktivuje s obsahem, který je vrácen během každé kontroly.</span><span class="sxs-lookup"><span data-stu-id="9cc21-116">hello logic app now fires with any content that is returned during each check.</span></span>

   ![Trigger HTTP](./media/connectors-native-http/using-trigger.png)

### <a name="how-hello-http-trigger-works"></a><span data-ttu-id="9cc21-118">Jak funguje hello triggeru protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-118">How hello HTTP trigger works</span></span>

<span data-ttu-id="9cc21-119">Aktivace protokolu HTTP Hello odešle tooHTTP koncový bod volání na intervalu opakování.</span><span class="sxs-lookup"><span data-stu-id="9cc21-119">hello HTTP trigger sends a call tooHTTP endpoint on a recurring interval.</span></span> <span data-ttu-id="9cc21-120">Ve výchozím kódu odpovědi HTTP, která je nižší než 300 způsobí, že toorun logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="9cc21-120">By default, any HTTP response code that is lower than 300 causes a logic app toorun.</span></span> <span data-ttu-id="9cc21-121">toospecify zda by měly aktivovat aplikaci logiky hello, můžete upravit aplikaci logiky hello v zobrazení kódu a přidejte podmínku, která vyhodnotí po hello volání protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9cc21-121">toospecify whether hello logic app should fire, you can edit hello logic app in code view, and add a condition that evaluates after hello HTTP call.</span></span> <span data-ttu-id="9cc21-122">Tady je příklad HTTP aktivační událost, která aktivuje se v případě hello vrátil kód stavu je větší než nebo roven hodnotě příliš`400`.</span><span class="sxs-lookup"><span data-stu-id="9cc21-122">Here's an example of an HTTP trigger that fires when hello returned status code is greater than or equal too`400`.</span></span>

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

<span data-ttu-id="9cc21-123">Úplné podrobnosti o parametrech hello HTTP aktivační události jsou k dispozici na [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).</span><span class="sxs-lookup"><span data-stu-id="9cc21-123">Full details about hello HTTP trigger parameters are available on [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).</span></span>

## <a name="use-hello-http-action"></a><span data-ttu-id="9cc21-124">Pomocí akce hello HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-124">Use hello HTTP action</span></span>

<span data-ttu-id="9cc21-125">Akce je operace, která se provádí v pracovním postupu hello, která je definována v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9cc21-125">An action is an operation that is carried out by hello workflow that is defined in a logic app.</span></span> 
<span data-ttu-id="9cc21-126">[Další informace o akcích](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9cc21-126">[Learn more about actions](connectors-overview.md).</span></span>

1. <span data-ttu-id="9cc21-127">Zvolte **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="9cc21-127">Choose **New Step** > **Add an action**.</span></span>
3. <span data-ttu-id="9cc21-128">Hello akce vyhledávacího pole zadejte **http** toolist hello HTTP akce.</span><span class="sxs-lookup"><span data-stu-id="9cc21-128">In hello action search box, type **http** toolist hello HTTP actions.</span></span>
   
    ![Vyberte akci hello HTTP](./media/connectors-native-http/using-action-1.png)

4. <span data-ttu-id="9cc21-130">Přidejte všechny požadované parametry pro volání hello protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9cc21-130">Add any required parameters for hello HTTP call.</span></span>
   
    ![Dokončení hello akce HTTP](./media/connectors-native-http/using-action-2.png)

5. <span data-ttu-id="9cc21-132">Na hello návrháře nástrojů, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9cc21-132">On hello designer toolbar, click **Save**.</span></span> <span data-ttu-id="9cc21-133">Uložení a publikování (aktivovaný) na hello svou aplikaci logiky stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="9cc21-133">Your logic app is saved and published (activated) at hello same time.</span></span>

## <a name="http-trigger"></a><span data-ttu-id="9cc21-134">Trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-134">HTTP trigger</span></span>
<span data-ttu-id="9cc21-135">Tady jsou hello podrobnosti hello aktivační událost, která tento konektor podporuje.</span><span class="sxs-lookup"><span data-stu-id="9cc21-135">Here are hello details for hello trigger that this connector supports.</span></span> <span data-ttu-id="9cc21-136">konektor HTTP Hello má jedna aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="9cc21-136">hello HTTP connector has one trigger.</span></span>

| <span data-ttu-id="9cc21-137">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="9cc21-137">Trigger</span></span> | <span data-ttu-id="9cc21-138">Popis</span><span class="sxs-lookup"><span data-stu-id="9cc21-138">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9cc21-139">HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-139">HTTP</span></span> |<span data-ttu-id="9cc21-140">Provede volání protokolu HTTP a vrátí obsah odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="9cc21-140">Makes an HTTP call and returns hello response content.</span></span> |

## <a name="http-action"></a><span data-ttu-id="9cc21-141">Akce HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-141">HTTP action</span></span>
<span data-ttu-id="9cc21-142">Tady jsou hello podrobnosti hello akce, který podporuje tento konektor.</span><span class="sxs-lookup"><span data-stu-id="9cc21-142">Here are hello details for hello action that this connector supports.</span></span> <span data-ttu-id="9cc21-143">konektor HTTP Hello má jednu možné akci.</span><span class="sxs-lookup"><span data-stu-id="9cc21-143">hello HTTP connector has one possible action.</span></span>

| <span data-ttu-id="9cc21-144">Akce</span><span class="sxs-lookup"><span data-stu-id="9cc21-144">Action</span></span> | <span data-ttu-id="9cc21-145">Popis</span><span class="sxs-lookup"><span data-stu-id="9cc21-145">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9cc21-146">HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-146">HTTP</span></span> |<span data-ttu-id="9cc21-147">Provede volání protokolu HTTP a vrátí obsah odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="9cc21-147">Makes an HTTP call and returns hello response content.</span></span> |

## <a name="http-details"></a><span data-ttu-id="9cc21-148">Podrobnosti protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-148">HTTP details</span></span>
<span data-ttu-id="9cc21-149">Hello následující tabulky popisují hello požadované a volitelné vstupních polí pro akce hello a hello odpovídající výstup podrobností, které jsou spojené s použitím akce hello.</span><span class="sxs-lookup"><span data-stu-id="9cc21-149">hello following tables describe hello required and optional input fields for hello action and hello corresponding output details that are associated with using hello action.</span></span>

#### <a name="http-request"></a><span data-ttu-id="9cc21-150">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-150">HTTP request</span></span>
<span data-ttu-id="9cc21-151">Hello následují vstupní pole pro hello akce, která umožňuje odchozí požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="9cc21-151">hello following are input fields for hello action, which makes an HTTP outbound request.</span></span>
<span data-ttu-id="9cc21-152">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="9cc21-152">A * means that it is a required field.</span></span>

| <span data-ttu-id="9cc21-153">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="9cc21-153">Display name</span></span> | <span data-ttu-id="9cc21-154">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9cc21-154">Property name</span></span> | <span data-ttu-id="9cc21-155">Popis</span><span class="sxs-lookup"><span data-stu-id="9cc21-155">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9cc21-156">Metoda *</span><span class="sxs-lookup"><span data-stu-id="9cc21-156">Method*</span></span> |<span data-ttu-id="9cc21-157">– Metoda</span><span class="sxs-lookup"><span data-stu-id="9cc21-157">method</span></span> |<span data-ttu-id="9cc21-158">příkaz toouse Hello HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-158">hello HTTP verb toouse</span></span> |
| <span data-ttu-id="9cc21-159">IDENTIFIKÁTOR URI *</span><span class="sxs-lookup"><span data-stu-id="9cc21-159">URI*</span></span> |<span data-ttu-id="9cc21-160">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="9cc21-160">uri</span></span> |<span data-ttu-id="9cc21-161">Hello identifikátor URI pro požadavek hello HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-161">hello URI for hello HTTP request</span></span> |
| <span data-ttu-id="9cc21-162">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="9cc21-162">Headers</span></span> |<span data-ttu-id="9cc21-163">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="9cc21-163">headers</span></span> |<span data-ttu-id="9cc21-164">Objekt JSON tooinclude hlavičky protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-164">A JSON object of HTTP headers tooinclude</span></span> |
| <span data-ttu-id="9cc21-165">Tělo</span><span class="sxs-lookup"><span data-stu-id="9cc21-165">Body</span></span> |<span data-ttu-id="9cc21-166">Text</span><span class="sxs-lookup"><span data-stu-id="9cc21-166">body</span></span> |<span data-ttu-id="9cc21-167">Hello požadavku HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-167">hello HTTP request body</span></span> |
| <span data-ttu-id="9cc21-168">Authentication</span><span class="sxs-lookup"><span data-stu-id="9cc21-168">Authentication</span></span> |<span data-ttu-id="9cc21-169">Ověřování</span><span class="sxs-lookup"><span data-stu-id="9cc21-169">authentication</span></span> |<span data-ttu-id="9cc21-170">Podrobnosti v hello [ověřování](#authentication) části</span><span class="sxs-lookup"><span data-stu-id="9cc21-170">Details in hello [Authentication](#authentication) section</span></span> |

<br>

#### <a name="output-details"></a><span data-ttu-id="9cc21-171">Podrobnosti o výstupu</span><span class="sxs-lookup"><span data-stu-id="9cc21-171">Output details</span></span>
<span data-ttu-id="9cc21-172">Hello následují výstup podrobnosti hello odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="9cc21-172">hello following are output details for hello HTTP response.</span></span>

| <span data-ttu-id="9cc21-173">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9cc21-173">Property name</span></span> | <span data-ttu-id="9cc21-174">Datový typ</span><span class="sxs-lookup"><span data-stu-id="9cc21-174">Data type</span></span> | <span data-ttu-id="9cc21-175">Popis</span><span class="sxs-lookup"><span data-stu-id="9cc21-175">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9cc21-176">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="9cc21-176">Headers</span></span> |<span data-ttu-id="9cc21-177">Objekt</span><span class="sxs-lookup"><span data-stu-id="9cc21-177">object</span></span> |<span data-ttu-id="9cc21-178">Hlavičky odpovědi</span><span class="sxs-lookup"><span data-stu-id="9cc21-178">Response headers</span></span> |
| <span data-ttu-id="9cc21-179">Tělo</span><span class="sxs-lookup"><span data-stu-id="9cc21-179">Body</span></span> |<span data-ttu-id="9cc21-180">Objekt</span><span class="sxs-lookup"><span data-stu-id="9cc21-180">object</span></span> |<span data-ttu-id="9cc21-181">Objekt odpovědi</span><span class="sxs-lookup"><span data-stu-id="9cc21-181">Response object</span></span> |
| <span data-ttu-id="9cc21-182">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="9cc21-182">Status Code</span></span> |<span data-ttu-id="9cc21-183">celá čísla</span><span class="sxs-lookup"><span data-stu-id="9cc21-183">int</span></span> |<span data-ttu-id="9cc21-184">Stavový kód protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="9cc21-184">HTTP status code</span></span> |

## <a name="authentication"></a><span data-ttu-id="9cc21-185">Authentication</span><span class="sxs-lookup"><span data-stu-id="9cc21-185">Authentication</span></span>
<span data-ttu-id="9cc21-186">Funkce Logic Apps Hello umožňuje toouse různé typy ověřování proti koncových bodů protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9cc21-186">hello Logic Apps feature allows you toouse different types of authentication against HTTP endpoints.</span></span> <span data-ttu-id="9cc21-187">Toto ověřování můžete použít s hello **HTTP**,  **[HTTP + Swagger](connectors-native-http-swagger.md)**, a  **[Webhooku protokolu HTTP](connectors-native-webhook.md)**  konektory.</span><span class="sxs-lookup"><span data-stu-id="9cc21-187">You can use this authentication with hello **HTTP**, **[HTTP + Swagger](connectors-native-http-swagger.md)**, and **[HTTP Webhook](connectors-native-webhook.md)** connectors.</span></span> <span data-ttu-id="9cc21-188">Hello následující typy ověřování se dají konfigurovat:</span><span class="sxs-lookup"><span data-stu-id="9cc21-188">hello following types of authentication are configurable:</span></span>

* [<span data-ttu-id="9cc21-189">Základní ověřování</span><span class="sxs-lookup"><span data-stu-id="9cc21-189">Basic authentication</span></span>](#basic-authentication)
* [<span data-ttu-id="9cc21-190">Ověřování certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="9cc21-190">Client certificate authentication</span></span>](#client-certificate-authentication)
* [<span data-ttu-id="9cc21-191">Ověřování Azure Active Directory (Azure AD) OAuth</span><span class="sxs-lookup"><span data-stu-id="9cc21-191">Azure Active Directory (Azure AD) OAuth authentication</span></span>](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a><span data-ttu-id="9cc21-192">Základní ověřování</span><span class="sxs-lookup"><span data-stu-id="9cc21-192">Basic authentication</span></span>

<span data-ttu-id="9cc21-193">Hello následující ověřování objektu je potřeba pro základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="9cc21-193">hello following authentication object is needed for basic authentication.</span></span>
<span data-ttu-id="9cc21-194">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="9cc21-194">A * means that it is a required field.</span></span>

| <span data-ttu-id="9cc21-195">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9cc21-195">Property name</span></span> | <span data-ttu-id="9cc21-196">Datový typ</span><span class="sxs-lookup"><span data-stu-id="9cc21-196">Data type</span></span> | <span data-ttu-id="9cc21-197">Popis</span><span class="sxs-lookup"><span data-stu-id="9cc21-197">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9cc21-198">Typ *</span><span class="sxs-lookup"><span data-stu-id="9cc21-198">Type*</span></span> |<span data-ttu-id="9cc21-199">type</span><span class="sxs-lookup"><span data-stu-id="9cc21-199">type</span></span> |<span data-ttu-id="9cc21-200">Typ ověřování (musí být `Basic` pro základní ověřování)</span><span class="sxs-lookup"><span data-stu-id="9cc21-200">Type of authentication (must be `Basic` for basic authentication)</span></span> |
| <span data-ttu-id="9cc21-201">Uživatelské jméno *</span><span class="sxs-lookup"><span data-stu-id="9cc21-201">Username*</span></span> |<span data-ttu-id="9cc21-202">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="9cc21-202">username</span></span> |<span data-ttu-id="9cc21-203">Název tooauthenticate uživatele</span><span class="sxs-lookup"><span data-stu-id="9cc21-203">User name tooauthenticate</span></span> |
| <span data-ttu-id="9cc21-204">Heslo *</span><span class="sxs-lookup"><span data-stu-id="9cc21-204">Password*</span></span> |<span data-ttu-id="9cc21-205">heslo</span><span class="sxs-lookup"><span data-stu-id="9cc21-205">password</span></span> |<span data-ttu-id="9cc21-206">Tooauthenticate heslo</span><span class="sxs-lookup"><span data-stu-id="9cc21-206">Password tooauthenticate</span></span> |

> [!TIP]
> <span data-ttu-id="9cc21-207">Pokud chcete toouse heslo, které nelze načíst z definice hello, použijte `securestring` parametr a hello `@parameters()`  
>  [funkce definice pracovního postupu](http://aka.ms/logicappdocs).</span><span class="sxs-lookup"><span data-stu-id="9cc21-207">If you want toouse a password that cannot be retrieved from hello definition, use a `securestring` parameter and hello `@parameters()` 
> [workflow definition function](http://aka.ms/logicappdocs).</span></span>

<span data-ttu-id="9cc21-208">Například:</span><span class="sxs-lookup"><span data-stu-id="9cc21-208">For example:</span></span>

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a><span data-ttu-id="9cc21-209">Ověřování klientských certifikátů</span><span class="sxs-lookup"><span data-stu-id="9cc21-209">Client certificate authentication</span></span>

<span data-ttu-id="9cc21-210">Hello následující ověřování objektu je potřeba pro ověřování pomocí certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="9cc21-210">hello following authentication object is needed for client certificate authentication.</span></span> <span data-ttu-id="9cc21-211">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="9cc21-211">A * means that it is a required field.</span></span>

| <span data-ttu-id="9cc21-212">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9cc21-212">Property name</span></span> | <span data-ttu-id="9cc21-213">Datový typ</span><span class="sxs-lookup"><span data-stu-id="9cc21-213">Data type</span></span> | <span data-ttu-id="9cc21-214">Popis</span><span class="sxs-lookup"><span data-stu-id="9cc21-214">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9cc21-215">Typ *</span><span class="sxs-lookup"><span data-stu-id="9cc21-215">Type*</span></span> |<span data-ttu-id="9cc21-216">type</span><span class="sxs-lookup"><span data-stu-id="9cc21-216">type</span></span> |<span data-ttu-id="9cc21-217">typ ověřování Hello (musí být `ClientCertificate` pro klientské certifikáty SSL)</span><span class="sxs-lookup"><span data-stu-id="9cc21-217">hello type of authentication (must be `ClientCertificate` for SSL client certificates)</span></span> |
| <span data-ttu-id="9cc21-218">SOUBOR PFX *</span><span class="sxs-lookup"><span data-stu-id="9cc21-218">PFX*</span></span> |<span data-ttu-id="9cc21-219">Soubor PFX</span><span class="sxs-lookup"><span data-stu-id="9cc21-219">pfx</span></span> |<span data-ttu-id="9cc21-220">Hello kódováním Base64 obsah souboru hello Personal Information Exchange (PFX)</span><span class="sxs-lookup"><span data-stu-id="9cc21-220">hello Base64-encoded contents of hello Personal Information Exchange (PFX) file</span></span> |
| <span data-ttu-id="9cc21-221">Heslo *</span><span class="sxs-lookup"><span data-stu-id="9cc21-221">Password*</span></span> |<span data-ttu-id="9cc21-222">heslo</span><span class="sxs-lookup"><span data-stu-id="9cc21-222">password</span></span> |<span data-ttu-id="9cc21-223">Hello heslo tooaccess hello souboru PFX</span><span class="sxs-lookup"><span data-stu-id="9cc21-223">hello password tooaccess hello PFX file</span></span> |

> [!TIP]
> <span data-ttu-id="9cc21-224">toouse parametr, který nebude možné číst v definici hello po uložení hello aplikace logiky, můžete použít `securestring` parametr a hello `@parameters()`  
>  [funkce definice pracovního postupu](http://aka.ms/logicappdocs).</span><span class="sxs-lookup"><span data-stu-id="9cc21-224">toouse a parameter that won't be readable in hello definition after saving hello logic app, you can use a `securestring` parameter and hello `@parameters()` 
> [workflow definition function](http://aka.ms/logicappdocs).</span></span>

<span data-ttu-id="9cc21-225">Například:</span><span class="sxs-lookup"><span data-stu-id="9cc21-225">For example:</span></span>

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a><span data-ttu-id="9cc21-226">Ověřování služby Azure AD OAuth</span><span class="sxs-lookup"><span data-stu-id="9cc21-226">Azure AD OAuth authentication</span></span>
<span data-ttu-id="9cc21-227">Hello následující ověřování objektu je potřeba pro ověřování Azure AD OAuth.</span><span class="sxs-lookup"><span data-stu-id="9cc21-227">hello following authentication object is needed for Azure AD OAuth authentication.</span></span> <span data-ttu-id="9cc21-228">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="9cc21-228">A * means that it is a required field.</span></span>

| <span data-ttu-id="9cc21-229">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9cc21-229">Property name</span></span> | <span data-ttu-id="9cc21-230">Datový typ</span><span class="sxs-lookup"><span data-stu-id="9cc21-230">Data type</span></span> | <span data-ttu-id="9cc21-231">Popis</span><span class="sxs-lookup"><span data-stu-id="9cc21-231">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9cc21-232">Typ *</span><span class="sxs-lookup"><span data-stu-id="9cc21-232">Type*</span></span> |<span data-ttu-id="9cc21-233">type</span><span class="sxs-lookup"><span data-stu-id="9cc21-233">type</span></span> |<span data-ttu-id="9cc21-234">typ ověřování Hello (musí být `ActiveDirectoryOAuth` pro Azure AD OAuth)</span><span class="sxs-lookup"><span data-stu-id="9cc21-234">hello type of authentication (must be `ActiveDirectoryOAuth` for Azure AD OAuth)</span></span> |
| <span data-ttu-id="9cc21-235">Klienta *</span><span class="sxs-lookup"><span data-stu-id="9cc21-235">Tenant*</span></span> |<span data-ttu-id="9cc21-236">Klienta</span><span class="sxs-lookup"><span data-stu-id="9cc21-236">tenant</span></span> |<span data-ttu-id="9cc21-237">identifikátor Hello klienta pro klienta hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9cc21-237">hello tenant identifier for hello Azure AD tenant</span></span> |
| <span data-ttu-id="9cc21-238">Cílová skupina *</span><span class="sxs-lookup"><span data-stu-id="9cc21-238">Audience*</span></span> |<span data-ttu-id="9cc21-239">Cílová skupina</span><span class="sxs-lookup"><span data-stu-id="9cc21-239">audience</span></span> |<span data-ttu-id="9cc21-240">prostředek Hello jste požádali toouse autorizace.</span><span class="sxs-lookup"><span data-stu-id="9cc21-240">hello resource you are requesting authorization toouse.</span></span> <span data-ttu-id="9cc21-241">Příklad: `https://management.core.windows.net/`</span><span class="sxs-lookup"><span data-stu-id="9cc21-241">For example: `https://management.core.windows.net/`</span></span> |
| <span data-ttu-id="9cc21-242">Klient ID *</span><span class="sxs-lookup"><span data-stu-id="9cc21-242">Client ID*</span></span> |<span data-ttu-id="9cc21-243">clientId</span><span class="sxs-lookup"><span data-stu-id="9cc21-243">clientId</span></span> |<span data-ttu-id="9cc21-244">Hello identifikátor klienta pro aplikaci hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9cc21-244">hello client identifier for hello Azure AD application</span></span> |
| <span data-ttu-id="9cc21-245">Tajný klíč *</span><span class="sxs-lookup"><span data-stu-id="9cc21-245">Secret*</span></span> |<span data-ttu-id="9cc21-246">tajný klíč</span><span class="sxs-lookup"><span data-stu-id="9cc21-246">secret</span></span> |<span data-ttu-id="9cc21-247">tajný klíč Hello hello klienta, který žádá o hello token</span><span class="sxs-lookup"><span data-stu-id="9cc21-247">hello secret of hello client that is requesting hello token</span></span> |

> [!TIP]
> <span data-ttu-id="9cc21-248">Můžete použít `securestring` parametr a hello `@parameters()` [funkce definice pracovního postupu](http://aka.ms/logicappdocs) toouse parametr, který nebude možné číst v definici hello po uložení.</span><span class="sxs-lookup"><span data-stu-id="9cc21-248">You can use a `securestring` parameter and hello `@parameters()` [workflow definition function](http://aka.ms/logicappdocs) toouse a parameter that won't be readable in hello definition after saving.</span></span>
> 
> 

<span data-ttu-id="9cc21-249">Například:</span><span class="sxs-lookup"><span data-stu-id="9cc21-249">For example:</span></span>

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a><span data-ttu-id="9cc21-250">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9cc21-250">Next steps</span></span>
<span data-ttu-id="9cc21-251">Teď vyzkoušet hello platformy a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9cc21-251">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="9cc21-252">Můžete prozkoumat hello dalších dostupných konektorů v Logic Apps pohledem na našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="9cc21-252">You can explore hello other available connectors in Logic Apps by looking at our [APIs list](apis-list.md).</span></span>

