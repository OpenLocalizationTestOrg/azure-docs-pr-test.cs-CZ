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
# <a name="send-sms-messages-from-azure-functions-using-hello-twilio-output-binding"></a>Odesílání zpráv SMS z Azure Functions pomocí hello Twilio výstup vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak Twilio vazby tooconfigure a použít s Azure Functions. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Azure Functions podporuje Twilio výstup vazby tooenable funkce toosend SMS text zprávy zadání několika řádků kódu a [Twilio](https://www.twilio.com/) účtu. 

## <a name="functionjson-for-hello-twilio-output-binding"></a>Function.JSON pro hello Twilio výstup vazby
soubor function.json Hello poskytuje hello následující vlastnosti:

* `name`: Název proměnné používá v kódu funkce pro hello textovou zprávu Twilio SMS.
* `type`: musí být nastaven příliš*"twilioSms"*.
* `accountSid`: Tato hodnota musí být nastavena toohello název nastavení aplikace, která obsahuje identifikátor Sid účtu Twilio.
* `authToken`: Tato hodnota musí být nastavena toohello název nastavení aplikace, která obsahuje vaše Twilio ověřovací token.
* `to`: Tato hodnota se nastavuje toohello telefonní číslo, který je odeslán textová zpráva hello.
* `from`: Tato hodnota se nastavuje toohello telefonní číslo, které je odeslaný hello textová zpráva.
* `direction`: musí být nastaven příliš*"se na"*.
* `body`: Tato hodnota může být použité toohard kód hello SMS zpráva, pokud nepotřebujete tooset dynamicky v hello kódu pro funkce. 

Příklad function.json:

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


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Příklad C# fronty aktivační událost s Twilio výstup vazby
#### <a name="synchronous"></a>Synchronní
Tato synchronní ukázkový kód pro aktivační procedury fronty Azure Storage používá k výstupnímu parametru toosend odběratele tooa zpráva text, který zadal pořadí.

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

#### <a name="asynchronous"></a>Asynchronní
Tento asynchronní ukázkový kód pro aktivační procedury fronty Azure Storage odešle odběratele tooa zpráva text, který zadal pořadí.

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

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Příklad Node.js fronty aktivační událost s Twilio výstup vazby
Tento příklad Node.js odešle odběratele tooa zpráva text, který zadal pořadí.

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

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

