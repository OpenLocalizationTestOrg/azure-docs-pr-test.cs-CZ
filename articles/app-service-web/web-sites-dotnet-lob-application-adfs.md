---
title: "Vytvoření aplikace Azure-obchodní ověřování službou AD FS | Microsoft Docs"
description: "Postup vytvoření-obchodní aplikace ve službě Azure App Service se ověřuje pomocí místní služby tokenů zabezpečení. V tomto kurzu cílem služby AD FS jako místní služba tokenů zabezpečení."
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
ms.openlocfilehash: f9a8984400378d154a504af8a41609900128d052
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-line-of-business-azure-app-with-ad-fs-authentication"></a>Vytvoření aplikace Azure-obchodní ověřování službou AD FS
V tomto článku se dozvíte, jak vytvořit aplikaci ASP.NET MVC – obchodní v [Azure App Service](../app-service/app-service-value-prop-what-is.md) pomocí místní [Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx) jako zprostředkovatele identity. V tomto scénáři můžete pracovat, pokud chcete vytvořit-obchodní aplikace ve službě Azure App Service, ale vaše organizace vyžaduje dat adresáře ukládaly na místě.

> [!NOTE]
> Přehled různých enterprise možnosti ověřování a autorizace pro službu Azure App Service najdete v tématu [ověřit pomocí místní služby Active Directory v aplikaci Azure](web-sites-authentication-authorization.md).
> 
> 

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a>Co se sestavení
Vytvoříte základní aplikaci ASP.NET v Azure App Service Web Apps s následující funkce:

* Ověřuje uživatele na základě služby AD FS
* Používá `[Authorize]` autorizace uživatelů pro různé akce
* Konfigurace statické pro ladění v sadě Visual Studio a publikování do App Service Web Apps (konfigurace jednou, ladění a kdykoli publikování)  

<a name="bkmk_need"></a>

## <a name="what-you-need"></a>Co potřebujete
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

Budete potřebovat k dokončení tohoto kurzu:

* Místní nasazení služby AD FS (návod začátku do konce tohoto testovacího prostředí používá v tomto kurzu, najdete v části [testovacího prostředí: samostatné služby tokenů zabezpečení s AD FS ve virtuálním počítači Azure (pro test pouze)](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))
* Oprávnění k vytvoření předávající strany vztahy důvěryhodnosti v Správa služby AD FS
* Visual Studio 2013 Update 4 nebo novější
* [Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) nebo novější

<a name="bkmk_sample"></a>

## <a name="use-sample-application-for-line-of-business-template"></a>Použití ukázkovou aplikaci pro obchodní – šablony
Ukázkové aplikace v tomto kurzu [WebApp. WSFederation DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), se vytvoří tým služby Azure Active Directory. Vzhledem k tomu, že služba AD FS podporuje služby WS-Federation, můžete ho použít jako šablonu pro vytvoření-obchodní aplikace s snadné. Má následující funkce:

* Používá [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) k ověřování pro místní nasazení služby AD FS
* Funkce přihlášení a odhlášení
* Používá [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (namísto technologie Windows Identity Foundation), což je budoucí technologie ASP.NET a mnohem jednodušší nastavení pro ověřování a autorizaci než WIF

<a name="bkmk_setup"></a>

## <a name="set-up-the-sample-application"></a>Nastavení ukázkové aplikace
1. Klonovat nebo stáhnout ukázkové řešení v [WebApp. WSFederation DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) do vašeho místního adresáře.
   
   > [!NOTE]
   > Postupujte podle pokynů v [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) ukazují, jak nastavit aplikaci s Azure Active Directory. Ale v tomto kurzu, můžete ho nastavit s AD FS, tak místo toho postupujte podle kroků v tomto poli.
   > 
   > 
2. Otevřete řešení a pak otevřete Controllers\AccountController.cs v **Průzkumníku řešení**.
   
   Zobrazí se, že kód jednoduše problémy výzvu ověřování k ověření uživatele pomocí protokolu WS-Federation. V App_Start\Startup.Auth.cs je nakonfigurované všechny ověřování.
3. Otevřete App_Start\Startup.Auth.cs. V `ConfigureAuth` metoda, poznamenejte si řádek:
   
       app.UseWsFederationAuthentication(
           new WsFederationAuthenticationOptions
           {
               Wtrealm = realm,
               MetadataAddress = metadata                                      
           });
   
   Na světě OWIN tento fragment kódu je skutečně minimum, budete muset nakonfigurovat ověřování WS-Federation. Je mnohem jednodušší a více elegantní než WIF, kde je soubor Web.config vložit se souborem XML po celém na místě. Pouze informace, které potřebujete je předávající strany (RP) identifikátor a adresu URL souboru metadat služby AD FS. Tady je příklad:
   
   * Identifikátor RP:`https://contoso.com/MyLOBApp`
   * Metadata adresa:`http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`
4. V App_Start\Startup.Auth.cs změňte následující definice statických řetězec:  
   
   <pre class="prettyprint">
   private static string realm = ConfigurationManager.AppSettings["ida:<mark>RPIdentifier</mark>"];
   <mark><del>private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];</del></mark>
   <mark><del>private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];</del></mark>
   <mark><del>private static string metadata = string.Format("{0}/{1}/federationmetadata/2007-06/federationmetadata.xml", aadInstance, tenant);</del></mark>
   <mark>private static string metadata = string.Format("https://{0}/federationmetadata/2007-06/federationmetadata.xml", ConfigurationManager.AppSettings["ida:ADFS"]);</mark>
   
   <mark><del>string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);</del></mark>
   </pre>
5. Nyní proveďte odpovídající změny v souboru Web.config. Otevřete soubor Web.config a upravit následující nastavení aplikace:  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="webpages:Version" value="3.0.0.0" /&gt;
   &lt;add key="webpages:Enabled" value="false" /&gt;
   &lt;add key="ClientValidationEnabled" value="true" /&gt;
   &lt;add key="UnobtrusiveJavaScriptEnabled" value="true" /&gt;
   <mark><del>&lt;add key="ida:Wtrealm" value="[Enter the App ID URI of WebApp-WSFederation-DotNet https://contoso.onmicrosoft.com/WebApp-WSFederation-DotNet]" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:AADInstance" value="https://login.microsoftonline.com" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" /&gt;</del></mark>
   <mark>&lt;add key="ida:RPIdentifier" value="[Enter the relying party identifier as configured in AD FS, e.g. https://localhost:44320/]" /&gt;</mark>
   <mark>&lt;add key="ida:ADFS" value="[Enter the FQDN of AD FS service, e.g. adfs.contoso.com]" /&gt;</mark>
   
   &lt;/appSettings&gt;
   </pre>
   
   Vyplňte hodnoty klíče podle příslušné prostředí.
6. Sestavení aplikace a ujistěte se, že nejsou žádné chyby.

A je to. Ukázkové aplikace je nyní připraven pro práci se službou AD FS. Dál musíte nakonfigurovat vztah důvěryhodnosti RP pomocí této aplikace ve službě AD FS později.

<a name="bkmk_deploy"></a>

## <a name="deploy-the-sample-application-to-azure-app-service-web-apps"></a>Nasadit ukázkovou aplikaci pro Azure App Service Web Apps
Zde publikování aplikace do webové aplikace v App Service Web Apps při zachování prostředí ladění. Všimněte si, že se chystáte publikovat aplikaci, než ho má vztah důvěryhodnosti RP se službou AD FS, tak ověřování se stále nefunguje ještě. Pokud se teď můžete máte ale adresa URL webové aplikace, které můžete použít ke konfiguraci vztahu důvěryhodnosti RP později.

1. Klikněte pravým tlačítkem na projekt a vyberte **publikovat**.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/01-publish-website.png)
2. Vyberte **služby Microsoft Azure App Service**.
3. Pokud nejste přihlášeni do Azure, klikněte na tlačítko **přihlásit** a přihlaste se pomocí účtu Microsoft pro vaše předplatné Azure.
4. Jakmile se přihlásíte, klikněte na tlačítko **nový** k vytvoření webové aplikace.
5. Vyplňte všechna povinná pole. Chystáte se připojit k místním datům později, tak nevytvářejte databáze pro tuto webovou aplikaci.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/02-create-website.png)
6. Klikněte na možnost **Vytvořit**. Po vytvoření webové aplikace se otevře dialogové okno publikování webu.
7. V **cílová adresa URL**, změňte **http** k **https**. Zkopírujte celou adresu URL do textového editoru pro pozdější použití. Potom klikněte na **publikovat**.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/03-destination-url.png)
8. V sadě Visual Studio otevřete **Web.Release.config** ve vašem projektu. Vložte následující kód XML do `<configuration>` značky a nahraďte hodnotu klíče s adresou URL vaší publikování webové aplikace.  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="ida:RPIdentifier" value="<mark>[e.g. https://mylobapp.azurewebsites.net/]</mark>" xdt:Transform="SetAttributes" xdt:Locator="Match(key)" /&gt;
   &lt;/appSettings&gt;</pre>

Když jste hotovi, máte dva identifikátory RP nakonfigurované v projektu, jeden pro vaše prostředí ladění v sadě Visual Studio a jeden pro publikované webové aplikaci v Azure. Vztah důvěryhodnosti RP nastavíte pro každou ze dvou prostředí ve službě AD FS. Během ladění, se používají nastavení aplikace v souboru Web.config, aby vaše **ladění** konfigurační práce se službou AD FS. Když je publikována (ve výchozím nastavení, **verze** konfigurace je publikována), transformovaných Web.config je načtený, která zahrnuje změny nastavení aplikace v Web.Release.config.

Pokud se chcete připojit publikované webové aplikace v Azure a ladicí program (tj. musíte nahrát symboly ladění kódu v publikované webové aplikaci), můžete vytvořit klon konfiguraci ladění pro Azure ladění, ale s jeho vlastní vlastní transformace Web.config (např.) Web.AzureDebug.config) používající nastavení aplikace z Web.Release.config. To umožňuje udržovat statické konfiguraci různých prostředích.

<a name="bkmk_rptrusts"></a>

## <a name="configure-relying-party-trusts-in-ad-fs-management"></a>Nakonfigurujte vztahy důvěryhodnosti předávající strany v Správa služby AD FS
Teď je potřeba nakonfigurovat v Správa služby AD FS vztah důvěryhodnosti RP, než bude možné použít ukázkovou aplikaci a ve skutečnosti ověření pomocí služby AD FS. Musíte nastavit dva vztahy důvěryhodnosti samostatných RP, jeden pro vaše prostředí ladění a jeden pro publikované webové aplikace.

> [!NOTE]
> Ujistěte se, opakujte tyto kroky pro obě prostředí.
> 
> 

1. Na serveru služby AD FS Přihlaste se pomocí přihlašovacích údajů, které mají práva pro správu do služby AD FS.
2. Otevřete správu služby AD FS. Klikněte pravým tlačítkem na **AD FS\Trusted Relationships\Relying strany důvěřuje** a vyberte **přidat vztah důvěryhodnosti předávající strany**.
   
   ![](./media/web-sites-dotnet-lob-application-adfs/1-add-rptrust.png)
3. V **vybrat zdroj dat** vyberte **zadat data o předávající straně ručně**. 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/2-enter-rp-manually.png)
4. V **zadání zobrazovaného názvu** , zadejte zobrazovaný název pro aplikaci a klikněte na tlačítko **Další**.
5. V **zvolte protokol** klikněte na tlačítko **Další**.
6. V **konfigurace certifikátu** klikněte na tlačítko **Další**.
   
   > [!NOTE]
   > Vzhledem k tomu, měli byste použít protokol HTTPS již, šifrovaných tokenů jsou volitelné. Pokud chcete skutečně šifrování tokenů ze služby AD FS na této stránce, musíte taky přidat logiku dešifrování tokenů v kódu. Další informace najdete v tématu [ruční konfiguraci middlewaru OWIN WS-Federation a přijímat šifrované tokeny](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).
   > 
   > 
7. Teprve potom přejděte na další krok, musíte jednu část informací z projektu sady Visual Studio. Ve vlastnostech projektu, Upozorňujeme **SSL URL** aplikace. 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/3-ssl-url.png)
8. Zpět na správu služby AD FS, v **konfigurace adresy URL** stránky **předávající strany Průvodce přidáním vztahu důvěryhodnosti**, vyberte **povolit podporu pasivního protokolu WS-Federation protokolu** a zadejte Adresa URL SSL projektu sady Visual Studio, kterou jste si poznamenali v předchozím kroku. Pak klikněte na **Další**.
   
   ![](./media/web-sites-dotnet-lob-application-adfs/4-configure-url.png)
   
   > [!NOTE]
   > Adresa URL určuje, kam má posílat klienta po ověření úspěšné. Pro prostředí ladění, by měla být <code>https://localhost:&lt;port&gt;/</code>. Pro publikované webové aplikaci mělo by být adresa URL webové aplikace.
   > 
   > 
