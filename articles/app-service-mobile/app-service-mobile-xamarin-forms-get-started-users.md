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
# <a name="add-authentication-tooyour-xamarin-forms-app"></a><span data-ttu-id="cc7ed-103">Přidat aplikaci Xamarin Forms tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="cc7ed-103">Add authentication tooyour Xamarin Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a><span data-ttu-id="cc7ed-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="cc7ed-104">Overview</span></span>
<span data-ttu-id="cc7ed-105">Toto téma ukazuje, jak uživatelé tooauthenticate aplikace App Service Mobile z klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-105">This topic shows you how tooauthenticate users of an App Service Mobile App from your client application.</span></span> <span data-ttu-id="cc7ed-106">V tomto kurzu přidání ověřování do projektu typu rychlý start Xamarin Forms hello pomocí zprostředkovatele identity, která je podporována službou App Service.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-106">In this tutorial, you add authentication to hello Xamarin Forms quickstart project using an identity provider that is supported by App Service.</span></span> <span data-ttu-id="cc7ed-107">Po se úspěšně ověří a autorizuje pomocí mobilní aplikace, zobrazí se hodnota ID uživatele hello a bude možné tooaccess omezený dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-107">After being successfully authenticated and authorized by your Mobile App, hello user ID value is displayed, and you will be able tooaccess restricted table data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc7ed-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cc7ed-108">Prerequisites</span></span>
<span data-ttu-id="cc7ed-109">Pro nejlepší výsledek hello v tomto kurzu, doporučujeme, abyste nejdříve dokončili hello [vytvoření aplikace pro Xamarin Forms] [ 1] kurzu.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-109">For hello best result with this tutorial, we recommend that you first complete hello [Create a Xamarin Forms app][1] tutorial.</span></span> <span data-ttu-id="cc7ed-110">Po dokončení tohoto kurzu budete mít projekt Xamarin Forms, který je aplikace seznamu úkolů více platformami.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-110">After you complete this tutorial, you will have a Xamarin Forms project that is a multi-platform TodoList app.</span></span>

<span data-ttu-id="cc7ed-111">Pokud nepoužijete hello stáhli úvodní serverový projekt, je nutné přidat hello ověřování rozšíření balíčku tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-111">If you do not use hello downloaded quick start server project, you must add hello authentication extension package tooyour project.</span></span> <span data-ttu-id="cc7ed-112">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="cc7ed-112">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps][2].</span></span>

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><span data-ttu-id="cc7ed-113">Registrace aplikace pro ověřování a nakonfigurujte aplikační služby</span><span class="sxs-lookup"><span data-stu-id="cc7ed-113">Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="cc7ed-114"><a name="redirecturl"></a>Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="cc7ed-114"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="cc7ed-115">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-115">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="cc7ed-116">To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-116">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="cc7ed-117">V tomto kurzu používáme schéma adresy URL hello _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-117">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="cc7ed-118">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-118">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="cc7ed-119">Mělo by být jedinečný tooyour mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-119">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="cc7ed-120">tooenable hello přesměrování na straně serveru hello:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-120">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="cc7ed-121">V [portál Azure] hello vyberte App Service.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-121">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="cc7ed-122">Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-122">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="cc7ed-123">V hello **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-123">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="cc7ed-124">Hello **url_scheme_of_your_app** v tento řetězec je hello schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-124">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="cc7ed-125">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="cc7ed-125">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="cc7ed-126">Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-126">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="cc7ed-127">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-127">Click **OK**.</span></span>

5. <span data-ttu-id="cc7ed-128">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-128">Click **Save**.</span></span>

