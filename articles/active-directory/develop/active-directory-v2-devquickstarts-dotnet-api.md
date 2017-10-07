---
title: "pomocí přihlášení tooa webového rozhraní API .NET MVC aaaAdd hello koncového bodu v2.0 Azure AD | Microsoft Docs"
description: "Jak toobuild .NET MVC webové rozhraní Api, které přijímá tokeny z Account Microsoft i osobní a pracovní nebo školní účty."
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
ms.openlocfilehash: 4e517145422bb6e9368e82a7eef4a5c57cce530a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-an-mvc-web-api"></a><span data-ttu-id="266c5-103">Zabezpečení webového rozhraní API MVC</span><span class="sxs-lookup"><span data-stu-id="266c5-103">Secure an MVC web API</span></span>
<span data-ttu-id="266c5-104">S koncovým bodem v2.0 hello Azure Active Directory, budete moci chránit webového rozhraní API pomocí [OAuth 2.0](active-directory-v2-protocols.md) přístupových tokenů, povolení uživatelé s i osobní účet Microsoft a pracovní nebo školní účty toosecurely přístup vašeho webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="266c5-104">With Azure Active Directory hello v2.0 endpoint, you can protect a Web API using [OAuth 2.0](active-directory-v2-protocols.md) access tokens, enabling users with both personal Microsoft account and work or school accounts toosecurely access your Web API.</span></span>

> [!NOTE]
> <span data-ttu-id="266c5-105">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="266c5-105">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="266c5-106">toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="266c5-106">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

<span data-ttu-id="266c5-107">V rozhraní ASP.NET web API můžete to provést pomocí middlewaru OWIN společnosti Microsoft, zahrnutá v rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="266c5-107">In ASP.NET web APIs, you can accomplish this using Microsoft’s OWIN middleware included in .NET Framework 4.5.</span></span>  <span data-ttu-id="266c5-108">Zde použijeme toobuild OWIN Web API MVC "tooDo seznamu", která umožňuje klientům úlohy toocreate a čtení ze seznamu úkolů uživatele.</span><span class="sxs-lookup"><span data-stu-id="266c5-108">Here we’ll use OWIN toobuild a "tooDo List" MVC Web API that allows clients toocreate and read tasks from a user's To-Do list.</span></span>  <span data-ttu-id="266c5-109">Hello webového rozhraní API ověří, že příchozí požadavky obsahovat platné přístupový token a odmítnout všechny požadavky, které nepředávejte ověření na chráněných trase.</span><span class="sxs-lookup"><span data-stu-id="266c5-109">hello web API will validate that incoming requests contain a valid access token and reject any requests that do not pass validation on a protected route.</span></span>  <span data-ttu-id="266c5-110">Tato ukázka byla vytvořená s využitím sady Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="266c5-110">This sample was built using Visual Studio 2015.</span></span>

## <a name="download"></a><span data-ttu-id="266c5-111">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="266c5-111">Download</span></span>
<span data-ttu-id="266c5-112">Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span><span class="sxs-lookup"><span data-stu-id="266c5-112">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span></span>  <span data-ttu-id="266c5-113">toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) nebo hello kostru klonovat:</span><span class="sxs-lookup"><span data-stu-id="266c5-113">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

