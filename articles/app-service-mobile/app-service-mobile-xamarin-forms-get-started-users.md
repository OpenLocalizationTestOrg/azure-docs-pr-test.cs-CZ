---
title: "aaaGet Začínáme s ověřováním mobilních aplikací v aplikaci Xamarin Forms | Microsoft Docs"
description: "Zjistěte, jak toouse Mobile Apps tooauthenticate uživatele vaší aplikace Xamarin Forms prostřednictvím řady různých zprostředkovatelů identity, včetně AAD, Google, Facebook, Twitter a Microsoft."
services: app-service\mobile
documentationcenter: xamarin
author: panarasi
manager: syntaxc4
editor: 
ms.assetid: 9c55e192-c761-4ff2-8d88-72260e9f6179
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: panarasi
ms.openlocfilehash: 7f6716619f33d9cc4f866c41effba8f048dc49fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-xamarin-forms-app"></a>Přidat aplikaci Xamarin Forms tooyour ověřování
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak uživatelé tooauthenticate aplikace App Service Mobile z klientské aplikace. V tomto kurzu přidání ověřování do projektu typu rychlý start Xamarin Forms hello pomocí zprostředkovatele identity, která je podporována službou App Service. Po se úspěšně ověří a autorizuje pomocí mobilní aplikace, zobrazí se hodnota ID uživatele hello a bude možné tooaccess omezený dat v tabulce.

## <a name="prerequisites"></a>Požadavky
Pro nejlepší výsledek hello v tomto kurzu, doporučujeme, abyste nejdříve dokončili hello [vytvoření aplikace pro Xamarin Forms] [ 1] kurzu. Po dokončení tohoto kurzu budete mít projekt Xamarin Forms, který je aplikace seznamu úkolů více platformami.

Pokud nepoužijete hello stáhli úvodní serverový projekt, je nutné přidat hello ověřování rozšíření balíčku tooyour projektu. Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps][2].

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Registrace aplikace pro ověřování a nakonfigurujte aplikační služby
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování

Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci. To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello. V tomto kurzu používáme schéma adresy URL hello _appname_ v průběhu. Můžete však použít žádné schéma adresy URL, které zvolíte. Mělo by být jedinečný tooyour mobilních aplikací. tooenable hello přesměrování na straně serveru hello:

1. V [portál Azure] hello vyberte App Service.

2. Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.

3. V hello **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.  Hello **url_scheme_of_your_app** v tento řetězec je hello schéma adresy URL pro mobilní aplikace.  Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).  Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.

4. Klikněte na **OK**.

5. Klikněte na **Uložit**.

## <a name="restrict-permissions-tooauthenticated-users"></a>Omezte oprávnění tooauthenticated
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-toohello-portable-class-library"></a>Přidat ověřování toohello Přenosná knihovna tříd
Mobilní aplikace používá hello [LoginAsync] [ 3] rozšiřující metody na hello [MobileServiceClient] [ 4] toosign v uživatele službou App Service ověřování. Tato ukázka používá toku spravovaný server ověřování, který zobrazí hello zprostředkovatele přihlášení rozhraní v aplikaci hello. Další informace najdete v tématu [spravovaný Server ověřování][5]. Chcete-li zajistit lepší prostředí uživatele ve vaší aplikaci produkční, měli byste zvážit namísto použití [klienta spravovat ověřování][6].

definování tooauthenticate s Xamarin Forms projektu, **IAuthenticate** rozhraní v hello knihovny přenosných tříd pro aplikaci hello. Pak přidejte **přihlášení** tlačítko toohello uživatelského rozhraní definované v hello přenosné knihovny tříd kliknutí toostart ověřování. Data jsou načítána z back-end mobilní aplikace hello po úspěšném ověření.

Implementace hello **IAuthenticate** rozhraní pro každou platformu podporovanou nástrojem vaší aplikace.

1. V sadě Visual Studio nebo Xamarin Studio otevřete App.cs z projektu hello s **přenosné** v hello název, který je projektu knihovny přenosných tříd, pak přidejte následující hello `using` příkaz:

        using System.Threading.Tasks;
