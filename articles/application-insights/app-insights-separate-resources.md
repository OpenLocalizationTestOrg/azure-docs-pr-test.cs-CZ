---
title: "Oddělení telemetrie z vývoj, testování a verzí v Azure Application Insights | Microsoft Docs"
description: "Přímé telemetrie na jiné prostředky pro vývoj, testování a provozním razítka."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: f51fa4639aaa60686cc349683713c6e5f9732bb9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="separating-telemetry-from-development-test-and-production"></a><span data-ttu-id="77fe7-103">Oddělení telemetrie z vývoj, testování a provozním</span><span class="sxs-lookup"><span data-stu-id="77fe7-103">Separating telemetry from Development, Test, and Production</span></span>

<span data-ttu-id="77fe7-104">Při vývoji příští verze webové aplikace, nechcete kombinovat [Application Insights](app-insights-overview.md) telemetrie z nové verze a již vydaná verze.</span><span class="sxs-lookup"><span data-stu-id="77fe7-104">When you are developing the next version of a web application, you don't want to mix up the [Application Insights](app-insights-overview.md) telemetry from the new version and the already released version.</span></span> <span data-ttu-id="77fe7-105">Pokud chcete předejít nejasnostem, odešlete telemetrii z různých vývoj fázích jednotlivé prostředky Application Insights, s klíči samostatné instrumentace (ikeys).</span><span class="sxs-lookup"><span data-stu-id="77fe7-105">To avoid confusion, send the telemetry from different development stages to separate Application Insights resources, with separate instrumentation keys (ikeys).</span></span> <span data-ttu-id="77fe7-106">Aby bylo snazší změnit klíč instrumentace, protože verze přesouvá z jedné fáze do jiného, může být užitečné k nastavení ikey v kódu místo v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="77fe7-106">To make it easier to change the instrumentation key as a version moves from one stage to another, it can be useful to set the ikey in code instead of in the configuration file.</span></span> 

