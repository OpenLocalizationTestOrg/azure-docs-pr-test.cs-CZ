---
title: "Začínáme s ověřováním pro Mobile Apps v Xamarin iOS"
description: "Další informace o použití mobilní aplikace ověřovat uživatele vaší aplikace Xamarin iOS prostřednictvím řady různých zprostředkovatelů identity, včetně AAD, Google, Facebook, Twitter a Microsoft."
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
ms.openlocfilehash: 454b2df5a9bf8cfba93befea54370957ab044d95
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-xamarinios-app"></a><span data-ttu-id="ff46b-103">Přidání ověřování do aplikace Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="ff46b-103">Add authentication to your Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="ff46b-104">Toto téma ukazuje, jak ověřovat uživatele aplikace služby mobilní aplikace z klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff46b-104">This topic shows you how to authenticate users of an App Service Mobile App from your client application.</span></span> <span data-ttu-id="ff46b-105">V tomto kurzu přidání ověřování do projektu pro rychlý start Xamarin.iOS pomocí zprostředkovatele identity, která je podporována službou App Service.</span><span class="sxs-lookup"><span data-stu-id="ff46b-105">In this tutorial, you add authentication to the Xamarin.iOS quickstart project using an identity provider that is supported by App Service.</span></span> <span data-ttu-id="ff46b-106">Poté, co se úspěšně ověří a autorizuje pomocí mobilní aplikace, zobrazí se hodnota ID uživatele a bude mít přístup k datům s omezeným přístupem tabulky.</span><span class="sxs-lookup"><span data-stu-id="ff46b-106">After being successfully authenticated and authorized by your Mobile App, the user ID value is displayed and you will be able to access restricted table data.</span></span>

<span data-ttu-id="ff46b-107">Musíte nejdřív dokončit tento kurz [vytvoření aplikace Xamarin.iOS].</span><span class="sxs-lookup"><span data-stu-id="ff46b-107">You must first complete the tutorial [Create a Xamarin.iOS app].</span></span> <span data-ttu-id="ff46b-108">Pokud použijete serverový projekt stažené rychlý start, musíte přidat balíček rozšíření ověřování do projektu.</span><span class="sxs-lookup"><span data-stu-id="ff46b-108">If you do not use the downloaded quick start server project, you must add the authentication extension package to your project.</span></span> <span data-ttu-id="ff46b-109">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="ff46b-109">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><span data-ttu-id="ff46b-110">Registrace aplikace pro ověřování a nakonfigurujte aplikační služby</span><span class="sxs-lookup"><span data-stu-id="ff46b-110">Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a><span data-ttu-id="ff46b-111">Přidání aplikace do adresy URL pro povolené externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="ff46b-111">Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="ff46b-112">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ff46b-112">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="ff46b-113">To umožňuje ověřování systému přesměrovat zpět do aplikace po dokončení procesu ověřování.</span><span class="sxs-lookup"><span data-stu-id="ff46b-113">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="ff46b-114">V tomto kurzu používáme schématu adresy URL _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="ff46b-114">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="ff46b-115">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="ff46b-115">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="ff46b-116">Musí být jedinečné pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff46b-116">It should be unique to your mobile application.</span></span> <span data-ttu-id="ff46b-117">Chcete povolit přesměrování na straně serveru:</span><span class="sxs-lookup"><span data-stu-id="ff46b-117">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="ff46b-118">V [portál Azure] vyberte App Service.</span><span class="sxs-lookup"><span data-stu-id="ff46b-118">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="ff46b-119">Klikněte **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="ff46b-119">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="ff46b-120">V **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="ff46b-120">In the **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="ff46b-121">**Url_scheme_of_your_app** v tento řetězec je schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff46b-121">The **url_scheme_of_your_app** in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="ff46b-122">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="ff46b-122">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="ff46b-123">Měli byste si poznamenat řetězce, který zvolíte, jako je třeba upravit kód mobilní aplikace s schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="ff46b-123">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="ff46b-124">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff46b-124">Click **OK**.</span></span>

5. <span data-ttu-id="ff46b-125">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ff46b-125">Click **Save**.</span></span>

