---
title: "aaaAzure Push bohaté centra oznámení"
description: "Zjistěte, jak toosend bohaté nabízená oznámení aplikace iOS tooan z Azure. Ukázky kódu jsou vytvořeny v Objective-C a C#."
documentationcenter: ios
services: notification-hubs
author: ysxu
manager: erikre
editor: 
ms.assetid: 590304df-c0a4-46c5-8ef5-6a6486bb3340
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 5432d8bf47777371bea3521a0c0176ade75fbd9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-rich-push"></a>Azure Notification Hubs bohaté Push
## <a name="overview"></a>Přehled
V pořadí tooengage uživatelům s rychlých bohaté obsah může být vhodné aplikace toopush nad rámec prostý text. Tato oznámení zvýšit úroveň, akce uživatelů a existuje obsah, jako jsou adresy URL, zvuky, Image nebo kupóny a další. V tomto kurzu vychází hello [upozornění uživatelů](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tématu a ukazuje, jak toosend nabízená oznámení, které jsou částí (například obrázek).

V tomto kurzu je kompatibilní s iOS 7 a 8.

  ![][IOS1]

Na vysoké úrovni:

1. back-end aplikace Hello:
   * Úložiště hello bohaté datová část (v tomto případě obrázek) v úložišti databáze nebo místní hello back-end
   * Odešle ID tohoto zařízení toohello bohaté oznámení
2. Aplikace na zařízení hello:
   * Kontakty hello požaduje hello bohaté datovou část s ID hello obdrží back-end
   * Při načítání dat je dokončena a ukazuje datovou část hello okamžitě, když uživatel klepnutím na další toolearn odešle oznámení uživatelům na zařízení hello

## <a name="webapi-project"></a>WebAPI projektu
1. V sadě Visual Studio otevřete hello **AppBackend** projekt, který jste vytvořili v hello [upozornění uživatelů](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kurzu.
2. Získat bitovou kopii chcete vytvořit toonotify uživatelů s a vložte ho **img** složky v adresáři projektu.
3. Klikněte na tlačítko **zobrazit všechny soubory** v hello Průzkumníku řešení a klikněte pravým tlačítkem na složku hello příliš**zahrnout do projektu**.
4. Vybraná Image hello, změnit jeho akce sestavení v okně Vlastnosti příliš**vložený prostředek**.
   
    ![][IOS2]
5. V **Notifications.cs**, přidejte hello následující příkaz using:
   
        using System.Reflection;
6. Aktualizace hello celou **oznámení** se hello následující kód. Být jisti tooreplace zástupné symboly hello s oznámení centra pověření a název souboru obrázku.
   
        public class Notification {
            public int Id { get; set; }
            // Initial notification message toodisplay toousers
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }
   
        public class Notifications {
            public static Notifications Instance = new Notifications();
   
            private List<Notification> notifications = new List<Notification>();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                // Placeholders: replace with hello connection string (with full access) for your notification hub and hello hub name from hello Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }
   
            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };
   
                notifications.Add(notification);
   
                return notification;
            }
   
            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }
   
   > [!NOTE]
   > (volitelné) Odkazovat příliš[jak tooembed a přístup k prostředkům pomocí Visual C#](http://support.microsoft.com/kb/319292) Další informace o tom, tooadd a získat prostředky projektu.
   > 
   > 
7. V **NotificationsController.cs**, znovu definovat **NotificationsController** s hello následující fragmenty kódu. To odešle toodevice id počáteční tichou bohaté oznámení a umožňuje klienta načtení obrázku:
   
        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);
   
            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");
   
            return result;
        }
   
        // Create rich notification and send initial silent notification (containing id) tooclient
        public async Task<HttpResponseMessage> Post() {
            // Replace hello placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");
   
            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";
   
            // Send notification tooapns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
8. Nyní nasadíme znovu tuto aplikaci tooan webu Azure v pořadí toomake je přístupná ze všech zařízení. Klikněte pravým tlačítkem na hello **AppBackend** projektu a vyberte **publikovat**.
9. Vyberte web Azure jako váš cíl publikování. Přihlaste se pomocí účtu Azure a vyberte stávajícího nebo nového webu a poznamenejte si hello **cílová adresa URL** vlastnost hello **připojení** kartě. Označujeme toothis adresu URL jako vaše *koncový bod back-end* dál v tomto kurzu. Klikněte na **Publikovat**.

## <a name="modify-hello-ios-project"></a>Upravit projektu iOS hello
Teď, když jste změnili jenom hello vaší aplikace back-end toosend *id* oznámení, změníte toto id vaší toohandle aplikace iOS a načíst hello bohaté zprávy z vaší back-end.

1. Otevřete projekt iOS a povolte vzdálené oznámení tak, že budete cíl tooyour hlavní aplikace v hello **cíle** části.
2. Klikněte na **možnosti**, zapnout **režimy pozadí**a zkontrolujte hello **vzdáleného oznámení** zaškrtávací políčko.
   
    ![][IOS3]
3. Přejděte příliš**Main.storyboard**a zajistěte, aby byla řadič zobrazení (tooas zmíněných Domů řadiče zobrazení v tomto kurzu) z [upozornit uživatele](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kurzu.
4. Přidat **navigační řadič** tooyour scénáře a Ctrl táhnout toomake View Controller tooHome ho hello **kořenový zobrazení** navigace. Ujistěte se, zda text hello **je počáteční View Controller** v atributech inspector je vybraná jenom hello navigační řadiče.
5. Přidat **View Controller** toostoryboard a přidejte **Image zobrazení**. Toto je stránka hello, se zobrazí uživatelům, jakmile toolearn vybírá informace kliknutím na hello notifiication. Vaše scénáře by měla vypadat takto:
   
    ![][IOS4]
6. Klikněte na hello **Domů View Controller** scénáře a ujistěte se, že má **homeViewController** jako jeho **vlastní třída** a **Storyboard ID**pod hello Identity inspector.
7. Hello stejné pro bitovou kopii řadiče zobrazení jako **imageViewController**.
8. Pak vytvořte novou třídu řadiče zobrazení s názvem **imageViewController** toohandle hello uživatelského rozhraní, kterou jste právě vytvořili.
9. V **imageViewController.h**, přidejte následující deklarace rozhraní toohello řadiče hello. Ujistěte se, že toocontrol přetažení z hello storyboard image zobrazení toothese vlastnosti toolink hello dva:
   
        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;
10. V **imageViewController.m**, přidejte následující hello na konci hello **viewDidload**:
    
        // Display hello UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];
11. V **AppDelegate.m**, import hello image řadiče jste vytvořili:
    
        #import "imageViewController.h"
12. Přidáte oddíl, který rozhraní s hello následující prohlášení:
    
        @interface AppDelegate ()
    
        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;
    
        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;
    
        // Redirect tooImage View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;
    
        @end
13. V **AppDelegate**, ujistěte se, že vaše aplikace se zaregistruje pro tichou oznámení v **aplikace: didFinishLaunchingWithOptions**:
    
        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];
    
        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");
    
            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";
    
            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];
    
            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];

            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

