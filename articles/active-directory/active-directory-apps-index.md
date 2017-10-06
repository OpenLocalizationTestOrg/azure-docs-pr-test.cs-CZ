---
title: "aaaArticle Index pro správu aplikací v Azure Active Directory | Microsoft Azure"
description: "Zjistěte, jak datum vypršení platnosti hello toocustomize federačních certifikátů a jak toorenew certifikáty, které brzy vyprší."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 5321b8e4-2afa-4dfe-8d53-4add7abb5ec8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: f8a584baa94dc50e279899074f50160978256559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="article-index-for-application-management-in-azure-active-directory"></a>Rejstřík článků o správě aplikací ve službě Azure Active Directory
Tato stránka obsahuje úplný seznam každému dokumentu napsané o hello různé funkce týkající se aplikace v Azure Active Directory (Azure AD).

Je stručný úvod tooeach hlavní funkce oblasti, jakož i pokyny k které tooread články v závislosti na tom, jaké informace, které hledáte.

## <a name="overview-articles"></a>Přehled článků
Hello článků níže jsou dobré počáteční body pro ty, kteří pouze stručné vysvětlení funkce správy aplikací Azure AD.

| Článek Průvodce |  |
|:---:| --- |
| Problémy Úvod toohello aplikací správy, které řeší Azure AD |[Správa aplikací pomocí služby Azure Active Directory (AD)](active-directory-enable-sso-scenario.md) |
| Přehled hello různých funkcí ve službě Azure AD související tooenabling jednotné přihlašování, definovat, kdo má přístup k tooapps a jak uživatelé spustí aplikace |[Přístup k aplikaci a jednotné přihlašování v Azure Active Directory](active-directory-appssoaccess-whatis.md) |
| Podívejte se na hello různé kroky při integraci aplikace do služby Azure AD |[Integrace s aplikacemi Azure Active Directory](active-directory-integrating-applications-getting-started.md)<br /><br />[Povolení jednotného přihlašování tooSaaS aplikace](active-directory-sso-integrate-saas-apps.md)<br /><br />[Správa přístupu tooApps](active-directory-managing-access-to-apps.md) |
| Technické vysvětlení, jak jsou reprezentována aplikací ve službě Azure AD |[Jak a proč se aplikace přidávají tooAzure AD](active-directory-how-applications-are-added.md) |

## <a name="troubleshooting-articles"></a>Řešení potíží s články
Tato část obsahuje řešení problémů s příručky toorelevant rychlý přístup. Další informace o jednotlivých oblast funkcí naleznete na hello zbývající části této stránky.

