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
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a>Akce pracovního postupu a aktivačních událostí pro Azure Logic Apps

Aplikace logiky obsahovat triggery a akce. Existuje šest typů služby aktivačních událostí. Každý typ má jiné rozhraní a různé chování. Taky se dozvíte další podrobnosti pohledem na podrobnosti [jazyk definic workflowů](logic-apps-workflow-definition-language.md).  
  
Přečtěte si další informace o aktivační události a akce a jak je může využít k vytváření aplikací logiky ke zlepšení vaší podnikové procesy a pracovních postupů.  
  
### <a name="triggers"></a>Triggery  

Aktivační událost určuje volání, které můžete zahájit spuštění pracovního postupu aplikace logiky. Zde jsou dva různé způsoby, jak zahájit spuštění pracovního postupu:  
  
-   Aktivační událost dotazování  

-   Aktivační událost nabízené - voláním [rozhraní API REST služby pracovního postupu](https://docs.microsoft.com/rest/api/logic/workflows)  
  
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
    "splitOn" : "<property to create runs for>",
    "operationOptions": "<operation options on the trigger>"
}
```

### <a name="trigger-types-and-their-inputs"></a>Typy aktivační události a jejich vstupy  

Můžete použít tyto typy triggerů:
  
-   **Žádosti o** \- díky aplikaci logiky koncový bod pro vás k volání  
  
-   **Opakování** \- aktivuje podle definovaného plánu  
  
-   **HTTP** \- dotazuje koncový bod webové HTTP. Koncový bod HTTP musí odpovídat kontraktu konkrétní spouštěcí \- buď pomocí 202\-asynchronní vzor nebo vrácením pole  
  
-   **ApiConnection** \- aktivovat hlasování jako HTTP, ale provádí se [Microsoft spravovaná rozhraní API](https://docs.microsoft.com/azure/connectors/apis-list)  
  
-   **HTTPWebhook** \- otevře na koncový bod, podobně jako na ruční aktivaci, ale také zavolá zadanou adresu URL pro registraci a zrušit registraci  
  
-   **ApiConnectionWebhook** \- Operates jako HTTPWebhook aktivační událost pomocí rozhraní API spravovaný společností Microsoft       
    Každý typ aktivační událost má jinou sadu **vstupy** své chování, který definuje.  
  
## <a name="request-trigger"></a>Žádost o aktivační události  

Této aktivační události slouží jako koncový bod, který volání prostřednictvím požadavku HTTP k vyvolání svou aplikaci logiky. Aktivační událost požadavku vypadá v tomto příkladu:  
  
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
|Schéma|Ne|Schéma JSON, který ověří příchozí žádost. Užitečné pomáháte kroky následné pracovního postupu vědět, vlastnosti, které chcete odkazovat.|

K vyvolání tento koncový bod, je třeba volat *listCallbackUrl* rozhraní API. V tématu [rozhraní API REST služby pracovního postupu](https://docs.microsoft.com/rest/api/logic/workflows).  
  
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

Jak je vidět, je jednoduchý způsob, jak spustit workflow.  
  
|Název elementu|Požaduje se|Popis|  
|----------------|------------|---------------|  
|frekvence|Ano|Jak často se spustí aktivační událost. Použít pouze jednu z těchto možné hodnoty: sekundu, minutu, hodinu, den, týden, měsíc nebo rok|  
|Interval|Ano|Interval dané frekvenci opakování|  
|startTime|Ne|Pokud parametr startTime je poskytován bez časový posun, použije se toto časové pásmo.|  
|Časové pásmo|Ne|Pokud parametr startTime je poskytován bez časový posun, použije se toto časové pásmo.|  
  
Můžete také naplánovat aktivační události spustí provádění v určitém okamžiku v budoucnu. Například pokud chcete spustit sestavu týdenní každé pondělí můžete naplánovat aplikaci logiky začít každé pondělí tím vytváření následující aktivační události:  

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

Aktivace protokolu HTTP dotazování zadaný koncový bod a zkontrolujte odpověď na určení, zda by měl pracovní postup provádět. Objekt vstupy přijímá sadu parametrů požadovaných pro vytvoření volání protokolu HTTP:  
  
|Název elementu|Požaduje se|Popis|Typ|  
|----------------|------------|---------------|--------|  
|– Metoda|Ano|Může být jedna z následujících metod HTTP: GET, POST, PUT, DELETE, PATCH nebo HEAD|Řetězec|  
|identifikátor URI|Ano|Protokolu http nebo https koncový bod, který se označuje jako. Maximální počet kilobajtů 2.|Řetězec|  
|Dotazy|Ne|Objekt reprezentující parametry dotazu, které chcete přidat na adresu URL. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.|Objekt|  
|Záhlaví|Ne|Objekt, který reprezentuje všechny hlavičky, které posílá požadavek. Chcete-li například nastavení jazyka a typu na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|Objekt|  
|Text|Ne|Objekt představující datovou část, která je odeslána koncovému bodu.|Objekt|  
|retryPolicy|Ne|Objekt, který umožňuje přizpůsobit chování opakování 4xx nebo 5xx chyby.|Objekt|  
|Ověřování|Ne|Představuje metodu, žádost by měl ověřit. Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Nad scheduler, existuje více podporované jednu vlastnost: `authority` ve výchozím nastavení, tato hodnota je `https://login.windows.net` není-li zadána, ale můžete použít různé cílové skupiny jako`https://login.windows\-ppe.net`|Objekt|  
  
