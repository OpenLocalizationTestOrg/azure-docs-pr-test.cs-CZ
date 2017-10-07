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
# <a name="azure-functions-javascript-developer-guide"></a>Příručka vývojáře Azure funkce JavaScript
> [!div class="op_single_selector"]
> * [Skript jazyka C#](functions-reference-csharp.md)
> * [F # skript](functions-reference-fsharp.md)
> * [JavaScript](functions-reference-node.md)
> 
> 

Hello prostředí JavaScript pro Azure Functions umožňuje snadno tooexport funkci, která se předá jako `context` objekt pro komunikaci s hello runtime a pro příjem a odesílání dat přes vazby.

Tento článek předpokládá, že jste si přečetli již hello [referenční informace pro vývojáře Azure Functions](functions-reference.md).

## <a name="exporting-a-function"></a>Export funkce
Všechny funkce jazyka JavaScript, musíte exportovat jedné `function` prostřednictvím `module.exports` pro modul runtime hello toofind hello funkce a potom ho spusťte. Tato funkce musí vždy zahrnovat `context` objektu.

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

Vazeb `direction === "in"` se předají jako argumenty funkce, což znamená, že můžete používat [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) toodynamically zpracovat nové vstupy (například pomocí `arguments.length` tooiterate přes všechny vstupy). Tato funkce je vhodné, pokud máte pouze aktivační událost a žádné další vstupy, protože vaše data aktivační události můžete předvídatelné přístup bez odkazující na váš `context` objektu.

argumenty Hello jsou vždy předají toohello funkce v hello pořadí, ve které se vyskytují v *function.json*i v případě, že je v příkazu exportuje nezadáte. Pokud máte například `function(context, a, b)` a změňte ji příliš`function(context, a)`, stále můžete získat hodnotu hello `b` v kódu funkce tím, že odkazuje příliš`arguments[3]`.

Všechny vazby, bez ohledu na směru, jsou také předají na hello `context` objektu (viz následující skript hello). 

## <a name="context-object"></a>objekt kontextu
modul runtime Hello používá `context` objekt toopass data tooand z funkce a toolet komunikovat s hello runtime.

Hello objekt kontextu je vždy hello první parametr tooa funkce a musí být zahrnut, protože obsahuje metody, jako `context.done` a `context.log`, které jsou požadované toouse hello runtime správně. Můžete pojmenovat hello objekt ať chcete (například `ctx` nebo `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a>Vlastnost Context.Bindings

```
context.bindings
```
Vrátí objekt s názvem, který obsahuje všechny vaše vstupní a výstupní data. Například hello následující definice vazby v vaše *function.json* hello umožní přístup k obsahu fronty hello z hello `context.bindings.myInput` objektu. 

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

### <a name="contextdone-method"></a>Context.Done – metoda
```
context.done([err],[propertyBag])
```

Informuje o hello modul runtime, který váš kód byl dokončen. Je třeba volat `context.done`, nebo jiný modul runtime hello nikdy ví, že funkce je dokončena a provádění hello bude časový limit. 

Hello `context.done` metoda umožňuje vám toopass zpět uživatelem definované chybové toohello runtime a kontejner objektů a vlastností, které přepsat hello vlastnosti hello `context.bindings` objektu.

```javascript
// Even though we set myOutput toohave:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object toohello done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// hello done method will overwrite hello myOutput binding toobe: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a>Context.log – metoda  

```
context.log(message)
```
Umožňuje vám toowrite toohello streamování konzoly protokoly na úrovni trasování výchozí hello. Na `context.log`, další metody protokolování jsou k dispozici, která umožňují zápis toohello konzoly protokolu na jiných úrovních trasování:


| Metoda                 | Popis                                |
| ---------------------- | ------------------------------------------ |
| **Chyba (_zpráva_)**   | Zapíše tooerror úroveň protokolování nebo nižší.   |
| **warn (_zpráva_)**    | Zapíše toowarning úroveň protokolování nebo nižší. |
| **informace o (_zpráva_)**    | Zapíše tooinfo úroveň protokolování nebo nižší.    |
| **verbose (_zpráva_)** | Zapíše tooverbose úrovně protokolování.           |

Hello následující příklad zapíše toohello konzoly na úrovni trasování upozornění hello:

```javascript
context.log.warn("Something has happened."); 
```
Můžete nastavit prahové hodnoty hello úroveň trasování pro protokolování v souboru host.json hello nebo vypnout.  Další informace o způsobu, jakým toowrite toohello zadává viz další část hello.

## <a name="binding-data-type"></a>Datový typ vazby

toodefine hello datový typ pro vstupní vazbu, použijte hello `dataType` vlastnost v definici vazby hello. Například tooread hello obsahu požadavku HTTP v binárním formátu, použijte typ hello `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Další možnosti pro `dataType` jsou `stream` a `string`.

## <a name="writing-trace-output-toohello-console"></a>Zápis trasování výstup toohello konzoly 

Ve funkcích, použijete hello `context.log` metody toowrite trasování výstup toohello konzoly. V tomto okamžiku nelze použít `console.log` toowrite toohello konzoly.

Při volání `context.log()`, zprávy se zapíše toohello konzoly na úrovni trasování výchozí hello, což je hello _informace_ úroveň trasování. Hello následující kód zapíše toohello konzoly na úrovni trasování hello informace:

```javascript
context.log({hello: 'world'});  
```

Hello předchozí kód je ekvivalentní toohello následující kód:

```javascript
context.log.info({hello: 'world'});  
```

Hello následující kód zapíše toohello konzoly na úroveň hello chyb:

```javascript
context.log.error("An error has occurred.");  
```

Protože _chyba_ je trasování hello nejvyšší úrovně, trasování je zapsán výstup toohello na všech úrovních trasování, dokud je povoleno protokolování.  


Všechny `context.log` metody podporují hello stejný formát parametru, která podporuje hello Node.js [util.format metoda](https://nodejs.org/api/util.html#util_util_format_format). Vezměte v úvahu následující kód, který zapíše toohello konzoly pomocí úroveň trasování výchozí hello hello:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

Můžete také zápisu hello stejný kód na hello následující formát:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-hello-trace-level-for-console-logging"></a>Konfigurace hello úroveň trasování pro protokolování konzoly

Funkce umožňuje definovat úroveň trasování hello prahová hodnota pro zápis toohello konzoly, která umožňuje snadno toocontrol hello způsob trasování jsou zapsány toohello konzoly z funkcí. Prahová hodnota hello tooset pro všechny trasování zapsat toohello konzoly, použijte hello `tracing.consoleLevel` vlastnost v souboru host.json hello. Toto nastavení platí tooall funkce v aplikaci funkce. Hello následující příklad ilustruje hello trasování prahová hodnota tooenable podrobné protokolování:

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

Hodnoty z **consoleLevel** odpovídají toohello názvy hello `context.log` metody. nastavení protokolování toohello konzole, všechny trasování toodisable **consoleLevel** too_off_. Další informace o souboru host.json hello najdete v tématu hello [host.json referenční téma](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).

## <a name="http-triggers-and-bindings"></a>HTTP triggerů a vazeb

HTTP a aktivační události webhooku a HTTP výstupu vazby používat požadavku a odpovědi objekty toorepresent hello HTTP zasílání zpráv.  

### <a name="request-object"></a>Objekt žádosti

Hello `request` objekt má hello následující vlastnosti:

| Vlastnost      | Popis                                                    |
| ------------- | -------------------------------------------------------------- |
| _text_        | Objekt, který obsahuje hello textu hello požadavku.               |
| _záhlaví_     | Objekt, který obsahuje hello hlavičky žádosti.                   |
| _– Metoda_      | Metoda HTTP požadavku hello Hello.                                |
| _PůvodníAdresaURL_ | Adresa URL Hello hello požadavku.                                        |
| _Parametry_      | Objekt, který obsahuje hello směrování parametrů hello žádosti. |
| _dotaz_       | Objekt, který obsahuje parametry dotazu hello.                  |
| _rawBody_     | Hello těla zprávy hello jako řetězec.                           |


### <a name="response-object"></a>Objekt odpovědi

Hello `response` objekt má hello následující vlastnosti:

| Vlastnost  | Popis                                               |
| --------- | --------------------------------------------------------- |
| _text_    | Objekt, který obsahuje hello text odpovědi hello.         |
| _záhlaví_ | Objekt, který obsahuje hello hlavičky odpovědi.             |
| _isRaw_   | Označuje, že formátování bylo přeskočeno pro odpověď hello.    |
| _Stav_  | Hello stavový kód HTTP odpovědi hello.                     |

### <a name="accessing-hello-request-and-response"></a>Přístup k hello žádosti a odpovědi 

Při práci s aktivace protokolu HTTP, můžete přístup k objektům požadavku a odpovědi HTTP hello v jednom ze tří způsobů:

+ Z hello s názvem vstup a výstup vazby. Tímto způsobem hello triggeru protokolu HTTP a pracovní vazby hello stejné jako druhé vazby. Hello následující příklad nastaví objektu odpovědi hello pomocí pojmenovaná `response` vazby: 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ Z `req` a `res` vlastnosti hello `context` objektu. Tímto způsobem můžete hello konvenční vzor tooaccess HTTP data z objektu context hello, místo nutnosti toouse hello úplné `context.bindings.name` vzor. Následující příklad ukazuje, jak Hello tooaccess hello `req` a `res` objekty v hello `context`:

    ```javascript
    // You can access your http request off hello context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Při volání `context.done()`. Zvláštní druh vazby HTTP vrátí hello odpověď, který je předán toohello `context.done()` metoda. výstup Hello následující HTTP definuje vazbu `$return` výstupní parametr:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    Tato vazba výstup očekává, že budete toosupply hello odpovědi při volání `done()`, a to takto:

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a>Uzel verze a správy balíčků
verze uzlu Hello je aktuálně uzamčena v `6.5.0`. Jsme se na odstranění příčin přidání podpory pro další verze a jejich zpřístupnění konfigurovat.

Hello následující kroky umožňují mezi ně patřit balíčky v aplikaci funkce: 

1. Přejděte příliš`https://<function_app_name>.scm.azurewebsites.net`.

2. Klikněte na tlačítko **ladění konzoly** > **CMD**.

3. Přejděte příliš`D:\home\site\wwwroot`a poté přetáhněte vaší toohello soubor package.json **wwwroot** složku v horní polovině hello hello stránky.  
    Také můžete nahrát aplikaci funkce tooyour soubory jinými způsoby. Další informace najdete v tématu [jak tooupdate funkce soubory aplikace](functions-reference.md#fileupdate). 

4. Po odeslání soubor package.json hello spustit hello `npm install` v hello **Kudu vzdálené spuštění konzoly**.  
    Tato akce stáhne hello balíčky uvedené v souboru package.json hello a restartuje aplikaci funkce hello.

Po hello potřebujete jsou nainstalované balíčky, importu funkce tooyour voláním `require('packagename')`, jako v hello následující ukázka:

```javascript
// Import hello underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

Měli byste `package.json` souboru v kořenovém hello funkce aplikace. Definiční soubor hello umožňuje všechny funkce ve sdílené složce aplikace hello hello stejné balíčky v mezipaměti, což dává hello nejlepší výkon. Pokud dojde ke konfliktu verze, abyste ho mohli vyřešit přidáním `package.json` souboru ve složce hello konkrétní funkce.  

## <a name="environment-variables"></a>Proměnné prostředí
tooget proměnné prostředí nebo hodnotu nastavení aplikace, použijte `process.env`, jak ukazuje následující příklad kódu hello:

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
## <a name="considerations-for-javascript-functions"></a>Důležité informace týkající se funkce jazyka JavaScript

Při práci s funkce jazyka JavaScript, mějte na paměti aspektů hello v hello následující dvě části.

### <a name="choose-single-core-app-service-plans"></a>Zvolte jednojádrový plány služby App Service

Když vytvoříte aplikaci funkce, která používá hello plán služby App Service, doporučujeme vybrat plán jednojádrový spíše než plán s více jádry. V současné době funkce jazyka JavaScript funkce efektivněji běží na virtuálních počítačích jedním jádrem a větší virtuální počítače pomocí nevytváří vylepšení výkonu hello očekává. Pokud je to nezbytné, můžete ručně škálovat tak, že přidáte více instancí virtuálního počítače jedním jádrem nebo můžete povolit automatické škálování. Další informace najdete v tématu [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>Podpora typeScript a CoffeeScript
Protože přímé podpory ještě neexistuje pro automatické kompilaci TypeScript nebo CoffeeScript prostřednictvím hello runtime, musí tato podpora toobe zpracovává mimo hello runtime v době nasazení. 

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello následující prostředky:

* [Osvědčené postupy pro službu Azure Functions](functions-best-practices.md)
* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)
* [Azure funkcí jazyka C# referenční informace pro vývojáře](functions-reference-csharp.md)
* [Azure funkce F # referenční informace pro vývojáře](functions-reference-fsharp.md)
* [Azure funkce triggerů a vazeb](functions-triggers-bindings.md)

