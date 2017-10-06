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
# <a name="azure-notification-hubs-secure-push"></a>Azure Notification Hubs zabezpečené Push
> [!div class="op_single_selector"]
> * [Univerzální pro Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Přehled
Podpora nabízená oznámení v Microsoft Azure umožňuje tooaccess nabízené snadno použitelnou, multiplatformní a škálovanou infrastrukturu, což výrazně zjednodušuje hello implementace nabízených oznámení spotřebních a podnikových aplikací pro mobilní platformy.

Z důvodu omezení tooregulatory nebo zabezpečení někdy aplikace může být vhodné tooinclude něco v hello oznámení, kterou nelze přenést prostřednictvím infrastrukturu pro hello standardní nabízená oznámení. Tento kurz popisuje, jak tooachieve hello stejné prostředí posíláním důvěrných informací o prostřednictvím zabezpečeného a ověřené připojení mezi hello klientského zařízení a back-end aplikace hello.

Na vysoké úrovni tok hello vypadá takto:

1. back-end Hello aplikace:
   * Zabezpečení datové úložiště v databázi back-end.
   * Odešle hello ID tohoto zařízení toohello oznámení (zabezpečené nebudou odeslány žádné informace).
2. aplikace Hello na hello zařízení při přijetí oznámení hello:
   * Hello zařízení kontaktuje hello back-end žádajícího hello zabezpečené datové části.
   * Hello aplikace můžete zobrazit datové části hello jako upozornění na hello zařízení.

Je důležité, že toonote, v předchozím toku hello (a v tomto kurzu) předpokládáme, že hello zařízení ukládá ověřovací token do místního úložiště, po přihlášení uživatele hello. Zaručí se tím úplně jednoduché prostředí, protože hello zařízení můžete načíst pomocí tohoto tokenu zabezpečení datové hello oznámení. Pokud vaše aplikace nejsou uložené ověřovací tokeny na hello zařízení nebo pokud tyto tokeny můžete vypršela platnost, hello aplikaci zařízení při přijetí oznámení hello by měl zobrazit obecné oznámení výzvy hello uživatele toolaunch hello aplikace. aplikace Hello pak ověřuje uživatele hello a ukazuje datová část oznámení hello.

Tento kurz zabezpečení nabízené ukazuje, jak toosend nabízených oznámení bezpečně. Hello kurzu vychází hello [upozornění uživatelů](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kurzu, a proto hello kroky musí dokončit v tomto kurzu první.

> [!NOTE]
> V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-ios-project"></a>Upravit projektu iOS hello
Teď, když upravit vaše aplikace právě hello back-end toosend *id* oznámení, máte toochange vaše toohandle aplikace iOS oznámení a zpětných volání váš back-end tooretrieve hello zabezpečit zprávy toobe zobrazí.

tooachieve tento cíl máme toowrite hello logiku tooretrieve hello zabezpečený obsah z back-end aplikace hello.

1. V **AppDelegate.m**, ujistěte se, že registry aplikace hello tichou oznámení, zpracovává hello id oznámení odeslaných z back-end hello. Přidat hello **UIRemoteNotificationTypeNewsstandContentAvailability** v didFinishLaunchingWithOptions možnost:
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
2. Ve vašem **AppDelegate.m** přidat oddíl implementace v horní části hello s hello následující prohlášení:
   
        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end
3. Pak přidejte v hello implementace části hello následující kód, nahraďte zástupný symbol hello `{back-end endpoint}` hello koncový bod pro váš back-end získali dříve:

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

1. Nyní jsme toohandle hello příchozí oznámení a použít metodu hello výše obsahu toodisplay tooretrieve hello. Nejprve máme tooenable vaše toorun aplikace iOS hello pozadí při přijímání nabízených oznámení. V **XCode**vyberte projektu aplikace na levém panelu hello a pak klikněte na cílovém hlavní aplikace v hello **cíle** části z centrální podokno hello.
2. Pak klikněte na vaše **možnosti** v horní části hello centrální podokna a zkontrolujte hello **vzdáleného oznámení** zaškrtávací políčko.
   
    ![][IOS1]
3. V **AppDelegate.m** přidejte následující metodu toohandle nabízená oznámení hello:
   
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
   
    Všimněte si, že je vhodnější toohandle hello případech chybějící vlastnost hlavičky ověřování nebo odmítání podle hello back-end. Hello konkrétní zpracování těchto případech závisí hlavně na cílové činnost koncového uživatele. Jednou z možností je toodisplay oznámení s obecné výzvu hello uživatele tooauthenticate tooretrieve hello skutečné oznámení.

## <a name="run-hello-application"></a>Spustit hello aplikace
toorun hello aplikace, hello následující:

1. V XCode spusťte aplikaci hello na fyzickém zařízení iOS (nabízených oznámení nebude fungovat v simulátoru hello).
2. V aplikaci pro iOS hello uživatelského rozhraní zadejte uživatelské jméno a heslo. Mohou to být libovolný řetězec, ale musí být hello stejnou hodnotu.
3. V aplikaci pro iOS hello uživatelského rozhraní, klikněte na **přihlásit**. Pak klikněte na tlačítko **odeslat nabízené**. Měli byste vidět hello zabezpečené oznámení se zobrazí v vaše Centrum oznámení.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
