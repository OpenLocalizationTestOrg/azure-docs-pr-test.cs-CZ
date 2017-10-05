---
title: "Upozornění uživatelů centra oznámení Azure pro iOS pomocí backendu .NET"
description: "Zjistěte, jak odesílat nabízená oznámení pro uživatele v Azure. Ukázky kódu jsou vytvořeny v Objective-C a rozhraní API .NET pro back-end."
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
ms.openlocfilehash: 0fa7a886e1ecb0a90b6aebc1dbf9ef0c6ce1acf1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-notify-users-for-ios-with-net-backend"></a><span data-ttu-id="c4a78-104">Upozornění uživatelů centra oznámení Azure pro iOS pomocí backendu .NET</span><span class="sxs-lookup"><span data-stu-id="c4a78-104">Azure Notification Hubs Notify Users for iOS with .NET backend</span></span>
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a><span data-ttu-id="c4a78-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="c4a78-105">Overview</span></span>
<span data-ttu-id="c4a78-106">Podpora nabízená oznámení v Azure umožňuje přístup snadné použití, multiplatform a nabízené škálovanou infrastrukturu, která výrazně zjednodušuje implementaci nabízená oznámení spotřebních a podnikových aplikací pro mobilní platformy.</span><span class="sxs-lookup"><span data-stu-id="c4a78-106">Push notification support in Azure enables you to access an easy-to-use, multiplatform, and scaled-out push infrastructure, which greatly simplifies the implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span> <span data-ttu-id="c4a78-107">V tomto kurzu se dozvíte, jak se dají pomocí Azure Notification Hubs posílat nabízená oznámení specifickým uživatelům aplikace na specifickém zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4a78-107">This tutorial shows you how to use Azure Notification Hubs to send push notifications to a specific app user on a specific device.</span></span> <span data-ttu-id="c4a78-108">Backendu ASP.NET WebAPI slouží k ověřování klientů a ke generování oznámení, jak je znázorněno v tématu pokyny [registrace z back-end aplikace](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span><span class="sxs-lookup"><span data-stu-id="c4a78-108">An ASP.NET WebAPI backend is used to authenticate clients and to generate notifications, as shown in the guidance topic [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span></span>

