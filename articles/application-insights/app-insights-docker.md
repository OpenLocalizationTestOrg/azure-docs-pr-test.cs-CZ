---
title: "aaaMonitor Docker aplikací ve službě Azure Application Insights | Microsoft Docs"
description: "Čítače výkonu docker, události a výjimek lze zobrazit na Application Insights, společně s hello telemetrie z hello kontejnerové aplikace."
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
ms.openlocfilehash: 9aaf1076bae25485a396db1bb3dcd13bccd87c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-docker-applications-in-application-insights"></a><span data-ttu-id="50464-103">Monitorování Docker aplikací ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="50464-103">Monitor Docker applications in Application Insights</span></span>
<span data-ttu-id="50464-104">Události životního cyklu a výkonu čítače z [Docker](https://www.docker.com/) kontejnery může být v grafu zobrazena na Application Insights.</span><span class="sxs-lookup"><span data-stu-id="50464-104">Lifecycle events and performance counters from [Docker](https://www.docker.com/) containers can be charted on Application Insights.</span></span> <span data-ttu-id="50464-105">Nainstalujte hello [Application Insights](app-insights-overview.md) bitové kopie v kontejneru v hostiteli a zobrazí čítače výkonu pro hello hostitele, jak dobře jako u hello ostatní Image.</span><span class="sxs-lookup"><span data-stu-id="50464-105">Install hello [Application Insights](app-insights-overview.md) image in a container in your host, and it will display performance counters for hello host, as well as for hello other images.</span></span>

<span data-ttu-id="50464-106">Pomocí Docker distribuovat aplikace v lightweight kontejnery kompletní s všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="50464-106">With Docker, you distribute your apps in lightweight containers complete with all dependencies.</span></span> <span data-ttu-id="50464-107">Budete běží na jakýkoli počítač hostitele, který spouští modul Docker.</span><span class="sxs-lookup"><span data-stu-id="50464-107">They'll run on any host machine that runs a Docker Engine.</span></span>

<span data-ttu-id="50464-108">Když spustíte hello [Application Insights image](https://hub.docker.com/r/microsoft/applicationinsights/) hostiteli Docker získáte tyto výhody:</span><span class="sxs-lookup"><span data-stu-id="50464-108">When you run hello [Application Insights image](https://hub.docker.com/r/microsoft/applicationinsights/) on your Docker host, you get these benefits:</span></span>

* <span data-ttu-id="50464-109">Životní cyklus telemetrii týkající se všechny kontejnery hello spuštěná na hostiteli hello - spuštění, zastavení a tak dále.</span><span class="sxs-lookup"><span data-stu-id="50464-109">Lifecycle telemetry about all hello containers running on hello host - start, stop, and so on.</span></span>
* <span data-ttu-id="50464-110">Čítače výkonu pro všechny kontejnery hello.</span><span class="sxs-lookup"><span data-stu-id="50464-110">Performance counters for all hello containers.</span></span> <span data-ttu-id="50464-111">Využití procesoru, paměti, využití sítě a další.</span><span class="sxs-lookup"><span data-stu-id="50464-111">CPU, memory, network usage, and more.</span></span>
* <span data-ttu-id="50464-112">Pokud jste [nainstalován Application Insights SDK pro jazyk Java](app-insights-java-live.md) ve hello aplikace běžící v kontejnerech hello, budou všechny telemetrická hello těchto aplikací mít další vlastnosti, které určíte hello kontejneru a hostitelský počítač.</span><span class="sxs-lookup"><span data-stu-id="50464-112">If you [installed Application Insights SDK for Java](app-insights-java-live.md) in hello apps running in hello containers, all hello telemetry of those apps will have additional properties identifying hello container and host machine.</span></span> <span data-ttu-id="50464-113">Tak například pokud máte instance aplikace spuštěná ve více než jednomu hostiteli, lze snadno filtrovat telemetrie aplikace hostitele.</span><span class="sxs-lookup"><span data-stu-id="50464-113">So for example, if you have instances of an app running in more than one host, you can easily filter your app telemetry by host.</span></span>

![Příklad](./media/app-insights-docker/00.png)

## <a name="set-up-your-application-insights-resource"></a><span data-ttu-id="50464-115">Nastavit zdroje Application Insights</span><span class="sxs-lookup"><span data-stu-id="50464-115">Set up your Application Insights resource</span></span>
1. <span data-ttu-id="50464-116">Přihlaste se k [portálu Microsoft Azure](https://azure.com) a otevřete prostředek Application Insights hello aplikace, nebo [vytvořte novou](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="50464-116">Sign into [Microsoft Azure portal](https://azure.com) and open hello Application Insights resource for your app; or [create a new one](app-insights-create-new-resource.md).</span></span> 
   
    <span data-ttu-id="50464-117">*Který prostředek by měl používat?*</span><span class="sxs-lookup"><span data-stu-id="50464-117">*Which resource should I use?*</span></span> <span data-ttu-id="50464-118">Pokud někdo vyvinul hello aplikace, které jsou spuštěné v hostiteli, pak musíte příliš[vytvořte nový prostředek Application Insights](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="50464-118">If hello apps that you are running on your host were developed by someone else, then you need too[create a new Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="50464-119">Toto je, kde můžete zobrazit a analyzovat hello telemetrie.</span><span class="sxs-lookup"><span data-stu-id="50464-119">This is where you view and analyze hello telemetry.</span></span> <span data-ttu-id="50464-120">(Vyberte obecné pro typ aplikace hello.)</span><span class="sxs-lookup"><span data-stu-id="50464-120">(Select 'General' for hello app type.)</span></span>
   
    <span data-ttu-id="50464-121">Ale pokud jste vývojář hello hello aplikace, pak Věříme, že jste [přidat Application Insights SDK](app-insights-java-live.md) tooeach z nich.</span><span class="sxs-lookup"><span data-stu-id="50464-121">But if you're hello developer of hello apps, then we hope you [added Application Insights SDK](app-insights-java-live.md) tooeach of them.</span></span> <span data-ttu-id="50464-122">Pokud jsou všechny skutečně součásti jeden obchodní aplikace, pak můžete nakonfigurovat všechny z nich toosend telemetrie tooone prostředků a budete používat tato prostředků toodisplay hello Docker životního cyklu a výkonu data.</span><span class="sxs-lookup"><span data-stu-id="50464-122">If they're all really components of a single business application, then you might configure all of them toosend telemetry tooone resource, and you'll use that same resource toodisplay hello Docker lifecycle and performance data.</span></span> 
   
    <span data-ttu-id="50464-123">Třetí scénář je vyvinuté většinu aplikací hello, že používáte samostatné prostředky toodisplay jejich telemetrie.</span><span class="sxs-lookup"><span data-stu-id="50464-123">A third scenario is that you developed most of hello apps, but you are using separate resources toodisplay their telemetry.</span></span> <span data-ttu-id="50464-124">V takovém případě budete pravděpodobně také chcete toocreate samostatné prostředku pro hello Docker data.</span><span class="sxs-lookup"><span data-stu-id="50464-124">In that case, you probably also want toocreate a separate resource for hello Docker data.</span></span> 
2. <span data-ttu-id="50464-125">Přidat hello Docker dlaždice: Zvolte **přidat dlaždici**, přetáhněte dlaždici Docker hello z Galerie hello a pak klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="50464-125">Add hello Docker tile: Choose **Add Tile**, drag hello Docker tile from hello gallery, and then click **Done**.</span></span> 
   
    ![Příklad](./media/app-insights-docker/03.png)
3. <span data-ttu-id="50464-127">Klikněte na tlačítko hello **Essentials** rozevíracího seznamu a zkopírujte klíč instrumentace hello.</span><span class="sxs-lookup"><span data-stu-id="50464-127">Click hello **Essentials** drop-down and copy hello Instrumentation Key.</span></span> <span data-ttu-id="50464-128">Použít tento tootell hello SDK kde toosend jeho telemetrie.</span><span class="sxs-lookup"><span data-stu-id="50464-128">You use this tootell hello SDK where toosend its telemetry.</span></span>

    ![Příklad](./media/app-insights-docker/02-props.png)

<span data-ttu-id="50464-130">Nechte okno prohlížeče užitečný, jak je budete vraťte tooit brzy toolook v telemetrii.</span><span class="sxs-lookup"><span data-stu-id="50464-130">Keep that browser window handy, as you'll come back tooit soon toolook at your telemetry.</span></span>

## <a name="run-hello-application-insights-monitor-on-your-host"></a><span data-ttu-id="50464-131">Spustit monitorování hello Application Insights na hostiteli</span><span class="sxs-lookup"><span data-stu-id="50464-131">Run hello Application Insights monitor on your host</span></span>
<span data-ttu-id="50464-132">Teď, když máte někde toodisplay hello telemetrie, můžete nastavit hello kontejnerizované aplikaci, která bude shromažďovat a odesílat je.</span><span class="sxs-lookup"><span data-stu-id="50464-132">Now that you've got somewhere toodisplay hello telemetry, you can set up hello containerized app that will collect and send it.</span></span>

1. <span data-ttu-id="50464-133">Připojte hostitele Docker tooyour.</span><span class="sxs-lookup"><span data-stu-id="50464-133">Connect tooyour Docker host.</span></span> 
2. <span data-ttu-id="50464-134">Upravit klíč instrumentace do tento příkaz a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="50464-134">Edit your instrumentation key into this command, and then run it:</span></span>
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

<span data-ttu-id="50464-135">Pouze jeden obrázek Application Insights je vyžadován na Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="50464-135">Only one Application Insights image is required per Docker host.</span></span> <span data-ttu-id="50464-136">Pokud vaše aplikace je nasazená na několika hostitelích Docker, pak opakujte příkaz hello na každém hostiteli.</span><span class="sxs-lookup"><span data-stu-id="50464-136">If your application is deployed on multiple Docker hosts, then repeat hello command on every host.</span></span>

## <a name="update-your-app"></a><span data-ttu-id="50464-137">Aktualizace aplikace</span><span class="sxs-lookup"><span data-stu-id="50464-137">Update your app</span></span>
<span data-ttu-id="50464-138">Pokud vaše aplikace není instrumentována s hello [Application Insights SDK pro jazyk Java](app-insights-java-get-started.md), přidejte následující řádek do souboru ApplicationInsights.xml hello ve vašem projektu, v části hello hello `<TelemetryInitializers>` element:</span><span class="sxs-lookup"><span data-stu-id="50464-138">If your application is instrumented with hello [Application Insights SDK for Java](app-insights-java-get-started.md), add hello following line into hello ApplicationInsights.xml file in your project, under hello `<TelemetryInitializers>` element:</span></span>

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

<span data-ttu-id="50464-139">Tento postup přidá Docker informace, jako je například kontejneru a hostitele id tooevery telemetrie položek odeslaných z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="50464-139">This adds Docker information such as container and host id tooevery telemetry item sent from your app.</span></span>

## <a name="view-your-telemetry"></a><span data-ttu-id="50464-140">Zobrazení telemetrie</span><span class="sxs-lookup"><span data-stu-id="50464-140">View your telemetry</span></span>
<span data-ttu-id="50464-141">Vraťte se zpátky prostředek Application Insights tooyour hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="50464-141">Go back tooyour Application Insights resource in hello Azure portal.</span></span>

<span data-ttu-id="50464-142">Proklikejte se prostřednictvím hello Docker dlaždice.</span><span class="sxs-lookup"><span data-stu-id="50464-142">Click through hello Docker tile.</span></span>

<span data-ttu-id="50464-143">Krátce zobrazí dat odesílaných z aplikace hello Docker, zvláště pokud máte jiné kontejnery systémem Docker modul.</span><span class="sxs-lookup"><span data-stu-id="50464-143">You'll shortly see data arriving from hello Docker app, especially if you have other containers running on your Docker engine.</span></span>

<span data-ttu-id="50464-144">Tady jsou některé z hello zobrazení, které můžete získat.</span><span class="sxs-lookup"><span data-stu-id="50464-144">Here are some of hello views you can get.</span></span>

### <a name="perf-counters-by-host-activity-by-image"></a><span data-ttu-id="50464-145">Čítače výkonu hostitele, aktivita podle bitové kopie</span><span class="sxs-lookup"><span data-stu-id="50464-145">Perf counters by host, activity by image</span></span>
![Příklad](./media/app-insights-docker/10.png)

![Příklad](./media/app-insights-docker/11.png)

<span data-ttu-id="50464-148">Klikněte na libovolný název hostitele nebo bitové kopie pro více podrobností.</span><span class="sxs-lookup"><span data-stu-id="50464-148">Click any host or image name for more detail.</span></span>

<span data-ttu-id="50464-149">toocustomize hello zobrazení, klikněte na libovolný graf, tabulka hello záhlaví, nebo pomocí přidat graf.</span><span class="sxs-lookup"><span data-stu-id="50464-149">toocustomize hello view, click any chart, hello grid heading, or use Add Chart.</span></span> 

<span data-ttu-id="50464-150">[Další informace o Průzkumníku metrik](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="50464-150">[Learn more about metrics explorer](app-insights-metrics-explorer.md).</span></span>

### <a name="docker-container-events"></a><span data-ttu-id="50464-151">Události kontejner docker</span><span class="sxs-lookup"><span data-stu-id="50464-151">Docker container events</span></span>
![Příklad](./media/app-insights-docker/13.png)

<span data-ttu-id="50464-153">Klikněte na jednotlivé události tooinvestigate, [vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="50464-153">tooinvestigate individual events, click [Search](app-insights-diagnostic-search.md).</span></span> <span data-ttu-id="50464-154">Hledat a filtr toofind hello události, které chcete.</span><span class="sxs-lookup"><span data-stu-id="50464-154">Search and filter toofind hello events you want.</span></span> <span data-ttu-id="50464-155">Další podrobnosti kliknutím na tlačítko tooget žádné události.</span><span class="sxs-lookup"><span data-stu-id="50464-155">Click any event tooget more detail.</span></span>

### <a name="exceptions-by-container-name"></a><span data-ttu-id="50464-156">Výjimky podle názvu kontejneru</span><span class="sxs-lookup"><span data-stu-id="50464-156">Exceptions by container name</span></span>
![Příklad](./media/app-insights-docker/14.png)

### <a name="docker-context-added-tooapp-telemetry"></a><span data-ttu-id="50464-158">Kontext docker přidat tooapp telemetrie</span><span class="sxs-lookup"><span data-stu-id="50464-158">Docker context added tooapp telemetry</span></span>
<span data-ttu-id="50464-159">Žádost o telemetrické zprávy odesílané z aplikace hello instrumentovány s AI SDK a rozšíření s kontextem Docker:</span><span class="sxs-lookup"><span data-stu-id="50464-159">Request telemetry sent from hello application instrumented with AI SDK, enriched with Docker context:</span></span>

![Příklad](./media/app-insights-docker/16.png)

<span data-ttu-id="50464-161">Čas procesoru a paměti k dispozici čítače výkonu, rozšíření a seskupené podle Docker název kontejneru:</span><span class="sxs-lookup"><span data-stu-id="50464-161">Processor time and available memory performance counters, enriched and grouped by Docker container name:</span></span>

![Příklad](./media/app-insights-docker/15.png)

## <a name="q--a"></a><span data-ttu-id="50464-163">Dotazy a odpovědi</span><span class="sxs-lookup"><span data-stu-id="50464-163">Q & A</span></span>
<span data-ttu-id="50464-164">*Co Application Insights mě, nemůže dostat z Docker?*</span><span class="sxs-lookup"><span data-stu-id="50464-164">*What does Application Insights give me that I can't get from Docker?*</span></span>

* <span data-ttu-id="50464-165">Podrobné rozpis čítačů výkonu tak, že kontejner a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="50464-165">Detailed breakdown of performance counters by container and image.</span></span>
* <span data-ttu-id="50464-166">Integrace aplikace a kontejneru dat v jednom řídicím.</span><span class="sxs-lookup"><span data-stu-id="50464-166">Integrate container and app data in one dashboard.</span></span>
* <span data-ttu-id="50464-167">[Export telemetrie](app-insights-export-telemetry.md) pro další tooa databáze analýzy, Power BI nebo jiné řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="50464-167">[Export telemetry](app-insights-export-telemetry.md) for further analysis tooa database, Power BI or other dashboard.</span></span>

<span data-ttu-id="50464-168">*Jak získat telemetrie z hello aplikace?*</span><span class="sxs-lookup"><span data-stu-id="50464-168">*How do I get telemetry from hello app itself?*</span></span>

* <span data-ttu-id="50464-169">Nainstalujte hello Application Insights SDK do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="50464-169">Install hello Application Insights SDK in hello app.</span></span> <span data-ttu-id="50464-170">Zjistěte, jak pro: [webové aplikace v jazyce Java](app-insights-java-get-started.md), [Windows webové aplikace](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="50464-170">Learn how for: [Java web apps](app-insights-java-get-started.md), [Windows web apps](app-insights-asp-net.md).</span></span>

## <a name="video"></a><span data-ttu-id="50464-171">Video</span><span class="sxs-lookup"><span data-stu-id="50464-171">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="50464-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="50464-172">Next steps</span></span>

* [<span data-ttu-id="50464-173">Application Insights pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="50464-173">Application Insights for Java</span></span>](app-insights-java-get-started.md)
* [<span data-ttu-id="50464-174">Application Insights pro Node.js</span><span class="sxs-lookup"><span data-stu-id="50464-174">Application Insights for Node.js</span></span>](app-insights-nodejs.md)
* [<span data-ttu-id="50464-175">Application Insights pro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="50464-175">Application Insights for ASP.NET</span></span>](app-insights-asp-net.md)
