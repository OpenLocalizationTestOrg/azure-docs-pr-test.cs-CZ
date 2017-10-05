---
title: "Správa partnerských řešení v Azure Security Center | Microsoft Docs"
description: "V tomto dokumentu se dozvíte, jak vám Azure Security Center umožňuje přehledně sledovat stav partnerských řešení integrovaných ve vašem předplatném Azure."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: terrylan
ms.openlocfilehash: 2ebb930e877c5027f4d7b0a316a7f5ebe84471b1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Sledování partnerských řešení pomocí Azure Security Center
V tomto dokumentu se dozvíte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.

> [!NOTE]
> Tento dokument vám tuto službu představí formou ukázkového nasazení. Tento dokument není to podrobný průvodce.
>
>

## <a name="monitoring-partner-solutions"></a>Sledování partnerských řešení
**Partner solutions** na dlaždici **Security Center** okno umožňuje přehledně sledovat stav vašich partnerských řešení, které jsou integrované s předplatným Azure.

![Dlaždice Partner solutions (Partnerská řešení)][1]

**Partner solutions** dlaždice zobrazí počet partnerských řešení integrovaných ve vašem předplatném. Pokud integrovaná žádná řešení se zobrazí na dlaždici hodnotu nula.

Stav partnerských řešení zobrazíte takto:

1. Vyberte dlaždici **Partner solutions** (Partnerská řešení). **Partner solutions** otevře se okno se seznamem partnerských řešení připojených k Security Center.

   ![Partnerská řešení][3]

   Stav partnerských řešení může být:

   * Chráněné (zelená) – Stav je zcela v pořádku.
   * Není v pořádku (červená) – Existuje problém stavu, které si žádá okamžitou pozornost.
   * Nehlásí se (oranžová) – Řešení přestalo hlásit svůj stav.
   * Neznámý stav ochrany (oranžová) – Stav řešení teď není známý, protože selhal proces přidávání nového prostředku do stávajícího řešení.
   * Neuveden (šedá) – řešení ještě nenahlásilo nic ještě stav řešení nemusí být uvedený, pokud se nedávno připojil a ještě probíhá jeho nasazení.

2. Vyberte jedno partnerské řešení. V tomto příkladu umožní vybrat **Qualys** řešení.  Otevře se okno, které obsahuje stav tohoto partnerského řešení a jeho přidružené prostředky. Výběrem možnosti **Solution console** (Konzola řešení) otevřete prostředí pro správu tohoto partnerského řešení.

   ![Podrobné zobrazení partnerského řešení][4]
3. Přejděte zpět **Qualys** a vyberte **propojení virtuálních počítačů**. Otevře se okno **Link Applications** (Připojit aplikace). Tady můžete ke svému partnerskému řešení připojit prostředky.

   ![Připojení prostředků k partnerskému řešení][5]

## <a name="next-steps"></a>Další kroky
V tomto dokumentu jste se seznámili s dlaždicí **Partner Solutions** (Partnerská řešení) ve službě Security Center. Další informace o službě Security Center najdete v následujících článcích:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure a informace.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
