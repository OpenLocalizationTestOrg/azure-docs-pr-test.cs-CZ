---
title: "Akce pracovního postupu a aktivační události - Azure Logic Apps | Microsoft Docs"
description: 
services: logic-apps
author: MandiOhlinger
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/17/2016
ms.author: LADocs; mandia
ms.openlocfilehash: bd3f1d225b974ebde889738bb435825658d1e1e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a><span data-ttu-id="52299-102">Akce pracovního postupu a aktivačních událostí pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="52299-102">Workflow actions and triggers for Azure Logic Apps</span></span>

<span data-ttu-id="52299-103">Aplikace logiky obsahovat triggery a akce.</span><span class="sxs-lookup"><span data-stu-id="52299-103">Logic apps consist of triggers and actions.</span></span> <span data-ttu-id="52299-104">Existuje šest typů služby aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="52299-104">There are six types of triggers.</span></span> <span data-ttu-id="52299-105">Každý typ má jiné rozhraní a různé chování.</span><span class="sxs-lookup"><span data-stu-id="52299-105">Each type has different interface and different behavior.</span></span> <span data-ttu-id="52299-106">Taky se dozvíte další podrobnosti pohledem na podrobnosti [jazyk definic workflowů](logic-apps-workflow-definition-language.md).</span><span class="sxs-lookup"><span data-stu-id="52299-106">You can also learn about other details by looking at the details of the [Workflow Definition Language](logic-apps-workflow-definition-language.md).</span></span>  
  
<span data-ttu-id="52299-107">Přečtěte si další informace o aktivační události a akce a jak je může využít k vytváření aplikací logiky ke zlepšení vaší podnikové procesy a pracovních postupů.</span><span class="sxs-lookup"><span data-stu-id="52299-107">Read on to learn more about triggers and actions and how you might use them to build logic apps to improve your business processes and workflows.</span></span>  
  
### <a name="triggers"></a><span data-ttu-id="52299-108">Triggery</span><span class="sxs-lookup"><span data-stu-id="52299-108">Triggers</span></span>  

<span data-ttu-id="52299-109">Aktivační událost určuje volání, které můžete zahájit spuštění pracovního postupu aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="52299-109">A trigger specifies the calls that can initiate a run of your logic app workflow.</span></span> <span data-ttu-id="52299-110">Zde jsou dva různé způsoby, jak zahájit spuštění pracovního postupu:</span><span class="sxs-lookup"><span data-stu-id="52299-110">Here are the two different ways to initiate a run of your workflow:</span></span>  
  
-   <span data-ttu-id="52299-111">Aktivační událost dotazování</span><span class="sxs-lookup"><span data-stu-id="52299-111">A polling trigger</span></span>  

