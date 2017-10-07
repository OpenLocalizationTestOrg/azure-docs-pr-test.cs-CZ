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
# <a name="use-notification-hubs-toosend-localized-breaking-news-tooios-devices"></a>Použití centra oznámení toosend lokalizované nejnovější novinky tooiOS zařízení
> [!div class="op_single_selector"]
> * [Windows Store jazyka C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak toouse hello [šablony](notification-hubs-templates-cross-platform-push-messages.md) funkce Azure Notification Hubs toobroadcast nejnovější zprávy oznámení, které lokalizované podle jazyka a zařízení. V tomto kurzu začnete s hello iOS aplikace vytvořená v [toosend použití centra oznámení nejnovější zprávy přes]. Po dokončení, že bude možné tooregister kategorií, které vás zajímají, zadejte jazyk v oznámení, která tooreceive hello a přijímat pouze nabízená oznámení pro hello vybrané kategorie v daném jazyce.

Existují dva scénáře toothis částí:

* aplikace pro iOS umožňuje klienta zařízení toospecify jazyk a toosubscribe toodifferent nejnovější novinky kategorií;
* Hello back-end vysílá hello oznámení, pomocí hello **značka** a **šablony** feautres Azure Notification Hubs.

## <a name="prerequisites"></a>Požadavky
Musí jste již dokončili hello [toosend použití centra oznámení nejnovější zprávy přes] kurz a mít hello kód je k dispozici, protože v tomto kurzu staví přímo na tento kód.

Visual Studio 2012 nebo novější je volitelný.

## <a name="template-concepts"></a>Koncepty šablon
V [toosend použití centra oznámení nejnovější zprávy přes] jste vytvořili aplikaci, která používá **značky** toosubscribe toonotifications pro různé zprávy kategorie.
Velký počet aplikací, ale cíli více trhů a vyžadují lokalizace. To znamená, že mají toobe lokalizovaný obsah hello hello oznámení, sami a doručené toohello opravte sadu zařízení.
V tomto tématu ukážeme, jak toouse hello **šablony** lokalizované funkce centra oznámení tooeasily doručování oznámení o aktuálních zprávách.

Poznámka: jedním ze způsobů toosend lokalizované oznámení je toocreate více verzí jednotlivé značky. Například toosupport angličtinu, francouzštinu a Mandarínština, musíme tří různých značek pro world zprávy: "world_en", "world_fr" a "world_ch". Pak nám toosend lokalizované verzi hello world zprávy tooeach tyto značky. V tomto tématu používáme šablony tooavoid hello, jak narůstá počet značek a hello požadavek odeslat více zpráv.

Na vysoké úrovni, šablony jsou toospecify způsob jak určité zařízení měli obdržet oznámení. Šablona Hello Určuje formát datové části přesně hello tím, že odkazuje tooproperties, které jsou součástí uvítací zprávu poslal váš back-end aplikace. V našem případě pošleme zprávu bez ohledu na národním prostředí obsahující všechny podporované jazyky:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Potom jsme zajistí, že zařízení zaregistrovat pomocí šablony, která odkazuje vlastnost toohello správné. Například aplikaci iOS, která chce tooregister pro francouzštině zprávy se registrují hello následující:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Šablony jsou velmi výkonné funkce, můžete další informace o možnostech v našem [šablony](notification-hubs-templates-cross-platform-push-messages.md) článku.

## <a name="hello-app-user-interface"></a>uživatelské rozhraní aplikace Hello
Nyní jsme upraví hello novinkách aplikaci, kterou jste vytvořili v tématu hello [toosend použití centra oznámení nejnovější zprávy přes] toosend lokalizované novinek pomocí šablon.

Vaše MainStoryboard_iPhone.storyboard přidat Segmentovaným ovládací prvek s hello tři jazyky, které bude podporujeme: angličtina, francouzština a Mandarínština.

![][13]

Proveďte zda tooadd IBOutlet ve vaší ViewController.h jak je uvedeno níže:

![][14]

## <a name="building-hello-ios-app"></a>Aplikace pro iOS hello sestavení
1. Ve vašem Notification.h přidat hello *retrieveLocale* metoda, změna hello úložiště a přihlášení k odběru metod, jak je uvedeno níže:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    V Notification.m, upravte hello *storeCategoriesAndSubscribe* metoda přidáním hello parametr národního prostředí a ukládání hello výchozí nastavení uživatele:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
   
    Potom upravte hello *přihlášení k odběru* metoda tooinclude hello národního prostředí:
   
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
   
    Všimněte si, jak se teď používá metoda hello *registerTemplateWithDeviceToken*, místo *registerNativeWithDeviceToken*. Když jsme zaregistrovat pro šablonu máme tooprovide hello json šablony a také název šablony hello (protože naše aplikace může být vhodné tooregister různé šablony). Ujistěte se, že tooregister vaše kategorií jako značky, jak chceme toomake zda tooreceive hello notifciations tyto informace.
   
    Přidejte metoda tooretrieve hello národního prostředí z hello uživatele výchozí nastavení:
   
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
2. Teď, když jsme upravit Naše třída oznámení, máme toomake jistotu, že naše ViewController díky použití hello nové UISegmentControl. Přidejte následující řádek ve hello hello *viewDidLoad* metoda toomake zda tooshow hello národního prostředí, je aktuálně vybranou:
   
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
   
    Pak na vaše *přihlášení k odběru* metoda, změnit vaše volání toohello *storeCategoriesAndSubscribe* toohello následující:
   
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
3. Nakonec máte tooupdate hello *didRegisterForRemoteNotificationsWithDeviceToken* metoda v AppDelegate.m, tak, aby správně můžete aktualizovat registrace při spuštění aplikace. Změnit vaše volání toohello *přihlášení k odběru* metoda oznámení s hello následující:
   
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(volitelné) Odesílání oznámení lokalizovanou šablonu z konzolové aplikace .NET.
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-hello-device"></a>(volitelné) Odesílání oznámení lokalizovanou šablonu z hello zařízení
Pokud nemáte přístup tooVisual Studio, nebo chcete toojust testovací odeslání hello lokalizované šablony oznámení přímo z aplikace hello na hello zařízení.  Můžete jednoduché přidat toohello parametry šablony hello lokalizované `SendNotificationRESTAPI` metoda definované v předchozích kurzu hello.

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




## <a name="next-steps"></a>Další kroky
Další informace o používání šablon najdete v tématu:

* [Upozorněte uživatele s centry oznámení: technologie ASP.NET]
* [Upozorněte uživatele s centry oznámení: Mobile Services]

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
