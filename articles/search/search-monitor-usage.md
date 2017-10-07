---
title: "aaaMonitor využití a statistické údaje ve službě Azure Search | Microsoft Docs"
description: "Sledování spotřeby a index velikost prostředků pro službu Azure Search, hostované cloudové vyhledávací službě v Microsoft Azure."
services: search
documentationcenter: 
author: bernitorres
manager: jlembicz
editor: 
tags: azure-portal
ms.assetid: 122948de-d29a-426e-88b4-58cbcee4bc23
ms.service: search
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/01/2017
ms.author: betorres
ms.openlocfilehash: f38eabb5d04a410e11eaaff22157da8aba9e4845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-an-azure-search-service"></a>Monitorování služby Azure Search

Služba Azure Search nabízí různé prostředky pro sledování využití a výkonu služby vyhledávání. Nabízí přístup k toometrics, protokoly, index statistiky a rozšířené možnosti monitorování v Power BI. Tento článek popisuje, jak tooenable hello různé strategie monitorování a jak toointerpret hello Výsledná data.

## <a name="azure-search-metrics"></a>Azure Search metriky
Metriky získáte téměř v reálném čase přehled služby search a jsou dostupné pro všechny služby, bez další nastavení. Které vám umožní sledovat výkon hello provozu too30 dny vaší služby.

Služba Azure Search shromažďuje data pro tři různé metriky:

* Hledání latence: čas službu vyhledávání hello potřeby tooprocess vyhledávací dotazy, agregovat za minutu.
* Vyhledávací dotazy za sekundu (QPS): počet vyhledávání dotazů přijatých za sekundu, agregovat za minutu.
* Procento dotazuje omezenému vyhledávání: procento vyhledávací dotazy, které byly omezené, agregován za minutu.

![Snímek obrazovky QPS aktivity][1]

### <a name="set-up-alerts"></a>nastavení výstrah
Na stránce podrobností metriky hello můžete nakonfigurovat výstrahy tootrigger e-mailových oznámení nebo automatizované akce když metriky překračuje prahovou hodnotu, která jste definovali.

Další informace o metrikách zkontrolujte hello úplná dokumentace k Azure monitorování.  

## <a name="how-tootrack-resource-usage"></a>Jak tootrack využití prostředků
Sledování hello růstu indexy a velikosti dokumentu můžete proaktivně upravit kapacity před stiskne hello horní limit, který jste vytvořili pro vaši službu. Můžete to provést na portálu hello nebo programově pomocí rozhraní REST API hello.

### <a name="using-hello-portal"></a>Pomocí portálu hello

