---
title: "Azure vazby centra oznámení funkce | Microsoft Docs"
description: "Pochopit, jak používat centra oznámení Azure vazby v Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: 0ff0c949-20bf-430b-8dd5-d72b7b6ee6f7
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/27/2016
ms.author: glenga
ms.openlocfilehash: fa3d37b963c1bb6b58127b9180cd657d7b1dabcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-notification-hub-output-binding"></a>Centra oznámení Azure funkce výstup vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje postup konfigurace a vazby centra oznámení Azure kódu v Azure Functions. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Funkce může odesílat nabízená oznámení pomocí nakonfigurované centra oznámení Azure se po zadání několika řádků kódu. Nicméně do centra oznámení Azure musí být nakonfigurovány pro služby platformy oznámení (PNS) chcete použít. Další informace o konfiguraci centra oznámení Azure a vývoj klientských aplikací, které zaregistrovat pro příjem oznámení najdete v tématu [Začínáme s Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) a klikněte na tlačítko svou cílovou platformu klienta v horní části.

Odesílat oznámení může být nativní oznámení nebo šablony oznámení. Nativní oznámení cílit na konkrétní oznámení platformu jako nakonfigurovaný v `platform` vlastnost vazby výstup. Šablona oznámení můžete použít pro více cílových platforem.   

## <a name="notification-hub-output-binding-properties"></a>Vlastnosti vazby výstupu centra oznámení
Soubor function.json poskytuje následujících vlastností:

