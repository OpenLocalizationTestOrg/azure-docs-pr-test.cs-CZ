---
title: "Prozkoumejte Java protokolů trasování v Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 5baba3deaf58a1a24995c60381592a9c2ffefd81
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="explore-java-trace-logs-in-application-insights"></a><span data-ttu-id="6bb56-103">Prozkoumejte Java protokolů trasování ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="6bb56-103">Explore Java trace logs in Application Insights</span></span>
<span data-ttu-id="6bb56-104">Pokud používáte Logback nebo Log4J (verze 1.2 nebo v2.0) pro trasování, může mít vaše protokoly trasování automaticky odešlou do Application Insights, kde vám umožní zkoumat a hledat v nich.</span><span class="sxs-lookup"><span data-stu-id="6bb56-104">If you're using Logback or Log4J (v1.2 or v2.0) for tracing, you can have your trace logs sent automatically to Application Insights where you can explore and search on them.</span></span>

## <a name="install-the-java-sdk"></a><span data-ttu-id="6bb56-105">Nainstalujte Java SDK</span><span class="sxs-lookup"><span data-stu-id="6bb56-105">Install the Java SDK</span></span>

<span data-ttu-id="6bb56-106">Nainstalujte [Application Insights SDK pro jazyk Java][java], pokud ještě neudělali.</span><span class="sxs-lookup"><span data-stu-id="6bb56-106">Install [Application Insights SDK for Java][java], if you haven't already done that.</span></span>

