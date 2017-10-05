---
title: "Zobrazit protokoly Azure aktivitu se analýza protokolů | Microsoft Docs"
description: "Řešení Azure aktivity protokoly můžete použít k analýze a hledání protokol činnosti Azure ve všech vašich předplatných Azure."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.openlocfilehash: 1ad56a54f094f3c314596b3a7c9fecd09647d065
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-azure-activity-logs"></a>Zobrazit protokoly aktivita Azure

![Azure symbol protokoly aktivity](./media/log-analytics-activity/activity-log-analytics.png)

Analýzy protokolů aktivity řešení vám pomáhají analyzovat a hledání [protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) ve všech vašich předplatných Azure. Protokol činnosti Azure je protokol, který nabízí přehled operací prováděných s prostředky v rámci vašich předplatných. Protokol aktivit se dřív označovala jako *protokoly auditu* nebo *operační protokoly* vzhledem k tomu, že oznámí události pro vaše předplatné.

Pomocí protokolu činnosti, můžete určit *co*, *kdo*, a *při* pro všechny zápisu operace (PUT, POST, DELETE) pro prostředky ve vašem předplatném. Můžete také chápou stav operace a další relevantní vlastnosti. Protokol aktivit nezahrnuje operace čtení (GET) nebo operace pro prostředky, které používají model nasazení Classic.

Pokud připojíte k analýze protokolů protokolů aktivita Azure, můžete:

- Analýza protokolů aktivity s předem definované zobrazení
- Analýza a protokoly vyhledávání a aktivity z několika předplatných Azure
- Zachovat protokoly aktivity po dobu delší než 90 dní<sup>1</sup>
- Korelovat protokoly aktivity s další Azure data platformy a aplikace
- Najdete v provozní aktivity agregovat podle stavu
- Zobrazení trendů aktivit děje na všech služeb Azure
- Zprávu o autorizaci změny na všech vašich prostředků Azure
- Určit vaše prostředky, které mají vliv výpadku nebo služby týkající se stavu
- Použijte funkci vyhledávání protokolu ke korelaci aktivity uživatelů, operace automatického škálování, změny autorizace a stav služby na jiné protokoly nebo metriky ze svého prostředí

<sup>1</sup>ve výchozím nastavení, analýzy protokolů udržuje protokolů Azure aktivity dobu 90 dnů, i když jsou na úrovni Free. Nebo, pokud máte pracovní prostor nastavení uchování menší než 90 dní. Pokud pracovní prostor uchovávání dat, který je delší než 90 dní, protokoly aktivity budou pro dobu uchování vašeho pracovního prostoru.

Analýzy protokolů shromažďuje protokoly aktivity, které jsou zdarma a ukládá protokoly 90 dní zdarma. Pokud ukládáte protokoly víc než 90 dnů, bude platit poplatky uchovávání dat pro data uložená déle než 90 dní.

Pokud jste na cenové úrovně Free, protokoly aktivity se nevztahují na vaše denní spotřeba data.

## <a name="connected-sources"></a>Připojené zdroje

Na rozdíl od většiny jiných řešení analýzy protokolů není data shromážděná protokoly aktivity pomocí agentů. Všechna data, která používá řešení pochází přímo z Azure.

| Připojený zdroj | Podporuje se | Popis |
| --- | --- | --- |
| [Agenti systému Windows](log-analytics-windows-agents.md) | Ne | Řešení neshromažďuje informace z agentů v systému Windows. |
| [Agenti systému Linux](log-analytics-linux-agents.md) | Ne | Řešení neshromažďuje informace od agentů systému Linux. |
| [Skupiny správy nástroje SCOM](log-analytics-om-agents.md) | Ne | Řešení neshromažďuje informace od agentů v připojené skupině pro správu SCOM. |
| [Účet služby Azure Storage](log-analytics-azure-storage.md) | Ne | Řešení neshromažďuje informace ze služby Azure storage. |

## <a name="prerequisites"></a>Požadavky

- Pro přístup k informacím protokolu aktivita Azure, musíte mít předplatné Azure.

## <a name="configuration"></a>Konfigurace

Proveďte následující postup pro konfiguraci řešení aktivity analýzy protokolů pro pracovní prostory.

