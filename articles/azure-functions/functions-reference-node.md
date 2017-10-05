---
title: "Referenční informace pro vývojáře JavaScript pro Azure Functions | Microsoft Docs"
description: "Pochopit, jak vyvíjet funkce pomocí jazyka JavaScript."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: functions
ms.devlang: nodejs
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: 7ea81ed47f391fbce1432c2b11ac176ab6c04ae0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-javascript-developer-guide"></a><span data-ttu-id="f50e0-104">Příručka vývojáře Azure funkce JavaScript</span><span class="sxs-lookup"><span data-stu-id="f50e0-104">Azure Functions JavaScript developer guide</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f50e0-105">Skript jazyka C#</span><span class="sxs-lookup"><span data-stu-id="f50e0-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="f50e0-106">F # skript</span><span class="sxs-lookup"><span data-stu-id="f50e0-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="f50e0-107">JavaScript</span><span class="sxs-lookup"><span data-stu-id="f50e0-107">JavaScript</span></span>](functions-reference-node.md)
> 
> 

<span data-ttu-id="f50e0-108">Možnosti JavaScript Azure Functions umožňuje snadno exportovat funkci, která se předá jako `context` objekt pro komunikaci s modulem runtime a pro příjem a odesílání dat přes vazby.</span><span class="sxs-lookup"><span data-stu-id="f50e0-108">The JavaScript experience for Azure Functions makes it easy to export a function, which is passed as a `context` object for communicating with the runtime and for receiving and sending data via bindings.</span></span>

<span data-ttu-id="f50e0-109">Tento článek předpokládá, že jste si již přečetli [referenční informace pro vývojáře Azure Functions](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="f50e0-109">This article assumes that you've already read the [Azure Functions developer reference](functions-reference.md).</span></span>

## <a name="exporting-a-function"></a><span data-ttu-id="f50e0-110">Export funkce</span><span class="sxs-lookup"><span data-stu-id="f50e0-110">Exporting a function</span></span>
<span data-ttu-id="f50e0-111">Všechny funkce jazyka JavaScript, musíte exportovat jedné `function` prostřednictvím `module.exports` pro modul runtime vyhledat funkci a spustit ho.</span><span class="sxs-lookup"><span data-stu-id="f50e0-111">All JavaScript functions must export a single `function` via `module.exports` for the runtime to find the function and run it.</span></span> <span data-ttu-id="f50e0-112">Tato funkce musí vždy zahrnovat `context` objektu.</span><span class="sxs-lookup"><span data-stu-id="f50e0-112">This function must always include a `context` object.</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

<span data-ttu-id="f50e0-113">Vazeb `direction === "in"` se předají jako argumenty funkce, což znamená, že můžete používat [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) dynamicky zpracovat nové vstupy (například pomocí `arguments.length` Iterujte přes všechny vstupy).</span><span class="sxs-lookup"><span data-stu-id="f50e0-113">Bindings of `direction === "in"` are passed along as function arguments, which means that you can use [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) to dynamically handle new inputs (for example, by using `arguments.length` to iterate over all your inputs).</span></span> <span data-ttu-id="f50e0-114">Tato funkce je vhodné, pokud máte pouze aktivační událost a žádné další vstupy, protože vaše data aktivační události můžete předvídatelné přístup bez odkazující na váš `context` objektu.</span><span class="sxs-lookup"><span data-stu-id="f50e0-114">This functionality is convenient when you have only a trigger and no additional inputs, because you can predictably access your trigger data without referencing your `context` object.</span></span>

<span data-ttu-id="f50e0-115">Argumenty, které jsou vždy funkci byl předán společně v pořadí, ve které se vyskytují v *function.json*i v případě, že je v příkazu exportuje nezadáte.</span><span class="sxs-lookup"><span data-stu-id="f50e0-115">The arguments are always passed along to the function in the order in which they occur in *function.json*, even if you don't specify them in your exports statement.</span></span> <span data-ttu-id="f50e0-116">Pokud máte například `function(context, a, b)` a změňte ji na `function(context, a)`, stále můžete získat hodnotu `b` v kódu funkce tím, že odkazuje na `arguments[3]`.</span><span class="sxs-lookup"><span data-stu-id="f50e0-116">For example, if you have `function(context, a, b)` and change it to `function(context, a)`, you can still get the value of `b` in function code by referring to `arguments[3]`.</span></span>

<span data-ttu-id="f50e0-117">Všechny vazby, bez ohledu na směru, jsou také předají `context` objektu (viz následující skript).</span><span class="sxs-lookup"><span data-stu-id="f50e0-117">All bindings, regardless of direction, are also passed along on the `context` object (see the following script).</span></span> 

## <a name="context-object"></a><span data-ttu-id="f50e0-118">objekt kontextu</span><span class="sxs-lookup"><span data-stu-id="f50e0-118">context object</span></span>
<span data-ttu-id="f50e0-119">Modul runtime používá `context` objekt k předávání dat do a z funkce a umožnit vám komunikovat s modulem runtime.</span><span class="sxs-lookup"><span data-stu-id="f50e0-119">The runtime uses a `context` object to pass data to and from your function and to let you communicate with the runtime.</span></span>

<span data-ttu-id="f50e0-120">Objekt kontextu je vždy první parametr funkce a musí být zahrnut, protože obsahuje metody, jako `context.done` a `context.log`, které jsou nutné k využití modulu runtime správně.</span><span class="sxs-lookup"><span data-stu-id="f50e0-120">The context object is always the first parameter to a function and must be included because it has methods such as `context.done` and `context.log`, which are required to use the runtime correctly.</span></span> <span data-ttu-id="f50e0-121">Můžete pojmenovat objekt ať chcete (například `ctx` nebo `c`).</span><span class="sxs-lookup"><span data-stu-id="f50e0-121">You can name the object whatever you would like (for example, `ctx` or `c`).</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a><span data-ttu-id="f50e0-122">Vlastnost Context.Bindings</span><span class="sxs-lookup"><span data-stu-id="f50e0-122">context.bindings property</span></span>

```
context.bindings
```
<span data-ttu-id="f50e0-123">Vrátí objekt s názvem, který obsahuje všechny vaše vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="f50e0-123">Returns a named object that contains all your input and output data.</span></span> <span data-ttu-id="f50e0-124">Například následující definice vazby v vaše *function.json* umožňuje přístup k obsahu z fronty `context.bindings.myInput` objektu.</span><span class="sxs-lookup"><span data-stu-id="f50e0-124">For example, the following binding definition in your *function.json* lets you access the contents of the queue from the `context.bindings.myInput` object.</span></span> 

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
}
```

```javascript
// myInput contains the input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

