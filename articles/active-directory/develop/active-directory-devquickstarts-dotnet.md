---
title: ".NET aaaAzure AD Začínáme | Microsoft Docs"
description: "Tom, jak toobuild .NET Windows Desktop aplikace, která se integruje se službou Azure AD pro přihlašování a volá Azure AD chráněný rozhraní API pomocí metody OAuth."
services: active-directory
documentationcenter: .net
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c09b358f24c7bfb371b34cf72ca48c0a45042f5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a><span data-ttu-id="2ec7c-103">Integrace Azure AD do aplikace Windows Desktop WPF</span><span class="sxs-lookup"><span data-stu-id="2ec7c-103">Integrate Azure AD into a Windows Desktop WPF App</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="2ec7c-104">Pokud vyvíjíte aplikace pracovní plochy, Azure AD je jednoduchá a přímočará pro tooauthenticate můžete uživatelům pomocí jejich účtů služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-104">If you're developing a desktop application, Azure AD makes it simple and straightforward for you tooauthenticate your users with their Active Directory accounts.</span></span>  <span data-ttu-id="2ec7c-105">Umožňuje také toosecurely vaše aplikace využívat žádné webové rozhraní API, které jsou chráněné službou Azure AD, jako například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-105">It also enables your application toosecurely consume any web API protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="2ec7c-106">Pro nativní klienty .NET, které vyžadují tooaccess chráněné zdroje Azure AD poskytuje hello knihovna ověřování Active Directory nebo ADAL.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-106">For .NET native clients that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library, or ADAL.</span></span>  <span data-ttu-id="2ec7c-107">Jediným účelem je ADAL v životnosti je toomake ho snadno pro vaše aplikace tooget přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-107">ADAL's sole purpose in life is toomake it easy for your app tooget access tokens.</span></span>  <span data-ttu-id="2ec7c-108">toodemonstrate, jak lze snadno, je, zde jsme budete sestavení aplikace seznam úkolů WPF .NET který:</span><span class="sxs-lookup"><span data-stu-id="2ec7c-108">toodemonstrate just how easy it is, here we'll build a .NET WPF To-Do List application that:</span></span>