9. V **konfigurovat identifikátory** , zkontrolujte, že je již uvedené projektu adresy URL protokolu SSL a klikněte na tlačítko **Další**. Klikněte na tlačítko **Další** zcela na konci průvodce výchozí možnosti.
   
   > [!NOTE]
   > V App_Start\Startup.Auth.cs projektu sady Visual Studio, tento identifikátor je nalezena shoda s hodnotou <code>WsFederationAuthenticationOptions.Wtrealm</code> během federovaného ověřování. Ve výchozím nastavení přidá se jako identifikátor RP adresa URL aplikace z předchozího kroku.
   > 
   > 
10. Nyní dokončení konfigurace aplikace RP pro svůj projekt ve službě AD FS. Dále je nutné nakonfigurovat k posílání deklarací identit vyžaduje vaše aplikace tuto aplikaci. **Upravit pravidla deklarací identity** dialogu je ve výchozím nastavení pro vás otevřen na konci tohoto průvodce, abyste mohli začít okamžitě. Umožňuje nakonfigurovat alespoň následující deklarace identity (s schémata v závorkách):
    
    * Název (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - použitý technologií ASP.NET k hydrate `User.Identity.Name`.
    * Hlavní název uživatele (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - použít k jednoznačné identifikaci uživatelů v organizaci.
    * Členství ve skupinách jako pro role (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - lze použít s `[Authorize(Roles="role1, role2,...")]` decoration autorizovat řadiče nebo akce. Ve skutečnosti nemusí být tento přístup většina původce pro povolení role. Pokud vaši uživatelé AD patří stovky skupin zabezpečení, budou stovky deklarace rolí v tokenu SAML. Alternativní způsob je poslat deklaraci identity jedné role podmíněně v závislosti na členství uživatele v dané skupině. Ale jsme budete jednoduchost pro účely tohoto kurzu.
    * Název ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) – lze použít pro ověření proti padělání. Další informace o tom, jak aby fungoval s ověřování proti padělání najdete v tématu **přidat funkce – obchodní** části [vytvoření-obchodní aplikace Azure s Azure Active Directory authentication](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).
    
    > [!NOTE]
    > Typy deklarací, které budete muset nakonfigurovat pro vaši aplikaci je určen podle potřeb vaší aplikace. Seznam deklarací identity, které podporuje aplikace Azure Active Directory (tj. vztahy důvěryhodnosti RP), například v tématu [podporované tokenu a typy deklarací identity](http://msdn.microsoft.com/library/azure/dn195587.aspx).
    > 
    > 
11. V dialogovém okně upravit pravidla deklarací identity, klikněte na tlačítko **přidat pravidlo**.
12. Nakonfigurujte název UPN a role deklarace identity, jak je znázorněno v snímek obrazovky a klikněte na **Dokončit**.
    
    ![](./media/web-sites-dotnet-lob-application-adfs/5-ldap-claims.png)
    
    Dále vytvoříte přechodný název ID deklarace identity pomocí kroků ukázáno v [identifikátory názvů v kontrolní výrazy SAML](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).
13. Klikněte na tlačítko **přidat pravidlo** znovu.
14. Vyberte **odesílat deklarace pomocí vlastního pravidla** a klikněte na tlačítko **Další**.
15. Vložte následující jazyka pravidel do **vlastní pravidlo** pole, název pravidla **za identifikátor relace** a klikněte na tlačítko **Dokončit**.  
    
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
18. Konfiguraci pravidla, jak je znázorněno na snímku obrazovky (typ deklarace identity, kterou jste vytvořili v vlastní pravidlo pomocí) a klikněte na tlačítko **Dokončit**.
    
    ![](./media/web-sites-dotnet-lob-application-adfs/7-transient-name-id.png)
    
    Podrobné informace o krocích pro přechodný deklarace ID názvu najdete v tématu [identifikátory názvů v kontrolní výrazy SAML](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).
19. Klikněte na tlačítko **použít** v **upravit pravidla deklarací identity** dialogové okno. Teď by měl vypadat jako na následujícím snímku obrazovky:
    
    ![](./media/web-sites-dotnet-lob-application-adfs/8-all-claim-rules.png)
    
    > [!NOTE]
    > Znovu zkontrolujte, zda tento postup opakujte pro vaše prostředí ladění a publikovanou webovou aplikací.
    > 
    > 

<a name="bkmk_test"></a>

## <a name="test-federated-authentication-for-your-application"></a>Test federovaného ověřování pro aplikaci
Jste připraveni k testování vaší aplikace logiky ověřování u služby AD FS. V mé testovacího prostředí služby AD FS je nutné testovacího uživatele, který patří do testovací skupiny ve službě Active Directory (AD).

![](./media/web-sites-dotnet-lob-application-adfs/10-test-user-and-group.png)

K testování ověřování v ladicím programu, musíte udělat teď je typ `F5`. Pokud chcete otestovat ověření v publikované webové aplikaci, přejděte na adresu URL.

Po načtení webové aplikace klikněte na tlačítko **přihlásit**. Měli byste nyní získat přihlašovací dialogové okno nebo přihlašovací stránku obsloužených služby AD FS, v závislosti na zvolené službou AD FS metodu ověřování. Zde je zobrazila v Internet Exploreru 11.

![](./media/web-sites-dotnet-lob-application-adfs/9-test-debugging.png)

Jakmile se můžete přihlásit jako uživatel v doméně AD nasazení služby AD FS, měli byste vidět domovské stránky znovu s **Hello, <User Name>!** v rohu. Zde je možné získat.

![](./media/web-sites-dotnet-lob-application-adfs/11-test-debugging-success.png)

Zatím jste úspěšně následujícími způsoby:

* Vaše aplikace úspěšně dosáhl služby AD FS a identifikátor odpovídající RP se nachází v databázi služby AD FS
* Služba AD FS byl úspěšně ověřen služby Active Directory a přesměrování, které zpět na domovskou stránku aplikace
* Služby AD FS jako úspěšně odeslána deklaraci identity názvu (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) k vaší aplikaci, jak je uvedeno fakt, že uživatelské jméno se zobrazí v horním. 

Pokud chybí deklarace identity názvu, jste viděli by **Hello,!**. Pokud se podíváte na Views\Shared\_LoginPartial.cshtml, zjistíte, že používá `User.Identity.Name` zobrazíte uživatelské jméno. Jak je uvedeno nahoře, pokud je k dispozici v tokenu SAML deklarace identity názvu ověřeného uživatele, ASP.NET hydráty tato vlastnost s ním. Pokud chcete zobrazit všechny deklarace identity, které se odesílají přes službu AD FS, uveďte zarážky v Controllers\HomeController.cs, v metodě akce indexu. Po ověření uživatele, zkontrolujte `System.Security.Claims.Current.Claims` kolekce.

![](./media/web-sites-dotnet-lob-application-adfs/12-test-debugging-all-claims.png) 

<a name="bkmk_authorize"></a>

## <a name="authorize-users-for-specific-controllers-or-actions"></a>Autorizace uživatelů pro konkrétní řadiče nebo akce
Vzhledem k tomu, že jste zahrnuli členství ve skupinách jako deklarace identity role ve vaší konfiguraci RP vztahu důvěryhodnosti, teď můžete například vytvořit přímo v `[Authorize(Roles="...")]` decoration pro kontrolery a akce. Obchodní aplikace pomocí vzoru vytvořit-čtení-aktualizace-odstranění (CRUD) můžete povolit konkrétní role pro přístup k každá akce. Prozatím se právě vyzkoušejte tuto funkci u existujícího řadiče domovské.

1. Otevřete Controllers\HomeController.cs.
2. Uspořádání `About` a `Contact` metody akce podobná následující kód, pomocí zabezpečení členství ve skupinách, které má ověřeného uživatele.  
   
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
   
    Vzhledem k tomu, že po přidání **testovací uživatele** k **testovací skupina** v mé testovacího prostředí služby AD FS I použijete testovací skupiny k testování autorizace na `About`. Pro `Contact`, I budete testovat záporné malá **Domain Admins**, do které **testovací uživatele** nepatří.
3. Spuštění ladicího programu zadáním `F5` a přihlaste se a pak klikněte na **o**. Má teď se zobrazil `~/About/Index` stránka úspěšně, pokud ověřený uživatel má oprávnění pro tuto akci.
4. Klikněte na tlačítko **kontaktujte**, v mém případě by neměl Autorizovat **testovacího uživatele** pro akci. V prohlížeči se však přesměruje na službu AD FS, který nakonec zobrazí tato zpráva:
   
    ![](./media/web-sites-dotnet-lob-application-adfs/13-authorize-adfs-error.png)
   
    Pokud byste prozkoumat této chybě v prohlížeči událostí na serveru služby AD FS, zobrazí tato zpráva o výjimce:  
   
    <pre class="prettyprint">
    Microsoft.IdentityServer.Web.InvalidRequestException: MSIS7042: <mark>The same client browser session has made '6' requests in the last '11' seconds.</mark> Contact your administrator for details.
       at Microsoft.IdentityServer.Web.Protocols.PassiveProtocolHandler.UpdateLoopDetectionCookie(WrappedHttpListenerContext context)
       at Microsoft.IdentityServer.Web.Protocols.WSFederation.WSFederationProtocolHandler.SendSignInResponse(WSFederationContext context, MSISSignInResponse response)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.ProcessProtocolRequest(ProtocolContext protocolContext, PassiveProtocolHandler protocolHandler)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.OnGetContext(WrappedHttpListenerContext context)
    </pre>
   
    Z důvodu této chyby je, že ve výchozím nastavení, MVC vrátí 401 Neautorizováno, pokud nemáte oprávnění role uživatele. Tím se spustí opětovné ověření požadavek na zprostředkovatele identity (AD FS). Vzhledem k tomu, že uživatel je již ověřen, služba AD FS vrátí na stejné stránce, která pak vydává jiný 401, vytváření smyčku přesměrování. Přepíše třídy AuthorizeAttribute na `HandleUnauthorizedRequest` metodu se zobrazit něco jednoduché logiky, která má smysl pokračovat smyčky přesměrování.
5. Vytvořte soubor v projektu názvem AuthorizeAttribute.cs a vložte následující kód do ní.
   
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
   
    Kód přepsání odešle v případech ověřený, ale neoprávněným HTTP 403 (zakázáno) místo protokolu HTTP 401 (Neautorizováno).
6. Spustit ladicí program znovu s `F5`. Kliknutím na tlačítko **kontaktujte** nyní zobrazuje informativnější (i když rozdělení nežádoucí) chybovou zprávu:
   
    ![](./media/web-sites-dotnet-lob-application-adfs/14-unauthorized-forbidden.png)
7. Publikování aplikace do Azure App Service Web Apps znovu a testování chování živé aplikace.

<a name="bkmk_data"></a>

## <a name="connect-to-on-premises-data"></a>Připojení k místním datům
Problémy s dodržováním předpisů s udržováním organizace dat mimo místní je důvod, že chcete implementovat-obchodní aplikace se službou AD FS místo Azure Active Directory. Také to může znamenat, že webové aplikace v Azure musí přístup k místní databáze, vzhledem k tomu, že nebudete moci používat [SQL Database](/services/sql-database/) jako datové vrstvy pro webové aplikace.

Azure App Service Web Apps podporuje přístupu k místní databáze s dva přístupy: [hybridní připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [virtuální sítě](web-sites-integrate-with-vnet.md). Další informace najdete v tématu [integrace pomocí virtuální sítě a hybridních připojení s Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).

<a name="bkmk_resources"></a>

## <a name="further-resources"></a>Další prostředky
* [Ověření pomocí místní služby Active Directory v aplikaci Azure](web-sites-authentication-authorization.md)
* [Vytvoření aplikace Azure-obchodní pomocí ověřování Azure Active Directory](web-sites-dotnet-lob-application-azure-ad.md)
* [Použijte možnost organizační ověřování místní (služby AD FS) pomocí technologie ASP.NET v sadě Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)
* [Migrovat VS2013 webového projektu ze WIF Katana](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
* [Přehled služby Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx)
* [Specifikace protokolu WS-Federation 1.1](http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf?S_TACT=105AGX04&S_CMP=LP)

