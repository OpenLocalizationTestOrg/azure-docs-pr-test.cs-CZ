---
title: "Zabezpečení webového rozhraní API – ASP.NET – Azure Active Directory B2C | Dokumentace Microsoftu"
description: "Postup pro sestavení webového rozhraní API .NET pomocí Azure Active Directory B2C zabezpečeného použitím přístupových tokenů OAuth 2.0 pro ověřování."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: mtillman
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 01/14/2018
ms.author: parakhj
ms.custom: seohack1
ms.openlocfilehash: 68c0f50e20b5a1a44ff2cb1bc5df35d60928b911
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a><span data-ttu-id="5a28e-103">Azure Active Directory B2C: Sestavení webového rozhraní API .NET</span><span class="sxs-lookup"><span data-stu-id="5a28e-103">Azure Active Directory B2C: Build a .NET web API</span></span>

<span data-ttu-id="5a28e-104">S Azure Active Directory (Azure AD) B2C můžete zabezpečit webové rozhraní API pomocí přístupových tokenů OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="5a28e-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="5a28e-105">Tyto tokeny umožňují ověření přístupu klientských aplikací do rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5a28e-105">These tokens allow your client apps to authenticate to the API.</span></span> <span data-ttu-id="5a28e-106">Tento článek ukazuje, jak vytvořit rozhraní API „seznam úkolů“ .NET MVC, které umožňuje uživatelům vaší klientské aplikace provádět CRUD u úloh.</span><span class="sxs-lookup"><span data-stu-id="5a28e-106">This article shows you how to create a .NET MVC "to-do list" API that allows users of your client application to CRUD tasks.</span></span> <span data-ttu-id="5a28e-107">Webové rozhraní API je zabezpečeno pomocí Azure AD B2C a umožňuje pouze ověřeným uživatelům spravovat své seznamy úkolů.</span><span class="sxs-lookup"><span data-stu-id="5a28e-107">The web API is secured using Azure AD B2C and only allows authenticated users to manage their to-do list.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="5a28e-108">Vytvoření adresáře Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="5a28e-108">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="5a28e-109">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="5a28e-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="5a28e-110">Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="5a28e-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="5a28e-111">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="5a28e-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

> [!NOTE]
> <span data-ttu-id="5a28e-112">Klientská aplikace a webové rozhraní API musí používat stejný adresář Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="5a28e-112">The client application and web API must use the same Azure AD B2C directory.</span></span>
>

## <a name="create-a-web-api"></a><span data-ttu-id="5a28e-113">Vytvoření webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="5a28e-113">Create a web API</span></span>

<span data-ttu-id="5a28e-114">Dále musíte vytvořit aplikaci webového rozhraní API v adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="5a28e-114">Next, you need to create a web API app in your B2C directory.</span></span> <span data-ttu-id="5a28e-115">Azure AD díky tomu získá informace potřebné k bezpečné komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="5a28e-115">This gives Azure AD information that it needs to securely communicate with your app.</span></span> <span data-ttu-id="5a28e-116">Chcete-li vytvořit aplikaci, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="5a28e-116">To create an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="5a28e-117">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="5a28e-117">Be sure to:</span></span>

* <span data-ttu-id="5a28e-118">Jste do aplikace zahrnuli **webovou aplikaci** nebo **webové rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="5a28e-118">Include a **web app** or **web API** in the application.</span></span>
* <span data-ttu-id="5a28e-119">Použijte **Identifikátor URI pro přesměrování** `https://localhost:44332/` pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5a28e-119">Use the **Redirect URI** `https://localhost:44332/` for the web app.</span></span> <span data-ttu-id="5a28e-120">Toto je výchozí umístění klienta webové aplikace pro tuto ukázku kódu.</span><span class="sxs-lookup"><span data-stu-id="5a28e-120">This is the default location of the web app client for this code sample.</span></span>
* <span data-ttu-id="5a28e-121">Poznamenejte si **ID aplikace** přiřazené vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5a28e-121">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="5a28e-122">Budete ho potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="5a28e-122">You'll need it later.</span></span>
* <span data-ttu-id="5a28e-123">Zadejte identifikátor aplikace do **Identifikátor URI ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5a28e-123">Enter an app identifier into **App ID URI**.</span></span> <span data-ttu-id="5a28e-124">Zkopírujte celý **Identifikátor URI ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5a28e-124">Copy the full **App ID URI**.</span></span> <span data-ttu-id="5a28e-125">Budete ho potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="5a28e-125">You'll need it later.</span></span>
* <span data-ttu-id="5a28e-126">Přidejte oprávnění prostřednictvím nabídky **Publikované obory**.</span><span class="sxs-lookup"><span data-stu-id="5a28e-126">Add permissions through the **Published scopes** menu.</span></span>

## <a name="create-your-policies"></a><span data-ttu-id="5a28e-127">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="5a28e-127">Create your policies</span></span>

<span data-ttu-id="5a28e-128">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="5a28e-128">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="5a28e-129">Budete muset vytvořit zásadu pro komunikaci s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="5a28e-129">You will need to create a policy to communicate with Azure AD B2C.</span></span> <span data-ttu-id="5a28e-130">Doporučujeme používat kombinovanou zásadu registrace/přihlášení, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="5a28e-130">We recommend using the combined sign-up/sign-in policy, as described in the [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="5a28e-131">Při vytváření zásady nezapomeňte na následující:</span><span class="sxs-lookup"><span data-stu-id="5a28e-131">When you create your policy, be sure to:</span></span>

* <span data-ttu-id="5a28e-132">Zvolit **Zobrazovaný název** a další atributy registrace ve svojí zásadě.</span><span class="sxs-lookup"><span data-stu-id="5a28e-132">Choose **Display name** and other sign-up attributes in your policy.</span></span>
* <span data-ttu-id="5a28e-133">Zvolit **Zobrazovaný název** a deklarace identity **ID objektu** jako deklarace identity aplikace v každé zásadě.</span><span class="sxs-lookup"><span data-stu-id="5a28e-133">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="5a28e-134">Můžete zvolit i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="5a28e-134">You can choose other claims as well.</span></span>
* <span data-ttu-id="5a28e-135">Po vytvoření každé zásady si poznamenejte její **Název**.</span><span class="sxs-lookup"><span data-stu-id="5a28e-135">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="5a28e-136">Název zásady budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="5a28e-136">You'll need the policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="5a28e-137">Po úspěšném vytvoření této zásady jste připraveni k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a28e-137">After you have successfully created the policy, you're ready to build your app.</span></span>

## <a name="download-the-code"></a><span data-ttu-id="5a28e-138">Stáhněte si kód</span><span class="sxs-lookup"><span data-stu-id="5a28e-138">Download the code</span></span>

<span data-ttu-id="5a28e-139">Kód k tomuto kurzu se udržuje na [GitHubu](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="5a28e-139">The code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="5a28e-140">Ukázku můžete klonovat spuštěním:</span><span class="sxs-lookup"><span data-stu-id="5a28e-140">You can clone the sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="5a28e-141">Po stažení ukázkového kódu otevřete soubor Visual Studio .sln, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="5a28e-141">After you download the sample code, open the Visual Studio .sln file to get started.</span></span> <span data-ttu-id="5a28e-142">Soubor řešení obsahuje dva projekty: `TaskWebApp` a `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="5a28e-142">The solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="5a28e-143">`TaskWebApp` je webová aplikace MVC, se kterou uživatel komunikuje.</span><span class="sxs-lookup"><span data-stu-id="5a28e-143">`TaskWebApp` is an MVC web application that the user interacts with.</span></span> <span data-ttu-id="5a28e-144">`TaskService` je back-endové webové rozhraní API aplikace, které ukládá seznam úkolů každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="5a28e-144">`TaskService` is the app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="5a28e-145">Tento článek bude probírat pouze aplikaci `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="5a28e-145">This article will only discuss the `TaskService` application.</span></span> <span data-ttu-id="5a28e-146">Způsob, jak vytvořit aplikaci `TaskWebApp` pomocí Azure AD B2C, najdete v [kurzu webových aplikací .NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="5a28e-146">To learn how to build `TaskWebApp` using Azure AD B2C, see [our .NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span>

### <a name="update-the-azure-ad-b2c-configuration"></a><span data-ttu-id="5a28e-147">Aktualizace konfigurace Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="5a28e-147">Update the Azure AD B2C configuration</span></span>

<span data-ttu-id="5a28e-148">Naše ukázka je nakonfigurovaná k použití zásad a ID klienta naše ukázkového tenanta.</span><span class="sxs-lookup"><span data-stu-id="5a28e-148">Our sample is configured to use the policies and client ID of our demo tenant.</span></span> <span data-ttu-id="5a28e-149">Pokud chcete použít vlastního tenanta, musíte provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="5a28e-149">If you would like to use your own tenant, you will need to do the following:</span></span>

1. <span data-ttu-id="5a28e-150">Otevřete `web.config` v projektu `TaskService` a nahraďte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5a28e-150">Open `web.config` in the `TaskService` project and replace the values for</span></span>
    * <span data-ttu-id="5a28e-151">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="5a28e-151">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="5a28e-152">`ida:ClientId` identifikátorem aplikace webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="5a28e-152">`ida:ClientId` with your web API application ID</span></span>
    * <span data-ttu-id="5a28e-153">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="5a28e-153">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="5a28e-154">Otevřete `web.config` v projektu `TaskWebApp` a nahraďte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5a28e-154">Open `web.config` in the `TaskWebApp` project and replace the values for</span></span>
    * <span data-ttu-id="5a28e-155">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="5a28e-155">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="5a28e-156">`ida:ClientId` identifikátorem webového aplikace</span><span class="sxs-lookup"><span data-stu-id="5a28e-156">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="5a28e-157">`ida:ClientSecret` tajným klíčem webové aplikace</span><span class="sxs-lookup"><span data-stu-id="5a28e-157">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="5a28e-158">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="5a28e-158">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="5a28e-159">`ida:EditProfilePolicyId` názvem zásady pro úpravu profilu</span><span class="sxs-lookup"><span data-stu-id="5a28e-159">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="5a28e-160">`ida:ResetPasswordPolicyId` názvem zásady pro resetování hesla</span><span class="sxs-lookup"><span data-stu-id="5a28e-160">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>
    * <span data-ttu-id="5a28e-161">`api:ApiIdentifier` identifikátorem URI ID aplikace</span><span class="sxs-lookup"><span data-stu-id="5a28e-161">`api:ApiIdentifier` with your "App ID URI"</span></span>


## <a name="secure-the-api"></a><span data-ttu-id="5a28e-162">Zabezpečení rozhraní API</span><span class="sxs-lookup"><span data-stu-id="5a28e-162">Secure the API</span></span>

<span data-ttu-id="5a28e-163">Pokud máte klienta, který volá vaše rozhraní API, můžete rozhraní API (například `TaskService`) zabezpečit pomocí nosných tokenů OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="5a28e-163">When you have a client that calls your API, you can secure your API (e.g `TaskService`) by using OAuth 2.0 bearer tokens.</span></span> <span data-ttu-id="5a28e-164">To zajistí, že každý požadavek na vaše rozhraní API bude platný pouze tehdy, pokud požadavek má nosný token.</span><span class="sxs-lookup"><span data-stu-id="5a28e-164">This ensures that each request to your API will only be valid if the request has a bearer token.</span></span> <span data-ttu-id="5a28e-165">Vaše rozhraní API může přijímat a ověřovat nosné tokeny pomocí knihovny Microsoft's Open Web Interface pro .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="5a28e-165">Your API can accept and validate bearer tokens by using Microsoft's Open Web Interface for .NET (OWIN) library.</span></span>

### <a name="install-owin"></a><span data-ttu-id="5a28e-166">Instalace OWIN</span><span class="sxs-lookup"><span data-stu-id="5a28e-166">Install OWIN</span></span>

<span data-ttu-id="5a28e-167">Začněte instalací ověřovacího kanálu OWIN OAuth pomocí konzoly Správce balíčků sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5a28e-167">Begin by installing the OWIN OAuth authentication pipeline by using the Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

<span data-ttu-id="5a28e-168">Tím se nainstaluje middleware OWIN, který bude přijímat a ověřovat nosné tokeny.</span><span class="sxs-lookup"><span data-stu-id="5a28e-168">This will install the OWIN middleware that will accept and validate bearer tokens.</span></span>

### <a name="add-an-owin-startup-class"></a><span data-ttu-id="5a28e-169">Přidání třídy pro spuštění OWIN</span><span class="sxs-lookup"><span data-stu-id="5a28e-169">Add an OWIN startup class</span></span>

<span data-ttu-id="5a28e-170">Přidejte třídu pro spuštění OWIN s názvem `Startup.cs` do rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5a28e-170">Add an OWIN startup class to the API called `Startup.cs`.</span></span>  <span data-ttu-id="5a28e-171">Klikněte pravým tlačítkem myši na projekt, vyberte **Přidat** a **Nová položka**, a poté vyhledejte OWIN.</span><span class="sxs-lookup"><span data-stu-id="5a28e-171">Right-click on the project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="5a28e-172">Middleware OWIN při spuštění vaší aplikace vyvolá metodu `Configuration(…)`.</span><span class="sxs-lookup"><span data-stu-id="5a28e-172">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="5a28e-173">V naší ukázce jsme změnili deklaraci třídy na `public partial class Startup` a implementovali druhou část třídy v `App_Start\Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="5a28e-173">In our sample, we changed the class declaration to `public partial class Startup` and implemented the other part of the class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="5a28e-174">Uvnitř metody `Configuration` jsme přidali volání metody `ConfigureAuth`, která je definovaná v `Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="5a28e-174">Inside the `Configuration` method, we added a call to `ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="5a28e-175">Po úpravách vypadá `Startup.cs` následovně:</span><span class="sxs-lookup"><span data-stu-id="5a28e-175">After the modifications, `Startup.cs` looks like the following:</span></span>

```CSharp
// Startup.cs

public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of the class
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a><span data-ttu-id="5a28e-176">Konfigurace ověřování OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="5a28e-176">Configure OAuth 2.0 authentication</span></span>

<span data-ttu-id="5a28e-177">Otevřete soubor `App_Start\Startup.Auth.cs` a implementujte metodu `ConfigureAuth(...)`.</span><span class="sxs-lookup"><span data-stu-id="5a28e-177">Open the file `App_Start\Startup.Auth.cs`, and implement the `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="5a28e-178">Například může vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="5a28e-178">For example, it could look like the following:</span></span>

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
         * Configure the authorization OWIN middleware.
         */
        public void ConfigureAuth(IAppBuilder app)
        {
            TokenValidationParameters tvps = new TokenValidationParameters
            {
                // Accept only those tokens where the audience of the token is equal to the client ID of this app
                ValidAudience = ClientId,
                AuthenticationType = Startup.DefaultPolicy
            };

            app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
            {
                // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
                AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(AadInstance, Tenant, DefaultPolicy)))
            });
        }
    }
