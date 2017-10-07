---
title: "aaaSharing účty pomocí služby Azure AD | Microsoft Docs"
description: "Popisuje, jak Azure Active Directory umožňuje organizacím toosecurely sdílené složky účty místními aplikacemi a příjemce cloudové služby."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e2d77104-d978-46a3-bfea-03ffdf3b61e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 9f98bfa97a6c9ba1566d3f921c1b676d5f3c2a88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sharing-accounts-with-azure-ad"></a>Sdílení účtů s Azure AD
## <a name="overview"></a>Přehled
Někdy organizace potřebují toouse jedné uživatelské jméno a heslo pro více uživatelů. To se obvykle stává ve dvou případech:

* Při přístupu k aplikacím, které vyžadují jedinečné přihlašovací jméno a heslo pro každého uživatele, zda místní aplikace nebo příjemce cloudové služby (například podniková sociální média účty).
* Při vytváření prostředí s více uživateli. Může mít jeden, místní účet, který se zvýšenými oprávněními a dá se použít toodo základní instalaci, správu a obnovení aktivit (například hello "globální účet místního správce" pro Office 365 nebo hello kořenového účtu Salesforce).

Tyto účty by sdílet tradičně, distribuci správné jednotlivce toohello hello přihlašovací údaje (uživatelské jméno a heslo) a ukládat je do sdíleného umístění, kde více důvěryhodných agentů k nim měli přístup.

Hello tradiční sdílení model má několik nevýhody:

* Povolení přístupu toonew aplikací vyžaduje přihlašovací údaje tooeveryone toodistribute, který potřebuje přístup.
* Každé sdílené aplikace může vyžadovat vlastní jedinečnou sadu sdílené přihlašovací údaje, vyžadování uživatelé tooremember více sad přihlašovací údaje. Uživatelé, kteří mají tooremember mnoho přihlašovací údaje, riziko hello zvyšuje, že se vrátí se toorisky postupy. (například zápis dolů hesla).
* Nelze zjistit, kdo má přístup tooan aplikace.
* Nelze zjistit, kdo má *přístup* aplikace.
* Pokud budete potřebovat tooremove přístup tooan aplikace, máte tooupdate hello pověření a znovu je distribuovat tooeveryone, který potřebuje přístup k aplikaci toothat.

## <a name="azure-active-directory-account-sharing"></a>Sdílení Azure pro účet služby Active Directory
Azure AD poskytuje nový přístup toousing sdílené účty, které eliminuje tyto nevýhody.

Hello správce Azure AD nakonfiguruje aplikace, které může uživatel získat přístup pomocí hello přístupového panelu a vyberete hello typ jednotného přihlašování nejvhodnější pro aplikace. Jeden z těchto typů *založené na heslech jednotného přihlašování*, umožňuje, aby fungoval jako typ "zprostředkovatele" během hello přihlášení pro tuto aplikaci Azure AD.

Uživatelé přihlašovat jednou pro účet své organizace. Toto je hello stejný účet, aby pravidelně používají tooaccess ploše nebo e-mailu. Mohou zjišťovat a přístup pouze aplikace, které jsou přiřazeny. S sdílené účty tento seznam aplikací, může obsahovat libovolný počet sdílené přihlašovací údaje. není nutné tooremember nebo zapište hello různé účty, které používají Hello koncového uživatele.

Sdílené účty nejen zvýšit dohledu a zlepšit použitelnost, budou také zvýšit zabezpečení. Uživatelům s přihlašovacími údaji hello toouse oprávnění nevidíte hello sdílené heslo, ale spíše získání oprávnění toouse hello heslo jako součást tok orchestrovat ověřování. Navíc s některými aplikacemi heslo jednotné přihlašování, máte možnost toohave hello Azure AD pravidelně výměny (update) hello heslo pomocí složitých hesel, zvýšení zabezpečení účtu hello. Hello správce můžete snadno udělit nebo odvolat přístup tooan aplikace a také vědět, kdo má účet pro přístup k toohello a kdo připojila během posledních hello.

Azure AD podporuje sdílené účty pro všechny Enterprise Mobility Suite (EMS), Premium nebo Basic licencovaní uživatelé všech typů hesla jednoho přihlášení aplikace. Účty pro všechny tisícům předem integrovaných aplikací v galerii aplikací hello můžete sdílet a můžete přidat vlastní aplikaci ověřování hesla s [vlastních aplikací jednotné přihlašování](active-directory-sso-integrate-saas-apps.md).

Azure AD funkce, které umožňují sdílení účet:

* [Heslo jednotné přihlašování](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Heslo jednoho přihlášení agenta
* [Přiřazení skupiny](active-directory-accessmanagement-self-service-group-management.md)
* Vlastní heslo aplikace
* [Využití aplikace řídicího panelu a sestavách](active-directory-passwords-get-insights.md)
* Portály koncový uživatel přístup
* [Proxy aplikace](active-directory-application-proxy-get-started.md)
* [Služby Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Sdílení účet
toouse Azure AD tooshare účet budete muset:

* Přidání aplikace [Galerie aplikací](https://azure.microsoft.com/marketplace/active-directory/) nebo [vlastní aplikace](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
* Konfigurace aplikace hello hesla jednotné přihlašování (SSO)
* Použití [přiřazení na základě skupiny](active-directory-accessmanagement-group-saasapps.md) a vyberte hello možnost tooenter sdílené přihlašovací údaje
* Volitelné: v některých aplikacích, jako je Facebook, Twitter a LinkedIn, můžete povolit možnost hello [Azure AD automatizované převrácení heslo](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Můžete provést také sdíleného účtu bezpečnější s Multi-Factor Authentication (MFA) (Další informace o [zabezpečení aplikací s Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) a můžete delegovat hello možnost toomanage, který má přístup k toohello aplikace pomocí [Samoobslužné služby azure AD](active-directory-accessmanagement-self-service-group-management.md) skupiny správy.

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Ochrana aplikací pomocí podmíněného přístupu](active-directory-conditional-access.md)
* [Samoobslužné služby skupiny správy nebo SSAA](active-directory-accessmanagement-self-service-group-management.md)

