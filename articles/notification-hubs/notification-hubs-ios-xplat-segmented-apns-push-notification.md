---
title: "Centra oznámení nejnovější novinky kurz – iOS"
description: "Naučte se používat Azure Service Bus Notification Hubs k odesílání oznámení o aktuálních zprávách do zařízení s iOS."
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
ms.openlocfilehash: dc47250db6fb3a2853dae24e02bda236154d93fb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a><span data-ttu-id="09c6e-103">Používání centra oznámení k odesílání novinek</span><span class="sxs-lookup"><span data-stu-id="09c6e-103">Use Notification Hubs to send breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="09c6e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="09c6e-104">Overview</span></span>
<span data-ttu-id="09c6e-105">Toto téma ukazuje, jak používat Azure Notification Hubs k vysílání oznámení o aktuálních zprávách aplikace pro iOS.</span><span class="sxs-lookup"><span data-stu-id="09c6e-105">This topic shows you how to use Azure Notification Hubs to broadcast breaking news notifications to an iOS app.</span></span> <span data-ttu-id="09c6e-106">Po dokončení bude moci zaregistrovat pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie.</span><span class="sxs-lookup"><span data-stu-id="09c6e-106">When complete, you will be able to register for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="09c6e-107">Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení k odeslání do skupiny uživatelů, které jste předtím nebyl deklarovaný zájem o jejich, např. čtečku RSS, aplikace pro Hudba ventilátory, atd.</span><span class="sxs-lookup"><span data-stu-id="09c6e-107">This scenario is a common pattern for many apps where notifications have to be sent to groups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="09c6e-108">Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-108">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in the notification hub.</span></span> <span data-ttu-id="09c6e-109">Pokud oznámení se odesílají do značku, všechna zařízení, která byla zaregistrovaná pro značku obdrží oznámení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-109">When notifications are sent to a tag, all devices that have registered for the tag will receive the notification.</span></span> <span data-ttu-id="09c6e-110">Protože značky jsou jednoduše řetězce, nemají být předem zřízená.</span><span class="sxs-lookup"><span data-stu-id="09c6e-110">Because tags are simply strings, they do not have to be provisioned in advance.</span></span> <span data-ttu-id="09c6e-111">Další informace o značkách najdete v části [směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="09c6e-111">For more information about tags, refer to [Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09c6e-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="09c6e-112">Prerequisites</span></span>
<span data-ttu-id="09c6e-113">Toto téma je založený na aplikaci, kterou jste vytvořili v [Začínáme s Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="09c6e-113">This topic builds on the app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="09c6e-114">Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="09c6e-114">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-to-the-app"></a><span data-ttu-id="09c6e-115">Přidat výběru kategorie do aplikace</span><span class="sxs-lookup"><span data-stu-id="09c6e-115">Add category selection to the app</span></span>
<span data-ttu-id="09c6e-116">Prvním krokem je přidání prvky uživatelského rozhraní do vaší stávající scénáře, který uživateli umožňuje výběr kategorií k registraci.</span><span class="sxs-lookup"><span data-stu-id="09c6e-116">The first step is to add the UI elements to your existing storyboard that enable the user to select categories to register.</span></span> <span data-ttu-id="09c6e-117">Kategorie, které uživatel jsou uloženy v zařízení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-117">The categories selected by a user are stored on the device.</span></span> <span data-ttu-id="09c6e-118">Při spuštění aplikace registrace zařízení se vytvoří v centru oznámení s vybrané kategorie jako značky.</span><span class="sxs-lookup"><span data-stu-id="09c6e-118">When the app starts, a device registration is created in your notification hub with the selected categories as tags.</span></span>

1. <span data-ttu-id="09c6e-119">Ve vašem MainStoryboard_iPhone.storyboard přidejte následující součásti z objektu knihovny:</span><span class="sxs-lookup"><span data-stu-id="09c6e-119">In your MainStoryboard_iPhone.storyboard add the following components from the object library:</span></span>
   
   * <span data-ttu-id="09c6e-120">Popisek s textem "Novinkách."</span><span class="sxs-lookup"><span data-stu-id="09c6e-120">A label with "Breaking News" text,</span></span>
   * <span data-ttu-id="09c6e-121">Popisky se texty kategorie "World", "Politika", "Firemní", "Technologie", "Vědecké účely", "Sports",</span><span class="sxs-lookup"><span data-stu-id="09c6e-121">Labels with category texts "World", "Politics", "Business", "Technology", "Science", "Sports",</span></span>
   * <span data-ttu-id="09c6e-122">Šest přepínače, jeden pro každou kategorii nastavit každý přepínač **stavu** být **vypnout** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-122">Six switches, one per category, set each switch **State** to be **Off** by default.</span></span>
   * <span data-ttu-id="09c6e-123">Jedno tlačítko s označením "Přihlásit k odběru"</span><span class="sxs-lookup"><span data-stu-id="09c6e-123">One button labeled "Subscribe"</span></span>
     
     <span data-ttu-id="09c6e-124">Vaše scénáře by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="09c6e-124">Your storyboard should look as follows:</span></span>
     
     ![][3]
2. <span data-ttu-id="09c6e-125">V editoru pomocníka vytvořit výstupy u všech přepínačů a volat je "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"</span><span class="sxs-lookup"><span data-stu-id="09c6e-125">In the assistant editor, create outlets for all the switches and call them "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"</span></span>
3. <span data-ttu-id="09c6e-126">Vytvoří akci pro vaše tlačítko se nazývá "přihlásit".</span><span class="sxs-lookup"><span data-stu-id="09c6e-126">Create an Action for your button called "subscribe".</span></span> <span data-ttu-id="09c6e-127">Vaše ViewController.h by měl obsahovat následující:</span><span class="sxs-lookup"><span data-stu-id="09c6e-127">Your ViewController.h should contain the following:</span></span>
   
        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;
   
        - (IBAction)subscribe:(id)sender;
4. <span data-ttu-id="09c6e-128">Vytvořte novou **kakao Touch třída** názvem `Notifications`.</span><span class="sxs-lookup"><span data-stu-id="09c6e-128">Create a new **Cocoa Touch Class** called `Notifications`.</span></span> <span data-ttu-id="09c6e-129">V části rozhraní souboru Notifications.h zkopírujte následující kód:</span><span class="sxs-lookup"><span data-stu-id="09c6e-129">Copy the following code in the interface section of the file Notifications.h:</span></span>
   
        @property NSData* deviceToken;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;
   
        - (NSSet*)retrieveCategories;
   
        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
5. <span data-ttu-id="09c6e-130">Přidejte následující direktivy importu Notifications.m:</span><span class="sxs-lookup"><span data-stu-id="09c6e-130">Add the following import directive to Notifications.m:</span></span>
   
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
6. <span data-ttu-id="09c6e-131">Zkopírujte následující kód v oddílu implementace souboru Notifications.m.</span><span class="sxs-lookup"><span data-stu-id="09c6e-131">Copy the following code in the implementation section of the file Notifications.m.</span></span>
   
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



    <span data-ttu-id="09c6e-132">Tato třída se používá místní úložiště pro ukládání a načítání kategorií příspěvků, který obdrží toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-132">This class uses local storage to store and retrieve the categories of news that this device will receive.</span></span> <span data-ttu-id="09c6e-133">Také obsahuje metody pro registraci pro tyto kategorie pomocí [šablony](notification-hubs-templates-cross-platform-push-messages.md) registrace.</span><span class="sxs-lookup"><span data-stu-id="09c6e-133">Also, it contains a method to register for these categories using a [Template](notification-hubs-templates-cross-platform-push-messages.md) registration.</span></span>

1. <span data-ttu-id="09c6e-134">V souboru AppDelegate.h přidejte příkaz import pro Notifications.h a přidejte vlastnost instance třídy oznámení:</span><span class="sxs-lookup"><span data-stu-id="09c6e-134">In the AppDelegate.h file, add an import statement for Notifications.h and add a property for an instance of the Notifications class:</span></span>
   
        #import "Notifications.h"
   
        @property (nonatomic) Notifications* notifications;
2. <span data-ttu-id="09c6e-135">V **didFinishLaunchingWithOptions** metoda v AppDelegate.m, přidat kód pro inicializaci instance oznámení na začátku metody.</span><span class="sxs-lookup"><span data-stu-id="09c6e-135">In the **didFinishLaunchingWithOptions** method in AppDelegate.m, add the code to initialize the notifications instance at the beginning of the method.</span></span>  
   
    <span data-ttu-id="09c6e-136">`HUBNAME`a `HUBLISTENACCESS` (definovanou v souboru hubinfo.h) byste již měli mít `<hub name>` a `<connection string with listen access>` nahradit zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultListenSharedAccessSignature* který jste získali dříve</span><span class="sxs-lookup"><span data-stu-id="09c6e-136">`HUBNAME` and `HUBLISTENACCESS` (defined in hubinfo.h) should already have the `<hub name>` and `<connection string with listen access>` placeholders replaced with your notification hub name and the connection string for *DefaultListenSharedAccessSignature* that you obtained earlier</span></span>
   
        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
   
   > [!NOTE]
   > <span data-ttu-id="09c6e-137">Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci není obvykle zabezpečení, by měl distribuovat klíč pro naslouchání přístup pouze s vaší klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="09c6e-137">Because credentials that are distributed with a client app are not generally secure, you should only distribute the key for listen access with your client app.</span></span> <span data-ttu-id="09c6e-138">Poslechněte umožní přístup k aplikaci zaregistrovat pro oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-138">Listen access enables your app to register for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="09c6e-139">Úplný přístup klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.</span><span class="sxs-lookup"><span data-stu-id="09c6e-139">The full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
3. <span data-ttu-id="09c6e-140">V **didRegisterForRemoteNotificationsWithDeviceToken** metoda v AppDelegate.m, nahraďte kód v metodě následujícím kódem předat token zařízení k třídě oznámení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-140">In the **didRegisterForRemoteNotificationsWithDeviceToken** method in AppDelegate.m, replace the code in the method with the following code to pass the device token to the notifications class.</span></span> <span data-ttu-id="09c6e-141">Třída oznámení provede registrace pro oznámení s kategorií.</span><span class="sxs-lookup"><span data-stu-id="09c6e-141">The notifications class will perform the registering for notifications with the categories.</span></span> <span data-ttu-id="09c6e-142">Pokud uživatel změní výběru kategorií, zavoláme `subscribeWithCategories` metoda v reakci **přihlášení k odběru** tlačítko je aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="09c6e-142">If the user changes category selections, we call the `subscribeWithCategories` method in response to the **subscribe** button to update them.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="09c6e-143">Protože token zařízení, které jsou přiřazené podle APNS Apple Push Notification Service () můžete kdykoli šance, byste měli zaregistrovat pro oznámení často, aby se zabránilo selhání oznámení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-143">Because the device token assigned by the Apple Push Notification Service (APNS) can chance at any time, you should register for notifications frequently to avoid notification failures.</span></span> <span data-ttu-id="09c6e-144">V tomto příkladu se zaregistruje pro oznámení při každém spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="09c6e-144">This example registers for notification every time that the app starts.</span></span> <span data-ttu-id="09c6e-145">Pro aplikace, které jsou často spouštíte více než jednou denně, můžete pravděpodobně přeskočit registraci byla zachována šířka pásma, pokud od předchozí registrace uplynul méně než jeden den.</span><span class="sxs-lookup"><span data-stu-id="09c6e-145">For apps that are run frequently, more than once a day, you can probably skip registration to preserve bandwidth if less than a day has passed since the previous registration.</span></span>
   > 
   > 
   
        self.notifications.deviceToken = deviceToken;
   
        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.
   
        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

    <span data-ttu-id="09c6e-146">V tomto okamžiku došlo by mělo být žádný kód v **didRegisterForRemoteNotificationsWithDeviceToken** metoda.</span><span class="sxs-lookup"><span data-stu-id="09c6e-146">Note that at this point there should be no other code in the **didRegisterForRemoteNotificationsWithDeviceToken** method.</span></span>

1. <span data-ttu-id="09c6e-147">Následující metody již mají být dostupné v AppDelegate.m dokončení [Začínáme s Notification Hubs] [ get-started] kurzu.</span><span class="sxs-lookup"><span data-stu-id="09c6e-147">The following methods should already be present in AppDelegate.m from completing the [Get started with Notification Hubs][get-started] tutorial.</span></span>  <span data-ttu-id="09c6e-148">Pokud ne, přidejte je.</span><span class="sxs-lookup"><span data-stu-id="09c6e-148">If not, add them.</span></span>
   
    <span data-ttu-id="09c6e-149">-(void) MessageBox:(NSString *) nadpis zpráva:(NSString *) messageText {</span><span class="sxs-lookup"><span data-stu-id="09c6e-149">-(void)MessageBox:(NSString *)title message:(NSString *)messageText  {</span></span>
   
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    <span data-ttu-id="09c6e-150">}</span><span class="sxs-lookup"><span data-stu-id="09c6e-150">}</span></span>
   
   * <span data-ttu-id="09c6e-151">(void) aplikace:(UIApplication *) aplikace didReceiveRemoteNotification: (NSDictionary *) informací o uživateli {NSLog (@"% @", informací o uživateli);   [vlastní MessageBox:@"Notification" zprávy: [valueForKey:@"alert [informací o uživateli objectForKey:@"aps"]"]]; }</span><span class="sxs-lookup"><span data-stu-id="09c6e-151">(void)application:(UIApplication *)application didReceiveRemoteNotification:   (NSDictionary *)userInfo {   NSLog(@"%@", userInfo);   [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]]; }</span></span>
   
   <span data-ttu-id="09c6e-152">Tato metoda zpracovává oznámení obdržená při spuštěné aplikaci tím, že zobrazuje jednoduchou **UIAlert**.</span><span class="sxs-lookup"><span data-stu-id="09c6e-152">This method handles notifications received when the app is running by displaying a simple **UIAlert**.</span></span>
2. <span data-ttu-id="09c6e-153">V ViewController.m, přidejte příkaz import pro AppDelegate.h a zkopírujte následující kód do generované XCode **přihlášení k odběru** metoda.</span><span class="sxs-lookup"><span data-stu-id="09c6e-153">In ViewController.m, add a import statement for AppDelegate.h and copy the following code into the XCode-generated **subscribe** method.</span></span> <span data-ttu-id="09c6e-154">Tento kód se bude aktualizovat registraci oznámení používat nové značky kategorie, které uživatelem vybrané v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="09c6e-154">This code will update the notification registration to use the new category tags the user has chosen in the user interface.</span></span>
   
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
   
   <span data-ttu-id="09c6e-155">Tato metoda vytvoří **NSMutableArray** kategorií a používá **oznámení** třídy pro uložení seznamu místní úložiště a registrů, odpovídající značky pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-155">This method creates an **NSMutableArray** of categories and uses the **Notifications** class to store the list in the local storage and registers the corresponding tags with your notification hub.</span></span> <span data-ttu-id="09c6e-156">Při změně kategorií, registrace se znovu vytvoří se nové kategorie.</span><span class="sxs-lookup"><span data-stu-id="09c6e-156">When categories are changed, the registration is recreated with the new categories.</span></span>
3. <span data-ttu-id="09c6e-157">V ViewController.m, přidejte následující kód do **viewDidLoad** metodu a nastavit uživatelské rozhraní založené na dříve uloženou kategoriích.</span><span class="sxs-lookup"><span data-stu-id="09c6e-157">In ViewController.m, add the following code in the **viewDidLoad** method to set the user interface based on the previously saved categories.</span></span>

        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



<span data-ttu-id="09c6e-158">Aplikaci teď můžete uložit sadu kategorií v místním úložišti zařízení používá k registraci do centra oznámení při každém spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="09c6e-158">The app can now store a set of categories in the device local storage used to register with the notification hub whenever the app starts.</span></span>  <span data-ttu-id="09c6e-159">Uživatel může změnit výběr kategorie v modulu runtime a kliknutím **přihlášení k odběru** metoda aktualizace registrace pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-159">The user can change the selection of categories at runtime and click the **subscribe** method to update the registration for the device.</span></span> <span data-ttu-id="09c6e-160">Dále se aktualizovat aplikaci odesílat oznámení o aktuálních zprávách přímo do aplikace.</span><span class="sxs-lookup"><span data-stu-id="09c6e-160">Next, you will update the app to send the breaking news notifications directly in the app itself.</span></span>

## <a name="optional-sending-tagged-notifications"></a><span data-ttu-id="09c6e-161">(volitelné) Odesílání oznámení s příznakem</span><span class="sxs-lookup"><span data-stu-id="09c6e-161">(optional) Sending tagged notifications</span></span>
<span data-ttu-id="09c6e-162">Pokud nemáte přístup k sadě Visual Studio, můžete přeskočit k další části a odesílání oznámení z aplikace.</span><span class="sxs-lookup"><span data-stu-id="09c6e-162">If you don't have access to Visual Studio, you can skip to the next section and send notifications from the app itself.</span></span> <span data-ttu-id="09c6e-163">Můžete také odeslat oznámení správné šablony z [portálu Azure Classic] pomocí karty ladění pro vaše Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-163">You can also send the proper template notification from the [Azure Classic Portal] using the debug tab for your notification hub.</span></span> 

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-the-device"></a><span data-ttu-id="09c6e-164">(volitelné) Odeslat oznámení ze zařízení</span><span class="sxs-lookup"><span data-stu-id="09c6e-164">(optional) Send notifications from the device</span></span>
<span data-ttu-id="09c6e-165">Obvykle bude odesláno oznámení přes službu back-end, ale můžete odesílat oznámení o aktuálních zprávách přímo z aplikace.</span><span class="sxs-lookup"><span data-stu-id="09c6e-165">Normally notifications would be sent by a backend service but, you can send breaking news notifications directly from the app.</span></span> <span data-ttu-id="09c6e-166">K tomu budeme aktualizovat `SendNotificationRESTAPI` metoda, která jsme definovali v [Začínáme s Notification Hubs] [ get-started] kurzu.</span><span class="sxs-lookup"><span data-stu-id="09c6e-166">To do this we will update the `SendNotificationRESTAPI` method that we defined in the [Get started with Notification Hubs][get-started] tutorial.</span></span>

1. <span data-ttu-id="09c6e-167">V aktualizaci ViewController.m `SendNotificationRESTAPI` jako metodu následuje tak, aby přijímá parametr pro značku kategorie a odešle správné [šablony](notification-hubs-templates-cross-platform-push-messages.md) oznámení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-167">In ViewController.m update the `SendNotificationRESTAPI` method as follows so that it accepts a parameter for the category tag and sends the proper [template](notification-hubs-templates-cross-platform-push-messages.md) notification.</span></span>
   
        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];
   
            NSString *json;
   
            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];
   
            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
   
            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];
   
            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];
   
            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send the REST request
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
2. <span data-ttu-id="09c6e-168">V aktualizaci ViewController.m **odeslat oznámení** akce, jak je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="09c6e-168">In ViewController.m update the **Send Notification** action as shown in the code that follows.</span></span> <span data-ttu-id="09c6e-169">Tak, aby se odesílání oznámení pomocí jednotlivé značky jednotlivě a odeslat na více platforem.</span><span class="sxs-lookup"><span data-stu-id="09c6e-169">So that it will send the notifications using each tag individually and send to multiple platforms.</span></span>

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



