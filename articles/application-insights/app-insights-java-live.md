---
title: "aaaApplication statistiky pro jazyk Java webové aplikace, které jsou již nasazeny"
description: "Spustit monitorování webové aplikace, která je již spuštěna na serveru"
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 12f3dbb9-915f-4087-87c9-807286030b0b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/10/2016
ms.author: bwren
ms.openlocfilehash: 2b01cd61657522ccf1d2d97b2a29cdeb08ec9a18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a>Application Insights pro webové aplikace v jazyce Java, které jsou již za provozu


Pokud máte webovou aplikaci, která je již spuštěna na serveru J2EE, můžete začít monitorovat její [Application Insights](app-insights-overview.md) bez hello potřebovat toomake změny kódu nebo nové kompilace projektu. Pomocí této možnosti získáte informace o požadavcích HTTP odeslané tooyour server, neošetřených výjimek a čítače výkonu.

Budete potřebovat předplatné příliš[Microsoft Azure](https://azure.com).

> [!NOTE]
> Postup Hello na této stránce přidá hello SDK tooyour webové aplikace za běhu. Tento modul runtime instrumentace je užitečné, pokud si chcete tooupdate nebo znovu sestavte vašeho zdrojového kódu. Pokud je to možné, doporučujeme, ale [přidat hello SDK toohello zdrojový kód](app-insights-java-get-started.md) místo. Získáte další možnosti, jako je například zápis aktivity uživatelů tootrack kódu.
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Získejte klíč instrumentace Application Insights
1. Přihlaste se toohello [portálu Microsoft Azure](https://portal.azure.com)
2. Vytvořte nový prostředek Application Insights a nastavte hello aplikace typu tooJava webové aplikace.
   
    ![Zadejte název, vyberte webovou aplikaci Java a klikněte na možnost Vytvořit](./media/app-insights-java-live/02-create.png)

    Hello prostředků se vytvoří během pár sekund.

4. Otevřete hello nového prostředku a získání svůj klíč instrumentace. Budete potřebovat toopaste tento klíč do projektu kódu za chvíli.
   
    ![V hello přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte hello klíč instrumentace](./media/app-insights-java-live/03-key.png)

## <a name="2-download-hello-sdk"></a>2. Stáhnout hello SDK
1. Stáhnout hello [Application Insights SDK pro jazyk Java](https://aka.ms/aijavasdk). 
2. Na serveru extrahujte hello SDK obsah toohello adresář, ve kterém jsou načtená binární soubory vašeho projektu. Pokud používáte Tomcat, se tento adresář by obvykle v části`webapps/<your_app_name>/WEB-INF/lib`

Všimněte si, že budete potřebovat toorepeat to na každou instanci serveru a pro každou aplikaci.

## <a name="3-add-an-application-insights-xml-file"></a>3. Přidání souboru xml Application Insights
Vytvořte soubor ApplicationInsights.xml hello složky, ve které jste přidali hello SDK. Umístit do ní hello následující XML.

Nahraďte klíč instrumentace hello, který jste získali z portálu Azure hello.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- hello key from hello portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data tooeach event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

* klíč instrumentace Hello se odesílají společně s každou položkou telemetrie a říká toodisplay Application Insights je ve vašem prostředku.
* Hello požadavek komponenty HTTP je volitelný. Automaticky odesílá telemetrii týkající se požadavků a odpovědí časy toohello portálu.
* Korelace událostí je komponenty požadavku HTTP toohello přidání. Přiřadí požadavek tooeach identifikátor přijatých hello serveru a přidá tento identifikátor jako položka tooevery vlastnost telemetrie jako hello vlastnost 'Operation.Id'. Umožňuje toocorrelate hello telemetrii související s každou žádostí nastavením filtru v [diagnostické vyhledávání](app-insights-diagnostic-search.md).

## <a name="4-add-an-http-filter"></a>4. Přidat filtr HTTP
Vyhledejte a otevřete soubor web.xml hello v projektu a merge hello následující fragment kódu v rámci uzlu hello webové aplikace, které jsou nakonfigurované filtry aplikace.

tooget hello nejpřesnější výsledky, hello filtr by měly být namapované před všemi ostatními filtry.

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
```

## <a name="5-check-firewall-exceptions"></a>5. Kontrola výjimky brány firewall
Může být nutné příliš[nastavit výjimky, odchozích dat toosend](app-insights-ip-addresses.md).

## <a name="6-restart-your-web-app"></a>6. Restartování webové aplikace
## <a name="7-view-your-telemetry-in-application-insights"></a>7. Zobrazte telemetrii ve službě Application Insights
Vrátí prostředek Application Insights tooyour [portálu Microsoft Azure](https://portal.azure.com).

Telemetrická data o požadavcích HTTP se zobrazí v okně Přehled hello. (Pokud zde nejsou, počkejte několik sekund a pak klikněte na tlačítko Aktualizovat.)

![ukázková data](./media/app-insights-java-live/5-results.png)

Klikněte na tlačítko prostřednictvím jakékoli grafu toosee podrobnější metriky. 

![](./media/app-insights-java-live/6-barchart.png)

A při zobrazení vlastností hello požadavku, uvidíte hello telemetrické události související s například požadavky a výjimkami.

![](./media/app-insights-java-live/7-instance.png)

[Další informace o metrikách.](app-insights-metrics-explorer.md)

## <a name="next-steps"></a>Další kroky
* [Přidat telemetrii tooyour webové stránky](app-insights-javascript.md) toomonitor stránky zobrazení a metrik uživatele.
* [Nastavit testy webu](app-insights-monitor-web-app-availability.md) toomake, že vaše aplikace zůstává aktivní a reagující.
* [Zaznamenat trasování protokolu](app-insights-java-trace-logs.md)
* [Hledání událostí a protokolů](app-insights-diagnostic-search.md) toohelp diagnostikovat problémy.