## <a name="restrict-permissions-to-authenticated-users"></a><span data-ttu-id="ff46b-126">Omezit oprávnění k ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="ff46b-126">Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="ff46b-127">&nbsp;&nbsp;4.</span><span class="sxs-lookup"><span data-stu-id="ff46b-127">&nbsp;&nbsp;4.</span></span> <span data-ttu-id="ff46b-128">V sadě Visual Studio nebo Xamarin Studio spuštění klientského projektu na emulátoru nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="ff46b-128">In Visual Studio or Xamarin Studio, run the client project on a device or emulator.</span></span> <span data-ttu-id="ff46b-129">Ověřte, že k neošetřené výjimce s stavový kód 401 (Neautorizováno) se vyvolá po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff46b-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="ff46b-130">Toto selhání se zaprotokoluje ke konzole ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="ff46b-130">The failure is logged to the console of the debugger.</span></span> <span data-ttu-id="ff46b-131">Proto v sadě Visual Studio, měli byste vidět selhání v okně výstupu.</span><span class="sxs-lookup"><span data-stu-id="ff46b-131">So in Visual Studio, you should see the failure in the output window.</span></span>

<span data-ttu-id="ff46b-132">&nbsp;&nbsp;Toto selhání neoprávněným se stane, protože se aplikace pokusí o přístup k váš back-end mobilní aplikace jako neověřený uživatel.</span><span class="sxs-lookup"><span data-stu-id="ff46b-132">&nbsp;&nbsp;This unauthorized failure happens because the app attempts to access your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="ff46b-133">*TodoItem* tabulka nyní vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="ff46b-133">The *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="ff46b-134">Potom bude aktualizujte klientskou aplikaci pro požadavky na prostředky z back-end mobilní aplikace s ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ff46b-134">Next, you will update the client app to request resources from the Mobile App backend with an authenticated user.</span></span>

## <a name="add-authentication-to-the-app"></a><span data-ttu-id="ff46b-135">Přidání ověřování do aplikace</span><span class="sxs-lookup"><span data-stu-id="ff46b-135">Add authentication to the app</span></span>
<span data-ttu-id="ff46b-136">V této části upravíte aplikace zobrazíte obrazovka pro přihlášení, než se zobrazí data.</span><span class="sxs-lookup"><span data-stu-id="ff46b-136">In this section, you will modify the app to display a login screen before displaying data.</span></span> <span data-ttu-id="ff46b-137">Při spuštění aplikace, nebude se připojit do vaší služby App Service a nebudou zobrazovat žádná data.</span><span class="sxs-lookup"><span data-stu-id="ff46b-137">When the app starts, it will not not connect to your App Service and will not display any data.</span></span> <span data-ttu-id="ff46b-138">Po prvním uživatel provádí aktualizace gesto, zobrazí se obrazovka pro přihlášení; Po úspěšném přihlášení se zobrazí seznam položek todo.</span><span class="sxs-lookup"><span data-stu-id="ff46b-138">After the first time that the user performs the refresh gesture, the login screen will appear; after successful login the list of todo items will be displayed.</span></span>

1. <span data-ttu-id="ff46b-139">V projektu klienta, otevřete soubor **QSTodoService.cs** a přidejte následující příkaz using a `MobileServiceUser` s přistupujícím k třídě QSTodoService:</span><span class="sxs-lookup"><span data-stu-id="ff46b-139">In the client project, open the file **QSTodoService.cs** and add the following using statement and `MobileServiceUser` with accessor to the QSTodoService class:</span></span>
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. <span data-ttu-id="ff46b-140">Přidat novou metodu s názvem **ověřit** k **QSTodoService** s následující definice:</span><span class="sxs-lookup"><span data-stu-id="ff46b-140">Add new method named **Authenticate** to **QSTodoService** with the following definition:</span></span>

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

    >[AZURE.NOTE] <span data-ttu-id="ff46b-141">Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu předaný **LoginAsync** výše na jednu z následujících: _MicrosoftAccount_, _Twitter_, _Google_, nebo _WindowsAzureActiveDirectory_.</span><span class="sxs-lookup"><span data-stu-id="ff46b-141">If you are using an identity provider other than a Facebook, change the value passed to **LoginAsync** above to one of the following: _MicrosoftAccount_, _Twitter_, _Google_, or _WindowsAzureActiveDirectory_.</span></span>