## <a name="restrict-permissions-tooauthenticated-users"></a><span data-ttu-id="cc7ed-129">Omezte oprávnění tooauthenticated</span><span class="sxs-lookup"><span data-stu-id="cc7ed-129">Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-toohello-portable-class-library"></a><span data-ttu-id="cc7ed-130">Přidat ověřování toohello Přenosná knihovna tříd</span><span class="sxs-lookup"><span data-stu-id="cc7ed-130">Add authentication toohello portable class library</span></span>
<span data-ttu-id="cc7ed-131">Mobilní aplikace používá hello [LoginAsync] [ 3] rozšiřující metody na hello [MobileServiceClient] [ 4] toosign v uživatele službou App Service ověřování.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-131">Mobile Apps uses hello [LoginAsync][3] extension method on hello [MobileServiceClient][4] toosign in a user with App Service authentication.</span></span> <span data-ttu-id="cc7ed-132">Tato ukázka používá toku spravovaný server ověřování, který zobrazí hello zprostředkovatele přihlášení rozhraní v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-132">This sample uses a server-managed authentication flow that displays hello provider's sign-in interface in hello app.</span></span> <span data-ttu-id="cc7ed-133">Další informace najdete v tématu [spravovaný Server ověřování][5].</span><span class="sxs-lookup"><span data-stu-id="cc7ed-133">For more information, see [Server-managed authentication][5].</span></span> <span data-ttu-id="cc7ed-134">Chcete-li zajistit lepší prostředí uživatele ve vaší aplikaci produkční, měli byste zvážit namísto použití [klienta spravovat ověřování][6].</span><span class="sxs-lookup"><span data-stu-id="cc7ed-134">To provide a better user experience in your production app, you should consider instead using [Client-managed authentication][6].</span></span>

<span data-ttu-id="cc7ed-135">definování tooauthenticate s Xamarin Forms projektu, **IAuthenticate** rozhraní v hello knihovny přenosných tříd pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-135">tooauthenticate with a Xamarin Forms project, define an **IAuthenticate** interface in hello Portable Class Library for hello app.</span></span> <span data-ttu-id="cc7ed-136">Pak přidejte **přihlášení** tlačítko toohello uživatelského rozhraní definované v hello přenosné knihovny tříd kliknutí toostart ověřování.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-136">Then add a **Sign-in** button toohello user interface defined in hello Portable Class Library, which you click toostart authentication.</span></span> <span data-ttu-id="cc7ed-137">Data jsou načítána z back-end mobilní aplikace hello po úspěšném ověření.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-137">Data is loaded from hello mobile app backend after successful authentication.</span></span>

<span data-ttu-id="cc7ed-138">Implementace hello **IAuthenticate** rozhraní pro každou platformu podporovanou nástrojem vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-138">Implement hello **IAuthenticate** interface for each platform supported by your app.</span></span>

