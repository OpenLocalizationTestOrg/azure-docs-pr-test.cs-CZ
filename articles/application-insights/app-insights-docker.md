---
title: "Monitorování aplikací Docker ve službě Azure Application Insights | Microsoft Docs"
description: "Čítače výkonu docker, události a výjimek lze zobrazit na Application Insights, společně s telemetrii z kontejnerizované aplikací."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 27a3083d-d67f-4a07-8f3c-4edb65a0a685
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: b082e345ca1bb3b12c548e05e699474d3aa9306c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-docker-applications-in-application-insights"></a><span data-ttu-id="6f6c8-103">Monitorování Docker aplikací ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="6f6c8-103">Monitor Docker applications in Application Insights</span></span>
<span data-ttu-id="6f6c8-104">Události životního cyklu a výkonu čítače z [Docker](https://www.docker.com/) kontejnery může být v grafu zobrazena na Application Insights.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-104">Lifecycle events and performance counters from [Docker](https://www.docker.com/) containers can be charted on Application Insights.</span></span> <span data-ttu-id="6f6c8-105">Nainstalujte [Application Insights](app-insights-overview.md) bitové kopie v kontejneru v hostiteli a zobrazí čítače výkonu pro hostitele, a také pro jiné bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-105">Install the [Application Insights](app-insights-overview.md) image in a container in your host, and it will display performance counters for the host, as well as for the other images.</span></span>

<span data-ttu-id="6f6c8-106">Pomocí Docker distribuovat aplikace v lightweight kontejnery kompletní s všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-106">With Docker, you distribute your apps in lightweight containers complete with all dependencies.</span></span> <span data-ttu-id="6f6c8-107">Budete běží na jakýkoli počítač hostitele, který spouští modul Docker.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-107">They'll run on any host machine that runs a Docker Engine.</span></span>

<span data-ttu-id="6f6c8-108">Při spuštění [Application Insights image](https://hub.docker.com/r/microsoft/applicationinsights/) hostiteli Docker získáte tyto výhody:</span><span class="sxs-lookup"><span data-stu-id="6f6c8-108">When you run the [Application Insights image](https://hub.docker.com/r/microsoft/applicationinsights/) on your Docker host, you get these benefits:</span></span>

* <span data-ttu-id="6f6c8-109">Životní cyklus telemetrii týkající se všechny kontejnery spuštěná v hostiteli - spuštění, zastavení a tak dále.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-109">Lifecycle telemetry about all the containers running on the host - start, stop, and so on.</span></span>
* <span data-ttu-id="6f6c8-110">Čítače výkonu pro všechny kontejnery.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-110">Performance counters for all the containers.</span></span> <span data-ttu-id="6f6c8-111">Využití procesoru, paměti, využití sítě a další.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-111">CPU, memory, network usage, and more.</span></span>
* <span data-ttu-id="6f6c8-112">Pokud jste [nainstalován Application Insights SDK pro jazyk Java](app-insights-java-live.md) ve aplikace běžící v kontejnerech, budou všechny telemetrická těchto aplikací mít další vlastnosti identifikace počítače kontejneru a hostitele.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-112">If you [installed Application Insights SDK for Java](app-insights-java-live.md) in the apps running in the containers, all the telemetry of those apps will have additional properties identifying the container and host machine.</span></span> <span data-ttu-id="6f6c8-113">Tak například pokud máte instance aplikace spuštěná ve více než jednomu hostiteli, lze snadno filtrovat telemetrie aplikace hostitele.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-113">So for example, if you have instances of an app running in more than one host, you can easily filter your app telemetry by host.</span></span>

![Příklad](./media/app-insights-docker/00.png)

## <a name="set-up-your-application-insights-resource"></a><span data-ttu-id="6f6c8-115">Nastavit zdroje Application Insights</span><span class="sxs-lookup"><span data-stu-id="6f6c8-115">Set up your Application Insights resource</span></span>
1. <span data-ttu-id="6f6c8-116">Přihlaste se k [portálu Microsoft Azure](https://azure.com) a otevřete prostředek Application Insights pro vaše aplikace, nebo [vytvořte novou](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="6f6c8-116">Sign into [Microsoft Azure portal](https://azure.com) and open the Application Insights resource for your app; or [create a new one](app-insights-create-new-resource.md).</span></span> 
   
    <span data-ttu-id="6f6c8-117">*Který prostředek by měl používat?*</span><span class="sxs-lookup"><span data-stu-id="6f6c8-117">*Which resource should I use?*</span></span> <span data-ttu-id="6f6c8-118">Pokud aplikace, které jsou spuštěné v hostiteli vyvinul někdo jiný, pak budete muset [vytvořte nový prostředek Application Insights](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="6f6c8-118">If the apps that you are running on your host were developed by someone else, then you need to [create a new Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="6f6c8-119">Toto je, kde můžete zobrazit a analyzovat telemetrii.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-119">This is where you view and analyze the telemetry.</span></span> <span data-ttu-id="6f6c8-120">(Vyberte obecné pro typ aplikace.)</span><span class="sxs-lookup"><span data-stu-id="6f6c8-120">(Select 'General' for the app type.)</span></span>
   
    <span data-ttu-id="6f6c8-121">Ale pokud jste vývojář aplikace, pak Věříme, že jste [přidat Application Insights SDK](app-insights-java-live.md) ke každému z nich.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-121">But if you're the developer of the apps, then we hope you [added Application Insights SDK](app-insights-java-live.md) to each of them.</span></span> <span data-ttu-id="6f6c8-122">Pokud jsou všechny skutečně součásti jeden obchodní aplikace, můžete pak nakonfigurovat všechny z nich k odesílání telemetrie na jeden prostředek a použijete stejný prostředku pro zobrazení dat Docker životního cyklu a výkonu.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-122">If they're all really components of a single business application, then you might configure all of them to send telemetry to one resource, and you'll use that same resource to display the Docker lifecycle and performance data.</span></span> 
   
    <span data-ttu-id="6f6c8-123">Třetí scénář je, že vyvinuté většinu aplikací, ale používáte samostatné prostředků k zobrazení jejich telemetrie.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-123">A third scenario is that you developed most of the apps, but you are using separate resources to display their telemetry.</span></span> <span data-ttu-id="6f6c8-124">V takovém případě budete pravděpodobně také chtít vytvořit samostatnou prostředků pro Docker data.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-124">In that case, you probably also want to create a separate resource for the Docker data.</span></span> 
2. <span data-ttu-id="6f6c8-125">Přidat dlaždice Docker: Zvolte **přidat dlaždici**, přetáhněte dlaždici Docker z galerie a pak klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-125">Add the Docker tile: Choose **Add Tile**, drag the Docker tile from the gallery, and then click **Done**.</span></span> 
   
    ![Příklad](./media/app-insights-docker/03.png)
3. <span data-ttu-id="6f6c8-127">Klikněte **Essentials** rozevíracího seznamu a zkopírujte klíč instrumentace.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-127">Click the **Essentials** drop-down and copy the Instrumentation Key.</span></span> <span data-ttu-id="6f6c8-128">To slouží k říct sadě SDK, kam má posílat jeho telemetrie.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-128">You use this to tell the SDK where to send its telemetry.</span></span>

    ![Příklad](./media/app-insights-docker/02-props.png)

<span data-ttu-id="6f6c8-130">Nechte okno prohlížeče užitečný, jak budete se vraťte k jeho brzo podívejte se na vaší telemetrie.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-130">Keep that browser window handy, as you'll come back to it soon to look at your telemetry.</span></span>

## <a name="run-the-application-insights-monitor-on-your-host"></a><span data-ttu-id="6f6c8-131">Spustit monitorování Application Insights na hostiteli</span><span class="sxs-lookup"><span data-stu-id="6f6c8-131">Run the Application Insights monitor on your host</span></span>
<span data-ttu-id="6f6c8-132">Teď, když máte k dispozici někde zobrazit telemetrii, můžete nastavit kontejnerizované aplikaci, která bude shromažďovat a odesílat je.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-132">Now that you've got somewhere to display the telemetry, you can set up the containerized app that will collect and send it.</span></span>

1. <span data-ttu-id="6f6c8-133">Připojte k hostiteli Docker.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-133">Connect to your Docker host.</span></span> 
2. <span data-ttu-id="6f6c8-134">Upravit klíč instrumentace do tento příkaz a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="6f6c8-134">Edit your instrumentation key into this command, and then run it:</span></span>
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

<span data-ttu-id="6f6c8-135">Pouze jeden obrázek Application Insights je vyžadován na Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-135">Only one Application Insights image is required per Docker host.</span></span> <span data-ttu-id="6f6c8-136">Pokud vaše aplikace je nasazená na několika hostitelích Docker, pak opakujte příkaz na každém hostiteli.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-136">If your application is deployed on multiple Docker hosts, then repeat the command on every host.</span></span>

## <a name="update-your-app"></a><span data-ttu-id="6f6c8-137">Aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="6f6c8-137">Update your app</span></span>
<span data-ttu-id="6f6c8-138">Pokud vaše aplikace není instrumentována s [Application Insights SDK pro jazyk Java](app-insights-java-get-started.md), přidejte následující řádek do souboru ApplicationInsights.xml ve vašem projektu v části `<TelemetryInitializers>` element:</span><span class="sxs-lookup"><span data-stu-id="6f6c8-138">If your application is instrumented with the [Application Insights SDK for Java](app-insights-java-get-started.md), add the following line into the ApplicationInsights.xml file in your project, under the `<TelemetryInitializers>` element:</span></span>

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

<span data-ttu-id="6f6c8-139">Tento postup přidá informace o Docker například kontejneru a id hostitele každé položce telemetrie odeslaných z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-139">This adds Docker information such as container and host id to every telemetry item sent from your app.</span></span>

## <a name="view-your-telemetry"></a><span data-ttu-id="6f6c8-140">Zobrazení telemetrie</span><span class="sxs-lookup"><span data-stu-id="6f6c8-140">View your telemetry</span></span>
<span data-ttu-id="6f6c8-141">Přejděte zpět do zdroje Application Insights na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-141">Go back to your Application Insights resource in the Azure portal.</span></span>

<span data-ttu-id="6f6c8-142">Proklikejte se prostřednictvím Docker dlaždice.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-142">Click through the Docker tile.</span></span>

<span data-ttu-id="6f6c8-143">Krátce zobrazí dat odesílaných z aplikace Docker, zvláště pokud máte jiné kontejnery systémem Docker modul.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-143">You'll shortly see data arriving from the Docker app, especially if you have other containers running on your Docker engine.</span></span>

<span data-ttu-id="6f6c8-144">Zde jsou některé zobrazení, která můžete získat.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-144">Here are some of the views you can get.</span></span>

### <a name="perf-counters-by-host-activity-by-image"></a><span data-ttu-id="6f6c8-145">Čítače výkonu hostitele, aktivita podle bitové kopie</span><span class="sxs-lookup"><span data-stu-id="6f6c8-145">Perf counters by host, activity by image</span></span>
![Příklad](./media/app-insights-docker/10.png)

![Příklad](./media/app-insights-docker/11.png)

<span data-ttu-id="6f6c8-148">Klikněte na libovolný název hostitele nebo bitové kopie pro více podrobností.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-148">Click any host or image name for more detail.</span></span>

<span data-ttu-id="6f6c8-149">Chcete-li přizpůsobit zobrazení, klikněte na libovolný graf mřížky záhlaví, nebo použijte přidat graf.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-149">To customize the view, click any chart, the grid heading, or use Add Chart.</span></span> 

<span data-ttu-id="6f6c8-150">[Další informace o Průzkumníku metrik](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="6f6c8-150">[Learn more about metrics explorer](app-insights-metrics-explorer.md).</span></span>

### <a name="docker-container-events"></a><span data-ttu-id="6f6c8-151">Události kontejner docker</span><span class="sxs-lookup"><span data-stu-id="6f6c8-151">Docker container events</span></span>
![Příklad](./media/app-insights-docker/13.png)

<span data-ttu-id="6f6c8-153">Chcete-li prozkoumat jednotlivé události, klikněte na tlačítko [vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="6f6c8-153">To investigate individual events, click [Search](app-insights-diagnostic-search.md).</span></span> <span data-ttu-id="6f6c8-154">Hledat a filtr k událostem, které chcete najít.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-154">Search and filter to find the events you want.</span></span> <span data-ttu-id="6f6c8-155">Kliknutím na libovolnou událost zobrazíte další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-155">Click any event to get more detail.</span></span>

### <a name="exceptions-by-container-name"></a><span data-ttu-id="6f6c8-156">Výjimky podle názvu kontejneru</span><span class="sxs-lookup"><span data-stu-id="6f6c8-156">Exceptions by container name</span></span>
![Příklad](./media/app-insights-docker/14.png)

### <a name="docker-context-added-to-app-telemetry"></a><span data-ttu-id="6f6c8-158">Kontext docker přidat do aplikace telemetrie</span><span class="sxs-lookup"><span data-stu-id="6f6c8-158">Docker context added to app telemetry</span></span>
<span data-ttu-id="6f6c8-159">Žádost o telemetrické zprávy odesílané z aplikace instrumentovány s AI SDK a rozšíření s kontextem Docker:</span><span class="sxs-lookup"><span data-stu-id="6f6c8-159">Request telemetry sent from the application instrumented with AI SDK, enriched with Docker context:</span></span>

![Příklad](./media/app-insights-docker/16.png)

<span data-ttu-id="6f6c8-161">Čas procesoru a paměti k dispozici čítače výkonu, rozšíření a seskupené podle Docker název kontejneru:</span><span class="sxs-lookup"><span data-stu-id="6f6c8-161">Processor time and available memory performance counters, enriched and grouped by Docker container name:</span></span>

![Příklad](./media/app-insights-docker/15.png)

## <a name="q--a"></a><span data-ttu-id="6f6c8-163">Dotazy a odpovědi</span><span class="sxs-lookup"><span data-stu-id="6f6c8-163">Q & A</span></span>
<span data-ttu-id="6f6c8-164">*Co Application Insights mě, nemůže dostat z Docker?*</span><span class="sxs-lookup"><span data-stu-id="6f6c8-164">*What does Application Insights give me that I can't get from Docker?*</span></span>

* <span data-ttu-id="6f6c8-165">Podrobné rozpis čítačů výkonu tak, že kontejner a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-165">Detailed breakdown of performance counters by container and image.</span></span>
* <span data-ttu-id="6f6c8-166">Integrace aplikace a kontejneru dat v jednom řídicím.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-166">Integrate container and app data in one dashboard.</span></span>
* <span data-ttu-id="6f6c8-167">[Export telemetrie](app-insights-export-telemetry.md) k další analýze databáze, Power BI nebo jiné řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-167">[Export telemetry](app-insights-export-telemetry.md) for further analysis to a database, Power BI or other dashboard.</span></span>

<span data-ttu-id="6f6c8-168">*Získání telemetrických dat z aplikace*</span><span class="sxs-lookup"><span data-stu-id="6f6c8-168">*How do I get telemetry from the app itself?*</span></span>

* <span data-ttu-id="6f6c8-169">Nainstalujte službu Application Insights SDK v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f6c8-169">Install the Application Insights SDK in the app.</span></span> <span data-ttu-id="6f6c8-170">Zjistěte, jak pro: [webové aplikace v jazyce Java](app-insights-java-get-started.md), [Windows webové aplikace](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="6f6c8-170">Learn how for: [Java web apps](app-insights-java-get-started.md), [Windows web apps](app-insights-asp-net.md).</span></span>

## <a name="video"></a><span data-ttu-id="6f6c8-171">Video</span><span class="sxs-lookup"><span data-stu-id="6f6c8-171">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="6f6c8-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f6c8-172">Next steps</span></span>

* [<span data-ttu-id="6f6c8-173">Application Insights pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="6f6c8-173">Application Insights for Java</span></span>](app-insights-java-get-started.md)
* [<span data-ttu-id="6f6c8-174">Application Insights pro Node.js</span><span class="sxs-lookup"><span data-stu-id="6f6c8-174">Application Insights for Node.js</span></span>](app-insights-nodejs.md)
* [<span data-ttu-id="6f6c8-175">Application Insights pro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6f6c8-175">Application Insights for ASP.NET</span></span>](app-insights-asp-net.md)
