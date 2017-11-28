---
title: "aaaAzure AD AngularJS Začínáme | Microsoft Docs"
description: "Jak toobuild jednostránková aplikace AngularJS, se integruje se službou Azure AD pro přihlášení a zavolá rozhraní API Azure AD chráněné pomocí OAuth."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: f2991054-8146-4718-a5f7-59b892230ad7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: eca5e1c9662186dfae4f96ca3041f9350583cf79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="help-secure-angularjs-single-page-apps-by-using-azure-ad"></a><span data-ttu-id="fccf4-103">Pomoc se zabezpečením AngularJS jednostránkové aplikace pomocí Azure AD</span><span class="sxs-lookup"><span data-stu-id="fccf4-103">Help secure AngularJS single-page apps by using Azure AD</span></span>

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="fccf4-104">Azure Active Directory (Azure AD) umožňuje jednoduchá a přímočará pro tooadd přihlášení, odhlášení a volání rozhraní API zabezpečené OAuth tooyour jednostránkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fccf4-104">Azure Active Directory (Azure AD) makes it simple and straightforward for you tooadd sign-in, sign-out, and secure OAuth API calls tooyour single-page apps.</span></span>  <span data-ttu-id="fccf4-105">To umožňuje uživatelům tooauthenticate aplikace pomocí jejich účtů systému Windows Server Active Directory a využívat žádné webové rozhraní API, které Azure AD pomáhá chránit, jako je například hello rozhraní API Office 365 nebo hello rozhraní API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="fccf4-105">It enables your apps tooauthenticate users with their Windows Server Active Directory accounts and consume any web API that Azure AD helps protect, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="fccf4-106">Pro aplikace JavaScript spuštěnému v prohlížeči, Azure AD poskytuje hello Active Directory Authentication Library (ADAL), nebo adal.js.</span><span class="sxs-lookup"><span data-stu-id="fccf4-106">For JavaScript applications running in a browser, Azure AD provides hello Active Directory Authentication Library (ADAL), or adal.js.</span></span> <span data-ttu-id="fccf4-107">jediný účel adal.js Hello je toomake ho snadno pro vaše aplikace tooget přístupových tokenů.</span><span class="sxs-lookup"><span data-stu-id="fccf4-107">hello sole purpose of adal.js is toomake it easy for your app tooget access tokens.</span></span> <span data-ttu-id="fccf4-108">toodemonstrate, jak lze snadno, je, zde jsme budete vytvářet AngularJS tooDo aplikaci seznamu který:</span><span class="sxs-lookup"><span data-stu-id="fccf4-108">toodemonstrate just how easy it is, here we'll build an AngularJS tooDo List application that:</span></span>

* <span data-ttu-id="fccf4-109">Hello uživatel přihlásí v aplikaci toohello pomocí Azure AD jako zprostředkovatele identity hello.</span><span class="sxs-lookup"><span data-stu-id="fccf4-109">Signs hello user in toohello app by using Azure AD as hello identity provider.</span></span>

* <span data-ttu-id="fccf4-110">Zobrazí některé informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="fccf4-110">Displays some information about hello user.</span></span>
* <span data-ttu-id="fccf4-111">Volání bezpečně hello tooDo aplikace API seznamu pomocí nosných tokenů z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fccf4-111">Securely calls hello app's tooDo List API by using bearer tokens from Azure AD.</span></span>
* <span data-ttu-id="fccf4-112">Přihlásí hello uživatele mimo aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="fccf4-112">Signs hello user out of hello app.</span></span>

<span data-ttu-id="fccf4-113">toobuild hello kompletní, funkční aplikaci, musíte:</span><span class="sxs-lookup"><span data-stu-id="fccf4-113">toobuild hello complete, working application, you need to:</span></span>

1. <span data-ttu-id="fccf4-114">Registrovat aplikaci s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fccf4-114">Register your app with Azure AD.</span></span>
2. <span data-ttu-id="fccf4-115">ADAL nainstalujte a nakonfigurujte hello jednostránkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fccf4-115">Install ADAL and configure hello single-page app.</span></span>
3. <span data-ttu-id="fccf4-116">Použití ADAL toohelp zabezpečené stránky v aplikaci jednostránkové hello.</span><span class="sxs-lookup"><span data-stu-id="fccf4-116">Use ADAL toohelp secure pages in hello single-page app.</span></span>

