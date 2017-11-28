---
title: "aaaGet začít s Azure Application Insights s Javou v prostředí Eclipse | Microsoft docs"
description: "Použít hello Eclipse modulu plug-in tooadd výkonu a využití sledování tooyour webu Java pomocí Application Insights"
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: e88c9f53-cd90-4abc-b097-1f170937908e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: bwren
ms.openlocfilehash: 3142a26a9e2d14c2c433882e3d337f2a8c8f2247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a><span data-ttu-id="76148-103">Začínáme s Application Insights s Javou v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="76148-103">Get started with Application Insights with Java in Eclipse</span></span>
<span data-ttu-id="76148-104">Hello Application Insights SDK odesílá telemetrii z webové aplikace Java, takže je můžete analyzovat využití a výkonu.</span><span class="sxs-lookup"><span data-stu-id="76148-104">hello Application Insights SDK sends telemetry from your Java web application so that you can analyze usage and performance.</span></span> <span data-ttu-id="76148-105">Hello Eclipse modulu plug-in pro službu Application Insights automaticky nainstaluje hello SDK do projektu, abyste měli mimo hello pole telemetrie a rozhraní API, které můžete použít vlastní telemetrii toowrite.</span><span class="sxs-lookup"><span data-stu-id="76148-105">hello Eclipse plug-in for Application Insights automatically installs hello SDK in your project so that you get out of hello box telemetry, plus an API that you can use toowrite custom telemetry.</span></span>   

## <a name="prerequisites"></a><span data-ttu-id="76148-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="76148-106">Prerequisites</span></span>
<span data-ttu-id="76148-107">Modul plug-in hello aktuálně funguje pro projekty Maven a dynamické webové projekty v prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="76148-107">Currently hello plug-in works for Maven projects and Dynamic Web Projects in Eclipse.</span></span>
<span data-ttu-id="76148-108">([Přidat službu Application Insights tooother typy projektu Java][java].)</span><span class="sxs-lookup"><span data-stu-id="76148-108">([Add Application Insights tooother types of Java project][java].)</span></span>

<span data-ttu-id="76148-109">Budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="76148-109">You'll need:</span></span>

