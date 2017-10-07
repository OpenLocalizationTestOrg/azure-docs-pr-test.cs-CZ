---
title: aaaAzure Active Directory PoC Playbook implementace | Microsoft Docs
description: "Prozkoumejte a rychle implementovat scénáře identita a správa přístupu"
services: active-directory
keywords: "Azure active directory, scénářem, testování konceptu, PoC"
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: 4d61f1432d5f1c15cd88fda4824cf1c1de64c712
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-implementation"></a>Ověření služby Azure Active Directory koncept Playbook: implementace

## <a name="foundation---syncing-ad-tooazure-ad"></a>Foundation - synchronizuje AD tooAzure AD 

Hybridní identita je hello foundation pro většinu hello podnikoví zákazníci, kteří již mají místního adresáře. Hello cílem je toointentionally jako méně času zde jako hodnota hello tooshow možných scénářů hello skutečné identit a přístupu. 

| Scénář | Stavební bloky| 
| --- | --- |  
| [Rozšíření místní identity toohello cloudu](#extending-your-on-premises-identity-to-the-cloud) | [Synchronizace adresáře – synchronizace hodnot Hash hesel](active-directory-playbook-building-blocks.md#directory-synchronization---password-hash-sync-phs---new-installation) <br/>**Poznámka:**: Pokud už máte DirSync nebo ADSync nebo dřívější verze služby Azure AD Connect, tento krok je volitelný. Některé scénáře v této příručce může vyžadovat novější verze služby Azure AD Connect.  <br/>[Branding](active-directory-playbook-building-blocks.md#branding) | 
| [Přiřazování licencí Azure AD pomocí skupin](#assigning-azure-ad-licenses-using-groups) | [Na základě skupiny licencí](active-directory-playbook-building-blocks.md#group-based-licensing) |


### <a name="extending-your-on-premises-identity-toohello-cloud"></a>Rozšíření místní identity toohello cloudu 

1. Bob se správce Active Directory hello společnosti Contoso. Zadá získá hello požadavek tooenable identity jako služba pro sadu uživatelů. Po spuštění Průvodce Azure AD Connect, hello identitu hello cíloví uživatelé k dispozici v cloudu hello. 
2. Bob zobrazí Susie, jeden z cílových uživatelů hello, tooaccess hello přístupový panel Azure Active Directory a potvrďte, že uživatel provést ověření. Susie uvidí partnerské přihlašovací stránku a prázdný přístupový panel je připravené pro povolení přístupu budoucí aplikace.

### <a name="assigning-azure-ad-licenses-using-groups"></a>Přiřazování licencí Azure AD pomocí skupin 

1. Robert je hello Azure AD globálního správce a chce tooallocate Azure AD licence tooa konkrétní skupinu uživatelů jako součást počáteční zavedení hello Azure AD.
2. Bob vytvoří skupinu pro hello uživatelů pilotního nasazení. 
3. Bob přiřadí hello licence toohello skupiny
4. Susie, jeden z hello informační pracovníci, je přidat skupinu zabezpečení toohello jako součást svůj pracovní funkce
5. Po určité době Susie má přístup toohello Azure AD premium licenci. Tím povolíte více scénářů POC hello později.

## <a name="theme---lots-of-apps-one-identity"></a>Motiv – velký počet aplikací, jednu identitu

| Scénář | Stavební bloky| 
| --- | --- |  
| [Integrace aplikace SaaS - federovaného jednotného přihlašování](#integrate-saas-applications---federated-sso) | [Konfigurace SaaS federovaného jednotného přihlašování](active-directory-playbook-building-blocks.md#saas-federated-sso-configuration) <br/>[Skupin, delegovat vlastnictví](active-directory-playbook-building-blocks.md#groups---delegated-ownership) |
| [Integrace aplikace SaaS – heslo jednotného přihlašování](#integrate-saas-applications---password-sso) | [Konfigurace jednotného přihlašování k SaaS heslo](active-directory-playbook-building-blocks.md#saas-password-sso-configuration) |
| [Jednotné přihlašování a události životního cyklu Identity](#sso-and-identity-lifecycle-events) | [SaaS a životního cyklu Identity](active-directory-playbook-building-blocks.md#saas-and-identity-lifecycle) |
| [Zabezpečené účty pro přístup k tooShared](#secure-access-to-shared-accounts) | [SaaS sdílenou konfiguraci účtů](active-directory-playbook-building-blocks.md#saas-shared-accounts-configuration) |
| [Zabezpečení vzdáleného přístupu tooOn místní aplikace](#secure-remote-access-to-on-premises-applications) | [Konfigurace Proxy aplikace](active-directory-playbook-building-blocks.md#app-proxy-configuration) |
| [Synchronizovat LDAP identity tooAzure AD](#synchronize-ldap-identities-to-azure-ad) |  [Obecná konfigurace konektoru LDAP](active-directory-playbook-building-blocks.md#generic-ldap-connector-configuration) |

### <a name="integrate-saas-applications---federated-sso"></a>Integrace aplikace SaaS - federovaného jednotného přihlašování 

1. Robert je hello Azure AD globálního správce a obdrží žádost od hello marketingové oddělení tooenable přístup tootheir ServiceNow Instance. Bob vyhledá hello podrobný kurz v dokumentaci k Azure AD a následujícího a delegáti hello přiřazení uživatelů toohello aplikace tooKevin, head hello Marketing týmu. 
2. Kevina přihlásí jako vlastník hello ServiceNow oprávnění a přiřadí Susie toohello aplikace. Kevina také oznámení, že profil Susie je automaticky vytvořen v ServiceNow
3. Susie je pracovník s informacemi v oddělení marketingu hello. Jana přihlásí tooazure AD a najde všechny aplikace SaaS Jana je přiřazen tooin myapps portálu. Z tohoto místa se bezproblémově získá tooServiceNow přístup.
4. oddělení marketingu Hello chce tooaudit, kdo měl přístup ServiceNow. Bob stáhne zprávu o činnosti a sdílet je s kevina prostřednictvím e-mailu.  

### <a name="sso-and-identity-lifecycle-events"></a>Jednotné přihlašování a události životního cyklu Identity

1. Susie trvá s absencí a podnikové zásady hello místní účet AD je dočasný zakázáno. Susie nyní nemůže přihlásit tooAzure AD a proto nemůžete ServiceNow. 
2. Susie díky laterální přesunout z marketingu tooSales. Kevina odebere ServiceNow přístup. Susie přihlásí myapps hello azure ad a uživatel se zobrazí už hello ServiceNow dlaždice. Po 10 minutách kevina potvrdí, že Susie účet byl zakázán z konzoly pro správu ServiceNow.

### <a name="integrate-saas-applications---password-sso"></a>Integrace aplikace SaaS – heslo jednotného přihlašování

1. Bob nakonfiguruje přístup tooAtlassian HipChat. HipChat má tooSusie heslo jednotného přihlašování k integraci a udělit přístup
2. Susie přihlásí toohello myapps portál a vidí hello toodownload odkaz rozšíření prohlížeče Azure AD IE, které se soubory ke stažení
3. Po kliknutí na tlačítko, uživatel získá vyzváni k zadání pověření jeho HipChat uživatelské jméno a heslo. Jedná se o jednorázovou operaci, a po jeho dokončení role zabezpečení tooHipChat přístup
4. Několik dní později Susie otevře portál myapps a klikne na tlačítko HipChat znovu. Tento čas přibližně uživatel získá bezproblémový přístup
5. Kevina hello HipChat aplikace vlastníka, chce tooaudit, kdo měl přístup aplikace hello. Bob stáhne zprávu o auditu a sdílet je s kevina prostřednictvím e-mailu. 

### <a name="secure-access-tooshared-accounts"></a>Zabezpečené účty pro přístup k tooShared 

1. Bob se musí toosecure hello sdílené Twitter popisovač pro členy hello prodejní tým. Mu přidá Twitter jako aplikace jednotného přihlašování a přiřadí ji skupinu zabezpečení toohello hello prodejní tým. Dostal hello pověření toohello sdíleného účtu a mu poskytuje v systému hello. 
2. Sdílení přihlašovací údaje služby Twitter je již důvěryhodný kvůli toomultiple osoby vědomí. Bob umožňuje automatickou výměnu hello Twitter hesla.
3. Susie, členem hello prodejní tým, zaprotokoluje toohello myapps portálu a vidí hello toodownload odkaz rozšíření prohlížeče Azure AD IE. Jana nainstaluje.
4. Po kliknutí na tlačítko Jana get přímo přistupovat ke tooTwitter. Jana neví hello heslo.
5. Arnold je také součástí hello prodejního týmu. Má hello stejné prostředí jako Susie v krocích 3 4
6. oddělení prodeje Hello chce tooaudit, kdo měl přístup Twitter. Bob stáhne zprávu o činnosti a sdílet je s kevina prostřednictvím e-mailu. 

### <a name="secure-remote-access-tooon-premises-applications"></a>Zabezpečení vzdáleného přístupu tooOn místní aplikace

1. Bob hello Azure AD globálního správce, obdržel množství požadavků tooenable zaměstnanci tooaccess užitečné několik místních prostředků, jako je například aplikace hello výdaje, při práci vzdáleně. Zadá následuje hello [Proxy aplikace dokumentaci](active-directory-application-proxy-enable.md) tooinstall konektor a publikovat výdaje jako aplikaci Proxy aplikace. 
2. Bob sdílet hello externí adresu URL aplikace výdaje s Susie, jeden hello zaměstnanců, kteří musí vzdáleného přístupu. Uživatel přistupuje k hello odkaz, a po ověření proti AAD, která je možné tooaccess hello výdaje aplikace a pokračovat toobe produktivitu při vzdálené. 
3. Bob pak bude pokračovat, že toopublish dalších místních aplikací s použitím hello stejný proces a udělení přístupu toousers podle potřeby. Přidá podmíněného přístupu a vícefaktorového ověřování pro hello více citlivých aplikací, které mu publikuje, tooensure další zabezpečení.

### <a name="synchronize-ldap-identities-tooazure-ad"></a>Synchronizovat LDAP identity tooAzure AD

1. Bob společnost má infrastruktury komplexní identit. Většina uživatelů hello udržované uvnitř Windows Server Active Directory Domain Services (přidá). Některé z nich, jsou spravovány systémem HR uvnitř Active Directory Lightweight Directory Services (ADLDS).
2. Bob za úkol povolení přístupu tooSaaS aplikací pro všechny uživatele (taky tyto není k dispozici v přidá).
3. Bob nakonfiguruje obecné konektor LDAP toopull data z ADLDS v Azure AD Connect.
4. Bob vytvoří synchronizační pravidla, takže uživatelé LDAP se importují do úložiště Metaverse a tooAzure AD
5. Synchronizované identity Susie se, že LDAP uživatel přistupuje k jeho pomocí aplikace SaaS



> [!IMPORTANT] 
> Toto je pokročilou konfiguraci nutnosti některé znalost FIM nebo MIM. Pokud se používá v produkčním prostředí, doporučujeme, aby dotazy týkající se tato konfigurace projít [Premier Support](https://support.microsoft.com/premier).



## <a name="theme---increase-your-security"></a>Motiv – zvýšení zabezpečení 

| Scénář | Stavební bloky| 
| --- | --- |  
| [Přístup k účtu správce zabezpečení](#secure-administrator-account-access) | [Azure MFA s telefonní hovory](active-directory-playbook-building-blocks.md#azure-multi-factor-authentication-with-phone-calls) |
| [Zabezpečit přístup k aplikacím](#secure-access-to-applications) | [Podmíněný přístup pro aplikace SaaS](active-directory-playbook-building-blocks.md#mfa-conditional-access-for-saas-applications) |
| [Povolit jenom v době správy](#enable-just-in-time-jit-administration) | [Privileged Identity Management](active-directory-playbook-building-blocks.md#privileged-identity-management-pim) |
| [Ochrana identit podle rizik](#protect-identities-based-on-risk) | [Zjišťování rizikových událostí](active-directory-playbook-building-blocks.md#discovering-risk-events) <br/>[Nasazení zásad riziko přihlášení](active-directory-playbook-building-blocks.md#deploying-sign-in-risk-policies) |
| [Ověřit bez hesla pomocí ověřování na základě certifikátů](#authenticate-without-passwords-using-certificate-based-authentication) | [Konfigurace ověřování na základě certifikátů](active-directory-playbook-building-blocks.md#configuring-certificate-based-authentication)

### <a name="secure-administrator-account-access"></a>Přístup k účtu správce zabezpečení

1. Robert je hello Azure AD globálního správce. Zadá označila Stanislav jako spolusprávce služby hello. 
2. Bob nakonfiguruje Stanislav na účet, který tooalways vyžadují postavení zabezpečení hello tooimprove vícefaktorového ověřování
3. Stanislav přihlásí toohello portál Azure a oznámení, že se mu musí tooregister jeho telefonní číslo toocontinue hello přihlášení
4. Další přihlášení z Stanislav jsou teď chráněné pomocí služby Multi-Factor Authentication, a mohl nyní získá telefonní hovor tooverify svou totožnost.

### <a name="secure-access-tooapplications"></a>Tooapplications zabezpečený přístup

1. Kevina je hello firmy vlastník ServiceNow. Společnost Hello nyní chce toologin těchto uživatelů pomocí vícefaktorového ověřování při přístupu k podnikové síti mimo hello.
2. Přidá podmíněného přístupu zásad toohello ServiceNow aplikace tooenable vícefaktorového ověřování pro přístup mimo Bob naše globální správce Azure AD
3. Susie naše pracovník přihlásí Moje aplikace portálu a kliknutím na dlaždici ServiceNow hello. Uživatel je nyní postiženy pomocí vícefaktorového ověřování.

### <a name="enable-just-in-time-jit-administration"></a>Povolit pouze v části Správa času (JIT)

1. Bob a Stanislav jsou globální správce Azure AD. Chtějí tooenable JIT přístup toohello správu rolí a také tookeep záznamů na hello využití hello privilegované role.
2. Bob umožňuje PIM v klientovi Azure AD hello a že bude hello správce zabezpečení. Změní sám a členství v roli globálního správce pro Stanislava z trvalé tooeligible.
3. Bob a Stanislav teď vyžadují aktivaci jejich role prostřednictvím portálu Azure hello před některého tooAzure AD změny konfigurace. 

### <a name="protect-identities-based-on-risk"></a>Ochrana identit podle rizik 

1. Susie, pokusy pracovník s informacemi o přihlášení z prohlížeče tor. 
2. Bob ověří hello řídicího panelu ochrany identit Azure AD a zobrazí Susie na přihlášení z anonymních IP adresu. Hello zabezpečení tým chce, aby toochallenge takové přistupuje k uživatele pomocí vícefaktorového ověřování
3. Bob umožňuje zásady služby Azure AD Identity Protection toochallenge MFA pro střední a vyšší riziko události
4. Čas přejde a Susie přihlásí z prohlížeče Tor znovu. Této doby, uživatel uvidí hello ověřovací test MFA

### <a name="authenticate-without-passwords-using-certificate-based-authentication"></a>Ověřit bez hesla pomocí ověřování na základě certifikátů

1. Robert je globální správce pro finanční instituce, která zakazuje použití hesel jako faktor ověřování pro své aplikace.
2. Bob umožňuje a vynucuje ověřování certifikátů na služby AD FS a Azure AD
3. Susie přístup k aplikaci při zobrazení výzvy tooauthenticate pomocí certifikátu

## <a name="theme---scale-with-self-service"></a>Motiv – škálování s Self Service

| Scénář | Stavební bloky| 
| --- | --- |  
| [Samoobslužné resetování hesla](#self-service-password-reset) | [Samoobslužné resetování hesla](active-directory-playbook-building-blocks.md#self-service-password-reset) |
| [Vlastní tooApplications přístup k službě](#self-service-access-to-applications) | [Vlastní tooApplications přístup k službě](active-directory-playbook-building-blocks.md#self-service-access-to-application-management) |

### <a name="self-service-password-reset"></a>Samoobslužné resetování hesla 

1. Bob se hello Azure AD globálního správce a umožňuje správu hesel samoobslužné služby tooa podmnožinu uživatelů, včetně Susie. 
2. Susie protokoly na portálu toomyapps najdete tooregister zpráva jeho informace o zabezpečení pro budoucí heslo resetovat události.
3. Rychlé dál několik dní Susie zapomene své heslo a obnoví prostřednictvím portálu Azure AD

### <a name="self-service-access-tooapplications"></a>Vlastní tooApplications přístup k službě 

1. Kevina je hello firmy vlastník ServiceNow. Chce uživatele příliš "zaregistrujte si" jej na vyžádání, místo přidávání je všechny najednou
2. Bob, naše globální správce Azure AD, upraví hello ServiceNow aplikace tooenable žádosti o samoobslužné služby
3. Susie naše pracovník přihlásí Moje aplikace portálu a klikne na tlačítko "Přidat další aplikace" hello a najdete v části ServiceNow jako jeden z hello doporučené aplikace. Jana poté přejde zpět toomy aplikace portálu a vidět hello ServiceNow aplikaci.

[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]