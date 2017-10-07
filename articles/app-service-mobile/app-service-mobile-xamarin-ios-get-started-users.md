---
title: "aaaGet Začínáme s ověřováním pro Mobile Apps v Xamarin iOS"
description: "Zjistěte, jak toouse Mobile Apps tooauthenticate uživatele vaší aplikace Xamarin iOS prostřednictvím řady různých zprostředkovatelů identity, včetně AAD, Google, Facebook, Twitter a Microsoft."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 180cc61b-19c5-48bf-a16c-7181aef3eacc
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: glenga
ms.openlocfilehash: 6458e9651b03df61c86b88b11953792e04bfa5b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-xamarinios-app"></a>Přidat aplikaci Xamarin.iOS tooyour ověřování
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Toto téma ukazuje, jak uživatelé tooauthenticate aplikace App Service Mobile z klientské aplikace. V tomto kurzu přidáte ověřování toohello Xamarin.iOS rychlý start projekt pomocí zprostředkovatele identity, která je podporována službou App Service. Po se úspěšně ověří a autorizuje pomocí mobilní aplikace, zobrazí se hodnota ID uživatele hello a bude moct tooaccess omezený dat v tabulce.

Je třeba nejprve provést hello kurzu [vytvoření aplikace Xamarin.iOS]. Pokud nepoužijete hello stáhli úvodní serverový projekt, je nutné přidat hello ověřování rozšíření balíčku tooyour projektu. Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Registrace aplikace pro ověřování a nakonfigurujte aplikační služby
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-toohello-allowed-external-redirect-urls"></a>Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování

Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci. To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello. V tomto kurzu používáme schéma adresy URL hello _appname_ v průběhu. Můžete však použít žádné schéma adresy URL, které zvolíte. Mělo by být jedinečný tooyour mobilních aplikací. tooenable hello přesměrování na straně serveru hello:

1. V [portál Azure] hello vyberte App Service.

2. Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.

3. V hello **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.  Hello **url_scheme_of_your_app** v tento řetězec je hello schéma adresy URL pro mobilní aplikace.  Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).  Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.

4. Klikněte na **OK**.

5. Klikněte na **Uložit**.

## <a name="restrict-permissions-tooauthenticated-users"></a>Omezte oprávnění tooauthenticated
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. V sadě Visual Studio nebo Xamarin Studio spusťte hello klientského projektu na emulátoru nebo zařízení. Ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno). selhání Hello je zaznamenané toohello konzole ladicího programu hello. Proto v sadě Visual Studio, měli byste vidět hello selhání v okně výstupu hello.

&nbsp;&nbsp;Toto selhání neoprávněným dojde proto, že aplikace hello pokusí tooaccess váš back-end mobilní aplikace jako neověřený uživatel. Hello *TodoItem* tabulka nyní vyžaduje ověření.

Potom aktualizujte hello klienta aplikace toorequest prostředky z back-end mobilní aplikace hello s ověřeného uživatele.

## <a name="add-authentication-toohello-app"></a>Přidat aplikaci toohello ověřování
V této části upravíte toodisplay aplikace hello obrazovka pro přihlášení, než se zobrazí data. Při spuštění aplikace hello nepřipojí tooyour služby App Service a nebudou zobrazovat žádná data. Po prvním hello této hello uživatel provede hello gesto aktualizace, zobrazí se obrazovka pro přihlášení hello; Po úspěšném přihlášení hello seznam položek todo se zobrazí.

1. V projektu klienta hello, otevřete soubor hello **QSTodoService.cs** a přidejte následující hello pomocí příkazu a `MobileServiceUser` s přistupujícího objektu toohello QSTodoService třídy:
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. Přidat novou metodu s názvem **ověřit** příliš**QSTodoService** s hello následující definice:

        public async Task Authenticate(UIViewController view)
        {
            try
            {
                AppDelegate.ResumeWithURL = url => url.Scheme == "zumoe2etestapp" && client.ResumeWithURL(url);
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu hello předán příliš**LoginAsync** výše tooone následující hello: _MicrosoftAccount_, _Twitter_, _Google_, nebo _WindowsAzureActiveDirectory_.

3. Otevřete **QSTodoListViewController.cs**. Upravit definici metoda hello **ViewDidLoad** odebrání hello volání příliš**RefreshAsync()** téměř hello end:
   
        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();
   
            todoService = QSTodoService.DefaultService;
            await todoService.InitializeStoreAsync();
   
            RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync();
            }
   
            // Comment out hello call tooRefreshAsync
            // await RefreshAsync();
        }
4. Změňte metodu hello **RefreshAsync** tooauthenticate Pokud hello **uživatele** vlastnost má hodnotu null. Přidejte následující kód v horní části hello hello metoda definice hello:
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. Otevřete **AppDelegate.cs**, přidejte následující metodu hello:

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. Otevřete **Info.plist** souboru, přejděte příliš**URL typy** v hello **Upřesnit** části. Teď nakonfigurovat hello **identifikátor** a hello **schémata URL** typ adresy URL a klikněte na tlačítko **přidat adresu URL typu**. **Schémata URL** by měla být stejná jako vaše {url_scheme_of_your_app} hello.
7. V sadě Visual Studio nebo Xamarin Studio připojené tooyour Xamarin sestavení hostitele na počítači Mac, spusťte projekt klienta hello cílení na emulátoru nebo zařízení. Ověřte, že tuto aplikaci hello nezobrazí žádná data.
   
    Proveďte aktualizaci gesto hello přidáváním dolů hello seznam položek, které způsobí, že hello přihlašovací obrazovky tooappear. Jakmile úspěšně jste zadali platné přihlašovací údaje, hello aplikace se zobrazí hello seznam položek todo a můžete provést aktualizace toohello data.

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[vytvoření aplikace Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started.md