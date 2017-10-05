---
title: "Začínáme se službou Azure AD Xamarin | Microsoft Docs"
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
ms.openlocfilehash: 54ee403f283bc5dc79911e2e813dd513ff595828
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-ad-with-xamarin-apps"></a><span data-ttu-id="1b9b9-103">Integrace Azure AD s aplikacemi Xamarin</span><span class="sxs-lookup"><span data-stu-id="1b9b9-103">Integrate Azure AD with Xamarin apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="1b9b9-104">S Xamarinem může zapisovat mobilní aplikace v jazyce C#, můžete spustit na iOS, Android a Windows (mobilních zařízení a počítačů).</span><span class="sxs-lookup"><span data-stu-id="1b9b9-104">With Xamarin, you can write mobile apps in C# that can run on iOS, Android, and Windows (mobile devices and PCs).</span></span> <span data-ttu-id="1b9b9-105">Pokud vytváříte aplikace pomocí Xamarinu, Azure Active Directory (Azure AD) usnadňuje ověřování uživatelů s účty služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-105">If you're building an app using Xamarin, Azure Active Directory (Azure AD) makes it simple to authenticate users with their Azure AD accounts.</span></span> <span data-ttu-id="1b9b9-106">Aplikace může také bezpečně využívat žádné webové rozhraní API, který je chráněný službou Azure AD, jako je například rozhraní API Office 365 nebo rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-106">The app can also securely consume any web API that's protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="1b9b9-107">Pro aplikace Xamarin, které potřebují přístup k chráněným prostředkům Azure AD poskytuje službě Active Directory Authentication Library (ADAL).</span><span class="sxs-lookup"><span data-stu-id="1b9b9-107">For Xamarin apps that need to access protected resources, Azure AD provides the Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="1b9b9-108">Jediný účel ADAL je snadno a získat tokeny přístupu aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-108">The sole purpose of ADAL is to make it easy for apps to get access tokens.</span></span> <span data-ttu-id="1b9b9-109">K předvedení, jak je snadné, tento článek ukazuje, jak vytvářet aplikace DirectorySearcher který:</span><span class="sxs-lookup"><span data-stu-id="1b9b9-109">To demonstrate how easy it is, this article shows how to build DirectorySearcher apps that:</span></span>

* <span data-ttu-id="1b9b9-110">Spusťte na iOS, Android, Windows Desktop, Windows Phone a Windows Store.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-110">Run on iOS, Android, Windows Desktop, Windows Phone, and Windows Store.</span></span>
* <span data-ttu-id="1b9b9-111">Použití jedné přenosných tříd knihovny (PCL) k ověření uživatelů a získat tokeny pro Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-111">Use a single portable class library (PCL) to authenticate users and get tokens for the Azure AD Graph API.</span></span>
* <span data-ttu-id="1b9b9-112">Vyhledejte adresář pro uživatele s danou UPN.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-112">Search a directory for users with a given UPN.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="1b9b9-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1b9b9-113">Before you get started</span></span>
* <span data-ttu-id="1b9b9-114">Stažení [kostru projektu](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), nebo stáhnout [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="1b9b9-114">Download the [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), or download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="1b9b9-115">Každého stažení je řešením sady Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-115">Each download is a Visual Studio 2013 solution.</span></span>
* <span data-ttu-id="1b9b9-116">Musíte taky klient služby Azure AD, ve kterém se má vytvořit uživatele a registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-116">You also need an Azure AD tenant in which to create users and register the app.</span></span> <span data-ttu-id="1b9b9-117">Pokud ještě nemáte klienta, [zjistěte, jak získat](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="1b9b9-117">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="1b9b9-118">Až budete připravení, postupujte podle pokynů v další čtyři části.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-118">When you are ready, follow the procedures in the next four sections.</span></span>

## <a name="step-1-set-up-your-xamarin-development-environment"></a><span data-ttu-id="1b9b9-119">Krok 1: Nastavení vývojového prostředí Xamarin</span><span class="sxs-lookup"><span data-stu-id="1b9b9-119">Step 1: Set up your Xamarin development environment</span></span>
<span data-ttu-id="1b9b9-120">Tento kurz zahrnuje projekty pro iOS, Android a Windows, a proto musíte Visual Studio a Xamarin.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-120">Because this tutorial includes projects for iOS, Android, and Windows, you need both Visual Studio and Xamarin.</span></span> <span data-ttu-id="1b9b9-121">Vytvořte potřebné prostředí, dokončete proces v [nastavit až a nainstalujte Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-121">To create the necessary environment, complete the process in [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) on MSDN.</span></span> <span data-ttu-id="1b9b9-122">Pokyny zahrnují materiál, který najdete další informace o Xamarin, zatímco čekáte pro instalace dokončit.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-122">The instructions include material that you can review to learn more about Xamarin while you're waiting for the installations to be completed.</span></span>

<span data-ttu-id="1b9b9-123">Po dokončení instalace, otevřete řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-123">After you've completed the setup, open the solution in Visual Studio.</span></span> <span data-ttu-id="1b9b9-124">Zde najdete šesti projekty: pět projekty specifické pro platformu a jeden PCL DirectorySearcher.cs, které budou sdílet všechny platformy.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-124">There, you will find six projects: five platform-specific projects and one PCL, DirectorySearcher.cs, which will be shared across all platforms.</span></span>

## <a name="step-2-register-the-directorysearcher-app"></a><span data-ttu-id="1b9b9-125">Krok 2: Registraci DirectorySearcher aplikace</span><span class="sxs-lookup"><span data-stu-id="1b9b9-125">Step 2: Register the DirectorySearcher app</span></span>
<span data-ttu-id="1b9b9-126">Pokud chcete povolit aplikaci získat tokeny, musíte nejprve zaregistrovat v klientovi služby Azure AD a udělit mu oprávnění k přístupu k Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-126">To enable the app to get tokens, you first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API.</span></span> <span data-ttu-id="1b9b9-127">Zde je uveden postup:</span><span class="sxs-lookup"><span data-stu-id="1b9b9-127">Here's how:</span></span>

1. <span data-ttu-id="1b9b9-128">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1b9b9-128">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="1b9b9-129">Na horním panelu klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-129">On the top bar, click your account.</span></span> <span data-ttu-id="1b9b9-130">Potom v části **Directory** vyberte klienta služby Active Directory, ve které chcete zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-130">Then, under the **Directory** list, select the Active Directory tenant where you want to register the app.</span></span>
3. <span data-ttu-id="1b9b9-131">Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-131">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="1b9b9-132">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-132">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="1b9b9-133">Chcete-li vytvořit novou **nativní klientská aplikace**, postupujte podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-133">To create a new **Native Client Application**, follow the prompts.</span></span>
  * <span data-ttu-id="1b9b9-134">**Název** popis aplikace pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-134">**Name** describes the app to users.</span></span>
  * <span data-ttu-id="1b9b9-135">**Identifikátor URI pro přesměrování** je kombinace schématu a řetězec, Azure AD se používá k vrácení odpovědi tokenu.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-135">**Redirect URI** is a scheme and string combination that Azure AD uses to return token responses.</span></span> <span data-ttu-id="1b9b9-136">Zadejte hodnotu (například http://DirectorySearcher).</span><span class="sxs-lookup"><span data-stu-id="1b9b9-136">Enter a value (for example, http://DirectorySearcher).</span></span>
6. <span data-ttu-id="1b9b9-137">Po dokončení registrace Azure AD přiřadí aplikace ID jedinečný aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-137">After you’ve completed registration, Azure AD assigns the app a unique application ID.</span></span> <span data-ttu-id="1b9b9-138">Zkopírujte hodnotu z **aplikace** kartě, protože ho budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-138">Copy the value from the **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="1b9b9-139">Na **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-139">On the **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="1b9b9-140">Vyberte **Microsoft Graph** jako rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-140">Select **Microsoft Graph** as the API.</span></span> <span data-ttu-id="1b9b9-141">V části **delegovaná oprávnění**, přidejte **čtení dat adresáře** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-141">Under **Delegated Permissions**, add the **Read Directory Data** permission.</span></span>  
<span data-ttu-id="1b9b9-142">Tato akce umožní aplikaci dotaz rozhraní Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-142">This action enables the app to query the Graph API for users.</span></span>

## <a name="step-3-install-and-configure-adal"></a><span data-ttu-id="1b9b9-143">Krok 3: Instalace a konfigurace ADAL</span><span class="sxs-lookup"><span data-stu-id="1b9b9-143">Step 3: Install and configure ADAL</span></span>
<span data-ttu-id="1b9b9-144">Nyní když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-144">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="1b9b9-145">Pokud chcete povolit ADAL ke komunikaci s Azure AD, jí některé informace o registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-145">To enable ADAL to communicate with Azure AD, give it some information about the app registration.</span></span>

1. <span data-ttu-id="1b9b9-146">Přidání ADAL do projektu DirectorySearcher pomocí konzoly Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-146">Add ADAL to the DirectorySearcher project by using the Package Manager Console.</span></span>

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

    <span data-ttu-id="1b9b9-147">Všimněte si, že dva odkazy knihovny jsou přidány do jednotlivých projektů: část PCL ADAL a část specifické pro platformu.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-147">Note that two library references are added to each project: the PCL portion of ADAL and a platform-specific portion.</span></span>
2. <span data-ttu-id="1b9b9-148">V projektu DirectorySearcherLib otevřete DirectorySearcher.cs.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-148">In the DirectorySearcherLib project, open DirectorySearcher.cs.</span></span>
3. <span data-ttu-id="1b9b9-149">Nahraďte hodnoty členů třídy hodnoty, které jste zadali v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-149">Replace the class member values with the values that you entered in the Azure portal.</span></span> <span data-ttu-id="1b9b9-150">Váš kód odkazoval na tyto hodnoty vždy, když ho využívá ADAL.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-150">Your code refers to these values whenever it uses ADAL.</span></span>

  * <span data-ttu-id="1b9b9-151">*Klienta* je doména klienta služby Azure AD (například contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="1b9b9-151">The *tenant* is the domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="1b9b9-152">*ClientId* je ID klienta aplikace, které jste zkopírovali z portálu.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-152">The *clientId* is the client ID of the app, which you copied from the portal.</span></span>
  * <span data-ttu-id="1b9b9-153">*ReturnUri* je identifikátor URI, který jste zadali v portálu (například http://DirectorySearcher) přesměrování.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-153">The *returnUri* is the redirect URI that you entered in the portal (for example, http://DirectorySearcher).</span></span>

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a><span data-ttu-id="1b9b9-154">Krok 4: Použití ADAL získat tokeny z Azure AD</span><span class="sxs-lookup"><span data-stu-id="1b9b9-154">Step 4: Use ADAL to get tokens from Azure AD</span></span>
<span data-ttu-id="1b9b9-155">Téměř všechny aplikace logiky ověřování spočívá v `DirectorySearcher.SearchByAlias(...)`.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-155">Almost all of the app's authentication logic lies in `DirectorySearcher.SearchByAlias(...)`.</span></span> <span data-ttu-id="1b9b9-156">Všechny, které je nezbytné v projektech specifických pro platformy je předat kontextové parametr, který se `DirectorySearcher` PCL.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-156">All that's necessary in the platform-specific projects is to pass a contextual parameter to the `DirectorySearcher` PCL.</span></span>

1. <span data-ttu-id="1b9b9-157">Otevřete DirectorySearcher.cs a pak přidejte nový parametr, který se `SearchByAlias(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-157">Open DirectorySearcher.cs, and then add a new parameter to the `SearchByAlias(...)` method.</span></span> <span data-ttu-id="1b9b9-158">`IPlatformParameters`je kontextová parametr, který zapouzdřuje specifické pro platformu objekty, které potřebuje provést ověřování ADAL.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-158">`IPlatformParameters` is the contextual parameter that encapsulates the platform-specific objects that ADAL needs to perform the authentication.</span></span>

    ```C#
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. <span data-ttu-id="1b9b9-159">Inicializace `AuthenticationContext`, což je primární třídou adal.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-159">Initialize `AuthenticationContext`, which is the primary class of ADAL.</span></span>  
<span data-ttu-id="1b9b9-160">Tato akce předá ADAL souřadnice ke komunikaci s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-160">This action passes ADAL the coordinates it needs to communicate with Azure AD.</span></span>
3. <span data-ttu-id="1b9b9-161">Volání `AcquireTokenAsync(...)`, který přijímá `IPlatformParameters` objektu a vyvolá tok ověřování, které jsou nutné k návratu token do aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-161">Call `AcquireTokenAsync(...)`, which accepts the `IPlatformParameters` object and invokes the authentication flow that's necessary to return a token to the app.</span></span>

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

    <span data-ttu-id="1b9b9-162">`AcquireTokenAsync(...)`Nejprve se pokusí vrátit token pro požadovaný prostředek (rozhraní Graph API v tomto případě) bez vyzvání uživatele k zadání přihlašovacích údajů (prostřednictvím ukládání do mezipaměti nebo obnovení původního tokeny).</span><span class="sxs-lookup"><span data-stu-id="1b9b9-162">`AcquireTokenAsync(...)` first attempts to return a token for the requested resource (the Graph API in this case) without prompting users to enter their credentials (via caching or refreshing old tokens).</span></span> <span data-ttu-id="1b9b9-163">Podle potřeby zobrazuje uživatele, kteří na přihlašovací stránku služby Azure AD před získávání požadovaný token.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-163">As necessary, it shows users the Azure AD sign-in page before acquiring the requested token.</span></span>
4. <span data-ttu-id="1b9b9-164">Připojit k rozhraní Graph API požadavku v tokenu přístupu **autorizace** hlavičky:</span><span class="sxs-lookup"><span data-stu-id="1b9b9-164">Attach the access token to the Graph API request in the **Authorization** header:</span></span>

    ```C#
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

<span data-ttu-id="1b9b9-165">To je všechno pro `DirectorySearcher` PCL a aplikace je spojené s kódem.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-165">That's all for the `DirectorySearcher` PCL and the app's identity-related code.</span></span> <span data-ttu-id="1b9b9-166">Zbývá k volání `SearchByAlias(...)` metoda v zobrazení pro každou platformu a v případě potřeby přidejte kód pro zpracování správně životní cyklus uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-166">All that remains is to call the `SearchByAlias(...)` method in each platform's views and, where necessary, to add code for correctly handling the UI lifecycle.</span></span>

### <a name="android"></a><span data-ttu-id="1b9b9-167">Android</span><span class="sxs-lookup"><span data-stu-id="1b9b9-167">Android</span></span>
1. <span data-ttu-id="1b9b9-168">V MainActivity.cs, že přidáte volání `SearchByAlias(...)` v tlačítko klikněte na obslužnou rutinu:</span><span class="sxs-lookup"><span data-stu-id="1b9b9-168">In MainActivity.cs, add a call to `SearchByAlias(...)` in the button click handler:</span></span>

    ```C#
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. <span data-ttu-id="1b9b9-169">Přepsání `OnActivityResult` životního cyklu metoda předávat žádné ověřování přesměruje zpět na odpovídající metodu.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-169">Override the `OnActivityResult` lifecycle method to forward any authentication redirects back to the appropriate method.</span></span> <span data-ttu-id="1b9b9-170">ADAL poskytuje Pomocná metoda pro to v Android:</span><span class="sxs-lookup"><span data-stu-id="1b9b9-170">ADAL provides a helper method for this in Android:</span></span>

    ```C#
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a><span data-ttu-id="1b9b9-171">Windows Desktop</span><span class="sxs-lookup"><span data-stu-id="1b9b9-171">Windows Desktop</span></span>
<span data-ttu-id="1b9b9-172">V MainWindow.xaml.cs, zkontrolujte volání `SearchByAlias(...)` předáním `WindowInteropHelper` na ploše Desktop `PlatformParameters` objektu:</span><span class="sxs-lookup"><span data-stu-id="1b9b9-172">In MainWindow.xaml.cs, make a call to `SearchByAlias(...)` by passing a `WindowInteropHelper` in the desktop's `PlatformParameters` object:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a><span data-ttu-id="1b9b9-173">iOS</span><span class="sxs-lookup"><span data-stu-id="1b9b9-173">iOS</span></span>
<span data-ttu-id="1b9b9-174">V DirSearchClient_iOSViewController.cs, iOS `PlatformParameters` objekt trvá odkaz na řadiče zobrazení:</span><span class="sxs-lookup"><span data-stu-id="1b9b9-174">In DirSearchClient_iOSViewController.cs, the iOS `PlatformParameters` object takes a reference to the View Controller:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a><span data-ttu-id="1b9b9-175">Univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="1b9b9-175">Windows Universal</span></span>
<span data-ttu-id="1b9b9-176">Otevřete MainPage.xaml.cs v univerzální pro Windows a potom implementovat `Search` metoda.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-176">In Windows Universal, open MainPage.xaml.cs, and then implement the `Search` method.</span></span> <span data-ttu-id="1b9b9-177">Tato metoda používá metodu helper v projektu sdíleného aktualizace uživatelského rozhraní podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-177">This method uses a helper method in a shared project to update UI as necessary.</span></span>

```C#
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

## <a name="whats-next"></a><span data-ttu-id="1b9b9-178">Kam dál</span><span class="sxs-lookup"><span data-stu-id="1b9b9-178">What's next</span></span>
<span data-ttu-id="1b9b9-179">Teď máte funkční aplikaci Xamarin, můžete ověřovat uživatele a bezpečně volat webové rozhraní API pomocí standardu OAuth 2.0 napříč pět různých platformách.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-179">You now have a working Xamarin app that can authenticate users and securely call web APIs by using OAuth 2.0 across five different platforms.</span></span>

<span data-ttu-id="1b9b9-180">Pokud již nejsou naplněny vašeho klienta s uživateli, nyní je čas Uděláte to tak.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-180">If you haven’t already populated your tenant with users, now is the time to do so.</span></span>

1. <span data-ttu-id="1b9b9-181">Spuštění aplikace DirectorySearcher a pak se přihlaste pomocí jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-181">Run your DirectorySearcher app, and then sign in with one of the users.</span></span>
2. <span data-ttu-id="1b9b9-182">Hledání jiných uživatelů podle jejich UPN.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-182">Search for other users based on their UPN.</span></span>

<span data-ttu-id="1b9b9-183">ADAL usnadňuje začlenit běžné funkce identity do aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-183">ADAL makes it easy to incorporate common identity features into the app.</span></span> <span data-ttu-id="1b9b9-184">Se postará všechnu práci dirty, jako je například Správa mezipaměti podpora protokolu OAuth, představuje uživatele s přihlašovacími údaji uživatelského rozhraní, a aktualizovat platnost tokenů.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-184">It takes care of all the dirty work for you, such as cache management, OAuth protocol support, presenting the user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="1b9b9-185">Je nutné znát jenom jednoho volání rozhraní API, `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-185">You need to know only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="1b9b9-186">Odkaz, stáhněte si [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (bez vašich hodnot nastavení).</span><span class="sxs-lookup"><span data-stu-id="1b9b9-186">For reference, download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="1b9b9-187">Nyní se můžete přesunout další identity scénářů.</span><span class="sxs-lookup"><span data-stu-id="1b9b9-187">You can now move on to additional identity scenarios.</span></span> <span data-ttu-id="1b9b9-188">Zkuste například [zabezpečení webového rozhraní API .NET s Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="1b9b9-188">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
