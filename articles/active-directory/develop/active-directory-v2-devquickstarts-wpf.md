---
title: "aaaAzure služby Active Directory v2.0 nativní aplikace .NET | Microsoft Docs"
description: "Jak toobuild nativní aplikace .NET, přihlášení uživatelů s Account Microsoft i osobní a pracovní nebo školní účty."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 46d81e09-bad0-44ce-9026-881805976e72
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/30/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 9418eeba02b800feee5cb00219574eb16506f0a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooa-windows-desktop-app"></a><span data-ttu-id="b087b-103">Přidat aplikaci Windows Desktop tooa přihlášení</span><span class="sxs-lookup"><span data-stu-id="b087b-103">Add sign-in tooa Windows Desktop app</span></span>
<span data-ttu-id="b087b-104">S koncovým bodem v2.0 hello hello můžete rychle přidat ověřování tooyour desktopové aplikace s podporou pro oba osobní účty Microsoft a pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="b087b-104">With hello hello v2.0 endpoint, you can quickly add authentication tooyour desktop apps with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="b087b-105">Je také umožňuje vaší aplikaci toosecurely komunikovat s back-end webové rozhraní api, a také [hello Microsoft Graph](https://graph.microsoft.io) a několik hello [Office 365 jednotné rozhraní API](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).</span><span class="sxs-lookup"><span data-stu-id="b087b-105">It also enables your app toosecurely communicate with a backend web api, as well as [hello Microsoft Graph](https://graph.microsoft.io) and a few of hello [Office 365 Unified APIs](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).</span></span>

> [!NOTE]
> <span data-ttu-id="b087b-106">Ne všechny scénáře Azure Active Directory (AD) a funkce jsou podporovány koncového bodu v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="b087b-106">Not all Azure Active Directory (AD) scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="b087b-107">toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="b087b-107">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

<span data-ttu-id="b087b-108">Pro [nativní aplikace .NET, které běží na zařízení](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD poskytuje knihovnu ověřování Identity Microsoft nebo MSAL hello.</span><span class="sxs-lookup"><span data-stu-id="b087b-108">For [.NET native apps that run on a device](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD provides hello Microsoft Identity Authentication Library, or MSAL.</span></span>  <span data-ttu-id="b087b-109">Jediným účelem je MSAL v životnosti je toomake ho snadno pro vaše aplikace tooget tokeny pro volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="b087b-109">MSAL's sole purpose in life is toomake it easy for your app tooget tokens for calling web services.</span></span>  <span data-ttu-id="b087b-110">toodemonstrate, jak lze snadno, je, zde jsme budete sestavení aplikace seznam úkolů WPF .NET který:</span><span class="sxs-lookup"><span data-stu-id="b087b-110">toodemonstrate just how easy it is, here we'll build a .NET WPF To-Do List app that:</span></span>

* <span data-ttu-id="b087b-111">Přihlásí hello uživatele v & získá přístup k tokeny pomocí hello [protokol ověřování OAuth 2.0](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="b087b-111">Signs hello user in & gets access tokens using hello [OAuth 2.0 authentication protocol](active-directory-v2-protocols.md).</span></span>
* <span data-ttu-id="b087b-112">Bezpečně volá back-end seznamu úkolů webové služby, která je také zabezpečeny OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="b087b-112">Securely calls a backend To-Do List web service, which is also secured by OAuth 2.0.</span></span>
* <span data-ttu-id="b087b-113">Přihlásí uživatele hello se.</span><span class="sxs-lookup"><span data-stu-id="b087b-113">Signs hello user out.</span></span>

## <a name="download-sample-code"></a><span data-ttu-id="b087b-114">Stáhněte si ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="b087b-114">Download sample code</span></span>
<span data-ttu-id="b087b-115">Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).</span><span class="sxs-lookup"><span data-stu-id="b087b-115">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).</span></span>  <span data-ttu-id="b087b-116">toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) nebo hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="b087b-116">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

<span data-ttu-id="b087b-117">aplikace Hello dokončit, je k dispozici na konci hello také tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b087b-117">hello completed app is provided at hello end of this tutorial as well.</span></span>

## <a name="register-an-app"></a><span data-ttu-id="b087b-118">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="b087b-118">Register an app</span></span>
<span data-ttu-id="b087b-119">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="b087b-119">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="b087b-120">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="b087b-120">Make sure to:</span></span>

* <span data-ttu-id="b087b-121">Poznamenejte hello **Id aplikace** přiřazen tooyour aplikace, budete ho potřebovat brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b087b-121">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>
* <span data-ttu-id="b087b-122">Přidat hello **Mobile** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b087b-122">Add hello **Mobile** platform for your app.</span></span>

## <a name="install--configure-msal"></a><span data-ttu-id="b087b-123">Instalace a konfigurace MSAL</span><span class="sxs-lookup"><span data-stu-id="b087b-123">Install & Configure MSAL</span></span>
<span data-ttu-id="b087b-124">Teď, když máte aplikaci zaregistrovat se společností Microsoft, můžete nainstalovat MSAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="b087b-124">Now that you have an app registered with Microsoft, you can install MSAL and write your identity-related code.</span></span>  <span data-ttu-id="b087b-125">Aby MSAL toobe možné toocommunicate hello koncového bodu v2.0, budete potřebovat tooprovide její některé informace o registraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b087b-125">In order for MSAL toobe able toocommunicate hello v2.0 endpoint, you need tooprovide it with some information about your app registration.</span></span>

* <span data-ttu-id="b087b-126">Začněte tím, že přidání MSAL toohello TodoListClient projekt pomocí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="b087b-126">Begin by adding MSAL toohello TodoListClient project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

* <span data-ttu-id="b087b-127">V projektu TodoListClient hello otevřete `app.config`.</span><span class="sxs-lookup"><span data-stu-id="b087b-127">In hello TodoListClient project, open `app.config`.</span></span>  <span data-ttu-id="b087b-128">Nahraďte hodnoty hello hello elementů v hello `<appSettings>` hello tooreflect část hodnoty, vstup do portálu pro registraci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b087b-128">Replace hello values of hello elements in hello `<appSettings>` section tooreflect hello values you input into hello app registration portal.</span></span>  <span data-ttu-id="b087b-129">Váš kód bude odkazovat tyto hodnoty vždy, když používá MSAL.</span><span class="sxs-lookup"><span data-stu-id="b087b-129">Your code will reference these values whenever it uses MSAL.</span></span>
  
  * <span data-ttu-id="b087b-130">Hello `ida:ClientId` je hello **Id aplikace** vaší aplikace, které jste zkopírovali z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="b087b-130">hello `ida:ClientId` is hello **Application Id** of your app you copied from hello portal.</span></span>
* <span data-ttu-id="b087b-131">V projektu hello seznamu úkolů služby otevřete `web.config` v kořenovém hello hello projektu.</span><span class="sxs-lookup"><span data-stu-id="b087b-131">In hello TodoList-Service project, open `web.config` in hello root of hello project.</span></span>  
  
  * <span data-ttu-id="b087b-132">Nahraďte hello `ida:Audience` hodnotu s hello stejné **Id aplikace** z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="b087b-132">Replace hello `ida:Audience` value with hello same **Application Id** from hello portal.</span></span>

## <a name="use-msal-tooget-tokens"></a><span data-ttu-id="b087b-133">Použít MSAL tooget tokeny</span><span class="sxs-lookup"><span data-stu-id="b087b-133">Use MSAL tooget tokens</span></span>
<span data-ttu-id="b087b-134">Hello základní princip za MSAL je vždy, když aplikace potřebuje přístupový token, jednoduše volat `app.AcquireToken(...)`, a MSAL hello rest.</span><span class="sxs-lookup"><span data-stu-id="b087b-134">hello basic principle behind MSAL is that whenever your app needs an access token, you simply call `app.AcquireToken(...)`, and MSAL does hello rest.</span></span>  

* <span data-ttu-id="b087b-135">V hello `TodoListClient` projekt, otevřete `MainWindow.xaml.cs` a vyhledejte hello `OnInitialized(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="b087b-135">In hello `TodoListClient` project, open `MainWindow.xaml.cs` and locate hello `OnInitialized(...)` method.</span></span>  <span data-ttu-id="b087b-136">prvním krokem Hello je tooinitialize vaší aplikace `PublicClientApplication` -MSAL na primární Třída reprezentující nativních aplikací.</span><span class="sxs-lookup"><span data-stu-id="b087b-136">hello first step is tooinitialize your app's `PublicClientApplication` - MSAL's primary class representing native applications.</span></span>  <span data-ttu-id="b087b-137">Toto je, kde je předat MSAL hello souřadnice potřebuje toocommunicate s Azure AD a určit, jak toocache tokeny.</span><span class="sxs-lookup"><span data-stu-id="b087b-137">This is where you pass MSAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

* <span data-ttu-id="b087b-138">Po spuštění aplikace hello jsme má toocheck a v tématu, pokud uživatel hello je již přihlášen do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b087b-138">When hello app starts up, we want toocheck and see if hello user is already signed into hello app.</span></span>  <span data-ttu-id="b087b-139">Však zatím nechceme tooinvoke uživatelského rozhraní přihlášení – vytočit hello uživatele, klikněte na tlačítko "Přihlásit" toodo.</span><span class="sxs-lookup"><span data-stu-id="b087b-139">However, we don't want tooinvoke a sign-in UI just yet - we'll make hello user click "Sign In" toodo so.</span></span>  <span data-ttu-id="b087b-140">Také v hello `OnInitialized(...)` metoda:</span><span class="sxs-lookup"><span data-stu-id="b087b-140">Also in hello `OnInitialized(...)` method:</span></span>

```C#
// As hello app starts, we want toocheck toosee if hello user is already signed in.
// You can do so by trying tooget a token from MSAL, using hello method
// AcquireTokenSilent.  This forces MSAL toothrow an exception if it cannot
// get a token for hello user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in hello cache - or MSAL was able tooget a new oen via refresh token.
    // Proceed toofetch hello user's tasks from hello TodoListService via hello GetTodoList() method.

    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, hello app should take no action,
        // and simply show hello user hello sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

* <span data-ttu-id="b087b-141">Pokud hello uživatel není přihlášený a klepnutím na tlačítko "Sign In" hello, jsme má tooinvoke přihlašovací údaje uživatelského rozhraní a mít zadat své přihlašovací údaje uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="b087b-141">If hello user is not signed in and they click hello "Sign In" button, we want tooinvoke a login UI and have hello user enter their credentials.</span></span>  <span data-ttu-id="b087b-142">Implementujte hello přihlašovací tlačítko rutinu:</span><span class="sxs-lookup"><span data-stu-id="b087b-142">Implement hello Sign-In button handler:</span></span>

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign hello user out if they clicked hello "Clear Cache" button

// If hello user clicked hello 'Sign-In' button, force
// MSAL tooprompt hello user for credentials by using
// AcquireTokenAsync, a method that is guaranteed tooshow a prompt toohello user.
// MSAL will get a token for hello TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If hello user canceled hello login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by hello user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

* <span data-ttu-id="b087b-143">Pokud hello uživatel úspěšně přihlásí, bude přijímat a token do mezipaměti pro vás MSAL a můžete pokračovat toocall hello `GetTodoList()` metoda s jistotou.</span><span class="sxs-lookup"><span data-stu-id="b087b-143">If hello user successfully signs-in, MSAL will receive and cache a token for you, and you can proceed toocall hello `GetTodoList()` method with confidence.</span></span>  <span data-ttu-id="b087b-144">Všechno, co opustil tooget úlohy uživatele je tooimplement hello `GetTodoList()` metoda.</span><span class="sxs-lookup"><span data-stu-id="b087b-144">All that's left tooget a user's tasks is tooimplement hello `GetTodoList()` method.</span></span>

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try tooget an access token toocall hello TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL toothrow an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show hello user a message
    // and let them click hello Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once hello token has been returned by MSAL,
// add it toohello http authorization header,
// before making hello call tooaccess hello tooDo list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When hello user is done managing their To-Do List, they may finally sign out of hello app by clicking hello "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If hello user clicked hello 'clear cache' button,
        // clear hello MSAL token cache and show hello user as signed out.
        // It's also necessary tooclear hello cookies from hello browser
        // control so hello next user has a chance toosign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a><span data-ttu-id="b087b-145">Spusťte</span><span class="sxs-lookup"><span data-stu-id="b087b-145">Run</span></span>
<span data-ttu-id="b087b-146">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="b087b-146">Congratulations!</span></span> <span data-ttu-id="b087b-147">Teď máte funkční aplikaci WPF v rozhraní .NET, která má hello možnost tooauthenticate uživatelé & bezpečně volání webového rozhraní API pomocí OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="b087b-147">You now have a working .NET WPF app that has hello ability tooauthenticate users & securely call Web APIs using OAuth 2.0.</span></span>  <span data-ttu-id="b087b-148">Spusťte oba projekty a přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="b087b-148">Run your both projects, and sign in with either a personal Microsoft account or a work or school account.</span></span>  <span data-ttu-id="b087b-149">Přidejte seznam úkolů uživatele toothat úlohy.</span><span class="sxs-lookup"><span data-stu-id="b087b-149">Add tasks toothat user's To-Do list.</span></span>  <span data-ttu-id="b087b-150">Odhlásit se a přihlaste se zpět jako jiný uživatel tooview jejich seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="b087b-150">Sign out, and sign back in as another user tooview their To-Do list.</span></span>  <span data-ttu-id="b087b-151">Zavření aplikace hello a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="b087b-151">Close hello app, and re-run it.</span></span>  <span data-ttu-id="b087b-152">Všimněte si, jak hello uživatelské relace zůstává nedotčeno – je to způsobeno hello aplikace ukládá do mezipaměti tokenů v místním souboru.</span><span class="sxs-lookup"><span data-stu-id="b087b-152">Notice how hello user's session remains intact - that is because hello app caches tokens in a local file.</span></span>

<span data-ttu-id="b087b-153">MSAL umožňuje snadno tooincorporate běžné funkce identity do vaší aplikace pomocí osobní i pracovní účty.</span><span class="sxs-lookup"><span data-stu-id="b087b-153">MSAL makes it easy tooincorporate common identity features into your app, using both personal and work accounts.</span></span>  <span data-ttu-id="b087b-154">Postará všechny pracovní dirty hello vám - Správa mezipaměti, podpora protokolu OAuth, prezentací hello uživatele s přihlašovacími údaji uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.</span><span class="sxs-lookup"><span data-stu-id="b087b-154">It takes care of all hello dirty work for you - cache management, OAuth protocol support, presenting hello user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="b087b-155">Všechny skutečně potřebujete tooknow je jednoho volání rozhraní API `app.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="b087b-155">All you really need tooknow is a single API call, `app.AcquireTokenAsync(...)`.</span></span>

<span data-ttu-id="b087b-156">Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), nebo ji můžete klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="b087b-156">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="b087b-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b087b-157">Next steps</span></span>
<span data-ttu-id="b087b-158">Nyní se můžete přesunout na pokročilejší témata.</span><span class="sxs-lookup"><span data-stu-id="b087b-158">You can now move onto more advanced topics.</span></span>  <span data-ttu-id="b087b-159">Může být vhodné tootry:</span><span class="sxs-lookup"><span data-stu-id="b087b-159">You may want tootry:</span></span>

* [<span data-ttu-id="b087b-160">Zabezpečení hello TodoListService webového rozhraní API s koncovým bodem v2.0 hello</span><span class="sxs-lookup"><span data-stu-id="b087b-160">Securing hello TodoListService Web API with hello v2.0 endpoint</span></span>](active-directory-v2-devquickstarts-dotnet-api.md)

<span data-ttu-id="b087b-161">Další zdroje projděte si:</span><span class="sxs-lookup"><span data-stu-id="b087b-161">For additional resources, check out:</span></span>  

* [<span data-ttu-id="b087b-162">Příručka vývojáře v2.0 Hello >></span><span class="sxs-lookup"><span data-stu-id="b087b-162">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="b087b-163">Značka "msal" StackOverflow >></span><span class="sxs-lookup"><span data-stu-id="b087b-163">StackOverflow "msal" tag >></span></span>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="b087b-164">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="b087b-164">Get security updates for our products</span></span>
<span data-ttu-id="b087b-165">Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="b087b-165">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

