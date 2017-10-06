---
title: "aaaAzure AD Windows Store Začínáme | Microsoft Docs"
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
ms.openlocfilehash: 1d12c7b928bc0e94fb823f8db4a09ff416205e2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-windows-store-apps"></a><span data-ttu-id="3f4c3-103">Integrace Azure AD s aplikací pro Windows Store</span><span class="sxs-lookup"><span data-stu-id="3f4c3-103">Integrate Azure AD with Windows Store apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> <span data-ttu-id="3f4c3-104">Windows Store 8.1 a předchozí verze projekty nejsou podporované v Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-104">Windows Store 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="3f4c3-105">Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="3f4c3-105">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

<span data-ttu-id="3f4c3-106">Pokud vyvíjíte aplikace pro Windows Store hello, Azure Active Directory (Azure AD) umožňuje jednoduchá a přímočará tooauthenticate vaši uživatelé s účty služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-106">If you're developing apps for hello Windows Store, Azure Active Directory (Azure AD) makes it simple and straightforward tooauthenticate your users with their Active Directory accounts.</span></span> <span data-ttu-id="3f4c3-107">Díky integraci s Azure AD, můžete aplikaci bezpečně využívat žádné webové rozhraní API, který je chráněný službou Azure AD, jako je například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-107">By integrating with Azure AD, an app can securely consume any web API that's protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="3f4c3-108">Pro desktopové aplikace Windows Store, které vyžadují tooaccess chráněné zdroje Azure AD poskytuje hello Active Directory Authentication Library (ADAL).</span><span class="sxs-lookup"><span data-stu-id="3f4c3-108">For Windows Store desktop apps that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="3f4c3-109">jediným účelem Hello adal je toomake snadno hello aplikace tooget přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-109">hello sole purpose of ADAL is toomake it easy for hello app tooget access tokens.</span></span> <span data-ttu-id="3f4c3-110">jak snadné je, tento článek ukazuje, jak pro toobuild DirectorySearcher Windows Store toodemonstrate aplikace který:</span><span class="sxs-lookup"><span data-stu-id="3f4c3-110">toodemonstrate how easy it is, this article shows how toobuild a DirectorySearcher Windows Store app that:</span></span>

