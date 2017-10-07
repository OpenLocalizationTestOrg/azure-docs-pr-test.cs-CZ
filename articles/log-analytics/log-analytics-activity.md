---
title: aaaView aktivita Azure protokoly s Log Analytics | Microsoft Docs
description: "Hello tooanalyze řešení Azure protokoly aktivity a protokol hledání hello aktivita Azure můžete použít ve všech vašich předplatných Azure."
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
ms.openlocfilehash: 171d0d604d03a5714a9599cc0b448fc5f6471f69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-azure-activity-logs"></a>Zobrazit protokoly aktivita Azure

![Azure symbol protokoly aktivity](./media/log-analytics-activity/activity-log-analytics.png)

Hello analýzy protokolů aktivity řešení vám pomáhají analyzovat a hledání hello [protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) ve všech vašich předplatných Azure. Hello protokol činnosti Azure je protokol, který nabízí přehled hello operace provedené na prostředky v rámci vašich předplatných. Hello protokol aktivit se dřív označovala jako *protokoly auditu* nebo *operační protokoly* vzhledem k tomu, že oznámí události pro vaše předplatné.

Pomocí hello protokolu činnosti, můžete určit hello *co*, *kdo*, a *při* pro všechny zápisu operace (PUT, POST, DELETE) pro hello prostředky ve vašem předplatném. Můžete také chápou hello stav operace hello a další relevantní vlastnosti. Hello protokol aktivit nezahrnuje čtení operací (GET) nebo operace pro prostředky, které používají model nasazení Classic hello.

Pokud připojíte vaši tooLog aktivita Azure protokoly analýzy, můžete:

- Analýza protokolů hello aktivity s předem definované zobrazení
- Analýza a protokoly vyhledávání a aktivity z několika předplatných Azure
- Zachovat protokoly aktivity po dobu delší než 90 dní<sup>1</sup>
- Korelovat protokoly aktivity s další Azure data platformy a aplikace
- Najdete v provozní aktivity agregovat podle stavu
- Zobrazení trendů aktivit děje na všech služeb Azure
- Zprávu o autorizaci změny na všech vašich prostředků Azure
- Určit vaše prostředky, které mají vliv výpadku nebo služby týkající se stavu
- Pomocí aktivity uživatele toocorrelate hledání protokolů, operace automatického škálování, autorizace změny a protokoly tooother služby stavu nebo metriky ze svého prostředí

<sup>1</sup>ve výchozím nastavení, analýzy protokolů udržuje protokolů Azure aktivity dobu 90 dnů, i když jsou na úrovni Free hello. Nebo, pokud máte pracovní prostor nastavení uchování menší než 90 dní. Pokud pracovní prostor uchovávání dat, který je delší než 90 dní, protokoly aktivity hello budou pro dobu uchování hello pracovního prostoru.

Analýzy protokolů shromažďuje protokoly aktivity, které jsou zdarma a ukládá protokoly hello 90 dní zdarma. Pokud ukládáte protokoly víc než 90 dnů, bude platit poplatky uchovávání dat pro hello data uložená déle než 90 dní.

Pokud jste na hello volná cenová úroveň, protokoly aktivity neplatí tooyour denní spotřeba data.

## <a name="connected-sources"></a>Připojené zdroje

Na rozdíl od většiny jiných řešení analýzy protokolů není data shromážděná protokoly aktivity pomocí agentů. Všechna data, která používá řešení hello pochází přímo z Azure.

| Připojený zdroj | Podporuje se | Popis |
| --- | --- | --- |
| [Agenti systému Windows](log-analytics-windows-agents.md) | Ne | řešení Hello neshromažďuje informace z agentů v systému Windows. |
| [Agenti systému Linux](log-analytics-linux-agents.md) | Ne | řešení Hello neshromažďuje informace od agentů systému Linux. |
| [Skupiny správy nástroje SCOM](log-analytics-om-agents.md) | Ne | řešení Hello neshromažďuje informace od agentů v připojené skupině pro správu SCOM. |
| [Účet služby Azure Storage](log-analytics-azure-storage.md) | Ne | řešení Hello neshromažďuje informace ze služby Azure storage. |

## <a name="prerequisites"></a>Požadavky

- tooaccess informace protokolu aktivita Azure, musíte mít předplatné Azure.

## <a name="configuration"></a>Konfigurace

Proveďte následující kroky tooconfigure hello analýzy protokolů aktivity řešení pro vaše pracovní prostory hello.

