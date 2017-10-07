---
title: "aaaIntegrate Azure Active Directory jednotné přihlašování s aplikacemi SaaS | Microsoft Docs"
description: "Povolte jednotné přihlašování, ověřování a zřizování centralizovanou správu přístupu aplikace SaaS ve službě Azure Active Directory uživatelů. Přehled o aplikace tooSaaS toointegrate Azure Active Directory."
services: active-directory
keywords: Integrace s aplikacemi SaaS Azure AD
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 53b9d341-d1fc-4bbb-ac7c-3f4c68fcf00a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: aaronsm
ms.openlocfilehash: fe621a30429c81c32470635d105ae3e95184efa1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Integrovat Azure Active Directory jednotné přihlašování s aplikacemi SaaS
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-enterprise-apps-manage-sso.md)
> * [Portál Azure Classic](active-directory-sso-integrate-saas-apps.md)
>
>

[!INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

tooget spuštění nastavení jednotného přihlašování pro aplikaci, která se převedou do vaší organizace, budete používat existující adresář v Azure Active Directory (Azure AD). Můžete vytvořit adresář služby Azure AD, který získáte prostřednictvím Microsoft Azure, Office 365 nebo služby Windows Intune. Pokud máte dva nebo více z těchto, najdete v části [Správa adresáře služby Azure AD](active-directory-administer.md) toodetermine které jeden toouse.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. Jak tooassign rolí správce ve službě Azure AD Dobrý den, správce center, najdete v části [přiřazení rolí správce v Azure Active Directory](active-directory-enterprise-apps-manage-sso.md).

## <a name="authentication"></a>Authentication
Pro aplikace, které podporují hello SAML 2.0, WS-Federation, nebo OpenID Connect protokoly služby Azure Active Directory používá podepisování vztahy důvěryhodnosti tooestablish certifikáty. Další informace o tom najdete v tématu [Správa certifikátů pro federované jednotné přihlašování](active-directory-sso-certs.md).

Pro aplikace, které podporují pouze HTML založené na formulářích přihlášení, Azure Active Directory používá 'heslo překlenutí vyrovnávací paměti, tooestablish vztahy důvěryhodnosti. Toto umožňuje hello uživatelé ve vaší organizaci toobe automaticky přihlášeni v tooa aplikace SaaS ve službě Azure AD pomocí informací o uživatelském účtu hello z hello aplikaci SaaS. Azure AD shromažďuje a bezpečně ukládá informace o uživatelském účtu hello a související heslo hello. Další informace najdete v tématu [založené na heslech jednotné přihlašování](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Autorizace
Účet zřízené umožňuje toouse toobe oprávnění uživatele aplikace po mít ověření přes jednotné přihlašování. Zřizování uživatelů lze provést ručně, nebo v některých případech můžete přidávat a odebírat uživatelské informace z aplikace SaaS hello na základě změn provedených ve službě Azure Active Directory. Další informace o použití existujících konektorů Azure AD pro automatizované zajišťování najdete v tématu [automatické zřizování uživatelů a jeho rušení pro aplikace SaaS](active-directory-saas-app-provisioning.md).

Jinak můžete ručně přidat uživatele informace tooan aplikaci, nebo použití jiných zřizování řešení, které jsou k dispozici v hello marketplace.

## <a name="access"></a>Access
Azure AD poskytuje několik způsobů přizpůsobitelné toodeploy aplikace tooend uživatelé ve vaší organizaci. Abyste neměli zablokován do žádné konkrétní nasazení nebo řešení přístupu. Můžete použít [hello řešení, které nejlépe vyhovuje vašim potřebám](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Další informace pro aplikace již používá
Nastavení jednotného přihlašování na pro aplikaci, která už vaše organizace používá jiný proces se od hello proces vytváření nových účtů pro novou aplikaci. Existují dvě předběžné kroků, včetně: Mapování uživatelských identit v hello identitou tooAzure AD a pochopení, jak uživatelé budou mít protokolování v aplikaci tooan po integraci.

> [!NOTE]
> tooset si jednotné přihlašování pro existující aplikace, potřebujete práva globálního správce toohave v obou Azure AD a hello aplikaci SaaS.
>
>

### <a name="mapping-user-accounts"></a>Mapování uživatelských účtů
Identita uživatele obvykle má jedinečný identifikátor, který může být e-mailovou adresu, nebo hlavní název uživatele (UPN). Budete potřebovat toolink (map) identita tootheir příslušných Azure AD identity aplikací jednotlivých uživatelů. Existuje několik způsobů tooaccomplish to v závislosti na tom, jak hello požadavek ověřování vaší aplikace.

Další informace o mapování identit aplikací s Azure AD identity najdete v tématu [přizpůsobení deklarace identity vystavené v tokenu SAML hello](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) a [přizpůsobení mapování atributů pro zřizování](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-hello-users-log-in-experience"></a>Principy hello uživatele přihlašování
Pokud integrujete jednotného přihlašování pro aplikace, která se už používá, je důležité, bude mít vliv toorealize, který hello činnost koncového uživatele. Pro všechny aplikace budou uživatelé začít používat jejich toosign přihlašovací údaje Azure AD v. Také může být, že uživatel musí použít různých portálu tooaccess hello aplikace.

Jednotné přihlašování pro některé aplikace lze provést na aplikace hello přihlášení rozhraní, ale pro jiné aplikace hello uživatel bude mít toogo přes centrální portál, jako ([Moje aplikace](http://myapps.microsoft.com) nebo [Office 365](http://portal.office.com/myapps)) toosign v. Další informace o různých typech hello jednotné přihlašování a jejich činnosti uživatelů [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

Je jiný prostředek cenné *potlačení souhlas uživatele* v hello [směrných vývojáři](active-directory-applications-guiding-developers-for-lob-applications.md) článku.

## <a name="next-steps"></a>Další kroky
Pro aplikace SaaS, které můžete najít v hello Galerie aplikací, Azure AD poskytuje řadu [kurzů aplikace SaaS toointegrate](active-directory-saas-tutorial-list.md).

Pokud aplikace není v galerii aplikací, můžete [přidejte jej jako vlastní aplikaci toohello Galerie aplikací Azure AD](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Je mnohem víc podrobností na všechny tyto problémy v hello knihovně Azure.com, počínaje [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

Plus, nezapomeňte si hello [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md).
