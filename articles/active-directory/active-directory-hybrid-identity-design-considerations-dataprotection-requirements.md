---
title: "Azure Active Directory hybridní identity aspekty návrhu - určení požadavků na ochranu dat | Microsoft Docs"
description: "Při plánování řešení hybridní identity, určete požadavky na ochranu dat pro vaši firmu a jaké možnosti jsou dostupné pro nejlépe splnit tyto požadavky."
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
ms.openlocfilehash: 96bf9d4c26a22f718c29804c11681199e775f589
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Plán pro zlepšení zabezpečení dat prostřednictvím řešení silné identity
Prvním krokem k ochraně dat je identifikovat, kdo má přístup k těmto datům a řešení, které můžete v rámci tohoto procesu je potřeba mít identity se integruje s systému a poskytuje možnosti ověřování a autorizace. Ověřování a autorizace často matoucí mezi sebou a nesprávně pochopeny jejich rolí. Ve skutečnosti jsou výrazně lišit, jak je znázorněno na obrázku níže:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)

**Fáze životního cyklu správy mobilních zařízení**

Při plánování řešení hybridní identity musíte pochopit požadavky na ochranu dat pro vaši firmu a jaké možnosti jsou dostupné pro nejlépe splnit tyto požadavky.

> [!NOTE]
> Jakmile dokončíte plánování zabezpečení dat, zkontrolujte [stanovení požadavků na službu Multi-Factor authentication](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) zajistit váš výběr ohledně požadavků službou Multi-Factor authentication nebyly vliv na rozhodnutí, která jste provedené v této části.
> 
> 

## <a name="determine-data-protection-requirements"></a>Určení požadavků na ochranu dat
V stáří mobility, většina společností má běžné cílem: povolit svým uživatelům zajistit produktivitu na mobilním zařízení místně nebo vzdáleně z libovolného místa zvýšení produktivity. To může být běžné cílem, společností, které mají takový požadavek bude také obavy týkající se množství hrozeb, které musí být omezeny aby bylo možné zabezpečit firemní data a zachovat ochranu osobních údajů uživatelů. Každá společnost může mít různé požadavky v tomto ohledu; pravidla různých dodržování předpisů, které se budou lišit podle odvětví, které společnost funguje přejde na jiný rozhodnutí. 

Existují však některé bezpečnostní aspekty které mají být prozkoumali a ověřit, bez ohledu na odvětví, které jsou vysvětlené v další části.

## <a name="data-protection-paths"></a>Cesty k datům ochrany
![](./media/hybrid-id-design-considerations/data-protection-paths.png)

**Cesty k datům ochrany**

V diagramu bude komponentu identita první z nich pro ověření, než je přístup k datům. Však tato data, může být v různých stavů dobu, kdy byl přistupoval. Všechna čísla v tomto diagramu představuje cestu, ve kterém může být data nachází v určitém okamžiku v čase. Tato čísla jsou vysvětleny níže:

1. Ochrana dat na úrovni zařízení.
2. Ochrana dat v průběhu přenosu.
3. Ochrana dat v rest na místě.
4. Ochrana dat v klidovém stavu uložených v cloudu.

I když technické ovládací prvky se bude dáte oddělení IT k ochraně samotná data na každý z těchto fází nenabízel přímo hybridní řešení identit, je nutné, že je schopná využívat místní i cloudové řešení hybridní identity zdroje informací pro správu identity k identifikaci uživatele před udělit přístup k datům. Při plánování řešení hybridní identity se ujistěte, že následující otázky jsou zodpovězeny v souladu s požadavky vaší organizace:

## <a name="data-protection-at-rest"></a>Ochrana dat v klidovém stavu
Bez ohledu na to, kde jsou data v klidovém stavu (zařízení, cloudové nebo místní) je důležité provést vyhodnocení v tomto ohledu pochopení potřeb organizace. Pro tuto oblast Ujistěte se, že se zobrazí výzva na následující otázky:

* Potřebuje vaše společnost k ochraně dat v klidovém stavu?
  * Pokud ano, zvládne hybridní řešení identit integraci s aktuální místní infrastrukturu?
  * Pokud ano, zvládne hybridní řešení identit integrovat úlohy umístěný v cloudu?
* Je-li správu identity cloudu schopné chránit přihlašovacích údajů uživatele a další data uložená v cloudu?

## <a name="data-protection-in-transit"></a>Ochrana dat při přenosu
Data na cestě mezi datovým centrem a zařízení nebo mezi zařízením a cloudem musí být chráněny. Ale probíhá během přenosu nemusí nutně znamenat proces komunikace s komponentu mimo cloudové služby; Přesune interně taky, například mezi dvě virtuální sítě. Pro tuto oblast Ujistěte se, že se zobrazí výzva na následující otázky:

* Potřebuje vaše společnost k ochraně dat během přenosu?
  * Pokud ano, zvládne hybridní řešení identit integrovat zabezpečené ovládací prvky, jako je například protokol SSL/TLS?
* Správu identity cloudu zachovat provoz do a v rámci adresáře úložiště (v rámci a mezi datovými centry) podepsané?

## <a name="compliance"></a>Dodržování předpisů
Požadavky předpisů, zákony a nařízení se liší podle odvětví, který je součástí vaší společnosti. Společností ve vysoké regulovaná odvětví, musí řešit správu identit otázky související s problémy s dodržováním předpisů. Předpisy, jako je Sarbanes-Oxley (SOX), zdravotním pojištění odpovědnosti za Application Compatibility Toolkit (HIPAA), Gramm. Leach Bliley Act (GLBA) a karty oborový Standard zabezpečení dat platebních (PCI DSS) jsou velmi přísná týkající se přístupu a identit. Řešení hybridní identity, kterou vaše společnost zavede musí mít základní možnosti, které bude splnit požadavky na jeden nebo více těchto pravidel. Pro tuto oblast Ujistěte se, že se zobrazí výzva na následující otázky:

* Je řešení hybridní identity splňovat zákonné požadavky pro vaši organizaci?
* Hybridní řešení identit má vestavěné funkce, které vám umožní vaší společnosti, abyste se kompatibilní zákonné požadavky? 

> [!NOTE]
> Zajistěte, aby poznamenejte každou odpověď a pochopit důvody odpověď. [Definování strategie ochrany dat](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) budou přenášeny po dostupných možností a výhod i nevýhod jednotlivých možností.  Po zodpovězení těchto otázek, které vyberete která možnost nejlépe vyhovuje vaší firmě potřebuje.
> 
> 

## <a name="next-steps"></a>Další kroky
 [Stanovení požadavků na správu obsahu](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

