---
title: "Přidání ověřování v systému iOS pomocí Azure Mobile Apps"
description: "Naučte se používat Azure Mobile Apps ověřovat uživatele vaší aplikace pro iOS prostřednictvím řady různých zprostředkovatelů identity, včetně AAD, Google, Facebook, Twitter a Microsoft."
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
ms.openlocfilehash: 21a2cc6c1eaf4b34cbe8c2d7c4dbb69c8730cf32
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-ios-app"></a><span data-ttu-id="02941-103">Přidání ověřování do vaší aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="02941-103">Add authentication to your iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="02941-104">V tomto kurzu přidáte ověřování [iOS rychlý start] projektu pomocí zprostředkovatele identity podporované.</span><span class="sxs-lookup"><span data-stu-id="02941-104">In this tutorial, you add authentication to the [iOS quick start] project using a supported identity provider.</span></span> <span data-ttu-id="02941-105">V tomto kurzu vychází z [iOS rychlý start] kurz, který je třeba nejprve provést.</span><span class="sxs-lookup"><span data-stu-id="02941-105">This tutorial is based on the [iOS quick start] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="02941-106"><a name="register"></a>Registrace aplikace pro ověřování a nakonfigurovat App Service</span><span class="sxs-lookup"><span data-stu-id="02941-106"><a name="register"></a>Register your app for authentication and configure the App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="02941-107"><a name="redirecturl"></a>Přidání aplikace do adresy URL pro povolené externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="02941-107"><a name="redirecturl"></a>Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="02941-108">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="02941-108">Secure authentication requires that you define a new URL scheme for your app.</span></span>  <span data-ttu-id="02941-109">To umožňuje ověřování systému přesměrovat zpět do aplikace po dokončení procesu ověřování.</span><span class="sxs-lookup"><span data-stu-id="02941-109">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span>  <span data-ttu-id="02941-110">V tomto kurzu používáme schématu adresy URL _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="02941-110">In this tutorial, we use the URL scheme _appname_ throughout.</span></span>  <span data-ttu-id="02941-111">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="02941-111">However, you can use any URL scheme you choose.</span></span>  <span data-ttu-id="02941-112">Musí být jedinečné pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="02941-112">It should be unique to your mobile application.</span></span>  <span data-ttu-id="02941-113">Chcete povolit přesměrování na straně serveru tý:</span><span class="sxs-lookup"><span data-stu-id="02941-113">To enable the redirection on th server side:</span></span>

1. <span data-ttu-id="02941-114">V [portál Azure], vyberte svoji službu aplikace.</span><span class="sxs-lookup"><span data-stu-id="02941-114">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="02941-115">Klikněte **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="02941-115">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="02941-116">Klikněte na tlačítko **Azure Active Directory** pod **zprostředkovatele ověřování** části.</span><span class="sxs-lookup"><span data-stu-id="02941-116">Click **Azure Active Directory** under the **Authentication Providers** section.</span></span>

4. <span data-ttu-id="02941-117">Nastavte **režim správy** k **Upřesnit**.</span><span class="sxs-lookup"><span data-stu-id="02941-117">Set the **Management mode** to **Advanced**.</span></span>

5. <span data-ttu-id="02941-118">V **povoleno externí adres URL pro přesměrování**, zadejte `appname://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="02941-118">In the **Allowed External Redirect URLs**, enter `appname://easyauth.callback`.</span></span>  <span data-ttu-id="02941-119">_Appname_ v tento řetězec je schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="02941-119">The _appname_ in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="02941-120">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="02941-120">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="02941-121">Měli byste si poznamenat řetězce, který zvolíte, jako je třeba upravit kód mobilní aplikace s schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="02941-121">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

6. <span data-ttu-id="02941-122">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="02941-122">Click **OK**.</span></span>

7. <span data-ttu-id="02941-123">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="02941-123">Click **Save**.</span></span>

