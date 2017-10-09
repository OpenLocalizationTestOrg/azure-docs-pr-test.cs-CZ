---
title: "aaaAzure AD Windows Phone Začínáme | Microsoft Docs"
description: "Tom, jak toobuild aplikace Windows Phone, která se integruje se službou Azure AD pro přihlašování a volá Azure AD chráněný rozhraní API pomocí metody OAuth."
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
ms.openlocfilehash: e766bfcdfae10483772154f4b5facdec05fc846f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-a-windows-phone-app"></a><span data-ttu-id="aa107-103">Integrace Azure AD a aplikaci pro Windows Phone</span><span class="sxs-lookup"><span data-stu-id="aa107-103">Integrate Azure AD with a Windows Phone App</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> <span data-ttu-id="aa107-104">Projekty pro Windows Phone 8.1 a starší verze nejsou podporovány v sadě Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="aa107-104">Windows Phone 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="aa107-105">Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="aa107-105">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

<span data-ttu-id="aa107-106">Pokud vyvíjíte aplikace Windows Phone 8.1, Azure AD je jednoduchá a přímočará pro tooauthenticate můžete uživatelům pomocí jejich účtů služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="aa107-106">If you're developing a Windows Phone 8.1 app, Azure AD makes it simple and straightforward for you tooauthenticate your users with their Active Directory accounts.</span></span>  <span data-ttu-id="aa107-107">Umožňuje také toosecurely vaše aplikace využívat žádné webové rozhraní API, které jsou chráněné službou Azure AD, jako například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="aa107-107">It also enables your application toosecurely consume any web API protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

> [!NOTE]
> <span data-ttu-id="aa107-108">Tento příklad používá ADAL v2.0.</span><span class="sxs-lookup"><span data-stu-id="aa107-108">This code sample uses ADAL v2.0.</span></span>  <span data-ttu-id="aa107-109">Pro hello nejnovější technologie, může být vhodné tooinstead zkuste naše [kurz pro univerzální aplikace Windows pomocí ADAL v3.0](active-directory-devquickstarts-windowsstore.md).</span><span class="sxs-lookup"><span data-stu-id="aa107-109">For hello latest technology, you may want tooinstead try our [Windows Universal Tutorial using ADAL v3.0](active-directory-devquickstarts-windowsstore.md).</span></span>  <span data-ttu-id="aa107-110">Pokud vytváříte skutečně aplikace pro Windows Phone 8.1, je to hello správném místě.</span><span class="sxs-lookup"><span data-stu-id="aa107-110">If you are indeed building an app for Windows Phone 8.1, this is hello right place.</span></span>  <span data-ttu-id="aa107-111">ADAL v2.0 je stále plně podporovaná a je hello doporučeným způsobem agianst vývoj aplikace Windows Phone 8.1 pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa107-111">ADAL v2.0 is still fully supported, and is hello recommended way of developing apps agianst Windows Phone 8.1 using Azure AD.</span></span>
> 
> 

<span data-ttu-id="aa107-112">Pro nativní klienty .NET, které vyžadují tooaccess chráněné zdroje Azure AD poskytuje hello knihovna ověřování Active Directory nebo ADAL.</span><span class="sxs-lookup"><span data-stu-id="aa107-112">For .NET native clients that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library, or ADAL.</span></span>  <span data-ttu-id="aa107-113">Jediným účelem je ADAL v životnosti je toomake ho snadno pro vaše aplikace tooget přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="aa107-113">ADAL’s sole purpose in life is toomake it easy for your app tooget access tokens.</span></span>  <span data-ttu-id="aa107-114">jak lze snadno, je, toodemonstrate Zde jsme budete sestavení "Directory osobou hledající" aplikace Windows Phone 8.1:</span><span class="sxs-lookup"><span data-stu-id="aa107-114">toodemonstrate just how easy it is, here we’ll build a "Directory Searcher" Windows Phone 8.1 app that:</span></span>

