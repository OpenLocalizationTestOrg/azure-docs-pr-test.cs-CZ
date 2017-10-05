---
title: Azure AD B2C | Dokumentace Microsoftu
description: "Postup pro sestavení webového rozhraní API .NET pomocí Azure Active Directory B2C zabezpečeného použitím přístupových tokenů OAuth 2.0 pro ověřování."
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
ms.openlocfilehash: 48749bfa2ab54a0e766a4aad4f39073cc4e90818
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a><span data-ttu-id="90b32-103">Azure Active Directory B2C: Sestavení webového rozhraní API .NET</span><span class="sxs-lookup"><span data-stu-id="90b32-103">Azure Active Directory B2C: Build a .NET web API</span></span>

<span data-ttu-id="90b32-104">S Azure Active Directory (Azure AD) B2C můžete zabezpečit webové rozhraní API pomocí přístupových tokenů OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="90b32-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="90b32-105">Tyto tokeny umožňují ověření přístupu klientských aplikací do rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="90b32-105">These tokens allow your client apps to authenticate to the API.</span></span> <span data-ttu-id="90b32-106">Tento článek ukazuje, jak vytvořit rozhraní API „seznam úkolů“ .NET MVC, které umožňuje uživatelům vaší klientské aplikace provádět CRUD u úloh.</span><span class="sxs-lookup"><span data-stu-id="90b32-106">This article shows you how to create a .NET MVC "to-do list" API that allows users of your client application to CRUD tasks.</span></span> <span data-ttu-id="90b32-107">Webové rozhraní API je zabezpečeno pomocí Azure AD B2C a umožňuje pouze ověřeným uživatelům spravovat své seznamy úkolů.</span><span class="sxs-lookup"><span data-stu-id="90b32-107">The web API is secured using Azure AD B2C and only allows authenticated users to manage their to-do list.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="90b32-108">Vytvoření adresáře Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="90b32-108">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="90b32-109">Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="90b32-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="90b32-110">Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další.</span><span class="sxs-lookup"><span data-stu-id="90b32-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="90b32-111">Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="90b32-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

> [!NOTE]
> <span data-ttu-id="90b32-112">Klientská aplikace a webové rozhraní API musí používat stejný adresář Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="90b32-112">The client application and web API must use the same Azure AD B2C directory.</span></span>
>

## <a name="create-a-web-api"></a><span data-ttu-id="90b32-113">Vytvoření webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="90b32-113">Create a web API</span></span>

<span data-ttu-id="90b32-114">Dále musíte vytvořit aplikaci webového rozhraní API v adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="90b32-114">Next, you need to create a web API app in your B2C directory.</span></span> <span data-ttu-id="90b32-115">Azure AD díky tomu získá informace potřebné k bezpečné komunikaci s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="90b32-115">This gives Azure AD information that it needs to securely communicate with your app.</span></span> <span data-ttu-id="90b32-116">Chcete-li vytvořit aplikaci, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="90b32-116">To create an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="90b32-117">Ujistěte se, že:</span><span class="sxs-lookup"><span data-stu-id="90b32-117">Be sure to:</span></span>

* <span data-ttu-id="90b32-118">Jste do aplikace zahrnuli **webovou aplikaci** nebo **webové rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="90b32-118">Include a **web app** or **web API** in the application.</span></span>
* <span data-ttu-id="90b32-119">Použijte **Identifikátor URI pro přesměrování** `https://localhost:44332/` pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="90b32-119">Use the **Redirect URI** `https://localhost:44332/` for the web app.</span></span> <span data-ttu-id="90b32-120">Toto je výchozí umístění klienta webové aplikace pro tuto ukázku kódu.</span><span class="sxs-lookup"><span data-stu-id="90b32-120">This is the default location of the web app client for this code sample.</span></span>
* <span data-ttu-id="90b32-121">Poznamenejte si **ID aplikace** přiřazené vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="90b32-121">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="90b32-122">Budete ho potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="90b32-122">You'll need it later.</span></span>
* <span data-ttu-id="90b32-123">Zadejte identifikátor aplikace do **Identifikátor URI ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="90b32-123">Enter an app identifier into **App ID URI**.</span></span>
* <span data-ttu-id="90b32-124">Přidejte oprávnění prostřednictvím nabídky **Publikované obory**.</span><span class="sxs-lookup"><span data-stu-id="90b32-124">Add permissions through the **Published scopes** menu.</span></span>

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="90b32-125">Vytvořte svoje zásady</span><span class="sxs-lookup"><span data-stu-id="90b32-125">Create your policies</span></span>