* <span data-ttu-id="3f4c3-111">Získá přístup k tokeny pro volání rozhraní API Azure AD Graph hello pomocí hello [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="3f4c3-111">Gets access tokens for calling hello Azure AD Graph API by using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="3f4c3-112">Vyhledá adresář pro uživatele s hlavní název daného uživatele (UPN).</span><span class="sxs-lookup"><span data-stu-id="3f4c3-112">Searches a directory for users with a given user principal name (UPN).</span></span>
* <span data-ttu-id="3f4c3-113">Uživatelé přihlásí se.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-113">Signs users out.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="3f4c3-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="3f4c3-114">Before you get started</span></span>
* <span data-ttu-id="3f4c3-115">Stáhnout hello [kostru projektu](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), nebo stáhnout hello [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="3f4c3-115">Download hello [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), or download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span></span> <span data-ttu-id="3f4c3-116">Každého stažení je řešením sady Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-116">Each download is a Visual Studio 2015 solution.</span></span>
* <span data-ttu-id="3f4c3-117">Musíte taky klient služby Azure AD, ve které toocreate uživatelé a aplikace hello registrace.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-117">You also need an Azure AD tenant in which toocreate users and register hello app.</span></span> <span data-ttu-id="3f4c3-118">Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="3f4c3-118">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="3f4c3-119">Až budete připravení, hello postupy hello postupujte podle kroků v následujících třech částech.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-119">When you are ready, follow hello procedures in hello next three sections.</span></span>

## <a name="step-1-register-hello-directorysearcher-app"></a><span data-ttu-id="3f4c3-120">Krok 1: Zaregistrujte hello DirectorySearcher aplikace</span><span class="sxs-lookup"><span data-stu-id="3f4c3-120">Step 1: Register hello DirectorySearcher app</span></span>
<span data-ttu-id="3f4c3-121">tooenable hello aplikace tooget tokeny, musíte nejprve tooregister ji ve službě Azure AD klienta a udělit mu oprávnění tooaccess hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-121">tooenable hello app tooget tokens, you first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API.</span></span> <span data-ttu-id="3f4c3-122">Zde je uveden postup:</span><span class="sxs-lookup"><span data-stu-id="3f4c3-122">Here's how:</span></span>

1. <span data-ttu-id="3f4c3-123">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3f4c3-123">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3f4c3-124">Na horním panelu hello klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-124">On hello top bar, click your account.</span></span> <span data-ttu-id="3f4c3-125">Potom v části hello **Directory** seznamu, vyberte hello klienta služby Active Directory místo tooregister hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-125">Then, under hello **Directory** list, select hello Active Directory tenant where you want tooregister hello app.</span></span>
3. <span data-ttu-id="3f4c3-126">Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-126">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="3f4c3-127">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-127">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="3f4c3-128">Postupujte podle pokynů toocreate hello **nativní klientská aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-128">Follow hello prompts toocreate a **Native Client Application**.</span></span>
  * <span data-ttu-id="3f4c3-129">**Název** popisuje toousers aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-129">**Name** describes hello app toousers.</span></span>
  * <span data-ttu-id="3f4c3-130">**Identifikátor URI pro přesměrování** schématu a řetězec kombinací, Azure AD používá tooreturn odpovědi tokenu.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-130">**Redirect URI** is a scheme and string combination that Azure AD uses tooreturn token responses.</span></span> <span data-ttu-id="3f4c3-131">Zadejte hodnotu zástupného symbolu pro nyní (například **http://DirectorySearcher**).</span><span class="sxs-lookup"><span data-stu-id="3f4c3-131">Enter a placeholder value for now (for example, **http://DirectorySearcher**).</span></span> <span data-ttu-id="3f4c3-132">Nahradíte hodnotu hello později.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-132">You'll replace hello value later.</span></span>
6. <span data-ttu-id="3f4c3-133">Po dokončení registrace hello, Azure AD přiřadí aplikace hello ID jedinečný aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-133">After you’ve completed hello registration, Azure AD assigns hello app a unique application ID.</span></span> <span data-ttu-id="3f4c3-134">Zkopírujte hodnotu hello na hello **aplikace** kartě, protože ho budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-134">Copy hello value on hello **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="3f4c3-135">Na hello **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-135">On hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="3f4c3-136">Pro hello **Azure Active Directory** aplikace, vyberte **Microsoft Graph** jako hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-136">For hello **Azure Active Directory** app, select **Microsoft Graph** as hello API.</span></span>
9. <span data-ttu-id="3f4c3-137">V části **delegovaná oprávnění**, přidejte hello **přístup k adresáři hello jako hello přihlášeného uživatele** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-137">Under **Delegated Permissions**, add hello **Access hello directory as hello signed-in user** permission.</span></span> <span data-ttu-id="3f4c3-138">Díky tomu hello aplikace tooquery hello rozhraní Graph API pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-138">Doing so enables hello app tooquery hello Graph API for users.</span></span>

## <a name="step-2-install-and-configure-adal"></a><span data-ttu-id="3f4c3-139">Krok 2: Instalace a konfigurace ADAL</span><span class="sxs-lookup"><span data-stu-id="3f4c3-139">Step 2: Install and configure ADAL</span></span>
<span data-ttu-id="3f4c3-140">Nyní když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-140">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="3f4c3-141">tooenable ADAL toocommunicate s Azure AD, poskytněte některé informace o registraci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-141">tooenable ADAL toocommunicate with Azure AD, give it some information about hello app registration.</span></span>

1. <span data-ttu-id="3f4c3-142">Přidejte ADAL toohello DirectorySearcher projekt pomocí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-142">Add ADAL toohello DirectorySearcher project by using hello Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. <span data-ttu-id="3f4c3-143">V projektu DirectorySearcher hello otevřete MainPage.xaml.cs.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-143">In hello DirectorySearcher project, open MainPage.xaml.cs.</span></span>
3. <span data-ttu-id="3f4c3-144">Nahraďte hodnoty hello v hello **konfiguračních hodnot** oblasti s hello hodnotami, které jste zadali v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-144">Replace hello values in hello **Config Values** region with hello values that you entered in hello Azure portal.</span></span> <span data-ttu-id="3f4c3-145">Váš kód odkazoval toothese hodnoty vždy, když ho využívá ADAL.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-145">Your code refers toothese values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="3f4c3-146">Hello *klienta* je hello domény klienta služby Azure AD (například contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="3f4c3-146">hello *tenant* is hello domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="3f4c3-147">Hello *clientId* je ID klienta hello hello aplikace, které jste zkopírovali z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-147">hello *clientId* is hello client ID of hello app, which you copied from hello portal.</span></span>
4. <span data-ttu-id="3f4c3-148">Teď musíte zpětného volání hello toodiscover identifikátor URI pro aplikace pro Windows Store.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-148">You now need toodiscover hello callback URI for your Windows Store app.</span></span> <span data-ttu-id="3f4c3-149">Na tomto řádku v hello zarážku `MainPage` metoda:</span><span class="sxs-lookup"><span data-stu-id="3f4c3-149">Set a breakpoint on this line in hello `MainPage` method:</span></span>
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. <span data-ttu-id="3f4c3-150">Vytvoření hello řešení, a ujistěte se, že se obnoví všechny odkazy na balíček.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-150">Build hello solution, making sure that all package references are restored.</span></span> <span data-ttu-id="3f4c3-151">Pokud chybí všechny balíčky, otevřete hello Správce balíčků NuGet a jejich obnovení.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-151">If any packages are missing, open hello NuGet Package Manager and restore them.</span></span>
6. <span data-ttu-id="3f4c3-152">Spuštění aplikace hello a zkopírujte hodnotu hello `redirectUri` při průchodu zarážek hello.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-152">Run hello app, and copy hello value of `redirectUri` when hello breakpoint is hit.</span></span> <span data-ttu-id="3f4c3-153">Hello hodnota by měla vypadat podobně jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="3f4c3-153">hello value should look something like hello following:</span></span>

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. <span data-ttu-id="3f4c3-154">Zpět na hello **nastavení** přidat na kartě aplikace hello v hello portál Azure, **RedirectUri** s hello předcházející hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-154">Back on hello **Settings** tab of hello app in hello Azure portal, add a **RedirectUri** with hello preceding value.</span></span>  

## <a name="step-3-use-adal-tooget-tokens-from-azure-ad"></a><span data-ttu-id="3f4c3-155">Krok 3: Použití ADAL tooget tokeny z Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f4c3-155">Step 3: Use ADAL tooget tokens from Azure AD</span></span>
<span data-ttu-id="3f4c3-156">Hello základní princip za ADAL je, že vždy, když aplikace hello je přístupový token, jednoduše volá `authContext.AcquireToken(…)`, a ADAL hello rest.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-156">hello basic principle behind ADAL is that whenever hello app needs an access token, it simply calls `authContext.AcquireToken(…)`, and ADAL does hello rest.</span></span>  

1. <span data-ttu-id="3f4c3-157">Inicializace aplikace hello `AuthenticationContext`, což je primární třídou hello adal.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-157">Initialize hello app’s `AuthenticationContext`, which is hello primary class of ADAL.</span></span> <span data-ttu-id="3f4c3-158">Tato akce předá ADAL hello souřadnice ho potřebuje toocommunicate s Azure AD a určit, jak toocache tokeny.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-158">This action passes ADAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

    ```C#
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. <span data-ttu-id="3f4c3-159">Vyhledejte hello `Search(...)` metodu, která je volána, když uživatelé kliknou na hello **vyhledávání** tlačítko na hello uživatelském rozhraní aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-159">Locate hello `Search(...)` method, which is invoked when users click hello **Search** button on hello app's UI.</span></span> <span data-ttu-id="3f4c3-160">Tato metoda vytváří tooquery toohello Azure AD Graph API požadavek get pro uživatele, jehož UPN začíná hello zadaný hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-160">This method makes a get request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span> <span data-ttu-id="3f4c3-161">tooquery hello rozhraní Graph API zahrnout přístupový token požadavku hello **autorizace** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-161">tooquery hello Graph API, include an access token in hello request's **Authorization** header.</span></span> <span data-ttu-id="3f4c3-162">Toto je, kde odeslán ADAL.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-162">This is where ADAL comes in.</span></span>

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
                ShowAuthError(string.Format("If hello error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    <span data-ttu-id="3f4c3-163">Když aplikace hello vyžaduje token voláním `AcquireTokenAsync(...)`, ADAL pokusí tooreturn token bez nutnosti hello uživatelské přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-163">When hello app requests a token by calling `AcquireTokenAsync(...)`, ADAL attempts tooreturn a token without asking hello user for credentials.</span></span> <span data-ttu-id="3f4c3-164">Pokud ADAL zjistí, že tento uživatel hello je toosign v tooget token, zobrazí přihlašovací dialogové okno, shromažďuje přihlašovací údaje uživatele hello a vrátí token po úspěšném provedení ověřování.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-164">If ADAL determines that hello user needs toosign in tooget a token, it displays a sign-in dialog box, collects hello user's credentials, and returns a token after authentication succeeds.</span></span> <span data-ttu-id="3f4c3-165">Pokud ADAL nelze tooreturn token z jakéhokoli důvodu, hello *AuthenticationResult* stav je k chybě.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-165">If ADAL is unable tooreturn a token for any reason, hello *AuthenticationResult* status is an error.</span></span>
3. <span data-ttu-id="3f4c3-166">Nyní je čas toouse hello přístupový token, který jste získali.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-166">Now it's time toouse hello access token you just acquired.</span></span> <span data-ttu-id="3f4c3-167">Také v hello `Search(...)` metoda, připojte hello tokenu toohello rozhraní Graph API získat žádost o v hello **autorizace** hlavičky:</span><span class="sxs-lookup"><span data-stu-id="3f4c3-167">Also in hello `Search(...)` method, attach hello token toohello Graph API get request in hello **Authorization** header:</span></span>

    ```C#
    // Add hello access token toohello Authorization header of hello call toohello Graph API, and call hello Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. <span data-ttu-id="3f4c3-168">Můžete použít hello `AuthenticationResult` objektu toodisplay informace o uživateli hello hello aplikace, jako je například ID uživatele hello:</span><span class="sxs-lookup"><span data-stu-id="3f4c3-168">You can use hello `AuthenticationResult` object toodisplay information about hello user in hello app, such as hello user's ID:</span></span>

    ```C#
    // Update hello page UI toorepresent hello signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. <span data-ttu-id="3f4c3-169">Můžete také použít ADAL toosign uživatelé mimo aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-169">You can also use ADAL toosign users out of hello app.</span></span> <span data-ttu-id="3f4c3-170">Když uživatel hello klikne hello **Odhlásit** tlačítko, ujistěte se, že další volání hello příliš`AcquireTokenAsync(...)` zobrazení přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-170">When hello user clicks hello **Sign Out** button, ensure that hello next call too`AcquireTokenAsync(...)` shows a sign-in view.</span></span> <span data-ttu-id="3f4c3-171">Pomocí knihovny ADAL tato akce je stejně snadná jako vymazání mezipamětí tokenů hello:</span><span class="sxs-lookup"><span data-stu-id="3f4c3-171">With ADAL, this action is as easy as clearing hello token cache:</span></span>

    ```C#
    private void SignOut()
    {
        // Clear session state from hello token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a><span data-ttu-id="3f4c3-172">Kam dál</span><span class="sxs-lookup"><span data-stu-id="3f4c3-172">What's next</span></span>
<span data-ttu-id="3f4c3-173">Teď máte funkční aplikace pro Windows Store, můžete ověřovat uživatele, bezpečně volání webových rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-173">You now have a working Windows Store app that can authenticate users, securely call web APIs using OAuth 2.0, and get basic information about hello user.</span></span>

<span data-ttu-id="3f4c3-174">Pokud již nejsou naplněny vašeho klienta s uživateli, teď proto je toodo čas hello.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-174">If you haven’t already populated your tenant with users, now is hello time toodo so.</span></span>
1. <span data-ttu-id="3f4c3-175">Spuštění aplikace DirectorySearcher a pak se přihlaste pomocí jeden z uživatelů hello.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-175">Run your DirectorySearcher app, and then sign in with one of hello users.</span></span>
2. <span data-ttu-id="3f4c3-176">Hledání jiných uživatelů podle jejich UPN.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-176">Search for other users based on their UPN.</span></span>
3. <span data-ttu-id="3f4c3-177">Zavření aplikace hello a znovu jej spusťte.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-177">Close hello app, and rerun it.</span></span> <span data-ttu-id="3f4c3-178">Všimněte si, jak hello uživatelské relace zůstává beze změn.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-178">Notice how hello user’s session remains intact.</span></span>
4. <span data-ttu-id="3f4c3-179">Odhlásit se kliknutím pravým tlačítkem na toodisplay hello dolním panelu a pak se přihlaste zpět v jako jiný uživatel.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-179">Sign out by right-clicking toodisplay hello bottom bar, and then sign back in as another user.</span></span>

<span data-ttu-id="3f4c3-180">ADAL umožňuje snadno tooincorporate všechny tyto běžné funkce identity do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-180">ADAL makes it easy tooincorporate all these common identity features into hello app.</span></span> <span data-ttu-id="3f4c3-181">Se postará všechny pracovní dirty hello, jako je například Správa mezipaměti podpora protokolu OAuth, prezentací hello uživatele s přihlašovacími údaji uživatelského rozhraní, a aktualizovat platnost tokenů.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-181">It takes care of all hello dirty work for you, such as cache management, OAuth protocol support, presenting hello user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="3f4c3-182">Je třeba volání tooknow pouze jediného rozhraní API `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-182">You need tooknow only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="3f4c3-183">Odkaz, stáhněte si hello [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (bez vašich hodnot nastavení).</span><span class="sxs-lookup"><span data-stu-id="3f4c3-183">For reference, download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="3f4c3-184">Nyní se můžete přesunout na tooadditional identity scénáře.</span><span class="sxs-lookup"><span data-stu-id="3f4c3-184">You can now move on tooadditional identity scenarios.</span></span> <span data-ttu-id="3f4c3-185">Zkuste například [zabezpečení webového rozhraní API .NET s Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="3f4c3-185">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
