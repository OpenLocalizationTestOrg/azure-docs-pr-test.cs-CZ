---
title: aaaTesting Azure Functions | Microsoft Docs
description: "Azure functions otestujte pomocí Postman, cURL a Node.js."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Azure funkce, funkce, zpracování událostí, webhooků, dynamické výpočetní, bez serveru architektura testování"
ms.assetid: c00f3082-30d2-46b3-96ea-34faf2f15f77
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/02/2017
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a084f8dbc8089356c3c19d789dc9098f2bb63052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a>Strategie pro testování kódu v Azure Functions

Toto téma popisuje různé způsoby tootest funkce, včetně použití hello následující obecné blíží hello:

+ Nástroje založené na protokolu HTTP, například cURL, Postman a i webový prohlížeč pro webové aktivační procedury
+ Azure Storage Explorer, aktivační události na základě Azure Storage tootest
+ Karta testu na portálu Azure Functions hello
+ Spustí časovač – funkce
+ Testování aplikace nebo framework

Všechny tyto metody testování pomocí funkce aktivační události protokolu HTTP, který přijímá vstupu prostřednictvím buď dotaz řetězec, nebo parametr hello obsah žádosti. Tato funkce vytvoříte v první části hello.

## <a name="create-a-function-for-testing"></a>Vytvořit funkci pro testování
Pro většinu v tomto kurzu používáme mírně upravenou verzi hello šablony funkce HttpTrigger JavaScript, která je dostupná, když vytvoříte funkci. Pokud potřebujete pomoc, vytváření funkce, přečtěte si to [kurzu](functions-create-first-azure-function.md). Zvolte hello **HttpTrigger - JavaScript** šablonu při vytváření hello testovací funkce v hello [portál Azure].

Hello výchozí šablony funkce je v podstatě "hello, world" funkce, která vrátí název back hello z hello textu nebo dotaz, řetězec parametr žádosti, `name=<your name>`.  Budeme budete aktualizovat kód hello tooalso umožňují tooprovide hello název a adresu jako obsah JSON v textu žádosti hello. Potom hello funkce vrátí tyto back toohello klienta, pokud je k dispozici.   

Aktualizujte hello funkce hello následující kód, který budeme používat pro testování:

```javascript
module.exports = function (context, req) {
    context.log("HTTP trigger function processed a request. RequestUri=%s", req.originalUrl);
    context.log("Request Headers = " + JSON.stringify(req.headers));
    var res;

    if (req.query.name || (req.body && req.body.name)) {
        if (typeof req.query.name != "undefined") {
            context.log("Name was provided as a query string param...");
            res = ProcessNewUserInformation(context, req.query.name);
        }
        else {
            context.log("Processing user info from request body...");
            res = ProcessNewUserInformation(context, req.body.name, req.body.address);
        }
    }
    else {
        res = {
            status: 400,
            body: "Please pass a name on hello query string or in hello request body"
        };
    }
    context.done(null, res);
};
function ProcessNewUserInformation(context, name, address) {
    context.log("Processing user information...");
    context.log("name = " + name);
    var echoString = "Hello " + name;
    var res;

    if (typeof address != "undefined") {
        echoString += "\n" + "hello address you provided is " + address;
        context.log("address = " + address);
    }
    res = {
        // status: 200, /* Defaults too200 */
        body: echoString
    };
    return res;
}
```

## <a name="test-a-function-with-tools"></a>Testování funkce nástroje
Mimo hello portálu Azure existují různé nástroje, které jsou používané tootrigger funkcí pro testování. Mezi ně patří HTTP testování nástroje (na základě uživatelského rozhraní i příkaz řádku), nástroje pro Azure Storage přístupu a i jednoduchý webový prohlížeč.

### <a name="test-with-a-browser"></a>Testování s prohlížečem
webový prohlížeč Hello je funkce tootrigger jednoduchý způsob prostřednictvím protokolu HTTP. Můžete použít prohlížeč pro požadavky GET, které nevyžadují datovou část textu a pouze dotazu použijte řetězec parametry.

Funkce hello tootest definovaného dříve, kopie hello **Url funkce** z portálu hello. Má následující formulář hello:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Připojit hello `name` parametr řetězce dotazu toohello. Použijte skutečný název pro hello `<Enter a name here>` zástupný symbol.

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>&name=<Enter a name here>

Adresa URL hello vložit do prohlížeče a měli získat následující toohello podobné a odpovědi.

![Snímek obrazovky Chrome kartu prohlížeče s odpovědí testu](./media/functions-test-a-function/browser-test.png)