<span data-ttu-id="90b32-126">V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="90b32-126">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="90b32-127">Budete muset vytvořit zásadu pro komunikaci s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="90b32-127">You will need to create a policy to communicate with Azure AD B2C.</span></span> <span data-ttu-id="90b32-128">Doporučujeme používat kombinovanou zásadu registrace/přihlášení, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="90b32-128">We recommend using the combined sign-up/sign-in policy, as described in the [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="90b32-129">Při vytváření zásady nezapomeňte na následující:</span><span class="sxs-lookup"><span data-stu-id="90b32-129">When you create your policy, be sure to:</span></span>

* <span data-ttu-id="90b32-130">Zvolit **Zobrazovaný název** a další atributy registrace ve svojí zásadě.</span><span class="sxs-lookup"><span data-stu-id="90b32-130">Choose **Display name** and other sign-up attributes in your policy.</span></span>
* <span data-ttu-id="90b32-131">Zvolit **Zobrazovaný název** a deklarace identity **ID objektu** jako deklarace identity aplikace v každé zásadě.</span><span class="sxs-lookup"><span data-stu-id="90b32-131">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="90b32-132">Můžete zvolit i další deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="90b32-132">You can choose other claims as well.</span></span>
* <span data-ttu-id="90b32-133">Po vytvoření každé zásady si poznamenejte její **Název**.</span><span class="sxs-lookup"><span data-stu-id="90b32-133">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="90b32-134">Název zásady budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="90b32-134">You'll need the policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="90b32-135">Po úspěšném vytvoření této zásady jste připraveni k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="90b32-135">After you have successfully created the policy, you're ready to build your app.</span></span>

## <a name="download-the-code"></a><span data-ttu-id="90b32-136">Stáhněte si kód</span><span class="sxs-lookup"><span data-stu-id="90b32-136">Download the code</span></span>

<span data-ttu-id="90b32-137">Kód k tomuto kurzu se udržuje na [GitHubu](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="90b32-137">The code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="90b32-138">Ukázku můžete klonovat spuštěním:</span><span class="sxs-lookup"><span data-stu-id="90b32-138">You can clone the sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="90b32-139">Po stažení ukázkového kódu otevřete soubor Visual Studio .sln, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="90b32-139">After you download the sample code, open the Visual Studio .sln file to get started.</span></span> <span data-ttu-id="90b32-140">Soubor řešení obsahuje dva projekty: `TaskWebApp` a `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="90b32-140">The solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="90b32-141">`TaskWebApp` je webová aplikace MVC, se kterou uživatel komunikuje.</span><span class="sxs-lookup"><span data-stu-id="90b32-141">`TaskWebApp` is an MVC web application that the user interacts with.</span></span> <span data-ttu-id="90b32-142">`TaskService` je back-endové webové rozhraní API aplikace, které ukládá seznam úkolů každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="90b32-142">`TaskService` is the app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="90b32-143">Tento článek bude probírat pouze aplikaci `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="90b32-143">This article will only discuss the `TaskService` application.</span></span> <span data-ttu-id="90b32-144">Způsob, jak vytvořit aplikaci `TaskWebApp` pomocí Azure AD B2C, najdete v [kurzu webových aplikací .NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="90b32-144">To learn how to build `TaskWebApp` using Azure AD B2C, see [our .NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span>

### <a name="update-the-azure-ad-b2c-configuration"></a><span data-ttu-id="90b32-145">Aktualizace konfigurace Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="90b32-145">Update the Azure AD B2C configuration</span></span>

<span data-ttu-id="90b32-146">Naše ukázka je nakonfigurovaná k použití zásad a ID klienta naše ukázkového tenanta.</span><span class="sxs-lookup"><span data-stu-id="90b32-146">Our sample is configured to use the policies and client ID of our demo tenant.</span></span> <span data-ttu-id="90b32-147">Pokud chcete použít vlastního tenanta, musíte provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="90b32-147">If you would like to use your own tenant, you will need to do the following:</span></span>

1. <span data-ttu-id="90b32-148">Otevřete `web.config` v projektu `TaskService` a nahraďte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="90b32-148">Open `web.config` in the `TaskService` project and replace the values for</span></span>
    * <span data-ttu-id="90b32-149">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="90b32-149">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="90b32-150">`ida:ClientId` identifikátorem aplikace webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="90b32-150">`ida:ClientId` with your web API application ID</span></span>
    * <span data-ttu-id="90b32-151">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="90b32-151">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="90b32-152">Otevřete `web.config` v projektu `TaskWebApp` a nahraďte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="90b32-152">Open `web.config` in the `TaskWebApp` project and replace the values for</span></span>
    * <span data-ttu-id="90b32-153">`ida:Tenant` názvem vašeho tenanta</span><span class="sxs-lookup"><span data-stu-id="90b32-153">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="90b32-154">`ida:ClientId` identifikátorem webového aplikace</span><span class="sxs-lookup"><span data-stu-id="90b32-154">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="90b32-155">`ida:ClientSecret` tajným klíčem webové aplikace</span><span class="sxs-lookup"><span data-stu-id="90b32-155">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="90b32-156">`ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení</span><span class="sxs-lookup"><span data-stu-id="90b32-156">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="90b32-157">`ida:EditProfilePolicyId` názvem zásady pro úpravu profilu</span><span class="sxs-lookup"><span data-stu-id="90b32-157">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="90b32-158">`ida:ResetPasswordPolicyId` názvem zásady pro resetování hesla</span><span class="sxs-lookup"><span data-stu-id="90b32-158">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>


## <a name="secure-the-api"></a><span data-ttu-id="90b32-159">Zabezpečení rozhraní API</span><span class="sxs-lookup"><span data-stu-id="90b32-159">Secure the API</span></span>

<span data-ttu-id="90b32-160">Pokud máte klienta, který volá vaše rozhraní API, můžete rozhraní API (například `TaskService`) zabezpečit pomocí nosných tokenů OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="90b32-160">When you have a client that calls your API, you can secure your API (e.g `TaskService`) by using OAuth 2.0 bearer tokens.</span></span> <span data-ttu-id="90b32-161">To zajistí, že každý požadavek na vaše rozhraní API bude platný pouze tehdy, pokud požadavek má nosný token.</span><span class="sxs-lookup"><span data-stu-id="90b32-161">This ensures that each request to your API will only be valid if the request has a bearer token.</span></span> <span data-ttu-id="90b32-162">Vaše rozhraní API může přijímat a ověřovat nosné tokeny pomocí knihovny Microsoft's Open Web Interface pro .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="90b32-162">Your API can accept and validate bearer tokens by using Microsoft's Open Web Interface for .NET (OWIN) library.</span></span>

### <a name="install-owin"></a><span data-ttu-id="90b32-163">Instalace OWIN</span><span class="sxs-lookup"><span data-stu-id="90b32-163">Install OWIN</span></span>

<span data-ttu-id="90b32-164">Začněte instalací ověřovacího kanálu OWIN OAuth pomocí konzoly Správce balíčků sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90b32-164">Begin by installing the OWIN OAuth authentication pipeline by using the Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

<span data-ttu-id="90b32-165">Tím se nainstaluje middleware OWIN, který bude přijímat a ověřovat nosné tokeny.</span><span class="sxs-lookup"><span data-stu-id="90b32-165">This will install the OWIN middleware that will accept and validate bearer tokens.</span></span>

### <a name="add-an-owin-startup-class"></a><span data-ttu-id="90b32-166">Přidání třídy pro spuštění OWIN</span><span class="sxs-lookup"><span data-stu-id="90b32-166">Add an OWIN startup class</span></span>

<span data-ttu-id="90b32-167">Přidejte třídu pro spuštění OWIN s názvem `Startup.cs` do rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="90b32-167">Add an OWIN startup class to the API called `Startup.cs`.</span></span>  <span data-ttu-id="90b32-168">Klikněte pravým tlačítkem myši na projekt, vyberte **Přidat** a **Nová položka**, a poté vyhledejte OWIN.</span><span class="sxs-lookup"><span data-stu-id="90b32-168">Right-click on the project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="90b32-169">Middleware OWIN při spuštění vaší aplikace vyvolá metodu `Configuration(…)`.</span><span class="sxs-lookup"><span data-stu-id="90b32-169">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="90b32-170">V naší ukázce jsme změnili deklaraci třídy na `public partial class Startup` a implementovali druhou část třídy v `App_Start\Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="90b32-170">In our sample, we changed the class declaration to `public partial class Startup` and implemented the other part of the class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="90b32-171">Uvnitř metody `Configuration` jsme přidali volání metody `ConfigureAuth`, která je definovaná v `Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="90b32-171">Inside the `Configuration` method, we added a call to `ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="90b32-172">Po úpravách vypadá `Startup.cs` následovně:</span><span class="sxs-lookup"><span data-stu-id="90b32-172">After the modifications, `Startup.cs` looks like the following:</span></span>

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

### <a name="configure-oauth-20-authentication"></a><span data-ttu-id="90b32-173">Konfigurace ověřování OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="90b32-173">Configure OAuth 2.0 authentication</span></span>

<span data-ttu-id="90b32-174">Otevřete soubor `App_Start\Startup.Auth.cs` a implementujte metodu `ConfigureAuth(...)`.</span><span class="sxs-lookup"><span data-stu-id="90b32-174">Open the file `App_Start\Startup.Auth.cs`, and implement the `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="90b32-175">Například může vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="90b32-175">For example, it could look like the following:</span></span>

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

### <a name="secure-the-task-controller"></a><span data-ttu-id="90b32-176">Zabezpečení ovladače úkolů</span><span class="sxs-lookup"><span data-stu-id="90b32-176">Secure the task controller</span></span>

<span data-ttu-id="90b32-177">Jakmile je aplikace nakonfigurovaná pro použití ověřování OAuth 2.0, můžete svoje webové rozhraní API zabezpečit přidáním značky `[Authorize]` do ovladače úkolů.</span><span class="sxs-lookup"><span data-stu-id="90b32-177">After the app is configured to use OAuth 2.0 authentication, you can secure your web API by adding an `[Authorize]` tag to the task controller.</span></span> <span data-ttu-id="90b32-178">To je ovladač, kde se odehrává veškerá manipulace se seznamem úkolů, takže byste měli zabezpečit celý ovladač na úrovni tříd.</span><span class="sxs-lookup"><span data-stu-id="90b32-178">This is the controller where all to-do list manipulation takes place, so you should secure the entire controller at the class level.</span></span> <span data-ttu-id="90b32-179">Pro větší kontrolu můžete značku `[Authorize]` přidat také k jednotlivým akcím.</span><span class="sxs-lookup"><span data-stu-id="90b32-179">You can also add the `[Authorize]` tag to individual actions for more fine-grained control.</span></span>

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a><span data-ttu-id="90b32-180">Získání informací o uživateli z tokenu</span><span class="sxs-lookup"><span data-stu-id="90b32-180">Get user information from the token</span></span>

<span data-ttu-id="90b32-181">`TasksController` ukládá úkoly do databáze, kde má každý úkol přidruženého uživatele, který úkol „vlastní“.</span><span class="sxs-lookup"><span data-stu-id="90b32-181">`TasksController` stores tasks in a database where each task has an associated user who "owns" the task.</span></span> <span data-ttu-id="90b32-182">Vlastník je identifikován pomocí **ID objektu** uživatele.</span><span class="sxs-lookup"><span data-stu-id="90b32-182">The owner is identified by the user's **object ID**.</span></span> <span data-ttu-id="90b32-183">(To je důvod, proč bylo nutné přidat ID objektu jako deklaraci identity aplikace ve všech vašich zásadách.)</span><span class="sxs-lookup"><span data-stu-id="90b32-183">(This is why you needed to add the object ID as an application claim in all of your policies.)</span></span>

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-the-permissions-in-the-token"></a><span data-ttu-id="90b32-184">Ověření oprávnění v tokenu</span><span class="sxs-lookup"><span data-stu-id="90b32-184">Validate the permissions in the token</span></span>

<span data-ttu-id="90b32-185">Běžné požadavky pro webová rozhraní API jsou ověření „oborů“ v tokenu.</span><span class="sxs-lookup"><span data-stu-id="90b32-185">A common requirement for web APIs is to validate the "scopes" present in the token.</span></span> <span data-ttu-id="90b32-186">Tím se zajistí, že uživatel souhlasil s oprávněními požadovanými pro přístup ke službě seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="90b32-186">This ensures that the user has consented to the permissions required to access the to-do list service.</span></span>

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

## <a name="run-the-sample-app"></a><span data-ttu-id="90b32-187">Spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="90b32-187">Run the sample app</span></span>

<span data-ttu-id="90b32-188">Nakonec sestavte a spusťte `TaskWebApp` a `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="90b32-188">Finally, build and run both `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="90b32-189">Vytvořte nějaké úkoly v seznamu úkolů uživatele a všimněte si, že jsou zachované v rozhraní API i po vypnutí a restartování klienta.</span><span class="sxs-lookup"><span data-stu-id="90b32-189">Create some tasks on the user's to-do list and notice how they are persisted in the API even after you stop and restart the client.</span></span>

## <a name="edit-your-policies"></a><span data-ttu-id="90b32-190">Úprava zásad</span><span class="sxs-lookup"><span data-stu-id="90b32-190">Edit your policies</span></span>

<span data-ttu-id="90b32-191">Po zabezpečení rozhraní API pomocí Azure AD B2C můžete experimentovat se zásadou registrace/přihlášení a zjistit, jaký vliv mají (nebo nemají) na rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="90b32-191">After you have secured an API by using Azure AD B2C, you can experiment with your Sign-in/Sign-up policy and view the effects (or lack thereof) on the API.</span></span> <span data-ttu-id="90b32-192">Můžete také manipulovat s deklaracemi identity aplikace v zásadách a změnit, jaké informace o uživateli mají být přístupné v rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="90b32-192">You can manipulate the application claims in the policies and change the user information that is available in the web API.</span></span> <span data-ttu-id="90b32-193">Jakékoli přidané deklarace identity budou dostupné vašemu webovému rozhraní API .NET MVC v objektu `ClaimsPrincipal`, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="90b32-193">Any claims that you add will be available to your .NET MVC web API in the `ClaimsPrincipal` object, as described earlier in this article.</span></span>
