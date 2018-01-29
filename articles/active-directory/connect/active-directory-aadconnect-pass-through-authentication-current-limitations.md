---
title: "Azure AD Connect: Předávací ověřování – aktuální omezení | Microsoft Docs"
description: "Tento článek popisuje aktuální omezení předávací ověřování Azure Active Directory (Azure AD)"
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
ms.date: 01/09/2018
ms.author: billmath
ms.openlocfilehash: aee90f278476a899e0d47fc572c4f375bf926de2
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure předávací ověřování služby Active Directory: Aktuální omezení

>[!IMPORTANT]
>Předávací ověřování Azure Active Directory (Azure AD) je bezplatná funkce a nepotřebujete žádné placené edice Azure AD pro použití. Předávací ověřování je k dispozici v celém světě instanci Azure AD a ne v pouze [cloudu Microsoft Azure v Německu](http://www.microsoft.de/cloud-deutschland) nebo [cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Podporované scénáře

Plně podporuje následující scénáře:

- Přihlášení uživatele na všechny webové aplikace založené na prohlížeči
- Uživatelská přihlášení do aplikace Office, které podporují [moderní ověřování](https://aka.ms/modernauthga): Office 2016 a Office 2013 _s_ moderní ověřování
- Přihlášení uživatele ke Skypu pro firmy moderní ověřování této podpory, včetně Online a hybridní topologie. Další informace o podporovaných topologiích [zde](https://technet.microsoft.com/library/mt803262.aspx).
- Azure AD domain spojení pro zařízení s Windows 10
- Podporu protokolu Exchange ActiveSync

## <a name="unsupported-scenarios"></a>Nepodporované scénáře

Následující scénáře jsou _není_ podporovány:

- Přihlášení uživatele pro starší klientské aplikace Office: Office 2010 a Office 2013 _bez_ moderní ověřování. Organizace doporučujeme přepnout na moderní ověřování, pokud je to možné. Moderní ověřování umožňuje podporu předávací ověřování. Také pomáhá vám zabezpečit vaše uživatelské účty pomocí [podmíněného přístupu](../active-directory-conditional-access-azure-portal.md) funkce, jako je Azure Multi-Factor Authentication.
- Uživatelská přihlášení ke Skypu pro firmy klientské aplikace _bez_ moderní ověřování.
- Přihlášení uživatele k prostředí PowerShell, verze 1.0. Doporučujeme použít PowerShell verze 2.0.
- Hesla aplikací pro službu Multi-Factor Authentication.
- Zjišťování uživatelů s [úniku přihlašovacích údajů](../active-directory-reporting-risk-events.md#leaked-credentials).
- Azure AD Domain Services vyžaduje synchronizaci hodnoty Hash hesla, aby byl povolen u klienta. Proto klientů, které používají předávací ověřování _pouze_ nefungují pro scénáře, které je třeba Azure AD Domain Services.
- Předávací ověřování není integrovaná s [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md).
- Apple Device Enrollment Program (Apple DEP) nepodporuje moderní ověřování.  Zařízení Apple DEP se nepovede zaregistrovat do služby Intune pro domény pomocí předávacího ověřování.

>[!IMPORTANT]
>Jako alternativní řešení pro nepodporované scénáře _pouze_, povolte synchronizaci hodnoty Hash hesla na [volitelné funkce](active-directory-aadconnect-get-started-custom.md#optional-features) stránku průvodce Azure AD Connect.

>[!NOTE]
Povolení synchronizaci hodnoty hash hesla nabízí možnost převzetí služeb při selhání ověřování Pokud dojde k narušení na místní infrastrukturu. Toto převzetí služeb při selhání z předávací ověřování synchronizaci hodnoty hash hesla služby Active Directory není automatické. Budete potřebovat přepnout metoda přihlašování ručně pomocí služby Azure AD Connect. Pokud server se službou Azure AD Connect přestane fungovat, budete vyžadovat pomoc od společnosti Microsoft Support vypnout předávací ověřování.

## <a name="next-steps"></a>Další postup
- [Rychlý start](active-directory-aadconnect-pass-through-authentication-quick-start.md): zprovoznění s Azure AD předávací ověřování.
- [Inteligentní uzamčení](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): Zjistěte, jak nakonfigurovat možnosti inteligentního uzamčení na vašeho klienta k ochraně uživatelské účty.
- [Podrobné technické informace](active-directory-aadconnect-pass-through-authentication-how-it-works.md): pochopit, jak funguje funkci předávací ověřování.
- [Nejčastější dotazy](active-directory-aadconnect-pass-through-authentication-faq.md): Vyhledejte odpovědi na nejčastější dotazy o funkci předávací ověřování.
- [Řešení potíží s](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Přečtěte si o řešení běžných problémů s funkcí předávací ověřování.
- [Podrobné informace zabezpečení](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): podrobný technický informace o funkci předávací ověřování.
- [Azure AD bezproblémové SSO](active-directory-aadconnect-sso.md): Další informace o této funkci vzájemně doplňují.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): použijte fóru Azure Active Directory do souboru žádosti o nové funkce.

