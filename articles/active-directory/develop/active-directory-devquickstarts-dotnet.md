---
title: "Začínáme se službou Azure AD .NET | Microsoft Docs"
description: "Jak vytvořit aplikaci .NET Windows Desktop, který se integruje s Azure AD pro přihlašování a volá Azure AD chráněné rozhraní API pomocí metody OAuth."
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
ms.openlocfilehash: 7a252e0e5243c7b7489373845531cb913ca1f6aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a><span data-ttu-id="e51df-103">Integrace Azure AD do aplikace Windows Desktop WPF</span><span class="sxs-lookup"><span data-stu-id="e51df-103">Integrate Azure AD into a Windows Desktop WPF App</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="e51df-104">Pokud vyvíjíte aplikace pracovní plochy, Azure AD je jednoduchá a přímočará pro vás k ověřování uživatelů s účty služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e51df-104">If you're developing a desktop application, Azure AD makes it simple and straightforward for you to authenticate your users with their Active Directory accounts.</span></span>  <span data-ttu-id="e51df-105">Taky umožňuje vaší aplikaci bezpečně využívat žádné webové rozhraní API, které jsou chráněné službou Azure AD, jako je například rozhraní API Office 365 nebo rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="e51df-105">It also enables your application to securely consume any web API protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="e51df-106">Azure AD pro rozhraní .NET nativní klienty, kteří potřebují přístup k chráněným prostředkům, poskytuje knihovna ověřování Active Directory nebo ADAL.</span><span class="sxs-lookup"><span data-stu-id="e51df-106">For .NET native clients that need to access protected resources, Azure AD provides the Active Directory Authentication Library, or ADAL.</span></span>  <span data-ttu-id="e51df-107">Jediným účelem je ADAL v životní je snadno pro aplikaci, kterou chcete získat přístupové tokeny.</span><span class="sxs-lookup"><span data-stu-id="e51df-107">ADAL's sole purpose in life is to make it easy for your app to get access tokens.</span></span>  <span data-ttu-id="e51df-108">Do ukazují, jak lze snadno je, zde jsme budete sestavení aplikace seznam úkolů WPF .NET který:</span><span class="sxs-lookup"><span data-stu-id="e51df-108">To demonstrate just how easy it is, here we'll build a .NET WPF To-Do List application that:</span></span>

