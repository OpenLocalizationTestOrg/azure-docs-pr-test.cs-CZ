---
title: "Řešení potíží s dynamické členství ve skupinách | Microsoft Docs"
description: "Tipy k řešení potíží pro dynamické členství ve skupinách v Azure AD."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 89bb04b6-a379-49c2-8465-fe386641816a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: 32947e8cc69c9a48d9a285bf0a37ab3398571f86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Řešení potíží s dynamickým členstvím ve skupinách
**I nakonfigurovali pravidlo na skupinu, ale aktualizovat žádné členství ve skupině**<br/>Ověřte, zda **povolit delegovaná Správa skupin** nastavení je **Ano** v **konfigurace** kartě. Toto nastavení se zobrazí jenom v případě, že jste přihlášeni jako uživatel, kterému je přiřazena licenci Azure Active Directory Premium. Ověřte hodnoty atributů uživatele v pravidle: existují uživatelé, kteří splňovat pravidla?

**I nakonfigurovali pravidlo, ale teď se odebrat členy existující pravidla**<br/>Toto je očekávané chování. Stávající členy skupiny se odeberou, když je pravidlem povolené nebo změnit. Uživatelé vrácená z vyhodnocení pravidla jsou přidány jako členy do skupiny.     

**Není zobrazen, že změní okamžitě při přidání nebo změna pravidla, proč ne?**<br/>Vyhodnocování členství vyhrazené se provádí pravidelně v procesu asynchronní pozadí. Jak dlouho trvá proces je určen podle počet uživatelů ve vašem adresáři a velikost skupiny vytvořeny v důsledku pravidlo. Obvykle adresáře s malý počet uživatelů se zobrazí změn členství ve skupinách za méně než několik minut. Adresáře s velký počet uživatelů, může trvat 30 minut nebo déle, než se naplnění.

### <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
