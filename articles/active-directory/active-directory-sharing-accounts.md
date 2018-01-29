---
title: "Sdílení účtů pomocí služby Azure AD | Microsoft Docs"
description: "Popisuje, jak Azure Active Directory umožňuje organizacím bezpečně sdílet účty pro místní aplikace a příjemce cloudové služby."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: e2d77104-d978-46a3-bfea-03ffdf3b61e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 3b6a83d91ec5d8466669655d6c3bd7ae7b42dd2f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="sharing-accounts-with-azure-ad"></a>Sdílení účtů s Azure AD
## <a name="overview"></a>Přehled
Někdy organizace je třeba použít jeden uživatelské jméno a heslo pro více osob, kterým obvykle dochází ve dvou případech:

* Při přístupu k aplikacím, které vyžaduje pro každého uživatele jedinečné přihlášení a heslo, zda místní aplikace nebo příjemce cloudové služby (například podniková sociální média účty).
* Při vytváření prostředí s více uživateli. Může mít jeden, místní účet, který se zvýšenými oprávněními a slouží k základní instalaci, správu a obnovení aktivity. Například místní "globální správce" účtu Office 365 nebo kořenového účtu v Salesforce.

Tradičně tyto účty jsou sdíleny správné jednotlivcům distribuci pověření (uživatelské jméno a heslo) a ukládat je do sdíleného umístění, kde více důvěryhodné agenty k nim mají přístup.

Tradiční sdílení model má několik nevýhody:

* Povolení přístupu k nové aplikace vyžaduje, abyste distribuovat přihlašovací údaje pro všechny uživatele, které potřebuje přístup.
* Každé sdílené aplikace může vyžadovat vlastní jedinečnou sadu sdílené přihlašovací údaje, by uživatelé museli pamatovat více sad přihlašovací údaje. Uživatelé, kteří mají k pamatovat mnoho přihlašovací údaje, se zvyšuje riziko jejich uchýlit k rizikové postupy. (například zápis dolů hesla).
* Nelze zjistit, kdo má přístup k aplikaci.
* Nelze zjistit, kdo má *přístup* aplikace.
* Pokud chcete odebrat přístup k aplikaci, budete muset aktualizovat přihlašovací údaje a znovu distribuovat pro všechny uživatele, který potřebuje přístup k dané aplikaci.

## <a name="azure-active-directory-account-sharing"></a>Sdílení Azure pro účet služby Active Directory
Azure AD poskytuje nový přístup k použití sdílených účtů, který eliminuje tyto nevýhody.

Správce Azure AD nakonfiguruje, aplikace, které může uživatel získat přístup pomocí přístupového panelu a vyberete typ nejvhodnější přihlašování vhodné pro tuto aplikaci. Jeden z těchto typů *založené na heslech jednotného přihlašování*, umožňuje, aby fungoval jako typ "zprostředkovatele" během procesu přihlášení pro tuto aplikaci Azure AD.

Uživatelé přihlašovat jednou pro účet své organizace. Tento účet je stejný jako ten, který pravidelně používají pro přístup k ploše nebo e-mailu. Mohou zjišťovat a přístup pouze aplikace, které jsou přiřazeny. S sdílené účty tento seznam aplikací, může obsahovat libovolný počet sdílené přihlašovací údaje. Koncový uživatel nebude muset mějte na paměti, nebo si poznamenejte různé účty, které by mohly používat.

Sdílené účty nejen zvýšit dohledu a zlepšit použitelnost, budou také zvýšit zabezpečení. Uživatelé s oprávněními k použití pověření nevidíte sdílené heslo, ale spíše získání oprávnění používat heslo jako součást tok orchestrovat ověřování. Navíc některé aplikace jednotného přihlašování k heslo získáte možnost pomocí služby Azure AD pravidelně hesla výměny (update). Systém použije složitých hesel, která zvyšuje zabezpečení účtu. Správce můžete snadno udělit nebo odvolat přístup k aplikaci vědět, kdo má přístup k účtu, a který přistupoval v minulosti.

Azure AD podporuje sdílené účty pro všechny Enterprise Mobility Suite (EMS), Premium nebo Basic licenci uživatelů, všech typů heslo jeden přihlašování aplikací. Účty pro všechny tisícům předem integrovaných aplikací v galerii aplikací můžete sdílet a můžete přidat vlastní aplikaci ověřování hesla s [vlastních aplikací jednotné přihlašování](active-directory-enterprise-apps-manage-sso.md).

Azure AD funkce, které umožňují sdílení účet:

* [Heslo jednotné přihlašování](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Heslo jednoho přihlášení agenta
* [Přiřazení skupiny](active-directory-accessmanagement-self-service-group-management.md)
* Vlastní heslo aplikace
* [Využití aplikace řídicího panelu a sestavách](active-directory-passwords-get-insights.md)
* Portálech pro koncové uživatele přístup
* [Proxy aplikace](active-directory-application-proxy-get-started.md)
* [Služby Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Sdílení účet
Chcete-li sdílet účet pomocí Azure AD, budete muset:

* Přidání aplikace [Galerie aplikací](https://azure.microsoft.com/marketplace/active-directory/) nebo [vlastní aplikace](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
* Konfigurace aplikace pro heslo jednotné přihlašování (SSO)
* Použití [přiřazování na základě skupiny](active-directory-accessmanagement-group-saasapps.md) a vyberte možnost a zadejte sdílené přihlašovací údaje
* Volitelné: v některých aplikacích, jako je Facebook, Twitter a LinkedIn, můžete povolit možnost pro [Azure AD automatizované převrácení heslo](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Můžete provést také sdíleného účtu bezpečnější s Multi-Factor Authentication (MFA) (Další informace o [zabezpečení aplikací s Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) a můžete delegovat možnosti správy, kdo má přístup k aplikaci pomocí [ Samoobslužná služba Azure AD](active-directory-accessmanagement-self-service-group-management.md) skupiny správy.

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Ochrana aplikací pomocí podmíněného přístupu](active-directory-conditional-access-azure-portal.md)
* [Samoobslužné služby skupiny správy nebo SSAA](active-directory-accessmanagement-self-service-group-management.md)

