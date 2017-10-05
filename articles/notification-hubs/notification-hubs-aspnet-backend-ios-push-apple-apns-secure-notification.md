---
title: "Azure Notification Hubs zabezpečené Push"
description: "Naučte se odesílání zabezpečené nabízených oznámení do aplikace pro iOS z Azure. Ukázky kódu jsou vytvořeny v Objective-C a C#."
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 17d42b0a-2c80-4e35-a1ed-ed510d19f4b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: e5f09fb3716303bb21fe7442aa6fa8832174838e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-secure-push"></a><span data-ttu-id="7c02c-104">Azure Notification Hubs zabezpečené Push</span><span class="sxs-lookup"><span data-stu-id="7c02c-104">Azure Notification Hubs Secure Push</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7c02c-105">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="7c02c-105">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="7c02c-106">iOS</span><span class="sxs-lookup"><span data-stu-id="7c02c-106">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="7c02c-107">Android</span><span class="sxs-lookup"><span data-stu-id="7c02c-107">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="7c02c-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="7c02c-108">Overview</span></span>
<span data-ttu-id="7c02c-109">Podpora nabízená oznámení v Microsoft Azure umožňuje přístup k infrastruktuře snadno použitelnou, multiplatformní a upraveným nabízené, což výrazně zjednodušuje implementaci nabízená oznámení spotřebních a podnikových aplikací pro mobilní platformy.</span><span class="sxs-lookup"><span data-stu-id="7c02c-109">Push notification support in Microsoft Azure enables you to access an easy-to-use, multiplatform, scaled-out push infrastructure, which greatly simplifies the implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="7c02c-110">Kvůli zákonným omezení zabezpečení, někdy aplikace může chtít zahrnout něco v oznámení, kterou nelze přenést prostřednictvím infrastrukturu pro standardní nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="7c02c-110">Due to regulatory or security constraints, sometimes an application might want to include something in the notification that cannot be transmitted through the standard push notification infrastructure.</span></span> <span data-ttu-id="7c02c-111">Tento kurz popisuje, jak zajistit stejné prostředí posíláním důvěrných informací o prostřednictvím zabezpečeného a ověřené připojení mezi klientské zařízení a back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c02c-111">This tutorial describes how to achieve the same experience by sending sensitive information through a secure, authenticated connection between the client device and the app backend.</span></span>

<span data-ttu-id="7c02c-112">Na vysoké úrovni tok je následující:</span><span class="sxs-lookup"><span data-stu-id="7c02c-112">At a high level, the flow is as follows:</span></span>

1. <span data-ttu-id="7c02c-113">Back-end aplikace:</span><span class="sxs-lookup"><span data-stu-id="7c02c-113">The app back-end:</span></span>
   * <span data-ttu-id="7c02c-114">Zabezpečení datové úložiště v databázi back-end.</span><span class="sxs-lookup"><span data-stu-id="7c02c-114">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="7c02c-115">ID tohoto oznámení se odešle do zařízení (zabezpečené nebudou odeslány žádné informace).</span><span class="sxs-lookup"><span data-stu-id="7c02c-115">Sends the ID of this notification to the device (no secure information is sent).</span></span>
2. <span data-ttu-id="7c02c-116">Aplikace na zařízení, když obdrží oznámení:</span><span class="sxs-lookup"><span data-stu-id="7c02c-116">The app on the device, when receiving the notification:</span></span>
   * <span data-ttu-id="7c02c-117">Zařízení kontaktuje back-end vyžaduje zabezpečené datové části.</span><span class="sxs-lookup"><span data-stu-id="7c02c-117">The device contacts the back-end requesting the secure payload.</span></span>
   * <span data-ttu-id="7c02c-118">Aplikace můžete zobrazit datové části jako upozornění na zařízení.</span><span class="sxs-lookup"><span data-stu-id="7c02c-118">The app can show the payload as a notification on the device.</span></span>