## <span data-ttu-id="02941-124"><a name="permissions"></a>Omezit oprávnění k ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="02941-124"><a name="permissions"></a>Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="02941-125">V Xcode, stiskněte klávesu **spustit** a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="02941-125">In Xcode, press **Run** to start the app.</span></span> <span data-ttu-id="02941-126">Je vyvolána výjimka, protože se aplikace pokusí o přístup k back-end jako neověřený uživatel, ale *TodoItem* tabulka nyní vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="02941-126">An exception is raised because the app attempts to access the backend as an unauthenticated user, but the *TodoItem* table now requires authentication.</span></span>

## <span data-ttu-id="02941-127"><a name="add-authentication"></a>Přidejte ověřování do aplikace</span><span class="sxs-lookup"><span data-stu-id="02941-127"><a name="add-authentication"></a>Add authentication to app</span></span>
<span data-ttu-id="02941-128">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="02941-128">**Objective-C**:</span></span>

1. <span data-ttu-id="02941-129">Na počítači Mac otevřete *QSTodoListViewController.m* v Xcode a přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="02941-129">On your Mac, open *QSTodoListViewController.m* in Xcode and add the following method:</span></span>

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

    <span data-ttu-id="02941-130">Změna *google* k *microsoftaccount*, *twitter*, *facebook*, nebo *windowsazureactivedirectory* Pokud nepoužíváte Google jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="02941-130">Change *google* to *microsoftaccount*, *twitter*, *facebook*, or *windowsazureactivedirectory* if you are not using Google as your identity provider.</span></span> <span data-ttu-id="02941-131">Pokud používáte sítě Facebook, musíte [povolených domén Facebook] [ 1] ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="02941-131">If you use Facebook, you must [whitelist Facebook domains][1] in your app.</span></span>

    <span data-ttu-id="02941-132">Nahraďte **urlScheme** s jedinečným názvem pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="02941-132">Replace the **urlScheme** with a unique name for your application.</span></span>  <span data-ttu-id="02941-133">UrlScheme by měla být stejná jako schéma adresy URL protokolu, který jste zadali v **povoleno externí adres URL pro přesměrování** pole na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="02941-133">The urlScheme should be the same as the URL Scheme protocol that you specified in the **Allowed External Redirect URLs** field in the Azure portal.</span></span> <span data-ttu-id="02941-134">UrlScheme je používán zpětné volání ověřování přepnout zpět do aplikace po dokončení žádosti o ověření.</span><span class="sxs-lookup"><span data-stu-id="02941-134">The urlScheme is used by the authentication callback to switch back to your application after the authentication request is complete.</span></span>

2. <span data-ttu-id="02941-135">Nahraďte `[self refresh]` v `viewDidLoad` v *QSTodoListViewController.m* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="02941-135">Replace `[self refresh]` in `viewDidLoad` in *QSTodoListViewController.m* with the following code:</span></span>

    ```Objective-C
    [self loginAndGetData];
    ```

3. <span data-ttu-id="02941-136">Otevřete `QSAppDelegate.h` souboru a přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="02941-136">Open the `QSAppDelegate.h` file and add the following code:</span></span>

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. <span data-ttu-id="02941-137">Otevřete `QSAppDelegate.m` souboru a přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="02941-137">Open the `QSAppDelegate.m` file and add the following code:</span></span>

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

   <span data-ttu-id="02941-138">Přidejte tento kód přímo před při čtení řádku `#pragma mark - Core Data stack`.</span><span class="sxs-lookup"><span data-stu-id="02941-138">Add this code directly before the line reading `#pragma mark - Core Data stack`.</span></span>  <span data-ttu-id="02941-139">Nahraďte _appname_ wih urlScheme hodnotu, která jste použili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="02941-139">Replace the _appname_ wih the urlScheme value that you used in step 1.</span></span>

