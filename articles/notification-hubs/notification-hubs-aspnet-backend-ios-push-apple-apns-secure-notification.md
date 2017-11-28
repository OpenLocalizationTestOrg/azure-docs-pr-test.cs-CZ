---
title: "aaaAzure oznámení Centra zabezpečení Push."
description: "Zjistěte, jak toosend zabezpečené nabízená oznámení aplikace iOS tooan z Azure. Ukázky kódu jsou vytvořeny v Objective-C a C#."
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
ms.openlocfilehash: 86dd8d7042e5b9e55d2d7ff41cb42f23831fc575
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-secure-push"></a><span data-ttu-id="de194-104">Azure Notification Hubs zabezpečené Push</span><span class="sxs-lookup"><span data-stu-id="de194-104">Azure Notification Hubs Secure Push</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="de194-105">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="de194-105">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="de194-106">iOS</span><span class="sxs-lookup"><span data-stu-id="de194-106">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="de194-107">Android</span><span class="sxs-lookup"><span data-stu-id="de194-107">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="de194-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="de194-108">Overview</span></span>
<span data-ttu-id="de194-109">Podpora nabízená oznámení v Microsoft Azure umožňuje tooaccess nabízené snadno použitelnou, multiplatformní a škálovanou infrastrukturu, což výrazně zjednodušuje hello implementace nabízených oznámení spotřebních a podnikových aplikací pro mobilní platformy.</span><span class="sxs-lookup"><span data-stu-id="de194-109">Push notification support in Microsoft Azure enables you tooaccess an easy-to-use, multiplatform, scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="de194-110">Z důvodu omezení tooregulatory nebo zabezpečení někdy aplikace může být vhodné tooinclude něco v hello oznámení, kterou nelze přenést prostřednictvím infrastrukturu pro hello standardní nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="de194-110">Due tooregulatory or security constraints, sometimes an application might want tooinclude something in hello notification that cannot be transmitted through hello standard push notification infrastructure.</span></span> <span data-ttu-id="de194-111">Tento kurz popisuje, jak tooachieve hello stejné prostředí posíláním důvěrných informací o prostřednictvím zabezpečeného a ověřené připojení mezi hello klientského zařízení a back-end aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="de194-111">This tutorial describes how tooachieve hello same experience by sending sensitive information through a secure, authenticated connection between hello client device and hello app backend.</span></span>

<span data-ttu-id="de194-112">Na vysoké úrovni tok hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="de194-112">At a high level, hello flow is as follows:</span></span>

1. <span data-ttu-id="de194-113">back-end Hello aplikace:</span><span class="sxs-lookup"><span data-stu-id="de194-113">hello app back-end:</span></span>
   * <span data-ttu-id="de194-114">Zabezpečení datové úložiště v databázi back-end.</span><span class="sxs-lookup"><span data-stu-id="de194-114">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="de194-115">Odešle hello ID tohoto zařízení toohello oznámení (zabezpečené nebudou odeslány žádné informace).</span><span class="sxs-lookup"><span data-stu-id="de194-115">Sends hello ID of this notification toohello device (no secure information is sent).</span></span>
2. <span data-ttu-id="de194-116">aplikace Hello na hello zařízení při přijetí oznámení hello:</span><span class="sxs-lookup"><span data-stu-id="de194-116">hello app on hello device, when receiving hello notification:</span></span>
   * <span data-ttu-id="de194-117">Hello zařízení kontaktuje hello back-end žádajícího hello zabezpečené datové části.</span><span class="sxs-lookup"><span data-stu-id="de194-117">hello device contacts hello back-end requesting hello secure payload.</span></span>
   * <span data-ttu-id="de194-118">Hello aplikace můžete zobrazit datové části hello jako upozornění na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="de194-118">hello app can show hello payload as a notification on hello device.</span></span>

