---
title: "referenční informace pro vývojáře aaaJavaScript pro Azure Functions | Microsoft Docs"
description: "Pochopte, jak funguje toodevelop pomocí jazyka JavaScript."
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
ms.openlocfilehash: 6220b42f965b6ee2463341aaf270836623fdf7fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-javascript-developer-guide"></a><span data-ttu-id="0d4af-104">Příručka vývojáře Azure funkce JavaScript</span><span class="sxs-lookup"><span data-stu-id="0d4af-104">Azure Functions JavaScript developer guide</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0d4af-105">Skript jazyka C#</span><span class="sxs-lookup"><span data-stu-id="0d4af-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="0d4af-106">F # skript</span><span class="sxs-lookup"><span data-stu-id="0d4af-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="0d4af-107">JavaScript</span><span class="sxs-lookup"><span data-stu-id="0d4af-107">JavaScript</span></span>](functions-reference-node.md)
> 
> 

<span data-ttu-id="0d4af-108">Hello prostředí JavaScript pro Azure Functions umožňuje snadno tooexport funkci, která se předá jako `context` objekt pro komunikaci s hello runtime a pro příjem a odesílání dat přes vazby.</span><span class="sxs-lookup"><span data-stu-id="0d4af-108">hello JavaScript experience for Azure Functions makes it easy tooexport a function, which is passed as a `context` object for communicating with hello runtime and for receiving and sending data via bindings.</span></span>

<span data-ttu-id="0d4af-109">Tento článek předpokládá, že jste si přečetli již hello [referenční informace pro vývojáře Azure Functions](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="0d4af-109">This article assumes that you've already read hello [Azure Functions developer reference](functions-reference.md).</span></span>

## <a name="exporting-a-function"></a><span data-ttu-id="0d4af-110">Export funkce</span><span class="sxs-lookup"><span data-stu-id="0d4af-110">Exporting a function</span></span>
<span data-ttu-id="0d4af-111">Všechny funkce jazyka JavaScript, musíte exportovat jedné `function` prostřednictvím `module.exports` pro modul runtime hello toofind hello funkce a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="0d4af-111">All JavaScript functions must export a single `function` via `module.exports` for hello runtime toofind hello function and run it.</span></span> <span data-ttu-id="0d4af-112">Tato funkce musí vždy zahrnovat `context` objektu.</span><span class="sxs-lookup"><span data-stu-id="0d4af-112">This function must always include a `context` object.</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by hello arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

<span data-ttu-id="0d4af-113">Vazeb `direction === "in"` se předají jako argumenty funkce, což znamená, že můžete používat [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) toodynamically zpracovat nové vstupy (například pomocí `arguments.length` tooiterate přes všechny vstupy).</span><span class="sxs-lookup"><span data-stu-id="0d4af-113">Bindings of `direction === "in"` are passed along as function arguments, which means that you can use [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) toodynamically handle new inputs (for example, by using `arguments.length` tooiterate over all your inputs).</span></span> <span data-ttu-id="0d4af-114">Tato funkce je vhodné, pokud máte pouze aktivační událost a žádné další vstupy, protože vaše data aktivační události můžete předvídatelné přístup bez odkazující na váš `context` objektu.</span><span class="sxs-lookup"><span data-stu-id="0d4af-114">This functionality is convenient when you have only a trigger and no additional inputs, because you can predictably access your trigger data without referencing your `context` object.</span></span>

<span data-ttu-id="0d4af-115">argumenty Hello jsou vždy předají toohello funkce v hello pořadí, ve které se vyskytují v *function.json*i v případě, že je v příkazu exportuje nezadáte.</span><span class="sxs-lookup"><span data-stu-id="0d4af-115">hello arguments are always passed along toohello function in hello order in which they occur in *function.json*, even if you don't specify them in your exports statement.</span></span> <span data-ttu-id="0d4af-116">Pokud máte například `function(context, a, b)` a změňte ji příliš`function(context, a)`, stále můžete získat hodnotu hello `b` v kódu funkce tím, že odkazuje příliš`arguments[3]`.</span><span class="sxs-lookup"><span data-stu-id="0d4af-116">For example, if you have `function(context, a, b)` and change it too`function(context, a)`, you can still get hello value of `b` in function code by referring too`arguments[3]`.</span></span>

<span data-ttu-id="0d4af-117">Všechny vazby, bez ohledu na směru, jsou také předají na hello `context` objektu (viz následující skript hello).</span><span class="sxs-lookup"><span data-stu-id="0d4af-117">All bindings, regardless of direction, are also passed along on hello `context` object (see hello following script).</span></span> 

## <a name="context-object"></a><span data-ttu-id="0d4af-118">objekt kontextu</span><span class="sxs-lookup"><span data-stu-id="0d4af-118">context object</span></span>
<span data-ttu-id="0d4af-119">modul runtime Hello používá `context` objekt toopass data tooand z funkce a toolet komunikovat s hello runtime.</span><span class="sxs-lookup"><span data-stu-id="0d4af-119">hello runtime uses a `context` object toopass data tooand from your function and toolet you communicate with hello runtime.</span></span>

<span data-ttu-id="0d4af-120">Hello objekt kontextu je vždy hello první parametr tooa funkce a musí být zahrnut, protože obsahuje metody, jako `context.done` a `context.log`, které jsou požadované toouse hello runtime správně.</span><span class="sxs-lookup"><span data-stu-id="0d4af-120">hello context object is always hello first parameter tooa function and must be included because it has methods such as `context.done` and `context.log`, which are required toouse hello runtime correctly.</span></span> <span data-ttu-id="0d4af-121">Můžete pojmenovat hello objekt ať chcete (například `ctx` nebo `c`).</span><span class="sxs-lookup"><span data-stu-id="0d4af-121">You can name hello object whatever you would like (for example, `ctx` or `c`).</span></span>

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a><span data-ttu-id="0d4af-122">Vlastnost Context.Bindings</span><span class="sxs-lookup"><span data-stu-id="0d4af-122">context.bindings property</span></span>

```
context.bindings
```
<span data-ttu-id="0d4af-123">Vrátí objekt s názvem, který obsahuje všechny vaše vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="0d4af-123">Returns a named object that contains all your input and output data.</span></span> <span data-ttu-id="0d4af-124">Například hello následující definice vazby v vaše *function.json* hello umožní přístup k obsahu fronty hello z hello `context.bindings.myInput` objektu.</span><span class="sxs-lookup"><span data-stu-id="0d4af-124">For example, hello following binding definition in your *function.json* lets you access hello contents of hello queue from hello `context.bindings.myInput` object.</span></span> 

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
}
```

```javascript
// myInput contains hello input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