5. <span data-ttu-id="02941-140">Otevřete `AppName-Info.plist` souboru (nahraďte AppName s názvem vaší aplikace) a přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="02941-140">Open the `AppName-Info.plist` file (replacing AppName with the name of your app), and add the following code:</span></span>

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

    <span data-ttu-id="02941-141">Tento kód by měl být umístěn uvnitř `<dict>` elementu.</span><span class="sxs-lookup"><span data-stu-id="02941-141">This code should be placed inside the `<dict>` element.</span></span>  <span data-ttu-id="02941-142">Nahraďte _appname_ řetězec (v rámci pole pro **CFBundleURLSchemes**) s názvem aplikace, který jste zvolili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="02941-142">Replace the _appname_ string (within the array for **CFBundleURLSchemes**) with the app name you chose in step 1.</span></span>  <span data-ttu-id="02941-143">Můžete provést také tyto změny v plist. editoru – kliknutím na tlačítko `AppName-Info.plist` souboru v XCode otevřete plist editor.</span><span class="sxs-lookup"><span data-stu-id="02941-143">You can also make these changes in the plist editor - click on the `AppName-Info.plist` file in XCode to open the plist editor.</span></span>

    <span data-ttu-id="02941-144">Nahraďte `com.microsoft.azure.zumo` řetězec **CFBundleURLName** identifikátor sady vaší společnosti Apple.</span><span class="sxs-lookup"><span data-stu-id="02941-144">Replace the `com.microsoft.azure.zumo` string for **CFBundleURLName** with your Apple bundle identifier.</span></span>

6. <span data-ttu-id="02941-145">Stiskněte klávesu *spustit* a spusťte aplikaci a znovu přihlásili.</span><span class="sxs-lookup"><span data-stu-id="02941-145">Press *Run* to start the app, and then log in.</span></span> <span data-ttu-id="02941-146">Pokud jste se přihlásili, byste měli moct zobrazit seznam úkolů a provádět aktualizace.</span><span class="sxs-lookup"><span data-stu-id="02941-146">When you are logged in, you should be able to view the Todo list and make updates.</span></span>

<span data-ttu-id="02941-147">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="02941-147">**Swift**:</span></span>

1. <span data-ttu-id="02941-148">Na počítači Mac otevřete *ToDoTableViewController.swift* v Xcode a přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="02941-148">On your Mac, open *ToDoTableViewController.swift* in Xcode and add the following method:</span></span>

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

    <span data-ttu-id="02941-149">Změna *google* k *microsoftaccount*, *twitter*, *facebook*, nebo *windowsazureactivedirectory* Pokud nepoužíváte Google jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="02941-149">Change *google* to *microsoftaccount*, *twitter*, *facebook*, or *windowsazureactivedirectory* if you are not using Google as your identity provider.</span></span> <span data-ttu-id="02941-150">Pokud používáte sítě Facebook, musíte [povolených domén Facebook] [ 1] ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="02941-150">If you use Facebook, you must [whitelist Facebook domains][1] in your app.</span></span>

    <span data-ttu-id="02941-151">Nahraďte **urlScheme** s jedinečným názvem pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="02941-151">Replace the **urlScheme** with a unique name for your application.</span></span>  <span data-ttu-id="02941-152">UrlScheme by měla být stejná jako schéma adresy URL protokolu, který jste zadali v **povoleno externí adres URL pro přesměrování** pole na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="02941-152">The urlScheme should be the same as the URL Scheme protocol that you specified in the **Allowed External Redirect URLs** field in the Azure portal.</span></span> <span data-ttu-id="02941-153">UrlScheme je používán zpětné volání ověřování přepnout zpět do aplikace po dokončení žádosti o ověření.</span><span class="sxs-lookup"><span data-stu-id="02941-153">The urlScheme is used by the authentication callback to switch back to your application after the authentication request is complete.</span></span>

