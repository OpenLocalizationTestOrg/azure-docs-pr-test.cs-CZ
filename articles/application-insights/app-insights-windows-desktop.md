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
# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Monitorování využití a výkonu desktopových aplikací pro Windows


[Azure Application Insights](app-insights-overview.md) a [HockeyApp](https://hockeyapp.net) umožňují monitorovat využití a výkon nasazené aplikace.

> [!IMPORTANT]
> Doporučujeme, abyste [HockeyApp](https://hockeyapp.net) toodistribute a monitorování aplikací desktop a zařízení. S HockeyApp může spravovat distribuci, testování za provozu a zpětnou vazbu od uživatelů, a zároveň monitorovat sestavy využití a chyb. Můžete také [exportovat telemetrii a dávat na ni dotazy pomocí Analytics](app-insights-hockeyapp-bridge-app.md).
> 
> I když telemetrie odesláním tooApplication statistiky z aplikace pracovní plochy, jedná se hlavně užitečná pro účely ladění a experimentální.
> 
> 

## <a name="toosend-telemetry-tooapplication-insights-from-a-windows-application"></a>toosend telemetrie tooApplication statistiky z aplikace systému Windows
1. V hello [portál Azure](https://portal.azure.com), [vytvořte prostředek Application Insights](app-insights-create-new-resource.md). Jako typ aplikace vyberte aplikaci ASP.NET.
2. Trvat kopii hello klíč instrumentace. Najít klíč hello v hello Essentials rozevíracího seznamu hello nového prostředku, který jste právě vytvořili. 
3. V sadě Visual Studio upravte balíčky NuGet hello projektu aplikace a přidejte Microsoft.ApplicationInsights.WindowsServer. (Nebo zvolte Microsoft.ApplicationInsights, pokud chcete úplné rozhraní API bez hello standardní telemetrie kolekce modulů hello.)
4. Nastavte klíč instrumentace hello buď ve vašem kódu:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "`*váš klíč*`";` 
   
    nebo v souboru ApplicationInsights.config (Pokud jste nainstalovali jednu standardní telemetrie balíčků hello):
   
    `<InstrumentationKey>`*váš klíč*`</InstrumentationKey>` 
   
    Pokud chcete použít soubor ApplicationInsights.config, ujistěte se, jeho vlastnosti v Průzkumníku řešení nastaveny příliš**akce sestavení = obsah, kopie tooOutput Directory = kopie**.
5. [Pomocí rozhraní API hello](app-insights-api-custom-events-metrics.md) toosend telemetrie.
6. Spuštění aplikace a v tématu hello telemetrie hello prostředků, kterou jste vytvořili v hello portálu Azure.

## <a name="telemetry"></a>Příklad kódu
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

## <a name="next-steps"></a>Další kroky
* [Vytvoření řídicího panelu](app-insights-dashboards.md)
* [Diagnostické vyhledávání](app-insights-diagnostic-search.md)
* [Zkoumání metrik](app-insights-metrics-explorer.md)
* [Psaní analytických dotazů](app-insights-analytics.md)

