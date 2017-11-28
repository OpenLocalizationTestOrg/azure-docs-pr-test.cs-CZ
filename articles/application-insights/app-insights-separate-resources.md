---
title: "aaaSeparating telemetrie z vývoj, testování a verze ve službě Azure Application Insights | Microsoft Docs"
description: "Přímé telemetrie toodifferent prostředky pro vývoj, testování a provozním razítka."
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
ms.openlocfilehash: a294c8c70f46d7c29b460461c3494c83e13a0cbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="separating-telemetry-from-development-test-and-production"></a><span data-ttu-id="c8395-103">Oddělení telemetrie z vývoj, testování a provozním</span><span class="sxs-lookup"><span data-stu-id="c8395-103">Separating telemetry from Development, Test, and Production</span></span>

<span data-ttu-id="c8395-104">Při vývoji hello další verze webové aplikace, které nechcete toomix až hello [Application Insights](app-insights-overview.md) telemetrie z hello nové verze a hello již vydané verze.</span><span class="sxs-lookup"><span data-stu-id="c8395-104">When you are developing hello next version of a web application, you don't want toomix up hello [Application Insights](app-insights-overview.md) telemetry from hello new version and hello already released version.</span></span> <span data-ttu-id="c8395-105">tooavoid nedorozuměním odesílání telemetrie hello z různých vývoj zpracuje tooseparate prostředky Application Insights, s klíči samostatné instrumentace (ikeys).</span><span class="sxs-lookup"><span data-stu-id="c8395-105">tooavoid confusion, send hello telemetry from different development stages tooseparate Application Insights resources, with separate instrumentation keys (ikeys).</span></span> <span data-ttu-id="c8395-106">toomake je snazší klíč instrumentace hello toochange jako verze přesune z tooanother v jediném kroku, může být užitečné tooset hello ikey v kódu místo v konfiguračním souboru hello.</span><span class="sxs-lookup"><span data-stu-id="c8395-106">toomake it easier toochange hello instrumentation key as a version moves from one stage tooanother, it can be useful tooset hello ikey in code instead of in hello configuration file.</span></span> 

