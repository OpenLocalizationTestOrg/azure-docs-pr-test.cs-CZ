---
title: "aaaSecuring privilegovaného přístupu ve službě Azure AD | Microsoft Docs"
description: "Téma, které vysvětluje hello blíží k zabezpečení privilegovaného přístupu v Azure, Azure Active Directory a služeb Microsoft Online Services."
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
ms.openlocfilehash: 694835dc5c41640673dbd996d44b0d1f217220de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-privileged-access-in-azure-ad"></a>Zabezpečení privilegovaného přístupu ve službě Azure AD
Zabezpečení privilegovaného přístupu je důležitým prvním krokem toohelp chránit prostředky obchodní moderní organizace. Privilegované účty jsou účty, které správu a spravovat systémy IT. Internetový útočník cílové tyto účty toogain přístup tooan organizace dat a systémy. toosecure privilegovaný přístup, musí izolovat hello účty a systémy ze hello riziko přičemž viditelné tooa uživatel se zlými úmysly.

Další uživatelé spouštějí tooget privilegovaný přístup prostřednictvím cloudových služeb. To může zahrnovat globální správce Office 365, správci předplatného Azure a uživatelé, kteří mají přístup pro správu ve virtuálních počítačích nebo na aplikace SaaS.

Společnost Microsoft doporučuje podle tento plán [zabezpečení privilegovaného přístupu](https://technet.microsoft.com/library/mt631194.aspx).

Tyto zásady se použijí pro zákazníky pomocí Azure Active Directory, Office 365 nebo jiné služby společnosti Microsoft a aplikace, zda jsou uživatelské účty spravovat a ověřovat pomocí služby Active Directory nebo Azure Active Directory. Hello následující části poskytují další informace o Azure AD toosupport funkce zabezpečení privilegovaného přístupu.

## <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication
zabezpečení hello tooincrease Správce ověřování, byste měli vyžadovat dvoustupňové ověření před udělením oprávnění. Dvoustupňové ověření je metoda ověřování, který jste si, že vyžaduje použití hello víc věcí než jenom uživatelské jméno a heslo. Poskytuje druhou vrstvu zabezpečení toouser přihlášení a transakce.

Azure Multi-Factor Authentication (MFA) se společnosti Microsoft dvoustupňové ověření řešení, která pomáhá zabezpečit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení. Zajišťuje silné ověřování přes celou řadu možností snadno ověření, včetně:

- telefonní hovory
- Textové zprávy
- oznámení mobilní aplikace
- Kódy ověřování mobilní aplikace
- Tokeny OATH třetích stran

Přehled o tom, jak funguje Azure Multi-Factor Authentication najdete hello následující video:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]

Další informace najdete v tématu [MFA pro Office 365 a Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Časově vázaných oprávnění
Některé organizace zjistit, zda mají příliš mnoho uživatelů ve vysoce privilegované role. Uživatel byla pravděpodobně přidána toohello role pro konkrétní aktivity, jako je toosign pro služby, ale často následně nepoužili těchto oprávnění.

Délka expozice hello toolower oprávnění a zvýšit vaši přehled o jejich používání tooonly limit uživatele s ohledem na jejich oprávnění "právě v čase" (JIT), když potřebují tooperform úlohu. Pro Azure Active Directory a služeb Microsoft Online Services, můžete použít [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).

![Řídicí panel PIM][2]

## <a name="attack-detection"></a>Detekce útoku
[Azure Active Directory Identity Protection](../active-directory-identityprotection.md) poskytuje ucelený přehled o rizikových událostech a potenciální ohrožení zabezpečení, které ovlivňují identity ve vaší organizaci. Na základě riziko událostí, Identity Protection vypočítá úroveň rizika uživatele pro každého uživatele, umožňuje na základě riziko zásady tooconfigure tooautomatically chránit hello identity vaší organizace. Tyto zásady, spolu s další řízení podmíněného přístupu poskytuje Azure Active Directory a EMS, můžete automaticky blokovat hello uživatele nebo rámci návrhů, které zahrnují resetování hesel a vynucení služby Multi-Factor authentication.

![Azure AD Identity Protection][3]

## <a name="conditional-access"></a>Podmíněný přístup
Pomocí podmíněného řízení přístupu Azure Active Directory kontroluje hello konkrétních podmínek, který si zvolíte při ověřování uživatele, před povolením přístupu tooan aplikace. Po splnění těchto podmínek hello uživatel ověří a povolená přístup toohello aplikace.

![Nastavení pravidla podmíněného přístupu pomocí vícefaktorového ověřování][4]

## <a name="related-articles"></a>Související články
* Povolit [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Povolit [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)
* Povolit [ochrany identit Azure AD](../active-directory-identityprotection.md)
* Povolit [řízení podmíněného přístupu](../active-directory-conditional-access.md)

Další informace o vytváření plán dokončení zabezpečení, najdete v části hello "odpovědnosti zákazníka a plán" části hello [Microsoft Cloud Security Enterprise architekty](http://aka.ms/securecustomer) dokumentu. Další informace o zapojení tooassist služby společnosti Microsoft s žádným z těchto témat, obraťte se na zástupce společnosti Microsoft nebo navštivte naše [počítačové bezpečnosti řešení stránky](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
