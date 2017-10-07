---
title: "aaaSigning výměny klíče ve službě Azure AD | Microsoft Docs"
description: "Tento článek popisuje hello podepisování výměna klíče osvědčené postupy pro Azure Active Directory"
services: active-directory
documentationcenter: .net
author: dstrockis
manager: krassk
editor: 
ms.assetid: ed964056-0723-42fe-bb69-e57323b9407f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ac6ade7f3ba2fbd22ea6d447aa5d07a2d6bdd451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="signing-key-rollover-in-azure-active-directory"></a>Podepisování výměna klíče v Azure Active Directory
Toto téma popisuje, co potřebujete tooknow o hello veřejných klíčů, které se používají v tokenech zabezpečení toosign Azure Active Directory (Azure AD). Je důležité toonote, který může být tyto výměny klíčů v pravidelných intervalech a v případě nouze převracet okamžitě. Všechny aplikace, které používají Azure AD by měla být schopný tooprogrammatically popisovač hello výměna klíče procesu nebo vytvořit proces periodické ruční výměny. Pokračujte ve čtení toounderstand jak hello klíče fungovat, jak tooassess hello dopad hello výměny tooyour aplikace a jak tooupdate aplikace nebo vytvořit klíče výměny pravidelné ruční výměna proces toohandle v případě potřeby.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Přehled podpisových klíčů ve službě Azure AD
Azure AD používá šifrování veřejného klíče založený na oborových standardů tooestablish vztah důvěryhodnosti mezi samostatně a hello aplikace, které ho používají. V praxi, toto funguje ve hello následujícím způsobem: Azure AD používá podpisový klíč, který se skládá z pár veřejného a privátního klíče. Pokud se uživatel přihlásí v aplikaci tooan, která používá Azure AD pro ověřování, Azure AD vytvoří token zabezpečení, který obsahuje informace o uživateli hello. Tento token je podepsána pomocí jeho privátní klíč před odesláním zpět toohello aplikace Azure AD. tooverify, který hello token je platný, ve skutečnosti původu z Azure AD, hello aplikace musíte ověřit hello token podpis pomocí veřejného klíče hello vystavené Azure AD, která je součástí klienta hello [OpenID Connect zjišťování dokumentu](http://openid.net/specs/openid-connect-discovery-1_0.html) nebo SAML/WS-Fed [dokument federačních metadat](active-directory-federation-metadata.md).

Z bezpečnostních důvodů Azure AD podpisového klíče zobrazí souhrn v pravidelných intervalech a v případě hello nouze, může být převracet okamžitě. Všechny aplikace, který se integruje s Azure AD měli připravit toohandle výměna klíče události bez ohledu na tom, jak často může dojít. Pokud tomu tak není, a aplikace pokusí toouse vypršenou platností klíče tooverify hello podpis na token, hello požádat o přihlášení selže.

Vždy není k dispozici v dokumentu zjišťování OpenID Connect hello a dokument federačních metadat hello více než jeden platný klíč. Je třeba připravit aplikaci toouse jakéhokoli hello klíčů zadaného v dokumentu hello vzhledem k tomu, že jeden klíč může být vrácena brzy, jiné můžou být jeho nahrazení a tak dále.

## <a name="how-tooassess-if-your-application-will-be-affected-and-what-toodo-about-it"></a>Jak tooassess, pokud vaše aplikace bude mít vliv na a jaké toodo o něm
Jak vaše aplikace zpracovává výměny klíčů, závisí na proměnné například hello typů aplikací nebo jaké protokol identity a knihovna byl použit. v níže uvedených částech Hello vyhodnocení, zda hello nejběžnějších typů aplikací je postiženo hello výměny klíčů a poskytovat informace o tom, jak tooupdate hello automatickou výměnu toosupport aplikace nebo ručně aktualizovat klíč hello.

* [Nativní klientské aplikace přístup k prostředkům](#nativeclient)
* [Webové aplikace / rozhraní API pro přístup k prostředkům](#webclient)
* [Webové aplikace / rozhraní API Ochrana prostředků a vyvíjené v Azure App Services](#appservices)
* [Webové aplikace / Ochrana prostředků pomocí rozhraní .NET OWIN OpenID Connect, WS-Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware rozhraní API](#owin)
* [Webové aplikace / Ochrana prostředků pomocí rozhraní .NET Core OpenID Connect nebo JwtBearerAuthentication middleware rozhraní API](#owincore)
* [Webové aplikace / rozhraní API Ochrana prostředků pomocí modulu passport-azure-ad Node.js](#passport)
* [Webové aplikace / rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2015 nebo Visual Studio 2017](#vs2015)
* [Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013](#vs2013)
* [Webová rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013](#vs2013_webapi)
* [Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2012](#vs2012)
* [Webové aplikace Ochrana prostředků a vytvořit s Visual Studio 2010, o 2008 pomocí technologie Windows Identity Foundation](#vs2010)
* [Webové aplikace / rozhraní API Ochrana prostředků pomocí kterékoli jiné knihovny nebo ručně implementací hello podporované protokoly](#other)

V tomto návodu je **není** platí pro:

* Aplikace přidána z Azure AD Application Gallery (včetně vlastní) mají zvláštní pokyny namapoval toosigning klíče. [Další informace.](../active-directory-sso-certs.md)
* Místní aplikacích publikovaných prostřednictvím proxy aplikace nemáte tooworry o podpisových klíčů.

### <a name="nativeclient"></a>Nativní klientské aplikace přístup k prostředkům
Aplikace, které jsou pouze přístup k prostředkům (tj Microsoft Graph, KeyVault, rozhraní API aplikace Outlook a další APIs Microsoft) obecně pouze získat token a předají toohello vlastníka prostředku. Vzhledem k tomu, že se nechrání žádné prostředky, není zkontrolovat hello token a proto není nutné tooensure, které je správně podepsaný.

Nativní klientské aplikace, zda desktop či mobile, do této kategorie patří a nejsou proto vliv hello výměny.

### <a name="webclient"></a>Webové aplikace / rozhraní API pro přístup k prostředkům
Aplikace, které jsou pouze přístup k prostředkům (tj Microsoft Graph, KeyVault, rozhraní API aplikace Outlook a další APIs Microsoft) obecně pouze získat token a předají toohello vlastníka prostředku. Vzhledem k tomu, že se nechrání žádné prostředky, není zkontrolovat hello token a proto není nutné tooensure, které je správně podepsaný.

Webové aplikace a webové rozhraní API, která používají toku jen aplikace hello (pověření klienta nebo certifikát klienta), do této kategorie patří a nejsou proto vliv hello výměny.

### <a name="appservices"></a>Webové aplikace / rozhraní API Ochrana prostředků a vyvíjené v Azure App Services
Azure App Services ověřování / autorizace (EasyAuth) funkce automaticky již má hello potřebné logiky toohandle výměny klíčů.

### <a name="owin"></a>Webové aplikace / Ochrana prostředků pomocí rozhraní .NET OWIN OpenID Connect, WS-Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware rozhraní API
Pokud vaše aplikace používá hello .NET OWIN OpenID Connect, WS-Fed nebo WindowsAzureActiveDirectoryBearerAuthentication middleware, už je výměna klíče toohandle hello potřebné logiky automaticky.

Můžete potvrdit, že vaše aplikace používá některý z těchto tak, že vyhledá všechny hello následující fragmenty kódu v souboru Startup.cs nebo Startup.Auth.cs vaší aplikace

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
     });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Webové aplikace / Ochrana prostředků pomocí rozhraní .NET Core OpenID Connect nebo JwtBearerAuthentication middleware rozhraní API
Pokud vaše aplikace používá hello .NET Core OWIN OpenID Connect nebo JwtBearerAuthentication middleware, už je výměna klíče toohandle hello potřebné logiky automaticky.

Můžete potvrdit, že vaše aplikace používá některý z těchto tak, že vyhledá všechny hello následující fragmenty kódu v souboru Startup.cs nebo Startup.Auth.cs vaší aplikace

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
     });
```

### <a name="passport"></a>Webové aplikace / rozhraní API Ochrana prostředků pomocí modulu passport-azure-ad Node.js
Pokud vaše aplikace používá modulu passport-ad Node.js hello, už je výměna klíče toohandle hello potřebné logiky automaticky.

Potvrďte, že vaše aplikace passport-ad vyhledáním hello následující fragment kódu v app.js vaší aplikace

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Webové aplikace / rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2015 nebo Visual Studio 2017
Pokud vaše aplikace byla vytvořena pomocí šablony webové aplikace v sadě Visual Studio 2015 nebo Visual Studio 2017 a jste vybrali **pracovní a školní účty** z hello **změna ověřování** nabídce již Výměna klíče toohandle hello potřebné logiky má automaticky. Tuto logiku vložených v middlewaru OWIN OpenID Connect hello, načítá a ukládá do mezipaměti klíče hello z hello OpenID Connect zjišťování dokumentu a je pravidelně aktualizuje.

Pokud jste ručně přidali řešení tooyour ověřování, nemusí mít aplikaci logiky hello potřeby výměny klíčů. Budete potřebovat toowrite ho sami nebo hello postupujte podle kroků v [webové aplikace / rozhraní API pomocí jiné knihovny nebo ručně implementací hello podporované protokoly.](#other).

### <a name="vs2013"></a>Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013
Pokud aplikace byla vytvořena pomocí šablony webové aplikace v sadě Visual Studio 2013 a jste vybrali **účty organizace** z hello **změna ověřování** nabídce už je nezbytné hello Logika toohandle klíče výměny automaticky. Tato logika ukládá jedinečný identifikátor vaší organizace a hello podepisování klíčové informace do dvou tabulek databáze přidružený hello projektu. Hello připojovacího řetězce pro databázi hello najdete v souboru Web.config hello projektu.

Pokud jste ručně přidali řešení tooyour ověřování, nemusí mít aplikaci logiky hello potřeby výměny klíčů. Budete potřebovat toowrite ho sami nebo hello postupujte podle kroků v [webové aplikace / rozhraní API pomocí jiné knihovny nebo ručně implementací hello podporované protokoly.](#other).

Hello následující kroky vám pomůže ověřte, zda text hello logiku správně funguje ve vaší aplikaci.

1. V sadě Visual Studio 2013, otevřete hello řešení a potom klikněte na hello **Průzkumníka serveru** karty v pravém podokně okna hello.
2. Rozbalte položku **připojení dat**, **objekt DefaultConnection**a potom **tabulky**. Vyhledejte hello **IssuingAuthorityKeys** tabulky, klikněte pravým tlačítkem ji a pak klikněte na tlačítko **zobrazit Data tabulky**.
3. V hello **IssuingAuthorityKeys** tabulky, budou existovat alespoň jeden řádek, který odpovídá toohello kryptografický otisk hodnotu pro klíč hello. Odstraňte všechny řádky v tabulce hello.
4. Klikněte pravým tlačítkem na hello **klienty** tabulky a potom klikněte na **zobrazit Data tabulky**.
5. V hello **klienty** tabulky, budou existovat alespoň jeden řádek, který odpovídá identifikátor klienta tooa jedinečný adresáře. Odstraňte všechny řádky v tabulce hello. Pokud nemáte odstraňování řádků hello v obou hello **klienty** tabulky a **IssuingAuthorityKeys** tabulky, bude dojde k chybě za běhu.
6. Sestavení a spuštění aplikace hello. Po přihlášení v účtu tooyour můžete zastavit aplikace hello.
7. Vrátí toohello **Průzkumníka serveru** a prohlédněte si hello hodnoty v hello **IssuingAuthorityKeys** a **klienty** tabulky. Můžete si všimnout, že budou mít byla automaticky jeho plnění znovu hello příslušné informace z dokument federačních metadat hello.

### <a name="vs2013"></a>Webová rozhraní API Ochrana prostředků a vytvořené pomocí sady Visual Studio 2013
Pokud jste vytvořili webové aplikace rozhraní API v sadě Visual Studio 2013 pomocí šablony hello webového rozhraní API a pak vybrali **účty organizace** z hello **změna ověřování** nabídky, které již mají hello potřebné logiky aplikace.

Pokud ručně nakonfigurované ověřování, postupujte podle pokynů hello níže toolearn jak tooconfigure vašeho webového rozhraní API tooautomatically aktualizovat informace o jeho klíči.

Hello následující fragment kódu ukazuje, jak tooget hello nejnovější klíče z hello dokument metadat federace a pak použít hello [obslužná rutina tokenu JWT](https://msdn.microsoft.com/library/dn205065.aspx) toovalidate hello token. fragment kódu Hello předpokládá, že budete používat vlastní ukládání do mezipaměti mechanismus pro zachování budoucí klíče toovalidate hello tokeny z Azure AD, ať to do databáze, konfigurační soubor nebo jinde.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates hello JWT Token that's part of hello Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in hello Azure Classic Portal]",
                ValidIssuer = "[hello issuer for hello token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache hello signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from hello specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in hello metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2012
Pokud vaše aplikace byla vytvořena v sadě Visual Studio 2012, pravděpodobně použijete hello identit a přístupu nástroj tooconfigure vaší aplikace. Je pravděpodobné, že používáte hello [ověřování vystavitele název registru (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). Hello VINR zodpovídá za údržbu informace o poskytovatelích důvěryhodné identity (Azure AD) a používají klíče hello toovalidate tokeny vydané jimi. Hello VINR také umožňuje snadno tooautomatically aktualizace hello klíčové informace uložené v souboru Web.config stažením hello nejnovější dokument metadat federation spojené s adresářem, kontrole, jestli se hello konfigurace je zastaralá s hello nejnovější dokument a aktualizuje hello aplikace toouse hello nového klíče podle potřeby.

Pokud jste vytvořili vaší aplikace pomocí některé z hello ukázky kódu nebo návod dokumentaci od společnosti Microsoft, logiku výměna klíče hello je již zahrnut ve vašem projektu. Si všimnete, že hello kódu níže v projektu již existuje. Pokud vaše aplikace již nemá tuto logiku, postupujte podle hello kroků tooadd a tooverify, zda pracuje správně.

1. V **Průzkumníku řešení**, přidat odkaz na toohello **System.IdentityModel** sestavení pro příslušné projekt hello.
2. Otevřete hello **Global.asax.cs** souboru a přidejte následující hello direktivy using:
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. Přidejte následující metodu toohello hello **Global.asax.cs** souboru:
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. Vyvolání hello **RefreshValidationSettings()** metoda v hello **Application_Start()** metoda v **Global.asax.cs** znázorněné:
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

Jakmile jste postupovali podle těchto kroků, souboru Web.config vaší aplikace bude aktualizována hello nejnovější informace z hello federační metadata dokumentu, včetně hello nejnovější klíče. Tato aktualizace se objeví pokaždé, když recykluje fond aplikací ve službě IIS; ve výchozím nastavení je služba IIS hodnotu toorecycle aplikace každých 29 hodin.

Postupujte podle kroků hello tooverify, zda pracuje logiku hello výměny klíčů.

1. Po ověření, že vaše aplikace používá hello kód výše, otevřete hello **Web.config** souboru a přejděte toohello  **<issuerNameRegistry>**  bloku, konkrétně hledá hello následující několika řádků:
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. V hello  **<add thumbprint=””>**  nastavení, změňte hodnotu kryptografický otisk hello nahrazením libovolný znak jiný. Uložit hello **Web.config** souboru.
3. Vytvoření aplikace hello a potom ho spusťte. Pokud dokončíte proces přihlášení hello, vaše aplikace úspěšně aktualizuje klíč hello stažením hello vyžaduje informace ze svého adresáře na dokument metadat federace. Pokud máte problémy s přihlášením, zkontrolujte hello změny ve vaší aplikaci jsou správné načtením hello [tooYour přidání přihlašování webové aplikace pomocí Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) tématu nebo stahování a zkontrolujete hello následující ukázka kódu: [ Víceklientská Cloudová aplikace pro Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).

### <a name="vs2010"></a>Webové aplikace Ochrana prostředků a vytvořené pomocí sady Visual Studio 2008 nebo 2010 a Windows Identity Foundation (WIF) verze 1.0 pro rozhraní .NET 3.5
Pokud jste vytvořili aplikaci na verzi WIF verze 1.0, není žádná aktualizace tooautomatically zadaný mechanismus toouse konfigurace vaší aplikace nový klíč.

* *Nejjednodušším způsobem, jak* pomocí nástrojů FedUtil hello součástí hello WIF SDK, která můžete načíst nejnovější dokument metadat hello a aktualizovat konfiguraci.
* Aktualizujte vaše aplikace too.NET 4.5, který obsahuje nejnovější verzi nachází v oboru názvů systému hello WIF hello. Pak můžete použít hello [ověřování vystavitele název registru (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) tooperform aplikace hello Konfigurace automatických aktualizací.
* Proveďte ruční výměna podle pokynů hello na konci hello tohoto dokumentu pokyny.

Pokyny toouse hello FedUtil tooupdate konfiguraci:

1. Ověřte, zda máte hello WIF v1.0 SDK pro Visual Studio 2008 nebo 2010 nainstalována na vývojovém počítači. Můžete [stáhnout odsud](https://www.microsoft.com/en-us/download/details.aspx?id=4451) Pokud jste ještě nenainstalovali ho.
2. V sadě Visual Studio otevřete hello řešení a potom klikněte pravým tlačítkem na příslušné projektu hello a vyberte **aktualizace federačních metadat**. Pokud tato možnost není k dispozici, FedUtil nebo hello WIF verze 1.0 SDK nebyl nainstalován.
3. Z příkazového řádku hello vyberte **aktualizace** toobegin aktualizace federačních metadat. Pokud máte prostředí serveru toohello přístupu je hostitelem aplikace hello, můžete volitelně použít na FedUtil [scheduler automatické metadata aktualizace](https://msdn.microsoft.com/library/ee517272.aspx).
4. Klikněte na tlačítko **Dokončit** procesu aktualizace toocomplete hello.

### <a name="other"></a>Webové aplikace / rozhraní API Ochrana prostředků pomocí kterékoli jiné knihovny nebo ručně implementací hello podporované protokoly
Pokud používáte některé jiné knihovny nebo ručně implementované všechny hello podporované protokoly, budete potřebovat tooreview hello knihovně nebo tooensure vaší implementace, která hello klíč je načítány z hello OpenID Connect zjišťování dokumentu nebo hello Dokument metadat federace. Jedním ze způsobů toocheck pro tento je toodo vyhledávání v kódu nebo kód hello knihovny pro všechny hovory se tooeither hello OpenID zjišťování dokument nebo dokument federačních metadat hello.

Pokud se klíče ukládají někde nebo pevně zakódované ve vaší aplikaci, můžete ručně načíst klíč hello a aktualizovat ji odpovídajícím způsobem podle provést ruční výměna podle pokynů hello na konci hello tohoto dokumentu pokyny. **Důrazně doporučujeme, můžete zvýšit vaše aplikace toosupport automatické výměny** pomocí kteréhokoli hello blíží obrysu v tomto článku tooavoid budoucí přerušení a režijní náklady, pokud Azure AD zvyšuje výměny cadence nebo má Nouzový out-of-band výměny.

## <a name="how-tootest-your-application-toodetermine-if-it-will-be-affected"></a>Jak tootest toodetermine vaší aplikace, pokud bude mít vliv
Můžete ověřit, jestli aplikace podporuje automatickou výměnu klíče stažením hello skripty a pokynů hello v [toto úložiště GitHub.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-tooperform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Jak tooperform ruční výměna, pokud je aplikace nepodporuje automatické výměny
Pokud aplikace nemá **není** podporují automatické výměny, budete potřebovat tooestablish proces, který pravidelně monitorování Azure AD podepisovací klíče a provede ruční výměna odpovídajícím způsobem. [Toto úložiště GitHub](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) obsahuje skripty a pokyny, jak toodo to.

