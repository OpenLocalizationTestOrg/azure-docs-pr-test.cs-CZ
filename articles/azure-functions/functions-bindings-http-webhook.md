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
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure funkce protokolu HTTP a webhooku vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje postup konfigurace a práce s HTTP triggerů a vazeb v Azure Functions.
Pomocí těchto můžete Azure Functions sestavení bez serveru rozhraní API a reagovat na webhooky.

Azure Functions nabízí následující vazby:
- [Triggeru protokolu HTTP](#httptrigger) umožňuje vyvolají funkci s žádostí HTTP. To lze přizpůsobit reagovat na [webhooky](#hooktrigger).
- [HTTP výstup vazby](#output) umožňuje odpovědět na požadavek.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a>Trigger HTTP
Aktivace protokolu HTTP bude vykonán funkce v odpovědi na požadavek HTTP. Můžete přizpůsobit ho reagovat na konkrétní adresu URL nebo sady metod HTTP. Aktivační událost INSTEAD HTTP lze také nakonfigurovat reagovat na webhooky. 

Pokud používáte portál funkce, můžete také začít používat hned použití předem vytvořené šablony. Vyberte **novou funkci** a zvolte "Rozhraní API a Webhooky" z **scénář** rozevíracího seznamu. Vyberte jednu z šablon a klikněte na **vytvořit**.

Ve výchozím nastavení bude aktivační procedury HTTP odpovědět na požadavek s kódem stavu HTTP 200 OK a prázdným textem zprávy. Chcete-li upravit odpověď, nakonfigurovat [HTTP výstup vazby](#output)

### <a name="configuring-an-http-trigger"></a>Konfigurace aktivace protokolu HTTP
Aktivační událost INSTEAD HTTP je definována včetně podobný následujícímu v objektu JSON `bindings` pole function.json:

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
Vazba podporuje následující vlastnosti:

* **název** : požadované – název proměnné používá v kódu funkce pro požadavek nebo textu požadavku. V tématu [práce s aktivační procedury HTTP z kódu](#httptriggerusage).
* **typ** : požadované – musí být nastavena na "httpTrigger".
* **směr** : požadované – musí být nastavena na "v".
* _authLevel_ : Určuje, co klíče, pokud existuje, musí být přítomen v požadavku k vyvolání funkce. V tématu [práci s klíči](#keys) níže. Hodnota může být jeden z následujících akcí:
    * _Anonymní_: je požadován klíč rozhraní API ne.
    * _funkce_: je požadován klíč rozhraní API specifických funkcí. Toto je výchozí hodnota, pokud žádný je k dispozici.
    * _správce_ : je nezbytný hlavní klíč.
* **metody** : Toto je pole metod HTTP, na které bude odpovídat funkce. Pokud není zadáno, bude funkce odpovídat na všechny metody HTTP. V tématu [přizpůsobení koncový bod HTTP](#url).
* **trasy** : definuje šablonu trasy, řízení, ke kterému žádosti adresy URL bude odpovídat funkce. Výchozí hodnota, pokud je zadaný žádný je `<functionname>`. V tématu [přizpůsobení koncový bod HTTP](#url).
* **webHookType** : tím se nakonfiguruje tak, aby fungoval jako reciever webhooku pro zadaného zprostředkovatele triggeru protokolu HTTP. _Metody_ vlastnost neměla by být nastavená, pokud to jste vybrali. V tématu [neodpovídá na požadavky webhooky](#hooktrigger). Hodnota může být jeden z následujících akcí:
    * _genericJson_ : webhooku koncový bod obecné účely bez logiku pro konkrétního zprostředkovatele.
    * _github_ : funkce bude reagovat na Githubu webhooky. _AuthLevel_ vlastnost neměla by být nastavená, pokud to jste vybrali.
    * _slack_ : funkce bude odpovídat Slack webhooky. _AuthLevel_ vlastnost neměla by být nastavená, pokud to jste vybrali.

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a>Práce s aktivační procedury HTTP z kódu
Pro funkce C# a F #, můžou deklarovat typ aktivační událost vstupu být buď `HttpRequestMessage` nebo vlastního typu. Pokud se rozhodnete `HttpRequestMessage`, pak budete mít plný přístup k objektu žádosti. Pro vlastní typ (například POCO) se pokusí funkce analyzovat datovou část požadavku jako JSON k naplnění vlastnosti objektu.

Pro funkce Node.js poskytuje Functions runtime textu žádosti namísto objektu žádosti.

V tématu [HTTP aktivační událost ukázky](#httptriggersample) například použití.


<a name="output"></a>
## <a name="http-response-output-binding"></a>Odpověď HTTP výstup vazby
Použijte protokol HTTP výstup vazby reagovat na odesílatel požadavku HTTP. Tato vazba vyžaduje aktivační procedury protokolu HTTP a umožňuje přizpůsobit odpověď přidružená k požadavku aktivační událost. Pokud výstup vazba HTTP není zadáno, aktivační procedury HTTP vrátí s prázdným textem zprávy HTTP 200 OK. 

### <a name="configuring-an-http-output-binding"></a>Konfigurace HTTP výstup vazby
HTTP výstup vazba je definována včetně podobný následujícímu v objektu JSON `bindings` pole function.json:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
Vazba obsahuje následující vlastnosti:

* **název** : požadované – název proměnné, které jsou použity v kódu funkce pro odpověď. V tématu [práci s HTTP výstup vazba z kódu](#outputusage).
* **typ** : požadované – musí být nastavena na "http".
* **směr** : požadované – musí být nastavena na "out".

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a>Práce s HTTP výstup vazba z kódu
Výstupní parametr (například "res") můžete reagovat na protokolu http nebo webhooku volajícího. Alternativně můžete použít standardní `Request.CreateResponse()` (C#) nebo `context.res` (Node.JS) vzor vrátit do odpovědi. Příklady použití druhé metody naleznete v tématu [HTTP aktivační událost ukázky](#httptriggersample) a [ukázky aktivační události Webhooku](#hooktriggersample).


<a name="hooktrigger"></a>
## <a name="responding-to-webhooks"></a>Odpovídá k webhookům
Aktivační událost INSTEAD HTTP s _webHookType_ vlastnost nakonfiguruje reagovat na [webhooky](https://en.wikipedia.org/wiki/Webhook). Základní konfigurace používá nastavení "genericJson". To omezuje požadavky pouze na ty pomocí protokolu HTTP POST a s `application/json` typ obsahu.

Aktivační událost, můžete přizpůsobit kromě poskytovatele konkrétní webhooku (například [Githubu](https://developer.github.com/webhooks/) a [Slack](https://api.slack.com/outgoing-webhooks)). Pokud je zadán poskytovatele, Functions runtime mohou starat o poskytovatele logiku ověření pro vás.  

### <a name="configuring-github-as-a-webhook-provider"></a>Konfigurace jako zprostředkovatel webhook Githubu
Chcete-li reagovat na Githubu webhooků, nejprve vytvoření funkce s aktivační procedury protokolu HTTP a nastavte _webHookType_ vlastnost "githubu". Zkopírujte jeho [URL](#url) a [klíč rozhraní API](#keys) do úložiště GitHub **přidat webhooku** stránky. Najdete v článku na Githubu [vytváření Webhooky](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) dokumentaci další informace.

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="configuring-slack-as-a-webhook-provider"></a>Konfigurace systému Slack jako zprostředkovatel webhooku
Slack webhooku generuje token pro vás místo umožňují určit, takže je nutné nakonfigurovat specifické funkce klíč pomocí tokenu z Slack. V tématu [práci s klíči](#keys).

<a name="url"></a>
## <a name="customizing-the-http-endpoint"></a>Přizpůsobení koncový bod HTTP
Ve výchozím nastavení vytvořit funkci pro triggeru protokolu HTTP, nebo Webhooku, funkce při adresovatelné s trasou ve tvaru:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Můžete přizpůsobit tuto trasu pomocí volitelné `route` vstup vazbu vlastnosti na triggeru protokolu HTTP. Jako příklad následující *function.json* soubor definuje `route` vlastnost pro aktivační procedury HTTP:

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

Pomocí této konfigurace, funkce je nyní adresovatelné s následující trasa místo původní trasy.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

To umožňuje kódu funkce, které podporují dva parametry adresa, "kategorie" a "id". Můžete použít libovolnou [omezení trasy webové rozhraní API](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) s parametry. Následující kód funkce jazyka C# využívá oba parametry.

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

Zde je kód Node.js funkce používat stejné parametry trasy.

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

Standardně jsou všechny funkce trasy předponou *rozhraní api*. Můžete také upravit nebo odebrat pomocí předpony `http.routePrefix` vlastnost ve vaší *host.json* souboru. Následující příklad odebere *rozhraní api* předpona trasy pomocí prázdný řetězec pro předponu v *host.json* souboru.

```json
    {
      "http": {
        "routePrefix": ""
      }
    }
```

Podrobné informace o tom, jak aktualizovat *host.json* funkce, najdete v souboru [jak aktualizovat soubory aplikace funkce](functions-reference.md#fileupdate). 

Informace o dalších vlastností můžete nakonfigurovat v vaše *host.json* souborů najdete v tématu [host.json odkaz](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).


<a name="keys"></a>
## <a name="working-with-keys"></a>Práce s klíči
HttpTriggers můžete využít klíče pro zvýšení zabezpečení. Standardní HttpTrigger můžete použít jako klíč rozhraní API nutnosti klíč nacházet v žádosti. Webhooky slouží k autorizaci požadavků v mnoha různými způsoby v závislosti na tom, co poskytovatel podporuje klíče.

Klíče jsou uložené v rámci funkce aplikace v Azure a jsou zašifrovaná přinejmenším. Chcete-li zobrazit vaše klíče, vytvořit nové, nebo vrátit klíče na nové hodnoty, přejděte do jednoho z funkcí v rámci portálu a vyberte "Manage". 

Existují dva typy klíčů:
- **Klíče hostitele**: tyto klíče jsou sdíleny všechny funkce v rámci funkce aplikace. Když se použije jako klíč rozhraní API, tyto rutiny umožňují přístup k žádné funkce v rámci funkce aplikace.
- **Funkční klávesy**: tyto klíče se vztahují pouze na určité funkce, pod kterým jsou definovány. Když se použije jako klíč rozhraní API, tyto pouze povolí přístup k této funkce.

Každý klíč jmenuje pro referenci a je výchozí klíč (s názvem "Výchozí") na úrovni funkce a hostitele. **Hlavní klíč** je výchozí hostitele klíč s názvem "_master", která je definována pro každou aplikaci funkce a nesmí být odvolaný. Poskytuje přístup pro modul runtime rozhraní API pro správu. Pomocí `"authLevel": "admin"` vazba JSON bude vyžadovat tento klíč prezentovány v žádosti; Další klíč způsobí selhání autorizace.

> [!NOTE]
> Z důvodu vyšší úroveň oprávnění udělená pomocí hlavního klíče nesmí sdílet s třetími stranami tento klíč nebo distribuovat v nativní klientské aplikace. Při výběru úroveň oprávnění správce, postupujte opatrně.
> 
> 

### <a name="api-key-authorization"></a>Autorizace pro klíč rozhraní API
Ve výchozím nastavení vyžaduje HttpTrigger klíč rozhraní API v požadavku HTTP. Proto požadavku HTTP obvykle vypadá takto:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

Klíč může být součástí proměnnou řetězce dotazu s názvem `code`, jak je uvedeno výše, nebo ho můžou být součástí `x-functions-key` hlavičky protokolu HTTP. Hodnota klíče může být jakékoli funkce definované pro tuto funkci, nebo všechny hostitele klíče.

Můžete povolit požadavky bez klíče nebo určíte, že hlavní klíč musí používat změnou `authLevel` vlastnost Vazba JSON (viz [triggeru protokolu HTTP](#httptrigger)).

### <a name="keys-and-webhooks"></a>Klíče a pomocí webhooků
Autorizace Webhooku se zpracovává souborem komponentu reciever webhooku součástí HttpTrigger, a tento mechanismus se může lišit podle typu webhooku. Jednotlivé mechanismy neodpovídá, ale závisí na klíč. Ve výchozím nastavení použije klíč funkce s názvem "Výchozí". Pokud chcete použít jiný kód, musíte nakonfigurovat poskytovatele webhooku odeslat název klíče s požadavkem v jednom z následujících způsobů:

- **Řetězec dotazu**: Zprostředkovatel předá název klíče v `clientid` parametr řetězce dotazu (například `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).
- **Hlavička požadavku**: Zprostředkovatel předá název klíče v `x-functions-clientid` záhlaví.

> [!NOTE]
> Funkční klávesy mají přednost před klíče hostitele. Pokud dva klíče jsou definovány se stejným názvem, použije se funkční klávesy.
> 
> 


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a>Ukázky aktivace protokolu HTTP
Předpokládejme, že máte následující triggeru protokolu HTTP `bindings` pole function.json:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

Viz ukázka konkrétní jazyk, které hledá `name` parametr buď v řetězci dotazu nebo textu požadavku HTTP.

* [C#](#httptriggercsharp)
* [F#](#httptriggerfsharp)
* [Node.js](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a>Ukázka aktivace protokolu HTTP v jazyce C# #
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

Také můžete vázat na objektů POCO místo `HttpRequestMessage`. To bude HYDRATOVANÝ z textu požadavku, analyzovat jako JSON. Podobně a typ se dá předat do výstupu odpovědi protokolu HTTP, vazby a to bude vrácen jako text odpovědi, se 200 stavovým kódem.
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
### <a name="http-trigger-sample-in-f"></a>Ukázka aktivační události protokolu HTTP v jazyce F # #
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

Je nutné `project.json` souboru, který používá NuGet tak, aby odkazovaly `FSharp.Interop.Dynamic` a `Dynamitey` sestavení, a to takto:

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

To bude používat NuGet načíst svoje závislosti a bude odkazovat ve vašem skriptu.

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a>Ukázka aktivace protokolu HTTP v Node.JS
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
## <a name="webhook-samples"></a>Ukázky Webhooku
Předpokládejme, že máte následující aktivační události webhooku `bindings` pole function.json:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

Naleznete v ukázce pro specifický jazyk, který zaznamenává komentáře problém Githubu.

* [C#](#hooktriggercsharp)
* [F#](#hooktriggerfsharp)
* [Node.js](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a>Ukázka Webhooku v jazyce C# #
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

### <a name="webhook-sample-in-f"></a>Ukázka Webhooku v jazyce F # #
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

### <a name="webhook-sample-in-nodejs"></a>Ukázka Webhooku v Node.JS
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

