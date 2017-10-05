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
# <a name="azure-functions-notification-hub-output-binding"></a><span data-ttu-id="bbbf1-104">Centra oznámení Azure funkce výstup vazby</span><span class="sxs-lookup"><span data-stu-id="bbbf1-104">Azure Functions Notification Hub output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="bbbf1-105">Tento článek vysvětluje postup konfigurace a vazby centra oznámení Azure kódu v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-105">This article explains how to configure and code Azure Notification Hub bindings in Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="bbbf1-106">Funkce může odesílat nabízená oznámení pomocí nakonfigurované centra oznámení Azure se po zadání několika řádků kódu.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-106">Your functions can send push notifications using a configured Azure Notification Hub with a few lines of code.</span></span> <span data-ttu-id="bbbf1-107">Nicméně do centra oznámení Azure musí být nakonfigurovány pro služby platformy oznámení (PNS) chcete použít.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-107">However, the Azure Notification Hub must be configured for the Platform Notifications Services (PNS) you want to use.</span></span> <span data-ttu-id="bbbf1-108">Další informace o konfiguraci centra oznámení Azure a vývoj klientských aplikací, které zaregistrovat pro příjem oznámení najdete v tématu [Začínáme s Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) a klikněte na tlačítko svou cílovou platformu klienta v horní části.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-108">For more information on configuring an Azure Notification Hub and developing a client applications that register to receive notifications, see [Getting started with Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) and click your target client platform at the top.</span></span>

<span data-ttu-id="bbbf1-109">Odesílat oznámení může být nativní oznámení nebo šablony oznámení.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-109">The notifications you send can be native notifications or template notifications.</span></span> <span data-ttu-id="bbbf1-110">Nativní oznámení cílit na konkrétní oznámení platformu jako nakonfigurovaný v `platform` vlastnost vazby výstup.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-110">Native notifications target a specific notification platform as configured in the `platform` property of the output binding.</span></span> <span data-ttu-id="bbbf1-111">Šablona oznámení můžete použít pro více cílových platforem.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-111">A template notification can be used to target multiple platforms.</span></span>   

## <a name="notification-hub-output-binding-properties"></a><span data-ttu-id="bbbf1-112">Vlastnosti vazby výstupu centra oznámení</span><span class="sxs-lookup"><span data-stu-id="bbbf1-112">Notification hub output binding properties</span></span>
<span data-ttu-id="bbbf1-113">Soubor function.json poskytuje následujících vlastností:</span><span class="sxs-lookup"><span data-stu-id="bbbf1-113">The function.json file provides the following properties:</span></span>

