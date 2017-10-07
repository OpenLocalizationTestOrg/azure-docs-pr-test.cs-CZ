---
title: "Azure AD Connect: Předávací ověřování – aktuální omezení | Microsoft Docs"
description: "Tento článek popisuje aktuální omezení hello předávací ověřování Azure Active Directory (Azure AD)."
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
ms.date: 08/03/2017
ms.author: billmath
ms.openlocfilehash: 2933745d071aae205c44659e6ea92697f390effb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure předávací ověřování služby Active Directory: Aktuální omezení

>[!IMPORTANT]
>Předávací ověřování Azure AD je aktuálně ve verzi preview. Je bezplatné funkce a nepotřebujete žádné placené edice Azure AD toouse ho. Předávací ověřování je dostupný jenom v hello celosvětové instance služby Azure AD a ne v [Microsoft Cloud Německo](http://www.microsoft.de/cloud-deutschland) a [cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Podporované scénáře

Hello následující scénáře jsou plně podporované verzi Preview:

- Přihlášení uživatele do všech webových aplikací založené na prohlížeči.
- Uživatelská přihlášení do služeb Office 365 klientské aplikace, které podporují [moderní ověřování](https://aka.ms/modernauthga).
- Azure AD Join pro zařízení s Windows 10.
- Podpora protokolu Exchange ActiveSync.

## <a name="unsupported-scenarios"></a>Nepodporované scénáře

Hello následující scénáře jsou _není_ během preview podporovány:

- Přihlášení uživatele do klientské aplikace starší verze systému Office (Office 2013 nebo starší). Organizace jsou podporovali tooswitch toomodern ověřování, pokud je to možné. Moderní ověřování umožňuje podporu předávací ověřování, ale také pomáhá se zabezpečením vašich uživatelských účtů pomocí [podmíněného přístupu](../active-directory-conditional-access.md) funkce jako je například služba Multi-Factor Authentication (MFA).
- Uživatelská přihlášení do Skype pro firmy klientské aplikace, včetně Skype pro firmy 2016.
- Přihlášení uživatele do prostředí PowerShell 1.0. Doporučuje se místo toho použít PowerShell v2.0.

>[!IMPORTANT]
>Jako řešení pro nepodporované scénáře, povolte synchronizaci hodnoty Hash hesla na hello [volitelné funkce](active-directory-aadconnect-get-started-custom.md#optional-features) stránky v Průvodci hello Azure AD Connect. Synchronizaci hodnoty Hash hesla funguje jako nouzové řešení pro hello předcházející scénáře _pouze_ (a _není_ jako obecný záložní tooPass prostřednictvím ověřování). Pokud nepotřebujete tyto scénáře, vypněte synchronizaci hodnoty Hash hesla.

## <a name="next-steps"></a>Další kroky
- [**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.
- [**Podrobné technické informace** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -pochopit, jak tato funkce funguje.
- [**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi toofrequently dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.
- [**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
