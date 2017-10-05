---
title: "Přidání přihlášení do .NET MVC webového rozhraní API pomocí koncového bodu v2.0 Azure AD | Microsoft Docs"
description: "Jak sestavit Web Api MVC .NET, které přijímá tokeny z obou osobní Account Microsoft a pracovní nebo školní účty."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e77bc4e0-d0c9-4075-a3f6-769e2c810206
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: b2d7bbfcd9218698f71e9dfdb1ad5d9ff8740f5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="secure-an-mvc-web-api"></a><span data-ttu-id="d0176-103">Zabezpečení webového rozhraní API MVC</span><span class="sxs-lookup"><span data-stu-id="d0176-103">Secure an MVC web API</span></span>
<span data-ttu-id="d0176-104">S Azure Active Directory koncový bod v2.0, budete moci chránit webového rozhraní API pomocí [OAuth 2.0](active-directory-v2-protocols.md) přístup tokeny, povolení uživatelé s i osobní účet Microsoft a pracovní nebo školní účty k bezpečnému přístupu k vaší webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d0176-104">With Azure Active Directory the v2.0 endpoint, you can protect a Web API using [OAuth 2.0](active-directory-v2-protocols.md) access tokens, enabling users with both personal Microsoft account and work or school accounts to securely access your Web API.</span></span>

> [!NOTE]
> <span data-ttu-id="d0176-105">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0.</span><span class="sxs-lookup"><span data-stu-id="d0176-105">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="d0176-106">Pokud chcete zjistit, pokud byste měli používat koncový bod v2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="d0176-106">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

<span data-ttu-id="d0176-107">V rozhraní ASP.NET web API můžete to provést pomocí middlewaru OWIN společnosti Microsoft, zahrnutá v rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="d0176-107">In ASP.NET web APIs, you can accomplish this using Microsoft’s OWIN middleware included in .NET Framework 4.5.</span></span>  <span data-ttu-id="d0176-108">Zde použijeme OWIN sestavit Web API MVC "Seznam úkolů", která umožňuje klientům vytváření a čtení úkolů ze seznamu úkolů uživatele.</span><span class="sxs-lookup"><span data-stu-id="d0176-108">Here we’ll use OWIN to build a "To Do List" MVC Web API that allows clients to create and read tasks from a user's To-Do list.</span></span>  <span data-ttu-id="d0176-109">Webové rozhraní API ověří, že příchozí požadavky obsahovat platné přístupový token a odmítnout všechny požadavky, které nepředávejte ověření na chráněných trase.</span><span class="sxs-lookup"><span data-stu-id="d0176-109">The web API will validate that incoming requests contain a valid access token and reject any requests that do not pass validation on a protected route.</span></span>  <span data-ttu-id="d0176-110">Tato ukázka byla vytvořená s využitím sady Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="d0176-110">This sample was built using Visual Studio 2015.</span></span>

## <a name="download"></a><span data-ttu-id="d0176-111">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="d0176-111">Download</span></span>
<span data-ttu-id="d0176-112">Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span><span class="sxs-lookup"><span data-stu-id="d0176-112">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span></span>  <span data-ttu-id="d0176-113">Chcete-li sledovat, můžete [stáhnout kostru aplikace jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) nebo tuto kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="d0176-113">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

