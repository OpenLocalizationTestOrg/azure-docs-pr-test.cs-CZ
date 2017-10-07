---
title: "aaaAzure doklad ze stavebních bloků playbook koncept služby Active Directory | Microsoft Docs"
description: "Prozkoumejte a rychle implementovat scénáře identita a správa přístupu"
services: active-directory
keywords: "Azure active directory, scénářem, testování konceptu, PoC"
documentationcenter: 
author: dstefanMSFT
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: dstefan
ms.openlocfilehash: e54148330a123baf27d7e0f73469ff2a24c0efcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-building-blocks"></a>Azure Active Directory doklad o koncept playbook: stavební bloky

## <a name="catalog-of-roles"></a>Katalog rolí

| Role | Popis | Testování konceptu (PoC) zodpovědnosti |
| --- | --- | --- |
| **Architektura identity / vývojový tým** | Tento tým je obvykle hello jednu, která navrhuje řešení hello, implementuje prototypy, jednotky schválení a nakonec předá toooperations | Poskytují hello prostředí a jsou ty, které jsou hello vyhodnocování hello různé scénáře z hlediska správy hello |
| **Místní Identity provozní tým** | Spravuje hello jinou identitu zdroje místní: doménové struktury služby Active Directory, adresářů protokolu LDAP, HR systémy a poskytovatelů federace identit. | Zadejte přístup tooon místní prostředky potřebné pro hello PoC scénáře.<br/>Jejich by měl být zahrnut co nejméně|
| **Technické vlastníci aplikace** | Technické vlastníky hello různých cloudových aplikací a služeb, které se integrují s Azure AD | Obsahují podrobnosti o SaaS aplikace (potenciálně instance pro testování) |
| **Globální správce Azure AD** | Spravuje konfiguraci hello Azure AD | Zadejte přihlašovací údaje tooconfigure hello synchronizační služba. Obvykle hello stejné team jako architektura Identity během testování koncepce ale oddělte během fáze operations hello|
| **Databáze team** | Vlastníci hello databáze infrastruktury | Poskytuje přístup tooSQL prostředí (služby AD FS nebo Azure AD Connect) pro konkrétní scénář přípravy.<br/>Jejich by měl být zahrnut co nejméně |
| **Tým síťových** | Vlastníci hello síťové infrastruktury | Zadejte požadovaný přístup na úrovni sítě hello hello synchronizace tooproperly servery, přístup ke zdrojům dat hello a cloudových služeb (pravidla brány firewall, porty otevřít, pravidla IPSec atd.) |
| **Bezpečnostní tým** | Definuje hello bezpečnostní strategie, analyzuje zabezpečení sestavy z různých zdrojů a postupuje podle zjištění. | Zadejte cíl zabezpečení vyhodnocení scénáře |

## <a name="common-prerequisites-for-all-building-blocks"></a>Běžné požadavky pro všechny stavební bloky

Toto jsou některé součásti potřebné pro všechny POC s Azure AD Premium.

