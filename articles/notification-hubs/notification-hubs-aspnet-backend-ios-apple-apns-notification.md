---
title: "aaaAzure upozornění uživatelů centra oznámení pro iOS pomocí backendu .NET"
description: "Zjistěte, jak toosend nabízená oznámení toousers v Azure. Ukázky kódu jsou vytvořeny v Objective-C a hello .NET API pro back-end hello."
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 1f7d1410-ef93-4c4b-813b-f075eed20082
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 56aed5b04d2d985b3f0e50c58991607f07b61248
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-notify-users-for-ios-with-net-backend"></a>Upozornění uživatelů centra oznámení Azure pro iOS pomocí backendu .NET
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Přehled
Podpora nabízená oznámení v Azure můžete tooaccess snadné použití, multiplatform a nabízené škálovanou infrastrukturu, která výrazně zjednodušuje hello implementace nabízených oznámení spotřebních a podnikových aplikací pro mobilní platformy. Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa konkrétní aplikace uživatele na konkrétní zařízení. Backendu ASP.NET WebAPI je použité tooauthenticate klientů a toogenerate oznámení, jak je znázorněno v tématu pokyny hello [registrace z back-end aplikace](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).

> [!NOTE]
> V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md). V tomto kurzu je také požadovaných toohello hello [zabezpečení Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) kurzu.
> Pokud chcete toouse Mobile Apps jako back-end službu, najdete v části hello [mobilní aplikace začít pracovat s nabízené](../app-service-mobile/app-service-mobile-ios-get-started-push.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a>Upravit aplikaci s iOS
1. Otevřete hello jednostránkové zobrazit aplikaci jste vytvořili v hello [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) kurzu.
   
   > [!NOTE]
   > V této části předpokládáme, že váš projekt je nakonfigurovaný s názvem prázdné organizace. Pokud ne, je nutné tooprepend názvy tříd tooall název vaší organizace.
   > 
   > 
2. Ve vaší Main.storyboard přidáte součásti hello ukazuje snímek obrazovky hello níže z objektu knihovny hello.
   
    ![][1]
   
   * **Uživatelské jméno**: UITextField A s zástupný text *zadejte uživatelské jméno*, okamžitě pod hello výsledky štítek a omezené toohello left a pravým okraje a odesílají pod hello popisek výsledky.
   * **Heslo**: UITextField A s zástupný text *zadejte heslo*, okamžitě ybrat hello textové pole uživatelské jméno a omezené toohello doleva a doprava okraje a pod hello uživatelské jméno textové pole. Zkontrolujte hello **zabezpečení zadávání textu** možnost hello atribut Inspector, v části *vrátit klíč*.
   * **Přihlaste se**: A UIButton pod hello heslo textové pole s popiskem okamžitě a zrušte zaškrtnutí políčka hello **povoleno** možnost hello Inspector atributy, v části *obsahu ovládacího prvku*
   * **WNS**: štítek a odesílání tooenable přepínač hello oznámení služby oznámení Windows, pokud byla instalace v centru hello. V tématu hello [Windows Začínáme](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kurzu.
   * **GCM**: popisek a odesílání tooenable přepínač hello oznámení tooGoogle Cloud Messaging, pokud byla instalace v centru hello. V tématu [Začínáme Android](notification-hubs-android-push-notification-google-gcm-get-started.md) kurzu.
   * **APNS**: štítek a odesílání tooenable přepínač hello toohello oznámení Apple platformy Notification Service.
   * **Uživatelské jméno Recipent**: UITextField A s zástupný text *značky uživatelské jméno příjemce*bezprostředně pod hello GCM označte a marže omezené toohello vlevo a vpravo a pod hello GCM označovat.

    Některé součásti byly přidány v hello [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) kurzu.

1. **CTRL** přetažením z hello součásti v zobrazení tooViewController.h hello a přidejte tyto nové výstupy.
   
        @property (weak, nonatomic) IBOutlet UITextField *UsernameField;
        @property (weak, nonatomic) IBOutlet UITextField *PasswordField;
        @property (weak, nonatomic) IBOutlet UITextField *RecipientField;
        @property (weak, nonatomic) IBOutlet UITextField *NotificationField;
   
        // Used tooenable hello buttons on hello UI
        @property (weak, nonatomic) IBOutlet UIButton *LogInButton;
        @property (weak, nonatomic) IBOutlet UIButton *SendNotificationButton;
   
        // Used tooenabled sending notifications across platforms
        @property (weak, nonatomic) IBOutlet UISwitch *WNSSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *GCMSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *APNSSwitch;
   
        - (IBAction)LogInAction:(id)sender;
2. V ViewController.h, přidejte následující hello `#define` pod příkazy pro import. SUBSTITUTE hello *< Zadejte koncový bod vašeho back-end\>*  zástupný symbol hello cílová adresa URL, které jste toodeploy použili back-end aplikace v předchozí části hello. Například *http://you_backend.azurewebsites.net*.
   
        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
3. V projektu, vytvořte novou **Touch kakao třída** s názvem **RegisterClient** toointerface s hello ASP.NET back-end jste vytvořili. Vytvoření, která dědí z třídy hello `NSObject`. Přidejte následující kód v hello RegisterClient.h hello.
   
        @interface RegisterClient : NSObject
   
        @property (strong, nonatomic) NSString* authenticationHeader;
   
        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;
   
        -(instancetype) initWithEndpoint:(NSString*)Endpoint;
   
        @end
4. V hello RegisterClient.m aktualizace hello `@interface` části:
   
        @interface RegisterClient ()
   
        @property (strong, nonatomic) NSURLSession* session;
        @property (strong, nonatomic) NSURLSession* endpoint;
   
        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion;
        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion;
        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSString*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion;
   
        @end
5. Nahraďte hello `@implementation` část v hello RegisterClient.m s hello následující kód.

        @implementation RegisterClient

        // Globals used by RegisterClient
        NSString *const RegistrationIdLocalStorageKey = @"RegistrationId";

        -(instancetype) initWithEndpoint:(NSString*)Endpoint
        {
            self = [super init];
            if (self) {
                NSURLSessionConfiguration* config = [NSURLSessionConfiguration defaultSessionConfiguration];
                _session = [NSURLSession sessionWithConfiguration:config delegate:nil delegateQueue:nil];
                _endpoint = Endpoint;
            }
            return self;
        }

        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
                    andCompletion:(void(^)(NSError*))completion
        {
            [self tryToRegisterWithDeviceToken:token tags:tags retry:YES andCompletion:completion];
        }

        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion
        {
            NSSet* tagsSet = tags?tags:[[NSSet alloc] init];

            NSString *deviceTokenString = [[token description]
                stringByTrimmingCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"<>"]];
            deviceTokenString = [[deviceTokenString stringByReplacingOccurrencesOfString:@" " withString:@""]
                                    uppercaseString];

            [self retrieveOrRequestRegistrationIdWithDeviceToken: deviceTokenString
                completion:^(NSString* registrationId, NSError *error) {
                NSLog(@"regId: %@", registrationId);
                if (error) {
                    completion(error);
                    return;
                }

                [self upsertRegistrationWithRegistrationId:registrationId deviceToken:deviceTokenString
                    tags:tagsSet andCompletion:^(NSURLResponse * response, NSError *error) {
                    if (error) {
                        completion(error);
                        return;
                    }

                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if (httpResponse.statusCode == 200) {
                        completion(nil);
                    } else if (httpResponse.statusCode == 410 && retry) {
                        [self tryToRegisterWithDeviceToken:token tags:tags retry:NO andCompletion:completion];
                    } else {
                        NSLog(@"Registration error with response status: %ld", (long)httpResponse.statusCode);

                        completion([NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }

                }];
            }];
        }

        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSData*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion
        {
            NSDictionary* deviceRegistration = @{@"Platform" : @"apns", @"Handle": token,
                                                    @"Tags": [tags allObjects]};
            NSData* jsonData = [NSJSONSerialization dataWithJSONObject:deviceRegistration
                                options:NSJSONWritingPrettyPrinted error:nil];

            NSLog(@"JSON registration: %@", [[NSString alloc] initWithData:jsonData
                                                encoding:NSUTF8StringEncoding]);

            NSString* endpoint = [NSString stringWithFormat:@"%@/api/register/%@", _endpoint,
                                    registrationId];
            NSURL* requestURL = [NSURL URLWithString:endpoint];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"PUT"];
            [request setHTTPBody:jsonData];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
            [request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                if (!error)
                {
                    completion(response, error);
                }
                else
                {
                    NSLog(@"Error request: %@", error);
                    completion(nil, error);
                }
            }];
            [dataTask resume];
        }

        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion
        {
            NSString* registrationId = [[NSUserDefaults standardUserDefaults]
                                        objectForKey:RegistrationIdLocalStorageKey];

            if (registrationId)
            {
                completion(registrationId, nil);
                return;
            }

            // request new one & save
            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/api/register?handle=%@",
                                    _endpoint, token]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"POST"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSString* registrationId = [[NSString alloc] initWithData:data
                        encoding:NSUTF8StringEncoding];

                    // remove quotes
                    registrationId = [registrationId substringWithRange:NSMakeRange(1,
                                        [registrationId length]-2)];

                    [[NSUserDefaults standardUserDefaults] setObject:registrationId
                        forKey:RegistrationIdLocalStorageKey];
                    [[NSUserDefaults standardUserDefaults] synchronize];

                    completion(registrationId, nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        @end

    výše uvedený kód Hello implementuje logiku hello popsané v článku pokyny hello [registrace z back-end aplikace](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) pomocí NSURLSession tooperform REST volá, back-end aplikace tooyour a NSUserDefaults toolocally úložiště hello registrationId vrácený hello centra oznámení.

    Všimněte si, že tato třída vyžaduje, aby jeho vlastnost **authorizationHeader** toobe nastavte v pořadí toowork správně. Tato vlastnost je nastavena podle hello **ViewController** třídy po přihlášení hello.

1. V ViewController.h, přidejte `#import` příkaz pro RegisterClient.h. Pak přidejte deklaraci pro token zařízení hello a odkazovat na tooa `RegisterClient` instance v hello `@interface` části:
   
        #import "RegisterClient.h"
   
        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;
2. V ViewController.m, přidejte deklaraci soukromá metoda v hello `@interface` části:
   
        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>
   
        // create hello Authorization header tooperform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
   
        @end

> [!NOTE]
> Hello následující fragment kódu není schéma zabezpečeného ověřování, doporučujeme nahradit hello implementace hello **createAndSetAuthenticationHeaderWithUsername:AndPassword:** s konkrétní ověřovací mechanismus k ověření tokenu toobe spotřebovávají hello registrace klienta třídu, například OAuth, služby Active Directory, která generuje.
> 
> 

1. Potom v hello `@implementation` části ViewController.m přidejte následující kód, který přidá hello implementace nastavení hello zařízení tokenu a ověření hlavičky hello.
   
        -(void) setDeviceToken: (NSData*) deviceToken
        {
            _deviceToken = deviceToken;
            self.LogInButton.enabled = YES;
        }
   
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
        {
            NSString* headerValue = [NSString stringWithFormat:@"%@:%@", username, password];
   
            NSData* encodedData = [[headerValue dataUsingEncoding:NSUTF8StringEncoding] base64EncodedDataWithOptions:NSDataBase64EncodingEndLineWithCarriageReturn];
   
            self.registerClient.authenticationHeader = [[NSString alloc] initWithData:encodedData
                                                        encoding:NSUTF8StringEncoding];
        }
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
   
    Všimněte si, jak token zařízení hello nastavení umožňuje hello tlačítko přihlásit. Je to proto, že jako součást hello přihlášení akce, řadiče zobrazení hello zaregistruje pro nabízená oznámení s back-end aplikace hello. Proto jsme nechcete, aby přihlášení toobe akce dostupný dokud hello token zařízení správně nastavit. Hello přihlášení z registrace nabízených hello můžete oddělit tak dlouho, dokud se stane hello dřívějším před pozdější hello.
2. V ViewController.m, použijte následující fragmenty tooimplement hello metoda akce pro hello vaše **protokolu v** tlačítko a metoda toosend hello oznámení zprávu pomocí hello ASP.NET back-end.
   
       - (IBAction) LogInAction: odesílatele (id) {/ / vytvoření záhlaví ověření a nastavte ji zaregistrovat klienta NSString * uživatelské jméno = sám sebou. UsernameField.text;   Heslo NSString * = sám sebou. PasswordField.text;
   
           [vlastní createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];
   
           __weak ViewController * selfie = vlastní;   [self.registerClient registerWithDeviceToken:self.deviceToken značky: nil andCompletion:^(NSError* error) {Pokud (! chyby) {dispatch_async(dispatch_get_main_queue(), ^ {selfie. SendNotificationButton.enabled = YES;               [samoobslužné MessageBox:@"Success" message:@"Registered úspěšně!"];});}}];}

        -SendNotificationASPNETBackend (void): (NSString*) pns UsernameTag: (NSString*) usernameTag zpráva: (NSString*) zpráva {NSURLSession* relace = [NSURLSession sessionWithConfiguration: delegateQueue:nil delegáta: nil [NSURLSessionConfiguration defaultSessionConfiguration]];

            Předat značky hello systém pns a uživatelské jméno jako parametry s hello REST URL toohello ASP.NET back-end nsurl, který * requestURL = [nsurl, který URLWithString: [NSString stringWithFormat:@"%@/api/notifications? systém pns = % @& to_tag = % @", BACKEND_ENDPOINT, systém pns, usernameTag]];

            Žádost o NSMutableURLRequest * = [NSMutableURLRequest requestWithURL:requestURL];    [požadavků setHTTPMethod:@"POST"];

            Získat imitované authenticationheader hello od klienta registrace hello NSString * authorizationHeaderValue = [NSString stringWithFormat:@"Basic % @", self.registerClient.authenticationHeader];    [požadavků setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            Přidat text zprávy oznámení hello [požadavku setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];    [požadavku setHTTPBody: [zpráva dataUsingEncoding:NSUTF8StringEncoding]];

            Spustit hello odesílání oznámení REST API na hello ASP.NET back-end NSURLSessionDataTask * dataTask = [relace dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError  *Chyba) {NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) odpovědi;        Pokud (Chyba || httpResponse.statusCode! = 200) {NSString* stav = [NSString stringWithFormat:@"Error stav pro % @: % d\nError: %@\n", systém pns, httpResponse.statusCode, chyba];            dispatch_async(dispatch_get_main_queue(), ^ {/ / Append textové protože všechna volání 3 systém PNS mohou mít i tooview informace [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];    [obnovit dataTask]; }


1. Aktualizovat hello akci pro hello **odeslat oznámení** tlačítko toouse hello ASP.NET back-end a odeslat tooany systém PNS povoleno přepínač.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            //[self SendNotificationRESTAPI];
            [self SendToEnabledPlatforms];
        }


        -(void)SendToEnabledPlatforms
        {
            NSString* json = [NSString stringWithFormat:@"\"%@\"",self.notificationMessage.text];

            [self.sendResults setText:@""];

            if ([self.WNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"wns" UsernameTag:self.RecipientField.text Message:json];

            if ([self.GCMSwitch isOn])
                [self SendNotificationASPNETBackend:@"gcm" UsernameTag:self.RecipientField.text Message:json];

            if ([self.APNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"apns" UsernameTag:self.RecipientField.text Message:json];
        }



1. Ve funkci **ViewDidLoad**, přidejte následující tooinstantiate hello RegisterClient instance hello a nastavte hello delegáta u textových polí.
   
       self.UsernameField.delegate = self;
       self.PasswordField.delegate = self;
       self.RecipientField.delegate = self;
       self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
2. Nyní ve **AppDelegate.m**, odeberte všechny obsah hello hello metody **aplikace: didRegisterForPushNotificationWithDeviceToken:** a nahraďte ji následujícím toomake opravdu hello, který hello zobrazení řadič obsahuje nejnovější token zařízení hello načíst ze služby APN:
   
       // Add import toohello top of hello file
       #import "ViewController.h"
   
       - (void)application:(UIApplication *)application
                   didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
       {
           ViewController* rvc = (ViewController*) self.window.rootViewController;
           rvc.deviceToken = deviceToken;
       }
3. Nakonec v **AppDelegate.m**, zajistěte, aby byla hello následující metodu:
   
       - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
           NSLog(@"%@", userInfo);
           [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
       }

## <a name="test-hello-application"></a>Test hello aplikace
1. V XCode spusťte aplikaci hello na fyzickém zařízení iOS (nabízených oznámení nebude fungovat v simulátoru hello).
2. V aplikaci pro iOS hello uživatelského rozhraní zadejte uživatelské jméno a heslo. Mohou to být libovolný řetězec, ale musí být obě hello stejná hodnota typu řetězec. Pak klikněte na tlačítko **protokolu v**.
   
    ![][2]
3. Měli byste vidět vyskakovací okno s informacemi o registraci úspěch. Klikněte na **OK**.
   
    ![][3]
4. V hello **značky uživatelské jméno příjemce* textové pole, zadejte značky jméno uživatele hello používá s registrací hello z jiného zařízení.
5. Zadejte zprávu oznámení a klikněte na tlačítko **odeslat oznámení**.  Hello oznámení zpráva se zobrazí pouze hello zařízení, která mají registrace s hello příjemce uživatele název značky.  Pouze odesláním toothose uživatele.
   
    ![][4]

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
