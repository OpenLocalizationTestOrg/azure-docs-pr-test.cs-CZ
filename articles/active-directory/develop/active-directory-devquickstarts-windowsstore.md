---
title: "Začínáme se službou Azure AD Windows Store | Microsoft Docs"
description: "Aplikace pro Windows Store sestavení, které integraci se službou Azure AD pro přihlášení a volání služby Azure AD chráněné rozhraní API pomocí metody OAuth."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 3b96a6d1-270b-4ac1-b9b5-58070c896a68
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 09/16/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 6b5189dc06d7f8b0ed4426944948b904feba847e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="integrate-azure-ad-with-windows-store-apps"></a><span data-ttu-id="e5ddd-103">Integrace Azure AD s aplikací pro Windows Store</span><span class="sxs-lookup"><span data-stu-id="e5ddd-103">Integrate Azure AD with Windows Store apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> <span data-ttu-id="e5ddd-104">Windows Store 8.1 a předchozí verze projekty nejsou podporované v Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-104">Windows Store 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="e5ddd-105">Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="e5ddd-105">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

<span data-ttu-id="e5ddd-106">Pokud vyvíjíte aplikace pro Windows Store, Azure Active Directory (Azure AD) je jednoduchá a přímočará k ověřování uživatelů s účty služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-106">If you're developing apps for the Windows Store, Azure Active Directory (Azure AD) makes it simple and straightforward to authenticate your users with their Active Directory accounts.</span></span> <span data-ttu-id="e5ddd-107">Díky integraci s Azure AD, můžete aplikace bezpečně využívat žádné webové rozhraní API, který je chráněný službou Azure AD, jako je například rozhraní API Office 365 nebo rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-107">By integrating with Azure AD, an app can securely consume any web API that's protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="e5ddd-108">Pro desktopové aplikace Windows Store, které potřebují přístup k chráněným prostředkům Azure AD poskytuje službě Active Directory Authentication Library (ADAL).</span><span class="sxs-lookup"><span data-stu-id="e5ddd-108">For Windows Store desktop apps that need to access protected resources, Azure AD provides the Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="e5ddd-109">Jediný účel ADAL je snadno pro aplikaci načíst tokeny přístupu.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-109">The sole purpose of ADAL is to make it easy for the app to get access tokens.</span></span> <span data-ttu-id="e5ddd-110">K předvedení, jak je snadné, tento článek ukazuje, jak vytvářet aplikace pro DirectorySearcher Windows Store který:</span><span class="sxs-lookup"><span data-stu-id="e5ddd-110">To demonstrate how easy it is, this article shows how to build a DirectorySearcher Windows Store app that:</span></span>

