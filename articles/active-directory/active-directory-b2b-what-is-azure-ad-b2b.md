---
title: "aaaWhat je spolupráce Azure Active Directory B2B? | Dokumentace Microsoftu"
description: "Spolupráce Azure Active Directory s B2B podporuje vaše vztahy povolením obchodní partnery tooselectively přístup k podnikovým aplikacím."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 1464387b-ee8b-4c7c-94b3-2c5567224c27
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 06/27/2017
ms.author: curtand
ms.custom: aaddev
ms.reviewer: sasubram
ms.openlocfilehash: 359989b66f3d012c306e8748a607662fffacb919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>Co je spolupráce B2B ve službě Azure AD?

<iframe width="560" height="315" src="https://www.youtube.com/embed/AhwrweCBdsc" frameborder="0" allowfullscreen></iframe>

Možnosti (B2B) spolupráce Azure AD business-to-business povolit všechny organizace pomocí služby Azure AD toowork bezpečně a bezpečně s uživateli z jakékoli jiné organizace, malý nebo velký. Tyto organizace může být s Azure AD nebo bez, nebo dokonce i s IT organizace nebo bez. 

Organizace pomocí služby Azure AD může poskytovat přístup partnery tootheir toodocuments, prostředky a aplikace, při zachování plnou kontrolu nad své vlastní firemní data. Vývojáři mohou použít hello Azure AD business-to-business rozhraní API toowrite aplikace, kterých se dvě organizace v další bezpečně. Je také velmi snadné pro toonavigate koncovým uživatelům.

97 % naše zákazníky jste se dozvěděli, že je velmi důležité toothem spolupráce Azure AD B2B.

![Výsečový graf](media/active-directory-b2b-what-is-azure-ad-b2b/97-percent-support.png)

Od verze časná dubna 2017 jsme měli o 3 miliony uživatelů již pomocí možnosti spolupráce Azure AD B2B. A maximálně 23 % organizace Azure AD, které mají víc než 10 uživateli již využily tyto možnosti.

## <a name="hello-key-benefits-of-azure-ad-b2b-collaboration-tooyour-organization"></a>klíčové výhody Hello organizace tooyour spolupráce Azure AD B2B

### <a name="work-with-any-user-from-any-partner"></a>Práce s všechny uživatele z partnerských

* Partneři používat svoje vlastní přihlašovací údaje

* Nevyžaduje se pro partnery toouse Azure AD

* Žádné externí adresáře nebo komplexní nastavení vyžaduje

### <a name="simple-and-secure-collaboration"></a>Jednoduché a bezpečné spolupráce

* Zadejte přístup tooany podnikové aplikace nebo data, při použití sofistikované, Azure AD zapnuté autorizační zásady

* Snadné pro uživatele

* Podnikové úrovni zabezpečení pro aplikace a data

### <a name="no-management-overhead"></a>Bez režie na správu

* Externí správu účet nebo heslo

* Žádná synchronizace nebo ruční správa životního cyklu účtu

* Žádné externí režijní náklady na správu

## <a name="you-can-easily-add-b2b-collaboration-users-tooyour-organization"></a>Můžete snadno přidat organizace tooyour uživatelé spolupráce B2B

Správci můžete přidat uživatele (Host) spolupráce B2B v hello [portál Azure](https://portal.azure.com).

![Přidat uživatele typu Host](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-toobring-their-own-identity"></a>Povolit vaší spolupracovníci toobring své vlastní identity

B2B spolupracovníci se můžete přihlásit pomocí identity si sami vyberou. Pokud uživatel hello nemá účet Microsoft nebo účtu Azure AD – jeden je pro ně bezproblémově hello dobu vytvořena pro uplatnění nabídky.

![Volba identity přihlášení](media/active-directory-b2b-what-is-azure-ad-b2b/sign-in-identity-choice.png)

### <a name="delegate-tooapplication-and-group-owners"></a>Delegovat vlastníky tooapplication a skupiny 
Aplikace a vlastníci skupiny můžete přidávat uživatele B2B přímo tooany aplikace, která se zajímáte o, zda je aplikace Microsoft, nebo ne. Správci mohou delegovat oprávnění tooadd B2B uživatelé toonon správce. Bez práv správce můžete použít hello [Azure AD Application přístupový Panel](https://myapps.microsoft.com) tooadd B2B tooapplications spolupráce uživatele nebo skupiny.

![panel přístupu](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![Přidat člena](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Zásady autorizace chránit vaše firemní obsah

Zásady podmíněného přístupu, jako je například služba Multi-Factor authentication, je možné vynutit:
- Na úrovni klienta hello
- Na úrovni aplikace hello
- Pro konkrétní uživatele tooprotect podnikové aplikace a data

![Přidat člena](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="use-our-apis-and-sample-code-tooeasily-build-applications-tooonboard"></a>Pomocí rozhraní API a ukázkový kód tooeasily sestavení aplikace tooonboard
Přepněte externími partnery na palubě v potřebám organizace přizpůsobené tooyour způsoby.

Pomocí hello [pozvánku spolupráce B2B rozhraní API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), můžete přizpůsobit své zkušenosti registrace, včetně vytváření samoobslužné portály registrace. Poskytujeme ukázkový kód pro samoobslužný portál [na Githubu](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![registrační portál](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

S spolupráce Azure AD B2B můžete získat hello úplné výkon služby Azure AD tooprotect vaše vztahy s partnery tak, aby koncoví uživatelé najít snadný a intuitivní. Proto pokračujte, hello připojení k tisícům organizací, které už používají Azure AD B2B pro jejich externí spolupráce!

## <a name="next-steps"></a>Další kroky

* Prostředí správce se nacházejí v hello [portál Azure](https://portal.azure.com).

* Informace o pracovním prostředí jsou k dispozici v hello [přístupový Panel](https://myapps.microsoft.com).

* A jako vždy připojit s hello produktový tým pro všechny zpětnou vazbu, diskusí a návrhy prostřednictvím našich [Microsoft technická komunita](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [elementy Hello hello e-mailová pozvánka pro spolupráci B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Řešení potíží s spolupráce Azure Active Directory s B2B](active-directory-b2b-troubleshooting.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení](active-directory-b2b-api.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Uživatel spolupráce B2B auditování a vytváření sestav](active-directory-b2b-auditing-and-reporting.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
