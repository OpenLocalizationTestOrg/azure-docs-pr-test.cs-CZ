---
title: "aaaDebug aplikace pomocí služby Azure Application Insights v sadě Visual Studio | Microsoft Docs"
description: "Analýza výkonu a diagnostika webové aplikace během ladění a v produkčním prostředí."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/7/2017
ms.author: bwren
ms.openlocfilehash: 20491fbe4505bf719039e5d1c220b1afec01db25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-applications-with-azure-application-insights-in-visual-studio"></a>Ladění aplikací pomocí služby Azure Application Insights v sadě Visual Studio
V sadě Visual Studio (2015 a novější) můžete analyzovat výkon a diagnostikovat problémy ve vaší webové aplikaci v ASP.NET během ladění i v produkčním prostředí pomocí telemetrie z [Azure Application Insights](app-insights-overview.md).

Pokud jste vytvořili webovou aplikaci ASP.NET pomocí Visual Studio 2017 nebo novější, už je hello Application Insights SDK. Jinak, pokud jste to ještě neudělali, [přidat Application Insights tooyour aplikaci](app-insights-asp-net.md).

toomonitor aplikace Pokud je v produkčním prostředí za provozu, můžete zobrazit v hello normálně telemetrie Application Insights hello [portál Azure](https://portal.azure.com), kde můžete nastavit upozornění a použít výkonných monitorovacích nástrojů. Ale pro ladění, můžete také vyhledat a analyzovat hello telemetrie v sadě Visual Studio. Telemetrie tooanalyze Visual Studio můžete použít z provozního webu i z ladění na vývojovém počítači běží. V druhém případě hello můžete analyzovat ladění spustí i v případě, že jste zatím nenakonfigurovali hello SDK toosend telemetrie toohello portálu Azure. 

## <a name="run"></a> Ladění projektu
Spusťte webovou aplikaci v režimu místního ladění pomocí klávesy F5. Otevřete různé stránky toogenerate nějaké telemetrie.

V sadě Visual Studio zobrazí počet hello události, které byly zaprotokolovány modulem hello Application Insights ve vašem projektu.

![V sadě Visual Studio zobrazí tlačítko Application Insights hello během ladění.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Klikněte na toto tlačítko toosearch telemetrie. 

## <a name="application-insights-search"></a>Hledání Application Insights
v okně hledání Application Insights Hello zobrazí události, které byly zaprotokolovány. (Pokud jste přihlášeni tooAzure při nastavení Application Insights, můžete hledat hello stejné události v hello portálu Azure.)

![Klikněte pravým tlačítkem na projekt hello a vyberte Application Insights, vyhledávání](./media/app-insights-visual-studio/34.png)

> [!NOTE] 
> Po vyberte nebo zrušte výběr filtry, klikněte na tlačítko Hledat hello na konci hello pole hledání text hello.
>

Hello textové vyhledávání funguje na všechna pole v událostech hello. Například vyhledejte část adresy URL hello stránky; nebo hello hodnotu vlastnosti, například města klienta; nebo určitá slova v protokolu trasování.

Klikněte na možnost všechny události toosee podrobné vlastnosti.

Pro žádosti o tooyour webovou aplikaci můžete kliknout na prostřednictvím toohello kódu.

![V části Podrobnosti o žádosti klikněte na tlačítko prostřednictvím toohello kódu](./media/app-insights-visual-studio/31.png)

Můžete také otevřít související položky toohelp diagnostikovat neúspěšné požadavky nebo výjimky.

![V části Podrobnosti o žádosti přejděte dolů toorelated položky](./media/app-insights-visual-studio/41.png)

## <a name="view-exceptions-and-failed-requests"></a>Zobrazit výjimky a nezdařených požadavků.
Zobrazit sestavy výjimka v okně hledání hello. (V některé starší typy aplikace ASP.NET, máte příliš[nastavili monitorování výjimek](app-insights-asp-net-exceptions.md) toosee výjimky, které jsou zpracovávány hello framework.)

Klikněte na tlačítko k výjimce tooget trasování zásobníku. Pokud je kód hello aplikace hello otevřete v sadě Visual Studio, můžete kliknutím z hello zásobník trasování toohello příslušný řádek kódu hello.

![Trasování zásobníku výjimky](./media/app-insights-visual-studio/17.png)

## <a name="view-request-and-exception-summaries-in-hello-code"></a>Zobrazit souhrny požadavku a výjimek v kódu hello
Hello řádku kódu přehledu výše každá metoda obslužná rutina se zobrazuje počet hello požadavky a výjimkami hello za posledních 24 h přihlášení pomocí Application Insights.

![Trasování zásobníku výjimky](./media/app-insights-visual-studio/21.png)

> [!NOTE] 
> Přehledu kódu zobrazuje pouze data Application Insights, pokud máte [nakonfigurované portálu služby Application Insights toohello telemetrie aplikace toosend](app-insights-asp-net.md).
>

[Další informace o Application Insights v Code Lens](app-insights-visual-studio-codelens.md)

## <a name="trends"></a>Trendy
Trendy představují nástroj pro vizualizaci chování aplikace v čase. 

Zvolte **prozkoumat trendy Telemetrie** z tlačítka panelu nástrojů Application Insights hello nebo v okně hledání Application Insights. Vyberte jednu z pěti běžné dotazy tooget spuštěna. Na základě typů telemetrie, časových rozsahů a dalších vlastností můžete analyzovat různé datové sady. 

toofind anomálie v datech, vyberte jednu z možností anomálií hello pod rozevíracího seznamu "Typ zobrazení" hello. Možnosti filtrování Hello v hello dolní části okna hello umožňují snadno toohone v na konkrétní podmnožiny telemetrie.

![Trendy](./media/app-insights-visual-studio/51.png)

[Další informace o trendech](app-insights-visual-studio-trends.md).

## <a name="local-monitoring"></a>Místní monitorování
(Z Visual Studio 2015 Update 2) Pokud jste nenakonfigurovali hello SDK toosend telemetrie toohello portál Application Insights (takže existuje v souboru ApplicationInsights.config nenachází žádný klíč instrumentace) zobrazí hello okno diagnostiky telemetrii z nejnovější relace ladění. 

Toto je žádoucí, pokud jste již publikovali předchozí verzi aplikace. Nechcete, aby hello telemetrie z vaší ladění toobe relací promíchají s telemetrií hello na hello portál Application Insights z publikované aplikace hello.

Je také užitečné, pokud máte některou [vlastní telemetrii](app-insights-api-custom-events-metrics.md) chcete toodebug před odesláním telemetrie toohello portálu.

* *Zpočátku jsem plně nakonfiguroval službu Application Insights toosend telemetrie toohello portálu. Ale nyní chcete toosee hello telemetrii pouze v sadě Visual Studio.*
  
  * V okně vyhledávání hello nastavení je možnost toosearch místní diagnostiky i v případě, že vaše aplikace odesílá telemetrii toohello portálu.
  * odesílání toohello portál, komentář hello řádku telemetrie toostop `<instrumentationkey>...` ze souboru ApplicationInsights.config. Až budete znovu připraven toosend telemetrie toohello portál, komentář zrušte.


## <a name="next-steps"></a>Další kroky
|  |  |
| --- | --- |
| **[Přidání dalších dat](app-insights-asp-net-more.md)**<br/>Sledování využití, dostupnosti, závislostí, výjimek. Integrujte trasování z rozhraní protokolování. Zapisuje vlastní telemetrii. |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| **[Práce s Application Insights portál hello](app-insights-dashboards.md)**<br/>Zobrazit řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a exportovaný telemetrická data. |![Visual Studio](./media/app-insights-visual-studio/62.png) |

