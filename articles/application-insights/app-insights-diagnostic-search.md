---
title: "aaaUsing vyhledávání ve službě Azure Application Insights | Microsoft Docs"
description: "Hledat a filtr nezpracovaná telemetrická data odesílaná vaší webové aplikace."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: df2b0eb017ad48bcdc6ef57d8dff207d120143b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-search-in-application-insights"></a>Pomocí vyhledávání ve službě Application Insights
Hledání je funkce [Application Insights](app-insights-overview.md) použít toofind a prozkoumejte telemetrie jednotlivých položek, například zobrazení stránky, výjimky nebo webové požadavky. A můžete zobrazit protokol trasování a události, které mají programového.

(Pro složitější dotazy na data, použijte [Analytics](app-insights-analytics-tour.md).)

## <a name="where-do-you-see-search"></a>Kde uvidíte hledání?
### <a name="in-hello-azure-portal"></a>V hello portálu Azure
Diagnostické vyhledávání můžete otevřít explicitně v okně Přehled Statistika aplikace hello vaší aplikace:

![Otevřete vyhledávání diagnostiky](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

Otevře také po kliknutí na tlačítko prostřednictvím některé grafy a položek mřížky. V takovém případě jeho filtry jsou předem nastavené toofocus na hello typ položky, které jste vybrali. 

Například v okně Přehled hello, není pruhový graf požadavků, které jsou klasifikovány podle doby odezvy. Klikněte na tlačítko prostřednictvím toosee rozsah výkon seznam jednotlivých požadavků v tom, že rozsah doby odezvy:

![Proklikejte se žádost o výkonu](./media/app-insights-diagnostic-search/07-open-from-filters.png)

hlavní text Hello diagnostické vyhledávání je seznam položek telemetrie – požadavky na server, vyberte zobrazení, vlastních událostí, které mají programového a tak dále. V hello je horní části seznamu hello souhrnné graf zobrazující počet událostí v čase.

Klikněte na tlačítko Aktualizovat tooget nové události.

### <a name="in-visual-studio"></a>V nástroji Visual Studio

V sadě Visual Studio je také hledání Application Insights okno. Je velmi užitečné pro zobrazení telemetrie události vygenerované modulem hello aplikace, kterou ladíte. Ale může také zobrazit události hello získanou z vašich publikovaných aplikací v hello portálu Azure.

Otevřete okno vyhledávání hello v sadě Visual Studio:

![Otevřete Visual Studio Application Insights vyhledávání](./media/app-insights-diagnostic-search/32.png)

okno vyhledávání Hello má webový portál podobný toohello funkce:

![Visual Studio Application Insights vyhledávání oken](./media/app-insights-diagnostic-search/34.png)

Karta sledovat operace Hello je k dispozici, při otevření žádost nebo zobrazení stránky. ' Operace, je posloupnost událostí, který je přidružen tooa jednoho požadavku nebo stránky zobrazení. Například volání závislostí, výjimek, protokoly trasování a vlastních událostí může být součástí jedné operace. Zobrazí kartu Sledování operaci Hello graficky hello načasování a dobu trvání tyto události v vztah toohello požadavku nebo stránky zobrazení. 

## <a name="inspect-individual-items"></a>Zkontrolujte jednotlivé položky
Vyberte všechny klíče polí toosee položek telemetrie a související položky. Pokud chcete toosee hello úplnou sadu pole, klikněte na tlačítko "...". 

![Klikněte na novou pracovní položku, upravit hello pole a pak klikněte na tlačítko OK.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a>Filtrování typů událostí
Otevřete okno filtru hello a vyberte typy událostí hello se vám mají toosee. (Pokud chcete později, toorestore hello filtry, pomocí kterých můžete otevřít okno hello, klikněte na resetovat.)

![Vyberte filtr a vyberte typy telemetrie](./media/app-insights-diagnostic-search/02-filter-req.png)

typy událostí Hello jsou:

* **Trasování** - [diagnostické protokoly](app-insights-asp-net-trace-logs.md) včetně TrackTrace, log4Net, NLog a System.Diagnostic.Trace volání.
* **Žádosti o** -požadavky HTTP přijatých vaší serverové aplikace, včetně stránky, skripty, bitové kopie, soubory stylu a data. Tyto události jsou použité toocreate hello žádostí a odpovědí tabulkách Přehled.
* **Zobrazení stránky** - [Telemetrie odesílaný klientem webové hello](app-insights-javascript.md), používá toocreate stránky zobrazení sestavy. 
* **Vlastní události** – Pokud je příliš vložit volání tooTrackEvent() v pořadí[sledování využití](app-insights-api-custom-events-metrics.md), můžete je zde prohledat.
* **Výjimka** – nezachycená [výjimky v serveru hello](app-insights-asp-net-exceptions.md)a ty, které se můžete přihlásit pomocí TrackException().
* **Závislost** - [volání ze serveru aplikace](app-insights-asp-net-dependencies.md) tooother službami, jako je rozhraní REST API nebo databáze a volání AJAX z vaší [kód klienta](app-insights-javascript.md).
* **Dostupnost** -výsledky [testy dostupnosti](app-insights-monitor-web-app-availability.md).

## <a name="filter-on-property-values"></a>Filtrovat podle hodnoty vlastností
Můžete filtrovat události na hodnotách hello jejich vlastnosti. Hello dostupné vlastnosti závisí na hello typy událostí, které jste vybrali. 

Můžete například vyberte požadavků s kódem konkrétní odezvy. 

![Vlastnost rozbalte a vyberte hodnotu](./media/app-insights-diagnostic-search/03-response500.png)

Výběr žádné hodnoty určité vlastnosti má hello stejná jako volba všech hodnot, které ovlivňuje. Přepíná vypnout filtrování pro tuto vlastnost.

### <a name="narrow-your-search"></a>Upřesněte hledání
Všimněte si, že tento hello počty toohello napravo od hodnoty filtru hello zobrazit, kolik výskytů existuje jsou v aktuální sadě filtrované hello. 

V tomto příkladu je jasné, že výsledky této žádosti hello 'RTP – nebo zaměstnancům, ve většině chyb hello "500":

![Vlastnost rozbalte a vyberte hodnotu](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-hello-same-property"></a>Vyhledejte události s hello stejnou vlastnost
Najít všechny hello položky hello stejná hodnota vlastnosti:

![Klikněte pravým tlačítkem na vlastnosti](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-hello-data"></a>Data vyhledávání hello

> [!NOTE]
> toowrite složitější dotazy, otevřete [ **Analytics** ](app-insights-analytics-tour.md) shora hello hello hledání okna.
> 

Můžete vyhledat podmínky v některém z hodnot vlastností hello. To je zvlášť užitečné, pokud jste napsali [vlastních událostí](app-insights-api-custom-events-metrics.md) s hodnot vlastností. 

Můžete chtít tooset časový rozsah vyhledávání za kratší rozsah jsou stejně jako rychlejší. 

![Otevřete vyhledávání diagnostiky](./media/app-insights-diagnostic-search/appinsights-311search.png)

Hledat celá slova, není dílčích řetězců. Používejte speciální znaky uvozovek tooenclose.

| Řetězec | je *není* nalezena. | ale tyto ji najít |
| --- | --- | --- |
| HomeController.About |domácí<br/>Řadiče<br/>na více systémů | homecontroller<br/>o<br/>"homecontroller.about"|
|Spojené státy|UNI<br/>Tomáš|Spojené<br/>stavy<br/>Spojené státy a<br/>"Spojené státy"

Zde jsou hello vyhledávacích výrazech, které můžete použít:

| Ukázkový dotaz | Efekt |
| --- | --- |
| `apple` |Najde všechny události v hello časové rozmezí, jejichž pole obsahovat hello slovo "apple" |
| `apple AND banana` |Najde události, které obsahují i slova. Použít kapitálu "a", není "a". |
| `apple OR banana`<br/>`apple banana` |Najde události, které obsahují buď aplikace word. Použití "Nebo", ne "nebo".<br/>Krátký formulář. |
| `apple NOT banana` |Najít události, které obsahují, ale není hello jiných jedné aplikace. |



## <a name="sampling"></a>Vzorkování
Pokud vaše aplikace generuje mnoho telemetrie (a používáte hello sady SDK technologie ASP.NET verze 2.0.0-beta3 nebo novější), hello modul adaptivního vzorkování automaticky sníží hello svazek, který je odeslán toohello portál odesláním pouze reprezentativní části události. Ale události, které jsou související toohello stejné žádosti jsou vybrané nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi. 

[Další informace o vzorkování](app-insights-sampling.md).



## <a name="create-work-item"></a>Vytvoření pracovní položky
Chyby v Githubu nebo Visual Studio Team Services můžete vytvořit s podrobnostmi hello z libovolné položce telemetrie. 

![Klikněte na novou pracovní položku, upravit hello pole a pak klikněte na tlačítko OK.](./media/app-insights-diagnostic-search/42.png)

Hello poprvé, které použijete, se zobrazí výzva tooconfigure tooyour odkaz Team Services účet a projektu.

![Zadejte adresu URL hello serverem Team Services a hello název projektu a klikněte na autorizovat](./media/app-insights-diagnostic-search/41.png)

(Můžete také nakonfigurovat hello odkaz na okno hello pracovních položek).

## <a name="save-your-search"></a>Uložení hledání
Po nastavení všech hello filtry, které chcete, můžete hello vyhledávání uložit jako oblíbenou položku. Pokud pracujete v účet organizace, můžete vybrat zda tooshare ji s ostatními členy týmu.

![Klikněte na tlačítko Oblíbené položky, hello název sady a klikněte na Uložit](./media/app-insights-diagnostic-search/08-favorite-save.png)

toosee hello znovu vyhledávání **okno Přehled přejděte toohello** a otevřete oblíbených položek:

![Dlaždice k oblíbeným položkám.](./media/app-insights-diagnostic-search/09-favorite-get.png)

Pokud jste uložili s relativní časové rozmezí, znovu otevřít okno hello má hello nejnovější data. Pokud jste uložili s absolutní časové rozmezí, zobrazí hello pokaždé, když stejná data. (Pokud "Relativní" není dostupná, pokud chcete, aby toosave Oblíbené, klepněte časový rozsah v záhlaví hello a nastavte časový rozsah, který není vlastní rozsah.)

## <a name="send-more-telemetry-tooapplication-insights"></a>Odeslat další telemetrie tooApplication Insights
Přidání toohello out box telemetrické zprávy odesílané Application Insights SDK můžete:

* Zaznamenat trasování protokolu z vašeho oblíbeného rozhraní protokolování v [.NET](app-insights-asp-net-trace-logs.md) nebo [Java](app-insights-java-trace-logs.md). To znamená, můžete vyhledat pomocí protokolů trasování a korelovat je zobrazení stránky, výjimky a dalších událostí. 
* [Psaní kódu](app-insights-api-custom-events-metrics.md) toosend vlastních událostí, zobrazení stránek a výjimek. 

[Zjistěte, jak toosend protokoly a vlastní telemetrii tooApplication Insights](app-insights-asp-net-trace-logs.md).

## <a name="questions"></a>MODUL OTÁZKY A ODPOVĚDI
### <a name="limits"></a>Množství dat, které se uchovávají?

V tématu hello [souhrn omezení](app-insights-pricing.md#limits-summary).

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Jak lze zobrazit následná data v Moje žádosti serveru?
Jsme nemáte protokolovat hello POST data automaticky, ale můžete použít [TrackTrace nebo protokolu volání](app-insights-asp-net-trace-logs.md). Uveďte hello následná data v parametru zpráva hello. Nelze filtrovat uvítací zprávu v hello stejný způsobem můžete filtrovat podle vlastností, ale limit velikosti hello je delší.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="add"></a>Další kroky
* [Zapisovat složitých dotazů v Analytics](app-insights-analytics-tour.md)
* [Odeslání protokolů a vlastní telemetrii tooApplication Insights](app-insights-asp-net-trace-logs.md)
* [Nastavení dostupnosti a odezvy testů](app-insights-monitor-web-app-availability.md)
* [Řešení potíží](app-insights-troubleshoot-faq.md)
