---
title: "aktuální uživatel hello aaaRegister pro nabízená oznámení pomocí webového rozhraní API | Microsoft Docs"
description: "Zjistěte, jak toorequest registrace nabízených oznámení v aplikaci pro iOS pomocí Azure Notification Hubs při registrace se provádí pomocí rozhraní ASP.NET Web API."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: f859feb436093e703d7e1db38354dd356fff8efe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="register-hello-current-user-for-push-notifications-by-using-aspnet"></a><span data-ttu-id="3b854-103">Aktuální uživatel hello zaregistrovat pro nabízená oznámení pomocí technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3b854-103">Register hello current user for push notifications by using ASP.NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3b854-104">iOS</span><span class="sxs-lookup"><span data-stu-id="3b854-104">iOS</span></span>](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="3b854-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="3b854-105">Overview</span></span>
<span data-ttu-id="3b854-106">Toto téma ukazuje, jak toorequest registrace nabízených oznámení pomocí Azure Notification Hubs při registraci se provádí pomocí rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="3b854-106">This topic shows you how toorequest push notification registration with Azure Notification Hubs when registration is performed by ASP.NET Web API.</span></span> <span data-ttu-id="3b854-107">Toto téma rozšiřuje hello kurzu [upozorněte uživatele s Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="3b854-107">This topic extends hello tutorial [Notify users with Notification Hubs].</span></span> <span data-ttu-id="3b854-108">Musí jste již dokončili hello požadované kroky v tomto kurzu toocreate hello ověření mobilní služby.</span><span class="sxs-lookup"><span data-stu-id="3b854-108">You must have already completed hello required steps in that tutorial toocreate hello authenticated mobile service.</span></span> <span data-ttu-id="3b854-109">Další informace o hello upozornit uživatele scénář, najdete v části [upozorněte uživatele s Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="3b854-109">For more information on hello notify users scenario, see [Notify users with Notification Hubs].</span></span>

## <a name="update-your-app"></a><span data-ttu-id="3b854-110">Aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="3b854-110">Update your app</span></span>
1. <span data-ttu-id="3b854-111">V MainStoryboard_iPhone.storyboard přidejte následující součásti z objektu knihovny hello hello:</span><span class="sxs-lookup"><span data-stu-id="3b854-111">In your MainStoryboard_iPhone.storyboard, add hello following components from hello object library:</span></span>
   
   * <span data-ttu-id="3b854-112">**Popisek**: "Push tooUser s centry oznámení"</span><span class="sxs-lookup"><span data-stu-id="3b854-112">**Label**: "Push tooUser with Notification Hubs"</span></span>
   * <span data-ttu-id="3b854-113">**Popisek**: "InstallationId"</span><span class="sxs-lookup"><span data-stu-id="3b854-113">**Label**: "InstallationId"</span></span>
   * <span data-ttu-id="3b854-114">**Popisek**: "User"</span><span class="sxs-lookup"><span data-stu-id="3b854-114">**Label**: "User"</span></span>
   * <span data-ttu-id="3b854-115">**Textové pole**: "User"</span><span class="sxs-lookup"><span data-stu-id="3b854-115">**Text Field**: "User"</span></span>
   * <span data-ttu-id="3b854-116">**Popisek**: "Password"</span><span class="sxs-lookup"><span data-stu-id="3b854-116">**Label**: "Password"</span></span>
   * <span data-ttu-id="3b854-117">**Textové pole**: "Password"</span><span class="sxs-lookup"><span data-stu-id="3b854-117">**Text Field**: "Password"</span></span>
   * <span data-ttu-id="3b854-118">**Tlačítko**: "Přihlášení"</span><span class="sxs-lookup"><span data-stu-id="3b854-118">**Button**: "Login"</span></span>
     
     <span data-ttu-id="3b854-119">V tomto okamžiku by váš scénáře vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="3b854-119">At this point, your storyboard looks like hello following:</span></span>
     
      ![][0]
2. <span data-ttu-id="3b854-120">V editoru pomocníka hello vytvořit výstupy pro všechny ovládací prvky hello přepnout a volat, připojit hello textové pole s hello View Controller (delegát) a vytvořte **akce** pro hello **přihlášení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b854-120">In hello assistant editor, create outlets for all hello switched controls and call them, connect hello text fields with hello View Controller (delegate), and create an **Action** for hello **login** button.</span></span>
   
       ![][1]
   
       Your BreakingNewsViewController.h file should now contain hello following code:
   
        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;
   
        - (IBAction)login:(id)sender;
3. <span data-ttu-id="3b854-121">Vytvoření třídy s názvem **DeviceInfo**, a kopírování hello následující kód do části rozhraní hello hello souboru DeviceInfo.h:</span><span class="sxs-lookup"><span data-stu-id="3b854-121">Create a class named **DeviceInfo**, and copy hello following code into hello interface section of hello file DeviceInfo.h:</span></span>
   
        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;
4. <span data-ttu-id="3b854-122">Zkopírujte následující kód v oddílu implementace hello hello DeviceInfo.m souboru hello:</span><span class="sxs-lookup"><span data-stu-id="3b854-122">Copy hello following code in hello implementation section of hello DeviceInfo.m file:</span></span>
   
            @synthesize installationId = _installationId;
   
            - (id)init {
                if (!(self = [super init]))
                    return nil;
   
                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);
   
                    //store hello install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }
   
                return self;
            }
   
            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }
5. <span data-ttu-id="3b854-123">V PushToUserAppDelegate.h přidejte následující vlastnost singleton hello:</span><span class="sxs-lookup"><span data-stu-id="3b854-123">In PushToUserAppDelegate.h, add hello following property singleton:</span></span>
   
        @property (strong, nonatomic) DeviceInfo* deviceInfo;
6. <span data-ttu-id="3b854-124">V hello **didFinishLaunchingWithOptions** metoda v PushToUserAppDelegate.m, přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="3b854-124">In hello **didFinishLaunchingWithOptions** method in PushToUserAppDelegate.m, add hello following code:</span></span>
   
        self.deviceInfo = [[DeviceInfo alloc] init];
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
   
    <span data-ttu-id="3b854-125">první řádek Hello inicializuje hello **DeviceInfo** typu singleton.</span><span class="sxs-lookup"><span data-stu-id="3b854-125">hello first line initializes hello **DeviceInfo** singleton.</span></span> <span data-ttu-id="3b854-126">Hello druhý řádek spustí hello registrace pro nabízená oznámení, které se již nachází je jste už dokončili hello [Začínáme s Notification Hubs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="3b854-126">hello second line starts hello registration for push notifications, which is already present is you have already completed hello [Get Started with Notification Hubs] tutorial.</span></span>
7. <span data-ttu-id="3b854-127">V PushToUserAppDelegate.m, implementujte metodu hello **didRegisterForRemoteNotificationsWithDeviceToken** ve vaší AppDelegate a přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="3b854-127">In PushToUserAppDelegate.m, implement hello method **didRegisterForRemoteNotificationsWithDeviceToken** in your AppDelegate and add hello following code:</span></span>
   
        self.deviceInfo.deviceToken = deviceToken;
   
    <span data-ttu-id="3b854-128">Toto nastaví hello token zařízení pro žádost hello.</span><span class="sxs-lookup"><span data-stu-id="3b854-128">This sets hello device token for hello request.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3b854-129">V tomto okamžiku by neměly existovat jiný kód v této metodě.</span><span class="sxs-lookup"><span data-stu-id="3b854-129">At this point, there should not be any other code in this method.</span></span> <span data-ttu-id="3b854-130">Pokud již máte volání toohello **registerNativeWithDeviceToken** metoda, která byla přidána po dokončení hello [Začínáme s Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) kurz, musíte okomentujte nebo odebrat volání.</span><span class="sxs-lookup"><span data-stu-id="3b854-130">If you already have a call toohello **registerNativeWithDeviceToken** method that was added when you completed hello [Get Started with Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) tutorial, you must comment-out or remove that call.</span></span>
   > 
   > 
8. <span data-ttu-id="3b854-131">Hello PushToUserAppDelegate.m souboru přidejte následující metodu obslužná rutina hello:</span><span class="sxs-lookup"><span data-stu-id="3b854-131">In hello PushToUserAppDelegate.m file, add hello following handler method:</span></span>
   
   * <span data-ttu-id="3b854-132">(void) aplikace:(UIApplication *) aplikace didReceiveRemoteNotification:(NSDictionary *) informací o uživateli {NSLog (@"% @", informací o uživateli);   UIAlertView * výstraha = [zpráva initWithTitle:@"Notification" [UIAlertView alokační]: cancelButtonTitle delegáta: nil [informací o uživateli objectForKey:@"inAppMessage"]: @"OK" otherButtonTitles:nil, nil];   [Zobrazit výstrahy]; }</span><span class="sxs-lookup"><span data-stu-id="3b854-132">(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {   NSLog(@"%@", userInfo);   UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:                         [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:                         @"OK" otherButtonTitles:nil, nil];   [alert show]; }</span></span>
   
   <span data-ttu-id="3b854-133">Tato metoda zobrazí výstrahu v hello uživatelského rozhraní, když aplikace obdrží oznámení, když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="3b854-133">This method displays an alert in hello UI when your app receives notifications while it is running.</span></span>
9. <span data-ttu-id="3b854-134">Otevřete soubor PushToUserViewController.m hello a návratové hello klávesnice v hello následující implementace:</span><span class="sxs-lookup"><span data-stu-id="3b854-134">Open hello PushToUserViewController.m file, and return hello keyboard in hello following implementation:</span></span>
   
        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }
10. <span data-ttu-id="3b854-135">V hello **viewDidLoad** metoda v souboru PushToUserViewController.m hello inicializovat hello installationId popisek následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3b854-135">In hello **viewDidLoad** method in hello PushToUserViewController.m file, initialize hello installationId label as follows:</span></span>
    
         DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
         Self.installationId.text = deviceInfo.installationId;
11. <span data-ttu-id="3b854-136">Přidejte následující vlastnosti v rozhraní v PushToUserViewController.m hello:</span><span class="sxs-lookup"><span data-stu-id="3b854-136">Add hello following properties in interface in PushToUserViewController.m:</span></span>
    
        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;
12. <span data-ttu-id="3b854-137">Pak přidejte následující implementace hello:</span><span class="sxs-lookup"><span data-stu-id="3b854-137">Then, add hello following implementation:</span></span>
    
            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }
    
            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];
    
                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";
    
                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;
    
                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;
    
                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }
    
                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }
    
                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }
13. <span data-ttu-id="3b854-138">Kopírování hello následující kód do hello **přihlášení** vytvořené XCode metoda obslužné rutiny:</span><span class="sxs-lookup"><span data-stu-id="3b854-138">Copy hello following code into hello **login** handler method created by XCode:</span></span>
    
            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
    
            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];
    
            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];
    
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];
    
            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];
    
    <span data-ttu-id="3b854-139">Tato metoda získá ID instalace a kanál pro nabízená oznámení a odešle ji, společně s hello typu zařízení, toohello ověření webového rozhraní API metoda, která vytvoří registraci v Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="3b854-139">This method gets both an installation ID and channel for push notifications and sends it, along with hello device type, toohello authenticated Web API method that creates a registration in Notification Hubs.</span></span> <span data-ttu-id="3b854-140">Toto webové rozhraní API byla definovaná v [upozorněte uživatele s Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="3b854-140">This Web API was defined in [Notify users with Notification Hubs].</span></span>

<span data-ttu-id="3b854-141">Teď, když hello klientskou aplikaci se aktualizovala, vrátí toohello [upozorněte uživatele s Notification Hubs] a aktualizovat hello mobilní služby toosend oznámení pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="3b854-141">Now that hello client app has been updated, return toohello [Notify users with Notification Hubs] and update hello mobile service toosend notifications by using Notification Hubs.</span></span>

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[upozorněte uživatele s Notification Hubs]: /manage/services/notification-hubs/notify-users-aspnet

[Začínáme s Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios
