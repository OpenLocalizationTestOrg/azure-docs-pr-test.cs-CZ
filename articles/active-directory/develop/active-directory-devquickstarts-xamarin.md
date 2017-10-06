---
title: "aaaAzure AD Xamarin Začínáme | Microsoft Docs"
description: "Sestavení Xamarin aplikace, které integrují s Azure AD pro přihlášení a volání Azure AD chráněné rozhraní API pomocí metody OAuth."
services: active-directory
documentationcenter: xamarin
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 6a0d189648b7071558ac1cf2b908808668960a4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-xamarin-apps"></a><span data-ttu-id="70701-103">Integrace Azure AD s aplikacemi Xamarin</span><span class="sxs-lookup"><span data-stu-id="70701-103">Integrate Azure AD with Xamarin apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="70701-104">S Xamarinem může zapisovat mobilní aplikace v jazyce C#, můžete spustit na iOS, Android a Windows (mobilních zařízení a počítačů).</span><span class="sxs-lookup"><span data-stu-id="70701-104">With Xamarin, you can write mobile apps in C# that can run on iOS, Android, and Windows (mobile devices and PCs).</span></span> <span data-ttu-id="70701-105">Pokud vytváříte aplikace pomocí Xamarinu, Azure Active Directory (Azure AD) umožňuje jednoduché tooauthenticate uživatelů s účty služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="70701-105">If you're building an app using Xamarin, Azure Active Directory (Azure AD) makes it simple tooauthenticate users with their Azure AD accounts.</span></span> <span data-ttu-id="70701-106">aplikace Hello můžete také bezpečně využívat žádné webové rozhraní API, který je chráněný službou Azure AD, jako je například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="70701-106">hello app can also securely consume any web API that's protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="70701-107">Pro aplikace Xamarin, které vyžadují tooaccess chráněné zdroje Azure AD poskytuje hello Active Directory Authentication Library (ADAL).</span><span class="sxs-lookup"><span data-stu-id="70701-107">For Xamarin apps that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="70701-108">jediným účelem Hello adal je toomake snadno aplikace tooget přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="70701-108">hello sole purpose of ADAL is toomake it easy for apps tooget access tokens.</span></span> <span data-ttu-id="70701-109">toodemonstrate jak snadné je, tento článek ukazuje, jak aplikace DirectorySearcher toobuild který:</span><span class="sxs-lookup"><span data-stu-id="70701-109">toodemonstrate how easy it is, this article shows how toobuild DirectorySearcher apps that:</span></span>

