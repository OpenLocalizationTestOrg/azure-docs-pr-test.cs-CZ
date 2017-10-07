---
title: "aaaCreate aplikace obchodní Azure ověřování službou AD FS | Microsoft Docs"
description: "Zjistěte, jak toocreate-obchodní aplikace ve službě Azure App Service, který ověřuje pomocí místní služby tokenů zabezpečení. V tomto kurzu cílem služby AD FS jako hello místní služby tokenů zabezpečení."
services: app-service\web
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: 0fa9f7a1-37bd-4d11-845f-aeff6fc9e4ca
ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 08/31/2016
ms.author: cephalin
ms.openlocfilehash: cfd6f14837642fbf58ca5da9dee72db69cd5ab7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-line-of-business-azure-app-with-ad-fs-authentication"></a>Vytvoření aplikace Azure-obchodní ověřování službou AD FS
Tento článek ukazuje, jak toocreate rozhraní ASP.NET MVC – obchodní aplikace v [Azure App Service](../app-service/app-service-value-prop-what-is.md) pomocí místní [Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx) jako zprostředkovatele identity hello. V tomto scénáři můžete pracovat, když chcete toocreate-obchodní aplikace ve službě Azure App Service, ale vaše organizace vyžaduje toobe directory data uložená na místě.

> [!NOTE]
> Přehled možností hello různých enterprise ověřování a autorizace pro službu Azure App Service naleznete v tématu [ověřit pomocí místní služby Active Directory v aplikaci Azure](web-sites-authentication-authorization.md).
> 
> 

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a>Co se sestavení
Vytvoříte základní aplikaci ASP.NET v Azure App Service Web Apps s hello následující funkce:

* Ověřuje uživatele na základě služby AD FS
* Používá `[Authorize]` tooauthorize uživatelů pro různé akce
* Konfigurace statické pro ladění v sadě Visual Studio i publikování tooApp Service Web Apps (konfigurace jednou, ladění a kdykoli publikování)  

<a name="bkmk_need"></a>

## <a name="what-you-need"></a>Co potřebujete
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

V tomto kurzu se třeba hello následující toocomplete:

* Místní nasazení služby AD FS (návod začátku do konce hello testovacím prostředí použili v tomto kurzu, najdete v části [testovacího prostředí: samostatné služby tokenů zabezpečení s AD FS ve virtuálním počítači Azure (pro test pouze)](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))
* Vztahy důvěryhodnosti předávající strany toocreate oprávnění v Správa služby AD FS
* Visual Studio 2013 Update 4 nebo novější
* [Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) nebo novější

<a name="bkmk_sample"></a>

## <a name="use-sample-application-for-line-of-business-template"></a>Použití ukázkovou aplikaci pro obchodní – šablony
Hello ukázkovou aplikaci v tomto kurzu [WebApp. WSFederation DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), vytvoří tým služby Azure Active Directory hello. Vzhledem k tomu, že služba AD FS podporuje služby WS-Federation, můžete snadno použít jako šablony toocreate-obchodní aplikace. Má hello následující funkce:

* Používá [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) tooauthenticate s místní nasazení služby AD FS
* Funkce přihlášení a odhlášení
* Používá [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (namísto technologie Windows Identity Foundation), který je hello budoucí technologie ASP.NET a mnohem jednodušší tooset pro ověřování a autorizace než WIF

<a name="bkmk_setup"></a>

## <a name="set-up-hello-sample-application"></a>Nastavit hello ukázkové aplikace
1. Klonovat nebo stáhnout hello ukázkové řešení v [WebApp. WSFederation DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) tooyour místního adresáře.
   
   > [!NOTE]
   > Hello pokynů [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) ukazují, jak tooset až hello aplikací s Azure Active Directory. Ale v tomto kurzu, můžete ho nastavit s AD FS, tak místo toho postupujte podle zde uvedených kroků hello.
   > 
   > 
2. Otevřete hello řešení a pak otevřete Controllers\AccountController.cs v hello **Průzkumníku řešení**.
   
   Zobrazí se, že hello kód jednoduše problémy uživatele hello tooauthenticate výzvu ověřování pomocí protokolu WS-Federation. V App_Start\Startup.Auth.cs je nakonfigurované všechny ověřování.
3. Otevřete App_Start\Startup.Auth.cs. V hello `ConfigureAuth` metoda, Poznámka hello řádku:
   
       app.UseWsFederationAuthentication(
           new WsFederationAuthenticationOptions
           {
               Wtrealm = realm,
               MetadataAddress = metadata                                      
           });
   
   V hello, world OWIN tento fragment kódu je ve skutečnosti hello úplné že minimální potřebujete tooconfigure WS-Federation ověřování. Je mnohem jednodušší a více elegantní než WIF, kde je soubor Web.config vložit se souborem XML po celém hello místě. Hello pouze informace, které potřebujete je hello předávající stranu pro identifikátor a hello adresa URL služby AD FS metadata souboru. Tady je příklad:
   
   * Identifikátor RP:`https://contoso.com/MyLOBApp`
   * Metadata adresa:`http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`
4. V App_Start\Startup.Auth.cs změňte hello následující definice statických řetězec:  
   
   <pre class="prettyprint">
   private static string realm = ConfigurationManager.AppSettings["ida:<mark>RPIdentifier</mark>"];
   <mark><del>private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];</del></mark>
   <mark><del>private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];</del></mark>
   <mark><del>private static string metadata = string.Format("{0}/{1}/federationmetadata/2007-06/federationmetadata.xml", aadInstance, tenant);</del></mark>
   <mark>private static string metadata = string.Format("https://{0}/federationmetadata/2007-06/federationmetadata.xml", ConfigurationManager.AppSettings["ida:ADFS"]);</mark>
   
   <mark><del>string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);</del></mark>
   </pre>
5. Nyní proveďte hello odpovídající změny v souboru Web.config. Otevřete soubor Web.config hello a upravte následující nastavení aplikace hello:  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="webpages:Version" value="3.0.0.0" /&gt;
   &lt;add key="webpages:Enabled" value="false" /&gt;
   &lt;add key="ClientValidationEnabled" value="true" /&gt;
   &lt;add key="UnobtrusiveJavaScriptEnabled" value="true" /&gt;
   <mark><del>&lt;add key="ida:Wtrealm" value="[Enter hello App ID URI of WebApp-WSFederation-DotNet https://contoso.onmicrosoft.com/WebApp-WSFederation-DotNet]" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:AADInstance" value="https://login.microsoftonline.com" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" /&gt;</del></mark>
   <mark>&lt;add key="ida:RPIdentifier" value="[Enter hello relying party identifier as configured in AD FS, e.g. https://localhost:44320/]" /&gt;</mark>
   <mark>&lt;add key="ida:ADFS" value="[Enter hello FQDN of AD FS service, e.g. adfs.contoso.com]" /&gt;</mark>
   
   &lt;/appSettings&gt;
   </pre>
   
   Vyplňte hodnoty klíče hello podle příslušné prostředí.
6. Sestavení toomake aplikace hello se, že nejsou žádné chyby.

A je to. Hello vzorová aplikace je nyní připraven toowork se službou AD FS. Stále potřebujete tooconfigure s tuto aplikaci ve službě AD FS později vztah důvěryhodnosti RP.

<a name="bkmk_deploy"></a>

## <a name="deploy-hello-sample-application-tooazure-app-service-web-apps"></a>Nasazení hello ukázkové aplikace tooAzure App Service Web Apps
Zde můžete publikovat hello aplikace tooa webové aplikace v App Service Web Apps při zachování hello ladění prostředí. Všimněte si, že předtím, než ho má vztah důvěryhodnosti RP se službou AD FS, tak ověřování se stále nefunguje ještě budete toopublish hello aplikace. Ale pokud je to provést nyní můžete máte hello adresa URL webové aplikace, které můžete použít důvěryhodnosti RP tooconfigure hello později.

1. Klikněte pravým tlačítkem na projekt a vyberte **publikovat**.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/01-publish-website.png)
2. Vyberte **služby Microsoft Azure App Service**.
3. Pokud nejste přihlášeni tooAzure, klikněte na tlačítko **přihlásit** hello účtu Microsoft a používat pro vaše předplatné Azure toosign v.
4. Jakmile se přihlásíte, klikněte na tlačítko **nový** toocreate webové aplikace.
5. Vyplňte všechna povinná pole. Chystáte tooconnect tooon místní data později, tak nevytvářejte databáze pro tuto webovou aplikaci.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/02-create-website.png)
6. Klikněte na možnost **Vytvořit**. Po vytvoření webové aplikace hello dialogového okna Publikovat Web hello je otevřené.
7. V **cílová adresa URL**, změňte **http** příliš**https**. Zkopírujte hello celou adresu URL tooa textový editor pro pozdější použití. Potom klikněte na **publikovat**.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/03-destination-url.png)
8. V sadě Visual Studio otevřete **Web.Release.config** ve vašem projektu. Vložte následující XML do hello hello `<configuration>` značky a nahraďte hodnotu klíče hello vaší webové aplikace publikovat adresu URL.  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="ida:RPIdentifier" value="<mark>[e.g. https://mylobapp.azurewebsites.net/]</mark>" xdt:Transform="SetAttributes" xdt:Locator="Match(key)" /&gt;
   &lt;/appSettings&gt;</pre>

Když jste hotovi, máte dva identifikátory RP nakonfigurované ve vašem projektu, jednu pro vaše prostředí ladění v sadě Visual Studio, a jeden pro hello publikované webové aplikace v Azure. Vztah důvěryhodnosti RP nastavíte pro každou ze dvou prostředí hello ve službě AD FS. Během ladění, jsou nastavení aplikace hello v souboru Web.config použité toomake vaše **ladění** konfigurační práce se službou AD FS. Když je publikována (ve výchozím nastavení, hello **verze** konfigurace je publikována), transformovaných Web.config je načtený, která zahrnuje změny nastavení aplikace hello v Web.Release.config.

Pokud chcete tooattach hello publikované webové aplikace v Azure toohello ladicí program (tj. musíte nahrát symboly ladění kódu v hello publikované webové aplikace), můžete vytvořit klon hello konfiguraci pro Azure ladění ladění, ale s jeho vlastní vlastní Web.config transformace ( například Web.AzureDebug.config) používající nastavení aplikace hello z Web.Release.config. To vám umožní toomaintain konfiguraci statického hello různých prostředích.

<a name="bkmk_rptrusts"></a>

## <a name="configure-relying-party-trusts-in-ad-fs-management"></a>Nakonfigurujte vztahy důvěryhodnosti předávající strany v Správa služby AD FS
Nyní je nutné tooconfigure v Správa služby AD FS vztah důvěryhodnosti RP, než bude možné použít ukázkovou aplikaci a ve skutečnosti ověření pomocí služby AD FS. Budete potřebovat tooset až dva vztahy důvěryhodnosti samostatných RP, jeden pro vaše prostředí ladění a jeden pro publikované webové aplikace.

> [!NOTE]
> Zajistěte, aby zopakovat hello následujících kroků pro oba prostředí.
> 
> 

1. Na serveru služby AD FS Přihlaste se pomocí přihlašovacích údajů, které mají tooAD rights management služby FS.
2. Otevřete správu služby AD FS. Klikněte pravým tlačítkem na **AD FS\Trusted Relationships\Relying strany důvěřuje** a vyberte **přidat vztah důvěryhodnosti předávající strany**.
   
   ![](./media/web-sites-dotnet-lob-application-adfs/1-add-rptrust.png)
3. V hello **vybrat zdroj dat** vyberte **ručně zadat data o předávající straně hello**. 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/2-enter-rp-manually.png)
4. V hello **zadání zobrazovaného názvu** , zadejte zobrazovaný název pro hello aplikaci a klikněte na tlačítko **Další**.
5. V hello **zvolte protokol** klikněte na tlačítko **Další**.
6. V hello **konfigurace certifikátu** klikněte na tlačítko **Další**.
   
   > [!NOTE]
   > Vzhledem k tomu, měli byste použít protokol HTTPS již, šifrovaných tokenů jsou volitelné. Pokud chcete skutečně tooencrypt tokeny od služby AD FS na této stránce, musíte taky přidat logiku dešifrování tokenů v kódu. Další informace najdete v tématu [ruční konfiguraci middlewaru OWIN WS-Federation a přijímat šifrované tokeny](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).
   > 
   > 
7. Než můžete přesunout na další krok text hello, je třeba jednu část informací z projektu sady Visual Studio. V okně Vlastnosti projektu hello, Všimněte si, hello **SSL URL** aplikace hello. 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/3-ssl-url.png)
8. Zpět v Správa služby AD FS, v hello **konfigurace adresy URL** stránku hello **předávající strany Průvodce přidáním vztahu důvěryhodnosti**, vyberte **povolit podporu pasivního protokolu WS-Federation hello** a Zadejte v hello SSL URL projektu sady Visual Studio, že jste si poznamenali v předchozím kroku hello. Pak klikněte na **Další**.
   
   ![](./media/web-sites-dotnet-lob-application-adfs/4-configure-url.png)
   
   > [!NOTE]
   > Adresa URL určuje, kde toosend hello klienta po ověření úspěšné. Pro prostředí hello ladění, by měla být <code>https://localhost:&lt;port&gt;/</code>. Pro hello publikované webové aplikaci mělo by být adresa URL webové aplikace hello.
   > 
   > 
9. V hello **konfigurovat identifikátory** , zkontrolujte, že je již uvedené projektu adresy URL protokolu SSL a klikněte na tlačítko **Další**. Klikněte na tlačítko **Další** všechny hello toohello způsob, jak na konci průvodce hello s výchozí možnosti.
   
   > [!NOTE]
   > V App_Start\Startup.Auth.cs projektu sady Visual Studio, je tento identifikátor porovnání hodnota hello <code>WsFederationAuthenticationOptions.Wtrealm</code> během federovaného ověřování. Ve výchozím nastavení přidá se jako identifikátor RP adresa URL aplikace hello hello v předchozím kroku.
   > 
   > 
10. Nyní dokončení konfigurace hello RP aplikací pro váš projekt ve službě AD FS. V dalším kroku nakonfigurujte tuto aplikaci toosend hello deklarace identity potřeby vaší aplikací. Hello **upravit pravidla deklarací identity** dialogu je ve výchozím nastavení pro vás otevřen na konci hello hello průvodce, abyste mohli začít okamžitě. Umožňuje nakonfigurovat alespoň hello následující deklarace identity (s schémata v závorkách):
    
    * Název (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - používaný ASP.NET toohydrate `User.Identity.Name`.
    * Hlavní název (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - použít toouniquely uživatele identifikaci uživatelů v organizaci hello.
    * Členství ve skupinách jako pro role (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - lze použít s `[Authorize(Roles="role1, role2,...")]` decoration tooauthorize řadiče nebo akce. Ve skutečnosti nemusí tento přístup hello většina původce pro povolení role. Pokud vaši uživatelé AD patří toohundreds skupin zabezpečení, budou stovky deklarace rolí v tokenu SAML hello. Alternativní způsob je toosend jedné role deklarací podmíněně v závislosti na členství hello uživatele v dané skupině. Ale jsme budete jednoduchost pro účely tohoto kurzu.
    * Název ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) – lze použít pro ověření proti padělání. Další informace o toomake ho fungování s proti padělání ověřování najdete v tématu hello **přidat funkce – obchodní** části [vytvoření-obchodní aplikace pro Azure pomocí ověřování Azure Active Directory ](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).
    
    > [!NOTE]
    > Hello deklarace identity typy budete potřebovat tooconfigure pro vaše aplikace je určen podle potřeb vaší aplikace. Hello seznam deklarací identity, které podporuje aplikace Azure Active Directory (tj. vztahy důvěryhodnosti RP), například naleznete v části [podporované tokenu a typy deklarací identity](http://msdn.microsoft.com/library/azure/dn195587.aspx).
    > 
    > 
11. V dialogovém okně upravit pravidla deklarací identity hello, klikněte na tlačítko **přidat pravidlo**.
12. Nakonfigurujte hello název UPN a role deklarací identity, jak je znázorněno v hello – snímek obrazovky a klikněte na **Dokončit**.
    
    ![](./media/web-sites-dotnet-lob-application-adfs/5-ldap-claims.png)
    
    Dále vytvoříte přechodný název ID deklarace identity pomocí kroků hello ukázáno v [identifikátory názvů v kontrolní výrazy SAML](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).
13. Klikněte na tlačítko **přidat pravidlo** znovu.
14. Vyberte **odesílat deklarace pomocí vlastního pravidla** a klikněte na tlačítko **Další**.
15. Vložení hello následující pravidlo jazyk do hello **vlastní pravidlo** pole, název hello pravidlo **za identifikátor relace** a klikněte na tlačítko **Dokončit**.  
    
    <pre class="prettyprint">
    c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] &amp;&amp;
    c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant"]
     => add(
         store = "_OpaqueIdStore",
         types = ("<mark>http://contoso.com/internal/sessionid</mark>"),
         query = "{0};{1};{2};{3};{4}",
         param = "useEntropy",
         param = c1.Value,
         param = c1.OriginalIssuer,
         param = "",
         param = c2.Value);
    </pre>
    
    Vlastní pravidlo by měl vypadat takto:
    
    ![](./media/web-sites-dotnet-lob-application-adfs/6-per-session-identifier.png)
16. Klikněte na tlačítko **přidat pravidlo** znovu.
17. Vyberte **transformovat příchozí deklarace** a klikněte na tlačítko **Další**.
18. Konfigurace hello pravidla, jak je znázorněno na snímku obrazovky hello (pomocí typu deklarace identity hello jste vytvořili v vlastní pravidlo hello) a klikněte na tlačítko **Dokončit**.
    
    ![](./media/web-sites-dotnet-lob-application-adfs/7-transient-name-id.png)
    
    Podrobné informace o hello kroky pro přechodný deklarace ID názvu hello najdete v tématu [identifikátory názvů v kontrolní výrazy SAML](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).
19. Klikněte na tlačítko **použít** v hello **upravit pravidla deklarací identity** dialogové okno. Teď by měl vypadat jako hello následující snímek obrazovky:
    
    ![](./media/web-sites-dotnet-lob-application-adfs/8-all-claim-rules.png)
    
    > [!NOTE]
    > Znovu zkontrolujte, zda tento postup opakujte pro vaše prostředí ladění a publikovanou webovou aplikací.
    > 
    > 

<a name="bkmk_test"></a>

## <a name="test-federated-authentication-for-your-application"></a>Test federovaného ověřování pro aplikaci
Můžete je připraven tootest logiku ověřování aplikace u služby AD FS. V mé testovacího prostředí služby AD FS je nutné testovacího uživatele, který patří tooa testovací skupiny ve službě Active Directory (AD).

![](./media/web-sites-dotnet-lob-application-adfs/10-test-user-and-group.png)

tootest ověřování v ladicím programu hello, stačí toodo nyní je typ `F5`. Pokud chcete ověřování tootest v hello publikované webové aplikaci, přejděte toohello adresy URL.

Po načtení hello webové aplikace, klikněte na tlačítko **přihlásit**. Měli byste obdržet teď buď přihlašovací dialogové okno nebo hello přihlašovací stránky obsloužených služby AD FS, v závislosti na metodě ověřování hello vybrané službou AD FS. Zde je zobrazila v Internet Exploreru 11.

![](./media/web-sites-dotnet-lob-application-adfs/9-test-debugging.png)

Jakmile se můžete přihlásit jako uživatel v doméně hello AD nasazení hello AD FS, měli byste vidět domovskou stránku hello znovu s **Hello, <User Name>!** v horním hello. Zde je možné získat.

![](./media/web-sites-dotnet-lob-application-adfs/11-test-debugging-success.png)

Dosavadní práce můžete po úspěšném hello následující způsoby:

* Vaše aplikace úspěšně dosáhl služby AD FS a identifikátor odpovídající RP se nachází v hello databáze služby AD FS
* Služby AD FS byl úspěšně ověřen AD uživatele a přesměrování zpět toohello aplikace domovské stránky
* Služby AD FS jako název úspěšně poslala hello deklarace identity aplikace tooyour (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name), jak je uvedeno hello fakt tento uživatel hello název se zobrazí v hello rohu. 

Pokud deklarace identity názvu hello chybí, jste viděli by **Hello,!**. Pokud se podíváte na Views\Shared\_LoginPartial.cshtml, zjistíte, že používá `User.Identity.Name` toodisplay hello uživatelské jméno. Jak je uvedeno nahoře, pokud název hello deklarací z hello ověření uživatele je k dispozici v tokenu SAML hello, ASP.NET hydráty tato vlastnost s ním. toosee, které všechny hello deklarací, že jsou odeslány službou AD FS, do hello umístit zarážky v Controllers\HomeController.cs, metoda akce indexu. Po ověření uživatele hello zkontrolovat hello `System.Security.Claims.Current.Claims` kolekce.

![](./media/web-sites-dotnet-lob-application-adfs/12-test-debugging-all-claims.png) 

<a name="bkmk_authorize"></a>

## <a name="authorize-users-for-specific-controllers-or-actions"></a>Autorizace uživatelů pro konkrétní řadiče nebo akce
Vzhledem k tomu, že jste zahrnuli členství ve skupinách jako deklarace identity role ve vaší konfiguraci RP vztahu důvěryhodnosti, teď můžete například vytvořit přímo v hello `[Authorize(Roles="...")]` decoration pro kontrolery a akce. V-obchodní aplikace s hello vytvořit-čtení-aktualizace-odstranění (CRUD) vzor můžete povolit konkrétní role tooaccess každá akce. Prozatím se právě vyzkoušejte tuto funkci na existující řadič domovské hello.

1. Otevřete Controllers\HomeController.cs.
2. Uspořádání hello `About` a `Contact` akce metody podobné toohello následující kód, pomocí členství ve skupinách zabezpečení, které má ověřeného uživatele.  
   
    <pre class="prettyprint">
    <mark>[Authorize(Roles="Test Group")]</mark>
    public ActionResult About()
    {
        ViewBag.Message = "Your application description page.";
   
        return View();
    }
   
    <mark>[Authorize(Roles="Domain Admins")]</mark>
    public ActionResult Contact()
    {
        ViewBag.Message = "Your contact page.";
   
        return View();
    }
    </pre>
   
    Vzhledem k tomu, že po přidání **testovacího uživatele** příliš**testovací skupina** v mé testovacího prostředí služby AD FS I budete používat testovací skupina tootest autorizace na `About`. Pro `Contact`, I budete testovat hello malá záporné **Domain Admins**, toowhich **testovací uživatele** nepatří.
3. Spuštění ladicího programu hello zadáním `F5` a přihlaste se a pak klikněte na **o**. Má teď se zobrazil hello `~/About/Index` stránka úspěšně, pokud ověřený uživatel má oprávnění pro tuto akci.
4. Klikněte na tlačítko **kontaktujte**, v mém případě by neměl Autorizovat **testovacího uživatele** pro akci hello. Prohlížeč hello je však přesměrovaného tooAD služby FS, který nakonec zobrazí tato zpráva:
   
    ![](./media/web-sites-dotnet-lob-application-adfs/13-authorize-adfs-error.png)
   
    Pokud byste prozkoumat této chybě v prohlížeči událostí na hello serveru služby AD FS, zobrazí tato zpráva o výjimce:  
   
    <pre class="prettyprint">
    Microsoft.IdentityServer.Web.InvalidRequestException: MSIS7042: <mark>hello same client browser session has made '6' requests in hello last '11' seconds.</mark> Contact your administrator for details.
       at Microsoft.IdentityServer.Web.Protocols.PassiveProtocolHandler.UpdateLoopDetectionCookie(WrappedHttpListenerContext context)
       at Microsoft.IdentityServer.Web.Protocols.WSFederation.WSFederationProtocolHandler.SendSignInResponse(WSFederationContext context, MSISSignInResponse response)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.ProcessProtocolRequest(ProtocolContext protocolContext, PassiveProtocolHandler protocolHandler)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.OnGetContext(WrappedHttpListenerContext context)
    </pre>
   
    Hello důvodem této chyby je, že ve výchozím nastavení, MVC vrátí 401 Neautorizováno, pokud nemáte oprávnění role uživatele. Tím se spustí opětovné ověření požadavku tooyour zprostředkovatele identity (AD FS). Vzhledem k tomu, že uživatel hello je již ověřen, služba AD FS vrátí toohello stejné stránky, pak vydá jiné 401, vytváření smyčku přesměrování. Přepíše třídy AuthorizeAttribute na `HandleUnauthorizedRequest` metoda s jednoduché logiku tooshow něco, co má smysl místo trvalého přesměrování smyčky hello.
5. Vytvořte soubor v hello projekt s názvem AuthorizeAttribute.cs a hello vložte následující kód do ní.
   
        using System;
        using System.Web.Mvc;
        using System.Web.Routing;
   
        namespace WebApp_WSFederation_DotNet
        {
            [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
            public class AuthorizeAttribute : System.Web.Mvc.AuthorizeAttribute
            {
                protected override void HandleUnauthorizedRequest(AuthorizationContext filterContext)
                {
                    if (filterContext.HttpContext.Request.IsAuthenticated)
                    {
                        filterContext.Result = new System.Web.Mvc.HttpStatusCodeResult((int)System.Net.HttpStatusCode.Forbidden);
                    }
                    else
                    {
                        base.HandleUnauthorizedRequest(filterContext);
                    }
                }
            }
        }
   
    Hello přepsat kód zasílá protokolu HTTP 403 (zakázáno) místo protokolu HTTP 401 (Neautorizováno) v případech ověřený, ale neoprávněným.
6. Spuštění ladicího programu hello znovu s `F5`. Kliknutím na tlačítko **kontaktujte** nyní zobrazuje informativnější (i když rozdělení nežádoucí) chybovou zprávu:
   
    ![](./media/web-sites-dotnet-lob-application-adfs/14-unauthorized-forbidden.png)
7. Znovu publikovat tooAzure aplikace hello App Service Web Apps a testování hello chování hello živé aplikace.

<a name="bkmk_data"></a>

## <a name="connect-tooon-premises-data"></a>Připojení místní tooon dat
Zda je vhodnější tooimplement-obchodní aplikace se službou AD FS místo Azure Active Directory je důvod problémy s dodržováním předpisů s udržováním organizace dat mimo místní. Také to může znamenat, že webové aplikace v Azure musí přístup k místní databáze, vzhledem k tomu, že nejsou povoleny toouse [SQL Database](/services/sql-database/) jako hello datové vrstvy pro webové aplikace.

Azure App Service Web Apps podporuje přístupu k místní databáze s dva přístupy: [hybridní připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [virtuální sítě](web-sites-integrate-with-vnet.md). Další informace najdete v tématu [integrace pomocí virtuální sítě a hybridních připojení s Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).

<a name="bkmk_resources"></a>

## <a name="further-resources"></a>Další prostředky
* [Ověření pomocí místní služby Active Directory v aplikaci Azure](web-sites-authentication-authorization.md)
* [Vytvoření aplikace Azure-obchodní pomocí ověřování Azure Active Directory](web-sites-dotnet-lob-application-azure-ad.md)
* [Použití hello místní organizace ověřování možnost (ADFS) s ASP.NET v sadě Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)
* [Migrace tooKatana VS2013 webového projektu ze WIF](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
* [Přehled služby Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx)
* [Specifikace protokolu WS-Federation 1.1](http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf?S_TACT=105AGX04&S_CMP=LP)

