---
title: "Začínáme s ověřováním mobilních aplikací v aplikaci Xamarin Forms | Microsoft Docs"
description: "Další informace o použití mobilní aplikace ověřovat uživatele vaší aplikace Xamarin Forms prostřednictvím řady různých zprostředkovatelů identity, včetně AAD, Google, Facebook, Twitter a Microsoft."
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
ms.openlocfilehash: 9e14e95793bcc81ad46783fd50ba223eec4ea360
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="add-authentication-to-your-xamarin-forms-app"></a><span data-ttu-id="3f22f-103">Přidání ověřování do aplikace Xamarin Forms</span><span class="sxs-lookup"><span data-stu-id="3f22f-103">Add authentication to your Xamarin Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a><span data-ttu-id="3f22f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3f22f-104">Overview</span></span>
<span data-ttu-id="3f22f-105">Toto téma ukazuje, jak ověřovat uživatele aplikace služby mobilní aplikace z klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f22f-105">This topic shows you how to authenticate users of an App Service Mobile App from your client application.</span></span> <span data-ttu-id="3f22f-106">V tomto kurzu přidání ověřování do projektu pro rychlý start Xamarin Forms pomocí zprostředkovatele identity, která je podporována službou App Service.</span><span class="sxs-lookup"><span data-stu-id="3f22f-106">In this tutorial, you add authentication to the Xamarin Forms quickstart project using an identity provider that is supported by App Service.</span></span> <span data-ttu-id="3f22f-107">Poté, co se úspěšně ověří a autorizuje pomocí mobilní aplikace, zobrazí se hodnota ID uživatele a bude mít přístup k datům s omezeným přístupem tabulky.</span><span class="sxs-lookup"><span data-stu-id="3f22f-107">After being successfully authenticated and authorized by your Mobile App, the user ID value is displayed, and you will be able to access restricted table data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f22f-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3f22f-108">Prerequisites</span></span>
<span data-ttu-id="3f22f-109">Nejlepších výsledků v tomto kurzu, doporučujeme, abyste nejdříve dokončili [vytvoření aplikace pro Xamarin Forms] [ 1] kurzu.</span><span class="sxs-lookup"><span data-stu-id="3f22f-109">For the best result with this tutorial, we recommend that you first complete the [Create a Xamarin Forms app][1] tutorial.</span></span> <span data-ttu-id="3f22f-110">Po dokončení tohoto kurzu budete mít projekt Xamarin Forms, který je aplikace seznamu úkolů více platformami.</span><span class="sxs-lookup"><span data-stu-id="3f22f-110">After you complete this tutorial, you will have a Xamarin Forms project that is a multi-platform TodoList app.</span></span>

<span data-ttu-id="3f22f-111">Pokud použijete serverový projekt stažené rychlý start, musíte přidat balíček rozšíření ověřování do projektu.</span><span class="sxs-lookup"><span data-stu-id="3f22f-111">If you do not use the downloaded quick start server project, you must add the authentication extension package to your project.</span></span> <span data-ttu-id="3f22f-112">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="3f22f-112">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps][2].</span></span>

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><span data-ttu-id="3f22f-113">Registrace aplikace pro ověřování a nakonfigurujte aplikační služby</span><span class="sxs-lookup"><span data-stu-id="3f22f-113">Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="3f22f-114"><a name="redirecturl"></a>Přidání aplikace do adresy URL pro povolené externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="3f22f-114"><a name="redirecturl"></a>Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="3f22f-115">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3f22f-115">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="3f22f-116">To umožňuje ověřování systému přesměrovat zpět do aplikace po dokončení procesu ověřování.</span><span class="sxs-lookup"><span data-stu-id="3f22f-116">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="3f22f-117">V tomto kurzu používáme schématu adresy URL _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="3f22f-117">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="3f22f-118">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="3f22f-118">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="3f22f-119">Musí být jedinečné pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f22f-119">It should be unique to your mobile application.</span></span> <span data-ttu-id="3f22f-120">Chcete povolit přesměrování na straně serveru:</span><span class="sxs-lookup"><span data-stu-id="3f22f-120">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="3f22f-121">V [portál Azure] vyberte App Service.</span><span class="sxs-lookup"><span data-stu-id="3f22f-121">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="3f22f-122">Klikněte **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="3f22f-122">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="3f22f-123">V **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="3f22f-123">In the **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="3f22f-124">**Url_scheme_of_your_app** v tento řetězec je schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f22f-124">The **url_scheme_of_your_app** in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="3f22f-125">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="3f22f-125">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="3f22f-126">Měli byste si poznamenat řetězce, který zvolíte, jako je třeba upravit kód mobilní aplikace s schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="3f22f-126">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="3f22f-127">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f22f-127">Click **OK**.</span></span>

5. <span data-ttu-id="3f22f-128">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3f22f-128">Click **Save**.</span></span>

## <a name="restrict-permissions-to-authenticated-users"></a><span data-ttu-id="3f22f-129">Omezit oprávnění k ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="3f22f-129">Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-to-the-portable-class-library"></a><span data-ttu-id="3f22f-130">Přidání ověřování do knihovny přenosných tříd</span><span class="sxs-lookup"><span data-stu-id="3f22f-130">Add authentication to the portable class library</span></span>
<span data-ttu-id="3f22f-131">Mobilní aplikace používá [LoginAsync] [ 3] rozšiřující metody na [MobileServiceClient] [ 4] k přihlášení uživatele pomocí ověřování služby App Service.</span><span class="sxs-lookup"><span data-stu-id="3f22f-131">Mobile Apps uses the [LoginAsync][3] extension method on the [MobileServiceClient][4] to sign in a user with App Service authentication.</span></span> <span data-ttu-id="3f22f-132">Tato ukázka používá toku spravovaný server ověřování, který zobrazí poskytovatele přihlášení rozhraní v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3f22f-132">This sample uses a server-managed authentication flow that displays the provider's sign-in interface in the app.</span></span> <span data-ttu-id="3f22f-133">Další informace najdete v tématu [spravovaný Server ověřování][5].</span><span class="sxs-lookup"><span data-stu-id="3f22f-133">For more information, see [Server-managed authentication][5].</span></span> <span data-ttu-id="3f22f-134">Chcete-li zajistit lepší prostředí uživatele ve vaší aplikaci produkční, měli byste zvážit namísto použití [klienta spravovat ověřování][6].</span><span class="sxs-lookup"><span data-stu-id="3f22f-134">To provide a better user experience in your production app, you should consider instead using [Client-managed authentication][6].</span></span>

<span data-ttu-id="3f22f-135">K ověření s projektem Xamarin Forms, definovat **IAuthenticate** rozhraní na přenosné knihovny tříd pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3f22f-135">To authenticate with a Xamarin Forms project, define an **IAuthenticate** interface in the Portable Class Library for the app.</span></span> <span data-ttu-id="3f22f-136">Pak přidejte **přihlášení** tlačítko uživatelského rozhraní definované v přenosných třída knihovny, která kliknutí na tlačítko spustit ověřování.</span><span class="sxs-lookup"><span data-stu-id="3f22f-136">Then add a **Sign-in** button to the user interface defined in the Portable Class Library, which you click to start authentication.</span></span> <span data-ttu-id="3f22f-137">Data jsou načítána z back-end mobilní aplikace po úspěšném ověření.</span><span class="sxs-lookup"><span data-stu-id="3f22f-137">Data is loaded from the mobile app backend after successful authentication.</span></span>

<span data-ttu-id="3f22f-138">Implementace **IAuthenticate** rozhraní pro každou platformu podporovanou nástrojem vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f22f-138">Implement the **IAuthenticate** interface for each platform supported by your app.</span></span>

1. <span data-ttu-id="3f22f-139">V sadě Visual Studio nebo Xamarin Studio otevřete App.cs z projektu s **přenosné** v názvu, který je projektu knihovny přenosných tříd, přidejte následující `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="3f22f-139">In Visual Studio or Xamarin Studio, open App.cs from the project with **Portable** in the name, which is Portable Class Library project, then  add the following `using` statement:</span></span>

        using System.Threading.Tasks;
2. <span data-ttu-id="3f22f-140">V App.cs, přidejte následující `IAuthenticate` definici rozhraní bezprostředně před `App` definici třídy.</span><span class="sxs-lookup"><span data-stu-id="3f22f-140">In App.cs, add the following `IAuthenticate` interface definition immediately before the `App` class definition.</span></span>

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. <span data-ttu-id="3f22f-141">K chybě při inicializaci rozhraní s implementací specifických pro platformy, přidejte následující statické členy **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="3f22f-141">To initialize the interface with a platform-specific implementation, add the following static members to the **App** class.</span></span>

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. <span data-ttu-id="3f22f-142">Otevřete TodoList.xaml z projektu knihovny přenosných tříd, přidejte následující **tlačítko** element v *buttonsPanel* element rozložení po existující tlačítko:</span><span class="sxs-lookup"><span data-stu-id="3f22f-142">Open TodoList.xaml from the Portable Class Library project, add the following **Button** element in the *buttonsPanel* layout element, after the existing button:</span></span>

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    <span data-ttu-id="3f22f-143">Toto tlačítko se aktivuje server spravovaný ověřování s váš back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f22f-143">This button triggers server-managed authentication with your mobile app backend.</span></span>
5. <span data-ttu-id="3f22f-144">Otevřete TodoList.xaml.cs z projektu knihovny přenosných tříd a pak přidejte následující pole, které chcete `TodoList` třídy:</span><span class="sxs-lookup"><span data-stu-id="3f22f-144">Open TodoList.xaml.cs from the Portable Class Library project, then add the following field to the `TodoList` class:</span></span>

        // Track whether the user has authenticated.
        bool authenticated = false;
6. <span data-ttu-id="3f22f-145">Nahraďte **OnAppearing** metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="3f22f-145">Replace the **OnAppearing** method with the following code:</span></span>

        protected override async void OnAppearing()
        {
            base.OnAppearing();

            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    <span data-ttu-id="3f22f-146">Tento kód je zajištěno, že data se pouze aktualizují ze služby po jste ověření.</span><span class="sxs-lookup"><span data-stu-id="3f22f-146">This code makes sure that data is only refreshed from the service after you have been authenticated.</span></span>
7. <span data-ttu-id="3f22f-147">Přidejte následující obslužnou rutinu pro **místní** událost, která má **TodoList** třídy:</span><span class="sxs-lookup"><span data-stu-id="3f22f-147">Add the following handler for the **Clicked** event to the **TodoList** class:</span></span>

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. <span data-ttu-id="3f22f-148">Uložte změny a znovu sestavte projekt knihovny přenosných tříd žádné chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="3f22f-148">Save your changes and rebuild the Portable Class Library project verifying no errors.</span></span>

## <a name="add-authentication-to-the-android-app"></a><span data-ttu-id="3f22f-149">Přidání ověřování do aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="3f22f-149">Add authentication to the Android app</span></span>
<span data-ttu-id="3f22f-150">V této části ukazuje, jak implementovat **IAuthenticate** rozhraní v projektu aplikace pro Android.</span><span class="sxs-lookup"><span data-stu-id="3f22f-150">This section shows how to implement the **IAuthenticate** interface in the Android app project.</span></span> <span data-ttu-id="3f22f-151">Tuto část přeskočte, pokud nejsou podpora zařízení se systémem Android.</span><span class="sxs-lookup"><span data-stu-id="3f22f-151">Skip this section if you are not supporting Android devices.</span></span>

1. <span data-ttu-id="3f22f-152">V sadě Visual Studio nebo Xamarin Studio, klikněte pravým tlačítkem **droid** projekt, pak **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="3f22f-152">In Visual Studio or Xamarin Studio, right-click the **droid** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="3f22f-153">Stisknutím klávesy F5 spusťte projekt v ladicím programu a potom ověřte, že k neošetřené výjimce s stavový kód 401 (Neautorizováno) se vyvolá po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f22f-153">Press F5 to start the project in the debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="3f22f-154">Kód 401 je vytvořen, protože přístup na back-end je omezený pouze autorizovaným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="3f22f-154">The 401 code is produced because access on the backend is restricted to authorized users only.</span></span>
3. <span data-ttu-id="3f22f-155">Otevřete MainActivity.cs v projektu pro Android a přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="3f22f-155">Open MainActivity.cs in the Android project and add the following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. <span data-ttu-id="3f22f-156">Aktualizace **MainActivity** třídu pro implementaci **IAuthenticate** rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3f22f-156">Update the **MainActivity** class to implement the **IAuthenticate** interface, as follows:</span></span>

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. <span data-ttu-id="3f22f-157">Aktualizace **MainActivity** třída přidáním **MobileServiceUser** pole a **ověřit** metoda, který je požadován **IAuthenticate** rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3f22f-157">Update the **MainActivity** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by the **IAuthenticate** interface, as follows:</span></span>

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

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }

    <span data-ttu-id="3f22f-158">Pokud používáte zprostředkovatele identity než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider][7].</span><span class="sxs-lookup"><span data-stu-id="3f22f-158">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider][7].</span></span>

6. <span data-ttu-id="3f22f-159">Přidejte následující kód do <application> uzlu AndroidManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="3f22f-159">Add the following code inside <application> node of AndroidManifest.xml:</span></span>

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

1. <span data-ttu-id="3f22f-160">Přidejte následující kód, který **OnCreate** metodu **MainActivity** třída před voláním `LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="3f22f-160">Add the following code to the **OnCreate** method of the **MainActivity** class before the call to `LoadApplication()`:</span></span>

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    <span data-ttu-id="3f22f-161">Tento kód zajistí, že ověřovacích se inicializuje před zatížení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f22f-161">This code ensures the authenticator is initialized before the app loads.</span></span>
2. <span data-ttu-id="3f22f-162">Znovu sestavte aplikaci, spusťte ji a pak přihlásit se pomocí zprostředkovatele ověřování, které jste zvolili a ověřte, zda že budete moci získat přístup k datům jako ověřený uživatel.</span><span class="sxs-lookup"><span data-stu-id="3f22f-162">Rebuild the app, run it, then sign in with the authentication provider you chose and verify you are able to access data as an authenticated user.</span></span>

## <a name="add-authentication-to-the-ios-app"></a><span data-ttu-id="3f22f-163">Přidání ověřování do aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="3f22f-163">Add authentication to the iOS app</span></span>
<span data-ttu-id="3f22f-164">V této části ukazuje, jak implementovat **IAuthenticate** rozhraní v projektu aplikace pro iOS.</span><span class="sxs-lookup"><span data-stu-id="3f22f-164">This section shows how to implement the **IAuthenticate** interface in the iOS app project.</span></span> <span data-ttu-id="3f22f-165">Tuto část přeskočte, pokud nejsou podporujete zařízení s iOS.</span><span class="sxs-lookup"><span data-stu-id="3f22f-165">Skip this section if you are not supporting iOS devices.</span></span>

1. <span data-ttu-id="3f22f-166">V sadě Visual Studio nebo Xamarin Studio, klikněte pravým tlačítkem **iOS** projekt, pak **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="3f22f-166">In Visual Studio or Xamarin Studio, right-click the **iOS** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="3f22f-167">Stisknutím klávesy F5 spusťte projekt v ladicím programu a potom ověřte, že k neošetřené výjimce s stavový kód 401 (Neautorizováno) se vyvolá po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f22f-167">Press F5 to start the project in the debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="3f22f-168">Odpovědi 401 je vytvořen, protože přístup na back-end je omezený pouze autorizovaným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="3f22f-168">The 401 response is produced because access on the backend is restricted to authorized users only.</span></span>
3. <span data-ttu-id="3f22f-169">Otevřete v projektu iOS AppDelegate.cs a přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="3f22f-169">Open AppDelegate.cs in the iOS project and add the following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. <span data-ttu-id="3f22f-170">Aktualizace **AppDelegate** třídu pro implementaci **IAuthenticate** rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3f22f-170">Update the **AppDelegate** class to implement the **IAuthenticate** interface, as follows:</span></span>

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. <span data-ttu-id="3f22f-171">Aktualizace **AppDelegate** třída přidáním **MobileServiceUser** pole a **ověřit** metoda, který je požadován **IAuthenticate** rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3f22f-171">Update the **AppDelegate** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by the **IAuthenticate** interface, as follows:</span></span>

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

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();

            return success;
        }

    <span data-ttu-id="3f22f-172">Pokud používáte zprostředkovatele identity než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider].</span><span class="sxs-lookup"><span data-stu-id="3f22f-172">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider].</span></span>

6. <span data-ttu-id="3f22f-173">Aktualizace třídy AppDelegate přidáním přetížení metody OpenUrl (UIApplication aplikaci a adresu url nsurl, který, NSDictionary možnosti)</span><span class="sxs-lookup"><span data-stu-id="3f22f-173">Update the AppDelegate class by adding OpenUrl(UIApplication app, NSUrl url, NSDictionary options) method overload</span></span>

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }

6. <span data-ttu-id="3f22f-174">Přidejte následující řádek kódu **FinishedLaunching** metody před volání `LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="3f22f-174">Add the following line of code to the **FinishedLaunching** method before the call to `LoadApplication()`:</span></span>

        App.Init(this);

    <span data-ttu-id="3f22f-175">Tento kód zajistí, že je ověřovacích inicializovat před načtením aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f22f-175">This code ensures the authenticator is initialized before the app is loaded.</span></span>

6. <span data-ttu-id="3f22f-176">Přidat **{url_scheme_of_your_app}** na schémata URL v Info.plist.</span><span class="sxs-lookup"><span data-stu-id="3f22f-176">Add **{url_scheme_of_your_app}** to URL Schemes in Info.plist.</span></span>

7. <span data-ttu-id="3f22f-177">Znovu sestavte aplikaci, spusťte ji a pak přihlásit se pomocí zprostředkovatele ověřování, které jste zvolili a ověřte, zda že budete moci získat přístup k datům jako ověřený uživatel.</span><span class="sxs-lookup"><span data-stu-id="3f22f-177">Rebuild the app, run it, then sign in with the authentication provider you chose and verify you are able to access data as an authenticated user.</span></span>

## <a name="add-authentication-to-windows-10-including-phone-app-projects"></a><span data-ttu-id="3f22f-178">Přidání ověřování do aplikace projekty Windows 10 (včetně Phone)</span><span class="sxs-lookup"><span data-stu-id="3f22f-178">Add authentication to Windows 10 (including Phone) app projects</span></span>
<span data-ttu-id="3f22f-179">V této části ukazuje, jak implementovat **IAuthenticate** rozhraní v projektech aplikace Windows 10.</span><span class="sxs-lookup"><span data-stu-id="3f22f-179">This section shows how to implement the **IAuthenticate** interface in the Windows 10 app projects.</span></span> <span data-ttu-id="3f22f-180">Stejný postup platí pro univerzální platformu Windows (UWP) projekty, ale pomocí **UWP** projektu (s uvedené změny).</span><span class="sxs-lookup"><span data-stu-id="3f22f-180">The same steps apply for Universal Windows Platform (UWP) projects, but using the **UWP** project (with noted changes).</span></span> <span data-ttu-id="3f22f-181">Tuto část přeskočte, pokud nejsou podpora zařízení se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="3f22f-181">Skip this section if you are not supporting Windows devices.</span></span>

1. <span data-ttu-id="3f22f-182">"V sadě Visual Studio, klikněte pravým tlačítkem na buď **UWP** projekt, pak **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="3f22f-182">"In Visual Studio, right-click either the **UWP** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="3f22f-183">Stisknutím klávesy F5 spusťte projekt v ladicím programu a potom ověřte, že k neošetřené výjimce s stavový kód 401 (Neautorizováno) se vyvolá po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f22f-183">Press F5 to start the project in the debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="3f22f-184">Odpovědi 401 se stane, protože přístup na back-end je omezený pouze autorizovaným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="3f22f-184">The 401 response happens because access on the backend is restricted to authorized users only.</span></span>
3. <span data-ttu-id="3f22f-185">Otevřete MainPage.xaml.cs pro projekt aplikace Windows a přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="3f22f-185">Open MainPage.xaml.cs for the Windows app project and add the following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    <span data-ttu-id="3f22f-186">Nahraďte `<your_Portable_Class_Library_namespace>` s oborem názvů pro knihovny přenosných tříd.</span><span class="sxs-lookup"><span data-stu-id="3f22f-186">Replace `<your_Portable_Class_Library_namespace>` with the namespace for your portable class library.</span></span>
4. <span data-ttu-id="3f22f-187">Aktualizace **MainPage** třídu pro implementaci **IAuthenticate** rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3f22f-187">Update the **MainPage** class to implement the **IAuthenticate** interface, as follows:</span></span>

        public sealed partial class MainPage : IAuthenticate
5. <span data-ttu-id="3f22f-188">Aktualizace **MainPage** třída přidáním **MobileServiceUser** pole a **ověřit** metoda, který je požadován **IAuthenticate** rozhraní, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3f22f-188">Update the **MainPage** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by the **IAuthenticate** interface, as follows:</span></span>

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

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }

    <span data-ttu-id="3f22f-189">Pokud používáte zprostředkovatele identity než Facebook, zvolte jinou hodnotu pro [MobileServiceAuthenticationProvider].</span><span class="sxs-lookup"><span data-stu-id="3f22f-189">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider].</span></span>

1. <span data-ttu-id="3f22f-190">Přidejte následující řádek kódu v konstruktoru pro **MainPage** třída před voláním `LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="3f22f-190">Add the following line of code in the constructor for the **MainPage** class before the call to `LoadApplication()`:</span></span>

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    <span data-ttu-id="3f22f-191">Nahraďte `<your_Portable_Class_Library_namespace>` s oborem názvů pro knihovny přenosných tříd.</span><span class="sxs-lookup"><span data-stu-id="3f22f-191">Replace `<your_Portable_Class_Library_namespace>` with the namespace for your portable class library.</span></span>

3. <span data-ttu-id="3f22f-192">Pokud používáte **UWP**, přidejte následující **OnActivated** metoda přepsání nastavení za účelem **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="3f22f-192">If you are using **UWP**, add the following **OnActivated** method override to the **App** class:</span></span>

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }

       }

   <span data-ttu-id="3f22f-193">Při přepsání metody již existuje, přidáte kód podmíněného z předchozí fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="3f22f-193">When the method override already exists, add the conditional code from the preceding snippet.</span></span>  <span data-ttu-id="3f22f-194">Tento kód se nevyžaduje pro projekty pro Universal Windows.</span><span class="sxs-lookup"><span data-stu-id="3f22f-194">This code is not required for Universal Windows projects.</span></span>

3. <span data-ttu-id="3f22f-195">Přidat **{url_scheme_of_your_app}** v Package.appxmanifest.</span><span class="sxs-lookup"><span data-stu-id="3f22f-195">Add **{url_scheme_of_your_app}** in Package.appxmanifest.</span></span> 

4. <span data-ttu-id="3f22f-196">Znovu sestavte aplikaci, spusťte ji a pak přihlásit se pomocí zprostředkovatele ověřování, které jste zvolili a ověřte, zda že budete moci získat přístup k datům jako ověřený uživatel.</span><span class="sxs-lookup"><span data-stu-id="3f22f-196">Rebuild the app, run it, then sign in with the authentication provider you chose and verify you are able to access data as an authenticated user.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f22f-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f22f-197">Next steps</span></span>
<span data-ttu-id="3f22f-198">Teď, když jste dokončili kurz základní ověřování, vezměte v úvahu pokračovat na jednu z následujících kurzů:</span><span class="sxs-lookup"><span data-stu-id="3f22f-198">Now that you completed this basic authentication tutorial, consider continuing on to one of the following tutorials:</span></span>

* [<span data-ttu-id="3f22f-199">Přidání nabízených oznámení do aplikace</span><span class="sxs-lookup"><span data-stu-id="3f22f-199">Add push notifications to your app</span></span>](app-service-mobile-xamarin-forms-get-started-push.md)

  <span data-ttu-id="3f22f-200">Naučte se přidávat do aplikace podporu nabízených oznámení a konfigurovat back-end mobilní aplikace tak, aby k zasílání nabízených oznámení používal Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="3f22f-200">Learn how to add push notifications support to your app and configure your Mobile App backend to use Azure Notification Hubs to send push notifications.</span></span>
* [<span data-ttu-id="3f22f-201">Povolení offline synchronizace u aplikace</span><span class="sxs-lookup"><span data-stu-id="3f22f-201">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  <span data-ttu-id="3f22f-202">Informace o postupu přidání vaší aplikace pomocí back-end mobilní aplikace podporu offline režimu.</span><span class="sxs-lookup"><span data-stu-id="3f22f-202">Learn how to add offline support your app using a Mobile App backend.</span></span> <span data-ttu-id="3f22f-203">Offline synchronizace umožňuje koncovým uživatelům pracovat s mobilní aplikací - zobrazení, přidávat a upravovat data - i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="3f22f-203">Offline sync allows end users to interact with a mobile app - viewing, adding, or modifying data - even when there is no network connection.</span></span>

<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-xamarin-forms-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[5]: app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx
