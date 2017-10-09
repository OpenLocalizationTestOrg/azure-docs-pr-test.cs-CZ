---
title: "aaaAdd ověřování v systému iOS pomocí Azure Mobile Apps"
description: "Zjistěte, jak toouse Azure Mobile Apps tooauthenticate uživatele vaší aplikace iOS prostřednictvím řady různých zprostředkovatelů identity, včetně AAD, Google, Facebook, Twitter a Microsoft."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: ef3d3cbe-e7ca-45f9-987f-80c44209dc06
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: glenga
ms.openlocfilehash: df129e1c7517582db0e4705e0a6e98345ac8a48c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-ios-app"></a><span data-ttu-id="8eb2c-103">Přidání ověřování tooyour iOS aplikace</span><span class="sxs-lookup"><span data-stu-id="8eb2c-103">Add authentication tooyour iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="8eb2c-104">V tomto kurzu přidáte ověřování toohello [iOS rychlý start] projektu pomocí zprostředkovatele identity podporované.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-104">In this tutorial, you add authentication toohello [iOS quick start] project using a supported identity provider.</span></span> <span data-ttu-id="8eb2c-105">V tomto kurzu vychází z hello [iOS rychlý start] kurz, který je třeba nejprve provést.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-105">This tutorial is based on hello [iOS quick start] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="8eb2c-106"><a name="register"></a>Registrace aplikace pro ověřování a nakonfigurujte hello služby App Service</span><span class="sxs-lookup"><span data-stu-id="8eb2c-106"><a name="register"></a>Register your app for authentication and configure hello App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="8eb2c-107"><a name="redirecturl"></a>Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="8eb2c-107"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="8eb2c-108">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-108">Secure authentication requires that you define a new URL scheme for your app.</span></span>  <span data-ttu-id="8eb2c-109">To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-109">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span>  <span data-ttu-id="8eb2c-110">V tomto kurzu používáme schématu adresy URL _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-110">In this tutorial, we use the URL scheme _appname_ throughout.</span></span>  <span data-ttu-id="8eb2c-111">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-111">However, you can use any URL scheme you choose.</span></span>  <span data-ttu-id="8eb2c-112">Mělo by být jedinečný tooyour mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-112">It should be unique tooyour mobile application.</span></span>  <span data-ttu-id="8eb2c-113">tooenable hello přesměrování na straně serveru tý:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-113">tooenable hello redirection on th server side:</span></span>

1. <span data-ttu-id="8eb2c-114">V hello [portál Azure], vyberte svoji službu aplikace.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-114">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="8eb2c-115">Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-115">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="8eb2c-116">Klikněte na tlačítko **Azure Active Directory** pod hello **zprostředkovatele ověřování** části.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-116">Click **Azure Active Directory** under hello **Authentication Providers** section.</span></span>

4. <span data-ttu-id="8eb2c-117">Sada hello **režim správy** příliš**Upřesnit**.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-117">Set hello **Management mode** too**Advanced**.</span></span>

5. <span data-ttu-id="8eb2c-118">V hello **povoleno externí adres URL pro přesměrování**, zadejte `appname://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-118">In hello **Allowed External Redirect URLs**, enter `appname://easyauth.callback`.</span></span>  <span data-ttu-id="8eb2c-119">Hello _appname_ v tento řetězec je hello schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-119">hello _appname_ in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="8eb2c-120">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="8eb2c-120">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="8eb2c-121">Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-121">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

6. <span data-ttu-id="8eb2c-122">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-122">Click **OK**.</span></span>

7. <span data-ttu-id="8eb2c-123">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-123">Click **Save**.</span></span>

## <span data-ttu-id="8eb2c-124"><a name="permissions"></a>Omezte oprávnění tooauthenticated</span><span class="sxs-lookup"><span data-stu-id="8eb2c-124"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="8eb2c-125">V Xcode, stiskněte klávesu **spustit** toostart hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-125">In Xcode, press **Run** toostart hello app.</span></span> <span data-ttu-id="8eb2c-126">Protože aplikace hello pokusí tooaccess back-end jako neověřený uživatel je vyvolána výjimka, ale hello *TodoItem* tabulka nyní vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-126">An exception is raised because hello app attempts tooaccess the backend as an unauthenticated user, but hello *TodoItem* table now requires authentication.</span></span>