1. <span data-ttu-id="cc7ed-139">V sadě Visual Studio nebo Xamarin Studio otevřete App.cs z projektu hello s **přenosné** v hello název, který je projektu knihovny přenosných tříd, pak přidejte následující hello `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-139">In Visual Studio or Xamarin Studio, open App.cs from hello project with **Portable** in hello name, which is Portable Class Library project, then  add hello following `using` statement:</span></span>

        using System.Threading.Tasks;
2. <span data-ttu-id="cc7ed-140">V App.cs, přidejte následující hello `IAuthenticate` definici bezprostředně před hello rozhraní `App` definici třídy.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-140">In App.cs, add hello following `IAuthenticate` interface definition immediately before hello `App` class definition.</span></span>

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. <span data-ttu-id="cc7ed-141">tooinitialize hello rozhraní s provádění specifických pro platformy, přidejte následující statické členy toohello hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-141">tooinitialize hello interface with a platform-specific implementation, add hello following static members toohello **App** class.</span></span>

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. <span data-ttu-id="cc7ed-142">Otevřete TodoList.xaml z projektu knihovny přenosných tříd hello, přidejte následující hello **tlačítko** element v hello *buttonsPanel* element rozložení po existující tlačítko hello:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-142">Open TodoList.xaml from hello Portable Class Library project, add hello following **Button** element in hello *buttonsPanel* layout element, after hello existing button:</span></span>

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    <span data-ttu-id="cc7ed-143">Toto tlačítko se aktivuje server spravovaný ověřování s váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-143">This button triggers server-managed authentication with your mobile app backend.</span></span>
5. <span data-ttu-id="cc7ed-144">Otevřete TodoList.xaml.cs z projektu knihovny přenosných tříd hello a pak přidejte následující pole toohello hello `TodoList` třídy:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-144">Open TodoList.xaml.cs from hello Portable Class Library project, then add hello following field toohello `TodoList` class:</span></span>

        // Track whether hello user has authenticated.
        bool authenticated = false;
6. <span data-ttu-id="cc7ed-145">Nahraďte hello **OnAppearing** metoda s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-145">Replace hello **OnAppearing** method with hello following code:</span></span>

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

    <span data-ttu-id="cc7ed-146">Tento kód je zajištěno, že data se pouze aktualizují ze služby hello po jste ověření.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-146">This code makes sure that data is only refreshed from hello service after you have been authenticated.</span></span>
7. <span data-ttu-id="cc7ed-147">Přidejte následující obslužnou rutinu pro hello hello **místní** událostí toohello **TodoList** třídy:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-147">Add hello following handler for hello **Clicked** event toohello **TodoList** class:</span></span>

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems tootrue toosynchronize hello data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. <span data-ttu-id="cc7ed-148">Uložte změny a znovu sestavte projekt knihovny přenosných tříd hello žádné chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-148">Save your changes and rebuild hello Portable Class Library project verifying no errors.</span></span>

## <a name="add-authentication-toohello-android-app"></a><span data-ttu-id="cc7ed-149">Přidat ověřování toohello aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="cc7ed-149">Add authentication toohello Android app</span></span>
<span data-ttu-id="cc7ed-150">Tato část uvádí, jak tooimplement hello **IAuthenticate** rozhraní v projektu aplikace pro Android hello.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-150">This section shows how tooimplement hello **IAuthenticate** interface in hello Android app project.</span></span> <span data-ttu-id="cc7ed-151">Tuto část přeskočte, pokud nejsou podpora zařízení se systémem Android.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-151">Skip this section if you are not supporting Android devices.</span></span>

1. <span data-ttu-id="cc7ed-152">V sadě Visual Studio nebo Xamarin Studio, klikněte pravým tlačítkem na hello **droid** projekt, pak **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-152">In Visual Studio or Xamarin Studio, right-click hello **droid** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="cc7ed-153">Stiskněte klávesu F5 toostart hello projektu v ladicím programu hello a potom ověřte, že k neošetřené výjimce s stavový kód 401 (Neautorizováno) se vyvolá po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-153">Press F5 toostart hello project in hello debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="cc7ed-154">kód 401 Hello se vytvářejí, protože přístup na back-end hello je jenom s omezeným přístupem tooauthorized uživatele.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-154">hello 401 code is produced because access on hello backend is restricted tooauthorized users only.</span></span>
3. <span data-ttu-id="cc7ed-155">Otevřete v projektu Android hello MainActivity.cs a přidejte následující hello `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-155">Open MainActivity.cs in hello Android project and add hello following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. <span data-ttu-id="cc7ed-156">Aktualizace hello **MainActivity** třída tooimplement hello **IAuthenticate** rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-156">Update hello **MainActivity** class tooimplement hello **IAuthenticate** interface, as follows:</span></span>

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. <span data-ttu-id="cc7ed-157">Aktualizace hello **MainActivity** třída přidáním **MobileServiceUser** pole a **ověřit** metoda, který je požadován hello **IAuthenticate**  rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-157">Update hello **MainActivity** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by hello **IAuthenticate** interface, as follows:</span></span>

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

    <span data-ttu-id="cc7ed-158">Pokud používáte zprostředkovatele identity než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider][7].</span><span class="sxs-lookup"><span data-stu-id="cc7ed-158">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider][7].</span></span>