<span data-ttu-id="fccf4-117">spuštění, tooget [stáhnout kostru aplikace hello](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="fccf4-117">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="fccf4-118">Musíte taky klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fccf4-118">You also need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="fccf4-119">Pokud ještě nemáte klienta, [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="fccf4-119">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-register-hello-directorysearcher-application"></a><span data-ttu-id="fccf4-120">Krok 1: Registrace aplikace DirectorySearcher hello</span><span class="sxs-lookup"><span data-stu-id="fccf4-120">Step 1: Register hello DirectorySearcher application</span></span>
<span data-ttu-id="fccf4-121">tooenable tooauthenticate uživatelů aplikace a get tokeny, musíte nejprve tooregister ji ve službě Azure AD klienta:</span><span class="sxs-lookup"><span data-stu-id="fccf4-121">tooenable your app tooauthenticate users and get tokens, you first need tooregister it in your Azure AD tenant:</span></span>

1. <span data-ttu-id="fccf4-122">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fccf4-122">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fccf4-123">Pokud jste přihlášeni toomultiple adresáře, musíte tooensure prohlížíte hello správném adresáři.</span><span class="sxs-lookup"><span data-stu-id="fccf4-123">If you are signed in toomultiple directories, you may need tooensure you are viewing hello correct directory.</span></span> <span data-ttu-id="fccf4-124">toodo tedy na horním panelu hello, klikněte na váš účet.</span><span class="sxs-lookup"><span data-stu-id="fccf4-124">toodo so, on hello top bar, click your account.</span></span> <span data-ttu-id="fccf4-125">V části hello **Directory** vyberte místo, kam chcete tooregister klienta hello Azure AD vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fccf4-125">Under hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="fccf4-126">Klikněte na tlačítko **více služeb** v levém podokně text hello a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="fccf4-126">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="fccf4-127">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="fccf4-127">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="fccf4-128">Postupujte podle pokynů hello a vytvořit novou webovou aplikaci nebo webové rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="fccf4-128">Follow hello prompts and create a new web application and/or web API:</span></span>
  * <span data-ttu-id="fccf4-129">**Název** popisuje toousers vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fccf4-129">**Name** describes your application toousers.</span></span>
  * <span data-ttu-id="fccf4-130">**Identifikátor Uri pro přesměrování** je hello umístění toowhich Azure AD vrátí tokeny.</span><span class="sxs-lookup"><span data-stu-id="fccf4-130">**Redirect Uri** is hello location toowhich Azure AD will return tokens.</span></span> <span data-ttu-id="fccf4-131">Hello výchozí umístění pro tato ukázka je `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="fccf4-131">hello default location for this sample is `https://localhost:44326/`.</span></span>
6. <span data-ttu-id="fccf4-132">Po dokončení registrace přiřadí Azure AD aplikace jedinečné ID tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="fccf4-132">After you finish registration, Azure AD assigns a unique application ID tooyour app.</span></span>  <span data-ttu-id="fccf4-133">Tuto hodnotu budete potřebovat v dalších částech hello, takže zkopírujte jej z karty aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fccf4-133">You'll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="fccf4-134">Adal.js používá toocommunicate implicitního toku OAuth hello s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fccf4-134">Adal.js uses hello OAuth implicit flow toocommunicate with Azure AD.</span></span> <span data-ttu-id="fccf4-135">Implicitní tok hello musíte povolit pro aplikace:</span><span class="sxs-lookup"><span data-stu-id="fccf4-135">You must enable hello implicit flow for your application:</span></span>
  1. <span data-ttu-id="fccf4-136">Klikněte na tlačítko aplikace hello a vyberte **Manifest** tooopen hello vložené manifestu editor.</span><span class="sxs-lookup"><span data-stu-id="fccf4-136">Click hello application and select **Manifest** tooopen hello inline manifest editor.</span></span>
  2. <span data-ttu-id="fccf4-137">Vyhledejte hello `oauth2AllowImplicitFlow` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="fccf4-137">Locate hello `oauth2AllowImplicitFlow` property.</span></span> <span data-ttu-id="fccf4-138">Jeho hodnotu nastavte příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="fccf4-138">Set its value too`true`.</span></span>
  3. <span data-ttu-id="fccf4-139">Klikněte na tlačítko **Uložit** toosave hello manifestu.</span><span class="sxs-lookup"><span data-stu-id="fccf4-139">Click **Save** toosave hello manifest.</span></span>
8. <span data-ttu-id="fccf4-140">Udělení oprávnění vašeho klienta pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fccf4-140">Grant permissions across your tenant for your application.</span></span> <span data-ttu-id="fccf4-141">Přejděte příliš**nastavení** > **vlastnosti** > **požadovaných oprávnění**a klikněte na tlačítko hello **udělit oprávnění**tlačítko na horním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="fccf4-141">Go too**Settings** > **Properties** > **Required Permissions**, and click hello **Grant Permissions** button on hello top bar.</span></span> <span data-ttu-id="fccf4-142">Klikněte na tlačítko **Ano** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="fccf4-142">Click **Yes** tooconfirm.</span></span>

## <a name="step-2-install-adal-and-configure-hello-single-page-app"></a><span data-ttu-id="fccf4-143">Krok 2: Instalace ADAL a konfigurace hello jednostránkové aplikace</span><span class="sxs-lookup"><span data-stu-id="fccf4-143">Step 2: Install ADAL and configure hello single-page app</span></span>
<span data-ttu-id="fccf4-144">Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat adal.js a zadejte kód, týkající se identity.</span><span class="sxs-lookup"><span data-stu-id="fccf4-144">Now that you have an application in Azure AD, you can install adal.js and write your identity-related code.</span></span>

### <a name="configure-hello-javascript-client"></a><span data-ttu-id="fccf4-145">Konfigurace klienta JavaScript hello</span><span class="sxs-lookup"><span data-stu-id="fccf4-145">Configure hello JavaScript client</span></span>
<span data-ttu-id="fccf4-146">Začněte tím, že přidání adal.js toohello TodoSPA projektu pomocí hello Konzola správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="fccf4-146">Begin by adding adal.js toohello TodoSPA project by using hello Package Manager Console:</span></span>
  1. <span data-ttu-id="fccf4-147">Stáhněte si [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) a přidejte ji toohello `App/Scripts/` adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="fccf4-147">Download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) and add it toohello `App/Scripts/` project directory.</span></span>
  2. <span data-ttu-id="fccf4-148">Stáhněte si [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) a přidejte ji toohello `App/Scripts/` adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="fccf4-148">Download [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) and add it toohello `App/Scripts/` project directory.</span></span>
  3. <span data-ttu-id="fccf4-149">Načíst každý skript hello konce hello `</body>` v `index.html`:</span><span class="sxs-lookup"><span data-stu-id="fccf4-149">Load each script before hello end of hello `</body>` in `index.html`:</span></span>

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-hello-back-end-server"></a><span data-ttu-id="fccf4-150">Konfigurace back-end server hello</span><span class="sxs-lookup"><span data-stu-id="fccf4-150">Configure hello back end server</span></span>
<span data-ttu-id="fccf4-151">Hello back-end pro hello jednostránkové aplikace tooDo back endové rozhraní API seznamu tooaccept tokeny z prohlížeče hello, musí konfigurace informace o registraci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fccf4-151">For hello single-page app's back-end tooDo List API tooaccept tokens from hello browser, hello back end needs configuration information about hello app registration.</span></span> <span data-ttu-id="fccf4-152">V projektu TodoSPA hello otevřete `web.config`.</span><span class="sxs-lookup"><span data-stu-id="fccf4-152">In hello TodoSPA project, open `web.config`.</span></span> <span data-ttu-id="fccf4-153">Nahraďte hodnoty hello hello elementů v hello `<appSettings>` hello části tooreflect hello hodnoty, které jste použili v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fccf4-153">Replace hello values of hello elements in hello `<appSettings>` section tooreflect hello values that you used in hello Azure portal.</span></span> <span data-ttu-id="fccf4-154">Váš kód bude odkazovat tyto hodnoty vždy, když ho využívá ADAL.</span><span class="sxs-lookup"><span data-stu-id="fccf4-154">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="fccf4-155">`ida:Tenant`je hello domény klienta služby Azure AD – například contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="fccf4-155">`ida:Tenant` is hello domain of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="fccf4-156">`ida:Audience`je hello ID klienta aplikace, který jste zkopírovali z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="fccf4-156">`ida:Audience` is hello client ID of your application that you copied from hello portal.</span></span>

