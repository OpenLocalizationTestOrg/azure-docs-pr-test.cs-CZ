---
title: Azure vazby HTTP funkce a webhooku | Microsoft Docs
description: "Pochopit, jak používat protokol HTTP a webhooku triggerů a vazeb v Azure Functions."
services: functions
documentationcenter: na
author: mattchenderson
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce, událostí zpracování, webhooků, dynamické výpočetní, bez serveru Architektura protokolu HTTP, API REST"
ms.assetid: 2b12200d-63d8-4ec1-9da8-39831d5a51b1
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/18/2016
ms.author: mahender
ms.openlocfilehash: 71c0d22c4b1824078982b9d1cc76645f947ae603
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-functions-http-and-webhook-bindings"></a><span data-ttu-id="8c098-104">Azure funkce protokolu HTTP a webhooku vazby</span><span class="sxs-lookup"><span data-stu-id="8c098-104">Azure Functions HTTP and webhook bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="8c098-105">Tento článek vysvětluje postup konfigurace a práce s HTTP triggerů a vazeb v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8c098-105">This article explains how to configure and work with HTTP triggers and bindings in Azure Functions.</span></span>
<span data-ttu-id="8c098-106">Pomocí těchto můžete Azure Functions sestavení bez serveru rozhraní API a reagovat na webhooky.</span><span class="sxs-lookup"><span data-stu-id="8c098-106">With these, you can use Azure Functions to build serverless APIs and respond to webhooks.</span></span>