6. <span data-ttu-id="cc7ed-159">Přidejte následující kód do hello <application> uzlu AndroidManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-159">Add hello following code inside <application> node of AndroidManifest.xml:</span></span>

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

1. <span data-ttu-id="cc7ed-160">Přidejte následující kód toohello hello **OnCreate** metoda hello **MainActivity** třídy před hello volání příliš`LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-160">Add hello following code toohello **OnCreate** method of hello **MainActivity** class before hello call too`LoadApplication()`:</span></span>

        // Initialize hello authenticator before loading hello app.
        App.Init((IAuthenticate)this);

    <span data-ttu-id="cc7ed-161">Tento kód zajistí, že ověřovací hello se inicializuje před zatížení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-161">This code ensures hello authenticator is initialized before hello app loads.</span></span>
2. <span data-ttu-id="cc7ed-162">Znovu sestavte aplikaci hello, spusťte ji a pak přihlásit se pomocí zprostředkovatele ověřování hello zvolili a ověřte, zda je možné tooaccess data jako ověřený uživatel.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-162">Rebuild hello app, run it, then sign in with hello authentication provider you chose and verify you are able tooaccess data as an authenticated user.</span></span>

## <a name="add-authentication-toohello-ios-app"></a><span data-ttu-id="cc7ed-163">Přidání ověřování toohello iOS aplikace</span><span class="sxs-lookup"><span data-stu-id="cc7ed-163">Add authentication toohello iOS app</span></span>
<span data-ttu-id="cc7ed-164">Tato část uvádí, jak tooimplement hello **IAuthenticate** rozhraní v projektu iOS aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-164">This section shows how tooimplement hello **IAuthenticate** interface in hello iOS app project.</span></span> <span data-ttu-id="cc7ed-165">Tuto část přeskočte, pokud nejsou podporujete zařízení s iOS.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-165">Skip this section if you are not supporting iOS devices.</span></span>

1. <span data-ttu-id="cc7ed-166">V sadě Visual Studio nebo Xamarin Studio, klikněte pravým tlačítkem na hello **iOS** projekt, pak **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-166">In Visual Studio or Xamarin Studio, right-click hello **iOS** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="cc7ed-167">Stiskněte klávesu F5 toostart hello projektu v ladicím programu hello a potom ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="cc7ed-167">Press F5 toostart hello project in hello debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="cc7ed-168">odpovědi 401 Hello se vytvářejí, protože přístup na back-end hello je jenom s omezeným přístupem tooauthorized uživatele.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-168">hello 401 response is produced because access on hello backend is restricted tooauthorized users only.</span></span>
3. <span data-ttu-id="cc7ed-169">Otevřete v projektu iOS hello AppDelegate.cs a přidejte následující hello `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-169">Open AppDelegate.cs in hello iOS project and add hello following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. <span data-ttu-id="cc7ed-170">Aktualizace hello **AppDelegate** třída tooimplement hello **IAuthenticate** rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-170">Update hello **AppDelegate** class tooimplement hello **IAuthenticate** interface, as follows:</span></span>

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. <span data-ttu-id="cc7ed-171">Aktualizace hello **AppDelegate** třída přidáním **MobileServiceUser** pole a **ověřit** metoda, který je požadován hello **IAuthenticate**  rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-171">Update hello **AppDelegate** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by hello **IAuthenticate** interface, as follows:</span></span>

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

    <span data-ttu-id="cc7ed-172">Pokud používáte zprostředkovatele identity než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider].</span><span class="sxs-lookup"><span data-stu-id="cc7ed-172">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider].</span></span>

6. <span data-ttu-id="cc7ed-173">Aktualizace třídy AppDelegate hello přidáním přetížení metody OpenUrl (UIApplication aplikaci a adresu url nsurl, který, NSDictionary možnosti)</span><span class="sxs-lookup"><span data-stu-id="cc7ed-173">Update hello AppDelegate class by adding OpenUrl(UIApplication app, NSUrl url, NSDictionary options) method overload</span></span>

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }

6. <span data-ttu-id="cc7ed-174">Přidejte následující řádek kódu toohello hello **FinishedLaunching** volání metody před hello příliš`LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-174">Add hello following line of code toohello **FinishedLaunching** method before hello call too`LoadApplication()`:</span></span>

        App.Init(this);

    <span data-ttu-id="cc7ed-175">Tento kód zajistí, že ověřovací hello je inicializována, před načtením aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-175">This code ensures hello authenticator is initialized before hello app is loaded.</span></span>

6. <span data-ttu-id="cc7ed-176">Přidat **{url_scheme_of_your_app}** tooURL schémata v Info.plist.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-176">Add **{url_scheme_of_your_app}** tooURL Schemes in Info.plist.</span></span>

7. <span data-ttu-id="cc7ed-177">Znovu sestavte aplikaci hello, spusťte ji a pak přihlásit se pomocí zprostředkovatele ověřování hello zvolili a ověřte, zda je možné tooaccess data jako ověřený uživatel.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-177">Rebuild hello app, run it, then sign in with hello authentication provider you chose and verify you are able tooaccess data as an authenticated user.</span></span>

## <a name="add-authentication-toowindows-10-including-phone-app-projects"></a><span data-ttu-id="cc7ed-178">Přidat ověřování tooWindows 10 (včetně Phone) projekty aplikací</span><span class="sxs-lookup"><span data-stu-id="cc7ed-178">Add authentication tooWindows 10 (including Phone) app projects</span></span>
<span data-ttu-id="cc7ed-179">Tato část uvádí, jak tooimplement hello **IAuthenticate** rozhraní v projektech aplikace hello Windows 10.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-179">This section shows how tooimplement hello **IAuthenticate** interface in hello Windows 10 app projects.</span></span> <span data-ttu-id="cc7ed-180">stejný postup platí pro univerzální platformu Windows (UWP) projekty, ale používá hello Hello **UWP** projektu (s uvedené změny).</span><span class="sxs-lookup"><span data-stu-id="cc7ed-180">hello same steps apply for Universal Windows Platform (UWP) projects, but using hello **UWP** project (with noted changes).</span></span> <span data-ttu-id="cc7ed-181">Tuto část přeskočte, pokud nejsou podpora zařízení se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-181">Skip this section if you are not supporting Windows devices.</span></span>

1. <span data-ttu-id="cc7ed-182">"V sadě Visual Studio, klikněte pravým tlačítkem na buď hello **UWP** projekt, pak **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-182">"In Visual Studio, right-click either hello **UWP** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="cc7ed-183">Stiskněte klávesu F5 toostart hello projektu v ladicím programu hello a potom ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="cc7ed-183">Press F5 toostart hello project in hello debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="cc7ed-184">odpovědi 401 Hello se stane, protože přístup na back-end hello je jenom s omezeným přístupem tooauthorized uživatele.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-184">hello 401 response happens because access on hello backend is restricted tooauthorized users only.</span></span>
3. <span data-ttu-id="cc7ed-185">Otevřete MainPage.xaml.cs pro projekt aplikace Windows hello a přidejte následující hello `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-185">Open MainPage.xaml.cs for hello Windows app project and add hello following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    <span data-ttu-id="cc7ed-186">Nahraďte `<your_Portable_Class_Library_namespace>` s hello obor názvů pro knihovny přenosných tříd.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-186">Replace `<your_Portable_Class_Library_namespace>` with hello namespace for your portable class library.</span></span>
4. <span data-ttu-id="cc7ed-187">Aktualizace hello **MainPage** třída tooimplement hello **IAuthenticate** rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-187">Update hello **MainPage** class tooimplement hello **IAuthenticate** interface, as follows:</span></span>

        public sealed partial class MainPage : IAuthenticate
