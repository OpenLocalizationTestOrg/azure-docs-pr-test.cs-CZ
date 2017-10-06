---
title: "Analýza aaaUsage pro webové aplikace pomocí služby Azure Application Insights | Microsoft docs"
description: "Porozumět, co dělají s vaší webové aplikace a jak pracují uživatelé."
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.openlocfilehash: f7f9173cf411fa0d2dfb3b5ba99134a02bbc0e89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="usage-analysis-for-web-applications-with-application-insights"></a>Analýza využití pro webové aplikace pomocí Application Insights

Funkce, které vaší webové aplikace je nejoblíbenější? Vaši uživatelé dosáhli svých cílů s vaší aplikací? Jejich vyřadit na konkrétní body a vracejí později?  [Azure Application Insights](app-insights-overview.md) umožňuje efektivní proniknout do jak lidí používá vaši webovou aplikaci. Při každé aktualizaci aplikace můžete vyhodnotit, jak dobře funguje pro uživatele. Replikace můžete provést rozhodnutí o další cyklech vývoj řízených daty.

## <a name="send-telemetry-from-your-app"></a>Odeslání telemetrie z vaší aplikace.

Hello co nejlepších výsledků je získat nainstalováním Application Insights v kódu aplikace serveru i na webových stránkách. Hello součásti klienta a serveru vaší aplikace odesílat telemetrii back toohello portál Azure pro analýzu.

1. **Serverový kód:** instalace hello příslušný modul pro vaše [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), nebo [jiných](app-insights-platforms.md) aplikace.

    * *Nechcete tooinstall serverový kód? Právě [vytvořte prostředek Azure Application Insights](app-insights-create-new-resource.md).*

