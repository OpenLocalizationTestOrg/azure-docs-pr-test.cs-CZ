---
title: "Vazba funkce centra oznámení aaaAzure | Microsoft Docs"
description: "Pochopit, jak toouse centra oznámení Azure vazby v Azure Functions."
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
ms.openlocfilehash: d192424a8ec701d02f8bcb4aa4c1d189b20537a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-notification-hub-output-binding"></a>Centra oznámení Azure funkce výstup vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak centra oznámení Azure vazeb tooconfigure a kódu v Azure Functions. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Funkce může odesílat nabízená oznámení pomocí nakonfigurované centra oznámení Azure se po zadání několika řádků kódu. Nicméně musí být nakonfigurovány hello centra oznámení Azure pro hello chcete toouse platformy oznámení služby (PNS). Další informace o konfiguraci centra oznámení Azure a vývoj klientských aplikací, které registrují tooreceive oznámení najdete v tématu [Začínáme s Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) a klikněte na tlačítko svou cílovou platformu klienta v hello nejvyšší.

Hello oznámení, která posíláte může být nativní oznámení nebo šablony oznámení. Nativní oznámení cílit na konkrétní oznámení platformu jako nakonfigurovaný v hello `platform` vlastnost hello výstup vazby. Šablona oznámení lze použít tootarget více platforem.   

## <a name="notification-hub-output-binding-properties"></a>Vlastnosti vazby výstupu centra oznámení
soubor function.json Hello poskytuje hello následující vlastnosti:

* `name`: Název proměnné používá v kódu funkce pro centra oznámení hello.
* `type`: musí být nastaven příliš*"notificationHub"*.
* `tagExpression`: Výrazy značka povolit toospecify, tooa skupiny zařízení, kteří zaregistrovali tooreceive oznámení, která odpovídají hello značky výrazu doručit oznámení.  Další informace najdete v tématu [výrazy směrování a značky](../notification-hubs/notification-hubs-tags-segment-push-message.md).
* `hubName`: Název zdroje centra oznámení hello v hello portálu Azure.
* `connection`: Tento připojovací řetězec musí být **nastavení aplikace** toohello připojení nastavit připojovací řetězec *DefaultFullSharedAccessSignature* hodnotu pro vaše Centrum oznámení.
* `direction`: musí být nastaven příliš*"se na"*. 
* `platform`: vlastnost platformy hello udává platformu oznámení hello vaše cíle oznámení. Musí být jedna z hello následující hodnoty:
  * Ve výchozím nastavení vlastnost platformy hello je vynechaný výstup hello vazby, může být šablony oznámení použité tootarget na hello centra oznámení Azure nakonfigurovaný libovolné platformě. Další informace o použití šablony obecně toosend pro různé platformy oznámení pomocí centra oznámení Azure, najdete v části [šablony](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
  * `apns`: Apple Push Notification Service. Další informace o konfiguraci hello centra oznámení pro služby APN a příjem hello oznámení v aplikaci klienta najdete v tématu [odesílající nabízená oznámení tooiOS pomocí Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
  * `adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging). Další informace o konfiguraci hello centra oznámení pro ADM a příjem hello oznámení v aplikaci Kindle najdete v tématu [Začínáme s Notification Hubs pro aplikace Kindle](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
  * `gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, což je nová verze hello GCM, je také podporována. Další informace o konfiguraci hello centra oznámení pro GCM/FCM a příjem hello oznámení v aplikaci pro Android klienta najdete v tématu [odesílající nabízená oznámení tooAndroid pomocí Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
  * `wns`: [Službu nabízených oznámení Windows](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) cílení na platformy Windows. Windows Phone 8.1 a novější také podporuje WNS. Další informace o konfiguraci hello centra oznámení pro WNS a příjem hello oznámení v aplikaci pro univerzální platformu Windows (UWP) najdete v tématu [Začínáme s centra oznámení pro univerzální platformu aplikace Windows](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
  * `mpns`: [Microsoft služba nabízených oznámení](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Tato platforma podporuje Windows Phone 8 a starší operační systémy Windows Phone. Další informace o konfiguraci hello centra oznámení pro MPNS a příjem hello oznámení v aplikaci pro Windows Phone najdete v tématu [odesílající nabízená oznámení pomocí Azure Notification Hubs ve Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)

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
toouse centra oznámení výstup vazba, je nutné nakonfigurovat hello připojovací řetězec pro hello rozbočovače. To lze provést na hello *integrací* karta výběr vaše Centrum oznámení nebo vytvořit nový. 

Můžete také ručně přidat připojovací řetězec pro existující centrum přidáním připojovací řetězec pro hello *DefaultFullSharedAccessSignature* tooyour centra oznámení. Tento připojovací řetězec poskytuje přístup k funkci zpráv s oznámením toosend oprávnění. Hello *DefaultFullSharedAccessSignature* hodnotu připojovacího řetězce je přístupná z hello **klíče** tlačítko v hlavním okně hello prostředku centra oznámení v hello portálu Azure. toomanually přidat připojovací řetězec pro vaše Centrum hello použijte následující kroky: 

1. Na hello **aplikaci funkce** okno hello portál Azure, klikněte na tlačítko **nastavení aplikace funkce > Přejít nastavení služby tooApp**.
2. V hello **nastavení** okně klikněte na tlačítko **nastavení aplikace**.
3. Projděte dolů toohello **nastavení aplikace** a přidejte položku s názvem pro *DefaultFullSharedAccessSignature* hodnotu pro vaše Centrum oznámení.
4. Referenční aplikace nastavení název řetězce v hello výstup vazby. Podobně jako příliš**MyHubConnectionString** použít v předchozím příkladu hello.

## <a name="apns-native-notifications-with-c-queue-triggers"></a>Nativní oznámení APNS s aktivační procedury fronty C#
Tento příklad ukazuje, jak toouse typy definované v hello [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend nativní oznámení APNS. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a new user toobe processed in hello form of a JSON string with 
    // a "name" value.
    //
    // hello JSON format for a native APNS notification is ...
    // { "aps": { "alert": "notification message" }}  

    log.Info($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants toobe added (" + 
                                        user.name + ")\" }}";
    log.Info($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="gcm-native-notifications-with-c-queue-triggers"></a>Nativní oznámení GCM s aktivační procedury fronty C#
Tento příklad ukazuje, jak toouse typy definované v hello [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend nativní oznámení GCM. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a new user toobe processed in hello form of a JSON string with 
    // a "name" value.
    //
    // hello JSON format for a native GCM notification is ...
    // { "data": { "message": "notification message" }}  

    log.Info($"Sending GCM notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants toobe added (" + 
                                        user.name + ")\" }}";
    log.Info($"{gcmNotificationPayload}");
    await notification.AddAsync(new GcmNotification(gcmNotificationPayload));        
}
```

## <a name="wns-native-notifications-with-c-queue-triggers"></a>Nativní oznámení WNS s aktivační procedury fronty C#
Tento příklad ukazuje, jak toouse typy definované v hello [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend nativní WNS připít oznámení. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a new user toobe processed in hello form of a JSON string with 
    // a "name" value.
    //
    // hello XML format for a native WNS toast notification is ...
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
                                            "A new user wants toobe added (" + user.name + ")" + 
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
Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `message` zástupný symbol v šabloně hello.

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
Pokud používáte asynchronní kódu, výstupní parametry nejsou povoleny. V takovém případě použijte `IAsyncCollector` tooreturn šablony oznámení. Hello následující kód je příkladem výše uvedený kód hello asynchronní. 

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    log.Info($"Sending Template Notification tooNotification Hub");
    await notification.AddAsync(GetTemplateProperties(myQueueItem));    
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["user"] = "A new user wants toobe added : " + message;
    return templateProperties;
}
```

## <a name="template-example-using-json"></a>Příklad šablony pomocí JSON
Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `message` zástupný symbol v šabloně hello pomocí platný řetězec formátu JSON.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a>Příklad šablony pomocí Notification Hubs knihovny typů
Tento příklad ukazuje, jak toouse typy definované v hello [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 

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