<span data-ttu-id="de194-119">Je důležité, že toonote, v předchozím toku hello (a v tomto kurzu) předpokládáme, že hello zařízení ukládá ověřovací token do místního úložiště, po přihlášení uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="de194-119">It is important toonote that in hello preceding flow (and in this tutorial), we assume that hello device stores an authentication token in local storage, after hello user logs in.</span></span> <span data-ttu-id="de194-120">Zaručí se tím úplně jednoduché prostředí, protože hello zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="de194-120">This guarantees a completely seamless experience, as hello device can retrieve hello notification’s secure payload using this token.</span></span> <span data-ttu-id="de194-121">Pokud vaše aplikace nejsou uložené ověřovací tokeny na hello zařízení nebo pokud tyto tokeny můžete vypršela platnost, hello aplikaci zařízení při přijetí oznámení hello by měl zobrazit obecné oznámení výzvy hello uživatele toolaunch hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="de194-121">If your application does not store authentication tokens on hello device, or if these tokens can be expired, hello device app, upon receiving hello notification should display a generic notification prompting hello user toolaunch hello app.</span></span> <span data-ttu-id="de194-122">aplikace Hello pak ověřuje uživatele hello a ukazuje datová část oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="de194-122">hello app then authenticates hello user and shows hello notification payload.</span></span>

