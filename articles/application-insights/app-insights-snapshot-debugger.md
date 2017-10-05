---
title: "Azure Application Insights snímku ladicí program pro aplikace .NET | Microsoft Docs"
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
ms.openlocfilehash: 56eba2ff7af228b3c44354ad43b384288b4e1972
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a><span data-ttu-id="345d0-103">Ladění snímků výjimky v aplikacích .NET</span><span class="sxs-lookup"><span data-stu-id="345d0-103">Debug snapshots on exceptions in .NET apps</span></span>

<span data-ttu-id="345d0-104">Když dojde k výjimce, může automaticky shromažďovat snímku ladění z provozu webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="345d0-104">When an exception occurs, you can automatically collect a debug snapshot from your live web application.</span></span> <span data-ttu-id="345d0-105">Snímek zobrazuje stav zdrojového kódu a proměnné v okamžiku, kdy byla výjimka vydána.</span><span class="sxs-lookup"><span data-stu-id="345d0-105">The snapshot shows the state of source code and variables at the moment the exception was thrown.</span></span> <span data-ttu-id="345d0-106">Snímek ladicí program (preview) v [Azure Application Insights](app-insights-overview.md) monitoruje výjimka telemetrie z vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="345d0-106">The Snapshot Debugger (preview) in [Azure Application Insights](app-insights-overview.md) monitors exception telemetry from your web app.</span></span> <span data-ttu-id="345d0-107">Shromažďuje snímky na vaše horní vyvolání výjimky, tak, aby informace, že potřebujete diagnostikovat problémy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="345d0-107">It collects snapshots on your top-throwing exceptions so that you have the information you need to diagnose issues in production.</span></span> <span data-ttu-id="345d0-108">Zahrnout [balíček NuGet kolekce snímku](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) ve vaší aplikaci a volitelně nakonfigurujte parametry kolekce v [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Snímky zobrazí na [výjimky](app-insights-asp-net-exceptions.md) na portálu služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="345d0-108">Include the [Snapshot collector NuGet package](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in your application, and optionally configure collection parameters in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Snapshots appear on [exceptions](app-insights-asp-net-exceptions.md) in the Application Insights portal.</span></span>

<span data-ttu-id="345d0-109">Ladění snímky můžete zobrazit na portálu najdete v části volání zásobníku a zkontrolovat proměnné v každé rámce zásobníku volání.</span><span class="sxs-lookup"><span data-stu-id="345d0-109">You can view debug snapshots in the portal to see the call stack and inspect variables at each call stack frame.</span></span> <span data-ttu-id="345d0-110">Pokud chcete získat výkonnější ladění zkušeností se zdrojovým kódem, otevřete snímky s Visual Studio Enterprise 2017 podle [stahování ladicí program snímku rozšíření pro Visual Studio](https://aka.ms/snapshotdebugger).</span><span class="sxs-lookup"><span data-stu-id="345d0-110">To get a more powerful debugging experience with source code, open snapshots with Visual Studio 2017 Enterprise by [downloading the Snapshot Debugger extension for Visual Studio](https://aka.ms/snapshotdebugger).</span></span>

<span data-ttu-id="345d0-111">Snímek kolekce je k dispozici pro:</span><span class="sxs-lookup"><span data-stu-id="345d0-111">Snapshot collection is available for:</span></span>
* <span data-ttu-id="345d0-112">Aplikace rozhraní .NET framework a ASP.NET spuštění rozhraní .NET Framework 4.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="345d0-112">.NET Framework and ASP.NET applications running .NET Framework 4.5 or later.</span></span>
* <span data-ttu-id="345d0-113">Aplikace rozhraní .NET 2.0 core a ASP.NET Core 2.0 v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="345d0-113">.NET Core 2.0 and ASP.NET Core 2.0 applications running on Windows.</span></span>

### <a name="configure-snapshot-collection-for-aspnet-applications"></a><span data-ttu-id="345d0-114">Konfigurace shromažďování snímků pro aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="345d0-114">Configure snapshot collection for ASP.NET applications</span></span>

1. <span data-ttu-id="345d0-115">[Povolit Application Insights ve vaší webové aplikaci](app-insights-asp-net.md), pokud jste ho ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="345d0-115">[Enable Application Insights in your web app](app-insights-asp-net.md), if you haven't done it yet.</span></span>

2. <span data-ttu-id="345d0-116">Zahrnout [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) balíček NuGet v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="345d0-116">Include the [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span> 

3. <span data-ttu-id="345d0-117">Zkontrolujte výchozí možnosti, které balíčku přidány do [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="345d0-117">Review the default options that the package added to [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span>

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in one minute. -->
        <SnapshotsPerMinuteLimit>2</SnapshotsPerMinuteLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. <span data-ttu-id="345d0-118">Snímky jsou shromažďovány pouze na výjimky, které jsou hlášeny Application insights.</span><span class="sxs-lookup"><span data-stu-id="345d0-118">Snapshots are collected only on exceptions that are reported to Application Insights.</span></span> <span data-ttu-id="345d0-119">V některých případech (například starší verze platformy .NET), možná budete muset [konfigurace shromažďování výjimek](app-insights-asp-net-exceptions.md#exceptions) pro zobrazení výjimek pomocí snímků na portálu.</span><span class="sxs-lookup"><span data-stu-id="345d0-119">In some cases (for example, older versions of the .NET platform), you might need to [configure exception collection](app-insights-asp-net-exceptions.md#exceptions) to see exceptions with snapshots in the portal.</span></span>


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a><span data-ttu-id="345d0-120">Konfigurace shromažďování snímků pro technologii ASP.NET 2.0 základní aplikace</span><span class="sxs-lookup"><span data-stu-id="345d0-120">Configure snapshot collection for ASP.NET Core 2.0 applications</span></span>

1. <span data-ttu-id="345d0-121">[Povolit Application Insights ve vaší webové aplikaci ASP.NET Core](app-insights-asp-net-core.md), pokud jste ho ještě neučinili.</span><span class="sxs-lookup"><span data-stu-id="345d0-121">[Enable Application Insights in your ASP.NET Core web app](app-insights-asp-net-core.md), if you haven't done it yet.</span></span>

2. <span data-ttu-id="345d0-122">Zahrnout [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) balíček NuGet v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="345d0-122">Include the [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span>

3. <span data-ttu-id="345d0-123">Změnit `ConfigureServices` metoda ve vaší aplikaci `Startup` třída přidat snímek kolekce telemetrie procesoru.</span><span class="sxs-lookup"><span data-stu-id="345d0-123">Modify the `ConfigureServices` method in your application's `Startup` class to add the Snapshot Collector's telemetry processor.</span></span> <span data-ttu-id="345d0-124">Kód, který byste měli přidat závisí na odkazované verze balíčku Microsoft.ApplicationInsights.ASPNETCore NuGet.</span><span class="sxs-lookup"><span data-stu-id="345d0-124">The code you should add depends on the referenced version of the Microsoft.ApplicationInsights.ASPNETCore NuGet package.</span></span>

   <span data-ttu-id="345d0-125">Pro Microsoft.ApplicationInsights.AspNetCore 2.1.0 přidejte:</span><span class="sxs-lookup"><span data-stu-id="345d0-125">For Microsoft.ApplicationInsights.AspNetCore 2.1.0 add:</span></span>
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       // This method is called by the runtime. Use it to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
           services.AddSingleton<Func<ITelemetryProcessor, ITelemetryProcessor>>(next => new SnapshotCollectorTelemetryProcessor(next));
           // TODO: Add any other services your application needs here.
       }
   }
   ```

   <span data-ttu-id="345d0-126">Pro Microsoft.ApplicationInsights.AspNetCore 2.1.1 přidejte:</span><span class="sxs-lookup"><span data-stu-id="345d0-126">For Microsoft.ApplicationInsights.AspNetCore 2.1.1 add:</span></span>
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

       // This method is called by the runtime. Use it to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
            services.AddSingleton<ITelemetryProcessorFactory>(new SnapshotCollectorTelemetryProcessorFactory());
           // TODO: Add any other services your application needs here.
       }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a><span data-ttu-id="345d0-127">Konfigurace shromažďování snímků pro jiné aplikace rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="345d0-127">Configure snapshot collection for other .NET applications</span></span>

1. <span data-ttu-id="345d0-128">Pokud vaše aplikace není instrumentována již s nástrojem Application Insights, začít [povolení Application Insights a nastavení klíč instrumentace](app-insights-windows-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="345d0-128">If your application is not already instrumented with Application Insights, get started by [enabling Application Insights and setting the instrumentation key](app-insights-windows-desktop.md).</span></span>

2. <span data-ttu-id="345d0-129">Přidat [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) balíček NuGet v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="345d0-129">Add the [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span>

3. <span data-ttu-id="345d0-130">Snímky jsou shromažďovány pouze na výjimky, které jsou hlášeny Application insights.</span><span class="sxs-lookup"><span data-stu-id="345d0-130">Snapshots are collected only on exceptions that are reported to Application Insights.</span></span> <span data-ttu-id="345d0-131">Potřebujete upravit kód k jejich sestavy.</span><span class="sxs-lookup"><span data-stu-id="345d0-131">You may need to modify your code to report them.</span></span> <span data-ttu-id="345d0-132">Kód zpracování výjimek závisí na struktuře vaší aplikace, ale zde je příklad:</span><span class="sxs-lookup"><span data-stu-id="345d0-132">The exception handling code depends on the structure of your application, but an example is below:</span></span>
    ```C#
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a><span data-ttu-id="345d0-133">Udělení oprávnění</span><span class="sxs-lookup"><span data-stu-id="345d0-133">Grant permissions</span></span>

<span data-ttu-id="345d0-134">Vlastníky předplatného Azure můžete prohlédnout snímky.</span><span class="sxs-lookup"><span data-stu-id="345d0-134">Owners of the Azure subscription can inspect snapshots.</span></span> <span data-ttu-id="345d0-135">Ostatní uživatelé musí udělit oprávnění vlastníka.</span><span class="sxs-lookup"><span data-stu-id="345d0-135">Other users must be granted permission by an owner.</span></span>

<span data-ttu-id="345d0-136">Udělit oprávnění, přiřaďte mu `Application Insights Snapshot Debugger` role pro uživatele, kteří bude kontrolovat snímky.</span><span class="sxs-lookup"><span data-stu-id="345d0-136">To grant permission, assign the `Application Insights Snapshot Debugger` role to users who will inspect snapshots.</span></span> <span data-ttu-id="345d0-137">Tato role může být přiřazena na jednotlivé uživatele nebo skupiny s vlastníky předplatného pro cílový prostředek Application Insights nebo jeho skupinu prostředků nebo předplatného.</span><span class="sxs-lookup"><span data-stu-id="345d0-137">This role can be assigned to individual users or groups by subscription owners for the target Application Insights resource or its resource group or subscription.</span></span>

1. <span data-ttu-id="345d0-138">Otevřete okno řízení přístupu (IAM).</span><span class="sxs-lookup"><span data-stu-id="345d0-138">Open the Access Control (IAM) blade.</span></span>
1. <span data-ttu-id="345d0-139">Klikněte tlačítko Přidat +.</span><span class="sxs-lookup"><span data-stu-id="345d0-139">Click the +Add button.</span></span>
1. <span data-ttu-id="345d0-140">Vyberte z rozevíracího seznamu rolí Application Insights snímku ladicí program.</span><span class="sxs-lookup"><span data-stu-id="345d0-140">Select Application Insights Snapshot Debugger from the Roles drop-down list.</span></span>
1. <span data-ttu-id="345d0-141">Vyhledejte a zadejte název pro uživatele, přidání.</span><span class="sxs-lookup"><span data-stu-id="345d0-141">Search for and enter a name for the user to add.</span></span>
1. <span data-ttu-id="345d0-142">Klikněte na tlačítko Uložit přidejte uživatele k roli.</span><span class="sxs-lookup"><span data-stu-id="345d0-142">Click the Save button to add the user to the role.</span></span>


[!IMPORTANT]
    <span data-ttu-id="345d0-143">Snímky mohou obsahovat osobní a dalších citlivých informací v proměnné a parametr hodnoty.</span><span class="sxs-lookup"><span data-stu-id="345d0-143">Snapshots can potentially contain personal and other sensitive information in variable and parameter values.</span></span>

## <a name="debug-snapshots-in-the-application-insights-portal"></a><span data-ttu-id="345d0-144">Ladění snímky v portálu služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="345d0-144">Debug snapshots in the Application Insights portal</span></span>

<span data-ttu-id="345d0-145">Pokud snímek je k dispozici pro danou výjimky nebo ID problému, **otevřít ladění snímek** na se zobrazí tlačítko [výjimka](app-insights-asp-net-exceptions.md) na portálu služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="345d0-145">If a snapshot is available for a given exception or a problem ID, an **Open Debug Snapshot** button appears on the [exception](app-insights-asp-net-exceptions.md) in the Application Insights portal.</span></span>

![Tlačítko Otevřít ladění snímku na výjimky](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

<span data-ttu-id="345d0-147">V zobrazení ladění snímků zobrazí zásobník volání a podokně proměnné.</span><span class="sxs-lookup"><span data-stu-id="345d0-147">In the Debug Snapshot view, you see a call stack and a variables pane.</span></span> <span data-ttu-id="345d0-148">Když vyberete v podokně Zásobník volání rámce zásobníku volání, můžete zobrazit místní proměnné a parametry pro tuto funkci volání v podokně proměnné.</span><span class="sxs-lookup"><span data-stu-id="345d0-148">When you select frames of the call stack in the call stack pane, you can view local variables and parameters for that function call in the variables pane.</span></span>

![Zobrazení ladění snímku na portálu](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

<span data-ttu-id="345d0-150">Snímky mohou obsahovat citlivé informace a ve výchozím nastavení nejsou viditelná.</span><span class="sxs-lookup"><span data-stu-id="345d0-150">Snapshots might contain sensitive information, and by default they are not viewable.</span></span> <span data-ttu-id="345d0-151">Chcete-li zobrazit snímky, musíte mít `Application Insights Snapshot Debugger` přiřazená role.</span><span class="sxs-lookup"><span data-stu-id="345d0-151">To view snapshots, you must have the `Application Insights Snapshot Debugger` role assigned to you.</span></span>

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a><span data-ttu-id="345d0-152">Ladění snímky s Visual Studio 2017 Enterprise</span><span class="sxs-lookup"><span data-stu-id="345d0-152">Debug snapshots with Visual Studio 2017 Enterprise</span></span>
1. <span data-ttu-id="345d0-153">Klikněte na tlačítko **stáhnout snímku** tlačítko Stáhnout `.diagsession` souboru, který lze otevřít v aplikaci Visual Studio Enterprise 2017.</span><span class="sxs-lookup"><span data-stu-id="345d0-153">Click the **Download Snapshot** button to download a `.diagsession` file, which can be opened by Visual Studio 2017 Enterprise.</span></span> 

2. <span data-ttu-id="345d0-154">Chcete-li otevřít `.diagsession` souboru, je nutné nejprve [stáhnout a nainstalovat rozšíření ladicí program snímku pro sadu Visual Studio](https://aka.ms/snapshotdebugger).</span><span class="sxs-lookup"><span data-stu-id="345d0-154">To open the `.diagsession` file, you must first [download and install the Snapshot Debugger extension for Visual Studio](https://aka.ms/snapshotdebugger).</span></span>

3. <span data-ttu-id="345d0-155">Po otevření souboru snímku, zobrazí se stránka minimální výpis ladění v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="345d0-155">After you open the snapshot file, the Minidump Debugging page in Visual Studio appears.</span></span> <span data-ttu-id="345d0-156">Klikněte na tlačítko **ladění spravovaného kódu** spustit ladění snímku.</span><span class="sxs-lookup"><span data-stu-id="345d0-156">Click **Debug Managed Code** to start debugging the snapshot.</span></span> <span data-ttu-id="345d0-157">Snímek se otevře na řádek kódu, kde byla výjimka vydána tak, aby můžete ladit, aktuální stav procesu.</span><span class="sxs-lookup"><span data-stu-id="345d0-157">The snapshot opens to the line of code where the exception was thrown so that you can debug the current state of the process.</span></span>

    ![Zobrazení ladění snímku v sadě Visual Studio](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

<span data-ttu-id="345d0-159">Stažený snímku obsahuje symbol soubory, které nebyly nalezeny na vašem webovém serveru aplikace.</span><span class="sxs-lookup"><span data-stu-id="345d0-159">The downloaded snapshot contains any symbol files that were found on your web application server.</span></span> <span data-ttu-id="345d0-160">Tyto soubory symbolů nutné přidružit data snímku se zdrojovým kódem.</span><span class="sxs-lookup"><span data-stu-id="345d0-160">These symbol files are required to associate snapshot data with source code.</span></span> <span data-ttu-id="345d0-161">Aplikace služby App Service je nutné povolit nasazení symbol při publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="345d0-161">For App Service apps, make sure to enable symbol deployment when you publish your web apps.</span></span>

## <a name="how-snapshots-work"></a><span data-ttu-id="345d0-162">Jak fungují snímky</span><span class="sxs-lookup"><span data-stu-id="345d0-162">How snapshots work</span></span>

<span data-ttu-id="345d0-163">Když se aplikace spustí, proces osoba samostatné snímku je vytvořen, který monitoruje vaše aplikace pro požadavky na snímku.</span><span class="sxs-lookup"><span data-stu-id="345d0-163">When your application starts, a separate snapshot uploader process is created that monitors your application for snapshot requests.</span></span> <span data-ttu-id="345d0-164">Pokud se požaduje snímku, stínové kopie běžící proces se provádí v asi 10 až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="345d0-164">When a snapshot is requested, a shadow copy of the running process is made in about 10 to 20 minutes.</span></span> <span data-ttu-id="345d0-165">Proces stínové pak analýzy a snímku se vytvoří během procesu hlavní nadále provozuje a poskytovat provozu pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="345d0-165">The shadow process is then analyzed, and a snapshot is created while the main process continues to run and serve traffic to users.</span></span> <span data-ttu-id="345d0-166">Potom nahrání snímku do služby Application Insights společně s všechny relevantní symbolu (.pdb) soubory, které jsou potřebné k zobrazení snímku.</span><span class="sxs-lookup"><span data-stu-id="345d0-166">The snapshot is then uploaded to Application Insights along with any relevant symbol (.pdb) files that are needed to view the snapshot.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="345d0-167">Aktuální omezení</span><span class="sxs-lookup"><span data-stu-id="345d0-167">Current limitations</span></span>

### <a name="publish-symbols"></a><span data-ttu-id="345d0-168">Publikování symboly</span><span class="sxs-lookup"><span data-stu-id="345d0-168">Publish symbols</span></span>
<span data-ttu-id="345d0-169">Snímek ladicí program vyžaduje soubory symbolů v provozním serveru k dekódování proměnné a pro poskytování zkušenosti s laděním v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="345d0-169">The Snapshot Debugger requires symbol files on the production server to decode variables and to provide a debugging experience in Visual Studio.</span></span> <span data-ttu-id="345d0-170">Symboly pro verzi sestavení 15.2 verzi Visual Studio 2017 publikuje ve výchozím nastavení, pokud tato možnost publikuje do služby App Service.</span><span class="sxs-lookup"><span data-stu-id="345d0-170">The 15.2 release of Visual Studio 2017 publishes symbols for release builds by default when it publishes to App Service.</span></span> <span data-ttu-id="345d0-171">V předchozích verzích, je nutné přidat následující řádek na svůj profil publikování `.pubxml` tak, aby symboly jsou publikovány v režimu vydání:</span><span class="sxs-lookup"><span data-stu-id="345d0-171">In prior versions, you need to add the following line to your publish profile `.pubxml` file so that symbols are published in release mode:</span></span>

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

<span data-ttu-id="345d0-172">Pro Azure Compute a dalších typů, zajistěte, aby soubory symbolů byly ve stejné složce dll hlavní aplikace (obvykle `wwwroot/bin`) nebo jsou k dispozici v aktuální cestě.</span><span class="sxs-lookup"><span data-stu-id="345d0-172">For Azure Compute and other types, ensure that the symbol files are in the same folder of the main application .dll (typically, `wwwroot/bin`) or are available on the current path.</span></span>

### <a name="optimized-builds"></a><span data-ttu-id="345d0-173">Optimalizované sestavení</span><span class="sxs-lookup"><span data-stu-id="345d0-173">Optimized builds</span></span>
<span data-ttu-id="345d0-174">V některých případech místní proměnné nelze zobrazit, v sestavení pro vydání z důvodu optimalizace, které se použijí během procesu vytváření.</span><span class="sxs-lookup"><span data-stu-id="345d0-174">In some cases, local variables cannot be viewed in release builds because of optimizations that are applied during the build process.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="345d0-175">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="345d0-175">Troubleshooting</span></span>

<span data-ttu-id="345d0-176">Tyto tipy pomáhají při řešení problémů s ladicím programem snímku.</span><span class="sxs-lookup"><span data-stu-id="345d0-176">These tips help you troubleshoot problems with the Snapshot Debugger.</span></span>

### <a name="verify-the-instrumentation-key"></a><span data-ttu-id="345d0-177">Ověřte klíč instrumentace</span><span class="sxs-lookup"><span data-stu-id="345d0-177">Verify the instrumentation key</span></span>

<span data-ttu-id="345d0-178">Ujistěte se, že používáte klíč instrumentace správné v k publikované aplikaci.</span><span class="sxs-lookup"><span data-stu-id="345d0-178">Make sure that you're using the correct instrumentation key in your published application.</span></span> <span data-ttu-id="345d0-179">Application Insights obvykle čte klíč instrumentace z soubor ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="345d0-179">Usually, Application Insights reads the instrumentation key from the ApplicationInsights.config file.</span></span> <span data-ttu-id="345d0-180">Ověřte, že hodnota je stejná jako klíč instrumentace pro prostředek Application Insights, který se zobrazí na portálu.</span><span class="sxs-lookup"><span data-stu-id="345d0-180">Verify that the value is the same as the instrumentation key for the Application Insights resource that you see in the portal.</span></span>

### <a name="check-the-uploader-logs"></a><span data-ttu-id="345d0-181">Zkontrolujte protokoly osoba</span><span class="sxs-lookup"><span data-stu-id="345d0-181">Check the uploader logs</span></span>

<span data-ttu-id="345d0-182">Po vytvoření snímku se vytvoří soubor s minimálním (.dmp) na disku.</span><span class="sxs-lookup"><span data-stu-id="345d0-182">After a snapshot is created, a minidump file (.dmp) is created on disk.</span></span> <span data-ttu-id="345d0-183">Proces samostatné osoba trvá tento soubor minimální výpis a odesílá, společně s všechny přidružené soubory PDB do úložiště Application Insights snímku ladicí program.</span><span class="sxs-lookup"><span data-stu-id="345d0-183">A separate uploader process takes that minidump file and uploads it, along with any associated PDBs, to Application Insights Snapshot Debugger storage.</span></span> <span data-ttu-id="345d0-184">Po úspěšném odeslání minimální výpis je odstraněn z disku.</span><span class="sxs-lookup"><span data-stu-id="345d0-184">After the minidump has uploaded successfully, it is deleted from disk.</span></span> <span data-ttu-id="345d0-185">Na disku zůstanou zachovány soubory protokolu pro minimální výpis osoba.</span><span class="sxs-lookup"><span data-stu-id="345d0-185">The log files for the minidump uploader are retained on disk.</span></span> <span data-ttu-id="345d0-186">V prostředí služby App Service, můžete najít tyto protokoly v `D:\Home\LogFiles\Uploader_*.log`.</span><span class="sxs-lookup"><span data-stu-id="345d0-186">In an App Service environment, you can find these logs in `D:\Home\LogFiles\Uploader_*.log`.</span></span> <span data-ttu-id="345d0-187">Použití serveru správy Kudu pro službu App Service k nalezení tyto soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="345d0-187">Use the Kudu management site for App Service to find these log files.</span></span>

1. <span data-ttu-id="345d0-188">Otevřete aplikaci aplikační služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="345d0-188">Open your App Service application in the Azure portal.</span></span>

2. <span data-ttu-id="345d0-189">Vyberte **Rozšířené nástroje** okno, nebo vyhledejte **Kudu**.</span><span class="sxs-lookup"><span data-stu-id="345d0-189">Select the **Advanced Tools** blade, or search for **Kudu**.</span></span>
3. <span data-ttu-id="345d0-190">Klikněte na tlačítko **přejděte**.</span><span class="sxs-lookup"><span data-stu-id="345d0-190">Click **Go**.</span></span>
4. <span data-ttu-id="345d0-191">V **konzolou pro ladění** rozevíracím seznamu vyberte **CMD**.</span><span class="sxs-lookup"><span data-stu-id="345d0-191">In the **Debug console** drop-down list box, select **CMD**.</span></span>
5. <span data-ttu-id="345d0-192">Klikněte na tlačítko **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="345d0-192">Click **LogFiles**.</span></span>

<span data-ttu-id="345d0-193">Měli byste vidět alespoň jeden soubor s názvem, který začíná `Uploader_` a `.log` rozšíření.</span><span class="sxs-lookup"><span data-stu-id="345d0-193">You should see at least one file with a name that begins with `Uploader_` and a `.log` extension.</span></span> <span data-ttu-id="345d0-194">Klikněte na příslušnou ikonu Stáhnout všechny soubory protokolů nebo je otevřít v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="345d0-194">Click the appropriate icon to download any log files or open them in a browser.</span></span>
<span data-ttu-id="345d0-195">Název souboru obsahuje název počítače.</span><span class="sxs-lookup"><span data-stu-id="345d0-195">The file name includes the machine name.</span></span> <span data-ttu-id="345d0-196">Pokud vaše instance služby App Service je hostitelem více než jeden počítač, nejsou samostatné soubory protokolu pro každý počítač.</span><span class="sxs-lookup"><span data-stu-id="345d0-196">If your App Service instance is hosted on more than one machine, there are separate log files for each machine.</span></span> <span data-ttu-id="345d0-197">Když osoba zjistí nový soubor s minimálními výpisy, zaznamenává se v souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="345d0-197">When the uploader detects a new minidump file, it is recorded in the log file.</span></span> <span data-ttu-id="345d0-198">Tady je příklad úspěšné odeslání:</span><span class="sxs-lookup"><span data-stu-id="345d0-198">Here's an example of a successful upload:</span></span>

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

<span data-ttu-id="345d0-199">V předchozím příkladu je klíč instrumentace `c12a605e73c44346a984e00000000000`.</span><span class="sxs-lookup"><span data-stu-id="345d0-199">In the previous example, the instrumentation key is `c12a605e73c44346a984e00000000000`.</span></span> <span data-ttu-id="345d0-200">Tato hodnota musí odpovídat klíč instrumentace pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="345d0-200">This value should match the instrumentation key for your application.</span></span>
<span data-ttu-id="345d0-201">Minimální výpis souvisí s snímku s ID `139e411a23934dc0b9ea08a626db16c5`.</span><span class="sxs-lookup"><span data-stu-id="345d0-201">The minidump is associated with a snapshot with the ID `139e411a23934dc0b9ea08a626db16c5`.</span></span> <span data-ttu-id="345d0-202">Toto ID můžete použít později k vyhledání telemetrie přidružené výjimek ve Application Insights Analytics.</span><span class="sxs-lookup"><span data-stu-id="345d0-202">You can use this ID later to locate the associated exception telemetry in Application Insights Analytics.</span></span>

<span data-ttu-id="345d0-203">Procesu pro načtení hledá nové soubory PDB o jednou za 15 minut.</span><span class="sxs-lookup"><span data-stu-id="345d0-203">The uploader scans for new PDBs about once every 15 minutes.</span></span> <span data-ttu-id="345d0-204">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="345d0-204">Here's an example:</span></span>

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

<span data-ttu-id="345d0-205">Pro aplikace, které jsou _není_ hostované ve službě App Service, protokoly osoba jsou ve stejné složce jako minimálním výpisem: `%TEMP%\Dumps\<ikey>` (kde `<ikey>` je klíč instrumentace).</span><span class="sxs-lookup"><span data-stu-id="345d0-205">For applications that are _not_ hosted in App Service, the uploader logs are in the same folder as the minidumps: `%TEMP%\Dumps\<ikey>` (where `<ikey>` is your instrumentation key).</span></span>

### <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a><span data-ttu-id="345d0-206">Výjimky se snímky hledat pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="345d0-206">Use Application Insights search to find exceptions with snapshots</span></span>

<span data-ttu-id="345d0-207">Když je snímek vytvořen, aktivační výjimky se označí s ID snímku.</span><span class="sxs-lookup"><span data-stu-id="345d0-207">When a snapshot is created, the throwing exception is tagged with a snapshot ID.</span></span> <span data-ttu-id="345d0-208">Kdy je telemetrie výjimek hlášena Application insights, zda jsou zahrnuty jako vlastní vlastnost ID snímku.</span><span class="sxs-lookup"><span data-stu-id="345d0-208">When the exception telemetry is reported to Application Insights, that snapshot ID is included as a custom property.</span></span> <span data-ttu-id="345d0-209">V okně hledání Application Insights, můžete najít všechny telemetrická s `ai.snapshot.id` vlastní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="345d0-209">Using the Search blade in Application Insights, you can find all telemetry with the `ai.snapshot.id` custom property.</span></span>

1. <span data-ttu-id="345d0-210">Procházejte do zdroje Application Insights na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="345d0-210">Browse to your Application Insights resource in the Azure portal.</span></span>
2. <span data-ttu-id="345d0-211">Klikněte na tlačítko **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="345d0-211">Click **Search**.</span></span>
3. <span data-ttu-id="345d0-212">Typ `ai.snapshot.id` textového vyhledávacího pole a stiskněte Enter.</span><span class="sxs-lookup"><span data-stu-id="345d0-212">Type `ai.snapshot.id` in the Search text box and press Enter.</span></span>

![Vyhledejte telemetrie s ID snímku na portálu](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

<span data-ttu-id="345d0-214">Pokud toto hledání nebyly vráceny žádné výsledky, pak žádné snímky byly hlášené Application insights pro aplikace v vybraný časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="345d0-214">If this search returns no results, then no snapshots were reported to Application Insights for your application in the selected time range.</span></span>

<span data-ttu-id="345d0-215">K vyhledání ID konkrétní snímek z nástroje odeslání protokolů, zadejte toto ID do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="345d0-215">To search for a specific snapshot ID from the Uploader logs, type that ID in the Search box.</span></span> <span data-ttu-id="345d0-216">Pokud nemůžete najít telemetrii pro snímek, který víte, že byl odeslán, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="345d0-216">If you can't find telemetry for a snapshot that you know was uploaded, follow these steps:</span></span>

1. <span data-ttu-id="345d0-217">Zkontrolujte, že se díváte na pravém prostředku Application Insights kontrolou klíč instrumentace.</span><span class="sxs-lookup"><span data-stu-id="345d0-217">Double-check that you're looking at the right Application Insights resource by verifying the instrumentation key.</span></span>

2. <span data-ttu-id="345d0-218">Pomocí časové razítko z procesu pro načtení protokolu, upravte filtr časový rozsah hledání tak, aby pokrývalo tento časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="345d0-218">Using the timestamp from the Uploader log, adjust the Time Range filter of the search to cover that time range.</span></span>

<span data-ttu-id="345d0-219">Pokud stále nevidíte výjimku s tímto ID snímku, telemetrie výjimek nebyla hlášena do Application Insights.</span><span class="sxs-lookup"><span data-stu-id="345d0-219">If you still don't see an exception with that snapshot ID, then the exception telemetry wasn't reported to Application Insights.</span></span> <span data-ttu-id="345d0-220">Tato situace může nastat, pokud vaše aplikace došlo k chybě po trvalo snímku, ale předtím, než ho hlášené telemetrie výjimek.</span><span class="sxs-lookup"><span data-stu-id="345d0-220">This situation can happen if your application crashed after it took the snapshot but before it reported the exception telemetry.</span></span> <span data-ttu-id="345d0-221">V takovém případě zkontrolujte protokoly služby App Service v části `Diagnose and solve problems` chcete zobrazit, pokud byly neočekávané restartování nebo neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="345d0-221">In this case, check the App Service logs under `Diagnose and solve problems` to see if there were unexpected restarts or unhandled exceptions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="345d0-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="345d0-222">Next steps</span></span>

* <span data-ttu-id="345d0-223">[Nastavit snappoints ve vašem kódu](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) získat snímky bez čekání na výjimku.</span><span class="sxs-lookup"><span data-stu-id="345d0-223">[Set snappoints in your code](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) to get snapshots without waiting for an exception.</span></span>
* <span data-ttu-id="345d0-224">[Diagnostikovat výjimky ve webových aplikacích](app-insights-asp-net-exceptions.md) vysvětluje, jak chcete zviditelnit více výjimek pro Application Insights.</span><span class="sxs-lookup"><span data-stu-id="345d0-224">[Diagnose exceptions in your web apps](app-insights-asp-net-exceptions.md) explains how to make more exceptions visible to Application Insights.</span></span> 
* <span data-ttu-id="345d0-225">[Inteligentní detekce](app-insights-proactive-diagnostics.md) automaticky vyhledá anomálie výkonu.</span><span class="sxs-lookup"><span data-stu-id="345d0-225">[Smart Detection](app-insights-proactive-diagnostics.md) automatically discovers performance anomalies.</span></span>