| Předpoklad | Zdroje |
| --- | --- |
| Klientovi Azure AD, které jsou definované s platným předplatným Azure | [Jak Azure Active Directory tooget klienta](active-directory-howto-tenant.md)<br/>**Poznámka:** Pokud již máte v prostředí s licencí Azure AD Premium, můžete získat nulové cap předplatné přechodem toohttps://aka.ms/accessaad <br/>Další informace na: https://blogs.technet.microsoft.com/enterprisemobility/2016/02/26/azure-ad-mailbag-azure-subscriptions-and-azure-ad-2/ a https://technet.microsoft.com/library/dn832618.aspx |
| Definované a ověření domény | [Přidat tooAzure název vlastní domény služby Active Directory](active-directory-domains-add-azure-portal.md)<br/>**Poznámka:** některé úlohy, jako je Power BI může mít zřízení klient služby azure AD v části hello obsahuje. toocheck-li pro danou doménu přidružené tooa tenanta, přejděte toohttps://login.microsoftonline.com/ {domain}/v2.0/.well-known/openid-configuration. Je-li získat k úspěšné odpovědi, pak hello domény je již přiřazen tooa klienta a převzít kontrolu nad mohou být potřebné. Pokud ano, požádejte Microsoft o další pokyny. Další informace o možnostech převzetí hello v: [co je Samoobslužná registrace pro Azure?](active-directory-self-service-signup.md) |
| Azure AD Premium nebo EMS zkušební povoleno | [Azure Active Directory Premium zdarma pro jeden měsíc](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Přiřadili jste licence Azure AD Premium nebo EMS tooPoC uživatelů | [Licence vy a vaši uživatelé v Azure Active Directory](active-directory-licensing-get-started-azure-portal.md) |
| Přihlašovací údaje Azure AD globálního správce. | [Přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md) |
| Volitelné, ale důrazně ho doporučujeme: paralelní testovacího prostředí jako zálohu | [Požadavky pro Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |

## <a name="directory-synchronization---password-hash-sync-phs---new-installation"></a>Instalace nové synchronizace - synchronizace hodnot Hash hesel (PBS) - Directory

Přibližná doba tooComplete: jednu hodinu, než menší než 1 000 uživatelů PoC

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Server tooRun Azure AD Connect | [Požadavky pro Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |
| Cíloví uživatelé POC v hello stejné doméně a součástí skupiny zabezpečení a organizační jednotky | [Vlastní instalace služby Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) |
| Azure AD Connect funkce, která je potřebná pro hello jsou identifikovány POC | [Připojení služby Active Directory s Azure Active Directory – konfigurace synchronizace funkce](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Požadovaná pověření pro místní a cloudové prostředí  | [Azure AD Connect: Účty a oprávnění](./connect/active-directory-aadconnect-accounts-permissions.md) |

### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Stažení nejnovější verze hello služby Azure AD Connect | [Stáhněte si Microsoft Azure Active Directory Connect](https://www.microsoft.com/download/details.aspx?id=47594) |
| Nainstalujte Azure AD Connect s nejjednodušší cesta hello: Express <br/>1. Filtrovat toohello cílový organizační jednotky toominimize hello synchronizačním cyklu čas<br/>2. Vyberte cílovou sadu uživatelů v místní skupině hello.<br/>3. Nasazení funkce hello potřeby nástrojem hello jiné POC motivy | [Azure AD Connect: Vlastní instalace: domény a organizační jednotky filtrování](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) <br/>[Azure AD Connect: Vlastní instalace: filtrování na základě skupiny](./connect/active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups)<br/>[Azure AD Connect: Integrování místních identit s Azure Active Directory: Konfigurace funkcí synchronizace](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Otevřete hello Azure AD Connect uživatelského rozhraní a zjistíte, že hello systémem profily dokončené (Import, synchronizaci a export) | [Synchronizace Azure AD Connect: Plánovač](./connect/active-directory-aadconnectsync-feature-scheduler.md) |
| Otevřete hello [portálu pro správu Azure AD](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/), přejděte toohello "Všichni uživatelé" okno, přidat sloupec "Zdroj autority" a zjistit, který se zobrazí uživatelům hello, správně označené jako pocházející od "Windows Server AD" | [Portál pro správu Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) |

### <a name="considerations"></a>Požadavky

1. Podívejte se na hello zabezpečení aspektů synchronizace hodnot hash hesel [zde](./connect/active-directory-aadconnectsync-implement-password-synchronization.md).  Pokud synchronizací hodnoty hash hesla pro uživatele pilotního nasazení produkční není platností možnost, pak zvažte následující alternativy hello:
   * Vytvoření testovacích uživatelů v doméně hello produkčním prostředí. Ujistěte se, že nemáte synchronizovat jiný účet
   * Přesunutí tooan UAT prostředí
2.  Pokud chcete, federační toopursue, je vhodné toounderstand hello náklady spojené s místní zprostředkovatele Identity nad rámec hello POC federované řešení a hledáte míře, které proti hello výhody můžete:
    * Je v cestě hello důležité, abyste získali toodesign pro zajištění vysoké dostupnosti
    * Je k místní službě potřebujete toocapacity plán
    * Je k místní službě potřebujete toomonitor nebo udržovat nebo oprava

Další informace: [identity Principy Office 365 a Azure Active Directory - federované Identity](https://support.office.com/article/Understanding-Office-365-identity-and-Azure-Active-Directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9#bk_federated)

## <a name="branding"></a>Branding

Přibližná doba tooComplete: 15 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Prostředky (bitové kopie, loga, atd.); Pro nejlepší vizualizaci Ujistěte se, zda text hello prostředky mít hello doporučené velikosti. | [Přidání firemního brandingu tooyour přihlašovací stránky v hello Azure Active Directory](active-directory-branding-custom-signon-azure-portal.md) |
| Volitelné: Pokud hello prostředí má server služby AD FS, přístup k toohello serveru toocustomize webový motiv | [AD FS uživatele přihlásit přizpůsobení](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-user-sign-in-customization) |
| Klientský počítač tooperform koncového uživatele přihlašování |  |
| Volitelné: Mobilní zařízení toovalidate prostředí |  |

### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Přejděte tooAzure AD portálu pro správu | [Portál pro správu Azure AD - firemního brandingu](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/LoginTenantBranding) |
| Nahrajte hello prostředků pro přihlašovací stránku hello (nejdůležitější logo, malé logo, popisky, atd.). Případně pokud máte služby AD FS, Zarovnat hello stejné prostředky s přihlašovací stránky služby AD FS | [Přidání firemního brandingu tooyour přihlásit a přístupového panelu: přizpůsobitelné prvky](active-directory-add-company-branding.md) |
| Počkejte několik minut, než se změna hello toofully vstoupí v platnost |  |
| Přihlaste se hello toohttps://myapps.microsoft.com přihlašovacích údajů uživatele POC |  |
| Potvrďte hello vzhled a chování v prohlížeči | [Přidání firemního brandingu tooyour přihlásit a přístupového panelu](active-directory-add-company-branding.md) |
| Volitelně můžete potvrďte hello vzhled a chování v jiných zařízení |  |

### <a name="considerations"></a>Požadavky

Pokud hello staré vzhled a chování zůstane po přizpůsobení hello vyprázdnit mezipaměť klienta prohlížeče hello a opakujte operaci hello.

## <a name="group-based-licensing"></a>Na základě skupiny licencí

Přibližná doba tooComplete: 10 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Všichni uživatelé POC jsou součástí skupiny zabezpečení (cloudové nebo místní) | [Vytvořte skupinu a přidejte členy v Azure Active Directory](active-directory-groups-create-azure-portal.md) |

### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Okno toolicenses přejděte na portálu správy Azure AD | [Portál pro správu Azure AD: Správa licencí](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) |
| Přiřazení skupiny zabezpečení toohello hello licence s POC uživateli. | [Přiřaďte licence tooa skupinu uživatelů ve službě Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md) |

### <a name="considerations"></a>Požadavky

V případě jakékoli problémy, přejděte příliš[scénáře, omezení a známé problémy s používáním skupin toomanage licencování v Azure Active Directory](active-directory-licensing-group-advanced.md)

## <a name="saas-federated-sso-configuration"></a>Konfigurace SaaS federovaného jednotného přihlašování

Přibližná doba tooComplete: 60 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Testovací prostředí hello aplikace SaaS, které jsou k dispozici. V této příručce použijeme ServiceNow jako příklad.<br/>Důrazně doporučujeme toouse testovací instance toominimize tření na navigace existující data kvality a mapování. | Přejděte toohttps://developer.servicenow.com/app.do#! / home toostart hello proces získávání testovací instance |
| Konzola pro správu přístupu toohello ServiceNow správu | [Kurz: Azure Active Directory integrace s ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Cíl nastaví aplikace hello tooassign uživatele. Doporučuje se skupinu zabezpečení obsahující uživatele PoC hello. <br/>Pokud vytváření skupiny hello není vhodná, pak přiřazení uživatelů hello toodirectly toohello aplikace pro hello PoC | [Přiřaďte aplikaci enterprise uživatele nebo skupinu tooan v Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |

### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Sdílet hello kurz tooall aktéři z Documentation společnosti Microsoft  | [Kurz: Azure Active Directory integrace s ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Nastavit pracovní schůzku a postupujte podle kroků kurzu hello s každou objektu actor. | [Kurz: Azure Active Directory integrace s ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Přiřaďte hello aplikace toohello skupinu určené v hello požadavky. Pokud hello POC podmíněného přístupu v oboru hello, můžete později, pokroku a přidat vícefaktorové ověřování a podobné. <br/>Všimněte si, že to bude nové hello proces zajišťování (Pokud je nakonfigurováno) |  [Přiřaďte aplikaci enterprise uživatele nebo skupinu tooan v Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) <br/>[Vytvořte skupinu a přidejte členy v Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Použít Azure AD management Portal tooadd ServiceNow aplikaci z Galerie| [Správa služby Azure AD portál: podnikové aplikace](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/Overview) <br/>[Co je nového v nástroji Správa podniková aplikace v Azure Active Directory](active-directory-enterprise-apps-whats-new-azure-portal.md) |
| V okně "Jednotného přihlašování" aplikace ServiceNow povolte "na základě SAML přihlašování" |  |
| Vyplňte pole "Přihlašovací adresa URL" a "Identifikátor" s ServiceNow adresu URL<br/>Zaškrtněte políčko hello příliš "aktivujte nový certifikát"<br/>a uložte nastavení |  |
| Otevřete okno "Konfigurace ServiceNow" na hello dolní části hello panel tooview přizpůsobit pokyny pro vás tooconfigure ServiceNow |  |
| Postupujte podle pokynů tooconfigure ServiceNow |  |
| V okně "Zřizování" aplikace ServiceNow povolte zřizování "Automatické" | [Správa uživatelský účet zřizování pro podnikové aplikace hello novou verzi portálu Azure](active-directory-enterprise-apps-manage-provisioning.md) |
| Zatímco bude zřizování dokončeno, počkejte několik minut.  V hello té doby můžete zkontrolovat na hello zřizování sestavy |  |
| Přihlaste se jako testovacího uživatele, který má přístup toohttps://myapps.microsoft.com/ | [Co je hello přístupový Panel?](active-directory-saas-access-panel-introduction.md) |
| Klikněte na dlaždici hello hello aplikace, kterou jste právě vytvořili. Potvrďte volbu přístup |  |
| Volitelně můžete zkontrolovat sestavy využití aplikace hello. Všimněte si, že se některé latenci, takže potřebujete toowait určitý čas toosee hello provoz v sestavách hello. | [Přihlašovací aktivity sestav na portálu Azure Active Directory hello: využití spravovaných aplikací](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Zásady uchovávání sestav služby Azure Active Directory](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Požadavky

1. Výše [kurzu](active-directory-saas-servicenow-tutorial.md) odkazuje prostředí pro správu tooold Azure AD. Ale PoC je založena na [úvodní](active-directory-enterprise-apps-whats-new-azure-portal.md#quick-start-get-going-with-your-new-application-right-away) prostředí.
2. Pokud cílová aplikace hello se nenachází v galerii hello, můžete použít, "Přineste vlastní aplikace". Další informace: [co je nového v nástroji Správa podniková aplikace v Azure Active Directory: Přidat vlastní aplikace z jednoho místa](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

## <a name="saas-password-sso-configuration"></a>Konfigurace jednotného přihlašování k SaaS heslo

Přibližná doba tooComplete: 15 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Testovací prostředí pro aplikace SaaS. Příklad přihlašování heslo je HipChat a Twitter. Pro všechny ostatní aplikace budete potřebovat hello přesnou adresu URL stránky hello s přihlášení formuláře html. | [Twitter na webu Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[HipChat na webu Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.hipchat) |
| Testovací účty pro aplikace hello. | [Zaregistrujte si služby Twitter.](https://twitter.com/signup?lang=en)<br/>[Zaregistrujte se zdarma: HipChat](https://www.hipchat.com/sign_up) |
| Cíl nastaví aplikace hello tooassign uživatele. Doporučuje se uživatel hello obsažené skupiny zabezpečení. | [Přiřaďte aplikaci enterprise uživatele nebo skupinu tooan v Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Místní správce přístup tooa počítače toodeploy hello rozšíření přístup k panelu pro Internet Explorer, Chrome nebo Firefox | [Rozšíření přístup k panelu pro aplikaci Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Rozšíření přístup k panelu pro Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Rozšíření přístup k panelu pro Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Nainstalujte rozšíření prohlížeče hello | [Rozšíření přístup k panelu pro aplikaci Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Rozšíření přístup k panelu pro Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Rozšíření přístup k panelu pro Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Nakonfigurovat aplikaci z Galerie | [Co je nového v nástroji Správa podniková aplikace v Azure Active Directory: Galerie nových a vylepšených aplikací hello](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Konfigurace hesla jednotného přihlašování | [Správa jednotného přihlašování pro podnikové aplikace na portálu Azure nové hello: založené na heslech přihlašování](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Přiřadit hello aplikace toohello skupinu určené v hello požadavky | [Přiřaďte aplikaci enterprise uživatele nebo skupinu tooan v Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Přihlaste se jako testovacího uživatele, který má přístup toohttps://myapps.microsoft.com/ |  |
| Klikněte na dlaždici hello hello aplikace, kterou jste právě vytvořili. | [Co je hello přístupový Panel?: založené na heslech jednotné přihlašování bez zřizování identity](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Zadejte přihlašovací údaje hello aplikací | [Co je hello přístupový Panel?: založené na heslech jednotné přihlašování bez zřizování identity](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Zavřete prohlížeč hello a opakovaném hello přihlášení. Nyní hello uživatel by měl vidět bezproblémový přístup toohello aplikace. |  |
| Volitelně můžete zkontrolovat sestavy využití aplikace hello. Všimněte si, že se některé latenci, takže potřebujete toowait určitý čas toosee hello provoz v sestavách hello. | [Přihlašovací aktivity sestav na portálu Azure Active Directory hello: využití spravovaných aplikací](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Zásady uchovávání sestav služby Azure Active Directory](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Požadavky

Pokud cílová aplikace hello se nenachází v galerii hello, můžete použít, "Přineste vlastní aplikace". Další informace: [co je nového v nástroji Správa podniková aplikace v Azure Active Directory: Přidat vlastní aplikace z jednoho místa](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Mějte na paměti hello následující požadavky:
   * Aplikace by měl mít známých přihlašovací adresa URL
   * Hello přihlašovací stránka by měla obsahovat formuláře HTML s jeden další textová pole, které rozšíření prohlížeče hello můžete automatické vyplnění. V hello minimální měl by obsahovat uživatelské jméno a heslo.

## <a name="saas-shared-accounts-configuration"></a>SaaS sdílenou konfiguraci účtů

Přibližná doba tooComplete: 30 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Hello seznamu cíl aplikací a hello přesný přihlašovací adresy URL předem. Jako příklad můžete Twitter. | [Twitter na webu Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[Zaregistrujte si služby Twitter.](https://twitter.com/signup?lang=en) |
| Sdílené přihlašovací údaje pro tuto aplikaci SaaS. | [Sdílení účtů pomocí služby Azure AD](active-directory-sharing-accounts.md)<br/>[Heslo převrácení pro Facebook, Twitter a LinkedIn nyní ve verzi preview služby azure AD automatizované! -Enterprise Mobility and Security Blog] (https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Přihlašovací údaje pro alespoň dva členy týmu, kteří budou mít přístup hello stejný účet. Musí být součástí skupiny zabezpečení. | [Přiřaďte aplikaci enterprise uživatele nebo skupinu tooan v Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Místní správce přístup tooa počítače toodeploy hello rozšíření přístup k panelu pro Internet Explorer, Chrome nebo Firefox | [Rozšíření přístup k panelu pro aplikaci Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Rozšíření přístup k panelu pro Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Rozšíření přístup k panelu pro Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Nainstalujte rozšíření prohlížeče hello | [Rozšíření přístup k panelu pro aplikaci Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Rozšíření přístup k panelu pro Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Rozšíření přístup k panelu pro Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Nakonfigurovat aplikaci z Galerie | [Co je nového v nástroji Správa podniková aplikace v Azure Active Directory: Galerie nových a vylepšených aplikací hello](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Konfigurace hesla jednotného přihlašování | [Správa jednotného přihlašování pro podnikové aplikace na portálu Azure nové hello: založené na heslech přihlašování](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Přiřadit hello aplikace toohello skupinu určené v hello požadavky při přiřazování jejich přihlašovací údaje | [Přiřaďte aplikaci enterprise uživatele nebo skupinu tooan v Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Přihlaste se jako různým uživatelům přístup k aplikaci jako hello **stejné sdílené účtu.**  |  |
| Volitelně můžete zkontrolovat sestavy využití aplikace hello. Všimněte si, že se některé latenci, takže potřebujete toowait určitý čas toosee hello provoz v sestavách hello. | [Přihlašovací aktivity sestav na portálu Azure Active Directory hello: využití spravovaných aplikací](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Zásady uchovávání sestav služby Azure Active Directory](active-directory-reporting-retention.md) |


### <a name="considerations"></a>Požadavky

Pokud cílová aplikace hello se nenachází v galerii hello, můžete použít, "Přineste vlastní aplikace". Další informace: [co je nového v nástroji Správa podniková aplikace v Azure Active Directory: Přidat vlastní aplikace z jednoho místa](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Mějte na paměti hello následující požadavky:
   * Aplikace by měl mít známých přihlašovací adresa URL
   * Hello přihlašovací stránka by měla obsahovat formuláře HTML s jeden další textová pole, které rozšíření prohlížeče hello můžete automatické vyplnění. V hello minimální měl by obsahovat uživatelské jméno a heslo.

## <a name="app-proxy-configuration"></a>Konfigurace Proxy aplikace

Přibližná doba tooComplete: 20 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Microsoft Azure AD basic nebo předplatné premium a adresář Azure AD, u kterého jste globální správce | [Edice služby Azure Active Directory](active-directory-editions.md) |
| Webové aplikace hostované místní, které chcete tooconfigure pro vzdálený přístup |  |
| Serveru se systémem Windows Server 2012 R2 nebo Windows 8.1 nebo vyšší, na který nainstalujete konektor Proxy aplikace hello | [Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md) |
| Pokud v cestě hello je brána firewall, ujistěte se, že je otevřené, že hello, které má konektor HTTPS (TCP) požaduje toohello Proxy aplikace | [Povolení Proxy aplikace v portálu Azure hello: požadavky na Proxy aplikace](active-directory-application-proxy-enable.md#application-proxy-prerequisites) |
| Pokud vaše organizace používá proxy servery tooconnect toohello Internetu, proveďte, podívejte se na hello blog post práci se stávající místní servery proxy pro podrobnosti o tom, tooconfigure je | [Práce s existující místní proxy servery](application-proxy-working-with-proxy-servers.md) |


### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Nainstalujte konektor na hello server | [Povolení Proxy aplikace v portálu Azure hello: Nainstalujte a zaregistrujte hello konektoru](active-directory-application-proxy-enable.md#install-and-register-a-connector) |
| Publikování aplikace hello místní ve službě Azure AD jako aplikace Proxy aplikace | [Publikování aplikací pomocí proxy aplikace služby Azure AD](application-proxy-publish-azure-portal.md) |
| Přiřadit testovací uživatele | [Publikování aplikací pomocí proxy aplikace služby Azure AD: Přidání testovacího uživatele](application-proxy-publish-azure-portal.md#add-a-test-user) |
| Volitelně můžete nakonfigurujte jeden přihlašování pro vaše uživatele | [Zadejte jednotné přihlašování s proxy aplikace služby Azure AD](application-proxy-sso-azure-portal.md) |
| Testování aplikace pomocí podepisování tooMyApps portálu jako přiřazený uživatel | https://myapps.microsoft.com |

### <a name="considerations"></a>Požadavky

1. Při doporučujeme uvedení hello konektor v podnikové síti, existují případy, když se zobrazí jeho umístění v cloudu hello lepší výkon. Další informace: [aspekty topologie sítě, při použití aplikace Proxy Azure Active Directory](application-proxy-network-topology-considerations.md)
2. Další podrobné informace o zabezpečení a jak to zejména zabezpečený vzdálený přístup poskytuje řešení udržováním pouze odchozí připojení najdete v tématu: [důležité informace o zabezpečení pro vzdálený přístup k aplikací pomocí proxy aplikace služby Azure AD](application-proxy-security-considerations.md)

## <a name="generic-ldap-connector-configuration"></a>Obecná konfigurace konektoru LDAP

Přibližná doba tooComplete: 60 minut

> [!IMPORTANT]
> Toto je pokročilou konfiguraci nutnosti některé znalost FIM nebo MIM. Pokud se používá v produkčním prostředí, doporučujeme, aby dotazy týkající se tato konfigurace projít [Premier Support](https://support.microsoft.com/premier).

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Instalace a konfigurace Azure AD Connect | Stavební blok: [synchronizace adresáře – synchronizace hodnot Hash hesel](#directory-synchronization--password-hash-sync-phs--new-installation) |
| Požadavky na schůzku ADLDS instance | [Technické informace o obecné konektor LDAP: Přehled hello obecné konektor LDAP](./connect/active-directory-aadconnectsync-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| Seznam úloh, které uživatelé používají a atributy přidružené tyto úlohy | [Synchronizace Azure AD Connect: atributy synchronizované tooAzure služby Active Directory](./connect/active-directory-aadconnectsync-attributes-synchronized.md) |


### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Přidat generický konektor LDAP | [Technické informace o obecné konektor LDAP: vytvořit nový konektor](./connect/active-directory-aadconnectsync-connector-genericldap.md#create-a-new-connector) |
| Vytvoření profilů spuštění pro vytvořený konektor (úplný import, Rozdílový import, úplná synchronizace, rozdílová synchronizace, export) | [Vytvořit profil spuštění agenta správy](https://technet.microsoft.com/library/jj590219(v=ws.10).aspx)<br/> [Pomocí konektorů hello Správce synchronizace služby Azure AD Connect](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md)|
| Spustit úplný import profilu a ověřte, jestli jsou objekty v prostoru konektoru | [Vyhledejte objekt prostoru konektoru](https://technet.microsoft.com/library/jj590287(v=ws.10).aspx)<br/>[Pomocí konektorů hello Správce synchronizace služby Azure AD Connect: hledání prostoru konektoru](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md#search-connector-space) |
| Vytvoření pravidla synchronizace, tak, aby objektů v Metaverse nezbytné atributy pro úlohy | [Synchronizace Azure AD Connect: osvědčené postupy pro změnu výchozí konfigurace hello: změny tooSynchronization pravidla](./connect/active-directory-aadconnectsync-best-practices-changing-default-configuration.md#changes-to-synchronization-rules)<br/>[Synchronizace Azure AD Connect: Principy deklarativní zřizování](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning.md)<br/>[Synchronizace Azure AD Connect: Principy výrazů deklarativní zřizování](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| Spustit úplnou synchronizaci cyklu | [Synchronizace Azure AD Connect: Scheduler: spuštění hello plánovače](./connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler) |
| V případě problémů se řešení potíží | [Řešení potíží s objekt, který není synchronizace tooAzure AD](./connect/active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) |
| Ověřte, že LDAP uživatel může přihlásit a přístup k aplikace hello | https://myapps.microsoft.com |

### <a name="considerations"></a>Požadavky

> [!IMPORTANT]
> Toto je pokročilou konfiguraci nutnosti některé znalost FIM nebo MIM. Pokud se používá v produkčním prostředí, doporučujeme, aby dotazy týkající se tato konfigurace projít [Premier Support](https://support.microsoft.com/premier).

## <a name="groups---delegated-ownership"></a>Skupin, delegovat vlastnictví

Přibližná doba tooComplete: 10 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Již nakonfigurována aplikace SaaS (Federovanému nebo heslo jednotné přihlašování) | Stavební blok: [Konfigurace jednotného přihlašování federovaného SaaS](#saas-federated-sso-configuration) |
| Skupina cloudu, který je přiřazen přístup toohello aplikace v #1 je určena | Stavební blok: [Konfigurace jednotného přihlašování federovaného SaaS](#saas-federated-sso-configuration) <br/>[Vytvořte skupinu a přidejte členy v Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Přihlašovací údaje pro vlastník skupiny hello jsou k dispozici | [Spravovat přístup tooresources pomocí skupin Azure Active Directory](active-directory-manage-groups.md) |
| Byla zjištěna přihlašovací údaje pro hello informace pracovní přístupem hello aplikace | [Co je hello přístupový Panel?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Identifikovat skupinu hello, kterému byla udělena přístup toohello aplikace a konfigurace hello vlastníkem daného skupiny| [Spravovat nastavení hello pro skupinu v Azure Active Directory](active-directory-groups-settings-azure-portal.md) |
| Přihlaste se jako vlastník skupiny hello najdete v tématu členství ve skupině hello skupiny kartě přístupového panelu | [Stránky Azure Active Directory, Správa skupin](https://account.activedirectory.windowsazure.com/r/#/groups) |
| Přidat hello pracovník chcete tootest |  |
| Přihlaste se jako hello pracovník, potvrďte, že je k dispozici hello dlaždice | [Co je hello přístupový Panel?](active-directory-saas-access-panel-introduction.md) |

### <a name="considerations"></a>Požadavky

Pokud má aplikace hello zřizování povoleno, bude pravděpodobně nutné toowait pár minut pro zřizování toocomplete před přístupem k aplikaci hello jako hello pracovník hello.

## <a name="saas-and-identity-lifecycle"></a>SaaS a životního cyklu Identity

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Již nakonfigurována aplikace SaaS (Federovanému nebo heslo jednotné přihlašování) | Stavební blok: [Konfigurace jednotného přihlašování federovaného SaaS](#saas-federated-sso-configuration) |
| Skupina cloudu, který je přiřazen přístup toohello aplikace v #1 je určena | Stavební blok: [Konfigurace jednotného přihlašování federovaného SaaS](#saas-federated-sso-configuration) <br/>[Vytvořte skupinu a přidejte členy v Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Byla zjištěna přihlašovací údaje pro hello informace pracovní přístupem hello aplikace | [Co je hello přístupový Panel?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Odebrat hello uživatel z hello skupiny hello aplikace je příliš přiřazen.| [Správa členství ve skupině pro uživatele v klientovi služby Azure Active Directory](active-directory-groups-members-azure-portal.md) |
| Počkejte několik minut, než jeho rušení | [Automatické zřizování uživatelů aplikace SaaS ve službě Azure AD: Jak funguje automatické zřizování?](active-directory-saas-app-provisioning.md#how-does-automated-provisioning-work) |
| Na relace samostatné prohlížeče Přihlaste se jako hello informačního pracovníka toomy aplikace portálu a potvrďte, že dlaždice chybí | http://myapps.microsoft.com |


### <a name="considerations"></a>Požadavky

Odvodit hello PoC scénář tooleavers nebo absencí scénáře. Pokud uživatel hello získá zakázán v místní službě AD nebo odebrán, již není toolog způsob, jak v toohello aplikaci SaaS.

## <a name="self-service-access-tooapplication-management"></a>Vlastní tooApplication přístup k službě správy

Přibližná doba tooComplete: 10 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Identifikace uživatelů POC, které bude požadovat přístup k aplikacím toohello, v rámci skupiny zabezpečení hello | Stavební blok: [Konfigurace jednotného přihlašování federovaného SaaS](#saas-federated-sso-configuration) |
| Cílová aplikace nasazené | Stavební blok: [Konfigurace jednotného přihlašování federovaného SaaS](#saas-federated-sso-configuration) |

### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Přejděte tooEnterprise okna aplikace na portálu správy Azure AD | [Portál pro správu Azure AD: Podnikové aplikace, které](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) |
| Nakonfigurovat aplikaci z požadavky se samoobslužné služby | [Co je nového v nástroji Správa podniková aplikace v Azure Active Directory: Konfigurace přístupu k aplikaci Samoobslužné služby](active-directory-enterprise-apps-whats-new-azure-portal.md#configure-self-service-application-access) |
| Přihlaste se jako hello informačního pracovníka toomy aplikace portálu | http://myapps.microsoft.com |
| Všimněte si, "+ Přidat aplikaci" tlačítko na op hello stránky. Použití ho tooget přístup toohello aplikace |  |

### <a name="considerations"></a>Požadavky

Vybraná aplikace Hello používají zřizování požadavky, takže okamžitě budete toohello aplikace může způsobit chyby. Pokud aplikace hello vybrali podporuje zřizování s azure ad a je nakonfigurován, může použít jako možnost tooshow hello celou tok práce end tooend. V tématu hello stavební blok pro [Konfigurace jednotného přihlašování federovaného SaaS](#saas-federated-sso-configuration) pro další doporučení

## <a name="self-service-password-reset"></a>Samoobslužné resetování hesla

Přibližná doba tooComplete: 15 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Povolte správu hesla pomocí samoobslužné služby ve vašem klientovi. | [Azure Active Directory resetování hesla pro správce IT](active-directory-passwords.md) |
| Povolte použití hesla toomanage zpětný zápis hesel z místní. Poznámka: to vyžaduje konkrétní Azure AD Connect verze | [Požadavky pro zpětný zápis hesla](active-directory-passwords-writeback.md) |
| Identifikujte hello PoC uživatele, kteří tuto funkci používat a ujistěte se, že jsou členy skupiny zabezpečení. musí být Hello uživatelé bez oprávnění správce toofully představením hello schopností | [Přizpůsobení: Správa hesel Azure AD: resetování toopassword omezit přístup](active-directory-passwords-writeback.md) |


### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Přejděte tooAzure AD portálu pro správu: resetování hesla | [Portálu pro správu Azure AD: Resetování hesla](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) |
| Určí, že zásady resetování hesel hello. Pro účely testování Koncepce můžete použít telefonní hovor a otázek a odpovědí. Je doporučeno toobe tooenable registrace vyžaduje v protokolu panelu tooaccess |  |
| Odhlaste se a přihlaste se jako pracovník s informacemi |  |
| Zadat data funkce samoobslužného resetování hesla hello podle konfigurace za krok 2 | http://aka.MS/ssprsetup |
| Prohlížeč zavřít hello |  |
| Začít od začátku procesu přihlášení hello jako hello pracovník, které jste použili v kroku 4 |  |
| Resetování hesla hello | [Aktualizujte své heslo: resetování hesla](active-directory-passwords-update-your-own-password.md) |
| Zkuste se přihlásit vaše nové heslo tooAzure AD, jakož i tooon místní prostředky |  |

### <a name="considerations"></a>Požadavky

1. Pokud toocause třecí upgrade hello Azure AD Connect, zvažte použití proti cloudové účty ani ji nastavit jako ukázku proti samostatné prostředí
2. Hello Správci mají jiné zásady a pomocí Dobrý den, Správce může heslo účtu tooreset hello nezanechávajícího skvrny hello PoC a způsobit nejasnostem. Ujistěte se, že používáte operace resetování tootest hello běžného uživatelského účtu


## <a name="azure-multi-factor-authentication-with-phone-calls"></a>Azure Multi-Factor Authentication s telefonní hovory

Přibližná doba tooComplete: 10 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Identifikovat POC uživatele, kteří budou používat vícefaktorového ověřování  |  |
| Phone s funkčním příjem pro ověřovací test MFA  | [Co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) |

### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Přejděte příliš "Uživatelé a skupiny" okno na portálu správy Azure AD | [Správy portálu Azure AD: Uživatelé a skupiny](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Overview/menuId/) |
| Zvolte okno "Všichni uživatelé" |  |
| V panelu zvolte tlačítko "Vícefaktorového ověřování" hello nahoře | Přímá adresa URL pro portál Azure MFA: https://aka.ms/mfaportal |
| V nastavení "User" hello vyberte hello PoC uživatelů a povolení vícefaktorového ověřování | [Stavy uživatele ve službě Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-get-started-user-states.md) |
| Přihlaste se jako uživatel PoC hello a procházení procesem výš hello  |  |

### <a name="considerations"></a>Požadavky

1. Hello PoC kroky v této stavebním blokem explicitně nastavení vícefaktorového ověřování pro uživatele na všechny přihlášení. Existují jiné nástroje, jako je například podmíněný přístup a ochrana Identity, které zaujmout MFA na více cílových scénářů. To bude vypadat přibližně tooconsider při přesouvání z POC tooproduction.
2. Hello PoC kroků v této stavebním blokem explicitně používají telefonní hovory jako hello metodu vícefaktorového ověřování pro expedience. Jak přejít z POC tooproduction, doporučujeme používat aplikace, jako je hello [Microsoft Authenticator](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) jako vaše druhý faktor, kdykoli je to možné.
Další informace: [koncept speciální publikace NIST 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="mfa-conditional-access-for-saas-applications"></a>MFA podmíněného přístupu pro aplikace SaaS

Přibližná doba tooComplete: 10 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Identifikujte PoC uživatelé tootarget hello zásad. Tito uživatelé musí být v zásadách podmíněného přístupu hello tooscope skupiny zabezpečení | [Konfigurace SaaS federovaného jednotného přihlašování](#saas-federated-sso-configuration) |
| Aplikace SaaS byl již nakonfigurován. |  |
| Uživatelé PoC jsou již přiřazena toohello aplikace |  |
| Přihlašovací údaje toohello POC uživatele jsou k dispozici |  |
| Testování Koncepce uživatel je registrovaný pro MFA. Pomocí telefonu s funkčním příjem | http://aka.MS/ssprsetup |
| Zařízení v interní síti hello. IP adresou nakonfigurovanou v hello interní adresa rozsahu | Najít ip adresu: https://www.bing.com/search?q=what%27s+my+ip |
| Zařízení v externí síti hello (může být telefon pomocí hello operátora mobilní sítě.) |  |

### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Přejděte tooAzure AD portálu pro správu: okno podmíněného přístupu | [Portálu pro správu Azure AD: Podmíněný přístup](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) |
| Vytvoření zásady podmíněného přístupu:<br/>-Cíl PoC uživatele v části "Uživatelé a skupiny"<br/>-Cíl aplikací PoC v části "Cloudových aplikací"<br/>-Cílový všech umístění s výjimkou důvěryhodné ty, které jsou v části "Podmínky" -> "Umístění" **Poznámka:** důvěryhodné IP adresy se konfigurují v [portál vícefaktorového ověřování](https://account.activedirectory.windowsazure.com/UserManagement/MfaSettings.aspx)<br/>-Vyžadovat vícefaktorové ověřování v části "Grant" | [Začínáme s podmíněným přístupem v Azure Active Directory: postup konfigurace zásad](active-directory-conditional-access-azure-portal-get-started.md#policy-configuration-steps) |
| Přístup z aplikace uvnitř podnikové sítě | [Začínáme s podmíněným přístupem v Azure Active Directory: testování zásad hello](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |
| Přístup k aplikaci z veřejné síti | [Začínáme s podmíněným přístupem v Azure Active Directory: testování zásad hello](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |

### <a name="considerations"></a>Požadavky

Pokud používáte federace, můžete použít hello místní zprostředkovatele Identity (IdP) toocommunicate hello uvnitř nebo mimo podnikovou síť stavu s deklaracemi identity. Tento postup můžete použít bez nutnosti toomanage hello seznam IP adres, které mohou být komplexní tooassess a spravovat ve velkých organizacích. V této instalaci potřebovat účet pro scénář "roamingu sítě" hello (uživatel, protokolování hello interní sítě a při přihlášeného přepínače umístění, například v kavárně) a ujistěte se, že vysvětlení důsledků pro hello. Další informace: [zabezpečení cloudových prostředků s Azure Multi-Factor Authentication a AD FS: důvěryhodné IP adresy pro federované uživatele](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md#trusted-ips-for-federated-users)

## <a name="privileged-identity-management-pim"></a>Správa privilegovaných identit (PIM)

Přibližná doba tooComplete: 15 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Identifikovat hello globální správce, který bude součástí hello POC v PIM. | [Začít používat Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md) |
| Identifikovat hello globální správce, který se stane hello správce zabezpečení. | [Začít používat Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)<br/> [Různé správu role v Azure Active Directory PIM](active-directory-privileged-identity-management-roles.md) |
| Volitelné: Potvrďte, pokud mají hello globální správci e-mailu přístup k e-mailová oznámení tooexercise v PIM | [Co je Azure AD Privileged Identity Management?: Konfigurace nastavení aktivace role hello](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)


### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Toohttps://portal.azure.com přihlášení jako globální správce (GA) a zavedení hello PIM okno. Hello globální správce, který provádí tento krok je nasazených jako správce zabezpečení hello.  Umožňuje volání této GA1 objektu actor | [Pomocí Průvodce hello zabezpečení v Azure AD Privileged Identity Management](active-directory-privileged-identity-management-security-wizard.md) |
| Identifikujte hello globálního správce a přesunout z trvalé tooeligible. To by měl být samostatné správce ze hello používá pro přehlednost v kroku 1. Umožňuje volání této GA2 objektu actor | [Azure AD Privileged Identity Management: Jak tooadd nebo odebrat roli uživatele](active-directory-privileged-identity-management-how-to-add-role-to-user.md)<br/>[Co je Azure AD Privileged Identity Management?: Konfigurace nastavení aktivace role hello](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)  |
| Teď Přihlaste se jako GA2 toohttps://portal.azure.com a zkuste změnit "Nastavení uživatele". Všimněte si, že se některé možnosti jsou zobrazena šedě. | |
| Na nové záložce v hello stejné relace jako krok 3, přejděte teď toohttps://portal.azure.com a přidat hello PIM okno toohello řídicího panelu. | [Jak tooactivate nebo deaktivace role v Azure AD Privileged Identity Management: Přidání aplikace Privileged Identity Management hello](active-directory-privileged-identity-management-how-to-activate-role.md#add-the-privileged-identity-management-application) |
| Role globálního správce toohello požadavek aktivace | [Jak tooactivate nebo deaktivace role v Azure AD Privileged Identity Management: aktivovat roli](active-directory-privileged-identity-management-how-to-activate-role.md#activate-a-role) |
| Všimněte si, že pokud GA2 nikdy zaregistrovali do vícefaktorového ověřování, registrace pro Azure MFA bude nutné |  |
| Přejděte zpět kartu původní toohello v kroku 3 a klikněte na tlačítko Aktualizovat hello v prohlížeči hello. Všimněte si, že teď máte přístup toochange "Uživatelská nastavení" | |
| Volitelně Pokud globální správci e-mailu povolen, můžete zkontrolovat GA1 a GA2 pro složky Doručená pošta a oznámení hello hello role aktivované |  |
| 8 zkontrolujte historie auditu hello a pozorovat, že se zobrazí hello sestavy tooconfirm hello zvýšení GA2. | [Co je Azure AD Privileged Identity Management?: kontrolní aktivita role](active-directory-privileged-identity-management-configure.md#review-role-activity) |

### <a name="considerations"></a>Požadavky

Tato funkce je součástí P2 Azure AD Premium nebo EMS E5

## <a name="discovering-risk-events"></a>Zjišťování rizikových událostí

Přibližná doba tooComplete: 20 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Zařízení s prohlížečem Tor staženy a nainstalovány | [Stáhněte si Tor prohlížeče](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Přístup k tooPOC uživatele toodo hello přihlášení | [Azure seznam strategií ochrany identit Active Directory](active-directory-identityprotection-playbook.md) |

### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Otevřete tor prohlížeče | [Stáhněte si Tor prohlížeče](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Přihlaste se toohttps://myapps.microsoft.com s hello POC uživatelský účet | [Azure Active Directory Identity Protection playbook: simulaci rizikových událostí](active-directory-identityprotection-playbook.md#simulating-risk-events) |
| Počkejte 5 – 7 minut |  |
| Přihlaste se jako globální správce toohttps://portal.azure.com a otevře okno hello Identity Protection | https://aka.MS/aadipgetstarted |
| Otevřete hello riziko události okno. Měli byste vidět položku v části "Přihlášení z anonymních IP adres"  | [Azure Active Directory Identity Protection playbook: simulaci rizikových událostí](active-directory-identityprotection-playbook.md#simulating-risk-events) |

### <a name="considerations"></a>Požadavky

Tato funkce je součástí P2 Azure AD Premium nebo EMS E5

## <a name="deploying-sign-in-risk-policies"></a>Nasazení zásad riziko přihlášení

Přibližná doba tooComplete: 10 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Zařízení s prohlížečem Tor staženy a nainstalovány | [Stáhněte si Tor prohlížeče](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Přístup do POC uživatele toodo hello protokolu při testování |  |
| Testování Koncepce uživatel je registrovaný pomocí vícefaktorového ověřování. Ujistěte se, že toouse telefonem s funkčním příjem | Stavební blok: [Azure Multi-Factor Authentication s telefonní hovory](#azure-multi-factor-authentication-with-phone-calls) |


### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Přihlaste se jako toohttps://portal.azure.com globálního správce a otevřete okno hello Identity Protection | https://aka.MS/aadipgetstarted |
| Povolte zásadu přihlášení riziko takto:<br/>– Přiřazené k: POC uživatele<br/>-Podmínky: Přihlášení riziko, střední a vyšší (přihlášení z anonymních umístění se považuje jako riziko střední úroveň)<br/>– Ovládací prvky: Vícefaktorové ověřování vyžadovat | [Azure Active Directory Identity Protection playbook: riziko přihlášení](active-directory-identityprotection-playbook.md#sign-in-risk) |
| Otevřete tor prohlížeče | [Stáhněte si Tor prohlížeče](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Přihlaste se toohttps://myapps.microsoft.com s hello PoC uživatelský účet |  |
| Všimněte si hello ověřovací test MFA | [Přihlášení vyskytne s Azure AD Identity Protection: obnovení rizikové přihlášení](active-directory-identityprotection-flows.md#risky-sign-in-recovery)

### <a name="considerations"></a>Požadavky

Tato funkce je součástí Azure AD Premium P2 nebo EMS E5. toolearn najdete další informace o rizikových událostech: [rizikových událostí Azure Active Directory](active-directory-reporting-risk-events.md)

## <a name="configuring-certificate-based-authentication"></a>Konfigurace ověřování na základě certifikátů

Přibližná doba toocomplete: 20 minut

### <a name="pre-requisites"></a>Požadavky

| Předpoklad | Zdroje |
| --- | --- |
| Zařízení s uživatelský certifikát zřízení (Windows, iOS nebo Android) z infrastruktury veřejných KLÍČŮ rozlehlé sítě | [Nasazení uživatelských certifikátů](https://msdn.microsoft.com/library/cc770857.aspx) |
| Azure AD domain sdružených se službou AD FS | [Azure AD Connect a federace](./connect/active-directory-aadconnectfed-whatis.md)<br/>[Přehled služby Active Directory Certificate Services](https://technet.microsoft.com/library/hh831740.aspx)|
| Pro zařízení s iOS mají nainstalovanou aplikaci Microsoft Authenticator | [Začínáme s aplikací Microsoft Authenticator hello](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) |

### <a name="steps"></a>Kroky

| Krok | Zdroje |
| --- | --- |
| Povolení "Ověřování pomocí certifikátů" na služby AD FS | [Konfigurace zásad ověřování: tooconfigure primární ověřování globálně ve Windows serveru 2012 R2](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-authentication-policies#to-configure-primary-authentication-globally-in-windows-server-2012-r2) |
| Volitelné: Povolení ověřování pomocí certifikátů ve službě Azure AD pro klienty protokolu Exchange Active Sync | [Začínáme s ověřováním na základě certifikátů ve službě Azure Active Directory](active-directory-certificate-based-authentication-get-started.md) |
| Přejděte tooAccess panely a ověřování pomocí certifikátu uživatele | https://myapps.microsoft.com |

### <a name="considerations"></a>Požadavky

toolearn najdete další informace o upozornění tohoto nasazení: [služby AD FS: ověření certifikátu s Azure AD & Office 365](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)



> [!NOTE]
> U sebe uživatelský certifikát by měl být chráněn. Buď pomocí správy zařízení nebo pomocí kódu PIN v případě čipové karty.



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