### <a name="contextdone-method"></a><span data-ttu-id="f50e0-125">Context.Done – metoda</span><span class="sxs-lookup"><span data-stu-id="f50e0-125">context.done method</span></span>
```
context.done([err],[propertyBag])
```

<span data-ttu-id="f50e0-126">Informuje o modul runtime, který váš kód byl dokončen.</span><span class="sxs-lookup"><span data-stu-id="f50e0-126">Informs the runtime that your code has finished.</span></span> <span data-ttu-id="f50e0-127">Je třeba volat `context.done`, nebo jinak se modul runtime nikdy ví, že funkce je dokončena a provádění bude časový limit.</span><span class="sxs-lookup"><span data-stu-id="f50e0-127">You must call `context.done`, or else the runtime never knows that your function is complete, and the execution will time out.</span></span> 

<span data-ttu-id="f50e0-128">`context.done` Metoda umožňuje předat zpět i uživatelem definované chybové modul runtime a kontejner objektů a vlastností, který přepíše vlastnosti na `context.bindings` objektu.</span><span class="sxs-lookup"><span data-stu-id="f50e0-128">The `context.done` method allows you to pass back both a user-defined error to the runtime and a property bag of properties that overwrite the properties on the `context.bindings` object.</span></span>

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a><span data-ttu-id="f50e0-129">Context.log – metoda</span><span class="sxs-lookup"><span data-stu-id="f50e0-129">context.log method</span></span>  

```
context.log(message)
```
<span data-ttu-id="f50e0-130">Umožňuje zapisovat do protokolů streamování konzoly na výchozí úrovni trasování.</span><span class="sxs-lookup"><span data-stu-id="f50e0-130">Allows you to write to the streaming console logs at the default trace level.</span></span> <span data-ttu-id="f50e0-131">Na `context.log`, další metody protokolování jsou k dispozici, který umožňuje zapisovat do protokolu konzoly na jiných úrovních trasování:</span><span class="sxs-lookup"><span data-stu-id="f50e0-131">On `context.log`, additional logging methods are available that let you write to the console log at other trace levels:</span></span>


