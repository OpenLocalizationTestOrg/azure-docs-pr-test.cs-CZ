---
title: Vazba funkce Twilio aaaAzure | Microsoft Docs
description: Pochopit, jak Twilio vazby toouse s Azure Functions.
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: a60263aa-3de9-4e1b-a2bb-0b52e70d559b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/20/2016
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 882853947850e7d6795ca5b2f3fb6b9a83ede182
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-sms-messages-from-azure-functions-using-hello-twilio-output-binding"></a><span data-ttu-id="cb63c-104">Odesílání zpráv SMS z Azure Functions pomocí hello Twilio výstup vazby</span><span class="sxs-lookup"><span data-stu-id="cb63c-104">Send SMS messages from Azure Functions using hello Twilio output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="cb63c-105">Tento článek vysvětluje, jak Twilio vazby tooconfigure a použít s Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="cb63c-105">This article explains how tooconfigure and use Twilio bindings with Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="cb63c-106">Azure Functions podporuje Twilio výstup vazby tooenable funkce toosend SMS text zprávy zadání několika řádků kódu a [Twilio](https://www.twilio.com/) účtu.</span><span class="sxs-lookup"><span data-stu-id="cb63c-106">Azure Functions supports Twilio output bindings tooenable your functions toosend SMS text messages with a few lines of code and a [Twilio](https://www.twilio.com/) account.</span></span> 

## <a name="functionjson-for-hello-twilio-output-binding"></a><span data-ttu-id="cb63c-107">Function.JSON pro hello Twilio výstup vazby</span><span class="sxs-lookup"><span data-stu-id="cb63c-107">function.json for hello Twilio output binding</span></span>
<span data-ttu-id="cb63c-108">soubor function.json Hello poskytuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="cb63c-108">hello function.json file provides hello following properties:</span></span>

* <span data-ttu-id="cb63c-109">`name`: Název proměnné používá v kódu funkce pro hello textovou zprávu Twilio SMS.</span><span class="sxs-lookup"><span data-stu-id="cb63c-109">`name` : Variable name used in function code for hello Twilio SMS text message.</span></span>
* <span data-ttu-id="cb63c-110">`type`: musí být nastaven příliš*"twilioSms"*.</span><span class="sxs-lookup"><span data-stu-id="cb63c-110">`type` : must be set too*"twilioSms"*.</span></span>
* <span data-ttu-id="cb63c-111">`accountSid`: Tato hodnota musí být nastavena toohello název nastavení aplikace, která obsahuje identifikátor Sid účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="cb63c-111">`accountSid` : This value must be set toohello name of an App Setting that holds your Twilio Account Sid.</span></span>
* <span data-ttu-id="cb63c-112">`authToken`: Tato hodnota musí být nastavena toohello název nastavení aplikace, která obsahuje vaše Twilio ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="cb63c-112">`authToken` : This value must be set toohello name of an App Setting that holds your Twilio authentication token.</span></span>
* <span data-ttu-id="cb63c-113">`to`: Tato hodnota se nastavuje toohello telefonní číslo, který je odeslán textová zpráva hello.</span><span class="sxs-lookup"><span data-stu-id="cb63c-113">`to` : This value is set toohello phone number that hello SMS text is sent to.</span></span>
* <span data-ttu-id="cb63c-114">`from`: Tato hodnota se nastavuje toohello telefonní číslo, které je odeslaný hello textová zpráva.</span><span class="sxs-lookup"><span data-stu-id="cb63c-114">`from` : This value is set toohello phone number that hello SMS text is sent from.</span></span>
* <span data-ttu-id="cb63c-115">`direction`: musí být nastaven příliš*"se na"*.</span><span class="sxs-lookup"><span data-stu-id="cb63c-115">`direction` : must be set too*"out"*.</span></span>
* <span data-ttu-id="cb63c-116">`body`: Tato hodnota může být použité toohard kód hello SMS zpráva, pokud nepotřebujete tooset dynamicky v hello kódu pro funkce.</span><span class="sxs-lookup"><span data-stu-id="cb63c-116">`body` : This value can be used toohard code hello SMS text message if you don't need tooset it dynamically in hello code for your function.</span></span> 

<span data-ttu-id="cb63c-117">Příklad function.json:</span><span class="sxs-lookup"><span data-stu-id="cb63c-117">Example function.json:</span></span>

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="cb63c-118">Příklad C# fronty aktivační událost s Twilio výstup vazby</span><span class="sxs-lookup"><span data-stu-id="cb63c-118">Example C# queue trigger with Twilio output binding</span></span>
#### <a name="synchronous"></a><span data-ttu-id="cb63c-119">Synchronní</span><span class="sxs-lookup"><span data-stu-id="cb63c-119">Synchronous</span></span>
<span data-ttu-id="cb63c-120">Tato synchronní ukázkový kód pro aktivační procedury fronty Azure Storage používá k výstupnímu parametru toosend odběratele tooa zpráva text, který zadal pořadí.</span><span class="sxs-lookup"><span data-stu-id="cb63c-120">This synchronous example code for an Azure Storage queue trigger uses an out parameter toosend a text message tooa customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a JSON string representing an order that contains hello name of a 
    // customer and a mobile number toosend text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want toouse a hard coded message and number in hello binding, you must at least 
    // initialize hello SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of hello body in hello output binding. In this example, we use 
    // hello order information toopersonalize a text message toohello mobile number provided for
    // order status updates.
    message.Body = msg;
    message.too= order.mobileNumber;
}
```

#### <a name="asynchronous"></a><span data-ttu-id="cb63c-121">Asynchronní</span><span class="sxs-lookup"><span data-stu-id="cb63c-121">Asynchronous</span></span>
<span data-ttu-id="cb63c-122">Tento asynchronní ukázkový kód pro aktivační procedury fronty Azure Storage odešle odběratele tooa zpráva text, který zadal pořadí.</span><span class="sxs-lookup"><span data-stu-id="cb63c-122">This asynchronous example code for an Azure Storage queue trigger sends a text message tooa customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a JSON string representing an order that contains hello name of a 
    // customer and a mobile number toosend text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want toouse a hard coded message and number in hello binding, you must at least 
    // initialize hello SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of hello body in hello output binding. In this example, we use 
    // hello order information toopersonalize a text message toohello mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.too= order.mobileNumber;

    await message.AddAsync(smsText);
}
```

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="cb63c-123">Příklad Node.js fronty aktivační událost s Twilio výstup vazby</span><span class="sxs-lookup"><span data-stu-id="cb63c-123">Example Node.js queue trigger with Twilio output binding</span></span>
<span data-ttu-id="cb63c-124">Tento příklad Node.js odešle odběratele tooa zpráva text, který zadal pořadí.</span><span class="sxs-lookup"><span data-stu-id="cb63c-124">This Node.js example sends a text message tooa customer who placed an order.</span></span>

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example hello queue item is a JSON string representing an order that contains hello name of a 
    // customer and a mobile number toosend text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want toouse a hard coded message and number in hello binding, you must at least 
    // initialize hello message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of hello body in hello output binding. In this example, we use 
    // hello order information toopersonalize a text message toohello mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        too: myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="cb63c-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb63c-125">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

