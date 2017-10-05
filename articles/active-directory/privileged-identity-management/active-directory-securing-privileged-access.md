---
title: "Zabezpečení privilegovaného přístupu ve službě Azure AD | Microsoft Docs"
description: "Téma, které popisuje přístupy k zabezpečení privilegovaného přístupu v Azure, Azure Active Directory a služeb Microsoft Online Services."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: mwahl
ms.assetid: 235a0ce9-1daf-4433-8f65-9c6afcd64d08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: acd1c11cecfa37f607fe5d82a76a40647093f43f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="securing-privileged-access-in-azure-ad"></a>Zabezpečení privilegovaného přístupu ve službě Azure AD
Zabezpečení privilegovaného přístupu je důležitým prvním krokem k ochraně obchodní prostředky v moderní organizaci. Privilegované účty jsou účty, které správu a spravovat systémy IT. Internetový útočník cíle tyto účty pro přístup k datům a systémy organizace. K zabezpečení privilegovaného přístupu, měli izolovat účty a systémy ze riziko napadení se zlými úmysly.

Další uživatelé spouštějí získat privilegovaný přístup prostřednictvím cloudové služby. To může zahrnovat globální správce Office 365, správci předplatného Azure a uživatelé, kteří mají přístup pro správu ve virtuálních počítačích nebo na aplikace SaaS.

Společnost Microsoft doporučuje podle tento plán [zabezpečení privilegovaného přístupu](https://technet.microsoft.com/library/mt631194.aspx).

Tyto zásady se použijí pro zákazníky pomocí Azure Active Directory, Office 365 nebo jiné služby společnosti Microsoft a aplikace, zda jsou uživatelské účty spravovat a ověřovat pomocí služby Active Directory nebo Azure Active Directory. Následující části obsahují další informace o funkcích služby Azure AD pro podporu zabezpečení privilegovaného přístupu.

## <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication
Pokud chcete zvýšit zabezpečení ověření správce, byste měli vyžadovat dvoustupňové ověření před udělením oprávnění. Dvoustupňové ověření je metoda ověřování, který jste si, že vyžaduje použití víc věcí než jenom uživatelské jméno a heslo. Poskytuje druhou vrstvu zabezpečení uživatelská přihlášení a transakce.

Azure Multi-Factor Authentication (MFA) se společnosti Microsoft dvoustupňové ověření řešení, které pomáhá chránit přístup k datům a aplikacím zároveň pokrývat uživatele potřebují pro jednoduchý proces přihlášení. Zajišťuje silné ověřování přes celou řadu možností snadno ověření, včetně:

- telefonní hovory
- Textové zprávy
- oznámení mobilní aplikace
- Kódy ověřování mobilní aplikace
- Tokeny OATH třetích stran

Přehled o tom, jak funguje Azure Multi-Factor Authentication najdete v následujícím videu:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]

Další informace najdete v tématu [MFA pro Office 365 a Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Časově vázaných oprávnění
Některé organizace zjistit, zda mají příliš mnoho uživatelů ve vysoce privilegované role. Uživatel byla pravděpodobně přidána do rolí pro konkrétní aktivitu, jako je zápis pro služby, ale často následně nepoužili těchto oprávnění.

Pokud chcete snížit čas ohrožení oprávnění a zvýšit vaši přehled o jejich používání, omezení uživatelů na pouze s ohledem na jejich oprávnění "právě v čase" (JIT), když potřebují k provedení úkolu. Pro Azure Active Directory a služeb Microsoft Online Services, můžete použít [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).

![Řídicí panel PIM][2]

## <a name="attack-detection"></a>Detekce útoku
[Azure Active Directory Identity Protection](../active-directory-identityprotection.md) poskytuje ucelený přehled o rizikových událostech a potenciální ohrožení zabezpečení, které ovlivňují identity ve vaší organizaci. Na základě riziko událostí, vypočítá ochrany identit úroveň rizika uživatele pro každého uživatele, což umožňuje konfigurovat zásady na základě riziko k automatické ochraně identity vaší organizace. Tyto zásady, spolu s další řízení podmíněného přístupu poskytuje Azure Active Directory a EMS, můžete automaticky blokovat uživateli nebo rámci návrhů, které zahrnují resetování hesel a vynucení služby Multi-Factor authentication.

![Azure AD Identity Protection][3]

## <a name="conditional-access"></a>Podmíněný přístup
Pomocí podmíněného řízení přístupu Azure Active Directory kontroluje konkrétních podmínek, které zvolíte při ověřování uživatele, před povolením přístupu k aplikaci. Po splnění těchto podmínek je uživatel ověřený a přistupovat k aplikaci.

![Nastavení pravidla podmíněného přístupu pomocí vícefaktorového ověřování][4]

## <a name="related-articles"></a>Související články
* Povolit [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Povolit [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)
* Povolit [ochrany identit Azure AD](../active-directory-identityprotection.md)
* Povolit [řízení podmíněného přístupu](../active-directory-conditional-access.md)

Další informace o vytváření plán dokončení zabezpečení, najdete v části "odpovědnosti zákazníka a plán" z [Microsoft Cloud Security Enterprise architekty](http://aka.ms/securecustomer) dokumentu. Další informace o zapojení služby společnosti Microsoft, které pomáhají s žádným z těchto témat, obraťte se na zástupce společnosti Microsoft nebo navštivte naše [počítačové bezpečnosti řešení stránky](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
