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
# <a name="add-authentication-tooyour-xamarinios-app"></a><span data-ttu-id="c4a78-103">Přidat aplikaci Xamarin.iOS tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="c4a78-103">Add authentication tooyour Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="c4a78-104">Toto téma ukazuje, jak uživatelé tooauthenticate aplikace App Service Mobile z klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4a78-104">This topic shows you how tooauthenticate users of an App Service Mobile App from your client application.</span></span> <span data-ttu-id="c4a78-105">V tomto kurzu přidáte ověřování toohello Xamarin.iOS rychlý start projekt pomocí zprostředkovatele identity, která je podporována službou App Service.</span><span class="sxs-lookup"><span data-stu-id="c4a78-105">In this tutorial, you add authentication toohello Xamarin.iOS quickstart project using an identity provider that is supported by App Service.</span></span> <span data-ttu-id="c4a78-106">Po se úspěšně ověří a autorizuje pomocí mobilní aplikace, zobrazí se hodnota ID uživatele hello a bude moct tooaccess omezený dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="c4a78-106">After being successfully authenticated and authorized by your Mobile App, hello user ID value is displayed and you will be able tooaccess restricted table data.</span></span>

<span data-ttu-id="c4a78-107">Je třeba nejprve provést hello kurzu [vytvoření aplikace Xamarin.iOS].</span><span class="sxs-lookup"><span data-stu-id="c4a78-107">You must first complete hello tutorial [Create a Xamarin.iOS app].</span></span> <span data-ttu-id="c4a78-108">Pokud nepoužijete hello stáhli úvodní serverový projekt, je nutné přidat hello ověřování rozšíření balíčku tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="c4a78-108">If you do not use hello downloaded quick start server project, you must add hello authentication extension package tooyour project.</span></span> <span data-ttu-id="c4a78-109">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="c4a78-109">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><span data-ttu-id="c4a78-110">Registrace aplikace pro ověřování a nakonfigurujte aplikační služby</span><span class="sxs-lookup"><span data-stu-id="c4a78-110">Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-toohello-allowed-external-redirect-urls"></a><span data-ttu-id="c4a78-111">Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="c4a78-111">Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="c4a78-112">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c4a78-112">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="c4a78-113">To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="c4a78-113">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="c4a78-114">V tomto kurzu používáme schéma adresy URL hello _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="c4a78-114">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="c4a78-115">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="c4a78-115">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="c4a78-116">Mělo by být jedinečný tooyour mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="c4a78-116">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="c4a78-117">tooenable hello přesměrování na straně serveru hello:</span><span class="sxs-lookup"><span data-stu-id="c4a78-117">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="c4a78-118">V [portál Azure] hello vyberte App Service.</span><span class="sxs-lookup"><span data-stu-id="c4a78-118">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="c4a78-119">Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="c4a78-119">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="c4a78-120">V hello **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="c4a78-120">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="c4a78-121">Hello **url_scheme_of_your_app** v tento řetězec je hello schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4a78-121">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="c4a78-122">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="c4a78-122">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="c4a78-123">Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="c4a78-123">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="c4a78-124">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4a78-124">Click **OK**.</span></span>

5. <span data-ttu-id="c4a78-125">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c4a78-125">Click **Save**.</span></span>

## <a name="restrict-permissions-tooauthenticated-users"></a><span data-ttu-id="c4a78-126">Omezte oprávnění tooauthenticated</span><span class="sxs-lookup"><span data-stu-id="c4a78-126">Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="c4a78-127">&nbsp;&nbsp;4.</span><span class="sxs-lookup"><span data-stu-id="c4a78-127">&nbsp;&nbsp;4.</span></span> <span data-ttu-id="c4a78-128">V sadě Visual Studio nebo Xamarin Studio spusťte hello klientského projektu na emulátoru nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4a78-128">In Visual Studio or Xamarin Studio, run hello client project on a device or emulator.</span></span> <span data-ttu-id="c4a78-129">Ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="c4a78-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="c4a78-130">selhání Hello je zaznamenané toohello konzole ladicího programu hello.</span><span class="sxs-lookup"><span data-stu-id="c4a78-130">hello failure is logged toohello console of hello debugger.</span></span> <span data-ttu-id="c4a78-131">Proto v sadě Visual Studio, měli byste vidět hello selhání v okně výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="c4a78-131">So in Visual Studio, you should see hello failure in hello output window.</span></span>

<span data-ttu-id="c4a78-132">&nbsp;&nbsp;Toto selhání neoprávněným dojde proto, že aplikace hello pokusí tooaccess váš back-end mobilní aplikace jako neověřený uživatel.</span><span class="sxs-lookup"><span data-stu-id="c4a78-132">&nbsp;&nbsp;This unauthorized failure happens because hello app attempts tooaccess your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="c4a78-133">Hello *TodoItem* tabulka nyní vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="c4a78-133">hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="c4a78-134">Potom aktualizujte hello klienta aplikace toorequest prostředky z back-end mobilní aplikace hello s ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4a78-134">Next, you will update hello client app toorequest resources from hello Mobile App backend with an authenticated user.</span></span>

## <a name="add-authentication-toohello-app"></a><span data-ttu-id="c4a78-135">Přidat aplikaci toohello ověřování</span><span class="sxs-lookup"><span data-stu-id="c4a78-135">Add authentication toohello app</span></span>
<span data-ttu-id="c4a78-136">V této části upravíte toodisplay aplikace hello obrazovka pro přihlášení, než se zobrazí data.</span><span class="sxs-lookup"><span data-stu-id="c4a78-136">In this section, you will modify hello app toodisplay a login screen before displaying data.</span></span> <span data-ttu-id="c4a78-137">Při spuštění aplikace hello nepřipojí tooyour služby App Service a nebudou zobrazovat žádná data.</span><span class="sxs-lookup"><span data-stu-id="c4a78-137">When hello app starts, it will not not connect tooyour App Service and will not display any data.</span></span> <span data-ttu-id="c4a78-138">Po prvním hello této hello uživatel provede hello gesto aktualizace, zobrazí se obrazovka pro přihlášení hello; Po úspěšném přihlášení hello seznam položek todo se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c4a78-138">After hello first time that hello user performs hello refresh gesture, hello login screen will appear; after successful login hello list of todo items will be displayed.</span></span>