* `name`: Název proměnné používá v kódu funkce pro centra oznámení.
* `type`: musí být nastavena na *"notificationHub"*.
* `tagExpression`: Výrazy značka umožňují určit doručit oznámení na skupiny zařízení, kteří zaregistrovali k odběru oznámení, které odpovídají výrazu značky.  Další informace najdete v tématu [výrazy směrování a značky](../notification-hubs/notification-hubs-tags-segment-push-message.md).
* `hubName`: Název prostředku centra oznámení na portálu Azure.
* `connection`: Tento připojovací řetězec musí být **nastavení aplikace** připojení nastavit připojovací řetězec *DefaultFullSharedAccessSignature* hodnotu pro vaše Centrum oznámení.
* `direction`: musí být nastavena na *"se na"*. 
* `platform`: Vlastnost platformy označuje platformou oznámení vaše cíle oznámení. Toto musí být jedna z následujících hodnot:
  * Ve výchozím nastavení pokud je vlastnost platformy vynechán z vazby výstup šablony oznámení lze cílit na libovolnou platformu nakonfigurované na do centra oznámení Azure. Další informace o použití šablony obecně pro různé platformy oznámení pomocí Azure Notification Hubs k odesílání najdete v tématu [šablony](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
  * `apns`: Apple Push Notification Service. Další informace o konfiguraci centra oznámení pro služby APN a příjem oznámení v aplikaci klienta najdete v tématu [odesílající nabízená oznámení iOS pomocí Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
  * `adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging). Další informace o konfiguraci centra oznámení pro ADM a příjem oznámení ve Kindle aplikaci najdete v tématu [Začínáme s Notification Hubs pro aplikace Kindle](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
  * `gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, což je nová verze služby GCM, je také podporována. Další informace o konfiguraci centra oznámení pro GCM/FCM a příjem oznámení v aplikaci pro Android klienta najdete v tématu [odesílající nabízená oznámení do systému Android pomocí Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
  * `wns`: [Službu nabízených oznámení Windows](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) cílení na platformy Windows. Windows Phone 8.1 a novější také podporuje WNS. Další informace o konfiguraci centra oznámení pro WNS a příjem oznámení v aplikaci pro univerzální platformu Windows (UWP) najdete v tématu [Začínáme s centra oznámení pro univerzální platformu aplikace Windows](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
  * `mpns`: [Microsoft služba nabízených oznámení](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Tato platforma podporuje Windows Phone 8 a starší operační systémy Windows Phone. Další informace o konfiguraci centra oznámení pro MPNS a příjem oznámení v aplikaci pro Windows Phone najdete v tématu [odesílající nabízená oznámení pomocí Azure Notification Hubs ve Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)

Příklad function.json:

```json
{
  "bindings": [
    {
      "name": "notification",
      "type": "notificationHub",
      "tagExpression": "",
      "hubName": "my-notification-hub",
      "connection": "MyHubConnectionString",
      "platform": "gcm",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="notification-hub-connection-string-setup"></a>Nastavení řetězce připojení centra oznámení
Pokud chcete použít výstupu centra oznámení vazby, musíte nakonfigurovat připojovací řetězec pro rozbočovač. To lze provést na *integrací* karta výběr vaše Centrum oznámení nebo vytvořit nový. 

Můžete také ručně přidat připojovací řetězec pro existující centrum přidáním připojovací řetězec *DefaultFullSharedAccessSignature* do vašeho centra oznámení. Tento připojovací řetězec poskytuje vaše oprávnění k funkci zasílání oznámení. *DefaultFullSharedAccessSignature* hodnotu připojovacího řetězce je přístupná z **klíče** tlačítko v hlavním okně prostředku centra oznámení na portálu Azure. Pokud chcete ručně přidat připojovací řetězec pro vaše centrum, použijte následující kroky: 

1. Na **aplikaci funkce** okno portálu Azure klikněte na tlačítko **nastavení aplikace funkce > přejděte na nastavení služby App Service**.
2. V **nastavení** okně klikněte na tlačítko **nastavení aplikace**.
3. Přejděte dolů k položce **nastavení aplikace** a přidejte položku s názvem pro *DefaultFullSharedAccessSignature* hodnotu pro vaše Centrum oznámení.
4. Referenční název řetězce pro nastavení vaší aplikace ve vazbách výstup. Podobně jako **MyHubConnectionString** použít v předchozím příkladu.

## <a name="apns-native-notifications-with-c-queue-triggers"></a>Nativní oznámení APNS s aktivační procedury fronty C#
Tento příklad ukazuje, jak používat typy definované v [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) odeslat nativní oznámení APNS. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native APNS notification is ...
    // { "aps": { "alert": "notification message" }}  

    log.Info($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="gcm-native-notifications-with-c-queue-triggers"></a>Nativní oznámení GCM s aktivační procedury fronty C#
Tento příklad ukazuje, jak používat typy definované v [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) odeslat nativní oznámení GCM. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native GCM notification is ...
    // { "data": { "message": "notification message" }}  

    log.Info($"Sending GCM notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{gcmNotificationPayload}");
    await notification.AddAsync(new GcmNotification(gcmNotificationPayload));        
}
```

## <a name="wns-native-notifications-with-c-queue-triggers"></a>Nativní oznámení WNS s aktivační procedury fronty C#
Tento příklad ukazuje, jak používat typy definované v [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) odeslat nativní oznámení WNS informační zprávy. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The XML format for a native WNS toast notification is ...
    // <?xml version="1.0" encoding="utf-8"?>
    // <toast>
    //      <visual>
    //     <binding template="ToastText01">
    //       <text id="1">notification message</text>
    //     </binding>
    //   </visual>
    // </toast>

    log.Info($"Sending WNS toast notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                    "<toast><visual><binding template=\"ToastText01\">" +
                                        "<text id=\"1\">" + 
                                            "A new user wants to be added (" + user.name + ")" + 
                                        "</text>" +
                                    "</binding></visual></toast>";

    log.Info($"{wnsNotificationPayload}");
    await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));        
}
```

## <a name="template-example-for-nodejs-timer-triggers"></a>Příklad šablony pro aktivační události časovače Node.js
Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `location` a `message`.

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);  
    context.bindings.notification = {
        location: "Redmond",
        message: "Hello from Node!"
    };
    context.done();
};
```

## <a name="template-example-for-f-timer-triggers"></a>Příklad šablony pro aktivační události časovače F #
Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `location` a `message`.

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a>Příklad šablony pomocí parametr typu out
Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `message` zástupný symbol v šabloně.

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = GetTemplateProperties(myQueueItem);
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return templateProperties;
}
```

## <a name="template-example-with-asynchronous-function"></a>Příklad šablony s asynchronní funkce
Pokud používáte asynchronní kódu, výstupní parametry nejsou povoleny. V takovém případě použijte `IAsyncCollector` vrátit šablony oznámení. Následující kód je příkladem asynchronní výše uvedený kód. 

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    log.Info($"Sending Template Notification to Notification Hub");
    await notification.AddAsync(GetTemplateProperties(myQueueItem));    
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["user"] = "A new user wants to be added : " + message;
    return templateProperties;
}
```

## <a name="template-example-using-json"></a>Příklad šablony pomocí JSON
Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `message` zástupný symbol v šabloně pomocí platný řetězec formátu JSON.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a>Příklad šablony pomocí Notification Hubs knihovny typů
Tento příklad ukazuje, jak používat typy definované v [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 

```cs
#r "Microsoft.Azure.NotificationHubs"

using System;
using System.Threading.Tasks;
using Microsoft.Azure.NotificationHubs;

public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
{
   log.Info($"C# Queue trigger function processed: {myQueueItem}");
   notification = GetTemplateNotification(myQueueItem);
}

private static TemplateNotification GetTemplateNotification(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return new TemplateNotification(templateProperties);
}
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

