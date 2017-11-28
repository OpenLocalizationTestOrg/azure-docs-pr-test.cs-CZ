---
title: "aaaSending nabízená oznámení tooiOS pomocí Azure Notification Hubs | Microsoft Docs"
description: "V tomto kurzu zjistíte, jak Azure Notification Hubs toosend toouse nabízená oznámení tooan iOS aplikace."
services: notification-hubs
documentationcenter: ios
keywords: "nabízené oznámení;nabízená oznámení;nabízená oznámení ios"
author: ysxu
manager: erikre
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: d8bb47fee4c229b3ed2a7a4dbff25a56a7a7d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooios-with-azure-notification-hubs"></a><span data-ttu-id="393a3-104">Odesílání nabízených oznámení tooiOS pomocí Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="393a3-104">Sending push notifications tooiOS with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="393a3-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="393a3-105">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="393a3-106">toocomplete tento kurz, musíte mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="393a3-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="393a3-107">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="393a3-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="393a3-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="393a3-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="393a3-109">Tento kurz ukazuje, jak Azure Notification Hubs toosend toouse nabízená oznámení tooan iOS aplikace.</span><span class="sxs-lookup"><span data-stu-id="393a3-109">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooan iOS application.</span></span> <span data-ttu-id="393a3-110">Vytvoříte prázdnou aplikaci iOS, která přijímá nabízená oznámení pomocí hello [služby nabízených oznámení Apple (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html).</span><span class="sxs-lookup"><span data-stu-id="393a3-110">You'll create a blank iOS app that receives push notifications by using hello [Apple Push Notification service (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html).</span></span> 

<span data-ttu-id="393a3-111">Jakmile budete hotovi, budete moct toouse vaše toobroadcast centra oznámení nabízená oznámení tooall hello zařízení používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="393a3-111">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="393a3-112">Než začnete</span><span class="sxs-lookup"><span data-stu-id="393a3-112">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="393a3-113">Hello dokončit kód v tomto kurzu lze najít [na Githubu](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="393a3-113">hello completed code for this tutorial can be found [on GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="393a3-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="393a3-114">Prerequisites</span></span>
<span data-ttu-id="393a3-115">Tento kurz vyžaduje hello následující:</span><span class="sxs-lookup"><span data-stu-id="393a3-115">This tutorial requires hello following:</span></span>

* <span data-ttu-id="393a3-116">[Mobile Services iOS SDK verze 1.2.4]</span><span class="sxs-lookup"><span data-stu-id="393a3-116">[Mobile Services iOS SDK version 1.2.4]</span></span>
* <span data-ttu-id="393a3-117">Poslední verze jazyka [Xcode]</span><span class="sxs-lookup"><span data-stu-id="393a3-117">Latest version of [Xcode]</span></span>
* <span data-ttu-id="393a3-118">Zařízení podporující iOS 8 (nebo novější verze)</span><span class="sxs-lookup"><span data-stu-id="393a3-118">An iOS 8 (or later version)-capable device</span></span>
* <span data-ttu-id="393a3-119">Členství v [programu pro vývojáře Apple](https://developer.apple.com/programs/).</span><span class="sxs-lookup"><span data-stu-id="393a3-119">[Apple Developer Program](https://developer.apple.com/programs/) membership.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="393a3-120">Z důvodu požadavky na konfiguraci pro nabízená oznámení musíte nasadit a otestovat nabízená oznámení na fyzickém zařízení iOS (iPhone nebo iPad) namísto simulátoru iOS hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-120">Because of configuration requirements for push notifications, you must deploy and test push notifications on a physical iOS device (iPhone or iPad) instead of hello iOS Simulator.</span></span>
  > 
  > 

<span data-ttu-id="393a3-121">Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro aplikace iOS.</span><span class="sxs-lookup"><span data-stu-id="393a3-121">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for iOS apps.</span></span>

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a><span data-ttu-id="393a3-122">Konfigurace centra oznámení pro nabízená oznámení iOS</span><span class="sxs-lookup"><span data-stu-id="393a3-122">Configure your Notification Hub for iOS push notifications</span></span>
<span data-ttu-id="393a3-123">Tato část vás provede vytvořením nového centra oznámení a konfiguraci ověřování pomocí služby APNS pomocí hello **.p12** nabízený certifikát, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="393a3-123">This section walks you through creating a new notification hub and configuring authentication with APNS using hello **.p12** push certificate that you created.</span></span> <span data-ttu-id="393a3-124">Pokud chcete toouse Centrum oznámení, které jste už vytvořili, můžete přeskočit toostep 5.</span><span class="sxs-lookup"><span data-stu-id="393a3-124">If you want toouse a notification hub that you have already created, you can skip toostep 5.</span></span>

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p><span data-ttu-id="393a3-125">Klikněte na tlačítko hello <b>služby oznámení</b> tlačítka na hello <b>nastavení</b> okně vyberte <b>Apple (APNS)</b>.</span><span class="sxs-lookup"><span data-stu-id="393a3-125">Click hello <b>Notification Services</b> button in hello <b>Settings</b> blade, then select <b>Apple (APNS)</b>.</span></span> <span data-ttu-id="393a3-126">Klikněte na <b>nahrát certifikát</b> a vyberte hello <b>.p12</b> soubor, který jste dříve exportovali.</span><span class="sxs-lookup"><span data-stu-id="393a3-126">Click on <b>Upload Certificate</b> and select hello <b>.p12</b> file that you exported earlier.</span></span> <span data-ttu-id="393a3-127">Ujistěte se, že zadáváte správné heslo hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-127">Make sure you also specify hello correct password.</span></span></p>

<p><span data-ttu-id="393a3-128">Ujistěte se, že tooselect <b>izolovaného prostoru</b> režimu, protože se jedná o vývoj.</span><span class="sxs-lookup"><span data-stu-id="393a3-128">Make sure tooselect <b>Sandbox</b> mode since this is for development.</span></span> <span data-ttu-id="393a3-129">Používat pouze hello <b>produkční</b> Pokud chcete, aby toousers toosend nabízená oznámení, kteří zakoupili aplikaci z úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-129">Only use hello <b>Production</b> if you want toosend push notifications toousers who purchased your app from hello store.</span></span></p>
</li>
</ol>
<span data-ttu-id="393a3-130">&emsp;&emsp;&emsp;&emsp;![Konfigurace služby APN na portálu Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)</span><span class="sxs-lookup"><span data-stu-id="393a3-130">&emsp;&emsp;&emsp;&emsp;![Configure APNS in Azure Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)</span></span>

&emsp;&emsp;&emsp;&emsp;![Konfigurace certifikační služby APNS na portálu Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

<span data-ttu-id="393a3-132">Vaše centrum oznámení je nyní nakonfigurované toowork pomocí služby APNS a máte hello připojovací řetězce tooregister vaší aplikace a odesílání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="393a3-132">Your notification hub is now configured toowork with APNS, and you have hello connection strings tooregister your app and send push notifications.</span></span>

## <a name="connect-your-ios-app-toonotification-hubs"></a><span data-ttu-id="393a3-133">Připojit vaše iOS aplikace tooNotification rozbočovače</span><span class="sxs-lookup"><span data-stu-id="393a3-133">Connect your iOS app tooNotification Hubs</span></span>
1. <span data-ttu-id="393a3-134">V Xcode, vytvořte nový projekt iOS a vyberte hello **jediné zobrazení aplikace** šablony.</span><span class="sxs-lookup"><span data-stu-id="393a3-134">In Xcode, create a new iOS project and select hello **Single View Application** template.</span></span>
   
    ![Xcode – jediné zobrazení aplikace][8]
    
2. <span data-ttu-id="393a3-136">Při nastavování hello možnosti pro nový projekt, ujistěte se, toouse hello stejné **název produktu** a **identifikátor organizace** jste použili při předchozím nastavení sady ID hello hello vývojáře Apple portál.</span><span class="sxs-lookup"><span data-stu-id="393a3-136">When setting hello options for your new project, make sure toouse hello same **Product Name** and **Organization Identifier** that you used when you previously set hello bundle ID on hello Apple Developer portal.</span></span>
   
    ![Xcode – možnosti projektu][11]
    
3. <span data-ttu-id="393a3-138">V části **cíle**, klikněte na název projektu, klikněte na hello **nastavení sestavení** a rozbalte **identitu podepisování kódu**a potom v části **ladění**, nastavte svoji identitu podepisování kódu.</span><span class="sxs-lookup"><span data-stu-id="393a3-138">Under **Targets**, click your project name, click hello **Build Settings** tab and expand **Code Signing Identity**, and then under **Debug**, set your code-signing identity.</span></span> <span data-ttu-id="393a3-139">Přepnutí **úrovně** z **základní** příliš**všechny**a nastavte **profil zřizování** toohello profil, který jste předtím vytvořili pro zřizování .</span><span class="sxs-lookup"><span data-stu-id="393a3-139">Toggle **Levels** from **Basic** too**All**, and set **Provisioning Profile** toohello provisioning profile that you created previously.</span></span>
   
    <span data-ttu-id="393a3-140">Pokud nevidíte hello nový zajišťování profil, který jste vytvořili v Xcode, pokuste se aktualizovat hello profily pro podpisové identity.</span><span class="sxs-lookup"><span data-stu-id="393a3-140">If you don't see hello new provisioning profile that you created in Xcode, try refreshing hello profiles for your signing identity.</span></span> <span data-ttu-id="393a3-141">Klikněte na tlačítko **Xcode** v řádku nabídek hello, klikněte na tlačítko **Předvolby**, klikněte na tlačítko hello **účet** , klikněte na hello **zobrazit podrobnosti** tlačítko, klikněte na vaše podepisování identity a pak klikněte na tlačítko Aktualizovat hello v pravém dolním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-141">Click **Xcode** on hello menu bar, click **Preferences**, click hello **Account** tab, click hello **View Details** button, click your signing identity, and then click hello refresh button in hello bottom-right corner.</span></span>
   
    ![Xcode – profil zřizování][9]
4. <span data-ttu-id="393a3-143">Stáhnout hello [Mobile Services iOS SDK verze 1.2.4] a rozbalte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-143">Download hello [Mobile Services iOS SDK version 1.2.4] and unzip hello file.</span></span> <span data-ttu-id="393a3-144">V Xcode, klikněte pravým tlačítkem na projekt a klikněte na tlačítko hello **přidat soubory do** možnost tooadd hello **WindowsAzureMessaging.framework** projektu Xcode tooyour složky.</span><span class="sxs-lookup"><span data-stu-id="393a3-144">In Xcode, right-click your project and click hello **Add Files to** option tooadd hello **WindowsAzureMessaging.framework** folder tooyour Xcode project.</span></span> <span data-ttu-id="393a3-145">Vyberte možnost **Kopírovat položky v případě potřeby** a pak klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="393a3-145">Select **Copy items if needed**, and then click **Add**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="393a3-146">Hello centra oznámení SDK aktuálně nepodporuje bitcode na Xcode 7.</span><span class="sxs-lookup"><span data-stu-id="393a3-146">hello notification hubs SDK does not currently support bitcode on Xcode 7.</span></span>  <span data-ttu-id="393a3-147">Je nutné nastavit **povolit Bitcode** příliš**ne** v hello **sestavení možnosti** pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="393a3-147">You must set **Enable Bitcode** too**No** in hello **Build Options** for your project.</span></span>
   > 
   > 
   
    ![Rozbalte Azure SDK][10]
5. <span data-ttu-id="393a3-149">Přidat nový hlavičky souboru tooyour projekt s názvem `HubInfo.h`.</span><span class="sxs-lookup"><span data-stu-id="393a3-149">Add a new header file tooyour project named `HubInfo.h`.</span></span> <span data-ttu-id="393a3-150">Tento soubor bude obsahovat hello konstanty pro vaše Centrum oznámení.</span><span class="sxs-lookup"><span data-stu-id="393a3-150">This file will hold hello constants for your notification hub.</span></span>  <span data-ttu-id="393a3-151">Přidejte následující definice hello a nahraďte s hello zástupné symboly literálu řetězce vaše *název centra* a hello *DefaultListenSharedAccessSignature* který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="393a3-151">Add hello following definitions and replace hello string literal placeholders with your *hub name* and hello *DefaultListenSharedAccessSignature* that you noted earlier.</span></span>
   
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter hello name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
6. <span data-ttu-id="393a3-152">Otevřete váš `AppDelegate.h` souboru přidejte následující direktivy importu hello:</span><span class="sxs-lookup"><span data-stu-id="393a3-152">Open your `AppDelegate.h` file add hello following import directives:</span></span>
   
         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
7. <span data-ttu-id="393a3-153">Ve vaší `AppDelegate.m file`, přidejte následující kód v hello hello `didFinishLaunchingWithOptions` metoda založené na vaší verzi iOS.</span><span class="sxs-lookup"><span data-stu-id="393a3-153">In your `AppDelegate.m file`, add hello following code in hello `didFinishLaunchingWithOptions` method based on your version of iOS.</span></span> <span data-ttu-id="393a3-154">Tento kód zaregistruje popisovač vašeho zařízení do APN:</span><span class="sxs-lookup"><span data-stu-id="393a3-154">This code registers your device handle with APNs:</span></span>
   
    <span data-ttu-id="393a3-155">Pro iOS 8:</span><span class="sxs-lookup"><span data-stu-id="393a3-155">For iOS 8:</span></span>
   
         UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
   
    <span data-ttu-id="393a3-156">Pro too8 předchozí verze iOS:</span><span class="sxs-lookup"><span data-stu-id="393a3-156">For iOS versions prior too8:</span></span>
   
         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
8. <span data-ttu-id="393a3-157">V hello stejný soubor, přidejte následující metody hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-157">In hello same file, add hello following methods.</span></span> <span data-ttu-id="393a3-158">Tento kód se připojí toohello centra oznámení pomocí hello informace o připojení, který jste zadali v souboru HubInfo.h.</span><span class="sxs-lookup"><span data-stu-id="393a3-158">This code connects toohello notification hub using hello connection information you specified in HubInfo.h.</span></span> <span data-ttu-id="393a3-159">Poté přiřadí centra oznámení tokenu toohello hello zařízení tak, aby hello centra oznámení můžete odesílat oznámení:</span><span class="sxs-lookup"><span data-stu-id="393a3-159">It then gives hello device token toohello notification hub so that hello notification hub can send notifications:</span></span>
   
        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }
   
        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
9. <span data-ttu-id="393a3-160">V hello stejný soubor, přidejte následující metodu toodisplay hello **UIAlert** Pokud bylo přijato oznámení hello je hello aplikace aktivní:</span><span class="sxs-lookup"><span data-stu-id="393a3-160">In hello same file, add hello following method toodisplay a **UIAlert** if hello notification is received while hello app is active:</span></span>

        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

1. <span data-ttu-id="393a3-161">Sestavení a spuštění aplikace hello na vašem zařízení tooverify, že neexistují žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="393a3-161">Build and run hello app on your device tooverify that there are no failures.</span></span>

## <a name="send-test-push-notifications"></a><span data-ttu-id="393a3-162">Odešlete nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="393a3-162">Send test push notifications</span></span>
<span data-ttu-id="393a3-163">Příjem oznámení ve vaší aplikaci odesláním nabízených oznámení v hello můžete otestovat [portálu Azure] prostřednictvím hello **Poradce při potížích s** část v okně centra hello (použijte hello *testovací odeslání* možnost).</span><span class="sxs-lookup"><span data-stu-id="393a3-163">You can test receiving notifications in your app by sending push notifications in hello [Azure Portal] via hello **Troubleshooting** section in hello hub blade (use hello *Test Send* option).</span></span>

![Portál Azure – testovací odeslání][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-from-hello-app"></a><span data-ttu-id="393a3-165">(Volitelné) Odesílání nabízených oznámení z aplikace hello</span><span class="sxs-lookup"><span data-stu-id="393a3-165">(Optional) Send push notifications from hello app</span></span>
> [!IMPORTANT]
> <span data-ttu-id="393a3-166">Tady je příklad odesílání oznámení z klienta aplikace hello se poskytuje pro učení pouze pro účely.</span><span class="sxs-lookup"><span data-stu-id="393a3-166">This example of sending notifications from hello client app is provided for learning purposes only.</span></span> <span data-ttu-id="393a3-167">Vzhledem k tomu, že to bude vyžadovat hello `DefaultFullSharedAccessSignature` toobe v hello klientskou aplikaci, zpřístupňuje riziko toohello centra oznámení, že uživatel může získat oznámení toosend neoprávněného přístupu tooyour klientů.</span><span class="sxs-lookup"><span data-stu-id="393a3-167">Since this will require hello `DefaultFullSharedAccessSignature` toobe present on hello client app, it exposes your notification hub toohello risk that a user may gain access toosend unauthorized notifications tooyour clients.</span></span>
> 
> 

<span data-ttu-id="393a3-168">Pokud chcete toosend nabízená oznámení z aplikace, tato část poskytuje příklad toodo této konfigurace pomocí hello rozhraní REST.</span><span class="sxs-lookup"><span data-stu-id="393a3-168">If you want toosend push notifications from within an app, this section provides an example of how toodo this using hello REST interface.</span></span>

1. <span data-ttu-id="393a3-169">V Xcode otevřete `Main.storyboard` a přidejte následující součásti uživatelského rozhraní z hello objekt knihovny tooallow hello uživatele toosend nabízená oznámení v aplikaci hello hello:</span><span class="sxs-lookup"><span data-stu-id="393a3-169">In Xcode, open `Main.storyboard` and add hello following UI components from hello object library tooallow hello user toosend push notifications in hello app:</span></span>
   
   * <span data-ttu-id="393a3-170">Popisek bez textu popisku.</span><span class="sxs-lookup"><span data-stu-id="393a3-170">A label with no label text.</span></span> <span data-ttu-id="393a3-171">Bude použité tooreport chyb v odesílání oznámení.</span><span class="sxs-lookup"><span data-stu-id="393a3-171">It will be used tooreport errors in sending notifications.</span></span> <span data-ttu-id="393a3-172">Hello **řádky** vlastnost musí být nastavená příliš**0** tak, aby se automaticky velikost omezené toohello práva a levý okraj a horní hello hello zobrazení.</span><span class="sxs-lookup"><span data-stu-id="393a3-172">hello **Lines** property should be set too**0** so that it will automatically size constrained toohello right and left margins and hello top of hello view.</span></span>
   * <span data-ttu-id="393a3-173">Textové pole s **zástupný symbol** text nastaven příliš**zadejte zprávu oznámení**.</span><span class="sxs-lookup"><span data-stu-id="393a3-173">A text field with **Placeholder** text set too**Enter Notification Message**.</span></span> <span data-ttu-id="393a3-174">Omezte hello pole přímo pod hello štítek, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="393a3-174">Constrain hello field just below hello label as shown below.</span></span> <span data-ttu-id="393a3-175">Nastavte hello řadiče zobrazení jako delegáta výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-175">Set hello View Controller as hello outlet delegate.</span></span>
   * <span data-ttu-id="393a3-176">Tlačítko s názvem **odeslat oznámení** omezené pod textové pole hello a ve vodorovném centru hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-176">A button titled **Send Notification** constrained just below hello text field and in hello horizontal center.</span></span>
     
     <span data-ttu-id="393a3-177">zobrazení Hello by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="393a3-177">hello view should look as follows:</span></span>
     
     ![Návrhář Xcode][32]
2. <span data-ttu-id="393a3-179">[Přidejte výstupy](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) pro hello popisek a textové pole připojené k vašemu zobrazení a aktualizovat vaše `interface` definice toosupport `UITextFieldDelegate` a `NSXMLParserDelegate`.</span><span class="sxs-lookup"><span data-stu-id="393a3-179">[Add outlets](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) for hello label and text field connected your view, and update your `interface` definition toosupport `UITextFieldDelegate` and `NSXMLParserDelegate`.</span></span> <span data-ttu-id="393a3-180">Přidejte hello tři vlastnosti deklarací uvedené níže toohelp podporu volání hello REST API a analyzování odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-180">Add hello three property declarations shown below toohelp support calling hello REST API and parsing hello response.</span></span>
   
    <span data-ttu-id="393a3-181">Váš soubor ViewController.h by měl vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="393a3-181">Your ViewController.h file should look as follows:</span></span>
   
        #import <UIKit/UIKit.h>
   
        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }
   
        // Make sure these outlets are connected tooyour UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;
   
        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;
   
        @end
3. <span data-ttu-id="393a3-182">Otevřete `HubInfo.h` a přidejte následující konstanty, které se použije pro odesílání oznámení tooyour rozbočovače hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-182">Open `HubInfo.h` and add hello following constants which will be used for sending notifications tooyour hub.</span></span> <span data-ttu-id="393a3-183">Nahraďte hello zástupný symbol řetězcového literálu skutečným *DefaultFullSharedAccessSignature* připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="393a3-183">Replace hello placeholder string literal with your actual *DefaultFullSharedAccessSignature* connection string.</span></span>
   
        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"
4. <span data-ttu-id="393a3-184">Přidejte následující hello `#import` tooyour příkazy `ViewController.h` souboru.</span><span class="sxs-lookup"><span data-stu-id="393a3-184">Add hello following `#import` statements tooyour `ViewController.h` file.</span></span>
   
        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"
5. <span data-ttu-id="393a3-185">V `ViewController.m` přidejte následující kód implementace rozhraní toohello hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-185">In `ViewController.m` add hello following code toohello interface implementation.</span></span> <span data-ttu-id="393a3-186">Tento kód provede analýzu vašeho připojovacího řetězce *DefaultFullSharedAccessSignature*.</span><span class="sxs-lookup"><span data-stu-id="393a3-186">This code will parse your *DefaultFullSharedAccessSignature* connection string.</span></span> <span data-ttu-id="393a3-187">Jak je uvedeno v hello [odkazu k REST API](http://msdn.microsoft.com/library/azure/dn495627.aspx), tyto analyzované informace budou použité toogenerate tokenu SaS pro hello **autorizace** hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="393a3-187">As mentioned in hello [REST API reference](http://msdn.microsoft.com/library/azure/dn495627.aspx), this parsed information will be used toogenerate a SaS token for hello **Authorization** request header.</span></span>
   
        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;
   
        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;
   
            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];
   
                @throw parseException;
            }
   
            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }
6. <span data-ttu-id="393a3-188">V `ViewController.m`, aktualizace hello `viewDidLoad` metoda tooparse hello připojovacího řetězce při načtení zobrazení hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-188">In `ViewController.m`, update hello `viewDidLoad` method tooparse hello connection string when hello view loads.</span></span> <span data-ttu-id="393a3-189">Také přidat hello pomocné metody uvedené níže, toohello implementaci rozhraní.</span><span class="sxs-lookup"><span data-stu-id="393a3-189">Also add hello utility methods, shown below, toohello interface implementation.</span></span>  

        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





1. <span data-ttu-id="393a3-190">V `ViewController.m`, přidejte následující kód toohello rozhraní implementace toogenerate hello tokenu autorizace SaS, bude k dispozici v hello hello **autorizace** záhlaví, jak je uvedeno v hello [REST API Referenční dokumentace](http://msdn.microsoft.com/library/azure/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="393a3-190">In `ViewController.m`, add hello following code toohello interface implementation toogenerate hello SaS authorization token that will be provided in hello **Authorization** header, as mentioned in hello [REST API Reference](http://msdn.microsoft.com/library/azure/dn495627.aspx).</span></span>
   
        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;
   
            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];
   
                // Get an hmac_sha1 Mac instance and initialize with hello signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];
   
                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }
   
            return token;
        }