1. <span data-ttu-id="c4a78-139">V projektu klienta hello, otevřete soubor hello **QSTodoService.cs** a přidejte následující hello pomocí příkazu a `MobileServiceUser` s přistupujícího objektu toohello QSTodoService třídy:</span><span class="sxs-lookup"><span data-stu-id="c4a78-139">In hello client project, open hello file **QSTodoService.cs** and add hello following using statement and `MobileServiceUser` with accessor toohello QSTodoService class:</span></span>
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. <span data-ttu-id="c4a78-140">Přidat novou metodu s názvem **ověřit** příliš**QSTodoService** s hello následující definice:</span><span class="sxs-lookup"><span data-stu-id="c4a78-140">Add new method named **Authenticate** too**QSTodoService** with hello following definition:</span></span>

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

    >[AZURE.NOTE] <span data-ttu-id="c4a78-141">Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu hello předán příliš**LoginAsync** výše tooone následující hello: _MicrosoftAccount_, _Twitter_, _Google_, nebo _WindowsAzureActiveDirectory_.</span><span class="sxs-lookup"><span data-stu-id="c4a78-141">If you are using an identity provider other than a Facebook, change hello value passed too**LoginAsync** above tooone of hello following: _MicrosoftAccount_, _Twitter_, _Google_, or _WindowsAzureActiveDirectory_.</span></span>

3. <span data-ttu-id="c4a78-142">Otevřete **QSTodoListViewController.cs**.</span><span class="sxs-lookup"><span data-stu-id="c4a78-142">Open **QSTodoListViewController.cs**.</span></span> <span data-ttu-id="c4a78-143">Upravit definici metoda hello **ViewDidLoad** odebrání hello volání příliš**RefreshAsync()** téměř hello end:</span><span class="sxs-lookup"><span data-stu-id="c4a78-143">Modify hello method definition of **ViewDidLoad** removing hello call too**RefreshAsync()** near hello end:</span></span>
   
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
4. <span data-ttu-id="c4a78-144">Změňte metodu hello **RefreshAsync** tooauthenticate Pokud hello **uživatele** vlastnost má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="c4a78-144">Modify hello method **RefreshAsync** tooauthenticate if hello **User** property is null.</span></span> <span data-ttu-id="c4a78-145">Přidejte následující kód v horní části hello hello metoda definice hello:</span><span class="sxs-lookup"><span data-stu-id="c4a78-145">Add hello following code at hello top of hello method definition:</span></span>
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. <span data-ttu-id="c4a78-146">Otevřete **AppDelegate.cs**, přidejte následující metodu hello:</span><span class="sxs-lookup"><span data-stu-id="c4a78-146">Open **AppDelegate.cs**, add hello following method:</span></span>

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. <span data-ttu-id="c4a78-147">Otevřete **Info.plist** souboru, přejděte příliš**URL typy** v hello **Upřesnit** části.</span><span class="sxs-lookup"><span data-stu-id="c4a78-147">Open **Info.plist** file, navigate too**URL Types** in hello **Advanced** section.</span></span> <span data-ttu-id="c4a78-148">Teď nakonfigurovat hello **identifikátor** a hello **schémata URL** typ adresy URL a klikněte na tlačítko **přidat adresu URL typu**.</span><span class="sxs-lookup"><span data-stu-id="c4a78-148">Now configure hello **Identifier** and hello **URL Schemes** of your URL Type and click **Add URL Type**.</span></span> <span data-ttu-id="c4a78-149">**Schémata URL** by měla být stejná jako vaše {url_scheme_of_your_app} hello.</span><span class="sxs-lookup"><span data-stu-id="c4a78-149">**URL Schemes** should be hello same as your {url_scheme_of_your_app}.</span></span>
7. <span data-ttu-id="c4a78-150">V sadě Visual Studio nebo Xamarin Studio připojené tooyour Xamarin sestavení hostitele na počítači Mac, spusťte projekt klienta hello cílení na emulátoru nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="c4a78-150">In Visual Studio or Xamarin Studio connected tooyour Xamarin Build Host on your Mac, run hello client project targeting a device or emulator.</span></span> <span data-ttu-id="c4a78-151">Ověřte, že tuto aplikaci hello nezobrazí žádná data.</span><span class="sxs-lookup"><span data-stu-id="c4a78-151">Verify that hello app displays no data.</span></span>
   
    <span data-ttu-id="c4a78-152">Proveďte aktualizaci gesto hello přidáváním dolů hello seznam položek, které způsobí, že hello přihlašovací obrazovky tooappear.</span><span class="sxs-lookup"><span data-stu-id="c4a78-152">Perform hello refresh gesture by pulling down hello list of items, which will cause hello login screen tooappear.</span></span> <span data-ttu-id="c4a78-153">Jakmile úspěšně jste zadali platné přihlašovací údaje, hello aplikace se zobrazí hello seznam položek todo a můžete provést aktualizace toohello data.</span><span class="sxs-lookup"><span data-stu-id="c4a78-153">Once you have successfully entered valid credentials, hello app will display hello list of todo items, and you can make updates toohello data.</span></span>

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[vytvoření aplikace Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started.md