* <span data-ttu-id="2ec7c-109">Získá přístup k tokeny pro volání rozhraní API Azure AD Graph hello pomocí hello [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="2ec7c-109">Gets access tokens for calling hello Azure AD Graph API using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="2ec7c-110">Vyhledá adresář pro uživatele s danou alias.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-110">Searches a directory for users with a given alias.</span></span>
* <span data-ttu-id="2ec7c-111">Uživatelé přihlásí se.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-111">Signs users out.</span></span>

<span data-ttu-id="2ec7c-112">toobuild hello dokončení pracovní aplikace, budete muset:</span><span class="sxs-lookup"><span data-stu-id="2ec7c-112">toobuild hello complete working application, you'll need to:</span></span>

1. <span data-ttu-id="2ec7c-113">Registrace vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="2ec7c-114">Instalace a konfigurace ADAL.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-114">Install & Configure ADAL.</span></span>
3. <span data-ttu-id="2ec7c-115">Pomocí ADAL tooget tokeny z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-115">Use ADAL tooget tokens from Azure AD.</span></span>

<span data-ttu-id="2ec7c-116">spuštění, tooget [stáhnout kostru aplikace hello](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="2ec7c-116">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="2ec7c-117">Budete také potřebovat klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-117">You'll also need an Azure AD tenant in which you can create users and register an application.</span></span>  <span data-ttu-id="2ec7c-118">Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="2ec7c-118">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="1-register-hello-directorysearcher-application"></a><span data-ttu-id="2ec7c-119">1. Zaregistrovat hello DirectorySearcher aplikace</span><span class="sxs-lookup"><span data-stu-id="2ec7c-119">1. Register hello DirectorySearcher Application</span></span>
<span data-ttu-id="2ec7c-120">tooenable tokeny tooget vaší aplikace, je nutné nejdříve tooregister ji ve službě Azure AD klienta a udělit mu oprávnění tooaccess hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="2ec7c-120">tooenable your app tooget tokens, you'll first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="2ec7c-121">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2ec7c-121">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2ec7c-122">Na horním panelu hello, klikněte na tlačítko na vašem účtu a v části hello **Directory** vyberte klienta hello služby Active Directory, kde chcete tooregister vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-122">On hello top bar, click on your account and under hello **Directory** list, choose hello Active Directory tenant where you wish tooregister your application.</span></span>
3. <span data-ttu-id="2ec7c-123">Klikněte na **více služeb** v hello navigaci vlevo a zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-123">Click on **More Services** in hello left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="2ec7c-124">Klikněte na **registrace aplikace** a zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-124">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="2ec7c-125">Postupujte podle pokynů hello a vytvořte novou **nativní klientská aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-125">Follow hello prompts and create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="2ec7c-126">Hello **název** z hello aplikace popíše aplikaci tooend uživatelé</span><span class="sxs-lookup"><span data-stu-id="2ec7c-126">hello **Name** of hello application will describe your application tooend-users</span></span>
  * <span data-ttu-id="2ec7c-127">Hello **identifikátor Uri pro přesměrování** schématu a řetězec kombinací, Azure AD použijete tooreturn odpovědi tokenu.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-127">hello **Redirect Uri** is a scheme and string combination that Azure AD will use tooreturn token responses.</span></span>  <span data-ttu-id="2ec7c-128">Zadejte například hodnotu konkrétní tooyour aplikace pomocí `http://DirectorySearcher`.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-128">Enter a value specific tooyour application, e.g. `http://DirectorySearcher`.</span></span>
6. <span data-ttu-id="2ec7c-129">Po dokončení registrace AAD přiřadí vaší aplikace, jedinečné ID aplikace</span><span class="sxs-lookup"><span data-stu-id="2ec7c-129">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="2ec7c-130">Tuto hodnotu budete potřebovat v dalších částech hello, takže zkopírujte jej ze stránky aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-130">You'll need this value in hello next sections, so copy it from hello application page.</span></span>
7. <span data-ttu-id="2ec7c-131">Z hello **nastavení** vyberte **požadovaných oprávnění** a zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-131">From hello **Settings** page, choose **Required Permissions** and choose **Add**.</span></span> <span data-ttu-id="2ec7c-132">Vyberte hello **Microsoft Graph** jako hello rozhraní API a přidejte hello **čtení dat adresáře** oprávnění v rámci **delegovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-132">Select hello **Microsoft Graph** as hello API and add hello **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="2ec7c-133">Tato akce povolí vaší aplikace tooquery hello rozhraní Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-133">This will enable your application tooquery hello Graph API for users.</span></span>

## <a name="2-install--configure-adal"></a><span data-ttu-id="2ec7c-134">2. Instalace a konfigurace ADAL</span><span class="sxs-lookup"><span data-stu-id="2ec7c-134">2. Install & Configure ADAL</span></span>
<span data-ttu-id="2ec7c-135">Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-135">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="2ec7c-136">Aby mohl mít toocommunicate ADAL toobe s Azure AD, je nutné tooprovide její některé informace o registraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-136">In order for ADAL toobe able toocommunicate with Azure AD, you need tooprovide it with some information about your app registration.</span></span>

* <span data-ttu-id="2ec7c-137">Začněte tím, že přidání ADAL toohello DirectorySearcher projekt pomocí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-137">Begin by adding ADAL toohello DirectorySearcher project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* <span data-ttu-id="2ec7c-138">V projektu DirectorySearcher hello otevřete `app.config`.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-138">In hello DirectorySearcher project, open `app.config`.</span></span>  <span data-ttu-id="2ec7c-139">Nahraďte hodnoty hello hello elementů v hello `<appSettings>` hello tooreflect část hodnoty, vstup do hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-139">Replace hello values of hello elements in hello `<appSettings>` section tooreflect hello values you input into hello Azure Portal.</span></span>  <span data-ttu-id="2ec7c-140">Váš kód bude odkazovat tyto hodnoty vždy, když ho využívá ADAL.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-140">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="2ec7c-141">Hello `ida:Tenant` je hello domény klienta služby Azure AD, např. contoso.onmicrosoft.com</span><span class="sxs-lookup"><span data-stu-id="2ec7c-141">hello `ida:Tenant` is hello domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com</span></span>
  * <span data-ttu-id="2ec7c-142">Hello `ida:ClientId` je hello clientId vaší aplikace, které jste zkopírovali z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-142">hello `ida:ClientId` is hello clientId of your application you copied from hello portal.</span></span>
  * <span data-ttu-id="2ec7c-143">Hello `ida:RedirectUri` je hello přesměrování URL adresy jste zaregistrovali hello portálu.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-143">hello `ida:RedirectUri` is hello redirect url you registered in hello portal.</span></span>

## <a name="3----use-adal-tooget-tokens-from-aad"></a><span data-ttu-id="2ec7c-144">3.    Použití ADAL tooGet tokeny od AAD</span><span class="sxs-lookup"><span data-stu-id="2ec7c-144">3.    Use ADAL tooGet Tokens from AAD</span></span>
<span data-ttu-id="2ec7c-145">Hello základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá `authContext.AcquireTokenAsync(...)`, a ADAL hello rest.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-145">hello basic principle behind ADAL is that whenever your app needs an access token, it simply calls `authContext.AcquireTokenAsync(...)`, and ADAL does hello rest.</span></span>  

* <span data-ttu-id="2ec7c-146">V hello `DirectorySearcher` projekt, otevřete `MainWindow.xaml.cs` a vyhledejte hello `MainWindow()` metoda.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-146">In hello `DirectorySearcher` project, open `MainWindow.xaml.cs` and locate hello `MainWindow()` method.</span></span>  <span data-ttu-id="2ec7c-147">prvním krokem Hello je tooinitialize vaší aplikace `AuthenticationContext` -ADAL je primární třídou.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-147">hello first step is tooinitialize your app's `AuthenticationContext` - ADAL's primary class.</span></span>  <span data-ttu-id="2ec7c-148">Toto je, kde je předat ADAL hello souřadnice potřebuje toocommunicate s Azure AD a určit, jak toocache tokeny.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-148">This is where you pass ADAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* <span data-ttu-id="2ec7c-149">Nyní najít hello `Search(...)` metodu, která bude vyvolán při hello uživatele cliks hello tlačítko "Search" v uživatelském rozhraní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-149">Now locate hello `Search(...)` method, which will be invoked when hello user cliks hello "Search" button in hello app's UI.</span></span>  <span data-ttu-id="2ec7c-150">Tato metoda vytváří tooquery toohello Azure AD Graph API požadavek GET pro uživatele, jehož UPN začíná hello zadaný hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-150">This method makes a GET request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span>  <span data-ttu-id="2ec7c-151">Ale v pořadí tooquery hello rozhraní Graph API, musíte tooinclude access_token v hello `Authorization` záhlaví hello – to přichází ADAL požadavku.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-151">But in order tooquery hello Graph API, you need tooinclude an access_token in hello `Authorization` header of hello request - this is where ADAL comes in.</span></span>

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate hello Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for hello tooDo item name");
        return;
    }

    // Get an Access Token for hello Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled hello sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* <span data-ttu-id="2ec7c-152">Když vaše aplikace vyžaduje token voláním `AcquireTokenAsync(...)`, ADAL pokusí tooreturn token bez nutnosti hello uživatelské přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-152">When your app requests a token by calling `AcquireTokenAsync(...)`, ADAL will attempt tooreturn a token without asking hello user for credentials.</span></span>  <span data-ttu-id="2ec7c-153">Pokud ADAL zjistí, že tento uživatel hello je toosign v tooget token, se zobrazí dialogové okno přihlášení, shromažďování přihlašovacích údajů uživatele hello a vrací token, po úspěšném ověření.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-153">If ADAL determines that hello user needs toosign in tooget a token, it will display a login dialog, collect hello user's credentials, and return a token upon successful authentication.</span></span>  <span data-ttu-id="2ec7c-154">Pokud ADAL nelze tooreturn token z jakéhokoli důvodu, vyvolá výjimku `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-154">If ADAL is unable tooreturn a token for any reason, it will throw an `AdalException`.</span></span>
* <span data-ttu-id="2ec7c-155">Všimněte si, že hello `AuthenticationResult` objekt obsahuje `UserInfo` objekt, který lze použít toocollect informace může být nutné vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-155">Notice that hello `AuthenticationResult` object contains a `UserInfo` object that can be used toocollect information your app may need.</span></span>  <span data-ttu-id="2ec7c-156">V hello DirectorySearcher `UserInfo` je použité toocustomize hello uživatelském rozhraní aplikace s id uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-156">In hello DirectorySearcher, `UserInfo` is used toocustomize hello app's UI with hello user's id.</span></span>
* <span data-ttu-id="2ec7c-157">Když hello uživatel klikne na tlačítko "Odhlásit" hello, chceme tooensure, který hello další volání příliš`AcquireTokenAsync(...)` požádá uživatele toosign hello v.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-157">When hello user clicks hello "Sign Out" button, we want tooensure that hello next call too`AcquireTokenAsync(...)` will ask hello user toosign in.</span></span>  <span data-ttu-id="2ec7c-158">Pomocí knihovny ADAL to je stejně snadná jako vymazání mezipamětí tokenů hello:</span><span class="sxs-lookup"><span data-stu-id="2ec7c-158">With ADAL, this is as easy as clearing hello token cache:</span></span>

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear hello token cache
    authContext.TokenCache.Clear();

    ...
}
```

* <span data-ttu-id="2ec7c-159">Ale pokud hello uživatele není klikněte na tlačítko "Odhlásit" hello, budete chtít toomaintain hello uživatelské relace pro hello při příštím spuštění hello DirectorySearcher.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-159">However, if hello user does not click hello "Sign Out" button, you will want toomaintain hello user's session for hello next time they run hello DirectorySearcher.</span></span>  <span data-ttu-id="2ec7c-160">Při spuštění aplikace hello můžete zkontrolovat mezipamětí tokenů na ADAL pro existující token a hello uživatelského rozhraní se aktualizují odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-160">When hello app launches, you can check ADAL's token cache for an existing token and update hello UI accordingly.</span></span>  <span data-ttu-id="2ec7c-161">V hello `CheckForCachedToken()` metody volání jiného příliš`AcquireTokenAsync(...)`, tentokrát předávání v hello `PromptBehavior.Never` parametr.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-161">In hello `CheckForCachedToken()` method, make another call too`AcquireTokenAsync(...)`, this time passing in hello `PromptBehavior.Never` parameter.</span></span>  <span data-ttu-id="2ec7c-162">`PromptBehavior.Never`ADAL bude informovat, že uživateli hello nesmí výzva k přihlášení a ADAL by měl místo vyvolána výjimka, pokud je nelze tooreturn token.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-162">`PromptBehavior.Never` will tell ADAL that hello user should not be prompted for sign in, and ADAL should instead throw an exception if it is unable tooreturn a token.</span></span>

```C#
public async void CheckForCachedToken() 
{
    // As hello application starts, try tooget an access token without prompting hello user.  If one exists, show hello user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed toomain page without singing hello user in.
        return;
    }

    // A valid token is in hello cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

