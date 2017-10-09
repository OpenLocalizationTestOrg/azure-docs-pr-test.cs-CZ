---
title: "protokoly aaaExplore trasování Java v Azure Application Insights | Microsoft Docs"
description: "Hledání Log4J nebo Logback trasování ve službě Application Insights"
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: bwren
ms.openlocfilehash: e5f8e8c67e57753ba7574b97aa96dbb41db00ce1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Prozkoumejte Java protokolů trasování ve službě Application Insights
Pokud používáte Logback nebo Log4J (verze 1.2 nebo v2.0) pro trasování, může mít vaše protokoly trasování automaticky odešlou tooApplication statistiky, které vám umožní zkoumat a hledat v nich.

## <a name="install-hello-java-sdk"></a>Nainstalujte hello sady Java SDK

Nainstalujte [Application Insights SDK pro jazyk Java][java], pokud ještě neudělali.

(Pokud nechcete, aby požadavky tootrack HTTP, můžete vynechat většinu hello .xml konfigurační soubor, ale musí obsahovat alespoň hello `InstrumentationKey` elementu. Měli byste také zavolat `new TelemetryClient()` tooinitialize hello SDK.)


## <a name="add-logging-libraries-tooyour-project"></a>Přidání protokolování knihovny tooyour projektu
*Zvolte hello vhodný způsob pro váš projekt.*

#### <a name="if-youre-using-maven"></a>Pokud používáte Maven...
Pokud je váš projekt již nastaven toouse Maven pro sestavení, slučte jednu z následujících fragmentů kódu do souboru pom.xml hello.

Pak obnovte závislosti projektu hello, stažených binárních souborů tooget hello.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v2.0*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v1.2*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Pokud používáte Gradle...
Pokud váš projekt již nastaven toouse Gradle pro sestavení, přidejte jeden z následujících řádků toohello hello `dependencies` v souboru build.gradle:

Pak obnovte závislosti projektu hello, stažených binárních souborů tooget hello.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

**Log4J v2.0**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

**Log4J v1.2**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a>V opačném případě...
Stažení a extrakci odpovídající appender hello a pak přidejte hello příslušnou knihovnu tooyour projektu:

| Protokoly | Ke stažení | Knihovna |
| --- | --- | --- |
| Logback |[SDK Logback appender](https://aka.ms/xt62a4) |applicationinsights. protokolování logback |
| Log4J v2.0 |[SDK Log4J v2 appender](https://aka.ms/qypznq) |applicationinsights. protokolování log4j2 |
| Log4j v1.2 |[SDK Log4J v1.2 appender](https://aka.ms/ky9cbo) |applicationinsights. protokolování log4j1_2 |

## <a name="add-hello-appender-tooyour-logging-framework"></a>Přidání rozhraní protokolování tooyour appender hello
toostart načtení trasování, sloučení hello relevantní fragmentu kódu toohello Log4J nebo Logback konfigurační soubor: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J v2.0*

```XML

    <Configuration packages="com.microsoft.applicationinsights.Log4j">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J v1.2*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

appenders Hello Application Insights může být odkaz žádné nakonfigurované protokolovacího nástroje a nemusí protokolovač kořenové hello (jak je znázorněno v ukázky kódu hello výše).

## <a name="explore-your-traces-in-hello-application-insights-portal"></a>Prozkoumat vaše trasování portálu Application Insights hello
Nyní, když jste nakonfigurovali vašeho projektu toosend trasování tooApplication statistiky, můžete zobrazit a hledání toto trasování Application Insights portálu hello v hello [vyhledávání] [ diagnostic] okno.

![Hello portálu Application Insights otevřete vyhledávání](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>Další kroky
[Diagnostické vyhledávání][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