| <span data-ttu-id="f50e0-132">Metoda</span><span class="sxs-lookup"><span data-stu-id="f50e0-132">Method</span></span>                 | <span data-ttu-id="f50e0-133">Popis</span><span class="sxs-lookup"><span data-stu-id="f50e0-133">Description</span></span>                                |
| ---------------------- | ------------------------------------------ |
| <span data-ttu-id="f50e0-134">**Chyba (_zpráva_)**</span><span class="sxs-lookup"><span data-stu-id="f50e0-134">**error(_message_)**</span></span>   | <span data-ttu-id="f50e0-135">Zapíše chyba úroveň protokolování nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="f50e0-135">Writes to error level logging, or lower.</span></span>   |
| <span data-ttu-id="f50e0-136">**warn (_zpráva_)**</span><span class="sxs-lookup"><span data-stu-id="f50e0-136">**warn(_message_)**</span></span>    | <span data-ttu-id="f50e0-137">Zapíše do varovná úroveň protokolování nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="f50e0-137">Writes to warning level logging, or lower.</span></span> |
| <span data-ttu-id="f50e0-138">**informace o (_zpráva_)**</span><span class="sxs-lookup"><span data-stu-id="f50e0-138">**info(_message_)**</span></span>    | <span data-ttu-id="f50e0-139">Zapíše informace o úroveň protokolování nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="f50e0-139">Writes to info level logging, or lower.</span></span>    |
| <span data-ttu-id="f50e0-140">**verbose (_zpráva_)**</span><span class="sxs-lookup"><span data-stu-id="f50e0-140">**verbose(_message_)**</span></span> | <span data-ttu-id="f50e0-141">Zapíše na podrobné úrovni protokolování.</span><span class="sxs-lookup"><span data-stu-id="f50e0-141">Writes to verbose level logging.</span></span>           |

<span data-ttu-id="f50e0-142">V následujícím příkladu se zapíše do konzoly na úrovni trasování upozornění:</span><span class="sxs-lookup"><span data-stu-id="f50e0-142">The following example writes to the console at the warning trace level:</span></span>

```javascript
context.log.warn("Something has happened."); 
```
<span data-ttu-id="f50e0-143">Můžete nastavit prahovou hodnotu úroveň trasování pro protokolování v souboru host.json nebo ho vypnout.</span><span class="sxs-lookup"><span data-stu-id="f50e0-143">You can set the trace-level threshold for logging in the host.json file, or turn it off.</span></span>  <span data-ttu-id="f50e0-144">Další informace o tom, jak zapsat do protokolů, najdete v další části.</span><span class="sxs-lookup"><span data-stu-id="f50e0-144">For more information about how to write to the logs, see the next section.</span></span>

## <a name="binding-data-type"></a><span data-ttu-id="f50e0-145">Datový typ vazby</span><span class="sxs-lookup"><span data-stu-id="f50e0-145">Binding data type</span></span>

<span data-ttu-id="f50e0-146">Datový typ pro vstupní vazbu, použijte `dataType` vlastnost v definici vazby.</span><span class="sxs-lookup"><span data-stu-id="f50e0-146">To define the data type for an input binding, use the `dataType` property in the binding definition.</span></span> <span data-ttu-id="f50e0-147">Například pokud chcete číst obsah požadavku HTTP v binárním formátu, použijte typ `binary`:</span><span class="sxs-lookup"><span data-stu-id="f50e0-147">For example, to read the content of an HTTP request in binary format, use the type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="f50e0-148">Další možnosti pro `dataType` jsou `stream` a `string`.</span><span class="sxs-lookup"><span data-stu-id="f50e0-148">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="writing-trace-output-to-the-console"></a><span data-ttu-id="f50e0-149">Zápis trasování výstup do konzoly</span><span class="sxs-lookup"><span data-stu-id="f50e0-149">Writing trace output to the console</span></span> 

