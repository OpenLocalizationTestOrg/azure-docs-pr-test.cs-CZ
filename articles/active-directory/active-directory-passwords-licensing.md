---
title: "Licence samoobslužného resetování hesel – Azure Active Directory"
description: "Azure AD samoobslužného resetování hesel licenční požadavky"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: ca6d7a6d2b7ffa49f3656510010cfb49a81e22d7
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Resetování licenční požadavky pro hesla pomocí samoobslužné služby Azure AD

Aby funkce, pro vytvoření nového hesla Azure Active Directory (Azure AD) můžete *musí mít alespoň jednu licenci přiřazené ve vaší organizaci*. Jsme nevynucují licencování na vlastní uživatelské prostředí resetování hesla na uživatele. Chcete-li udržovat kompatibilitu s licenční smlouvou Microsoft, přiřadit licence pro všechny uživatele, které používají prémiových funkcí.

* **Uživatelé cloudové**: Office 365 žádné placené SKU nebo Azure AD Basic
* **Cloud** nebo **místních uživatelů**: Azure AD Premium P1 nebo P2, Enterprise Mobility + Security (EMS) nebo Microsoft 365

## <a name="licenses-required-for-password-writeback"></a>Licence potřebné pro zpětný zápis hesla

Pokud chcete používat zpětný zápis hesla, musí mít jednu z následujících licencí přiřazených na vašeho klienta:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Microsoft 365 (plán E3)
* Microsoft 365 (Plan E5)

> [!WARNING]
> Samostatné Office 365 licenční plány *nepodporují zpětný zápis hesla* a vyžadovat, že máte jeden z předchozí plánů pro tuto funkci pro práci.
>

Další licenční informace, včetně náklady, najdete na následujících stránkách:

* [Azure Active Directory ceny lokality](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory funkce a možnosti](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Povolit skupinu nebo uživatele na základě licencí

Azure AD nyní podporuje, na základě skupin licencí. Správci mohou přiřadit hromadných licencí pro skupinu uživatelů, než jim přiřadil najednou. Další informace najdete v tématu [přiřadit zkontrolujte a vyřešte problémy s licencí](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses).

Některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních. Předtím, než je možné přiřadit licence pro uživatele, Správce musí určit **umístění využití** vlastnost na uživatele. Přiřazení licencí, lze provést v rámci **uživatele** > **profil** > **nastavení** části portálu Azure. *Při použití přiřazení skupiny licencí, zdědí všechny uživatele bez využití umístění zadané umístění adresáře.*

## <a name="next-steps"></a>Další postup

* [Jak dokončit úspěšné zavedení SSPR?](active-directory-passwords-best-practices.md)
* [Resetování nebo změna hesla](active-directory-passwords-update-your-own-password.md)
* [Registrace samoobslužného resetování hesla](active-directory-passwords-reset-register.md)
* [Jaká data používá SSPR a která data byste měli naplnit pro vaše uživatele?](active-directory-passwords-data.md)
* [Které metody ověřování jsou dostupné pro uživatele?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jaké jsou možnosti zásad se SSPR?](active-directory-passwords-policy.md)
* [Co je zpětný zápis hesla a proč byste se o něj měli starat?](active-directory-passwords-writeback.md)
* [Jak hlásit aktivitu v SSPR?](active-directory-passwords-reporting.md)
* [Jaké jsou všechny možnosti v SSPR a co znamenají?](active-directory-passwords-how-it-works.md)
* [Myslím, že je něco poškozené. Jak řešit problémy SSPR?](active-directory-passwords-troubleshoot.md)
* [Mám otázku, která není zodpovězená jinde](active-directory-passwords-faq.md)