* <span data-ttu-id="bbbf1-114">`name`: Název proměnné používá v kódu funkce pro centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-114">`name` : Variable name used in function code for the notification hub message.</span></span>
* <span data-ttu-id="bbbf1-115">`type`: musí být nastavena na *"notificationHub"*.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-115">`type` : must be set to *"notificationHub"*.</span></span>
* <span data-ttu-id="bbbf1-116">`tagExpression`: Výrazy značka umožňují určit doručit oznámení na skupiny zařízení, kteří zaregistrovali k odběru oznámení, které odpovídají výrazu značky.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-116">`tagExpression` : Tag expressions allow you to specify that notifications be delivered to a set of devices who have registered to receive notifications that match the tag expression.</span></span>  <span data-ttu-id="bbbf1-117">Další informace najdete v tématu [výrazy směrování a značky](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="bbbf1-117">For more information, see [Routing and tag expressions](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span></span>
* <span data-ttu-id="bbbf1-118">`hubName`: Název prostředku centra oznámení na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-118">`hubName` : Name of the notification hub resource in the Azure portal.</span></span>
* <span data-ttu-id="bbbf1-119">`connection`: Tento připojovací řetězec musí být **nastavení aplikace** připojení nastavit připojovací řetězec *DefaultFullSharedAccessSignature* hodnotu pro vaše Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-119">`connection` : This connection string must be an **Application Setting** connection string set to the *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
* <span data-ttu-id="bbbf1-120">`direction`: musí být nastavena na *"se na"*.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-120">`direction` : must be set to *"out"*.</span></span> 
* <span data-ttu-id="bbbf1-121">`platform`: Vlastnost platformy označuje platformou oznámení vaše cíle oznámení.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-121">`platform` : The platform property indicates the notification platform your notification targets.</span></span> <span data-ttu-id="bbbf1-122">Toto musí být jedna z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="bbbf1-122">Must be one of the following values:</span></span>
  * <span data-ttu-id="bbbf1-123">Ve výchozím nastavení pokud je vlastnost platformy vynechán z vazby výstup šablony oznámení lze cílit na libovolnou platformu nakonfigurované na do centra oznámení Azure.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-123">By default, if the platform property is omitted from the output binding, template notifications can be used to target any platform configured on the Azure Notification Hub.</span></span> <span data-ttu-id="bbbf1-124">Další informace o použití šablony obecně pro různé platformy oznámení pomocí Azure Notification Hubs k odesílání najdete v tématu [šablony](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="bbbf1-124">For more information on using templates in general to send cross platform notifications with an Azure Notification Hub, see [Templates](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span></span>
  * <span data-ttu-id="bbbf1-125">`apns`: Apple Push Notification Service.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-125">`apns` : Apple Push Notification Service.</span></span> <span data-ttu-id="bbbf1-126">Další informace o konfiguraci centra oznámení pro služby APN a příjem oznámení v aplikaci klienta najdete v tématu [odesílající nabízená oznámení iOS pomocí Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="bbbf1-126">For more information on configuring the notification hub for APNS and receiving the notification in a client app, see [Sending push notifications to iOS with Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span></span> 
  * <span data-ttu-id="bbbf1-127">`adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span><span class="sxs-lookup"><span data-stu-id="bbbf1-127">`adm` : [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span></span> <span data-ttu-id="bbbf1-128">Další informace o konfiguraci centra oznámení pro ADM a příjem oznámení ve Kindle aplikaci najdete v tématu [Začínáme s Notification Hubs pro aplikace Kindle](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="bbbf1-128">For more information on configuring the notification hub for ADM and receiving the notification in a Kindle app, see [Getting Started with Notification Hubs for Kindle apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span></span> 
  * <span data-ttu-id="bbbf1-129">`gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span><span class="sxs-lookup"><span data-stu-id="bbbf1-129">`gcm` : [Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span></span> <span data-ttu-id="bbbf1-130">Firebase Cloud Messaging, což je nová verze služby GCM, je také podporována.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-130">Firebase Cloud Messaging, which is the new version of GCM, is also supported.</span></span> <span data-ttu-id="bbbf1-131">Další informace o konfiguraci centra oznámení pro GCM/FCM a příjem oznámení v aplikaci pro Android klienta najdete v tématu [odesílající nabízená oznámení do systému Android pomocí Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="bbbf1-131">For more information on configuring the notification hub for GCM/FCM and receiving the notification in an Android client app, see [Sending push notifications to Android with Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span></span>
  * <span data-ttu-id="bbbf1-132">`wns`: [Službu nabízených oznámení Windows](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) cílení na platformy Windows.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-132">`wns` : [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) targeting Windows platforms.</span></span> <span data-ttu-id="bbbf1-133">Windows Phone 8.1 a novější také podporuje WNS.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-133">Windows Phone 8.1 and later is also supported by WNS.</span></span> <span data-ttu-id="bbbf1-134">Další informace o konfiguraci centra oznámení pro WNS a příjem oznámení v aplikaci pro univerzální platformu Windows (UWP) najdete v tématu [Začínáme s centra oznámení pro univerzální platformu aplikace Windows](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="bbbf1-134">For more information on configuring the notification hub for WNS and receiving the notification in a Universal Windows Platform (UWP) app, see [Getting started with Notification Hubs for Windows Universal Platform Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span></span>
  * <span data-ttu-id="bbbf1-135">`mpns`: [Microsoft služba nabízených oznámení](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span><span class="sxs-lookup"><span data-stu-id="bbbf1-135">`mpns` : [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span></span> <span data-ttu-id="bbbf1-136">Tato platforma podporuje Windows Phone 8 a starší operační systémy Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-136">This platform supports Windows Phone 8 and earlier Windows Phone platforms.</span></span> <span data-ttu-id="bbbf1-137">Další informace o konfiguraci centra oznámení pro MPNS a příjem oznámení v aplikaci pro Windows Phone najdete v tématu [odesílající nabízená oznámení pomocí Azure Notification Hubs ve Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span><span class="sxs-lookup"><span data-stu-id="bbbf1-137">For more information on configuring the notification hub for MPNS and receiving the notification in a Windows Phone app, see [Sending push notifications with Azure Notification Hubs on Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span></span>

<span data-ttu-id="bbbf1-138">Příklad function.json:</span><span class="sxs-lookup"><span data-stu-id="bbbf1-138">Example function.json:</span></span>

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

## <a name="notification-hub-connection-string-setup"></a><span data-ttu-id="bbbf1-139">Nastavení řetězce připojení centra oznámení</span><span class="sxs-lookup"><span data-stu-id="bbbf1-139">Notification hub connection string setup</span></span>
<span data-ttu-id="bbbf1-140">Pokud chcete použít výstupu centra oznámení vazby, musíte nakonfigurovat připojovací řetězec pro rozbočovač.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-140">To use a Notification hub output binding, you must configure the connection string for the hub.</span></span> <span data-ttu-id="bbbf1-141">To lze provést na *integrací* karta výběr vaše Centrum oznámení nebo vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-141">This can be done on the *Integrate* tab by selecting your notification hub or creating a new one.</span></span> 

<span data-ttu-id="bbbf1-142">Můžete také ručně přidat připojovací řetězec pro existující centrum přidáním připojovací řetězec *DefaultFullSharedAccessSignature* do vašeho centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-142">You can also manually add a connection string for an existing hub by adding a connection string for the *DefaultFullSharedAccessSignature* to your notification hub.</span></span> <span data-ttu-id="bbbf1-143">Tento připojovací řetězec poskytuje vaše oprávnění k funkci zasílání oznámení.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-143">This connection string provides your function access permission to send notification messages.</span></span> <span data-ttu-id="bbbf1-144">*DefaultFullSharedAccessSignature* hodnotu připojovacího řetězce je přístupná z **klíče** tlačítko v hlavním okně prostředku centra oznámení na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-144">The *DefaultFullSharedAccessSignature* connection string value can be accessed from the **keys** button in the main blade of your notification hub resource in the Azure portal.</span></span> <span data-ttu-id="bbbf1-145">Pokud chcete ručně přidat připojovací řetězec pro vaše centrum, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bbbf1-145">To manually add a connection string for your hub, use the following steps:</span></span> 

1. <span data-ttu-id="bbbf1-146">Na **aplikaci funkce** okno portálu Azure klikněte na tlačítko **nastavení aplikace funkce > přejděte na nastavení služby App Service**.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-146">On the **Function app** blade of the Azure portal, click **Function App Settings > Go to App Service settings**.</span></span>
2. <span data-ttu-id="bbbf1-147">V **nastavení** okně klikněte na tlačítko **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-147">In the **Settings** blade, click **Application Settings**.</span></span>
3. <span data-ttu-id="bbbf1-148">Přejděte dolů k položce **nastavení aplikace** a přidejte položku s názvem pro *DefaultFullSharedAccessSignature* hodnotu pro vaše Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-148">Scroll down to the **App settings** section, and add a named entry for *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
4. <span data-ttu-id="bbbf1-149">Referenční název řetězce pro nastavení vaší aplikace ve vazbách výstup.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-149">Reference your App setting string name in the output bindings.</span></span> <span data-ttu-id="bbbf1-150">Podobně jako **MyHubConnectionString** použít v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-150">Similar to **MyHubConnectionString** used in the example above.</span></span>

## <a name="apns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="bbbf1-151">Nativní oznámení APNS s aktivační procedury fronty C#</span><span class="sxs-lookup"><span data-stu-id="bbbf1-151">APNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="bbbf1-152">Tento příklad ukazuje, jak používat typy definované v [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) odeslat nativní oznámení APNS.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-152">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) to send a native APNS notification.</span></span> 

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

## <a name="gcm-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="bbbf1-153">Nativní oznámení GCM s aktivační procedury fronty C#</span><span class="sxs-lookup"><span data-stu-id="bbbf1-153">GCM native notifications with C# queue triggers</span></span>
<span data-ttu-id="bbbf1-154">Tento příklad ukazuje, jak používat typy definované v [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) odeslat nativní oznámení GCM.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-154">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) to send a native GCM notification.</span></span> 

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

## <a name="wns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="bbbf1-155">Nativní oznámení WNS s aktivační procedury fronty C#</span><span class="sxs-lookup"><span data-stu-id="bbbf1-155">WNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="bbbf1-156">Tento příklad ukazuje, jak používat typy definované v [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) odeslat nativní oznámení WNS informační zprávy.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-156">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) to send a native WNS toast notification.</span></span> 

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

## <a name="template-example-for-nodejs-timer-triggers"></a><span data-ttu-id="bbbf1-157">Příklad šablony pro aktivační události časovače Node.js</span><span class="sxs-lookup"><span data-stu-id="bbbf1-157">Template example for Node.js timer triggers</span></span>
<span data-ttu-id="bbbf1-158">Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `location` a `message`.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-158">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

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

## <a name="template-example-for-f-timer-triggers"></a><span data-ttu-id="bbbf1-159">Příklad šablony pro aktivační události časovače F #</span><span class="sxs-lookup"><span data-stu-id="bbbf1-159">Template example for F# timer triggers</span></span>
<span data-ttu-id="bbbf1-160">Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `location` a `message`.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-160">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a><span data-ttu-id="bbbf1-161">Příklad šablony pomocí parametr typu out</span><span class="sxs-lookup"><span data-stu-id="bbbf1-161">Template example using an out parameter</span></span>
<span data-ttu-id="bbbf1-162">Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `message` zástupný symbol v šabloně.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-162">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in the template.</span></span>

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

## <a name="template-example-with-asynchronous-function"></a><span data-ttu-id="bbbf1-163">Příklad šablony s asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="bbbf1-163">Template example with asynchronous function</span></span>
<span data-ttu-id="bbbf1-164">Pokud používáte asynchronní kódu, výstupní parametry nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-164">If you are using asynchronous code, out parameters are not allowed.</span></span> <span data-ttu-id="bbbf1-165">V takovém případě použijte `IAsyncCollector` vrátit šablony oznámení.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-165">In this case use `IAsyncCollector` to return your template notification.</span></span> <span data-ttu-id="bbbf1-166">Následující kód je příkladem asynchronní výše uvedený kód.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-166">The following code is an asynchronous example of the code above.</span></span> 

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

## <a name="template-example-using-json"></a><span data-ttu-id="bbbf1-167">Příklad šablony pomocí JSON</span><span class="sxs-lookup"><span data-stu-id="bbbf1-167">Template example using JSON</span></span>
<span data-ttu-id="bbbf1-168">Tento příklad odešle oznámení [šablony registrace](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) obsahující `message` zástupný symbol v šabloně pomocí platný řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="bbbf1-168">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in the template using a valid JSON string.</span></span>

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a><span data-ttu-id="bbbf1-169">Příklad šablony pomocí Notification Hubs knihovny typů</span><span class="sxs-lookup"><span data-stu-id="bbbf1-169">Template example using Notification Hubs library types</span></span>
<span data-ttu-id="bbbf1-170">Tento příklad ukazuje, jak používat typy definované v [knihovnu Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="bbbf1-170">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="bbbf1-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bbbf1-171">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

