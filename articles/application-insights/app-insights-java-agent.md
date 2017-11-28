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
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a><span data-ttu-id="3ee48-103">Monitorování závislostí, výjimky a časy spuštění webové aplikace v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="3ee48-103">Monitor dependencies, exceptions and execution times in Java web apps</span></span>


<span data-ttu-id="3ee48-104">Pokud máte [instrumentovány webové aplikace Java pomocí Application Insights][java], můžete použít hello agenta Java tooget podrobnější přehled, beze změn kódu:</span><span class="sxs-lookup"><span data-stu-id="3ee48-104">If you have [instrumented your Java web app with Application Insights][java], you can use hello Java Agent tooget deeper insights, without any code changes:</span></span>

* <span data-ttu-id="3ee48-105">**Závislosti:** Data o voláních, která vaše aplikace provede tooother součásti, včetně:</span><span class="sxs-lookup"><span data-stu-id="3ee48-105">**Dependencies:** Data about calls that your application makes tooother components, including:</span></span>
  * <span data-ttu-id="3ee48-106">**Volání REST** provedené prostřednictvím HttpClient, OkHttp a RestTemplate (pružiny).</span><span class="sxs-lookup"><span data-stu-id="3ee48-106">**REST calls** made via HttpClient, OkHttp, and RestTemplate (Spring).</span></span>
  * <span data-ttu-id="3ee48-107">**Redis** volání provedená prostřednictvím klienta Jedis hello.</span><span class="sxs-lookup"><span data-stu-id="3ee48-107">**Redis** calls made via hello Jedis client.</span></span> <span data-ttu-id="3ee48-108">Pokud volání hello trvá déle, než 10s, načte hello agenta také hello volání argumenty.</span><span class="sxs-lookup"><span data-stu-id="3ee48-108">If hello call takes longer than 10s, hello agent also fetches hello call arguments.</span></span>
  * <span data-ttu-id="3ee48-109">**[Volání JDBC](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB nebo Apache Derby DB.</span><span class="sxs-lookup"><span data-stu-id="3ee48-109">**[JDBC calls](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB or Apache Derby DB.</span></span> <span data-ttu-id="3ee48-110">"executeBatch" hovory nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="3ee48-110">"executeBatch" calls are supported.</span></span> <span data-ttu-id="3ee48-111">MySQL a PostgreSQL Pokud hello volání trvá déle, než 10s, hello agent podřízen hello plán dotazu.</span><span class="sxs-lookup"><span data-stu-id="3ee48-111">For MySQL and PostgreSQL, if hello call takes longer than 10s, hello agent reports hello query plan.</span></span>
* <span data-ttu-id="3ee48-112">**Zachycení výjimky:** Data o výjimky, které jsou zpracovávány vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="3ee48-112">**Caught exceptions:** Data about exceptions that are handled by your code.</span></span>
* <span data-ttu-id="3ee48-113">**Metoda čas spuštění:** Data o hello čas trvá tooexecute konkrétní metody.</span><span class="sxs-lookup"><span data-stu-id="3ee48-113">**Method execution time:** Data about hello time it takes tooexecute specific methods.</span></span>

<span data-ttu-id="3ee48-114">agenta Java hello toouse, můžete jej nainstalovat na server.</span><span class="sxs-lookup"><span data-stu-id="3ee48-114">toouse hello Java agent, you install it on your server.</span></span> <span data-ttu-id="3ee48-115">Webové aplikace musí být instrumentovány s hello [Application Insights Java SDK][java].</span><span class="sxs-lookup"><span data-stu-id="3ee48-115">Your web apps must be instrumented with hello [Application Insights Java SDK][java].</span></span> 

## <a name="install-hello-application-insights-agent-for-java"></a><span data-ttu-id="3ee48-116">Nainstalujte agenta hello Application Insights pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="3ee48-116">Install hello Application Insights agent for Java</span></span>
1. <span data-ttu-id="3ee48-117">Na počítači hello systémem vašeho serveru Java, [stáhnout agenta hello](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="3ee48-117">On hello machine running your Java server, [download hello agent](https://aka.ms/aijavasdk).</span></span>
2. <span data-ttu-id="3ee48-118">Upravit hello aplikace serveru při spuštění skriptu a přidejte následující JVM hello:</span><span class="sxs-lookup"><span data-stu-id="3ee48-118">Edit hello application server startup script, and add hello following JVM:</span></span>
   
    <span data-ttu-id="3ee48-119">`javaagent:`*soubor JAR agenta toohello úplná cesta*</span><span class="sxs-lookup"><span data-stu-id="3ee48-119">`javaagent:`*full path toohello agent JAR file*</span></span>
   
    <span data-ttu-id="3ee48-120">Například v Tomcat na počítač s Linuxem:</span><span class="sxs-lookup"><span data-stu-id="3ee48-120">For example, in Tomcat on a Linux machine:</span></span>
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path tooagent JAR file>"`
3. <span data-ttu-id="3ee48-121">Restartujte server aplikace.</span><span class="sxs-lookup"><span data-stu-id="3ee48-121">Restart your application server.</span></span>

## <a name="configure-hello-agent"></a><span data-ttu-id="3ee48-122">Konfigurace agenta hello</span><span class="sxs-lookup"><span data-stu-id="3ee48-122">Configure hello agent</span></span>
<span data-ttu-id="3ee48-123">Vytvořte soubor s názvem `AI-Agent.xml` a jeho následné uložení do hello stejné složce jako soubor JAR hello agenta.</span><span class="sxs-lookup"><span data-stu-id="3ee48-123">Create a file named `AI-Agent.xml` and place it in hello same folder as hello agent JAR file.</span></span>

<span data-ttu-id="3ee48-124">Nastavit hello obsah souboru xml hello.</span><span class="sxs-lookup"><span data-stu-id="3ee48-124">Set hello content of hello xml file.</span></span> <span data-ttu-id="3ee48-125">Upravte následující příklad tooinclude hello nebo hello funkce, které chcete vynechat.</span><span class="sxs-lookup"><span data-stu-id="3ee48-125">Edit hello following example tooinclude or omit hello features you want.</span></span>

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

<span data-ttu-id="3ee48-126">Máte tooenable sestavy výjimky a metoda časování pro jednotlivé metody.</span><span class="sxs-lookup"><span data-stu-id="3ee48-126">You have tooenable reports exception and method timing for individual methods.</span></span>

<span data-ttu-id="3ee48-127">Ve výchozím nastavení `reportExecutionTime` hodnotu true a `reportCaughtExceptions` je false.</span><span class="sxs-lookup"><span data-stu-id="3ee48-127">By default, `reportExecutionTime` is true and `reportCaughtExceptions` is false.</span></span>

## <a name="view-hello-data"></a><span data-ttu-id="3ee48-128">Zobrazení dat hello</span><span class="sxs-lookup"><span data-stu-id="3ee48-128">View hello data</span></span>
<span data-ttu-id="3ee48-129">V hello prostředek Application Insights, se zobrazí agregovaný vzdálené závislostí a metoda provádění časy [pod hello výkonu dlaždici][metrics].</span><span class="sxs-lookup"><span data-stu-id="3ee48-129">In hello Application Insights resource, aggregated remote dependency and method execution times appears [under hello Performance tile][metrics].</span></span>

<span data-ttu-id="3ee48-130">Otevřete toosearch pro jednotlivé instance závislostí, výjimek a metoda sestav [vyhledávání][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="3ee48-130">toosearch for individual instances of dependency, exception, and method reports, open [Search][diagnostic].</span></span>

<span data-ttu-id="3ee48-131">[Diagnostika problémů závislostí - Další](app-insights-asp-net-dependencies.md#diagnosis).</span><span class="sxs-lookup"><span data-stu-id="3ee48-131">[Diagnosing dependency issues - learn more](app-insights-asp-net-dependencies.md#diagnosis).</span></span>

## <a name="questions-problems"></a><span data-ttu-id="3ee48-132">Máte dotazy?</span><span class="sxs-lookup"><span data-stu-id="3ee48-132">Questions?</span></span> <span data-ttu-id="3ee48-133">Problémy?</span><span class="sxs-lookup"><span data-stu-id="3ee48-133">Problems?</span></span>
* <span data-ttu-id="3ee48-134">Žádná data?</span><span class="sxs-lookup"><span data-stu-id="3ee48-134">No data?</span></span> [<span data-ttu-id="3ee48-135">Sada výjimky brány firewall</span><span class="sxs-lookup"><span data-stu-id="3ee48-135">Set firewall exceptions</span></span>](app-insights-ip-addresses.md)
* [<span data-ttu-id="3ee48-136">Řešení potíží s Javou</span><span class="sxs-lookup"><span data-stu-id="3ee48-136">Troubleshooting Java</span></span>](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