<span data-ttu-id="8c098-107">Azure Functions nabízí následující vazby:</span><span class="sxs-lookup"><span data-stu-id="8c098-107">Azure Functions provides the following bindings:</span></span>
- <span data-ttu-id="8c098-108">[Triggeru protokolu HTTP](#httptrigger) umožňuje vyvolají funkci s žádostí HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c098-108">An [HTTP trigger](#httptrigger) lets you invoke a function with an HTTP request.</span></span> <span data-ttu-id="8c098-109">To lze přizpůsobit reagovat na [webhooky](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="8c098-109">This can be customized to respond to [webhooks](#hooktrigger).</span></span>
- <span data-ttu-id="8c098-110">[HTTP výstup vazby](#output) umožňuje odpovědět na požadavek.</span><span class="sxs-lookup"><span data-stu-id="8c098-110">An [HTTP output binding](#output) allows you to respond to the request.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a><span data-ttu-id="8c098-111">Trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="8c098-111">HTTP trigger</span></span>
<span data-ttu-id="8c098-112">Aktivace protokolu HTTP bude vykonán funkce v odpovědi na požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c098-112">The HTTP trigger will execute your function in response to an HTTP request.</span></span> <span data-ttu-id="8c098-113">Můžete přizpůsobit ho reagovat na konkrétní adresu URL nebo sady metod HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c098-113">You can customize it to respond to a particular URL or set of HTTP methods.</span></span> <span data-ttu-id="8c098-114">Aktivační událost INSTEAD HTTP lze také nakonfigurovat reagovat na webhooky.</span><span class="sxs-lookup"><span data-stu-id="8c098-114">An HTTP trigger can also be configured to respond to webhooks.</span></span> 

<span data-ttu-id="8c098-115">Pokud používáte portál funkce, můžete také začít používat hned použití předem vytvořené šablony.</span><span class="sxs-lookup"><span data-stu-id="8c098-115">If using the Functions portal, you can also get started right away using a pre-made template.</span></span> <span data-ttu-id="8c098-116">Vyberte **novou funkci** a zvolte "Rozhraní API a Webhooky" z **scénář** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="8c098-116">Select **New function** and choose "API & Webhooks" from the **Scenario** dropdown.</span></span> <span data-ttu-id="8c098-117">Vyberte jednu z šablon a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8c098-117">Select one of the templates and click **Create**.</span></span>

<span data-ttu-id="8c098-118">Ve výchozím nastavení bude aktivační procedury HTTP odpovědět na požadavek s kódem stavu HTTP 200 OK a prázdným textem zprávy.</span><span class="sxs-lookup"><span data-stu-id="8c098-118">By default, an HTTP trigger will respond to the request with an HTTP 200 OK status code and an empty body.</span></span> <span data-ttu-id="8c098-119">Chcete-li upravit odpověď, nakonfigurovat [HTTP výstup vazby](#output)</span><span class="sxs-lookup"><span data-stu-id="8c098-119">To modify the response, configure an [HTTP output binding](#output)</span></span>

### <a name="configuring-an-http-trigger"></a><span data-ttu-id="8c098-120">Konfigurace aktivace protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="8c098-120">Configuring an HTTP trigger</span></span>
<span data-ttu-id="8c098-121">Aktivační událost INSTEAD HTTP je definována včetně podobný následujícímu v objektu JSON `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="8c098-121">An HTTP trigger is defined by including a JSON object similar to the following in the `bindings` array of function.json:</span></span>

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function",
    "methods": [ "get" ],
    "route": "values/{id}"
},
```
<span data-ttu-id="8c098-122">Vazba podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="8c098-122">The binding supports the following properties:</span></span>

* <span data-ttu-id="8c098-123">**název** : požadované – název proměnné používá v kódu funkce pro požadavek nebo textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="8c098-123">**name** : Required - the variable name used in function code for the request or request body.</span></span> <span data-ttu-id="8c098-124">V tématu [práce s aktivační procedury HTTP z kódu](#httptriggerusage).</span><span class="sxs-lookup"><span data-stu-id="8c098-124">See [Working with an HTTP trigger from code](#httptriggerusage).</span></span>
* <span data-ttu-id="8c098-125">**typ** : požadované – musí být nastavena na "httpTrigger".</span><span class="sxs-lookup"><span data-stu-id="8c098-125">**type** : Required - must be set to "httpTrigger".</span></span>
* <span data-ttu-id="8c098-126">**směr** : požadované – musí být nastavena na "v".</span><span class="sxs-lookup"><span data-stu-id="8c098-126">**direction** : Required - must be set to "in".</span></span>
* <span data-ttu-id="8c098-127">_authLevel_ : Určuje, co klíče, pokud existuje, musí být přítomen v požadavku k vyvolání funkce.</span><span class="sxs-lookup"><span data-stu-id="8c098-127">_authLevel_ : This determines what keys, if any, need to be present on the request in order to invoke the function.</span></span> <span data-ttu-id="8c098-128">V tématu [práci s klíči](#keys) níže.</span><span class="sxs-lookup"><span data-stu-id="8c098-128">See [Working with keys](#keys) below.</span></span> <span data-ttu-id="8c098-129">Hodnota může být jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="8c098-129">The value can be one of the following:</span></span>
    * <span data-ttu-id="8c098-130">_Anonymní_: je požadován klíč rozhraní API ne.</span><span class="sxs-lookup"><span data-stu-id="8c098-130">_anonymous_: No API key is required.</span></span>
    * <span data-ttu-id="8c098-131">_funkce_: je požadován klíč rozhraní API specifických funkcí.</span><span class="sxs-lookup"><span data-stu-id="8c098-131">_function_: A function-specific API key is required.</span></span> <span data-ttu-id="8c098-132">Toto je výchozí hodnota, pokud žádný je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8c098-132">This is the default value if none is provided.</span></span>
    * <span data-ttu-id="8c098-133">_správce_ : je nezbytný hlavní klíč.</span><span class="sxs-lookup"><span data-stu-id="8c098-133">_admin_ : The master key is required.</span></span>
* <span data-ttu-id="8c098-134">**metody** : Toto je pole metod HTTP, na které bude odpovídat funkce.</span><span class="sxs-lookup"><span data-stu-id="8c098-134">**methods** : This is an array of the HTTP methods to which the function will respond.</span></span> <span data-ttu-id="8c098-135">Pokud není zadáno, bude funkce odpovídat na všechny metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c098-135">If not specified, the function will respond to all HTTP methods.</span></span> <span data-ttu-id="8c098-136">V tématu [přizpůsobení koncový bod HTTP](#url).</span><span class="sxs-lookup"><span data-stu-id="8c098-136">See [Customizing the HTTP endpoint](#url).</span></span>
* <span data-ttu-id="8c098-137">**trasy** : definuje šablonu trasy, řízení, ke kterému žádosti adresy URL bude odpovídat funkce.</span><span class="sxs-lookup"><span data-stu-id="8c098-137">**route** : This defines the route template, controlling to which request URLs your function will respond.</span></span> <span data-ttu-id="8c098-138">Výchozí hodnota, pokud je zadaný žádný je `<functionname>`.</span><span class="sxs-lookup"><span data-stu-id="8c098-138">The default value if none is provided is `<functionname>`.</span></span> <span data-ttu-id="8c098-139">V tématu [přizpůsobení koncový bod HTTP](#url).</span><span class="sxs-lookup"><span data-stu-id="8c098-139">See [Customizing the HTTP endpoint](#url).</span></span>
* <span data-ttu-id="8c098-140">**webHookType** : tím se nakonfiguruje tak, aby fungoval jako reciever webhooku pro zadaného zprostředkovatele triggeru protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c098-140">**webHookType** : This configures the HTTP trigger to act as a webhook reciever for the specified provider.</span></span> <span data-ttu-id="8c098-141">_Metody_ vlastnost neměla by být nastavená, pokud to jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="8c098-141">The _methods_ property should not be set if this is chosen.</span></span> <span data-ttu-id="8c098-142">V tématu [neodpovídá na požadavky webhooky](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="8c098-142">See [Responding to webhooks](#hooktrigger).</span></span> <span data-ttu-id="8c098-143">Hodnota může být jeden z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="8c098-143">The value can be one of the following:</span></span>
    * <span data-ttu-id="8c098-144">_genericJson_ : webhooku koncový bod obecné účely bez logiku pro konkrétního zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="8c098-144">_genericJson_ : A general purpose webhook endpoint without logic for a specific provider.</span></span>
    * <span data-ttu-id="8c098-145">_github_ : funkce bude reagovat na Githubu webhooky.</span><span class="sxs-lookup"><span data-stu-id="8c098-145">_github_ : The function will respond to GitHub webhooks.</span></span> <span data-ttu-id="8c098-146">_AuthLevel_ vlastnost neměla by být nastavená, pokud to jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="8c098-146">The _authLevel_ property should not be set if this is chosen.</span></span>
    * <span data-ttu-id="8c098-147">_slack_ : funkce bude odpovídat Slack webhooky.</span><span class="sxs-lookup"><span data-stu-id="8c098-147">_slack_ : The function will respond to Slack webhooks.</span></span> <span data-ttu-id="8c098-148">_AuthLevel_ vlastnost neměla by být nastavená, pokud to jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="8c098-148">The _authLevel_ property should not be set if this is chosen.</span></span>

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a><span data-ttu-id="8c098-149">Práce s aktivační procedury HTTP z kódu</span><span class="sxs-lookup"><span data-stu-id="8c098-149">Working with an HTTP trigger from code</span></span>
<span data-ttu-id="8c098-150">Pro funkce C# a F #, můžou deklarovat typ aktivační událost vstupu být buď `HttpRequestMessage` nebo vlastního typu.</span><span class="sxs-lookup"><span data-stu-id="8c098-150">For C# and F# functions, you can declare the type of your trigger input to be either `HttpRequestMessage` or a custom type.</span></span> <span data-ttu-id="8c098-151">Pokud se rozhodnete `HttpRequestMessage`, pak budete mít plný přístup k objektu žádosti.</span><span class="sxs-lookup"><span data-stu-id="8c098-151">If you choose `HttpRequestMessage`, then you will get full access to the request object.</span></span> <span data-ttu-id="8c098-152">Pro vlastní typ (například POCO) se pokusí funkce analyzovat datovou část požadavku jako JSON k naplnění vlastnosti objektu.</span><span class="sxs-lookup"><span data-stu-id="8c098-152">For a custom type (such as a POCO), Functions will attempt to parse the request body as JSON to populate the object properties.</span></span>

<span data-ttu-id="8c098-153">Pro funkce Node.js poskytuje Functions runtime textu žádosti namísto objektu žádosti.</span><span class="sxs-lookup"><span data-stu-id="8c098-153">For Node.js functions, the Functions runtime provides the request body instead of the request object.</span></span>

<span data-ttu-id="8c098-154">V tématu [HTTP aktivační událost ukázky](#httptriggersample) například použití.</span><span class="sxs-lookup"><span data-stu-id="8c098-154">See [HTTP trigger samples](#httptriggersample) for example usages.</span></span>


<a name="output"></a>
## <a name="http-response-output-binding"></a><span data-ttu-id="8c098-155">Odpověď HTTP výstup vazby</span><span class="sxs-lookup"><span data-stu-id="8c098-155">HTTP response output binding</span></span>
<span data-ttu-id="8c098-156">Použijte protokol HTTP výstup vazby reagovat na odesílatel požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c098-156">Use the HTTP output binding to respond to the HTTP request sender.</span></span> <span data-ttu-id="8c098-157">Tato vazba vyžaduje aktivační procedury protokolu HTTP a umožňuje přizpůsobit odpověď přidružená k požadavku aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="8c098-157">This binding requires an HTTP trigger and allows you to customize the response associated with the trigger's request.</span></span> <span data-ttu-id="8c098-158">Pokud výstup vazba HTTP není zadáno, aktivační procedury HTTP vrátí s prázdným textem zprávy HTTP 200 OK.</span><span class="sxs-lookup"><span data-stu-id="8c098-158">If an HTTP output binding is not provided, an HTTP trigger will return HTTP 200 OK with an empty body.</span></span> 

### <a name="configuring-an-http-output-binding"></a><span data-ttu-id="8c098-159">Konfigurace HTTP výstup vazby</span><span class="sxs-lookup"><span data-stu-id="8c098-159">Configuring an HTTP output binding</span></span>
<span data-ttu-id="8c098-160">HTTP výstup vazba je definována včetně podobný následujícímu v objektu JSON `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="8c098-160">The HTTP output binding is defined by including a JSON object similar to the following in the `bindings` array of function.json:</span></span>

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
<span data-ttu-id="8c098-161">Vazba obsahuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="8c098-161">The binding contains the following properties:</span></span>

* <span data-ttu-id="8c098-162">**název** : požadované – název proměnné, které jsou použity v kódu funkce pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="8c098-162">**name** : Required - the variable name used in function code for the response.</span></span> <span data-ttu-id="8c098-163">V tématu [práci s HTTP výstup vazba z kódu](#outputusage).</span><span class="sxs-lookup"><span data-stu-id="8c098-163">See [Working with an HTTP output binding from code](#outputusage).</span></span>
* <span data-ttu-id="8c098-164">**typ** : požadované – musí být nastavena na "http".</span><span class="sxs-lookup"><span data-stu-id="8c098-164">**type** : Required - must be set to "http".</span></span>
* <span data-ttu-id="8c098-165">**směr** : požadované – musí být nastavena na "out".</span><span class="sxs-lookup"><span data-stu-id="8c098-165">**direction** : Required - must be set to "out".</span></span>

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a><span data-ttu-id="8c098-166">Práce s HTTP výstup vazba z kódu</span><span class="sxs-lookup"><span data-stu-id="8c098-166">Working with an HTTP output binding from code</span></span>
<span data-ttu-id="8c098-167">Výstupní parametr (například "res") můžete reagovat na protokolu http nebo webhooku volajícího.</span><span class="sxs-lookup"><span data-stu-id="8c098-167">You can use the output parameter (e.g., "res") to respond to the http or webhook caller.</span></span> <span data-ttu-id="8c098-168">Alternativně můžete použít standardní `Request.CreateResponse()` (C#) nebo `context.res` (Node.JS) vzor vrátit do odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8c098-168">Alternatively, you can use the standard `Request.CreateResponse()` (C#) or `context.res` (Node.JS) pattern to return your response.</span></span> <span data-ttu-id="8c098-169">Příklady použití druhé metody naleznete v tématu [HTTP aktivační událost ukázky](#httptriggersample) a [ukázky aktivační události Webhooku](#hooktriggersample).</span><span class="sxs-lookup"><span data-stu-id="8c098-169">For examples on how to use the latter method, see [HTTP trigger samples](#httptriggersample) and [Webhook trigger samples](#hooktriggersample).</span></span>


<a name="hooktrigger"></a>
## <a name="responding-to-webhooks"></a><span data-ttu-id="8c098-170">Odpovídá k webhookům</span><span class="sxs-lookup"><span data-stu-id="8c098-170">Responding to webhooks</span></span>
<span data-ttu-id="8c098-171">Aktivační událost INSTEAD HTTP s _webHookType_ vlastnost nakonfiguruje reagovat na [webhooky](https://en.wikipedia.org/wiki/Webhook).</span><span class="sxs-lookup"><span data-stu-id="8c098-171">An HTTP trigger with the _webHookType_ property will be configured to respond to [webhooks](https://en.wikipedia.org/wiki/Webhook).</span></span> <span data-ttu-id="8c098-172">Základní konfigurace používá nastavení "genericJson".</span><span class="sxs-lookup"><span data-stu-id="8c098-172">The basic configuration uses the "genericJson" setting.</span></span> <span data-ttu-id="8c098-173">To omezuje požadavky pouze na ty pomocí protokolu HTTP POST a s `application/json` typ obsahu.</span><span class="sxs-lookup"><span data-stu-id="8c098-173">This restricts requests to only those using HTTP POST and with the `application/json` content type.</span></span>

<span data-ttu-id="8c098-174">Aktivační událost, můžete přizpůsobit kromě poskytovatele konkrétní webhooku (například [Githubu](https://developer.github.com/webhooks/) a [Slack](https://api.slack.com/outgoing-webhooks)).</span><span class="sxs-lookup"><span data-stu-id="8c098-174">The trigger can additionally be tailored to a specific webhook provider (e.g., [GitHub](https://developer.github.com/webhooks/) and [Slack](https://api.slack.com/outgoing-webhooks)).</span></span> <span data-ttu-id="8c098-175">Pokud je zadán poskytovatele, Functions runtime mohou starat o poskytovatele logiku ověření pro vás.</span><span class="sxs-lookup"><span data-stu-id="8c098-175">If a provider is specified, the Functions runtime can take care of the provider's validation logic for you.</span></span>  

### <a name="configuring-github-as-a-webhook-provider"></a><span data-ttu-id="8c098-176">Konfigurace jako zprostředkovatel webhook Githubu</span><span class="sxs-lookup"><span data-stu-id="8c098-176">Configuring GitHub as a webhook provider</span></span>
<span data-ttu-id="8c098-177">Chcete-li reagovat na Githubu webhooků, nejprve vytvoření funkce s aktivační procedury protokolu HTTP a nastavte _webHookType_ vlastnost "githubu".</span><span class="sxs-lookup"><span data-stu-id="8c098-177">To respond to GitHub webhooks, first create your function with an HTTP Trigger, and set the _webHookType_ property to "github".</span></span> <span data-ttu-id="8c098-178">Zkopírujte jeho [URL](#url) a [klíč rozhraní API](#keys) do úložiště GitHub **přidat webhooku** stránky.</span><span class="sxs-lookup"><span data-stu-id="8c098-178">Then copy its [URL](#url) and [API key](#keys) into your GitHub repository's **Add webhook** page.</span></span> <span data-ttu-id="8c098-179">Najdete v článku na Githubu [vytváření Webhooky](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) dokumentaci další informace.</span><span class="sxs-lookup"><span data-stu-id="8c098-179">See GitHub's [Creating Webhooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) documentation for more.</span></span>

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="configuring-slack-as-a-webhook-provider"></a><span data-ttu-id="8c098-180">Konfigurace systému Slack jako zprostředkovatel webhooku</span><span class="sxs-lookup"><span data-stu-id="8c098-180">Configuring Slack as a webhook provider</span></span>
<span data-ttu-id="8c098-181">Slack webhooku generuje token pro vás místo umožňují určit, takže je nutné nakonfigurovat specifické funkce klíč pomocí tokenu z Slack.</span><span class="sxs-lookup"><span data-stu-id="8c098-181">The Slack webhook generates a token for you instead of letting you specify it, so you must configure a function-specific key with the token from Slack.</span></span> <span data-ttu-id="8c098-182">V tématu [práci s klíči](#keys).</span><span class="sxs-lookup"><span data-stu-id="8c098-182">See [Working with keys](#keys).</span></span>

<a name="url"></a>
## <a name="customizing-the-http-endpoint"></a><span data-ttu-id="8c098-183">Přizpůsobení koncový bod HTTP</span><span class="sxs-lookup"><span data-stu-id="8c098-183">Customizing the HTTP endpoint</span></span>
<span data-ttu-id="8c098-184">Ve výchozím nastavení vytvořit funkci pro triggeru protokolu HTTP, nebo Webhooku, funkce při adresovatelné s trasou ve tvaru:</span><span class="sxs-lookup"><span data-stu-id="8c098-184">By default when you create a function for an HTTP trigger, or WebHook, the function is addressable with a route of the form:</span></span>

    http://<yourapp>.azurewebsites.net/api/<funcname> 

<span data-ttu-id="8c098-185">Můžete přizpůsobit tuto trasu pomocí volitelné `route` vstup vazbu vlastnosti na triggeru protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c098-185">You can customize this route using the optional `route` property on the HTTP trigger's input binding.</span></span> <span data-ttu-id="8c098-186">Jako příklad následující *function.json* soubor definuje `route` vlastnost pro aktivační procedury HTTP:</span><span class="sxs-lookup"><span data-stu-id="8c098-186">As an example, the following *function.json* file defines a `route` property for an HTTP trigger:</span></span>

```json
    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }
```

<span data-ttu-id="8c098-187">Pomocí této konfigurace, funkce je nyní adresovatelné s následující trasa místo původní trasy.</span><span class="sxs-lookup"><span data-stu-id="8c098-187">Using this configuration, the function is now addressable with the following route instead of the original route.</span></span>

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

<span data-ttu-id="8c098-188">To umožňuje kódu funkce, které podporují dva parametry adresa, "kategorie" a "id".</span><span class="sxs-lookup"><span data-stu-id="8c098-188">This allows the function code to support two parameters in the address, "category" and "id".</span></span> <span data-ttu-id="8c098-189">Můžete použít libovolnou [omezení trasy webové rozhraní API](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) s parametry.</span><span class="sxs-lookup"><span data-stu-id="8c098-189">You can use any [Web API Route Constraint](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) with your parameters.</span></span> <span data-ttu-id="8c098-190">Následující kód funkce jazyka C# využívá oba parametry.</span><span class="sxs-lookup"><span data-stu-id="8c098-190">The following C# function code makes use of both parameters.</span></span>

```csharp
    public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }
```

<span data-ttu-id="8c098-191">Zde je kód Node.js funkce používat stejné parametry trasy.</span><span class="sxs-lookup"><span data-stu-id="8c098-191">Here is Node.js function code to use the same route parameters.</span></span>

```javascript
    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;

        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }

        context.done();
    } 
```

<span data-ttu-id="8c098-192">Standardně jsou všechny funkce trasy předponou *rozhraní api*.</span><span class="sxs-lookup"><span data-stu-id="8c098-192">By default, all function routes are prefixed with *api*.</span></span> <span data-ttu-id="8c098-193">Můžete také upravit nebo odebrat pomocí předpony `http.routePrefix` vlastnost ve vaší *host.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="8c098-193">You can also customize or remove the prefix using the `http.routePrefix` property in your *host.json* file.</span></span> <span data-ttu-id="8c098-194">Následující příklad odebere *rozhraní api* předpona trasy pomocí prázdný řetězec pro předponu v *host.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="8c098-194">The following example removes the *api* route prefix by using an empty string for the prefix in the *host.json* file.</span></span>

```json
    {
      "http": {
        "routePrefix": ""
      }
    }
```

<span data-ttu-id="8c098-195">Podrobné informace o tom, jak aktualizovat *host.json* funkce, najdete v souboru [jak aktualizovat soubory aplikace funkce](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="8c098-195">For detailed information on how to update the *host.json* file for your function, See, [How to update function app files](functions-reference.md#fileupdate).</span></span> 

<span data-ttu-id="8c098-196">Informace o dalších vlastností můžete nakonfigurovat v vaše *host.json* souborů najdete v tématu [host.json odkaz](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="8c098-196">For information on other properties you can configure in your *host.json* file, see [host.json reference](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>


<a name="keys"></a>
## <a name="working-with-keys"></a><span data-ttu-id="8c098-197">Práce s klíči</span><span class="sxs-lookup"><span data-stu-id="8c098-197">Working with keys</span></span>
<span data-ttu-id="8c098-198">HttpTriggers můžete využít klíče pro zvýšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="8c098-198">HttpTriggers can leverage keys for added security.</span></span> <span data-ttu-id="8c098-199">Standardní HttpTrigger můžete použít jako klíč rozhraní API nutnosti klíč nacházet v žádosti.</span><span class="sxs-lookup"><span data-stu-id="8c098-199">A standard HttpTrigger can use these as an API key, requiring the key to be present on the request.</span></span> <span data-ttu-id="8c098-200">Webhooky slouží k autorizaci požadavků v mnoha různými způsoby v závislosti na tom, co poskytovatel podporuje klíče.</span><span class="sxs-lookup"><span data-stu-id="8c098-200">Webhooks can use keys to authorize requests in a variety of ways, depending on what the provider supports.</span></span>

<span data-ttu-id="8c098-201">Klíče jsou uložené v rámci funkce aplikace v Azure a jsou zašifrovaná přinejmenším.</span><span class="sxs-lookup"><span data-stu-id="8c098-201">Keys are stored as part of your function app in Azure and are encrypted at rest.</span></span> <span data-ttu-id="8c098-202">Chcete-li zobrazit vaše klíče, vytvořit nové, nebo vrátit klíče na nové hodnoty, přejděte do jednoho z funkcí v rámci portálu a vyberte "Manage".</span><span class="sxs-lookup"><span data-stu-id="8c098-202">To view your keys, create new ones, or roll keys to new values, navigate to one of your functions within the portal and select "Manage."</span></span> 

<span data-ttu-id="8c098-203">Existují dva typy klíčů:</span><span class="sxs-lookup"><span data-stu-id="8c098-203">There are two types of keys:</span></span>
- <span data-ttu-id="8c098-204">**Klíče hostitele**: tyto klíče jsou sdíleny všechny funkce v rámci funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="8c098-204">**Host keys**: These keys are shared by all functions within the function app.</span></span> <span data-ttu-id="8c098-205">Když se použije jako klíč rozhraní API, tyto rutiny umožňují přístup k žádné funkce v rámci funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="8c098-205">When used as an API key, these allow access to any function within the function app.</span></span>
- <span data-ttu-id="8c098-206">**Funkční klávesy**: tyto klíče se vztahují pouze na určité funkce, pod kterým jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="8c098-206">**Function keys**: These keys apply only to the specific functions under which they are defined.</span></span> <span data-ttu-id="8c098-207">Když se použije jako klíč rozhraní API, tyto pouze povolí přístup k této funkce.</span><span class="sxs-lookup"><span data-stu-id="8c098-207">When used as an API key, these only allow access to that function.</span></span>

<span data-ttu-id="8c098-208">Každý klíč jmenuje pro referenci a je výchozí klíč (s názvem "Výchozí") na úrovni funkce a hostitele.</span><span class="sxs-lookup"><span data-stu-id="8c098-208">Each key is named for reference, and there is a default key (named "default") at the function and host level.</span></span> <span data-ttu-id="8c098-209">**Hlavní klíč** je výchozí hostitele klíč s názvem "_master", která je definována pro každou aplikaci funkce a nesmí být odvolaný.</span><span class="sxs-lookup"><span data-stu-id="8c098-209">The **master key** is a default host key named "_master" that is defined for each function app and cannot be revoked.</span></span> <span data-ttu-id="8c098-210">Poskytuje přístup pro modul runtime rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="8c098-210">It provides administrative access to the runtime APIs.</span></span> <span data-ttu-id="8c098-211">Pomocí `"authLevel": "admin"` vazba JSON bude vyžadovat tento klíč prezentovány v žádosti; Další klíč způsobí selhání autorizace.</span><span class="sxs-lookup"><span data-stu-id="8c098-211">Using `"authLevel": "admin"` in the binding JSON will require this key to be presented on the request; any other key will result in a authorization failure.</span></span>

> [!NOTE]
> <span data-ttu-id="8c098-212">Z důvodu vyšší úroveň oprávnění udělená pomocí hlavního klíče nesmí sdílet s třetími stranami tento klíč nebo distribuovat v nativní klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="8c098-212">Due to the elevated permissions granted by the master key, you should not share this key with third parties or distribute it in native client applications.</span></span> <span data-ttu-id="8c098-213">Při výběru úroveň oprávnění správce, postupujte opatrně.</span><span class="sxs-lookup"><span data-stu-id="8c098-213">Exercise caution when choosing the admin authorization level.</span></span>
> 
> 

### <a name="api-key-authorization"></a><span data-ttu-id="8c098-214">Autorizace pro klíč rozhraní API</span><span class="sxs-lookup"><span data-stu-id="8c098-214">API key authorization</span></span>
<span data-ttu-id="8c098-215">Ve výchozím nastavení vyžaduje HttpTrigger klíč rozhraní API v požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c098-215">By default, an HttpTrigger requires an API key in the HTTP request.</span></span> <span data-ttu-id="8c098-216">Proto požadavku HTTP obvykle vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8c098-216">So your HTTP request normally looks like this:</span></span>

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

<span data-ttu-id="8c098-217">Klíč může být součástí proměnnou řetězce dotazu s názvem `code`, jak je uvedeno výše, nebo ho můžou být součástí `x-functions-key` hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c098-217">The key can be included in a query string variable named `code`, as above, or it can be included in an `x-functions-key` HTTP header.</span></span> <span data-ttu-id="8c098-218">Hodnota klíče může být jakékoli funkce definované pro tuto funkci, nebo všechny hostitele klíče.</span><span class="sxs-lookup"><span data-stu-id="8c098-218">The value of the key can be any function key defined for the function, or any host key.</span></span>

<span data-ttu-id="8c098-219">Můžete povolit požadavky bez klíče nebo určíte, že hlavní klíč musí používat změnou `authLevel` vlastnost Vazba JSON (viz [triggeru protokolu HTTP](#httptrigger)).</span><span class="sxs-lookup"><span data-stu-id="8c098-219">You can choose to allow requests without keys or specify that the master key must be used by changing the `authLevel` property in the binding JSON (see [HTTP trigger](#httptrigger)).</span></span>

### <a name="keys-and-webhooks"></a><span data-ttu-id="8c098-220">Klíče a pomocí webhooků</span><span class="sxs-lookup"><span data-stu-id="8c098-220">Keys and webhooks</span></span>
<span data-ttu-id="8c098-221">Autorizace Webhooku se zpracovává souborem komponentu reciever webhooku součástí HttpTrigger, a tento mechanismus se může lišit podle typu webhooku.</span><span class="sxs-lookup"><span data-stu-id="8c098-221">Webhook authorization is handled by the webhook reciever component, part of the HttpTrigger, and the mechanism varies based on the webhook type.</span></span> <span data-ttu-id="8c098-222">Jednotlivé mechanismy neodpovídá, ale závisí na klíč.</span><span class="sxs-lookup"><span data-stu-id="8c098-222">Each mechanism does, however rely on a key.</span></span> <span data-ttu-id="8c098-223">Ve výchozím nastavení použije klíč funkce s názvem "Výchozí".</span><span class="sxs-lookup"><span data-stu-id="8c098-223">By default, the function key named "default" will be used.</span></span> <span data-ttu-id="8c098-224">Pokud chcete použít jiný kód, musíte nakonfigurovat poskytovatele webhooku odeslat název klíče s požadavkem v jednom z následujících způsobů:</span><span class="sxs-lookup"><span data-stu-id="8c098-224">If you wish to use a different key, you will need to configure the webhook provider to send the key name with the request in one of the following ways:</span></span>

- <span data-ttu-id="8c098-225">**Řetězec dotazu**: Zprostředkovatel předá název klíče v `clientid` parametr řetězce dotazu (například `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span><span class="sxs-lookup"><span data-stu-id="8c098-225">**Query string**: The provider passes the key name in the `clientid` query string parameter (e.g., `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span></span>
- <span data-ttu-id="8c098-226">**Hlavička požadavku**: Zprostředkovatel předá název klíče v `x-functions-clientid` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="8c098-226">**Request header**: The provider passes the key name in the `x-functions-clientid` header.</span></span>

> [!NOTE]
> <span data-ttu-id="8c098-227">Funkční klávesy mají přednost před klíče hostitele.</span><span class="sxs-lookup"><span data-stu-id="8c098-227">Function keys take precedence over host keys.</span></span> <span data-ttu-id="8c098-228">Pokud dva klíče jsou definovány se stejným názvem, použije se funkční klávesy.</span><span class="sxs-lookup"><span data-stu-id="8c098-228">If two keys are defined with the same name, the function key will be used.</span></span>
> 
> 


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a><span data-ttu-id="8c098-229">Ukázky aktivace protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="8c098-229">HTTP trigger samples</span></span>
<span data-ttu-id="8c098-230">Předpokládejme, že máte následující triggeru protokolu HTTP `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="8c098-230">Suppose you have the following HTTP trigger in the `bindings` array of function.json:</span></span>

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

<span data-ttu-id="8c098-231">Viz ukázka konkrétní jazyk, které hledá `name` parametr buď v řetězci dotazu nebo textu požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c098-231">See the language-specific sample that looks for a `name` parameter either in the query string or the body of the HTTP request.</span></span>

* [<span data-ttu-id="8c098-232">C#</span><span class="sxs-lookup"><span data-stu-id="8c098-232">C#</span></span>](#httptriggercsharp)
* [<span data-ttu-id="8c098-233">F#</span><span class="sxs-lookup"><span data-stu-id="8c098-233">F#</span></span>](#httptriggerfsharp)
* [<span data-ttu-id="8c098-234">Node.js</span><span class="sxs-lookup"><span data-stu-id="8c098-234">Node.js</span></span>](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a><span data-ttu-id="8c098-235">Ukázka aktivace protokolu HTTP v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="8c098-235">HTTP trigger sample in C#</span></span> #
```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

<span data-ttu-id="8c098-236">Také můžete vázat na objektů POCO místo `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="8c098-236">You can also bind to a POCO instead of `HttpRequestMessage`.</span></span> <span data-ttu-id="8c098-237">To bude HYDRATOVANÝ z textu požadavku, analyzovat jako JSON.</span><span class="sxs-lookup"><span data-stu-id="8c098-237">This will be hydrated from the body of the request, parsed as JSON.</span></span> <span data-ttu-id="8c098-238">Podobně a typ se dá předat do výstupu odpovědi protokolu HTTP, vazby a to bude vrácen jako text odpovědi, se 200 stavovým kódem.</span><span class="sxs-lookup"><span data-stu-id="8c098-238">Similarly, a type can be passed to the HTTP response output binding, and this will be returned as the response body, with a 200 status code.</span></span>
```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
}
```

<a name="httptriggerfsharp"></a>
### <a name="http-trigger-sample-in-f"></a><span data-ttu-id="8c098-239">Ukázka aktivační události protokolu HTTP v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="8c098-239">HTTP trigger sample in F#</span></span> #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

<span data-ttu-id="8c098-240">Je nutné `project.json` souboru, který používá NuGet tak, aby odkazovaly `FSharp.Interop.Dynamic` a `Dynamitey` sestavení, a to takto:</span><span class="sxs-lookup"><span data-stu-id="8c098-240">You need a `project.json` file that uses NuGet to reference the `FSharp.Interop.Dynamic` and `Dynamitey` assemblies, like this:</span></span>

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

<span data-ttu-id="8c098-241">To bude používat NuGet načíst svoje závislosti a bude odkazovat ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="8c098-241">This will use NuGet to fetch your dependencies and will reference them in your script.</span></span>

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a><span data-ttu-id="8c098-242">Ukázka aktivace protokolu HTTP v Node.JS</span><span class="sxs-lookup"><span data-stu-id="8c098-242">HTTP trigger sample in Node.JS</span></span>
```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```



<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a><span data-ttu-id="8c098-243">Ukázky Webhooku</span><span class="sxs-lookup"><span data-stu-id="8c098-243">Webhook samples</span></span>
<span data-ttu-id="8c098-244">Předpokládejme, že máte následující aktivační události webhooku `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="8c098-244">Suppose you have the following webhook trigger in the `bindings` array of function.json:</span></span>

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

<span data-ttu-id="8c098-245">Naleznete v ukázce pro specifický jazyk, který zaznamenává komentáře problém Githubu.</span><span class="sxs-lookup"><span data-stu-id="8c098-245">See the language-specific sample that logs GitHub issue comments.</span></span>

* [<span data-ttu-id="8c098-246">C#</span><span class="sxs-lookup"><span data-stu-id="8c098-246">C#</span></span>](#hooktriggercsharp)
* [<span data-ttu-id="8c098-247">F#</span><span class="sxs-lookup"><span data-stu-id="8c098-247">F#</span></span>](#hooktriggerfsharp)
* [<span data-ttu-id="8c098-248">Node.js</span><span class="sxs-lookup"><span data-stu-id="8c098-248">Node.js</span></span>](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a><span data-ttu-id="8c098-249">Ukázka Webhooku v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="8c098-249">Webhook sample in C#</span></span> #
```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

<a name="hooktriggerfsharp"></a>

### <a name="webhook-sample-in-f"></a><span data-ttu-id="8c098-250">Ukázka Webhooku v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="8c098-250">Webhook sample in F#</span></span> #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

<a name="hooktriggernodejs"></a>

### <a name="webhook-sample-in-nodejs"></a><span data-ttu-id="8c098-251">Ukázka Webhooku v Node.JS</span><span class="sxs-lookup"><span data-stu-id="8c098-251">Webhook sample in Node.JS</span></span>
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a><span data-ttu-id="8c098-252">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c098-252">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

