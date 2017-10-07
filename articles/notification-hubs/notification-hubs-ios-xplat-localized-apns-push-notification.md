---
title: "aaaNotification centra lokalizované nejnovější novinky kurz pro iOS"
description: "Zjistěte, jak Azure Service Bus Notification Hubs toosend toouse lokalizované oznámení o aktuálních zprávách (iOS)."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 9fe88c0440e93b72d349574160ddcd85a7ba0be0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-localized-breaking-news-tooios-devices"></a><span data-ttu-id="fe566-103">Použití centra oznámení toosend lokalizované nejnovější novinky tooiOS zařízení</span><span class="sxs-lookup"><span data-stu-id="fe566-103">Use Notification Hubs toosend localized breaking news tooiOS devices</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fe566-104">Windows Store jazyka C#</span><span class="sxs-lookup"><span data-stu-id="fe566-104">Windows Store C#</span></span>](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [<span data-ttu-id="fe566-105">iOS</span><span class="sxs-lookup"><span data-stu-id="fe566-105">iOS</span></span>](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="fe566-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="fe566-106">Overview</span></span>
<span data-ttu-id="fe566-107">Toto téma ukazuje, jak toouse hello [šablony](notification-hubs-templates-cross-platform-push-messages.md) funkce Azure Notification Hubs toobroadcast nejnovější zprávy oznámení, které lokalizované podle jazyka a zařízení.</span><span class="sxs-lookup"><span data-stu-id="fe566-107">This topic shows you how toouse hello [templates](notification-hubs-templates-cross-platform-push-messages.md) feature of Azure Notification Hubs toobroadcast breaking news notifications that have been localized by language and device.</span></span> <span data-ttu-id="fe566-108">V tomto kurzu začnete s hello iOS aplikace vytvořená v [toosend použití centra oznámení nejnovější zprávy přes].</span><span class="sxs-lookup"><span data-stu-id="fe566-108">In this tutorial you start with hello iOS app created in [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="fe566-109">Po dokončení, že bude možné tooregister kategorií, které vás zajímají, zadejte jazyk v oznámení, která tooreceive hello a přijímat pouze nabízená oznámení pro hello vybrané kategorie v daném jazyce.</span><span class="sxs-lookup"><span data-stu-id="fe566-109">When complete, you will be able tooregister for categories you are interested in, specify a language in which tooreceive hello notifications, and receive only push notifications for hello selected categories in that language.</span></span>

<span data-ttu-id="fe566-110">Existují dva scénáře toothis částí:</span><span class="sxs-lookup"><span data-stu-id="fe566-110">There are two parts toothis scenario:</span></span>

* <span data-ttu-id="fe566-111">aplikace pro iOS umožňuje klienta zařízení toospecify jazyk a toosubscribe toodifferent nejnovější novinky kategorií;</span><span class="sxs-lookup"><span data-stu-id="fe566-111">iOS app allows client devices toospecify a language, and toosubscribe toodifferent breaking news categories;</span></span>
* <span data-ttu-id="fe566-112">Hello back-end vysílá hello oznámení, pomocí hello **značka** a **šablony** feautres Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="fe566-112">hello back-end broadcasts hello notifications, using hello **tag** and **template** feautres of Azure Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe566-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fe566-113">Prerequisites</span></span>
<span data-ttu-id="fe566-114">Musí jste již dokončili hello [toosend použití centra oznámení nejnovější zprávy přes] kurz a mít hello kód je k dispozici, protože v tomto kurzu staví přímo na tento kód.</span><span class="sxs-lookup"><span data-stu-id="fe566-114">You must have already completed hello [Use Notification Hubs toosend breaking news] tutorial and have hello code available, because this tutorial builds directly upon that code.</span></span>

<span data-ttu-id="fe566-115">Visual Studio 2012 nebo novější je volitelný.</span><span class="sxs-lookup"><span data-stu-id="fe566-115">Visual Studio 2012 or later is optional.</span></span>

## <a name="template-concepts"></a><span data-ttu-id="fe566-116">Koncepty šablon</span><span class="sxs-lookup"><span data-stu-id="fe566-116">Template concepts</span></span>
<span data-ttu-id="fe566-117">V [toosend použití centra oznámení nejnovější zprávy přes] jste vytvořili aplikaci, která používá **značky** toosubscribe toonotifications pro různé zprávy kategorie.</span><span class="sxs-lookup"><span data-stu-id="fe566-117">In [Use Notification Hubs toosend breaking news] you built an app that used **tags** toosubscribe toonotifications for different news categories.</span></span>
<span data-ttu-id="fe566-118">Velký počet aplikací, ale cíli více trhů a vyžadují lokalizace.</span><span class="sxs-lookup"><span data-stu-id="fe566-118">Many apps, however, target multiple markets and require localization.</span></span> <span data-ttu-id="fe566-119">To znamená, že mají toobe lokalizovaný obsah hello hello oznámení, sami a doručené toohello opravte sadu zařízení.</span><span class="sxs-lookup"><span data-stu-id="fe566-119">This means that hello content of hello notifications themselves have toobe localized and delivered toohello correct set of devices.</span></span>
<span data-ttu-id="fe566-120">V tomto tématu ukážeme, jak toouse hello **šablony** lokalizované funkce centra oznámení tooeasily doručování oznámení o aktuálních zprávách.</span><span class="sxs-lookup"><span data-stu-id="fe566-120">In this topic we will show how toouse hello **template** feature of Notification Hubs tooeasily deliver localized breaking news notifications.</span></span>

<span data-ttu-id="fe566-121">Poznámka: jedním ze způsobů toosend lokalizované oznámení je toocreate více verzí jednotlivé značky.</span><span class="sxs-lookup"><span data-stu-id="fe566-121">Note: one way toosend localized notifications is toocreate multiple versions of each tag.</span></span> <span data-ttu-id="fe566-122">Například toosupport angličtinu, francouzštinu a Mandarínština, musíme tří různých značek pro world zprávy: "world_en", "world_fr" a "world_ch".</span><span class="sxs-lookup"><span data-stu-id="fe566-122">For instance, toosupport English, French, and Mandarin, we would need three different tags for world news: "world_en", "world_fr", and "world_ch".</span></span> <span data-ttu-id="fe566-123">Pak nám toosend lokalizované verzi hello world zprávy tooeach tyto značky.</span><span class="sxs-lookup"><span data-stu-id="fe566-123">We would then have toosend a localized version of hello world news tooeach of these tags.</span></span> <span data-ttu-id="fe566-124">V tomto tématu používáme šablony tooavoid hello, jak narůstá počet značek a hello požadavek odeslat více zpráv.</span><span class="sxs-lookup"><span data-stu-id="fe566-124">In this topic we use templates tooavoid hello proliferation of tags and hello requirement of sending multiple messages.</span></span>

<span data-ttu-id="fe566-125">Na vysoké úrovni, šablony jsou toospecify způsob jak určité zařízení měli obdržet oznámení.</span><span class="sxs-lookup"><span data-stu-id="fe566-125">At a high level, templates are a way toospecify how a specific device should receive a notification.</span></span> <span data-ttu-id="fe566-126">Šablona Hello Určuje formát datové části přesně hello tím, že odkazuje tooproperties, které jsou součástí uvítací zprávu poslal váš back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe566-126">hello template specifies hello exact payload format by referring tooproperties that are part of hello message sent by your app back-end.</span></span> <span data-ttu-id="fe566-127">V našem případě pošleme zprávu bez ohledu na národním prostředí obsahující všechny podporované jazyky:</span><span class="sxs-lookup"><span data-stu-id="fe566-127">In our case, we will send a locale-agnostic message containing all supported languages:</span></span>

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

<span data-ttu-id="fe566-128">Potom jsme zajistí, že zařízení zaregistrovat pomocí šablony, která odkazuje vlastnost toohello správné.</span><span class="sxs-lookup"><span data-stu-id="fe566-128">Then we will ensure that devices register with a template that refers toohello correct property.</span></span> <span data-ttu-id="fe566-129">Například aplikaci iOS, která chce tooregister pro francouzštině zprávy se registrují hello následující:</span><span class="sxs-lookup"><span data-stu-id="fe566-129">For instance,  an iOS app that wants tooregister for French news will register hello following:</span></span>

    {
        aps:{
            alert: "$(News_French)"
        }
    }

<span data-ttu-id="fe566-130">Šablony jsou velmi výkonné funkce, můžete další informace o možnostech v našem [šablony](notification-hubs-templates-cross-platform-push-messages.md) článku.</span><span class="sxs-lookup"><span data-stu-id="fe566-130">Templates are a very powerful feature you can learn more about in our [Templates](notification-hubs-templates-cross-platform-push-messages.md) article.</span></span>

## <a name="hello-app-user-interface"></a><span data-ttu-id="fe566-131">uživatelské rozhraní aplikace Hello</span><span class="sxs-lookup"><span data-stu-id="fe566-131">hello app user interface</span></span>
<span data-ttu-id="fe566-132">Nyní jsme upraví hello novinkách aplikaci, kterou jste vytvořili v tématu hello [toosend použití centra oznámení nejnovější zprávy přes] toosend lokalizované novinek pomocí šablon.</span><span class="sxs-lookup"><span data-stu-id="fe566-132">We will now modify hello Breaking News app that you created in hello topic [Use Notification Hubs toosend breaking news] toosend localized breaking news using templates.</span></span>

<span data-ttu-id="fe566-133">Vaše MainStoryboard_iPhone.storyboard přidat Segmentovaným ovládací prvek s hello tři jazyky, které bude podporujeme: angličtina, francouzština a Mandarínština.</span><span class="sxs-lookup"><span data-stu-id="fe566-133">In your MainStoryboard_iPhone.storyboard, add a Segmented Control with hello three languages which we will support: English, French, and Mandarin.</span></span>

![][13]

<span data-ttu-id="fe566-134">Proveďte zda tooadd IBOutlet ve vaší ViewController.h jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="fe566-134">Then make sure tooadd an IBOutlet in your ViewController.h as shown below:</span></span>

![][14]

## <a name="building-hello-ios-app"></a><span data-ttu-id="fe566-135">Aplikace pro iOS hello sestavení</span><span class="sxs-lookup"><span data-stu-id="fe566-135">Building hello iOS app</span></span>
1. <span data-ttu-id="fe566-136">Ve vašem Notification.h přidat hello *retrieveLocale* metoda, změna hello úložiště a přihlášení k odběru metod, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="fe566-136">In your Notification.h add hello *retrieveLocale* method, and modify hello store and subscribe methods as shown below:</span></span>
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    <span data-ttu-id="fe566-137">V Notification.m, upravte hello *storeCategoriesAndSubscribe* metoda přidáním hello parametr národního prostředí a ukládání hello výchozí nastavení uživatele:</span><span class="sxs-lookup"><span data-stu-id="fe566-137">In your Notification.m, modify hello *storeCategoriesAndSubscribe* method, by adding hello locale parameter and storing it in hello user defaults:</span></span>
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
   
    <span data-ttu-id="fe566-138">Potom upravte hello *přihlášení k odběru* metoda tooinclude hello národního prostředí:</span><span class="sxs-lookup"><span data-stu-id="fe566-138">Then modify hello *subscribe* method tooinclude hello locale:</span></span>
   
        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];
   
            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }
   
            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];
   
            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }
   
    <span data-ttu-id="fe566-139">Všimněte si, jak se teď používá metoda hello *registerTemplateWithDeviceToken*, místo *registerNativeWithDeviceToken*.</span><span class="sxs-lookup"><span data-stu-id="fe566-139">Note how we are now using hello method *registerTemplateWithDeviceToken*, instead of *registerNativeWithDeviceToken*.</span></span> <span data-ttu-id="fe566-140">Když jsme zaregistrovat pro šablonu máme tooprovide hello json šablony a také název šablony hello (protože naše aplikace může být vhodné tooregister různé šablony).</span><span class="sxs-lookup"><span data-stu-id="fe566-140">When we register for a template we have tooprovide hello json template and also a name for hello template (as our app might want tooregister different templates).</span></span> <span data-ttu-id="fe566-141">Ujistěte se, že tooregister vaše kategorií jako značky, jak chceme toomake zda tooreceive hello notifciations tyto informace.</span><span class="sxs-lookup"><span data-stu-id="fe566-141">Make sure tooregister your categories as tags, as we want toomake sure tooreceive hello notifciations for those news.</span></span>
   
    <span data-ttu-id="fe566-142">Přidejte metoda tooretrieve hello národního prostředí z hello uživatele výchozí nastavení:</span><span class="sxs-lookup"><span data-stu-id="fe566-142">Add a method tooretrieve hello locale from hello user default settings:</span></span>
   
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
2. <span data-ttu-id="fe566-143">Teď, když jsme upravit Naše třída oznámení, máme toomake jistotu, že naše ViewController díky použití hello nové UISegmentControl.</span><span class="sxs-lookup"><span data-stu-id="fe566-143">Now that we modified our Notifications class, we have toomake sure that our ViewController makes use of hello new UISegmentControl.</span></span> <span data-ttu-id="fe566-144">Přidejte následující řádek ve hello hello *viewDidLoad* metoda toomake zda tooshow hello národního prostředí, je aktuálně vybranou:</span><span class="sxs-lookup"><span data-stu-id="fe566-144">Add hello following line in hello *viewDidLoad* method toomake sure tooshow hello locale that is currently selected:</span></span>
   
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
   
    <span data-ttu-id="fe566-145">Pak na vaše *přihlášení k odběru* metoda, změnit vaše volání toohello *storeCategoriesAndSubscribe* toohello následující:</span><span class="sxs-lookup"><span data-stu-id="fe566-145">Then, in your *subscribe* method, change your call toohello *storeCategoriesAndSubscribe* toohello following:</span></span>
   
        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];
3. <span data-ttu-id="fe566-146">Nakonec máte tooupdate hello *didRegisterForRemoteNotificationsWithDeviceToken* metoda v AppDelegate.m, tak, aby správně můžete aktualizovat registrace při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe566-146">Finally, you have tooupdate hello *didRegisterForRemoteNotificationsWithDeviceToken* method in your AppDelegate.m, so that you can correctly refresh your registration when your app starts.</span></span> <span data-ttu-id="fe566-147">Změnit vaše volání toohello *přihlášení k odběru* metoda oznámení s hello následující:</span><span class="sxs-lookup"><span data-stu-id="fe566-147">Change your call toohello *subscribe* method of notifications with hello following:</span></span>
   
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a><span data-ttu-id="fe566-148">(volitelné) Odesílání oznámení lokalizovanou šablonu z konzolové aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="fe566-148">(optional) Send localized template notifications from .NET console app.</span></span>
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-hello-device"></a><span data-ttu-id="fe566-149">(volitelné) Odesílání oznámení lokalizovanou šablonu z hello zařízení</span><span class="sxs-lookup"><span data-stu-id="fe566-149">(optional) Send localized template notifications from hello device</span></span>
<span data-ttu-id="fe566-150">Pokud nemáte přístup tooVisual Studio, nebo chcete toojust testovací odeslání hello lokalizované šablony oznámení přímo z aplikace hello na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="fe566-150">If you don't have access tooVisual Studio, or want toojust test sending hello localized template notifications directly from hello app on hello device.</span></span>  <span data-ttu-id="fe566-151">Můžete jednoduché přidat toohello parametry šablony hello lokalizované `SendNotificationRESTAPI` metoda definované v předchozích kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="fe566-151">You can simple add hello localized template parameters toohello `SendNotificationRESTAPI` method you defined in hello previous tutorial.</span></span>

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct hello messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated hello token toobe used in hello authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create hello request tooadd hello template notification message toohello hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add hello category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate hello notification message POST request with hello SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add hello notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send hello REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }




## <a name="next-steps"></a><span data-ttu-id="fe566-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fe566-152">Next Steps</span></span>
<span data-ttu-id="fe566-153">Další informace o používání šablon najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="fe566-153">For more information on using templates, see:</span></span>

* <span data-ttu-id="fe566-154">[Upozorněte uživatele s centry oznámení: technologie ASP.NET]</span><span class="sxs-lookup"><span data-stu-id="fe566-154">[Notify users with Notification Hubs: ASP.NET]</span></span>
* <span data-ttu-id="fe566-155">[Upozorněte uživatele s centry oznámení: Mobile Services]</span><span class="sxs-lookup"><span data-stu-id="fe566-155">[Notify users with Notification Hubs: Mobile Services]</span></span>

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[toosend použití centra oznámení nejnovější zprávy přes]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Upozorněte uživatele s centry oznámení: technologie ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Upozorněte uživatele s centry oznámení: Mobile Services]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications tooapp users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
