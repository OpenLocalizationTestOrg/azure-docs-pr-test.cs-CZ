---
title: "aaaGet spuštění integrace s aplikacemi Azure AD | Microsoft Docs"
description: "Tento článek je příručka Začínáme k integraci Azure Active Directory (AD) s místními aplikacemi a cloudových aplikací."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: db6d210d-c970-49e9-bd20-36d984bcd1c3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: 5a7a851e8418083fee72ab58477a9cab75d0d4bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Integrace služby Azure Active Directory s aplikacemi, získávání Příručka Začínáme
## <a name="overview"></a>Přehled
Toto téma je určený toogive jste plán pro integrace aplikací s Azure Active Directory (AD). Každý z níže uvedených částech hello obsahují stručné souhrnné informace o podrobnější téma, abyste mohli identifikovat, které části Tato příručka Začínáme jsou relevantní tooyou.  Postupujte podle hello odkazy na podrobnější prohlídku na každý předmět.

## <a name="before-you-begin-take-inventory"></a>Než začnete, udělejte inventáře
Před přechodem toointegrating aplikací s Azure AD, je důležité tooknow, kde jste a kde chcete toogo.  Hello tyto otázky, které jsou určený toohelp, které si myslíte o projektu integrace aplikace Azure AD.

### <a name="application-inventory"></a>Inventář aplikací
* Kde jsou všechny aplikace? Kdo je vlastníkem je?
* Jaký typ ověřování, vaše aplikace vyžadují?
* Potřebuje přístup k aplikacím toowhich?
* Chcete, aby toodeploy novou aplikaci?
  * Bude sestavení interních a nasadit ji v instanci Azure výpočetní?
  * Použijete jeden, který je k dispozici v galerii aplikací Azure hello?

### <a name="user-and-group-inventory"></a>Soupis uživatelů a skupin
* Kde jsou umístěny uživatelské účty?
  * Místní služby Active Directory
  * Azure AD
  * V databázi samostatné aplikace, která vlastníte
  * V neschválené aplikace
  * Všechny výše uvedené hello
* Jaké oprávnění a přiřazení rolí jednotlivých aktuálně mají uživatelé? Potřebujete tooreview jejich přístup nebo jste si jisti, že vaše přiřazení přístupu a roli uživatele jsou vhodné nyní?
* Jsou skupiny už vytvořené v místní Active Directory?
  * Jak jsou uspořádány skupin?
  * Kteří jsou členy skupiny hello?
  * Jaké oprávnění nebo roli přiřazení skupiny hello aktuálně mají?
* Budete potřebovat tooclean až uživatele nebo skupiny databází před integrací?  (Toto je poměrně důležité otázku. Uvolňování paměti v paměti se.)

### <a name="access-management-inventory"></a>Správa inventáře přístup
* Jak budete aktuálně spravovat tooapplications přístup uživatele? Potřebuje, toochange?  Můžete udělat další způsoby toomanage přístup, například s [RBAC](role-based-access-control-configure.md) například?
* Potřebuje přístup toowhat?

Možná nemáte hello odpovědi tooall těchto otázek budete ale to nevadí.  Tento průvodce vám může pomoct odpovědět na některé z těchto dotazů a některé informovanému rozhodnutí.

## <a name="prerequisites"></a>Požadavky
* Předplatné Azure a adresář služby Azure Active Directory.  Pokud nemáte předplatné Azure, můžete vyzkoušet Azure zdarma po dobu 30 dnů. [Vyzkoušejte si to!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Integrace aplikací s Azure AD
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Vyhledání aplikace neschválená cloudových aplikací s Cloud App Discovery
Jak je uvedeno nahoře, může být aplikace, které nebyly byla spravovaná vaší organizací až doteď.  Jako součást procesu inventáře hello je možné toofind neschválená cloudové aplikace. V tématu [hledání nedovolené cloudové aplikace s Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Typy ověřování
Každý z vašich aplikací mohou mít různá ověřovací požadavky. S Azure AD podpisové certifikáty lze použít s aplikací, které používají SAML 2.0, WS-Federation, nebo OpenID Connect protokoly, stejně jako heslo jednotné přihlašování. Další informace o aplikaci typy ověřování pro použití s Azure AD najdete v části [Správa certifikátů pro federované jednotné přihlašování v Azure Active Directory](active-directory-sso-certs.md) a [jednotného přihlašování na založené na heslech](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Povolení přihlášení SSO se Proxy aplikace Azure AD
S Microsoft Azure AD Application Proxy můžete zadat tooapplications přístupu umístěný uvnitř vaší privátní sítě bezpečně, z libovolného místa a na jakémkoli zařízení. Po instalaci konektoru proxy aplikace v rámci vašeho prostředí, můžete snadno nastavit s Azure AD.

### <a name="integrating-applications-with-azure-ad"></a>Integrace aplikací s Azure AD
Hello následující články popisují různé způsoby hello aplikace integraci s Azure AD a obsahují některé pokyny.

* [Určení, které toouse služby Active Directory](active-directory-administer.md)
* [Pomocí aplikace v galerii aplikací Azure hello](active-directory-appssoaccess-whatis.md)
* [Integrace seznam kurzy aplikací SaaS](active-directory-saas-tutorial-list.md)

## <a name="managing-access-tooapplications"></a>Správa přístupu tooapplications
Hello následující články popisují způsoby, jak můžete spravovat přístup tooapplications po byly integrovány s Azure AD pomocí konektorů Azure AD a Azure AD.

* [Správa tooapps přístup pomocí služby Azure AD](active-directory-managing-access-to-apps.md)
* [Automatizace pomocí konektorů Azure AD](active-directory-saas-app-provisioning.md)
* [Přiřazení uživatelů tooan aplikace](active-directory-applications-guiding-developers-assigning-users.md)
* [Přiřazení skupiny tooan aplikace](active-directory-applications-guiding-developers-assigning-groups.md)
* [Sdílení účtů](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integrace vlastních aplikací
Pokud jsou zápis novou aplikaci a chcete tooassist vývojáři v využití hello power Azure AD, najdete v části [směrných vývojáři](active-directory-applications-guiding-developers-for-lob-applications.md).

Pokud chcete tooadd vaší vlastní aplikaci toohello galerii aplikací Azure, najdete v části ["Přineste vlastní aplikace" s konfigurací Azure AD samoobslužného SAML](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Viz také
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

