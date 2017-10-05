---
title: "Začínáme se službou Azure AD Windows Phone | Microsoft Docs"
description: "Jak vytvořit aplikaci pro Windows Phone, který se integruje s Azure AD pro přihlašování a volá Azure AD chráněné rozhraní API pomocí metody OAuth."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 66f5ac20-5e1f-4b9d-bb99-9b3305e26416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 03c4b6d225dce99d79ef6c1ba2af43af8dea3eae
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="integrate-azure-ad-with-a-windows-phone-app"></a><span data-ttu-id="9ff6b-103">Integrace Azure AD a aplikaci pro Windows Phone</span><span class="sxs-lookup"><span data-stu-id="9ff6b-103">Integrate Azure AD with a Windows Phone App</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> <span data-ttu-id="9ff6b-104">Projekty pro Windows Phone 8.1 a starší verze nejsou podporovány v sadě Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-104">Windows Phone 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="9ff6b-105">Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="9ff6b-105">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

<span data-ttu-id="9ff6b-106">Pokud vyvíjíte aplikace Windows Phone 8.1, Azure AD je jednoduchá a přímočará pro vás k ověřování uživatelů s účty služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-106">If you're developing a Windows Phone 8.1 app, Azure AD makes it simple and straightforward for you to authenticate your users with their Active Directory accounts.</span></span>  <span data-ttu-id="9ff6b-107">Taky umožňuje vaší aplikaci bezpečně využívat žádné webové rozhraní API, které jsou chráněné službou Azure AD, jako je například rozhraní API Office 365 nebo rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-107">It also enables your application to securely consume any web API protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

> [!NOTE]
> <span data-ttu-id="9ff6b-108">Tento příklad používá ADAL v2.0.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-108">This code sample uses ADAL v2.0.</span></span>  <span data-ttu-id="9ff6b-109">Pro nejnovější technologie, můžete místo toho zkuste naše [kurz pro univerzální aplikace Windows pomocí ADAL v3.0](active-directory-devquickstarts-windowsstore.md).</span><span class="sxs-lookup"><span data-stu-id="9ff6b-109">For the latest technology, you may want to instead try our [Windows Universal Tutorial using ADAL v3.0](active-directory-devquickstarts-windowsstore.md).</span></span>  <span data-ttu-id="9ff6b-110">Pokud vytváříte skutečně aplikace pro Windows Phone 8.1, je to na správném místě.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-110">If you are indeed building an app for Windows Phone 8.1, this is the right place.</span></span>  <span data-ttu-id="9ff6b-111">ADAL v2.0 je stále plně podporovaná a je doporučený způsob vývoje agianst aplikace Windows Phone 8.1 pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-111">ADAL v2.0 is still fully supported, and is the recommended way of developing apps agianst Windows Phone 8.1 using Azure AD.</span></span>
> 
> 

<span data-ttu-id="9ff6b-112">Azure AD pro rozhraní .NET nativní klienty, kteří potřebují přístup k chráněným prostředkům, poskytuje knihovna ověřování Active Directory nebo ADAL.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-112">For .NET native clients that need to access protected resources, Azure AD provides the Active Directory Authentication Library, or ADAL.</span></span>  <span data-ttu-id="9ff6b-113">Jediným účelem je ADAL v životní je snadno pro aplikaci, kterou chcete získat přístupové tokeny.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-113">ADAL’s sole purpose in life is to make it easy for your app to get access tokens.</span></span>  <span data-ttu-id="9ff6b-114">K předvedení právě jak je snadné, zde jsme budete sestavení "Directory osobou hledající" aplikace Windows Phone 8.1 který:</span><span class="sxs-lookup"><span data-stu-id="9ff6b-114">To demonstrate just how easy it is, here we’ll build a "Directory Searcher" Windows Phone 8.1 app that:</span></span>

