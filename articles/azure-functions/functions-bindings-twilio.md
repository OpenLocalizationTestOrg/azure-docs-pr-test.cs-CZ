---
title: Azure vazba funkce Twilio | Microsoft Docs
description: "Pochopit, jak pomocí Azure Functions Twilio vazby."
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
ms.openlocfilehash: 870e47ec7f8ce41ee4acadc7b8ed59298958acbe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="send-sms-messages-from-azure-functions-using-the-twilio-output-binding"></a><span data-ttu-id="e16e6-104">Odeslat SMS zprávy z Azure Functions pomocí Twilio výstup vazby</span><span class="sxs-lookup"><span data-stu-id="e16e6-104">Send SMS messages from Azure Functions using the Twilio output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="e16e6-105">Tento článek vysvětluje, jak konfigurovat a používat Twilio vazby s Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="e16e6-105">This article explains how to configure and use Twilio bindings with Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="e16e6-106">Azure Functions podporuje Twilio výstup vazby k povolení funkcí k posílání textových zpráv SMS zadání několika řádků kódu a [Twilio](https://www.twilio.com/) účtu.</span><span class="sxs-lookup"><span data-stu-id="e16e6-106">Azure Functions supports Twilio output bindings to enable your functions to send SMS text messages with a few lines of code and a [Twilio](https://www.twilio.com/) account.</span></span> 

## <a name="functionjson-for-the-twilio-output-binding"></a><span data-ttu-id="e16e6-107">Function.JSON pro Twilio výstup vazby</span><span class="sxs-lookup"><span data-stu-id="e16e6-107">function.json for the Twilio output binding</span></span>
<span data-ttu-id="e16e6-108">Soubor function.json poskytuje následujících vlastností:</span><span class="sxs-lookup"><span data-stu-id="e16e6-108">The function.json file provides the following properties:</span></span>

* <span data-ttu-id="e16e6-109">`name`: Název proměnné používá v kódu funkce pro textovou zprávu Twilio SMS.</span><span class="sxs-lookup"><span data-stu-id="e16e6-109">`name` : Variable name used in function code for the Twilio SMS text message.</span></span>
* <span data-ttu-id="e16e6-110">`type`: musí být nastavena na *"twilioSms"*.</span><span class="sxs-lookup"><span data-stu-id="e16e6-110">`type` : must be set to *"twilioSms"*.</span></span>
* <span data-ttu-id="e16e6-111">`accountSid`: Tato hodnota musí být nastavena na název nastavení aplikace, která obsahuje identifikátor Sid účtu Twilio.</span><span class="sxs-lookup"><span data-stu-id="e16e6-111">`accountSid` : This value must be set to the name of an App Setting that holds your Twilio Account Sid.</span></span>
* <span data-ttu-id="e16e6-112">`authToken`: Tato hodnota musí být nastavte na název nastavení aplikace, která obsahuje vaše Twilio ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="e16e6-112">`authToken` : This value must be set to the name of an App Setting that holds your Twilio authentication token.</span></span>
* <span data-ttu-id="e16e6-113">`to`: Tato hodnota nastavena na telefonní číslo, který je odeslán textová zpráva.</span><span class="sxs-lookup"><span data-stu-id="e16e6-113">`to` : This value is set to the phone number that the SMS text is sent to.</span></span>
* <span data-ttu-id="e16e6-114">`from`: Tato hodnota nastavena na telefonní číslo, který je odeslán textová zpráva z.</span><span class="sxs-lookup"><span data-stu-id="e16e6-114">`from` : This value is set to the phone number that the SMS text is sent from.</span></span>
* <span data-ttu-id="e16e6-115">`direction`: musí být nastavena na *"se na"*.</span><span class="sxs-lookup"><span data-stu-id="e16e6-115">`direction` : must be set to *"out"*.</span></span>
* <span data-ttu-id="e16e6-116">`body`: Tato hodnota slouží k pevného code textovou zprávu SMS, pokud nepotřebujete dynamické nastavení v kódu pro funkce.</span><span class="sxs-lookup"><span data-stu-id="e16e6-116">`body` : This value can be used to hard code the SMS text message if you don't need to set it dynamically in the code for your function.</span></span> 

<span data-ttu-id="e16e6-117">Příklad function.json:</span><span class="sxs-lookup"><span data-stu-id="e16e6-117">Example function.json:</span></span>

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


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="e16e6-118">Příklad C# fronty aktivační událost s Twilio výstup vazby</span><span class="sxs-lookup"><span data-stu-id="e16e6-118">Example C# queue trigger with Twilio output binding</span></span>
#### <a name="synchronous"></a><span data-ttu-id="e16e6-119">Synchronní</span><span class="sxs-lookup"><span data-stu-id="e16e6-119">Synchronous</span></span>
<span data-ttu-id="e16e6-120">Tato synchronní ukázkový kód pro aktivační procedury fronty Azure Storage používá k odesílání textovou zprávu zákazníkovi, který objednávku parametr typu out.</span><span class="sxs-lookup"><span data-stu-id="e16e6-120">This synchronous example code for an Azure Storage queue trigger uses an out parameter to send a text message to a customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

#### <a name="asynchronous"></a><span data-ttu-id="e16e6-121">Asynchronní</span><span class="sxs-lookup"><span data-stu-id="e16e6-121">Asynchronous</span></span>
<span data-ttu-id="e16e6-122">Tento asynchronní ukázkový kód pro aktivační procedury fronty Azure Storage odešle textovou zprávu zákazníkovi, který objednávku.</span><span class="sxs-lookup"><span data-stu-id="e16e6-122">This asynchronous example code for an Azure Storage queue trigger sends a text message to a customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="e16e6-123">Příklad Node.js fronty aktivační událost s Twilio výstup vazby</span><span class="sxs-lookup"><span data-stu-id="e16e6-123">Example Node.js queue trigger with Twilio output binding</span></span>
<span data-ttu-id="e16e6-124">Tento příklad Node.js odešle textovou zprávu zákazníkovi, který objednávku.</span><span class="sxs-lookup"><span data-stu-id="e16e6-124">This Node.js example sends a text message to a customer who placed an order.</span></span>

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="e16e6-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e16e6-125">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

