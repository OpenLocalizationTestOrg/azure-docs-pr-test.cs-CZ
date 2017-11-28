---
title: "aaaGet Začínáme s ověřováním pro Mobile Apps v Xamarin Android"
description: "Zjistěte, jak toouse Mobile Apps tooauthenticate uživatele vaší aplikace Xamarin Android prostřednictvím řady různých zprostředkovatelů identity, včetně AAD, Google, Facebook, Twitter a Microsoft."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: panarasi
editor: 
ms.assetid: 570fc12b-46a9-4722-b2e0-0d1c45fb2152
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: 500a4efa816e4f6d75d359e31d6357da56a72f6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-xamarinandroid-app"></a><span data-ttu-id="04fbb-103">Přidat aplikaci Xamarin.Android tooyour ověřování</span><span class="sxs-lookup"><span data-stu-id="04fbb-103">Add authentication tooyour Xamarin.Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="04fbb-104">Toto téma ukazuje, jak uživatelé tooauthenticate mobilní aplikace z klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="04fbb-104">This topic shows you how tooauthenticate users of a Mobile App from your client application.</span></span> <span data-ttu-id="04fbb-105">V tomto kurzu přidáte projekt quickstart toohello ověřování pomocí zprostředkovatele identity, který podporuje Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="04fbb-105">In this tutorial, you add authentication toohello quickstart project using an identity provider that is supported by Azure Mobile Apps.</span></span> <span data-ttu-id="04fbb-106">Po se úspěšně ověří a autorizuje v hello mobilní aplikace, zobrazí se hodnota ID uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="04fbb-106">After being successfully authenticated and authorized in hello Mobile App, hello user ID value is displayed.</span></span>

<span data-ttu-id="04fbb-107">V tomto kurzu vychází z mobilní aplikace hello rychlý start.</span><span class="sxs-lookup"><span data-stu-id="04fbb-107">This tutorial is based on hello Mobile App quickstart.</span></span> <span data-ttu-id="04fbb-108">Musíte také nejdřív dokončit kurz hello [vytvoření aplikace Xamarin.Android].</span><span class="sxs-lookup"><span data-stu-id="04fbb-108">You must also first complete hello tutorial [Create a Xamarin.Android app].</span></span> <span data-ttu-id="04fbb-109">Pokud nepoužijete hello stáhli úvodní serverový projekt, je nutné přidat hello ověřování rozšíření balíčku tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="04fbb-109">If you do not use hello downloaded quick start server project, you must add hello authentication extension package tooyour project.</span></span> <span data-ttu-id="04fbb-110">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="04fbb-110">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <span data-ttu-id="04fbb-111"><a name="register"></a>Registrace aplikace pro ověřování a nakonfigurujte aplikační služby</span><span class="sxs-lookup"><span data-stu-id="04fbb-111"><a name="register"></a>Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="04fbb-112"><a name="redirecturl"></a>Přidání adresy URL pro vaše aplikace toohello povoleno externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="04fbb-112"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="04fbb-113">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="04fbb-113">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="04fbb-114">To umožňuje hello ověřování systému tooredirect back tooyour aplikaci po dokončení procesu ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="04fbb-114">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="04fbb-115">V tomto kurzu používáme schéma adresy URL hello _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="04fbb-115">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="04fbb-116">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="04fbb-116">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="04fbb-117">Mělo by být jedinečný tooyour mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="04fbb-117">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="04fbb-118">tooenable hello přesměrování na straně serveru hello:</span><span class="sxs-lookup"><span data-stu-id="04fbb-118">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="04fbb-119">V [portál Azure] hello vyberte App Service.</span><span class="sxs-lookup"><span data-stu-id="04fbb-119">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="04fbb-120">Klikněte na tlačítko hello **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="04fbb-120">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="04fbb-121">V hello **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="04fbb-121">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="04fbb-122">Hello **url_scheme_of_your_app** v tento řetězec je hello schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="04fbb-122">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="04fbb-123">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="04fbb-123">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="04fbb-124">Měli byste si poznamenat hello řetězce, který zvolíte, bude nutné tooadjust kód mobilní aplikace s hello schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="04fbb-124">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="04fbb-125">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="04fbb-125">Click **OK**.</span></span>

5. <span data-ttu-id="04fbb-126">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="04fbb-126">Click **Save**.</span></span>

## <span data-ttu-id="04fbb-127"><a name="permissions"></a>Omezte oprávnění tooauthenticated</span><span class="sxs-lookup"><span data-stu-id="04fbb-127"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="04fbb-128">V sadě Visual Studio nebo Xamarin Studio spusťte hello klientského projektu na emulátoru nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="04fbb-128">In Visual Studio or Xamarin Studio, run hello client project on a device or emulator.</span></span> <span data-ttu-id="04fbb-129">Ověřte, že se po spuštění aplikace hello vyvolá k neošetřené výjimce s stavový kód 401 (Neautorizováno).</span><span class="sxs-lookup"><span data-stu-id="04fbb-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="04fbb-130">K tomu dochází, protože aplikace hello pokusí tooaccess váš back-end mobilní aplikace jako neověřený uživatel.</span><span class="sxs-lookup"><span data-stu-id="04fbb-130">This happens because hello app attempts tooaccess your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="04fbb-131">Hello *TodoItem* tabulka nyní vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="04fbb-131">hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="04fbb-132">Potom aktualizujte hello klienta aplikace toorequest prostředky z back-end mobilní aplikace hello s ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="04fbb-132">Next, you will update hello client app toorequest resources from hello Mobile App backend with an authenticated user.</span></span>

