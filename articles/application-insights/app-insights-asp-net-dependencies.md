---
title: "aaaDependency sledování ve službě Azure Application Insights | Microsoft Docs"
description: "Analýza místního využití, dostupnosti a výkonu nebo webová aplikace Microsoft Azure s nástrojem Application Insights."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: bwren
ms.openlocfilehash: e72f5465462ae8e64363cbbaa62911aff636c504
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-application-insights-dependency-tracking"></a>Nastavte Application Insights: sledování závislostí
A *závislostí* je externí komponenta, která je volána aplikace. Obvykle se jedná o službu volat pomocí protokolu HTTP, nebo databázi nebo systému souborů. [Application Insights](app-insights-overview.md) měří, jak dlouho aplikace čeká závislosti a jak často závislostí volání selže. Můžete prozkoumat konkrétní volání a propojovat je toorequests a výjimky.

![ukázkové grafy](./media/app-insights-asp-net-dependencies/10-intro.png)

monitorování závislostí na více systémů pole Hello aktuálně sestavy volání toothese typy závislosti:

* Server
  * Databáze SQL
  * Technologie ASP.NET a služby WCF, které používají vazby založené na protokolu HTTP
  * Místní nebo vzdálené volání protokolu HTTP
  * Azure Cosmos DB, table, úložiště objektů blob a fronty
* Webové stránky
  * Volání AJAX

Monitorování funguje pomocí [bajtů kód instrumentace](https://msdn.microsoft.com/library/z9z62c29.aspx) kolem vybrané metody. Nároky na výkon je minimální.

Můžete taky napsat vlastní SDK volá toomonitor Další závislosti, v hello kód klienta a serveru, i pomocí hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).

## <a name="set-up-dependency-monitoring"></a>Nastavení monitorování závislostí
Automaticky shromažďuje informace o závislostech částečné hello [Application Insights SDK](app-insights-asp-net.md). kompletní datový tooget, nainstalujte hello odpovídající agenta pro hostitelský server hello.