<span data-ttu-id="c8395-107">(Pokud je váš systém cloudové služby Azure je [jinou metodu nastavení samostatné ikeys](app-insights-cloudservices.md).)</span><span class="sxs-lookup"><span data-stu-id="c8395-107">(If your system is an Azure Cloud Service, there's [another method of setting separate ikeys](app-insights-cloudservices.md).)</span></span>

## <a name="about-resources-and-instrumentation-keys"></a><span data-ttu-id="c8395-108">O prostředcích a klíčů instrumentace</span><span class="sxs-lookup"><span data-stu-id="c8395-108">About resources and instrumentation keys</span></span>

<span data-ttu-id="c8395-109">Když jste nastavili Application Insights monitorování pro webovou aplikaci, můžete vytvořit Application Insights *prostředků* v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c8395-109">When you set up Application Insights monitoring for your web app, you create an Application Insights *resource* in Microsoft Azure.</span></span> <span data-ttu-id="c8395-110">Otevřete tento prostředek v hello portálu Azure v pořadí toosee a analyzovat hello telemetrii získanou z vašich aplikací.</span><span class="sxs-lookup"><span data-stu-id="c8395-110">You open this resource in hello Azure portal in order toosee and analyze hello telemetry collected from your app.</span></span> <span data-ttu-id="c8395-111">je identifikována Hello prostředků *klíč instrumentace* (ikey).</span><span class="sxs-lookup"><span data-stu-id="c8395-111">hello resource is identified by an *instrumentation key* (ikey).</span></span> <span data-ttu-id="c8395-112">Při instalaci hello Application Insights balíček toomonitor vaší aplikace, můžete nakonfigurovat ji hello klíč instrumentace, tak, že ví, kde toosend hello telemetrie.</span><span class="sxs-lookup"><span data-stu-id="c8395-112">When you install hello Application Insights package toomonitor your app, you configure it with hello instrumentation key, so that it knows where toosend hello telemetry.</span></span>

<span data-ttu-id="c8395-113">Obvykle zvolíte toouse samostatné prostředky nebo jeden sdílený prostředek v různých scénářích:</span><span class="sxs-lookup"><span data-stu-id="c8395-113">You typically choose toouse separate resources or a single shared resource in different scenarios:</span></span>

* <span data-ttu-id="c8395-114">Jiné, nezávislé aplikace – použít samostatné prostředků a ikey pro každou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c8395-114">Different, independent applications - Use a separate resource and ikey for each app.</span></span>
* <span data-ttu-id="c8395-115">Více součástí nebo role jeden obchodní aplikace – použijte [jeden sdílený prostředek](app-insights-monitor-multi-role-apps.md) pro všechny součásti aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c8395-115">Multiple components or roles of one business application - Use a [single shared resource](app-insights-monitor-multi-role-apps.md) for all hello component apps.</span></span> <span data-ttu-id="c8395-116">Telemetrická data lze filtrovat a segmentované podle vlastnosti cloud_RoleName hello.</span><span class="sxs-lookup"><span data-stu-id="c8395-116">Telemetry can be filtered or segmented by hello cloud_RoleName property.</span></span>
* <span data-ttu-id="c8395-117">Vývoj, testování a verze – použijte samostatné prostředků a ikey pro verze systému hello v 'razítka, nebo fáze produkce.</span><span class="sxs-lookup"><span data-stu-id="c8395-117">Development, Test, and Release - Use a separate resource and ikey for versions of hello system in 'stamp' or stage of production.</span></span>
* <span data-ttu-id="c8395-118">A | Testování B – použít jeden zdroj.</span><span class="sxs-lookup"><span data-stu-id="c8395-118">A | B testing - Use a single resource.</span></span> <span data-ttu-id="c8395-119">Vytvořte TelemetryInitializer tooadd telemetrie toohello vlastnost, která identifikuje hello variant.</span><span class="sxs-lookup"><span data-stu-id="c8395-119">Create a TelemetryInitializer tooadd a property toohello telemetry that identifies hello variants.</span></span>


## <span data-ttu-id="c8395-120"><a name="dynamic-ikey"></a>Klíč dynamické instrumentace</span><span class="sxs-lookup"><span data-stu-id="c8395-120"><a name="dynamic-ikey"></a> Dynamic instrumentation key</span></span>

<span data-ttu-id="c8395-121">toomake, které je snazší toochange hello ikey jako kód hello přesune mezi fáze produkce, nastavit kód místo v konfiguračním souboru hello.</span><span class="sxs-lookup"><span data-stu-id="c8395-121">toomake it easier toochange hello ikey as hello code moves between stages of production, set it in code instead of in hello configuration file.</span></span>

<span data-ttu-id="c8395-122">V metodě inicializace, jako jsou například souboru global.aspx.cs v službě ASP.NET nastavit hello:</span><span class="sxs-lookup"><span data-stu-id="c8395-122">Set hello key in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

<span data-ttu-id="c8395-123">*C#*</span><span class="sxs-lookup"><span data-stu-id="c8395-123">*C#*</span></span>

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

<span data-ttu-id="c8395-124">V tomto příkladu jsou hello ikeys pro různé prostředky hello umístěny v různých verzích hello soubor webové konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c8395-124">In this example, hello ikeys for hello different resources are placed in different versions of hello web configuration file.</span></span> <span data-ttu-id="c8395-125">Prohazuje hello soubor webové konfigurace – což lze provést v rámci skriptu verze hello - bude Prohodit hello cílový prostředek.</span><span class="sxs-lookup"><span data-stu-id="c8395-125">Swapping hello web configuration file - which you can do as part of hello release script - will swap hello target resource.</span></span>

### <a name="web-pages"></a><span data-ttu-id="c8395-126">Webové stránky</span><span class="sxs-lookup"><span data-stu-id="c8395-126">Web pages</span></span>
<span data-ttu-id="c8395-127">Hello iKey používá taky na webových stránkách vaší aplikace, v hello [skript, který jste získali v okně rychlý start hello](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="c8395-127">hello iKey is also used in your app's web pages, in hello [script that you got from hello quick start blade](app-insights-javascript.md).</span></span> <span data-ttu-id="c8395-128">Místo kódování je oznámena do skriptu hello, vygenerujte ho z stav serveru hello.</span><span class="sxs-lookup"><span data-stu-id="c8395-128">Instead of coding it literally into hello script, generate it from hello server state.</span></span> <span data-ttu-id="c8395-129">Například v aplikaci ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="c8395-129">For example, in an ASP.NET app:</span></span>

<span data-ttu-id="c8395-130">*JavaScript ve Razor*</span><span class="sxs-lookup"><span data-stu-id="c8395-130">*JavaScript in Razor*</span></span>

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a><span data-ttu-id="c8395-131">Vytvořit další prostředky Application Insights</span><span class="sxs-lookup"><span data-stu-id="c8395-131">Create additional Application Insights resources</span></span>
<span data-ttu-id="c8395-132">telemetrie tooseparate pro součásti jiné aplikace, nebo pro jiný razítky (dev/testovací/produkční) hello stejné součásti a potom je budete mít toocreate nový prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="c8395-132">tooseparate telemetry for different application components, or for different stamps (dev/test/production) of hello same component, then you'll have toocreate a new Application Insights resource.</span></span>

<span data-ttu-id="c8395-133">V hello [portal.azure.com](https://portal.azure.com), přidejte prostředek Application Insights:</span><span class="sxs-lookup"><span data-stu-id="c8395-133">In hello [portal.azure.com](https://portal.azure.com), add an Application Insights resource:</span></span>

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-separate-resources/01-new.png)

* <span data-ttu-id="c8395-135">**Typ aplikace** ovlivňuje, co se zobrazí v okně Přehled hello a hello vlastnosti, které jsou k dispozici v [metriky explorer](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="c8395-135">**Application type** affects what you see on hello overview blade and hello properties available in [metric explorer](app-insights-metrics-explorer.md).</span></span> <span data-ttu-id="c8395-136">Pokud nevidíte vašeho typu aplikace, vyberte jeden z typů webových hello pro webové stránky.</span><span class="sxs-lookup"><span data-stu-id="c8395-136">If you don't see your type of app, choose one of hello web types for web pages.</span></span>
* <span data-ttu-id="c8395-137">**Skupina prostředků** je užitečný pro správu vlastnosti, například [řízení přístupu](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="c8395-137">**Resource group** is a convenience for managing properties like [access control](app-insights-resources-roles-access-control.md).</span></span> <span data-ttu-id="c8395-138">Pro vývoj, testování a provozním můžete použít samostatné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="c8395-138">You could use separate resource groups for development, test, and production.</span></span>
* <span data-ttu-id="c8395-139">**Předplatné** je váš účet platebních v Azure.</span><span class="sxs-lookup"><span data-stu-id="c8395-139">**Subscription** is your payment account in Azure.</span></span>
* <span data-ttu-id="c8395-140">**Umístění** je, kde společnost Microsoft uchovávat data.</span><span class="sxs-lookup"><span data-stu-id="c8395-140">**Location** is where we keep your data.</span></span> <span data-ttu-id="c8395-141">Aktuálně jej nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="c8395-141">Currently it can't be changed.</span></span> 
* <span data-ttu-id="c8395-142">**Přidat toodashboard** vloží dlaždici rychlý přístup pro prostředek na Azure domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="c8395-142">**Add toodashboard** puts a quick-access tile for your resource on your Azure Home page.</span></span> 

<span data-ttu-id="c8395-143">Vytvoření prostředku hello trvá několik sekund.</span><span class="sxs-lookup"><span data-stu-id="c8395-143">Creating hello resource takes a few seconds.</span></span> <span data-ttu-id="c8395-144">Pokud se provádí, zobrazí se výstraha.</span><span class="sxs-lookup"><span data-stu-id="c8395-144">You'll see an alert when it's done.</span></span>

<span data-ttu-id="c8395-145">(Můžete napsat [skript prostředí PowerShell](app-insights-powershell-script-create-resource.md) toocreate prostředek automaticky.)</span><span class="sxs-lookup"><span data-stu-id="c8395-145">(You can write a [PowerShell script](app-insights-powershell-script-create-resource.md) toocreate a resource automatically.)</span></span>

### <a name="getting-hello-instrumentation-key"></a><span data-ttu-id="c8395-146">Získávání klíč instrumentace hello</span><span class="sxs-lookup"><span data-stu-id="c8395-146">Getting hello instrumentation key</span></span>
<span data-ttu-id="c8395-147">klíč instrumentace Hello identifikuje hello prostředek, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c8395-147">hello instrumentation key identifies hello resource that you created.</span></span> 

![Klikněte na tlačítko Essentials, klikněte na tlačítko hello klíč instrumentace, CTRL + C](./media/app-insights-separate-resources/02-props.png)

<span data-ttu-id="c8395-149">Je třeba klíčů instrumentace hello z všechny prostředky toowhich hello vaše aplikace bude posílat data.</span><span class="sxs-lookup"><span data-stu-id="c8395-149">You need hello instrumentation keys of all hello resources toowhich your app will send data.</span></span>

## <a name="filter-on-build-number"></a><span data-ttu-id="c8395-150">Filtrovat podle čísla sestavení</span><span class="sxs-lookup"><span data-stu-id="c8395-150">Filter on build number</span></span>
<span data-ttu-id="c8395-151">Při publikování nové verze aplikace, budete muset toobe možné tooseparate hello telemetrie z jiné sestavení.</span><span class="sxs-lookup"><span data-stu-id="c8395-151">When you publish a new version of your app, you'll want toobe able tooseparate hello telemetry from different builds.</span></span>

<span data-ttu-id="c8395-152">Vlastnost verze aplikace hello lze nastavit tak, aby můžete filtrovat [vyhledávání](app-insights-diagnostic-search.md) a [metriky explorer](app-insights-metrics-explorer.md) výsledky.</span><span class="sxs-lookup"><span data-stu-id="c8395-152">You can set hello Application Version property so that you can filter [search](app-insights-diagnostic-search.md) and [metric explorer](app-insights-metrics-explorer.md) results.</span></span>

![Filtrování u vlastnosti](./media/app-insights-separate-resources/050-filter.png)

<span data-ttu-id="c8395-154">Existuje několik různých metod nastavení vlastnosti verze aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c8395-154">There are several different methods of setting hello Application Version property.</span></span>

* <span data-ttu-id="c8395-155">Nastavte přímo:</span><span class="sxs-lookup"><span data-stu-id="c8395-155">Set directly:</span></span>

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* <span data-ttu-id="c8395-156">Zabalení daného řádku v [telemetrie inicializátoru](app-insights-api-custom-events-metrics.md#defaults) tooensure, zda jsou všechny instance TelemetryClient konzistentně nastavena.</span><span class="sxs-lookup"><span data-stu-id="c8395-156">Wrap that line in a [telemetry initializer](app-insights-api-custom-events-metrics.md#defaults) tooensure that all TelemetryClient instances are set consistently.</span></span>
* <span data-ttu-id="c8395-157">[ASP.NET] Nastavit verzi hello v `BuildInfo.config`.</span><span class="sxs-lookup"><span data-stu-id="c8395-157">[ASP.NET] Set hello version in `BuildInfo.config`.</span></span> <span data-ttu-id="c8395-158">v modulu web Hello vyzvedne, až bude hello verze z uzlu BuildLabel hello.</span><span class="sxs-lookup"><span data-stu-id="c8395-158">hello web module will pick up hello version from hello BuildLabel node.</span></span> <span data-ttu-id="c8395-159">Zahrnout tento soubor do projektu a zapamatovat si tooset hello kopie vždy vlastnost v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="c8395-159">Include this file in your project and remember tooset hello Copy Always property in Solution Explorer.</span></span>

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
* <span data-ttu-id="c8395-160">[ASP.NET] Automaticky generovat BuildInfo.config v nástroji MSBuild.</span><span class="sxs-lookup"><span data-stu-id="c8395-160">[ASP.NET] Generate BuildInfo.config automatically in MSBuild.</span></span> <span data-ttu-id="c8395-161">toodo, přidat pár řádků tooyour `.csproj` souboru:</span><span class="sxs-lookup"><span data-stu-id="c8395-161">toodo this, add a few lines tooyour `.csproj` file:</span></span>

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    <span data-ttu-id="c8395-162">Tím se vygeneruje soubor s názvem *Názevvašehoprojektu*. BuildInfo.config. hello proces publikování se přejmenuje tooBuildInfo.config.</span><span class="sxs-lookup"><span data-stu-id="c8395-162">This generates a file called *yourProjectName*.BuildInfo.config. hello Publish process renames it tooBuildInfo.config.</span></span>

    <span data-ttu-id="c8395-163">Popisek sestavení Hello obsahuje zástupný znak (AutoGen_...), když vytvoříte pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8395-163">hello build label contains a placeholder (AutoGen_...) when you build with Visual Studio.</span></span> <span data-ttu-id="c8395-164">Ale když vytvořené pomocí nástroje MSBuild, je naplněn hello správné číslo verze.</span><span class="sxs-lookup"><span data-stu-id="c8395-164">But when built with MSBuild, it is populated with hello correct version number.</span></span>

    <span data-ttu-id="c8395-165">čísla verzí nástroje MSBuild toogenerate tooallow, nastavit hello verzi jako `1.0.*` v AssemblyReference.cs</span><span class="sxs-lookup"><span data-stu-id="c8395-165">tooallow MSBuild toogenerate version numbers, set hello version like `1.0.*` in AssemblyReference.cs</span></span>

## <a name="version-and-release-tracking"></a><span data-ttu-id="c8395-166">Sledování verzí a vydání</span><span class="sxs-lookup"><span data-stu-id="c8395-166">Version and release tracking</span></span>
<span data-ttu-id="c8395-167">verze aplikace hello tootrack, zajistěte, aby `buildinfo.config` je generován váš proces Microsoft Build Engine.</span><span class="sxs-lookup"><span data-stu-id="c8395-167">tootrack hello application version, make sure `buildinfo.config` is generated by your Microsoft Build Engine process.</span></span> <span data-ttu-id="c8395-168">Do souboru .csproj přidejte:</span><span class="sxs-lookup"><span data-stu-id="c8395-168">In your .csproj file, add:</span></span>  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

<span data-ttu-id="c8395-169">Pokud má hello informace o sestavení, webový modul Application Insights hello automaticky přidá **verze aplikace** jako položka tooevery vlastnost telemetrie.</span><span class="sxs-lookup"><span data-stu-id="c8395-169">When it has hello build info, hello Application Insights web module automatically adds **Application version** as a property tooevery item of telemetry.</span></span> <span data-ttu-id="c8395-170">Což vám umožní toofilter podle verze při provádění [diagnostických hledání](app-insights-diagnostic-search.md), nebo když jste [zkoumat metriky](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="c8395-170">That allows you toofilter by version when you perform [diagnostic searches](app-insights-diagnostic-search.md), or when you [explore metrics](app-insights-metrics-explorer.md).</span></span>

<span data-ttu-id="c8395-171">Všimněte si však, že hello číslo verze sestavení je generováno pouze pomocí hello Microsoft Build Engine, ne podle vývojáře hello sestavení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8395-171">However, notice that hello build version number is generated only by hello Microsoft Build Engine, not by hello developer build in Visual Studio.</span></span>

### <a name="release-annotations"></a><span data-ttu-id="c8395-172">Poznámky k verzi</span><span class="sxs-lookup"><span data-stu-id="c8395-172">Release annotations</span></span>
<span data-ttu-id="c8395-173">Pokud používáte Visual Studio Team Services, můžete [získat značku poznámky](app-insights-annotations.md) přidat tooyour grafy při každém vydání nové verze.</span><span class="sxs-lookup"><span data-stu-id="c8395-173">If you use Visual Studio Team Services, you can [get an annotation marker](app-insights-annotations.md) added tooyour charts whenever you release a new version.</span></span> <span data-ttu-id="c8395-174">Hello následující obrázek ukazuje, jak se zobrazí tato značka.</span><span class="sxs-lookup"><span data-stu-id="c8395-174">hello following image shows how this marker appears.</span></span>

![Snímek obrazovky grafu s ukázkovou poznámkou k verzi](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a><span data-ttu-id="c8395-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8395-176">Next steps</span></span>

* [<span data-ttu-id="c8395-177">Sdílené prostředky pro víc rolí</span><span class="sxs-lookup"><span data-stu-id="c8395-177">Shared resources for multiple roles</span></span>](app-insights-monitor-multi-role-apps.md)
* [<span data-ttu-id="c8395-178">Vytvoření toodistinguish inicializátoru Telemetrie A | Variant B</span><span class="sxs-lookup"><span data-stu-id="c8395-178">Create a Telemetry Initializer toodistinguish A|B variants</span></span>](app-insights-api-filtering-sampling.md#add-properties)
