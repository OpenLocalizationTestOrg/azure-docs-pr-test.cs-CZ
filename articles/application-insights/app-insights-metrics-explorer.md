---
title: "aaaExploring metriky ve službě Azure Application Insights | Microsoft Docs"
description: "Jak toointerpret grafy na metriky Průzkumníka a jak toocustomize metriky explorer okna."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: bwren
ms.openlocfilehash: b77ae227ae61e800ad6f3af8a05cd123ea1d69e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exploring-metrics-in-application-insights"></a>Zkoumání metriky ve službě Application Insights
Metriky v [Application Insights] [ start] jsou měřené hodnoty a počet událostí, které se odesílají v telemetrie z vaší aplikace. Pomáhají zjistit problémy s výkonem a sledujte trendy ve využití vaší aplikace. Je širokou škálu standardní metriky a můžete také vytvořit vlastní vlastní metriky a události.

Počet metriky a události se zobrazí v grafech agregované hodnoty, jako je například součtů a průměry, počty.

Zde je ukázka sadu grafy:

![](./media/app-insights-metrics-explorer/01-overview.png)

Metriky grafy všude, kde zjistíte hello portálu Application Insights. Ve většině případů lze přizpůsobit a můžete přidat další okno toohello grafy. V okně Přehled hello, klikněte na tlačítko prostřednictvím toomore podrobné grafy (která mají názvy, například "Servery"), nebo klikněte na tlačítko **Průzkumníku metrik** tooopen nové okno, kde můžete vytvořit vlastní grafy.

## <a name="time-range"></a>Časové rozmezí
Můžete změnit hello časové rozmezí předmětem hello grafy nebo mřížky na libovolné okno.

![Okno Přehled otevřete hello vaší aplikace v hello portálu Azure](./media/app-insights-metrics-explorer/03-range.png)

Pokud očekáváte data, která ještě nebyla zobrazovaly, klikněte na tlačítko Aktualizovat. Grafy sami aktualizace v intervalech, ale jsou hello intervaly delší dobu, větší časových rozsahů. Může trvat nějakou dobu toocome data prostřednictvím kanálu analýzy hello do grafu.

toozoom do části grafu, přetáhněte nad ním:

![Přetažením přes část grafu.](./media/app-insights-metrics-explorer/12-drag.png)

Klikněte na tlačítko hello vrátit zpět zvětšení tlačítko toorestore ho.

## <a name="granularity-and-point-values"></a>Hodnoty členitosti a bodu
V tomto bodě umístěte ukazatel myši nad hello grafu toodisplay hello hodnoty metrik hello.

![Najeďte hello myš graf](./media/app-insights-metrics-explorer/02-focus.png)

Hodnota Hello hello metrika na určitém místě je agregován v hello předcházející intervalu vzorkování.

interval vzorkování Hello nebo "členitosti" se zobrazí v horní části hello hello okna.

![záhlaví Hello okno.](./media/app-insights-metrics-explorer/11-grain.png)

Můžete upravit hello členitosti v okně rozsah hello čas:

![záhlaví Hello okno.](./media/app-insights-metrics-explorer/grain.png)

k dispozici Hello členitostí závisí na hello časové rozmezí, kterou vyberete. explicitní členitostí Hello jsou alternativy toohello "automatické" členitosti hello časovém rozmezí.


## <a name="editing-charts-and-grids"></a>Úpravy grafy a mřížky
tooadd nové okno toohello grafu:

![V Průzkumníku metrik zvolte přidat graf](./media/app-insights-metrics-explorer/04-add.png)

Vyberte **upravit** na graf tooedit existující nebo nové co zobrazí:

![Vyberte jeden nebo více metriky](./media/app-insights-metrics-explorer/08-select.png)

Více než jeden metrika můžete zobrazit v grafu, i když existují omezení o hello kombinace, které lze zobrazit najednou. Jakmile vyberete jeden metriku, některé hello jiné jsou zakázány.

Pokud jste programového [vlastní metriky] [ track] do vaší aplikace (volání tooTrackMetric a TrackEvent) budou uvedené v tomto poli.

## <a name="segment-your-data"></a>Segmentovat vaše data
Metriky lze rozdělit vlastnost – například toocompare stránky zobrazení na klientských počítačích s jinými operačními systémy.

Vyberte graf nebo tabulku, přepínač na seskupení a vyberte vlastnost toogroup podle:

![Vyberte seskupení na a pak sadu vyberte vlastnost v Seskupit podle](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> Pokud používáte seskupení, oblasti a pruhový graf typy hello poskytují skládaný zobrazení. Toto je vhodný, kde je metoda hello agregace Sum. Ale hello typ agregace jsou průměr, zvolte Zobrazení typů hello řádek nebo mřížky.
>
>

Pokud jste programového [vlastní metriky] [ track] do vaší aplikace a obsahují hodnoty vlastností, budete moct tooselect hello vlastnost v seznamu hello.

Graf hello je příliš malá pro segmentovaným dat? Nastavte jeho výšku:

![Upravit hello posuvníku](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a>Typy agregace
Hello legendy na stranu hello ve výchozím nastavení je obvykle zobrazena hodnota hello agregovat období hello hello grafu. Pokud je ukazatel myši nad hello grafu, zobrazí hello hodnotu v tomto bodě.

Každý datový bod v grafu hello je agregace hodnot hello dat přijatých v předchozím vzorkování interval nebo "členitosti" hello. členitost Hello se zobrazí v horní části hello hello okna a se liší podle hello celkový časový rámec hello grafu.

Metriky lze agregovat různými způsoby:

* **Počet** je počet událostí hello přijatých hello intervalu vzorkování. Používá se pro události, jako jsou žádosti o. Rozdíly v hello výška grafu hello označuje rozdíly v hello rychlost, jakou hello k událostem. Ale Všimněte si, že hello číselná hodnota se změní při změně hello intervalu vzorkování.
* **Součet** přidá hello hodnot všech datových bodů hello přijatých prostřednictvím interval vzorkování hello nebo období hello hello grafu.
* **Průměrná** vydělí hello součet hello počtem přijatých prostřednictvím hello interval datových bodů.
* **Jedinečné** počty se používají pro počty uživatelů a účty. Během intervalu vzorkování hello nebo období hello hello grafu hello obrázku hello počet různých uživatelé vidět v té době.
* **%**-Procento verze každé agregace se používají pouze s segmentovaným grafy. Celkový počet Hello vždy přidá too100 % a hello graf zobrazuje hello relativní příspěvek různé komponenty celkem.

    ![Procento agregace](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-hello-aggregation-type"></a>Změnit typ agregace hello

![Upravit hello graf a pak vyberte agregace](./media/app-insights-metrics-explorer/05-aggregation.png)

Hello výchozí metodou pro jednotlivé metriky se zobrazí, když vytvoříte nový graf nebo když vypnete všechny metriky:

![Zrušte výběr všechny metriky toosee hello výchozí hodnoty](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a>Osy y PIN kódu 
Ve výchozím nastavení graf znázorňuje hodnoty osy Y od nuly do maximální hodnoty v oblasti dat hello se toogive vizuální reprezentace quantum hello hodnot. Ale v některých případech víc než hello quantum, může to být zajímavé toovisually zkontrolovat malých změn v hodnoty. Pro přizpůsobení tuto použijte hello osy y úpravy funkce toopin hello osy y minimální nebo maximální hodnotu rozsahu na požadovanou místě.
Klikněte na políčko toobring "Upřesnit nastavení" nahoru hello osy y rozsah nastavení

![Klikněte na tlačítko Upřesnit nastavení, vyberte vlastní rozsah a zadejte maximální hodnoty min.](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a>Filtrování dat
toosee jenom hello metriky pro vybranou sadu hodnot vlastností:

![Klikněte na tlačítko filtru, rozbalte vlastnost a zkontrolujte některé hodnoty](./media/app-insights-metrics-explorer/19-filter.png)

Pokud nevyberete žádné hodnoty pro určité vlastnosti, ho má stejné hello jako jejich všechny výběrem: pro tuto vlastnost neexistuje žádný filtr.

Všimněte si hello počty událostí společně se všechny hodnoty vlastností. Když vyberete hodnoty jednu vlastnost, hello počty spolu s další vlastnosti, které hodnoty se upraví.

Filtry se používají tooall hello grafy v okně. Pokud chcete použít jiné filtry toodifferent grafy, vytvořte a uložte okna jiné metriky. Pokud chcete, můžete připnout grafy z různých oknech toohello řídicího panelu, tak, aby si mohli prohlédnout vedle sebe navzájem.

### <a name="remove-bot-and-web-test-traffic"></a>Odebrat robota a webové přenosy testu
Použití filtru hello **skutečné nebo syntetické přenosů** a zkontrolujte **skutečné**.

Můžete také filtrovat podle **zdroj syntetické provoz**.

### <a name="tooadd-properties-toohello-filter-list"></a>seznam filtrů toohello tooadd vlastnosti
Chcete, aby toofilter telemetrii na vlastního výběru kategorie? Například možná zdola nahoru vaši uživatelé do různých kategorií a chcete segmentovat vaše data podle těchto kategorií.

[Vytvořit vlastní vlastnost](app-insights-api-custom-events-metrics.md#properties). Nastavit [Telemetrie inicializátoru](app-insights-api-custom-events-metrics.md#defaults) toohave se objevit ve všech telemetrie – včetně standardní telemetrie hello poslal jiné moduly SDK.

## <a name="edit-hello-chart-type"></a>Upravit typ grafu hello
Všimněte si, že můžete přepínat mezi mřížky a grafy:

![Vyberte graf nebo mřížky, a potom vyberte typ grafu](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>Uložit okně vaší metriky
Pokud jste vytvořili některé grafy, je uložte jako oblíbenou položku. Můžete vybrat, zda tooshare ho s jinými členové týmu, pokud používáte účet organizace.

![Vyberte Oblíbené položky.](./media/app-insights-metrics-explorer/21-favorite-save.png)

toosee hello znovu okno **okno Přehled přejděte toohello** a otevřete oblíbených položek:

![V okně přehledu hello vyberte Oblíbené položky](./media/app-insights-metrics-explorer/22-favorite-get.png)

Pokud jste zvolili relativní časové rozmezí, když jste uložili, hello okno bude aktualizována hello nejnovější metriky. Pokud jste zvolili absolutní časové rozmezí, se zobrazí pokaždé, když text hello stejná data.

## <a name="reset-hello-blade"></a>Resetování hello okno
Pokud upravíte okno, ale pak byste chtěli tooget původní back toohello uložit sadu, stačí kliknout na resetovat.

![V hello tlačítka v horní části hello metrika Exploreru](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a>Datový proud za provozu metriky

Zobrazení telemetrie a víc okamžitou otevřete [živý datový proud](app-insights-live-stream.md). Většina metriky trvat několik minut tooappear kvůli hello proces agregace. Naopak za provozu metriky jsou optimalizované pro s nízkou latencí. 

## <a name="set-alerts"></a>Nastavení upozornění
toobe oznámení e-mailem neobvyklou hodnot všechny metriky přidat oznámení. Můžete buď toosend hello e-mailu toohello účet správce, nebo toospecific e-mailové adresy.

![V Průzkumníku metrik zvolte pravidla výstrah, přidejte výstrah](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

[Další informace o výstrahách][alerts].


## <a name="continuous-export"></a>Souvislý export
Pokud chcete data nepřetržitě exportovali, aby ho mohl zpracovávat externě, zvažte použití [průběžné export](app-insights-export-telemetry.md).

### <a name="power-bi"></a>Power BI
Pokud chcete, aby i bohatší zobrazení dat, můžete [exportovat tooPower BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).

## <a name="analytics"></a>Analýza
[Analýza](app-insights-analytics.md) je rozmanitější tooanalyze způsob telemetrie pomocí účinný dotazovací jazyk. Použijte jej, pokud chcete toocombine nebo výpočetní výsledky z metriky nebo provést podrobné zkoumání poslední výkon vaší aplikace. 

Z metriky grafu, můžete kliknout na hello Analytics ikonu tooget přímo toohello ekvivalentní Analytics dotazu.

## <a name="troubleshooting"></a>Řešení potíží
*V grafu se nezobrazí žádná data.*

* Filtry se používají tooall hello grafy v okně hello. Ujistěte se, že když se zaměřením na jeden graf, nebyla nastavena filtr, který vylučuje všechna data hello na jiném.

    Pokud chcete tooset různých filtrů na různých grafů, je vytvořte v různých oknech, je uložíte jako samostatné Oblíbené položky. Pokud chcete, můžete připnout je toohello řídicí panel, abyste je viděli vedle sebe navzájem.
* Graf při seskupení podle vlastnosti, která není definován na hello metrika, pak bude nic na hello grafu. Vymažte "Seskupit podle", nebo zvolte jinou seskupení vlastnost.
* Údaje o výkonu (procesoru, vstupně-výstupní operace rychlost a tak dále) je k dispozici pro webové služby, Java, desktopové aplikace systému Windows, [IIS webové aplikace a služby, pokud nainstalujete monitorování stavu](app-insights-monitor-performance-live-website-now.md), a [Azure Cloud Services](app-insights-azure.md). Není k dispozici pro weby Azure.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Další kroky
* [Sledování použití s nástrojem Application Insights](app-insights-web-track-usage.md)
* [Pomocí vyhledávání diagnostiky](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