1. <span data-ttu-id="09c6e-170">Znovu sestavte projekt a ujistěte se, že máte žádné chyby sestavení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-170">Rebuild your project and make sure you have no build errors.</span></span>

## <a name="run-the-app-and-generate-notifications"></a><span data-ttu-id="09c6e-171">Spusťte aplikaci a generovat upozornění</span><span class="sxs-lookup"><span data-stu-id="09c6e-171">Run the app and generate notifications</span></span>
1. <span data-ttu-id="09c6e-172">Klikněte na tlačítko spustit pro sestavení projektu a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="09c6e-172">Press the Run button to build the project and start the app.</span></span> <span data-ttu-id="09c6e-173">Vyberte některé zprávy možnosti ukončování řádků pro přihlášení k odběru a potom stiskněte klávesu **přihlásit k odběru** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="09c6e-173">Select some breaking news options to subscribe to and then press the **Subscribe** button.</span></span> <span data-ttu-id="09c6e-174">Měli byste vidět, zobrazí se dialogové okno oznamující, že se odebírá oznámení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-174">You should see a dialog indicating the notifications have been subscribed to.</span></span>
   
    ![][1]
   
    <span data-ttu-id="09c6e-175">Pokud vyberete **přihlásit k odběru**, převede vybraných kategorií značky a požaduje novou registraci zařízení pro vybranou značky z centra oznámení aplikace.</span><span class="sxs-lookup"><span data-stu-id="09c6e-175">When you choose **Subscribe**, the app converts the selected categories into tags and requests a new device registration for the selected tags from the notification hub.</span></span>