## <span data-ttu-id="04fbb-133"><a name="add-authentication"></a>Přidat aplikaci toohello ověřování</span><span class="sxs-lookup"><span data-stu-id="04fbb-133"><a name="add-authentication"></a>Add authentication toohello app</span></span>
<span data-ttu-id="04fbb-134">aplikace Hello je aktualizovaný toorequire uživatelé tootap hello **přihlášení** tlačítko a ověření, než se zobrazí data.</span><span class="sxs-lookup"><span data-stu-id="04fbb-134">hello app is updated toorequire users tootap hello **Sign in** button and authenticate before data is displayed.</span></span>

1. <span data-ttu-id="04fbb-135">Přidejte následující kód toohello hello **TodoActivity** třídy:</span><span class="sxs-lookup"><span data-stu-id="04fbb-135">Add hello following code toohello **TodoActivity** class:</span></span>
   
        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");
   
                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }
   
        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide hello button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load hello data.
                OnRefreshItemsSelected();
            }
        }
   
    <span data-ttu-id="04fbb-136">Tím se vytvoří nový tooauthenticate metoda uživatele a metoda obslužnou rutinu pro novou **přihlášení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="04fbb-136">This creates a new method tooauthenticate a user and a method handler for a new **Sign in** button.</span></span> <span data-ttu-id="04fbb-137">uživatel Hello v hello výše uvedený příklad kód ověřen pomocí přihlášení Facebook.</span><span class="sxs-lookup"><span data-stu-id="04fbb-137">hello user in hello example code above is authenticated by using a Facebook login.</span></span> <span data-ttu-id="04fbb-138">Zobrazí se dialogové okno je ID uživatele používané toodisplay hello jednou ověření.</span><span class="sxs-lookup"><span data-stu-id="04fbb-138">A dialog is used toodisplay hello user ID once authenticated.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="04fbb-139">Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu hello předán příliš**LoginAsync** výše tooone následující hello: *MicrosoftAccount*, *Twitter*, *Google*, nebo *WindowsAzureActiveDirectory*.</span><span class="sxs-lookup"><span data-stu-id="04fbb-139">If you are using an identity provider other than Facebook, change hello value passed too**LoginAsync** above tooone of hello following: *MicrosoftAccount*, *Twitter*, *Google*, or *WindowsAzureActiveDirectory*.</span></span>
   > 
   > 
2. <span data-ttu-id="04fbb-140">V hello **OnCreate** metoda, odstranění nebo okomentujte hello následující řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="04fbb-140">In hello **OnCreate** method, delete or comment-out hello following line of code:</span></span>
   
        OnRefreshItemsSelected ();
3. <span data-ttu-id="04fbb-141">Hello Activity_To_Do.axml souboru přidejte následující hello *LoginUser* tlačítko definice před hello existující *AddItem* tlačítko:</span><span class="sxs-lookup"><span data-stu-id="04fbb-141">In hello Activity_To_Do.axml file, add hello following *LoginUser* button definition before hello existing *AddItem* button:</span></span>
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. <span data-ttu-id="04fbb-142">Přidejte následující element toohello Strings.xml prostředky soubor hello:</span><span class="sxs-lookup"><span data-stu-id="04fbb-142">Add hello following element toohello Strings.xml resources file:</span></span>
   
        <string name="login_button_text">Sign in</string>
5. <span data-ttu-id="04fbb-143">Otevření souboru AndroidManifest.xml hello, přidejte následující kód do hello `<application>` – element XML:</span><span class="sxs-lookup"><span data-stu-id="04fbb-143">Open hello AndroidManifest.xml file, add hello following code inside `<application>` XML element:</span></span>

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. <span data-ttu-id="04fbb-144">V sadě Visual Studio nebo Xamarin Studio spusťte hello klientského projektu na emulátoru nebo zařízení a přihlaste se pomocí zprostředkovatele identity vybrané.</span><span class="sxs-lookup"><span data-stu-id="04fbb-144">In Visual Studio or Xamarin Studio, run hello client project on a device or emulator and sign in with your chosen identity provider.</span></span> <span data-ttu-id="04fbb-145">Pokud jste úspěšně přihlášeni, hello aplikace se zobrazí přihlašovací ID a hello seznam položek todo a provádět aktualizace toohello data.</span><span class="sxs-lookup"><span data-stu-id="04fbb-145">When you are successfully logged-in, hello app will display your login ID and hello list of todo items, and you can make updates toohello data.</span></span>

<!-- URLs. -->
[vytvoření aplikace Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md