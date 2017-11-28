---
title: "aaaWorkflow akce a aktivační události - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 857927b7d7df3fc9cdc4931ffdb613efde0db9f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a><span data-ttu-id="87f81-102">Akce pracovního postupu a aktivačních událostí pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="87f81-102">Workflow actions and triggers for Azure Logic Apps</span></span>

<span data-ttu-id="87f81-103">Aplikace logiky obsahovat triggery a akce.</span><span class="sxs-lookup"><span data-stu-id="87f81-103">Logic apps consist of triggers and actions.</span></span> <span data-ttu-id="87f81-104">Existuje šest typů služby aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="87f81-104">There are six types of triggers.</span></span> <span data-ttu-id="87f81-105">Každý typ má jiné rozhraní a různé chování.</span><span class="sxs-lookup"><span data-stu-id="87f81-105">Each type has different interface and different behavior.</span></span> <span data-ttu-id="87f81-106">Taky se dozvíte další podrobnosti prohlížením hello podrobnosti o hello [jazyk definic workflowů](logic-apps-workflow-definition-language.md).</span><span class="sxs-lookup"><span data-stu-id="87f81-106">You can also learn about other details by looking at hello details of hello [Workflow Definition Language](logic-apps-workflow-definition-language.md).</span></span>  
  
<span data-ttu-id="87f81-107">Přečtěte si další informace o aktivační události a akce a jak může využít toobuild logiku aplikace tooimprove toolearn podnikové procesy a pracovních postupů.</span><span class="sxs-lookup"><span data-stu-id="87f81-107">Read on toolearn more about triggers and actions and how you might use them toobuild logic apps tooimprove your business processes and workflows.</span></span>  
  
### <a name="triggers"></a><span data-ttu-id="87f81-108">Triggery</span><span class="sxs-lookup"><span data-stu-id="87f81-108">Triggers</span></span>  

<span data-ttu-id="87f81-109">Aktivační událost určuje hello volání, která můžete zahájit spuštění pracovního postupu aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="87f81-109">A trigger specifies hello calls that can initiate a run of your logic app workflow.</span></span> <span data-ttu-id="87f81-110">Zde jsou hello dvěma různými způsoby tooinitiate spuštění pracovního postupu:</span><span class="sxs-lookup"><span data-stu-id="87f81-110">Here are hello two different ways tooinitiate a run of your workflow:</span></span>  
  
-   <span data-ttu-id="87f81-111">Aktivační událost dotazování</span><span class="sxs-lookup"><span data-stu-id="87f81-111">A polling trigger</span></span>  

-   <span data-ttu-id="87f81-112">Aktivační událost nabízené - ve volání hello [rozhraní API REST služby pracovního postupu](https://docs.microsoft.com/rest/api/logic/workflows)</span><span class="sxs-lookup"><span data-stu-id="87f81-112">A push trigger - by calling hello [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows)</span></span>  
  
<span data-ttu-id="87f81-113">Žádná z aktivačních událostí obsahovat tyto prvky nejvyšší úrovně:</span><span class="sxs-lookup"><span data-stu-id="87f81-113">All triggers contain these top-level elements:</span></span>  
  
```json
"<name-of-the-trigger>" : {
    "type": "<type-of-trigger>",
    "inputs": { <settings-for-the-call> },
    "recurrence": {  
        "frequency": "Second|Minute|Hour|Week|Month|Year",
        "interval": "<recurrence interval in units of frequency>"
    },
    "conditions": [ <array-of-required-conditions > ],
    "splitOn" : "<property toocreate runs for>",
    "operationOptions": "<operation options on hello trigger>"
}
```

### <a name="trigger-types-and-their-inputs"></a><span data-ttu-id="87f81-114">Typy aktivační události a jejich vstupy</span><span class="sxs-lookup"><span data-stu-id="87f81-114">Trigger types and their inputs</span></span>  

<span data-ttu-id="87f81-115">Můžete použít tyto typy triggerů:</span><span class="sxs-lookup"><span data-stu-id="87f81-115">You can use these types of triggers:</span></span>
  
-   <span data-ttu-id="87f81-116">**Žádosti o** \- díky aplikaci logiky hello koncového bodu je toocall</span><span class="sxs-lookup"><span data-stu-id="87f81-116">**Request** \- Makes hello logic app an endpoint for you toocall</span></span>  
  
-   <span data-ttu-id="87f81-117">**Opakování** \- aktivuje podle definovaného plánu</span><span class="sxs-lookup"><span data-stu-id="87f81-117">**Recurrence** \- Fires based on a defined schedule</span></span>  
  
-   <span data-ttu-id="87f81-118">**HTTP** \- dotazuje koncový bod webové HTTP.</span><span class="sxs-lookup"><span data-stu-id="87f81-118">**HTTP** \- Polls an HTTP web endpoint.</span></span> <span data-ttu-id="87f81-119">koncový bod Hello HTTP musí odpovídat tooa konkrétní spouštěcí kontrakt \- buď pomocí 202\-asynchronní vzor nebo vrácením pole</span><span class="sxs-lookup"><span data-stu-id="87f81-119">hello HTTP endpoint must conform tooa specific triggering contract \- either by using a 202\-async pattern, or by returning an array</span></span>  
  
-   <span data-ttu-id="87f81-120">**ApiConnection** \- aktivovat hlasování jako hello HTTP, ale provádí se hello [Microsoft spravovaná rozhraní API](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="87f81-120">**ApiConnection** \- Polls like hello HTTP trigger, however, it takes advantage of hello [Microsoft-managed APIs](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>  
  
-   <span data-ttu-id="87f81-121">**HTTPWebhook** \- otevře na koncový bod, podobně jako toohello ruční aktivační událost, ale také zavolá se tooa zadané adresy URL tooregister a zrušit registraci</span><span class="sxs-lookup"><span data-stu-id="87f81-121">**HTTPWebhook** \- Opens an endpoint, similar toohello Manual trigger, however, it also calls out tooa specified URL tooregister and unregister</span></span>  
  
-   <span data-ttu-id="87f81-122">**ApiConnectionWebhook** \- funguje jako hello aktivační událost HTTPWebhook využitím hello Microsoft spravovaná rozhraní API</span><span class="sxs-lookup"><span data-stu-id="87f81-122">**ApiConnectionWebhook** \- Operates like hello HTTPWebhook trigger by taking advantage of hello Microsoft-managed APIs</span></span>       
    <span data-ttu-id="87f81-123">Každý typ aktivační událost má jinou sadu **vstupy** své chování, který definuje.</span><span class="sxs-lookup"><span data-stu-id="87f81-123">Each trigger type has a different set of **inputs** that defines its behavior.</span></span>  
  
## <a name="request-trigger"></a><span data-ttu-id="87f81-124">Žádost o aktivační události</span><span class="sxs-lookup"><span data-stu-id="87f81-124">Request trigger</span></span>  

<span data-ttu-id="87f81-125">Této aktivační události slouží jako koncový bod, který volání prostřednictvím požadavku HTTP tooinvoke svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="87f81-125">This trigger serves as an endpoint that you call via an HTTP Request tooinvoke your logic app.</span></span> <span data-ttu-id="87f81-126">Aktivační událost požadavku vypadá v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="87f81-126">A request trigger looks like this example:</span></span>  
  
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

<span data-ttu-id="87f81-127">Je také volitelná vlastnost s názvem **schématu**:</span><span class="sxs-lookup"><span data-stu-id="87f81-127">There is also an optional property called **schema**:</span></span>  
  
|<span data-ttu-id="87f81-128">Název elementu</span><span class="sxs-lookup"><span data-stu-id="87f81-128">Element name</span></span>|<span data-ttu-id="87f81-129">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-129">Required</span></span>|<span data-ttu-id="87f81-130">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-130">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="87f81-131">Schéma</span><span class="sxs-lookup"><span data-stu-id="87f81-131">schema</span></span>|<span data-ttu-id="87f81-132">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-132">No</span></span>|<span data-ttu-id="87f81-133">Schéma JSON, který ověří příchozí žádost hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-133">A JSON schema that validates hello incoming request.</span></span> <span data-ttu-id="87f81-134">Užitečné pomáháte vědět, které vlastnosti tooreference kroky následné pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="87f81-134">Useful for helping subsequent workflow steps know which properties tooreference.</span></span>|

<span data-ttu-id="87f81-135">tooinvoke tento koncový bod, je nutné toocall hello *listCallbackUrl* rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="87f81-135">tooinvoke this endpoint, you need toocall hello *listCallbackUrl* API.</span></span> <span data-ttu-id="87f81-136">V tématu [rozhraní API REST služby pracovního postupu](https://docs.microsoft.com/rest/api/logic/workflows).</span><span class="sxs-lookup"><span data-stu-id="87f81-136">See [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows).</span></span>  
  
## <a name="recurrence-trigger"></a><span data-ttu-id="87f81-137">Aktivační událost opakování</span><span class="sxs-lookup"><span data-stu-id="87f81-137">Recurrence trigger</span></span>  

<span data-ttu-id="87f81-138">Aktivační událost opakování je ten, který spouští na základě podle definovaného plánu.</span><span class="sxs-lookup"><span data-stu-id="87f81-138">A Recurrence trigger is one that runs based on a defined schedule.</span></span> <span data-ttu-id="87f81-139">V tomto příkladu může vypadat aktivační události:</span><span class="sxs-lookup"><span data-stu-id="87f81-139">Such a trigger might look like this example:</span></span>  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

<span data-ttu-id="87f81-140">Jak je vidět, je jednoduchý způsob toorun pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="87f81-140">As you can see, it is a simple way toorun a workflow.</span></span>  
  
|<span data-ttu-id="87f81-141">Název elementu</span><span class="sxs-lookup"><span data-stu-id="87f81-141">Element name</span></span>|<span data-ttu-id="87f81-142">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-142">Required</span></span>|<span data-ttu-id="87f81-143">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-143">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="87f81-144">frequency</span><span class="sxs-lookup"><span data-stu-id="87f81-144">frequency</span></span>|<span data-ttu-id="87f81-145">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-145">Yes</span></span>|<span data-ttu-id="87f81-146">Jak často hello trigger spustí.</span><span class="sxs-lookup"><span data-stu-id="87f81-146">How often hello trigger executes.</span></span> <span data-ttu-id="87f81-147">Použít pouze jednu z těchto možné hodnoty: sekundu, minutu, hodinu, den, týden, měsíc nebo rok</span><span class="sxs-lookup"><span data-stu-id="87f81-147">Use only one of these possible values: second, minute, hour, day, week, month, or year</span></span>|  
|<span data-ttu-id="87f81-148">interval</span><span class="sxs-lookup"><span data-stu-id="87f81-148">interval</span></span>|<span data-ttu-id="87f81-149">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-149">Yes</span></span>|<span data-ttu-id="87f81-150">Interval hello zadané frekvence opakování hello</span><span class="sxs-lookup"><span data-stu-id="87f81-150">Interval of hello given frequency for hello recurrence</span></span>|  
|<span data-ttu-id="87f81-151">startTime</span><span class="sxs-lookup"><span data-stu-id="87f81-151">startTime</span></span>|<span data-ttu-id="87f81-152">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-152">No</span></span>|<span data-ttu-id="87f81-153">Pokud parametr startTime je poskytován bez časový posun, použije se toto časové pásmo.</span><span class="sxs-lookup"><span data-stu-id="87f81-153">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
|<span data-ttu-id="87f81-154">Časové pásmo</span><span class="sxs-lookup"><span data-stu-id="87f81-154">timeZone</span></span>|<span data-ttu-id="87f81-155">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-155">no</span></span>|<span data-ttu-id="87f81-156">Pokud parametr startTime je poskytován bez časový posun, použije se toto časové pásmo.</span><span class="sxs-lookup"><span data-stu-id="87f81-156">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
  
<span data-ttu-id="87f81-157">Můžete také naplánovat provádění toostart aktivační události v určitém okamžiku v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-157">You can also schedule a trigger toostart executing at some point in hello future.</span></span> <span data-ttu-id="87f81-158">Například pokud chcete, aby toostart týdenního sestavy každé pondělí můžete naplánovat hello logiku aplikace toostart každé pondělí vytvořením hello následující aktivační události:</span><span class="sxs-lookup"><span data-stu-id="87f81-158">For example, if you want toostart a weekly report every Monday you can schedule hello logic app toostart every Monday by creating hello following trigger:</span></span>  

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

## <a name="http-trigger"></a><span data-ttu-id="87f81-159">Trigger HTTP</span><span class="sxs-lookup"><span data-stu-id="87f81-159">HTTP trigger</span></span>  

<span data-ttu-id="87f81-160">Aktivace protokolu HTTP dotazování zadaný koncový bod a zkontrolujte hello odpovědi toodetermine, zda se má provést hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="87f81-160">HTTP triggers poll a specified endpoint and check hello response toodetermine whether hello workflow should be executed.</span></span> <span data-ttu-id="87f81-161">objekt Hello vstupy přijímá hello sadu parametrů požadované tooconstruct volání protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="87f81-161">hello inputs object takes hello set of parameters required tooconstruct an HTTP call:</span></span>  
  
|<span data-ttu-id="87f81-162">Název elementu</span><span class="sxs-lookup"><span data-stu-id="87f81-162">Element name</span></span>|<span data-ttu-id="87f81-163">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-163">Required</span></span>|<span data-ttu-id="87f81-164">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-164">Description</span></span>|<span data-ttu-id="87f81-165">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-165">Type</span></span>|  
|----------------|------------|---------------|--------|  
|<span data-ttu-id="87f81-166">– Metoda</span><span class="sxs-lookup"><span data-stu-id="87f81-166">method</span></span>|<span data-ttu-id="87f81-167">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-167">yes</span></span>|<span data-ttu-id="87f81-168">Může být jedna z následujících metod HTTP hello: GET, POST, PUT, DELETE, PATCH nebo HEAD</span><span class="sxs-lookup"><span data-stu-id="87f81-168">Can be one of hello following HTTP methods: GET, POST, PUT, DELETE, PATCH, or HEAD</span></span>|<span data-ttu-id="87f81-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-169">String</span></span>|  
|<span data-ttu-id="87f81-170">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="87f81-170">uri</span></span>|<span data-ttu-id="87f81-171">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-171">yes</span></span>|<span data-ttu-id="87f81-172">koncový bod http nebo https, která je volána, Hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-172">hello http or https endpoint that is called.</span></span> <span data-ttu-id="87f81-173">Maximální počet kilobajtů 2.</span><span class="sxs-lookup"><span data-stu-id="87f81-173">Maximum of 2 kilobytes.</span></span>|<span data-ttu-id="87f81-174">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-174">String</span></span>|  
|<span data-ttu-id="87f81-175">Dotazy</span><span class="sxs-lookup"><span data-stu-id="87f81-175">queries</span></span>|<span data-ttu-id="87f81-176">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-176">No</span></span>|<span data-ttu-id="87f81-177">Objekt reprezentující hello dotazu parametry tooadd toohello URL.</span><span class="sxs-lookup"><span data-stu-id="87f81-177">An object representing hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="87f81-178">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="87f81-178">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|<span data-ttu-id="87f81-179">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-179">Object</span></span>|  
|<span data-ttu-id="87f81-180">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="87f81-180">headers</span></span>|<span data-ttu-id="87f81-181">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-181">No</span></span>|<span data-ttu-id="87f81-182">Objekt, který reprezentuje všechny hello hlavičky, které je odeslána žádost o toohello.</span><span class="sxs-lookup"><span data-stu-id="87f81-182">An object representing each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="87f81-183">Například tooset hello jazyk a typ na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="87f81-183">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|<span data-ttu-id="87f81-184">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-184">Object</span></span>|  
|<span data-ttu-id="87f81-185">Text</span><span class="sxs-lookup"><span data-stu-id="87f81-185">body</span></span>|<span data-ttu-id="87f81-186">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-186">No</span></span>|<span data-ttu-id="87f81-187">Objekt reprezentující hello datové části, která je odeslána toohello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="87f81-187">An object representing hello payload that is sent toohello endpoint.</span></span>|<span data-ttu-id="87f81-188">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-188">Object</span></span>|  
|<span data-ttu-id="87f81-189">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="87f81-189">retryPolicy</span></span>|<span data-ttu-id="87f81-190">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-190">No</span></span>|<span data-ttu-id="87f81-191">Objekt, který umožňuje přizpůsobit chování opakování hello 4xx nebo 5xx chyby.</span><span class="sxs-lookup"><span data-stu-id="87f81-191">An object that lets you customize hello retry behavior for 4xx or 5xx errors.</span></span>|<span data-ttu-id="87f81-192">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-192">Object</span></span>|  
|<span data-ttu-id="87f81-193">Ověřování</span><span class="sxs-lookup"><span data-stu-id="87f81-193">authentication</span></span>|<span data-ttu-id="87f81-194">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-194">No</span></span>|<span data-ttu-id="87f81-195">Představuje hello metoda, která hello požadavek by měl být ověřen.</span><span class="sxs-lookup"><span data-stu-id="87f81-195">Represents hello method that hello request should be authenticated.</span></span> <span data-ttu-id="87f81-196">Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="87f81-196">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="87f81-197">Nad scheduler, existuje více podporované jednu vlastnost: `authority` ve výchozím nastavení, tato hodnota je `https://login.windows.net` není-li zadána, ale můžete použít různé cílové skupiny jako`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="87f81-197">Beyond scheduler, there is one more supported property: `authority` By default, this value is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|<span data-ttu-id="87f81-198">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-198">Object</span></span>|  
  
<span data-ttu-id="87f81-199">Aktivace protokolu HTTP Hello vyžaduje hello tooconform rozhraní API HTTP pomocí specifického vzoru toowork vhodná pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="87f81-199">hello HTTP trigger requires hello HTTP API tooconform with a specific pattern toowork well with your logic app.</span></span> <span data-ttu-id="87f81-200">Vyžaduje hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="87f81-200">It requires hello following fields:</span></span>  
  
|<span data-ttu-id="87f81-201">Odpověď</span><span class="sxs-lookup"><span data-stu-id="87f81-201">Response</span></span>|<span data-ttu-id="87f81-202">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-202">Description</span></span>|  
|------------|---------------|  
|<span data-ttu-id="87f81-203">Stavový kód</span><span class="sxs-lookup"><span data-stu-id="87f81-203">Status code</span></span>|<span data-ttu-id="87f81-204">Stavovým kódem 200 \(OK\) toocause spustit.</span><span class="sxs-lookup"><span data-stu-id="87f81-204">Status code 200 \(OK\) toocause a run.</span></span> <span data-ttu-id="87f81-205">Další kód stavu nezpůsobí spustit.</span><span class="sxs-lookup"><span data-stu-id="87f81-205">Any other status code doesn't cause a run.</span></span>|  
|<span data-ttu-id="87f81-206">Opakujte\-po záhlaví</span><span class="sxs-lookup"><span data-stu-id="87f81-206">Retry\-after header</span></span>|<span data-ttu-id="87f81-207">Počet sekund do aplikace logiky hello dotazuje hello koncový bod znovu.</span><span class="sxs-lookup"><span data-stu-id="87f81-207">Number of seconds until hello logic app polls hello endpoint again.</span></span>|  
|<span data-ttu-id="87f81-208">Hlavička umístění</span><span class="sxs-lookup"><span data-stu-id="87f81-208">Location header</span></span>|<span data-ttu-id="87f81-209">Adresa URL toocall Hello na další interval dotazování hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-209">hello URL toocall on hello next polling interval.</span></span> <span data-ttu-id="87f81-210">Pokud není zadaný, použije se původní adresu URL hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-210">If not specified, hello original URL is used.</span></span>|  
  
<span data-ttu-id="87f81-211">Zde je několik příkladů různých chování pro různé typy požadavků:</span><span class="sxs-lookup"><span data-stu-id="87f81-211">Here are some examples of different behaviors for different types of requests:</span></span>  
  
|<span data-ttu-id="87f81-212">Kód odpovědi</span><span class="sxs-lookup"><span data-stu-id="87f81-212">Response code</span></span>|<span data-ttu-id="87f81-213">Opakujte\-po</span><span class="sxs-lookup"><span data-stu-id="87f81-213">Retry\-After</span></span>|<span data-ttu-id="87f81-214">Chování</span><span class="sxs-lookup"><span data-stu-id="87f81-214">Behavior</span></span>|  
|-----------------|----------------|------------|  
|<span data-ttu-id="87f81-215">200</span><span class="sxs-lookup"><span data-stu-id="87f81-215">200</span></span>|<span data-ttu-id="87f81-216">\(None\)</span><span class="sxs-lookup"><span data-stu-id="87f81-216">\(none\)</span></span>|<span data-ttu-id="87f81-217">Není platný aktivační událost, opakování\-se po hello požadovanou nebo jiný modul nikdy hlasování pro další požadavek hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-217">Not a valid trigger, Retry\-After is required, or else hello engine never polls for hello next request.</span></span>|  
|<span data-ttu-id="87f81-218">202</span><span class="sxs-lookup"><span data-stu-id="87f81-218">202</span></span>|<span data-ttu-id="87f81-219">60</span><span class="sxs-lookup"><span data-stu-id="87f81-219">60</span></span>|<span data-ttu-id="87f81-220">Nespouštějí hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="87f81-220">Do not trigger hello workflow.</span></span> <span data-ttu-id="87f81-221">Další pokus o Hello se stane za jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="87f81-221">hello next attempt happens in one minute.</span></span>|  
|<span data-ttu-id="87f81-222">200</span><span class="sxs-lookup"><span data-stu-id="87f81-222">200</span></span>|<span data-ttu-id="87f81-223">10</span><span class="sxs-lookup"><span data-stu-id="87f81-223">10</span></span>|<span data-ttu-id="87f81-224">Spustit hello workflow a znovu zkontrolujte pro další obsah za 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="87f81-224">Run hello workflow, and check again for more content in 10 seconds.</span></span>|  
|<span data-ttu-id="87f81-225">400</span><span class="sxs-lookup"><span data-stu-id="87f81-225">400</span></span>|<span data-ttu-id="87f81-226">\(None\)</span><span class="sxs-lookup"><span data-stu-id="87f81-226">\(none\)</span></span>|<span data-ttu-id="87f81-227">Chybný požadavek, nespouštějte hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="87f81-227">Bad request, do not run hello workflow.</span></span> <span data-ttu-id="87f81-228">Pokud neexistuje žádné **opakujte zásad** definován, je použita výchozí zásady hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-228">If there is no **Retry Policy** defined, then hello default policy is used.</span></span> <span data-ttu-id="87f81-229">Po překročení hello počet opakovaných pokusů, hello aktivační událost již není platný.</span><span class="sxs-lookup"><span data-stu-id="87f81-229">After hello number of retries has been reached, hello trigger is no longer valid.</span></span>|  
|<span data-ttu-id="87f81-230">500</span><span class="sxs-lookup"><span data-stu-id="87f81-230">500</span></span>|<span data-ttu-id="87f81-231">\(None\)</span><span class="sxs-lookup"><span data-stu-id="87f81-231">\(none\)</span></span>|<span data-ttu-id="87f81-232">Chyba serveru, nespouštějte hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="87f81-232">Server error, do not run hello workflow.</span></span>  <span data-ttu-id="87f81-233">Pokud neexistuje žádné **opakujte zásad** definován, je použita výchozí zásady hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-233">If there is no **Retry Policy** defined, then hello default policy is used.</span></span> <span data-ttu-id="87f81-234">Po překročení hello počet opakovaných pokusů, hello aktivační událost již není platný.</span><span class="sxs-lookup"><span data-stu-id="87f81-234">After hello number of retries has been reached, hello trigger is no longer valid.</span></span>|  
  
<span data-ttu-id="87f81-235">Hello výstupy aktivační procedury HTTP vypadat podobně jako tento příklad:</span><span class="sxs-lookup"><span data-stu-id="87f81-235">hello outputs of an HTTP trigger look like this example:</span></span>  
  
|<span data-ttu-id="87f81-236">Název elementu</span><span class="sxs-lookup"><span data-stu-id="87f81-236">Element name</span></span>|<span data-ttu-id="87f81-237">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-237">Description</span></span>|<span data-ttu-id="87f81-238">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-238">Type</span></span>|  
|----------------|---------------|--------|  
|<span data-ttu-id="87f81-239">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="87f81-239">headers</span></span>|<span data-ttu-id="87f81-240">Hello hlavičky odpovědi http hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-240">hello headers of hello http response.</span></span>|<span data-ttu-id="87f81-241">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-241">Object</span></span>|  
|<span data-ttu-id="87f81-242">Text</span><span class="sxs-lookup"><span data-stu-id="87f81-242">body</span></span>|<span data-ttu-id="87f81-243">Hello text odpovědi http hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-243">hello body of hello http response.</span></span>|<span data-ttu-id="87f81-244">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-244">Object</span></span>|  
  
## <a name="api-connection-trigger"></a><span data-ttu-id="87f81-245">Aktivační události připojení k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="87f81-245">API Connection trigger</span></span>  

<span data-ttu-id="87f81-246">aktivační události připojení Hello rozhraní API je podobné toohello triggeru protokolu HTTP v jeho základní funkce.</span><span class="sxs-lookup"><span data-stu-id="87f81-246">hello API connection trigger is similar toohello HTTP trigger in its basic functionality.</span></span> <span data-ttu-id="87f81-247">Hello parametry pro identifikaci hello akce se ale liší.</span><span class="sxs-lookup"><span data-stu-id="87f81-247">However, hello parameters for identifying hello action are different.</span></span> <span data-ttu-id="87f81-248">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="87f81-248">Here is an example:</span></span>  
  
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

|<span data-ttu-id="87f81-249">Název elementu</span><span class="sxs-lookup"><span data-stu-id="87f81-249">Element name</span></span>|<span data-ttu-id="87f81-250">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-250">Required</span></span>|<span data-ttu-id="87f81-251">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-251">Type</span></span>|<span data-ttu-id="87f81-252">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-252">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="87f81-253">hostitele</span><span class="sxs-lookup"><span data-stu-id="87f81-253">host</span></span>|<span data-ttu-id="87f81-254">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-254">Yes</span></span>||<span data-ttu-id="87f81-255">Hello ApiApp hostované brány a id.</span><span class="sxs-lookup"><span data-stu-id="87f81-255">hello ApiApp hosted gateway and id.</span></span>|  
|<span data-ttu-id="87f81-256">– Metoda</span><span class="sxs-lookup"><span data-stu-id="87f81-256">method</span></span>|<span data-ttu-id="87f81-257">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-257">Yes</span></span>|<span data-ttu-id="87f81-258">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-258">String</span></span>|<span data-ttu-id="87f81-259">Může být jedna z následujících metod HTTP hello: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo  **HEAD**</span><span class="sxs-lookup"><span data-stu-id="87f81-259">Can be one of hello following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="87f81-260">Dotazy</span><span class="sxs-lookup"><span data-stu-id="87f81-260">queries</span></span>|<span data-ttu-id="87f81-261">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-261">No</span></span>|<span data-ttu-id="87f81-262">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-262">Object</span></span>|<span data-ttu-id="87f81-263">Představuje hello dotazu parametry toobe přidat adresu URL toohello.</span><span class="sxs-lookup"><span data-stu-id="87f81-263">Represents hello query parameters toobe added toohello URL.</span></span> <span data-ttu-id="87f81-264">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="87f81-264">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="87f81-265">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="87f81-265">headers</span></span>|<span data-ttu-id="87f81-266">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-266">No</span></span>|<span data-ttu-id="87f81-267">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-267">Object</span></span>|<span data-ttu-id="87f81-268">Představuje všechny hello hlavičky, které je odeslána žádost o toohello.</span><span class="sxs-lookup"><span data-stu-id="87f81-268">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="87f81-269">Například tooset hello jazyk a typ na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="87f81-269">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="87f81-270">Text</span><span class="sxs-lookup"><span data-stu-id="87f81-270">body</span></span>|<span data-ttu-id="87f81-271">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-271">No</span></span>|<span data-ttu-id="87f81-272">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-272">Object</span></span>|<span data-ttu-id="87f81-273">Představuje datovou část hello odeslaný toohello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="87f81-273">Represents hello payload that is sent toohello endpoint.</span></span>|  
|<span data-ttu-id="87f81-274">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="87f81-274">retryPolicy</span></span>|<span data-ttu-id="87f81-275">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-275">No</span></span>|<span data-ttu-id="87f81-276">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-276">Object</span></span>|<span data-ttu-id="87f81-277">Umožňuje vám toocustomize hello opakování chování 4xx nebo 5xx chyby.</span><span class="sxs-lookup"><span data-stu-id="87f81-277">Allows you toocustomize hello retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="87f81-278">Ověřování</span><span class="sxs-lookup"><span data-stu-id="87f81-278">authentication</span></span>|<span data-ttu-id="87f81-279">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-279">No</span></span>|<span data-ttu-id="87f81-280">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-280">Object</span></span>|<span data-ttu-id="87f81-281">Představuje hello metoda, která hello požadavek by měl být ověřen.</span><span class="sxs-lookup"><span data-stu-id="87f81-281">Represents hello method that hello request should be authenticated.</span></span> <span data-ttu-id="87f81-282">Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span><span class="sxs-lookup"><span data-stu-id="87f81-282">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span></span>|  
  
<span data-ttu-id="87f81-283">jsou Hello vlastnosti pro hostitele:</span><span class="sxs-lookup"><span data-stu-id="87f81-283">hello properties for host are:</span></span>  
  
|<span data-ttu-id="87f81-284">Název elementu</span><span class="sxs-lookup"><span data-stu-id="87f81-284">Element name</span></span>|<span data-ttu-id="87f81-285">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-285">Required</span></span>|<span data-ttu-id="87f81-286">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-286">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="87f81-287">rozhraní API runtimeUrl</span><span class="sxs-lookup"><span data-stu-id="87f81-287">api runtimeUrl</span></span>|<span data-ttu-id="87f81-288">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-288">Yes</span></span>|<span data-ttu-id="87f81-289">koncový bod Hello hello spravované rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="87f81-289">hello endpoint of hello managed API.</span></span>|  
|<span data-ttu-id="87f81-290">Název připojení</span><span class="sxs-lookup"><span data-stu-id="87f81-290">connection name</span></span>||<span data-ttu-id="87f81-291">Musí být parametr tooa odkaz nazvaný `$connection` a je název hello připojení hello spravované rozhraní API, které hello používá pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="87f81-291">Must be a reference tooa parameter called `$connection` and is hello name of hello managed API connection that hello workflow uses.</span></span>|
  
<span data-ttu-id="87f81-292">Hello výstupy aktivační událost pro připojení rozhraní API jsou:</span><span class="sxs-lookup"><span data-stu-id="87f81-292">hello outputs of an API connection trigger are:</span></span>
  
|<span data-ttu-id="87f81-293">Název elementu</span><span class="sxs-lookup"><span data-stu-id="87f81-293">Element name</span></span>|<span data-ttu-id="87f81-294">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-294">Type</span></span>|<span data-ttu-id="87f81-295">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-295">Description</span></span>|  
|----------------|--------|---------------|  
|<span data-ttu-id="87f81-296">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="87f81-296">headers</span></span>|<span data-ttu-id="87f81-297">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-297">Object</span></span>|<span data-ttu-id="87f81-298">Hello hlavičky odpovědi http hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-298">hello headers of hello http response.</span></span>|  
|<span data-ttu-id="87f81-299">Text</span><span class="sxs-lookup"><span data-stu-id="87f81-299">body</span></span>|<span data-ttu-id="87f81-300">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-300">Object</span></span>|<span data-ttu-id="87f81-301">Hello text odpovědi http hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-301">hello body of hello http response.</span></span>|  
  
## <a name="httpwebhook-trigger"></a><span data-ttu-id="87f81-302">Aktivační událost HTTPWebhook</span><span class="sxs-lookup"><span data-stu-id="87f81-302">HTTPWebhook trigger</span></span>  

<span data-ttu-id="87f81-303">Hello HTTPWebhook aktivační událost otevře koncový bod, podobně jako ruční aktivační událost toohello ale hello aktivační událost HTTPWebhook také voláním out tooa zadané adresy URL tooregister a zrušit registraci.</span><span class="sxs-lookup"><span data-stu-id="87f81-303">hello HTTPWebhook trigger opens an endpoint, similar toohello manual trigger, but hello HTTPWebhook trigger also calls out tooa specified URL tooregister and unregister.</span></span> <span data-ttu-id="87f81-304">Tady je příklad, jak může vypadat aktivační procedury HTTPWebhook:</span><span class="sxs-lookup"><span data-stu-id="87f81-304">Here's an example of what an HTTPWebhook trigger might look like:</span></span>  

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

<span data-ttu-id="87f81-305">Mnoho z těchto částí je volitelné a hello chování hello Webhooku závisí na oddíly, které jsou zadané nebo tento parametr vynechán.</span><span class="sxs-lookup"><span data-stu-id="87f81-305">Many of these sections are optional, and hello behavior of hello Webhook depends on which sections are provided or omitted.</span></span>  
<span data-ttu-id="87f81-306">Hello Webhook, jehož vlastnosti jsou následující:</span><span class="sxs-lookup"><span data-stu-id="87f81-306">hello properties of a Webhook are as follows:</span></span>  
  
|<span data-ttu-id="87f81-307">Název elementu</span><span class="sxs-lookup"><span data-stu-id="87f81-307">Element name</span></span>|<span data-ttu-id="87f81-308">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-308">Required</span></span>|<span data-ttu-id="87f81-309">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-309">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="87f81-310">přihlášení k odběru</span><span class="sxs-lookup"><span data-stu-id="87f81-310">subscribe</span></span>|<span data-ttu-id="87f81-311">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-311">No</span></span>|<span data-ttu-id="87f81-312">Hello odchozí požadavku, která je volána, když hello aktivační události je vytvořen a provede počáteční registrace hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-312">hello outgoing request that is called when hello trigger is created and performs hello initial registration.</span></span>|  
|<span data-ttu-id="87f81-313">Odhlásit</span><span class="sxs-lookup"><span data-stu-id="87f81-313">unsubscribe</span></span>|<span data-ttu-id="87f81-314">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-314">No</span></span>|<span data-ttu-id="87f81-315">Hello odchozí požadavek při odstranění hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="87f81-315">hello outgoing request when hello trigger is deleted.</span></span>|  
  
-   <span data-ttu-id="87f81-316">**Přihlášení k odběru** je hello odchozí volání, které provedl naslouchání tooevents toostart.</span><span class="sxs-lookup"><span data-stu-id="87f81-316">**Subscribe** is hello outgoing call that's made toostart listening tooevents.</span></span> <span data-ttu-id="87f81-317">Toto volání začíná hello se stejnou sadu parametrů, které hello normální akce HTTP.</span><span class="sxs-lookup"><span data-stu-id="87f81-317">This call starts with hello same set of parameters that hello normal HTTP actions do.</span></span> <span data-ttu-id="87f81-318">Tato odchozí Přišla žádost žádné hello čas pracovní postup změny žádným způsobem, například vždy, když hello přihlašovací údaje jsou vráceny, nebo aktivační událost hello vstup změnu parametry.</span><span class="sxs-lookup"><span data-stu-id="87f81-318">This outgoing call is made any time hello workflow changes in any way, for example, whenever hello credentials are rolled, or hello trigger's input parameters change.</span></span>
  
    <span data-ttu-id="87f81-319">toosupport toto volání, je nová funkce: `@listCallbackUrl()`.</span><span class="sxs-lookup"><span data-stu-id="87f81-319">toosupport this call, there is a new function: `@listCallbackUrl()`.</span></span> <span data-ttu-id="87f81-320">Tato funkce vrátí jedinečnou adresu URL pro tuto konkrétní aktivační událost v tomto pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="87f81-320">This function returns a unique URL for this specific trigger in this workflow.</span></span> <span data-ttu-id="87f81-321">Reprezentuje hello jedinečný identifikátor pro hello koncové body, které používají hello služby REST.</span><span class="sxs-lookup"><span data-stu-id="87f81-321">It represents hello unique identifier for hello endpoints that use hello Service REST.</span></span>  
  
-   <span data-ttu-id="87f81-322">**Odhlášení** je volána, když operace vykreslí této aktivační události neplatná, včetně:</span><span class="sxs-lookup"><span data-stu-id="87f81-322">**Unsubscribe** is called when an operation renders this trigger invalid, including:</span></span>  
  
    -   <span data-ttu-id="87f81-323">Odstranit nebo zakázat aktivační událost hello</span><span class="sxs-lookup"><span data-stu-id="87f81-323">Deleting or disabling hello trigger</span></span>  
  
    -   <span data-ttu-id="87f81-324">Odstranit nebo zakázat pracovní postup hello</span><span class="sxs-lookup"><span data-stu-id="87f81-324">Deleting or disabling hello workflow</span></span>  
  
    -   <span data-ttu-id="87f81-325">Odstranit nebo zakázat odběr hello</span><span class="sxs-lookup"><span data-stu-id="87f81-325">Deleting or disabling hello subscription</span></span>  
  
    <span data-ttu-id="87f81-326">aplikace logiky Hello automaticky volá hello zrušit akci.</span><span class="sxs-lookup"><span data-stu-id="87f81-326">hello logic app automatically calls hello unsubscribe action.</span></span> <span data-ttu-id="87f81-327">Hello parametry jsou funkce toothis hello stejné jako hello triggeru protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="87f81-327">hello parameters toothis function are hello same as hello HTTP trigger.</span></span>  
  
    <span data-ttu-id="87f81-328">Hello výstupy hello HTTPWebhook aktivační události jsou hello obsah hello příchozích požadavků:</span><span class="sxs-lookup"><span data-stu-id="87f81-328">hello outputs of hello HTTPWebhook trigger are hello contents of hello incoming request:</span></span>  
  
|<span data-ttu-id="87f81-329">Název elementu</span><span class="sxs-lookup"><span data-stu-id="87f81-329">Element name</span></span>|<span data-ttu-id="87f81-330">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-330">Type</span></span>|<span data-ttu-id="87f81-331">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-331">Description</span></span>|  
|-----------------|--------|---------------|  
|<span data-ttu-id="87f81-332">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="87f81-332">headers</span></span>|<span data-ttu-id="87f81-333">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-333">Object</span></span>|<span data-ttu-id="87f81-334">Hello hlavičky požadavku http hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-334">hello headers of hello http request.</span></span>|  
|<span data-ttu-id="87f81-335">Text</span><span class="sxs-lookup"><span data-stu-id="87f81-335">body</span></span>|<span data-ttu-id="87f81-336">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-336">Object</span></span>|<span data-ttu-id="87f81-337">Hello textu hello požadavku protokolu http.</span><span class="sxs-lookup"><span data-stu-id="87f81-337">hello body of hello http request.</span></span>|  

<span data-ttu-id="87f81-338">Omezení pro akci webhooku lze zadat v hello stejným způsobem jako [HTTP asynchronní omezení](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="87f81-338">Limits on a webhook action can be specified in hello same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  

## <a name="conditions"></a><span data-ttu-id="87f81-339">Podmínky</span><span class="sxs-lookup"><span data-stu-id="87f81-339">Conditions</span></span>  

<span data-ttu-id="87f81-340">Pro všechny aktivační události můžete použít jeden nebo více podmínek toodetermine zda pracovní postup hello měly být spuštěny, nebo není.</span><span class="sxs-lookup"><span data-stu-id="87f81-340">For any trigger, you can use one or more conditions toodetermine whether hello workflow should run or not.</span></span> <span data-ttu-id="87f81-341">Například:</span><span class="sxs-lookup"><span data-stu-id="87f81-341">For example:</span></span>  

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

<span data-ttu-id="87f81-342">V takovém případě hello pouze aktivační události sestavy při hello workflow `sendReports` tootrue je nastaven parametr.</span><span class="sxs-lookup"><span data-stu-id="87f81-342">In this case, hello report only triggers while hello workflow's `sendReports` parameter is set tootrue.</span></span> <span data-ttu-id="87f81-343">Nakonec podmínky odkazy na hello stavový kód hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="87f81-343">Finally, conditions may reference hello status code of hello trigger.</span></span> <span data-ttu-id="87f81-344">Například může ji pracovního postupu jenom v případě, že váš web vrátí stavový kód 500, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="87f81-344">For example, you could kick off a workflow only when your website returns a status code 500, as follows:</span></span>
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> <span data-ttu-id="87f81-345">Když jakýkoli výraz odkazuje na hello stavový kód aktivační událost hello \(žádným způsobem\), hello výchozí chování \(aktivační událost pouze na 200 \(OK\) \) je nahrazena.</span><span class="sxs-lookup"><span data-stu-id="87f81-345">When any expression references hello status code of hello trigger \(in any way\), hello default behavior \(trigger only on 200 \(OK\)\) is replaced.</span></span> <span data-ttu-id="87f81-346">Například pokud chcete tootrigger na stavovým kódem 200 a stavový kód 201, máte tooinclude: `@or(equals(triggers().code, 200),equals(triggers().code,201))` jako vaše podmínky.</span><span class="sxs-lookup"><span data-stu-id="87f81-346">For example, if you want tootrigger on both status code 200 and status code 201, you have tooinclude: `@or(equals(triggers().code, 200),equals(triggers().code,201))` as your condition.</span></span>  
  
## <a name="start-multiple-runs-for-a-request"></a><span data-ttu-id="87f81-347">Spuštění více spuštění pro žádost</span><span class="sxs-lookup"><span data-stu-id="87f81-347">Start multiple runs for a request</span></span>

<span data-ttu-id="87f81-348">tookick vypnout více spuštění pro jeden požadavek, `splitOn` je užitečné, například, když chcete toopoll koncový bod, který může mít několik nových položek mezi intervaly cyklického dotazování.</span><span class="sxs-lookup"><span data-stu-id="87f81-348">tookick off multiple runs for a single request, `splitOn` is useful, for example, when you want toopoll an endpoint that can have multiple new items between polling intervals.</span></span>
  
<span data-ttu-id="87f81-349">S `splitOn`, určíte hello vlastnost uvnitř datové části hello odpovědi, která obsahuje pole hello položek, z nichž každá má toouse toostart spustit hello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="87f81-349">With `splitOn`, you specify hello property inside hello response payload that contains hello array of items, each of which you want toouse toostart a run of hello trigger.</span></span> <span data-ttu-id="87f81-350">Představte si například, že máte rozhraní API, které vrátí hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="87f81-350">For example, imagine you have an API that returns hello following response:</span></span>  
  
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
  
<span data-ttu-id="87f81-351">Aplikace logiky stačí hello řádků obsahu, tak můžete vytvořit aktivační událost jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="87f81-351">Your logic app only needs hello Rows content, so you can construct your trigger like this example:</span></span>  
  
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
  
<span data-ttu-id="87f81-352">Potom v hello Definice pracovního postupu, `@triggerBody().name` vrátí `mycoolrow` pro hello nejprve spustit, a `another row` pro druhý spustit hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-352">Then, in hello workflow definition, `@triggerBody().name` returns `mycoolrow` for hello first run, and `another row` for hello second run.</span></span> <span data-ttu-id="87f81-353">Hello aktivační událost výstupy vypadají v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="87f81-353">hello trigger outputs look like this example:</span></span>  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

<span data-ttu-id="87f81-354">Pokud používáte `SplitOn`, nelze získat hello vlastnosti, které jsou mimo hello pole, v takovém případě hello `Status` pole.</span><span class="sxs-lookup"><span data-stu-id="87f81-354">So if you use `SplitOn`, you can't get hello properties that are outside hello array, in this case, hello `Status` field.</span></span>  
  
> [!NOTE]  
> <span data-ttu-id="87f81-355">V tomto příkladu používáme hello `?` operátor toobe možné tooavoid selhání, pokud hello `Rows` vlastnost není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="87f81-355">In this example, we use hello `?` operator toobe able tooavoid a failure if hello `Rows` property is not present.</span></span> 
  
## <a name="single-run-instance"></a><span data-ttu-id="87f81-356">Spuštění jedné instance</span><span class="sxs-lookup"><span data-stu-id="87f81-356">Single run instance</span></span>

<span data-ttu-id="87f81-357">Můžete nakonfigurovat aktivační události, které obsahují ještě efektivněji opakování vlastnost tooonly, pokud byly dokončeny všechny aktivní spustí.</span><span class="sxs-lookup"><span data-stu-id="87f81-357">You can configure triggers that have a recurrence property tooonly fire if all active runs have completed.</span></span> <span data-ttu-id="87f81-358">Pokud naplánované opakování dojde při spuštění s průběhem, aktivační události hello přeskočí a čeká, až hello další naplánované opakování interval toocheck znovu.</span><span class="sxs-lookup"><span data-stu-id="87f81-358">If a scheduled recurrence occurs while there is an in-progress run, hello trigger skips and waits until hello next scheduled recurrence interval toocheck again.</span></span>

<span data-ttu-id="87f81-359">Můžete nakonfigurovat toto nastavení prostřednictvím možnosti operaci hello:</span><span class="sxs-lookup"><span data-stu-id="87f81-359">You can configure this setting through hello operation options:</span></span>

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

## <a name="types-and-inputs"></a><span data-ttu-id="87f81-360">Typy a vstupy</span><span class="sxs-lookup"><span data-stu-id="87f81-360">Types and inputs</span></span>  

<span data-ttu-id="87f81-361">Existuje mnoho typů akcí, každý s jedinečný chování.</span><span class="sxs-lookup"><span data-stu-id="87f81-361">There are many types of actions, each with unique behavior.</span></span> <span data-ttu-id="87f81-362">Akce kolekce může obsahovat mnoho dalších akcí v rámci samotného.</span><span class="sxs-lookup"><span data-stu-id="87f81-362">Collection actions may contain many other actions within itself.</span></span>

### <a name="standard-actions"></a><span data-ttu-id="87f81-363">Standardní akce</span><span class="sxs-lookup"><span data-stu-id="87f81-363">Standard actions</span></span>  

-   <span data-ttu-id="87f81-364">**HTTP** tato akce zavolá koncový bod webové HTTP.</span><span class="sxs-lookup"><span data-stu-id="87f81-364">**HTTP** This action calls an HTTP web endpoint.</span></span>  
  
-   <span data-ttu-id="87f81-365">**ApiConnection** \- chová se tato akce jako hello akce HTTP, ale používá hello Microsoft spravovaná rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="87f81-365">**ApiConnection** \- This action behaves like hello HTTP action, but uses hello Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="87f81-366">**ApiConnectionWebhook** \- jako HTTPWebhook, ale hello používá rozhraní API pro spravovaný společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="87f81-366">**ApiConnectionWebhook** \- Like HTTPWebhook, but uses hello Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="87f81-367">**Odpověď** \- tato akce definuje odpovědi pro příchozí volání.</span><span class="sxs-lookup"><span data-stu-id="87f81-367">**Response** \- This action defines a response for an incoming call.</span></span>  
  
-   <span data-ttu-id="87f81-368">**Počkejte** \- tím jednoduché čeká pevnou hodnotu čas, nebo dokud určitý čas.</span><span class="sxs-lookup"><span data-stu-id="87f81-368">**Wait** \- This simple action waits a fixed amount of time or until a specific time.</span></span>  
  
-   <span data-ttu-id="87f81-369">**Pracovní postup** \- tato akce představuje vnořený pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="87f81-369">**Workflow** \- This action represents a nested workflow.</span></span>  

-   <span data-ttu-id="87f81-370">**Funkce** \- tato akce představuje funkci Azure.</span><span class="sxs-lookup"><span data-stu-id="87f81-370">**Function** \- This action represents an Azure Function.</span></span>

### <a name="collection-actions"></a><span data-ttu-id="87f81-371">Kolekce akce</span><span class="sxs-lookup"><span data-stu-id="87f81-371">Collection actions</span></span>

-   <span data-ttu-id="87f81-372">**Obor** \- tato akce je logické seskupení dalších akcí.</span><span class="sxs-lookup"><span data-stu-id="87f81-372">**Scope** \- This action is a logical grouping of other actions.</span></span>

-   <span data-ttu-id="87f81-373">**Podmínka** \- tato akce vyhodnotí výraz a provede hello odpovídající výsledek větev.</span><span class="sxs-lookup"><span data-stu-id="87f81-373">**Condition** \- This action evaluates an expression and executes hello corresponding result branch.</span></span>

-   <span data-ttu-id="87f81-374">**ForEach** \- tato opakování akce iteruje v rámci pole a provede vnitřní akce pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="87f81-374">**ForEach** \- This looping action iterates through an array and performs inner actions for each item.</span></span>

-   <span data-ttu-id="87f81-375">**Dokud** \- tato opakování akce provede vnitřní akce, dokud podmínku výsledků tootrue.</span><span class="sxs-lookup"><span data-stu-id="87f81-375">**Until** \- This looping action executes inner actions until a condition results tootrue.</span></span>
  
<span data-ttu-id="87f81-376">Každý typ akce má jinou sadu **vstupy** které definují chování akci.</span><span class="sxs-lookup"><span data-stu-id="87f81-376">Each type of action has a different set of **inputs** that define an action's behavior.</span></span>  
  
## <a name="http-action"></a><span data-ttu-id="87f81-377">Akce HTTP</span><span class="sxs-lookup"><span data-stu-id="87f81-377">HTTP action</span></span>  

<span data-ttu-id="87f81-378">Akce HTTP volání zadaný koncový bod a zkontrolujte hello odpovědi toodetermine, zda text hello pracovní postup spuštěn.</span><span class="sxs-lookup"><span data-stu-id="87f81-378">HTTP actions call a specified endpoint and check hello response toodetermine whether hello workflow should run.</span></span> <span data-ttu-id="87f81-379">Hello **vstupy** objekt trvá hello sadu parametrů požadované tooconstruct hello HTTP volání:</span><span class="sxs-lookup"><span data-stu-id="87f81-379">hello **inputs** object takes hello set of parameters required tooconstruct hello HTTP call:</span></span>  
  
|<span data-ttu-id="87f81-380">Název elementu</span><span class="sxs-lookup"><span data-stu-id="87f81-380">Element name</span></span>|<span data-ttu-id="87f81-381">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-381">Required</span></span>|<span data-ttu-id="87f81-382">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-382">Type</span></span>|<span data-ttu-id="87f81-383">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-383">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="87f81-384">– Metoda</span><span class="sxs-lookup"><span data-stu-id="87f81-384">method</span></span>|<span data-ttu-id="87f81-385">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-385">Yes</span></span>|<span data-ttu-id="87f81-386">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-386">String</span></span>|<span data-ttu-id="87f81-387">Může být jedna z následujících metod HTTP hello: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo  **HEAD**</span><span class="sxs-lookup"><span data-stu-id="87f81-387">Can be one of hello following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="87f81-388">identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="87f81-388">uri</span></span>|<span data-ttu-id="87f81-389">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-389">Yes</span></span>|<span data-ttu-id="87f81-390">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-390">String</span></span>|<span data-ttu-id="87f81-391">koncový bod http nebo https, která je volána, Hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-391">hello http or https endpoint that is called.</span></span> <span data-ttu-id="87f81-392">Maximální délka je 2 kB.</span><span class="sxs-lookup"><span data-stu-id="87f81-392">Maximum length is 2 kilobytes.</span></span>|  
|<span data-ttu-id="87f81-393">Dotazy</span><span class="sxs-lookup"><span data-stu-id="87f81-393">queries</span></span>|<span data-ttu-id="87f81-394">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-394">No</span></span>|<span data-ttu-id="87f81-395">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-395">Object</span></span>|<span data-ttu-id="87f81-396">Představuje hello dotazu parametry tooadd toohello URL.</span><span class="sxs-lookup"><span data-stu-id="87f81-396">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="87f81-397">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="87f81-397">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="87f81-398">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="87f81-398">headers</span></span>|<span data-ttu-id="87f81-399">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-399">No</span></span>|<span data-ttu-id="87f81-400">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-400">Object</span></span>|<span data-ttu-id="87f81-401">Představuje všechny hello hlavičky, které je odeslána žádost o toohello.</span><span class="sxs-lookup"><span data-stu-id="87f81-401">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="87f81-402">Například tooset hello jazyk a typ na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="87f81-402">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="87f81-403">Text</span><span class="sxs-lookup"><span data-stu-id="87f81-403">body</span></span>|<span data-ttu-id="87f81-404">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-404">No</span></span>|<span data-ttu-id="87f81-405">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-405">Object</span></span>|<span data-ttu-id="87f81-406">Představuje datovou část hello odeslaný toohello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="87f81-406">Represents hello payload that is sent toohello endpoint.</span></span>|  
|<span data-ttu-id="87f81-407">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="87f81-407">retryPolicy</span></span>|<span data-ttu-id="87f81-408">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-408">No</span></span>|<span data-ttu-id="87f81-409">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-409">Object</span></span>|<span data-ttu-id="87f81-410">Umožňuje upravovat chování opakování hello 4xx nebo 5xx chyby.</span><span class="sxs-lookup"><span data-stu-id="87f81-410">Lets you customize hello retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="87f81-411">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="87f81-411">operationsOptions</span></span>|<span data-ttu-id="87f81-412">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-412">No</span></span>|<span data-ttu-id="87f81-413">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-413">String</span></span>|<span data-ttu-id="87f81-414">Definuje sadu hello toooverride zvláštní chování.</span><span class="sxs-lookup"><span data-stu-id="87f81-414">Defines hello set of special behaviors toooverride.</span></span>|  
|<span data-ttu-id="87f81-415">Ověřování</span><span class="sxs-lookup"><span data-stu-id="87f81-415">authentication</span></span>|<span data-ttu-id="87f81-416">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-416">No</span></span>|<span data-ttu-id="87f81-417">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-417">Object</span></span>|<span data-ttu-id="87f81-418">Představuje hello metoda, která hello požadavek by měl být ověřen.</span><span class="sxs-lookup"><span data-stu-id="87f81-418">Represents hello method that hello request should be authenticated.</span></span> <span data-ttu-id="87f81-419">Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="87f81-419">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="87f81-420">Nad scheduler, existuje více podporované jednu vlastnost: `authority`.</span><span class="sxs-lookup"><span data-stu-id="87f81-420">Beyond scheduler, there is one more supported property: `authority`.</span></span> <span data-ttu-id="87f81-421">Ve výchozím nastavení je to `https://login.windows.net` není-li zadána, ale můžete použít různé cílové skupiny jako`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="87f81-421">By default, this is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|  
  
<span data-ttu-id="87f81-422">Akce HTTP \(a připojení k rozhraní API\) podporu akce opakujte zásady.</span><span class="sxs-lookup"><span data-stu-id="87f81-422">HTTP actions \(and API Connection\) actions support retry policies.</span></span> <span data-ttu-id="87f81-423">Zásady opakování platí toointermittent selhání, vyznačují jako stav HTTP 408 429 a 5xx v výjimky připojení tooany přidání kódy.</span><span class="sxs-lookup"><span data-stu-id="87f81-423">A retry policy applies toointermittent failures, characterized as HTTP status codes 408, 429, and 5xx, in addition tooany connectivity exceptions.</span></span> <span data-ttu-id="87f81-424">Tato zásada je popsán pomocí hello *retryPolicy* objekt definovaný jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="87f81-424">This policy is described using hello *retryPolicy* object defined as shown here:</span></span>
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
<span data-ttu-id="87f81-425">interval opakování Hello je zadána ve formátu ISO 8601 hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-425">hello retry interval is specified in hello ISO 8601 format.</span></span> <span data-ttu-id="87f81-426">Tento interval má výchozí a minimální hodnotu 20 sekund, zatímco hello maximální hodnota je jedna hodina.</span><span class="sxs-lookup"><span data-stu-id="87f81-426">This interval has a default and minimum value of 20 seconds, while hello maximum value is one hour.</span></span> <span data-ttu-id="87f81-427">Hello výchozí a maximální počet opakování je čtyři hodiny.</span><span class="sxs-lookup"><span data-stu-id="87f81-427">hello default and maximum retry count is four hours.</span></span> <span data-ttu-id="87f81-428">Pokud není zadán hello definice zásady opakování, `fixed` strategie se používá s výchozí hodnoty a interval opakování.</span><span class="sxs-lookup"><span data-stu-id="87f81-428">If hello retry policy definition is not specified, a `fixed` strategy is used with default retry count and interval values.</span></span> <span data-ttu-id="87f81-429">zásady opakovaných pokusů hello toodisable, nastavte její typ příliš`None`.</span><span class="sxs-lookup"><span data-stu-id="87f81-429">toodisable hello retry policy, set its type too`None`.</span></span>  
  
<span data-ttu-id="87f81-430">Například hello následující akci opakovat načítání hello nejnovější informace dvakrát, pokud existují náhodnými poruchami, celkem tři spuštěních s 30 sekund zpoždění mezi jednotlivými pokusy o:</span><span class="sxs-lookup"><span data-stu-id="87f81-430">For example, hello following action retries fetching hello latest news two times, if there are intermittent failures, for a total of three executions, with a 30-second delay between each attempt:</span></span>  
  
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
### <a name="asynchronous-patterns"></a><span data-ttu-id="87f81-431">Asynchronními vzory</span><span class="sxs-lookup"><span data-stu-id="87f81-431">Asynchronous patterns</span></span>

<span data-ttu-id="87f81-432">Ve výchozím nastavení podporují všechny akce založené na protokolu HTTP hello standardní asynchronní operaci vzor.</span><span class="sxs-lookup"><span data-stu-id="87f81-432">By default, all HTTP-based actions support hello standard asynchronous operation pattern.</span></span> <span data-ttu-id="87f81-433">Takže pokud vzdálený server hello označuje této žádosti hello je přijaté ke zpracování s 202 \(platných\) odpovědi, modul Logic Apps hello udržuje dotazování hello adresa URL zadaná v hlavičce umístění hello odpovědi až do dosažení terminál Stav \(bez\-202 odpovědi\).</span><span class="sxs-lookup"><span data-stu-id="87f81-433">So if hello remote server indicates that hello request is accepted for processing with a 202 \(Accepted\) response, hello Logic Apps engine keeps polling hello URL specified in hello response's location header until reaching a terminal state \(a non\-202 response\).</span></span>  
  
<span data-ttu-id="87f81-434">asynchronní chování toodisable hello dříve popsané, nastavené `DisableAsyncPattern` možnost v hello akce vstupy.</span><span class="sxs-lookup"><span data-stu-id="87f81-434">toodisable hello asynchronous behavior previously described, set a `DisableAsyncPattern` option in hello action inputs.</span></span> <span data-ttu-id="87f81-435">V takovém případě hello výstup hello akce je založená na počáteční 202 odpovědi ze serveru hello hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-435">In this case, hello output of hello action is based on hello initial 202 response from hello server.</span></span>  
  
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

#### <a name="asynchronous-limits"></a><span data-ttu-id="87f81-436">Asynchronní omezení</span><span class="sxs-lookup"><span data-stu-id="87f81-436">Asynchronous Limits</span></span>

<span data-ttu-id="87f81-437">Asynchronní vzor může být omezena pouze jeho trvání tooa specifického časového intervalu.</span><span class="sxs-lookup"><span data-stu-id="87f81-437">An asynchronous pattern can be limited in its duration tooa specific time interval.</span></span>  <span data-ttu-id="87f81-438">Pokud hello časový interval uplynutí bez dosažení stavu terminálu, budou označeny hello stav akce hello `Cancelled` s a kód `ActionTimedOut`.</span><span class="sxs-lookup"><span data-stu-id="87f81-438">If hello time interval elapses without reaching a terminal state, hello status of hello action will be marked `Cancelled` with a code of `ActionTimedOut`.</span></span>  <span data-ttu-id="87f81-439">časový limit omezení Hello je zadána ve formátu ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="87f81-439">hello limit timeout is specified in ISO 8601 format.</span></span>  <span data-ttu-id="87f81-440">Omezení lze určit hello následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="87f81-440">Limits can be specified with hello following syntax:</span></span>

``` json
"<action-name>": {
    "type": "workflow|webhook|http|apiconnectionwebhook|apiconnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="api-connection"></a><span data-ttu-id="87f81-441">Připojení k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="87f81-441">API Connection</span></span>  

<span data-ttu-id="87f81-442">Připojení k rozhraní API je akce, která odkazuje na konektor spravovaný společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="87f81-442">API Connection is an action that references a Microsoft-managed connector.</span></span>
<span data-ttu-id="87f81-443">Tato akce vyžaduje odkaz tooa platné připojení a informace o hello rozhraní API a požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="87f81-443">This action requires a reference tooa valid connection, and information on hello API and parameters required.</span></span>

|<span data-ttu-id="87f81-444">Název elementu</span><span class="sxs-lookup"><span data-stu-id="87f81-444">Element name</span></span>|<span data-ttu-id="87f81-445">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-445">Required</span></span>|<span data-ttu-id="87f81-446">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-446">Type</span></span>|<span data-ttu-id="87f81-447">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-447">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="87f81-448">hostitele</span><span class="sxs-lookup"><span data-stu-id="87f81-448">host</span></span>|<span data-ttu-id="87f81-449">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-449">Yes</span></span>|<span data-ttu-id="87f81-450">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-450">Object</span></span>|<span data-ttu-id="87f81-451">Představuje informace o konektoru hello například objekt připojení toohello hello pro runtimeUrl a referenční informace</span><span class="sxs-lookup"><span data-stu-id="87f81-451">Represents hello connector information such as hello runtimeUrl and reference toohello connection object</span></span>|
|<span data-ttu-id="87f81-452">– Metoda</span><span class="sxs-lookup"><span data-stu-id="87f81-452">method</span></span>|<span data-ttu-id="87f81-453">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-453">Yes</span></span>|<span data-ttu-id="87f81-454">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-454">String</span></span>|<span data-ttu-id="87f81-455">Může být jedna z následujících metod HTTP hello: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo  **HEAD**</span><span class="sxs-lookup"><span data-stu-id="87f81-455">Can be one of hello following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="87f81-456">Cesta</span><span class="sxs-lookup"><span data-stu-id="87f81-456">path</span></span>|<span data-ttu-id="87f81-457">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-457">Yes</span></span>|<span data-ttu-id="87f81-458">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-458">String</span></span>|<span data-ttu-id="87f81-459">Cesta Hello operace hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="87f81-459">hello path of hello API operation.</span></span>|  
|<span data-ttu-id="87f81-460">Dotazy</span><span class="sxs-lookup"><span data-stu-id="87f81-460">queries</span></span>|<span data-ttu-id="87f81-461">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-461">No</span></span>|<span data-ttu-id="87f81-462">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-462">Object</span></span>|<span data-ttu-id="87f81-463">Představuje hello dotazu parametry tooadd toohello URL.</span><span class="sxs-lookup"><span data-stu-id="87f81-463">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="87f81-464">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="87f81-464">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="87f81-465">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="87f81-465">headers</span></span>|<span data-ttu-id="87f81-466">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-466">No</span></span>|<span data-ttu-id="87f81-467">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-467">Object</span></span>|<span data-ttu-id="87f81-468">Představuje všechny hello hlavičky, které je odeslána žádost o toohello.</span><span class="sxs-lookup"><span data-stu-id="87f81-468">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="87f81-469">Například tooset hello jazyk a typ na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="87f81-469">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="87f81-470">Text</span><span class="sxs-lookup"><span data-stu-id="87f81-470">body</span></span>|<span data-ttu-id="87f81-471">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-471">No</span></span>|<span data-ttu-id="87f81-472">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-472">Object</span></span>|<span data-ttu-id="87f81-473">Představuje datovou část hello odeslaný toohello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="87f81-473">Represents hello payload that is sent toohello endpoint.</span></span>|  
|<span data-ttu-id="87f81-474">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="87f81-474">retryPolicy</span></span>|<span data-ttu-id="87f81-475">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-475">No</span></span>|<span data-ttu-id="87f81-476">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-476">Object</span></span>|<span data-ttu-id="87f81-477">Umožňuje upravovat chování opakování hello 4xx nebo 5xx chyby.</span><span class="sxs-lookup"><span data-stu-id="87f81-477">Lets you customize hello retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="87f81-478">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="87f81-478">operationsOptions</span></span>|<span data-ttu-id="87f81-479">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-479">No</span></span>|<span data-ttu-id="87f81-480">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-480">String</span></span>|<span data-ttu-id="87f81-481">Definuje sadu hello toooverride zvláštní chování.</span><span class="sxs-lookup"><span data-stu-id="87f81-481">Defines hello set of special behaviors toooverride.</span></span>|  

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

## <a name="api-connection-webhook-action"></a><span data-ttu-id="87f81-482">Akce webhooku připojení k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="87f81-482">API Connection webhook action</span></span>

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

<span data-ttu-id="87f81-483">Omezení pro akci webhooku lze zadat v hello stejným způsobem jako [HTTP asynchronní omezení](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="87f81-483">Limits on a webhook action can be specified in hello same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  
## <a name="response-action"></a><span data-ttu-id="87f81-484">Akce odpovědi</span><span class="sxs-lookup"><span data-stu-id="87f81-484">Response action</span></span>  

<span data-ttu-id="87f81-485">Tento typ akce obsahuje datovou část celé odpovědi hello z požadavku HTTP a zahrnuje statusCode, text a hlavičky:</span><span class="sxs-lookup"><span data-stu-id="87f81-485">This action type contains hello entire response payload from an HTTP request and includes a statusCode, body, and headers:</span></span>  
  
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
  
<span data-ttu-id="87f81-486">Hello odpověď má zvláštní omezení, které se neuplatní tooother akce.</span><span class="sxs-lookup"><span data-stu-id="87f81-486">hello response action has special restrictions that don't apply tooother actions.</span></span> <span data-ttu-id="87f81-487">Zejména:</span><span class="sxs-lookup"><span data-stu-id="87f81-487">Specifically:</span></span>  
  
-   <span data-ttu-id="87f81-488">Akce reagující nemůže být paralelní v definici, protože příchozí požadavek toohello deterministický odpovědi je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="87f81-488">Response actions cannot be parallel in a definition because a deterministic response toohello incoming request is required.</span></span>  
  
-   <span data-ttu-id="87f81-489">Pokud akce odpovědi je dostupný po příchozího požadavku hello obdržel odpověď, považuje hello akce se nezdařilo \(konflikt\), a v důsledku toho je hello spustit `Failed`.</span><span class="sxs-lookup"><span data-stu-id="87f81-489">If a response action is reached after hello incoming request has received a response, hello action is considered failed \(conflict\), and as a result, hello run is `Failed`.</span></span>  
  
-   <span data-ttu-id="87f81-490">Pracovní postup s akcemi odpověď nemůže mít `splitOn` v jeho aktivační událost protože jedno volání způsobí, že mnoho spustí.</span><span class="sxs-lookup"><span data-stu-id="87f81-490">A workflow with Response actions cannot have `splitOn` in its trigger because one call causes many runs.</span></span> <span data-ttu-id="87f81-491">V důsledku toho to možné ověřit, když hello toku PUT a příčina chybný požadavek.</span><span class="sxs-lookup"><span data-stu-id="87f81-491">As a result, this should be validated when hello flow is PUT and cause a Bad Request.</span></span>  
  
## <a name="wait-action"></a><span data-ttu-id="87f81-492">Počkejte akce</span><span class="sxs-lookup"><span data-stu-id="87f81-492">Wait action</span></span>  

<span data-ttu-id="87f81-493">Hello `wait` akce pozastaví spuštění pracovního postupu pro hello zadaného intervalu.</span><span class="sxs-lookup"><span data-stu-id="87f81-493">hello `wait` action suspends workflow execution for hello specified interval.</span></span> <span data-ttu-id="87f81-494">Například toowait 15 minut, můžete použít tento fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="87f81-494">For example, toowait 15 minutes, you can use this snippet:</span></span>  
  
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
  
<span data-ttu-id="87f81-495">Alternativně toowait dokud konkrétní chvíli v čase, můžete použít tento příklad:</span><span class="sxs-lookup"><span data-stu-id="87f81-495">Alternatively, toowait until a specific moment in time, you can use this example:</span></span>  
  
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
> <span data-ttu-id="87f81-496">Doba čekání Hello můžete buď zadat pomocí hello **interval** objekt nebo hello **dokud** objektu, ale ne obojí.</span><span class="sxs-lookup"><span data-stu-id="87f81-496">hello wait duration can be either specified using hello **interval** object or hello **until** object, but not both.</span></span>  
  
|<span data-ttu-id="87f81-497">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87f81-497">Name</span></span>|<span data-ttu-id="87f81-498">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-498">Required</span></span>|<span data-ttu-id="87f81-499">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-499">Type</span></span>|<span data-ttu-id="87f81-500">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-500">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="87f81-501">interval</span><span class="sxs-lookup"><span data-stu-id="87f81-501">interval</span></span>|<span data-ttu-id="87f81-502">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-502">No</span></span>|<span data-ttu-id="87f81-503">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-503">Object</span></span>|<span data-ttu-id="87f81-504">doba podle časového intervalu čekání na Hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-504">hello wait duration based on amount of time.</span></span>|  
|<span data-ttu-id="87f81-505">jednotku intervalu</span><span class="sxs-lookup"><span data-stu-id="87f81-505">interval unit</span></span>|<span data-ttu-id="87f81-506">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-506">Yes</span></span>|<span data-ttu-id="87f81-507">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-507">String</span></span>|<span data-ttu-id="87f81-508">Mezi těmito intervaly: sekundu, minutu, hodinu, den, týden, měsíc, rok.</span><span class="sxs-lookup"><span data-stu-id="87f81-508">One of these intervals: second, minute, hour, day, week, month, year.</span></span>|  
|<span data-ttu-id="87f81-509">počet intervalu</span><span class="sxs-lookup"><span data-stu-id="87f81-509">interval count</span></span>|<span data-ttu-id="87f81-510">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-510">Yes</span></span>|<span data-ttu-id="87f81-511">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-511">String</span></span>|<span data-ttu-id="87f81-512">Doba trvání podle hello zadané interní jednotky.</span><span class="sxs-lookup"><span data-stu-id="87f81-512">Duration based on hello given internal unit.</span></span>|  
|<span data-ttu-id="87f81-513">dokud</span><span class="sxs-lookup"><span data-stu-id="87f81-513">until</span></span>|<span data-ttu-id="87f81-514">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-514">No</span></span>|<span data-ttu-id="87f81-515">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-515">Object</span></span>|<span data-ttu-id="87f81-516">Hello Počkejte, než doba trvání podle bod v čase.</span><span class="sxs-lookup"><span data-stu-id="87f81-516">hello wait duration based on a point in time.</span></span>|  
|<span data-ttu-id="87f81-517">dokud časové razítko</span><span class="sxs-lookup"><span data-stu-id="87f81-517">until timestamp</span></span>|<span data-ttu-id="87f81-518">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-518">Yes</span></span>|<span data-ttu-id="87f81-519">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-519">String</span></span>|<span data-ttu-id="87f81-520">Řetězec &#124; hello bodu v čase ve standardu UTC, když vyprší platnost hello čekání.</span><span class="sxs-lookup"><span data-stu-id="87f81-520">String&#124;hello point in time in UTC when hello wait expires.</span></span>|  

## <a name="query-action"></a><span data-ttu-id="87f81-521">Akce dotazu</span><span class="sxs-lookup"><span data-stu-id="87f81-521">Query action</span></span>

<span data-ttu-id="87f81-522">Hello `query` akce umožňuje filtrovat pole na základě podmínky.</span><span class="sxs-lookup"><span data-stu-id="87f81-522">hello `query` action lets you filter an array based on a condition.</span></span> <span data-ttu-id="87f81-523">Například tooselect čísla větší než 2, můžete použít:</span><span class="sxs-lookup"><span data-stu-id="87f81-523">For example, tooselect numbers greater than 2, you can use:</span></span>

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

<span data-ttu-id="87f81-524">Hello výstup hello `query` není pole, které má elementy z hello vstupní pole, které splňují podmínku hello akce.</span><span class="sxs-lookup"><span data-stu-id="87f81-524">hello output from hello `query` action is an array that has elements from hello input array that satisfy hello condition.</span></span>

> [!NOTE]
> <span data-ttu-id="87f81-525">Pokud žádné hodnoty splňují hello `where` podmínky, hello výsledkem je prázdné pole.</span><span class="sxs-lookup"><span data-stu-id="87f81-525">If no values satisfy hello `where` condition, hello result is an empty array.</span></span>

|<span data-ttu-id="87f81-526">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87f81-526">Name</span></span>|<span data-ttu-id="87f81-527">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-527">Required</span></span>|<span data-ttu-id="87f81-528">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-528">Type</span></span>|<span data-ttu-id="87f81-529">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-529">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="87f81-530">Z</span><span class="sxs-lookup"><span data-stu-id="87f81-530">from</span></span>|<span data-ttu-id="87f81-531">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-531">Yes</span></span>|<span data-ttu-id="87f81-532">Pole</span><span class="sxs-lookup"><span data-stu-id="87f81-532">Array</span></span>|<span data-ttu-id="87f81-533">Hello zdrojové pole.</span><span class="sxs-lookup"><span data-stu-id="87f81-533">hello source array.</span></span>|
|<span data-ttu-id="87f81-534">kde</span><span class="sxs-lookup"><span data-stu-id="87f81-534">where</span></span>|<span data-ttu-id="87f81-535">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-535">Yes</span></span>|<span data-ttu-id="87f81-536">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-536">String</span></span>|<span data-ttu-id="87f81-537">Hello podmínku tooapply tooeach element hello zdrojové pole.</span><span class="sxs-lookup"><span data-stu-id="87f81-537">hello condition tooapply tooeach element of hello source array.</span></span>|

## <a name="select-action"></a><span data-ttu-id="87f81-538">Vyberte akci</span><span class="sxs-lookup"><span data-stu-id="87f81-538">Select action</span></span>

<span data-ttu-id="87f81-539">Hello `select` akce umožňuje projektu každý element pole na novou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="87f81-539">hello `select` action lets you project each element of an array into a new value.</span></span>
<span data-ttu-id="87f81-540">Například tooconvert na pole čísla do pole objektů, můžete použít:</span><span class="sxs-lookup"><span data-stu-id="87f81-540">For example, tooconvert an array of numbers into an array of objects, you can use:</span></span>

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

<span data-ttu-id="87f81-541">Hello výstup hello `select` akce je pole, které má hello kardinalitu to stejné jako hello vstupního pole s každý prvek transformovat jako definované hello `select` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="87f81-541">hello output of hello `select` action is an array that has hello same cardinality as hello input array, with each element transformed as defined by hello `select` property.</span></span> <span data-ttu-id="87f81-542">Pokud vstupní hello je prázdné pole, výstup hello je také prázdné pole.</span><span class="sxs-lookup"><span data-stu-id="87f81-542">If hello input is an empty array, hello output is also an empty array.</span></span>

|<span data-ttu-id="87f81-543">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87f81-543">Name</span></span>|<span data-ttu-id="87f81-544">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-544">Required</span></span>|<span data-ttu-id="87f81-545">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-545">Type</span></span>|<span data-ttu-id="87f81-546">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-546">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="87f81-547">Z</span><span class="sxs-lookup"><span data-stu-id="87f81-547">from</span></span>|<span data-ttu-id="87f81-548">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-548">Yes</span></span>|<span data-ttu-id="87f81-549">Pole</span><span class="sxs-lookup"><span data-stu-id="87f81-549">Array</span></span>|<span data-ttu-id="87f81-550">Hello zdrojové pole.</span><span class="sxs-lookup"><span data-stu-id="87f81-550">hello source array.</span></span>|
|<span data-ttu-id="87f81-551">Vyberte</span><span class="sxs-lookup"><span data-stu-id="87f81-551">select</span></span>|<span data-ttu-id="87f81-552">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-552">Yes</span></span>|<span data-ttu-id="87f81-553">Všechny</span><span class="sxs-lookup"><span data-stu-id="87f81-553">Any</span></span>|<span data-ttu-id="87f81-554">Hello projekce tooapply tooeach element hello zdrojové pole.</span><span class="sxs-lookup"><span data-stu-id="87f81-554">hello projection tooapply tooeach element of hello source array.</span></span>|

## <a name="terminate-action"></a><span data-ttu-id="87f81-555">Ukončit akce</span><span class="sxs-lookup"><span data-stu-id="87f81-555">Terminate action</span></span>

<span data-ttu-id="87f81-556">Hello akce ukončení zastaví provádění hello pracovního postupu spustit, Probíhá rušení všechny během letu akce a přeskočení všechny zbývající akce.</span><span class="sxs-lookup"><span data-stu-id="87f81-556">hello Terminate action stops execution of hello workflow run, aborting any in-flight actions, and skipping any remaining actions.</span></span> <span data-ttu-id="87f81-557">Například tooterminate spustit stavem **se nezdařilo**, můžete použít hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="87f81-557">For example, tooterminate a run with status **Failed**, you can use hello following snippet:</span></span>

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
> <span data-ttu-id="87f81-558">Akce již byla dokončena nemá vliv hello ukončit akci.</span><span class="sxs-lookup"><span data-stu-id="87f81-558">Actions already completed are not affected by hello terminate action.</span></span>

|<span data-ttu-id="87f81-559">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87f81-559">Name</span></span>|<span data-ttu-id="87f81-560">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-560">Required</span></span>|<span data-ttu-id="87f81-561">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-561">Type</span></span>|<span data-ttu-id="87f81-562">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-562">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="87f81-563">runStatus</span><span class="sxs-lookup"><span data-stu-id="87f81-563">runStatus</span></span>|<span data-ttu-id="87f81-564">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-564">Yes</span></span>|<span data-ttu-id="87f81-565">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-565">String</span></span>|<span data-ttu-id="87f81-566">cíl Hello spustit stav.</span><span class="sxs-lookup"><span data-stu-id="87f81-566">hello target run status.</span></span> <span data-ttu-id="87f81-567">Buď **se nezdařilo** nebo **zrušena**.</span><span class="sxs-lookup"><span data-stu-id="87f81-567">Either **Failed** or **Cancelled**.</span></span>|
|<span data-ttu-id="87f81-568">runError</span><span class="sxs-lookup"><span data-stu-id="87f81-568">runError</span></span>|<span data-ttu-id="87f81-569">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-569">No</span></span>|<span data-ttu-id="87f81-570">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-570">Object</span></span>|<span data-ttu-id="87f81-571">Podrobnosti o chybě Hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-571">hello error details.</span></span> <span data-ttu-id="87f81-572">Když podporována pouze **runStatus** je nastaven příliš**se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="87f81-572">Only supported when **runStatus** is set too**Failed**.</span></span>|
|<span data-ttu-id="87f81-573">runError kódu</span><span class="sxs-lookup"><span data-stu-id="87f81-573">runError code</span></span>|<span data-ttu-id="87f81-574">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-574">No</span></span>|<span data-ttu-id="87f81-575">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-575">String</span></span>|<span data-ttu-id="87f81-576">Hello spustit kód chyby.</span><span class="sxs-lookup"><span data-stu-id="87f81-576">hello run error code.</span></span>|
|<span data-ttu-id="87f81-577">zpráva runError</span><span class="sxs-lookup"><span data-stu-id="87f81-577">runError message</span></span>|<span data-ttu-id="87f81-578">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-578">No</span></span>|<span data-ttu-id="87f81-579">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-579">String</span></span>|<span data-ttu-id="87f81-580">Hello spustit chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="87f81-580">hello run error message.</span></span>|

## <a name="compose-action"></a><span data-ttu-id="87f81-581">Vytvořit akce</span><span class="sxs-lookup"><span data-stu-id="87f81-581">Compose action</span></span>

<span data-ttu-id="87f81-582">Hello vytvářené akce umožňuje vytvořit libovolný objekt.</span><span class="sxs-lookup"><span data-stu-id="87f81-582">hello Compose action lets you construct an arbitrary object.</span></span> <span data-ttu-id="87f81-583">výstup Hello hello tvoří akce je hello výsledek vyhodnocení vstupy.</span><span class="sxs-lookup"><span data-stu-id="87f81-583">hello output of hello compose action is hello result of evaluating its inputs.</span></span> <span data-ttu-id="87f81-584">Například můžete použít hello tvoří akce toomerge výstupy několik akcí:</span><span class="sxs-lookup"><span data-stu-id="87f81-584">For example, you can use hello compose action toomerge outputs of multiple actions:</span></span>

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
> <span data-ttu-id="87f81-585">Hello **vytvářené** akce lze použít tooconstruct žádný výstup, včetně objektů, pole a jiný typ nativně podporuje logiku aplikace jako soubor XML a binární.</span><span class="sxs-lookup"><span data-stu-id="87f81-585">hello **Compose** action can be used tooconstruct any output, including objects, arrays, and any other type natively supported by logic apps like XML and binary.</span></span>

## <a name="table-action"></a><span data-ttu-id="87f81-586">Tabulka akcí</span><span class="sxs-lookup"><span data-stu-id="87f81-586">Table action</span></span>

<span data-ttu-id="87f81-587">Hello `table` vám umožní tooconvert na pole položek do **CSV** nebo **HTML** tabulky.</span><span class="sxs-lookup"><span data-stu-id="87f81-587">hello `table` allows you tooconvert an array of items into a **CSV** or **HTML** table.</span></span>

<span data-ttu-id="87f81-588">Předpokládejme, že @triggerBodyje)</span><span class="sxs-lookup"><span data-stu-id="87f81-588">Suppose @triggerBody() is</span></span>

```json
[{
  "id": 0,
  "name": "apples"
},{
  "id": 1, 
  "name": "oranges"
}]
```

<span data-ttu-id="87f81-589">A mohli být definován jako akce hello</span><span class="sxs-lookup"><span data-stu-id="87f81-589">And let hello action be defined as</span></span>

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

<span data-ttu-id="87f81-590">Vytvoří Hello výše</span><span class="sxs-lookup"><span data-stu-id="87f81-590">hello above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="87f81-591">id</span><span class="sxs-lookup"><span data-stu-id="87f81-591">id</span></span></th><th><span data-ttu-id="87f81-592">jméno</span><span class="sxs-lookup"><span data-stu-id="87f81-592">name</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="87f81-593">0</span><span class="sxs-lookup"><span data-stu-id="87f81-593">0</span></span></td><td><span data-ttu-id="87f81-594">jablka</span><span class="sxs-lookup"><span data-stu-id="87f81-594">apples</span></span></td></tr><tr><td><span data-ttu-id="87f81-595">1</span><span class="sxs-lookup"><span data-stu-id="87f81-595">1</span></span></td><td><span data-ttu-id="87f81-596">Pomeranče</span><span class="sxs-lookup"><span data-stu-id="87f81-596">oranges</span></span></td></tr></tbody></table><span data-ttu-id="87f81-597">"</span><span class="sxs-lookup"><span data-stu-id="87f81-597">"</span></span>

<span data-ttu-id="87f81-598">Tabulka hello toocustomize pořadí můžete zadat hello sloupce explicitně.</span><span class="sxs-lookup"><span data-stu-id="87f81-598">In order toocustomize hello table, you can specify hello columns explicitly.</span></span> <span data-ttu-id="87f81-599">Například:</span><span class="sxs-lookup"><span data-stu-id="87f81-599">For example:</span></span>

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

<span data-ttu-id="87f81-600">Vytvoří Hello výše</span><span class="sxs-lookup"><span data-stu-id="87f81-600">hello above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="87f81-601">vytvoření id</span><span class="sxs-lookup"><span data-stu-id="87f81-601">produce id</span></span></th><th><span data-ttu-id="87f81-602">description</span><span class="sxs-lookup"><span data-stu-id="87f81-602">description</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="87f81-603">0</span><span class="sxs-lookup"><span data-stu-id="87f81-603">0</span></span></td><td><span data-ttu-id="87f81-604">Čerstvá jablka</span><span class="sxs-lookup"><span data-stu-id="87f81-604">fresh apples</span></span></td></tr><tr><td><span data-ttu-id="87f81-605">1</span><span class="sxs-lookup"><span data-stu-id="87f81-605">1</span></span></td><td><span data-ttu-id="87f81-606">čerstvé pomeranče</span><span class="sxs-lookup"><span data-stu-id="87f81-606">fresh oranges</span></span></td></tr></tbody></table><span data-ttu-id="87f81-607">"</span><span class="sxs-lookup"><span data-stu-id="87f81-607">"</span></span>

<span data-ttu-id="87f81-608">Pokud hello `from` hodnota vlastnosti je prázdné pole, výstup hello je prázdná tabulka.</span><span class="sxs-lookup"><span data-stu-id="87f81-608">If hello `from` property value is an empty array, hello output is an empty table.</span></span>

|<span data-ttu-id="87f81-609">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87f81-609">Name</span></span>|<span data-ttu-id="87f81-610">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-610">Required</span></span>|<span data-ttu-id="87f81-611">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-611">Type</span></span>|<span data-ttu-id="87f81-612">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-612">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="87f81-613">Z</span><span class="sxs-lookup"><span data-stu-id="87f81-613">from</span></span>|<span data-ttu-id="87f81-614">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-614">Yes</span></span>|<span data-ttu-id="87f81-615">Pole</span><span class="sxs-lookup"><span data-stu-id="87f81-615">Array</span></span>|<span data-ttu-id="87f81-616">Hello zdrojové pole.</span><span class="sxs-lookup"><span data-stu-id="87f81-616">hello source array.</span></span>|
|<span data-ttu-id="87f81-617">Formát</span><span class="sxs-lookup"><span data-stu-id="87f81-617">format</span></span>|<span data-ttu-id="87f81-618">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-618">Yes</span></span>|<span data-ttu-id="87f81-619">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-619">String</span></span>|<span data-ttu-id="87f81-620">Hello formátu, buď **CSV** nebo **HTML**.</span><span class="sxs-lookup"><span data-stu-id="87f81-620">hello format, either **CSV** or **HTML**.</span></span>|
|<span data-ttu-id="87f81-621">sloupce</span><span class="sxs-lookup"><span data-stu-id="87f81-621">columns</span></span>|<span data-ttu-id="87f81-622">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-622">No</span></span>|<span data-ttu-id="87f81-623">Pole</span><span class="sxs-lookup"><span data-stu-id="87f81-623">Array</span></span>|<span data-ttu-id="87f81-624">Hello sloupce.</span><span class="sxs-lookup"><span data-stu-id="87f81-624">hello columns.</span></span> <span data-ttu-id="87f81-625">Umožňuje toooverride hello výchozí tvar hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="87f81-625">Allows toooverride hello default shape of hello table.</span></span>|
|<span data-ttu-id="87f81-626">záhlaví sloupce</span><span class="sxs-lookup"><span data-stu-id="87f81-626">column header</span></span>|<span data-ttu-id="87f81-627">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-627">No</span></span>|<span data-ttu-id="87f81-628">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-628">String</span></span>|<span data-ttu-id="87f81-629">Hello záhlaví sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-629">hello header of hello column.</span></span>|
|<span data-ttu-id="87f81-630">Hodnota sloupce</span><span class="sxs-lookup"><span data-stu-id="87f81-630">column value</span></span>|<span data-ttu-id="87f81-631">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-631">Yes</span></span>|<span data-ttu-id="87f81-632">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-632">String</span></span>|<span data-ttu-id="87f81-633">Hodnota Hello hello sloupce.</span><span class="sxs-lookup"><span data-stu-id="87f81-633">hello value of hello column.</span></span>|

## <a name="workflow-action"></a><span data-ttu-id="87f81-634">Akce pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="87f81-634">Workflow action</span></span>   

|<span data-ttu-id="87f81-635">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87f81-635">Name</span></span>|<span data-ttu-id="87f81-636">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-636">Required</span></span>|<span data-ttu-id="87f81-637">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-637">Type</span></span>|<span data-ttu-id="87f81-638">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-638">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="87f81-639">id hostitele</span><span class="sxs-lookup"><span data-stu-id="87f81-639">host id</span></span>|<span data-ttu-id="87f81-640">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-640">Yes</span></span>|<span data-ttu-id="87f81-641">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-641">String</span></span>|<span data-ttu-id="87f81-642">ID prostředku Hello hello pracovního postupu, které chcete toocall.</span><span class="sxs-lookup"><span data-stu-id="87f81-642">hello resource ID of hello workflow that you want toocall.</span></span>|  
|<span data-ttu-id="87f81-643">Název aktivační události hostitele</span><span class="sxs-lookup"><span data-stu-id="87f81-643">host triggerName</span></span>|<span data-ttu-id="87f81-644">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-644">Yes</span></span>|<span data-ttu-id="87f81-645">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-645">String</span></span>|<span data-ttu-id="87f81-646">Název Hello hello aktivační události, které chcete tooinvoke.</span><span class="sxs-lookup"><span data-stu-id="87f81-646">hello name of hello trigger that you want tooinvoke.</span></span>|  
|<span data-ttu-id="87f81-647">Dotazy</span><span class="sxs-lookup"><span data-stu-id="87f81-647">queries</span></span>|<span data-ttu-id="87f81-648">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-648">No</span></span>|<span data-ttu-id="87f81-649">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-649">Object</span></span>|<span data-ttu-id="87f81-650">Představuje hello dotazu parametry tooadd toohello URL.</span><span class="sxs-lookup"><span data-stu-id="87f81-650">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="87f81-651">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="87f81-651">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="87f81-652">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="87f81-652">headers</span></span>|<span data-ttu-id="87f81-653">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-653">No</span></span>|<span data-ttu-id="87f81-654">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-654">Object</span></span>|<span data-ttu-id="87f81-655">Představuje všechny hello hlavičky, které je odeslána žádost o toohello.</span><span class="sxs-lookup"><span data-stu-id="87f81-655">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="87f81-656">Například tooset hello jazyk a typ na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="87f81-656">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="87f81-657">Text</span><span class="sxs-lookup"><span data-stu-id="87f81-657">body</span></span>|<span data-ttu-id="87f81-658">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-658">No</span></span>|<span data-ttu-id="87f81-659">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-659">Object</span></span>|<span data-ttu-id="87f81-660">Představuje hello datová část odeslaná toohello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="87f81-660">Represents hello payload sent toohello endpoint.</span></span>|  
  
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
  
<span data-ttu-id="87f81-661">Kontrolu přístupu se provádí v pracovním postupu hello \(přesněji řečeno, aktivační události hello\), což znamená, že potřebujete přístup toohello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="87f81-661">An access check is made on hello workflow \(more specifically, hello trigger\), meaning you need access toohello workflow.</span></span>  
  
<span data-ttu-id="87f81-662">Hello výstupy z hello `workflow` akce jsou založené na definované v hello `response` akce v pracovním postupu podřízené hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-662">hello outputs from hello `workflow` action are based on what you defined in hello `response` action in hello child workflow.</span></span> <span data-ttu-id="87f81-663">Pokud nebyly definovány žádné `response` akce a potom hello výstupy jsou prázdné.</span><span class="sxs-lookup"><span data-stu-id="87f81-663">If you have not defined any `response` action, then hello outputs are empty.</span></span>  

## <a name="function-action"></a><span data-ttu-id="87f81-664">Akce – funkce</span><span class="sxs-lookup"><span data-stu-id="87f81-664">Function action</span></span>   

|<span data-ttu-id="87f81-665">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87f81-665">Name</span></span>|<span data-ttu-id="87f81-666">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-666">Required</span></span>|<span data-ttu-id="87f81-667">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-667">Type</span></span>|<span data-ttu-id="87f81-668">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-668">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="87f81-669">id – funkce</span><span class="sxs-lookup"><span data-stu-id="87f81-669">function id</span></span>|<span data-ttu-id="87f81-670">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-670">Yes</span></span>|<span data-ttu-id="87f81-671">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-671">String</span></span>|<span data-ttu-id="87f81-672">ID prostředku Hello hello funkce, které chcete tooinvoke.</span><span class="sxs-lookup"><span data-stu-id="87f81-672">hello resource ID of hello function that you want tooinvoke.</span></span>|  
|<span data-ttu-id="87f81-673">– Metoda</span><span class="sxs-lookup"><span data-stu-id="87f81-673">method</span></span>|<span data-ttu-id="87f81-674">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-674">No</span></span>|<span data-ttu-id="87f81-675">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-675">String</span></span>|<span data-ttu-id="87f81-676">Metoda HTTP Hello používá tooinvoke hello funkce.</span><span class="sxs-lookup"><span data-stu-id="87f81-676">hello HTTP method used tooinvoke hello function.</span></span> <span data-ttu-id="87f81-677">Ve výchozím nastavení, je `POST` není-li zadána.</span><span class="sxs-lookup"><span data-stu-id="87f81-677">By default, it is `POST` when not specified.</span></span>|  
|<span data-ttu-id="87f81-678">Dotazy</span><span class="sxs-lookup"><span data-stu-id="87f81-678">queries</span></span>|<span data-ttu-id="87f81-679">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-679">No</span></span>|<span data-ttu-id="87f81-680">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-680">Object</span></span>|<span data-ttu-id="87f81-681">Představuje hello dotazu parametry tooadd toohello URL.</span><span class="sxs-lookup"><span data-stu-id="87f81-681">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="87f81-682">Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="87f81-682">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="87f81-683">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="87f81-683">headers</span></span>|<span data-ttu-id="87f81-684">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-684">No</span></span>|<span data-ttu-id="87f81-685">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-685">Object</span></span>|<span data-ttu-id="87f81-686">Představuje všechny hello hlavičky, které je odeslána žádost o toohello.</span><span class="sxs-lookup"><span data-stu-id="87f81-686">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="87f81-687">Například tooset hello jazyk a typ na vyžádání: `"headers" : { "Accept-Language": "en-us" }`.</span><span class="sxs-lookup"><span data-stu-id="87f81-687">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us" }`.</span></span>|  
|<span data-ttu-id="87f81-688">Text</span><span class="sxs-lookup"><span data-stu-id="87f81-688">body</span></span>|<span data-ttu-id="87f81-689">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-689">No</span></span>|<span data-ttu-id="87f81-690">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-690">Object</span></span>|<span data-ttu-id="87f81-691">Představuje hello datová část odeslaná toohello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="87f81-691">Represents hello payload sent toohello endpoint.</span></span>|  

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

<span data-ttu-id="87f81-692">Při ukládání aplikace logiky hello jsme provést nějaké kontroly na hello odkazuje funkce:</span><span class="sxs-lookup"><span data-stu-id="87f81-692">When you save hello logic app, we perform some checks on hello referenced function:</span></span>
-   <span data-ttu-id="87f81-693">Je nutné funkce toohello toohave přístup.</span><span class="sxs-lookup"><span data-stu-id="87f81-693">You need toohave access toohello function.</span></span>
-   <span data-ttu-id="87f81-694">Jenom je povolen standard triggeru protokolu HTTP nebo obecné aktivační události webhooku JSON.</span><span class="sxs-lookup"><span data-stu-id="87f81-694">Only standard HTTP trigger or generic JSON webhook trigger is allowed.</span></span>
-   <span data-ttu-id="87f81-695">Všechny trasy definované by neměl mít.</span><span class="sxs-lookup"><span data-stu-id="87f81-695">It should not have any route defined.</span></span>
-   <span data-ttu-id="87f81-696">Pouze "funkce" a "anonymní" oprávnění na úrovni je povolen.</span><span class="sxs-lookup"><span data-stu-id="87f81-696">Only "function" and "anonymous" authorization level is allowed.</span></span>

<span data-ttu-id="87f81-697">Adresa URL Hello aktivační události je načíst, do mezipaměti a použít za běhu.</span><span class="sxs-lookup"><span data-stu-id="87f81-697">hello trigger URL is retrieved, cached, and used at runtime.</span></span> <span data-ttu-id="87f81-698">Takže pokud všechny operace by způsobila neplatnost URL hello do mezipaměti, hello akce selže za běhu.</span><span class="sxs-lookup"><span data-stu-id="87f81-698">So if any operation invalidates hello cached URL, hello action fails at runtime.</span></span> <span data-ttu-id="87f81-699">toowork kolem toho uložte hello aplikace logiky akci, která bude způsobit tooretrieve aplikace logiky a mezipaměti hello aktivační událost URL znovu.</span><span class="sxs-lookup"><span data-stu-id="87f81-699">toowork around this, save hello logic app again, which will cause logic app tooretrieve and cache hello trigger URL again.</span></span>

## <a name="collection-actions-scopes-and-loops"></a><span data-ttu-id="87f81-700">Kolekce akce (rozsahy a smyčky)</span><span class="sxs-lookup"><span data-stu-id="87f81-700">Collection actions (scopes and loops)</span></span>

<span data-ttu-id="87f81-701">Některé typy akcí může obsahovat akce v rámci sami.</span><span class="sxs-lookup"><span data-stu-id="87f81-701">Some action types can contain actions within themselves.</span></span> <span data-ttu-id="87f81-702">Odkaz akce v rámci kolekce může být odkaz přímo mimo hello kolekce.</span><span class="sxs-lookup"><span data-stu-id="87f81-702">Reference actions within a collection can be referenced directly outside of hello collection.</span></span> <span data-ttu-id="87f81-703">Pokud jste definovali `http` v oboru, `@body('http')` je stále platný kdekoli v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="87f81-703">If you defined `http` in a scope, `@body('http')` is still valid anywhere in a workflow.</span></span> <span data-ttu-id="87f81-704">Akce v rámci kolekce můžete `runAfter` hello jenom jiné akce v rámci stejné kolekci.</span><span class="sxs-lookup"><span data-stu-id="87f81-704">Actions within a collection can `runAfter` only other actions within hello same collection.</span></span>

## <a name="scope-action"></a><span data-ttu-id="87f81-705">Akce oboru</span><span class="sxs-lookup"><span data-stu-id="87f81-705">Scope action</span></span>

<span data-ttu-id="87f81-706">Hello `scope` akce umožňuje logicky akce skupiny v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="87f81-706">hello `scope` action lets you logically group actions in a workflow.</span></span>

|<span data-ttu-id="87f81-707">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87f81-707">Name</span></span>|<span data-ttu-id="87f81-708">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-708">Required</span></span>|<span data-ttu-id="87f81-709">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-709">Type</span></span>|<span data-ttu-id="87f81-710">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-710">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="87f81-711">Akce</span><span class="sxs-lookup"><span data-stu-id="87f81-711">actions</span></span>|<span data-ttu-id="87f81-712">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-712">Yes</span></span>|<span data-ttu-id="87f81-713">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-713">Object</span></span>|<span data-ttu-id="87f81-714">Tooexecute vnitřní akce v rámci oboru hello</span><span class="sxs-lookup"><span data-stu-id="87f81-714">Inner actions tooexecute within hello scope</span></span>|

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

## <a name="foreach-action"></a><span data-ttu-id="87f81-715">Akce ForEach</span><span class="sxs-lookup"><span data-stu-id="87f81-715">ForEach action</span></span>

<span data-ttu-id="87f81-716">Tato akce opakování iteruje v rámci pole a provádí vnitřní akce pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="87f81-716">This looping action iterates through an array and performs inner actions for each item.</span></span> <span data-ttu-id="87f81-717">Ve výchozím nastavení smyčka typu foreach hello spustí paralelně (20 spuštěních paralelně v čase).</span><span class="sxs-lookup"><span data-stu-id="87f81-717">By default, hello foreach loop executes in parallel (20 executions in parallel at a time).</span></span> <span data-ttu-id="87f81-718">Budete moct nastavit pravidla spouštění pomocí hello `operationOptions` parametr.</span><span class="sxs-lookup"><span data-stu-id="87f81-718">You can set execution rules using hello `operationOptions` parameter.</span></span>

|<span data-ttu-id="87f81-719">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87f81-719">Name</span></span>|<span data-ttu-id="87f81-720">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-720">Required</span></span>|<span data-ttu-id="87f81-721">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-721">Type</span></span>|<span data-ttu-id="87f81-722">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-722">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="87f81-723">Akce</span><span class="sxs-lookup"><span data-stu-id="87f81-723">actions</span></span>|<span data-ttu-id="87f81-724">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-724">Yes</span></span>|<span data-ttu-id="87f81-725">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-725">Object</span></span>|<span data-ttu-id="87f81-726">Tooexecute vnitřní akce v rámci hello smyčky</span><span class="sxs-lookup"><span data-stu-id="87f81-726">Inner actions tooexecute within hello loop</span></span>|
|<span data-ttu-id="87f81-727">foreach</span><span class="sxs-lookup"><span data-stu-id="87f81-727">foreach</span></span>|<span data-ttu-id="87f81-728">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-728">Yes</span></span>|<span data-ttu-id="87f81-729">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-729">string</span></span>|<span data-ttu-id="87f81-730">pole tooiterate Hello přes</span><span class="sxs-lookup"><span data-stu-id="87f81-730">hello array tooiterate over</span></span>|
|<span data-ttu-id="87f81-731">operationOptions</span><span class="sxs-lookup"><span data-stu-id="87f81-731">operationOptions</span></span>|<span data-ttu-id="87f81-732">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-732">no</span></span>|<span data-ttu-id="87f81-733">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-733">string</span></span>|<span data-ttu-id="87f81-734">Všechny operace možnosti chování.</span><span class="sxs-lookup"><span data-stu-id="87f81-734">Any operation options for behavior.</span></span> <span data-ttu-id="87f81-735">Aktuálně podporuje jenom `sequential` tooexecute iterací postupně (výchozí chování je paralelní)</span><span class="sxs-lookup"><span data-stu-id="87f81-735">Currently only supports `sequential` tooexecute iterations sequentially (default behavior is parallel)</span></span>|

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

## <a name="until-action"></a><span data-ttu-id="87f81-736">Dokud akce</span><span class="sxs-lookup"><span data-stu-id="87f81-736">Until action</span></span>

<span data-ttu-id="87f81-737">Tato opakování akce provede vnitřní akce, dokud podmínku výsledků tootrue.</span><span class="sxs-lookup"><span data-stu-id="87f81-737">This looping action executes inner actions until a condition results tootrue.</span></span>

|<span data-ttu-id="87f81-738">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87f81-738">Name</span></span>|<span data-ttu-id="87f81-739">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-739">Required</span></span>|<span data-ttu-id="87f81-740">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-740">Type</span></span>|<span data-ttu-id="87f81-741">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-741">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="87f81-742">Akce</span><span class="sxs-lookup"><span data-stu-id="87f81-742">actions</span></span>|<span data-ttu-id="87f81-743">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-743">Yes</span></span>|<span data-ttu-id="87f81-744">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-744">Object</span></span>|<span data-ttu-id="87f81-745">Tooexecute vnitřní akce v rámci hello smyčky</span><span class="sxs-lookup"><span data-stu-id="87f81-745">Inner actions tooexecute within hello loop</span></span>|
|<span data-ttu-id="87f81-746">výraz</span><span class="sxs-lookup"><span data-stu-id="87f81-746">expression</span></span>|<span data-ttu-id="87f81-747">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-747">Yes</span></span>|<span data-ttu-id="87f81-748">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-748">string</span></span>|<span data-ttu-id="87f81-749">výraz tooevaluate Hello po každé iteraci</span><span class="sxs-lookup"><span data-stu-id="87f81-749">hello expression tooevaluate after each iteration</span></span>|
|<span data-ttu-id="87f81-750">Limit</span><span class="sxs-lookup"><span data-stu-id="87f81-750">limit</span></span>|<span data-ttu-id="87f81-751">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-751">yes</span></span>|<span data-ttu-id="87f81-752">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-752">Object</span></span>|<span data-ttu-id="87f81-753">Hello limity pro hello smyčku – žádný limit musí být definován.</span><span class="sxs-lookup"><span data-stu-id="87f81-753">hello limits for hello loop - at least one limit must be defined</span></span>|
|<span data-ttu-id="87f81-754">Počet</span><span class="sxs-lookup"><span data-stu-id="87f81-754">count</span></span>|<span data-ttu-id="87f81-755">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-755">no</span></span>|<span data-ttu-id="87f81-756">celá čísla</span><span class="sxs-lookup"><span data-stu-id="87f81-756">int</span></span>|<span data-ttu-id="87f81-757">Hello limit toohello počet iterací, které lze provést</span><span class="sxs-lookup"><span data-stu-id="87f81-757">hello limit toohello number of iterations that can be performed</span></span>|
|<span data-ttu-id="87f81-758">timeout</span><span class="sxs-lookup"><span data-stu-id="87f81-758">timeout</span></span>|<span data-ttu-id="87f81-759">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-759">no</span></span>|<span data-ttu-id="87f81-760">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-760">string</span></span>|<span data-ttu-id="87f81-761">Hello časový limit pro jak dlouho by měl cykly.</span><span class="sxs-lookup"><span data-stu-id="87f81-761">hello timeout for how long it should loop.</span></span>  <span data-ttu-id="87f81-762">Formátu ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="87f81-762">ISO 8601 format</span></span>|


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

## <a name="conditions---if-action"></a><span data-ttu-id="87f81-763">Podmínky - li akce</span><span class="sxs-lookup"><span data-stu-id="87f81-763">Conditions - If Action</span></span>

<span data-ttu-id="87f81-764">Hello `If` akce umožňuje vyhodnotit podmínku a provést větev založené na tom, zda text hello výraz vyhodnocen jako příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="87f81-764">hello `If` action lets you evaluate a condition and execute a branch based on whether hello expression evaluates too`true`.</span></span>

|<span data-ttu-id="87f81-765">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="87f81-765">Name</span></span>|<span data-ttu-id="87f81-766">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="87f81-766">Required</span></span>|<span data-ttu-id="87f81-767">Typ</span><span class="sxs-lookup"><span data-stu-id="87f81-767">Type</span></span>|<span data-ttu-id="87f81-768">Popis</span><span class="sxs-lookup"><span data-stu-id="87f81-768">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="87f81-769">Akce</span><span class="sxs-lookup"><span data-stu-id="87f81-769">actions</span></span>|<span data-ttu-id="87f81-770">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-770">Yes</span></span>|<span data-ttu-id="87f81-771">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-771">Object</span></span>|<span data-ttu-id="87f81-772">Vnitřní akce tooexecute, pokud se výraz vyhodnotí jako příliš`true`</span><span class="sxs-lookup"><span data-stu-id="87f81-772">Inner actions tooexecute when expression evaluates too`true`</span></span>|
|<span data-ttu-id="87f81-773">výraz</span><span class="sxs-lookup"><span data-stu-id="87f81-773">expression</span></span>|<span data-ttu-id="87f81-774">Ano</span><span class="sxs-lookup"><span data-stu-id="87f81-774">Yes</span></span>|<span data-ttu-id="87f81-775">Řetězec</span><span class="sxs-lookup"><span data-stu-id="87f81-775">string</span></span>|<span data-ttu-id="87f81-776">výraz tooevaluate Hello</span><span class="sxs-lookup"><span data-stu-id="87f81-776">hello expression tooevaluate</span></span>|
|<span data-ttu-id="87f81-777">else</span><span class="sxs-lookup"><span data-stu-id="87f81-777">else</span></span>|<span data-ttu-id="87f81-778">Ne</span><span class="sxs-lookup"><span data-stu-id="87f81-778">no</span></span>|<span data-ttu-id="87f81-779">Objekt</span><span class="sxs-lookup"><span data-stu-id="87f81-779">Object</span></span>|<span data-ttu-id="87f81-780">Vnitřní akce tooexecute, pokud se výraz vyhodnotí jako příliš`false`</span><span class="sxs-lookup"><span data-stu-id="87f81-780">Inner actions tooexecute when expression evaluates too`false`</span></span>|
  
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
  
<span data-ttu-id="87f81-781">Hello následující tabulka uvádí příklady jak podmínky použití výrazů v akci:</span><span class="sxs-lookup"><span data-stu-id="87f81-781">hello following table shows examples of how conditions can use expressions in an action:</span></span>  
  
|<span data-ttu-id="87f81-782">Hodnota JSON</span><span class="sxs-lookup"><span data-stu-id="87f81-782">JSON value</span></span>|<span data-ttu-id="87f81-783">výsledek</span><span class="sxs-lookup"><span data-stu-id="87f81-783">Result</span></span>|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|<span data-ttu-id="87f81-784">Libovolná hodnota, které by vyhodnocovaly tootrue způsobí, že tato podmínka toopass.</span><span class="sxs-lookup"><span data-stu-id="87f81-784">Any value that would evaluate tootrue causes this condition toopass.</span></span> <span data-ttu-id="87f81-785">Podporovány jsou pouze výrazy logických hodnot.</span><span class="sxs-lookup"><span data-stu-id="87f81-785">Only Boolean expressions are supported.</span></span> <span data-ttu-id="87f81-786">tooconvert jiné typy tooBoolean, použití funkcí `empty`, `equals`.</span><span class="sxs-lookup"><span data-stu-id="87f81-786">tooconvert other types tooBoolean, use functions `empty`, `equals`.</span></span>|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|<span data-ttu-id="87f81-787">Porovnání funkcí jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="87f81-787">Comparison functions are supported.</span></span> <span data-ttu-id="87f81-788">Například hello zde hello akce jenom provede, když se výstup hello act1 je větší než prahová hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="87f81-788">For hello example here, hello action only executes when hello output of act1 is greater than hello threshold.</span></span>|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|<span data-ttu-id="87f81-789">Funkce logiky jsou taky podporované toocreate vnořené výrazy logických hodnot.</span><span class="sxs-lookup"><span data-stu-id="87f81-789">Logic functions are also supported toocreate nested Boolean expressions.</span></span> <span data-ttu-id="87f81-790">V takovém případě hello akce provede, když se výstup hello act1 je nad prahovou hodnotou hello nebo nižší než 100.</span><span class="sxs-lookup"><span data-stu-id="87f81-790">In this case, hello action executes when hello output of act1 is above hello threshold or below 100.</span></span>|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|<span data-ttu-id="87f81-791">Pokud pole má všechny položky, můžete použít funkce toocheck pole.</span><span class="sxs-lookup"><span data-stu-id="87f81-791">You can use array functions toocheck if an array has any items.</span></span> <span data-ttu-id="87f81-792">V takovém případě hello akce provede, když se pole chyby hello je prázdný.</span><span class="sxs-lookup"><span data-stu-id="87f81-792">In this case, hello action executes when hello errors array is empty.</span></span>| 
|`"expression": "parameters('hasSpecialAction')"`|<span data-ttu-id="87f81-793">Chyba: není platný podmínky, protože @ je vyžadován pro podmínky.</span><span class="sxs-lookup"><span data-stu-id="87f81-793">Error - not a valid condition because @ is required for conditions.</span></span>|  
  
<span data-ttu-id="87f81-794">Pokud je podmínka vyhodnocena jako úspěšně, hello podmínky je označen jako `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="87f81-794">If a condition evaluates successfully, hello condition is marked as `Succeeded`.</span></span> <span data-ttu-id="87f81-795">Akce v rámci buď hello `actions` nebo `else` objekty vyhodnocení příliš`Succeeded` když spustí a byly úspěšné, `Failed` při provést a selhala, nebo `Skipped` Pokud není uvedené pobočky provést.</span><span class="sxs-lookup"><span data-stu-id="87f81-795">Actions within either hello `actions` or `else` objects evaluate too`Succeeded` when executed and succeeded, `Failed` when executed and failed, or `Skipped` when that branch is not executed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87f81-796">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87f81-796">Next steps</span></span>

[<span data-ttu-id="87f81-797">Rozhraní API REST služby pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="87f81-797">Workflow Service REST API</span></span>](https://docs.microsoft.com/rest/api/logic/workflows)
