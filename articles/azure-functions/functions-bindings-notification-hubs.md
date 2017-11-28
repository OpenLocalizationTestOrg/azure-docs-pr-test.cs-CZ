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
# <a name="azure-functions-notification-hub-output-binding"></a><span data-ttu-id="47140-104">Centra oznámení Azure funkce výstup vazby</span><span class="sxs-lookup"><span data-stu-id="47140-104">Azure Functions Notification Hub output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="47140-105">Tento článek vysvětluje, jak centra oznámení Azure vazeb tooconfigure a kódu v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="47140-105">This article explains how tooconfigure and code Azure Notification Hub bindings in Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="47140-106">Funkce může odesílat nabízená oznámení pomocí nakonfigurované centra oznámení Azure se po zadání několika řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="47140-106">Your functions can send push notifications using a configured Azure Notification Hub with a few lines of code.</span></span> <span data-ttu-id="47140-107">Nicméně musí být nakonfigurovány hello centra oznámení Azure pro hello chcete toouse platformy oznámení služby (PNS).</span><span class="sxs-lookup"><span data-stu-id="47140-107">However, hello Azure Notification Hub must be configured for hello Platform Notifications Services (PNS) you want toouse.</span></span> <span data-ttu-id="47140-108">Další informace o konfiguraci centra oznámení Azure a vývoj klientských aplikací, které registrují tooreceive oznámení najdete v tématu [Začínáme s Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) a klikněte na tlačítko svou cílovou platformu klienta v hello nejvyšší.</span><span class="sxs-lookup"><span data-stu-id="47140-108">For more information on configuring an Azure Notification Hub and developing a client applications that register tooreceive notifications, see [Getting started with Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) and click your target client platform at hello top.</span></span>

<span data-ttu-id="47140-109">Hello oznámení, která posíláte může být nativní oznámení nebo šablony oznámení.</span><span class="sxs-lookup"><span data-stu-id="47140-109">hello notifications you send can be native notifications or template notifications.</span></span> <span data-ttu-id="47140-110">Nativní oznámení cílit na konkrétní oznámení platformu jako nakonfigurovaný v hello `platform` vlastnost hello výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="47140-110">Native notifications target a specific notification platform as configured in hello `platform` property of hello output binding.</span></span> <span data-ttu-id="47140-111">Šablona oznámení lze použít tootarget více platforem.</span><span class="sxs-lookup"><span data-stu-id="47140-111">A template notification can be used tootarget multiple platforms.</span></span>   

## <a name="notification-hub-output-binding-properties"></a><span data-ttu-id="47140-112">Vlastnosti vazby výstupu centra oznámení</span><span class="sxs-lookup"><span data-stu-id="47140-112">Notification hub output binding properties</span></span>
<span data-ttu-id="47140-113">soubor function.json Hello poskytuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="47140-113">hello function.json file provides hello following properties:</span></span>