### <a name="contextdone-method"></a><span data-ttu-id="0d4af-125">Context.Done – metoda</span><span class="sxs-lookup"><span data-stu-id="0d4af-125">context.done method</span></span>
```
context.done([err],[propertyBag])
```

<span data-ttu-id="0d4af-126">Informuje o hello modul runtime, který váš kód byl dokončen.</span><span class="sxs-lookup"><span data-stu-id="0d4af-126">Informs hello runtime that your code has finished.</span></span> <span data-ttu-id="0d4af-127">Je třeba volat `context.done`, nebo jiný modul runtime hello nikdy ví, že funkce je dokončena a provádění hello bude časový limit.</span><span class="sxs-lookup"><span data-stu-id="0d4af-127">You must call `context.done`, or else hello runtime never knows that your function is complete, and hello execution will time out.</span></span> 

<span data-ttu-id="0d4af-128">Hello `context.done` metoda umožňuje vám toopass zpět uživatelem definované chybové toohello runtime a kontejner objektů a vlastností, které přepsat hello vlastnosti hello `context.bindings` objektu.</span><span class="sxs-lookup"><span data-stu-id="0d4af-128">hello `context.done` method allows you toopass back both a user-defined error toohello runtime and a property bag of properties that overwrite hello properties on hello `context.bindings` object.</span></span>

