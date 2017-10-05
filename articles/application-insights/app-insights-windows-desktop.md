---
title: "Monitorování využití a výkonu desktopových aplikací pro Windows"
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
ms.openlocfilehash: 9d7e2a390adf10cbf5d88dd0084ce09136987309
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a><span data-ttu-id="60846-103">Monitorování využití a výkonu desktopových aplikací pro Windows</span><span class="sxs-lookup"><span data-stu-id="60846-103">Monitoring usage and performance in Windows Desktop apps</span></span>


<span data-ttu-id="60846-104">[Azure Application Insights](app-insights-overview.md) a [HockeyApp](https://hockeyapp.net) umožňují monitorovat využití a výkon nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="60846-104">[Azure Application Insights](app-insights-overview.md) and [HockeyApp](https://hockeyapp.net) let you monitor your deployed application for usage and performance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="60846-105">Doporučujeme, abyste k distribuci a monitorování desktopových aplikací a aplikací zařízení používali [HockeyApp](https://hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="60846-105">We recommend [HockeyApp](https://hockeyapp.net) to distribute and monitor desktop and device apps.</span></span> <span data-ttu-id="60846-106">S HockeyApp může spravovat distribuci, testování za provozu a zpětnou vazbu od uživatelů, a zároveň monitorovat sestavy využití a chyb.</span><span class="sxs-lookup"><span data-stu-id="60846-106">With HockeyApp, you can manage distribution, live testing, and user feedback, as well as monitor usage and crash reports.</span></span> <span data-ttu-id="60846-107">Můžete také [exportovat telemetrii a dávat na ni dotazy pomocí Analytics](app-insights-hockeyapp-bridge-app.md).</span><span class="sxs-lookup"><span data-stu-id="60846-107">You can also [export and query your telemetry with Analytics](app-insights-hockeyapp-bridge-app.md).</span></span>
> 
> <span data-ttu-id="60846-108">I když telemetrii lze odeslat do Application Insights z desktopové aplikace, hodí se hlavně pro ladění a experimentální účely.</span><span class="sxs-lookup"><span data-stu-id="60846-108">Although telemetry can be sent to Application Insights from a desktop application, this is chiefly useful for debugging and experimental purposes.</span></span>
> 
> 

## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a><span data-ttu-id="60846-109">Odeslání telemetrie do Application Insights z aplikace Windows</span><span class="sxs-lookup"><span data-stu-id="60846-109">To send telemetry to Application Insights from a Windows application</span></span>
1. <span data-ttu-id="60846-110">Na webu [Azure Portal](https://portal.azure.com) [vytvořte prostředek Application Insights](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="60846-110">In the [Azure portal](https://portal.azure.com), [create an Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="60846-111">Jako typ aplikace vyberte aplikaci ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="60846-111">For application type, choose ASP.NET app.</span></span>
2. <span data-ttu-id="60846-112">Zkopírujte klíč instrumentace.</span><span class="sxs-lookup"><span data-stu-id="60846-112">Take a copy of the Instrumentation Key.</span></span> <span data-ttu-id="60846-113">Klíč najdete v rozevírací nabídce Základy nového prostředku, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="60846-113">Find the key in the Essentials drop-down of the new resource you just created.</span></span> 
3. <span data-ttu-id="60846-114">V sadě Visual Studio upravte balíčky NuGet projektu aplikace a přidejte Microsoft.ApplicationInsights.WindowsServer.</span><span class="sxs-lookup"><span data-stu-id="60846-114">In Visual Studio, edit the NuGet packages of your app project, and add Microsoft.ApplicationInsights.WindowsServer.</span></span> <span data-ttu-id="60846-115">(Nebo zvolte Microsoft.ApplicationInsights, pokud chcete prosté rozhraní API bez standardních modulů kolekce telemetrie.)</span><span class="sxs-lookup"><span data-stu-id="60846-115">(Or choose Microsoft.ApplicationInsights if you just want the bare API, without the standard telemetry collection modules.)</span></span>
4. <span data-ttu-id="60846-116">Nastavte klíč instrumentace, buď ve vašem kódu:</span><span class="sxs-lookup"><span data-stu-id="60846-116">Set the instrumentation key either in your code:</span></span>
   
    <span data-ttu-id="60846-117">`TelemetryConfiguration.Active.InstrumentationKey = "` *váš klíč* `";`</span><span class="sxs-lookup"><span data-stu-id="60846-117">`TelemetryConfiguration.Active.InstrumentationKey = "` *your key* `";`</span></span> 
   
    <span data-ttu-id="60846-118">nebo v souboru ApplicationInsights.config (pokud jste nainstalovali jeden ze standardních balíčků telemetrie):</span><span class="sxs-lookup"><span data-stu-id="60846-118">or in ApplicationInsights.config (if you installed one of the standard telemetry packages):</span></span>
   
    <span data-ttu-id="60846-119">`<InstrumentationKey>`*váš klíč*`</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="60846-119">`<InstrumentationKey>`*your key*`</InstrumentationKey>`</span></span> 
   
    <span data-ttu-id="60846-120">Pokud používáte soubor ApplicationInsights.config, ujistěte se, že jsou jeho vlastnosti v Průzkumníku řešení nastavené na: **Build Action = Content, Copy to Output Directory = Copy**.</span><span class="sxs-lookup"><span data-stu-id="60846-120">If you use ApplicationInsights.config, make sure its properties in Solution Explorer are set to **Build Action = Content, Copy to Output Directory = Copy**.</span></span>
5. <span data-ttu-id="60846-121">[Použijte rozhraní API](app-insights-api-custom-events-metrics.md) k odesílání telemetrie.</span><span class="sxs-lookup"><span data-stu-id="60846-121">[Use the API](app-insights-api-custom-events-metrics.md) to send telemetry.</span></span>
6. <span data-ttu-id="60846-122">Spusťte aplikaci a zobrazte telemetrii v prostředku, který jste vytvořili na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="60846-122">Run your app, and see the telemetry in the resource you created in the Azure Portal.</span></span>

## <span data-ttu-id="60846-123"><a name="telemetry"></a>Příklad kódu</span><span class="sxs-lookup"><span data-stu-id="60846-123"><a name="telemetry"></a>Example code</span></span>
```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
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

## <a name="next-steps"></a><span data-ttu-id="60846-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60846-124">Next steps</span></span>
* [<span data-ttu-id="60846-125">Vytvoření řídicího panelu</span><span class="sxs-lookup"><span data-stu-id="60846-125">Create a dashboard</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="60846-126">Diagnostické vyhledávání</span><span class="sxs-lookup"><span data-stu-id="60846-126">Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="60846-127">Zkoumání metrik</span><span class="sxs-lookup"><span data-stu-id="60846-127">Explore metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="60846-128">Psaní analytických dotazů</span><span class="sxs-lookup"><span data-stu-id="60846-128">Write Analytics queries</span></span>](app-insights-analytics.md)