## <a name="step-3-use-adal-toohelp-secure-pages-in-hello-single-page-app"></a><span data-ttu-id="fccf4-157">Krok 3: Použití ADAL toohelp zabezpečené stránky v hello jednostránkové aplikace</span><span class="sxs-lookup"><span data-stu-id="fccf4-157">Step 3: Use ADAL toohelp secure pages in hello single-page app</span></span>
<span data-ttu-id="fccf4-158">Adal.js se integruje s AngularJS trasy a zprostředkovatele protokolu HTTP, můžete pomoct zabezpečené jednotlivých zobrazení v jednostránkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fccf4-158">Adal.js integrates with AngularJS route and HTTP providers, so you can help secure individual views in your single-page app.</span></span>

1. <span data-ttu-id="fccf4-159">V `App/Scripts/app.js`, převeďte v modulu adal.js hello:</span><span class="sxs-lookup"><span data-stu-id="fccf4-159">In `App/Scripts/app.js`, bring in hello adal.js module:</span></span>

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. <span data-ttu-id="fccf4-160">Inicializace `adalProvider` také pomocí hodnoty konfigurace hello registrace vaší aplikace v `App/Scripts/app.js`:</span><span class="sxs-lookup"><span data-stu-id="fccf4-160">Initialize `adalProvider` by using hello configuration values of your application registration, also in `App/Scripts/app.js`:</span></span>

    ```js
    adalProvider.init(
      {
          instance: 'https://login.microsoftonline.com/',
          tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
          clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
          extraQueryParameter: 'nux=1',
          //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
      },
      $httpProvider
    );
    ```
