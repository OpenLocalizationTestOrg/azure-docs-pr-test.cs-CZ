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
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure funkce protokolu HTTP a webhooku vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak se aktivuje tooconfigure a práce s protokoly HTTP a vazeb v Azure Functions.
Pomocí těchto můžete použít toowebhooks toobuild Azure Functions bez serveru rozhraní API a neodpověděl.

Azure Functions nabízí hello následující vazby:
- [Triggeru protokolu HTTP](#httptrigger) umožňuje vyvolají funkci s žádostí HTTP. To může být vlastní toorespond příliš[webhooky](#hooktrigger).
- [HTTP výstup vazby](#output) vám umožní toorespond toohello požadavku.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a>Trigger HTTP
Aktivace protokolu HTTP Hello spustí funkce v požadavku HTTP tooan odpovědi. Můžete ji přizpůsobit toorespond tooa konkrétní adresu URL nebo sady metod HTTP. Aktivační událost INSTEAD HTTP může být také nakonfigurované toorespond toowebhooks. 

Pokud používáte portál hello funkce, můžete také začít používat hned použití předem vytvořené šablony. Vyberte **novou funkci** a vybírat "Rozhraní API a Webhooky" hello **scénář** rozevíracího seznamu. Vyberte jednu z hello šablon a klikněte na tlačítko **vytvořit**.

Ve výchozím nastavení bude aktivační procedury HTTP odpovídat toohello žádost s stavový kód HTTP 200 OK a prázdným textem zprávy. toomodify hello odpovědi, nakonfigurovat [HTTP výstup vazby](#output)

### <a name="configuring-an-http-trigger"></a>Konfigurace aktivace protokolu HTTP
Aktivační událost INSTEAD HTTP je definována včetně toohello podobně jako objekt JSON následující v hello `bindings` pole function.json:

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
Vazba Hello podporuje hello následující vlastnosti:

* **název** : požadované – název proměnné hello používá v kódu funkce pro požadavek hello nebo textu požadavku. V tématu [práce s aktivační procedury HTTP z kódu](#httptriggerusage).
* **typ** : požadované – musí být nastaven příliš "httpTrigger".
* **směr** : požadované – musí být nastaven příliš "v".
* _authLevel_ : Určuje, jaké klíče, pokud existuje, třeba toobe hello požadavek neobsahuje ve funkci hello tooinvoke pořadí. V tématu [práci s klíči](#keys) níže. Hello hodnota může být jedna z následujících hello:
    * _Anonymní_: je požadován klíč rozhraní API ne.
    * _funkce_: je požadován klíč rozhraní API specifických funkcí. Toto je hello výchozí hodnotu, pokud žádný je k dispozici.
    * _správce_ : je nezbytný hlavní klíč hello.
* **metody** : Toto je pole metod hello HTTP bude odpovídat toowhich hello funkce. Pokud není zadáno, bude funkce hello odpovídat metody tooall HTTP. V tématu [přizpůsobení koncový bod hello HTTP](#url).
* **trasy** : definuje šablonu trasy hello, řízení toowhich žádosti o funkce bude odpovídat adresy URL. Hello výchozí hodnota není-li žádné je `<functionname>`. V tématu [přizpůsobení koncový bod hello HTTP](#url).
* **webHookType** : tím se nakonfiguruje tooact aktivační událost hello HTTP jako reciever webhooku pro zadaného zprostředkovatele hello. Hello _metody_ vlastnost neměla by být nastavená, pokud to jste vybrali. V tématu [odpovídá toowebhooks](#hooktrigger). Hello hodnota může být jedna z následujících hello:
    * _genericJson_ : webhooku koncový bod obecné účely bez logiku pro konkrétního zprostředkovatele.
    * _github_ : funkce hello bude odpovídat tooGitHub webhooky. Hello _authLevel_ vlastnost neměla by být nastavená, pokud to jste vybrali.
    * _slack_ : funkce hello bude odpovídat tooSlack webhooky. Hello _authLevel_ vlastnost neměla by být nastavená, pokud to jste vybrali.

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a>Práce s aktivační procedury HTTP z kódu
Pro funkce C# a F #, je možné deklarovat hello typ vaše vstupní toobe aktivační události buď `HttpRequestMessage` nebo vlastního typu. Pokud se rozhodnete `HttpRequestMessage`, pak budete mít objekt žádosti toohello úplný přístup. Pro vlastní typ (například POCO) funkce pokusí textu žádosti hello tooparse jako vlastnosti objektu hello toopopulate JSON.

Pro funkce Node.js hello Functions runtime poskytuje textu hello žádosti namísto objektu žádosti hello.

V tématu [HTTP aktivační událost ukázky](#httptriggersample) například použití.


<a name="output"></a>
## <a name="http-response-output-binding"></a>Odpověď HTTP výstup vazby
Použijte hello HTTP výstup vazby toorespond toohello HTTP žádost odesílatele. Tato vazba vyžaduje aktivační procedury protokolu HTTP a umožňuje vám toocustomize hello odpověď přidruženou k požadavek aktivace hello. Pokud výstup vazba HTTP není zadáno, aktivační procedury HTTP vrátí s prázdným textem zprávy HTTP 200 OK. 

### <a name="configuring-an-http-output-binding"></a>Konfigurace HTTP výstup vazby
výstup Hello HTTP vazba je definována včetně toohello podobně jako objekt JSON následující v hello `bindings` pole function.json:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
Vazba Hello obsahuje hello následující vlastnosti:

* **název** : požadované - hello používá v kódu funkce pro odpověď hello název proměnné. V tématu [práci s HTTP výstup vazba z kódu](#outputusage).
* **typ** : požadované – musí být nastaven příliš "http".
* **směr** : požadované – musí být nastaven příliš "na".

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a>Práce s HTTP výstup vazba z kódu
Můžete použít hello výstupní parametr (například "res") toorespond toohello protokolu http nebo webhooku volajícího. Alternativně můžete použít standardní `Request.CreateResponse()` (C#) nebo `context.res` (Node.JS) vzor tooreturn vaši odpověď. Příklady na tom, jak toouse hello druhá metoda najdete v tématu [HTTP aktivační událost ukázky](#httptriggersample) a [ukázky aktivační události Webhooku](#hooktriggersample).


<a name="hooktrigger"></a>
## <a name="responding-toowebhooks"></a>Odpovídá toowebhooks
Aktivační událost INSTEAD HTTP s hello _webHookType_ , bude mít vlastnost nakonfigurované toorespond příliš[webhooky](https://en.wikipedia.org/wiki/Webhook). základní konfigurace Hello používá nastavení "genericJson" hello. To omezuje požadavky tooonly ty pomocí protokolu HTTP POST a s hello `application/json` typ obsahu.

Hello aktivační událost se dá dál šité na míru tooa konkrétní webhooku zprostředkovatele (například [Githubu](https://developer.github.com/webhooks/) a [Slack](https://api.slack.com/outgoing-webhooks)). Pokud je zadán poskytovatele, hello Functions runtime mohou starat o logiku hello zprostředkovatele ověření pro vás.  

### <a name="configuring-github-as-a-webhook-provider"></a>Konfigurace jako zprostředkovatel webhook Githubu
toorespond tooGitHub webhooků, nejprve vytvořte funkce s aktivační procedury protokolu HTTP a nastavit hello _webHookType_ vlastnost příliš "githubu". Zkopírujte jeho [URL](#url) a [klíč rozhraní API](#keys) do úložiště GitHub **přidat webhooku** stránky. Najdete v článku na Githubu [vytváření Webhooky](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409) dokumentaci další informace.

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

### <a name="configuring-slack-as-a-webhook-provider"></a>Konfigurace systému Slack jako zprostředkovatel webhooku
Slack webhooku Hello generuje token pro vás místo umožňuje zadat, takže je nutné nakonfigurovat specifické funkce klíč pomocí hello tokenu z Slack. V tématu [práci s klíči](#keys).

<a name="url"></a>
## <a name="customizing-hello-http-endpoint"></a>Přizpůsobení koncový bod HTTP hello
Ve výchozím nastavení při vytváření funkce triggeru protokolu HTTP, nebo Webhooku, funkce hello je adresovatelný s trasou hello formuláře:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Můžete přizpůsobit tuto trasu pomocí hello volitelné `route` vlastnost na aktivační událost hello HTTP vstup vazby. Jako příklad hello následující *function.json* soubor definuje `route` vlastnost pro aktivační procedury HTTP:

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

Pomocí této konfigurace, hello funkce je nyní adresovatelné s hello následující trasy místo hello původní trasy.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

To umožňuje kód funkce hello toosupport dva parametry hello adresa, "kategorie" a "id". Můžete použít libovolnou [omezení trasy webové rozhraní API](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) s parametry. Dobrý den, následující funkce kód C# využívá oba parametry.

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

Zde je Node.js funkce kód toouse hello stejné parametry trasy.

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

Standardně jsou všechny funkce trasy předponou *rozhraní api*. Můžete také upravit nebo odebrat předpony hello pomocí hello `http.routePrefix` vlastnost ve vaší *host.json* souboru. Hello následující příklad odebere hello *rozhraní api* předpona trasy pomocí prázdný řetězec pro předponu hello hello *host.json* souboru.

```json
    {
      "http": {
        "routePrefix": ""
      }
    }
```

Podrobné informace o tom, tooupdate hello *host.json* funkce, najdete v souboru [jak tooupdate funkce soubory aplikace](functions-reference.md#fileupdate). 

Informace o dalších vlastností můžete nakonfigurovat v vaše *host.json* souborů najdete v tématu [host.json odkaz](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).


<a name="keys"></a>
## <a name="working-with-keys"></a>Práce s klíči
HttpTriggers můžete využít klíče pro zvýšení zabezpečení. Standardní HttpTrigger můžete použít jako klíč rozhraní API nutnosti hello klíče toobe hello požadavek neobsahuje. Webhooky pomocí klíče tooauthorize požadavků v mnoha různými způsoby v závislosti na tom, jaké hello zprostředkovatel podporuje.

Klíče jsou uložené v rámci funkce aplikace v Azure a jsou zašifrovaná přinejmenším. tooview klíče, vytvořit nové nebo klíčů pro vrácení hodnoty toonew, přejděte tooone funkcí portálu hello a vyberte "Manage". 

Existují dva typy klíčů:
- **Klíče hostitele**: tyto klíče jsou sdíleny všechny funkce v rámci aplikace hello funkce. Když se použije jako klíč rozhraní API, tyto rutiny umožňují funkce tooany přístup v rámci aplikace hello funkce.
- **Funkční klávesy**: tyto klíče použít jenom toohello specifické funkce, pod kterým jsou definovány. Když se použije jako klíč rozhraní API, povolí tyto pouze funkce toothat přístup.

Každý klíč jmenuje pro referenci a úrovni hello funkce a hostitele je výchozí klíč (s názvem "Výchozí"). Hello **hlavní klíč** je výchozí hostitele klíč s názvem "_master", která je definována pro každou aplikaci funkce a nesmí být odvolaný. Poskytuje přístup pro správu toohello runtime rozhraní API. Pomocí `"authLevel": "admin"` ve hello vazby JSON vyžaduje tento klíč toobe uvedené na žádost hello; Další klíč způsobí selhání autorizace.

> [!NOTE]
> Kvůli toohello zvýšená oprávnění udělené pomocí hello hlavního klíče, by neměly sdílet s třetími stranami tento klíč nebo distribuovat v nativní klientské aplikace. Při výběru hello úroveň oprávnění správce, postupujte opatrně.
> 
> 

### <a name="api-key-authorization"></a>Autorizace pro klíč rozhraní API
Ve výchozím nastavení vyžaduje HttpTrigger klíč rozhraní API v požadavku HTTP hello. Proto požadavku HTTP obvykle vypadá takto:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

klíč Hello můžou být součástí proměnnou řetězce dotazu s názvem `code`, jak je uvedeno výše, nebo ho můžou být součástí `x-functions-key` hlavičky protokolu HTTP. Hodnota Hello hello klíče může být jakékoli funkce pro funkci hello definováno, nebo všechny hostitele klíče.

Můžete zvolit tooallow požadavků bez klíče, nebo zadat, že hello hlavní klíč musí být použita změnou hello `authLevel` vlastnost hello vazby JSON (viz [triggeru protokolu HTTP](#httptrigger)).

### <a name="keys-and-webhooks"></a>Klíče a pomocí webhooků
Autorizace Webhooku se zpracovává hello webhooku reciever součásti, součástí hello HttpTrigger a hello mechanismus se může lišit podle typu webhooku hello. Jednotlivé mechanismy neodpovídá, ale závisí na klíč. Ve výchozím nastavení použije klíč hello funkce s názvem "Výchozí". Pokud chcete toouse jiný klíč, budete potřebovat tooconfigure hello zprostředkovatele toosend hello klíče název webhooku hello požadavku v jednom z následujících způsobů hello:

- **Řetězec dotazu**: hello zprostředkovatele předá název klíče hello hello `clientid` parametr řetězce dotazu (například `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`).
- **Hlavička požadavku**: hello zprostředkovatele předá název klíče hello hello `x-functions-clientid` záhlaví.

> [!NOTE]
> Funkční klávesy mají přednost před klíče hostitele. Pokud dva klíče jsou definovány s hello stejný název, hello funkce klíč se použije.
> 
> 


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a>Ukázky aktivace protokolu HTTP
Předpokládejme, že máte následující triggeru protokolu HTTP v hello hello `bindings` pole function.json:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

V tématu vzorku hello konkrétní jazyk, který hledá `name` parametr buď v řetězci dotazu hello nebo textu hello hello HTTP žádosti.

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

    // Set name tooquery string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on hello query string or in hello request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

Můžete také navázat tooa objektů POCO místo `HttpRequestMessage`. To bude HYDRATOVANÝ z textu hello hello požadavku, analyzovat jako JSON. Podobně typu lze předat výstup odezvy toohello HTTP vazby, a to bude vrácen jako hello odpovědi, které se 200 stavovým kódem.
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
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on hello query string or in hello request body")
    } |> Async.StartAsTask
```

Je nutné `project.json` souboru, který NuGet tooreference hello používá `FSharp.Interop.Dynamic` a `Dynamitey` sestavení, a to takto:

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

To bude používat NuGet toofetch svoje závislosti a bude odkazovat ve vašem skriptu.

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a>Ukázka aktivace protokolu HTTP v Node.JS
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
## <a name="webhook-samples"></a>Ukázky Webhooku
Předpokládejme, že máte následující aktivační události webhooku v hello hello `bindings` pole function.json:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

V tématu vzorku hello konkrétní jazyk, který protokoly komentáře problém Githubu.

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

