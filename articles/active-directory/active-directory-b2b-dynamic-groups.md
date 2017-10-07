---
title: "aaaDynamic skupin a spolupráce Azure Active Directory s B2B | Microsoft Docs"
description: "Spolupráce Azure Active Directory s B2B lze použít s dynamickými skupinami Azure AD"
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
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: sasubram
ms.openlocfilehash: b011298de5fd2c851c6d9caaf5c2b257807ef0a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dynamické skupiny a spolupráce Azure Active Directory s B2B

## <a name="what-are-dynamic-groups"></a>Co jsou dynamické skupiny?
Konfigurace dynamické členství ve skupině zabezpečení pro Azure Active Directory (Azure AD) je k dispozici v [hello portál Azure](https://portal.azure.com). Správci mohou nastavit pravidla, která toopopulate skupin, které jsou vytvořené v Azure Active Directory založit na atributech uživatelů (například userType, oddělení nebo země). Členové můžete automaticky přidají tooor odebrat ze skupiny zabezpečení na základě jejich atributů. Tyto skupiny může poskytovat přístup k tooapplications nebo cloudové prostředky (webů služby SharePoint, dokumenty) a tooassign licence toomembers. Další informace o dynamických skupin v [vyhrazené skupiny ve službě Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).

Hello odpovídající [licencování Azure AD Premium P1 a P2](https://azure.microsoft.com/pricing/details/active-directory/) je požadovaná toocreate a používání dynamických skupin. Další informace najdete v článku hello [vytvořit pravidla založená na atributu pro dynamické členství ve skupině v Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).

## <a name="what-are-hello-built-in-dynamic-groups"></a>Co jsou hello integrované dynamické skupiny?
Hello **všichni uživatelé** dynamická skupina umožňuje toocreate Správci klienta skupina obsahující všechny uživatele v klientovi hello s jedním, klikněte na tlačítko. Ve výchozím nastavení, hello **všichni uživatelé** skupina obsahuje všechny uživatele v adresáři hello, včetně členů a hostů.
V rámci hello nového portálu správce Azure Active Directory, můžete zvolit tooenable hello **všichni uživatelé** v nastavení skupiny zobrazit hello.

![předdefinované skupiny](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-hello-all-users-dynamic-group"></a>Posílení zabezpečení hello všechny dynamické skupiny uživatelů
Ve výchozím nastavení, hello **všichni uživatelé** skupina obsahuje také uživatelům (Host) spolupráce B2B. Dále je možné zabezpečit vaše **všichni uživatelé** skupiny pomocí uživatele typu Host tooremove pravidla. Hello následující obrázek znázorňuje hello **všichni uživatelé** skupinu upravit tooexclude hostů.

![Povolit všechny skupiny uživatelů](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

Může také pro vás užitečné toocreate nové dynamické skupiny, který obsahuje pouze uživatele typu Host, takže můžete použít toothem zásady (například zásady Azure AD podmíněného přístupu).
Jak taková skupina může vypadat:

![vyloučit uživatele typu Host](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md)
* [Přidání role uživatele tooa spolupráce B2B](active-directory-b2b-add-guest-to-role.md)
* [Delegovat pozvánek spolupráce B2B](active-directory-b2b-delegate-invitations.md)
* [Kód spolupráce B2B a ukázky prostředí PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurace aplikací SaaS pro spolupráci B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Deklarace uživatele spolupráce B2B mapování](active-directory-b2b-claims-mapping.md)
* [Externí sdílení Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuální omezení spolupráce B2B](active-directory-b2b-current-limitations.md)