3. <span data-ttu-id="fccf4-161">Pomůže zabezpečené hello `TodoList` zobrazení v hello aplikace pomocí pouze jeden řádek kódu: `requireADLogin`.</span><span class="sxs-lookup"><span data-stu-id="fccf4-161">Help secure hello `TodoList` view in hello app by using only one line of code: `requireADLogin`.</span></span>

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a><span data-ttu-id="fccf4-162">Souhrn</span><span class="sxs-lookup"><span data-stu-id="fccf4-162">Summary</span></span>
<span data-ttu-id="fccf4-163">Nyní máte zabezpečené aplikaci jednostránkové, která může přihlásit uživatele a vydat chráněný token nosiče požadavky tooits back endové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="fccf4-163">You now have a secure single-page app that can sign in users and issue bearer-token-protected requests tooits back-end API.</span></span> <span data-ttu-id="fccf4-164">Když uživatel klikne hello **TodoList** odkaz, adal.js se automaticky přesměruje tooAzure AD pro přihlášení v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="fccf4-164">When a user clicks hello **TodoList** link, adal.js will automatically redirect tooAzure AD for sign-in if necessary.</span></span> <span data-ttu-id="fccf4-165">Kromě toho adal.js automaticky připojí přístupu k tokenu tooany, které požadavky Ajax odeslaný toohello aplikace back-end.</span><span class="sxs-lookup"><span data-stu-id="fccf4-165">In addition, adal.js will automatically attach an access token tooany Ajax requests that are sent toohello app's back end.</span></span>  

<span data-ttu-id="fccf4-166">Hello předchozí kroky jsou hello úplné minimální potřebné toobuild jednostránkové aplikace s použitím adal.js.</span><span class="sxs-lookup"><span data-stu-id="fccf4-166">hello preceding steps are hello bare minimum necessary toobuild a single-page app by using adal.js.</span></span> <span data-ttu-id="fccf4-167">Ale několik dalších funkcí jsou užitečné při jednostránkové aplikace:</span><span class="sxs-lookup"><span data-stu-id="fccf4-167">But a few other features are useful in single-page app:</span></span>

* <span data-ttu-id="fccf4-168">tooexplicitly zasílání požadavků na přihlášení a odhlášení, funkce můžete definovat v řadičích, které vyvolají adal.js.</span><span class="sxs-lookup"><span data-stu-id="fccf4-168">tooexplicitly issue sign-in and sign-out requests, you can define functions in your controllers that invoke adal.js.</span></span>  <span data-ttu-id="fccf4-169">V `App/Scripts/homeCtrl.js`:</span><span class="sxs-lookup"><span data-stu-id="fccf4-169">In `App/Scripts/homeCtrl.js`:</span></span>

    ```js
    ...
    $scope.login = function () {
        adalService.login();
    };
    $scope.logout = function () {
        adalService.logOut();
    };
    ...
    ```
