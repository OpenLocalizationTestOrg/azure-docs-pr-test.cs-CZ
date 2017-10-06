---
title: "aaaJava analýzy webových aplikací pomocí služby Azure Application Insights | Microsoft Docs"
description: "Sledování výkonu webových aplikací Java pomocí Application Insights "
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 6555ee53a44f937350e4fa296080f7dce4f45226
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a>Začínáme s Application Insights ve webovém projektu Java


[Application Insights](https://azure.microsoft.com/services/application-insights/) je rozšiřitelnou analytickou službu vývojářům webů, které vám pomůže pochopit hello výkonu a využití vaší živé aplikace. Použít příliš[najít a diagnostikovat problémy s výkonem a výjimkami](app-insights-detect-triage-diagnose.md), a [napsat kód] [ api] tootrack co uživatelé dělají s vaší aplikací.

![ukázková data](./media/app-insights-java-get-started/5-results.png)

Application Insights podporuje aplikace v Javě spuštěné v systému Linux, Unix nebo Windows.

Budete potřebovat:

* Oracle JRE 1.6 nebo novější nebo Zulu JRE 1.6 nebo novější
* Předplatné příliš[Microsoft Azure](https://azure.microsoft.com/).

*Pokud máte webovou aplikaci, která je již živá, můžete sledovat alternativní postup hello příliš[přidat hello SDK v době běhu hello webový server](app-insights-java-live.md). Tento alternativa zabraňuje opětovnému sestavení kódu hello, ale neobdržíte hello možnost toowrite kód tootrack aktivity uživatelů.*

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Získejte klíč instrumentace Application Insights
1. Přihlaste se toohello [portálu Microsoft Azure](https://portal.azure.com).
2. Vytvořte prostředek Application Insights. Nastavit hello aplikace typu tooJava webové aplikace.

    ![Zadejte název, vyberte webovou aplikaci Java a klikněte na možnost Vytvořit](./media/app-insights-java-get-started/02-create.png)
3. Najít hello klíč instrumentace nového prostředku hello. Budete potřebovat toopaste tento klíč do projektu kódu za chvíli.

    ![V hello přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte hello klíč instrumentace](./media/app-insights-java-get-started/03-key.png)

## <a name="2-add-hello-application-insights-sdk-for-java-tooyour-project"></a>2. Přidat hello Application Insights SDK pro jazyk Java tooyour projekt
*Zvolte hello vhodný způsob pro váš projekt.*

#### <a name="if-youre-using-eclipse-toocreate-a-maven-or-dynamic-web-project-"></a>Pokud používáte Eclipse toocreate Maven nebo dynamického webového projektu...
Použití hello [Application Insights SDK pro modul Java plug-in][eclipse].

#### <a name="if-youre-using-maven"></a>Pokud používáte Maven...
Pokud váš projekt již nastaven toouse Maven pro sestavení, slučte následující kód soubor pom.xml tooyour hello.

Potom závislosti projektu aktualizace hello tooget hello binární soubory stáhnout.

```XML

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[1.0,)</version>
      </dependency>
    </dependencies>
```

* *Chyby ověření sestavení nebo kontrolního součtu?* Zkuste použít konkrétní verzi, například: `<version>1.0.n</version>`. Nejnovější verzi hello najdete v hello [poznámky k verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) nebo v našich [artefaktech Maven](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *Třeba tooupdate tooa novou sadu SDK?* Obnovte závislosti svého projektu.

#### <a name="if-youre-using-gradle"></a>Pokud používáte Gradle...
Pokud váš projekt již nastaven toouse Gradle pro sestavení, slučte následující kód tooyour build.gradle soubor hello.

Potom závislosti projektu aktualizace hello tooget hello binární soubory stáhnout.

```JSON

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '1.+'
      // or applicationinsights-core for bare API
    }
```

* *Chyby ověření sestavení nebo kontrolního součtu? Zkuste použít konkrétní verzi, například:* `version:'1.0.n'`. *Nejnovější verzi hello najdete v hello [poznámky k verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).*
* *tooupdate tooa novou sadu SDK*
  * Obnovte závislosti svého projektu.

#### <a name="otherwise-"></a>V opačném případě...
Ručně přidejte hello SDK:

1. Stáhnout hello [Application Insights SDK pro jazyk Java](https://aka.ms/aijavasdk).
2. Rozbalte binární soubory hello ze souboru zip hello a přidejte tooyour projektu.

### <a name="questions"></a>Otázky...
* *Co je hello vztah mezi hello `-core` a `-web` součásti v hello zip?*

  * `applicationinsights-core`poskytuje hello úplné rozhraní API. Tuto komponentu budete vždy potřebovat.
  * `applicationinsights-web` poskytuje metriky, které sledují počty žádostí HTTP a časy odezvy. Tuto komponentu můžete vynechat, pokud nechcete automaticky shromažďovat tuto telemetrii. Pokud například chcete toowrite vlastní.
* *hello tooupdate SDK, když publikujeme změny*

  * Stáhněte si nejnovější hello [Application Insights SDK pro jazyk Java](https://aka.ms/qqkaq6) a nahradit text hello staré.
  * Změny jsou popsány v hello [poznámky k verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).

## <a name="3-add-an-application-insights-xml-file"></a>3. Vytvořte soubor Application Insights .xml
Přidejte soubor ApplicationInsights.xml toohello prostředky složku ve vašem projektu a ujistěte se, že je přidaná cesty nasazení tříd projektu tooyour. Zkopírujte následující XML do ní hello.

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
* Korelace událostí je komponenty požadavku HTTP toohello přidání. Přiřadí požadavek tooeach identifikátor přijatých hello serveru a přidá tento identifikátor jako položka tooevery vlastnost telemetrie jako hello vlastnost 'Operation.Id'. Umožňuje toocorrelate hello telemetrii související s každou žádostí nastavením filtru v [diagnostické vyhledávání][diagnostic].
* klíč Application Insights Hello možné předat dynamicky z hello portálu Azure jako vlastnost systému (-DAPPLICATION_INSIGHTS_IKEY = your_ikey). Pokud není definovaná žádná vlastnost, hledá se proměnná prostředí (APPLICATION_INSIGHTS_IKEY) v nastavení aplikace Azure. Pokud jsou obě vlastnosti hello nedefinované, použije se výchozí hello InstrumentationKey z ApplicationInsights.xml. Toto pořadí vám pomůže toomanage různých InstrumentationKeys pro různá prostředí dynamicky.

### <a name="alternative-ways-tooset-hello-instrumentation-key"></a>Klíč instrumentace hello tooset alternativní způsoby
Application Insights SDK hledá hello klíč v tomto pořadí:

1. Systémová vlastnost: -DAPPLICATION_INSIGHTS_IKEY=váš_ikey
2. Proměnná prostředí: APPLICATION_INSIGHTS_IKEY
3. Konfigurační soubor: ApplicationInsights.xml

Můžete ho taky [nastavit v kódu](app-insights-api-custom-events-metrics.md#ikey):

```Java

    telemetryClient.InstrumentationKey = "...";
```

## <a name="4-add-an-http-filter"></a>4. Přidat filtr HTTP
Hello poslední krok konfigurace umožňuje toolog komponenty požadavku HTTP hello každý webový požadavek. (Není povinné, pokud chcete úplné rozhraní API hello.)

Vyhledejte a otevřete soubor web.xml hello v projektu a merge hello následující kód pod uzlem hello webové aplikace, které jsou nakonfigurované filtry aplikace.

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

#### <a name="if-youre-using-spring-web-mvc-31-or-later"></a>Pokud používáte Spring Web MVC 3.1 nebo novější
Upravte tyto prvky v *-servlet.xml tooinclude hello Application Insights balíčku:

```XML

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>
```

#### <a name="if-youre-using-struts-2"></a>Pokud používáte Struts 2
Přidejte tuto položku toohello Struts konfigurační soubor (obvykle s názvem struts.xml nebo struts default.xml):

```XML

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />
```

(Pokud máte sběrače definované ve výchozím zásobníku, hello interceptoru jednoduše přidáním toothat zásobníku.)

## <a name="5-run-your-application"></a>5. Spusťte aplikaci
Buď spustit v režimu ladění na vývojovém počítači, nebo publikovat tooyour serveru.

## <a name="6-view-your-telemetry-in-application-insights"></a>6. Zobrazte telemetrii ve službě Application Insights
Vrátí prostředek Application Insights tooyour [portálu Microsoft Azure](https://portal.azure.com).

Data požadavků HTTP se zobrazí v okně Přehled hello. (Pokud zde nejsou, počkejte několik sekund a pak klikněte na tlačítko Aktualizovat.)

![ukázková data](./media/app-insights-java-get-started/5-results.png)

[Další informace o metrikách.][metrics]

Klikněte na tlačítko prostřednictvím jakékoli grafu toosee podrobnější agregován metriky.

![](./media/app-insights-java-get-started/6-barchart.png)

> Application Insights předpokládá hello formát požadavků HTTP pro aplikace MVC je: `VERB controller/action`. Například `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` a `GET Home/Product/sdf96vws` se seskupí do `GET Home/Product`. Toto seskupení umožňuje smysluplné agregace požadavků, jako je počet požadavků a průměrná doba provádění pro požadavky.
>
>

### <a name="instance-data"></a>Data instance
Klikněte na tlačítko prostřednictvím konkrétního požadavku zadejte toosee jednotlivých instancí.

Ve službě Application Insights se zobrazí dva druhy dat: agregovaná data, uložená a zobrazená jako průměry, počty a součty; a data instancí – jednotlivé sestavy požadavků protokolu HTTP, výjimky, zobrazení stránek nebo uživatelské události.

Při prohlížení vlastností hello požadavku, uvidíte hello telemetrické události související s například požadavky a výjimkami.

![](./media/app-insights-java-get-started/7-instance.png)

### <a name="analytics-powerful-query-language"></a>Analýzy: účinný dotazovací jazyk
Jak shromažďujete další data, můžete spouštět dotazy obou tooaggregate dat a toofind jednotlivých instancí.  [Analýzy](app-insights-analytics.md) představují výkonný nástroj jak pro vysvětlení výkonu, tak i využití a k diagnostickým účelům.

![Příklad analýz](./media/app-insights-java-get-started/025.png)

## <a name="7-install-your-app-on-hello-server"></a>7. Nainstalujte aplikaci na hello server
Teď publikujte aplikačního serveru toohello, uživatelé mohou ho použít a sledujte telemetrii hello objeví na portálu hello.

* Ujistěte se, že brána firewall umožňuje vaší aplikace toosend telemetrie toothese porty:

  * dc.services.visualstudio.com:443
  * f5.services.visualstudio.com:443

* Pokud je nutné odchozí provoz směrovat přes bránu firewall, nadefinujte systémové vlastnosti `http.proxyHost` a `http.proxyPort`.

* Na serverech Windows nainstalujte:

  * [Microsoft Visual C++ Redistributable](http://www.microsoft.com/download/details.aspx?id=40784)

    (Tato komponenta povoluje čítače výkonu.)


## <a name="exceptions-and-request-failures"></a>Výjimky a chyby požadavků
Nezpracované výjimky jsou shromažďovány automaticky:

![Otevřete Nastavení, Selhání.](./media/app-insights-java-get-started/21-exceptions.png)

toocollect data o dalších výjimkách, máte dvě možnosti:

* [Vložení volání tootrackException() ve vašem kódu][apiexceptions].
* [Nainstalujte na server agenta Java hello](app-insights-java-agent.md). Zadáte hello metody, které chcete toowatch.

## <a name="monitor-method-calls-and-external-dependencies"></a>Volání metody monitorování a externí závislosti
[Nainstalujte agenta Java hello](app-insights-java-agent.md) toolog zadané vnitřních metod a volání provedená prostřednictvím JDBC s daty časování.

## <a name="performance-counters"></a>Čítače výkonu
Otevřete **nastavení**, **servery**, toosee rozsah čítačů výkonu.

![](./media/app-insights-java-get-started/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Vlastní nastavení kolekce čítačů výkonu
kolekce toodisable hello standardní sady čítačů výkonu, přidejte následující kód pod hello kořenového uzlu souboru ApplicationInsights.xml hello hello:

```XML
    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a>Shromažďování dalších čítačů výkonu
Můžete zadat další výkonu čítače toobe shromážděných.

#### <a name="jmx-counters-exposed-by-hello-java-virtual-machine"></a>Čítače JMX (vystavené ve virtuálním počítači Java hello)

```XML
    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* `displayName`– hello název zobrazený na portálu služby Application Insights hello.
* `objectName`– Název objektu JMX hello.
* `attribute`– atribut hello toofetch název objektu JMX hello
* `type`(volitelné) – hello typ atributu JMX objektu:
  * Výchozí hodnota: jednoduchý typ, například int nebo long.
  * `composite`: data čítače výkonu hello je ve formátu "Attribute.Data" hello
  * `tabular`: data čítače výkonu hello je ve formátu hello řádku tabulky

#### <a name="windows-performance-counters"></a>Čítače výkonu Windows
Každý [čítačů výkonu systému Windows](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) je členem určité kategorie (v hello stejným způsobem, že je pole členem třídy). Kategorie mohou být buď globální, nebo mohou mít číslované nebo pojmenované instance.

```XML
    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* displayName – název hello zobrazený na portálu služby Application Insights hello.
* categoryName – hello kategorie čítače výkonu (objekt výkonu) ke kterému je přiřazeno tento čítač výkonu.
* counterName – název čítače výkonu hello hello.
* instanceName – název hello hello instance kategorie čítače výkonu nebo prázdný řetězec (""), pokud hello kategorie obsahuje jednu instanci. Pokud je hello categoryName proces a hello chcete toocollect je hodnota čítače výkonu z aktuálního procesu JVM hello na, které běží vaše aplikace, zadejte `"__SELF__"`.

Čítače výkonu jsou zobrazené jako vlastní metriky v [Průzkumníku metrik][metrics].

![](./media/app-insights-java-get-started/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Čítače výkonu Unix
* [Nainstalujte collectd s plug-in Application Insights hello](app-insights-java-collectd.md) tooget celou řadu dat systému a sítě.

## <a name="get-user-and-session-data"></a>Získejte data uživatele a relace
Takže odesíláte telemetrii z webového serveru. Nyní tooget hello úplné 360 stupňové zobrazení vaší aplikace, můžete přidat další monitorování:

* [Přidat telemetrii tooyour webové stránky] [ usage] toomonitor stránky zobrazení a metrik uživatele.
* [Nastavit testy webu] [ availability] toomake, že vaše aplikace zůstává aktivní a reagující.

## <a name="capture-log-traces"></a>Zaznamenat trasování protokolu
Můžete použít tooslice Application Insights a rozčlenění protokolů z Log4J, Logback nebo jiných rozhraní protokolování. Hello protokoly mohou korelovat s požadavky HTTP a další telemetrií. [Zjistěte jak][javalogs].

## <a name="send-your-own-telemetry"></a>Odeslat vlastní telemetrii
Teď, když jste nainstalovali hello SDK, můžete použít rozhraní API toosend hello vlastní telemetrii.

* [Sledujte vlastní události a metriky] [ api] toolearn co uživatelé dělají s vaší aplikací.
* [Hledání událostí a protokolů] [ diagnostic] toohelp diagnostikovat problémy.

## <a name="availability-web-tests"></a>Testy dostupnosti webu
Application Insights může otestovat váš web v pravidelných intervalech toocheck, zda je funkční a dobře reaguje. [tooset až][availability], klikněte na tlačítko testy webu.

![Klikněte na Webové testy a pak přidejte webový test.](./media/app-insights-java-get-started/31-config-web-test.png)

Získáte tabulky s dobami odezvy a navíc e-mailová oznámení, pokud váš web nefunguje.

![Příklad webového testu](./media/app-insights-java-get-started/appinsights-10webtestresult.png)

[Další informace o testech dostupnosti webu.][availability]

## <a name="questions-problems"></a>Máte dotazy? Problémy?
[Řešení potíží s Javou](app-insights-java-troubleshoot.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Další kroky
* [Monitorování volání závislostí](app-insights-java-agent.md)
* [Monitorování čítačů výkonu Unix](app-insights-java-collectd.md)
* Přidat [monitorování tooyour webové stránky](app-insights-javascript.md) načtení stránky toomonitor krát, volání AJAX výjimky prohlížeče.
* Zápis [vlastní telemetrii](app-insights-api-custom-events-metrics.md) tootrack využití v prohlížeči hello nebo na hello server.
* Vytvoření [řídicí panely](app-insights-dashboards.md) toobring společně hello klíče grafy pro sledování systému.
* [Analytické funkce](app-insights-analytics.md) vám pomůžou přímo z vaší aplikace zadávat efektivní dotazy na telemetrie.
* Další informace najdete na webu [Azure pro vývojáře v Javě](/java/azure).

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#trackexception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-javascript.md