## <span data-ttu-id="8eb2c-127"><a name="add-authentication"></a>Přidat ověřování tooapp</span><span class="sxs-lookup"><span data-stu-id="8eb2c-127"><a name="add-authentication"></a>Add authentication tooapp</span></span>
<span data-ttu-id="8eb2c-128">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-128">**Objective-C**:</span></span>

1. <span data-ttu-id="8eb2c-129">Na počítači Mac otevřete *QSTodoListViewController.m* v Xcode a přidejte následující metodu hello:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-129">On your Mac, open *QSTodoListViewController.m* in Xcode and add hello following method:</span></span>

    ```Objective-C
    - (void)loginAndGetData
    {
        QSAppDelegate *appDelegate = (QSAppDelegate *)[UIApplication sharedApplication].delegate;
        appDelegate.qsTodoService = self.todoService;

        [self.todoService.client loginWithProvider:@"google" urlScheme:@"appname" controller:self animated:YES completion:^(MSUser * _Nullable user, NSError * _Nullable error) {
            if (error) {
                NSLog(@"Login failed with error: %@, %@", error, [error userInfo]);
            }
            else {
                self.todoService.client.currentUser = user;
                NSLog(@"User logged in: %@", user.userId);

                [self refresh];
            }
        }];
    }
    ```

    <span data-ttu-id="8eb2c-130">Změna *google* příliš*microsoftaccount*, *twitter*, *facebook*, nebo *windowsazureactivedirectory* Pokud nepoužíváte Google jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-130">Change *google* too*microsoftaccount*, *twitter*, *facebook*, or *windowsazureactivedirectory* if you are not using Google as your identity provider.</span></span> <span data-ttu-id="8eb2c-131">Pokud používáte sítě Facebook, musíte [povolených domén Facebook] [ 1] ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-131">If you use Facebook, you must [whitelist Facebook domains][1] in your app.</span></span>

    <span data-ttu-id="8eb2c-132">Nahraďte hello **urlScheme** s jedinečným názvem pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-132">Replace hello **urlScheme** with a unique name for your application.</span></span>  <span data-ttu-id="8eb2c-133">Hello urlScheme by měl být hello stejné jako hello schéma adresy URL protokolu, který jste zadali v hello **povoleno externí adres URL pro přesměrování** pole hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-133">hello urlScheme should be hello same as hello URL Scheme protocol that you specified in hello **Allowed External Redirect URLs** field in hello Azure portal.</span></span> <span data-ttu-id="8eb2c-134">Hello urlScheme používá hello ověřování zpětného volání tooswitch back tooyour aplikace po dokončení žádosti o ověření.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-134">hello urlScheme is used by hello authentication callback tooswitch back tooyour application after the authentication request is complete.</span></span>

2. <span data-ttu-id="8eb2c-135">Nahraďte `[self refresh]` v `viewDidLoad` v *QSTodoListViewController.m* s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-135">Replace `[self refresh]` in `viewDidLoad` in *QSTodoListViewController.m* with hello following code:</span></span>

    ```Objective-C
    [self loginAndGetData];
    ```

3. <span data-ttu-id="8eb2c-136">Otevřete hello `QSAppDelegate.h` souboru a přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-136">Open hello `QSAppDelegate.h` file and add hello following code:</span></span>

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. <span data-ttu-id="8eb2c-137">Otevřete hello `QSAppDelegate.m` souboru a přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-137">Open hello `QSAppDelegate.m` file and add hello following code:</span></span>

    ```Objective-C
    - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
    {
        if ([[url.scheme lowercaseString] isEqualToString:@"appname"]) {
            // Resume login flow
            return [self.qsTodoService.client resumeWithURL:url];
        }
        else {
            return NO;
        }
    }
    ```

   <span data-ttu-id="8eb2c-138">Přidejte tento kód přímo před čtení řádku hello `#pragma mark - Core Data stack`.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-138">Add this code directly before hello line reading `#pragma mark - Core Data stack`.</span></span>  <span data-ttu-id="8eb2c-139">Nahraďte _appname_ wih hello urlScheme hodnotu, která jste použili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-139">Replace the _appname_ wih hello urlScheme value that you used in step 1.</span></span>