<span data-ttu-id="266c5-114">kostru aplikace Hello zahrnuje všechny hello často používaný kód pro jednoduché rozhraní API, ale chybí všechny součásti související s identity hello.</span><span class="sxs-lookup"><span data-stu-id="266c5-114">hello skeleton app includes all hello boilerplate code for a simple API, but is missing all of hello identity-related pieces.</span></span> <span data-ttu-id="266c5-115">Pokud nechcete, aby toofollow společně, můžete místo toho klonovat nebo [stažení ukázky hello Dokončit](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="266c5-115">If you don't want toofollow along, you can instead clone or [download hello completed sample](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span></span>

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a><span data-ttu-id="266c5-116">Registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="266c5-116">Register an app</span></span>
<span data-ttu-id="266c5-117">Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="266c5-117">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="266c5-118">Zkontrolujte, že:</span><span class="sxs-lookup"><span data-stu-id="266c5-118">Make sure to:</span></span>

* <span data-ttu-id="266c5-119">Poznamenejte hello **Id aplikace** přiřazen tooyour aplikace, budete ho potřebovat brzy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="266c5-119">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>

<span data-ttu-id="266c5-120">Toto řešení sady visual studio také obsahuje "TodoListClient", což je jednoduchou aplikaci WPF.</span><span class="sxs-lookup"><span data-stu-id="266c5-120">This visual studio solution also contains a "TodoListClient", which is a simple WPF app.</span></span>  <span data-ttu-id="266c5-121">Hello TodoListClient je použité toodemonstrate jak uživatel přihlásí, a jak můžete vydat klient požaduje tooyour webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="266c5-121">hello TodoListClient is used toodemonstrate how a user signs-in and how a client can issue requests tooyour Web API.</span></span>  <span data-ttu-id="266c5-122">V takovém případě hello TodoListClient i hello TodoListService jsou reprezentované pomocí hello stejná aplikace.</span><span class="sxs-lookup"><span data-stu-id="266c5-122">In this case, both hello TodoListClient and hello TodoListService are represented by hello same app.</span></span>  <span data-ttu-id="266c5-123">tooconfigure hello TodoListClient, měli byste také:</span><span class="sxs-lookup"><span data-stu-id="266c5-123">tooconfigure hello TodoListClient, you should also:</span></span>

* <span data-ttu-id="266c5-124">Přidat hello **Mobile** platformu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="266c5-124">Add hello **Mobile** platform for your app.</span></span>

## <a name="install-owin"></a><span data-ttu-id="266c5-125">Instalace OWIN</span><span class="sxs-lookup"><span data-stu-id="266c5-125">Install OWIN</span></span>
<span data-ttu-id="266c5-126">Teď, když jste registrováni aplikace, musíte tooset až toocommunicate vaší aplikace s koncovým bodem v2.0 hello v pořadí toovalidate příchozí žádosti a tokeny.</span><span class="sxs-lookup"><span data-stu-id="266c5-126">Now that you’ve registered an app, you need tooset up your app toocommunicate with hello v2.0 endpoint in order toovalidate incoming requests & tokens.</span></span>

* <span data-ttu-id="266c5-127">toobegin, otevřete řešení hello a přidejte hello OWIN middleware NuGet balíčky toohello TodoListService projekt pomocí hello Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="266c5-127">toobegin, open hello solution and add hello OWIN middleware NuGet packages toohello TodoListService project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
PM> Install-Package Microsoft.IdentityModel.Protocol.Extensions -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a><span data-ttu-id="266c5-128">Konfigurace ověřování OAuth</span><span class="sxs-lookup"><span data-stu-id="266c5-128">Configure OAuth authentication</span></span>
* <span data-ttu-id="266c5-129">Přidat OWIN při spuštění třída toohello TodoListService projekt s názvem `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="266c5-129">Add an OWIN Startup class toohello TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="266c5-130">Klikněte pravým tlačítkem na projekt hello--> **přidat** --> **nová položka** --> vyhledejte "OWIN".</span><span class="sxs-lookup"><span data-stu-id="266c5-130">Right click on hello project --> **Add** --> **New Item** --> Search for “OWIN”.</span></span>  <span data-ttu-id="266c5-131">Hello OWIN middleware vyvolá hello `Configuration(…)` metoda při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="266c5-131">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>
* <span data-ttu-id="266c5-132">Změňte deklaraci třídy hello příliš`public partial class Startup` -již implementovali jsme součástí této třídy pro vás v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="266c5-132">Change hello class declaration too`public partial class Startup` - we’ve already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="266c5-133">V hello `Configuration(…)` metoda, ujistěte se, volání tooConfgureAuth(...) tooset až ověřování pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="266c5-133">In hello `Configuration(…)` method, make a call tooConfgureAuth(…) tooset up authentication for your web app.</span></span>

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* <span data-ttu-id="266c5-134">Soubor otevřete hello `App_Start\Startup.Auth.cs` a implementovat hello `ConfigureAuth(…)` metodu, která bude nastavit hello webového rozhraní API tooaccept tokeny z koncového bodu v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="266c5-134">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(…)` method, which will set up hello Web API tooaccept tokens from hello v2.0 endpoint.</span></span>

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, hello TodoListClient and TodoListService
                // are represented using hello same Application Id - we use
                // hello Application Id toorepresent hello audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that hello user's organization (if applicable) has
                // signed up for hello app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up hello OWIN pipeline toouse OAuth 2.0 Bearer authentication.
        // hello options provided here tell hello middleware about hello type of tokens
        // that will be recieved, which are JWTs for hello v2.0 endpoint.

        // NOTE: hello usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by hello v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used toofetch & use hello OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

* <span data-ttu-id="266c5-135">Nyní můžete pomocí `[Authorize]` atributy tooprotect řadiče a akce s ověřování nosiče OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="266c5-135">Now you can use `[Authorize]` attributes tooprotect your controllers and actions with OAuth 2.0 bearer authentication.</span></span>  <span data-ttu-id="266c5-136">Uspořádání hello `Controllers\TodoListController.cs` se značky autorizovat.</span><span class="sxs-lookup"><span data-stu-id="266c5-136">Decorate hello `Controllers\TodoListController.cs` class with an authorize tag.</span></span>  <span data-ttu-id="266c5-137">Tato akce vynutí hello toosign uživatele v před přístupem k této stránce.</span><span class="sxs-lookup"><span data-stu-id="266c5-137">This will force hello user toosign in before accessing that page.</span></span>

```C#
[Authorize]
public class TodoListController : ApiController
{
```

* <span data-ttu-id="266c5-138">Když oprávnění volající úspěšně vyvolá jeden hello `TodoListController` rozhraní API, hello akce může potřebovat přístup k tooinformation o hello volajícího.</span><span class="sxs-lookup"><span data-stu-id="266c5-138">When an authorized caller successfully invokes one of hello `TodoListController` APIs, hello action might need access tooinformation about hello caller.</span></span>  <span data-ttu-id="266c5-139">OWIN poskytuje přístup toohello deklarace identity uvnitř tokenu nosiče hello prostřednictvím hello `ClaimsPrincpal` objektu.</span><span class="sxs-lookup"><span data-stu-id="266c5-139">OWIN provides access toohello claims inside hello bearer token via hello `ClaimsPrincpal` object.</span></span>  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use hello ClaimsPrincipal tooaccess information about the
    // user making hello call.  In this case, we use hello 'sub' or
    // NameIdentifier claim tooserve as a key for hello tasks in hello data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

* <span data-ttu-id="266c5-140">Nakonec otevřete hello `web.config` souboru v kořenovém hello hello TodoListService projektu a zadejte hodnoty konfigurace v hello `<appSettings>` části.</span><span class="sxs-lookup"><span data-stu-id="266c5-140">Finally, open hello `web.config` file in hello root of hello TodoListService project, and enter your configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="266c5-141">Vaše `ida:Audience` je hello **Id aplikace** hello aplikace, kterou jste zadali v portálu hello.</span><span class="sxs-lookup"><span data-stu-id="266c5-141">Your `ida:Audience` is hello **Application Id** of hello app that you entered in hello portal.</span></span>

## <a name="configure-hello-client-app"></a><span data-ttu-id="266c5-142">Konfigurace klienta aplikace hello</span><span class="sxs-lookup"><span data-stu-id="266c5-142">Configure hello client app</span></span>
<span data-ttu-id="266c5-143">Než budete moct vidět hello služby seznamu úkolů v akci, je nutné tooconfigure hello klienta seznamu úkolů, můžete získat tokeny z koncového bodu v2.0 hello a ujistěte se, volání toohello služby.</span><span class="sxs-lookup"><span data-stu-id="266c5-143">Before you can see hello Todo List Service in action, you need tooconfigure hello Todo List Client so it can get tokens from hello v2.0 endpoint and make calls toohello service.</span></span>

* <span data-ttu-id="266c5-144">V projektu TodoListClient hello otevřete `App.config` a zadejte hodnoty konfigurace v hello `<appSettings>` části.</span><span class="sxs-lookup"><span data-stu-id="266c5-144">In hello TodoListClient project, open `App.config` and enter your configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="266c5-145">Vaše `ida:ClientId` Id aplikace, které jste zkopírovali z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="266c5-145">Your `ida:ClientId` Application Id you copied from hello portal.</span></span>

<span data-ttu-id="266c5-146">Nakonec vyčistit, sestavte a spusťte každý projekt!</span><span class="sxs-lookup"><span data-stu-id="266c5-146">Finally, clean, build and run each project!</span></span>  <span data-ttu-id="266c5-147">Nyní máte rozhraní .NET MVC webové rozhraní API, které přijímá tokeny z obou osobní účty Microsoft a pracovní nebo školní účty.</span><span class="sxs-lookup"><span data-stu-id="266c5-147">You now have a .NET MVC Web API that accepts tokens from both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="266c5-148">Přihlaste se k hello TodoListClient a volání webového rozhraní api tooadd úlohy toohello uživatele na seznam úkolů.</span><span class="sxs-lookup"><span data-stu-id="266c5-148">Sign into hello TodoListClient, and call your web api tooadd tasks toohello user's To-Do list.</span></span>

<span data-ttu-id="266c5-149">Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), nebo ji můžete klonovat z Githubu:</span><span class="sxs-lookup"><span data-stu-id="266c5-149">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="266c5-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="266c5-150">Next steps</span></span>
<span data-ttu-id="266c5-151">Nyní se můžete přesunout na další témata.</span><span class="sxs-lookup"><span data-stu-id="266c5-151">You can now move onto additional topics.</span></span>  <span data-ttu-id="266c5-152">Může být vhodné tootry:</span><span class="sxs-lookup"><span data-stu-id="266c5-152">You may want tootry:</span></span>

[<span data-ttu-id="266c5-153">Volání webového rozhraní API z webové aplikace >></span><span class="sxs-lookup"><span data-stu-id="266c5-153">Calling a Web API from a Web App >></span></span>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

<span data-ttu-id="266c5-154">Další zdroje projděte si:</span><span class="sxs-lookup"><span data-stu-id="266c5-154">For additional resources, check out:</span></span>

* [<span data-ttu-id="266c5-155">Příručka vývojáře v2.0 Hello >></span><span class="sxs-lookup"><span data-stu-id="266c5-155">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="266c5-156">Značka StackOverflow "azure-active-directory" >></span><span class="sxs-lookup"><span data-stu-id="266c5-156">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="266c5-157">Získejte bezpečnostní aktualizace našich produktů</span><span class="sxs-lookup"><span data-stu-id="266c5-157">Get security updates for our products</span></span>
<span data-ttu-id="266c5-158">Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="266c5-158">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>
