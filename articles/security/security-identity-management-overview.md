---
title: "aaaAzure bezpečnostní funkce, které pomáhají se správou identit | Microsoft Docs"
description: " Tento článek obsahuje přehled funkcí Azure zabezpečení jádra hello, které pomáhají se správou identit. Microsoft identit a přístupu řešení Nápověda pro správu IT chránit tooapplications přístup a prostředky v podnikovém datovém centru hello a do cloudu hello povolení další úrovně ověřování, jako je vícefaktorové ověřování a podmíněného zásady přístupu. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: terrylan
ms.openlocfilehash: f08e4f6cf2e48e455a16858b7fee08b53d5aa585
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-identity-management-security-overview"></a>Přehled zabezpečení správy identit Azure
Microsoft identit a přístupu řešení Nápověda pro správu IT chránit tooapplications přístup a prostředky v podnikovém datovém centru hello a do cloudu hello povolení další úrovně ověřování, jako je vícefaktorové ověřování a podmíněného zásady přístupu. Monitorování podezřelé aktivity přes pokročilé zabezpečení vytváření sestav a auditování, výstrahy, pomáhá zmírnit potenciální potíže se zabezpečením. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) poskytuje jeden toothousands přihlášení (SaaS) cloudových aplikací a aplikací tooweb přístup spustit místně.

Výhody zabezpečení služby Azure Active Directory (AD) zahrnují možnost hello:

* Vytvořit a spravovat jedinou identitu pro každého uživatele v rámci podniku hybridní udržování synchronizace uživatele, skupiny a zařízení
* Zadejte přístup přihlášení tooyour aplikace včetně tisícům předem integrovaných aplikací SaaS
* Povolit aplikaci přístup k zabezpečení vynucením vícefaktorového ověřování založeného na pravidlech pro místní i cloudové aplikace.
* Poskytování zabezpečeného vzdáleného přístupu tooon místní webové aplikace prostřednictvím proxy aplikace služby Azure AD

Hello cílem tohoto článku je tooprovide přehled hello základní zabezpečení Azure funkcí, které pomáhají se správou identit. Poskytujeme také tooarticles odkazy, které poskytují podrobnosti o každé funkce tak další informace.  

Hello článek se zaměřuje na hello následující funkce správy identit Azure jádra:

* Jednotné přihlašování
* Reverzní proxy server
* Ověřování pomocí služby Multi-Factor Authentication
* Sledování zabezpečení, výstrahy a sestavy na základě learning počítače
* Správa identit a přístupu zákazníků
* Registrace zařízení
* Správa privilegovaných identit
* Ochrana identit
* Hybridní Správa identit

## <a name="single-sign-on"></a>Jednotné přihlašování
Jednotné přihlašování (SSO) se rozumí se může tooaccess všechny hello aplikace a prostředky, které potřebujete toodo podnikání, po přihlášení pouze jednou pomocí jediného uživatelského účtu. Jakmile se přihlásíte, dostanete všechny aplikace hello budete potřebovat, aniž by musel být požadované tooauthenticate (zadejte například heslo) ještě jednou.

Mnoho organizací závisí software jako služba (SaaS) aplikace například Office 365, pole a Salesforce pro produktivita koncového uživatele. V minulosti tooindividually IT pracovníci potřeba vytvářet a aktualizovat uživatelské účty v každé aplikaci SaaS, a uživatelé měli tooremember heslo pro každou aplikaci SaaS.

Azure AD rozšiřuje prostředí místní služby Active Directory do cloudu hello povolení uživatelé toouse primární účet organizace toonot pouze přihlášení tootheir zařízení připojených k doméně a prostředkům společnosti, ale také všechny hello webu a aplikace SaaS potřebné pro své úlohy.

Jenom uživatelé nemají toomanage více sad uživatelských jmen a hesel, přístup k aplikaci lze automaticky zřízeného nebo zrušte zřízené na základě organizační skupiny a jejich stav jako zaměstnanec. Azure AD nabízí zabezpečení a řízení přístupu zásad správného řízení, které umožňují toocentrally můžete spravovat přístup uživatelů v rámci aplikací SaaS.

Další informace:

* [Přehled jednotného přihlašování](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
* [Integrovat Azure Active Directory jednotné přihlašování s aplikacemi SaaS](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Reverzní proxy server
Proxy aplikace služby Azure AD umožňuje publikovat místní aplikace, jako například [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) lokalit, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx), a [IIS](http://www.iis.net/)-aplikací uvnitř vaší privátní sítě a poskytuje zabezpečený přístup toousers mimo vaši síť. Proxy aplikace nabízí vzdáleného přístupu a jednotné přihlašování (SSO) pro mnoho typů místní webové aplikace s hello tisíců aplikací SaaS, které podporuje Azure AD. Zaměstnanci se můžete přihlásit tooyour aplikace z domácí na jejich vlastní zařízení a ověřování prostřednictvím této cloudové proxy.

Další informace:

* [Povolení proxy aplikace služby Azure AD](../active-directory/active-directory-application-proxy-enable.md)
* [Publikování aplikací pomocí proxy aplikace služby Azure AD](../active-directory/active-directory-application-proxy-publish.md)
* [Jednotné přihlášení pomocí Proxy aplikace](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
* [Práce s podmíněným přístupem](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Ověřování pomocí služby Multi-Factor Authentication
Azure Multi-Factor authentication (MFA) je metoda ověřování, který vyžaduje hello použití více než jednu metodu ověřování a přidá velmi důležitou druhou vrstvu zabezpečení toouser přihlášení a transakce. MFA pomáhá chránit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení. Zajišťuje silné ověřování přes celou řadu možností ověření – telefonní hovor, textová zpráva nebo mobilní aplikace oznámení nebo ověřovací kód a třetích stran tokeny OAuth.

Další informace:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* [Jak funguje Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Sledování zabezpečení, výstrahy a sestavy na základě learning počítače
Sledování zabezpečení a výstrah a na základě learning sestav počítače, které identifikují nekonzistentní přístupové vzorce vám umožní chránit vaši firmu. Můžete použít Azure Active Directory přístup a použití sestav toogain přehled hello integrity a zabezpečení adresáře vaší organizace. Tyto informace a správce directory pomohou určit, kde může být bezpečnostním rizikům, tak, aby se adekvátní naplánovat toomitigate těchto rizik.

V hello portál Azure classic sestavy jsou rozdělené do hello následující způsoby:

* Sestavy anomálií – obsahovat přihlášení, jsme našli toobe neobvyklé události. Naším cílem je toomake si vědom tyto aktivity a díky kterému budete mít toomake toobe rozhodnutí o tom, zda je událost podezřelé.
* Integrované sestavy aplikací – poskytují přehled o tom, jak cloudové aplikace jsou používány ve vaší organizaci. Azure Active Directory umožňuje integraci s tisíci cloudových aplikací.
* Zprávy o chybách – znamenat chyby, které mohou nastat při zřizování účtů tooexternal aplikace.
* Uživatelská sestavy – zobrazí zařízení nebo přihlášení v data aktivit pro konkrétního uživatele.
* Protokoly aktivity – obsahovat záznam všech auditované události v rámci hello posledních 24 hodin, 7 dní, nebo posledních 30 dnů a změny aktivity skupiny a poslední aktivita resetování a registraci hesla.

Další informace:

* [Zobrazení sestav přístupů a používání](../active-directory/active-directory-view-access-usage-reports.md)
* [Začínáme s Azure Active Directory vytváření sestav](../active-directory/active-directory-reporting-getting-started.md)
* [Průvodce vytvářením sestav Azure Active Directory](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Správa identit a přístupu zákazníků
Azure Active Directory B2C je vysokou dostupností, globální, služba identity management určených aplikací, škáluje toohundreds milionů identit. Dá se integrovat do mobilních i webových platforem. Uživatele mohou přihlásit tooall aplikace prostřednictvím přizpůsobitelné pomocí svých účtů na sociálních nebo vytvořením nové přihlašovací údaje.

V posledních hello byste chtěli toosign nahoru a přihlaste se příjemci do svých aplikací vývojáři aplikací napsali vlastní kód. A použili by místní databáze nebo systémy toostore uživatelských jmen a hesel. Azure Active Directory B2C nabízí vaší organizace lepší způsob toointegrate správu identit uživatelů do aplikace pomocí hello zabezpečené, na standardech postavené platformy a velké sady rozšiřitelných zásad.

Pokud používáte Azure Active Directory B2C, vaši uživatelé mohou zaregistrovat do pro vaše aplikace pomocí svých účtů na sociálních (Facebook, Google, Amazon, LinkedIn) nebo vytvořením nové přihlašovací údaje (e-mailovou adresu a heslo, nebo uživatelské jméno a heslo).

Další informace:

* [Co je Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C ve verzi preview: Přihlaste registrace a přihlašování uživatelů aplikace](../active-directory-b2c/active-directory-b2c-overview.md)
* [Azure Active Directory B2C ve verzi Preview: Typy aplikací](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Registrace zařízení
Registrace zařízení služby Azure AD je hello základ pro zařízení na základě [podmíněného přístupu](../active-directory/active-directory-conditional-access-device-registration-overview.md) scénáře. Když je zařízení registrováno, poskytuje Azure Active Directory Device Registration hello zařízení s identitou, která je použité tooauthenticate hello zařízení při přihlášení uživatele hello. Hello ověření zařízení a atributy hello hello zařízení, lze potom použít tooenforce zásady podmíněného přístupu pro aplikace, které jsou hostované v cloudu hello a místní.

V kombinaci s řešením pro správu (MDM) mobilních zařízení, jako je například Intune, hello atributy zařízení ve službě Azure Active Directory jsou aktualizované o další informace o zařízení hello. Díky tomu můžete toocreate pravidla podmíněného přístupu, které vynucují přístup ze zařízení toomeet vaše standardy zabezpečení a dodržování předpisů.

Další informace:

* [Začínáme s Azure Active Directory Device Registration](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Automatická registrace zařízení v Azure Active Directory pro Windows připojená k doméně](../active-directory/active-directory-conditional-access-automatic-device-registration.md)
* [Nastavit automatické registrace Windows připojená k doméně se službou Azure Active Directory](../active-directory/active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="privileged-identity-management"></a>Správa privilegovaných identit
Azure Active Directory (AD) Privileged Identity Management vám umožňuje spravovat, řídit a monitorovat privilegované identity a přístupu tooresources ve službě Azure AD, jakož i jiných služeb Microsoft online services jako je Office 365 nebo Microsoft Intune.

Uživatelé někdy potřebují toocarry out privilegované operace v Azure nebo Office 365 prostředků nebo jiných aplikací SaaS. Často to znamená organizace mají toogive je trvalé privilegovaný přístup v Azure AD. Toto je rostoucí bezpečnostní riziko pro hostované cloudové prostředky, protože organizace nelze monitorovat dostatečně tyto činnosti uživatelů s jejich oprávněními správce. Kromě toho pokud uživatelský účet s privilegovaného přístupu je ohrožen, že jeden porušení zabezpečení by mohlo mít vliv jejich celkové zabezpečení cloudu. Azure AD Privileged Identity Management pomáhá tooresolve toto riziko.

Azure AD Privileged Identity Management vám umožní:

* Uvidíte, kteří uživatelé jsou správci Azure AD
* Povolit na vyžádání "právě v čase" tooMicrosoft přístup pro správu Online služeb, jako třeba Office 365 a Intune
* Získání sestavy o historii přístup správce a změny v přiřazení správců
* Dostávat upozornění na přístup tooa privilegované role

Další informace:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Role v Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-roles.md)
* [Azure AD Privileged Identity Management: Jak tooadd nebo odebrat roli uživatele](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Ochrana identit
Azure AD Identity Protection je služba zabezpečení, která poskytuje ucelený přehled o rizikových událostech a potenciální ohrožení zabezpečení, které ovlivňují identity ve vaší organizaci. Ochrany identit využívá možnosti detekce anomálií existující Azure služby Active Directory (k dispozici prostřednictvím neobvyklé aktivity sestav Azure AD) a zavádí nové typy událostí rizik, které můžete zjišťovat anomálie v reálném čase.

Další informace:

* [Ochrany identit Azure Active Directory](../active-directory/active-directory-identityprotection.md)
* [Kanál 9: Azure AD a Identity zobrazení: Identity Protection verze Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Hybridní Správa identit
Společnosti Microsoft přístup tooidentity rozsahy místních a hello cloudu, vytváření identitu jednoho uživatele pro ověřování a autorizaci tooall prostředků, bez ohledu na umístění.

Další informace:

* [Dokument white paper hybridní identity](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Blog týmu Active Directory](https://blogs.technet.microsoft.com/ad/)
