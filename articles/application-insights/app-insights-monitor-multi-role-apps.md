---
title: "aaaAzure Application Insights podporu pro více komponent, mikroslužeb a kontejnery | Microsoft Docs"
description: "Monitorování aplikací, které se skládají z několika součástí nebo role pro výkonu a využití."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: bwren
ms.openlocfilehash: 6185eedf32ec450d7541603b94de6c3dcdf64a85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a><span data-ttu-id="02963-103">Monitorování více součásti aplikací s nástrojem Application Insights (preview)</span><span class="sxs-lookup"><span data-stu-id="02963-103">Monitor multi-component applications with Application Insights (preview)</span></span>

<span data-ttu-id="02963-104">Můžete monitorovat aplikace, které se skládají z více součásti serveru, role nebo služby s [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="02963-104">You can monitor apps that consist of multiple server components, roles, or services with [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="02963-105">Stav Hello hello součástí a hello vztahů mezi nimi jsou zobrazeny na jednu mapu aplikace.</span><span class="sxs-lookup"><span data-stu-id="02963-105">hello health of hello components and hello relationships between them are displayed on a single Application Map.</span></span> <span data-ttu-id="02963-106">Mohou sledovat jednotlivých operací prostřednictvím více součástí, které se automatické korelace HTTP.</span><span class="sxs-lookup"><span data-stu-id="02963-106">You can trace individual operations through multiple components with automatic HTTP correlation.</span></span> <span data-ttu-id="02963-107">Kontejner diagnostiky můžete integrovat a korelační s telemetrií aplikace.</span><span class="sxs-lookup"><span data-stu-id="02963-107">Container diagnostics can be integrated and correlated with application telemetry.</span></span> <span data-ttu-id="02963-108">Použijte jeden prostředek Application Insights pro všechny součásti hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="02963-108">Use a single Application Insights resource for all hello components of your application.</span></span> 

![Mapa více součásti aplikace](./media/app-insights-monitor-multi-role-apps/app-map.png)

<span data-ttu-id="02963-110">Používáme, součásti, zde toomean libovolná funkční součást rozsáhlé aplikace.</span><span class="sxs-lookup"><span data-stu-id="02963-110">We use 'component' here toomean any functioning part of a large application.</span></span> <span data-ttu-id="02963-111">Například typický obchodní aplikace může obsahovat kód klienta, které jsou spuštěné v prohlížečích webové, rozhovoru tooone nebo další webové aplikace služby, které pak použijte zpět ukončení služby.</span><span class="sxs-lookup"><span data-stu-id="02963-111">For example, a typical business application may consist of client code running in web browsers, talking tooone or more web app services, which in turn use back end services.</span></span> <span data-ttu-id="02963-112">Součásti serveru může být hostovaná místně na v hello cloudu, nebo může být Azure webové a pracovní role nebo může spustit v kontejnerech například Docker nebo Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="02963-112">Server components may be hosted on-premises on in hello cloud, or may be Azure web and worker roles, or may run in containers such as Docker or Service Fabric.</span></span> 

### <a name="sharing-a-single-application-insights-resource"></a><span data-ttu-id="02963-113">Sdílení jednoho prostředku Application Insights</span><span class="sxs-lookup"><span data-stu-id="02963-113">Sharing a single Application Insights resource</span></span> 

<span data-ttu-id="02963-114">Hello klíče technika tady je toosend telemetrie z každé součástí vaší aplikace toohello stejný prostředek Application Insights, ale použití hello `cloud_RoleName` vlastnost toodistinguish součásti, pokud je to nezbytné.</span><span class="sxs-lookup"><span data-stu-id="02963-114">hello key technique here is toosend telemetry from every component in your application toohello same Application Insights resource, but use hello `cloud_RoleName` property toodistinguish components when necessary.</span></span> <span data-ttu-id="02963-115">Hello Application Insights SDK přidá hello `cloud_RoleName` emitování vlastnost toohello telemetrie součásti.</span><span class="sxs-lookup"><span data-stu-id="02963-115">hello Application Insights SDK adds hello `cloud_RoleName` property toohello telemetry components emit.</span></span> <span data-ttu-id="02963-116">Například hello SDK přidá název webového serveru nebo služby role název toohello `cloud_RoleName` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="02963-116">For example, hello SDK will add a web site name, or service role name toohello `cloud_RoleName` property.</span></span> <span data-ttu-id="02963-117">Tato hodnota se telemetryinitializer můžete přepsat.</span><span class="sxs-lookup"><span data-stu-id="02963-117">You can override this value with a telemetryinitializer.</span></span> <span data-ttu-id="02963-118">Hello mapy aplikací používá hello `cloud_RoleName` vlastnost tooidentify hello součásti na mapě hello.</span><span class="sxs-lookup"><span data-stu-id="02963-118">hello Application Map uses hello `cloud_RoleName` property tooidentify hello components on hello map.</span></span>

<span data-ttu-id="02963-119">Další informace o přepsání hello `cloud_RoleName` vlastnost najdete [přidávat vlastnosti: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span><span class="sxs-lookup"><span data-stu-id="02963-119">For more information about how do override hello `cloud_RoleName` property see [Add properties: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span></span>  

<span data-ttu-id="02963-120">V některých případech to nemusí být vhodné a dáte možná přednost toouse samostatné prostředky pro různé skupiny součástí.</span><span class="sxs-lookup"><span data-stu-id="02963-120">In some cases, this may not be appropriate, and you may prefer toouse separate resources for different groups of components.</span></span> <span data-ttu-id="02963-121">Můžete například potřebovat, aby toouse různých prostředků pro správu nebo pro účely fakturace.</span><span class="sxs-lookup"><span data-stu-id="02963-121">For example, you might need toouse different resources for management or billing purposes.</span></span> <span data-ttu-id="02963-122">Pomocí samostatných prostředků znamená, že nevidíte všechny součásti hello zobrazí na jednu mapu aplikace; a že nemůže zadat dotaz mezi komponentami v [Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="02963-122">Using separate resources means that you don't see all hello components displayed on a single Application Map; and that you can't query across components in [Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="02963-123">Máte také tooset systémové prostředky samostatné hello.</span><span class="sxs-lookup"><span data-stu-id="02963-123">You also have tooset up hello separate resources.</span></span>

<span data-ttu-id="02963-124">S výstrahou budeme předpokládat v hello zbytek tohoto dokumentu má toosend data z několika součástí tooone prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="02963-124">With that caveat, we'll assume in hello rest of this document that you want toosend data from multiple components tooone Application Insights resource.</span></span>

## <a name="configure-multi-component-applications"></a><span data-ttu-id="02963-125">Konfigurace více součásti aplikací</span><span class="sxs-lookup"><span data-stu-id="02963-125">Configure multi-component applications</span></span>

<span data-ttu-id="02963-126">mapování tooget více součásti aplikace, musíte tooachieve tyto cíle:</span><span class="sxs-lookup"><span data-stu-id="02963-126">tooget a multi-component application map, you need tooachieve these goals:</span></span>

* <span data-ttu-id="02963-127">**Nainstalujte nejnovější předběžné verze hello** balíček Application Insights v jednotlivých součástí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="02963-127">**Install hello latest pre-release** Application Insights package in each component of hello application.</span></span> 
* <span data-ttu-id="02963-128">**Sdílet jeden prostředek Application Insights** pro všechny součásti aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="02963-128">**Share a single Application Insights resource** for all hello components of your application.</span></span>
* <span data-ttu-id="02963-129">**Povolit aplikaci mapování více rolí** v okně hello verze Preview.</span><span class="sxs-lookup"><span data-stu-id="02963-129">**Enable Multi-role Application Map** in hello Previews blade.</span></span>

<span data-ttu-id="02963-130">Nakonfigurujte jednotlivé komponenty aplikace pomocí odpovídající metody hello u tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="02963-130">Configure each component of your application using hello appropriate method for its type.</span></span> <span data-ttu-id="02963-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span><span class="sxs-lookup"><span data-stu-id="02963-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span></span>

### <a name="1-install-hello-latest-pre-release-package"></a><span data-ttu-id="02963-132">1. Instalovat nejnovější balíček předběžné verze hello</span><span class="sxs-lookup"><span data-stu-id="02963-132">1. Install hello latest pre-release package</span></span>

<span data-ttu-id="02963-133">Aktualizace nebo nainstalujte hello Statistika aplikací balíčky v projektu hello pro každou součást serveru.</span><span class="sxs-lookup"><span data-stu-id="02963-133">Update or install hello Appication Insights packages in hello project for each server component.</span></span> <span data-ttu-id="02963-134">Pokud používáte Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="02963-134">If you're using Visual Studio:</span></span>

1. <span data-ttu-id="02963-135">Klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="02963-135">Right-click a project and select **Manage NuGet Packages**.</span></span> 
2. <span data-ttu-id="02963-136">Vyberte **zahrnout předběžné verze**.</span><span class="sxs-lookup"><span data-stu-id="02963-136">Select **Include prerelease**.</span></span>
3. <span data-ttu-id="02963-137">Pokud je balíčky zobrazí v aktualizace, vybrat Application Insights.</span><span class="sxs-lookup"><span data-stu-id="02963-137">If Application Insights packages appear in Updates, select them.</span></span> 

    <span data-ttu-id="02963-138">Jinak vyhledávat a instalovat příslušný balíček hello:</span><span class="sxs-lookup"><span data-stu-id="02963-138">Otherwise, browse for and install hello appropriate package:</span></span>
    
    * <span data-ttu-id="02963-139">Microsoft.ApplicationInsights.WindowsServer</span><span class="sxs-lookup"><span data-stu-id="02963-139">Microsoft.ApplicationInsights.WindowsServer</span></span>
    * <span data-ttu-id="02963-140">Microsoft.ApplicationInsights.ServiceFabric - pro součásti spuštěný jako Host spustitelných souborů a Docker kontejnery spuštění aplikace v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="02963-140">Microsoft.ApplicationInsights.ServiceFabric - for components running as guest executables and Docker containers running a in Service Fabric application</span></span>
    * <span data-ttu-id="02963-141">Microsoft.ApplicationInsights.ServiceFabric.Native - pro spolehlivé služby v aplikacích ServiceFabric</span><span class="sxs-lookup"><span data-stu-id="02963-141">Microsoft.ApplicationInsights.ServiceFabric.Native - for reliable services in ServiceFabric applications</span></span>
    * <span data-ttu-id="02963-142">Microsoft.ApplicationInsights.Kubernetes pro součásti systémem Kubernetes v Docker</span><span class="sxs-lookup"><span data-stu-id="02963-142">Microsoft.ApplicationInsights.Kubernetes for components running in Docker on Kubernetes</span></span>

### <a name="2-share-a-single-application-insights-resource"></a><span data-ttu-id="02963-143">2. Sdílet jeden prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="02963-143">2. Share a single Application Insights resource</span></span>

* <span data-ttu-id="02963-144">V sadě Visual Studio, klikněte pravým tlačítkem na projekt a vyberte **konfigurovat Application Insights**, nebo **Application Insights > Konfigurovat**.</span><span class="sxs-lookup"><span data-stu-id="02963-144">In Visual Studio, right-click a project and select **Configure Application Insights**, or **Application Insights > Configure**.</span></span> <span data-ttu-id="02963-145">Pro první projekt hello použijte Průvodce toocreate hello prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="02963-145">For hello first project, use hello wizard toocreate an Application Insights resource.</span></span> <span data-ttu-id="02963-146">Pro další projekty vyberte hello stejného zdroje.</span><span class="sxs-lookup"><span data-stu-id="02963-146">For subsequent projects, select hello same resource.</span></span>
* <span data-ttu-id="02963-147">Pokud není žádná nabídka Application Insights, nakonfigurujte ručně:</span><span class="sxs-lookup"><span data-stu-id="02963-147">If there is no Application Insights menu, configure manually:</span></span>

   1. <span data-ttu-id="02963-148">V [portál Azure](https://portal,azure.com), otevřete prostředek Application Insights hello jste již vytvořili pro jiné komponenty.</span><span class="sxs-lookup"><span data-stu-id="02963-148">In [Azure portal](https://portal,azure.com), open hello Application Insights resource you already created for another component.</span></span>
   2. <span data-ttu-id="02963-149">Okno Přehled hello, karta otevřete hello Essentials rozevíracího seznamu a kopírování hello **klíč instrumentace.**</span><span class="sxs-lookup"><span data-stu-id="02963-149">In hello Overview blade, open hello Essentials drop-down tab, and copy hello **Instrumentation Key.**</span></span>
   3. <span data-ttu-id="02963-150">V projektu otevřete soubor ApplicationInsights.config a vložit:`<InstrumentationKey>your copied key</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="02963-150">In your project, open ApplicationInsights.config and insert: `<InstrumentationKey>your copied key</InstrumentationKey>`</span></span>

![Zkopírujte soubor .config klíče toohello instrumentace hello](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-multi-role-application-map"></a><span data-ttu-id="02963-152">3. Povolit aplikaci mapování více rolí</span><span class="sxs-lookup"><span data-stu-id="02963-152">3. Enable multi-role Application Map</span></span>

<span data-ttu-id="02963-153">V hello portálu Azure otevřete hello prostředků pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="02963-153">In hello Azure portal, open hello resource for your application.</span></span> <span data-ttu-id="02963-154">V okně hello verze Preview povolit *mapování aplikace s více rolí*.</span><span class="sxs-lookup"><span data-stu-id="02963-154">In hello Previews blade, enable *Multi-role Application Map*.</span></span>

### <a name="4-enable-docker-metrics-optional"></a><span data-ttu-id="02963-155">4. Povolit Docker metriky (volitelné)</span><span class="sxs-lookup"><span data-stu-id="02963-155">4. Enable Docker metrics (Optional)</span></span> 

<span data-ttu-id="02963-156">Pokud součást běží v Docker, hostované na virtuálním počítači Windows Azure, můžete shromažďovat další metriky z kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="02963-156">If a component runs in a Docker hosted on an Azure Windows VM, you can collect additional metrics from hello container.</span></span> <span data-ttu-id="02963-157">Vložte tuto ve vaší [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="02963-157">Insert this in your [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) configuration file:</span></span>

```
"DiagnosticMonitorConfiguration": {
        ...
        "sinks": "applicationInsights",
        "DockerSources": {
                "Stats": {
                    "enabled": true,
                    "sampleRate": "PT1M"
                }
            },
        ...
    }
    ...   
    "SinksConfig": {
        "Sink": [{
            "name": "applicationInsights",
            "ApplicationInsights": "<your instrumentation key here>"
        }]
    }
    ...
}

```

## <a name="use-cloudrolename-tooseparate-components"></a><span data-ttu-id="02963-158">Použití cloud_RoleName tooseparate komponent</span><span class="sxs-lookup"><span data-stu-id="02963-158">Use cloud_RoleName tooseparate components</span></span>

<span data-ttu-id="02963-159">Hello `cloud_RoleName` vlastnost je připojený tooall telemetrie.</span><span class="sxs-lookup"><span data-stu-id="02963-159">hello `cloud_RoleName` property is attached tooall telemetry.</span></span> <span data-ttu-id="02963-160">Identifikuje součást hello - hello roli nebo službě -, která pochází hello telemetrie.</span><span class="sxs-lookup"><span data-stu-id="02963-160">It identifies hello component - hello role or service - that originates hello telemetry.</span></span> <span data-ttu-id="02963-161">(Není stejná hello jako cloud_RoleInstance, který odděluje identické rolí, které běží souběžně na více procesy serveru nebo počítače.)</span><span class="sxs-lookup"><span data-stu-id="02963-161">(It is not hello same as cloud_RoleInstance, which separates identical roles that are running in parallel on multiple server processes or machines.)</span></span>

<span data-ttu-id="02963-162">Hello portálu můžete filtrovat nebo segmentovat telemetrie používání této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="02963-162">In hello portal, you can filter or segment your telemetry using this property.</span></span> <span data-ttu-id="02963-163">V tomto příkladu je okno selhání hello filtrované tooshow jenom informace z hello front-endu webové služby, filtrování chyb z back-end CRM API hello:</span><span class="sxs-lookup"><span data-stu-id="02963-163">In this example, hello Failures blade is filtered tooshow just information from hello front-end web service, filtering out failures from hello CRM API backend:</span></span>

![Metriky grafu segmentované podle názvu Role cloudu](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a><span data-ttu-id="02963-165">Operace trasování mezi součástmi</span><span class="sxs-lookup"><span data-stu-id="02963-165">Trace operations between components</span></span>

<span data-ttu-id="02963-166">Můžete sledovat z jedné součásti tooanother, hello volání provedená při zpracování jednotlivých operací.</span><span class="sxs-lookup"><span data-stu-id="02963-166">You can trace from one component tooanother, hello calls made while processing an individual operation.</span></span>


![Zobrazit telemetrii pro operaci](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

<span data-ttu-id="02963-168">Klikněte na tlačítko prostřednictvím tooa korelační seznam telemetrická data pro tuto operaci napříč hello klientské části webového serveru a rozhraní API back-end hello:</span><span class="sxs-lookup"><span data-stu-id="02963-168">Click through tooa correlated list of telemetry for this operation across hello front-end web server and hello back-end API:</span></span>

![Hledání mezi komponentami](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a><span data-ttu-id="02963-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="02963-170">Next steps</span></span>

* [<span data-ttu-id="02963-171">Samostatné telemetrie z vývoj, testování a provozním</span><span class="sxs-lookup"><span data-stu-id="02963-171">Separate telemetry from Development, Test, and Production</span></span>](app-insights-separate-resources.md)
