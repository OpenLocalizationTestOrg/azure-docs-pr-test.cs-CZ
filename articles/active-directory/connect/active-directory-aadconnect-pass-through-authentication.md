---
title: "Azure AD Connect: Předávací ověřování | Microsoft Docs"
description: "Tento článek popisuje předávací ověřování Azure Active Directory (Azure AD) a jak umožňuje Azure AD přihlášení pomocí ověřování hesla uživatelů pro místní službu Active Directory."
services: active-directory
keywords: "Co je Azure AD Connect předávací ověřování, nainstalovat službu Active Directory, požadovaných součástí pro Azure AD, jednotné přihlašování, jednotné přihlašování"
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
ms.openlocfilehash: 56cfb2c4f4afc7d46c926a392ae6ec01e96224d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Přihlášení uživatele pomocí ověřování Azure Active Directory průchozí

## <a name="what-is-azure-active-directory-pass-through-authentication"></a>Co je Azure Active Directory předávací ověřování?

Předávací ověřování Azure Active Directory (Azure AD) umožňuje toosign vaši uživatelé v tooboth místní a cloudové aplikace pomocí hello stejnými hesly. Tato funkce poskytuje lepší zkušenosti – jeden menší tooremember heslo uživatele a snižuje náklady na IT helpdesk, protože jak tooforget méně pravděpodobné, že jsou vaši uživatelé toosign v. Když se uživatelé přihlašují pomocí služby Azure AD, ověří tato funkce hesla uživatelů přímo pro vaše místní službu Active Directory.

Tato funkce je příliš alternativu[synchronizaci hodnoty Hash hesel služby Azure AD](active-directory-aadconnectsync-implement-password-synchronization.md), který poskytuje hello stejné výhodou tooorganizations cloudové ověřování. Však zásady zabezpečení a dodržování předpisů v některých organizacích není povolit tyto organizace hesla uživatelů toosend, i v hash formuláře, mimo jejich interní hranice. Předávací ověřování je hello správné řešení pro tyto organizace.

![Azure AD předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

Předávací ověřování můžete kombinovat s hello [bezproblémové jednotné přihlašování](active-directory-aadconnect-sso.md) funkce. Tímto způsobem, když uživatelé přistupují aplikace na jejich podnikových počítačů uvnitř firemní sítě, nemusí tootype v jejich hesla toosign v.

>[!IMPORTANT]
>Předávací ověřování Azure AD je aktuálně ve verzi preview.

## <a name="key-benefits-of-using-azure-ad-pass-through-authentication"></a>Hlavní výhody použití Azure AD předávací ověřování

- *Vysoký výkon uživatele*
  - Uživatelé používají hello stejné toosign hesla do místní i cloudové aplikace.
  - Uživatelé stráví méně času rozhovoru toohello IT helpdesk, vyřešte problémy související s hesly.
  - Uživatelé mohou dokončit [hesla pomocí samoobslužné služby správy](../active-directory-passwords-overview.md) úloh v cloudu hello.
- *Snadno toodeploy & Spravovat*
  - Není nutné komplexní místní nasazení nebo konfiguraci sítě.
  - Vyžaduje právě toobe lightweight agent nainstalovaný místně.
  - Bez režie na správu. vylepšení a opravy chyb automaticky obdrží Hello agenta.
- *Zabezpečení*
  - Místních hesel nikdy ukládají v cloudu hello v žádný formulář.
  - Hello agent umožní pouze odchozí připojení z vaší sítě. V hraniční síti, označované také jako zóna DMZ proto není žádný agent hello tooinstall požadavek.
  - Chrání vaše uživatelské účty ve spolupráci s bezproblémově [zásady Azure AD podmíněného přístupu](../active-directory-conditional-access-azure-portal.md), včetně vícefaktorového ověřování (MFA) a nástrojem [filtrování útoky hrubou silou heslo](active-directory-aadconnect-pass-through-authentication-smart-lockout.md).
- *Vysoce dostupný*
  - Další agenty lze nainstalovat více místními servery tooprovide vysokou dostupnost žádostí o přihlášení.

## <a name="feature-highlights"></a>Označuje funkce

- Podporuje přihlášení uživatele do všech webových aplikací založené na prohlížeči a do klientské aplikace Microsoft Office, které používají [moderní ověřování](https://aka.ms/modernauthga).
- Uživatelská jména přihlášení může být buď hello místní výchozí uživatelské jméno (`userPrincipalName`) nebo jiný atribut, které jsou nakonfigurované v Azure AD Connect (označované jako `Alternate ID`).
- Funkce Hello bezproblémově pracuje s [podmíněného přístupu](../active-directory-conditional-access.md) funkce, jako je služba Multi-Factor Authentication (MFA) toohelp zabezpečit vaši uživatelé.
- Prostředí s více doménovými strukturami jsou podporovány, pokud existují vztahy důvěryhodnosti doménové struktury mezi doménovými strukturami vaší AD a v případě směrování přípon názvů je správně nakonfigurován.
- Je bezplatné funkce a nepotřebujete žádné placené edice Azure AD toouse ho.
- Může být povoleno prostřednictvím [Azure AD Connect](active-directory-aadconnect.md).
- Používá lightweight místní agent, který naslouchá a odpoví toopassword ověření žádosti.
- Instalace více agentů poskytuje vysokou dostupnost žádostí o přihlášení.
- Ho [chrání](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) vaše místní účty před hrubou vynutit heslo útoky v cloudu hello.

## <a name="next-steps"></a>Další kroky

- [**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.
- [**Aktuální omezení** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – tato funkce je aktuálně ve verzi preview. Zjistěte, jaké postupy se podporují, a ty, které nejsou.
- [**Podrobné technické informace** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -pochopit, jak tato funkce funguje.
- [**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi toofrequently dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.
- [**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