<span data-ttu-id="f50e0-150">Ve funkcích, můžete použít `context.log` metody k zápisu výstupu trasování do konzoly.</span><span class="sxs-lookup"><span data-stu-id="f50e0-150">In Functions, you use the `context.log` methods to write trace output to the console.</span></span> <span data-ttu-id="f50e0-151">V tomto okamžiku nelze použít `console.log` k zápisu do konzoly.</span><span class="sxs-lookup"><span data-stu-id="f50e0-151">At this point, you cannot use `console.log` to write to the console.</span></span>

<span data-ttu-id="f50e0-152">Při volání `context.log()`, zprávy se zapíše do konzoly na výchozí úrovni trasování, která je _informace_ úroveň trasování.</span><span class="sxs-lookup"><span data-stu-id="f50e0-152">When you call `context.log()`, your message is written to the console at the default trace level, which is the _info_ trace level.</span></span> <span data-ttu-id="f50e0-153">Následující kód zapíše do konzoly na úrovni trasování informace:</span><span class="sxs-lookup"><span data-stu-id="f50e0-153">The following code writes to the console at the info trace level:</span></span>

```javascript
context.log({hello: 'world'});  
```

<span data-ttu-id="f50e0-154">Předchozí kód je ekvivalentní následující kód:</span><span class="sxs-lookup"><span data-stu-id="f50e0-154">The preceding code is equivalent to the following code:</span></span>

```javascript
context.log.info({hello: 'world'});  
```

<span data-ttu-id="f50e0-155">Následující kód zapíše do konzoly na úrovni Chyba:</span><span class="sxs-lookup"><span data-stu-id="f50e0-155">The following code writes to the console at the error level:</span></span>

```javascript
context.log.error("An error has occurred.");  
```

<span data-ttu-id="f50e0-156">Protože _chyba_ je trasování nejvyšší úrovně, toto trasování bude zapsáno do výstupu na všech úrovních trasování, dokud je povoleno protokolování.</span><span class="sxs-lookup"><span data-stu-id="f50e0-156">Because _error_ is the highest trace level, this trace is written to the output at all trace levels as long as logging is enabled.</span></span>  


