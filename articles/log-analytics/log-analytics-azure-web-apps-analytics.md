---
title: "aaaView analytických dat Azure Web Apps | Microsoft Docs"
description: "Hello Azure Web Apps Analytics řešení toogain insights můžete použít o webových aplikacích Azure shromažďováním jiné metriky mezi všechny prostředky webové aplikace Azure."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 20ff337f-b1a3-4696-9b5a-d39727a94220
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: banders
ms.openlocfilehash: 7e9725f95c9faf01da89184975ad5444dd19ff95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-analytic-data-for-metrics-across-all-your-azure-web-app-resources"></a>Zobrazení analýzy dat pro metriky mezi všechny prostředky webové aplikace Azure

![Symbol webové aplikace](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-symbol.png)  
Hello řešení analýzy pro webové aplikace Azure (Preview) poskytuje přehledy o vaší [Azure Web Apps](../app-service-web/app-service-web-overview.md) shromažďováním jiné metriky mezi všechny prostředky webové aplikace Azure. S řešením hello můžete analyzovat a vyhledejte metriky dat prostředků webové aplikace.

Pomocí hello řešení, můžete zobrazit:

- Hlavní webové aplikace s nejvyšším doba odezvy hello
- Počet požadavky v rámci vaší webové aplikace, včetně úspěšné i neúspěšné požadavky
- Hlavní webové aplikace s nejvyšším příchozí a odchozí provoz
- Plány nejvyšší služby s vysokou mírou využití procesoru a paměti
- Operace protokolu aktivit v Azure Web Apps

## <a name="connected-sources"></a>Připojené zdroje

Na rozdíl od většině ostatních řešení pro analýzu protokolu není data shromážděná pro Azure Web Apps pomocí agentů. Všechna data, která používá řešení hello pochází přímo z Azure.

| Připojený zdroj | Podporuje se | Popis |
| --- | --- | --- |
| [Agenti systému Windows](log-analytics-windows-agents.md) | Ne | řešení Hello neshromažďuje informace z agentů v systému Windows. |
| [Agenti systému Linux](log-analytics-linux-agents.md) | Ne | řešení Hello neshromažďuje informace od agentů systému Linux. |
| [Skupiny správy nástroje SCOM](log-analytics-om-agents.md) | Ne | řešení Hello neshromažďuje informace od agentů v připojené skupině pro správu SCOM. |
| [Účet služby Azure Storage](log-analytics-azure-storage.md) | Ne | řešení Hello nemá shromažďování informací z úložiště Azure. |

## <a name="prerequisites"></a>Požadavky

- tooaccess informace o metrikách prostředků webové aplikace Azure, musíte mít předplatné Azure.

## <a name="configuration"></a>Konfigurace

Proveďte následující kroky tooconfigure hello Azure Web Apps Analytics řešení pro vaše pracovní prostory hello.

1. Povolit řešení Azure Web Apps analýzy hello z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureWebAppsAnalyticsOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).
2. [Povolit protokolování tooOMS metriky pro prostředek Azure pomocí prostředí PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell).

Hello řešení Azure Web Apps analýzy shromažďuje dvě sady metriky z Azure:

- Metriky Azure Web Apps
  - Průměrná paměti pracovní sady
  - Průměrná doba odezvy
  - Počet přijatých nebo odeslaných bajtů
  - Čas procesoru
  - Požadavky
  - Paměť pracovní sada
  - Httpxxx
- Metriky plánu služby Service aplikace
  - Počet přijatých nebo odeslaných bajtů
  - Procento procesoru
  - Délka fronty disku
  - Délka fronty http
  - Procento paměti

Metriky plánu služby App Service jsou shromažďovány pouze pokud používáte vyhrazené tarifu. To se netýká toofree nebo sdílené plány služby App Service.

Pokud přidáte hello řešení pomocí portálu OMS hello, zobrazí se následující hello dlaždici. Je třeba příliš[povolit tooOMS protokolování metriky prostředků Azure pomocí prostředí PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell).

![Provádění Assessment oznámení](./media/log-analytics-azure-web-apps-analytics/performing-assessment.png)

