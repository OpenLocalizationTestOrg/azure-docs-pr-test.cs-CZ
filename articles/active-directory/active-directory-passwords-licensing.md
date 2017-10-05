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
ms.openlocfilehash: 936134bddad19964f809a17f200ebbeed5aa853c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Resetování licenční požadavky pro hesla pomocí samoobslužné služby Azure AD

V pořadí pro resetování hesel služby Azure AD pro funkce které **musí mít alespoň jednu licenci přiřazené ve vaší organizaci**. Nevynucovat jsme licencování na vlastní uživatelské prostředí resetování hesla na uživatele. Chcete-li udržovat kompatibilitu s licenční smlouvou Microsoft, přiřadit licence pro všechny uživatele, které používají prémiových funkcí.

* **Jenom uživatelé v cloudu** -Office 365 (O365) žádné placené SKU nebo Azure AD Basic
* **Cloud** nebo **místních uživatelů** – Azure AD Premium P1 nebo P2, Enterprise Mobility + Security (EMS) nebo zabezpečení produktivní Enterprise (ZAD)

## <a name="licenses-required-for-password-writeback"></a>Licence potřebné pro zpětný zápis hesla

Pokud chcete používat zpětný zápis hesla, musí mít jednu z následujících licencí přiřazených ve vašem klientovi.

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Secure Productive Enterprise E3
* Secure Productive Enterprise E5

> [!NOTE]
> Samostatné Office 365 licenční plány **nepodporují zpětný zápis hesla** a vyžaduje jedna z předchozí plány pro tato funkce fungovat.

Další informace o licencování včetně náklady najdete na následujících stránkách

* [Azure Active Directory ceny lokality](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Zabezpečené produktivní Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

## <a name="enable-group-or-user-based-licensing"></a>Povolit skupinu nebo uživatele na základě licencí

Azure AD nyní podporuje na základě skupiny licencování povolení správci přiřadit hromadných licencí pro skupinu uživatelů, nikoli po jednom přiřazení. [Přiřadit zkontrolujte a vyřešte problémy s licencí](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních. Předtím, než je možné přiřadit licence pro uživatele, Správce musí určovat vlastnost "Umístění využití" na uživatele. Přiřazení licencí, lze provést v rámci uživatele > Profil > v oddílu nastavení na portálu Azure. **Při použití přiřazení skupiny licencí, zdědí všechny uživatele bez využití umístění zadané umístění adresáře.**

## <a name="next-steps"></a>Další kroky

Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD

* [**Rychlý Start**](active-directory-passwords-getting-started.md) – Zprovozněte samoobslužné resetování hesla Azure AD. 
* [**Data**](active-directory-passwords-data.md) – Pochopte požadovaná data a jejich použití pro správu hesel.
* [**Uvedení**](active-directory-passwords-best-practices.md) – Naplánujte a nasaďte pro své uživatele samoobslužné resetování hesla pomocí zde uvedených pokynů.
* [**Přizpůsobení**](active-directory-passwords-customize.md) – Přizpůsobte vzhled a chování samoobslužného resetování hesla pro vaši společnost.
* [**Vytváření sestav**](active-directory-passwords-reporting.md) – Zjistěte, jestli, kdy a kde vaši uživatelé používají funkci samoobslužného resetování hesla.
* [**Podrobné technické informace**](active-directory-passwords-how-it-works.md) – Nahlédněte za oponu a pochopte, jak to funguje.
* [**Nejčastější dotazy**](active-directory-passwords-faq.md) – Jak? Proč? Co? Kde? Kdo? Kdy? – Odpovědi na otázky, na které jste se vždy chtěli zeptat.
* [**Řešení potíží**](active-directory-passwords-troubleshoot.md) – Zjistěte, jak řešit běžné problémy, ke kterým dochází u samoobslužného resetování hesla.

