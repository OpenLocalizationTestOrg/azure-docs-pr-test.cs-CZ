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
# <a name="azure-notification-hubs-notify-users-for-ios-with-net-backend"></a><span data-ttu-id="27f3f-104">Upozornění uživatelů centra oznámení Azure pro iOS pomocí backendu .NET</span><span class="sxs-lookup"><span data-stu-id="27f3f-104">Azure Notification Hubs Notify Users for iOS with .NET backend</span></span>
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a><span data-ttu-id="27f3f-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="27f3f-105">Overview</span></span>
<span data-ttu-id="27f3f-106">Podpora nabízená oznámení v Azure můžete tooaccess snadné použití, multiplatform a nabízené škálovanou infrastrukturu, která výrazně zjednodušuje hello implementace nabízených oznámení spotřebních a podnikových aplikací pro mobilní platformy.</span><span class="sxs-lookup"><span data-stu-id="27f3f-106">Push notification support in Azure enables you tooaccess an easy-to-use, multiplatform, and scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span> <span data-ttu-id="27f3f-107">Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooa konkrétní aplikace uživatele na konkrétní zařízení.</span><span class="sxs-lookup"><span data-stu-id="27f3f-107">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa specific app user on a specific device.</span></span> <span data-ttu-id="27f3f-108">Backendu ASP.NET WebAPI je použité tooauthenticate klientů a toogenerate oznámení, jak je znázorněno v tématu pokyny hello [registrace z back-end aplikace](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span><span class="sxs-lookup"><span data-stu-id="27f3f-108">An ASP.NET WebAPI backend is used tooauthenticate clients and toogenerate notifications, as shown in hello guidance topic [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span></span>

> [!NOTE]
> <span data-ttu-id="27f3f-109">V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="27f3f-109">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span></span> <span data-ttu-id="27f3f-110">V tomto kurzu je také požadovaných toohello hello [zabezpečení Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="27f3f-110">This tutorial is also hello prerequisite toohello [Secure Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) tutorial.</span></span>
> <span data-ttu-id="27f3f-111">Pokud chcete toouse Mobile Apps jako back-end službu, najdete v části hello [mobilní aplikace začít pracovat s nabízené](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="27f3f-111">If you want toouse Mobile Apps as your backend service, see hello [Mobile Apps Get Started with Push](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a><span data-ttu-id="27f3f-112">Upravit aplikaci s iOS</span><span class="sxs-lookup"><span data-stu-id="27f3f-112">Modify your iOS app</span></span>
1. <span data-ttu-id="27f3f-113">Otevřete hello jednostránkové zobrazit aplikaci jste vytvořili v hello [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="27f3f-113">Open hello Single Page view app you created in hello [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) tutorial.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="27f3f-114">V této části předpokládáme, že váš projekt je nakonfigurovaný s názvem prázdné organizace.</span><span class="sxs-lookup"><span data-stu-id="27f3f-114">In this section we assume that your project is configured with an empty organization name.</span></span> <span data-ttu-id="27f3f-115">Pokud ne, je nutné tooprepend názvy tříd tooall název vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="27f3f-115">If not, you will need tooprepend your organization name tooall class names.</span></span>
   > 
   > 
2. <span data-ttu-id="27f3f-116">Ve vaší Main.storyboard přidáte součásti hello ukazuje snímek obrazovky hello níže z objektu knihovny hello.</span><span class="sxs-lookup"><span data-stu-id="27f3f-116">In your Main.storyboard add hello components shown in hello screenshot below from hello object library.</span></span>
   
    ![][1]
   
   * <span data-ttu-id="27f3f-117">**Uživatelské jméno**: UITextField A s zástupný text *zadejte uživatelské jméno*, okamžitě pod hello výsledky štítek a omezené toohello left a pravým okraje a odesílají pod hello popisek výsledky.</span><span class="sxs-lookup"><span data-stu-id="27f3f-117">**Username**: A UITextField with placeholder text, *Enter Username*, immediately beneath hello send results label and constrained toohello left and right margins and beneath hello send results label.</span></span>
   * <span data-ttu-id="27f3f-118">**Heslo**: UITextField A s zástupný text *zadejte heslo*, okamžitě ybrat hello textové pole uživatelské jméno a omezené toohello doleva a doprava okraje a pod hello uživatelské jméno textové pole.</span><span class="sxs-lookup"><span data-stu-id="27f3f-118">**Password**: A UITextField with placeholder text, *Enter Password*, immediately beneath hello username text field and constrained toohello left and right margins and beneath hello username text field.</span></span> <span data-ttu-id="27f3f-119">Zkontrolujte hello **zabezpečení zadávání textu** možnost hello atribut Inspector, v části *vrátit klíč*.</span><span class="sxs-lookup"><span data-stu-id="27f3f-119">Check hello **Secure Text Entry** option in hello Attribute Inspector, under *Return Key*.</span></span>
   * <span data-ttu-id="27f3f-120">**Přihlaste se**: A UIButton pod hello heslo textové pole s popiskem okamžitě a zrušte zaškrtnutí políčka hello **povoleno** možnost hello Inspector atributy, v části *obsahu ovládacího prvku*</span><span class="sxs-lookup"><span data-stu-id="27f3f-120">**Log in**: A UIButton labeled immediately beneath hello password text field and uncheck hello **Enabled** option in hello Attributes Inspector, under *Control-Content*</span></span>
   * <span data-ttu-id="27f3f-121">**WNS**: štítek a odesílání tooenable přepínač hello oznámení služby oznámení Windows, pokud byla instalace v centru hello.</span><span class="sxs-lookup"><span data-stu-id="27f3f-121">**WNS**: Label and switch tooenable sending hello notification Windows Notification Service if it has been setup on hello hub.</span></span> <span data-ttu-id="27f3f-122">V tématu hello [Windows Začínáme](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="27f3f-122">See hello [Windows Getting Started](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) tutorial.</span></span>
   * <span data-ttu-id="27f3f-123">**GCM**: popisek a odesílání tooenable přepínač hello oznámení tooGoogle Cloud Messaging, pokud byla instalace v centru hello.</span><span class="sxs-lookup"><span data-stu-id="27f3f-123">**GCM**: Label and switch tooenable sending hello notification tooGoogle Cloud Messaging if it has been setup on hello hub.</span></span> <span data-ttu-id="27f3f-124">V tématu [Začínáme Android](notification-hubs-android-push-notification-google-gcm-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="27f3f-124">See [Android Getting Started](notification-hubs-android-push-notification-google-gcm-get-started.md) tutorial.</span></span>
   * <span data-ttu-id="27f3f-125">**APNS**: štítek a odesílání tooenable přepínač hello toohello oznámení Apple platformy Notification Service.</span><span class="sxs-lookup"><span data-stu-id="27f3f-125">**APNS**: Label and switch tooenable sending hello notification toohello Apple Platform Notification Service.</span></span>
   * <span data-ttu-id="27f3f-126">**Uživatelské jméno Recipent**: UITextField A s zástupný text *značky uživatelské jméno příjemce*bezprostředně pod hello GCM označte a marže omezené toohello vlevo a vpravo a pod hello GCM označovat.</span><span class="sxs-lookup"><span data-stu-id="27f3f-126">**Recipent Username**:A UITextField with placeholder text, *Recipient username tag*, immediately beneath hello GCM label and constrained toohello left and right margins and beneath hello GCM label.</span></span>

    <span data-ttu-id="27f3f-127">Některé součásti byly přidány v hello [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="27f3f-127">Some components were added in hello [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) tutorial.</span></span>

1. <span data-ttu-id="27f3f-128">**CTRL** přetažením z hello součásti v zobrazení tooViewController.h hello a přidejte tyto nové výstupy.</span><span class="sxs-lookup"><span data-stu-id="27f3f-128">**Ctrl** drag from hello components in hello view tooViewController.h and add these new outlets.</span></span>
   
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
2. <span data-ttu-id="27f3f-129">V ViewController.h, přidejte následující hello `#define` pod příkazy pro import.</span><span class="sxs-lookup"><span data-stu-id="27f3f-129">In ViewController.h, add hello following `#define` just below your import statements.</span></span> <span data-ttu-id="27f3f-130">SUBSTITUTE hello *< Zadejte koncový bod vašeho back-end\>*  zástupný symbol hello cílová adresa URL, které jste toodeploy použili back-end aplikace v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="27f3f-130">Substitute hello *<Enter Your Backend Endpoint\>* placeholder with hello Destination URL you used toodeploy your app backend in hello previous section.</span></span> <span data-ttu-id="27f3f-131">Například *http://you_backend.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="27f3f-131">For example, *http://you_backend.azurewebsites.net*.</span></span>
   
        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
3. <span data-ttu-id="27f3f-132">V projektu, vytvořte novou **Touch kakao třída** s názvem **RegisterClient** toointerface s hello ASP.NET back-end jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="27f3f-132">In your project, create a new **Cocoa Touch class** named **RegisterClient** toointerface with hello ASP.NET back-end you created.</span></span> <span data-ttu-id="27f3f-133">Vytvoření, která dědí z třídy hello `NSObject`.</span><span class="sxs-lookup"><span data-stu-id="27f3f-133">Create hello class inheriting from `NSObject`.</span></span> <span data-ttu-id="27f3f-134">Přidejte následující kód v hello RegisterClient.h hello.</span><span class="sxs-lookup"><span data-stu-id="27f3f-134">Then add hello following code in hello RegisterClient.h.</span></span>
   
        @interface RegisterClient : NSObject
   
        @property (strong, nonatomic) NSString* authenticationHeader;
   
        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;
   
        -(instancetype) initWithEndpoint:(NSString*)Endpoint;
   
        @end
4. <span data-ttu-id="27f3f-135">V hello RegisterClient.m aktualizace hello `@interface` části:</span><span class="sxs-lookup"><span data-stu-id="27f3f-135">In hello RegisterClient.m update hello `@interface` section:</span></span>
   
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
5. <span data-ttu-id="27f3f-136">Nahraďte hello `@implementation` část v hello RegisterClient.m s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="27f3f-136">Replace hello `@implementation` section in hello RegisterClient.m with hello following code.</span></span>

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

    <span data-ttu-id="27f3f-137">výše uvedený kód Hello implementuje logiku hello popsané v článku pokyny hello [registrace z back-end aplikace](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) pomocí NSURLSession tooperform REST volá, back-end aplikace tooyour a NSUserDefaults toolocally úložiště hello registrationId vrácený hello centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="27f3f-137">hello code above implements hello logic explained in hello guidance article [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) using NSURLSession tooperform REST calls tooyour app backend, and NSUserDefaults toolocally store hello registrationId returned by hello notification hub.</span></span>

    <span data-ttu-id="27f3f-138">Všimněte si, že tato třída vyžaduje, aby jeho vlastnost **authorizationHeader** toobe nastavte v pořadí toowork správně.</span><span class="sxs-lookup"><span data-stu-id="27f3f-138">Note that this class requires its property **authorizationHeader** toobe set in order toowork properly.</span></span> <span data-ttu-id="27f3f-139">Tato vlastnost je nastavena podle hello **ViewController** třídy po přihlášení hello.</span><span class="sxs-lookup"><span data-stu-id="27f3f-139">This property is set by hello **ViewController** class after hello log in.</span></span>

1. <span data-ttu-id="27f3f-140">V ViewController.h, přidejte `#import` příkaz pro RegisterClient.h.</span><span class="sxs-lookup"><span data-stu-id="27f3f-140">In ViewController.h, add a `#import` statement for RegisterClient.h.</span></span> <span data-ttu-id="27f3f-141">Pak přidejte deklaraci pro token zařízení hello a odkazovat na tooa `RegisterClient` instance v hello `@interface` části:</span><span class="sxs-lookup"><span data-stu-id="27f3f-141">Then add a declaration for hello device token and reference tooa `RegisterClient` instance in hello `@interface` section:</span></span>
   
        #import "RegisterClient.h"
   
        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;
2. <span data-ttu-id="27f3f-142">V ViewController.m, přidejte deklaraci soukromá metoda v hello `@interface` části:</span><span class="sxs-lookup"><span data-stu-id="27f3f-142">In ViewController.m, add a private method declaration in hello `@interface` section:</span></span>
   
        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>
   
        // create hello Authorization header tooperform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
   
        @end

> [!NOTE]
> <span data-ttu-id="27f3f-143">Hello následující fragment kódu není schéma zabezpečeného ověřování, doporučujeme nahradit hello implementace hello **createAndSetAuthenticationHeaderWithUsername:AndPassword:** s konkrétní ověřovací mechanismus k ověření tokenu toobe spotřebovávají hello registrace klienta třídu, například OAuth, služby Active Directory, která generuje.</span><span class="sxs-lookup"><span data-stu-id="27f3f-143">hello following snippet is not a secure authentication scheme, you should substitute hello implementation of hello **createAndSetAuthenticationHeaderWithUsername:AndPassword:** with your specific authentication mechanism that generates an authentication token toobe consumed by hello register client class, e.g. OAuth, Active Directory.</span></span>
> 
> 

1. <span data-ttu-id="27f3f-144">Potom v hello `@implementation` části ViewController.m přidejte následující kód, který přidá hello implementace nastavení hello zařízení tokenu a ověření hlavičky hello.</span><span class="sxs-lookup"><span data-stu-id="27f3f-144">Then in hello `@implementation` section of ViewController.m add hello following code which adds hello implementation for setting hello device token and authentication header.</span></span>
   
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
   
    <span data-ttu-id="27f3f-145">Všimněte si, jak token zařízení hello nastavení umožňuje hello tlačítko přihlásit.</span><span class="sxs-lookup"><span data-stu-id="27f3f-145">Note how setting hello device token enables hello log in button.</span></span> <span data-ttu-id="27f3f-146">Je to proto, že jako součást hello přihlášení akce, řadiče zobrazení hello zaregistruje pro nabízená oznámení s back-end aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="27f3f-146">This is becasue as a part of hello login action, hello view controller registers for push notifications with hello app backend.</span></span> <span data-ttu-id="27f3f-147">Proto jsme nechcete, aby přihlášení toobe akce dostupný dokud hello token zařízení správně nastavit.</span><span class="sxs-lookup"><span data-stu-id="27f3f-147">Hence, we do not want Log In action toobe accessible till hello device token has been properly set up.</span></span> <span data-ttu-id="27f3f-148">Hello přihlášení z registrace nabízených hello můžete oddělit tak dlouho, dokud se stane hello dřívějším před pozdější hello.</span><span class="sxs-lookup"><span data-stu-id="27f3f-148">You can decouple hello log in from hello push registration as long as hello former happens before hello latter.</span></span>
2. <span data-ttu-id="27f3f-149">V ViewController.m, použijte následující fragmenty tooimplement hello metoda akce pro hello vaše **protokolu v** tlačítko a metoda toosend hello oznámení zprávu pomocí hello ASP.NET back-end.</span><span class="sxs-lookup"><span data-stu-id="27f3f-149">In ViewController.m, use hello following snippets tooimplement hello action method for your **Log In** button and a method toosend hello notification message using hello ASP.NET backend.</span></span>
   
       - <span data-ttu-id="27f3f-150">(IBAction) LogInAction: odesílatele (id) {/ / vytvoření záhlaví ověření a nastavte ji zaregistrovat klienta NSString * uživatelské jméno = sám sebou. UsernameField.text;   Heslo NSString * = sám sebou. PasswordField.text;</span><span class="sxs-lookup"><span data-stu-id="27f3f-150">(IBAction)LogInAction:(id)sender {   // create authentication header and set it in register client   NSString* username = self.UsernameField.text;   NSString* password = self.PasswordField.text;</span></span>
   
           <span data-ttu-id="27f3f-151">[vlastní createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];</span><span class="sxs-lookup"><span data-stu-id="27f3f-151">[self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];</span></span>
   
           <span data-ttu-id="27f3f-152">__weak ViewController * selfie = vlastní;   [self.registerClient registerWithDeviceToken:self.deviceToken značky: nil andCompletion:^(NSError* error) {Pokud (! chyby) {dispatch_async(dispatch_get_main_queue(), ^ {selfie. SendNotificationButton.enabled = YES;               [samoobslužné MessageBox:@"Success" message:@"Registered úspěšně!"];});}}];}</span><span class="sxs-lookup"><span data-stu-id="27f3f-152">__weak ViewController* selfie = self;   [self.registerClient registerWithDeviceToken:self.deviceToken tags:nil       andCompletion:^(NSError* error) {       if (!error) {           dispatch_async(dispatch_get_main_queue(),           ^{               selfie.SendNotificationButton.enabled = YES;               [self MessageBox:@"Success" message:@"Registered successfully!"];           });       }   }]; }</span></span>

        <span data-ttu-id="27f3f-153">-SendNotificationASPNETBackend (void): (NSString*) pns UsernameTag: (NSString*) usernameTag zpráva: (NSString*) zpráva {NSURLSession* relace = [NSURLSession sessionWithConfiguration: delegateQueue:nil delegáta: nil [NSURLSessionConfiguration defaultSessionConfiguration]];</span><span class="sxs-lookup"><span data-stu-id="27f3f-153">- (void)SendNotificationASPNETBackend:(NSString*)pns UsernameTag:(NSString*)usernameTag            Message:(NSString*)message {    NSURLSession* session = [NSURLSession        sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:nil        delegateQueue:nil];</span></span>

            <span data-ttu-id="27f3f-154">Předat značky hello systém pns a uživatelské jméno jako parametry s hello REST URL toohello ASP.NET back-end nsurl, který * requestURL = [nsurl, který URLWithString: [NSString stringWithFormat:@"%@/api/notifications? systém pns = % @& to_tag = % @", BACKEND_ENDPOINT, systém pns, usernameTag]];</span><span class="sxs-lookup"><span data-stu-id="27f3f-154">// Pass hello pns and username tag as parameters with hello REST URL toohello ASP.NET backend    NSURL* requestURL = [NSURL URLWithString:[NSString        stringWithFormat:@"%@/api/notifications?pns=%@&to_tag=%@", BACKEND_ENDPOINT, pns,        usernameTag]];</span></span>

            <span data-ttu-id="27f3f-155">Žádost o NSMutableURLRequest * = [NSMutableURLRequest requestWithURL:requestURL];    [požadavků setHTTPMethod:@"POST"];</span><span class="sxs-lookup"><span data-stu-id="27f3f-155">NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];    [request setHTTPMethod:@"POST"];</span></span>

            <span data-ttu-id="27f3f-156">Získat imitované authenticationheader hello od klienta registrace hello NSString * authorizationHeaderValue = [NSString stringWithFormat:@"Basic % @", self.registerClient.authenticationHeader];    [požadavků setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];</span><span class="sxs-lookup"><span data-stu-id="27f3f-156">// Get hello mock authenticationheader from hello register client    NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",        self.registerClient.authenticationHeader];    [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];</span></span>

            <span data-ttu-id="27f3f-157">Přidat text zprávy oznámení hello [požadavku setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];    [požadavku setHTTPBody: [zpráva dataUsingEncoding:NSUTF8StringEncoding]];</span><span class="sxs-lookup"><span data-stu-id="27f3f-157">//Add hello notification message body    [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];    [request setHTTPBody:[message dataUsingEncoding:NSUTF8StringEncoding]];</span></span>

            <span data-ttu-id="27f3f-158">Spustit hello odesílání oznámení REST API na hello ASP.NET back-end NSURLSessionDataTask * dataTask = [relace dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError  *Chyba) {NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) odpovědi;        Pokud (Chyba || httpResponse.statusCode! = 200) {NSString* stav = [NSString stringWithFormat:@"Error stav pro % @: % d\nError: %@\n", systém pns, httpResponse.statusCode, chyba];            dispatch_async(dispatch_get_main_queue(), ^ {/ / Append textové protože všechna volání 3 systém PNS mohou mít i tooview informace [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }</span><span class="sxs-lookup"><span data-stu-id="27f3f-158">// Execute hello send notification REST API on hello ASP.NET Backend    NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request        completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)    {        NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;        if (error || httpResponse.statusCode != 200)        {            NSString* status = [NSString stringWithFormat:@"Error Status for %@: %d\nError: %@\n",                                pns, httpResponse.statusCode, error];            dispatch_async(dispatch_get_main_queue(),            ^{                // Append text because all 3 PNS calls may also have information tooview                [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }</span></span>

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            <span data-ttu-id="27f3f-159">}];    [obnovit dataTask]; }</span><span class="sxs-lookup"><span data-stu-id="27f3f-159">}];    [dataTask resume]; }</span></span>


1. <span data-ttu-id="27f3f-160">Aktualizovat hello akci pro hello **odeslat oznámení** tlačítko toouse hello ASP.NET back-end a odeslat tooany systém PNS povoleno přepínač.</span><span class="sxs-lookup"><span data-stu-id="27f3f-160">Update hello action for hello **Send Notification** button toouse hello ASP.NET backend and send tooany PNS enabled by a switch.</span></span>

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



1. <span data-ttu-id="27f3f-161">Ve funkci **ViewDidLoad**, přidejte následující tooinstantiate hello RegisterClient instance hello a nastavte hello delegáta u textových polí.</span><span class="sxs-lookup"><span data-stu-id="27f3f-161">In function **ViewDidLoad**, add hello following tooinstantiate hello RegisterClient instance and set hello delegate for your text fields.</span></span>
   
       self.UsernameField.delegate = self;
       self.PasswordField.delegate = self;
       self.RecipientField.delegate = self;
       self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
2. <span data-ttu-id="27f3f-162">Nyní ve **AppDelegate.m**, odeberte všechny obsah hello hello metody **aplikace: didRegisterForPushNotificationWithDeviceToken:** a nahraďte ji následujícím toomake opravdu hello, který hello zobrazení řadič obsahuje nejnovější token zařízení hello načíst ze služby APN:</span><span class="sxs-lookup"><span data-stu-id="27f3f-162">Now in **AppDelegate.m**, remove all hello content of hello method **application:didRegisterForPushNotificationWithDeviceToken:** and replace it with hello following toomake sure that hello view controller contains hello latest device token retrieved from APNs:</span></span>
   
       // Add import toohello top of hello file
       #import "ViewController.h"
   
       - (void)application:(UIApplication *)application
                   didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
       {
           ViewController* rvc = (ViewController*) self.window.rootViewController;
           rvc.deviceToken = deviceToken;
       }
3. <span data-ttu-id="27f3f-163">Nakonec v **AppDelegate.m**, zajistěte, aby byla hello následující metodu:</span><span class="sxs-lookup"><span data-stu-id="27f3f-163">Finally in **AppDelegate.m**, make sure you have hello following method:</span></span>
   
       - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
           NSLog(@"%@", userInfo);
           [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
       }

## <a name="test-hello-application"></a><span data-ttu-id="27f3f-164">Test hello aplikace</span><span class="sxs-lookup"><span data-stu-id="27f3f-164">Test hello Application</span></span>
1. <span data-ttu-id="27f3f-165">V XCode spusťte aplikaci hello na fyzickém zařízení iOS (nabízených oznámení nebude fungovat v simulátoru hello).</span><span class="sxs-lookup"><span data-stu-id="27f3f-165">In XCode, run hello app on a physical iOS device (push notifications will not work in hello simulator).</span></span>
2. <span data-ttu-id="27f3f-166">V aplikaci pro iOS hello uživatelského rozhraní zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="27f3f-166">In hello iOS app UI, enter a username and password.</span></span> <span data-ttu-id="27f3f-167">Mohou to být libovolný řetězec, ale musí být obě hello stejná hodnota typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="27f3f-167">These can be any string, but they must both be hello same string value.</span></span> <span data-ttu-id="27f3f-168">Pak klikněte na tlačítko **protokolu v**.</span><span class="sxs-lookup"><span data-stu-id="27f3f-168">Then click **Log In**.</span></span>
   
    ![][2]
3. <span data-ttu-id="27f3f-169">Měli byste vidět vyskakovací okno s informacemi o registraci úspěch.</span><span class="sxs-lookup"><span data-stu-id="27f3f-169">You should see a pop-up informing you of registration success.</span></span> <span data-ttu-id="27f3f-170">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="27f3f-170">Click **OK**.</span></span>
   
    ![][3]
4. <span data-ttu-id="27f3f-171">V hello **značky uživatelské jméno příjemce* textové pole, zadejte značky jméno uživatele hello používá s registrací hello z jiného zařízení.</span><span class="sxs-lookup"><span data-stu-id="27f3f-171">In hello **Recipient username tag* text field, enter hello user name tag used with hello registration from another device.</span></span>
5. <span data-ttu-id="27f3f-172">Zadejte zprávu oznámení a klikněte na tlačítko **odeslat oznámení**.</span><span class="sxs-lookup"><span data-stu-id="27f3f-172">Enter a notification message and click **Send Notification**.</span></span>  <span data-ttu-id="27f3f-173">Hello oznámení zpráva se zobrazí pouze hello zařízení, která mají registrace s hello příjemce uživatele název značky.</span><span class="sxs-lookup"><span data-stu-id="27f3f-173">Only hello devices that have a registration with hello recipient user name tag receive hello notification message.</span></span>  <span data-ttu-id="27f3f-174">Pouze odesláním toothose uživatele.</span><span class="sxs-lookup"><span data-stu-id="27f3f-174">It is only sent toothose users.</span></span>
   
    ![][4]

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