* <span data-ttu-id="aa107-115">Získá přístup k tokeny pro volání rozhraní API Azure AD Graph hello pomocí hello [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="aa107-115">Gets access tokens for calling hello Azure AD Graph API using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="aa107-116">Vyhledá adresář pro uživatele s danou UPN.</span><span class="sxs-lookup"><span data-stu-id="aa107-116">Searches a directory for users with a given UPN.</span></span>
* <span data-ttu-id="aa107-117">Uživatelé přihlásí se.</span><span class="sxs-lookup"><span data-stu-id="aa107-117">Signs users out.</span></span>

<span data-ttu-id="aa107-118">toobuild hello dokončení pracovní aplikace, budete muset:</span><span class="sxs-lookup"><span data-stu-id="aa107-118">toobuild hello complete working application, you’ll need to:</span></span>

1. <span data-ttu-id="aa107-119">Registrace vaší aplikace s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa107-119">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="aa107-120">Instalace a konfigurace ADAL.</span><span class="sxs-lookup"><span data-stu-id="aa107-120">Install & Configure ADAL.</span></span>
3. <span data-ttu-id="aa107-121">Pomocí ADAL tooget tokeny z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa107-121">Use ADAL tooget tokens from Azure AD.</span></span>

<span data-ttu-id="aa107-122">spuštění, tooget [stáhnout kostru projektu](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="aa107-122">tooget started, [download a skeleton project](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span></span>  <span data-ttu-id="aa107-123">Každá je řešením sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="aa107-123">Each is a Visual Studio 2013 solution.</span></span>  <span data-ttu-id="aa107-124">Budete také potřebovat klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aa107-124">You'll also need an Azure AD tenant in which you can create users and register an application.</span></span>  <span data-ttu-id="aa107-125">Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="aa107-125">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="1-register-hello-directory-searcher-application"></a><span data-ttu-id="aa107-126">1. Zaregistrovat hello aplikace Directory osobou hledající</span><span class="sxs-lookup"><span data-stu-id="aa107-126">1. Register hello Directory Searcher Application</span></span>
<span data-ttu-id="aa107-127">tooenable tokeny tooget vaší aplikace, je nutné nejdříve tooregister ji ve službě Azure AD klienta a udělit mu oprávnění tooaccess hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="aa107-127">tooenable your app tooget tokens, you’ll first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="aa107-128">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="aa107-128">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="aa107-129">Na horním panelu hello, klikněte na tlačítko na vašem účtu a v části hello **Directory** vyberte klienta hello služby Active Directory, kde chcete tooregister vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa107-129">On hello top bar, click on your account and under hello **Directory** list, choose hello Active Directory tenant where you wish tooregister your application.</span></span>
3. <span data-ttu-id="aa107-130">Klikněte na **více služeb** v hello navigaci vlevo a zvolte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="aa107-130">Click on **More Services** in hello left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="aa107-131">Klikněte na **registrace aplikace** a zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="aa107-131">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="aa107-132">Postupujte podle pokynů hello a vytvořte novou **nativní klientská aplikace**.</span><span class="sxs-lookup"><span data-stu-id="aa107-132">Follow hello prompts and create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="aa107-133">Hello **název** z hello aplikace popíše aplikaci tooend uživatelé</span><span class="sxs-lookup"><span data-stu-id="aa107-133">hello **Name** of hello application will describe your application tooend-users</span></span>
  * <span data-ttu-id="aa107-134">Hello **identifikátor Uri pro přesměrování** schématu a řetězec kombinací, Azure AD použijete tooreturn odpovědi tokenu.</span><span class="sxs-lookup"><span data-stu-id="aa107-134">hello **Redirect Uri** is a scheme and string combination that Azure AD will use tooreturn token responses.</span></span>  <span data-ttu-id="aa107-135">Zadejte hodnotu zástupného symbolu pro nyní, například `http://DirectorySearcher`.</span><span class="sxs-lookup"><span data-stu-id="aa107-135">Enter a placeholder value for now, e.g. `http://DirectorySearcher`.</span></span>  <span data-ttu-id="aa107-136">Tato hodnota není později budete nahradit.</span><span class="sxs-lookup"><span data-stu-id="aa107-136">We'll replace this value later.</span></span>
6. <span data-ttu-id="aa107-137">Po dokončení registrace AAD přiřadí vaší aplikace, jedinečné ID aplikace</span><span class="sxs-lookup"><span data-stu-id="aa107-137">Once you’ve completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="aa107-138">Tuto hodnotu budete potřebovat v dalších částech hello, takže zkopírujte jej z karty aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="aa107-138">You’ll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="aa107-139">Z hello **nastavení** vyberte **požadovaných oprávnění** a zvolte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="aa107-139">From hello **Settings** page, choose **Required Permissions** and choose **Add**.</span></span> <span data-ttu-id="aa107-140">Vyberte hello **Microsoft Graph** jako hello rozhraní API a přidejte hello **čtení dat adresáře** oprávnění v rámci **delegovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="aa107-140">Select hello **Microsoft Graph** as hello API and add hello **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="aa107-141">Tato akce povolí vaší aplikace tooquery hello rozhraní Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="aa107-141">This will enable your application tooquery hello Graph API for users.</span></span>

## <a name="2-install--configure-adal"></a><span data-ttu-id="aa107-142">2. Instalace a konfigurace ADAL</span><span class="sxs-lookup"><span data-stu-id="aa107-142">2. Install & Configure ADAL</span></span>
<span data-ttu-id="aa107-143">Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="aa107-143">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="aa107-144">Aby mohl mít toocommunicate ADAL toobe s Azure AD, je nutné tooprovide její některé informace o registraci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa107-144">In order for ADAL toobe able toocommunicate with Azure AD, you need tooprovide it with some information about your app registration.</span></span>

* <span data-ttu-id="aa107-145">Začněte tím, že přidání ADAL toohello DirectorySearcher projekt pomocí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="aa107-145">Begin by adding ADAL toohello DirectorySearcher project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* <span data-ttu-id="aa107-146">V projektu DirectorySearcher hello otevřete `MainPage.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="aa107-146">In hello DirectorySearcher project, open `MainPage.xaml.cs`.</span></span>  <span data-ttu-id="aa107-147">Nahraďte hodnoty hello v hello `Config Values` oblast tooreflect hello hodnoty, vstup do hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aa107-147">Replace hello values in hello `Config Values` region tooreflect hello values you input into hello Azure Portal.</span></span>  <span data-ttu-id="aa107-148">Váš kód bude odkazovat tyto hodnoty vždy, když ho využívá ADAL.</span><span class="sxs-lookup"><span data-stu-id="aa107-148">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="aa107-149">Hello `tenant` je hello domény klienta služby Azure AD, např. contoso.onmicrosoft.com</span><span class="sxs-lookup"><span data-stu-id="aa107-149">hello `tenant` is hello domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com</span></span>
  * <span data-ttu-id="aa107-150">Hello `clientId` je hello clientId vaší aplikace, které jste zkopírovali z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="aa107-150">hello `clientId` is hello clientId of your application you copied from hello portal.</span></span>
* <span data-ttu-id="aa107-151">Teď musíte identifikátor uri zpětného volání hello toodiscover pro aplikace pro Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="aa107-151">You now need toodiscover hello callback uri for your Windows Phone app.</span></span>  <span data-ttu-id="aa107-152">Na tomto řádku v hello zarážku `MainPage` metoda:</span><span class="sxs-lookup"><span data-stu-id="aa107-152">Set a breakpoint on this line in hello `MainPage` method:</span></span>

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* <span data-ttu-id="aa107-153">Spuštění aplikace hello a zkopírujte z produkce hello hodnotu `redirectUri` při průchodu zarážek hello.</span><span class="sxs-lookup"><span data-stu-id="aa107-153">Run hello app, and copy aside hello value of `redirectUri` when hello breakpoint is hit.</span></span>  <span data-ttu-id="aa107-154">Měl by vypadat nějak podobně jako</span><span class="sxs-lookup"><span data-stu-id="aa107-154">It should look something like</span></span>

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* <span data-ttu-id="aa107-155">Zpět na hello **konfigurace** kartě vaší aplikace v hello portálu pro správu Azure, nahraďte hodnotu hello hello **RedirectUri** s touto hodnotou.</span><span class="sxs-lookup"><span data-stu-id="aa107-155">Back on hello **Configure** tab of your application in hello Azure Management Portal, replace hello value of hello **RedirectUri** with this value.</span></span>  

## <a name="3-use-adal-tooget-tokens-from-aad"></a><span data-ttu-id="aa107-156">3. Použití ADAL tooGet tokeny od AAD</span><span class="sxs-lookup"><span data-stu-id="aa107-156">3. Use ADAL tooGet Tokens from AAD</span></span>
<span data-ttu-id="aa107-157">Hello základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá `authContext.AcquireToken(…)`, a ADAL hello rest.</span><span class="sxs-lookup"><span data-stu-id="aa107-157">hello basic principle behind ADAL is that whenever your app needs an access token, it simply calls `authContext.AcquireToken(…)`, and ADAL does hello rest.</span></span>  

* <span data-ttu-id="aa107-158">prvním krokem Hello je tooinitialize vaší aplikace `AuthenticationContext` -ADAL je primární třídou.</span><span class="sxs-lookup"><span data-stu-id="aa107-158">hello first step is tooinitialize your app’s `AuthenticationContext` - ADAL’s primary class.</span></span>  <span data-ttu-id="aa107-159">Toto je, kde je předat ADAL hello souřadnice potřebuje toocommunicate s Azure AD a určit, jak toocache tokeny.</span><span class="sxs-lookup"><span data-stu-id="aa107-159">This is where you pass ADAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* <span data-ttu-id="aa107-160">Nyní najít hello `Search(...)` metodu, která bude vyvolán při hello uživatele cliks hello tlačítko "Search" v uživatelském rozhraní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="aa107-160">Now locate hello `Search(...)` method, which will be invoked when hello user cliks hello "Search" button in hello app's UI.</span></span>  <span data-ttu-id="aa107-161">Tato metoda vytváří tooquery toohello Azure AD Graph API požadavek GET pro uživatele, jehož UPN začíná hello zadaný hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="aa107-161">This method makes a GET request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span>  <span data-ttu-id="aa107-162">Ale v pořadí tooquery hello rozhraní Graph API, musíte tooinclude access_token v hello `Authorization` záhlaví hello – to přichází ADAL požadavku.</span><span class="sxs-lookup"><span data-stu-id="aa107-162">But in order tooquery hello Graph API, you need tooinclude an access_token in hello `Authorization` header of hello request - this is where ADAL comes in.</span></span>

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try tooget a token without triggering any user prompt.
    // ADAL will check whether hello requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained hello QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
* <span data-ttu-id="aa107-163">Pokud je nutné interaktivního ověřování, ADAL použije webové ověřování zprostředkovatele (soubor WAB) Windows Phone a [pokračování modelu](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) toodisplay hello Azure AD přihlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="aa107-163">If interactive authentication is necessary, ADAL will use Windows Phone's Web Authentication Broker (WAB) and [continuation model](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) toodisplay hello Azure AD sign in page.</span></span>  <span data-ttu-id="aa107-164">Když hello uživatel přihlásí, vaše aplikace musí toopass ADAL hello výsledky hello WAB interakce.</span><span class="sxs-lookup"><span data-stu-id="aa107-164">When hello user signs in, your app needs toopass ADAL hello results of hello WAB interaction.</span></span>  <span data-ttu-id="aa107-165">To je jednoduché, implementace hello `ContinueWebAuthentication` rozhraní:</span><span class="sxs-lookup"><span data-stu-id="aa107-165">This is as simple as implementing hello `ContinueWebAuthentication` interface:</span></span>

```C#
// This method is automatically invoked when hello application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass hello authentication interaction results tooADAL, which will
    // conclude hello token acquisition operation and invoke hello callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

* <span data-ttu-id="aa107-166">Nyní je čas toouse hello `AuthenticationResult` této ADAL vrátil tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa107-166">Now it's time toouse hello `AuthenticationResult` that ADAL returned tooyour app.</span></span>  <span data-ttu-id="aa107-167">V hello `QueryGraph(...)` zpětné volání, připojte hello access_token jste získali požadavek GET toohello v hlavičce autorizace hello:</span><span class="sxs-lookup"><span data-stu-id="aa107-167">In hello `QueryGraph(...)` callback, attach hello access_token you acquired toohello GET request in hello Authorization header:</span></span>

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If hello error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add hello access token toohello Authorization Header of hello call toohello Graph API, and call hello Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
* <span data-ttu-id="aa107-168">Můžete taky hello `AuthenticationResult` objektu toodisplay informace o hello uživateli ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aa107-168">You can also use hello `AuthenticationResult` object toodisplay information about hello user in your app.</span></span> <span data-ttu-id="aa107-169">V hello `QueryGraph(...)` metoda, použijte hello výsledek tooshow hello id uživatele na stránku hello:</span><span class="sxs-lookup"><span data-stu-id="aa107-169">In hello `QueryGraph(...)` method, use hello result tooshow hello user's id on hello page:</span></span>

```C#
// Update hello Page UI toorepresent hello signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* <span data-ttu-id="aa107-170">Nakonec můžete ADAL toosign hello uživatele mimo i aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa107-170">Finally, you can use ADAL toosign hello user out of hte application as well.</span></span>  <span data-ttu-id="aa107-171">Když hello uživatel klikne na tlačítko "Odhlásit" hello, chceme tooensure, který hello další volání příliš`AcquireTokenSilentAsync(...)` se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="aa107-171">When hello user clicks hello "Sign Out" button, we want tooensure that hello next call too`AcquireTokenSilentAsync(...)` will fail.</span></span>  <span data-ttu-id="aa107-172">Pomocí knihovny ADAL to je stejně snadná jako vymazání mezipamětí tokenů hello:</span><span class="sxs-lookup"><span data-stu-id="aa107-172">With ADAL, this is as easy as clearing hello token cache:</span></span>

```C#
private void SignOut()
{
    // Clear session state from hello token cache.
    authContext.TokenCache.Clear();

    ...
}
```

<span data-ttu-id="aa107-173">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="aa107-173">Congratulations!</span></span> <span data-ttu-id="aa107-174">Teď máte funkční aplikaci pro Windows Phone, která má hello možnost tooauthenticate uživatelé, bezpečně volání webového rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="aa107-174">You now have a working Windows Phone app that has hello ability tooauthenticate users, securely call Web APIs using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="aa107-175">Pokud jste to ještě neudělali, nyní je čas toopopulate hello vašeho klienta s některými uživateli.</span><span class="sxs-lookup"><span data-stu-id="aa107-175">If you haven’t already, now is hello time toopopulate your tenant with some users.</span></span>  <span data-ttu-id="aa107-176">Spuštění aplikace DirectorySearcher a přihlaste se pomocí jeden z těchto uživatelů.</span><span class="sxs-lookup"><span data-stu-id="aa107-176">Run your DirectorySearcher app, and sign in with one of those users.</span></span>  <span data-ttu-id="aa107-177">Hledání jiných uživatelů podle jejich UPN.</span><span class="sxs-lookup"><span data-stu-id="aa107-177">Search for other users based on their UPN.</span></span>  <span data-ttu-id="aa107-178">Zavření aplikace hello a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="aa107-178">Close hello app, and re-run it.</span></span>  <span data-ttu-id="aa107-179">Všimněte si, jak hello uživatelské relace zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="aa107-179">Notice how hello user’s session remains intact.</span></span>  <span data-ttu-id="aa107-180">Odhlásit se a přihlaste se zpět jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="aa107-180">Sign out, and sign back in as another user.</span></span>

<span data-ttu-id="aa107-181">ADAL umožňuje snadno tooincorporate všechny tyto běžné funkce identity do aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa107-181">ADAL makes it easy tooincorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="aa107-182">Postará všechny pracovní dirty hello vám - Správa mezipaměti, podpora protokolu OAuth, prezentací hello uživatele s přihlašovacími údaji uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.</span><span class="sxs-lookup"><span data-stu-id="aa107-182">It takes care of all hello dirty work for you - cache management, OAuth protocol support, presenting hello user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="aa107-183">Všechny skutečně potřebujete tooknow je jednoho volání rozhraní API `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="aa107-183">All you really need tooknow is a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="aa107-184">Pro odkaz, hello dokončit ukázka (bez vašich hodnot nastavení) zajišťuje [zde](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="aa107-184">For reference, hello completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span></span>  <span data-ttu-id="aa107-185">Nyní se můžete přesunout na tooadditional identity scénáře.</span><span class="sxs-lookup"><span data-stu-id="aa107-185">You can now move on tooadditional identity scenarios.</span></span>  <span data-ttu-id="aa107-186">Může být vhodné tootry:</span><span class="sxs-lookup"><span data-stu-id="aa107-186">You may want tootry:</span></span>

[<span data-ttu-id="aa107-187">Zabezpečení rozhraní .NET webového rozhraní API pomocí Azure AD >></span><span class="sxs-lookup"><span data-stu-id="aa107-187">Secure a .NET Web API with Azure AD >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

