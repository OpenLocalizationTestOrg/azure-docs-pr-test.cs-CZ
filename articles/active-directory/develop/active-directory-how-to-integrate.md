---
title: aaaHow tooIntegrate s Azure Active Directory | Microsoft Docs
description: "Průvodce toobenefits nástroje a prostředky pro integraci s Azure Active Directory."
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: d13bba54-96bd-4b81-bee9-c8025ffa1648
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 4542965ae4a7756eda9f57e9e895f8044892f20e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-with-azure-active-directory"></a>Integrace se službou Azure Active Directory
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory poskytuje organizacím podnikové úrovni Správa identit pro cloudové aplikace.  Integrace se službou Azure AD poskytuje uživatelům moderní prostředí přihlášení a pomáhá vaší aplikace, které odpovídají tooIT zásad.

## <a name="how-toointegrate"></a>Jak tooIntegrate
Existuje několik způsobů pro vaše aplikace toointegrate s Azure AD.  Využijte jako mnoho nebo několika z těchto scénářů, jako je vhodné pro vaši aplikaci.

### <a name="support-azure-ad-as-a-way-toosign-in-tooyour-application"></a>Podpora Azure AD jako způsob tooSign v tooYour aplikace
**Snižte přihlášení třením a snížit náklady na podporu.** Pomocí Azure AD toosign v aplikaci tooyour vaši uživatelé nebudou mít jeden další tooremember jméno a heslo.  Jako vývojář budete mít jeden menší toostore heslo a chránit.  Nemá toohandle zapomenete heslo resetovat, může být důležité ušetříte samostatně.  Azure AD poskytuje přihlášení pro některé hello world nejoblíbenější cloudových aplikací, včetně služeb Office 365 a Microsoft Azure.  S stovky milionů uživatelů z milionů organizací, je možné, se uživatel již přihlášen tooAzure AD.  Další informace o [přidání podpory pro Azure AD přihlášení](active-directory-authentication-scenarios.md).

