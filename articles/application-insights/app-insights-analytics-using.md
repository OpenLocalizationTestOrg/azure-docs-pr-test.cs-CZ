---
title: "aaaUsing Analytics - hello vyhledávání výkonné nástroje Azure Application Insights | Microsoft Docs"
description: "Pomocí hello analýzy, hello výkonné diagnostické vyhledávání nástroje Application Insights. "
services: application-insights
documentationcenter: 
author: danhadari
manager: carmonm
ms.assetid: c3b34430-f592-4c32-b900-e9f50ca096b3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 6e0246848457db368c57d08c47b5bf73f4e5e3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-analytics-in-application-insights"></a>Pomocí analýzy ve službě Application Insights
[Analýza](app-insights-analytics.md) je výkonný vyhledávání funkcí hello [Application Insights](app-insights-overview.md). Tyto stránek popisují dotazovací jazyk analýzy protokolů.

* **[Podívejte se na úvodní video hello](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Vyzkoušejte Analytics na naše simulované data](https://analytics.applicationinsights.io/demo)**  Pokud aplikace není ještě odesílá data tooApplication statistiky.

## <a name="open-analytics"></a>Otevřete Analytics
Z domovské prostředek vaší aplikace ve službě Application Insights klikněte na tlačítko Analytics.

![Otevřete portal.azure.com otevřete prostředek Application Insights a klikněte na Analytics.](./media/app-insights-analytics-using/001.png)

Hello vložené kurzu vám dává některé nápady, o co můžete dělat.

Je [rozsáhlejší prohlídka zde](app-insights-analytics-tour.md).

## <a name="query-your-telemetry"></a>Dotaz telemetrie
### <a name="write-a-query"></a>Napsat dotaz
![Zobrazení schématu](./media/app-insights-analytics-using/150.png)

Začněte s hello názvy všech hello tabulky na levé straně hello (nebo hello [rozsah](https://docs.loganalytics.io/queryLanguage/query_language_rangeoperator.html) nebo [sjednocení](https://docs.loganalytics.io/queryLanguage/query_language_unionoperator.html) operátory). Použití `|` toocreate a kanál z [operátory](https://docs.loganalytics.io/learn/cheatsheets/useful_operators.html). 

IntelliSense zobrazí výzvu s hello operátory a hello výraz prvky, které můžete použít. Klikněte na ikonu informací o hello (nebo stiskněte klávesu CTRL + MEZERNÍK) tooget delší popis a příklady, jak toouse každý prvek.

V tématu hello [Analytics jazyk prohlídka](app-insights-analytics-tour.md) a [referenční příručka jazyka](app-insights-analytics-reference.md).

### <a name="run-a-query"></a>Spuštění dotazu
![Spuštění dotazu](./media/app-insights-analytics-using/130.png)

1. Jeden řádek zalomení můžete použít v dotazu.
2. Umístěte kurzor hello uvnitř nebo na konci hello hello dotazu, že který má toorun.
3. Zkontrolujte časové rozmezí hello tohoto dotazu. (Můžete ho změnit nebo potlačit včetně vlastní [ `where...timestamp...` ](https://docs.loganalytics.io/concepts/concepts_datatypes_timespan.html) klauzule v dotazu.)
3. Klikněte na dotaz hello toorun přejděte.
4. Nevkládejte prázdné řádky v dotazu. Několik oddělených dotazů můžete ponechat v jedné karty dotazu jejich oddělením prázdné řádky. Spuštění pouze hello dotazu, který má hello kurzoru.

### <a name="save-a-query"></a>Uložení dotazu
![Uložení dotazu](./media/app-insights-analytics-using/140.png)

1. Uložte aktuální soubor dotazů hello.
2. Otevřete soubor uložený dotaz.
3. Vytvořte nový soubor dotazu.

## <a name="see-hello-details"></a>Zobrazit podrobnosti hello
Rozbalte všechny řádek hello výsledky toosee jeho úplný seznam vlastností. Můžete dále rozšířit všechny vlastnosti, která je strukturovaných hodnota – například, vlastní dimenze nebo hello zásobníku výpis výjimku.

![Rozbalte řádek](./media/app-insights-analytics-using/070.png)

## <a name="arrange-hello-results"></a>Uspořádat výsledky hello
Můžete řadit, filtrovat, stránkování a seskupení hello výsledků vrácená z dotazu.

> [!NOTE]
> Řazení, seskupování a filtrování v prohlížeči hello nemáte spusťte znovu dotaz. Jejich pouze změna uspořádání hello výsledky, které byly vráceny poslední dotaz. 
> 
> tooperform tyto úlohy v hello serveru předtím, než budou vráceny výsledky hello, napsat dotaz s hello [řazení](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html), [shrnout](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) a [kde](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) operátory.
> 
> 

Vyberte sloupce hello byste jako toosee, přetáhněte toorearrange záhlaví sloupce a změny velikosti sloupce přetažením jejich ohraničení.

![Uspořádání sloupců](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a>Třídění a filtrování položek
Seřaďte výsledky klepnutím hello head sloupce. Klikněte na tlačítko znovu toosort hello jiným způsobem, a klikněte na třetí čas toorevert toohello původní pořadí vrácených v dotazu.

Použití hello filtrovat toonarrow ikonu hledání.

![Sloupce řadit a filtrovat.](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a>Seskupení položek
toosort pomocí seskupení více než jeden sloupec. Nejprve povolte a poté přetáhněte záhlaví sloupců do prostoru hello nad hello tabulky.

![Skupina](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a>Chybí některé výsledky?

Pokud se domníváte, že nevidíte všechny výsledky hello vašim očekáváním, existuje několik možných příčin.

* **Filtr času rozsah**. Ve výchozím nastavení zobrazí, jenom výsledky z hello posledních 24 hodin. Není automatické filtr, který omezuje hello rozsah výsledky, které jsou načteny z hello zdrojové tabulky. 

    Můžete však změnit časový rozsah hello filtr pomocí hello rozevírací nabídce.

    Nebo můžete přepsat hello automatického rozsahu včetně vlastní [ `where  ... timestamp ...` klauzule](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) do dotazu. Například:

    `requests | where timestamp > ago('2d')`

* **Limit výsledků**. Existuje limit asi 10 kB řádků na hello výsledky vrácené z portálu hello. Upozornění zobrazí, pokud přejdete přesahuje hello limit. Pokud k tomu dojde, řazení výsledky v tabulce hello vždy nezobrazí se vám všechny hello skutečné první nebo poslední výsledky. 

    Je dobrým zvykem tooavoid stiskne hello limit. Použijte hello časový rozsah filtr, nebo použijte například operátory:

  * [prvních 100 pomocí časového razítka](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) 
  * [trvat 100](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html)
  * [shrnutí](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) 
  * [kde časové razítko > ago(3d)](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html)

(Má více než 10 TIS řádků? Zvažte použití [průběžné exportovat](app-insights-export-telemetry.md) místo. Analýza je určená pro analýzy, nikoli načítání nezpracovaná data.)

## <a name="diagrams"></a>Diagramy
Vyberte typ hello diagramu, který chcete:

![Vyberte typ diagramu](./media/app-insights-analytics-using/230.png)

Pokud máte několik sloupců hello správné typy, můžete hello x a y osy a sloupec dimenze toosplit hello výsledků podle.

Ve výchozím nastavení výsledky se zpočátku zobrazují jako tabulku a vyberete hello diagram ručně. Ale můžete použít hello [vykreslení – direktiva](https://docs.loganalytics.io/queryLanguage/query_language_renderoperator.html) na konci hello tohoto dotazu tooselect diagram.

### <a name="analytics-diagnostics"></a>Analýza diagnostiky


Na timechart Pokud nečekané Špička nebo krok v datech, může se zobrazit bod zvýrazněných v řádku hello. To znamená, že Analytics diagnostiky zjistil kombinaci vlastnosti, které vyfiltrovat hello náhlé změny. Klikněte na tlačítko hello bodu tooget další podrobnosti o hello filtr a toosee hello filtrované verze. To může pomoct zjistit, jaké změny způsobeny hello. 

[Další informace o analýzy diagnostiky](app-insights-analytics-diagnostics.md)


![Analýza diagnostiky](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-toodashboard"></a>Toodashboard PIN kód
Budete moct připnout diagram nebo tabulka, tooone z vaší [sdílet řídicí panely](app-insights-dashboards.md) – stačí kliknout na hello pin. (Může být nutné příliš[upgradu vaší aplikace cenové balíček](app-insights-pricing.md) tooturn na tuto funkci.) 

![Klikněte na připnout hello](./media/app-insights-analytics-using/pin-01.png)

To znamená, že při umístění společně toohelp řídicího panelu monitorování hello výkonu a využití webových služeb, můžete zahrnout poměrně složité analysis spolu s hello jiné metriky. 

Řídicí panel toohello tabulky, můžete připnout, pokud má čtyři nebo méně sloupců. Zobrazí se pouze hello horních sedm řádků.

### <a name="dashboard-refresh"></a>Obnovení řídicího panelu
Hello grafu připnutý toohello řídicí panel se automaticky aktualizují podle opětovné spuštění dotazu hello přibližně každých hodin. Můžete také kliknutím na tlačítko Aktualizovat hello.

### <a name="automatic-simplifications"></a>Automatické zjednodušení

Některé zjednodušení jsou použité tooa grafu při připnout tooa řídicího panelu.

**Čas omezení:** dotazy jsou automaticky omezený toohello posledních 14 dnů. Hello vliv je hello stejné jako v případě, že váš dotaz obsahuje `where timestamp > ago(14d)`.

**Omezení počtu Bin:** -li zobrazit graf, který obsahuje mnoho diskrétní přihrádek (obvykle pruhový graf), hello menší vyplněná přihrádek budou automaticky seskupeny do jedné "ostatní" bin. Tento dotaz:

    requests | summarize count_search = count() by client_CountryOrRegion

Vypadá to v Analytics:

![Graf s dlouhou tail](./media/app-insights-analytics-using/pin-07.png)

ale pokud připnete ji tooa řídicí panel, vypadá takto:

![Graf s omezenou přihrádek](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-tooexcel"></a>Export tooExcel
Po spuštění dotazu, můžete stáhnout soubor .csv. Klikněte na tlačítko **exportovat, aplikace Excel**.

## <a name="export-toopower-bi"></a>Export tooPower BI
Umístěte kurzor hello v dotazu a vyberte **exportovat, Power BI**.

![Export z Analytics tooPower BI](./media/app-insights-analytics-using/240.png)

Při spuštění dotazu hello v Power BI. Můžete ho nastavit toorefresh podle plánu.

S Power BI můžete vytvořit řídicí panely, které shromažďování dat z různých zdrojů.

[Další informace o exportu tooPower BI](app-insights-export-power-bi.md)

## <a name="deep-link"></a>Přímý odkaz

Získejte odkaz v části **exportu, sdílené složky odkaz** , můžete odeslat tooanother uživatele. Zadaný uživatel hello [skupině prostředků přístup tooyour](app-insights-resources-roles-access-control.md), hello dotazu se otevřou v hello Analytics uživatelského rozhraní.

(V odkazu hello hello text dotazu se zobrazí po "? q =", gzip komprimované a s kódováním base-64. Můžete napsat kód toogenerate přímých odkazů zadat toousers. Způsob toorun Analytics z kódu je pomocí hello však doporučeno hello [REST API](https://dev.applicationinsights.io/).)


## <a name="automation"></a>Automation

Použití hello [REST API služby Data Access](https://dev.applicationinsights.io/) toorun analytické dotazy. [Například](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (pomocí prostředí PowerShell):

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

Na rozdíl od hello Analytics uživatelského rozhraní hello REST API automaticky nepřidá všechny dotazy tooyour omezení časové razítko. Mějte na paměti tooadd vlastní-klauzule where, tooavoid získávání obrovské odpovědi.



## <a name="import-data"></a>Import dat

Data můžete importovat ze souboru CSV. Typická využití je tooimport statických dat, které se můžete připojit s tabulkami z telemetrie. 

Například pokud ověřený uživatelé se budou identifikovat telemetrie alias nebo zkomolené id, může importovat tabulku, která mapuje názvy tooreal aliasy. Provedením spojení na telemetrická žádost hello můžete identifikovat uživatele, jejich skutečné názvy v sestavách analýzy hello.

### <a name="define-your-data-schema"></a>Definování schématu dat

1. Klikněte na tlačítko **nastavení** (v horní části vlevo) a potom **zdroje dat**. 
2. Přidání zdroje dat, hello pokynů. Jste vyzváni toosupply ukázku hello dat, která by měla obsahovat aspoň deset řádků. Je pak opravit hello schématu.

Definuje zdroj dat, který pak můžete použít tooimport jednotlivé tabulky.

### <a name="import-a-table"></a>Import tabulky

1. Otevřete vaše definice zdroje dat ze seznamu hello.
2. Klikněte na tlačítko "Odeslat" a postupujte podle hello pokyny tooupload hello tabulky. To zahrnuje tooa volání rozhraní REST API, a proto je snadno tooautomate. 

Vaše tabulka je nyní k dispozici pro použití v analytické dotazy. Zobrazí se vám v Analytics 

### <a name="use-hello-table"></a>Tabulka hello

Předpokládejme, že vaše definice zdroje dat se nazývá `usermap`, a že má dvě pole `realName` a `user_AuthenticatedId`. Hello `requests` tabulka má také pole s názvem `user_AuthenticatedId`, tak, aby byl snadno toojoin je:

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
Hello výsledná tabulka požadavků má sloupec, `realName`.

### <a name="import-from-logstash"></a>Importovat z LogStash

Pokud používáte [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), můžete použít tooquery analýzy protokolů. Použití hello [modul plug-in, který prostřednictvím kanálu předá data do Analytics](https://github.com/Microsoft/logstash-output-application-insights). 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