5. <span data-ttu-id="cc7ed-188">Aktualizace hello **MainPage** třída přidáním **MobileServiceUser** pole a **ověřit** metoda, který je požadován hello **IAuthenticate** rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-188">Update hello **MainPage** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by hello **IAuthenticate** interface, as follows:</span></span>

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

    <span data-ttu-id="cc7ed-189">Pokud používáte zprostředkovatele identity než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider].</span><span class="sxs-lookup"><span data-stu-id="cc7ed-189">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider].</span></span>

1. <span data-ttu-id="cc7ed-190">Přidejte následující řádek kódu hello konstruktor hello hello **MainPage** třídy před hello volání příliš`LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-190">Add hello following line of code in hello constructor for hello **MainPage** class before hello call too`LoadApplication()`:</span></span>

        // Initialize hello authenticator before loading hello app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    <span data-ttu-id="cc7ed-191">Nahraďte `<your_Portable_Class_Library_namespace>` s hello obor názvů pro knihovny přenosných tříd.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-191">Replace `<your_Portable_Class_Library_namespace>` with hello namespace for your portable class library.</span></span>

3. <span data-ttu-id="cc7ed-192">Pokud používáte **UWP**, přidejte následující hello **OnActivated** metoda přepsání toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-192">If you are using **UWP**, add hello following **OnActivated** method override toohello **App** class:</span></span>

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }

       }

   <span data-ttu-id="cc7ed-193">Při přepsání metody hello již existuje, přidáte kód podmíněného hello z hello předchozím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-193">When hello method override already exists, add hello conditional code from hello preceding snippet.</span></span>  <span data-ttu-id="cc7ed-194">Tento kód se nevyžaduje pro projekty pro Universal Windows.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-194">This code is not required for Universal Windows projects.</span></span>

3. <span data-ttu-id="cc7ed-195">Přidat **{url_scheme_of_your_app}** v Package.appxmanifest.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-195">Add **{url_scheme_of_your_app}** in Package.appxmanifest.</span></span> 

4. <span data-ttu-id="cc7ed-196">Znovu sestavte aplikaci hello, spusťte ji a pak přihlásit se pomocí zprostředkovatele ověřování hello zvolili a ověřte, zda je možné tooaccess data jako ověřený uživatel.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-196">Rebuild hello app, run it, then sign in with hello authentication provider you chose and verify you are able tooaccess data as an authenticated user.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc7ed-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cc7ed-197">Next steps</span></span>
<span data-ttu-id="cc7ed-198">Teď, když jste dokončili kurz základní ověřování, zvažte, budete pokračovat v tooone hello následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="cc7ed-198">Now that you completed this basic authentication tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="cc7ed-199">Přidat nabízená oznámení tooyour aplikaci</span><span class="sxs-lookup"><span data-stu-id="cc7ed-199">Add push notifications tooyour app</span></span>](app-service-mobile-xamarin-forms-get-started-push.md)

  <span data-ttu-id="cc7ed-200">Zjistěte, jak tooadd nabízená oznámení podporují tooyour aplikace a konfigurace mobilní aplikace back-end toouse Azure Notification Hubs toosend nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-200">Learn how tooadd push notifications support tooyour app and configure your Mobile App backend toouse Azure Notification Hubs toosend push notifications.</span></span>
* [<span data-ttu-id="cc7ed-201">Povolení offline synchronizace u aplikace</span><span class="sxs-lookup"><span data-stu-id="cc7ed-201">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  <span data-ttu-id="cc7ed-202">Zjistěte, jak tooadd offline podporují aplikace pomocí back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-202">Learn how tooadd offline support your app using a Mobile App backend.</span></span> <span data-ttu-id="cc7ed-203">Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací - zobrazení, přidávat a upravovat data - i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="cc7ed-203">Offline sync allows end users toointeract with a mobile app - viewing, adding, or modifying data - even when there is no network connection.</span></span>

<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-xamarin-forms-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[5]: app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx
