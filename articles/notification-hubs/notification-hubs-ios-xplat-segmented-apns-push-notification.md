---
title: "aaaNotification centra nejnovější novinky kurz – iOS"
description: "Zjistěte, jak toosend Azure Service Bus Notification Hubs toouse nejnovější zprávy oznámení tooiOS zařízení."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 6ead4169-deff-4947-858c-8c6cf03cc3b2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 763b80b5ffed238b351d95bd3d6a96cb914f53cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a>Použít nejnovější zprávy přes toosend centra oznámení
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak toouse Azure Notification Hubs toobroadcast nejnovější zprávy oznámení tooan aplikace iOS. Po dokončení budete mít možnost tooregister pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie. Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení odeslaných toobe toogroups uživatelů, kteří mají dříve deklarované zájem o jejich, např. čtečku RSS, aplikace pro Hudba ventilátory, atd.

Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení hello. Pokud jsou oznámení odesílána tooa značky, všechna zařízení, která jste zaregistrovali hello značky obdrží oznámení hello. Protože značky jsou jednoduše řetězce, nemají toobe předem zřízený. Další informace o značkách najdete v části příliš[směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Požadavky
Toto téma je založený na hello aplikace, které jste vytvořili v [Začínáme s Notification Hubs][get-started]. Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs][get-started].

## <a name="add-category-selection-toohello-app"></a>Přidat aplikaci toohello výběru kategorie
prvním krokem Hello je tooadd hello uživatelského rozhraní elementy tooyour existující scénáře, povolit hello uživatele tooselect kategorie tooregister. kategorie Hello vybrané uživatelem se ukládají na hello zařízení. Při spuštění aplikace hello, registrace zařízení se vytvoří v centru oznámení s hello vybrané kategorie jako značky.

1. Ve vaší MainStoryboard_iPhone.storyboard přidáte hello z objektu knihovny hello následující součásti:
   
   * Popisek s textem "Novinkách."
   * Popisky se texty kategorie "World", "Politika", "Firemní", "Technologie", "Vědecké účely", "Sports",
   * Šest přepínače, jeden pro každou kategorii nastavit každý přepínač **stavu** toobe **vypnout** ve výchozím nastavení.
   * Jedno tlačítko s označením "Přihlásit k odběru"
     
     Vaše scénáře by měla vypadat takto:
     
     ![][3]
2. V editoru pomocníka hello vytvořit výstupy u všech přepínačů hello a volat je "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"
3. Vytvoří akci pro vaše tlačítko se nazývá "přihlásit". Vaše ViewController.h by měl obsahovat hello následující:
   
        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;
   
        - (IBAction)subscribe:(id)sender;
4. Vytvořte novou **kakao Touch třída** názvem `Notifications`. Zkopírujte následující kód v části rozhraní hello hello souboru Notifications.h hello:
   
        @property NSData* deviceToken;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;
   
        - (NSSet*)retrieveCategories;
   
        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
5. Přidejte následující direktivy tooNotifications.m import hello:
   
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
6. Zkopírujte následující kód v oddílu implementace hello hello souboru Notifications.m hello.
   
        SBNotificationHub* hub;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{
   
            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];
   
            return self;
        }
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
   
            [self subscribeWithCategories:categories completion:completion];
        }

        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Tato třída se používá místní úložiště toostore a načtení kategorií hello příspěvků, který obdrží toto zařízení. Také obsahuje tooregister metoda pro tyto kategorie pomocí [šablony](notification-hubs-templates-cross-platform-push-messages.md) registrace.

1. V souboru AppDelegate.h hello přidejte příkaz import pro Notifications.h a přidání vlastnosti pro instanci hello třída oznámení:
   
        #import "Notifications.h"
   
        @property (nonatomic) Notifications* notifications;
2. V hello **didFinishLaunchingWithOptions** metoda v AppDelegate.m, přidat hello kód tooinitialize hello oznámení instanci od začátku hello hello metody.  
   
    `HUBNAME`a `HUBLISTENACCESS` (definovanou v souboru hubinfo.h) byste již měli mít hello `<hub name>` a `<connection string with listen access>` nahradit zástupné symboly oznámení centra název a hello připojovacím řetězcem pro *DefaultListenSharedAccessSignature*získaného dříve
   
        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
   
   > [!NOTE]
   > Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci nejsou obecně bezpečné, musí distribuovat hello klíč pro přístup k naslouchání pouze s vaší klientské aplikace. Poslechněte umožní přístup k vaší aplikaci tooregister oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení. Hello úplné přístupový klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.
   > 
   > 
3. V hello **didRegisterForRemoteNotificationsWithDeviceToken** metoda v AppDelegate.m, nahraďte hello následující kód toopass hello zařízení tokenu toohello oznámení třída hello kód v metodě hello. Třída oznámení Hello provede hello registrace pro oznámení s hello kategorií. Pokud uživatel hello změní výběru kategorií, říkáme hello `subscribeWithCategories` metoda v odpovědi toohello **přihlášení k odběru** tlačítko tooupdate je.
   
   > [!NOTE]
   > Protože token zařízení hello přiřadila hello Apple Push Notification Service (APNS) můžete kdykoli šance, byste měli zaregistrovat pro oznámení často tooavoid oznámení selhání. Tento příklad zaregistruje oznámení pokaždé, když spustí aplikaci hello. Pro aplikace, které jsou často spouštíte více než jednou denně, pravděpodobně Pokud můžete přeskočit šířky pásma toopreserve registrace od předchozí registrace hello uplynul méně než jeden den.
   > 
   > 
   
        self.notifications.deviceToken = deviceToken;
   
        // Retrieves hello categories from local storage and requests a registration for these categories
        // each time hello app starts and performs a registration.
   
        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

    V tomto okamžiku došlo by mělo být žádný další kód v hello **didRegisterForRemoteNotificationsWithDeviceToken** metoda.