Po dokončení konfigurace hello řešení, by se měl spustit dat předávaných tooyour prostoru do 15 minut.

## <a name="using-hello-solution"></a>Pomocí řešení hello

Když přidáte hello Azure Web Apps Analytics řešení tooyour prostoru, hello **Azure Web Apps Analytics** dlaždice se přidá tooyour přehled řídicí panel. Tuto dlaždici zobrazí počet hello počet Azure Web Apps má hello řešení tooin přístup vašeho předplatného Azure.

![Azure dlaždice analýzy webové aplikace](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-tile.png)

### <a name="view-azure-web-apps-analytics-information"></a>Zobrazit informace o Azure Web Apps Analytics

Klikněte na tlačítko hello **Azure Web Apps Analytics** dlaždice tooopen hello **Azure Web Apps Analytics** řídicího panelu. řídicí panel Hello zahrnuje hello oken v hello následující tabulka. Každý okno uvádí až tooten položky odpovídající, aby na okno kritéria pro hello zadán oboru a časový rozsah. Můžete spustit vyhledávání protokolu, který vrátí všechny záznamy kliknutím **zobrazit všechny** dole hello v okně hello nebo kliknutím na záhlaví okna hello.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

| Sloupec | Popis |
| --- | --- |
| Azure Webapps |   |
| Trendy webové žádosti o aplikace | Zobrazuje graf řádku hello trend žádost webové aplikace hello časové období, které jste vybrali a zobrazí se seznam hello top deset webových požadavků. Klikněte na tlačítko hello řádku grafu toorun hledat protokolu pro<code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"* (MetricName=Requests OR MetricName=Http*) &#124; measure avg(Average) by MetricName interval 1HOUR</code> <br>Klikněte na tlačítko webové žádosti položky toorun hledání protokolů pro hello webové žádosti metriky trend který požadují. |
| Doba odezvy aplikací webové | Zobrazuje spojnicový graf doby odezvy webové aplikace hello hello časové období, které jste vybrali. Také ukazuje seznam seznam hello top deset webové aplikace odpovědi časový limit. Klikněte na graf toorun hello protokolu vyhledejte<code>Type:AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"* MetricName="AverageResponseTime" &#124; measure avg(Average) by Resource interval 1HOUR</code><br> Klikněte na webovou aplikaci toorun protokolu vyhledávání vrací doby odezvy u hello webové aplikace. |
| Provoz webové aplikace | Spojnicový graf pro provoz webové aplikace se zobrazí v MB a uvádí hello horní provoz webové aplikace. Klikněte na graf toorun hello protokolu vyhledejte<code>Type:AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"*  MetricName=BytesSent OR BytesReceived &#124; measure sum(Average) by Resource interval 1HOUR</code><br> Zobrazuje všechny webové aplikace s přenosy dat pro hello poslední minutu. Klikněte na webovou aplikaci toorun vyhledávání protokolu zobrazující bajtů přijatých a odeslaných pro hello webové aplikace. |
| Plánů služby Azure App Service |   |
| S využitím výkonu procesoru plánů služby App Service &gt; 80 % | Zobrazí celkový počet hello plány služby App Service, mají využití procesoru vyšší než 80 % a seznamy hello plány služby prvních 10 App Service dle využití procesoru. Klikněte na tlačítko hello celkový oblasti toorun hledat protokolu pro<code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SERVERFARMS/"* MetricName=CpuPercentage &#124; measure Avg(Average) by Resource</code><br> Zobrazuje seznam vaše plány služby App Service a jejich průměrné využití procesoru. Klikněte plán služby App Service toorun vyhledávání protokolu zobrazující jeho průměrné využití procesoru. |
| Plánů služby App Service se využití paměti &gt; 80 % | Zobrazí celkový počet hello plány služby App Service, mají využití paměti větší, než 80 % a seznamy hello plány služby prvních 10 App Service pomocí využití paměti. Klikněte na tlačítko hello celkový oblasti toorun hledat protokolu pro<code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SERVERFARMS/"* MetricName=MemoryPercentage &#124; measure Avg(Average) by Resource</code><br> Zobrazuje seznam vaše plány služby App Service a jejich průměrné využití paměti. Klikněte plán služby App Service toorun vyhledávání protokolu zobrazující jeho průměrné využití paměti. |
| Protokoly aktivity službě Azure Web Apps |   |
| Službě Azure Web Apps aktivity auditu | Ukazuje hello celkový počet webové aplikace s [protokoly aktivity](log-analytics-activity.md) a seznamy hello operací protokolu aktivit prvních 10. Klikněte na tlačítko hello celkový oblasti toorun hledat protokolu pro<code>Type=AzureActivity ResourceProvider= "Azure Web Sites" &#124; measure count() by OperationName</code><br> Zobrazuje seznam hello operací protokolu aktivit. Klikněte na tlačítko toorun operaci protokolu aktivity hledání protokolu, které zobrazí hello záznamy pro operaci hello. |



