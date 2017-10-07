---
title: "aaaMonitoring využití a výkonu aplikací klasické pracovní plochy Windows"
description: "Analyzujte využití a výkon vaší desktopové aplikace Windows pomocí HockeyApp a Application Insights."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 19040746-3315-47e7-8c60-4b3000d2ddc4
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/26/2016
ms.author: bwren
ms.openlocfilehash: 73806885a6f0ed3896c0e43308c90ba087007887
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a><span data-ttu-id="e9b4d-103">Monitorování využití a výkonu desktopových aplikací pro Windows</span><span class="sxs-lookup"><span data-stu-id="e9b4d-103">Monitoring usage and performance in Windows Desktop apps</span></span>


<span data-ttu-id="e9b4d-104">[Azure Application Insights](app-insights-overview.md) a [HockeyApp](https://hockeyapp.net) umožňují monitorovat využití a výkon nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9b4d-104">[Azure Application Insights](app-insights-overview.md) and [HockeyApp](https://hockeyapp.net) let you monitor your deployed application for usage and performance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e9b4d-105">Doporučujeme, abyste [HockeyApp](https://hockeyapp.net) toodistribute a monitorování aplikací desktop a zařízení.</span><span class="sxs-lookup"><span data-stu-id="e9b4d-105">We recommend [HockeyApp](https://hockeyapp.net) toodistribute and monitor desktop and device apps.</span></span> <span data-ttu-id="e9b4d-106">S HockeyApp může spravovat distribuci, testování za provozu a zpětnou vazbu od uživatelů, a zároveň monitorovat sestavy využití a chyb.</span><span class="sxs-lookup"><span data-stu-id="e9b4d-106">With HockeyApp, you can manage distribution, live testing, and user feedback, as well as monitor usage and crash reports.</span></span> <span data-ttu-id="e9b4d-107">Můžete také [exportovat telemetrii a dávat na ni dotazy pomocí Analytics](app-insights-hockeyapp-bridge-app.md).</span><span class="sxs-lookup"><span data-stu-id="e9b4d-107">You can also [export and query your telemetry with Analytics](app-insights-hockeyapp-bridge-app.md).</span></span>
> 
> <span data-ttu-id="e9b4d-108">I když telemetrie odesláním tooApplication statistiky z aplikace pracovní plochy, jedná se hlavně užitečná pro účely ladění a experimentální.</span><span class="sxs-lookup"><span data-stu-id="e9b4d-108">Although telemetry can be sent tooApplication Insights from a desktop application, this is chiefly useful for debugging and experimental purposes.</span></span>
> 
> 

## <a name="toosend-telemetry-tooapplication-insights-from-a-windows-application"></a><span data-ttu-id="e9b4d-109">toosend telemetrie tooApplication statistiky z aplikace systému Windows</span><span class="sxs-lookup"><span data-stu-id="e9b4d-109">toosend telemetry tooApplication Insights from a Windows application</span></span>
1. <span data-ttu-id="e9b4d-110">V hello [portál Azure](https://portal.azure.com), [vytvořte prostředek Application Insights](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="e9b4d-110">In hello [Azure portal](https://portal.azure.com), [create an Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="e9b4d-111">Jako typ aplikace vyberte aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e9b4d-111">For application type, choose ASP.NET app.</span></span>
2. <span data-ttu-id="e9b4d-112">Trvat kopii hello klíč instrumentace.</span><span class="sxs-lookup"><span data-stu-id="e9b4d-112">Take a copy of hello Instrumentation Key.</span></span> <span data-ttu-id="e9b4d-113">Najít klíč hello v hello Essentials rozevíracího seznamu hello nového prostředku, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e9b4d-113">Find hello key in hello Essentials drop-down of hello new resource you just created.</span></span> 
3. <span data-ttu-id="e9b4d-114">V sadě Visual Studio upravte balíčky NuGet hello projektu aplikace a přidejte Microsoft.ApplicationInsights.WindowsServer.</span><span class="sxs-lookup"><span data-stu-id="e9b4d-114">In Visual Studio, edit hello NuGet packages of your app project, and add Microsoft.ApplicationInsights.WindowsServer.</span></span> <span data-ttu-id="e9b4d-115">(Nebo zvolte Microsoft.ApplicationInsights, pokud chcete úplné rozhraní API bez hello standardní telemetrie kolekce modulů hello.)</span><span class="sxs-lookup"><span data-stu-id="e9b4d-115">(Or choose Microsoft.ApplicationInsights if you just want hello bare API, without hello standard telemetry collection modules.)</span></span>
4. <span data-ttu-id="e9b4d-116">Nastavte klíč instrumentace hello buď ve vašem kódu:</span><span class="sxs-lookup"><span data-stu-id="e9b4d-116">Set hello instrumentation key either in your code:</span></span>
   
    <span data-ttu-id="e9b4d-117">`TelemetryConfiguration.Active.InstrumentationKey = "`*váš klíč*`";`</span><span class="sxs-lookup"><span data-stu-id="e9b4d-117">`TelemetryConfiguration.Active.InstrumentationKey = "` *your key* `";`</span></span> 
   
    <span data-ttu-id="e9b4d-118">nebo v souboru ApplicationInsights.config (Pokud jste nainstalovali jednu standardní telemetrie balíčků hello):</span><span class="sxs-lookup"><span data-stu-id="e9b4d-118">or in ApplicationInsights.config (if you installed one of hello standard telemetry packages):</span></span>
   
    <span data-ttu-id="e9b4d-119">`<InstrumentationKey>`*váš klíč*`</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="e9b4d-119">`<InstrumentationKey>`*your key*`</InstrumentationKey>`</span></span> 
   
    <span data-ttu-id="e9b4d-120">Pokud chcete použít soubor ApplicationInsights.config, ujistěte se, jeho vlastnosti v Průzkumníku řešení nastaveny příliš**akce sestavení = obsah, kopie tooOutput Directory = kopie**.</span><span class="sxs-lookup"><span data-stu-id="e9b4d-120">If you use ApplicationInsights.config, make sure its properties in Solution Explorer are set too**Build Action = Content, Copy tooOutput Directory = Copy**.</span></span>
5. <span data-ttu-id="e9b4d-121">[Pomocí rozhraní API hello](app-insights-api-custom-events-metrics.md) toosend telemetrie.</span><span class="sxs-lookup"><span data-stu-id="e9b4d-121">[Use hello API](app-insights-api-custom-events-metrics.md) toosend telemetry.</span></span>
6. <span data-ttu-id="e9b4d-122">Spuštění aplikace a v tématu hello telemetrie hello prostředků, kterou jste vytvořili v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e9b4d-122">Run your app, and see hello telemetry in hello resource you created in hello Azure Portal.</span></span>

## <span data-ttu-id="e9b4d-123"><a name="telemetry"></a>Příklad kódu</span><span class="sxs-lookup"><span data-stu-id="e9b4d-123"><a name="telemetry"></a>Example code</span></span>
```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative toosetting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a><span data-ttu-id="e9b4d-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9b4d-124">Next steps</span></span>
* [<span data-ttu-id="e9b4d-125">Vytvoření řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="e9b4d-125">Create a dashboard</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="e9b4d-126">Diagnostické vyhledávání</span><span class="sxs-lookup"><span data-stu-id="e9b4d-126">Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="e9b4d-127">Zkoumání metrik</span><span class="sxs-lookup"><span data-stu-id="e9b4d-127">Explore metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="e9b4d-128">Psaní analytických dotazů</span><span class="sxs-lookup"><span data-stu-id="e9b4d-128">Write Analytics queries</span></span>](app-insights-analytics.md)