1. Hello následující metody musí již být v AppDelegate.m dokončení hello [Začínáme s Notification Hubs] [ get-started] kurzu.  Pokud ne, přidejte je.
   
    -(void) MessageBox:(NSString *) nadpis zpráva:(NSString *) messageText {
   
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    }
   
   * (void) aplikace:(UIApplication *) aplikace didReceiveRemoteNotification: (NSDictionary *) informací o uživateli {NSLog (@"% @", informací o uživateli);   [vlastní MessageBox:@"Notification" zprávy: [valueForKey:@"alert [informací o uživateli objectForKey:@"aps"]"]]; }
   
   Tato metoda zpracovává oznámení obdržená při spuštěné aplikaci hello zobrazením jednoduchou **UIAlert**.
2. V ViewController.m, přidejte příkaz import pro hello AppDelegate.h a zkopírujte následující kód do hello generované XCode **přihlášení k odběru** metoda. Tento kód se bude aktualizovat hello oznámení registrace toouse hello nové kategorie značky, které hello uživatelem vybrané v uživatelském rozhraní hello.
   
       ```
       #import "Notifications.h"
       ```
   
       NSMutableArray* categories = [[NSMutableArray alloc] init];
   
       if (self.WorldSwitch.isOn) [categories addObject:@"World"];
       if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
       if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
       if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
       if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
       if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];
   
       Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];
   
       [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
           if (!error) {
               [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
           } else {
               NSLog(@"Error subscribing: %@", error);
           }
       }];
   
   Tato metoda vytvoří **NSMutableArray** z kategorií a používá hello **oznámení** třída toostore hello seznamu v hello místní úložiště a zaregistruje hello odpovídající značky pomocí centra oznámení. Při změně kategorií, hello registrace se znovu vytvoří se nové kategorie hello.
3. V ViewController.m, přidejte následující kód v hello hello **viewDidLoad** metoda tooset hello uživatelské rozhraní založené na kategoriích hello uložili dřív.

        // This updates hello UI on startup based on hello status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



aplikace Hello můžete nyní uložit sadu kategorií v hello zařízení používá místní úložiště tooregister hello centra oznámení při každém spuštění aplikace hello.  Hello uživatele můžete změnit výběr hello kategorií za běhu a klikněte na tlačítko hello **přihlášení k odběru** metoda tooupdate hello registrace pro zařízení hello. Potom aktualizujte hello toosend aplikace hello nejnovější zprávy oznámení přímo ve hello aplikace.

## <a name="optional-sending-tagged-notifications"></a>(volitelné) Odesílání oznámení s příznakem
Pokud nemáte přístup tooVisual Studio, můžete přeskočit následující části toohello a odesílání oznámení z hello aplikace. Můžete také odeslat hello správné šablony oznámení z hello [portálu Azure Classic] pomocí karty hello ladění pro vaše Centrum oznámení. 

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-hello-device"></a>(volitelné) Odesílání oznámení z hello zařízení
Obvykle bude odesláno oznámení přes službu back-end, ale můžete odesílat oznámení o aktuálních zprávách přímo z aplikace hello. toodo to budeme aktualizovat hello `SendNotificationRESTAPI` metoda, která jsme definovali v hello [Začínáme s Notification Hubs] [ get-started] kurzu.

1. V aktualizaci hello ViewController.m `SendNotificationRESTAPI` jako metodu následuje tak, aby přijímá parametr hello kategorie značky a odešle hello správné [šablony](notification-hubs-templates-cross-platform-push-messages.md) oznámení.
   
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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];
   
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
2. V aktualizaci hello ViewController.m **odeslat oznámení** akce viz hello kód, který následuje. Tak, aby se odeslání oznámení hello pomocí jednotlivé značky jednotlivě a odeslat toomultiple platformy.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send hello message as breaking news for each category tooWNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



1. Znovu sestavte projekt a ujistěte se, že máte žádné chyby sestavení.

## <a name="run-hello-app-and-generate-notifications"></a>Spuštění aplikace hello a generovat oznámení
1. Stiskněte klávesu hello spusťte tlačítko toobuild hello projekt a spusťte aplikace hello. Vyberte některé nejnovější novinky možnosti toosubscribe tooand a potom stiskněte hello **přihlásit k odběru** tlačítko. Měli byste vidět dialogové okno oznamující hello oznámení přihlásili k odběru.
   
    ![][1]
   
    Pokud vyberete **přihlásit k odběru**, hello aplikace převede hello vybrané kategorie do značky a požaduje novou registraci zařízení pro hello vybrané značky z centra oznámení hello.
2. Zadejte zprávu toobe, odeslána jako novinek stiskněte hello **odeslat oznámení** tlačítko. Alternativně spusťte hello .NET konzole aplikace toogenerate oznámení.
   
    ![][2]
3. Každé zařízení odběru toobreaking zprávy obdrží hello oznámení o aktuálních zprávách, který jste právě zaslali.

## <a name="next-steps"></a>Další kroky
V tomto kurzu jsme se dozvěděli, jak toobroadcast nejnovější zprávy přes podle kategorie. Vezměte v úvahu dokončení jednu z následujících návodů, které zvýrazněte pokročilé scénáře Notification Hubs hello:

* **[Použití centra oznámení toobroadcast lokalizované novinek]**
  
    Zjistěte, jak lokalizované tooexpand hello nejnovější novinky aplikace tooenable odesílání oznámení.

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Použití centra oznámení toobroadcast lokalizované novinek]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[portálu Azure Classic]: https://manage.windowsazure.com