### <a name="azure-web-apps"></a>Azure Web Apps

V řídicím panelu hello podrobnostem tooget lepší náhled na vaše webové aplikace metriky. Tato první sada okna zobrazit trend hello hello požadavků webové aplikace, počet chyb (například HTTP404), provozu a průměrná doba odezvy v čase. Také ukazuje rozpis tyto metriky pro jiné webové aplikace.

![Oken Azure Webapps](./media/log-analytics-azure-web-apps-analytics/web-apps-dash01.png)

Hlavním důvodem pro zobrazující, že data jsou tak, aby mohli identifikovat webové aplikace s vysokou odezvu a prozkoumat toofind hello hlavní příčinu. Limit prahové hodnoty je také použité toohelp jste, které informace snadno identifikovat hello těch, které jsou s problémy.

- Zobrazené červeně webových aplikací mít doba odezvy vyšší než 1 sekunda.
- Webové aplikace, které jsou uvedené v oranžová mít odezvu vyšší než 0.7 sekundu a menší než 1 sekundu.
- Zobrazené zeleně webových aplikací mít doba odezvy menší než 0,7 druhý.

V hello následující obrázek příkladu vyhledávání protokolu, uvidíte, že hello *anugup3* webové aplikace měla mnohem vyšší doba odezvy než hello jiné webové aplikace.

![Příklad protokolu vyhledávání](./media/log-analytics-azure-web-apps-analytics/web-app-search-example.png)

### <a name="app-service-plans"></a>Plány služby App Service

Pokud používáte vyhrazené plány služby, může taky shromažďovat metriky pro vaše plány služby App Service. V tomto zobrazení uvidíte vaše plány služby App Service s vysokou mírou využití procesoru nebo paměti (&gt; 80 %). Také vám ukazuje hello nejvyšší aplikační služby s vysokou mírou využití paměti nebo procesoru. Podobně limit prahové hodnoty je použité toohelp jste, které informace snadno identifikovat hello těch, které jsou s problémy.

- Plány služby App Service zobrazené červeně mít využití procesoru nebo paměti, která je vyšší než 80 %.
- Plány služby App Service, které jsou uvedené v oranžová mít využití procesoru nebo paměti, vyšší než 60 % a nižší než 80 %.
- Plány služby App Service zobrazené zeleně mít nižší než 60 % využití procesoru nebo paměti.

![Oken Azure plány služby App Service](./media/log-analytics-azure-web-apps-analytics/web-apps-dash02.png)

## <a name="azure-web-apps-log-searches"></a>Hledání protokolu Azure Web Apps

Hello **seznamu z oblíbených Azure webové aplikace vyhledávací dotazy** ukazuje všechny hello související protokoly aktivity pro webové aplikace, které poskytuje přehledy o hello operace, které byly provedeny v prostředkům webových aplikací. Také uvádí všechny hello související operace a hello počet, kolikrát jste došlo k chybě.

Pomocí kteréhokoli hello protokolu vyhledávací dotazy jako počáteční bod, můžete snadno vytvořit výstrahu. Můžete například toocreate výstrahu při metrika Průměrná doba odezvy je větší než každou 1 sekundu.

## <a name="next-steps"></a>Další kroky

- Vytvoření [výstraha](log-analytics-alerts-creating.md) pro určité metriky.
- Použití [hledání protokolů](log-analytics-log-searches.md) tooview podrobné informace z protokolů aktivity.
