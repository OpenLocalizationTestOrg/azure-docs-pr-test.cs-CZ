---
title: aaaAzure AD B2C | Microsoft Docs
description: "Jak toobuild webového rozhraní API .NET pomocí Azure Active Directory B2C zabezpečené, pomocí přístupových tokenů OAuth 2.0 pro ověřování."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: d45364216deda38ef44b60dd11e86d9a089ad509
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a><span data-ttu-id="21c5d-103">Azure Active Directory B2C: Sestavení webového rozhraní API .NET</span><span class="sxs-lookup"><span data-stu-id="21c5d-103">Azure Active Directory B2C: Build a .NET web API</span></span>

<span data-ttu-id="21c5d-104">S Azure Active Directory (Azure AD) B2C můžete zabezpečit webové rozhraní API pomocí přístupových tokenů OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="21c5d-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="21c5d-105">Tyto tokeny umožňují vašeho klienta toohello tooauthenticate aplikace API.</span><span class="sxs-lookup"><span data-stu-id="21c5d-105">These tokens allow your client apps tooauthenticate toohello API.</span></span> <span data-ttu-id="21c5d-106">Tento článek ukazuje, jak toocreate rozhraní API .NET MVC "seznam úkolů", umožňuje uživatelům vašeho klienta úlohy tooCRUD aplikace.</span><span class="sxs-lookup"><span data-stu-id="21c5d-106">This article shows you how toocreate a .NET MVC "to-do list" API that allows users of your client application tooCRUD tasks.</span></span> <span data-ttu-id="21c5d-107">Hello webového rozhraní API zabezpečené pomocí Azure AD B2C a pouze umožňuje ověřeným uživatelům toomanage jejich seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="21c5d-107">hello web API is secured using Azure AD B2C and only allows authenticated users toomanage their to-do list.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="21c5d-108">Vytvoření adresáře Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="21c5d-108">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="21c5d-109">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="21c5d-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="21c5d-110">Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="21c5d-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="21c5d-111">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="21c5d-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

> [!NOTE]
> <span data-ttu-id="21c5d-112">Hello klientská aplikace a webové rozhraní API musí používat adresář hello stejnou službou Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="21c5d-112">hello client application and web API must use hello same Azure AD B2C directory.</span></span>
>

## <a name="create-a-web-api"></a><span data-ttu-id="21c5d-113">Vytvoření webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="21c5d-113">Create a web API</span></span>

<span data-ttu-id="21c5d-114">Dále musíte ve svém adresáři B2C toocreate webové aplikace API.</span><span class="sxs-lookup"><span data-stu-id="21c5d-114">Next, you need toocreate a web API app in your B2C directory.</span></span> <span data-ttu-id="21c5d-115">Díky tomu získá informace o Azure AD, se vyžaduje toosecurely komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="21c5d-115">This gives Azure AD information that it needs toosecurely communicate with your app.</span></span> <span data-ttu-id="21c5d-116">toocreate na aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="21c5d-116">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="21c5d-117">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="21c5d-117">Be sure to:</span></span>

* <span data-ttu-id="21c5d-118">Zahrnout **webové aplikace** nebo **webové rozhraní API** v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="21c5d-118">Include a **web app** or **web API** in hello application.</span></span>
* <span data-ttu-id="21c5d-119">Použití hello **identifikátor URI pro přesměrování** `https://localhost:44332/` pro hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="21c5d-119">Use hello **Redirect URI** `https://localhost:44332/` for hello web app.</span></span> <span data-ttu-id="21c5d-120">Toto je výchozí umístění hello hello klienta webové aplikace pro tuto ukázku kódu.</span><span class="sxs-lookup"><span data-stu-id="21c5d-120">This is hello default location of hello web app client for this code sample.</span></span>
* <span data-ttu-id="21c5d-121">Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="21c5d-121">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="21c5d-122">Budete ho potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="21c5d-122">You'll need it later.</span></span>
* <span data-ttu-id="21c5d-123">Zadejte identifikátor aplikace do **Identifikátor URI ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="21c5d-123">Enter an app identifier into **App ID URI**.</span></span>
* <span data-ttu-id="21c5d-124">Přidání oprávnění prostřednictvím hello **publikovaná obory** nabídky.</span><span class="sxs-lookup"><span data-stu-id="21c5d-124">Add permissions through hello **Published scopes** menu.</span></span>

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="21c5d-125">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="21c5d-125">Create your policies</span></span>

