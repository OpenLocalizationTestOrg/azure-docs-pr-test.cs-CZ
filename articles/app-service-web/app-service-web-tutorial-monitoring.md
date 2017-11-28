---
title: "aaaMonitor webové aplikace | Microsoft Docs"
description: "Zjistěte, jak tooset až monitorování na vaší webové aplikace"
services: App-Service
keywords: 
author: btardif
ms.author: byvinyal
ms.date: 04/04/2017
ms.topic: article
ms.service: app-service-web
ms.openlocfilehash: c2f5e9842c732a804f1caee5d67e53dad24e190a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-app-service"></a><span data-ttu-id="ef01e-103">Monitorování služby App Service</span><span class="sxs-lookup"><span data-stu-id="ef01e-103">Monitor App Service</span></span>
<span data-ttu-id="ef01e-104">Tento kurz vás provede monitorování aplikace a použití hello integrovanou platformu nástroje toosolve problémy, když k nim dojde.</span><span class="sxs-lookup"><span data-stu-id="ef01e-104">This tutorial walks you through monitoring your app and using hello built-in platform tools toosolve problems when they occur.</span></span>

<span data-ttu-id="ef01e-105">Každá část tohoto dokumentu prochází přes konkrétní funkci.</span><span class="sxs-lookup"><span data-stu-id="ef01e-105">Each section of this document goes over a specific feature.</span></span> <span data-ttu-id="ef01e-106">Pomocí funkce hello společně umožňují:</span><span class="sxs-lookup"><span data-stu-id="ef01e-106">Using hello features together let you:</span></span>
- <span data-ttu-id="ef01e-107">Identifikace problém ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ef01e-107">Identifying an issue in your app.</span></span>
- <span data-ttu-id="ef01e-108">Určení, kdy hello problém je způsobený platformou kód nebo hello.</span><span class="sxs-lookup"><span data-stu-id="ef01e-108">Determining when hello issue is caused by your code or hello platform.</span></span>
- <span data-ttu-id="ef01e-109">Zúží hello zdroj hello problému v kódu.</span><span class="sxs-lookup"><span data-stu-id="ef01e-109">Narrow down hello source of hello problem in your code.</span></span>
- <span data-ttu-id="ef01e-110">Ladění a opravě problému hello.</span><span class="sxs-lookup"><span data-stu-id="ef01e-110">Debugging and fixing hello issue.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ef01e-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ef01e-111">Before you begin</span></span>
- <span data-ttu-id="ef01e-112">Potřebujete toomonitor webové aplikace a hello postupujte podle uvedených kroků.</span><span class="sxs-lookup"><span data-stu-id="ef01e-112">You need a Web App toomonitor and follow hello outlined steps.</span></span>
    - <span data-ttu-id="ef01e-113">Můžete vytvořit aplikace hello kroků popsaných v hello [vytvoření aplikace ASP.NET v Azure SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="ef01e-113">You can create an application following hello steps described in hello [Create an ASP.NET app in Azure with SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) tutorial.</span></span>

- <span data-ttu-id="ef01e-114">Pokud chcete tootry **vzdálené ladění** vaší aplikace, musíte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef01e-114">If you want tootry out **Remote Debugging** of your application, you need Visual Studio.</span></span>
    - <span data-ttu-id="ef01e-115">Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít hello volné [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ef01e-115">If you don’t already have Visual Studio 2017 installed, you can download and use hello free [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span>
    - <span data-ttu-id="ef01e-116">Ujistěte se, že povolíte **Azure development** při instalaci sady Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="ef01e-116">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

## <span data-ttu-id="ef01e-117"><a name="metrics"></a>Krok 1 – zobrazit metriky</span><span class="sxs-lookup"><span data-stu-id="ef01e-117"><a name="metrics"></a> Step 1 - View metrics</span></span>
<span data-ttu-id="ef01e-118">**Metriky** jsou užitečné toounderstand:</span><span class="sxs-lookup"><span data-stu-id="ef01e-118">**Metrics** are useful toounderstand:</span></span>
- <span data-ttu-id="ef01e-119">Stav aplikace</span><span class="sxs-lookup"><span data-stu-id="ef01e-119">Application health</span></span>
- <span data-ttu-id="ef01e-120">Výkon aplikace</span><span class="sxs-lookup"><span data-stu-id="ef01e-120">App performance</span></span>
- <span data-ttu-id="ef01e-121">Spotřeba prostředků</span><span class="sxs-lookup"><span data-stu-id="ef01e-121">Resource consumption</span></span>

<span data-ttu-id="ef01e-122">Při zkoumání problému s aplikací, je kontrola metriky toostart vhodné místo.</span><span class="sxs-lookup"><span data-stu-id="ef01e-122">When investigating an application issue, reviewing metrics is a good place toostart.</span></span> <span data-ttu-id="ef01e-123">Portál Azure je rychlý způsob toovisually, zkontrolujte hello metrik vaší aplikace pomocí **Azure monitorování**.</span><span class="sxs-lookup"><span data-stu-id="ef01e-123">Azure portal has a quick way toovisually inspect hello metrics of your app using **Azure Monitor**.</span></span>

<span data-ttu-id="ef01e-124">Metriky zadejte Historický přehled napříč několika klíčových agregace pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ef01e-124">Metrics provide a historical view across several key aggregations for your app.</span></span> <span data-ttu-id="ef01e-125">Pro žádné aplikace hostované ve službě app service měli byste sledovat hello webové aplikace a hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="ef01e-125">For any app hosted in app service, you should monitor both hello Web App and hello App Service plan.</span></span>

> [!NOTE]
> <span data-ttu-id="ef01e-126">Plán služby App Service představuje kolekci hello toohost fyzické prostředky, které používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef01e-126">An App Service plan represents hello collection of physical resources used toohost your apps.</span></span> <span data-ttu-id="ef01e-127">Všechny aplikace přiřazené tooan služby App Service plán sdílené složky hello prostředky definovaná tímto povolení toosave nákladů při hostování více aplikací.</span><span class="sxs-lookup"><span data-stu-id="ef01e-127">All applications assigned tooan App Service plan share hello resources defined by it allowing you toosave cost when hosting multiple apps.</span></span>
>
> <span data-ttu-id="ef01e-128">Plány služby App Service definují:</span><span class="sxs-lookup"><span data-stu-id="ef01e-128">App Service plans define:</span></span>
> * <span data-ttu-id="ef01e-129">Oblast: Severní Evropa, východní USA, jihovýchodní Asie, atd.</span><span class="sxs-lookup"><span data-stu-id="ef01e-129">Region: North Europe, East US, Southeast Asia, etc.</span></span>
> * <span data-ttu-id="ef01e-130">Velikost instance: Malé, střední, velké, atd.</span><span class="sxs-lookup"><span data-stu-id="ef01e-130">Instance Size: Small, Medium, Large, etc.</span></span>
> * <span data-ttu-id="ef01e-131">Počet škálování: jednu, dvě nebo tři instance, atd.</span><span class="sxs-lookup"><span data-stu-id="ef01e-131">Scale Count: one, two, or three instances, etc.</span></span>
> * <span data-ttu-id="ef01e-132">Skladová položka: Free, sdílené, Basic, Standard, Premium, atd.</span><span class="sxs-lookup"><span data-stu-id="ef01e-132">SKU: Free, Shared, Basic, Standard, Premium, etc.</span></span>

<span data-ttu-id="ef01e-133">tooreview metriky pro vaši webovou aplikaci, přejděte toohello **přehled** okno aplikace hello chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="ef01e-133">tooreview metrics for your Web App, go toohello **Overview** blade of hello app you want toomonitor.</span></span> <span data-ttu-id="ef01e-134">Tady můžete zobrazit graf metrik vaší aplikace, jako **dlaždice monitorování**.</span><span class="sxs-lookup"><span data-stu-id="ef01e-134">From here, you can view a chart for your app's metrics as a **Monitoring tile**.</span></span> <span data-ttu-id="ef01e-135">Klikněte na dlaždici tooedit hello a nakonfigurovat co tooview metriky a hello toodisplay rozsah času.</span><span class="sxs-lookup"><span data-stu-id="ef01e-135">Click hello tile tooedit and configure what metrics tooview and hello time range toodisplay.</span></span>

<span data-ttu-id="ef01e-136">Ve výchozím okně prostředků hello poskytuje zobrazení pro žádosti o aplikace hello a chyby pro hello poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="ef01e-136">By default hello resource blade provides a view for hello Application Requests and errors for hello last hour.</span></span>
<span data-ttu-id="ef01e-137">![Monitorování aplikace](media/app-service-web-tutorial-monitoring/app-service-monitor.png)</span><span class="sxs-lookup"><span data-stu-id="ef01e-137">![Monitor App](media/app-service-web-tutorial-monitoring/app-service-monitor.png)</span></span>

<span data-ttu-id="ef01e-138">Jak vidíte v příkladu hello, budeme mít aplikaci, která generuje mnoho **chyby protokolu HTTP serveru**.</span><span class="sxs-lookup"><span data-stu-id="ef01e-138">As you can see in hello example, we have an application that is generating many **HTTP Server Errors**.</span></span> <span data-ttu-id="ef01e-139">Hello velkému počtu chyb je hello první znamenat, že potřebujeme tooinvestigate této aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef01e-139">hello high volume of errors is hello first indication we need tooinvestigate this application.</span></span>

> [!TIP]
> <span data-ttu-id="ef01e-140">Další informace o monitorování Azure s hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="ef01e-140">Learn more about Azure Monitor with hello following links:</span></span>
> - [<span data-ttu-id="ef01e-141">Začínáme s Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="ef01e-141">Get started with Azure Monitor</span></span>](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [<span data-ttu-id="ef01e-142">Azure metriky</span><span class="sxs-lookup"><span data-stu-id="ef01e-142">Azure Metrics</span></span>](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [<span data-ttu-id="ef01e-143">Podporované metriky s monitorováním Azure</span><span class="sxs-lookup"><span data-stu-id="ef01e-143">Supported metrics with Azure Monitor</span></span>](..\monitoring-and-diagnostics\monitoring-supported-metrics.md)
> - [<span data-ttu-id="ef01e-144">Azure řídicí panely</span><span class="sxs-lookup"><span data-stu-id="ef01e-144">Azure Dashboards</span></span>](..\azure-portal\azure-portal-dashboards.md)

## <span data-ttu-id="ef01e-145"><a name="alerts"></a>Krok 2 – konfigurace výstrah</span><span class="sxs-lookup"><span data-stu-id="ef01e-145"><a name="alerts"></a> Step 2 - Configure Alerts</span></span>
<span data-ttu-id="ef01e-146">**Výstrahy** může být nakonfigurované tootrigger na konkrétní podmínky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ef01e-146">**Alerts** can be configured tootrigger on specific conditions for your app.</span></span>

<span data-ttu-id="ef01e-147">V [krok 1 – zobrazit metriky](#metrics), jsme viděli, že aplikace hello měl velký počet chyb.</span><span class="sxs-lookup"><span data-stu-id="ef01e-147">In [Step 1 - View metrics](#metrics), we saw that hello application had a high number of errors.</span></span>

<span data-ttu-id="ef01e-148">Umožňuje nakonfigurovat výstrahu tooautomatically dostat upozornění, pokud dojde k chybám.</span><span class="sxs-lookup"><span data-stu-id="ef01e-148">Lets configure an alert tooautomatically get notified when errors occur.</span></span> <span data-ttu-id="ef01e-149">V takovém případě má toosend hello výstrah a e-mailu pokaždé, když hello počet chyb HTTP 50 X prochází přes určitou mez.</span><span class="sxs-lookup"><span data-stu-id="ef01e-149">In this case, we want hello alert toosend and e-mail every time hello number of HTTP 50X errors goes over a certain threshold.</span></span>

<span data-ttu-id="ef01e-150">toocreate výstraha, přejděte příliš**monitorování** > **výstrahy** a klikněte na tlačítko **[+] přidat upozornění**.</span><span class="sxs-lookup"><span data-stu-id="ef01e-150">toocreate an alert, navigate too**Monitoring** > **Alerts** and click **[+] Add Alert**.</span></span>

![Výstrahy](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

<span data-ttu-id="ef01e-152">Zadejte hodnoty pro konfiguraci výstrah hello:</span><span class="sxs-lookup"><span data-stu-id="ef01e-152">Provide values for hello Alert configuration:</span></span>
- <span data-ttu-id="ef01e-153">**Prostředek:** hello toomonitor lokality s hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="ef01e-153">**Resource:** hello site toomonitor with hello alert.</span></span>
- <span data-ttu-id="ef01e-154">**Název:** název výstrahy, v tomto případě: *vysoké HTTP 50 X*.</span><span class="sxs-lookup"><span data-stu-id="ef01e-154">**Name:** A name for your alert, in this case: *High HTTP 50X*.</span></span>
- <span data-ttu-id="ef01e-155">**Popis:** prostý text vysvětlení, co je tato výstraha prohlížení.</span><span class="sxs-lookup"><span data-stu-id="ef01e-155">**Description:** Plain text explanation of what this alert is looking at.</span></span>
- <span data-ttu-id="ef01e-156">**Výstraha:** výstrahy můžete si prohlédnout metriky nebo událostí, v tomto příkladu Těšíme se na metriky.</span><span class="sxs-lookup"><span data-stu-id="ef01e-156">**Alert on:** Alerts can look at Metrics or Events, for this example we are looking at metrics.</span></span>
- <span data-ttu-id="ef01e-157">**Metrika:** jaké metriky toomonitor, v tomto případě: *chyby protokolu HTTP serveru*.</span><span class="sxs-lookup"><span data-stu-id="ef01e-157">**Metric:** What metric toomonitor, in this case: *HTTP Server Errors*.</span></span>
- <span data-ttu-id="ef01e-158">**Podmínka:** při tooalert, v tomto případě vyberte hello *větší než* možnost.</span><span class="sxs-lookup"><span data-stu-id="ef01e-158">**Condition:** When tooalert, in this case select hello *greater than* option.</span></span>
- <span data-ttu-id="ef01e-159">**Prahová hodnota:** co toolook hodnotu, je v tomto případě: *400*.</span><span class="sxs-lookup"><span data-stu-id="ef01e-159">**Threshold:** What is value toolook for, in this case: *400*.</span></span>
- <span data-ttu-id="ef01e-160">**Období:** výstrahy pracovat s průměrnou hodnotu hello metriky.</span><span class="sxs-lookup"><span data-stu-id="ef01e-160">**Period:** Alerts operate over hello average value of a metric.</span></span> <span data-ttu-id="ef01e-161">Menší období yield citlivější výstrahy.</span><span class="sxs-lookup"><span data-stu-id="ef01e-161">Smaller periods of time yield more sensitive alerts.</span></span> <span data-ttu-id="ef01e-162">v takovém případě Těšíme se na *5 minut*.</span><span class="sxs-lookup"><span data-stu-id="ef01e-162">in this case we are looking at *5 Minutes*.</span></span>
- <span data-ttu-id="ef01e-163">**E-mailu vlastníci a přispěvatelé:** v tomto případě: *povoleno*.</span><span class="sxs-lookup"><span data-stu-id="ef01e-163">**Email Owners and contributors:** in this case: *Enabled*.</span></span>

<span data-ttu-id="ef01e-164">Je teď vytvořená hello výstraha e-mail je odeslán pokaždé, když hello aplikace přejde nad prahovou hodnotou hello nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="ef01e-164">Now that hello alert is created an email is sent every time hello app goes over hello configured threshold.</span></span> <span data-ttu-id="ef01e-165">Aktivní výstrahy mohou být také zjišťovány hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ef01e-165">Active alerts can also be reviewed in hello Azure portal.</span></span>

![Spouštěná výstrahy](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)


> [!TIP]
> <span data-ttu-id="ef01e-167">Další informace o výstrahách Azure s hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="ef01e-167">Learn more about Azure Alerts with hello following links:</span></span>
> - [<span data-ttu-id="ef01e-168">Co jsou výstrahy v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="ef01e-168">What are alerts in Microsoft Azure</span></span>](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [<span data-ttu-id="ef01e-169">Provést akci pro metriky</span><span class="sxs-lookup"><span data-stu-id="ef01e-169">Take Action On Metrics</span></span>](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [<span data-ttu-id="ef01e-170">Vytvoření metriky výstrahy</span><span class="sxs-lookup"><span data-stu-id="ef01e-170">Create metric alerts</span></span>](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <span data-ttu-id="ef01e-171"><a name="companion"></a>Krok 3 – doprovodné služby App Service</span><span class="sxs-lookup"><span data-stu-id="ef01e-171"><a name="companion"></a> Step 3 - App Service Companion</span></span>
<span data-ttu-id="ef01e-172">**Služby App Service doprovodné** nabízí pohodlný způsob toomonitor aplikace s nativním prostředím v mobilních zařízení (iOS nebo Android).</span><span class="sxs-lookup"><span data-stu-id="ef01e-172">**App Service companion** offers a convenient way toomonitor your app with a native experience in your mobile device (iOS or Android).</span></span>

<span data-ttu-id="ef01e-173">Použijte doprovodné služby App Service na:</span><span class="sxs-lookup"><span data-stu-id="ef01e-173">Use App Service Companion to:</span></span>
- <span data-ttu-id="ef01e-174">Zkontrolujte metriky aplikace</span><span class="sxs-lookup"><span data-stu-id="ef01e-174">Review application metrics</span></span>
- <span data-ttu-id="ef01e-175">Zkontrolujte a provádění akcí na výstrahy aplikace a doporučení</span><span class="sxs-lookup"><span data-stu-id="ef01e-175">Review and act on application alerts and recommendations</span></span>
- <span data-ttu-id="ef01e-176">Provádět základní řešení problémů (Procházet, spuštění, zastavení, restartování aplikace hello)</span><span class="sxs-lookup"><span data-stu-id="ef01e-176">Perform basic troubleshooting (browse, start, stop, restart hello app)</span></span>
- <span data-ttu-id="ef01e-177">Získáte nabízená oznámení pro kritické události.</span><span class="sxs-lookup"><span data-stu-id="ef01e-177">Get push notifications for critical events.</span></span>

![Doprovodná služby App Service](media/app-service-web-tutorial-monitoring/app-service-companion.png)

<span data-ttu-id="ef01e-179">[![Aplikace služby doprovodné obchod](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![doprovodné aplikace služby Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span><span class="sxs-lookup"><span data-stu-id="ef01e-179">[![App Service Companion App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![App Service Companion Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span></span>

<span data-ttu-id="ef01e-180">Doprovodná služby App Service si můžete nainstalovat z hello [obchod](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) nebo [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span><span class="sxs-lookup"><span data-stu-id="ef01e-180">You can install App Service companion from hello [App Store](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) or [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span></span>

## <span data-ttu-id="ef01e-181"><a name="diagnose"></a>Krok 4 – Diagnostika a řešení problémů</span><span class="sxs-lookup"><span data-stu-id="ef01e-181"><a name="diagnose"></a> Step 4 - Diagnose and solve problems</span></span>
<span data-ttu-id="ef01e-182">**Diagnostika a řešení problémů** pomáhá oddělíte problémy aplikací formuláři problém s platformou.</span><span class="sxs-lookup"><span data-stu-id="ef01e-182">**Diagnose and solve problems** helps you separate application issues form platform issues.</span></span> <span data-ttu-id="ef01e-183">Ho můžete také navrhnout možné způsoby zmírnění tooget back toohealthy vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef01e-183">It can also suggest possible mitigations tooget your Web App back toohealthy.</span></span>

![Diagnostika a řešení problémů](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

<span data-ttu-id="ef01e-185">Pokračováním hello příklad formuláře předchozí kroky, vidíme, že aplikace hello má byla dostupnosti problémy s.</span><span class="sxs-lookup"><span data-stu-id="ef01e-185">Continuing with hello example form previous steps, we can see that hello application has been having availably issues.</span></span> <span data-ttu-id="ef01e-186">Naproti tomu hello platformy dostupnosti nepřesunul ze 100 %.</span><span class="sxs-lookup"><span data-stu-id="ef01e-186">In contrast, hello platform availability has not moved from 100%.</span></span>

<span data-ttu-id="ef01e-187">Když aplikace hello má problém a hello platformy zůstane až, je jasný náznak, který jsme se problému s aplikací pracovat.</span><span class="sxs-lookup"><span data-stu-id="ef01e-187">When hello app is having issue and hello platform stays up, it's a clear indication that we are dealing with an application issue.</span></span>

## <span data-ttu-id="ef01e-188"><a name="logging"></a>Krok 5 – protokolování</span><span class="sxs-lookup"><span data-stu-id="ef01e-188"><a name="logging"></a> Step 5 - Logging</span></span>
<span data-ttu-id="ef01e-189">Teď, když budeme mít co nejlépe určen hello selhání tooan aplikace problém, můžete podíváme na tooget protokoly aplikací a server hello Další informace.</span><span class="sxs-lookup"><span data-stu-id="ef01e-189">Now that we have narrowed down hello failures tooan application issue, we can look at hello application and server logs tooget more information.</span></span>

<span data-ttu-id="ef01e-190">Protokolování vám umožní toocollect obou **rozhraní Application Diagnostics** a **Diagnostika webového serveru** protokoly pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ef01e-190">Logging allows you toocollect both **Application Diagnostics** and **Web Server Diagnostics** logs for your Web App.</span></span>

### <a name="application-diagnostics"></a><span data-ttu-id="ef01e-191">Rozhraní Application Diagnostics</span><span class="sxs-lookup"><span data-stu-id="ef01e-191">Application Diagnostics</span></span>
<span data-ttu-id="ef01e-192">Rozhraní Application diagnostics umožňuje vám trasování toocapture vyprodukované hello aplikace za běhu.</span><span class="sxs-lookup"><span data-stu-id="ef01e-192">Application diagnostics allows you toocapture traces produced by hello application at runtime.</span></span>

<span data-ttu-id="ef01e-193">Přidání trasování tooyour aplikace výrazně zlepšuje možnost toodebug a problémy Kotvicí bod.</span><span class="sxs-lookup"><span data-stu-id="ef01e-193">Adding tracing tooyour application greatly improves your ability toodebug and pin-point issues.</span></span>

<span data-ttu-id="ef01e-194">V technologii ASP.NET, může přihlásit pomocí trasování aplikací [System.Diagnostics.Trace – třída](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) toogenerate události, které jsou zachyceny hello protokolu infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="ef01e-194">In ASP.NET, you can log application traces using [System.Diagnostics.Trace class](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) toogenerate events that are captured by hello log infrastructure.</span></span> <span data-ttu-id="ef01e-195">Můžete také zadat hello závažnost hello trasování pro snazší filtrování.</span><span class="sxs-lookup"><span data-stu-id="ef01e-195">You can also specify hello severity of hello trace for easier filtering.</span></span>

```csharp
public ActionResult Delete(Guid? id)
{
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id);
    if (id == null)
    {
        System.Diagnostics.Trace.TraceError("/Todos/Delete/ failed, ID is null");
        return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
    }
    Todo todo = db.Todoes.Find(id);
    if (todo == null)
    {
        System.Diagnostics.Trace.TraceWarning("/Todos/Delete/ failed, ID: " + id + " could not be found");
        return HttpNotFound();
    }
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id + "completed successfully");
    return View(todo);
}
```
<span data-ttu-id="ef01e-196">protokolování aplikací tooenable přejděte příliš**monitorování** > **diagnostické protokoly** a povolit protokolování aplikace pomocí přepínačů hello.</span><span class="sxs-lookup"><span data-stu-id="ef01e-196">tooenable Application logging Go too**Monitoring** > **Diagnostic Logs** and Enable Application Logging using hello toggles.</span></span>

![Monitorování aplikace](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

<span data-ttu-id="ef01e-198">Protokoly aplikací může být systém souborů uložených tooyour webovou aplikaci nebo instalováni tooblob úložiště.</span><span class="sxs-lookup"><span data-stu-id="ef01e-198">Application logs can be stored tooyour Web App's file system or pushed out tooblob storage.</span></span> <span data-ttu-id="ef01e-199">U produkčních scénářích je doporučené toouse úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="ef01e-199">For production scenarios, it's recommended toouse blob storage.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ef01e-200">Povolení protokolování má vliv na vaše aplikace výkonu a využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="ef01e-200">Enabling logging has an impact on your application performance and resource utilization.</span></span> <span data-ttu-id="ef01e-201">Pro produkčních scénářích se doporučují protokoly chyb.</span><span class="sxs-lookup"><span data-stu-id="ef01e-201">For production scenarios, error logs are recommended.</span></span> <span data-ttu-id="ef01e-202">Podrobnější protokolování povolte pouze v případě příčin problémů.</span><span class="sxs-lookup"><span data-stu-id="ef01e-202">Only enable more verbose logging when investigating issues.</span></span>

 ### <a name="web-server-diagnostics"></a><span data-ttu-id="ef01e-203">Diagnostika webového serveru</span><span class="sxs-lookup"><span data-stu-id="ef01e-203">Web Server Diagnostics</span></span>
<span data-ttu-id="ef01e-204">Protokoly webového serveru jsou generovány, i v případě, že vaše aplikace není instrumentována.</span><span class="sxs-lookup"><span data-stu-id="ef01e-204">Web Server logs are generated even if your app is not instrumented.</span></span> <span data-ttu-id="ef01e-205">Služby App Service může shromažďovat tři různé typy protokolů serveru:</span><span class="sxs-lookup"><span data-stu-id="ef01e-205">App Service can collect three different types of server logs:</span></span>

- <span data-ttu-id="ef01e-206">**Protokolování webového serveru**</span><span class="sxs-lookup"><span data-stu-id="ef01e-206">**Web Server Logging**</span></span>
    - <span data-ttu-id="ef01e-207">Informace o transakcích HTTP pomocí hello [rozšířený formát protokolu W3C souboru](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef01e-207">Information about HTTP transactions using hello [W3C extended log file format](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).</span></span>
    - <span data-ttu-id="ef01e-208">Užitečné při určování metriky celkového lokality například hello počet požadavků zpracovaných nebo je počet požadavků z konkrétní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="ef01e-208">Useful when determining overall site metrics such as hello number of requests handled or how many requests are from a specific IP address.</span></span>
- <span data-ttu-id="ef01e-209">**Podrobné informace o chybě protokolování**</span><span class="sxs-lookup"><span data-stu-id="ef01e-209">**Detailed Error Logging**</span></span>
    - <span data-ttu-id="ef01e-210">Podrobné informace o chybě pro stavové kódy HTTP, které indikují chybu (kód stavu 400 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="ef01e-210">Detailed error information for HTTP status codes that indicate a failure (status code 400 or greater).</span></span>
    - [<span data-ttu-id="ef01e-211">Další informace o protokolování podrobné informace o chybě</span><span class="sxs-lookup"><span data-stu-id="ef01e-211">Learn more about detailed error logging</span></span>](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- <span data-ttu-id="ef01e-212">**Trasování neúspěšných žádostí.**</span><span class="sxs-lookup"><span data-stu-id="ef01e-212">**Failed Request Tracing**</span></span>
    - <span data-ttu-id="ef01e-213">Podrobné informace o chybných žádostech, včetně trasování pro součásti služby IIS hello používá tooprocess hello požadavku a hello doba v jednotlivých součástí.</span><span class="sxs-lookup"><span data-stu-id="ef01e-213">Detailed information on failed requests, including a trace of hello IIS components used tooprocess hello request and hello time taken in each component.</span></span>
    - <span data-ttu-id="ef01e-214">Při pokusu o tooisolate co ho způsobuje. konkrétní chyba protokolu HTTP lze využít protokoly chybných požadavků.</span><span class="sxs-lookup"><span data-stu-id="ef01e-214">Failed request logs are useful when trying tooisolate what is causing a specific HTTP error.</span></span>
    - [<span data-ttu-id="ef01e-215">Další informace o trasování neúspěšných žádostí.</span><span class="sxs-lookup"><span data-stu-id="ef01e-215">Learn more about failed request tracing</span></span>](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

<span data-ttu-id="ef01e-216">protokolování serveru tooenable:</span><span class="sxs-lookup"><span data-stu-id="ef01e-216">tooenable Server logging:</span></span>
- <span data-ttu-id="ef01e-217">přejděte příliš**monitorování** > **diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="ef01e-217">go too**Monitoring** > **Diagnostic Logs**.</span></span>
- <span data-ttu-id="ef01e-218">Povolte hello různé typy Diagnostika webového serveru pomocí přepínačů hello.</span><span class="sxs-lookup"><span data-stu-id="ef01e-218">Enable hello different types of Web Server Diagnostics using hello toggles.</span></span>

![Monitorování aplikace](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> <span data-ttu-id="ef01e-220">Povolení protokolování má vliv na vaše aplikace výkonu a využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="ef01e-220">Enabling logging has an impact on your application performance and resource utilization.</span></span> <span data-ttu-id="ef01e-221">Pro produkčních scénářích se doporučuje protokoly chyb, pouze povolit podrobnější protokolování při zkoumání problémů.</span><span class="sxs-lookup"><span data-stu-id="ef01e-221">For Production Scenarios, Error logs are recommended, Only Enable more verbose logging when investigating issues.</span></span>

### <a name="accessing-logs"></a><span data-ttu-id="ef01e-222">Přístup k protokoly</span><span class="sxs-lookup"><span data-stu-id="ef01e-222">Accessing Logs</span></span>
<span data-ttu-id="ef01e-223">Protokolů uložených v úložišti objektů blob ke kterým se přistupuje pomocí Průzkumníka úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ef01e-223">Logs stored in blob storage are accessed using Azure Storage Explorer.</span></span> <span data-ttu-id="ef01e-224">Protokolů uložených v systému souborů hello webové aplikace jsou přístupné prostřednictvím FTP v rámci hello následující cesty:</span><span class="sxs-lookup"><span data-stu-id="ef01e-224">Logs stored in hello Web App's filesystem are accessed through FTP under hello following paths:</span></span>

- <span data-ttu-id="ef01e-225">**Protokoly aplikací** - `%HOME%/LogFiles/Application/`.</span><span class="sxs-lookup"><span data-stu-id="ef01e-225">**Application logs** - `%HOME%/LogFiles/Application/`.</span></span>
    - <span data-ttu-id="ef01e-226">Tato složka obsahuje jeden nebo více souborů textu obsahujícího informace o vyprodukované protokolování aplikací.</span><span class="sxs-lookup"><span data-stu-id="ef01e-226">This folder contains one or more text files containing information produced by application logging.</span></span>
- <span data-ttu-id="ef01e-227">**Trasování požadavku se nezdařilo** - `%HOME%/LogFiles/W3SVC#########/`.</span><span class="sxs-lookup"><span data-stu-id="ef01e-227">**Failed Request Traces** - `%HOME%/LogFiles/W3SVC#########/`.</span></span>
    - <span data-ttu-id="ef01e-228">Tato složka obsahuje soubor XSL a jeden nebo více souborů XML.</span><span class="sxs-lookup"><span data-stu-id="ef01e-228">This folder contains an XSL file and one or more XML files.</span></span>
- <span data-ttu-id="ef01e-229">**Podrobné protokoly chyb** - `%HOME%/LogFiles/DetailedErrors/`.</span><span class="sxs-lookup"><span data-stu-id="ef01e-229">**Detailed Error Logs** - `%HOME%/LogFiles/DetailedErrors/`.</span></span>
    - <span data-ttu-id="ef01e-230">Tato složka obsahuje jeden nebo více souborů HTM rozsáhlé informace na chyby protokolu HTTP vygenerovaný vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="ef01e-230">This folder contains one or more .htm files with extensive information on HTTP errors generated by your app.</span></span>
- <span data-ttu-id="ef01e-231">**Webový Server protokoly** - `%HOME%/LogFiles/http/RawLogs`.</span><span class="sxs-lookup"><span data-stu-id="ef01e-231">**Web Server Logs** - `%HOME%/LogFiles/http/RawLogs`.</span></span>
    - <span data-ttu-id="ef01e-232">Tato složka obsahuje jeden nebo více souborů text formátován pomocí formát souboru protokolu rozšířené hello W3C.</span><span class="sxs-lookup"><span data-stu-id="ef01e-232">This folder contains one or more text files formatted using hello W3C extended log file format.</span></span>

## <span data-ttu-id="ef01e-233"><a name="streaming"></a>Krok 6: protokolu streamování</span><span class="sxs-lookup"><span data-stu-id="ef01e-233"><a name="streaming"></a> Step 6 - Log Streaming</span></span>
<span data-ttu-id="ef01e-234">Protokoly streamování jsou vhodné při ladění aplikace, protože ho šetří čas porovnání příliš[přístup k hello protokoly](#Accessing-Logs) pomocí služby FTP.</span><span class="sxs-lookup"><span data-stu-id="ef01e-234">Streaming logs are convenient when debugging an application since it saves time compared too[accessing hello logs](#Accessing-Logs) through FTP.</span></span>

<span data-ttu-id="ef01e-235">Služby App Service můžete stream **protokoly aplikací** a **protokolů webového serveru** jako jsou generovány.</span><span class="sxs-lookup"><span data-stu-id="ef01e-235">App Service can stream **Application Logs** and **Web Server Logs** as they are generated.</span></span>

> [!TIP]
> <span data-ttu-id="ef01e-236">Než to zkusíte toostream protokoly, ujistěte se, jak je popsáno v hello jste povolili shromažďování protokolů [protokolování](#logging) části.</span><span class="sxs-lookup"><span data-stu-id="ef01e-236">Before trying toostream logs, make sure you have enabled collecting logs as described in hello [Logging](#logging) section.</span></span>

<span data-ttu-id="ef01e-237">protokoly toostream přejděte příliš**monitorování**> **datový proud protokolu**.</span><span class="sxs-lookup"><span data-stu-id="ef01e-237">toostream logs, go too**Monitoring**> **Log Stream**.</span></span> <span data-ttu-id="ef01e-238">Vyberte **protokoly aplikací** nebo **webové protokoly serveru** podle informací hledáte.</span><span class="sxs-lookup"><span data-stu-id="ef01e-238">Select **Application Logs** or **Web server logs** depending what information you are looking for.</span></span> <span data-ttu-id="ef01e-239">Tady můžete pozastavit, restartovat a vymazat hello vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="ef01e-239">From here, you can also pause, restart, and clear hello buffer.</span></span>

![Protokoly streamování](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> <span data-ttu-id="ef01e-241">Protokoly jsou generovány pouze, pokud je provoz na hello aplikace, můžete taky zvýšit hello podrobností protokoly tooget další události nebo informace.</span><span class="sxs-lookup"><span data-stu-id="ef01e-241">Logs are only generated when there is traffic on hello app, you can also increase hello verbosity of logs tooget more events or information.</span></span>

## <span data-ttu-id="ef01e-242"><a name="remote"></a>Krok 7 – vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="ef01e-242"><a name="remote"></a> Step 7 - Remote Debugging</span></span>
<span data-ttu-id="ef01e-243">Jakmile máte PIN kód odkazoval hello zdroje hello problémů aplikace, použijte **vzdálené ladění** toowalk prostřednictvím hello kódu.</span><span class="sxs-lookup"><span data-stu-id="ef01e-243">Once you have pin-pointed hello source of hello applications problems, use **Remote Debugging** toowalk through hello code.</span></span>

<span data-ttu-id="ef01e-244">Vzdálené ladění umožňuje připojit ladicí program tooyour webová aplikace spuštěna v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="ef01e-244">Remote debugging lets you attach a debugger tooyour Web App running in hello cloud.</span></span> <span data-ttu-id="ef01e-245">Můžete nastavit zarážky, pracovat přímo s paměti, krok prostřednictvím kódu a dokonce změnit hello kódové cestě stejně, jako je tomu u aplikace spuštěná místně.</span><span class="sxs-lookup"><span data-stu-id="ef01e-245">You can set breakpoints, manipulate memory directly, step through code, and even change hello code path just like you do for an app running locally.</span></span>

<span data-ttu-id="ef01e-246">aplikaci tooyour ladicího programu hello tooattach spuštěnou v cloudu hello:</span><span class="sxs-lookup"><span data-stu-id="ef01e-246">tooattach hello debugger tooyour app running in hello cloud:</span></span>

- <span data-ttu-id="ef01e-247">Pomocí Visual Studio 2017, otevřete hello řešení pro aplikaci hello chcete toodebug</span><span class="sxs-lookup"><span data-stu-id="ef01e-247">Using Visual Studio 2017, open hello solution for hello app you want toodebug</span></span>
- <span data-ttu-id="ef01e-248">Nastavte některé body brzdy stejně, jako byste pro místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="ef01e-248">Set some brake points just like you would for local development.</span></span>
- <span data-ttu-id="ef01e-249">Otevřete **Průzkumník cloudu** (pev.cenu + /, ctrl + x).</span><span class="sxs-lookup"><span data-stu-id="ef01e-249">Open **cloud explorer** (ctr + /, ctrl + x).</span></span>
- <span data-ttu-id="ef01e-250">Přihlaste se pomocí přihlašovacích údajů azure podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="ef01e-250">Log in with your azure credentials as needed.</span></span>
- <span data-ttu-id="ef01e-251">Aplikace hello najít chcete toodebug</span><span class="sxs-lookup"><span data-stu-id="ef01e-251">Find hello app you want toodebug</span></span>
- <span data-ttu-id="ef01e-252">Vyberte **připojit ladicí program** formuláře hello **akce** podokně.</span><span class="sxs-lookup"><span data-stu-id="ef01e-252">Select **Attach Debugger** form hello **Actions** pane.</span></span>

![Vzdálené ladění](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

<span data-ttu-id="ef01e-254">Visual Studio nakonfiguruje aplikaci pro vzdálené ladění a spustí okno prohlížeče, který odkazuje tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef01e-254">Visual Studio configures your application for remote debugging and launches a browser window that navigates tooyour app.</span></span> <span data-ttu-id="ef01e-255">Procházet body rozdělení tootrigger vaší aplikace a krok prostřednictvím hello kódu.</span><span class="sxs-lookup"><span data-stu-id="ef01e-255">Browse through your app tootrigger break points and step through hello code.</span></span>

> [!WARNING]
> <span data-ttu-id="ef01e-256">Spuštění v režimu ladění v produkčním prostředí se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="ef01e-256">Running in debug mode in production is not recommended.</span></span> <span data-ttu-id="ef01e-257">Pokud vaše produkční aplikace není škálovat instance serveru toomultiple, ladění zabránit hello webový server z odpovídá tooother žádostí.</span><span class="sxs-lookup"><span data-stu-id="ef01e-257">If your production app is not scaled out toomultiple server instances, debugging prevent hello web server from responding tooother requests.</span></span> <span data-ttu-id="ef01e-258">Při řešení problémů produkční, nejlepší prostředek je příliš[konfigurace protokolování](#logging) a [Application Insights](#insights).</span><span class="sxs-lookup"><span data-stu-id="ef01e-258">For troubleshooting production problems, your best resource is too[configure logging](#logging) and [Application Insights](#insights).</span></span>



## <span data-ttu-id="ef01e-259"><a name="explorer"></a>Krok 8 – Průzkumníka procesů</span><span class="sxs-lookup"><span data-stu-id="ef01e-259"><a name="explorer"></a> Step 8 - Process Explorer</span></span>
<span data-ttu-id="ef01e-260">Pokud vaše aplikace je škálovat toomore než jednu instanci, **Průzkumníka procesů** můžete identifikovat problémy konkrétní instanci.</span><span class="sxs-lookup"><span data-stu-id="ef01e-260">When your application is scaled out toomore than one instance, **process explorer** can help you identify instance specific problems.</span></span>

<span data-ttu-id="ef01e-261">Použití **zpracovat Explorer** na:</span><span class="sxs-lookup"><span data-stu-id="ef01e-261">Use **Process Explorer** to:</span></span>

- <span data-ttu-id="ef01e-262">Výčet všech procesů hello mezi různými instancemi plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="ef01e-262">Enumerate all hello processes across different instances of your App Service plan.</span></span>
- <span data-ttu-id="ef01e-263">Procházet a zobrazte hello obslužné rutiny a moduly, které jsou přidružené k jednotlivých procesů.</span><span class="sxs-lookup"><span data-stu-id="ef01e-263">Drill down and view hello handles and modules associated with each process.</span></span>
- <span data-ttu-id="ef01e-264">Počet zobrazení procesoru, pracovní sady a přístup z více vláken v hello zpracovat úrovně toohelp identifikovat nezvladatelné procesy</span><span class="sxs-lookup"><span data-stu-id="ef01e-264">View CPU, Working Set, and Thread count at hello process level toohelp you identify runaway processes</span></span>
- <span data-ttu-id="ef01e-265">Najděte otevřených popisovačů souborů a i kill instance konkrétní procesu.</span><span class="sxs-lookup"><span data-stu-id="ef01e-265">Find open file handles, and even kill a specific process instance.</span></span>

<span data-ttu-id="ef01e-266">Průzkumníka procesů naleznete v části **monitorování** > **Průzkumníka procesů**.</span><span class="sxs-lookup"><span data-stu-id="ef01e-266">Process Explorer can be found under **Monitoring** > **Process Explorer**.</span></span>

![Průzkumník procesů](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)


## <span data-ttu-id="ef01e-268"><a name="insights"></a>Krok 9 – Application Insights</span><span class="sxs-lookup"><span data-stu-id="ef01e-268"><a name="insights"></a> Step 9 - Application Insights</span></span>
<span data-ttu-id="ef01e-269">**Application Insights** poskytuje profilace aplikací a rozšířené možnosti monitorování pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ef01e-269">**Application Insights** provides application profiling and advanced monitoring capabilities for your app.</span></span>

<span data-ttu-id="ef01e-270">Pomocí Application Insights toodetect a diagnostikovat výjimky a problémy s výkonem ve vaší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ef01e-270">Use Application Insights toodetect and diagnose exceptions and performance issues in your Web App.</span></span>

<span data-ttu-id="ef01e-271">Můžete povolit Application Insights pro webové aplikace v rámci **monitorování** > **Application Insights**</span><span class="sxs-lookup"><span data-stu-id="ef01e-271">You can enable Application Insights for your Web App under **Monitoring** > **Application Insights**</span></span>

> [!NOTE]
> <span data-ttu-id="ef01e-272">Application Insights může výzvu tooinstall hello Application Insights lokality rozšíření toostart shromažďování dat.</span><span class="sxs-lookup"><span data-stu-id="ef01e-272">Application Insights might prompt you tooinstall hello Application Insights site extension toostart collecting data.</span></span> <span data-ttu-id="ef01e-273">Instaluje se rozšíření lokality hello způsobí restartování aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef01e-273">Installing hello site extension causes an application restart.</span></span>

![Application Insights](media/app-service-web-tutorial-monitoring/app-service-monitor-appinsights.png)

<span data-ttu-id="ef01e-275">Application Insights obsahuje bohaté funkce nastavit, toolearn víc, použijte odkazy hello součástí hello [další kroky](#next) části.</span><span class="sxs-lookup"><span data-stu-id="ef01e-275">Application Insights has a rich feature set, toolearn more, follow hello links included in hello [Next Steps](#next) section.</span></span>

## <span data-ttu-id="ef01e-276"><a name="next"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="ef01e-276"><a name="next"></a> Next steps</span></span>

 - [<span data-ttu-id="ef01e-277">Co je Application Insights</span><span class="sxs-lookup"><span data-stu-id="ef01e-277">What is Application Insights</span></span>](..\application-insights\app-insights-overview.md)
 - [<span data-ttu-id="ef01e-278">Monitorování výkonu Azure webové aplikace pomocí Application Insights</span><span class="sxs-lookup"><span data-stu-id="ef01e-278">Monitor Azure web app performance with Application Insights</span></span>](..\application-insights\app-insights-azure-web-apps.md)
 - [<span data-ttu-id="ef01e-279">Sledování dostupnosti a odezvy libovolných webů s Application Insights</span><span class="sxs-lookup"><span data-stu-id="ef01e-279">Monitor availability and responsiveness of any web site with Application Insights</span></span>](..\application-insights\app-insights-monitor-web-app-availability.md)
