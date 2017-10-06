---
title: "Azure AD Connect: Předávací ověřování – jak to funguje? | Dokumentace Microsoftu"
description: "Tento článek popisuje, jak funguje Azure Active Directory předávací ověřování."
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: billmath
ms.openlocfilehash: ffcebee572a9ba2840e81250651dea45599d65d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure předávací ověřování služby Active Directory: Technické podrobné informace

>[!IMPORTANT]
>Předávací ověřování Azure AD je aktuálně ve verzi preview. 

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Jak funguje Azure předávací ověřování služby Active Directory?

Když se uživatel pokusí toosign do k aplikaci zabezpečené službou Azure Active Directory (Azure AD) a pokud je povolené ověřování průchozí na klienta hello hello provedeny následující kroky:

1. uživatel Hello pokusí tooaccess aplikace (například hello aplikaci Outlook Web App - https://outlook.office365.com/owa/).
2. Pokud již není přihlášený hello uživatele, uživatel hello je přesměrovaného toohello Azure AD přihlašovací stránky.
3. Hello uživatel zadá své uživatelské jméno a heslo do Azure AD hello přihlašovací stránku a klikne na tlačítko "Přihlásit" hello.
4. Azure AD, na příjem hello požádat o přihlášení, umístí hello uživatelského jména a hesla (šifrované pomocí veřejného klíče) ve frontě.
5. Předávací ověřování agenta místní vytvoří frontu toohello odchozí volání a načte hello uživatelské jméno a heslo šifrované.
6. Hello agent dešifruje hello heslo pomocí jeho privátní klíč.
7. Hello agent pak ověří hello uživatelské jméno a heslo pro službu Active Directory pomocí standardních API systému Windows (podobně jako mechanismus toowhat používá Active Directory Federation Services). Hello uživatelské jméno může být buď hello místní výchozí uživatelské jméno (obvykle `userPrincipalName`) nebo jiný atribut, které jsou nakonfigurované v Azure AD Connect (označované jako `Alternate ID`).
8. Hello místní řadiči domény Active Directory (DC) pak vyhodnotí hello požadavku, a vrátí hello odpovídající odpověď (úspěch, chyba, platnost hesla nebo uzamčení uživatele) toohello agenta.
9. Hello agenta, pak vrátí zpět tooAzure AD této odpovědi.
10. Azure AD vyhodnotí hello odpovědi a odpoví toohello uživatele podle potřeby – například nebude moci okamžitě přihlásí uživatel hello nebo požadavky pro multi-Factor Authentication (MFA).
11. Pokud přihlášení uživatele hello je úspěšné, uživatel hello je možné tooaccess hello aplikace.

Hello následující diagram znázorňuje všechny komponenty hello a hello kroky.

![Předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Další kroky
- [**Aktuální omezení** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – tato funkce je aktuálně ve verzi preview. Zjistěte, jaké postupy se podporují, a ty, které nejsou.
- [**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.
- [**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi toofrequently dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.
- [**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
