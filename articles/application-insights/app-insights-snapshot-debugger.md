---
title: "aaaAzure ladicí program snímku Application Insights pro aplikace .NET | Microsoft Docs"
description: "Ladění snímky jsou shromažďovány automaticky, pokud jsou výjimky vyvolány v produkční aplikace .NET"
services: application-insights
documentationcenter: 
author: qubitron
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: bwren
ms.openlocfilehash: f0173a752b5795d934fbab1bd53eb077433edc90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a><span data-ttu-id="af9e0-103">Ladění snímků výjimky v aplikacích .NET</span><span class="sxs-lookup"><span data-stu-id="af9e0-103">Debug snapshots on exceptions in .NET apps</span></span>

<span data-ttu-id="af9e0-104">Když dojde k výjimce, může automaticky shromažďovat snímku ladění z provozu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="af9e0-104">When an exception occurs, you can automatically collect a debug snapshot from your live web application.</span></span> <span data-ttu-id="af9e0-105">Hello snímku ukazuje stav hello zdrojový kód a došlo k proměnné v hello chvíli hello výjimka.</span><span class="sxs-lookup"><span data-stu-id="af9e0-105">hello snapshot shows hello state of source code and variables at hello moment hello exception was thrown.</span></span> <span data-ttu-id="af9e0-106">Hello ladicí program snímku (preview) v [Azure Application Insights](app-insights-overview.md) monitoruje výjimka telemetrie z vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="af9e0-106">hello Snapshot Debugger (preview) in [Azure Application Insights](app-insights-overview.md) monitors exception telemetry from your web app.</span></span> <span data-ttu-id="af9e0-107">Shromažďuje snímky na vaše horní vyvolání výjimky, tak, aby hello informace, které potřebujete toodiagnose problémy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="af9e0-107">It collects snapshots on your top-throwing exceptions so that you have hello information you need toodiagnose issues in production.</span></span> <span data-ttu-id="af9e0-108">Zahrnout hello [balíček NuGet kolekce snímku](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) ve vaší aplikaci a volitelně nakonfigurujte parametry kolekce v [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Snímky zobrazí na [výjimky](app-insights-asp-net-exceptions.md) hello portálu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="af9e0-108">Include hello [Snapshot collector NuGet package](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in your application, and optionally configure collection parameters in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Snapshots appear on [exceptions](app-insights-asp-net-exceptions.md) in hello Application Insights portal.</span></span>

<span data-ttu-id="af9e0-109">Můžete zobrazit ladění snímky v zásobníku volání hello portálu toosee hello a prohlédnout proměnné v každé rámce zásobníku volání.</span><span class="sxs-lookup"><span data-stu-id="af9e0-109">You can view debug snapshots in hello portal toosee hello call stack and inspect variables at each call stack frame.</span></span> <span data-ttu-id="af9e0-110">tooget výkonnější ladění zkušeností se zdrojovým kódem open snímky s Visual Studio Enterprise 2017 podle [stahování hello ladicí program snímku rozšíření pro Visual Studio](https://aka.ms/snapshotdebugger).</span><span class="sxs-lookup"><span data-stu-id="af9e0-110">tooget a more powerful debugging experience with source code, open snapshots with Visual Studio 2017 Enterprise by [downloading hello Snapshot Debugger extension for Visual Studio](https://aka.ms/snapshotdebugger).</span></span>

<span data-ttu-id="af9e0-111">Snímek kolekce je k dispozici pro:</span><span class="sxs-lookup"><span data-stu-id="af9e0-111">Snapshot collection is available for:</span></span>
* <span data-ttu-id="af9e0-112">Aplikace rozhraní .NET framework a ASP.NET spuštění rozhraní .NET Framework 4.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="af9e0-112">.NET Framework and ASP.NET applications running .NET Framework 4.5 or later.</span></span>
* <span data-ttu-id="af9e0-113">Aplikace rozhraní .NET 2.0 core a ASP.NET Core 2.0 v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="af9e0-113">.NET Core 2.0 and ASP.NET Core 2.0 applications running on Windows.</span></span>

### <a name="configure-snapshot-collection-for-aspnet-applications"></a><span data-ttu-id="af9e0-114">Konfigurace shromažďování snímků pro aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="af9e0-114">Configure snapshot collection for ASP.NET applications</span></span>

1. <span data-ttu-id="af9e0-115">[Povolit Application Insights ve vaší webové aplikaci](app-insights-asp-net.md), pokud jste ho ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="af9e0-115">[Enable Application Insights in your web app](app-insights-asp-net.md), if you haven't done it yet.</span></span>

2. <span data-ttu-id="af9e0-116">Zahrnout hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) balíček NuGet v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af9e0-116">Include hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span> 

3. <span data-ttu-id="af9e0-117">Zkontrolujte hello výchozí možnosti, které hello balíček přidat příliš[souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="af9e0-117">Review hello default options that hello package added too[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span>

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- hello default is true, but you can disable Snapshot Debugging by setting it toofalse -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this tootrue. -->
        <!-- DeveloperMode is a property on hello active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need toosee an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- hello maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- hello maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often tooreset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- hello maximum number of snapshots allowed in one minute. -->
        <SnapshotsPerMinuteLimit>2</SnapshotsPerMinuteLimit>
        <!-- hello maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. <span data-ttu-id="af9e0-118">Snímky se shromažďují pouze na výjimky, které jsou hlášené tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="af9e0-118">Snapshots are collected only on exceptions that are reported tooApplication Insights.</span></span> <span data-ttu-id="af9e0-119">V některých případech (například starší verze platformy .NET hello), může být příliš nutné[konfigurace shromažďování výjimek](app-insights-asp-net-exceptions.md#exceptions) toosee výjimky se snímky hello portálu.</span><span class="sxs-lookup"><span data-stu-id="af9e0-119">In some cases (for example, older versions of hello .NET platform), you might need too[configure exception collection](app-insights-asp-net-exceptions.md#exceptions) toosee exceptions with snapshots in hello portal.</span></span>


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a><span data-ttu-id="af9e0-120">Konfigurace shromažďování snímků pro technologii ASP.NET 2.0 základní aplikace</span><span class="sxs-lookup"><span data-stu-id="af9e0-120">Configure snapshot collection for ASP.NET Core 2.0 applications</span></span>

1. <span data-ttu-id="af9e0-121">[Povolit Application Insights ve vaší webové aplikaci ASP.NET Core](app-insights-asp-net-core.md), pokud jste ho ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="af9e0-121">[Enable Application Insights in your ASP.NET Core web app](app-insights-asp-net-core.md), if you haven't done it yet.</span></span>

2. <span data-ttu-id="af9e0-122">Zahrnout hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) balíček NuGet v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af9e0-122">Include hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span>

3. <span data-ttu-id="af9e0-123">Upravit hello `ConfigureServices` metoda ve vaší aplikaci `Startup` třídy tooadd hello snímek kolekce telemetrie procesoru.</span><span class="sxs-lookup"><span data-stu-id="af9e0-123">Modify hello `ConfigureServices` method in your application's `Startup` class tooadd hello Snapshot Collector's telemetry processor.</span></span> <span data-ttu-id="af9e0-124">Hello kódu, které byste měli přidat závisí na hello odkazované verzi hello balíček Microsoft.ApplicationInsights.ASPNETCore NuGet.</span><span class="sxs-lookup"><span data-stu-id="af9e0-124">hello code you should add depends on hello referenced version of hello Microsoft.ApplicationInsights.ASPNETCore NuGet package.</span></span>

   <span data-ttu-id="af9e0-125">Pro Microsoft.ApplicationInsights.AspNetCore 2.1.0 přidejte:</span><span class="sxs-lookup"><span data-stu-id="af9e0-125">For Microsoft.ApplicationInsights.AspNetCore 2.1.0 add:</span></span>
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
           services.AddSingleton<Func<ITelemetryProcessor, ITelemetryProcessor>>(next => new SnapshotCollectorTelemetryProcessor(next));
           // TODO: Add any other services your application needs here.
       }
   }
   ```

   <span data-ttu-id="af9e0-126">Pro Microsoft.ApplicationInsights.AspNetCore 2.1.1 přidejte:</span><span class="sxs-lookup"><span data-stu-id="af9e0-126">For Microsoft.ApplicationInsights.AspNetCore 2.1.1 add:</span></span>
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           public ITelemetryProcessor Create(ITelemetryProcessor next) =>
               new SnapshotCollectorTelemetryProcessor(next);
       }

       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
            services.AddSingleton<ITelemetryProcessorFactory>(new SnapshotCollectorTelemetryProcessorFactory());
           // TODO: Add any other services your application needs here.
       }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a><span data-ttu-id="af9e0-127">Konfigurace shromažďování snímků pro jiné aplikace rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="af9e0-127">Configure snapshot collection for other .NET applications</span></span>

1. <span data-ttu-id="af9e0-128">Pokud vaše aplikace není instrumentována již s nástrojem Application Insights, začít [povolení Application Insights a klíč instrumentace hello nastavení](app-insights-windows-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="af9e0-128">If your application is not already instrumented with Application Insights, get started by [enabling Application Insights and setting hello instrumentation key](app-insights-windows-desktop.md).</span></span>

2. <span data-ttu-id="af9e0-129">Přidat hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) balíček NuGet v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af9e0-129">Add hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span>

3. <span data-ttu-id="af9e0-130">Snímky se shromažďují pouze na výjimky, které jsou hlášené tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="af9e0-130">Snapshots are collected only on exceptions that are reported tooApplication Insights.</span></span> <span data-ttu-id="af9e0-131">Může být nutné toomodify tooreport váš kód je.</span><span class="sxs-lookup"><span data-stu-id="af9e0-131">You may need toomodify your code tooreport them.</span></span> <span data-ttu-id="af9e0-132">zpracování kódu výjimek Hello závisí na hello struktura vaší aplikace, ale zde je příklad:</span><span class="sxs-lookup"><span data-stu-id="af9e0-132">hello exception handling code depends on hello structure of your application, but an example is below:</span></span>
    ```C#
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle hello request.
        }
        catch (Exception ex)
        {
            // Report hello exception tooApplication Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow hello exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a><span data-ttu-id="af9e0-133">Udělení oprávnění</span><span class="sxs-lookup"><span data-stu-id="af9e0-133">Grant permissions</span></span>

<span data-ttu-id="af9e0-134">Vlastníci hello předplatného Azure můžete prohlédnout snímky.</span><span class="sxs-lookup"><span data-stu-id="af9e0-134">Owners of hello Azure subscription can inspect snapshots.</span></span> <span data-ttu-id="af9e0-135">Ostatní uživatelé musí udělit oprávnění vlastníka.</span><span class="sxs-lookup"><span data-stu-id="af9e0-135">Other users must be granted permission by an owner.</span></span>

<span data-ttu-id="af9e0-136">toogrant oprávnění, přiřaďte hello `Application Insights Snapshot Debugger` toousers role, který bude kontrolovat snímky.</span><span class="sxs-lookup"><span data-stu-id="af9e0-136">toogrant permission, assign hello `Application Insights Snapshot Debugger` role toousers who will inspect snapshots.</span></span> <span data-ttu-id="af9e0-137">Tato role může být přiřazena tooindividual uživatelé nebo skupiny s vlastníky předplatného pro hello cílový prostředek Application Insights nebo jeho skupiny prostředků nebo předplatného.</span><span class="sxs-lookup"><span data-stu-id="af9e0-137">This role can be assigned tooindividual users or groups by subscription owners for hello target Application Insights resource or its resource group or subscription.</span></span>

1. <span data-ttu-id="af9e0-138">Otevřete okno hello řízení přístupu (IAM).</span><span class="sxs-lookup"><span data-stu-id="af9e0-138">Open hello Access Control (IAM) blade.</span></span>
1. <span data-ttu-id="af9e0-139">Klikněte na tlačítko Přidat + hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-139">Click hello +Add button.</span></span>
1. <span data-ttu-id="af9e0-140">Hello role rozevíracím seznamu vyberte Application Insights snímku ladicí program.</span><span class="sxs-lookup"><span data-stu-id="af9e0-140">Select Application Insights Snapshot Debugger from hello Roles drop-down list.</span></span>
1. <span data-ttu-id="af9e0-141">Vyhledejte a zadejte název pro uživatele tooadd hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-141">Search for and enter a name for hello user tooadd.</span></span>
1. <span data-ttu-id="af9e0-142">Klikněte na tlačítko Uložit hello, tooadd role toohello uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-142">Click hello Save button tooadd hello user toohello role.</span></span>


[!IMPORTANT]
    <span data-ttu-id="af9e0-143">Snímky mohou obsahovat osobní a dalších citlivých informací v proměnné a parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="af9e0-143">Snapshots can potentially contain personal and other sensitive information in variable and parameter values.</span></span>

## <a name="debug-snapshots-in-hello-application-insights-portal"></a><span data-ttu-id="af9e0-144">Ladění snímky portálu Application Insights hello</span><span class="sxs-lookup"><span data-stu-id="af9e0-144">Debug snapshots in hello Application Insights portal</span></span>

<span data-ttu-id="af9e0-145">Pokud snímek je k dispozici pro danou výjimky nebo ID problému, **otevřít ladění snímek** na hello se zobrazí tlačítko [výjimka](app-insights-asp-net-exceptions.md) hello portálu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="af9e0-145">If a snapshot is available for a given exception or a problem ID, an **Open Debug Snapshot** button appears on hello [exception](app-insights-asp-net-exceptions.md) in hello Application Insights portal.</span></span>

![Tlačítko Otevřít ladění snímku na výjimky](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

<span data-ttu-id="af9e0-147">V zobrazení ladění snímku hello uvidíte zásobník volání a podokně proměnné.</span><span class="sxs-lookup"><span data-stu-id="af9e0-147">In hello Debug Snapshot view, you see a call stack and a variables pane.</span></span> <span data-ttu-id="af9e0-148">Když vyberete hello volání rámce zásobníku v podokně Zásobník volání hello, můžete zobrazit místní proměnné a parametry pro tuto funkci volání v podokně proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-148">When you select frames of hello call stack in hello call stack pane, you can view local variables and parameters for that function call in hello variables pane.</span></span>

![Zobrazení ladění snímku hello portálu](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

<span data-ttu-id="af9e0-150">Snímky mohou obsahovat citlivé informace a ve výchozím nastavení nejsou viditelná.</span><span class="sxs-lookup"><span data-stu-id="af9e0-150">Snapshots might contain sensitive information, and by default they are not viewable.</span></span> <span data-ttu-id="af9e0-151">tooview snímky, musí mít hello `Application Insights Snapshot Debugger` tooyou přiřazenou roli.</span><span class="sxs-lookup"><span data-stu-id="af9e0-151">tooview snapshots, you must have hello `Application Insights Snapshot Debugger` role assigned tooyou.</span></span>

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a><span data-ttu-id="af9e0-152">Ladění snímky s Visual Studio 2017 Enterprise</span><span class="sxs-lookup"><span data-stu-id="af9e0-152">Debug snapshots with Visual Studio 2017 Enterprise</span></span>
1. <span data-ttu-id="af9e0-153">Klikněte na tlačítko hello **stáhnout snímku** tlačítko toodownload `.diagsession` souboru, který lze otevřít v aplikaci Visual Studio Enterprise 2017.</span><span class="sxs-lookup"><span data-stu-id="af9e0-153">Click hello **Download Snapshot** button toodownload a `.diagsession` file, which can be opened by Visual Studio 2017 Enterprise.</span></span> 

2. <span data-ttu-id="af9e0-154">tooopen hello `.diagsession` souboru, je nutné nejprve [stáhněte a nainstalujte hello ladicí program snímku rozšíření pro Visual Studio](https://aka.ms/snapshotdebugger).</span><span class="sxs-lookup"><span data-stu-id="af9e0-154">tooopen hello `.diagsession` file, you must first [download and install hello Snapshot Debugger extension for Visual Studio](https://aka.ms/snapshotdebugger).</span></span>

3. <span data-ttu-id="af9e0-155">Po otevření souboru snímku hello, zobrazí se stránka minimální výpis ladění hello v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af9e0-155">After you open hello snapshot file, hello Minidump Debugging page in Visual Studio appears.</span></span> <span data-ttu-id="af9e0-156">Klikněte na tlačítko **ladění spravovaného kódu** toostart ladění hello snímku.</span><span class="sxs-lookup"><span data-stu-id="af9e0-156">Click **Debug Managed Code** toostart debugging hello snapshot.</span></span> <span data-ttu-id="af9e0-157">Hello snímku otevře toohello řádek kódu, kde byla vyvolána výjimka hello, takže můžete ladit, aktuální stav procesu hello hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-157">hello snapshot opens toohello line of code where hello exception was thrown so that you can debug hello current state of hello process.</span></span>

    ![Zobrazení ladění snímku v sadě Visual Studio](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

<span data-ttu-id="af9e0-159">stažený snímku Hello obsahuje symbol soubory, které nebyly nalezeny na vašem webovém serveru aplikace.</span><span class="sxs-lookup"><span data-stu-id="af9e0-159">hello downloaded snapshot contains any symbol files that were found on your web application server.</span></span> <span data-ttu-id="af9e0-160">Tyto soubory symbolů jsou požadované tooassociate data snímku se zdrojovým kódem.</span><span class="sxs-lookup"><span data-stu-id="af9e0-160">These symbol files are required tooassociate snapshot data with source code.</span></span> <span data-ttu-id="af9e0-161">Pro aplikace služby App Service zkontrolujte že nasazení symbol tooenable při publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="af9e0-161">For App Service apps, make sure tooenable symbol deployment when you publish your web apps.</span></span>

## <a name="how-snapshots-work"></a><span data-ttu-id="af9e0-162">Jak fungují snímky</span><span class="sxs-lookup"><span data-stu-id="af9e0-162">How snapshots work</span></span>

<span data-ttu-id="af9e0-163">Když se aplikace spustí, proces osoba samostatné snímku je vytvořen, který monitoruje vaše aplikace pro požadavky na snímku.</span><span class="sxs-lookup"><span data-stu-id="af9e0-163">When your application starts, a separate snapshot uploader process is created that monitors your application for snapshot requests.</span></span> <span data-ttu-id="af9e0-164">Pokud se požaduje snímku, stínové kopie hello spuštěn proces se provádí v přibližně 10 minut too20.</span><span class="sxs-lookup"><span data-stu-id="af9e0-164">When a snapshot is requested, a shadow copy of hello running process is made in about 10 too20 minutes.</span></span> <span data-ttu-id="af9e0-165">proces stínové Hello pak analýzy a snímku se vytvoří během procesu hlavní hello pokračuje toorun a sloužit toousers provoz.</span><span class="sxs-lookup"><span data-stu-id="af9e0-165">hello shadow process is then analyzed, and a snapshot is created while hello main process continues toorun and serve traffic toousers.</span></span> <span data-ttu-id="af9e0-166">Hello snímek je pak nahrané tooApplication Statistika společně s všechny relevantní symbolu (.pdb) soubory, které jsou potřeba tooview hello snímku.</span><span class="sxs-lookup"><span data-stu-id="af9e0-166">hello snapshot is then uploaded tooApplication Insights along with any relevant symbol (.pdb) files that are needed tooview hello snapshot.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="af9e0-167">Aktuální omezení</span><span class="sxs-lookup"><span data-stu-id="af9e0-167">Current limitations</span></span>

### <a name="publish-symbols"></a><span data-ttu-id="af9e0-168">Publikování symboly</span><span class="sxs-lookup"><span data-stu-id="af9e0-168">Publish symbols</span></span>
<span data-ttu-id="af9e0-169">Hello snímku ladicí program vyžaduje soubory symbolů na hello provozním serveru toodecode proměnné a tooprovide zkušenosti s laděním v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af9e0-169">hello Snapshot Debugger requires symbol files on hello production server toodecode variables and tooprovide a debugging experience in Visual Studio.</span></span> <span data-ttu-id="af9e0-170">Pokud tato možnost publikuje tooApp služby Hello 15.2 verzi Visual Studio 2017 publikuje symboly pro verzi sestavení ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="af9e0-170">hello 15.2 release of Visual Studio 2017 publishes symbols for release builds by default when it publishes tooApp Service.</span></span> <span data-ttu-id="af9e0-171">V předchozích verzích, potřebujete následující hello tooadd profilu publikování řádku tooyour `.pubxml` tak, aby symboly jsou publikovány v režimu vydání:</span><span class="sxs-lookup"><span data-stu-id="af9e0-171">In prior versions, you need tooadd hello following line tooyour publish profile `.pubxml` file so that symbols are published in release mode:</span></span>

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

<span data-ttu-id="af9e0-172">Pro Azure Compute a dalších typů ujistit, že soubory symbolů hello hello stejné složce dll hlavní aplikace hello (obvykle `wwwroot/bin`) nebo je k dispozici v aktuální cestě hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-172">For Azure Compute and other types, ensure that hello symbol files are in hello same folder of hello main application .dll (typically, `wwwroot/bin`) or are available on hello current path.</span></span>

### <a name="optimized-builds"></a><span data-ttu-id="af9e0-173">Optimalizované sestavení</span><span class="sxs-lookup"><span data-stu-id="af9e0-173">Optimized builds</span></span>
<span data-ttu-id="af9e0-174">V některých případech místní proměnné nelze zobrazit, v sestavení pro vydání z důvodu optimalizace, které se použijí během procesu sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-174">In some cases, local variables cannot be viewed in release builds because of optimizations that are applied during hello build process.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="af9e0-175">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="af9e0-175">Troubleshooting</span></span>

<span data-ttu-id="af9e0-176">Tyto tipy pomáhají při řešení problémů s hello snímku ladicí program.</span><span class="sxs-lookup"><span data-stu-id="af9e0-176">These tips help you troubleshoot problems with hello Snapshot Debugger.</span></span>

### <a name="verify-hello-instrumentation-key"></a><span data-ttu-id="af9e0-177">Ověřte klíč instrumentace hello</span><span class="sxs-lookup"><span data-stu-id="af9e0-177">Verify hello instrumentation key</span></span>

<span data-ttu-id="af9e0-178">Ujistěte se, že používáte klíč instrumentace správné hello v k publikované aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af9e0-178">Make sure that you're using hello correct instrumentation key in your published application.</span></span> <span data-ttu-id="af9e0-179">Application Insights obvykle čte klíč instrumentace hello z soubor ApplicationInsights.config hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-179">Usually, Application Insights reads hello instrumentation key from hello ApplicationInsights.config file.</span></span> <span data-ttu-id="af9e0-180">Ověřte, že hodnota hello je text hello stejné jako klíč instrumentace hello hello prostředku Application Insights se zobrazí na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-180">Verify that hello value is hello same as hello instrumentation key for hello Application Insights resource that you see in hello portal.</span></span>

### <a name="check-hello-uploader-logs"></a><span data-ttu-id="af9e0-181">Zkontrolujte protokoly osoba hello</span><span class="sxs-lookup"><span data-stu-id="af9e0-181">Check hello uploader logs</span></span>

<span data-ttu-id="af9e0-182">Po vytvoření snímku se vytvoří soubor s minimálním (.dmp) na disku.</span><span class="sxs-lookup"><span data-stu-id="af9e0-182">After a snapshot is created, a minidump file (.dmp) is created on disk.</span></span> <span data-ttu-id="af9e0-183">Proces samostatné osoba trvá tento soubor minimální výpis a odesílá, společně s všechny přidružené soubory PDB tooApplication ladicí program Statistika snímku úložiště.</span><span class="sxs-lookup"><span data-stu-id="af9e0-183">A separate uploader process takes that minidump file and uploads it, along with any associated PDBs, tooApplication Insights Snapshot Debugger storage.</span></span> <span data-ttu-id="af9e0-184">Po úspěšném odeslání minimální výpis hello je odstraněn z disku.</span><span class="sxs-lookup"><span data-stu-id="af9e0-184">After hello minidump has uploaded successfully, it is deleted from disk.</span></span> <span data-ttu-id="af9e0-185">na disku zůstanou zachovány Hello soubory protokolu pro hello minimální výpis procesu pro načtení.</span><span class="sxs-lookup"><span data-stu-id="af9e0-185">hello log files for hello minidump uploader are retained on disk.</span></span> <span data-ttu-id="af9e0-186">V prostředí služby App Service, můžete najít tyto protokoly v `D:\Home\LogFiles\Uploader_*.log`.</span><span class="sxs-lookup"><span data-stu-id="af9e0-186">In an App Service environment, you can find these logs in `D:\Home\LogFiles\Uploader_*.log`.</span></span> <span data-ttu-id="af9e0-187">Použití hello Kudu Správa lokality pro App Service toofind tyto soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="af9e0-187">Use hello Kudu management site for App Service toofind these log files.</span></span>

1. <span data-ttu-id="af9e0-188">Otevřete aplikaci služby App Service v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="af9e0-188">Open your App Service application in hello Azure portal.</span></span>

2. <span data-ttu-id="af9e0-189">Vyberte hello **Rozšířené nástroje** okno, nebo vyhledejte **Kudu**.</span><span class="sxs-lookup"><span data-stu-id="af9e0-189">Select hello **Advanced Tools** blade, or search for **Kudu**.</span></span>
3. <span data-ttu-id="af9e0-190">Klikněte na tlačítko **přejděte**.</span><span class="sxs-lookup"><span data-stu-id="af9e0-190">Click **Go**.</span></span>
4. <span data-ttu-id="af9e0-191">V hello **konzolou pro ladění** rozevíracím seznamu vyberte **CMD**.</span><span class="sxs-lookup"><span data-stu-id="af9e0-191">In hello **Debug console** drop-down list box, select **CMD**.</span></span>
5. <span data-ttu-id="af9e0-192">Klikněte na tlačítko **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="af9e0-192">Click **LogFiles**.</span></span>

<span data-ttu-id="af9e0-193">Měli byste vidět alespoň jeden soubor s názvem, který začíná `Uploader_` a `.log` rozšíření.</span><span class="sxs-lookup"><span data-stu-id="af9e0-193">You should see at least one file with a name that begins with `Uploader_` and a `.log` extension.</span></span> <span data-ttu-id="af9e0-194">Klikněte na příslušnou ikonu toodownload hello všechny soubory protokolů, nebo je, otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="af9e0-194">Click hello appropriate icon toodownload any log files or open them in a browser.</span></span>
<span data-ttu-id="af9e0-195">Název souboru Hello zahrnuje název počítače hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-195">hello file name includes hello machine name.</span></span> <span data-ttu-id="af9e0-196">Pokud vaše instance služby App Service je hostitelem více než jeden počítač, nejsou samostatné soubory protokolu pro každý počítač.</span><span class="sxs-lookup"><span data-stu-id="af9e0-196">If your App Service instance is hosted on more than one machine, there are separate log files for each machine.</span></span> <span data-ttu-id="af9e0-197">Zjistí-li hello osoba nový soubor s minimálními výpisy, zaznamenává se v souboru protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-197">When hello uploader detects a new minidump file, it is recorded in hello log file.</span></span> <span data-ttu-id="af9e0-198">Tady je příklad úspěšné odeslání:</span><span class="sxs-lookup"><span data-stu-id="af9e0-198">Here's an example of a successful upload:</span></span>

```
MinidumpUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:08.0349846Z
MinidumpUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 329.12 MB
    DateTime=2017-05-25T14:25:16.0145444Z
MinidumpUploader.exe Information: 0 : Upload successful.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2017-05-25T14:25:44.2310982Z
MinidumpUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2017-05-25T14:25:44.5435948Z
MinidumpUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:44.6095821Z
```

<span data-ttu-id="af9e0-199">V předchozím příkladu hello klíč instrumentace hello je `c12a605e73c44346a984e00000000000`.</span><span class="sxs-lookup"><span data-stu-id="af9e0-199">In hello previous example, hello instrumentation key is `c12a605e73c44346a984e00000000000`.</span></span> <span data-ttu-id="af9e0-200">Tato hodnota musí odpovídat hello klíč instrumentace pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af9e0-200">This value should match hello instrumentation key for your application.</span></span>
<span data-ttu-id="af9e0-201">minimální výpis Hello souvisí s snímku s hello ID `139e411a23934dc0b9ea08a626db16c5`.</span><span class="sxs-lookup"><span data-stu-id="af9e0-201">hello minidump is associated with a snapshot with hello ID `139e411a23934dc0b9ea08a626db16c5`.</span></span> <span data-ttu-id="af9e0-202">Toto ID můžete použít novější hello toolocate přidružené telemetrie výjimek ve statistice analýza aplikace.</span><span class="sxs-lookup"><span data-stu-id="af9e0-202">You can use this ID later toolocate hello associated exception telemetry in Application Insights Analytics.</span></span>

<span data-ttu-id="af9e0-203">osoba Hello hledá nové soubory PDB o jednou za 15 minut.</span><span class="sxs-lookup"><span data-stu-id="af9e0-203">hello uploader scans for new PDBs about once every 15 minutes.</span></span> <span data-ttu-id="af9e0-204">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="af9e0-204">Here's an example:</span></span>

```
MinidumpUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\home\site\wwwroot\ for local PDBs.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\local\Temporary ASP.NET Files\root\a6554c94\e3ad6f22\assembly\dl3\81d5008b\00b93cc8_dec5d201 for local PDBs.
    DateTime=2017-05-25T15:11:38.8160276Z
MinidumpUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2017-05-25T15:11:38.8316450Z
MinidumpUploader.exe Information: 0 : Deleted PDB scan marker D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\.pdbscan.
    DateTime=2017-05-25T15:11:38.8316450Z
```

<span data-ttu-id="af9e0-205">Pro aplikace, které jsou _není_ hostované ve službě App Service, hello osoba protokoly jsou ve hello stejné složce jako minimálním výpisem hello: `%TEMP%\Dumps\<ikey>` (kde `<ikey>` je klíč instrumentace).</span><span class="sxs-lookup"><span data-stu-id="af9e0-205">For applications that are _not_ hosted in App Service, hello uploader logs are in hello same folder as hello minidumps: `%TEMP%\Dumps\<ikey>` (where `<ikey>` is your instrumentation key).</span></span>

### <a name="use-application-insights-search-toofind-exceptions-with-snapshots"></a><span data-ttu-id="af9e0-206">Vyhledávání pomocí Application Insights toofind výjimky se snímky</span><span class="sxs-lookup"><span data-stu-id="af9e0-206">Use Application Insights search toofind exceptions with snapshots</span></span>

<span data-ttu-id="af9e0-207">Při vytváření snímku se označí hello vyvolání výjimky s ID snímku.</span><span class="sxs-lookup"><span data-stu-id="af9e0-207">When a snapshot is created, hello throwing exception is tagged with a snapshot ID.</span></span> <span data-ttu-id="af9e0-208">Statistika hlášené tooApplication po telemetrie výjimek hello toto ID snímku je zahrnuta jako vlastní vlastnost.</span><span class="sxs-lookup"><span data-stu-id="af9e0-208">When hello exception telemetry is reported tooApplication Insights, that snapshot ID is included as a custom property.</span></span> <span data-ttu-id="af9e0-209">V okně hledání hello Application Insights, můžete najít všechny telemetrická s hello `ai.snapshot.id` vlastní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="af9e0-209">Using hello Search blade in Application Insights, you can find all telemetry with hello `ai.snapshot.id` custom property.</span></span>

1. <span data-ttu-id="af9e0-210">Vyhledejte prostředek Application Insights tooyour hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="af9e0-210">Browse tooyour Application Insights resource in hello Azure portal.</span></span>
2. <span data-ttu-id="af9e0-211">Klikněte na tlačítko **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="af9e0-211">Click **Search**.</span></span>
3. <span data-ttu-id="af9e0-212">Typ `ai.snapshot.id` v hello textového vyhledávacího pole a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="af9e0-212">Type `ai.snapshot.id` in hello Search text box and press Enter.</span></span>

![Vyhledejte telemetrie s ID snímku hello portálu](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

<span data-ttu-id="af9e0-214">V případě, že toto hledání nebyly vráceny žádné výsledky, žádné snímky byly hlášené tooApplication Insights pro aplikace v hello vybrané časové rozmezí.</span><span class="sxs-lookup"><span data-stu-id="af9e0-214">If this search returns no results, then no snapshots were reported tooApplication Insights for your application in hello selected time range.</span></span>

<span data-ttu-id="af9e0-215">toosearch pro konkrétní snímku ID z hello nástroje odeslání protokolů, zadejte toto ID hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="af9e0-215">toosearch for a specific snapshot ID from hello Uploader logs, type that ID in hello Search box.</span></span> <span data-ttu-id="af9e0-216">Pokud nemůžete najít telemetrii pro snímek, který víte, že byl odeslán, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="af9e0-216">If you can't find telemetry for a snapshot that you know was uploaded, follow these steps:</span></span>

1. <span data-ttu-id="af9e0-217">Zkontrolujte, že se díváte na pravém prostředek Application Insights hello kontrolou klíč instrumentace hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-217">Double-check that you're looking at hello right Application Insights resource by verifying hello instrumentation key.</span></span>

2. <span data-ttu-id="af9e0-218">Pomocí hello časové razítko z protokolu nástroje odeslání hello, upravte filtr časové rozmezí hello hello vyhledávání toocover tento časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="af9e0-218">Using hello timestamp from hello Uploader log, adjust hello Time Range filter of hello search toocover that time range.</span></span>

<span data-ttu-id="af9e0-219">Pokud stále nevidíte výjimku s tímto ID snímku, nebyl telemetrie výjimek hello hlášené tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="af9e0-219">If you still don't see an exception with that snapshot ID, then hello exception telemetry wasn't reported tooApplication Insights.</span></span> <span data-ttu-id="af9e0-220">Tato situace může nastat, pokud vaše aplikace došlo k chybě po trvalo hello snímku, ale před jeho hlášené telemetrie výjimek hello.</span><span class="sxs-lookup"><span data-stu-id="af9e0-220">This situation can happen if your application crashed after it took hello snapshot but before it reported hello exception telemetry.</span></span> <span data-ttu-id="af9e0-221">V takovém případě zkontrolujte protokoly služby App Service v části hello `Diagnose and solve problems` toosee, pokud došlo k neočekávané restartuje nebo neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="af9e0-221">In this case, check hello App Service logs under `Diagnose and solve problems` toosee if there were unexpected restarts or unhandled exceptions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af9e0-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af9e0-222">Next steps</span></span>

* <span data-ttu-id="af9e0-223">[V kódu nastavit snappoints](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) tooget snímky bez čekání na výjimku.</span><span class="sxs-lookup"><span data-stu-id="af9e0-223">[Set snappoints in your code](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) tooget snapshots without waiting for an exception.</span></span>
* <span data-ttu-id="af9e0-224">[Diagnostikovat výjimky ve webových aplikacích](app-insights-asp-net-exceptions.md) vysvětluje, jak toomake další výjimky viditelné tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="af9e0-224">[Diagnose exceptions in your web apps](app-insights-asp-net-exceptions.md) explains how toomake more exceptions visible tooApplication Insights.</span></span> 
* <span data-ttu-id="af9e0-225">[Inteligentní detekce](app-insights-proactive-diagnostics.md) automaticky vyhledá anomálie výkonu.</span><span class="sxs-lookup"><span data-stu-id="af9e0-225">[Smart Detection](app-insights-proactive-diagnostics.md) automatically discovers performance anomalies.</span></span>