<span data-ttu-id="77fe7-107">(Pokud je váš systém cloudové služby Azure je [jinou metodu nastavení samostatné ikeys](app-insights-cloudservices.md).)</span><span class="sxs-lookup"><span data-stu-id="77fe7-107">(If your system is an Azure Cloud Service, there's [another method of setting separate ikeys](app-insights-cloudservices.md).)</span></span>

## <a name="about-resources-and-instrumentation-keys"></a><span data-ttu-id="77fe7-108">O prostředcích a klíčů instrumentace</span><span class="sxs-lookup"><span data-stu-id="77fe7-108">About resources and instrumentation keys</span></span>

<span data-ttu-id="77fe7-109">Když jste nastavili Application Insights monitorování pro webovou aplikaci, můžete vytvořit Application Insights *prostředků* v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="77fe7-109">When you set up Application Insights monitoring for your web app, you create an Application Insights *resource* in Microsoft Azure.</span></span> <span data-ttu-id="77fe7-110">Tento prostředek otevřít na portálu Azure, aby bylo možné zobrazit a analyzovat telemetrii získanou z vašich aplikací.</span><span class="sxs-lookup"><span data-stu-id="77fe7-110">You open this resource in the Azure portal in order to see and analyze the telemetry collected from your app.</span></span> <span data-ttu-id="77fe7-111">Prostředek je identifikována *klíč instrumentace* (ikey).</span><span class="sxs-lookup"><span data-stu-id="77fe7-111">The resource is identified by an *instrumentation key* (ikey).</span></span> <span data-ttu-id="77fe7-112">Když nainstalujete balíček Application Insights pro sledování aplikace, musíte ho nakonfigurovat s klíč instrumentace, tak, aby věděl, že může kam má posílat telemetrii.</span><span class="sxs-lookup"><span data-stu-id="77fe7-112">When you install the Application Insights package to monitor your app, you configure it with the instrumentation key, so that it knows where to send the telemetry.</span></span>

<span data-ttu-id="77fe7-113">Obvykle je rozhodnete použít samostatné prostředky nebo jeden sdílený prostředek v různých scénářích:</span><span class="sxs-lookup"><span data-stu-id="77fe7-113">You typically choose to use separate resources or a single shared resource in different scenarios:</span></span>

* <span data-ttu-id="77fe7-114">Jiné, nezávislé aplikace – použít samostatné prostředků a ikey pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="77fe7-114">Different, independent applications - Use a separate resource and ikey for each app.</span></span>
* <span data-ttu-id="77fe7-115">Více součástí nebo role jeden obchodní aplikace – použijte [jeden sdílený prostředek](app-insights-monitor-multi-role-apps.md) pro všechny součásti aplikace.</span><span class="sxs-lookup"><span data-stu-id="77fe7-115">Multiple components or roles of one business application - Use a [single shared resource](app-insights-monitor-multi-role-apps.md) for all the component apps.</span></span> <span data-ttu-id="77fe7-116">Telemetrická data lze filtrovat a segmentované cloud_RoleName vlastností.</span><span class="sxs-lookup"><span data-stu-id="77fe7-116">Telemetry can be filtered or segmented by the cloud_RoleName property.</span></span>
* <span data-ttu-id="77fe7-117">Vývoj, testování a verze – použijte samostatné prostředků a ikey pro verze systému, v 'razítka, nebo fáze produkce.</span><span class="sxs-lookup"><span data-stu-id="77fe7-117">Development, Test, and Release - Use a separate resource and ikey for versions of the system in 'stamp' or stage of production.</span></span>
* <span data-ttu-id="77fe7-118">A | Testování B – použít jeden zdroj.</span><span class="sxs-lookup"><span data-stu-id="77fe7-118">A | B testing - Use a single resource.</span></span> <span data-ttu-id="77fe7-119">Vytvořte TelemetryInitializer k přidání vlastnosti do telemetrii, kterou identifikuje varianty.</span><span class="sxs-lookup"><span data-stu-id="77fe7-119">Create a TelemetryInitializer to add a property to the telemetry that identifies the variants.</span></span>


## <span data-ttu-id="77fe7-120"><a name="dynamic-ikey"></a>Klíč dynamické instrumentace</span><span class="sxs-lookup"><span data-stu-id="77fe7-120"><a name="dynamic-ikey"></a> Dynamic instrumentation key</span></span>

<span data-ttu-id="77fe7-121">Aby bylo snazší ke změnám ikey podle kód přesune mezi fáze produkce, nastavte v kódu místo v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="77fe7-121">To make it easier to change the ikey as the code moves between stages of production, set it in code instead of in the configuration file.</span></span>

<span data-ttu-id="77fe7-122">V metodě inicializace, jako jsou například souboru global.aspx.cs v službě ASP.NET nastavte:</span><span class="sxs-lookup"><span data-stu-id="77fe7-122">Set the key in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

<span data-ttu-id="77fe7-123">*C#*</span><span class="sxs-lookup"><span data-stu-id="77fe7-123">*C#*</span></span>

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

<span data-ttu-id="77fe7-124">V tomto příkladu ikeys pro různé prostředky jsou umístěny v různých verzích soubor webové konfigurace.</span><span class="sxs-lookup"><span data-stu-id="77fe7-124">In this example, the ikeys for the different resources are placed in different versions of the web configuration file.</span></span> <span data-ttu-id="77fe7-125">Vzájemná záměna konfigurační soubor webu – což lze provést v rámci skriptu verze - bude Prohodit cílový prostředek.</span><span class="sxs-lookup"><span data-stu-id="77fe7-125">Swapping the web configuration file - which you can do as part of the release script - will swap the target resource.</span></span>

### <a name="web-pages"></a><span data-ttu-id="77fe7-126">Webové stránky</span><span class="sxs-lookup"><span data-stu-id="77fe7-126">Web pages</span></span>
<span data-ttu-id="77fe7-127">IKey se také používá ve webových stránkách vaší aplikace, v [skript, který jste získali z okna rychlý start](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="77fe7-127">The iKey is also used in your app's web pages, in the [script that you got from the quick start blade](app-insights-javascript.md).</span></span> <span data-ttu-id="77fe7-128">Místo kódování je oznámena do skriptu, vygenerujte ho z stavu serveru.</span><span class="sxs-lookup"><span data-stu-id="77fe7-128">Instead of coding it literally into the script, generate it from the server state.</span></span> <span data-ttu-id="77fe7-129">Například v aplikaci ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="77fe7-129">For example, in an ASP.NET app:</span></span>

<span data-ttu-id="77fe7-130">*JavaScript ve Razor*</span><span class="sxs-lookup"><span data-stu-id="77fe7-130">*JavaScript in Razor*</span></span>

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a><span data-ttu-id="77fe7-131">Vytvořit další prostředky Application Insights</span><span class="sxs-lookup"><span data-stu-id="77fe7-131">Create additional Application Insights resources</span></span>
<span data-ttu-id="77fe7-132">K oddělení telemetrie pro součásti jiné aplikace, nebo pro jiný razítka (dev/testovací/produkční) stejné komponenty, pak budete muset vytvořit nový prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="77fe7-132">To separate telemetry for different application components, or for different stamps (dev/test/production) of the same component, then you'll have to create a new Application Insights resource.</span></span>

<span data-ttu-id="77fe7-133">V [portal.azure.com](https://portal.azure.com), přidejte prostředek Application Insights:</span><span class="sxs-lookup"><span data-stu-id="77fe7-133">In the [portal.azure.com](https://portal.azure.com), add an Application Insights resource:</span></span>

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-separate-resources/01-new.png)

* <span data-ttu-id="77fe7-135">**Typ aplikace** ovlivňuje, co se zobrazí v okně Přehled a vlastnosti, které jsou k dispozici v [metriky explorer](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="77fe7-135">**Application type** affects what you see on the overview blade and the properties available in [metric explorer](app-insights-metrics-explorer.md).</span></span> <span data-ttu-id="77fe7-136">Pokud nevidíte vašeho typu aplikace, vyberte jeden z typů webových pro webové stránky.</span><span class="sxs-lookup"><span data-stu-id="77fe7-136">If you don't see your type of app, choose one of the web types for web pages.</span></span>
* <span data-ttu-id="77fe7-137">**Skupina prostředků** je užitečný pro správu vlastnosti, například [řízení přístupu](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="77fe7-137">**Resource group** is a convenience for managing properties like [access control](app-insights-resources-roles-access-control.md).</span></span> <span data-ttu-id="77fe7-138">Pro vývoj, testování a provozním můžete použít samostatné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="77fe7-138">You could use separate resource groups for development, test, and production.</span></span>
* <span data-ttu-id="77fe7-139">**Předplatné** je váš účet platebních v Azure.</span><span class="sxs-lookup"><span data-stu-id="77fe7-139">**Subscription** is your payment account in Azure.</span></span>
* <span data-ttu-id="77fe7-140">**Umístění** je, kde společnost Microsoft uchovávat data.</span><span class="sxs-lookup"><span data-stu-id="77fe7-140">**Location** is where we keep your data.</span></span> <span data-ttu-id="77fe7-141">Aktuálně jej nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="77fe7-141">Currently it can't be changed.</span></span> 
* <span data-ttu-id="77fe7-142">**Přidat na řídicí panel** vloží dlaždici rychlý přístup pro prostředek na Azure domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="77fe7-142">**Add to dashboard** puts a quick-access tile for your resource on your Azure Home page.</span></span> 

<span data-ttu-id="77fe7-143">Vytvoření prostředku trvá několik sekund.</span><span class="sxs-lookup"><span data-stu-id="77fe7-143">Creating the resource takes a few seconds.</span></span> <span data-ttu-id="77fe7-144">Pokud se provádí, zobrazí se výstraha.</span><span class="sxs-lookup"><span data-stu-id="77fe7-144">You'll see an alert when it's done.</span></span>

<span data-ttu-id="77fe7-145">(Můžete napsat [skript prostředí PowerShell](app-insights-powershell-script-create-resource.md) vytvoření prostředku automaticky.)</span><span class="sxs-lookup"><span data-stu-id="77fe7-145">(You can write a [PowerShell script](app-insights-powershell-script-create-resource.md) to create a resource automatically.)</span></span>

### <a name="getting-the-instrumentation-key"></a><span data-ttu-id="77fe7-146">Získávání klíč instrumentace</span><span class="sxs-lookup"><span data-stu-id="77fe7-146">Getting the instrumentation key</span></span>
<span data-ttu-id="77fe7-147">Klíč instrumentace identifikuje prostředek, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="77fe7-147">The instrumentation key identifies the resource that you created.</span></span> 

![Klikněte na tlačítko Essentials, klikněte na klíč instrumentace, CTRL + C](./media/app-insights-separate-resources/02-props.png)

<span data-ttu-id="77fe7-149">Je nutné klíčů instrumentace všech prostředků, do které bude vaše aplikace posílat data.</span><span class="sxs-lookup"><span data-stu-id="77fe7-149">You need the instrumentation keys of all the resources to which your app will send data.</span></span>

## <a name="filter-on-build-number"></a><span data-ttu-id="77fe7-150">Filtrovat podle čísla sestavení</span><span class="sxs-lookup"><span data-stu-id="77fe7-150">Filter on build number</span></span>
<span data-ttu-id="77fe7-151">Při publikování nové verze aplikace, budete chtít mít možnost oddělit telemetrii z různých sestavení.</span><span class="sxs-lookup"><span data-stu-id="77fe7-151">When you publish a new version of your app, you'll want to be able to separate the telemetry from different builds.</span></span>

<span data-ttu-id="77fe7-152">Verze aplikace vlastnost lze nastavit tak, aby můžete filtrovat [vyhledávání](app-insights-diagnostic-search.md) a [metriky explorer](app-insights-metrics-explorer.md) výsledky.</span><span class="sxs-lookup"><span data-stu-id="77fe7-152">You can set the Application Version property so that you can filter [search](app-insights-diagnostic-search.md) and [metric explorer](app-insights-metrics-explorer.md) results.</span></span>

![Filtrování u vlastnosti](./media/app-insights-separate-resources/050-filter.png)

<span data-ttu-id="77fe7-154">Existuje několik různých metod nastavení vlastnosti verze aplikace.</span><span class="sxs-lookup"><span data-stu-id="77fe7-154">There are several different methods of setting the Application Version property.</span></span>

* <span data-ttu-id="77fe7-155">Nastavte přímo:</span><span class="sxs-lookup"><span data-stu-id="77fe7-155">Set directly:</span></span>

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* <span data-ttu-id="77fe7-156">Zabalení daného řádku v [telemetrie inicializátoru](app-insights-api-custom-events-metrics.md#defaults) zajistit, že jsou všechny instance TelemetryClient konzistentně nastavené.</span><span class="sxs-lookup"><span data-stu-id="77fe7-156">Wrap that line in a [telemetry initializer](app-insights-api-custom-events-metrics.md#defaults) to ensure that all TelemetryClient instances are set consistently.</span></span>
* <span data-ttu-id="77fe7-157">[ASP.NET] Nastavit verzi `BuildInfo.config`.</span><span class="sxs-lookup"><span data-stu-id="77fe7-157">[ASP.NET] Set the version in `BuildInfo.config`.</span></span> <span data-ttu-id="77fe7-158">Modul web vyzvedne, až bude verze z uzlu BuildLabel.</span><span class="sxs-lookup"><span data-stu-id="77fe7-158">The web module will pick up the version from the BuildLabel node.</span></span> <span data-ttu-id="77fe7-159">Zahrnout tento soubor ve vašem projektu a nezapomeňte nastavit vlastnost kopírování vždy v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="77fe7-159">Include this file in your project and remember to set the Copy Always property in Solution Explorer.</span></span>

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* <span data-ttu-id="77fe7-160">[ASP.NET] Automaticky generovat BuildInfo.config v nástroji MSBuild.</span><span class="sxs-lookup"><span data-stu-id="77fe7-160">[ASP.NET] Generate BuildInfo.config automatically in MSBuild.</span></span> <span data-ttu-id="77fe7-161">K tomuto účelu přidat pár řádků do vaší `.csproj` souboru:</span><span class="sxs-lookup"><span data-stu-id="77fe7-161">To do this, add a few lines to your `.csproj` file:</span></span>

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    <span data-ttu-id="77fe7-162">Tím se vygeneruje soubor s názvem *Názevvašehoprojektu*. BuildInfo.config. Proces publikování se přejmenuje na BuildInfo.config.</span><span class="sxs-lookup"><span data-stu-id="77fe7-162">This generates a file called *yourProjectName*.BuildInfo.config. The Publish process renames it to BuildInfo.config.</span></span>

    <span data-ttu-id="77fe7-163">Popisek sestavení obsahuje zástupný znak (AutoGen_...), když vytvoříte pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="77fe7-163">The build label contains a placeholder (AutoGen_...) when you build with Visual Studio.</span></span> <span data-ttu-id="77fe7-164">Ale když vytvořené pomocí nástroje MSBuild, je naplněn číslo správnou verzi.</span><span class="sxs-lookup"><span data-stu-id="77fe7-164">But when built with MSBuild, it is populated with the correct version number.</span></span>

    <span data-ttu-id="77fe7-165">Chcete-li povolit MSBuild ke generování čísel verzí, nastavte verze jako `1.0.*` v AssemblyReference.cs</span><span class="sxs-lookup"><span data-stu-id="77fe7-165">To allow MSBuild to generate version numbers, set the version like `1.0.*` in AssemblyReference.cs</span></span>

## <a name="version-and-release-tracking"></a><span data-ttu-id="77fe7-166">Sledování verzí a vydání</span><span class="sxs-lookup"><span data-stu-id="77fe7-166">Version and release tracking</span></span>
<span data-ttu-id="77fe7-167">Pokud chcete sledovat verzi aplikace, ujistěte se, že proces Microsoft Build Engine vygeneroval soubor `buildinfo.config`.</span><span class="sxs-lookup"><span data-stu-id="77fe7-167">To track the application version, make sure `buildinfo.config` is generated by your Microsoft Build Engine process.</span></span> <span data-ttu-id="77fe7-168">Do souboru .csproj přidejte:</span><span class="sxs-lookup"><span data-stu-id="77fe7-168">In your .csproj file, add:</span></span>  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

<span data-ttu-id="77fe7-169">Pokud obsahuje informace o sestavení, webový modul Application Insights automaticky přidá položku **Verze aplikace** jako vlastnost pro každý předmět telemetrie.</span><span class="sxs-lookup"><span data-stu-id="77fe7-169">When it has the build info, the Application Insights web module automatically adds **Application version** as a property to every item of telemetry.</span></span> <span data-ttu-id="77fe7-170">Díky tomu můžete při provádění [diagnostických hledání](app-insights-diagnostic-search.md) nebo při [zkoumání metrik](app-insights-metrics-explorer.md) filtrovat podle verze.</span><span class="sxs-lookup"><span data-stu-id="77fe7-170">That allows you to filter by version when you perform [diagnostic searches](app-insights-diagnostic-search.md), or when you [explore metrics](app-insights-metrics-explorer.md).</span></span>

<span data-ttu-id="77fe7-171">Všimněte si však, že číslo verze sestavení je generováno pouze pomocí procesu Microsoft Build Engine, ne sestavením vývojáře v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="77fe7-171">However, notice that the build version number is generated only by the Microsoft Build Engine, not by the developer build in Visual Studio.</span></span>

### <a name="release-annotations"></a><span data-ttu-id="77fe7-172">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="77fe7-172">Release annotations</span></span>
<span data-ttu-id="77fe7-173">Pokud používáte Visual Studio Team Services, můžete [získat značku poznámek](app-insights-annotations.md), kterou můžete přidat do svých grafů pokaždé, když vydáte novou verzi.</span><span class="sxs-lookup"><span data-stu-id="77fe7-173">If you use Visual Studio Team Services, you can [get an annotation marker](app-insights-annotations.md) added to your charts whenever you release a new version.</span></span> <span data-ttu-id="77fe7-174">Následující obrázek ukazuje, jak se tato značka zobrazuje.</span><span class="sxs-lookup"><span data-stu-id="77fe7-174">The following image shows how this marker appears.</span></span>

![Snímek obrazovky grafu s ukázkovou poznámkou k verzi](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a><span data-ttu-id="77fe7-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77fe7-176">Next steps</span></span>

* [<span data-ttu-id="77fe7-177">Sdílené prostředky pro víc rolí</span><span class="sxs-lookup"><span data-stu-id="77fe7-177">Shared resources for multiple roles</span></span>](app-insights-monitor-multi-role-apps.md)
* [<span data-ttu-id="77fe7-178">Vytvoření inicializátoru Telemetrie k rozlišení A | Variant B</span><span class="sxs-lookup"><span data-stu-id="77fe7-178">Create a Telemetry Initializer to distinguish A|B variants</span></span>](app-insights-api-filtering-sampling.md#add-properties)