2. V App.cs, přidejte následující hello `IAuthenticate` definici bezprostředně před hello rozhraní `App` definici třídy.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. tooinitialize hello rozhraní s provádění specifických pro platformy, přidejte následující statické členy toohello hello **aplikace** třídy.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. Otevřete TodoList.xaml z projektu knihovny přenosných tříd hello, přidejte následující hello **tlačítko** element v hello *buttonsPanel* element rozložení po existující tlačítko hello:

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    Toto tlačítko se aktivuje server spravovaný ověřování s váš back-end mobilní aplikace.
5. Otevřete TodoList.xaml.cs z projektu knihovny přenosných tříd hello a pak přidejte následující pole toohello hello `TodoList` třídy:

        // Track whether hello user has authenticated.
        bool authenticated = false;
6. Nahraďte hello **OnAppearing** metoda s hello následující kód:

        protected override async void OnAppearing()
        {
            base.OnAppearing();

            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems tootrue in order toosynchronize hello data
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide hello Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Tento kód je zajištěno, že data se pouze aktualizují ze služby hello po jste ověření.
7. Přidejte následující obslužnou rutinu pro hello hello **místní** událostí toohello **TodoList** třídy:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems tootrue toosynchronize hello data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. Uložte změny a znovu sestavte projekt knihovny přenosných tříd hello žádné chyby ověření.

## <a name="add-authentication-toohello-android-app"></a>Přidat ověřování toohello aplikace pro Android
Tato část uvádí, jak tooimplement hello **IAuthenticate** rozhraní v projektu aplikace pro Android hello. Tuto část přeskočte, pokud nejsou podpora zařízení se systémem Android.

1. V sadě Visual Studio nebo Xamarin Studio, klikněte pravým tlačítkem na hello **droid** projekt, pak **nastavit jako spouštěný projekt**.
2. Stiskněte klávesu F5 toostart hello projektu v ladicím programu hello a potom ověřte, že k neošetřené výjimce s stavový kód 401 (Neautorizováno) se vyvolá po spuštění aplikace. kód 401 Hello se vytvářejí, protože přístup na back-end hello je jenom s omezeným přístupem tooauthorized uživatele.
3. Otevřete v projektu Android hello MainActivity.cs a přidejte následující hello `using` příkazy:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Aktualizace hello **MainActivity** třída tooimplement hello **IAuthenticate** rozhraní, následujícím způsobem:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. Aktualizace hello **MainActivity** třída přidáním **MobileServiceUser** pole a **ověřit** metoda, který je požadován hello **IAuthenticate**  rozhraní, následujícím způsobem:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this, 
                    MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display hello success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }

    Pokud používáte zprostředkovatele identity než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider][7].

6. Přidejte následující kód do hello <application> uzlu AndroidManifest.xml:

```xml
    <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
      </intent-filter>
    </activity>
```

1. Přidejte následující kód toohello hello **OnCreate** metoda hello **MainActivity** třídy před hello volání příliš`LoadApplication()`:

        // Initialize hello authenticator before loading hello app.
        App.Init((IAuthenticate)this);

    Tento kód zajistí, že ověřovací hello se inicializuje před zatížení aplikace hello.
2. Znovu sestavte aplikaci hello, spusťte ji a pak přihlásit se pomocí zprostředkovatele ověřování hello zvolili a ověřte, zda je možné tooaccess data jako ověřený uživatel.

## <a name="add-authentication-toohello-ios-app"></a>Přidání ověřování toohello iOS aplikace
Tato část uvádí, jak tooimplement hello **IAuthenticate** rozhraní v projektu iOS aplikace hello. Tuto část přeskočte, pokud nejsou podporujete zařízení s iOS.

1. V sadě Visual Studio nebo Xamarin Studio, klikněte pravým tlačítkem na hello **iOS** projekt, pak **nastavit jako spouštěný projekt**.
2. Stiskněte klávesu F5 toostart hello projektu v ladicím programu hello a potom ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno). odpovědi 401 Hello se vytvářejí, protože přístup na back-end hello je jenom s omezeným přístupem tooauthorized uživatele.
3. Otevřete v projektu iOS hello AppDelegate.cs a přidejte následující hello `using` příkazy:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Aktualizace hello **AppDelegate** třída tooimplement hello **IAuthenticate** rozhraní, následujícím způsobem:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. Aktualizace hello **AppDelegate** třída přidáním **MobileServiceUser** pole a **ověřit** metoda, který je požadován hello **IAuthenticate**  rozhraní, následujícím způsobem:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;
                    }
                }
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display hello success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();

            return success;
        }

    Pokud používáte zprostředkovatele identity než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider].