Aktivace protokolu HTTP vyžaduje rozhraní API HTTP tak, aby odpovídala vyhovujících určitému vzoru do funkce fungují dobře u aplikace logiky. Vyžaduje následující pole:  
  
|Odpověď|Popis|  
|------------|---------------|  
|Stavový kód|Stavovým kódem 200 \(OK\) způsobí spuštění. Další kód stavu nezpůsobí spustit.|  
|Opakujte\-po záhlaví|Počet sekund do aplikace logiky dotazuje koncový bod znovu.|  
|Hlavička umístění|Adresa URL pro volání na další interval dotazování. Pokud není zadaný, použije se původní adresu URL.|  
  
Zde je několik příkladů různých chování pro různé typy požadavků:  
  
|Kód odpovědi|Opakujte\-po|Chování|  
|-----------------|----------------|------------|  
|200|\(None\)|Není platný aktivační událost, opakování\-poté, co je to požadováno, nebo jinak modul nikdy dotazuje pro další požadavek.|  
|202|60|Nespouštějí pracovní postup. Další pokus se stane za jednu minutu.|  
|200|10|Spuštění pracovního postupu a znovu zkontrolujte pro další obsah za 10 sekund.|  
|400|\(None\)|Chybný požadavek, nespouštějte pracovního postupu. Pokud není žádná **opakujte zásad** definován, je použita výchozí zásady. Po překročení počet opakovaných pokusů, aktivační událost již není platný.|  
|500|\(None\)|Chyba serveru, nespouštějte pracovního postupu.  Pokud není žádná **opakujte zásad** definován, je použita výchozí zásady. Po překročení počet opakovaných pokusů, aktivační událost již není platný.|  
  
Výstupy aktivační procedury HTTP vypadat podobně jako tento příklad:  
  
|Název elementu|Popis|Typ|  
|----------------|---------------|--------|  
|Záhlaví|Hlavičky http odpovědi.|Objekt|  
|Text|Text odpovědi http.|Objekt|  
  
## <a name="api-connection-trigger"></a>Aktivační události připojení k rozhraní API  

Aktivační událost připojení rozhraní API je podobná triggeru protokolu HTTP v jeho základních funkcí. Parametry pro identifikaci akce se ale liší. Zde naleznete příklad:  
  
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
|hostitele|Ano||ApiApp hostované brány a id.|  
|– Metoda|Ano|Řetězec|Může být jedna z následujících metod HTTP: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo ** HEAD**|  
|Dotazy|Ne|Objekt|Představuje parametry dotazu, který se má přidat k adrese URL. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.|  
|Záhlaví|Ne|Objekt|Představuje všechny hlavičky, které posílá požadavek. Chcete-li například nastavení jazyka a typu na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Text|Ne|Objekt|Představuje datovou část, která je odeslána koncovému bodu.|  
|retryPolicy|Ne|Objekt|Umožňuje přizpůsobit chování opakování 4xx nebo 5xx chyby.|  
|Ověřování|Ne|Objekt|Představuje metodu, žádost by měl ověřit. Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)|  
  
