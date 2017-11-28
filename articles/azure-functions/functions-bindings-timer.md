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
# <a name="azure-functions-timer-trigger"></a><span data-ttu-id="acc39-104">Azure aktivaci časovačem funkce</span><span class="sxs-lookup"><span data-stu-id="acc39-104">Azure Functions timer trigger</span></span>

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="acc39-105">Tento článek vysvětluje, jak se spustí časovač tooconfigure a kódu v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="acc39-105">This article explains how tooconfigure and code timer triggers in Azure Functions.</span></span> <span data-ttu-id="acc39-106">Azure Functions nabízí vazbu aktivační událost časovače, který vám umožní spustit funkce kódu podle definovaného plánu.</span><span class="sxs-lookup"><span data-stu-id="acc39-106">Azure Functions has a timer trigger binding that lets you run your function code based on a defined schedule.</span></span> 

<span data-ttu-id="acc39-107">aktivační událost časovače Hello podporuje víc instancí Škálováním na více systémů. Ve všech instancích je spuštěna jedna instance funkce konkrétní časovače.</span><span class="sxs-lookup"><span data-stu-id="acc39-107">hello timer trigger supports multi-instance scale-out. A single instance of a particular timer function is run across all instances.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a><span data-ttu-id="acc39-108">Trigger časovače</span><span class="sxs-lookup"><span data-stu-id="acc39-108">Timer trigger</span></span>
<span data-ttu-id="acc39-109">Hello funkce tooa aktivační událost časovače používá následující objekt JSON v hello hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="acc39-109">hello timer trigger tooa function uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

