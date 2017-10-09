---
title: aaaIntroduction tooAzure Advisor | Microsoft Docs
description: "Pomocí Azure Advisor toooptimize Azure nasazení."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 5d796fc06366221efdb6f1bda39ab3fb676abfd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-advisor"></a>Úvod tooAzure Advisor

Další informace o Azure Advisor a jejích klíčových funkcích a získat odpovědi toofrequently dotazy.

## <a name="what-is-advisor"></a>Co je Advisor?
Advisor je konzultantem přizpůsobené cloudu, který vám pomůže sledovat osvědčené postupy toooptimize svá nasazení Azure. Ho analyzuje konfigurace prostředků a telemetrii využití a potom doporučuje řešení, které vám mohou pomoci zlepšit hello finanční efektivita, výkon, vysokou dostupnost a zabezpečení vašich prostředků Azure.

Advisor můžete:
* Získejte proaktivní, kterého lze provést akci a přizpůsobené osvědčené postupy a doporučení. 
* Zvýšení výkonu hello, zabezpečení a vysokou dostupnost vašich prostředků, jak identifikovat příležitosti tooreduce tráví vaše celkové Azure.
* Získejte doporučení s vložené navrhovaná akce.

Advisor můžete přistupovat prostřednictvím hello [portál Azure](https://aka.ms/azureadvisordashboard). Přihlaste se toohello [portál](https://portal.azure.com), vyberte **Procházet**a posuňte se příliš**Azure Advisor**. řídicí panel Advisor Hello zobrazuje přizpůsobené doporučení pro vybrané předplatné. 

Hello doporučení jsou rozděleny do čtyř kategorií: 

* **Vysoká dostupnost**: tooensure a zlepšování kontinuity hello důležitými obchodními aplikacemi. Další informace najdete v tématu [vysokou dostupnost Advisor doporučení](advisor-high-availability-recommendations.md).

* **Zabezpečení**: toodetect hrozby a ohrožení zabezpečení, které můžou způsobit narušení toosecurity. Další informace najdete v tématu [doporučení zabezpečení Advisor](advisor-security-recommendations.md).

* **Výkon**: tooimprove hello rychlosti aplikací. Další informace najdete v tématu [Poradce pro výkon doporučení](advisor-performance-recommendations.md).

* **Náklady na**: toooptimize a snížit vaše celkové Azure tráví. Další informace najdete v tématu [doporučení služby Advisor náklady](advisor-cost-recommendations.md).

  ![Typy doporučení služby Advisor](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> tooaccess doporučení služby Advisor, musíte nejdřív *zaregistrovat předplatné* službou Advisor. Předplatné je zaregistrován při *předplatné vlastníka* spustí hello Advisor řídicí panel a klikne na tlačítko hello **získat doporučení** tlačítko. Toto je *jednorázovou operaci*. Po registraci předplatného hello dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.

Můžete kliknout na další informace doporučení toolearn. Můžete si také přečíst o akcích, můžete provést tootake výhod příležitost nebo vyřešte problém. 

Advisor nabízí doporučení s vložené akce nebo odkazy na dokumentaci. Kliknutím na vložené akce vás provede tooimplement "cesty s průvodcem uživatele" jej. Kliknutím na odkaz dokumentace body toodocumentation, který popisuje, jak implementovat toomanually hello akce. 

Advisor aktualizuje doporučení každou hodinu. Na základě doporučení nechystáte tootake okamžitý zásah, můžete zopakovat později pro zadané časové období, nebo ji zavřít. 

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="how-do-i-access-advisor"></a>Přístupu Advisor
Advisor můžete přistupovat prostřednictvím hello [portál Azure](https://aka.ms/azureadvisordashboard). Přihlaste se toohello [portál](https://portal.azure.com), vyberte **Procházet**a posuňte se příliš**Azure Advisor**. řídicí panel Advisor Hello zobrazuje přizpůsobené doporučení pro vybrané předplatné. 

Doporučení služby Advisor můžete zobrazit také prostřednictvím hello okna prostředků virtuálního počítače. Vyberte virtuální počítač a poté přejděte tooAdvisor doporučení v nabídce hello. 

### <a name="what-permissions-do-i-need-tooaccess-advisor"></a>Co dělat, oprávnění tooaccess Advisor je potřeba?

tooaccess doporučení služby Advisor, musíte nejdřív *zaregistrovat předplatné* službou Advisor. Předplatné je zaregistrován při *předplatné vlastníka* spustí hello Advisor řídicí panel a klikne na tlačítko hello **získat doporučení** tlačítko. Toto je *jednorázovou operaci*. Po registraci předplatného hello dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.

### <a name="how-often-are-advisor-recommendations-updated"></a>Jak často jsou doporučení služby Advisor aktualizovat?

Doporučení služby Advisor se aktualizují každou hodinu.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Jaké prostředky poskytuje Advisor doporučení pro?

Advisor poskytuje doporučení pro virtuální počítače, skupiny dostupnosti, application Gateway, aplikační služby, servery SQL Server, databáze SQL a Redis Cache.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Můžete připomenout znovu nebo zrušit doporučení?

toosnooze nebo zrušit doporučení, klikněte na hello **připomenout znovu** tlačítko nebo odkaz. Můžete zadat dobu připomenutí období nebo vybrat možnost **nikdy** toodismiss hello doporučení.

## <a name="next-steps"></a>Další kroky

toolearn Další informace o doporučení služby Advisor, najdete v části:

* [Začínáme se službou Advisor](advisor-get-started.md)
* [Doporučení pro vysokou dostupnost služby Advisor](advisor-high-availability-recommendations.md)
* [Doporučení zabezpečení Advisor](advisor-security-recommendations.md)
* [Poradce při hodnocení výkonu doporučení](advisor-performance-recommendations.md)
* [Náklady na doporučení služby Advisor](advisor-cost-recommendations.md)
