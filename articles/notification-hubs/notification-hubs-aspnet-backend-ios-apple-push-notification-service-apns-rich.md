---
title: "Azure Notification Hubs bohaté Push"
description: "Naučte se odesílání bohaté nabízených oznámení do aplikace pro iOS z Azure. Ukázky kódu jsou vytvořeny v Objective-C a C#."
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
ms.openlocfilehash: 394efdc2dfaff0666bc23d8a448b0a00d414da99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-rich-push"></a><span data-ttu-id="148ca-104">Azure Notification Hubs bohaté Push</span><span class="sxs-lookup"><span data-stu-id="148ca-104">Azure Notification Hubs Rich Push</span></span>
## <a name="overview"></a><span data-ttu-id="148ca-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="148ca-105">Overview</span></span>
<span data-ttu-id="148ca-106">Chcete-li zapojení uživatelů rychlých bohaté obsah, aplikace chtít nabízet nad rámec prostý text.</span><span class="sxs-lookup"><span data-stu-id="148ca-106">In order to engage users with instant rich contents, an application might want to push beyond plain text.</span></span> <span data-ttu-id="148ca-107">Tato oznámení zvýšit úroveň, akce uživatelů a existuje obsah, jako jsou adresy URL, zvuky, Image nebo kupóny a další.</span><span class="sxs-lookup"><span data-stu-id="148ca-107">These notifications promote user interactions and  present content such as urls, sounds, images/coupons, and more.</span></span> <span data-ttu-id="148ca-108">V tomto kurzu vychází [upozornění uživatelů](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tématu a ukazuje, jak odesílat nabízená oznámení, které jsou částí (například obrázek).</span><span class="sxs-lookup"><span data-stu-id="148ca-108">This tutorial builds on the [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) topic, and shows how to send push notifications that incorporate payloads (for example, image).</span></span>

<span data-ttu-id="148ca-109">V tomto kurzu je kompatibilní s iOS 7 a 8.</span><span class="sxs-lookup"><span data-stu-id="148ca-109">This tutorial is compatible with iOS 7 & 8.</span></span>

  ![][IOS1]

<span data-ttu-id="148ca-110">Na vysoké úrovni:</span><span class="sxs-lookup"><span data-stu-id="148ca-110">At a high level:</span></span>

1. <span data-ttu-id="148ca-111">Back-end aplikace:</span><span class="sxs-lookup"><span data-stu-id="148ca-111">The app backend:</span></span>
   * <span data-ttu-id="148ca-112">Ukládá bohaté datová část (v tomto případě obrázek) v úložišti databáze nebo místní back-end</span><span class="sxs-lookup"><span data-stu-id="148ca-112">Stores the rich payload (in this case, image) in the backend database/local storage</span></span>
   * <span data-ttu-id="148ca-113">Odešle ID tohoto bohaté oznámení do zařízení.</span><span class="sxs-lookup"><span data-stu-id="148ca-113">Sends ID of this rich notification to the device</span></span>
2. <span data-ttu-id="148ca-114">Aplikace na zařízení:</span><span class="sxs-lookup"><span data-stu-id="148ca-114">App on the device:</span></span>
   * <span data-ttu-id="148ca-115">Kontaktuje back-end vyžaduje bohaté datovou část s ID obdrží</span><span class="sxs-lookup"><span data-stu-id="148ca-115">Contacts the backend requesting the rich payload with the ID it receives</span></span>
   * <span data-ttu-id="148ca-116">Odešle oznámení uživatelům na zařízení po načtení dat dokončení a zobrazuje datové části okamžitě, když uživatel klepnutím na další informace</span><span class="sxs-lookup"><span data-stu-id="148ca-116">Sends users notifications on the device when data retrieval is complete, and shows the payload immediately when users tap to learn more</span></span>

## <a name="webapi-project"></a><span data-ttu-id="148ca-117">WebAPI projektu</span><span class="sxs-lookup"><span data-stu-id="148ca-117">WebAPI Project</span></span>
1. <span data-ttu-id="148ca-118">V sadě Visual Studio, otevřete **AppBackend** projekt, který jste vytvořili v [upozornění uživatelů](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="148ca-118">In Visual Studio, open the **AppBackend** project that you created in the [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
2. <span data-ttu-id="148ca-119">Získat image, kterou chcete upozornit uživatele a umístí jej **img** složky v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="148ca-119">Obtain an image you would like to notify users with, and put it in an **img** folder in your project directory.</span></span>
3. <span data-ttu-id="148ca-120">Klikněte na tlačítko **zobrazit všechny soubory** v Průzkumníku řešení klikněte pravým tlačítkem složku pro **zahrnout do projektu**.</span><span class="sxs-lookup"><span data-stu-id="148ca-120">Click **Show All Files** in the Solution Explorer, and right-click the folder to **Include In Project**.</span></span>
4. <span data-ttu-id="148ca-121">Vybraná Image, změnit jeho akce sestavení v okně Vlastnosti a **vložený prostředek**.</span><span class="sxs-lookup"><span data-stu-id="148ca-121">With the image selected, change its Build Action in Properties window to **Embedded Resource**.</span></span>
   
    ![][IOS2]
5. <span data-ttu-id="148ca-122">V **Notifications.cs**, přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="148ca-122">In **Notifications.cs**, add the following using statement:</span></span>
   
        using System.Reflection;
6. <span data-ttu-id="148ca-123">Aktualizovat celek **oznámení** třídy následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="148ca-123">Update the whole **Notifications** class with the following code.</span></span> <span data-ttu-id="148ca-124">Ujistěte se, že nahraďte zástupné symboly vaše přihlašovací údaje centra oznámení a název souboru obrázku.</span><span class="sxs-lookup"><span data-stu-id="148ca-124">Be sure to replace the placeholders with your notification hub credentials and image file name.</span></span>
   
        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
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
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
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
   > <span data-ttu-id="148ca-125">(volitelné) Odkazovat na [postup vložení a přístup k prostředkům pomocí Visual C#](http://support.microsoft.com/kb/319292) Další informace o tom, jak přidat a získat prostředky projektu.</span><span class="sxs-lookup"><span data-stu-id="148ca-125">(optional) Refer to [How to embed and access resources by using Visual C#](http://support.microsoft.com/kb/319292) for more information on how to add and obtain project resources.</span></span>
   > 
   > 
7. <span data-ttu-id="148ca-126">V **NotificationsController.cs**, znovu definovat **NotificationsController** s následující fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="148ca-126">In **NotificationsController.cs**, redefine **NotificationsController**  with the following snippets.</span></span> <span data-ttu-id="148ca-127">To odešle počáteční tichou bohaté oznámení id zařízení a umožňuje načítání klientské bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="148ca-127">This sends an initial silent rich notification id to device and allows client-side retrieval of image:</span></span>
   
        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);
   
            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");
   
            return result;
        }
   
        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");
   
            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";
   
            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
8. <span data-ttu-id="148ca-128">Nyní jsme bude znovu nasaďte tuto aplikaci na web Azure aby přístupná ze všech zařízení.</span><span class="sxs-lookup"><span data-stu-id="148ca-128">Now we will re-deploy this app to an Azure Website in order to make it accessible from all devices.</span></span> <span data-ttu-id="148ca-129">Klikněte pravým tlačítkem na projekt **AppBackend** a vyberte **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="148ca-129">Right-click on the **AppBackend** project and select **Publish**.</span></span>
9. <span data-ttu-id="148ca-130">Vyberte web Azure jako váš cíl publikování.</span><span class="sxs-lookup"><span data-stu-id="148ca-130">Select Azure Website as your publish target.</span></span> <span data-ttu-id="148ca-131">Přihlaste se pomocí účtu Azure a vyberte stávajícího nebo nového webu a poznamenejte si **cílová adresa URL** vlastnost **připojení** kartě.</span><span class="sxs-lookup"><span data-stu-id="148ca-131">Log in with your Azure account and select an existing or new Website, and make a note of the **destination URL** property in the **Connection** tab.</span></span> <span data-ttu-id="148ca-132">Na tuto adresu URL budeme odkazovat jako na *koncový bod back-endu* později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="148ca-132">We will refer to this URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="148ca-133">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="148ca-133">Click **Publish**.</span></span>

## <a name="modify-the-ios-project"></a><span data-ttu-id="148ca-134">Upravit projekt pro iOS</span><span class="sxs-lookup"><span data-stu-id="148ca-134">Modify the iOS project</span></span>
<span data-ttu-id="148ca-135">Teď, když jste změnili váš back-end aplikace k odesílání jen na *id* oznámení, se změní aplikace pro iOS k zpracování toto id a načtení bohaté zprávy z vaší back-end.</span><span class="sxs-lookup"><span data-stu-id="148ca-135">Now that you have modified your app backend to send just the *id* of a notification, you will change your iOS app to handle that id and retrieve the rich message from your backend.</span></span>

1. <span data-ttu-id="148ca-136">Otevřete projekt iOS a povolit vzdálenou oznámení tak, že přejdete do cílových hlavní aplikace v **cíle** části.</span><span class="sxs-lookup"><span data-stu-id="148ca-136">Open your iOS project, and enable remote notifications by going to your main app target in the **Targets** section.</span></span>
2. <span data-ttu-id="148ca-137">Klikněte na **možnosti**, zapnout **režimy pozadí**a zkontrolujte **vzdáleného oznámení** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="148ca-137">Click on **Capabilities**, turn on **Background Modes**, and check the **Remote Notifications** checkbox.</span></span>
   
    ![][IOS3]
3. <span data-ttu-id="148ca-138">Přejděte na **Main.storyboard**a zajistěte, aby byla řadič zobrazení (uvedená jako Domů řadiče zobrazení v tomto kurzu) z [upozornit uživatele](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="148ca-138">Go to **Main.storyboard**, and make sure you have a View Controller (refered to as Home View Controller in this tutorial) from [Notify User](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
4. <span data-ttu-id="148ca-139">Přidat **navigační řadič** scénáře a přetáhněte ovládací prvek na Domů zobrazení kontroler, aby bylo **kořenový zobrazení** navigace.</span><span class="sxs-lookup"><span data-stu-id="148ca-139">Add a **Navigation Controller** to your storyboard, and control-drag to Home View Controller to make it the **root view** of navigation.</span></span> <span data-ttu-id="148ca-140">Zajistěte, aby **je počáteční View Controller** v atributech inspector je vybraná řadičem navigace.</span><span class="sxs-lookup"><span data-stu-id="148ca-140">Make sure the **Is Initial View Controller** in Attributes inspector is selected for the Navigation Controller only.</span></span>
5. <span data-ttu-id="148ca-141">Přidat **View Controller** scénáře a přidat **Image zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="148ca-141">Add a **View Controller** to storyboard and add an **Image View**.</span></span> <span data-ttu-id="148ca-142">Toto je stránka, kterou uživatelé uvidí, když se rozhodnete další informace kliknutím na notifiication.</span><span class="sxs-lookup"><span data-stu-id="148ca-142">This is the page users will see once they choose to learn more by clicking on the notifiication.</span></span> <span data-ttu-id="148ca-143">Vaše scénáře by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="148ca-143">Your storyboard should look as follows:</span></span>
   
    ![][IOS4]
6. <span data-ttu-id="148ca-144">Klikněte na **Domů View Controller** scénáře a ujistěte se, že má **homeViewController** jako jeho **vlastní třída** a **Storyboard ID**pod Identity inspector.</span><span class="sxs-lookup"><span data-stu-id="148ca-144">Click on the **Home View Controller** in storyboard, and make sure it has **homeViewController** as its **Custom Class** and **Storyboard ID** under the Identity inspector.</span></span>
7. <span data-ttu-id="148ca-145">Totéž proveďte pro bitovou kopii řadiče zobrazení jako **imageViewController**.</span><span class="sxs-lookup"><span data-stu-id="148ca-145">Do the same for Image View Controller as **imageViewController**.</span></span>
8. <span data-ttu-id="148ca-146">Pak vytvořte novou třídu řadiče zobrazení s názvem **imageViewController** pro zpracování uživatelského rozhraní, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="148ca-146">Then, create a new View Controller class titled **imageViewController** to handle the UI you just created.</span></span>
9. <span data-ttu-id="148ca-147">V **imageViewController.h**, přidejte následující deklarace rozhraní kontroleru.</span><span class="sxs-lookup"><span data-stu-id="148ca-147">In **imageViewController.h**, add the following to the controller's interface declarations.</span></span> <span data-ttu-id="148ca-148">Nezapomeňte řízení přetažení z bitové kopie zobrazení scénáře pro tyto vlastnosti k propojení dvou:</span><span class="sxs-lookup"><span data-stu-id="148ca-148">Make sure to control-drag from the storyboard image view to these properties to link the two:</span></span>
   
        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;
10. <span data-ttu-id="148ca-149">V **imageViewController.m**, přidejte následující na konci **viewDidload**:</span><span class="sxs-lookup"><span data-stu-id="148ca-149">In **imageViewController.m**, add the following at the end of **viewDidload**:</span></span>
    
        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];
11. <span data-ttu-id="148ca-150">V **AppDelegate.m**, import řadičem bitové kopie, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="148ca-150">In **AppDelegate.m**, import the image controller you created:</span></span>
    
        #import "imageViewController.h"
12. <span data-ttu-id="148ca-151">Přidáte oddíl rozhraní s následující prohlášení:</span><span class="sxs-lookup"><span data-stu-id="148ca-151">Add an interface section with the following declaration:</span></span>
    
        @interface AppDelegate ()
    
        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;
    
        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;
    
        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;
    
        @end
13. <span data-ttu-id="148ca-152">V **AppDelegate**, ujistěte se, že vaše aplikace se zaregistruje pro tichou oznámení v **aplikace: didFinishLaunchingWithOptions**:</span><span class="sxs-lookup"><span data-stu-id="148ca-152">In **AppDelegate**, make sure your app registers for silent notifications in **application: didFinishLaunchingWithOptions**:</span></span>
    
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

1. <span data-ttu-id="148ca-153">Nahraďte následující implementace pro **aplikace: didRegisterForRemoteNotificationsWithDeviceToken** provést storyboard uživatelského rozhraní se změní v úvahu:</span><span class="sxs-lookup"><span data-stu-id="148ca-153">Subsitute in the following implementation for **application:didRegisterForRemoteNotificationsWithDeviceToken** to take the storyboard UI changes into account:</span></span>
   
       // Access navigation controller which is at the root of window
       UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
       // Get home view controller from stack on navigation controller
       homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
       hvc.deviceToken = deviceToken;
2. <span data-ttu-id="148ca-154">Pak přidejte následující metody, které **AppDelegate.m** načíst obrázek z váš koncový bod a odesílat místního oznámení po dokončení načítání.</span><span class="sxs-lookup"><span data-stu-id="148ca-154">Then, add the following methods to **AppDelegate.m** to retrieve the image from your endpoint and send a local notification when retrieval is complete.</span></span> <span data-ttu-id="148ca-155">Ujistěte se, zda jste nahraďte zástupný symbol `{backend endpoint}` s back-end koncový bod:</span><span class="sxs-lookup"><span data-stu-id="148ca-155">Make sure to substitute the placeholder `{backend endpoint}` with your backend endpoint:</span></span>
   
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
                   // From NSData to UIImage
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
   
                       // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
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
           // Add "else if" here to handle more types of rich content such as url, sound files, etc.
       }
3. <span data-ttu-id="148ca-156">Zpracování místního oznámení nad otevřením řadiče zobrazení bitové kopie v **AppDelegate.m** pomocí následujících metod:</span><span class="sxs-lookup"><span data-stu-id="148ca-156">Handle the local notification above by opening up the image view controller in **AppDelegate.m** with the following methods:</span></span>
   
       // Helper: redirect users to image view controller
       - (void)redirectToImageViewWithImage: (UIImage *)img {
           UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
           UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                    bundle: nil];
           imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
           // Pass data/image to image view controller
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
           // Add "else if" here to handle more buttons
       }
   
       // Handle notification setting actions in iOS8
       - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
           // Handle richPush related buttons
           if ([identifier isEqualToString:@"richPushMore"]) {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
           completionHandler();
       }

## <a name="run-the-application"></a><span data-ttu-id="148ca-157">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="148ca-157">Run the Application</span></span>
1. <span data-ttu-id="148ca-158">V XCode spusťte aplikaci na fyzickém zařízení iOS (nabízených oznámení nebude fungovat v simulátoru).</span><span class="sxs-lookup"><span data-stu-id="148ca-158">In XCode, run the app on a physical iOS device (push notifications will not work in the simulator).</span></span>
2. <span data-ttu-id="148ca-159">V aplikaci pro iOS uživatelského rozhraní, zadejte uživatelské jméno a heslo na stejnou hodnotu pro ověřování a klikněte na tlačítko **protokolu v**.</span><span class="sxs-lookup"><span data-stu-id="148ca-159">In the iOS app UI, enter a username and password of the same value for authentication and click **Log In**.</span></span>
3. <span data-ttu-id="148ca-160">Klikněte na tlačítko **odeslat nabízené** a měli byste vidět výstrahu v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="148ca-160">Click **Send push** and you should see an in-app alert.</span></span> <span data-ttu-id="148ca-161">Pokud kliknete na **Další**, je přesměrován zpět na bitovou kopii jste zvolili pro zahrnutí do back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="148ca-161">If you click on **More**, you will be brought to the image you chose to include in your app backend.</span></span>
4. <span data-ttu-id="148ca-162">Můžete také kliknout na **odeslat nabízené** a okamžitě stiskněte tlačítko Domů vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="148ca-162">You can also click **Send push** and immediately press the home button of your device.</span></span> <span data-ttu-id="148ca-163">Ve chvíli obdržíte nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="148ca-163">In a few moments, you will receive a push notification.</span></span> <span data-ttu-id="148ca-164">Pokud klepněte na něm nebo klikněte na tlačítko Další, můžete uvede do vaší aplikace a obsah bohaté image.</span><span class="sxs-lookup"><span data-stu-id="148ca-164">If you tap on it or click More, you will be brought to your app and the rich image content.</span></span>

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