3. <span data-ttu-id="ff46b-142">Otevřete **QSTodoListViewController.cs**.</span><span class="sxs-lookup"><span data-stu-id="ff46b-142">Open **QSTodoListViewController.cs**.</span></span> <span data-ttu-id="ff46b-143">Upravit definici metoda **ViewDidLoad** odebrání volání **RefreshAsync()** v blízkosti end:</span><span class="sxs-lookup"><span data-stu-id="ff46b-143">Modify the method definition of **ViewDidLoad** removing the call to **RefreshAsync()** near the end:</span></span>
   
        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();
   
            todoService = QSTodoService.DefaultService;
            await todoService.InitializeStoreAsync();
   
            RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync();
            }
   
            // Comment out the call to RefreshAsync
            // await RefreshAsync();
        }
4. <span data-ttu-id="ff46b-144">Změňte metodu **RefreshAsync** k ověření, pokud **uživatele** vlastnost má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="ff46b-144">Modify the method **RefreshAsync** to authenticate if the **User** property is null.</span></span> <span data-ttu-id="ff46b-145">Přidejte následující kód v horní části definici metody:</span><span class="sxs-lookup"><span data-stu-id="ff46b-145">Add the following code at the top of the method definition:</span></span>
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. <span data-ttu-id="ff46b-146">Otevřete **AppDelegate.cs**, přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="ff46b-146">Open **AppDelegate.cs**, add the following method:</span></span>

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. <span data-ttu-id="ff46b-147">Otevřete **Info.plist** souboru, přejděte na **URL typy** v **Upřesnit** části.</span><span class="sxs-lookup"><span data-stu-id="ff46b-147">Open **Info.plist** file, navigate to **URL Types** in the **Advanced** section.</span></span> <span data-ttu-id="ff46b-148">Teď nakonfigurovat **identifikátor** a **schémata URL** typ adresy URL a klikněte na tlačítko **přidat adresu URL typu**.</span><span class="sxs-lookup"><span data-stu-id="ff46b-148">Now configure the **Identifier** and the **URL Schemes** of your URL Type and click **Add URL Type**.</span></span> <span data-ttu-id="ff46b-149">**Schémata URL** by měla být stejná jako vaše {url_scheme_of_your_app}.</span><span class="sxs-lookup"><span data-stu-id="ff46b-149">**URL Schemes** should be the same as your {url_scheme_of_your_app}.</span></span>
7. <span data-ttu-id="ff46b-150">V sadě Visual Studio nebo Xamarin Studio připojený k hostiteli sestavení Xamarin na počítači Mac, spuštění klientského projektu cílení na emulátoru nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="ff46b-150">In Visual Studio or Xamarin Studio connected to your Xamarin Build Host on your Mac, run the client project targeting a device or emulator.</span></span> <span data-ttu-id="ff46b-151">Ověřte, že aplikace zobrazí žádná data.</span><span class="sxs-lookup"><span data-stu-id="ff46b-151">Verify that the app displays no data.</span></span>
   
    <span data-ttu-id="ff46b-152">Proveďte aktualizaci gesto přidáváním dolů v seznamu položek, které způsobí, že přihlašovací obrazovce se objeví.</span><span class="sxs-lookup"><span data-stu-id="ff46b-152">Perform the refresh gesture by pulling down the list of items, which will cause the login screen to appear.</span></span> <span data-ttu-id="ff46b-153">Jakmile úspěšně jste zadali platné přihlašovací údaje, aplikace se zobrazí seznam položek todo a data můžete provádět aktualizace.</span><span class="sxs-lookup"><span data-stu-id="ff46b-153">Once you have successfully entered valid credentials, the app will display the list of todo items, and you can make updates to the data.</span></span>

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
<span data-ttu-id="ff46b-154">[vytvoření aplikace Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="ff46b-154">[Create a Xamarin.iOS app]: app-service-mobile-xamarin-ios-get-started.md</span></span>