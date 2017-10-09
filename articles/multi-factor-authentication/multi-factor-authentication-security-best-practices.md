---
title: "aaaSecurity osvědčené postupy pro vícefaktorového ověřování | Microsoft Docs"
description: "Tento dokument obsahuje osvědčené postupy kolem účty Azure pomocí Azure MFA"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3be7d968-96bb-4320-8701-869fd04a2595
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 7f18c2592764878b842d81783b321a05f29ee3d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Osvědčené postupy zabezpečení pro používání ověřování Azure Multi-Factor Authentication s účty Azure AD

Dvoustupňové ověření je hello preferované volbou pro většinu organizací, které mají tooenhance jejich procesu ověřování. Azure Multi-Factor Authentication (MFA) pomáhá společností splňují požadavky na jejich zabezpečení a dodržování předpisů při současném poskytování jednoduché prostředí přihlašování pro své uživatele. Tento článek se zabývá některé tipy, které byste měli zvážit při plánování pro přijetí hello Azure mfa.

## <a name="deploy-azure-mfa-in-hello-cloud"></a>Nasazení Azure MFA v cloudu hello

Existují dva způsoby tooenable Azure MFA pro všechny uživatele.

* Zakoupit licence pro každého uživatele (buď Azure MFA, Azure AD Premium nebo Enterprise Mobility + Security)
* Vytvoření poskytovatele Multi-Factor Auth a platím na uživatele nebo podle ověření