6. Aktualizace třídy AppDelegate hello přidáním přetížení metody OpenUrl (UIApplication aplikaci a adresu url nsurl, který, NSDictionary možnosti)

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }

6. Přidejte následující řádek kódu toohello hello **FinishedLaunching** volání metody před hello příliš`LoadApplication()`:

        App.Init(this);

    Tento kód zajistí, že ověřovací hello je inicializována, před načtením aplikace hello.

6. Přidat **{url_scheme_of_your_app}** tooURL schémata v Info.plist.

7. Znovu sestavte aplikaci hello, spusťte ji a pak přihlásit se pomocí zprostředkovatele ověřování hello zvolili a ověřte, zda je možné tooaccess data jako ověřený uživatel.

## <a name="add-authentication-toowindows-10-including-phone-app-projects"></a>Přidat ověřování tooWindows 10 (včetně Phone) projekty aplikací
Tato část uvádí, jak tooimplement hello **IAuthenticate** rozhraní v projektech aplikace hello Windows 10. stejný postup platí pro univerzální platformu Windows (UWP) projekty, ale používá hello Hello **UWP** projektu (s uvedené změny). Tuto část přeskočte, pokud nejsou podpora zařízení se systémem Windows.

1. "V sadě Visual Studio, klikněte pravým tlačítkem na buď hello **UWP** projekt, pak **nastavit jako spouštěný projekt**.
2. Stiskněte klávesu F5 toostart hello projektu v ladicím programu hello a potom ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno). odpovědi 401 Hello se stane, protože přístup na back-end hello je jenom s omezeným přístupem tooauthorized uživatele.
3. Otevřete MainPage.xaml.cs pro projekt aplikace Windows hello a přidejte následující hello `using` příkazy:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Nahraďte `<your_Portable_Class_Library_namespace>` s hello obor názvů pro knihovny přenosných tříd.
4. Aktualizace hello **MainPage** třída tooimplement hello **IAuthenticate** rozhraní, následujícím způsobem:

        public sealed partial class MainPage : IAuthenticate
5. Aktualizace hello **MainPage** třída přidáním **MobileServiceUser** pole a **ověřit** metoda, který je požadován hello **IAuthenticate** rozhraní, následujícím způsobem:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }

            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display hello success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }

    Pokud používáte zprostředkovatele identity než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider].

1. Přidejte následující řádek kódu hello konstruktor hello hello **MainPage** třídy před hello volání příliš`LoadApplication()`:

        // Initialize hello authenticator before loading hello app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    Nahraďte `<your_Portable_Class_Library_namespace>` s hello obor názvů pro knihovny přenosných tříd.

3. Pokud používáte **UWP**, přidejte následující hello **OnActivated** metoda přepsání toohello **aplikace** třídy:

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }

       }

   Při přepsání metody hello již existuje, přidáte kód podmíněného hello z hello předchozím fragmentu kódu.  Tento kód se nevyžaduje pro projekty pro Universal Windows.

3. Přidat **{url_scheme_of_your_app}** v Package.appxmanifest. 

4. Znovu sestavte aplikaci hello, spusťte ji a pak přihlásit se pomocí zprostředkovatele ověřování hello zvolili a ověřte, zda je možné tooaccess data jako ověřený uživatel.

## <a name="next-steps"></a>Další kroky
Teď, když jste dokončili kurz základní ověřování, zvažte, budete pokračovat v tooone hello následující kurzy:

* [Přidat nabízená oznámení tooyour aplikaci](app-service-mobile-xamarin-forms-get-started-push.md)

  Zjistěte, jak tooadd nabízená oznámení podporují tooyour aplikace a konfigurace mobilní aplikace back-end toouse Azure Notification Hubs toosend nabízených oznámení.
* [Povolení offline synchronizace u aplikace](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  Zjistěte, jak tooadd offline podporují aplikace pomocí back-end mobilní aplikace. Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací - zobrazení, přidávat a upravovat data - i v případě, že není žádné síťové připojení.

<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-xamarin-forms-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[5]: app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx
