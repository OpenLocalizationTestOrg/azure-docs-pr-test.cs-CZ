---
title: vazby funkce Mobile Apps aaaAzure | Microsoft Docs
description: Pochopit, jak toouse vazeb Azure Mobile Apps v Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: faad1263-0fa5-41a9-964f-aecbc0be706a
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/31/2016
ms.author: glenga
ms.openlocfilehash: d3679a5d5c66705b32e422ec17e3a1e6d6ac063c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-mobile-apps-bindings"></a>Azure Mobile Apps funkce vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak tooconfigure a kód [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) vazeb v Azure Functions. Azure Functions podporuje vstup a výstup vazby pro Mobile Apps.

vstupní technologie Hello Mobile Apps a výstup vazby umožňují [číst a zapisovat toodata tabulky](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) v mobilní aplikaci.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a>Vstupní vazba Mobile Apps
Hello Vstupní vazba Mobile Apps načte záznam z koncového bodu mobilní tabulky a předá ji do funkce. V jazyce C# a F # funkce všechny změny provedené toohello záznam automaticky odešlou back toohello tabulky při ukončení hello funkce úspěšně.

Hello Mobile Apps vstupní tooa funkce používá následující objekt JSON v hello hello `bindings` pole function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of hello record tooretrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

Vezměte na vědomí následující hello:

* `id`může být statické nebo může být založen na hello aktivační událost, která volá funkce hello. Například, pokud používáte [aktivační událost fronty]() pro funkce, pak `"id": "{queueTrigger}"` používá hello řetězcovou hodnotou obsahující zprávu fronty hello jako hello záznamů tooretrieve ID.
* `connection`by mělo obsahovat název hello nastavení aplikace v aplikaci funkce, která obsahuje adresu URL hello mobilní aplikace. Funkce Hello používá tuto adresu URL tooconstruct hello požadované operace REST proti mobilní aplikaci. Můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující adresu URL mobilních aplikací (který vypadá podobně jako `http://<appname>.azurewebsites.net`), potom zadejte název nastavení aplikace hello hello do hello `connection` vlastnost Vstupní vazba. 
* Je třeba toospecify `apiKey` Pokud jste [implementovat klíč rozhraní API vašeho back-end mobilní aplikace Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), nebo [implementovat klíč rozhraní API vašeho back-end mobilní aplikace .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). toodo, můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující klíč hello rozhraní API, pak přidejte hello `apiKey` vlastnost vaše Vstupní vazba s hello název nastavení aplikace hello. 
  
  > [!IMPORTANT]
  > Tento klíč rozhraní API nesmí sdílet s klienty mobilní aplikace. By mělo být distribuované bezpečně tooservice straně klienty, jako jsou Azure Functions. 
  > 
  > [!NOTE]
  > Azure Functions ukládá informace o připojení a klíče rozhraní API jako nastavení aplikace, tak, aby se změnami do vašeho úložiště správy zdrojového kódu. To chrání vaše citlivé informace.
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a>Vstupní využití
Tato část uvádí, jak toouse Mobile Apps vstupní vazby v kódu funkce. 

Pokud záznam hello se hello zadána tabulka a záznam ID nenajde, je předána do hello s názvem [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parametr (nebo v Node.js, je předaná do hello `context.bindings.<name>` objekt). Když se nenašel záznam hello, parametr hello je `null`. 

Funkcí jazyka C# a F # je všechny změny toohello vstupní záznamu (vstupní parametr) automaticky odeslán zpět toohello Mobile Apps tabulce, při ukončení hello funkce úspěšně. Ve funkcích Node.js, použijte `context.bindings.<name>` tooaccess hello vstupního záznamu. Nelze upravit záznam v Node.js.

<a name="inputsample"></a>

## <a name="input-sample"></a>Vstupní ukázka
Předpokládejme, že máte hello následující function.json, který načte záznam tabulky mobilní aplikace s id hello hello fronty aktivační událost zprávy:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
],
"disabled": false
}
```

V tématu vzorku hello konkrétní jazyk, který používá hello vstupního záznamu z vazby hello. Ukázky jazyka C# a F # Hello také upravit záznam hello `text` vlastnost.

* [C#](#inputcsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Vstupní ukázka v jazyce C# #

```cs
#r "Newtonsoft.Json"    
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, JObject record)
{
    if (record != null)
    {
        record["Text"] = "This has changed.";
    }    
}
```

<!--
<a name="inputfsharp"></a>
### Input sample in F# ## 

```fsharp
#r "Newtonsoft.Json"    
open Newtonsoft.Json.Linq
let Run(myQueueItem: string, record: JObject) =
  inputDocument?text <- "This has changed."
```
-->

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Vstupní ukázka v Node.js

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a>Mobile Apps výstup vazby
Použijte hello Mobile Apps výstup vazby toowrite nový koncový bod záznamů tooa tabulky mobilní aplikace.  

Hello Mobile Apps výstup hello následující objekt JSON v hello používá funkci `bindings` pole function.json:

```json
{
    "name": "<Name of output parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "out"
}
```

Vezměte na vědomí následující hello:

* `connection`by mělo obsahovat název hello nastavení aplikace v aplikaci funkce, která obsahuje adresu URL hello mobilní aplikace. Funkce Hello používá tuto adresu URL tooconstruct hello požadované operace REST proti mobilní aplikaci. Můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující adresu URL mobilních aplikací (který vypadá podobně jako `http://<appname>.azurewebsites.net`), potom zadejte název nastavení aplikace hello hello do hello `connection` vlastnost Vstupní vazba. 
* Je třeba toospecify `apiKey` Pokud jste [implementovat klíč rozhraní API vašeho back-end mobilní aplikace Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), nebo [implementovat klíč rozhraní API vašeho back-end mobilní aplikace .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). toodo, můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující klíč hello rozhraní API, pak přidejte hello `apiKey` vlastnost vaše Vstupní vazba s hello název nastavení aplikace hello. 
  
  > [!IMPORTANT]
  > Tento klíč rozhraní API nesmí sdílet s klienty mobilní aplikace. By mělo být distribuované bezpečně tooservice straně klienty, jako jsou Azure Functions. 
  > 
  > [!NOTE]
  > Azure Functions ukládá informace o připojení a klíče rozhraní API jako nastavení aplikace, tak, aby se změnami do vašeho úložiště správy zdrojového kódu. To chrání vaše citlivé informace.
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a>Využití výstupní
Tato část uvádí, jak toouse Mobile Apps výstup vazby v kódu funkce. 

V C# funkce, použijte parametr s názvem výstup typu `out object` tooaccess hello výstup záznamu. Ve funkcích Node.js, použijte `context.bindings.<name>` tooaccess hello výstup záznamu.

<a name="outputsample"></a>

## <a name="output-sample"></a>Ukázkový výstup
Předpokládejme, že máte následující function.json, který definuje aktivační procedury fronty a Mobile Apps výstup hello:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
],
"disabled": false
}
```

V tématu vzorku hello konkrétní jazyk, který vytvoří záznam v koncový bod hello Mobile Apps tabulek s obsahem hello uvítací zprávu fronty.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ukázka výstupu v jazyce C# #

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

<!--
<a name="outfsharp"></a>
### Output sample in F# ## 
```fsharp

```
-->
<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Ukázka výstupu v Node.js

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

