---
title: "Začínáme s Azure Application Insights s Javou v prostředí Eclipse | Microsoft docs"
description: "Pomocí modulu plug-in Eclipse přidejte výkon a sledování využití do vašeho webu Java pomocí Application Insights"
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
ms.openlocfilehash: f2f696a3bbe7893c1f521a3e5588f4f93805d6a2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a><span data-ttu-id="09055-103">Začínáme s Application Insights s Javou v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="09055-103">Get started with Application Insights with Java in Eclipse</span></span>
<span data-ttu-id="09055-104">Application Insights SDK odesílá telemetrii z webové aplikace Java, takže je můžete analyzovat využití a výkonu.</span><span class="sxs-lookup"><span data-stu-id="09055-104">The Application Insights SDK sends telemetry from your Java web application so that you can analyze usage and performance.</span></span> <span data-ttu-id="09055-105">Modul plug-in pro službu Application Insights Eclipse automaticky nainstaluje sady SDK v projektu, abyste měli mimo pole telemetrie a rozhraní API, které můžete psát vlastní telemetrii.</span><span class="sxs-lookup"><span data-stu-id="09055-105">The Eclipse plug-in for Application Insights automatically installs the SDK in your project so that you get out of the box telemetry, plus an API that you can use to write custom telemetry.</span></span>   

## <a name="prerequisites"></a><span data-ttu-id="09055-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="09055-106">Prerequisites</span></span>
<span data-ttu-id="09055-107">Aktuálně modul plug-in funguje pro projekty Maven a dynamické webové projekty v prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="09055-107">Currently the plug-in works for Maven projects and Dynamic Web Projects in Eclipse.</span></span>
<span data-ttu-id="09055-108">([Přidat službu Application Insights na jiné typy projektu Java][java].)</span><span class="sxs-lookup"><span data-stu-id="09055-108">([Add Application Insights to other types of Java project][java].)</span></span>

<span data-ttu-id="09055-109">Budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="09055-109">You'll need:</span></span>