* <span data-ttu-id="e5ddd-111">Získá přístup k tokeny pro volání rozhraní API služby Azure AD Graph pomocí [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5ddd-111">Gets access tokens for calling the Azure AD Graph API by using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="e5ddd-112">Vyhledá adresář pro uživatele s hlavní název daného uživatele (UPN).</span><span class="sxs-lookup"><span data-stu-id="e5ddd-112">Searches a directory for users with a given user principal name (UPN).</span></span>
* <span data-ttu-id="e5ddd-113">Uživatelé přihlásí se.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-113">Signs users out.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="e5ddd-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="e5ddd-114">Before you get started</span></span>
* <span data-ttu-id="e5ddd-115">Stažení [kostru projektu](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), nebo stáhnout [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="e5ddd-115">Download the [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), or download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span></span> <span data-ttu-id="e5ddd-116">Každého stažení je řešením sady Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-116">Each download is a Visual Studio 2015 solution.</span></span>
* <span data-ttu-id="e5ddd-117">Musíte taky klient služby Azure AD, ve kterém se má vytvořit uživatele a registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-117">You also need an Azure AD tenant in which to create users and register the app.</span></span> <span data-ttu-id="e5ddd-118">Pokud ještě nemáte klienta, [zjistěte, jak získat](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="e5ddd-118">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="e5ddd-119">Až budete připravení, postupujte podle pokynů v následujících třech částech.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-119">When you are ready, follow the procedures in the next three sections.</span></span>

## <a name="step-1-register-the-directorysearcher-app"></a><span data-ttu-id="e5ddd-120">Krok 1: Zaregistrujte DirectorySearcher aplikace</span><span class="sxs-lookup"><span data-stu-id="e5ddd-120">Step 1: Register the DirectorySearcher app</span></span>
<span data-ttu-id="e5ddd-121">Pokud chcete povolit aplikaci získat tokeny, musíte nejprve zaregistrovat v klientovi služby Azure AD a udělit mu oprávnění k přístupu k Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-121">To enable the app to get tokens, you first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API.</span></span> <span data-ttu-id="e5ddd-122">Zde je uveden postup:</span><span class="sxs-lookup"><span data-stu-id="e5ddd-122">Here's how:</span></span>

1. <span data-ttu-id="e5ddd-123">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e5ddd-123">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e5ddd-124">Na horním panelu klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-124">On the top bar, click your account.</span></span> <span data-ttu-id="e5ddd-125">Potom v části **Directory** vyberte klienta služby Active Directory, ve které chcete zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-125">Then, under the **Directory** list, select the Active Directory tenant where you want to register the app.</span></span>
3. <span data-ttu-id="e5ddd-126">Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-126">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="e5ddd-127">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-127">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="e5ddd-128">Postupujte podle výzev a vytvořte **nativní klientská aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-128">Follow the prompts to create a **Native Client Application**.</span></span>
  * <span data-ttu-id="e5ddd-129">**Název** popis aplikace pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-129">**Name** describes the app to users.</span></span>
  * <span data-ttu-id="e5ddd-130">**Identifikátor URI pro přesměrování** je kombinace schématu a řetězec, Azure AD se používá k vrácení odpovědi tokenu.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-130">**Redirect URI** is a scheme and string combination that Azure AD uses to return token responses.</span></span> <span data-ttu-id="e5ddd-131">Zadejte hodnotu zástupného symbolu pro nyní (například **http://DirectorySearcher**).</span><span class="sxs-lookup"><span data-stu-id="e5ddd-131">Enter a placeholder value for now (for example, **http://DirectorySearcher**).</span></span> <span data-ttu-id="e5ddd-132">Později budete nahradí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-132">You'll replace the value later.</span></span>
6. <span data-ttu-id="e5ddd-133">Po dokončení registrace Azure AD přiřadí aplikace ID jedinečný aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-133">After you’ve completed the registration, Azure AD assigns the app a unique application ID.</span></span> <span data-ttu-id="e5ddd-134">Zkopírujte hodnotu na **aplikace** kartě, protože ho budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-134">Copy the value on the **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="e5ddd-135">Na **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-135">On the **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="e5ddd-136">Pro **Azure Active Directory** aplikace, vyberte **Microsoft Graph** jako rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-136">For the **Azure Active Directory** app, select **Microsoft Graph** as the API.</span></span>
9. <span data-ttu-id="e5ddd-137">V části **delegovaná oprávnění**, přidejte **přístup k adresáři jako přihlášeného uživatele** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-137">Under **Delegated Permissions**, add the **Access the directory as the signed-in user** permission.</span></span> <span data-ttu-id="e5ddd-138">Díky tomu aplikaci dotaz rozhraní Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-138">Doing so enables the app to query the Graph API for users.</span></span>

## <a name="step-2-install-and-configure-adal"></a><span data-ttu-id="e5ddd-139">Krok 2: Instalace a konfigurace ADAL</span><span class="sxs-lookup"><span data-stu-id="e5ddd-139">Step 2: Install and configure ADAL</span></span>
<span data-ttu-id="e5ddd-140">Nyní když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-140">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="e5ddd-141">Pokud chcete povolit ADAL ke komunikaci s Azure AD, jí některé informace o registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-141">To enable ADAL to communicate with Azure AD, give it some information about the app registration.</span></span>

1. <span data-ttu-id="e5ddd-142">Přidání ADAL do projektu DirectorySearcher pomocí konzoly Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-142">Add ADAL to the DirectorySearcher project by using the Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. <span data-ttu-id="e5ddd-143">V projektu DirectorySearcher otevřete MainPage.xaml.cs.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-143">In the DirectorySearcher project, open MainPage.xaml.cs.</span></span>
3. <span data-ttu-id="e5ddd-144">Nahraďte hodnoty v **konfiguračních hodnot** oblasti s hodnotami, které jste zadali v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-144">Replace the values in the **Config Values** region with the values that you entered in the Azure portal.</span></span> <span data-ttu-id="e5ddd-145">Váš kód odkazoval na tyto hodnoty vždy, když ho využívá ADAL.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-145">Your code refers to these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="e5ddd-146">*Klienta* je doména klienta služby Azure AD (například contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="e5ddd-146">The *tenant* is the domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="e5ddd-147">*ClientId* je ID klienta aplikace, které jste zkopírovali z portálu.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-147">The *clientId* is the client ID of the app, which you copied from the portal.</span></span>
4. <span data-ttu-id="e5ddd-148">Nyní je třeba zjistit identifikátor URI zpětného volání pro aplikace pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-148">You now need to discover the callback URI for your Windows Store app.</span></span> <span data-ttu-id="e5ddd-149">Na tomto řádku v zarážku `MainPage` metoda:</span><span class="sxs-lookup"><span data-stu-id="e5ddd-149">Set a breakpoint on this line in the `MainPage` method:</span></span>
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. <span data-ttu-id="e5ddd-150">Sestavte řešení, a ujistěte se, že se obnoví všechny odkazy na balíček.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-150">Build the solution, making sure that all package references are restored.</span></span> <span data-ttu-id="e5ddd-151">Pokud chybí všechny balíčky, otevřete Správce balíčků NuGet a jejich obnovení.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-151">If any packages are missing, open the NuGet Package Manager and restore them.</span></span>
6. <span data-ttu-id="e5ddd-152">Spusťte aplikaci a zkopírujte hodnotu `redirectUri` při průchodu zarážkou.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-152">Run the app, and copy the value of `redirectUri` when the breakpoint is hit.</span></span> <span data-ttu-id="e5ddd-153">Hodnota by měla vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="e5ddd-153">The value should look something like the following:</span></span>

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. <span data-ttu-id="e5ddd-154">Zpět na **nastavení** karta aplikace na webu Azure portal a přidat **RedirectUri** s předchozí hodnotou.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-154">Back on the **Settings** tab of the app in the Azure portal, add a **RedirectUri** with the preceding value.</span></span>  

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a><span data-ttu-id="e5ddd-155">Krok 3: Použití ADAL získat tokeny z Azure AD</span><span class="sxs-lookup"><span data-stu-id="e5ddd-155">Step 3: Use ADAL to get tokens from Azure AD</span></span>
<span data-ttu-id="e5ddd-156">Základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá `authContext.AcquireToken(…)`, a zbývající ADAL.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-156">The basic principle behind ADAL is that whenever the app needs an access token, it simply calls `authContext.AcquireToken(…)`, and ADAL does the rest.</span></span>  

1. <span data-ttu-id="e5ddd-157">Inicializace aplikace `AuthenticationContext`, což je primární třídou adal.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-157">Initialize the app’s `AuthenticationContext`, which is the primary class of ADAL.</span></span> <span data-ttu-id="e5ddd-158">Tato akce předá ADAL souřadnice musí komunikovat s Azure AD a určit, jak pro ukládání do mezipaměti tokenů.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-158">This action passes ADAL the coordinates it needs to communicate with Azure AD and tell it how to cache tokens.</span></span>

    ```C#
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. <span data-ttu-id="e5ddd-159">Vyhledejte `Search(...)` metodu, která je volána, když uživatelé kliknou na **vyhledávání** tlačítko v uživatelském rozhraní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-159">Locate the `Search(...)` method, which is invoked when users click the **Search** button on the app's UI.</span></span> <span data-ttu-id="e5ddd-160">Tato metoda vytváří požadavek get na Azure AD Graph API k dotazu pro uživatele, jehož UPN začíná zadaný hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-160">This method makes a get request to the Azure AD Graph API to query for users whose UPN begins with the given search term.</span></span> <span data-ttu-id="e5ddd-161">Dotaz na rozhraní Graph API, zahrňte přístupový token v žádosti **autorizace** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-161">To query the Graph API, include an access token in the request's **Authorization** header.</span></span> <span data-ttu-id="e5ddd-162">Toto je, kde odeslán ADAL.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-162">This is where ADAL comes in.</span></span>

    ```C#
    private async void Search(object sender, RoutedEventArgs e)
    {
        ...
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "authentication_canceled")
            {
                ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    <span data-ttu-id="e5ddd-163">Když aplikace požaduje token voláním `AcquireTokenAsync(...)`, ADAL pokusí vrátit token bez požadavku uživatele na přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-163">When the app requests a token by calling `AcquireTokenAsync(...)`, ADAL attempts to return a token without asking the user for credentials.</span></span> <span data-ttu-id="e5ddd-164">Pokud ADAL zjistí, že uživatel musí pro přihlášení k získání tokenu, zobrazí přihlašovací dialogové okno, shromažďuje přihlašovací údaje uživatele a vrátí token po úspěšném provedení ověřování.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-164">If ADAL determines that the user needs to sign in to get a token, it displays a sign-in dialog box, collects the user's credentials, and returns a token after authentication succeeds.</span></span> <span data-ttu-id="e5ddd-165">Pokud se nepodařilo vrátit token z jakéhokoli důvodu ADAL *AuthenticationResult* stav je k chybě.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-165">If ADAL is unable to return a token for any reason, the *AuthenticationResult* status is an error.</span></span>
3. <span data-ttu-id="e5ddd-166">Nyní je čas použití tokenu přístupu, kterou jste právě získali.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-166">Now it's time to use the access token you just acquired.</span></span> <span data-ttu-id="e5ddd-167">Také v `Search(...)` metoda, připojit k rozhraní Graph API požadavek get v tokenu **autorizace** hlavičky:</span><span class="sxs-lookup"><span data-stu-id="e5ddd-167">Also in the `Search(...)` method, attach the token to the Graph API get request in the **Authorization** header:</span></span>

    ```C#
    // Add the access token to the Authorization header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. <span data-ttu-id="e5ddd-168">Můžete použít `AuthenticationResult` objekt, který chcete zobrazit informace o uživateli v aplikaci, jako je například ID uživatele:</span><span class="sxs-lookup"><span data-stu-id="e5ddd-168">You can use the `AuthenticationResult` object to display information about the user in the app, such as the user's ID:</span></span>

    ```C#
    // Update the page UI to represent the signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. <span data-ttu-id="e5ddd-169">Můžete taky ADAL pro přihlášení uživatelé mimo aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-169">You can also use ADAL to sign users out of the app.</span></span> <span data-ttu-id="e5ddd-170">Když uživatel klikne **Odhlásit** tlačítko, ujistěte se, že další volání `AcquireTokenAsync(...)` zobrazení přihlášení.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-170">When the user clicks the **Sign Out** button, ensure that the next call to `AcquireTokenAsync(...)` shows a sign-in view.</span></span> <span data-ttu-id="e5ddd-171">Pomocí knihovny ADAL tato akce je stejně snadná jako vymazání mezipamětí tokenů:</span><span class="sxs-lookup"><span data-stu-id="e5ddd-171">With ADAL, this action is as easy as clearing the token cache:</span></span>

    ```C#
    private void SignOut()
    {
        // Clear session state from the token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a><span data-ttu-id="e5ddd-172">Kam dál</span><span class="sxs-lookup"><span data-stu-id="e5ddd-172">What's next</span></span>
<span data-ttu-id="e5ddd-173">Teď máte funkční aplikace pro Windows Store, můžete ověřovat uživatele, bezpečně volání webových rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-173">You now have a working Windows Store app that can authenticate users, securely call web APIs using OAuth 2.0, and get basic information about the user.</span></span>

<span data-ttu-id="e5ddd-174">Pokud již nejsou naplněny vašeho klienta s uživateli, nyní je čas Uděláte to tak.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-174">If you haven’t already populated your tenant with users, now is the time to do so.</span></span>
1. <span data-ttu-id="e5ddd-175">Spuštění aplikace DirectorySearcher a pak se přihlaste pomocí jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-175">Run your DirectorySearcher app, and then sign in with one of the users.</span></span>
2. <span data-ttu-id="e5ddd-176">Hledání jiných uživatelů podle jejich UPN.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-176">Search for other users based on their UPN.</span></span>
3. <span data-ttu-id="e5ddd-177">Zavřete aplikaci a znovu ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-177">Close the app, and rerun it.</span></span> <span data-ttu-id="e5ddd-178">Všimněte si, jak uživatelské relace zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-178">Notice how the user’s session remains intact.</span></span>
4. <span data-ttu-id="e5ddd-179">Odhlásit se kliknutím pravým tlačítkem myši zobrazíte dolním panelu a pak se přihlaste zpět v jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-179">Sign out by right-clicking to display the bottom bar, and then sign back in as another user.</span></span>

<span data-ttu-id="e5ddd-180">ADAL usnadňuje začlenit všechny tyto běžné funkce identity do aplikace.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-180">ADAL makes it easy to incorporate all these common identity features into the app.</span></span> <span data-ttu-id="e5ddd-181">Se postará všechnu práci dirty, jako je například Správa mezipaměti podpora protokolu OAuth, představuje uživatele s přihlašovacími údaji uživatelského rozhraní, a aktualizovat platnost tokenů.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-181">It takes care of all the dirty work for you, such as cache management, OAuth protocol support, presenting the user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="e5ddd-182">Je nutné znát jenom jednoho volání rozhraní API, `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-182">You need to know only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="e5ddd-183">Odkaz, stáhněte si [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (bez vašich hodnot nastavení).</span><span class="sxs-lookup"><span data-stu-id="e5ddd-183">For reference, download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="e5ddd-184">Nyní se můžete přesunout další identity scénářů.</span><span class="sxs-lookup"><span data-stu-id="e5ddd-184">You can now move on to additional identity scenarios.</span></span> <span data-ttu-id="e5ddd-185">Zkuste například [zabezpečení webového rozhraní API .NET s Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="e5ddd-185">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
