---
title: "aaaOverview výstrah v Microsoft Azure a Azure monitorování | Microsoft Docs"
description: "Výstrahy umožňují metriky toomonitor prostředků Azure, události nebo protokoly a být upozorněni, když je splněna podmínka, které zadáte."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: robb
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d080080666fedc9eefda6b35cdea3714785d2223
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-alerts-in-microsoft-azure"></a>Co jsou výstrahy v Microsoft Azure?
Tento článek popisuje hello různých zdrojů výstrah v Microsoft Azure, jaké jsou hello účely pro tyto výstrahy, jejich výhody a jak se tooget pracovat s jejich používání. Konkrétně platí tooAzure monitorování, ale poskytuje ukazatele tooother služby s výstrahami také. Výstrahy nabízejí metoda monitorování v Azure, který povolí tooconfigure podmínky přes data a stát upozorněni, když hello podmínky odpovídají hello nejnovější data monitorování.

## <a name="taxonomy-of-azure-alerts"></a>Taxonomii Azure výstrah
Azure používá hello následující podmínky toodescribe výstrahy a jejich funkce:
* **Výstrahy** -definici kritéria (jeden nebo více pravidel nebo podmínky), která se změní na aktivovat, pokud splněny.
* **Aktivní** – hello stavu hello při splnění kritérií pro definované výstrahu.
* **Vyřešit** – hello stavu při hello po dříve splněné již není splněna kritéria definovaná výstrahu.
* **Oznámení** -hello akce na základě z výstrahu, aby se aktivovala.
* **Akce** -konkrétní volání odeslaných tooa příjemce oznámení (například e-mailem na adresu nebo publikování adresa URL webhooku tooa). Oznámení můžete aktivovat obvykle více akcí.

## <a name="alerts-in-different-azure-services"></a>Výstrahy v různých služeb Azure
Výstrahy jsou k dispozici v rámci několik Azure monitorování služeb. Informace o tom, jak a kdy toouse tyto služby [najdete v článku](./monitoring-overview.md). Zde je výčet typů výstrah hello k dispozici v Azure:

| Služba | Typ výstrahy | Podporované služby | Popis |
|---|---|---|---|
| Azure Monitor | [Metriky výstrahy](./insights-alerts-portal.md) | [Podporované metriky z monitorování Azure](./monitoring-supported-metrics.md) | Přijímat oznámení v případě jakékoli úrovni platformy metrika splňuje určité podmínky (například % využití procesoru na virtuálním počítači je větší než 90 pro hello za posledních 5 minut). |
| Azure Monitor | [Aktivity protokolu výstrahy](./monitoring-activity-log-alerts.md) | Všechny typy prostředků, k dispozici ve službě Správce prostředků Azure | Příjem oznámení, když se všechny nové události v hello [protokol činnosti Azure](./monitoring-overview-activity-logs.md) odpovídá konkrétní podmínky (například pokud se operace "Odstranit virtuální počítač" dojde v myProductionResourceGroup nebo pokud novou událost stavu služby s "Aktivní" jako Zobrazí stav Hello). |
| Application Insights | [Metriky výstrahy](../application-insights/app-insights-alerts.md) | Všechny aplikace instrumentovány toosend data tooApplication statistiky | Přijímat oznámení v případě jakékoli úrovni aplikace metrika splňuje určité podmínky (například doba odezvy serveru je větší než 2 sekund). |
| Application Insights | [Webový test výstrahy](../application-insights/app-insights-monitor-web-app-availability.md) | Všechny webu instrumentovány toosend data tooApplication statistiky | Přijímat oznámení v případě dostupnosti nebo odezvy webu je nižší než očekávání. |
| Log Analytics | [Upozornění analýzy protokolů](../log-analytics/log-analytics-alerts.md) | Všechny služby konfigurovaná toosend data do analýzy protokolů | Přijímat oznámení v případě vyhledávací dotaz analýzy protokolů přes data metriky a události splňuje určitá kritéria. |

## <a name="alerts-on-azure-monitor-data"></a>Výstrahy na dat monitorování Azure
Existují dva typy výstrah z dat z Azure sledování – metriky výstrahy a protokol aktivit výstrahy k dispozici.

