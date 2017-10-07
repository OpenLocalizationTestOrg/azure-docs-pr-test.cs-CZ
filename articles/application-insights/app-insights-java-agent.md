---
title: "aaaPerformance monitorování pro webové aplikace v jazyce Java ve službě Azure Application Insights | Microsoft Docs"
description: "Rozšířené výkon a sledování využití vašeho webu Java pomocí Application Insights."
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: bwren
ms.openlocfilehash: bf3983e3b4a16e72bc606b6468a757288d05ebaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Monitorování závislostí, výjimky a časy spuštění webové aplikace v jazyce Java


Pokud máte [instrumentovány webové aplikace Java pomocí Application Insights][java], můžete použít hello agenta Java tooget podrobnější přehled, beze změn kódu:

* **Závislosti:** Data o voláních, která vaše aplikace provede tooother součásti, včetně:
  * **Volání REST** provedené prostřednictvím HttpClient, OkHttp a RestTemplate (pružiny).
  * **Redis** volání provedená prostřednictvím klienta Jedis hello. Pokud volání hello trvá déle, než 10s, načte hello agenta také hello volání argumenty.
  * **[Volání JDBC](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB nebo Apache Derby DB. "executeBatch" hovory nejsou podporovány. MySQL a PostgreSQL Pokud hello volání trvá déle, než 10s, hello agent podřízen hello plán dotazu.
* **Zachycení výjimky:** Data o výjimky, které jsou zpracovávány vašeho kódu.
* **Metoda čas spuštění:** Data o hello čas trvá tooexecute konkrétní metody.

agenta Java hello toouse, můžete jej nainstalovat na server. Webové aplikace musí být instrumentovány s hello [Application Insights Java SDK][java]. 

## <a name="install-hello-application-insights-agent-for-java"></a>Nainstalujte agenta hello Application Insights pro jazyk Java
1. Na počítači hello systémem vašeho serveru Java, [stáhnout agenta hello](https://aka.ms/aijavasdk).
2. Upravit hello aplikace serveru při spuštění skriptu a přidejte následující JVM hello:
   
    `javaagent:`*soubor JAR agenta toohello úplná cesta*
   
    Například v Tomcat na počítač s Linuxem:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path tooagent JAR file>"`
3. Restartujte server aplikace.

## <a name="configure-hello-agent"></a>Konfigurace agenta hello
Vytvořte soubor s názvem `AI-Agent.xml` a jeho následné uložení do hello stejné složce jako soubor JAR hello agenta.

Nastavit hello obsah souboru xml hello. Upravte následující příklad tooinclude hello nebo hello funkce, které chcete vynechat.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on hello particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

Máte tooenable sestavy výjimky a metoda časování pro jednotlivé metody.

Ve výchozím nastavení `reportExecutionTime` hodnotu true a `reportCaughtExceptions` je false.

## <a name="view-hello-data"></a>Zobrazení dat hello
V hello prostředek Application Insights, se zobrazí agregovaný vzdálené závislostí a metoda provádění časy [pod hello výkonu dlaždici][metrics].

Otevřete toosearch pro jednotlivé instance závislostí, výjimek a metoda sestav [vyhledávání][diagnostic].

[Diagnostika problémů závislostí - Další](app-insights-asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Máte dotazy? Problémy?
* Žádná data? [Sada výjimky brány firewall](app-insights-ip-addresses.md)
* [Řešení potíží s Javou](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