| Oblast funkcí |  |
|:---:| --- |
| Federované jednotné přihlašování |[Řešení potíží s na základě SAML jednotné přihlašování](active-directory-saml-debugging.md) |
| Založené na heslech jednotné přihlašování |[Řešení potíží s hello rozšíření panely přístup pro prohlížeč Internet Explorer](active-directory-saas-ie-troubleshooting.md) |
| Aplikační proxy server |[Průvodce odstraňováním potíží Proxy aplikace](active-directory-application-proxy-troubleshoot.md) |
| Jednotné přihlašování mezi místní AD a Azure AD |[Řešení potíží s synchronizace hesel](connect/active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization)<br /><br />[Řešení potíží s zpětný zápis hesla](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Dynamické členství ve skupinách |[Řešení potíží s dynamické členství ve skupinách](active-directory-accessmanagement-troubleshooting.md) |

## <a name="single-sign-on-sso"></a>Jednotné přihlašování
### <a name="federated-single-sign-on-sign-into-many-apps-using-one-identity"></a>Federované jednotné přihlašování: Přihlaste se k mnoho aplikací pomocí jednu identitu
Jednotné přihlašování umožňuje uživatelům tooaccess celou řadu aplikací a služeb pomocí jen jednu sadu přihlašovacích údajů. Federace se nachází jedna metoda, pomocí kterého můžete povolit jednotné přihlašování. Pokud se uživatelé pokusí o toosign do federovaným aplikacím, získají přesměrovaného tootheir oficiální přihlašovací stránku organizace vykreslen tak, že Azure Active Directory a jsou pak přesměrované back toohello aplikace po úspěšném ověření.

| Článek Průvodce |  |
|:---:| --- |
| Toofederation úvod a dalších typů přihlášení |[Jednotné přihlašování s Azure AD](active-directory-appssoaccess-whatis.md) |
| Tisíce aplikacemi SaaS, které jsou předem integrované se službou Azure AD s zjednodušené kroky konfigurace přihlášení |[Začínáme s hello galerii aplikací Azure AD](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)<br /><br />[Úplný seznam předem integrovaných aplikací, které podporují Federation](http://aka.ms/aadfederatedapps)<br /><br />[Jak tooAdd aplikace toohello Galerie aplikací Azure AD](active-directory-app-gallery-listing.md) |
| Víc než 150 aplikace kurzy o tom, jak tooconfigure jednotného přihlašování pro aplikace, jako například [Salesforce](active-directory-saas-salesforce-tutorial.md), [ServiceNow](active-directory-saas-servicenow-tutorial.md), [Google Apps](active-directory-saas-google-apps-tutorial.md), [Workday](active-directory-saas-workday-tutorial.md)a mnoho dalších |[Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory](active-directory-saas-tutorial-list.md) |
| Jak toomanually nastavit a přizpůsobit konfiguraci jednoho přihlášení |[Jak tooConfigure tooApps federované jednotné přihlašování, nejsou v hello Azure Active Directory Application Gallery](active-directory-saas-custom-apps.md)<br /><br />[Jak tooCustomize deklarace identity vystavené v hello tokenu SAML pro Pre-Integrated aplikace](active-directory-saml-claims-customization.md) |
| Průvodce řešením potíží pro federované aplikace, které používají protokol SAML hello |[Řešení potíží s na základě SAML jednotné přihlašování](active-directory-saml-debugging.md) |
| Jak tooconfigure datum vypršení platnosti certifikátu vaší aplikace a jak toorenew certifikáty |[Správa certifikátů pro federované jednotné přihlašování v Azure Active Directory](active-directory-sso-certs.md) |

Federované jednotné přihlašování je k dispozici pro všechny edice Azure AD pro až tooten aplikace na uživatele. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) podporuje neomezená aplikace. Pokud má vaše organizace [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) nebo [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), pak můžete [pomocí skupin aplikací toofederated přístup tooassign](#managing-access-to-applications).

### <a name="password-based-single-sign-on-account-sharing-and-sso-for-non-federated-apps"></a>Založené na heslech jednotné přihlašování: účet sdílení a jednotné přihlašování pro aplikace, nefederovaných
tooenable jeden přihlašování tooapplications, nepodporují federation, Azure AD nabízí heslo správu funkce, které můžete bezpečně uložit hesla tooSaaS aplikace a automaticky přihlášení uživatelé do těchto aplikací. Snadno můžete distribuovat pověření pro nově vytvořené účty a sdílení účtů tým s více uživateli. Uživatelé nepotřebují nutně tooknow hello pověření toohello účty, které získá přístup k.

| Článek Průvodce |  |
|:---:| --- |
| Stručný technický přehled a úvod toohow založené na heslech jednotné přihlašování funguje |[Založené na heslech jednotné přihlašování s Azure AD](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) |
| Souhrn hello scénáře související tooaccount sdílení a jak jsou tyto problémy vyřešit pomocí Azure AD |[Sdílení účtů s Azure AD](active-directory-sharing-accounts.md) |
| Změna hesla hello automaticky pro určité aplikace v pravidelných intervalech |[Automatizované výměny heslo (preview)](https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Nasazení a řešení potíží s příručky pro verzi aplikace Internet Explorer hello hello rozšíření správy hesel Azure AD |[Jak tooDeploy hello rozšíření přístup k panelu pro Internet Explorer pomocí zásad skupiny](active-directory-saas-ie-group-policy.md)<br /><br />[Řešení potíží s hello rozšíření panely přístup pro prohlížeč Internet Explorer](active-directory-saas-ie-troubleshooting.md) |

Založené na heslech jednotné přihlašování je k dispozici pro všechny edice Azure AD pro až tooten aplikace na uživatele. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) podporuje neomezená aplikace. Pokud má vaše organizace [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) nebo [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), pak můžete [použití skupin tooassign přístup tooapplications](#managing-access-to-applications). Výměna automatizované heslo je [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funkce.

### <a name="app-proxy-single-sign-on-and-remote-access-tooon-premises-applications"></a>Proxy aplikace: Jeden přihlašování a vzdálený přístup tooon místní aplikace
Pokud máte aplikace v privátní síti, potřebujete toobe přístup uživatelů a zařízení mimo hello síť, můžete použít Azure AD Application Proxy tooenable zabezpečený vzdálený přístup toothose aplikace.

| Článek Průvodce |  |
|:---:| --- |
| Přehled proxy aplikace služby Azure AD a jak to funguje |[Poskytování zabezpečeného vzdáleného přístupu tooon místní aplikace](active-directory-application-proxy-get-started.md) |
| Kurzů tooconfigure Proxy aplikace a jak toopublish první aplikace |[Jak tooSet až Proxy aplikace Azure AD](active-directory-application-proxy-enable.md)<br /><br />[Jak nainstalovat tooSilently hello konektoru Proxy aplikace](active-directory-application-proxy-silent-installation.md)<br /><br />[Jak tooPublish aplikací pomocí Proxy aplikace](active-directory-application-proxy-publish.md)<br /><br />[Jak tooUse vlastní název domény](active-directory-application-proxy-custom-domains.md) |
| Jak tooenable jednotné přihlašování a podmíněného přístupu pro aplikace publikované s Proxy aplikace |[Jednotné přihlášení pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)<br /><br />[Podmíněný přístup a Proxy aplikací](active-directory-application-proxy-conditional-access.md) |
| Pokyny pro následující scénáře hello toouse Proxy aplikace |[Jak tooSupport nativní klientské aplikace](active-directory-application-proxy-native-client.md)<br /><br />[Jak tooSupport deklaracemi identity aplikace](active-directory-application-proxy-claims-aware-apps.md)<br /><br />[Jak publikovat aplikace tooSupport na oddělené sítě a umístění](active-directory-application-proxy-connectors.md) |
| Průvodce řešením potíží pro Proxy aplikace |[Průvodce odstraňováním potíží Proxy aplikace](active-directory-application-proxy-troubleshoot.md) |

Proxy aplikace je k dispozici pro všechny edice Azure AD pro až tooten aplikace na uživatele. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) podporuje neomezená aplikace. Pokud má vaše organizace [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) nebo [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), pak můžete [použití skupin tooassign přístup tooapplications](#managing-access-to-applications).

Také může být zájem o [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md), což vám umožní toomigrate vaše místní aplikace tooAzure při stále neodpovídajících hello identity musí těchto aplikací.

### <a name="enabling-single-sign-on-between-azure-ad-and-on-premises-ad"></a>Povolení jednotného přihlašování mezi službou Azure AD a místní AD
Pokud vaše organizace udržuje Windows Server Active Directory místní společně s Azure Active Directory v cloudu hello, pak budete pravděpodobně chtít tooenable jednotné přihlašování mezi těmito dvěma systémy. Azure AD Connect (hello nástroj, který se integruje se tyto dvě systémy společně) poskytuje několik možností nastavení jednotného přihlašování: navázání federaci se službou AD FS nebo jiného zprostředkovatele federace, nebo povolení synchronizace hesel.

| Článek Průvodce |  |
|:---:| --- |
| Přehled o hello jeden přihlašování možnostech nabízena v Azure AD Connect a také informace o správě hybridní prostředí |[Přihlášení uživatele na možnosti v Azure AD Connect](active-directory-aadconnect-user-signin.md) |
| Obecné pokyny pro správu prostředí s oběma místní služby Active Directory a Azure Active Directory |[Aspekty návrhu Azure AD hybridní Identity](active-directory-hybrid-identity-design-considerations-overview.md)<br /><br />[Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md) |
| Pokyny k pomocí synchronizace hesla tooenable jednotného přihlašování |[Implementace synchronizace hesel s Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md)<br /><br />[Řešení potíží s synchronizace hesel](https://support.microsoft.com/en-us/kb/2855271) |
| Pokyny k použití zpětný zápis hesla tooenable jednotného přihlašování |[Začínáme se správou hesel ve službě Azure AD](active-directory-passwords-getting-started.md)<br /><br />[Řešení potíží se zpětným zápisem hesla](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Pokyny k použití tooenable zprostředkovatelé identity jiných výrobců jednotné přihlašování |[Seznam kompatibilní třetích stran identitu poskytovatelů, je možné použít tooEnable jednotné přihlašování](https://aka.ms/ssoproviders) |
| Jak uživatelé Windows 10 využívat hello výhody jednotné přihlašování přes Azure AD Join |[Rozšíření možností cloudu tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-overview.md) |

Azure AD Connect je k dispozici pro [všechny edice služby Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). Azure AD funkce samoobslužného resetování hesla je k dispozici pro [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) a [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Zpětný zápis hesla tooon místní AD [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funkce.

### <a name="conditional-access-enforce-additional-security-requirements-for-high-risk-apps"></a>Podmíněného přístupu: Dodatečné požadavky na zabezpečení pro aplikace s vysokým rizikem pro vynucení
Jakmile nastavíte tooyour přihlašování aplikacím a prostředkům, můžete pak další zabezpečit citlivých aplikací vynucením specifické požadavky na zabezpečení na každou aplikaci toothat přihlášení. Například můžete použít Azure AD toodemand všechny konkrétní aplikace access tooa vždy vyžadují ověřování Multi-Factor authentication, bez ohledu na to, jestli tuto aplikaci podporuje innately této funkce. Další běžné příklad podmíněného přístupu je, že toorequire, že uživatelé budou připojené toohello organizace důvěryhodné sítě v pořadí tooaccess zvláště citlivé z hlediska aplikace.

| Článek Průvodce |  |
|:---:| --- |
| Nabízí možnosti podmíněného přístupu Úvod toohello přes Azure AD, Office 365 a Intune |[Řízení rizik pomocí podmíněného přístupu](active-directory-conditional-access.md) |
| Jak tooenable podmíněný přístup pro následující typy prostředků hello |[Podmíněný přístup pro aplikace SaaS](active-directory-conditional-access-azuread-connected-apps.md)<br /><br />[Podmíněný přístup pro služby Office 365](active-directory-conditional-access-device-policies.md)<br /><br />[Podmíněný přístup pro místní aplikace](active-directory-conditional-access.md)<br /><br />[Podmíněný přístup pro místní aplikace publikované prostřednictvím Proxy aplikace Azure AD](active-directory-application-proxy-conditional-access.md) |

| Jak tooregister zařízení s Azure Active Directory v pořadí tooenable zásady podmíněného přístupu na základě zařízení | [Přehled registrace zařízení služby Azure Active Directory](active-directory-conditional-access-device-registration-overview.md)<br /><br />[Jak tooEnable automatické registrace zařízení pro zařízení se systémem Windows připojených k doméně](active-directory-conditional-access-automatic-device-registration.md)<br />– [Kroky pro Windows 8.1 zařízení](active-directory-conditional-access-automatic-device-registration-setup.md)<br />– [Zařízení kroky pro systém Windows 7](active-directory-conditional-access-automatic-device-registration-setup.md) |

| Jak toouse hello aplikaci Microsoft Authenticator pro dvoustupňové ověření | [Microsoft Authenticator](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) |

Podmíněný přístup [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funkce.

## <a name="apps--azure-ad"></a>Aplikace a služby Azure AD
### <a name="cloud-app-discovery-find-which-saas-apps-are-being-used-in-your-organization"></a>Cloud App Discovery: Najít, které aplikace SaaS jsou používány ve vaší organizaci
Cloud App Discovery pomáhá IT oddělení informace, které aplikace SaaS jsou používány v rámci organizace hello. Můžete ho měření využití aplikací a oblíbenosti, které můžete určit, které aplikace bude mít prospěch hello naplno využít se převedené pod řízení IT a probíhá integrované s Azure AD.

| Článek Průvodce |  |
|:---:| --- |
| Obecný přehled o tom, jak funguje |[Vyhledání aplikace neschválená cloudových aplikací s Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md) |
| Podrobnější informace o tom, jak funguje, s tooquestions odpovědi na ochranu osobních údajů |[Zabezpečení a důležité informace o ochraně osobních údajů](active-directory-cloudappdiscovery-security-and-privacy-considerations.md) |
| Nejčastější dotazy |[Nejčastější dotazy ke Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx) |
| Podrobné pokyny pro nasazení Cloud App Discovery |[Příručka pro nasazení zásad skupiny](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)<br /><br />[Průvodci nasazením aplikace System Center](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)<br /><br />[Instalace na Proxy serverech s vlastní porty](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md) |
| Hello změnit protokolu pro agenta Cloud App Discovery toohello aktualizace |[Protokol změn](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx) |

Cloud App Discovery je [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funkce.

### <a name="automatically-provision-and-deprovision-user-accounts-in-saas-apps"></a>Automaticky zřizovat a zrušit jejich zřízení uživatelských účtů ve SaaS aplikace
Automatizovat hello vytváření, údržbu a odebírání uživatelských identit v aplikace SaaS, jako je Dropbox, Salesforce, ServiceNow a další. Odpovídající a synchronizovat existující identit mezi službou Azure AD a aplikace SaaS a řízení přístupu automatickým zakázáním účtů, když někteří uživatelé opustí organizaci hello.

| Článek Průvodce |  |
|:---:| --- |
| Další informace o tom, jak funguje a najděte odpovědi toocommon otázky |[Automatizace zřizování uživatelů a jeho rušení tooSaaS aplikace](active-directory-saas-app-provisioning.md) |
| Nakonfigurujte, jak je namapovaný informace mezi službou Azure AD a aplikace SaaS |[Přizpůsobení mapování atributů](active-directory-saas-customizing-attribute-mappings.md)<br><br>[Zapisují se výrazy pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md) |
| Jak tooenable automatické zřizování tooany aplikace, který podporuje protokol SCIM hello |[Nastavit automatické zřizování uživatelů tooany SCIM-Enabled aplikace](active-directory-scim-provisioning.md) |
| Jak tooreport na a řešení potíží s zřizování uživatelů |[Zprávy o zřizování automatické uživatelů](active-directory-saas-provisioning-reporting.md)<br><br>[Zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)<br><br>[Řešení potíží s zřizování uživatelů](active-directory-application-provisioning-content-map.md) |
| Omezení, kdo získá zřízené tooan aplikaci na základě jejich hodnot atributů |[Filtry oborů](active-directory-saas-scoping-filters.md) |

Zřizování automatizované uživatelů je k dispozici pro všechny edice Azure AD pro až tooten aplikace na uživatele. [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) podporuje neomezená aplikace. Pokud má vaše organizace [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) nebo [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), pak můžete [použít toomanage skupiny uživatelů, kteří získat zřízený](#managing-access-to-applications).

### <a name="building-applications-that-integrate-with-azure-ad"></a>Vytváření aplikací, které se integrují s Azure AD
Pokud vaše organizace je vývoj nebo udržování-obchodní aplikace (LoB), nebo pokud jste vývojář aplikace se zákazníky, kteří používají Azure Active Directory, hello následující kurzy vám pomůže integrace aplikací s Azure AD.

| Článek Průvodce |  |
|:---:| --- |
| Pokyny pro odborníky v oblasti IT a vývojáři aplikace k integraci aplikací s Azure AD |[Hello IT specialista je Průvodce pro vývoj aplikací pro Azure AD](active-directory-applications-guiding-developers-for-lob-applications.md)<br /><br />[Příručka pro vývojáře Hello pro Azure Active Directory](active-directory-developers-guide.md) |
| Jak přidat tooapplication dodavatelé jejich aplikace toohello Galerie aplikací Azure AD |[Výpis svoji aplikaci v Azure Active Directory Application Gallery hello](active-directory-app-gallery-listing.md) |
| Jak toomanage přístup toodeveloped aplikací pomocí služby Azure Active Directory |[Jak tooEnable přiřazení uživatelů pro aplikace vyvinuté](active-directory-applications-guiding-developers-requiring-user-assignment.md)<br /><br />[Přiřazení uživatelů tooyour aplikace](active-directory-applications-guiding-developers-assigning-users.md)<br /><br />[Přiřazení skupiny tooyour aplikace](active-directory-applications-guiding-developers-assigning-groups.md) |

Pokud vyvíjíte aplikace určených, může zajímat pomocí [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) tak, že nemáte toodevelop toomanage systému vlastní identitu uživatele. [Další informace](../active-directory-b2c/active-directory-b2c-overview.md).

## <a name="managing-access-tooapplications"></a>Správa přístupu tooApplications
### <a name="using-groups-and-self-service-toomanage-who-has-access-toowhich-apps"></a>Pomocí skupin a toomanage samoobslužné služby, který má přístup toowhich aplikace
toohelp spravujete kteří mají mít přístup k prostředkům toowhich, Azure Active Directory umožňuje tooset přiřazení oprávnění ve velkém měřítku pomocí skupin. IT může zvolit tooenable samoobslužných funkcí tak, aby uživatelé můžete jednoduše požadovat oprávnění, když ji potřebují.

| Článek Průvodce |  |
|:---:| --- |
| Přehled funkcí řízení přístupu Azure AD |[Úvod tooManaging tooApps přístup](active-directory-managing-access-to-apps.md)<br /><br />[Jak funguje správa přístupu ve službě Azure AD](active-directory-manage-groups.md)<br /><br />[Jak tooUse skupiny tooManage tooSaaS přístupu aplikace](active-directory-accessmanagement-group-saasapps.md) |
| Povolení samoobslužné správy skupin a aplikací |[Správa aplikací samoobslužné služby](active-directory-self-service-application-access.md)<br /><br />[Samoobslužná správa skupin](active-directory-accessmanagement-self-service-group-management.md) |
| Pokyny k nastavení skupin ve službě Azure AD |[Jak tooCreate skupin zabezpečení](active-directory-accessmanagement-manage-groups.md)<br /><br />[Jak tooDesignate vlastníků pro skupinu](active-directory-accessmanagement-managing-group-owners.md)<br /><br />[Jak tooUse hello "všechny" skupinu uživatelů](active-directory-accessmanagement-dedicated-groups.md) |
| Použití dynamických skupin tooautomatically naplňte členství skupiny pomocí pravidla členství na základě atributů |[Členství ve skupině dynamické: Rozšířená pravidla](active-directory-accessmanagement-groups-with-advanced-rules.md)<br /><br />[Řešení potíží s dynamické členství ve skupinách](active-directory-accessmanagement-troubleshooting.md) |

Správa přístupu na základě skupin aplikací je k dispozici pro [Azure AD Basic](https://azure.microsoft.com/pricing/details/active-directory/) a [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Samoobslužná správa skupin, Správa aplikací samoobslužné služby a dynamických skupin se [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funkce.

### <a name="b2b-collaboration-enable-partner-access-tooapplications"></a>Spolupráce B2B: Povolení tooapplications přístupu partnera
Pokud váš podnik má ve spolupráci s jinými společnostmi, je pravděpodobné, že je potřeba toomanage partnera přístup tooyour podnikovým aplikacím. Azure Active Directory spolupráce B2B ve poskytuje tooshare snadný a bezpečný způsob, jak vaše aplikace s partnery.

| Článek Průvodce |  |
|:---:| --- |
| Přehled různých Azure AD funkce, že můžete snadněji tak můžete spravovat externích uživatelů, jako partnerům, zákazníky, atd. |[Porovnání funkcí pro správu externí identity ve službě Azure AD](active-directory-b2b-compare-external-identities.md) |
| Úvod tooB2B spolupráce a způsob spuštění tooget |[Jednoduchý, zabezpečení, cloudu partnera integraci s Azure AD](active-directory-b2b-what-is-azure-ad-b2b.md)<br /><br />[Spolupráce B2B Azure Active Directory](active-directory-b2b-collaboration-overview.md) |
| O podrobnější prohlídku do spolupráce Azure AD B2B a jak toouse ho |[Spolupráce B2B: Jak to funguje](active-directory-b2b-how-it-works.md)<br /><br />[Aktuální omezení spolupráce Azure AD B2B](active-directory-b2b-current-limitations.md)<br /><br />[Podrobný návod k používání spolupráce Azure AD B2B](active-directory-b2b-detailed-walkthrough.md) |
| Odkaz na články s technické informace o tom, jak funguje spolupráce Azure AD B2B |[Formát souboru CSV pro přidávání uživatelů partnera](active-directory-b2b-references-csv-file-format.md)<br /><br />[Uživatelské atributy vliv spolupráce Azure AD B2B](active-directory-b2b-references-external-user-object-attribute-changes.md)<br /><br />[Formát tokenu uživatele pro uživatele partnera](active-directory-b2b-references-external-user-token-format.md) |

Spolupráce B2B je aktuálně dostupné pro [všechny edice služby Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="access-panel-a-portal-for-accessing-apps-and-self-service-features"></a>Přístupový Panel: Portál pro přístup k aplikacím a samoobslužných funkcí
Hello přístupový Panel Azure AD je, kde koncoví uživatelé můžete spustit své aplikace a přístup hello samoobslužné funkce, které mu umožní toomanage jejich aplikace a členství ve skupinách. Kromě toho toohello přístupový Panel další možnosti pro přístup k aplikací podporujících jednotné přihlašování jsou součástí hello seznamu dole.

| Článek Průvodce |  |
|:---:| --- |
| Porovnání hello různé možnosti k dispozici pro nasazení aplikace přihlašování toousers |[Nasazení tooUsers integrovaných aplikací Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) |
| Přehled hello přístupového panelu a jeho mobilních ekvivalentní MyApps |[Úvod tooAccess panelu a MyApps](active-directory-saas-access-panel-introduction.md)<br />– [iOS](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8)<br />– [Android](https://play.google.com/store/apps/details?id=com.microsoft.myapps) |
| Jak hello tooaccess Azure AD aplikace z webu Office 365 |[Pomocí hello Spouštěč aplikace Office 365](https://support.office.com/en-us/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a) |
| Jak aplikace tooaccess Azure AD z hello mobilní aplikace Intune Managed Browser |[Spravovaný prohlížeč Intune](https://technet.microsoft.com/en-us/library/dn878029.aspx)<br />– [iOS](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8)<br />– [Android](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser) |
| Jak aplikace tooaccess Azure AD pomocí přímých odkazů tooinitiate jednotného přihlašování |[Získávání aplikací tooYour přímé odkazy přihlášení](active-directory-appssoaccess-whatis.md#direct-sign-on-links-for-federated-password-based-or-existing-apps) |

Přístupový Panel je k dispozici pro [všechny edice služby Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="reports-easily-audit-app-access-changes-and-monitor-sign-ins-tooapps"></a>Sestavy: Snadno auditovat přístup změny aplikace a sledovat tooapps přihlášení
Azure Active Directory nabízí několik sestav a upozorní toohelp monitorování tooapplications přístup vaší organizace. Může přijímat výstrahy pro neobvyklých přihlášení tooyour aplikace a můžete sledovat, kdy a proč došlo ke změně aplikace tooan přístup uživatelé.

| Článek Průvodce |  |
|:---:| --- |
| Přehled hello funkce v Azure Active Directory generování sestav |[Začínáme s vytvářením sestav Azure AD](active-directory-reporting-getting-started.md) |
| Jak toomonitor hello přihlášení a používání aplikace uživatelů |[Zobrazit sestavy o využití a přístupu](active-directory-view-access-usage-reports.md) |
| Sledovat změny provedené toowho můžete přístup k určité aplikaci |[Události sestavy auditování Azure Active Directory](active-directory-reporting-audit-events.md) |
| Export dat hello tyto nástroje tooyour upřednostňovaný sestavy pomocí hello Reporting rozhraní API |[Začínáme s hello rozhraní API pro vytváření sestav Azure AD](active-directory-reporting-api-getting-started.md) |

se dodává s různými edicemi služby Azure Active Directory, které sestavami toosee [kliknutím sem](active-directory-view-access-usage-reports.md).

## <a name="see-also"></a>Viz také
[Představení služby Azure Active Directory](active-directory-whatis.md)

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/)

[Azure Active Directory Domain Services](https://azure.microsoft.com/services/active-directory-ds/)

[Azure Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/)
