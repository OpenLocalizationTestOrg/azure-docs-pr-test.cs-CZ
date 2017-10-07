---
title: "Azure AD Connect: Požadavky a hardware | Microsoft Docs"
description: "Toto téma popisuje hello předpoklady a hello hardwarové požadavky pro Azure AD Connect"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 91b88fda-bca6-49a8-898f-8d906a661f07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2ec8b5da9440c92e8f9d6702d5e5e20de952b588
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-for-azure-ad-connect"></a>Požadavky pro Azure AD Connect
Toto téma popisuje požadavky hello a hello hardwarové požadavky pro Azure AD Connect.

## <a name="before-you-install-azure-ad-connect"></a>Před instalací Azure AD Connect
Před instalací Azure AD Connect, existuje pár věcí, které potřebujete.

### <a name="azure-ad"></a>Azure AD
* Předplatné Azure nebo [zkušební předplatné](https://azure.microsoft.com/pricing/free-trial/). Toto předplatné je pouze požadované pro přístup k hello portál Azure a ne pro pomocí služby Azure AD Connect. Pokud používáte prostředí PowerShell nebo Office 365, není nutné toouse předplatné Azure AD Connect. Pokud máte licenci pro Office 365, můžete taky použít portál hello Office 365. S placenou licencí Office 365 můžete také získat do hello portál Azure z portálu hello Office 365.
  * Můžete taky hello [portál Azure](https://portal.azure.com). Tento portál nevyžaduje licenci Azure AD.
* [Přidání a ověření domény hello](../active-directory-domains-add-azure-portal.md) plánování toouse ve službě Azure AD. Například pokud plánování toouse contoso.com pro vaše uživatele, zkontrolujte tato doména byla ověřena a pouze nepoužíváte výchozí doménu contoso.onmicrosoft.com hello.
* Klient služby Azure AD umožňuje výchozích 50 tisíc objektů. Když je ověřit doménu, hello limit je vyšší too300k objekty. Pokud potřebujete ještě další objekty ve službě Azure AD, musíte tooopen omezení podpory případu toohave hello vyšší ještě víc. Pokud potřebujete více než 500 tisíc objektů, potřebujete licenci, jako je například Office 365, Azure AD Basic, Azure AD Premium nebo Enterprise Mobility a zabezpečení.

### <a name="prepare-your-on-premises-data"></a>Příprava vaše místní data
* Použití [IdFix](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) tooidentify chyby jako např. duplicitní položky a formátování problémy ve vašem adresáři před synchronizací tooAzure AD a Office 365.
* Zkontrolujte [funkce volitelné sync můžete povolit ve službě Azure AD](active-directory-aadconnectsyncservice-features.md) a vyhodnocení funkcí, které byste měli povolit.

### <a name="on-premises-active-directory"></a>Místní služby Active Directory
* Hello AD schéma verze a doménová struktura úroveň funkčnosti musí být Windows Server 2003 nebo novější. Hello řadičů domény můžete spustit všechny verze, tak dlouho, dokud hello schématu a doménové struktury úrovně požadavků.
* Pokud máte v plánu toouse hello funkce **zpětný zápis hesla**, hello řadiče domény musí být v systému Windows Server 2008 (s nejnovější SP) nebo novější. Pokud jsou vaše řadiče domény v 2008 (pre-R2), pak musíte také použít [opravu hotfix KB2386717](http://support.microsoft.com/kb/2386717).
* musí být zapisovatelný řadič domény Hello používá Azure AD. Je **nepodporuje** toouse řadiče jen pro čtení (řadič domény jen pro čtení) a Azure AD Connect nedodrží všechny zápisu přesměrování.
* Je **nepodporuje** toouse místními doménovými strukturami nebo doménami pomocí domény SLD (jeden štítek domény).
* Je **nepodporuje** toouse místními doménovými strukturami nebo doménami pomocí "s tečkami" (název obsahuje dobou ".") Názvy pro rozhraní NetBios.
* Doporučuje se příliš[povolit odpadkový koš služby Active Directory hello](active-directory-aadconnectsync-recycle-bin.md).

### <a name="azure-ad-connect-server"></a>Server Azure AD Connect
* Azure AD Connect nejde nainstalovat na Small Business Server nebo Windows Server Essentials. Hello server musí používat systém Windows Server standard nebo vyšší.
* Hello Azure AD Connect server musí mít úplným grafickým uživatelským rozhraním nainstalována. Je **nepodporuje** tooinstall na jádru serveru.
* Azure AD Connect musí být nainstalován v systému Windows Server 2008 nebo novějším. Tento server může být řadič domény nebo členském serveru, při použití Expresní nastavení. Pokud chcete použít vlastní nastavení, hello server může být také samostatné a nemá toobe tooa připojené k doméně.
* Pokud Azure AD Connect instalujete na Windows Server 2008 nebo Windows Server 2008 R2, zkontrolujte že tooapply hello nejnovější opravy hotfix z webu Windows Update. Hello instalace není možné toostart s serveru bez nainstalované opravy.
* Pokud máte v plánu toouse hello funkce **synchronizace hesel**, server hello Azure AD Connect musí být na Windows Server 2008 R2 SP1 nebo novější.
* Pokud máte v plánu toouse **skupiny účet spravované služby**, server hello Azure AD Connect musí být v systému Windows Server 2012 nebo novějším.
* musí mít server Hello Azure AD Connect [rozhraní .NET Framework 4.5.1](#component-prerequisites) nebo novější a [Microsoft prostředí PowerShell 3.0](#component-prerequisites) nainstalovaný nebo novější.
* server Azure AD Connect Hello nesmí mít zásady skupiny přepis prostředí PowerShell povolené.
* Pokud se nasazuje Active Directory Federation Services, hello servery, kde jsou nainstalovány služby AD FS nebo Proxy webových aplikací, musí být Windows Server 2012 R2 nebo novější. [Vzdálená správa systému Windows](#windows-remote-management) musí být povolené na těchto serverech pro vzdálenou instalaci.
* Pokud se nasazuje Active Directory Federation Services, je třeba [certifikáty SSL](#ssl-certificate-requirements).
* Pokud se nasazuje Active Directory Federation Services, pak musíte tooconfigure [překlad názvů](#name-resolution-for-federation-servers).
* Pokud globální Správci mají povolené ověřování MFA, pak hello URL **https://secure.aadcdn.microsoftonline-p.com** musí být v seznamu důvěryhodných serverů hello. Jste výzvami tooadd této lokality toohello seznamu důvěryhodných serverů po zobrazení výzvy pro výzvu MFA a nebyl přidán před. Můžete použít aplikaci Internet Explorer tooadd ho tooyour Důvěryhodné servery.

### <a name="sql-server-used-by-azure-ad-connect"></a>SQL Server používá Azure AD Connect
* Azure AD Connect vyžaduje dat identity toostore databáze systému SQL Server. Ve výchozím nastavení je nainstalována SQL Server 2012 Express LocalDB (světla verze systému SQL Server Express). SQL Server Express má limit velikosti 10GB, která vám umožní toomanage přibližně 100 000 objektů. Pokud potřebujete toomanage větší rozsah objektů adresáře, je třeba toopoint hello instalace Průvodce tooa jinou instalaci systému SQL Server.
* Pokud budete používat samostatný SQL Server, potom použijte tyto požadavky:
  * Azure AD Connect podporuje všechny typů systému Microsoft SQL Server ze systému SQL Server 2008 (s nejnovější aktualizaci Service Pack) tooSQL Server 2016 SP1. Microsoft Azure SQL Database je **nepodporuje** jako databáze.
  * Je nutné použít velká a malá písmena kolace SQL. Tyto kolace jsou označeny \_CI_ v názvu. Je **nepodporuje** toouse malá a velká písmena kolace, identifikovaný \_CS_ v názvu.
  * Může mít pouze jeden synchronizační modul na jednu instanci SQL. Je **nepodporuje** tooshare SQL instanci s FIM nebo MIM Sync, DirSync nebo Azure AD Sync.

### <a name="accounts"></a>Účty
* Účet Azure AD globálního správce pro klienta hello Azure AD, které chcete toointegrate s. Tento účet musí být **školní nebo organizace účet** a nemůže být **účtu Microsoft**.
* Pokud používáte Expresní nastavení nebo upgradu z nástroje DirSync, musí mít účet správce podnikové sítě pro místní služby Active Directory.
* [Účty ve službě Active Directory](active-directory-aadconnect-accounts-permissions.md) Pokud hello vlastní nastavení instalace cestu použít.

### <a name="connectivity"></a>Připojení
* server Hello Azure AD Connect musí překlad názvů DNS pro intranetu i Internetu. Hello DNS server musí být schopný tooresolve názvy tooyour místní služby Active Directory a hello koncové body Azure AD.
* Pokud máte brány firewall na intranetu a potřebujete tooopen porty mezi servery hello Azure AD Connect a řadičů domény, pak v tématu [Azure AD Connect porty](active-directory-aadconnect-ports.md) Další informace.
* Pokud váš server proxy nebo brány firewall limit, kterému může získat přístup na adresy URL, pak hello adresy URL, které jsou dokumentovány v článku [Office 365 adresy URL a rozsahy IP adres](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) musí být otevřen.
  * Pokud používáte hello cloudu Microsoftu v Německu nebo hello cloudu Microsoft Azure Government, pak najdete v části [služba Azure AD Connect sync instance aspekty](active-directory-aadconnect-instances.md) pro adresy URL.
* Azure AD Connect je ve výchozím nastavení pomocí protokolu TLS 1.0 toocommunicate s Azure AD. Tato tooTLS 1.2 můžete změnit pomocí následujících kroků hello v [povolení TLS 1.2 pro Azure AD Connect](#enable-tls-12-for-azure-ad-connect).
* Pokud používáte odchozího proxy serveru pro připojení toohello Internet, hello následující nastavení v hello **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** soubor musí být přidán pro Průvodce instalací hello a Azure AD Connect sync toobe možné tooconnect toohello Internet a Azure AD. Tento text je třeba zadat v dolní části hello hello souboru. V tomto kódu &lt;PROXYADRESS&gt; představuje hello skutečné proxy IP adresu nebo název hostitele.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Pokud proxy server vyžaduje ověřování, pak hello [účet služby](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) musí být umístěn v hello domény a použijete hello přizpůsobit nastavení instalace cesta toospecify [účet vlastní služby](active-directory-aadconnect-get-started-custom.md#install-required-components). Budete také potřebovat toomachine.config různé změny. Díky této změně v souboru machine.config hello instalace průvodce a synchronizace stroj reagovat tooauthentication požadavků z proxy serveru hello. Na všech stránkách Průvodce instalací, s výjimkou hello **konfigurace** stránku hello přihlášení uživatele se používají přihlašovací údaje. Na hello **konfigurace** stránka na konci hello tohoto průvodce instalací hello, hello kontextu je vypnuté toohello [účet služby](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) který byl vytvořen vy. oddíl machine.config Hello by měla vypadat takto.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Pokud Azure AD Connect odešle jako součást synchronizace adresářů webový požadavek tooAzure AD, Azure AD může trvat až toorespond too5 minut. Je běžné pro konfiguraci časový limit nečinnosti připojení toohave servery proxy. Zkontrolujte, zda text hello konfigurace je nastavena tooat minimálně 6 minut nebo déle.

Další informace najdete v tématu MSDN o hello [výchozí proxy Element](https://msdn.microsoft.com/library/kd3cf2ex.aspx).  
Další informace najdete v případě, že máte problémy s připojením, [řešení problémů s připojením](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Ostatní
* Volitelné: Testovací uživatele účtu tooverify synchronizaci.

## <a name="component-prerequisites"></a>Součást požadavky
### <a name="powershell-and-net-framework"></a>Prostředí PowerShell a rozhraní .net Framework
Azure AD Connect, závisí na Microsoft PowerShell a rozhraní .NET Framework 4.5.1. Je nutné tuto verzi nebo novější verze na serveru nainstalována. V závislosti na vaší verze systému Windows Server hello následující:

* Windows Server 2012 R2
  * Microsoft PowerShell je nainstalován ve výchozím nastavení. Není vyžadována žádná akce.
  * Rozhraní .NET framework 4.5.1 a pozdějších verzích jsou nabízena prostřednictvím služby Windows Update. Ujistěte se, že jste nainstalovali nejnovější aktualizace tooWindows hello serveru v hello ovládací panely.
* Windows Server 2008 R2 a Windows Server 2012
  * je k dispozici v nejnovější verzi Microsoft PowerShell Hello **Windows Management Framework 4.0**, k dispozici na [Microsoft Download Center](http://www.microsoft.com/downloads).
  * Rozhraní .NET framework 4.5.1 a pozdějších verzích jsou dostupné na [Microsoft Download Center](http://www.microsoft.com/downloads).
* Windows Server 2008
  * Hello nejnovější podporované verze prostředí PowerShell je k dispozici v **Windows Management Framework 3.0**, k dispozici na [Microsoft Download Center](http://www.microsoft.com/downloads).
  * Rozhraní .NET framework 4.5.1 a pozdějších verzích jsou dostupné na [Microsoft Download Center](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>Povolit protokol TLS 1.2 pro Azure AD Connect
Azure AD Connect je pomocí protokolu TLS 1.0 ve výchozím nastavení pro šifrování komunikace hello mezi hello synchronizační modul server a Azure AD. Toto můžete změnit tak, že nakonfigurujete toouse aplikace .net TLS 1.2 ve výchozím nastavení na serveru hello. Další informace o protokolu TLS 1.2 naleznete v [2960358 informační zpravodaj zabezpečení společnosti Microsoft](https://technet.microsoft.com/security/advisory/2960358).

1. Protokol TLS 1.2 nelze povolit v systému Windows Server 2008. Je třeba Windows Server 2008 R2 nebo novější. Ověřte, že je nainstalována hello technologie .net 4.5.1 oprava hotfix pro operační systém najdete v tématu [2960358 informační zpravodaj zabezpečení společnosti Microsoft](https://technet.microsoft.com/security/advisory/2960358). Můžete mít tato oprava hotfix nebo novější verze na serveru již nainstalován.
2. Pokud používáte Windows Server 2008 R2, zkontrolujte, jestli že je zapnutá TLS 1.2. Na server Windows Server 2012 a novějších verzích by měl být již povolena TLS 1.2.
   ```
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   ```
3. Pro všechny operační systémy nastavte tento klíč registru a restartujte hello server.
   ```
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
   "SchUseStrongCrypto"=dword:00000001
   ```
4. Pokud chcete taky tooenable TLS 1.2 mezi hello synchronizační modul server a vzdálený SQL Server a potom zkontrolujte, zda máte nainstalované pro verze hello požadované [podpora protokolu TLS 1.2 pro Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Předpoklady pro federační instalace a konfigurace
### <a name="windows-remote-management"></a>Vzdálená správa systému Windows
Pokud používáte Azure AD Connect toodeploy Active Directory Federation Services nebo hello Proxy webových aplikací, zkontrolujte tyto požadavky:

* Pokud cílový server hello připojený k doméně, pak se ujistěte, zda je povoleno vzdálené spravované Windows
  * V PSH příkaz okno se zvýšenými oprávněními použijte příkaz`Enable-PSRemoting –force`
* Pokud je cílový server hello WAP počítač připojený k jiné doméně-, pak existuje několik dalších požadavků
  * V cílovém počítači hello (WAP počítači):
    * Ujistěte se, vzdálené správy systému Windows hello (Vzdálená správa systému Windows nebo WS-Management) je spuštěna služba prostřednictvím modulu snap-in služby hello
    * V PSH příkaz okno se zvýšenými oprávněními použijte příkaz`Enable-PSRemoting –force`
  * Na počítači hello, na které hello je spuštěný Průvodce (Pokud hello cílový počítač je připojený k nebo nedůvěryhodné doméně nepřipojená k doméně):
    * V PSH příkaz okno se zvýšenými oprávněními použijte příkaz hello`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
    * Ve Správci serveru:
      * Přidejte fond toomachine hostitele hraniční sítě WAP (Správce serveru -> Správa -> Přidat servery..., použijte kartu DNS)
      * Karta servery všechny správce serveru: klikněte pravým tlačítkem na WAP server a zvolte spravovat jako..., zadejte přihlašovací údaje pro místní (domény) pro počítač WAP hello
      * toovalidate vzdáleného PSH připojení, v hello kartě všechny servery správce serveru: klikněte pravým tlačítkem na WAP server a zvolte prostředí Windows PowerShell. By měla otevřít relaci vzdálené PSH by bylo možné navázat tooensure vzdálené relace prostředí PowerShell.

### <a name="ssl-certificate-requirements"></a>Požadavky na certifikát SSL
* Důrazně doporučujeme toouse hello stejný certifikát protokolu SSL mezi všechny uzly farmu služby AD FS a všechny proxy servery webových aplikací.
* Hello certifikát musí být X509 certifikátu.
* Certifikát podepsaný svým držitelem můžete použít na federačních serverech v prostředí testovací laboratoře. Pro produkční prostředí, ale doporučujeme si opatřit certifikát hello od veřejné certifikační Autority.
  * Pokud používáte certifikát, který není veřejně důvěryhodný, ujistěte se, že hello certifikát nainstalovaný na každém serveru Proxy webových aplikací je důvěryhodný na místním serveru obou hello a na všech federačních serverech
* Hello identity hello certifikátu musí odpovídat hello název federační služby (např. sts.contoso.com).
  * Hello identity je buď subjektu (SAN) pro alternativní název rozšíření o dNSName typu nebo, pokud nejsou žádné položky sítě SAN, zadaný název subjektu hello jako běžný název.  
  * Několik záznamů SAN mohou existovat v hello certifikát zadaný jeden z nich odpovídá hello název federační služby.
  * Pokud plánujete toouse připojení k pracovní ploše, další sítě SAN je vyžadován s hodnotou hello **enterpriseregistration.** Následuje hello hlavní název uživatele (UPN) příponu vaší organizace, například **enterpriseregistration.contoso.com**.
* Na základě klíčů další nové generace (CNG) rozhraní CryptoAPI a poskytovatelů úložiště klíčů certifikátů nejsou podporovány. To znamená, že musíte použít certifikát na základě CSP (zprostředkovatel kryptografických služeb) a ne na KSP (Zprostředkovatel úložiště klíčů).
* Zástupný znak-certifikáty jsou podporovány.

### <a name="name-resolution-for-federation-servers"></a>Překlad názvů pro federační servery
* Nastavte záznamy DNS pro hello název služby FS (např. sts.contoso.com) služby AD FS pro intranetové hello (vaše interní server DNS) i hello extranetu (veřejném DNS prostřednictvím doménového registrátora). Záznam DNS intranetu hello, ujistěte se, že používáte A záznamů a není záznamy CNAME. To je potřeba pro toowork ověřování systému windows správně z počítače připojeného k doméně.
* Pokud nasazujete víc než jeden server služby AD FS nebo služby proxy webové aplikace, pak se ujistěte, že jste nakonfigurovali nástroj pro vyrovnávání zatížení a zda hello záznamy DNS pro hello AD FS federation service name (např. sts.contoso.com) bodu toohello nástroj pro vyrovnávání zatížení.
* Pro toowork integrované ověřování systému windows pro aplikace prohlížeče Internet Explorer v intranetu zajistěte, že hello název federační služby AD FS (např. sts.contoso.com) je přidána toohello zóny intranetu v aplikaci Internet Explorer. To se dá řídit pomocí zásad skupiny a nasazené tooall vaší doméně počítače.

## <a name="azure-ad-connect-supporting-components"></a>Podpora součásti Azure AD Connect
Hello následuje seznam součástí, které Azure AD Connect nainstaluje na server hello nainstalovanou Azure AD Connect. Tento seznam je pro základní Expresní instalace. Pokud se rozhodnete toouse jiný Server SQL na hello nainstalujte stránka služby synchronizace, pak není místně nainstalovaný SQL Express LocalDB.

* Azure AD Connect Health
* Microsoft Online Services Sign-In Pomocníka pro IT profesionály (nainstalovanou ale žádná závislost na něm)
* Nástroje příkazového řádku Microsoft SQL Server 2012
* Microsoft SQL Server 2012 Express LocalDB
* Nativní klient Microsoft SQL Server 2012
* Microsoft Visual C++ 2013 opětovnou distribuci balíčku

## <a name="hardware-requirements-for-azure-ad-connect"></a>Požadavky na hardware pro Azure AD Connect
Následující tabulka Hello ukazuje hello minimální požadavky na počítači se hello Azure AD Connect sync.

| Počet objektů ve službě Active Directory | Procesor | Memory (Paměť) | Velikost pevného disku |
| --- | --- | --- | --- |
| Méně než 10 000 |1,6 GHz |4 GB |70 GB |
| 10,000–50,000 |1,6 GHz |4 GB |70 GB |
| 50,000–100,000 |1,6 GHz |16 GB |100 GB |
| U 100 000 nebo více objektů se vyžaduje plnou verzi systému SQL Server hello | | | |
| 100,000–300,000 |1,6 GHz |32 GB |300 GB |
| 300,000–600,000 |1,6 GHz |32 GB |450 GB |
| Více než 600 000 |1,6 GHz |32 GB |500 GB |

Hello minimální požadavky pro počítače se službou AD FS nebo servery webových aplikací je hello následující:

* Využití procesoru: Duální základní 1,6 GHz nebo vyšší
* PAMĚŤ: 2 GB nebo vyšší
* Virtuálního počítače Azure: Konfigurace A2 nebo vyšší

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