V tomto příkladu je prohlížeč Chrome hello, který zabalí hello vrátil řetězec v kódu XML. Jiné prohlížeče zobrazí právě hello řetězcovou hodnotu.

V portálu hello **protokoly** okně výstup, podobně jako následující toohello přihlášen provádění hello funkce:

    2016-03-23T07:34:59  Welcome, you are now connected toolog-streaming service.
    2016-03-23T07:35:09.195 Function started (Id=61a8c5a9-5e44-4da0-909d-91d293f20445)
    2016-03-23T07:35:10.338 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==&name=Glenn from a browser
    2016-03-23T07:35:10.338 Request Headers = {"cache-control":"max-age=0","connection":"Keep-Alive","accept":"text/html","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T07:35:10.338 Name was provided as a query string param.
    2016-03-23T07:35:10.338 Processing User Information...
    2016-03-23T07:35:10.369 Function completed (Success, Id=61a8c5a9-5e44-4da0-909d-91d293f20445)

### <a name="test-with-postman"></a>Testování s Postman
Hello doporučuje nástroj tootest většinu funkcí je Postman, který se integruje s prohlížeč Chrome hello. tooinstall Postman, najdete v části [získat Postman](https://www.getpostman.com/). Postman umožňuje řídit mnoho dalších atributů požadavku HTTP.

> [!TIP]
> Pomocí protokolu HTTP hello testování se nástroj, který jste nejvíce vyhovuje. Zde jsou některé tooPostman alternativy:  
>
> * [Fiddler](http://www.telerik.com/fiddler)  
> * [Tlapa](https://luckymarmot.com/paw)  
>
>

Funkce hello tootest s obsah žádosti v Postman:

1. Spusťte Postman z hello **aplikace** tlačítka na hello levém horním rohu okna prohlížeče Chrome.
2. Kopie vašeho **Url funkce**a vložte jej do Postman. Obsahuje parametr řetězce dotazu hello přístupový kód.
3. Změnit metodu hello HTTP příliš**POST**.
4. Klikněte na tlačítko **textu** > **nezpracovaná**a přidejte JSON žádosti podobné toohello následující text:

    ```json
    {
        "name" : "Wes testing with Postman",
        "address" : "Seattle, WA 98101"
    }
    ```
5. Klikněte na tlačítko **odeslat**.

Hello následující obrázek ukazuje testování hello jednoduché echo funkce příklad v tomto kurzu.

![Snímek obrazovky Postman uživatelské rozhraní](./media/functions-test-a-function/postman-test.png)

V portálu hello **protokoly** okně výstup, podobně jako následující toohello přihlášen provádění hello funkce:

    2016-03-23T08:04:51  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:04:57.107 Function started (Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)
    2016-03-23T08:04:57.763 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==
    2016-03-23T08:04:57.763 Request Headers = {"cache-control":"no-cache","connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:04:57.763 Processing user info from request body...
    2016-03-23T08:04:57.763 Processing User Information...
    2016-03-23T08:04:57.763 name = Wes testing with Postman
    2016-03-23T08:04:57.763 address = Seattle, W.A. 98101
    2016-03-23T08:04:57.795 Function completed (Success, Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)

### <a name="test-with-curl-from-hello-command-line"></a>Testování pomocí cURL z příkazového řádku hello
Často, když testujete softwaru, není nutné toolook všechny další než hello příkazového řádku toohelp ladění aplikace. Nijak se to neliší při testování funkcí. Všimněte si, že hello cURL je k dispozici ve výchozím nastavení počítače se systémem Linux. V systému Windows, musíte nejdřív stáhnout a nainstalovat hello [cURL nástroj](https://curl.haxx.se/).

Funkce hello tootest že definovaného dříve, kopie hello **URL funkce** z portálu hello. Má následující formulář hello:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Toto je hello URL pro spuštění funkce. Otestování tohoto pomocí hello cURL příkazu na příkazovém řádku toomake hello GET (`-G` nebo `--get`) požadavku vůči hello funkce:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Tento příklad konkrétní vyžaduje parametr řetězce dotazu, které lze předat jako Data (`-d`) v hello cURL příkaz:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code> -d name=<Enter a name here>

Příkaz spusťte hello a najdete hello následující výstup hello funkce na příkazovém řádku hello:

![Výstup – snímek obrazovky příkazového řádku](./media/functions-test-a-function/curl-test.png)

V portálu hello **protokoly** okně výstup, podobně jako následující toohello přihlášen provádění hello funkce:

    2016-04-05T21:55:09  Welcome, you are now connected toolog-streaming service.
    2016-04-05T21:55:30.738 Function started (Id=ae6955da-29db-401a-b706-482fcd1b8f7a)
    2016-04-05T21:55:30.738 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/HttpTriggerNodeJS1?code=XXXXXXX&name=Azure Functions
    2016-04-05T21:55:30.738 Function completed (Success, Id=ae6955da-29db-401a-b706-482fcd1b8f7a)

### <a name="test-a-blob-trigger-by-using-storage-explorer"></a>Testovací aktivační události objektu blob pomocí Průzkumníka úložiště
Funkce aktivační události objektu blob můžete otestovat pomocí [Azure Storage Explorer](http://storageexplorer.com/).

1. V hello [portál Azure] pro vaši aplikaci funkce vytvořit jazyka C#, F # nebo JavaScript funkci aktivační události objektu blob. Nastavit hello toomonitor toohello název cesty kontejnerech objektů blob. Například:

        files
2. Klikněte na tlačítko hello  **+**  tlačítko tooselect nebo vytvořit účet úložiště hello chcete toouse. Poté klikněte na **Vytvořit**.
3. Vytvořte textový soubor s hello následující text a uložte jej:

        A text file for blob trigger function testing.
4. Spustit [Azure Storage Explorer](http://storageexplorer.com/)a připojte toohello kontejneru objektů blob v účtu úložiště hello monitorovány.
5. Klikněte na tlačítko **nahrát** tooupload hello textového souboru.

    ![Snímek obrazovky Průzkumníka úložiště](./media/functions-test-a-function/azure-storage-explorer-test.png)

Kód funkce aktivační události objektu blob výchozí Hello sestavy hello zpracování objektu blob hello hello protokolů:

    2016-03-24T11:30:10  Welcome, you are now connected toolog-streaming service.
    2016-03-24T11:30:34.472 Function started (Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)
    2016-03-24T11:30:34.472 C# Blob trigger function processed: A text file for blob trigger function testing.
    2016-03-24T11:30:34.472 Function completed (Success, Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)

## <a name="test-a-function-within-functions"></a>Testování funkce v rámci funkcí
Hello portálu Azure Functions je určená toolet testování HTTP a časovač aktivuje funkce. Můžete také vytvořit funkce tootrigger dalších funkcí, které jsou testování.

### <a name="test-with-hello-functions-portal-run-button"></a>Testování s tlačítko Spustit hello funkce portálu
poskytuje Hello portál **spustit** tlačítko, které můžete použít toodo některé omezené testování. Obsah žádosti můžete zadat pomocí tlačítka hello, ale nelze zadat parametrů řetězce dotazu nebo aktualizovat hlavičky žádosti.

Testování funkce aktivační událost hello HTTP jsme vytvořili předtím přidáním toohello podobné řetězec JSON následující hello **text žádosti** pole. Pak klikněte na tlačítko hello **spustit** tlačítko.

```json
{
    "name" : "Wes testing Run button",
    "address" : "USA"
}
```

V portálu hello **protokoly** okně výstup, podobně jako následující toohello přihlášen provádění hello funkce:

    2016-03-23T08:03:12  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:03:17.357 Function started (Id=753a01b0-45a8-4125-a030-3ad543a89409)
    2016-03-23T08:03:18.697 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/wesmchttptriggernodejs1
    2016-03-23T08:03:18.697 Request Headers = {"connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:03:18.697 Processing user info from request body...
    2016-03-23T08:03:18.697 Processing User Information...
    2016-03-23T08:03:18.697 name = Wes testing Run button
    2016-03-23T08:03:18.697 address = USA
    2016-03-23T08:03:18.744 Function completed (Success, Id=753a01b0-45a8-4125-a030-3ad543a89409)


### <a name="test-with-a-timer-trigger"></a>Testování s aktivační událost časovače
Některé funkce nelze testovat adekvátní nástroje hello již bylo zmíněno dříve. Představte si třeba funkci fronty aktivační událost, která se spustí v případě, že je vyřazeno zprávu do [Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md). Lze vždy psát kód toodrop zprávu do fronty, a příklady v konzolovém projektu se poskytuje později v tomto článku. Je však jiný přístup, které můžete použít, který testuje funkce přímo.  

Můžete použít aktivační událost časovače nakonfigurovaný s frontou výstup vazby. Tento časovač aktivační kód pak můžete napsat hello zkušební zprávy toohello fronty. Tato část vás provede příklad.

Další podrobné informace o používání vazby s Azure Functions najdete v tématu hello [referenční informace pro vývojáře Azure Functions](functions-reference.md).

#### <a name="create-a-queue-trigger-for-testing"></a>Vytvořit aktivační událost fronty pro testování
toodemonstrate tento přístup, jsme nejprve vytvořit funkci fronty aktivační událost, která chceme tootest pro frontu s názvem `queue-newusers`. Tato funkce zpracovává informace o názvu a adresy do fronty úložiště pro nového uživatele.

> [!NOTE]
> Pokud používáte jiný fronty názvu, ujistěte se, můžete použít název hello vyhovuje toohello [pojmenování front a MetaData](https://msdn.microsoft.com/library/dd179349.aspx) pravidla. Jinak dojde k chybě.
>
>

1. V hello [portál Azure] funkce aplikace, klikněte na tlačítko **novou funkci** > **QueueTrigger - C#**.
2. Zadejte toobe název fronty hello monitorovat pomocí funkce fronty hello:

        queue-newusers
3. Klikněte na tlačítko hello  **+**  tlačítko tooselect nebo vytvořit účet úložiště hello chcete toouse. Poté klikněte na **Vytvořit**.
4. Nechte toto okno prohlížeče portálu otevřené, tak můžete monitorovat hello položky protokolu pro kód šablony funkce fronty výchozí hello.

#### <a name="create-a-timer-trigger-toodrop-a-message-in-hello-queue"></a>Vytvořte toodrop aktivační události časovač zprávu ve frontě hello
1. Otevřete hello [portál Azure] v nové okno prohlížeče a přejděte tooyour funkce aplikace.
2. Klikněte na tlačítko **novou funkci** > **TimerTrigger - C#**. Zadejte tooset výraz cron jak často hello časovače kód testuje funkce fronty. Poté klikněte na **Vytvořit**. Pokud chcete testovací toorun hello každých 30 sekund, můžete použít následující hello [výraz CRON](https://wikipedia.org/wiki/Cron#CRON_expression):

        */30 * * * * *
3. Klikněte na tlačítko hello **integrací** kartě nové aktivační události časovače.
4. V části **výstup**, klikněte na tlačítko **+ nový výstupní**. Pak klikněte na tlačítko **fronty** a **vyberte**.
5. Název poznámky hello použijete pro hello **objekt fronty zpráv**. Můžete použít v kódu funkce časovače hello.

        myQueue
6. Zadejte název fronty hello, kde je odeslána zpráva hello:

        queue-newusers
7. Klikněte na tlačítko hello  **+**  tlačítko účet úložiště hello tooselect jste použili předtím se aktivační událostí hello fronty. Potom klikněte na **Uložit**.
8. Klikněte na tlačítko hello **vývoj** kartě aktivační události časovače.
9. Hello následující kód pro funkce časovače hello C#, můžete použít, dokud jste použili hello stejné fronty zpráv název objektu uvedena výše. Potom klikněte na **Uložit**.

    ```cs
    using System;

    public static void Run(TimerInfo myTimer, out String myQueue, TraceWriter log)
    {
        String newUser =
        "{\"name\":\"User testing from C# timer function\",\"address\":\"XYZ\"}";

        log.Verbose($"C# Timer trigger function executed at: {DateTime.Now}");   
        log.Verbose($"{newUser}");   

        myQueue = newUser;
    }
    ```

V tomto okamžiku hello časovače funkce jazyka C# spustí každých 30 sekund, pokud jste použili výraz cron příklad hello. Hello protokoly pro funkci časovače hello sestav každé spuštění:

    2016-03-24T10:27:02  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.004 Function started (Id=04061790-974f-4043-b851-48bd4ac424d1)
    2016-03-24T10:27:30.004 C# Timer trigger function executed at: 3/24/2016 10:27:30 AM
    2016-03-24T10:27:30.004 {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.004 Function completed (Success, Id=04061790-974f-4043-b851-48bd4ac424d1)

V okně prohlížeče hello hello fronty funkce můžete zjistit každou zprávu zpracovává:

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)

## <a name="test-a-function-with-code"></a>Testování funkce pomocí kódu
Může být nutné toocreate externí aplikace nebo framework tootest funkcí.

### <a name="test-an-http-trigger-function-with-code-nodejs"></a>Testování funkce aktivace protokolu HTTP s kódem: Node.js
Můžete vytvořit tooexecute aplikace Node.js tootest požadavku HTTP funkce.
Ujistěte se, že tooset:

* Hello `host` hello požadavek možnosti tooyour funkce aplikace hostiteli.
* Název funkce v hello `path`.
* Přístupový kód (`<your code>`) v hello `path`.

Příklad kódu:

```javascript
var http = require("http");

var nameQueryString = "name=Wes%20Query%20String%20Test%20From%20Node.js";

var nameBodyJSON = {
    name : "Wes testing with Node.JS code",
    address : "Dallas, T.X. 75201"
};

var bodyString = JSON.stringify(nameBodyJSON);

var options = {
  host: "functions841def78.azurewebsites.net",
  //path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9&" + nameQueryString,
  path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9",
  method: "POST",
  headers : {
      "Content-Type":"application/json",
      "Content-Length": Buffer.byteLength(bodyString)
    }    
};

callback = function(response) {
  var str = ""
  response.on("data", function (chunk) {
    str += chunk;
  });

  response.on("end", function () {
    console.log(str);
  });
}

var req = http.request(options, callback);
console.log("*** Sending name and address in body ***");
console.log(bodyString);
req.end(bodyString);
```


Výstup:

    C:\Users\Wesley\testing\Node.js>node testHttpTriggerExample.js
    *** Sending name and address in body ***
    {"name" : "Wes testing with Node.JS code","address" : "Dallas, T.X. 75201"}
    Hello Wes testing with Node.JS code
    hello address you provided is Dallas, T.X. 75201

V portálu hello **protokoly** okně výstup, podobně jako následující toohello přihlášen provádění hello funkce:

    2016-03-23T08:08:55  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:08:59.736 Function started (Id=607b891c-08a1-427f-910c-af64ae4f7f9c)
    2016-03-23T08:09:01.153 HTTP trigger function processed a request. RequestUri=http://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1/?code=XXXXXXXXXX==
    2016-03-23T08:09:01.153 Request Headers = {"connection":"Keep-Alive","host":"functionsExample.azurewebsites.net"}
    2016-03-23T08:09:01.153 Name not provided as query string param. Checking body...
    2016-03-23T08:09:01.153 Request Body Type = object
    2016-03-23T08:09:01.153 Request Body = [object Object]
    2016-03-23T08:09:01.153 Processing User Information...
    2016-03-23T08:09:01.215 Function completed (Success, Id=607b891c-08a1-427f-910c-af64ae4f7f9c)


### <a name="test-a-queue-trigger-function-with-code-c"></a>Testování funkce aktivační událost fronty s kódem: C# #
Jsme již bylo zmíněno dříve, můžete otestovat aktivační procedury fronty pomocí kódu toodrop zprávu ve frontě. Následující příklad kódu Hello podle hello C# kód uvedený v hello [Začínáme s Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md) kurzu. Kód pro jiné jazyky je také k dispozici prostřednictvím tohoto připojení.

tootest tento kód v konzolovou aplikaci, musíte:

* [Konfigurace připojovacího řetězce úložiště v souboru app.config hello](../storage/queues/storage-dotnet-how-to-use-queues.md).
* Předat `name` a `address` jako parametry toohello aplikaci. Například, `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`. (Tento kód přijímá hello název a adresu pro nového uživatele jako argumenty příkazového řádku za běhu.)

Příklad C# kódu:

```cs
static void Main(string[] args)
{
    string name = null;
    string address = null;
    string queueName = "queue-newusers";
    string JSON = null;

    if (args.Length > 0)
    {
        name = args[0];
    }
    if (args.Length > 1)
    {
        address = args[1];
    }

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["StorageConnectionString"]);

    // Create hello queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference tooa queue
    CloudQueue queue = queueClient.GetQueueReference(queueName);

    // Create hello queue if it doesn't already exist
    queue.CreateIfNotExists();

    // Create a message and add it toohello queue.
    if (name != null)
    {
        if (address != null)
            JSON = String.Format("{{\"name\":\"{0}\",\"address\":\"{1}\"}}", name, address);
        else
            JSON = String.Format("{{\"name\":\"{0}\"}}", name);
    }

    Console.WriteLine("Adding message too" + queueName + "...");
    Console.WriteLine(JSON);

    CloudQueueMessage message = new CloudQueueMessage(JSON);
    queue.AddMessage(message);
}
```

V okně prohlížeče hello hello fronty funkce můžete zjistit každou zprávu zpracovává:

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"Wes testing queues","address":"in a console app"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)


<!-- URLs. -->

[portál Azure]: https://portal.azure.com
