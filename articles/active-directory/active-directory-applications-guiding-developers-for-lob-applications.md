---
title: "aaaDevelop aplikací pro Azure AD | Microsoft Docs"
description: "Napsané pro IT specialisty hello, tento článek obsahuje pokyny k integraci aplikací Azure se službou Active Directory."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: dd69f2bc-37c5-457c-857d-27acb84267fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2924be752af0be2843b1d9b74d9ec446d3fe1ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a>Vyvíjet-obchodní aplikace pro Azure Active Directory
Tato příručka poskytuje přehled vývoje-obchodní aplikace (LoB) pro Azure Active Directory (AD) .hello určené cílové skupiny je globální správci Active Directory nebo Office 365.

## <a name="overview"></a>Přehled
Vytváření aplikací, které jsou integrované s Azure AD poskytuje uživatelům ve vaší organizaci jednotné přihlašování s Office 365. S hello aplikace ve službě Azure AD umožňuje spravovat přes hello zásady ověřování pro aplikaci hello. Další informace o podmíněného přístupu a jak zjistit, tooprotect aplikací pomocí služby Multi-Factor authentication (MFA) toolearn [pravidla přístupu k konfigurace](active-directory-conditional-access-azuread-connected-apps.md).

Zaregistrujte toouse vaše aplikace Azure Active Directory. Registrace aplikace hello znamená, že vaše vývojáři mohou použít tooauthenticate uživatele Azure AD a požádat o přístup k prostředkům toouser například e-mailu, kalendáři a dokumenty.

Každý člen adresáři (ne Hosté) můžete zaregistrovat aplikaci, v opačném případě se označuje jako *vytvoření objektu aplikace*.

Registrace aplikace umožňuje všechny uživatele toodo hello následující:

* Získat identity pro svou aplikaci, který rozpoznává Azure AD
* Získat jeden nebo více tajné klíče nebo klíče, které hello aplikace můžete použít tooauthenticate sám sebe tooAD
* Aplikace hello Brand hello portál Azure s vlastní název, logem, atd.
* Použít aplikaci tootheir autorizace funkce Azure AD, včetně:

  * Řízení přístupu na základě role (RBAC)
  * Azure Active Directory jako server autorizace oAuth (zabezpečení rozhraní API vystavené aplikace hello)
* Deklarujte požadovaná oprávnění potřebná pro toofunction aplikace hello podle očekávání, včetně:

      - Oprávnění aplikací (jenom globální správci). Příklad: členství Role v jiné službě Azure AD aplikace nebo role členství relativní tooan prostředků Azure, skupinu prostředků nebo předplatného
      - Přidělená oprávnění (všechny uživatele). Příklad: Azure AD, přihlášení a čtení profilu

> [!NOTE]
> Ve výchozím nastavení může každý člen zaregistrovat aplikaci. toolearn jak zjistit, toorestrict oprávnění pro registraci aplikace toospecific členy, [jak se aplikace přidávají tooAzure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).
>
>

Tady je co jste globální správce hello, třeba toodo toohelp vývojář podá jejich aplikace připravené pro produkci:

* Konfigurace pravidel přístupu (zásady přístupu vícefaktorového ověřování)
* Konfigurace přiřazení uživatelských toorequire aplikace hello a přiřadit uživatele
* Potlačit hello výchozí uživatelské souhlasu prostředí

## <a name="configure-access-rules"></a>Nakonfigurovat pravidla přístupu
Konfigurace aplikace SaaS tooyour pravidla přístupu pro každou aplikaci. Můžete například vyžadovat vícefaktorové ověřování nebo povolit toousers přístup jenom v důvěryhodných sítích. Podrobnosti Hello jsou k dispozici v dokumentu hello [pravidla přístupu k konfigurace](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-hello-app-toorequire-user-assignment-and-assign-users"></a>Konfigurace přiřazení uživatelských toorequire aplikace hello a přiřadit uživatele
Ve výchozím nastavení uživatelé mohou využívat aplikace bez přiřazení. Ale pokud hello aplikace zpřístupní role nebo pokud chcete tooappear aplikace hello na panel přístupu uživatele, byste měli vyžadovat přiřazení uživatelů.

[Vyžádání přiřazení uživatele](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Pokud jste Azure AD Premium nebo Enterprise Mobility Suite (EMS) odběratele, důrazně doporučujeme použití skupin. Přiřazení skupiny toohello aplikace umožňuje toodelegate probíhající přístup správu toohello vlastníkem skupiny hello. Můžete vytvořit skupiny hello nebo požádejte hello zodpovědná strany ve vaší organizaci toocreate hello skupině pomocí vaše skupiny správy zařízení.

[Přiřazení uživatelů tooan aplikace](active-directory-applications-guiding-developers-assigning-users.md)  
[Přiřazení skupiny tooan aplikace](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Potlačit souhlas uživatele
Ve výchozím nastavení každý uživatel prochází toosign prostředí souhlasu v. prostředí pro souhlasu Hello žádostí oprávnění uživatelé toogrant tooan aplikace, může být zneklidňovat pro uživatele, kteří jsou obeznámeni s rozhodování.

Pro aplikace, které důvěřujete můžete zjednodušit hello uživatelské prostředí aplikace toohello souhlas jménem vaší organizace.

Další informace o souhlas uživatele a hello souhlasu uživatele v Azure, najdete v části [integrace aplikací s Azure Active Directory](active-directory-integrating-applications.md).

## <a name="related-articles"></a>Související články
* [Povolit zabezpečený vzdálený přístup tooon místní aplikací pomocí proxy aplikace služby Azure AD](active-directory-application-proxy-get-started.md)
* [Verze Preview Azure podmíněného přístupu pro aplikace SaaS](active-directory-conditional-access-azuread-connected-apps.md)
* [Správa přístupu tooapps s Azure AD](active-directory-managing-access-to-apps.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