<span data-ttu-id="2ec7c-163">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="2ec7c-163">Congratulations!</span></span> <span data-ttu-id="2ec7c-164">Teď máte funkční aplikaci WPF v rozhraní .NET, která má hello možnost tooauthenticate uživatelé, bezpečně volání webového rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-164">You now have a working .NET WPF application that has hello ability tooauthenticate users, securely call Web APIs using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="2ec7c-165">Pokud jste to ještě neudělali, nyní je čas toopopulate hello vašeho klienta s některými uživateli.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-165">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span>  <span data-ttu-id="2ec7c-166">Spuštění aplikace DirectorySearcher a přihlaste se pomocí jeden z těchto uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-166">Run your DirectorySearcher app, and sign in with one of those users.</span></span>  <span data-ttu-id="2ec7c-167">Hledání jiných uživatelů podle jejich UPN.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-167">Search for other users based on their UPN.</span></span>  <span data-ttu-id="2ec7c-168">Zavření aplikace hello a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-168">Close hello app, and re-run it.</span></span>  <span data-ttu-id="2ec7c-169">Všimněte si, jak hello uživatelské relace zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-169">Notice how hello user's session remains intact.</span></span>  <span data-ttu-id="2ec7c-170">Odhlásit se a přihlaste se zpět jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-170">Sign out, and sign back in as another user.</span></span>