### <a name="licenses"></a>Licence
![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Pokud máte Azure AD Premium nebo Enterprise Mobility + Security licence, už máte Azure MFA. Vaše organizace nepotřebuje cokoli další tooextend hello dvoustupňové ověření schopnosti tooall uživatele. Potřebujete jenom tooassign uživatel tooa licencí a pak můžete zapnout vícefaktorové ověřování.

Při nastavování služby Multi-Factor Authentication, vezměte v úvahu hello následující tipy:

* Nevytvářejte zprostředkovatel vícefaktorového ověřování na ověřování. V takovém případě může skončili platícího pro žádosti o ověření od uživatelů, které už máte licence.
* Pokud nemáte dost licencí pro všechny uživatele, můžete vytvořit jednotlivé uživatele zprostředkovatel vícefaktorového ověřování toocover hello rest vaší organizace. 
* Azure AD Connect je jenom potřeba, pokud synchronizujete prostředí služby Active Directory místní adresář služby Azure AD. Pokud používáte adresář služby Azure AD, který není synchronizován s místní instancí Active Directory, není nutné Azure AD Connect.

### <a name="multi-factor-auth-provider"></a>Zprostředkovatel vícefaktorového ověřování
![Zprostředkovatel vícefaktorového ověřování](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Pokud nemáte licence, které zahrnují Azure MFA, můžete vytvořit poskytovatele ověřování MFA. 

Při vytváření hello zprostředkovatel ověřování, potřebujete tooselect adresáře a zvažte hello následující podrobnosti:

* Není nutné toocreate adresář služby Azure AD zprostředkovatel vícefaktorového ověřování, ale můžete získat další funkce s jednou. Pokud přidružíte hello zprostředkovatel ověřování adresář služby Azure AD jsou povoleny Hello následující funkce:  
  * Rozšíření tooall dvoustupňové ověření uživatelé  
  * Globální správci nabízí další funkce, jako je například hello portálu pro správu, vlastní přivítání a sestav.
* Pokud synchronizujete prostředí služby Active Directory v místě s adresářem služby Azure AD, je třeba DirSync nebo AAD Sync. Pokud používáte adresář služby Azure AD, který není synchronizován s místní instancí Active Directory, není nutné DirSync nebo AAD Sync.
* Vyberte model spotřeby hello, který nejlépe vyhovuje vaší firmy. Jakmile vyberete modelu využití hello, už nemůžete provést změnu. jsou dva modely Hello:
  * Za ověření: vám poplatky za každé ověření. Pokud chcete dvoustupňové ověřování pro každý uživatel, který přistupuje k určité aplikaci, ne pro konkrétní uživatele, použijte tento model.
  * Za povoleného uživatele: poplatky můžete pro každého uživatele, můžete povolit pro Azure MFA. Pokud máte někteří uživatelé s Azure AD Premium nebo Enterprise Mobility Suite licencí a některé bez použití tohoto modelu.

### <a name="supportability"></a>Možnosti podpory
Vzhledem k tomu, že většina uživatelů jsou uzpůsobené toousing tooauthenticate pouze hesla, je důležité, aby vaše společnost informuje uživatele tooall týkající se tohoto procesu. Toto sledování může snížit hello pravděpodobnost, že uživatelé volání pro menší problémy související tooMFA vaše oddělení technické podpory. Existují však některých scénářích, kde je nutné dočasně zakázat MFA. Hello použijte následující pokyny toounderstand jak toohandle tyto scénáře:

* Cvičení scénářů toohandle pracovníci technické podpory kde hello uživatele nelze přihlásit, protože hello mobilní aplikaci nebo phone nepřijímá oznámení nebo telefonní hovor. Můžete se na technickou podporu [povolit jednorázové přihlášení](multi-factor-authentication-whats-next.md#one-time-bypass) tooallow tooauthenticate uživatele jednorázově "vynecháte" dvoustupňové ověřování. Hello jednorázové přihlášení je dočasné a vyprší po zadaném počtu sekund.
* Vezměte v úvahu hello [důvěryhodné IP adresy schopností](multi-factor-authentication-whats-next.md#trusted-ips) v Azure MFA jako způsob toominimize dvoustupňové ověřování. Pomocí této funkce mohou správci klienta spravované nebo federované obejít dvoustupňové ověřování pro uživatele, kteří se přihlašují ze společnosti hello místní intranet. Hello funkce jsou dostupné pro klienty Azure AD, kteří mají licence Azure AD Premium, Enterprise Mobility Suite nebo Azure Multi-Factor Authentication.

## <a name="best-practices-for-an-on-premises-deployment"></a>Osvědčené postupy pro místní nasazení
Pokud vaše společnost se rozhodla tooleverage svou vlastní infrastrukturu tooenable vícefaktorového ověřování, musíte toodeploy serveru Azure Multi-Factor Authentication Server na místě. součásti serveru MFA Hello se zobrazuje hello následující diagram:

![Výchozí součásti serveru MFA: konzoly, synchronizační modul, portálu pro správu, Cloudová služba](./media/multi-factor-authentication-security-best-practices/server.png) \*není ve výchozím nastavení nainstalovaná \** nainstalován, ale není povoleno ve výchozím nastavení

Azure Multi-Factor Authentication Server můžete zabezpečit cloudové prostředky a místní prostředky pomocí federace. Musí mít služby AD FS a mějte ho sdružených se službou klientovi Azure AD.
Při nastavování aplikace Multi-Factor Authentication Server, vezměte v úvahu hello následující podrobnosti:

* Pokud jsou zabezpečení prostředků Azure AD pomocí služby Active Directory Federation Services (AD FS), pak hello prvním krokem ověření se provádí místně pomocí služby AD FS. druhým krokem Hello je prováděn v místě dodržením deklarace identity hello.
* Nemáte tooinstall hello Azure Multi-Factor Authentication Server federační server služby AD FS. Ale hello Multi-Factor Authentication Adapter pro AD FS musí být nainstalován na službou AD FS Windows serveru 2012 R2. Můžete nainstalovat hello server na jiném počítači, dokud se jedná o podporovanou verzi a instalovat hello AD FS adapteru samostatně na federační server služby AD FS. 
* Průvodce instalací služby Multi-Factor Authentication AD FS adapteru Hello vytvoří skupinu zabezpečení s názvem PhoneFactor Admins ve službě Active Directory a potom přidá toothis skupinu účtů služby AD FS. Ověřte, zda hello skupiny PhoneFactor Admins byl vytvořen na řadiči domény a že účet služby hello AD FS členem této skupiny. V případě potřeby přidejte hello služby AD FS služby účet toohello skupiny PhoneFactor Admins ručně na vašem řadiči domény.

### <a name="user-portal"></a>Uživatelský portál
Hello uživatelského portálu umožňuje schopnosti samoobslužné služby a poskytuje úplnou sadu funkcí správy uživatelů. Spustí se v webovému serveru Internet Information Server (IIS). Použijte následující pokyny tooconfigure hello této součásti:

* Pomocí služby IIS 6 nebo vyšší.
* Nainstalujte a zaregistrujte ASP.NET v2.0.507207
* Ujistěte se, že tento server se dá nasadit v hraniční síti

### <a name="app-passwords"></a>Hesla aplikací
Pokud je vaše organizace Federovaná pro jednotné přihlašování s Azure AD a budete toobe pomocí Azure MFA, pak Uvědomte hello následující podrobnosti:

* heslo aplikace Hello je ověřit pomocí služby Azure AD a proto obchází federace. Federace se používá pouze při nastavování hesla aplikací.
* Hesla pro federované uživatele (SSO), jsou uložena v id organizace hello. Pokud hello uživatel odejde hello společnosti, má tento informace id tooorganizational tooflow pomocí nástroje DirSync. Zakázání/odstranění účtu může trvat hodiny toosync toothree, který zpoždění zakázání/odstranění hesla aplikací ve službě Azure AD.
* Místní nastavení služby Access Control klienta není dodrženo heslem aplikace.
* Žádné místní ověřování protokolování nebo auditování funkce je dostupná pro hesla aplikací.
* Některé pokročilé architektury návrhů může vyžadovat při použití dvoustupňové ověřování s klienty, v závislosti na tom, kde se ověřují pomocí kombinace organizační uživatelského jména a hesla aplikace. Pro klienty, kteří se ověřují se na místní infrastrukturu byste použili organizační uživatelské jméno a heslo. Pro klienty, kteří se ověřují se Azure AD použijete heslo aplikace hello.
* Ve výchozím nastavení uživatelé nemůžou vytvářet hesla aplikací. Pokud potřebujete hesla aplikace toocreate tooallow uživatelů, vyberte hello **povolit uživatelům toocreate aplikace hesla toosign do neprohlížečových aplikací** možnost.

## <a name="additional-considerations"></a>Další informace
Pomocí tohoto seznamu Další informace a pokyny pro jednotlivé komponenty, který je nasazen na místě:

- Nastavení služby Azure Multi-Factor Authentication se službou [Active Directory Federation Services (AD FS)](multi-factor-authentication-get-started-adfs.md).
- Nastavení a konfigurace hello Azure MFA serveru s [ověřování pomocí protokolu RADIUS](multi-factor-authentication-get-started-server-radius.md).
- Nastavení a konfigurace hello Azure MFA serveru s [ověřování služby IIS](multi-factor-authentication-get-started-server-iis.md).
- Nastavení a konfigurace hello Azure MFA serveru s [ověřování systému Windows](multi-factor-authentication-get-started-server-windows.md).
- Nastavení a konfigurace hello Azure MFA serveru s [ověřování pomocí protokolu LDAP](multi-factor-authentication-get-started-server-ldap.md).
- Nastavení a konfigurace hello Azure MFA serveru s [Brána vzdálené plochy a pomocí protokolu RADIUS serveru Azure Multi-Factor Authentication Server](multi-factor-authentication-get-started-server-rdg.md).
- Nastavení a konfiguraci synchronizace mezi hello Azure MFA serveru a [Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md).
- [Nasazení hello Azure Multi-Factor Authentication Server webové služby mobilní aplikace](multi-factor-authentication-get-started-server-webservice.md).
- [Rozšířené konfigurace sítě VPN s ověřováním Azure Multi-Factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) pro zařízení Cisco ASA, Citrix Netscaler a Juniper/Pulse Secure VPN pomocí protokolu LDAP nebo RADIUS.

## <a name="next-steps"></a>Další kroky
Při tomto článku klade důraz některé osvědčené postupy pro Azure MFA, existují další prostředky, které můžete použít také při plánování nasazení vícefaktorového ověřování. Hello seznam dole obsahuje některé klíče články, které mohou pomoci během tohoto procesu:

* [Sestavy v Azure Multi-Factor Authentication](multi-factor-authentication-manage-reports.md)
* [Hello dvoustupňové ověření registrace prostředí](multi-factor-authentication-end-user-first-time.md)
* [Nejčastější dotazy k Azure Multi-Factor Authentication](multi-factor-authentication-faq.md)