<span data-ttu-id="21c5d-126">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="21c5d-126">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="21c5d-127">Budete potřebovat toocreate toocommunicate zásady s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="21c5d-127">You will need toocreate a policy toocommunicate with Azure AD B2C.</span></span> <span data-ttu-id="21c5d-128">Doporučujeme, abyste pomocí hello kombinaci registrace-množství nebo zásad přihlašování, jak je popsáno v hello [článku o zásadách](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="21c5d-128">We recommend using hello combined sign-up/sign-in policy, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="21c5d-129">Při vytváření zásady nezapomeňte na následující:</span><span class="sxs-lookup"><span data-stu-id="21c5d-129">When you create your policy, be sure to:</span></span>

* <span data-ttu-id="21c5d-130">Zvolit **Zobrazovaný název** a další atributy registrace ve svojí zásadě.</span><span class="sxs-lookup"><span data-stu-id="21c5d-130">Choose **Display name** and other sign-up attributes in your policy.</span></span>
* <span data-ttu-id="21c5d-131">Zvolit **Zobrazovaný název** a deklarace identity **ID objektu** jako deklarace identity aplikace v každé zásadě.</span><span class="sxs-lookup"><span data-stu-id="21c5d-131">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="21c5d-132">Můžete zvolit i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="21c5d-132">You can choose other claims as well.</span></span>
* <span data-ttu-id="21c5d-133">Kopírování hello **název** po jejím vytvoření každé zásady.</span><span class="sxs-lookup"><span data-stu-id="21c5d-133">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="21c5d-134">Název zásady hello budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="21c5d-134">You'll need hello policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="21c5d-135">Po úspěšném vytvoření zásad hello jste připravené toobuild vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="21c5d-135">After you have successfully created hello policy, you're ready toobuild your app.</span></span>

## <a name="download-hello-code"></a><span data-ttu-id="21c5d-136">Stáhněte si kód hello</span><span class="sxs-lookup"><span data-stu-id="21c5d-136">Download hello code</span></span>

<span data-ttu-id="21c5d-137">Hello kódu pro účely tohoto kurzu je udržovaný na [Githubu](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="21c5d-137">hello code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="21c5d-138">Ukázka hello může klonovat spuštěním:</span><span class="sxs-lookup"><span data-stu-id="21c5d-138">You can clone hello sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="21c5d-139">Po stažení ukázkového kódu hello hello otevřete Visual Studio .sln souboru tooget spustit.</span><span class="sxs-lookup"><span data-stu-id="21c5d-139">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="21c5d-140">Hello soubor řešení obsahuje dva projekty: `TaskWebApp` a `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="21c5d-140">hello solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="21c5d-141">`TaskWebApp`je webová aplikace MVC, která hello uživatel komunikuje se službou.</span><span class="sxs-lookup"><span data-stu-id="21c5d-141">`TaskWebApp` is an MVC web application that hello user interacts with.</span></span> <span data-ttu-id="21c5d-142">`TaskService`je back endové webové rozhraní API hello aplikace, které ukládá seznam úkolů každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="21c5d-142">`TaskService` is hello app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="21c5d-143">V tomto článku pouze zabývat hello `TaskService` aplikace.</span><span class="sxs-lookup"><span data-stu-id="21c5d-143">This article will only discuss hello `TaskService` application.</span></span> <span data-ttu-id="21c5d-144">toolearn jak toobuild `TaskWebApp` pomocí Azure AD B2C, najdete v tématu [naše kurz .NET pro aplikace webových](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="21c5d-144">toolearn how toobuild `TaskWebApp` using Azure AD B2C, see [our .NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span>

### <a name="update-hello-azure-ad-b2c-configuration"></a><span data-ttu-id="21c5d-145">Aktualizace konfigurace hello Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="21c5d-145">Update hello Azure AD B2C configuration</span></span>

<span data-ttu-id="21c5d-146">Naše ukázka je ID zásad a klienta hello nakonfigurované toouse naše ukázka klienta.</span><span class="sxs-lookup"><span data-stu-id="21c5d-146">Our sample is configured toouse hello policies and client ID of our demo tenant.</span></span> <span data-ttu-id="21c5d-147">Pokud chcete toouse vlastního klienta, budete potřebovat toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="21c5d-147">If you would like toouse your own tenant, you will need toodo hello following:</span></span>

1. <span data-ttu-id="21c5d-148">Otevřete `web.config` v hello `TaskService` projektu a nahraďte hodnoty hello</span><span class="sxs-lookup"><span data-stu-id="21c5d-148">Open `web.config` in hello `TaskService` project and replace hello values for</span></span>
    * <span data-ttu-id="21c5d-149">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="21c5d-149">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="21c5d-150">`ida:ClientId` identifikátorem aplikace webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="21c5d-150">`ida:ClientId` with your web API application ID</span></span>
    * <span data-ttu-id="21c5d-151">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="21c5d-151">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="21c5d-152">Otevřete `web.config` v hello `TaskWebApp` projektu a nahraďte hodnoty hello</span><span class="sxs-lookup"><span data-stu-id="21c5d-152">Open `web.config` in hello `TaskWebApp` project and replace hello values for</span></span>
    * <span data-ttu-id="21c5d-153">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="21c5d-153">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="21c5d-154">`ida:ClientId` identifikátorem webového aplikace</span><span class="sxs-lookup"><span data-stu-id="21c5d-154">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="21c5d-155">`ida:ClientSecret` tajným klíčem webové aplikace</span><span class="sxs-lookup"><span data-stu-id="21c5d-155">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="21c5d-156">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="21c5d-156">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="21c5d-157">`ida:EditProfilePolicyId` názvem zásady pro úpravu profilu</span><span class="sxs-lookup"><span data-stu-id="21c5d-157">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="21c5d-158">`ida:ResetPasswordPolicyId` názvem zásady pro resetování hesla</span><span class="sxs-lookup"><span data-stu-id="21c5d-158">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>


## <a name="secure-hello-api"></a><span data-ttu-id="21c5d-159">Zabezpečení rozhraní API hello</span><span class="sxs-lookup"><span data-stu-id="21c5d-159">Secure hello API</span></span>

<span data-ttu-id="21c5d-160">Pokud máte klienta, který volá vaše rozhraní API, můžete rozhraní API (například `TaskService`) zabezpečit pomocí nosných tokenů OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="21c5d-160">When you have a client that calls your API, you can secure your API (e.g `TaskService`) by using OAuth 2.0 bearer tokens.</span></span> <span data-ttu-id="21c5d-161">To zajistí, že každé rozhraní API tooyour požadavek bude pouze platný, pokud hello žádost obsahuje token nosiče.</span><span class="sxs-lookup"><span data-stu-id="21c5d-161">This ensures that each request tooyour API will only be valid if hello request has a bearer token.</span></span> <span data-ttu-id="21c5d-162">Vaše rozhraní API může přijímat a ověřovat nosné tokeny pomocí knihovny Microsoft's Open Web Interface pro .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="21c5d-162">Your API can accept and validate bearer tokens by using Microsoft's Open Web Interface for .NET (OWIN) library.</span></span>

### <a name="install-owin"></a><span data-ttu-id="21c5d-163">Instalace OWIN</span><span class="sxs-lookup"><span data-stu-id="21c5d-163">Install OWIN</span></span>

<span data-ttu-id="21c5d-164">Začněte instalací ověřovacího kanálu OWIN OAuth hello pomocí hello Konzola správce balíčků Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="21c5d-164">Begin by installing hello OWIN OAuth authentication pipeline by using hello Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

<span data-ttu-id="21c5d-165">Nainstaluje hello middleware OWIN, který bude přijmout a ověřit nosné tokeny.</span><span class="sxs-lookup"><span data-stu-id="21c5d-165">This will install hello OWIN middleware that will accept and validate bearer tokens.</span></span>

### <a name="add-an-owin-startup-class"></a><span data-ttu-id="21c5d-166">Přidání třídy pro spuštění OWIN</span><span class="sxs-lookup"><span data-stu-id="21c5d-166">Add an OWIN startup class</span></span>

<span data-ttu-id="21c5d-167">Přidání třídy toohello spuštění OWIN rozhraní API volat `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="21c5d-167">Add an OWIN startup class toohello API called `Startup.cs`.</span></span>  <span data-ttu-id="21c5d-168">Klikněte pravým tlačítkem na projekt hello, vyberte **přidat** a **nová položka**a poté vyhledejte OWIN.</span><span class="sxs-lookup"><span data-stu-id="21c5d-168">Right-click on hello project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="21c5d-169">Hello OWIN middleware vyvolá hello `Configuration(…)` metoda při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="21c5d-169">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="21c5d-170">V našem ukázce jsme příliš změnili deklaraci třídy hello`public partial class Startup` a implementovat hello jiných součástí třídy hello v `App_Start\Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="21c5d-170">In our sample, we changed hello class declaration too`public partial class Startup` and implemented hello other part of hello class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="21c5d-171">Uvnitř hello `Configuration` metoda, jsme přidali volání příliš`ConfigureAuth`, která je definována v `Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="21c5d-171">Inside hello `Configuration` method, we added a call too`ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="21c5d-172">Po hello úpravy `Startup.cs` vypadá jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="21c5d-172">After hello modifications, `Startup.cs` looks like hello following:</span></span>

```CSharp
// Startup.cs

public partial class Startup
{
    // hello OWIN middleware will invoke this method when hello app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of hello class
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a><span data-ttu-id="21c5d-173">Konfigurace ověřování OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="21c5d-173">Configure OAuth 2.0 authentication</span></span>

<span data-ttu-id="21c5d-174">Soubor otevřete hello `App_Start\Startup.Auth.cs`a implementovat hello `ConfigureAuth(...)` metoda.</span><span class="sxs-lookup"><span data-stu-id="21c5d-174">Open hello file `App_Start\Startup.Auth.cs`, and implement hello `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="21c5d-175">Například by měl vypadat jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="21c5d-175">For example, it could look like hello following:</span></span>

```CSharp
// App_Start\Startup.Auth.cs

 public partial class Startup
    {
        // These values are pulled from web.config
        public static string AadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
        public static string Tenant = ConfigurationManager.AppSettings["ida:Tenant"];
        public static string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
        public static string SignUpSignInPolicy = ConfigurationManager.AppSettings["ida:SignUpSignInPolicyId"];
        public static string DefaultPolicy = SignUpSignInPolicy;

        /*
         * Configure hello authorization OWIN middleware.
         */
        public void ConfigureAuth(IAppBuilder app)
        {
            TokenValidationParameters tvps = new TokenValidationParameters
            {
                // Accept only those tokens where hello audience of hello token is equal toohello client ID of this app
                ValidAudience = ClientId,
                AuthenticationType = Startup.DefaultPolicy
            };

            app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
            {
                // This SecurityTokenProvider fetches hello Azure AD B2C metadata & signing keys from hello OpenIDConnect metadata endpoint
                AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(AadInstance, Tenant, DefaultPolicy)))
            });
        }
    }