<span data-ttu-id="2ec7c-171">ADAL umožňuje snadno tooincorporate všechny tyto běžné funkce identity do aplikace.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-171">ADAL makes it easy tooincorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="2ec7c-172">Postará všechny pracovní dirty hello vám - Správa mezipaměti, podpora protokolu OAuth, prezentací hello uživatele s přihlašovacími údaji uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-172">It takes care of all hello dirty work for you - cache management, OAuth protocol support, presenting hello user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="2ec7c-173">Všechny skutečně potřebujete tooknow je jednoho volání rozhraní API `authContext.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-173">All you really need tooknow is a single API call, `authContext.AcquireTokenAsync(...)`.</span></span>

<span data-ttu-id="2ec7c-174">Pro odkaz, hello dokončit ukázka (bez vašich hodnot nastavení) zajišťuje [zde](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="2ec7c-174">For reference, hello completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="2ec7c-175">Nyní se můžete přesunout na tooadditional scénáře.</span><span class="sxs-lookup"><span data-stu-id="2ec7c-175">You can now move on tooadditional scenarios.</span></span>  <span data-ttu-id="2ec7c-176">Může být vhodné tootry:</span><span class="sxs-lookup"><span data-stu-id="2ec7c-176">You may want tootry:</span></span>

[<span data-ttu-id="2ec7c-177">Zabezpečení rozhraní .NET webového rozhraní API pomocí Azure AD >></span><span class="sxs-lookup"><span data-stu-id="2ec7c-177">Secure a .NET Web API with Azure AD >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

