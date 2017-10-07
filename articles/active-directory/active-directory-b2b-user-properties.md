---
title: "aaaProperties spolupráce uživatele Azure Active Directory s B2B | Microsoft Docs"
description: "Vlastnosti uživatelů služby Active Directory s B2B spolupráce se dají konfigurovat"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/25/2017
ms.author: sasubram
ms.openlocfilehash: 78709f64430ed4c14eadf4dc257f175c24698c5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Vlastnosti uživatele spolupráce Azure Active Directory s B2B

Uživatelé Azure Active Directory (Azure AD) (B2B) business-to-business spolupráce je uživatel s UserType = hosta. Tento uživatel guest obvykle je od organizace partnera a má omezené oprávnění v hello pozvání adresář, ve výchozím nastavení.

V závislosti na hello pozvání potřebám organizace uživatelé spolupráce Azure AD B2B může být v jednom z hello následující stavy účet:

- Stav 1: Adresami v externí instanci Azure AD a reprezentována jako uživatel guest v hello pozvání organizace. V takovém případě hello B2B uživatel přihlásí pomocí účtu Azure AD, který patří toohello pozvat klienta. Pokud hello partnerské organizace nepoužívá Azure AD, vytvoří se stále uživatele guest hello ve službě Azure AD. požadavky Hello je, že jejich uplatnit své pozvánky a Azure AD ověřuje e-mailové adresy. Toto uspořádání se také nazývá klientů za běhu (JIT) nebo "virální" klientů.

- Stav 2: Adresami v účtu Microsoft a reprezentována jako uživatel guest v organizaci hello hostitele. V takovém případě hello hosta uživatel přihlásí pomocí účtu Microsoft. Hello pozvat sociálních identitu uživatele (google.com nebo podobné), což není účet Microsoft, se vytvoří během nabídka uplatnění jako účet Microsoft.

- Stav 3: Adresami v organizaci hello hostitele v místní službě Active Directory a synchronizované s hello hostitele organizace Azure AD. Během této verze je nutné použít PowerShell toomanually změnu hello UserType takových uživatelů v cloudu hello.

- Stav 4: Adresami v hostiteli organizace Azure AD s UserType = hosta a přihlašovací údaje, které spravuje organizace hello hostitele.

  ![zobrazení iniciály pozvánky hello](media/active-directory-b2b-user-properties/redemption-diagram.png)


Teď se podíváme, jak uživatelé spolupráce Azure AD B2B ve stavu 1 vypadá ve službě Azure AD.

### <a name="before-invitation-redemption"></a>Před pozvánku k uplatnění.

![Před nabídka uplatnění.](media/active-directory-b2b-user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Po uplatnění Pozvánka

![Po uplatnění nabídka](media/active-directory-b2b-user-properties/after-redemption.png)

## <a name="key-properties-of-hello-azure-ad-b2b-collaboration-user"></a>Klíčové vlastnosti uživatele spolupráce Azure AD B2B hello
### <a name="usertype"></a>UserType
Tato vlastnost určuje hello relace hello uživatele toohello hostitele klientů. Tato vlastnost může mít dvě hodnoty:
- Člen: Tato hodnota označuje zaměstnanec hello hostitele organizace a uživatele v organizaci hello mzdy. Tento uživatel například očekává pouze toointernal přístup k webům toohave. Tohoto uživatele nebyla považována za externí spolupracovníka.

- Host: Tato hodnota označuje uživatele, který není považována za interní toohello společnosti, jako je například externí spolupracovník, partnera, zákazník nebo podobné uživatele. Takový uživatel by být očekávané tooreceive interní memo generálního ředitele nebo přijímat společnosti výhod, třeba.

  > [!NOTE]
  > Hello UserType má žádný vztah toohow hello uživatel přihlásí, role directory hello hello uživatele a tak dále. Tato vlastnost jednoduše označuje hello uživatele vztah toohello hostitele organizace a umožňuje hello organizace tooenforce zásady, které jsou závislé na této vlastnosti.

### <a name="source"></a>Zdroj
Tato vlastnost určuje, jak hello uživatel přihlásí.

- Pozvat uživatele: Tento uživatel má dostali pozvánku, ale nebyla ještě uplatněny Pozvánka.

- Externí služby Active Directory: Tento uživatel je adresami v organizaci externí a provede ověření pomocí účtu Azure AD, který patří toohello jiné organizaci. Tento typ přihlášení odpovídá tooState 1.

- Účet Microsoft: Tento uživatel je adresami v účtu Microsoft a provede ověření pomocí účtu Microsoft. Tento typ přihlášení odpovídá tooState 2.

- Windows Server Active Directory: Tento uživatel je přihlášený z místní služby Active Directory, který patří organizaci toothis. Tento typ přihlášení odpovídá tooState 3.

- Azure Active Directory: Tento uživatel se ověřuje pomocí účtu Azure AD, který patří organizaci toothis. Tento typ přihlášení odpovídá tooState 4.
  > [!NOTE]
  > Zdroj a UserType jsou nezávislé vlastnosti. Hodnota zdroje neznamená pro UserType určitou hodnotu.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Může uživatele Azure AD s B2B přidáni jako členové místo hosté?
Obvykle je shodný služby Azure AD s B2B uživatele a uživatele typu Host. Proto uživatelé spolupráce Azure AD B2B je přidána jako uživatel s UserType = hosta ve výchozím nastavení. Ale v některých případech hello organizaci partnera poskytujícího prostředky je že členem větší organizaci hostitele toowhich hello organizace také patří. Pokud ano, hello hostitele organizace chtít tootreat uživatelé v organizaci partnera hello jako členy místo guests. Použít rozhraní API Správce Azure AD s B2B pozvánku Manager tooadd hello nebo pozvat uživatele od hello hostitele organizace toohello organizace partnera jako člena.

## <a name="filter-for-guest-users-in-hello-directory"></a>Filtr pro uživatele typu Host v adresáři hello

![Filtrovat uživatele typu Host](media/active-directory-b2b-user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>Převést UserType
V současné době je možné pro uživatele tooconvert UserType z tooGuest člen a naopak pomocí prostředí PowerShell. Hello vlastnosti UserType je by měl uživatel hello toorepresent vztah toohello organizace. Proto hello hodnota této vlastnosti změnit pouze v případě, že změní hello vztah hello uživatele toohello organizace. Pokud se změní hello vztah hello uživatele, měli problémy, jako jestli by se měl změnit hello hlavní název uživatele (UPN), řešit? Hello uživatele pokračovat toohave přístup toohello stejné prostředky? By měla být přiřazená poštovní schránky? Nedoporučujeme proto změna hello UserType pomocí prostředí PowerShell jako atomic aktivita. Kromě toho v případě, že tato vlastnost bude neměnné pomocí prostředí PowerShell, nedoporučujeme trvá závislost na této hodnotě.

## <a name="remove-guest-user-limitations"></a>Odeberte omezení uživatele guest
Někdy může být místo toogive vaše hosta vyšší oprávnění uživatelů. Můžete přidat roli hosta uživatele tooany a dokonce odebírat hello výchozí hosta uživatele omezení v hello directory toogive uživatele hello, stejné oprávnění jako členy.

Je možné tooturn vypnout hello výchozí hosta uživatele omezení tak, aby uživatel guest v adresáři společnosti hello hello stejná oprávnění jako uživatel člen.

![Odeberte omezení uživatele guest](media/active-directory-b2b-user-properties/remove-guest-limitations.png)

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Přidání role uživatele tooa spolupráce B2B](active-directory-b2b-add-guest-to-role.md)
* [Delegovat pozvánek spolupráce B2B](active-directory-b2b-delegate-invitations.md)
* [Uživatel spolupráce B2B auditování a vytváření sestav](active-directory-b2b-auditing-and-reporting.md)
* [Dynamické skupiny a spolupráci B2B](active-directory-b2b-dynamic-groups.md)
* [Kód spolupráce B2B a ukázky prostředí PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurace aplikací SaaS pro spolupráci B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Deklarace uživatele spolupráce B2B mapování](active-directory-b2b-claims-mapping.md)
* [Externí sdílení Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuální omezení spolupráce B2B](active-directory-b2b-current-limitations.md)
