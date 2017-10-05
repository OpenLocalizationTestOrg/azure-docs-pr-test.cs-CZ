---
title: "Azure aktivaci časovačem funkce | Microsoft Docs"
description: "Pochopit, jak použít aktivační události časovače v Azure Functions."
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
ms.openlocfilehash: 6a97ab8508f889b77d064a5da70e3c726d62900c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-timer-trigger"></a><span data-ttu-id="91cf0-104">Azure aktivaci časovačem funkce</span><span class="sxs-lookup"><span data-stu-id="91cf0-104">Azure Functions timer trigger</span></span>

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="91cf0-105">Tento článek vysvětluje postup konfigurace a aktivační události časovače kódu v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="91cf0-105">This article explains how to configure and code timer triggers in Azure Functions.</span></span> <span data-ttu-id="91cf0-106">Azure Functions nabízí vazbu aktivační událost časovače, který vám umožní spustit funkce kódu podle definovaného plánu.</span><span class="sxs-lookup"><span data-stu-id="91cf0-106">Azure Functions has a timer trigger binding that lets you run your function code based on a defined schedule.</span></span> 

<span data-ttu-id="91cf0-107">Aktivační událost časovače podporuje víc instancí Škálováním na více systémů. Ve všech instancích je spuštěna jedna instance funkce konkrétní časovače.</span><span class="sxs-lookup"><span data-stu-id="91cf0-107">The timer trigger supports multi-instance scale-out. A single instance of a particular timer function is run across all instances.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a><span data-ttu-id="91cf0-108">Trigger časovače</span><span class="sxs-lookup"><span data-stu-id="91cf0-108">Timer trigger</span></span>
<span data-ttu-id="91cf0-109">Aktivační událost časovače funkce používá následující objekt JSON v `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="91cf0-109">The timer trigger to a function uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

<span data-ttu-id="91cf0-110">Hodnota `schedule` je [výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) obsahující tyto šest polí:</span><span class="sxs-lookup"><span data-stu-id="91cf0-110">The value of `schedule` is a [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression) that includes these six fields:</span></span> 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
><span data-ttu-id="91cf0-111">Řada výrazů cron zjistíte online vynechejte `{second}` pole.</span><span class="sxs-lookup"><span data-stu-id="91cf0-111">Many of the cron expressions you find online omit the `{second}` field.</span></span> <span data-ttu-id="91cf0-112">Pokud zkopírujete z jednoho z nich, bude nutné upravit pro nadbytečné `{second}` pole.</span><span class="sxs-lookup"><span data-stu-id="91cf0-112">If you copy from one of them, you need to adjust for the extra `{second}` field.</span></span> <span data-ttu-id="91cf0-113">Konkrétní příklady najdete v tématu [naplánovat příklady](#examples) níže.</span><span class="sxs-lookup"><span data-stu-id="91cf0-113">For specific examples, see [Schedule examples](#examples) below.</span></span>

<span data-ttu-id="91cf0-114">Použít s výrazy CRON výchozí časové pásmo je koordinovaný světový čas (UTC).</span><span class="sxs-lookup"><span data-stu-id="91cf0-114">The default time zone used with the CRON expressions is Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="91cf0-115">Pokud chcete, aby vaše výraz CRON založený na jiném časovém pásmu, vytvořte nové nastavení aplikace pro funkce aplikace s názvem `WEBSITE_TIME_ZONE`.</span><span class="sxs-lookup"><span data-stu-id="91cf0-115">To have your CRON expression based on another time zone, create a new app setting for your function app named `WEBSITE_TIME_ZONE`.</span></span> <span data-ttu-id="91cf0-116">Nastavte hodnotu na název požadované časové pásmo, jak je znázorněno [Microsoft časové pásmo Index](https://msdn.microsoft.com/library/ms912391.aspx).</span><span class="sxs-lookup"><span data-stu-id="91cf0-116">Set the value to the name of the desired time zone as shown in the [Microsoft Time Zone Index](https://msdn.microsoft.com/library/ms912391.aspx).</span></span> 

<span data-ttu-id="91cf0-117">Například *východní běžný čas* je UTC-05:00.</span><span class="sxs-lookup"><span data-stu-id="91cf0-117">For example, *Eastern Standard Time* is UTC-05:00.</span></span> <span data-ttu-id="91cf0-118">Má vaše časovače aktivovat ještě efektivněji na 10:00 AM EST každý den, použijte následující výraz CRON účty pro časové pásmo UTC:</span><span class="sxs-lookup"><span data-stu-id="91cf0-118">To have your timer trigger fire at 10:00 AM EST every day, use the following CRON expression that accounts for UTC time zone:</span></span>

```json
"schedule": "0 0 15 * * *",
``` 

<span data-ttu-id="91cf0-119">Alternativně můžete přidat nové nastavení aplikace pro funkce aplikace s názvem `WEBSITE_TIME_ZONE` a nastavte hodnotu na **východní běžný čas**.</span><span class="sxs-lookup"><span data-stu-id="91cf0-119">Alternatively, you could add a new app setting for your function app named `WEBSITE_TIME_ZONE` and set the value to **Eastern Standard Time**.</span></span>  <span data-ttu-id="91cf0-120">Následující výraz CRON pak může použít pro 10:00 AM EST:</span><span class="sxs-lookup"><span data-stu-id="91cf0-120">Then the following CRON expression could be used for 10:00 AM EST:</span></span> 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a><span data-ttu-id="91cf0-121">Příklady plán</span><span class="sxs-lookup"><span data-stu-id="91cf0-121">Schedule examples</span></span>
<span data-ttu-id="91cf0-122">Tady jsou některé ukázky můžete použít pro výrazy CRON `schedule` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="91cf0-122">Here are some samples of CRON expressions you can use for the `schedule` property.</span></span> 

<span data-ttu-id="91cf0-123">K aktivaci každých pět minut:</span><span class="sxs-lookup"><span data-stu-id="91cf0-123">To trigger once every five minutes:</span></span>

```json
"schedule": "0 */5 * * * *"
```

<span data-ttu-id="91cf0-124">K aktivaci jednou v horní části každou hodinu:</span><span class="sxs-lookup"><span data-stu-id="91cf0-124">To trigger once at the top of every hour:</span></span>

```json
"schedule": "0 0 * * * *",
```

<span data-ttu-id="91cf0-125">K aktivaci každé dvě hodiny:</span><span class="sxs-lookup"><span data-stu-id="91cf0-125">To trigger once every two hours:</span></span>

```json
"schedule": "0 0 */2 * * *",
```

<span data-ttu-id="91cf0-126">K aktivaci jednou za hodinu z 9: 00 do 17: 00:</span><span class="sxs-lookup"><span data-stu-id="91cf0-126">To trigger once every hour from 9 AM to 5 PM:</span></span>

```json
"schedule": "0 0 9-17 * * *",
```

<span data-ttu-id="91cf0-127">Spouštět v 9:30:00 každý den:</span><span class="sxs-lookup"><span data-stu-id="91cf0-127">To trigger At 9:30 AM every day:</span></span>

```json
"schedule": "0 30 9 * * *",
```

<span data-ttu-id="91cf0-128">Spouštět v 9:30:00 každý den v týdnu:</span><span class="sxs-lookup"><span data-stu-id="91cf0-128">To trigger At 9:30 AM every weekday:</span></span>

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="91cf0-129">Aktivační události využití</span><span class="sxs-lookup"><span data-stu-id="91cf0-129">Trigger usage</span></span>
<span data-ttu-id="91cf0-130">Po vyvolání funkce aktivační událost časovače [časovače objekt](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) je předána do funkce.</span><span class="sxs-lookup"><span data-stu-id="91cf0-130">When a timer trigger function is invoked, the [timer object](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) is passed into the function.</span></span> <span data-ttu-id="91cf0-131">Následujícím kódu JSON je příklad reprezentací objekt časovače.</span><span class="sxs-lookup"><span data-stu-id="91cf0-131">The following JSON is an example representation of the timer object.</span></span> 

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

## <a name="trigger-sample"></a><span data-ttu-id="91cf0-132">Ukázka aktivační události</span><span class="sxs-lookup"><span data-stu-id="91cf0-132">Trigger sample</span></span>
<span data-ttu-id="91cf0-133">Předpokládejme, že máte následující aktivační událost časovače `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="91cf0-133">Suppose you have the following timer trigger in the `bindings` array of function.json:</span></span>

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

<span data-ttu-id="91cf0-134">Naleznete v ukázce pro specifický jazyk, který čte objekt časovače pro zjištění, zda je spuštěna pozdní.</span><span class="sxs-lookup"><span data-stu-id="91cf0-134">See the language-specific sample that reads the timer object to see whether it's running late.</span></span>

* [<span data-ttu-id="91cf0-135">C#</span><span class="sxs-lookup"><span data-stu-id="91cf0-135">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="91cf0-136">F#</span><span class="sxs-lookup"><span data-stu-id="91cf0-136">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="91cf0-137">Node.js</span><span class="sxs-lookup"><span data-stu-id="91cf0-137">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="91cf0-138">Ukázka aktivační události v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="91cf0-138">Trigger sample in C#</span></span> #
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

### <a name="trigger-sample-in-f"></a><span data-ttu-id="91cf0-139">Ukázka aktivační události v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="91cf0-139">Trigger sample in F#</span></span> #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="91cf0-140">Ukázka aktivační události v Node.js</span><span class="sxs-lookup"><span data-stu-id="91cf0-140">Trigger sample in Node.js</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="91cf0-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="91cf0-141">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