* <span data-ttu-id="76148-110">Oracle JRE 1.6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="76148-110">Oracle JRE 1.6 or later</span></span>
* <span data-ttu-id="76148-111">Předplatné příliš[Microsoft Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="76148-111">A subscription too[Microsoft Azure](https://azure.microsoft.com/).</span></span>
* <span data-ttu-id="76148-112">[Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE](http://www.eclipse.org/downloads/), džínovinu nebo novější.</span><span class="sxs-lookup"><span data-stu-id="76148-112">[Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/), Indigo or later.</span></span>
* <span data-ttu-id="76148-113">Windows 7 nebo novější nebo Windows Server 2008 nebo novějším.</span><span class="sxs-lookup"><span data-stu-id="76148-113">Windows 7 or later, or Windows Server 2008 or later</span></span>

## <a name="install-hello-sdk-on-eclipse-one-time"></a><span data-ttu-id="76148-114">Nainstalujte hello SDK Eclipse (jednou)</span><span class="sxs-lookup"><span data-stu-id="76148-114">Install hello SDK on Eclipse (one time)</span></span>
<span data-ttu-id="76148-115">Můžete mít pouze toodo tento jednou pro každý počítač.</span><span class="sxs-lookup"><span data-stu-id="76148-115">You only have toodo this one time per machine.</span></span> <span data-ttu-id="76148-116">Tento krok nainstaluje sada nástrojů, který poté můžete přidat hello SDK tooeach Dynamic Web Project.</span><span class="sxs-lookup"><span data-stu-id="76148-116">This step installs a toolkit which can then add hello SDK tooeach Dynamic Web Project.</span></span>

1. <span data-ttu-id="76148-117">V prostředí Eclipse klikněte na tlačítko Nápověda, nainstalovat nový Software.</span><span class="sxs-lookup"><span data-stu-id="76148-117">In Eclipse, click Help, Install New Software.</span></span>

    ![Nápověda, instalace nového softwaru](./media/app-insights-java-eclipse/0-plugin.png)
2. <span data-ttu-id="76148-119">Hello SDK se http://dl.microsoft.com/eclipse v rámci Azure Toolkit.</span><span class="sxs-lookup"><span data-stu-id="76148-119">hello SDK is in http://dl.microsoft.com/eclipse, under Azure Toolkit.</span></span>
3. <span data-ttu-id="76148-120">Zrušte zaškrtnutí políčka **obraťte se na všechny lokality aktualizace...**</span><span class="sxs-lookup"><span data-stu-id="76148-120">Uncheck **Contact all update sites...**</span></span>

    ![Application Insights SDK zrušte vám poskytne všechny aktualizace lokality](./media/app-insights-java-eclipse/1-plugin.png)

<span data-ttu-id="76148-122">Postupujte podle zbývajících kroků pro každý projekt, Java hello.</span><span class="sxs-lookup"><span data-stu-id="76148-122">Follow hello remaining steps for each Java project.</span></span>

## <a name="create-an-application-insights-resource-in-azure"></a><span data-ttu-id="76148-123">Vytvořte prostředek Application Insights v Azure</span><span class="sxs-lookup"><span data-stu-id="76148-123">Create an Application Insights resource in Azure</span></span>
1. <span data-ttu-id="76148-124">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="76148-124">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="76148-125">Vytvořte nový prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="76148-125">Create a new Application Insights resource.</span></span> <span data-ttu-id="76148-126">Nastavit hello aplikace typu tooJava webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="76148-126">Set hello application type tooJava web application.</span></span>  

    ![Klikněte na + a vyberte Application Insights](./media/app-insights-java-eclipse/01-create.png)  

4. <span data-ttu-id="76148-128">Najít hello klíč instrumentace nového prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="76148-128">Find hello instrumentation key of hello new resource.</span></span> <span data-ttu-id="76148-129">Budete potřebovat toopaste tím do projektu kódu za chvíli.</span><span class="sxs-lookup"><span data-stu-id="76148-129">You'll need toopaste this into your code project shortly.</span></span>  

    ![V hello přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte hello klíč instrumentace](./media/app-insights-java-eclipse/03-key.png)  

## <a name="add-application-insights-tooyour-project"></a><span data-ttu-id="76148-131">Přidání Application Insights tooyour projektu</span><span class="sxs-lookup"><span data-stu-id="76148-131">Add Application Insights tooyour project</span></span>
1. <span data-ttu-id="76148-132">Přidejte Application Insights z kontextové nabídky hello webového projektu Java.</span><span class="sxs-lookup"><span data-stu-id="76148-132">Add Application Insights from hello context menu of your Java web project.</span></span>

    ![V hello přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte hello klíč instrumentace](./media/app-insights-java-eclipse/02-context-menu.png)
2. <span data-ttu-id="76148-134">Vložení hello klíč instrumentace, který jste získali z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="76148-134">Paste hello instrumentation key that you got from hello Azure portal.</span></span>

    ![V hello přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte hello klíč instrumentace](./media/app-insights-java-eclipse/03-ikey.png)

<span data-ttu-id="76148-136">klíč Hello se odesílají společně s každou položkou telemetrie a říká toodisplay Application Insights je ve vašem prostředku.</span><span class="sxs-lookup"><span data-stu-id="76148-136">hello key is sent along with every item of telemetry and tells Application Insights toodisplay it in your resource.</span></span>

## <a name="run-hello-application-and-see-metrics"></a><span data-ttu-id="76148-137">Spuštění aplikace hello a zobrazit metriky</span><span class="sxs-lookup"><span data-stu-id="76148-137">Run hello application and see metrics</span></span>
<span data-ttu-id="76148-138">Spusťte svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="76148-138">Run your application.</span></span>

<span data-ttu-id="76148-139">Vrátí prostředek Application Insights tooyour v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="76148-139">Return tooyour Application Insights resource in Microsoft Azure.</span></span>

<span data-ttu-id="76148-140">Data požadavků HTTP se zobrazí v okně Přehled hello.</span><span class="sxs-lookup"><span data-stu-id="76148-140">HTTP requests data will appear on hello overview blade.</span></span> <span data-ttu-id="76148-141">(Pokud zde nejsou, počkejte několik sekund a pak klikněte na tlačítko Aktualizovat.)</span><span class="sxs-lookup"><span data-stu-id="76148-141">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![<span data-ttu-id="76148-142">Odpověď serveru, počty žádostí a selhání</span><span class="sxs-lookup"><span data-stu-id="76148-142">Server response, request counts, and failures</span></span> ](./media/app-insights-java-eclipse/5-results.png)

<span data-ttu-id="76148-143">Klikněte na tlačítko prostřednictvím jakékoli grafu toosee podrobnější metriky.</span><span class="sxs-lookup"><span data-stu-id="76148-143">Click through any chart toosee more detailed metrics.</span></span>

![Počty žádostí podle názvu](./media/app-insights-java-eclipse/6-barchart.png)

<span data-ttu-id="76148-145">[Další informace o metrikách.][metrics]</span><span class="sxs-lookup"><span data-stu-id="76148-145">[Learn more about metrics.][metrics]</span></span>

<span data-ttu-id="76148-146">A při zobrazení vlastností hello požadavku, uvidíte hello telemetrické události související s například požadavky a výjimkami.</span><span class="sxs-lookup"><span data-stu-id="76148-146">And when viewing hello properties of a request, you can see hello telemetry events associated with it such as requests and exceptions.</span></span>

![Všechny trasování pro tento požadavek](./media/app-insights-java-eclipse/7-instance.png)

## <a name="client-side-telemetry"></a><span data-ttu-id="76148-148">Telemetrických dat na straně klienta</span><span class="sxs-lookup"><span data-stu-id="76148-148">Client-side telemetry</span></span>
<span data-ttu-id="76148-149">V okně rychlého startu hello klikněte na tlačítko získat toomonitor kódu webové stránky:</span><span class="sxs-lookup"><span data-stu-id="76148-149">From hello QuickStart blade, click Get code toomonitor my web pages:</span></span>

![V okně přehledu aplikace zvolte rychlý Start, získat kód toomonitor webové stránky.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

<span data-ttu-id="76148-152">Vložte fragment kódu hello hello head souborů HTML.</span><span class="sxs-lookup"><span data-stu-id="76148-152">Insert hello code snippet in hello head of your HTML files.</span></span>

#### <a name="view-client-side-data"></a><span data-ttu-id="76148-153">Zobrazení dat na straně klienta</span><span class="sxs-lookup"><span data-stu-id="76148-153">View client-side data</span></span>
<span data-ttu-id="76148-154">Otevřete aktualizované webových stránek a jejich použití.</span><span class="sxs-lookup"><span data-stu-id="76148-154">Open your updated web pages and use them.</span></span> <span data-ttu-id="76148-155">Počkejte minutu nebo dvě a poté vrátí tooApplication Insights a otevřete hello využití okno.</span><span class="sxs-lookup"><span data-stu-id="76148-155">Wait a minute or two, then return tooApplication Insights and open hello usage blade.</span></span> <span data-ttu-id="76148-156">(V okně Přehled hello, posuňte se dolů a klikněte na využití.)</span><span class="sxs-lookup"><span data-stu-id="76148-156">(From hello Overview blade, scroll down and click Usage.)</span></span>

<span data-ttu-id="76148-157">Stránka zobrazení, uživatele a relace metriky se zobrazí v okně využití hello:</span><span class="sxs-lookup"><span data-stu-id="76148-157">Page view, user, and session metrics will appear on hello usage blade:</span></span>

![Relace, uživatelů a zobrazení stránek](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

<span data-ttu-id="76148-159">[Další informace o nastavení telemetrických dat na straně klienta.][usage]</span><span class="sxs-lookup"><span data-stu-id="76148-159">[Learn more about setting up client-side telemetry.][usage]</span></span>

## <a name="publish-your-application"></a><span data-ttu-id="76148-160">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="76148-160">Publish your application</span></span>
<span data-ttu-id="76148-161">Teď publikujte aplikačního serveru toohello, uživatelé mohou ho použít a sledujte telemetrii hello objeví na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="76148-161">Now publish your app toohello server, let people use it, and watch hello telemetry show up on hello portal.</span></span>

* <span data-ttu-id="76148-162">Ujistěte se, že brána firewall umožňuje vaší aplikace toosend telemetrie toothese porty:</span><span class="sxs-lookup"><span data-stu-id="76148-162">Make sure your firewall allows your application toosend telemetry toothese ports:</span></span>

  * <span data-ttu-id="76148-163">dc.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="76148-163">dc.services.visualstudio.com:443</span></span>
  * <span data-ttu-id="76148-164">dc.services.visualstudio.com:80</span><span class="sxs-lookup"><span data-stu-id="76148-164">dc.services.visualstudio.com:80</span></span>
  * <span data-ttu-id="76148-165">f5.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="76148-165">f5.services.visualstudio.com:443</span></span>
  * <span data-ttu-id="76148-166">f5.services.visualstudio.com:80</span><span class="sxs-lookup"><span data-stu-id="76148-166">f5.services.visualstudio.com:80</span></span>
* <span data-ttu-id="76148-167">Na serverech Windows nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="76148-167">On Windows servers, install:</span></span>

  * [<span data-ttu-id="76148-168">Microsoft Visual C++ Redistributable</span><span class="sxs-lookup"><span data-stu-id="76148-168">Microsoft Visual C++ Redistributable</span></span>](http://www.microsoft.com/download/details.aspx?id=40784)

    <span data-ttu-id="76148-169">(Umožňuje čítače výkonu.)</span><span class="sxs-lookup"><span data-stu-id="76148-169">(This enables performance counters.)</span></span>

## <a name="exceptions-and-request-failures"></a><span data-ttu-id="76148-170">Výjimky a chyby požadavků</span><span class="sxs-lookup"><span data-stu-id="76148-170">Exceptions and request failures</span></span>
<span data-ttu-id="76148-171">Nezpracované výjimky jsou shromažďovány automaticky:</span><span class="sxs-lookup"><span data-stu-id="76148-171">Unhandled exceptions are automatically collected:</span></span>

![](./media/app-insights-java-eclipse/21-exceptions.png)

<span data-ttu-id="76148-172">toocollect data o dalších výjimkách, máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="76148-172">toocollect data on other exceptions, you have two options:</span></span>

* <span data-ttu-id="76148-173">[Vložení volání tooTrackException ve vašem kódu](app-insights-api-custom-events-metrics.md#trackexception).</span><span class="sxs-lookup"><span data-stu-id="76148-173">[Insert calls tooTrackException in your code](app-insights-api-custom-events-metrics.md#trackexception).</span></span>
* <span data-ttu-id="76148-174">[Nainstalujte na server agenta Java hello](app-insights-java-agent.md).</span><span class="sxs-lookup"><span data-stu-id="76148-174">[Install hello Java Agent on your server](app-insights-java-agent.md).</span></span> <span data-ttu-id="76148-175">Zadáte hello metody, které chcete toowatch.</span><span class="sxs-lookup"><span data-stu-id="76148-175">You specify hello methods you want toowatch.</span></span>

## <a name="monitor-method-calls-and-external-dependencies"></a><span data-ttu-id="76148-176">Volání metody monitorování a externí závislosti</span><span class="sxs-lookup"><span data-stu-id="76148-176">Monitor method calls and external dependencies</span></span>
<span data-ttu-id="76148-177">[Nainstalujte agenta Java hello](app-insights-java-agent.md) toolog zadané vnitřních metod a volání provedená prostřednictvím JDBC s daty časování.</span><span class="sxs-lookup"><span data-stu-id="76148-177">[Install hello Java Agent](app-insights-java-agent.md) toolog specified internal methods and calls made through JDBC, with timing data.</span></span>

## <a name="performance-counters"></a><span data-ttu-id="76148-178">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="76148-178">Performance counters</span></span>
<span data-ttu-id="76148-179">V okně vaší přehled přejděte dolů a klikněte na tlačítko hello **servery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="76148-179">On your Overview blade, scroll down and click hello  **Servers** tile.</span></span> <span data-ttu-id="76148-180">Uvidíte rozsah čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="76148-180">You'll see a range of performance counters.</span></span>

![Posuňte se dolů tooclick hello servery dlaždice](./media/app-insights-java-eclipse/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a><span data-ttu-id="76148-182">Vlastní nastavení kolekce čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="76148-182">Customize performance counter collection</span></span>
<span data-ttu-id="76148-183">kolekce toodisable hello standardní sady čítačů výkonu, přidejte následující kód pod hello kořenového uzlu souboru ApplicationInsights.xml hello hello:</span><span class="sxs-lookup"><span data-stu-id="76148-183">toodisable collection of hello standard set of performance counters, add hello following code under hello root node of hello ApplicationInsights.xml file:</span></span>

```XML

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a><span data-ttu-id="76148-184">Shromažďování dalších čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="76148-184">Collect additional performance counters</span></span>
<span data-ttu-id="76148-185">Můžete zadat další výkonu čítače toobe shromážděných.</span><span class="sxs-lookup"><span data-stu-id="76148-185">You can specify additional performance counters toobe collected.</span></span>

#### <a name="jmx-counters-exposed-by-hello-java-virtual-machine"></a><span data-ttu-id="76148-186">Čítače JMX (vystavené ve virtuálním počítači Java hello)</span><span class="sxs-lookup"><span data-stu-id="76148-186">JMX counters (exposed by hello Java Virtual Machine)</span></span>

```XML

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* <span data-ttu-id="76148-187">`displayName`– hello název zobrazený na portálu služby Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="76148-187">`displayName` – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="76148-188">`objectName`– Název objektu JMX hello.</span><span class="sxs-lookup"><span data-stu-id="76148-188">`objectName` – hello JMX object name.</span></span>
* <span data-ttu-id="76148-189">`attribute`– atribut hello toofetch název objektu JMX hello</span><span class="sxs-lookup"><span data-stu-id="76148-189">`attribute` – hello attribute of hello JMX object name toofetch</span></span>
* <span data-ttu-id="76148-190">`type`(volitelné) – hello typ atributu JMX objektu:</span><span class="sxs-lookup"><span data-stu-id="76148-190">`type` (optional) - hello type of JMX object’s attribute:</span></span>
  * <span data-ttu-id="76148-191">Výchozí hodnota: jednoduchý typ, například int nebo long.</span><span class="sxs-lookup"><span data-stu-id="76148-191">Default: a simple type such as int or long.</span></span>
  * <span data-ttu-id="76148-192">`composite`: data čítače výkonu hello je ve formátu "Attribute.Data" hello</span><span class="sxs-lookup"><span data-stu-id="76148-192">`composite`: hello perf counter data is in hello format of 'Attribute.Data'</span></span>
  * <span data-ttu-id="76148-193">`tabular`: data čítače výkonu hello je ve formátu hello řádku tabulky</span><span class="sxs-lookup"><span data-stu-id="76148-193">`tabular`: hello perf counter data is in hello format of a table row</span></span>

#### <a name="windows-performance-counters"></a><span data-ttu-id="76148-194">Čítače výkonu Windows</span><span class="sxs-lookup"><span data-stu-id="76148-194">Windows performance counters</span></span>
<span data-ttu-id="76148-195">Každý [čítačů výkonu systému Windows](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) je členem určité kategorie (v hello stejným způsobem, že je pole členem třídy).</span><span class="sxs-lookup"><span data-stu-id="76148-195">Each [Windows performance counter](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) is a member of a category (in hello same way that a field is a member of a class).</span></span> <span data-ttu-id="76148-196">Kategorie mohou být buď globální, nebo mohou mít číslované nebo pojmenované instance.</span><span class="sxs-lookup"><span data-stu-id="76148-196">Categories can either be global, or can have numbered or named instances.</span></span>

```XML

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* <span data-ttu-id="76148-197">displayName – název hello zobrazený na portálu služby Application Insights hello.</span><span class="sxs-lookup"><span data-stu-id="76148-197">displayName – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="76148-198">categoryName – hello kategorie čítače výkonu (objekt výkonu) ke kterému je přiřazeno tento čítač výkonu.</span><span class="sxs-lookup"><span data-stu-id="76148-198">categoryName – hello performance counter category (performance object) with which this performance counter is associated.</span></span>
* <span data-ttu-id="76148-199">counterName – název čítače výkonu hello hello.</span><span class="sxs-lookup"><span data-stu-id="76148-199">counterName – hello name of hello performance counter.</span></span>
* <span data-ttu-id="76148-200">instanceName – název hello hello instance kategorie čítače výkonu nebo prázdný řetězec (""), pokud hello kategorie obsahuje jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="76148-200">instanceName – hello name of hello performance counter category instance, or an empty string (""), if hello category contains a single instance.</span></span> <span data-ttu-id="76148-201">Pokud je hello categoryName proces a hello chcete toocollect je hodnota čítače výkonu z aktuálního procesu JVM hello na, které běží vaše aplikace, zadejte `"__SELF__"`.</span><span class="sxs-lookup"><span data-stu-id="76148-201">If hello categoryName is Process, and hello performance counter you'd like toocollect is from hello current JVM process on which your app is running, specify `"__SELF__"`.</span></span>

<span data-ttu-id="76148-202">Čítače výkonu jsou zobrazené jako vlastní metriky v [Průzkumníku metrik][metrics].</span><span class="sxs-lookup"><span data-stu-id="76148-202">Your performance counters are visible as custom metrics in [Metrics Explorer][metrics].</span></span>

![](./media/app-insights-java-eclipse/12-custom-perfs.png)

### <a name="unix-performance-counters"></a><span data-ttu-id="76148-203">Čítače výkonu Unix</span><span class="sxs-lookup"><span data-stu-id="76148-203">Unix performance counters</span></span>
* <span data-ttu-id="76148-204">[Nainstalujte collectd s plug-in Application Insights hello](app-insights-java-collectd.md) tooget celou řadu dat systému a sítě.</span><span class="sxs-lookup"><span data-stu-id="76148-204">[Install collectd with hello Application Insights plugin](app-insights-java-collectd.md) tooget a wide variety of system and network data.</span></span>

## <a name="availability-web-tests"></a><span data-ttu-id="76148-205">Testy dostupnosti webu</span><span class="sxs-lookup"><span data-stu-id="76148-205">Availability web tests</span></span>
<span data-ttu-id="76148-206">Application Insights může otestovat váš web v pravidelných intervalech toocheck, zda je funkční a dobře reaguje.</span><span class="sxs-lookup"><span data-stu-id="76148-206">Application Insights can test your website at regular intervals toocheck that it's up and responding well.</span></span> <span data-ttu-id="76148-207">[tooset až][availability], projděte dolů tooclick dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="76148-207">[tooset up][availability], scroll down tooclick Availability.</span></span>

![Posuňte se dolů, klikněte na tlačítko Dostupnost a pak Přidat test webu](./media/app-insights-java-eclipse/31-config-web-test.png)

<span data-ttu-id="76148-209">Získáte tabulky s dobami odezvy a navíc e-mailová oznámení, pokud váš web nefunguje.</span><span class="sxs-lookup"><span data-stu-id="76148-209">You'll get charts of response times, plus email notifications if your site goes down.</span></span>

![Příklad webového testu](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

<span data-ttu-id="76148-211">[Další informace o testech dostupnosti webu.][availability]</span><span class="sxs-lookup"><span data-stu-id="76148-211">[Learn more about availability web tests.][availability]</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="76148-212">Diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="76148-212">Diagnostic logs</span></span>
<span data-ttu-id="76148-213">Pokud používáte Logback nebo Log4J (verze 1.2 nebo v2.0) pro trasování, může mít vaše protokoly trasování automaticky odešlou tooApplication statistiky, které vám umožní zkoumat a hledat v nich.</span><span class="sxs-lookup"><span data-stu-id="76148-213">If you're using Logback or Log4J (v1.2 or v2.0) for tracing, you can have your trace logs sent automatically tooApplication Insights where you can explore and search on them.</span></span>

<span data-ttu-id="76148-214">[Další informace o diagnostických protokolů][javalogs]</span><span class="sxs-lookup"><span data-stu-id="76148-214">[Learn more about diagnostic logs][javalogs]</span></span>

## <a name="custom-telemetry"></a><span data-ttu-id="76148-215">Vlastní telemetrii</span><span class="sxs-lookup"><span data-stu-id="76148-215">Custom telemetry</span></span>
<span data-ttu-id="76148-216">Vložte po zadání několika řádků kódu do vaší toofind Java webové aplikace se co uživatelé dělají s ním nebo toohelp diagnostikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="76148-216">Insert a few lines of code in your Java web application toofind out what users are doing with it or toohelp diagnose problems.</span></span>

<span data-ttu-id="76148-217">Kód můžete vložit na webové stránce JavaScript i v jazyce Java serverové hello.</span><span class="sxs-lookup"><span data-stu-id="76148-217">You can insert code both in web page JavaScript and in hello server-side Java.</span></span>

<span data-ttu-id="76148-218">[Další informace o vlastní telemetrii][track]</span><span class="sxs-lookup"><span data-stu-id="76148-218">[Learn about custom telemetry][track]</span></span>

## <a name="next-steps"></a><span data-ttu-id="76148-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="76148-219">Next steps</span></span>
#### <a name="detect-and-diagnose-issues"></a><span data-ttu-id="76148-220">Najít a diagnostikovat problémy</span><span class="sxs-lookup"><span data-stu-id="76148-220">Detect and diagnose issues</span></span>
* <span data-ttu-id="76148-221">[Přidat telemetrie webového klienta] [ usage] tooget výkonu telemetrie z hello webového klienta.</span><span class="sxs-lookup"><span data-stu-id="76148-221">[Add web client telemetry][usage] tooget performance telemetry from hello web client.</span></span>
* <span data-ttu-id="76148-222">[Nastavit testy webu] [ availability] toomake, že vaše aplikace zůstává aktivní a reagující.</span><span class="sxs-lookup"><span data-stu-id="76148-222">[Set up web tests][availability] toomake sure your application stays live and responsive.</span></span>
* <span data-ttu-id="76148-223">[Hledání událostí a protokolů] [ diagnostic] toohelp diagnostikovat problémy.</span><span class="sxs-lookup"><span data-stu-id="76148-223">[Search events and logs][diagnostic] toohelp diagnose problems.</span></span>
* <span data-ttu-id="76148-224">[Zaznamenat trasování Log4J nebo Logback][javalogs]</span><span class="sxs-lookup"><span data-stu-id="76148-224">[Capture Log4J or Logback traces][javalogs]</span></span>

#### <a name="track-usage"></a><span data-ttu-id="76148-225">Sledovat využití</span><span class="sxs-lookup"><span data-stu-id="76148-225">Track usage</span></span>
* <span data-ttu-id="76148-226">[Přidat telemetrie webového klienta] [ usage] toomonitor stránky zobrazení a metriky základní uživatel.</span><span class="sxs-lookup"><span data-stu-id="76148-226">[Add web client telemetry][usage] toomonitor page views and basic user metrics.</span></span>
* <span data-ttu-id="76148-227">[Sledujte vlastní události a metriky](app-insights-web-track-usage.md) toolearn o tom, jak se používá aplikace, jak v hello klient a hello server.</span><span class="sxs-lookup"><span data-stu-id="76148-227">[Track custom events and metrics](app-insights-web-track-usage.md) toolearn about how your application is used, both at hello client and hello server.</span></span>

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md
