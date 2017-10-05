---
title: "Začínáme s ověřováním pro Mobile Apps v Xamarin Android"
description: "Další informace o použití mobilní aplikace ověřovat uživatele vaší aplikace Xamarin Android prostřednictvím řady různých zprostředkovatelů identity, včetně AAD, Google, Facebook, Twitter a Microsoft."
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
ms.openlocfilehash: 8f9a1109018c708d52cdcb7b8bce43861cecd31c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-xamarinandroid-app"></a><span data-ttu-id="0b576-103">Přidání ověřování do aplikace Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="0b576-103">Add authentication to your Xamarin.Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="0b576-104">Toto téma ukazuje, jak ověřovat uživatele mobilní aplikace z klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b576-104">This topic shows you how to authenticate users of a Mobile App from your client application.</span></span> <span data-ttu-id="0b576-105">V tomto kurzu přidání ověřování do projektu pro rychlý start pomocí zprostředkovatele identity, který podporuje Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="0b576-105">In this tutorial, you add authentication to the quickstart project using an identity provider that is supported by Azure Mobile Apps.</span></span> <span data-ttu-id="0b576-106">Po se úspěšně ověří a autorizuje v mobilní aplikace, se zobrazí hodnota ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="0b576-106">After being successfully authenticated and authorized in the Mobile App, the user ID value is displayed.</span></span>

<span data-ttu-id="0b576-107">V tomto kurzu vychází z rychlého startu mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b576-107">This tutorial is based on the Mobile App quickstart.</span></span> <span data-ttu-id="0b576-108">Musíte také nejdřív dokončit tento kurz [vytvoření aplikace Xamarin.Android].</span><span class="sxs-lookup"><span data-stu-id="0b576-108">You must also first complete the tutorial [Create a Xamarin.Android app].</span></span> <span data-ttu-id="0b576-109">Pokud použijete serverový projekt stažené rychlý start, musíte přidat balíček rozšíření ověřování do projektu.</span><span class="sxs-lookup"><span data-stu-id="0b576-109">If you do not use the downloaded quick start server project, you must add the authentication extension package to your project.</span></span> <span data-ttu-id="0b576-110">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="0b576-110">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <span data-ttu-id="0b576-111"><a name="register"></a>Registrace aplikace pro ověřování a nakonfigurujte aplikační služby</span><span class="sxs-lookup"><span data-stu-id="0b576-111"><a name="register"></a>Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="0b576-112"><a name="redirecturl"></a>Přidání aplikace do adresy URL pro povolené externí přesměrování</span><span class="sxs-lookup"><span data-stu-id="0b576-112"><a name="redirecturl"></a>Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="0b576-113">Zabezpečené ověřování vyžaduje, můžete definovat nové schéma adresy URL pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0b576-113">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="0b576-114">To umožňuje ověřování systému přesměrovat zpět do aplikace po dokončení procesu ověřování.</span><span class="sxs-lookup"><span data-stu-id="0b576-114">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="0b576-115">V tomto kurzu používáme schématu adresy URL _appname_ v průběhu.</span><span class="sxs-lookup"><span data-stu-id="0b576-115">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="0b576-116">Můžete však použít žádné schéma adresy URL, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="0b576-116">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="0b576-117">Musí být jedinečné pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b576-117">It should be unique to your mobile application.</span></span> <span data-ttu-id="0b576-118">Chcete povolit přesměrování na straně serveru:</span><span class="sxs-lookup"><span data-stu-id="0b576-118">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="0b576-119">V [portál Azure] vyberte App Service.</span><span class="sxs-lookup"><span data-stu-id="0b576-119">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="0b576-120">Klikněte **ověřování / autorizace** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="0b576-120">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="0b576-121">V **povoleno externí adres URL pro přesměrování**, zadejte `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="0b576-121">In the **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="0b576-122">**Url_scheme_of_your_app** v tento řetězec je schéma adresy URL pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b576-122">The **url_scheme_of_your_app** in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="0b576-123">Měl by splňovat specifikaci normální adresu URL pro určitý protokol (používejte písmena a čísla pouze a začněte s písmenem).</span><span class="sxs-lookup"><span data-stu-id="0b576-123">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="0b576-124">Měli byste si poznamenat řetězce, který zvolíte, jako je třeba upravit kód mobilní aplikace s schéma adresy URL na několika místech.</span><span class="sxs-lookup"><span data-stu-id="0b576-124">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="0b576-125">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0b576-125">Click **OK**.</span></span>

5. <span data-ttu-id="0b576-126">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0b576-126">Click **Save**.</span></span>

