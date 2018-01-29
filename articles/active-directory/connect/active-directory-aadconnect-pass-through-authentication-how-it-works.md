---
title: "Azure AD Connect: Předávací ověřování – jak to funguje | Microsoft Docs"
description: "Tento článek popisuje, jak funguje Azure předávací ověřování služby Active Directory"
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: billmath
ms.openlocfilehash: eaa9995430833c0c087ed0d4044f6c41d254e3ff
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure předávací ověřování služby Active Directory: Technické podrobné informace
Tento článek je základní informace o službě Azure Active directory (Azure AD) předávací ověřování funguje. Přímý technické a informace o zabezpečení najdete v tématu [deep Dive informace o zabezpečení](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) článku.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Jak funguje Azure předávací ověřování služby Active Directory?

Když se uživatel pokusí přihlásit k aplikaci zabezpečené službou Azure AD, a pokud je povolené předávací ověřování u klienta, jsou provedeny následující kroky:

1. Uživatel se pokusí o přístup k aplikaci, například [Outlook Web App](https://outlook.office365.com/owa/).
2. Pokud již není přihlášený uživatel, bude uživatel přesměrován do služby Azure AD **přihlášení uživatele** stránky.
3. Uživatel zadá své uživatelské jméno a heslo na stránku pro přihlášení Azure AD a vybere **přihlášení** tlačítko.
4. Azure AD, obdrží požadavek na přihlášení, umístí uživatelského jména a hesla (šifrované pomocí veřejného klíče) ve frontě.
5. Agent ověřování místní načte uživatelské jméno a heslo šifrované z fronty. Všimněte si, že Agent nemá často dotazování na požadavky z fronty, ale načte požadavky přes předem zavedené trvalé připojení.
6. Agent dešifruje heslo pomocí jeho privátní klíč.
7. Agent ověří uživatelské jméno a heslo pro službu Active Directory pomocí standardních API systému Windows, který je podobný mechanismus pro jaké Active Directory Federation Services (AD FS) používá. Uživatelské jméno může být buď místní výchozí uživatelské jméno, obvykle `userPrincipalName`, nebo jiný atribut, které jsou nakonfigurované v Azure AD Connect (označované jako `Alternate ID`).
8. Místní řadič domény služby Active Directory (DC) vyhodnotí žádost a vrátí odpovídající odpověď (úspěch, chyba, platnost hesla nebo uzamčení uživatele) k agentovi.
9. Agent ověřování pak vrátí tuto odpověď zpět do Azure AD.
10. Azure AD vyhodnotí odpovědi a reaguje na uživatele podle potřeby. Například Azure AD buď okamžitě přihlásí uživatel nebo požadavky pro Azure Multi-Factor Authentication.
11. Pokud přihlášení uživatele je úspěšné, uživatel přístup k aplikaci.

Následující diagram znázorňuje všechny součásti a kroky:

![Předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Další postup
- [Aktuální omezení](active-directory-aadconnect-pass-through-authentication-current-limitations.md): Zjistěte, jaké postupy se podporují, a ty, které nejsou.
- [Rychlý Start](active-directory-aadconnect-pass-through-authentication-quick-start.md): zprovoznění na Azure AD předávací ověřování.
- [Inteligentní uzamčení](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): nakonfigurovat možnosti inteligentního uzamčení na vašeho klienta k ochraně uživatelské účty.
- [Nejčastější dotazy](active-directory-aadconnect-pass-through-authentication-faq.md): Vyhledejte odpovědi na nejčastější dotazy.
- [Řešení potíží s](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Přečtěte si o řešení běžných problémů s funkcí předávací ověřování.
- [Podrobné informace zabezpečení](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): podrobný technický informace o funkci předávací ověřování.
- [Azure AD bezproblémové SSO](active-directory-aadconnect-sso.md): Další informace o této funkci vzájemně doplňují.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): použijte fóru Azure Active Directory do souboru žádosti o nové funkce.