* <span data-ttu-id="09055-110">Oracle JRE 1.6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="09055-110">Oracle JRE 1.6 or later</span></span>
* <span data-ttu-id="09055-111">Předplatné [Microsoft Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="09055-111">A subscription to [Microsoft Azure](https://azure.microsoft.com/).</span></span>
* <span data-ttu-id="09055-112">[Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE](http://www.eclipse.org/downloads/), džínovinu nebo novější.</span><span class="sxs-lookup"><span data-stu-id="09055-112">[Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/), Indigo or later.</span></span>
* <span data-ttu-id="09055-113">Windows 7 nebo novější nebo Windows Server 2008 nebo novějším.</span><span class="sxs-lookup"><span data-stu-id="09055-113">Windows 7 or later, or Windows Server 2008 or later</span></span>

## <a name="install-the-sdk-on-eclipse-one-time"></a><span data-ttu-id="09055-114">Instalace sady SDK v prostředí Eclipse (jednou)</span><span class="sxs-lookup"><span data-stu-id="09055-114">Install the SDK on Eclipse (one time)</span></span>
<span data-ttu-id="09055-115">Stačí udělat tento jednou pro každý počítač.</span><span class="sxs-lookup"><span data-stu-id="09055-115">You only have to do this one time per machine.</span></span> <span data-ttu-id="09055-116">Tento krok nainstaluje sada nástrojů, který poté můžete přidat sadu SDK do každé Dynamic Web Project.</span><span class="sxs-lookup"><span data-stu-id="09055-116">This step installs a toolkit which can then add the SDK to each Dynamic Web Project.</span></span>

1. <span data-ttu-id="09055-117">V prostředí Eclipse klikněte na tlačítko Nápověda, nainstalovat nový Software.</span><span class="sxs-lookup"><span data-stu-id="09055-117">In Eclipse, click Help, Install New Software.</span></span>

    ![Nápověda, instalace nového softwaru](./media/app-insights-java-eclipse/0-plugin.png)
2. <span data-ttu-id="09055-119">Sada SDK se http://dl.microsoft.com/eclipse v rámci Azure Toolkit.</span><span class="sxs-lookup"><span data-stu-id="09055-119">The SDK is in http://dl.microsoft.com/eclipse, under Azure Toolkit.</span></span>
3. <span data-ttu-id="09055-120">Zrušte zaškrtnutí políčka **obraťte se na všechny lokality aktualizace...**</span><span class="sxs-lookup"><span data-stu-id="09055-120">Uncheck **Contact all update sites...**</span></span>

    ![Application Insights SDK zrušte vám poskytne všechny aktualizace lokality](./media/app-insights-java-eclipse/1-plugin.png)

<span data-ttu-id="09055-122">Postupujte podle zbývajících kroků pro každý projekt, Java.</span><span class="sxs-lookup"><span data-stu-id="09055-122">Follow the remaining steps for each Java project.</span></span>

## <a name="create-an-application-insights-resource-in-azure"></a><span data-ttu-id="09055-123">Vytvořte prostředek Application Insights v Azure</span><span class="sxs-lookup"><span data-stu-id="09055-123">Create an Application Insights resource in Azure</span></span>
1. <span data-ttu-id="09055-124">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="09055-124">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="09055-125">Vytvořte nový prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="09055-125">Create a new Application Insights resource.</span></span> <span data-ttu-id="09055-126">Nastavte typ aplikace na webovou aplikaci Java.</span><span class="sxs-lookup"><span data-stu-id="09055-126">Set the application type to Java web application.</span></span>  

    ![Klikněte na + a vyberte Application Insights](./media/app-insights-java-eclipse/01-create.png)  

4. <span data-ttu-id="09055-128">Najděte klíč instrumentace nového prostředku.</span><span class="sxs-lookup"><span data-stu-id="09055-128">Find the instrumentation key of the new resource.</span></span> <span data-ttu-id="09055-129">Ten budete muset krátce vložit do projektu kódu.</span><span class="sxs-lookup"><span data-stu-id="09055-129">You'll need to paste this into your code project shortly.</span></span>  

    ![V přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte klíč instrumentace](./media/app-insights-java-eclipse/03-key.png)  

## <a name="add-application-insights-to-your-project"></a><span data-ttu-id="09055-131">Přidejte ke svému projektu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="09055-131">Add Application Insights to your project</span></span>
1. <span data-ttu-id="09055-132">Přidejte Application Insights z místní nabídky projektu webové Java.</span><span class="sxs-lookup"><span data-stu-id="09055-132">Add Application Insights from the context menu of your Java web project.</span></span>

    ![V přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte klíč instrumentace](./media/app-insights-java-eclipse/02-context-menu.png)
2. <span data-ttu-id="09055-134">Vložte klíč instrumentace, který jste získali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="09055-134">Paste the instrumentation key that you got from the Azure portal.</span></span>

    ![V přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte klíč instrumentace](./media/app-insights-java-eclipse/03-ikey.png)

<span data-ttu-id="09055-136">Klíč je odeslán společně s každou položkou telemetrie a říká službě Application Insights, aby ho zobrazila v prostředku.</span><span class="sxs-lookup"><span data-stu-id="09055-136">The key is sent along with every item of telemetry and tells Application Insights to display it in your resource.</span></span>

## <a name="run-the-application-and-see-metrics"></a><span data-ttu-id="09055-137">Spusťte aplikaci a zobrazit metriky</span><span class="sxs-lookup"><span data-stu-id="09055-137">Run the application and see metrics</span></span>
<span data-ttu-id="09055-138">Spusťte svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="09055-138">Run your application.</span></span>

<span data-ttu-id="09055-139">Vraťte se do zdroje Application Insights v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="09055-139">Return to your Application Insights resource in Microsoft Azure.</span></span>

<span data-ttu-id="09055-140">Data požadavků HTTP se zobrazí v okně přehled.</span><span class="sxs-lookup"><span data-stu-id="09055-140">HTTP requests data will appear on the overview blade.</span></span> <span data-ttu-id="09055-141">(Pokud zde nejsou, počkejte několik sekund a pak klikněte na tlačítko Aktualizovat.)</span><span class="sxs-lookup"><span data-stu-id="09055-141">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![<span data-ttu-id="09055-142">Odpověď serveru, počty žádostí a selhání</span><span class="sxs-lookup"><span data-stu-id="09055-142">Server response, request counts, and failures</span></span> ](./media/app-insights-java-eclipse/5-results.png)

<span data-ttu-id="09055-143">Proklikejte se prostřednictvím jakékoli grafu pro zobrazení podrobnějších metrik.</span><span class="sxs-lookup"><span data-stu-id="09055-143">Click through any chart to see more detailed metrics.</span></span>

![Počty žádostí podle názvu](./media/app-insights-java-eclipse/6-barchart.png)

<span data-ttu-id="09055-145">[Další informace o metrikách.][metrics]</span><span class="sxs-lookup"><span data-stu-id="09055-145">[Learn more about metrics.][metrics]</span></span>

<span data-ttu-id="09055-146">A při zobrazení vlastností požadavku, uvidíte telemetrické události související s například požadavky a výjimkami.</span><span class="sxs-lookup"><span data-stu-id="09055-146">And when viewing the properties of a request, you can see the telemetry events associated with it such as requests and exceptions.</span></span>

![Všechny trasování pro tento požadavek](./media/app-insights-java-eclipse/7-instance.png)

## <a name="client-side-telemetry"></a><span data-ttu-id="09055-148">Telemetrických dat na straně klienta</span><span class="sxs-lookup"><span data-stu-id="09055-148">Client-side telemetry</span></span>
<span data-ttu-id="09055-149">V okně Rychlý Start klikněte na tlačítko Get kód ke sledování mých webových stránek:</span><span class="sxs-lookup"><span data-stu-id="09055-149">From the QuickStart blade, click Get code to monitor my web pages:</span></span>

![V okně přehledu aplikace zvolte Rychlý start, získat kód ke sledování webové stránky.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

<span data-ttu-id="09055-152">Vložte fragment kódu v hlavě soubory ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="09055-152">Insert the code snippet in the head of your HTML files.</span></span>

#### <a name="view-client-side-data"></a><span data-ttu-id="09055-153">Zobrazení dat na straně klienta</span><span class="sxs-lookup"><span data-stu-id="09055-153">View client-side data</span></span>
<span data-ttu-id="09055-154">Otevřete aktualizované webových stránek a jejich použití.</span><span class="sxs-lookup"><span data-stu-id="09055-154">Open your updated web pages and use them.</span></span> <span data-ttu-id="09055-155">Počkejte minutu nebo dvě, pak se vraťte do Application Insights a otevřete okno využití.</span><span class="sxs-lookup"><span data-stu-id="09055-155">Wait a minute or two, then return to Application Insights and open the usage blade.</span></span> <span data-ttu-id="09055-156">(V okně Přehled přejděte dolů a klikněte na využití.)</span><span class="sxs-lookup"><span data-stu-id="09055-156">(From the Overview blade, scroll down and click Usage.)</span></span>

<span data-ttu-id="09055-157">Stránka zobrazení, uživatele a relace metriky se zobrazí v okně využití:</span><span class="sxs-lookup"><span data-stu-id="09055-157">Page view, user, and session metrics will appear on the usage blade:</span></span>

![Relace, uživatelů a zobrazení stránek](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

<span data-ttu-id="09055-159">[Další informace o nastavení telemetrických dat na straně klienta.][usage]</span><span class="sxs-lookup"><span data-stu-id="09055-159">[Learn more about setting up client-side telemetry.][usage]</span></span>

## <a name="publish-your-application"></a><span data-ttu-id="09055-160">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="09055-160">Publish your application</span></span>
<span data-ttu-id="09055-161">Teď publikujte aplikaci na server, dovolte osobám ji používat a sledujte telemetrii zobrazenou na portálu.</span><span class="sxs-lookup"><span data-stu-id="09055-161">Now publish your app to the server, let people use it, and watch the telemetry show up on the portal.</span></span>

* <span data-ttu-id="09055-162">Ujistěte se, že brána firewall umožňuje vaší aplikace odesílat telemetrii na tyto porty:</span><span class="sxs-lookup"><span data-stu-id="09055-162">Make sure your firewall allows your application to send telemetry to these ports:</span></span>

  * <span data-ttu-id="09055-163">dc.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="09055-163">dc.services.visualstudio.com:443</span></span>
  * <span data-ttu-id="09055-164">dc.services.visualstudio.com:80</span><span class="sxs-lookup"><span data-stu-id="09055-164">dc.services.visualstudio.com:80</span></span>
  * <span data-ttu-id="09055-165">f5.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="09055-165">f5.services.visualstudio.com:443</span></span>
  * <span data-ttu-id="09055-166">f5.services.visualstudio.com:80</span><span class="sxs-lookup"><span data-stu-id="09055-166">f5.services.visualstudio.com:80</span></span>
* <span data-ttu-id="09055-167">Na serverech Windows nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="09055-167">On Windows servers, install:</span></span>

  * [<span data-ttu-id="09055-168">Microsoft Visual C++ Redistributable</span><span class="sxs-lookup"><span data-stu-id="09055-168">Microsoft Visual C++ Redistributable</span></span>](http://www.microsoft.com/download/details.aspx?id=40784)

    <span data-ttu-id="09055-169">(Umožňuje čítače výkonu.)</span><span class="sxs-lookup"><span data-stu-id="09055-169">(This enables performance counters.)</span></span>

## <a name="exceptions-and-request-failures"></a><span data-ttu-id="09055-170">Výjimky a chyby požadavků</span><span class="sxs-lookup"><span data-stu-id="09055-170">Exceptions and request failures</span></span>
<span data-ttu-id="09055-171">Nezpracované výjimky jsou shromažďovány automaticky:</span><span class="sxs-lookup"><span data-stu-id="09055-171">Unhandled exceptions are automatically collected:</span></span>

![](./media/app-insights-java-eclipse/21-exceptions.png)

<span data-ttu-id="09055-172">Chcete-li shromažďovat data o dalších výjimkách, máte dvě možnosti:</span><span class="sxs-lookup"><span data-stu-id="09055-172">To collect data on other exceptions, you have two options:</span></span>

* <span data-ttu-id="09055-173">[Vložit volání pro TrackException ve vašem kódu](app-insights-api-custom-events-metrics.md#trackexception).</span><span class="sxs-lookup"><span data-stu-id="09055-173">[Insert calls to TrackException in your code](app-insights-api-custom-events-metrics.md#trackexception).</span></span>
* <span data-ttu-id="09055-174">[Nainstalovat na server agenta Java](app-insights-java-agent.md).</span><span class="sxs-lookup"><span data-stu-id="09055-174">[Install the Java Agent on your server](app-insights-java-agent.md).</span></span> <span data-ttu-id="09055-175">Určete metody, které chcete sledovat.</span><span class="sxs-lookup"><span data-stu-id="09055-175">You specify the methods you want to watch.</span></span>

## <a name="monitor-method-calls-and-external-dependencies"></a><span data-ttu-id="09055-176">Volání metody monitorování a externí závislosti</span><span class="sxs-lookup"><span data-stu-id="09055-176">Monitor method calls and external dependencies</span></span>
<span data-ttu-id="09055-177">[Nainstalujte agenta Java](app-insights-java-agent.md) k protokolování určených vnitřních metod a volání provedená prostřednictvím JDBC s daty časování.</span><span class="sxs-lookup"><span data-stu-id="09055-177">[Install the Java Agent](app-insights-java-agent.md) to log specified internal methods and calls made through JDBC, with timing data.</span></span>

## <a name="performance-counters"></a><span data-ttu-id="09055-178">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="09055-178">Performance counters</span></span>
<span data-ttu-id="09055-179">V okně vaší přehled přejděte dolů a kliknutím **servery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="09055-179">On your Overview blade, scroll down and click the  **Servers** tile.</span></span> <span data-ttu-id="09055-180">Uvidíte rozsah čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="09055-180">You'll see a range of performance counters.</span></span>

![Posuňte se dolů a klikněte na dlaždici servery](./media/app-insights-java-eclipse/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a><span data-ttu-id="09055-182">Vlastní nastavení kolekce čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="09055-182">Customize performance counter collection</span></span>
<span data-ttu-id="09055-183">Pro zakázání shromažďování standardní sady čítačů výkonu přidejte následující kód do kořenového uzlu souboru ApplicationInsights.xml:</span><span class="sxs-lookup"><span data-stu-id="09055-183">To disable collection of the standard set of performance counters, add the following code under the root node of the ApplicationInsights.xml file:</span></span>

```XML

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a><span data-ttu-id="09055-184">Shromažďování dalších čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="09055-184">Collect additional performance counters</span></span>
<span data-ttu-id="09055-185">Můžete zadat další čítače výkonu, které se mají shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="09055-185">You can specify additional performance counters to be collected.</span></span>

#### <a name="jmx-counters-exposed-by-the-java-virtual-machine"></a><span data-ttu-id="09055-186">Čítače JMX (vystavené ve virtuálním počítači Java)</span><span class="sxs-lookup"><span data-stu-id="09055-186">JMX counters (exposed by the Java Virtual Machine)</span></span>

```XML

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* <span data-ttu-id="09055-187">`displayName` – název zobrazený na portálu služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="09055-187">`displayName` – The name displayed in the Application Insights portal.</span></span>
* <span data-ttu-id="09055-188">`objectName` – název objektu JMX</span><span class="sxs-lookup"><span data-stu-id="09055-188">`objectName` – The JMX object name.</span></span>
* <span data-ttu-id="09055-189">`attribute` – atribut názvu objektu JMX k načtení</span><span class="sxs-lookup"><span data-stu-id="09055-189">`attribute` – The attribute of the JMX object name to fetch</span></span>
* <span data-ttu-id="09055-190">`type`(volitelné) – typ atributu JMX objektu:</span><span class="sxs-lookup"><span data-stu-id="09055-190">`type` (optional) - The type of JMX object’s attribute:</span></span>
  * <span data-ttu-id="09055-191">Výchozí hodnota: jednoduchý typ, například int nebo long.</span><span class="sxs-lookup"><span data-stu-id="09055-191">Default: a simple type such as int or long.</span></span>
  * <span data-ttu-id="09055-192">`composite`: data čítače výkonu jsou ve formátu „Attribute.Data“</span><span class="sxs-lookup"><span data-stu-id="09055-192">`composite`: the perf counter data is in the format of 'Attribute.Data'</span></span>
  * <span data-ttu-id="09055-193">`tabular`: data čítače výkonu jsou ve formátu řádku tabulky</span><span class="sxs-lookup"><span data-stu-id="09055-193">`tabular`: the perf counter data is in the format of a table row</span></span>

#### <a name="windows-performance-counters"></a><span data-ttu-id="09055-194">Čítače výkonu Windows</span><span class="sxs-lookup"><span data-stu-id="09055-194">Windows performance counters</span></span>
<span data-ttu-id="09055-195">Každý [čítač výkonu systému Windows](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) je členem určité kategorie (stejným způsobem, jakým je pole členem třídy).</span><span class="sxs-lookup"><span data-stu-id="09055-195">Each [Windows performance counter](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) is a member of a category (in the same way that a field is a member of a class).</span></span> <span data-ttu-id="09055-196">Kategorie mohou být buď globální, nebo mohou mít číslované nebo pojmenované instance.</span><span class="sxs-lookup"><span data-stu-id="09055-196">Categories can either be global, or can have numbered or named instances.</span></span>

```XML

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* <span data-ttu-id="09055-197">displayName – Název zobrazený na portálu služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="09055-197">displayName – The name displayed in the Application Insights portal.</span></span>
* <span data-ttu-id="09055-198">categoryName – kategorie čítače výkonu (objekt výkonu) ke kterému je přiřazen tento čítač výkonu.</span><span class="sxs-lookup"><span data-stu-id="09055-198">categoryName – The performance counter category (performance object) with which this performance counter is associated.</span></span>
* <span data-ttu-id="09055-199">counterName – název čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="09055-199">counterName – The name of the performance counter.</span></span>
* <span data-ttu-id="09055-200">instanceName – název instance kategorie čítače výkonu nebo prázdný řetězec (""), pokud kategorie obsahuje jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="09055-200">instanceName – The name of the performance counter category instance, or an empty string (""), if the category contains a single instance.</span></span> <span data-ttu-id="09055-201">Pokud je categoryName proces a čítač výkonu, který chcete shromáždit, pochází z aktuálního procesu JVM, na kterém běží vaše aplikace, zadejte `"__SELF__"`.</span><span class="sxs-lookup"><span data-stu-id="09055-201">If the categoryName is Process, and the performance counter you'd like to collect is from the current JVM process on which your app is running, specify `"__SELF__"`.</span></span>

<span data-ttu-id="09055-202">Čítače výkonu jsou zobrazené jako vlastní metriky v [Průzkumníku metrik][metrics].</span><span class="sxs-lookup"><span data-stu-id="09055-202">Your performance counters are visible as custom metrics in [Metrics Explorer][metrics].</span></span>

![](./media/app-insights-java-eclipse/12-custom-perfs.png)

### <a name="unix-performance-counters"></a><span data-ttu-id="09055-203">Čítače výkonu Unix</span><span class="sxs-lookup"><span data-stu-id="09055-203">Unix performance counters</span></span>
* <span data-ttu-id="09055-204">[Nainstalujte collectd s modulem plug-in Application Insights](app-insights-java-collectd.md) a získejte celou řadu dat systému a sítě.</span><span class="sxs-lookup"><span data-stu-id="09055-204">[Install collectd with the Application Insights plugin](app-insights-java-collectd.md) to get a wide variety of system and network data.</span></span>

## <a name="availability-web-tests"></a><span data-ttu-id="09055-205">Testy dostupnosti webu</span><span class="sxs-lookup"><span data-stu-id="09055-205">Availability web tests</span></span>
<span data-ttu-id="09055-206">Application Insights může otestovat váš web v pravidelných intervalech a zkontrolovat, zda je funkční a dobře reaguje.</span><span class="sxs-lookup"><span data-stu-id="09055-206">Application Insights can test your website at regular intervals to check that it's up and responding well.</span></span> <span data-ttu-id="09055-207">[Nastavit][availability], přejděte dolů a klikněte na tlačítko dostupnost.</span><span class="sxs-lookup"><span data-stu-id="09055-207">[To set up][availability], scroll down to click Availability.</span></span>

![Posuňte se dolů, klikněte na tlačítko Dostupnost a pak Přidat test webu](./media/app-insights-java-eclipse/31-config-web-test.png)

<span data-ttu-id="09055-209">Získáte tabulky s dobami odezvy a navíc e-mailová oznámení, pokud váš web nefunguje.</span><span class="sxs-lookup"><span data-stu-id="09055-209">You'll get charts of response times, plus email notifications if your site goes down.</span></span>

![Příklad webového testu](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

<span data-ttu-id="09055-211">[Další informace o testech dostupnosti webu.][availability]</span><span class="sxs-lookup"><span data-stu-id="09055-211">[Learn more about availability web tests.][availability]</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="09055-212">Diagnostické protokoly</span><span class="sxs-lookup"><span data-stu-id="09055-212">Diagnostic logs</span></span>
<span data-ttu-id="09055-213">Pokud používáte Logback nebo Log4J (verze 1.2 nebo v2.0) pro trasování, může mít vaše protokoly trasování automaticky odešlou do Application Insights, kde vám umožní zkoumat a hledat v nich.</span><span class="sxs-lookup"><span data-stu-id="09055-213">If you're using Logback or Log4J (v1.2 or v2.0) for tracing, you can have your trace logs sent automatically to Application Insights where you can explore and search on them.</span></span>

<span data-ttu-id="09055-214">[Další informace o diagnostických protokolů][javalogs]</span><span class="sxs-lookup"><span data-stu-id="09055-214">[Learn more about diagnostic logs][javalogs]</span></span>

## <a name="custom-telemetry"></a><span data-ttu-id="09055-215">Vlastní telemetrii</span><span class="sxs-lookup"><span data-stu-id="09055-215">Custom telemetry</span></span>
<span data-ttu-id="09055-216">Po zadání několika řádků kódu vložte ve webové aplikaci Java a zjistěte, co uživatelé dělají s ním nebo k diagnostice potíží.</span><span class="sxs-lookup"><span data-stu-id="09055-216">Insert a few lines of code in your Java web application to find out what users are doing with it or to help diagnose problems.</span></span>

<span data-ttu-id="09055-217">Kód můžete vložit na webové stránce JavaScript i v jazyce Java na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="09055-217">You can insert code both in web page JavaScript and in the server-side Java.</span></span>

<span data-ttu-id="09055-218">[Další informace o vlastní telemetrii][track]</span><span class="sxs-lookup"><span data-stu-id="09055-218">[Learn about custom telemetry][track]</span></span>

## <a name="next-steps"></a><span data-ttu-id="09055-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="09055-219">Next steps</span></span>
#### <a name="detect-and-diagnose-issues"></a><span data-ttu-id="09055-220">Najít a diagnostikovat problémy</span><span class="sxs-lookup"><span data-stu-id="09055-220">Detect and diagnose issues</span></span>
* <span data-ttu-id="09055-221">[Přidat telemetrie webového klienta] [ usage] sám výkonu telemetrie webového klienta.</span><span class="sxs-lookup"><span data-stu-id="09055-221">[Add web client telemetry][usage] to get performance telemetry from the web client.</span></span>
* <span data-ttu-id="09055-222">[Nastavení webových testů][availability] pro zajištění, že aplikace zůstane funkční a bude reagovat.</span><span class="sxs-lookup"><span data-stu-id="09055-222">[Set up web tests][availability] to make sure your application stays live and responsive.</span></span>
* <span data-ttu-id="09055-223">[Prohledávejte události a protokoly][diagnostic] pro pomoc s diagnostikou problémů.</span><span class="sxs-lookup"><span data-stu-id="09055-223">[Search events and logs][diagnostic] to help diagnose problems.</span></span>
* <span data-ttu-id="09055-224">[Zaznamenat trasování Log4J nebo Logback][javalogs]</span><span class="sxs-lookup"><span data-stu-id="09055-224">[Capture Log4J or Logback traces][javalogs]</span></span>

#### <a name="track-usage"></a><span data-ttu-id="09055-225">Sledovat využití</span><span class="sxs-lookup"><span data-stu-id="09055-225">Track usage</span></span>
* <span data-ttu-id="09055-226">[Přidat telemetrie webového klienta] [ usage] monitorování zobrazení stránek a metrik základní uživatele.</span><span class="sxs-lookup"><span data-stu-id="09055-226">[Add web client telemetry][usage] to monitor page views and basic user metrics.</span></span>
* <span data-ttu-id="09055-227">[Sledujte vlastní události a metriky](app-insights-web-track-usage.md) Další informace o tom, jak se používá aplikace, jak na klientovi a na serveru.</span><span class="sxs-lookup"><span data-stu-id="09055-227">[Track custom events and metrics](app-insights-web-track-usage.md) to learn about how your application is used, both at the client and the server.</span></span>

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md