```

### <a name="secure-the-task-controller"></a><span data-ttu-id="5a28e-179">Zabezpečení ovladače úkolů</span><span class="sxs-lookup"><span data-stu-id="5a28e-179">Secure the task controller</span></span>

<span data-ttu-id="5a28e-180">Jakmile je aplikace nakonfigurovaná pro použití ověřování OAuth 2.0, můžete svoje webové rozhraní API zabezpečit přidáním značky `[Authorize]` do ovladače úkolů.</span><span class="sxs-lookup"><span data-stu-id="5a28e-180">After the app is configured to use OAuth 2.0 authentication, you can secure your web API by adding an `[Authorize]` tag to the task controller.</span></span> <span data-ttu-id="5a28e-181">To je ovladač, kde se odehrává veškerá manipulace se seznamem úkolů, takže byste měli zabezpečit celý ovladač na úrovni tříd.</span><span class="sxs-lookup"><span data-stu-id="5a28e-181">This is the controller where all to-do list manipulation takes place, so you should secure the entire controller at the class level.</span></span> <span data-ttu-id="5a28e-182">Pro větší kontrolu můžete značku `[Authorize]` přidat také k jednotlivým akcím.</span><span class="sxs-lookup"><span data-stu-id="5a28e-182">You can also add the `[Authorize]` tag to individual actions for more fine-grained control.</span></span>

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a><span data-ttu-id="5a28e-183">Získání informací o uživateli z tokenu</span><span class="sxs-lookup"><span data-stu-id="5a28e-183">Get user information from the token</span></span>

<span data-ttu-id="5a28e-184">`TasksController` ukládá úkoly do databáze, kde má každý úkol přidruženého uživatele, který úkol „vlastní“.</span><span class="sxs-lookup"><span data-stu-id="5a28e-184">`TasksController` stores tasks in a database where each task has an associated user who "owns" the task.</span></span> <span data-ttu-id="5a28e-185">Vlastník je identifikován pomocí **ID objektu** uživatele.</span><span class="sxs-lookup"><span data-stu-id="5a28e-185">The owner is identified by the user's **object ID**.</span></span> <span data-ttu-id="5a28e-186">(To je důvod, proč bylo nutné přidat ID objektu jako deklaraci identity aplikace ve všech vašich zásadách.)</span><span class="sxs-lookup"><span data-stu-id="5a28e-186">(This is why you needed to add the object ID as an application claim in all of your policies.)</span></span>

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-the-permissions-in-the-token"></a><span data-ttu-id="5a28e-187">Ověření oprávnění v tokenu</span><span class="sxs-lookup"><span data-stu-id="5a28e-187">Validate the permissions in the token</span></span>

<span data-ttu-id="5a28e-188">Běžné požadavky pro webová rozhraní API jsou ověření „oborů“ v tokenu.</span><span class="sxs-lookup"><span data-stu-id="5a28e-188">A common requirement for web APIs is to validate the "scopes" present in the token.</span></span> <span data-ttu-id="5a28e-189">Tím se zajistí, že uživatel souhlasil s oprávněními požadovanými pro přístup ke službě seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="5a28e-189">This ensures that the user has consented to the permissions required to access the to-do list service.</span></span>

```CSharp
public IEnumerable<Models.Task> Get()
{
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "read")
    {
        throw new HttpResponseException(new HttpResponseMessage {
            StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = "The Scope claim does not contain 'read' or scope claim not found"
        });
    }
    ...
}
```

## <a name="run-the-sample-app"></a><span data-ttu-id="5a28e-190">Spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="5a28e-190">Run the sample app</span></span>

<span data-ttu-id="5a28e-191">Nakonec sestavte a spusťte `TaskWebApp` a `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="5a28e-191">Finally, build and run both `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="5a28e-192">Vytvořte nějaké úkoly v seznamu úkolů uživatele a všimněte si, že jsou zachované v rozhraní API i po vypnutí a restartování klienta.</span><span class="sxs-lookup"><span data-stu-id="5a28e-192">Create some tasks on the user's to-do list and notice how they are persisted in the API even after you stop and restart the client.</span></span>

## <a name="edit-your-policies"></a><span data-ttu-id="5a28e-193">Úprava zásad</span><span class="sxs-lookup"><span data-stu-id="5a28e-193">Edit your policies</span></span>

<span data-ttu-id="5a28e-194">Po zabezpečení rozhraní API pomocí Azure AD B2C můžete experimentovat se zásadou registrace/přihlášení a zjistit, jaký vliv mají (nebo nemají) na rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5a28e-194">After you have secured an API by using Azure AD B2C, you can experiment with your Sign-in/Sign-up policy and view the effects (or lack thereof) on the API.</span></span> <span data-ttu-id="5a28e-195">Můžete také manipulovat s deklaracemi identity aplikace v zásadách a změnit, jaké informace o uživateli mají být přístupné v rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="5a28e-195">You can manipulate the application claims in the policies and change the user information that is available in the web API.</span></span> <span data-ttu-id="5a28e-196">Jakékoli přidané deklarace identity budou dostupné vašemu webovému rozhraní API .NET MVC v objektu `ClaimsPrincipal`, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="5a28e-196">Any claims that you add will be available to your .NET MVC web API in the `ClaimsPrincipal` object, as described earlier in this article.</span></span>
