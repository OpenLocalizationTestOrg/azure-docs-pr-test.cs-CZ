---
title: "Řešení Azure SQL analýzy v Log Analytics | Microsoft Docs"
description: "Řešení analýzy SQL Azure pomáhá spravovat vaše databáze Azure SQL."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: banders
ms.openlocfilehash: cab45cc6dd621eb4a95ef5f1842ec38c25e980b6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a>Monitorování databáze Azure SQL pomocí analýzy SQL Azure (Preview) v analýzy protokolů

![Azure SQL Analytics symbol](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Řešení Azure SQL analýzy v Azure Log Analytics shromažďuje a vizualizuje důležité metriky výkonu SQL Azure. Pomocí metriky, které shromažďujete s řešením, můžete vytvořit vlastní pravidla monitorování a výstrahy. A, můžete monitorovat Azure SQL Database a metriky elastického fondu napříč více předplatných Azure a elastického fondu a vizualizovat je. Řešení také vám pomůže identifikovat problémy v každé vrstvě zásobníku vaší aplikace.  Používá [Azure diagnostiky metriky](log-analytics-azure-storage.md) společně s zobrazení analýzy protokolů pro zobrazení dat o databází Azure SQL a elastické fondy v jedné pracovní prostor analýzy protokolů.

Toto řešení preview v současné době podporuje až 150 000 databází SQL Azure a 5 000 SQL elastické fondy podle pracovního prostoru.

Řešení Azure SQL Analytics, jako je k dispozici pro analýzy protokolů, ostatní pomáhá sledovat a přijímání oznámení o stavu vašich prostředků Azure – v tomto případě Azure SQL Database. Microsoft Azure SQL Database je služba škálovatelné relační databáze, který nabízí známé jako SQL Server – funkce pro aplikace běžící v cloudu Azure. Analýzy protokolů umožňuje shromažďovat, korelaci a vizualizovat strukturovaná i nestrukturovaná data.

## <a name="connected-sources"></a>Připojené zdroje

Řešení Azure SQL analýzy nepoužívá agentů pro připojení ke službě Analýza protokolů.

Následující tabulka popisuje připojené zdroje, které toto řešení podporuje.

| Připojený zdroj | Podpora | Popis |
| --- | --- | --- |
| [Agenti systému Windows](log-analytics-windows-agents.md) | Ne | Přímé agentů v systému Windows nejsou používány nástrojem řešení. |
| [Agenti systému Linux](log-analytics-linux-agents.md) | Ne | Přímé agenty Linux nejsou používány nástrojem řešení. |
| [Skupiny správy nástroje SCOM](log-analytics-om-agents.md) | Ne | Přímé připojení z agenta nástroje SCOM k analýze protokolů nepoužívá řešení. |
| [Účet služby Azure Storage](log-analytics-azure-storage.md) | Ne | Analýzy protokolů číst data z účtu úložiště. |
| [Azure diagnostics](log-analytics-azure-storage.md) | Ano | Azure metriky data se odešlou do Log Analytics přímo v Azure. |

## <a name="prerequisites"></a>Požadavky

- Předplatné Azure. Pokud nemáte, můžete vytvořit jeden pro [volné](https://azure.microsoft.com/free/).
- Pracovní prostor analýzy protokolů. Můžete použít existující, nebo můžete [vytvořte novou](log-analytics-get-started.md) než začnete používat tohoto řešení.
- Povolit Azure Diagnostics pro databáze Azure SQL a elastické fondy a [nakonfigurovat je pro odesílají data k analýze protokolů](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

## <a name="configuration"></a>Konfigurace

Proveďte následující postup pro přidání řešení analýzy SQL Azure do pracovního prostoru.

1. Přidat řešení analýzy SQL Azure do pracovního prostoru z [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).
2. Na portálu Azure klikněte na tlačítko **nový** (+ symbol), pak v seznamu prostředků, vyberte **monitorování + správu**.  
    ![Monitorování a správa](./media/log-analytics-azure-sql/monitoring-management.png)
3. V **monitorování + správu** seznamu klikněte na tlačítko **zobrazit všechny**.
4. V **doporučeno** seznamu, klikněte na tlačítko **Další** a pak v seznamu nové vyhledejte **analýzy SQL Azure (Preview)** a pak ho vyberte.  
    ![Řešení Azure SQL analýzy](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)
5. V **analýzy SQL Azure (Preview)** okně klikněte na tlačítko **vytvořit**.  
    ![Vytvoření](./media/log-analytics-azure-sql/portal-create.png)
6. V **vytvořte nové řešení** okně, vyberte pracovní prostor, který chcete přidat řešení a pak klikněte na tlačítko **vytvořit**.  
    ![Přidat do pracovního prostoru](./media/log-analytics-azure-sql/add-to-workspace.png)


### <a name="to-configure-multiple-azure-subscriptions"></a>Ke konfiguraci víc předplatných Azure

Pro podporu více předplatných, pomocí skriptu prostředí PowerShell z [povolit Azure prostředku metriky protokolování pomocí prostředí PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Při provádění skriptu k odesílání diagnostických dat z prostředků v rámci jednoho předplatného Azure do pracovního prostoru v jiného předplatného Azure, zadejte ID prostředku prostoru jako parametr.

**Příklad**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>Použití řešení

Když přidáte řešení do pracovního prostoru, dlaždici analýzy SQL Azure je přidat do pracovního prostoru, a zobrazí se v přehledu. Na dlaždici zobrazuje počet databází Azure SQL a Azure SQL elastické fondy, které řešení je připojen k.

![Azure SQL Analytics dlaždice](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Zobrazení dat analýzy SQL Azure

Klikněte na **analýzy SQL Azure** dlaždici otevřete řídicí panel Azure SQL Analytics. Řídicí panel obsahuje okna definovaná níže. Každý okno seznam až 15 prostředků (předplatné, server, elastického fondu a databáze). Klikněte na některý ze zdrojů otevřete řídící panel pro tento konkrétní prostředek. Elastický fond nebo databáze obsahuje grafy s metriky pro vybraný zdroj. Klikněte na graf a otevřete dialogové okno hledání protokolů.

| Okno | Popis |
|---|---|
| Předplatná | Seznam předplatných s počtem propojené servery, fondy a databáze. |
| Servery | Seznam serverů s počtem připojené fondy a databází. |
| Elastické fondy | Seznam připojených elastické fondy s maximální GB a eDTU v zjištěnou období. |
|Databáze | Seznam připojených databází s maximální GB a DTU v zjištěnou období.|


### <a name="analyze-data-and-create-alerts"></a>Analyzovat data a vytvářet výstrahy

Výstrahy můžete snadno vytvořit s dat pocházejících z prostředků Azure SQL Database. Tady je několik užitečné [hledání protokolů](log-analytics-log-searches.md) dotazy, které můžete použít pro výstrahy:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


*Vysoký počet jednotek DTU na databázi Azure SQL*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

*Vysoký počet jednotek DTU na fond elastické databáze Azure SQL*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

Tyto dotazy na základě výstrahy můžete výstrahy na specifické prahové hodnoty pro Azure SQL Database a elastické fondy. Konfigurace oznámení pro pracovní prostor OMS:

#### <a name="to-configure-an-alert-for-your-workspace"></a>Konfigurace oznámení pro pracovní prostor

1. Přejděte na [portálu OMS](http://mms.microsoft.com/) a přihlaste se.
2. Otevřete pracovní prostor, který jste nakonfigurovali pro řešení.
3. Na stránce Přehled klikněte na **analýzy SQL Azure (Preview)** dlaždici.
4. Spusťte jeden z příkladů dotazů.
5. V protokolu hledání, klikněte na **výstrahy**.  
![Vytvořit výstrahu pro vyhledávání](./media/log-analytics-azure-sql/create-alert01.png)
6. Na **přidat pravidlo výstrahy** nakonfigurujte příslušné vlastnosti a specifické prahové hodnoty, které chcete a pak klikněte na tlačítko **Uložit**.  
![Přidání pravidla výstrahy](./media/log-analytics-azure-sql/create-alert02.png)

### <a name="act-on-azure-sql-analytics-data"></a>Fungují v Azure SQL analytická data

Jako příklad jedním nejužitečnější dotazů, které můžete provádět je porovnání využití DTU pro všechny fondy elastické SQL Azure ve všech vašich předplatných Azure. Jednotky propustnosti databáze (DTU) poskytuje způsob, jak popisují relativní kapacitu úrovně výkonu databází Basic, Standard a Premium a fondy. Počet jednotek Dtu jsou založená na kombinaci měření procesoru, paměti, čte a zapisuje. Jak zvýšit počet jednotek Dtu, zvyšuje napájení, které nabízí úroveň výkonu. Například úroveň výkonu s 5 Dtu má pětkrát další výkon než úroveň výkonu s 1 DTU. Maximální kvóty DTU se vztahuje na každém serveru a elastického fondu.

Spuštěním následujícího dotazu hledání protokolů můžete snadno určit, pokud jsou nedostatečně využívá nebo přes využitím fondech elastické SQL Azure.

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak výše uvedeném dotazu by změnit na následující.
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

V následujícím příkladu vidíte, že jeden elastického fondu má vysoké využití téměř 100 % DTU jiné mají velmi malé využití. Můžete prozkoumat další řešení potenciální dopad nedávných změn ve vašem prostředí, pomocí protokolů aktivita služby Azure.

![Výsledky hledání protokolu - vysoké využití](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a>Viz také

- Použití [protokolu hledání](log-analytics-log-searches.md) v analýzy protokolů, chcete-li zobrazit podrobné dat Azure SQL.
- [Vytvořit vlastní řídicí panely](log-analytics-dashboards.md) zobrazující dat Azure SQL.
- [Vytvářet výstrahy](log-analytics-alerts.md) při výskytu určité události Azure SQL.