* **Metriky výstrahy** – Tato výstraha aktivuje, když hodnota hello zadaný metriky překračuje prahovou hodnotu, která přiřadíte. Výstraha Hello generuje oznámení, pokud je výstraha hello "aktivován" (Pokud se překročí prahovou hodnotu hello a je splněna podmínka výstrahy hello) a také při se "nevyřeší" (Pokud je znovu překročí prahovou hodnotu hello a již není splněna podmínka hello). Rostoucí seznam dostupné metriky, které podporuje Azure monitorování, naleznete v části [seznam metriky, které jsou podporovány pro monitorování Azure](monitoring-supported-metrics.md).
* **Aktivity protokolu výstrahy** -streamování výstrahu protokolu, které spustí, když je generována událost protokol aktivit, že kritéria, které jste přiřadili filtru odpovídá. Tyto výstrahy mít pouze jeden stav "Aktivovali," vzhledem k tomu použije výstrahy modul hello jednoduše hello filtru kritéria tooany novou událost. Tyto výstrahy mohou být použité toobecome upozornění v případě nový incident stav služby nebo pokud uživatele nebo aplikace provede operaci v rámci vašeho předplatného, například "Odstranění virtuálního počítače."

Pro data protokolů diagnostiky k dispozici prostřednictvím Azure monitorování doporučujeme dat směrování hello do analýzy protokolů a používání analýzy protokolů výstrahy. Hello následující diagram shrnuje zdroje dat v Azure monitorování a, koncepčně, jak můžete výstrahy z tato data.

![Vysvětlení výstrah](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="how-do-i-receive-a-notification-on-an-azure-monitor-alert"></a>Jak mohu dostávat oznámení na výstrahu monitorování Azure?
V minulosti Azure výstrahy z různých služeb používá vlastní metody předdefinovaných oznámení. Do budoucna, monitorování Azure nabízí opakovaně použitelné oznámení seskupení nazývají skupiny akce. Akce skupiny zadejte sadu příjemci pro oznámení – libovolný počet e-mailové adresy, telefonních čísel (pro SMS) nebo adresy URL webhooku – a kdykoli výstraha je aktivována, že odkazy hello akce skupiny všichni příjemci příjmu tohoto oznámení. To vám umožní tooreuse seskupení příjemci (například na pracovníka seznamu volání) napříč mnoho výstrah objektů. V současné době pouze protokol aktivit výstrahy využívat skupin akce, ale několik dalších Azure typy výstrah pracují na také použití skupin akce.

Akce skupiny podporují oznámení zveřejněním adresa URL webhooku tooa v přidání tooemail adresy a čísla SMS. To umožňuje automatizace a nápravu, například pomocí:
    - Runbook Azure Automation
    - Azure – funkce
    - Azure Logic Apps
    - službu třetí strany

Metriky výstrahy ještě nepoužívejte skupiny akcí. Na jednotlivé metriky výstrahy můžete nakonfigurovat oznámení:
* Odeslat e-mailové oznámení toohello služby správce, tooco správci nebo tooadditional e-mailové adresy, které zadáte.
* Volejte webhook, což vám umožní toolaunch automatizace další akce.

## <a name="next-steps"></a>Další kroky
Získání informací o pravidla výstrah a jejich konfigurací pomocí:

* Další informace o [metriky](monitoring-overview-metrics.md)
* Konfigurace [metrika výstrahy prostřednictvím portálu Azure](insights-alerts-portal.md)
* Konfigurace [metrika výstrahy prostředí PowerShell](insights-alerts-powershell.md)
* Konfigurace [rozhraní metrika výstrahy příkazového řádku (CLI)](insights-alerts-command-line-interface.md)
* Konfigurace [metrika výstrahy monitorování Azure rozhraní REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Další informace o [protokol aktivit](monitoring-overview-activity-logs.md)
* Konfigurace [aktivity protokolu výstrahy prostřednictvím portálu Azure](monitoring-activity-log-alerts.md)
* Konfigurace [aktivity protokolu výstrahy prostřednictvím Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Zkontrolujte hello [schématu výstrahy webhooku protokolu aktivit](monitoring-activity-log-alerts-webhook.md)
* Další informace o [oznámení o službách](monitoring-service-notifications.md)
* Další informace o [skupiny akcí](monitoring-action-groups.md)
