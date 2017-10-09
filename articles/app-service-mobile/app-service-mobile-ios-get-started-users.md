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
# <a name="add-authentication-tooyour-ios-app"></a>Přidání ověřování tooyour iOS aplikace
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

V tomto kurzu přidáte ověřování toohello [iOS rychlý start] projektu pomocí zprostředkovatele identity podporované. V tomto kurzu vychází z hello [iOS rychlý start] kurz, který je třeba nejprve provést.

## <a name="register"></a>Registrace aplikace pro ověřování a nakonfigurujte hello služby App Service
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování

Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.  To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello.  V tomto kurzu používáme schématu adresy URL _appname_ v průběhu.  Můžete však použít žádné schéma adresy URL, které zvolíte.  Mělo by být jedinečný tooyour mobilních aplikací.  tooenable hello přesměrování na straně serveru tý:

1. V hello [portál Azure], vyberte svoji službu aplikace.

2. Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.

3. Klikněte na tlačítko **Azure Active Directory** pod hello **zprostředkovatele ověřování** části.

4. Sada hello **režim správy** příliš**Upřesnit**.

5. V hello **povoleno externí adres URL pro přesměrování**, zadejte `appname://easyauth.callback`.  Hello _appname_ v tento řetězec je hello schéma adresy URL pro mobilní aplikace.  Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).  Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.

6. Klikněte na **OK**.

7. Klikněte na **Uložit**.

## <a name="permissions"></a>Omezte oprávnění tooauthenticated
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

V Xcode, stiskněte klávesu **spustit** toostart hello aplikace. Protože aplikace hello pokusí tooaccess back-end jako neověřený uživatel je vyvolána výjimka, ale hello *TodoItem* tabulka nyní vyžaduje ověření.

## <a name="add-authentication"></a>Přidat ověřování tooapp
**Jazyka Objective-C**:

1. Na počítači Mac otevřete *QSTodoListViewController.m* v Xcode a přidejte následující metodu hello:

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

    Změna *google* příliš*microsoftaccount*, *twitter*, *facebook*, nebo *windowsazureactivedirectory* Pokud nepoužíváte Google jako zprostředkovatele identity. Pokud používáte sítě Facebook, musíte [povolených domén Facebook] [ 1] ve vaší aplikaci.

    Nahraďte hello **urlScheme** s jedinečným názvem pro vaši aplikaci.  Hello urlScheme by měl být hello stejné jako hello schéma adresy URL protokolu, který jste zadali v hello **povoleno externí adres URL pro přesměrování** pole hello portálu Azure. Hello urlScheme používá hello ověřování zpětného volání tooswitch back tooyour aplikace po dokončení žádosti o ověření.

2. Nahraďte `[self refresh]` v `viewDidLoad` v *QSTodoListViewController.m* s hello následující kód:

    ```Objective-C
    [self loginAndGetData];
    ```

3. Otevřete hello `QSAppDelegate.h` souboru a přidejte následující kód hello:

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. Otevřete hello `QSAppDelegate.m` souboru a přidejte následující kód hello:

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

   Přidejte tento kód přímo před čtení řádku hello `#pragma mark - Core Data stack`.  Nahraďte _appname_ wih hello urlScheme hodnotu, která jste použili v kroku 1.

5. Otevřete hello `AppName-Info.plist` souboru (nahraďte AppName s hello název vaší aplikace) a přidejte následující kód hello:

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

    Tento kód by měl být umístěn uvnitř hello `<dict>` elementu.  Nahraďte hello _appname_ řetězec (v rámci pole pro **CFBundleURLSchemes**) s názvem aplikace hello jste zvolili v kroku 1.  Můžete také provádět tyto změny v hello plist editoru – klikněte na hello `AppName-Info.plist` souboru v editoru plist hello tooopen XCode.

    Nahraďte hello `com.microsoft.azure.zumo` řetězec **CFBundleURLName** identifikátor sady vaší společnosti Apple.

6. Stiskněte klávesu *spustit* toostart hello aplikaci a znovu přihlásili. Pokud jste se přihlásili, by měl být schopný tooview seznam úkolů hello a provádět aktualizace.

**Kód SWIFT**:

1. Na počítači Mac otevřete *ToDoTableViewController.swift* v Xcode a přidejte následující metodu hello:

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

    Změna *google* příliš*microsoftaccount*, *twitter*, *facebook*, nebo *windowsazureactivedirectory* Pokud nepoužíváte Google jako zprostředkovatele identity. Pokud používáte sítě Facebook, musíte [povolených domén Facebook] [ 1] ve vaší aplikaci.

    Nahraďte hello **urlScheme** s jedinečným názvem pro vaši aplikaci.  Hello urlScheme by měl být hello stejné jako hello schéma adresy URL protokolu, který jste zadali v hello **povoleno externí adres URL pro přesměrování** pole hello portálu Azure. Hello urlScheme používá hello ověřování zpětného volání tooswitch back tooyour aplikace po dokončení žádosti o ověření.

2. Odstranění řádků hello `self.refreshControl?.beginRefreshing()` a `self.onRefresh(self.refreshControl)` na konci `viewDidLoad()` v *ToDoTableViewController.swift*. Přidejte volání příliš`loginAndGetData()` místo nich:

    ```swift
    loginAndGetData()
    ```

3. Otevřete hello `AppDelegate.swift` souboru a přidejte následující řádek toohello hello `AppDelegate` třídy:

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

    Nahraďte hello _appname_ wih hello urlScheme hodnotu, která jste použili v kroku 1.

4. Otevřete hello `AppName-Info.plist` souboru (nahraďte AppName s hello název vaší aplikace) a přidejte následující kód hello:

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

    Tento kód by měl být umístěn uvnitř hello `<dict>` elementu.  Nahraďte hello _appname_ řetězec (v rámci pole pro **CFBundleURLSchemes**) s názvem aplikace hello jste zvolili v kroku 1.  Můžete také provádět tyto změny v hello plist editoru – klikněte na hello `AppName-Info.plist` souboru v editoru plist hello tooopen XCode.

    Nahraďte hello `com.microsoft.azure.zumo` řetězec **CFBundleURLName** identifikátor sady vaší společnosti Apple.

5. Stiskněte klávesu *spustit* toostart hello aplikaci a znovu přihlásili. Pokud jste se přihlásili, by měl být schopný tooview seznam úkolů hello a provádět aktualizace.

Ověřování služby App Service používá jablka Inter aplikace komunikace.  Další podrobnosti o této problematice naleznete toohello [dokumentaci od společnosti Apple][2]
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
[portál Azure]: https://portal.azure.com

[iOS rychlý start]: app-service-mobile-ios-get-started.md