<span data-ttu-id="acc39-110">Hello hodnotu `schedule` je [výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) obsahující tyto šest polí:</span><span class="sxs-lookup"><span data-stu-id="acc39-110">hello value of `schedule` is a [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression) that includes these six fields:</span></span> 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
><span data-ttu-id="acc39-111">Řada výrazů cron hello zjistíte online vynechejte hello `{second}` pole.</span><span class="sxs-lookup"><span data-stu-id="acc39-111">Many of hello cron expressions you find online omit hello `{second}` field.</span></span> <span data-ttu-id="acc39-112">Pokud zkopírujete z jednoho z nich, potřebujete tooadjust hello navíc `{second}` pole.</span><span class="sxs-lookup"><span data-stu-id="acc39-112">If you copy from one of them, you need tooadjust for hello extra `{second}` field.</span></span> <span data-ttu-id="acc39-113">Konkrétní příklady najdete v tématu [naplánovat příklady](#examples) níže.</span><span class="sxs-lookup"><span data-stu-id="acc39-113">For specific examples, see [Schedule examples](#examples) below.</span></span>

<span data-ttu-id="acc39-114">použít s výrazy CRON hello Hello výchozí časové pásmo je koordinovaný světový čas (UTC).</span><span class="sxs-lookup"><span data-stu-id="acc39-114">hello default time zone used with hello CRON expressions is Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="acc39-115">toohave výraz CRON založené na jiném časovém pásmu, vytvořit nové nastavení aplikace pro funkce aplikace s názvem `WEBSITE_TIME_ZONE`.</span><span class="sxs-lookup"><span data-stu-id="acc39-115">toohave your CRON expression based on another time zone, create a new app setting for your function app named `WEBSITE_TIME_ZONE`.</span></span> <span data-ttu-id="acc39-116">Sada hello hodnota toohello název hello potřeby časové pásmo, jak je znázorněno v hello [Microsoft časové pásmo Index](https://msdn.microsoft.com/library/ms912391.aspx).</span><span class="sxs-lookup"><span data-stu-id="acc39-116">Set hello value toohello name of hello desired time zone as shown in hello [Microsoft Time Zone Index](https://msdn.microsoft.com/library/ms912391.aspx).</span></span> 

<span data-ttu-id="acc39-117">Například *východní běžný čas* je UTC-05:00.</span><span class="sxs-lookup"><span data-stu-id="acc39-117">For example, *Eastern Standard Time* is UTC-05:00.</span></span> <span data-ttu-id="acc39-118">toohave vaše časovače aktivovat ještě efektivněji na 10:00 AM EST každý den, hello použijte následující výraz CRON, který účty pro časové pásmo UTC:</span><span class="sxs-lookup"><span data-stu-id="acc39-118">toohave your timer trigger fire at 10:00 AM EST every day, use hello following CRON expression that accounts for UTC time zone:</span></span>

```json
"schedule": "0 0 15 * * *",
``` 

<span data-ttu-id="acc39-119">Alternativně můžete přidat nové nastavení aplikace pro funkce aplikace s názvem `WEBSITE_TIME_ZONE` a nastavte hodnotu hello příliš**východní běžný čas**.</span><span class="sxs-lookup"><span data-stu-id="acc39-119">Alternatively, you could add a new app setting for your function app named `WEBSITE_TIME_ZONE` and set hello value too**Eastern Standard Time**.</span></span>  <span data-ttu-id="acc39-120">Následující výraz CRON hello pak by mohly být použity 10:00 AM EST:</span><span class="sxs-lookup"><span data-stu-id="acc39-120">Then hello following CRON expression could be used for 10:00 AM EST:</span></span> 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a><span data-ttu-id="acc39-121">Příklady plán</span><span class="sxs-lookup"><span data-stu-id="acc39-121">Schedule examples</span></span>
<span data-ttu-id="acc39-122">Tady jsou některé ukázky CRON výrazy, které můžete použít pro hello `schedule` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="acc39-122">Here are some samples of CRON expressions you can use for hello `schedule` property.</span></span> 

<span data-ttu-id="acc39-123">tootrigger každých pět minut:</span><span class="sxs-lookup"><span data-stu-id="acc39-123">tootrigger once every five minutes:</span></span>

```json
"schedule": "0 */5 * * * *"
```

<span data-ttu-id="acc39-124">tootrigger jednou na začátku hello každou hodinu:</span><span class="sxs-lookup"><span data-stu-id="acc39-124">tootrigger once at hello top of every hour:</span></span>

```json
"schedule": "0 0 * * * *",
```

<span data-ttu-id="acc39-125">tootrigger každé dvě hodiny:</span><span class="sxs-lookup"><span data-stu-id="acc39-125">tootrigger once every two hours:</span></span>

```json
"schedule": "0 0 */2 * * *",
```

<span data-ttu-id="acc39-126">tootrigger jednou za hodinu z too5 9: 00 PM:</span><span class="sxs-lookup"><span data-stu-id="acc39-126">tootrigger once every hour from 9 AM too5 PM:</span></span>

```json
"schedule": "0 0 9-17 * * *",
```

<span data-ttu-id="acc39-127">tootrigger v 9:30:00 každý den:</span><span class="sxs-lookup"><span data-stu-id="acc39-127">tootrigger At 9:30 AM every day:</span></span>

```json
"schedule": "0 30 9 * * *",
```

<span data-ttu-id="acc39-128">tootrigger v 9:30:00 každý den v týdnu:</span><span class="sxs-lookup"><span data-stu-id="acc39-128">tootrigger At 9:30 AM every weekday:</span></span>

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="acc39-129">Aktivační události využití</span><span class="sxs-lookup"><span data-stu-id="acc39-129">Trigger usage</span></span>
<span data-ttu-id="acc39-130">Po vyvolání funkce aktivační událost časovače hello [časovače objekt](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) je předána do funkce hello.</span><span class="sxs-lookup"><span data-stu-id="acc39-130">When a timer trigger function is invoked, hello [timer object](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) is passed into hello function.</span></span> <span data-ttu-id="acc39-131">Hello následujícím kódu JSON je reprezentaci příklad hello časovače objektu.</span><span class="sxs-lookup"><span data-stu-id="acc39-131">hello following JSON is an example representation of hello timer object.</span></span> 

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

## <a name="trigger-sample"></a><span data-ttu-id="acc39-132">Ukázka aktivační události</span><span class="sxs-lookup"><span data-stu-id="acc39-132">Trigger sample</span></span>
<span data-ttu-id="acc39-133">Předpokládejme, že máte následující aktivační událost časovače v hello hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="acc39-133">Suppose you have hello following timer trigger in hello `bindings` array of function.json:</span></span>

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

<span data-ttu-id="acc39-134">V tématu vzorku hello konkrétní jazyk, který čte hello časovače objekt toosee, ať už běží pozdní.</span><span class="sxs-lookup"><span data-stu-id="acc39-134">See hello language-specific sample that reads hello timer object toosee whether it's running late.</span></span>

* [<span data-ttu-id="acc39-135">C#</span><span class="sxs-lookup"><span data-stu-id="acc39-135">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="acc39-136">F#</span><span class="sxs-lookup"><span data-stu-id="acc39-136">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="acc39-137">Node.js</span><span class="sxs-lookup"><span data-stu-id="acc39-137">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="acc39-138">Ukázka aktivační události v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="acc39-138">Trigger sample in C#</span></span> #
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

### <a name="trigger-sample-in-f"></a><span data-ttu-id="acc39-139">Ukázka aktivační události v jazyce F #</span><span class="sxs-lookup"><span data-stu-id="acc39-139">Trigger sample in F#</span></span> #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="acc39-140">Ukázka aktivační události v Node.js</span><span class="sxs-lookup"><span data-stu-id="acc39-140">Trigger sample in Node.js</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="acc39-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="acc39-141">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