5. <span data-ttu-id="8eb2c-140">Otevřete hello `AppName-Info.plist` souboru (nahraďte AppName s hello název vaší aplikace) a přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-140">Open hello `AppName-Info.plist` file (replacing AppName with hello name of your app), and add hello following code:</span></span>

    ```XML
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    <span data-ttu-id="8eb2c-141">Tento kód by měl být umístěn uvnitř hello `<dict>` elementu.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-141">This code should be placed inside hello `<dict>` element.</span></span>  <span data-ttu-id="8eb2c-142">Nahraďte hello _appname_ řetězec (v rámci pole pro **CFBundleURLSchemes**) s názvem aplikace hello jste zvolili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-142">Replace hello _appname_ string (within the array for **CFBundleURLSchemes**) with hello app name you chose in step 1.</span></span>  <span data-ttu-id="8eb2c-143">Můžete také provádět tyto změny v hello plist editoru – klikněte na hello `AppName-Info.plist` souboru v editoru plist hello tooopen XCode.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-143">You can also make these changes in hello plist editor - click on hello `AppName-Info.plist` file in XCode tooopen hello plist editor.</span></span>

    <span data-ttu-id="8eb2c-144">Nahraďte hello `com.microsoft.azure.zumo` řetězec **CFBundleURLName** identifikátor sady vaší společnosti Apple.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-144">Replace hello `com.microsoft.azure.zumo` string for **CFBundleURLName** with your Apple bundle identifier.</span></span>

6. <span data-ttu-id="8eb2c-145">Stiskněte klávesu *spustit* toostart hello aplikaci a znovu přihlásili.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-145">Press *Run* toostart hello app, and then log in.</span></span> <span data-ttu-id="8eb2c-146">Pokud jste se přihlásili, by měl být schopný tooview seznam úkolů hello a provádět aktualizace.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-146">When you are logged in, you should be able tooview hello Todo list and make updates.</span></span>

<span data-ttu-id="8eb2c-147">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-147">**Swift**:</span></span>

1. <span data-ttu-id="8eb2c-148">Na počítači Mac otevřete *ToDoTableViewController.swift* v Xcode a přidejte následující metodu hello:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-148">On your Mac, open *ToDoTableViewController.swift* in Xcode and add hello following method:</span></span>

    ```swift
    func loginAndGetData() {

        guard let client = self.table?.client, client.currentUser == nil else {
            return
        }

        let appDelegate = UIApplication.shared.delegate as! AppDelegate
        appDelegate.todoTableViewController = self

        let loginBlock: MSClientLoginBlock = {(user, error) -> Void in
            if (error != nil) {
                print("Error: \(error?.localizedDescription)")
            }
            else {
                client.currentUser = user
                print("User logged in: \(user?.userId)")
            }
        }

        client.login(withProvider:"google", urlScheme: "appname", controller: self, animated: true, completion: loginBlock)

    }
    ```

    <span data-ttu-id="8eb2c-149">Změna *google* příliš*microsoftaccount*, *twitter*, *facebook*, nebo *windowsazureactivedirectory* Pokud nepoužíváte Google jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-149">Change *google* too*microsoftaccount*, *twitter*, *facebook*, or *windowsazureactivedirectory* if you are not using Google as your identity provider.</span></span> <span data-ttu-id="8eb2c-150">Pokud používáte sítě Facebook, musíte [povolených domén Facebook] [ 1] ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-150">If you use Facebook, you must [whitelist Facebook domains][1] in your app.</span></span>

    <span data-ttu-id="8eb2c-151">Nahraďte hello **urlScheme** s jedinečným názvem pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-151">Replace hello **urlScheme** with a unique name for your application.</span></span>  <span data-ttu-id="8eb2c-152">Hello urlScheme by měl být hello stejné jako hello schéma adresy URL protokolu, který jste zadali v hello **povoleno externí adres URL pro přesměrování** pole hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-152">hello urlScheme should be hello same as hello URL Scheme protocol that you specified in hello **Allowed External Redirect URLs** field in hello Azure portal.</span></span> <span data-ttu-id="8eb2c-153">Hello urlScheme používá hello ověřování zpětného volání tooswitch back tooyour aplikace po dokončení žádosti o ověření.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-153">hello urlScheme is used by hello authentication callback tooswitch back tooyour application after the authentication request is complete.</span></span>

2. <span data-ttu-id="8eb2c-154">Odstranění řádků hello `self.refreshControl?.beginRefreshing()` a `self.onRefresh(self.refreshControl)` na konci `viewDidLoad()` v *ToDoTableViewController.swift*.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-154">Remove hello lines `self.refreshControl?.beginRefreshing()` and `self.onRefresh(self.refreshControl)` at the end of `viewDidLoad()` in *ToDoTableViewController.swift*.</span></span> <span data-ttu-id="8eb2c-155">Přidejte volání příliš`loginAndGetData()` místo nich:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-155">Add a call too`loginAndGetData()` in their place:</span></span>

    ```swift
    loginAndGetData()
    ```

3. <span data-ttu-id="8eb2c-156">Otevřete hello `AppDelegate.swift` souboru a přidejte následující řádek toohello hello `AppDelegate` třídy:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-156">Open hello `AppDelegate.swift` file and add hello following line toohello `AppDelegate` class:</span></span>

    ```swift
    var todoTableViewController: ToDoTableViewController?

    func application(_ application: UIApplication, openURL url: NSURL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        if url.scheme?.lowercased() == "appname" {
            return (todoTableViewController!.table?.client.resume(with: url as URL))!
        }
        else {
            return false
        }
    }
    ```

    <span data-ttu-id="8eb2c-157">Nahraďte hello _appname_ wih hello urlScheme hodnotu, která jste použili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-157">Replace hello _appname_ wih hello urlScheme value that you used in step 1.</span></span>

4. <span data-ttu-id="8eb2c-158">Otevřete hello `AppName-Info.plist` souboru (nahraďte AppName s hello název vaší aplikace) a přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="8eb2c-158">Open hello `AppName-Info.plist` file (replacing AppName with hello name of your app), and add hello following code:</span></span>

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    <span data-ttu-id="8eb2c-159">Tento kód by měl být umístěn uvnitř hello `<dict>` elementu.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-159">This code should be placed inside hello `<dict>` element.</span></span>  <span data-ttu-id="8eb2c-160">Nahraďte hello _appname_ řetězec (v rámci pole pro **CFBundleURLSchemes**) s názvem aplikace hello jste zvolili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-160">Replace hello _appname_ string (within the array for **CFBundleURLSchemes**) with hello app name you chose in step 1.</span></span>  <span data-ttu-id="8eb2c-161">Můžete také provádět tyto změny v hello plist editoru – klikněte na hello `AppName-Info.plist` souboru v editoru plist hello tooopen XCode.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-161">You can also make these changes in hello plist editor - click on hello `AppName-Info.plist` file in XCode tooopen hello plist editor.</span></span>

    <span data-ttu-id="8eb2c-162">Nahraďte hello `com.microsoft.azure.zumo` řetězec **CFBundleURLName** identifikátor sady vaší společnosti Apple.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-162">Replace hello `com.microsoft.azure.zumo` string for **CFBundleURLName** with your Apple bundle identifier.</span></span>

5. <span data-ttu-id="8eb2c-163">Stiskněte klávesu *spustit* toostart hello aplikaci a znovu přihlásili.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-163">Press *Run* toostart hello app, and then log in.</span></span> <span data-ttu-id="8eb2c-164">Pokud jste se přihlásili, by měl být schopný tooview seznam úkolů hello a provádět aktualizace.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-164">When you are logged in, you should be able tooview hello Todo list and make updates.</span></span>

<span data-ttu-id="8eb2c-165">Ověřování služby App Service používá jablka Inter aplikace komunikace.</span><span class="sxs-lookup"><span data-stu-id="8eb2c-165">App Service Authentication uses Apples Inter-App Communication.</span></span>  <span data-ttu-id="8eb2c-166">Další podrobnosti o této problematice naleznete toohello [dokumentaci od společnosti Apple][2]</span><span class="sxs-lookup"><span data-stu-id="8eb2c-166">For more details on this subject, refer toohello [Apple Documentation][2]</span></span>
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
[portál Azure]: https://portal.azure.com

[iOS rychlý start]: app-service-mobile-ios-get-started.md