Jsou vlastnosti pro hostitele:  
  
|Název elementu|Požaduje se|Popis|  
|----------------|------------|---------------|  
|rozhraní API runtimeUrl|Ano|Koncový bod spravované rozhraní API.|  
|Název připojení||Musí být odkaz na parametr s názvem `$connection` a je název připojení spravované rozhraní API, které používá pracovní postup.|
  
Výstupy aktivační událost pro připojení rozhraní API jsou:
  
|Název elementu|Typ|Popis|  
|----------------|--------|---------------|  
|Záhlaví|Objekt|Hlavičky http odpovědi.|  
|Text|Objekt|Text odpovědi http.|  
  
## <a name="httpwebhook-trigger"></a>Aktivační událost HTTPWebhook  

Aktivační událost HTTPWebhook otevře koncový bod, podobně jako ruční aktivační událost, ale HTTPWebhook aktivační událost se taky volá zadanou adresu URL pro registraci a zrušit registraci. Tady je příklad, jak může vypadat aktivační procedury HTTPWebhook:  

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

Mnoho z těchto částí je volitelné a chování Webhooku závisí na oddíly, které jsou zadané nebo tento parametr vynechán.  
Webhook, jehož vlastnosti jsou následující:  
  
|Název elementu|Požaduje se|Popis|  
|----------------|------------|---------------|  
|přihlášení k odběru|Ne|Odchozí požadavku, která je volána, když se vytvoří aktivační událost a provede počáteční registrace.|  
|Odhlásit|Ne|Odchozí požadavek při odstranění aktivační událost.|  
  
-   **Přihlášení k odběru** je odchozí hovoru, který provedl zahájit naslouchání na události. Toto volání začíná stejnou sadu parametrů, které provádějí normální akce HTTP. Tato odchozí přišla žádný čas pracovní postup změny žádným způsobem, například vždy, když přihlašovací údaje jsou vráceny, nebo změňte vstupní parametry aktivační události.
  
    Pro podporu toto volání, je nová funkce: `@listCallbackUrl()`. Tato funkce vrátí jedinečnou adresu URL pro tuto konkrétní aktivační událost v tomto pracovním postupu. Představuje jedinečný identifikátor pro koncové body, které používají služby REST.  
  
-   **Odhlášení** je volána, když operace vykreslí této aktivační události neplatná, včetně:  
  
    -   Odstranit nebo zakázat aktivační události  
  
    -   Odstranit nebo zakázat pracovní postup  
  
    -   Odstranit nebo zakázat odběr  
  
    Aplikace logiky automaticky zavolá akci zrušení odběru. Parametry této funkce jsou stejné jako triggeru protokolu HTTP.  
  
    Výstupy HTTPWebhook aktivační události jsou příchozí žádost o:  
  
|Název elementu|Typ|Popis|  
|-----------------|--------|---------------|  
|Záhlaví|Objekt|Hlavičky požadavku http.|  
|Text|Objekt|Text žádosti http.|  