2. <span data-ttu-id="09c6e-176">Zadejte zprávu k odeslání jako novinek stiskněte **odeslat oznámení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="09c6e-176">Enter a message to be sent as breaking news then press the **Send Notification** button.</span></span> <span data-ttu-id="09c6e-177">Alternativně spusťte konzolové aplikace .NET pro generování oznámení.</span><span class="sxs-lookup"><span data-stu-id="09c6e-177">Alternatively, run the .NET console app to generate notifications.</span></span>
   
    ![][2]
3. <span data-ttu-id="09c6e-178">Každé zařízení předplatné nejnovější zprávy přes se zobrazí oznámení o aktuálních zprávách, které jste poslali.</span><span class="sxs-lookup"><span data-stu-id="09c6e-178">Each device subscribed to breaking news will receive the breaking news notifications you just sent.</span></span>

## <a name="next-steps"></a><span data-ttu-id="09c6e-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="09c6e-179">Next steps</span></span>
<span data-ttu-id="09c6e-180">V tomto kurzu jsme zjistili, jak k vysílání novinek podle kategorie.</span><span class="sxs-lookup"><span data-stu-id="09c6e-180">In this tutorial we learned how to broadcast breaking news by category.</span></span> <span data-ttu-id="09c6e-181">Vezměte v úvahu dokončení jednu z následujících kurzů upozorňující na jiné pokročilé scénáře centra oznámení:</span><span class="sxs-lookup"><span data-stu-id="09c6e-181">Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="09c6e-182">**[Použití centra oznámení k vysílání lokalizované novinek]**</span><span class="sxs-lookup"><span data-stu-id="09c6e-182">**[Use Notification Hubs to broadcast localized breaking news]**</span></span>
  
    <span data-ttu-id="09c6e-183">Zjistěte, jak rozšířit aplikace nejnovější zprávy k povolení odesílání lokalizované upozornění.</span><span class="sxs-lookup"><span data-stu-id="09c6e-183">Learn how to expand the breaking news app to enable sending localized notifications.</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
<span data-ttu-id="09c6e-184">[Použití centra oznámení k vysílání lokalizované novinek]: notification-hubs-ios-xplat-localized-apns-push-notification.md</span><span class="sxs-lookup"><span data-stu-id="09c6e-184">[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-ios-xplat-localized-apns-push-notification.md</span></span>
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
<span data-ttu-id="09c6e-185">[portálu Azure Classic]: https://manage.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="09c6e-185">[Azure Classic Portal]: https://manage.windowsazure.com</span></span>