<span data-ttu-id="7c02c-119">Je důležité si uvědomit, že v předchozím toku (a v tomto kurzu) předpokládáme, že zařízení ukládá ověřovací token do místního úložiště, po přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="7c02c-119">It is important to note that in the preceding flow (and in this tutorial), we assume that the device stores an authentication token in local storage, after the user logs in.</span></span> <span data-ttu-id="7c02c-120">Zaručí se tím úplně jednoduché prostředí, protože zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové na oznámení.</span><span class="sxs-lookup"><span data-stu-id="7c02c-120">This guarantees a completely seamless experience, as the device can retrieve the notification’s secure payload using this token.</span></span> <span data-ttu-id="7c02c-121">Pokud vaše aplikace nejsou uložené tokeny ověřování v zařízení, nebo pokud tyto tokeny můžete vypršela platnost, by měla aplikace zařízení při přijetí oznámení zobrazit obecné oznámení uživateli zobrazuje výzvu spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7c02c-121">If your application does not store authentication tokens on the device, or if these tokens can be expired, the device app, upon receiving the notification should display a generic notification prompting the user to launch the app.</span></span> <span data-ttu-id="7c02c-122">Aplikace pak ověřuje uživatele a ukazuje datová část oznámení.</span><span class="sxs-lookup"><span data-stu-id="7c02c-122">The app then authenticates the user and shows the notification payload.</span></span>

<span data-ttu-id="7c02c-123">V tomto kurzu zabezpečení nabízené ukazuje, jak bezpečně odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="7c02c-123">This Secure Push tutorial shows how to send a push notification securely.</span></span> <span data-ttu-id="7c02c-124">Tento kurz je založený na [upozornění uživatelů](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kurzu, a proto kroky musí dokončit v tomto kurzu první.</span><span class="sxs-lookup"><span data-stu-id="7c02c-124">The tutorial builds on the [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial, so you should complete the steps in that tutorial first.</span></span>

> [!NOTE]
> <span data-ttu-id="7c02c-125">V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7c02c-125">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a><span data-ttu-id="7c02c-126">Upravit projekt pro iOS</span><span class="sxs-lookup"><span data-stu-id="7c02c-126">Modify the iOS project</span></span>
<span data-ttu-id="7c02c-127">Teď, když změnit váš back-end aplikace k odesílání jen na *id* oznámení, budete muset změnit své aplikace pro iOS ke zpracování tohoto oznámení a zpětné volání váš back-end pro načtení zabezpečenou zprávu, který se má zobrazit.</span><span class="sxs-lookup"><span data-stu-id="7c02c-127">Now that you modified your app back-end to send just the *id* of a notification, you have to change your iOS app to handle that notification and call back your back-end to retrieve the secure message to be displayed.</span></span>

<span data-ttu-id="7c02c-128">K dosažení tohoto cíle, musíme zapisovat logiku načíst zabezpečený obsah z back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="7c02c-128">To achieve this goal, we have to write the logic to retrieve the secure content from the app back-end.</span></span>

1. <span data-ttu-id="7c02c-129">V **AppDelegate.m**, ujistěte se, že aplikace se zaregistruje pro tichou oznámení, zpracuje id oznámení odeslaných z back-end.</span><span class="sxs-lookup"><span data-stu-id="7c02c-129">In **AppDelegate.m**, make sure the app registers for silent notifications so it processes the notification id sent from the backend.</span></span> <span data-ttu-id="7c02c-130">Přidat **UIRemoteNotificationTypeNewsstandContentAvailability** v didFinishLaunchingWithOptions možnost:</span><span class="sxs-lookup"><span data-stu-id="7c02c-130">Add the **UIRemoteNotificationTypeNewsstandContentAvailability** option in didFinishLaunchingWithOptions:</span></span>
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
2. <span data-ttu-id="7c02c-131">Ve vašem **AppDelegate.m** přidat oddíl implementace v horní části s následující prohlášení:</span><span class="sxs-lookup"><span data-stu-id="7c02c-131">In your **AppDelegate.m** add an implementation section at the top with the following declaration:</span></span>
   
        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end
3. <span data-ttu-id="7c02c-132">V oddílu implementace pak přidejte následující kód, nahraďte zástupný symbol `{back-end endpoint}` s koncovým bodem pro váš back-end získali dříve:</span><span class="sxs-lookup"><span data-stu-id="7c02c-132">Then add in the implementation section the following code, substituting the placeholder `{back-end endpoint}` with the endpoint for your back-end obtained previously:</span></span>

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

1. <span data-ttu-id="7c02c-133">Teď musíme zpracování příchozí oznámení a získání obsahu zobrazíte pomocí této metody výše.</span><span class="sxs-lookup"><span data-stu-id="7c02c-133">Now we have to handle the incoming notification and use the method above to retrieve the content to display.</span></span> <span data-ttu-id="7c02c-134">Nejprve máme povolení spuštěný na pozadí při přijímání nabízených oznámení v aplikaci iOS.</span><span class="sxs-lookup"><span data-stu-id="7c02c-134">First, we have to enable your iOS app to run in the background when receiving a push notification.</span></span> <span data-ttu-id="7c02c-135">V **XCode**vyberte projektu aplikace na levém panelu a pak klikněte na cílovém hlavní aplikace v **cíle** část v centrálním podokně.</span><span class="sxs-lookup"><span data-stu-id="7c02c-135">In **XCode**, select your app project on the left panel, then click your main app target in the **Targets** section from the central pane.</span></span>
2. <span data-ttu-id="7c02c-136">Pak klikněte na vaše **možnosti** v horní části podokna centrální a zkontrolujte, zda **vzdáleného oznámení** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="7c02c-136">Then click your **Capabilities** tab at the top of your central pane, and check the **Remote Notifications** checkbox.</span></span>
   
    ![][IOS1]
3. <span data-ttu-id="7c02c-137">V **AppDelegate.m** přidejte následující metodu ke zpracování nabízených oznámení:</span><span class="sxs-lookup"><span data-stu-id="7c02c-137">In **AppDelegate.m** add the following method to handle push notifications:</span></span>
   
        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);
   
            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];
   
                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];
   
        }
   
    <span data-ttu-id="7c02c-138">Všimněte si, že je vhodnější pro zpracování v případech chybějící vlastnost hlavičky ověřování nebo odmítání back-end.</span><span class="sxs-lookup"><span data-stu-id="7c02c-138">Note that it is preferable to handle the cases of missing authentication header property or rejection by the back-end.</span></span> <span data-ttu-id="7c02c-139">Konkrétní zpracování těchto případech závisí hlavně na cílové činnost koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="7c02c-139">The specific handling of these cases depend mostly on your target user experience.</span></span> <span data-ttu-id="7c02c-140">Jednou z možností je zobrazit oznámení s výzvou obecný pro ověření uživatele pro načtení skutečné oznámení.</span><span class="sxs-lookup"><span data-stu-id="7c02c-140">One option is to display a notification with a generic prompt for the user to authenticate to retrieve the actual notification.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="7c02c-141">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="7c02c-141">Run the Application</span></span>
