---
title: "aaaUsing systému pro správu identit napříč doménami automaticky zřízení uživatelů a skupin ze služby Azure Active Directory tooapplications | Microsoft Docs"
description: "Azure Active Directory, mohou automaticky poskytovat uživatelé a skupiny tooany aplikace nebo identity úložiště, které je přední stěnou webovou službou hello rozhraní definované v hello specifikace protokolu SCIM"
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 4d86f3dc-e2d3-4bde-81a3-4a0e092551c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;oldportal
ms.openlocfilehash: 43045c97e68d0d22db598dcb5ec23481c4e97718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-system-for-cross-domain-identity-management-tooautomatically-provision-users-and-groups-from-azure-active-directory-tooapplications"></a>Pomocí systému pro správu identit napříč doménami tooautomatically zřizování uživatelů a skupin ze služby Azure Active Directory tooapplications

## <a name="overview"></a>Přehled
Azure Active Directory (Azure AD), mohou automaticky poskytovat uživatelé a skupiny tooany aplikace nebo identity úložiště, které je přední stěnou webovou službou hello rozhraní definované v hello [systému pro napříč doménami Identity Management (SCIM) 2.0 Specifikace protokolu](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory můžete odesílat požadavky toocreate, upravit nebo odstranit přiřazené uživatelů a skupin toohello webové služby. Hello webové služby může překládat pak tyto požadavky do operací na úložiště identit cíl hello. 

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. 



![][0]
*Obrázek 1: Zřizování z úložiště identit Azure Active Directory tooan prostřednictvím webové služby*

Tato funkce může používá ve spojení s hello "přineste vlastní aplikace" funkcích ve službě Azure AD tooenable jednotné přihlašování a automatické zřizování pro aplikace, které poskytují nebo jsou přední stěnou webovou službou SCIM uživatelů.

Existují dva případy použití pro pomocí SCIM v Azure Active Directory:

* **Zřizování uživatelů a skupin tooapplications, které podporují SCIM** aplikace, které podporují SCIM 2.0 a používala tokeny nosičů OAuth pro ověřování pracuje s Azure AD bez konfigurace.
* **Vytvoření vlastního řešení zřizování pro aplikace, které podporují jiné zajišťování na základě rozhraní API** pro jiný SCIM aplikace, můžete vytvořit koncový bod tootranslate SCIM mezi koncový bod Azure AD SCIM hello a podporuje všechny rozhraní API hello aplikace pro zřizování uživatelů. toohelp vyvíjet koncový bod SCIM, poskytujeme knihovny společné jazykové infrastruktury (CLI) společně s ukázky kódu, které ukazují, jak toodo zadejte koncový bod SCIM a převede SCIM zprávy.  

## <a name="provisioning-users-and-groups-tooapplications-that-support-scim"></a>Zřizování uživatelů a skupin tooapplications, které podporují SCIM
Azure AD může být nakonfigurované tooautomatically přiřazené zřizování uživatelů a skupin tooapplications, implementovat [systému pro správu identit napříč doménami 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) webové služby a přijímat tokeny nosičů OAuth pro ověřování . V rámci specifikace hello SCIM 2.0 aplikace musí splňovat tyto požadavky:

* Podporuje vytváření uživatelů nebo skupin, podle části 3.3 hello SCIM protokolu.  
* Úprava uživatele nebo skupiny s požadavky patch podle části 3.5.2 hello SCIM protokol podporuje.  
* Podporuje načítání známých prostředků podle části 3.4.1 hello SCIM protokolu.  
* Podporuje dotazování uživatelů nebo skupin, podle části 3.4.2 hello SCIM protokolu.  Ve výchozím nastavení externalId se zadají dotaz uživatelé a skupiny se zadají dotaz displayName.  
* Dotazování uživatele podle ID a správcem podle části 3.4.2 hello SCIM protokol podporuje.  
* Podporuje dotazování skupin podle ID a členové podle části 3.4.2 hello SCIM protokolu.  
* Přijímá tokeny nosičů OAuth pro autorizaci podle části 2.1 hello SCIM protokolu.

Zeptejte se svého poskytovatele aplikace nebo v dokumentaci poskytovatele aplikace pro příkazy kompatibility s těmito požadavky.

### <a name="getting-started"></a>Začínáme
Aplikace, které podporují profil SCIM hello popsané v tomto článku může být připojené tooAzure služby Active Directory pomocí funkce hello "bez Galerie aplikace" v galerii aplikací Azure AD hello. Po spuštění připojené, Azure AD proces synchronizace každých 20 minut, kde vyžádá aplikace hello SCIM koncový bod pro přiřazené uživatele a skupiny a vytvoří nebo upraví je podle toohello podrobnosti o přiřazení.

**tooconnect aplikace, která podporuje SCIM:**

1. Přihlaste se příliš[hello portál Azure](https://portal.azure.com). 
2. Procházet příliš ** Azure Active Directory > podnikové aplikace a vyberte **novou aplikaci > všechny > aplikace bez Galerie**.
3. Zadejte název pro vaši aplikaci a klikněte na tlačítko **přidat** ikonu toocreate objekt aplikace.
    
  ![][1]
  *Obrázek 2: Galerii aplikací Azure AD*
    
4. Na obrazovce výsledné hello vyberte hello **zřizování** kartě v levém sloupci hello.
5. V hello **režimu zřizování** nabídce vyberte možnost **automatické**.
    
  ![][2]
  *Obrázek 3: Konfigurace zřizování v hello portálu Azure*
    
6. V hello **URL klienta** pole, zadejte adresu URL hello aplikace hello SCIM koncového bodu. Příklad: https://api.contoso.com/scim/v2/
7. Pokud koncový bod SCIM hello vyžaduje tokenu nosiče OAuth z vystavitele než Azure AD, tak kopie hello požadované tokenu nosiče OAuth do hello volitelné **tajný klíč tokenu** pole. Pokud toto pole je prázdné, Azure AD zahrnuty tokenu nosiče OAuth, který je vydán z Azure AD s každou žádostí. Aplikace, které používají Azure AD jako zprostředkovatele identity můžete ověřit tento Azure AD-vydán token.
8. Klikněte na tlačítko hello **Test připojení** tlačítko toohave Azure Active Directory pokus o tooconnect toohello SCIM koncový bod. Pokud hello pokusy selžou, zobrazí se informace o chybě.  
9. Pokud aplikace toohello tooconnect pokusy o hello úspěšné, pak klikněte na **Uložit** přihlašovací údaje správce toosave hello.
10. V hello **mapování** část, existují dvě sady vybrat mapování atributů: jeden pro uživatelské objekty a jeden pro objekty skupiny. Vyberte každý jeden tooreview hello atributy, které jsou synchronizované z tooyour aplikace Azure Active Directory. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelů a skupin v aplikaci pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.

    >[!NOTE]
    >Volitelně můžete zakázat synchronizaci objektů skupiny zakázáním hello "skupiny" mapování. 

11. V části **nastavení**, hello **oboru** pole definuje, které uživatele nebo skupiny jsou synchronizovány. Výběr "Synchronizace přiřadit pouze uživatelé a skupiny" (doporučeno) bude synchronizovat pouze uživatelé a skupiny přiřazené v hello **uživatelů a skupin** kartě.
12. Po dokončení konfiguraci změnit hello **Stav zřizování** příliš**na**.
13. Klikněte na tlačítko **Uložit** toostart hello zřizování služby Azure AD. 
14. Pokud synchronizaci pouze přiřazenou uživatelů a skupin (doporučeno), se že hello tooselect **uživatelů a skupin** a přiřaďte hello uživatelů nebo skupin, které chcete toosync.

Po zahájení hello počáteční synchronizaci, můžete hello **protokoly auditu** kartě toomonitor průběhu, který zobrazuje všechny akce prováděné hello zřizování služby ve vaší aplikaci. Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

>[!NOTE]
>počáteční synchronizace Hello trvá déle tooperform než následné synchronizace, ke kterým dochází přibližně každých 20 minut, dokud se službou hello. 


## <a name="building-your-own-provisioning-solution-for-any-application"></a>Vytváření vlastní zřizování řešení pro všechny aplikace
Vytvořením SCIM webová služba, která rozhraní s Azure Active Directory, můžete povolit jeden uživatel přihlašování a automatické zřizování prakticky jakékoli aplikace, která poskytuje zřizování rozhraní API REST nebo SOAP uživatelů.

Zde je, jak to funguje:

1. Azure AD poskytuje společné jazykové infrastruktury knihovny s názvem [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Systémoví integrátoři a vývojářům toocreate této knihovny a nasadit můžete použít koncový bod na základě SCIM webové služby lze připojit aplikace Azure AD tooany úložiště identit.
2. Mapování se implementují ve hello webové služby toomap hello standardizované schématu toohello uživatele schéma uživatele a protokol vyžaduje aplikace hello.
3. Adresa URL koncového bodu Hello je zaregistrován ve službě Azure AD jako součást vlastní aplikaci v galerii aplikací hello.
4. Uživatelé a skupiny přiřazené toothis aplikace ve službě Azure AD. Po přiřazení jsou vloženy do fronty toobe synchronizované toohello cílovou aplikaci. Proces synchronizace Hello zpracování hello fronty spouští každých 20 minut.

### <a name="code-samples"></a>Ukázky kódů
toomake to zpracovat snadnější, sadu [ukázky kódu jsou](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) jsou k dispozici, vytvořit koncový bod webové služby SCIM a ukázka automatické zřizování. Jeden ukázka je zprostředkovatele, který udržuje soubor s řádky představující uživatele a skupiny hodnot oddělených čárkami.  je Hello další zprostředkovatele, který funguje u služby Amazon Web Services identita a správa přístupu hello.  

**Požadavky**

* Visual Studio 2013 nebo novější
* [Azure SDK pro .NET](https://azure.microsoft.com/downloads/)
* Počítače s Windows, který podporuje toobe framework 4.5 ASP.NET hello používá jako hello SCIM koncový bod. Tento počítač musí být přístupné z cloudu hello
* [Předplatné Azure zkušební nebo licencovanou verzi Azure AD Premium](https://azure.microsoft.com/services/active-directory/)
* Ukázka Amazon AWS Hello vyžaduje knihovny z hello [AWS nástrojů pro Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). Další informace najdete v tématu hello README hello ukázka je součástí souboru.

### <a name="getting-started"></a>Začínáme
Hello nejjednodušší způsob, jak tooimplement, které SCIM koncový bod, který může přijmout zřizování požadavky z Azure AD je toobuild a nasaďte hello ukázka kódu, který produkuje hello zřízené uživatelé tooa textový soubor s oddělovači (CSV) souboru.

**toocreate SCIM koncový bod ukázka:**

1. Stáhněte si balíček ukázkový kód hello v [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2. Rozbalte balíček hello a umístěte ji na počítač se systémem Windows v umístění, jako je například C:\AzureAD-BYOA-Provisioning-Samples\.
3. V této složce spusťte hello FileProvisioningAgent řešení v sadě Visual Studio.
4. Vyberte **nástroje > Správce balíčků knihoven > Konzola správce balíčků**a spusťte následující příkazy pro hello FileProvisioningAgent tooresolve hello řešení odkazy na projekt hello:
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. Sestavení projektu FileProvisioningAgent hello.
6. Spuštění aplikace příkazového řádku hello v systému Windows (jako správce) a použít hello **cd** příkaz toochange hello directory tooyour **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** složky.
7. Spusťte následující příkaz, nahraďte < adresa > hello IP adresy nebo názvu domény počítače Windows hello hello:
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. V systému Windows v rámci **nastavení systému Windows > síť a Internet nastavení**, vyberte hello **brány Windows Firewall > Upřesnit nastavení**a vytvořit **příchozí pravidlo** , umožňuje příchozí přístup tooport 9000.
9. Pokud je počítač Windows hello za směrovačem, hello směrovač potřebám toobe nakonfigurované tooperform překlad síťových přístup mezi její port 9000, je zveřejněné toohello internet a port 9000 na počítači Windows hello. To je potřeba pro Azure AD toobe možné tooaccess tento koncový bod v cloudu hello.

**tooregister hello ukázka SCIM koncový bod ve službě Azure AD:**

1. Přihlaste se příliš[hello portál Azure](https://portal.azure.com). 
2. Procházet příliš ** Azure Active Directory > podnikové aplikace a vyberte **novou aplikaci > všechny > aplikace bez Galerie**.
3. Zadejte název pro vaši aplikaci a klikněte na tlačítko **přidat** ikonu toocreate objekt aplikace. objekt aplikace Hello vytvořený je určený toorepresent hello cílové aplikace by zřizování tooand implementace jeden pro přihlašování a ne jenom hello SCIM koncový bod.
4. Na obrazovce výsledné hello vyberte hello **zřizování** kartě v levém sloupci hello.
5. V hello **režimu zřizování** nabídce vyberte možnost **automatické**.
    
  ![][2]
  *Obrázek 4: Konfigurace zřizování v hello portálu Azure*
    
6. V hello **URL klienta** zadejte hello vystaven internetové adresy URL a port SCIM koncový bod. To by něco jako http://testmachine.contoso.com:9000 nebo http://<ip-address>:9000/, kde < adresa > je hello internet zveřejněné IP adresu.  
7. Pokud koncový bod SCIM hello vyžaduje tokenu nosiče OAuth z vystavitele než Azure AD, tak kopie hello požadované tokenu nosiče OAuth do hello volitelné **tajný klíč tokenu** pole. Pokud toto pole je prázdné, bude obsahovat Azure AD tokenu nosiče OAuth, který je vydán z Azure AD s každou žádostí. Aplikace, které používají Azure AD jako zprostředkovatele identity můžete ověřit tento Azure AD-vydán token.
8. Klikněte na tlačítko hello **Test připojení** tlačítko toohave Azure Active Directory pokus o tooconnect toohello SCIM koncový bod. Pokud hello pokusy selžou, zobrazí se informace o chybě.  
9. Pokud aplikace toohello tooconnect pokusy o hello úspěšné, pak klikněte na **Uložit** přihlašovací údaje správce toosave hello.
10. V hello **mapování** část, existují dvě sady vybrat mapování atributů: jeden pro uživatelské objekty a jeden pro objekty skupiny. Vyberte každý jeden tooreview hello atributy, které jsou synchronizované z tooyour aplikace Azure Active Directory. Hello atributy vybrán jako **párování** vlastnosti jsou použité toomatch hello uživatelů a skupin v aplikaci pro operace aktualizace. Vyberte toocommit tlačítko hello uložit změny.
11. V části **nastavení**, hello **oboru** pole definuje, které uživatele nebo skupiny jsou synchronizovány. Výběr "Synchronizace přiřadit pouze uživatelé a skupiny" (doporučeno) bude synchronizovat pouze uživatelé a skupiny přiřazené v hello **uživatelů a skupin** kartě.
12. Po dokončení konfiguraci změnit hello **Stav zřizování** příliš**na**.
13. Klikněte na tlačítko **Uložit** toostart hello zřizování služby Azure AD. 
14. Pokud synchronizaci pouze přiřazenou uživatelů a skupin (doporučeno), se že hello tooselect **uživatelů a skupin** a přiřaďte hello uživatelů nebo skupin, které chcete toosync.

Po zahájení hello počáteční synchronizaci, můžete hello **protokoly auditu** kartě toomonitor průběhu, který zobrazuje všechny akce prováděné hello zřizování služby ve vaší aplikaci. Další informace o zřizování hello Azure AD tooread jak protokolů najdete v tématu [zprávy o zřizování účtu automatické uživatele](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

ověření hello ukázka Hello posledním krokem je tooopen hello TargetFile.csv souboru ve složce \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug hello na počítač se systémem Windows. Jakmile se spustí hello procesu zřizování, tento soubor zobrazuje hello podrobnosti o veškerém přiřazené a zřízení uživatelů a skupin.

### <a name="development-libraries"></a>Vývojářské knihovny
toodevelop vlastní webové služby, který splňuje specifikace SCIM toohello, nejdřív seznámit se s hello poskytované společností Microsoft toohelp urychlit proces vývoje hello následující knihovny: 

1. Společné jazykové infrastruktury (CLI) knihovny jsou nabízena pro použití s jazyky na základě této infrastruktury, jako je například C#. Jedna z těchto knihoven [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), deklaruje rozhraní, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, znázorňuje následující obrázek hello: A Používání knihoven hello vývojáře by implementovat dané rozhraní s třídu, která může označovat, obecně jako zprostředkovatel. Hello knihovny povolte hello vývojáře toodeploy webové služby, který splňuje specifikace SCIM toohello. Hello webová služba může být buď hostovaný v rámci služby IIS nebo libovolného spustitelného souboru Common Language Infrastructure sestavení. Žádost je přeložit na zprostředkovatele toohello volání metody, které by být naprogramovaný tak podle hello vývojáře toooperate na některé úložiště identit.
  
  ![][3]
  
2. [Obslužné rutiny trasy Express](http://expressjs.com/guide/routing.html) jsou k dispozici pro analýzu node.js požadavek objekty, které představují volání (jak je definována hello SCIM specification), tooa node.js webové služby.   

### <a name="building-a-custom-scim-endpoint"></a>Vytváření koncový bod vlastní SCIM
Používání knihoven hello rozhraní příkazového řádku, vývojáře, kteří používají tyto knihovny hostování své služby v rámci všech spustitelný soubor sestavení Common Language Infrastructure, nebo Internetová informační služba. Tady je ukázkový kód pro hostování služby v rámci spustitelný soubor sestavení, na adrese hello http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Tato služba musí mít HTTP adresa a server ověřování certifikát z které hello kořenové certifikační autority je jedním z následujících hello: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Ověřovací certifikát serverů může být vázané tooa port na hostitele Windows hello nástroj shell sítě: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

Zde hello hodnota poskytnutá pro hello certhash argument je hello kryptografický otisk certifikátu hello, zatímco hello hodnota poskytnutá pro hello appid argument je libovolný identifikátor, globálně jedinečný.  

toohost hello služby v rámci Internetové informační služby, vývojář by pomocí třídy v oboru názvů výchozí hello hello sestavení s názvem spuštění sestavit sestavení knihovny CLA kódu.  Zde je ukázka této třídy: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a>Zpracování koncový bod ověřování
Žádosti z Azure Active Directory zahrnovat tokenu nosiče OAuth 2.0.   Každá žádost služby přijímající hello by měl ověřit hello vystavitele jako jménem klienta Azure Active Directory hello očekávání, pro přístup k toohello Azure Active Directory Graph webové služby Azure Active Directory.  V tokenu hello hello vystavitele identifikovaný deklaraci identity iss, například "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  V tomto příkladu hello základní adresa hello hodnoty deklarace identity, https://sts.windows.net, identifikuje Azure Active Directory jako hello vystavitele, zatímco hello relativní adresu segmentu, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, je jedinečný identifikátor hello Azure Active Klient Directory jménem které hello byl vydán token.  Pokud hello token vydán pro přístup k hello Azure Active Directory Graph webové služby, potom hello identifikátor této služby, 00000002-0000-0000-c000-000000000000, by měla být v hello hodnotu oblast hello token deklarace identity.  

Vývojáři pomocí knihovny CLA hello od společnosti Microsoft pro vytváření SCIM služby můžete ověřování žádostí z Azure Active Directory pomocí balíček Microsoft.Owin.Security.ActiveDirectory hello pomocí následujících kroků: 

1. U zprostředkovatele implementujte hello Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior vlastnost tak, že ho vrátit toobe metoda volána, když je spuštěna služba hello: 

  ````
    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }
  ````

2. Přidejte následující kód toothat metoda toohave hello všechny žádosti o tooany koncových bodů služby hello ověřený jako opatřené tokenem vydaným službou Azure Active Directory jménem zadaného klienta, pro přístup k toohello Azure AD Graph webové služby: 

  ````
    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute hello appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }
  ````


## <a name="user-and-group-schema"></a>Schéma uživatelů a skupin
Azure Active Directory můžete zřídit dva typy prostředků tooSCIM webových služeb.  Tyto typy prostředků jsou uživatelé a skupiny.  

Uživatel prostředky jsou označeny hello identifikátor schématu, urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, který je součástí této specifikace protokolu: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Hello výchozí mapování atributů hello uživatelů ve službě Azure Active Directory toohello atributy urn: ietf:params:scim:schemas:extension:enterprise:2.0:User prostředků je uvedené v tabulce 1, níže.  

Skupiny prostředků jsou identifikovány hello identifikátor schématu, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabulka 2 níže ukazuje hello výchozí mapování atributů hello skupin v Azure Active Directory toohello atributy http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group prostředků.  

### <a name="table-1-default-user-attribute-mapping"></a>Tabulka 1: Výchozí mapování atributů uživatele
| Uživatele Azure Active Directory | název urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| --- | --- |
| IsSoftDeleted |Aktivní |
| displayName |displayName |
| TelephoneNumber faxu |.value phoneNumbers [typ eq "fax"] |
| givenName |name.givenName |
| pracovní funkce |Název |
| E-mailu |.value e-mailů [typ eq "práce"] |
| mailNickname |externalId |
| Správce |Správce |
| mobilní |.value phoneNumbers [eq typ "mobilní"] |
| objectId |id |
| PSČ |adresy [typ eq "práce"] .postalCode |
| proxy adresy |[Zadejte eq "ostatní"] e-mailů. Hodnota |
| fyzický. doručení OfficeName |adresy [Zadejte eq "ostatní"]. Formátu |
| StreetAddress |adresy [typ eq "práce"] .streetAddress |
| Příjmení |name.familyName |
| telefonní číslo |.value phoneNumbers [typ eq "práce"] |
| PrincipalName uživatele |Uživatelské jméno |

### <a name="table-2-default-group-attribute-mapping"></a>Tabulka 2: Výchozí skupiny mapování atributů
| Skupiny Azure Active Directory | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| --- | --- |
| displayName |externalId |
| E-mailu |.value e-mailů [typ eq "práce"] |
| mailNickname |displayName |
| Členy |Členy |
| objectId |id |
| proxyAddresses |[Zadejte eq "ostatní"] e-mailů. Hodnota |

## <a name="user-provisioning-and-de-provisioning"></a>Zřizování uživatelů a jeho rušení
Následující obrázek ukazuje hello zprávy, že Azure Active Directory odešle životního cyklu tooa SCIM služby toomanage hello uživatele v jiné úložiště identit Hello. Hello diagram také ukazuje, jak SCIM implementovaná pomocí rozhraní příkazového řádku knihovny hello poskytovat službu Microsoft pro vytvoření, že tyto služby převede tyto požadavky na volání metody toohello zprostředkovatele.  

![][4]
*Obrázek 5: Zřizování uživatelů a jeho rušení pořadí*

1. Azure dotazů služby Active Directory hello služby pro uživatele s hodnotou atributu externalId odpovídající hodnota atributu mailNickname hello uživatele ve službě Azure AD. Hello dotazu je vyjádřena jako žádost protokolu HTTP (Hypertext Transfer), jako je tento příklad, ve kterém jyoung je ukázka mailNickname uživatele v Azure Active Directory: 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  Pokud služba hello je vytvořená pomocí knihovny Common Language Infrastructure hello od společnosti Microsoft pro implementaci služby SCIM, žádost hello přeložit na volání toohello dotazu metoda zprostředkovatele služeb hello.  Tady je hello podpis této metody: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);
  ````
  Zde je definice hello hello Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters rozhraní: 
  ````
    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }

    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }
  ````
  V následující ukázkový dotaz pro uživatele s danou hodnotou atributu externalId hello hello hodnoty hello argumentů předaných metoda dotazu toohello jsou: 
  * Parametry. AlternateFilters.Count: 1
  * Parametry. AlternateFilters.ElementAt(0). AttributePath: "externalId"
  * Parametry. AlternateFilters.ElementAt(0). Porovnávací operátor: ComparisonOperator.Equals
  * Parametry. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
  * correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. ID žádosti"] 

2. Pokud hello odpovědi tooa dotazu toohello webové služby pro uživatele s hodnotou atributu externalId odpovídající hodnota atributu mailNickname hello uživatele nevrátí žádné uživatele, pak Azure Active Directory požadavky tohoto poskytování služeb hello uživatele odpovídající toohello jednu v Azure Active Directory.  Tady je příklad této žádosti: 
  ````
    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}
  ````
  knihovny Common Language Infrastructure Hello od společnosti Microsoft pro implementaci služby SCIM by převede na volání toohello metodu Create zprostředkovatele služeb hello této žádosti.  Metoda Create Hello má tento podpis: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  V žádosti o tooprovision uživatele hello hello prostředků argument hodnotu instanci hello Microsoft.SystemForCrossDomainIdentityManagement. Třída Core2EnterpriseUser definovaných v hello Microsoft.SystemForCrossDomainIdentityManagement.Schemas knihovny.  Pokud hello požadavek tooprovision hello uživatele úspěšné, pak hello implementace metody hello je očekávané tooreturn instanci hello Microsoft.SystemForCrossDomainIdentityManagement. Třída Core2EnterpriseUser s hodnotou hello hello identifikátor vlastnosti nastavit toohello jedinečný identifikátor uživatele nově zřízeného hello.  

3. tooupdate uživatele známé tooexist v úložišti identity přední stěnou pomocí SCIM, Azure Active Directory bude pokračovat podle požaduje hello aktuální stav daného uživatele ze služby hello s žádostí. například: 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  Ve službě sestaven pomocí knihovny Common Language Infrastructure hello od společnosti Microsoft pro implementaci služby SCIM je požadavek hello přeložit na toohello volání metody načtení poskytovatele služeb hello.  Tady je hello podpis metody načtení hello: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);

    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }
  ````
  V příkladu hello požadavek tooretrieve hello aktuálního stavu uživatele hello hodnoty vlastností hello objektu hello zadaný jako hodnota hello argumentu hello parametry jsou následující: 
  
  * Identifikátor: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

4. Pokud atribut typu odkaz toobe aktualizován, pak Azure Active Directory dotazy hello služby toodetermine zda hello aktuální hodnotu atributu hello odkaz v úložišti identity hello přední stěnou pomocí služby hello již odpovídá hello hodnotu tohoto atributu. v Azure Active Directory. Pro uživatele hello pouze z které hello aktuální hodnota je dotazován tímto způsobem je atribut hello správce. Tady je příklad požadavek toodetermine zda atribut manager hello objektu konkrétní uživatel aktuálně má určitou hodnotu: 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  Hello hodnota parametru dotazu hello atributy, id, označuje, která by splnila hello výraz zadaný jako hodnota hello parametru dotazu filtru hello pokud existuje objekt uživatele, pak služba hello je očekávané toorespond s urn: ietf:params:scim:schemas: Jádro: 2.0:User nebo urn: ietf:params:scim:schemas:extension:enterprise:2.0:User prostředků, včetně pouze hello hodnota atributu id tohoto zdroje.  Hello hodnotu hello **id** atribut je známý toohello žadatele. Je součástí hello hodnota parametru dotazu filtru hello; účelem Hello žádostí o jeho je ve skutečnosti toorequest minimální reprezentace prostředku, který neodpovídajících výraz filtru hello jako indikaci, jestli všechny například objekt již existuje.   

  Pokud služba hello je vytvořená pomocí knihovny Common Language Infrastructure hello od společnosti Microsoft pro implementaci služby SCIM, žádost hello přeložit na volání toohello dotazu metoda zprostředkovatele služeb hello. Hodnota Hello hello vlastnosti objektu hello zadaný jako hodnota hello argumentu hello parametry jsou následující: 
  
  * Parametry. AlternateFilters.Count: 2
  * Parametry. AlternateFilters.ElementAt(x). AttributePath: "id"
  * Parametry. AlternateFilters.ElementAt(x). Porovnávací operátor: ComparisonOperator.Equals
  * Parametry. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * Parametry. AlternateFilters.ElementAt(y). AttributePath: "správce"
  * Parametry. AlternateFilters.ElementAt(y). Porovnávací operátor: ComparisonOperator.Equals
  * Parametry. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
  * Parametry. RequestedAttributePaths.ElementAt(0): "id"
  * Parametry. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

  Zde hello hodnota indexu hello x může být 0 a hello hodnotu hello index y může být 1, nebo hello hodnota x může být 1 a hello hodnotu y, může být 0, v závislosti na hello pořadí hello výrazy hello parametr dotazu filtru.   

5. Tady je příklad požadavku z Azure Active Directory tooan SCIM služby tooupdate uživatele: 
  ````
    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
  ````
  knihovny Microsoft Common Language Infrastructure Hello pro implementaci služby SCIM by převede hello požadavek na volání toohello aktualizační metody poskytovatele služeb hello. Tady je hello podpis hello metoda aktualizace: 
  ````
    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }

    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }

      public string Value
      { get; set; }
    }
  ````
    V příkladu hello tooupdate požadavku uživatele má zadaný jako hodnota hello argumentu oprava hello objekt hello hodnoty těchto vlastností: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
  * (PatchRequest jako PatchRequest2). Operations.Count: 1
  * (PatchRequest jako PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
  * (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "správce"
  * (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.Count: 1
  * (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Referenční dokumentace: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
  * (PatchRequest jako PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Hodnota: 2819c223-7f76-453a-919d-413861904646

6. toode-provision uživatele z identity úložiště přední stěnou služba SCIM, Azure AD, jako odešle žádost: 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  Pokud služba hello je vytvořená pomocí knihovny Common Language Infrastructure hello od společnosti Microsoft pro implementaci služby SCIM, žádost hello přeložit na volání toohello metodu Delete zprostředkovatele služeb hello.   Tato metoda má tento podpis: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  zadaný jako hodnota hello argumentu resourceIdentifier hello Hello objekt má tyto hodnoty vlastností v příkladu hello žádost toode-provision uživatele: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

## <a name="group-provisioning-and-de-provisioning"></a>Zřizování skupiny a jeho rušení
Následující obrázek ukazuje hello zprávy, že Azure AcD odešle životního cyklu tooa SCIM služby toomanage hello skupiny v jiné úložiště identit Hello.  Tyto zprávy se liší od hello zprávy, která se týkají toousers třemi způsoby: 

* Hello schéma skupiny prostředků je označený jako http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  
* Požadavky, že tooretrieve skupiny stanovení tento atribut členy hello je toobe vyloučené z jakémukoli prostředku, zadaný v požadavku toohello odpovědi.  
* Toodetermine žádosti o tom, jestli atribut typu odkaz má určitou hodnotu se žádostí o hello členů atributu.  

![][5]
*Obrázek 6: Zřizování skupiny a jeho rušení pořadí*

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Automatizace zřizování uživatelů nebo jeho rušení tooSaaS aplikace](active-directory-saas-app-provisioning.md)
* [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
* [Zapisují se výrazy pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtry pro zřizování uživatelů oborů](active-directory-saas-scoping-filters.md)
* [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
* [Seznam kurzů tooIntegrate aplikace SaaS](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