<span data-ttu-id="f50e0-157">Všechny `context.log` metody podporují stejný formát parametr, který je podporován Node.js [util.format metoda](https://nodejs.org/api/util.html#util_util_format_format).</span><span class="sxs-lookup"><span data-stu-id="f50e0-157">All `context.log` methods support the same parameter format that's supported by the Node.js [util.format method](https://nodejs.org/api/util.html#util_util_format_format).</span></span> <span data-ttu-id="f50e0-158">Vezměte v úvahu následující kód, který zapisuje do konzoly pomocí výchozí úroveň trasování:</span><span class="sxs-lookup"><span data-stu-id="f50e0-158">Consider the following code, which writes to the console by using the default trace level:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

<span data-ttu-id="f50e0-159">Můžete taky napsat stejný kód v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="f50e0-159">You can also write the same code in the following format:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a><span data-ttu-id="f50e0-160">Nakonfiguruje úroveň trasování pro protokolování konzoly</span><span class="sxs-lookup"><span data-stu-id="f50e0-160">Configure the trace level for console logging</span></span>

<span data-ttu-id="f50e0-161">Funkce umožňuje definovat úroveň trasování prahová hodnota pro zápis do konzoly, která usnadňuje řízení, které trasování způsob, jak se zapisují do konzoly z funkcí.</span><span class="sxs-lookup"><span data-stu-id="f50e0-161">Functions lets you define the threshold trace level for writing to the console, which makes it easy to control the way traces are written to the console from your functions.</span></span> <span data-ttu-id="f50e0-162">Pokud chcete nastavit prahovou hodnotu pro všechny trasování zapsána do konzoly, použijte `tracing.consoleLevel` vlastnost v souboru host.json.</span><span class="sxs-lookup"><span data-stu-id="f50e0-162">To set the threshold for all traces written to the console, use the `tracing.consoleLevel` property in the host.json file.</span></span> <span data-ttu-id="f50e0-163">Toto nastavení platí pro všechny funkce v aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="f50e0-163">This setting applies to all functions in your function app.</span></span> <span data-ttu-id="f50e0-164">Následující příklad nastavuje mezní hodnotu trasování Zapnutí podrobného protokolování:</span><span class="sxs-lookup"><span data-stu-id="f50e0-164">The following example sets the trace threshold to enable verbose logging:</span></span>

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

<span data-ttu-id="f50e0-165">Hodnoty z **consoleLevel** odpovídají názvy `context.log` metody.</span><span class="sxs-lookup"><span data-stu-id="f50e0-165">Values of **consoleLevel** correspond to the names of the `context.log` methods.</span></span> <span data-ttu-id="f50e0-166">Chcete-li zakázat veškeré protokolování trasování do konzoly, nastavte **consoleLevel** k _vypnout_.</span><span class="sxs-lookup"><span data-stu-id="f50e0-166">To disable all trace logging to the console, set **consoleLevel** to _off_.</span></span> <span data-ttu-id="f50e0-167">Další informace o souboru host.json najdete v tématu [host.json referenční téma](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="f50e0-167">For more information about the host.json file, see the [host.json reference topic](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>

## <a name="http-triggers-and-bindings"></a><span data-ttu-id="f50e0-168">HTTP triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="f50e0-168">HTTP triggers and bindings</span></span>

<span data-ttu-id="f50e0-169">HTTP a aktivační události webhooku a HTTP výstupu vazby používají žádosti a odpovědi objekty k reprezentaci zasílání zpráv protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="f50e0-169">HTTP and webhook triggers and HTTP output bindings use request and response objects to represent the HTTP messaging.</span></span>  

### <a name="request-object"></a><span data-ttu-id="f50e0-170">Objekt žádosti</span><span class="sxs-lookup"><span data-stu-id="f50e0-170">Request object</span></span>

<span data-ttu-id="f50e0-171">`request` Objekt má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f50e0-171">The `request` object has the following properties:</span></span>

| <span data-ttu-id="f50e0-172">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f50e0-172">Property</span></span>      | <span data-ttu-id="f50e0-173">Popis</span><span class="sxs-lookup"><span data-stu-id="f50e0-173">Description</span></span>                                                    |
| ------------- | -------------------------------------------------------------- |
| <span data-ttu-id="f50e0-174">_text_</span><span class="sxs-lookup"><span data-stu-id="f50e0-174">_body_</span></span>        | <span data-ttu-id="f50e0-175">Objekt, který obsahuje text žádosti.</span><span class="sxs-lookup"><span data-stu-id="f50e0-175">An object that contains the body of the request.</span></span>               |
| <span data-ttu-id="f50e0-176">_záhlaví_</span><span class="sxs-lookup"><span data-stu-id="f50e0-176">_headers_</span></span>     | <span data-ttu-id="f50e0-177">Objekt, který obsahuje hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="f50e0-177">An object that contains the request headers.</span></span>                   |
| <span data-ttu-id="f50e0-178">_– Metoda_</span><span class="sxs-lookup"><span data-stu-id="f50e0-178">_method_</span></span>      | <span data-ttu-id="f50e0-179">Metoda HTTP požadavku.</span><span class="sxs-lookup"><span data-stu-id="f50e0-179">The HTTP method of the request.</span></span>                                |
| <span data-ttu-id="f50e0-180">_PůvodníAdresaURL_</span><span class="sxs-lookup"><span data-stu-id="f50e0-180">_originalUrl_</span></span> | <span data-ttu-id="f50e0-181">Adresa URL požadavku.</span><span class="sxs-lookup"><span data-stu-id="f50e0-181">The URL of the request.</span></span>                                        |
| <span data-ttu-id="f50e0-182">_Parametry_</span><span class="sxs-lookup"><span data-stu-id="f50e0-182">_params_</span></span>      | <span data-ttu-id="f50e0-183">Objekt, který obsahuje parametry směrování žádosti.</span><span class="sxs-lookup"><span data-stu-id="f50e0-183">An object that contains the routing parameters of the request.</span></span> |
| <span data-ttu-id="f50e0-184">_dotaz_</span><span class="sxs-lookup"><span data-stu-id="f50e0-184">_query_</span></span>       | <span data-ttu-id="f50e0-185">Objekt, který obsahuje parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="f50e0-185">An object that contains the query parameters.</span></span>                  |
| <span data-ttu-id="f50e0-186">_rawBody_</span><span class="sxs-lookup"><span data-stu-id="f50e0-186">_rawBody_</span></span>     | <span data-ttu-id="f50e0-187">Tělo zprávy jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="f50e0-187">The body of the message as a string.</span></span>                           |


### <a name="response-object"></a><span data-ttu-id="f50e0-188">Objekt odpovědi</span><span class="sxs-lookup"><span data-stu-id="f50e0-188">Response object</span></span>

<span data-ttu-id="f50e0-189">`response` Objekt má následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="f50e0-189">The `response` object has the following properties:</span></span>

| <span data-ttu-id="f50e0-190">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f50e0-190">Property</span></span>  | <span data-ttu-id="f50e0-191">Popis</span><span class="sxs-lookup"><span data-stu-id="f50e0-191">Description</span></span>                                               |
| --------- | --------------------------------------------------------- |
| <span data-ttu-id="f50e0-192">_text_</span><span class="sxs-lookup"><span data-stu-id="f50e0-192">_body_</span></span>    | <span data-ttu-id="f50e0-193">Objekt, který obsahuje text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f50e0-193">An object that contains the body of the response.</span></span>         |
| <span data-ttu-id="f50e0-194">_záhlaví_</span><span class="sxs-lookup"><span data-stu-id="f50e0-194">_headers_</span></span> | <span data-ttu-id="f50e0-195">Objekt, který obsahuje hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f50e0-195">An object that contains the response headers.</span></span>             |
| <span data-ttu-id="f50e0-196">_isRaw_</span><span class="sxs-lookup"><span data-stu-id="f50e0-196">_isRaw_</span></span>   | <span data-ttu-id="f50e0-197">Označuje, že formátování bylo přeskočeno pro odpověď.</span><span class="sxs-lookup"><span data-stu-id="f50e0-197">Indicates that formatting is skipped for the response.</span></span>    |
| <span data-ttu-id="f50e0-198">_Stav_</span><span class="sxs-lookup"><span data-stu-id="f50e0-198">_status_</span></span>  | <span data-ttu-id="f50e0-199">Stavový kód HTTP odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f50e0-199">The HTTP status code of the response.</span></span>                     |

### <a name="accessing-the-request-and-response"></a><span data-ttu-id="f50e0-200">Přístup k požadavku a odpovědi</span><span class="sxs-lookup"><span data-stu-id="f50e0-200">Accessing the request and response</span></span> 

<span data-ttu-id="f50e0-201">Při práci s aktivace protokolu HTTP, se můžete dostat objekty žádosti a odpovědi protokolu HTTP v jednom ze tří způsobů:</span><span class="sxs-lookup"><span data-stu-id="f50e0-201">When you work with HTTP triggers, you can access the HTTP request and response objects in any of three ways:</span></span>

+ <span data-ttu-id="f50e0-202">Z pojmenované vstup a výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="f50e0-202">From the named input and output bindings.</span></span> <span data-ttu-id="f50e0-203">Tímto způsobem triggeru protokolu HTTP a vazeb fungovat stejně jako ostatní vazby.</span><span class="sxs-lookup"><span data-stu-id="f50e0-203">In this way, the HTTP trigger and bindings work the same as any other binding.</span></span> <span data-ttu-id="f50e0-204">Následující příklad nastaví objekt odpovědi pomocí pojmenovaná `response` vazby:</span><span class="sxs-lookup"><span data-stu-id="f50e0-204">The following example sets the response object by using a named `response` binding:</span></span> 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ <span data-ttu-id="f50e0-205">Z `req` a `res` vlastnosti `context` objektu.</span><span class="sxs-lookup"><span data-stu-id="f50e0-205">From `req` and `res` properties on the `context` object.</span></span> <span data-ttu-id="f50e0-206">Tímto způsobem můžete použít konvenční vzor pro přístup protokolu HTTP k datům z objektu context, místo nutnosti použít celý `context.bindings.name` vzor.</span><span class="sxs-lookup"><span data-stu-id="f50e0-206">In this way, you can use the conventional pattern to access HTTP data from the context object, instead of having to use the full `context.bindings.name` pattern.</span></span> <span data-ttu-id="f50e0-207">Následující příklad ukazuje, jak získat přístup `req` a `res` objekty na `context`:</span><span class="sxs-lookup"><span data-stu-id="f50e0-207">The following example shows how to access the `req` and `res` objects on the `context`:</span></span>

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ <span data-ttu-id="f50e0-208">Při volání `context.done()`.</span><span class="sxs-lookup"><span data-stu-id="f50e0-208">By calling `context.done()`.</span></span> <span data-ttu-id="f50e0-209">Zvláštní druh vazby HTTP vrátí odpověď, který je předán `context.done()` metoda.</span><span class="sxs-lookup"><span data-stu-id="f50e0-209">A special kind of HTTP binding returns the response that is passed to the `context.done()` method.</span></span> <span data-ttu-id="f50e0-210">Následující HTTP výstup vazba definuje `$return` výstupní parametr:</span><span class="sxs-lookup"><span data-stu-id="f50e0-210">The following HTTP output binding defines a `$return` output parameter:</span></span>

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    <span data-ttu-id="f50e0-211">Tato vazba výstup očekává, kde zadáte odpovědi při volání `done()`, a to takto:</span><span class="sxs-lookup"><span data-stu-id="f50e0-211">This output binding expects you to supply the response when you call `done()`, as follows:</span></span>

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a><span data-ttu-id="f50e0-212">Uzel verze a správy balíčků</span><span class="sxs-lookup"><span data-stu-id="f50e0-212">Node version and Package Management</span></span>
<span data-ttu-id="f50e0-213">Verze uzlu je aktuálně uzamčena v `6.5.0`.</span><span class="sxs-lookup"><span data-stu-id="f50e0-213">The node version is currently locked at `6.5.0`.</span></span> <span data-ttu-id="f50e0-214">Jsme se na odstranění příčin přidání podpory pro další verze a jejich zpřístupnění konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="f50e0-214">We're investigating adding support for more versions and making it configurable.</span></span>

<span data-ttu-id="f50e0-215">Následující kroky umožňují mezi ně patřit balíčky v aplikaci funkce:</span><span class="sxs-lookup"><span data-stu-id="f50e0-215">The following steps let you include packages in your function app:</span></span> 

1. <span data-ttu-id="f50e0-216">Přejděte do části `https://<function_app_name>.scm.azurewebsites.net` (Soubor > Nový > Jiné).</span><span class="sxs-lookup"><span data-stu-id="f50e0-216">Go to `https://<function_app_name>.scm.azurewebsites.net`.</span></span>

2. <span data-ttu-id="f50e0-217">Klikněte na tlačítko **ladění konzoly** > **CMD**.</span><span class="sxs-lookup"><span data-stu-id="f50e0-217">Click **Debug Console** > **CMD**.</span></span>

3. <span data-ttu-id="f50e0-218">Přejděte na `D:\home\site\wwwroot`a poté přetáhněte souboru package.json k **wwwroot** složku v horní polovině stránky.</span><span class="sxs-lookup"><span data-stu-id="f50e0-218">Go to `D:\home\site\wwwroot`, and then drag your package.json file to the **wwwroot** folder at the top half of the page.</span></span>  
    <span data-ttu-id="f50e0-219">Soubory můžete nahrát také do vaší aplikace funkce jinými způsoby.</span><span class="sxs-lookup"><span data-stu-id="f50e0-219">You can upload files to your function app in other ways also.</span></span> <span data-ttu-id="f50e0-220">Další informace najdete v tématu [jak aktualizovat soubory aplikace funkce](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="f50e0-220">For more information, see [How to update function app files](functions-reference.md#fileupdate).</span></span> 

4. <span data-ttu-id="f50e0-221">Po nahrání souboru package.json, spusťte `npm install` v příkazu **Kudu vzdálené spuštění konzoly**.</span><span class="sxs-lookup"><span data-stu-id="f50e0-221">After the package.json file is uploaded, run the `npm install` command in the **Kudu remote execution console**.</span></span>  
    <span data-ttu-id="f50e0-222">Tato akce stáhne balíčky uvedené v souboru package.json a restartuje aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="f50e0-222">This action downloads the packages indicated in the package.json file and restarts the function app.</span></span>

<span data-ttu-id="f50e0-223">Po instalaci balíčků, které potřebujete, můžete je importovat do funkce voláním `require('packagename')`, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="f50e0-223">After the packages you need are installed, you import them to your function by calling `require('packagename')`, as in the following example:</span></span>

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

<span data-ttu-id="f50e0-224">Měli byste `package.json` souboru v kořenu aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="f50e0-224">You should define a `package.json` file at the root of your function app.</span></span> <span data-ttu-id="f50e0-225">Definování souboru umožňuje všechny funkce v aplikaci sdílet stejné balíčky v mezipaměti, který poskytuje nejlepší výkon.</span><span class="sxs-lookup"><span data-stu-id="f50e0-225">Defining the file lets all functions in the app share the same cached packages, which gives the best performance.</span></span> <span data-ttu-id="f50e0-226">Pokud dojde ke konfliktu verze, abyste ho mohli vyřešit přidáním `package.json` soubor ve složce konkrétní funkce.</span><span class="sxs-lookup"><span data-stu-id="f50e0-226">If a version conflict arises, you can resolve it by adding a `package.json` file in the folder of a specific function.</span></span>  

## <a name="environment-variables"></a><span data-ttu-id="f50e0-227">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="f50e0-227">Environment variables</span></span>
<span data-ttu-id="f50e0-228">Proměnné prostředí nebo nastavení hodnoty aplikace, použijte `process.env`, jak ukazuje následující příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="f50e0-228">To get an environment variable or an app setting value, use `process.env`, as shown in the following code example:</span></span>

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));

    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```
## <a name="considerations-for-javascript-functions"></a><span data-ttu-id="f50e0-229">Důležité informace týkající se funkce jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="f50e0-229">Considerations for JavaScript functions</span></span>

<span data-ttu-id="f50e0-230">Pokud pracujete s funkce jazyka JavaScript, nezapomeňte aspekty uvedené v následujících dvou částech.</span><span class="sxs-lookup"><span data-stu-id="f50e0-230">When you work with JavaScript functions, be aware of the considerations in the following two sections.</span></span>

### <a name="choose-single-core-app-service-plans"></a><span data-ttu-id="f50e0-231">Zvolte jednojádrový plány služby App Service</span><span class="sxs-lookup"><span data-stu-id="f50e0-231">Choose single-core App Service plans</span></span>

<span data-ttu-id="f50e0-232">Když vytvoříte aplikaci funkce, která používá plán služby App Service, doporučujeme vybrat plán jednojádrový spíše než plán s více jádry.</span><span class="sxs-lookup"><span data-stu-id="f50e0-232">When you create a function app that uses the App Service plan, we recommend that you select a single-core plan rather than a plan with multiple cores.</span></span> <span data-ttu-id="f50e0-233">V současné době funkce jazyka JavaScript funkce efektivněji běží na virtuálních počítačích jedním jádrem a větší virtuální počítače pomocí nevytváří vylepšení očekávaný výkon.</span><span class="sxs-lookup"><span data-stu-id="f50e0-233">Today, Functions runs JavaScript functions more efficiently on single-core VMs, and using larger VMs does not produce the expected performance improvements.</span></span> <span data-ttu-id="f50e0-234">Pokud je to nezbytné, můžete ručně škálovat tak, že přidáte více instancí virtuálního počítače jedním jádrem nebo můžete povolit automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="f50e0-234">When necessary, you can manually scale out by adding more single-core VM instances, or you can enable auto-scale.</span></span> <span data-ttu-id="f50e0-235">Další informace najdete v tématu [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f50e0-235">For more information, see [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span></span>    

### <a name="typescript-and-coffeescript-support"></a><span data-ttu-id="f50e0-236">Podpora typeScript a CoffeeScript</span><span class="sxs-lookup"><span data-stu-id="f50e0-236">TypeScript and CoffeeScript support</span></span>
<span data-ttu-id="f50e0-237">Protože přímé podpory ještě neexistuje pro automatické kompilaci TypeScript nebo CoffeeScript prostřednictvím modulu runtime, tato podpora se musí zpracovávat mimo modulu runtime v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="f50e0-237">Because direct support does not yet exist for auto-compiling TypeScript or CoffeeScript via the runtime, such support needs to be handled outside the runtime, at deployment time.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f50e0-238">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f50e0-238">Next steps</span></span>
<span data-ttu-id="f50e0-239">Další informace najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="f50e0-239">For more information, see the following resources:</span></span>

* [<span data-ttu-id="f50e0-240">Osvědčené postupy pro službu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="f50e0-240">Best practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="f50e0-241">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="f50e0-241">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="f50e0-242">Azure funkcí jazyka C# referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="f50e0-242">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="f50e0-243">Azure funkce F # referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="f50e0-243">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="f50e0-244">Azure funkce triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="f50e0-244">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)

