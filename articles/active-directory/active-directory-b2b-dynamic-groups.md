---
title: "Dynamické skupiny a spolupráce Azure Active Directory s B2B | Microsoft Docs"
description: "Ukazuje, jak používat Azure AD dynamických skupin se spoluprací Azure Active Directory s B2B"
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 12/14/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: d60b7f70332d6183a67003c1dceb96241fc6bae4
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dynamické skupiny a spolupráce Azure Active Directory s B2B

## <a name="what-are-dynamic-groups"></a>Co jsou dynamické skupiny?
Konfigurace dynamické členství ve skupině zabezpečení pro Azure Active Directory (Azure AD) je k dispozici v [portálu Azure](https://portal.azure.com). Správci mohou nastavit pravidla pro naplnění skupin, které jsou vytvořeny ve službě Azure AD založit na atributech uživatelů (například userType, oddělení nebo země). Členové dají automaticky přidat nebo odebrat ze skupiny zabezpečení na základě jejich atributů. Tyto skupiny můžete poskytnout přístup k aplikacím nebo prostředkům cloudu (webů služby SharePoint, dokumenty) a přiřadit licence na členy. Další informace o dynamických skupin v [vyhrazené skupiny ve službě Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).

Odpovídající [licencování Azure AD Premium P1 a P2](https://azure.microsoft.com/pricing/details/active-directory/) je nutná k vytváření a použití dynamických skupin. Další informace najdete v článku [vytvořit pravidla založená na atributu pro dynamické členství ve skupině v Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).

## <a name="what-are-the-built-in-dynamic-groups"></a>Co jsou integrované dynamické skupiny?
**Všichni uživatelé** dynamická skupina umožňuje správci klientů k vytvoření skupiny obsahující všechny uživatele v klientovi s jedním kliknutím. Ve výchozím nastavení **všichni uživatelé** skupina obsahuje všechny uživatele v adresáři, včetně členů a hostů.
V rámci nový portál pro správu Azure Active Directory, můžete povolit **všichni uživatelé** skupiny v nastavení skupiny zobrazení.

![Zobrazuje povolení skupině všichni uživatelé nastavenou na Ano](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

## <a name="hardening-the-all-users-dynamic-group"></a>Posílení zabezpečení všech dynamické skupiny uživatelů
Ve výchozím nastavení **všichni uživatelé** skupina obsahuje také uživatelům (Host) spolupráce B2B. Dále je možné zabezpečit vaše **všichni uživatelé** skupiny pomocí pravidla odebrat uživatele typu Host. Následující obrázek ukazuje **všichni uživatelé** skupiny upravit tak, aby vyloučit hostů.

![Pravidlo ukazuje, kde typ uživatele není rovno hosta](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

Může také pro vás užitečné k vytvoření nové dynamické skupiny, která obsahuje pouze uživatele typu Host, tak, aby mohli použít zásady (například zásady Azure AD podmíněného přístupu) k nim.
Jak taková skupina může vypadat:

![Pravidlo ukazuje, kde typ uživatele rovná hosta](media/active-directory-b2b-dynamic-groups/only-guest-users.png)

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md)
* [Přidání uživatele spolupráce B2B k roli](active-directory-b2b-add-guest-to-role.md)
* [Delegovat pozvánek spolupráce B2B](active-directory-b2b-delegate-invitations.md)
* [Kód spolupráce B2B a ukázky prostředí PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurace aplikací SaaS pro spolupráci B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Deklarace uživatele spolupráce B2B mapování](active-directory-b2b-claims-mapping.md)
* [Externí sdílení Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuální omezení spolupráce B2B](active-directory-b2b-current-limitations.md)