<span data-ttu-id="6bb56-107">(Pokud nechcete, aby pro sledování požadavků HTTP, můžete vynechat většinu konfiguračního souboru .xml, ale musí obsahovat alespoň `InstrumentationKey` elementu.</span><span class="sxs-lookup"><span data-stu-id="6bb56-107">(If you don't want to track HTTP requests, you can omit most of the .xml configuration file, but you must at least include the `InstrumentationKey` element.</span></span> <span data-ttu-id="6bb56-108">Měli byste také zavolat `new TelemetryClient()` k chybě při inicializaci sady SDK.)</span><span class="sxs-lookup"><span data-stu-id="6bb56-108">You should also call `new TelemetryClient()` to initialize the SDK.)</span></span>


## <a name="add-logging-libraries-to-your-project"></a><span data-ttu-id="6bb56-109">Do projektu přidejte knihovny protokolování</span><span class="sxs-lookup"><span data-stu-id="6bb56-109">Add logging libraries to your project</span></span>
<span data-ttu-id="6bb56-110">*Zvolte vhodný způsob pro váš projekt.*</span><span class="sxs-lookup"><span data-stu-id="6bb56-110">*Choose the appropriate way for your project.*</span></span>

#### <a name="if-youre-using-maven"></a><span data-ttu-id="6bb56-111">Pokud používáte Maven...</span><span class="sxs-lookup"><span data-stu-id="6bb56-111">If you're using Maven...</span></span>
<span data-ttu-id="6bb56-112">Pokud je váš projekt již nastaven na sestavení s použitím nástroje Maven, slučte jednu z následujících fragmentů kódu do souboru pom.xml.</span><span class="sxs-lookup"><span data-stu-id="6bb56-112">If your project is already set up to use Maven for build, merge one of the following snippets of code into your pom.xml file.</span></span>

<span data-ttu-id="6bb56-113">Pak obnovte závislosti projektu, k získání stažených binárních souborů.</span><span class="sxs-lookup"><span data-stu-id="6bb56-113">Then refresh the project dependencies, to get the binaries downloaded.</span></span>

<span data-ttu-id="6bb56-114">*Logback*</span><span class="sxs-lookup"><span data-stu-id="6bb56-114">*Logback*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

<span data-ttu-id="6bb56-115">*Log4J v2.0*</span><span class="sxs-lookup"><span data-stu-id="6bb56-115">*Log4J v2.0*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

<span data-ttu-id="6bb56-116">*Log4J v1.2*</span><span class="sxs-lookup"><span data-stu-id="6bb56-116">*Log4J v1.2*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a><span data-ttu-id="6bb56-117">Pokud používáte Gradle...</span><span class="sxs-lookup"><span data-stu-id="6bb56-117">If you're using Gradle...</span></span>
<span data-ttu-id="6bb56-118">Pokud váš projekt je již nastaven na Gradle použít pro sestavení, přidejte jeden z následujících řádky, které se `dependencies` v souboru build.gradle:</span><span class="sxs-lookup"><span data-stu-id="6bb56-118">If your project is already set up to use Gradle for build, add one of the following lines to the `dependencies` group in your build.gradle file:</span></span>

<span data-ttu-id="6bb56-119">Pak obnovte závislosti projektu, k získání stažených binárních souborů.</span><span class="sxs-lookup"><span data-stu-id="6bb56-119">Then refresh the project dependencies, to get the binaries downloaded.</span></span>

<span data-ttu-id="6bb56-120">**Logback**</span><span class="sxs-lookup"><span data-stu-id="6bb56-120">**Logback**</span></span>

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

<span data-ttu-id="6bb56-121">**Log4J v2.0**</span><span class="sxs-lookup"><span data-stu-id="6bb56-121">**Log4J v2.0**</span></span>

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

<span data-ttu-id="6bb56-122">**Log4J v1.2**</span><span class="sxs-lookup"><span data-stu-id="6bb56-122">**Log4J v1.2**</span></span>

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a><span data-ttu-id="6bb56-123">V opačném případě...</span><span class="sxs-lookup"><span data-stu-id="6bb56-123">Otherwise ...</span></span>
<span data-ttu-id="6bb56-124">Stažení a extrakci odpovídající appender a pak přidejte příslušnou knihovnu do projektu:</span><span class="sxs-lookup"><span data-stu-id="6bb56-124">Download and extract the appropriate appender, then add the appropriate library to your project:</span></span>

| <span data-ttu-id="6bb56-125">Protokoly</span><span class="sxs-lookup"><span data-stu-id="6bb56-125">Logger</span></span> | <span data-ttu-id="6bb56-126">Ke stažení</span><span class="sxs-lookup"><span data-stu-id="6bb56-126">Download</span></span> | <span data-ttu-id="6bb56-127">Knihovna</span><span class="sxs-lookup"><span data-stu-id="6bb56-127">Library</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6bb56-128">Logback</span><span class="sxs-lookup"><span data-stu-id="6bb56-128">Logback</span></span> |[<span data-ttu-id="6bb56-129">SDK Logback appender</span><span class="sxs-lookup"><span data-stu-id="6bb56-129">SDK with Logback appender</span></span>](https://aka.ms/xt62a4) |<span data-ttu-id="6bb56-130">applicationinsights. protokolování logback</span><span class="sxs-lookup"><span data-stu-id="6bb56-130">applicationinsights-logging-logback</span></span> |
| <span data-ttu-id="6bb56-131">Log4J v2.0</span><span class="sxs-lookup"><span data-stu-id="6bb56-131">Log4J v2.0</span></span> |[<span data-ttu-id="6bb56-132">SDK Log4J v2 appender</span><span class="sxs-lookup"><span data-stu-id="6bb56-132">SDK with Log4J v2 appender</span></span>](https://aka.ms/qypznq) |<span data-ttu-id="6bb56-133">applicationinsights. protokolování log4j2</span><span class="sxs-lookup"><span data-stu-id="6bb56-133">applicationinsights-logging-log4j2</span></span> |
| <span data-ttu-id="6bb56-134">Log4j v1.2</span><span class="sxs-lookup"><span data-stu-id="6bb56-134">Log4j v1.2</span></span> |[<span data-ttu-id="6bb56-135">SDK Log4J v1.2 appender</span><span class="sxs-lookup"><span data-stu-id="6bb56-135">SDK with Log4J v1.2 appender</span></span>](https://aka.ms/ky9cbo) |<span data-ttu-id="6bb56-136">applicationinsights. protokolování log4j1_2</span><span class="sxs-lookup"><span data-stu-id="6bb56-136">applicationinsights-logging-log4j1_2</span></span> |

## <a name="add-the-appender-to-your-logging-framework"></a><span data-ttu-id="6bb56-137">Přidat appender do vašeho rozhraní protokolování</span><span class="sxs-lookup"><span data-stu-id="6bb56-137">Add the appender to your logging framework</span></span>
<span data-ttu-id="6bb56-138">Chcete-li spustit načtení trasování, sloučení relevantní fragment kódu do konfiguračního souboru Log4J nebo Logback:</span><span class="sxs-lookup"><span data-stu-id="6bb56-138">To start getting traces, merge the relevant snippet of code to the Log4J or Logback configuration file:</span></span> 

<span data-ttu-id="6bb56-139">*Logback*</span><span class="sxs-lookup"><span data-stu-id="6bb56-139">*Logback*</span></span>

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

<span data-ttu-id="6bb56-140">*Log4J v2.0*</span><span class="sxs-lookup"><span data-stu-id="6bb56-140">*Log4J v2.0*</span></span>

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

<span data-ttu-id="6bb56-141">*Log4J v1.2*</span><span class="sxs-lookup"><span data-stu-id="6bb56-141">*Log4J v1.2*</span></span>

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

<span data-ttu-id="6bb56-142">Appenders Application Insights může být odkaz žádné nakonfigurované protokolovacího nástroje a nemusí protokolovacího nástroje root (jak je znázorněno v ukázky kódu, výše).</span><span class="sxs-lookup"><span data-stu-id="6bb56-142">The Application Insights appenders can be referenced by any configured logger, and not necessarily by the root logger (as shown in the code samples above).</span></span>

## <a name="explore-your-traces-in-the-application-insights-portal"></a><span data-ttu-id="6bb56-143">Prozkoumat vaše trasování v portálu služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="6bb56-143">Explore your traces in the Application Insights portal</span></span>
<span data-ttu-id="6bb56-144">Nyní, když jste nakonfigurovali projekt k odeslání trasování do Application Insights, můžete zobrazit a vyhledávat tyto trasování v portálu služby Application Insights [vyhledávání] [ diagnostic] okno.</span><span class="sxs-lookup"><span data-stu-id="6bb56-144">Now that you've configured your project to send traces to Application Insights, you can view and search these traces in the Application Insights portal, in the [Search][diagnostic] blade.</span></span>

![Otevřete vyhledávání v portálu služby Application Insights](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="6bb56-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6bb56-146">Next steps</span></span>
<span data-ttu-id="6bb56-147">[Diagnostické vyhledávání][diagnostic]</span><span class="sxs-lookup"><span data-stu-id="6bb56-147">[Diagnostic search][diagnostic]</span></span>

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