* <span data-ttu-id="70701-110">Spusťte na iOS, Android, Windows Desktop, Windows Phone a Windows Store.</span><span class="sxs-lookup"><span data-stu-id="70701-110">Run on iOS, Android, Windows Desktop, Windows Phone, and Windows Store.</span></span>
* <span data-ttu-id="70701-111">Použití jedné třídy přenosné knihovny (PCL) tooauthenticate uživatelů a získat tokeny pro hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="70701-111">Use a single portable class library (PCL) tooauthenticate users and get tokens for hello Azure AD Graph API.</span></span>
* <span data-ttu-id="70701-112">Vyhledejte adresář pro uživatele s danou UPN.</span><span class="sxs-lookup"><span data-stu-id="70701-112">Search a directory for users with a given UPN.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="70701-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="70701-113">Before you get started</span></span>
* <span data-ttu-id="70701-114">Stáhnout hello [kostru projektu](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), nebo stáhnout hello [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="70701-114">Download hello [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), or download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="70701-115">Každého stažení je řešením sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="70701-115">Each download is a Visual Studio 2013 solution.</span></span>
* <span data-ttu-id="70701-116">Musíte taky klient služby Azure AD, ve které toocreate uživatelé a aplikace hello registrace.</span><span class="sxs-lookup"><span data-stu-id="70701-116">You also need an Azure AD tenant in which toocreate users and register hello app.</span></span> <span data-ttu-id="70701-117">Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="70701-117">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="70701-118">Až budete připravení, postupujte podle pokynů hello v hello další čtyři části.</span><span class="sxs-lookup"><span data-stu-id="70701-118">When you are ready, follow hello procedures in hello next four sections.</span></span>

## <a name="step-1-set-up-your-xamarin-development-environment"></a><span data-ttu-id="70701-119">Krok 1: Nastavení vývojového prostředí Xamarin</span><span class="sxs-lookup"><span data-stu-id="70701-119">Step 1: Set up your Xamarin development environment</span></span>
<span data-ttu-id="70701-120">Tento kurz zahrnuje projekty pro iOS, Android a Windows, a proto musíte Visual Studio a Xamarin.</span><span class="sxs-lookup"><span data-stu-id="70701-120">Because this tutorial includes projects for iOS, Android, and Windows, you need both Visual Studio and Xamarin.</span></span> <span data-ttu-id="70701-121">toocreate hello nezbytné prostředí, proces dokončení hello v [nastavit až a nainstalujte Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="70701-121">toocreate hello necessary environment, complete hello process in [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) on MSDN.</span></span> <span data-ttu-id="70701-122">Hello pokyny zahrnují materiálu, kterou můžete zkontrolovat informace o Xamarin toolearn, zatímco čekáte toobe hello instalace byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="70701-122">hello instructions include material that you can review toolearn more about Xamarin while you're waiting for hello installations toobe completed.</span></span>

<span data-ttu-id="70701-123">Po dokončení instalace hello, otevřete v sadě Visual Studio hello řešení.</span><span class="sxs-lookup"><span data-stu-id="70701-123">After you've completed hello setup, open hello solution in Visual Studio.</span></span> <span data-ttu-id="70701-124">Zde najdete šesti projekty: pět projekty specifické pro platformu a jeden PCL DirectorySearcher.cs, které budou sdílet všechny platformy.</span><span class="sxs-lookup"><span data-stu-id="70701-124">There, you will find six projects: five platform-specific projects and one PCL, DirectorySearcher.cs, which will be shared across all platforms.</span></span>

## <a name="step-2-register-hello-directorysearcher-app"></a><span data-ttu-id="70701-125">Krok 2: Registrace hello DirectorySearcher aplikace</span><span class="sxs-lookup"><span data-stu-id="70701-125">Step 2: Register hello DirectorySearcher app</span></span>
<span data-ttu-id="70701-126">tooenable hello aplikace tooget tokeny, musíte nejprve tooregister ji ve službě Azure AD klienta a udělit mu oprávnění tooaccess hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="70701-126">tooenable hello app tooget tokens, you first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API.</span></span> <span data-ttu-id="70701-127">Zde je uveden postup:</span><span class="sxs-lookup"><span data-stu-id="70701-127">Here's how:</span></span>

1. <span data-ttu-id="70701-128">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="70701-128">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="70701-129">Na horním panelu hello klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="70701-129">On hello top bar, click your account.</span></span> <span data-ttu-id="70701-130">Potom v části hello **Directory** seznamu, vyberte hello klienta služby Active Directory místo tooregister hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="70701-130">Then, under hello **Directory** list, select hello Active Directory tenant where you want tooregister hello app.</span></span>
3. <span data-ttu-id="70701-131">Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="70701-131">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="70701-132">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="70701-132">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="70701-133">toocreate nový **nativní klientská aplikace**, postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="70701-133">toocreate a new **Native Client Application**, follow hello prompts.</span></span>
  * <span data-ttu-id="70701-134">**Název** popisuje toousers aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="70701-134">**Name** describes hello app toousers.</span></span>
  * <span data-ttu-id="70701-135">**Identifikátor URI pro přesměrování** schématu a řetězec kombinací, Azure AD používá tooreturn odpovědi tokenu.</span><span class="sxs-lookup"><span data-stu-id="70701-135">**Redirect URI** is a scheme and string combination that Azure AD uses tooreturn token responses.</span></span> <span data-ttu-id="70701-136">Zadejte hodnotu (například http://DirectorySearcher).</span><span class="sxs-lookup"><span data-stu-id="70701-136">Enter a value (for example, http://DirectorySearcher).</span></span>
6. <span data-ttu-id="70701-137">Po dokončení registrace Azure AD přiřadí aplikace hello ID jedinečný aplikace.</span><span class="sxs-lookup"><span data-stu-id="70701-137">After you’ve completed registration, Azure AD assigns hello app a unique application ID.</span></span> <span data-ttu-id="70701-138">Zkopírujte hodnotu hello z hello **aplikace** kartě, protože ho budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="70701-138">Copy hello value from hello **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="70701-139">Na hello **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="70701-139">On hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="70701-140">Vyberte **Microsoft Graph** jako hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="70701-140">Select **Microsoft Graph** as hello API.</span></span> <span data-ttu-id="70701-141">V části **delegovaná oprávnění**, přidejte hello **čtení dat adresáře** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="70701-141">Under **Delegated Permissions**, add hello **Read Directory Data** permission.</span></span>  
<span data-ttu-id="70701-142">Tato akce umožní hello aplikace tooquery hello rozhraní Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="70701-142">This action enables hello app tooquery hello Graph API for users.</span></span>

## <a name="step-3-install-and-configure-adal"></a><span data-ttu-id="70701-143">Krok 3: Instalace a konfigurace ADAL</span><span class="sxs-lookup"><span data-stu-id="70701-143">Step 3: Install and configure ADAL</span></span>
<span data-ttu-id="70701-144">Nyní když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="70701-144">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="70701-145">tooenable ADAL toocommunicate s Azure AD, poskytněte některé informace o registraci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="70701-145">tooenable ADAL toocommunicate with Azure AD, give it some information about hello app registration.</span></span>

1. <span data-ttu-id="70701-146">Přidejte ADAL toohello DirectorySearcher projekt pomocí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="70701-146">Add ADAL toohello DirectorySearcher project by using hello Package Manager Console.</span></span>

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
    `

    <span data-ttu-id="70701-147">Všimněte si, že dva odkazy knihovny jsou přidány tooeach projektu: část PCL hello ADAL a část specifické pro platformu.</span><span class="sxs-lookup"><span data-stu-id="70701-147">Note that two library references are added tooeach project: hello PCL portion of ADAL and a platform-specific portion.</span></span>
2. <span data-ttu-id="70701-148">V projektu DirectorySearcherLib hello otevřete DirectorySearcher.cs.</span><span class="sxs-lookup"><span data-stu-id="70701-148">In hello DirectorySearcherLib project, open DirectorySearcher.cs.</span></span>
3. <span data-ttu-id="70701-149">Nahraďte hodnoty členů třídy hello hello hodnoty, které jste zadali v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="70701-149">Replace hello class member values with hello values that you entered in hello Azure portal.</span></span> <span data-ttu-id="70701-150">Váš kód odkazoval toothese hodnoty vždy, když ho využívá ADAL.</span><span class="sxs-lookup"><span data-stu-id="70701-150">Your code refers toothese values whenever it uses ADAL.</span></span>

  * <span data-ttu-id="70701-151">Hello *klienta* je hello domény klienta služby Azure AD (například contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="70701-151">hello *tenant* is hello domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="70701-152">Hello *clientId* je ID klienta hello hello aplikace, které jste zkopírovali z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="70701-152">hello *clientId* is hello client ID of hello app, which you copied from hello portal.</span></span>
  * <span data-ttu-id="70701-153">Hello *returnUri* je identifikátor URI, který jste zadali v portálu hello (například http://DirectorySearcher) hello přesměrování.</span><span class="sxs-lookup"><span data-stu-id="70701-153">hello *returnUri* is hello redirect URI that you entered in hello portal (for example, http://DirectorySearcher).</span></span>

## <a name="step-4-use-adal-tooget-tokens-from-azure-ad"></a><span data-ttu-id="70701-154">Krok 4: Použití ADAL tooget tokeny z Azure AD</span><span class="sxs-lookup"><span data-stu-id="70701-154">Step 4: Use ADAL tooget tokens from Azure AD</span></span>
<span data-ttu-id="70701-155">Téměř všechny logiku ověřování aplikace hello spočívá v `DirectorySearcher.SearchByAlias(...)`.</span><span class="sxs-lookup"><span data-stu-id="70701-155">Almost all of hello app's authentication logic lies in `DirectorySearcher.SearchByAlias(...)`.</span></span> <span data-ttu-id="70701-156">Všechny, které je nezbytné v projektech specifické pro platformu hello je toopass toohello kontextových parametrů `DirectorySearcher` PCL.</span><span class="sxs-lookup"><span data-stu-id="70701-156">All that's necessary in hello platform-specific projects is toopass a contextual parameter toohello `DirectorySearcher` PCL.</span></span>

1. <span data-ttu-id="70701-157">Otevřete DirectorySearcher.cs a pak přidejte nový parametr toohello `SearchByAlias(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="70701-157">Open DirectorySearcher.cs, and then add a new parameter toohello `SearchByAlias(...)` method.</span></span> <span data-ttu-id="70701-158">`IPlatformParameters`je hello kontextové parametr, který zapouzdřuje hello specifické platformy objekty, ADAL potřebám tooperform hello ověření.</span><span class="sxs-lookup"><span data-stu-id="70701-158">`IPlatformParameters` is hello contextual parameter that encapsulates hello platform-specific objects that ADAL needs tooperform hello authentication.</span></span>

    ```C#
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. <span data-ttu-id="70701-159">Inicializace `AuthenticationContext`, což je primární třídou hello adal.</span><span class="sxs-lookup"><span data-stu-id="70701-159">Initialize `AuthenticationContext`, which is hello primary class of ADAL.</span></span>  
<span data-ttu-id="70701-160">Tato akce hello ADAL předává jej koordinuje potřebám toocommunicate s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="70701-160">This action passes ADAL hello coordinates it needs toocommunicate with Azure AD.</span></span>
3. <span data-ttu-id="70701-161">Volání `AcquireTokenAsync(...)`, který přijímá hello `IPlatformParameters` objektu a vyvolá hello tok ověřování, který je nutné tooreturn tokenu toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="70701-161">Call `AcquireTokenAsync(...)`, which accepts hello `IPlatformParameters` object and invokes hello authentication flow that's necessary tooreturn a token toohello app.</span></span>

    ```C#
    ...
        AuthenticationResult authResult = null;
        try
        {
            AuthenticationContext authContext = new AuthenticationContext(authority);
            authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
        }
        catch (Exception ee)
        {
            results.Add(new User { error = ee.Message });
            return results;
        }
    ...
    ```

    <span data-ttu-id="70701-162">`AcquireTokenAsync(...)`první tooreturn pokusy o token pro hello požadovaný prostředek (hello rozhraní Graph API v tomto případě) bez vyzvání uživatele tooenter přihlašovacích údajů (prostřednictvím ukládání do mezipaměti nebo obnovení původního tokeny).</span><span class="sxs-lookup"><span data-stu-id="70701-162">`AcquireTokenAsync(...)` first attempts tooreturn a token for hello requested resource (hello Graph API in this case) without prompting users tooenter their credentials (via caching or refreshing old tokens).</span></span> <span data-ttu-id="70701-163">Podle potřeby zobrazuje hello Azure AD přihlašovací stránka uživatelé před získání tokenu požadovaný hello.</span><span class="sxs-lookup"><span data-stu-id="70701-163">As necessary, it shows users hello Azure AD sign-in page before acquiring hello requested token.</span></span>
4. <span data-ttu-id="70701-164">Připojte hello žádost o rozhraní Graph API tokenu toohello přístup v hello **autorizace** hlavičky:</span><span class="sxs-lookup"><span data-stu-id="70701-164">Attach hello access token toohello Graph API request in hello **Authorization** header:</span></span>

    ```C#
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

<span data-ttu-id="70701-165">To je všechno pro hello `DirectorySearcher` kódu spojené s PCL a hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="70701-165">That's all for hello `DirectorySearcher` PCL and hello app's identity-related code.</span></span> <span data-ttu-id="70701-166">Zbývá toocall hello `SearchByAlias(...)` metoda v zobrazení pro každou platformu a v případě potřeby tooadd kód pro zpracování správně hello životní cyklus uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="70701-166">All that remains is toocall hello `SearchByAlias(...)` method in each platform's views and, where necessary, tooadd code for correctly handling hello UI lifecycle.</span></span>

### <a name="android"></a><span data-ttu-id="70701-167">Android</span><span class="sxs-lookup"><span data-stu-id="70701-167">Android</span></span>
1. <span data-ttu-id="70701-168">V MainActivity.cs, přidejte volání příliš`SearchByAlias(...)` v hello tlačítko klikněte na obslužnou rutinu:</span><span class="sxs-lookup"><span data-stu-id="70701-168">In MainActivity.cs, add a call too`SearchByAlias(...)` in hello button click handler:</span></span>

    ```C#
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. <span data-ttu-id="70701-169">Přepsání hello `OnActivityResult` životního cyklu metoda tooforward přesměruje back toohello odpovídající metodu ověřování.</span><span class="sxs-lookup"><span data-stu-id="70701-169">Override hello `OnActivityResult` lifecycle method tooforward any authentication redirects back toohello appropriate method.</span></span> <span data-ttu-id="70701-170">ADAL poskytuje Pomocná metoda pro to v Android:</span><span class="sxs-lookup"><span data-stu-id="70701-170">ADAL provides a helper method for this in Android:</span></span>

    ```C#
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a><span data-ttu-id="70701-171">Windows Desktop</span><span class="sxs-lookup"><span data-stu-id="70701-171">Windows Desktop</span></span>
<span data-ttu-id="70701-172">V MainWindow.xaml.cs, zkontrolujte volání příliš`SearchByAlias(...)` předáním `WindowInteropHelper` v hello plochy `PlatformParameters` objektu:</span><span class="sxs-lookup"><span data-stu-id="70701-172">In MainWindow.xaml.cs, make a call too`SearchByAlias(...)` by passing a `WindowInteropHelper` in hello desktop's `PlatformParameters` object:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a><span data-ttu-id="70701-173">iOS</span><span class="sxs-lookup"><span data-stu-id="70701-173">iOS</span></span>
<span data-ttu-id="70701-174">V DirSearchClient_iOSViewController.cs, hello iOS `PlatformParameters` objekt trvá odkaz toohello řadiče zobrazení:</span><span class="sxs-lookup"><span data-stu-id="70701-174">In DirSearchClient_iOSViewController.cs, hello iOS `PlatformParameters` object takes a reference toohello View Controller:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a><span data-ttu-id="70701-175">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="70701-175">Windows Universal</span></span>
<span data-ttu-id="70701-176">Otevřete MainPage.xaml.cs v univerzální pro Windows a potom implementovat hello `Search` metoda.</span><span class="sxs-lookup"><span data-stu-id="70701-176">In Windows Universal, open MainPage.xaml.cs, and then implement hello `Search` method.</span></span> <span data-ttu-id="70701-177">Tato metoda používá metodu helper v sdílený projekt tooupdate uživatelského rozhraní podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="70701-177">This method uses a helper method in a shared project tooupdate UI as necessary.</span></span>

```C#
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

## <a name="whats-next"></a><span data-ttu-id="70701-178">Kam dál</span><span class="sxs-lookup"><span data-stu-id="70701-178">What's next</span></span>
<span data-ttu-id="70701-179">Teď máte funkční aplikaci Xamarin, můžete ověřovat uživatele a bezpečně volat webové rozhraní API pomocí standardu OAuth 2.0 napříč pět různých platformách.</span><span class="sxs-lookup"><span data-stu-id="70701-179">You now have a working Xamarin app that can authenticate users and securely call web APIs by using OAuth 2.0 across five different platforms.</span></span>

<span data-ttu-id="70701-180">Pokud již nejsou naplněny vašeho klienta s uživateli, teď proto je toodo čas hello.</span><span class="sxs-lookup"><span data-stu-id="70701-180">If you haven’t already populated your tenant with users, now is hello time toodo so.</span></span>

1. <span data-ttu-id="70701-181">Spuštění aplikace DirectorySearcher a pak se přihlaste pomocí jeden z uživatelů hello.</span><span class="sxs-lookup"><span data-stu-id="70701-181">Run your DirectorySearcher app, and then sign in with one of hello users.</span></span>
2. <span data-ttu-id="70701-182">Hledání jiných uživatelů podle jejich UPN.</span><span class="sxs-lookup"><span data-stu-id="70701-182">Search for other users based on their UPN.</span></span>

<span data-ttu-id="70701-183">ADAL umožňuje snadno tooincorporate běžné funkce identity do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="70701-183">ADAL makes it easy tooincorporate common identity features into hello app.</span></span> <span data-ttu-id="70701-184">Se postará všechny pracovní dirty hello, jako je například Správa mezipaměti podpora protokolu OAuth, prezentací hello uživatele s přihlašovacími údaji uživatelského rozhraní, a aktualizovat platnost tokenů.</span><span class="sxs-lookup"><span data-stu-id="70701-184">It takes care of all hello dirty work for you, such as cache management, OAuth protocol support, presenting hello user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="70701-185">Je třeba volání tooknow pouze jediného rozhraní API `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="70701-185">You need tooknow only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="70701-186">Odkaz, stáhněte si hello [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (bez vašich hodnot nastavení).</span><span class="sxs-lookup"><span data-stu-id="70701-186">For reference, download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="70701-187">Nyní se můžete přesunout na tooadditional identity scénáře.</span><span class="sxs-lookup"><span data-stu-id="70701-187">You can now move on tooadditional identity scenarios.</span></span> <span data-ttu-id="70701-188">Zkuste například [zabezpečení webového rozhraní API .NET s Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="70701-188">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
