---
title: "aaaDelegate pozvánky pro spolupráci Azure Active Directory s B2B | Microsoft Docs"
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
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: c0122d6f60d494c6e251c41d947dc254ea887620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Delegát pozvánky pro spolupráci Azure Active Directory s B2B

Se spoluprací business-to-business (B2B) Azure Active Directory (Azure AD) nemáte toobe pozvánek toosend globálního správce. Místo toho můžete použít zásady a delegovat pozvánek toousers, jejichž role mohly toosend pozvánky. Důležité nový způsob toodelegate hosta uživatele pozvánek je prostřednictvím hello pozvání hosta odeslal role.

## <a name="guest-inviter-role"></a>Pozvání hosta odeslal role
Jsme můžete přiřadit uživatele tooGuest hello pozval vás role toosend pozvánky. Nemáte toobe členem pozvánek toosend roli globálního správce hello. Ve výchozím nastavení můžete běžní uživatelé také vyvolat API pozvání hello, pokud není globální správce zakázáno pozvánky k běžní uživatelé. Uživatele můžete také vyvolat hello rozhraní API pomocí hello portál Azure nebo Powershellu.

Tady je příklad, který ukazuje, jak toouse prostředí PowerShell tooadd roli uživatele toohello pozvání hosta odeslal:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Ovládací prvek, který můžete pozvat

![Ovládací prvek jak tooinvite](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

S spolupráce Azure AD B2B můžete nastavit správce tenanta hello následující pozvánku zásady:

- Vypnout pozvánek
- Můžete pozvat jenom správci a uživatelé v roli hosta pozvánky hello
- Můžete pozvat správci, hello pozvání hosta odeslal role a členů
- Všichni uživatelé, včetně hostů, můžete pozvat

Ve výchozím nastavení, klienti jsou nastavené příliš č. 4. (Všechny uživatele, včetně hostů, můžete uživatele pozvat, B2B.)

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md)
* [Přidání role uživatele tooa spolupráce B2B](active-directory-b2b-add-guest-to-role.md)
* [Dynamické skupiny a spolupráci B2B](active-directory-b2b-dynamic-groups.md)
* [Kód spolupráce B2B a ukázky prostředí PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurace aplikací SaaS pro spolupráci B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Deklarace uživatele spolupráce B2B mapování](active-directory-b2b-claims-mapping.md)
* [Externí sdílení Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuální omezení spolupráce B2B](active-directory-b2b-current-limitations.md)
