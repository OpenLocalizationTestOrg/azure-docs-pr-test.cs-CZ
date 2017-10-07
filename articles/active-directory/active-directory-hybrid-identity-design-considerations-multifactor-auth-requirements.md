---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity - stanovit požadavky na služby Multi-Factor authentication"
description: "Pomocí podmíněného řízení přístupu Azure Active Directory kontroluje hello konkrétní podmínky, kterou vyberete při ověřování uživatele hello a před povolením přístupu toohello aplikace. Po splnění těchto podmínek hello uživatel ověří a povolená přístup toohello aplikace."
documentationcenter: 
services: active-directory
author: femila
manager: billmath
editor: 
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 49fa7b43772fb3a2d6664747477c60a34cddde2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Stanovení požadavků na službu Multi-Factor authentication pro vaše řešení hybridní identity
V tomto světě mobility s uživateli přístup k datům a aplikacím v hello cloudu a z jakéhokoli zařízení se stává zabezpečení tyto informace prvořadým.  Každý den je nové titulku o porušení zabezpečení.  I když není zaručeno, proti takové narušení, služba Multi-Factor authentication, poskytuje další úroveň zabezpečení toohelp brání tyto narušení.
Začít vyhodnocování požadavků organizace hello u služby Multi-Factor authentication. To znamená co je snažíme toosecure hello organizace.  Toto testování je důležité toodefine hello technické požadavky pro nastavení a povolení hello organizace uživatele u služby Multi-Factor authentication.

> [!NOTE]
> Pokud nejste obeznámeni s MFA a jakým způsobem, důrazně doporučujeme, abyste si přečetli hello článku [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) předchozí toocontinue čtení v této části.
> 
> 

Ujistěte se, že tooanswer hello následující:

* Aplikace Microsoft toosecure zkouší vaší společnosti? 
* Jak jsou publikovány těchto aplikací?
* Vaše společnost poskytuje vzdáleného přístupu tooallow zaměstnanci tooaccess místní aplikace?

Pokud ano, jaké typy vzdáleného přístupu? Budete také potřebovat tooevaluate, kde budou umístěné hello uživatelů, kteří používají tyto aplikace. Další důležitý krok toodefine hello správné služby Multi-Factor authentication strategie je toto testování. Ujistěte se, zda text hello tooanswer následující otázky:

* Kde se nachází uživatelé hello budete toobe?
* Může se být umístěna kdekoli?
* Vaše společnost chcete tooestablish omezení podle umístění uživatele toohello?

Jakmile porozumíte tyto požadavky, je důležité vyhodnotit tooalso hello uživatelských požadavků u služby Multi-Factor authentication. Toto testování je důležité, protože ji definovat hello požadavky pro nasazení služby Multi-Factor authentication. Ujistěte se, zda text hello tooanswer následující otázky:

* Znáte hello uživatelů pomocí služby Multi-Factor authentication?
* Budou některé používá požadované tooprovide další ověřování?  
  * Pokud ano, všechny hello čas, při pocházející z externí sítě nebo přístupem ke konkrétní aplikace, nebo v jiných podmínkách?
* Budou uživatelé hello vyžadovat školení na postupy toosetup a implementace služby Multi-Factor authentication?
* Jaké jsou hello klíčových scénářů, že vaše společnost chce tooenable služby Multi-Factor authentication pro své uživatele?

Po zodpovězení otázek předchozí hello, budete mít toounderstand Pokud jsou již implementovaná službu Multi-Factor authentication na místě. Toto testování je důležité toodefine hello technické požadavky pro nastavení a povolení hello organizace uživatele u služby Multi-Factor authentication. Ujistěte se, zda text hello tooanswer následující otázky:

* Potřebuje vaše společnost tooprotect privilegované účty s MFA?
* Potřebuje vaše společnost tooenable MFA pro určité aplikace kvůli dodržování předpisů?
* Potřebuje vaše společnost pro všechny oprávněné uživatele těchto aplikací nebo pouze správci tooenable MFA?
* Třeba máte vždy povolené ověřování MFA nebo pouze při protokolování hello uživatelé mimo podnikovou síť?

## <a name="next-steps"></a>Další kroky
[Definování strategie přijetí hybridní identity](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

