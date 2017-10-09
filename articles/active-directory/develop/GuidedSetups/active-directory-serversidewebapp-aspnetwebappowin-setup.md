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
## <a name="set-up-your-project"></a>Nastavení projektu

Tato část ukazuje hello kroky tooinstall a nakonfiguruje kanál hello ověřování prostřednictvím middlewaru OWIN v projektu ASP.NET pomocí OpenID Connect. 

> Dáváte přednost toodownload projektu sady Visual Studio Tato ukázka místo? [Stažení projektu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) a přeskočit toohello [krok konfigurace](#create-an-application-express) ukázka kódu hello tooconfigure před provedením.

<!--start-collapse-->
> ### <a name="create-your-aspnet-project"></a>Vytvoření projektu ASP.NET

> 1. V sadě Visual Studio:`File` > `New` > `Project`<br/>
> 2. V části *Visual C# \Web*, vyberte `ASP.NET Web Application (.NET Framework)`.
> 3. Název aplikace a klikněte na tlačítko *OK*
> 4. Vyberte `Empty` a vyberte hello políčko tooadd `MVC` odkazy
<!--end-collapse-->

## <a name="add-authentication-components"></a>Přidat ověřování součásti

1. V sadě Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Přidat *balíčky NuGet middleware OWIN* zadáním následujících hello hello okna konzoly Správce balíčků:

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>O tyto knihovny

>Hello knihovny výše povolit jednotné přihlašování (SSO) prostřednictvím ověřování na základě souborů cookie pomocí OpenID Connect. Po dokončení ověření a hello token představující uživatele hello je odeslána tooyour aplikace, middlewaru OWIN, který vytvoří soubor cookie relace. Hello prohlížeč použije tento soubor cookie na následné žádosti tak hello uživatele nepotřebuje tooretype své heslo a je potřeba žádné další ověření.
<!--end-collapse-->

## <a name="configure-hello-authentication-pipeline"></a>Konfigurace hello ověřovacího kanálu
Hello kroky jsou použité toocreate ověřování middlewaru OWIN třída při spuštění tooconfigure OpenID Connect. Tato třída bude provedena automaticky při spuštění procesu služby IIS.

> Pokud nemá projektu `Startup.cs` souboru hello kořenové složky:<br/>
> 1. Klikněte pravým tlačítkem na projekt hello kořenové složky: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Název`Startup.cs`

> Zkontrolujte, zda vybranou třídu hello je třídy pro spuštění OWIN a není standardní C# třídu. To můžete ověřit kontrolou, pokud se zobrazí `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` výše hello oboru názvů.


1. Přidat *OWIN* a *Microsoft.IdentityModel* odkazuje příliš`Startup.cs`:

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
Třída při spuštění nahraďte hello kódu níže:
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
> ### <a name="more-information"></a>Další informace

> Hello parametry, které zadáte v *OpenIDConnectAuthenticationOptions* sloužit jako souřadnice hello toocommunicate aplikací s Azure AD. Protože middleware OpenID Connect hello používá soubory cookie hello pozadí, musíte taky tooset až ověřování souborů cookie jako kód hello výš ukazuje. Hello *atribut ValidateIssuer* hodnota informuje OpenIdConnect toonot omezit přístup tooone konkrétní organizace.
<!--end-collapse-->

