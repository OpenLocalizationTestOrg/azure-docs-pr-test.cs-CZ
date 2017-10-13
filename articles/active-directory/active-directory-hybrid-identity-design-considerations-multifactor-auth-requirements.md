---
title: "Azure Active Directory hybridní identity aspekty návrhu - stanovit požadavky na služby Multi-Factor authentication"
description: "Azure Active Directory pomocí podmíněného řízení přístupu, zkontroluje konkrétní podmínky, kterou vyberete při ověřování uživatele a před povolením přístupu k aplikaci. Po splnění těchto podmínek je uživatel ověřený a přistupovat k aplikaci."
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
ms.openlocfilehash: 5b3a8ce6e4203dfb3700f324e32687dd910118af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Stanovení požadavků na službu Multi-Factor authentication pro vaše řešení hybridní identity
V tomto světě mobility s uživateli přístup k datům a aplikacím v cloudu a z jakéhokoli zařízení se stává zabezpečení tyto informace prvořadým.  Každý den je nové titulku o porušení zabezpečení.  I když není zaručeno, proti takové narušení, služba Multi-Factor authentication, poskytuje další úroveň zabezpečení, aby se zabránilo tyto narušení.
Začít vyhodnocování požadavků organizace pro službu Multi-Factor authentication. To znamená co je organizace pokusu o zabezpečení.  Toto testování je důležité určit technické požadavky pro nastavení a povolení organizace uživatele u služby Multi-Factor authentication.

> [!NOTE]
> Pokud nejste obeznámeni s MFA a jakým způsobem, důrazně doporučujeme, abyste si přečetli článek [co je Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) před pokračujte ve čtení v této části.
> 
> 

Nezapomeňte odpovědět následující:

* Vaše společnost pokouší zabezpečit aplikace společnosti Microsoft? 
* Jak jsou publikovány těchto aplikací?
* Poskytuje vaše společnost vzdáleným zaměstnancům přístup k místní aplikace?

Pokud ano, jaké typy vzdáleného přístupu? Budete také muset vyhodnotit, kde budou umístěné uživatelů, kteří používají tyto aplikace. Toto testování je jiný důležitým krokem při definování strategie správné služby Multi-Factor authentication. Ujistěte se, že odpovězte si na následující otázky:

* Kde budou uživatelé k nalezen?
* Může se být umístěna kdekoli?
* Vaše společnost chce vytvořit omezení podle umístění uživatele?

Jakmile porozumíte tyto požadavky, je potřeba také měli zvážit požadavky na uživatele služby Multi-Factor Authentication. Toto testování je důležité, protože ho definují požadavky na nasazení služby Multi-Factor authentication. Ujistěte se, že odpovězte si na následující otázky:

* Uživatelé se seznámíte s službou Multi-Factor authentication?
* Některé používá, se budou muset poskytnout další ověřování?  
  * Pokud ano, vždy, když pocházející z externí sítě nebo přístupem ke konkrétní aplikace, nebo v jiných podmínkách?
* Uživatelé budou vyžadovat školení o tom, jak nastavit a implementovat vícefaktorové ověřování?
* Jaké jsou klíčové scénáře, které chce vaše společnost povolit službu Multi-Factor authentication pro své uživatele?

Po zodpovězení otázek předchozí, budete moct porozumění, zda jsou již implementovaná službu Multi-Factor authentication na místě. Toto testování je důležité určit technické požadavky pro nastavení a povolení organizace uživatele u služby Multi-Factor authentication. Ujistěte se, že odpovězte si na následující otázky:

* Potřebuje vaše společnost pro ochranu privilegovaných účtů s MFA?
* Potřebuje vaše společnost povolit MFA pro určité aplikace kvůli dodržování předpisů?
* Potřebuje vaše společnost povolit MFA pro všechny oprávněné uživatele těchto aplikací nebo pouze správci?
* Třeba máte vždy povolené ověřování MFA nebo pouze při přihlášení uživatele mimo podnikovou síť?

## <a name="next-steps"></a>Další kroky
[Definování strategie přijetí hybridní identity](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