| Platforma | Instalace |
| --- | --- |
| Server služby IIS |Buď [nainstalujte monitorování stavu na serveru](app-insights-monitor-performance-live-website-now.md) nebo [upgradu vaší aplikace too.NET framework 4.6 nebo novější](http://go.microsoft.com/fwlink/?LinkId=528259) a nainstalujte hello [Application Insights SDK](app-insights-asp-net.md) ve vaší aplikaci. |
| Webové aplikace Azure |Ve webové aplikaci ovládacího panelu [hello otevřete okno Application Insights ve vaší webové aplikace ovládacích panelů](app-insights-azure-web-apps.md) a instalace zvolte, pokud se zobrazí výzva. |
| Cloudové služby Azure |[Úloha spuštění použití](app-insights-cloudservices.md) nebo [nainstalovat rozhraní .NET framework 4.6 +](../cloud-services/cloud-services-dotnet-install-dotnet.md) |

## <a name="where-toofind-dependency-data"></a>Kde toofind data závislostí
* [Mapa aplikace](#application-map) vizualizuje závislosti mezi aplikací a sousedních součásti.
* [Okna výkonu, prohlížeče a selhání](#performance-and-blades) zobrazit závislosti dat serveru.
* [Okno prohlížečů](#ajax-calls) ukazuje volání AJAX z prohlížečů uživatelů.
* [Klikněte na položku přes pomalé nebo chybných požadavků](#diagnose-slow-requests) volá toocheck jejich závislost.
* [Analýza](#analytics) lze použít tooquery data závislostí.

## <a name="application-map"></a>Mapa aplikace
Mapa aplikace funguje jako vizuální pomůcka toodiscovering závislosti mezi hello komponent vaší aplikace. Automaticky se generují z hello telemetrie z vaší aplikace. Tento příklad ukazuje volání AJAX z hello prohlížeč skriptů a volání REST ze serveru hello aplikace tootwo externích služeb.

![Mapa aplikace](./media/app-insights-asp-net-dependencies/08.png)

* **Přejděte z oken hello** toorelevant závislostí a jinými grafy.
* **PIN kód hello mapy** toohello [řídicí panel](app-insights-dashboards.md), kde budou plně funkční.

[Další informace](app-insights-app-map.md).

## <a name="performance-and-failure-blades"></a>Výkon a selhání oken
Hello výkonu ukazovat hello trvání závislosti volání aplikace server hello. Není souhrnné graf a tabulku oddělených volání.

![Grafy závislostí okno výkonu.](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

Proklikejte se prostřednictvím souhrnné grafy hello nebo hello tabulky položky toosearch nezpracovaná výskyty těchto volání.

![Instance volání závislostí](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

**Počet selhání** se zobrazují na hello **selhání** okno. Selhání je jakýkoli návratový kód, který není v rozsahu 200 hello-399, nebo neznámý.

> [!NOTE]
> **100 % selhání?** – Pravděpodobně to znamená, jsou jenom získávání dat částečné závislostí. Je třeba příliš[nastavit závislost monitorování platformy odpovídající tooyour](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>Volání AJAX
Doba trvání hello se zobrazí okno prohlížeče Hello a míra selhání AJAX volání ze [JavaScript na webových stránkách](app-insights-javascript.md). Zobrazí se jako závislosti.

## <a name="diagnosis"></a>Diagnostika pomalé požadavků
Každá žádost o událost souvisí s hello závislostí volání, výjimky a další události, které jsou sledovány při zpracování aplikace hello požadavku. Takže pokud chybně provedli některé požadavky, můžete zjistit ať to je z důvodu tooslow odpovědí z závislost.

Projděme příklad této.

### <a name="tracing-from-requests-toodependencies"></a>Trasování z toodependencies požadavky
Otevřete okno výkon hello a podívejte se na hello mřížky požadavků:

![Seznam požadavků s průměry a počty](./media/app-insights-asp-net-dependencies/02-reqs.png)

Hello nejvyšší, které jeden trvá velmi dlouho. Podívejme se, pokud jsme můžete zjistit, kde je hello čas strávený.

Klikněte na události jednotlivých žádostí toosee tento řádek:

![Seznam výskytů požadavku](./media/app-insights-asp-net-dependencies/03-instances.png)

Klikněte na všechny instance tooinspect dlouho běžící dál a posuňte se dolů toohello závislostí vzdáleného volání související toothis žádost:

![Najít volání tooRemote závislosti, identifikovat neobvyklé doba trvání](./media/app-insights-asp-net-dependencies/04-dependencies.png)

To vypadá většinu hello doba údržby, které tento požadavek byl stráven v místní službě tooa volání.

Vyberte tento řádek tooget Další informace:

![Klikněte na tlačítko prostřednictvím tohoto vzdáleného závislostí tooidentify hello který](./media/app-insights-asp-net-dependencies/05-detail.png)

Vypadá to jde, kde je problém hello. Jsme jste přesně vymezená problém hello, takže teď jsme právě měli toofind out proč tohoto volání trvá tak dlouho.

### <a name="request-timeline"></a>Časová osa požadavku
V případě různých je volání žádné závislosti, které je zvláště dlouhý. Ale přepnutím zobrazení časové osy toohello uvidíme, kde došlo k hello zpoždění v našem interní zpracování:

![Najít volání tooRemote závislosti, identifikovat neobvyklé doba trvání](./media/app-insights-asp-net-dependencies/04-1.png)

Vypadá to, toobe big mezera po prvním volání závislostí hello, takže jsme by měl vypadat v našem kódu toosee, proč to znamená.

### <a name="profile-your-live-site"></a>Profil živý web

Žádné představu, kde přejde hello čas? Hello [Application Insights profileru](app-insights-profiler.md) trasování HTTP volá tooyour živý web a ukazuje, které funkce ve vašem kódu trvalo hello nejdelší dobu.

## <a name="failed-requests"></a>Neúspěšné požadavky
Neúspěšné požadavky může být také přidružen toodependencies volání se nezdařilo. Jsme znovu, můžete kliknutím prostřednictvím tootrack dolů hello problém.

![Klikněte na graf neúspěšných požadavků hello](./media/app-insights-asp-net-dependencies/06-fail.png)

Klikněte na tlačítko prostřednictvím tooan výskytem chybné žádosti a podívejte se na jeho přidružené události.

![Klikněte na typ požadavku, klikněte na tlačítko hello tooget tooa různých zobrazení instance hello stejnou instanci, klikněte na podrobnosti o výjimce tooget.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a>Analýza
Můžete sledovat závislostí v hello [analýzy protokolů dotazu jazyka](https://docs.loganalytics.io/). Zde je několik příkladů:

* Najděte žádné volání se nezdařilo závislost:

```

    dependencies | where success != "True" | take 10
```

* Najděte volání AJAX:

```

    dependencies | where client_Type == "Browser" | take 10
```

* Najděte závislostí volání přidružené k žádosti:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Volání AJAX najít přidružené zobrazení stránky:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a>Vlastní závislost sledování
standardní modul sledování závislostí Hello automaticky zjišťuje externí závislosti, jako jsou databáze a rozhraní REST API. Ale můžete chtít některé další součásti toobe zpracovávají v hello stejný způsobem.

Můžete napsat kód, který odesílá informace o závislostech, pomocí stejné hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) používané standardní moduly hello.

Například pokud vytvoříte kódu se sestavením, že napíšete nebyla sami, může čas všechny tooit volání hello, případech toofind out jaký příspěvek umožňuje tooyour odpovědi. toohave tato data zobrazí v grafech závislostí hello ve službě Application Insights odeslat pomocí `TrackDependency`.

```C#

            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
            }
```

Pokud chcete tooswitch vypnout hello standardní závislost sledování modulu, odeberte hello tooDependencyTrackingTelemetryModule odkaz v [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).

## <a name="troubleshooting"></a>Řešení potíží
*Závislost úspěch příznak vždycky zobrazí hodnotu PRAVDA nebo NEPRAVDA.*

*Úplné to není znázorněné dotazu SQL.*

* Upgrade toohello nejnovější verzi hello SDK. Pokud vaše verze .NET je menší než 4.6:
  * Hostitele služby IIS: Nainstalujte [agenta Application Insights](app-insights-monitor-performance-live-website-now.md) na hostitelských serverech hello.
  * Webové aplikace Azure: Otevřete Application Insights v hello webové aplikace ovládacích panelů a nainstalujte službu Application Insights.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Další kroky
* [Výjimky](app-insights-asp-net-exceptions.md)
* [Data uživatele a stránky](app-insights-javascript.md)
* [Dostupnost](app-insights-monitor-web-app-availability.md)