2. <span data-ttu-id="02941-154">Odstranit řádky `self.refreshControl?.beginRefreshing()` a `self.onRefresh(self.refreshControl)` na konci `viewDidLoad()` v *ToDoTableViewController.swift*.</span><span class="sxs-lookup"><span data-stu-id="02941-154">Remove the lines `self.refreshControl?.beginRefreshing()` and `self.onRefresh(self.refreshControl)` at the end of `viewDidLoad()` in *ToDoTableViewController.swift*.</span></span> <span data-ttu-id="02941-155">Přidejte volání `loginAndGetData()` místo nich:</span><span class="sxs-lookup"><span data-stu-id="02941-155">Add a call to `loginAndGetData()` in their place:</span></span>

    ```swift
    loginAndGetData()
    ```

3. <span data-ttu-id="02941-156">Otevřete `AppDelegate.swift` souboru a přidejte následující řádek do `AppDelegate` třídy:</span><span class="sxs-lookup"><span data-stu-id="02941-156">Open the `AppDelegate.swift` file and add the following line to the `AppDelegate` class:</span></span>

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

    <span data-ttu-id="02941-157">Nahraďte _appname_ wih urlScheme hodnotu, která jste použili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="02941-157">Replace the _appname_ wih the urlScheme value that you used in step 1.</span></span>

4. <span data-ttu-id="02941-158">Otevřete `AppName-Info.plist` souboru (nahraďte AppName s názvem vaší aplikace) a přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="02941-158">Open the `AppName-Info.plist` file (replacing AppName with the name of your app), and add the following code:</span></span>

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

    <span data-ttu-id="02941-159">Tento kód by měl být umístěn uvnitř `<dict>` elementu.</span><span class="sxs-lookup"><span data-stu-id="02941-159">This code should be placed inside the `<dict>` element.</span></span>  <span data-ttu-id="02941-160">Nahraďte _appname_ řetězec (v rámci pole pro **CFBundleURLSchemes**) s názvem aplikace, který jste zvolili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="02941-160">Replace the _appname_ string (within the array for **CFBundleURLSchemes**) with the app name you chose in step 1.</span></span>  <span data-ttu-id="02941-161">Můžete provést také tyto změny v plist. editoru – kliknutím na tlačítko `AppName-Info.plist` souboru v XCode otevřete plist editor.</span><span class="sxs-lookup"><span data-stu-id="02941-161">You can also make these changes in the plist editor - click on the `AppName-Info.plist` file in XCode to open the plist editor.</span></span>

    <span data-ttu-id="02941-162">Nahraďte `com.microsoft.azure.zumo` řetězec **CFBundleURLName** identifikátor sady vaší společnosti Apple.</span><span class="sxs-lookup"><span data-stu-id="02941-162">Replace the `com.microsoft.azure.zumo` string for **CFBundleURLName** with your Apple bundle identifier.</span></span>

5. <span data-ttu-id="02941-163">Stiskněte klávesu *spustit* a spusťte aplikaci a znovu přihlásili.</span><span class="sxs-lookup"><span data-stu-id="02941-163">Press *Run* to start the app, and then log in.</span></span> <span data-ttu-id="02941-164">Pokud jste se přihlásili, byste měli moct zobrazit seznam úkolů a provádět aktualizace.</span><span class="sxs-lookup"><span data-stu-id="02941-164">When you are logged in, you should be able to view the Todo list and make updates.</span></span>

<span data-ttu-id="02941-165">Ověřování služby App Service používá jablka Inter aplikace komunikace.</span><span class="sxs-lookup"><span data-stu-id="02941-165">App Service Authentication uses Apples Inter-App Communication.</span></span>  <span data-ttu-id="02941-166">Další podrobnosti o této problematice naleznete [dokumentaci od společnosti Apple][2]</span><span class="sxs-lookup"><span data-stu-id="02941-166">For more details on this subject, refer to the [Apple Documentation][2]</span></span>
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
<span data-ttu-id="02941-167">[portál Azure]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="02941-167">[Azure portal]: https://portal.azure.com</span></span>

<span data-ttu-id="02941-168">[iOS rychlý start]: app-service-mobile-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="02941-168">[iOS quick start]: app-service-mobile-ios-get-started.md</span></span>

