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
# <a name="explore-java-trace-logs-in-application-insights"></a><span data-ttu-id="17837-103">Prozkoumejte Java protokolů trasování ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="17837-103">Explore Java trace logs in Application Insights</span></span>
<span data-ttu-id="17837-104">Pokud používáte Logback nebo Log4J (verze 1.2 nebo v2.0) pro trasování, může mít vaše protokoly trasování automaticky odešlou tooApplication statistiky, které vám umožní zkoumat a hledat v nich.</span><span class="sxs-lookup"><span data-stu-id="17837-104">If you're using Logback or Log4J (v1.2 or v2.0) for tracing, you can have your trace logs sent automatically tooApplication Insights where you can explore and search on them.</span></span>

## <a name="install-hello-java-sdk"></a><span data-ttu-id="17837-105">Nainstalujte hello sady Java SDK</span><span class="sxs-lookup"><span data-stu-id="17837-105">Install hello Java SDK</span></span>

<span data-ttu-id="17837-106">Nainstalujte [Application Insights SDK pro jazyk Java][java], pokud ještě neudělali.</span><span class="sxs-lookup"><span data-stu-id="17837-106">Install [Application Insights SDK for Java][java], if you haven't already done that.</span></span>

<span data-ttu-id="17837-107">(Pokud nechcete, aby požadavky tootrack HTTP, můžete vynechat většinu hello .xml konfigurační soubor, ale musí obsahovat alespoň hello `InstrumentationKey` elementu.</span><span class="sxs-lookup"><span data-stu-id="17837-107">(If you don't want tootrack HTTP requests, you can omit most of hello .xml configuration file, but you must at least include hello `InstrumentationKey` element.</span></span> <span data-ttu-id="17837-108">Měli byste také zavolat `new TelemetryClient()` tooinitialize hello SDK.)</span><span class="sxs-lookup"><span data-stu-id="17837-108">You should also call `new TelemetryClient()` tooinitialize hello SDK.)</span></span>


## <a name="add-logging-libraries-tooyour-project"></a><span data-ttu-id="17837-109">Přidání protokolování knihovny tooyour projektu</span><span class="sxs-lookup"><span data-stu-id="17837-109">Add logging libraries tooyour project</span></span>
<span data-ttu-id="17837-110">*Zvolte hello vhodný způsob pro váš projekt.*</span><span class="sxs-lookup"><span data-stu-id="17837-110">*Choose hello appropriate way for your project.*</span></span>

#### <a name="if-youre-using-maven"></a><span data-ttu-id="17837-111">Pokud používáte Maven...</span><span class="sxs-lookup"><span data-stu-id="17837-111">If you're using Maven...</span></span>
<span data-ttu-id="17837-112">Pokud je váš projekt již nastaven toouse Maven pro sestavení, slučte jednu z následujících fragmentů kódu do souboru pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="17837-112">If your project is already set up toouse Maven for build, merge one of hello following snippets of code into your pom.xml file.</span></span>

<span data-ttu-id="17837-113">Pak obnovte závislosti projektu hello, stažených binárních souborů tooget hello.</span><span class="sxs-lookup"><span data-stu-id="17837-113">Then refresh hello project dependencies, tooget hello binaries downloaded.</span></span>

<span data-ttu-id="17837-114">*Logback*</span><span class="sxs-lookup"><span data-stu-id="17837-114">*Logback*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

<span data-ttu-id="17837-115">*Log4J v2.0*</span><span class="sxs-lookup"><span data-stu-id="17837-115">*Log4J v2.0*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

<span data-ttu-id="17837-116">*Log4J v1.2*</span><span class="sxs-lookup"><span data-stu-id="17837-116">*Log4J v1.2*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a><span data-ttu-id="17837-117">Pokud používáte Gradle...</span><span class="sxs-lookup"><span data-stu-id="17837-117">If you're using Gradle...</span></span>
<span data-ttu-id="17837-118">Pokud váš projekt již nastaven toouse Gradle pro sestavení, přidejte jeden z následujících řádků toohello hello `dependencies` v souboru build.gradle:</span><span class="sxs-lookup"><span data-stu-id="17837-118">If your project is already set up toouse Gradle for build, add one of hello following lines toohello `dependencies` group in your build.gradle file:</span></span>

<span data-ttu-id="17837-119">Pak obnovte závislosti projektu hello, stažených binárních souborů tooget hello.</span><span class="sxs-lookup"><span data-stu-id="17837-119">Then refresh hello project dependencies, tooget hello binaries downloaded.</span></span>

<span data-ttu-id="17837-120">**Logback**</span><span class="sxs-lookup"><span data-stu-id="17837-120">**Logback**</span></span>

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

<span data-ttu-id="17837-121">**Log4J v2.0**</span><span class="sxs-lookup"><span data-stu-id="17837-121">**Log4J v2.0**</span></span>

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

<span data-ttu-id="17837-122">**Log4J v1.2**</span><span class="sxs-lookup"><span data-stu-id="17837-122">**Log4J v1.2**</span></span>

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a><span data-ttu-id="17837-123">V opačném případě...</span><span class="sxs-lookup"><span data-stu-id="17837-123">Otherwise ...</span></span>
<span data-ttu-id="17837-124">Stažení a extrakci odpovídající appender hello a pak přidejte hello příslušnou knihovnu tooyour projektu:</span><span class="sxs-lookup"><span data-stu-id="17837-124">Download and extract hello appropriate appender, then add hello appropriate library tooyour project:</span></span>

| <span data-ttu-id="17837-125">Protokoly</span><span class="sxs-lookup"><span data-stu-id="17837-125">Logger</span></span> | <span data-ttu-id="17837-126">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="17837-126">Download</span></span> | <span data-ttu-id="17837-127">Knihovna</span><span class="sxs-lookup"><span data-stu-id="17837-127">Library</span></span> |
| --- | --- | --- |
| <span data-ttu-id="17837-128">Logback</span><span class="sxs-lookup"><span data-stu-id="17837-128">Logback</span></span> |[<span data-ttu-id="17837-129">SDK Logback appender</span><span class="sxs-lookup"><span data-stu-id="17837-129">SDK with Logback appender</span></span>](https://aka.ms/xt62a4) |<span data-ttu-id="17837-130">applicationinsights. protokolování logback</span><span class="sxs-lookup"><span data-stu-id="17837-130">applicationinsights-logging-logback</span></span> |
| <span data-ttu-id="17837-131">Log4J v2.0</span><span class="sxs-lookup"><span data-stu-id="17837-131">Log4J v2.0</span></span> |[<span data-ttu-id="17837-132">SDK Log4J v2 appender</span><span class="sxs-lookup"><span data-stu-id="17837-132">SDK with Log4J v2 appender</span></span>](https://aka.ms/qypznq) |<span data-ttu-id="17837-133">applicationinsights. protokolování log4j2</span><span class="sxs-lookup"><span data-stu-id="17837-133">applicationinsights-logging-log4j2</span></span> |
| <span data-ttu-id="17837-134">Log4j v1.2</span><span class="sxs-lookup"><span data-stu-id="17837-134">Log4j v1.2</span></span> |[<span data-ttu-id="17837-135">SDK Log4J v1.2 appender</span><span class="sxs-lookup"><span data-stu-id="17837-135">SDK with Log4J v1.2 appender</span></span>](https://aka.ms/ky9cbo) |<span data-ttu-id="17837-136">applicationinsights. protokolování log4j1_2</span><span class="sxs-lookup"><span data-stu-id="17837-136">applicationinsights-logging-log4j1_2</span></span> |

## <a name="add-hello-appender-tooyour-logging-framework"></a><span data-ttu-id="17837-137">Přidání rozhraní protokolování tooyour appender hello</span><span class="sxs-lookup"><span data-stu-id="17837-137">Add hello appender tooyour logging framework</span></span>
<span data-ttu-id="17837-138">toostart načtení trasování, sloučení hello relevantní fragmentu kódu toohello Log4J nebo Logback konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="17837-138">toostart getting traces, merge hello relevant snippet of code toohello Log4J or Logback configuration file:</span></span> 

<span data-ttu-id="17837-139">*Logback*</span><span class="sxs-lookup"><span data-stu-id="17837-139">*Logback*</span></span>

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

<span data-ttu-id="17837-140">*Log4J v2.0*</span><span class="sxs-lookup"><span data-stu-id="17837-140">*Log4J v2.0*</span></span>

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

<span data-ttu-id="17837-141">*Log4J v1.2*</span><span class="sxs-lookup"><span data-stu-id="17837-141">*Log4J v1.2*</span></span>

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

<span data-ttu-id="17837-142">appenders Hello Application Insights může být odkaz žádné nakonfigurované protokolovacího nástroje a nemusí protokolovač kořenové hello (jak je znázorněno v ukázky kódu hello výše).</span><span class="sxs-lookup"><span data-stu-id="17837-142">hello Application Insights appenders can be referenced by any configured logger, and not necessarily by hello root logger (as shown in hello code samples above).</span></span>

## <a name="explore-your-traces-in-hello-application-insights-portal"></a><span data-ttu-id="17837-143">Prozkoumat vaše trasování portálu Application Insights hello</span><span class="sxs-lookup"><span data-stu-id="17837-143">Explore your traces in hello Application Insights portal</span></span>
<span data-ttu-id="17837-144">Nyní, když jste nakonfigurovali vašeho projektu toosend trasování tooApplication statistiky, můžete zobrazit a hledání toto trasování Application Insights portálu hello v hello [vyhledávání] [ diagnostic] okno.</span><span class="sxs-lookup"><span data-stu-id="17837-144">Now that you've configured your project toosend traces tooApplication Insights, you can view and search these traces in hello Application Insights portal, in hello [Search][diagnostic] blade.</span></span>

![Hello portálu Application Insights otevřete vyhledávání](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="17837-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17837-146">Next steps</span></span>
<span data-ttu-id="17837-147">[Diagnostické vyhledávání][diagnostic]</span><span class="sxs-lookup"><span data-stu-id="17837-147">[Diagnostic search][diagnostic]</span></span>

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