toomonitor využití prostředků, zobrazit hello počet a statistiky pro vaši službu v hello [portál](https://portal.azure.com).

1. Přihlaste se toohello [portál](https://portal.azure.com).
2. Otevřete řídicí panel služby hello služby Azure Search. Dlaždice pro službu hello můžete najít na domovskou stránku hello, nebo můžete procházet toohello služby z procházení na hello vlevo.

Hello oddíl využití zahrnuje monitorování, která vám ukáže, jaká část dostupné prostředky jsou právě používány. Informace o omezení za služby pro indexy, dokumenty a úložiště najdete v tématu [omezení služby](search-limits-quotas-capacity.md).

  ![Dlaždice využití][2]

> [!NOTE]
> výše uvedený snímek obrazovky Hello hello volné službou, která může mít nejvýše jednu repliku a každý oddílu a můžete pouze indexy hostitele 3, 10 000 dokumentů nebo 50 MB dat, nastane dříve. Mnohem větší limity služby jsou služby vytvořené na úroveň Basic nebo Standard. Další informace o volba úrovně najdete v tématu [vyberte úroveň nebo SKU](search-sku-tier.md).
>
>

### <a name="using-hello-rest-api"></a>Pomocí hello REST API
Hello REST API služby Azure Search a hello .NET SDK poskytují programový přístup tooservice metriky.  Pokud používáte [indexery](https://msdn.microsoft.com/library/azure/dn946891.aspx) tooload na index z databáze SQL Azure nebo Azure Cosmos DB další rozhraní API je k dispozici tooget hello čísla požadujete.

* [Získat statistiku indexu](/rest/api/searchservice/get-index-statistics)
* [Počet dokumentů](/rest/api/searchservice/count-documents)
* [Získat stav indexeru](/rest/api/searchservice/get-indexer-status)

## <a name="how-tooexport-logs-and-metrics"></a>Jak tooexport protokoly a metriky

Hello protokoly operací můžete exportovat pro služby a hello nezpracovaných dat hello metrik, které jsou popsané v předcházející části hello. Protokoly operací umožňují vědět, jak služba hello používá a může být používán z Power BI, pokud jsou data zkopírují tooa účet úložiště. Vyhledávání systému Azure poskytuje monitorování balíček obsahu Power BI pro tento účel.


### <a name="enabling-monitoring"></a>Povolení monitorování
Otevřete služby Azure Search v hello [portál Azure](http://portal.azure.com) pod hello možnost povolit monitorování.

Zvolte hello data chcete tooexport: protokoly, metriky nebo obojí. Můžete zkopírovat ho tooa účet úložiště, odešle centra událostí tooan nebo ho exportovat tooLog Analytics.

![Jak tooenable monitorování hello portálu][3]

tooenable pomocí prostředí PowerShell nebo hello Azure CLI, najdete v dokumentaci k hello [zde](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

### <a name="logs-and-metrics-schemas"></a>Protokoly a metriky schémata
Pokud hello data je zkopírovaný tooa úložiště účet, hello dat je formátován jako JSON a jeho místě v dvě kontejnerů:

* insights-logs-operationlogs: vyhledávání přenosů protokolů
* Statistika. metriky pt1m: metrik

Neexistuje jeden objekt blob, za hodinu na kontejneru.

Příklad cesty:`resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2015/m=12/d=25/h=01/m=00/name=PT1H.json`

#### <a name="log-schema"></a>Schéma protokolu
objekty BLOB Hello protokoly obsahovat protokolů přenosů služby vyhledávání.
Každý objekt blob má jeden kořenový objekt názvem **záznamy** obsahující pole objektů protokolu.
Každý objekt blob má záznamy na všechny operace hello, ke kterým došlo během hello tutéž hodinu.

| Name (Název) | Typ | Příklad | Poznámky |
| --- | --- | --- | --- |
| time |Data a času |"2015-12-07T00:00:43.6872559Z" |Časové razítko operace hello |
| resourceId |Řetězec |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>SKUPINYPROSTŘEDKŮ/VÝCHOZÍ NEBO ZPROSTŘEDKOVATELE NEBO<br/> SPOLEČNOSTI MICROSOFT. HLEDÁNÍ NEBO SEARCHSERVICES/SEARCHSERVICE" |Vaše ID prostředku |
| operationName |Řetězec |"Query.Search" |Název Hello operace hello |
| operationVersion |Řetězec |"2015-02-28" |Hello používá na verzi rozhraní api |
| category |Řetězec |"OperationLogs" |Konstantní |
| resultType |Řetězec |"ÚSPĚCH" |Možné hodnoty: úspěch nebo neúspěch |
| resultSignature |celá čísla |200 |Kód výsledku HTTP |
| durationMS |celá čísla |50 |Doba trvání operace hello v milisekundách |
| properties |Objekt |viz následující tabulka hello |Objekt obsahující data specifická pro operace |

**Vlastnosti schématu**
| Name (Název) | Typ | Příklad | Poznámky |
| --- | --- | --- | --- |
| Popis |Řetězec |"GET /indexes('content')/docs" |koncový bod Hello operace |
| Dotaz |Řetězec |"? hledání = AzureSearch & $count = true & verze api-version = 2015-02-28" |Parametry dotazu Hello |
| Dokumenty |celá čísla |42 |Počet zpracovaných dokumentů |
| indexName |Řetězec |"testindex" |Název přidružené hello operace indexu hello |

#### <a name="metrics-schema"></a>Metriky schématu
| Name (Název) | Typ | Příklad | Poznámky |
| --- | --- | --- | --- |
| resourceId |Řetězec |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>SKUPINYPROSTŘEDKŮ/VÝCHOZÍ NEBO ZPROSTŘEDKOVATELE NEBO<br/>SPOLEČNOSTI MICROSOFT. HLEDÁNÍ NEBO SEARCHSERVICES/SEARCHSERVICE" |vaše id prostředku |
| metricName |Řetězec |"Latence" |Název Hello metrika hello |
| time |Data a času |"2015-12-07T00:00:43.6872559Z" |časové razítko Hello operace |
| Průměr |celá čísla |64 |Průměrná hodnota Hello hello nezpracovaná ukázky v hello metriky časovém intervalu |
| minimální |celá čísla |37 |minimální hodnota Hello hello nezpracovaná ukázky v hello metriky časovém intervalu |
| Maximální počet |celá čísla |78 |Maximální hodnota Hello nezpracovaná ukázek hello v hello metriky časovém intervalu |
| Celkový počet |celá čísla |258 |Hello celkovou hodnotu hello nezpracovaná ukázky v hello metriky časovém intervalu |
| Počet |celá čísla |4 |číslo Hello nezpracovaná vzorků, které se používá metrika toogenerate hello |
| časovými úseky |Řetězec |"PT1M" |Hello časovým intervalem hello metriky v ISO 8601. |

Všechny metriky oznamuje v intervalech jedné minuty. Každý metrika uvádí minimální, maximální a průměrné hodnoty za minutu.

Pro metriku SearchQueriesPerSecond hello je minimální hello nejnižší hodnotu pro vyhledávací dotazy za sekundu, který byl zaregistrován během tohoto minutu. Hello totéž platí i toohello maximální hodnota. Průměrná, je hello agregační napříč celou minutu hello.
Představte si, že o tomto scénáři během jedné minuty: jednu sekundu vysokého zatížení, která je hello maximální pro SearchQueriesPerSecond, za nímž následuje 58 sekund průměrnou zátěž a nakonec jednu sekundu s pouze jeden dotaz, což je minimální hello.

Pro ThrottledSearchQueriesPercentage, minimum, maximum, průměr a celkový počet, všechny mají hello stejnou hodnotu: hello procento vyhledávací dotazy, které byly omezené z hello celkový počet vyhledávacích dotazů za jednu minutu.

## <a name="analyzing-your-data-with-power-bi"></a>Analýza dat s Power BI

Doporučujeme používat [Power BI](https://powerbi.microsoft.com) tooexplore a vizualizovat vaše data. Můžete snadno připojit ho tooyour účet úložiště Azure a rychle začít analýza vaše data.

Poskytuje službě Azure Search [balíček obsahu Power BI](https://app.powerbi.com/getdata/services/azure-search) který vám umožní toomonitor a pochopit provozu vyhledávání pomocí předdefinovaných grafů a tabulek. Obsahuje sadu sestavy Power BI, které automaticky připojení tooyour dat a nabízejí visual přehled o vaši službu vyhledávání. Další informace najdete v tématu hello [balíček obsahu stránce nápovědy](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/).

![Řídicí panel Power BI pro službu Azure Search][4]

## <a name="next-steps"></a>Další kroky
Zkontrolujte [škálování repliky a oddíly, které](search-limits-quotas-capacity.md) pokyny jak toobalance hello přidělení replik pro existující službu a oddíly.

Navštivte [Správa služby Search v Microsoft Azure](search-manage.md) Další informace o správě služby, nebo [výkonu a optimalizace](search-performance-optimization.md) pro ladění pokyny.

Další informace o vytváření úžasné sestav. V tématu [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) podrobnosti

<!--Image references-->
[1]: ./media/search-monitor-usage/AzSearch-Monitor-BarChart.PNG
[2]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG
[3]: ./media/search-monitor-usage/AzureSearch-Enable-Monitoring.PNG
[4]: ./media/search-monitor-usage/AzureSearch-PowerBI-Dashboard.png