1. Povolit řešení analýzy protokolů aktivity z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).
2. Konfigurace protokolů událostí přejděte do pracovního prostoru analýzy protokolů.
    1. Na portálu Azure vyberte pracovní prostor a pak klikněte na tlačítko **protokol činnosti Azure**.
    2. Pro každé předplatné klikněte na název odběru.  
        ![Přidat předplatné](./media/log-analytics-activity/add-subscription.png)
    3. V *Název_předplatného* okně klikněte na tlačítko **Connect**.  
        ![připojení odběru](./media/log-analytics-activity/subscription-connect.png)

Pokud přidáte řešení pomocí portálu OMS, uvidíte následující dlaždice. Přihlásit se k portálu Azure a předplatné Azure připojit do pracovního prostoru.  
![provádění hodnocení](./media/log-analytics-activity/tile-performing-assessment.png)

## <a name="using-the-solution"></a>Použití řešení

Když přidáte řešení analýzy protokolů aktivitu do pracovního prostoru **protokoly aktivity Azure** dlaždice se přidá na řídicí panel Přehled. Tuto dlaždici zobrazí počet záznamů aktivita Azure pro předplatná Azure, která má přístup k řešení.

![Azure dlaždice protokoly aktivity](./media/log-analytics-activity/azure-activity-logs-tile.png)

### <a name="view-azure-activity-logs"></a>Aktivita služby Azure zobrazení protokolů

Klikněte na tlačítko **protokoly aktivity Azure** dlaždici otevřete **protokoly aktivity Azure** řídicího panelu. Řídicí panel obsahuje okna v následující tabulce. Každý okno uvádí až 10 položky odpovídající kritériím tohoto okna pro zadaný obor a časový rozsah. Můžete spustit vyhledávání protokolu, který vrátí všechny záznamy kliknutím **zobrazit všechny** v dolní části okna, nebo kliknutím na záhlaví okna.

Data protokolu aktivity se zobrazí pouze *po* protokolů aktivity k přejít na řešení, takže data nelze zobrazit, předtím jste nakonfigurovali.

| Okno | Popis |
| --- | --- |
| Aktivita Azure položky protokolu | Ukazuje pruhový graf prvních položka protokolu aktivita Azure záznamů součty pro rozsah dat, které jste vybrali a seznam top 10 aktivity volající. Klikněte na tlačítko pruhový graf ke spuštění protokolu vyhledejte <code>Type=AzureActivity</code>. Klikněte na položku volající ke spuštění vyhledávání protokolu vrácení všech položek protokolů aktivity pro tuto položku. |
| Protokoly aktivity podle stavu | Zobrazí prstencový graf pro stav protokolu Azure aktivity pro rozsah, kterou jste vybrali. Také ukazuje seznam seznam top deset stav záznamů. Klikněte na graf ke spuštění protokolu vyhledejte <code>Type=AzureActivity &#124; measure count() by ActivityStatus</code>. Klikněte na položku Stav spuštění vyhledávání protokolu vrácení všech položek protokolů aktivity pro tento stav záznamu. |
| Protokoly aktivity podle zdroje | Zobrazuje celkový počet prostředků s protokoly aktivity a uvádí top deset prostředky pomocí záznamu počty každého prostředku. Klikněte na tlačítko Spustit hledání protokolů pro oblasti celkový <code>Type=AzureActivity &#124; measure count() by Resource</code>, který zobrazuje všechny prostředky Azure k dispozici pro řešení. Klikněte na tlačítko prostředků ke spuštění vyhledávání protokolu vrátit všechny záznamy aktivity pro tento prostředek. |
| Protokoly aktivity poskytovatelem prostředků | Zobrazuje celkový počet poskytovatelů prostředků, které produkují aktivity protokoly a uvádí deset. Klikněte na tlačítko Spustit hledání protokolů pro oblasti celkový <code>Type=AzureActivity &#124; measure count() by ResourceProvider</code>, který zobrazuje všechny poskytovatele prostředků Azure. Klikněte na zprostředkovatele prostředků ke spuštění vyhledávání protokolu vrátit všechny záznamy aktivity pro zprostředkovatele. |

![Řídicí panel Azure protokoly aktivity](./media/log-analytics-activity/activity-log-dash.png)

## <a name="next-steps"></a>Další kroky

- Vytvoření [výstraha](log-analytics-alerts-creating.md) když se stane konkrétní aktivity.
- Použití [hledání protokolů](log-analytics-log-searches.md) k zobrazení podrobných informací z protokolů aktivity.
