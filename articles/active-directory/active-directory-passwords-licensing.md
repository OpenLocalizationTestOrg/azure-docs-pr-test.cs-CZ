---
title: "Správa licencí: Azure AD SSPR | Microsoft Docs"
description: "Azure AD samoobslužného resetování hesel licenční požadavky"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 9cecaaac429165346f7082f1965dc8a21063fe7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Resetování licenční požadavky pro hesla pomocí samoobslužné služby Azure AD

V pořadí pro resetování hesla v Azure AD toofunction jste **musí mít alespoň jednu licenci přiřazené ve vaší organizaci**. Nevynucovat jsme licencování v prostředí resetování hesla hello jednotlivé uživatele. toomaintain shody s multilicenční smlouvu vaší společnosti Microsoft, budete potřebovat tooassign licence tooany uživatelů, kteří používají prémiových funkcí.

* **Jenom uživatelé v cloudu** -Office 365 (O365) žádné placené SKU nebo Azure AD Basic
* **Cloud** nebo **místních uživatelů** – Azure AD Premium P1 nebo P2, Enterprise Mobility + Security (EMS) nebo zabezpečení produktivní Enterprise (ZAD)

## <a name="licenses-required-for-password-writeback"></a>Licence potřebné pro zpětný zápis hesla

zpětný zápis hesla toouse, musíte mít jednu z následujících přiřazené ve vašem klientovi licence hello.

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Secure Productive Enterprise E3
* Secure Productive Enterprise E5

> [!NOTE]
> Samostatné Office 365 licenční plány **nepodporují zpětný zápis hesla** a vyžaduje jedna z předchozích plány pro tuto funkci toowork hello.

Další informace o licencování včetně náklady naleznete na následující stránky hello

* [Azure Active Directory ceny lokality](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Zabezpečené produktivní Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a>Povolit skupinu nebo uživatele na základě licencí

Azure AD nyní podporuje na základě skupiny licencování povolení licence tooassign správci v hromadné tooa skupinu uživatelů, nikoli po jednom přiřazení. [Přiřadit zkontrolujte a vyřešte problémy s licencí](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních. Před tooa uživatele lze přiřadit licenci, musí správce hello určit vlastnost "Využití umístění" hello hello uživatele. Přiřazení licencí, lze provést v rámci uživatele > Profil > část nastavení v hello portálu Azure. **Při použití přiřazení skupiny licencí, zdědí všechny uživatele bez zadané umístění využití hello umístění adresáře hello.**

## <a name="next-steps"></a>Další kroky

Hello následující odkazy obsahují další informace o resetování hesla pomocí služby Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Data** ](active-directory-passwords-data.md) – pochopit hello data, která je požadována a jak se používají pro správu hesel
* [**Zavedení** ](active-directory-passwords-best-practices.md) -plánování a nasazení SSPR tooyour uživatelů podle pokynů hello je zde uveden
* [**Přizpůsobení** ](active-directory-passwords-customize.md) -přizpůsobit hello vzhledu a chování hello SSPR prostředí pro vaši společnost.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace** ](active-directory-passwords-how-it-works.md) -přejděte za hello závěsem toounderstand, jak to funguje
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? -Odpovědi tooquestions vždy chtěli tooask
* [**Řešení potíží s** ](active-directory-passwords-troubleshoot.md) – zjistěte, jak tooresolve běžné problémy, že vidíte s SSPR

