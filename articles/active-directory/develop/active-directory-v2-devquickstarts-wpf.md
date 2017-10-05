---
title: "Nativní aplikace .NET v2.0 Azure Active Directory | Microsoft Docs"
description: "Jak vytvářet nativní aplikace .NET, která podepisuje uživatele přihlašují pomocí Account Microsoft i osobní a pracovní nebo školní účty."
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
ms.openlocfilehash: 7389f55ee6fef9548abb0ca4ac1bbd0399868d47
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-a-windows-desktop-app"></a><span data-ttu-id="1c27a-103">Přidání přihlášení do aplikace Windows Desktop</span><span class="sxs-lookup"><span data-stu-id="1c27a-103">Add sign-in to a Windows Desktop app</span></span>
<span data-ttu-id="1c27a-104">S koncový bod v2.0, můžete rychle přidat ověřování desktopových aplikací s podporou pro oba osobní účty Microsoft a pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="1c27a-104">With the the v2.0 endpoint, you can quickly add authentication to your desktop apps with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="1c27a-105">Umožňuje také aplikace k bezpečné komunikaci s back-end webové rozhraní api, a také [Microsoft Graph](https://graph.microsoft.io) a několik [Office 365 jednotné rozhraní API](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).</span><span class="sxs-lookup"><span data-stu-id="1c27a-105">It also enables your app to securely communicate with a backend web api, as well as [the Microsoft Graph](https://graph.microsoft.io) and a few of the [Office 365 Unified APIs](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).</span></span>

> [!NOTE]
> <span data-ttu-id="1c27a-106">Ne všechny scénáře Azure Active Directory (AD) a funkce jsou podporovány koncového bodu v2.0.</span><span class="sxs-lookup"><span data-stu-id="1c27a-106">Not all Azure Active Directory (AD) scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="1c27a-107">Pokud chcete zjistit, pokud byste měli používat koncový bod v2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="1c27a-107">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

<span data-ttu-id="1c27a-108">Pro [nativní aplikace .NET, které běží na zařízení](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD poskytuje knihovnu ověřování Identity Microsoft nebo MSAL.</span><span class="sxs-lookup"><span data-stu-id="1c27a-108">For [.NET native apps that run on a device](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD provides the Microsoft Identity Authentication Library, or MSAL.</span></span>  <span data-ttu-id="1c27a-109">Jediným účelem je MSAL v životnosti je snadno své aplikace a získat tokeny pro volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="1c27a-109">MSAL's sole purpose in life is to make it easy for your app to get tokens for calling web services.</span></span>  <span data-ttu-id="1c27a-110">Do ukazují, jak lze snadno je, zde jsme budete sestavení aplikace seznam úkolů WPF .NET který:</span><span class="sxs-lookup"><span data-stu-id="1c27a-110">To demonstrate just how easy it is, here we'll build a .NET WPF To-Do List app that:</span></span>

* <span data-ttu-id="1c27a-111">Přihlásí uživatele & získá přístup k tokeny pomocí [protokol ověřování OAuth 2.0](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="1c27a-111">Signs the user in & gets access tokens using the [OAuth 2.0 authentication protocol](active-directory-v2-protocols.md).</span></span>
* <span data-ttu-id="1c27a-112">Bezpečně volá back-end seznamu úkolů webové služby, která je také zabezpečeny OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="1c27a-112">Securely calls a backend To-Do List web service, which is also secured by OAuth 2.0.</span></span>
* <span data-ttu-id="1c27a-113">Odhlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="1c27a-113">Signs the user out.</span></span>

## <a name="download-sample-code"></a><span data-ttu-id="1c27a-114">Stáhněte si ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="1c27a-114">Download sample code</span></span>
<span data-ttu-id="1c27a-115">Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).</span><span class="sxs-lookup"><span data-stu-id="1c27a-115">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).</span></span>  <span data-ttu-id="1c27a-116">Chcete-li sledovat, můžete [stáhnout kostru aplikace jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) nebo tuto kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="1c27a-116">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

<span data-ttu-id="1c27a-117">Dokončená aplikace je k dispozici na konci tohoto kurzu také.</span><span class="sxs-lookup"><span data-stu-id="1c27a-117">The completed app is provided at the end of this tutorial as well.</span></span>

## <a name="register-an-app"></a><span data-ttu-id="1c27a-118">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="1c27a-118">Register an app</span></span>
<span data-ttu-id="1c27a-119">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="1c27a-119">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="1c27a-120">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="1c27a-120">Make sure to:</span></span>

* <span data-ttu-id="1c27a-121">Zkopírování **Id aplikace** přiřazené vaší aplikaci, budete ho potřebovat brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="1c27a-121">Copy down the **Application Id** assigned to your app, you'll need it soon.</span></span>
* <span data-ttu-id="1c27a-122">Přidat **Mobile** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1c27a-122">Add the **Mobile** platform for your app.</span></span>

## <a name="install--configure-msal"></a><span data-ttu-id="1c27a-123">Instalace a konfigurace MSAL</span><span class="sxs-lookup"><span data-stu-id="1c27a-123">Install & Configure MSAL</span></span>
<span data-ttu-id="1c27a-124">Teď, když máte aplikaci zaregistrovat se společností Microsoft, můžete nainstalovat MSAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="1c27a-124">Now that you have an app registered with Microsoft, you can install MSAL and write your identity-related code.</span></span>  <span data-ttu-id="1c27a-125">Aby MSAL být schopni komunikovat se koncový bod v2.0 budete muset poskytnout některé informace o registraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c27a-125">In order for MSAL to be able to communicate the v2.0 endpoint, you need to provide it with some information about your app registration.</span></span>

* <span data-ttu-id="1c27a-126">Začněte tím, že přidání MSAL projekt TodoListClient pomocí konzoly Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="1c27a-126">Begin by adding MSAL to the TodoListClient project using the Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

* <span data-ttu-id="1c27a-127">Otevřete v projektu TodoListClient `app.config`.</span><span class="sxs-lookup"><span data-stu-id="1c27a-127">In the TodoListClient project, open `app.config`.</span></span>  <span data-ttu-id="1c27a-128">Nahraďte hodnoty elementů v `<appSettings>` oddílu tak, aby odrážela hodnoty vstup na portálu pro registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c27a-128">Replace the values of the elements in the `<appSettings>` section to reflect the values you input into the app registration portal.</span></span>  <span data-ttu-id="1c27a-129">Váš kód bude odkazovat tyto hodnoty vždy, když používá MSAL.</span><span class="sxs-lookup"><span data-stu-id="1c27a-129">Your code will reference these values whenever it uses MSAL.</span></span>
  
  * <span data-ttu-id="1c27a-130">`ida:ClientId` Je **Id aplikace** vaší aplikace, které jste zkopírovali z portálu.</span><span class="sxs-lookup"><span data-stu-id="1c27a-130">The `ida:ClientId` is the **Application Id** of your app you copied from the portal.</span></span>
* <span data-ttu-id="1c27a-131">V seznamu úkolů služby projektu otevřete `web.config` v kořenovém adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="1c27a-131">In the TodoList-Service project, open `web.config` in the root of the project.</span></span>  
  
  * <span data-ttu-id="1c27a-132">Nahraďte `ida:Audience` hodnota se stejným **Id aplikace** z portálu.</span><span class="sxs-lookup"><span data-stu-id="1c27a-132">Replace the `ida:Audience` value with the same **Application Id** from the portal.</span></span>

## <a name="use-msal-to-get-tokens"></a><span data-ttu-id="1c27a-133">Získat tokeny pomocí MSAL</span><span class="sxs-lookup"><span data-stu-id="1c27a-133">Use MSAL to get tokens</span></span>
<span data-ttu-id="1c27a-134">Základní princip za MSAL je vždy, když aplikace potřebuje přístupový token, jednoduše volat `app.AcquireToken(...)`, a MSAL nedojde zbytek.</span><span class="sxs-lookup"><span data-stu-id="1c27a-134">The basic principle behind MSAL is that whenever your app needs an access token, you simply call `app.AcquireToken(...)`, and MSAL does the rest.</span></span>  

* <span data-ttu-id="1c27a-135">V `TodoListClient` projekt, otevřete `MainWindow.xaml.cs` a najděte `OnInitialized(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="1c27a-135">In the `TodoListClient` project, open `MainWindow.xaml.cs` and locate the `OnInitialized(...)` method.</span></span>  <span data-ttu-id="1c27a-136">Prvním krokem je k chybě při inicializaci aplikace `PublicClientApplication` -MSAL na primární Třída reprezentující nativních aplikací.</span><span class="sxs-lookup"><span data-stu-id="1c27a-136">The first step is to initialize your app's `PublicClientApplication` - MSAL's primary class representing native applications.</span></span>  <span data-ttu-id="1c27a-137">Toto je, kde můžete předat MSAL souřadnice, které jsou zapotřebí ke komunikaci s Azure AD a určit, jak pro ukládání do mezipaměti tokenů.</span><span class="sxs-lookup"><span data-stu-id="1c27a-137">This is where you pass MSAL the coordinates it needs to communicate with Azure AD and tell it how to cache tokens.</span></span>

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

* <span data-ttu-id="1c27a-138">Při spuštění aplikace, chceme, můžete zkontrolovat, pokud je uživatel již přihlášen k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1c27a-138">When the app starts up, we want to check and see if the user is already signed into the app.</span></span>  <span data-ttu-id="1c27a-139">Ale nechceme vyvolání uživatelského rozhraní přihlášení zatím – vytočit uživatele, klikněte na tlačítko "Přihlásit" Uděláte to tak.</span><span class="sxs-lookup"><span data-stu-id="1c27a-139">However, we don't want to invoke a sign-in UI just yet - we'll make the user click "Sign In" to do so.</span></span>  <span data-ttu-id="1c27a-140">Také v `OnInitialized(...)` metoda:</span><span class="sxs-lookup"><span data-stu-id="1c27a-140">Also in the `OnInitialized(...)` method:</span></span>

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.

    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
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

* <span data-ttu-id="1c27a-141">Pokud uživatel není přihlášený a kliknutí na tlačítko "Přihlásit", chceme vyvolání přihlášení uživatelského rozhraní a uživatelské zadat své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="1c27a-141">If the user is not signed in and they click the "Sign In" button, we want to invoke a login UI and have the user enter their credentials.</span></span>  <span data-ttu-id="1c27a-142">Implementace přihlašovací tlačítko obslužná rutina:</span><span class="sxs-lookup"><span data-stu-id="1c27a-142">Implement the Sign-In button handler:</span></span>

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

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
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
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

* <span data-ttu-id="1c27a-143">Pokud uživatel úspěšně přihlásí, bude přijímat a token do mezipaměti pro vás MSAL a můžete přejít k volání `GetTodoList()` metoda s jistotou.</span><span class="sxs-lookup"><span data-stu-id="1c27a-143">If the user successfully signs-in, MSAL will receive and cache a token for you, and you can proceed to call the `GetTodoList()` method with confidence.</span></span>  <span data-ttu-id="1c27a-144">Již zbývá k získání úloh uživatele je implementace `GetTodoList()` metoda.</span><span class="sxs-lookup"><span data-stu-id="1c27a-144">All that's left to get a user's tasks is to implement the `GetTodoList()` method.</span></span>

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

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

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

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

## <a name="run"></a><span data-ttu-id="1c27a-145">Spuštění</span><span class="sxs-lookup"><span data-stu-id="1c27a-145">Run</span></span>
<span data-ttu-id="1c27a-146">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="1c27a-146">Congratulations!</span></span> <span data-ttu-id="1c27a-147">Nyní máte pracovní aplikace WPF v rozhraní .NET, který má schopnost ověřovat uživatele & bezpečně volání webového rozhraní API pomocí OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="1c27a-147">You now have a working .NET WPF app that has the ability to authenticate users & securely call Web APIs using OAuth 2.0.</span></span>  <span data-ttu-id="1c27a-148">Spusťte oba projekty a přihlaste se pomocí osobního účtu Microsoft nebo pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="1c27a-148">Run your both projects, and sign in with either a personal Microsoft account or a work or school account.</span></span>  <span data-ttu-id="1c27a-149">Přidání úkolů do seznamu úkolů daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="1c27a-149">Add tasks to that user's To-Do list.</span></span>  <span data-ttu-id="1c27a-150">Odhlásit se a přihlaste se zpět jako jiný uživatel Chcete-li zobrazit jejich seznam úkolů.</span><span class="sxs-lookup"><span data-stu-id="1c27a-150">Sign out, and sign back in as another user to view their To-Do list.</span></span>  <span data-ttu-id="1c27a-151">Zavřete aplikaci a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="1c27a-151">Close the app, and re-run it.</span></span>  <span data-ttu-id="1c27a-152">Všimněte si, jak uživatelské relace zůstává nedotčeno – je to způsobeno aplikace ukládá do mezipaměti tokenů v místním souboru.</span><span class="sxs-lookup"><span data-stu-id="1c27a-152">Notice how the user's session remains intact - that is because the app caches tokens in a local file.</span></span>

<span data-ttu-id="1c27a-153">MSAL usnadňuje začlenit běžné funkce identity do vaší aplikace pomocí osobní i pracovní účty.</span><span class="sxs-lookup"><span data-stu-id="1c27a-153">MSAL makes it easy to incorporate common identity features into your app, using both personal and work accounts.</span></span>  <span data-ttu-id="1c27a-154">Postará všechny dirty činností, které pro vás - Správa mezipaměti, podpora protokolu OAuth, představuje uživatele s přihlašovacími údaji uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.</span><span class="sxs-lookup"><span data-stu-id="1c27a-154">It takes care of all the dirty work for you - cache management, OAuth protocol support, presenting the user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="1c27a-155">Všechny skutečně potřebujete vědět, je jednoho volání rozhraní API `app.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="1c27a-155">All you really need to know is a single API call, `app.AcquireTokenAsync(...)`.</span></span>

<span data-ttu-id="1c27a-156">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), nebo ji můžete klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="1c27a-156">For reference, the completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="1c27a-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1c27a-157">Next steps</span></span>
<span data-ttu-id="1c27a-158">Nyní se můžete přesunout na pokročilejší témata.</span><span class="sxs-lookup"><span data-stu-id="1c27a-158">You can now move onto more advanced topics.</span></span>  <span data-ttu-id="1c27a-159">Můžete se pokusit:</span><span class="sxs-lookup"><span data-stu-id="1c27a-159">You may want to try:</span></span>

* [<span data-ttu-id="1c27a-160">Zabezpečení webového rozhraní API TodoListService s koncovým bodem v2.0</span><span class="sxs-lookup"><span data-stu-id="1c27a-160">Securing the TodoListService Web API with the v2.0 endpoint</span></span>](active-directory-v2-devquickstarts-dotnet-api.md)

<span data-ttu-id="1c27a-161">Další zdroje projděte si:</span><span class="sxs-lookup"><span data-stu-id="1c27a-161">For additional resources, check out:</span></span>  

* [<span data-ttu-id="1c27a-162">Příručka vývojáře v2.0 >></span><span class="sxs-lookup"><span data-stu-id="1c27a-162">The v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="1c27a-163">Značka "msal" StackOverflow >></span><span class="sxs-lookup"><span data-stu-id="1c27a-163">StackOverflow "msal" tag >></span></span>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="1c27a-164">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="1c27a-164">Get security updates for our products</span></span>
<span data-ttu-id="1c27a-165">Doporučujeme vám získávat oznámení o bezpečnostních incidentech tak, že navštívíte [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlásíte se k odběru služby Security Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="1c27a-165">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