> [!NOTE]
> <span data-ttu-id="c4a78-109">V tomto kurzu se předpokládá, že jste vytvořili a nakonfigurovali vaše Centrum oznámení, jak je popsáno v [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c4a78-109">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span></span> <span data-ttu-id="c4a78-110">V tomto kurzu je také předpokladem pro [zabezpečení Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="c4a78-110">This tutorial is also the prerequisite to the [Secure Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) tutorial.</span></span>
> <span data-ttu-id="c4a78-111">Pokud chcete použít jako back-end služby Mobile Apps, najdete v článku [mobilní aplikace začít pracovat s nabízené](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="c4a78-111">If you want to use Mobile Apps as your backend service, see the [Mobile Apps Get Started with Push](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a><span data-ttu-id="c4a78-112">Upravit aplikaci s iOS</span><span class="sxs-lookup"><span data-stu-id="c4a78-112">Modify your iOS app</span></span>
1. <span data-ttu-id="c4a78-113">Otevřete aplikaci zobrazení jednu stránku, kterou jste vytvořili v [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="c4a78-113">Open the Single Page view app you created in the [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) tutorial.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c4a78-114">V této části předpokládáme, že váš projekt je nakonfigurovaný s názvem prázdné organizace.</span><span class="sxs-lookup"><span data-stu-id="c4a78-114">In this section we assume that your project is configured with an empty organization name.</span></span> <span data-ttu-id="c4a78-115">Pokud ne, budete muset předřazení název vaší organizace na všechny názvy tříd.</span><span class="sxs-lookup"><span data-stu-id="c4a78-115">If not, you will need to prepend your organization name to all class names.</span></span>
   > 
   > 
2. <span data-ttu-id="c4a78-116">Ve vaší Main.storyboard přidáte součásti znázorněno na snímku obrazovky níže z objektu knihovny.</span><span class="sxs-lookup"><span data-stu-id="c4a78-116">In your Main.storyboard add the components shown in the screenshot below from the object library.</span></span>
   
    ![][1]
   
   * <span data-ttu-id="c4a78-117">**Uživatelské jméno**: UITextField A s zástupný text *zadejte uživatelské jméno*bezprostředně pod odesílání výsledky popisku a omezená na levý a pravý okraj a pod návěští odesílání výsledky.</span><span class="sxs-lookup"><span data-stu-id="c4a78-117">**Username**: A UITextField with placeholder text, *Enter Username*, immediately beneath the send results label and constrained to the left and right margins and beneath the send results label.</span></span>
   * <span data-ttu-id="c4a78-118">**Heslo**: UITextField A s zástupný text *zadejte heslo*, bezprostředně pod uživatelské jméno textové pole a omezené levý a pravý okraj a pod textové pole uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="c4a78-118">**Password**: A UITextField with placeholder text, *Enter Password*, immediately beneath the username text field and constrained to the left and right margins and beneath the username text field.</span></span> <span data-ttu-id="c4a78-119">Zkontrolujte **zabezpečení zadávání textu** možnost v atributu Inspector, v části *vrátit klíč*.</span><span class="sxs-lookup"><span data-stu-id="c4a78-119">Check the **Secure Text Entry** option in the Attribute Inspector, under *Return Key*.</span></span>
   * <span data-ttu-id="c4a78-120">**Přihlaste se**: A UIButton s názvem bez přípony bezprostředně pod textové pole heslo a zrušte zaškrtnutí políčka **povoleno** možnost Inspector atributy, v části *obsahu ovládacího prvku*</span><span class="sxs-lookup"><span data-stu-id="c4a78-120">**Log in**: A UIButton labeled immediately beneath the password text field and uncheck the **Enabled** option in the Attributes Inspector, under *Control-Content*</span></span>
   * <span data-ttu-id="c4a78-121">**WNS**: štítek a přepínače, které umožňují odesílání oznámení služby oznámení Windows, pokud byla instalace v centru.</span><span class="sxs-lookup"><span data-stu-id="c4a78-121">**WNS**: Label and switch to enable sending the notification Windows Notification Service if it has been setup on the hub.</span></span> <span data-ttu-id="c4a78-122">Najdete v článku [Windows Začínáme](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="c4a78-122">See the [Windows Getting Started](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) tutorial.</span></span>
   * <span data-ttu-id="c4a78-123">**GCM**: štítek a přepínače povolíte odesílání oznámení pro Google Cloud Messaging, pokud byla instalace v rozbočovači.</span><span class="sxs-lookup"><span data-stu-id="c4a78-123">**GCM**: Label and switch to enable sending the notification to Google Cloud Messaging if it has been setup on the hub.</span></span> <span data-ttu-id="c4a78-124">V tématu [Začínáme Android](notification-hubs-android-push-notification-google-gcm-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="c4a78-124">See [Android Getting Started](notification-hubs-android-push-notification-google-gcm-get-started.md) tutorial.</span></span>
   * <span data-ttu-id="c4a78-125">**APNS**: štítek a přepínače povolíte odesílání oznámení do služby Apple platformy oznámení.</span><span class="sxs-lookup"><span data-stu-id="c4a78-125">**APNS**: Label and switch to enable sending the notification to the Apple Platform Notification Service.</span></span>
   * <span data-ttu-id="c4a78-126">**Uživatelské jméno Recipent**: UITextField A s zástupný text *značky uživatelské jméno příjemce*, bezprostředně pod GCM popisku a omezené levý a pravý okraj a pod popisek GCM.</span><span class="sxs-lookup"><span data-stu-id="c4a78-126">**Recipent Username**:A UITextField with placeholder text, *Recipient username tag*, immediately beneath the GCM label and constrained to the left and right margins and beneath the GCM label.</span></span>

    <span data-ttu-id="c4a78-127">Některé součásti byly přidány v [Začínáme s Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="c4a78-127">Some components were added in the [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) tutorial.</span></span>

1. <span data-ttu-id="c4a78-128">**CTRL** přetáhněte z komponenty v zobrazení ViewController.h a přidejte tyto nové výstupy.</span><span class="sxs-lookup"><span data-stu-id="c4a78-128">**Ctrl** drag from the components in the view to ViewController.h and add these new outlets.</span></span>
   
        @property (weak, nonatomic) IBOutlet UITextField *UsernameField;
        @property (weak, nonatomic) IBOutlet UITextField *PasswordField;
        @property (weak, nonatomic) IBOutlet UITextField *RecipientField;
        @property (weak, nonatomic) IBOutlet UITextField *NotificationField;
   
        // Used to enable the buttons on the UI
        @property (weak, nonatomic) IBOutlet UIButton *LogInButton;
        @property (weak, nonatomic) IBOutlet UIButton *SendNotificationButton;
   
        // Used to enabled sending notifications across platforms
        @property (weak, nonatomic) IBOutlet UISwitch *WNSSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *GCMSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *APNSSwitch;
   
        - (IBAction)LogInAction:(id)sender;
2. <span data-ttu-id="c4a78-129">V ViewController.h, přidejte následující `#define` pod příkazy pro import.</span><span class="sxs-lookup"><span data-stu-id="c4a78-129">In ViewController.h, add the following `#define` just below your import statements.</span></span> <span data-ttu-id="c4a78-130">Nahraďte *< Zadejte koncový bod vašeho back-end\>*  zástupný symbol cílová adresa URL můžete použít k nasazení vašeho back-end aplikace v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="c4a78-130">Substitute the *<Enter Your Backend Endpoint\>* placeholder with the Destination URL you used to deploy your app backend in the previous section.</span></span> <span data-ttu-id="c4a78-131">Například *http://you_backend.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="c4a78-131">For example, *http://you_backend.azurewebsites.net*.</span></span>
   
        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
3. <span data-ttu-id="c4a78-132">V projektu, vytvořte novou **Touch kakao třída** s názvem **RegisterClient** rozhraní s ASP.NET back-end jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c4a78-132">In your project, create a new **Cocoa Touch class** named **RegisterClient** to interface with the ASP.NET back-end you created.</span></span> <span data-ttu-id="c4a78-133">Vytvořte třídu, která dědí z `NSObject`.</span><span class="sxs-lookup"><span data-stu-id="c4a78-133">Create the class inheriting from `NSObject`.</span></span> <span data-ttu-id="c4a78-134">V RegisterClient.h přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="c4a78-134">Then add the following code in the RegisterClient.h.</span></span>
   
        @interface RegisterClient : NSObject
   
        @property (strong, nonatomic) NSString* authenticationHeader;
   
        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;
   
        -(instancetype) initWithEndpoint:(NSString*)Endpoint;
   
        @end
4. <span data-ttu-id="c4a78-135">V aktualizaci RegisterClient.m `@interface` části:</span><span class="sxs-lookup"><span data-stu-id="c4a78-135">In the RegisterClient.m update the `@interface` section:</span></span>
   
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
5. <span data-ttu-id="c4a78-136">Nahraďte `@implementation` část v RegisterClient.m následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="c4a78-136">Replace the `@implementation` section in the RegisterClient.m with the following code.</span></span>

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

    <span data-ttu-id="c4a78-137">Výše uvedený kód implementuje logiku popsané v článku pokyny [registrace z back-end aplikace](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) pomocí NSURLSession k provedení REST zavolá na váš back-end aplikace a NSUserDefaults ukládat místně registrationId vrácený centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="c4a78-137">The code above implements the logic explained in the guidance article [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) using NSURLSession to perform REST calls to your app backend, and NSUserDefaults to locally store the registrationId returned by the notification hub.</span></span>

    <span data-ttu-id="c4a78-138">Všimněte si, že tato třída vyžaduje, aby jeho vlastnost **authorizationHeader** nastavit správné fungování.</span><span class="sxs-lookup"><span data-stu-id="c4a78-138">Note that this class requires its property **authorizationHeader** to be set in order to work properly.</span></span> <span data-ttu-id="c4a78-139">Tato vlastnost je nastavena **ViewController** třída po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c4a78-139">This property is set by the **ViewController** class after the log in.</span></span>

1. <span data-ttu-id="c4a78-140">V ViewController.h, přidejte `#import` příkaz pro RegisterClient.h.</span><span class="sxs-lookup"><span data-stu-id="c4a78-140">In ViewController.h, add a `#import` statement for RegisterClient.h.</span></span> <span data-ttu-id="c4a78-141">Pak přidejte deklaraci pro token zařízení a odkaz na `RegisterClient` instance v `@interface` části:</span><span class="sxs-lookup"><span data-stu-id="c4a78-141">Then add a declaration for the device token and reference to a `RegisterClient` instance in the `@interface` section:</span></span>
   
        #import "RegisterClient.h"
   
        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;
2. <span data-ttu-id="c4a78-142">V ViewController.m, přidejte deklaraci soukromá metoda v `@interface` části:</span><span class="sxs-lookup"><span data-stu-id="c4a78-142">In ViewController.m, add a private method declaration in the `@interface` section:</span></span>
   
        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>
   
        // create the Authorization header to perform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
   
        @end

> [!NOTE]
> <span data-ttu-id="c4a78-143">Následující fragment kódu není schéma zabezpečeného ověřování, doporučujeme nahradit implementace **createAndSetAuthenticationHeaderWithUsername:AndPassword:** s vaší konkrétní ověřovací mechanismus, který generuje ověřovací token pro registraci klienta třídu, například OAuth, služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c4a78-143">The following snippet is not a secure authentication scheme, you should substitute the implementation of the **createAndSetAuthenticationHeaderWithUsername:AndPassword:** with your specific authentication mechanism that generates an authentication token to be consumed by the register client class, e.g. OAuth, Active Directory.</span></span>
> 
> 

1. <span data-ttu-id="c4a78-144">Potom v `@implementation` části ViewController.m přidejte následující kód, který přidá implementaci nastavení hlavičku tokenu a ověření zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4a78-144">Then in the `@implementation` section of ViewController.m add the following code which adds the implementation for setting the device token and authentication header.</span></span>
   
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
   
    <span data-ttu-id="c4a78-145">Všimněte si, jak nastavení token zařízení povolí tlačítko přihlásit.</span><span class="sxs-lookup"><span data-stu-id="c4a78-145">Note how setting the device token enables the log in button.</span></span> <span data-ttu-id="c4a78-146">Je to proto, že v rámci akce přihlášení se zaregistruje řadiče zobrazení pro nabízená oznámení s back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4a78-146">This is becasue as a part of the login action, the view controller registers for push notifications with the app backend.</span></span> <span data-ttu-id="c4a78-147">Proto jsme nechcete, aby akce přihlášení být přístupné, dokud token zařízení správně nastavit.</span><span class="sxs-lookup"><span data-stu-id="c4a78-147">Hence, we do not want Log In action to be accessible till the device token has been properly set up.</span></span> <span data-ttu-id="c4a78-148">Přihlášení z registraci nabízených můžete oddělit tak dlouho, dokud první se stane před jeho.</span><span class="sxs-lookup"><span data-stu-id="c4a78-148">You can decouple the log in from the push registration as long as the former happens before the latter.</span></span>
2. <span data-ttu-id="c4a78-149">V ViewController.m, použijte následující fragmenty implementovat metodu akce pro vaše **protokolu v** tlačítko a metody k odeslání zprávy oznámení pomocí ASP.NET back-end.</span><span class="sxs-lookup"><span data-stu-id="c4a78-149">In ViewController.m, use the following snippets to implement the action method for your **Log In** button and a method to send the notification message using the ASP.NET backend.</span></span>
   
       - <span data-ttu-id="c4a78-150">(IBAction) LogInAction: odesílatele (id) {/ / vytvoření záhlaví ověření a nastavte ji zaregistrovat klienta NSString * uživatelské jméno = sám sebou. UsernameField.text;   Heslo NSString * = sám sebou. PasswordField.text;</span><span class="sxs-lookup"><span data-stu-id="c4a78-150">(IBAction)LogInAction:(id)sender {   // create authentication header and set it in register client   NSString* username = self.UsernameField.text;   NSString* password = self.PasswordField.text;</span></span>
   
           <span data-ttu-id="c4a78-151">[vlastní createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];</span><span class="sxs-lookup"><span data-stu-id="c4a78-151">[self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];</span></span>
   
           <span data-ttu-id="c4a78-152">__weak ViewController * selfie = vlastní;   [self.registerClient registerWithDeviceToken:self.deviceToken značky: nil andCompletion:^(NSError* error) {Pokud (! chyby) {dispatch_async(dispatch_get_main_queue(), ^ {selfie. SendNotificationButton.enabled = YES;               [samoobslužné MessageBox:@"Success" message:@"Registered úspěšně!"];});}}];}</span><span class="sxs-lookup"><span data-stu-id="c4a78-152">__weak ViewController* selfie = self;   [self.registerClient registerWithDeviceToken:self.deviceToken tags:nil       andCompletion:^(NSError* error) {       if (!error) {           dispatch_async(dispatch_get_main_queue(),           ^{               selfie.SendNotificationButton.enabled = YES;               [self MessageBox:@"Success" message:@"Registered successfully!"];           });       }   }]; }</span></span>

        <span data-ttu-id="c4a78-153">-SendNotificationASPNETBackend (void): (NSString*) pns UsernameTag: (NSString*) usernameTag zpráva: (NSString*) zpráva {NSURLSession* relace = [NSURLSession sessionWithConfiguration: delegateQueue:nil delegáta: nil [NSURLSessionConfiguration defaultSessionConfiguration]];</span><span class="sxs-lookup"><span data-stu-id="c4a78-153">- (void)SendNotificationASPNETBackend:(NSString*)pns UsernameTag:(NSString*)usernameTag            Message:(NSString*)message {    NSURLSession* session = [NSURLSession        sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:nil        delegateQueue:nil];</span></span>

            <span data-ttu-id="c4a78-154">Předat značce systém pns a uživatelské jméno jako parametry s adresou URL REST ASP.NET back-end nsurl, který * requestURL = [nsurl, který URLWithString: [NSString stringWithFormat:@"%@/api/notifications? pns = % @& to_tag = % @", BACKEND_ENDPOINT, systém pns, usernameTag]];</span><span class="sxs-lookup"><span data-stu-id="c4a78-154">// Pass the pns and username tag as parameters with the REST URL to the ASP.NET backend    NSURL* requestURL = [NSURL URLWithString:[NSString        stringWithFormat:@"%@/api/notifications?pns=%@&to_tag=%@", BACKEND_ENDPOINT, pns,        usernameTag]];</span></span>

            <span data-ttu-id="c4a78-155">Žádost o NSMutableURLRequest * = [NSMutableURLRequest requestWithURL:requestURL];    [požadavků setHTTPMethod:@"POST"];</span><span class="sxs-lookup"><span data-stu-id="c4a78-155">NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];    [request setHTTPMethod:@"POST"];</span></span>

            <span data-ttu-id="c4a78-156">Získat imitované authenticationheader od klienta registrace NSString * authorizationHeaderValue = [NSString stringWithFormat:@"Basic % @", self.registerClient.authenticationHeader];    [požadavků setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];</span><span class="sxs-lookup"><span data-stu-id="c4a78-156">// Get the mock authenticationheader from the register client    NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",        self.registerClient.authenticationHeader];    [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];</span></span>

            <span data-ttu-id="c4a78-157">Přidat obsah zprávy oznámení [požadavku setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];    [požadavku setHTTPBody: [zpráva dataUsingEncoding:NSUTF8StringEncoding]];</span><span class="sxs-lookup"><span data-stu-id="c4a78-157">//Add the notification message body    [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];    [request setHTTPBody:[message dataUsingEncoding:NSUTF8StringEncoding]];</span></span>

            <span data-ttu-id="c4a78-158">Spustit odesílání oznámení REST API na dataTask ASP.NET back-end NSURLSessionDataTask * = [completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) dataTaskWithRequest:request relace {NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) odpovědi;        Pokud (Chyba || httpResponse.statusCode! = 200) {NSString* stav = [NSString stringWithFormat:@"Error stav pro % @: % d\nError: %@\n", systém pns, httpResponse.statusCode, chyba];            dispatch_async(dispatch_get_main_queue(), ^ {/ / Append textové protože všechna volání 3 systém PNS mohou mít i informace o zobrazení [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }</span><span class="sxs-lookup"><span data-stu-id="c4a78-158">// Execute the send notification REST API on the ASP.NET Backend    NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request        completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)    {        NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;        if (error || httpResponse.statusCode != 200)        {            NSString* status = [NSString stringWithFormat:@"Error Status for %@: %d\nError: %@\n",                                pns, httpResponse.statusCode, error];            dispatch_async(dispatch_get_main_queue(),            ^{                // Append text because all 3 PNS calls may also have information to view                [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }</span></span>

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            <span data-ttu-id="c4a78-159">}];    [obnovit dataTask]; }</span><span class="sxs-lookup"><span data-stu-id="c4a78-159">}];    [dataTask resume]; }</span></span>


1. <span data-ttu-id="c4a78-160">Akce pro aktualizace **odeslat oznámení** tlačítko pomocí ASP.NET back-end a poslat jakékoli PNS povoleno přepínač.</span><span class="sxs-lookup"><span data-stu-id="c4a78-160">Update the action for the **Send Notification** button to use the ASP.NET backend and send to any PNS enabled by a switch.</span></span>

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



1. <span data-ttu-id="c4a78-161">Ve funkci **ViewDidLoad**, přidejte následující instance RegisterClient instance a nastavit delegáta u textových polí.</span><span class="sxs-lookup"><span data-stu-id="c4a78-161">In function **ViewDidLoad**, add the following to instantiate the RegisterClient instance and set the delegate for your text fields.</span></span>
   
       self.UsernameField.delegate = self;
       self.PasswordField.delegate = self;
       self.RecipientField.delegate = self;
       self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
2. <span data-ttu-id="c4a78-162">Nyní ve **AppDelegate.m**, odeberte všechny obsah metody **aplikace: didRegisterForPushNotificationWithDeviceToken:** a nahraďte ji následujícím a ujistěte se, že řadiče zobrazení obsahuje nejnovější token zařízení načíst ze služby APN:</span><span class="sxs-lookup"><span data-stu-id="c4a78-162">Now in **AppDelegate.m**, remove all the content of the method **application:didRegisterForPushNotificationWithDeviceToken:** and replace it with the following to make sure that the view controller contains the latest device token retrieved from APNs:</span></span>
   
       // Add import to the top of the file
       #import "ViewController.h"
   
       - (void)application:(UIApplication *)application
                   didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
       {
           ViewController* rvc = (ViewController*) self.window.rootViewController;
           rvc.deviceToken = deviceToken;
       }
3. <span data-ttu-id="c4a78-163">Nakonec v **AppDelegate.m**, zajistěte, aby byla následující metodu:</span><span class="sxs-lookup"><span data-stu-id="c4a78-163">Finally in **AppDelegate.m**, make sure you have the following method:</span></span>
   
       - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
           NSLog(@"%@", userInfo);
           [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
       }

## <a name="test-the-application"></a><span data-ttu-id="c4a78-164">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="c4a78-164">Test the Application</span></span>
1. <span data-ttu-id="c4a78-165">V XCode spusťte aplikaci na fyzickém zařízení iOS (nabízených oznámení nebude fungovat v simulátoru).</span><span class="sxs-lookup"><span data-stu-id="c4a78-165">In XCode, run the app on a physical iOS device (push notifications will not work in the simulator).</span></span>
2. <span data-ttu-id="c4a78-166">V aplikaci pro iOS uživatelského rozhraní zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="c4a78-166">In the iOS app UI, enter a username and password.</span></span> <span data-ttu-id="c4a78-167">Mohou to být libovolný řetězec, ale musí být obě stejnou hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="c4a78-167">These can be any string, but they must both be the same string value.</span></span> <span data-ttu-id="c4a78-168">Pak klikněte na tlačítko **protokolu v**.</span><span class="sxs-lookup"><span data-stu-id="c4a78-168">Then click **Log In**.</span></span>
   
    ![][2]
3. <span data-ttu-id="c4a78-169">Měli byste vidět vyskakovací okno s informacemi o registraci úspěch.</span><span class="sxs-lookup"><span data-stu-id="c4a78-169">You should see a pop-up informing you of registration success.</span></span> <span data-ttu-id="c4a78-170">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4a78-170">Click **OK**.</span></span>
   
    ![][3]
4. <span data-ttu-id="c4a78-171">V **značky uživatelské jméno příjemce* textové pole, zadejte název značky uživatele použít s registrací z jiného zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4a78-171">In the **Recipient username tag* text field, enter the user name tag used with the registration from another device.</span></span>
5. <span data-ttu-id="c4a78-172">Zadejte zprávu oznámení a klikněte na tlačítko **odeslat oznámení**.</span><span class="sxs-lookup"><span data-stu-id="c4a78-172">Enter a notification message and click **Send Notification**.</span></span>  <span data-ttu-id="c4a78-173">Jenom zařízení, která mají registrace pomocí značky jméno příjemce uživatele přijímat oznámení.</span><span class="sxs-lookup"><span data-stu-id="c4a78-173">Only the devices that have a registration with the recipient user name tag receive the notification message.</span></span>  <span data-ttu-id="c4a78-174">Pouze odesláním pro tyto uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4a78-174">It is only sent to those users.</span></span>
   
    ![][4]

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