* <span data-ttu-id="47140-114">`name`: Název proměnné používá v kódu funkce pro centra oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="47140-114">`name` : Variable name used in function code for hello notification hub message.</span></span>
* <span data-ttu-id="47140-115">`type`: musí být nastaven příliš*"notificationHub"*.</span><span class="sxs-lookup"><span data-stu-id="47140-115">`type` : must be set too*"notificationHub"*.</span></span>
* <span data-ttu-id="47140-116">`tagExpression`: Výrazy značka povolit toospecify, tooa skupiny zařízení, kteří zaregistrovali tooreceive oznámení, která odpovídají hello značky výrazu doručit oznámení.</span><span class="sxs-lookup"><span data-stu-id="47140-116">`tagExpression` : Tag expressions allow you toospecify that notifications be delivered tooa set of devices who have registered tooreceive notifications that match hello tag expression.</span></span>  <span data-ttu-id="47140-117">Další informace najdete v tématu [výrazy směrování a značky](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="47140-117">For more information, see [Routing and tag expressions](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span></span>
* <span data-ttu-id="47140-118">`hubName`: Název zdroje centra oznámení hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="47140-118">`hubName` : Name of hello notification hub resource in hello Azure portal.</span></span>
* <span data-ttu-id="47140-119">`connection`: Tento připojovací řetězec musí být **nastavení aplikace** toohello připojení nastavit připojovací řetězec *DefaultFullSharedAccessSignature* hodnotu pro vaše Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="47140-119">`connection` : This connection string must be an **Application Setting** connection string set toohello *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
* <span data-ttu-id="47140-120">`direction`: musí být nastaven příliš*"se na"*.</span><span class="sxs-lookup"><span data-stu-id="47140-120">`direction` : must be set too*"out"*.</span></span> 
* <span data-ttu-id="47140-121">`platform`: vlastnost platformy hello udává platformu oznámení hello vaše cíle oznámení.</span><span class="sxs-lookup"><span data-stu-id="47140-121">`platform` : hello platform property indicates hello notification platform your notification targets.</span></span> <span data-ttu-id="47140-122">Musí být jedna z hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="47140-122">Must be one of hello following values:</span></span>
  * <span data-ttu-id="47140-123">Ve výchozím nastavení vlastnost platformy hello je vynechaný výstup hello vazby, může být šablony oznámení použité tootarget na hello centra oznámení Azure nakonfigurovaný libovolné platformě.</span><span class="sxs-lookup"><span data-stu-id="47140-123">By default, if hello platform property is omitted from hello output binding, template notifications can be used tootarget any platform configured on hello Azure Notification Hub.</span></span> <span data-ttu-id="47140-124">Další informace o použití šablony obecně toosend pro různé platformy oznámení pomocí centra oznámení Azure, najdete v části [šablony](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="47140-124">For more information on using templates in general toosend cross platform notifications with an Azure Notification Hub, see [Templates](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span></span>
  * <span data-ttu-id="47140-125">`apns`: Apple Push Notification Service.</span><span class="sxs-lookup"><span data-stu-id="47140-125">`apns` : Apple Push Notification Service.</span></span> <span data-ttu-id="47140-126">Další informace o konfiguraci hello centra oznámení pro služby APN a příjem hello oznámení v aplikaci klienta najdete v tématu [odesílající nabízená oznámení tooiOS pomocí Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="47140-126">For more information on configuring hello notification hub for APNS and receiving hello notification in a client app, see [Sending push notifications tooiOS with Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span></span> 
  * <span data-ttu-id="47140-127">`adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span><span class="sxs-lookup"><span data-stu-id="47140-127">`adm` : [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span></span> <span data-ttu-id="47140-128">Další informace o konfiguraci hello centra oznámení pro ADM a příjem hello oznámení v aplikaci Kindle najdete v tématu [Začínáme s Notification Hubs pro aplikace Kindle](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="47140-128">For more information on configuring hello notification hub for ADM and receiving hello notification in a Kindle app, see [Getting Started with Notification Hubs for Kindle apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span></span> 
  * <span data-ttu-id="47140-129">`gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span><span class="sxs-lookup"><span data-stu-id="47140-129">`gcm` : [Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span></span> <span data-ttu-id="47140-130">Firebase Cloud Messaging, což je nová verze hello GCM, je také podporována.</span><span class="sxs-lookup"><span data-stu-id="47140-130">Firebase Cloud Messaging, which is hello new version of GCM, is also supported.</span></span> <span data-ttu-id="47140-131">Další informace o konfiguraci hello centra oznámení pro GCM/FCM a příjem hello oznámení v aplikaci pro Android klienta najdete v tématu [odesílající nabízená oznámení tooAndroid pomocí Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="47140-131">For more information on configuring hello notification hub for GCM/FCM and receiving hello notification in an Android client app, see [Sending push notifications tooAndroid with Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span></span>
  * <span data-ttu-id="47140-132">`wns`: [Službu nabízených oznámení Windows](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) cílení na platformy Windows.</span><span class="sxs-lookup"><span data-stu-id="47140-132">`wns` : [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) targeting Windows platforms.</span></span> <span data-ttu-id="47140-133">Windows Phone 8.1 a novější také podporuje WNS.</span><span class="sxs-lookup"><span data-stu-id="47140-133">Windows Phone 8.1 and later is also supported by WNS.</span></span> <span data-ttu-id="47140-134">Další informace o konfiguraci hello centra oznámení pro WNS a příjem hello oznámení v aplikaci pro univerzální platformu Windows (UWP) najdete v tématu [Začínáme s centra oznámení pro univerzální platformu aplikace Windows](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="47140-134">For more information on configuring hello notification hub for WNS and receiving hello notification in a Universal Windows Platform (UWP) app, see [Getting started with Notification Hubs for Windows Universal Platform Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span></span>
  * <span data-ttu-id="47140-135">`mpns`: [Microsoft služba nabízených oznámení](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span><span class="sxs-lookup"><span data-stu-id="47140-135">`mpns` : [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span></span> <span data-ttu-id="47140-136">Tato platforma podporuje Windows Phone 8 a starší operační systémy Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="47140-136">This platform supports Windows Phone 8 and earlier Windows Phone platforms.</span></span> <span data-ttu-id="47140-137">Další informace o konfiguraci hello centra oznámení pro MPNS a příjem hello oznámení v aplikaci pro Windows Phone najdete v tématu [odesílající nabízená oznámení pomocí Azure Notification Hubs ve Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span><span class="sxs-lookup"><span data-stu-id="47140-137">For more information on configuring hello notification hub for MPNS and receiving hello notification in a Windows Phone app, see [Sending push notifications with Azure Notification Hubs on Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span></span>

<span data-ttu-id="47140-138">Příklad function.json:</span><span class="sxs-lookup"><span data-stu-id="47140-138">Example function.json:</span></span>

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

## <a name="notification-hub-connection-string-setup"></a><span data-ttu-id="47140-139">Nastavení řetězce připojení centra oznámení</span><span class="sxs-lookup"><span data-stu-id="47140-139">Notification hub connection string setup</span></span>
<span data-ttu-id="47140-140">toouse centra oznámení výstup vazba, je nutné nakonfigurovat hello připojovací řetězec pro hello rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="47140-140">toouse a Notification hub output binding, you must configure hello connection string for hello hub.</span></span> <span data-ttu-id="47140-141">To lze provést na hello *integrací* karta výběr vaše Centrum oznámení nebo vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="47140-141">This can be done on hello *Integrate* tab by selecting your notification hub or creating a new one.</span></span> 

<span data-ttu-id="47140-142">Můžete také ručně přidat připojovací řetězec pro existující centrum přidáním připojovací řetězec pro hello *DefaultFullSharedAccessSignature* tooyour centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="47140-142">You can also manually add a connection string for an existing hub by adding a connection string for hello *DefaultFullSharedAccessSignature* tooyour notification hub.</span></span> <span data-ttu-id="47140-143">Tento připojovací řetězec poskytuje přístup k funkci zpráv s oznámením toosend oprávnění.</span><span class="sxs-lookup"><span data-stu-id="47140-143">This connection string provides your function access permission toosend notification messages.</span></span> <span data-ttu-id="47140-144">Hello *DefaultFullSharedAccessSignature* hodnotu připojovacího řetězce je přístupná z hello **klíče** tlačítko v hlavním okně hello prostředku centra oznámení v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="47140-144">hello *DefaultFullSharedAccessSignature* connection string value can be accessed from hello **keys** button in hello main blade of your notification hub resource in hello Azure portal.</span></span> <span data-ttu-id="47140-145">toomanually přidat připojovací řetězec pro vaše Centrum hello použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="47140-145">toomanually add a connection string for your hub, use hello following steps:</span></span> 

1. <span data-ttu-id="47140-146">Na hello **aplikaci funkce** okno hello portál Azure, klikněte na tlačítko **nastavení aplikace funkce > Přejít nastavení služby tooApp**.</span><span class="sxs-lookup"><span data-stu-id="47140-146">On hello **Function app** blade of hello Azure portal, click **Function App Settings > Go tooApp Service settings**.</span></span>
2. <span data-ttu-id="47140-147">V hello **nastavení** okně klikněte na tlačítko **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="47140-147">In hello **Settings** blade, click **Application Settings**.</span></span>
3. <span data-ttu-id="47140-148">Projděte dolů toohello **nastavení aplikace** a přidejte položku s názvem pro *DefaultFullSharedAccessSignature* hodnotu pro vaše Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="47140-148">Scroll down toohello **App settings** section, and add a named entry for *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
4. <span data-ttu-id="47140-149">Referenční aplikace nastavení název řetězce v hello výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="47140-149">Reference your App setting string name in hello output bindings.</span></span> <span data-ttu-id="47140-150">Podobně jako příliš**MyHubConnectionString** použít v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="47140-150">Similar too**MyHubConnectionString** used in hello example above.</span></span>

## <a name="apns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="47140-151">Nativní oznámení APNS s aktivační procedury fronty C#</span><span class="sxs-lookup"><span data-stu-id="47140-151">APNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="47140-152">Tento příklad ukazuje, jak toouse typy definované v hello [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend nativní oznámení APNS.</span><span class="sxs-lookup"><span data-stu-id="47140-152">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend a native APNS notification.</span></span> 

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

## <a name="gcm-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="47140-153">Nativní oznámení GCM s aktivační procedury fronty C#</span><span class="sxs-lookup"><span data-stu-id="47140-153">GCM native notifications with C# queue triggers</span></span>
<span data-ttu-id="47140-154">Tento příklad ukazuje, jak toouse typy definované v hello [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend nativní oznámení GCM.</span><span class="sxs-lookup"><span data-stu-id="47140-154">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend a native GCM notification.</span></span> 

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

## <a name="wns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="47140-155">Nativní oznámení WNS s aktivační procedury fronty C#</span><span class="sxs-lookup"><span data-stu-id="47140-155">WNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="47140-156">Tento příklad ukazuje, jak toouse typy definované v hello [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend nativní WNS připít oznámení.</span><span class="sxs-lookup"><span data-stu-id="47140-156">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend a native WNS toast notification.</span></span> 

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

## <a name="template-example-for-nodejs-timer-triggers"></a><span data-ttu-id="47140-157">Příklad šablony pro aktivační události časovače Node.js</span><span class="sxs-lookup"><span data-stu-id="47140-157">Template example for Node.js timer triggers</span></span>
<span data-ttu-id="47140-158">Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `location` a `message`.</span><span class="sxs-lookup"><span data-stu-id="47140-158">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

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

## <a name="template-example-for-f-timer-triggers"></a><span data-ttu-id="47140-159">Příklad šablony pro aktivační události časovače F #</span><span class="sxs-lookup"><span data-stu-id="47140-159">Template example for F# timer triggers</span></span>
<span data-ttu-id="47140-160">Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `location` a `message`.</span><span class="sxs-lookup"><span data-stu-id="47140-160">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a><span data-ttu-id="47140-161">Příklad šablony pomocí parametr typu out</span><span class="sxs-lookup"><span data-stu-id="47140-161">Template example using an out parameter</span></span>
<span data-ttu-id="47140-162">Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `message` zástupný symbol v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="47140-162">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in hello template.</span></span>

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

## <a name="template-example-with-asynchronous-function"></a><span data-ttu-id="47140-163">Příklad šablony s asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="47140-163">Template example with asynchronous function</span></span>
<span data-ttu-id="47140-164">Pokud používáte asynchronní kódu, výstupní parametry nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="47140-164">If you are using asynchronous code, out parameters are not allowed.</span></span> <span data-ttu-id="47140-165">V takovém případě použijte `IAsyncCollector` tooreturn šablony oznámení.</span><span class="sxs-lookup"><span data-stu-id="47140-165">In this case use `IAsyncCollector` tooreturn your template notification.</span></span> <span data-ttu-id="47140-166">Hello následující kód je příkladem výše uvedený kód hello asynchronní.</span><span class="sxs-lookup"><span data-stu-id="47140-166">hello following code is an asynchronous example of hello code above.</span></span> 

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

## <a name="template-example-using-json"></a><span data-ttu-id="47140-167">Příklad šablony pomocí JSON</span><span class="sxs-lookup"><span data-stu-id="47140-167">Template example using JSON</span></span>
<span data-ttu-id="47140-168">Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `message` zástupný symbol v šabloně hello pomocí platný řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="47140-168">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in hello template using a valid JSON string.</span></span>

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a><span data-ttu-id="47140-169">Příklad šablony pomocí Notification Hubs knihovny typů</span><span class="sxs-lookup"><span data-stu-id="47140-169">Template example using Notification Hubs library types</span></span>
<span data-ttu-id="47140-170">Tento příklad ukazuje, jak toouse typy definované v hello [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="47140-170">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="47140-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47140-171">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