2. **Kód webové stránky:** otevřete hello [portál Azure](https://portal.azure.com), otevřete prostředek Application Insights hello pro vaši aplikaci a pak otevřete **Začínáme > sledování a diagnostikovat Client-Side**. 

    ![Zkopírujte skript hello do hello head vaší hlavní webové stránky.](./media/app-insights-usage-overview/02-monitor-web-page.png)


3. **Získat telemetrie:** spusťte projekt v režimu ladění pro několik minut a potom vyhledejte výsledky v okně Přehled hello ve službě Application Insights.

    Publikování vaši aplikaci toomonitor výkon vaší aplikace a zjistit, co uživatelé dělají s vaší aplikací.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Zahrnout ID uživatele a relace telemetrie
Uživatelé tootrack v čase, Application Insights vyžaduje tooidentify způsob, jak je. Hello události, které je nástroj hello pouze využití nástroj, který nevyžaduje ID uživatele nebo ID relace.

Zahájit odesílání tyto identifikátory [zde](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).

## <a name="explore-usage-demographics-and-statistics"></a>Prozkoumat využití demografické údaje a statistiky
Zjistí, pokud uživatelé používají vaši aplikaci, jaké stránky, budou se nejvíce zajímá, kde se nachází vaši uživatelé, jaké prohlížeče a operační systémy používají. 

Hello uživatelů a relací sestavy filtr dat, stránky nebo vlastní události a je rozdělit pomocí vlastnosti, jako je například umístění prostředí a stránky. Můžete také přidat vlastní filtry.

![Uživatelé](./media/app-insights-usage-overview/users.png)  

Statistika na pravém hello bodu zajímavých vzorců v sadě dat hello.  

* Hello **uživatelé** sestavy počty hello počet jedinečných uživatelů, kteří přístup k vaší stránky v rámci vašich vybrané časové období. (Uživatelé, se počítají pomocí souborů cookie. Pokud někdo přistupuje k webu s jinou prohlížeče nebo klientské počítače nebo vymaže jejich souborů cookie, pak se bude počítat více než jednou.)
* Hello **relací** sestavy počty hello počet uživatelských relací, které přistupují k webu. Relace je období aktivity uživatelem, byl ukončen tečkou nečinnosti více než půl hodiny.

[Další informace o hello uživatelů, relací a události nástroje](app-insights-usage-segmentation.md)  

## <a name="page-views"></a>Zobrazení stránky

V okně hello využití klikněte na tlačítko prostřednictvím hello zobrazení stránky dlaždici tooget rozpis nejoblíbenější stránky:

![V okně Přehled hello klikněte na tlačítko hello stránky zobrazení grafu](./media/app-insights-usage-overview/05-games.png)

výše uvedený příklad Hello je z webu hry. Z hello grafy okamžitě uvidíme:

* Využití nebyl v hello vylepšené minulého týdne. Možná jsme měli zamyslet optimalizaci pro vyhledávací weby?
* Tenis je nejoblíbenější herní stránku hello. Umožňuje zaměřit se na další stránce toothis vylepšení.
* Uživatelé v průměru, navštivte stránku hello tenis o třikrát za týden. (Existují další o třikrát relace než uživatelů.)
* Většina uživatelů získáte hello serveru během hello USA pracovní týden a v pracovní době. Možná jsme by měl poskytovat tlačítko "rychlý skrýt" na webové stránce hello.
* Hello [poznámky](app-insights-annotations.md) v grafu hello zobrazit, pokud byly nasazeny nové verze hello webu. Žádná z poslední nasazení hello měl znatelný vliv na využití.

Co dělat, když chcete tooinvestigate hello tooyour provozem podrobněji, jako je rozdělení pomocí vlastní vlastnosti, které váš web odesílá v jeho telemetrická zobrazení stránky?

1. Otevřete hello **události** nástroj v nabídce prostředku Application Insights hello. Tento nástroj umožňuje analyzovat, kolik zobrazení stránky a vlastních událostí, které byly odeslány z vaší aplikace na základě různých možností filtrování, cohorting a segmentace.
2. V rozevírací nabídce "Kdo používá" hello vyberte "Any zobrazení stránky".
3. V rozevírací nabídce "Rozdělení podle" hello vyberte vlastnost, pomocí které toosplit stránku zobrazit telemetrická data.

## <a name="retention---how-many-users-come-back"></a>Uchování – počet uživatelů, kteří se vrátit?

Uchování pomáhá pochopit, jak často uživatelům vrátit toouse své aplikace založené na kohorty uživatele, kteří provést některé akce obchodní během určité časové období. 

- Pochopit, jaké konkrétní funkce způsobit uživatelé toocome zpět více než jiné 
- Hypotézy formulář na základě reálného uživatelských dat 
- Zjistěte, jestli uchování problém ve vašem produkt 

![Uchovávání](./media/app-insights-usage-overview/retention.png) 

ovládací prvky uchování Hello nahoře povolit jste toodefine konkrétních událostí a čas rozsah toocalculate uchování. Hello grafu uprostřed hello nabízí vizuální znázornění hello celkové procento uchování podle zadaný časový rozsah hello. Graf Hello na hello dolní představuje jednotlivé uchování v daném časovém období. Tato úroveň podrobností můžete toounderstand co uživatelé dělají a co může mít vliv na stávajícím uživatelům na podrobnější členitosti.  

[Informace o nástroji pro uchování hello](app-insights-usage-retention.md)

## <a name="custom-business-events"></a>Vlastní obchodní události

tooget vědět, co uživatelé dělají s vaší aplikací webové, je užitečné tooinsert řádků kódu toolog vlastních událostí. Tyto události můžete sledovat nic akcím podrobné uživatel kliknutím na konkrétní tlačítka, toomore významné obchodní události například nakupování nebo vítězství. 

I když v některých případech může představovat zobrazení stránky užitečné události, není true obecně. Uživatele můžete otevřít stránku produktu bez kdybyste kupovali hello produktu. 

S konkrétní obchodní události můžete grafu vaši uživatelé průběh svého webu. Můžete zjistit jejich předvoleb pro různé možnosti a tam, kde se vyřadit out nebo mít problémy. Replikace můžete provést informované rozhodnutí o hello priority v backlogu vývoj.

Události může být přihlášen hello webové stránky:

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

Nebo v hello hello webové aplikace na straně serveru:

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

Vlastnost hodnoty toothese události, můžete připojit, takže můžete filtrovat nebo rozdělení hello událostí v případě, že si prohlédnout hello portálu. Kromě toho je standardní sada vlastností připojené tooeach události, jako je například ID anonymního uživatele, které vám umožní tootrace hello posloupnost aktivit jednotlivé uživatele.

Další informace o [vlastních událostí](app-insights-api-custom-events-metrics.md#trackevent) a [vlastnosti](app-insights-api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Nařezání a rozčlenění události

V nástrojích hello uživatelů, relací a události můžete řezu a rozčlenění vlastních událostí podle uživatele, název události a vlastnosti.
![Uživatelé](./media/app-insights-usage-overview/users.png)  
  
## <a name="design-hello-telemetry-with-hello-app"></a>Návrh hello telemetrie aplikace hello

Při navrhování jednotlivých funkcí aplikace, zvažte, jak budete toomeasure jeho úspěch s uživateli. Rozhodněte, jaké obchodní události je třeba toorecord a kód hello sledování volání pro tyto události do vaší aplikace z hello spustit.

## <a name="a--b-testing"></a>A | Testování B
Pokud si nejste jisti, který variant funkce, která bude více úspěšné, uvolněte obou z nich, provádění každé přístupné toodifferent uživatele. Měření úspěchu hello jednotlivých a poté přesuňte tooa jednotná verze.

Pro tento postup připojte jedinečné vlastnosti hodnoty tooall hello telemetrie, které odesílají každou verzi vaší aplikace. Můžete to udělat tak, že definují vlastnosti v hello active TelemetryContext. Tyto výchozí vlastnosti jsou přidány, odešle zprávu tooevery telemetrická data, která hello aplikace – nejen vaše vlastní zprávy, ale hello standardní telemetrie.

Hello portálu Application Insights filtrovat a rozdělení dat na hodnoty vlastností hello, takže jako toocompare hello různé verze.

toodo, [nastavení inicializátoru telemetrie](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):

```C#


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

V hello webové aplikace inicializátoru například Global.asax.cs:

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

Všechny nové TelemetryClients automaticky přidají hello hodnotu vlastnosti, které určíte. Jednotlivé telemetrické události můžete přepsat výchozí hodnoty hello.

## <a name="next-steps"></a>Další kroky
   - [Uživatelé, relace, události](app-insights-usage-segmentation.md)
   - [Trychtýře](usage-funnels.md)
   - [Uchování](app-insights-usage-retention.md)
   - [Toky uživatele](app-insights-usage-flows.md)
   - [Workbooks](app-insights-usage-workbooks.md)
   - [Přidat uživatelský kontext](app-insights-usage-send-user-context.md)