```javascript
// Even though we set myOutput toohave:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object toohello done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// hello done method will overwrite hello myOutput binding toobe: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a><span data-ttu-id="0d4af-129">Context.log – metoda</span><span class="sxs-lookup"><span data-stu-id="0d4af-129">context.log method</span></span>  

```
context.log(message)
```
<span data-ttu-id="0d4af-130">Umožňuje vám toowrite toohello streamování konzoly protokoly na úrovni trasování výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="0d4af-130">Allows you toowrite toohello streaming console logs at hello default trace level.</span></span> <span data-ttu-id="0d4af-131">Na `context.log`, další metody protokolování jsou k dispozici, která umožňují zápis toohello konzoly protokolu na jiných úrovních trasování:</span><span class="sxs-lookup"><span data-stu-id="0d4af-131">On `context.log`, additional logging methods are available that let you write toohello console log at other trace levels:</span></span>


| <span data-ttu-id="0d4af-132">Metoda</span><span class="sxs-lookup"><span data-stu-id="0d4af-132">Method</span></span>                 | <span data-ttu-id="0d4af-133">Popis</span><span class="sxs-lookup"><span data-stu-id="0d4af-133">Description</span></span>                                |
| ---------------------- | ------------------------------------------ |
| <span data-ttu-id="0d4af-134">**Chyba (_zpráva_)**</span><span class="sxs-lookup"><span data-stu-id="0d4af-134">**error(_message_)**</span></span>   | <span data-ttu-id="0d4af-135">Zapíše tooerror úroveň protokolování nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="0d4af-135">Writes tooerror level logging, or lower.</span></span>   |
| <span data-ttu-id="0d4af-136">**warn (_zpráva_)**</span><span class="sxs-lookup"><span data-stu-id="0d4af-136">**warn(_message_)**</span></span>    | <span data-ttu-id="0d4af-137">Zapíše toowarning úroveň protokolování nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="0d4af-137">Writes toowarning level logging, or lower.</span></span> |
| <span data-ttu-id="0d4af-138">**informace o (_zpráva_)**</span><span class="sxs-lookup"><span data-stu-id="0d4af-138">**info(_message_)**</span></span>    | <span data-ttu-id="0d4af-139">Zapíše tooinfo úroveň protokolování nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="0d4af-139">Writes tooinfo level logging, or lower.</span></span>    |
| <span data-ttu-id="0d4af-140">**verbose (_zpráva_)**</span><span class="sxs-lookup"><span data-stu-id="0d4af-140">**verbose(_message_)**</span></span> | <span data-ttu-id="0d4af-141">Zapíše tooverbose úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="0d4af-141">Writes tooverbose level logging.</span></span>           |

<span data-ttu-id="0d4af-142">Hello následující příklad zapíše toohello konzoly na úrovni trasování upozornění hello:</span><span class="sxs-lookup"><span data-stu-id="0d4af-142">hello following example writes toohello console at hello warning trace level:</span></span>

```javascript
context.log.warn("Something has happened."); 
```
<span data-ttu-id="0d4af-143">Můžete nastavit prahové hodnoty hello úroveň trasování pro protokolování v souboru host.json hello nebo vypnout.</span><span class="sxs-lookup"><span data-stu-id="0d4af-143">You can set hello trace-level threshold for logging in hello host.json file, or turn it off.</span></span>  <span data-ttu-id="0d4af-144">Další informace o způsobu, jakým toowrite toohello zadává viz další část hello.</span><span class="sxs-lookup"><span data-stu-id="0d4af-144">For more information about how toowrite toohello logs, see hello next section.</span></span>

## <a name="binding-data-type"></a><span data-ttu-id="0d4af-145">Datový typ vazby</span><span class="sxs-lookup"><span data-stu-id="0d4af-145">Binding data type</span></span>

<span data-ttu-id="0d4af-146">toodefine hello datový typ pro vstupní vazbu, použijte hello `dataType` vlastnost v definici vazby hello.</span><span class="sxs-lookup"><span data-stu-id="0d4af-146">toodefine hello data type for an input binding, use hello `dataType` property in hello binding definition.</span></span> <span data-ttu-id="0d4af-147">Například tooread hello obsahu požadavku HTTP v binárním formátu, použijte typ hello `binary`:</span><span class="sxs-lookup"><span data-stu-id="0d4af-147">For example, tooread hello content of an HTTP request in binary format, use hello type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="0d4af-148">Další možnosti pro `dataType` jsou `stream` a `string`.</span><span class="sxs-lookup"><span data-stu-id="0d4af-148">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="writing-trace-output-toohello-console"></a><span data-ttu-id="0d4af-149">Zápis trasování výstup toohello konzoly</span><span class="sxs-lookup"><span data-stu-id="0d4af-149">Writing trace output toohello console</span></span> 

<span data-ttu-id="0d4af-150">Ve funkcích, použijete hello `context.log` metody toowrite trasování výstup toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="0d4af-150">In Functions, you use hello `context.log` methods toowrite trace output toohello console.</span></span> <span data-ttu-id="0d4af-151">V tomto okamžiku nelze použít `console.log` toowrite toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="0d4af-151">At this point, you cannot use `console.log` toowrite toohello console.</span></span>

<span data-ttu-id="0d4af-152">Při volání `context.log()`, zprávy se zapíše toohello konzoly na úrovni trasování výchozí hello, což je hello _informace_ úroveň trasování.</span><span class="sxs-lookup"><span data-stu-id="0d4af-152">When you call `context.log()`, your message is written toohello console at hello default trace level, which is hello _info_ trace level.</span></span> <span data-ttu-id="0d4af-153">Hello následující kód zapíše toohello konzoly na úrovni trasování hello informace:</span><span class="sxs-lookup"><span data-stu-id="0d4af-153">hello following code writes toohello console at hello info trace level:</span></span>

```javascript
context.log({hello: 'world'});  
```

<span data-ttu-id="0d4af-154">Hello předchozí kód je ekvivalentní toohello následující kód:</span><span class="sxs-lookup"><span data-stu-id="0d4af-154">hello preceding code is equivalent toohello following code:</span></span>

```javascript
context.log.info({hello: 'world'});  
```

<span data-ttu-id="0d4af-155">Hello následující kód zapíše toohello konzoly na úroveň hello chyb:</span><span class="sxs-lookup"><span data-stu-id="0d4af-155">hello following code writes toohello console at hello error level:</span></span>

```javascript
context.log.error("An error has occurred.");  
```

<span data-ttu-id="0d4af-156">Protože _chyba_ je trasování hello nejvyšší úrovně, trasování je zapsán výstup toohello na všech úrovních trasování, dokud je povoleno protokolování.</span><span class="sxs-lookup"><span data-stu-id="0d4af-156">Because _error_ is hello highest trace level, this trace is written toohello output at all trace levels as long as logging is enabled.</span></span>  


<span data-ttu-id="0d4af-157">Všechny `context.log` metody podporují hello stejný formát parametru, která podporuje hello Node.js [util.format metoda](https://nodejs.org/api/util.html#util_util_format_format).</span><span class="sxs-lookup"><span data-stu-id="0d4af-157">All `context.log` methods support hello same parameter format that's supported by hello Node.js [util.format method](https://nodejs.org/api/util.html#util_util_format_format).</span></span> <span data-ttu-id="0d4af-158">Vezměte v úvahu následující kód, který zapíše toohello konzoly pomocí úroveň trasování výchozí hello hello:</span><span class="sxs-lookup"><span data-stu-id="0d4af-158">Consider hello following code, which writes toohello console by using hello default trace level:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

<span data-ttu-id="0d4af-159">Můžete také zápisu hello stejný kód na hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="0d4af-159">You can also write hello same code in hello following format:</span></span>

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-hello-trace-level-for-console-logging"></a><span data-ttu-id="0d4af-160">Konfigurace hello úroveň trasování pro protokolování konzoly</span><span class="sxs-lookup"><span data-stu-id="0d4af-160">Configure hello trace level for console logging</span></span>

<span data-ttu-id="0d4af-161">Funkce umožňuje definovat úroveň trasování hello prahová hodnota pro zápis toohello konzoly, která umožňuje snadno toocontrol hello způsob trasování jsou zapsány toohello konzoly z funkcí.</span><span class="sxs-lookup"><span data-stu-id="0d4af-161">Functions lets you define hello threshold trace level for writing toohello console, which makes it easy toocontrol hello way traces are written toohello console from your functions.</span></span> <span data-ttu-id="0d4af-162">Prahová hodnota hello tooset pro všechny trasování zapsat toohello konzoly, použijte hello `tracing.consoleLevel` vlastnost v souboru host.json hello.</span><span class="sxs-lookup"><span data-stu-id="0d4af-162">tooset hello threshold for all traces written toohello console, use hello `tracing.consoleLevel` property in hello host.json file.</span></span> <span data-ttu-id="0d4af-163">Toto nastavení platí tooall funkce v aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="0d4af-163">This setting applies tooall functions in your function app.</span></span> <span data-ttu-id="0d4af-164">Hello následující příklad ilustruje hello trasování prahová hodnota tooenable podrobné protokolování:</span><span class="sxs-lookup"><span data-stu-id="0d4af-164">hello following example sets hello trace threshold tooenable verbose logging:</span></span>

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

<span data-ttu-id="0d4af-165">Hodnoty z **consoleLevel** odpovídají toohello názvy hello `context.log` metody.</span><span class="sxs-lookup"><span data-stu-id="0d4af-165">Values of **consoleLevel** correspond toohello names of hello `context.log` methods.</span></span> <span data-ttu-id="0d4af-166">nastavení protokolování toohello konzole, všechny trasování toodisable **consoleLevel** too_off_.</span><span class="sxs-lookup"><span data-stu-id="0d4af-166">toodisable all trace logging toohello console, set **consoleLevel** too_off_.</span></span> <span data-ttu-id="0d4af-167">Další informace o souboru host.json hello najdete v tématu hello [host.json referenční téma](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="0d4af-167">For more information about hello host.json file, see hello [host.json reference topic](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span>

## <a name="http-triggers-and-bindings"></a><span data-ttu-id="0d4af-168">HTTP triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="0d4af-168">HTTP triggers and bindings</span></span>

<span data-ttu-id="0d4af-169">HTTP a aktivační události webhooku a HTTP výstupu vazby používat požadavku a odpovědi objekty toorepresent hello HTTP zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="0d4af-169">HTTP and webhook triggers and HTTP output bindings use request and response objects toorepresent hello HTTP messaging.</span></span>  

### <a name="request-object"></a><span data-ttu-id="0d4af-170">Objekt žádosti</span><span class="sxs-lookup"><span data-stu-id="0d4af-170">Request object</span></span>

<span data-ttu-id="0d4af-171">Hello `request` objekt má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="0d4af-171">hello `request` object has hello following properties:</span></span>

| <span data-ttu-id="0d4af-172">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0d4af-172">Property</span></span>      | <span data-ttu-id="0d4af-173">Popis</span><span class="sxs-lookup"><span data-stu-id="0d4af-173">Description</span></span>                                                    |
| ------------- | -------------------------------------------------------------- |
| <span data-ttu-id="0d4af-174">_text_</span><span class="sxs-lookup"><span data-stu-id="0d4af-174">_body_</span></span>        | <span data-ttu-id="0d4af-175">Objekt, který obsahuje hello textu hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="0d4af-175">An object that contains hello body of hello request.</span></span>               |
| <span data-ttu-id="0d4af-176">_záhlaví_</span><span class="sxs-lookup"><span data-stu-id="0d4af-176">_headers_</span></span>     | <span data-ttu-id="0d4af-177">Objekt, který obsahuje hello hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="0d4af-177">An object that contains hello request headers.</span></span>                   |
| <span data-ttu-id="0d4af-178">_– Metoda_</span><span class="sxs-lookup"><span data-stu-id="0d4af-178">_method_</span></span>      | <span data-ttu-id="0d4af-179">Metoda HTTP požadavku hello Hello.</span><span class="sxs-lookup"><span data-stu-id="0d4af-179">hello HTTP method of hello request.</span></span>                                |
| <span data-ttu-id="0d4af-180">_PůvodníAdresaURL_</span><span class="sxs-lookup"><span data-stu-id="0d4af-180">_originalUrl_</span></span> | <span data-ttu-id="0d4af-181">Adresa URL Hello hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="0d4af-181">hello URL of hello request.</span></span>                                        |
| <span data-ttu-id="0d4af-182">_Parametry_</span><span class="sxs-lookup"><span data-stu-id="0d4af-182">_params_</span></span>      | <span data-ttu-id="0d4af-183">Objekt, který obsahuje hello směrování parametrů hello žádosti.</span><span class="sxs-lookup"><span data-stu-id="0d4af-183">An object that contains hello routing parameters of hello request.</span></span> |
| <span data-ttu-id="0d4af-184">_dotaz_</span><span class="sxs-lookup"><span data-stu-id="0d4af-184">_query_</span></span>       | <span data-ttu-id="0d4af-185">Objekt, který obsahuje parametry dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="0d4af-185">An object that contains hello query parameters.</span></span>                  |
| <span data-ttu-id="0d4af-186">_rawBody_</span><span class="sxs-lookup"><span data-stu-id="0d4af-186">_rawBody_</span></span>     | <span data-ttu-id="0d4af-187">Hello těla zprávy hello jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="0d4af-187">hello body of hello message as a string.</span></span>                           |


### <a name="response-object"></a><span data-ttu-id="0d4af-188">Objekt odpovědi</span><span class="sxs-lookup"><span data-stu-id="0d4af-188">Response object</span></span>

<span data-ttu-id="0d4af-189">Hello `response` objekt má hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="0d4af-189">hello `response` object has hello following properties:</span></span>

| <span data-ttu-id="0d4af-190">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="0d4af-190">Property</span></span>  | <span data-ttu-id="0d4af-191">Popis</span><span class="sxs-lookup"><span data-stu-id="0d4af-191">Description</span></span>                                               |
| --------- | --------------------------------------------------------- |
| <span data-ttu-id="0d4af-192">_text_</span><span class="sxs-lookup"><span data-stu-id="0d4af-192">_body_</span></span>    | <span data-ttu-id="0d4af-193">Objekt, který obsahuje hello text odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="0d4af-193">An object that contains hello body of hello response.</span></span>         |
| <span data-ttu-id="0d4af-194">_záhlaví_</span><span class="sxs-lookup"><span data-stu-id="0d4af-194">_headers_</span></span> | <span data-ttu-id="0d4af-195">Objekt, který obsahuje hello hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0d4af-195">An object that contains hello response headers.</span></span>             |
| <span data-ttu-id="0d4af-196">_isRaw_</span><span class="sxs-lookup"><span data-stu-id="0d4af-196">_isRaw_</span></span>   | <span data-ttu-id="0d4af-197">Označuje, že formátování bylo přeskočeno pro odpověď hello.</span><span class="sxs-lookup"><span data-stu-id="0d4af-197">Indicates that formatting is skipped for hello response.</span></span>    |
| <span data-ttu-id="0d4af-198">_Stav_</span><span class="sxs-lookup"><span data-stu-id="0d4af-198">_status_</span></span>  | <span data-ttu-id="0d4af-199">Hello stavový kód HTTP odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="0d4af-199">hello HTTP status code of hello response.</span></span>                     |

### <a name="accessing-hello-request-and-response"></a><span data-ttu-id="0d4af-200">Přístup k hello žádosti a odpovědi</span><span class="sxs-lookup"><span data-stu-id="0d4af-200">Accessing hello request and response</span></span> 

<span data-ttu-id="0d4af-201">Při práci s aktivace protokolu HTTP, můžete přístup k objektům požadavku a odpovědi HTTP hello v jednom ze tří způsobů:</span><span class="sxs-lookup"><span data-stu-id="0d4af-201">When you work with HTTP triggers, you can access hello HTTP request and response objects in any of three ways:</span></span>

+ <span data-ttu-id="0d4af-202">Z hello s názvem vstup a výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="0d4af-202">From hello named input and output bindings.</span></span> <span data-ttu-id="0d4af-203">Tímto způsobem hello triggeru protokolu HTTP a pracovní vazby hello stejné jako druhé vazby.</span><span class="sxs-lookup"><span data-stu-id="0d4af-203">In this way, hello HTTP trigger and bindings work hello same as any other binding.</span></span> <span data-ttu-id="0d4af-204">Hello následující příklad nastaví objektu odpovědi hello pomocí pojmenovaná `response` vazby:</span><span class="sxs-lookup"><span data-stu-id="0d4af-204">hello following example sets hello response object by using a named `response` binding:</span></span> 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ <span data-ttu-id="0d4af-205">Z `req` a `res` vlastnosti hello `context` objektu.</span><span class="sxs-lookup"><span data-stu-id="0d4af-205">From `req` and `res` properties on hello `context` object.</span></span> <span data-ttu-id="0d4af-206">Tímto způsobem můžete hello konvenční vzor tooaccess HTTP data z objektu context hello, místo nutnosti toouse hello úplné `context.bindings.name` vzor.</span><span class="sxs-lookup"><span data-stu-id="0d4af-206">In this way, you can use hello conventional pattern tooaccess HTTP data from hello context object, instead of having toouse hello full `context.bindings.name` pattern.</span></span> <span data-ttu-id="0d4af-207">Následující příklad ukazuje, jak Hello tooaccess hello `req` a `res` objekty v hello `context`:</span><span class="sxs-lookup"><span data-stu-id="0d4af-207">hello following example shows how tooaccess hello `req` and `res` objects on hello `context`:</span></span>

    ```javascript
    // You can access your http request off hello context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ <span data-ttu-id="0d4af-208">Při volání `context.done()`.</span><span class="sxs-lookup"><span data-stu-id="0d4af-208">By calling `context.done()`.</span></span> <span data-ttu-id="0d4af-209">Zvláštní druh vazby HTTP vrátí hello odpověď, který je předán toohello `context.done()` metoda.</span><span class="sxs-lookup"><span data-stu-id="0d4af-209">A special kind of HTTP binding returns hello response that is passed toohello `context.done()` method.</span></span> <span data-ttu-id="0d4af-210">výstup Hello následující HTTP definuje vazbu `$return` výstupní parametr:</span><span class="sxs-lookup"><span data-stu-id="0d4af-210">hello following HTTP output binding defines a `$return` output parameter:</span></span>

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    <span data-ttu-id="0d4af-211">Tato vazba výstup očekává, že budete toosupply hello odpovědi při volání `done()`, a to takto:</span><span class="sxs-lookup"><span data-stu-id="0d4af-211">This output binding expects you toosupply hello response when you call `done()`, as follows:</span></span>

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a><span data-ttu-id="0d4af-212">Uzel verze a správy balíčků</span><span class="sxs-lookup"><span data-stu-id="0d4af-212">Node version and Package Management</span></span>
<span data-ttu-id="0d4af-213">verze uzlu Hello je aktuálně uzamčena v `6.5.0`.</span><span class="sxs-lookup"><span data-stu-id="0d4af-213">hello node version is currently locked at `6.5.0`.</span></span> <span data-ttu-id="0d4af-214">Jsme se na odstranění příčin přidání podpory pro další verze a jejich zpřístupnění konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="0d4af-214">We're investigating adding support for more versions and making it configurable.</span></span>

