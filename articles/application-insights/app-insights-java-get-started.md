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
# <a name="get-started-with-application-insights-in-a-java-web-project"></a><span data-ttu-id="94bfe-103">Začínáme s Application Insights ve webovém projektu Java</span><span class="sxs-lookup"><span data-stu-id="94bfe-103">Get started with Application Insights in a Java web project</span></span>


<span data-ttu-id="94bfe-104">[Application Insights](https://azure.microsoft.com/services/application-insights/) je rozšiřitelnou analytickou službu vývojářům webů, které vám pomůže pochopit hello výkonu a využití vaší živé aplikace.</span><span class="sxs-lookup"><span data-stu-id="94bfe-104">[Application Insights](https://azure.microsoft.com/services/application-insights/) is an extensible analytics service for web developers that helps you understand hello performance and usage of your live application.</span></span> <span data-ttu-id="94bfe-105">Použít příliš[najít a diagnostikovat problémy s výkonem a výjimkami](app-insights-detect-triage-diagnose.md), a [napsat kód] [ api] tootrack co uživatelé dělají s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="94bfe-105">Use it too[detect and diagnose performance issues and exceptions](app-insights-detect-triage-diagnose.md), and [write code][api] tootrack what users do with your app.</span></span>

![ukázková data](./media/app-insights-java-get-started/5-results.png)

<span data-ttu-id="94bfe-107">Application Insights podporuje aplikace v Javě spuštěné v systému Linux, Unix nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="94bfe-107">Application Insights supports Java apps running on Linux, Unix, or Windows.</span></span>

<span data-ttu-id="94bfe-108">Budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="94bfe-108">You need:</span></span>

* <span data-ttu-id="94bfe-109">Oracle JRE 1.6 nebo novější nebo Zulu JRE 1.6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="94bfe-109">Oracle JRE 1.6 or later, or Zulu JRE 1.6 or later</span></span>
* <span data-ttu-id="94bfe-110">Předplatné příliš[Microsoft Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="94bfe-110">A subscription too[Microsoft Azure](https://azure.microsoft.com/).</span></span>

<span data-ttu-id="94bfe-111">*Pokud máte webovou aplikaci, která je již živá, můžete sledovat alternativní postup hello příliš[přidat hello SDK v době běhu hello webový server](app-insights-java-live.md). Tento alternativa zabraňuje opětovnému sestavení kódu hello, ale neobdržíte hello možnost toowrite kód tootrack aktivity uživatelů.*</span><span class="sxs-lookup"><span data-stu-id="94bfe-111">*If you have a web app that's already live, you could follow hello alternative procedure too[add hello SDK at runtime in hello web server](app-insights-java-live.md). That alternative avoids rebuilding hello code, but you don't get hello option toowrite code tootrack user activity.*</span></span>

## <a name="1-get-an-application-insights-instrumentation-key"></a><span data-ttu-id="94bfe-112">1. Získejte klíč instrumentace Application Insights</span><span class="sxs-lookup"><span data-stu-id="94bfe-112">1. Get an Application Insights instrumentation key</span></span>
1. <span data-ttu-id="94bfe-113">Přihlaste se toohello [portálu Microsoft Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="94bfe-113">Sign in toohello [Microsoft Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="94bfe-114">Vytvořte prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="94bfe-114">Create an Application Insights resource.</span></span> <span data-ttu-id="94bfe-115">Nastavit hello aplikace typu tooJava webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="94bfe-115">Set hello application type tooJava web application.</span></span>

    ![Zadejte název, vyberte webovou aplikaci Java a klikněte na možnost Vytvořit](./media/app-insights-java-get-started/02-create.png)
3. <span data-ttu-id="94bfe-117">Najít hello klíč instrumentace nového prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="94bfe-117">Find hello instrumentation key of hello new resource.</span></span> <span data-ttu-id="94bfe-118">Budete potřebovat toopaste tento klíč do projektu kódu za chvíli.</span><span class="sxs-lookup"><span data-stu-id="94bfe-118">You'll need toopaste this key into your code project shortly.</span></span>

    ![V hello přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte hello klíč instrumentace](./media/app-insights-java-get-started/03-key.png)

## <a name="2-add-hello-application-insights-sdk-for-java-tooyour-project"></a><span data-ttu-id="94bfe-120">2. Přidat hello Application Insights SDK pro jazyk Java tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="94bfe-120">2. Add hello Application Insights SDK for Java tooyour project</span></span>
<span data-ttu-id="94bfe-121">*Zvolte hello vhodný způsob pro váš projekt.*</span><span class="sxs-lookup"><span data-stu-id="94bfe-121">*Choose hello appropriate way for your project.*</span></span>

#### <a name="if-youre-using-eclipse-toocreate-a-maven-or-dynamic-web-project-"></a><span data-ttu-id="94bfe-122">Pokud používáte Eclipse toocreate Maven nebo dynamického webového projektu...</span><span class="sxs-lookup"><span data-stu-id="94bfe-122">If you're using Eclipse toocreate a Maven or Dynamic Web project ...</span></span>
<span data-ttu-id="94bfe-123">Použití hello [Application Insights SDK pro modul Java plug-in][eclipse].</span><span class="sxs-lookup"><span data-stu-id="94bfe-123">Use hello [Application Insights SDK for Java plug-in][eclipse].</span></span>

#### <a name="if-youre-using-maven"></a><span data-ttu-id="94bfe-124">Pokud používáte Maven...</span><span class="sxs-lookup"><span data-stu-id="94bfe-124">If you're using Maven...</span></span>
<span data-ttu-id="94bfe-125">Pokud váš projekt již nastaven toouse Maven pro sestavení, slučte následující kód soubor pom.xml tooyour hello.</span><span class="sxs-lookup"><span data-stu-id="94bfe-125">If your project is already set up toouse Maven for build, merge hello following code tooyour pom.xml file.</span></span>

<span data-ttu-id="94bfe-126">Potom závislosti projektu aktualizace hello tooget hello binární soubory stáhnout.</span><span class="sxs-lookup"><span data-stu-id="94bfe-126">Then, refresh hello project dependencies tooget hello binaries downloaded.</span></span>

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

* <span data-ttu-id="94bfe-127">*Chyby ověření sestavení nebo kontrolního součtu?*</span><span class="sxs-lookup"><span data-stu-id="94bfe-127">*Build or checksum validation errors?*</span></span> <span data-ttu-id="94bfe-128">Zkuste použít konkrétní verzi, například: `<version>1.0.n</version>`.</span><span class="sxs-lookup"><span data-stu-id="94bfe-128">Try using a specific version, such as: `<version>1.0.n</version>`.</span></span> <span data-ttu-id="94bfe-129">Nejnovější verzi hello najdete v hello [poznámky k verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) nebo v našich [artefaktech Maven](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).</span><span class="sxs-lookup"><span data-stu-id="94bfe-129">You'll find hello latest version in hello [SDK release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) or in our [Maven artifacts](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).</span></span>
* <span data-ttu-id="94bfe-130">*Třeba tooupdate tooa novou sadu SDK?*</span><span class="sxs-lookup"><span data-stu-id="94bfe-130">*Need tooupdate tooa new SDK?*</span></span> <span data-ttu-id="94bfe-131">Obnovte závislosti svého projektu.</span><span class="sxs-lookup"><span data-stu-id="94bfe-131">Refresh your project's dependencies.</span></span>

#### <a name="if-youre-using-gradle"></a><span data-ttu-id="94bfe-132">Pokud používáte Gradle...</span><span class="sxs-lookup"><span data-stu-id="94bfe-132">If you're using Gradle...</span></span>
<span data-ttu-id="94bfe-133">Pokud váš projekt již nastaven toouse Gradle pro sestavení, slučte následující kód tooyour build.gradle soubor hello.</span><span class="sxs-lookup"><span data-stu-id="94bfe-133">If your project is already set up toouse Gradle for build, merge hello following code tooyour build.gradle file.</span></span>

<span data-ttu-id="94bfe-134">Potom závislosti projektu aktualizace hello tooget hello binární soubory stáhnout.</span><span class="sxs-lookup"><span data-stu-id="94bfe-134">Then refresh hello project dependencies tooget hello binaries downloaded.</span></span>

```JSON

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '1.+'
      // or applicationinsights-core for bare API
    }
```

* <span data-ttu-id="94bfe-135">*Chyby ověření sestavení nebo kontrolního součtu? Zkuste použít konkrétní verzi, například:* `version:'1.0.n'`.</span><span class="sxs-lookup"><span data-stu-id="94bfe-135">*Build or checksum validation errors? Try using a specific version, such as:* `version:'1.0.n'`.</span></span> <span data-ttu-id="94bfe-136">*Nejnovější verzi hello najdete v hello [poznámky k verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).*</span><span class="sxs-lookup"><span data-stu-id="94bfe-136">*You'll find hello latest version in hello [SDK release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).*</span></span>
* <span data-ttu-id="94bfe-137">*tooupdate tooa novou sadu SDK*</span><span class="sxs-lookup"><span data-stu-id="94bfe-137">*tooupdate tooa new SDK*</span></span>
  * <span data-ttu-id="94bfe-138">Obnovte závislosti svého projektu.</span><span class="sxs-lookup"><span data-stu-id="94bfe-138">Refresh your project's dependencies.</span></span>

#### <a name="otherwise-"></a><span data-ttu-id="94bfe-139">V opačném případě...</span><span class="sxs-lookup"><span data-stu-id="94bfe-139">Otherwise ...</span></span>
<span data-ttu-id="94bfe-140">Ručně přidejte hello SDK:</span><span class="sxs-lookup"><span data-stu-id="94bfe-140">Manually add hello SDK:</span></span>

1. <span data-ttu-id="94bfe-141">Stáhnout hello [Application Insights SDK pro jazyk Java](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="94bfe-141">Download hello [Application Insights SDK for Java](https://aka.ms/aijavasdk).</span></span>
2. <span data-ttu-id="94bfe-142">Rozbalte binární soubory hello ze souboru zip hello a přidejte tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="94bfe-142">Extract hello binaries from hello zip file and add them tooyour project.</span></span>

### <a name="questions"></a><span data-ttu-id="94bfe-143">Otázky...</span><span class="sxs-lookup"><span data-stu-id="94bfe-143">Questions...</span></span>
* <span data-ttu-id="94bfe-144">*Co je hello vztah mezi hello `-core` a `-web` součásti v hello zip?*</span><span class="sxs-lookup"><span data-stu-id="94bfe-144">*What's hello relationship between hello `-core` and `-web` components in hello zip?*</span></span>

  * <span data-ttu-id="94bfe-145">`applicationinsights-core`poskytuje hello úplné rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="94bfe-145">`applicationinsights-core` gives you hello bare API.</span></span> <span data-ttu-id="94bfe-146">Tuto komponentu budete vždy potřebovat.</span><span class="sxs-lookup"><span data-stu-id="94bfe-146">You always need this component.</span></span>
  * <span data-ttu-id="94bfe-147">`applicationinsights-web` poskytuje metriky, které sledují počty žádostí HTTP a časy odezvy.</span><span class="sxs-lookup"><span data-stu-id="94bfe-147">`applicationinsights-web` gives you metrics that track HTTP request counts and response times.</span></span> <span data-ttu-id="94bfe-148">Tuto komponentu můžete vynechat, pokud nechcete automaticky shromažďovat tuto telemetrii.</span><span class="sxs-lookup"><span data-stu-id="94bfe-148">You can omit this component if you don't want this telemetry automatically collected.</span></span> <span data-ttu-id="94bfe-149">Pokud například chcete toowrite vlastní.</span><span class="sxs-lookup"><span data-stu-id="94bfe-149">For example, if you want toowrite your own.</span></span>
* <span data-ttu-id="94bfe-150">*hello tooupdate SDK, když publikujeme změny*</span><span class="sxs-lookup"><span data-stu-id="94bfe-150">*tooupdate hello SDK when we publish changes*</span></span>

  * <span data-ttu-id="94bfe-151">Stáhněte si nejnovější hello [Application Insights SDK pro jazyk Java](https://aka.ms/qqkaq6) a nahradit text hello staré.</span><span class="sxs-lookup"><span data-stu-id="94bfe-151">Download hello latest [Application Insights SDK for Java](https://aka.ms/qqkaq6) and replace hello old ones.</span></span>
  * <span data-ttu-id="94bfe-152">Změny jsou popsány v hello [poznámky k verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).</span><span class="sxs-lookup"><span data-stu-id="94bfe-152">Changes are described in hello [SDK release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).</span></span>

## <a name="3-add-an-application-insights-xml-file"></a><span data-ttu-id="94bfe-153">3. Vytvořte soubor Application Insights .xml</span><span class="sxs-lookup"><span data-stu-id="94bfe-153">3. Add an Application Insights .xml file</span></span>
<span data-ttu-id="94bfe-154">Přidejte soubor ApplicationInsights.xml toohello prostředky složku ve vašem projektu a ujistěte se, že je přidaná cesty nasazení tříd projektu tooyour.</span><span class="sxs-lookup"><span data-stu-id="94bfe-154">Add ApplicationInsights.xml toohello resources folder in your project, or make sure it is added tooyour project’s deployment class path.</span></span> <span data-ttu-id="94bfe-155">Zkopírujte následující XML do ní hello.</span><span class="sxs-lookup"><span data-stu-id="94bfe-155">Copy hello following XML into it.</span></span>

<span data-ttu-id="94bfe-156">Nahraďte klíč instrumentace hello, který jste získali z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="94bfe-156">Substitute hello instrumentation key that you got from hello Azure portal.</span></span>

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


* <span data-ttu-id="94bfe-157">klíč instrumentace Hello se odesílají společně s každou položkou telemetrie a říká toodisplay Application Insights je ve vašem prostředku.</span><span class="sxs-lookup"><span data-stu-id="94bfe-157">hello instrumentation key is sent along with every item of telemetry and tells Application Insights toodisplay it in your resource.</span></span>
* <span data-ttu-id="94bfe-158">Hello požadavek komponenty HTTP je volitelný.</span><span class="sxs-lookup"><span data-stu-id="94bfe-158">hello HTTP Request component is optional.</span></span> <span data-ttu-id="94bfe-159">Automaticky odesílá telemetrii týkající se požadavků a odpovědí časy toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="94bfe-159">It automatically sends telemetry about requests and response times toohello portal.</span></span>
* <span data-ttu-id="94bfe-160">Korelace událostí je komponenty požadavku HTTP toohello přidání.</span><span class="sxs-lookup"><span data-stu-id="94bfe-160">Events correlation is an addition toohello HTTP request component.</span></span> <span data-ttu-id="94bfe-161">Přiřadí požadavek tooeach identifikátor přijatých hello serveru a přidá tento identifikátor jako položka tooevery vlastnost telemetrie jako hello vlastnost 'Operation.Id'.</span><span class="sxs-lookup"><span data-stu-id="94bfe-161">It assigns an identifier tooeach request received by hello server, and adds this identifier as a property tooevery item of telemetry as hello property 'Operation.Id'.</span></span> <span data-ttu-id="94bfe-162">Umožňuje toocorrelate hello telemetrii související s každou žádostí nastavením filtru v [diagnostické vyhledávání][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="94bfe-162">It allows you toocorrelate hello telemetry associated with each request by setting a filter in [diagnostic search][diagnostic].</span></span>
* <span data-ttu-id="94bfe-163">klíč Application Insights Hello možné předat dynamicky z hello portálu Azure jako vlastnost systému (-DAPPLICATION_INSIGHTS_IKEY = your_ikey).</span><span class="sxs-lookup"><span data-stu-id="94bfe-163">hello Application Insights key can be passed dynamically from hello Azure portal as a system property (-DAPPLICATION_INSIGHTS_IKEY=your_ikey).</span></span> <span data-ttu-id="94bfe-164">Pokud není definovaná žádná vlastnost, hledá se proměnná prostředí (APPLICATION_INSIGHTS_IKEY) v nastavení aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="94bfe-164">If there is no property defined, it checks for environment variable (APPLICATION_INSIGHTS_IKEY) in Azure App Settings.</span></span> <span data-ttu-id="94bfe-165">Pokud jsou obě vlastnosti hello nedefinované, použije se výchozí hello InstrumentationKey z ApplicationInsights.xml.</span><span class="sxs-lookup"><span data-stu-id="94bfe-165">If both hello properties are undefined, hello default InstrumentationKey is used from ApplicationInsights.xml.</span></span> <span data-ttu-id="94bfe-166">Toto pořadí vám pomůže toomanage různých InstrumentationKeys pro různá prostředí dynamicky.</span><span class="sxs-lookup"><span data-stu-id="94bfe-166">This sequence helps you toomanage different InstrumentationKeys for different environments dynamically.</span></span>

### <a name="alternative-ways-tooset-hello-instrumentation-key"></a><span data-ttu-id="94bfe-167">Klíč instrumentace hello tooset alternativní způsoby</span><span class="sxs-lookup"><span data-stu-id="94bfe-167">Alternative ways tooset hello instrumentation key</span></span>
<span data-ttu-id="94bfe-168">Application Insights SDK hledá hello klíč v tomto pořadí:</span><span class="sxs-lookup"><span data-stu-id="94bfe-168">Application Insights SDK looks for hello key in this order:</span></span>

1. <span data-ttu-id="94bfe-169">Systémová vlastnost: -DAPPLICATION_INSIGHTS_IKEY=váš_ikey</span><span class="sxs-lookup"><span data-stu-id="94bfe-169">System property: -DAPPLICATION_INSIGHTS_IKEY=your_ikey</span></span>
2. <span data-ttu-id="94bfe-170">Proměnná prostředí: APPLICATION_INSIGHTS_IKEY</span><span class="sxs-lookup"><span data-stu-id="94bfe-170">Environment variable: APPLICATION_INSIGHTS_IKEY</span></span>
3. <span data-ttu-id="94bfe-171">Konfigurační soubor: ApplicationInsights.xml</span><span class="sxs-lookup"><span data-stu-id="94bfe-171">Configuration file: ApplicationInsights.xml</span></span>

<span data-ttu-id="94bfe-172">Můžete ho taky [nastavit v kódu](app-insights-api-custom-events-metrics.md#ikey):</span><span class="sxs-lookup"><span data-stu-id="94bfe-172">You can also [set it in code](app-insights-api-custom-events-metrics.md#ikey):</span></span>

```Java

    telemetryClient.InstrumentationKey = "...";
```

## <a name="4-add-an-http-filter"></a><span data-ttu-id="94bfe-173">4. Přidat filtr HTTP</span><span class="sxs-lookup"><span data-stu-id="94bfe-173">4. Add an HTTP filter</span></span>
<span data-ttu-id="94bfe-174">Hello poslední krok konfigurace umožňuje toolog komponenty požadavku HTTP hello každý webový požadavek.</span><span class="sxs-lookup"><span data-stu-id="94bfe-174">hello last configuration step allows hello HTTP request component toolog each web request.</span></span> <span data-ttu-id="94bfe-175">(Není povinné, pokud chcete úplné rozhraní API hello.)</span><span class="sxs-lookup"><span data-stu-id="94bfe-175">(Not required if you just want hello bare API.)</span></span>

<span data-ttu-id="94bfe-176">Vyhledejte a otevřete soubor web.xml hello v projektu a merge hello následující kód pod uzlem hello webové aplikace, které jsou nakonfigurované filtry aplikace.</span><span class="sxs-lookup"><span data-stu-id="94bfe-176">Locate and open hello web.xml file in your project, and merge hello following code under hello web-app node, where your application filters are configured.</span></span>

<span data-ttu-id="94bfe-177">tooget hello nejpřesnější výsledky, hello filtr by měly být namapované před všemi ostatními filtry.</span><span class="sxs-lookup"><span data-stu-id="94bfe-177">tooget hello most accurate results, hello filter should be mapped before all other filters.</span></span>

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

#### <a name="if-youre-using-spring-web-mvc-31-or-later"></a><span data-ttu-id="94bfe-178">Pokud používáte Spring Web MVC 3.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="94bfe-178">If you're using Spring Web MVC 3.1 or later</span></span>
<span data-ttu-id="94bfe-179">Upravte tyto prvky v *-servlet.xml tooinclude hello Application Insights balíčku:</span><span class="sxs-lookup"><span data-stu-id="94bfe-179">Edit these elements in *-servlet.xml tooinclude hello Application Insights package:</span></span>

```XML

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>
```

#### <a name="if-youre-using-struts-2"></a><span data-ttu-id="94bfe-180">Pokud používáte Struts 2</span><span class="sxs-lookup"><span data-stu-id="94bfe-180">If you're using Struts 2</span></span>
<span data-ttu-id="94bfe-181">Přidejte tuto položku toohello Struts konfigurační soubor (obvykle s názvem struts.xml nebo struts default.xml):</span><span class="sxs-lookup"><span data-stu-id="94bfe-181">Add this item toohello Struts configuration file (usually named struts.xml or struts-default.xml):</span></span>

```XML

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />
```

<span data-ttu-id="94bfe-182">(Pokud máte sběrače definované ve výchozím zásobníku, hello interceptoru jednoduše přidáním toothat zásobníku.)</span><span class="sxs-lookup"><span data-stu-id="94bfe-182">(If you have interceptors defined in a default stack, hello interceptor can simply be added toothat stack.)</span></span>

## <a name="5-run-your-application"></a><span data-ttu-id="94bfe-183">5. Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="94bfe-183">5. Run your application</span></span>
<span data-ttu-id="94bfe-184">Buď spustit v režimu ladění na vývojovém počítači, nebo publikovat tooyour serveru.</span><span class="sxs-lookup"><span data-stu-id="94bfe-184">Either run it in debug mode on your development machine, or publish tooyour server.</span></span>

## <a name="6-view-your-telemetry-in-application-insights"></a><span data-ttu-id="94bfe-185">6. Zobrazte telemetrii ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="94bfe-185">6. View your telemetry in Application Insights</span></span>
<span data-ttu-id="94bfe-186">Vrátí prostředek Application Insights tooyour [portálu Microsoft Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="94bfe-186">Return tooyour Application Insights resource in [Microsoft Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="94bfe-187">Data požadavků HTTP se zobrazí v okně Přehled hello.</span><span class="sxs-lookup"><span data-stu-id="94bfe-187">HTTP requests data appears on hello overview blade.</span></span> <span data-ttu-id="94bfe-188">(Pokud zde nejsou, počkejte několik sekund a pak klikněte na tlačítko Aktualizovat.)</span><span class="sxs-lookup"><span data-stu-id="94bfe-188">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![ukázková data](./media/app-insights-java-get-started/5-results.png)

<span data-ttu-id="94bfe-190">[Další informace o metrikách.][metrics]</span><span class="sxs-lookup"><span data-stu-id="94bfe-190">[Learn more about metrics.][metrics]</span></span>

<span data-ttu-id="94bfe-191">Klikněte na tlačítko prostřednictvím jakékoli grafu toosee podrobnější agregován metriky.</span><span class="sxs-lookup"><span data-stu-id="94bfe-191">Click through any chart toosee more detailed aggregated metrics.</span></span>

![](./media/app-insights-java-get-started/6-barchart.png)

> <span data-ttu-id="94bfe-192">Application Insights předpokládá hello formát požadavků HTTP pro aplikace MVC je: `VERB controller/action`.</span><span class="sxs-lookup"><span data-stu-id="94bfe-192">Application Insights assumes hello format of HTTP requests for MVC applications is: `VERB controller/action`.</span></span> <span data-ttu-id="94bfe-193">Například `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` a `GET Home/Product/sdf96vws` se seskupí do `GET Home/Product`.</span><span class="sxs-lookup"><span data-stu-id="94bfe-193">For example, `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` and `GET Home/Product/sdf96vws` are grouped into `GET Home/Product`.</span></span> <span data-ttu-id="94bfe-194">Toto seskupení umožňuje smysluplné agregace požadavků, jako je počet požadavků a průměrná doba provádění pro požadavky.</span><span class="sxs-lookup"><span data-stu-id="94bfe-194">This grouping enables meaningful aggregations of requests, such as number of requests and average execution time for requests.</span></span>
>
>

### <a name="instance-data"></a><span data-ttu-id="94bfe-195">Data instance</span><span class="sxs-lookup"><span data-stu-id="94bfe-195">Instance data</span></span>
<span data-ttu-id="94bfe-196">Klikněte na tlačítko prostřednictvím konkrétního požadavku zadejte toosee jednotlivých instancí.</span><span class="sxs-lookup"><span data-stu-id="94bfe-196">Click through a specific request type toosee individual instances.</span></span>

<span data-ttu-id="94bfe-197">Ve službě Application Insights se zobrazí dva druhy dat: agregovaná data, uložená a zobrazená jako průměry, počty a součty; a data instancí – jednotlivé sestavy požadavků protokolu HTTP, výjimky, zobrazení stránek nebo uživatelské události.</span><span class="sxs-lookup"><span data-stu-id="94bfe-197">Two kinds of data are displayed in Application Insights: aggregated data, stored and displayed as averages, counts, and sums; and instance data - individual reports of HTTP requests, exceptions, page views, or custom events.</span></span>

<span data-ttu-id="94bfe-198">Při prohlížení vlastností hello požadavku, uvidíte hello telemetrické události související s například požadavky a výjimkami.</span><span class="sxs-lookup"><span data-stu-id="94bfe-198">When viewing hello properties of a request, you can see hello telemetry events associated with it such as requests and exceptions.</span></span>

![](./media/app-insights-java-get-started/7-instance.png)

### <a name="analytics-powerful-query-language"></a><span data-ttu-id="94bfe-199">Analýzy: účinný dotazovací jazyk</span><span class="sxs-lookup"><span data-stu-id="94bfe-199">Analytics: Powerful query language</span></span>
<span data-ttu-id="94bfe-200">Jak shromažďujete další data, můžete spouštět dotazy obou tooaggregate dat a toofind jednotlivých instancí.</span><span class="sxs-lookup"><span data-stu-id="94bfe-200">As you accumulate more data, you can run queries both tooaggregate data and toofind individual instances.</span></span>  <span data-ttu-id="94bfe-201">[Analýzy](app-insights-analytics.md) představují výkonný nástroj jak pro vysvětlení výkonu, tak i využití a k diagnostickým účelům.</span><span class="sxs-lookup"><span data-stu-id="94bfe-201">[Analytics](app-insights-analytics.md) is a powerful tool for both for understanding performance and usage, and for diagnostic purposes.</span></span>

![Příklad analýz](./media/app-insights-java-get-started/025.png)

## <a name="7-install-your-app-on-hello-server"></a><span data-ttu-id="94bfe-203">7. Nainstalujte aplikaci na hello server</span><span class="sxs-lookup"><span data-stu-id="94bfe-203">7. Install your app on hello server</span></span>
<span data-ttu-id="94bfe-204">Teď publikujte aplikačního serveru toohello, uživatelé mohou ho použít a sledujte telemetrii hello objeví na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="94bfe-204">Now publish your app toohello server, let people use it, and watch hello telemetry show up on hello portal.</span></span>

* <span data-ttu-id="94bfe-205">Ujistěte se, že brána firewall umožňuje vaší aplikace toosend telemetrie toothese porty:</span><span class="sxs-lookup"><span data-stu-id="94bfe-205">Make sure your firewall allows your application toosend telemetry toothese ports:</span></span>

  * <span data-ttu-id="94bfe-206">dc.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="94bfe-206">dc.services.visualstudio.com:443</span></span>
  * <span data-ttu-id="94bfe-207">f5.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="94bfe-207">f5.services.visualstudio.com:443</span></span>

* <span data-ttu-id="94bfe-208">Pokud je nutné odchozí provoz směrovat přes bránu firewall, nadefinujte systémové vlastnosti `http.proxyHost` a `http.proxyPort`.</span><span class="sxs-lookup"><span data-stu-id="94bfe-208">If outgoing traffic must be routed through a firewall, define system properties `http.proxyHost` and `http.proxyPort`.</span></span>

* <span data-ttu-id="94bfe-209">Na serverech Windows nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="94bfe-209">On Windows servers, install:</span></span>

  * [<span data-ttu-id="94bfe-210">Microsoft Visual C++ Redistributable</span><span class="sxs-lookup"><span data-stu-id="94bfe-210">Microsoft Visual C++ Redistributable</span></span>](http://www.microsoft.com/download/details.aspx?id=40784)

    <span data-ttu-id="94bfe-211">(Tato komponenta povoluje čítače výkonu.)</span><span class="sxs-lookup"><span data-stu-id="94bfe-211">(This component enables performance counters.)</span></span>


## <a name="exceptions-and-request-failures"></a><span data-ttu-id="94bfe-212">Výjimky a chyby požadavků</span><span class="sxs-lookup"><span data-stu-id="94bfe-212">Exceptions and request failures</span></span>
<span data-ttu-id="94bfe-213">Nezpracované výjimky jsou shromažďovány automaticky:</span><span class="sxs-lookup"><span data-stu-id="94bfe-213">Unhandled exceptions are automatically collected:</span></span>

![Otevřete Nastavení, Selhání.](./media/app-insights-java-get-started/21-exceptions.png)

<span data-ttu-id="94bfe-215">toocollect data o dalších výjimkách, máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="94bfe-215">toocollect data on other exceptions, you have two options:</span></span>

* <span data-ttu-id="94bfe-216">[Vložení volání tootrackException() ve vašem kódu][apiexceptions].</span><span class="sxs-lookup"><span data-stu-id="94bfe-216">[Insert calls tootrackException() in your code][apiexceptions].</span></span>
* <span data-ttu-id="94bfe-217">[Nainstalujte na server agenta Java hello](app-insights-java-agent.md).</span><span class="sxs-lookup"><span data-stu-id="94bfe-217">[Install hello Java Agent on your server](app-insights-java-agent.md).</span></span> <span data-ttu-id="94bfe-218">Zadáte hello metody, které chcete toowatch.</span><span class="sxs-lookup"><span data-stu-id="94bfe-218">You specify hello methods you want toowatch.</span></span>

## <a name="monitor-method-calls-and-external-dependencies"></a><span data-ttu-id="94bfe-219">Volání metody monitorování a externí závislosti</span><span class="sxs-lookup"><span data-stu-id="94bfe-219">Monitor method calls and external dependencies</span></span>
<span data-ttu-id="94bfe-220">[Nainstalujte agenta Java hello](app-insights-java-agent.md) toolog zadané vnitřních metod a volání provedená prostřednictvím JDBC s daty časování.</span><span class="sxs-lookup"><span data-stu-id="94bfe-220">[Install hello Java Agent](app-insights-java-agent.md) toolog specified internal methods and calls made through JDBC, with timing data.</span></span>

## <a name="performance-counters"></a><span data-ttu-id="94bfe-221">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="94bfe-221">Performance counters</span></span>
<span data-ttu-id="94bfe-222">Otevřete **nastavení**, **servery**, toosee rozsah čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="94bfe-222">Open **Settings**, **Servers**, toosee a range of performance counters.</span></span>

![](./media/app-insights-java-get-started/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a><span data-ttu-id="94bfe-223">Vlastní nastavení kolekce čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="94bfe-223">Customize performance counter collection</span></span>
<span data-ttu-id="94bfe-224">kolekce toodisable hello standardní sady čítačů výkonu, přidejte následující kód pod hello kořenového uzlu souboru ApplicationInsights.xml hello hello:</span><span class="sxs-lookup"><span data-stu-id="94bfe-224">toodisable collection of hello standard set of performance counters, add hello following code under hello root node of hello ApplicationInsights.xml file:</span></span>

```XML
    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a><span data-ttu-id="94bfe-225">Shromažďování dalších čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="94bfe-225">Collect additional performance counters</span></span>
<span data-ttu-id="94bfe-226">Můžete zadat další výkonu čítače toobe shromážděných.</span><span class="sxs-lookup"><span data-stu-id="94bfe-226">You can specify additional performance counters toobe collected.</span></span>

#### <a name="jmx-counters-exposed-by-hello-java-virtual-machine"></a><span data-ttu-id="94bfe-227">Čítače JMX (vystavené ve virtuálním počítači Java hello)</span><span class="sxs-lookup"><span data-stu-id="94bfe-227">JMX counters (exposed by hello Java Virtual Machine)</span></span>

```XML
    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* <span data-ttu-id="94bfe-228">`displayName`– hello název zobrazený na portálu služby Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="94bfe-228">`displayName` – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="94bfe-229">`objectName`– Název objektu JMX hello.</span><span class="sxs-lookup"><span data-stu-id="94bfe-229">`objectName` – hello JMX object name.</span></span>
* <span data-ttu-id="94bfe-230">`attribute`– atribut hello toofetch název objektu JMX hello</span><span class="sxs-lookup"><span data-stu-id="94bfe-230">`attribute` – hello attribute of hello JMX object name toofetch</span></span>
* <span data-ttu-id="94bfe-231">`type`(volitelné) – hello typ atributu JMX objektu:</span><span class="sxs-lookup"><span data-stu-id="94bfe-231">`type` (optional) - hello type of JMX object’s attribute:</span></span>
  * <span data-ttu-id="94bfe-232">Výchozí hodnota: jednoduchý typ, například int nebo long.</span><span class="sxs-lookup"><span data-stu-id="94bfe-232">Default: a simple type such as int or long.</span></span>
  * <span data-ttu-id="94bfe-233">`composite`: data čítače výkonu hello je ve formátu "Attribute.Data" hello</span><span class="sxs-lookup"><span data-stu-id="94bfe-233">`composite`: hello perf counter data is in hello format of 'Attribute.Data'</span></span>
  * <span data-ttu-id="94bfe-234">`tabular`: data čítače výkonu hello je ve formátu hello řádku tabulky</span><span class="sxs-lookup"><span data-stu-id="94bfe-234">`tabular`: hello perf counter data is in hello format of a table row</span></span>

#### <a name="windows-performance-counters"></a><span data-ttu-id="94bfe-235">Čítače výkonu Windows</span><span class="sxs-lookup"><span data-stu-id="94bfe-235">Windows performance counters</span></span>
<span data-ttu-id="94bfe-236">Každý [čítačů výkonu systému Windows](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) je členem určité kategorie (v hello stejným způsobem, že je pole členem třídy).</span><span class="sxs-lookup"><span data-stu-id="94bfe-236">Each [Windows performance counter](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) is a member of a category (in hello same way that a field is a member of a class).</span></span> <span data-ttu-id="94bfe-237">Kategorie mohou být buď globální, nebo mohou mít číslované nebo pojmenované instance.</span><span class="sxs-lookup"><span data-stu-id="94bfe-237">Categories can either be global, or can have numbered or named instances.</span></span>

```XML
    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* <span data-ttu-id="94bfe-238">displayName – název hello zobrazený na portálu služby Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="94bfe-238">displayName – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="94bfe-239">categoryName – hello kategorie čítače výkonu (objekt výkonu) ke kterému je přiřazeno tento čítač výkonu.</span><span class="sxs-lookup"><span data-stu-id="94bfe-239">categoryName – hello performance counter category (performance object) with which this performance counter is associated.</span></span>
* <span data-ttu-id="94bfe-240">counterName – název čítače výkonu hello hello.</span><span class="sxs-lookup"><span data-stu-id="94bfe-240">counterName – hello name of hello performance counter.</span></span>
* <span data-ttu-id="94bfe-241">instanceName – název hello hello instance kategorie čítače výkonu nebo prázdný řetězec (""), pokud hello kategorie obsahuje jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="94bfe-241">instanceName – hello name of hello performance counter category instance, or an empty string (""), if hello category contains a single instance.</span></span> <span data-ttu-id="94bfe-242">Pokud je hello categoryName proces a hello chcete toocollect je hodnota čítače výkonu z aktuálního procesu JVM hello na, které běží vaše aplikace, zadejte `"__SELF__"`.</span><span class="sxs-lookup"><span data-stu-id="94bfe-242">If hello categoryName is Process, and hello performance counter you'd like toocollect is from hello current JVM process on which your app is running, specify `"__SELF__"`.</span></span>

<span data-ttu-id="94bfe-243">Čítače výkonu jsou zobrazené jako vlastní metriky v [Průzkumníku metrik][metrics].</span><span class="sxs-lookup"><span data-stu-id="94bfe-243">Your performance counters are visible as custom metrics in [Metrics Explorer][metrics].</span></span>

![](./media/app-insights-java-get-started/12-custom-perfs.png)

### <a name="unix-performance-counters"></a><span data-ttu-id="94bfe-244">Čítače výkonu Unix</span><span class="sxs-lookup"><span data-stu-id="94bfe-244">Unix performance counters</span></span>
* <span data-ttu-id="94bfe-245">[Nainstalujte collectd s plug-in Application Insights hello](app-insights-java-collectd.md) tooget celou řadu dat systému a sítě.</span><span class="sxs-lookup"><span data-stu-id="94bfe-245">[Install collectd with hello Application Insights plugin](app-insights-java-collectd.md) tooget a wide variety of system and network data.</span></span>

## <a name="get-user-and-session-data"></a><span data-ttu-id="94bfe-246">Získejte data uživatele a relace</span><span class="sxs-lookup"><span data-stu-id="94bfe-246">Get user and session data</span></span>
<span data-ttu-id="94bfe-247">Takže odesíláte telemetrii z webového serveru.</span><span class="sxs-lookup"><span data-stu-id="94bfe-247">OK, you're sending telemetry from your web server.</span></span> <span data-ttu-id="94bfe-248">Nyní tooget hello úplné 360 stupňové zobrazení vaší aplikace, můžete přidat další monitorování:</span><span class="sxs-lookup"><span data-stu-id="94bfe-248">Now tooget hello full 360-degree view of your application, you can add more monitoring:</span></span>

* <span data-ttu-id="94bfe-249">[Přidat telemetrii tooyour webové stránky] [ usage] toomonitor stránky zobrazení a metrik uživatele.</span><span class="sxs-lookup"><span data-stu-id="94bfe-249">[Add telemetry tooyour web pages][usage] toomonitor page views and user metrics.</span></span>
* <span data-ttu-id="94bfe-250">[Nastavit testy webu] [ availability] toomake, že vaše aplikace zůstává aktivní a reagující.</span><span class="sxs-lookup"><span data-stu-id="94bfe-250">[Set up web tests][availability] toomake sure your application stays live and responsive.</span></span>

## <a name="capture-log-traces"></a><span data-ttu-id="94bfe-251">Zaznamenat trasování protokolu</span><span class="sxs-lookup"><span data-stu-id="94bfe-251">Capture log traces</span></span>
<span data-ttu-id="94bfe-252">Můžete použít tooslice Application Insights a rozčlenění protokolů z Log4J, Logback nebo jiných rozhraní protokolování.</span><span class="sxs-lookup"><span data-stu-id="94bfe-252">You can use Application Insights tooslice and dice logs from Log4J, Logback, or other logging frameworks.</span></span> <span data-ttu-id="94bfe-253">Hello protokoly mohou korelovat s požadavky HTTP a další telemetrií.</span><span class="sxs-lookup"><span data-stu-id="94bfe-253">You can correlate hello logs with HTTP requests and other telemetry.</span></span> <span data-ttu-id="94bfe-254">[Zjistěte jak][javalogs].</span><span class="sxs-lookup"><span data-stu-id="94bfe-254">[Learn how][javalogs].</span></span>

## <a name="send-your-own-telemetry"></a><span data-ttu-id="94bfe-255">Odeslat vlastní telemetrii</span><span class="sxs-lookup"><span data-stu-id="94bfe-255">Send your own telemetry</span></span>
<span data-ttu-id="94bfe-256">Teď, když jste nainstalovali hello SDK, můžete použít rozhraní API toosend hello vlastní telemetrii.</span><span class="sxs-lookup"><span data-stu-id="94bfe-256">Now that you've installed hello SDK, you can use hello API toosend your own telemetry.</span></span>

* <span data-ttu-id="94bfe-257">[Sledujte vlastní události a metriky] [ api] toolearn co uživatelé dělají s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="94bfe-257">[Track custom events and metrics][api] toolearn what users are doing with your application.</span></span>
* <span data-ttu-id="94bfe-258">[Hledání událostí a protokolů] [ diagnostic] toohelp diagnostikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="94bfe-258">[Search events and logs][diagnostic] toohelp diagnose problems.</span></span>

## <a name="availability-web-tests"></a><span data-ttu-id="94bfe-259">Testy dostupnosti webu</span><span class="sxs-lookup"><span data-stu-id="94bfe-259">Availability web tests</span></span>
<span data-ttu-id="94bfe-260">Application Insights může otestovat váš web v pravidelných intervalech toocheck, zda je funkční a dobře reaguje.</span><span class="sxs-lookup"><span data-stu-id="94bfe-260">Application Insights can test your website at regular intervals toocheck that it's up and responding well.</span></span> <span data-ttu-id="94bfe-261">[tooset až][availability], klikněte na tlačítko testy webu.</span><span class="sxs-lookup"><span data-stu-id="94bfe-261">[tooset up][availability], click Web tests.</span></span>

![Klikněte na Webové testy a pak přidejte webový test.](./media/app-insights-java-get-started/31-config-web-test.png)

<span data-ttu-id="94bfe-263">Získáte tabulky s dobami odezvy a navíc e-mailová oznámení, pokud váš web nefunguje.</span><span class="sxs-lookup"><span data-stu-id="94bfe-263">You'll get charts of response times, plus email notifications if your site goes down.</span></span>

![Příklad webového testu](./media/app-insights-java-get-started/appinsights-10webtestresult.png)

<span data-ttu-id="94bfe-265">[Další informace o testech dostupnosti webu.][availability]</span><span class="sxs-lookup"><span data-stu-id="94bfe-265">[Learn more about availability web tests.][availability]</span></span>

## <a name="questions-problems"></a><span data-ttu-id="94bfe-266">Máte dotazy?</span><span class="sxs-lookup"><span data-stu-id="94bfe-266">Questions?</span></span> <span data-ttu-id="94bfe-267">Problémy?</span><span class="sxs-lookup"><span data-stu-id="94bfe-267">Problems?</span></span>
[<span data-ttu-id="94bfe-268">Řešení potíží s Javou</span><span class="sxs-lookup"><span data-stu-id="94bfe-268">Troubleshooting Java</span></span>](app-insights-java-troubleshoot.md)

## <a name="video"></a><span data-ttu-id="94bfe-269">Video</span><span class="sxs-lookup"><span data-stu-id="94bfe-269">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="94bfe-270">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94bfe-270">Next steps</span></span>
* [<span data-ttu-id="94bfe-271">Monitorování volání závislostí</span><span class="sxs-lookup"><span data-stu-id="94bfe-271">Monitor dependency calls</span></span>](app-insights-java-agent.md)
* [<span data-ttu-id="94bfe-272">Monitorování čítačů výkonu Unix</span><span class="sxs-lookup"><span data-stu-id="94bfe-272">Monitor Unix performance counters</span></span>](app-insights-java-collectd.md)
* <span data-ttu-id="94bfe-273">Přidat [monitorování tooyour webové stránky](app-insights-javascript.md) načtení stránky toomonitor krát, volání AJAX výjimky prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="94bfe-273">Add [monitoring tooyour web pages](app-insights-javascript.md) toomonitor page load times, AJAX calls, browser exceptions.</span></span>
* <span data-ttu-id="94bfe-274">Zápis [vlastní telemetrii](app-insights-api-custom-events-metrics.md) tootrack využití v prohlížeči hello nebo na hello server.</span><span class="sxs-lookup"><span data-stu-id="94bfe-274">Write [custom telemetry](app-insights-api-custom-events-metrics.md) tootrack usage in hello browser or at hello server.</span></span>
* <span data-ttu-id="94bfe-275">Vytvoření [řídicí panely](app-insights-dashboards.md) toobring společně hello klíče grafy pro sledování systému.</span><span class="sxs-lookup"><span data-stu-id="94bfe-275">Create [dashboards](app-insights-dashboards.md) toobring together hello key charts for monitoring your system.</span></span>
* <span data-ttu-id="94bfe-276">[Analytické funkce](app-insights-analytics.md) vám pomůžou přímo z vaší aplikace zadávat efektivní dotazy na telemetrie.</span><span class="sxs-lookup"><span data-stu-id="94bfe-276">Use  [Analytics](app-insights-analytics.md) for powerful queries over telemetry from your app</span></span>
* <span data-ttu-id="94bfe-277">Další informace najdete na webu [Azure pro vývojáře v Javě](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="94bfe-277">For more information, visit [Azure for Java developers](/java/azure).</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#trackexception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-javascript.md
