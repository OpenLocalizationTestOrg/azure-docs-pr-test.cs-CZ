---
title: aaaPermissions v Azure Security Center | Microsoft Docs
description: "Tento článek vysvětluje, jak Azure Security Center používá toousers oprávnění tooassign řízení přístupu podle rolí a identifikuje hello povolené akce pro každou roli."
services: security-center
cloud: na
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
ms.assetid: 
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: terrylan
ms.openlocfilehash: 03e16132dc3d951ef8ad9e86b9970b9e4d15c76b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-in-azure-security-center"></a>Oprávnění v Azure Security Center

Azure Security Center používá [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md), který poskytuje [předdefinované role](../active-directory/role-based-access-built-in-roles.md) , lze přiřadit toousers, skupiny a služby v Azure.

Security Center vyhodnocuje hello konfiguraci problémy se zabezpečením tooidentify prostředky a ohrožení zabezpečení. V Centru zabezpečení, uvidíte jenom informace související s tooa prostředků, když jsou přiřazeny hello roli vlastník, Přispěvatel nebo Čtenář pro předplatné nebo prostředek skupiny hello, která daný prostředek patří.

Přidání toothese role existují dvě konkrétní role Security Center:

* **Zabezpečení čtečky**: uživatel, který patří toothis role má zobrazení Center tooSecurity práva. uživatel Hello můžete zobrazit doporučení, výstrahy, zásady zabezpečení a stavy zabezpečení, ale nelze provádět změny.
* **Správce zabezpečení**: uživatel, který patří toothis role má hello stejná práva jako hello čtečky zabezpečení a můžete také aktualizovat zásady zabezpečení hello a zavření výstrah a doporučení.

> [!NOTE]
> role zabezpečení Hello čtečky zabezpečení a správce zabezpečení, mají přístup pouze ve službě Security Center. role zabezpečení Hello nemají přístup tooother služby oblasti Azure, jako je například úložiště, webové a mobilní telefon nebo Internet věcí.
>
>

## <a name="roles-and-allowed-actions"></a>Role a povolených akcí

Hello následující tabulka zobrazuje role a povolené akce v Centru zabezpečení. X označuje, že je hello akci povolit pro tuto roli.

| Role | Upravit zásady zabezpečení | Použít doporučení zabezpečení pro prostředek | Zavření výstrahy a doporučení | Zobrazit výstrahy a doporučení |
|:--- |:---:|:---:|:---:|:---:|
| Vlastník předplatného | X | X | X | X |
| Předplatné přispěvatele | X | X | X | X |
| Vlastník skupiny prostředků | -- | X | -- | X |
| Skupina prostředků přispěvatele | -- | X | -- | X |
| Čtenář | -- | -- | -- | X |
| Správce zabezpečení. | X | -- | X | X |
| Čtečka zabezpečení | -- | -- | -- | X |

> [!NOTE]
> Doporučujeme, abyste přiřadili hello omezenou roli potřebné pro uživatele toocomplete úkoly. Například přiřadíte hello čtečky role toousers, kdo stačí tooview informace o stavu zabezpečení hello prostředku, ale nemusí provádět žádné akce, třeba uplatňovat doporučení nebo upravovat zásady.
>
>

## <a name="next-steps"></a>Další kroky
Tento článek vysvětlení, jak Security Center používá toousers oprávnění RBAC tooassign a identifikovat hello povolené akce pro každou roli. Teď, když jste obeznámeni s přiřazení rolí hello potřeby toomonitor hello stavu zabezpečení vašeho předplatného, upravit zásady zabezpečení a použijete doporučení, zjistěte, jak:

- [Nastavení zásad zabezpečení v Centru zabezpečení](security-center-policies.md)
- [Správa doporučení zabezpečení v Centru zabezpečení](security-center-recommendations.md)
- [Sledování stavu zabezpečení hello vašich prostředků Azure](security-center-monitoring.md)
- [Spravovat a reagovat toosecurity výstrah v Security Center](security-center-managing-and-responding-alerts.md)
- [Sledování partnerských řešení zabezpečení](security-center-partner-solutions.md)