<span data-ttu-id="7c02c-142">Ke spuštění aplikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="7c02c-142">To run the application, do the following:</span></span>

1. <span data-ttu-id="7c02c-143">V XCode spusťte aplikaci na fyzickém zařízení iOS (nabízených oznámení nebude fungovat v simulátoru).</span><span class="sxs-lookup"><span data-stu-id="7c02c-143">In XCode, run the app on a physical iOS device (push notifications will not work in the simulator).</span></span>
2. <span data-ttu-id="7c02c-144">V aplikaci pro iOS uživatelského rozhraní zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="7c02c-144">In the iOS app UI, enter a username and password.</span></span> <span data-ttu-id="7c02c-145">Mohou to být libovolný řetězec, ale musí být stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7c02c-145">These can be any string, but they must be the same value.</span></span>
3. <span data-ttu-id="7c02c-146">V aplikaci pro iOS uživatelského rozhraní, klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="7c02c-146">In the iOS app UI, click **Log in**.</span></span> <span data-ttu-id="7c02c-147">Pak klikněte na tlačítko **odeslat nabízené**.</span><span class="sxs-lookup"><span data-stu-id="7c02c-147">Then click **Send push**.</span></span> <span data-ttu-id="7c02c-148">Měli byste vidět zabezpečené oznámení se zobrazí v vaše Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="7c02c-148">You should see the secure notification being displayed in your notification center.</span></span>

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