1. Nahraďte v hello následující implementace pro **aplikace: didRegisterForRemoteNotificationsWithDeviceToken** tootake hello storyboard uživatelského rozhraní se změní v úvahu:
   
       // Access navigation controller which is at hello root of window
       UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
       // Get home view controller from stack on navigation controller
       homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
       hvc.deviceToken = deviceToken;
2. Pak přidejte následující metody příliš hello**AppDelegate.m** tooretrieve hello bitovou kopii z váš koncový bod a odesílání místní oznámení po dokončení načítání. Ujistěte se, že zástupný symbol hello toosubstitute `{backend endpoint}` s back-end koncový bod:
   
       NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";
   
       // Helper: retrieve notification content from backend with rich notification id
       - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
           UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
           homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
           NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
           // Check if authenticated
           if (!authenticationHeader) return;
   
           NSURLSession* session = [NSURLSession
                                    sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                    delegate:nil
                                    delegateQueue:nil];
   
           NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
           NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
           [request setHTTPMethod:@"GET"];
           NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
           [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
   
           NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
   
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
               if (!error && httpResponse.statusCode == 200) {
                   // From NSData tooUIImage
                   self.imagePayload = [UIImage imageWithData:data];
   
                   completion(nil);
               }
               else {
                   NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                   if (error)
                       completion(error);
                   else {
                       completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                   }
               }
           }];
           [dataTask resume];
       }
   
       // Handle silent push notifications when id is sent from backend
       - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
           self.userInfo = userInfo;
           int richId = [[self.userInfo objectForKey:@"richId"] intValue];
           NSString* richType = [self.userInfo objectForKey:@"richType"];
   
           // Retrieve image data
           if ([richType isEqualToString:@"img"]) {  
               [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                   if (!error){
                       // Send local notification
                       UILocalNotification* localNotification = [[UILocalNotification alloc] init];
   
                       // "5" is arbitrary here toogive you enough time tooquit out of hello app and receive push notifications
                       localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                       localNotification.userInfo = self.userInfo;
                       localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                       localNotification.timeZone = [NSTimeZone defaultTimeZone];
   
                       // iOS8 categories
                       if (self.iOS8) {
                           localNotification.category = @"richPush";
                       }
   
                       [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];
   
                       handler(UIBackgroundFetchResultNewData);
                   }
                   else{
                       handler(UIBackgroundFetchResultFailed);
                   }
               }];
           }
           // Add "else if" here toohandle more types of rich content such as url, sound files, etc.
       }
