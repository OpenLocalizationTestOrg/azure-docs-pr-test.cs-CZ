---
title: aaaAzure Active Directory B2C | Microsoft Docs
description: "Jak profil toobuild webové aplikace, která má registrace-množství nebo přihlášení, úpravy a resetování hesla pomocí Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: barbaraselden
ms.assetid: 30261336-d7a5-4a6d-8c1a-7943ad76ed25
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: 187f99a8dd50d212de4f0517f552cdbbe5a8edf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a>Vytvoření webové aplikace ASP.NET s Azure Active Directory B2C profil registrace, přihlášení, úpravy a resetování hesla

V tomto kurzu získáte informace o následujících postupech:

> [!div class="checklist"]
> * Přidání Azure AD B2C identity funkce tooyour webové aplikace
> * Registrace vaší webové aplikace ve vašem adresáři Azure AD B2C
> * Vytvoření uživatele registrace-množství nebo přihlášení, upravit profil a zásady resetování hesel pro vaši webovou aplikaci

## <a name="prerequisites"></a>Požadavky

- Je nutné připojit vašeho klienta B2C tooan účet Azure. Můžete vytvořit bezplatný účet Azure [zde](https://azure.microsoft.com/en-us/).
- Je třeba [Microsoft Visual Studio](https://www.visualstudio.com/) nebo podobné programu tooview a upravit hello ukázkový kód.

## <a name="create-an-azure-ad-b2c-directory"></a>Vytvoření adresáře Azure AD B2C

Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta. Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další. Pokud nemáte již, vytvořte adresář B2C, než budete pokračovat v této příručce.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> Je nutné tooconnect hello klienta B2C tooyour předplatného Azure. Po výběru **vytvořit**, vyberte hello **toomy odkaz existující Azure AD B2C klienta předplatného Azure** možnost a potom v hello **klienta Azure AD B2C** rozevírací nabídku, vyberte Chcete-li tooassociate Hello klienta.

## <a name="create-and-register-an-application"></a>Vytvoření a registrace aplikace

V dalším kroku třeba toocreate a zaregistrujte hello aplikace ve svém adresáři B2C. Obsahuje informace, že Azure AD B2C musí toosecurely komunikaci s vaší aplikací. 

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

Až skončíte, bude mít rozhraní API a nativní aplikace v nastavení aplikace.

## <a name="create-policies-on-your-b2c-tenant"></a>Vytvořit zásady na svého klienta B2C

V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Tato ukázka kódu obsahuje tři činnosti identity: **registrace a přihlášení**, **profilu upravit**, a **resetování hesla**.  Je třeba jedna zásada toocreate každého typu, jak je popsáno v hello [článku o zásadách](active-directory-b2c-reference-policies.md). Pro každou zásadu být jisti tooselect hello zobrazovaný název atributu nebo deklarace identity a toocopy dolů hello název vaší zásady pro pozdější použití.

### <a name="add-your-identity-providers"></a>Přidat vaši zprostředkovatelé identity

Nastavení, vyberte **zprostředkovatelů Identity** a zvolte registrace uživatelského jména nebo e-mailu registrace.

### <a name="create-a-sign-up-and-sign-in-policy"></a>Vytvoření zásad registrace a přihlášení

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a>Vytvoření profilu úpravy zásad

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a>Vytvoření zásady resetování hesel

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

Po vytvoření zásad jste připravené toobuild vaší aplikace.

## <a name="download-hello-sample-code"></a>Stáhněte si ukázkový kód hello

Hello kódu pro účely tohoto kurzu je udržovaný na [Githubu](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). Ukázka hello může klonovat spuštěním:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Po stažení ukázkového kódu hello hello otevřete Visual Studio .sln souboru tooget spustit. Hello soubor řešení obsahuje dva projekty: `TaskWebApp` a `TaskService`. `TaskWebApp`je hello webové aplikace MVC, která hello uživatel komunikuje se službou. `TaskService`je back endové webové rozhraní API hello aplikace, které ukládá seznam úkolů každého uživatele. V tomto článku pouze zabývat hello `TaskWebApp` aplikace. toolearn jak toobuild `TaskService` pomocí Azure AD B2C, najdete v tématu [naše kurz webové rozhraní api .NET](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="update-code-toouse-your-tenant-and-policies"></a>Aktualizovat toouse kód klienta a zásad

Naše ukázka je ID zásad a klienta hello nakonfigurované toouse naše ukázka klienta. tooconnect ho tooyour vlastní klienta, je nutné tooopen `web.config` v hello `TaskWebApp` projektu a nahraďte hello následující hodnoty:

* `ida:Tenant` názvem vašeho tenanta
* `ida:ClientId` identifikátorem webového aplikace
* `ida:ClientSecret` tajným klíčem webové aplikace
* `ida:SignUpSignInPolicyId` názvem zásady registrace/přihlášení
* `ida:EditProfilePolicyId` názvem zásady pro úpravu profilu
* `ida:ResetPasswordPolicyId` názvem zásady pro resetování hesla

## <a name="launch-hello-app"></a>Spusťte aplikaci hello
Z Visual Studia, spusťte aplikaci hello. Přejděte toohello kartě seznam úkolů a Poznámka hello je adresa URl: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*& client_id = *YourclientID*...

Zaregistrujte se pro aplikaci hello pomocí e-mailové adresy nebo uživatelské jméno. Odhlásit se, potom znovu přihlásit a upravit profil hello nebo resetování hesla hello. Odhlásit se a přihlaste se jako jiný uživatel. 

## <a name="add-social-idps"></a>Přidat sociálních IDPs

V současné době podporuje pouze uživatel registrace a přihlášení pomocí aplikace hello **místní účty**; uložené v adresáři B2C účty, které používají uživatelské jméno a heslo. Pomocí Azure AD B2C můžete přidat podporu pro ostatní **zprostředkovatelů identity** (IDPs) beze změny některé z vašeho kódu.

tooadd sociálních IDPs tooyour aplikace, začněte tím, že následující hello podrobné pokyny v těchto článcích. Pro každé rozšíření IDP chcete toosupport, potřebujete tooregister aplikace v daném systému a získat ID klienta.

* [Nastavení sítě Facebook jako IDP](active-directory-b2c-setup-fb-app.md)
* [Nastavit Google jako IDP](active-directory-b2c-setup-goog-app.md)
* [Nastavit Amazon jako IDP](active-directory-b2c-setup-amzn-app.md)
* [Nastavit LinkedIn jako IDP](active-directory-b2c-setup-li-app.md)

Po přidání tooyour zprostředkovatelé identity hello adresáři B2C, upravit každý vaší tři zásady tooinclude hello nové IDPs, jak je popsáno v hello [článku o zásadách](active-directory-b2c-reference-policies.md). Po uložení zásad hello a znovu spusťte aplikaci.  Měli byste vidět hello přidají nové IDPs jako přihlášení a odhlášení možnosti v každé z vaší činnosti identity.

Můžete experimentovat s vašimi zásadami a sledovat hello vliv na vaši ukázkovou aplikaci. Přidat nebo odebrat IDPs, pracovat s deklarace identity aplikace nebo změnit atributy registrace. Experiment, dokud se nezobrazí, jak zásady, žádosti o ověření a OWIN tie společně.

## <a name="sample-code-walkthrough"></a>Ukázka kódu návod
Konfigurace kódu aplikace hello ukázka zobrazit Hello následující části. Může to použijte jako vodítko při vývoji vaší budoucí aplikace.

### <a name="add-authentication-support"></a>Přidat podporu ověřování

Teď můžete konfigurovat toouse vaše aplikace Azure AD B2C. Aplikace komunikuje se službou Azure AD B2C odesláním OpenID Connect žádosti o ověření. Hello požadavky stanovují, hello uživatelské prostředí aplikace chce tooexecute zadáním hello zásad. Můžete použít tyto požadavky společnosti Microsoft OWIN knihovny toosend, provést zásady, spravovat relace uživatele a další.

#### <a name="install-owin"></a>Instalace OWIN

toobegin, přidejte hello OWIN middleware NuGet balíčky toohello projekt pomocí hello Konzola správce balíčků Visual Studio.

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

#### <a name="add-an-owin-startup-class"></a>Přidání třídy pro spuštění OWIN

Přidání třídy toohello spuštění OWIN rozhraní API volat `Startup.cs`.  Klikněte pravým tlačítkem na projekt hello, vyberte **přidat** a **nová položka**a poté vyhledejte OWIN. Hello OWIN middleware vyvolá hello `Configuration(…)` metoda při spuštění aplikace.

V našem ukázce jsme příliš změnili deklaraci třídy hello`public partial class Startup` a implementovat hello jiných součástí třídy hello v `App_Start\Startup.Auth.cs`. Uvnitř hello `Configuration` metoda, jsme přidali volání příliš`ConfigureAuth`, která je definována v `Startup.Auth.cs`. Po hello úpravy `Startup.cs` vypadá jako hello následující:

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

#### <a name="configure-hello-authentication-middleware"></a>Nakonfigurujte middleware ověřování hello

Soubor otevřete hello `App_Start\Startup.Auth.cs` a implementovat hello `ConfigureAuth(...)` metoda. Hello parametry, které zadáte v `OpenIdConnectAuthenticationOptions` sloužit jako souřadnice toocommunicate vaší aplikace s Azure AD B2C. Pokud některé parametry nezadáte, použije hello výchozí hodnotu. Například můžeme nezadávejte hello `ResponseType` v ukázce hello, takže hello výchozí hodnota `code id_token` se použije v každém odchozí požadavek tooAzure AD B2C.

Budete také potřebovat tooset až ověřování souborů cookie. middleware OpenID Connect Hello používá soubory cookie toomaintain uživatelské relace, mimo jiné.

```CSharp
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // Initialize variables ...

    // Configure hello OWIN middleware
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseCookieAuthentication(new CookieAuthenticationOptions());
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Generate hello metadata address using hello tenant and policy information
                MetadataAddress = String.Format(AadInstance, Tenant, DefaultPolicy),

                // These are standard OpenID Connect parameters, with values pulled from web.config
                ClientId = ClientId,
                RedirectUri = RedirectUri,
                PostLogoutRedirectUri = RedirectUri,

                // Specify hello callbacks for each type of notifications
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                    AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    AuthenticationFailed = OnAuthenticationFailed,
                },

                // Specify hello claims toovalidate
                TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name"
                },

                // Specify hello scope by appending all of hello scopes requested into one string (seperated by a blank space)
                Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement hello "Notification" methods...
}
```

V `OpenIdConnectAuthenticationOptions` vyšší, jsme definovali sadu funkce zpětného volání pro konkrétní oznámení, které jsou přijaty middlewarem OpenID Connect hello. Tyto chování jsou definovány pomocí `OpenIdConnectAuthenticationNotifications` objektu a uložena do hello `Notifications` proměnné. V našem ukázce jsme definovali tři různé zpětná volání v závislosti na hello událostí.

### <a name="using-different-policies"></a>Pomocí různých zásad

Hello `RedirectToIdentityProvider` oznámení se aktivuje vždy, když je vytvořena žádost tooAzure AD B2C. Ve funkci zpětného volání hello `OnRedirectToIdentityProvider`, jsme se změnami hello odchozí volání, pokud má být toouse jiné zásady. V pořadí toodo heslo resetovat, nebo upravte profil musíte toouse hello odpovídající zásada, například zásady místo výchozí hello "Registrace nebo přihlášení" zásady resetování hesel hello.

Ve výběru když uživatel chce tooreset hello heslo nebo upravte profil hello, přidáme hello zásad jsme dáváte přednost toouse do kontextu OWIN hello. To můžete udělat pomocí tohoto postupu hello následující:

```CSharp
    // Let hello middleware know you are trying toouse hello edit profile policy
    HttpContext.GetOwinContext().Set("Policy", EditProfilePolicyId);
```

A můžete implementovat funkce zpětného volání hello `OnRedirectToIdentityProvider` díky hello následující:

```CSharp
/*
*  On each call tooAzure AD B2C, check if a policy (e.g. hello profile edit or password reset policy) has been specified in hello OWIN context.
*  If so, use that policy when making hello call. Also, don't request a code (since it won't be needed).
*/
private Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    var policy = notification.OwinContext.Get<string>("Policy");

    if (!string.IsNullOrEmpty(policy) && !policy.Equals(DefaultPolicy))
    {
        notification.ProtocolMessage.Scope = OpenIdConnectScopes.OpenId;
        notification.ProtocolMessage.ResponseType = OpenIdConnectResponseTypes.IdToken;
        notification.ProtocolMessage.IssuerAddress = notification.ProtocolMessage.IssuerAddress.Replace(DefaultPolicy, policy);
    }

    return Task.FromResult(0);
}
```

### <a name="handling-authorization-codes"></a>Zpracování autorizačních kódů

Hello `AuthorizationCodeReceived` oznámení se aktivuje při přijetí autorizační kód. middleware OpenID Connect Hello nepodporuje výměnou kódy pro tokeny přístupu. Můžete ručně vyměnit hello kód pro token hello ve funkci zpětného volání. Další informace, podívejte se prosím na hello [dokumentace](active-directory-b2c-devquickstarts-web-api-dotnet.md) to vysvětluje, jak.

### <a name="handling-errors"></a>Zpracování chyb

Hello `AuthenticationFailed` oznámení se aktivuje, když se ověřování nezdaří. V jeho metoda zpětného volání může zpracovávat chyby hello podle potřeby. Měli byste ale přidat kontrolu pro kód chyby hello `AADB2C90118`. Během provádění hello hello "Registrace nebo přihlášení" zásady, má uživatel hello hello příležitost tooselect **zapomněli jste heslo?** odkaz. V takovém případě Azure AD B2C odešle aplikace této chyby kódu oznamující, že vaše aplikace by měl provádět žádost místo toho použít zásady resetování hesel hello.

```CSharp
/*
* Catch any failures received by hello authentication middleware and handle appropriately
*/
private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    // Handle hello error code that Azure AD B2C throws when trying tooreset a password from hello login page
    // because password reset is not supported by a "sign-up or sign-in policy"
    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If hello user clicked hello reset password link, redirect toohello reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```

### <a name="send-authentication-requests-tooazure-ad"></a>Odeslání žádosti o tooAzure ověřování AD

Aplikace je nyní správně nakonfigurované toocommunicate s Azure AD B2C pomocí ověřovacího protokolu OpenID Connect hello. OWIN spravuje hello podrobnosti o věnujte zpráv ověřování, ověřování tokenů z Azure AD B2C a údržbě uživatelské relace. Všechno, co zůstane je tooinitiate toku každého uživatele.

Když uživatel vybere **až přihlášení nebo přihlášení**, **upravit profil**, nebo **resetovat heslo** hello webové aplikace hello přidružené akce je volána v `Controllers\AccountController.cs`:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign up or sign in
*/
public void SignUpSignIn()
{
    // Use hello default policy tooprocess hello sign up / sign in flow
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge();
        return;
    }

    Response.Redirect("/");
}

/*
*  Called when requesting tooedit a profile
*/
public void EditProfile()
{
    if (Request.IsAuthenticated)
    {
        // Let hello middleware know you are trying toouse hello edit profile policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
        HttpContext.GetOwinContext().Set("Policy", Startup.EditProfilePolicyId);

        // Set hello page tooredirect tooafter editing hello profile
        var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
        HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

        return;
    }

    Response.Redirect("/");

}

/*
*  Called when requesting tooreset a password
*/
public void ResetPassword()
{
    // Let hello middleware know you are trying toouse hello reset password policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
    HttpContext.GetOwinContext().Set("Policy", Startup.ResetPasswordPolicyId);

    // Set hello page tooredirect tooafter changing passwords
    var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
    HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

    return;
}
```

Můžete také použít OWIN toosign out hello uživatele z aplikace hello. V `Controllers\AccountController.cs` máme:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign out
*/
public void SignOut()
{
    // toosign out hello user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

V přidání tooexplicitly vyvolání zásady, můžete použít `[Authorize]` značky v řadičích, které provádí zásadu, pokud hello uživatel není přihlášený. Otevřete `Controllers\HomeController.cs` a přidejte hello `[Authorize]` značky toohello deklarací řadiče.  OWIN vybere hello poslední zásady nakonfigurované při hello `[Authorize]` je dosáhl značky.

```CSharp
// Controllers\HomeController.cs

// You can use hello Authorize decorator tooexecute a certain policy if hello user is not already signed into hello app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a>Zobrazit informace o uživateli

Při ověřování uživatele pomocí OpenID Connect, Azure AD B2C vrátí ID tokenu toohello aplikaci, která obsahuje **deklarace identity**. Toto jsou tvrzení o hello uživatele. Můžete použít deklarace identity toopersonalize vaší aplikace.

Otevřete hello `Controllers\HomeController.cs` souboru. Dostanete deklarace identity uživatelů v řadičích prostřednictvím hello `ClaimsPrincipal.Current` zaregistrovaný objekt zabezpečení.

```CSharp
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Dostanete všechny deklarace identity, aby vaše aplikace přijímá v hello stejný způsobem.  Seznam všech deklarací identity hello hello aplikace obdrží je k dispozici pro vás na hello **deklarace identity** stránky.
