---
title: "aaaAzure Active Directory Identity Protection - jak uživatelé toounblock | Microsoft Docs"
description: "Zjistěte, jak odblokovat uživatele, které byly blokována zásady služby Azure Active Directory Identity Protection."
services: active-directory
keywords: "ochrany identit Azure active directory, odblokovat uživatele"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: cdda2808822888f76aa75cf46478738c94df51a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection---how-toounblock-users"></a>Azure Active Directory identitu ochrana – jak toounblock uživatelů
S Azure Active Directory Identity Protection můžete nakonfigurovat zásady, které uživatelé tooblock Pokud hello nakonfigurované podmínky jsou splněny. Obvykle blokovaný uživatel kontakty nápovědy podpory toobecome odblokováno. Tato témata vysvětluje hello kroky můžete provádět toounblock blokovaný uživatel.

## <a name="determine-hello-reason-for-blocking"></a>Určení hello důvod pro blokování
Jako první krok toounblock uživatele musíte toodetermine hello typ zásad, který má blokovaný hello uživatele, protože na něm závisí další kroky.
S Azure Active Directory Identity Protection může se uživatel buď blokovaný přihlášení riziko zásadu nebo zásady riziko uživatele.

Můžete získat hello typ zásad, který má blokovaný uživatel z hello záhlaví v dialogovém okně hello, který byl předložený toohello uživatele při pokusu o přihlášení:

| Zásada | Dialogové okno uživatele |
| --- | --- |
| Riziko přihlášení |![Blokované přihlášení](./media/active-directory-identityprotection-unblock-howto/02.png) |
| Riziko pro uživatele |![Zablokovaný účet](./media/active-directory-identityprotection-unblock-howto/104.png) |

Uživatel, který je blokována:

* Zásady přihlášení riziko je také označován jako podezřelou přihlášení
* Zásady uživatele riziko je také označován jako účet v ohrožení

## <a name="unblocking-suspicious-sign-ins"></a>Odblokování podezřelé přihlášení
toounblock podezřelé přihlášení, máte následující možnosti hello:

1. **Přihlášení ze zařízení nebo známé umístění** -běžným důvodem pro blokované podezřelé přihlášení jsou pokusů o přihlášení z neznámých míst nebo zařízení. Vaši uživatelé můžete rychle zjistit, zda je hello Důvod blokování tak, že zkusíte toosign-in ze známé umístění nebo zařízení.
2. **Vyloučit ze zásad** – Pokud se domníváte, že hello aktuální konfiguraci zásad přihlášení způsobuje problémy pro konkrétní uživatele, můžete vyloučit hello uživatelé z něj. V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.
3. **Zakažte zásadu** – Pokud se domníváte, že vaše konfigurace zásad způsobuje problémy pro všechny uživatele, můžete zakázat hello zásad. V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.

## <a name="unblocking-accounts-at-risk"></a>Odblokování účty v ohrožení
toounblock na účtu ohrožený, máte následující možnosti hello:

1. **Resetovat heslo** -resetujete heslo uživatele hello. V tématu [ruční zabezpečené heslo resetovat](active-directory-identityprotection.md#manual-secure-password-reset) další podrobnosti.
2. **Zavřít všechny události riziko** – Pokud bylo dosaženo úroveň rizika uživatele hello nakonfigurované pro blokování přístupu blokuje hello uživatele riziko zásad uživatele. Můžete snížit uživatele je úroveň rizika ručně ukončením hlášené rizikových událostech. Další podrobnosti najdete v tématu [zavření rizikových událostí ručně](active-directory-identityprotection.md#closing-risk-events-manually).
3. **Vyloučit ze zásad** – Pokud se domníváte, že hello aktuální konfiguraci zásad přihlášení způsobuje problémy pro konkrétní uživatele, můžete vyloučit hello uživatelé z něj. V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.
4. **Zakažte zásadu** – Pokud se domníváte, že vaše konfigurace zásad způsobuje problémy pro všechny uživatele, můžete zakázat hello zásad. V tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md) další podrobnosti.

## <a name="next-steps"></a>Další kroky
 Chcete, aby tooknow Další informace o Azure AD Identity Protection? Podívejte se na [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
