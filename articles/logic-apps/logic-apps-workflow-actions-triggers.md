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
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a>Akce pracovního postupu a aktivačních událostí pro Azure Logic Apps

Aplikace logiky obsahovat triggery a akce. Existuje šest typů služby aktivačních událostí. Každý typ má jiné rozhraní a různé chování. Taky se dozvíte další podrobnosti prohlížením hello podrobnosti o hello [jazyk definic workflowů](logic-apps-workflow-definition-language.md).  
  
Přečtěte si další informace o aktivační události a akce a jak může využít toobuild logiku aplikace tooimprove toolearn podnikové procesy a pracovních postupů.  
  
### <a name="triggers"></a>Triggery  

Aktivační událost určuje hello volání, která můžete zahájit spuštění pracovního postupu aplikace logiky. Zde jsou hello dvěma různými způsoby tooinitiate spuštění pracovního postupu:  
  
-   Aktivační událost dotazování  

-   Aktivační událost nabízené - ve volání hello [rozhraní API REST služby pracovního postupu](https://docs.microsoft.com/rest/api/logic/workflows)  
  
Žádná z aktivačních událostí obsahovat tyto prvky nejvyšší úrovně:  
  
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

### <a name="trigger-types-and-their-inputs"></a>Typy aktivační události a jejich vstupy  

Můžete použít tyto typy triggerů:
  
-   **Žádosti o** \- díky aplikaci logiky hello koncového bodu je toocall  
  
-   **Opakování** \- aktivuje podle definovaného plánu  
  
-   **HTTP** \- dotazuje koncový bod webové HTTP. koncový bod Hello HTTP musí odpovídat tooa konkrétní spouštěcí kontrakt \- buď pomocí 202\-asynchronní vzor nebo vrácením pole  
  
-   **ApiConnection** \- aktivovat hlasování jako hello HTTP, ale provádí se hello [Microsoft spravovaná rozhraní API](https://docs.microsoft.com/azure/connectors/apis-list)  
  
-   **HTTPWebhook** \- otevře na koncový bod, podobně jako toohello ruční aktivační událost, ale také zavolá se tooa zadané adresy URL tooregister a zrušit registraci  
  
-   **ApiConnectionWebhook** \- funguje jako hello aktivační událost HTTPWebhook využitím hello Microsoft spravovaná rozhraní API       
    Každý typ aktivační událost má jinou sadu **vstupy** své chování, který definuje.  
  
## <a name="request-trigger"></a>Žádost o aktivační události  

Této aktivační události slouží jako koncový bod, který volání prostřednictvím požadavku HTTP tooinvoke svou aplikaci logiky. Aktivační událost požadavku vypadá v tomto příkladu:  
  
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

Je také volitelná vlastnost s názvem **schématu**:  
  
|Název elementu|Požaduje se|Popis|  
|----------------|------------|---------------|  
|Schéma|Ne|Schéma JSON, který ověří příchozí žádost hello. Užitečné pomáháte vědět, které vlastnosti tooreference kroky následné pracovního postupu.|

tooinvoke tento koncový bod, je nutné toocall hello *listCallbackUrl* rozhraní API. V tématu [rozhraní API REST služby pracovního postupu](https://docs.microsoft.com/rest/api/logic/workflows).  
  
## <a name="recurrence-trigger"></a>Aktivační událost opakování  

Aktivační událost opakování je ten, který spouští na základě podle definovaného plánu. V tomto příkladu může vypadat aktivační události:  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

Jak je vidět, je jednoduchý způsob toorun pracovního postupu.  
  
|Název elementu|Požaduje se|Popis|  
|----------------|------------|---------------|  
|frequency|Ano|Jak často hello trigger spustí. Použít pouze jednu z těchto možné hodnoty: sekundu, minutu, hodinu, den, týden, měsíc nebo rok|  
|interval|Ano|Interval hello zadané frekvence opakování hello|  
|startTime|Ne|Pokud parametr startTime je poskytován bez časový posun, použije se toto časové pásmo.|  
|Časové pásmo|Ne|Pokud parametr startTime je poskytován bez časový posun, použije se toto časové pásmo.|  
  
Můžete také naplánovat provádění toostart aktivační události v určitém okamžiku v budoucnu hello. Například pokud chcete, aby toostart týdenního sestavy každé pondělí můžete naplánovat hello logiku aplikace toostart každé pondělí vytvořením hello následující aktivační události:  

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

## <a name="http-trigger"></a>Trigger HTTP  

Aktivace protokolu HTTP dotazování zadaný koncový bod a zkontrolujte hello odpovědi toodetermine, zda se má provést hello pracovního postupu. objekt Hello vstupy přijímá hello sadu parametrů požadované tooconstruct volání protokolu HTTP:  
  
|Název elementu|Požaduje se|Popis|Typ|  
|----------------|------------|---------------|--------|  
|– Metoda|Ano|Může být jedna z následujících metod HTTP hello: GET, POST, PUT, DELETE, PATCH nebo HEAD|Řetězec|  
|identifikátor URI|Ano|koncový bod http nebo https, která je volána, Hello. Maximální počet kilobajtů 2.|Řetězec|  
|Dotazy|Ne|Objekt reprezentující hello dotazu parametry tooadd toohello URL. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.|Objekt|  
|Záhlaví|Ne|Objekt, který reprezentuje všechny hello hlavičky, které je odeslána žádost o toohello. Například tooset hello jazyk a typ na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|Objekt|  
|Text|Ne|Objekt reprezentující hello datové části, která je odeslána toohello koncový bod.|Objekt|  
|retryPolicy|Ne|Objekt, který umožňuje přizpůsobit chování opakování hello 4xx nebo 5xx chyby.|Objekt|  
|Ověřování|Ne|Představuje hello metoda, která hello požadavek by měl být ověřen. Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Nad scheduler, existuje více podporované jednu vlastnost: `authority` ve výchozím nastavení, tato hodnota je `https://login.windows.net` není-li zadána, ale můžete použít různé cílové skupiny jako`https://login.windows\-ppe.net`|Objekt|  
  
Aktivace protokolu HTTP Hello vyžaduje hello tooconform rozhraní API HTTP pomocí specifického vzoru toowork vhodná pro svou aplikaci logiky. Vyžaduje hello následující pole:  
  
|Odpověď|Popis|  
|------------|---------------|  
|Stavový kód|Stavovým kódem 200 \(OK\) toocause spustit. Další kód stavu nezpůsobí spustit.|  
|Opakujte\-po záhlaví|Počet sekund do aplikace logiky hello dotazuje hello koncový bod znovu.|  
|Hlavička umístění|Adresa URL toocall Hello na další interval dotazování hello. Pokud není zadaný, použije se původní adresu URL hello.|  
  
Zde je několik příkladů různých chování pro různé typy požadavků:  
  
|Kód odpovědi|Opakujte\-po|Chování|  
|-----------------|----------------|------------|  
|200|\(None\)|Není platný aktivační událost, opakování\-se po hello požadovanou nebo jiný modul nikdy hlasování pro další požadavek hello.|  
|202|60|Nespouštějí hello pracovního postupu. Další pokus o Hello se stane za jednu minutu.|  
|200|10|Spustit hello workflow a znovu zkontrolujte pro další obsah za 10 sekund.|  
|400|\(None\)|Chybný požadavek, nespouštějte hello pracovního postupu. Pokud neexistuje žádné **opakujte zásad** definován, je použita výchozí zásady hello. Po překročení hello počet opakovaných pokusů, hello aktivační událost již není platný.|  
|500|\(None\)|Chyba serveru, nespouštějte hello pracovního postupu.  Pokud neexistuje žádné **opakujte zásad** definován, je použita výchozí zásady hello. Po překročení hello počet opakovaných pokusů, hello aktivační událost již není platný.|  
  
Hello výstupy aktivační procedury HTTP vypadat podobně jako tento příklad:  
  
|Název elementu|Popis|Typ|  
|----------------|---------------|--------|  
|Záhlaví|Hello hlavičky odpovědi http hello.|Objekt|  
|Text|Hello text odpovědi http hello.|Objekt|  
  
## <a name="api-connection-trigger"></a>Aktivační události připojení k rozhraní API  

aktivační události připojení Hello rozhraní API je podobné toohello triggeru protokolu HTTP v jeho základní funkce. Hello parametry pro identifikaci hello akce se ale liší. Zde naleznete příklad:  
  
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

|Název elementu|Požaduje se|Typ|Popis|  
|----------------|------------|--------|---------------|  
|hostitele|Ano||Hello ApiApp hostované brány a id.|  
|– Metoda|Ano|Řetězec|Může být jedna z následujících metod HTTP hello: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo  **HEAD**|  
|Dotazy|Ne|Objekt|Představuje hello dotazu parametry toobe přidat adresu URL toohello. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.|  
|Záhlaví|Ne|Objekt|Představuje všechny hello hlavičky, které je odeslána žádost o toohello. Například tooset hello jazyk a typ na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Text|Ne|Objekt|Představuje datovou část hello odeslaný toohello koncový bod.|  
|retryPolicy|Ne|Objekt|Umožňuje vám toocustomize hello opakování chování 4xx nebo 5xx chyby.|  
|Ověřování|Ne|Objekt|Představuje hello metoda, která hello požadavek by měl být ověřen. Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)|  
  
jsou Hello vlastnosti pro hostitele:  
  
|Název elementu|Požaduje se|Popis|  
|----------------|------------|---------------|  
|rozhraní API runtimeUrl|Ano|koncový bod Hello hello spravované rozhraní API.|  
|Název připojení||Musí být parametr tooa odkaz nazvaný `$connection` a je název hello připojení hello spravované rozhraní API, které hello používá pracovní postup.|
  
Hello výstupy aktivační událost pro připojení rozhraní API jsou:
  
|Název elementu|Typ|Popis|  
|----------------|--------|---------------|  
|Záhlaví|Objekt|Hello hlavičky odpovědi http hello.|  
|Text|Objekt|Hello text odpovědi http hello.|  
  
## <a name="httpwebhook-trigger"></a>Aktivační událost HTTPWebhook  

Hello HTTPWebhook aktivační událost otevře koncový bod, podobně jako ruční aktivační událost toohello ale hello aktivační událost HTTPWebhook také voláním out tooa zadané adresy URL tooregister a zrušit registraci. Tady je příklad, jak může vypadat aktivační procedury HTTPWebhook:  

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

Mnoho z těchto částí je volitelné a hello chování hello Webhooku závisí na oddíly, které jsou zadané nebo tento parametr vynechán.  
Hello Webhook, jehož vlastnosti jsou následující:  
  
|Název elementu|Požaduje se|Popis|  
|----------------|------------|---------------|  
|přihlášení k odběru|Ne|Hello odchozí požadavku, která je volána, když hello aktivační události je vytvořen a provede počáteční registrace hello.|  
|Odhlásit|Ne|Hello odchozí požadavek při odstranění hello aktivační události.|  
  
-   **Přihlášení k odběru** je hello odchozí volání, které provedl naslouchání tooevents toostart. Toto volání začíná hello se stejnou sadu parametrů, které hello normální akce HTTP. Tato odchozí Přišla žádost žádné hello čas pracovní postup změny žádným způsobem, například vždy, když hello přihlašovací údaje jsou vráceny, nebo aktivační událost hello vstup změnu parametry.
  
    toosupport toto volání, je nová funkce: `@listCallbackUrl()`. Tato funkce vrátí jedinečnou adresu URL pro tuto konkrétní aktivační událost v tomto pracovním postupu. Reprezentuje hello jedinečný identifikátor pro hello koncové body, které používají hello služby REST.  
  
-   **Odhlášení** je volána, když operace vykreslí této aktivační události neplatná, včetně:  
  
    -   Odstranit nebo zakázat aktivační událost hello  
  
    -   Odstranit nebo zakázat pracovní postup hello  
  
    -   Odstranit nebo zakázat odběr hello  
  
    aplikace logiky Hello automaticky volá hello zrušit akci. Hello parametry jsou funkce toothis hello stejné jako hello triggeru protokolu HTTP.  
  
    Hello výstupy hello HTTPWebhook aktivační události jsou hello obsah hello příchozích požadavků:  
  
|Název elementu|Typ|Popis|  
|-----------------|--------|---------------|  
|Záhlaví|Objekt|Hello hlavičky požadavku http hello.|  
|Text|Objekt|Hello textu hello požadavku protokolu http.|  

Omezení pro akci webhooku lze zadat v hello stejným způsobem jako [HTTP asynchronní omezení](#asynchronous-limits).
  

## <a name="conditions"></a>Podmínky  

Pro všechny aktivační události můžete použít jeden nebo více podmínek toodetermine zda pracovní postup hello měly být spuštěny, nebo není. Například:  

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

V takovém případě hello pouze aktivační události sestavy při hello workflow `sendReports` tootrue je nastaven parametr. Nakonec podmínky odkazy na hello stavový kód hello aktivační události. Například může ji pracovního postupu jenom v případě, že váš web vrátí stavový kód 500, následujícím způsobem:
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> Když jakýkoli výraz odkazuje na hello stavový kód aktivační událost hello \(žádným způsobem\), hello výchozí chování \(aktivační událost pouze na 200 \(OK\) \) je nahrazena. Například pokud chcete tootrigger na stavovým kódem 200 a stavový kód 201, máte tooinclude: `@or(equals(triggers().code, 200),equals(triggers().code,201))` jako vaše podmínky.  
  
## <a name="start-multiple-runs-for-a-request"></a>Spuštění více spuštění pro žádost

tookick vypnout více spuštění pro jeden požadavek, `splitOn` je užitečné, například, když chcete toopoll koncový bod, který může mít několik nových položek mezi intervaly cyklického dotazování.
  
S `splitOn`, určíte hello vlastnost uvnitř datové části hello odpovědi, která obsahuje pole hello položek, z nichž každá má toouse toostart spustit hello aktivační události. Představte si například, že máte rozhraní API, které vrátí hello následující odpověď:  
  
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
  
Aplikace logiky stačí hello řádků obsahu, tak můžete vytvořit aktivační událost jako tento ukázkový:  
  
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
  
Potom v hello Definice pracovního postupu, `@triggerBody().name` vrátí `mycoolrow` pro hello nejprve spustit, a `another row` pro druhý spustit hello. Hello aktivační událost výstupy vypadají v tomto příkladu:  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

Pokud používáte `SplitOn`, nelze získat hello vlastnosti, které jsou mimo hello pole, v takovém případě hello `Status` pole.  
  
> [!NOTE]  
> V tomto příkladu používáme hello `?` operátor toobe možné tooavoid selhání, pokud hello `Rows` vlastnost není k dispozici. 
  
## <a name="single-run-instance"></a>Spuštění jedné instance

Můžete nakonfigurovat aktivační události, které obsahují ještě efektivněji opakování vlastnost tooonly, pokud byly dokončeny všechny aktivní spustí. Pokud naplánované opakování dojde při spuštění s průběhem, aktivační události hello přeskočí a čeká, až hello další naplánované opakování interval toocheck znovu.

Můžete nakonfigurovat toto nastavení prostřednictvím možnosti operaci hello:

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

## <a name="types-and-inputs"></a>Typy a vstupy  

Existuje mnoho typů akcí, každý s jedinečný chování. Akce kolekce může obsahovat mnoho dalších akcí v rámci samotného.

### <a name="standard-actions"></a>Standardní akce  

-   **HTTP** tato akce zavolá koncový bod webové HTTP.  
  
-   **ApiConnection** \- chová se tato akce jako hello akce HTTP, ale používá hello Microsoft spravovaná rozhraní API.  
  
-   **ApiConnectionWebhook** \- jako HTTPWebhook, ale hello používá rozhraní API pro spravovaný společností Microsoft.  
  
-   **Odpověď** \- tato akce definuje odpovědi pro příchozí volání.  
  
-   **Počkejte** \- tím jednoduché čeká pevnou hodnotu čas, nebo dokud určitý čas.  
  
-   **Pracovní postup** \- tato akce představuje vnořený pracovní postup.  

-   **Funkce** \- tato akce představuje funkci Azure.

### <a name="collection-actions"></a>Kolekce akce

-   **Obor** \- tato akce je logické seskupení dalších akcí.

-   **Podmínka** \- tato akce vyhodnotí výraz a provede hello odpovídající výsledek větev.

-   **ForEach** \- tato opakování akce iteruje v rámci pole a provede vnitřní akce pro každou položku.

-   **Dokud** \- tato opakování akce provede vnitřní akce, dokud podmínku výsledků tootrue.
  
Každý typ akce má jinou sadu **vstupy** které definují chování akci.  
  
## <a name="http-action"></a>Akce HTTP  

Akce HTTP volání zadaný koncový bod a zkontrolujte hello odpovědi toodetermine, zda text hello pracovní postup spuštěn. Hello **vstupy** objekt trvá hello sadu parametrů požadované tooconstruct hello HTTP volání:  
  
|Název elementu|Požaduje se|Typ|Popis|  
|----------------|------------|--------|---------------|  
|– Metoda|Ano|Řetězec|Může být jedna z následujících metod HTTP hello: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo  **HEAD**|  
|identifikátor URI|Ano|Řetězec|koncový bod http nebo https, která je volána, Hello. Maximální délka je 2 kB.|  
|Dotazy|Ne|Objekt|Představuje hello dotazu parametry tooadd toohello URL. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.|  
|Záhlaví|Ne|Objekt|Představuje všechny hello hlavičky, které je odeslána žádost o toohello. Například tooset hello jazyk a typ na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Text|Ne|Objekt|Představuje datovou část hello odeslaný toohello koncový bod.|  
|retryPolicy|Ne|Objekt|Umožňuje upravovat chování opakování hello 4xx nebo 5xx chyby.|  
|operationsOptions|Ne|Řetězec|Definuje sadu hello toooverride zvláštní chování.|  
|Ověřování|Ne|Objekt|Představuje hello metoda, která hello požadavek by měl být ověřen. Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Nad scheduler, existuje více podporované jednu vlastnost: `authority`. Ve výchozím nastavení je to `https://login.windows.net` není-li zadána, ale můžete použít různé cílové skupiny jako`https://login.windows\-ppe.net`|  
  
Akce HTTP \(a připojení k rozhraní API\) podporu akce opakujte zásady. Zásady opakování platí toointermittent selhání, vyznačují jako stav HTTP 408 429 a 5xx v výjimky připojení tooany přidání kódy. Tato zásada je popsán pomocí hello *retryPolicy* objekt definovaný jak je vidět tady:
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
interval opakování Hello je zadána ve formátu ISO 8601 hello. Tento interval má výchozí a minimální hodnotu 20 sekund, zatímco hello maximální hodnota je jedna hodina. Hello výchozí a maximální počet opakování je čtyři hodiny. Pokud není zadán hello definice zásady opakování, `fixed` strategie se používá s výchozí hodnoty a interval opakování. zásady opakovaných pokusů hello toodisable, nastavte její typ příliš`None`.  
  
Například hello následující akci opakovat načítání hello nejnovější informace dvakrát, pokud existují náhodnými poruchami, celkem tři spuštěních s 30 sekund zpoždění mezi jednotlivými pokusy o:  
  
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
### <a name="asynchronous-patterns"></a>Asynchronními vzory

Ve výchozím nastavení podporují všechny akce založené na protokolu HTTP hello standardní asynchronní operaci vzor. Takže pokud vzdálený server hello označuje této žádosti hello je přijaté ke zpracování s 202 \(platných\) odpovědi, modul Logic Apps hello udržuje dotazování hello adresa URL zadaná v hlavičce umístění hello odpovědi až do dosažení terminál Stav \(bez\-202 odpovědi\).  
  
asynchronní chování toodisable hello dříve popsané, nastavené `DisableAsyncPattern` možnost v hello akce vstupy. V takovém případě hello výstup hello akce je založená na počáteční 202 odpovědi ze serveru hello hello.  
  
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

#### <a name="asynchronous-limits"></a>Asynchronní omezení

Asynchronní vzor může být omezena pouze jeho trvání tooa specifického časového intervalu.  Pokud hello časový interval uplynutí bez dosažení stavu terminálu, budou označeny hello stav akce hello `Cancelled` s a kód `ActionTimedOut`.  časový limit omezení Hello je zadána ve formátu ISO 8601.  Omezení lze určit hello následující syntaxi:

``` json
"<action-name>": {
    "type": "workflow|webhook|http|apiconnectionwebhook|apiconnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="api-connection"></a>Připojení k rozhraní API  

Připojení k rozhraní API je akce, která odkazuje na konektor spravovaný společností Microsoft.
Tato akce vyžaduje odkaz tooa platné připojení a informace o hello rozhraní API a požadované parametry.

|Název elementu|Požaduje se|Typ|Popis|  
|----------------|------------|--------|---------------|  
|hostitele|Ano|Objekt|Představuje informace o konektoru hello například objekt připojení toohello hello pro runtimeUrl a referenční informace|
|– Metoda|Ano|Řetězec|Může být jedna z následujících metod HTTP hello: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo  **HEAD**|  
|Cesta|Ano|Řetězec|Cesta Hello operace hello rozhraní API.|  
|Dotazy|Ne|Objekt|Představuje hello dotazu parametry tooadd toohello URL. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.|  
|Záhlaví|Ne|Objekt|Představuje všechny hello hlavičky, které je odeslána žádost o toohello. Například tooset hello jazyk a typ na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Text|Ne|Objekt|Představuje datovou část hello odeslaný toohello koncový bod.|  
|retryPolicy|Ne|Objekt|Umožňuje upravovat chování opakování hello 4xx nebo 5xx chyby.|  
|operationsOptions|Ne|Řetězec|Definuje sadu hello toooverride zvláštní chování.|  

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

## <a name="api-connection-webhook-action"></a>Akce webhooku připojení k rozhraní API

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

Omezení pro akci webhooku lze zadat v hello stejným způsobem jako [HTTP asynchronní omezení](#asynchronous-limits).
  
## <a name="response-action"></a>Akce odpovědi  

Tento typ akce obsahuje datovou část celé odpovědi hello z požadavku HTTP a zahrnuje statusCode, text a hlavičky:  
  
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
  
Hello odpověď má zvláštní omezení, které se neuplatní tooother akce. Zejména:  
  
-   Akce reagující nemůže být paralelní v definici, protože příchozí požadavek toohello deterministický odpovědi je vyžadován.  
  
-   Pokud akce odpovědi je dostupný po příchozího požadavku hello obdržel odpověď, považuje hello akce se nezdařilo \(konflikt\), a v důsledku toho je hello spustit `Failed`.  
  
-   Pracovní postup s akcemi odpověď nemůže mít `splitOn` v jeho aktivační událost protože jedno volání způsobí, že mnoho spustí. V důsledku toho to možné ověřit, když hello toku PUT a příčina chybný požadavek.  
  
## <a name="wait-action"></a>Počkejte akce  

Hello `wait` akce pozastaví spuštění pracovního postupu pro hello zadaného intervalu. Například toowait 15 minut, můžete použít tento fragment kódu:  
  
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
  
Alternativně toowait dokud konkrétní chvíli v čase, můžete použít tento příklad:  
  
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
> Doba čekání Hello můžete buď zadat pomocí hello **interval** objekt nebo hello **dokud** objektu, ale ne obojí.  
  
|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|interval|Ne|Objekt|doba podle časového intervalu čekání na Hello.|  
|jednotku intervalu|Ano|Řetězec|Mezi těmito intervaly: sekundu, minutu, hodinu, den, týden, měsíc, rok.|  
|počet intervalu|Ano|Řetězec|Doba trvání podle hello zadané interní jednotky.|  
|dokud|Ne|Objekt|Hello Počkejte, než doba trvání podle bod v čase.|  
|dokud časové razítko|Ano|Řetězec|Řetězec &#124; hello bodu v čase ve standardu UTC, když vyprší platnost hello čekání.|  

## <a name="query-action"></a>Akce dotazu

Hello `query` akce umožňuje filtrovat pole na základě podmínky. Například tooselect čísla větší než 2, můžete použít:

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

Hello výstup hello `query` není pole, které má elementy z hello vstupní pole, které splňují podmínku hello akce.

> [!NOTE]
> Pokud žádné hodnoty splňují hello `where` podmínky, hello výsledkem je prázdné pole.

|Name (Název)|Požaduje se|Typ|Popis|
|--------|------------|--------|---------------|
|Z|Ano|Pole|Hello zdrojové pole.|
|kde|Ano|Řetězec|Hello podmínku tooapply tooeach element hello zdrojové pole.|

## <a name="select-action"></a>Vyberte akci

Hello `select` akce umožňuje projektu každý element pole na novou hodnotu.
Například tooconvert na pole čísla do pole objektů, můžete použít:

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

Hello výstup hello `select` akce je pole, které má hello kardinalitu to stejné jako hello vstupního pole s každý prvek transformovat jako definované hello `select` vlastnost. Pokud vstupní hello je prázdné pole, výstup hello je také prázdné pole.

|Name (Název)|Požaduje se|Typ|Popis|
|--------|------------|--------|---------------|
|Z|Ano|Pole|Hello zdrojové pole.|
|Vyberte|Ano|Všechny|Hello projekce tooapply tooeach element hello zdrojové pole.|

## <a name="terminate-action"></a>Ukončit akce

Hello akce ukončení zastaví provádění hello pracovního postupu spustit, Probíhá rušení všechny během letu akce a přeskočení všechny zbývající akce. Například tooterminate spustit stavem **se nezdařilo**, můžete použít hello následující fragment kódu:

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
> Akce již byla dokončena nemá vliv hello ukončit akci.

|Name (Název)|Požaduje se|Typ|Popis|
|--------|------------|--------|---------------|
|runStatus|Ano|Řetězec|cíl Hello spustit stav. Buď **se nezdařilo** nebo **zrušena**.|
|runError|Ne|Objekt|Podrobnosti o chybě Hello. Když podporována pouze **runStatus** je nastaven příliš**se nezdařilo**.|
|runError kódu|Ne|Řetězec|Hello spustit kód chyby.|
|zpráva runError|Ne|Řetězec|Hello spustit chybová zpráva.|

## <a name="compose-action"></a>Vytvořit akce

Hello vytvářené akce umožňuje vytvořit libovolný objekt. výstup Hello hello tvoří akce je hello výsledek vyhodnocení vstupy. Například můžete použít hello tvoří akce toomerge výstupy několik akcí:

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
> Hello **vytvářené** akce lze použít tooconstruct žádný výstup, včetně objektů, pole a jiný typ nativně podporuje logiku aplikace jako soubor XML a binární.

## <a name="table-action"></a>Tabulka akcí

Hello `table` vám umožní tooconvert na pole položek do **CSV** nebo **HTML** tabulky.

Předpokládejme, že @triggerBodyje)

```json
[{
  "id": 0,
  "name": "apples"
},{
  "id": 1, 
  "name": "oranges"
}]
```

A mohli být definován jako akce hello

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

Vytvoří Hello výše

<table><thead><tr><th>id</th><th>jméno</th></tr></thead><tbody><tr><td>0</td><td>jablka</td></tr><tr><td>1</td><td>Pomeranče</td></tr></tbody></table>"

Tabulka hello toocustomize pořadí můžete zadat hello sloupce explicitně. Například:

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

Vytvoří Hello výše

<table><thead><tr><th>vytvoření id</th><th>description</th></tr></thead><tbody><tr><td>0</td><td>Čerstvá jablka</td></tr><tr><td>1</td><td>čerstvé pomeranče</td></tr></tbody></table>"

Pokud hello `from` hodnota vlastnosti je prázdné pole, výstup hello je prázdná tabulka.

|Name (Název)|Požaduje se|Typ|Popis|
|--------|------------|--------|---------------|
|Z|Ano|Pole|Hello zdrojové pole.|
|Formát|Ano|Řetězec|Hello formátu, buď **CSV** nebo **HTML**.|
|sloupce|Ne|Pole|Hello sloupce. Umožňuje toooverride hello výchozí tvar hello tabulky.|
|záhlaví sloupce|Ne|Řetězec|Hello záhlaví sloupce hello.|
|Hodnota sloupce|Ano|Řetězec|Hodnota Hello hello sloupce.|

## <a name="workflow-action"></a>Akce pracovního postupu   

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|id hostitele|Ano|Řetězec|ID prostředku Hello hello pracovního postupu, které chcete toocall.|  
|Název aktivační události hostitele|Ano|Řetězec|Název Hello hello aktivační události, které chcete tooinvoke.|  
|Dotazy|Ne|Objekt|Představuje hello dotazu parametry tooadd toohello URL. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.|  
|Záhlaví|Ne|Objekt|Představuje všechny hello hlavičky, které je odeslána žádost o toohello. Například tooset hello jazyk a typ na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Text|Ne|Objekt|Představuje hello datová část odeslaná toohello koncový bod.|  
  
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
  
Kontrolu přístupu se provádí v pracovním postupu hello \(přesněji řečeno, aktivační události hello\), což znamená, že potřebujete přístup toohello pracovního postupu.  
  
Hello výstupy z hello `workflow` akce jsou založené na definované v hello `response` akce v pracovním postupu podřízené hello. Pokud nebyly definovány žádné `response` akce a potom hello výstupy jsou prázdné.  

## <a name="function-action"></a>Akce – funkce   

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|id – funkce|Ano|Řetězec|ID prostředku Hello hello funkce, které chcete tooinvoke.|  
|– Metoda|Ne|Řetězec|Metoda HTTP Hello používá tooinvoke hello funkce. Ve výchozím nastavení, je `POST` není-li zadána.|  
|Dotazy|Ne|Objekt|Představuje hello dotazu parametry tooadd toohello URL. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` toohello adresy URL.|  
|Záhlaví|Ne|Objekt|Představuje všechny hello hlavičky, které je odeslána žádost o toohello. Například tooset hello jazyk a typ na vyžádání: `"headers" : { "Accept-Language": "en-us" }`.|  
|Text|Ne|Objekt|Představuje hello datová část odeslaná toohello koncový bod.|  

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

Při ukládání aplikace logiky hello jsme provést nějaké kontroly na hello odkazuje funkce:
-   Je nutné funkce toohello toohave přístup.
-   Jenom je povolen standard triggeru protokolu HTTP nebo obecné aktivační události webhooku JSON.
-   Všechny trasy definované by neměl mít.
-   Pouze "funkce" a "anonymní" oprávnění na úrovni je povolen.

Adresa URL Hello aktivační události je načíst, do mezipaměti a použít za běhu. Takže pokud všechny operace by způsobila neplatnost URL hello do mezipaměti, hello akce selže za běhu. toowork kolem toho uložte hello aplikace logiky akci, která bude způsobit tooretrieve aplikace logiky a mezipaměti hello aktivační událost URL znovu.

## <a name="collection-actions-scopes-and-loops"></a>Kolekce akce (rozsahy a smyčky)

Některé typy akcí může obsahovat akce v rámci sami. Odkaz akce v rámci kolekce může být odkaz přímo mimo hello kolekce. Pokud jste definovali `http` v oboru, `@body('http')` je stále platný kdekoli v pracovním postupu. Akce v rámci kolekce můžete `runAfter` hello jenom jiné akce v rámci stejné kolekci.

## <a name="scope-action"></a>Akce oboru

Hello `scope` akce umožňuje logicky akce skupiny v pracovním postupu.

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|Akce|Ano|Objekt|Tooexecute vnitřní akce v rámci oboru hello|

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

## <a name="foreach-action"></a>Akce ForEach

Tato akce opakování iteruje v rámci pole a provádí vnitřní akce pro každou položku. Ve výchozím nastavení smyčka typu foreach hello spustí paralelně (20 spuštěních paralelně v čase). Budete moct nastavit pravidla spouštění pomocí hello `operationOptions` parametr.

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|Akce|Ano|Objekt|Tooexecute vnitřní akce v rámci hello smyčky|
|foreach|Ano|Řetězec|pole tooiterate Hello přes|
|operationOptions|Ne|Řetězec|Všechny operace možnosti chování. Aktuálně podporuje jenom `sequential` tooexecute iterací postupně (výchozí chování je paralelní)|

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

## <a name="until-action"></a>Dokud akce

Tato opakování akce provede vnitřní akce, dokud podmínku výsledků tootrue.

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|Akce|Ano|Objekt|Tooexecute vnitřní akce v rámci hello smyčky|
|výraz|Ano|Řetězec|výraz tooevaluate Hello po každé iteraci|
|Limit|Ano|Objekt|Hello limity pro hello smyčku – žádný limit musí být definován.|
|Počet|Ne|celá čísla|Hello limit toohello počet iterací, které lze provést|
|timeout|Ne|Řetězec|Hello časový limit pro jak dlouho by měl cykly.  Formátu ISO 8601.|


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

## <a name="conditions---if-action"></a>Podmínky - li akce

Hello `If` akce umožňuje vyhodnotit podmínku a provést větev založené na tom, zda text hello výraz vyhodnocen jako příliš`true`.

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|Akce|Ano|Objekt|Vnitřní akce tooexecute, pokud se výraz vyhodnotí jako příliš`true`|
|výraz|Ano|Řetězec|výraz tooevaluate Hello|
|else|Ne|Objekt|Vnitřní akce tooexecute, pokud se výraz vyhodnotí jako příliš`false`|
  
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
  
Hello následující tabulka uvádí příklady jak podmínky použití výrazů v akci:  
  
|Hodnota JSON|výsledek|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|Libovolná hodnota, které by vyhodnocovaly tootrue způsobí, že tato podmínka toopass. Podporovány jsou pouze výrazy logických hodnot. tooconvert jiné typy tooBoolean, použití funkcí `empty`, `equals`.|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|Porovnání funkcí jsou podporovány. Například hello zde hello akce jenom provede, když se výstup hello act1 je větší než prahová hodnota hello.|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|Funkce logiky jsou taky podporované toocreate vnořené výrazy logických hodnot. V takovém případě hello akce provede, když se výstup hello act1 je nad prahovou hodnotou hello nebo nižší než 100.|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|Pokud pole má všechny položky, můžete použít funkce toocheck pole. V takovém případě hello akce provede, když se pole chyby hello je prázdný.| 
|`"expression": "parameters('hasSpecialAction')"`|Chyba: není platný podmínky, protože @ je vyžadován pro podmínky.|  
  
Pokud je podmínka vyhodnocena jako úspěšně, hello podmínky je označen jako `Succeeded`. Akce v rámci buď hello `actions` nebo `else` objekty vyhodnocení příliš`Succeeded` když spustí a byly úspěšné, `Failed` při provést a selhala, nebo `Skipped` Pokud není uvedené pobočky provést.

## <a name="next-steps"></a>Další kroky

[Rozhraní API REST služby pracovního postupu](https://docs.microsoft.com/rest/api/logic/workflows)