```

### <a name="secure-hello-task-controller"></a><span data-ttu-id="21c5d-176">Zabezpečení ovladače úkolů hello</span><span class="sxs-lookup"><span data-stu-id="21c5d-176">Secure hello task controller</span></span>

<span data-ttu-id="21c5d-177">Až aplikace hello ověřování nakonfigurované toouse OAuth 2.0, můžete webového rozhraní API zabezpečit přidáním `[Authorize]` ovladače úkolů toohello značky.</span><span class="sxs-lookup"><span data-stu-id="21c5d-177">After hello app is configured toouse OAuth 2.0 authentication, you can secure your web API by adding an `[Authorize]` tag toohello task controller.</span></span> <span data-ttu-id="21c5d-178">Toto je hello řadiče, kde všechny manipulace se seznamem úkolů probíhá, takže byste měli zabezpečit celý ovladač hello na úrovni třídy hello.</span><span class="sxs-lookup"><span data-stu-id="21c5d-178">This is hello controller where all to-do list manipulation takes place, so you should secure hello entire controller at hello class level.</span></span> <span data-ttu-id="21c5d-179">Můžete také přidat hello `[Authorize]` značky tooindividual akce pro podrobnější řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="21c5d-179">You can also add hello `[Authorize]` tag tooindividual actions for more fine-grained control.</span></span>

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-hello-token"></a><span data-ttu-id="21c5d-180">Získání informací o uživateli z hello tokenu</span><span class="sxs-lookup"><span data-stu-id="21c5d-180">Get user information from hello token</span></span>

<span data-ttu-id="21c5d-181">`TasksController`ukládá úkoly do databáze, kde má každý úkol přidruženého uživatele, který "vlastní" hello úloh.</span><span class="sxs-lookup"><span data-stu-id="21c5d-181">`TasksController` stores tasks in a database where each task has an associated user who "owns" hello task.</span></span> <span data-ttu-id="21c5d-182">Hello vlastník je identifikován pomocí hello uživatele **ID objektu**.</span><span class="sxs-lookup"><span data-stu-id="21c5d-182">hello owner is identified by hello user's **object ID**.</span></span> <span data-ttu-id="21c5d-183">(To je proto potřeba ID objektu hello tooadd jako aplikace deklarace identity ve všech zásad.)</span><span class="sxs-lookup"><span data-stu-id="21c5d-183">(This is why you needed tooadd hello object ID as an application claim in all of your policies.)</span></span>

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-hello-permissions-in-hello-token"></a><span data-ttu-id="21c5d-184">Ověření oprávnění hello v tokenu hello</span><span class="sxs-lookup"><span data-stu-id="21c5d-184">Validate hello permissions in hello token</span></span>

<span data-ttu-id="21c5d-185">Běžné požadavky pro webové rozhraní API je toovalidate hello "obory" v hello token.</span><span class="sxs-lookup"><span data-stu-id="21c5d-185">A common requirement for web APIs is toovalidate hello "scopes" present in hello token.</span></span> <span data-ttu-id="21c5d-186">Tím se zajistí, že tento hello uživatel souhlasí toohello oprávnění požadované tooaccess hello úkolů seznamu služby.</span><span class="sxs-lookup"><span data-stu-id="21c5d-186">This ensures that hello user has consented toohello permissions required tooaccess hello to-do list service.</span></span>

```CSharp
public IEnumerable<Models.Task> Get()
{
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "read")
    {
        throw new HttpResponseException(new HttpResponseMessage {
            StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = "hello Scope claim does not contain 'read' or scope claim not found"
        });
    }
    ...
}
```

## <a name="run-hello-sample-app"></a><span data-ttu-id="21c5d-187">Spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="21c5d-187">Run hello sample app</span></span>

<span data-ttu-id="21c5d-188">Nakonec sestavte a spusťte `TaskWebApp` a `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="21c5d-188">Finally, build and run both `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="21c5d-189">Vytvořte nějaké úkoly v seznamu úkolů hello uživatele a Všimněte si, že jsou zachované v hello rozhraní API i po ukončení a restartování klienta hello.</span><span class="sxs-lookup"><span data-stu-id="21c5d-189">Create some tasks on hello user's to-do list and notice how they are persisted in hello API even after you stop and restart hello client.</span></span>

## <a name="edit-your-policies"></a><span data-ttu-id="21c5d-190">Úprava zásad</span><span class="sxs-lookup"><span data-stu-id="21c5d-190">Edit your policies</span></span>

<span data-ttu-id="21c5d-191">Po zabezpečení rozhraní API pomocí Azure AD B2C, můžete experimentovat s vaší zásady Sign v nebo registrace-množství a zobrazení hello efekty (nebo neexistenci) na hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="21c5d-191">After you have secured an API by using Azure AD B2C, you can experiment with your Sign-in/Sign-up policy and view hello effects (or lack thereof) on hello API.</span></span> <span data-ttu-id="21c5d-192">Můžete manipulovat s deklaracemi identity aplikace hello v hello zásady a změna hello informací o uživateli, který je k dispozici v hello webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="21c5d-192">You can manipulate hello application claims in hello policies and change hello user information that is available in hello web API.</span></span> <span data-ttu-id="21c5d-193">Deklarace identity, které přidáte bude k dispozici tooyour .NET MVC webového rozhraní API v hello `ClaimsPrincipal` objektu, jak je popsáno výše v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="21c5d-193">Any claims that you add will be available tooyour .NET MVC web API in hello `ClaimsPrincipal` object, as described earlier in this article.</span></span>