* <span data-ttu-id="fccf4-170">Můžete chtít toopresent informace o uživateli v uživatelském rozhraní aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fccf4-170">You might want toopresent user information in hello app's UI.</span></span> <span data-ttu-id="fccf4-171">Hello ADAL služby již byla přidána toohello `userDataCtrl` řadič, takže přistupujete hello `userInfo` objekt v hello přidružené zobrazení, `App/Views/UserData.html`:</span><span class="sxs-lookup"><span data-stu-id="fccf4-171">hello ADAL service has already been added toohello `userDataCtrl` controller, so you can access hello `userInfo` object in hello associated view, `App/Views/UserData.html`:</span></span>

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* <span data-ttu-id="fccf4-172">Existuje mnoho scénářů, ve kterých budete muset tooknow Pokud hello uživatel je přihlášený nebo ne.</span><span class="sxs-lookup"><span data-stu-id="fccf4-172">There are many scenarios in which you'll want tooknow if hello user is signed in or not.</span></span> <span data-ttu-id="fccf4-173">Můžete taky hello `userInfo` objektu toogather tyto informace.</span><span class="sxs-lookup"><span data-stu-id="fccf4-173">You can also use hello `userInfo` object toogather this information.</span></span>  <span data-ttu-id="fccf4-174">Například v `index.html`, můžete zobrazit buď hello **přihlášení** nebo **odhlášení** tlačítko na základě ověření stavu:</span><span class="sxs-lookup"><span data-stu-id="fccf4-174">For instance, in `index.html`, you can show either hello **Login** or **Logout** button based on authentication status:</span></span>

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

<span data-ttu-id="fccf4-175">Jednostránkové aplikace Azure integrované s Active Directory můžete ověřovat uživatele, bezpečně volat jeho back-end pomocí standardu OAuth 2.0 a získat základní informace o uživateli hello.</span><span class="sxs-lookup"><span data-stu-id="fccf4-175">Your Azure AD-integrated single-page app can authenticate users, securely call its back end by using OAuth 2.0, and get basic information about hello user.</span></span> <span data-ttu-id="fccf4-176">Pokud jste to ještě neudělali, nyní je čas toopopulate hello vašeho klienta s některými uživateli.</span><span class="sxs-lookup"><span data-stu-id="fccf4-176">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span> <span data-ttu-id="fccf4-177">Spuštění tooDo seznamu jednostránkové aplikace a přihlaste se pomocí jeden z těchto uživatelů.</span><span class="sxs-lookup"><span data-stu-id="fccf4-177">Run your tooDo List single-page app, and sign in with one of those users.</span></span> <span data-ttu-id="fccf4-178">Přidejte uživatele toohello úlohy na seznam úkolů, odhlásit se a přihlaste se zpět.</span><span class="sxs-lookup"><span data-stu-id="fccf4-178">Add tasks toohello user's to-do list, sign out, and sign back in.</span></span>

<span data-ttu-id="fccf4-179">Adal.js umožňuje snadno tooincorporate běžné funkce identity do aplikace.</span><span class="sxs-lookup"><span data-stu-id="fccf4-179">Adal.js makes it easy tooincorporate common identity features into your application.</span></span> <span data-ttu-id="fccf4-180">Se stará o všechny pracovní dirty hello za vás: Správa mezipaměti, podpora protokolu OAuth, prezentuje hello uživatele přihlásit uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.</span><span class="sxs-lookup"><span data-stu-id="fccf4-180">It takes care of all hello dirty work for you: cache management, OAuth protocol support, presenting hello user with a sign-in UI, refreshing expired tokens, and more.</span></span>

<span data-ttu-id="fccf4-181">Pro srovnání je k dispozici v hello dokončit ukázka (bez vašich hodnot nastavení) [Githubu](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="fccf4-181">For reference, hello completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fccf4-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fccf4-182">Next steps</span></span>
<span data-ttu-id="fccf4-183">Nyní se můžete přesunout na tooadditional scénáře.</span><span class="sxs-lookup"><span data-stu-id="fccf4-183">You can now move on tooadditional scenarios.</span></span> <span data-ttu-id="fccf4-184">Můžete chtít tootry: [volání CORS webového rozhraní API z jednostránkové aplikace](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).</span><span class="sxs-lookup"><span data-stu-id="fccf4-184">You might want tootry: [Call a CORS web API from a single-page app](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