<span data-ttu-id="0d4af-215">Hello následující kroky umožňují mezi ně patřit balíčky v aplikaci funkce:</span><span class="sxs-lookup"><span data-stu-id="0d4af-215">hello following steps let you include packages in your function app:</span></span> 

1. <span data-ttu-id="0d4af-216">Přejděte příliš`https://<function_app_name>.scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="0d4af-216">Go too`https://<function_app_name>.scm.azurewebsites.net`.</span></span>

2. <span data-ttu-id="0d4af-217">Klikněte na tlačítko **ladění konzoly** > **CMD**.</span><span class="sxs-lookup"><span data-stu-id="0d4af-217">Click **Debug Console** > **CMD**.</span></span>

3. <span data-ttu-id="0d4af-218">Přejděte příliš`D:\home\site\wwwroot`a poté přetáhněte vaší toohello soubor package.json **wwwroot** složku v horní polovině hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="0d4af-218">Go too`D:\home\site\wwwroot`, and then drag your package.json file toohello **wwwroot** folder at hello top half of hello page.</span></span>  
    <span data-ttu-id="0d4af-219">Také můžete nahrát aplikaci funkce tooyour soubory jinými způsoby.</span><span class="sxs-lookup"><span data-stu-id="0d4af-219">You can upload files tooyour function app in other ways also.</span></span> <span data-ttu-id="0d4af-220">Další informace najdete v tématu [jak tooupdate funkce soubory aplikace](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="0d4af-220">For more information, see [How tooupdate function app files](functions-reference.md#fileupdate).</span></span> 

4. <span data-ttu-id="0d4af-221">Po odeslání soubor package.json hello spustit hello `npm install` v hello **Kudu vzdálené spuštění konzoly**.</span><span class="sxs-lookup"><span data-stu-id="0d4af-221">After hello package.json file is uploaded, run hello `npm install` command in hello **Kudu remote execution console**.</span></span>  
    <span data-ttu-id="0d4af-222">Tato akce stáhne hello balíčky uvedené v souboru package.json hello a restartuje aplikaci funkce hello.</span><span class="sxs-lookup"><span data-stu-id="0d4af-222">This action downloads hello packages indicated in hello package.json file and restarts hello function app.</span></span>

<span data-ttu-id="0d4af-223">Po hello potřebujete jsou nainstalované balíčky, importu funkce tooyour voláním `require('packagename')`, jako v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="0d4af-223">After hello packages you need are installed, you import them tooyour function by calling `require('packagename')`, as in hello following example:</span></span>

```javascript
// Import hello underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

<span data-ttu-id="0d4af-224">Měli byste `package.json` souboru v kořenovém hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d4af-224">You should define a `package.json` file at hello root of your function app.</span></span> <span data-ttu-id="0d4af-225">Definiční soubor hello umožňuje všechny funkce ve sdílené složce aplikace hello hello stejné balíčky v mezipaměti, což dává hello nejlepší výkon.</span><span class="sxs-lookup"><span data-stu-id="0d4af-225">Defining hello file lets all functions in hello app share hello same cached packages, which gives hello best performance.</span></span> <span data-ttu-id="0d4af-226">Pokud dojde ke konfliktu verze, abyste ho mohli vyřešit přidáním `package.json` souboru ve složce hello konkrétní funkce.</span><span class="sxs-lookup"><span data-stu-id="0d4af-226">If a version conflict arises, you can resolve it by adding a `package.json` file in hello folder of a specific function.</span></span>  

## <a name="environment-variables"></a><span data-ttu-id="0d4af-227">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="0d4af-227">Environment variables</span></span>
<span data-ttu-id="0d4af-228">tooget proměnné prostředí nebo hodnotu nastavení aplikace, použijte `process.env`, jak ukazuje následující příklad kódu hello:</span><span class="sxs-lookup"><span data-stu-id="0d4af-228">tooget an environment variable or an app setting value, use `process.env`, as shown in hello following code example:</span></span>

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
## <a name="considerations-for-javascript-functions"></a><span data-ttu-id="0d4af-229">Důležité informace týkající se funkce jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="0d4af-229">Considerations for JavaScript functions</span></span>

<span data-ttu-id="0d4af-230">Při práci s funkce jazyka JavaScript, mějte na paměti aspektů hello v hello následující dvě části.</span><span class="sxs-lookup"><span data-stu-id="0d4af-230">When you work with JavaScript functions, be aware of hello considerations in hello following two sections.</span></span>

### <a name="choose-single-core-app-service-plans"></a><span data-ttu-id="0d4af-231">Zvolte jednojádrový plány služby App Service</span><span class="sxs-lookup"><span data-stu-id="0d4af-231">Choose single-core App Service plans</span></span>

<span data-ttu-id="0d4af-232">Když vytvoříte aplikaci funkce, která používá hello plán služby App Service, doporučujeme vybrat plán jednojádrový spíše než plán s více jádry.</span><span class="sxs-lookup"><span data-stu-id="0d4af-232">When you create a function app that uses hello App Service plan, we recommend that you select a single-core plan rather than a plan with multiple cores.</span></span> <span data-ttu-id="0d4af-233">V současné době funkce jazyka JavaScript funkce efektivněji běží na virtuálních počítačích jedním jádrem a větší virtuální počítače pomocí nevytváří vylepšení výkonu hello očekává.</span><span class="sxs-lookup"><span data-stu-id="0d4af-233">Today, Functions runs JavaScript functions more efficiently on single-core VMs, and using larger VMs does not produce hello expected performance improvements.</span></span> <span data-ttu-id="0d4af-234">Pokud je to nezbytné, můžete ručně škálovat tak, že přidáte více instancí virtuálního počítače jedním jádrem nebo můžete povolit automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="0d4af-234">When necessary, you can manually scale out by adding more single-core VM instances, or you can enable auto-scale.</span></span> <span data-ttu-id="0d4af-235">Další informace najdete v tématu [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0d4af-235">For more information, see [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).</span></span>    

### <a name="typescript-and-coffeescript-support"></a><span data-ttu-id="0d4af-236">Podpora typeScript a CoffeeScript</span><span class="sxs-lookup"><span data-stu-id="0d4af-236">TypeScript and CoffeeScript support</span></span>
<span data-ttu-id="0d4af-237">Protože přímé podpory ještě neexistuje pro automatické kompilaci TypeScript nebo CoffeeScript prostřednictvím hello runtime, musí tato podpora toobe zpracovává mimo hello runtime v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="0d4af-237">Because direct support does not yet exist for auto-compiling TypeScript or CoffeeScript via hello runtime, such support needs toobe handled outside hello runtime, at deployment time.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0d4af-238">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d4af-238">Next steps</span></span>
<span data-ttu-id="0d4af-239">Další informace najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="0d4af-239">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="0d4af-240">Osvědčené postupy pro službu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="0d4af-240">Best practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="0d4af-241">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="0d4af-241">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="0d4af-242">Azure funkcí jazyka C# referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="0d4af-242">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="0d4af-243">Azure funkce F # referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="0d4af-243">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="0d4af-244">Azure funkce triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="0d4af-244">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)