<span data-ttu-id="d0176-114">Kostru aplikace zahrnuje často používaný kód pro jednoduché rozhraní API, ale všechny součásti související s identity chybí.</span><span class="sxs-lookup"><span data-stu-id="d0176-114">The skeleton app includes all the boilerplate code for a simple API, but is missing all of the identity-related pieces.</span></span> <span data-ttu-id="d0176-115">Pokud nechcete, aby se podle nich zorientujete, můžete místo toho klonovat nebo [stažení je hotová ukázka](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="d0176-115">If you don't want to follow along, you can instead clone or [download the completed sample](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span></span>

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a><span data-ttu-id="d0176-116">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="d0176-116">Register an app</span></span>
<span data-ttu-id="d0176-117">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="d0176-117">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="d0176-118">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="d0176-118">Make sure to:</span></span>

* <span data-ttu-id="d0176-119">Zkopírování **Id aplikace** přiřazené vaší aplikaci, budete ho potřebovat brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d0176-119">Copy down the **Application Id** assigned to your app, you'll need it soon.</span></span>

<span data-ttu-id="d0176-120">Toto řešení sady visual studio také obsahuje "TodoListClient", což je jednoduchou aplikaci WPF.</span><span class="sxs-lookup"><span data-stu-id="d0176-120">This visual studio solution also contains a "TodoListClient", which is a simple WPF app.</span></span>  <span data-ttu-id="d0176-121">TodoListClient se používá k předvedení jak uživatel přihlásí, a jak můžete vydat požadavky pro webové rozhraní API klienta.</span><span class="sxs-lookup"><span data-stu-id="d0176-121">The TodoListClient is used to demonstrate how a user signs-in and how a client can issue requests to your Web API.</span></span>  <span data-ttu-id="d0176-122">V takovém případě TodoListClient i TodoListService jsou reprezentované pomocí stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d0176-122">In this case, both the TodoListClient and the TodoListService are represented by the same app.</span></span>  <span data-ttu-id="d0176-123">Pokud chcete nakonfigurovat TodoListClient, měli byste také:</span><span class="sxs-lookup"><span data-stu-id="d0176-123">To configure the TodoListClient, you should also:</span></span>

* <span data-ttu-id="d0176-124">Přidat **Mobile** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d0176-124">Add the **Mobile** platform for your app.</span></span>

## <a name="install-owin"></a><span data-ttu-id="d0176-125">Instalace OWIN</span><span class="sxs-lookup"><span data-stu-id="d0176-125">Install OWIN</span></span>
<span data-ttu-id="d0176-126">Teď, když jste registrováni aplikace, musíte nastavit aplikaci ke komunikaci s koncovým bodem v2.0, aby bylo možné ověřit příchozí žádosti a tokeny.</span><span class="sxs-lookup"><span data-stu-id="d0176-126">Now that you’ve registered an app, you need to set up your app to communicate with the v2.0 endpoint in order to validate incoming requests & tokens.</span></span>

* <span data-ttu-id="d0176-127">Pokud chcete začít, otevřete řešení a přidání balíčků NuGet middleware OWIN projekt TodoListService pomocí konzoly Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="d0176-127">To begin, open the solution and add the OWIN middleware NuGet packages to the TodoListService project using the Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
PM> Install-Package Microsoft.IdentityModel.Protocol.Extensions -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a><span data-ttu-id="d0176-128">Konfigurace ověřování OAuth</span><span class="sxs-lookup"><span data-stu-id="d0176-128">Configure OAuth authentication</span></span>
* <span data-ttu-id="d0176-129">Přidejte třídu OWIN při spuštění do projektu TodoListService názvem `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="d0176-129">Add an OWIN Startup class to the TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="d0176-130">Klikněte pravým tlačítkem na projekt--> **přidat** --> **nová položka** --> vyhledejte "OWIN".</span><span class="sxs-lookup"><span data-stu-id="d0176-130">Right click on the project --> **Add** --> **New Item** --> Search for “OWIN”.</span></span>  <span data-ttu-id="d0176-131">Middleware OWIN při spuštění vaší aplikace vyvolá metodu `Configuration(…)`.</span><span class="sxs-lookup"><span data-stu-id="d0176-131">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>
* <span data-ttu-id="d0176-132">Změňte deklaraci třídy k `public partial class Startup` -již implementovali jsme součástí této třídy pro vás v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="d0176-132">Change the class declaration to `public partial class Startup` - we’ve already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="d0176-133">V `Configuration(…)` metoda, zkontrolujte zavolá ConfgureAuth(...) nastavení ověřování pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d0176-133">In the `Configuration(…)` method, make a call to ConfgureAuth(…) to set up authentication for your web app.</span></span>

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* <span data-ttu-id="d0176-134">Otevřete soubor `App_Start\Startup.Auth.cs` a implementovat `ConfigureAuth(…)` metoda, která bude přijímat tokeny z koncového bodu v2.0 nastavení webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d0176-134">Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(…)` method, which will set up the Web API to accept tokens from the v2.0 endpoint.</span></span>

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

* <span data-ttu-id="d0176-135">Nyní můžete pomocí `[Authorize]` atributy chránit řadiče a akce s ověřování nosiče OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="d0176-135">Now you can use `[Authorize]` attributes to protect your controllers and actions with OAuth 2.0 bearer authentication.</span></span>  <span data-ttu-id="d0176-136">Uspořádání `Controllers\TodoListController.cs` se značky autorizovat.</span><span class="sxs-lookup"><span data-stu-id="d0176-136">Decorate the `Controllers\TodoListController.cs` class with an authorize tag.</span></span>  <span data-ttu-id="d0176-137">Tato akce vynutí uživatele k přihlášení před přístupem k této stránce.</span><span class="sxs-lookup"><span data-stu-id="d0176-137">This will force the user to sign in before accessing that page.</span></span>

```C#
[Authorize]
public class TodoListController : ApiController
{
```

* <span data-ttu-id="d0176-138">Když oprávnění volající úspěšně vyvolá jeden z `TodoListController` rozhraní API, akce může potřebovat přístup k informacím o volajícím.</span><span class="sxs-lookup"><span data-stu-id="d0176-138">When an authorized caller successfully invokes one of the `TodoListController` APIs, the action might need access to information about the caller.</span></span>  <span data-ttu-id="d0176-139">OWIN poskytuje přístup k deklarace identity uvnitř tokenu nosiče prostřednictvím `ClaimsPrincpal` objektu.</span><span class="sxs-lookup"><span data-stu-id="d0176-139">OWIN provides access to the claims inside the bearer token via the `ClaimsPrincpal` object.</span></span>  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

* <span data-ttu-id="d0176-140">Nakonec otevřete `web.config` souboru v kořenovém adresáři projektu TodoListService a zadejte svoje hodnoty konfigurace v `<appSettings>` oddílu.</span><span class="sxs-lookup"><span data-stu-id="d0176-140">Finally, open the `web.config` file in the root of the TodoListService project, and enter your configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="d0176-141">Vaše `ida:Audience` je **Id aplikace** aplikace, kterou jste zadali v portálu.</span><span class="sxs-lookup"><span data-stu-id="d0176-141">Your `ida:Audience` is the **Application Id** of the app that you entered in the portal.</span></span>

## <a name="configure-the-client-app"></a><span data-ttu-id="d0176-142">Nakonfigurovat klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="d0176-142">Configure the client app</span></span>
<span data-ttu-id="d0176-143">Než budete moct vidět službu seznamu úkolů v akci, budete muset nakonfigurovat klienta seznamu úkolů, můžete získat tokeny z koncového bodu v2.0 a provádět volání do služby.</span><span class="sxs-lookup"><span data-stu-id="d0176-143">Before you can see the Todo List Service in action, you need to configure the Todo List Client so it can get tokens from the v2.0 endpoint and make calls to the service.</span></span>

* <span data-ttu-id="d0176-144">Otevřete v projektu TodoListClient `App.config` a zadejte svoje hodnoty konfigurace v `<appSettings>` oddílu.</span><span class="sxs-lookup"><span data-stu-id="d0176-144">In the TodoListClient project, open `App.config` and enter your configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="d0176-145">Vaše `ida:ClientId` Id aplikace, které jste zkopírovali z portálu.</span><span class="sxs-lookup"><span data-stu-id="d0176-145">Your `ida:ClientId` Application Id you copied from the portal.</span></span>

<span data-ttu-id="d0176-146">Nakonec vyčistit, sestavte a spusťte každý projekt!</span><span class="sxs-lookup"><span data-stu-id="d0176-146">Finally, clean, build and run each project!</span></span>  <span data-ttu-id="d0176-147">Nyní máte rozhraní .NET MVC webové rozhraní API, které přijímá tokeny z obou osobní účty Microsoft a pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="d0176-147">You now have a .NET MVC Web API that accepts tokens from both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="d0176-148">Přihlaste se k TodoListClient a volání webového rozhraní api pro přidání úkolů do seznamu úkolů uživatele.</span><span class="sxs-lookup"><span data-stu-id="d0176-148">Sign into the TodoListClient, and call your web api to add tasks to the user's To-Do list.</span></span>

<span data-ttu-id="d0176-149">Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), nebo ji můžete klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="d0176-149">For reference, the completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="d0176-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0176-150">Next steps</span></span>
<span data-ttu-id="d0176-151">Nyní se můžete přesunout na další témata.</span><span class="sxs-lookup"><span data-stu-id="d0176-151">You can now move onto additional topics.</span></span>  <span data-ttu-id="d0176-152">Můžete se pokusit:</span><span class="sxs-lookup"><span data-stu-id="d0176-152">You may want to try:</span></span>

[<span data-ttu-id="d0176-153">Volání webového rozhraní API z webové aplikace >></span><span class="sxs-lookup"><span data-stu-id="d0176-153">Calling a Web API from a Web App >></span></span>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

<span data-ttu-id="d0176-154">Další zdroje projděte si:</span><span class="sxs-lookup"><span data-stu-id="d0176-154">For additional resources, check out:</span></span>

* [<span data-ttu-id="d0176-155">Příručka vývojáře v2.0 >></span><span class="sxs-lookup"><span data-stu-id="d0176-155">The v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="d0176-156">Značka StackOverflow "azure-active-directory" >></span><span class="sxs-lookup"><span data-stu-id="d0176-156">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="d0176-157">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="d0176-157">Get security updates for our products</span></span>
<span data-ttu-id="d0176-158">Doporučujeme vám získávat oznámení o bezpečnostních incidentech tak, že navštívíte [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlásíte se k odběru služby Security Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="d0176-158">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>
