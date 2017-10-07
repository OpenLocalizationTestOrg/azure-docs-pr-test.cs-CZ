---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity - stanovení požadavků na incidentu rResponse | Microsoft Docs"
description: "Určení schopnosti sledování a hlášení pro hello řešení hybridní identity, které můžete využít IT tootake akce tooidentify a zmírnit potenciální hrozby"
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: a3d2a459-599b-4b67-8e51-7369ee25082d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 7084096f318ef461e8331fb6edde1b77d4108466
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Stanovení požadavků na reakce na incidenty pro vaše řešení hybridní identity
Střední a velké organizace s největší pravděpodobností bude mít [reakcí na incidenty zabezpečení](https://technet.microsoft.com/library/cc700825.aspx) v místě toohelp IT provést akce odpovídajícím způsobem toohello úroveň incident. systém správy identit Hello je důležitou součástí v procesu hello reakcí na incidenty, protože může být použité toohelp identifikace, kdo provedl konkrétní akce hello cíli. Hello hybridní řešení identit musí být schopný tooprovide monitorování a vytváření sestav funkce, které můžete využít IT tootake akce tooidentify a zmírnit potenciální ohrožení. V plánu reakcí na incidenty typické budete mít hello následující fáze jako součást plánu hello:

1. Počáteční hodnocení.
2. Incidentu komunikaci.
3. Ovládací prvek poškození a snížení rizika.
4. Identifikace co bylo ohrožení zabezpečení a závažnosti.
5. Zachování důkaz.
6. Oznámení tooappropriate strany.
7. Obnovení systému.
8. Dokumentace.
9. Poškození a náklady na hodnocení.
10. Proces a plán revize.

Při identifikaci hello co IT oddělení byl ohrožení zabezpečení a závažnost fáze, bude nutné tooidentify hello systémy, které ohrožený, soubory, které mají přístup k a určení hello citlivosti těchto souborů. Hybridní identity systému by měl být schopný toofulfill tooassist tyto požadavky je identifikace hello uživatele, který tyto změny. 

## <a name="monitoring-and-reporting"></a>Monitorování a vytváření sestav
Systém identit hello tolikrát, kolikrát může také pomoci v prvním posouzení fázi, hlavně v případě, že má integrovaný systém hello auditování a funkce vytváření sestav. Během prvního posouzení hello správce IT musí být schopný tooidentify podezřelou aktivitu, nebo systém hello by měl být schopný tootrigger, které se automaticky podle předem nakonfigurovaná úloh. Řada aktivit by to znamenat možný útok, ale chybně nakonfigurovaný systém v jiných případech může vést tooa počet falešně pozitivních zjištění systému zjišťování neoprávněných vniknutí. 

systém správy identity Hello by měl pomůže tooidentify správci IT a sestavy těchto podezřelých aktivit. Tyto technické požadavky lze splnit obvykle monitorování všech systémů a nutnosti vykazovací funkci, můžete upozorňující na potenciální hrozby. Použít hello otázky níže toohelp návrhu hybridní řešení identit při vezme v úvahu reakcí na incidenty požadavky:

* Má vaše společnost reakce na incidenty zabezpečení v místě?
  * Pokud ano, hello aktuální systém správy identit slouží jako součást procesu hello?
* Potřebuje vaše společnost tooidentify podezřelé pokusů o přihlášení od uživatelů na různých zařízeních?
* Potřebuje vaše společnost toodetect potenciální ohrožení zabezpečení přihlašovacích údajů uživatele?
* Potřebuje vaše společnost přístup a akce tooaudit uživatele?
* Potřebuje vaše společnost tooknow při resetovat heslo uživatele?

## <a name="policy-enforcement"></a>Vynucení zásad
Během kontroly poškození a snížení rizika – fáze je důležité tooquickly snížit hello skutečné a potenciální účinky útoku. Tato akce, který bude trvat v tomto okamžiku můžete provést hello rozdíl mezi menší a hlavní jeden. Hello přesná reakce bude záviset na vaší organizace a hello povaha hello útoku, který jste čelí. Pokud počáteční assessment hello dospělo k závěru, že došlo k ohrožení účet, budete potřebovat tooenforce zásad tooblock tento účet. To je pouze příklad, kdy bude systém správy identit hello využít. Použijte hello otázky níže toohelp návrhu hybridní řešení identit při vezměte v úvahu, jak zásady budou vynucovat tooreact tooan probíhající incidentu:

* Má vaše společnost nějaké zásady v místě tooblock uživatelé ze sítě hello přístup v případě potřeby?
  * Pokud ano, umožňuje hello aktuální řešení integraci s systém správy identit pro hybridní hello se probíhající tooadopt?
* Potřebuje vaše společnost tooenforce podmíněného přístupu pro uživatele, kteří jsou do karantény? 

> [!NOTE]
> Ujistěte se, že tootake poznámky u každé odpovědi a pochopit hello důvody, které hello odpovědí. [Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přenášeny po hello dostupných možností a výhod i nevýhod jednotlivých možností.  Po zodpovězení těchto otázek, které vyberete která možnost nejlépe vyhovuje vaší firmě potřebuje.
> 
> 

## <a name="next-steps"></a>Další kroky
[Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