3. Zpracování hello místního oznámení nad otevřením řadiče zobrazení hello bitové kopie v **AppDelegate.m** s hello následující metody:
   
       // Helper: redirect users tooimage view controller
       - (void)redirectToImageViewWithImage: (UIImage *)img {
           UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
           UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                    bundle: nil];
           imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
           // Pass data/image tooimage view controller
           imgViewController.imagePayload = img;
   
           // Redirect
           [navigationController pushViewController:imgViewController animated:YES];
       }
   
       // Handle local notification sent above in didReceiveRemoteNotification
       - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
           if (application.applicationState == UIApplicationStateActive) {
               // Show in-app alert with an extra "more" button
               UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];
   
               [alert show];
           }
           // App becomes active from user's tap on notification
           else {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
       }
   
       // Handle buttons in in-app alerts and redirect with data/image
       - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
           // Handle "more" button
           if (buttonIndex == 1)
           {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
           // Add "else if" here toohandle more buttons
       }
   
       // Handle notification setting actions in iOS8
       - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
           // Handle richPush related buttons
           if ([identifier isEqualToString:@"richPushMore"]) {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
           completionHandler();
       }

## <a name="run-hello-application"></a>Spustit hello aplikace
1. V XCode spusťte aplikaci hello na fyzickém zařízení iOS (nabízených oznámení nebude fungovat v simulátoru hello).
2. V aplikaci pro iOS hello uživatelského rozhraní, zadejte uživatelské jméno a heslo hello stejné hodnoty pro ověřování a klikněte na tlačítko **protokolu v**.
3. Klikněte na tlačítko **odeslat nabízené** a měli byste vidět výstrahu v aplikaci. Pokud kliknete na **Další**, bude možné přepnout do toohello image, které jste zvolili tooinclude v back-end aplikace.
4. Můžete také kliknout na **odeslat nabízené** a okamžitě stiskněte tlačítko Domů hello vašeho zařízení. Ve chvíli obdržíte nabízená oznámení. Klepněte na něm nebo klikněte na tlačítko Další, bude začlenění obsahu tooyour aplikace a hello bohaté bitové kopie.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
