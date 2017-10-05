---
title: "Podpora Azure Application Insights pro několik komponent, mikroslužeb a kontejnery | Microsoft Docs"
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
ms.openlocfilehash: ca1bb8ee886c4b4e69be9dd653d6a52b874e1f5a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a><span data-ttu-id="40dd3-103">Monitorování více součásti aplikací s nástrojem Application Insights (preview)</span><span class="sxs-lookup"><span data-stu-id="40dd3-103">Monitor multi-component applications with Application Insights (preview)</span></span>

<span data-ttu-id="40dd3-104">Můžete monitorovat aplikace, které se skládají z více součásti serveru, role nebo služby s [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="40dd3-104">You can monitor apps that consist of multiple server components, roles, or services with [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="40dd3-105">Zobrazí se stav součásti a vztahy mezi nimi na jednu mapu aplikace.</span><span class="sxs-lookup"><span data-stu-id="40dd3-105">The health of the components and the relationships between them are displayed on a single Application Map.</span></span> <span data-ttu-id="40dd3-106">Mohou sledovat jednotlivých operací prostřednictvím více součástí, které se automatické korelace HTTP.</span><span class="sxs-lookup"><span data-stu-id="40dd3-106">You can trace individual operations through multiple components with automatic HTTP correlation.</span></span> <span data-ttu-id="40dd3-107">Kontejner diagnostiky můžete integrovat a korelační s telemetrií aplikace.</span><span class="sxs-lookup"><span data-stu-id="40dd3-107">Container diagnostics can be integrated and correlated with application telemetry.</span></span> <span data-ttu-id="40dd3-108">Použijte jeden prostředek Application Insights pro všechny součásti aplikace.</span><span class="sxs-lookup"><span data-stu-id="40dd3-108">Use a single Application Insights resource for all the components of your application.</span></span> 

![Mapa více součásti aplikace](./media/app-insights-monitor-multi-role-apps/app-map.png)

<span data-ttu-id="40dd3-110">Rozumí jakékoli funkční součástí velké aplikace tady používáme "součást".</span><span class="sxs-lookup"><span data-stu-id="40dd3-110">We use 'component' here to mean any functioning part of a large application.</span></span> <span data-ttu-id="40dd3-111">Například typické obchodní aplikace se může skládat z klienta kód spuštěný ve webové prohlížeče, rozhovoru na jeden nebo více webové aplikace služby, které pak použijte zpět ukončení služby.</span><span class="sxs-lookup"><span data-stu-id="40dd3-111">For example, a typical business application may consist of client code running in web browsers, talking to one or more web app services, which in turn use back end services.</span></span> <span data-ttu-id="40dd3-112">Součásti serveru může být hostovaná místně na v cloudu, nebo může být Azure webové a pracovní role nebo může spustit v kontejnerech například Docker nebo Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="40dd3-112">Server components may be hosted on-premises on in the cloud, or may be Azure web and worker roles, or may run in containers such as Docker or Service Fabric.</span></span> 

### <a name="sharing-a-single-application-insights-resource"></a><span data-ttu-id="40dd3-113">Sdílení jednoho prostředku Application Insights</span><span class="sxs-lookup"><span data-stu-id="40dd3-113">Sharing a single Application Insights resource</span></span> 

<span data-ttu-id="40dd3-114">Zde klíče technika je odesílat telemetrická data z každé komponenty v aplikaci do stejného zdroje Application Insights, ale použít `cloud_RoleName` vlastnost k rozlišení součásti, pokud je to nezbytné.</span><span class="sxs-lookup"><span data-stu-id="40dd3-114">The key technique here is to send telemetry from every component in your application to the same Application Insights resource, but use the `cloud_RoleName` property to distinguish components when necessary.</span></span> <span data-ttu-id="40dd3-115">Přidá Application Insights SDK `cloud_RoleName` emitování vlastnost součástí telemetrie.</span><span class="sxs-lookup"><span data-stu-id="40dd3-115">The Application Insights SDK adds the `cloud_RoleName` property to the telemetry components emit.</span></span> <span data-ttu-id="40dd3-116">Například sada SDK přidá název webového serveru nebo název role Služba pro `cloud_RoleName` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="40dd3-116">For example, the SDK will add a web site name, or service role name to the `cloud_RoleName` property.</span></span> <span data-ttu-id="40dd3-117">Tato hodnota se telemetryinitializer můžete přepsat.</span><span class="sxs-lookup"><span data-stu-id="40dd3-117">You can override this value with a telemetryinitializer.</span></span> <span data-ttu-id="40dd3-118">Mapování aplikací používá `cloud_RoleName` vlastnost k identifikaci součásti na mapě.</span><span class="sxs-lookup"><span data-stu-id="40dd3-118">The Application Map uses the `cloud_RoleName` property to identify the components on the map.</span></span>

<span data-ttu-id="40dd3-119">Další informace o přepsání `cloud_RoleName` vlastnost najdete [přidávat vlastnosti: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span><span class="sxs-lookup"><span data-stu-id="40dd3-119">For more information about how do override the `cloud_RoleName` property see [Add properties: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span></span>  

<span data-ttu-id="40dd3-120">V některých případech to nemusí být vhodné a chcete používat samostatný prostředky pro různé skupiny součástí.</span><span class="sxs-lookup"><span data-stu-id="40dd3-120">In some cases, this may not be appropriate, and you may prefer to use separate resources for different groups of components.</span></span> <span data-ttu-id="40dd3-121">Například budete muset použít jiné prostředky pro správu nebo pro účely fakturace.</span><span class="sxs-lookup"><span data-stu-id="40dd3-121">For example, you might need to use different resources for management or billing purposes.</span></span> <span data-ttu-id="40dd3-122">Pomocí samostatných prostředků znamená, že nevidíte všechny součásti, na které se zobrazí na jednu mapu aplikace; a že nemůže zadat dotaz mezi komponentami v [Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="40dd3-122">Using separate resources means that you don't see all the components displayed on a single Application Map; and that you can't query across components in [Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="40dd3-123">Také budete muset nastavit samostatnou prostředky.</span><span class="sxs-lookup"><span data-stu-id="40dd3-123">You also have to set up the separate resources.</span></span>

<span data-ttu-id="40dd3-124">S výstrahou budeme předpokládat ve zbývající části tohoto dokumentu, který chcete odesílat data z několika součástí jeden prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="40dd3-124">With that caveat, we'll assume in the rest of this document that you want to send data from multiple components to one Application Insights resource.</span></span>

## <a name="configure-multi-component-applications"></a><span data-ttu-id="40dd3-125">Konfigurace více součásti aplikací</span><span class="sxs-lookup"><span data-stu-id="40dd3-125">Configure multi-component applications</span></span>

<span data-ttu-id="40dd3-126">Chcete-li získat více součásti aplikace mapy, těchto cílů dosáhnout:</span><span class="sxs-lookup"><span data-stu-id="40dd3-126">To get a multi-component application map, you need to achieve these goals:</span></span>

* <span data-ttu-id="40dd3-127">**Nainstalujte nejnovější předběžnou verzi** balíček Application Insights v jednotlivých součástí aplikace.</span><span class="sxs-lookup"><span data-stu-id="40dd3-127">**Install the latest pre-release** Application Insights package in each component of the application.</span></span> 
* <span data-ttu-id="40dd3-128">**Sdílet jeden prostředek Application Insights** pro všechny součásti aplikace.</span><span class="sxs-lookup"><span data-stu-id="40dd3-128">**Share a single Application Insights resource** for all the components of your application.</span></span>
* <span data-ttu-id="40dd3-129">**Povolit aplikaci mapování více rolí** v okně verze Preview.</span><span class="sxs-lookup"><span data-stu-id="40dd3-129">**Enable Multi-role Application Map** in the Previews blade.</span></span>

<span data-ttu-id="40dd3-130">Nakonfigurujte jednotlivé komponenty pomocí příslušné metody pro tento typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="40dd3-130">Configure each component of your application using the appropriate method for its type.</span></span> <span data-ttu-id="40dd3-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span><span class="sxs-lookup"><span data-stu-id="40dd3-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span></span>

### <a name="1-install-the-latest-pre-release-package"></a><span data-ttu-id="40dd3-132">1. Instalovat nejnovější balíček předběžné verze</span><span class="sxs-lookup"><span data-stu-id="40dd3-132">1. Install the latest pre-release package</span></span>

<span data-ttu-id="40dd3-133">Aktualizujte nebo instalaci balíčků aplikací statistiky v projektu pro každou součást serveru.</span><span class="sxs-lookup"><span data-stu-id="40dd3-133">Update or install the Appication Insights packages in the project for each server component.</span></span> <span data-ttu-id="40dd3-134">Pokud používáte Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="40dd3-134">If you're using Visual Studio:</span></span>

1. <span data-ttu-id="40dd3-135">Klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="40dd3-135">Right-click a project and select **Manage NuGet Packages**.</span></span> 
2. <span data-ttu-id="40dd3-136">Vyberte **zahrnout předběžné verze**.</span><span class="sxs-lookup"><span data-stu-id="40dd3-136">Select **Include prerelease**.</span></span>
3. <span data-ttu-id="40dd3-137">Pokud je balíčky zobrazí v aktualizace, vybrat Application Insights.</span><span class="sxs-lookup"><span data-stu-id="40dd3-137">If Application Insights packages appear in Updates, select them.</span></span> 

    <span data-ttu-id="40dd3-138">Jinak vyhledávat a instalovat příslušný balíček:</span><span class="sxs-lookup"><span data-stu-id="40dd3-138">Otherwise, browse for and install the appropriate package:</span></span>
    
    * <span data-ttu-id="40dd3-139">Microsoft.ApplicationInsights.WindowsServer</span><span class="sxs-lookup"><span data-stu-id="40dd3-139">Microsoft.ApplicationInsights.WindowsServer</span></span>
    * <span data-ttu-id="40dd3-140">Microsoft.ApplicationInsights.ServiceFabric - pro součásti spuštěný jako Host spustitelných souborů a Docker kontejnery spuštění aplikace v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="40dd3-140">Microsoft.ApplicationInsights.ServiceFabric - for components running as guest executables and Docker containers running a in Service Fabric application</span></span>
    * <span data-ttu-id="40dd3-141">Microsoft.ApplicationInsights.ServiceFabric.Native - pro spolehlivé služby v aplikacích ServiceFabric</span><span class="sxs-lookup"><span data-stu-id="40dd3-141">Microsoft.ApplicationInsights.ServiceFabric.Native - for reliable services in ServiceFabric applications</span></span>
    * <span data-ttu-id="40dd3-142">Microsoft.ApplicationInsights.Kubernetes pro součásti systémem Kubernetes v Docker</span><span class="sxs-lookup"><span data-stu-id="40dd3-142">Microsoft.ApplicationInsights.Kubernetes for components running in Docker on Kubernetes</span></span>

### <a name="2-share-a-single-application-insights-resource"></a><span data-ttu-id="40dd3-143">2. Sdílet jeden prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="40dd3-143">2. Share a single Application Insights resource</span></span>

* <span data-ttu-id="40dd3-144">V sadě Visual Studio, klikněte pravým tlačítkem na projekt a vyberte **konfigurovat Application Insights**, nebo **Application Insights > Konfigurovat**.</span><span class="sxs-lookup"><span data-stu-id="40dd3-144">In Visual Studio, right-click a project and select **Configure Application Insights**, or **Application Insights > Configure**.</span></span> <span data-ttu-id="40dd3-145">Pro první projekt pomocí průvodce vytvořte prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="40dd3-145">For the first project, use the wizard to create an Application Insights resource.</span></span> <span data-ttu-id="40dd3-146">Pro další projekty vyberte stejný prostředek.</span><span class="sxs-lookup"><span data-stu-id="40dd3-146">For subsequent projects, select the same resource.</span></span>
* <span data-ttu-id="40dd3-147">Pokud není žádná nabídka Application Insights, nakonfigurujte ručně:</span><span class="sxs-lookup"><span data-stu-id="40dd3-147">If there is no Application Insights menu, configure manually:</span></span>

   1. <span data-ttu-id="40dd3-148">V [portál Azure](https://portal,azure.com), otevřete prostředek Application Insights jste již vytvořili pro jiné komponenty.</span><span class="sxs-lookup"><span data-stu-id="40dd3-148">In [Azure portal](https://portal,azure.com), open the Application Insights resource you already created for another component.</span></span>
   2. <span data-ttu-id="40dd3-149">V okně přehledu, otevřete rozevírací Essentials kartě a zkopírujte **klíč instrumentace.**</span><span class="sxs-lookup"><span data-stu-id="40dd3-149">In the Overview blade, open the Essentials drop-down tab, and copy the **Instrumentation Key.**</span></span>
   3. <span data-ttu-id="40dd3-150">V projektu otevřete soubor ApplicationInsights.config a vložit:`<InstrumentationKey>your copied key</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="40dd3-150">In your project, open ApplicationInsights.config and insert: `<InstrumentationKey>your copied key</InstrumentationKey>`</span></span>

![Zkopírovat klíč instrumentace do souboru .config](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-multi-role-application-map"></a><span data-ttu-id="40dd3-152">3. Povolit aplikaci mapování více rolí</span><span class="sxs-lookup"><span data-stu-id="40dd3-152">3. Enable multi-role Application Map</span></span>

<span data-ttu-id="40dd3-153">Na portálu Azure otevřete prostředek vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="40dd3-153">In the Azure portal, open the resource for your application.</span></span> <span data-ttu-id="40dd3-154">V okně verze Preview povolit *mapování aplikace s více rolí*.</span><span class="sxs-lookup"><span data-stu-id="40dd3-154">In the Previews blade, enable *Multi-role Application Map*.</span></span>

### <a name="4-enable-docker-metrics-optional"></a><span data-ttu-id="40dd3-155">4. Povolit Docker metriky (volitelné)</span><span class="sxs-lookup"><span data-stu-id="40dd3-155">4. Enable Docker metrics (Optional)</span></span> 

<span data-ttu-id="40dd3-156">Pokud součást běží v Docker, hostované na virtuálním počítači Windows Azure, můžete shromažďovat další metriky z kontejneru.</span><span class="sxs-lookup"><span data-stu-id="40dd3-156">If a component runs in a Docker hosted on an Azure Windows VM, you can collect additional metrics from the container.</span></span> <span data-ttu-id="40dd3-157">Vložte tuto ve vaší [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="40dd3-157">Insert this in your [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) configuration file:</span></span>

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

## <a name="use-cloudrolename-to-separate-components"></a><span data-ttu-id="40dd3-158">Použít cloud_RoleName jednotlivé komponenty</span><span class="sxs-lookup"><span data-stu-id="40dd3-158">Use cloud_RoleName to separate components</span></span>

<span data-ttu-id="40dd3-159">`cloud_RoleName` Vlastnost je připojen k všechny telemetrie.</span><span class="sxs-lookup"><span data-stu-id="40dd3-159">The `cloud_RoleName` property is attached to all telemetry.</span></span> <span data-ttu-id="40dd3-160">Identifikuje komponentu - k roli nebo službě -, který pochází telemetrii.</span><span class="sxs-lookup"><span data-stu-id="40dd3-160">It identifies the component - the role or service - that originates the telemetry.</span></span> <span data-ttu-id="40dd3-161">(Není stejný jako cloud_RoleInstance, který odděluje identické rolí, které běží souběžně na více procesy serveru nebo počítače.)</span><span class="sxs-lookup"><span data-stu-id="40dd3-161">(It is not the same as cloud_RoleInstance, which separates identical roles that are running in parallel on multiple server processes or machines.)</span></span>

<span data-ttu-id="40dd3-162">Na portálu můžete filtrovat nebo segmentovat telemetrie používání této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="40dd3-162">In the portal, you can filter or segment your telemetry using this property.</span></span> <span data-ttu-id="40dd3-163">V tomto příkladu je v okně selhání vyfiltrovány a zobrazí se jenom informace ze služby front-endové webové filtrování chyb z back-end CRM rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="40dd3-163">In this example, the Failures blade is filtered to show just information from the front-end web service, filtering out failures from the CRM API backend:</span></span>

![Metriky grafu segmentované podle názvu Role cloudu](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a><span data-ttu-id="40dd3-165">Operace trasování mezi součástmi</span><span class="sxs-lookup"><span data-stu-id="40dd3-165">Trace operations between components</span></span>

<span data-ttu-id="40dd3-166">Můžete sledovat z jedné součásti na jiný, volání provedená při zpracování jednotlivých operací.</span><span class="sxs-lookup"><span data-stu-id="40dd3-166">You can trace from one component to another, the calls made while processing an individual operation.</span></span>


![Zobrazit telemetrii pro operaci](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

<span data-ttu-id="40dd3-168">Klikněte na tlačítko prostřednictvím korelační seznam telemetrická data pro tuto operaci v klientské části webového serveru a rozhraní API back-end:</span><span class="sxs-lookup"><span data-stu-id="40dd3-168">Click through to a correlated list of telemetry for this operation across the front-end web server and the back-end API:</span></span>

![Hledání mezi komponentami](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a><span data-ttu-id="40dd3-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="40dd3-170">Next steps</span></span>

* [<span data-ttu-id="40dd3-171">Samostatné telemetrie z vývoj, testování a provozním</span><span class="sxs-lookup"><span data-stu-id="40dd3-171">Separate telemetry from Development, Test, and Production</span></span>](app-insights-separate-resources.md)