* <span data-ttu-id="e51df-109">Získá přístup k tokeny pro volání pomocí Azure AD Graph API [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="e51df-109">Gets access tokens for calling the Azure AD Graph API using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="e51df-110">Vyhledá adresář pro uživatele s danou alias.</span><span class="sxs-lookup"><span data-stu-id="e51df-110">Searches a directory for users with a given alias.</span></span>
* <span data-ttu-id="e51df-111">Uživatelé přihlásí se.</span><span class="sxs-lookup"><span data-stu-id="e51df-111">Signs users out.</span></span>

<span data-ttu-id="e51df-112">K vytvoření dokončení funkční aplikaci, musíte:</span><span class="sxs-lookup"><span data-stu-id="e51df-112">To build the complete working application, you'll need to:</span></span>

1. <span data-ttu-id="e51df-113">Registrace vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e51df-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="e51df-114">Instalace a konfigurace ADAL.</span><span class="sxs-lookup"><span data-stu-id="e51df-114">Install & Configure ADAL.</span></span>
3. <span data-ttu-id="e51df-115">Pomocí ADAL získat tokeny z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e51df-115">Use ADAL to get tokens from Azure AD.</span></span>

<span data-ttu-id="e51df-116">Abyste mohli začít, [stáhnout kostru aplikace](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) nebo [stažení je hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="e51df-116">To get started, [download the app skeleton](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="e51df-117">Budete také potřebovat klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e51df-117">You'll also need an Azure AD tenant in which you can create users and register an application.</span></span>  <span data-ttu-id="e51df-118">Pokud ještě nemáte klienta, [zjistěte, jak získat](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="e51df-118">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="1-register-the-directorysearcher-application"></a><span data-ttu-id="e51df-119">1. Registrace aplikace DirectorySearcher</span><span class="sxs-lookup"><span data-stu-id="e51df-119">1. Register the DirectorySearcher Application</span></span>
<span data-ttu-id="e51df-120">Chcete-li aplikaci, kterou chcete získat tokeny, budete nejprve muset zaregistrovat v klientovi služby Azure AD a udělit mu oprávnění k přístupu k Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="e51df-120">To enable your app to get tokens, you'll first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API:</span></span>

1. <span data-ttu-id="e51df-121">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e51df-121">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e51df-122">Na horním panelu klikněte na tlačítko na vašem účtu a v části **Directory** vyberte klienta služby Active Directory, kam chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e51df-122">On the top bar, click on your account and under the **Directory** list, choose the Active Directory tenant where you wish to register your application.</span></span>
3. <span data-ttu-id="e51df-123">Klikněte na **více služeb** v navigaci vlevo a zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e51df-123">Click on **More Services** in the left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="e51df-124">Klikněte na **registrace aplikace** a zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e51df-124">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="e51df-125">Postupujte podle výzev a vytvořte novou **nativní klientská aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e51df-125">Follow the prompts and create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="e51df-126">**Název** aplikace popíše aplikaci pro koncové uživatele</span><span class="sxs-lookup"><span data-stu-id="e51df-126">The **Name** of the application will describe your application to end-users</span></span>
  * <span data-ttu-id="e51df-127">**Identifikátor Uri pro přesměrování** je kombinace schéma a řetězec, který Azure AD použije k vrácení odpovědi tokenu.</span><span class="sxs-lookup"><span data-stu-id="e51df-127">The **Redirect Uri** is a scheme and string combination that Azure AD will use to return token responses.</span></span>  <span data-ttu-id="e51df-128">Zadejte hodnotu konkrétní k vaší aplikaci, například `http://DirectorySearcher`.</span><span class="sxs-lookup"><span data-stu-id="e51df-128">Enter a value specific to your application, e.g. `http://DirectorySearcher`.</span></span>
6. <span data-ttu-id="e51df-129">Po dokončení registrace AAD přiřadí vaší aplikace, jedinečné ID aplikace</span><span class="sxs-lookup"><span data-stu-id="e51df-129">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="e51df-130">Tuto hodnotu budete potřebovat v další části, zkopírujte jej ze stránky aplikace.</span><span class="sxs-lookup"><span data-stu-id="e51df-130">You'll need this value in the next sections, so copy it from the application page.</span></span>
7. <span data-ttu-id="e51df-131">Z **nastavení** vyberte **požadovaných oprávnění** a zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e51df-131">From the **Settings** page, choose **Required Permissions** and choose **Add**.</span></span> <span data-ttu-id="e51df-132">Vyberte **Microsoft Graph** jako rozhraní API a přidejte **čtení dat adresáře** oprávnění v rámci **delegovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="e51df-132">Select the **Microsoft Graph** as the API and add the **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="e51df-133">Tato akce povolí aplikaci zpracovat dotaz rozhraní Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="e51df-133">This will enable your application to query the Graph API for users.</span></span>

## <a name="2-install--configure-adal"></a><span data-ttu-id="e51df-134">2. Instalace a konfigurace ADAL</span><span class="sxs-lookup"><span data-stu-id="e51df-134">2. Install & Configure ADAL</span></span>
<span data-ttu-id="e51df-135">Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="e51df-135">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="e51df-136">Aby ADAL být schopni komunikovat se službou Azure AD budete muset poskytnout některé informace o registraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e51df-136">In order for ADAL to be able to communicate with Azure AD, you need to provide it with some information about your app registration.</span></span>

* <span data-ttu-id="e51df-137">Začněte tím, že přidávání ADAL k DirectorySearcher projektu pomocí konzoly Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="e51df-137">Begin by adding ADAL to the DirectorySearcher project using the Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* <span data-ttu-id="e51df-138">Otevřete v projektu DirectorySearcher `app.config`.</span><span class="sxs-lookup"><span data-stu-id="e51df-138">In the DirectorySearcher project, open `app.config`.</span></span>  <span data-ttu-id="e51df-139">Nahraďte hodnoty elementů v `<appSettings>` oddílu tak, aby odrážela hodnoty vstup na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e51df-139">Replace the values of the elements in the `<appSettings>` section to reflect the values you input into the Azure Portal.</span></span>  <span data-ttu-id="e51df-140">Váš kód bude odkazovat tyto hodnoty vždy, když ho využívá ADAL.</span><span class="sxs-lookup"><span data-stu-id="e51df-140">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="e51df-141">`ida:Tenant` Je doména klienta služby Azure AD, např. contoso.onmicrosoft.com</span><span class="sxs-lookup"><span data-stu-id="e51df-141">The `ida:Tenant` is the domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com</span></span>
  * <span data-ttu-id="e51df-142">`ida:ClientId` Je clientId vaší aplikace, které jste zkopírovali z portálu.</span><span class="sxs-lookup"><span data-stu-id="e51df-142">The `ida:ClientId` is the clientId of your application you copied from the portal.</span></span>
  * <span data-ttu-id="e51df-143">`ida:RedirectUri` Je přesměrování url jste zaregistrovali na portálu.</span><span class="sxs-lookup"><span data-stu-id="e51df-143">The `ida:RedirectUri` is the redirect url you registered in the portal.</span></span>

## <a name="3----use-adal-to-get-tokens-from-aad"></a><span data-ttu-id="e51df-144">3.    Získat tokeny z AAD pomocí ADAL</span><span class="sxs-lookup"><span data-stu-id="e51df-144">3.    Use ADAL to Get Tokens from AAD</span></span>
<span data-ttu-id="e51df-145">Základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá `authContext.AcquireTokenAsync(...)`, a zbývající ADAL.</span><span class="sxs-lookup"><span data-stu-id="e51df-145">The basic principle behind ADAL is that whenever your app needs an access token, it simply calls `authContext.AcquireTokenAsync(...)`, and ADAL does the rest.</span></span>  

* <span data-ttu-id="e51df-146">V `DirectorySearcher` projekt, otevřete `MainWindow.xaml.cs` a najděte `MainWindow()` metoda.</span><span class="sxs-lookup"><span data-stu-id="e51df-146">In the `DirectorySearcher` project, open `MainWindow.xaml.cs` and locate the `MainWindow()` method.</span></span>  <span data-ttu-id="e51df-147">Prvním krokem je k chybě při inicializaci aplikace `AuthenticationContext` -ADAL je primární třídou.</span><span class="sxs-lookup"><span data-stu-id="e51df-147">The first step is to initialize your app's `AuthenticationContext` - ADAL's primary class.</span></span>  <span data-ttu-id="e51df-148">Toto je, kde je předat ADAL souřadnice musí komunikovat s Azure AD a určit, jak pro ukládání do mezipaměti tokenů.</span><span class="sxs-lookup"><span data-stu-id="e51df-148">This is where you pass ADAL the coordinates it needs to communicate with Azure AD and tell it how to cache tokens.</span></span>

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* <span data-ttu-id="e51df-149">Nyní najít `Search(...)` metodu, která bude vyvolán při cliks uživatele "Vyhledat" tlačítko v uživatelském rozhraní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e51df-149">Now locate the `Search(...)` method, which will be invoked when the user cliks the "Search" button in the app's UI.</span></span>  <span data-ttu-id="e51df-150">Tato metoda vytváří požadavek GET na Azure AD Graph API k dotazu pro uživatele, jehož UPN začíná zadaný hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="e51df-150">This method makes a GET request to the Azure AD Graph API to query for users whose UPN begins with the given search term.</span></span>  <span data-ttu-id="e51df-151">Pro dotaz na rozhraní Graph API, musíte zahrnout access_token v, ale `Authorization` hlavičky požadavku – to přichází ADAL.</span><span class="sxs-lookup"><span data-stu-id="e51df-151">But in order to query the Graph API, you need to include an access_token in the `Authorization` header of the request - this is where ADAL comes in.</span></span>

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* <span data-ttu-id="e51df-152">Když vaše aplikace vyžaduje token voláním `AcquireTokenAsync(...)`, ADAL se pokusí o vrací token, aniž by požádal uživatele přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="e51df-152">When your app requests a token by calling `AcquireTokenAsync(...)`, ADAL will attempt to return a token without asking the user for credentials.</span></span>  <span data-ttu-id="e51df-153">Pokud ADAL zjistí, že uživatel musí pro přihlášení k získání tokenu, se zobrazí dialogové okno přihlášení, shromažďuje přihlašovací údaje uživatele a vrací token, po úspěšném ověření.</span><span class="sxs-lookup"><span data-stu-id="e51df-153">If ADAL determines that the user needs to sign in to get a token, it will display a login dialog, collect the user's credentials, and return a token upon successful authentication.</span></span>  <span data-ttu-id="e51df-154">Pokud se nepodařilo vrátit token z jakéhokoli důvodu ADAL, vyvolá výjimku `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="e51df-154">If ADAL is unable to return a token for any reason, it will throw an `AdalException`.</span></span>
* <span data-ttu-id="e51df-155">Všimněte si, že `AuthenticationResult` objekt obsahuje `UserInfo` objekt, který můžete použít ke shromažďování informací může být nutné vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e51df-155">Notice that the `AuthenticationResult` object contains a `UserInfo` object that can be used to collect information your app may need.</span></span>  <span data-ttu-id="e51df-156">V DirectorySearcher `UserInfo` slouží k přizpůsobení uživatelského rozhraní aplikace s id uživatele.</span><span class="sxs-lookup"><span data-stu-id="e51df-156">In the DirectorySearcher, `UserInfo` is used to customize the app's UI with the user's id.</span></span>
* <span data-ttu-id="e51df-157">Když uživatel klikne na tlačítko "Odhlásit", chceme, abyste ověřili, že další volání `AcquireTokenAsync(...)` požádá uživatele k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="e51df-157">When the user clicks the "Sign Out" button, we want to ensure that the next call to `AcquireTokenAsync(...)` will ask the user to sign in.</span></span>  <span data-ttu-id="e51df-158">Pomocí knihovny ADAL to je stejně snadná jako vymazání mezipamětí tokenů:</span><span class="sxs-lookup"><span data-stu-id="e51df-158">With ADAL, this is as easy as clearing the token cache:</span></span>

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

* <span data-ttu-id="e51df-159">Ale pokud uživatel není klikněte na tlačítko "Odhlásit", budete chtít zachovat relace uživatele pro příští, na které poběží DirectorySearcher.</span><span class="sxs-lookup"><span data-stu-id="e51df-159">However, if the user does not click the "Sign Out" button, you will want to maintain the user's session for the next time they run the DirectorySearcher.</span></span>  <span data-ttu-id="e51df-160">Při spuštění aplikace můžete zkontrolovat mezipamětí tokenů na ADAL pro existující token a uživatelského rozhraní se aktualizují odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e51df-160">When the app launches, you can check ADAL's token cache for an existing token and update the UI accordingly.</span></span>  <span data-ttu-id="e51df-161">V `CheckForCachedToken()` metoda, proveďte jiné volání `AcquireTokenAsync(...)`, tentokrát předávání v `PromptBehavior.Never` parametr.</span><span class="sxs-lookup"><span data-stu-id="e51df-161">In the `CheckForCachedToken()` method, make another call to `AcquireTokenAsync(...)`, this time passing in the `PromptBehavior.Never` parameter.</span></span>  <span data-ttu-id="e51df-162">`PromptBehavior.Never`ADAL bude informovat, že by neměl být uživatel vyzván pro přihlašování a ADAL místo toho by měla vyvolána výjimka, pokud nelze vrátit token.</span><span class="sxs-lookup"><span data-stu-id="e51df-162">`PromptBehavior.Never` will tell ADAL that the user should not be prompted for sign in, and ADAL should instead throw an exception if it is unable to return a token.</span></span>

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
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

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

<span data-ttu-id="e51df-163">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="e51df-163">Congratulations!</span></span> <span data-ttu-id="e51df-164">Teď máte funkční aplikaci WPF rozhraní .NET, který má schopnost ověřovat uživatele, bezpečně volání webového rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="e51df-164">You now have a working .NET WPF application that has the ability to authenticate users, securely call Web APIs using OAuth 2.0, and get basic information about the user.</span></span>  <span data-ttu-id="e51df-165">Pokud jste to ještě neudělali, nyní je čas k naplnění vašeho klienta s některými uživateli.</span><span class="sxs-lookup"><span data-stu-id="e51df-165">If you haven't already, now is the time to populate your tenant with some users.</span></span>  <span data-ttu-id="e51df-166">Spuštění aplikace DirectorySearcher a přihlaste se pomocí jeden z těchto uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e51df-166">Run your DirectorySearcher app, and sign in with one of those users.</span></span>  <span data-ttu-id="e51df-167">Hledání jiných uživatelů podle jejich UPN.</span><span class="sxs-lookup"><span data-stu-id="e51df-167">Search for other users based on their UPN.</span></span>  <span data-ttu-id="e51df-168">Zavřete aplikaci a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="e51df-168">Close the app, and re-run it.</span></span>  <span data-ttu-id="e51df-169">Všimněte si, jak uživatelské relace zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="e51df-169">Notice how the user's session remains intact.</span></span>  <span data-ttu-id="e51df-170">Odhlásit se a přihlaste se zpět jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="e51df-170">Sign out, and sign back in as another user.</span></span>

<span data-ttu-id="e51df-171">ADAL usnadňuje všechny tyto běžné funkce identity začlenit do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e51df-171">ADAL makes it easy to incorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="e51df-172">Postará všechny dirty činností, které pro vás - Správa mezipaměti, podpora protokolu OAuth, představuje uživatele s přihlašovacími údaji uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.</span><span class="sxs-lookup"><span data-stu-id="e51df-172">It takes care of all the dirty work for you - cache management, OAuth protocol support, presenting the user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="e51df-173">Všechny skutečně potřebujete vědět, je jednoho volání rozhraní API `authContext.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="e51df-173">All you really need to know is a single API call, `authContext.AcquireTokenAsync(...)`.</span></span>

<span data-ttu-id="e51df-174">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) zajišťuje [zde](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="e51df-174">For reference, the completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="e51df-175">Nyní se můžete přesunout dalších scénářů.</span><span class="sxs-lookup"><span data-stu-id="e51df-175">You can now move on to additional scenarios.</span></span>  <span data-ttu-id="e51df-176">Můžete se pokusit:</span><span class="sxs-lookup"><span data-stu-id="e51df-176">You may want to try:</span></span>

[<span data-ttu-id="e51df-177">Zabezpečení rozhraní .NET webového rozhraní API pomocí Azure AD >></span><span class="sxs-lookup"><span data-stu-id="e51df-177">Secure a .NET Web API with Azure AD >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

