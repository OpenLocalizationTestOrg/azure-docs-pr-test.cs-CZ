---
title: "aaaTroubleshooting dynamické členství ve skupinách | Microsoft Docs"
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
ms.openlocfilehash: d792fc406288844e2c5dbe3702c2c9870d09473e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Řešení potíží s dynamickým členstvím ve skupinách
**I nakonfigurovali pravidlo na skupinu, ale žádné členství ve skupině hello aktualizovat**<br/>Ověřte, že hello **povolit delegovaná Správa skupin** nastavení je příliš**Ano** v hello **konfigurace** kartě. Toto nastavení se zobrazí jenom v případě, že jste přihlášeni jako uživatel toowhom licenci Azure Active Directory Premium je přiřazen. Ověřte hodnoty hello atributů uživatele na pravidlo hello: existují uživatelé, kteří odpovídají hello pravidlo?

**I nakonfigurovali pravidlo, ale teď se odeberou hello stávající členy pravidlo hello**<br/>Toto je očekávané chování. Stávající členy skupiny hello se odeberou, když je pravidlem povolené nebo změnit. Uživatelé Hello vrácená z vyhodnocení hello pravidla jsou přidány jako členové skupiny toohello.     

**Není zobrazen, že změní okamžitě při přidání nebo změna pravidla, proč ne?**<br/>Vyhodnocování členství vyhrazené se provádí pravidelně v procesu asynchronní pozadí. Jak dlouho trvá proces hello je určen podle hello počet uživatelů v velikost vašeho adresáře a hello hello skupiny vytvořeny v důsledku hello pravidlo. Obvykle adresáře s malý počet uživatelů se zobrazí změn členství ve skupinách hello za méně než několik minut. Adresáře s velký počet uživatelů, může trvat 30 minut nebo déle toopopulate.

### <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu tooresources pomocí skupin Azure Active Directory](active-directory-manage-groups.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
