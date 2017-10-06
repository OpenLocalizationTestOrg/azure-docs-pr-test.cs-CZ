---
title: "aaaManage přístup toocloud aplikace tak, že omezíte klienti – Azure | Microsoft Docs"
description: "Jak toomanage toouse omezení klienta, které uživatelé můžou používat aplikace založené na jejich klienta Azure AD."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: kgremban
ms.openlocfilehash: 6470fa217738b29104353ae17a2f53216f825c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-tenant-restrictions-toomanage-access-toosaas-cloud-applications"></a>Použijte omezení klienta toomanage přístup tooSaaS cloudové aplikace

Velké organizace, které zdůraznil zabezpečení toomove toocloud služby, jako je Office 365, ale chcete třeba tooknow, kterému mají přístup jenom uživatelé schválení prostředky. Společností tradičně, omezit názvů domény nebo IP adresy, když chtějí toomanage přístup. Tento postup selže ve světě, kde jsou hostované aplikace SaaS ve veřejném cloudu, na názvy sdílených domén jako outlook.office.com a login.microsoftonline.com spuštěna. Blokování tyto adresy by zabránit uživatelům v přístupu k aplikaci Outlook na webu hello zcela, místo prostého je omezení tooapproved identity a prostředky.

Azure Active Directory na řešení toothis výzvy je funkci omezení klienta. Klienta omezení umožňuje organizacím toocontrol přístup tooSaaS cloudové aplikace, které jsou založeny na použití o klienta hello aplikace hello Azure AD pro jednotné přihlašování. Například aplikace Office 365 tooallow přístup tooyour organizace, můžete při brání přístupu tooother organizace instancí těchto stejné aplikací.  

Omezení klienta poskytuje organizacím hello možnost toospecify hello seznam klientů, jejich mají uživatelé tooaccess. Azure AD poté pouze uděluje přístup toothese povolené klientů.

Tento článek zaměřuje na omezení klienta pro Office 365, ale hello funkce by měly spolupracovat s všech cloudových aplikací SaaS, která používá moderní ověřování protokoly s Azure AD pro jednotné přihlašování. Pokud používáte SaaS aplikace s jinou Azure AD klienta z klienta hello používá Office 365, ujistěte se, že všechny požadované, klienti jsou povoleny. Další informace o cloudových aplikací SaaS, najdete v části hello [Active Directory Marketplace](https://azure.microsoft.com/en-us/marketplace/active-directory/).

## <a name="how-it-works"></a>Jak to funguje

Hello celkového řešení zahrnuje hello následující součásti: 

1. **Azure AD** – Pokud hello `Restrict-Access-To-Tenants: <permitted tenant list>` je přítomen, Azure AD jen povolené problémy tokenů zabezpečení pro hello klientům. 

2. **Místní proxy server infrastruktury** – proxy zařízení schopné kontrolu SSL, nakonfigurované tooinsert hello záhlaví obsahující seznam hello klientům povoleny ve provoz určený pro Azure AD. 

3. **Klientský software** – toosupport omezení klienta, klientský software musí požádat tokeny přímo z Azure AD, tak, aby provoz může být zachycen infrastruktury služby proxy hello. Omezení klienta se aktuálně podporuje založené na prohlížeči aplikace Office 365 a klienti Office při použití moderní ověřování (např. OAuth 2.0). 

4. **Moderní ověřování** – cloudové služby musíte použít toouse moderní ověřování klienta omezení a zablokování přístupu tooall klienty není povolené. Office 365 cloudové služby musí být nakonfigurované toouse moderních ověřovacích protokolů ve výchozím nastavení. Hello nejnovější informace o podporovaných Office 365 pro moderní ověřování, najdete v tématu [moderní ověřování Office 365 aktualizovat](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/).

Hello následující diagram znázorňuje tok přenosů vysoké úrovně hello. Kontrolu SSL je vyžadováno pouze u přenosů tooAzure AD, není toohello Office 365 cloudové služby. Tento rozdíl je důležitý, protože hello objem provozu pro ověřování tooAzure AD je obvykle mnohem nižší než provoz svazku tooSaaS aplikace, jako je Exchange Online a SharePoint Online.

![Klienta omezení přenosový tok – diagram](./media/active-directory-tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Nastavení omezení klienta

Existují dva kroky tooget začít s omezení klienta. prvním krokem Hello je toomake jistotu, že vaši klienti můžou připojit toohello správné adresy. Hello druhou je tooconfigure infrastruktury proxy serveru.

### <a name="urls-and-ip-addresses"></a>Adresy URL a IP adresy

toouse omezení klienta, klienti musí být schopný tooconnect toohello následující adresy URL služby Azure AD tooauthenticate: login.microsoftonline.com, login.microsoft.com a login.windows.net. Kromě toho tooaccess Office 365, klienti musí být také možné tooconnect toohello plně kvalifikované názvy domény nebo adresy URL a IP adresy definované v [Office 365 adresy URL a rozsahy IP adres](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2). 

### <a name="proxy-configuration-and-requirements"></a>Konfigurace proxy serveru a požadavky

Hello následující konfigurace je požadovaná tooenable omezení klienta prostřednictvím infrastruktury proxy serveru. V tomto návodu je obecný, měli byste tedy dokumentaci od dodavatele tooyour proxy pro konkrétní implementaci.

#### <a name="prerequisites"></a>Požadavky

- Hello proxy musí být schopný tooperform SSL zachycení, vkládání hlavičky protokolu HTTP a filtr cíle pomocí plně kvalifikovaných názvů domény nebo adresy URL. 

- Klienti musí důvěřovat řetězu certifikátů hello předložený hello proxy pro komunikaci SSL. Například pokud se používají certifikáty z interní infrastruktury veřejných KLÍČŮ, hello interní vydávající certifikát kořenové certifikační autority musí být důvěryhodný.

- Tato funkce je zahrnutá v předplatná Office 365, ale pokud chcete, aby toouse omezení klienta toocontrol přístup tooother SaaS aplikace jsou nutné pak licence Azure AD Premium 1.

#### <a name="configuration"></a>Konfigurace

Pro každou příchozí požadavek toologin.microsoftonline.com, login.microsoft.com a login.windows.net vložit dvě hlavičky protokolu HTTP: *omezit přístup k klienty* a *omezit-Access-kontext*.

Hello hlavičky by měla obsahovat hello následující prvky: 
- Pro *omezit přístup k klienty*, hodnota \<povolené klienta seznamu\>, což je čárkami oddělený seznam klientů, které chcete, aby uživatelé tooaccess tooallow. Do libovolné domény, která je zaregistrovaná u klienta může být použité tooidentify hello klienta v tomto seznamu. Například toopermit přístup klientů tooboth Contoso a Fabrikam, hello vypadá dvojice název hodnota jako:`Restrict-Access-To-Tenants: contoso.onmicrosoft.com,fabrikam.onmicrosoft.com` 
- Pro *omezit-Access-kontext*, hodnotu a ID jednoho adresářového deklarace, které klient je nastavení omezení klienta hello. Například toodeclare Contoso jako hello klienta, který nastavit hello klienta omezení zásad, dvojici název – hodnota hello vypadá takto:`Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d`  

> [!TIP]
> Vaše ID adresáře můžete najít v hello [portál Azure](https://portal.azure.com). Přihlaste se jako správce, vyberte **Azure Active Directory**, pak vyberte **vlastnosti**.

tooprevent uživatele z vkládání vlastní hlavičku HTTP s neschválené klienty, hello je proxy server tooreplace hello omezit přístup k klienty záhlaví Pokud se již nachází v hello příchozího požadavku. 

Klienti musí být vynucené toouse hello proxy pro všechny požadavky toologin.microsoftonline.com, login.microsoft.com a login.windows.net. Například pokud jsou soubory PAC použité toodirect klienti toouse hello proxy, koncoví uživatelé by neměl být schopný tooedit nebo zakázat hello PAC soubory.

## <a name="hello-user-experience"></a>činnost koncového uživatele Hello

Tato část uvádí hello prostředí pro koncové uživatele a správce.

### <a name="end-user-experience"></a>Činnost koncového uživatele

Uživatel s příklad je v síti Contoso hello, ale se pokouší tooaccess hello Fabrikam instance sdílené SaaS aplikace jako je Outlook online. Pokud Contoso-povolené klienta pro tuto instanci, hello uživateli se zobrazí následující stránka hello:

![Přístup odepřen stránku pro uživatele v klientech – povoleno](./media/active-directory-tenant-restrictions/end-user-denied.png)

### <a name="admin-experience"></a>Z pohledu správce

Během konfigurace omezení klienta se provádí na hello podnikový proxy server infrastruktury, správci hello omezení klienta k sestavám můžete přistupovat v hello portál Azure přímo. tooview hello sestavy, přejděte toohello stránku Přehled služby Azure Active Directory a potom podívejte se do části Další možnosti.

Dobrý den, správce pro tenanta hello zadané jako hello s omezeným přístupem-Access-kontext klienta můžete použít tuto sestavu toosee všechny přihlášení blokovaný z důvodu hello klienta omezení zásad, včetně identity hello používá a ID hello cílový adresář.

![Použít hello Azure portálu tooview omezený pokusů o přihlášení](./media/active-directory-tenant-restrictions/portal-report.png)

Jako další sestavy v hello portálu Azure můžete použít filtry toospecify hello oboru sestavy. Můžete filtrovat podle konkrétního uživatele, aplikace, klient nebo časovém intervalu.

## <a name="office-365-support"></a>Podpora Office 365

Aplikace Office 365, musí splňovat dvě kritéria toofully podpora omezení klienta:

1. podporuje Hello klient používá moderní ověřování
2. Jako ověřovací protokol hello výchozí hello Cloudová služba je povolené moderní ověřování.

Odkazovat příliš[moderní ověřování Office 365 aktualizovat](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/) hello nejnovější informace, na které Office klienti v současné době podporují moderní ověřování. Této stránce taky obsahuje odkazy tooinstructions pro povolení moderní ověřování na konkrétní Exchange Online a Skype for Business Online klientů. Ve výchozím nastavení ve službě SharePoint Online je již povolené moderní ověřování.

Omezení klienta v současné době podporuje aplikace založené na prohlížeči Office 365 (hello služby SharePoint portálu Office, Yammer, lokality, Outlook na hello webové atd.). Pro silné klienty (Outlook, Skype pro firmy, Word, Excel, PowerPoint, atd.) Omezení klienta můžete vynutit pouze, pokud se používá moderní ověřování.  

Outlook a Skype pro obchodní klientů, které podporují moderní ověřování jsou stále možné toouse starších protokolů pro klienty, kde není povolené moderní ověřování, efektivně obcházení omezení klienta. Pro aplikaci Outlook v systému Windows zákazníci zvolit tooimplement omezení koncovým uživatelům brání přidání profilů tootheir účty neschválený e-mailu. Například v tématu hello [zabránit přidání jiné než výchozí účty serveru Exchange](http://gpsearch.azurewebsites.net/default.aspx?ref=1) nastavení zásad skupiny. Pro aplikaci Outlook na platformách systému Windows a pro Skype pro firmy na všech platformách není aktuálně k dispozici plnou podporu pro omezení klienta.

## <a name="testing"></a>Testování

Pokud chcete tootry limit omezení klienta před implementací pro celou organizaci, máte dvě možnosti: použití metody založené na hostiteli pomocí nástroje, například aplikaci Fiddler nebo dvoufázové instalace zavedení nastavení proxy serveru.

### <a name="fiddler-for-a-host-based-approach"></a>Fiddler přístup, založené na hostiteli

Fiddler je bezplatných webových ladění proxy server, který lze použít toocapture a upravit provoz protokolu HTTP nebo HTTPS, včetně vkládání hlavičky protokolu HTTP. tooconfigure Fiddler tootest omezení klienta, proveďte následující kroky hello:

1.  [Stáhněte a nainstalujte aplikaci Fiddler](http://www.telerik.com/fiddler).
2.  Konfigurace provoz HTTPS toodecrypt aplikaci Fiddler, za [nápovědě k aplikaci Fiddler na](http://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).
3.  Nakonfigurujte aplikaci Fiddler tooinsert hello *omezit přístup k klienty* a *omezit-Access-kontext* hlavičky pomocí vlastní pravidla:
  1. V nástroji hello ladicí program webové aplikaci Fiddler, vyberte hello **pravidla** nabídku a vyberte **přizpůsobit pravidla...** soubor CustomRules tooopen hello.
  2. Přidejte následující řádky na začátku hello hello hello *OnBeforeRequest* funkce. Nahraďte \<doména klienta\> s doménou zaregistrována vašeho klienta, například contoso.onmicrosoft.com. Nahraďte \<ID adresáře\> s identifikátorem Azure AD GUID vašeho klienta.

  ```
  if (oSession.HostnameIs("login.microsoftonline.com") || oSession.HostnameIs("login.microsoft.com") || oSession.HostnameIs("login.windows.net")){      oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";      oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";}
  ```

  Pokud potřebujete tooallow víc klientů, použijte názvy čárkami tooseparate hello klienta. Například:

  ```
  oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";
  ```

4. Uložte a zavřete soubor CustomRules hello.

Po dokončení konfigurace aplikaci Fiddler, můžete zaznamenávat provoz podle budete toohello **soubor** nabídky a výběrem **zachycování provozu**.

### <a name="staged-rollout-of-proxy-settings"></a>Dvoufázové instalace zavedení nastavení proxy serveru

V závislosti na možnosti hello infrastruktury proxy serveru může být schopný toostage hello zavedení nastavení tooyour uživatelů. Zde jsou základní možnosti několik důvodů:

1.  Používejte PAC soubory toopoint testovací uživatele tooa testovací proxy infrastrukturu, při normálním uživatelé dál infrastruktury služby proxy produkční toouse hello.
2.  Některé servery proxy může podporovat různé konfigurace pomocí skupin.

Konkrétní podrobnosti naleznete v dokumentaci k systému tooyour proxy server.

## <a name="next-steps"></a>Další kroky

- Přečtěte si informace o [moderní ověřování Office 365 aktualizovat](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)

- Zkontrolujte hello [Office 365 adresy URL a rozsahy IP adres](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
