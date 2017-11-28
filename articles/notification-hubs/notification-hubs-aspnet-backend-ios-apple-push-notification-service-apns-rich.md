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
# <a name="azure-notification-hubs-rich-push"></a><span data-ttu-id="89cd7-104">Azure Notification Hubs bohaté Push</span><span class="sxs-lookup"><span data-stu-id="89cd7-104">Azure Notification Hubs Rich Push</span></span>
## <a name="overview"></a><span data-ttu-id="89cd7-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="89cd7-105">Overview</span></span>
<span data-ttu-id="89cd7-106">V pořadí tooengage uživatelům s rychlých bohaté obsah může být vhodné aplikace toopush nad rámec prostý text.</span><span class="sxs-lookup"><span data-stu-id="89cd7-106">In order tooengage users with instant rich contents, an application might want toopush beyond plain text.</span></span> <span data-ttu-id="89cd7-107">Tato oznámení zvýšit úroveň, akce uživatelů a existuje obsah, jako jsou adresy URL, zvuky, Image nebo kupóny a další.</span><span class="sxs-lookup"><span data-stu-id="89cd7-107">These notifications promote user interactions and  present content such as urls, sounds, images/coupons, and more.</span></span> <span data-ttu-id="89cd7-108">V tomto kurzu vychází hello [upozornění uživatelů](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tématu a ukazuje, jak toosend nabízená oznámení, které jsou částí (například obrázek).</span><span class="sxs-lookup"><span data-stu-id="89cd7-108">This tutorial builds on hello [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) topic, and shows how toosend push notifications that incorporate payloads (for example, image).</span></span>

<span data-ttu-id="89cd7-109">V tomto kurzu je kompatibilní s iOS 7 a 8.</span><span class="sxs-lookup"><span data-stu-id="89cd7-109">This tutorial is compatible with iOS 7 & 8.</span></span>

  ![][IOS1]

<span data-ttu-id="89cd7-110">Na vysoké úrovni:</span><span class="sxs-lookup"><span data-stu-id="89cd7-110">At a high level:</span></span>

1. <span data-ttu-id="89cd7-111">back-end aplikace Hello:</span><span class="sxs-lookup"><span data-stu-id="89cd7-111">hello app backend:</span></span>
   * <span data-ttu-id="89cd7-112">Úložiště hello bohaté datová část (v tomto případě obrázek) v úložišti databáze nebo místní hello back-end</span><span class="sxs-lookup"><span data-stu-id="89cd7-112">Stores hello rich payload (in this case, image) in hello backend database/local storage</span></span>
   * <span data-ttu-id="89cd7-113">Odešle ID tohoto zařízení toohello bohaté oznámení</span><span class="sxs-lookup"><span data-stu-id="89cd7-113">Sends ID of this rich notification toohello device</span></span>
2. <span data-ttu-id="89cd7-114">Aplikace na zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="89cd7-114">App on hello device:</span></span>
   * <span data-ttu-id="89cd7-115">Kontakty hello požaduje hello bohaté datovou část s ID hello obdrží back-end</span><span class="sxs-lookup"><span data-stu-id="89cd7-115">Contacts hello backend requesting hello rich payload with hello ID it receives</span></span>
   * <span data-ttu-id="89cd7-116">Při načítání dat je dokončena a ukazuje datovou část hello okamžitě, když uživatel klepnutím na další toolearn odešle oznámení uživatelům na zařízení hello</span><span class="sxs-lookup"><span data-stu-id="89cd7-116">Sends users notifications on hello device when data retrieval is complete, and shows hello payload immediately when users tap toolearn more</span></span>

## <a name="webapi-project"></a><span data-ttu-id="89cd7-117">WebAPI projektu</span><span class="sxs-lookup"><span data-stu-id="89cd7-117">WebAPI Project</span></span>
1. <span data-ttu-id="89cd7-118">V sadě Visual Studio otevřete hello **AppBackend** projekt, který jste vytvořili v hello [upozornění uživatelů](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="89cd7-118">In Visual Studio, open hello **AppBackend** project that you created in hello [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
2. <span data-ttu-id="89cd7-119">Získat bitovou kopii chcete vytvořit toonotify uživatelů s a vložte ho **img** složky v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="89cd7-119">Obtain an image you would like toonotify users with, and put it in an **img** folder in your project directory.</span></span>
3. <span data-ttu-id="89cd7-120">Klikněte na tlačítko **zobrazit všechny soubory** v hello Průzkumníku řešení a klikněte pravým tlačítkem na složku hello příliš**zahrnout do projektu**.</span><span class="sxs-lookup"><span data-stu-id="89cd7-120">Click **Show All Files** in hello Solution Explorer, and right-click hello folder too**Include In Project**.</span></span>
4. <span data-ttu-id="89cd7-121">Vybraná Image hello, změnit jeho akce sestavení v okně Vlastnosti příliš**vložený prostředek**.</span><span class="sxs-lookup"><span data-stu-id="89cd7-121">With hello image selected, change its Build Action in Properties window too**Embedded Resource**.</span></span>
   
    ![][IOS2]
5. <span data-ttu-id="89cd7-122">V **Notifications.cs**, přidejte hello následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="89cd7-122">In **Notifications.cs**, add hello following using statement:</span></span>
   
        using System.Reflection;
6. <span data-ttu-id="89cd7-123">Aktualizace hello celou **oznámení** se hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="89cd7-123">Update hello whole **Notifications** class with hello following code.</span></span> <span data-ttu-id="89cd7-124">Být jisti tooreplace zástupné symboly hello s oznámení centra pověření a název souboru obrázku.</span><span class="sxs-lookup"><span data-stu-id="89cd7-124">Be sure tooreplace hello placeholders with your notification hub credentials and image file name.</span></span>
   
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
   > <span data-ttu-id="89cd7-125">(volitelné) Odkazovat příliš[jak tooembed a přístup k prostředkům pomocí Visual C#](http://support.microsoft.com/kb/319292) Další informace o tom, tooadd a získat prostředky projektu.</span><span class="sxs-lookup"><span data-stu-id="89cd7-125">(optional) Refer too[How tooembed and access resources by using Visual C#](http://support.microsoft.com/kb/319292) for more information on how tooadd and obtain project resources.</span></span>
   > 
   > 
7. <span data-ttu-id="89cd7-126">V **NotificationsController.cs**, znovu definovat **NotificationsController** s hello následující fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="89cd7-126">In **NotificationsController.cs**, redefine **NotificationsController**  with hello following snippets.</span></span> <span data-ttu-id="89cd7-127">To odešle toodevice id počáteční tichou bohaté oznámení a umožňuje klienta načtení obrázku:</span><span class="sxs-lookup"><span data-stu-id="89cd7-127">This sends an initial silent rich notification id toodevice and allows client-side retrieval of image:</span></span>
   
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
8. <span data-ttu-id="89cd7-128">Nyní nasadíme znovu tuto aplikaci tooan webu Azure v pořadí toomake je přístupná ze všech zařízení.</span><span class="sxs-lookup"><span data-stu-id="89cd7-128">Now we will re-deploy this app tooan Azure Website in order toomake it accessible from all devices.</span></span> <span data-ttu-id="89cd7-129">Klikněte pravým tlačítkem na hello **AppBackend** projektu a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="89cd7-129">Right-click on hello **AppBackend** project and select **Publish**.</span></span>
9. <span data-ttu-id="89cd7-130">Vyberte web Azure jako váš cíl publikování.</span><span class="sxs-lookup"><span data-stu-id="89cd7-130">Select Azure Website as your publish target.</span></span> <span data-ttu-id="89cd7-131">Přihlaste se pomocí účtu Azure a vyberte stávajícího nebo nového webu a poznamenejte si hello **cílová adresa URL** vlastnost hello **připojení** kartě. Označujeme toothis adresu URL jako vaše *koncový bod back-end* dál v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="89cd7-131">Log in with your Azure account and select an existing or new Website, and make a note of hello **destination URL** property in hello **Connection** tab. We will refer toothis URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="89cd7-132">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="89cd7-132">Click **Publish**.</span></span>

## <a name="modify-hello-ios-project"></a><span data-ttu-id="89cd7-133">Upravit projektu iOS hello</span><span class="sxs-lookup"><span data-stu-id="89cd7-133">Modify hello iOS project</span></span>
<span data-ttu-id="89cd7-134">Teď, když jste změnili jenom hello vaší aplikace back-end toosend *id* oznámení, změníte toto id vaší toohandle aplikace iOS a načíst hello bohaté zprávy z vaší back-end.</span><span class="sxs-lookup"><span data-stu-id="89cd7-134">Now that you have modified your app backend toosend just hello *id* of a notification, you will change your iOS app toohandle that id and retrieve hello rich message from your backend.</span></span>

1. <span data-ttu-id="89cd7-135">Otevřete projekt iOS a povolte vzdálené oznámení tak, že budete cíl tooyour hlavní aplikace v hello **cíle** části.</span><span class="sxs-lookup"><span data-stu-id="89cd7-135">Open your iOS project, and enable remote notifications by going tooyour main app target in hello **Targets** section.</span></span>
2. <span data-ttu-id="89cd7-136">Klikněte na **možnosti**, zapnout **režimy pozadí**a zkontrolujte hello **vzdáleného oznámení** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="89cd7-136">Click on **Capabilities**, turn on **Background Modes**, and check hello **Remote Notifications** checkbox.</span></span>
   
    ![][IOS3]
3. <span data-ttu-id="89cd7-137">Přejděte příliš**Main.storyboard**a zajistěte, aby byla řadič zobrazení (tooas zmíněných Domů řadiče zobrazení v tomto kurzu) z [upozornit uživatele](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="89cd7-137">Go too**Main.storyboard**, and make sure you have a View Controller (refered tooas Home View Controller in this tutorial) from [Notify User](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
4. <span data-ttu-id="89cd7-138">Přidat **navigační řadič** tooyour scénáře a Ctrl táhnout toomake View Controller tooHome ho hello **kořenový zobrazení** navigace.</span><span class="sxs-lookup"><span data-stu-id="89cd7-138">Add a **Navigation Controller** tooyour storyboard, and control-drag tooHome View Controller toomake it hello **root view** of navigation.</span></span> <span data-ttu-id="89cd7-139">Ujistěte se, zda text hello **je počáteční View Controller** v atributech inspector je vybraná jenom hello navigační řadiče.</span><span class="sxs-lookup"><span data-stu-id="89cd7-139">Make sure hello **Is Initial View Controller** in Attributes inspector is selected for hello Navigation Controller only.</span></span>
5. <span data-ttu-id="89cd7-140">Přidat **View Controller** toostoryboard a přidejte **Image zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="89cd7-140">Add a **View Controller** toostoryboard and add an **Image View**.</span></span> <span data-ttu-id="89cd7-141">Toto je stránka hello, se zobrazí uživatelům, jakmile toolearn vybírá informace kliknutím na hello notifiication.</span><span class="sxs-lookup"><span data-stu-id="89cd7-141">This is hello page users will see once they choose toolearn more by clicking on hello notifiication.</span></span> <span data-ttu-id="89cd7-142">Vaše scénáře by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="89cd7-142">Your storyboard should look as follows:</span></span>
   
    ![][IOS4]
6. <span data-ttu-id="89cd7-143">Klikněte na hello **Domů View Controller** scénáře a ujistěte se, že má **homeViewController** jako jeho **vlastní třída** a **Storyboard ID**pod hello Identity inspector.</span><span class="sxs-lookup"><span data-stu-id="89cd7-143">Click on hello **Home View Controller** in storyboard, and make sure it has **homeViewController** as its **Custom Class** and **Storyboard ID** under hello Identity inspector.</span></span>
7. <span data-ttu-id="89cd7-144">Hello stejné pro bitovou kopii řadiče zobrazení jako **imageViewController**.</span><span class="sxs-lookup"><span data-stu-id="89cd7-144">Do hello same for Image View Controller as **imageViewController**.</span></span>
8. <span data-ttu-id="89cd7-145">Pak vytvořte novou třídu řadiče zobrazení s názvem **imageViewController** toohandle hello uživatelského rozhraní, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="89cd7-145">Then, create a new View Controller class titled **imageViewController** toohandle hello UI you just created.</span></span>
9. <span data-ttu-id="89cd7-146">V **imageViewController.h**, přidejte následující deklarace rozhraní toohello řadiče hello.</span><span class="sxs-lookup"><span data-stu-id="89cd7-146">In **imageViewController.h**, add hello following toohello controller's interface declarations.</span></span> <span data-ttu-id="89cd7-147">Ujistěte se, že toocontrol přetažení z hello storyboard image zobrazení toothese vlastnosti toolink hello dva:</span><span class="sxs-lookup"><span data-stu-id="89cd7-147">Make sure toocontrol-drag from hello storyboard image view toothese properties toolink hello two:</span></span>
   
        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;
10. <span data-ttu-id="89cd7-148">V **imageViewController.m**, přidejte následující hello na konci hello **viewDidload**:</span><span class="sxs-lookup"><span data-stu-id="89cd7-148">In **imageViewController.m**, add hello following at hello end of **viewDidload**:</span></span>
    
        // Display hello UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];
11. <span data-ttu-id="89cd7-149">V **AppDelegate.m**, import hello image řadiče jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="89cd7-149">In **AppDelegate.m**, import hello image controller you created:</span></span>
    
        #import "imageViewController.h"
12. <span data-ttu-id="89cd7-150">Přidáte oddíl, který rozhraní s hello následující prohlášení:</span><span class="sxs-lookup"><span data-stu-id="89cd7-150">Add an interface section with hello following declaration:</span></span>
    
        @interface AppDelegate ()
    
        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;
    
        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;
    
        // Redirect tooImage View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;
    
        @end
13. <span data-ttu-id="89cd7-151">V **AppDelegate**, ujistěte se, že vaše aplikace se zaregistruje pro tichou oznámení v **aplikace: didFinishLaunchingWithOptions**:</span><span class="sxs-lookup"><span data-stu-id="89cd7-151">In **AppDelegate**, make sure your app registers for silent notifications in **application: didFinishLaunchingWithOptions**:</span></span>
    
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

1. <span data-ttu-id="89cd7-152">Nahraďte v hello následující implementace pro **aplikace: didRegisterForRemoteNotificationsWithDeviceToken** tootake hello storyboard uživatelského rozhraní se změní v úvahu:</span><span class="sxs-lookup"><span data-stu-id="89cd7-152">Subsitute in hello following implementation for **application:didRegisterForRemoteNotificationsWithDeviceToken** tootake hello storyboard UI changes into account:</span></span>
   
       // Access navigation controller which is at hello root of window
       UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
       // Get home view controller from stack on navigation controller
       homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
       hvc.deviceToken = deviceToken;
2. <span data-ttu-id="89cd7-153">Pak přidejte následující metody příliš hello**AppDelegate.m** tooretrieve hello bitovou kopii z váš koncový bod a odesílání místní oznámení po dokončení načítání.</span><span class="sxs-lookup"><span data-stu-id="89cd7-153">Then, add hello following methods too**AppDelegate.m** tooretrieve hello image from your endpoint and send a local notification when retrieval is complete.</span></span> <span data-ttu-id="89cd7-154">Ujistěte se, že zástupný symbol hello toosubstitute `{backend endpoint}` s back-end koncový bod:</span><span class="sxs-lookup"><span data-stu-id="89cd7-154">Make sure toosubstitute hello placeholder `{backend endpoint}` with your backend endpoint:</span></span>
   
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
3. <span data-ttu-id="89cd7-155">Zpracování hello místního oznámení nad otevřením řadiče zobrazení hello bitové kopie v **AppDelegate.m** s hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="89cd7-155">Handle hello local notification above by opening up hello image view controller in **AppDelegate.m** with hello following methods:</span></span>
   
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

## <a name="run-hello-application"></a><span data-ttu-id="89cd7-156">Spustit hello aplikace</span><span class="sxs-lookup"><span data-stu-id="89cd7-156">Run hello Application</span></span>
1. <span data-ttu-id="89cd7-157">V XCode spusťte aplikaci hello na fyzickém zařízení iOS (nabízených oznámení nebude fungovat v simulátoru hello).</span><span class="sxs-lookup"><span data-stu-id="89cd7-157">In XCode, run hello app on a physical iOS device (push notifications will not work in hello simulator).</span></span>
2. <span data-ttu-id="89cd7-158">V aplikaci pro iOS hello uživatelského rozhraní, zadejte uživatelské jméno a heslo hello stejné hodnoty pro ověřování a klikněte na tlačítko **protokolu v**.</span><span class="sxs-lookup"><span data-stu-id="89cd7-158">In hello iOS app UI, enter a username and password of hello same value for authentication and click **Log In**.</span></span>
3. <span data-ttu-id="89cd7-159">Klikněte na tlačítko **odeslat nabízené** a měli byste vidět výstrahu v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="89cd7-159">Click **Send push** and you should see an in-app alert.</span></span> <span data-ttu-id="89cd7-160">Pokud kliknete na **Další**, bude možné přepnout do toohello image, které jste zvolili tooinclude v back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="89cd7-160">If you click on **More**, you will be brought toohello image you chose tooinclude in your app backend.</span></span>
4. <span data-ttu-id="89cd7-161">Můžete také kliknout na **odeslat nabízené** a okamžitě stiskněte tlačítko Domů hello vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="89cd7-161">You can also click **Send push** and immediately press hello home button of your device.</span></span> <span data-ttu-id="89cd7-162">Ve chvíli obdržíte nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="89cd7-162">In a few moments, you will receive a push notification.</span></span> <span data-ttu-id="89cd7-163">Klepněte na něm nebo klikněte na tlačítko Další, bude začlenění obsahu tooyour aplikace a hello bohaté bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="89cd7-163">If you tap on it or click More, you will be brought tooyour app and hello rich image content.</span></span>

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