**Zjednodušení přihlašování nahoru pro vaši aplikaci.**  Během registrace pro vaši aplikaci Azure AD můžete odeslat základní informace o uživateli můžete předem vyplnit vaše přihlašovací formulář nebo zcela eliminuje.  Uživatelé mohou zaregistrovat pro vaši aplikaci pomocí svého účtu Azure AD prostřednictvím toothose podobné prostředí známé souhlasu nalezených v sociálních a mobilních aplikací.  Každý uživatel, můžete zaregistrovat a přihlásit tooan aplikace, která je integrovaná s Azure AD bez nutnosti IT zapojení.  Další informace o [zaregistrovali aplikaci pro účet Azure AD přihlášení](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-tooyour-application"></a>Procházením vyhledejte uživatele, spravovat zřizování uživatelů a řízení přístupu tooYour aplikace
**Procházením vyhledejte uživatele v adresáři hello.**  Použijte hello rozhraní Graph API toohelp uživatelé vyhledávání a procházení pro ostatní uživatelé v organizaci, pokud vyzvání jiných uživatelů nebo k udělení přístupu, místo aby je tootype e-mailových adres.  Uživatelé mohou procházet pomocí známých adres kniha styl rozhraní, včetně zobrazení podrobností hello hello organizační hierarchie.  Další informace o hello [rozhraní Graph API](active-directory-graph-api.md).

**Znovu použijte skupiny služby Active Directory a distribuční seznamy, které už spravuje vašich zákazníků.**  Azure AD obsahuje hello skupiny už pomocí pro distribuci e-mailu a správa přístupu vašich zákazníků.  Pomocí hello rozhraní Graph API, znovu použít tyto skupiny, místo aby toocreate vašeho zákazníka a spravovat samostatnou sadu skupin v aplikaci.  Informace o skupině můžete také odeslat tooyour aplikace v přihlášení tokeny.  Další informace o hello [rozhraní Graph API](active-directory-graph-api.md).

**Toocontrol používání Azure AD, který má přístup tooyour aplikace.**  Správci a vlastníci aplikace ve službě Azure AD můžete přiřadit přístup tooapplications toospecific uživatelů a skupin.  Pomocí hello rozhraní Graph API, můžete číst tento seznam a použít ho toocontrol zřizování a jeho rušení prostředků a přístup v rámci vaší aplikace.

**Řízení přístupu na základě rolí pomocí Azure AD.**  Správci a vlastníci aplikace můžete přiřadit uživatele a skupiny tooroles, který definujete při registraci aplikace ve službě Azure AD.  Informace o rolích je odeslán tooyour aplikace v přihlášení tokeny a můžete také načíst pomocí hello rozhraní Graph API.  Další informace o [pomocí služby Azure AD pro autorizaci](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-toousers-profile-calendar-email-contacts-files-and-more"></a>Profil tooUser přístup GET, kalendář, e-mailu, kontakty, soubory a další
**Azure AD je hello serveru ověřování pro Office 365 a jiné obchodní služby společnosti Microsoft.**  Pokud podporujete Azure AD pro přihlašování tooyour aplikace nebo podporu propojení vaše aktuální uživatelské účty tooAzure AD uživatelské účty pomocí OAuth 2.0, můžete požádat, čtení a přístup pro zápis tooa profilu, kalendář, e-mailu, kontakty, soubory uživatele a další informace.  Můžete bez problémů zapisovat kalendář události toouser a čtení nebo zápisu tootheir soubory OneDrive.  Další informace o [přístup k rozhraní API Office 365 hello](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-hello-azure-and-office-365-marketplaces"></a>Zvýšení úrovně vaše aplikace hello Azure a Office 365 Tržiště
**Umožňuje zvýšit úroveň vaší aplikace toohello miliony organizacemi, které už používají Azure AD.**  Uživatelé, kteří vyhledávání a procházení tyto tržišť už používají jeden nebo více cloudových služeb, což je kvalifikovaný Zákazníci využívající službu cloudu.  Další informace o povýšení svoji aplikaci v [hello Azure Marketplace](https://azure.microsoft.com/marketplace/partner-program/).

**Když se uživatelé přihlásí k aplikaci, zobrazí se v jejich přístupový panel Azure AD a Spouštěč aplikace Office 365.**  Uživatelé budou moct tooquickly a snadno návratový tooyour aplikace později, vylepšení zapojení uživatelů.  Další informace o hello [přístupový panel Azure AD](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Zabezpečené komunikace služby a zařízení a Service-to-Service
**Pomocí služby Azure AD pro správu identit zařízení a služeb snižuje hello kódu potřebujete toowrite a umožňuje IT toomanage přístup.**  Umožňuje získat tokeny z Azure AD pomocí OAuth a použijte tyto tokeny tooaccess webových rozhraní API služby a zařízení.  Pomocí služby Azure AD se můžete vyhnout zápis komplexní ověřovacího kódu.  Vzhledem k tomu, že se ukládají identity hello hello služeb a zařízení ve službě Azure AD, IT může spravovat klíče a odvolání v jednom místě, ne s toodo to samostatně ve vaší aplikace.

## <a name="benefits-of-integration"></a>Výhody integrace
Integrace s Azure AD se dodává s výhody, které nevyžadují toowrite další kód.

### <a name="integration-with-enterprise-identity-management"></a>Integrace s Enterprise Identity Management
**Nápověda aplikace v souladu se zásadami IT.**  Organizace integrovat Azure AD, jejich systémů správy identit enterprise, pokud uživatel opustí organizaci, budou automaticky ztratí přístup tooyour aplikaci bez nutnosti tootake IT dodatečné kroky.  IT můžete spravovat, kdo může získat přístup k vaší aplikaci a určí, jaké zásady přístupu je potřeba – příklad službu Multi-Factor Authentication - snižuje vaše potřeby toowrite kód toocomply komplexní podnikové zásady.  Azure AD poskytuje správcům podrobné auditování protokolu kdo podepsané tak v aplikaci tooyour IT můžete sledovat využití.

**Azure AD rozšiřuje toohello cloudové služby Active Directory tak, aby vaše aplikace může integrovat AD.**  Mnoho organizací kolem hello, world pomocí služby Active Directory jako jejich hlavní přihlášení a systém správy identit a vyžadovat jejich toowork aplikace s AD.  Aplikace integraci s Azure AD integruje se službou Active Directory.

### <a name="advanced-security-features"></a>Funkce Rozšířené zabezpečení
**Služba Multi-Factor authentication.**  Azure AD, poskytuje nativní aplikace Multi-Factor authentication.  Správci IT může vyžadovat služby Multi-Factor authentication tooaccess aplikaci, tak, že nemáte toocode tato podpora sami.  Další informace o [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Neobvyklé přihlašovací při zjišťování.**  Azure AD zpracovává více než miliardu přihlášení denně, při použití machine learning algoritmy toodetect podezřelé aktivity a oznámí správci IT možných problémů.  Díky podpoře přihlášení k Azure AD, aplikace získá hello Výhodou této ochrany. Další informace o [zobrazení sestav Azure Active Directory přístup](../active-directory-view-access-usage-reports.md).

**Podmíněný přístup.**  Kromě toho může vyžadovat správci toomulti Multi-Factor authentication, než uživatelé můžou přihlásit splnit určité podmínky tooyour aplikace.  Podmínky, které lze nastavit zahrnuty rozsah IP adres hello klientských zařízení, členství zadaných skupin a hello stav zařízení hello používá pro přístup.  Další informace o [podmíněného přístupu Azure Active Directory](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Snadný vývoj
**Standardních protokolů.**  Společnost Microsoft se potvrdí toosupporting oborových standardů.  Azure AD podporuje hello SAML 2.0, OpenID Connect 1.0, OAuth 2.0 a WS-Federation 1.2 ověřovací protokoly.  Hello rozhraní Graph API je OData 4.0 kompatibilní.  Pokud už vaše aplikace podporuje hello SAML 2.0 nebo OpenID Connect 1.0 protokoly pro federované přihlašování, může být přehledné přidání podpory pro Azure AD.  Další informace o [podporované protokoly pro ověřování Azure AD](active-directory-authentication-protocols.md).

**Opensourcové knihovny.**  Společnost Microsoft poskytuje plně podporované opensourcové knihovny pro oblíbené platformy a jazyky toospeed vývoj.  Hello zdrojový kód je licencováno v rámci Apache 2.0, a jsou volné toofork a přispívat back toohello projekty.  Další informace o [knihovny ověřování služby Azure AD](active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Po celém světě přítomnosti a vysokou dostupnost
**Azure AD je nasazena v datových centrech kolem hello, world a je spravovat a monitorovat kolem hello hodiny.**  Azure AD je hello systém správy identit pro Microsoft Azure a Office 365 a je nasazena v 28 datových centrech kolem hello, world.  Data adresáře je zaručeno datových centrech tooat minimálně tři toobe replikovat.  Nástroje pro vyrovnávání zatížení globální zajistěte uživatelé přistupovat k hello nejbližší kopii Azure AD obsahující svá data, a automaticky znovu směrovat požadavky tooother datových centrech v případě zjištění problému.

## <a name="next-steps"></a>Další kroky
[Začněte psát kód](active-directory-developers-guide.md#get-started).

[Přihlášení uživatelů pomocí služby Azure AD](active-directory-authentication-scenarios.md)