-   <span data-ttu-id="52299-112">Aktivační událost nabízené - voláním [rozhraní API REST služby pracovního postupu](https://docs.microsoft.com/rest/api/logic/workflows)</span><span class="sxs-lookup"><span data-stu-id="52299-112">A push trigger - by calling the [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows)</span></span>  
  
<span data-ttu-id="52299-113">Žádná z aktivačních událostí obsahovat tyto prvky nejvyšší úrovně:</span><span class="sxs-lookup"><span data-stu-id="52299-113">All triggers contain these top-level elements:</span></span>  
  
```json
"<name-of-the-trigger>" : {
    "type": "<type-of-trigger>",
    "inputs": { <settings-for-the-call> },
    "recurrence": {  
        "frequency": "Second|Minute|Hour|Week|Month|Year",
        "interval": "<recurrence interval in units of frequency>"
    },
    "conditions": [ <array-of-required-conditions > ],
    "splitOn" : "<property to create runs for>",
    "operationOptions": "<operation options on the trigger>"
}
```

### <a name="trigger-types-and-their-inputs"></a><span data-ttu-id="52299-114">Typy aktivační události a jejich vstupy</span><span class="sxs-lookup"><span data-stu-id="52299-114">Trigger types and their inputs</span></span>  

<span data-ttu-id="52299-115">Můžete použít tyto typy triggerů:</span><span class="sxs-lookup"><span data-stu-id="52299-115">You can use these types of triggers:</span></span>
  
-   <span data-ttu-id="52299-116">**Žádosti o** \- díky aplikaci logiky koncový bod pro vás k volání</span><span class="sxs-lookup"><span data-stu-id="52299-116">**Request** \- Makes the logic app an endpoint for you to call</span></span>  
  
-   <span data-ttu-id="52299-117">**Opakování** \- aktivuje podle definovaného plánu</span><span class="sxs-lookup"><span data-stu-id="52299-117">**Recurrence** \- Fires based on a defined schedule</span></span>  
  
-   <span data-ttu-id="52299-118">**HTTP** \- dotazuje koncový bod webové HTTP.</span><span class="sxs-lookup"><span data-stu-id="52299-118">**HTTP** \- Polls an HTTP web endpoint.</span></span> <span data-ttu-id="52299-119">Koncový bod HTTP musí odpovídat kontraktu konkrétní spouštěcí \- buď pomocí 202\-asynchronní vzor nebo vrácením pole</span><span class="sxs-lookup"><span data-stu-id="52299-119">The HTTP endpoint must conform to a specific triggering contract \- either by using a 202\-async pattern, or by returning an array</span></span>  
  
-   <span data-ttu-id="52299-120">**ApiConnection** \- aktivovat hlasování jako HTTP, ale provádí se [Microsoft spravovaná rozhraní API](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="52299-120">**ApiConnection** \- Polls like the HTTP trigger, however, it takes advantage of the [Microsoft-managed APIs](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>  
  
-   <span data-ttu-id="52299-121">**HTTPWebhook** \- otevře na koncový bod, podobně jako na ruční aktivaci, ale také zavolá zadanou adresu URL pro registraci a zrušit registraci</span><span class="sxs-lookup"><span data-stu-id="52299-121">**HTTPWebhook** \- Opens an endpoint, similar to the Manual trigger, however, it also calls out to a specified URL to register and unregister</span></span>  
  
-   <span data-ttu-id="52299-122">**ApiConnectionWebhook** \- Operates jako HTTPWebhook aktivační událost pomocí rozhraní API spravovaný společností Microsoft</span><span class="sxs-lookup"><span data-stu-id="52299-122">**ApiConnectionWebhook** \- Operates like the HTTPWebhook trigger by taking advantage of the Microsoft-managed APIs</span></span>       
    <span data-ttu-id="52299-123">Každý typ aktivační událost má jinou sadu **vstupy** své chování, který definuje.</span><span class="sxs-lookup"><span data-stu-id="52299-123">Each trigger type has a different set of **inputs** that defines its behavior.</span></span>  
  
## <a name="request-trigger"></a><span data-ttu-id="52299-124">Žádost o aktivační události</span><span class="sxs-lookup"><span data-stu-id="52299-124">Request trigger</span></span>  

<span data-ttu-id="52299-125">Této aktivační události slouží jako koncový bod, který volání prostřednictvím požadavku HTTP k vyvolání svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="52299-125">This trigger serves as an endpoint that you call via an HTTP Request to invoke your logic app.</span></span> <span data-ttu-id="52299-126">Aktivační událost požadavku vypadá v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="52299-126">A request trigger looks like this example:</span></span>  
  
```json
"<name-of-the-trigger>" : {
    "type" : "request",
    "kind": "http",
    "inputs" : {
        "schema" : {
            "properties" : {
                "myInputProperty1" : { "type" : "string" },
                "myInputProperty2" : { "type" : "number" }
            },
        "required" : [ "myInputProperty1" ],
        "type" : "object"
        }
    }
} 
```

<span data-ttu-id="52299-127">Je také volitelná vlastnost s názvem **schématu**:</span><span class="sxs-lookup"><span data-stu-id="52299-127">There is also an optional property called **schema**:</span></span>  
  
|<span data-ttu-id="52299-128">Název elementu</span><span class="sxs-lookup"><span data-stu-id="52299-128">Element name</span></span>|<span data-ttu-id="52299-129">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-129">Required</span></span>|<span data-ttu-id="52299-130">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-130">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="52299-131">Schéma</span><span class="sxs-lookup"><span data-stu-id="52299-131">schema</span></span>|<span data-ttu-id="52299-132">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-132">No</span></span>|<span data-ttu-id="52299-133">Schéma JSON, který ověří příchozí žádost.</span><span class="sxs-lookup"><span data-stu-id="52299-133">A JSON schema that validates the incoming request.</span></span> <span data-ttu-id="52299-134">Užitečné pomáháte kroky následné pracovního postupu vědět, vlastnosti, které chcete odkazovat.</span><span class="sxs-lookup"><span data-stu-id="52299-134">Useful for helping subsequent workflow steps know which properties to reference.</span></span>|

<span data-ttu-id="52299-135">K vyvolání tento koncový bod, je třeba volat *listCallbackUrl* rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="52299-135">To invoke this endpoint, you need to call the *listCallbackUrl* API.</span></span> <span data-ttu-id="52299-136">V tématu [rozhraní API REST služby pracovního postupu](https://docs.microsoft.com/rest/api/logic/workflows).</span><span class="sxs-lookup"><span data-stu-id="52299-136">See [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows).</span></span>  
  
## <a name="recurrence-trigger"></a><span data-ttu-id="52299-137">Aktivační událost opakování</span><span class="sxs-lookup"><span data-stu-id="52299-137">Recurrence trigger</span></span>  

<span data-ttu-id="52299-138">Aktivační událost opakování je ten, který spouští na základě podle definovaného plánu.</span><span class="sxs-lookup"><span data-stu-id="52299-138">A Recurrence trigger is one that runs based on a defined schedule.</span></span> <span data-ttu-id="52299-139">V tomto příkladu může vypadat aktivační události:</span><span class="sxs-lookup"><span data-stu-id="52299-139">Such a trigger might look like this example:</span></span>  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

<span data-ttu-id="52299-140">Jak je vidět, je jednoduchý způsob, jak spustit workflow.</span><span class="sxs-lookup"><span data-stu-id="52299-140">As you can see, it is a simple way to run a workflow.</span></span>  
  
|<span data-ttu-id="52299-141">Název elementu</span><span class="sxs-lookup"><span data-stu-id="52299-141">Element name</span></span>|<span data-ttu-id="52299-142">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-142">Required</span></span>|<span data-ttu-id="52299-143">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-143">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="52299-144">frekvence</span><span class="sxs-lookup"><span data-stu-id="52299-144">frequency</span></span>|<span data-ttu-id="52299-145">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-145">Yes</span></span>|<span data-ttu-id="52299-146">Jak často se spustí aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="52299-146">How often the trigger executes.</span></span> <span data-ttu-id="52299-147">Použít pouze jednu z těchto možné hodnoty: sekundu, minutu, hodinu, den, týden, měsíc nebo rok</span><span class="sxs-lookup"><span data-stu-id="52299-147">Use only one of these possible values: second, minute, hour, day, week, month, or year</span></span>|  
|<span data-ttu-id="52299-148">Interval</span><span class="sxs-lookup"><span data-stu-id="52299-148">interval</span></span>|<span data-ttu-id="52299-149">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-149">Yes</span></span>|<span data-ttu-id="52299-150">Interval dané frekvenci opakování</span><span class="sxs-lookup"><span data-stu-id="52299-150">Interval of the given frequency for the recurrence</span></span>|  
|<span data-ttu-id="52299-151">startTime</span><span class="sxs-lookup"><span data-stu-id="52299-151">startTime</span></span>|<span data-ttu-id="52299-152">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-152">No</span></span>|<span data-ttu-id="52299-153">Pokud parametr startTime je poskytován bez časový posun, použije se toto časové pásmo.</span><span class="sxs-lookup"><span data-stu-id="52299-153">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
|<span data-ttu-id="52299-154">Časové pásmo</span><span class="sxs-lookup"><span data-stu-id="52299-154">timeZone</span></span>|<span data-ttu-id="52299-155">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-155">no</span></span>|<span data-ttu-id="52299-156">Pokud parametr startTime je poskytován bez časový posun, použije se toto časové pásmo.</span><span class="sxs-lookup"><span data-stu-id="52299-156">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
  
<span data-ttu-id="52299-157">Můžete také naplánovat aktivační události spustí provádění v určitém okamžiku v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="52299-157">You can also schedule a trigger to start executing at some point in the future.</span></span> <span data-ttu-id="52299-158">Například pokud chcete spustit sestavu týdenní každé pondělí můžete naplánovat aplikaci logiky začít každé pondělí tím vytváření následující aktivační události:</span><span class="sxs-lookup"><span data-stu-id="52299-158">For example, if you want to start a weekly report every Monday you can schedule the logic app to start every Monday by creating the following trigger:</span></span>  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": "1",
        "startTime" : "2015-06-22T00:00:00Z"
    }
}
```

## <a name="http-trigger"></a><span data-ttu-id="52299-159">Trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="52299-159">HTTP trigger</span></span>  

<span data-ttu-id="52299-160">Aktivace protokolu HTTP dotazování zadaný koncový bod a zkontrolujte odpověď na určení, zda by měl pracovní postup provádět.</span><span class="sxs-lookup"><span data-stu-id="52299-160">HTTP triggers poll a specified endpoint and check the response to determine whether the workflow should be executed.</span></span> <span data-ttu-id="52299-161">Objekt vstupy přijímá sadu parametrů požadovaných pro vytvoření volání protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="52299-161">The inputs object takes the set of parameters required to construct an HTTP call:</span></span>  
  
|<span data-ttu-id="52299-162">Název elementu</span><span class="sxs-lookup"><span data-stu-id="52299-162">Element name</span></span>|<span data-ttu-id="52299-163">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-163">Required</span></span>|<span data-ttu-id="52299-164">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-164">Description</span></span>|<span data-ttu-id="52299-165">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-165">Type</span></span>|  
|----------------|------------|---------------|--------|  
|<span data-ttu-id="52299-166">– Metoda</span><span class="sxs-lookup"><span data-stu-id="52299-166">method</span></span>|<span data-ttu-id="52299-167">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-167">yes</span></span>|<span data-ttu-id="52299-168">Může být jedna z následujících metod HTTP: GET, POST, PUT, DELETE, PATCH nebo HEAD</span><span class="sxs-lookup"><span data-stu-id="52299-168">Can be one of the following HTTP methods: GET, POST, PUT, DELETE, PATCH, or HEAD</span></span>|<span data-ttu-id="52299-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-169">String</span></span>|  
|<span data-ttu-id="52299-170">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="52299-170">uri</span></span>|<span data-ttu-id="52299-171">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-171">yes</span></span>|<span data-ttu-id="52299-172">Protokolu http nebo https koncový bod, který se označuje jako.</span><span class="sxs-lookup"><span data-stu-id="52299-172">The http or https endpoint that is called.</span></span> <span data-ttu-id="52299-173">Maximální počet kilobajtů 2.</span><span class="sxs-lookup"><span data-stu-id="52299-173">Maximum of 2 kilobytes.</span></span>|<span data-ttu-id="52299-174">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-174">String</span></span>|  
|<span data-ttu-id="52299-175">Dotazy</span><span class="sxs-lookup"><span data-stu-id="52299-175">queries</span></span>|<span data-ttu-id="52299-176">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-176">No</span></span>|<span data-ttu-id="52299-177">Objekt reprezentující parametry dotazu, které chcete přidat na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-177">An object representing the query parameters to add to the URL.</span></span> <span data-ttu-id="52299-178">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-178">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|<span data-ttu-id="52299-179">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-179">Object</span></span>|  
|<span data-ttu-id="52299-180">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="52299-180">headers</span></span>|<span data-ttu-id="52299-181">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-181">No</span></span>|<span data-ttu-id="52299-182">Objekt, který reprezentuje všechny hlavičky, které posílá požadavek.</span><span class="sxs-lookup"><span data-stu-id="52299-182">An object representing each of the headers that is sent to the request.</span></span> <span data-ttu-id="52299-183">Chcete-li například nastavení jazyka a typu na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="52299-183">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|<span data-ttu-id="52299-184">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-184">Object</span></span>|  
|<span data-ttu-id="52299-185">Text</span><span class="sxs-lookup"><span data-stu-id="52299-185">body</span></span>|<span data-ttu-id="52299-186">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-186">No</span></span>|<span data-ttu-id="52299-187">Objekt představující datovou část, která je odeslána koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="52299-187">An object representing the payload that is sent to the endpoint.</span></span>|<span data-ttu-id="52299-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-188">Object</span></span>|  
|<span data-ttu-id="52299-189">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="52299-189">retryPolicy</span></span>|<span data-ttu-id="52299-190">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-190">No</span></span>|<span data-ttu-id="52299-191">Objekt, který umožňuje přizpůsobit chování opakování 4xx nebo 5xx chyby.</span><span class="sxs-lookup"><span data-stu-id="52299-191">An object that lets you customize the retry behavior for 4xx or 5xx errors.</span></span>|<span data-ttu-id="52299-192">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-192">Object</span></span>|  
|<span data-ttu-id="52299-193">Ověřování</span><span class="sxs-lookup"><span data-stu-id="52299-193">authentication</span></span>|<span data-ttu-id="52299-194">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-194">No</span></span>|<span data-ttu-id="52299-195">Představuje metodu, žádost by měl ověřit.</span><span class="sxs-lookup"><span data-stu-id="52299-195">Represents the method that the request should be authenticated.</span></span> <span data-ttu-id="52299-196">Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="52299-196">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="52299-197">Nad scheduler, existuje více podporované jednu vlastnost: `authority` ve výchozím nastavení, tato hodnota je `https://login.windows.net` není-li zadána, ale můžete použít různé cílové skupiny jako`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="52299-197">Beyond scheduler, there is one more supported property: `authority` By default, this value is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|<span data-ttu-id="52299-198">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-198">Object</span></span>|  
  
<span data-ttu-id="52299-199">Aktivace protokolu HTTP vyžaduje rozhraní API HTTP tak, aby odpovídala vyhovujících určitému vzoru do funkce fungují dobře u aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="52299-199">The HTTP trigger requires the HTTP API to conform with a specific pattern to work well with your logic app.</span></span> <span data-ttu-id="52299-200">Vyžaduje následující pole:</span><span class="sxs-lookup"><span data-stu-id="52299-200">It requires the following fields:</span></span>  
  
|<span data-ttu-id="52299-201">Odpověď</span><span class="sxs-lookup"><span data-stu-id="52299-201">Response</span></span>|<span data-ttu-id="52299-202">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-202">Description</span></span>|  
|------------|---------------|  
|<span data-ttu-id="52299-203">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="52299-203">Status code</span></span>|<span data-ttu-id="52299-204">Stavovým kódem 200 \(OK\) způsobí spuštění.</span><span class="sxs-lookup"><span data-stu-id="52299-204">Status code 200 \(OK\) to cause a run.</span></span> <span data-ttu-id="52299-205">Další kód stavu nezpůsobí spustit.</span><span class="sxs-lookup"><span data-stu-id="52299-205">Any other status code doesn't cause a run.</span></span>|  
|<span data-ttu-id="52299-206">Opakujte\-po záhlaví</span><span class="sxs-lookup"><span data-stu-id="52299-206">Retry\-after header</span></span>|<span data-ttu-id="52299-207">Počet sekund do aplikace logiky dotazuje koncový bod znovu.</span><span class="sxs-lookup"><span data-stu-id="52299-207">Number of seconds until the logic app polls the endpoint again.</span></span>|  
|<span data-ttu-id="52299-208">Hlavička umístění</span><span class="sxs-lookup"><span data-stu-id="52299-208">Location header</span></span>|<span data-ttu-id="52299-209">Adresa URL pro volání na další interval dotazování.</span><span class="sxs-lookup"><span data-stu-id="52299-209">The URL to call on the next polling interval.</span></span> <span data-ttu-id="52299-210">Pokud není zadaný, použije se původní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-210">If not specified, the original URL is used.</span></span>|  
  
<span data-ttu-id="52299-211">Zde je několik příkladů různých chování pro různé typy požadavků:</span><span class="sxs-lookup"><span data-stu-id="52299-211">Here are some examples of different behaviors for different types of requests:</span></span>  
  
|<span data-ttu-id="52299-212">Kód odpovědi</span><span class="sxs-lookup"><span data-stu-id="52299-212">Response code</span></span>|<span data-ttu-id="52299-213">Opakujte\-po</span><span class="sxs-lookup"><span data-stu-id="52299-213">Retry\-After</span></span>|<span data-ttu-id="52299-214">Chování</span><span class="sxs-lookup"><span data-stu-id="52299-214">Behavior</span></span>|  
|-----------------|----------------|------------|  
|<span data-ttu-id="52299-215">200</span><span class="sxs-lookup"><span data-stu-id="52299-215">200</span></span>|<span data-ttu-id="52299-216">\(None\)</span><span class="sxs-lookup"><span data-stu-id="52299-216">\(none\)</span></span>|<span data-ttu-id="52299-217">Není platný aktivační událost, opakování\-poté, co je to požadováno, nebo jinak modul nikdy dotazuje pro další požadavek.</span><span class="sxs-lookup"><span data-stu-id="52299-217">Not a valid trigger, Retry\-After is required, or else the engine never polls for the next request.</span></span>|  
|<span data-ttu-id="52299-218">202</span><span class="sxs-lookup"><span data-stu-id="52299-218">202</span></span>|<span data-ttu-id="52299-219">60</span><span class="sxs-lookup"><span data-stu-id="52299-219">60</span></span>|<span data-ttu-id="52299-220">Nespouštějí pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="52299-220">Do not trigger the workflow.</span></span> <span data-ttu-id="52299-221">Další pokus se stane za jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="52299-221">The next attempt happens in one minute.</span></span>|  
|<span data-ttu-id="52299-222">200</span><span class="sxs-lookup"><span data-stu-id="52299-222">200</span></span>|<span data-ttu-id="52299-223">10</span><span class="sxs-lookup"><span data-stu-id="52299-223">10</span></span>|<span data-ttu-id="52299-224">Spuštění pracovního postupu a znovu zkontrolujte pro další obsah za 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="52299-224">Run the workflow, and check again for more content in 10 seconds.</span></span>|  
|<span data-ttu-id="52299-225">400</span><span class="sxs-lookup"><span data-stu-id="52299-225">400</span></span>|<span data-ttu-id="52299-226">\(None\)</span><span class="sxs-lookup"><span data-stu-id="52299-226">\(none\)</span></span>|<span data-ttu-id="52299-227">Chybný požadavek, nespouštějte pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="52299-227">Bad request, do not run the workflow.</span></span> <span data-ttu-id="52299-228">Pokud není žádná **opakujte zásad** definován, je použita výchozí zásady.</span><span class="sxs-lookup"><span data-stu-id="52299-228">If there is no **Retry Policy** defined, then the default policy is used.</span></span> <span data-ttu-id="52299-229">Po překročení počet opakovaných pokusů, aktivační událost již není platný.</span><span class="sxs-lookup"><span data-stu-id="52299-229">After the number of retries has been reached, the trigger is no longer valid.</span></span>|  
|<span data-ttu-id="52299-230">500</span><span class="sxs-lookup"><span data-stu-id="52299-230">500</span></span>|<span data-ttu-id="52299-231">\(None\)</span><span class="sxs-lookup"><span data-stu-id="52299-231">\(none\)</span></span>|<span data-ttu-id="52299-232">Chyba serveru, nespouštějte pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="52299-232">Server error, do not run the workflow.</span></span>  <span data-ttu-id="52299-233">Pokud není žádná **opakujte zásad** definován, je použita výchozí zásady.</span><span class="sxs-lookup"><span data-stu-id="52299-233">If there is no **Retry Policy** defined, then the default policy is used.</span></span> <span data-ttu-id="52299-234">Po překročení počet opakovaných pokusů, aktivační událost již není platný.</span><span class="sxs-lookup"><span data-stu-id="52299-234">After the number of retries has been reached, the trigger is no longer valid.</span></span>|  
  
<span data-ttu-id="52299-235">Výstupy aktivační procedury HTTP vypadat podobně jako tento příklad:</span><span class="sxs-lookup"><span data-stu-id="52299-235">The outputs of an HTTP trigger look like this example:</span></span>  
  
|<span data-ttu-id="52299-236">Název elementu</span><span class="sxs-lookup"><span data-stu-id="52299-236">Element name</span></span>|<span data-ttu-id="52299-237">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-237">Description</span></span>|<span data-ttu-id="52299-238">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-238">Type</span></span>|  
|----------------|---------------|--------|  
|<span data-ttu-id="52299-239">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="52299-239">headers</span></span>|<span data-ttu-id="52299-240">Hlavičky http odpovědi.</span><span class="sxs-lookup"><span data-stu-id="52299-240">The headers of the http response.</span></span>|<span data-ttu-id="52299-241">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-241">Object</span></span>|  
|<span data-ttu-id="52299-242">Text</span><span class="sxs-lookup"><span data-stu-id="52299-242">body</span></span>|<span data-ttu-id="52299-243">Text odpovědi http.</span><span class="sxs-lookup"><span data-stu-id="52299-243">The body of the http response.</span></span>|<span data-ttu-id="52299-244">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-244">Object</span></span>|  
  
## <a name="api-connection-trigger"></a><span data-ttu-id="52299-245">Aktivační události připojení k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="52299-245">API Connection trigger</span></span>  

<span data-ttu-id="52299-246">Aktivační událost připojení rozhraní API je podobná triggeru protokolu HTTP v jeho základních funkcí.</span><span class="sxs-lookup"><span data-stu-id="52299-246">The API connection trigger is similar to the HTTP trigger in its basic functionality.</span></span> <span data-ttu-id="52299-247">Parametry pro identifikaci akce se ale liší.</span><span class="sxs-lookup"><span data-stu-id="52299-247">However, the parameters for identifying the action are different.</span></span> <span data-ttu-id="52299-248">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="52299-248">Here is an example:</span></span>  
  
```json
"dailyReport" : {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://myarticles.example.com/"
            },
        }
        "connection": {
            "name": "@parameters('$connections')['myconnection'].name"
        }
    },  
    "method": "POST",
    "body": {
        "category": "awesomest"
    }
}
```

|<span data-ttu-id="52299-249">Název elementu</span><span class="sxs-lookup"><span data-stu-id="52299-249">Element name</span></span>|<span data-ttu-id="52299-250">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-250">Required</span></span>|<span data-ttu-id="52299-251">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-251">Type</span></span>|<span data-ttu-id="52299-252">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-252">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="52299-253">hostitele</span><span class="sxs-lookup"><span data-stu-id="52299-253">host</span></span>|<span data-ttu-id="52299-254">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-254">Yes</span></span>||<span data-ttu-id="52299-255">ApiApp hostované brány a id.</span><span class="sxs-lookup"><span data-stu-id="52299-255">The ApiApp hosted gateway and id.</span></span>|  
|<span data-ttu-id="52299-256">– Metoda</span><span class="sxs-lookup"><span data-stu-id="52299-256">method</span></span>|<span data-ttu-id="52299-257">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-257">Yes</span></span>|<span data-ttu-id="52299-258">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-258">String</span></span>|<span data-ttu-id="52299-259">Může být jedna z následujících metod HTTP: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo ** HEAD**</span><span class="sxs-lookup"><span data-stu-id="52299-259">Can be one of the following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="52299-260">Dotazy</span><span class="sxs-lookup"><span data-stu-id="52299-260">queries</span></span>|<span data-ttu-id="52299-261">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-261">No</span></span>|<span data-ttu-id="52299-262">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-262">Object</span></span>|<span data-ttu-id="52299-263">Představuje parametry dotazu, který se má přidat k adrese URL.</span><span class="sxs-lookup"><span data-stu-id="52299-263">Represents the query parameters to be added to the URL.</span></span> <span data-ttu-id="52299-264">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-264">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="52299-265">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="52299-265">headers</span></span>|<span data-ttu-id="52299-266">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-266">No</span></span>|<span data-ttu-id="52299-267">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-267">Object</span></span>|<span data-ttu-id="52299-268">Představuje všechny hlavičky, které posílá požadavek.</span><span class="sxs-lookup"><span data-stu-id="52299-268">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="52299-269">Chcete-li například nastavení jazyka a typu na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="52299-269">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="52299-270">Text</span><span class="sxs-lookup"><span data-stu-id="52299-270">body</span></span>|<span data-ttu-id="52299-271">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-271">No</span></span>|<span data-ttu-id="52299-272">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-272">Object</span></span>|<span data-ttu-id="52299-273">Představuje datovou část, která je odeslána koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="52299-273">Represents the payload that is sent to the endpoint.</span></span>|  
|<span data-ttu-id="52299-274">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="52299-274">retryPolicy</span></span>|<span data-ttu-id="52299-275">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-275">No</span></span>|<span data-ttu-id="52299-276">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-276">Object</span></span>|<span data-ttu-id="52299-277">Umožňuje přizpůsobit chování opakování 4xx nebo 5xx chyby.</span><span class="sxs-lookup"><span data-stu-id="52299-277">Allows you to customize the retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="52299-278">Ověřování</span><span class="sxs-lookup"><span data-stu-id="52299-278">authentication</span></span>|<span data-ttu-id="52299-279">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-279">No</span></span>|<span data-ttu-id="52299-280">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-280">Object</span></span>|<span data-ttu-id="52299-281">Představuje metodu, žádost by měl ověřit.</span><span class="sxs-lookup"><span data-stu-id="52299-281">Represents the method that the request should be authenticated.</span></span> <span data-ttu-id="52299-282">Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span><span class="sxs-lookup"><span data-stu-id="52299-282">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span></span>|  
  
<span data-ttu-id="52299-283">Jsou vlastnosti pro hostitele:</span><span class="sxs-lookup"><span data-stu-id="52299-283">The properties for host are:</span></span>  
  
|<span data-ttu-id="52299-284">Název elementu</span><span class="sxs-lookup"><span data-stu-id="52299-284">Element name</span></span>|<span data-ttu-id="52299-285">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-285">Required</span></span>|<span data-ttu-id="52299-286">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-286">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="52299-287">rozhraní API runtimeUrl</span><span class="sxs-lookup"><span data-stu-id="52299-287">api runtimeUrl</span></span>|<span data-ttu-id="52299-288">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-288">Yes</span></span>|<span data-ttu-id="52299-289">Koncový bod spravované rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="52299-289">The endpoint of the managed API.</span></span>|  
|<span data-ttu-id="52299-290">Název připojení</span><span class="sxs-lookup"><span data-stu-id="52299-290">connection name</span></span>||<span data-ttu-id="52299-291">Musí být odkaz na parametr s názvem `$connection` a je název připojení spravované rozhraní API, které používá pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="52299-291">Must be a reference to a parameter called `$connection` and is the name of the managed API connection that the workflow uses.</span></span>|
  
<span data-ttu-id="52299-292">Výstupy aktivační událost pro připojení rozhraní API jsou:</span><span class="sxs-lookup"><span data-stu-id="52299-292">The outputs of an API connection trigger are:</span></span>
  
|<span data-ttu-id="52299-293">Název elementu</span><span class="sxs-lookup"><span data-stu-id="52299-293">Element name</span></span>|<span data-ttu-id="52299-294">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-294">Type</span></span>|<span data-ttu-id="52299-295">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-295">Description</span></span>|  
|----------------|--------|---------------|  
|<span data-ttu-id="52299-296">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="52299-296">headers</span></span>|<span data-ttu-id="52299-297">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-297">Object</span></span>|<span data-ttu-id="52299-298">Hlavičky http odpovědi.</span><span class="sxs-lookup"><span data-stu-id="52299-298">The headers of the http response.</span></span>|  
|<span data-ttu-id="52299-299">Text</span><span class="sxs-lookup"><span data-stu-id="52299-299">body</span></span>|<span data-ttu-id="52299-300">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-300">Object</span></span>|<span data-ttu-id="52299-301">Text odpovědi http.</span><span class="sxs-lookup"><span data-stu-id="52299-301">The body of the http response.</span></span>|  
  
## <a name="httpwebhook-trigger"></a><span data-ttu-id="52299-302">Aktivační událost HTTPWebhook</span><span class="sxs-lookup"><span data-stu-id="52299-302">HTTPWebhook trigger</span></span>  

<span data-ttu-id="52299-303">Aktivační událost HTTPWebhook otevře koncový bod, podobně jako ruční aktivační událost, ale HTTPWebhook aktivační událost se taky volá zadanou adresu URL pro registraci a zrušit registraci.</span><span class="sxs-lookup"><span data-stu-id="52299-303">The HTTPWebhook trigger opens an endpoint, similar to the manual trigger, but the HTTPWebhook trigger also calls out to a specified URL to register and unregister.</span></span> <span data-ttu-id="52299-304">Tady je příklad, jak může vypadat aktivační procedury HTTPWebhook:</span><span class="sxs-lookup"><span data-stu-id="52299-304">Here's an example of what an HTTPWebhook trigger might look like:</span></span>  

```json
"myappspottrigger": {
    "type": "httpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "https://pubsubhubbub.appspot.com/subscribe",
            "headers": { },
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": { },
            "retryPolicy": { }
        },
        "unsubscribe": {
            "url": "https://pubsubhubbub.appspot.com/subscribe",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "method": "POST",
            "authentication": { }
        }
    },
    "conditions": [ ]
    }
```

<span data-ttu-id="52299-305">Mnoho z těchto částí je volitelné a chování Webhooku závisí na oddíly, které jsou zadané nebo tento parametr vynechán.</span><span class="sxs-lookup"><span data-stu-id="52299-305">Many of these sections are optional, and the behavior of the Webhook depends on which sections are provided or omitted.</span></span>  
<span data-ttu-id="52299-306">Webhook, jehož vlastnosti jsou následující:</span><span class="sxs-lookup"><span data-stu-id="52299-306">The properties of a Webhook are as follows:</span></span>  
  
|<span data-ttu-id="52299-307">Název elementu</span><span class="sxs-lookup"><span data-stu-id="52299-307">Element name</span></span>|<span data-ttu-id="52299-308">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-308">Required</span></span>|<span data-ttu-id="52299-309">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-309">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="52299-310">přihlášení k odběru</span><span class="sxs-lookup"><span data-stu-id="52299-310">subscribe</span></span>|<span data-ttu-id="52299-311">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-311">No</span></span>|<span data-ttu-id="52299-312">Odchozí požadavku, která je volána, když se vytvoří aktivační událost a provede počáteční registrace.</span><span class="sxs-lookup"><span data-stu-id="52299-312">The outgoing request that is called when the trigger is created and performs the initial registration.</span></span>|  
|<span data-ttu-id="52299-313">Odhlásit</span><span class="sxs-lookup"><span data-stu-id="52299-313">unsubscribe</span></span>|<span data-ttu-id="52299-314">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-314">No</span></span>|<span data-ttu-id="52299-315">Odchozí požadavek při odstranění aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="52299-315">The outgoing request when the trigger is deleted.</span></span>|  
  
-   <span data-ttu-id="52299-316">**Přihlášení k odběru** je odchozí hovoru, který provedl zahájit naslouchání na události.</span><span class="sxs-lookup"><span data-stu-id="52299-316">**Subscribe** is the outgoing call that's made to start listening to events.</span></span> <span data-ttu-id="52299-317">Toto volání začíná stejnou sadu parametrů, které provádějí normální akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="52299-317">This call starts with the same set of parameters that the normal HTTP actions do.</span></span> <span data-ttu-id="52299-318">Tato odchozí přišla žádný čas pracovní postup změny žádným způsobem, například vždy, když přihlašovací údaje jsou vráceny, nebo změňte vstupní parametry aktivační události.</span><span class="sxs-lookup"><span data-stu-id="52299-318">This outgoing call is made any time the workflow changes in any way, for example, whenever the credentials are rolled, or the trigger's input parameters change.</span></span>
  
    <span data-ttu-id="52299-319">Pro podporu toto volání, je nová funkce: `@listCallbackUrl()`.</span><span class="sxs-lookup"><span data-stu-id="52299-319">To support this call, there is a new function: `@listCallbackUrl()`.</span></span> <span data-ttu-id="52299-320">Tato funkce vrátí jedinečnou adresu URL pro tuto konkrétní aktivační událost v tomto pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="52299-320">This function returns a unique URL for this specific trigger in this workflow.</span></span> <span data-ttu-id="52299-321">Představuje jedinečný identifikátor pro koncové body, které používají služby REST.</span><span class="sxs-lookup"><span data-stu-id="52299-321">It represents the unique identifier for the endpoints that use the Service REST.</span></span>  
  
-   <span data-ttu-id="52299-322">**Odhlášení** je volána, když operace vykreslí této aktivační události neplatná, včetně:</span><span class="sxs-lookup"><span data-stu-id="52299-322">**Unsubscribe** is called when an operation renders this trigger invalid, including:</span></span>  
  
    -   <span data-ttu-id="52299-323">Odstranit nebo zakázat aktivační události</span><span class="sxs-lookup"><span data-stu-id="52299-323">Deleting or disabling the trigger</span></span>  
  
    -   <span data-ttu-id="52299-324">Odstranit nebo zakázat pracovní postup</span><span class="sxs-lookup"><span data-stu-id="52299-324">Deleting or disabling the workflow</span></span>  
  
    -   <span data-ttu-id="52299-325">Odstranit nebo zakázat odběr</span><span class="sxs-lookup"><span data-stu-id="52299-325">Deleting or disabling the subscription</span></span>  
  
    <span data-ttu-id="52299-326">Aplikace logiky automaticky zavolá akci zrušení odběru.</span><span class="sxs-lookup"><span data-stu-id="52299-326">The logic app automatically calls the unsubscribe action.</span></span> <span data-ttu-id="52299-327">Parametry této funkce jsou stejné jako triggeru protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="52299-327">The parameters to this function are the same as the HTTP trigger.</span></span>  
  
    <span data-ttu-id="52299-328">Výstupy HTTPWebhook aktivační události jsou příchozí žádost o:</span><span class="sxs-lookup"><span data-stu-id="52299-328">The outputs of the HTTPWebhook trigger are the contents of the incoming request:</span></span>  
  
|<span data-ttu-id="52299-329">Název elementu</span><span class="sxs-lookup"><span data-stu-id="52299-329">Element name</span></span>|<span data-ttu-id="52299-330">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-330">Type</span></span>|<span data-ttu-id="52299-331">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-331">Description</span></span>|  
|-----------------|--------|---------------|  
|<span data-ttu-id="52299-332">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="52299-332">headers</span></span>|<span data-ttu-id="52299-333">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-333">Object</span></span>|<span data-ttu-id="52299-334">Hlavičky požadavku http.</span><span class="sxs-lookup"><span data-stu-id="52299-334">The headers of the http request.</span></span>|  
|<span data-ttu-id="52299-335">Text</span><span class="sxs-lookup"><span data-stu-id="52299-335">body</span></span>|<span data-ttu-id="52299-336">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-336">Object</span></span>|<span data-ttu-id="52299-337">Text žádosti http.</span><span class="sxs-lookup"><span data-stu-id="52299-337">The body of the http request.</span></span>|  

<span data-ttu-id="52299-338">Omezení pro akci webhooku lze zadat v stejným způsobem jako [HTTP asynchronní omezení](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="52299-338">Limits on a webhook action can be specified in the same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  

## <a name="conditions"></a><span data-ttu-id="52299-339">Podmínky</span><span class="sxs-lookup"><span data-stu-id="52299-339">Conditions</span></span>  

<span data-ttu-id="52299-340">Pro všechny aktivační události můžete jednu nebo víc podmínek k určení, zda se má pracovní postup spustit nebo ne.</span><span class="sxs-lookup"><span data-stu-id="52299-340">For any trigger, you can use one or more conditions to determine whether the workflow should run or not.</span></span> <span data-ttu-id="52299-341">Například:</span><span class="sxs-lookup"><span data-stu-id="52299-341">For example:</span></span>  

```json
"dailyReport" : {
    "type": "recurrence",
    "conditions": [ {
        "expression": "@parameters('sendReports')"
    } ],
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

<span data-ttu-id="52299-342">V takovém případě aktivuje pouze sestavy při pracovního postupu `sendReports` parametr je nastaven na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="52299-342">In this case, the report only triggers while the workflow's `sendReports` parameter is set to true.</span></span> <span data-ttu-id="52299-343">Nakonec může odkazovat na podmínky stavový kód aktivační události.</span><span class="sxs-lookup"><span data-stu-id="52299-343">Finally, conditions may reference the status code of the trigger.</span></span> <span data-ttu-id="52299-344">Například může ji pracovního postupu jenom v případě, že váš web vrátí stavový kód 500, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="52299-344">For example, you could kick off a workflow only when your website returns a status code 500, as follows:</span></span>
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> <span data-ttu-id="52299-345">Když jakýkoli výraz odkazuje na kód stavu aktivační události \(žádným způsobem\), použije se výchozí chování \(aktivační událost pouze na 200 \(OK\) \) je nahrazena.</span><span class="sxs-lookup"><span data-stu-id="52299-345">When any expression references the status code of the trigger \(in any way\), the default behavior \(trigger only on 200 \(OK\)\) is replaced.</span></span> <span data-ttu-id="52299-346">Například pokud chcete aktivovat na stavovým kódem 200 a stavový kód 201, je nutné zahrnout: `@or(equals(triggers().code, 200),equals(triggers().code,201))` jako vaše podmínky.</span><span class="sxs-lookup"><span data-stu-id="52299-346">For example, if you want to trigger on both status code 200 and status code 201, you have to include: `@or(equals(triggers().code, 200),equals(triggers().code,201))` as your condition.</span></span>  
  
## <a name="start-multiple-runs-for-a-request"></a><span data-ttu-id="52299-347">Spuštění více spuštění pro žádost</span><span class="sxs-lookup"><span data-stu-id="52299-347">Start multiple runs for a request</span></span>

<span data-ttu-id="52299-348">Chcete-li ji více spuštění pro jeden požadavek, `splitOn` je užitečné, například, když chcete dotazování koncový bod, který může mít několik nových položek mezi intervaly cyklického dotazování.</span><span class="sxs-lookup"><span data-stu-id="52299-348">To kick off multiple runs for a single request, `splitOn` is useful, for example, when you want to poll an endpoint that can have multiple new items between polling intervals.</span></span>
  
<span data-ttu-id="52299-349">S `splitOn`, určete vlastnost do datové části odpovědi, která obsahuje pole položek, každý z nich chcete použít ke spuštění spuštění aktivační události.</span><span class="sxs-lookup"><span data-stu-id="52299-349">With `splitOn`, you specify the property inside the response payload that contains the array of items, each of which you want to use to start a run of the trigger.</span></span> <span data-ttu-id="52299-350">Představte si například, že máte rozhraní API, které vrátí následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="52299-350">For example, imagine you have an API that returns the following response:</span></span>  
  
```json
{
    "Status" : "success",
    "Rows" : [
        {  
            "id" : 938109380,
            "name" : "mycoolrow"
        },
        {
            "id" : 938109381,
            "name" : "another row"
        }
    ]
}
```
  
<span data-ttu-id="52299-351">Aplikace logiky pouze potřebuje obsah řádků, takže můžete vytvořit aktivační událost jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="52299-351">Your logic app only needs the Rows content, so you can construct your trigger like this example:</span></span>  
  
```json
"mysplitter" : {
    "type" : "http",
    "recurrence": {
        "frequency": "Minute",
        "interval": "1"
    },
    "intputs" : {
        "uri" : "https://mydomain.com/myAPI",
        "method" : "GET"
    },
    "splitOn" : "@triggerBody()?.Rows"
}
```
  
<span data-ttu-id="52299-352">Potom v definici pracovního postupu `@triggerBody().name` vrátí `mycoolrow` pro první spuštění a `another row` pro druhý spustit.</span><span class="sxs-lookup"><span data-stu-id="52299-352">Then, in the workflow definition, `@triggerBody().name` returns `mycoolrow` for the first run, and `another row` for the second run.</span></span> <span data-ttu-id="52299-353">Vzhled výstupy aktivační událost následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="52299-353">The trigger outputs look like this example:</span></span>  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

<span data-ttu-id="52299-354">Pokud používáte `SplitOn`, nelze získat vlastnosti, které jsou mimo pole, v takovém případě `Status` pole.</span><span class="sxs-lookup"><span data-stu-id="52299-354">So if you use `SplitOn`, you can't get the properties that are outside the array, in this case, the `Status` field.</span></span>  
  
> [!NOTE]  
> <span data-ttu-id="52299-355">V tomto příkladu používáme `?` operátor moct nedošlo k selhání, pokud `Rows` vlastnost není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="52299-355">In this example, we use the `?` operator to be able to avoid a failure if the `Rows` property is not present.</span></span> 
  
## <a name="single-run-instance"></a><span data-ttu-id="52299-356">Spuštění jedné instance</span><span class="sxs-lookup"><span data-stu-id="52299-356">Single run instance</span></span>

<span data-ttu-id="52299-357">Můžete nakonfigurovat aktivační události, které mají vlastnost opakování má pouze provést, pokud byly dokončeny všechny aktivní spustí.</span><span class="sxs-lookup"><span data-stu-id="52299-357">You can configure triggers that have a recurrence property to only fire if all active runs have completed.</span></span> <span data-ttu-id="52299-358">Pokud naplánované opakování dojde při spuštění s průběhem, aktivační událost přeskočí a čeká, až další interval opakování naplánované zkontrolujte znovu.</span><span class="sxs-lookup"><span data-stu-id="52299-358">If a scheduled recurrence occurs while there is an in-progress run, the trigger skips and waits until the next scheduled recurrence interval to check again.</span></span>

<span data-ttu-id="52299-359">Můžete nakonfigurovat toto nastavení prostřednictvím možnosti operace:</span><span class="sxs-lookup"><span data-stu-id="52299-359">You can configure this setting through the operation options:</span></span>

```json
"triggers": {
    "mytrigger": {
        "type": "http",
        "inputs": { ... },
        "recurrence": { ... },
        "operationOptions": "singleInstance"
    }
}
```

## <a name="types-and-inputs"></a><span data-ttu-id="52299-360">Typy a vstupy</span><span class="sxs-lookup"><span data-stu-id="52299-360">Types and inputs</span></span>  

<span data-ttu-id="52299-361">Existuje mnoho typů akcí, každý s jedinečný chování.</span><span class="sxs-lookup"><span data-stu-id="52299-361">There are many types of actions, each with unique behavior.</span></span> <span data-ttu-id="52299-362">Akce kolekce může obsahovat mnoho dalších akcí v rámci samotného.</span><span class="sxs-lookup"><span data-stu-id="52299-362">Collection actions may contain many other actions within itself.</span></span>

### <a name="standard-actions"></a><span data-ttu-id="52299-363">Standardní akce</span><span class="sxs-lookup"><span data-stu-id="52299-363">Standard actions</span></span>  

-   <span data-ttu-id="52299-364">**HTTP** tato akce zavolá koncový bod webové HTTP.</span><span class="sxs-lookup"><span data-stu-id="52299-364">**HTTP** This action calls an HTTP web endpoint.</span></span>  
  
-   <span data-ttu-id="52299-365">**ApiConnection** \- tato akce se chová jako akce HTTP, ale používá rozhraní API spravovaný společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="52299-365">**ApiConnection** \- This action behaves like the HTTP action, but uses the Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="52299-366">**ApiConnectionWebhook** \- jako HTTPWebhook, ale používá rozhraní API spravovaný společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="52299-366">**ApiConnectionWebhook** \- Like HTTPWebhook, but uses the Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="52299-367">**Odpověď** \- tato akce definuje odpovědi pro příchozí volání.</span><span class="sxs-lookup"><span data-stu-id="52299-367">**Response** \- This action defines a response for an incoming call.</span></span>  
  
-   <span data-ttu-id="52299-368">**Počkejte** \- tím jednoduché čeká pevnou hodnotu čas, nebo dokud určitý čas.</span><span class="sxs-lookup"><span data-stu-id="52299-368">**Wait** \- This simple action waits a fixed amount of time or until a specific time.</span></span>  
  
-   <span data-ttu-id="52299-369">**Pracovní postup** \- tato akce představuje vnořený pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="52299-369">**Workflow** \- This action represents a nested workflow.</span></span>  

-   <span data-ttu-id="52299-370">**Funkce** \- tato akce představuje funkci Azure.</span><span class="sxs-lookup"><span data-stu-id="52299-370">**Function** \- This action represents an Azure Function.</span></span>

### <a name="collection-actions"></a><span data-ttu-id="52299-371">Kolekce akce</span><span class="sxs-lookup"><span data-stu-id="52299-371">Collection actions</span></span>

-   <span data-ttu-id="52299-372">**Obor** \- tato akce je logické seskupení dalších akcí.</span><span class="sxs-lookup"><span data-stu-id="52299-372">**Scope** \- This action is a logical grouping of other actions.</span></span>

-   <span data-ttu-id="52299-373">**Podmínka** \- tato akce vyhodnotí výraz a provede odpovídající firemní pobočky výsledek.</span><span class="sxs-lookup"><span data-stu-id="52299-373">**Condition** \- This action evaluates an expression and executes the corresponding result branch.</span></span>

-   <span data-ttu-id="52299-374">**ForEach** \- tato opakování akce iteruje v rámci pole a provede vnitřní akce pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="52299-374">**ForEach** \- This looping action iterates through an array and performs inner actions for each item.</span></span>

-   <span data-ttu-id="52299-375">**Dokud** \- tato opakování akce provede vnitřní akce, dokud podmínku výsledků na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="52299-375">**Until** \- This looping action executes inner actions until a condition results to true.</span></span>
  
<span data-ttu-id="52299-376">Každý typ akce má jinou sadu **vstupy** které definují chování akci.</span><span class="sxs-lookup"><span data-stu-id="52299-376">Each type of action has a different set of **inputs** that define an action's behavior.</span></span>  
  
## <a name="http-action"></a><span data-ttu-id="52299-377">Akce HTTP</span><span class="sxs-lookup"><span data-stu-id="52299-377">HTTP action</span></span>  

<span data-ttu-id="52299-378">Akce HTTP volání zadaný koncový bod a zkontrolujte odpovědi k určení, zda se má spustit pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="52299-378">HTTP actions call a specified endpoint and check the response to determine whether the workflow should run.</span></span> <span data-ttu-id="52299-379">**Vstupy** objekt trvá sadu parametrů požadovaných pro vytvoření volání protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="52299-379">The **inputs** object takes the set of parameters required to construct the HTTP call:</span></span>  
  
|<span data-ttu-id="52299-380">Název elementu</span><span class="sxs-lookup"><span data-stu-id="52299-380">Element name</span></span>|<span data-ttu-id="52299-381">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-381">Required</span></span>|<span data-ttu-id="52299-382">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-382">Type</span></span>|<span data-ttu-id="52299-383">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-383">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="52299-384">– Metoda</span><span class="sxs-lookup"><span data-stu-id="52299-384">method</span></span>|<span data-ttu-id="52299-385">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-385">Yes</span></span>|<span data-ttu-id="52299-386">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-386">String</span></span>|<span data-ttu-id="52299-387">Může být jedna z následujících metod HTTP: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo ** HEAD**</span><span class="sxs-lookup"><span data-stu-id="52299-387">Can be one of the following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="52299-388">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="52299-388">uri</span></span>|<span data-ttu-id="52299-389">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-389">Yes</span></span>|<span data-ttu-id="52299-390">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-390">String</span></span>|<span data-ttu-id="52299-391">Protokolu http nebo https koncový bod, který se označuje jako.</span><span class="sxs-lookup"><span data-stu-id="52299-391">The http or https endpoint that is called.</span></span> <span data-ttu-id="52299-392">Maximální délka je 2 kB.</span><span class="sxs-lookup"><span data-stu-id="52299-392">Maximum length is 2 kilobytes.</span></span>|  
|<span data-ttu-id="52299-393">Dotazy</span><span class="sxs-lookup"><span data-stu-id="52299-393">queries</span></span>|<span data-ttu-id="52299-394">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-394">No</span></span>|<span data-ttu-id="52299-395">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-395">Object</span></span>|<span data-ttu-id="52299-396">Představuje parametry dotazu, které chcete přidat na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-396">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="52299-397">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-397">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="52299-398">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="52299-398">headers</span></span>|<span data-ttu-id="52299-399">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-399">No</span></span>|<span data-ttu-id="52299-400">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-400">Object</span></span>|<span data-ttu-id="52299-401">Představuje všechny hlavičky, které posílá požadavek.</span><span class="sxs-lookup"><span data-stu-id="52299-401">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="52299-402">Chcete-li například nastavení jazyka a typu na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="52299-402">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="52299-403">Text</span><span class="sxs-lookup"><span data-stu-id="52299-403">body</span></span>|<span data-ttu-id="52299-404">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-404">No</span></span>|<span data-ttu-id="52299-405">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-405">Object</span></span>|<span data-ttu-id="52299-406">Představuje datovou část, která je odeslána koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="52299-406">Represents the payload that is sent to the endpoint.</span></span>|  
|<span data-ttu-id="52299-407">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="52299-407">retryPolicy</span></span>|<span data-ttu-id="52299-408">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-408">No</span></span>|<span data-ttu-id="52299-409">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-409">Object</span></span>|<span data-ttu-id="52299-410">Umožňuje přizpůsobit chování opakování 4xx nebo 5xx chyby.</span><span class="sxs-lookup"><span data-stu-id="52299-410">Lets you customize the retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="52299-411">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="52299-411">operationsOptions</span></span>|<span data-ttu-id="52299-412">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-412">No</span></span>|<span data-ttu-id="52299-413">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-413">String</span></span>|<span data-ttu-id="52299-414">Definuje sadu zvláštní chování potlačit.</span><span class="sxs-lookup"><span data-stu-id="52299-414">Defines the set of special behaviors to override.</span></span>|  
|<span data-ttu-id="52299-415">Ověřování</span><span class="sxs-lookup"><span data-stu-id="52299-415">authentication</span></span>|<span data-ttu-id="52299-416">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-416">No</span></span>|<span data-ttu-id="52299-417">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-417">Object</span></span>|<span data-ttu-id="52299-418">Představuje metodu, žádost by měl ověřit.</span><span class="sxs-lookup"><span data-stu-id="52299-418">Represents the method that the request should be authenticated.</span></span> <span data-ttu-id="52299-419">Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="52299-419">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="52299-420">Nad scheduler, existuje více podporované jednu vlastnost: `authority`.</span><span class="sxs-lookup"><span data-stu-id="52299-420">Beyond scheduler, there is one more supported property: `authority`.</span></span> <span data-ttu-id="52299-421">Ve výchozím nastavení je to `https://login.windows.net` není-li zadána, ale můžete použít různé cílové skupiny jako`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="52299-421">By default, this is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|  
  
<span data-ttu-id="52299-422">Akce HTTP \(a připojení k rozhraní API\) podporu akce opakujte zásady.</span><span class="sxs-lookup"><span data-stu-id="52299-422">HTTP actions \(and API Connection\) actions support retry policies.</span></span> <span data-ttu-id="52299-423">Zásady opakování platí pro náhodnými poruchami, vyznačují jako stavové kódy HTTP 408 429 a 5xx kromě všechny výjimky připojení.</span><span class="sxs-lookup"><span data-stu-id="52299-423">A retry policy applies to intermittent failures, characterized as HTTP status codes 408, 429, and 5xx, in addition to any connectivity exceptions.</span></span> <span data-ttu-id="52299-424">Tato zásada je popsán pomocí *retryPolicy* objekt definovaný jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="52299-424">This policy is described using the *retryPolicy* object defined as shown here:</span></span>
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
<span data-ttu-id="52299-425">Interval opakování je zadána ve formátu ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="52299-425">The retry interval is specified in the ISO 8601 format.</span></span> <span data-ttu-id="52299-426">Tento interval má výchozí a minimální hodnotu 20 sekund, zatímco maximální hodnota je jedna hodina.</span><span class="sxs-lookup"><span data-stu-id="52299-426">This interval has a default and minimum value of 20 seconds, while the maximum value is one hour.</span></span> <span data-ttu-id="52299-427">Výchozí a maximální počet opakování je čtyři hodiny.</span><span class="sxs-lookup"><span data-stu-id="52299-427">The default and maximum retry count is four hours.</span></span> <span data-ttu-id="52299-428">Pokud není zadaná definice zásady opakování, `fixed` strategie se používá s výchozí hodnoty a interval opakování.</span><span class="sxs-lookup"><span data-stu-id="52299-428">If the retry policy definition is not specified, a `fixed` strategy is used with default retry count and interval values.</span></span> <span data-ttu-id="52299-429">Chcete-li zakázat zásady opakovaných pokusů, nastavte její typ na `None`.</span><span class="sxs-lookup"><span data-stu-id="52299-429">To disable the retry policy, set its type to `None`.</span></span>  
  
<span data-ttu-id="52299-430">Například následující akce opakované pokusy načítání nejnovější informace dvakrát, pokud existují náhodnými poruchami, celkem tři spuštěních s 30 sekund zpoždění mezi jednotlivými pokusy o:</span><span class="sxs-lookup"><span data-stu-id="52299-430">For example, the following action retries fetching the latest news two times, if there are intermittent failures, for a total of three executions, with a 30-second delay between each attempt:</span></span>  
  
```json
"latestNews" : {
    "type": "http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```
### <a name="asynchronous-patterns"></a><span data-ttu-id="52299-431">Asynchronními vzory</span><span class="sxs-lookup"><span data-stu-id="52299-431">Asynchronous patterns</span></span>

<span data-ttu-id="52299-432">Ve výchozím nastavení všechny akce založené na protokolu HTTP podporují vzor standardní asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="52299-432">By default, all HTTP-based actions support the standard asynchronous operation pattern.</span></span> <span data-ttu-id="52299-433">Pokud vzdálený server označuje, zda je žádost přijaté ke zpracování s 202 \(platných\) odpovědi, modul Logic Apps udržuje dotazování adresa URL zadaná v odpovědi na umístění záhlaví, dokud nebude dosaženo stavu terminálu \(bez\-202 odpovědi\).</span><span class="sxs-lookup"><span data-stu-id="52299-433">So if the remote server indicates that the request is accepted for processing with a 202 \(Accepted\) response, the Logic Apps engine keeps polling the URL specified in the response's location header until reaching a terminal state \(a non\-202 response\).</span></span>  
  
<span data-ttu-id="52299-434">Chcete-li zakázat asynchronní chování se popisuje výš, nastavte `DisableAsyncPattern` možnost v vstupy akce.</span><span class="sxs-lookup"><span data-stu-id="52299-434">To disable the asynchronous behavior previously described, set a `DisableAsyncPattern` option in the action inputs.</span></span> <span data-ttu-id="52299-435">V takovém případě výstup akce je založená na počáteční 202 odpověď ze serveru.</span><span class="sxs-lookup"><span data-stu-id="52299-435">In this case, the output of the action is based on the initial 202 response from the server.</span></span>  
  
```json
"invokeLongRunningOperation" : {
    "type": "http",
    "inputs": {
        "method": "POST",
        "uri": "https://host.example.com/resources"
    },
    "operationOptions": "DisableAsyncPattern"
}
```

#### <a name="asynchronous-limits"></a><span data-ttu-id="52299-436">Asynchronní omezení</span><span class="sxs-lookup"><span data-stu-id="52299-436">Asynchronous Limits</span></span>

<span data-ttu-id="52299-437">Asynchronní vzor může být omezena pouze doby trvání na určitém časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="52299-437">An asynchronous pattern can be limited in its duration to a specific time interval.</span></span>  <span data-ttu-id="52299-438">Pokud časový interval uplynutí bez dosažení stavu terminálu, budou označeny stav akce `Cancelled` s a kód `ActionTimedOut`.</span><span class="sxs-lookup"><span data-stu-id="52299-438">If the time interval elapses without reaching a terminal state, the status of the action will be marked `Cancelled` with a code of `ActionTimedOut`.</span></span>  <span data-ttu-id="52299-439">Časový limit omezení je zadána ve formátu ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="52299-439">The limit timeout is specified in ISO 8601 format.</span></span>  <span data-ttu-id="52299-440">Omezení lze zadat pomocí následující syntaxe:</span><span class="sxs-lookup"><span data-stu-id="52299-440">Limits can be specified with the following syntax:</span></span>

``` json
"<action-name>": {
    "type": "workflow|webhook|http|apiconnectionwebhook|apiconnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="api-connection"></a><span data-ttu-id="52299-441">Připojení k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="52299-441">API Connection</span></span>  

<span data-ttu-id="52299-442">Připojení k rozhraní API je akce, která odkazuje na konektor spravovaný společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="52299-442">API Connection is an action that references a Microsoft-managed connector.</span></span>
<span data-ttu-id="52299-443">Tato akce vyžaduje odkaz na platné připojení a informace o rozhraní API a požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="52299-443">This action requires a reference to a valid connection, and information on the API and parameters required.</span></span>

|<span data-ttu-id="52299-444">Název elementu</span><span class="sxs-lookup"><span data-stu-id="52299-444">Element name</span></span>|<span data-ttu-id="52299-445">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-445">Required</span></span>|<span data-ttu-id="52299-446">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-446">Type</span></span>|<span data-ttu-id="52299-447">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-447">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="52299-448">hostitele</span><span class="sxs-lookup"><span data-stu-id="52299-448">host</span></span>|<span data-ttu-id="52299-449">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-449">Yes</span></span>|<span data-ttu-id="52299-450">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-450">Object</span></span>|<span data-ttu-id="52299-451">Představuje informace o konektoru například runtimeUrl a odkaz na objekt připojení</span><span class="sxs-lookup"><span data-stu-id="52299-451">Represents the connector information such as the runtimeUrl and reference to the connection object</span></span>|
|<span data-ttu-id="52299-452">– Metoda</span><span class="sxs-lookup"><span data-stu-id="52299-452">method</span></span>|<span data-ttu-id="52299-453">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-453">Yes</span></span>|<span data-ttu-id="52299-454">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-454">String</span></span>|<span data-ttu-id="52299-455">Může být jedna z následujících metod HTTP: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo ** HEAD**</span><span class="sxs-lookup"><span data-stu-id="52299-455">Can be one of the following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="52299-456">Cesta</span><span class="sxs-lookup"><span data-stu-id="52299-456">path</span></span>|<span data-ttu-id="52299-457">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-457">Yes</span></span>|<span data-ttu-id="52299-458">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-458">String</span></span>|<span data-ttu-id="52299-459">Cesta operace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="52299-459">The path of the API operation.</span></span>|  
|<span data-ttu-id="52299-460">Dotazy</span><span class="sxs-lookup"><span data-stu-id="52299-460">queries</span></span>|<span data-ttu-id="52299-461">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-461">No</span></span>|<span data-ttu-id="52299-462">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-462">Object</span></span>|<span data-ttu-id="52299-463">Představuje parametry dotazu, které chcete přidat na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-463">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="52299-464">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-464">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="52299-465">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="52299-465">headers</span></span>|<span data-ttu-id="52299-466">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-466">No</span></span>|<span data-ttu-id="52299-467">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-467">Object</span></span>|<span data-ttu-id="52299-468">Představuje všechny hlavičky, které posílá požadavek.</span><span class="sxs-lookup"><span data-stu-id="52299-468">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="52299-469">Chcete-li například nastavení jazyka a typu na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="52299-469">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="52299-470">Text</span><span class="sxs-lookup"><span data-stu-id="52299-470">body</span></span>|<span data-ttu-id="52299-471">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-471">No</span></span>|<span data-ttu-id="52299-472">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-472">Object</span></span>|<span data-ttu-id="52299-473">Představuje datovou část, která je odeslána koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="52299-473">Represents the payload that is sent to the endpoint.</span></span>|  
|<span data-ttu-id="52299-474">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="52299-474">retryPolicy</span></span>|<span data-ttu-id="52299-475">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-475">No</span></span>|<span data-ttu-id="52299-476">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-476">Object</span></span>|<span data-ttu-id="52299-477">Umožňuje přizpůsobit chování opakování 4xx nebo 5xx chyby.</span><span class="sxs-lookup"><span data-stu-id="52299-477">Lets you customize the retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="52299-478">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="52299-478">operationsOptions</span></span>|<span data-ttu-id="52299-479">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-479">No</span></span>|<span data-ttu-id="52299-480">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-480">String</span></span>|<span data-ttu-id="52299-481">Definuje sadu zvláštní chování potlačit.</span><span class="sxs-lookup"><span data-stu-id="52299-481">Defines the set of special behaviors to override.</span></span>|  

```json
"Send_Email": {
    "type": "apiconnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "method": "post",
        "body": {
            "Subject": "New Tweet from @{triggerBody()['TweetedBy']}",
            "Body": "@{triggerBody()['TweetText']}",
            "To": "me@example.com"
        },
        "path": "/Mail"
    },
    "runAfter": {}
    }
```

## <a name="api-connection-webhook-action"></a><span data-ttu-id="52299-482">Akce webhooku připojení k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="52299-482">API Connection webhook action</span></span>

```json
"Send_approval_email": {
    "type": "apiconnectionwebhook",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "body": {
            "Message": {
                "Subject": "Approval Request",
                "Options": "Approve, Reject",
                "Importance": "Normal",
                "To": "me@email.com"
            }
        },
        "path": "/approvalmail",
        "authentication": "@parameters('$authentication')"
    },
    "runAfter": {}
}
```

<span data-ttu-id="52299-483">Omezení pro akci webhooku lze zadat v stejným způsobem jako [HTTP asynchronní omezení](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="52299-483">Limits on a webhook action can be specified in the same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  
## <a name="response-action"></a><span data-ttu-id="52299-484">Akce odpovědi</span><span class="sxs-lookup"><span data-stu-id="52299-484">Response action</span></span>  

<span data-ttu-id="52299-485">Tento typ akce obsahuje datové části celé odpovědi z požadavku HTTP a zahrnuje statusCode, text a hlavičky:</span><span class="sxs-lookup"><span data-stu-id="52299-485">This action type contains the entire response payload from an HTTP request and includes a statusCode, body, and headers:</span></span>  
  
```json
"myresponse" : {
    "type" : "response",
    "inputs" : {
        "statusCode" : 200,
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        }
    },
    "runAfter": {}
}
```
  
<span data-ttu-id="52299-486">Akce odpovědi obsahuje speciální omezení, která se nevztahují na dalších akcí.</span><span class="sxs-lookup"><span data-stu-id="52299-486">The response action has special restrictions that don't apply to other actions.</span></span> <span data-ttu-id="52299-487">Zejména:</span><span class="sxs-lookup"><span data-stu-id="52299-487">Specifically:</span></span>  
  
-   <span data-ttu-id="52299-488">Akce reagující nemůže být paralelní v definici, protože deterministickou odpovědi na příchozí požadavek je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="52299-488">Response actions cannot be parallel in a definition because a deterministic response to the incoming request is required.</span></span>  
  
-   <span data-ttu-id="52299-489">Pokud akce odpovědi je dostupný po obdržel odpověď příchozího požadavku, akce se považuje se nezdařilo \(konflikt\), a v důsledku toho je spustit `Failed`.</span><span class="sxs-lookup"><span data-stu-id="52299-489">If a response action is reached after the incoming request has received a response, the action is considered failed \(conflict\), and as a result, the run is `Failed`.</span></span>  
  
-   <span data-ttu-id="52299-490">Pracovní postup s akcemi odpověď nemůže mít `splitOn` v jeho aktivační událost protože jedno volání způsobí, že mnoho spustí.</span><span class="sxs-lookup"><span data-stu-id="52299-490">A workflow with Response actions cannot have `splitOn` in its trigger because one call causes many runs.</span></span> <span data-ttu-id="52299-491">V důsledku toho to možné ověřit, když toku PUT a příčina chybný požadavek.</span><span class="sxs-lookup"><span data-stu-id="52299-491">As a result, this should be validated when the flow is PUT and cause a Bad Request.</span></span>  
  
## <a name="wait-action"></a><span data-ttu-id="52299-492">Počkejte akce</span><span class="sxs-lookup"><span data-stu-id="52299-492">Wait action</span></span>  

<span data-ttu-id="52299-493">`wait` Akce pozastaví spuštění pracovního postupu po zadanou dobu.</span><span class="sxs-lookup"><span data-stu-id="52299-493">The `wait` action suspends workflow execution for the specified interval.</span></span> <span data-ttu-id="52299-494">Například 15 minut, než, můžete použít tento fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="52299-494">For example, to wait 15 minutes, you can use this snippet:</span></span>  
  
```json
"waitForFifteenMinutes" : {
    "type": "wait",
    "inputs": {
        "interval": {
            "unit" : "minute",
            "count" : 15
        }
    }
}
```  
  
<span data-ttu-id="52299-495">Alternativně počkejte chvilku konkrétní v čase, můžete v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="52299-495">Alternatively, to wait until a specific moment in time, you can use this example:</span></span>  
  
```json
"waitUntilOctober" : {
    "type": "wait",
    "inputs": {
        "until": {
            "timestamp" : "2016-10-01T00:00:00Z"
        }
    }
}
```
  
> [!NOTE]  
> <span data-ttu-id="52299-496">Doba čekání můžete buď zadat pomocí **interval** objekt nebo **dokud** objektu, ale ne obojí.</span><span class="sxs-lookup"><span data-stu-id="52299-496">The wait duration can be either specified using the **interval** object or the **until** object, but not both.</span></span>  
  
|<span data-ttu-id="52299-497">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52299-497">Name</span></span>|<span data-ttu-id="52299-498">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-498">Required</span></span>|<span data-ttu-id="52299-499">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-499">Type</span></span>|<span data-ttu-id="52299-500">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-500">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="52299-501">Interval</span><span class="sxs-lookup"><span data-stu-id="52299-501">interval</span></span>|<span data-ttu-id="52299-502">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-502">No</span></span>|<span data-ttu-id="52299-503">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-503">Object</span></span>|<span data-ttu-id="52299-504">Čekací doba trvání podle množství času.</span><span class="sxs-lookup"><span data-stu-id="52299-504">The wait duration based on amount of time.</span></span>|  
|<span data-ttu-id="52299-505">jednotku intervalu</span><span class="sxs-lookup"><span data-stu-id="52299-505">interval unit</span></span>|<span data-ttu-id="52299-506">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-506">Yes</span></span>|<span data-ttu-id="52299-507">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-507">String</span></span>|<span data-ttu-id="52299-508">Mezi těmito intervaly: sekundu, minutu, hodinu, den, týden, měsíc, rok.</span><span class="sxs-lookup"><span data-stu-id="52299-508">One of these intervals: second, minute, hour, day, week, month, year.</span></span>|  
|<span data-ttu-id="52299-509">počet intervalu</span><span class="sxs-lookup"><span data-stu-id="52299-509">interval count</span></span>|<span data-ttu-id="52299-510">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-510">Yes</span></span>|<span data-ttu-id="52299-511">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-511">String</span></span>|<span data-ttu-id="52299-512">Doba trvání na základě daného interní jednotky.</span><span class="sxs-lookup"><span data-stu-id="52299-512">Duration based on the given internal unit.</span></span>|  
|<span data-ttu-id="52299-513">dokud</span><span class="sxs-lookup"><span data-stu-id="52299-513">until</span></span>|<span data-ttu-id="52299-514">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-514">No</span></span>|<span data-ttu-id="52299-515">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-515">Object</span></span>|<span data-ttu-id="52299-516">Čekací doba trvání podle bod v čase.</span><span class="sxs-lookup"><span data-stu-id="52299-516">The wait duration based on a point in time.</span></span>|  
|<span data-ttu-id="52299-517">dokud časové razítko</span><span class="sxs-lookup"><span data-stu-id="52299-517">until timestamp</span></span>|<span data-ttu-id="52299-518">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-518">Yes</span></span>|<span data-ttu-id="52299-519">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-519">String</span></span>|<span data-ttu-id="52299-520">Řetězec & #124; Do bodu v čase ve standardu UTC, když vyprší platnost čekání.</span><span class="sxs-lookup"><span data-stu-id="52299-520">String&#124;The point in time in UTC when the wait expires.</span></span>|  

## <a name="query-action"></a><span data-ttu-id="52299-521">Akce dotazu</span><span class="sxs-lookup"><span data-stu-id="52299-521">Query action</span></span>

<span data-ttu-id="52299-522">`query` Akce umožňuje filtrovat pole na základě podmínky.</span><span class="sxs-lookup"><span data-stu-id="52299-522">The `query` action lets you filter an array based on a condition.</span></span> <span data-ttu-id="52299-523">Například pokud chcete vybrat čísla větší než 2, můžete použít:</span><span class="sxs-lookup"><span data-stu-id="52299-523">For example, to select numbers greater than 2, you can use:</span></span>

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

<span data-ttu-id="52299-524">Výstup `query` akce je pole, které má elementy ze vstupní pole, které splňují zadanou podmínku.</span><span class="sxs-lookup"><span data-stu-id="52299-524">The output from the `query` action is an array that has elements from the input array that satisfy the condition.</span></span>

> [!NOTE]
> <span data-ttu-id="52299-525">Pokud žádné hodnoty odpovídají `where` podmínka, výsledek je prázdné pole.</span><span class="sxs-lookup"><span data-stu-id="52299-525">If no values satisfy the `where` condition, the result is an empty array.</span></span>

|<span data-ttu-id="52299-526">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52299-526">Name</span></span>|<span data-ttu-id="52299-527">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-527">Required</span></span>|<span data-ttu-id="52299-528">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-528">Type</span></span>|<span data-ttu-id="52299-529">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-529">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="52299-530">Z</span><span class="sxs-lookup"><span data-stu-id="52299-530">from</span></span>|<span data-ttu-id="52299-531">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-531">Yes</span></span>|<span data-ttu-id="52299-532">Pole</span><span class="sxs-lookup"><span data-stu-id="52299-532">Array</span></span>|<span data-ttu-id="52299-533">Zdrojové pole.</span><span class="sxs-lookup"><span data-stu-id="52299-533">The source array.</span></span>|
|<span data-ttu-id="52299-534">kde</span><span class="sxs-lookup"><span data-stu-id="52299-534">where</span></span>|<span data-ttu-id="52299-535">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-535">Yes</span></span>|<span data-ttu-id="52299-536">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-536">String</span></span>|<span data-ttu-id="52299-537">Podmínku, která má použít pro každý prvek zdrojové pole.</span><span class="sxs-lookup"><span data-stu-id="52299-537">The condition to apply to each element of the source array.</span></span>|

## <a name="select-action"></a><span data-ttu-id="52299-538">Vyberte akci</span><span class="sxs-lookup"><span data-stu-id="52299-538">Select action</span></span>

<span data-ttu-id="52299-539">`select` Akce umožňuje projektu každý element pole na novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="52299-539">The `select` action lets you project each element of an array into a new value.</span></span>
<span data-ttu-id="52299-540">Například k převedení pole čísla do pole objektů, můžete použít:</span><span class="sxs-lookup"><span data-stu-id="52299-540">For example, to convert an array of numbers into an array of objects, you can use:</span></span>

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

<span data-ttu-id="52299-541">Výstup `select` akce je pole, které má stejné mohutnost jako vstupní pole s každý prvek transformovat podle definice `select` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="52299-541">The output of the `select` action is an array that has the same cardinality as the input array, with each element transformed as defined by the `select` property.</span></span> <span data-ttu-id="52299-542">Pokud vstup je prázdné pole, je výstup také prázdné pole.</span><span class="sxs-lookup"><span data-stu-id="52299-542">If the input is an empty array, the output is also an empty array.</span></span>

|<span data-ttu-id="52299-543">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52299-543">Name</span></span>|<span data-ttu-id="52299-544">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-544">Required</span></span>|<span data-ttu-id="52299-545">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-545">Type</span></span>|<span data-ttu-id="52299-546">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-546">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="52299-547">Z</span><span class="sxs-lookup"><span data-stu-id="52299-547">from</span></span>|<span data-ttu-id="52299-548">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-548">Yes</span></span>|<span data-ttu-id="52299-549">Pole</span><span class="sxs-lookup"><span data-stu-id="52299-549">Array</span></span>|<span data-ttu-id="52299-550">Zdrojové pole.</span><span class="sxs-lookup"><span data-stu-id="52299-550">The source array.</span></span>|
|<span data-ttu-id="52299-551">Vyberte</span><span class="sxs-lookup"><span data-stu-id="52299-551">select</span></span>|<span data-ttu-id="52299-552">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-552">Yes</span></span>|<span data-ttu-id="52299-553">Všechny</span><span class="sxs-lookup"><span data-stu-id="52299-553">Any</span></span>|<span data-ttu-id="52299-554">Projekce, které chcete použít pro každý prvek zdrojové pole.</span><span class="sxs-lookup"><span data-stu-id="52299-554">The projection to apply to each element of the source array.</span></span>|

## <a name="terminate-action"></a><span data-ttu-id="52299-555">Ukončit akce</span><span class="sxs-lookup"><span data-stu-id="52299-555">Terminate action</span></span>

<span data-ttu-id="52299-556">Akce ukončení zastaví provedení spuštění pracovního postupu, přerušení všechny během letu akce a přeskočení všechny zbývající akce.</span><span class="sxs-lookup"><span data-stu-id="52299-556">The Terminate action stops execution of the workflow run, aborting any in-flight actions, and skipping any remaining actions.</span></span> <span data-ttu-id="52299-557">Například pro ukončení spuštění se stavem **se nezdařilo**, můžete použít následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="52299-557">For example, to terminate a run with status **Failed**, you can use the following snippet:</span></span>

```json
"HandleUnexpectedResponse" : {
    "type": "terminate",
    "inputs": {
        "runStatus" : "failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response.",
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="52299-558">Ukončit akce nemá vliv akce již byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="52299-558">Actions already completed are not affected by the terminate action.</span></span>

|<span data-ttu-id="52299-559">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52299-559">Name</span></span>|<span data-ttu-id="52299-560">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-560">Required</span></span>|<span data-ttu-id="52299-561">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-561">Type</span></span>|<span data-ttu-id="52299-562">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-562">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="52299-563">runStatus</span><span class="sxs-lookup"><span data-stu-id="52299-563">runStatus</span></span>|<span data-ttu-id="52299-564">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-564">Yes</span></span>|<span data-ttu-id="52299-565">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-565">String</span></span>|<span data-ttu-id="52299-566">Cíl spustit stav.</span><span class="sxs-lookup"><span data-stu-id="52299-566">The target run status.</span></span> <span data-ttu-id="52299-567">Buď **se nezdařilo** nebo **zrušena**.</span><span class="sxs-lookup"><span data-stu-id="52299-567">Either **Failed** or **Cancelled**.</span></span>|
|<span data-ttu-id="52299-568">runError</span><span class="sxs-lookup"><span data-stu-id="52299-568">runError</span></span>|<span data-ttu-id="52299-569">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-569">No</span></span>|<span data-ttu-id="52299-570">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-570">Object</span></span>|<span data-ttu-id="52299-571">Podrobnosti o chybě.</span><span class="sxs-lookup"><span data-stu-id="52299-571">The error details.</span></span> <span data-ttu-id="52299-572">Když podporována pouze **runStatus** je nastaven na **se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="52299-572">Only supported when **runStatus** is set to **Failed**.</span></span>|
|<span data-ttu-id="52299-573">runError kódu</span><span class="sxs-lookup"><span data-stu-id="52299-573">runError code</span></span>|<span data-ttu-id="52299-574">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-574">No</span></span>|<span data-ttu-id="52299-575">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-575">String</span></span>|<span data-ttu-id="52299-576">Kód chyby spuštění.</span><span class="sxs-lookup"><span data-stu-id="52299-576">The run error code.</span></span>|
|<span data-ttu-id="52299-577">zpráva runError</span><span class="sxs-lookup"><span data-stu-id="52299-577">runError message</span></span>|<span data-ttu-id="52299-578">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-578">No</span></span>|<span data-ttu-id="52299-579">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-579">String</span></span>|<span data-ttu-id="52299-580">Spuštění chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="52299-580">The run error message.</span></span>|

## <a name="compose-action"></a><span data-ttu-id="52299-581">Vytvořit akce</span><span class="sxs-lookup"><span data-stu-id="52299-581">Compose action</span></span>

<span data-ttu-id="52299-582">Vytvářené akce umožňuje vytvořit libovolný objekt.</span><span class="sxs-lookup"><span data-stu-id="52299-582">The Compose action lets you construct an arbitrary object.</span></span> <span data-ttu-id="52299-583">Výstup akce compose je výsledkem vyhodnocení vstupy.</span><span class="sxs-lookup"><span data-stu-id="52299-583">The output of the compose action is the result of evaluating its inputs.</span></span> <span data-ttu-id="52299-584">Například můžete použít akci vytvářené sloučit výstupy několik akcí:</span><span class="sxs-lookup"><span data-stu-id="52299-584">For example, you can use the compose action to merge outputs of multiple actions:</span></span>

```json
"composeUserRecord" : {
    "type": "compose",
    "inputs": {
        "firstName": "@actions('getUser').firstName",
        "alias": "@actions('getUser').alias",
        "thumbnailLink": "@actions('lookupThumbnail').url"
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="52299-585">**Vytvářené** akci lze použít k vytvoření žádný výstup, včetně objektů, pole a jiný typ nativně podporuje logiku aplikace jako soubor XML a binární.</span><span class="sxs-lookup"><span data-stu-id="52299-585">The **Compose** action can be used to construct any output, including objects, arrays, and any other type natively supported by logic apps like XML and binary.</span></span>

## <a name="table-action"></a><span data-ttu-id="52299-586">Tabulka akcí</span><span class="sxs-lookup"><span data-stu-id="52299-586">Table action</span></span>

<span data-ttu-id="52299-587">`table` Umožňuje převod pole položek do **CSV** nebo **HTML** tabulky.</span><span class="sxs-lookup"><span data-stu-id="52299-587">The `table` allows you to convert an array of items into a **CSV** or **HTML** table.</span></span>

<span data-ttu-id="52299-588">Předpokládejme, že @triggerBodyje)</span><span class="sxs-lookup"><span data-stu-id="52299-588">Suppose @triggerBody() is</span></span>

```json
[{
  "id": 0,
  "name": "apples"
},{
  "id": 1, 
  "name": "oranges"
}]
```

<span data-ttu-id="52299-589">A mohli být definován jako akce</span><span class="sxs-lookup"><span data-stu-id="52299-589">And let the action be defined as</span></span>

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

<span data-ttu-id="52299-590">Vytvoří výše</span><span class="sxs-lookup"><span data-stu-id="52299-590">The above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="52299-591">id</span><span class="sxs-lookup"><span data-stu-id="52299-591">id</span></span></th><th><span data-ttu-id="52299-592">jméno</span><span class="sxs-lookup"><span data-stu-id="52299-592">name</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="52299-593">0</span><span class="sxs-lookup"><span data-stu-id="52299-593">0</span></span></td><td><span data-ttu-id="52299-594">jablka</span><span class="sxs-lookup"><span data-stu-id="52299-594">apples</span></span></td></tr><tr><td><span data-ttu-id="52299-595">1</span><span class="sxs-lookup"><span data-stu-id="52299-595">1</span></span></td><td><span data-ttu-id="52299-596">Pomeranče</span><span class="sxs-lookup"><span data-stu-id="52299-596">oranges</span></span></td></tr></tbody></table><span data-ttu-id="52299-597">"</span><span class="sxs-lookup"><span data-stu-id="52299-597">"</span></span>

<span data-ttu-id="52299-598">Chcete-li přizpůsobit v tabulce, je explicitně zadat sloupce.</span><span class="sxs-lookup"><span data-stu-id="52299-598">In order to customize the table, you can specify the columns explicitly.</span></span> <span data-ttu-id="52299-599">Například:</span><span class="sxs-lookup"><span data-stu-id="52299-599">For example:</span></span>

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html",
        "columns": [{
          "header": "produce id",
          "value": "@item().id"
        },{
          "header": "description",
          "value": "@concat('fresh ', item().name)"
        }]
    }
}
```

<span data-ttu-id="52299-600">Vytvoří výše</span><span class="sxs-lookup"><span data-stu-id="52299-600">The above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="52299-601">vytvoření id</span><span class="sxs-lookup"><span data-stu-id="52299-601">produce id</span></span></th><th><span data-ttu-id="52299-602">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-602">description</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="52299-603">0</span><span class="sxs-lookup"><span data-stu-id="52299-603">0</span></span></td><td><span data-ttu-id="52299-604">Čerstvá jablka</span><span class="sxs-lookup"><span data-stu-id="52299-604">fresh apples</span></span></td></tr><tr><td><span data-ttu-id="52299-605">1</span><span class="sxs-lookup"><span data-stu-id="52299-605">1</span></span></td><td><span data-ttu-id="52299-606">čerstvé pomeranče</span><span class="sxs-lookup"><span data-stu-id="52299-606">fresh oranges</span></span></td></tr></tbody></table><span data-ttu-id="52299-607">"</span><span class="sxs-lookup"><span data-stu-id="52299-607">"</span></span>

<span data-ttu-id="52299-608">Pokud `from` hodnota vlastnosti je prázdné pole, výstup je prázdná tabulka.</span><span class="sxs-lookup"><span data-stu-id="52299-608">If the `from` property value is an empty array, the output is an empty table.</span></span>

|<span data-ttu-id="52299-609">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52299-609">Name</span></span>|<span data-ttu-id="52299-610">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-610">Required</span></span>|<span data-ttu-id="52299-611">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-611">Type</span></span>|<span data-ttu-id="52299-612">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-612">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="52299-613">Z</span><span class="sxs-lookup"><span data-stu-id="52299-613">from</span></span>|<span data-ttu-id="52299-614">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-614">Yes</span></span>|<span data-ttu-id="52299-615">Pole</span><span class="sxs-lookup"><span data-stu-id="52299-615">Array</span></span>|<span data-ttu-id="52299-616">Zdrojové pole.</span><span class="sxs-lookup"><span data-stu-id="52299-616">The source array.</span></span>|
|<span data-ttu-id="52299-617">Formát</span><span class="sxs-lookup"><span data-stu-id="52299-617">format</span></span>|<span data-ttu-id="52299-618">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-618">Yes</span></span>|<span data-ttu-id="52299-619">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-619">String</span></span>|<span data-ttu-id="52299-620">Formát, buď **CSV** nebo **HTML**.</span><span class="sxs-lookup"><span data-stu-id="52299-620">The format, either **CSV** or **HTML**.</span></span>|
|<span data-ttu-id="52299-621">sloupce</span><span class="sxs-lookup"><span data-stu-id="52299-621">columns</span></span>|<span data-ttu-id="52299-622">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-622">No</span></span>|<span data-ttu-id="52299-623">Pole</span><span class="sxs-lookup"><span data-stu-id="52299-623">Array</span></span>|<span data-ttu-id="52299-624">Sloupce.</span><span class="sxs-lookup"><span data-stu-id="52299-624">The columns.</span></span> <span data-ttu-id="52299-625">Umožňuje přepsat výchozí tvar tabulky.</span><span class="sxs-lookup"><span data-stu-id="52299-625">Allows to override the default shape of the table.</span></span>|
|<span data-ttu-id="52299-626">záhlaví sloupce</span><span class="sxs-lookup"><span data-stu-id="52299-626">column header</span></span>|<span data-ttu-id="52299-627">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-627">No</span></span>|<span data-ttu-id="52299-628">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-628">String</span></span>|<span data-ttu-id="52299-629">Záhlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="52299-629">The header of the column.</span></span>|
|<span data-ttu-id="52299-630">Hodnota sloupce</span><span class="sxs-lookup"><span data-stu-id="52299-630">column value</span></span>|<span data-ttu-id="52299-631">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-631">Yes</span></span>|<span data-ttu-id="52299-632">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-632">String</span></span>|<span data-ttu-id="52299-633">Hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="52299-633">The value of the column.</span></span>|

## <a name="workflow-action"></a><span data-ttu-id="52299-634">Akce pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="52299-634">Workflow action</span></span>   

|<span data-ttu-id="52299-635">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52299-635">Name</span></span>|<span data-ttu-id="52299-636">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-636">Required</span></span>|<span data-ttu-id="52299-637">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-637">Type</span></span>|<span data-ttu-id="52299-638">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-638">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="52299-639">id hostitele</span><span class="sxs-lookup"><span data-stu-id="52299-639">host id</span></span>|<span data-ttu-id="52299-640">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-640">Yes</span></span>|<span data-ttu-id="52299-641">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-641">String</span></span>|<span data-ttu-id="52299-642">ID prostředku pracovního postupu, který chcete volat.</span><span class="sxs-lookup"><span data-stu-id="52299-642">The resource ID of the workflow that you want to call.</span></span>|  
|<span data-ttu-id="52299-643">Název aktivační události hostitele</span><span class="sxs-lookup"><span data-stu-id="52299-643">host triggerName</span></span>|<span data-ttu-id="52299-644">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-644">Yes</span></span>|<span data-ttu-id="52299-645">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-645">String</span></span>|<span data-ttu-id="52299-646">Název aktivační události, kterou chcete volat.</span><span class="sxs-lookup"><span data-stu-id="52299-646">The name of the trigger that you want to invoke.</span></span>|  
|<span data-ttu-id="52299-647">Dotazy</span><span class="sxs-lookup"><span data-stu-id="52299-647">queries</span></span>|<span data-ttu-id="52299-648">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-648">No</span></span>|<span data-ttu-id="52299-649">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-649">Object</span></span>|<span data-ttu-id="52299-650">Představuje parametry dotazu, které chcete přidat na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-650">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="52299-651">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-651">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="52299-652">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="52299-652">headers</span></span>|<span data-ttu-id="52299-653">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-653">No</span></span>|<span data-ttu-id="52299-654">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-654">Object</span></span>|<span data-ttu-id="52299-655">Představuje všechny hlavičky, které posílá požadavek.</span><span class="sxs-lookup"><span data-stu-id="52299-655">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="52299-656">Chcete-li například nastavení jazyka a typu na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="52299-656">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="52299-657">Text</span><span class="sxs-lookup"><span data-stu-id="52299-657">body</span></span>|<span data-ttu-id="52299-658">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-658">No</span></span>|<span data-ttu-id="52299-659">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-659">Object</span></span>|<span data-ttu-id="52299-660">Představuje datová část odeslaná ke koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="52299-660">Represents the payload sent to the endpoint.</span></span>|  
  
```json
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName " : "mytrigger001"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },  
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "runAfter": {}
    }
```
  
<span data-ttu-id="52299-661">Pro pracovní postup se provádí kontrolu přístupu \(přesněji řečeno, aktivační událost\), což znamená, potřebujete přístup do pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="52299-661">An access check is made on the workflow \(more specifically, the trigger\), meaning you need access to the workflow.</span></span>  
  
<span data-ttu-id="52299-662">Výstup z `workflow` akce jsou založené na definované v `response` akce v pracovním postupu podřízené.</span><span class="sxs-lookup"><span data-stu-id="52299-662">The outputs from the `workflow` action are based on what you defined in the `response` action in the child workflow.</span></span> <span data-ttu-id="52299-663">Pokud nebyly definovány žádné `response` akce a potom výstupy jsou prázdné.</span><span class="sxs-lookup"><span data-stu-id="52299-663">If you have not defined any `response` action, then the outputs are empty.</span></span>  

## <a name="function-action"></a><span data-ttu-id="52299-664">Akce – funkce</span><span class="sxs-lookup"><span data-stu-id="52299-664">Function action</span></span>   

|<span data-ttu-id="52299-665">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52299-665">Name</span></span>|<span data-ttu-id="52299-666">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-666">Required</span></span>|<span data-ttu-id="52299-667">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-667">Type</span></span>|<span data-ttu-id="52299-668">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-668">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="52299-669">id – funkce</span><span class="sxs-lookup"><span data-stu-id="52299-669">function id</span></span>|<span data-ttu-id="52299-670">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-670">Yes</span></span>|<span data-ttu-id="52299-671">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-671">String</span></span>|<span data-ttu-id="52299-672">ID prostředku funkce, kterou chcete volat.</span><span class="sxs-lookup"><span data-stu-id="52299-672">The resource ID of the function that you want to invoke.</span></span>|  
|<span data-ttu-id="52299-673">– Metoda</span><span class="sxs-lookup"><span data-stu-id="52299-673">method</span></span>|<span data-ttu-id="52299-674">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-674">No</span></span>|<span data-ttu-id="52299-675">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-675">String</span></span>|<span data-ttu-id="52299-676">Metodu protokolu HTTP použitou k vyvolání funkce.</span><span class="sxs-lookup"><span data-stu-id="52299-676">The HTTP method used to invoke the function.</span></span> <span data-ttu-id="52299-677">Ve výchozím nastavení, je `POST` není-li zadána.</span><span class="sxs-lookup"><span data-stu-id="52299-677">By default, it is `POST` when not specified.</span></span>|  
|<span data-ttu-id="52299-678">Dotazy</span><span class="sxs-lookup"><span data-stu-id="52299-678">queries</span></span>|<span data-ttu-id="52299-679">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-679">No</span></span>|<span data-ttu-id="52299-680">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-680">Object</span></span>|<span data-ttu-id="52299-681">Představuje parametry dotazu, které chcete přidat na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-681">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="52299-682">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52299-682">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="52299-683">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="52299-683">headers</span></span>|<span data-ttu-id="52299-684">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-684">No</span></span>|<span data-ttu-id="52299-685">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-685">Object</span></span>|<span data-ttu-id="52299-686">Představuje všechny hlavičky, které posílá požadavek.</span><span class="sxs-lookup"><span data-stu-id="52299-686">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="52299-687">Chcete-li například nastavit jazyk a typ na vyžádání: `"headers" : { "Accept-Language": "en-us" }`.</span><span class="sxs-lookup"><span data-stu-id="52299-687">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us" }`.</span></span>|  
|<span data-ttu-id="52299-688">Text</span><span class="sxs-lookup"><span data-stu-id="52299-688">body</span></span>|<span data-ttu-id="52299-689">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-689">No</span></span>|<span data-ttu-id="52299-690">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-690">Object</span></span>|<span data-ttu-id="52299-691">Představuje datová část odeslaná ke koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="52299-691">Represents the payload sent to the endpoint.</span></span>|  

```json
"myfunc" : {
    "type" : "Function",
    "inputs" : {
        "function" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Web/sites/myfuncapp/functions/myfunc"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },  
        "headers" : {
            "x-ms-date" : "@utcnow()"
        },
        "method" : "POST",
    "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "runAfter": {}
}
```

<span data-ttu-id="52299-692">Při ukládání aplikace logiky, můžeme provést nějaké kontroly na odkazované funkce:</span><span class="sxs-lookup"><span data-stu-id="52299-692">When you save the logic app, we perform some checks on the referenced function:</span></span>
-   <span data-ttu-id="52299-693">Musíte mít přístup k funkci.</span><span class="sxs-lookup"><span data-stu-id="52299-693">You need to have access to the function.</span></span>
-   <span data-ttu-id="52299-694">Jenom je povolen standard triggeru protokolu HTTP nebo obecné aktivační události webhooku JSON.</span><span class="sxs-lookup"><span data-stu-id="52299-694">Only standard HTTP trigger or generic JSON webhook trigger is allowed.</span></span>
-   <span data-ttu-id="52299-695">Všechny trasy definované by neměl mít.</span><span class="sxs-lookup"><span data-stu-id="52299-695">It should not have any route defined.</span></span>
-   <span data-ttu-id="52299-696">Pouze "funkce" a "anonymní" oprávnění na úrovni je povolen.</span><span class="sxs-lookup"><span data-stu-id="52299-696">Only "function" and "anonymous" authorization level is allowed.</span></span>

<span data-ttu-id="52299-697">Adresa URL aktivační události je načíst, do mezipaměti a použít za běhu.</span><span class="sxs-lookup"><span data-stu-id="52299-697">The trigger URL is retrieved, cached, and used at runtime.</span></span> <span data-ttu-id="52299-698">Takže pokud všechny operace by způsobila neplatnost adresu URL v mezipaměti, akce selže za běhu.</span><span class="sxs-lookup"><span data-stu-id="52299-698">So if any operation invalidates the cached URL, the action fails at runtime.</span></span> <span data-ttu-id="52299-699">Chcete-li tento problém obejít, uložte aplikaci logiky znovu, což způsobí, že aplikace logiky k načtení a znovu mezipaměti adresu URL aktivační události.</span><span class="sxs-lookup"><span data-stu-id="52299-699">To work around this, save the logic app again, which will cause logic app to retrieve and cache the trigger URL again.</span></span>

## <a name="collection-actions-scopes-and-loops"></a><span data-ttu-id="52299-700">Kolekce akce (rozsahy a smyčky)</span><span class="sxs-lookup"><span data-stu-id="52299-700">Collection actions (scopes and loops)</span></span>

<span data-ttu-id="52299-701">Některé typy akcí může obsahovat akce v rámci sami.</span><span class="sxs-lookup"><span data-stu-id="52299-701">Some action types can contain actions within themselves.</span></span> <span data-ttu-id="52299-702">Odkaz akce v rámci kolekce může být odkaz přímo mimo kolekce.</span><span class="sxs-lookup"><span data-stu-id="52299-702">Reference actions within a collection can be referenced directly outside of the collection.</span></span> <span data-ttu-id="52299-703">Pokud jste definovali `http` v oboru, `@body('http')` je stále platný kdekoli v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="52299-703">If you defined `http` in a scope, `@body('http')` is still valid anywhere in a workflow.</span></span> <span data-ttu-id="52299-704">Akce v rámci kolekce můžete `runAfter` jenom jiné akce v rámci stejné kolekce.</span><span class="sxs-lookup"><span data-stu-id="52299-704">Actions within a collection can `runAfter` only other actions within the same collection.</span></span>

## <a name="scope-action"></a><span data-ttu-id="52299-705">Akce oboru</span><span class="sxs-lookup"><span data-stu-id="52299-705">Scope action</span></span>

<span data-ttu-id="52299-706">`scope` Akce umožňuje logicky akce skupiny v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="52299-706">The `scope` action lets you logically group actions in a workflow.</span></span>

|<span data-ttu-id="52299-707">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52299-707">Name</span></span>|<span data-ttu-id="52299-708">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-708">Required</span></span>|<span data-ttu-id="52299-709">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-709">Type</span></span>|<span data-ttu-id="52299-710">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-710">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="52299-711">Akce</span><span class="sxs-lookup"><span data-stu-id="52299-711">actions</span></span>|<span data-ttu-id="52299-712">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-712">Yes</span></span>|<span data-ttu-id="52299-713">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-713">Object</span></span>|<span data-ttu-id="52299-714">Vnitřní akce provést v rámci oboru</span><span class="sxs-lookup"><span data-stu-id="52299-714">Inner actions to execute within the scope</span></span>|

```json
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```

## <a name="foreach-action"></a><span data-ttu-id="52299-715">Akce ForEach</span><span class="sxs-lookup"><span data-stu-id="52299-715">ForEach action</span></span>

<span data-ttu-id="52299-716">Tato akce opakování iteruje v rámci pole a provádí vnitřní akce pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="52299-716">This looping action iterates through an array and performs inner actions for each item.</span></span> <span data-ttu-id="52299-717">Ve výchozím nastavení smyčka typu foreach spustí paralelně (20 spuštěních paralelně v čase).</span><span class="sxs-lookup"><span data-stu-id="52299-717">By default, the foreach loop executes in parallel (20 executions in parallel at a time).</span></span> <span data-ttu-id="52299-718">Budete moct nastavit pravidla spouštění pomocí `operationOptions` parametr.</span><span class="sxs-lookup"><span data-stu-id="52299-718">You can set execution rules using the `operationOptions` parameter.</span></span>

|<span data-ttu-id="52299-719">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52299-719">Name</span></span>|<span data-ttu-id="52299-720">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-720">Required</span></span>|<span data-ttu-id="52299-721">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-721">Type</span></span>|<span data-ttu-id="52299-722">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-722">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="52299-723">Akce</span><span class="sxs-lookup"><span data-stu-id="52299-723">actions</span></span>|<span data-ttu-id="52299-724">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-724">Yes</span></span>|<span data-ttu-id="52299-725">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-725">Object</span></span>|<span data-ttu-id="52299-726">Vnitřní akce provést v rámci smyčky</span><span class="sxs-lookup"><span data-stu-id="52299-726">Inner actions to execute within the loop</span></span>|
|<span data-ttu-id="52299-727">foreach</span><span class="sxs-lookup"><span data-stu-id="52299-727">foreach</span></span>|<span data-ttu-id="52299-728">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-728">Yes</span></span>|<span data-ttu-id="52299-729">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-729">string</span></span>|<span data-ttu-id="52299-730">Pole pro iterace</span><span class="sxs-lookup"><span data-stu-id="52299-730">The array to iterate over</span></span>|
|<span data-ttu-id="52299-731">operationOptions</span><span class="sxs-lookup"><span data-stu-id="52299-731">operationOptions</span></span>|<span data-ttu-id="52299-732">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-732">no</span></span>|<span data-ttu-id="52299-733">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-733">string</span></span>|<span data-ttu-id="52299-734">Všechny operace možnosti chování.</span><span class="sxs-lookup"><span data-stu-id="52299-734">Any operation options for behavior.</span></span> <span data-ttu-id="52299-735">Podporuje aktuálně pouze `sequential` postupně provést iterací (výchozí chování je paralelní)</span><span class="sxs-lookup"><span data-stu-id="52299-735">Currently only supports `sequential` to execute iterations sequentially (default behavior is parallel)</span></span>|

```json
"forEach_email": {
    "type": "foreach",
    "foreach": "@body('email_filter')",
    "actions": {
        "send_email": {
            "type": "ApiConnection",
            "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
            }
        }
    },
    "runAfter":{
        "email_filter": [ "Succeeded" ]
    }
}
```

## <a name="until-action"></a><span data-ttu-id="52299-736">Dokud akce</span><span class="sxs-lookup"><span data-stu-id="52299-736">Until action</span></span>

<span data-ttu-id="52299-737">Tato opakování akce provede vnitřní akce, dokud podmínku výsledků na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="52299-737">This looping action executes inner actions until a condition results to true.</span></span>

|<span data-ttu-id="52299-738">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52299-738">Name</span></span>|<span data-ttu-id="52299-739">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-739">Required</span></span>|<span data-ttu-id="52299-740">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-740">Type</span></span>|<span data-ttu-id="52299-741">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-741">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="52299-742">Akce</span><span class="sxs-lookup"><span data-stu-id="52299-742">actions</span></span>|<span data-ttu-id="52299-743">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-743">Yes</span></span>|<span data-ttu-id="52299-744">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-744">Object</span></span>|<span data-ttu-id="52299-745">Vnitřní akce provést v rámci smyčky</span><span class="sxs-lookup"><span data-stu-id="52299-745">Inner actions to execute within the loop</span></span>|
|<span data-ttu-id="52299-746">výraz</span><span class="sxs-lookup"><span data-stu-id="52299-746">expression</span></span>|<span data-ttu-id="52299-747">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-747">Yes</span></span>|<span data-ttu-id="52299-748">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-748">string</span></span>|<span data-ttu-id="52299-749">Výraz k vyhodnocení po každé iteraci</span><span class="sxs-lookup"><span data-stu-id="52299-749">The expression to evaluate after each iteration</span></span>|
|<span data-ttu-id="52299-750">Limit</span><span class="sxs-lookup"><span data-stu-id="52299-750">limit</span></span>|<span data-ttu-id="52299-751">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-751">yes</span></span>|<span data-ttu-id="52299-752">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-752">Object</span></span>|<span data-ttu-id="52299-753">Limity pro smyčky - alespoň jeden limit musí být definován.</span><span class="sxs-lookup"><span data-stu-id="52299-753">The limits for the loop - at least one limit must be defined</span></span>|
|<span data-ttu-id="52299-754">Počet</span><span class="sxs-lookup"><span data-stu-id="52299-754">count</span></span>|<span data-ttu-id="52299-755">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-755">no</span></span>|<span data-ttu-id="52299-756">celá čísla</span><span class="sxs-lookup"><span data-stu-id="52299-756">int</span></span>|<span data-ttu-id="52299-757">Limit počtu opakování, které lze provést</span><span class="sxs-lookup"><span data-stu-id="52299-757">The limit to the number of iterations that can be performed</span></span>|
|<span data-ttu-id="52299-758">Časový limit</span><span class="sxs-lookup"><span data-stu-id="52299-758">timeout</span></span>|<span data-ttu-id="52299-759">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-759">no</span></span>|<span data-ttu-id="52299-760">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-760">string</span></span>|<span data-ttu-id="52299-761">Časový limit pro jak dlouho by měl cykly.</span><span class="sxs-lookup"><span data-stu-id="52299-761">The timeout for how long it should loop.</span></span>  <span data-ttu-id="52299-762">Formátu ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="52299-762">ISO 8601 format</span></span>|


```json
 "Until_succeeded": {
    "actions": {
        "Http": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "expression": "@equals(outputs('Http')['statusCode', 200)",
    "limit": {
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {},
    "type": "Until"
}
```

## <a name="conditions---if-action"></a><span data-ttu-id="52299-763">Podmínky - li akce</span><span class="sxs-lookup"><span data-stu-id="52299-763">Conditions - If Action</span></span>

<span data-ttu-id="52299-764">`If` Akce umožňuje vyhodnotit podmínku a provést větev založené na tom, jestli je vyhodnocen `true`.</span><span class="sxs-lookup"><span data-stu-id="52299-764">The `If` action lets you evaluate a condition and execute a branch based on whether the expression evaluates to `true`.</span></span>

|<span data-ttu-id="52299-765">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="52299-765">Name</span></span>|<span data-ttu-id="52299-766">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52299-766">Required</span></span>|<span data-ttu-id="52299-767">Typ</span><span class="sxs-lookup"><span data-stu-id="52299-767">Type</span></span>|<span data-ttu-id="52299-768">Popis</span><span class="sxs-lookup"><span data-stu-id="52299-768">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="52299-769">Akce</span><span class="sxs-lookup"><span data-stu-id="52299-769">actions</span></span>|<span data-ttu-id="52299-770">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-770">Yes</span></span>|<span data-ttu-id="52299-771">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-771">Object</span></span>|<span data-ttu-id="52299-772">Vnitřní akce má provést při výraz vyhodnocen`true`</span><span class="sxs-lookup"><span data-stu-id="52299-772">Inner actions to execute when expression evaluates to `true`</span></span>|
|<span data-ttu-id="52299-773">výraz</span><span class="sxs-lookup"><span data-stu-id="52299-773">expression</span></span>|<span data-ttu-id="52299-774">Ano</span><span class="sxs-lookup"><span data-stu-id="52299-774">Yes</span></span>|<span data-ttu-id="52299-775">Řetězec</span><span class="sxs-lookup"><span data-stu-id="52299-775">string</span></span>|<span data-ttu-id="52299-776">Výraz k vyhodnocení</span><span class="sxs-lookup"><span data-stu-id="52299-776">The expression to evaluate</span></span>|
|<span data-ttu-id="52299-777">else</span><span class="sxs-lookup"><span data-stu-id="52299-777">else</span></span>|<span data-ttu-id="52299-778">Ne</span><span class="sxs-lookup"><span data-stu-id="52299-778">no</span></span>|<span data-ttu-id="52299-779">Objekt</span><span class="sxs-lookup"><span data-stu-id="52299-779">Object</span></span>|<span data-ttu-id="52299-780">Vnitřní akce má provést při výraz vyhodnocen`false`</span><span class="sxs-lookup"><span data-stu-id="52299-780">Inner actions to execute when expression evaluates to `false`</span></span>|
  
```json
"My_condition": {
    "actions": {
        "If_true": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "else": {
        "actions": {
            "if_false": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://myurl"
                },
                "runAfter": {},
                "type": "Http"
            }
        }
    },
    "expression": "@equals(triggerBody(), json(true))",
    "runAfter": {},
    "type": "If"
}
```  
  
<span data-ttu-id="52299-781">V následující tabulce jsou uvedeny příklady jak podmínky použití výrazů v akci:</span><span class="sxs-lookup"><span data-stu-id="52299-781">The following table shows examples of how conditions can use expressions in an action:</span></span>  
  
|<span data-ttu-id="52299-782">Hodnota JSON</span><span class="sxs-lookup"><span data-stu-id="52299-782">JSON value</span></span>|<span data-ttu-id="52299-783">výsledek</span><span class="sxs-lookup"><span data-stu-id="52299-783">Result</span></span>|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|<span data-ttu-id="52299-784">Jakoukoli hodnotu, která by vyhodnotit na hodnotu true způsobí, že tato podmínka předat.</span><span class="sxs-lookup"><span data-stu-id="52299-784">Any value that would evaluate to true causes this condition to pass.</span></span> <span data-ttu-id="52299-785">Podporovány jsou pouze výrazy logických hodnot.</span><span class="sxs-lookup"><span data-stu-id="52299-785">Only Boolean expressions are supported.</span></span> <span data-ttu-id="52299-786">Jiné typy převést na logickou hodnotu, použít funkce `empty`, `equals`.</span><span class="sxs-lookup"><span data-stu-id="52299-786">To convert other types to Boolean, use functions `empty`, `equals`.</span></span>|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|<span data-ttu-id="52299-787">Porovnání funkcí jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="52299-787">Comparison functions are supported.</span></span> <span data-ttu-id="52299-788">Například zde akce jenom provede, když se výstup act1 je větší než prahová hodnota.</span><span class="sxs-lookup"><span data-stu-id="52299-788">For the example here, the action only executes when the output of act1 is greater than the threshold.</span></span>|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|<span data-ttu-id="52299-789">Funkce logiky jsou podporovány také pro vytvoření vnořené výrazy logických hodnot.</span><span class="sxs-lookup"><span data-stu-id="52299-789">Logic functions are also supported to create nested Boolean expressions.</span></span> <span data-ttu-id="52299-790">V takovém případě akci provede, když se výstup act1 je nad prahovou hodnotou nebo nižší než 100.</span><span class="sxs-lookup"><span data-stu-id="52299-790">In this case, the action executes when the output of act1 is above the threshold or below 100.</span></span>|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|<span data-ttu-id="52299-791">Funkce pole můžete použít ke kontrole, pokud má pole všechny položky.</span><span class="sxs-lookup"><span data-stu-id="52299-791">You can use array functions to check if an array has any items.</span></span> <span data-ttu-id="52299-792">V takovém případě akci provede, když se pole chyby je prázdný.</span><span class="sxs-lookup"><span data-stu-id="52299-792">In this case, the action executes when the errors array is empty.</span></span>| 
|`"expression": "parameters('hasSpecialAction')"`|<span data-ttu-id="52299-793">Chyba: není platný podmínky, protože @ je vyžadován pro podmínky.</span><span class="sxs-lookup"><span data-stu-id="52299-793">Error - not a valid condition because @ is required for conditions.</span></span>|  
  
<span data-ttu-id="52299-794">Pokud je podmínka vyhodnocena jako úspěšně, podmínky je označen jako `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="52299-794">If a condition evaluates successfully, the condition is marked as `Succeeded`.</span></span> <span data-ttu-id="52299-795">Akce v rámci buď `actions` nebo `else` objekty vyhodnocení `Succeeded` když spustí a byly úspěšné, `Failed` když spustí a se nezdařilo, nebo `Skipped` Pokud není uvedené pobočky provést.</span><span class="sxs-lookup"><span data-stu-id="52299-795">Actions within either the `actions` or `else` objects evaluate to `Succeeded` when executed and succeeded, `Failed` when executed and failed, or `Skipped` when that branch is not executed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52299-796">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52299-796">Next steps</span></span>

[<span data-ttu-id="52299-797">Rozhraní API REST služby pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="52299-797">Workflow Service REST API</span></span>](https://docs.microsoft.com/rest/api/logic/workflows)