* <span data-ttu-id="9ff6b-115">Získá přístup k tokeny pro volání pomocí Azure AD Graph API [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ff6b-115">Gets access tokens for calling the Azure AD Graph API using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="9ff6b-116">Vyhledá adresář pro uživatele s danou UPN.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-116">Searches a directory for users with a given UPN.</span></span>
* <span data-ttu-id="9ff6b-117">Uživatelé přihlásí se.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-117">Signs users out.</span></span>

<span data-ttu-id="9ff6b-118">K vytvoření dokončení funkční aplikaci, musíte:</span><span class="sxs-lookup"><span data-stu-id="9ff6b-118">To build the complete working application, you’ll need to:</span></span>

1. <span data-ttu-id="9ff6b-119">Registrace vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-119">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="9ff6b-120">Instalace a konfigurace ADAL.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-120">Install & Configure ADAL.</span></span>
3. <span data-ttu-id="9ff6b-121">Pomocí ADAL získat tokeny z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-121">Use ADAL to get tokens from Azure AD.</span></span>

<span data-ttu-id="9ff6b-122">Abyste mohli začít, [stáhnout kostru projektu](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) nebo [stažení je hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="9ff6b-122">To get started, [download a skeleton project](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span></span>  <span data-ttu-id="9ff6b-123">Každá je řešením sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-123">Each is a Visual Studio 2013 solution.</span></span>  <span data-ttu-id="9ff6b-124">Budete také potřebovat klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-124">You'll also need an Azure AD tenant in which you can create users and register an application.</span></span>  <span data-ttu-id="9ff6b-125">Pokud ještě nemáte klienta, [zjistěte, jak získat](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="9ff6b-125">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="1-register-the-directory-searcher-application"></a><span data-ttu-id="9ff6b-126">1. Registrace aplikace osobou hledající adresáře</span><span class="sxs-lookup"><span data-stu-id="9ff6b-126">1. Register the Directory Searcher Application</span></span>
<span data-ttu-id="9ff6b-127">Chcete-li aplikaci, kterou chcete získat tokeny, budete nejprve muset zaregistrovat v klientovi služby Azure AD a udělit mu oprávnění k přístupu k Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="9ff6b-127">To enable your app to get tokens, you’ll first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API:</span></span>

1. <span data-ttu-id="9ff6b-128">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9ff6b-128">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="9ff6b-129">Na horním panelu klikněte na tlačítko na vašem účtu a v části **Directory** vyberte klienta služby Active Directory, kam chcete registrace vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-129">On the top bar, click on your account and under the **Directory** list, choose the Active Directory tenant where you wish to register your application.</span></span>
3. <span data-ttu-id="9ff6b-130">Klikněte na **více služeb** v navigaci vlevo a zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-130">Click on **More Services** in the left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="9ff6b-131">Klikněte na **registrace aplikace** a zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-131">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="9ff6b-132">Postupujte podle výzev a vytvořte novou **nativní klientská aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-132">Follow the prompts and create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="9ff6b-133">**Název** aplikace popíše aplikaci pro koncové uživatele</span><span class="sxs-lookup"><span data-stu-id="9ff6b-133">The **Name** of the application will describe your application to end-users</span></span>
  * <span data-ttu-id="9ff6b-134">**Identifikátor Uri pro přesměrování** je kombinace schéma a řetězec, který Azure AD použije k vrácení odpovědi tokenu.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-134">The **Redirect Uri** is a scheme and string combination that Azure AD will use to return token responses.</span></span>  <span data-ttu-id="9ff6b-135">Zadejte hodnotu zástupného symbolu pro nyní, například `http://DirectorySearcher`.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-135">Enter a placeholder value for now, e.g. `http://DirectorySearcher`.</span></span>  <span data-ttu-id="9ff6b-136">Tato hodnota není později budete nahradit.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-136">We'll replace this value later.</span></span>
6. <span data-ttu-id="9ff6b-137">Po dokončení registrace AAD přiřadí vaší aplikace, jedinečné ID aplikace</span><span class="sxs-lookup"><span data-stu-id="9ff6b-137">Once you’ve completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="9ff6b-138">Tuto hodnotu budete potřebovat v další části, zkopírujte jej na kartě aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-138">You’ll need this value in the next sections, so copy it from the application tab.</span></span>
7. <span data-ttu-id="9ff6b-139">Z **nastavení** vyberte **požadovaných oprávnění** a zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-139">From the **Settings** page, choose **Required Permissions** and choose **Add**.</span></span> <span data-ttu-id="9ff6b-140">Vyberte **Microsoft Graph** jako rozhraní API a přidejte **čtení dat adresáře** oprávnění v rámci **delegovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-140">Select the **Microsoft Graph** as the API and add the **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="9ff6b-141">Tato akce povolí aplikaci zpracovat dotaz rozhraní Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-141">This will enable your application to query the Graph API for users.</span></span>

## <a name="2-install--configure-adal"></a><span data-ttu-id="9ff6b-142">2. Instalace a konfigurace ADAL</span><span class="sxs-lookup"><span data-stu-id="9ff6b-142">2. Install & Configure ADAL</span></span>
<span data-ttu-id="9ff6b-143">Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-143">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="9ff6b-144">Aby ADAL být schopni komunikovat se službou Azure AD budete muset poskytnout některé informace o registraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-144">In order for ADAL to be able to communicate with Azure AD, you need to provide it with some information about your app registration.</span></span>

* <span data-ttu-id="9ff6b-145">Začněte tím, že přidávání ADAL k DirectorySearcher projektu pomocí konzoly Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-145">Begin by adding ADAL to the DirectorySearcher project using the Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* <span data-ttu-id="9ff6b-146">Otevřete v projektu DirectorySearcher `MainPage.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-146">In the DirectorySearcher project, open `MainPage.xaml.cs`.</span></span>  <span data-ttu-id="9ff6b-147">Nahraďte hodnoty v `Config Values` oblast tak, aby odrážela hodnoty vstup na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-147">Replace the values in the `Config Values` region to reflect the values you input into the Azure Portal.</span></span>  <span data-ttu-id="9ff6b-148">Váš kód bude odkazovat tyto hodnoty vždy, když ho využívá ADAL.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-148">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="9ff6b-149">`tenant` Je doména klienta služby Azure AD, např. contoso.onmicrosoft.com</span><span class="sxs-lookup"><span data-stu-id="9ff6b-149">The `tenant` is the domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com</span></span>
  * <span data-ttu-id="9ff6b-150">`clientId` Je clientId vaší aplikace, které jste zkopírovali z portálu.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-150">The `clientId` is the clientId of your application you copied from the portal.</span></span>
* <span data-ttu-id="9ff6b-151">Nyní je třeba zjistit identifikátor uri zpětného volání pro aplikace pro Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-151">You now need to discover the callback uri for your Windows Phone app.</span></span>  <span data-ttu-id="9ff6b-152">Na tomto řádku v zarážku `MainPage` metoda:</span><span class="sxs-lookup"><span data-stu-id="9ff6b-152">Set a breakpoint on this line in the `MainPage` method:</span></span>

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* <span data-ttu-id="9ff6b-153">Spusťte aplikaci a zkopírujte hodnotu z produkce `redirectUri` při průchodu zarážkou.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-153">Run the app, and copy aside the value of `redirectUri` when the breakpoint is hit.</span></span>  <span data-ttu-id="9ff6b-154">Měl by vypadat nějak podobně jako</span><span class="sxs-lookup"><span data-stu-id="9ff6b-154">It should look something like</span></span>

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* <span data-ttu-id="9ff6b-155">Zpět na **konfigurace** karta aplikace v portálu pro správu Azure, nahraďte hodnotu **RedirectUri** s touto hodnotou.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-155">Back on the **Configure** tab of your application in the Azure Management Portal, replace the value of the **RedirectUri** with this value.</span></span>  

## <a name="3-use-adal-to-get-tokens-from-aad"></a><span data-ttu-id="9ff6b-156">3. Získat tokeny z AAD pomocí ADAL</span><span class="sxs-lookup"><span data-stu-id="9ff6b-156">3. Use ADAL to Get Tokens from AAD</span></span>
<span data-ttu-id="9ff6b-157">Základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá `authContext.AcquireToken(…)`, a zbývající ADAL.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-157">The basic principle behind ADAL is that whenever your app needs an access token, it simply calls `authContext.AcquireToken(…)`, and ADAL does the rest.</span></span>  

* <span data-ttu-id="9ff6b-158">Prvním krokem je k chybě při inicializaci aplikace `AuthenticationContext` -ADAL je primární třídou.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-158">The first step is to initialize your app’s `AuthenticationContext` - ADAL’s primary class.</span></span>  <span data-ttu-id="9ff6b-159">Toto je, kde je předat ADAL souřadnice musí komunikovat s Azure AD a určit, jak pro ukládání do mezipaměti tokenů.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-159">This is where you pass ADAL the coordinates it needs to communicate with Azure AD and tell it how to cache tokens.</span></span>

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* <span data-ttu-id="9ff6b-160">Nyní najít `Search(...)` metodu, která bude vyvolán při cliks uživatele "Vyhledat" tlačítko v uživatelském rozhraní aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-160">Now locate the `Search(...)` method, which will be invoked when the user cliks the "Search" button in the app's UI.</span></span>  <span data-ttu-id="9ff6b-161">Tato metoda vytváří požadavek GET na Azure AD Graph API k dotazu pro uživatele, jehož UPN začíná zadaný hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-161">This method makes a GET request to the Azure AD Graph API to query for users whose UPN begins with the given search term.</span></span>  <span data-ttu-id="9ff6b-162">Pro dotaz na rozhraní Graph API, musíte zahrnout access_token v, ale `Authorization` hlavičky požadavku – to přichází ADAL.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-162">But in order to query the Graph API, you need to include an access_token in the `Authorization` header of the request - this is where ADAL comes in.</span></span>

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
* <span data-ttu-id="9ff6b-163">Pokud je nutné interaktivního ověřování, ADAL použije webové ověřování zprostředkovatele (soubor WAB) Windows Phone a [pokračování modelu](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) zobrazení Azure AD přihlašovací stránce.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-163">If interactive authentication is necessary, ADAL will use Windows Phone's Web Authentication Broker (WAB) and [continuation model](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) to display the Azure AD sign in page.</span></span>  <span data-ttu-id="9ff6b-164">Když se uživatel přihlásí, vaše aplikace musí předat ADAL výsledky WAB interakce.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-164">When the user signs in, your app needs to pass ADAL the results of the WAB interaction.</span></span>  <span data-ttu-id="9ff6b-165">To je jednoduché, implementace `ContinueWebAuthentication` rozhraní:</span><span class="sxs-lookup"><span data-stu-id="9ff6b-165">This is as simple as implementing the `ContinueWebAuthentication` interface:</span></span>

```C#
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

* <span data-ttu-id="9ff6b-166">Nyní je čas používat `AuthenticationResult` ADAL vrácená do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-166">Now it's time to use the `AuthenticationResult` that ADAL returned to your app.</span></span>  <span data-ttu-id="9ff6b-167">V `QueryGraph(...)` zpětné volání, připojte access_token jste získali na požadavek GET v hlavičce autorizace:</span><span class="sxs-lookup"><span data-stu-id="9ff6b-167">In the `QueryGraph(...)` callback, attach the access_token you acquired to the GET request in the Authorization header:</span></span>

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
* <span data-ttu-id="9ff6b-168">Můžete také `AuthenticationResult` objekt, který chcete zobrazit informace o uživateli ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-168">You can also use the `AuthenticationResult` object to display information about the user in your app.</span></span> <span data-ttu-id="9ff6b-169">V `QueryGraph(...)` metoda, použijte výsledek pro zobrazení id uživatele na stránce:</span><span class="sxs-lookup"><span data-stu-id="9ff6b-169">In the `QueryGraph(...)` method, use the result to show the user's id on the page:</span></span>

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* <span data-ttu-id="9ff6b-170">Nakonec se přihlásit uživatele mimo aplikace také můžete ADAL.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-170">Finally, you can use ADAL to sign the user out of hte application as well.</span></span>  <span data-ttu-id="9ff6b-171">Když uživatel klikne na tlačítko "Odhlásit", chceme, abyste ověřili, že další volání `AcquireTokenSilentAsync(...)` se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-171">When the user clicks the "Sign Out" button, we want to ensure that the next call to `AcquireTokenSilentAsync(...)` will fail.</span></span>  <span data-ttu-id="9ff6b-172">Pomocí knihovny ADAL to je stejně snadná jako vymazání mezipamětí tokenů:</span><span class="sxs-lookup"><span data-stu-id="9ff6b-172">With ADAL, this is as easy as clearing the token cache:</span></span>

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

<span data-ttu-id="9ff6b-173">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="9ff6b-173">Congratulations!</span></span> <span data-ttu-id="9ff6b-174">Teď máte funkční aplikaci pro Windows Phone, která má schopnost ověřovat uživatele, bezpečně volání webového rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-174">You now have a working Windows Phone app that has the ability to authenticate users, securely call Web APIs using OAuth 2.0, and get basic information about the user.</span></span>  <span data-ttu-id="9ff6b-175">Pokud jste to ještě neudělali, nyní je čas k naplnění vašeho klienta s některými uživateli.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-175">If you haven’t already, now is the time to populate your tenant with some users.</span></span>  <span data-ttu-id="9ff6b-176">Spuštění aplikace DirectorySearcher a přihlaste se pomocí jeden z těchto uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-176">Run your DirectorySearcher app, and sign in with one of those users.</span></span>  <span data-ttu-id="9ff6b-177">Hledání jiných uživatelů podle jejich UPN.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-177">Search for other users based on their UPN.</span></span>  <span data-ttu-id="9ff6b-178">Zavřete aplikaci a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-178">Close the app, and re-run it.</span></span>  <span data-ttu-id="9ff6b-179">Všimněte si, jak uživatelské relace zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-179">Notice how the user’s session remains intact.</span></span>  <span data-ttu-id="9ff6b-180">Odhlásit se a přihlaste se zpět jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-180">Sign out, and sign back in as another user.</span></span>

<span data-ttu-id="9ff6b-181">ADAL usnadňuje všechny tyto běžné funkce identity začlenit do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-181">ADAL makes it easy to incorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="9ff6b-182">Postará všechny dirty činností, které pro vás - Správa mezipaměti, podpora protokolu OAuth, představuje uživatele s přihlašovacími údaji uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-182">It takes care of all the dirty work for you - cache management, OAuth protocol support, presenting the user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="9ff6b-183">Všechny skutečně potřebujete vědět, je jednoho volání rozhraní API `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-183">All you really need to know is a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="9ff6b-184">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) zajišťuje [zde](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="9ff6b-184">For reference, the completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span></span>  <span data-ttu-id="9ff6b-185">Nyní se můžete přesunout další identity scénářů.</span><span class="sxs-lookup"><span data-stu-id="9ff6b-185">You can now move on to additional identity scenarios.</span></span>  <span data-ttu-id="9ff6b-186">Můžete se pokusit:</span><span class="sxs-lookup"><span data-stu-id="9ff6b-186">You may want to try:</span></span>

[<span data-ttu-id="9ff6b-187">Zabezpečení rozhraní .NET webového rozhraní API pomocí Azure AD >></span><span class="sxs-lookup"><span data-stu-id="9ff6b-187">Secure a .NET Web API with Azure AD >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