<span data-ttu-id="de194-123">Tento kurz zabezpečení nabízené ukazuje, jak toosend nabízených oznámení bezpečně.</span><span class="sxs-lookup"><span data-stu-id="de194-123">This Secure Push tutorial shows how toosend a push notification securely.</span></span> <span data-ttu-id="de194-124">Hello kurzu vychází hello [upozornění uživatelů](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kurzu, a proto hello kroky musí dokončit v tomto kurzu první.</span><span class="sxs-lookup"><span data-stu-id="de194-124">hello tutorial builds on hello [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial, so you should complete hello steps in that tutorial first.</span></span>

> [!NOTE]
> <span data-ttu-id="de194-125">V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="de194-125">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-ios-project"></a><span data-ttu-id="de194-126">Upravit projektu iOS hello</span><span class="sxs-lookup"><span data-stu-id="de194-126">Modify hello iOS project</span></span>
<span data-ttu-id="de194-127">Teď, když upravit vaše aplikace právě hello back-end toosend *id* oznámení, máte toochange vaše toohandle aplikace iOS oznámení a zpětných volání váš back-end tooretrieve hello zabezpečit zprávy toobe zobrazí.</span><span class="sxs-lookup"><span data-stu-id="de194-127">Now that you modified your app back-end toosend just hello *id* of a notification, you have toochange your iOS app toohandle that notification and call back your back-end tooretrieve hello secure message toobe displayed.</span></span>

<span data-ttu-id="de194-128">tooachieve tento cíl máme toowrite hello logiku tooretrieve hello zabezpečený obsah z back-end aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="de194-128">tooachieve this goal, we have toowrite hello logic tooretrieve hello secure content from hello app back-end.</span></span>

1. <span data-ttu-id="de194-129">V **AppDelegate.m**, ujistěte se, že registry aplikace hello tichou oznámení, zpracovává hello id oznámení odeslaných z back-end hello.</span><span class="sxs-lookup"><span data-stu-id="de194-129">In **AppDelegate.m**, make sure hello app registers for silent notifications so it processes hello notification id sent from hello backend.</span></span> <span data-ttu-id="de194-130">Přidat hello **UIRemoteNotificationTypeNewsstandContentAvailability** v didFinishLaunchingWithOptions možnost:</span><span class="sxs-lookup"><span data-stu-id="de194-130">Add hello **UIRemoteNotificationTypeNewsstandContentAvailability** option in didFinishLaunchingWithOptions:</span></span>
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
2. <span data-ttu-id="de194-131">Ve vašem **AppDelegate.m** přidat oddíl implementace v horní části hello s hello následující prohlášení:</span><span class="sxs-lookup"><span data-stu-id="de194-131">In your **AppDelegate.m** add an implementation section at hello top with hello following declaration:</span></span>
   
        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end
3. <span data-ttu-id="de194-132">Pak přidejte v hello implementace části hello následující kód, nahraďte zástupný symbol hello `{back-end endpoint}` hello koncový bod pro váš back-end získali dříve:</span><span class="sxs-lookup"><span data-stu-id="de194-132">Then add in hello implementation section hello following code, substituting hello placeholder `{back-end endpoint}` with hello endpoint for your back-end obtained previously:</span></span>

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

    This method calls your app back-end tooretrieve hello notification content using hello credentials stored in hello shared preferences.

1. <span data-ttu-id="de194-133">Nyní jsme toohandle hello příchozí oznámení a použít metodu hello výše obsahu toodisplay tooretrieve hello.</span><span class="sxs-lookup"><span data-stu-id="de194-133">Now we have toohandle hello incoming notification and use hello method above tooretrieve hello content toodisplay.</span></span> <span data-ttu-id="de194-134">Nejprve máme tooenable vaše toorun aplikace iOS hello pozadí při přijímání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="de194-134">First, we have tooenable your iOS app toorun in hello background when receiving a push notification.</span></span> <span data-ttu-id="de194-135">V **XCode**vyberte projektu aplikace na levém panelu hello a pak klikněte na cílovém hlavní aplikace v hello **cíle** části z centrální podokno hello.</span><span class="sxs-lookup"><span data-stu-id="de194-135">In **XCode**, select your app project on hello left panel, then click your main app target in hello **Targets** section from hello central pane.</span></span>
2. <span data-ttu-id="de194-136">Pak klikněte na vaše **možnosti** v horní části hello centrální podokna a zkontrolujte hello **vzdáleného oznámení** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="de194-136">Then click your **Capabilities** tab at hello top of your central pane, and check hello **Remote Notifications** checkbox.</span></span>
   
    ![][IOS1]
3. <span data-ttu-id="de194-137">V **AppDelegate.m** přidejte následující metodu toohandle nabízená oznámení hello:</span><span class="sxs-lookup"><span data-stu-id="de194-137">In **AppDelegate.m** add hello following method toohandle push notifications:</span></span>
   
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
   
    <span data-ttu-id="de194-138">Všimněte si, že je vhodnější toohandle hello případech chybějící vlastnost hlavičky ověřování nebo odmítání podle hello back-end.</span><span class="sxs-lookup"><span data-stu-id="de194-138">Note that it is preferable toohandle hello cases of missing authentication header property or rejection by hello back-end.</span></span> <span data-ttu-id="de194-139">Hello konkrétní zpracování těchto případech závisí hlavně na cílové činnost koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="de194-139">hello specific handling of these cases depend mostly on your target user experience.</span></span> <span data-ttu-id="de194-140">Jednou z možností je toodisplay oznámení s obecné výzvu hello uživatele tooauthenticate tooretrieve hello skutečné oznámení.</span><span class="sxs-lookup"><span data-stu-id="de194-140">One option is toodisplay a notification with a generic prompt for hello user tooauthenticate tooretrieve hello actual notification.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="de194-141">Spustit hello aplikace</span><span class="sxs-lookup"><span data-stu-id="de194-141">Run hello Application</span></span>
<span data-ttu-id="de194-142">toorun hello aplikace, hello následující:</span><span class="sxs-lookup"><span data-stu-id="de194-142">toorun hello application, do hello following:</span></span>

1. <span data-ttu-id="de194-143">V XCode spusťte aplikaci hello na fyzickém zařízení iOS (nabízených oznámení nebude fungovat v simulátoru hello).</span><span class="sxs-lookup"><span data-stu-id="de194-143">In XCode, run hello app on a physical iOS device (push notifications will not work in hello simulator).</span></span>
2. <span data-ttu-id="de194-144">V aplikaci pro iOS hello uživatelského rozhraní zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="de194-144">In hello iOS app UI, enter a username and password.</span></span> <span data-ttu-id="de194-145">Mohou to být libovolný řetězec, ale musí být hello stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="de194-145">These can be any string, but they must be hello same value.</span></span>
3. <span data-ttu-id="de194-146">V aplikaci pro iOS hello uživatelského rozhraní, klikněte na **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="de194-146">In hello iOS app UI, click **Log in**.</span></span> <span data-ttu-id="de194-147">Pak klikněte na tlačítko **odeslat nabízené**.</span><span class="sxs-lookup"><span data-stu-id="de194-147">Then click **Send push**.</span></span> <span data-ttu-id="de194-148">Měli byste vidět hello zabezpečené oznámení se zobrazí v vaše Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="de194-148">You should see hello secure notification being displayed in your notification center.</span></span>

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
