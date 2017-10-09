
<span data-ttu-id="ae2ac-101">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="ae2ac-101">**Objective-C**:</span></span>

1. <span data-ttu-id="ae2ac-102">V **QSAppDelegate.m**, import hello iOS SDK a **QSTodoService.h**:</span><span class="sxs-lookup"><span data-stu-id="ae2ac-102">In **QSAppDelegate.m**, import hello iOS SDK and **QSTodoService.h**:</span></span>
   
        #import <MicrosoftAzureMobile/MicrosoftAzureMobile.h>
        #import "QSTodoService.h"
2. <span data-ttu-id="ae2ac-103">V `didFinishLaunchingWithOptions` v **QSAppDelegate.m**, následující hello vložení řádků těsně před `return YES;`:</span><span class="sxs-lookup"><span data-stu-id="ae2ac-103">In `didFinishLaunchingWithOptions` in **QSAppDelegate.m**, insert hello following lines right before `return YES;`:</span></span>
   
        UIUserNotificationSettings* notificationSettings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeAlert | UIUserNotificationTypeBadge | UIUserNotificationTypeSound categories:nil];
        [[UIApplication sharedApplication] registerUserNotificationSettings:notificationSettings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
3. <span data-ttu-id="ae2ac-104">V **QSAppDelegate.m**, přidejte následující metody obslužná rutina hello.</span><span class="sxs-lookup"><span data-stu-id="ae2ac-104">In **QSAppDelegate.m**, add hello following handler methods.</span></span> <span data-ttu-id="ae2ac-105">Vaše aplikace je teď aktualizovaný toosupport nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="ae2ac-105">Your app is now updated toosupport push notifications.</span></span> 
   
        // Registration with APNs is successful
        - (void)application:(UIApplication *)application
        didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
   
            QSTodoService *todoService = [QSTodoService defaultService];
            MSClient *client = todoService.client;
   
            [client.push registerDeviceToken:deviceToken completion:^(NSError *error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
            }];
        }
   
        // Handle any failure tooregister
        - (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:
        (NSError *)error {
            NSLog(@"Failed tooregister for remote notifications: %@", error);
        }
   
        // Use userInfo in hello payload toodisplay an alert.
        - (void)application:(UIApplication *)application
              didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
   
            NSDictionary *apsPayload = userInfo[@"aps"];
            NSString *alertString = apsPayload[@"alert"];
   
            // Create alert with notification content.
            UIAlertController *alertController = [UIAlertController
                                          alertControllerWithTitle:@"Notification"
                                          message:alertString
                                          preferredStyle:UIAlertControllerStyleAlert];
   
            UIAlertAction *cancelAction = [UIAlertAction
                                           actionWithTitle:NSLocalizedString(@"Cancel", @"Cancel")
                                           style:UIAlertActionStyleCancel
                                           handler:^(UIAlertAction *action)
                                           {
                                               NSLog(@"Cancel");
                                           }];
   
            UIAlertAction *okAction = [UIAlertAction
                                       actionWithTitle:NSLocalizedString(@"OK", @"OK")
                                       style:UIAlertActionStyleDefault
                                       handler:^(UIAlertAction *action)
                                       {
                                           NSLog(@"OK");
                                       }];
   
            [alertController addAction:cancelAction];
            [alertController addAction:okAction];
   
            // Get current view controller.
            UIViewController *currentViewController = [[[[UIApplication sharedApplication] delegate] window] rootViewController];
            while (currentViewController.presentedViewController)
            {
                currentViewController = currentViewController.presentedViewController;
            }
   
            // Display alert.
            [currentViewController presentViewController:alertController animated:YES completion:nil];
   
        }

<span data-ttu-id="ae2ac-106">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="ae2ac-106">**Swift**:</span></span>

1. <span data-ttu-id="ae2ac-107">Přidejte soubor **ClientManager.swift** s hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="ae2ac-107">Add file **ClientManager.swift** with hello following contents.</span></span> <span data-ttu-id="ae2ac-108">Nahraďte *AppUrl %* s adresou URL back-end mobilní aplikace Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="ae2ac-108">Replace *%AppUrl%* with hello URL of hello Azure Mobile App backend.</span></span>
   
        class ClientManager {
            static let sharedClient = MSClient(applicationURLString: "%AppUrl%")
        }
2. <span data-ttu-id="ae2ac-109">V **ToDoTableViewController.swift**, nahraďte hello `let client` řádek, který inicializuje `MSClient` tento řádek:</span><span class="sxs-lookup"><span data-stu-id="ae2ac-109">In **ToDoTableViewController.swift**, replace hello `let client` line that initializes an `MSClient` with this line:</span></span>
   
        let client = ClientManager.sharedClient
3. <span data-ttu-id="ae2ac-110">V **AppDelegate.swift**, nahraďte text hello `func application` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ae2ac-110">In **AppDelegate.swift**, replace hello body of `func application` as follows:</span></span>
   
        func application(application: UIApplication,
          didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
           application.registerUserNotificationSettings(
               UIUserNotificationSettings(forTypes: [.Alert, .Badge, .Sound],
                   categories: nil))
           application.registerForRemoteNotifications()
           return true
        }
4. <span data-ttu-id="ae2ac-111">V **AppDelegate.swift**, přidejte následující metody obslužná rutina hello.</span><span class="sxs-lookup"><span data-stu-id="ae2ac-111">In **AppDelegate.swift**, add hello following handler methods.</span></span> <span data-ttu-id="ae2ac-112">Vaše aplikace je teď aktualizovaný toosupport nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="ae2ac-112">Your app is now updated toosupport push notifications.</span></span>
   
        func application(application: UIApplication,
           didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData) {
            ClientManager.sharedClient.push?.registerDeviceToken(deviceToken) { error in
                print("Error registering for notifications: ", error?.description)
            }
        }
   
        func application(application: UIApplication,
           didFailToRegisterForRemoteNotificationsWithError error: NSError) {
            print("Failed tooregister for remote notifications: ", error.description)
        }
   
        func application(application: UIApplication,
           didReceiveRemoteNotification userInfo: [NSObject: AnyObject]) {
   
            print(userInfo)
   
            let apsNotification = userInfo["aps"] as? NSDictionary
            let apsString       = apsNotification?["alert"] as? String
   
            let alert = UIAlertController(title: aaa"Alert", message: apsString, preferredStyle: .Alert)
            let okAction = UIAlertAction(title: aaa"OK", style: .Default) { _ in
                print("OK")
            }
            let cancelAction = UIAlertAction(title: aaa"Cancel", style: .Default) { _ in
                print("Cancel")
            }
   
            alert.addAction(okAction)
            alert.addAction(cancelAction)
   
            var currentViewController = self.window?.rootViewController
            while currentViewController?.presentedViewController != nil {
                currentViewController = currentViewController?.presentedViewController
            }
   
            currentViewController?.presentViewController(alert, animated: true) {}
   
        }

