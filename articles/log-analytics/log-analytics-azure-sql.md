---
title: "aaaAzure řešení SQL analýzy v Log Analytics | Microsoft Docs"
description: "Hello řešení analýzy SQL Azure pomáhá spravovat vaše databáze Azure SQL."
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
ms.openlocfilehash: fe228bb3cb3f9d578a84707c3917f02fbeb8a627
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a>Monitorování databáze Azure SQL pomocí analýzy SQL Azure (Preview) v analýzy protokolů

![Azure SQL Analytics symbol](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Hello řešení analýzy SQL Azure v Azure Log Analytics shromažďuje a vizualizuje důležité metriky výkonu SQL Azure. Pomocí hello metriky, které shromažďujete hello řešení, můžete vytvořit vlastní pravidla monitorování a výstrahy. A, můžete monitorovat Azure SQL Database a metriky elastického fondu napříč více předplatných Azure a elastického fondu a vizualizovat je. řešení Hello také pomáhá tooidentify problémy v každé vrstvě zásobníku vaší aplikace.  Používá [Azure diagnostiky metriky](log-analytics-azure-storage.md) společně s analýzy protokolů zobrazení toopresent data o databází Azure SQL a elastické fondy v jedné pracovní prostor analýzy protokolů.

Toto řešení preview v současné době podporuje až too150 000 databází SQL Azure a 5 000 SQL elastické fondy podle pracovního prostoru.

Hello řešení analýzy SQL Azure, jako je k dispozici pro analýzy protokolů, ostatní pomáhá sledovat a přijímat oznámení o stavu hello vašich prostředků Azure – v tomto případě Azure SQL Database. Microsoft Azure SQL Database je služba škálovatelné relační databáze, která poskytuje známé tooapplications možnosti SQL. Server jako spuštěn v hello cloudu Azure. Analýzy protokolů vám pomůže toocollect, korelaci a vizualizovat strukturovaná i nestrukturovaná data.

## <a name="connected-sources"></a>Připojené zdroje

Hello řešení analýzy SQL Azure nepoužívá agenty tooconnect toohello analýzy protokolů služby.

Hello následující tabulka popisuje hello připojené zdroje, které podporuje toto řešení.

| Připojený zdroj | Podpora | Popis |
| --- | --- | --- |
| [Agenti systému Windows](log-analytics-windows-agents.md) | Ne | Přímé agentů v systému Windows nejsou používány nástrojem hello řešení. |
| [Agenti systému Linux](log-analytics-linux-agents.md) | Ne | Přímé agenty Linux nejsou používány nástrojem hello řešení. |
| [Skupiny správy nástroje SCOM](log-analytics-om-agents.md) | Ne | Přímé připojení z hello SCOM agenta tooLog Analytics není používán hello řešení. |
| [Účet služby Azure Storage](log-analytics-azure-storage.md) | Ne | Analýzy protokolů není přečíst hello data z účtu úložiště. |
| [Azure diagnostics](log-analytics-azure-storage.md) | Ano | Azure metriky data se odešlou tooLog Analytics přímo v Azure. |

## <a name="prerequisites"></a>Požadavky

- Předplatné Azure. Pokud nemáte, můžete vytvořit jeden pro [volné](https://azure.microsoft.com/free/).
- Pracovní prostor analýzy protokolů. Můžete použít existující, nebo můžete [vytvořte novou](log-analytics-get-started.md) než začnete používat tohoto řešení.
- Povolit Azure Diagnostics pro databáze Azure SQL a elastické fondy a [je nakonfigurovat toosend jejich data tooLog Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

## <a name="configuration"></a>Konfigurace

Proveďte následující kroky tooadd hello Azure SQL řešení tooyour pracovní prostor analýzy hello.

1. Přidat hello pracovní prostor tooyour řešení analýzy SQL Azure z [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).
2. V hello portálu Azure, klikněte na **nový** (hello + symbol), pak v seznamu hello prostředků, vyberte **monitorování + správu**.  
    ![Monitorování a správa](./media/log-analytics-azure-sql/monitoring-management.png)
3. V hello **monitorování + správu** seznamu klikněte na tlačítko **zobrazit všechny**.
4. V hello **doporučeno** seznamu, klikněte na tlačítko **Další** a potom v seznamu hello nové, vyhledáte **analýzy SQL Azure (Preview)** a pak ho vyberte.  
    ![Řešení Azure SQL analýzy](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)
5. V hello **analýzy SQL Azure (Preview)** okně klikněte na tlačítko **vytvořit**.  
    ![Vytvoření](./media/log-analytics-azure-sql/portal-create.png)
6. V hello **vytvořte nové řešení** okně, vyberte hello prostoru má tooadd hello řešení tooand a klikněte na **vytvořit**.  
    ![Přidat tooworkspace](./media/log-analytics-azure-sql/add-to-workspace.png)


### <a name="tooconfigure-multiple-azure-subscriptions"></a>tooconfigure víc předplatných Azure

toosupport více předplatných, pomocí skriptu prostředí PowerShell hello z [povolit Azure prostředku metriky protokolování pomocí prostředí PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Při provádění hello skriptu toosend diagnostických dat z prostředků v prostoru tooa jedno předplatné jiného předplatného Azure, zadejte ID prostředku prostoru hello jako parametr.

**Příklad**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-hello-solution"></a>Pomocí řešení hello

Když přidáte hello řešení tooyour prostoru, hello dlaždici analýzy SQL Azure je přidána tooyour prostoru a zobrazí se v přehledu. dlaždice Hello znázorňuje hello počet databází Azure SQL a Azure SQL elastické fondy, které řešení hello je připojen k.

![Azure SQL Analytics dlaždice](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Zobrazení dat analýzy SQL Azure

Klikněte na hello **Azure SQL Analytics** řídicí panel dlaždice tooopen hello analýzy SQL Azure. řídicí panel Hello zahrnuje hello okna definovaná níže. Každý okno uvádí systémové prostředky too15 (předplatné, server, elastického fondu a databáze). Klikněte na jakékoliv hello řídicí panel hello tooopen prostředky pro tento konkrétní prostředek. Elastický fond nebo databáze obsahuje hello grafy s metriky pro vybraný zdroj. Kliknutím na dialogové okno hledání protokolů hello tooopen grafu.

| Okno | Popis |
|---|---|
| Předplatná | Seznam předplatných s počtem propojené servery, fondy a databáze. |
| Servery | Seznam serverů s počtem připojené fondy a databází. |
| Elastické fondy | Seznam připojených elastické fondy s maximální GB a eDTU v hello zjištěnými období. |
|Databáze | Seznam připojených databází s maximální GB a DTU v hello zjištěnými období.|


### <a name="analyze-data-and-create-alerts"></a>Analyzovat data a vytvářet výstrahy

Výstrahy můžete snadno vytvořit s hello dat pocházejících z prostředků Azure SQL Database. Tady je několik užitečné [hledání protokolů](log-analytics-log-searches.md) dotazy, které můžete použít pro výstrahy:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


*Vysoký počet jednotek DTU na databázi Azure SQL*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

*Vysoký počet jednotek DTU na fond elastické databáze Azure SQL*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

Můžete použít tyto dotazy založené na výstrahu tooalert na specifické prahové hodnoty pro Azure SQL Database a elastické fondy. tooconfigure oznámení pro pracovní prostor OMS:

#### <a name="tooconfigure-an-alert-for-your-workspace"></a>tooconfigure výstrahu vašeho pracovního prostoru

1. Přejděte toohello [portálu OMS](http://mms.microsoft.com/) a přihlaste se.
2. Otevřete pracovní prostor hello, který jste nakonfigurovali pro řešení hello.
3. Na stránce Přehled hello klikněte hello **analýzy SQL Azure (Preview)** dlaždici.
4. Spusťte jeden z hello ukázky dotazů.
5. V protokolu hledání, klikněte na **výstrahy**.  
![Vytvořit výstrahu pro vyhledávání](./media/log-analytics-azure-sql/create-alert01.png)
6. Na hello **přidat pravidlo výstrahy** nakonfigurujte příslušné vlastnosti hello a hello specifické prahové hodnoty, které chcete a pak klikněte na tlačítko **Uložit**.  
![Přidání pravidla výstrahy](./media/log-analytics-azure-sql/create-alert02.png)

### <a name="act-on-azure-sql-analytics-data"></a>Fungují v Azure SQL analytická data

Například jeden z hello nejužitečnější dotazy, které můžete provádět toocompare hello využití DTU pro všechny fondy elastické SQL Azure je ve všech vašich předplatných Azure. Jednotky propustnosti databáze (DTU) poskytuje způsob toodescribe hello relativní kapacitu úrovně výkonu databází Basic, Standard a Premium a fondy. Počet jednotek Dtu jsou založená na kombinaci měření procesoru, paměti, čte a zapisuje. Jak zvýšit počet jednotek Dtu, hello napájení, které nabízí zvýšení úrovně výkonu hello. Například úroveň výkonu s 5 Dtu má pětkrát další výkon než úroveň výkonu s 1 DTU. Maximální kvóty DTU platí tooeach serveru a elastického fondu.

Spuštěním hello následující protokolu vyhledávací dotaz lze snadno určit, pokud jsou nedostatečně využívá nebo přes využitím fondech elastické SQL Azure.

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak hello výše dotazu by změňte následující toohello.
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

V následující ukázka hello, uvidíte, že jeden elastického fondu má vysoké využití téměř 100 % DTU jiné mají velmi malé využití. Můžete prozkoumat další tootroubleshoot potenciální nedávné změny ve vašem prostředí, pomocí protokolů aktivita služby Azure.

![Výsledky hledání protokolu - vysoké využití](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a>Viz také

- Použití [protokolu hledání](log-analytics-log-searches.md) v tooview analýzy protokolů podrobné dat Azure SQL.
- [Vytvořit vlastní řídicí panely](log-analytics-dashboards.md) zobrazující dat Azure SQL.
- [Vytvářet výstrahy](log-analytics-alerts.md) při výskytu určité události Azure SQL.
