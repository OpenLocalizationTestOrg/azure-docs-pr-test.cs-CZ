---
title: "aaaSession Management - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "způsoby zmírnění hrozeb v hello nástroj modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 915ffae3f775ca6902fcfb93e7e1952ce85612f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-session-management--articles"></a>Rámce zabezpečení: Správa relací | Články 
| Produktům a službám | Článek |
| --------------- | ------- |
| **Azure AD**    | <ul><li>[Implementace správné odhlášení pomocí ADAL metod při používání služby Azure AD](#logout-adal)</li></ul> |
| Zařízení IoT | <ul><li>[Použití omezené životnost generovaných tokenů SaS](#finite-tokens)</li></ul> |
| **Azure Documentdb** | <ul><li>[Pomocí minimální životnosti tokenu pro generovaných tokenů prostředků](#resource-tokens)</li></ul> |
| **ADFS** | <ul><li>[Implementace správné odhlášení metodami WsFederation při použití služby AD FS](#wsfederation-logout)</li></ul> |
| **Serveru identit** | <ul><li>[Implementace správné odhlášení při použití serveru identit](#proper-logout)</li></ul> |
| **Webové aplikace** | <ul><li>[Aplikace dostupné přes protokol HTTPS, musíte použít bezpečné soubory cookie](#https-secure-cookies)</li><li>[Všechny aplikace http, na základě by měl určovat pouze pro definici souboru cookie http](#cookie-definition)</li><li>[Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na webových stránkách ASP.NET](#csrf-asp)</li><li>[Nastavení relace dobu jeho existence nečinnosti](#inactivity-lifetime)</li><li>[Implementace správné odhlášení z aplikace hello](#proper-app-logout)</li></ul> |
| **Webové rozhraní API** | <ul><li>[Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na rozhraní ASP.NET Web API](#csrf-api)</li></ul> |

## <a id="logout-adal"></a>Implementace správné odhlášení pomocí ADAL metod při používání služby Azure AD

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure AD | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Pokud aplikace hello závisí na přístupový token vydán Azure AD, by měly volat obslužné rutiny události odhlášení hello |

### <a name="example"></a>Příklad
```C#
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a>Příklad
Relace uživatele ho měli zrušte také voláním metody Session.Abandon(). Následující metoda ukazuje zabezpečené implementací odhlášení uživatele:
```C#
    [HttpPost]
        [ValidateAntiForgeryToken]
        public void LogOff()
        {
            string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            AuthenticationContext authContext = new AuthenticationContext(Authority + TenantId, new NaiveSessionCache(userObjectID));
            authContext.TokenCache.Clear();
            Session.Clear();
            Session.Abandon();
            Response.SetCookie(new HttpCookie("ASP.NET_SessionId", string.Empty));
            HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
        } 
```

## <a id="finite-tokens"></a>Použití omezené životnost generovaných tokenů SaS

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Zařízení IoT | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Tokeny SaS vygenerované ověřování tooAzure IoT Hub by měl mít dobou vypršení platnosti. Zachovat hello SaS token životnosti tooa minimální toolimit hello časového intervalu, který může být přehrány v případě, že dojde k ohrožení hello tokeny.|

## <a id="resource-tokens"></a>Pomocí minimální životnosti tokenu pro generovaných tokenů prostředků

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Documentdb | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Snižte časový interval hello z prostředků tokenu tooa minimální požadovanou hodnotu. Prostředek tokeny mají platný časový interval výchozí 1 hodina.|

## <a id="wsfederation-logout"></a>Implementace správné odhlášení metodami WsFederation při použití služby AD FS

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | ADFS | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Pokud aplikace hello spoléhá na službu tokenů zabezpečení tokenu vystavený služby AD FS, obslužné rutiny události odhlášení hello by měly volat metoda toolog WSFederationAuthenticationModule.FederatedSignOut() out hello uživatele. Také hello aktuální relace musí být zničený, a hello relace tokenu hodnota by měla být resetovat a zrušeny.|

### <a name="example"></a>Příklad
```C#
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes hello user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at hello specified security token service (STS) by using hello WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of hello current session and raises hello appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at hello specified security token service (STS) by using hello WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <a id="proper-logout"></a>Implementace správné odhlášení při použití serveru identit

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Serveru identit | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Federované IdentityServer3 odhlášení](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| **Kroky** | IdentityServer podporuje možnost toofederate hello zprostředkovatelům externí identity. Když se uživatel odhlásí od zprostředkovatele identity nadřazeného, v závislosti na protokolu hello, může to být možné tooreceive oznámení při odhlášení uživatele hello. To umožňuje toonotify IdentityServer, které jeho klienty, lze také podepsat hello uživatele se. Podívejte se do dokumentace hello v části odkazy hello podrobnosti implementace hello.|

## <a id="https-secure-cookies"></a>Aplikace dostupné přes protokol HTTPS, musíte použít bezpečné soubory cookie

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | EnvironmentType - OnPrem |
| **Odkazy**              | [Element (schéma nastavení ASP.NET) httpCookies](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure vlastnost](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx) |
| **Kroky** | Soubory cookie jsou obvykle přístupné toohello domény jen pro kterou byly obor. Definice hello "domény" bohužel nezahrnuje protokol hello, takže soubory cookie, které jsou vytvořené pomocí protokolu HTTPS jsou přístupné přes protokol HTTP. Hello "zabezpečené" atribut uvádí, že toohello prohlížeč, který hello soubor cookie měl pouze být k dispozici prostřednictvím protokolu HTTPS. Zkontrolujte, jestli všechny soubory cookie nastavte přes protokol HTTPS, používají hello **zabezpečené** atribut. v souboru web.config hello nastavením hello requireSSL atribut tootrue lze vynutit Hello požadavek. Je hello upřednostňovaný způsob, protože vynutí hello **zabezpečené** atribut pro všechny soubory cookie aktuálních a budoucích bez nutnosti toomake hello změny další kód.|

### <a name="example"></a>Příklad
```C#
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
nastavení Hello se vynucuje, i když je aplikace hello použité tooaccess HTTP. Pokud se používá HTTP tooaccess hello aplikace, hello konců nastavení, které aplikace hello protože soubory cookie hello se nastavují s hello zabezpečené atribut a hello prohlížeče nebude jejich odeslání zpět toohello aplikace.

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Webové formuláře, MVC5 |
| **Atributy**              | EnvironmentType - OnPrem |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Pokud hello webové aplikace je hello předávající strany a hello IdP serverem služby AD FS, tokenu FedAuth hello zabezpečené atribut je možné nakonfigurovat pomocí nastavení tooTrue requireSSL v `system.identityModel.services` části souboru Web.config:|

### <a name="example"></a>Příklad
```C#
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <a id="cookie-definition"></a>Všechny aplikace http, na základě by měl určovat pouze pro definici souboru cookie http

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Zabezpečený soubor Cookie atribut](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| **Kroky** | toomitigate hello riziko zpřístupnění informací s útoku skriptování (XSS), nový atribut - httpOnly - byl přináší toocookies a podporuje všechny hlavní prohlížeče. Hello atribut určuje, že soubor cookie není přístupná prostřednictvím skriptu. Webovou aplikaci pomocí souborů cookie HttpOnly snižuje hello možnost, můžete důvěrné informace obsažené v souboru cookie hello odcizení pomocí skriptu a odeslat tooan útočník webu. |

### <a name="example"></a>Příklad
Všechny aplikace založené na protokolu HTTP, které používají soubory cookie by měl v definici hello souboru cookie, zadat HttpOnly implementací následující konfigurace v souboru web.config:
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Webové formuláře |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Vlastnost FormsAuthentication.RequireSSL](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| **Kroky** | Hello RequireSSL hodnota vlastnosti je nastavena v hello konfigurační soubor pro aplikaci ASP.NET pomocí hello requireSSL atribut elementu konfigurace hello. Zadaný v souboru Web.config hello pro vaši aplikaci ASP.NET zda SSL (Secure Sockets Layer) je požadovaná tooreturn hello ověřování založené na formulářích souboru cookie toohello server atributem requireSSL hello nastavení.|

### <a name="example"></a>Příklad 
Hello následující příklad kódu nastaví atribut hello requireSSL v souboru Web.config hello.
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC5 |
| **Atributy**              | EnvironmentType - OnPrem |
| **Odkazy**              | [Windows Identity Foundation (WIF) konfigurace – část II](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| **Kroky** | atribut httpOnly tooset pro soubory cookie FedAuth hideFromCsript hodnota atributu musí být nastavená tooTrue. |

### <a name="example"></a>Příklad
Následující konfigurace zobrazuje hello správnou konfiguraci:
```XML
<federatedAuthentication>
<cookieHandler mode="Custom"
                       hideFromScript="true"
                       name="FedAuth"
                       path="/"
                       requireSsl="true"
                       persistentSessionLifetime="25">
</cookieHandler>
</federatedAuthentication>
```

## <a id="csrf-asp"></a>Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na webových stránkách ASP.NET

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Padělání požadavku posílaného mezi weby (proti útokům CSRF nebo XSRF) je typ útoku, ve kterém může útočník provádět akce v kontextu zabezpečení hello navázanou relaci jiného uživatele na webovém serveru. cílem Hello je toomodify nebo odstraňovat obsah, pokud cílový webový server hello závisí výhradně na relaci soubory cookie tooauthenticate přijatý požadavek. Útočník by mohl zneužít tuto chybu zabezpečení tím, že získáme jiný uživatel prohlížeče tooload adresu URL pomocí příkazu z citlivé lokality, na kterém je hello uživatel již přihlášen. Existuje mnoho způsobů pro útočník toodo, že jako hostováním na jiný web, načte prostředek z citlivé server hello nebo získávání hello uživatele tooclick odkaz. Pokud hello server odešle klientovi další token toohello, vyžaduje hello tooinclude klienta ve všech budoucích požadavků a ověřuje, že všechny budoucí požadavky patří token, který se týká toohello aktuální relaci, například pomocí se dá zabránit útokům Hello použití hello ASP.NET AntiForgeryToken nebo stavu zobrazení. |

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC5 MVC6 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Prevence XSRF/proti útokům CSRF v architektuře ASP.NET MVC a webových stránek](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| **Kroky** | Anti-proti útokům CSRF a architektura ASP.NET MVC forms - použití hello `AntiForgeryToken` pomocnou metodu pro zobrazení; put `Html.AntiForgeryToken()` do hello formuláře, například|

### <a name="example"></a>Příklad
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a>Příklad
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Příklad
V hello stejný čas, Html.AntiForgeryToken() poskytuje hello návštěvníka __RequestVerificationToken s hello stejnou hodnotu jako hodnota hello náhodných skrytá výše uvedeném názvem souboru cookie. Dále toovalidate příchozí post formuláře, přidejte metodu akce cíl toohello filtru hello [ValidateAntiForgeryToken]. Například:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Filtr autorizace, který kontroluje, zda:
* Hello příchozí požadavek má souboru cookie s názvem __RequestVerificationToken
* Hello příchozí požadavek má `Request.Form` položka názvem __RequestVerificationToken
* Tyto soubory cookie a `Request.Form` za předpokladu, že všechny shody hodnoty je dobře, hello žádost prochází jako normální. Ale pokud ne, pak chybu autorizace se zprávou "požadovaný token proti padělání nebyl zadán nebo byl neplatný". 

### <a name="example"></a>Příklad
Ochrana proti útokům CSRF a AJAX: hello tokenu formuláře může být problém pro požadavky AJAX, protože požadavek AJAX může odesílat data JSON, ne data formuláře HTML. Jedno řešení je toosend hello tokeny ve vlastní hlavičku HTTP. Hello následující kód používá tokeny hello toogenerate syntaxe Razor a potom přidá požadavek AJAX tooan tokeny hello. 
```C#
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }

    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Příklad
Když při zpracování požadavku hello, extrahujte hello tokeny z hlavičky požadavku hello. Potom volejte metodu AntiForgery.Validate hello toovalidate hello tokeny. Hello metodu Validate vyvolá výjimku, pokud hello tokeny nejsou platné.
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Webové formuláře |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Využít výhod z ASP.NET integrované funkce tooFend vypnout útoků Web](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| **Kroky** | Můžete být omezeny útoku proti útokům CSRF v aplikacích webový formulář na základě nastavení tooa ViewStateUserKey náhodný se řetězec, který se liší pro jednotlivé uživatele – ID uživatele nebo lépe ještě ID relace. Pro počtu technická a sociálních důvodů ID relace je mnohem lepší umístit, protože relace na ID nepředvídatelným, časový limit a liší se na jednotlivé uživatele.|

### <a name="example"></a>Příklad
Tady je kód hello potřebujete toohave na všechny stránky:
```C#
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <a id="inactivity-lifetime"></a>Nastavení relace dobu jeho existence nečinnosti

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Vlastnost HttpSessionState.Timeout](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx) |
| **Kroky** | Časový limit relace představuje výskytu události hello, pokud uživatel neprovede žádnou akci na webovém serveru během intervalu (definovanou webový server). Dobrý den událost na straně serveru, změňte stav hello hello uživatelské relace too'invalid "(například" nepoužívá už") a dostane pokyn hello webového serveru toodestroy (odstranění všech dat obsažených do něj). Hello následující příklad kódu nastaví hello časový limit relace atributu too15 minut v souboru Web.config hello.|

### <a name="example"></a>Příklad
"" Kód XML <configuration> < system.web > <sessionState mode="InProc" cookieless="true" timeout="15" /> < /system.web ></configuration>
```

## <a id="threat-detection"></a>Enable Threat detection on Azure SQL
```

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Webové formuláře |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [forms Element pro ověřování (schéma nastavení ASP.NET)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx) |
| **Kroky** | Nastavení minut hello too15 časový limit souboru cookie lístek pro ověřování pomocí formulářů|

### <a name="example"></a>Příklad
"" Kód XML<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms>
```

| Title                   | Details      |
| ----------------------- | ------------ |
| **Component**               | Web Application | 
| **SDL Phase**               | Build |  
| **Applicable Technologies** | Web Forms, MVC5 |
| **Attributes**              | EnvironmentType - OnPrem |
| **References**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Steps** | When hello web application is Relying Party and ADFS is hello STS, hello lifetime of hello authentication cookies - FedAuth tokens - can be set by hello following configuration in web.config:|

### Example
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use hello code below tooenable encryption-decryption of claims received from ADFS. Thumbprint value varies based on hello certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a>Příklad
Hello služby AD FS vydán doba platnosti tokenu SAML deklarace by mělo být nastavené too15 minut také tak, že spustíte následující příkaz prostředí powershell na serveru služby AD FS hello hello:
```C#
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <a id="proper-app-logout"></a>Implementace správné odhlášení z aplikace hello

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Proveďte správné odhlásit z aplikace hello při stisknutí uživatele odhlášení tlačítko. Po odhlášení aplikace by měl destroy uživatelské relace a také resetování a nezruší se tím hodnota souboru cookie relace, spolu s obnovením a anulovány hodnota souboru cookie ověřování. Navíc pokud více relací jsou vázané tooa identity jednoho uživatele, se musí být souhrnně ukončena na straně serveru hello při vypršení časového limitu nebo odhlášení. Nakonec zkontrolujte, že odhlášení funkce je k dispozici na každé stránce. |

## <a id="csrf-api"></a>Zmírnění před útoky webů požadavku padělání (proti útokům CSRF) na rozhraní ASP.NET Web API

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Padělání požadavku posílaného mezi weby (proti útokům CSRF nebo XSRF) je typ útoku, ve kterém může útočník provádět akce v kontextu zabezpečení hello navázanou relaci jiného uživatele na webovém serveru. cílem Hello je toomodify nebo odstraňovat obsah, pokud cílový webový server hello závisí výhradně na relaci soubory cookie tooauthenticate přijatý požadavek. Útočník by mohl zneužít tuto chybu zabezpečení tím, že získáme jiný uživatel prohlížeče tooload adresu URL pomocí příkazu z citlivé lokality, na kterém je hello uživatel již přihlášen. Existuje mnoho způsobů pro útočník toodo, že jako hostováním na jiný web, načte prostředek z citlivé server hello nebo získávání hello uživatele tooclick odkaz. Pokud hello server odešle klientovi další token toohello, vyžaduje hello tooinclude klienta ve všech budoucích požadavků a ověřuje, že všechny budoucí požadavky patří token, který se týká toohello aktuální relaci, například pomocí se dá zabránit útokům Hello použití hello ASP.NET AntiForgeryToken nebo stavu zobrazení. |

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC5 MVC6 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Prevence útoků (proti útokům CSRF) padělání požadavku posílaného mezi weby v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| **Kroky** | Ochrana proti útokům CSRF a AJAX: hello tokenu formuláře může být problém pro požadavky AJAX, protože požadavek AJAX může odesílat data JSON, ne data formuláře HTML. Jedno řešení je toosend hello tokeny ve vlastní hlavičku HTTP. Hello následující kód používá tokeny hello toogenerate syntaxe Razor a potom přidá požadavek AJAX tooan tokeny hello. |

### <a name="example"></a>Příklad
```Javascript
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }
    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a>Příklad
Když při zpracování požadavku hello, extrahujte hello tokeny z hlavičky požadavku hello. Potom volejte metodu AntiForgery.Validate hello toovalidate hello tokeny. Hello metodu Validate vyvolá výjimku, pokud hello tokeny nejsou platné.
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

### <a name="example"></a>Příklad
Anti-proti útokům CSRF a formulářů rozhraní ASP.NET MVC – použití hello pomocnou metodu AntiForgeryToken na zobrazení; Html.AntiForgeryToken() vložena hello formuláře, například
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a>Příklad
výše uvedený příklad Hello výstup podobný hello následující:
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a>Příklad
V hello stejný čas, Html.AntiForgeryToken() poskytuje hello návštěvníka __RequestVerificationToken s hello stejnou hodnotu jako hodnota hello náhodných skrytá výše uvedeném názvem souboru cookie. Dále toovalidate příchozí post formuláře, přidejte metodu akce cíl toohello filtru hello [ValidateAntiForgeryToken]. Například:
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
Filtr autorizace, který kontroluje, zda:
* Hello příchozí požadavek má souboru cookie s názvem __RequestVerificationToken
* Hello příchozí požadavek má `Request.Form` položka názvem __RequestVerificationToken
* Tyto soubory cookie a `Request.Form` za předpokladu, že všechny shody hodnoty je dobře, hello žádost prochází jako normální. Ale pokud ne, pak chybu autorizace se zprávou "požadovaný token proti padělání nebyl zadán nebo byl neplatný".

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC5 MVC6 |
| **Atributy**              | Poskytovatel Identity identity zprostředkovatel – AD FS, – Azure AD |
| **Odkazy**              | [Zabezpečení webového rozhraní API s jednotlivých účtů a místní přihlášení v rozhraní ASP.NET Web API 2.2](http://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| **Kroky** | Hello webového rozhraní API je zabezpečena pomocí OAuth 2.0, pak se očekává, že token nosiče v požadavku hlavičku a uděluje přístup toohello požadavek ověřování jenom v případě, že hello token je platný. Na rozdíl od ověřování na základě souborů cookie prohlížeče nepřipojujte toorequests tokenů nosiče hello. Hello požadování klient potřebuje tooexplicitly připojit hello nosný token v hlavičce žádosti hello. Pro rozhraní ASP.NET Web API chráněné pomocí OAuth 2.0, proto jsou nosné tokeny za obrana proti útokům proti útokům CSRF. Upozorňujeme, že pokud hello MVC část aplikace hello používá ověřování pomocí formulářů (tj, soubory cookie používá), tokeny proti zfalšování mají toobe používané hello MVC webové aplikace. |

### <a name="example"></a>Příklad
Hello webového rozhraní API má toobe informován toorely pouze na nosné tokeny, nikoli na soubory cookie. Je možné ji provést hello následující konfigurace v `WebApiConfig.Register` metoda: '''C ostré kód konfigurace. SuppressDefaultHostAuthentication(); konfigurace. Filters.Add (nové HostAuthenticationFilter(OAuthDefaults.AuthenticationType));
```
hello SuppressDefaultHostAuthentication method tells Web API tooignore any authentication that happens before hello request reaches hello Web API pipeline, either by IIS or by OWIN middleware. That way, we can restrict Web API tooauthenticate only using bearer tokens.
