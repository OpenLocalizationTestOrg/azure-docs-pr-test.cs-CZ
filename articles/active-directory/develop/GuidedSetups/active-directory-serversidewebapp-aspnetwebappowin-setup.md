---
title: "aaaAzure AD v2 ASP.NET Web Server Začínáme - instalace | Microsoft Docs"
description: "Implementace přihlašování společnosti Microsoft na řešení technologie ASP.NET s tradiční webovou aplikací využívajících prohlížeč pomocí OpenID Connect standard"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: eadc59666557e9cd294e6e99391001120579144c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="set-up-your-project"></a><span data-ttu-id="e297d-103">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="e297d-103">Set up your project</span></span>

<span data-ttu-id="e297d-104">Tato část ukazuje hello kroky tooinstall a nakonfiguruje kanál hello ověřování prostřednictvím middlewaru OWIN v projektu ASP.NET pomocí OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="e297d-104">This section shows hello steps tooinstall and configure hello authentication pipeline via OWIN middleware on an ASP.NET project using OpenID Connect.</span></span> 

> <span data-ttu-id="e297d-105">Dáváte přednost toodownload projektu sady Visual Studio Tato ukázka místo?</span><span class="sxs-lookup"><span data-stu-id="e297d-105">Prefer toodownload this sample's Visual Studio project instead?</span></span> <span data-ttu-id="e297d-106">[Stažení projektu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) a přeskočit toohello [krok konfigurace](#create-an-application-express) ukázka kódu hello tooconfigure před provedením.</span><span class="sxs-lookup"><span data-stu-id="e297d-106">[Download a project](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) and skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing.</span></span>

<!--start-collapse-->
> ### <a name="create-your-aspnet-project"></a><span data-ttu-id="e297d-107">Vytvoření projektu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e297d-107">Create your ASP.NET project</span></span>

> 1. <span data-ttu-id="e297d-108">V sadě Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="e297d-108">In Visual Studio: `File` > `New` > `Project`</span></span><br/>
> 2. <span data-ttu-id="e297d-109">V části *Visual C# \Web*, vyberte `ASP.NET Web Application (.NET Framework)`.</span><span class="sxs-lookup"><span data-stu-id="e297d-109">Under *Visual C#\Web*, select `ASP.NET Web Application (.NET Framework)`.</span></span>
> 3. <span data-ttu-id="e297d-110">Název aplikace a klikněte na tlačítko *OK*</span><span class="sxs-lookup"><span data-stu-id="e297d-110">Name your application and click *OK*</span></span>
> 4. <span data-ttu-id="e297d-111">Vyberte `Empty` a vyberte hello políčko tooadd `MVC` odkazy</span><span class="sxs-lookup"><span data-stu-id="e297d-111">Select `Empty` and select hello checkbox tooadd `MVC` references</span></span>
<!--end-collapse-->

## <a name="add-authentication-components"></a><span data-ttu-id="e297d-112">Přidat ověřování součásti</span><span class="sxs-lookup"><span data-stu-id="e297d-112">Add authentication components</span></span>

1. <span data-ttu-id="e297d-113">V sadě Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`</span><span class="sxs-lookup"><span data-stu-id="e297d-113">In Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`</span></span>
2. <span data-ttu-id="e297d-114">Přidat *balíčky NuGet middleware OWIN* zadáním následujících hello hello okna konzoly Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="e297d-114">Add *OWIN middleware NuGet packages* by typing hello following in hello Package Manager Console window:</span></span>

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a><span data-ttu-id="e297d-115">O tyto knihovny</span><span class="sxs-lookup"><span data-stu-id="e297d-115">About these libraries</span></span>

><span data-ttu-id="e297d-116">Hello knihovny výše povolit jednotné přihlašování (SSO) prostřednictvím ověřování na základě souborů cookie pomocí OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="e297d-116">hello libraries above enable single sign-on (SSO) using OpenID Connect via cookie-based authentication.</span></span> <span data-ttu-id="e297d-117">Po dokončení ověření a hello token představující uživatele hello je odeslána tooyour aplikace, middlewaru OWIN, který vytvoří soubor cookie relace.</span><span class="sxs-lookup"><span data-stu-id="e297d-117">After authentication is completed and hello token representing hello user is sent tooyour application, OWIN middleware creates a session cookie.</span></span> <span data-ttu-id="e297d-118">Hello prohlížeč použije tento soubor cookie na následné žádosti tak hello uživatele nepotřebuje tooretype své heslo a je potřeba žádné další ověření.</span><span class="sxs-lookup"><span data-stu-id="e297d-118">hello browser then uses this cookie on subsequent requests so hello user doesn't need tooretype their password, and no additional verification is needed.</span></span>
<!--end-collapse-->

## <a name="configure-hello-authentication-pipeline"></a><span data-ttu-id="e297d-119">Konfigurace hello ověřovacího kanálu</span><span class="sxs-lookup"><span data-stu-id="e297d-119">Configure hello authentication pipeline</span></span>
<span data-ttu-id="e297d-120">Hello kroky jsou použité toocreate ověřování middlewaru OWIN třída při spuštění tooconfigure OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="e297d-120">hello steps below are used toocreate an OWIN middleware Startup Class tooconfigure OpenID Connect authentication.</span></span> <span data-ttu-id="e297d-121">Tato třída bude provedena automaticky při spuštění procesu služby IIS.</span><span class="sxs-lookup"><span data-stu-id="e297d-121">This class will be executed automatically when your IIS process starts.</span></span>

> <span data-ttu-id="e297d-122">Pokud nemá projektu `Startup.cs` souboru hello kořenové složky:</span><span class="sxs-lookup"><span data-stu-id="e297d-122">If your project doesn't have a `Startup.cs` file in hello root folder:</span></span><br/>
> 1. <span data-ttu-id="e297d-123">Klikněte pravým tlačítkem na projekt hello kořenové složky: >`Add` > `New Item...` > `OWIN Startup class`</span><span class="sxs-lookup"><span data-stu-id="e297d-123">Right click on hello project's root folder: >  `Add` > `New Item...` > `OWIN Startup class`</span></span><br/>
> 2. <span data-ttu-id="e297d-124">Název`Startup.cs`</span><span class="sxs-lookup"><span data-stu-id="e297d-124">Name it `Startup.cs`</span></span>

> <span data-ttu-id="e297d-125">Zkontrolujte, zda vybranou třídu hello je třídy pro spuštění OWIN a není standardní C# třídu.</span><span class="sxs-lookup"><span data-stu-id="e297d-125">Make sure hello class selected is an OWIN Startup Class and not a standard C# class.</span></span> <span data-ttu-id="e297d-126">To můžete ověřit kontrolou, pokud se zobrazí `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` výše hello oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="e297d-126">Confirm this by checking if you see `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` above hello namespace.</span></span>


1. <span data-ttu-id="e297d-127">Přidat *OWIN* a *Microsoft.IdentityModel* odkazuje příliš`Startup.cs`:</span><span class="sxs-lookup"><span data-stu-id="e297d-127">Add *OWIN* and *Microsoft.IdentityModel* references too`Startup.cs`:</span></span>

```csharp
using Microsoft.Owin;
using Owin;
using Microsoft.IdentityModel.Protocols;
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
using Microsoft.Owin.Security.Notifications;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="e297d-128">Třída při spuštění nahraďte hello kódu níže:</span><span class="sxs-lookup"><span data-stu-id="e297d-128">Replace Startup class with hello code below:</span></span>
</li>
</ol>

```csharp
public class Startup
{        
    // hello Client ID is used by hello application toouniquely identify itself tooAzure AD.
    string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

    // RedirectUri is hello URL where hello user will be redirected tooafter they sign in.
    string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

    // Tenant is hello tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
    static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

    // Authority is hello URL for authority, composed by Azure Active Directory v2 endpoint and hello tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
    string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

    /// <summary>
    /// Configure OWIN toouse OpenIdConnect 
    /// </summary>
    /// <param name="app"></param>
    public void Configuration(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());
            app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Sets hello ClientId, authority, RedirectUri as obtained from web.config
                ClientId = clientId,
                Authority = authority,
                RedirectUri = redirectUri,
                // PostLogoutRedirectUri is hello page that users will be redirected tooafter sign-out. In this case, it is using hello home page
                PostLogoutRedirectUri = redirectUri,
                Scope = OpenIdConnectScopes.OpenIdProfile,
                // ResponseType is set toorequest hello id_token - which contains basic information about hello signed-in user
                ResponseType = OpenIdConnectResponseTypes.IdToken,
                // ValidateIssuer set toofalse tooallow personal and work accounts from any organization toosign in tooyour application
                // tooonly allow users from a single organizations, set ValidateIssuer tootrue and 'tenant' setting in web.config toohello tenant name
                // tooallow users from only a list of specific organizations, set ValidateIssuer tootrue and use ValidIssuers parameter 
                TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters() { ValidateIssuer = false },
                // OpenIdConnectAuthenticationNotifications configures OWIN toosend notification of failed authentications tooOnAuthenticationFailed method
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    AuthenticationFailed = OnAuthenticationFailed
                }
            }
        );
    }

    /// <summary>
    /// Handle failed authentication requests by redirecting hello user toohello home page with an error in hello query string
    /// </summary>
    /// <param name="context"></param>
    /// <returns></returns>
    private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
    {
        context.HandleResponse();
        context.Response.Redirect("/?errormessage=" + context.Exception.Message);
        return Task.FromResult(0);
    }
}

```
<!--start-collapse-->
> ### <a name="more-information"></a><span data-ttu-id="e297d-129">Další informace</span><span class="sxs-lookup"><span data-stu-id="e297d-129">More Information</span></span>

> <span data-ttu-id="e297d-130">Hello parametry, které zadáte v *OpenIDConnectAuthenticationOptions* sloužit jako souřadnice hello toocommunicate aplikací s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e297d-130">hello parameters you provide in *OpenIDConnectAuthenticationOptions* serve as coordinates for hello application toocommunicate with Azure AD.</span></span> <span data-ttu-id="e297d-131">Protože middleware OpenID Connect hello používá soubory cookie hello pozadí, musíte taky tooset až ověřování souborů cookie jako kód hello výš ukazuje.</span><span class="sxs-lookup"><span data-stu-id="e297d-131">Because hello OpenID Connect middleware uses cookies in hello background, you also need tooset up cookie authentication as hello code above shows.</span></span> <span data-ttu-id="e297d-132">Hello *atribut ValidateIssuer* hodnota informuje OpenIdConnect toonot omezit přístup tooone konkrétní organizace.</span><span class="sxs-lookup"><span data-stu-id="e297d-132">hello *ValidateIssuer* value tells OpenIdConnect toonot restrict access tooone specific organization.</span></span>
<!--end-collapse-->