Omezení pro akci webhooku lze zadat v stejným způsobem jako [HTTP asynchronní omezení](#asynchronous-limits).
  

## <a name="conditions"></a>Podmínky  

Pro všechny aktivační události můžete jednu nebo víc podmínek k určení, zda se má pracovní postup spustit nebo ne. Například:  

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

V takovém případě aktivuje pouze sestavy při pracovního postupu `sendReports` parametr je nastaven na hodnotu true. Nakonec může odkazovat na podmínky stavový kód aktivační události. Například může ji pracovního postupu jenom v případě, že váš web vrátí stavový kód 500, následujícím způsobem:
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> Když jakýkoli výraz odkazuje na kód stavu aktivační události \(žádným způsobem\), použije se výchozí chování \(aktivační událost pouze na 200 \(OK\) \) je nahrazena. Například pokud chcete aktivovat na stavovým kódem 200 a stavový kód 201, je nutné zahrnout: `@or(equals(triggers().code, 200),equals(triggers().code,201))` jako vaše podmínky.  
  
## <a name="start-multiple-runs-for-a-request"></a>Spuštění více spuštění pro žádost

Chcete-li ji více spuštění pro jeden požadavek, `splitOn` je užitečné, například, když chcete dotazování koncový bod, který může mít několik nových položek mezi intervaly cyklického dotazování.
  
S `splitOn`, určete vlastnost do datové části odpovědi, která obsahuje pole položek, každý z nich chcete použít ke spuštění spuštění aktivační události. Představte si například, že máte rozhraní API, které vrátí následující odpověď:  
  
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
  
Aplikace logiky pouze potřebuje obsah řádků, takže můžete vytvořit aktivační událost jako tento ukázkový:  
  
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
  
Potom v definici pracovního postupu `@triggerBody().name` vrátí `mycoolrow` pro první spuštění a `another row` pro druhý spustit. Vzhled výstupy aktivační událost následujícím způsobem:  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

Pokud používáte `SplitOn`, nelze získat vlastnosti, které jsou mimo pole, v takovém případě `Status` pole.  
  
> [!NOTE]  
> V tomto příkladu používáme `?` operátor moct nedošlo k selhání, pokud `Rows` vlastnost není k dispozici. 
  
## <a name="single-run-instance"></a>Spuštění jedné instance

Můžete nakonfigurovat aktivační události, které mají vlastnost opakování má pouze provést, pokud byly dokončeny všechny aktivní spustí. Pokud naplánované opakování dojde při spuštění s průběhem, aktivační událost přeskočí a čeká, až další interval opakování naplánované zkontrolujte znovu.

Můžete nakonfigurovat toto nastavení prostřednictvím možnosti operace:

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
  
-   **ApiConnection** \- tato akce se chová jako akce HTTP, ale používá rozhraní API spravovaný společností Microsoft.  
  
-   **ApiConnectionWebhook** \- jako HTTPWebhook, ale používá rozhraní API spravovaný společností Microsoft.  
  
-   **Odpověď** \- tato akce definuje odpovědi pro příchozí volání.  
  
-   **Počkejte** \- tím jednoduché čeká pevnou hodnotu čas, nebo dokud určitý čas.  
  
-   **Pracovní postup** \- tato akce představuje vnořený pracovní postup.  

-   **Funkce** \- tato akce představuje funkci Azure.

### <a name="collection-actions"></a>Kolekce akce

-   **Obor** \- tato akce je logické seskupení dalších akcí.

-   **Podmínka** \- tato akce vyhodnotí výraz a provede odpovídající firemní pobočky výsledek.

-   **ForEach** \- tato opakování akce iteruje v rámci pole a provede vnitřní akce pro každou položku.

-   **Dokud** \- tato opakování akce provede vnitřní akce, dokud podmínku výsledků na hodnotu true.
  
Každý typ akce má jinou sadu **vstupy** které definují chování akci.  
  
## <a name="http-action"></a>Akce HTTP  

Akce HTTP volání zadaný koncový bod a zkontrolujte odpovědi k určení, zda se má spustit pracovní postup. **Vstupy** objekt trvá sadu parametrů požadovaných pro vytvoření volání protokolu HTTP:  
  
|Název elementu|Požaduje se|Typ|Popis|  
|----------------|------------|--------|---------------|  
|– Metoda|Ano|Řetězec|Může být jedna z následujících metod HTTP: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo ** HEAD**|  
|identifikátor URI|Ano|Řetězec|Protokolu http nebo https koncový bod, který se označuje jako. Maximální délka je 2 kB.|  
|Dotazy|Ne|Objekt|Představuje parametry dotazu, které chcete přidat na adresu URL. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.|  
|Záhlaví|Ne|Objekt|Představuje všechny hlavičky, které posílá požadavek. Chcete-li například nastavení jazyka a typu na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Text|Ne|Objekt|Představuje datovou část, která je odeslána koncovému bodu.|  
|retryPolicy|Ne|Objekt|Umožňuje přizpůsobit chování opakování 4xx nebo 5xx chyby.|  
|operationsOptions|Ne|Řetězec|Definuje sadu zvláštní chování potlačit.|  
|Ověřování|Ne|Objekt|Představuje metodu, žádost by měl ověřit. Podrobnosti na tomto objektu najdete v tématu [odchozí ověření Scheduleru](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication). Nad scheduler, existuje více podporované jednu vlastnost: `authority`. Ve výchozím nastavení je to `https://login.windows.net` není-li zadána, ale můžete použít různé cílové skupiny jako`https://login.windows\-ppe.net`|  
  
Akce HTTP \(a připojení k rozhraní API\) podporu akce opakujte zásady. Zásady opakování platí pro náhodnými poruchami, vyznačují jako stavové kódy HTTP 408 429 a 5xx kromě všechny výjimky připojení. Tato zásada je popsán pomocí *retryPolicy* objekt definovaný jak je vidět tady:
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
Interval opakování je zadána ve formátu ISO 8601. Tento interval má výchozí a minimální hodnotu 20 sekund, zatímco maximální hodnota je jedna hodina. Výchozí a maximální počet opakování je čtyři hodiny. Pokud není zadaná definice zásady opakování, `fixed` strategie se používá s výchozí hodnoty a interval opakování. Chcete-li zakázat zásady opakovaných pokusů, nastavte její typ na `None`.  
  
Například následující akce opakované pokusy načítání nejnovější informace dvakrát, pokud existují náhodnými poruchami, celkem tři spuštěních s 30 sekund zpoždění mezi jednotlivými pokusy o:  
  
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

Ve výchozím nastavení všechny akce založené na protokolu HTTP podporují vzor standardní asynchronní operaci. Pokud vzdálený server označuje, zda je žádost přijaté ke zpracování s 202 \(platných\) odpovědi, modul Logic Apps udržuje dotazování adresa URL zadaná v odpovědi na umístění záhlaví, dokud nebude dosaženo stavu terminálu \(bez\-202 odpovědi\).  
  
Chcete-li zakázat asynchronní chování se popisuje výš, nastavte `DisableAsyncPattern` možnost v vstupy akce. V takovém případě výstup akce je založená na počáteční 202 odpověď ze serveru.  
  
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

Asynchronní vzor může být omezena pouze doby trvání na určitém časovém intervalu.  Pokud časový interval uplynutí bez dosažení stavu terminálu, budou označeny stav akce `Cancelled` s a kód `ActionTimedOut`.  Časový limit omezení je zadána ve formátu ISO 8601.  Omezení lze zadat pomocí následující syntaxe:

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
Tato akce vyžaduje odkaz na platné připojení a informace o rozhraní API a požadované parametry.

|Název elementu|Požaduje se|Typ|Popis|  
|----------------|------------|--------|---------------|  
|hostitele|Ano|Objekt|Představuje informace o konektoru například runtimeUrl a odkaz na objekt připojení|
|– Metoda|Ano|Řetězec|Může být jedna z následujících metod HTTP: **získat**, **POST**, **PUT**, **odstranit**, **oprava**, nebo ** HEAD**|  
|Cesta|Ano|Řetězec|Cesta operace rozhraní API.|  
|Dotazy|Ne|Objekt|Představuje parametry dotazu, které chcete přidat na adresu URL. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.|  
|Záhlaví|Ne|Objekt|Představuje všechny hlavičky, které posílá požadavek. Chcete-li například nastavení jazyka a typu na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Text|Ne|Objekt|Představuje datovou část, která je odeslána koncovému bodu.|  
|retryPolicy|Ne|Objekt|Umožňuje přizpůsobit chování opakování 4xx nebo 5xx chyby.|  
|operationsOptions|Ne|Řetězec|Definuje sadu zvláštní chování potlačit.|  

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

Omezení pro akci webhooku lze zadat v stejným způsobem jako [HTTP asynchronní omezení](#asynchronous-limits).
  
## <a name="response-action"></a>Akce odpovědi  

Tento typ akce obsahuje datové části celé odpovědi z požadavku HTTP a zahrnuje statusCode, text a hlavičky:  
  
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
  
Akce odpovědi obsahuje speciální omezení, která se nevztahují na dalších akcí. Zejména:  
  
-   Akce reagující nemůže být paralelní v definici, protože deterministickou odpovědi na příchozí požadavek je vyžadován.  
  
-   Pokud akce odpovědi je dostupný po obdržel odpověď příchozího požadavku, akce se považuje se nezdařilo \(konflikt\), a v důsledku toho je spustit `Failed`.  
  
-   Pracovní postup s akcemi odpověď nemůže mít `splitOn` v jeho aktivační událost protože jedno volání způsobí, že mnoho spustí. V důsledku toho to možné ověřit, když toku PUT a příčina chybný požadavek.  
  
## <a name="wait-action"></a>Počkejte akce  

`wait` Akce pozastaví spuštění pracovního postupu po zadanou dobu. Například 15 minut, než, můžete použít tento fragment kódu:  
  
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
  
Alternativně počkejte chvilku konkrétní v čase, můžete v tomto příkladu:  
  
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
> Doba čekání můžete buď zadat pomocí **interval** objekt nebo **dokud** objektu, ale ne obojí.  
  
|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|Interval|Ne|Objekt|Čekací doba trvání podle množství času.|  
|jednotku intervalu|Ano|Řetězec|Mezi těmito intervaly: sekundu, minutu, hodinu, den, týden, měsíc, rok.|  
|počet intervalu|Ano|Řetězec|Doba trvání na základě daného interní jednotky.|  
|dokud|Ne|Objekt|Čekací doba trvání podle bod v čase.|  
|dokud časové razítko|Ano|Řetězec|Řetězec & #124; Do bodu v čase ve standardu UTC, když vyprší platnost čekání.|  

## <a name="query-action"></a>Akce dotazu

`query` Akce umožňuje filtrovat pole na základě podmínky. Například pokud chcete vybrat čísla větší než 2, můžete použít:

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

Výstup `query` akce je pole, které má elementy ze vstupní pole, které splňují zadanou podmínku.

> [!NOTE]
> Pokud žádné hodnoty odpovídají `where` podmínka, výsledek je prázdné pole.

|Name (Název)|Požaduje se|Typ|Popis|
|--------|------------|--------|---------------|
|Z|Ano|Pole|Zdrojové pole.|
|kde|Ano|Řetězec|Podmínku, která má použít pro každý prvek zdrojové pole.|

## <a name="select-action"></a>Vyberte akci

`select` Akce umožňuje projektu každý element pole na novou hodnotu.
Například k převedení pole čísla do pole objektů, můžete použít:

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

Výstup `select` akce je pole, které má stejné mohutnost jako vstupní pole s každý prvek transformovat podle definice `select` vlastnost. Pokud vstup je prázdné pole, je výstup také prázdné pole.

|Name (Název)|Požaduje se|Typ|Popis|
|--------|------------|--------|---------------|
|Z|Ano|Pole|Zdrojové pole.|
|Vyberte|Ano|Všechny|Projekce, které chcete použít pro každý prvek zdrojové pole.|

## <a name="terminate-action"></a>Ukončit akce

Akce ukončení zastaví provedení spuštění pracovního postupu, přerušení všechny během letu akce a přeskočení všechny zbývající akce. Například pro ukončení spuštění se stavem **se nezdařilo**, můžete použít následující fragment kódu:

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
> Ukončit akce nemá vliv akce již byla dokončena.

|Name (Název)|Požaduje se|Typ|Popis|
|--------|------------|--------|---------------|
|runStatus|Ano|Řetězec|Cíl spustit stav. Buď **se nezdařilo** nebo **zrušena**.|
|runError|Ne|Objekt|Podrobnosti o chybě. Když podporována pouze **runStatus** je nastaven na **se nezdařilo**.|
|runError kódu|Ne|Řetězec|Kód chyby spuštění.|
|zpráva runError|Ne|Řetězec|Spuštění chybová zpráva.|

## <a name="compose-action"></a>Vytvořit akce

Vytvářené akce umožňuje vytvořit libovolný objekt. Výstup akce compose je výsledkem vyhodnocení vstupy. Například můžete použít akci vytvářené sloučit výstupy několik akcí:

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
> **Vytvářené** akci lze použít k vytvoření žádný výstup, včetně objektů, pole a jiný typ nativně podporuje logiku aplikace jako soubor XML a binární.

## <a name="table-action"></a>Tabulka akcí

`table` Umožňuje převod pole položek do **CSV** nebo **HTML** tabulky.

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

A mohli být definován jako akce

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

Vytvoří výše

<table><thead><tr><th>id</th><th>jméno</th></tr></thead><tbody><tr><td>0</td><td>jablka</td></tr><tr><td>1</td><td>Pomeranče</td></tr></tbody></table>"

Chcete-li přizpůsobit v tabulce, je explicitně zadat sloupce. Například:

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

Vytvoří výše

<table><thead><tr><th>vytvoření id</th><th>Popis</th></tr></thead><tbody><tr><td>0</td><td>Čerstvá jablka</td></tr><tr><td>1</td><td>čerstvé pomeranče</td></tr></tbody></table>"

Pokud `from` hodnota vlastnosti je prázdné pole, výstup je prázdná tabulka.

|Name (Název)|Požaduje se|Typ|Popis|
|--------|------------|--------|---------------|
|Z|Ano|Pole|Zdrojové pole.|
|Formát|Ano|Řetězec|Formát, buď **CSV** nebo **HTML**.|
|sloupce|Ne|Pole|Sloupce. Umožňuje přepsat výchozí tvar tabulky.|
|záhlaví sloupce|Ne|Řetězec|Záhlaví sloupce.|
|Hodnota sloupce|Ano|Řetězec|Hodnota sloupce.|

## <a name="workflow-action"></a>Akce pracovního postupu   

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|id hostitele|Ano|Řetězec|ID prostředku pracovního postupu, který chcete volat.|  
|Název aktivační události hostitele|Ano|Řetězec|Název aktivační události, kterou chcete volat.|  
|Dotazy|Ne|Objekt|Představuje parametry dotazu, které chcete přidat na adresu URL. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.|  
|Záhlaví|Ne|Objekt|Představuje všechny hlavičky, které posílá požadavek. Chcete-li například nastavení jazyka a typu na vyžádání:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`|  
|Text|Ne|Objekt|Představuje datová část odeslaná ke koncovému bodu.|  
  
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
  
Pro pracovní postup se provádí kontrolu přístupu \(přesněji řečeno, aktivační událost\), což znamená, potřebujete přístup do pracovního postupu.  
  
Výstup z `workflow` akce jsou založené na definované v `response` akce v pracovním postupu podřízené. Pokud nebyly definovány žádné `response` akce a potom výstupy jsou prázdné.  

## <a name="function-action"></a>Akce – funkce   

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|id – funkce|Ano|Řetězec|ID prostředku funkce, kterou chcete volat.|  
|– Metoda|Ne|Řetězec|Metodu protokolu HTTP použitou k vyvolání funkce. Ve výchozím nastavení, je `POST` není-li zadána.|  
|Dotazy|Ne|Objekt|Představuje parametry dotazu, které chcete přidat na adresu URL. Například `"queries" : { "api-version": "2015-02-01" }` přidá `?api-version=2015-02-01` na adresu URL.|  
|Záhlaví|Ne|Objekt|Představuje všechny hlavičky, které posílá požadavek. Chcete-li například nastavit jazyk a typ na vyžádání: `"headers" : { "Accept-Language": "en-us" }`.|  
|Text|Ne|Objekt|Představuje datová část odeslaná ke koncovému bodu.|  

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

Při ukládání aplikace logiky, můžeme provést nějaké kontroly na odkazované funkce:
-   Musíte mít přístup k funkci.
-   Jenom je povolen standard triggeru protokolu HTTP nebo obecné aktivační události webhooku JSON.
-   Všechny trasy definované by neměl mít.
-   Pouze "funkce" a "anonymní" oprávnění na úrovni je povolen.

Adresa URL aktivační události je načíst, do mezipaměti a použít za běhu. Takže pokud všechny operace by způsobila neplatnost adresu URL v mezipaměti, akce selže za běhu. Chcete-li tento problém obejít, uložte aplikaci logiky znovu, což způsobí, že aplikace logiky k načtení a znovu mezipaměti adresu URL aktivační události.

## <a name="collection-actions-scopes-and-loops"></a>Kolekce akce (rozsahy a smyčky)

Některé typy akcí může obsahovat akce v rámci sami. Odkaz akce v rámci kolekce může být odkaz přímo mimo kolekce. Pokud jste definovali `http` v oboru, `@body('http')` je stále platný kdekoli v pracovním postupu. Akce v rámci kolekce můžete `runAfter` jenom jiné akce v rámci stejné kolekce.

## <a name="scope-action"></a>Akce oboru

`scope` Akce umožňuje logicky akce skupiny v pracovním postupu.

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|Akce|Ano|Objekt|Vnitřní akce provést v rámci oboru|

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

Tato akce opakování iteruje v rámci pole a provádí vnitřní akce pro každou položku. Ve výchozím nastavení smyčka typu foreach spustí paralelně (20 spuštěních paralelně v čase). Budete moct nastavit pravidla spouštění pomocí `operationOptions` parametr.

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|Akce|Ano|Objekt|Vnitřní akce provést v rámci smyčky|
|foreach|Ano|Řetězec|Pole pro iterace|
|operationOptions|Ne|Řetězec|Všechny operace možnosti chování. Podporuje aktuálně pouze `sequential` postupně provést iterací (výchozí chování je paralelní)|

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

Tato opakování akce provede vnitřní akce, dokud podmínku výsledků na hodnotu true.

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|Akce|Ano|Objekt|Vnitřní akce provést v rámci smyčky|
|výraz|Ano|Řetězec|Výraz k vyhodnocení po každé iteraci|
|Limit|Ano|Objekt|Limity pro smyčky - alespoň jeden limit musí být definován.|
|Počet|Ne|celá čísla|Limit počtu opakování, které lze provést|
|Časový limit|Ne|Řetězec|Časový limit pro jak dlouho by měl cykly.  Formátu ISO 8601.|


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

`If` Akce umožňuje vyhodnotit podmínku a provést větev založené na tom, jestli je vyhodnocen `true`.

|Name (Název)|Požaduje se|Typ|Popis|  
|--------|------------|--------|---------------|  
|Akce|Ano|Objekt|Vnitřní akce má provést při výraz vyhodnocen`true`|
|výraz|Ano|Řetězec|Výraz k vyhodnocení|
|else|Ne|Objekt|Vnitřní akce má provést při výraz vyhodnocen`false`|
  
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
  
V následující tabulce jsou uvedeny příklady jak podmínky použití výrazů v akci:  
  
|Hodnota JSON|výsledek|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|Jakoukoli hodnotu, která by vyhodnotit na hodnotu true způsobí, že tato podmínka předat. Podporovány jsou pouze výrazy logických hodnot. Jiné typy převést na logickou hodnotu, použít funkce `empty`, `equals`.|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|Porovnání funkcí jsou podporovány. Například zde akce jenom provede, když se výstup act1 je větší než prahová hodnota.|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|Funkce logiky jsou podporovány také pro vytvoření vnořené výrazy logických hodnot. V takovém případě akci provede, když se výstup act1 je nad prahovou hodnotou nebo nižší než 100.|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|Funkce pole můžete použít ke kontrole, pokud má pole všechny položky. V takovém případě akci provede, když se pole chyby je prázdný.| 
|`"expression": "parameters('hasSpecialAction')"`|Chyba: není platný podmínky, protože @ je vyžadován pro podmínky.|  
  
Pokud je podmínka vyhodnocena jako úspěšně, podmínky je označen jako `Succeeded`. Akce v rámci buď `actions` nebo `else` objekty vyhodnocení `Succeeded` když spustí a byly úspěšné, `Failed` když spustí a se nezdařilo, nebo `Skipped` Pokud není uvedené pobočky provést.

## <a name="next-steps"></a>Další kroky

[Rozhraní API REST služby pracovního postupu](https://docs.microsoft.com/rest/api/logic/workflows)
