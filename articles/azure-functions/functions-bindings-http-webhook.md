---
title: vazby funkce protokolu HTTP a webhooku aaaAzure | Microsoft Docs
description: Pochopit, jak toouse HTTP a webhooku aktivuje a vazeb v Azure Functions.
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
ms.openlocfilehash: c23b7a1443d492ed78c595e97d1d778a7ab12416
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-http-and-webhook-bindings"></a><span data-ttu-id="45df1-104">Azure funkce protokolu HTTP a webhooku vazby</span><span class="sxs-lookup"><span data-stu-id="45df1-104">Azure Functions HTTP and webhook bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="45df1-105">Tento článek vysvětluje, jak se aktivuje tooconfigure a práce s protokoly HTTP a vazeb v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="45df1-105">This article explains how tooconfigure and work with HTTP triggers and bindings in Azure Functions.</span></span>
<span data-ttu-id="45df1-106">Pomocí těchto můžete použít toowebhooks toobuild Azure Functions bez serveru rozhraní API a neodpověděl.</span><span class="sxs-lookup"><span data-stu-id="45df1-106">With these, you can use Azure Functions toobuild serverless APIs and respond toowebhooks.</span></span>

<span data-ttu-id="45df1-107">Azure Functions nabízí hello následující vazby:</span><span class="sxs-lookup"><span data-stu-id="45df1-107">Azure Functions provides hello following bindings:</span></span>
- <span data-ttu-id="45df1-108">[Triggeru protokolu HTTP](#httptrigger) umožňuje vyvolají funkci s žádostí HTTP.</span><span class="sxs-lookup"><span data-stu-id="45df1-108">An [HTTP trigger](#httptrigger) lets you invoke a function with an HTTP request.</span></span> <span data-ttu-id="45df1-109">To může být vlastní toorespond příliš[webhooky](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="45df1-109">This can be customized toorespond too[webhooks](#hooktrigger).</span></span>
- <span data-ttu-id="45df1-110">[HTTP výstup vazby](#output) vám umožní toorespond toohello požadavku.</span><span class="sxs-lookup"><span data-stu-id="45df1-110">An [HTTP output binding](#output) allows you toorespond toohello request.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a><span data-ttu-id="45df1-111">Trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="45df1-111">HTTP trigger</span></span>
<span data-ttu-id="45df1-112">Aktivace protokolu HTTP Hello spustí funkce v požadavku HTTP tooan odpovědi.</span><span class="sxs-lookup"><span data-stu-id="45df1-112">hello HTTP trigger will execute your function in response tooan HTTP request.</span></span> <span data-ttu-id="45df1-113">Můžete ji přizpůsobit toorespond tooa konkrétní adresu URL nebo sady metod HTTP.</span><span class="sxs-lookup"><span data-stu-id="45df1-113">You can customize it toorespond tooa particular URL or set of HTTP methods.</span></span> <span data-ttu-id="45df1-114">Aktivační událost INSTEAD HTTP může být také nakonfigurované toorespond toowebhooks.</span><span class="sxs-lookup"><span data-stu-id="45df1-114">An HTTP trigger can also be configured toorespond toowebhooks.</span></span> 

<span data-ttu-id="45df1-115">Pokud používáte portál hello funkce, můžete také začít používat hned použití předem vytvořené šablony.</span><span class="sxs-lookup"><span data-stu-id="45df1-115">If using hello Functions portal, you can also get started right away using a pre-made template.</span></span> <span data-ttu-id="45df1-116">Vyberte **novou funkci** a vybírat "Rozhraní API a Webhooky" hello **scénář** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="45df1-116">Select **New function** and choose "API & Webhooks" from hello **Scenario** dropdown.</span></span> <span data-ttu-id="45df1-117">Vyberte jednu z hello šablon a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="45df1-117">Select one of hello templates and click **Create**.</span></span>

<span data-ttu-id="45df1-118">Ve výchozím nastavení bude aktivační procedury HTTP odpovídat toohello žádost s stavový kód HTTP 200 OK a prázdným textem zprávy.</span><span class="sxs-lookup"><span data-stu-id="45df1-118">By default, an HTTP trigger will respond toohello request with an HTTP 200 OK status code and an empty body.</span></span> <span data-ttu-id="45df1-119">toomodify hello odpovědi, nakonfigurovat [HTTP výstup vazby](#output)</span><span class="sxs-lookup"><span data-stu-id="45df1-119">toomodify hello response, configure an [HTTP output binding](#output)</span></span>

### <a name="configuring-an-http-trigger"></a><span data-ttu-id="45df1-120">Konfigurace aktivace protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="45df1-120">Configuring an HTTP trigger</span></span>
<span data-ttu-id="45df1-121">Aktivační událost INSTEAD HTTP je definována včetně toohello podobně jako objekt JSON následující v hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="45df1-121">An HTTP trigger is defined by including a JSON object similar toohello following in hello `bindings` array of function.json:</span></span>

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
<span data-ttu-id="45df1-122">Vazba Hello podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="45df1-122">hello binding supports hello following properties:</span></span>

* <span data-ttu-id="45df1-123">**název** : požadované – název proměnné hello používá v kódu funkce pro požadavek hello nebo textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="45df1-123">**name** : Required - hello variable name used in function code for hello request or request body.</span></span> <span data-ttu-id="45df1-124">V tématu [práce s aktivační procedury HTTP z kódu](#httptriggerusage).</span><span class="sxs-lookup"><span data-stu-id="45df1-124">See [Working with an HTTP trigger from code](#httptriggerusage).</span></span>
* <span data-ttu-id="45df1-125">**typ** : požadované – musí být nastaven příliš "httpTrigger".</span><span class="sxs-lookup"><span data-stu-id="45df1-125">**type** : Required - must be set too"httpTrigger".</span></span>
* <span data-ttu-id="45df1-126">**směr** : požadované – musí být nastaven příliš "v".</span><span class="sxs-lookup"><span data-stu-id="45df1-126">**direction** : Required - must be set too"in".</span></span>
* <span data-ttu-id="45df1-127">_authLevel_ : Určuje, jaké klíče, pokud existuje, třeba toobe hello požadavek neobsahuje ve funkci hello tooinvoke pořadí.</span><span class="sxs-lookup"><span data-stu-id="45df1-127">_authLevel_ : This determines what keys, if any, need toobe present on hello request in order tooinvoke hello function.</span></span> <span data-ttu-id="45df1-128">V tématu [práci s klíči](#keys) níže.</span><span class="sxs-lookup"><span data-stu-id="45df1-128">See [Working with keys](#keys) below.</span></span> <span data-ttu-id="45df1-129">Hello hodnota může být jedna z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="45df1-129">hello value can be one of hello following:</span></span>
    * <span data-ttu-id="45df1-130">_Anonymní_: je požadován klíč rozhraní API ne.</span><span class="sxs-lookup"><span data-stu-id="45df1-130">_anonymous_: No API key is required.</span></span>
    * <span data-ttu-id="45df1-131">_funkce_: je požadován klíč rozhraní API specifických funkcí.</span><span class="sxs-lookup"><span data-stu-id="45df1-131">_function_: A function-specific API key is required.</span></span> <span data-ttu-id="45df1-132">Toto je hello výchozí hodnotu, pokud žádný je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="45df1-132">This is hello default value if none is provided.</span></span>
    * <span data-ttu-id="45df1-133">_správce_ : je nezbytný hlavní klíč hello.</span><span class="sxs-lookup"><span data-stu-id="45df1-133">_admin_ : hello master key is required.</span></span>
* <span data-ttu-id="45df1-134">**metody** : Toto je pole metod hello HTTP bude odpovídat toowhich hello funkce.</span><span class="sxs-lookup"><span data-stu-id="45df1-134">**methods** : This is an array of hello HTTP methods toowhich hello function will respond.</span></span> <span data-ttu-id="45df1-135">Pokud není zadáno, bude funkce hello odpovídat metody tooall HTTP.</span><span class="sxs-lookup"><span data-stu-id="45df1-135">If not specified, hello function will respond tooall HTTP methods.</span></span> <span data-ttu-id="45df1-136">V tématu [přizpůsobení koncový bod hello HTTP](#url).</span><span class="sxs-lookup"><span data-stu-id="45df1-136">See [Customizing hello HTTP endpoint](#url).</span></span>
* <span data-ttu-id="45df1-137">**trasy** : definuje šablonu trasy hello, řízení toowhich žádosti o funkce bude odpovídat adresy URL.</span><span class="sxs-lookup"><span data-stu-id="45df1-137">**route** : This defines hello route template, controlling toowhich request URLs your function will respond.</span></span> <span data-ttu-id="45df1-138">Hello výchozí hodnota není-li žádné je `<functionname>`.</span><span class="sxs-lookup"><span data-stu-id="45df1-138">hello default value if none is provided is `<functionname>`.</span></span> <span data-ttu-id="45df1-139">V tématu [přizpůsobení koncový bod hello HTTP](#url).</span><span class="sxs-lookup"><span data-stu-id="45df1-139">See [Customizing hello HTTP endpoint](#url).</span></span>
* <span data-ttu-id="45df1-140">**webHookType** : tím se nakonfiguruje tooact aktivační událost hello HTTP jako reciever webhooku pro zadaného zprostředkovatele hello.</span><span class="sxs-lookup"><span data-stu-id="45df1-140">**webHookType** : This configures hello HTTP trigger tooact as a webhook reciever for hello specified provider.</span></span> <span data-ttu-id="45df1-141">Hello _metody_ vlastnost neměla by být nastavená, pokud to jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="45df1-141">hello _methods_ property should not be set if this is chosen.</span></span> <span data-ttu-id="45df1-142">V tématu [odpovídá toowebhooks](#hooktrigger).</span><span class="sxs-lookup"><span data-stu-id="45df1-142">See [Responding toowebhooks](#hooktrigger).</span></span> <span data-ttu-id="45df1-143">Hello hodnota může být jedna z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="45df1-143">hello value can be one of hello following:</span></span>
    * <span data-ttu-id="45df1-144">_genericJson_ : webhooku koncový bod obecné účely bez logiku pro konkrétního zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="45df1-144">_genericJson_ : A general purpose webhook endpoint without logic for a specific provider.</span></span>
    * <span data-ttu-id="45df1-145">_github_ : funkce hello bude odpovídat tooGitHub webhooky.</span><span class="sxs-lookup"><span data-stu-id="45df1-145">_github_ : hello function will respond tooGitHub webhooks.</span></span> <span data-ttu-id="45df1-146">Hello _authLevel_ vlastnost neměla by být nastavená, pokud to jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="45df1-146">hello _authLevel_ property should not be set if this is chosen.</span></span>
    * <span data-ttu-id="45df1-147">_slack_ : funkce hello bude odpovídat tooSlack webhooky.</span><span class="sxs-lookup"><span data-stu-id="45df1-147">_slack_ : hello function will respond tooSlack webhooks.</span></span> <span data-ttu-id="45df1-148">Hello _authLevel_ vlastnost neměla by být nastavená, pokud to jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="45df1-148">hello _authLevel_ property should not be set if this is chosen.</span></span>

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a><span data-ttu-id="45df1-149">Práce s aktivační procedury HTTP z kódu</span><span class="sxs-lookup"><span data-stu-id="45df1-149">Working with an HTTP trigger from code</span></span>
<span data-ttu-id="45df1-150">Pro funkce C# a F #, je možné deklarovat hello typ vaše vstupní toobe aktivační události buď `HttpRequestMessage` nebo vlastního typu.</span><span class="sxs-lookup"><span data-stu-id="45df1-150">For C# and F# functions, you can declare hello type of your trigger input toobe either `HttpRequestMessage` or a custom type.</span></span> <span data-ttu-id="45df1-151">Pokud se rozhodnete `HttpRequestMessage`, pak budete mít objekt žádosti toohello úplný přístup.</span><span class="sxs-lookup"><span data-stu-id="45df1-151">If you choose `HttpRequestMessage`, then you will get full access toohello request object.</span></span> <span data-ttu-id="45df1-152">Pro vlastní typ (například POCO) funkce pokusí textu žádosti hello tooparse jako vlastnosti objektu hello toopopulate JSON.</span><span class="sxs-lookup"><span data-stu-id="45df1-152">For a custom type (such as a POCO), Functions will attempt tooparse hello request body as JSON toopopulate hello object properties.</span></span>

<span data-ttu-id="45df1-153">Pro funkce Node.js hello Functions runtime poskytuje textu hello žádosti namísto objektu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="45df1-153">For Node.js functions, hello Functions runtime provides hello request body instead of hello request object.</span></span>

<span data-ttu-id="45df1-154">V tématu [HTTP aktivační událost ukázky](#httptriggersample) například použití.</span><span class="sxs-lookup"><span data-stu-id="45df1-154">See [HTTP trigger samples](#httptriggersample) for example usages.</span></span>


<a name="output"></a>
## <a name="http-response-output-binding"></a><span data-ttu-id="45df1-155">Odpověď HTTP výstup vazby</span><span class="sxs-lookup"><span data-stu-id="45df1-155">HTTP response output binding</span></span>
<span data-ttu-id="45df1-156">Použijte hello HTTP výstup vazby toorespond toohello HTTP žádost odesílatele.</span><span class="sxs-lookup"><span data-stu-id="45df1-156">Use hello HTTP output binding toorespond toohello HTTP request sender.</span></span> <span data-ttu-id="45df1-157">Tato vazba vyžaduje aktivační procedury protokolu HTTP a umožňuje vám toocustomize hello odpověď přidruženou k požadavek aktivace hello.</span><span class="sxs-lookup"><span data-stu-id="45df1-157">This binding requires an HTTP trigger and allows you toocustomize hello response associated with hello trigger's request.</span></span> <span data-ttu-id="45df1-158">Pokud výstup vazba HTTP není zadáno, aktivační procedury HTTP vrátí s prázdným textem zprávy HTTP 200 OK.</span><span class="sxs-lookup"><span data-stu-id="45df1-158">If an HTTP output binding is not provided, an HTTP trigger will return HTTP 200 OK with an empty body.</span></span> 

### <a name="configuring-an-http-output-binding"></a><span data-ttu-id="45df1-159">Konfigurace HTTP výstup vazby</span><span class="sxs-lookup"><span data-stu-id="45df1-159">Configuring an HTTP output binding</span></span>
<span data-ttu-id="45df1-160">výstup Hello HTTP vazba je definována včetně toohello podobně jako objekt JSON následující v hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="45df1-160">hello HTTP output binding is defined by including a JSON object similar toohello following in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
<span data-ttu-id="45df1-161">Vazba Hello obsahuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="45df1-161">hello binding contains hello following properties:</span></span>

* <span data-ttu-id="45df1-162">**název** : požadované - hello používá v kódu funkce pro odpověď hello název proměnné.</span><span class="sxs-lookup"><span data-stu-id="45df1-162">**name** : Required - hello variable name used in function code for hello response.</span></span> <span data-ttu-id="45df1-163">V tématu [práci s HTTP výstup vazba z kódu](#outputusage).</span><span class="sxs-lookup"><span data-stu-id="45df1-163">See [Working with an HTTP output binding from code](#outputusage).</span></span>
* <span data-ttu-id="45df1-164">**typ** : požadované – musí být nastaven příliš "http".</span><span class="sxs-lookup"><span data-stu-id="45df1-164">**type** : Required - must be set too"http".</span></span>
* <span data-ttu-id="45df1-165">**směr** : požadované – musí být nastaven příliš "na".</span><span class="sxs-lookup"><span data-stu-id="45df1-165">**direction** : Required - must be set too"out".</span></span>

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a><span data-ttu-id="45df1-166">Práce s HTTP výstup vazba z kódu</span><span class="sxs-lookup"><span data-stu-id="45df1-166">Working with an HTTP output binding from code</span></span>
<span data-ttu-id="45df1-167">Můžete použít hello výstupní parametr (například "res") toorespond toohello protokolu http nebo webhooku volajícího.</span><span class="sxs-lookup"><span data-stu-id="45df1-167">You can use hello output parameter (e.g., "res") toorespond toohello http or webhook caller.</span></span> <span data-ttu-id="45df1-168">Alternativně můžete použít standardní `Request.CreateResponse()` (C#) nebo `context.res` (Node.JS) vzor tooreturn vaši odpověď.</span><span class="sxs-lookup"><span data-stu-id="45df1-168">Alternatively, you can use the standard `Request.CreateResponse()` (C#) or `context.res` (Node.JS) pattern tooreturn your response.</span></span> <span data-ttu-id="45df1-169">Příklady na tom, jak toouse hello druhá metoda najdete v tématu [HTTP aktivační událost ukázky](#httptriggersample) a [ukázky aktivační události Webhooku](#hooktriggersample).</span><span class="sxs-lookup"><span data-stu-id="45df1-169">For examples on how toouse hello latter method, see [HTTP trigger samples](#httptriggersample) and [Webhook trigger samples](#hooktriggersample).</span></span>


<a name="hooktrigger"></a>
## <a name="responding-toowebhooks"></a><span data-ttu-id="45df1-170">Odpovídá toowebhooks</span><span class="sxs-lookup"><span data-stu-id="45df1-170">Responding toowebhooks</span></span>
<span data-ttu-id="45df1-171">Aktivační událost INSTEAD HTTP s hello _webHookType_ , bude mít vlastnost nakonfigurované toorespond příliš[webhooky](https://en.wikipedia.org/wiki/Webhook).</span><span class="sxs-lookup"><span data-stu-id="45df1-171">An HTTP trigger with hello _webHookType_ property will be configured toorespond too[webhooks](https://en.wikipedia.org/wiki/Webhook).</span></span> <span data-ttu-id="45df1-172">základní konfigurace Hello používá nastavení "genericJson" hello.</span><span class="sxs-lookup"><span data-stu-id="45df1-172">hello basic configuration uses hello "genericJson" setting.</span></span> <span data-ttu-id="45df1-173">To omezuje požadavky tooonly ty pomocí protokolu HTTP POST a s hello `application/json` typ obsahu.</span><span class="sxs-lookup"><span data-stu-id="45df1-173">This restricts requests tooonly those using HTTP POST and with hello `application/json` content type.</span></span>

<span data-ttu-id="45df1-174">Hello aktivační událost se dá dál šité na míru tooa konkrétní webhooku zprostředkovatele (například [Githubu](https://developer.github.com/webhooks/) a [Slack](https://api.slack.com/outgoing-webhooks)).</span><span class="sxs-lookup"><span data-stu-id="45df1-174">hello trigger can additionally be tailored tooa specific webhook provider (e.g., [GitHub](https://developer.github.com/webhooks/) and [Slack](https://api.slack.com/outgoing-webhooks)).</span></span> <span data-ttu-id="45df1-175">Pokud je zadán poskytovatele, hello Functions runtime mohou starat o logiku hello zprostředkovatele ověření pro vás.</span><span class="sxs-lookup"><span data-stu-id="45df1-175">If a provider is specified, hello Functions runtime can take care of hello provider's validation logic for you.</span></span>  

### <a name="configuring-github-as-a-webhook-provider"></a><span data-ttu-id="45df1-176">Konfigurace jako zprostředkovatel webhook Githubu</span><span class="sxs-lookup"><span data-stu-id="45df1-176">Configuring GitHub as a webhook provider</span></span>
<span data-ttu-id="45df1-177">toorespond tooGitHub webhooků, nejprve vytvořte funkce s aktivační procedury protokolu HTTP a nastavit hello _webHookType_ vlastnost příliš "githubu".</span><span class="sxs-lookup"><span data-stu-id="45df1-177">toorespond tooGitHub webhooks, first create your function with an HTTP Trigger, and set hello _webHookType_ property too"github".</span></span> <span data-ttu-id="45df1-178">Zkopírujte jeho [URL](#url) a [klíč rozhraní API](#keys) do úložiště GitHub **přidat webhooku** stránky.</span><span class="sxs-lookup"><span data-stu-id="45df1-178">Then copy its [URL](#url) and [API key](#keys) into your GitHub repository's **Add webhook** page.</span></span> <span data-ttu-id="45df1-179">Najdete v článku na Githubu [vytváření Webhooky](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) dokumentaci další informace.</span><span class="sxs-lookup"><span data-stu-id="45df1-179">See GitHub's [Creating Webhooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) documentation for more.</span></span>

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="configuring-slack-as-a-webhook-provider"></a><span data-ttu-id="45df1-180">Konfigurace systému Slack jako zprostředkovatel webhooku</span><span class="sxs-lookup"><span data-stu-id="45df1-180">Configuring Slack as a webhook provider</span></span>
<span data-ttu-id="45df1-181">Slack webhooku Hello generuje token pro vás místo umožňuje zadat, takže je nutné nakonfigurovat specifické funkce klíč pomocí hello tokenu z Slack.</span><span class="sxs-lookup"><span data-stu-id="45df1-181">hello Slack webhook generates a token for you instead of letting you specify it, so you must configure a function-specific key with hello token from Slack.</span></span> <span data-ttu-id="45df1-182">V tématu [práci s klíči](#keys).</span><span class="sxs-lookup"><span data-stu-id="45df1-182">See [Working with keys](#keys).</span></span>

<a name="url"></a>
## <a name="customizing-hello-http-endpoint"></a><span data-ttu-id="45df1-183">Přizpůsobení koncový bod HTTP hello</span><span class="sxs-lookup"><span data-stu-id="45df1-183">Customizing hello HTTP endpoint</span></span>
<span data-ttu-id="45df1-184">Ve výchozím nastavení při vytváření funkce triggeru protokolu HTTP, nebo Webhooku, funkce hello je adresovatelný s trasou hello formuláře:</span><span class="sxs-lookup"><span data-stu-id="45df1-184">By default when you create a function for an HTTP trigger, or WebHook, hello function is addressable with a route of hello form:</span></span>

    http://<yourapp>.azurewebsites.net/api/<funcname> 

<span data-ttu-id="45df1-185">Můžete přizpůsobit tuto trasu pomocí hello volitelné `route` vlastnost na aktivační událost hello HTTP vstup vazby.</span><span class="sxs-lookup"><span data-stu-id="45df1-185">You can customize this route using hello optional `route` property on hello HTTP trigger's input binding.</span></span> <span data-ttu-id="45df1-186">Jako příklad hello následující *function.json* soubor definuje `route` vlastnost pro aktivační procedury HTTP:</span><span class="sxs-lookup"><span data-stu-id="45df1-186">As an example, hello following *function.json* file defines a `route` property for an HTTP trigger:</span></span>

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

<span data-ttu-id="45df1-187">Pomocí této konfigurace, hello funkce je nyní adresovatelné s hello následující trasy místo hello původní trasy.</span><span class="sxs-lookup"><span data-stu-id="45df1-187">Using this configuration, hello function is now addressable with hello following route instead of hello original route.</span></span>

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

<span data-ttu-id="45df1-188">To umožňuje kód funkce hello toosupport dva parametry hello adresa, "kategorie" a "id".</span><span class="sxs-lookup"><span data-stu-id="45df1-188">This allows hello function code toosupport two parameters in hello address, "category" and "id".</span></span> <span data-ttu-id="45df1-189">Můžete použít libovolnou [omezení trasy webové rozhraní API](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) s parametry.</span><span class="sxs-lookup"><span data-stu-id="45df1-189">You can use any [Web API Route Constraint](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) with your parameters.</span></span> <span data-ttu-id="45df1-190">Dobrý den, následující funkce kód C# využívá oba parametry.</span><span class="sxs-lookup"><span data-stu-id="45df1-190">hello following C# function code makes use of both parameters.</span></span>

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

<span data-ttu-id="45df1-191">Zde je Node.js funkce kód toouse hello stejné parametry trasy.</span><span class="sxs-lookup"><span data-stu-id="45df1-191">Here is Node.js function code toouse hello same route parameters.</span></span>

```javascript
    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;

        if (!id) {
            context.res = {
                // status: 200, /* Defaults too200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults too200 */
                body: category + " item with id = " + id + " was requested."
            };
        }

        context.done();
    } 
```

<span data-ttu-id="45df1-192">Standardně jsou všechny funkce trasy předponou *rozhraní api*.</span><span class="sxs-lookup"><span data-stu-id="45df1-192">By default, all function routes are prefixed with *api*.</span></span> <span data-ttu-id="45df1-193">Můžete také upravit nebo odebrat předpony hello pomocí hello `http.routePrefix` vlastnost ve vaší *host.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="45df1-193">You can also customize or remove hello prefix using hello `http.routePrefix` property in your *host.json* file.</span></span> <span data-ttu-id="45df1-194">Hello následující příklad odebere hello *rozhraní api* předpona trasy pomocí prázdný řetězec pro předponu hello hello *host.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="45df1-194">hello following example removes hello *api* route prefix by using an empty string for hello prefix in hello *host.json* file.</span></span>

```json
    {
      "http": {
        "routePrefix": ""
      }
    }
```

<span data-ttu-id="45df1-195">Podrobné informace o tom, tooupdate hello *host.json* funkce, najdete v souboru [jak tooupdate funkce soubory aplikace](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="45df1-195">For detailed information on how tooupdate hello *host.json* file for your function, See, [How tooupdate function app files](functions-reference.md#fileupdate).</span></span> 

<span data-ttu-id="45df1-196">Informace o dalších vlastností můžete nakonfigurovat v vaše *host.json* souborů najdete v tématu [host.json odkaz](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="45df1-196">For information on other properties you can configure in your *host.json* file, see [host.json reference](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>


<a name="keys"></a>
## <a name="working-with-keys"></a><span data-ttu-id="45df1-197">Práce s klíči</span><span class="sxs-lookup"><span data-stu-id="45df1-197">Working with keys</span></span>
<span data-ttu-id="45df1-198">HttpTriggers můžete využít klíče pro zvýšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="45df1-198">HttpTriggers can leverage keys for added security.</span></span> <span data-ttu-id="45df1-199">Standardní HttpTrigger můžete použít jako klíč rozhraní API nutnosti hello klíče toobe hello požadavek neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="45df1-199">A standard HttpTrigger can use these as an API key, requiring hello key toobe present on hello request.</span></span> <span data-ttu-id="45df1-200">Webhooky pomocí klíče tooauthorize požadavků v mnoha různými způsoby v závislosti na tom, jaké hello zprostředkovatel podporuje.</span><span class="sxs-lookup"><span data-stu-id="45df1-200">Webhooks can use keys tooauthorize requests in a variety of ways, depending on what hello provider supports.</span></span>

<span data-ttu-id="45df1-201">Klíče jsou uložené v rámci funkce aplikace v Azure a jsou zašifrovaná přinejmenším.</span><span class="sxs-lookup"><span data-stu-id="45df1-201">Keys are stored as part of your function app in Azure and are encrypted at rest.</span></span> <span data-ttu-id="45df1-202">tooview klíče, vytvořit nové nebo klíčů pro vrácení hodnoty toonew, přejděte tooone funkcí portálu hello a vyberte "Manage".</span><span class="sxs-lookup"><span data-stu-id="45df1-202">tooview your keys, create new ones, or roll keys toonew values, navigate tooone of your functions within hello portal and select "Manage."</span></span> 

<span data-ttu-id="45df1-203">Existují dva typy klíčů:</span><span class="sxs-lookup"><span data-stu-id="45df1-203">There are two types of keys:</span></span>
- <span data-ttu-id="45df1-204">**Klíče hostitele**: tyto klíče jsou sdíleny všechny funkce v rámci aplikace hello funkce.</span><span class="sxs-lookup"><span data-stu-id="45df1-204">**Host keys**: These keys are shared by all functions within hello function app.</span></span> <span data-ttu-id="45df1-205">Když se použije jako klíč rozhraní API, tyto rutiny umožňují funkce tooany přístup v rámci aplikace hello funkce.</span><span class="sxs-lookup"><span data-stu-id="45df1-205">When used as an API key, these allow access tooany function within hello function app.</span></span>
- <span data-ttu-id="45df1-206">**Funkční klávesy**: tyto klíče použít jenom toohello specifické funkce, pod kterým jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="45df1-206">**Function keys**: These keys apply only toohello specific functions under which they are defined.</span></span> <span data-ttu-id="45df1-207">Když se použije jako klíč rozhraní API, povolí tyto pouze funkce toothat přístup.</span><span class="sxs-lookup"><span data-stu-id="45df1-207">When used as an API key, these only allow access toothat function.</span></span>

<span data-ttu-id="45df1-208">Každý klíč jmenuje pro referenci a úrovni hello funkce a hostitele je výchozí klíč (s názvem "Výchozí").</span><span class="sxs-lookup"><span data-stu-id="45df1-208">Each key is named for reference, and there is a default key (named "default") at hello function and host level.</span></span> <span data-ttu-id="45df1-209">Hello **hlavní klíč** je výchozí hostitele klíč s názvem "_master", která je definována pro každou aplikaci funkce a nesmí být odvolaný.</span><span class="sxs-lookup"><span data-stu-id="45df1-209">hello **master key** is a default host key named "_master" that is defined for each function app and cannot be revoked.</span></span> <span data-ttu-id="45df1-210">Poskytuje přístup pro správu toohello runtime rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="45df1-210">It provides administrative access toohello runtime APIs.</span></span> <span data-ttu-id="45df1-211">Pomocí `"authLevel": "admin"` ve hello vazby JSON vyžaduje tento klíč toobe uvedené na žádost hello; Další klíč způsobí selhání autorizace.</span><span class="sxs-lookup"><span data-stu-id="45df1-211">Using `"authLevel": "admin"` in hello binding JSON will require this key toobe presented on hello request; any other key will result in a authorization failure.</span></span>

> [!NOTE]
> <span data-ttu-id="45df1-212">Kvůli toohello zvýšená oprávnění udělené pomocí hello hlavního klíče, by neměly sdílet s třetími stranami tento klíč nebo distribuovat v nativní klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="45df1-212">Due toohello elevated permissions granted by hello master key, you should not share this key with third parties or distribute it in native client applications.</span></span> <span data-ttu-id="45df1-213">Při výběru hello úroveň oprávnění správce, postupujte opatrně.</span><span class="sxs-lookup"><span data-stu-id="45df1-213">Exercise caution when choosing hello admin authorization level.</span></span>
> 
> 

### <a name="api-key-authorization"></a><span data-ttu-id="45df1-214">Autorizace pro klíč rozhraní API</span><span class="sxs-lookup"><span data-stu-id="45df1-214">API key authorization</span></span>
<span data-ttu-id="45df1-215">Ve výchozím nastavení vyžaduje HttpTrigger klíč rozhraní API v požadavku HTTP hello.</span><span class="sxs-lookup"><span data-stu-id="45df1-215">By default, an HttpTrigger requires an API key in hello HTTP request.</span></span> <span data-ttu-id="45df1-216">Proto požadavku HTTP obvykle vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="45df1-216">So your HTTP request normally looks like this:</span></span>

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

<span data-ttu-id="45df1-217">klíč Hello můžou být součástí proměnnou řetězce dotazu s názvem `code`, jak je uvedeno výše, nebo ho můžou být součástí `x-functions-key` hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="45df1-217">hello key can be included in a query string variable named `code`, as above, or it can be included in an `x-functions-key` HTTP header.</span></span> <span data-ttu-id="45df1-218">Hodnota Hello hello klíče může být jakékoli funkce pro funkci hello definováno, nebo všechny hostitele klíče.</span><span class="sxs-lookup"><span data-stu-id="45df1-218">hello value of hello key can be any function key defined for hello function, or any host key.</span></span>

<span data-ttu-id="45df1-219">Můžete zvolit tooallow požadavků bez klíče, nebo zadat, že hello hlavní klíč musí být použita změnou hello `authLevel` vlastnost hello vazby JSON (viz [triggeru protokolu HTTP](#httptrigger)).</span><span class="sxs-lookup"><span data-stu-id="45df1-219">You can choose tooallow requests without keys or specify that hello master key must be used by changing hello `authLevel` property in hello binding JSON (see [HTTP trigger](#httptrigger)).</span></span>

### <a name="keys-and-webhooks"></a><span data-ttu-id="45df1-220">Klíče a pomocí webhooků</span><span class="sxs-lookup"><span data-stu-id="45df1-220">Keys and webhooks</span></span>
<span data-ttu-id="45df1-221">Autorizace Webhooku se zpracovává hello webhooku reciever součásti, součástí hello HttpTrigger a hello mechanismus se může lišit podle typu webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="45df1-221">Webhook authorization is handled by hello webhook reciever component, part of hello HttpTrigger, and hello mechanism varies based on hello webhook type.</span></span> <span data-ttu-id="45df1-222">Jednotlivé mechanismy neodpovídá, ale závisí na klíč.</span><span class="sxs-lookup"><span data-stu-id="45df1-222">Each mechanism does, however rely on a key.</span></span> <span data-ttu-id="45df1-223">Ve výchozím nastavení použije klíč hello funkce s názvem "Výchozí".</span><span class="sxs-lookup"><span data-stu-id="45df1-223">By default, hello function key named "default" will be used.</span></span> <span data-ttu-id="45df1-224">Pokud chcete toouse jiný klíč, budete potřebovat tooconfigure hello zprostředkovatele toosend hello klíče název webhooku hello požadavku v jednom z následujících způsobů hello:</span><span class="sxs-lookup"><span data-stu-id="45df1-224">If you wish toouse a different key, you will need tooconfigure hello webhook provider toosend hello key name with hello request in one of hello following ways:</span></span>

- <span data-ttu-id="45df1-225">**Řetězec dotazu**: hello zprostředkovatele předá název klíče hello hello `clientid` parametr řetězce dotazu (například `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span><span class="sxs-lookup"><span data-stu-id="45df1-225">**Query string**: hello provider passes hello key name in hello `clientid` query string parameter (e.g., `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).</span></span>
- <span data-ttu-id="45df1-226">**Hlavička požadavku**: hello zprostředkovatele předá název klíče hello hello `x-functions-clientid` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="45df1-226">**Request header**: hello provider passes hello key name in hello `x-functions-clientid` header.</span></span>

> [!NOTE]
> <span data-ttu-id="45df1-227">Funkční klávesy mají přednost před klíče hostitele.</span><span class="sxs-lookup"><span data-stu-id="45df1-227">Function keys take precedence over host keys.</span></span> <span data-ttu-id="45df1-228">Pokud dva klíče jsou definovány s hello stejný název, hello funkce klíč se použije.</span><span class="sxs-lookup"><span data-stu-id="45df1-228">If two keys are defined with hello same name, hello function key will be used.</span></span>
> 
> 


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a><span data-ttu-id="45df1-229">Ukázky aktivace protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="45df1-229">HTTP trigger samples</span></span>
<span data-ttu-id="45df1-230">Předpokládejme, že máte následující triggeru protokolu HTTP v hello hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="45df1-230">Suppose you have hello following HTTP trigger in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

<span data-ttu-id="45df1-231">V tématu vzorku hello konkrétní jazyk, který hledá `name` parametr buď v řetězci dotazu hello nebo textu hello hello HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="45df1-231">See hello language-specific sample that looks for a `name` parameter either in hello query string or hello body of hello HTTP request.</span></span>

* [<span data-ttu-id="45df1-232">C#</span><span class="sxs-lookup"><span data-stu-id="45df1-232">C#</span></span>](#httptriggercsharp)
* [<span data-ttu-id="45df1-233">F#</span><span class="sxs-lookup"><span data-stu-id="45df1-233">F#</span></span>](#httptriggerfsharp)
* [<span data-ttu-id="45df1-234">Node.js</span><span class="sxs-lookup"><span data-stu-id="45df1-234">Node.js</span></span>](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a><span data-ttu-id="45df1-235">Ukázka aktivace protokolu HTTP v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="45df1-235">HTTP trigger sample in C#</span></span> #
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

    // Set name tooquery string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on hello query string or in hello request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

<span data-ttu-id="45df1-236">Můžete také navázat tooa objektů POCO místo `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="45df1-236">You can also bind tooa POCO instead of `HttpRequestMessage`.</span></span> <span data-ttu-id="45df1-237">To bude HYDRATOVANÝ z textu hello hello požadavku, analyzovat jako JSON.</span><span class="sxs-lookup"><span data-stu-id="45df1-237">This will be hydrated from hello body of hello request, parsed as JSON.</span></span> <span data-ttu-id="45df1-238">Podobně typu lze předat výstup odezvy toohello HTTP vazby, a to bude vrácen jako hello odpovědi, které se 200 stavovým kódem.</span><span class="sxs-lookup"><span data-stu-id="45df1-238">Similarly, a type can be passed toohello HTTP response output binding, and this will be returned as hello response body, with a 200 status code.</span></span>
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
### <a name="http-trigger-sample-in-f"></a><span data-ttu-id="45df1-239">Ukázka aktivační události protokolu HTTP v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="45df1-239">HTTP trigger sample in F#</span></span> #
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
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on hello query string or in hello request body")
    } |> Async.StartAsTask
```

<span data-ttu-id="45df1-240">Je nutné `project.json` souboru, který NuGet tooreference hello používá `FSharp.Interop.Dynamic` a `Dynamitey` sestavení, a to takto:</span><span class="sxs-lookup"><span data-stu-id="45df1-240">You need a `project.json` file that uses NuGet tooreference hello `FSharp.Interop.Dynamic` and `Dynamitey` assemblies, like this:</span></span>

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

<span data-ttu-id="45df1-241">To bude používat NuGet toofetch svoje závislosti a bude odkazovat ve vašem skriptu.</span><span class="sxs-lookup"><span data-stu-id="45df1-241">This will use NuGet toofetch your dependencies and will reference them in your script.</span></span>

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a><span data-ttu-id="45df1-242">Ukázka aktivace protokolu HTTP v Node.JS</span><span class="sxs-lookup"><span data-stu-id="45df1-242">HTTP trigger sample in Node.JS</span></span>
```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults too200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on hello query string or in hello request body"
        };
    }
    context.done();
};
```



<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a><span data-ttu-id="45df1-243">Ukázky Webhooku</span><span class="sxs-lookup"><span data-stu-id="45df1-243">Webhook samples</span></span>
<span data-ttu-id="45df1-244">Předpokládejme, že máte následující aktivační události webhooku v hello hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="45df1-244">Suppose you have hello following webhook trigger in hello `bindings` array of function.json:</span></span>

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

<span data-ttu-id="45df1-245">V tématu vzorku hello konkrétní jazyk, který protokoly komentáře problém Githubu.</span><span class="sxs-lookup"><span data-stu-id="45df1-245">See hello language-specific sample that logs GitHub issue comments.</span></span>

* [<span data-ttu-id="45df1-246">C#</span><span class="sxs-lookup"><span data-stu-id="45df1-246">C#</span></span>](#hooktriggercsharp)
* [<span data-ttu-id="45df1-247">F#</span><span class="sxs-lookup"><span data-stu-id="45df1-247">F#</span></span>](#hooktriggerfsharp)
* [<span data-ttu-id="45df1-248">Node.js</span><span class="sxs-lookup"><span data-stu-id="45df1-248">Node.js</span></span>](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a><span data-ttu-id="45df1-249">Ukázka Webhooku v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="45df1-249">Webhook sample in C#</span></span> #
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

### <a name="webhook-sample-in-f"></a><span data-ttu-id="45df1-250">Ukázka Webhooku v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="45df1-250">Webhook sample in F#</span></span> #
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

### <a name="webhook-sample-in-nodejs"></a><span data-ttu-id="45df1-251">Ukázka Webhooku v Node.JS</span><span class="sxs-lookup"><span data-stu-id="45df1-251">Webhook sample in Node.JS</span></span>
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a><span data-ttu-id="45df1-252">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45df1-252">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