## <span data-ttu-id="0b576-127"><a name="permissions"></a>Omezit oprávnění k ověření uživatelé</span><span class="sxs-lookup"><span data-stu-id="0b576-127"><a name="permissions"></a>Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="0b576-128">V sadě Visual Studio nebo Xamarin Studio spuštění klientského projektu na emulátoru nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="0b576-128">In Visual Studio or Xamarin Studio, run the client project on a device or emulator.</span></span> <span data-ttu-id="0b576-129">Ověřte, že k neošetřené výjimce s stavový kód 401 (Neautorizováno) se vyvolá po spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="0b576-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="0b576-130">K tomu dochází, protože se aplikace pokusí o přístup k váš back-end mobilní aplikace jako neověřený uživatel.</span><span class="sxs-lookup"><span data-stu-id="0b576-130">This happens because the app attempts to access your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="0b576-131">*TodoItem* tabulka nyní vyžaduje ověření.</span><span class="sxs-lookup"><span data-stu-id="0b576-131">The *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="0b576-132">Potom bude aktualizujte klientskou aplikaci pro požadavky na prostředky z back-end mobilní aplikace s ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0b576-132">Next, you will update the client app to request resources from the Mobile App backend with an authenticated user.</span></span>

## <span data-ttu-id="0b576-133"><a name="add-authentication"></a>Přidání ověřování do aplikace</span><span class="sxs-lookup"><span data-stu-id="0b576-133"><a name="add-authentication"></a>Add authentication to the app</span></span>
<span data-ttu-id="0b576-134">Aplikace se aktualizuje tak, aby vyžadovala uživatelům umožní klepnout **přihlášení** tlačítko a ověření, než se zobrazí data.</span><span class="sxs-lookup"><span data-stu-id="0b576-134">The app is updated to require users to tap the **Sign in** button and authenticate before data is displayed.</span></span>

1. <span data-ttu-id="0b576-135">Přidejte následující kód, který **TodoActivity** třídy:</span><span class="sxs-lookup"><span data-stu-id="0b576-135">Add the following code to the **TodoActivity** class:</span></span>
   
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
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load the data.
                OnRefreshItemsSelected();
            }
        }
   
    <span data-ttu-id="0b576-136">Tím se vytvoří novou metodu k ověření uživatele a metoda obslužnou rutinu pro novou **přihlášení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0b576-136">This creates a new method to authenticate a user and a method handler for a new **Sign in** button.</span></span> <span data-ttu-id="0b576-137">Ověření uživatele ve výše uvedeném příkladu kódu pomocí přihlášení Facebook.</span><span class="sxs-lookup"><span data-stu-id="0b576-137">The user in the example code above is authenticated by using a Facebook login.</span></span> <span data-ttu-id="0b576-138">Zobrazí se dialogové okno se používá k zobrazení ID uživatele po ověření.</span><span class="sxs-lookup"><span data-stu-id="0b576-138">A dialog is used to display the user ID once authenticated.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0b576-139">Pokud používáte zprostředkovatele identity než Facebook, změňte hodnotu předaný **LoginAsync** výše na jednu z následujících: *MicrosoftAccount*, *Twitter*, *Google*, nebo *WindowsAzureActiveDirectory*.</span><span class="sxs-lookup"><span data-stu-id="0b576-139">If you are using an identity provider other than Facebook, change the value passed to **LoginAsync** above to one of the following: *MicrosoftAccount*, *Twitter*, *Google*, or *WindowsAzureActiveDirectory*.</span></span>
   > 
   > 
2. <span data-ttu-id="0b576-140">V **OnCreate** metoda, odstranit nebo okomentujte následující řádek kódu:</span><span class="sxs-lookup"><span data-stu-id="0b576-140">In the **OnCreate** method, delete or comment-out the following line of code:</span></span>
   
        OnRefreshItemsSelected ();
3. <span data-ttu-id="0b576-141">V souboru Activity_To_Do.axml, přidejte následující *LoginUser* tlačítko definice před existující *AddItem* tlačítko:</span><span class="sxs-lookup"><span data-stu-id="0b576-141">In the Activity_To_Do.axml file, add the following *LoginUser* button definition before the existing *AddItem* button:</span></span>
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. <span data-ttu-id="0b576-142">Přidejte následující element do souboru prostředků Strings.xml:</span><span class="sxs-lookup"><span data-stu-id="0b576-142">Add the following element to the Strings.xml resources file:</span></span>
   
        <string name="login_button_text">Sign in</string>
5. <span data-ttu-id="0b576-143">Otevření souboru AndroidManifest.xml, přidejte následující kód do `<application>` – element XML:</span><span class="sxs-lookup"><span data-stu-id="0b576-143">Open the AndroidManifest.xml file, add the following code inside `<application>` XML element:</span></span>

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. <span data-ttu-id="0b576-144">V sadě Visual Studio nebo Xamarin Studio spuštění klientského projektu na emulátoru nebo zařízení a přihlaste se pomocí zprostředkovatele identity vybrané.</span><span class="sxs-lookup"><span data-stu-id="0b576-144">In Visual Studio or Xamarin Studio, run the client project on a device or emulator and sign in with your chosen identity provider.</span></span> <span data-ttu-id="0b576-145">Pokud jste úspěšně přihlášeni, aplikace se zobrazí přihlašovací ID a seznam položek todo a data můžete provádět aktualizace.</span><span class="sxs-lookup"><span data-stu-id="0b576-145">When you are successfully logged-in, the app will display your login ID and the list of todo items, and you can make updates to the data.</span></span>

<!-- URLs. -->
<span data-ttu-id="0b576-146">[vytvoření aplikace Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="0b576-146">[Create a Xamarin.Android app]: app-service-mobile-xamarin-android-get-started.md</span></span>