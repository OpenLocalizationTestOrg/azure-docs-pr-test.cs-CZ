---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity – určení požadavků na ochranu dat | Microsoft Docs"
description: "Při plánování řešení hybridní identity, identifikovat hello požadavků na ochranu dat pro vaši firmu a jaké možnosti jsou k dispozici toobest splňovat tyto požadavky."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 40dc4baa-fe82-4ab6-a3e4-f36fa9dcd0df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 189abf9affbc2894c322f362d84222d4e33d472e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Plán pro zlepšení zabezpečení dat prostřednictvím řešení silné identity
Hello první krok tooprotect hello dat je identifikovat, kdo má přístup k těmto datům a jako součást tohoto procesu musíte toohave, které se integruje řešení identitám, které můžete s možnosti vašeho systému tooprovide ověřování a autorizace. Ověřování a autorizace často matoucí mezi sebou a nesprávně pochopeny jejich rolí. Ve skutečnosti jsou výrazně lišit, jak je znázorněno na následujícím obrázku hello:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)

**Fáze životního cyklu správy mobilních zařízení**

Při plánování řešení hybridní identity je potřeba pochopit, že byly splněny požadavky na ochranu dat hello pro vaši firmu a jaké možnosti jsou k dispozici toobest. tyto požadavky.

> [!NOTE]
> Jakmile dokončíte plánování zabezpečení dat, zkontrolujte [stanovení požadavků na službu Multi-Factor authentication](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) tooensure, že váš výběr ohledně požadavků službou Multi-Factor authentication nedošlo k ovlivnění rozhodnutími hello můžete provedeny v této části.
> 
> 

## <a name="determine-data-protection-requirements"></a>Určení požadavků na ochranu dat
V hello stáří mobility, většina společností má běžné cílem: povolit jejich uživatelé toobe produktivitu na mobilním zařízení místně nebo vzdáleně z kdekoli v pořadí tooincrease produktivitu. To může být běžné cílem, společností, které mají takový požadavek bude také obavy hello množství hrozeb, které musí být omezeny v pořadí tookeep firemní data zabezpečené a spravovat ochranu osobních údajů uživatelů. Každá společnost může mít různé požadavky v tomto ohledu; pravidla různých dodržování předpisů, které se budou lišit podle toowhich odvětví hello společnosti funguje bude vést toodifferent rozhodnutí o návrhu. 

Existují však některé bezpečnostní aspekty které mají být prozkoumali a ověřit, bez ohledu na odvětví hello, které jsou vysvětlené v další části hello.

## <a name="data-protection-paths"></a>Cesty k datům ochrany
![](./media/hybrid-id-design-considerations/data-protection-paths.png)

**Cesty k datům ochrany**

V hello výše diagramu bude komponentu identita hello hello první z nich toobe ověřit předtím, než je přístup k datům. Však tato data mohou být v různých stavů během doby hello, ke kterému byl přístup. Všechna čísla v tomto diagramu představuje cestu, ve kterém může být data nachází v určitém okamžiku v čase. Tato čísla jsou vysvětleny níže:

1. Ochrana dat na úrovni zařízení hello.
2. Ochrana dat v průběhu přenosu.
3. Ochrana dat v rest na místě.
4. Ochrana dat v klidovém stavu uložených v cloudu hello.

I když hello technických prvků, které vám umožní IT tooprotect hello samotná data na každý z těchto fází nenabízel přímo hello hybridní řešení identit, je nezbytné, aby hello hybridní řešení identit schopná využívat místní a cloudové identity management prostředky tooidentify hello uživatele před udělit přístup k toohello datům. Při plánování řešení hybridní identity Ujistěte se, že hello následující otázky jsou zodpovězeny podle požadavků organizace tooyour:

## <a name="data-protection-at-rest"></a>Ochrana dat v klidovém stavu
Bez ohledu na to, kde jsou hello dat v klidovém stavu (zařízení, cloudové nebo místní) je důležité, že v tomto ohledu je tooperform organizaci assessment toounderstand hello. Pro tuto oblast Ujistěte se, se zobrazí výzva, že hello následující otázky:

* Potřebuje vaše společnost tooprotect dat v klidovém stavu?
  * Pokud ano, je možné toointegrate hello hybridní identity řešení s aktuální místní infrastrukturu?
  * Pokud ano, je možné toointegrate hello hybridní identity řešení úlohy umístěný v cloudu hello?
* Je hello cloudové identity management může tooprotect hello přihlašovacích údajů uživatele a další data uložená v cloudu hello?

## <a name="data-protection-in-transit"></a>Ochrana dat při přenosu
Musí být chráněny dat během přenosu mezi hello zařízení a hello datacenter nebo mezi hello zařízení a hello cloudu. Ale probíhá během přenosu nemusí nutně znamenat proces komunikace s komponentu mimo cloudové služby; Přesune interně taky, například mezi dvě virtuální sítě. Pro tuto oblast Ujistěte se, se zobrazí výzva, že hello následující otázky:

* Potřebuje vaše společnost tooprotect dat během přenosu?
  * Pokud ano, je možné toointegrate hello hybridní identity řešení zabezpečeného ovládací prvky, jako je například protokol SSL/TLS?
* Správu identity hello cloudu zachovat hello provoz tooand v rámci hello úložiště adresář (v rámci a mezi datovými centry) podepsané?

## <a name="compliance"></a>Dodržování předpisů
Požadavky předpisů, zákony a nařízení se bude lišit podle toohello odvětví, který je součástí vaší společnosti. Společností ve vysoké regulovaná odvětví, musí řešit problémy související toocompliance obavy správu identit. Předpisy, jako je Sarbanes-Oxley (SOX), hello zdravotním pojištění a odpovědnosti za Application Compatibility Toolkit (HIPAA) hello Gramm. Leach Bliley Act (GLBA) a hello karty oborový Standard zabezpečení dat platebních (PCI DSS) jsou velmi přísná týkající se přístupu a identit. Hello řešení hybridní identity, kterou vaše společnost zavede musí mít hello základní možnosti, které bude splnit požadavky hello jednoho nebo více těchto pravidel. Pro tuto oblast Ujistěte se, se zobrazí výzva, že hello následující otázky:

* Je kompatibilní s hello zákonné požadavky pro vaši firmu hello hybridní řešení identit?
* Nepodporuje hello hybridní řešení identit má vestavěné funkce, které vám umožní vaší společnosti toobe kompatibilní zákonné požadavky? 

> [!NOTE]
> Ujistěte se, že tootake poznámky u každé odpovědi a pochopit hello důvody, které hello odpovědí. [Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přenášeny po hello dostupných možností a výhod i nevýhod jednotlivých možností.  Po zodpovězení těchto otázek, které vyberete která možnost nejlépe vyhovuje vaší firmě potřebuje.
> 
> 

## <a name="next-steps"></a>Další kroky
 [Stanovení požadavků na správu obsahu](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