2. <span data-ttu-id="393a3-191">CTRL + přetažení z hello **odeslat oznámení** tlačítko příliš`ViewController.m` tooadd akci s názvem **SendNotificationMessage** pro hello **Touch Down** událostí.</span><span class="sxs-lookup"><span data-stu-id="393a3-191">Ctrl+drag from hello **Send Notification** button too`ViewController.m` tooadd an action named **SendNotificationMessage** for hello **Touch Down** event.</span></span> <span data-ttu-id="393a3-192">Metoda aktualizace pomocí následujícího kódu toosend hello oznámení pomocí rozhraní REST API hello hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-192">Update method with hello following code toosend hello notification using hello REST API.</span></span>
   
        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }
   
        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];
   
            // Apple Notification format of hello notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];
   
            // Construct hello message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];
   
            // Generate hello token toobe used in hello authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create hello request tooadd hello APNs notification message toohello hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            //Authenticate hello notification message POST request with hello SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add hello notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send hello REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                       [xmlParser parse];
                }
            }];
            [dataTask resume];
        }
3. <span data-ttu-id="393a3-193">V `ViewController.m`, přidejte následující toosupport metoda delegáta zavřením hello klávesnice pro textové pole hello hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-193">In `ViewController.m`, add hello following delegate method toosupport closing hello keyboard for hello text field.</span></span> <span data-ttu-id="393a3-194">CTRL + přetažení z ikonu řadiče zobrazení hello textové pole toohello v hello rozhraní návrháře tooset hello zobrazení jako delegáta výstupu hello řadiče.</span><span class="sxs-lookup"><span data-stu-id="393a3-194">Ctrl+drag from hello text field toohello View Controller icon in hello interface designer tooset hello view controller as hello outlet delegate.</span></span>
   
        //===[ Implement UITextFieldDelegate methods ]===
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
4. <span data-ttu-id="393a3-195">V `ViewController.m`, přidejte následující hello delegovat metody toosupport analýzy hello odpovědi pomocí `NSXMLParser`.</span><span class="sxs-lookup"><span data-stu-id="393a3-195">In `ViewController.m`, add hello following delegate methods toosupport parsing hello response by using `NSXMLParser`.</span></span>
   
       //===[ Implement NSXMLParserDelegate methods ]===
   
       -(void)parserDidStartDocument:(NSXMLParser *)parser
       {
           self.statusResult = @"";
       }
   
       -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
           namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
           attributes:(NSDictionary *)attributeDict
       {
           NSString * element = [elementName lowercaseString];
           NSLog(@"*** New element parsed : %@ ***",element);
   
           if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
           {
               self.currentElement = element;
           }
       }
   
       -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
       {
           self.statusResult = [self.statusResult stringByAppendingString:
               [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
       }
   
       -(void)parserDidEndDocument:(NSXMLParser *)parser
       {
           // Set hello status label text on hello UI thread
           dispatch_async(dispatch_get_main_queue(),
           ^{
               [self.sendResults setText:self.statusResult];
           });
       }
5. <span data-ttu-id="393a3-196">Sestavení projektu hello a ověřte, zda nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="393a3-196">Build hello project and verify that there are no errors.</span></span>

> [!NOTE]
> <span data-ttu-id="393a3-197">Pokud dojde k chybě sestavení v Xcode7 o podpoře bitcode, měli byste změnit hello **nastavení sestavení** > **povolit Bitcode (ENABLE_BITCODE)** příliš**ne** v Xcode.</span><span class="sxs-lookup"><span data-stu-id="393a3-197">If you encounter a build error in Xcode7 about bitcode support, you should change hello **Build Settings** > **Enable Bitcode (ENABLE_BITCODE)** too**NO** in Xcode.</span></span> <span data-ttu-id="393a3-198">Hello centra oznámení SDK aktuálně nepodporuje bitcode.</span><span class="sxs-lookup"><span data-stu-id="393a3-198">hello Notification Hubs SDK does not currently support bitcode.</span></span> 
> 
> 

<span data-ttu-id="393a3-199">Můžete najít všechna možná oznámení datových částí hello v hello Apple [průvodci místních a nabízených oznámení programování].</span><span class="sxs-lookup"><span data-stu-id="393a3-199">You can find all hello possible notification payloads in hello Apple [Local and Push Notification Programming Guide].</span></span>

## <a name="checking-if-your-app-can-receive-push-notifications"></a><span data-ttu-id="393a3-200">Kontrola, zda vaše aplikace může přijímat nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="393a3-200">Checking if your app can receive push notifications</span></span>
<span data-ttu-id="393a3-201">tootest nabízená oznámení na iOS, musíte nasadit hello aplikace tooa fyzickém zařízení iOS.</span><span class="sxs-lookup"><span data-stu-id="393a3-201">tootest push notifications on iOS, you must deploy hello app tooa physical iOS device.</span></span> <span data-ttu-id="393a3-202">Nabízená oznámení Apple nelze odeslat pomocí simulátoru iOS hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-202">You cannot send Apple push notifications by using hello iOS Simulator.</span></span>

1. <span data-ttu-id="393a3-203">Spuštění aplikace hello a ověřte, zda byla registrace úspěšná a stiskněte klávesu **OK**.</span><span class="sxs-lookup"><span data-stu-id="393a3-203">Run hello app and verify that registration succeeds, and then press **OK**.</span></span>
   
    ![Test registrace nabízených oznámení aplikace iOS][33]
2. <span data-ttu-id="393a3-205">Můžete odeslat testovací nabízená oznámení z hello [portálu Azure], jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="393a3-205">You can send a test push notification from hello [Azure Portal], as described above.</span></span> <span data-ttu-id="393a3-206">Pokud jste přidali kód pro odesílání nabízených oznámení v aplikaci hello, klepněte do hello textové pole tooenter zprávu oznámení.</span><span class="sxs-lookup"><span data-stu-id="393a3-206">If you added code for sending push notifications in hello app, touch inside hello text field tooenter a notification message.</span></span> <span data-ttu-id="393a3-207">Stiskněte klávesu hello **odeslat** na klávesnici hello nebo hello tlačítko **odeslat oznámení** tlačítka na hello zobrazení toosend hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="393a3-207">Then press hello **Send** button on hello keyboard or hello **Send Notification** button in hello view toosend hello notification message.</span></span>
   
    ![Test odeslání nabízených oznámení aplikace iOS][34]
3. <span data-ttu-id="393a3-209">Hello nabízená oznámení se odesílá tooall zařízení, která jsou registrovaná tooreceive hello oznámení z konkrétní centra oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="393a3-209">hello push notification is sent tooall devices that are registered tooreceive hello notifications from hello particular Notification Hub.</span></span>
   
    ![Test příjmu nabízených oznámení aplikace iOS][35]

## <a name="next-steps"></a><span data-ttu-id="393a3-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="393a3-211">Next steps</span></span>
<span data-ttu-id="393a3-212">V tomto jednoduchém příkladu jste vysílali nabízená oznámení tooall vaše registrovaná zařízení iOS.</span><span class="sxs-lookup"><span data-stu-id="393a3-212">In this simple example, you broadcasted push notifications tooall your registered iOS devices.</span></span> <span data-ttu-id="393a3-213">Doporučujeme jako další krok ve svém studiu, že přejdete toohello [Azure upozornění uživatelů centra oznámení pro iOS pomocí backendu .NET] kurz, který vás provede procesem vytvoření back-end toosend nabízená oznámení pomocí značek.</span><span class="sxs-lookup"><span data-stu-id="393a3-213">We suggest as a next step in your learning that you proceed toohello [Azure Notification Hubs Notify Users for iOS with .NET backend] tutorial, which will walk you through creating a backend toosend push notifications using tags.</span></span> 

<span data-ttu-id="393a3-214">Pokud chcete toosegment uživatele podle zájmových skupin, můžete se navíc přesunout na toohello [toosend použití centra oznámení nejnovější zprávy přes] kurzu.</span><span class="sxs-lookup"><span data-stu-id="393a3-214">If you want toosegment your users by interest groups, you can additionally move on toohello [Use Notification Hubs toosend breaking news] tutorial.</span></span> 

<span data-ttu-id="393a3-215">Obecné informace o centrech oznámení naleznete v tématu [Průvodce centry oznámení].</span><span class="sxs-lookup"><span data-stu-id="393a3-215">For general information about Notification Hubs, see [Notification Hubs Guidance].</span></span>

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile Services iOS SDK verze 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Průvodce centry oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure upozornění uživatelů centra oznámení pro iOS pomocí backendu .NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[toosend použití centra oznámení nejnovější zprávy přes]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[průvodci místních a nabízených oznámení programování]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[portálu Azure]: https://portal.azure.com