1. Povolit hello analýzy protokolů aktivity řešení z hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).
2. Konfigurace aktivity protokoly toogo tooyour pracovní prostor analýzy protokolů.
    1. V hello portálu Azure, vyberte pracovní prostor a pak klikněte na tlačítko **protokol činnosti Azure**.
    2. Pro každé předplatné klikněte na název odběru hello.  
        ![Přidat předplatné](./media/log-analytics-activity/add-subscription.png)
    3. V hello *Název_předplatného* okně klikněte na tlačítko **Connect**.  
        ![připojení odběru](./media/log-analytics-activity/subscription-connect.png)

Pokud přidáte hello řešení pomocí portálu OMS hello, zobrazí se následující hello dlaždici. Přihlaste se toohello Azure portálu tooconnect k pracovnímu prostoru tooyour předplatného Azure.  
![provádění hodnocení](./media/log-analytics-activity/tile-performing-assessment.png)

## <a name="using-hello-solution"></a>Pomocí řešení hello

Když přidáte pracovní prostor tooyour hello analýzy protokolů aktivity řešení, hello **protokoly aktivity Azure** dlaždice se přidá tooyour přehled řídicí panel. Tuto dlaždici zobrazí počet hello počet záznamů aktivita Azure pro hello předplatná Azure, které má řešení hello přístup.

![Azure dlaždice protokoly aktivity](./media/log-analytics-activity/azure-activity-logs-tile.png)

### <a name="view-azure-activity-logs"></a>Aktivita služby Azure zobrazení protokolů

Klikněte na tlačítko hello **protokoly aktivity Azure** dlaždice tooopen hello **protokoly aktivity Azure** řídicího panelu. řídicí panel Hello zahrnuje hello oken v hello následující tabulka. Každý okno uvádí až too10 položky odpovídající, aby na okno kritéria pro hello zadán oboru a časový rozsah. Můžete spustit vyhledávání protokolu, který vrátí všechny záznamy kliknutím **zobrazit všechny** dole hello v okně hello nebo kliknutím na záhlaví okna hello.

Data protokolu aktivity se zobrazí pouze *po* řešení aktivity protokoly toogo toohello jste nakonfigurovali tak data nelze zobrazit, předtím.

| Okno | Popis |
| --- | --- |
| Aktivita Azure položky protokolu | Ukazuje pruhový graf hello horních položka protokolu aktivita Azure záznamů součty pro hello období, které jste vybrali a seznam hello top 10 aktivity volající. Klikněte na tlačítko hello pruhový graf toorun protokolu vyhledejte <code>Type=AzureActivity</code>. Klikněte na položky toorun volající vyhledávání protokolu vrácení všech položek protokolů aktivity pro tuto položku. |
| Protokoly aktivity podle stavu | Zobrazí prstencový graf pro stav protokolu Azure aktivity hello časové období, které jste vybrali. Také ukazuje seznam seznam hello top deset stav záznamů. Klikněte na graf toorun hello protokolu vyhledejte <code>Type=AzureActivity &#124; measure count() by ActivityStatus</code>. Klikněte na položku toorun stav vyhledávání protokolu vrácení všech položek protokolů aktivity pro tento stav záznamu. |
| Protokoly aktivity podle zdroje | Zobrazí hello celkový počet prostředků s protokoly aktivity a uvádí hello top deset prostředky pomocí záznamu počty každého prostředku. Klikněte na tlačítko toorun celkový oblasti hello protokolu vyhledejte <code>Type=AzureActivity &#124; measure count() by Resource</code>, který zobrazuje všechny prostředky Azure k dispozici toohello řešení. Klikněte na tlačítko toorun prostředků vyhledávání protokolu vrátit všechny záznamy aktivity pro tento prostředek. |
| Protokoly aktivity poskytovatelem prostředků | Ukazuje hello celkový počet poskytovatelů prostředků, které produkují aktivity uvádí hello prvních deset a protokolu. Klikněte na tlačítko toorun celkový oblasti hello protokolu vyhledejte <code>Type=AzureActivity &#124; measure count() by ResourceProvider</code>, který zobrazuje všechny poskytovatele prostředků Azure. Klikněte toorun zprostředkovatele prostředků vyhledávání protokolu vrátit všechny záznamy aktivity pro zprostředkovatele hello. |

![Řídicí panel Azure protokoly aktivity](./media/log-analytics-activity/activity-log-dash.png)

## <a name="next-steps"></a>Další kroky

- Vytvoření [výstraha](log-analytics-alerts-creating.md) když se stane konkrétní aktivity.
- Použití [hledání protokolů](log-analytics-log-searches.md) tooview podrobné informace z protokolů aktivity.
