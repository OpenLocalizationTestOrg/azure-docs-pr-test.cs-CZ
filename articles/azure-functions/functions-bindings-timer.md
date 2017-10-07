---
title: "aktivační událost časovače funkce aaaAzure | Microsoft Docs"
description: "Pochopte, jak se spustí časovač toouse v Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: 17fca22372dbc55d4684c8c099cc97923a7d3cf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-timer-trigger"></a>Azure aktivaci časovačem funkce

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak se spustí časovač tooconfigure a kódu v Azure Functions. Azure Functions nabízí vazbu aktivační událost časovače, který vám umožní spustit funkce kódu podle definovaného plánu. 

aktivační událost časovače Hello podporuje víc instancí Škálováním na více systémů. Ve všech instancích je spuštěna jedna instance funkce konkrétní časovače.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a>Trigger časovače
Hello funkce tooa aktivační událost časovače používá následující objekt JSON v hello hello `bindings` pole function.json:

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

Hello hodnotu `schedule` je [výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) obsahující tyto šest polí: 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
>Řada výrazů cron hello zjistíte online vynechejte hello `{second}` pole. Pokud zkopírujete z jednoho z nich, potřebujete tooadjust hello navíc `{second}` pole. Konkrétní příklady najdete v tématu [naplánovat příklady](#examples) níže.

použít s výrazy CRON hello Hello výchozí časové pásmo je koordinovaný světový čas (UTC). toohave výraz CRON založené na jiném časovém pásmu, vytvořit nové nastavení aplikace pro funkce aplikace s názvem `WEBSITE_TIME_ZONE`. Sada hello hodnota toohello název hello potřeby časové pásmo, jak je znázorněno v hello [Microsoft časové pásmo Index](https://msdn.microsoft.com/library/ms912391.aspx). 

Například *východní běžný čas* je UTC-05:00. toohave vaše časovače aktivovat ještě efektivněji na 10:00 AM EST každý den, hello použijte následující výraz CRON, který účty pro časové pásmo UTC:

```json
"schedule": "0 0 15 * * *",
``` 

Alternativně můžete přidat nové nastavení aplikace pro funkce aplikace s názvem `WEBSITE_TIME_ZONE` a nastavte hodnotu hello příliš**východní běžný čas**.  Následující výraz CRON hello pak by mohly být použity 10:00 AM EST: 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a>Příklady plán
Tady jsou některé ukázky CRON výrazy, které můžete použít pro hello `schedule` vlastnost. 

tootrigger každých pět minut:

```json
"schedule": "0 */5 * * * *"
```

tootrigger jednou na začátku hello každou hodinu:

```json
"schedule": "0 0 * * * *",
```

tootrigger každé dvě hodiny:

```json
"schedule": "0 0 */2 * * *",
```

tootrigger jednou za hodinu z too5 9: 00 PM:

```json
"schedule": "0 0 9-17 * * *",
```

tootrigger v 9:30:00 každý den:

```json
"schedule": "0 30 9 * * *",
```

tootrigger v 9:30:00 každý den v týdnu:

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a>Aktivační události využití
Po vyvolání funkce aktivační událost časovače hello [časovače objekt](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) je předána do funkce hello. Hello následujícím kódu JSON je reprezentaci příklad hello časovače objektu. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00.012699+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

<a name="sample"></a>

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte následující aktivační událost časovače v hello hello `bindings` pole function.json:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

V tématu vzorku hello konkrétní jazyk, který čte hello časovače objekt toosee, ať už běží pozdní.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Ukázka aktivační události v jazyce C# #
```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Ukázka aktivační události v jazyce F # #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Ukázka aktivační události v Node.js
```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);   

    context.done();
};
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

